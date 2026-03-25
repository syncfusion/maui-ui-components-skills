# Getting Started with .NET MAUI SignaturePad

This guide covers the complete setup and basic implementation of the Syncfusion .NET MAUI SignaturePad control, including installation, configuration, and initial usage across multiple development environments.

## Installation and Setup

### Step 1: Create a .NET MAUI Project

#### Visual Studio

1. Open Visual Studio 2026
2. Go to **File > New > Project**
3. Select **.NET MAUI App** template
4. Name your project (e.g., "SignaturePadDemo")
5. Choose a location and click **Next**
6. Select the .NET framework version (.NET 9 or later)
7. Click **Create**

#### Visual Studio Code

1. Open the Command Palette (`Ctrl+Shift+P` on Windows/Linux, `Cmd+Shift+P` on Mac)
2. Type **.NET: New Project** and press Enter
3. Choose **.NET MAUI App** template
4. Select the project location
5. Enter the project name and press Enter
6. Choose **Create project**

#### JetBrains Rider

1. Open JetBrains Rider
2. Go to **File > New Solution**
3. Select **.NET (C#)** and choose **.NET MAUI App** template
4. Enter the Project Name and Solution Name
5. Choose the Location
6. Select the .NET framework version
7. Click **Create**

### Step 2: Install Syncfusion.Maui.SignaturePad NuGet Package

#### Visual Studio

1. In **Solution Explorer**, right-click the project
2. Choose **Manage NuGet Packages**
3. Select the **Browse** tab
4. Search for `Syncfusion.Maui.SignaturePad`
5. Select the package and click **Install**
6. Accept the license agreement
7. Ensure all dependencies are installed correctly

#### Visual Studio Code

1. Press `Ctrl+\`` (backtick) to open the integrated terminal
2. Navigate to your project root directory (where the `.csproj` file is located)
3. Run the following command:

```bash
dotnet add package Syncfusion.Maui.SignaturePad
```

4. Restore dependencies:

```bash
dotnet restore
```

#### JetBrains Rider

1. In **Solution Explorer**, right-click the project
2. Choose **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.SignaturePad`
4. Select the package and click **Install**
5. If needed, manually restore by opening Terminal and running:

```bash
dotnet restore
```

### Step 3: Register Syncfusion Handler

The `Syncfusion.Maui.Core` package is a dependency for all Syncfusion .NET MAUI controls. You must register the Syncfusion handler in your `MauiProgram.cs` file.

Open `MauiProgram.cs` and add the handler registration:

```csharp
using Syncfusion.Maui.Core.Hosting;

namespace SignaturePadDemo
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .ConfigureSyncfusionCore()  // Register Syncfusion handlers
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                    fonts.AddFont("OpenSans-Semibold.ttf", "OpenSansSemibold");
                });

            return builder.Build();
        }
    }
}
```

**Key Points:**
- Add `using Syncfusion.Maui.Core.Hosting;` at the top
- Call `.ConfigureSyncfusionCore()` in the builder chain
- This registration is required for all Syncfusion MAUI controls

## Basic Implementation

### XAML Implementation

#### Step 1: Add Namespace Declaration

Open your XAML page (e.g., `MainPage.xaml`) and add the SignaturePad namespace:

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:signaturePad="clr-namespace:Syncfusion.Maui.SignaturePad;assembly=Syncfusion.Maui.SignaturePad"
             x:Class="SignaturePadDemo.MainPage">
    
    <!-- Content here -->
    
</ContentPage>
```

#### Step 2: Add SignaturePad Control

Add the `SfSignaturePad` control to your layout:

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:signaturePad="clr-namespace:Syncfusion.Maui.SignaturePad;assembly=Syncfusion.Maui.SignaturePad"
             x:Class="SignaturePadDemo.MainPage">
    
    <Grid Padding="20">
        <signaturePad:SfSignaturePad x:Name="signaturePad"
                                      HeightRequest="300"
                                      VerticalOptions="Center" />
    </Grid>
    
</ContentPage>
```

#### Complete XAML Example with Buttons

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:signaturePad="clr-namespace:Syncfusion.Maui.SignaturePad;assembly=Syncfusion.Maui.SignaturePad"
             x:Class="SignaturePadDemo.MainPage">
    
    <Grid Padding="20">
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
        
        <!-- SignaturePad -->
        <Border Grid.Row="0"
                Stroke="Gray"
                StrokeThickness="2"
                Padding="5">
            <signaturePad:SfSignaturePad x:Name="signaturePad" />
        </Border>
        
        <!-- Action Buttons -->
        <HorizontalStackLayout Grid.Row="1"
                               Spacing="10"
                               Padding="10"
                               HorizontalOptions="Center">
            <Button Text="Save"
                    Clicked="OnSaveButtonClicked"
                    WidthRequest="100" />
            <Button Text="Clear"
                    Clicked="OnClearButtonClicked"
                    WidthRequest="100" />
        </HorizontalStackLayout>
    </Grid>
    
</ContentPage>
```

#### Code-Behind for XAML

