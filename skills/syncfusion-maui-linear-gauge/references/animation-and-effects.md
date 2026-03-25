# Animation and Effects

## Table of Contents
- [Overview](#overview)
- [Axis Animation](#axis-animation)
- [Range Animation](#range-animation)
- [Pointer Animation](#pointer-animation)
  - [EnableAnimation Property](#enableanimation-property)
  - [Animation Duration](#animation-duration)
  - [Animation Easing](#animation-easing)
- [Mirror Gauge Effect](#mirror-gauge-effect)
- [Orientation Changes](#orientation-changes)
- [Complete Animation Examples](#complete-animation-examples)

## Overview

Linear Gauge supports animations for smooth visual transitions when elements load or values change. Animations improve user experience by providing visual continuity and feedback.

**Animatable Elements:**
- Scale, ticks, and labels (axis)
- Ranges
- All pointer types (bar, shape marker, content marker)

**Animation Properties:**
- **Enable/Disable** - Control which elements animate
- **Duration** - How long animations take
- **Easing** - Animation acceleration curves

**When to Use Animations:**
- Initial gauge load for dramatic effect
- Value changes to show progression
- User interactions for visual feedback
- Smooth transitions during updates

## Axis Animation

Animate the scale, ticks, and labels on initial load.

### EnableAxisAnimation

**XAML:**
```xml
<gauge:SfLinearGauge EnableAxisAnimation="True" 
                    AnimationDuration="1500"/>
```

**C#:**
```csharp
SfLinearGauge gauge = new SfLinearGauge
{
    EnableAxisAnimation = true,
    AnimationDuration = 1500  // 1.5 seconds
};
```

**Animation Effect:**
- Fade-in with opacity transition
- Scale elements gradually appear
- Smooth entry effect
- Default duration: 1000ms (1 second)

**Example: Dramatic Axis Entry**

```csharp
SfLinearGauge dramaticGauge = new SfLinearGauge
{
    Minimum = 0,
    Maximum = 100,
    Interval = 10,
    EnableAxisAnimation = true,
    AnimationDuration = 2000  // 2 seconds for dramatic effect
};

dramaticGauge.LineStyle = new LinearLineStyle
{
    Thickness = 12,
    Fill = new SolidColorBrush(Color.FromArgb("#2196F3"))
};
```

## Range Animation

Animate ranges on initial load.

### EnableRangeAnimation

**XAML:**
```xml
<gauge:SfLinearGauge EnableRangeAnimation="True" 
                    AnimationDuration="1200">
    <gauge:SfLinearGauge.Ranges>
        <gauge:LinearRange StartValue="0" EndValue="100" Fill="#4CAF50"/>
    </gauge:SfLinearGauge.Ranges>
</gauge:SfLinearGauge>
```

**C#:**
```csharp
SfLinearGauge gauge = new SfLinearGauge
{
    EnableRangeAnimation = true,
    AnimationDuration = 1200
};

gauge.Ranges.Add(new LinearRange
{
    StartValue = 0,
    EndValue = 100,
    Fill = new SolidColorBrush(Color.FromArgb("#4CAF50"))
});
```

**Animation Effect:**
- Fade-in with opacity transition
- Ranges gradually appear
- Coordinated with axis animation if both enabled

**Example: Multi-Range Animation**

```csharp
SfLinearGauge multiRangeGauge = new SfLinearGauge
{
    EnableRangeAnimation = true,
    AnimationDuration = 1500
};

// Ranges animate in together
multiRangeGauge.Ranges.Add(new LinearRange
{
    StartValue = 0,
    EndValue = 33,
    Fill = new SolidColorBrush(Colors.Green)
});

multiRangeGauge.Ranges.Add(new LinearRange
{
    StartValue = 33,
    EndValue = 66,
    Fill = new SolidColorBrush(Colors.Yellow)
});

multiRangeGauge.Ranges.Add(new LinearRange
{
    StartValue = 66,
    EndValue = 100,
    Fill = new SolidColorBrush(Colors.Red)
});
```

## Pointer Animation

All three pointer types support animation:
- Bar pointers
- Shape marker pointers
- Content marker pointers

### EnableAnimation Property

Enable animation for individual pointers.

**XAML:**
```xml
<gauge:BarPointer Value="75" 
                 EnableAnimation="True"
                 AnimationDuration="1000"/>

<gauge:LinearShapePointer Value="80" 
                         EnableAnimation="True"
                         AnimationDuration="1000"/>

<gauge:LinearContentPointer Value="65" 
                           EnableAnimation="True"
                           AnimationDuration="1000">
    <gauge:LinearContentPointer.Content>
        <Image Source="pin.png"/>
    </gauge:LinearContentPointer.Content>
</gauge:LinearContentPointer>
```

**C#:**
```csharp
// Bar pointer animation
gauge.BarPointers.Add(new BarPointer
{
    Value = 75,
    EnableAnimation = true,
    AnimationDuration = 1000
});

// Shape pointer animation
gauge.MarkerPointers.Add(new LinearShapePointer
{
    Value = 80,
    EnableAnimation = true,
    AnimationDuration = 1000
});

// Content pointer animation
gauge.MarkerPointers.Add(new LinearContentPointer
{
    Value = 65,
    EnableAnimation = true,
    AnimationDuration = 1000,
    Content = new Image { Source = "pin.png" }
});
```

**Default Values:**
- `EnableAnimation`: false
- `AnimationDuration`: 1000ms
- `AnimationEasing`: Easing.Linear

### Animation Duration

Control how long pointer animations take (in milliseconds).

**Fast Animation (500ms):**
```csharp
barPointer.EnableAnimation = true;
barPointer.AnimationDuration = 500;
```

**Standard Animation (1000ms):**
```csharp
barPointer.AnimationDuration = 1000;  // Default
```

**Slow Animation (2000ms):**
```csharp
barPointer.AnimationDuration = 2000;
```

**Duration Guidelines:**
- **Fast (300-500ms):** Quick updates, frequent changes
- **Standard (800-1200ms):** General purpose, balanced
- **Slow (1500-2500ms):** Dramatic effect, initial load
- **Very Slow (3000+ms):** Special cases, attention-grabbing

### Animation Easing

Control the acceleration curve of animations.

**Available Easing Types:**

| Easing | Description | Use Case |
|--------|-------------|----------|
| `Linear` | Constant speed (default) | Simple, predictable movement |
| `SinIn` | Slow start, accelerate | Gentle beginning |
| `SinOut` | Fast start, decelerate | Smooth landing |
| `SinInOut` | Slow start and end | Natural, fluid motion |
| `CubicIn` | Very slow start | Dramatic acceleration |
| `CubicOut` | Very fast start | Strong deceleration |
| `CubicInOut` | Slow start, fast middle, slow end | Emphasized motion |
| `BounceIn` | Bounce at start | Playful entry |
| `BounceOut` | Bounce at end | Fun landing effect |
| `SpringIn` | Spring tension at start | Elastic entry |
| `SpringOut` | Spring release at end | Bouncy exit |

**XAML:**
```xml
<gauge:BarPointer Value="70" 
                 EnableAnimation="True"
                 AnimationDuration="1000"
                 AnimationEasing="{x:Static Easing.BounceOut}"/>
```

**C#:**
```csharp
// Linear (default)
barPointer.AnimationEasing = Easing.Linear;

// Smooth deceleration
barPointer.AnimationEasing = Easing.SinOut;

// Bounce effect
barPointer.AnimationEasing = Easing.BounceOut;

// Spring effect
barPointer.AnimationEasing = Easing.SpringOut;

// Natural motion
barPointer.AnimationEasing = Easing.SinInOut;
```

**Easing Comparison Example:**

```csharp
SfLinearGauge easingDemo = new SfLinearGauge();

// Linear
easingDemo.BarPointers.Add(new BarPointer
{
    Value = 75,
    EnableAnimation = true,
    AnimationDuration = 1500,
    AnimationEasing = Easing.Linear,
    Position = GaugeElementPosition.Outside,
    Offset = 0
});

// BounceOut
easingDemo.BarPointers.Add(new BarPointer
{
    Value = 75,
    EnableAnimation = true,
    AnimationDuration = 1500,
    AnimationEasing = Easing.BounceOut,
    Position = GaugeElementPosition.Outside,
    Offset = 15
});

// SinInOut
easingDemo.BarPointers.Add(new BarPointer
{
    Value = 75,
    EnableAnimation = true,
    AnimationDuration = 1500,
    AnimationEasing = Easing.SinInOut,
    Position = GaugeElementPosition.Outside,
    Offset = 30
});
```

**Animation on Value Change:**

Pointers animate when their `Value` property changes at runtime:

```csharp
BarPointer animatedPointer = new BarPointer
{
    Value = 30,
    EnableAnimation = true,
    AnimationDuration = 800,
    AnimationEasing = Easing.SinOut
};

gauge.BarPointers.Add(animatedPointer);

// Later: change value triggers animation
animatedPointer.Value = 75;  // Animates from 30 to 75
```

## Mirror Gauge Effect

The `IsMirrored` property flips all gauge elements horizontally or vertically.

### IsMirrored Property

**XAML:**
```xml
<gauge:SfLinearGauge IsMirrored="True"/>
```

**C#:**
```csharp
gauge.IsMirrored = true;
```

**Effect:**
- **Horizontal gauges:** Labels/ticks move to opposite side
- **Vertical gauges:** Labels/ticks move to opposite side
- **All elements mirrored:** Scale, ranges, pointers, labels, ticks

### Mirrored vs Normal Comparison

**Normal Horizontal Gauge:**
```
Labels at bottom
Scale: [0 -------- 50 -------- 100]
```

**Mirrored Horizontal Gauge:**
```
Scale: [0 -------- 50 -------- 100]
Labels at top
```

**Normal Vertical Gauge:**
```
100 --|
 75 --|
 50 --|
 25 --|
  0 --|
```

**Mirrored Vertical Gauge:**
```
|-- 100
|-- 75
|-- 50
|-- 25
|-- 0
```

### Use Cases for Mirroring

**RTL (Right-to-Left) Localization:**
```csharp
// Auto-mirror for RTL languages
if (CultureInfo.CurrentCulture.TextInfo.IsRightToLeft)
{
    gauge.IsMirrored = true;
}
```

**Symmetric Dual Gauges:**
```xml
<HorizontalStackLayout>
    <!-- Left gauge (normal) -->
    <gauge:SfLinearGauge IsMirrored="False">
        <gauge:SfLinearGauge.BarPointers>
            <gauge:BarPointer Value="60"/>
        </gauge:SfLinearGauge.BarPointers>
    </gauge:SfLinearGauge>
    
    <!-- Right gauge (mirrored) -->
    <gauge:SfLinearGauge IsMirrored="True">
        <gauge:SfLinearGauge.BarPointers>
            <gauge:BarPointer Value="75"/>
        </gauge:SfLinearGauge.BarPointers>
    </gauge:SfLinearGauge>
</HorizontalStackLayout>
```

**Custom Layout Requirements:**
```csharp
// Labels on top instead of bottom
gauge.IsMirrored = true;
```

## Orientation Changes

Switch between horizontal and vertical orientations.

**Horizontal:**
```xml
<gauge:SfLinearGauge Orientation="Horizontal"/>
```

**Vertical:**
```xml
<gauge:SfLinearGauge Orientation="Vertical" HeightRequest="300"/>
```

**Dynamic Orientation Change:**
```csharp
private void ToggleOrientation()
{
    if (gauge.Orientation == GaugeOrientation.Horizontal)
        gauge.Orientation = GaugeOrientation.Vertical;
    else
        gauge.Orientation = GaugeOrientation.Horizontal;
}
```

**Responsive Orientation:**
```csharp
// Switch orientation based on screen size
private void UpdateOrientationForScreen()
{
    double screenWidth = DeviceDisplay.MainDisplayInfo.Width;
    double screenHeight = DeviceDisplay.MainDisplayInfo.Height;
    
    if (screenWidth > screenHeight)
        gauge.Orientation = GaugeOrientation.Horizontal;
    else
        gauge.Orientation = GaugeOrientation.Vertical;
}
```

## Complete Animation Examples

### Example 1: Fully Animated Gauge (All Elements)

```csharp
SfLinearGauge fullyAnimated = new SfLinearGauge
{
    Minimum = 0,
    Maximum = 100,
    Interval = 20,
    
    // Animate axis
    EnableAxisAnimation = true,
    
    // Animate ranges
    EnableRangeAnimation = true,
    
    // Global animation duration
    AnimationDuration = 1500
};

// Range
fullyAnimated.Ranges.Add(new LinearRange
{
    StartValue = 0,
    EndValue = 100,
    Fill = new SolidColorBrush(Color.FromArgb("#E0E0E0"))
});

// Animated bar pointer
fullyAnimated.BarPointers.Add(new BarPointer
{
    Value = 70,
    EnableAnimation = true,
    AnimationDuration = 1500,
    AnimationEasing = Easing.SinOut,
    Fill = new SolidColorBrush(Color.FromArgb("#2196F3"))
});

// Animated marker pointer
fullyAnimated.MarkerPointers.Add(new LinearShapePointer
{
    Value = 70,
    EnableAnimation = true,
    AnimationDuration = 1500,
    AnimationEasing = Easing.BounceOut,
    ShapeType = LinearShapeType.Circle,
    Fill = new SolidColorBrush(Color.FromArgb("#1976D2")),
    ShapeHeight = 25,
    ShapeWidth = 25
});
```

### Example 2: Staggered Animations

```csharp
SfLinearGauge staggered = new SfLinearGauge
{
    EnableAxisAnimation = true,
    AnimationDuration = 800
};

// Bar pointer: fast animation
staggered.BarPointers.Add(new BarPointer
{
    Value = 60,
    EnableAnimation = true,
    AnimationDuration = 600,
    AnimationEasing = Easing.SinOut
});

// Marker pointer: delayed, slower animation
Task.Delay(400).ContinueWith(_ =>
{
    MainThread.BeginInvokeOnMainThread(() =>
    {
        staggered.MarkerPointers.Add(new LinearShapePointer
        {
            Value = 60,
            EnableAnimation = true,
            AnimationDuration = 1000,
            AnimationEasing = Easing.BounceOut,
            ShapeType = LinearShapeType.Circle,
            ShapeHeight = 20,
            ShapeWidth = 20
        });
    });
});
```

### Example 3: Progress Bar with Smooth Update

```csharp
BarPointer progressPointer = new BarPointer
{
    Value = 0,
    EnableAnimation = true,
    AnimationDuration = 800,
    AnimationEasing = Easing.SinOut,
    PointerSize = 20,
    CornerStyle = CornerStyle.BothCurve,
    Fill = new SolidColorBrush(Color.FromArgb("#4CAF50"))
};

gauge.BarPointers.Add(progressPointer);

// Update progress smoothly
public async Task UpdateProgress(int targetPercent)
{
    // Each update animates smoothly
    progressPointer.Value = targetPercent;
    await Task.Delay(progressPointer.AnimationDuration);
}

// Simulate progress
await UpdateProgress(25);
await UpdateProgress(50);
await UpdateProgress(75);
await UpdateProgress(100);
```

### Example 4: Interactive Slider with Animation

```csharp
LinearShapePointer interactivePointer = new LinearShapePointer
{
    Value = 50,
    IsInteractive = true,
    EnableAnimation = true,
    AnimationDuration = 300,  // Fast animation for responsiveness
    AnimationEasing = Easing.SinOut,
    ShapeType = LinearShapeType.Circle,
    Fill = new SolidColorBrush(Colors.Blue),
    ShapeHeight = 30,
    ShapeWidth = 30
};

// Snap to nearest 10 with animation
interactivePointer.ValueChangeCompleted += (s, e) =>
{
    double snapped = Math.Round(e.Value / 10) * 10;
    if (Math.Abs(snapped - e.Value) > 0.1)
    {
        interactivePointer.Value = snapped;  // Animates to snapped value
    }
};
```

### Example 5: Thermometer with Rising Mercury

```csharp
SfLinearGauge thermometer = new SfLinearGauge
{
    Minimum = -20,
    Maximum = 50,
    Orientation = GaugeOrientation.Vertical,
    HeightRequest = 400,
    EnableAxisAnimation = true,
    AnimationDuration = 1000
};

thermometer.LineStyle = new LinearLineStyle
{
    Thickness = 15,
    Fill = new SolidColorBrush(Colors.LightGray),
    CornerStyle = CornerStyle.BothCurve
};

BarPointer mercury = new BarPointer
{
    Value = -10,
    EnableAnimation = true,
    AnimationDuration = 2000,
    AnimationEasing = Easing.SinInOut,
    PointerSize = 15,
    Fill = new SolidColorBrush(Colors.Red),
    CornerStyle = CornerStyle.BothCurve
};

thermometer.BarPointers.Add(mercury);

// Simulate temperature rising
public async Task SimulateTemperatureRise()
{
    for (int temp = -10; temp <= 25; temp += 5)
    {
        mercury.Value = temp;
        await Task.Delay(2200);  // Wait for animation + buffer
    }
}
```

### Example 6: Mirrored Dual Gauge Comparison

```xml
<HorizontalStackLayout Spacing="20" Padding="20">
    
    <!-- Left gauge (Person A) -->
    <VerticalStackLayout>
        <Label Text="Person A" HorizontalOptions="Center"/>
        <gauge:SfLinearGauge IsMirrored="False" 
                            EnableAxisAnimation="True"
                            WidthRequest="200">
            <gauge:SfLinearGauge.BarPointers>
                <gauge:BarPointer Value="75" 
                                 EnableAnimation="True"
                                 AnimationDuration="1200"
                                 Fill="#2196F3"/>
            </gauge:SfLinearGauge.BarPointers>
        </gauge:SfLinearGauge>
    </VerticalStackLayout>
    
    <!-- Right gauge (Person B, mirrored) -->
    <VerticalStackLayout>
        <Label Text="Person B" HorizontalOptions="Center"/>
        <gauge:SfLinearGauge IsMirrored="True" 
                            EnableAxisAnimation="True"
                            WidthRequest="200">
            <gauge:SfLinearGauge.BarPointers>
                <gauge:BarPointer Value="85" 
                                 EnableAnimation="True"
                                 AnimationDuration="1200"
                                 Fill="#FF5722"/>
            </gauge:SfLinearGauge.BarPointers>
        </gauge:SfLinearGauge>
    </VerticalStackLayout>
    
</HorizontalStackLayout>
```

**Animation Best Practices:**
- Use animations for initial load and value changes
- Keep durations under 2 seconds for most cases
- Choose easing appropriate to context (bounce for fun, sinOut for professional)
- Disable animations for rapid, frequent updates
- Consider performance on low-end devices
- Provide option to disable animations for accessibility
