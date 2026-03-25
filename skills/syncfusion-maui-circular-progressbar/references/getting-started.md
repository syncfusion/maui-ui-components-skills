# Getting Started with Circular ProgressBar

This guide covers installation, setup, and basic implementation of the Syncfusion .NET MAUI Circular ProgressBar (SfCircularProgressBar) control.

## Table of Contents
- [Installation](#installation)
- [Register Handler](#register-handler)
- [Basic Implementation](#basic-implementation)
- [Setting Progress Value](#setting-progress-value)
- [First Complete Example](#first-complete-example)
- [Troubleshooting](#troubleshooting)

## Installation

### Step 1: Create a New .NET MAUI Project

**Visual Studio:**
1. Go to **File > New > Project**
2. Choose the **.NET MAUI App** template
3. Name the project and choose a location
4. Click **Next**, select the .NET framework version, and click **Create**

**Visual Studio Code:**
1. Open command palette: `Ctrl+Shift+P`
2. Type **.NET:New Project** and press Enter
3. Choose **.NET MAUI App** template
4. Select project location, type project name, and press **Enter**
5. Choose **Create project**

### Step 2: Install NuGet Package

**Option 1: NuGet Package Manager (Visual Studio/Rider)**
1. Right-click the project in Solution Explorer
2. Choose **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.ProgressBar`
4. Install the latest version
5. Ensure dependencies are installed correctly

**Option 2: Command Line**
```bash
dotnet add package Syncfusion.Maui.ProgressBar
```

**Option 3: Package Reference (Manual)**

Add to your `.csproj` file:
```xml
<ItemGroup>
    <PackageReference Include="Syncfusion.Maui.ProgressBar" Version="*" />
</ItemGroup>
```

Then run:
```bash
dotnet restore
```

## Register Handler

The `Syncfusion.Maui.Core` NuGet is a dependent package for all Syncfusion controls of .NET MAUI. You must register the handler for Syncfusion core in `MauiProgram.cs`.

### MauiProgram.cs Configuration

```csharp
using Syncfusion.Maui.Core.Hosting;

namespace GettingStarted
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();

            builder.ConfigureSyncfusionCore(); // Register Syncfusion handler
            
            builder
                .UseMauiApp<App>()
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                });

            return builder.Build();
        }
    }
}
```

**Important:** The `ConfigureSyncfusionCore()` call is mandatory for all Syncfusion MAUI controls.

## Basic Implementation

### Step 1: Import Namespace

**XAML:**
```xml
xmlns:progressBar="clr-namespace:Syncfusion.Maui.ProgressBar;assembly=Syncfusion.Maui.ProgressBar"
```

**C#:**
```csharp
using Syncfusion.Maui.ProgressBar;
```

### Step 2: Add Control to Page

**XAML Implementation:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:progressBar="clr-namespace:Syncfusion.Maui.ProgressBar;assembly=Syncfusion.Maui.ProgressBar"
             x:Class="MyApp.MainPage">
    
    <StackLayout Padding="20" VerticalOptions="Center">
        <progressBar:SfCircularProgressBar Progress="75" />
    </StackLayout>
    
</ContentPage>
```

**C# Implementation:**
```csharp
using Syncfusion.Maui.ProgressBar;

namespace MyApp
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            SfCircularProgressBar circularProgressBar = new SfCircularProgressBar 
            { 
                Progress = 75 
            };
            
            Content = circularProgressBar;
        }
    }
}
```

## Setting Progress Value

By default, the progress value should be specified between 0 and 100.

### Default Range (0-100)

**XAML:**
```xml
<progressBar:SfCircularProgressBar Progress="75" />
```

**C#:**
```csharp
SfCircularProgressBar circularProgressBar = new SfCircularProgressBar();
circularProgressBar.Progress = 75; // 75%
```

### Custom Range (0-1)

To determine progress value between 0 and 1, set the Minimum property to 0 and the Maximum property to 1.

**XAML:**
```xml
<progressBar:SfCircularProgressBar Minimum="0" 
                                   Maximum="1" 
                                   Progress="0.75" />
```

**C#:**
```csharp
SfCircularProgressBar circularProgressBar = new SfCircularProgressBar
{
    Minimum = 0,
    Maximum = 1,
    Progress = 0.75 // 75%
};
```

## First Complete Example

### Complete XAML Page

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:progressBar="clr-namespace:Syncfusion.Maui.ProgressBar;assembly=Syncfusion.Maui.ProgressBar"
             x:Class="MyApp.MainPage"
             BackgroundColor="{DynamicResource PageBackgroundColor}">

    <StackLayout Padding="20" Spacing="30" VerticalOptions="Center">
        
        <!-- Basic Progress Bar -->
        <Label Text="Basic Progress (75%)" 
               FontSize="16" 
               FontAttributes="Bold" />
        <progressBar:SfCircularProgressBar Progress="75" />
        
        <!-- Colored Progress Bar -->
        <Label Text="Colored Progress (60%)" 
               FontSize="16" 
               FontAttributes="Bold" />
        <progressBar:SfCircularProgressBar Progress="60"
                                           TrackFill="#33c15244" 
                                           ProgressFill="#FFc15244" />
        
        <!-- Indeterminate State -->
        <Label Text="Loading (Indeterminate)" 
               FontSize="16" 
               FontAttributes="Bold" />
        <progressBar:SfCircularProgressBar IsIndeterminate="True" />
        
    </StackLayout>

</ContentPage>
```

### Complete C# Page

```csharp
using Syncfusion.Maui.ProgressBar;
using Microsoft.Maui.Controls;

namespace MyApp
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            var stackLayout = new StackLayout
            {
                Padding = new Thickness(20),
                Spacing = 30,
                VerticalOptions = LayoutOptions.Center
            };
            
            // Basic Progress Bar
            stackLayout.Children.Add(new Label 
            { 
                Text = "Basic Progress (75%)", 
                FontSize = 16, 
                FontAttributes = FontAttributes.Bold 
            });
            
            var basicProgressBar = new SfCircularProgressBar 
            { 
                Progress = 75 
            };
            stackLayout.Children.Add(basicProgressBar);
            
            // Colored Progress Bar
            stackLayout.Children.Add(new Label 
            { 
                Text = "Colored Progress (60%)", 
                FontSize = 16, 
                FontAttributes = FontAttributes.Bold 
            });
            
            var coloredProgressBar = new SfCircularProgressBar
            {
                Progress = 60,
                TrackFill = new SolidColorBrush(Color.FromArgb("#33c15244")),
                ProgressFill = new SolidColorBrush(Color.FromArgb("#FFc15244"))
            };
            stackLayout.Children.Add(coloredProgressBar);
            
            // Indeterminate State
            stackLayout.Children.Add(new Label 
            { 
                Text = "Loading (Indeterminate)", 
                FontSize = 16, 
                FontAttributes = FontAttributes.Bold 
            });
            
            var indeterminateProgressBar = new SfCircularProgressBar 
            { 
                IsIndeterminate = true 
            };
            stackLayout.Children.Add(indeterminateProgressBar);
            
            Content = stackLayout;
        }
    }
}
```

## Troubleshooting

### Handler Not Registered Error

**Error:** "Handler not found for view Syncfusion.Maui.ProgressBar.SfCircularProgressBar"

**Solution:** Ensure `builder.ConfigureSyncfusionCore()` is called in `MauiProgram.cs`:
```csharp
builder.ConfigureSyncfusionCore();
```

### NuGet Package Not Found

**Error:** Package 'Syncfusion.Maui.ProgressBar' not found

**Solution:**
1. Check your internet connection
2. Clear NuGet cache: `dotnet nuget locals all --clear`
3. Restore packages: `dotnet restore`
4. Ensure NuGet source includes nuget.org

### Progress Not Visible

**Issue:** Progress bar shows but no progress is visible

**Solution:**
1. Check that `Progress` value is set between `Minimum` and `Maximum`
2. Verify `ProgressFill` color contrasts with `TrackFill`
3. Ensure the control has sufficient size (default requires ~200x200 minimum)

### Control Not Rendering

**Issue:** Control doesn't appear on page

**Solution:**
1. Verify namespace import is correct
2. Check that handler is registered in `MauiProgram.cs`
3. Ensure parent container has sufficient space
4. Try setting explicit `WidthRequest` and `HeightRequest` (e.g., 200)

### Build Errors After Package Installation

**Issue:** Build fails after installing Syncfusion package

**Solution:**
1. Clean solution: `dotnet clean`
2. Rebuild: `dotnet build`
3. If errors persist, delete `bin/` and `obj/` folders manually
4. Restore packages: `dotnet restore`
5. Rebuild the project

## Next Steps

Now that you have a basic circular progress bar running:
- Explore **states.md** for determinate and indeterminate modes
- Review **appearance.md** for customizing colors, angles, and thickness
- Check **animation.md** for smooth progress transitions
- Learn about **segments.md** for multi-step progress visualization