# Ticks and Dividers in DateTime Range Selector

## Table of Contents
- [Overview](#overview)
- [Ticks Configuration](#ticks-configuration)
- [Major Ticks](#major-ticks)
- [Minor Ticks](#minor-ticks)
- [Tick Styling](#tick-styling)
- [Dividers](#dividers)
- [Divider Styling](#divider-styling)
- [Common Patterns](#common-patterns)

## Overview

The DateTime Range Selector supports two types of visual indicators:
- **Ticks**: Small marks on the track at specified intervals
- **Dividers**: Vertical lines separating the track into sections

Both can be styled separately for active and inactive states.

## Ticks Configuration

Enable ticks using the `ShowTicks` property. The default value is `False`.

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2018-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2016-01-01"
                                 Interval="2" 
                                 ShowLabels="True"
                                 ShowTicks="True">
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

```csharp
rangeSelector.ShowTicks = true;
rangeSelector.Interval = 2;
```

## Major Ticks

Major ticks appear at primary intervals and are typically larger than minor ticks.

### Major Tick Size

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2018-01-01" 
                                 Interval="2" 
                                 ShowTicks="True"
                                 ShowLabels="True">
    <sliders:SfDateTimeRangeSelector.MajorTickStyle>
        <sliders:SliderTickStyle ActiveSize="10,3" 
                                InactiveSize="10,3" />
    </sliders:SfDateTimeRangeSelector.MajorTickStyle>
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

```csharp
rangeSelector.MajorTickStyle.ActiveSize = new Size(10, 3);
rangeSelector.MajorTickStyle.InactiveSize = new Size(10, 3);
```

**Size Format:** `Width, Height`
- **Width**: Thickness perpendicular to track
- **Height**: Length along track direction

**Default:** `2, 8`

### Major Tick Colors

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2018-01-01" 
                                 Interval="2" 
                                 ShowTicks="True"
                                 ShowLabels="True">
    <sliders:SfDateTimeRangeSelector.MajorTickStyle>
        <sliders:SliderTickStyle ActiveFill="#EE3F3F" 
                                InactiveFill="#F7B1AE" />
    </sliders:SfDateTimeRangeSelector.MajorTickStyle>
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

```csharp
rangeSelector.MajorTickStyle.ActiveFill = new SolidColorBrush(Color.FromArgb("#EE3F3F"));
rangeSelector.MajorTickStyle.InactiveFill = new SolidColorBrush(Color.FromArgb("#F7B1AE"));
```

### Major Tick Offset

Adjust the distance between major ticks and the track:

```xaml
<sliders:SfDateTimeRangeSelector.MajorTickStyle>
    <sliders:SliderTickStyle Offset="5" />
</sliders:SfDateTimeRangeSelector.MajorTickStyle>
```

```csharp
rangeSelector.MajorTickStyle.Offset = 5;
```

## Minor Ticks

Minor ticks provide finer granularity between major ticks.

### Enable Minor Ticks

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2014-01-01" 
                                 RangeStart="2011-01-01" 
                                 RangeEnd="2013-01-01"
                                 Interval="1" 
                                 MinorTicksPerInterval="1" 
                                 ShowTicks="True"
                                 ShowLabels="True">
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

```csharp
rangeSelector.MinorTicksPerInterval = 1;
rangeSelector.ShowTicks = true;
```

**Note:** `MinorTicksPerInterval` specifies how many minor ticks appear *between* major ticks.

### Minor Tick Interval Calculation

The minor tick interval is automatically calculated:

```
MinorTickInterval = Interval / (MinorTicksPerInterval + 1)
```

**Example:** 
- `Interval = 1 year`
- `MinorTicksPerInterval = 1`
- Result: 1 minor tick appears every 6 months

### Minor Tick Styling

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2014-01-01" 
                                 RangeStart="2011-01-01" 
                                 RangeEnd="2013-01-01"
                                 Interval="1" 
                                 MinorTicksPerInterval="1" 
                                 ShowTicks="True"
                                 ShowLabels="True">
    <sliders:SfDateTimeRangeSelector.MinorTickStyle>
        <sliders:SliderTickStyle ActiveSize="8,2" 
                                InactiveSize="8,2"
                                ActiveFill="#EE3F3F" 
                                InactiveFill="#F7B1AE" />
    </sliders:SfDateTimeRangeSelector.MinorTickStyle>
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

```csharp
rangeSelector.MinorTickStyle.ActiveSize = new Size(8, 2);
rangeSelector.MinorTickStyle.InactiveSize = new Size(8, 2);
rangeSelector.MinorTickStyle.ActiveFill = new SolidColorBrush(Color.FromArgb("#EE3F3F"));
rangeSelector.MinorTickStyle.InactiveFill = new SolidColorBrush(Color.FromArgb("#F7B1AE"));
```

### Minor Tick Offset

```xaml
<sliders:SfDateTimeRangeSelector.MinorTickStyle>
    <sliders:SliderTickStyle Offset="3" />
</sliders:SfDateTimeRangeSelector.MinorTickStyle>
```

```csharp
rangeSelector.MinorTickStyle.Offset = 3;
```

## Tick Styling

Complete example showing all tick styling options:

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2018-01-01" 
                                 Interval="2" 
                                 MinorTicksPerInterval="1"
                                 ShowTicks="True"
                                 ShowLabels="True">
    
    <!-- Major Tick Styling -->
    <sliders:SfDateTimeRangeSelector.MajorTickStyle>
        <sliders:SliderTickStyle ActiveSize="12,4" 
                                InactiveSize="12,4"
                                ActiveFill="#EE3F3F" 
                                InactiveFill="#CCCCCC"
                                Offset="5" />
    </sliders:SfDateTimeRangeSelector.MajorTickStyle>
    
    <!-- Minor Tick Styling -->
    <sliders:SfDateTimeRangeSelector.MinorTickStyle>
        <sliders:SliderTickStyle ActiveSize="8,2" 
                                InactiveSize="8,2"
                                ActiveFill="#F7B1AE" 
                                InactiveFill="#DDDDDD"
                                Offset="3" />
    </sliders:SfDateTimeRangeSelector.MinorTickStyle>
    
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

### Active vs Inactive States

- **Active ticks**: Between start and end thumbs
- **Inactive ticks**: Outside the selected range

## Dividers

Dividers are vertical lines that visually separate intervals on the track. They extend from the track to the labels.

### Enable Dividers

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2018-01-01" 
                                 Interval="2"
                                 ShowDividers="True"
                                 ShowLabels="True">
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

```csharp
rangeSelector.ShowDividers = true;
```

**Note:** Dividers appear at major tick positions.

### Dividers with Labels Between Ticks

When `LabelsPlacement="BetweenTicks"`, dividers align with labels for better visual grouping:

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2018-01-01" 
                                 Interval="2"
                                 ShowDividers="True"
                                 ShowLabels="True"
                                 LabelsPlacement="BetweenTicks">
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

## Divider Styling

### Divider Colors

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2018-01-01" 
                                 Interval="2"
                                 ShowDividers="True"
                                 ShowLabels="True">
    <sliders:SfDateTimeRangeSelector.DividerStyle>
        <sliders:SliderDividerStyle ActiveFill="#EE3F3F" 
                                   InactiveFill="#F7B1AE" />
    </sliders:SfDateTimeRangeSelector.DividerStyle>
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

```csharp
rangeSelector.DividerStyle.ActiveFill = new SolidColorBrush(Color.FromArgb("#EE3F3F"));
rangeSelector.DividerStyle.InactiveFill = new SolidColorBrush(Color.FromArgb("#F7B1AE"));
```

### Divider Thickness and Radius

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2018-01-01" 
                                 Interval="2"
                                 ShowDividers="True"
                                 ShowLabels="True">
    <sliders:SfDateTimeRangeSelector.DividerStyle>
        <sliders:SliderDividerStyle ActiveFill="#EE3F3F" 
                                   InactiveFill="#F7B1AE"
                                   ActiveStrokeThickness="2" 
                                   InactiveStrokeThickness="2"
                                   ActiveRadius="3" 
                                   InactiveRadius="3" />
    </sliders:SfDateTimeRangeSelector.DividerStyle>
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

```csharp
rangeSelector.DividerStyle.ActiveStrokeThickness = 2;
rangeSelector.DividerStyle.InactiveStrokeThickness = 2;
rangeSelector.DividerStyle.ActiveRadius = 3;
rangeSelector.DividerStyle.InactiveRadius = 3;
```

**Properties:**
- `ActiveStrokeThickness` / `InactiveStrokeThickness`: Line width (default: 1)
- `ActiveRadius` / `InactiveRadius`: Corner radius for rounded dividers (default: 0)

### Complete Divider Styling

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2018-01-01" 
                                 Interval="2"
                                 ShowDividers="True"
                                 ShowLabels="True">
    <sliders:SfDateTimeRangeSelector.DividerStyle>
        <sliders:SliderDividerStyle ActiveFill="#2196F3" 
                                   InactiveFill="#BDBDBD"
                                   ActiveStrokeThickness="3" 
                                   InactiveStrokeThickness="1"
                                   ActiveRadius="2" 
                                   InactiveRadius="0" />
    </sliders:SfDateTimeRangeSelector.DividerStyle>
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

## Common Patterns

### Pattern 1: Ticks Only

Clear visual indicators without dividers:

```xaml
<sliders:SfDateTimeRangeSelector ShowTicks="True" 
                                 ShowLabels="True"
                                 ShowDividers="False"
                                 Interval="1">
    <sliders:SfDateTimeRangeSelector.MajorTickStyle>
        <sliders:SliderTickStyle ActiveSize="10,3" 
                                ActiveFill="#2196F3" />
    </sliders:SfDateTimeRangeSelector.MajorTickStyle>
</sliders:SfDateTimeRangeSelector>
```

### Pattern 2: Dividers Only

Clean separation without ticks:

```xaml
<sliders:SfDateTimeRangeSelector ShowTicks="False" 
                                 ShowLabels="True"
                                 ShowDividers="True"
                                 Interval="2">
    <sliders:SfDateTimeRangeSelector.DividerStyle>
        <sliders:SliderDividerStyle ActiveFill="#2196F3" 
                                   ActiveStrokeThickness="2" />
    </sliders:SfDateTimeRangeSelector.DividerStyle>
</sliders:SfDateTimeRangeSelector>
```

### Pattern 3: Both Ticks and Dividers

Maximum visual clarity:

```xaml
<sliders:SfDateTimeRangeSelector ShowTicks="True" 
                                 ShowLabels="True"
                                 ShowDividers="True"
                                 Interval="2"
                                 MinorTicksPerInterval="1">
    <sliders:SfDateTimeRangeSelector.MajorTickStyle>
        <sliders:SliderTickStyle ActiveSize="12,4" 
                                ActiveFill="#2196F3" />
    </sliders:SfDateTimeRangeSelector.MajorTickStyle>
    <sliders:SfDateTimeRangeSelector.MinorTickStyle>
        <sliders:SliderTickStyle ActiveSize="8,2" 
                                ActiveFill="#64B5F6" />
    </sliders:SfDateTimeRangeSelector.MinorTickStyle>
    <sliders:SfDateTimeRangeSelector.DividerStyle>
        <sliders:SliderDividerStyle ActiveFill="#2196F3" 
                                   ActiveStrokeThickness="1" />
    </sliders:SfDateTimeRangeSelector.DividerStyle>
</sliders:SfDateTimeRangeSelector>
```

### Pattern 4: Minimal Style

Subtle indicators:

```xaml
<sliders:SfDateTimeRangeSelector ShowTicks="True" 
                                 ShowLabels="True"
                                 Interval="1">
    <sliders:SfDateTimeRangeSelector.MajorTickStyle>
        <sliders:SliderTickStyle ActiveSize="6,2" 
                                InactiveSize="6,2"
                                ActiveFill="#888888" 
                                InactiveFill="#CCCCCC" />
    </sliders:SfDateTimeRangeSelector.MajorTickStyle>
</sliders:SfDateTimeRangeSelector>
```

### Pattern 5: Yearly with Quarterly Minor Ticks

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2016-01-01" 
                                 RangeStart="2011-01-01" 
                                 RangeEnd="2015-01-01"
                                 Interval="1"
                                 IntervalType="Years"
                                 MinorTicksPerInterval="3"
                                 ShowTicks="True"
                                 ShowLabels="True">
    <sliders:SfDateTimeRangeSelector.MajorTickStyle>
        <sliders:SliderTickStyle ActiveSize="12,4" 
                                InactiveSize="12,4" />
    </sliders:SfDateTimeRangeSelector.MajorTickStyle>
    <sliders:SfDateTimeRangeSelector.MinorTickStyle>
        <sliders:SliderTickStyle ActiveSize="8,2" 
                                InactiveSize="8,2" />
    </sliders:SfDateTimeRangeSelector.MinorTickStyle>
</sliders:SfDateTimeRangeSelector>
```

**Result:** Major ticks at years, minor ticks at quarters (3-month intervals)

## Best Practices

1. **Size Hierarchy**: Make major ticks larger than minor ticks (e.g., major: 10x3, minor: 8x2)
2. **Color Contrast**: Use distinct colors for active vs inactive states
3. **Choose One or Both**: Use ticks OR dividers OR both based on visual preference
4. **Minor Tick Density**: Don't overcrowd - 1-3 minor ticks per interval is ideal
5. **Offset Spacing**: Adjust offsets to prevent overlap with labels
6. **Match Track Style**: Coordinate tick/divider colors with track colors

## Troubleshooting

**Issue:** Ticks not showing
- **Solution:** Ensure `ShowTicks="True"` and `Interval` is set

**Issue:** Minor ticks not appearing
- **Solution:** Set `MinorTicksPerInterval` to a value > 0 and ensure `ShowTicks="True"`

**Issue:** Dividers not visible
- **Solution:** Check `ShowDividers="True"` and divider colors contrast with background

**Issue:** Active/inactive colors not working
- **Solution:** Verify range is set properly (`RangeStart` and `RangeEnd` values)

**Issue:** Ticks overlap labels
- **Solution:** Increase `MajorTickStyle.Offset` or `MinorTickStyle.Offset`

**Issue:** Too many minor ticks
- **Solution:** Reduce `MinorTicksPerInterval` value

## Related Properties

- `ShowLabels` - Display text labels at intervals
- `Interval` - Spacing for major ticks/dividers
- `LabelsPlacement` - Affects divider alignment
- `TrackExtent` - Additional space for ticks/dividers
