# Animation and Interaction

## Table of Contents
- [Overview](#overview)
- [Pointer Animation](#pointer-animation)
  - [Enabling Animation](#enabling-animation)
  - [Animation Duration](#animation-duration)
  - [Easing Functions](#easing-functions)
  - [Animation on Load vs Value Change](#animation-on-load-vs-value-change)
- [Initial Loading Animation](#initial-loading-animation)
- [Pointer Interaction](#pointer-interaction)
  - [Making Pointers Draggable](#making-pointers-draggable)
  - [ValueChanged Event](#valuechanged-event)
  - [ValueChanging Event](#valuechanging-event)
  - [Handling User Input](#handling-user-input)
- [Animation Examples](#animation-examples)
  - [Smooth Transitions](#smooth-transitions)
  - [Spring Effects](#spring-effects)
  - [Custom Easing](#custom-easing)
- [Interactive Dashboard Examples](#interactive-dashboard-examples)
- [Combining Animation with Interaction](#combining-animation-with-interaction)
- [Performance Considerations](#performance-considerations)
- [Best Practices](#best-practices)

## Overview

Animations and interactions bring gauges to life, creating engaging user experiences. Syncfusion .NET MAUI Radial Gauge supports:

**Animation:**
- Smooth pointer transitions when values change
- Initial loading animations for gauge elements
- Customizable duration and easing functions
- Automatic animation on data updates

**Interaction:**
- Draggable pointers for user input
- Value change events
- Touch/swipe gestures
- Real-time value updates

## Pointer Animation

### Enabling Animation

Enable animation for smooth pointer movement:

```xaml
<gauge:NeedlePointer Value="60"
                     EnableAnimation="True" />
```

```csharp
NeedlePointer pointer = new NeedlePointer
{
    Value = 60,
    EnableAnimation = true
};
```

**Default:** `false`

**Effect:** When `Value` changes, the pointer smoothly animates to the new position instead of jumping immediately.

**Works With:**
- Needle Pointer
- Shape Pointer
- Content Pointer
- Range Pointer

### Animation Duration

Control animation speed with `AnimationDuration` (in milliseconds):

```xaml
<gauge:NeedlePointer Value="60"
                     EnableAnimation="True"
                     AnimationDuration="1500" />
```

```csharp
pointer.EnableAnimation = true;
pointer.AnimationDuration = 1500;  // 1.5 seconds
```

**Default:** 1000ms (1 second)

**Guidelines:**
- **500-1000ms:** Quick, responsive (real-time data)
- **1000-2000ms:** Smooth, noticeable (user interactions)
- **2000+ms:** Dramatic, emphasized (initial load, major changes)

### Easing Functions

Control animation feel with `AnimationEasing`:

```xaml
<gauge:NeedlePointer Value="60"
                     EnableAnimation="True"
                     AnimationDuration="1200"
                     AnimationEasing="{x:Static Easing.SpringOut}" />
```

```csharp
pointer.AnimationEasing = Easing.SpringOut;
```

**Available Easing Functions:**

| Easing | Effect | Best For |
|--------|--------|----------|
| `Linear` | Constant speed | Simple transitions |
| `SinIn` / `SinOut` / `SinInOut` | Gentle acceleration/deceleration | Smooth, natural feel |
| `CubicIn` / `CubicOut` / `CubicInOut` | Strong acceleration/deceleration | Emphasized movement |
| `BounceIn` / `BounceOut` | Bouncing effect | Playful, attention-getting |
| `SpringIn` / `SpringOut` | Spring/elastic effect | Natural, physical feel |

**Examples:**

```csharp
// Smooth start and end
pointer.AnimationEasing = Easing.CubicInOut;

// Bouncy arrival
pointer.AnimationEasing = Easing.BounceOut;

// Elastic spring effect
pointer.AnimationEasing = Easing.SpringOut;

// Linear (no easing)
pointer.AnimationEasing = Easing.Linear;
```

### Animation on Load vs Value Change

**Initial Display (Page Load):**
```xaml
<gauge:NeedlePointer Value="60"
                     EnableAnimation="True"
                     AnimationDuration="1500"
                     AnimationEasing="{x:Static Easing.SpringOut}" />
```

When the page loads, the pointer animates from 0 to 60.

**Value Updates:**
```csharp
// Pointer animates from current value to new value
pointer.Value = 85;
```

**Disabling Initial Animation (Start at Value):**
```csharp
// Set value before adding pointer to axis (no animation)
pointer.Value = 60;
pointer.EnableAnimation = false;
axis.Pointers.Add(pointer);

// Enable animation for subsequent changes
pointer.EnableAnimation = true;
```

## Initial Loading Animation

Animate the entire gauge (axis, ranges, pointers) on initial load:

```xaml
<gauge:RadialAxis EnableLoadingAnimation="True"
                  AnimationDuration="2000">
    <gauge:RadialAxis.Ranges>
        <gauge:RadialRange StartValue="0" EndValue="50" Fill="Green" />
        <gauge:RadialRange StartValue="50" EndValue="100" Fill="Orange" />
        <gauge:RadialRange StartValue="100" EndValue="150" Fill="Red" />
    </gauge:RadialAxis.Ranges>
    
    <gauge:RadialAxis.Pointers>
        <gauge:NeedlePointer Value="90" />
    </gauge:RadialAxis.Pointers>
</gauge:RadialAxis>
```

```csharp
RadialAxis axis = new RadialAxis
{
    EnableLoadingAnimation = true,
    AnimationDuration = 2000
};
```

**Effect:** All gauge elements (axis line, labels, ticks, ranges, pointers) animate into view.

**Use Cases:**
- Splash screens
- Dashboard reveals
- Emphasis on gauge importance
- Creating visual interest

## Pointer Interaction

### Making Pointers Draggable

Enable user interaction with `IsInteractive`:

```xaml
<gauge:NeedlePointer Value="60"
                     IsInteractive="True" />
```

```csharp
pointer.IsInteractive = true;
```

**Effect:** Users can drag the pointer around the axis to change its value.

**Supported Gestures:**
- Touch and drag
- Swipe
- Tap to move pointer

**Works With All Pointer Types:**
```xaml
<!-- Needle -->
<gauge:NeedlePointer IsInteractive="True" />

<!-- Shape -->
<gauge:ShapePointer IsInteractive="True" />

<!-- Content -->
<gauge:ContentPointer IsInteractive="True" />

<!-- Range -->
<gauge:RangePointer IsInteractive="True" />
```

### ValueChanged Event

Fires when pointer value changes (after user stops dragging or programmatic update):

```xaml
<gauge:NeedlePointer Value="60"
                     IsInteractive="True"
                     ValueChanged="OnPointerValueChanged" />
```

```csharp
private void OnPointerValueChanged(object sender, ValueChangedEventArgs e)
{
    double oldValue = e.OldValue;
    double newValue = e.Value;
    
    // Update UI, save to database, etc.
    DisplayLabel.Text = $"New value: {newValue:F1}";
}
```

**Event Args:**
- `OldValue` - Previous value
- `Value` - New value

**When It Fires:**
- After user drags and releases pointer
- After programmatic value change

### ValueChanging Event

Fires continuously while pointer is being dragged:

```xaml
<gauge:NeedlePointer Value="60"
                     IsInteractive="True"
                     ValueChanging="OnPointerValueChanging" />
```

```csharp
private void OnPointerValueChanging(object sender, ValueChangingEventArgs e)
{
    double currentValue = e.NewValue;
    
    // Real-time updates
    LiveLabel.Text = $"Current: {currentValue:F1}";
    
    // Cancel change if needed
    if (currentValue > 90)
    {
        e.Cancel = true;  // Prevent values above 90
    }
}
```

**Event Args:**
- `OldValue` - Previous value
- `NewValue` - Current value during drag
- `Cancel` - Set to `true` to prevent change

**Use Cases:**
- Real-time value display
- Live validation
- Restricting value ranges
- Haptic feedback triggers

### Handling User Input

**Complete Interactive Example:**

```xaml
<gauge:SfRadialGauge>
    <gauge:SfRadialGauge.Axes>
        <gauge:RadialAxis Minimum="0"
                          Maximum="100"
                          Interval="10">
            
            <!-- Background range -->
            <gauge:RadialAxis.Ranges>
                <gauge:RadialRange StartValue="0"
                                   EndValue="100"
                                   Fill="LightGray"
                                   StartWidth="20"
                                   EndWidth="20" />
            </gauge:RadialAxis.Ranges>
            
            <!-- Interactive pointer -->
            <gauge:RadialAxis.Pointers>
                <gauge:NeedlePointer x:Name="InteractivePointer"
                                     Value="50"
                                     IsInteractive="True"
                                     EnableAnimation="True"
                                     AnimationDuration="300"
                                     ValueChanging="OnValueChanging"
                                     ValueChanged="OnValueChanged" />
            </gauge:RadialAxis.Pointers>
            
            <!-- Value annotation -->
            <gauge:RadialAxis.Annotations>
                <gauge:GaugeAnnotation PositionFactor="0">
                    <gauge:GaugeAnnotation.Content>
                        <Label x:Name="ValueLabel"
                               Text="50"
                               FontSize="32"
                               FontAttributes="Bold" />
                    </gauge:GaugeAnnotation.Content>
                </gauge:GaugeAnnotation>
            </gauge:RadialAxis.Annotations>
            
        </gauge:RadialAxis>
    </gauge:SfRadialGauge.Axes>
</gauge:SfRadialGauge>
```

```csharp
private void OnValueChanging(object sender, ValueChangingEventArgs e)
{
    // Update label in real-time
    ValueLabel.Text = e.Value.ToString("F0");
}

private void OnValueChanged(object sender, ValueChangedEventArgs e)
{
    // Final value
    Debug.WriteLine($"Value changed from {e.OldValue} to {e.Value}");
    
    // Save to settings, database, etc.
    SaveVolumeSetting(e.Value);
}
```

## Animation Examples

### Smooth Transitions

**Real-time data updates:**
```csharp
// In your data update method
public void UpdateGaugeValue(double newValue)
{
    // Pointer smoothly animates to new value
    speedPointer.Value = newValue;
}
```

### Spring Effects

**Bouncy, playful animation:**
```xaml
<gauge:NeedlePointer Value="75"
                     EnableAnimation="True"
                     AnimationDuration="1200"
                     AnimationEasing="{x:Static Easing.SpringOut}" />
```

```csharp
pointer.EnableAnimation = true;
pointer.AnimationDuration = 1200;
pointer.AnimationEasing = Easing.SpringOut;

// Update value - watch it spring!
pointer.Value = 85;
```

### Custom Easing

**Create custom animation curves:**
```csharp
// Custom ease-out curve
pointer.AnimationEasing = new Easing(t => 
{
    return 1 - Math.Pow(1 - t, 3);  // Cubic ease-out
});
```

**Stepped animation:**
```csharp
// Step animation (no smoothing)
pointer.EnableAnimation = false;
pointer.Value = targetValue;
```

## Interactive Dashboard Examples

### Volume Control

```xaml
<gauge:SfRadialGauge>
    <gauge:SfRadialGauge.Axes>
        <gauge:RadialAxis Minimum="0"
                          Maximum="100"
                          StartAngle="180"
                          EndAngle="0"
                          ShowLabels="False">
            
            <gauge:RadialAxis.AxisLineStyle>
                <gauge:RadialLineStyle Thickness="30" Fill="LightGray" />
            </gauge:RadialAxis.AxisLineStyle>
            
            <gauge:RadialAxis.Pointers>
                <!-- Volume indicator -->
                <gauge:RangePointer Value="65"
                                    IsInteractive="True"
                                    PointerWidth="30"
                                    Fill="CornflowerBlue"
                                    ValueChanged="OnVolumeChanged" />
                
                <!-- Drag handle -->
                <gauge:ShapePointer Value="65"
                                    IsInteractive="True"
                                    ShapeType="Circle"
                                    ShapeHeight="35"
                                    ShapeWidth="35"
                                    Fill="White"
                                    Stroke="CornflowerBlue"
                                    BorderWidth="3"
                                    HasShadow="True"
                                    ValueChanged="OnVolumeChanged" />
            </gauge:RadialAxis.Pointers>
            
            <gauge:RadialAxis.Annotations>
                <gauge:GaugeAnnotation PositionFactor="0">
                    <gauge:GaugeAnnotation.Content>
                        <VerticalStackLayout>
                            <Label x:Name="VolumeLabel"
                                   Text="65"
                                   FontSize="48"
                                   FontAttributes="Bold"
                                   HorizontalOptions="Center" />
                            <Label Text="Volume"
                                   FontSize="16"
                                   TextColor="Gray"
                                   HorizontalOptions="Center" />
                        </VerticalStackLayout>
                    </gauge:GaugeAnnotation.Content>
                </gauge:GaugeAnnotation>
            </gauge:RadialAxis.Annotations>
        </gauge:RadialAxis>
    </gauge:SfRadialGauge.Axes>
</gauge:SfRadialGauge>
```

```csharp
private void OnVolumeChanged(object sender, ValueChangedEventArgs e)
{
    int volume = (int)Math.Round(e.Value);
    VolumeLabel.Text = volume.ToString();
    
    // Update system volume
    SetSystemVolume(volume);
}
```

### Temperature Thermostat

```csharp
private void OnTemperatureChanging(object sender, ValueChangingEventArgs e)
{
    // Round to nearest 0.5 degree
    double rounded = Math.Round(e.Value * 2) / 2;
    
    // Update display in real-time
    TempLabel.Text = $"{rounded}°C";
}

private void OnTemperatureChanged(object sender, ValueChangedEventArgs e)
{
    double newTemp = Math.Round(e.Value * 2) / 2;
    
    // Send to thermostat
    await SetThermostatTemperature(newTemp);
}
```

## Combining Animation with Interaction

**Smooth interactive experience:**
```xaml
<gauge:NeedlePointer Value="50"
                     IsInteractive="True"
                     EnableAnimation="True"
                     AnimationDuration="300"
                     AnimationEasing="{x:Static Easing.CubicOut}"
                     ValueChanging="OnValueChanging"
                     ValueChanged="OnValueChanged" />
```

**Benefits:**
- Smooth visual feedback during drag
- Snap-to-value animation after release
- Natural, responsive feel

**Advanced: Snap to Increments**
```csharp
private void OnValueChanged(object sender, ValueChangedEventArgs e)
{
    // Snap to nearest 10
    double snapped = Math.Round(e.Value / 10) * 10;
    
    if (Math.Abs(snapped - e.Value) > 0.1)
    {
        // Animate to snapped value
        (sender as RadialPointer).Value = snapped;
    }
}
```

## Performance Considerations

**Animation Performance:**
- Animations are hardware-accelerated
- Multiple animated pointers have minimal overhead
- Smooth on all device types

**Optimization Tips:**
1. **Reduce Duration for Real-Time:** Use 100-300ms for live data updates
2. **Disable When Hidden:** Set `EnableAnimation=false` if gauge isn't visible
3. **Throttle Updates:** Don't update pointer values more than 60 times/second
4. **Use Appropriate Easing:** Simple easing (Linear, SinOut) performs best

**Interaction Performance:**
- Touch handling is optimized
- ValueChanging event fires at ~60fps
- No performance impact from IsInteractive when not in use

## Best Practices

### Animation

1. **Enable by Default:** Most gauges benefit from smooth transitions
2. **Match Duration to Context:**
   - Fast (300ms): Real-time data
   - Medium (1000ms): User interactions
   - Slow (2000ms): Initial reveals
3. **Choose Appropriate Easing:**
   - `CubicOut` or `SinOut`: General use
   - `SpringOut`: Fun, playful
   - `Linear`: Simple, straightforward
4. **Test on Target Devices:** Ensure animations feel smooth

### Interaction

1. **Provide Visual Feedback:** Use shadows, borders on draggable pointers
2. **Show Current Value:** Update annotations in real-time
3. **Consider Snap Behavior:** Round to logical increments
4. **Handle Edge Cases:** Validate min/max values
5. **Responsive Animation:** Keep dragging smooth (short animation duration)
6. **Accessibility:** Ensure keyboard/alternative input methods work

### Combined

1. **Smooth Dragging:** Use `EnableAnimation=true` with short duration (200-300ms)
2. **Snap After Release:** Animate to final value in `ValueChanged` event
3. **Real-Time Display:** Update annotations in `ValueChanging` event
4. **Provide Feedback:** Visual, audio, or haptic feedback during interaction

**Common Patterns:**
```csharp
// Pattern: Smooth interaction with snap
pointer.EnableAnimation = true;
pointer.AnimationDuration = 250;
pointer.IsInteractive = true;

private void OnValueChanged(object sender, ValueChangedEventArgs e)
{
    // Snap to integer
    int snapped = (int)Math.Round(e.Value);
    if (snapped != (int)e.Value)
    {
        (sender as RadialPointer).Value = snapped;
    }
}
```

## Summary

**Animation:**
- Enable with `EnableAnimation=true`
- Control speed with `AnimationDuration` (ms)
- Customize feel with `AnimationEasing` functions
- Applies to all pointer types

**Interaction:**
- Enable with `IsInteractive=true`
- Handle `ValueChanging` for real-time updates
- Handle `ValueChanged` for final values
- Cancel changes with `e.Cancel = true`

**Best Results:**
- Combine animation + interaction for smooth UX
- Use short durations for dragging (200-300ms)
- Snap to logical increments
- Provide visual feedback
- Update annotations in real-time

Create engaging, responsive gauges that users love to interact with!
