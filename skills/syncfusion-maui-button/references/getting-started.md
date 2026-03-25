# Getting Started with .NET MAUI Button

This guide covers the complete setup process for implementing the Syncfusion .NET MAUI Button (SfButton) control in your application, including installation, configuration, and basic usage examples.

## Installation

### Step 1: Create or Open a .NET MAUI Project

**For new projects:**

**Visual Studio:**
1. Go to **File > New > Project**
2. Search for "**.NET MAUI App**" template
3. Name your project and select location
4. Choose **.NET 9.0** or later framework
5. Click **Create**

**Visual Studio Code / CLI:**
```bash
dotnet new maui -n MyMauiApp
cd MyMauiApp
```

### Step 2: Install the NuGet Package

**Visual Studio:**
1. Right-click project in **Solution Explorer**
2. Select **Manage NuGet Packages**
3. Search for **Syncfusion.Maui.Buttons**
4. Click **Install** for the latest stable version

**Visual Studio Code / CLI:**
```bash
dotnet add package Syncfusion.Maui.Buttons
```

**Verify installation** in your `.csproj` file:
```xml
<ItemGroup>
  <PackageReference Include="Syncfusion.Maui.Buttons" Version="*" />
</ItemGroup>
```

> **Note:** The `Syncfusion.Maui.Core` package will be installed automatically as a dependency.

### Step 3: Register the Handler

Open **MauiProgram.cs** and register the Syncfusion Core handler:

```csharp
using Microsoft.Extensions.Logging;
using Syncfusion.Maui.Core.Hosting;

namespace MyMauiApp;

public static class MauiProgram
{
    public static MauiApp CreateMauiApp()
    {
        var builder = MauiApp.CreateBuilder();
        builder
            .UseMauiApp<App>()
            .ConfigureSyncfusionCore()  // ← Add this line
            .ConfigureFonts(fonts =>
            {
                fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                fonts.AddFont("OpenSans-Semibold.ttf", "OpenSansSemibold");
            });

#if DEBUG
        builder.Logging.AddDebug();
#endif

        return builder.Build();
    }
}
```

**Important:** Always call `ConfigureSyncfusionCore()` before building the app. This registers handlers for all Syncfusion MAUI controls.

## Basic Button Implementation

### XAML Implementation

**Step 1:** Add the Syncfusion namespace to your XAML page:

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:buttons="clr-namespace:Syncfusion.Maui.Buttons;assembly=Syncfusion.Maui.Buttons"
             x:Class="MyMauiApp.MainPage">
    <!-- Content here -->
</ContentPage>
```

**Step 2:** Add a basic SfButton:

```xml
<buttons:SfButton x:Name="myButton"
                  Text="Click Me" />
```

**Complete example:**

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:buttons="clr-namespace:Syncfusion.Maui.Buttons;assembly=Syncfusion.Maui.Buttons"
             x:Class="MyMauiApp.MainPage">

    <VerticalStackLayout Spacing="20"
                         Padding="30"
                         VerticalOptions="Center">
        
        <buttons:SfButton x:Name="myButton"
                          Text="Click Me"
                          TextColor="White"
                          Background="#6200EE"
                          CornerRadius="8"
                          WidthRequest="200"
                          HeightRequest="44"
                          Clicked="OnButtonClicked" />
                          
    </VerticalStackLayout>

</ContentPage>
```

**Code-behind (MainPage.xaml.cs):**

```csharp
namespace MyMauiApp;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }

    private void OnButtonClicked(object sender, EventArgs e)
    {
        DisplayAlert("Button Clicked", "You clicked the button!", "OK");
    }
}
```

### C# Implementation

You can also create buttons entirely in C#:

```csharp
using Syncfusion.Maui.Buttons;

namespace MyMauiApp;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        var button = new SfButton
        {
            Text = "Click Me",
            TextColor = Colors.White,
            Background = Color.FromArgb("#6200EE"),
            CornerRadius = 8,
            WidthRequest = 200,
            HeightRequest = 44
        };
        
        button.Clicked += OnButtonClicked;
        
        Content = new VerticalStackLayout
        {
            Spacing = 20,
            Padding = new Thickness(30),
            VerticalOptions = LayoutOptions.Center,
            Children = { button }
        };
    }

    private void OnButtonClicked(object sender, EventArgs e)
    {
        DisplayAlert("Button Clicked", "You clicked the button!", "OK");
    }
}
```

## Adding Button Icons

### Basic Icon Setup

Use the `ImageSource` and `ShowIcon` properties to add icons:

**XAML:**
```xml
<buttons:SfButton Text="Save"
                  TextColor="White"
                  Background="#4CAF50"
                  ShowIcon="True"
                  ImageSource="save_icon.png"
                  WidthRequest="120"
                  HeightRequest="40" />
```

