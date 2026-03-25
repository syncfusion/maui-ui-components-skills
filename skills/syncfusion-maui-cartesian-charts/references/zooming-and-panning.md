# Zooming and Panning

## Table of Contents
- [Overview](#overview)
- [Enable Zooming](#enable-zooming)
- [Zoom Modes](#zoom-modes)
- [Pinch Zooming](#pinch-zooming)
- [Selection Zooming](#selection-zooming)
- [Panning](#panning)
- [Zoom Reset](#zoom-reset)
- [Best Practices](#best-practices)

## Overview

Zooming and panning enable users to explore large datasets by focusing on specific data regions. Syncfusion .NET MAUI Chart provides multiple zooming options for interactive data exploration.

## Enable Zooming

### Basic Zooming Setup

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.ZoomPanBehavior>
        <chart:ChartZoomPanBehavior/>
    </chart:SfCartesianChart.ZoomPanBehavior>
    
    <chart:SfCartesianChart.XAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:LineSeries ItemsSource="{Binding Data}"
                     XBindingPath="X"
                     YBindingPath="Y"/>
</chart:SfCartesianChart>
```

```csharp
ChartZoomPanBehavior zoomPan = new ChartZoomPanBehavior()
{
    EnablePanning = true
};

SfCartesianChart chart = new SfCartesianChart();
chart.ZoomPanBehavior = zoomPan;
```

## Zoom Modes

### XY Zooming (Default)

Zoom both axes simultaneously:

```xml
<chart:ChartZoomPanBehavior ZoomMode="XY"/>
```

### X-Axis Only Zooming

Zoom only horizontally:

```xml
<chart:ChartZoomPanBehavior ZoomMode="X"/>
```

### Y-Axis Only Zooming

Zoom only vertically:

```xml
<chart:ChartZoomPanBehavior ZoomMode="Y"/>
```

```csharp
ChartZoomPanBehavior zoomPan = new ChartZoomPanBehavior()
{
    ZoomMode = ZoomMode.Y  // or ZoomMode.X, ZoomMode.XY
};
```

## Pinch Zooming

Enable touch-based pinch zooming:

```xml
<chart:ChartZoomPanBehavior EnablePinchZooming="True"/>
```

```csharp
ChartZoomPanBehavior zoomPan = new ChartZoomPanBehavior()
{
    EnablePinchZooming = true
};
```

## Selection Zooming

### Enable Selection Zoom

```xml
<chart:ChartZoomPanBehavior EnableSelectionZooming="True"/>
```

```csharp
ChartZoomPanBehavior zoomPan = new ChartZoomPanBehavior()
{
    EnableSelectionZooming = true
};
```

### Customize Selection Rectangle

```xml
<chart:ChartZoomPanBehavior EnableSelectionZooming="True"
                            SelectionRectStroke="Blue"
                            SelectionRectStrokeWidth="2"
                            SelectionRectFill="#804CAF50"/>
```

```csharp
ChartZoomPanBehavior zoomPan = new ChartZoomPanBehavior()
{
    EnableSelectionZooming = true,
    SelectionRectStroke = new SolidColorBrush(Colors.Blue),
    SelectionRectStrokeWidth = 2,
    SelectionRectFill = new SolidColorBrush(Color.FromArgb("#804CAF50"))
};
```

## Panning

### Enable Panning

```xml
<chart:ChartZoomPanBehavior EnablePanning="True"/>
```

```csharp
ChartZoomPanBehavior zoomPan = new ChartZoomPanBehavior()
{
    EnablePanning = true
};
```

### Combined Zoom and Pan

```xml
<chart:ChartZoomPanBehavior EnablePanning="True"
                            EnablePinchZooming="True"
                            EnableDoubleTap="True"/>
```

```csharp
ChartZoomPanBehavior zoomPan = new ChartZoomPanBehavior()
{
    EnablePanning = true,
    EnablePinchZooming = true,
    EnableDoubleTap = true
};
```

## Zoom Reset

### Enable Double-Tap Reset

```xml
<chart:ChartZoomPanBehavior EnableDoubleTap="True"/>
```

```csharp
ChartZoomPanBehavior zoomPan = new ChartZoomPanBehavior()
{
    EnableDoubleTap = true  // Double-tap to reset zoom
};
```

### Programmatic Zoom

```csharp
// Zoom to specific range
chart.ZoomPanBehavior.ZoomByFactor(0.5);

// Reset zoom
chart.ZoomPanBehavior.Reset();

// Zoom to specific axis range
chart.XAxes[0].ZoomFactor = 0.5;
chart.XAxes[0].ZoomPosition = 0.25;
```

### Maximum Zoom Level

```xml
<chart:ChartZoomPanBehavior MaximumZoomLevel="5"/>
```

```csharp
ChartZoomPanBehavior zoomPan = new ChartZoomPanBehavior()
{
    MaximumZoomLevel = 5  // Maximum zoom is 5x
};
```

## Complete Example

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.ZoomPanBehavior>
        <chart:ChartZoomPanBehavior EnablePanning="True"
                                    EnablePinchZooming="True"
                                    EnableSelectionZooming="True"
                                    EnableDoubleTap="True"
                                    ZoomMode="XY"
                                    MaximumZoomLevel="10"
                                    SelectionRectStroke="Blue"
                                    SelectionRectStrokeWidth="2"
                                    SelectionRectFill="#404CAF50"/>
    </chart:SfCartesianChart.ZoomPanBehavior>
    
    <chart:SfCartesianChart.XAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:LineSeries ItemsSource="{Binding LargeDataset}"
                     XBindingPath="X"
                     YBindingPath="Y"/>
</chart:SfCartesianChart>
```

## Best Practices

### When to Enable Zooming

**Use zooming when:**
- Dataset has many data points (>100)
- Detailed exploration is needed
- Time-series data with wide range
- Multi-level data analysis required

**Avoid zooming when:**
- Dataset is small (<20 points)
- Overview is more important than details
- Chart is for static presentation
- Screen space is limited

### Zoom Mode Selection

1. **XY Mode**: General purpose, most flexible
2. **X Mode**: Time-series, horizontal trends
3. **Y Mode**: Value comparisons, vertical analysis

### Mobile Considerations

1. Enable pinch zooming for touch devices
2. Provide reset button for easy navigation
3. Test gesture conflicts with other UI elements
4. Consider screen size limitations

### Performance Tips

1. Limit data points for smooth zooming
2. Use FastLineSeries for large datasets
3. Disable animations during zoom
4. Test on target devices

### User Experience

1. **Visual Feedback**: Clear selection rectangle
2. **Reset Option**: Easy zoom reset (double-tap)
3. **Boundaries**: Set MaximumZoomLevel
4. **Instructions**: Provide user guidance for gestures

### Common Patterns

**Time-Series Analysis:**
```xml
<chart:ChartZoomPanBehavior EnablePanning="True"
                            ZoomMode="X"
                            EnableDoubleTap="True"/>
```

**Data Exploration:**
```xml
<chart:ChartZoomPanBehavior EnablePanning="True"
                            EnableSelectionZooming="True"
                            ZoomMode="XY"/>
```

**Mobile-Optimized:**
```xml
<chart:ChartZoomPanBehavior EnablePinchZooming="True"
                            EnableDoubleTap="True"
                            MaximumZoomLevel="5"/>
```

## Troubleshooting

**Issue**: Zooming not working
- **Solution**: Verify `EnablePinchZooming="True"` is set

**Issue**: Can't zoom Y-axis
- **Solution**: Set `ZoomMode="Y"` or `ZoomMode="XY"`

**Issue**: Zoom too sensitive
- **Solution**: Adjust `MaximumZoomLevel` to limit zoom range

**Issue**: Can't reset zoom
- **Solution**: Enable `EnableDoubleTap="True"` or call `Reset()` method

**Issue**: Panning after zooming not working
- **Solution**: Ensure `EnablePanning="True"` is set

**Issue**: Selection rectangle not visible
- **Solution**: Set `SelectionRectStroke` and `SelectionRectFill` colors

**Issue**: Poor performance when zooming
- **Solution**: Reduce data points or use FastLineSeries
