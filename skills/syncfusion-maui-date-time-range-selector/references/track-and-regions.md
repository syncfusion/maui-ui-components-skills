# Track and Regions in DateTime Range Selector

## Table of Contents
- [Overview](#overview)
- [Track Styling](#track-styling)
- [Track Colors](#track-colors)
- [Track Height](#track-height)
- [Track Extent](#track-extent)
- [Track Corner Radius](#track-corner-radius)
- [Active and Inactive Tracks](#active-and-inactive-tracks)
- [Regions](#regions)
- [Region Styling](#region-styling)
- [Common Patterns](#common-patterns)

## Overview

The track is the horizontal bar on which thumbs slide. It has two visual states:
- **Active track**: The portion between start and end thumbs
- **Inactive track**: The portions outside the selected range


## Track Styling

Customize track appearance using the `TrackStyle` property.

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2018-01-01">
    <sliders:SfDateTimeRangeSelector.TrackStyle>
        <sliders:SliderTrackStyle ActiveFill="#EE3F3F" 
                                 InactiveFill="#F7B1AE" />
    </sliders:SfDateTimeRangeSelector.TrackStyle>
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

```csharp
rangeSelector.TrackStyle.ActiveFill = new SolidColorBrush(Color.FromArgb("#EE3F3F"));
rangeSelector.TrackStyle.InactiveFill = new SolidColorBrush(Color.FromArgb("#F7B1AE"));
```

## Track Colors

### Solid Colors

```xaml
<sliders:SfDateTimeRangeSelector.TrackStyle>
    <sliders:SliderTrackStyle ActiveFill="#2196F3" 
                             InactiveFill="#E0E0E0" />
</sliders:SfDateTimeRangeSelector.TrackStyle>
```

```csharp
rangeSelector.TrackStyle.ActiveFill = new SolidColorBrush(Colors.Blue);
rangeSelector.TrackStyle.InactiveFill = new SolidColorBrush(Colors.LightGray);
```

### Gradient Colors

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2018-01-01">
    <sliders:SfDateTimeRangeSelector.TrackStyle>
        <sliders:SliderTrackStyle ActiveFill="{StaticResource ActiveGradient}" 
                                 InactiveFill="{StaticResource InactiveGradient}" />
    </sliders:SfDateTimeRangeSelector.TrackStyle>
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

**Define gradients in ResourceDictionary:**

```xaml
<ContentPage.Resources>
    <LinearGradientBrush x:Key="ActiveGradient" StartPoint="0,0" EndPoint="1,0">
        <GradientStop Color="#FF6B6B" Offset="0.0" />
        <GradientStop Color="#4ECDC4" Offset="1.0" />
    </LinearGradientBrush>
    <LinearGradientBrush x:Key="InactiveGradient" StartPoint="0,0" EndPoint="1,0">
        <GradientStop Color="#E0E0E0" Offset="0.0" />
        <GradientStop Color="#BDBDBD" Offset="1.0" />
    </LinearGradientBrush>
</ContentPage.Resources>
```

```csharp
var activeGradient = new LinearGradientBrush
{
    StartPoint = new Point(0, 0),
    EndPoint = new Point(1, 0),
    GradientStops = new GradientStopCollection
    {
        new GradientStop { Color = Color.FromArgb("#FF6B6B"), Offset = 0.0f },
        new GradientStop { Color = Color.FromArgb("#4ECDC4"), Offset = 1.0f }
    }
};
rangeSelector.TrackStyle.ActiveFill = activeGradient;
```

## Track Height

Control track thickness using the `ActiveSize` and `InactiveSize` properties.

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2018-01-01">
    <sliders:SfDateTimeRangeSelector.TrackStyle>
        <sliders:SliderTrackStyle ActiveSize="10" 
                                 InactiveSize="8" />
    </sliders:SfDateTimeRangeSelector.TrackStyle>
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

```csharp
rangeSelector.TrackStyle.ActiveSize = 10;
rangeSelector.TrackStyle.InactiveSize = 8;
```

**Default:** Both are `8.0`

## Track Extent

Add extra space above the track for labels, ticks, and dividers using the `TrackExtent` property.

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2018-01-01"
                                 TrackExtent="40"
                                 ShowLabels="True"
                                 ShowTicks="True">
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

```csharp
rangeSelector.TrackExtent = 40;
```

**Default:** `0.0`

**Use Cases:**
- Prevent labels from being cut off at top
- Add space for custom content above track
- Improve visual spacing in dense layouts

## Track Corner Radius

Round the track corners using `ActiveRadius` and `InactiveRadius`.

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2018-01-01">
    <sliders:SfDateTimeRangeSelector.TrackStyle>
        <sliders:SliderTrackStyle ActiveRadius="5" 
                                 InactiveRadius="5"
                                 ActiveSize="10"
                                 InactiveSize="10" />
    </sliders:SfDateTimeRangeSelector.TrackStyle>
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

```csharp
rangeSelector.TrackStyle.ActiveRadius = 5;
rangeSelector.TrackStyle.InactiveRadius = 5;
```

**Default:** Both are `0.0` (square corners)

## Active and Inactive Tracks

### Complete Track Styling Example

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2018-01-01"
                                 TrackExtent="30"
                                 ShowLabels="True"
                                 ShowTicks="True">
    <sliders:SfDateTimeRangeSelector.TrackStyle>
        <sliders:SliderTrackStyle ActiveFill="#2196F3" 
                                 InactiveFill="#E0E0E0"
                                 ActiveSize="12" 
                                 InactiveSize="10"
                                 ActiveRadius="6" 
                                 InactiveRadius="5" />
    </sliders:SfDateTimeRangeSelector.TrackStyle>
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

**Effect:** Active track is slightly larger and more prominent than inactive tracks.

## Regions

The `SfDateTimeRangeSelector` (via its `RangeSelectorBase`) provides built-in region styling properties to customize the active and inactive regions of the selector. The active region is the area between the start and end thumbs; the inactive regions are the areas from `Minimum` to the left thumb and from the right thumb to `Maximum`.

## Region Styling

### Region color

Set the region fill brushes with `ActiveRegionFill` and `InactiveRegionFill`. Defaults:
- `ActiveRegionFill` — `SolidColorBrush(Colors.Transparent)`
- `InactiveRegionFill` — `SolidColorBrush(Color.FromRgba(255, 255, 255, 192))`

XAML example:

```xaml
<ContentPage xmlns:sliders="clr-namespace:Syncfusion.Maui.Sliders;assembly=Syncfusion.Maui.Sliders">
    <sliders:SfDateTimeRangeSelector Minimum="2000-01-01"
                                     Maximum="2005-01-01"
                                     RangeStart="2001-01-01"
                                     RangeEnd="2004-01-01"
                                     ActiveRegionFill="#40FFFF00"
                                     InactiveRegionFill="#33FF8A00">
    </sliders:SfDateTimeRangeSelector>
</ContentPage>
```

### Region stroke

Control region borders with `ActiveRegionStroke` and `InactiveRegionStroke`. Defaults are transparent brushes.

XAML example with strokes:

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2000-01-01"
                                 Maximum="2005-01-01"
                                 RangeStart="2001-01-01"
                                 RangeEnd="2004-01-01"
                                 ActiveRegionFill="#40FFFF00"
                                 InactiveRegionFill="#33FF8A00"
                                 ActiveRegionStroke="#FFFF00"
                                 InactiveRegionStroke="#B8860B">
</sliders:SfDateTimeRangeSelector>
```

### Region stroke thickness

Use `ActiveRegionStrokeThickness` and `InactiveRegionStrokeThickness` (type `Thickness`) to set border widths. Default is `Thickness(1)`.

XAML example:

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2000-01-01"
                                 Maximum="2005-01-01"
                                 RangeStart="2001-01-01"
                                 RangeEnd="2004-01-01"
                                 ActiveRegionFill="#40FFFF00"
                                 InactiveRegionFill="#33FF8A00"
                                 ActiveRegionStroke="#A52A2A"
                                 InactiveRegionStroke="#A52A2A"
                                 ActiveRegionStrokeThickness="3,0,3,0"
                                 InactiveRegionStrokeThickness="0,3,0,3">
</sliders:SfDateTimeRangeSelector>
```

These built-in properties are the recommended way to style active/inactive zones when using Syncfusion's control. If you need more complex, overlapping or labeled zones beyond what these properties provide, consider the overlay techniques described below.

## Custom Overlays (Regions alternative)

Note: For cases where you need multiple, overlapping, or labeled bands beyond the active/inactive styling, you can use overlays or drawing to highlight arbitrary ranges. The pattern below shows a simple, robust alternative.

Approach:
- Convert date/time to a normalized position: `(value - Minimum) / (Maximum - Minimum)`.
- Multiply by the control `Width` to compute pixel offsets.
- Position one or more overlay elements (`BoxView`, shapes, or a `GraphicsView`) behind or above the selector.

Example (XAML):

```xaml
<Grid>
  <BoxView x:Name="RegionOverlay" BackgroundColor="#40FFCDD2" HorizontalOptions="Start" VerticalOptions="Fill" />
  <sliders:SfDateTimeRangeSelector x:Name="rangeSelector" Minimum="2020-01-01" Maximum="2024-01-01" />
</Grid>
```

Example (code-behind):

```csharp
void UpdateOverlay(DateTime start, DateTime end)
{
    var min = rangeSelector.Minimum;
    var max = rangeSelector.Maximum;
    var totalMs = (max - min).TotalMilliseconds;
    if (totalMs <= 0) return;

    var startOffset = (start - min).TotalMilliseconds / totalMs;
    var endOffset = (end - min).TotalMilliseconds / totalMs;

    var controlWidth = rangeSelector.Width;
    if (double.IsNaN(controlWidth) || controlWidth <= 0) return;

    var left = controlWidth * startOffset;
    var width = Math.Max(1, controlWidth * (endOffset - startOffset));

    RegionOverlay.Margin = new Thickness(left, 0, 0, 0);
    RegionOverlay.WidthRequest = width;
}

// Call UpdateOverlay from SizeChanged/layout events and when date values change.
```

Tips:
- For multiple highlighted ranges, use multiple overlay elements or draw them on a `Canvas`/`GraphicsView`.
- Use semi-transparent colors so labels/ticks remain readable.
- For pixel-perfect integration, consider a custom handler/renderer or draw directly into the control's visual layer.

## Common Patterns

### Pattern 1: Standard Track

Clean, simple track styling:

```xaml
<sliders:SfDateTimeRangeSelector.TrackStyle>
    <sliders:SliderTrackStyle ActiveFill="#2196F3" 
                             InactiveFill="#E0E0E0"
                             ActiveSize="10" 
                             InactiveSize="10"
                             ActiveRadius="5" 
                             InactiveRadius="5" />
</sliders:SfDateTimeRangeSelector.TrackStyle>
```

### Pattern 2: Emphasized Active Track

Make selected range stand out:

```xaml
<sliders:SfDateTimeRangeSelector.TrackStyle>
    <sliders:SliderTrackStyle ActiveFill="#FF5722" 
                             InactiveFill="#F5F5F5"
                             ActiveSize="14" 
                             InactiveSize="8"
                             ActiveRadius="7" 
                             InactiveRadius="4" />
</sliders:SfDateTimeRangeSelector.TrackStyle>
```

### Pattern 3: Gradient Track

Modern gradient appearance:

```xaml
<sliders:SfDateTimeRangeSelector.TrackStyle>
    <sliders:SliderTrackStyle ActiveFill="{StaticResource BlueGradient}" 
                             InactiveFill="#E0E0E0"
                             ActiveSize="12" 
                             InactiveSize="12"
                             ActiveRadius="6" 
                             InactiveRadius="6" />
</sliders:SfDateTimeRangeSelector.TrackStyle>
```

### Pattern 4: Performance Zones
Use overlays (see "Custom Overlays" section) to color-code performance periods. Place semi-transparent `BoxView` elements and position/size them with the `UpdateOverlay` technique shown earlier.

### Pattern 5: Seasonal Regions
Highlight seasonal periods using overlay elements or a drawing surface; compute each season's left/width via date→position math and render semi-transparent bands.

### Pattern 6: Business Quarters
Use overlays to mark business quarters; compute each quarter's position and render overlay bands programmatically.

## Best Practices

1. **Color Contrast**: Ensure active track color contrasts well with inactive track
2. **Size Consistency**: Keep active and inactive track sizes close (within 2-4 pixels)
3. **Track Extent**: Use when labels/ticks are cut off at top
4. **Overlay Colors**: Use light, semi-transparent colors for overlays to avoid overwhelming the chart
5. **Overlay Labels**: Keep text short (1-2 words) and ensure contrast with the overlay color
6. **Non-Overlapping Overlays**: Ensure overlay ranges don't overlap
7. **Gradient Performance**: Use solid colors for better performance on low-end devices

## Troubleshooting

**Issue:** Track not visible
- **Solution:** Check `TrackStyle.ActiveFill` and `InactiveFill` contrast with background

**Issue:** Active/inactive colors look the same
- **Solution:** Verify `RangeStart` and `RangeEnd` are different from `Minimum`/`Maximum`

**Issue:** Labels cut off at top
- **Solution:** Increase `TrackExtent` property (e.g., 30-40)

**Issue:** Track corners not rounded
- **Solution:** Ensure both `ActiveRadius` and `InactiveRadius` are > 0

## Related Properties

- `ShowLabels` - Display labels above track
- `ShowTicks` - Display tick marks
- `ShowDividers` - Display dividers between intervals
- `Interval` - Spacing for labels/ticks
- `ThumbStyle` - Style the draggable thumbs
