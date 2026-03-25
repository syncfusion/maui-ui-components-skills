# Gauges Migration: Xamarin.Forms to .NET MAUI

Migration guide for Syncfusion gauge controls from Xamarin.Forms to .NET MAUI.

## Table of Contents
- [Overview](#overview)
- [SfRadialGauge Migration](#sfradialgauge-migration)
- [SfLinearGauge Migration](#sflineargauge-migration)
- [SfDigitalGauge Migration](#sfdigitalgauge-migration)
- [Common Migration Patterns](#common-migration-patterns)

## Overview

Syncfusion provides three gauge controls for .NET MAUI with improved performance and updated APIs:
- **SfRadialGauge** (formerly SfCircularGauge) - Circular/arc gauges
- **SfLinearGauge** - Horizontal/vertical linear gauges  
- **SfDigitalGauge** - Digital/segment display gauges

## SfRadialGauge Migration

### Component Rename

| Xamarin | MAUI | Reason |
|---------|------|--------|
| SfCircularGauge | SfRadialGauge | Better describes radial nature |

### Namespace Changes

```csharp
// Xamarin.Forms
using Syncfusion.SfGauge.XForms;

// .NET MAUI
using Syncfusion.Maui.Gauges;
```

### Initialization

**Xamarin:**
```xml
<gauge:SfCircularGauge>
    <gauge:SfCircularGauge.Scales>
        <gauge:Scale StartValue="0" EndValue="100"/>
    </gauge:SfCircularGauge.Scales>
</gauge:SfCircularGauge>
```

**.NET MAUI:**
```xml
<gauge:SfRadialGauge>
    <gauge:SfRadialGauge.Axes>
        <gauge:RadialAxis Minimum="0" Maximum="100"/>
    </gauge:SfRadialGauge.Axes>
</gauge:SfRadialGauge>
```

### Key Property Changes

| Xamarin | MAUI | Description |
|---------|------|-------------|
| `Scales` | `Axes` | Collection property rename |
| `Scale` | `RadialAxis` | Class rename |
| `StartValue` | `Minimum` | Minimum value |
| `EndValue` | `Maximum` | Maximum value |
| `ShowRim` | `ShowAxisLine` | Axis line visibility |
| `SweepAngle` | `EndAngle` | Angle calculation difference |
| `EnableAutoAngle` | `CanRotateLabels` | Label rotation |
| `MaximumLabels` | `MaximumLabelsCount` | Label count limit |
| `Direction` | `IsInversed` | Invert direction |
| `MajorTickSettings` | `MajorTickStyle` | Tick customization |
| `MinorTickSettings` | `MinorTickStyle` | Tick customization |
| `UseRangeColorForLabels` | `UseRangeColorForAxis` | Range color application |

### Axis Migration Example

**Xamarin:**
```csharp
SfCircularGauge circularGauge = new SfCircularGauge();

Scale scale = new Scale();
scale.StartValue = 0;
scale.EndValue = 12;
scale.Interval = 1;
scale.StartAngle = 270;
scale.SweepAngle = 360;
scale.ShowFirstLabel = false;
circularGauge.Scales.Add(scale);
```

**.NET MAUI:**
```csharp
SfRadialGauge radialGauge = new SfRadialGauge();

RadialAxis radialAxis = new RadialAxis();
radialAxis.Minimum = 0;
radialAxis.Maximum = 12;
radialAxis.Interval = 1;
radialAxis.StartAngle = 270;
radialAxis.EndAngle = 270;  // Note: EndAngle = StartAngle + 360
radialAxis.ShowFirstLabel = false;
radialGauge.Axes.Add(radialAxis);
```

### Range Migration

| Xamarin | MAUI | Description |
|---------|------|-------------|
| `Range` | `RadialRange` | Class rename |
| `Offset` | `RangeOffset` | Position offset |
| `Color` | `Fill` | Range color (Brush) |

**Xamarin:**
```xml
<gauge:Scale.Ranges>
    <gauge:Range StartValue="0" 
                 EndValue="40" 
                 Color="Red"
                 Offset="0.5"/>
</gauge:Scale.Ranges>
```

**.NET MAUI:**
```xml
<gauge:RadialAxis.Ranges>
    <gauge:RadialRange StartValue="0"
                       EndValue="40"
                       Fill="Red"
                       RangeOffset="0.5"/>
</gauge:RadialAxis.Ranges>
```

### Pointer Migration

| Xamarin | MAUI | Description |
|---------|------|-------------|
| `NeedlePointer` | `NeedlePointer` | Same |
| `RangePointer` | `RangePointer` | Same |
| `MarkerPointer` | `MarkerPointer` | Same |
| `Color` | `Fill` | Pointer color |

**Xamarin:**
```xml
<gauge:NeedlePointer Value="60" Color="Blue" Thickness="5"/>
```

**.NET MAUI:**
```xml
<gauge:NeedlePointer Value="60" Fill="Blue" NeedleThickness="5"/>
```

### Annotation Migration

**Xamarin:**
```xml
<gauge:GaugeAnnotation>
    <gauge:GaugeAnnotation.View>
        <Label Text="60°" TextColor="Black"/>
    </gauge:GaugeAnnotation.View>
</gauge:GaugeAnnotation>
```

**.NET MAUI:**
```xml
<gauge:GaugeAnnotation DirectionUnit="Angle" 
                       DirectionValue="90"
                       PositionFactor="0.5">
    <gauge:GaugeAnnotation.Content>
        <Label Text="60°" TextColor="Black"/>
    </gauge:GaugeAnnotation.Content>
</gauge:GaugeAnnotation>
```

## SfLinearGauge Migration

### Namespace Changes

```csharp
// Same as Radial Gauge
using Syncfusion.Maui.Gauges;
```

### Structure Simplification

In MAUI, there's no separate `Scale` collection. Properties are set directly on `SfLinearGauge`.

**Xamarin:**
```xml
<gauge:SfLinearGauge>
    <gauge:SfLinearGauge.Scales>
        <gauge:LinearScale MinimumValue="0" 
                          MaximumValue="100" 
                          ScaleBarSize="20"/>
    </gauge:SfLinearGauge.Scales>
</gauge:SfLinearGauge>
```

**.NET MAUI:**
```xml
<gauge:SfLinearGauge Minimum="0" 
                     Maximum="100">
    <gauge:SfLinearGauge.LineStyle>
        <gauge:LinearLineStyle Thickness="20"/>
    </gauge:SfLinearGauge.LineStyle>
</gauge:SfLinearGauge>
```

### Key Property Changes

| Xamarin LinearScale | MAUI SfLinearGauge | Description |
|--------------------|-------------------|-------------|
| `MinimumValue` | `Minimum` | Minimum value |
| `MaximumValue` | `Maximum` | Maximum value |
| `ScalePosition` | `IsInversed` | Invert scale |
| `OpposedPosition` | `IsMirrored` | Mirror position |
| `ScaleBarSize` | `LineStyle.Thickness` | Line thickness |
| `ScaleBarColor` | `LineStyle.Fill` | Line color |
| `CornerRadiusType` | `LineStyle.CornerStyle` | Corner style |
| `MajorTickSettings` | `MajorTickStyle` | Tick style |
| `MinorTickSettings` | `MinorTickStyle` | Tick style |
| `MaximumLabels` | `MaximumLabelsCount` | Label limit |

### Complete Migration Example

**Xamarin:**
```csharp
SfLinearGauge linearGauge = new SfLinearGauge();

LinearScale linearScale = new LinearScale();
linearScale.MinimumValue = 0;
linearScale.MaximumValue = 100;
linearScale.Interval = 10;
linearScale.ScaleBarSize = 20;
linearScale.ScaleBarColor = Color.Blue;
linearScale.OpposedPosition = true;
linearScale.ScalePosition = ScalePosition.BackWard;

linearGauge.Scales.Add(linearScale);
```

**.NET MAUI:**
```csharp
SfLinearGauge gauge = new SfLinearGauge();
gauge.Minimum = 0;
gauge.Maximum = 100;
gauge.Interval = 10;
gauge.IsInversed = true;
gauge.IsMirrored = true;

gauge.LineStyle = new LinearLineStyle
{
    Thickness = 20,
    Fill = new SolidColorBrush(Colors.Blue)
};
```

### Range Migration

| Xamarin | MAUI | Description |
|---------|------|-------------|
| `LinearRange` | `LinearRange` | Same class name |
| `Color` | `Fill` | Range color |
| `Offset` | `Position` | Range position |

**Xamarin:**
```xml
<gauge:LinearScale.Ranges>
    <gauge:LinearRange StartValue="0" 
                       EndValue="33" 
                       Color="Red"
                       Offset="-40"/>
</gauge:LinearScale.Ranges>
```

**.NET MAUI:**
```xml
<gauge:SfLinearGauge.Ranges>
    <gauge:LinearRange StartValue="0" 
                       EndValue="33" 
                       Fill="Red"
                       Position="Cross"/>
</gauge:SfLinearGauge.Ranges>
```

### Pointer Migration

**Xamarin:**
```xml
<gauge:LinearScale.Pointers>
    <gauge:BarPointer Value="60" 
                      Color="Orange" 
                      Thickness="10"/>
</gauge:LinearScale.Pointers>
```

**.NET MAUI:**
```xml
<gauge:SfLinearGauge.MarkerPointers>
    <gauge:LinearShapePointer Value="60" 
                              Fill="Orange"
                              ShapeHeight="20"
                              ShapeWidth="20"/>
</gauge:SfLinearGauge.MarkerPointers>
```

## SfDigitalGauge Migration

### Namespace Changes

```csharp
// Same namespace
using Syncfusion.Maui.Gauges;
```

### Key Property Changes

| Xamarin | MAUI | Description |
|---------|------|-------------|
| `DisabledSegmentColor` | `DisabledSegmentStroke` | Disabled segment color |
| `SegmentStrokeWidth` | `StrokeWidth` | Segment border width |
| `CharacterStrokeColor` | `CharacterStroke` | Character stroke color |

### Migration Example

**Xamarin:**
```xml
<gauge:SfDigitalGauge Value="12:34"
                      CharacterType="SegmentSeven"
                      DisabledSegmentColor="Gray"
                      SegmentStrokeWidth="2"/>
```

**.NET MAUI:**
```xml
<gauge:SfDigitalGauge Text="12:34"
                      CharacterType="SegmentSeven"
                      DisabledSegmentStroke="Gray"
                      StrokeWidth="2"/>
```

## Common Migration Patterns

### Handler Registration

Add to `MauiProgram.cs`:

```csharp
using Syncfusion.Maui.Core.Hosting;

builder.ConfigureSyncfusionCore();
```

### Color to Brush Migration

Many `Color` properties became `Brush` properties:

**Xamarin:**
```csharp
range.Color = Color.Red;
```

**.NET MAUI:**
```csharp
range.Fill = new SolidColorBrush(Colors.Red);
// Or simply
range.Fill = Colors.Red;  // Implicit conversion
```

### Angle Calculation Difference

**SweepAngle vs EndAngle:**

```csharp
// Xamarin: StartAngle + SweepAngle
scale.StartAngle = 90;
scale.SweepAngle = 270;  // Goes 270° from start

// MAUI: Explicit end angle
axis.StartAngle = 90;
axis.EndAngle = 360;     // 90 + 270 = 360
```

## Troubleshooting

### Issue: SfCircularGauge not found

**Solution:** Renamed to `SfRadialGauge`:
```csharp
// Change this:
SfCircularGauge gauge = new SfCircularGauge();

// To this:
SfRadialGauge gauge = new SfRadialGauge();
```

### Issue: Scales property not found

**Solution:** 
- For RadialGauge: Use `Axes` instead of `Scales`
- For LinearGauge: No scale collection, set properties directly on the gauge

### Issue: StartValue/EndValue not found

**Solution:** Use `Minimum` and `Maximum`:
```csharp
// Change:
scale.StartValue = 0;
scale.EndValue = 100;

// To:
axis.Minimum = 0;
axis.Maximum = 100;
```

### Issue: Color property not found

**Solution:** Use `Fill` property with Brush:
```csharp
// Change:
pointer.Color = Color.Blue;

// To:
pointer.Fill = Colors.Blue;
```

## Next Steps

1. Update NuGet package: `Syncfusion.Maui.Gauges`
2. Update namespaces
3. Replace control names (SfCircularGauge → SfRadialGauge)
4. Update property names (StartValue → Minimum, etc.)
5. Update collections (Scales → Axes)
6. Update Color → Fill conversions
7. Test gauge rendering and interactions
