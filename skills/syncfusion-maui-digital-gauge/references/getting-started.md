# Getting Started with .NET MAUI DigitalGauge

This guide covers the setup and basic implementation of the Syncfusion .NET MAUI DigitalGauge control. Follow these steps to add digital LED-style displays to your .NET MAUI application.

## Step 1: Create a New MAUI Project

### Using Visual Studio

1. Launch Visual Studio
2. Select **File > New > Project**
3. Search for and select **.NET MAUI App** template
4. Click **Next**
5. Enter project details:
   - **Project name**: e.g., "DigitalGaugeApp"
   - **Location**: Choose your workspace directory
6. Click **Next**
7. Select **.NET framework version** (.NET 9 or later)
8. Click **Create**

### Using .NET CLI

```bash
# Create new MAUI project
dotnet new maui -n DigitalGaugeApp

# Navigate to project directory
cd DigitalGaugeApp

# Restore dependencies
dotnet restore
```

## Step 2: Install Syncfusion MAUI Gauges NuGet Package

The DigitalGauge control is part of the Syncfusion.Maui.Gauges package.

### Using Visual Studio

1. In **Solution Explorer**, right-click your project
2. Select **Manage NuGet Packages**
3. Click the **Browse** tab
4. Search for **Syncfusion.Maui.Gauges**
5. Select the package and click **Install**
6. Accept the license agreement
7. Wait for installation to complete

### Using Package Manager Console

```bash
Install-Package Syncfusion.Maui.Gauges
```

### Using .NET CLI

```bash
dotnet add package Syncfusion.Maui.Gauges
```

### Verify Installation

Check your project file (.csproj) contains the package reference:

```xml
<ItemGroup>
  <PackageReference Include="Syncfusion.Maui.Gauges" Version="*" />
</ItemGroup>
```

## Step 3: Register the Syncfusion Handler

The Syncfusion.Maui.Core package is a dependency that provides core functionality. You must register the Syncfusion handler in your application startup.

### Locate MauiProgram.cs

Open the `MauiProgram.cs` file in your project root.

### Add Using Statement

```csharp
using Syncfusion.Maui.Core.Hosting;
```

### Register Handler

Add `.ConfigureSyncfusionCore()` to the builder chain:

```csharp
using Microsoft.Maui;
using Microsoft.Maui.Hosting;
using Microsoft.Maui.Controls.Hosting;
using Syncfusion.Maui.Core.Hosting;

namespace DigitalGaugeApp
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .ConfigureSyncfusionCore()  // Register Syncfusion handler
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

**Important:** This registration must be done before building the app, or the DigitalGauge will not render correctly.

## Step 4: Add DigitalGauge to Your Page

### XAML Implementation

#### Add Namespace Declaration

In your XAML file (e.g., `MainPage.xaml`), add the gauge namespace:

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:gauge="clr-namespace:Syncfusion.Maui.Gauges;assembly=Syncfusion.Maui.Gauges"
             x:Class="DigitalGaugeApp.MainPage">
    
    <!-- Your content here -->
    
</ContentPage>
```

#### Add Basic DigitalGauge

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:gauge="clr-namespace:Syncfusion.Maui.Gauges;assembly=Syncfusion.Maui.Gauges"
             x:Class="DigitalGaugeApp.MainPage">
    
    <VerticalStackLayout Padding="30" Spacing="25">
        <Label Text="Digital Gauge Example" 
               FontSize="24" 
               HorizontalOptions="Center" />
        
        <!-- Basic digital gauge without text -->
        <gauge:SfDigitalGauge />
        
    </VerticalStackLayout>
    
</ContentPage>
```

### C# Implementation

Open your code-behind file (e.g., `MainPage.xaml.cs`) or create a new page in C#:

```csharp
using Syncfusion.Maui.Gauges;
using Microsoft.Maui.Controls;

