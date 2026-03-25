# Track Customization

This guide covers how to customize the track appearance in the .NET MAUI Range Selector (SfRangeSelector), including minimum/maximum values, range values, track colors, height, and extent.

## Table of Contents
- [Overview](#overview)
- [Minimum and Maximum](#minimum-and-maximum)
- [Range Values](#range-values)
- [Track Colors](#track-colors)
- [Track Height](#track-height)
- [Track Extent](#track-extent)
- [Complete Examples](#complete-examples)
- [Troubleshooting](#troubleshooting)

## Overview

The track is the horizontal bar in the Range Selector where thumbs slide to select a range. The track has two distinct regions:
- **Active Region**: The area between the start and end thumbs
- **Inactive Region**: The areas from Minimum to start thumb, and end thumb to Maximum

You can customize both regions independently with different colors, sizes, and styles.

## Minimum and Maximum

The `Minimum` and `Maximum` properties define the value range of the Range Selector.

**Properties:**
- `Minimum` (double): The minimum value users can select. Default: `0.0`
- `Maximum` (double): The maximum value users can select. Default: `1.0`

**Rules:**
- `Minimum` must be less than `Maximum`
- Default range is 0 to 1

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="10"
                         Maximum="20"
                         RangeStart="13"
                         RangeEnd="17"
                         ShowLabels="True">
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
SfRangeSelector rangeSelector = new SfRangeSelector();
rangeSelector.Minimum = 10;
rangeSelector.Maximum = 20;
rangeSelector.RangeStart = 13;
rangeSelector.RangeEnd = 17;
rangeSelector.ShowLabels = true;
```

## Range Values

The `RangeStart` and `RangeEnd` properties represent the currently selected values. The thumbs are positioned based on these values.

**Properties:**
- `RangeStart` (double): The start value of the selected range. Default: `0.0`
- `RangeEnd` (double): The end value of the selected range. Default: `1.0`

**Rules:**
- `RangeStart` must be >= `Minimum` and < `RangeEnd`
- `RangeEnd` must be <= `Maximum` and > `RangeStart`
- Both values should be within the Minimum/Maximum bounds

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="0"
                         Maximum="100"
                         RangeStart="25"
                         RangeEnd="75"
                         ShowLabels="True">
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
SfRangeSelector rangeSelector = new SfRangeSelector
{
    Minimum = 0,
    Maximum = 100,
    RangeStart = 25,
    RangeEnd = 75,
    ShowLabels = true
};
```

**Data Binding:**
```xaml
<sliders:SfRangeSelector Minimum="0"
                         Maximum="100"
                         RangeStart="{Binding SelectedStart}"
                         RangeEnd="{Binding SelectedEnd, Mode=TwoWay}"
                         ShowLabels="True" />
```

## Track Colors

Customize the active and inactive track colors using the `TrackStyle` property.

**Properties:**
- `ActiveFill` (Brush): Color of the active region (between thumbs)
- `InactiveFill` (Brush): Color of the inactive regions (outside thumbs)

**Default Colors:**
- ActiveFill: Theme-dependent primary color
- InactiveFill: Theme-dependent lighter shade

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="100"
                         RangeStart="25" RangeEnd="75">
    <sliders:SfRangeSelector.TrackStyle>
        <sliders:SliderTrackStyle ActiveFill="#EE3F3F"
                                  InactiveFill="#F7B1AE" />
    </sliders:SfRangeSelector.TrackStyle>
    
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
SfRangeSelector rangeSelector = new SfRangeSelector
{
    Minimum = 0,
    Maximum = 100,
    RangeStart = 25,
    RangeEnd = 75
};

rangeSelector.TrackStyle = new SliderTrackStyle
{
    ActiveFill = new SolidColorBrush(Color.FromArgb("#EE3F3F")),
    InactiveFill = new SolidColorBrush(Color.FromArgb("#F7B1AE"))
};
```

**Visual Explanation:**
```
[Minimum]━━━━━━[RangeStart]▓▓▓▓▓▓[RangeEnd]━━━━━━[Maximum]
           ↑                  ↑                 ↑
      InactiveFill       ActiveFill        InactiveFill
```

## Track Height

Customize the track height (thickness) for both active and inactive regions.

**Properties:**
- `ActiveSize` (double): Height of the active region track. Default: `8.0`
- `InactiveSize` (double): Height of the inactive region track. Default: `6.0`

**Common Patterns:**
- **Emphasize Active**: ActiveSize > InactiveSize (default behavior)
- **Uniform Thickness**: ActiveSize == InactiveSize
- **Subtle Active**: ActiveSize < InactiveSize (uncommon)

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="100"
                         RangeStart="25" RangeEnd="75">
    <sliders:SfRangeSelector.TrackStyle>
        <sliders:SliderTrackStyle ActiveSize="10" InactiveSize="8" />
    </sliders:SfRangeSelector.TrackStyle>
    
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
SfRangeSelector rangeSelector = new SfRangeSelector
{
    Minimum = 0,
    Maximum = 100,
    RangeStart = 25,
    RangeEnd = 75
};

rangeSelector.TrackStyle = new SliderTrackStyle
{
    ActiveSize = 10,
    InactiveSize = 8
};
```

**Uniform Track Height:**
```xaml
<sliders:SfRangeSelector.TrackStyle>
    <sliders:SliderTrackStyle ActiveSize="8" InactiveSize="8" />
</sliders:SfRangeSelector.TrackStyle>
```

## Track Extent

The `TrackExtent` property extends the track beyond the minimum and maximum positions at both edges. This creates padding around the track.

**Property:**
- `TrackExtent` (double): Pixels to extend track at each edge. Default: `0.0`

**Use Cases:**
- Create visual breathing room
- Align track with other UI elements
- Ensure ticks don't clip at edges

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="1"
                         Maximum="7"
                         RangeStart="2"
                         RangeEnd="6"
                         Interval="2"
                         TrackExtent="25"
                         ShowTicks="True"
                         ShowLabels="True">
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
SfRangeSelector rangeSelector = new SfRangeSelector
{
    Minimum = 1,
    Maximum = 7,
    RangeStart = 2,
    RangeEnd = 6,
    Interval = 2,
    TrackExtent = 25,
    ShowTicks = true,
    ShowLabels = true
};
```

**Visual Comparison:**

**Without TrackExtent (default 0):**
```
|━━━━━━━━━━━━━━━━━━━━|
↑                     ↑
Min                   Max
```

**With TrackExtent = 25:**
```
    |━━━━━━━━━━━━━━━━━━━━|
    ↑                     ↑
    Min                   Max
←25px→                 ←25px→
```

## Complete Examples

### Basic Track with Custom Colors
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="100"
                         RangeStart="20" RangeEnd="80"
                         ShowLabels="True" ShowTicks="True">
    <sliders:SfRangeSelector.TrackStyle>
        <sliders:SliderTrackStyle ActiveFill="#2196F3"
                                  InactiveFill="#BBDEFB" />
    </sliders:SfRangeSelector.TrackStyle>
    
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

### Track with Custom Height and Colors
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="100"
                         RangeStart="25" RangeEnd="75"
                         ShowLabels="True" ShowTicks="True">
    <sliders:SfRangeSelector.TrackStyle>
        <sliders:SliderTrackStyle ActiveFill="#4CAF50"
                                  InactiveFill="#C8E6C9"
                                  ActiveSize="12"
                                  InactiveSize="8" />
    </sliders:SfRangeSelector.TrackStyle>
    
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

### Complete Customization with Extent
```xaml
<sliders:SfRangeSelector Minimum="0"
                         Maximum="10"
                         RangeStart="2"
                         RangeEnd="8"
                         Interval="2"
                         TrackExtent="20"
                         ShowLabels="True"
                         ShowTicks="True">
    <sliders:SfRangeSelector.TrackStyle>
        <sliders:SliderTrackStyle ActiveFill="#FF5722"
                                  InactiveFill="#FFCCBC"
                                  ActiveSize="10"
                                  InactiveSize="6" />
    </sliders:SfRangeSelector.TrackStyle>
    
    <charts:SfCartesianChart>
        <charts:SfCartesianChart.XAxes>
            <charts:DateTimeAxis IsVisible="False" ShowMajorGridLines="False" />
        </charts:SfCartesianChart.XAxes>
        <charts:SfCartesianChart.YAxes>
            <charts:NumericalAxis IsVisible="False" ShowMajorGridLines="False" />
        </charts:SfCartesianChart.YAxes>
        <charts:SfCartesianChart.Series>
            <charts:SplineAreaSeries ItemsSource="{Binding Data}"
                                     XBindingPath="X"
                                     YBindingPath="Y" />
        </charts:SfCartesianChart.Series>
    </charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

## Troubleshooting

### Track not visible
**Problem:** Track appears invisible or very thin.

**Solution:**
- Check that `ActiveSize` and `InactiveSize` are > 0
- Verify track colors contrast with background
- Ensure `Minimum` < `Maximum`

### Track colors not applying
**Problem:** Custom track colors not showing.

**Solution:**
```xaml
<!-- Correct: Use TrackStyle -->
<sliders:SfRangeSelector.TrackStyle>
    <sliders:SliderTrackStyle ActiveFill="#FF0000" />
</sliders:SfRangeSelector.TrackStyle>

<!-- Wrong: Direct property (doesn't exist) -->
<sliders:SfRangeSelector ActiveFill="#FF0000" />
```

### Track extent causing layout issues
**Problem:** Track extent pushes other elements.

**Solution:**
- Reduce `TrackExtent` value
- Add margin to parent container
- Use `HorizontalOptions="Center"` on RangeSelector

### RangeStart/RangeEnd not working
**Problem:** Setting RangeStart/RangeEnd has no effect.

**Solution:**
- Ensure values are within Minimum and Maximum bounds
- Check that RangeStart < RangeEnd
- Verify binding context if using data binding

```csharp
// Correct
rangeSelector.Minimum = 0;
rangeSelector.Maximum = 100;
rangeSelector.RangeStart = 25;  // Valid: 0 ≤ 25 < 75 ≤ 100
rangeSelector.RangeEnd = 75;

// Wrong
rangeSelector.RangeStart = 75;  // Invalid: RangeStart >= RangeEnd
rangeSelector.RangeEnd = 25;
```
