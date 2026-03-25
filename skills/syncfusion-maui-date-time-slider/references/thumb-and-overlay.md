# Thumb and Overlay in .NET MAUI DateTime Slider

This guide covers customization of the thumb (draggable element) and its overlay (interactive feedback circle) in the DateTime Slider.

## Overview

### Thumb
The thumb is the draggable circular element that represents the current value on the slider track. Users interact with it to select different date/time values.

### Thumb Overlay
The overlay is a circular highlight that appears around the thumb during interaction (tap, drag, hover), providing visual feedback.

## Thumb Customization

### Thumb Size

Control thumb size using the `Radius` property:

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2018-01-01"
                          Value="2014-01-01">
    <sliders:SfDateTimeSlider.ThumbStyle>
        <sliders:SliderThumbStyle Radius="15" />
    </sliders:SfDateTimeSlider.ThumbStyle>
</sliders:SfDateTimeSlider>
```

**C# Implementation:**

```csharp
var slider = new SfDateTimeSlider
{
    Minimum = new DateTime(2010, 01, 01),
    Maximum = new DateTime(2018, 01, 01),
    Value = new DateTime(2014, 01, 01)
};

slider.ThumbStyle.Radius = 15;
```

**Default:** `10.0`

### Thumb Color

Customize thumb fill color:

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2018-01-01"
                          Value="2014-01-01">
    <sliders:SfDateTimeSlider.ThumbStyle>
        <sliders:SliderThumbStyle Fill="#EE3F3F" />
    </sliders:SfDateTimeSlider.ThumbStyle>
</sliders:SfDateTimeSlider>
```

**C# Implementation:**

```csharp
slider.ThumbStyle.Fill = new SolidColorBrush(Color.FromArgb("#EE3F3F"));
```

### Thumb Stroke (Border)

Add a border to the thumb:

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2018-01-01"
                          Value="2014-01-01">
    <sliders:SfDateTimeSlider.ThumbStyle>
        <sliders:SliderThumbStyle Fill="White"
                                  Stroke="#EE3F3F"
                                  StrokeThickness="2" />
    </sliders:SfDateTimeSlider.ThumbStyle>
</sliders:SfDateTimeSlider>
```

**C# Implementation:**

```csharp
slider.ThumbStyle.Fill = new SolidColorBrush(Colors.White);
slider.ThumbStyle.Stroke = new SolidColorBrush(Color.FromArgb("#EE3F3F"));
slider.ThumbStyle.StrokeThickness = 2;
```

**Default:** 
- `Stroke`: Transparent
- `StrokeThickness`: 0

### Complete Thumb Styling

```csharp
slider.ThumbStyle.Radius = 12;
slider.ThumbStyle.Fill = new SolidColorBrush(Colors.Blue);
slider.ThumbStyle.Stroke = new SolidColorBrush(Colors.White);
slider.ThumbStyle.StrokeThickness = 3;
```

## Thumb Overlay Customization

The overlay appears as a semi-transparent circle around the thumb during interaction.

### Overlay Size

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2018-01-01"
                          Value="2014-01-01">
    <sliders:SfDateTimeSlider.ThumbOverlayStyle>
        <sliders:SliderThumbOverlayStyle Radius="18" />
    </sliders:SfDateTimeSlider.ThumbOverlayStyle>
</sliders:SfDateTimeSlider>
```

**C# Implementation:**

```csharp
slider.ThumbOverlayStyle.Radius = 18;
```

**Default:** `24.0`

**Tip:** Overlay radius should be larger than thumb radius for proper visual feedback.

### Overlay Color

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2018-01-01"
                          Value="2014-01-01">
    <sliders:SfDateTimeSlider.ThumbOverlayStyle>
        <sliders:SliderThumbOverlayStyle Fill="#66FFD700" />
    </sliders:SfDateTimeSlider.ThumbOverlayStyle>
