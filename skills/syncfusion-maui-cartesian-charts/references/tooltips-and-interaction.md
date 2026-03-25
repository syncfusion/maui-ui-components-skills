# Tooltips and Interaction

## Table of Contents
- [Overview](#overview)
- [Tooltip](#tooltip)
- [Trackball](#trackball)
- [Crosshair](#crosshair)
- [Best Practices](#best-practices)

## Overview

Interactive features like tooltips, trackball, and crosshair enhance user experience by providing detailed information about data points. These features help users explore chart data dynamically.

## Tooltip

Tooltips display information about data points when users hover over or tap them.

### Basic Tooltip

```xml
<chart:LineSeries ItemsSource="{Binding Data}"
                 XBindingPath="Month"
                 YBindingPath="Value"
                 EnableTooltip="True"/>
```

```csharp
LineSeries series = new LineSeries()
{
    ItemsSource = new ViewModel().Data,
    XBindingPath = "Month",
    YBindingPath = "Value",
    EnableTooltip = true
};
```

### Tooltip Customization

```xml
<chart:LineSeries ItemsSource="{Binding Data}"
                 XBindingPath="Month"
                 YBindingPath="Value"
                 EnableTooltip="True">
    <chart:LineSeries.TooltipTemplate>
        <DataTemplate>
            <StackLayout>
                <Label Text="{Binding Item.Month}"
                       FontAttributes="Bold"
                       TextColor="White"/>
                <Label Text="{Binding Item.Value, StringFormat='Value: {0:N2}'}"
                       TextColor="White"/>
            </StackLayout>
        </DataTemplate>
    </chart:LineSeries.TooltipTemplate>
</chart:LineSeries>
```

### Chart Tooltip Behavior

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.TooltipBehavior>
        <chart:ChartTooltipBehavior Background="#4CAF50"
                                    TextColor="White"
                                    FontSize="14"
                                    Margin="5"
                                    CornerRadius="5"
                                    Duration="3"/>
    </chart:SfCartesianChart.TooltipBehavior>
    
    <chart:LineSeries ItemsSource="{Binding Data}"
                     XBindingPath="Month"
                     YBindingPath="Value"
                     EnableTooltip="True"/>
</chart:SfCartesianChart>
```

## Trackball

Trackball displays a vertical line and shows data values for all series at that position.

### Basic Trackball

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.TrackballBehavior>
        <chart:ChartTrackballBehavior/>
    </chart:SfCartesianChart.TrackballBehavior>
    
    <chart:LineSeries ItemsSource="{Binding Data1}"
                     XBindingPath="Month"
                     YBindingPath="Value"
                     Label="Series 1"/>
    
    <chart:LineSeries ItemsSource="{Binding Data2}"
                     XBindingPath="Month"
                     YBindingPath="Value"
                     Label="Series 2"/>
</chart:SfCartesianChart>
```

```csharp
ChartTrackballBehavior trackball = new ChartTrackballBehavior();

SfCartesianChart chart = new SfCartesianChart();
chart.TrackballBehavior = trackball;

LineSeries series1 = new LineSeries()
{
    ItemsSource = new ViewModel().Data1,
    XBindingPath = "Month",
    YBindingPath = "Value",
    Label = "Series 1"
};

LineSeries series2 = new LineSeries()
{
    ItemsSource = new ViewModel().Data2,
    XBindingPath = "Month",
    YBindingPath = "Value",
    Label = "Series 2"
};

chart.Series.Add(series1);
chart.Series.Add(series2);
```

### Trackball Customization

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.TrackballBehavior>
        <chart:ChartTrackballBehavior ShowLine="True"
                                      ShowLabel="True"
                                      ShowMarkers="True">
            <chart:ChartTrackballBehavior.LineStyle>
                <chart:ChartLineStyle Stroke="Red"
                                      StrokeWidth="2"
                                      StrokeDashArray="5,3"/>
            </chart:ChartTrackballBehavior.LineStyle>
            
            <chart:ChartTrackballBehavior.LabelStyle>
                <chart:ChartLabelStyle Background="Blue"
                                       TextColor="White"
                                       FontSize="12"
                                       Margin="3"/>
            </chart:ChartTrackballBehavior.LabelStyle>
        </chart:ChartTrackballBehavior>
    </chart:SfCartesianChart.TrackballBehavior>
</chart:SfCartesianChart>
```

### Trackball Label Template

```xml
<chart:ChartTrackballBehavior>
    <chart:ChartTrackballBehavior.LabelTemplate>
        <DataTemplate>
            <StackLayout Orientation="Vertical">
                <Label Text="{Binding Label}"
                       FontAttributes="Bold"
                       TextColor="White"/>
                <Label Text="{Binding ValueY, StringFormat='{0:N2}'}"
                       TextColor="White"/>
            </StackLayout>
        </DataTemplate>
    </chart:ChartTrackballBehavior.LabelTemplate>
</chart:ChartTrackballBehavior>
```

## Crosshair

Crosshair displays perpendicular lines with axis labels at the touch/cursor position.

### Basic Crosshair

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.CrosshairBehavior>
        <chart:ChartCrosshairBehavior/>
    </chart:SfCartesianChart.CrosshairBehavior>
    
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:LineSeries ItemsSource="{Binding Data}"
                     XBindingPath="Month"
                     YBindingPath="Value"/>
</chart:SfCartesianChart>
```

```csharp
ChartCrosshairBehavior crosshair = new ChartCrosshairBehavior();

SfCartesianChart chart = new SfCartesianChart();
chart.CrosshairBehavior = crosshair;
```

### Crosshair Customization

```xml
<chart:ChartCrosshairBehavior ShowHorizontalLine="True"
                              ShowVerticalLine="True"
                              ShowLabel="True">
    <chart:ChartCrosshairBehavior.HorizontalLineStyle>
        <chart:ChartLineStyle Stroke="Blue"
                              StrokeWidth="1"
                              StrokeDashArray="3,3"/>
    </chart:ChartCrosshairBehavior.HorizontalLineStyle>
    
    <chart:ChartCrosshairBehavior.VerticalLineStyle>
        <chart:ChartLineStyle Stroke="Green"
                              StrokeWidth="1"
                              StrokeDashArray="3,3"/>
    </chart:ChartCrosshairBehavior.VerticalLineStyle>
    
    <chart:ChartCrosshairBehavior.HorizontalLabelStyle>
        <chart:ChartLabelStyle Background="Blue"
                               TextColor="White"
                               Margin="2"/>
    </chart:ChartCrosshairBehavior.HorizontalLabelStyle>
    
    <chart:ChartCrosshairBehavior.VerticalLabelStyle>
        <chart:ChartLabelStyle Background="Green"
                               TextColor="White"
                               Margin="2"/>
    </chart:ChartCrosshairBehavior.VerticalLabelStyle>
</chart:ChartCrosshairBehavior>
```

### Complete Crosshair Example

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.CrosshairBehavior>
        <chart:ChartCrosshairBehavior ShowHorizontalLine="True"
                                      ShowVerticalLine="True"
                                      ShowLabel="True">
            <chart:ChartCrosshairBehavior.HorizontalLineStyle>
                <chart:ChartLineStyle Stroke="Red" StrokeWidth="2"/>
            </chart:ChartCrosshairBehavior.HorizontalLineStyle>
            
            <chart:ChartCrosshairBehavior.VerticalLineStyle>
                <chart:ChartLineStyle Stroke="Red" StrokeWidth="2"/>
            </chart:ChartCrosshairBehavior.VerticalLineStyle>
        </chart:ChartCrosshairBehavior>
    </chart:SfCartesianChart.CrosshairBehavior>
    
    <chart:SfCartesianChart.XAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:ScatterSeries ItemsSource="{Binding Data}"
                         XBindingPath="X"
                         YBindingPath="Y"
                         PointHeight="10"
                         PointWidth="10"/>
</chart:SfCartesianChart>
```

## Best Practices

### Tooltip Guidelines

1. **Keep It Concise**: Show essential information only
2. **Format Values**: Use appropriate number formatting
3. **Add Context**: Include series name for multi-series charts
4. **Visual Hierarchy**: Bold important values
5. **Consistent Style**: Match chart theme

### Trackball Best Practices

1. **Multi-Series**: Ideal for comparing multiple series
2. **Line Width**: Use visible but not obtrusive lines
3. **Label Clarity**: Ensure labels don't overlap
4. **Performance**: Test with large datasets
5. **Mobile**: Ensure touch-friendly interaction

### Crosshair Usage

1. **Precision**: Best for precise value reading
2. **Axis Labels**: Enable labels for exact values
3. **Line Style**: Use dashed lines for clarity
4. **Color Coordination**: Match or contrast with series
5. **Scientific Data**: Ideal for technical charts

### Combining Features

Avoid using all features simultaneously:
```xml
<!-- Good: Use tooltip OR trackball -->
<chart:SfCartesianChart>
    <chart:SfCartesianChart.TooltipBehavior>
        <chart:ChartTooltipBehavior/>
    </chart:SfCartesianChart.TooltipBehavior>
</chart:SfCartesianChart>

<!-- Avoid: Multiple interactive features -->
<chart:SfCartesianChart>
    <chart:SfCartesianChart.TooltipBehavior>
        <chart:ChartTooltipBehavior/>
    </chart:SfCartesianChart.TooltipBehavior>
    <chart:SfCartesianChart.TrackballBehavior>
        <chart:ChartTrackballBehavior/>
    </chart:SfCartesianChart.TrackballBehavior>
    <chart:SfCartesianChart.CrosshairBehavior>
        <chart:ChartCrosshairBehavior/>
    </chart:SfCartesianChart.CrosshairBehavior>
</chart:SfCartesianChart>
```

### Performance Optimization

1. Limit template complexity
2. Avoid heavy computations in templates
3. Test on target devices
4. Consider disabling for very large datasets
5. Use simple tooltips for basic scenarios

### Troubleshooting

**Issue**: Tooltip not appearing
- **Solution**: Verify `EnableTooltip="True"` on series

**Issue**: Trackball showing wrong values
- **Solution**: Check XBindingPath matches across all series

**Issue**: Crosshair lines not visible
- **Solution**: Increase StrokeWidth or change Stroke color

**Issue**: Tooltips cut off at edges
- **Solution**: Add chart margin or adjust tooltip position

**Issue**: Performance lag with trackball
- **Solution**: Simplify label template or reduce data points
