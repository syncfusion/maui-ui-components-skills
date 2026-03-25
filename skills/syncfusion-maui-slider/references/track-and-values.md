# Track and Value Configuration

This guide covers track configuration, value management, and track styling for the .NET MAUI Slider.

## Table of Contents
- [Minimum and Maximum](#minimum-and-maximum)
- [Value Property](#value-property)
- [Track Customization](#track-customization)
- [Track Colors](#track-colors)
- [Track Size](#track-size)
- [Complete Examples](#complete-examples)

## Minimum and Maximum

The `Minimum` and `Maximum` properties define the range of values the slider can represent.

### Minimum Property

The minimum value that the user can select. Default value is `0`.

```xml
<sliders:SfSlider Minimum="20" />
```

```csharp
SfSlider slider = new SfSlider
{
    Minimum = 20
};
```

**Important**: Minimum must be less than Maximum. Setting an invalid range will cause unexpected behavior.

### Maximum Property

The maximum value that the user can select. Default value is `1`.

```xml
<sliders:SfSlider Maximum="100" />
```

```csharp
SfSlider slider = new SfSlider
{
    Maximum = 100
};
```

### Setting Range

Always set both Minimum and Maximum together:

```xml
<sliders:SfSlider Minimum="0"
                  Maximum="100"
                  Interval="10"
                  ShowLabels="True"
                  ShowTicks="True" />
```

```csharp
SfSlider slider = new SfSlider
{
    Minimum = 0,
    Maximum = 100,
    Interval = 10,
    ShowLabels = true,
    ShowTicks = true
};
```

**Use Cases:**
- **0-100**: Percentage values (volume, brightness)
- **0-1**: Normalized values (opacity, probability)
- **Custom ranges**: Temperature (10-30°C), price ($0-$1000), age (18-65)

## Value Property

The `Value` property represents the currently selected value. The slider thumb is drawn at the position corresponding to this value.

### Setting Initial Value

```xml
<sliders:SfSlider Minimum="0"
                  Maximum="100"
                  Value="50" />
```

```csharp
SfSlider slider = new SfSlider
{
    Minimum = 0,
    Maximum = 100,
    Value = 50  // Initial value
};
```

### Data Binding

Bind the Value property to a ViewModel property:

**XAML:**
```xml
<ContentPage.BindingContext>
    <local:ViewModel />
</ContentPage.BindingContext>

<sliders:SfSlider Minimum="0"
                  Maximum="100"
                  Value="{Binding Volume, Mode=TwoWay}" />
```

**ViewModel:**
```csharp
using System.ComponentModel;
using System.Runtime.CompilerServices;

public class ViewModel : INotifyPropertyChanged
{
    private double _volume = 50;
    
    public double Volume
    {
        get => _volume;
        set
        {
            if (_volume != value)
            {
                _volume = value;
                OnPropertyChanged();
            }
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**Important**: Use `Mode=TwoWay` for bidirectional value updates.

### Programmatic Value Updates

Update the value programmatically:

```csharp
// Direct assignment
slider.Value = 75;

// Animate to value (smooth transition)
await AnimateValueAsync(slider, targetValue: 75, duration: 300);

private async Task AnimateValueAsync(SfSlider slider, double targetValue, uint duration)
{
    var animation = new Animation(v => slider.Value = v, slider.Value, targetValue);
    animation.Commit(slider, "ValueAnimation", length: duration, easing: Easing.CubicInOut);
    await Task.Delay((int)duration);
}
```

### Value Constraints

The Value is automatically constrained between Minimum and Maximum:

```csharp
SfSlider slider = new SfSlider
{
    Minimum = 0,
    Maximum = 100,
    Value = 150  // Will be clamped to 100
};

// Value will be 100, not 150
Console.WriteLine(slider.Value);  // Output: 100
```

## Track Customization

The track is the horizontal or vertical line along which the thumb moves. It consists of two parts:

- **Active Track**: The portion from Minimum to the current Value (typically colored)
- **Inactive Track**: The portion from the current Value to Maximum (typically gray)

### TrackStyle Property

Use `SliderTrackStyle` to customize the track appearance:

```xml
<sliders:SfSlider Minimum="0" Maximum="100" Value="60">
    <sliders:SfSlider.TrackStyle>
        <sliders:SliderTrackStyle ActiveFill="#EE3F3F"
                                  InactiveFill="#88EE3F3F"
                                  ActiveSize="8"
                                  InactiveSize="4" />
    </sliders:SfSlider.TrackStyle>
</sliders:SfSlider>
```

```csharp
SfSlider slider = new SfSlider
{
    Minimum = 0,
    Maximum = 100,
    Value = 60
};

slider.TrackStyle.ActiveFill = new SolidColorBrush(Color.FromArgb("#EE3F3F"));
slider.TrackStyle.InactiveFill = new SolidColorBrush(Color.FromArgb("#88EE3F3F"));
slider.TrackStyle.ActiveSize = 8;
slider.TrackStyle.InactiveSize = 4;
```

## Track Colors

### ActiveFill Property

Color of the track from Minimum to current Value:

```xml
<sliders:SfSlider>
    <sliders:SfSlider.TrackStyle>
        <sliders:SliderTrackStyle ActiveFill="#FF6B6B" />
    </sliders:SfSlider.TrackStyle>
</sliders:SfSlider>
```

```csharp
slider.TrackStyle.ActiveFill = new SolidColorBrush(Color.FromArgb("#FF6B6B"));
// Or use named colors
slider.TrackStyle.ActiveFill = new SolidColorBrush(Colors.CornflowerBlue);
```

### InactiveFill Property

Color of the track from current Value to Maximum:

```xml
<sliders:SfSlider>
    <sliders:SfSlider.TrackStyle>
        <sliders:SliderTrackStyle InactiveFill="#E0E0E0" />
    </sliders:SfSlider.TrackStyle>
</sliders:SfSlider>
```

```csharp
slider.TrackStyle.InactiveFill = new SolidColorBrush(Color.FromArgb("#E0E0E0"));
// Or with transparency
slider.TrackStyle.InactiveFill = new SolidColorBrush(Colors.Gray.WithAlpha(0.3f));
```

### Color Combinations

**Example 1: High Contrast**
```xml
<sliders:SliderTrackStyle ActiveFill="#FF4081"
                          InactiveFill="#BDBDBD" />
```

**Example 2: Monochrome with Transparency**
```xml
<sliders:SliderTrackStyle ActiveFill="#2196F3"
                          InactiveFill="#882196F3" />
```
The inactive track uses the same color with 50% transparency (88 in hex).

**Example 3: Gradient Effect (Manual)**
```csharp
// Use gradient brushes for advanced effects
var gradient = new LinearGradientBrush
{
    StartPoint = new Point(0, 0),
    EndPoint = new Point(1, 0),
    GradientStops = new GradientStopCollection
    {
        new GradientStop { Color = Colors.Blue, Offset = 0.0f },
        new GradientStop { Color = Colors.Purple, Offset = 1.0f }
    }
};
slider.TrackStyle.ActiveFill = gradient;
```

## Track Size

### ActiveSize Property

Height (horizontal slider) or width (vertical slider) of the active track:

```xml
<sliders:SfSlider>
    <sliders:SfSlider.TrackStyle>
        <sliders:SliderTrackStyle ActiveSize="10" />
    </sliders:SfSlider.TrackStyle>
</sliders:SfSlider>
```

```csharp
slider.TrackStyle.ActiveSize = 10;
```

### InactiveSize Property

Height (horizontal slider) or width (vertical slider) of the inactive track:

```xml
<sliders:SfSlider>
    <sliders:SfSlider.TrackStyle>
        <sliders:SliderTrackStyle InactiveSize="4" />
    </sliders:SfSlider.TrackStyle>
</sliders:SfSlider>
```

```csharp
slider.TrackStyle.InactiveSize = 4;
```

### Size Variations

**Example 1: Thick Active Track**
```xml
<sliders:SliderTrackStyle ActiveSize="12"
                          InactiveSize="4" />
```
Creates emphasis on the selected portion.

**Example 2: Uniform Track**
```xml
<sliders:SliderTrackStyle ActiveSize="6"
                          InactiveSize="6" />
```
Both portions have the same thickness.

**Example 3: Subtle Active Track**
```xml
<sliders:SliderTrackStyle ActiveSize="4"
                          InactiveSize="8" />
```
Inactive track is thicker (uncommon, but useful for certain designs).

## Complete Examples

### Example 1: Volume Slider

```xml
<VerticalStackLayout Padding="20" Spacing="10">
    <Label Text="Volume" FontSize="16" />
    
    <sliders:SfSlider Minimum="0"
                      Maximum="100"
                      Value="75"
                      Interval="25"
                      ShowLabels="True"
                      ShowTicks="True">
        <sliders:SfSlider.TrackStyle>
            <sliders:SliderTrackStyle ActiveFill="#4CAF50"
                                      InactiveFill="#88000000"
                                      ActiveSize="8"
                                      InactiveSize="4" />
        </sliders:SfSlider.TrackStyle>
    </sliders:SfSlider>
</VerticalStackLayout>
```

### Example 2: Temperature Control

```xml
<sliders:SfSlider Minimum="10"
                  Maximum="30"
                  Value="22"
                  NumberFormat="0'°C'"
                  ShowLabels="True"
                  Interval="5">
    <sliders:SfSlider.TrackStyle>
        <sliders:SliderTrackStyle ActiveFill="#FF6B35"
                                  InactiveFill="#C7C7C7"
                                  ActiveSize="6"
                                  InactiveSize="6" />
    </sliders:SfSlider.TrackStyle>
</sliders:SfSlider>
```

### Example 3: Price Range

```csharp
public class PriceSlider : ContentView
{
    public PriceSlider()
    {
        var slider = new SfSlider
        {
            Minimum = 0,
            Maximum = 1000,
            Value = 500,
            Interval = 200,
            NumberFormat = "$#",
            ShowLabels = true,
            ShowDividers = true
        };
        
        slider.TrackStyle.ActiveFill = new SolidColorBrush(Color.FromArgb("#9C27B0"));
        slider.TrackStyle.InactiveFill = new SolidColorBrush(Color.FromArgb("#88000000"));
        slider.TrackStyle.ActiveSize = 8;
        slider.TrackStyle.InactiveSize = 4;
        
        Content = slider;
    }
}
```

### Example 4: Vertical Slider with Custom Track

```xml
<sliders:SfSlider Orientation="Vertical"
                  Minimum="0"
                  Maximum="100"
                  Value="60"
                  ShowTicks="True"
                  HeightRequest="300">
    <sliders:SfSlider.TrackStyle>
        <sliders:SliderTrackStyle ActiveFill="#03A9F4"
                                  InactiveFill="#E0E0E0"
                                  ActiveSize="10"
                                  InactiveSize="10" />
    </sliders:SfSlider.TrackStyle>
</sliders:SfSlider>
```

## Best Practices

### Track Sizing
- **Standard**: ActiveSize = 6-8, InactiveSize = 4-6
- **Bold**: ActiveSize = 10-12, InactiveSize = 4-6
- **Subtle**: ActiveSize = 4-6, InactiveSize = 4-6

### Color Selection
- Use brand colors for ActiveFill
- Use gray or transparent for InactiveFill
- Ensure sufficient contrast between active and inactive
- Consider dark mode: provide alternative colors

### Performance
- Avoid complex gradient brushes if creating many sliders
- Use solid colors for better performance
- Cache brush instances if reusing across multiple sliders

## Troubleshooting

### Issue: Track Not Visible

**Cause**: Fill color matches background  
**Solution**: Use contrasting colors for ActiveFill and InactiveFill

### Issue: Track Too Thin/Thick

**Cause**: ActiveSize or InactiveSize set incorrectly  
**Solution**: Use values between 4-12 for optimal appearance

### Issue: Track Colors Don't Change

**Cause**: TrackStyle not properly instantiated  
**Solution**: Ensure TrackStyle is created:
```xml
<sliders:SfSlider.TrackStyle>
    <sliders:SliderTrackStyle ActiveFill="Red" />
</sliders:SfSlider.TrackStyle>
```

## Summary

Key track configuration properties:

- **Minimum/Maximum**: Define value range
- **Value**: Current selected value (supports binding)
- **ActiveFill**: Color from Minimum to Value
- **InactiveFill**: Color from Value to Maximum
- **ActiveSize**: Thickness of active track
- **InactiveSize**: Thickness of inactive track

Use TrackStyle for visual customization, and bind Value for dynamic updates.