**C#:**
```csharp
var saveButton = new SfButton
{
    Text = "Save",
    TextColor = Colors.White,
    Background = Color.FromArgb("#4CAF50"),
    ShowIcon = true,
    ImageSource = "save_icon.png",
    WidthRequest = 120,
    HeightRequest = 40
};
```

### Icon Image Placement

Icons must be placed in the **Resources/Images** folder of your project:

```
MyMauiApp/
├── Resources/
│   ├── Images/
│   │   ├── save_icon.png
│   │   ├── delete_icon.png
│   │   └── edit_icon.png
```

**Using platform-specific images:**
```xml
<buttons:SfButton ShowIcon="True"
                  ImageSource="save_icon.png" />  <!-- Automatically resolves -->
```

**Using FontImageSource (icon fonts):**
```xml
<buttons:SfButton ShowIcon="True">
    <buttons:SfButton.ImageSource>
        <FontImageSource Glyph="&#xE74E;"
                         FontFamily="MaterialIcons"
                         Size="24"
                         Color="White" />
    </buttons:SfButton.ImageSource>
</buttons:SfButton>
```

### Icon-Only Buttons

Create icon-only buttons by omitting the `Text` property:

```xml
<buttons:SfButton ShowIcon="True"
                  ImageSource="add_icon.png"
                  Background="#6200EE"
                  CornerRadius="28"
                  WidthRequest="56"
                  HeightRequest="56"
                  Padding="0" />
```

## Adding Background Images

Use `BackgroundImageSource` to set a background image for the button:

**XAML:**
```xml
<buttons:SfButton Text="Nature"
                  TextColor="White"
                  FontAttributes="Bold"
                  FontSize="18"
                  BackgroundImageSource="scenic_background.png"
                  CornerRadius="12"
                  WidthRequest="200"
                  HeightRequest="100" />
```

**C#:**
```csharp
var imageButton = new SfButton
{
    Text = "Nature",
    TextColor = Colors.White,
    FontAttributes = FontAttributes.Bold,
    FontSize = 18,
    BackgroundImageSource = "scenic_background.png",
    CornerRadius = 12,
    WidthRequest = 200,
    HeightRequest = 100
};
```

**Image Requirements:**
- Place images in **Resources/Images/** folder
- Supported formats: PNG, JPG, SVG
- Image will be stretched to fill the button area
- Text will overlay the background image

## Minimal Working Examples

### Example 1: Simple Text Button

```xml
<buttons:SfButton Text="Submit" />
```

### Example 2: Styled Button

```xml
<buttons:SfButton Text="Primary Action"
                  TextColor="White"
                  Background="#6200EE"
                  CornerRadius="8"
                  WidthRequest="180"
                  HeightRequest="44" />
```

### Example 3: Button with Icon and Text

```xml
<buttons:SfButton Text="Delete"
                  TextColor="White"
                  Background="#F44336"
                  ShowIcon="True"
                  ImageSource="delete_icon.png"
                  CornerRadius="8" />
```

### Example 4: Complete Button with Event

```xml
<!-- XAML -->
<buttons:SfButton x:Name="submitButton"
                  Text="Submit Form"
                  TextColor="White"
                  Background="#6200EE"
                  CornerRadius="8"
                  WidthRequest="200"
                  HeightRequest="44"
                  Clicked="OnSubmitClicked" />
```

```csharp
// Code-behind
private async void OnSubmitClicked(object sender, EventArgs e)
{
    submitButton.IsEnabled = false;
    submitButton.Text = "Submitting...";
    
    await Task.Delay(2000); // Simulate async operation
    
    await DisplayAlert("Success", "Form submitted successfully!", "OK");
    
    submitButton.Text = "Submit Form";
    submitButton.IsEnabled = true;
}
```

## Next Steps

Now that you have a basic button working:

- **Customize appearance:** See [customization.md](customization.md) for styling options
- **Add visual states:** See [visual-states.md](visual-states.md) for hover/press effects
- **Explore advanced features:** See [advanced-features.md](advanced-features.md) for RTL, Liquid Glass, and custom content
- **Troubleshooting:** See [troubleshooting.md](troubleshooting.md) for common issues

## Quick Reference

| Task | Property/Method |
|------|----------------|
| Set button text | `Text="..."` |
| Set text color | `TextColor="..."` |
| Set background color | `Background="..."` |
| Add icon | `ShowIcon="True"` + `ImageSource="..."` |
| Set background image | `BackgroundImageSource="..."` |
| Round corners | `CornerRadius="8"` |
| Handle click | `Clicked="OnButtonClicked"` |
| Set size | `WidthRequest="..."` + `HeightRequest="..."` |
