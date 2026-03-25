# Getting Started with .NET MAUI Radial Gauge

## Table of Contents
- [Overview](#overview)
- [Installation](#installation)
- [Registering the Syncfusion Handler](#registering-the-syncfusion-handler)
- [Creating Your First Radial Gauge](#creating-your-first-radial-gauge)
- [Adding an Axis](#adding-an-axis)
- [Adding Ranges](#adding-ranges)
- [Adding a Pointer](#adding-a-pointer)
- [Adding an Annotation](#adding-an-annotation)
- [Complete Working Example](#complete-working-example)
- [Project Setup Checklist](#project-setup-checklist)
- [Troubleshooting](#troubleshooting)

## Overview

This guide walks you through the complete setup process for the Syncfusion .NET MAUI Radial Gauge control, from installation to creating your first fully functional gauge with ranges, pointers, and annotations.

**What You'll Build:** A speedometer-style gauge with color-coded zones, a needle pointer showing the current value, and a text annotation displaying the numeric value.

## Installation

### Step 1: Create a New .NET MAUI Project

**Visual Studio:**
1. Go to **File > New > Project**
2. Choose **.NET MAUI App** template
3. Name the project (e.g., "MyGaugeApp")
4. Choose a location and click **Next**
5. Select the .NET framework version and click **Create**

**Visual Studio Code or Command Line:**
```bash
dotnet new maui -n MyGaugeApp
cd MyGaugeApp
```

### Step 2: Install the Syncfusion .NET MAUI Gauges NuGet Package

**Visual Studio:**
1. In **Solution Explorer**, right-click the project
2. Choose **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.Gauges`
4. Install the latest version
5. Accept the license agreement

**Command Line:**
```bash
dotnet add package Syncfusion.Maui.Gauges
```

**Package Details:**
- **Package Name:** `Syncfusion.Maui.Gauges`
- **Dependency:** `Syncfusion.Maui.Core` (installed automatically)
- **License:** Syncfusion Community License or commercial license required

### Step 3: Verify Installation

After installation, check that the package appears in your `.csproj` file:

```xml
<ItemGroup>
  <PackageReference Include="Syncfusion.Maui.Gauges" Version="*" />
</ItemGroup>
```

## Registering the Syncfusion Handler

The `Syncfusion.Maui.Core` NuGet package is a required dependency for all Syncfusion .NET MAUI controls. You must register the Syncfusion handler in your `MauiProgram.cs` file.

### MauiProgram.cs Configuration

Open `MauiProgram.cs` and add the Syncfusion Core configuration:

```csharp
using Syncfusion.Maui.Core.Hosting;

namespace MyGaugeApp
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            
            // Register Syncfusion Core handler (REQUIRED)
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
}
```

**Key Points:**
- Add `using Syncfusion.Maui.Core.Hosting;` at the top
- Call `builder.ConfigureSyncfusionCore();` before building the app
- This must be done **before** any Syncfusion controls are used

## Creating Your First Radial Gauge

### Step 1: Import the Namespace

Add the Syncfusion.Maui.Gauges namespace to your XAML or C# file.

**XAML (in ContentPage or other container):**
```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:gauge="clr-namespace:Syncfusion.Maui.Gauges;assembly=Syncfusion.Maui.Gauges"
             x:Class="MyGaugeApp.MainPage">
    <!-- Content here -->
</ContentPage>
```

**C# (in code-behind or view model):**
```csharp
using Syncfusion.Maui.Gauges;
```

### Step 2: Initialize the SfRadialGauge

**XAML:**
```xaml
<gauge:SfRadialGauge />
```

**C#:**
```csharp
SfRadialGauge gauge = new SfRadialGauge();
this.Content = gauge;
```

At this point, you have an empty gauge container. It won't display anything useful until you add an axis.

## Adding an Axis

The axis is the circular scale on which values are displayed. Every radial gauge needs at least one axis.

### Basic Axis Setup

**XAML:**
```xaml
<gauge:SfRadialGauge>
    <gauge:SfRadialGauge.Axes>
        <gauge:RadialAxis Minimum="0"
                          Maximum="150"
                          Interval="10" />
    </gauge:SfRadialGauge.Axes>
</gauge:SfRadialGauge>
```

**C#:**
```csharp
SfRadialGauge gauge = new SfRadialGauge();

RadialAxis axis = new RadialAxis
{
    Minimum = 0,
    Maximum = 150,
    Interval = 10
};

gauge.Axes.Add(axis);
this.Content = gauge;
```

**What This Creates:**
- A circular gauge from 0 to 150
- Labels at intervals of 10 (0, 10, 20, ..., 150)
- Major ticks at each label
- Minor ticks between labels (default 1 per interval)

**Result:** A basic circular gauge with axis line, labels, and ticks.

## Adding Ranges

Ranges add color-coded zones to help users interpret values at a glance (e.g., safe/warning/danger zones).

### Creating Color-Coded Ranges

**XAML:**
```xaml
<gauge:SfRadialGauge>
    <gauge:SfRadialGauge.Axes>
        <gauge:RadialAxis Minimum="0"
                          Maximum="150"
                          Interval="10">
            
            <gauge:RadialAxis.Ranges>
                <gauge:RadialRange StartValue="0"
                                   EndValue="50"
                                   Fill="Green" />
                <gauge:RadialRange StartValue="50"
                                   EndValue="100"
                                   Fill="Orange" />
                <gauge:RadialRange StartValue="100"
                                   EndValue="150"
                                   Fill="Red" />
            </gauge:RadialAxis.Ranges>
            
        </gauge:RadialAxis>
    </gauge:SfRadialGauge.Axes>
</gauge:SfRadialGauge>
```

**C#:**
```csharp
RadialAxis axis = new RadialAxis
{
    Minimum = 0,
    Maximum = 150,
    Interval = 10
};

// Green zone (safe)
axis.Ranges.Add(new RadialRange 
{ 
    StartValue = 0, 
    EndValue = 50, 
    Fill = new SolidColorBrush(Colors.Green) 
});

// Orange zone (warning)
axis.Ranges.Add(new RadialRange 
{ 
    StartValue = 50, 
    EndValue = 100, 
    Fill = new SolidColorBrush(Colors.Orange) 
});

// Red zone (danger)
axis.Ranges.Add(new RadialRange 
{ 
    StartValue = 100, 
    EndValue = 150, 
    Fill = new SolidColorBrush(Colors.Red) 
});

gauge.Axes.Add(axis);
```

**What This Creates:** Three colored arc segments showing different value zones.

## Adding a Pointer

Pointers indicate the current value on the gauge. The most common type is the needle pointer.

### Adding a Needle Pointer

**XAML:**
```xaml
<gauge:RadialAxis Minimum="0"
                  Maximum="150"
                  Interval="10">
    <!-- Ranges here -->
    
    <gauge:RadialAxis.Pointers>
        <gauge:NeedlePointer Value="90" />
    </gauge:RadialAxis.Pointers>
    
</gauge:RadialAxis>
```

**C#:**
```csharp
NeedlePointer pointer = new NeedlePointer
{
    Value = 90
};

axis.Pointers.Add(pointer);
```

**What This Creates:** A needle pointing to the value 90 on the gauge.

## Adding an Annotation

Annotations display text, images, or custom views at specific positions on the gauge.

### Adding a Value Label Annotation

**XAML:**
```xaml
<gauge:RadialAxis Minimum="0"
                  Maximum="150"
                  Interval="10">
    <!-- Ranges and Pointers here -->
    
    <gauge:RadialAxis.Annotations>
        <gauge:GaugeAnnotation DirectionUnit="Angle"
                               DirectionValue="90"
                               PositionFactor="0.5">
            <gauge:GaugeAnnotation.Content>
                <Label Text="90"
                       FontSize="25"
                       FontAttributes="Bold"
                       TextColor="Black" />
            </gauge:GaugeAnnotation.Content>
        </gauge:GaugeAnnotation>
    </gauge:RadialAxis.Annotations>
    
</gauge:RadialAxis>
```

**C#:**
```csharp
GaugeAnnotation annotation = new GaugeAnnotation
{
    DirectionUnit = AnnotationDirection.Angle,
    DirectionValue = 90,
    PositionFactor = 0.5,
    Content = new Label 
    { 
        Text = "90", 
        FontSize = 25, 
        FontAttributes = FontAttributes.Bold,
        TextColor = Colors.Black
    }
};

axis.Annotations.Add(annotation);
```

**What This Creates:** A bold "90" label displayed in the center of the gauge.

**Position Explanation:**
- `DirectionUnit="Angle"` - Position by angle (0° = right, 90° = bottom, 180° = left, 270° = top)
- `DirectionValue="90"` - Position at 90° (bottom of circle)
- `PositionFactor="0.5"` - Halfway from center (0) to edge (1)

## Complete Working Example

Here's the complete code combining all the elements above:

**XAML (MainPage.xaml):**
```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:gauge="clr-namespace:Syncfusion.Maui.Gauges;assembly=Syncfusion.Maui.Gauges"
             x:Class="MyGaugeApp.MainPage">

    <gauge:SfRadialGauge>
        <gauge:SfRadialGauge.Axes>
            <gauge:RadialAxis Minimum="0"
                              Maximum="150"
                              Interval="10">
                
                <!-- Color-coded ranges -->
                <gauge:RadialAxis.Ranges>
                    <gauge:RadialRange StartValue="0"
                                       EndValue="50"
                                       Fill="Green" />
                    <gauge:RadialRange StartValue="50"
                                       EndValue="100"
                                       Fill="Orange" />
                    <gauge:RadialRange StartValue="100"
                                       EndValue="150"
                                       Fill="Red" />
                </gauge:RadialAxis.Ranges>
                
                <!-- Needle pointer -->
                <gauge:RadialAxis.Pointers>
                    <gauge:NeedlePointer Value="90" />
                </gauge:RadialAxis.Pointers>
                
                <!-- Value annotation -->
                <gauge:RadialAxis.Annotations>
                    <gauge:GaugeAnnotation DirectionUnit="Angle"
                                           DirectionValue="90"
                                           PositionFactor="0.5">
                        <gauge:GaugeAnnotation.Content>
                            <Label Text="90"
                                   FontSize="25"
                                   FontAttributes="Bold"
                                   TextColor="Black" />
                        </gauge:GaugeAnnotation.Content>
                    </gauge:GaugeAnnotation>
                </gauge:RadialAxis.Annotations>
                
            </gauge:RadialAxis>
        </gauge:SfRadialGauge.Axes>
    </gauge:SfRadialGauge>

</ContentPage>
```

**C# (Alternative Code-Behind Approach):**
```csharp
using Syncfusion.Maui.Gauges;

namespace MyGaugeApp;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        // Create gauge
        SfRadialGauge gauge = new SfRadialGauge();
        
        // Create axis
        RadialAxis axis = new RadialAxis
        {
            Minimum = 0,
            Maximum = 150,
            Interval = 10
        };
        
        // Add ranges
        axis.Ranges.Add(new RadialRange 
        { 
            StartValue = 0, 
            EndValue = 50, 
            Fill = new SolidColorBrush(Colors.Green) 
        });
        axis.Ranges.Add(new RadialRange 
        { 
            StartValue = 50, 
            EndValue = 100, 
            Fill = new SolidColorBrush(Colors.Orange) 
        });
        axis.Ranges.Add(new RadialRange 
        { 
            StartValue = 100, 
            EndValue = 150, 
            Fill = new SolidColorBrush(Colors.Red) 
        });
        
        // Add pointer
        axis.Pointers.Add(new NeedlePointer { Value = 90 });
        
        // Add annotation
        GaugeAnnotation annotation = new GaugeAnnotation
        {
            DirectionUnit = AnnotationDirection.Angle,
            DirectionValue = 90,
            PositionFactor = 0.5,
            Content = new Label 
            { 
                Text = "90", 
                FontSize = 25, 
                FontAttributes = FontAttributes.Bold,
                TextColor = Colors.Black
            }
        };
        axis.Annotations.Add(annotation);
        
        // Add axis to gauge
        gauge.Axes.Add(axis);
        
        // Set as page content
        this.Content = gauge;
    }
}
```

## Project Setup Checklist

Use this checklist to verify your setup:

- [ ] .NET MAUI workload installed (`dotnet workload install maui`)
- [ ] Syncfusion.Maui.Gauges NuGet package installed
- [ ] `builder.ConfigureSyncfusionCore()` added to MauiProgram.cs
- [ ] Syncfusion namespace imported in XAML or C#
- [ ] Valid Syncfusion license (community or commercial)
- [ ] At least one RadialAxis added to SfRadialGauge.Axes collection
- [ ] Project builds without errors
- [ ] Gauge displays on the page

## Troubleshooting

### "The name 'SfRadialGauge' does not exist in the namespace"

**Cause:** Namespace not imported or NuGet package not installed.

**Solution:**
1. Verify package is installed: `dotnet list package`
2. Add namespace: `xmlns:gauge="clr-namespace:Syncfusion.Maui.Gauges;assembly=Syncfusion.Maui.Gauges"`
3. Rebuild the project

### "Syncfusion license key not registered"

**Cause:** No license key configured (required for production).

**Solution:**
1. Get a license from [Syncfusion](https://www.syncfusion.com/sales/products)
2. Register in App.xaml.cs or MauiProgram.cs:
   ```csharp
   Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
   ```

### Gauge not displaying or blank page

**Cause:** Missing `ConfigureSyncfusionCore()` registration or no axis added.

**Solution:**
1. Verify `builder.ConfigureSyncfusionCore()` in MauiProgram.cs
2. Ensure at least one RadialAxis is added to the Axes collection
3. Check that Content is set: `this.Content = gauge;`

### Build errors after installing package

**Cause:** NuGet package cache or version mismatch.

**Solution:**
1. Clean and rebuild: `dotnet clean && dotnet build`
2. Clear NuGet cache: `dotnet nuget locals all --clear`
3. Update all Syncfusion packages to the same version

## Next Steps

Now that you have a working radial gauge:
- **Customize the axis** → See [axes.md](axes.md) for scale, labels, ticks, angles
- **Style your ranges** → See [ranges.md](ranges.md) for colors, gradients, positioning
- **Choose pointer types** → See [pointers.md](pointers.md) for needle, shape, content, range pointers
- **Add more annotations** → See [annotation.md](annotation.md) for positioning and content
- **Add animation** → See [animation-and-interaction.md](animation-and-interaction.md) for smooth transitions