```csharp
using Syncfusion.Maui.SignaturePad;

namespace SignaturePadDemo
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
        }

        private void OnSaveButtonClicked(object sender, EventArgs e)
        {
            ImageSource? signature = signaturePad.ToImageSource();
            if (signature != null)
            {
                DisplayAlert("Success", "Signature saved successfully!", "OK");
                // Additional save logic here
            }
            else
            {
                DisplayAlert("Error", "No signature to save", "OK");
            }
        }

        private void OnClearButtonClicked(object sender, EventArgs e)
        {
            signaturePad.Clear();
        }
    }
}
```

### C# Implementation (Code-Only)

For a code-only approach without XAML:

```csharp
using Syncfusion.Maui.SignaturePad;

namespace SignaturePadDemo
{
    public class MainPage : ContentPage
    {
        private SfSignaturePad signaturePad;

        public MainPage()
        {
            // Create SignaturePad
            signaturePad = new SfSignaturePad
            {
                HeightRequest = 300,
                VerticalOptions = LayoutOptions.Center
            };

            // Create buttons
            var saveButton = new Button
            {
                Text = "Save",
                WidthRequest = 100
            };
            saveButton.Clicked += OnSaveButtonClicked;

            var clearButton = new Button
            {
                Text = "Clear",
                WidthRequest = 100
            };
            clearButton.Clicked += OnClearButtonClicked;

            // Create layout
            var buttonLayout = new HorizontalStackLayout
            {
                Spacing = 10,
                Padding = 10,
                HorizontalOptions = LayoutOptions.Center,
                Children = { saveButton, clearButton }
            };

            var border = new Border
            {
                Stroke = Colors.Gray,
                StrokeThickness = 2,
                Padding = 5,
                Content = signaturePad
            };

            var grid = new Grid
            {
                Padding = 20,
                RowDefinitions =
                {
                    new RowDefinition { Height = new GridLength(1, GridUnitType.Star) },
                    new RowDefinition { Height = GridLength.Auto }
                },
                Children = { border, buttonLayout }
            };

            Grid.SetRow(border, 0);
            Grid.SetRow(buttonLayout, 1);

            Content = grid;
        }

        private void OnSaveButtonClicked(object sender, EventArgs e)
        {
            ImageSource? signature = signaturePad.ToImageSource();
            if (signature != null)
            {
                DisplayAlert("Success", "Signature saved successfully!", "OK");
                // Additional save logic here
            }
            else
            {
                DisplayAlert("Error", "No signature to save", "OK");
            }
        }

        private void OnClearButtonClicked(object sender, EventArgs e)
        {
            signaturePad.Clear();
        }
    }
}
```

## Default Behavior

When you first add SignaturePad without additional configuration:

- **Stroke Color:** Black (`Colors.Black`)
- **Stroke Thickness:** Dynamic (varies from minimum to maximum based on speed)
- **Default Minimum Thickness:** 1
- **Default Maximum Thickness:** 3
- **Background:** White
- **Input Support:** Touch, pen, mouse (platform-dependent)
- **Rendering:** Smooth, anti-aliased strokes with realistic handwriting appearance

## Verifying the Setup

Run your application to verify SignaturePad is working:

1. Build and run the application (`F5` in Visual Studio/Rider, or `dotnet run` in terminal)
2. You should see the SignaturePad area
3. Try drawing a signature with mouse/touch
4. Click "Save" to test the save functionality
5. Click "Clear" to reset the signature

### Troubleshooting Common Issues

**Issue: SignaturePad not appearing**
- Verify `ConfigureSyncfusionCore()` is called in `MauiProgram.cs`
- Check that the NuGet package is properly installed
- Ensure the namespace declaration is correct in XAML

**Issue: Build errors about missing assemblies**
- Clean and rebuild the solution
- Restore NuGet packages: `dotnet restore`
- Check that `Syncfusion.Maui.Core` is installed as a dependency

**Issue: SignaturePad not responding to input**
- Ensure the control has proper `HeightRequest` and `WidthRequest`
- Check that the control is not covered by other elements
- Verify platform-specific permissions if on mobile devices

## Next Steps

Now that you have a basic SignaturePad implementation:

- **Customize Appearance:** See [stroke-customization.md](stroke-customization.md) for color and thickness options
- **Implement Saving:** See [saving-signatures.md](saving-signatures.md) for image export and point data retrieval
- **Add Event Handling:** See [events-and-methods.md](events-and-methods.md) for validation and workflows
- **Apply Effects:** See [liquid-glass-effect.md](liquid-glass-effect.md) for modern UI styling

## Additional Notes

### NuGet Package Information
- **Package Name:** `Syncfusion.Maui.SignaturePad`
- **Dependency:** `Syncfusion.Maui.Core` (automatically installed)
- **Namespace:** `Syncfusion.Maui.SignaturePad`
- **Class Name:** `SfSignaturePad`

### Platform Support
- **iOS:** iOS 11 and later
- **Android:** API 21 (Android 5.0) and later
- **macOS:** macOS 10.15 and later
- **Windows:** Windows 10 version 1809 and later

### License Requirements
Syncfusion components require a valid license for production use. You can:
- Use the 30-day free trial
- Register for a community license (for qualifying projects)
- Purchase a commercial license

Register your license key in `MauiProgram.cs` before `CreateMauiApp()`:

```csharp
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
```
