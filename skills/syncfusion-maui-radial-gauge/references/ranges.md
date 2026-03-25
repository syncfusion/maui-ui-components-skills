# Ranges and Visual Indicators

## Table of Contents
- [Overview](#overview)
- [Creating a Range](#creating-a-range)
- [Start and End Values](#start-and-end-values)
- [Range Styling](#range-styling)
  - [Colors](#colors)
  - [Gradients](#gradients)
  - [Width Customization](#width-customization)
  - [Variable Width Ranges](#variable-width-ranges)
- [Range Positioning](#range-positioning)
  - [Offset (Pixel)](#offset-pixel)
  - [Offset (Factor)](#offset-factor)
  - [Inside vs Outside Axis](#inside-vs-outside-axis)
- [Multiple Ranges](#multiple-ranges)
- [Range Caps](#range-caps)
- [Reverse Order Ranges](#reverse-order-ranges-360-gauges)
- [Common Use Cases](#common-use-cases)
  - [Speedometer Zones](#speedometer-zones)
  - [Temperature Indicators](#temperature-indicators)
  - [Progress Indicators](#progress-indicators)
  - [Alert Thresholds](#alert-thresholds)
- [Best Practices](#best-practices)

## Overview

Radial ranges are colored arc segments that help users quickly visualize value zones on the gauge. They're perfect for showing safe/warning/danger zones, performance thresholds, or progress segments.

**Key Features:**
- Define custom value ranges (e.g., 0-50, 50-100)
- Apply solid colors or gradients
- Control width (uniform or tapered)
- Position inside/outside/on the axis
- Create multiple overlapping or adjacent ranges
- Rounded or flat end caps

## Creating a Range

Add ranges to the `RadialAxis.Ranges` collection:

```xaml
<gauge:RadialAxis Minimum="0"
                  Maximum="150"
                  Interval="10">
    <gauge:RadialAxis.Ranges>
        <gauge:RadialRange StartValue="30"
                           EndValue="65"
                           Fill="Orange" />
    </gauge:RadialAxis.Ranges>
</gauge:RadialAxis>
```

```csharp
RadialAxis axis = new RadialAxis
{
    Minimum = 0,
    Maximum = 150,
    Interval = 10
};

RadialRange range = new RadialRange
{
    StartValue = 30,
    EndValue = 65,
    Fill = new SolidColorBrush(Colors.Orange)
};

axis.Ranges.Add(range);
```

## Start and End Values

The `StartValue` and `EndValue` properties define the range boundaries on the axis scale.

```xaml
<gauge:RadialRange StartValue="0"
                   EndValue="50"
                   Fill="Green" />
```

```csharp
RadialRange range = new RadialRange
{
    StartValue = 0,
    EndValue = 50,
    Fill = new SolidColorBrush(Colors.Green)
};
```

**Important:** Start and end values must be within the axis Minimum and Maximum.

## Range Styling

### Colors

Set a solid color using the `Fill` property:

```xaml
<gauge:RadialRange StartValue="0"
                   EndValue="50"
                   Fill="Green" />
```

```csharp
range.Fill = new SolidColorBrush(Colors.Green);
// Or with hex
range.Fill = new SolidColorBrush(Color.FromHex("#00FF00"));
// Or with RGB
range.Fill = new SolidColorBrush(Color.FromRgb(0, 255, 0));
```

### Gradients

Create smooth color transitions using `GradientStops`:

```xaml
<gauge:RadialRange StartValue="30"
                   EndValue="65">
    <gauge:RadialRange.GradientStops>
        <gauge:GaugeGradientStop Value="30" Color="Yellow" />
        <gauge:GaugeGradientStop Value="50" Color="Orange" />
        <gauge:GaugeGradientStop Value="65" Color="Red" />
    </gauge:RadialRange.GradientStops>
</gauge:RadialRange>
```

```csharp
RadialRange range = new RadialRange
{
    StartValue = 30,
    EndValue = 65
};

range.GradientStops.Add(new GaugeGradientStop { Value = 30, Color = Colors.Yellow });
range.GradientStops.Add(new GaugeGradientStop { Value = 50, Color = Colors.Orange });
range.GradientStops.Add(new GaugeGradientStop { Value = 65, Color = Colors.Red });

axis.Ranges.Add(range);
```

**Gradient Values:** Must be within the range's StartValue and EndValue.

**Use Cases:**
- Temperature gradients (cold blue → warm red)
- Performance metrics (poor → excellent)
- Smooth warning transitions

### Width Customization

Control range thickness using `StartWidth` and `EndWidth`:

**Equal Width:**
```xaml
<gauge:RadialRange StartValue="30"
                   EndValue="65"
                   StartWidth="20"
                   EndWidth="20"
                   WidthUnit="Pixel" />
```

```csharp
range.StartWidth = 20;
range.EndWidth = 20;
range.WidthUnit = SizeUnit.Pixel;
```

**Factor-Based Width:**
```xaml
<gauge:RadialRange StartWidth="0.15"
                   EndWidth="0.15"
                   WidthUnit="Factor" />
```

**Factor Range:** 0 to 1 (0.15 = 15% of axis radius)

**Default Width:** 10 pixels

### Variable Width Ranges

Create tapered ranges with different start and end widths:

```xaml
<gauge:RadialRange StartValue="30"
                   EndValue="65"
                   StartWidth="5"
                   EndWidth="30"
                   WidthUnit="Pixel" />
```

```csharp
range.StartWidth = 5;
range.EndWidth = 30;
```

**Effect:** Range gradually widens from start to end.

**Use Cases:**
- Emphasize higher values
- Creative visual effects
- Funnel-style progress indicators

## Range Positioning

### Offset (Pixel)

Move ranges away from or toward the axis line:

**Outside (Positive Offset):**
```xaml
<gauge:RadialRange StartValue="30"
                   EndValue="65"
                   RangeOffset="50"
                   OffsetUnit="Pixel" />
```

**Inside (Negative Offset):**
```xaml
<gauge:RadialRange StartValue="30"
                   EndValue="65"
                   RangeOffset="-30"
                   OffsetUnit="Pixel" />
```

```csharp
range.RangeOffset = 50;  // Positive = outward
range.OffsetUnit = SizeUnit.Pixel;
```

**Default:** 0 (range positioned on axis line)

### Offset (Factor)

Position relative to axis radius:

```xaml
<gauge:RadialRange RangeOffset="0.2"
                   OffsetUnit="Factor" />
```

```csharp
range.RangeOffset = 0.2;  // 20% of radius outward
range.OffsetUnit = SizeUnit.Factor;
```

**Negative Factor:** Positions toward center

### Inside vs Outside Axis

**Range Outside Axis:**
```xaml
<gauge:RadialAxis>
    <gauge:RadialAxis.AxisLineStyle>
        <gauge:RadialLineStyle Thickness="3" Fill="Black" />
    </gauge:RadialAxis.AxisLineStyle>
    <gauge:RadialAxis.Ranges>
        <gauge:RadialRange StartValue="0"
                           EndValue="100"
                           RangeOffset="10"
                           Fill="LightBlue" />
    </gauge:RadialAxis.Ranges>
</gauge:RadialAxis>
```

**Range Inside Axis:**
```xaml
<gauge:RadialRange StartValue="0"
                   EndValue="100"
                   RangeOffset="-10"
                   Fill="LightBlue" />
```

**On Axis (Overlay):**
```xaml
<gauge:RadialRange StartValue="0"
                   EndValue="100"
                   RangeOffset="0"
                   StartWidth="10"
                   Fill="LightBlue" />
```

## Multiple Ranges

Add multiple ranges to show different zones:

```xaml
<gauge:RadialAxis Minimum="0"
                  Maximum="150"
                  Interval="10">
    <gauge:RadialAxis.Ranges>
        <!-- Safe zone -->
        <gauge:RadialRange StartValue="0"
                           EndValue="50"
                           Fill="Green" />
        
        <!-- Warning zone -->
        <gauge:RadialRange StartValue="50"
                           EndValue="100"
                           Fill="Yellow" />
        
        <!-- Danger zone -->
        <gauge:RadialRange StartValue="100"
                           EndValue="150"
                           Fill="Red" />
    </gauge:RadialAxis.Ranges>
</gauge:RadialAxis>
```

```csharp
axis.Ranges.Add(new RadialRange { StartValue = 0, EndValue = 50, Fill = new SolidColorBrush(Colors.Green) });
axis.Ranges.Add(new RadialRange { StartValue = 50, EndValue = 100, Fill = new SolidColorBrush(Colors.Yellow) });
axis.Ranges.Add(new RadialRange { StartValue = 100, EndValue = 150, Fill = new SolidColorBrush(Colors.Red) });
```

**Overlapping Ranges:**
```xaml
<!-- Outer range -->
<gauge:RadialRange StartValue="0"
                   EndValue="100"
                   StartWidth="30"
                   EndWidth="30"
                   Fill="LightGray"
                   RangeOffset="0" />

<!-- Inner range -->
<gauge:RadialRange StartValue="0"
                   EndValue="75"
                   StartWidth="20"
                   EndWidth="20"
                   Fill="Blue"
                   RangeOffset="-5" />
```

**Stacked Ranges (Different Radii):**
```xaml
<!-- Outer ring -->
<gauge:RadialRange StartValue="0"
                   EndValue="100"
                   RangeOffset="20"
                   Fill="Blue" />

<!-- Middle ring -->
<gauge:RadialRange StartValue="0"
                   EndValue="100"
                   RangeOffset="0"
                   Fill="Green" />

<!-- Inner ring -->
<gauge:RadialRange StartValue="0"
                   EndValue="100"
                   RangeOffset="-20"
                   Fill="Red" />
```

## Range Caps

Control the end shape of ranges:

**Rounded Caps (Default):**
```xaml
<gauge:RadialRange StartValue="30"
                   EndValue="65"
                   RangeCap="Round" />
```

**Flat Caps:**
```xaml
<gauge:RadialRange StartValue="30"
                   EndValue="65"
                   RangeCap="Flat" />
```

```csharp
range.RangeCap = RangeCap.Flat;  // or RangeCap.Round
```

**Use Cases:**
- **Round:** Smooth, modern appearance
- **Flat:** Clean alignment with ticks/labels

## Reverse Order Ranges (360° Gauges)

In a full 360° gauge (StartAngle=EndAngle), you can create ranges in reverse order by swapping start and end values:

```xaml
<gauge:RadialAxis StartAngle="270"
                  EndAngle="270"
                  Minimum="0"
                  Maximum="24"
                  Interval="6"
                  ShowFirstLabel="False">
    <gauge:RadialAxis.Ranges>
        <!-- Reverse range: 12 to 6 (going around) -->
        <gauge:RadialRange StartValue="12"
                           EndValue="6"
                           Fill="DarkGray" />
    </gauge:RadialAxis.Ranges>
</gauge:RadialAxis>
```

```csharp
RadialAxis axis = new RadialAxis
{
    StartAngle = 270,
    EndAngle = 270,  // Full circle
    Minimum = 0,
    Maximum = 24
};

// This range goes from 12, wrapping around to 6
axis.Ranges.Add(new RadialRange 
{ 
    StartValue = 12, 
    EndValue = 6,  // "Reverse" order
    Fill = new SolidColorBrush(Colors.DarkGray) 
});
```

**Use Cases:**
- Night/day indicators on 24-hour clock
- Sleep/wake cycles
- Shaded regions on circular timers

**Note:** Only works with 360° gauges (when StartAngle == EndAngle).

## Common Use Cases

### Speedometer Zones

```xaml
<gauge:RadialAxis Minimum="0"
                  Maximum="200"
                  Interval="20"
                  StartAngle="180"
                  EndAngle="0">
    <gauge:RadialAxis.Ranges>
        <!-- Eco zone -->
        <gauge:RadialRange StartValue="0"
                           EndValue="60"
                           Fill="#00C853"
                           StartWidth="15"
                           EndWidth="15" />
        
        <!-- Normal zone -->
        <gauge:RadialRange StartValue="60"
                           EndValue="120"
                           Fill="#FFD600"
                           StartWidth="15"
                           EndWidth="15" />
        
        <!-- Performance zone -->
        <gauge:RadialRange StartValue="120"
                           EndValue="160"
                           Fill="#FF6D00"
                           StartWidth="15"
                           EndWidth="15" />
        
        <!-- Danger zone -->
        <gauge:RadialRange StartValue="160"
                           EndValue="200"
                           Fill="#DD2C00"
                           StartWidth="15"
                           EndWidth="15" />
    </gauge:RadialAxis.Ranges>
</gauge:RadialAxis>
```

### Temperature Indicators

```xaml
<gauge:RadialAxis Minimum="-30"
                  Maximum="50"
                  Interval="10"
                  StartAngle="180"
                  EndAngle="0">
    <gauge:RadialAxis.Ranges>
        <!-- Cold (Blue gradient) -->
        <gauge:RadialRange StartValue="-30"
                           EndValue="0">
            <gauge:RadialRange.GradientStops>
                <gauge:GaugeGradientStop Value="-30" Color="#0D47A1" />
                <gauge:GaugeGradientStop Value="0" Color="#42A5F5" />
            </gauge:RadialRange.GradientStops>
        </gauge:RadialRange>
        
        <!-- Cool (Cyan to Green) -->
        <gauge:RadialRange StartValue="0"
                           EndValue="15">
            <gauge:RadialRange.GradientStops>
                <gauge:GaugeGradientStop Value="0" Color="#42A5F5" />
                <gauge:GaugeGradientStop Value="15" Color="#66BB6A" />
            </gauge:RadialRange.GradientStops>
        </gauge:RadialRange>
        
        <!-- Warm (Green to Yellow) -->
        <gauge:RadialRange StartValue="15"
                           EndValue="30">
            <gauge:RadialRange.GradientStops>
                <gauge:GaugeGradientStop Value="15" Color="#66BB6A" />
                <gauge:GaugeGradientStop Value="30" Color="#FDD835" />
            </gauge:RadialRange.GradientStops>
        </gauge:RadialRange>
        
        <!-- Hot (Yellow to Red) -->
        <gauge:RadialRange StartValue="30"
                           EndValue="50">
            <gauge:RadialRange.GradientStops>
                <gauge:GaugeGradientStop Value="30" Color="#FDD835" />
                <gauge:GaugeGradientStop Value="50" Color="#D32F2F" />
            </gauge:RadialRange.GradientStops>
        </gauge:RadialRange>
    </gauge:RadialAxis.Ranges>
</gauge:RadialAxis>
```

### Progress Indicators

```xaml
<!-- Background range (total) -->
<gauge:RadialRange StartValue="0"
                   EndValue="100"
                   Fill="LightGray"
                   StartWidth="20"
                   EndWidth="20" />

<!-- Progress range (completed) -->
<gauge:RadialRange StartValue="0"
                   EndValue="{Binding Progress}"
                   Fill="#2196F3"
                   StartWidth="20"
                   EndWidth="20" />
```

### Alert Thresholds

```xaml
<gauge:RadialAxis Minimum="0"
                  Maximum="100">
    <!-- Normal range (subtle) -->
    <gauge:RadialRange StartValue="0"
                       EndValue="80"
                       Fill="Transparent"
                       StartWidth="5" />
    
    <!-- Warning range (visible) -->
    <gauge:RadialRange StartValue="80"
                       EndValue="90"
                       Fill="Orange"
                       StartWidth="15"
                       EndWidth="15" />
    
    <!-- Critical range (prominent) -->
    <gauge:RadialRange StartValue="90"
                       EndValue="100"
                       Fill="Red"
                       StartWidth="20"
                       EndWidth="20" />
</gauge:RadialAxis>
```

## Best Practices

1. **Use 3-5 Ranges:** More than 5 ranges can be visually confusing
2. **Choose Distinct Colors:** Ensure ranges are easily distinguishable
3. **Align Boundaries:** Adjacent ranges should share boundaries (e.g., 0-50, 50-100)
4. **Match Width:** Use consistent widths unless emphasizing specific zones
5. **Consider Gradients:** For smooth transitions, use gradients instead of multiple ranges
6. **Test Contrast:** Ensure pointer visibility against range colors
7. **Document Meaning:** Make range colors intuitive (green=good, red=bad)
8. **Responsive Sizing:** Use factor-based widths for consistent appearance across sizes

**Performance:**
- Ranges are lightweight and don't impact performance
- Use as many as needed for clarity
- Gradients have minimal overhead

**Accessibility:**
- Don't rely solely on color (use labels/annotations too)
- Provide high contrast between ranges
- Consider color-blind friendly palettes

**Common Mistakes:**
- ❌ Gaps between ranges (users can't determine zone)
- ❌ Overlapping value ranges without clear purpose
- ❌ Too many narrow ranges (cluttered appearance)
- ❌ Range colors too similar to pointer color
- ❌ Forgetting to set StartWidth/EndWidth (may be too thin to see)

## Summary

Radial ranges provide visual context for gauge values:
- **Define zones** with StartValue/EndValue
- **Style with colors** or gradients
- **Control width** (uniform or tapered)
- **Position** inside/outside/on axis
- **Combine multiple** ranges for complex visualizations

Ranges work seamlessly with pointers, annotations, and animations to create intuitive, visually appealing gauges.