</sliders:SfDateTimeSlider>
```

**C# Implementation:**

```csharp
slider.ThumbOverlayStyle.Fill = new SolidColorBrush(Color.FromArgb("#66FFD700"));
```

**Tip:** Use semi-transparent colors (alpha channel) for best visual effect.

### Complete Overlay Styling

```csharp
slider.ThumbOverlayStyle.Radius = 20;
slider.ThumbOverlayStyle.Fill = new SolidColorBrush(Color.FromArgb("#44FF5722"));
```

## Combined Thumb and Overlay Example

```xaml
<sliders:SfDateTimeSlider Minimum="2020-01-01"
                          Maximum="2025-12-31"
                          Value="2023-06-15"
                          Interval="1"
                          IntervalType="Years"
                          ShowLabels="True"
                          ShowTicks="True">
    
    <!-- Thumb Configuration -->
    <sliders:SfDateTimeSlider.ThumbStyle>
        <sliders:SliderThumbStyle Radius="14"
                                  Fill="#2196F3"
                                  Stroke="White"
                                  StrokeThickness="3" />
    </sliders:SfDateTimeSlider.ThumbStyle>
    
    <!-- Overlay Configuration -->
    <sliders:SfDateTimeSlider.ThumbOverlayStyle>
        <sliders:SliderThumbOverlayStyle Radius="22"
                                         Fill="#332196F3" />
    </sliders:SfDateTimeSlider.ThumbOverlayStyle>
    
</sliders:SfDateTimeSlider>
```

**C# Equivalent:**

```csharp
var slider = new SfDateTimeSlider
{
    Minimum = new DateTime(2020, 01, 01),
    Maximum = new DateTime(2025, 12, 31),
    Value = new DateTime(2023, 06, 15),
    Interval = 1,
    IntervalType = SliderDateIntervalType.Years,
    ShowLabels = true,
    ShowTicks = true
};

// Thumb
slider.ThumbStyle.Radius = 14;
slider.ThumbStyle.Fill = new SolidColorBrush(Color.FromArgb("#2196F3"));
slider.ThumbStyle.Stroke = new SolidColorBrush(Colors.White);
slider.ThumbStyle.StrokeThickness = 3;

// Overlay
slider.ThumbOverlayStyle.Radius = 22;
slider.ThumbOverlayStyle.Fill = new SolidColorBrush(Color.FromArgb("#332196F3"));
```

## Disabled Thumb State

Customize thumb appearance when slider is disabled using Visual State Manager:

```xaml
<ContentPage.Resources>
    <Style TargetType="sliders:SfDateTimeSlider">
        <Setter Property="Minimum" Value="2010-01-01" />
        <Setter Property="Maximum" Value="2018-01-01" />
        <Setter Property="Value" Value="2014-01-01" />
        <Setter Property="Interval" Value="2" />
        <Setter Property="VisualStateManager.VisualStateGroups">
            <VisualStateGroupList>
                <VisualStateGroup>
                    <!-- Enabled State -->
                    <VisualState x:Name="Default">
                        <VisualState.Setters>
                            <Setter Property="ThumbStyle">
                                <Setter.Value>
                                    <sliders:SliderThumbStyle Radius="13"
                                                              Fill="Red"
                                                              Stroke="Yellow"
                                                              StrokeThickness="3" />
                                </Setter.Value>
                            </Setter>
                        </VisualState.Setters>
                    </VisualState>
                    
                    <!-- Disabled State -->
                    <VisualState x:Name="Disabled">
                        <VisualState.Setters>
                            <Setter Property="ThumbStyle">
                                <Setter.Value>
                                    <sliders:SliderThumbStyle Radius="13"
                                                              Fill="Gray"
                                                              Stroke="LightGray"
                                                              StrokeThickness="3" />
                                </Setter.Value>
                            </Setter>
                            <Setter Property="TrackStyle">
                                <Setter.Value>
                                    <sliders:SliderTrackStyle ActiveFill="Gray"
                                                              InactiveFill="LightGray" />
                                </Setter.Value>
                            </Setter>
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateGroupList>
        </Setter>
    </Style>
</ContentPage.Resources>

<VerticalStackLayout>
    <Label Text="Enabled" Padding="24,10" />
    <sliders:SfDateTimeSlider />
    
    <Label Text="Disabled" Padding="24,10" />
    <sliders:SfDateTimeSlider IsEnabled="False" />
</VerticalStackLayout>
```

**C# Implementation:**

```csharp
var visualStateGroupList = new VisualStateGroupList();
var commonStateGroup = new VisualStateGroup();

// Default State
var defaultState = new VisualState { Name = "Default" };
defaultState.Setters.Add(new Setter
{
    Property = SfDateTimeSlider.ThumbStyleProperty,
    Value = new SliderThumbStyle
    {
        Radius = 13,
        Fill = Colors.Red,
        Stroke = Colors.Yellow,
        StrokeThickness = 3
    }
});

