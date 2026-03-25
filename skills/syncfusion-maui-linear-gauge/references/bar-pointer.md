# Bar Pointer

## Table of Contents
- [Overview](#overview)
- [Basic Bar Pointer](#basic-bar-pointer)
- [Bar Pointer Thickness](#bar-pointer-thickness)
- [Corner Styles](#corner-styles)
- [Position](#position)
- [Offset](#offset)
- [Styling](#styling)
- [Gradient Fill](#gradient-fill)
- [Child Content](#child-content)
- [Common Bar Pointer Patterns](#common-bar-pointer-patterns)

## Overview

A bar pointer is a filled bar that extends from the scale minimum to a specific value. It's ideal for progress indicators, volume displays, and filled measurement visualizations.

**Key Characteristics:**
- Always starts from scale minimum
- Fills to the specified value
- Customizable thickness, color, and corner style
- Supports gradients and child content
- Can be positioned inside, outside, or crossing the scale

**Best Uses:**
- Progress bars and loading indicators
- Volume/level meters
- Filled thermometers
- Capacity indicators
- Completion percentage displays

## Basic Bar Pointer

Create a simple bar pointer by specifying its value.

**XAML:**
```xml
<gauge:SfLinearGauge>
    <gauge:SfLinearGauge.BarPointers>
        <gauge:BarPointer Value="50"/>
    </gauge:SfLinearGauge.BarPointers>
</gauge:SfLinearGauge>
```

**C#:**
```csharp
SfLinearGauge gauge = new SfLinearGauge();
gauge.BarPointers.Add(new BarPointer
{
    Value = 50
});
this.Content = gauge;
```

**Properties:**
- `Value` - Required: The endpoint value for the bar
- Default appearance: Crosses scale, flat corners
- Bar fills from minimum to specified value

## Bar Pointer Thickness

Control bar thickness using the `PointerSize` property.

**XAML:**
```xml
<gauge:BarPointer Value="60" PointerSize="15"/>
```

**C#:**
```csharp
gauge.BarPointers.Add(new BarPointer
{
    Value = 60,
    PointerSize = 15
});
```

**Thickness Guidelines:**
- **Thin (2-5px):** Minimalist, accent lines
- **Medium (8-12px):** Standard progress bars
- **Thick (15-30px):** Prominent indicators, capacity meters
- **Extra thick (30+px):** Hero elements, primary focus

**Example: Variable Thickness**

```csharp
// Thin accent
gauge.BarPointers.Add(new BarPointer
{
    Value = 25,
    PointerSize = 3,
    Fill = new SolidColorBrush(Colors.Gray)
});

// Standard progress
gauge.BarPointers.Add(new BarPointer
{
    Value = 65,
    PointerSize = 12,
    Fill = new SolidColorBrush(Colors.Blue)
});

// Thick capacity indicator
gauge.BarPointers.Add(new BarPointer
{
    Value = 90,
    PointerSize = 25,
    Fill = new SolidColorBrush(Colors.Green)
});
```

## Corner Styles

Customize bar endpoints with the `CornerStyle` property.

### Available Styles

**`BothFlat`** - Square corners on both ends (default)  
**`BothCurve`** - Rounded corners on both ends  
**`StartCurve`** - Rounded start, flat end  
**`EndCurve`** - Flat start, rounded end

**XAML:**
```xml
<!-- Fully rounded -->
<gauge:BarPointer Value="50" 
                  PointerSize="15"
                  CornerStyle="BothCurve"/>

<!-- Rounded end only -->
<gauge:BarPointer Value="75" 
                  PointerSize="15"
                  CornerStyle="EndCurve"/>
```

**C#:**
```csharp
// Both curved
gauge.BarPointers.Add(new BarPointer
{
    Value = 50,
    PointerSize = 15,
    CornerStyle = CornerStyle.BothCurve
});

// End curved only
gauge.BarPointers.Add(new BarPointer
{
    Value = 75,
    PointerSize = 15,
    CornerStyle = CornerStyle.EndCurve
});
```

**Style Comparison:**

```xml
<VerticalStackLayout Spacing="20">
    
    <!-- BothFlat (default) -->
    <gauge:SfLinearGauge>
        <gauge:SfLinearGauge.BarPointers>
            <gauge:BarPointer Value="60" 
                             PointerSize="12"
                             CornerStyle="BothFlat"/>
        </gauge:SfLinearGauge.BarPointers>
    </gauge:SfLinearGauge>
    
    <!-- BothCurve -->
    <gauge:SfLinearGauge>
        <gauge:SfLinearGauge.BarPointers>
            <gauge:BarPointer Value="60" 
                             PointerSize="12"
                             CornerStyle="BothCurve"/>
        </gauge:SfLinearGauge.BarPointers>
    </gauge:SfLinearGauge>
    
    <!-- StartCurve -->
    <gauge:SfLinearGauge>
        <gauge:SfLinearGauge.BarPointers>
            <gauge:BarPointer Value="60" 
                             PointerSize="12"
                             CornerStyle="StartCurve"/>
        </gauge:SfLinearGauge.BarPointers>
    </gauge:SfLinearGauge>
    
    <!-- EndCurve -->
    <gauge:SfLinearGauge>
        <gauge:SfLinearGauge.BarPointers>
            <gauge:BarPointer Value="60" 
                             PointerSize="12"
                             CornerStyle="EndCurve"/>
        </gauge:SfLinearGauge.BarPointers>
    </gauge:SfLinearGauge>
    
</VerticalStackLayout>
```

## Position

Position the bar pointer relative to the scale using the `Position` property.

### Position Options

**`Cross`** - Crosses the scale (default)  
**`Inside`** - Inside the scale track  
**`Outside`** - Outside the scale track

**XAML:**
```xml
<!-- Inside the scale -->
<gauge:BarPointer Value="60" Position="Inside"/>

<!-- Outside the scale -->
<gauge:BarPointer Value="60" Position="Outside"/>
```

**C#:**
```csharp
// Inside
gauge.BarPointers.Add(new BarPointer
{
    Value = 60,
    Position = GaugeElementPosition.Inside
});

// Outside
gauge.BarPointers.Add(new BarPointer
{
    Value = 60,
    Position = GaugeElementPosition.Outside
});
```

**Use Cases:**
- **Cross:** Standard progress bars, overlays
- **Inside:** Compact layouts, filled scales
- **Outside:** Separate indicator, layered displays

## Offset

Adjust distance from scale using the `Offset` property.

**XAML:**
```xml
<gauge:BarPointer Value="50" 
                  Position="Outside" 
                  Offset="10"/>
```

**C#:**
```csharp
gauge.BarPointers.Add(new BarPointer
{
    Value = 50,
    Position = GaugeElementPosition.Outside,
    Offset = 10  // 10 pixels from scale
});
```

**Offset Behavior:**
- **Positive:** Moves away from scale
- **Negative:** Moves toward scale (overlap possible)
- **Zero:** Adjacent to scale
- **No effect on Cross position**

**Example: Layered Indicators**

```csharp
// Primary indicator (inside)
gauge.BarPointers.Add(new BarPointer
{
    Value = 75,
    Position = GaugeElementPosition.Inside,
    PointerSize = 10,
    Fill = new SolidColorBrush(Colors.Blue)
});

// Secondary indicator (outside with offset)
gauge.BarPointers.Add(new BarPointer
{
    Value = 60,
    Position = GaugeElementPosition.Outside,
    Offset = 5,
    PointerSize = 8,
    Fill = new SolidColorBrush(Colors.LightBlue)
});
```

## Styling

### Solid Color Fill

**XAML:**
```xml
<gauge:BarPointer Value="70" Fill="#2196F3"/>
```

**C#:**
```csharp
gauge.BarPointers.Add(new BarPointer
{
    Value = 70,
    Fill = new SolidColorBrush(Color.FromArgb("#2196F3"))
});
```

### Common Color Schemes

**Success Indicator:**
```csharp
gauge.BarPointers.Add(new BarPointer
{
    Value = 85,
    Fill = new SolidColorBrush(Color.FromArgb("#4CAF50"))  // Green
});
```

**Warning Indicator:**
```csharp
gauge.BarPointers.Add(new BarPointer
{
    Value = 55,
    Fill = new SolidColorBrush(Color.FromArgb("#FF9800"))  // Orange
});
```

**Error/Critical:**
```csharp
gauge.BarPointers.Add(new BarPointer
{
    Value = 25,
    Fill = new SolidColorBrush(Color.FromArgb("#F44336"))  // Red
});
```

### Semi-Transparent

```csharp
gauge.BarPointers.Add(new BarPointer
{
    Value = 60,
    Fill = new SolidColorBrush(Color.FromArgb("#804CAF50"))  // 50% opacity green
});
```

## Gradient Fill

Apply smooth color transitions using `GradientStops`.

**XAML:**
```xml
<gauge:BarPointer Value="70" 
                  Position="Outside" 
                  Offset="5"
                  CornerStyle="BothCurve">
    <gauge:BarPointer.GradientStops>
        <gauge:GaugeGradientStop Value="0" Color="#4CAF50"/>
        <gauge:GaugeGradientStop Value="35" Color="#FFC107"/>
        <gauge:GaugeGradientStop Value="70" Color="#F44336"/>
    </gauge:BarPointer.GradientStops>
</gauge:BarPointer>
```

**C#:**
```csharp
var gradientStops = new ObservableCollection<GaugeGradientStop>
{
    new GaugeGradientStop { Value = 0, Color = Color.FromArgb("#4CAF50") },   // Green
    new GaugeGradientStop { Value = 35, Color = Color.FromArgb("#FFC107") },  // Yellow
    new GaugeGradientStop { Value = 70, Color = Color.FromArgb("#F44336") }   // Red
};

gauge.BarPointers.Add(new BarPointer
{
    Value = 70,
    Position = GaugeElementPosition.Outside,
    Offset = 5,
    CornerStyle = CornerStyle.BothCurve,
    GradientStops = gradientStops
});
```

**Gradient Patterns:**

**Two-Tone:**
```csharp
var twoToneGradient = new ObservableCollection<GaugeGradientStop>
{
    new GaugeGradientStop { Value = 0, Color = Colors.Blue },
    new GaugeGradientStop { Value = 50, Color = Colors.Cyan }
};
```

**Rainbow:**
```csharp
var rainbowGradient = new ObservableCollection<GaugeGradientStop>
{
    new GaugeGradientStop { Value = 0, Color = Colors.Red },
    new GaugeGradientStop { Value = 16.67, Color = Colors.Orange },
    new GaugeGradientStop { Value = 33.33, Color = Colors.Yellow },
    new GaugeGradientStop { Value = 50, Color = Colors.Green },
    new GaugeGradientStop { Value = 66.67, Color = Colors.Blue },
    new GaugeGradientStop { Value = 83.33, Color = Colors.Indigo },
    new GaugeGradientStop { Value = 100, Color = Colors.Violet }
};
```

**Temperature Gradient:**
```csharp
var tempGradient = new ObservableCollection<GaugeGradientStop>
{
    new GaugeGradientStop { Value = 0, Color = Colors.Blue },      // Cold
    new GaugeGradientStop { Value = 50, Color = Colors.Green },    // Moderate
    new GaugeGradientStop { Value = 100, Color = Colors.Red }      // Hot
};
```

## Child Content

Add custom content (text, images, icons) inside bar pointers using the `Child` property.

**XAML:**
```xml
<gauge:SfLinearGauge ShowLabels="False" ShowTicks="False">
    <gauge:SfLinearGauge.LineStyle>
        <gauge:LinearLineStyle CornerStyle="BothCurve" Thickness="30"/>
    </gauge:SfLinearGauge.LineStyle>
    
    <gauge:SfLinearGauge.BarPointers>
        <gauge:BarPointer Value="65" 
                         PointerSize="30"
                         CornerStyle="BothCurve"
                         Fill="#2196F3">
            <gauge:BarPointer.Child>
                <Label Text="65%" 
                       Margin="0,0,15,0"
                       HorizontalOptions="End"
                       VerticalOptions="Center"
                       TextColor="White"
                       FontSize="16"
                       FontAttributes="Bold"/>
            </gauge:BarPointer.Child>
        </gauge:BarPointer>
    </gauge:SfLinearGauge.BarPointers>
</gauge:SfLinearGauge>
```

**C#:**
```csharp
SfLinearGauge progressGauge = new SfLinearGauge
{
    ShowLabels = false,
    ShowTicks = false
};

progressGauge.LineStyle = new LinearLineStyle
{
    Thickness = 30,
    CornerStyle = CornerStyle.BothCurve
};

Label percentLabel = new Label
{
    Text = "65%",
    Margin = new Thickness(0, 0, 15, 0),
    HorizontalOptions = LayoutOptions.End,
    VerticalOptions = LayoutOptions.Center,
    TextColor = Colors.White,
    FontSize = 16,
    FontAttributes = FontAttributes.Bold
};

progressGauge.BarPointers.Add(new BarPointer
{
    Value = 65,
    PointerSize = 30,
    CornerStyle = CornerStyle.BothCurve,
    Fill = new SolidColorBrush(Color.FromArgb("#2196F3")),
    Child = percentLabel
});
```

**Child Content Examples:**

**Icon + Text:**
```csharp
var iconTextStack = new HorizontalStackLayout
{
    Spacing = 5,
    HorizontalOptions = LayoutOptions.End,
    VerticalOptions = LayoutOptions.Center,
    Margin = new Thickness(0, 0, 10, 0)
};

iconTextStack.Add(new Image
{
    Source = "check_icon.png",
    HeightRequest = 16,
    WidthRequest = 16
});

iconTextStack.Add(new Label
{
    Text = "Complete",
    TextColor = Colors.White,
    FontSize = 14
});

barPointer.Child = iconTextStack;
```

**Dynamic Value Display:**
```csharp
Label valueLabel = new Label
{
    HorizontalOptions = LayoutOptions.End,
    VerticalOptions = LayoutOptions.Center,
    TextColor = Colors.White,
    Margin = new Thickness(0, 0, 10, 0)
};

// Update label when pointer value changes
barPointer.PropertyChanged += (s, e) =>
{
    if (e.PropertyName == "Value")
    {
        valueLabel.Text = $"{barPointer.Value:F1}%";
    }
};

barPointer.Child = valueLabel;
```

## Common Bar Pointer Patterns

### Pattern 1: Classic Progress Bar

```xml
<gauge:SfLinearGauge ShowLabels="False" ShowTicks="False">
    <!-- Background -->
    <gauge:SfLinearGauge.LineStyle>
        <gauge:LinearLineStyle Thickness="20" 
                              Fill="#E0E0E0"
                              CornerStyle="BothCurve"/>
    </gauge:SfLinearGauge.LineStyle>
    
    <!-- Progress -->
    <gauge:SfLinearGauge.BarPointers>
        <gauge:BarPointer Value="70" 
                         PointerSize="20"
                         Fill="#4CAF50"
                         CornerStyle="BothCurve"
                         EnableAnimation="True"
                         AnimationDuration="1000"/>
    </gauge:SfLinearGauge.BarPointers>
</gauge:SfLinearGauge>
```

### Pattern 2: Thermometer (Vertical)

```csharp
SfLinearGauge thermometer = new SfLinearGauge
{
    Minimum = -20,
    Maximum = 50,
    Orientation = GaugeOrientation.Vertical,
    HeightRequest = 400
};

thermometer.LineStyle = new LinearLineStyle
{
    Thickness = 15,
    Fill = new SolidColorBrush(Colors.LightGray),
    CornerStyle = CornerStyle.BothCurve
};

thermometer.BarPointers.Add(new BarPointer
{
    Value = 23,
    PointerSize = 15,
    Fill = new SolidColorBrush(Colors.Red),
    CornerStyle = CornerStyle.BothCurve,
    EnableAnimation = true
});
```

### Pattern 3: Multi-Bar Comparison

```csharp
// Target (gray, thinner)
gauge.BarPointers.Add(new BarPointer
{
    Value = 80,
    PointerSize = 15,
    Position = GaugeElementPosition.Outside,
    Fill = new SolidColorBrush(Colors.LightGray),
    CornerStyle = CornerStyle.BothCurve
});

// Actual (blue, thicker, inside)
gauge.BarPointers.Add(new BarPointer
{
    Value = 65,
    PointerSize = 20,
    Position = GaugeElementPosition.Inside,
    Fill = new SolidColorBrush(Colors.Blue),
    CornerStyle = CornerStyle.BothCurve
});
```

### Pattern 4: Segmented Progress with Gradient

```csharp
var progressGradient = new ObservableCollection<GaugeGradientStop>
{
    new GaugeGradientStop { Value = 0, Color = Color.FromArgb("#F44336") },
    new GaugeGradientStop { Value = 50, Color = Color.FromArgb("#FFC107") },
    new GaugeGradientStop { Value = 100, Color = Color.FromArgb("#4CAF50") }
};

gauge.BarPointers.Add(new BarPointer
{
    Value = 75,
    PointerSize = 25,
    CornerStyle = CornerStyle.BothCurve,
    GradientStops = progressGradient
});
```

### Pattern 5: Progress with Percentage Label

```csharp
double progress = 67;

Label progressLabel = new Label
{
    Text = $"{progress}%",
    TextColor = Colors.White,
    FontSize = 16,
    FontAttributes = FontAttributes.Bold,
    HorizontalOptions = LayoutOptions.End,
    VerticalOptions = LayoutOptions.Center,
    Margin = new Thickness(0, 0, 10, 0)
};

gauge.BarPointers.Add(new BarPointer
{
    Value = progress,
    PointerSize = 30,
    Fill = new SolidColorBrush(Color.FromArgb("#2196F3")),
    CornerStyle = CornerStyle.BothCurve,
    Child = progressLabel
});
```

### Pattern 6: Stacked Bar Indicators

```csharp
// Background layer
gauge.BarPointers.Add(new BarPointer
{
    Value = 100,
    PointerSize = 25,
    Fill = new SolidColorBrush(Color.FromArgb("#40000000")),  // Transparent black
    CornerStyle = CornerStyle.BothCurve
});

// Primary progress
gauge.BarPointers.Add(new BarPointer
{
    Value = 70,
    PointerSize = 25,
    Fill = new SolidColorBrush(Colors.Blue),
    CornerStyle = CornerStyle.BothCurve
});
```
