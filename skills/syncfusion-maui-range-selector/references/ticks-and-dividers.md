# Ticks and Dividers

This guide covers how to configure and customize ticks and dividers in the .NET MAUI Range Selector (SfRangeSelector).

## Table of Contents
- [Overview](#overview)
- [Major Ticks](#major-ticks)
- [Minor Ticks](#minor-ticks)
- [Tick Styling](#tick-styling)
- [Dividers](#dividers)
- [Divider Styling](#divider-styling)
- [Complete Examples](#complete-examples)

## Overview

**Ticks** are small marks on the track that indicate interval points. **Dividers** are circular marks that serve a similar purpose but with a different visual style.

- **Major Ticks**: Render at each `Interval` value
- **Minor Ticks**: Render between major ticks based on `MinorTicksPerInterval`
- **Dividers**: Circular marks at each interval (alternative to ticks)

## Major Ticks

Enable major ticks using the `ShowTicks` property. Ticks render at positions defined by the `Interval`.

**Property:**
- `ShowTicks` (bool): Enable/disable major ticks. Default: `false`

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="10"
                         Interval="2" ShowTicks="True">
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**With Interval (recommended):**
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="1"
                         Interval="0.2" ShowTicks="True">
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
<!-- Renders ticks at: 0, 0.2, 0.4, 0.6, 0.8, 1.0 -->
```

**Auto Interval:**
If `Interval="0"` and `ShowTicks="True"`, interval is calculated automatically based on available space.

## Minor Ticks

Add minor ticks between major ticks using the `MinorTicksPerInterval` property.

**Property:**
- `MinorTicksPerInterval` (int): Number of minor ticks between major ticks. Default: `0`

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="10"
                         Interval="2"
                         ShowTicks="True"
                         MinorTicksPerInterval="1">
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
<!-- Major ticks at: 0, 2, 4, 6, 8, 10 -->
<!-- Minor ticks at: 1, 3, 5, 7, 9 -->
```

**Multiple Minor Ticks:**
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="10"
                         Interval="5"
                         ShowTicks="True"
                         MinorTicksPerInterval="4">
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
<!-- Major: 0, 5, 10 -->
<!-- Minor: 1, 2, 3, 4, 6, 7, 8, 9 -->
```

## Tick Styling

Customize tick appearance using `MajorTickStyle` and `MinorTickStyle`.

**Properties (MajorTickStyle & MinorTickStyle):**
- `ActiveFill` (Brush): Color of ticks in active region
- `InactiveFill` (Brush): Color of ticks in inactive regions
- `ActiveSize` (Size): Size of active ticks. Default: `Size(2, 8)`
- `InactiveSize` (Size): Size of inactive ticks. Default: `Size(2, 8)`
- `Offset` (double): Distance from track. Default: `3.0`

### Major Tick Colors

**XAML:**
```xaml
<sliders:SfRangeSelector Interval="0.2" ShowTicks="True">
    <sliders:SfRangeSelector.MajorTickStyle>
        <sliders:SliderTickStyle ActiveFill="#EE3F3F"
                                 InactiveFill="#F7B1AE" />
    </sliders:SfRangeSelector.MajorTickStyle>
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
rangeSelector.MajorTickStyle = new SliderTickStyle
{
    ActiveFill = new SolidColorBrush(Color.FromArgb("#EE3F3F")),
    InactiveFill = new SolidColorBrush(Color.FromArgb("#F7B1AE"))
};
```

### Minor Tick Colors

**XAML:**
```xaml
<sliders:SfRangeSelector Interval="0.2" ShowTicks="True"
                         MinorTicksPerInterval="1">
    <sliders:SfRangeSelector.MinorTickStyle>
        <sliders:SliderTickStyle ActiveFill="#4CAF50"
                                 InactiveFill="#A5D6A7" />
    </sliders:SfRangeSelector.MinorTickStyle>
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

### Tick Size

**XAML:**
```xaml
<sliders:SfRangeSelector Interval="0.2" ShowTicks="True"
                         MinorTicksPerInterval="1">
    <sliders:SfRangeSelector.MajorTickStyle>
        <sliders:SliderTickStyle ActiveSize="2,15" InactiveSize="2,15" />
    </sliders:SfRangeSelector.MajorTickStyle>
    <sliders:SfRangeSelector.MinorTickStyle>
        <sliders:SliderTickStyle ActiveSize="2,10" InactiveSize="2,10" />
    </sliders:SfRangeSelector.MinorTickStyle>
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
rangeSelector.MajorTickStyle.ActiveSize = new Size(2, 15);
rangeSelector.MajorTickStyle.InactiveSize = new Size(2, 15);
rangeSelector.MinorTickStyle.ActiveSize = new Size(2, 10);
rangeSelector.MinorTickStyle.InactiveSize = new Size(2, 10);
```

### Tick Offset

**XAML:**
```xaml
<sliders:SfRangeSelector Interval="0.2" ShowTicks="True"
                         MinorTicksPerInterval="1">
    <sliders:SfRangeSelector.MajorTickStyle>
        <sliders:SliderTickStyle Offset="5" />
    </sliders:SfRangeSelector.MajorTickStyle>
    <sliders:SfRangeSelector.MinorTickStyle>
        <sliders:SliderTickStyle Offset="5" />
    </sliders:SfRangeSelector.MinorTickStyle>
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

## Dividers

Dividers are circular marks rendered at interval points, providing an alternative visual to ticks.

**Property:**
- `ShowDividers` (bool): Enable/disable dividers. Default: `false`

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="1"
                         Interval="0.2"
                         ShowDividers="True">
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
SfRangeSelector rangeSelector = new SfRangeSelector
{
    Minimum = 0,
    Maximum = 1,
    Interval = 0.2,
    ShowDividers = true
};
```

**Note:** Dividers render at the same positions as major ticks (based on `Interval`).

## Divider Styling

Customize divider appearance using the `DividerStyle` property.

**Properties:**
- `ActiveRadius` (double): Radius of active dividers. Default: `4.0`
- `InactiveRadius` (double): Radius of inactive dividers. Default: `4.0`
- `ActiveFill` (Brush): Fill color of active dividers
- `InactiveFill` (Brush): Fill color of inactive dividers
- `ActiveStroke` (Brush): Stroke color of active dividers
- `InactiveStroke` (Brush): Stroke color of inactive dividers
- `ActiveStrokeThickness` (double): Stroke width of active dividers. Default: `0`
- `InactiveStrokeThickness` (double): Stroke width of inactive dividers. Default: `0`

### Divider Radius

**XAML:**
```xaml
<sliders:SfRangeSelector Interval="0.2" ShowDividers="True">
    <sliders:SfRangeSelector.DividerStyle>
        <sliders:SliderDividerStyle ActiveRadius="5"
                                    InactiveRadius="3" />
    </sliders:SfRangeSelector.DividerStyle>
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

### Divider Colors

**XAML:**
```xaml
<sliders:SfRangeSelector Interval="0.2" ShowDividers="True">
    <sliders:SfRangeSelector.DividerStyle>
        <sliders:SliderDividerStyle ActiveRadius="7"
                                    InactiveRadius="7"
                                    ActiveFill="#EE3F3F"
                                    InactiveFill="#F7B1AE" />
    </sliders:SfRangeSelector.DividerStyle>
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
rangeSelector.DividerStyle = new SliderDividerStyle
{
    ActiveRadius = 7,
    InactiveRadius = 7,
    ActiveFill = new SolidColorBrush(Color.FromArgb("#EE3F3F")),
    InactiveFill = new SolidColorBrush(Color.FromArgb("#F7B1AE"))
};
```

### Divider Stroke

**XAML:**
```xaml
<sliders:SfRangeSelector Interval="0.2" ShowDividers="True">
    <sliders:SfRangeSelector.DividerStyle>
        <sliders:SliderDividerStyle ActiveRadius="7"
                                    InactiveRadius="7"
                                    ActiveFill="#2196F3"
                                    InactiveFill="#BBDEFB"
                                    ActiveStroke="#0D47A1"
                                    InactiveStroke="#64B5F6"
                                    ActiveStrokeThickness="2"
                                    InactiveStrokeThickness="1" />
    </sliders:SfRangeSelector.DividerStyle>
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

## Complete Examples

### Ticks with Custom Colors and Sizes
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="10"
                         RangeStart="2" RangeEnd="8"
                         Interval="2"
                         ShowTicks="True"
                         MinorTicksPerInterval="1"
                         ShowLabels="True">
    <sliders:SfRangeSelector.MajorTickStyle>
        <sliders:SliderTickStyle ActiveFill="#FF5722"
                                 InactiveFill="#FFCCBC"
                                 ActiveSize="3,12"
                                 InactiveSize="3,10"
                                 Offset="4" />
    </sliders:SfRangeSelector.MajorTickStyle>
    <sliders:SfRangeSelector.MinorTickStyle>
        <sliders:SliderTickStyle ActiveFill="#FF5722"
                                 InactiveFill="#FFCCBC"
                                 ActiveSize="2,8"
                                 InactiveSize="2,6"
                                 Offset="4" />
    </sliders:SfRangeSelector.MinorTickStyle>
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

### Dividers with Stroke
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="100"
                         RangeStart="25" RangeEnd="75"
                         Interval="25"
                         ShowDividers="True"
                         ShowLabels="True">
    <sliders:SfRangeSelector.DividerStyle>
        <sliders:SliderDividerStyle ActiveRadius="6"
                                    InactiveRadius="6"
                                    ActiveFill="#4CAF50"
                                    InactiveFill="#C8E6C9"
                                    ActiveStroke="#1B5E20"
                                    InactiveStroke="#81C784"
                                    ActiveStrokeThickness="2"
                                    InactiveStrokeThickness="1" />
    </sliders:SfRangeSelector.DividerStyle>
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

### Combined Ticks, Dividers, and Labels
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="100"
                         RangeStart="20" RangeEnd="80"
                         Interval="20"
                         ShowTicks="True"
                         MinorTicksPerInterval="1"
                         ShowDividers="True"
                         ShowLabels="True"
                         NumberFormat="$#">
    <sliders:SfRangeSelector.MajorTickStyle>
        <sliders:SliderTickStyle ActiveFill="#2196F3"
                                 InactiveFill="#90CAF9" />
    </sliders:SfRangeSelector.MajorTickStyle>
    <sliders:SfRangeSelector.MinorTickStyle>
        <sliders:SliderTickStyle ActiveFill="#2196F3"
                                 InactiveFill="#90CAF9" />
    </sliders:SfRangeSelector.MinorTickStyle>
    <sliders:SfRangeSelector.DividerStyle>
        <sliders:SliderDividerStyle ActiveRadius="5"
                                    InactiveRadius="5"
                                    ActiveFill="#FFFFFF"
                                    InactiveFill="#E3F2FD" />
    </sliders:SfRangeSelector.DividerStyle>
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```
