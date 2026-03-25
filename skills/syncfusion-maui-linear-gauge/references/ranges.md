# Ranges

## Table of Contents
- [Overview](#overview)
- [Basic Range Implementation](#basic-range-implementation)
- [Range Shape Customization](#range-shape-customization)
- [Range Styling](#range-styling)
- [Range Positioning](#range-positioning)
- [Multiple Ranges](#multiple-ranges)
- [Range Child Content](#range-child-content)
- [Use Range Color for Axis](#use-range-color-for-axis)
- [Common Range Patterns](#common-range-patterns)

## Overview

Ranges are visual elements that highlight specific value zones on the scale track. They're perfect for creating color-coded performance indicators, status zones, and visual data segmentation.

**Key Uses:**
- Performance zones (red/yellow/green)
- Temperature ranges (cold/moderate/hot)
- Status indicators (low/medium/high)
- Background tracks for pointers
- Visual data segmentation

## Basic Range Implementation

Create a simple range by specifying start and end values.

**XAML:**
```xml
<gauge:SfLinearGauge>
    <gauge:SfLinearGauge.Ranges>
        <gauge:LinearRange StartValue="20" EndValue="80"/>
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

**Default Appearance:**
- Positioned outside the scale
- Default fill color
- Constant width (matches scale thickness)

## Range Shape Customization

Control range shape using width properties to create rectangles, trapezoids, or curved shapes.

### Width Properties

**`StartWidth`** - Width at the beginning of the range  
**`MidWidth`** - Width at the middle (creates curves)  
**`EndWidth`** - Width at the end of the range

### Rectangle Range (Constant Width)

**XAML:**
```xml
<gauge:LinearRange StartValue="0" 
                   EndValue="100" 
                   StartWidth="20"
                   EndWidth="20"/>
```

**C#:**
```csharp
gauge.Ranges.Add(new LinearRange
{
    StartValue = 0,
    EndValue = 100,
    StartWidth = 20,
    EndWidth = 20
});
```

### Convex Range (Bulge Outward)

**XAML:**
```xml
<gauge:LinearRange StartValue="0" 
                   EndValue="100"
                   StartWidth="10" 
                   MidWidth="40"
                   EndWidth="10"/>
```

**C#:**
```csharp
gauge.Ranges.Add(new LinearRange
{
    StartValue = 0,
    EndValue = 100,
    StartWidth = 10,
    MidWidth = 40,   // Wider in middle
    EndWidth = 10
});
```

### Concave Range (Curve Inward)

For concave shapes, extend `LinearRange` and override `UpdateMidRangePath`:

**XAML:**
```xml
<gauge:SfLinearGauge>
    <gauge:SfLinearGauge.Ranges>
        <local:ConcaveLinearRange StartValue="0" 
                                  EndValue="100"
                                  StartWidth="40" 
                                  MidWidth="-20"
                                  EndWidth="40"/>
    </gauge:SfLinearGauge.Ranges>
</gauge:SfLinearGauge>
```

**C#:**
```csharp
public class ConcaveLinearRange : LinearRange
{
    protected override void UpdateMidRangePath(PathF pathF, PointF startPoint, PointF midPoint, PointF endPoint)
    {
        pathF.CurveTo(startPoint, midPoint, endPoint);
    }
}

// Usage
gauge.Ranges.Add(new ConcaveLinearRange
{
    StartValue = 0,
    EndValue = 100,
    StartWidth = 40,
    MidWidth = -20,  // Negative for concave
    EndWidth = 40
});
```

### Tapered Range (Wedge Shape)

**XAML:**
```xml
<gauge:LinearRange StartValue="0" 
                   EndValue="100"
                   StartWidth="5"
                   EndWidth="30"/>
```

**C#:**
```csharp
gauge.Ranges.Add(new LinearRange
{
    StartValue = 0,
    EndValue = 100,
    StartWidth = 5,   // Narrow start
    EndWidth = 30     // Wide end
});
```

## Range Styling

### Solid Fill Color

**XAML:**
```xml
<gauge:LinearRange StartValue="0" 
                   EndValue="100" 
                   Fill="#4CAF50"/>
```

**C#:**
```csharp
gauge.Ranges.Add(new LinearRange
{
    StartValue = 0,
    EndValue = 100,
    Fill = new SolidColorBrush(Color.FromArgb("#4CAF50"))
});
```

### Gradient Fill

Apply gradients using `GradientStops`:

**XAML:**
```xml
<gauge:LinearRange StartValue="0" EndValue="100" StartWidth="30">
    <gauge:LinearRange.GradientStops>
        <gauge:GaugeGradientStop Value="0" Color="Red"/>
        <gauge:GaugeGradientStop Value="50" Color="Yellow"/>
        <gauge:GaugeGradientStop Value="100" Color="Green"/>
    </gauge:LinearRange.GradientStops>
</gauge:LinearRange>
```

**C#:**
```csharp
var gradientStops = new ObservableCollection<GaugeGradientStop>
{
    new GaugeGradientStop { Value = 0, Color = Colors.Red },
    new GaugeGradientStop { Value = 50, Color = Colors.Yellow },
    new GaugeGradientStop { Value = 100, Color = Colors.Green }
};

gauge.Ranges.Add(new LinearRange
{
    StartValue = 0,
    EndValue = 100,
    StartWidth = 30,
    GradientStops = gradientStops
});
```

**Gradient Types:**
- **Linear:** Smooth color transition along range
- **Value-based:** Gradient stops at specific scale values
- **Multi-stop:** Complex gradients with multiple colors

### Semi-Transparent Range

Use alpha channel for transparency:

```csharp
gauge.Ranges.Add(new LinearRange
{
    StartValue = 0,
    EndValue = 100,
    Fill = new SolidColorBrush(Color.FromArgb("#80FF5722"))  // 50% opacity
});
```

## Range Positioning

Position ranges relative to the scale track.

### Position Options

**`Outside`** - Range outside the scale (default)  
**`Inside`** - Range inside the scale  
**`Cross`** - Range crosses the scale

**XAML:**
```xml
<!-- Outside (default) -->
<gauge:LinearRange Position="Outside"/>

<!-- Inside the scale -->
<gauge:LinearRange Position="Inside"/>

<!-- Crossing the scale -->
<gauge:LinearRange Position="Cross"/>
```

**C#:**
```csharp
// Outside
gauge.Ranges.Add(new LinearRange
{
    Position = GaugeElementPosition.Outside
});

// Inside
gauge.Ranges.Add(new LinearRange
{
    Position = GaugeElementPosition.Inside
});

// Cross
gauge.Ranges.Add(new LinearRange
{
    Position = GaugeElementPosition.Cross
});
```

**Use Cases:**
- **Outside:** Background track, surrounding indicators
- **Inside:** Compact layouts, fill-style indicators
- **Cross:** Integrated appearance, combined with scale

## Multiple Ranges

Add multiple ranges to create segmented visualizations.

### Three-Zone Performance Indicator

**XAML:**
```xml
<gauge:SfLinearGauge>
    <gauge:SfLinearGauge.Ranges>
        <!-- Red zone (0-33): Poor -->
        <gauge:LinearRange StartValue="0" 
                          EndValue="33" 
                          Fill="#F44336"
                          StartWidth="20"
                          EndWidth="20"/>
        
        <!-- Yellow zone (33-66): Fair -->
        <gauge:LinearRange StartValue="33" 
                          EndValue="66" 
                          Fill="#FFC107"
                          StartWidth="20"
                          EndWidth="20"/>
        
        <!-- Green zone (66-100): Good -->
        <gauge:LinearRange StartValue="66" 
                          EndValue="100" 
                          Fill="#4CAF50"
                          StartWidth="20"
                          EndWidth="20"/>
    </gauge:SfLinearGauge.Ranges>
</gauge:SfLinearGauge>
```

**C#:**
```csharp
SfLinearGauge performanceGauge = new SfLinearGauge();

// Poor (Red)
performanceGauge.Ranges.Add(new LinearRange
{
    StartValue = 0,
    EndValue = 33,
    Fill = new SolidColorBrush(Color.FromArgb("#F44336")),
    StartWidth = 20,
    EndWidth = 20
});

// Fair (Yellow)
performanceGauge.Ranges.Add(new LinearRange
{
    StartValue = 33,
    EndValue = 66,
    Fill = new SolidColorBrush(Color.FromArgb("#FFC107")),
    StartWidth = 20,
    EndWidth = 20
});

// Good (Green)
performanceGauge.Ranges.Add(new LinearRange
{
    StartValue = 66,
    EndValue = 100,
    Fill = new SolidColorBrush(Color.FromArgb("#4CAF50")),
    StartWidth = 20,
    EndWidth = 20
});
```

### Five-Zone Temperature Scale

```csharp
// Freezing (Blue)
gauge.Ranges.Add(new LinearRange
{
    StartValue = -20,
    EndValue = 0,
    Fill = new SolidColorBrush(Color.FromArgb("#2196F3"))
});

// Cold (Light Blue)
gauge.Ranges.Add(new LinearRange
{
    StartValue = 0,
    EndValue = 15,
    Fill = new SolidColorBrush(Color.FromArgb("#81D4FA"))
});

// Moderate (Green)
gauge.Ranges.Add(new LinearRange
{
    StartValue = 15,
    EndValue = 25,
    Fill = new SolidColorBrush(Color.FromArgb("#4CAF50"))
});

// Warm (Orange)
gauge.Ranges.Add(new LinearRange
{
    StartValue = 25,
    EndValue = 35,
    Fill = new SolidColorBrush(Color.FromArgb("#FF9800"))
});

// Hot (Red)
gauge.Ranges.Add(new LinearRange
{
    StartValue = 35,
    EndValue = 50,
    Fill = new SolidColorBrush(Color.FromArgb("#F44336"))
});
```

## Range Child Content

Add custom content inside ranges using the `Child` property.

**XAML:**
```xml
<gauge:SfLinearGauge ShowLabels="False" ShowTicks="False">
    <gauge:SfLinearGauge.Ranges>
        
        <!-- Bad range with label -->
        <gauge:LinearRange StartValue="0" 
                          EndValue="30"
                          StartWidth="40" 
                          EndWidth="40"
                          Fill="#FF5722">
            <gauge:LinearRange.Child>
                <Label Text="Bad" 
                       HorizontalOptions="Center"
                       VerticalOptions="Center" 
                       TextColor="White"
                       FontAttributes="Bold"/>
            </gauge:LinearRange.Child>
        </gauge:LinearRange>
        
        <!-- Good range with label -->
        <gauge:LinearRange StartValue="30" 
                          EndValue="70"
                          StartWidth="40" 
                          EndWidth="40"
                          Fill="#FFC107">
            <gauge:LinearRange.Child>
                <Label Text="Good" 
                       HorizontalOptions="Center"
                       VerticalOptions="Center" 
                       TextColor="Black"
                       FontAttributes="Bold"/>
            </gauge:LinearRange.Child>
        </gauge:LinearRange>
        
        <!-- Excellent range with label -->
        <gauge:LinearRange StartValue="70" 
                          EndValue="100"
                          StartWidth="40" 
                          EndWidth="40"
                          Fill="#4CAF50">
            <gauge:LinearRange.Child>
                <Label Text="Excellent" 
                       HorizontalOptions="Center"
                       VerticalOptions="Center" 
                       TextColor="White"
                       FontAttributes="Bold"/>
            </gauge:LinearRange.Child>
        </gauge:LinearRange>
        
    </gauge:SfLinearGauge.Ranges>
</gauge:SfLinearGauge>
```

**C# with Icons:**
```csharp
// Range with icon
LinearRange rangeWithIcon = new LinearRange
{
    StartValue = 0,
    EndValue = 50,
    StartWidth = 50,
    EndWidth = 50,
    Fill = new SolidColorBrush(Color.FromArgb("#FF6B6B")),
    Child = new Image
    {
        Source = "warning_icon.png",
        HeightRequest = 20,
        WidthRequest = 20,
        HorizontalOptions = LayoutOptions.Center,
        VerticalOptions = LayoutOptions.Center
    }
};
gauge.Ranges.Add(rangeWithIcon);
```

## Use Range Color for Axis

Apply range colors to scale elements (track, labels, ticks) using `UseRangeColorForAxis`.

**XAML:**
```xml
<gauge:SfLinearGauge UseRangeColorForAxis="True">
    <gauge:SfLinearGauge.Ranges>
        <gauge:LinearRange StartValue="0" 
                          EndValue="33" 
                          Fill="#F45656"
                          Position="Cross"/>
        <gauge:LinearRange StartValue="33" 
                          EndValue="66" 
                          Fill="#FFC93E"
                          Position="Cross"/>
        <gauge:LinearRange StartValue="66" 
                          EndValue="100" 
                          Fill="#0DC9AB"
                          Position="Cross"/>
    </gauge:SfLinearGauge.Ranges>
</gauge:SfLinearGauge>
```

**C#:**
```csharp
SfLinearGauge coloredAxisGauge = new SfLinearGauge
{
    UseRangeColorForAxis = true
};

coloredAxisGauge.Ranges.Add(new LinearRange
{
    StartValue = 0,
    EndValue = 33,
    Fill = new SolidColorBrush(Color.FromArgb("#F45656")),
    Position = GaugeElementPosition.Cross
});

coloredAxisGauge.Ranges.Add(new LinearRange
{
    StartValue = 33,
    EndValue = 66,
    Fill = new SolidColorBrush(Color.FromArgb("#FFC93E")),
    Position = GaugeElementPosition.Cross
});

coloredAxisGauge.Ranges.Add(new LinearRange
{
    StartValue = 66,
    EndValue = 100,
    Fill = new SolidColorBrush(Color.FromArgb("#0DC9AB")),
    Position = GaugeElementPosition.Cross
});
```

**Effect:** Scale track, labels, and ticks adopt the color of the range they fall within.

## Common Range Patterns

### Pattern 1: Background Track for Progress

```xml
<gauge:SfLinearGauge ShowLabels="False" ShowTicks="False">
    <!-- Gray background range -->
    <gauge:SfLinearGauge.Ranges>
        <gauge:LinearRange StartValue="0" 
                          EndValue="100"
                          StartWidth="15" 
                          EndWidth="15"
                          Fill="#E0E0E0"/>
    </gauge:SfLinearGauge.Ranges>
    
    <!-- Progress bar pointer -->
    <gauge:SfLinearGauge.BarPointers>
        <gauge:BarPointer Value="70" 
                         PointerSize="15"
                         Fill="#2196F3"/>
    </gauge:SfLinearGauge.BarPointers>
</gauge:SfLinearGauge>
```

### Pattern 2: Health Status Zones

```csharp
// Critical (Red)
gauge.Ranges.Add(new LinearRange
{
    StartValue = 0,
    EndValue = 20,
    Fill = new SolidColorBrush(Colors.Red)
});

// Warning (Orange)
gauge.Ranges.Add(new LinearRange
{
    StartValue = 20,
    EndValue = 40,
    Fill = new SolidColorBrush(Colors.Orange)
});

// Normal (Green)
gauge.Ranges.Add(new LinearRange
{
    StartValue = 40,
    EndValue = 100,
    Fill = new SolidColorBrush(Colors.Green)
});
```

### Pattern 3: Layered Ranges (Inside + Outside)

```xml
<gauge:SfLinearGauge>
    <!-- Outside background -->
    <gauge:SfLinearGauge.Ranges>
        <gauge:LinearRange StartValue="0" 
                          EndValue="100"
                          Position="Outside"
                          StartWidth="30"
                          EndWidth="30"
                          Fill="#E3F2FD"/>
        
        <!-- Inside accent -->
        <gauge:LinearRange StartValue="70" 
                          EndValue="100"
                          Position="Inside"
                          StartWidth="10"
                          EndWidth="10"
                          Fill="#1976D2"/>
    </gauge:SfLinearGauge.Ranges>
</gauge:SfLinearGauge>
```

### Pattern 4: Gradient Temperature Range

```csharp
var tempGradient = new ObservableCollection<GaugeGradientStop>
{
    new GaugeGradientStop { Value = -20, Color = Colors.Blue },
    new GaugeGradientStop { Value = 0, Color = Colors.Cyan },
    new GaugeGradientStop { Value = 15, Color = Colors.Green },
    new GaugeGradientStop { Value = 25, Color = Colors.Yellow },
    new GaugeGradientStop { Value = 40, Color = Colors.Orange },
    new GaugeGradientStop { Value = 50, Color = Colors.Red }
};

gauge.Ranges.Add(new LinearRange
{
    StartValue = -20,
    EndValue = 50,
    StartWidth = 25,
    EndWidth = 25,
    GradientStops = tempGradient
});
```

### Pattern 5: Segmented Progress with Labels

```csharp
string[] stages = { "Planning", "Development", "Testing", "Deployment" };
Color[] colors = { Colors.Blue, Colors.Purple, Colors.Orange, Colors.Green };

for (int i = 0; i < 4; i++)
{
    gauge.Ranges.Add(new LinearRange
    {
        StartValue = i * 25,
        EndValue = (i + 1) * 25,
        StartWidth = 40,
        EndWidth = 40,
        Fill = new SolidColorBrush(colors[i]),
        Child = new Label
        {
            Text = stages[i],
            TextColor = Colors.White,
            FontSize = 12,
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center
        }
    });
}
```
