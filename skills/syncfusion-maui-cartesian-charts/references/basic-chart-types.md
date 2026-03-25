# Basic Chart Types

## Table of Contents
- [Overview](#overview)
- [Line Chart](#line-chart)
- [Spline Chart](#spline-chart)
- [Area Chart](#area-chart)
- [Spline Area Chart](#spline-area-chart)
- [Column Chart](#column-chart)
- [Bar Chart](#bar-chart)
- [Scatter Chart](#scatter-chart)
- [Bubble Chart](#bubble-chart)
- [When to Use Each Type](#when-to-use-each-type)

## Overview

Syncfusion .NET MAUI Cartesian Chart provides fundamental chart types for common data visualization scenarios. These basic chart types form the foundation of most data visualization needs, from trend analysis to comparisons and distributions.

## Line Chart

Line charts connect data points with straight lines, ideal for showing trends over time or continuous data.

### Basic Implementation

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:LineSeries ItemsSource="{Binding Data}"
                     XBindingPath="Month"
                     YBindingPath="Sales"/>
</chart:SfCartesianChart>
```

```csharp
LineSeries series = new LineSeries()
{
    ItemsSource = new ViewModel().Data,
    XBindingPath = "Month",
    YBindingPath = "Sales"
};
chart.Series.Add(series);
```

### Dashed Line

Create dashed lines using the `StrokeDashArray` property:

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.Resources>
        <DoubleCollection x:Key="dashArray">
            <x:Double>5</x:Double>
            <x:Double>2</x:Double>
        </DoubleCollection>
    </chart:SfCartesianChart.Resources>
    
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:LineSeries ItemsSource="{Binding Data}"
                     XBindingPath="Month"
                     YBindingPath="Sales"
                     StrokeDashArray="{StaticResource dashArray}"/>
</chart:SfCartesianChart>
```

```csharp
DoubleCollection dashArray = new DoubleCollection { 5, 2 };
LineSeries series = new LineSeries()
{
    ItemsSource = new ViewModel().Data,
    XBindingPath = "Month",
    YBindingPath = "Sales",
    StrokeDashArray = dashArray
};
```

**Pattern:** Odd values are dash length, even values are gap length (5 pixels dash, 2 pixels gap).

### Line with Markers

Enable markers to highlight individual data points:

```xml
<chart:LineSeries ItemsSource="{Binding Data}"
                 XBindingPath="Month"
                 YBindingPath="Sales"
                 ShowMarkers="True">
    <chart:LineSeries.MarkerSettings>
        <chart:ChartMarkerSettings Type="Diamond"
                                  Fill="Red"
                                  Stroke="Black"
                                  StrokeWidth="1"
                                  Height="10"
                                  Width="10"/>
    </chart:LineSeries.MarkerSettings>
</chart:LineSeries>
```

```csharp
LineSeries series = new LineSeries()
{
    ItemsSource = data,
    XBindingPath = "Month",
    YBindingPath = "Sales",
    ShowMarkers = true,
    MarkerSettings = new ChartMarkerSettings
    {
        Type = ShapeType.Diamond,
        Fill = Colors.Red,
        Stroke = Colors.Black,
        StrokeWidth = 1,
        Height = 10,
        Width = 10
    }
};
```

**Marker Types:** Circle, Cross, Diamond, Hexagon, InvertedTriangle, Pentagon, Plus, Square, Triangle

## Spline Chart

Spline charts use smooth Bezier curves instead of straight lines, creating a more flowing visualization.

### Basic Spline

```xml
<chart:SplineSeries ItemsSource="{Binding Data}"
                   XBindingPath="Month"
                   YBindingPath="Sales"/>
```

```csharp
SplineSeries series = new SplineSeries()
{
    ItemsSource = new ViewModel().Data,
    XBindingPath = "Month",
    YBindingPath = "Sales"
};
```

### Spline Types

The `Type` property controls the curve algorithm:

```xml
<chart:SplineSeries ItemsSource="{Binding Data}"
                   XBindingPath="Month"
                   YBindingPath="Sales"
                   Type="Cardinal"/>
```

**Available Types:**
- **Natural** (default): Natural cubic spline interpolation
- **Monotonic**: Preserves monotonicity of data (no overshooting)
- **Cardinal**: Cardinal spline with tension control
- **Clamped**: Clamped cubic spline with controlled endpoints

```csharp
SplineSeries series = new SplineSeries()
{
    ItemsSource = data,
    XBindingPath = "Month",
    YBindingPath = "Sales",
    Type = SplineType.Cardinal
};
```

## Area Chart

Area charts fill the region between the line and the axis, emphasizing magnitude of change.

### Basic Area

```xml
<chart:AreaSeries ItemsSource="{Binding Data}"
                 XBindingPath="Month"
                 YBindingPath="Sales"/>
```

```csharp
AreaSeries series = new AreaSeries()
{
    ItemsSource = new ViewModel().Data,
    XBindingPath = "Month",
    YBindingPath = "Sales"
};
```

### Area with Transparency

Use opacity to show overlapping areas:

```xml
<chart:AreaSeries ItemsSource="{Binding Data1}"
                 XBindingPath="Month"
                 YBindingPath="Sales"
                 Fill="#80FF0000"
                 Opacity="0.6"/>
                 
<chart:AreaSeries ItemsSource="{Binding Data2}"
                 XBindingPath="Month"
                 YBindingPath="Sales"
                 Fill="#800000FF"
                 Opacity="0.6"/>
```

### Area with Markers

```xml
<chart:AreaSeries ItemsSource="{Binding Data}"
                 XBindingPath="Month"
                 YBindingPath="Sales"
                 ShowMarkers="True">
    <chart:AreaSeries.MarkerSettings>
        <chart:ChartMarkerSettings Type="Circle"
                                  Fill="White"
                                  Stroke="Blue"
                                  StrokeWidth="2"
                                  Height="8"
                                  Width="8"/>
    </chart:AreaSeries.MarkerSettings>
</chart:AreaSeries>
```

## Spline Area Chart

Combines spline curves with area filling:

```xml
<chart:SplineAreaSeries ItemsSource="{Binding Data}"
                       XBindingPath="Month"
                       YBindingPath="Sales"/>
```

```csharp
SplineAreaSeries series = new SplineAreaSeries()
{
    ItemsSource = new ViewModel().Data,
    XBindingPath = "Month",
    YBindingPath = "Sales"
};
```

## Column Chart

Column charts use vertical bars to compare values across categories.

### Basic Column

```xml
<chart:ColumnSeries ItemsSource="{Binding Data}"
                   XBindingPath="Category"
                   YBindingPath="Value"/>
```

```csharp
ColumnSeries series = new ColumnSeries()
{
    ItemsSource = new ViewModel().Data,
    XBindingPath = "Category",
    YBindingPath = "Value"
};
```

### Spacing and Width

Control bar spacing and width:

```xml
<chart:ColumnSeries ItemsSource="{Binding Data}"
                   XBindingPath="Category"
                   YBindingPath="Value"
                   Spacing="0.3"
                   Width="0.7"/>
```

**Spacing:** 0 to 1 (0 = no gap, 1 = 100% gap)  
**Width:** 0 to 1 (0 = no width, 1 = full width)

```csharp
ColumnSeries series = new ColumnSeries()
{
    ItemsSource = data,
    XBindingPath = "Category",
    YBindingPath = "Value",
    Spacing = 0.3,  // 30% spacing between bars
    Width = 0.7     // 70% of available width
};
```

### Overlapped Columns

Place multiple series on top of each other:

```xml
<chart:SfCartesianChart EnableSideBySideSeriesPlacement="False">
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:ColumnSeries ItemsSource="{Binding Data1}"
                       XBindingPath="Category"
                       YBindingPath="Value"
                       Width="0.8"/>
    
    <chart:ColumnSeries ItemsSource="{Binding Data2}"
                       XBindingPath="Category"
                       YBindingPath="Value"
                       Width="0.4"/>
</chart:SfCartesianChart>
```

```csharp
chart.EnableSideBySideSeriesPlacement = false;

ColumnSeries series1 = new ColumnSeries() { Width = 0.8 };
ColumnSeries series2 = new ColumnSeries() { Width = 0.4 };
```

## Bar Chart

Bar charts are horizontal columns, created by transposing the chart.

### Basic Bar

```xml
<chart:SfCartesianChart IsTransposed="True">
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:ColumnSeries ItemsSource="{Binding Data}"
                       XBindingPath="Category"
                       YBindingPath="Value"/>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();
chart.IsTransposed = true;  // This makes columns horizontal (bar chart)

ColumnSeries series = new ColumnSeries()
{
    ItemsSource = data,
    XBindingPath = "Category",
    YBindingPath = "Value"
};
```

### Bar with Spacing

```xml
<chart:SfCartesianChart IsTransposed="True">
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:ColumnSeries ItemsSource="{Binding Data}"
                       XBindingPath="Category"
                       YBindingPath="Value"
                       Spacing="0.2"
                       Width="0.6"/>
</chart:SfCartesianChart>
```

## Scatter Chart

Scatter charts display data points as individual markers, ideal for correlation analysis.

### Basic Scatter

```xml
<chart:ScatterSeries ItemsSource="{Binding Data}"
                    XBindingPath="XValue"
                    YBindingPath="YValue"
                    PointHeight="10"
                    PointWidth="10"/>
```

```csharp
ScatterSeries series = new ScatterSeries()
{
    ItemsSource = new ViewModel().Data,
    XBindingPath = "XValue",
    YBindingPath = "YValue",
    PointHeight = 10,
    PointWidth = 10
};
```

### Custom Point Shape

```xml
<chart:ScatterSeries ItemsSource="{Binding Data}"
                    XBindingPath="XValue"
                    YBindingPath="YValue"
                    PointHeight="12"
                    PointWidth="12"
                    Fill="Orange"
                    Stroke="DarkOrange"
                    StrokeWidth="2"/>
```

```csharp
ScatterSeries series = new ScatterSeries()
{
    ItemsSource = data,
    XBindingPath = "XValue",
    YBindingPath = "YValue",
    PointHeight = 12,
    PointWidth = 12,
    Fill = new SolidColorBrush(Colors.Orange),
    Stroke = new SolidColorBrush(Colors.DarkOrange),
    StrokeWidth = 2
};
```

## Bubble Chart

Bubble charts add a third dimension (size) to scatter plots.

### Basic Bubble

```xml
<chart:BubbleSeries ItemsSource="{Binding Data}"
                   XBindingPath="XValue"
                   YBindingPath="YValue"
                   SizeValuePath="Size"/>
```

```csharp
BubbleSeries series = new BubbleSeries()
{
    ItemsSource = new ViewModel().Data,
    XBindingPath = "XValue",
    YBindingPath = "YValue",
    SizeValuePath = "Size"  // Property that determines bubble size
};
```

**Data Model Example:**
```csharp
public class BubbleData
{
    public double XValue { get; set; }
    public double YValue { get; set; }
    public double Size { get; set; }  // Third dimension
}
```

### Size Constraints

Control minimum and maximum bubble sizes:

```xml
<chart:BubbleSeries ItemsSource="{Binding Data}"
                   XBindingPath="XValue"
                   YBindingPath="YValue"
                   SizeValuePath="Size"
                   MinimumRadius="5"
                   MaximumRadius="50"/>
```

```csharp
BubbleSeries series = new BubbleSeries()
{
    ItemsSource = data,
    XBindingPath = "XValue",
    YBindingPath = "YValue",
    SizeValuePath = "Size",
    MinimumRadius = 5,
    MaximumRadius = 50
};
```

### Zero-Size Bubbles

Control visibility of zero-size bubbles:

```xml
<chart:BubbleSeries ItemsSource="{Binding Data}"
                   XBindingPath="XValue"
                   YBindingPath="YValue"
                   SizeValuePath="Size"
                   ShowZeroSizeBubbles="False"/>
```

```csharp
BubbleSeries series = new BubbleSeries()
{
    ShowZeroSizeBubbles = false  // Hide bubbles with size = 0
};
```

## When to Use Each Type

### Line Chart
**Use when:**
- Showing trends over time
- Continuous data visualization
- Multiple series comparison
- Emphasizing rate of change

**Example scenarios:** Stock prices, temperature changes, sales trends

### Spline Chart
**Use when:**
- Smoother trend visualization needed
- Data has natural curves
- Reducing visual noise from jagged lines

**Example scenarios:** Growth curves, demographic trends, smooth projections

### Area Chart
**Use when:**
- Emphasizing magnitude of change
- Showing cumulative totals
- Visualizing part-to-whole relationships
- Comparing volumes over time

**Example scenarios:** Market share evolution, resource utilization, population growth

### Column Chart
**Use when:**
- Comparing discrete categories
- Showing exact values
- Limited number of data points
- Vertical space available

**Example scenarios:** Monthly sales by product, survey results, regional comparisons

### Bar Chart
**Use when:**
- Long category names
- Many categories to compare
- Horizontal layout preferred
- Reading convenience for labels

**Example scenarios:** Country rankings, feature comparisons, horizontal timelines

### Scatter Chart
**Use when:**
- Analyzing correlation between two variables
- Identifying patterns or clusters
- Detecting outliers
- No categorical axis needed

**Example scenarios:** Height vs weight, price vs demand, test score correlations

### Bubble Chart
**Use when:**
- Three variables need visualization
- Size represents important dimension
- Scatter plot needs additional context
- Comparing relative magnitudes

**Example scenarios:** Market analysis (price, volume, market cap), geographic data with population

## Common Patterns

### Multi-Series Comparison

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.Legend>
        <chart:ChartLegend/>
    </chart:SfCartesianChart.Legend>
    
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:LineSeries ItemsSource="{Binding Data2023}"
                     XBindingPath="Month"
                     YBindingPath="Sales"
                     Label="2023"
                     ShowMarkers="True"/>
    
    <chart:LineSeries ItemsSource="{Binding Data2024}"
                     XBindingPath="Month"
                     YBindingPath="Sales"
                     Label="2024"
                     ShowMarkers="True"/>
</chart:SfCartesianChart>
```

### Mixed Chart Types

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <!-- Column for actual values -->
    <chart:ColumnSeries ItemsSource="{Binding ActualData}"
                       XBindingPath="Month"
                       YBindingPath="Value"
                       Label="Actual"/>
    
    <!-- Line for target/trend -->
    <chart:LineSeries ItemsSource="{Binding TargetData}"
                     XBindingPath="Month"
                     YBindingPath="Value"
                     Label="Target"
                     StrokeDashArray="5,2"/>
</chart:SfCartesianChart>
```

## Tips and Best Practices

1. **Choose appropriate chart type** based on data characteristics and message
2. **Use markers sparingly** - only when highlighting specific points adds value
3. **Limit series count** - too many overlapping series reduce clarity
4. **Consider color contrast** - especially for overlapping area charts
5. **Use legends** when displaying multiple series
6. **Adjust spacing/width** for better visual balance
7. **Match axis types** to data (categorical, numerical, datetime)
8. **Enable tooltips** for detailed value inspection
