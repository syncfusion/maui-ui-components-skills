# Axis Configuration

## Table of Contents
- [Overview](#overview)
- [Axis Types](#axis-types)
- [Numerical Axis](#numerical-axis)
- [Category Axis](#category-axis)
- [DateTime Axis](#datetime-axis)
- [DateTimeCategory Axis](#datetimecategory-axis)
- [Logarithmic Axis](#logarithmic-axis)
- [Axis Labels](#axis-labels)
- [Axis Title](#axis-title)
- [Custom Labels](#custom-labels)
- [Best Practices](#best-practices)

## Overview

Chart axes are fundamental components that define the coordinate system for plotting data. Cartesian charts typically use two axes: a horizontal (X) axis and a vertical (Y) axis to measure and categorize data points.

**Key Features:**
- Multiple axis types (Numerical, Category, DateTime, Logarithmic)
- Customizable intervals, ranges, and labels
- Support for multiple axes in a single chart
- Extensive styling and formatting options

## Axis Types

Syncfusion .NET MAUI Cartesian Chart supports five axis types:

1. **NumericalAxis**: Plots numerical values with linear scale
2. **CategoryAxis**: Plots categorical data using index-based positioning
3. **DateTimeAxis**: Plots datetime values with timeline scale
4. **DateTimeCategoryAxis**: Combines datetime and category features
5. **LogarithmicAxis**: Plots values on logarithmic scale

## Numerical Axis

NumericalAxis displays numerical values with linear intervals, suitable for continuous data.

### Basic Implementation

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:LineSeries ItemsSource="{Binding Data}"
                     XBindingPath="XValue"
                     YBindingPath="YValue"/>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();

NumericalAxis primaryAxis = new NumericalAxis();
chart.XAxes.Add(primaryAxis);

NumericalAxis secondaryAxis = new NumericalAxis();
chart.YAxes.Add(secondaryAxis);

LineSeries series = new LineSeries()
{
    ItemsSource = new ViewModel().Data,
    XBindingPath = "XValue",
    YBindingPath = "YValue"
};

chart.Series.Add(series);
this.Content = chart;
```

### Interval Customization

```xml
<chart:NumericalAxis Interval="10"
                     Minimum="0"
                     Maximum="100"/>
```

```csharp
NumericalAxis axis = new NumericalAxis()
{
    Interval = 10,
    Minimum = 0,
    Maximum = 100
};
```

### Range Configuration

```xml
<chart:NumericalAxis Maximum="2750"
                     Minimum="250"
                     Interval="250"/>
```

```csharp
NumericalAxis axis = new NumericalAxis()
{
    Maximum = 2750,
    Minimum = 250,
    Interval = 250
};
```

## Category Axis

CategoryAxis is index-based, plotting values based on the data point collection index with equal spacing.

### Basic Implementation

```xml
<chart:SfCartesianChart>
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

CategoryAxis primaryAxis = new CategoryAxis();
chart.XAxes.Add(primaryAxis);

NumericalAxis secondaryAxis = new NumericalAxis();
chart.YAxes.Add(secondaryAxis);

ColumnSeries series = new ColumnSeries()
{
    ItemsSource = new ViewModel().Data,
    XBindingPath = "Category",
    YBindingPath = "Value"
};

chart.Series.Add(series);
this.Content = chart;
```

### Label Placement

```xml
<chart:CategoryAxis LabelPlacement="BetweenTicks"/>
```

```csharp
CategoryAxis axis = new CategoryAxis()
{
    LabelPlacement = LabelPlacement.BetweenTicks
};
```

**Options:**
- `OnTicks`: Labels appear on tick marks (default)
- `BetweenTicks`: Labels appear between tick marks

## DateTime Axis

DateTimeAxis plots datetime values along a timeline with appropriate intervals.

### Basic Implementation

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:DateTimeAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:LineSeries ItemsSource="{Binding TimeSeriesData}"
                     XBindingPath="Date"
                     YBindingPath="Value"/>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();

DateTimeAxis primaryAxis = new DateTimeAxis();
chart.XAxes.Add(primaryAxis);

NumericalAxis secondaryAxis = new NumericalAxis();
chart.YAxes.Add(secondaryAxis);

LineSeries series = new LineSeries()
{
    ItemsSource = new ViewModel().TimeSeriesData,
    XBindingPath = "Date",
    YBindingPath = "Value"
};

chart.Series.Add(series);
this.Content = chart;
```

### Interval Types

```xml
<chart:DateTimeAxis IntervalType="Months"
                    Interval="1"/>
```

```csharp
DateTimeAxis axis = new DateTimeAxis()
{
    IntervalType = DateTimeIntervalType.Months,
    Interval = 1
};
```

**IntervalType Options:**
- `Auto`: Automatically determines interval
- `Years`: Yearly intervals
- `Months`: Monthly intervals
- `Days`: Daily intervals
- `Hours`: Hourly intervals
- `Minutes`: Minute intervals
- `Seconds`: Second intervals
- `Milliseconds`: Millisecond intervals

### Date Format

```xml
<chart:DateTimeAxis LabelFormat="MMM-yyyy"/>
```

```csharp
DateTimeAxis axis = new DateTimeAxis()
{
    LabelFormat = "MMM-yyyy"  // e.g., "Jan-2024"
};
```

### Range Configuration

```xml
<chart:DateTimeAxis Minimum="2024-01-01"
                    Maximum="2024-12-31"
                    IntervalType="Months"
                    Interval="1"/>
```

```csharp
DateTimeAxis axis = new DateTimeAxis()
{
    Minimum = new DateTime(2024, 1, 1),
    Maximum = new DateTime(2024, 12, 31),
    IntervalType = DateTimeIntervalType.Months,
    Interval = 1
};
```

## DateTimeCategory Axis

DateTimeCategoryAxis combines features of both DateTime and Category axes, ideal for datetime data with irregular intervals.

### Basic Implementation

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:DateTimeCategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:LineSeries ItemsSource="{Binding IrregularData}"
                     XBindingPath="Date"
                     YBindingPath="Value"/>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();

DateTimeCategoryAxis primaryAxis = new DateTimeCategoryAxis();
chart.XAxes.Add(primaryAxis);

NumericalAxis secondaryAxis = new NumericalAxis();
chart.YAxes.Add(secondaryAxis);

LineSeries series = new LineSeries()
{
    ItemsSource = new ViewModel().IrregularData,
    XBindingPath = "Date",
    YBindingPath = "Value"
};

chart.Series.Add(series);
this.Content = chart;
```

### Label Format

```xml
<chart:DateTimeCategoryAxis LabelFormat="dd-MMM"
                            Interval="1"/>
```

```csharp
DateTimeCategoryAxis axis = new DateTimeCategoryAxis()
{
    LabelFormat = "dd-MMM",
    Interval = 1
};
```

## Logarithmic Axis

LogarithmicAxis uses logarithmic scale, useful for data with wide value ranges.

### Basic Implementation

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:LogarithmicAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:LineSeries ItemsSource="{Binding ExponentialData}"
                     XBindingPath="X"
                     YBindingPath="Y"/>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();

NumericalAxis primaryAxis = new NumericalAxis();
chart.XAxes.Add(primaryAxis);

LogarithmicAxis secondaryAxis = new LogarithmicAxis();
chart.YAxes.Add(secondaryAxis);

LineSeries series = new LineSeries()
{
    ItemsSource = new ViewModel().ExponentialData,
    XBindingPath = "X",
    YBindingPath = "Y"
};

chart.Series.Add(series);
this.Content = chart;
```

### Logarithmic Base

```xml
<chart:LogarithmicAxis LogarithmicBase="10"/>
```

```csharp
LogarithmicAxis axis = new LogarithmicAxis()
{
    LogarithmicBase = 10  // Default is 10
};
```

## Axis Labels

### Label Style

```xml
<chart:NumericalAxis>
    <chart:NumericalAxis.LabelStyle>
        <chart:ChartAxisLabelStyle TextColor="Blue"
                                   FontSize="14"
                                   FontAttributes="Bold"
                                   Margin="5"/>
    </chart:NumericalAxis.LabelStyle>
</chart:NumericalAxis>
```

```csharp
ChartAxisLabelStyle labelStyle = new ChartAxisLabelStyle()
{
    TextColor = Colors.Blue,
    FontSize = 14,
    FontAttributes = FontAttributes.Bold,
    Margin = new Thickness(5)
};

NumericalAxis axis = new NumericalAxis()
{
    LabelStyle = labelStyle
};
```
### Label Format

Format axis labels using standard format strings:

```xml
<chart:NumericalAxis>
    <chart:NumericalAxis.LabelStyle>
        <chart:ChartAxisLabelStyle LabelFormat="N2"/>
    </chart:NumericalAxis.LabelStyle>
</chart:NumericalAxis>
<chart:NumericalAxis />
```

```csharp
ChartAxisLabelStyle labelStyle = new ChartAxisLabelStyle()
{
    LabelFormat = "N2"  // Two decimal places
};

NumericalAxis axis = new NumericalAxis()
{
    LabelStyle = labelStyle
};
```

**Common Formats:**
- `N0`: Number with no decimals
- `N2`: Number with 2 decimals
- `C`: Currency
- `P`: Percentage
- `dd-MMM-yyyy`: Date format

### Label Rotation

```xml
<chart:CategoryAxis LabelRotation="45"/>
```

```csharp
CategoryAxis axis = new CategoryAxis()
{
    LabelRotation = 45
};
```

### Edge Labels Visibility

```xml
<chart:NumericalAxis EdgeLabelsDrawingMode="Shift"/>
```

```csharp
NumericalAxis axis = new NumericalAxis()
{
    EdgeLabelsDrawingMode = EdgeLabelsDrawingMode.Shift
};
```

**Options:**
- `Center`: Center edge labels
- `Shift`: Shift edge labels inside
- `Hide`: Hide edge labels

## Axis Title

### Basic Title

```xml
<chart:NumericalAxis>
    <chart:NumericalAxis.Title>
        <chart:ChartAxisTitle Text="Sales (in thousands)"/>
    </chart:NumericalAxis.Title>
</chart:NumericalAxis>
```

```csharp
ChartAxisTitle title = new ChartAxisTitle()
{
    Text = "Sales (in thousands)"
};

NumericalAxis axis = new NumericalAxis()
{
    Title = title
};
```

### Title Customization

```xml
<chart:NumericalAxis>
    <chart:NumericalAxis.Title>
        <chart:ChartAxisTitle Text="Revenue"
                              TextColor="Red"
                              FontSize="16"
                              FontAttributes="Bold"
                              Margin="10"/>
    </chart:NumericalAxis.Title>
</chart:NumericalAxis>
```

```csharp
ChartAxisTitle title = new ChartAxisTitle()
{
    Text = "Revenue",
    TextColor = Colors.Red,
    FontSize = 16,
    FontAttributes = FontAttributes.Bold,
    Margin = new Thickness(10)
};

NumericalAxis axis = new NumericalAxis()
{
    Title = title
};
```
## Custom Labels

Add custom text labels to chart axes for better context.

### Adding Custom Labels to Axis

```csharp
// In code-behind or ViewModel
CategoryAxis xAxis = new CategoryAxis();

// Add custom labels
xAxis.LabelCreated += (sender, e) =>
{
    // Customize labels based on position
    if (e.Position == 0)
        e.LabelStyle = new ChartAxisLabelStyle { TextColor = Colors.Red };
    else if (e.Position == 5)
        e.LabelStyle = new ChartAxisLabelStyle { TextColor = Colors.Green };
};
```

### Custom Label Formatting

```csharp
NumericalAxis yAxis = new NumericalAxis();

yAxis.LabelCreated += (sender, e) =>
{
    // Format as currency
    if (double.TryParse(e.Label, out double value))
    {
        e.Label = $"${value:N0}";
    }
};

chart.YAxes.Add(yAxis);
```

### Category-Specific Styling

```csharp
CategoryAxis xAxis = new CategoryAxis();

xAxis.LabelCreated += (sender, e) =>
{
    // Highlight weekend labels
    if (e.Label == "Saturday" || e.Label == "Sunday")
    {
        e.LabelStyle = new ChartAxisLabelStyle
        {
            TextColor = Colors.Blue,
            FontAttributes = FontAttributes.Bold
        };
    }
};
```
## Best Practices

### Axis Type Selection

**Use NumericalAxis when:**
- Data consists of continuous numeric values
- Need linear scale representation
- X and Y values are both numeric

**Use CategoryAxis when:**
- Data has distinct categories
- Equal spacing between points is desired
- Index-based positioning is preferred

**Use DateTimeAxis when:**
- Data has datetime values
- Timeline visualization is needed
- Automatic date interval calculation is required

**Use DateTimeCategoryAxis when:**
- Data has irregular datetime intervals
- Both datetime and categorical features needed
- Non-uniform time gaps exist

**Use LogarithmicAxis when:**
- Data spans multiple orders of magnitude
- Exponential growth patterns
- Scientific or statistical data

### Common Guidelines

1. **Interval Setting**: Set appropriate intervals for readability
2. **Range Definition**: Define min/max when auto-range is insufficient
3. **Label Format**: Use consistent, readable formats
4. **Title Clarity**: Add descriptive axis titles
5. **Multiple Axes**: Use when comparing different scales

### Performance Tips

1. Limit custom labels to necessary positions
2. Use appropriate interval to reduce label count
3. Avoid excessive label rotation
4. Consider label visibility with large datasets

### Troubleshooting

**Issue**: Labels overlapping
- **Solution**: Increase interval, rotate labels, or reduce font size

**Issue**: Axis not showing all data
- **Solution**: Set explicit Minimum/Maximum values

**Issue**: DateTime labels showing incorrectly
- **Solution**: Verify LabelFormat and IntervalType settings

**Issue**: Logarithmic axis showing gaps
- **Solution**: Ensure all values are positive and non-zero
