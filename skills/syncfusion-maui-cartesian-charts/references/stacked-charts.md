# Stacked Charts

## Table of Contents
- [Overview](#overview)
- [Stacked Area Chart](#stacked-area-chart)
- [Stacked Column Chart](#stacked-column-chart)
- [Stacked Line Chart](#stacked-line-chart)
- [100% Stacked Area Chart](#100-stacked-area-chart)
- [100% Stacked Column Chart](#100-stacked-column-chart)
- [100% Stacked Line Chart](#100-stacked-line-chart)
- [Grouping Series](#grouping-series)
- [Best Practices](#best-practices)

## Overview

Stacked charts display multiple data series stacked on top of each other to show the cumulative value. They are ideal for comparing the contribution of each series to the total across categories.

**Types of Stacked Charts:**
- **Stacked Area**: Shows cumulative area trends
- **Stacked Column**: Displays cumulative column values
- **Stacked Line**: Shows cumulative line trends
- **100% Variants**: Normalizes values to show percentage contribution

## Stacked Area Chart

Stacked area charts visually represent data points layered on top of each other to indicate cumulative values over a continuous range.

### Basic Implementation

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:StackingAreaSeries ItemsSource="{Binding Data1}"
                              XBindingPath="Year"
                              YBindingPath="Value"
                              Label="Product A"/>
    
    <chart:StackingAreaSeries ItemsSource="{Binding Data2}"
                              XBindingPath="Year"
                              YBindingPath="Value"
                              Label="Product B"/>
    
    <chart:StackingAreaSeries ItemsSource="{Binding Data3}"
                              XBindingPath="Year"
                              YBindingPath="Value"
                              Label="Product C"/>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();

CategoryAxis primaryAxis = new CategoryAxis();
chart.XAxes.Add(primaryAxis);

NumericalAxis secondaryAxis = new NumericalAxis();
chart.YAxes.Add(secondaryAxis);

ViewModel viewModel = new ViewModel();

StackingAreaSeries series1 = new StackingAreaSeries()
{
    ItemsSource = viewModel.Data1,
    XBindingPath = "Year",
    YBindingPath = "Value",
    Label = "Product A"
};

StackingAreaSeries series2 = new StackingAreaSeries()
{
    ItemsSource = viewModel.Data2,
    XBindingPath = "Year",
    YBindingPath = "Value",
    Label = "Product B"
};

StackingAreaSeries series3 = new StackingAreaSeries()
{
    ItemsSource = viewModel.Data3,
    XBindingPath = "Year",
    YBindingPath = "Value",
    Label = "Product C"
};

chart.Series.Add(series1);
chart.Series.Add(series2);
chart.Series.Add(series3);
this.Content = chart;
```

### Enable Markers

Display markers at data points:

```xml
<chart:StackingAreaSeries ItemsSource="{Binding Data1}"
                          XBindingPath="Year"
                          YBindingPath="Value"
                          ShowMarkers="True"/>
```

```csharp
StackingAreaSeries series = new StackingAreaSeries()
{
    ItemsSource = new ViewModel().Data1,
    XBindingPath = "Year",
    YBindingPath = "Value",
    ShowMarkers = true
};
```

### Marker Customization

```xml
<chart:StackingAreaSeries ItemsSource="{Binding Data1}"
                          XBindingPath="Year"
                          YBindingPath="Value"
                          ShowMarkers="True">
    <chart:StackingAreaSeries.MarkerSettings>
        <chart:ChartMarkerSettings Type="Diamond"
                                   Fill="LightBlue"
                                   Stroke="Blue"
                                   StrokeWidth="2"
                                   Height="10"
                                   Width="10"/>
    </chart:StackingAreaSeries.MarkerSettings>
</chart:StackingAreaSeries>
```

```csharp
ChartMarkerSettings markerSettings = new ChartMarkerSettings()
{
    Type = ShapeType.Diamond,
    Fill = Colors.LightBlue,
    Stroke = Colors.Blue,
    StrokeWidth = 2,
    Height = 10,
    Width = 10
};

StackingAreaSeries series = new StackingAreaSeries()
{
    ItemsSource = new ViewModel().Data1,
    XBindingPath = "Year",
    YBindingPath = "Value",
    ShowMarkers = true,
    MarkerSettings = markerSettings
};
```

## Stacked Column Chart

Stacked column charts represent data values in vertical columns stacked on each other to show cumulative values.

### Basic Implementation

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:StackingColumnSeries ItemsSource="{Binding Data1}"
                                XBindingPath="Category"
                                YBindingPath="Value"
                                Label="Q1"/>
    
    <chart:StackingColumnSeries ItemsSource="{Binding Data2}"
                                XBindingPath="Category"
                                YBindingPath="Value"
                                Label="Q2"/>
    
    <chart:StackingColumnSeries ItemsSource="{Binding Data3}"
                                XBindingPath="Category"
                                YBindingPath="Value"
                                Label="Q3"/>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();

CategoryAxis primaryAxis = new CategoryAxis();
chart.XAxes.Add(primaryAxis);

NumericalAxis secondaryAxis = new NumericalAxis();
chart.YAxes.Add(secondaryAxis);

StackingColumnSeries series1 = new StackingColumnSeries()
{
    ItemsSource = new ViewModel().Data1,
    XBindingPath = "Category",
    YBindingPath = "Value",
    Label = "Q1"
};

StackingColumnSeries series2 = new StackingColumnSeries()
{
    ItemsSource = new ViewModel().Data2,
    XBindingPath = "Category",
    YBindingPath = "Value",
    Label = "Q2"
};

StackingColumnSeries series3 = new StackingColumnSeries()
{
    ItemsSource = new ViewModel().Data3,
    XBindingPath = "Category",
    YBindingPath = "Value",
    Label = "Q3"
};

chart.Series.Add(series1);
chart.Series.Add(series2);
chart.Series.Add(series3);
this.Content = chart;
```

### Appearance Customization

```xml
<chart:StackingColumnSeries ItemsSource="{Binding Data1}"
                            XBindingPath="Category"
                            YBindingPath="Value"
                            Spacing="0.2"
                            Width="0.7"
                            CornerRadius="5"
                            Stroke="Black"
                            StrokeWidth="1"/>
```

```csharp
StackingColumnSeries series = new StackingColumnSeries()
{
    ItemsSource = new ViewModel().Data1,
    XBindingPath = "Category",
    YBindingPath = "Value",
    Spacing = 0.2,        // Space between segments (0-1)
    Width = 0.7,          // Column width (0-1)
    CornerRadius = new CornerRadius(5),
    Stroke = new SolidColorBrush(Colors.Black),
    StrokeWidth = 1
};
```

## Stacked Line Chart

Stacked line charts display multiple line series stacked on top of each other showing cumulative trends.

### Basic Implementation

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:StackingLineSeries ItemsSource="{Binding Data1}"
                              XBindingPath="Month"
                              YBindingPath="Value"
                              Label="Revenue"/>
    
    <chart:StackingLineSeries ItemsSource="{Binding Data2}"
                              XBindingPath="Month"
                              YBindingPath="Value"
                              Label="Cost"/>
    
    <chart:StackingLineSeries ItemsSource="{Binding Data3}"
                              XBindingPath="Month"
                              YBindingPath="Value"
                              Label="Profit"/>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();

CategoryAxis primaryAxis = new CategoryAxis();
chart.XAxes.Add(primaryAxis);

NumericalAxis secondaryAxis = new NumericalAxis();
chart.YAxes.Add(secondaryAxis);

StackingLineSeries series1 = new StackingLineSeries()
{
    ItemsSource = new ViewModel().Data1,
    XBindingPath = "Month",
    YBindingPath = "Value",
    Label = "Revenue"
};

StackingLineSeries series2 = new StackingLineSeries()
{
    ItemsSource = new ViewModel().Data2,
    XBindingPath = "Month",
    YBindingPath = "Value",
    Label = "Cost"
};

StackingLineSeries series3 = new StackingLineSeries()
{
    ItemsSource = new ViewModel().Data3,
    XBindingPath = "Month",
    YBindingPath = "Value",
    Label = "Profit"
};

chart.Series.Add(series1);
chart.Series.Add(series2);
chart.Series.Add(series3);
this.Content = chart;
```

### Dashed Lines

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.Resources>
        <DoubleCollection x:Key="dashArray">
            <x:Double>5</x:Double>
            <x:Double>3</x:Double>
        </DoubleCollection>
    </chart:SfCartesianChart.Resources>
    
    <chart:StackingLineSeries ItemsSource="{Binding Data1}"
                              XBindingPath="Month"
                              YBindingPath="Value"
                              StrokeDashArray="{StaticResource dashArray}"/>
</chart:SfCartesianChart>
```

```csharp
DoubleCollection dashArray = new DoubleCollection();
dashArray.Add(5);
dashArray.Add(3);

StackingLineSeries series = new StackingLineSeries()
{
    ItemsSource = new ViewModel().Data1,
    XBindingPath = "Month",
    YBindingPath = "Value",
    StrokeDashArray = dashArray
};
```

### Markers with Customization

```xml
<chart:StackingLineSeries ItemsSource="{Binding Data1}"
                          XBindingPath="Month"
                          YBindingPath="Value"
                          ShowMarkers="True">
    <chart:StackingLineSeries.MarkerSettings>
        <chart:ChartMarkerSettings Type="Circle"
                                   Fill="White"
                                   Stroke="Blue"
                                   StrokeWidth="2"
                                   Height="8"
                                   Width="8"/>
    </chart:StackingLineSeries.MarkerSettings>
</chart:StackingLineSeries>
```

```csharp
ChartMarkerSettings markerSettings = new ChartMarkerSettings()
{
    Type = ShapeType.Circle,
    Fill = Colors.White,
    Stroke = Colors.Blue,
    StrokeWidth = 2,
    Height = 8,
    Width = 8
};

StackingLineSeries series = new StackingLineSeries()
{
    ItemsSource = new ViewModel().Data1,
    XBindingPath = "Month",
    YBindingPath = "Value",
    ShowMarkers = true,
    MarkerSettings = markerSettings
};
```

## 100% Stacked Area Chart

100% stacked area charts normalize values to show the percentage contribution of each series to the total.

### Basic Implementation

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:StackingArea100Series ItemsSource="{Binding Data1}"
                                 XBindingPath="Year"
                                 YBindingPath="Value"
                                 Label="Desktop"/>
    
    <chart:StackingArea100Series ItemsSource="{Binding Data2}"
                                 XBindingPath="Year"
                                 YBindingPath="Value"
                                 Label="Mobile"/>
    
    <chart:StackingArea100Series ItemsSource="{Binding Data3}"
                                 XBindingPath="Year"
                                 YBindingPath="Value"
                                 Label="Tablet"/>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();

CategoryAxis primaryAxis = new CategoryAxis();
chart.XAxes.Add(primaryAxis);

NumericalAxis secondaryAxis = new NumericalAxis();
chart.YAxes.Add(secondaryAxis);

StackingArea100Series series1 = new StackingArea100Series()
{
    ItemsSource = new ViewModel().Data1,
    XBindingPath = "Year",
    YBindingPath = "Value",
    Label = "Desktop"
};

StackingArea100Series series2 = new StackingArea100Series()
{
    ItemsSource = new ViewModel().Data2,
    XBindingPath = "Year",
    YBindingPath = "Value",
    Label = "Mobile"
};

StackingArea100Series series3 = new StackingArea100Series()
{
    ItemsSource = new ViewModel().Data3,
    XBindingPath = "Year",
    YBindingPath = "Value",
    Label = "Tablet"
};

chart.Series.Add(series1);
chart.Series.Add(series2);
chart.Series.Add(series3);
this.Content = chart;
```

## 100% Stacked Column Chart

100% stacked column charts show the proportion of different categories within a single column, always totaling 100%.

### Basic Implementation

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:StackingColumn100Series ItemsSource="{Binding Data1}"
                                   XBindingPath="Region"
                                   YBindingPath="Value"
                                   Label="North"/>
    
    <chart:StackingColumn100Series ItemsSource="{Binding Data2}"
                                   XBindingPath="Region"
                                   YBindingPath="Value"
                                   Label="South"/>
    
    <chart:StackingColumn100Series ItemsSource="{Binding Data3}"
                                   XBindingPath="Region"
                                   YBindingPath="Value"
                                   Label="East"/>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();

CategoryAxis primaryAxis = new CategoryAxis();
chart.XAxes.Add(primaryAxis);

NumericalAxis secondaryAxis = new NumericalAxis();
chart.YAxes.Add(secondaryAxis);

StackingColumn100Series series1 = new StackingColumn100Series()
{
    ItemsSource = new ViewModel().Data1,
    XBindingPath = "Region",
    YBindingPath = "Value",
    Label = "North"
};

StackingColumn100Series series2 = new StackingColumn100Series()
{
    ItemsSource = new ViewModel().Data2,
    XBindingPath = "Region",
    YBindingPath = "Value",
    Label = "South"
};

StackingColumn100Series series3 = new StackingColumn100Series()
{
    ItemsSource = new ViewModel().Data3,
    XBindingPath = "Region",
    YBindingPath = "Value",
    Label = "East"
};

chart.Series.Add(series1);
chart.Series.Add(series2);
chart.Series.Add(series3);
this.Content = chart;
```

## 100% Stacked Line Chart

100% stacked line charts display percentage contribution of each series over time or categories.

### Basic Implementation

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:StackingLine100Series ItemsSource="{Binding Data1}"
                                 XBindingPath="Month"
                                 YBindingPath="Value"
                                 Label="Category A"/>
    
    <chart:StackingLine100Series ItemsSource="{Binding Data2}"
                                 XBindingPath="Month"
                                 YBindingPath="Value"
                                 Label="Category B"/>
    
    <chart:StackingLine100Series ItemsSource="{Binding Data3}"
                                 XBindingPath="Month"
                                 YBindingPath="Value"
                                 Label="Category C"/>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();

CategoryAxis primaryAxis = new CategoryAxis();
chart.XAxes.Add(primaryAxis);

NumericalAxis secondaryAxis = new NumericalAxis();
chart.YAxes.Add(secondaryAxis);

StackingLine100Series series1 = new StackingLine100Series()
{
    ItemsSource = new ViewModel().Data1,
    XBindingPath = "Month",
    YBindingPath = "Value",
    Label = "Category A"
};

StackingLine100Series series2 = new StackingLine100Series()
{
    ItemsSource = new ViewModel().Data2,
    XBindingPath = "Month",
    YBindingPath = "Value",
    Label = "Category B"
};

StackingLine100Series series3 = new StackingLine100Series()
{
    ItemsSource = new ViewModel().Data3,
    XBindingPath = "Month",
    YBindingPath = "Value",
    Label = "Category C"
};

chart.Series.Add(series1);
chart.Series.Add(series2);
chart.Series.Add(series3);
this.Content = chart;
```

### Dashed 100% Stacked Line

```xml
<chart:SfCartesianChart.Resources>
    <DoubleCollection x:Key="dashArray">
        <x:Double>5</x:Double>
        <x:Double>2</x:Double>
    </DoubleCollection>
</chart:SfCartesianChart.Resources>

<chart:StackingLine100Series ItemsSource="{Binding Data1}"
                             XBindingPath="Month"
                             YBindingPath="Value"
                             StrokeDashArray="{StaticResource dashArray}"/>
```

```csharp
DoubleCollection dashArray = new DoubleCollection { 5, 2 };

StackingLine100Series series = new StackingLine100Series()
{
    ItemsSource = new ViewModel().Data1,
    XBindingPath = "Month",
    YBindingPath = "Value",
    StrokeDashArray = dashArray
};
```

## Grouping Series

The `GroupingLabel` property allows you to create multiple independent stacked groups within the same chart.

### Basic Grouping

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <!-- Group One -->
    <chart:StackingColumnSeries ItemsSource="{Binding Data1}"
                                XBindingPath="Category"
                                YBindingPath="Value"
                                GroupingLabel="GroupOne"
                                Label="Series 1"/>
    
    <chart:StackingColumnSeries ItemsSource="{Binding Data2}"
                                XBindingPath="Category"
                                YBindingPath="Value"
                                GroupingLabel="GroupOne"
                                Label="Series 2"/>
    
    <!-- Group Two -->
    <chart:StackingColumnSeries ItemsSource="{Binding Data3}"
                                XBindingPath="Category"
                                YBindingPath="Value"
                                GroupingLabel="GroupTwo"
                                Label="Series 3"/>
    
    <chart:StackingColumnSeries ItemsSource="{Binding Data4}"
                                XBindingPath="Category"
                                YBindingPath="Value"
                                GroupingLabel="GroupTwo"
                                Label="Series 4"/>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();

CategoryAxis primaryAxis = new CategoryAxis();
chart.XAxes.Add(primaryAxis);

NumericalAxis secondaryAxis = new NumericalAxis();
chart.YAxes.Add(secondaryAxis);

// Group One
StackingColumnSeries series1 = new StackingColumnSeries()
{
    ItemsSource = new ViewModel().Data1,
    XBindingPath = "Category",
    YBindingPath = "Value",
    GroupingLabel = "GroupOne",
    Label = "Series 1"
};

StackingColumnSeries series2 = new StackingColumnSeries()
{
    ItemsSource = new ViewModel().Data2,
    XBindingPath = "Category",
    YBindingPath = "Value",
    GroupingLabel = "GroupOne",
    Label = "Series 2"
};

// Group Two
StackingColumnSeries series3 = new StackingColumnSeries()
{
    ItemsSource = new ViewModel().Data3,
    XBindingPath = "Category",
    YBindingPath = "Value",
    GroupingLabel = "GroupTwo",
    Label = "Series 3"
};

StackingColumnSeries series4 = new StackingColumnSeries()
{
    ItemsSource = new ViewModel().Data4,
    XBindingPath = "Category",
    YBindingPath = "Value",
    GroupingLabel = "GroupTwo",
    Label = "Series 4"
};

chart.Series.Add(series1);
chart.Series.Add(series2);
chart.Series.Add(series3);
chart.Series.Add(series4);
this.Content = chart;
```

### Grouping 100% Series

```xml
<chart:StackingColumn100Series ItemsSource="{Binding Data1}"
                               XBindingPath="Category"
                               YBindingPath="Value"
                               GroupingLabel="Sales"
                               Label="Q1"/>

<chart:StackingColumn100Series ItemsSource="{Binding Data2}"
                               XBindingPath="Category"
                               YBindingPath="Value"
                               GroupingLabel="Sales"
                               Label="Q2"/>

<chart:StackingColumn100Series ItemsSource="{Binding Data3}"
                               XBindingPath="Category"
                               YBindingPath="Value"
                               GroupingLabel="Marketing"
                               Label="Q1"/>

<chart:StackingColumn100Series ItemsSource="{Binding Data4}"
                               XBindingPath="Category"
                               YBindingPath="Value"
                               GroupingLabel="Marketing"
                               Label="Q2"/>
```

## Best Practices

### Data Structure

Create a proper ViewModel for stacked chart data:

```csharp
public class ChartDataModel
{
    public string Category { get; set; }
    public double Value { get; set; }
}

public class ViewModel
{
    public ObservableCollection<ChartDataModel> Data1 { get; set; }
    public ObservableCollection<ChartDataModel> Data2 { get; set; }
    public ObservableCollection<ChartDataModel> Data3 { get; set; }
    
    public ViewModel()
    {
        Data1 = new ObservableCollection<ChartDataModel>
        {
            new ChartDataModel { Category = "Jan", Value = 35 },
            new ChartDataModel { Category = "Feb", Value = 28 },
            new ChartDataModel { Category = "Mar", Value = 42 }
        };
        
        Data2 = new ObservableCollection<ChartDataModel>
        {
            new ChartDataModel { Category = "Jan", Value = 48 },
            new ChartDataModel { Category = "Feb", Value = 52 },
            new ChartDataModel { Category = "Mar", Value = 39 }
        };
        
        Data3 = new ObservableCollection<ChartDataModel>
        {
            new ChartDataModel { Category = "Jan", Value = 27 },
            new ChartDataModel { Category = "Feb", Value = 33 },
            new ChartDataModel { Category = "Mar", Value = 38 }
        };
    }
}
```

### When to Use Each Type

**Stacked Area:**
- Show cumulative trends over time
- Visualize contribution to total across continuous data
- Compare multiple data series' contribution

**Stacked Column:**
- Compare category totals and individual contributions
- Show discrete data points
- Best for fewer categories (< 10)

**Stacked Line:**
- Track cumulative trends with clear data point identification
- Show continuous data with distinct series
- Useful when markers are important

**100% Stacked Charts:**
- Show percentage distribution
- Compare proportions across categories
- When actual values are less important than relative contributions

### Common Guidelines

1. **Series Count**: Limit to 3-5 series for readability
2. **Color Selection**: Use distinguishable colors with sufficient contrast
3. **Labels**: Always provide meaningful labels for each series
4. **X-Axis Consistency**: Ensure all series have identical X values
5. **Data Order**: Add series in logical order (bottom to top)
6. **Grouping**: Use when comparing multiple independent stacks

### Performance Optimization

```csharp
// Good: Reuse collections
public class ViewModel
{
    public ObservableCollection<ChartDataModel> Data { get; set; }
    
    public ViewModel()
    {
        Data = new ObservableCollection<ChartDataModel>();
        LoadData();
    }
    
    private void LoadData()
    {
        // Load data once
        for (int i = 0; i < 100; i++)
        {
            Data.Add(new ChartDataModel { Category = $"Cat{i}", Value = i * 10 });
        }
    }
}
```

### Common Gotchas

1. **Mismatched X Values**: All stacked series must have matching X-axis values
2. **Negative Values**: Stacked charts handle negatives, but visualization may be confusing
3. **Empty Series**: An empty series will cause gaps in the stack
4. **Grouping Without Label**: If `GroupingLabel` is not set, all series stack together
5. **100% Charts Scale**: Y-axis automatically scales to 0-100, don't set manual range
6. **Marker Overlap**: In stacked charts with many series, markers may overlap

### Styling Complete Example

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis>
            <chart:CategoryAxis.Title>
                <chart:ChartAxisTitle Text="Months"/>
            </chart:CategoryAxis.Title>
        </chart:CategoryAxis>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis>
            <chart:NumericalAxis.Title>
                <chart:ChartAxisTitle Text="Sales (in thousands)"/>
            </chart:NumericalAxis.Title>
        </chart:NumericalAxis>
    </chart:SfCartesianChart.YAxes>
    
    <chart:SfCartesianChart.Legend>
        <chart:ChartLegend/>
    </chart:SfCartesianChart.Legend>
    
    <chart:StackingColumnSeries ItemsSource="{Binding Data1}"
                                XBindingPath="Month"
                                YBindingPath="Value"
                                Label="Product A"
                                Fill="#2E7D32"
                                Width="0.8"
                                Spacing="0.1"/>
    
    <chart:StackingColumnSeries ItemsSource="{Binding Data2}"
                                XBindingPath="Month"
                                YBindingPath="Value"
                                Label="Product B"
                                Fill="#1976D2"
                                Width="0.8"
                                Spacing="0.1"/>
    
    <chart:StackingColumnSeries ItemsSource="{Binding Data3}"
                                XBindingPath="Month"
                                YBindingPath="Value"
                                Label="Product C"
                                Fill="#F57C00"
                                Width="0.8"
                                Spacing="0.1"/>
</chart:SfCartesianChart>
```

### Troubleshooting

**Issue**: Series not stacking properly
- **Solution**: Ensure all series have identical X-axis values and same X-axis type

**Issue**: Gaps in stacked columns
- **Solution**: Check for null or missing data in any series at that X value

**Issue**: 100% chart not showing 100%
- **Solution**: Verify Y-axis is not manually configured with a range

**Issue**: Groups not separating
- **Solution**: Ensure `GroupingLabel` is set with different values for each group

**Issue**: Colors too similar
- **Solution**: Use contrasting colors or apply palette theming
