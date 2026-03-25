# Getting Started with Syncfusion .NET MAUI Parallax View

This guide walks you through installing, configuring, and implementing the basic Syncfusion .NET MAUI Parallax View (SfParallaxView) control in your application.

## Installation

### Step 1: Install the NuGet Package

1. In **Solution Explorer**, right-click your project and choose **Manage NuGet Packages**
2. Search for `Syncfusion.Maui.ParallaxView`
3. Install the latest version of the package
4. Ensure all dependencies are installed correctly and the project is restored

**Package Name:** [Syncfusion.Maui.ParallaxView](https://www.nuget.org/packages/Syncfusion.Maui.ParallaxView/)

**Via Package Manager Console:**
```bash
Install-Package Syncfusion.Maui.ParallaxView
```

**Via .NET CLI:**
```bash
dotnet add package Syncfusion.Maui.ParallaxView
```
### Step 2: Register the Syncfusion Handler

**CRITICAL:** The `Syncfusion.Maui.Core` NuGet is a dependent package for all Syncfusion controls. You MUST register the handler in `MauiProgram.cs`.

**File:** `MauiProgram.cs`

```csharp
using Syncfusion.Maui.Core.Hosting;

    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            
            // Register Syncfusion Core - REQUIRED!
            builder.ConfigureSyncfusionCore();
            
            builder
                .UseMauiApp<App>()
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                    fonts.AddFont("OpenSans-Semibold.ttf", "OpenSansSemibold");
                });

            return builder.Build();
        }
    }
```

**⚠️ Important:** Call `ConfigureSyncfusionCore()` before `Build()`. Failure to register the handler will result in runtime errors.

### Step 3: Register Namespace

After installing the package, import the namespace in your XAML or C# files.

**XAML:**
```xaml
xmlns:parallax="clr-namespace:Syncfusion.Maui.ParallaxView;assembly=Syncfusion.Maui.ParallaxView"
```

**C#:**
```csharp
using Syncfusion.Maui.ParallaxView;
```

## Basic SfParallaxView Initialization

The SfParallaxView control requires two main components:
1. **Content** - The background view that moves with the parallax effect
2. **Source** - The foreground scrollable view that drives the parallax motion

### XAML Initialization

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:parallax="clr-namespace:Syncfusion.Maui.ParallaxView;assembly=Syncfusion.Maui.ParallaxView"
    x:Class="ParallaxViewDemo.MainPage">
    
    <Grid>
        <parallax:SfParallaxView x:Name="parallaxView" />
    </Grid>
</ContentPage>
```

### C# Initialization

```csharp
using Syncfusion.Maui.ParallaxView;

namespace ParallaxViewDemo
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            Grid grid = new Grid();
            SfParallaxView parallaxView = new SfParallaxView();
            grid.Children.Add(parallaxView);
            
            this.Content = grid;
        }
    }
}
```

## Adding Content (Background View)

The `Content` property represents the background view of the parallax view. You can set any kind of view to the Content property, such as Image, StackLayout, Grid, or any custom view.

### Setting Image as Content

**XAML:**
```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:parallax="clr-namespace:Syncfusion.Maui.ParallaxView;assembly=Syncfusion.Maui.ParallaxView"
    x:Class="ParallaxViewDemo.MainPage">
    
    <Grid>
        <parallax:SfParallaxView x:Name="parallaxView">
            <parallax:SfParallaxView.Content>
                <Image Source="{Binding BackgroundImage}" 
                       BackgroundColor="Transparent"
                       HorizontalOptions="Fill" 
                       VerticalOptions="Fill" 
                       Aspect="AspectFill" />
            </parallax:SfParallaxView.Content>
        </parallax:SfParallaxView>
    </Grid>
</ContentPage>
```

**C#:**
```csharp
using Syncfusion.Maui.ParallaxView;

