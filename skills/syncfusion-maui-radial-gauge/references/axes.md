# Axis Configuration

## Table of Contents
- [Overview](#overview)
- [Basic Axis Setup](#basic-axis-setup)
- [Scale Configuration](#scale-configuration)
  - [Minimum and Maximum](#minimum-and-maximum)
  - [Interval](#interval)
- [Axis Angles](#axis-angles)
  - [Creating Semi-Circular Gauges](#creating-semi-circular-gauges)
  - [Creating Arc Gauges](#creating-arc-gauges)
  - [Angle Reference](#angle-reference)
- [Axis Sizing](#axis-sizing)
  - [Radius Factor](#radius-factor)
  - [Scale to Fit](#scale-to-fit)
- [Axis Line Styling](#axis-line-styling)
  - [Thickness (Pixel vs Factor)](#thickness-pixel-vs-factor)
  - [Colors and Gradients](#colors-and-gradients)
  - [Hiding the Axis Line](#hiding-the-axis-line)
- [Labels](#labels)
  - [Label Position and Offset](#label-position-and-offset)
  - [Label Rotation](#label-rotation)
  - [Edge Labels (First/Last)](#edge-labels-firstlast)
  - [Label Styling](#label-styling)
  - [Custom Label Text](#custom-label-text)
  - [Maximum Labels Count](#maximum-labels-count)
- [Ticks](#ticks)
  - [Major and Minor Ticks](#major-and-minor-ticks)
  - [Tick Position and Length](#tick-position-and-length)
  - [Tick Styling](#tick-styling)
  - [Hiding Ticks](#hiding-ticks)
- [Axis Direction](#axis-direction)
- [Background Content](#background-content)
- [Multiple Axes](#multiple-axes)
- [Common Configurations](#common-configurations)

## Overview

The `RadialAxis` is the circular scale where values, pointers, ranges, and annotations are displayed. Every radial gauge requires at least one axis. The axis provides extensive customization options for scale range, angles, labels, ticks, and visual styling.

**Key Capabilities:**
- Define scale range (Minimum/Maximum)
- Control axis arc (full circle, semi-circle, or custom angle)
- Customize labels (position, rotation, formatting)
- Style ticks (major/minor, length, appearance)
- Apply colors and gradients to axis line
- Add background images or custom content
- Create multiple axes on one gauge

## Basic Axis Setup

Every `SfRadialGauge` contains an `Axes` collection. Add at least one `RadialAxis`:

```xaml
<gauge:SfRadialGauge>
    <gauge:SfRadialGauge.Axes>
        <gauge:RadialAxis Minimum="0"
                          Maximum="100"
                          Interval="10" />
    </gauge:SfRadialGauge.Axes>
</gauge:SfRadialGauge>
```

```csharp
SfRadialGauge gauge = new SfRadialGauge();

RadialAxis axis = new RadialAxis
{
    Minimum = 0,
    Maximum = 100,
    Interval = 10
};

gauge.Axes.Add(axis);
```

## Scale Configuration

### Minimum and Maximum

Control the value range using `Minimum` and `Maximum` properties.

**Example: Temperature Gauge (-20°C to 50°C)**
```xaml
<gauge:RadialAxis Minimum="-20"
                  Maximum="50"
                  Interval="10" />
```

```csharp
RadialAxis axis = new RadialAxis
{
    Minimum = -20,
    Maximum = 50,
    Interval = 10
};
```

**Example: Speedometer (0-200 km/h)**
```xaml
<gauge:RadialAxis Minimum="0"
                  Maximum="200"
                  Interval="20" />
```

**Default Values:**
- Minimum: 0
- Maximum: 100

### Interval

The `Interval` property defines the spacing between labels and major ticks.

```xaml
<!-- Labels at 0, 20, 40, 60, 80, 100 -->
<gauge:RadialAxis Minimum="0"
                  Maximum="100"
                  Interval="20" />
```

```csharp
axis.Interval = 20;
```

**Auto Interval:** If not specified, the interval is automatically calculated based on the scale range and available space.

**Best Practices:**
- Use intervals that result in 5-15 labels for readability
- Choose round numbers (5, 10, 20, 25, 50) for cleaner appearance
- For small gauges, use larger intervals to avoid label crowding

## Axis Angles

Control the arc of the gauge using `StartAngle` and `EndAngle` properties.

**Angle System:**
- 0° = Right (3 o'clock)
- 90° = Bottom (6 o'clock)
- 180° = Left (9 o'clock)
- 270° = Top (12 o'clock)
- Angles increase clockwise

### Full Circle (360°)

**Default:** StartAngle=0, EndAngle=360

```xaml
<gauge:RadialAxis StartAngle="0"
                  EndAngle="360" />
```

### Creating Semi-Circular Gauges

**Bottom Half (Speedometer Style):**
```xaml
<gauge:RadialAxis StartAngle="180"
                  EndAngle="0"
                  Minimum="0"
                  Maximum="200" />
```

**Top Half:**
```xaml
<gauge:RadialAxis StartAngle="0"
                  EndAngle="180" />
```

**Left Half:**
```xaml
<gauge:RadialAxis StartAngle="90"
                  EndAngle="270" />
```

**Right Half:**
```xaml
<gauge:RadialAxis StartAngle="270"
                  EndAngle="90" />
```

### Creating Arc Gauges

**Three-Quarter Circle:**
```xaml
<gauge:RadialAxis StartAngle="180"
                  EndAngle="90" />
```

**Quarter Circle:**
```xaml
<gauge:RadialAxis StartAngle="270"
                  EndAngle="0" />
```

### Angle Reference

```
        270° (Top)
           |
    180° ——+—— 0° (Right)
           |
        90° (Bottom)
```

**Example: Clock-Style Gauge (Starting at Top)**
```xaml
<gauge:RadialAxis StartAngle="270"
                  EndAngle="270"
                  Minimum="0"
                  Maximum="12"
                  Interval="1" />
```

## Axis Sizing

### Radius Factor

The `RadiusFactor` controls the size of the axis relative to available space.

**Range:** 0 to 1
- **0:** Zero radius (invisible)
- **0.5:** Half of available radius
- **0.8:** Default (80% of available space)
- **1:** Full radius (touches edges)

```xaml
<gauge:RadialAxis RadiusFactor="0.9" />
```

```csharp
axis.RadiusFactor = 0.9;
```

**Use Cases:**
- **0.6-0.7:** Leave more space for external labels/annotations
- **0.8-0.9:** Standard gauges
- **0.95-1.0:** Maximum size for dashboard displays

### Scale to Fit

The `CanScaleToFit` property automatically positions the axis to fit within available space based on start/end angles.

**Default:** true

```xaml
<gauge:RadialAxis CanScaleToFit="False"
                  StartAngle="180"
                  EndAngle="0" />
```

**When to Disable:**
- You want consistent positioning regardless of angles
- Creating multi-axis gauges with specific layouts
- Overlaying gauges with custom positioning

## Axis Line Styling

The axis line is the circular arc on which ticks and labels are placed.

### Thickness (Pixel vs Factor)

**Pixel Mode (Fixed Size):**
```xaml
<gauge:RadialAxis>
    <gauge:RadialAxis.AxisLineStyle>
        <gauge:RadialLineStyle Thickness="30"
                               ThicknessUnit="Pixel"
                               Fill="LightBlue" />
    </gauge:RadialAxis.AxisLineStyle>
</gauge:RadialAxis>
```

```csharp
axis.AxisLineStyle = new RadialLineStyle
{
    Thickness = 30,
    ThicknessUnit = SizeUnit.Pixel,
    Fill = new SolidColorBrush(Colors.LightBlue)
};
```

**Factor Mode (Relative to Radius):**
```xaml
<gauge:RadialAxis>
    <gauge:RadialAxis.AxisLineStyle>
        <gauge:RadialLineStyle Thickness="0.1"
                               ThicknessUnit="Factor"
                               Fill="CornflowerBlue" />
    </gauge:RadialAxis.AxisLineStyle>
</gauge:RadialAxis>
```

**Factor Range:** 0 to 1 (0.1 = 10% of radius)

**Best Practices:**
- **Pixel:** For consistent sizing across devices (e.g., always 20px thick)
- **Factor:** For responsive sizing (e.g., always 10% of gauge size)

### Colors and Gradients

**Solid Color:**
```xaml
<gauge:RadialLineStyle Fill="Orange"
                       Thickness="20" />
```

```csharp
axis.AxisLineStyle.Fill = new SolidColorBrush(Colors.Orange);
axis.AxisLineStyle.Thickness = 20;
```

**Gradient:**
```xaml
<gauge:RadialAxis>
    <gauge:RadialAxis.AxisLineStyle>
        <gauge:RadialLineStyle Thickness="25">
            <gauge:RadialLineStyle.GradientStops>
                <gauge:GaugeGradientStop Value="0" Color="Green" />
                <gauge:GaugeGradientStop Value="50" Color="Yellow" />
                <gauge:GaugeGradientStop Value="100" Color="Red" />
            </gauge:RadialLineStyle.GradientStops>
        </gauge:RadialLineStyle>
    </gauge:RadialAxis.AxisLineStyle>
</gauge:RadialAxis>
```

```csharp
axis.AxisLineStyle = new RadialLineStyle
{
    Thickness = 25,
    GradientStops = new ObservableCollection<GaugeGradientStop>
    {
        new GaugeGradientStop { Value = 0, Color = Colors.Green },
        new GaugeGradientStop { Value = 50, Color = Colors.Yellow },
        new GaugeGradientStop { Value = 100, Color = Colors.Red }
    }
};
```

**Gradient Use Case:** Gradients are excellent for showing value zones without separate range elements.

### Hiding the Axis Line

```xaml
<gauge:RadialAxis ShowAxisLine="False" />
```

```csharp
axis.ShowAxisLine = false;
```

**Use Cases:**
- Clean progress indicators (only show range pointer)
- Minimal designs
- When using ranges as the primary visual element

## Labels

### Label Position and Offset

The `LabelOffset` property controls distance from the axis line to labels.

**Positive Offset (Outside):**
```xaml
<gauge:RadialAxis LabelOffset="30"
                  OffsetUnit="Pixel" />
```

**Negative Offset (Inside):**
```xaml
<gauge:RadialAxis LabelOffset="-30"
                  OffsetUnit="Pixel" />
```

**Factor-Based Offset:**
```xaml
<gauge:RadialAxis LabelOffset="0.15"
                  OffsetUnit="Factor" />
```

```csharp
axis.LabelOffset = 0.15;
axis.OffsetUnit = SizeUnit.Factor;
```

**Default:** Labels are positioned outside the axis line.

### Label Rotation

**Enable Radial Label Rotation:**
```xaml
<gauge:RadialAxis CanRotateLabels="True" />
```

```csharp
axis.CanRotateLabels = true;
```

**Effect:** Labels rotate to align with their position angle on the circle (readable from any direction).

**When to Use:**
- Circular gauges where users view from multiple angles
- Compass-style displays
- Artistic or decorative designs

**When to Avoid:**
- Standard dashboard gauges (upright labels are more readable)
- Semi-circular gauges (rotation not helpful)

### Edge Labels (First/Last)

Control visibility of the first and last labels:

```xaml
<gauge:RadialAxis ShowFirstLabel="False"
                  ShowLastLabel="True"
                  StartAngle="270"
                  EndAngle="270"
                  Minimum="0"
                  Maximum="12" />
```

```csharp
axis.ShowFirstLabel = false;  // Hide "0"
axis.ShowLastLabel = true;    // Show "12"
```

**Use Cases:**
- **Clock faces:** Hide "0" when 12 is at the same position
- **Overlapping labels:** Hide one when start/end angles meet
- **Cleaner appearance:** Remove edge labels when they crowd the design

### Label Styling

```xaml
<gauge:RadialAxis>
    <gauge:RadialAxis.AxisLabelStyle>
        <gauge:GaugeLabelStyle TextColor="DarkBlue"
                               FontSize="14"
                               FontAttributes="Bold"
                               FontFamily="Arial" />
    </gauge:RadialAxis.AxisLabelStyle>
</gauge:RadialAxis>
```

```csharp
axis.AxisLabelStyle = new GaugeLabelStyle
{
    TextColor = Colors.DarkBlue,
    FontSize = 14,
    FontAttributes = FontAttributes.Bold,
    FontFamily = "Arial"
};
```

### Custom Label Text

Use the `LabelCreated` event to customize label text:

```xaml
<gauge:RadialAxis LabelCreated="OnLabelCreated"
                  Minimum="0"
                  Maximum="360"
                  Interval="90" />
```

```csharp
private void OnLabelCreated(object sender, LabelCreatedEventArgs e)
{
    // Convert degrees to compass directions
    if (e.Text == "0" || e.Text == "360")
        e.Text = "N";
    else if (e.Text == "90")
        e.Text = "E";
    else if (e.Text == "180")
        e.Text = "S";
    else if (e.Text == "270")
        e.Text = "W";
}
```

**Common Use Cases:**
- Compass directions (N, E, S, W)
- Custom units (°C, km/h, PSI)
- Formatted numbers (1k, 2k instead of 1000, 2000)
- Text labels for categorical data

### Maximum Labels Count

Control automatic label density:

```xaml
<gauge:RadialAxis MaximumLabelsCount="5" />
```

```csharp
axis.MaximumLabelsCount = 5;
```

**Default:** 3 labels per 100 logical pixels

**Note:** Only applies when using auto-interval. If you specify `Interval`, this property is ignored.

## Ticks

### Major and Minor Ticks

**Major ticks** appear at each interval (where labels are).  
**Minor ticks** appear between major ticks.

```xaml
<gauge:RadialAxis Interval="10"
                  MinorTicksPerInterval="4" />
```

The above creates:
- Major ticks at 0, 10, 20, 30, ...
- Minor ticks at 2.5, 5, 7.5, 12.5, 15, 17.5, ...

```csharp
axis.Interval = 10;
axis.MinorTicksPerInterval = 4;  // 4 minor ticks between each major tick
```

**Default:** `MinorTicksPerInterval = 1`

### Tick Position and Length

**Major Tick Styling:**
```xaml
<gauge:RadialAxis>
    <gauge:RadialAxis.MajorTickStyle>
        <gauge:RadialTickStyle Length="15"
                               LengthUnit="Pixel"
                               Stroke="Black"
                               StrokeThickness="2" />
    </gauge:RadialAxis.MajorTickStyle>
</gauge:RadialAxis>
```

**Minor Tick Styling:**
```xaml
<gauge:RadialAxis>
    <gauge:RadialAxis.MinorTickStyle>
        <gauge:RadialTickStyle Length="8"
                               LengthUnit="Pixel"
                               Stroke="Gray"
                               StrokeThickness="1" />
    </gauge:RadialAxis.MinorTickStyle>
</gauge:RadialAxis>
```

**Factor-Based Length:**
```xaml
<gauge:RadialTickStyle Length="0.05"
                       LengthUnit="Factor"
                       Stroke="Black"
                       StrokeThickness="2" />
```

**Tick Offset:**
```xaml
<gauge:RadialAxis TickOffset="30"
                  OffsetUnit="Pixel">
    <!-- Ticks positioned 30 pixels from axis line -->
</gauge:RadialAxis>
```

### Tick Styling

```csharp
axis.MajorTickStyle = new RadialTickStyle
{
    Length = 15,
    LengthUnit = SizeUnit.Pixel,
    Stroke = new SolidColorBrush(Colors.Black),
    StrokeThickness = 2
};

axis.MinorTickStyle = new RadialTickStyle
{
    Length = 8,
    LengthUnit = SizeUnit.Pixel,
    Stroke = new SolidColorBrush(Colors.Gray),
    StrokeThickness = 1
};
```

### Hiding Ticks

```xaml
<gauge:RadialAxis ShowTicks="False" />
```

```csharp
axis.ShowTicks = false;
```

**Use Cases:**
- Minimal gauge designs
- Progress indicators
- When ticks add visual clutter

**Hiding Only Labels (Keep Ticks):**
```xaml
<gauge:RadialAxis ShowLabels="False" />
```

## Axis Direction

**Clockwise (Default):**
```xaml
<gauge:RadialAxis IsInversed="False" />
```

**Counter-Clockwise:**
```xaml
<gauge:RadialAxis IsInversed="True" />
```

```csharp
axis.IsInversed = true;
```

**Effect:** Values increase counter-clockwise instead of clockwise.

**Use Cases:**
- Certain regional gauge conventions
- Artistic designs
- Countdown timers (decreasing values)

## Background Content

Add any MAUI view as the axis background:

**Image Background:**
```xaml
<gauge:RadialAxis>
    <gauge:RadialAxis.BackgroundContent>
        <Image Source="gauge_background.png"
               Aspect="AspectFit" />
    </gauge:RadialAxis.BackgroundContent>
</gauge:RadialAxis>
```

**Custom Layout:**
```xaml
<gauge:RadialAxis>
    <gauge:RadialAxis.BackgroundContent>
        <Grid>
            <BoxView Color="LightGray"
                     Opacity="0.3"
                     CornerRadius="200" />
            <Label Text="SPEED"
                   VerticalOptions="Center"
                   HorizontalOptions="Center"
                   FontSize="12"
                   TextColor="Gray" />
        </Grid>
    </gauge:RadialAxis.BackgroundContent>
</gauge:RadialAxis>
```

```csharp
axis.BackgroundContent = new Image { Source = "gauge_background.png", Aspect = Aspect.AspectFit };
```

**Use Cases:**
- Branded gauge designs
- Textured backgrounds
- Decorative elements
- Company logos or icons

## Multiple Axes

Add multiple axes to create complex gauges:

```xaml
<gauge:SfRadialGauge>
    <gauge:SfRadialGauge.Axes>
        <!-- Outer axis -->
        <gauge:RadialAxis Minimum="0"
                          Maximum="100"
                          RadiusFactor="0.9" />
        
        <!-- Inner axis -->
        <gauge:RadialAxis Minimum="0"
                          Maximum="50"
                          RadiusFactor="0.6">
            <gauge:RadialAxis.AxisLineStyle>
                <gauge:RadialLineStyle Fill="LightBlue" />
            </gauge:RadialAxis.AxisLineStyle>
        </gauge:RadialAxis>
    </gauge:SfRadialGauge.Axes>
</gauge:SfRadialGauge>
```

```csharp
// Outer axis
RadialAxis outerAxis = new RadialAxis
{
    Minimum = 0,
    Maximum = 100,
    RadiusFactor = 0.9
};

// Inner axis
RadialAxis innerAxis = new RadialAxis
{
    Minimum = 0,
    Maximum = 50,
    RadiusFactor = 0.6,
    AxisLineStyle = new RadialLineStyle { Fill = new SolidColorBrush(Colors.LightBlue) }
};

gauge.Axes.Add(outerAxis);
gauge.Axes.Add(innerAxis);
```

**Use Cases:**
- Dual-scale gauges (km/h and mph)
- Related metrics (RPM outer, fuel level inner)
- Complex dashboard displays

## Common Configurations

### Configuration 1: Standard Speedometer
```xaml
<gauge:RadialAxis Minimum="0"
                  Maximum="200"
                  Interval="20"
                  StartAngle="180"
                  EndAngle="0">
    <gauge:RadialAxis.AxisLineStyle>
        <gauge:RadialLineStyle Thickness="3" Fill="Black" />
    </gauge:RadialAxis.AxisLineStyle>
</gauge:RadialAxis>
```

### Configuration 2: Clean Progress Gauge
```xaml
<gauge:RadialAxis Minimum="0"
                  Maximum="100"
                  ShowAxisLine="False"
                  ShowTicks="False"
                  ShowLabels="False" />
```

### Configuration 3: Compass (360°)
```xaml
<gauge:RadialAxis Minimum="0"
                  Maximum="360"
                  Interval="90"
                  StartAngle="270"
                  EndAngle="270"
                  LabelCreated="OnCompassLabelCreated"
                  CanRotateLabels="False">
    <gauge:RadialAxis.AxisLineStyle>
        <gauge:RadialLineStyle Thickness="2" Fill="Navy" />
    </gauge:RadialAxis.AxisLineStyle>
</gauge:RadialAxis>
```

### Configuration 4: Clock Face
```xaml
<gauge:RadialAxis Minimum="0"
                  Maximum="12"
                  Interval="1"
                  StartAngle="270"
                  EndAngle="270"
                  ShowFirstLabel="False"
                  MinorTicksPerInterval="4">
    <gauge:RadialAxis.MajorTickStyle>
        <gauge:RadialTickStyle Length="12" StrokeThickness="2" />
    </gauge:RadialAxis.MajorTickStyle>
    <gauge:RadialAxis.MinorTickStyle>
        <gauge:RadialTickStyle Length="6" StrokeThickness="1" />
    </gauge:RadialAxis.MinorTickStyle>
</gauge:RadialAxis>
```

### Configuration 5: Temperature Gauge with Gradient
```xaml
<gauge:RadialAxis Minimum="-20"
                  Maximum="50"
                  Interval="10"
                  StartAngle="180"
                  EndAngle="0">
    <gauge:RadialAxis.AxisLineStyle>
        <gauge:RadialLineStyle Thickness="20">
            <gauge:RadialLineStyle.GradientStops>
                <gauge:GaugeGradientStop Value="-20" Color="Blue" />
                <gauge:GaugeGradientStop Value="10" Color="Green" />
                <gauge:GaugeGradientStop Value="30" Color="Orange" />
                <gauge:GaugeGradientStop Value="50" Color="Red" />
            </gauge:RadialLineStyle.GradientStops>
        </gauge:RadialLineStyle>
    </gauge:RadialAxis.AxisLineStyle>
</gauge:RadialAxis>
```

## Summary

The RadialAxis provides complete control over the gauge scale:
- **Scale:** Minimum, Maximum, Interval
- **Shape:** StartAngle, EndAngle, RadiusFactor
- **Axis Line:** Thickness, colors, gradients
- **Labels:** Position, rotation, custom text, styling
- **Ticks:** Major/minor, length, styling, positioning
- **Advanced:** Multiple axes, background content, direction

For most gauges, start with basic scale configuration (min/max/interval), then customize appearance as needed.
