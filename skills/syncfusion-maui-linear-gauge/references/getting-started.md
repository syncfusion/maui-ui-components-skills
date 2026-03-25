# Getting Started with Linear Gauges

This guide covers everything you need to set up and implement your first Syncfusion .NET MAUI Linear Gauge, from installation to a complete working example.

## Overview

The Syncfusion .NET MAUI Linear Gauge (SfLinearGauge) is a data visualization control that displays values on a linear scale. It's perfect for creating:
- Progress indicators and loading bars
- Temperature displays and thermometer visualizations
- Speed meters and volume indicators
- Interactive sliders with visual feedback
- Performance dashboards with color-coded ranges

## Installation

**Option 1: Visual Studio NuGet Package Manager**

1. Right-click your project in Solution Explorer
2. Select **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.Gauges`
4. Install the latest version
5. Verify dependencies are automatically installed

**Option 2: Package Manager Console**

```powershell
Install-Package Syncfusion.Maui.Gauges
```

**Option 3: .NET CLI**

```bash
dotnet add package Syncfusion.Maui.Gauges
```

**Option 4: Edit .csproj Directly**

```xml
<ItemGroup>
  <PackageReference Include="Syncfusion.Maui.Gauges" Version="*" />
</ItemGroup>
```

## Handler Registration

The Syncfusion.Maui.Core package is a required dependency. You must register the Syncfusion handler in your `MauiProgram.cs` file.

### Step 1: Import Syncfusion Namespace

Add the following using statement at the top of `MauiProgram.cs`:

```csharp
using Syncfusion.Maui.Core.Hosting;
```

### Step 2: Register Syncfusion Core

In the `CreateMauiApp` method, call `ConfigureSyncfusionCore()`:

```csharp
public static class MauiProgram
{
    public static MauiApp CreateMauiApp()
    {
        var builder = MauiApp.CreateBuilder();
        
        // Register Syncfusion core handler
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

**Important:** Call `ConfigureSyncfusionCore()` before `UseMauiApp<App>()`.

## Basic Linear Gauge Implementation

### Import the Namespace

In your XAML page, add the Syncfusion.Maui.Gauges namespace:

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:gauge="clr-namespace:Syncfusion.Maui.Gauges;assembly=Syncfusion.Maui.Gauges"
             x:Class="YourApp.MainPage">
    
    <!-- Your content here -->
    
</ContentPage>
```

For C# code-behind, add:

```csharp
using Syncfusion.Maui.Gauges;
```

### Create a Default Linear Gauge

The simplest linear gauge with default settings:

**XAML:**
```xml
<gauge:SfLinearGauge />
```

**C#:**
```csharp
SfLinearGauge gauge = new SfLinearGauge();
this.Content = gauge;
```

**Result:** Displays a horizontal gauge with scale from 0 to 100, with labels and tick marks.

### Customize Scale Values

Set custom minimum and maximum values:

**XAML:**
```xml
<gauge:SfLinearGauge Minimum="0" Maximum="200" />
```

**C#:**
```csharp
SfLinearGauge gauge = new SfLinearGauge
{
    Minimum = 0,
    Maximum = 200
};
this.Content = gauge;
```

## Adding Scale Elements

### Adding Ranges

Ranges highlight specific value zones on the scale. They're perfect for color-coding performance levels.

**Basic Range:**

**XAML:**
```xml
<gauge:SfLinearGauge>
    <gauge:SfLinearGauge.Ranges>
        <gauge:LinearRange StartValue="20" EndValue="80" />
    </gauge:SfLinearGauge.Ranges>
</gauge:SfLinearGauge>
```

**C#:**
```csharp
SfLinearGauge gauge = new SfLinearGauge();
gauge.Ranges.Add(new LinearRange
{
    StartValue = 20,
    EndValue = 80
});
this.Content = gauge;
```

**Styled Range with Color:**

**XAML:**
```xml
<gauge:LinearRange StartValue="0" 
                   EndValue="50" 
                   Fill="#FF6B6B"
                   StartWidth="15"
                   EndWidth="15"/>
```

**C#:**
```csharp
gauge.Ranges.Add(new LinearRange
{
    StartValue = 0,
    EndValue = 50,
    Fill = new SolidColorBrush(Color.FromArgb("#FF6B6B")),
    StartWidth = 15,
    EndWidth = 15
});
```

**Multiple Ranges (Color Zones):**

```xml
<gauge:SfLinearGauge Minimum="0" Maximum="100">
    <gauge:SfLinearGauge.Ranges>
        <!-- Green zone (good) -->
        <gauge:LinearRange StartValue="0" EndValue="33" Fill="#4CAF50"/>
        <!-- Yellow zone (warning) -->
        <gauge:LinearRange StartValue="33" EndValue="66" Fill="#FFC107"/>
        <!-- Red zone (critical) -->
        <gauge:LinearRange StartValue="66" EndValue="100" Fill="#F44336"/>
    </gauge:SfLinearGauge.Ranges>
</gauge:SfLinearGauge>
```

## Adding Pointers

Pointers indicate specific values on the scale. Linear Gauge supports three pointer types:

### 1. Bar Pointer

A filled bar from the scale start to the pointer value.

**XAML:**
```xml
<gauge:SfLinearGauge>
    <gauge:SfLinearGauge.BarPointers>
        <gauge:BarPointer Value="60" Fill="#2196F3"/>
    </gauge:SfLinearGauge.BarPointers>
</gauge:SfLinearGauge>
```

**C#:**
```csharp
SfLinearGauge gauge = new SfLinearGauge();
gauge.BarPointers.Add(new BarPointer
{
    Value = 60,
    Fill = new SolidColorBrush(Color.FromArgb("#2196F3"))
});
this.Content = gauge;
```

### 2. Shape Marker Pointer

A shape (circle, triangle, diamond, etc.) that marks a specific value.

**XAML:**
```xml
<gauge:SfLinearGauge>
    <gauge:SfLinearGauge.MarkerPointers>
        <gauge:LinearShapePointer Value="70" 
                                  ShapeType="Circle"
                                  Fill="#FF5722"
                                  ShapeHeight="20"
                                  ShapeWidth="20"/>
    </gauge:SfLinearGauge.MarkerPointers>
</gauge:SfLinearGauge>
```

**C#:**
```csharp
gauge.MarkerPointers.Add(new LinearShapePointer
{
    Value = 70,
    ShapeType = LinearShapeType.Circle,
    Fill = new SolidColorBrush(Color.FromArgb("#FF5722")),
    ShapeHeight = 20,
    ShapeWidth = 20
});
```

**Available Shape Types:**
- `Circle`
- `Triangle`
- `InvertedTriangle`
- `Diamond`
- `Rectangle`

### 3. Content Marker Pointer

Use custom content (images, text, or any view) as a pointer.

**XAML:**
```xml
<gauge:SfLinearGauge>
    <gauge:SfLinearGauge.MarkerPointers>
        <gauge:LinearContentPointer Value="80">
            <gauge:LinearContentPointer.Content>
                <Image Source="pin.png" 
                       HeightRequest="25" 
                       WidthRequest="25"/>
            </gauge:LinearContentPointer.Content>
        </gauge:LinearContentPointer>
    </gauge:SfLinearGauge.MarkerPointers>
</gauge:SfLinearGauge>
```

**C#:**
```csharp
LinearContentPointer contentPointer = new LinearContentPointer
{
    Value = 80,
    Content = new Image 
    { 
        Source = "pin.png", 
        HeightRequest = 25, 
        WidthRequest = 25 
    }
};
gauge.MarkerPointers.Add(contentPointer);
```

## Complete Working Example

Here's a complete example combining ranges and multiple pointer types:

**XAML (MainPage.xaml):**

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:gauge="clr-namespace:Syncfusion.Maui.Gauges;assembly=Syncfusion.Maui.Gauges"
             x:Class="LinearGaugeDemo.MainPage">

    <VerticalStackLayout Padding="20" Spacing="20">
        
        <Label Text="Temperature Monitor" 
               FontSize="20" 
               FontAttributes="Bold"
               HorizontalOptions="Center"/>
        
        <gauge:SfLinearGauge Minimum="0" 
                            Maximum="100"
                            Interval="10">
            
            <!-- Background track -->
            <gauge:SfLinearGauge.Ranges>
                <gauge:LinearRange StartValue="0" 
                                   EndValue="100" 
                                   StartWidth="20"
                                   EndWidth="20"
                                   Fill="#E0E0E0"/>
            </gauge:SfLinearGauge.Ranges>
            
            <!-- Progress bar -->
            <gauge:SfLinearGauge.BarPointers>
                <gauge:BarPointer Value="70" 
                                 Fill="#FF6B6B"
                                 EnableAnimation="True"
                                 AnimationDuration="1000"/>
            </gauge:SfLinearGauge.BarPointers>
            
            <!-- Value marker -->
            <gauge:SfLinearGauge.MarkerPointers>
                <gauge:LinearShapePointer Value="70" 
                                         ShapeType="Circle"
                                         Fill="#D32F2F"
                                         ShapeHeight="25"
                                         ShapeWidth="25"
                                         IsInteractive="True"/>
            </gauge:SfLinearGauge.MarkerPointers>
            
        </gauge:SfLinearGauge>
        
        <Label Text="Drag the marker to adjust temperature" 
               HorizontalOptions="Center"
               TextColor="Gray"/>
        
    </VerticalStackLayout>

</ContentPage>
```

**C# (MainPage.xaml.cs):**

```csharp
using Syncfusion.Maui.Gauges;

namespace LinearGaugeDemo;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }
}
```

**Alternative: Pure C# Implementation:**

```csharp
using Syncfusion.Maui.Gauges;

namespace LinearGaugeDemo;

public class MainPage : ContentPage
{
    public MainPage()
    {
        var layout = new VerticalStackLayout
        {
            Padding = 20,
            Spacing = 20
        };
        
        // Title
        layout.Add(new Label
        {
            Text = "Temperature Monitor",
            FontSize = 20,
            FontAttributes = FontAttributes.Bold,
            HorizontalOptions = LayoutOptions.Center
        });
        
        // Create gauge
        SfLinearGauge gauge = new SfLinearGauge
        {
            Minimum = 0,
            Maximum = 100,
            Interval = 10
        };
        
        // Add background range
        gauge.Ranges.Add(new LinearRange
        {
            StartValue = 0,
            EndValue = 100,
            StartWidth = 20,
            EndWidth = 20,
            Fill = new SolidColorBrush(Color.FromArgb("#E0E0E0"))
        });
        
        // Add bar pointer
        gauge.BarPointers.Add(new BarPointer
        {
            Value = 70,
            Fill = new SolidColorBrush(Color.FromArgb("#FF6B6B")),
            EnableAnimation = true,
            AnimationDuration = 1000
        });
        
        // Add interactive marker
        gauge.MarkerPointers.Add(new LinearShapePointer
        {
            Value = 70,
            ShapeType = LinearShapeType.Circle,
            Fill = new SolidColorBrush(Color.FromArgb("#D32F2F")),
            ShapeHeight = 25,
            ShapeWidth = 25,
            IsInteractive = true
        });
        
        layout.Add(gauge);
        
        // Instructions
        layout.Add(new Label
        {
            Text = "Drag the marker to adjust temperature",
            HorizontalOptions = LayoutOptions.Center,
            TextColor = Colors.Gray
        });
        
        this.Content = layout;
    }
}
```

## Troubleshooting Setup Issues

### Issue 1: Gauge Not Appearing

**Symptoms:** Empty space where gauge should appear

**Solutions:**
1. Verify NuGet package is installed
2. Check that `ConfigureSyncfusionCore()` is called in MauiProgram.cs
3. Ensure namespace is imported correctly
4. Set explicit height: `<gauge:SfLinearGauge HeightRequest="100" />`

### Issue 2: Handler Not Registered Error

**Error:** "Handler not found for view SfLinearGauge"

**Solution:** Add `builder.ConfigureSyncfusionCore();` in MauiProgram.cs before `UseMauiApp<App>()`

```csharp
builder.ConfigureSyncfusionCore();  // Add this
builder.UseMauiApp<App>()
```

### Issue 3: Namespace Not Found

**Error:** "The type or namespace name 'Syncfusion' could not be found"

**Solutions:**
1. Verify package is installed via NuGet Package Manager
2. Restore NuGet packages: `dotnet restore`
3. Clean and rebuild: `dotnet clean && dotnet build`
4. Check .csproj for PackageReference

### Issue 4: Pointers Not Visible

**Symptoms:** Scale appears but no pointers

**Solutions:**
1. Verify pointer Value is within Minimum/Maximum range
2. Check Fill property is set (transparent by default)
3. Ensure pointer is added to correct collection (BarPointers vs MarkerPointers)
4. Set explicit size for shape pointers (ShapeHeight, ShapeWidth)

### Issue 5: Build Errors After Installation

**Error:** Compile-time errors after adding package

**Solutions:**
1. Close and restart Visual Studio
2. Delete `bin` and `obj` folders
3. Restore packages: Right-click solution → Restore NuGet Packages
4. Rebuild solution

## Next Steps

Now that you have a working linear gauge:

- **Customize the scale** → Read [scale-configuration.md](scale-configuration.md)
- **Style labels and ticks** → Read [labels-and-ticks.md](labels-and-ticks.md)
- **Add visual ranges** → Read [ranges.md](ranges.md)
- **Configure pointers** → Read [bar-pointer.md](bar-pointer.md) and [marker-pointers.md](marker-pointers.md)
- **Enable interaction** → Read [interaction.md](interaction.md)
- **Add animations** → Read [animation-and-effects.md](animation-and-effects.md)
