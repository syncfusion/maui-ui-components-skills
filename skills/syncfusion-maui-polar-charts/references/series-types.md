# Polar Chart Series Types

## Table of Contents
- [Overview](#overview)
- [Polar Line Series](#polar-line-series)
- [Polar Area Series](#polar-area-series)
- [Grid Line Types](#grid-line-types)
- [Closed vs Open Series](#closed-vs-open-series)
- [Multiple Series Support](#multiple-series-support)
- [Markers](#markers)
- [Choosing the Right Series Type](#choosing-the-right-series-type)

## Overview

Syncfusion .NET MAUI Polar Chart supports two types of series for visualizing data in polar coordinates:

1. **PolarLineSeries** - Displays data as connected line segments
2. **PolarAreaSeries** - Displays data as a filled area

Both series types support:
- Circle and polygon grid lines (polar vs radar appearance)
- Closed and open paths
- Multiple series overlays
- Customizable markers
- Data labels and tooltips

## Polar Line Series

PolarLineSeries displays data points connected by line segments in a circular layout. This is ideal for showing trends and patterns across angular dimensions.

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

    <chart:PolarLineSeries ItemsSource="{Binding PlantDetails}" 
                           XBindingPath="Direction" 
                           YBindingPath="Tree"
                           Label="Tree Data"/>
</chart:SfPolarChart>
```

**C#:**
```csharp
SfPolarChart chart = new SfPolarChart();

// Configure axes
chart.PrimaryAxis = new CategoryAxis();
chart.SecondaryAxis = new NumericalAxis();

// Create polar line series
PolarLineSeries series = new PolarLineSeries
{
    ItemsSource = new PlantViewModel().PlantDetails,
    XBindingPath = "Direction",
    YBindingPath = "Tree",
    Label = "Tree Data"
};

chart.Series.Add(series);
this.Content = chart;
```

### Customizing Line Appearance

```csharp
PolarLineSeries series = new PolarLineSeries
{
    ItemsSource = data,
    XBindingPath = "Direction",
    YBindingPath = "Value",
    Stroke = new SolidColorBrush(Colors.Blue),  // Line color
    StrokeWidth = 3,                              // Line thickness
    StrokeDashArray = new double[] { 5, 2 },     // Dashed line pattern
    ShowMarkers = true                            // Show data point markers
};
```

### When to Use Polar Line Series

Use PolarLineSeries when you need to:
- **Show trends** - Visualize how values change across categories
- **Compare patterns** - Overlay multiple line series to compare datasets
- **Emphasize connections** - Highlight relationships between adjacent points
- **Minimize visual weight** - Keep the chart clean with just lines, no fills

**Use Cases:**
- Performance metrics across categories
- Skill assessments (spider charts)
- Directional measurements (wind patterns, compass data)
- Time-series cyclic data (monthly trends)

## Polar Area Series

PolarAreaSeries displays data as a filled area, making it easy to see the magnitude or coverage of values.

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

    <chart:PolarAreaSeries ItemsSource="{Binding PlantDetails}" 
                           XBindingPath="Direction" 
                           YBindingPath="Tree"
                           Label="Tree Coverage"/>
</chart:SfPolarChart>
```

**C#:**
```csharp
SfPolarChart chart = new SfPolarChart();

// Configure axes
chart.PrimaryAxis = new CategoryAxis();
chart.SecondaryAxis = new NumericalAxis();

// Create polar area series
PolarAreaSeries series = new PolarAreaSeries
{
    ItemsSource = new PlantViewModel().PlantDetails,
    XBindingPath = "Direction",
    YBindingPath = "Tree",
    Label = "Tree Coverage"
};

chart.Series.Add(series);
this.Content = chart;
```

### Customizing Area Appearance

```csharp
PolarAreaSeries series = new PolarAreaSeries
{
    ItemsSource = data,
    XBindingPath = "Category",
    YBindingPath = "Value",
    Fill = new SolidColorBrush(Colors.LightBlue),  // Area fill color
    Opacity = 0.6,                                   // Transparency
    Stroke = new SolidColorBrush(Colors.Blue),      // Border color
    StrokeWidth = 2,                                 // Border thickness
    ShowMarkers = true                               // Show data point markers
};
```

### When to Use Polar Area Series

Use PolarAreaSeries when you need to:
- **Show magnitude** - Emphasize the size or coverage of values
- **Highlight coverage areas** - Visualize filled regions
- **Compare volumes** - See the relative size of different datasets
- **Emphasize presence** - Show that values exist across categories

**Use Cases:**
- Coverage maps (signal strength, service areas)
- Competency assessments (showing skill coverage)
- Survey results (showing response coverage)
- Resource allocation (showing distribution across categories)

## Grid Line Types

Polar charts support two grid line rendering styles that dramatically change the chart's appearance:

### Circle Grid Lines (Polar Chart)

Circle grid lines create concentric circles, giving a traditional polar chart appearance.

**Implementation:**
```csharp
// Circle is the default
chart.GridLineType = PolarChartGridLineType.Circle;
```

**XAML:**
```xml
<chart:SfPolarChart GridLineType="Circle">
    <!-- Series here -->
</chart:SfPolarChart>
```

**Best for:** 
- True polar data (directional, circular patterns)
- Smooth, continuous data visualization
- Emphasizing radial distance

### Polygon Grid Lines (Radar/Spider Chart)

Polygon grid lines connect the axis points, creating a web-like appearance typical of radar or spider charts.

**Implementation:**
```csharp
chart.GridLineType = PolarChartGridLineType.Polygon;
```

**XAML:**
```xml
<chart:SfPolarChart GridLineType="Polygon">
    <!-- Series here -->
</chart:SfPolarChart>
```

**Best for:**
- Multi-dimensional comparisons
- Skills or competency charts
- Performance metrics across categories
- Emphasizing categorical segments

### Switching Grid Line Types Dynamically

```csharp
// Toggle between circle and polygon
private void ToggleGridType()
{
    if (chart.GridLineType == PolarChartGridLineType.Circle)
    {
        chart.GridLineType = PolarChartGridLineType.Polygon;
    }
    else
    {
        chart.GridLineType = PolarChartGridLineType.Circle;
    }
}
```

## Closed vs Open Series

The `IsClosed` property determines whether the series connects the first and last data points.

### Closed Series (Default)

**IsClosed = true** - The series forms a complete loop by connecting the last point back to the first.

```csharp
PolarLineSeries series = new PolarLineSeries
{
    ItemsSource = data,
    XBindingPath = "Direction",
    YBindingPath = "Value",
    IsClosed = true  // Default - creates closed loop
};
```

**XAML:**
```xml
<chart:PolarLineSeries ItemsSource="{Binding Data}" 
                       XBindingPath="Direction" 
                       YBindingPath="Value"
                       IsClosed="True"/>
```

**Use when:**
- Data is cyclic or continuous (compass directions, months)
- Comparing complete coverage across all categories
- Creating spider/radar charts

### Open Series

**IsClosed = false** - The series does not connect the last point to the first, leaving a gap.

```csharp
PolarLineSeries series = new PolarLineSeries
{
    ItemsSource = data,
    XBindingPath = "Direction",
    YBindingPath = "Value",
    IsClosed = false  // Creates open path
};
```

**XAML:**
```xml
<chart:PolarLineSeries ItemsSource="{Binding Data}" 
                       XBindingPath="Direction" 
                       YBindingPath="Value"
                       IsClosed="False"/>
```

**Use when:**
- Data has a natural start and end (non-cyclic)
- Showing partial coverage
- Emphasizing that data doesn't complete a full cycle

## Multiple Series Support

Polar charts can display multiple series simultaneously, ideal for comparisons.

### Overlaying Multiple Series

**XAML:**
```xml
<chart:SfPolarChart>
    <chart:SfPolarChart.PrimaryAxis>
        <chart:CategoryAxis/>
    </chart:SfPolarChart.PrimaryAxis>

    <chart:SfPolarChart.SecondaryAxis>
        <chart:NumericalAxis Maximum="100"/>
    </chart:SfPolarChart.SecondaryAxis>

    <!-- Multiple series for comparison -->
    <chart:PolarLineSeries ItemsSource="{Binding PlantDetails}" 
                           XBindingPath="Direction" 
                           YBindingPath="Tree"
                           Label="Trees"/>
    
    <chart:PolarLineSeries ItemsSource="{Binding PlantDetails}" 
                           XBindingPath="Direction" 
                           YBindingPath="Weed"
                           Label="Weeds"/>

    <chart:PolarLineSeries ItemsSource="{Binding PlantDetails}" 
                           XBindingPath="Direction" 
                           YBindingPath="Flower"
                           Label="Flowers"/>
</chart:SfPolarChart>
```

**C#:**
```csharp
SfPolarChart chart = new SfPolarChart();

// Configure axes
chart.PrimaryAxis = new CategoryAxis();
chart.SecondaryAxis = new NumericalAxis { Maximum = 100 };

// Add multiple series
chart.Series.Add(new PolarLineSeries
{
    ItemsSource = viewModel.PlantDetails,
    XBindingPath = "Direction",
    YBindingPath = "Tree",
    Label = "Trees"
});

chart.Series.Add(new PolarLineSeries
{
    ItemsSource = viewModel.PlantDetails,
    XBindingPath = "Direction",
    YBindingPath = "Weed",
    Label = "Weeds"
});

chart.Series.Add(new PolarLineSeries
{
    ItemsSource = viewModel.PlantDetails,
    XBindingPath = "Direction",
    YBindingPath = "Flower",
    Label = "Flowers"
});
```

### Mixing Line and Area Series

```csharp
// Add area series as background
chart.Series.Add(new PolarAreaSeries
{
    ItemsSource = data,
    XBindingPath = "Category",
    YBindingPath = "BaselineValue",
    Label = "Baseline",
    Opacity = 0.3,
    Fill = new SolidColorBrush(Colors.LightGray)
});

// Add line series for actual values
chart.Series.Add(new PolarLineSeries
{
    ItemsSource = data,
    XBindingPath = "Category",
    YBindingPath = "ActualValue",
    Label = "Actual",
    Stroke = new SolidColorBrush(Colors.Blue),
    StrokeWidth = 3
});
```

## Markers

Markers highlight individual data points on the series. Both PolarLineSeries and PolarAreaSeries support markers.

### Enable Markers

```csharp
PolarLineSeries series = new PolarLineSeries
{
    ItemsSource = data,
    XBindingPath = "Category",
    YBindingPath = "Value",
    ShowMarkers = true  // Enable markers
};
```

### Customize Marker Appearance

```csharp
PolarLineSeries series = new PolarLineSeries
{
    ItemsSource = data,
    XBindingPath = "Category",
    YBindingPath = "Value",
    ShowMarkers = true
};

series.MarkerSettings = new ChartMarkerSettings
{
    Type = ShapeType.Diamond,           // Marker shape
    Fill = Colors.Red,                   // Fill color
    Stroke = Colors.DarkRed,            // Border color
    StrokeWidth = 2,                     // Border thickness
    Width = 12,                          // Marker width
    Height = 12                          // Marker height
};
```

**Available marker shapes:**
- Circle
- Cross
- Diamond
- Hexagon
- InvertedTriangle
- Pentagon
- Plus
- Rectangle
- Triangle

**Learn more:** See [data-labels-markers.md](data-labels-markers.md) for comprehensive marker customization.

## Choosing the Right Series Type

### Use PolarLineSeries When:

| Scenario | Why |
|----------|-----|
| Tracking trends | Lines show changes clearly |
| Multiple comparisons | Lines don't obscure each other |
| Emphasizing connections | Lines show relationships between points |
| Clean, minimal look | Less visual clutter than filled areas |
| Performance metrics | Standard choice for spider/radar charts |

### Use PolarAreaSeries When:

| Scenario | Why |
|----------|-----|
| Showing magnitude/coverage | Filled area emphasizes size |
| Single series focus | Area clearly shows the extent |
| Highlighting presence | Area shows "what is covered" |
| Survey/assessment results | Area shows competency coverage |
| Resource distribution | Area shows allocation across categories |

### Comparison Matrix

| Feature | PolarLineSeries | PolarAreaSeries |
|---------|----------------|-----------------|
| Visual Weight | Light | Heavy |
| Best for Overlays | ✓ Excellent | Limited (use transparency) |
| Shows Magnitude | Good | ✓ Excellent |
| Emphasizes Trends | ✓ Excellent | Good |
| Multiple Series | ✓ Ideal | Challenging (overlapping) |
| Single Series | Good | ✓ Ideal |

## Common Patterns

### Pattern 1: Spider Chart for Skills Assessment

```csharp
SfPolarChart chart = new SfPolarChart();
chart.GridLineType = PolarChartGridLineType.Polygon;  // Radar style

PolarLineSeries candidateSeries = new PolarLineSeries
{
    ItemsSource = candidateSkills,
    XBindingPath = "Skill",
    YBindingPath = "Score",
    Label = "Candidate",
    ShowMarkers = true
};

chart.Series.Add(candidateSeries);
```

### Pattern 2: Directional Data with Circle Grid

```csharp
SfPolarChart chart = new SfPolarChart();
chart.GridLineType = PolarChartGridLineType.Circle;  // Polar style

PolarAreaSeries windSeries = new PolarAreaSeries
{
    ItemsSource = windData,
    XBindingPath = "Direction",  // N, NE, E, SE, S, SW, W, NW
    YBindingPath = "Speed",
    Fill = new SolidColorBrush(Colors.LightBlue),
    Opacity = 0.6
};

chart.Series.Add(windSeries);
```

### Pattern 3: Comparison with Multiple Line Series

```csharp
SfPolarChart chart = new SfPolarChart();
chart.GridLineType = PolarChartGridLineType.Polygon;

// Add series for each team member
foreach (var member in teamMembers)
{
    chart.Series.Add(new PolarLineSeries
    {
        ItemsSource = member.Assessments,
        XBindingPath = "Skill",
        YBindingPath = "Score",
        Label = member.Name,
        ShowMarkers = true
    });
}
```

## Troubleshooting

### Series Not Visible

**Problem:** Series added but not showing on chart.

**Solutions:**
```csharp
// 1. Check data exists
Console.WriteLine($"Data count: {data.Count}");

// 2. Verify binding paths match property names (case-sensitive)
XBindingPath = "Direction"  // Must match property name exactly

// 3. Check axis ranges
chart.SecondaryAxis = new NumericalAxis
{
    Minimum = 0,
    Maximum = 100  // Ensure data fits within range
};

// 4. Verify series was added to chart
chart.Series.Add(series);
```

### Overlapping Area Series Not Visible

**Problem:** Multiple area series overlap and hide each other.

**Solution:** Use transparency or switch to line series:
```csharp
// Option 1: Add transparency
PolarAreaSeries series = new PolarAreaSeries
{
    // ... properties
    Opacity = 0.5  // Make semi-transparent
};

// Option 2: Use line series for comparisons
PolarLineSeries series = new PolarLineSeries { /* ... */ };
```

## Related Topics

- **Getting Started**: [getting-started.md](getting-started.md) - Basic chart setup
- **Markers**: [data-labels-markers.md](data-labels-markers.md) - Detailed marker customization
- **Styling**: [appearance-customization.md](appearance-customization.md) - Colors, gradients, themes
- **Axes**: [axis-configuration.md](axis-configuration.md) - Axis types and customization
