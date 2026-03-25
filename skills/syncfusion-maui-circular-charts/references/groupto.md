# Data Grouping in .NET MAUI Circular Charts

The GroupTo feature automatically combines small segments into an "Others" category, improving chart readability when you have many small values.

## Overview

When a chart has many small segments, it becomes cluttered and hard to read. The `GroupTo` property groups these small segments into a single "Others" segment.

**Supported Chart Types:** PieSeries and DoughnutSeries only (not RadialBarSeries).

## Basic Grouping

Enable grouping by setting the `GroupTo` property with a threshold value.

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:PieSeries ItemsSource="{Binding Data}"
                     XBindingPath="Product"
                     YBindingPath="SalesRate"
                     GroupTo="15"
                     GroupMode="Value"/>
</chart:SfCircularChart>
```

**C#:**
```csharp
PieSeries series = new PieSeries
{
    ItemsSource = data,
    XBindingPath = "Product",
    YBindingPath = "SalesRate",
    GroupTo = 15,
    GroupMode = PieGroupMode.Value
};
```

## Group Modes

The `GroupMode` property determines how grouping is calculated.

### Value Mode

Groups segments where the Y value is less than the `GroupTo` threshold.

**XAML:**
```xml
<chart:PieSeries GroupTo="10" GroupMode="Value"/>
```

**C#:**
```csharp
series.GroupMode = PieGroupMode.Value;
series.GroupTo = 10;  // Group segments with value < 10
```

**Example:**
- Data: [5, 8, 25, 30, 3, 40]
- GroupTo: 10
- Result: Groups 5, 8, and 3 into "Others" (total: 16)

### Percentage Mode

Groups segments where the percentage is less than the `GroupTo` threshold.

**XAML:**
```xml
<chart:PieSeries GroupTo="5" GroupMode="Percentage"/>
```

**C#:**
```csharp
series.GroupMode = PieGroupMode.Percentage;
series.GroupTo = 5;  // Group segments < 5%
```

**Example:**
- Total: 100
- Data: [40, 30, 15, 8, 4, 3]
- GroupTo: 5 (5%)
- Result: Groups 4% and 3% into "Others" (total: 7%)

### Angle Mode

Groups segments where the angle is less than the `GroupTo` threshold (in degrees).

**XAML:**
```xml
<chart:PieSeries GroupTo="20" GroupMode="Angle"/>
```

**C#:**
```csharp
series.GroupMode = PieGroupMode.Angle;
series.GroupTo = 20;  // Group segments < 20 degrees
```

**Example:**
- GroupTo: 30 degrees
- Segments with angles < 30° are grouped into "Others"

## Complete Examples

### Example 1: Group by Value Threshold

```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.Title>
        <Label Text="Product Sales (Values < 15 grouped)"/>
    </chart:SfCircularChart.Title>
    
    <chart:SfCircularChart.Legend>
        <chart:ChartLegend/>
    </chart:SfCircularChart.Legend>
    
    <chart:PieSeries ItemsSource="{Binding Data}"
                     XBindingPath="Product"
                     YBindingPath="SalesRate"
                     GroupTo="15"
                     GroupMode="Value"
                     ShowDataLabels="True"/>
</chart:SfCircularChart>
```

### Example 2: Group by Percentage

```csharp
SfCircularChart chart = new SfCircularChart();

PieSeries series = new PieSeries
{
    ItemsSource = salesData,
    XBindingPath = "Category",
    YBindingPath = "Amount",
    GroupMode = PieGroupMode.Percentage,
    GroupTo = 3,  // Group categories < 3%
    ShowDataLabels = true
};

chart.Series.Add(series);
```

### Example 3: Doughnut Chart with Grouping

```xml
<chart:SfCircularChart>
    <chart:DoughnutSeries ItemsSource="{Binding MarketData}"
                          XBindingPath="Company"
                          YBindingPath="MarketShare"
                          GroupTo="5"
                          GroupMode="Percentage"
                          InnerRadius="0.6"
                          ShowDataLabels="True">
        <chart:DoughnutSeries.DataLabelSettings>
            <chart:CircularDataLabelSettings LabelContext="Percentage"
                                             LabelPosition="Outside"/>
        </chart:DoughnutSeries.DataLabelSettings>
    </chart:DoughnutSeries>
</chart:SfCircularChart>
```

### Example 4: Dynamic Grouping Threshold

```csharp
public class SalesViewModel
{
    public double GroupingThreshold { get; set; } = 10;
    
    public void UpdateGrouping(double threshold)
    {
        GroupingThreshold = threshold;
        // Rebind or update series
        OnPropertyChanged(nameof(GroupingThreshold));
    }
}
```

```xml
<Slider Minimum="0" 
        Maximum="50" 
        Value="{Binding GroupingThreshold}"
        ValueChanged="OnThresholdChanged"/>

<chart:PieSeries GroupTo="{Binding GroupingThreshold}"
                 GroupMode="Value"/>
```

### Example 5: Grouping with Custom Styling

```csharp
PieSeries series = new PieSeries
{
    ItemsSource = data,
    XBindingPath = "Region",
    YBindingPath = "Sales",
    GroupMode = PieGroupMode.Value,
    GroupTo = 20,
    ShowDataLabels = true
};