namespace DigitalGaugeApp
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            // Create digital gauge
            SfDigitalGauge digitalGauge = new SfDigitalGauge();
            
            // Add to page
            this.Content = digitalGauge;
        }
    }
}
```

## Step 5: Display Text in DigitalGauge

The most important property of DigitalGauge is the `Text` property, which specifies what to display.

### XAML with Text

```xml
<gauge:SfDigitalGauge Text="12345" />
```

### C# with Text

```csharp
SfDigitalGauge digitalGauge = new SfDigitalGauge();
digitalGauge.Text = "12345";
this.Content = digitalGauge;
```

### Complete XAML Example

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:gauge="clr-namespace:Syncfusion.Maui.Gauges;assembly=Syncfusion.Maui.Gauges"
             x:Class="DigitalGaugeApp.MainPage">
    
    <VerticalStackLayout Padding="30" Spacing="25">
        
        <Label Text="Digital Gauge Examples" 
               FontSize="24" 
               HorizontalOptions="Center" />
        
        <!-- Simple number display -->
        <gauge:SfDigitalGauge Text="12345" />
        
        <!-- Text display -->
        <gauge:SfDigitalGauge Text="SYNCFUSION" />
        
        <!-- Date display -->
        <gauge:SfDigitalGauge Text="2026-03-19" />
        
    </VerticalStackLayout>
    
</ContentPage>
```

### Complete C# Example

```csharp
using Syncfusion.Maui.Gauges;
using Microsoft.Maui.Controls;

namespace DigitalGaugeApp
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            // Create container
            var layout = new VerticalStackLayout
            {
                Padding = new Thickness(30),
                Spacing = 25
            };
            
            // Title
            layout.Add(new Label
            {
                Text = "Digital Gauge Examples",
                FontSize = 24,
                HorizontalOptions = LayoutOptions.Center
            });
            
            // Number display
            var numberGauge = new SfDigitalGauge
            {
                Text = "12345"
            };
            layout.Add(numberGauge);
            
            // Text display
            var textGauge = new SfDigitalGauge
            {
                Text = "SYNCFUSION"
            };
            layout.Add(textGauge);
            
            // Date display
            var dateGauge = new SfDigitalGauge
            {
                Text = "2026-03-19"
            };
            layout.Add(dateGauge);
            
            this.Content = layout;
        }
    }
}
```

## Running Your Application

### Visual Studio

1. Select your target platform (Android, iOS, Windows, or MacCatalyst)
2. Press **F5** or click **Start Debugging**
3. The app will build and launch on your selected platform

### .NET CLI

```bash
# Run on Windows
dotnet build -t:Run -f net9.0-windows10.0.19041.0

# Run on Android (device/emulator must be connected)
dotnet build -t:Run -f net9.0-android

# Run on iOS (Mac only, device/simulator must be available)
dotnet build -t:Run -f net9.0-ios
```

## Troubleshooting

### DigitalGauge Not Rendering

**Problem:** The gauge appears as a blank space or doesn't show up at all.

**Solutions:**
1. Verify `.ConfigureSyncfusionCore()` is called in MauiProgram.cs
2. Ensure the NuGet package is properly installed
3. Clean and rebuild the solution
4. Check that the namespace is correctly imported

### Namespace Not Found

**Problem:** `The type or namespace name 'Syncfusion' could not be found`

**Solutions:**
1. Verify the NuGet package is installed
2. Restore NuGet packages (right-click solution → Restore NuGet Packages)
3. Clean solution and rebuild
4. Check the package version is compatible with your .NET version

### Text Not Displaying

**Problem:** DigitalGauge renders but the text doesn't appear.

**Solutions:**
1. Ensure the `Text` property is set
2. Check that the text contains supported characters for the current CharacterType
3. Verify character colors are not the same as the background
4. Check CharacterHeight and CharacterWidth are appropriate values

### Build Errors After Installation

**Problem:** Build fails after installing the Syncfusion package.

**Solutions:**
1. Clean the solution (Build → Clean Solution)
2. Delete `bin` and `obj` folders
3. Restore NuGet packages
4. Rebuild the solution
5. Restart Visual Studio if the issue persists

## Next Steps

Now that you have a basic DigitalGauge working, explore:
- Character segment types (7, 14, 16-segment, dot matrix)
- Display types (numbers, alphabets, special characters)
- Customization options (colors, sizes, spacing)
- Event handling (TextChanged)

## Quick Reference

### Minimal XAML Setup

```xml
<ContentPage xmlns:gauge="clr-namespace:Syncfusion.Maui.Gauges;assembly=Syncfusion.Maui.Gauges">
    <gauge:SfDigitalGauge Text="HELLO" />
</ContentPage>
```

### Minimal C# Setup

```csharp
using Syncfusion.Maui.Gauges;

var gauge = new SfDigitalGauge { Text = "HELLO" };
this.Content = gauge;
```

### Required Using Statements

```csharp
using Syncfusion.Maui.Gauges;  // For SfDigitalGauge
using Syncfusion.Maui.Core.Hosting;  // For ConfigureSyncfusionCore
```
