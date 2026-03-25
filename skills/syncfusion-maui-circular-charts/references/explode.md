# Explode Feature in .NET MAUI Circular Charts

The explode feature pulls segments outward from the chart center to emphasize specific data points. This guide covers exploding single or multiple segments.

## Explode Properties

| Property | Type | Description |
|----------|------|-------------|
| **ExplodeIndex** | int | Index of the segment to explode |
| **ExplodeRadius** | double | Distance to explode the segment |
| **ExplodeOnTouch** | bool | Enable tap-to-explode interaction |
| **ExplodeAll** | bool | Explode all segments at once |

## Exploding a Single Segment

Use `ExplodeIndex` to explode a specific segment by its index (0-based).

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:PieSeries ItemsSource="{Binding Data}"
                     XBindingPath="Product"
                     YBindingPath="SalesRate"
                     ExplodeIndex="2"
                     ExplodeRadius="10"/>
</chart:SfCircularChart>
```

**C#:**
```csharp
PieSeries series = new PieSeries
{
    ItemsSource = data,
    XBindingPath = "Product",
    YBindingPath = "SalesRate",
    ExplodeIndex = 2,       // Third segment (0-based index)
    ExplodeRadius = 10      // Pixels to explode outward
};
```

### ExplodeIndex Examples

```csharp
// Explode first segment
series.ExplodeIndex = 0;

// Explode third segment
series.ExplodeIndex = 2;

// No explosion (default)
series.ExplodeIndex = -1;
```

## Explode Radius

The `ExplodeRadius` property defines how far the segment moves from the center (in pixels).

**XAML:**
```xml
<chart:DoughnutSeries ExplodeIndex="1" ExplodeRadius="15"/>
```

**C#:**
```csharp
series.ExplodeRadius = 15;  // Explode 15 pixels outward
```

### Radius Guidelines

- **Small explosion**: 5-10 pixels (subtle emphasis)
- **Medium explosion**: 10-20 pixels (noticeable separation)
- **Large explosion**: 20-30 pixels (dramatic emphasis)

```csharp
// Subtle
series.ExplodeRadius = 8;

// Moderate
series.ExplodeRadius = 15;

// Dramatic
series.ExplodeRadius = 25;
```

## Explode on Touch

Enable interactive explosion by setting `ExplodeOnTouch` to `true`. Users can tap segments to explode/collapse them.

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:DoughnutSeries ItemsSource="{Binding Data}"
                          XBindingPath="XValue"
                          YBindingPath="YValue"
                          ExplodeOnTouch="True"
                          ExplodeRadius="12"/>
</chart:SfCircularChart>
```

**C#:**
```csharp
DoughnutSeries series = new DoughnutSeries
{
    ExplodeOnTouch = true,
    ExplodeRadius = 12
};
```

### Interaction Behavior

When `ExplodeOnTouch` is enabled:
- **First tap**: Explodes the segment
- **Second tap**: Collapses the segment back
- **Tap another segment**: Previous segment collapses, new segment explodes

## Exploding All Segments

Use `ExplodeAll` to explode every segment simultaneously.

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:PieSeries ItemsSource="{Binding Data}"
                     XBindingPath="Product"
                     YBindingPath="SalesRate"
                     ExplodeAll="True"
                     ExplodeRadius="10"/>
</chart:SfCircularChart>
```

**C#:**
```csharp
PieSeries series = new PieSeries
{
    ExplodeAll = true,
    ExplodeRadius = 10
};
```

### When to Use ExplodeAll

- Creating a "flower" or "burst" visual effect
- Emphasizing separation between all categories
- Aesthetic purposes in dashboards or presentations
- When you want all segments equally visible

## Combining Explode Features

### Pre-exploded with Touch Interaction

```xml
<chart:DoughnutSeries ItemsSource="{Binding Data}"
                      XBindingPath="Category"
                      YBindingPath="Value"
                      ExplodeIndex="1"
                      ExplodeOnTouch="True"
                      ExplodeRadius="15"/>
```

This configuration:
- Initially explodes the first segment (index 1)
- Allows users to tap any segment to explode it
- Previously exploded segments collapse when a new one is tapped

### All Segments with Touch Toggle

```csharp
series.ExplodeAll = true;
series.ExplodeOnTouch = true;
series.ExplodeRadius = 8;
```

This allows users to:
- See all segments initially exploded
- Tap segments to collapse/expand them individually

## Complete Examples

### Example 1: Highlight Top Performer

```xml
<chart:SfCircularChart>
    <chart:PieSeries ItemsSource="{Binding SalesData}"
                     XBindingPath="Region"
                     YBindingPath="Revenue"
                     ExplodeIndex="1"
                     ExplodeRadius="20"
                     ShowDataLabels="True"/>