// Disabled State
var disabledState = new VisualState { Name = "Disabled" };
disabledState.Setters.Add(new Setter
{
    Property = SfDateTimeSlider.ThumbStyleProperty,
    Value = new SliderThumbStyle
    {
        Radius = 13,
        Fill = Colors.Gray,
        Stroke = Colors.LightGray,
        StrokeThickness = 3
    }
});
disabledState.Setters.Add(new Setter
{
    Property = SfDateTimeSlider.TrackStyleProperty,
    Value = new SliderTrackStyle
    {
        ActiveFill = Colors.Gray,
        InactiveFill = Colors.LightGray
    }
});

commonStateGroup.States.Add(defaultState);
commonStateGroup.States.Add(disabledState);
visualStateGroupList.Add(commonStateGroup);

VisualStateManager.SetVisualStateGroups(slider, visualStateGroupList);
```

## Properties Reference

### ThumbStyle

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Radius` | double | 10.0 | Size of the thumb (radius in pixels) |
| `Fill` | Brush | Theme default | Fill color of the thumb |
| `Stroke` | Brush | Transparent | Border color of the thumb |
| `StrokeThickness` | double | 0.0 | Border width of the thumb |

### ThumbOverlayStyle

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Radius` | double | 24.0 | Size of the overlay (radius in pixels) |
| `Fill` | Brush | Theme default (semi-transparent) | Fill color of the overlay |

## Common Patterns

### Pattern 1: Material Design Style

```csharp
// Primary color thumb with ripple effect
slider.ThumbStyle.Radius = 12;
slider.ThumbStyle.Fill = new SolidColorBrush(Color.FromArgb("#2196F3"));
slider.ThumbStyle.Stroke = new SolidColorBrush(Colors.White);
slider.ThumbStyle.StrokeThickness = 2;

slider.ThumbOverlayStyle.Radius = 20;
slider.ThumbOverlayStyle.Fill = new SolidColorBrush(Color.FromArgb("#332196F3"));
```

### Pattern 2: Minimal Flat Design

```csharp
// Small, subtle thumb without border
slider.ThumbStyle.Radius = 8;
slider.ThumbStyle.Fill = new SolidColorBrush(Colors.DarkGray);
slider.ThumbStyle.Stroke = new SolidColorBrush(Colors.Transparent);

slider.ThumbOverlayStyle.Radius = 16;
slider.ThumbOverlayStyle.Fill = new SolidColorBrush(Color.FromArgb("#22000000"));
```

### Pattern 3: High Contrast Accessibility

```csharp
// Large thumb with strong border for visibility
slider.ThumbStyle.Radius = 16;
slider.ThumbStyle.Fill = new SolidColorBrush(Colors.White);
slider.ThumbStyle.Stroke = new SolidColorBrush(Colors.Black);
slider.ThumbStyle.StrokeThickness = 4;

slider.ThumbOverlayStyle.Radius = 26;
slider.ThumbOverlayStyle.Fill = new SolidColorBrush(Color.FromArgb("#44000000"));
```

### Pattern 4: iOS-Style Thumb

```csharp
// White thumb with shadow effect
slider.ThumbStyle.Radius = 14;
slider.ThumbStyle.Fill = new SolidColorBrush(Colors.White);
slider.ThumbStyle.Stroke = new SolidColorBrush(Color.FromArgb("#DDDDDD"));
slider.ThumbStyle.StrokeThickness = 1;

// Subtle overlay
slider.ThumbOverlayStyle.Radius = 20;
slider.ThumbOverlayStyle.Fill = new SolidColorBrush(Color.FromArgb("#11000000"));
```

## Best Practices

1. **Size Ratio**: Overlay radius should be 1.5-2x the thumb radius
2. **Contrast**: Ensure thumb is visible against track and background
3. **Touch Target**: Minimum thumb radius of 20px for touch interfaces
4. **Overlay Opacity**: Use 10-30% opacity for overlay (alpha: 0x19-0x4C)
5. **Border Usage**: Add stroke for better definition on light/dark themes
6. **Accessibility**: Larger thumbs improve usability for motor-impaired users

## Troubleshooting

### Thumb Not Visible
- Verify `Fill` color contrasts with track
- Check if `Radius` > 0
- Ensure thumb isn't hidden behind other elements

### Overlay Not Appearing
- Overlay only appears during interaction (drag/hover)
- Verify `Radius` > thumb radius
- Check `Fill` has visible alpha channel

### Thumb Too Small for Touch
- Increase `Radius` to at least 20 for mobile devices
- Increase `ThumbOverlayStyle.Radius` for larger touch target

## Next Steps

- **Tooltip**: Display formatted date during thumb interaction
- **Events**: Handle thumb drag events for custom logic
- **Track Styling**: Coordinate thumb color with track colors
