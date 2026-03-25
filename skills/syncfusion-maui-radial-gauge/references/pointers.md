# Pointers (All Types)

## Table of Contents
- [Overview](#overview)
- [Pointer Types](#pointer-types)
- [Needle Pointer](#needle-pointer)
  - [Basic Needle](#basic-needle)
  - [Needle Customization](#needle-customization)
  - [Knob Styling](#knob-styling)
  - [Tail Customization](#tail-customization)
- [Shape Pointer (Marker)](#shape-pointer-marker)
  - [Shape Types](#shape-types)
  - [Shape Customization](#shape-customization)
  - [Shadow and Border](#shadow-and-border)
- [Content Pointer](#content-pointer)
  - [Adding Custom Views](#adding-custom-views)
  - [Text Content](#text-content)
  - [Image Content](#image-content)
  - [Complex Layouts](#complex-layouts)
- [Range Pointer](#range-pointer)
  - [Basic Range Pointer](#basic-range-pointer)
  - [Width and Styling](#width-and-styling)
  - [Gradient Range Pointer](#gradient-range-pointer)
- [Setting Pointer Values](#setting-pointer-values)
- [Multiple Pointers](#multiple-pointers)
- [Pointer Positioning (Offset)](#pointer-positioning-offset)
- [Choosing the Right Pointer Type](#choosing-the-right-pointer-type)
- [Common Patterns](#common-patterns)

## Overview

Pointers indicate the current value on a radial gauge. Syncfusion .NET MAUI Radial Gauge supports **four pointer types**, each suitable for different use cases:

1. **Needle Pointer** - Classic gauge needle (speedometer style)
2. **Shape Pointer** - Geometric markers (circle, diamond, triangle, etc.)
3. **Content Pointer** - Custom .NET MAUI views (text, images, complex layouts)
4. **Range Pointer** - Filled arc from start to current value

**All pointers share common properties:**
- `Value` - The value to indicate on the axis
- `Offset` / `OffsetUnit` - Position adjustment
- `EnableAnimation` - Smooth value transitions
- `AnimationDuration` / `AnimationEasing` - Animation behavior
- `IsInteractive` - Enable dragging

## Pointer Types

**Quick Reference:**

| Pointer Type | Best For | Visual Style |
|-------------|----------|--------------|
| **Needle** | Speedometers, gauges, dials | Classic needle with knob |
| **Shape** | Minimal designs, markers | Geometric symbol |
| **Content** | Custom designs, labels | Any MAUI view |
| **Range** | Progress, filled indicators | Colored arc |

## Needle Pointer

The classic gauge pointer consisting of a needle, knob (center), and optional tail.

### Basic Needle

```xaml
<gauge:RadialAxis>
    <gauge:RadialAxis.Pointers>
        <gauge:NeedlePointer Value="60" />
    </gauge:RadialAxis.Pointers>
</gauge:RadialAxis>
```

```csharp
NeedlePointer pointer = new NeedlePointer
{
    Value = 60
};

axis.Pointers.Add(pointer);
```

### Needle Customization

**Length:**
```xaml
<!-- Factor-based (0-1, relative to radius) -->
<gauge:NeedlePointer Value="60"
                     NeedleLength="0.8"
                     NeedleLengthUnit="Factor" />

<!-- Pixel-based (fixed size) -->
<gauge:NeedlePointer Value="60"
                     NeedleLength="130"
                     NeedleLengthUnit="Pixel" />
```

```csharp
pointer.NeedleLength = 0.8;
pointer.NeedleLengthUnit = SizeUnit.Factor;  // or SizeUnit.Pixel
```

**Default:** 0.6 (60% of radius)

**Width:**
```xaml
<gauge:NeedlePointer Value="60"
                     NeedleStartWidth="2"
                     NeedleEndWidth="8"
                     NeedleFill="DarkBlue" />
```

```csharp
pointer.NeedleStartWidth = 2;   // Width at knob
pointer.NeedleEndWidth = 8;     // Width at tip
pointer.NeedleFill = new SolidColorBrush(Colors.DarkBlue);
```

**Tapered Needle:** Different start/end widths create a pointed needle effect.

### Knob Styling

The knob is the center circle where the needle rotates.

```xaml
<gauge:NeedlePointer Value="60">
    <gauge:NeedlePointer KnobFill="DarkGray"
                         KnobRadius="0.06"
                         KnobStrokeThickness="0.02"
                         KnobStroke="Black" >
</gauge:NeedlePointer>
```

```csharp
NeedlePointer needlePointer = new NeedlePointer();
needlePointer.KnobRadius = 0.06;
needlePointer.KnobStrokeThickness = 0.02;
needlePointer.KnobFill = new SolidColorBrush(Colors.White);
needlePointer.KnobStroke = Colors.Black;
```

### Tail Customization

The tail extends from the knob in the opposite direction of the needle.

```xaml
<gauge:NeedlePointer Value="60"
                     TailLength="0.2"
                     TailLengthUnit="Factor"
                     TailWidth="5"
                     TailFill="Red" />
```

```csharp
pointer.TailLength = 0.2;
pointer.TailLengthUnit = SizeUnit.Factor;
pointer.TailWidth = 5;
pointer.TailFill = new SolidColorBrush(Colors.Red);
```

**Hide Tail:**
```xaml
<gauge:NeedlePointer TailLength="0" />
```

### Complete Needle Example

```xaml
<gauge:NeedlePointer Value="75"
                     NeedleLength="0.75"
                     NeedleLengthUnit="Factor"
                     NeedleStartWidth="3"
                     NeedleEndWidth="10"
                     NeedleFill="#D32F2F"
                     TailLength="0.15"
                     TailWidth="5"
                     TailFill="#D32F2F"
                     KnobFill="DarkGray"
                     KnobRadius="0.06"
                     KnobStrokeThickness="0.02"
                     KnobStroke="Black">
</gauge:NeedlePointer>
```

## Shape Pointer (Marker)

Geometric symbols placed at the value position on the axis.

### Shape Types

Available shapes:
- `Circle` (default)
- `Diamond`
- `Triangle`
- `InvertedTriangle`
- `Rectangle`

```xaml
<gauge:ShapePointer Value="60"
                    ShapeType="Diamond" />
```

```csharp
ShapePointer pointer = new ShapePointer
{
    Value = 60,
    ShapeType = ShapeType.Diamond
};
```

### Shape Customization

```xaml
<gauge:ShapePointer Value="60"
                    ShapeType="Circle"
                    ShapeHeight="25"
                    ShapeWidth="25"
                    Fill="CornflowerBlue"
                    Stroke="Navy"
                    BorderWidth="2" />
```

```csharp
pointer.ShapeHeight = 25;
pointer.ShapeWidth = 25;
pointer.Fill = new SolidColorBrush(Colors.CornflowerBlue);
pointer.Stroke = new SolidColorBrush(Colors.Navy);
pointer.BorderWidth = 2;
```

### Shadow and Border

```xaml
<gauge:ShapePointer Value="60"
                    ShapeType="Circle"
                    ShapeHeight="30"
                    ShapeWidth="30"
                    Fill="White"
                    Stroke="Black"
                    BorderWidth="3"
                    HasShadow="True" />
```

```csharp
pointer.HasShadow = true;
```

**Use Cases:**
- **Circle:** General indicators, clean minimal design
- **Triangle:** Directional pointers, arrow-style indicators
- **Diamond:** Distinctive marker, premium appearance
- **Rectangle:** Bold markers, high visibility

## Content Pointer

Display any .NET MAUI view as a pointer.

### Adding Custom Views

```xaml
<gauge:ContentPointer Value="45">
    <gauge:ContentPointer.Content>
        <Label Text="45"
               FontSize="20"
               FontAttributes="Bold"
               TextColor="Black" />
    </gauge:ContentPointer.Content>
</gauge:ContentPointer>
```

```csharp
ContentPointer pointer = new ContentPointer
{
    Value = 45,
    Content = new Label 
    { 
        Text = "45", 
        FontSize = 20, 
        FontAttributes = FontAttributes.Bold, 
        TextColor = Colors.Black 
    }
};
```

### Text Content

```xaml
<gauge:ContentPointer Value="75">
    <gauge:ContentPointer.Content>
        <Border BackgroundColor="White"
                Stroke="Black"
                StrokeThickness="2"
                Padding="8,4">
            <Label Text="75°C"
                   FontSize="16"
                   FontAttributes="Bold"
                   TextColor="Red" />
        </Border>
    </gauge:ContentPointer.Content>
</gauge:ContentPointer>
```

### Image Content

```xaml
<gauge:ContentPointer Value="50">
    <gauge:ContentPointer.Content>
        <Image Source="indicator_icon.png"
               HeightRequest="30"
               WidthRequest="30" />
    </gauge:ContentPointer.Content>
</gauge:ContentPointer>
```

### Complex Layouts

```xaml
<gauge:ContentPointer Value="45">
    <gauge:ContentPointer.Content>
        <Grid HeightRequest="50" WidthRequest="50">
            <RoundRectangle Fill="White"
                           CornerRadius="8"
                           Stroke="Black"
                           StrokeThickness="2" />
            <VerticalStackLayout Padding="5"
                                VerticalOptions="Center">
                <Image Source="sun_icon.png"
                       HeightRequest="20"
                       WidthRequest="20"
                       HorizontalOptions="Center" />
                <Label Text="45°F"
                       FontSize="10"
                       FontAttributes="Bold"
                       HorizontalOptions="Center"
                       TextColor="Black" />
            </VerticalStackLayout>
        </Grid>
    </gauge:ContentPointer.Content>
</gauge:ContentPointer>
```

```csharp
ContentPointer pointer = new ContentPointer { Value = 45 };

Grid grid = new Grid { HeightRequest = 50, WidthRequest = 50 };
grid.Children.Add(new RoundRectangle 
{ 
    Fill = new SolidColorBrush(Colors.White),
    CornerRadius = 8,
    Stroke = new SolidColorBrush(Colors.Black),
    StrokeThickness = 2
});

VerticalStackLayout layout = new VerticalStackLayout { Padding = 5 };
layout.Children.Add(new Image { Source = "sun_icon.png", HeightRequest = 20, WidthRequest = 20 });
layout.Children.Add(new Label { Text = "45°F", FontSize = 10, FontAttributes = FontAttributes.Bold });
grid.Children.Add(layout);

pointer.Content = grid;
```

**Use Cases:**
- Value labels with units
- Icon-based indicators
- Status badges
- Custom branding elements

## Range Pointer

A filled arc that extends from the axis minimum (or custom start) to the pointer value.

### Basic Range Pointer

```xaml
<gauge:RangePointer Value="60"
                    Fill="CornflowerBlue" />
```

```csharp
RangePointer pointer = new RangePointer
{
    Value = 60,
    Fill = new SolidColorBrush(Colors.CornflowerBlue)
};
```

### Width and Styling

```xaml
<!-- Pixel width -->
<gauge:RangePointer Value="60"
                    PointerWidth="20"
                    WidthUnit="Pixel"
                    Fill="Green" />

<!-- Factor width (relative to radius) -->
<gauge:RangePointer Value="60"
                    PointerWidth="0.15"
                    WidthUnit="Factor"
                    Fill="Green" />
```

```csharp
pointer.PointerWidth = 0.15;
pointer.WidthUnit = SizeUnit.Factor;
pointer.Fill = new SolidColorBrush(Colors.Green);
```

**Rounded Caps:**
```xaml
<gauge:RangePointer Value="60"
                    CornerStyle="BothCurve" />
<!-- Options: BothFlat, StartCurve, EndCurve, BothCurve -->
```

### Gradient Range Pointer

```xaml
<gauge:RangePointer Value="75"
                    PointerWidth="20">
    <gauge:RangePointer.GradientStops>
        <gauge:GaugeGradientStop Value="0" Color="Green" />
        <gauge:GaugeGradientStop Value="50" Color="Yellow" />
        <gauge:GaugeGradientStop Value="75" Color="Red" />
    </gauge:RangePointer.GradientStops>
</gauge:RangePointer>
```

```csharp
RangePointer pointer = new RangePointer { Value = 75, PointerWidth = 20 };
pointer.GradientStops.Add(new GaugeGradientStop { Value = 0, Color = Colors.Green });
pointer.GradientStops.Add(new GaugeGradientStop { Value = 50, Color = Colors.Yellow });
pointer.GradientStops.Add(new GaugeGradientStop { Value = 75, Color = Colors.Red });
```

**Use Cases:**
- Progress indicators
- Completion meters
- Filled gauges
- Circular progress bars

## Setting Pointer Values

All pointers use the `Value` property:

```xaml
<gauge:NeedlePointer Value="75" />
```

```csharp
pointer.Value = 75;
```

**Binding to ViewModel:**
```xaml
<gauge:NeedlePointer Value="{Binding CurrentSpeed}" />
```

**Dynamic Updates:**
```csharp
// Update pointer value at runtime
pointer.Value = newValue;  // Will animate if EnableAnimation is true
```

## Multiple Pointers

Add multiple pointers to display different values or layers:

```xaml
<gauge:RadialAxis>
    <gauge:RadialAxis.Pointers>
        <!-- Background range -->
        <gauge:RangePointer Value="100"
                            PointerWidth="15"
                            Fill="LightGray" />
        
        <!-- Progress range -->
        <gauge:RangePointer Value="60"
                            PointerWidth="15"
                            Fill="Green" />
        
        <!-- Needle indicator -->
        <gauge:NeedlePointer Value="60"
                             NeedleFill="DarkGreen" />
        
        <!-- Value label -->
        <gauge:ContentPointer Value="60">
            <gauge:ContentPointer.Content>
                <Label Text="60" FontSize="20" FontAttributes="Bold" />
            </gauge:ContentPointer.Content>
        </gauge:ContentPointer>
    </gauge:RadialAxis.Pointers>
</gauge:RadialAxis>
```

**Multi-Metric Example:**
```xaml
<!-- Outer metric -->
<gauge:NeedlePointer Value="75"
                     Offset="0"
                     NeedleFill="Blue" />

<!-- Inner metric -->
<gauge:NeedlePointer Value="50"
                     Offset="-30"
                     NeedleFill="Red" />
```

## Pointer Positioning (Offset)

Adjust pointer position relative to the axis:

**Positive Offset (Outward):**
```xaml
<gauge:ShapePointer Value="60"
                    Offset="30"
                    OffsetUnit="Pixel" />
```

**Negative Offset (Inward):**
```xaml
<gauge:ShapePointer Value="60"
                    Offset="-20"
                    OffsetUnit="Pixel" />
```

**Factor-Based:**
```xaml
<gauge:ShapePointer Offset="0.1"
                    OffsetUnit="Factor" />
```

```csharp
pointer.Offset = 0.1;
pointer.OffsetUnit = SizeUnit.Factor;
```

**Use Cases:**
- Position multiple pointers at different radii
- Align pointers with specific ranges
- Create layered gauge designs

## Choosing the Right Pointer Type

| Scenario | Recommended Pointer |
|----------|-------------------|
| Traditional gauge (speedometer, tachometer) | **Needle** |
| Minimal, modern design | **Shape** (Circle or Diamond) |
| Progress/completion | **Range** |
| Show value with units | **Content** (Label) |
| Icon-based indicator | **Content** (Image) |
| Multiple related values | **Multiple Needles** at different offsets |
| Filled circular progress | **Range** + hide axis line |

## Common Patterns

### Pattern 1: Speedometer with Needle
```xaml
<gauge:NeedlePointer Value="85"
                     NeedleLength="0.7"
                     NeedleStartWidth="2"
                     NeedleEndWidth="10"
                     NeedleFill="#D32F2F"
                     EnableAnimation="True"
                     KnobFill="DarkGray"
                     KnobRadius="0.06"
                     KnobStrokeThickness="0.02"
                     KnobStroke="Black">
</gauge:NeedlePointer>
```

### Pattern 2: Progress Indicator
```xaml
<!-- Background (total) -->
<gauge:RangePointer Value="100"
                    PointerWidth="20"
                    Fill="LightGray" />

<!-- Progress (completed) -->
<gauge:RangePointer Value="{Binding ProgressValue}"
                    PointerWidth="20"
                    Fill="#4CAF50"
                    CornerStyle="BothCurve" />
```

### Pattern 3: Temperature with Icon
```xaml
<gauge:ContentPointer Value="{Binding Temperature}">
    <gauge:ContentPointer.Content>
        <Grid>
            <Ellipse Fill="White"
                     Stroke="Black"
                     StrokeThickness="2"
                     HeightRequest="40"
                     WidthRequest="40" />
            <Image Source="temperature_icon.png"
                   HeightRequest="24"
                   WidthRequest="24"
                   HorizontalOptions="Center"
                   VerticalOptions="Center" />
        </Grid>
    </gauge:ContentPointer.Content>
</gauge:ContentPointer>
```

### Pattern 4: Multi-Value Dashboard
```xaml
<!-- Primary metric (outer) -->
<gauge:NeedlePointer Value="75"
                     Offset="0"
                     NeedleFill="Blue"
                     NeedleLength="0.8" />

<!-- Secondary metric (inner) -->
<gauge:NeedlePointer Value="50"
                     Offset="-40"
                     NeedleFill="Green"
                     NeedleLength="0.6" />

<!-- Labels -->
<gauge:ContentPointer Value="75" Offset="60">
    <gauge:ContentPointer.Content>
        <Label Text="75%" TextColor="Blue" />
    </gauge:ContentPointer.Content>
</gauge:ContentPointer>

<gauge:ContentPointer Value="50" Offset="-80">
    <gauge:ContentPointer.Content>
        <Label Text="50°" TextColor="Green" />
    </gauge:ContentPointer.Content>
</gauge:ContentPointer>
```

## Summary

**4 Pointer Types:**
1. **Needle** - Classic gauge with customizable needle, knob, and tail
2. **Shape** - Geometric markers (5 shapes available)
3. **Content** - Any .NET MAUI view (limitless customization)
4. **Range** - Filled arc pointer (progress style)

**All pointers support:**
- Value positioning on axis
- Offset adjustment (pixel or factor)
- Animation (see animation-and-interaction.md)
- Interactive dragging (see animation-and-interaction.md)
- Multiple pointers per axis

Choose the pointer type that best matches your design and user experience goals. Combine multiple pointer types for rich, informative visualizations.
