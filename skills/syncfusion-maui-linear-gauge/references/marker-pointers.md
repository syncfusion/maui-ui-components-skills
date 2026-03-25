# Marker Pointers

## Table of Contents
- [Overview](#overview)
- [Shape Marker Pointer](#shape-marker-pointer)
  - [Available Shapes](#available-shapes)
  - [Shape Size](#shape-size)
  - [Shape Styling](#shape-styling)
  - [Shape Position](#shape-position)
  - [Shape Offset](#shape-offset)
  - [Stroke Customization](#stroke-customization)
- [Content Marker Pointer](#content-marker-pointer)
  - [Image Content](#image-content)
  - [Text Content](#text-content)
  - [Custom View Content](#custom-view-content)
  - [Content Alignment](#content-alignment)
  - [Content Position](#content-position)
  - [Content Offset](#content-offset)
- [Multiple Marker Pointers](#multiple-marker-pointers)
- [Common Marker Patterns](#common-marker-patterns)

## Overview

Marker pointers indicate specific values on the scale without filling from the minimum. They're perfect for precise value indicators, targets, and comparison points.

**Two Types:**
1. **Shape Marker Pointer** - Built-in geometric shapes
2. **Content Marker Pointer** - Custom content (images, text, views)

**Best Uses:**
- Current value indicators
- Target or goal markers
- Threshold indicators
- Multi-point comparisons
- Interactive value selectors

## Shape Marker Pointer

### Available Shapes

The LinearShapePointer supports five built-in shapes:

**Shape Types:**
- `InvertedTriangle` (default) - Downward pointing triangle
- `Triangle` - Upward pointing triangle
- `Circle` - Round marker
- `Diamond` - Diamond/rhombus shape
- `Rectangle` - Square/rectangular marker

**XAML:**
```xml
<!-- Default shape (Inverted Triangle) -->
<gauge:LinearShapePointer Value="50"/>

<!-- Circle -->
<gauge:LinearShapePointer Value="60" ShapeType="Circle"/>

<!-- Diamond -->
<gauge:LinearShapePointer Value="70" ShapeType="Diamond"/>

<!-- Triangle -->
<gauge:LinearShapePointer Value="80" ShapeType="Triangle"/>

<!-- Rectangle -->
<gauge:LinearShapePointer Value="90" ShapeType="Rectangle"/>
```

**C#:**
```csharp
// Inverted Triangle (default)
gauge.MarkerPointers.Add(new LinearShapePointer
{
    Value = 50,
    ShapeType = LinearShapeType.InvertedTriangle
});

// Circle
gauge.MarkerPointers.Add(new LinearShapePointer
{
    Value = 60,
    ShapeType = LinearShapeType.Circle
});

// Diamond
gauge.MarkerPointers.Add(new LinearShapePointer
{
    Value = 70,
    ShapeType = LinearShapeType.Diamond
});

// Triangle
gauge.MarkerPointers.Add(new LinearShapePointer
{
    Value = 80,
    ShapeType = LinearShapeType.Triangle
});

// Rectangle
gauge.MarkerPointers.Add(new LinearShapePointer
{
    Value = 90,
    ShapeType = LinearShapeType.Rectangle
});
```

### Shape Size

Control marker size with `ShapeHeight` and `ShapeWidth`.

**XAML:**
```xml
<gauge:LinearShapePointer Value="50" 
                         ShapeType="Circle"
                         ShapeHeight="25" 
                         ShapeWidth="25"/>
```

**C#:**
```csharp
gauge.MarkerPointers.Add(new LinearShapePointer
{
    Value = 50,
    ShapeType = LinearShapeType.Circle,
    ShapeHeight = 25,
    ShapeWidth = 25
});
```

**Size Guidelines:**
- **Small (10-15px):** Subtle indicators, dense layouts
- **Medium (20-25px):** Standard markers, clear visibility
- **Large (30-40px):** Prominent indicators, interactive elements
- **Extra Large (40+px):** Primary focus, touch targets

**Aspect Ratio Examples:**

```csharp
// Square marker
gauge.MarkerPointers.Add(new LinearShapePointer
{
    ShapeHeight = 20,
    ShapeWidth = 20,
    ShapeType = LinearShapeType.Circle
});

// Wide marker
gauge.MarkerPointers.Add(new LinearShapePointer
{
    ShapeHeight = 15,
    ShapeWidth = 30,
    ShapeType = LinearShapeType.Rectangle
});

// Tall marker
gauge.MarkerPointers.Add(new LinearShapePointer
{
    ShapeHeight = 35,
    ShapeWidth = 15,
    ShapeType = LinearShapeType.Diamond
});
```

### Shape Styling

**Fill Color:**

**XAML:**
```xml
<gauge:LinearShapePointer Value="70" 
                         ShapeType="Circle"
                         Fill="#2196F3"/>
```

**C#:**
```csharp
gauge.MarkerPointers.Add(new LinearShapePointer
{
    Value = 70,
    ShapeType = LinearShapeType.Circle,
    Fill = new SolidColorBrush(Color.FromArgb("#2196F3"))
});
```

**Gradient Fill:**

```csharp
gauge.MarkerPointers.Add(new LinearShapePointer
{
    Value = 65,
    ShapeType = LinearShapeType.Circle,
    ShapeHeight = 30,
    ShapeWidth = 30,
    Fill = new RadialGradientBrush
    {
        GradientStops = new GradientStopCollection
        {
            new GradientStop { Color = Colors.LightBlue, Offset = 0.0f },
            new GradientStop { Color = Colors.Blue, Offset = 1.0f }
        }
    }
});
```

### Shape Position

Position markers relative to the scale.

**Position Options:**
- `Outside` - Outside the scale (default)
- `Inside` - Inside the scale
- `Cross` - Crossing the scale

**XAML:**
```xml
<!-- Outside (default) -->
<gauge:LinearShapePointer Value="50" Position="Outside"/>

<!-- Inside -->
<gauge:LinearShapePointer Value="60" Position="Inside"/>

<!-- Cross -->
<gauge:LinearShapePointer Value="70" Position="Cross"/>
```

**C#:**
```csharp
// Outside
gauge.MarkerPointers.Add(new LinearShapePointer
{
    Value = 50,
    Position = GaugeElementPosition.Outside
});

// Inside
gauge.MarkerPointers.Add(new LinearShapePointer
{
    Value = 60,
    Position = GaugeElementPosition.Inside
});

// Cross
gauge.MarkerPointers.Add(new LinearShapePointer
{
    Value = 70,
    Position = GaugeElementPosition.Cross
});
```

### Shape Offset

Adjust marker distance from scale with `Offset`.

**XAML:**
```xml
<gauge:LinearShapePointer Value="60" 
                         Position="Outside" 
                         Offset="10"/>
```

**C#:**
```csharp
gauge.MarkerPointers.Add(new LinearShapePointer
{
    Value = 60,
    Position = GaugeElementPosition.Outside,
    Offset = 10  // 10 pixels from scale
});
```

### Stroke Customization

Add borders to shape markers.

**XAML:**
```xml
<gauge:LinearShapePointer Value="50" 
                         ShapeType="Circle"
                         Fill="White"
                         Stroke="#2196F3"
                         StrokeThickness="3"
                         ShapeHeight="25"
                         ShapeWidth="25"/>
```

**C#:**
```csharp
gauge.MarkerPointers.Add(new LinearShapePointer
{
    Value = 50,
    ShapeType = LinearShapeType.Circle,
    Fill = new SolidColorBrush(Colors.White),
    Stroke = new SolidColorBrush(Color.FromArgb("#2196F3")),
    StrokeThickness = 3,
    ShapeHeight = 25,
    ShapeWidth = 25
});
```

**Example: Hollow Marker**

```csharp
gauge.MarkerPointers.Add(new LinearShapePointer
{
    Value = 75,
    ShapeType = LinearShapeType.Circle,
    Fill = new SolidColorBrush(Colors.Transparent),  // Transparent fill
    Stroke = new SolidColorBrush(Colors.Red),
    StrokeThickness = 2,
    ShapeHeight = 20,
    ShapeWidth = 20
});
```

## Content Marker Pointer

Use custom content as markers - images, text, or any MAUI view.

### Image Content

**XAML:**
```xml
<gauge:LinearContentPointer Value="50">
    <gauge:LinearContentPointer.Content>
        <Image Source="pin.png" 
               HeightRequest="25" 
               WidthRequest="25"/>
    </gauge:LinearContentPointer.Content>
</gauge:LinearContentPointer>
```

**C#:**
```csharp
gauge.MarkerPointers.Add(new LinearContentPointer
{
    Value = 50,
    Content = new Image
    {
        Source = "pin.png",
        HeightRequest = 25,
        WidthRequest = 25
    }
});
```

**Example: Icon Marker**

```csharp
gauge.MarkerPointers.Add(new LinearContentPointer
{
    Value = 80,
    Content = new Image
    {
        Source = "checkmark_icon.png",
        HeightRequest = 30,
        WidthRequest = 30,
        Aspect = Aspect.AspectFit
    }
});
```

### Text Content

**XAML:**
```xml
<gauge:LinearContentPointer Value="65">
    <gauge:LinearContentPointer.Content>
        <Label Text="65°" 
               TextColor="White"
               BackgroundColor="#2196F3"
               Padding="5"
               FontSize="14"
               FontAttributes="Bold"/>
    </gauge:LinearContentPointer.Content>
</gauge:LinearContentPointer>
```

**C#:**
```csharp
gauge.MarkerPointers.Add(new LinearContentPointer
{
    Value = 65,
    Content = new Label
    {
        Text = "65°",
        TextColor = Colors.White,
        BackgroundColor = Color.FromArgb("#2196F3"),
        Padding = 5,
        FontSize = 14,
        FontAttributes = FontAttributes.Bold
    }
});
```

### Custom View Content

Create complex markers with multiple elements.

**XAML:**
```xml
<gauge:LinearContentPointer Value="75" Alignment="End">
    <gauge:LinearContentPointer.Content>
        <Grid HeightRequest="30" WidthRequest="30">
            <RoundRectangle CornerRadius="5" Fill="#FF5722"/>
            <Label Text="75" 
                   HorizontalOptions="Center"
                   VerticalOptions="Center" 
                   TextColor="White"
                   FontSize="12"
                   FontAttributes="Bold"/>
        </Grid>
    </gauge:LinearContentPointer.Content>
</gauge:LinearContentPointer>
```

**C#:**
```csharp
Grid customMarker = new Grid
{
    HeightRequest = 30,
    WidthRequest = 30
};

customMarker.Add(new RoundRectangle
{
    Fill = new SolidColorBrush(Color.FromArgb("#FF5722")),
    CornerRadius = 5
});

customMarker.Add(new Label
{
    Text = "75",
    TextColor = Colors.White,
    FontSize = 12,
    FontAttributes = FontAttributes.Bold,
    HorizontalOptions = LayoutOptions.Center,
    VerticalOptions = LayoutOptions.Center
});

gauge.MarkerPointers.Add(new LinearContentPointer
{
    Value = 75,
    Alignment = GaugeAlignment.End,
    Content = customMarker
});
```

**Example: Icon + Text Marker**

```csharp
HorizontalStackLayout markerContent = new HorizontalStackLayout
{
    Spacing = 5,
    Padding = 5,
    BackgroundColor = Colors.White
};

markerContent.Add(new Image
{
    Source = "warning.png",
    HeightRequest = 16,
    WidthRequest = 16
});

markerContent.Add(new Label
{
    Text = "Alert",
    FontSize = 12,
    TextColor = Colors.Red,
    VerticalOptions = LayoutOptions.Center
});

gauge.MarkerPointers.Add(new LinearContentPointer
{
    Value = 90,
    Content = markerContent
});
```

### Content Alignment

Control how content aligns with the value position.

**Alignment Options:**
- `Start` - Content starts at value position
- `Center` - Content centers on value position (default)
- `End` - Content ends at value position

**XAML:**
```xml
<!-- Center aligned (default) -->
<gauge:LinearContentPointer Value="50" Alignment="Center"/>

<!-- End aligned -->
<gauge:LinearContentPointer Value="60" Alignment="End"/>

<!-- Start aligned -->
<gauge:LinearContentPointer Value="70" Alignment="Start"/>
```

**C#:**
```csharp
// Center
gauge.MarkerPointers.Add(new LinearContentPointer
{
    Value = 50,
    Alignment = GaugeAlignment.Center
});

// End
gauge.MarkerPointers.Add(new LinearContentPointer
{
    Value = 60,
    Alignment = GaugeAlignment.End
});

// Start
gauge.MarkerPointers.Add(new LinearContentPointer
{
    Value = 70,
    Alignment = GaugeAlignment.Start
});
```

### Content Position

Position content markers relative to scale.

**XAML:**
```xml
<gauge:LinearContentPointer Value="50" Position="Outside">
    <gauge:LinearContentPointer.Content>
        <Image Source="marker.png" HeightRequest="20" WidthRequest="20"/>
    </gauge:LinearContentPointer.Content>
</gauge:LinearContentPointer>
```

**C#:**
```csharp
gauge.MarkerPointers.Add(new LinearContentPointer
{
    Value = 50,
    Position = GaugeElementPosition.Outside,
    Content = new Image { Source = "marker.png", HeightRequest = 20, WidthRequest = 20 }
});
```

### Content Offset

Adjust distance from scale.

**XAML:**
```xml
<gauge:LinearContentPointer Value="60" 
                           Position="Outside" 
                           OffsetY="15" OffsetX="15">
    <gauge:LinearContentPointer.Content>
        <Label Text="60" TextColor="Black"/>
    </gauge:LinearContentPointer.Content>
</gauge:LinearContentPointer>
```

**C#:**
```csharp
gauge.MarkerPointers.Add(new LinearContentPointer
{
    Value = 60,
    Position = GaugeElementPosition.Outside,
    OffsetY = 15,
    OffsetX = 15,
    Content = new Label { Text = "60", TextColor = Colors.Black }
});
```

## Multiple Marker Pointers

Combine multiple markers on the same scale.

**Example: Current vs Target**

```xml
<gauge:SfLinearGauge>
    <gauge:SfLinearGauge.MarkerPointers>
        
        <!-- Target marker (gray triangle) -->
        <gauge:LinearShapePointer Value="80" 
                                 ShapeType="InvertedTriangle"
                                 Fill="Gray"
                                 ShapeHeight="15"
                                 ShapeWidth="15"/>
        
        <!-- Current value (blue circle) -->
        <gauge:LinearShapePointer Value="65" 
                                 ShapeType="Circle"
                                 Fill="#2196F3"
                                 ShapeHeight="20"
                                 ShapeWidth="20"
                                 IsInteractive="True"/>
    </gauge:SfLinearGauge.MarkerPointers>
</gauge:SfLinearGauge>
```

**Example: Min/Max/Current**

```csharp
// Minimum marker
gauge.MarkerPointers.Add(new LinearShapePointer
{
    Value = 20,
    ShapeType = LinearShapeType.Triangle,
    Fill = new SolidColorBrush(Colors.Blue),
    ShapeHeight = 12,
    ShapeWidth = 12
});

// Maximum marker
gauge.MarkerPointers.Add(new LinearShapePointer
{
    Value = 95,
    ShapeType = LinearShapeType.InvertedTriangle,
    Fill = new SolidColorBrush(Colors.Red),
    ShapeHeight = 12,
    ShapeWidth = 12
});

// Current value marker
gauge.MarkerPointers.Add(new LinearShapePointer
{
    Value = 67,
    ShapeType = LinearShapeType.Diamond,
    Fill = new SolidColorBrush(Colors.Green),
    ShapeHeight = 18,
    ShapeWidth = 18
});
```

## Common Marker Patterns

### Pattern 1: Threshold Indicator

```csharp
// Warning threshold
gauge.MarkerPointers.Add(new LinearContentPointer
{
    Value = 75,
    Alignment = GaugeAlignment.End,
    Content = new Image
    {
        Source = "warning_icon.png",
        HeightRequest = 20,
        WidthRequest = 20
    }
});

// Critical threshold
gauge.MarkerPointers.Add(new LinearContentPointer
{
    Value = 90,
    Alignment = GaugeAlignment.End,
    Content = new Image
    {
        Source = "alert_icon.png",
        HeightRequest = 20,
        WidthRequest = 20
    }
});
```

### Pattern 2: Value with Label

```xml
<gauge:LinearContentPointer Value="68" Alignment="End">
    <gauge:LinearContentPointer.Content>
        <VerticalStackLayout Spacing="2">
            <Label Text="68°F" 
                   FontSize="14" 
                   FontAttributes="Bold"
                   HorizontalOptions="Center"/>
            <BoxView Color="#2196F3" 
                     HeightRequest="3" 
                     WidthRequest="30"
                     CornerRadius="1.5"/>
        </VerticalStackLayout>
    </gauge:LinearContentPointer.Content>
</gauge:LinearContentPointer>
```

### Pattern 3: Multi-Shape Comparison

```csharp
// Yesterday (gray circle)
gauge.MarkerPointers.Add(new LinearShapePointer
{
    Value = 55,
    ShapeType = LinearShapeType.Circle,
    Fill = new SolidColorBrush(Colors.LightGray),
    ShapeHeight = 15,
    ShapeWidth = 15
});

// Today (blue circle)
gauge.MarkerPointers.Add(new LinearShapePointer
{
    Value = 72,
    ShapeType = LinearShapeType.Circle,
    Fill = new SolidColorBrush(Colors.Blue),
    ShapeHeight = 20,
    ShapeWidth = 20
});
```

### Pattern 4: Interactive Selector with Value Display

```xml
<gauge:LinearContentPointer Value="50" 
                           IsInteractive="True"
                           ValueChanging="OnValueChanging">
    <gauge:LinearContentPointer.Content>
        <Grid>
            <!-- Circular handle -->
            <Ellipse Fill="#2196F3" 
                    WidthRequest="30" 
                    HeightRequest="30"/>
            <!-- Value label -->
            <Label x:Name="ValueLabel" 
                   Text="50" 
                   TextColor="White"
                   FontSize="12"
                   HorizontalOptions="Center"
                   VerticalOptions="Center"/>
        </Grid>
    </gauge:LinearContentPointer.Content>
</gauge:LinearContentPointer>
```

```csharp
private void OnValueChanging(object sender, ValueChangingEventArgs e)
{
    ValueLabel.Text = e.NewValue.ToString("F0");
}
```

### Pattern 5: Layered Markers (Shape + Content)

```csharp
// Background shape
gauge.MarkerPointers.Add(new LinearShapePointer
{
    Value = 75,
    ShapeType = LinearShapeType.Circle,
    Fill = new SolidColorBrush(Colors.White),
    Stroke = new SolidColorBrush(Colors.Blue),
    StrokeThickness = 2,
    ShapeHeight = 35,
    ShapeWidth = 35
});

// Foreground content
gauge.MarkerPointers.Add(new LinearContentPointer
{
    Value = 75,
    Content = new Label
    {
        Text = "75",
        TextColor = Colors.Blue,
        FontSize = 14,
        FontAttributes = FontAttributes.Bold,
        HorizontalOptions = LayoutOptions.Center,
        VerticalOptions = LayoutOptions.Center
    }
});
```
