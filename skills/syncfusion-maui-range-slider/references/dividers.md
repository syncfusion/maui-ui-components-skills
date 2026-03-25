# Dividers in .NET MAUI Range Slider

## Table of Contents
- [Overview](#overview)
- [Core Properties](#core-properties)
  - [ShowDividers](#showdividers)
- [Divider Styling](#divider-styling)
  - [Divider Radius](#divider-radius)
  - [Divider Colors](#divider-colors)
  - [Divider Stroke](#divider-stroke)
- [Visual State Management](#visual-state-management)
  - [Disabled Dividers](#disabled-dividers)
- [Common Scenarios](#common-scenarios)
- [Best Practices](#best-practices)
- [Related References](#related-references)

## Overview

Dividers in the .NET MAUI Range Slider (`SfRangeSlider`) are circular markers that appear at interval points along the track. Unlike ticks (which are line-based), dividers are shape-based indicators that provide a more prominent visual representation of interval points. This reference covers all aspects of configuring and customizing dividers.

## Core Properties

### ShowDividers

The `ShowDividers` property controls whether dividers are displayed on the track.

**Type:** `bool`  
**Default:** `false`

Dividers are rendered at major interval points based on the `Minimum`, `Maximum`, and `Interval` properties.

**XAML Example:**
```xaml
<sliders:SfRangeSlider RangeStart="0.2"
                       RangeEnd="0.8"
                       Interval="0.2"
                       ShowDividers="True" />
```

**C# Example:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    RangeStart = 0.2,
    RangeEnd = 0.8,
    Interval = 0.2,
    ShowDividers = true
};
```

For example, if `Minimum` is 0.0, `Maximum` is 10.0, and `Interval` is 2.0, dividers will render at 0.0, 2.0, 4.0, 6.0, 8.0, and 10.0.

## Divider Styling

### Divider Radius

Control the size of dividers using the `ActiveRadius` and `InactiveRadius` properties of the `DividerStyle` class.

**Properties:**
- `ActiveRadius` - Radius of dividers within selected range
- `InactiveRadius` - Radius of dividers outside selected range

**Type:** `double`  
**Default:** Varies by platform

**Active/Inactive Range Definition:**
- **Active side**: Between start and end thumbs
- **Inactive side**: From `Minimum` to start thumb, and end thumb to `Maximum`

**XAML Example:**
```xaml
<sliders:SfRangeSlider RangeStart="0.2"
                       RangeEnd="0.8"
                       Interval="0.2"
                       ShowDividers="True">
    <sliders:SfRangeSlider.DividerStyle>
        <sliders:SliderDividerStyle ActiveRadius="3"
                                    InactiveRadius="5" />
    </sliders:SfRangeSlider.DividerStyle>
</sliders:SfRangeSlider>
```

**C# Example:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    RangeStart = 0.2,
    RangeEnd = 0.8,
    Interval = 0.2,
    ShowDividers = true
};
rangeSlider.DividerStyle.ActiveRadius = 3;
rangeSlider.DividerStyle.InactiveRadius = 5;
```

### Divider Colors

Customize divider fill colors using the `ActiveFill` and `InactiveFill` properties.

**Properties:**
- `ActiveFill` - Fill color for dividers in active range
- `InactiveFill` - Fill color for dividers in inactive range

**Type:** `Brush`

**XAML Example:**
```xaml
<sliders:SfRangeSlider RangeStart="0.2"
                       RangeEnd="0.8"
                       Interval="0.2"
                       ShowDividers="True">
    <sliders:SfRangeSlider.DividerStyle>
        <sliders:SliderDividerStyle ActiveRadius="7"
                                    InactiveRadius="7"
                                    ActiveFill="#EE3F3F"
                                    InactiveFill="#F7B1AE" />
    </sliders:SfRangeSlider.DividerStyle>
</sliders:SfRangeSlider>
```

**C# Example:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    RangeStart = 0.2,
    RangeEnd = 0.8,
    Interval = 0.2,
    ShowDividers = true
};
rangeSlider.DividerStyle.ActiveRadius = 7;
rangeSlider.DividerStyle.InactiveRadius = 7;
rangeSlider.DividerStyle.ActiveFill = new SolidColorBrush(Color.FromArgb("#EE3F3F"));
rangeSlider.DividerStyle.InactiveFill = new SolidColorBrush(Color.FromArgb("#F7B1AE"));
```

### Divider Stroke

Add borders to dividers using stroke properties.

**Properties:**
- `ActiveStroke` - Stroke color for active dividers
- `InactiveStroke` - Stroke color for inactive dividers
- `ActiveStrokeThickness` - Stroke width for active dividers
- `InactiveStrokeThickness` - Stroke width for inactive dividers

**Type:** 
- `Stroke` properties: `Brush`
- `StrokeThickness` properties: `double`

**XAML Example:**
```xaml
<sliders:SfRangeSlider RangeStart="0.2"
                       RangeEnd="0.8"
                       Interval="0.2"
                       ShowDividers="True">
    <sliders:SfRangeSlider.DividerStyle>
        <sliders:SliderDividerStyle ActiveRadius="7"
                                    InactiveRadius="7"
                                    ActiveFill="#EE3F3F"
                                    InactiveFill="#F7B1AE"
                                    ActiveStrokeThickness="2"
                                    InactiveStrokeThickness="2"
                                    ActiveStroke="#FFD700"
                                    InactiveStroke="#FFD700" />
    </sliders:SfRangeSlider.DividerStyle>
</sliders:SfRangeSlider>
```

**C# Example:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    RangeStart = 0.2,
    RangeEnd = 0.8,
    Interval = 0.2,
    ShowDividers = true
};
rangeSlider.DividerStyle.ActiveRadius = 7;
rangeSlider.DividerStyle.InactiveRadius = 7;
rangeSlider.DividerStyle.ActiveFill = new SolidColorBrush(Color.FromArgb("#EE3F3F"));
rangeSlider.DividerStyle.InactiveFill = new SolidColorBrush(Color.FromArgb("#F7B1AE"));
rangeSlider.DividerStyle.ActiveStroke = new SolidColorBrush(Color.FromArgb("#FFD700"));
rangeSlider.DividerStyle.InactiveStroke = new SolidColorBrush(Color.FromArgb("#FFD700"));
rangeSlider.DividerStyle.ActiveStrokeThickness = 2;
rangeSlider.DividerStyle.InactiveStrokeThickness = 2;
```

**Complete DividerStyle Example:**
```csharp
rangeSlider.DividerStyle = new SliderDividerStyle
{
    ActiveRadius = 7,
    InactiveRadius = 7,
    ActiveFill = new SolidColorBrush(Color.FromArgb("#EE3F3F")),
    InactiveFill = new SolidColorBrush(Color.FromArgb("#F7B1AE")),
    ActiveStroke = new SolidColorBrush(Color.FromArgb("#FFD700")),
    InactiveStroke = new SolidColorBrush(Color.FromArgb("#FFD700")),
    ActiveStrokeThickness = 2,
    InactiveStrokeThickness = 2
};
```

## Visual State Management

### Disabled Dividers

Customize divider appearance when the slider is disabled using Visual State Manager.

**XAML Example:**
```xaml
<ContentPage.Resources>
    <Style TargetType="sliders:SfRangeSlider">
        <Setter Property="Interval" Value="0.25" />
        <Setter Property="ShowDividers" Value="True" />
        <Setter Property="VisualStateManager.VisualStateGroups">
            <VisualStateGroupList>
                <VisualStateGroup>
                    <VisualState x:Name="Default">
                        <VisualState.Setters>
                            <Setter Property="DividerStyle">
                                <Setter.Value>
                                    <sliders:SliderDividerStyle ActiveFill="#EE3F3F"
                                                                InactiveFill="#88EE3F3F"
                                                                ActiveRadius="5"
                                                                InactiveRadius="4" />
                                </Setter.Value>
                            </Setter>
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState x:Name="Disabled">
                        <VisualState.Setters>
                            <Setter Property="DividerStyle">
                                <Setter.Value>
                                    <sliders:SliderDividerStyle ActiveFill="Gray"
                                                                InactiveFill="LightGray"
                                                                ActiveRadius="5"
                                                                InactiveRadius="4" />
                                </Setter.Value>
                            </Setter>
                            <Setter Property="TrackStyle">
                                <Setter.Value>
                                    <sliders:SliderTrackStyle ActiveFill="Gray"
                                                              InactiveFill="LightGray" />
                                </Setter.Value>
                            </Setter>
                            <Setter Property="ThumbStyle">
                                <Setter.Value>
                                    <sliders:SliderThumbStyle Fill="Gray" />
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
        <sliders:SfRangeSlider />
        <Label Text="Disabled" Padding="24,10" />
        <sliders:SfRangeSlider IsEnabled="False" />
    </VerticalStackLayout>
</ContentPage.Content>
```

**C# Example:**
```csharp
VisualStateGroupList visualStateGroupList = new();
VisualStateGroup commonStateGroup = new();

// Default State
VisualState enabledState = new() { Name = "Default" };
enabledState.Setters.Add(new Setter
{
    Property = SfRangeSlider.DividerStyleProperty,
    Value = new SliderDividerStyle
    {
        ActiveFill = Color.FromArgb("#EE3F3F"),
        InactiveFill = Color.FromArgb("#88EE3F3F"),
        ActiveRadius = 5,
        InactiveRadius = 4
    }
});

// Disabled State
VisualState disabledState = new() { Name = "Disabled" };
disabledState.Setters.Add(new Setter
{
    Property = SfRangeSlider.DividerStyleProperty,
    Value = new SliderDividerStyle
    {
        ActiveFill = Colors.Gray,
        InactiveFill = Colors.LightGray,
        ActiveRadius = 5,
        InactiveRadius = 4
    }
});
disabledState.Setters.Add(new Setter
{
    Property = SfRangeSlider.TrackStyleProperty,
    Value = new SliderTrackStyle
    {
        ActiveFill = Colors.Gray,
        InactiveFill = Colors.LightGray
    }
});
disabledState.Setters.Add(new Setter
{
    Property = SfRangeSlider.ThumbStyleProperty,
    Value = new SliderThumbStyle
    {
        Fill = Colors.Gray
    }
});

commonStateGroup.States.Add(enabledState);
commonStateGroup.States.Add(disabledState);
visualStateGroupList.Add(commonStateGroup);
VisualStateManager.SetVisualStateGroups(rangeSlider, visualStateGroupList);
```

## Common Scenarios

### Basic Divider Configuration
```xaml
<sliders:SfRangeSlider Interval="5"
                       ShowDividers="True">
    <sliders:SfRangeSlider.DividerStyle>
        <sliders:SliderDividerStyle ActiveRadius="6"
                                    InactiveRadius="6" />
    </sliders:SfRangeSlider.DividerStyle>
</sliders:SfRangeSlider>
```

### Emphasized Active Range with Different Sizes
```xaml
<sliders:SfRangeSlider.DividerStyle>
    <sliders:SliderDividerStyle ActiveRadius="8"
                                InactiveRadius="5"
                                ActiveFill="DarkBlue"
                                InactiveFill="LightGray" />
</sliders:SfRangeSlider.DividerStyle>
```

### Dividers with Contrasting Borders
```xaml
<sliders:SfRangeSlider.DividerStyle>
    <sliders:SliderDividerStyle ActiveRadius="6"
                                InactiveRadius="6"
                                ActiveFill="White"
                                InactiveFill="White"
                                ActiveStroke="DarkBlue"
                                InactiveStroke="Gray"
                                ActiveStrokeThickness="2"
                                InactiveStrokeThickness="1" />
</sliders:SfRangeSlider.DividerStyle>
```

### Semi-transparent Inactive Dividers
```xaml
<sliders:SfRangeSlider.DividerStyle>
    <sliders:SliderDividerStyle ActiveRadius="7"
                                InactiveRadius="7"
                                ActiveFill="#FF4081"
                                InactiveFill="#4D4081" />
</sliders:SfRangeSlider.DividerStyle>
```

### Matching Track and Divider Colors
```csharp
var primaryColor = Color.FromArgb("#6200EE");
var secondaryColor = Color.FromArgb("#03DAC6");

rangeSlider.TrackStyle.ActiveFill = primaryColor;
rangeSlider.TrackStyle.InactiveFill = secondaryColor.WithAlpha(0.3f);
rangeSlider.DividerStyle.ActiveFill = primaryColor;
rangeSlider.DividerStyle.InactiveFill = secondaryColor.WithAlpha(0.5f);
```

## Best Practices

1. **Choose Between Ticks and Dividers**: Generally, use either ticks or dividers, not both, to avoid visual clutter.

2. **Interval Requirement**: Always set the `Interval` property when using dividers for predictable placement.

3. **Radius Guidelines**: 
   - Small sliders: 4-6 pixels
   - Medium sliders: 6-8 pixels
   - Large sliders: 8-10 pixels

4. **Active/Inactive Distinction**: Use contrasting colors or sizes to clearly differentiate active and inactive ranges.

5. **Stroke Usage**: Add strokes for better visibility on complex backgrounds or when dividers blend with the track.

6. **Color Coordination**: Match divider colors with track and thumb colors for cohesive design.

7. **Performance**: Limit the number of dividers (keep interval reasonable) to maintain smooth rendering on mobile devices.

8. **Accessibility**: Ensure sufficient color contrast between dividers and background for users with visual impairments.

9. **Responsive Design**: Test divider appearance at different slider widths and ensure they don't overlap.

10. **Visual Hierarchy**: If using dividers with labels, ensure dividers don't overshadow the label text.

## Related References

- [ticks.md](./ticks.md) - Alternative tick marks configuration
- [intervals-and-selection.md](./intervals-and-selection.md) - Interval property details
- [labels.md](./labels.md) - Label configuration
- [thumbs-and-overlays.md](./thumbs-and-overlays.md) - Thumb styling
- [track.md](./track.md) - Track customization