namespace ParallaxViewDemo
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            BindingContext = new ParallaxViewModel();
        }
    }
    
    public class ParallaxViewModel
    {
        public ImageSource BackgroundImage { get; set; }
        
        public ParallaxViewModel()
        {
            // Load from resource
            BackgroundImage = ImageSource.FromResource(
                "ParallaxViewDemo.Resources.Images.parallax_bg.jpg", 
                typeof(MainPage).GetTypeInfo().Assembly);
                
            // Or load from file
            // BackgroundImage = ImageSource.FromFile("parallax_bg.jpg");
        }
    }
}
```

### Setting StackLayout as Content

You can use any view as the Content, not just images:

**XAML:**
```xaml
<parallax:SfParallaxView x:Name="parallaxView">
    <parallax:SfParallaxView.Content>
        <StackLayout BackgroundColor="#4A90E2" Padding="20">
            <Label Text="Parallax Background" 
                   FontSize="32" 
                   TextColor="White" 
                   HorizontalOptions="Center" 
                   VerticalOptions="Center" />
        </StackLayout>
    </parallax:SfParallaxView.Content>
</parallax:SfParallaxView>
```

**C#:**
```csharp
var parallaxView = new SfParallaxView
{
    Content = new StackLayout
    {
        BackgroundColor = Color.FromArgb("#4A90E2"),
        Padding = new Thickness(20),
        Children =
        {
            new Label
            {
                Text = "Parallax Background",
                FontSize = 32,
                TextColor = Colors.White,
                HorizontalOptions = LayoutOptions.Center,
                VerticalOptions = LayoutOptions.Center
            }
        }
    }
};
```

## Supported Content Types

The Content property accepts any .NET MAUI View, including:

- **Image** - Most common for parallax backgrounds
- **StackLayout** - For layered or gradient backgrounds
- **Grid** - For complex background layouts
- **AbsoluteLayout** - For positioned background elements
- **Frame** - For bordered backgrounds with shadows
- **Border** - For rounded corner backgrounds
- **Custom Views** - Any custom control inheriting from View

### Multiple Elements in Content

Use a container layout to add multiple elements:

```xaml
<parallax:SfParallaxView.Content>
    <Grid>
        <Image Source="background.jpg" Aspect="AspectFill" />
        <BoxView Color="#80000000" /> <!-- Semi-transparent overlay -->
        <Label Text="Hero Title" 
               FontSize="48" 
               TextColor="White" 
               HorizontalOptions="Center" 
               VerticalOptions="Center" />
    </Grid>
</parallax:SfParallaxView.Content>
```

## Important Notes

### Content Size Behavior

The size of the Content view will **automatically be stretched** to match the size of the Source view. This ensures that:
- The background covers the entire scrollable area
- No gaps appear during scrolling
- The parallax effect works smoothly across all scroll positions

You don't need to manually set the size of the Content - it's handled automatically by the control.

### Transparent Foreground

For the parallax effect to be visible, ensure your Source control (ScrollView, ListView, etc.) has a **transparent background**:

```xaml
<ScrollView x:Name="scrollView" BackgroundColor="Transparent">
    <!-- Content here -->
</ScrollView>
```

Without a transparent background, the Source will obscure the Content, and no parallax effect will be visible.

## Video Tutorial

For a quick visual guide, watch the official Syncfusion video tutorial:
[Getting Started with .NET MAUI Parallax View](https://youtu.be/ezzIDWYYrUc)

## Next Steps

Now that you have the basic setup:
- **[Source Binding](source-binding.md)** - Learn how to bind scrollable controls to create the parallax effect
- **[Customization](customization.md)** - Customize speed and orientation for different effects
- **[Custom Controls](custom-controls.md)** - Implement IParallaxView for custom scrollable controls

## Common Issues

### Issue: Parallax effect not visible
**Solution:** Ensure the Source control has `BackgroundColor="Transparent"` so the Content is visible through it.

### Issue: Content doesn't fill the entire area
**Solution:** The Content automatically stretches to match the Source size. If this isn't working, ensure your Source control has a defined size.

### Issue: NuGet package not found
**Solution:** Ensure you have the Syncfusion NuGet feed configured or are using the public NuGet.org feed where the package is available.
