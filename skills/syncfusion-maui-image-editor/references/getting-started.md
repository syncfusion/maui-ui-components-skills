# Getting Started with .NET MAUI ImageEditor

This guide walks you through installing, configuring, and implementing the Syncfusion .NET MAUI ImageEditor (SfImageEditor) control in your application.

## Table of Contents
- [Installation](#installation)
- [Handler Registration](#handler-registration)
- [Basic ImageEditor Setup](#basic-imageeditor-setup)
- [Loading Images](#loading-images)
- [Image Information](#image-information)
- [Layout Considerations](#layout-considerations)
- [Customization](#customization)
- [Troubleshooting](#troubleshooting)
- [Next Steps](#next-steps)

## Installation

### Step 1: Install NuGet Package

**Option 1 — .NET CLI:**
```bash
dotnet add package Syncfusion.Maui.ImageEditor
```

**Option 2 — Visual Studio:**
1. Right-click on your project in Solution Explorer
2. Select **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.ImageEditor`
4. Install the latest version

**Option 3 — Visual Studio Code:**
```bash
# From integrated terminal (Ctrl + `)
dotnet add package Syncfusion.Maui.ImageEditor
dotnet restore
```

**Option 4 — Package Manager Console:**
```powershell
Install-Package Syncfusion.Maui.ImageEditor
```

### Step 2: Verify Installation

After installation, verify the package is listed in your `.csproj` file:

```xml
<ItemGroup>
  <PackageReference Include="Syncfusion.Maui.ImageEditor" Version="x.x.x.x" />
  <PackageReference Include="Syncfusion.Maui.Core" Version="x.x.x.x" />
</ItemGroup>
```

> **Note:** `Syncfusion.Maui.Core` is automatically included as a dependency for all Syncfusion .NET MAUI controls.

## Handler Registration

### Register Syncfusion Core Handler

In your `MauiProgram.cs` file, register the Syncfusion Core handler **before** building the application:

```csharp
using Syncfusion.Maui.Core.Hosting;

namespace MyImageEditorApp;

public static class MauiProgram
{
    public static MauiApp CreateMauiApp()
    {
        var builder = MauiApp.CreateBuilder();
        
        builder
            .UseMauiApp<App>()
            .ConfigureSyncfusionCore();  // ← Register Syncfusion handler
        
        builder.ConfigureFonts(fonts =>
        {
            fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
            fonts.AddFont("OpenSans-Semibold.ttf", "OpenSansSemibold");
        });

        return builder.Build();
    }
}
```

> **Important:** If you skip this step, you'll encounter a "Handler not registered" exception at runtime.

## Basic ImageEditor Setup

### XAML Implementation

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:imageEditor="clr-namespace:Syncfusion.Maui.ImageEditor;assembly=Syncfusion.Maui.ImageEditor"
             x:Class="MyApp.MainPage">
    
    <imageEditor:SfImageEditor x:Name="imageEditor" 
                               Source="sample.jpg"
                               ShowToolbar="True" />
</ContentPage>
```

### C# Implementation

```csharp
using Syncfusion.Maui.ImageEditor;

namespace MyApp;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        SfImageEditor imageEditor = new SfImageEditor
        {
            Source = "sample.jpg",
            ShowToolbar = true
        };
        
        this.Content = imageEditor;
    }
}
```

## Loading Images

The `Source` property supports multiple image loading methods:

### 1. Load from File (Local Path)

**Supported formats:** JPEG, JPG, PNG, BMP

```xml
<!-- XAML -->
<imageEditor:SfImageEditor Source="photo.jpg" />
```

```csharp
// C#
imageEditor.Source = ImageSource.FromFile("D:\\Images\\photo.jpg");

// Or use relative path with embedded resources
imageEditor.Source = "photo.jpg";
```

### 2. Load from URI (Remote URL)

```xml
<!-- XAML -->
<imageEditor:SfImageEditor Source="https://example.com/image.png" />
```

```csharp
// C#
imageEditor.Source = ImageSource.FromUri(new Uri("https://example.com/image.png"));
```

> **iOS Note:** For HTTP URLs, configure App Transport Security in `Info.plist`:
> ```xml
> <key>NSAppTransportSecurity</key>
> <dict>
>     <key>NSAllowsArbitraryLoads</key>
>     <true/>
> </dict>
> ```

### 3. Load from Resources Folder

**Add image to project:**
1. Locate the `Resources` folder (typically `Resources/Images/`)
2. Right-click → **Add** → **Existing Item**
3. Select your image file
4. Ensure **Build Action** is set to **MauiImage**

```xml
<!-- XAML -->
<imageEditor:SfImageEditor Source="photo.jpg" />
```

```csharp
// C#
imageEditor.Source = "photo.jpg";

// Or use full resource path
imageEditor.Source = ImageSource.FromResource("MyProject.Resources.Images.photo.jpg");
```

### 4. Load from Stream

```csharp
// From byte array
byte[] imageBytes = File.ReadAllBytes("path/to/image.jpg");
imageEditor.Source = ImageSource.FromStream(() => new MemoryStream(imageBytes));

// From embedded resource
Assembly assembly = Assembly.GetExecutingAssembly();
imageEditor.Source = ImageSource.FromStream(() => 
    assembly.GetManifestResourceStream("MyProject.Resources.Images.photo.jpg")
);
```

> **Important:** Use lambda functions to create a new stream instance each time. This ensures the stream remains accessible for multiple operations.

### 5. Load from File Picker

```csharp
private async void OnPickImageClicked(object sender, EventArgs e)
{
    var result = await FilePicker.PickAsync(new PickOptions
    {
        FileTypes = FilePickerFileType.Images,
        PickerTitle = "Select an image to edit"
    });

    if (result != null)
    {
        var stream = await result.OpenReadAsync();
        imageEditor.Source = ImageSource.FromStream(() => stream);
    }
}
```

### 6. Load from Camera (Media Picker)

```csharp
private async void OnCaptureImageClicked(object sender, EventArgs e)
{
    if (MediaPicker.Default.IsCaptureSupported)
    {
        var photo = await MediaPicker.Default.CapturePhotoAsync();
        if (photo != null)
        {
            var stream = await photo.OpenReadAsync();
            imageEditor.Source = ImageSource.FromStream(() => stream);
        }
    }
}
```

## Image Information

### Get Image Stream

Retrieve the current edited image as a stream:

```csharp
Stream imageStream = imageEditor.GetImageStream();

// Use stream for further processing
using (var fileStream = File.Create("edited_image.jpg"))
{
    imageStream.CopyTo(fileStream);
}
```

### Get Original Image Size

```csharp
Size originalSize = imageEditor.OriginalImageSize;
Console.WriteLine($"Original: {originalSize.Width} x {originalSize.Height}");
```

> **Note:** This value is only available after the image has loaded. Use the `ImageLoaded` event to ensure the image is ready.

### Get Rendered Image Size

The ImageEditor uses `AspectFit` scaling, which may add blank space to maintain aspect ratio. Get the actual rendered size:

```csharp
Size renderedSize = imageEditor.ImageRenderedSize;
Console.WriteLine($"Rendered: {renderedSize.Width} x {renderedSize.Height}");
```

### Check if Image is Edited

```csharp
if (imageEditor.IsImageEdited)
{
    await imageEditor.Save();
}
else
{
    await DisplayAlert("Info", "No changes to save", "OK");
}
```

## Layout Considerations

### Inside Vertical StackLayout

When placing ImageEditor inside a vertical stack, define `MinimumHeightRequest` (default: 100):

```xml
<VerticalStackLayout>
    <imageEditor:SfImageEditor Source="photo.jpg"
                               MinimumHeightRequest="400" />
</VerticalStackLayout>
```

```csharp
var verticalLayout = new VerticalStackLayout();
var imageEditor = new SfImageEditor
{
    Source = "photo.jpg",
    MinimumHeightRequest = 400
};
verticalLayout.Add(imageEditor);
```

### Inside Horizontal StackLayout

Define `MinimumWidthRequest` (default: 100):

```xml
<HorizontalStackLayout>
    <imageEditor:SfImageEditor Source="photo.jpg"
                               MinimumWidthRequest="400" />
</HorizontalStackLayout>
```

```csharp
var horizontalLayout = new HorizontalStackLayout();
var imageEditor = new SfImageEditor
{
    Source = "photo.jpg",
    MinimumWidthRequest = 400
};
horizontalLayout.Add(imageEditor);
```

### Inside Grid

No special requirements, but use appropriate row/column definitions:

```xml
<Grid RowDefinitions="*, Auto">
    <imageEditor:SfImageEditor Grid.Row="0" Source="photo.jpg" />
    <HorizontalStackLayout Grid.Row="1" Spacing="10" Padding="10">
        <Button Text="Save" Clicked="OnSaveClicked" />
        <Button Text="Reset" Clicked="OnResetClicked" />
    </HorizontalStackLayout>
</Grid>
```

## Customization

### Change Background Color

```xml
<!-- XAML -->
<imageEditor:SfImageEditor Source="photo.jpg"
                           Background="LightGray" />
```

```csharp
// C#
imageEditor.Background = Colors.LightGray;
```

### Show/Hide Toolbar

```xml
<!-- XAML -->
<imageEditor:SfImageEditor Source="photo.jpg"
                           ShowToolbar="False" />
```

```csharp
// C#
imageEditor.ShowToolbar = false; // Hide built-in toolbar
```

## Troubleshooting

### Issue: "Handler not registered" Exception

**Solution:** Ensure `ConfigureSyncfusionCore()` is called in `MauiProgram.cs`:
```csharp
builder.ConfigureSyncfusionCore();
```

### Issue: Image Not Loading

**Possible causes:**
1. **Wrong file path** — Verify the image exists at the specified location
2. **Build Action** — Ensure image Build Action is set to `MauiImage` in Resources
3. **File permissions** — Check read permissions for the image file
4. **Unsupported format** — Only JPEG, JPG, PNG, and BMP are supported

**Solution:**
```csharp
// Verify image loaded successfully
imageEditor.ImageLoaded += (s, e) =>
{
    Console.WriteLine("Image loaded successfully");
};
```

### Issue: NuGet Package Conflicts

**Solution:** Ensure all Syncfusion packages have the same version:
```bash
dotnet list package
# Update if versions mismatch
dotnet add package Syncfusion.Maui.ImageEditor --version x.x.x.x
```

### Issue: Image Properties Return Empty/Zero

**Problem:** Accessing `OriginalImageSize` or `ImageRenderedSize` before image loads

**Solution:** Use `ImageLoaded` event:
```csharp
imageEditor.ImageLoaded += (s, e) =>
{
    Size originalSize = imageEditor.OriginalImageSize;
    // Now size is available
};
```

### Issue: Poor Performance on Large Images

**Solution:** Resize images before loading or use lower resolution versions for editing:
```csharp
// Load thumbnail for editing, save at full resolution later
imageEditor.Source = ImageSource.FromFile("thumbnail.jpg");
```

## Next Steps

Now that you have the ImageEditor set up, explore these features:

- **Crop Images:** Learn about cropping modes and aspect ratios in [crop-transform.md](crop-transform.md)
- **Add Annotations:** Add shapes, text, and freehand drawings in [annotations.md](annotations.md)
- **Apply Filters:** Enhance images with filters and effects in [filters-effects.md](filters-effects.md)
- **Customize Toolbar:** Build custom editing interfaces in [toolbar.md](toolbar.md)
- **Save Images:** Export edited images in [save-serialization.md](save-serialization.md)
- **Handle Events:** Respond to user actions in [events.md](events.md)