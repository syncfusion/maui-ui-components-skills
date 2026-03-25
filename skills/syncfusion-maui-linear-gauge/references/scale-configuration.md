# Scale Configuration

## Table of Contents
- [Overview](#overview)
- [Default Scale Behavior](#default-scale-behavior)
- [Minimum and Maximum Values](#minimum-and-maximum-values)
- [Scale Intervals](#scale-intervals)
- [Scale Appearance](#scale-appearance)
- [Orientation](#orientation)
- [Inverse Scale](#inverse-scale)
- [Scale Positioning and Offset](#scale-positioning-and-offset)
- [Common Scale Patterns](#common-scale-patterns)

## Overview

The scale is the backbone of a linear gauge - it defines the range of values that can be displayed and provides the visual track for all other gauge elements. Understanding scale configuration is essential for creating effective data visualizations.

**Key Scale Properties:**
- Minimum/Maximum values define the data range
- Interval controls label spacing
- Appearance properties (thickness, fill, edge style) control visual design
- Orientation determines layout direction (horizontal/vertical)
- IsInversed reverses value direction

## Default Scale Behavior

Without any configuration, a linear gauge displays a scale from 0 to 100:

**XAML:**
```xml
<gauge:SfLinearGauge />
```

**C#:**
```csharp
SfLinearGauge gauge = new SfLinearGauge();
this.Content = gauge;
```

**Default Properties:**
- Minimum: 0
- Maximum: 100
- Interval: 10 (auto-calculated)
- Orientation: Horizontal
- Scale thickness: Default size
- Labels and ticks: Enabled

## Minimum and Maximum Values

Customize the scale range to match your data:

### Basic Range Setting

**XAML:**
```xml
<gauge:SfLinearGauge Minimum="-50" Maximum="50" />
```

**C#:**
```csharp
SfLinearGauge gauge = new SfLinearGauge
{
    Minimum = -50,
    Maximum = 50
};
```

### Common Range Scenarios

**Temperature Scale (Celsius):**
```xml
<gauge:SfLinearGauge Minimum="-20" Maximum="50" />
```

**Percentage Scale:**
```xml
<gauge:SfLinearGauge Minimum="0" Maximum="100" />
```

**Speed Meter (MPH):**
```xml
<gauge:SfLinearGauge Minimum="0" Maximum="160" />
```

**Financial Scale (with negatives):**
```csharp
SfLinearGauge gauge = new SfLinearGauge
{
    Minimum = -1000,
    Maximum = 1000
};
```

### Dynamic Range Updates

Update scale range at runtime:

```csharp
private void UpdateScaleRange(double min, double max)
{
    gauge.Minimum = min;
    gauge.Maximum = max;
}

// Example: Auto-scale based on data
private void AutoScale(List<double> dataPoints)
{
    if (dataPoints.Any())
    {
        double minValue = dataPoints.Min();
        double maxValue = dataPoints.Max();
        
        // Add 10% padding
        double padding = (maxValue - minValue) * 0.1;
        gauge.Minimum = minValue - padding;
        gauge.Maximum = maxValue + padding;
    }
}
```

## Scale Intervals

The Interval property controls the spacing between scale labels and major ticks.

### Auto Interval (Default)

When Interval is not set, the gauge calculates it automatically:

```xml
<gauge:SfLinearGauge Minimum="0" Maximum="100" />
<!-- Auto-generates interval of 10 -->
```

### Custom Interval

**XAML:**
```xml
<gauge:SfLinearGauge Minimum="0" Maximum="100" Interval="20" />
<!-- Shows labels at: 0, 20, 40, 60, 80, 100 -->
```

**C#:**
```csharp
SfLinearGauge gauge = new SfLinearGauge
{
    Minimum = 0,
    Maximum = 100,
    Interval = 20
};
```

### Interval Best Practices

**Even Distribution:**
```csharp
// Good: Interval divides range evenly
gauge.Minimum = 0;
gauge.Maximum = 100;
gauge.Interval = 10;  // Creates 10 segments
```

**Avoid Overcrowding:**
```csharp
// Too many labels (21 labels!)
gauge.Minimum = 0;
gauge.Maximum = 100;
gauge.Interval = 5;   // Consider larger interval

// Better for readability
gauge.Interval = 10;  // 10 labels
```

**Temperature Scale Example:**
```xml
<gauge:SfLinearGauge Minimum="-20" Maximum="50" Interval="10" />
<!-- Labels: -20, -10, 0, 10, 20, 30, 40, 50 -->
```

### Dynamic Interval Calculation

Calculate interval based on data range:

```csharp
private void SetOptimalInterval(double min, double max)
{
    double range = max - min;
    
    // Target 5-10 labels
    double targetLabels = 8;
    double interval = range / targetLabels;
    
    // Round to nice number
    interval = RoundToNiceNumber(interval);
    
    gauge.Minimum = min;
    gauge.Maximum = max;
    gauge.Interval = interval;
}

private double RoundToNiceNumber(double value)
{
    // Round to nearest 1, 2, 5, 10, 20, 50, 100, etc.
    double magnitude = Math.Pow(10, Math.Floor(Math.Log10(value)));
    double normalized = value / magnitude;
    
    if (normalized < 1.5) return 1 * magnitude;
    if (normalized < 3) return 2 * magnitude;
    if (normalized < 7) return 5 * magnitude;
    return 10 * magnitude;
}
```

## Scale Appearance

Customize the visual appearance of the scale track.

### Scale Line Style

Control scale track appearance with `LineStyle` property:

**XAML:**
```xml
<gauge:SfLinearGauge>
    <gauge:SfLinearGauge.LineStyle>
        <gauge:LinearLineStyle Thickness="10" 
                               Fill="#E0E0E0"
                               CornerStyle="BothCurve"/>
    </gauge:SfLinearGauge.LineStyle>
</gauge:SfLinearGauge>
```

**C#:**
```csharp
gauge.LineStyle = new LinearLineStyle
{
    Thickness = 10,
    Fill = new SolidColorBrush(Color.FromArgb("#E0E0E0")),
    CornerStyle = CornerStyle.BothCurve
};
```

### Scale Thickness

**Thin Scale (Minimalist):**
```xml
<gauge:SfLinearGauge.LineStyle>
    <gauge:LinearLineStyle Thickness="2" Fill="#CCCCCC"/>
</gauge:SfLinearGauge.LineStyle>
```

**Medium Scale (Default):**
```xml
<gauge:LinearLineStyle Thickness="8" Fill="#E0E0E0"/>
```

**Thick Scale (Prominent):**
```xml
<gauge:LinearLineStyle Thickness="20" Fill="#BDBDBD"/>
```

### Corner Styles

Control the edge appearance of the scale track:

**Both Curved (Rounded Ends):**
```xml
<gauge:LinearLineStyle Thickness="15" 
                       Fill="#90CAF9"
                       CornerStyle="BothCurve"/>
```

**Both Flat (Square Ends):**
```xml
<gauge:LinearLineStyle CornerStyle="BothFlat"/>
```

**Start Curve (Left/Top Rounded):**
```xml
<gauge:LinearLineStyle CornerStyle="StartCurve"/>
```

**End Curve (Right/Bottom Rounded):**
```xml
<gauge:LinearLineStyle CornerStyle="EndCurve"/>
```

### Scale Fill Colors

**Solid Color:**
```csharp
gauge.LineStyle = new LinearLineStyle
{
    Fill = new SolidColorBrush(Colors.LightBlue)
};
```

**Gradient Fill:**
```csharp
if(gauge.LineStyle != null)
{
gauge.LineStyle.GradientStops.Add(new GaugeGradientStop() { Value = 0, Color = Colors.Red });
gauge.LineStyle.GradientStops.Add(new GaugeGradientStop() { Value = 100, Color = Colors.Blue });
}
```

## Orientation

Linear gauges support horizontal and vertical orientations.

### Horizontal Orientation (Default)

**XAML:**
```xml
<gauge:SfLinearGauge Orientation="Horizontal" />
```

**C#:**
```csharp
gauge.Orientation = GaugeOrientation.Horizontal;
```

**Best for:**
- Progress bars
- Timeline visualizations
- Horizontal dashboards
- Wide layout spaces

### Vertical Orientation

**XAML:**
```xml
<gauge:SfLinearGauge Orientation="Vertical" />
```

**C#:**
```csharp
gauge.Orientation = GaugeOrientation.Vertical;
```

**Best for:**
- Thermometers
- Volume/level indicators
- Tall layout spaces
- Side panels

### Orientation Comparison Example

```xml
<HorizontalStackLayout Spacing="50">
    
    <!-- Horizontal gauge -->
    <VerticalStackLayout>
        <Label Text="Speed (MPH)" HorizontalOptions="Center"/>
        <gauge:SfLinearGauge Minimum="0" 
                            Maximum="120" 
                            Orientation="Horizontal"
                            WidthRequest="300">
            <gauge:SfLinearGauge.BarPointers>
                <gauge:BarPointer Value="65" Fill="#4CAF50"/>
            </gauge:SfLinearGauge.BarPointers>
        </gauge:SfLinearGauge>
    </VerticalStackLayout>
    
    <!-- Vertical gauge -->
    <VerticalStackLayout>
        <Label Text="Temperature (°C)" HorizontalOptions="Center"/>
        <gauge:SfLinearGauge Minimum="-20" 
                            Maximum="50" 
                            Orientation="Vertical"
                            HeightRequest="300">
            <gauge:SfLinearGauge.BarPointers>
                <gauge:BarPointer Value="23" Fill="#FF5722"/>
            </gauge:SfLinearGauge.BarPointers>
        </gauge:SfLinearGauge>
    </VerticalStackLayout>
    
</HorizontalStackLayout>
```

## Inverse Scale

Reverse the direction of value progression with `IsInversed`.

### Normal Scale Direction

**Horizontal:** Left (min) → Right (max)  
**Vertical:** Bottom (min) → Top (max)

```xml
<gauge:SfLinearGauge IsInversed="False" />
```

### Inversed Scale Direction

**Horizontal:** Right (min) → Left (max)  
**Vertical:** Top (min) → Bottom (max)

**XAML:**
```xml
<gauge:SfLinearGauge IsInversed="True" />
```

**C#:**
```csharp
gauge.IsInversed = true;
```

### Inverse Scale Use Cases

**Countdown Timer:**
```xml
<gauge:SfLinearGauge Minimum="0" 
                    Maximum="60" 
                    IsInversed="True">
    <gauge:SfLinearGauge.BarPointers>
        <gauge:BarPointer Value="{Binding TimeRemaining}" 
                         Fill="#F44336"/>
    </gauge:SfLinearGauge.BarPointers>
</gauge:SfLinearGauge>
```

**Depth Indicator (Vertical):**
```csharp
SfLinearGauge depthGauge = new SfLinearGauge
{
    Minimum = 0,
    Maximum = 100,
    Orientation = GaugeOrientation.Vertical,
    IsInversed = true  // 0 at top, 100 at bottom
};
```

**Right-to-Left Layout:**
```xml
<gauge:SfLinearGauge IsInversed="True" 
                    Minimum="0" 
                    Maximum="100"/>
<!-- Values decrease left to right -->
```

## Scale Positioning and Offset

Control the position of scale elements relative to the gauge container.

### Scale Offset

Position labels and ticks relative to scale:

```xml
<gauge:SfLinearGauge LabelOffset="10" TickOffset="-5"/>
```

**Positive offset:** Moves away from scale  
**Negative offset:** Moves toward scale

## Common Scale Patterns

### Pattern 1: Percentage Progress Bar

```xml
<gauge:SfLinearGauge Minimum="0" 
                    Maximum="100" 
                    Interval="25"
                    ShowTicks="False">
    <gauge:SfLinearGauge.LineStyle>
        <gauge:LinearLineStyle Thickness="20" 
                               Fill="#E0E0E0"
                               CornerStyle="BothCurve"/>
    </gauge:SfLinearGauge.LineStyle>
    
    <gauge:SfLinearGauge.BarPointers>
        <gauge:BarPointer Value="67" 
                         Fill="#4CAF50"
                         EnableAnimation="True"
                         CornerStyle="BothCurve"/>
    </gauge:SfLinearGauge.BarPointers>
</gauge:SfLinearGauge>
```

### Pattern 2: Temperature Thermometer

```csharp
SfLinearGauge thermometer = new SfLinearGauge
{
    Minimum = -20,
    Maximum = 50,
    Interval = 10,
    Orientation = GaugeOrientation.Vertical,
    HeightRequest = 400
};

thermometer.LineStyle = new LinearLineStyle
{
    Thickness = 12,
    Fill = new SolidColorBrush(Colors.LightGray),
    CornerStyle = CornerStyle.BothCurve
};

thermometer.BarPointers.Add(new BarPointer
{
    Value = 23,
    Fill = new SolidColorBrush(Colors.Red),
    EnableAnimation = true
});
```

### Pattern 3: Bidirectional Scale (Positive/Negative)

```xml
<gauge:SfLinearGauge Minimum="-100" 
                    Maximum="100" 
                    Interval="25">
    <gauge:SfLinearGauge.Ranges>
        <!-- Negative range (red) -->
        <gauge:LinearRange StartValue="-100" 
                          EndValue="0" 
                          Fill="#FFCDD2"/>
        <!-- Positive range (green) -->
        <gauge:LinearRange StartValue="0" 
                          EndValue="100" 
                          Fill="#C8E6C9"/>
    </gauge:SfLinearGauge.Ranges>
    
    <gauge:SfLinearGauge.MarkerPointers>
        <gauge:LinearShapePointer Value="35" 
                                 ShapeType="Diamond"
                                 Fill="#4CAF50"/>
    </gauge:SfLinearGauge.MarkerPointers>
</gauge:SfLinearGauge>
```

### Pattern 4: Minimalist Scale (No Decorations)

```xml
<gauge:SfLinearGauge Minimum="0" 
                    Maximum="10" 
                    ShowLabels="False"
                    ShowTicks="False">
    <gauge:SfLinearGauge.LineStyle>
        <gauge:LinearLineStyle Thickness="4" Fill="#BDBDBD"/>
    </gauge:SfLinearGauge.LineStyle>
    
    <gauge:SfLinearGauge.MarkerPointers>
        <gauge:LinearShapePointer Value="7.5" 
                                 ShapeType="Circle"
                                 Fill="#2196F3"/>
    </gauge:SfLinearGauge.MarkerPointers>
</gauge:SfLinearGauge>
```

### Pattern 5: Multi-Range Scale

```csharp
SfLinearGauge multiRangeGauge = new SfLinearGauge
{
    Minimum = 0,
    Maximum = 200,
    Interval = 20
};

// Low range (0-60)
multiRangeGauge.Ranges.Add(new LinearRange
{
    StartValue = 0,
    EndValue = 60,
    Fill = new SolidColorBrush(Colors.Green)
});

// Medium range (60-120)
multiRangeGauge.Ranges.Add(new LinearRange
{
    StartValue = 60,
    EndValue = 120,
    Fill = new SolidColorBrush(Colors.Yellow)
});

// High range (120-160)
multiRangeGauge.Ranges.Add(new LinearRange
{
    StartValue = 120,
    EndValue = 160,
    Fill = new SolidColorBrush(Colors.Orange)
});

// Critical range (160-200)
multiRangeGauge.Ranges.Add(new LinearRange
{
    StartValue = 160,
    EndValue = 200,
    Fill = new SolidColorBrush(Colors.Red)
});
```
