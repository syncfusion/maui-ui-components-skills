# Labels and Ticks

## Table of Contents
- [Overview](#overview)
- [Labels](#labels)
  - [Label Styling](#label-styling)
  - [Label Position](#label-position)
  - [Label Offset](#label-offset)
  - [Label Visibility](#label-visibility)
  - [Label Format](#label-format)
  - [Custom Label Text](#custom-label-text)
  - [Maximum Labels Count](#maximum-labels-count)
- [Ticks](#ticks)
  - [Major Ticks](#major-ticks)
  - [Minor Ticks](#minor-ticks)
  - [Tick Position](#tick-position)
  - [Tick Offset](#tick-offset)
  - [Tick Visibility](#tick-visibility)
- [Combined Examples](#combined-examples)

## Overview

Labels and ticks are essential scale elements that help users interpret gauge values. Labels display the numeric values, while ticks provide visual markers along the scale.

**Labels:** Show numeric values at intervals along the scale  
**Major Ticks:** Larger tick marks aligned with labels  
**Minor Ticks:** Smaller tick marks between major ticks

## Labels

### Label Styling

Customize label appearance using the `LabelStyle` property.

**Available Style Properties:**
- `TextColor` - Label text color
- `FontFamily` - Font typeface
- `FontSize` - Text size
- `FontAttributes` - Bold, Italic, or both

**XAML:**
```xml
<gauge:SfLinearGauge>
    <gauge:SfLinearGauge.LabelStyle>
        <gauge:LinearLabelStyle TextColor="#2196F3"
                               FontSize="14"
                               FontFamily="Arial"
                               FontAttributes="Bold"/>
    </gauge:SfLinearGauge.LabelStyle>
</gauge:SfLinearGauge>
```

**C#:**
```csharp
gauge.LabelStyle = new LinearLabelStyle
{
    TextColor = Color.FromArgb("#2196F3"),
    FontSize = 14,
    FontFamily = "Arial",
    FontAttributes = FontAttributes.Bold
};
```

**Example: Styled Labels for Dashboard**

```csharp
SfLinearGauge dashboardGauge = new SfLinearGauge
{
    Minimum = 0,
    Maximum = 100
};

dashboardGauge.LabelStyle = new LinearLabelStyle
{
    TextColor = Color.FromArgb("#424242"),
    FontSize = 16,
    FontFamily = "Roboto",
    FontAttributes = FontAttributes.Bold
};
```

### Label Position

Position labels inside or outside the gauge track using `LabelPosition`.

**Positions:**
- `Inside` - Labels inside the track (default)
- `Outside` - Labels outside the track

**XAML:**
```xml
<gauge:SfLinearGauge LabelPosition="Outside" 
                    TickPosition="Outside">
    <!-- Ticks typically move with labels -->
</gauge:SfLinearGauge>
```

**C#:**
```csharp
gauge.LabelPosition = GaugeLabelsPosition.Outside;
gauge.TickPosition = GaugeElementPosition.Outside;
```

**Use Cases:**
- **Inside:** Compact layouts, minimal space
- **Outside:** Clear separation, easier reading

### Label Offset

Adjust distance between tick ends and labels with `LabelOffset`.

**XAML:**
```xml
<gauge:SfLinearGauge LabelOffset="15"/>
```

**C#:**
```csharp
gauge.LabelOffset = 15;  // Pixels from tick end
```

**Offset Values:**
- **Positive:** Move labels away from scale
- **Negative:** Move labels toward scale (overlap possible)
- **Default:** Auto-calculated spacing

**Example: Custom Label Spacing**

```csharp
// Close labels to track
gauge.LabelOffset = 5;

// Far labels from track
gauge.LabelOffset = 25;

// Labels overlap track (negative)
gauge.LabelOffset = -10;
```

### Label Visibility

Show or hide labels using `ShowLabels`.

**XAML:**
```xml
<gauge:SfLinearGauge ShowLabels="False"/>
```

**C#:**
```csharp
gauge.ShowLabels = false;
```

**When to Hide Labels:**
- Minimalist designs
- Custom label implementations
- Space constraints
- Decorative gauges

**Example: Clean Progress Bar**

```xml
<gauge:SfLinearGauge Minimum="0" 
                    Maximum="100" 
                    ShowLabels="False"
                    ShowTicks="False">
    <gauge:SfLinearGauge.BarPointers>
        <gauge:BarPointer Value="75" Fill="#4CAF50"/>
    </gauge:SfLinearGauge.BarPointers>
</gauge:SfLinearGauge>
```

### Label Format

Format numeric labels using standard .NET format strings with `LabelFormat`.

**Common Formats:**
- `"C"` or `"C2"` - Currency ($100.00)
- `"P"` or `"P0"` - Percentage (75%)
- `"N"` or `"N2"` - Number with commas (1,000.00)
- `"F2"` - Fixed-point (100.50)
- `"0"` - Integer only (100)

**XAML:**
```xml
<!-- Currency format -->
<gauge:SfLinearGauge LabelFormat="C" 
                    Minimum="0" 
                    Maximum="1000" 
                    Interval="250"/>

<!-- Percentage format -->
<gauge:SfLinearGauge LabelFormat="P0" 
                    Minimum="0" 
                    Maximum="1" 
                    Interval="0.25"/>

<!-- Decimal places -->
<gauge:SfLinearGauge LabelFormat="F1" 
                    Minimum="0" 
                    Maximum="10" 
                    Interval="2.5"/>
```

**C#:**
```csharp
// Currency
gauge.LabelFormat = "C";

// Percentage (no decimals)
gauge.LabelFormat = "P0";

// Two decimal places
gauge.LabelFormat = "F2";

// Custom format
gauge.LabelFormat = "#,0.##";
```

**Example: Temperature with Units**

```csharp
SfLinearGauge tempGauge = new SfLinearGauge
{
    Minimum = -20,
    Maximum = 50,
    Interval = 10,
    LabelFormat = "0°C"  // Custom unit suffix
};
```

### Custom Label Text

Replace numeric labels with custom text using the `LabelCreated` event.

**XAML:**
```xml
<gauge:SfLinearGauge LabelCreated="OnLabelCreated"/>
```

**C#:**
```csharp
gauge.LabelCreated += OnLabelCreated;

private void OnLabelCreated(object sender, LabelCreatedEventArgs e)
{
    // Replace specific values
    if (e.Text == "0")
        e.Text = "Start";
    else if (e.Text == "50")
        e.Text = "Middle";
    else if (e.Text == "100")
        e.Text = "End";
}
```

**Example: Performance Levels**

```csharp
private void OnPerformanceLabelCreated(object sender, LabelCreatedEventArgs e)
{
    double value = double.Parse(e.Text);
    
    if (value == 0)
        e.Text = "Poor";
    else if (value == 25)
        e.Text = "Fair";
    else if (value == 50)
        e.Text = "Good";
    else if (value == 75)
        e.Text = "Great";
    else if (value == 100)
        e.Text = "Excellent";
    else
        e.Text = string.Empty;  // Hide intermediate labels
}
```

**Example: Time Labels**

```csharp
private void OnTimeLabelCreated(object sender, LabelCreatedEventArgs e)
{
    int hours = (int)double.Parse(e.Text);
    
    if (hours == 0)
        e.Text = "12 AM";
    else if (hours < 12)
        e.Text = $"{hours} AM";
    else if (hours == 12)
        e.Text = "12 PM";
    else
        e.Text = $"{hours - 12} PM";
}
```

**Example: Conditional Formatting**

```csharp
private void OnConditionalLabelCreated(object sender, LabelCreatedEventArgs e)
{
    double value = double.Parse(e.Text);
    
    // Add prefix for positive values
    if (value > 0)
        e.Text = "+" + e.Text;
    
    // Add emoji indicators
    if (value < 33)
        e.Text += " 🔴";
    else if (value < 66)
        e.Text += " 🟡";
    else
        e.Text += " 🟢";
}
```

### Maximum Labels Count

Control label density with `MaximumLabelsCount` - the maximum number of labels per 100 logical pixels.

**XAML:**
```xml
<!-- Default: 3 labels per 100 pixels -->
<gauge:SfLinearGauge MaximumLabelsCount="3"/>

<!-- Sparse: 1 label per 100 pixels -->
<gauge:SfLinearGauge MaximumLabelsCount="1"/>

<!-- Dense: 5 labels per 100 pixels -->
<gauge:SfLinearGauge MaximumLabelsCount="5"/>
```

**C#:**
```csharp
gauge.MaximumLabelsCount = 1;  // Fewer labels
```

**Guidelines:**
- **Small gauges:** Use 1-2 to avoid crowding
- **Large gauges:** Use 3-5 for better granularity
- **Mobile:** Keep at 2-3 for readability

## Ticks

### Major Ticks

Major ticks align with scale labels and mark primary intervals.

**Styling Major Ticks:**

**XAML:**
```xml
<gauge:SfLinearGauge>
    <gauge:SfLinearGauge.MajorTickStyle>
        <gauge:LinearTickStyle Length="15"
                              Stroke="#424242"
                              StrokeThickness="2"/>
    </gauge:SfLinearGauge.MajorTickStyle>
</gauge:SfLinearGauge>
```

**C#:**
```csharp
gauge.MajorTickStyle = new LinearTickStyle
{
    Length = 15,
    Stroke = new SolidColorBrush(Color.FromArgb("#424242")),
    StrokeThickness = 2
};
```

**Major Tick Properties:**
- `Length` - Tick height/width (pixels)
- `Stroke` - Tick color
- `StrokeThickness` - Line thickness
- `StrokeDashArray` - Dash pattern (for dashed ticks)

**Example: Prominent Major Ticks**

```csharp
gauge.MajorTickStyle = new LinearTickStyle
{
    Length = 20,
    Stroke = new SolidColorBrush(Colors.Black),
    StrokeThickness = 3
};
```

### Minor Ticks

Minor ticks appear between major ticks for finer granularity.

**Styling Minor Ticks:**

**XAML:**
```xml
<gauge:SfLinearGauge MinorTicksPerInterval="4">
    <gauge:SfLinearGauge.MinorTickStyle>
        <gauge:LinearTickStyle Length="8"
                              Stroke="#BDBDBD"
                              StrokeThickness="1"/>
    </gauge:SfLinearGauge.MinorTickStyle>
</gauge:SfLinearGauge>
```

**C#:**
```csharp
gauge.MinorTicksPerInterval = 4;
gauge.MinorTickStyle = new LinearTickStyle
{
    Length = 8,
    Stroke = new SolidColorBrush(Color.FromArgb("#BDBDBD")),
    StrokeThickness = 1
};
```

**Minor Tick Frequency:**

The `MinorTicksPerInterval` property determines how many minor ticks appear between major ticks.

```csharp
// 1 minor tick between major ticks (default)
gauge.MinorTicksPerInterval = 1;

// 4 minor ticks (divides interval into 5 parts)
gauge.MinorTicksPerInterval = 4;

// 9 minor ticks (divides interval into 10 parts)
gauge.MinorTicksPerInterval = 9;
```

**Example: Ruler-Style Ticks**

```xml
<gauge:SfLinearGauge Interval="10" MinorTicksPerInterval="9">
    <gauge:SfLinearGauge.MajorTickStyle>
        <gauge:LinearTickStyle Length="20" StrokeThickness="2"/>
    </gauge:SfLinearGauge.MajorTickStyle>
    <gauge:SfLinearGauge.MinorTickStyle>
        <gauge:LinearTickStyle Length="10" StrokeThickness="1"/>
    </gauge:SfLinearGauge.MinorTickStyle>
</gauge:SfLinearGauge>
```

### Tick Position

Position ticks inside or outside the scale track.

**XAML:**
```xml
<gauge:SfLinearGauge TickPosition="Outside"/>
```

**C#:**
```csharp
gauge.TickPosition = GaugeElementPosition.Outside;
```

**Positions:**
- `Inside` - Ticks inside track (default)
- `Outside` - Ticks outside track
- `Cross` - Ticks cross the track

**Example: Outside Positioning**

```xml
<gauge:SfLinearGauge TickPosition="Outside" 
                    LabelPosition="Outside">
    <!-- Labels typically match tick position -->
</gauge:SfLinearGauge>
```

### Tick Offset

Adjust tick distance from the scale using `TickOffset`.

**XAML:**
```xml
<gauge:SfLinearGauge TickOffset="10"/>
```

**C#:**
```csharp
gauge.TickOffset = 10;
```

**Note:** When you set tick offset, labels move with ticks to maintain alignment.

**Example: Spaced Ticks**

```csharp
// Ticks closer to scale
gauge.TickOffset = 5;

// Ticks farther from scale
gauge.TickOffset = 20;

// Default position
gauge.TickOffset = double.NaN;
```

### Tick Visibility

Show or hide all ticks using `ShowTicks`.

**XAML:**
```xml
<gauge:SfLinearGauge ShowTicks="False"/>
```

**C#:**
```csharp
gauge.ShowTicks = false;
```

**When to Hide Ticks:**
- Clean, minimal designs
- Progress bars without scale details
- Custom tick implementations
- Space-constrained layouts

## Combined Examples

### Example 1: Dashboard Gauge with Styled Elements

```xml
<gauge:SfLinearGauge Minimum="0" 
                    Maximum="100" 
                    Interval="20"
                    LabelPosition="Outside"
                    TickPosition="Outside"
                    LabelOffset="10"
                    TickOffset="5">
    
    <!-- Label styling -->
    <gauge:SfLinearGauge.LabelStyle>
        <gauge:LinearLabelStyle TextColor="#1976D2"
                               FontSize="14"
                               FontAttributes="Bold"/>
    </gauge:SfLinearGauge.LabelStyle>
    
    <!-- Major ticks -->
    <gauge:SfLinearGauge.MajorTickStyle>
        <gauge:LinearTickStyle Length="15"
                              Stroke="#1976D2"
                              StrokeThickness="2"/>
    </gauge:SfLinearGauge.MajorTickStyle>
    
    <!-- Minor ticks -->
    <gauge:SfLinearGauge.MinorTickStyle>
        <gauge:LinearTickStyle Length="8"
                              Stroke="#64B5F6"
                              StrokeThickness="1"/>
    </gauge:SfLinearGauge.MinorTickStyle>
    
</gauge:SfLinearGauge>
```

### Example 2: Thermometer with Custom Labels

```csharp
SfLinearGauge thermometer = new SfLinearGauge
{
    Minimum = -20,
    Maximum = 50,
    Interval = 10,
    Orientation = GaugeOrientation.Vertical,
    MinorTicksPerInterval = 4,
    LabelFormat = "0°C"
};

// Style labels
thermometer.LabelStyle = new LinearLabelStyle
{
    TextColor = Colors.Black,
    FontSize = 12
};

// Style major ticks
thermometer.MajorTickStyle = new LinearTickStyle
{
    Length = 12,
    StrokeThickness = 2,
    Stroke = new SolidColorBrush(Colors.Black)
};

// Style minor ticks
thermometer.MinorTickStyle = new LinearTickStyle
{
    Length = 6,
    StrokeThickness = 1,
    Stroke = new SolidColorBrush(Colors.Gray)
};
```

### Example 3: Dashed Ticks Pattern

```csharp
SfLinearGauge dashedGauge = new SfLinearGauge();

// Dashed major ticks
var majorDashArray = new DoubleCollection { 4, 2 };
dashedGauge.MajorTickStyle = new LinearTickStyle
{
    Length = 20,
    StrokeThickness = 2,
    Stroke = new SolidColorBrush(Colors.Red),
    StrokeDashArray = majorDashArray
};

// Dotted minor ticks
var minorDashArray = new DoubleCollection { 1, 2 };
dashedGauge.MinorTickStyle = new LinearTickStyle
{
    Length = 10,
    StrokeThickness = 1,
    Stroke = new SolidColorBrush(Colors.Blue),
    StrokeDashArray = minorDashArray
};
```

### Example 4: Performance Indicator with Custom Labels

```csharp
SfLinearGauge performanceGauge = new SfLinearGauge
{
    Minimum = 0,
    Maximum = 100,
    Interval = 25
};

performanceGauge.LabelCreated += (s, e) =>
{
    double value = double.Parse(e.Text);
    if (value == 0) e.Text = "Poor";
    else if (value == 25) e.Text = "Fair";
    else if (value == 50) e.Text = "Good";
    else if (value == 75) e.Text = "Great";
    else if (value == 100) e.Text = "Excellent";
};

performanceGauge.LabelStyle = new LinearLabelStyle
{
    TextColor = Color.FromArgb("#424242"),
    FontSize = 14,
    FontAttributes = FontAttributes.Bold
};
```

### Example 5: Minimalist Progress (No Decorations)

```xml
<gauge:SfLinearGauge Minimum="0" 
                    Maximum="100"
                    ShowLabels="False"
                    ShowTicks="False">
    
    <gauge:SfLinearGauge.LineStyle>
        <gauge:LinearLineStyle Thickness="8" 
                              Fill="#E0E0E0"
                              CornerStyle="BothCurve"/>
    </gauge:SfLinearGauge.LineStyle>
    
    <gauge:SfLinearGauge.BarPointers>
        <gauge:BarPointer Value="65" 
                         Fill="#2196F3"
                         CornerStyle="BothCurve"/>
    </gauge:SfLinearGauge.BarPointers>
    
</gauge:SfLinearGauge>
```

### Example 6: Dense Scale with Many Minor Ticks

```csharp
SfLinearGauge denseGauge = new SfLinearGauge
{
    Minimum = 0,
    Maximum = 10,
    Interval = 1,
    MinorTicksPerInterval = 9,  // 10 divisions between major ticks
    MaximumLabelsCount = 5
};

denseGauge.MajorTickStyle = new LinearTickStyle
{
    Length = 15,
    StrokeThickness = 2
};

denseGauge.MinorTickStyle = new LinearTickStyle
{
    Length = 7,
    StrokeThickness = 1,
    Stroke = new SolidColorBrush(Colors.LightGray)
};
```
