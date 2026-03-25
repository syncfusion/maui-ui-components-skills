# Axis Configuration for Polar Charts

## Table of Contents
- [Overview](#overview)
- [Axis Types](#axis-types)
- [Category Axis](#category-axis)
- [Numerical Axis](#numerical-axis)
- [DateTime Axis](#datetime-axis)
- [DateTimeCategory Axis](#datetimecategory-axis)
- [Axis Labels](#axis-labels)
- [Axis Lines and Styling](#axis-lines-and-styling)
- [Grid Lines](#grid-lines)
- [Tick Lines](#tick-lines)
- [Axis Title](#axis-title)
- [Axis Range and Interval](#axis-range-and-interval)
- [Inversed Axis](#inversed-axis)
- [Axis Events](#axis-events)

## Overview

Polar charts use two axes to position data points:

- **PrimaryAxis** (X-axis) - Represents the angular position (categories, dates, or numeric values)
- **SecondaryAxis** (Y-axis) - Represents the radial distance from the center (typically numeric)

Understanding axis configuration is crucial for properly displaying and interpreting polar chart data.

## Axis Types

Sync fusion .NET MAUI Polar Charts support four axis types:

| Axis Type | Purpose | Best For |
|-----------|---------|----------|
| **CategoryAxis** | Index-based with string labels | Named categories, directions, skills |
| **NumericalAxis** | Numeric values with auto-scaling | Continuous numeric data |
| **DateTimeAxis** | Time-based data | Time series, dates |
| **DateTimeCategoryAxis** | DateTime without gaps | Financial data, irregular intervals |

## Category Axis

CategoryAxis is an indexed axis that plots values based on the index of data points. Points are equally spaced.

### Basic Implementation

**XAML:**
```xml
<chart:SfPolarChart>
    <chart:SfPolarChart.PrimaryAxis>
        <chart:CategoryAxis/>
    </chart:SfPolarChart.PrimaryAxis>
    
    <chart:SfPolarChart.SecondaryAxis>
        <chart:NumericalAxis/>
    </chart:SfPolarChart.SecondaryAxis>
</chart:SfPolarChart>
```

**C#:**
```csharp
SfPolarChart chart = new SfPolarChart();

CategoryAxis primaryAxis = new CategoryAxis();
chart.PrimaryAxis = primaryAxis;

NumericalAxis secondaryAxis = new NumericalAxis();
chart.SecondaryAxis = secondaryAxis;
```

### Interval

Control the spacing of labels with the `Interval` property (default is 1):

```csharp
CategoryAxis primaryAxis = new CategoryAxis
{
    Interval = 2  // Show every 2nd label
};
```

**XAML:**
```xml
<chart:CategoryAxis Interval="2"/>
```

### Use Cases for Category Axis

**Perfect for:**
- Compass directions (N, NE, E, SE, S, SW, W, NW)
- Skill names (Communication, Leadership, Technical, etc.)
- Product categories
- Department names
- Any non-numeric, named categories

**Example - Skill Assessment:**
```csharp
public class SkillData
{
    public string Skill { get; set; }  // "Communication", "Leadership", etc.
    public double Score { get; set; }
}

CategoryAxis primaryAxis = new CategoryAxis();
chart.PrimaryAxis = primaryAxis;

PolarLineSeries series = new PolarLineSeries
{
    ItemsSource = skillData,
    XBindingPath = "Skill",    // String category names
    YBindingPath = "Score"
};
```

## Numerical Axis

NumericalAxis plots numeric values with automatic scaling based on data range.

### Basic Implementation

**XAML:**
```xml
<chart:SfPolarChart>
    <chart:SfPolarChart.PrimaryAxis>
        <chart:NumericalAxis/>
    </chart:SfPolarChart.PrimaryAxis>
    
    <chart:SfPolarChart.SecondaryAxis>
        <chart:NumericalAxis Maximum="100" Minimum="0"/>
    </chart:SfPolarChart.SecondaryAxis>
</chart:SfPolarChart>
```

**C#:**
```csharp
SfPolarChart chart = new SfPolarChart();

NumericalAxis primaryAxis = new NumericalAxis();
chart.PrimaryAxis = primaryAxis;

NumericalAxis secondaryAxis = new NumericalAxis
{
    Maximum = 100,
    Minimum = 0
};
chart.SecondaryAxis = secondaryAxis;
```

### Interval Configuration

```csharp
NumericalAxis axis = new NumericalAxis
{
    Interval = 10  // Show labels every 10 units
};
```

**XAML:**
```xml
<chart:NumericalAxis Interval="10"/>
```

### Range Customization

Set minimum and maximum values to control the visible range:

```csharp
NumericalAxis axis = new NumericalAxis
{
    Minimum = 0,
    Maximum = 100,
    Interval = 20  // 0, 20, 40, 60, 80, 100
};
```

**Note:** If only Minimum or Maximum is set, the other value is calculated automatically from the data.

### Use Cases for Numerical Axis

**Primary Axis (X-axis):**
- Angular measurements (0-360 degrees)
- Numeric indices or IDs
- Ordered numeric categories

**Secondary Axis (Y-axis):**
- Percentages (0-100%)
- Measurements (temperature, speed, distance)
- Scores, ratings, metrics

## DateTime Axis

DateTimeAxis plots DateTime values with automatic interval calculation.

### Basic Implementation

```csharp
DateTimeAxis primaryAxis = new DateTimeAxis();
chart.PrimaryAxis = primaryAxis;

PolarLineSeries series = new PolarLineSeries
{
    ItemsSource = timeSeriesData,
    XBindingPath = "Date",      // DateTime property
    YBindingPath = "Value"
};
```

**XAML:**
```xml
<chart:SfPolarChart>
    <chart:SfPolarChart.PrimaryAxis>
        <chart:DateTimeAxis/>
    </chart:SfPolarChart.PrimaryAxis>
</chart:SfPolarChart>
```

### Interval and Interval Type

Configure how dates are spaced:

```csharp
DateTimeAxis primaryAxis = new DateTimeAxis
{
    Interval = 6,
    IntervalType = DateTimeIntervalType.Months  // Every 6 months
};
```

**Available IntervalType values:**
- `Auto` - Automatically determined
- `Years`
- `Months`
- `Days`
- `Hours`
- `Minutes`
- `Seconds`
- `Milliseconds`

**XAML:**
```xml
<chart:DateTimeAxis Interval="6" IntervalType="Months"/>
```

### Date Range Customization

```csharp
DateTimeAxis primaryAxis = new DateTimeAxis
{
    Minimum = new DateTime(2023, 1, 1),
    Maximum = new DateTime(2023, 12, 31),
    IntervalType = DateTimeIntervalType.Months
};
```

**XAML:**
```xml
<chart:DateTimeAxis Minimum="2023/01/01" Maximum="2023/12/31" IntervalType="Months"/>
```

### Use Cases for DateTime Axis

- Monthly sales trends (cyclic, 12 months)
- Seasonal patterns
- Time-based measurements over a cycle
- Historical data visualization

## DateTimeCategory Axis

DateTimeCategoryAxis is similar to CategoryAxis but for DateTime values. It plots points with equal spacing, eliminating gaps for missing dates.

### Basic Implementation

```csharp
DateTimeCategoryAxis primaryAxis = new DateTimeCategoryAxis();
chart.PrimaryAxis = primaryAxis;
```

**XAML:**
```xml
<chart:SfPolarChart>
    <chart:SfPolarChart.PrimaryAxis>
        <chart:DateTimeCategoryAxis/>
    </chart:SfPolarChart.PrimaryAxis>
</chart:SfPolarChart>
```

### Interval Configuration

```csharp
DateTimeCategoryAxis primaryAxis = new DateTimeCategoryAxis
{
    Interval = 3,
    IntervalType = DateTimeIntervalType.Months
};
```

### Key Difference from DateTime Axis

- **DateTimeAxis**: Shows gaps for missing data (e.g., weekends, holidays)
- **DateTimeCategoryAxis**: No gaps, all points equally spaced

**Use DateTimeCategoryAxis for:**
- Financial data (skip non-trading days)
- Irregular time intervals
- When consistent spacing is more important than proportional time representation

## Axis Labels

Customize how axis labels appear:

### Label Style Customization

```csharp
CategoryAxis axis = new CategoryAxis();
axis.LabelStyle = new ChartAxisLabelStyle
{
    TextColor = Colors.Blue,
    FontSize = 14,
    FontAttributes = FontAttributes.Bold,
    FontFamily = "Arial",
    Margin = new Thickness(5)
};
```

**XAML:**
```xml
<chart:CategoryAxis>
    <chart:CategoryAxis.LabelStyle>
        <chart:ChartAxisLabelStyle TextColor="Blue"
                                   FontSize="14"
                                   FontAttributes="Bold"
                                   FontFamily="Arial"
                                   Margin="5"/>
    </chart:CategoryAxis.LabelStyle>
</chart:CategoryAxis>
```

### Label Rotation

```csharp
axis.LabelStyle = new ChartAxisLabelStyle
{
    LabelRotation = 45  // Rotate labels 45 degrees
};
```

### Label Format

For numerical or date values:

```csharp
// Numerical axis formatting
NumericalAxis axis = new NumericalAxis
{
    LabelFormat = "0.00"  // Two decimal places
};

// DateTime axis formatting
DateTimeAxis dateAxis = new DateTimeAxis
{
    LabelFormat = "MMM yyyy"  // "Jan 2023"
};
```

## Axis Lines and Styling

Customize the appearance of axis lines:

### Axis Line Style

```csharp
CategoryAxis axis = new CategoryAxis();
axis.AxisLineStyle = new ChartLineStyle
{
    Stroke = new SolidColorBrush(Colors.Gray),
    StrokeWidth = 2,
    StrokeDashArray = new double[] { 5, 2 }  // Dashed line
};
```

**XAML:**
```xml
<chart:CategoryAxis>
    <chart:CategoryAxis.AxisLineStyle>
        <chart:ChartLineStyle Stroke="Gray" 
                              StrokeWidth="2"/>
    </chart:CategoryAxis.AxisLineStyle>
</chart:CategoryAxis>
```

### Hide Axis Line

```csharp
axis.AxisLineStyle = new ChartLineStyle
{
    StrokeWidth = 0  // Hide line
};
```

## Grid Lines

Grid lines help users read values from the chart:

### Major Grid Lines

```csharp
NumericalAxis axis = new NumericalAxis();
axis.MajorGridLineStyle = new ChartLineStyle
{
    Stroke = new SolidColorBrush(Colors.LightGray),
    StrokeWidth = 1,
    StrokeDashArray = new double[] { 3, 3 }
};
```

**XAML:**
```xml
<chart:NumericalAxis>
    <chart:NumericalAxis.MajorGridLineStyle>
        <chart:ChartLineStyle Stroke="LightGray" StrokeWidth="1"/>
    </chart:NumericalAxis.MajorGridLineStyle>
</chart:NumericalAxis>
```

### Minor Grid Lines

```csharp
axis.MinorTicksPerInterval = 4;  // 4 minor ticks between major ticks
axis.MinorGridLineStyle = new ChartLineStyle
{
    Stroke = new SolidColorBrush(Colors.LightGray),
    StrokeWidth = 0.5
};
```

### Hide Grid Lines

```csharp
axis.MajorGridLineStyle = new ChartLineStyle { StrokeWidth = 0 };
axis.MinorGridLineStyle = new ChartLineStyle { StrokeWidth = 0 };
```

## Tick Lines

Tick lines mark axis values:

### Major Tick Lines

```csharp
axis.MajorTickStyle = new ChartAxisTickStyle
{
    Stroke = new SolidColorBrush(Colors.Black),
    StrokeWidth = 2,
    TickSize = 10  // Length of tick mark
};
```

**XAML:**
```xml
<chart:CategoryAxis>
    <chart:CategoryAxis.MajorTickStyle>
        <chart:ChartAxisTickStyle Stroke="Black" 
                                  StrokeWidth="2" 
                                  TickSize="10"/>
    </chart:CategoryAxis.MajorTickStyle>
</chart:CategoryAxis>
```

### Minor Tick Lines

```csharp
axis.MinorTicksPerInterval = 4;
axis.MinorTickStyle = new ChartAxisTickStyle
{
    Stroke = new SolidColorBrush(Colors.Gray),
    StrokeWidth = 1,
    TickSize = 5
};
```

## Axis Title

Add a descriptive title to axes:

```csharp
NumericalAxis axis = new NumericalAxis
{
    Title = new ChartAxisTitle
    {
        Text = "Performance Score (%)",
        TextColor = Colors.DarkBlue,
        FontSize = 16,
        FontAttributes = FontAttributes.Bold
    }
};
```

**XAML:**
```xml
<chart:NumericalAxis>
    <chart:NumericalAxis.Title>
        <chart:ChartAxisTitle Text="Performance Score (%)"
                              TextColor="DarkBlue"
                              FontSize="16"
                              FontAttributes="Bold"/>
    </chart:NumericalAxis.Title>
</chart:NumericalAxis>
```

## Axis Range and Interval

### Auto Range (Default)

By default, axes calculate range from data:

```csharp
NumericalAxis axis = new NumericalAxis();
// Range automatically calculated from data min/max
```

### Fixed Range

Set explicit minimum and maximum:

```csharp
NumericalAxis axis = new NumericalAxis
{
    Minimum = 0,
    Maximum = 100,
    Interval = 10
};
```

### Padding

Add padding around data range:

```csharp
NumericalAxis axis = new NumericalAxis
{
    RangePadding = NumericalPadding.Round  // Round to nice numbers
};
```

**Available RangePadding values:**
- `None` - No padding
- `Round` - Round to nearest nice number
- `Additional` - Add 5% padding
- `RoundStart` - Round minimum only
- `RoundEnd` - Round maximum only
- `PrependInterval` - Add one interval before minimum
- `AppendInterval` - Add one interval after maximum

## Inversed Axis

Reverse the axis direction:

```csharp
NumericalAxis axis = new NumericalAxis
{
    IsInversed = true  // Reverse direction
};
```

**XAML:**
```xml
<chart:NumericalAxis IsInversed="True"/>
```

**Use cases:**
- Reverse ranking (lower is better)
- Custom data orientation
- Mirror chart layout

## Axis Events

Subscribe to axis events for custom behavior:

### ActualRangeChanged Event

Triggered when the axis range changes:

```csharp
axis.ActualRangeChanged += (sender, e) =>
{
    Console.WriteLine($"Range: {e.ActualMinimum} to {e.ActualMaximum}");
};
```

### LabelCreated Event

Customize individual labels as they're created:

```csharp
axis.LabelCreated += (sender, e) =>
{
    // e.Label - Label text
    // e.Position - Label position
    // e.LabelStyle - Style to customize
    
    if (e.Position > 80)
    {
        e.LabelStyle.TextColor = Colors.Red;  // Highlight high values
    }
};
```

## Common Patterns

### Pattern 1: Percentage-Based Secondary Axis

```csharp
NumericalAxis secondaryAxis = new NumericalAxis
{
    Minimum = 0,
    Maximum = 100,
    Interval = 20,
    LabelFormat = "0'%'",
    Title = new ChartAxisTitle { Text = "Percentage (%)" }
};
```

### Pattern 2: Directional Primary Axis

```csharp
CategoryAxis primaryAxis = new CategoryAxis
{
    // Data: N, NE, E, SE, S, SW, W, NW
    Interval = 1  // Show all directions
};
```

### Pattern 3: Monthly Time Series

```csharp
DateTimeAxis primaryAxis = new DateTimeAxis
{
    IntervalType = DateTimeIntervalType.Months,
    Interval = 1,
    LabelFormat = "MMM",  // Jan, Feb, Mar...
    Minimum = new DateTime(2023, 1, 1),
    Maximum = new DateTime(2023, 12, 31)
};
```

## Troubleshooting

### Labels Overlapping

**Problem:** Axis labels overlap and are unreadable.

**Solutions:**
```csharp
// Solution 1: Increase interval
axis.Interval = 2;

// Solution 2: Rotate labels
axis.LabelStyle = new ChartAxisLabelStyle { LabelRotation = 45 };

// Solution 3: Reduce font size
axis.LabelStyle = new ChartAxisLabelStyle { FontSize = 10 };
```

### Incorrect Data Range

**Problem:** Data points cut off or too much empty space.

**Solutions:**
```csharp
// Check data range
Console.WriteLine($"Data min: {data.Min(d => d.Value)}");
Console.WriteLine($"Data max: {data.Max(d => d.Value)}");

// Set appropriate range
axis.Minimum = 0;
axis.Maximum = 100;

// Or use auto-range with padding
axis.RangePadding = NumericalPadding.Round;
```

### DateTime Axis Not Working

**Problem:** DateTime axis shows no data or incorrect dates.

**Solutions:**
```csharp
// Ensure XBindingPath points to DateTime property
public class DataModel
{
    public DateTime Date { get; set; }  // Must be DateTime, not string
    public double Value { get; set; }
}

// Set appropriate date range
DateTimeAxis axis = new DateTimeAxis
{
    Minimum = new DateTime(2023, 1, 1),
    Maximum = new DateTime(2023, 12, 31)
};
```

## Related Topics

- **Getting Started**: [getting-started.md](getting-started.md) - Basic axis setup
- **Series Types**: [series-types.md](series-types.md) - How series use axes
- **Rendering Position**: [rendering-position.md](rendering-position.md) - StartAngle configuration
