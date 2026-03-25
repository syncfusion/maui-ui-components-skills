# Annotations

## Table of Contents
- [Overview](#overview)
- [Creating Annotations](#creating-annotations)
- [Annotation Content](#annotation-content)
  - [Text Annotations](#text-annotations)
  - [Image Annotations](#image-annotations)
  - [Custom View Annotations](#custom-view-annotations)
- [Positioning Annotations](#positioning-annotations)
  - [DirectionUnit (Axis vs Angle)](#directionunit-axis-vs-angle)
  - [DirectionValue](#directionvalue)
  - [PositionFactor (Radial Distance)](#positionfactor-radial-distance)
- [Alignment](#alignment)
- [Multiple Annotations](#multiple-annotations)
- [Dynamic Annotations](#dynamic-annotations)
- [Common Use Cases](#common-use-cases)
  - [Value Labels](#value-labels)
  - [Units Display](#units-display)
  - [Status Indicators](#status-indicators)
  - [Branding Elements](#branding-elements)
- [Best Practices](#best-practices)

## Overview

Annotations allow you to add any .NET MAUI view (text, images, custom layouts) at specific positions on the radial gauge. They're perfect for displaying values, units, icons, labels, or any custom content that enhances the gauge's readability.

**Key Features:**
- Add unlimited annotations per axis
- Position by angle or axis value
- Control radial distance from center
- Use any .NET MAUI view as content
- Align content precisely
- Dynamic updates with data binding

## Creating Annotations

Add annotations to the `RadialAxis.Annotations` collection:

```xaml
<gauge:RadialAxis>
    <gauge:RadialAxis.Annotations>
        <gauge:GaugeAnnotation>
            <gauge:GaugeAnnotation.Content>
                <Label Text="50"
                       FontSize="20"
                       FontAttributes="Bold"
                       TextColor="Black" />
            </gauge:GaugeAnnotation.Content>
        </gauge:GaugeAnnotation>
    </gauge:RadialAxis.Annotations>
</gauge:RadialAxis>
```

```csharp
GaugeAnnotation annotation = new GaugeAnnotation
{
    Content = new Label 
    { 
        Text = "50", 
        FontSize = 20, 
        FontAttributes = FontAttributes.Bold,
        TextColor = Colors.Black
    }
};

axis.Annotations.Add(annotation);
```

**Default Position:** Center of the gauge (PositionFactor=0, DirectionValue=0)

## Annotation Content

The `Content` property accepts any .NET MAUI view.

### Text Annotations

**Simple Label:**
```xaml
<gauge:GaugeAnnotation>
    <gauge:GaugeAnnotation.Content>
        <Label Text="75 km/h"
               FontSize="24"
               FontAttributes="Bold"
               TextColor="Navy" />
    </gauge:GaugeAnnotation.Content>
</gauge:GaugeAnnotation>
```

**Formatted Text:**
```xaml
<gauge:GaugeAnnotation>
    <gauge:GaugeAnnotation.Content>
        <VerticalStackLayout>
            <Label Text="85"
                   FontSize="32"
                   FontAttributes="Bold"
                   HorizontalOptions="Center" />
            <Label Text="km/h"
                   FontSize="14"
                   TextColor="Gray"
                   HorizontalOptions="Center" />
        </VerticalStackLayout>
    </gauge:GaugeAnnotation.Content>
</gauge:GaugeAnnotation>
```

### Image Annotations

```xaml
<gauge:GaugeAnnotation DirectionUnit="Angle"
                       DirectionValue="270"
                       PositionFactor="0.3">
    <gauge:GaugeAnnotation.Content>
        <Image Source="logo.png"
               HeightRequest="40"
               WidthRequest="40" />
    </gauge:GaugeAnnotation.Content>
</gauge:GaugeAnnotation>
```

```csharp
annotation.Content = new Image 
{ 
    Source = "logo.png", 
    HeightRequest = 40, 
    WidthRequest = 40 
};
```

### Custom View Annotations

**Badge with Border:**
```xaml
<gauge:GaugeAnnotation>
    <gauge:GaugeAnnotation.Content>
        <Border BackgroundColor="White"
                Stroke="Black"
                StrokeThickness="2"
                Padding="10,5"
                StrokeShape="RoundRectangle 8">
            <Label Text="SPEED"
                   FontSize="12"
                   FontAttributes="Bold"
                   TextColor="Black" />
        </Border>
    </gauge:GaugeAnnotation.Content>
</gauge:GaugeAnnotation>
```

**Icon with Text:**
```xaml
<gauge:GaugeAnnotation>
    <gauge:GaugeAnnotation.Content>
        <HorizontalStackLayout Spacing="5">
            <Image Source="warning_icon.png"
                   HeightRequest="20"
                   WidthRequest="20" />
            <Label Text="High"
                   FontSize="14"
                   TextColor="Red"
                   VerticalOptions="Center" />
        </HorizontalStackLayout>
    </gauge:GaugeAnnotation.Content>
</gauge:GaugeAnnotation>
```

**Complex Layout:**
```xaml
<gauge:GaugeAnnotation DirectionValue="90"
                       PositionFactor="0.5">
    <gauge:GaugeAnnotation.Content>
        <Grid WidthRequest="100" HeightRequest="60"
              BackgroundColor="White"
              Padding="5">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto" />
                <RowDefinition Height="*" />
            </Grid.RowDefinitions>
            
            <Label Grid.Row="0"
                   Text="Current"
                   FontSize="10"
                   TextColor="Gray"
                   HorizontalOptions="Center" />
            
            <Label Grid.Row="1"
                   Text="75 km/h"
                   FontSize="18"
                   FontAttributes="Bold"
                   HorizontalOptions="Center"
                   VerticalOptions="Center" />
        </Grid>
    </gauge:GaugeAnnotation.Content>
</gauge:GaugeAnnotation>
```

## Positioning Annotations

Annotations are positioned using three key properties:

1. **DirectionUnit** - Position by `Angle` or `AxisValue`
2. **DirectionValue** - The angle (degrees) or axis value
3. **PositionFactor** - Distance from center (0=center, 1=edge)

### DirectionUnit (Axis vs Angle)

**Angle Positioning (Default):**
```xaml
<gauge:GaugeAnnotation DirectionUnit="Angle"
                       DirectionValue="90">
    <!-- Positioned at 90° (bottom) -->
</gauge:GaugeAnnotation>
```

**Angle Reference:**
- 0° = Right (3 o'clock)
- 90° = Bottom (6 o'clock)
- 180° = Left (9 o'clock)
- 270° = Top (12 o'clock)

```csharp
annotation.DirectionUnit = AnnotationDirection.Angle;
annotation.DirectionValue = 90;
```

**Axis Value Positioning:**
```xaml
<gauge:RadialAxis Minimum="0" Maximum="100">
    <gauge:RadialAxis.Annotations>
        <gauge:GaugeAnnotation DirectionUnit="AxisValue"
                               DirectionValue="75">
            <!-- Positioned at value 75 on the axis -->
        </gauge:GaugeAnnotation>
    </gauge:RadialAxis.Annotations>
</gauge:RadialAxis>
```

```csharp
annotation.DirectionUnit = AnnotationDirection.AxisValue;
annotation.DirectionValue = 75;
```

**When to Use Each:**
- **Angle:** Fixed position regardless of axis scale (logos, titles, fixed labels)
- **AxisValue:** Position relative to a value on the scale (markers, value indicators)

### DirectionValue

The meaning depends on `DirectionUnit`:

**If DirectionUnit = Angle:**
- Value is angle in degrees (0-360)
- Example: `DirectionValue="270"` places annotation at top

**If DirectionUnit = AxisValue:**
- Value is a point on the axis scale
- Example: For axis 0-100, `DirectionValue="50"` places annotation at the midpoint

### PositionFactor (Radial Distance)

Controls distance from the gauge center.

**Range:** 0 to 1 (and beyond)
- **0:** Center of gauge
- **0.5:** Halfway between center and axis
- **1:** On the axis line
- **>1:** Beyond the axis (outside gauge)

```xaml
<!-- Center -->
<gauge:GaugeAnnotation PositionFactor="0" />

<!-- Halfway to axis -->
<gauge:GaugeAnnotation PositionFactor="0.5" />

<!-- On axis -->
<gauge:GaugeAnnotation PositionFactor="1" />

<!-- Outside gauge -->
<gauge:GaugeAnnotation PositionFactor="1.2" />
```

```csharp
annotation.PositionFactor = 0.5;
```

**Default:** 0 (center)

## Alignment

Fine-tune annotation positioning using alignment properties:

```xaml
<gauge:GaugeAnnotation HorizontalAlignment="Center"
                       VerticalAlignment="Center">
    <!-- Annotation centered on its position -->
</gauge:GaugeAnnotation>
```

**Options:**
- `HorizontalAlignment`: Start, Center, End, Fill
- `VerticalAlignment`: Start, Center, End, Fill

```csharp
annotation.HorizontalAlignment = GaugeAlignment.Center;
annotation.VerticalAlignment = GaugeAlignment.Center;
```

**Use Cases:**
- **Center/Center:** Default, annotation centered on position
- **Start/Start:** Annotation starts from position (useful for labels outside gauge)
- **End/End:** Annotation ends at position

## Multiple Annotations

Add as many annotations as needed:

```xaml
<gauge:RadialAxis>
    <gauge:RadialAxis.Annotations>
        <!-- Center value -->
        <gauge:GaugeAnnotation PositionFactor="0">
            <gauge:GaugeAnnotation.Content>
                <Label Text="75" FontSize="32" FontAttributes="Bold" />
            </gauge:GaugeAnnotation.Content>
        </gauge:GaugeAnnotation>
        
        <!-- Units below value -->
        <gauge:GaugeAnnotation DirectionValue="90"
                               PositionFactor="0.2">
            <gauge:GaugeAnnotation.Content>
                <Label Text="km/h" FontSize="14" TextColor="Gray" />
            </gauge:GaugeAnnotation.Content>
        </gauge:GaugeAnnotation>
        
        <!-- Title at top -->
        <gauge:GaugeAnnotation DirectionValue="270"
                               PositionFactor="0.3">
            <gauge:GaugeAnnotation.Content>
                <Label Text="SPEED" FontSize="12" FontAttributes="Bold" />
            </gauge:GaugeAnnotation.Content>
        </gauge:GaugeAnnotation>
        
        <!-- Logo bottom-right -->
        <gauge:GaugeAnnotation DirectionValue="45"
                               PositionFactor="1.1">
            <gauge:GaugeAnnotation.Content>
                <Image Source="logo.png" HeightRequest="30" WidthRequest="30" />
            </gauge:GaugeAnnotation.Content>
        </gauge:GaugeAnnotation>
    </gauge:RadialAxis.Annotations>
</gauge:RadialAxis>
```

## Dynamic Annotations

Update annotation content dynamically using data binding:

```xaml
<gauge:GaugeAnnotation>
    <gauge:GaugeAnnotation.Content>
        <Label Text="{Binding CurrentSpeed}"
               FontSize="28"
               FontAttributes="Bold" />
    </gauge:GaugeAnnotation.Content>
</gauge:GaugeAnnotation>
```

**ViewModel:**
```csharp
public class GaugeViewModel : INotifyPropertyChanged
{
    private double _currentSpeed;
    public double CurrentSpeed
    {
        get => _currentSpeed;
        set
        {
            _currentSpeed = value;
            OnPropertyChanged();
        }
    }
    
    // INotifyPropertyChanged implementation...
}
```

**Update Position Dynamically:**
```csharp
// Update annotation position based on pointer value
annotation.DirectionValue = pointerValue;
annotation.PositionFactor = 1.1;
```

## Common Use Cases

### Value Labels

Display the current gauge value:

```xaml
<gauge:GaugeAnnotation PositionFactor="0">
    <gauge:GaugeAnnotation.Content>
        <VerticalStackLayout>
            <Label Text="{Binding Value}"
                   FontSize="36"
                   FontAttributes="Bold"
                   HorizontalOptions="Center" />
            <Label Text="°C"
                   FontSize="16"
                   TextColor="Gray"
                   HorizontalOptions="Center" />
        </VerticalStackLayout>
    </gauge:GaugeAnnotation.Content>
</gauge:GaugeAnnotation>
```

### Units Display

Show units or labels:

```xaml
<!-- Bottom -->
<gauge:GaugeAnnotation DirectionValue="90"
                       PositionFactor="0.7">
    <gauge:GaugeAnnotation.Content>
        <Label Text="PSI" FontSize="12" TextColor="Gray" />
    </gauge:GaugeAnnotation.Content>
</gauge:GaugeAnnotation>

<!-- Top -->
<gauge:GaugeAnnotation DirectionValue="270"
                       PositionFactor="0.3">
    <gauge:GaugeAnnotation.Content>
        <Label Text="PRESSURE" FontSize="10" FontAttributes="Bold" />
    </gauge:GaugeAnnotation.Content>
</gauge:GaugeAnnotation>
```

### Status Indicators

Show status or alerts:

```xaml
<gauge:GaugeAnnotation DirectionValue="180"
                       PositionFactor="1.1">
    <gauge:GaugeAnnotation.Content>
        <Grid>
            <Ellipse Fill="Red"
                     HeightRequest="30"
                     WidthRequest="30" />
            <Label Text="!"
                   FontSize="20"
                   FontAttributes="Bold"
                   TextColor="White"
                   HorizontalOptions="Center"
                   VerticalOptions="Center" />
        </Grid>
    </gauge:GaugeAnnotation.Content>
</gauge:GaugeAnnotation>
```

### Branding Elements

Add logos or custom branding:

```xaml
<gauge:GaugeAnnotation DirectionValue="270"
                       PositionFactor="0.2">
    <gauge:GaugeAnnotation.Content>
        <Image Source="company_logo.png"
               HeightRequest="50"
               WidthRequest="50" />
    </gauge:GaugeAnnotation.Content>
</gauge:GaugeAnnotation>
```

## Best Practices

1. **Center Important Values:** Use `PositionFactor="0"` for primary values
2. **Use Consistent Positioning:** Standardize annotation positions across similar gauges
3. **Consider Readability:** Ensure annotations don't overlap with pointers or ranges
4. **Keep Text Concise:** Short labels are easier to read at a glance
5. **Use Hierarchy:** Larger font for values, smaller for units/labels
6. **Test Different Sizes:** Ensure annotations scale well on different screen sizes
7. **Limit Count:** Too many annotations create visual clutter (3-5 is typical)
8. **Contrast Matters:** Ensure text is readable against gauge background

**Performance:**
- Annotations are lightweight
- Dynamic updates are efficient
- Use data binding for real-time value displays

**Common Mistakes:**
- ❌ Overlapping annotations (adjust PositionFactor or DirectionValue)
- ❌ Annotations behind pointers (use appropriate PositionFactor)
- ❌ Text too large for gauge size (test on small screens)
- ❌ Poor contrast with background (add borders or backgrounds to text)
- ❌ Not using VerticalStackLayout for value+unit combinations

**Layering:**
- Annotations are drawn in the order they're added to the collection
- Later annotations appear above earlier ones
- Position important annotations last for top visibility

## Summary

Annotations enhance gauges with custom content:
- **Content:** Any .NET MAUI view (text, images, layouts)
- **Positioning:** By angle or axis value
- **Distance:** PositionFactor controls radial distance
- **Alignment:** Fine-tune placement with alignment properties
- **Dynamic:** Update content via data binding
- **Multiple:** Add unlimited annotations per gauge

Use annotations to display values, units, labels, icons, and branding elements that make your gauges informative and visually appealing.
