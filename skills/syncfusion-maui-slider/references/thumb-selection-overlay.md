# Thumb, Selection, and Overlay

This guide covers thumb customization, thumb overlay configuration, and selection track styling for the .NET MAUI Slider.

## Table of Contents
- [Thumb Overview](#thumb-overview)
- [Thumb Size](#thumb-size)
- [Thumb Color](#thumb-color)
- [Thumb Stroke](#thumb-stroke)
- [Thumb Overlay Size](#thumb-overlay-size)
- [Thumb Overlay Color](#thumb-overlay-color)
- [Disabled Thumb with Visual State Manager](#disabled-thumb-with-visual-state-manager)
- [Complete Examples](#complete-examples)

## Thumb Overview

The slider consists of two interactive visual elements:

- **Thumb**: The draggable circular button that the user manipulates to change the slider value
- **Thumb Overlay**: A circular ripple effect that appears around the thumb during interaction

Both elements can be extensively customized for appearance and behavior.

## Thumb Size

Control thumb size using the `Radius` property of `SliderThumbStyle`.

### Setting Thumb Radius

```xml
<sliders:SfSlider>
    <sliders:SfSlider.ThumbStyle>
        <sliders:SliderThumbStyle Radius="15" />
    </sliders:SfSlider.ThumbStyle>
</sliders:SfSlider>
```

```csharp
SfSlider slider = new SfSlider();
slider.ThumbStyle.Radius = 15;
```

**Default value**: 10.0

### Size Guidelines

- **Small**: 8-10 pixels (subtle, minimalist design)
- **Medium**: 10-14 pixels (standard, recommended)
- **Large**: 15-20 pixels (prominent, easy to grab)

**Accessibility Note**: Larger thumbs (14-20px) are easier to tap on touch devices and improve accessibility.

### Example with Different Sizes

```csharp
// Small thumb
SfSlider smallSlider = new SfSlider();
smallSlider.ThumbStyle.Radius = 8;

// Medium thumb (default)
SfSlider mediumSlider = new SfSlider();
mediumSlider.ThumbStyle.Radius = 12;

// Large thumb
SfSlider largeSlider = new SfSlider();
largeSlider.ThumbStyle.Radius = 18;
```

## Thumb Color

Customize thumb color using the `Fill` property of `SliderThumbStyle`.

### Setting Thumb Fill

```xml
<sliders:SfSlider>
    <sliders:SfSlider.ThumbStyle>
        <sliders:SliderThumbStyle Fill="#EE3F3F" />
    </sliders:SfSlider.ThumbStyle>
</sliders:SfSlider>
```

```csharp
slider.ThumbStyle.Fill = new SolidColorBrush(Color.FromArgb("#EE3F3F"));
```

### Using Named Colors

```csharp
// Using named colors
slider.ThumbStyle.Fill = new SolidColorBrush(Colors.CornflowerBlue);
slider.ThumbStyle.Fill = new SolidColorBrush(Colors.Purple);
slider.ThumbStyle.Fill = new SolidColorBrush(Colors.Teal);
```

### Using Brand Colors

```csharp
// Define brand color
Color brandPrimary = Color.FromArgb("#6200EE");
slider.ThumbStyle.Fill = new SolidColorBrush(brandPrimary);
```

### Gradient Thumb (Advanced)

```csharp
var gradient = new LinearGradientBrush
{
    StartPoint = new Point(0, 0),
    EndPoint = new Point(1, 1),
    GradientStops = new GradientStopCollection
    {
        new GradientStop { Color = Colors.Blue, Offset = 0.0f },
        new GradientStop { Color = Colors.Purple, Offset = 1.0f }
    }
};
slider.ThumbStyle.Fill = gradient;
```

## Thumb Stroke

Add a border to the thumb using stroke properties.

### Stroke Color and Thickness

```xml
<sliders:SfSlider>
    <sliders:SfSlider.ThumbStyle>
        <sliders:SliderThumbStyle Stroke="#EE3F3F"
                                  StrokeThickness="2" />
    </sliders:SfSlider.ThumbStyle>
</sliders:SfSlider>
```

```csharp
slider.ThumbStyle.Stroke = new SolidColorBrush(Color.FromArgb("#EE3F3F"));
slider.ThumbStyle.StrokeThickness = 2;
```

### SliderThumbStyle Properties

- **Radius**: Size of the thumb (radius in pixels)
- **Fill**: Fill color of the thumb
- **Stroke**: Border color of the thumb
- **StrokeThickness**: Border width of the thumb

### Creating Outlined Thumbs

**Thick Border:**
```xml
<sliders:SliderThumbStyle Fill="White"
                          Stroke="#2196F3"
                          StrokeThickness="4"
                          Radius="14" />
```

**Hollow Thumb:**
```xml
<sliders:SliderThumbStyle Fill="Transparent"
                          Stroke="#FF6B6B"
                          StrokeThickness="3"
                          Radius="12" />
```

### Color Combinations

**Example 1: Classic**
```xml
<sliders:SliderThumbStyle Fill="White"
                          Stroke="#666666"
                          StrokeThickness="2"
                          Radius="12" />
```

**Example 2: Material Design**
```xml
<sliders:SliderThumbStyle Fill="#6200EE"
                          Stroke="White"
                          StrokeThickness="2"
                          Radius="14" />
```

**Example 3: High Contrast**
```xml
<sliders:SliderThumbStyle Fill="#FF4081"
                          Stroke="Black"
                          StrokeThickness="3"
                          Radius="15" />
```

## Thumb Overlay Size

The thumb overlay is a circular ripple effect that appears around the thumb during interaction. Control its size with the `Radius` property of `SliderThumbOverlayStyle`.

### Setting Overlay Radius

```xml
<sliders:SfSlider>
    <sliders:SfSlider.ThumbOverlayStyle>
        <sliders:SliderThumbOverlayStyle Radius="18" />
    </sliders:SfSlider.ThumbOverlayStyle>
</sliders:SfSlider>
```

```csharp
slider.ThumbOverlayStyle.Radius = 18;
```

**Default value**: 24.0

### Size Guidelines

- **Small**: 16-20 pixels (subtle ripple)
- **Medium**: 20-28 pixels (standard ripple)
- **Large**: 28-40 pixels (prominent ripple)

**Design Tip**: Overlay radius should typically be 1.5x to 2x the thumb radius for balanced appearance.

### Example with Proportional Sizing

```csharp
// Thumb radius: 12
// Overlay radius: 24 (2x thumb radius)
slider.ThumbStyle.Radius = 12;
slider.ThumbOverlayStyle.Radius = 24;
```

## Thumb Overlay Color

Customize overlay color using the `Fill` property of `SliderThumbOverlayStyle`.

### Setting Overlay Fill

```xml
<sliders:SfSlider>
    <sliders:SfSlider.ThumbOverlayStyle>
        <sliders:SliderThumbOverlayStyle Fill="#66FFD700" />
    </sliders:SfSlider.ThumbOverlayStyle>
</sliders:SfSlider>
```

```csharp
slider.ThumbOverlayStyle.Fill = new SolidColorBrush(Color.FromArgb("#66FFD700"));
```

### Using Transparency

The overlay typically uses semi-transparent colors for a subtle ripple effect:

```csharp
// 50% transparent blue
slider.ThumbOverlayStyle.Fill = new SolidColorBrush(Colors.Blue.WithAlpha(0.5f));

// 30% transparent purple
slider.ThumbOverlayStyle.Fill = new SolidColorBrush(Color.FromArgb("#4D6200EE"));
```

**Transparency Hex Values:**
- 100% (FF): Opaque
- 75% (BF): Semi-opaque
- 50% (80): Half transparent
- 30% (4D): Mostly transparent
- 25% (40): Very transparent

### Matching Thumb Color

```csharp
// Thumb color
Color thumbColor = Color.FromArgb("#EE3F3F");
slider.ThumbStyle.Fill = new SolidColorBrush(thumbColor);

// Overlay with 30% opacity of thumb color
slider.ThumbOverlayStyle.Fill = new SolidColorBrush(thumbColor.WithAlpha(0.3f));
```

### Complete Thumb and Overlay Setup

```xml
<sliders:SfSlider>
    <sliders:SfSlider.ThumbStyle>
        <sliders:SliderThumbStyle Radius="14"
                                  Fill="#6200EE"
                                  Stroke="White"
                                  StrokeThickness="2" />
    </sliders:SfSlider.ThumbStyle>
    <sliders:SfSlider.ThumbOverlayStyle>
        <sliders:SliderThumbOverlayStyle Radius="28"
                                         Fill="#4D6200EE" />
    </sliders:SfSlider.ThumbOverlayStyle>
</sliders:SfSlider>
```

```csharp
// Thumb
slider.ThumbStyle.Radius = 14;
slider.ThumbStyle.Fill = new SolidColorBrush(Color.FromArgb("#6200EE"));
slider.ThumbStyle.Stroke = new SolidColorBrush(Colors.White);
slider.ThumbStyle.StrokeThickness = 2;

// Overlay
slider.ThumbOverlayStyle.Radius = 28;
slider.ThumbOverlayStyle.Fill = new SolidColorBrush(Color.FromArgb("#4D6200EE"));
```

## Disabled Thumb with Visual State Manager

Use Visual State Manager (VSM) to customize thumb appearance when the slider is disabled.

### Complete Example with VSM

**XAML:**
```xml
<ContentPage.Resources>
    <Style TargetType="sliders:SfSlider">
        <Setter Property="Interval" Value="0.25" />
        <Setter Property="VisualStateManager.VisualStateGroups">
            <VisualStateGroupList>
                <VisualStateGroup>
                    <!-- Default (Enabled) State -->
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

<ContentPage.Content>
    <VerticalStackLayout>
        <Label Text="Enabled" Padding="24,10" />
        <sliders:SfSlider />
        
        <Label Text="Disabled" Padding="24,10" />
        <sliders:SfSlider IsEnabled="False" />
    </VerticalStackLayout>
</ContentPage.Content>
```

### C# Implementation with VSM

```csharp
VerticalStackLayout stackLayout = new();
SfSlider defaultSlider = new();
SfSlider disabledSlider = new() { IsEnabled = false };

VisualStateGroupList visualStateGroupList = new();
VisualStateGroup commonStateGroup = new();

// Default State
VisualState defaultState = new() { Name = "Default" };
defaultState.Setters.Add(new Setter
{
    Property = SfSlider.ThumbStyleProperty,
    Value = new SliderThumbStyle
    {
        Radius = 13,
        Fill = Colors.Red,
        Stroke = Colors.Yellow,
        StrokeThickness = 3
    }
});

// Disabled State
VisualState disabledState = new() { Name = "Disabled" };
disabledState.Setters.Add(new Setter
{
    Property = SfSlider.ThumbStyleProperty,
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
    Property = SfSlider.TrackStyleProperty,
    Value = new SliderTrackStyle
    {
        ActiveFill = Colors.Gray,
        InactiveFill = Colors.LightGray
    }
});

commonStateGroup.States.Add(defaultState);
commonStateGroup.States.Add(disabledState);
visualStateGroupList.Add(commonStateGroup);
VisualStateManager.SetVisualStateGroups(defaultSlider, visualStateGroupList);
VisualStateManager.SetVisualStateGroups(disabledSlider, visualStateGroupList);

stackLayout.Children.Add(new Label { Text = "Enabled", Padding = new Thickness(24, 10) });
stackLayout.Children.Add(defaultSlider);
stackLayout.Children.Add(new Label { Text = "Disabled", Padding = new Thickness(24, 10) });
stackLayout.Children.Add(disabledSlider);

Content = stackLayout;
```

### VSM States

Available visual states:
- **Default**: Normal enabled state
- **Disabled**: When `IsEnabled="False"`

Customize any slider property based on state:
- ThumbStyle
- ThumbOverlayStyle
- TrackStyle
- TickStyle
- DividerStyle
- LabelStyle

## Complete Examples

### Example 1: Material Design Style

```xml
<sliders:SfSlider Minimum="0"
                  Maximum="100"
                  Value="60"
                  ShowLabels="True"
                  Interval="25">
    <sliders:SfSlider.TrackStyle>
        <sliders:SliderTrackStyle ActiveFill="#6200EE"
                                  InactiveFill="#C8B5F7"
                                  ActiveSize="6"
                                  InactiveSize="4" />
    </sliders:SfSlider.TrackStyle>
    <sliders:SfSlider.ThumbStyle>
        <sliders:SliderThumbStyle Radius="14"
                                  Fill="#6200EE"
                                  Stroke="White"
                                  StrokeThickness="2" />
    </sliders:SfSlider.ThumbStyle>
    <sliders:SfSlider.ThumbOverlayStyle>
        <sliders:SliderThumbOverlayStyle Radius="28"
                                         Fill="#4D6200EE" />
    </sliders:SfSlider.ThumbOverlayStyle>
</sliders:SfSlider>
```

### Example 2: iOS Style

```xml
<sliders:SfSlider Minimum="0"
                  Maximum="100"
                  Value="50">
    <sliders:SfSlider.TrackStyle>
        <sliders:SliderTrackStyle ActiveFill="#007AFF"
                                  InactiveFill="#E5E5EA"
                                  ActiveSize="4"
                                  InactiveSize="4" />
    </sliders:SfSlider.TrackStyle>
    <sliders:SfSlider.ThumbStyle>
        <sliders:SliderThumbStyle Radius="14"
                                  Fill="White"
                                  Stroke="#007AFF"
                                  StrokeThickness="1" />
    </sliders:SfSlider.ThumbStyle>
    <sliders:SfSlider.ThumbOverlayStyle>
        <sliders:SliderThumbOverlayStyle Radius="24"
                                         Fill="#33007AFF" />
    </sliders:SfSlider.ThumbOverlayStyle>
</sliders:SfSlider>
```

### Example 3: Bold and Colorful

```xml
<sliders:SfSlider Minimum="0"
                  Maximum="100"
                  Value="75"
                  ShowTicks="True"
                  ShowDividers="True"
                  Interval="25">
    <sliders:SfSlider.TrackStyle>
        <sliders:SliderTrackStyle ActiveFill="#FF4081"
                                  InactiveFill="#FFB2C6"
                                  ActiveSize="10"
                                  InactiveSize="6" />
    </sliders:SfSlider.TrackStyle>
    <sliders:SfSlider.ThumbStyle>
        <sliders:SliderThumbStyle Radius="18"
                                  Fill="#FF4081"
                                  Stroke="White"
                                  StrokeThickness="4" />
    </sliders:SfSlider.ThumbStyle>
    <sliders:SfSlider.ThumbOverlayStyle>
        <sliders:SliderThumbOverlayStyle Radius="36"
                                         Fill="#40FF4081" />
    </sliders:SfSlider.ThumbOverlayStyle>
</sliders:SfSlider>
```

### Example 4: Minimalist Design

```csharp
public class MinimalistSlider : ContentView
{
    public MinimalistSlider()
    {
        var slider = new SfSlider
        {
            Minimum = 0,
            Maximum = 100,
            Value = 50
        };
        
        // Thin track
        slider.TrackStyle.ActiveFill = new SolidColorBrush(Colors.Black);
        slider.TrackStyle.InactiveFill = new SolidColorBrush(Colors.LightGray);
        slider.TrackStyle.ActiveSize = 2;
        slider.TrackStyle.InactiveSize = 2;
        
        // Small thumb
        slider.ThumbStyle.Radius = 8;
        slider.ThumbStyle.Fill = new SolidColorBrush(Colors.Black);
        
        // Subtle overlay
        slider.ThumbOverlayStyle.Radius = 16;
        slider.ThumbOverlayStyle.Fill = new SolidColorBrush(Colors.Black.WithAlpha(0.2f));
        
        Content = slider;
    }
}
```

## Best Practices

### Thumb Sizing
- Use 12-16px radius for standard touch targets
- Minimum 10px for accessibility on mobile devices
- Maximum 20px to avoid overwhelming the UI

### Color Selection
- Match thumb color with track ActiveFill for consistency
- Use white or contrasting stroke for better visibility
- Overlay should be 30-50% transparent version of thumb color

### Proportions
- Overlay radius = 1.5x to 2x thumb radius (recommended)
- Track ActiveSize = 0.5x to 0.75x thumb radius
- Stroke thickness = 1-3px (2px is standard)

### Performance
- Use solid colors instead of gradients for better performance
- Cache brush instances if creating multiple sliders
- Avoid complex brush patterns in thumb/overlay

### Accessibility
- Ensure thumb is large enough for easy tapping (14px+ radius)
- Provide sufficient color contrast between thumb and track
- Consider users with motor impairments (larger targets)

## Troubleshooting

### Issue: Thumb Not Visible

**Cause**: Thumb fill color matches background or radius is 0  
**Solution**: Set contrasting fill color and appropriate radius:
```xml
<sliders:SliderThumbStyle Fill="#FF6B6B" Radius="12" />
```

### Issue: Overlay Not Showing

**Cause**: Overlay color is fully transparent or radius too small  
**Solution**: Use semi-transparent color:
```xml
<sliders:SliderThumbOverlayStyle Fill="#66FF6B6B" Radius="24" />
```

### Issue: Thumb Border Not Visible

**Cause**: StrokeThickness is 0 or Stroke color not set  
**Solution**: Set both stroke and thickness:
```xml
<sliders:SliderThumbStyle Stroke="White" StrokeThickness="2" />
```

### Issue: Disabled State Not Changing Appearance

**Cause**: Visual State Manager not configured  
**Solution**: Define VSM states as shown in the examples above.

## Summary

Key thumb and overlay properties:

**ThumbStyle:**
- `Radius`: Size of the thumb (default: 10)
- `Fill`: Thumb color
- `Stroke`: Thumb border color
- `StrokeThickness`: Thumb border width

**ThumbOverlayStyle:**
- `Radius`: Size of the ripple overlay (default: 24)
- `Fill`: Overlay color (typically semi-transparent)

**Visual State Manager:**
- Define "Default" and "Disabled" states
- Customize all styling properties per state
- Apply to slider using `VisualStateManager.VisualStateGroups`

Use proportional sizing (overlay = 2x thumb radius) and semi-transparent overlay colors for best visual results.
