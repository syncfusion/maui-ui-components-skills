# Rendering Position (Start Angle)

## Overview

The `StartAngle` property controls the starting position of the polar chart's primary axis, allowing you to rotate the entire chart to different orientations. This is useful for aligning data with natural reference points (e.g., compass directions) or creating specific visual layouts.

## StartAngle Property

### Available Values

The StartAngle property accepts these predefined values:

| Value | Degrees | Description |
|-------|---------|-------------|
| `ChartPolarAngle.Rotate0` | 0° | Start from right (3 o'clock) |
| `ChartPolarAngle.Rotate90` | 90° | Start from top (12 o'clock) |
| `ChartPolarAngle.Rotate180` | 180° | Start from left (9 o'clock) |
| `ChartPolarAngle.Rotate270` | 270° | Start from bottom (6 o'clock) - **Default** |

### Default Behavior

By default, polar charts use `Rotate270` (270°), which starts the axis from the bottom.

## Basic Implementation

### C# Implementation

```csharp
SfPolarChart chart = new SfPolarChart();

// Set start angle
chart.StartAngle = ChartPolarAngle.Rotate0;  // Start from right

// Configure axes
chart.PrimaryAxis = new CategoryAxis();
chart.SecondaryAxis = new NumericalAxis();

// Add series
chart.Series.Add(new PolarAreaSeries
{
    ItemsSource = data,
    XBindingPath = "Direction",
    YBindingPath = "Value"
});
```

### XAML Implementation

```xml
<chart:SfPolarChart StartAngle="Rotate0">
    <chart:SfPolarChart.PrimaryAxis>
        <chart:CategoryAxis/>
    </chart:SfPolarChart.PrimaryAxis>
    
    <chart:SfPolarChart.SecondaryAxis>
        <chart:NumericalAxis/>
    </chart:SfPolarChart.SecondaryAxis>
    
    <chart:PolarAreaSeries ItemsSource="{Binding PlantDetails}"
                           XBindingPath="Direction"
                           YBindingPath="Tree"/>
</chart:SfPolarChart>
```

## Use Cases by Angle

### Rotate0 (0°) - Start from Right

```csharp
chart.StartAngle = ChartPolarAngle.Rotate0;
```

**Best for:**
- Standard mathematical polar coordinates
- Technical or engineering charts
- Data starting from east/right

**Example - Engineering Data:**
```csharp
public class MeasurementData
{
    public int Angle { get; set; }  // 0, 45, 90, 135, 180, 225, 270, 315
    public double Force { get; set; }
}

chart.StartAngle = ChartPolarAngle.Rotate0;  // Mathematical convention
```

### Rotate90 (90°) - Start from Top

```csharp
chart.StartAngle = ChartPolarAngle.Rotate90;
```

**Best for:**
- Compass-style visualizations
- Directional data (North at top)
- Maps and navigation charts

**Example - Compass Directions:**
```csharp
public class WindData
{
    public string Direction { get; set; }  // N, NE, E, SE, S, SW, W, NW
    public double Speed { get; set; }
}

chart.StartAngle = ChartPolarAngle.Rotate90;  // North at top
chart.PrimaryAxis = new CategoryAxis();

// Data order: N, NE, E, SE, S, SW, W, NW
// Will display with N at top (12 o'clock position)
```

### Rotate180 (180°) - Start from Left

```csharp
chart.StartAngle = ChartPolarAngle.Rotate180;
```

**Best for:**
- Inverted or mirrored layouts
- Specific design requirements
- Alternative data presentation

**Example - Reversed Layout:**
```csharp
chart.StartAngle = ChartPolarAngle.Rotate180;  // Start from left side
```

### Rotate270 (270°) - Start from Bottom (Default)

```csharp
chart.StartAngle = ChartPolarAngle.Rotate270;  // Default, can be omitted
```

**Best for:**
- Default behavior (no specific orientation needed)
- General-purpose polar charts
- When orientation doesn't matter

## Common Patterns

### Pattern 1: Compass/Navigation Chart

```csharp
SfPolarChart chart = new SfPolarChart();
chart.StartAngle = ChartPolarAngle.Rotate90;  // North at top
chart.GridLineType = PolarChartGridLineType.Polygon;  // Radar style

CategoryAxis primaryAxis = new CategoryAxis();
chart.PrimaryAxis = primaryAxis;

NumericalAxis secondaryAxis = new NumericalAxis { Maximum = 100 };
chart.SecondaryAxis = secondaryAxis;

// Data with compass directions
var compassData = new List<DirectionData>
{
    new DirectionData { Direction = "N", Value = 80 },
    new DirectionData { Direction = "NE", Value = 65 },
    new DirectionData { Direction = "E", Value = 45 },
    new DirectionData { Direction = "SE", Value = 30 },
    new DirectionData { Direction = "S", Value = 20 },
    new DirectionData { Direction = "SW", Value = 35 },
    new DirectionData { Direction = "W", Value = 50 },
    new DirectionData { Direction = "NW", Value = 70 }
};

chart.Series.Add(new PolarAreaSeries
{
    ItemsSource = compassData,
    XBindingPath = "Direction",
    YBindingPath = "Value"
});
```

### Pattern 2: Mathematical/Engineering Polar Chart

```csharp
SfPolarChart chart = new SfPolarChart();
chart.StartAngle = ChartPolarAngle.Rotate0;  // 0° from right (mathematical standard)

NumericalAxis primaryAxis = new NumericalAxis
{
    Minimum = 0,
    Maximum = 360,
    Interval = 45
};
chart.PrimaryAxis = primaryAxis;

NumericalAxis secondaryAxis = new NumericalAxis
{
    Minimum = 0,
    Maximum = 10
};
chart.SecondaryAxis = secondaryAxis;

// Angular measurements
var angularData = new List<PolarPoint>
{
    new PolarPoint { Angle = 0, Magnitude = 5 },
    new PolarPoint { Angle = 45, Magnitude = 7 },
    new PolarPoint { Angle = 90, Magnitude = 8 },
    // ... more points
};

chart.Series.Add(new PolarLineSeries
{
    ItemsSource = angularData,
    XBindingPath = "Angle",
    YBindingPath = "Magnitude"
});
```

### Pattern 3: Dynamic Rotation

Allow users to change the chart orientation:

```csharp
public class ChartViewModel : INotifyPropertyChanged
{
    private ChartPolarAngle _startAngle = ChartPolarAngle.Rotate90;
    
    public ChartPolarAngle StartAngle
    {
        get => _startAngle;
        set
        {
            _startAngle = value;
            OnPropertyChanged(nameof(StartAngle));
        }
    }
    
    public void RotateChart()
    {
        // Cycle through angles
        StartAngle = StartAngle switch
        {
            ChartPolarAngle.Rotate0 => ChartPolarAngle.Rotate90,
            ChartPolarAngle.Rotate90 => ChartPolarAngle.Rotate180,
            ChartPolarAngle.Rotate180 => ChartPolarAngle.Rotate270,
            ChartPolarAngle.Rotate270 => ChartPolarAngle.Rotate0,
            _ => ChartPolarAngle.Rotate0
        };
    }
}
```

**XAML:**
```xml
<StackLayout>
    <Button Text="Rotate Chart" Clicked="OnRotateClicked"/>
    
    <chart:SfPolarChart StartAngle="{Binding StartAngle}">
        <!-- Chart configuration -->
    </chart:SfPolarChart>
</StackLayout>
```

## Practical Examples

### Example 1: Wind Rose Chart

```csharp
// Wind data by direction
var windData = new List<WindMeasurement>
{
    new WindMeasurement { Direction = "N", Frequency = 15 },
    new WindMeasurement { Direction = "NNE", Frequency = 12 },
    new WindMeasurement { Direction = "NE", Frequency = 10 },
    // ... 16 directions total
};

SfPolarChart chart = new SfPolarChart();
chart.StartAngle = ChartPolarAngle.Rotate90;  // North at top
chart.GridLineType = PolarChartGridLineType.Polygon;

chart.Series.Add(new PolarAreaSeries
{
    ItemsSource = windData,
    XBindingPath = "Direction",
    YBindingPath = "Frequency",
    Fill = new SolidColorBrush(Colors.LightBlue),
    Opacity = 0.6
});
```

### Example 2: Skill Assessment (Default Angle)

```csharp
// Skills don't need specific orientation
var skills = new List<SkillScore>
{
    new SkillScore { Skill = "Communication", Score = 85 },
    new SkillScore { Skill = "Leadership", Score = 78 },
    new SkillScore { Skill = "Technical", Score = 92 },
    new SkillScore { Skill = "Creativity", Score = 75 },
    new SkillScore { Skill = "Teamwork", Score = 88 }
};

SfPolarChart chart = new SfPolarChart();
// Use default StartAngle (Rotate270) - orientation doesn't matter
chart.GridLineType = PolarChartGridLineType.Polygon;

chart.Series.Add(new PolarLineSeries
{
    ItemsSource = skills,
    XBindingPath = "Skill",
    YBindingPath = "Score",
    ShowMarkers = true
});
```

### Example 3: Time-Based Circular Data

```csharp
// Hourly data in 24-hour cycle
var hourlyData = new List<HourlyMetric>
{
    new HourlyMetric { Hour = 0, Value = 10 },   // Midnight
    new HourlyMetric { Hour = 6, Value = 15 },   // 6 AM
    new HourlyMetric { Hour = 12, Value = 25 },  // Noon
    new HourlyMetric { Hour = 18, Value = 20 },  // 6 PM
};

SfPolarChart chart = new SfPolarChart();
chart.StartAngle = ChartPolarAngle.Rotate90;  // 0 hours (midnight) at top

NumericalAxis primaryAxis = new NumericalAxis
{
    Minimum = 0,
    Maximum = 24,
    Interval = 6
};
chart.PrimaryAxis = primaryAxis;
```

## Tips and Best Practices

### Choose StartAngle Based on Data Type

| Data Type | Recommended Angle | Reason |
|-----------|------------------|---------|
| Compass directions | Rotate90 (90°) | North at top is intuitive |
| Mathematical polar | Rotate0 (0°) | Standard convention |
| Time-based (clock) | Rotate90 (90°) | 12 o'clock at top |
| General categories | Rotate270 (270°) | Default, no bias |

### Consistency Across Charts

If you have multiple polar charts in the same application, use the same StartAngle for consistency:

```csharp
// Define constant for all charts
public const ChartPolarAngle StandardStartAngle = ChartPolarAngle.Rotate90;

// Apply to all charts
chart1.StartAngle = StandardStartAngle;
chart2.StartAngle = StandardStartAngle;
chart3.StartAngle = StandardStartAngle;
```

### Consider User Expectations

```csharp
// For compass/map data - users expect North at top
if (isCompassData)
{
    chart.StartAngle = ChartPolarAngle.Rotate90;
}
// For general data - use default
else
{
    chart.StartAngle = ChartPolarAngle.Rotate270;
}
```

## Troubleshooting

### Chart Appears Rotated Incorrectly

**Problem:** Chart doesn't start from expected position.

**Solution:**
```csharp
// Verify StartAngle is set correctly
chart.StartAngle = ChartPolarAngle.Rotate90;  // Explicit setting

// Check data order matches expected rotation
// First data point will appear at StartAngle position
```

### Data Not Aligned with Labels

**Problem:** Data points don't align with expected compass directions.

**Solution:**
```csharp
// Ensure data order matches rotation
// For Rotate90 with North at top, first item should be "N"
var data = new List<DirectionData>
{
    new DirectionData { Direction = "N", Value = 80 },    // First = top
    new DirectionData { Direction = "NE", Value = 65 },   // Second = clockwise
    // ...
};

chart.StartAngle = ChartPolarAngle.Rotate90;
```

## Related Topics

- **Getting Started**: [getting-started.md](getting-started.md) - Basic chart setup
- **Axis Configuration**: [axis-configuration.md](axis-configuration.md) - Axis types and properties
- **Series Types**: [series-types.md](series-types.md) - How series display with different angles