</chart:SfCircularChart>
```

```csharp
// Sort data so top performer is first
var sortedData = salesData.OrderByDescending(x => x.Revenue).ToList();
series.ItemsSource = sortedData;
series.ExplodeIndex = 1;  // Explode top performer
series.ExplodeRadius = 20;
```

### Example 2: Interactive Doughnut Chart

```csharp
SfCircularChart chart = new SfCircularChart();

DoughnutSeries series = new DoughnutSeries
{
    ItemsSource = data,
    XBindingPath = "Category",
    YBindingPath = "Percentage",
    InnerRadius = 0.6,
    ExplodeOnTouch = true,
    ExplodeRadius = 15,
    EnableTooltip = true,
    ShowDataLabels = true
};

chart.Series.Add(series);
```

### Example 3: Burst Effect with All Segments

```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.Title>
        <Label Text="Market Distribution"/>
    </chart:SfCircularChart.Title>
    
    <chart:PieSeries ItemsSource="{Binding MarketData}"
                     XBindingPath="Company"
                     YBindingPath="Share"
                     ExplodeAll="True"
                     ExplodeRadius="12"
                     Stroke="White"
                     StrokeWidth="2"/>
</chart:SfCircularChart>
```

### Example 4: Dynamic Explode Based on Value

```csharp
// Explode the segment with the highest value
int maxIndex = data
    .Select((item, index) => new { item, index })
    .OrderByDescending(x => x.item.Value)
    .First()
    .index;

series.ExplodeIndex = maxIndex;
series.ExplodeRadius = 18;
```

### Example 5: Multiple Exploded Segments (Workaround)

Since `ExplodeIndex` only supports one segment, use multiple series for multiple exploded segments:

```csharp
// Note: This is a workaround - the proper way is to use ExplodeAll
// or handle through custom interaction logic

// Data split approach
var highValue = data.Where(x => x.Value > threshold).ToList();
var normalValue = data.Where(x => x.Value <= threshold).ToList();

PieSeries explodedSeries = new PieSeries
{
    ItemsSource = highValue,
    ExplodeAll = true,
    ExplodeRadius = 15
};

PieSeries normalSeries = new PieSeries
{
    ItemsSource = normalValue
};
```

## Visual Combinations

### Explode + Data Labels

```xml
<chart:PieSeries ExplodeIndex="1"
                 ExplodeRadius="15"
                 ShowDataLabels="True">
    <chart:PieSeries.DataLabelSettings>
        <chart:CircularDataLabelSettings LabelPosition="Outside"/>
    </chart:PieSeries.DataLabelSettings>
</chart:PieSeries>
```

### Explode + Selection

```xml
<chart:DoughnutSeries ExplodeOnTouch="True"
                      ExplodeRadius="12">
    <chart:DoughnutSeries.SelectionBehavior>
        <chart:DataPointSelectionBehavior SelectionBrush="#FF6B35"/>
    </chart:DoughnutSeries.SelectionBehavior>
</chart:DoughnutSeries>
```

### Explode + Tooltip

```csharp
series.ExplodeOnTouch = true;
series.ExplodeRadius = 15;
series.EnableTooltip = true;
```

## Best Practices

1. **Moderate Radius**: Use 10-20 pixels for most cases
2. **Clear Purpose**: Explode to emphasize important data, not for decoration
3. **Touch Interaction**: Enable `ExplodeOnTouch` for interactive dashboards
4. **Combine Features**: Pair explode with tooltips and labels for better UX
5. **ExplodeAll**: Use sparingly - works best with 3-6 segments
6. **Consistency**: If exploding one chart, consider consistency across similar charts

## When to Use Explode

**Good Use Cases:**
- Highlighting the largest or most important segment
- Drawing attention to outliers or special categories
- Interactive exploration of data
- Creating visual separation in dense charts

**Avoid When:**
- Chart has many segments (>8) - becomes cluttered
- All segments are equally important - use tooltips instead
- Space is limited - explosion may push segments out of view
- Chart updates frequently - constant animation can be distracting

## Troubleshooting

### Segment Not Exploding

**Check:**
- `ExplodeIndex` is within valid range (0 to data count - 1)
- `ExplodeRadius` is greater than 0
- Data is bound correctly to the series

### Touch Not Working

**Check:**
- `ExplodeOnTouch="True"` is set
- Chart has touch interaction enabled (not disabled by parent container)
- `ExplodeRadius` is set to a visible value

### All Segments Exploding When Only One Should

**Check:**
- `ExplodeAll` is not set to `true`
- Only `ExplodeIndex` should be set for single segment explosion
