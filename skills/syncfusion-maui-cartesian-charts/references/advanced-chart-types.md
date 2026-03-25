# Advanced Chart Types

## Table of Contents
- [Overview](#overview)
- [Fast Line Chart](#fast-line-chart)
- [Step Area Chart](#step-area-chart)
- [Step Line Chart](#step-line-chart)
- [Waterfall Chart](#waterfall-chart)
- [Best Practices](#best-practices)

## Overview

Advanced chart types provide specialized visualization capabilities for specific data representation needs. These chart types offer performance optimization, step-based visualization, and financial analysis capabilities.

**Available Advanced Chart Types:**
- **Fast Line**: Optimized for large datasets (thousands of points)
- **Step Area**: Displays stepwise changes with filled area
- **Step Line**: Shows discrete value changes with step patterns
- **Waterfall**: Visualizes cumulative effects of sequential values

## Fast Line Chart

Fast line chart is optimized for rendering large datasets with thousands of data points efficiently. It's ideal for real-time data visualization and performance-critical scenarios.

### Basic Implementation

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:DateTimeAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:FastLineSeries ItemsSource="{Binding LargeData}"
                          XBindingPath="Time"
                          YBindingPath="Value"/>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();

DateTimeAxis primaryAxis = new DateTimeAxis();
chart.XAxes.Add(primaryAxis);

NumericalAxis secondaryAxis = new NumericalAxis();
chart.YAxes.Add(secondaryAxis);

FastLineSeries series = new FastLineSeries()
{
    ItemsSource = new ViewModel().LargeData,
    XBindingPath = "Time",
    YBindingPath = "Value"
};

chart.Series.Add(series);
this.Content = chart;
```

### Performance Data Model

```csharp
public class TimeSeriesData
{
    public DateTime Time { get; set; }
    public double Value { get; set; }
}

public class ViewModel
{
    public ObservableCollection<TimeSeriesData> LargeData { get; set; }
    
    public ViewModel()
    {
        LargeData = new ObservableCollection<TimeSeriesData>();
        GenerateLargeDataset();
    }
    
    private void GenerateLargeDataset()
    {
        Random random = new Random();
        DateTime startDate = DateTime.Now.AddDays(-1000);
        
        // Generate 10,000 data points
        for (int i = 0; i < 10000; i++)
        {
            LargeData.Add(new TimeSeriesData
            {
                Time = startDate.AddMinutes(i),
                Value = random.NextDouble() * 100
            });
        }
    }
}
```

### Dashed Fast Line

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.Resources>
        <DoubleCollection x:Key="dashArray">
            <x:Double>5</x:Double>
            <x:Double>2</x:Double>
        </DoubleCollection>
    </chart:SfCartesianChart.Resources>
    
    <chart:SfCartesianChart.XAxes>
        <chart:DateTimeAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:FastLineSeries ItemsSource="{Binding LargeData}"
                          XBindingPath="Time"
                          YBindingPath="Value"
                          StrokeDashArray="{StaticResource dashArray}"/>
</chart:SfCartesianChart>
```

```csharp
DoubleCollection dashArray = new DoubleCollection { 5, 2 };

FastLineSeries series = new FastLineSeries()
{
    ItemsSource = new ViewModel().LargeData,
    XBindingPath = "Time",
    YBindingPath = "Value",
    StrokeDashArray = dashArray
};
```

### Anti-Aliasing

Enable anti-aliasing to reduce jagged edges:

```xml
<chart:FastLineSeries ItemsSource="{Binding LargeData}"
                      XBindingPath="Time"
                      YBindingPath="Value"
                      EnableAntiAliasing="True"/>
```

```csharp
FastLineSeries series = new FastLineSeries()
{
    ItemsSource = new ViewModel().LargeData,
    XBindingPath = "Time",
    YBindingPath = "Value",
    EnableAntiAliasing = true
};
```

**Note**: Enabling anti-aliasing may slightly reduce performance but improves visual quality.

## Step Area Chart

Step area charts display data changes over time with horizontal and vertical lines creating steps, with the area beneath filled.

### Basic Implementation

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:DateTimeAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:StepAreaSeries ItemsSource="{Binding ProductionData}"
                          XBindingPath="Month"
                          YBindingPath="Units"/>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();

DateTimeAxis primaryAxis = new DateTimeAxis();
chart.XAxes.Add(primaryAxis);

NumericalAxis secondaryAxis = new NumericalAxis();
chart.YAxes.Add(secondaryAxis);

StepAreaSeries series = new StepAreaSeries()
{
    ItemsSource = new ViewModel().ProductionData,
    XBindingPath = "Month",
    YBindingPath = "Units"
};

chart.Series.Add(series);
this.Content = chart;
```

### Data Model

```csharp
public class ProductionModel
{
    public DateTime Month { get; set; }
    public double Units { get; set; }
}

public class ViewModel
{
    public ObservableCollection<ProductionModel> ProductionData { get; set; }
    
    public ViewModel()
    {
        ProductionData = new ObservableCollection<ProductionModel>
        {
            new ProductionModel { Month = new DateTime(2024, 1, 1), Units = 1200 },
            new ProductionModel { Month = new DateTime(2024, 2, 1), Units = 1200 },
            new ProductionModel { Month = new DateTime(2024, 3, 1), Units = 1500 },
            new ProductionModel { Month = new DateTime(2024, 4, 1), Units = 1500 },
            new ProductionModel { Month = new DateTime(2024, 5, 1), Units = 1800 },
            new ProductionModel { Month = new DateTime(2024, 6, 1), Units = 1800 }
        };
    }
}
```

### Enable Markers

```xml
<chart:StepAreaSeries ItemsSource="{Binding ProductionData}"
                      XBindingPath="Month"
                      YBindingPath="Units"
                      ShowMarkers="True"/>
```

```csharp
StepAreaSeries series = new StepAreaSeries()
{
    ItemsSource = new ViewModel().ProductionData,
    XBindingPath = "Month",
    YBindingPath = "Units",
    ShowMarkers = true
};
```

### Marker Customization

```xml
<chart:StepAreaSeries ItemsSource="{Binding ProductionData}"
                      XBindingPath="Month"
                      YBindingPath="Units"
                      ShowMarkers="True">
    <chart:StepAreaSeries.MarkerSettings>
        <chart:ChartMarkerSettings Type="Diamond"
                                   Fill="Orange"
                                   Stroke="DarkOrange"
                                   StrokeWidth="2"
                                   Height="10"
                                   Width="10"/>
    </chart:StepAreaSeries.MarkerSettings>
</chart:StepAreaSeries>
```

```csharp
ChartMarkerSettings markerSettings = new ChartMarkerSettings()
{
    Type = ShapeType.Diamond,
    Fill = Colors.Orange,
    Stroke = Colors.DarkOrange,
    StrokeWidth = 2,
    Height = 10,
    Width = 10
};

StepAreaSeries series = new StepAreaSeries()
{
    ItemsSource = new ViewModel().ProductionData,
    XBindingPath = "Month",
    YBindingPath = "Units",
    ShowMarkers = true,
    MarkerSettings = markerSettings
};
```

## Step Line Chart

Step line charts connect data points with horizontal and vertical lines, creating a stepped appearance ideal for showing discrete value changes.

### Basic Implementation

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:DateTimeAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:StepLineSeries ItemsSource="{Binding PriceData}"
                          XBindingPath="Date"
                          YBindingPath="Price"
                          Label="Product A"/>
    
    <chart:StepLineSeries ItemsSource="{Binding PriceData2}"
                          XBindingPath="Date"
                          YBindingPath="Price"
                          Label="Product B"/>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();

DateTimeAxis primaryAxis = new DateTimeAxis();
chart.XAxes.Add(primaryAxis);

NumericalAxis secondaryAxis = new NumericalAxis();
chart.YAxes.Add(secondaryAxis);

StepLineSeries series1 = new StepLineSeries()
{
    ItemsSource = new ViewModel().PriceData,
    XBindingPath = "Date",
    YBindingPath = "Price",
    Label = "Product A"
};

StepLineSeries series2 = new StepLineSeries()
{
    ItemsSource = new ViewModel().PriceData2,
    XBindingPath = "Date",
    YBindingPath = "Price",
    Label = "Product B"
};

chart.Series.Add(series1);
chart.Series.Add(series2);
this.Content = chart;
```

### Dashed Step Line

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.Resources>
        <DoubleCollection x:Key="dashArray">
            <x:Double>5</x:Double>
            <x:Double>3</x:Double>
        </DoubleCollection>
    </chart:SfCartesianChart.Resources>
    
    <chart:SfCartesianChart.XAxes>
        <chart:DateTimeAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:StepLineSeries ItemsSource="{Binding PriceData}"
                          XBindingPath="Date"
                          YBindingPath="Price"
                          StrokeDashArray="{StaticResource dashArray}"/>
</chart:SfCartesianChart>
```

```csharp
DoubleCollection dashArray = new DoubleCollection { 5, 3 };

StepLineSeries series = new StepLineSeries()
{
    ItemsSource = new ViewModel().PriceData,
    XBindingPath = "Date",
    YBindingPath = "Price",
    StrokeDashArray = dashArray
};
```

### Vertical Step Line

Render step line vertically using the `IsTransposed` property:

```xml
<chart:SfCartesianChart IsTransposed="True">
    <chart:SfCartesianChart.XAxes>
        <chart:DateTimeAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:StepLineSeries ItemsSource="{Binding PriceData}"
                          XBindingPath="Date"
                          YBindingPath="Price"/>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();
chart.IsTransposed = true;

DateTimeAxis primaryAxis = new DateTimeAxis();
chart.XAxes.Add(primaryAxis);

NumericalAxis secondaryAxis = new NumericalAxis();
chart.YAxes.Add(secondaryAxis);

StepLineSeries series = new StepLineSeries()
{
    ItemsSource = new ViewModel().PriceData,
    XBindingPath = "Date",
    YBindingPath = "Price"
};

chart.Series.Add(series);
this.Content = chart;
```

## Waterfall Chart

Waterfall charts visualize the cumulative effect of sequential positive and negative values, commonly used in financial analysis.

### Basic Implementation

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:WaterfallSeries ItemsSource="{Binding FinancialData}"
                           XBindingPath="Category"
                           YBindingPath="Value"
                           SummaryBindingPath="IsSummary"
                           AllowAutoSum="True"
                           ShowConnectorLine="True"
                           NegativePointsBrush="Red"
                           SummaryPointsBrush="RoyalBlue"/>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();

CategoryAxis primaryAxis = new CategoryAxis();
chart.XAxes.Add(primaryAxis);

NumericalAxis secondaryAxis = new NumericalAxis();
chart.YAxes.Add(secondaryAxis);

WaterfallSeries series = new WaterfallSeries()
{
    ItemsSource = new ViewModel().FinancialData,
    XBindingPath = "Category",
    YBindingPath = "Value",
    SummaryBindingPath = "IsSummary",
    AllowAutoSum = true,
    ShowConnectorLine = true,
    NegativePointsBrush = new SolidColorBrush(Colors.Red),
    SummaryPointsBrush = new SolidColorBrush(Colors.RoyalBlue)
};

chart.Series.Add(series);
this.Content = chart;
```

### Data Model for Waterfall

```csharp
public class FinancialModel
{
    public string Category { get; set; }
    public double Value { get; set; }
    public bool IsSummary { get; set; }
}

public class ViewModel
{
    public ObservableCollection<FinancialModel> FinancialData { get; set; }
    
    public ViewModel()
    {
        FinancialData = new ObservableCollection<FinancialModel>
        {
            new FinancialModel { Category = "Sales", Value = 10000, IsSummary = false },
            new FinancialModel { Category = "Marketing", Value = -2000, IsSummary = false },
            new FinancialModel { Category = "Development", Value = -3000, IsSummary = false },
            new FinancialModel { Category = "Operations", Value = -1500, IsSummary = false },
            new FinancialModel { Category = "Q1 Total", Value = 3500, IsSummary = true },
            new FinancialModel { Category = "Support", Value = -500, IsSummary = false },
            new FinancialModel { Category = "Net Total", Value = 3000, IsSummary = true }
        };
    }
}
```

### Waterfall Properties

**AllowAutoSum**: Automatically calculates cumulative totals

```xml
<chart:WaterfallSeries ItemsSource="{Binding FinancialData}"
                       XBindingPath="Category"
                       YBindingPath="Value"
                       AllowAutoSum="True"
                       SummaryBindingPath="IsSummary"/>
```

```csharp
WaterfallSeries series = new WaterfallSeries()
{
    ItemsSource = new ViewModel().FinancialData,
    XBindingPath = "Category",
    YBindingPath = "Value",
    AllowAutoSum = true,  // Enable automatic sum calculation
    SummaryBindingPath = "IsSummary"
};
```

**ShowConnectorLine**: Display/hide connector lines between segments

```xml
<chart:WaterfallSeries ItemsSource="{Binding FinancialData}"
                       XBindingPath="Category"
                       YBindingPath="Value"
                       ShowConnectorLine="True"/>
```

```csharp
WaterfallSeries series = new WaterfallSeries()
{
    ItemsSource = new ViewModel().FinancialData,
    XBindingPath = "Category",
    YBindingPath = "Value",
    ShowConnectorLine = true  // Default is true
};
```

### Connector Line Customization

```xml
<chart:WaterfallSeries ItemsSource="{Binding FinancialData}"
                       XBindingPath="Category"
                       YBindingPath="Value">
    <chart:WaterfallSeries.ConnectorLineStyle>
        <chart:ChartLineStyle Stroke="DarkViolet"
                              StrokeWidth="2"/>
    </chart:WaterfallSeries.ConnectorLineStyle>
</chart:WaterfallSeries>
```

```csharp
ChartLineStyle connectorStyle = new ChartLineStyle()
{
    Stroke = new SolidColorBrush(Colors.DarkViolet),
    StrokeWidth = 2
};

WaterfallSeries series = new WaterfallSeries()
{
    ItemsSource = new ViewModel().FinancialData,
    XBindingPath = "Category",
    YBindingPath = "Value",
    ConnectorLineStyle = connectorStyle
};
```

### Complete Waterfall Example

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis>
            <chart:NumericalAxis.Title>
                <chart:ChartAxisTitle Text="Amount ($)"/>
            </chart:NumericalAxis.Title>
        </chart:NumericalAxis>
    </chart:SfCartesianChart.YAxes>
    
    <chart:WaterfallSeries ItemsSource="{Binding FinancialData}"
                           XBindingPath="Category"
                           YBindingPath="Value"
                           SummaryBindingPath="IsSummary"
                           AllowAutoSum="True"
                           ShowConnectorLine="True"
                           Fill="#4CAF50"
                           NegativePointsBrush="#F44336"
                           SummaryPointsBrush="#2196F3">
        <chart:WaterfallSeries.ConnectorLineStyle>
            <chart:ChartLineStyle Stroke="Gray" StrokeWidth="1"/>
        </chart:WaterfallSeries.ConnectorLineStyle>
    </chart:WaterfallSeries>
</chart:SfCartesianChart>
```

## Best Practices

### Fast Line Chart

**When to Use:**
- Datasets with 1,000+ data points
- Real-time data visualization
- Performance-critical scenarios
- Time-series data

**Optimization Tips:**
1. Disable animations for large datasets
2. Use DateTimeAxis for time-based data
3. Enable anti-aliasing only when necessary
4. Limit simultaneous series to 2-3
5. Consider data sampling for extremely large datasets (>100k points)

### Step Area Chart

**When to Use:**
- Discrete value changes
- Production/inventory levels
- State transitions
- When exact change points matter

**Guidelines:**
1. Ideal for 10-50 data points
2. Use with CategoryAxis or DateTimeAxis
3. Markers help identify exact change points
4. Combine with tooltips for value details

### Step Line Chart

**When to Use:**
- Price changes
- Status transitions
- Multi-level comparisons
- Discrete value tracking

**Guidelines:**
1. Limit to 3-5 series for clarity
2. Use contrasting colors
3. Dashed lines help distinguish series
4. Consider vertical orientation for readability

### Waterfall Chart

**When to Use:**
- Financial statements
- Variance analysis
- Sequential impact visualization
- Profit and loss analysis

**Guidelines:**
1. Clearly mark summary points
2. Use distinct colors for positive/negative/summary
3. Limit categories to <15 for readability
4. Ensure connector lines are visible
5. Add data labels for exact values

### Common Gotchas

**Fast Line:**
- Markers not supported (performance trade-off)
- No data label support
- Animation may cause lag with large data

**Step Charts:**
- Data points must be ordered by X-value
- Sharp transitions may appear abrupt
- Not suitable for smooth trend analysis

**Waterfall:**
- Summary points must be marked in data model
- Negative values appear below baseline
- Connector line visibility depends on column height
- AutoSum calculation requires proper IsSummary flags

### Performance Comparison

| Chart Type | Optimal Data Points | Update Speed | Memory Usage |
|------------|---------------------|--------------|--------------|
| Fast Line  | 1,000 - 100,000    | Excellent    | Low          |
| Step Area  | 10 - 100           | Good         | Medium       |
| Step Line  | 10 - 200           | Good         | Low          |
| Waterfall  | 5 - 30             | Good         | Medium       |

### Troubleshooting

**Issue**: Fast line chart appears pixelated
- **Solution**: Enable `EnableAntiAliasing="True"`

**Issue**: Step chart transitions too abrupt
- **Solution**: Consider using spline charts for smoother transitions

**Issue**: Waterfall summary not calculating correctly
- **Solution**: Verify `IsSummary` binding path and ensure boolean values are correct

**Issue**: Connector lines not visible in waterfall
- **Solution**: Increase `StrokeWidth` in `ConnectorLineStyle`

**Issue**: Step line appears jagged
- **Solution**: Ensure data is sorted by X-value ascending