// Custom colors
series.PaletteBrushes = new List<Brush>
{
    new SolidColorBrush(Colors.Blue),
    new SolidColorBrush(Colors.Green),
    new SolidColorBrush(Colors.Orange),
    new SolidColorBrush(Colors.Gray)  // Color for "Others"
};
```

## Grouping Behavior

### "Others" Segment

The grouped segment automatically receives the label "Others" and appears in the legend.

**Characteristics:**
- Label: "Others"
- Value: Sum of all grouped segments
- Color: Next available color in the palette
- Interactive: Supports tooltip, selection, and explode like other segments

### Order of Segments

After grouping:
1. Individual segments (not grouped)
2. "Others" segment (at the end)

### Legend Display

The "Others" segment appears in the legend like any other segment, showing the combined value.

## Choosing the Right GroupMode

| Mode | When to Use | Example Scenario |
|------|-------------|------------------|
| **Value** | Fixed threshold regardless of total | Sales < $1000 |
| **Percentage** | Relative to total | Market share < 5% |
| **Angle** | Visual balance | Segments too small to see |

### Value Mode Use Cases

```csharp
// E-commerce: Group products with < 100 sales
series.GroupMode = PieGroupMode.Value;
series.GroupTo = 100;

// Budget: Group expenses < $500
series.GroupTo = 500;
```

### Percentage Mode Use Cases

```csharp
// Market share: Group companies < 2%
series.GroupMode = PieGroupMode.Percentage;
series.GroupTo = 2;

// Survey results: Group responses < 5%
series.GroupTo = 5;
```

### Angle Mode Use Cases

```csharp
// Visual clarity: Group tiny slices
series.GroupMode = PieGroupMode.Angle;
series.GroupTo = 15;  // < 15 degrees
```

## Best Practices

1. **Threshold Selection**: 
   - Value mode: 5-10% of average value
   - Percentage mode: 2-5%
   - Angle mode: 10-20 degrees

2. **Label Display**: Enable data labels to show what's grouped

3. **Tooltip**: Use tooltips to provide details about grouped items

4. **Legend**: Always show legend when using grouping

5. **Documentation**: Inform users that "Others" represents grouped values

## Advanced Techniques

### Conditional Grouping

```csharp
public void SetGrouping(ObservableCollection<DataModel> data)
{
    // Only group if there are many small segments
    int smallSegments = data.Count(x => x.Value < 10);
    
    if (smallSegments > 3)
    {
        series.GroupMode = PieGroupMode.Value;
        series.GroupTo = 10;
    }
    else
    {
        series.GroupTo = 0;  // Disable grouping
    }
}
```

### Custom "Others" Label (Workaround)

Since you can't directly change the "Others" label, you can preprocess your data:

```csharp
public ObservableCollection<DataModel> GetGroupedData(double threshold)
{
    var grouped = new ObservableCollection<DataModel>();
    double othersTotal = 0;
    
    foreach (var item in originalData)
    {
        if (item.Value >= threshold)
        {
            grouped.Add(item);
        }
        else
        {
            othersTotal += item.Value;
        }
    }
    
    if (othersTotal > 0)
    {
        grouped.Add(new DataModel 
        { 
            Category = "Miscellaneous",  // Custom label
            Value = othersTotal 
        });
    }
    
    return grouped;
}
```

### Interactive Threshold Adjustment

```xml
<StackLayout>
    <Label Text="{Binding GroupThreshold, StringFormat='Group threshold: {0}'}"/>
    <Slider Minimum="0" 
            Maximum="50" 
            Value="{Binding GroupThreshold}"/>
    
    <chart:SfCircularChart>
        <chart:PieSeries ItemsSource="{Binding Data}"
                         XBindingPath="Category"
                         YBindingPath="Value"
                         GroupTo="{Binding GroupThreshold}"
                         GroupMode="Value"/>
    </chart:SfCircularChart>
</StackLayout>
```

## Common Pitfalls

### Too Aggressive Grouping

```csharp
// Bad: Groups too much
series.GroupTo = 50;  // Most data goes to "Others"

// Good: Reasonable threshold
series.GroupTo = 10;  // Groups only small segments
```

### Wrong Mode for Data Type

```csharp
// Bad: Using percentage mode with absolute values
series.GroupMode = PieGroupMode.Percentage;
series.GroupTo = 1000;  // Doesn't make sense

// Good: Use value mode for absolute values
series.GroupMode = PieGroupMode.Value;
series.GroupTo = 1000;
```

### Forgetting to Set GroupMode

```csharp
// Incomplete: GroupMode defaults to Value
series.GroupTo = 5;

// Complete: Explicitly set both
series.GroupMode = PieGroupMode.Percentage;
series.GroupTo = 5;
```

## Troubleshooting

### Grouping Not Working

**Check:**
- `GroupTo` value is appropriate for your data
- `GroupMode` matches your data type
- Series is PieSeries or DoughnutSeries (not RadialBarSeries)
- Data actually has values below the threshold

### All Data Grouped into "Others"

**Check:**
- `GroupTo` threshold is too high
- `GroupMode` matches your data scale
- Data values are correct

### "Others" Not Appearing

**Check:**
- No segments are below the threshold
- `GroupTo` is greater than 0
- Legend is enabled to see the "Others" item

## Summary

- **GroupTo**: Threshold value for grouping
- **GroupMode**: How threshold is calculated (Value, Percentage, Angle)
- **Supported**: PieSeries and DoughnutSeries only
- **Result**: Small segments combined into "Others" category
- **Benefits**: Improved readability, reduced clutter, clearer insights
