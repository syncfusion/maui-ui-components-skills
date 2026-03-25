# Statistical Charts

## Table of Contents
- [Overview](#overview)
- [Box and Whisker Chart](#box-and-whisker-chart)
- [Histogram Chart](#histogram-chart)
- [Error Bar Chart](#error-bar-chart)
- [Best Practices](#best-practices)

## Overview

Statistical charts in Syncfusion .NET MAUI provide powerful visualization tools for analyzing data distributions, variations, and statistical measures. These specialized chart types help identify patterns, outliers, and statistical properties of datasets.

**Available Statistical Chart Types:**
- **Box and Whisker**: Display distribution using quartiles, median, and outliers
- **Histogram**: Show frequency distribution across intervals
- **Error Bar**: Visualize uncertainty and variance in measurements

## Box and Whisker Chart

Box and whisker charts (box plots) display the distribution of data within a population using five key statistics: minimum, first quartile (Q1), median (Q2), third quartile (Q3), and maximum.

### Basic Implementation

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:BoxAndWhiskerSeries ItemsSource="{Binding BoxWhiskerData}"
                               XBindingPath="Department"
                               YBindingPath="Age"/>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();

CategoryAxis primaryAxis = new CategoryAxis();
chart.XAxes.Add(primaryAxis);

NumericalAxis secondaryAxis = new NumericalAxis();
chart.YAxes.Add(secondaryAxis);

BoxAndWhiskerSeries series = new BoxAndWhiskerSeries()
{
    ItemsSource = new ViewModel().BoxWhiskerData,
    XBindingPath = "Department",
    YBindingPath = "Age"
};

chart.Series.Add(series);
this.Content = chart;
```

### Data Model

```csharp
public class BoxWhiskerModel
{
    public string Department { get; set; }
    public List<double> Age { get; set; }
}

public class ViewModel
{
    public ObservableCollection<BoxWhiskerModel> BoxWhiskerData { get; set; }
    
    public ViewModel()
    {
        BoxWhiskerData = new ObservableCollection<BoxWhiskerModel>
        {
            new BoxWhiskerModel 
            { 
                Department = "Sales",
                Age = new List<double> { 22, 26, 28, 30, 32, 34, 36, 38, 40, 42 }
            },
            new BoxWhiskerModel 
            { 
                Department = "Engineering",
                Age = new List<double> { 24, 28, 30, 34, 36, 38, 40, 44, 46, 50 }
            },
            new BoxWhiskerModel 
            { 
                Department = "Marketing",
                Age = new List<double> { 20, 24, 26, 28, 30, 32, 34, 36, 38, 40 }
            }
        };
    }
}
```

### Box Plot Modes

The `BoxPlotMode` property determines how quartiles are calculated:

**1. Exclusive Mode (Default)**

Calculates quartiles using the formula (N+1) * P, with index starting from 1:

```xml
<chart:BoxAndWhiskerSeries ItemsSource="{Binding BoxWhiskerData}"
                           XBindingPath="Department"
                           YBindingPath="Age"
                           BoxPlotMode="Exclusive"/>
```

```csharp
BoxAndWhiskerSeries series = new BoxAndWhiskerSeries()
{
    ItemsSource = new ViewModel().BoxWhiskerData,
    XBindingPath = "Department",
    YBindingPath = "Age",
    BoxPlotMode = BoxPlotMode.Exclusive
};
```

**2. Inclusive Mode**

Calculates quartiles using the formula (N-1) * P, with index starting from 0:

```xml
<chart:BoxAndWhiskerSeries ItemsSource="{Binding BoxWhiskerData}"
                           XBindingPath="Department"
                           YBindingPath="Age"
                           BoxPlotMode="Inclusive"/>
```

**3. Normal Mode**

Extends whiskers to minimum and maximum values within two standard deviations:

```xml
<chart:BoxAndWhiskerSeries ItemsSource="{Binding BoxWhiskerData}"
                           XBindingPath="Department"
                           YBindingPath="Age"
                           BoxPlotMode="Normal"/>
```

### Show Median

Display the median line within the box:

```xml
<chart:BoxAndWhiskerSeries ItemsSource="{Binding BoxWhiskerData}"
                           XBindingPath="Department"
                           YBindingPath="Age"
                           ShowMedian="True"/>
```

```csharp
BoxAndWhiskerSeries series = new BoxAndWhiskerSeries()
{
    ItemsSource = new ViewModel().BoxWhiskerData,
    XBindingPath = "Department",
    YBindingPath = "Age",
    ShowMedian = true
};
```

**Median Calculation:**
- For odd number of data points: middle value
- For even number of data points: average of two middle values

### Outliers

Outliers are data points that lie beyond the whiskers (typically 1.5 × IQR from quartiles).

**Show/Hide Outliers:**

```xml
<chart:BoxAndWhiskerSeries ItemsSource="{Binding BoxWhiskerData}"
                           XBindingPath="Department"
                           YBindingPath="Age"
                           ShowOutlier="True"/>
```

```csharp
BoxAndWhiskerSeries series = new BoxAndWhiskerSeries()
{
    ItemsSource = new ViewModel().BoxWhiskerData,
    XBindingPath = "Department",
    YBindingPath = "Age",
    ShowOutlier = true // Default is true
};
```

**Outlier Shape Types:**

```xml
<chart:BoxAndWhiskerSeries ItemsSource="{Binding BoxWhiskerData}"
                           XBindingPath="Department"
                           YBindingPath="Age"
                           OutlierShapeType="Diamond"/>
```

```csharp
BoxAndWhiskerSeries series = new BoxAndWhiskerSeries()
{
    ItemsSource = new ViewModel().BoxWhiskerData,
    XBindingPath = "Department",
    YBindingPath = "Age",
    OutlierShapeType = ShapeType.Diamond
};
```

**Available Shapes:**
- Circle (default)
- Cross
- Diamond
- Hexagon
- InvertedTriangle
- Pentagon
- Plus
- Rectangle
- Triangle

## Histogram Chart

Histogram charts organize data into user-specified intervals (bins) and display the frequency distribution, similar to a column chart but specifically for frequency data.

### Basic Implementation

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:HistogramSeries ItemsSource="{Binding HistogramData}"
                           XBindingPath="Value"
                           YBindingPath="Size"
                           HistogramInterval="20"
                           ShowNormalDistributionCurve="True"/>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();

NumericalAxis xAxis = new NumericalAxis();
chart.XAxes.Add(xAxis);

NumericalAxis yAxis = new NumericalAxis();
chart.YAxes.Add(yAxis);

HistogramSeries series = new HistogramSeries()
{
    ItemsSource = new ViewModel().HistogramData,
    XBindingPath = "Value",
    YBindingPath = "Size",
    HistogramInterval = 20,
    ShowNormalDistributionCurve = true
};

chart.Series.Add(series);
this.Content = chart;
```

### Data Model

```csharp
public class HistogramModel
{
    public double Value { get; set; }
    public double Size { get; set; }
}

public class ViewModel
{
    public ObservableCollection<HistogramModel> HistogramData { get; set; }
    
    public ViewModel()
    {
        Random random = new Random();
        HistogramData = new ObservableCollection<HistogramModel>();
        
        for (int i = 0; i < 100; i++)
        {
            HistogramData.Add(new HistogramModel
            {
                Value = random.Next(10, 100),
                Size = 0
            });
        }
    }
}
```

### Histogram Interval

Control the width of bins using `HistogramInterval`:

```xml
<chart:HistogramSeries ItemsSource="{Binding HistogramData}"
                       XBindingPath="Value"
                       YBindingPath="Size"
                       HistogramInterval="15"/>
```

```csharp
HistogramSeries series = new HistogramSeries()
{
    ItemsSource = new ViewModel().HistogramData,
    XBindingPath = "Value",
    YBindingPath = "Size",
    HistogramInterval = 15
};
```

### Normal Distribution Curve

Display a bell curve overlay showing the normal distribution:

```xml
<chart:HistogramSeries ItemsSource="{Binding HistogramData}"
                       XBindingPath="Value"
                       YBindingPath="Size"
                       HistogramInterval="20"
                       ShowNormalDistributionCurve="True">
    <chart:HistogramSeries.CurveStyle>
        <chart:ChartLineStyle Stroke="Blue"
                              StrokeWidth="2"
                              StrokeDashArray="5,3"/>
    </chart:HistogramSeries.CurveStyle>
</chart:HistogramSeries>
```

```csharp
HistogramSeries series = new HistogramSeries()
{
    ItemsSource = new ViewModel().HistogramData,
    XBindingPath = "Value",
    YBindingPath = "Size",
    HistogramInterval = 20,
    ShowNormalDistributionCurve = true,
    CurveStyle = new ChartLineStyle()
    {
        Stroke = Colors.Blue,
        StrokeWidth = 2,
        StrokeDashArray = new double[] { 5, 3 }
    }
};
```

## Error Bar Chart

Error bars indicate uncertainty or variability in data measurements. They display the potential error or deviation from the reported value.

### Basic Implementation

Error bars are typically used with scatter series or other point-based charts:

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <!-- Main data series -->
    <chart:ScatterSeries ItemsSource="{Binding EnergyProductions}"
                         XBindingPath="ID"
                         YBindingPath="Coal"
                         PointHeight="15"
                         PointWidth="15"/>
    
    <!-- Error bar series -->
    <chart:ErrorBarSeries ItemsSource="{Binding EnergyProductions}"
                          XBindingPath="ID"
                          YBindingPath="Coal"
                          HorizontalErrorValue="0.5"
                          VerticalErrorValue="50"/>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();

NumericalAxis xAxis = new NumericalAxis();
chart.XAxes.Add(xAxis);

NumericalAxis yAxis = new NumericalAxis();
chart.YAxes.Add(yAxis);

// Main series
ScatterSeries scatterSeries = new ScatterSeries()
{
    ItemsSource = new ViewModel().EnergyProductions,
    XBindingPath = "ID",
    YBindingPath = "Coal",
    PointWidth = 15,
    PointHeight = 15
};

// Error bar series
ErrorBarSeries errorBarSeries = new ErrorBarSeries()
{
    ItemsSource = new ViewModel().EnergyProductions,
    XBindingPath = "ID",
    YBindingPath = "Coal",
    HorizontalErrorValue = 0.5,
    VerticalErrorValue = 50
};

chart.Series.Add(scatterSeries);
chart.Series.Add(errorBarSeries);
this.Content = chart;
```

### Error Bar Modes

The `Mode` property controls whether error bars display horizontally, vertically, or both:

**Both (Default):**

```xml
<chart:ErrorBarSeries ItemsSource="{Binding EnergyProductions}"
                      XBindingPath="ID"
                      YBindingPath="Coal"
                      HorizontalErrorValue="0.5"
                      VerticalErrorValue="50"
                      Mode="Both"/>
```

**Horizontal Only:**

```xml
<chart:ErrorBarSeries ItemsSource="{Binding EnergyProductions}"
                      XBindingPath="ID"
                      YBindingPath="Coal"
                      HorizontalErrorValue="0.5"
                      Mode="Horizontal"/>
```

**Vertical Only:**

```xml
<chart:ErrorBarSeries ItemsSource="{Binding EnergyProductions}"
                      XBindingPath="ID"
                      YBindingPath="Coal"
                      VerticalErrorValue="50"
                      Mode="Vertical"/>
```

### Error Bar Direction

Control which direction(s) the error bars extend:

```xml
<chart:ErrorBarSeries ItemsSource="{Binding EnergyProductions}"
                      XBindingPath="ID"
                      YBindingPath="Coal"
                      HorizontalErrorValue="0.5"
                      VerticalErrorValue="50"
                      HorizontalDirection="Plus"
                      VerticalDirection="Both"/>
```

```csharp
ErrorBarSeries errorBarSeries = new ErrorBarSeries()
{
    ItemsSource = new ViewModel().EnergyProductions,
    XBindingPath = "ID",
    YBindingPath = "Coal",
    HorizontalErrorValue = 0.5,
    VerticalErrorValue = 50,
    HorizontalDirection = ErrorBarDirection.Plus,
    VerticalDirection = ErrorBarDirection.Both
};
```

**Available Directions:**
- `Both`: Show positive and negative error bars
- `Plus`: Show only positive error bars
- `Minus`: Show only negative error bars

### Error Bar Types

The `Type` property defines how error values are calculated:

**1. Fixed (Default):**

```xml
<chart:ErrorBarSeries ItemsSource="{Binding EnergyProductions}"
                      XBindingPath="ID"
                      YBindingPath="Coal"
                      HorizontalErrorValue="0.5"
                      VerticalErrorValue="50"
                      Type="Fixed"/>
```

**2. Percentage:**

Error calculated as a percentage of the data value:

```xml
<chart:ErrorBarSeries ItemsSource="{Binding EnergyProductions}"
                      XBindingPath="ID"
                      YBindingPath="Coal"
                      HorizontalErrorValue="5"
                      VerticalErrorValue="10"
                      Type="Percentage"/>
```

**3. StandardDeviation:**

Error calculated using standard deviation:

```xml
<chart:ErrorBarSeries ItemsSource="{Binding EnergyProductions}"
                      XBindingPath="ID"
                      YBindingPath="Coal"
                      VerticalErrorValue="1"
                      Type="StandardDeviation"/>
```

**4. StandardError:**

Error calculated using standard error of the mean:

```xml
<chart:ErrorBarSeries ItemsSource="{Binding EnergyProductions}"
                      XBindingPath="ID"
                      YBindingPath="Coal"
                      VerticalErrorValue="1"
                      Type="StandardError"/>
```

**5. Custom:**

Bind to custom error values from data source:

```xml
<chart:ErrorBarSeries ItemsSource="{Binding EnergyProductions}"
                      XBindingPath="ID"
                      YBindingPath="Coal"
                      Type="Custom"
                      HorizontalErrorPath="HorizontalError"
                      VerticalErrorPath="VerticalError"/>
```

```csharp
public class EnergyModel
{
    public int ID { get; set; }
    public double Coal { get; set; }
    public double HorizontalError { get; set; }
    public double VerticalError { get; set; }
}

ErrorBarSeries errorBarSeries = new ErrorBarSeries()
{
    ItemsSource = new ViewModel().EnergyProductions,
    XBindingPath = "ID",
    YBindingPath = "Coal",
    Type = ErrorBarType.Custom,
    HorizontalErrorPath = "HorizontalError",
    VerticalErrorPath = "VerticalError"
};
```

### Error Bar Customization

**Line Style:**

```xml
<chart:ErrorBarSeries ItemsSource="{Binding EnergyProductions}"
                      XBindingPath="ID"
                      YBindingPath="Coal"
                      VerticalErrorValue="50">
    <chart:ErrorBarSeries.HorizontalLineStyle>
        <chart:ErrorBarLineStyle Stroke="Red" StrokeWidth="2"/>
    </chart:ErrorBarSeries.HorizontalLineStyle>
    
    <chart:ErrorBarSeries.VerticalLineStyle>
        <chart:ErrorBarLineStyle Stroke="Blue" StrokeWidth="2"/>
    </chart:ErrorBarSeries.VerticalLineStyle>
</chart:ErrorBarSeries>
```

```csharp
ErrorBarSeries errorBarSeries = new ErrorBarSeries()
{
    ItemsSource = new ViewModel().EnergyProductions,
    XBindingPath = "ID",
    YBindingPath = "Coal",
    VerticalErrorValue = 50,
    HorizontalLineStyle = new ErrorBarLineStyle()
    {
        Stroke = new SolidColorBrush(Colors.Red),
        StrokeWidth = 2
    },
    VerticalLineStyle = new ErrorBarLineStyle()
    {
        Stroke = new SolidColorBrush(Colors.Blue),
        StrokeWidth = 2
    }
};
```

**Cap Line Style:**

```xml
<chart:ErrorBarSeries ItemsSource="{Binding EnergyProductions}"
                      XBindingPath="ID"
                      YBindingPath="Coal"
                      VerticalErrorValue="50">
    <chart:ErrorBarSeries.HorizontalCapLineStyle>
        <chart:ErrorBarCapLineStyle Stroke="Red" 
                                    StrokeWidth="2"
                                    CapLineSize="10"/>
    </chart:ErrorBarSeries.HorizontalCapLineStyle>
    
    <chart:ErrorBarSeries.VerticalCapLineStyle>
        <chart:ErrorBarCapLineStyle Stroke="Blue" 
                                    StrokeWidth="2"
                                    CapLineSize="10"/>
    </chart:ErrorBarSeries.VerticalCapLineStyle>
</chart:ErrorBarSeries>
```

```csharp
ErrorBarSeries errorBarSeries = new ErrorBarSeries()
{
    ItemsSource = new ViewModel().EnergyProductions,
    XBindingPath = "ID",
    YBindingPath = "Coal",
    VerticalErrorValue = 50,
    HorizontalCapLineStyle = new ErrorBarCapLineStyle()
    {
        Stroke = new SolidColorBrush(Colors.Red),
        StrokeWidth = 2
    },
    VerticalCapLineStyle = new ErrorBarCapLineStyle()
    {
        Stroke = new SolidColorBrush(Colors.Blue),
        StrokeWidth = 2
    }
};
```

## Best Practices

### Box and Whisker Charts

1. **Data Structure**: Ensure Y-values are collections (List, Array) containing multiple data points per category
2. **Quartile Calculation**: Choose appropriate `BoxPlotMode` based on statistical methodology
3. **Outlier Detection**: Keep `ShowOutlier` enabled to identify anomalous data points
4. **Median Display**: Enable `ShowMedian` for clearer distribution understanding
5. **Compare Distributions**: Use box plots to compare multiple datasets side-by-side

### Histogram Charts

1. **Interval Selection**: Choose `HistogramInterval` appropriate to data range and distribution
2. **Bin Count**: Aim for 5-20 bins for optimal visualization (too few: oversimplification, too many: noise)
3. **Normal Curve**: Show distribution curve when data follows normal distribution
4. **Data Volume**: Histograms work best with large datasets (50+ data points)
5. **Axis Configuration**: Use `NumericalAxis` for both X and Y axes

### Error Bar Charts

1. **Main Series Required**: Always add error bars to an existing data series (scatter, line, etc.)
2. **Error Type Selection**: Choose appropriate `Type` based on data characteristics
3. **Value Ranges**: Set `HorizontalErrorValue` and `VerticalErrorValue` appropriately
4. **Direction Control**: Use `HorizontalDirection` and `VerticalDirection` to reduce visual clutter
5. **Custom Errors**: Use `Type="Custom"` when error values vary per data point
6. **Visual Clarity**: Customize line styles to distinguish error bars from main series

### Common Gotchas

**Box and Whisker:**
- Y-binding must be a collection type, not a single value
- Empty or null collections will cause rendering issues
- Outlier calculation depends on `BoxPlotMode` setting

**Histogram:**
- Small intervals may create excessive bins and poor performance
- Large intervals may hide important distribution details
- Negative values require appropriate axis minimum settings

**Error Bar:**
- Must match ItemsSource of the associated main series
- Error values must be positive (direction handled by `HorizontalDirection`/`VerticalDirection`)
- Custom type requires valid binding paths in data model

### Performance Optimization

1. **Data Volume**: Limit box plot categories to <20 for optimal performance
2. **Histogram Bins**: Keep bin count reasonable (avoid extremely small intervals)
3. **Error Bars**: Consider showing only vertical or horizontal error bars when appropriate
4. **Update Frequency**: Avoid frequent data source updates for statistical charts
5. **Rendering**: Use `EnableAnimation="False"` for large datasets
