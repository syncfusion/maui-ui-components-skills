# Ticks in .NET MAUI Range Slider

## Table of Contents
- [Overview](#overview)
- [Major Ticks](#major-ticks)
  - [ShowTicks Property](#showticks-property)
  - [With and Without Interval](#with-and-without-interval)
- [Minor Ticks](#minor-ticks)
  - [MinorTicksPerInterval](#minorticksperinterval)
- [Major Tick Styling](#major-tick-styling)
  - [Major Tick Colors](#major-tick-colors)
  - [Major Tick Sizes](#major-tick-sizes)
- [Minor Tick Styling](#minor-tick-styling)
  - [Minor Tick Colors](#minor-tick-colors)
  - [Minor Tick Sizes](#minor-tick-sizes)
- [Tick Positioning](#tick-positioning)
  - [Tick Offset](#tick-offset)
- [Visual State Management](#visual-state-management)
  - [Disabled Ticks](#disabled-ticks)
- [Common Scenarios](#common-scenarios)
- [Best Practices](#best-practices)
- [Related References](#related-references)

## Overview

Ticks in the .NET MAUI Range Slider (`SfRangeSlider`) are visual markers that indicate interval points along the track. The control supports both major ticks (at main intervals) and minor ticks (subdivisions between major ticks). This reference covers all aspects of configuring and customizing tick marks.

## Major Ticks

### ShowTicks Property

The `ShowTicks` property controls whether major tick marks are displayed on the track.

**Type:** `bool`  
**Default:** `false`

Major ticks are rendered at each interval point based on the `Minimum`, `Maximum`, and `Interval` properties.

### With and Without Interval

**Without Interval (Auto-calculated):**
```xaml
<sliders:SfRangeSlider ShowTicks="True" />
```

```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    ShowTicks = true
};
```

**With Explicit Interval:**
```xaml
<sliders:SfRangeSlider Interval="0.2"
                       ShowTicks="True" />
```

```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    Interval = 0.2,
    ShowTicks = true
};
```

**Example with Range:**
```xaml
<sliders:SfRangeSlider Minimum="0"
                       Maximum="10"
                       RangeStart="2"
                       RangeEnd="8"
                       Interval="2"
                       ShowTicks="True" />
```

```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    Minimum = 0,
    Maximum = 10,
    RangeStart = 2,
    RangeEnd = 8,
    Interval = 2,
    ShowTicks = true
};
```

For example, if `Minimum` is 0.0, `Maximum` is 10.0, and `Interval` is 2.0, major ticks render at 0.0, 2.0, 4.0, 6.0, 8.0, and 10.0.

## Minor Ticks

### MinorTicksPerInterval

The `MinorTicksPerInterval` property specifies the number of minor ticks between major ticks.

**Type:** `int`  
**Default:** `0`

**Without Interval:**
```xaml
<sliders:SfRangeSlider MinorTicksPerInterval="2"
                       ShowTicks="True" />
```

```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    MinorTicksPerInterval = 2,
    ShowTicks = true
};
```

**With Interval:**
```xaml
<sliders:SfRangeSlider Interval="0.25"
                       MinorTicksPerInterval="1"
                       ShowTicks="True" />
```

```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    Interval = 0.25,
    MinorTicksPerInterval = 1,
    ShowTicks = true
};
```

For example, if `Minimum` is 0.0, `Maximum` is 10.0, `Interval` is 2.0, and `MinorTicksPerInterval` is 1, minor ticks render at 1.0, 3.0, 5.0, 7.0, and 9.0.

## Major Tick Styling

### Major Tick Colors

The `MajorTickStyle` class provides properties to customize active and inactive major tick appearance.

**Active and Inactive Fill Colors:**

**Properties:**
- `ActiveFill` - Color of major ticks within selected range
- `InactiveFill` - Color of major ticks outside selected range

**XAML Example:**
```xaml
<sliders:SfRangeSlider Interval="0.2"
                       ShowTicks="True">
    <sliders:SfRangeSlider.MajorTickStyle>
        <sliders:SliderTickStyle ActiveFill="#EE3F3F"
                                 InactiveFill="#F7B1AE" />
    </sliders:SfRangeSlider.MajorTickStyle>
</sliders:SfRangeSlider>
```

**C# Example:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    Interval = 0.2,
    ShowTicks = true
};
rangeSlider.MajorTickStyle.ActiveFill = new SolidColorBrush(Color.FromArgb("#EE3F3F"));
rangeSlider.MajorTickStyle.InactiveFill = new SolidColorBrush(Color.FromArgb("#F7B1AE"));
```

**Active Range Definition:**
- **Active side**: Between start and end thumbs
- **Inactive side**: From `Minimum` to start thumb, and end thumb to `Maximum`

### Major Tick Sizes

Configure major tick dimensions using `ActiveSize` and `InactiveSize` properties.

**Properties:**
- `ActiveSize` - Size of active major ticks
- `InactiveSize` - Size of inactive major ticks

**Type:** `Size`  
**Default:** `Size(2.0, 8.0)` (width, height)

**XAML Example:**
```xaml
<sliders:SfRangeSlider Interval="0.2"
                       ShowTicks="True">
    <sliders:SfRangeSlider.MajorTickStyle>
        <sliders:SliderTickStyle ActiveSize="2,15"
                                 InactiveSize="2,15" />
    </sliders:SfRangeSlider.MajorTickStyle>
</sliders:SfRangeSlider>
```

**C# Example:**
```csharp
rangeSlider.MajorTickStyle.ActiveSize = new Size(2, 15);
rangeSlider.MajorTickStyle.InactiveSize = new Size(2, 15);
```

**Complete Styling Example:**
```xaml
<sliders:SfRangeSlider.MajorTickStyle>
    <sliders:SliderTickStyle ActiveFill="#EE3F3F"
                             InactiveFill="#F7B1AE"
                             ActiveSize="3,12"
                             InactiveSize="2,10" />
</sliders:SfRangeSlider.MajorTickStyle>
```

## Minor Tick Styling

### Minor Tick Colors

The `MinorTickStyle` class provides properties to customize minor tick appearance.

**Properties:**
- `ActiveFill` - Color of minor ticks within selected range
- `InactiveFill` - Color of minor ticks outside selected range

**XAML Example:**
```xaml
<sliders:SfRangeSlider Interval="0.2"
                       ShowTicks="True"
                       MinorTicksPerInterval="1">
    <sliders:SfRangeSlider.MinorTickStyle>
        <sliders:SliderTickStyle ActiveFill="#EE3F3F"
                                 InactiveFill="#F7B1AE" />
    </sliders:SfRangeSlider.MinorTickStyle>
</sliders:SfRangeSlider>
```

**C# Example:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    Interval = 0.2,
    ShowTicks = true,
    MinorTicksPerInterval = 1
};
rangeSlider.MinorTickStyle.ActiveFill = new SolidColorBrush(Color.FromArgb("#EE3F3F"));
rangeSlider.MinorTickStyle.InactiveFill = new SolidColorBrush(Color.FromArgb("#F7B1AE"));
```

### Minor Tick Sizes

Configure minor tick dimensions using `ActiveSize` and `InactiveSize` properties.

**Type:** `Size`  
**Default:** `Size(2.0, 8.0)`

**XAML Example:**
```xaml
<sliders:SfRangeSlider Interval="0.2"
                       ShowTicks="True"
                       MinorTicksPerInterval="1">
    <sliders:SfRangeSlider.MinorTickStyle>
        <sliders:SliderTickStyle ActiveSize="2,10"
                                 InactiveSize="2,10" />
    </sliders:SfRangeSlider.MinorTickStyle>
</sliders:SfRangeSlider>
```

**C# Example:**
```csharp
rangeSlider.MinorTickStyle.ActiveSize = new Size(2, 10);
rangeSlider.MinorTickStyle.InactiveSize = new Size(2, 10);
```

**Combined Major and Minor Tick Styling:**
```xaml
<sliders:SfRangeSlider Interval="0.2"
                       ShowTicks="True"
                       MinorTicksPerInterval="1">
    <sliders:SfRangeSlider.MajorTickStyle>
        <sliders:SliderTickStyle ActiveSize="2,15"
                                 InactiveSize="2,15"
                                 ActiveFill="#EE3F3F"
                                 InactiveFill="#F7B1AE" />
    </sliders:SfRangeSlider.MajorTickStyle>
    
    <sliders:SfRangeSlider.MinorTickStyle>
        <sliders:SliderTickStyle ActiveSize="2,10"
                                 InactiveSize="2,10"
                                 ActiveFill="#EE3F3F"
                                 InactiveFill="#F7B1AE" />
    </sliders:SfRangeSlider.MinorTickStyle>
</sliders:SfRangeSlider>
```

```csharp
rangeSlider.MajorTickStyle = new SliderTickStyle
{
    ActiveSize = new Size(2, 15),
    InactiveSize = new Size(2, 15),
    ActiveFill = new SolidColorBrush(Color.FromArgb("#EE3F3F")),
    InactiveFill = new SolidColorBrush(Color.FromArgb("#F7B1AE"))
};

rangeSlider.MinorTickStyle = new SliderTickStyle
{
    ActiveSize = new Size(2, 10),
    InactiveSize = new Size(2, 10),
    ActiveFill = new SolidColorBrush(Color.FromArgb("#EE3F3F")),
    InactiveFill = new SolidColorBrush(Color.FromArgb("#F7B1AE"))
};
```

## Tick Positioning

### Tick Offset

The `Offset` property adjusts vertical spacing between ticks and the track.

**Type:** `double`  
**Default:** `3.0`

**XAML Example:**
```xaml
<sliders:SfRangeSlider Interval="0.2"
                       ShowTicks="True"
                       MinorTicksPerInterval="1">
    <sliders:SfRangeSlider.MajorTickStyle>
        <sliders:SliderTickStyle Offset="5" />
    </sliders:SfRangeSlider.MajorTickStyle>
    
    <sliders:SfRangeSlider.MinorTickStyle>
        <sliders:SliderTickStyle Offset="5" />
    </sliders:SfRangeSlider.MinorTickStyle>
</sliders:SfRangeSlider>
```

**C# Example:**
```csharp
rangeSlider.MajorTickStyle.Offset = 5;
rangeSlider.MinorTickStyle.Offset = 5;
```

## Visual State Management

### Disabled Ticks

Customize tick appearance when the slider is disabled using Visual State Manager.

**XAML Example:**
```xaml
<ContentPage.Resources>
    <Style TargetType="sliders:SfRangeSlider">
        <Setter Property="Interval" Value="0.25" />
        <Setter Property="ShowTicks" Value="True" />
        <Setter Property="MinorTicksPerInterval" Value="2" />
        <Setter Property="VisualStateManager.VisualStateGroups">
            <VisualStateGroupList>
                <VisualStateGroup>
                    <VisualState x:Name="Default">
                        <VisualState.Setters>
                            <Setter Property="MajorTickStyle">
                                <Setter.Value>
                                    <sliders:SliderTickStyle ActiveSize="2,10"
                                                             InactiveSize="2,10"
                                                             ActiveFill="#EE3F3F"
                                                             InactiveFill="#F7B1AE" />
                                </Setter.Value>
                            </Setter>
                            <Setter Property="MinorTickStyle">
                                <Setter.Value>
                                    <sliders:SliderTickStyle ActiveSize="2,6"
                                                             InactiveSize="2,6"
                                                             ActiveFill="#EE3F3F"
                                                             InactiveFill="#F7B1AE" />
                                </Setter.Value>
                            </Setter>
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState x:Name="Disabled">
                        <VisualState.Setters>
                            <Setter Property="MajorTickStyle">
                                <Setter.Value>
                                    <sliders:SliderTickStyle ActiveSize="2,10"
                                                             InactiveSize="2,10"
                                                             ActiveFill="Gray"
                                                             InactiveFill="LightGray" />
                                </Setter.Value>
                            </Setter>
                            <Setter Property="MinorTickStyle">
                                <Setter.Value>
                                    <sliders:SliderTickStyle ActiveSize="2,6"
                                                             InactiveSize="2,6"
                                                             ActiveFill="Gray"
                                                             InactiveFill="LightGray" />
                                </Setter.Value>
                            </Setter>
                            <Setter Property="ThumbStyle">
                                <Setter.Value>
                                    <sliders:SliderThumbStyle Fill="Gray" />
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
    <VerticalStackLayout Padding="10">
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
VisualState defaultState = new() { Name = "Default" };
defaultState.Setters.Add(new Setter
{
    Property = SfRangeSlider.MajorTickStyleProperty,
    Value = new SliderTickStyle
    {
        ActiveSize = new Size(2, 10),
        InactiveSize = new Size(2, 10),
        ActiveFill = Color.FromArgb("#EE3F3F"),
        InactiveFill = Color.FromArgb("#F7B1AE")
    }
});
defaultState.Setters.Add(new Setter
{
    Property = SfRangeSlider.MinorTickStyleProperty,
    Value = new SliderTickStyle
    {
        ActiveSize = new Size(2, 6),
        InactiveSize = new Size(2, 6),
        ActiveFill = Color.FromArgb("#EE3F3F"),
        InactiveFill = Color.FromArgb("#F7B1AE")
    }
});

// Disabled State
VisualState disabledState = new() { Name = "Disabled" };
disabledState.Setters.Add(new Setter
{
    Property = SfRangeSlider.MajorTickStyleProperty,
    Value = new SliderTickStyle
    {
        ActiveSize = new Size(2, 10),
        InactiveSize = new Size(2, 10),
        ActiveFill = Colors.Gray,
        InactiveFill = Colors.LightGray
    }
});
disabledState.Setters.Add(new Setter
{
    Property = SfRangeSlider.MinorTickStyleProperty,
    Value = new SliderTickStyle
    {
        ActiveSize = new Size(2, 6),
        InactiveSize = new Size(2, 6),
        ActiveFill = Colors.Gray,
        InactiveFill = Colors.LightGray
    }
});

commonStateGroup.States.Add(defaultState);
commonStateGroup.States.Add(disabledState);
visualStateGroupList.Add(commonStateGroup);
VisualStateManager.SetVisualStateGroups(rangeSlider, visualStateGroupList);
```

## Common Scenarios

### Standard Tick Configuration
```xaml
<sliders:SfRangeSlider Interval="10"
                       ShowTicks="True"
                       MinorTicksPerInterval="4" />
```

### Emphasized Active Range
```xaml
<sliders:SfRangeSlider.MajorTickStyle>
    <sliders:SliderTickStyle ActiveFill="DarkBlue"
                             InactiveFill="LightGray"
                             ActiveSize="3,12"
                             InactiveSize="2,8" />
</sliders:SfRangeSlider.MajorTickStyle>
```

### Subtle Minor Ticks
```xaml
<sliders:SfRangeSlider.MinorTickStyle>
    <sliders:SliderTickStyle ActiveFill="#88000000"
                             InactiveFill="#44000000"
                             ActiveSize="1,6"
                             InactiveSize="1,6" />
</sliders:SfRangeSlider.MinorTickStyle>
```

### Matching Track Colors
```csharp
var activeColor = Color.FromArgb("#EE3F3F");
var inactiveColor = Color.FromArgb("#F7B1AE");

rangeSlider.TrackStyle.ActiveFill = activeColor;
rangeSlider.TrackStyle.InactiveFill = inactiveColor;
rangeSlider.MajorTickStyle.ActiveFill = activeColor;
rangeSlider.MajorTickStyle.InactiveFill = inactiveColor;
rangeSlider.MinorTickStyle.ActiveFill = activeColor;
rangeSlider.MinorTickStyle.InactiveFill = inactiveColor;
```

## Best Practices

1. **Set ShowTicks and Interval Together**: Always define `Interval` when using `ShowTicks` for predictable tick placement.

2. **Minor Tick Ratio**: Keep `MinorTicksPerInterval` between 1-4 for optimal visual balance.

3. **Size Differentiation**: Make major ticks larger than minor ticks for clear hierarchy:
   - Major: `Size(2, 12-15)`
   - Minor: `Size(1-2, 6-8)`

4. **Color Coordination**: Match tick colors with track colors for cohesive appearance.

5. **Active/Inactive Distinction**: Use contrasting colors between active and inactive states for clarity.

6. **Offset Adjustment**: Increase offset if ticks overlap with labels or track elements.

7. **Performance**: Limit total tick count (major + minor) to avoid rendering overhead on mobile devices.

8. **Accessibility**: Ensure sufficient color contrast for users with visual impairments.

9. **Responsive Design**: Test tick appearance at different slider widths and orientations.

10. **Visual States**: Always provide disabled state styling for complete user experience.

## Related References

- [labels.md](./labels.md) - Label configuration and styling
- [intervals-and-selection.md](./intervals-and-selection.md) - Interval property details
- [dividers.md](./dividers.md) - Divider customization
- [thumbs-and-overlays.md](./thumbs-and-overlays.md) - Thumb styling
- [track.md](./track.md) - Track customization
