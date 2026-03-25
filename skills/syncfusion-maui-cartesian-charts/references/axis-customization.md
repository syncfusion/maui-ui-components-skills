# Axis Customization

## Table of Contents
- [Overview](#overview)
- [Grid Lines](#grid-lines)
- [Tick Lines](#tick-lines)
- [Axis Line](#axis-line)
- [Axis Padding](#axis-padding)
- [Range Padding](#range-padding)
- [Auto Scrolling Delta](#auto-scrolling-delta)
- [Custom Axis Labels](#custom-axis-labels)
- [Best Practices](#best-practices)

## Overview

Axis customization in Syncfusion .NET MAUI Cartesian Chart allows you to style and configure various visual elements including grid lines, tick lines, axis lines, and spacing. These customizations enhance chart readability and visual appeal.

## Grid Lines

Grid lines are horizontal and vertical lines that help identify data point values on the axis.

### Basic Grid Line Configuration

```xml
<chart:NumericalAxis ShowMajorGridLines="True"
                     ShowMinorGridLines="True"/>
```

```csharp
NumericalAxis axis = new NumericalAxis()
{
    ShowMajorGridLines = true,
    ShowMinorGridLines = true
};
```

### Major Grid Line Styling

```xml
<chart:NumericalAxis ShowMajorGridLines="True">
    <chart:NumericalAxis.MajorGridLineStyle>
        <chart:ChartLineStyle Stroke="Gray"
                              StrokeWidth="1"
                              StrokeDashArray="5,3"/>
    </chart:NumericalAxis.MajorGridLineStyle>
</chart:NumericalAxis>
```

```csharp
ChartLineStyle majorGridStyle = new ChartLineStyle()
{
    Stroke = new SolidColorBrush(Colors.Gray),
    StrokeWidth = 1,
    StrokeDashArray = new double[] { 5, 3 }
};

NumericalAxis axis = new NumericalAxis()
{
    ShowMajorGridLines = true,
    MajorGridLineStyle = majorGridStyle
};
```

### Minor Grid Line Styling

```xml
<chart:NumericalAxis ShowMinorGridLines="True"
                     MinorTicksPerInterval="4">
    <chart:NumericalAxis.MinorGridLineStyle>
        <chart:ChartLineStyle Stroke="LightGray"
                              StrokeWidth="0.5"
                              StrokeDashArray="2,2"/>
    </chart:NumericalAxis.MinorGridLineStyle>
</chart:NumericalAxis>
```

```csharp
ChartLineStyle minorGridStyle = new ChartLineStyle()
{
    Stroke = new SolidColorBrush(Colors.LightGray),
    StrokeWidth = 0.5,
    StrokeDashArray = new double[] { 2, 2 }
};

NumericalAxis axis = new NumericalAxis()
{
    ShowMinorGridLines = true,
    MinorTicksPerInterval = 4,
    MinorGridLineStyle = minorGridStyle
};
```

### Complete Grid Line Example

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis ShowMajorGridLines="True">
            <chart:CategoryAxis.MajorGridLineStyle>
                <chart:ChartLineStyle Stroke="#E0E0E0" StrokeWidth="1"/>
            </chart:CategoryAxis.MajorGridLineStyle>
        </chart:CategoryAxis>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis ShowMajorGridLines="True"
                             ShowMinorGridLines="True"
                             MinorTicksPerInterval="4">
            <chart:NumericalAxis.MajorGridLineStyle>
                <chart:ChartLineStyle Stroke="Gray" StrokeWidth="1"/>
            </chart:NumericalAxis.MajorGridLineStyle>
            <chart:NumericalAxis.MinorGridLineStyle>
                <chart:ChartLineStyle Stroke="LightGray" 
                                      StrokeWidth="0.5"
                                      StrokeDashArray="2,2"/>
            </chart:NumericalAxis.MinorGridLineStyle>
        </chart:NumericalAxis>
    </chart:SfCartesianChart.YAxes>
</chart:SfCartesianChart>
```

## Tick Lines

Tick lines are small lines on the axis that indicate data point values.

### Major Tick Lines

```xml
<chart:NumericalAxis ShowMajorTickLines="True"
                     MajorTickLineSize="8"/>
```

```csharp
NumericalAxis axis = new NumericalAxis()
{
    ShowMajorTickLines = true,
    MajorTickLineSize = 8
};
```

### Major Tick Line Styling

```xml
<chart:NumericalAxis ShowMajorTickLines="True"
                     MajorTickLineSize="8">
    <chart:NumericalAxis.MajorTickLineStyle>
        <chart:ChartLineStyle Stroke="Black" StrokeWidth="2"/>
    </chart:NumericalAxis.MajorTickLineStyle>
</chart:NumericalAxis>
```

```csharp
ChartLineStyle tickStyle = new ChartLineStyle()
{
    Stroke = new SolidColorBrush(Colors.Black),
    StrokeWidth = 2
};

NumericalAxis axis = new NumericalAxis()
{
    ShowMajorTickLines = true,
    MajorTickLineSize = 8,
    MajorTickLineStyle = tickStyle
};
```

### Minor Tick Lines

```xml
<chart:NumericalAxis ShowMinorTickLines="True"
                     MinorTicksPerInterval="4"
                     MinorTickLineSize="4">
    <chart:NumericalAxis.MinorTickLineStyle>
        <chart:ChartLineStyle Stroke="Gray" StrokeWidth="1"/>
    </chart:NumericalAxis.MinorTickLineStyle>
</chart:NumericalAxis>
```

```csharp
ChartLineStyle minorTickStyle = new ChartLineStyle()
{
    Stroke = new SolidColorBrush(Colors.Gray),
    StrokeWidth = 1
};

NumericalAxis axis = new NumericalAxis()
{
    ShowMinorTickLines = true,
    MinorTicksPerInterval = 4,
    MinorTickLineSize = 4,
    MinorTickLineStyle = minorTickStyle
};
```

## Axis Line

The axis line is the main line of the axis.

### Basic Axis Line

```xml
<chart:NumericalAxis ShowAxisLine="True"/>
```

```csharp
NumericalAxis axis = new NumericalAxis()
{
    ShowAxisLine = true
};
```

### Axis Line Styling

```xml
<chart:NumericalAxis ShowAxisLine="True">
    <chart:NumericalAxis.AxisLineStyle>
        <chart:ChartLineStyle Stroke="Blue"
                              StrokeWidth="2"/>
    </chart:NumericalAxis.AxisLineStyle>
</chart:NumericalAxis>
```

```csharp
ChartLineStyle axisLineStyle = new ChartLineStyle()
{
    Stroke = new SolidColorBrush(Colors.Blue),
    StrokeWidth = 2
};

NumericalAxis axis = new NumericalAxis()
{
    ShowAxisLine = true,
    AxisLineStyle = axisLineStyle
};
```

### Complete Axis Line Example

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis ShowAxisLine="True">
            <chart:CategoryAxis.AxisLineStyle>
                <chart:ChartLineStyle Stroke="#333333" StrokeWidth="2"/>
            </chart:CategoryAxis.AxisLineStyle>
        </chart:CategoryAxis>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis ShowAxisLine="True">
            <chart:NumericalAxis.AxisLineStyle>
                <chart:ChartLineStyle Stroke="#333333" StrokeWidth="2"/>
            </chart:NumericalAxis.AxisLineStyle>
        </chart:NumericalAxis>
    </chart:SfCartesianChart.YAxes>
</chart:SfCartesianChart>
```

## Axis Padding

PlotOffset adds padding between the axis and the series.

### Plot Offset Start

```xml
<chart:NumericalAxis PlotOffsetStart="20"/>
```

```csharp
NumericalAxis axis = new NumericalAxis()
{
    PlotOffsetStart = 20
};
```

### Plot Offset End

```xml
<chart:NumericalAxis PlotOffsetEnd="20"/>
```

```csharp
NumericalAxis axis = new NumericalAxis()
{
    PlotOffsetEnd = 20
};
```

### Combined Plot Offsets

```xml
<chart:CategoryAxis PlotOffsetStart="30"
                    PlotOffsetEnd="30"/>
```

```csharp
CategoryAxis axis = new CategoryAxis()
{
    PlotOffsetStart = 30,
    PlotOffsetEnd = 30
};
```

## Range Padding

RangePadding adds space at the edges of the axis range.

### Numerical Axis Range Padding

```xml
<chart:NumericalAxis RangePadding="Auto"/>
```

```csharp
NumericalAxis axis = new NumericalAxis()
{
    RangePadding = NumericalPadding.Auto
};
```

**Options:**
- `None`: No padding
- `Normal`: Default padding
- `Additional`: Extra padding
- `Round`: Round to nearest interval
- `Auto`: Automatically determined
- `RoundStart`: Round at start only
- `RoundEnd`: Round at end only
- `PrependInterval`: Add interval at start
- `AppendInterval`: Add interval at end

### Category Axis Range Padding

```xml
<chart:CategoryAxis RangePadding="Auto"/>
```

```csharp
CategoryAxis axis = new CategoryAxis()
{
    RangePadding = CategoryPadding.Auto
};
```

**Options:**
- `None`: No padding
- `Additional`: Extra padding
- `Normal`: Default padding
- `RoundStart`: Round at start
- `RoundEnd`: Round at end
- `Auto`: Automatically determined

### DateTime Axis Range Padding

```xml
<chart:DateTimeAxis RangePadding="Auto"/>
```

```csharp
DateTimeAxis axis = new DateTimeAxis()
{
    RangePadding = DateTimeRangePadding.Auto
};
```

**Options:**
- `None`: No padding
- `Additional`: Extra padding
- `Round`: Round to interval
- `RoundStart`: Round at start
- `RoundEnd`: Round at end
- `Auto`: Automatically determined

### Range Padding Examples

**No Padding:**
```xml
<chart:NumericalAxis Minimum="0"
                     Maximum="100"
                     RangePadding="None"/>
```

**Normal Padding:**
```xml
<chart:NumericalAxis Minimum="0"
                     Maximum="100"
                     RangePadding="Normal"/>
```

**Additional Padding:**
```xml
<chart:NumericalAxis Minimum="0"
                     Maximum="100"
                     RangePadding="Additional"/>
```

## Auto Scrolling Delta

AutoScrollingDelta enables automatic scrolling with a visible range window.

### Basic Auto Scrolling

```xml
<chart:DateTimeAxis AutoScrollingDelta="7"
                    AutoScrollingDeltaType="Days"
                    AutoScrollingMode="End"/>
```

```csharp
DateTimeAxis axis = new DateTimeAxis()
{
    AutoScrollingDelta = 7,
    AutoScrollingDeltaType = DateTimeIntervalType.Days,
    AutoScrollingMode = ChartAutoScrollingMode.End
};
```

### Scrolling Modes

**End (Default):**
Shows the most recent data:

```xml
<chart:DateTimeAxis AutoScrollingDelta="10"
                    AutoScrollingDeltaType="Days"
                    AutoScrollingMode="End"/>
```

**Start:**
Shows from the beginning:

```xml
<chart:DateTimeAxis AutoScrollingDelta="10"
                    AutoScrollingDeltaType="Days"
                    AutoScrollingMode="Start"/>
```

### Numerical Axis Auto Scrolling

```xml
<chart:NumericalAxis AutoScrollingDelta="50"
                     AutoScrollingMode="End"/>
```

```csharp
NumericalAxis axis = new NumericalAxis()
{
    AutoScrollingDelta = 50,
    AutoScrollingMode = ChartAutoScrollingMode.End
};
```

### Complete Auto Scrolling Example

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:DateTimeAxis AutoScrollingDelta="30"
                            AutoScrollingDeltaType="Days"
                            AutoScrollingMode="End"
                            IntervalType="Days"
                            Interval="5"/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:LineSeries ItemsSource="{Binding TimeSeriesData}"
                     XBindingPath="Date"
                     YBindingPath="Value"/>
</chart:SfCartesianChart>
```

## Custom Axis Labels

`ChartAxis` provides the `OnCreateLabels` override method to add custom axis labels. The `OnCreateLabels` method is called whenever new labels are generated. The following properties are available to add custom labels:

* `VisibleLabels` - This property is used to get an Observable Collection of visible axis labels.
* `VisibleMaximum` - This property is used to get the double value that represents the maximum observable value of the axis range.
* `VisibleMinimum` - This property is used to get the double value that represents the minimum observable value of the axis range.

### Example: Custom Numerical Axis

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <local:CustomNumericalAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:ColumnSeries ItemsSource="{Binding Data}"
                       XBindingPath="XValue"
                       YBindingPath="YValue"/>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();

CustomNumericalAxis primaryAxis = new CustomNumericalAxis();
chart.XAxes.Add(primaryAxis);

NumericalAxis secondaryAxis = new NumericalAxis();
chart.YAxes.Add(secondaryAxis);

ColumnSeries series = new ColumnSeries()
{
    ItemsSource = new ViewModel().Data,
    XBindingPath = "XValue",
    YBindingPath = "YValue"
};

chart.Series.Add(series);
this.Content = chart;
```

### Custom Axis Implementation

```csharp
public class CustomNumericalAxis : NumericalAxis
{
    // Adding custom axis labels by displaying labels only on specific x-axis values
    protected override void OnCreateLabels()
    {
        base.OnCreateLabels();

        if (VisibleLabels != null)
        {
            VisibleLabels.Clear();

            ViewModel viewModel = BindingContext as ViewModel;

            for (int i = 0; i < viewModel.Data.Count; i++)
            {
                var data = viewModel.Data[i];
                VisibleLabels.Add(new ChartAxisLabel(data.XValue, data.XValue.ToString()));
            }
        }
    }
}
```

**Note**: 
- This approach applies to all types of axes.
- Labels are rendered only if the label position presents within the visible range.
- The labels should be created only if users call the base of `OnCreateLabels`.

## Best Practices

### Grid Lines

1. **Major Grid Lines**: Use for primary value divisions
2. **Minor Grid Lines**: Use sparingly; too many can clutter
3. **Color Selection**: Use subtle colors (light gray) for grids
4. **Dashed Lines**: Consider dashed lines for better distinction

### Tick Lines

1. **Visibility**: Show major tick lines for clarity
2. **Size**: Keep tick lines proportional to axis (6-10 pixels)
3. **Minor Ticks**: Use 3-5 minor ticks per interval
4. **Color**: Match or complement axis line color

### Axis Line

1. **Always Visible**: Axis lines should typically be visible
2. **Weight**: Use slightly thicker lines (2px) than grid lines
3. **Color**: Use darker colors for better definition

### Padding and Spacing

1. **PlotOffset**: Use 10-30 pixels for breathing room
2. **RangePadding**: Use `Auto` or `Normal` for most cases
3. **Additional Padding**: Use when data points touch edges
4. **Round Padding**: Good for clean, rounded values

### Auto Scrolling

1. **Real-time Data**: Ideal for live data streams
2. **Delta Size**: Choose delta based on update frequency
3. **Mode Selection**: Use `End` for recent data focus
4. **Performance**: Limit visible range for better performance

### Common Combinations

**Clean Professional Look:**
```xml
<chart:NumericalAxis ShowMajorGridLines="True"
                     ShowAxisLine="True"
                     ShowMajorTickLines="True"
                     PlotOffsetStart="20"
                     PlotOffsetEnd="20"
                     RangePadding="Normal">
    <chart:NumericalAxis.MajorGridLineStyle>
        <chart:ChartLineStyle Stroke="#E0E0E0" StrokeWidth="1"/>
    </chart:NumericalAxis.MajorGridLineStyle>
    <chart:NumericalAxis.AxisLineStyle>
        <chart:ChartLineStyle Stroke="#333333" StrokeWidth="2"/>
    </chart:NumericalAxis.AxisLineStyle>
</chart:NumericalAxis>
```

**Minimal Clean Look:**
```xml
<chart:NumericalAxis ShowMajorGridLines="False"
                     ShowAxisLine="True"
                     ShowMajorTickLines="False"
                     RangePadding="Additional">
    <chart:NumericalAxis.AxisLineStyle>
        <chart:ChartLineStyle Stroke="Black" StrokeWidth="1"/>
    </chart:NumericalAxis.AxisLineStyle>
</chart:NumericalAxis>
```

### Troubleshooting

**Issue**: Grid lines too prominent
- **Solution**: Use lighter colors or dashed lines

**Issue**: Tick lines not visible
- **Solution**: Increase `MajorTickLineSize` or StrokeWidth

**Issue**: Data points cut off at edges
- **Solution**: Add PlotOffset or use RangePadding="Additional"

**Issue**: Auto scrolling not working
- **Solution**: Verify AutoScrollingDelta and Mode are set correctly

**Issue**: Too much whitespace around data
- **Solution**: Set RangePadding="None" or use explicit Minimum/Maximum values
