# Track and Range Configuration

This guide covers track customization, range values, orientation, and styling for the .NET MAUI DateTime Range Slider.

## Table of Contents
- [Overview](#overview)
- [Minimum and Maximum](#minimum-and-maximum)
- [Range Values (RangeStart and RangeEnd)](#range-values-rangestart-and-rangeend)
- [Track Colors](#track-colors)
- [Track Height](#track-height)
- [Track Extent](#track-extent)
- [Orientation](#orientation)
- [Inverse Direction](#inverse-direction)
- [Disabled Track (Visual State Manager)](#disabled-track-visual-state-manager)

## Overview

The track is the horizontal or vertical line on which the thumbs slide. The `TrackStyle` property controls the appearance of both active and inactive portions of the track.

**Active Track:** The region between the start and end thumbs (selected range)  
**Inactive Track:** The regions from Minimum to start thumb and from end thumb to Maximum

## Minimum and Maximum

Set the selectable range boundaries with `Minimum` and `Maximum` properties.

### Properties

- `Minimum` (DateTime) - Lower bound that users can select
- `Maximum` (DateTime) - Upper bound that users can select

**Rules:**
- `Minimum` must be less than `Maximum`
- Default value is `null` for both

### Example

```xaml
<sliders:SfDateTimeRangeSlider 
    Minimum="2010-01-01" 
    Maximum="2020-01-01" 
    RangeStart="2012-01-01" 
    RangeEnd="2018-01-01" 
    ShowLabels="True" />
```

```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2020, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2018, 01, 01);
rangeSlider.ShowLabels = true;
```

## Range Values (RangeStart and RangeEnd)

The selected range is defined by `RangeStart` and `RangeEnd` properties, which position the start and end thumbs.

### Properties

- `RangeStart` (DateTime) - Starting value of the selected range
- `RangeEnd` (DateTime) - Ending value of the selected range

**Rules:**
- `RangeStart` must be >= `Minimum`
- `RangeEnd` must be <= `Maximum`
- `RangeStart` must be <= `RangeEnd`

### Example

```xaml
<sliders:SfDateTimeRangeSlider 
    Minimum="2010-01-01" 
    Maximum="2020-01-01" 
    RangeStart="2012-01-01" 
    RangeEnd="2018-01-01" 
    ShowLabels="True" />
```

```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2020, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2018, 01, 01);
rangeSlider.ShowLabels = true;
```

## Track Colors

Customize active and inactive track colors using the `TrackStyle` property.

### Properties

- `ActiveFill` (Brush) - Color of the track between thumbs
- `InactiveFill` (Brush) - Color of the track outside the thumbs

### Example

```xaml
<sliders:SfDateTimeRangeSlider 
    Minimum="2010-01-01" 
    Maximum="2018-01-01" 
    RangeStart="2012-01-01" 
    RangeEnd="2016-01-01">
  
    <sliders:SfDateTimeRangeSlider.TrackStyle>
        <sliders:SliderTrackStyle 
            ActiveFill="#EE3F3F" 
            InactiveFill="#F7B1AE" />
    </sliders:SfDateTimeRangeSlider.TrackStyle>

</sliders:SfDateTimeRangeSlider>
```

```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2018, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2016, 01, 01);
rangeSlider.TrackStyle.ActiveFill = new SolidColorBrush(Color.FromArgb("#EE3F3F"));
rangeSlider.TrackStyle.InactiveFill = new SolidColorBrush(Color.FromArgb("#F7B1AE"));
```

## Track Height

Adjust the thickness of active and inactive track portions.

### Properties

- `ActiveSize` (double) - Height of active track (default: 8.0)
- `InactiveSize` (double) - Height of inactive track (default: 6.0)

### Example

```xaml
<sliders:SfDateTimeRangeSlider 
    Minimum="2010-01-01" 
    Maximum="2018-01-01" 
    RangeStart="2012-01-01" 
    RangeEnd="2016-01-01">
   
    <sliders:SfDateTimeRangeSlider.TrackStyle>
        <sliders:SliderTrackStyle 
            ActiveSize="10" 
            InactiveSize="8" />
    </sliders:SfDateTimeRangeSlider.TrackStyle>

</sliders:SfDateTimeRangeSlider>
```

```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2018, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2016, 01, 01);
rangeSlider.TrackStyle.ActiveSize = 10;
rangeSlider.TrackStyle.InactiveSize = 8;
```

## Track Extent

Extend the track beyond the edges using `TrackExtent` (in pixels).

### Property

- `TrackExtent` (double) - Extension in pixels (default: 0)

### Example

```xaml
<sliders:SfDateTimeRangeSlider 
    Minimum="2010-01-01"
    Maximum="2018-01-01"
    RangeStart="2012-01-01"
    RangeEnd="2016-01-01"
    Interval="2"
    ShowTicks="True"
    TrackExtent="25" />
```

```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2018, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2016, 01, 01);
rangeSlider.Interval = 2;
rangeSlider.ShowTicks = true;
rangeSlider.TrackExtent = 25;
```

**Use Case:** Extend track so labels or ticks at edges don't get cut off.

## Orientation

Display the slider horizontally or vertically.

### Property

- `Orientation` (SliderOrientation) - Horizontal (default) or Vertical

### Vertical Example

```xaml
<sliders:SfDateTimeRangeSlider 
    Minimum="2010-01-01" 
    Maximum="2018-01-01" 
    RangeStart="2012-01-01" 
    RangeEnd="2016-01-01"
    ShowTicks="True" 
    ShowLabels="True"
    Interval="2" 
    MinorTicksPerInterval="1" 
    Orientation="Vertical" />
```

```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Orientation = SliderOrientation.Vertical;
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2018, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2016, 01, 01);
rangeSlider.ShowLabels = true;
rangeSlider.ShowTicks = true;
rangeSlider.Interval = 2;
rangeSlider.MinorTicksPerInterval = 1;
```

## Inverse Direction

Reverse the direction of the slider.

### Property

- `IsInversed` (bool) - Reverse direction (default: false)

### Example

```xaml
<sliders:SfDateTimeRangeSlider 
    Minimum="2010-01-01" 
    Maximum="2018-01-01" 
    RangeStart="2012-01-01" 
    RangeEnd="2016-01-01"
    Interval="2" 
    ShowTicks="True"
    ShowLabels="True"  
    MinorTicksPerInterval="1" 
    IsInversed="True" />
```

```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2018, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2016, 01, 01);
rangeSlider.Interval = 2;
rangeSlider.ShowLabels = true;
rangeSlider.ShowTicks = true;
rangeSlider.MinorTicksPerInterval = 1;
rangeSlider.IsInversed = true;
```

**Horizontal Inversed:** Right-to-left (useful for RTL languages)  
**Vertical Inversed:** Bottom-to-top

## Disabled Track (Visual State Manager)

Customize track appearance when `IsEnabled = false` using Visual State Manager.

### Example

```xaml
<ContentPage.Resources>
    <Style TargetType="sliders:SfDateTimeRangeSlider">
        <Setter Property="Minimum" Value="2010-01-01" />
        <Setter Property="Maximum" Value="2018-01-01" />
        <Setter Property="RangeStart" Value="2012-01-01" />
        <Setter Property="RangeEnd" Value="2016-01-01" />
        <Setter Property="ThumbStyle">
            <sliders:SliderThumbStyle Radius="0" />
        </Setter>
        <Setter Property="VisualStateManager.VisualStateGroups">
            <VisualStateGroupList>
                <VisualStateGroup>
                    <VisualState x:Name="Default">
                        <VisualState.Setters>
                            <Setter Property="TrackStyle">
                                <Setter.Value>
                                    <sliders:SliderTrackStyle 
                                        ActiveSize="8"
                                        InactiveSize="6"
                                        ActiveFill="#EE3F3F"
                                        InactiveFill="#F7B1AE" />
                                </Setter.Value>
                            </Setter>
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState x:Name="Disabled">
                        <VisualState.Setters>
                            <Setter Property="TrackStyle">
                                <Setter.Value>
                                    <sliders:SliderTrackStyle 
                                        ActiveSize="10"
                                        InactiveSize="8"
                                        ActiveFill="Gray"
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
        <sliders:SfDateTimeRangeSlider />
        
        <Label Text="Disabled" Padding="24,10" />
        <sliders:SfDateTimeRangeSlider IsEnabled="False" />
    </VerticalStackLayout>
</ContentPage.Content>
```

```csharp
VerticalStackLayout stackLayout = new();
SfDateTimeRangeSlider defaultRangeSlider = new()
{
    Minimum = new DateTime(2010, 01, 01),
    Maximum = new DateTime(2018, 01, 01),
    RangeStart = new DateTime(2012, 01, 01),
    RangeEnd = new DateTime(2016, 01, 01),
};
SfDateTimeRangeSlider disabledRangeSlider = new()
{
    Minimum = new DateTime(2010, 01, 01),
    Maximum = new DateTime(2018, 01, 01),
    RangeStart = new DateTime(2012, 01, 01),
    RangeEnd = new DateTime(2016, 01, 01),
    IsEnabled = false,
};

VisualStateGroupList visualStateGroupList = new();
VisualStateGroup commonStateGroup = new();

// Default State
VisualState defaultState = new() { Name = "Default" };
defaultState.Setters.Add(new Setter
{
    Property = SfDateTimeRangeSlider.TrackStyleProperty,
    Value = new SliderTrackStyle
    {
        ActiveFill = Color.FromArgb("#EE3F3F"),
        InactiveFill = Color.FromArgb("#F7B1AE"),
        ActiveSize = 8,
        InactiveSize = 6,
    }
});

// Disabled State
VisualState disabledState = new() { Name = "Disabled" };
disabledState.Setters.Add(new Setter
{
    Property = SfDateTimeRangeSlider.TrackStyleProperty,
    Value = new SliderTrackStyle
    {
        ActiveFill = Colors.Gray,
        InactiveFill = Colors.LightGray,
        ActiveSize = 10,
        InactiveSize = 8,
    }
});

commonStateGroup.States.Add(defaultState);
commonStateGroup.States.Add(disabledState);
visualStateGroupList.Add(commonStateGroup);
VisualStateManager.SetVisualStateGroups(defaultRangeSlider, visualStateGroupList);
VisualStateManager.SetVisualStateGroups(disabledRangeSlider, visualStateGroupList);

stackLayout.Children.Add(new Label() { Text = "Enabled", Padding = new Thickness(24, 10) });
stackLayout.Children.Add(defaultRangeSlider);
stackLayout.Children.Add(new Label() { Text = "Disabled", Padding = new Thickness(24, 10) });
stackLayout.Children.Add(disabledRangeSlider);
this.Content = stackLayout;
```

## Summary

**Track Configuration Properties:**

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Minimum` | DateTime | null | Lower bound |
| `Maximum` | DateTime | null | Upper bound |
| `RangeStart` | DateTime | - | Start thumb position |
| `RangeEnd` | DateTime | - | End thumb position |
| `Orientation` | SliderOrientation | Horizontal | Layout direction |
| `IsInversed` | bool | false | Reverse direction |
| `TrackExtent` | double | 0 | Edge extension (pixels) |

**TrackStyle Properties:**

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `ActiveFill` | Brush | - | Color between thumbs |
| `InactiveFill` | Brush | - | Color outside thumbs |
| `ActiveSize` | double | 8.0 | Active track height |
| `InactiveSize` | double | 6.0 | Inactive track height |

**Next:** See [interval-and-labels.md](interval-and-labels.md) for label configuration and formatting.
