# Track Customization in .NET MAUI Range Slider

## Table of Contents
- [Overview](#overview)
- [Range Values](#range-values)
- [Minimum and Maximum](#minimum-and-maximum)
- [Track Colors](#track-colors)
- [Track Height](#track-height)
- [Track Extent](#track-extent)
- [Orientation](#orientation)
- [Inverse Direction](#inverse-direction)
- [Disabled Track](#disabled-track)

## Overview

The track is the horizontal or vertical line on which thumbs slide to select range values. This guide covers customizing track appearance, dimensions, colors, and behavior.

## Range Values

The `RangeStart` and `RangeEnd` properties represent the currently selected range values. The slider draws thumbs at these positions.

### Setting Range Values

**XAML:**
```xaml
<sliders:SfRangeSlider Minimum="0"
                       Maximum="10"
                       RangeStart="3"
                       RangeEnd="7"
                       ShowLabels="True" />
```

**C#:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    Minimum = 0,
    Maximum = 10,
    RangeStart = 3,  // Start thumb position
    RangeEnd = 7,    // End thumb position
    ShowLabels = true
};
```

**Result**: Thumbs positioned at 3 and 7, with active track between them.

### Two-Way Binding

Bind RangeStart and RangeEnd for MVVM scenarios:

**XAML:**
```xaml
<sliders:SfRangeSlider Minimum="0"
                       Maximum="100"
                       RangeStart="{Binding MinPrice}"
                       RangeEnd="{Binding MaxPrice}"
                       ShowLabels="True" />
```

**ViewModel:**
```csharp
public class PriceFilterViewModel : INotifyPropertyChanged
{
    private double minPrice = 20;
    private double maxPrice = 80;
    
    public double MinPrice
    {
        get => minPrice;
        set
        {
            minPrice = value;
            OnPropertyChanged();
        }
    }
    
    public double MaxPrice
    {
        get => maxPrice;
        set
        {
            maxPrice = value;
            OnPropertyChanged();
        }
    }
    
    // INotifyPropertyChanged implementation...
}
```

## Minimum and Maximum

The `Minimum` and `Maximum` properties define the value range of the slider.

### Default Values
- **Minimum**: 0 (default)
- **Maximum**: 1 (default)

### Setting Custom Range

**XAML:**
```xaml
<!-- Age range selector -->
<sliders:SfRangeSlider Minimum="18"
                       Maximum="65"
                       RangeStart="25"
                       RangeEnd="45"
                       ShowLabels="True" />
```

**C#:**
```csharp
// Temperature range (Celsius)
SfRangeSlider rangeSlider = new SfRangeSlider
{
    Minimum = -10,
    Maximum = 40,
    RangeStart = 18,
    RangeEnd = 24,
    ShowLabels = true
};
```

**Important**: 
- Minimum must be less than Maximum
- RangeStart must be >= Minimum
- RangeEnd must be <= Maximum
- RangeStart must be <= RangeEnd

## Track Colors

Customize active and inactive track colors using the `TrackStyle` property.

### Active vs Inactive Regions

- **Active region**: Between start and end thumbs
- **Inactive regions**: From Minimum to start thumb, and from end thumb to Maximum

### Setting Track Colors

**XAML:**
```xaml
<sliders:SfRangeSlider Minimum="0"
                       Maximum="10"
                       RangeStart="3"
                       RangeEnd="7">
    <sliders:SfRangeSlider.TrackStyle>
        <sliders:SliderTrackStyle ActiveFill="#2196F3"
                                  InactiveFill="#E0E0E0" />
    </sliders:SfRangeSlider.TrackStyle>
</sliders:SfRangeSlider>
```

**C#:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    Minimum = 0,
    Maximum = 10,
    RangeStart = 3,
    RangeEnd = 7
};

rangeSlider.TrackStyle.ActiveFill = new SolidColorBrush(Color.FromArgb("#2196F3"));
rangeSlider.TrackStyle.InactiveFill = new SolidColorBrush(Color.FromArgb("#E0E0E0"));
```

### Gradient Track Colors

```csharp
// Gradient active track
rangeSlider.TrackStyle.ActiveFill = new LinearGradientBrush
{
    StartPoint = new Point(0, 0),
    EndPoint = new Point(1, 0),
    GradientStops = new GradientStopCollection
    {
        new GradientStop { Color = Colors.Blue, Offset = 0.0f },
        new GradientStop { Color = Colors.Purple, Offset = 1.0f }
    }
};
```

## Track Height

Control track thickness using `ActiveSize` and `InactiveSize` properties.

### Default Sizes
- **ActiveSize**: 8.0 (default)
- **InactiveSize**: 6.0 (default)

### Setting Track Height

**XAML:**
```xaml
<sliders:SfRangeSlider>
    <sliders:SfRangeSlider.TrackStyle>
        <sliders:SliderTrackStyle ActiveSize="12"
                                  InactiveSize="8" />
    </sliders:SfRangeSlider.TrackStyle>
</sliders:SfRangeSlider>
```

**C#:**
```csharp
rangeSlider.TrackStyle.ActiveSize = 12;
rangeSlider.TrackStyle.InactiveSize = 8;
```

### Consistent Track Height

Make active and inactive regions the same height:

```csharp
rangeSlider.TrackStyle.ActiveSize = 10;
rangeSlider.TrackStyle.InactiveSize = 10;
```

## Track Extent

The `TrackExtent` property extends the track beyond the first and last tick/label positions.

### Default Value
- **TrackExtent**: 0 (no extension)

### Setting Track Extent

**XAML:**
```xaml
<sliders:SfRangeSlider Interval="0.25"
                       ShowTicks="True"
                       ShowLabels="True"
                       TrackExtent="25" />
```

**C#:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    Interval = 0.25,
    ShowTicks = true,
    ShowLabels = true,
    TrackExtent = 25  // Extend track by 25 pixels on each side
};
```

**Use case**: When labels need extra padding from screen edges, or for visual balance with custom styling.

## Orientation

The `Orientation` property controls whether the slider is horizontal or vertical.

### Available Orientations
- **Horizontal** (default): Left-to-right slider
- **Vertical**: Bottom-to-top slider

### Horizontal Slider (Default)

**XAML:**
```xaml
<sliders:SfRangeSlider Minimum="0"
                       Maximum="10"
                       RangeStart="3"
                       RangeEnd="7"
                       ShowLabels="True"
                       ShowTicks="True"
                       Orientation="Horizontal" />
```

### Vertical Slider

**XAML:**
```xaml
<sliders:SfRangeSlider Minimum="0"
                       Maximum="10"
                       RangeStart="3"
                       RangeEnd="7"
                       ShowLabels="True"
                       ShowTicks="True"
                       Orientation="Vertical"
                       HeightRequest="300" />
```

**C#:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    Orientation = SliderOrientation.Vertical,
    HeightRequest = 300,
    Minimum = 0,
    Maximum = 10,
    RangeStart = 3,
    RangeEnd = 7,
    ShowLabels = true,
    ShowTicks = true
};
```

**Important**: 
- For vertical sliders, set `HeightRequest` to control slider height
- For horizontal sliders, set `WidthRequest` if needed

### Use Cases
- **Horizontal**: Standard controls, desktop layouts
- **Vertical**: Volume controls, thermostats, mobile layouts with limited horizontal space

## Inverse Direction

The `IsInversed` property reverses the slider direction.

### Default Behavior
- **Horizontal**: Minimum at left, Maximum at right
- **Vertical**: Minimum at bottom, Maximum at top

### Inversed Behavior
- **Horizontal**: Minimum at right, Maximum at left
- **Vertical**: Minimum at top, Maximum at bottom

### Setting Inverse Direction

**XAML:**
```xaml
<sliders:SfRangeSlider Minimum="0"
                       Maximum="10"
                       RangeStart="3"
                       RangeEnd="7"
                       ShowLabels="True"
                       ShowTicks="True"
                       IsInversed="True" />
```

**C#:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    Minimum = 0,
    Maximum = 10,
    RangeStart = 3,
    RangeEnd = 7,
    ShowLabels = true,
    ShowTicks = true,
    IsInversed = true
};
```

**Use cases**:
- RTL (Right-to-Left) language support
- Countdown timers or countdown displays
- Temperature scales showing higher values at bottom

## Disabled Track

Disable user interaction and customize disabled appearance using Visual State Manager (VSM).

### Disabling the Slider

**XAML:**
```xaml
<sliders:SfRangeSlider IsEnabled="False" />
```

**C#:**
```csharp
rangeSlider.IsEnabled = false;
```

### Customizing Disabled State with VSM

**XAML:**
```xaml
<ContentPage.Resources>
    <Style TargetType="sliders:SfRangeSlider">
        <Setter Property="Interval" Value="0.25" />
        <Setter Property="VisualStateManager.VisualStateGroups">
            <VisualStateGroupList>
                <VisualStateGroup>
                    <!-- Default (Enabled) State -->
                    <VisualState x:Name="Default">
                        <VisualState.Setters>
                            <Setter Property="TrackStyle">
                                <Setter.Value>
                                    <sliders:SliderTrackStyle ActiveSize="8"
                                                              InactiveSize="6"
                                                              ActiveFill="#2196F3"
                                                              InactiveFill="#E0E0E0" />
                                </Setter.Value>
                            </Setter>
                        </VisualState.Setters>
                    </VisualState>
                    
                    <!-- Disabled State -->
                    <VisualState x:Name="Disabled">
                        <VisualState.Setters>
                            <Setter Property="TrackStyle">
                                <Setter.Value>
                                    <sliders:SliderTrackStyle ActiveSize="10"
                                                              InactiveSize="8"
                                                              ActiveFill="Gray"
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

<VerticalStackLayout>
    <Label Text="Enabled" Padding="24,10" />
    <sliders:SfRangeSlider />
    
    <Label Text="Disabled" Padding="24,10" />
    <sliders:SfRangeSlider IsEnabled="False" />
</VerticalStackLayout>
```

**C#:**
```csharp
VerticalStackLayout stackLayout = new VerticalStackLayout();
SfRangeSlider enabledSlider = new SfRangeSlider { Interval = 0.25 };
SfRangeSlider disabledSlider = new SfRangeSlider { IsEnabled = false, Interval = 0.25 };

VisualStateGroupList visualStateGroupList = new VisualStateGroupList();
VisualStateGroup commonStateGroup = new VisualStateGroup();

// Default State
VisualState defaultState = new VisualState { Name = "Default" };
defaultState.Setters.Add(new Setter
{
    Property = SfRangeSlider.TrackStyleProperty,
    Value = new SliderTrackStyle
    {
        ActiveFill = Color.FromArgb("#2196F3"),
        InactiveFill = Color.FromArgb("#E0E0E0"),
        ActiveSize = 8,
        InactiveSize = 6
    }
});

// Disabled State
VisualState disabledState = new VisualState { Name = "Disabled" };
disabledState.Setters.Add(new Setter
{
    Property = SfRangeSlider.TrackStyleProperty,
    Value = new SliderTrackStyle
    {
        ActiveFill = Colors.Gray,
        InactiveFill = Colors.LightGray,
        ActiveSize = 10,
        InactiveSize = 8
    }
});
disabledState.Setters.Add(new Setter
{
    Property = SfRangeSlider.ThumbStyleProperty,
    Value = new SliderThumbStyle { Fill = Colors.Gray }
});

commonStateGroup.States.Add(defaultState);
commonStateGroup.States.Add(disabledState);
visualStateGroupList.Add(commonStateGroup);

VisualStateManager.SetVisualStateGroups(enabledSlider, visualStateGroupList);
VisualStateManager.SetVisualStateGroups(disabledSlider, visualStateGroupList);

stackLayout.Children.Add(new Label { Text = "Enabled", Padding = new Thickness(24, 10) });
stackLayout.Children.Add(enabledSlider);
stackLayout.Children.Add(new Label { Text = "Disabled", Padding = new Thickness(24, 10) });
stackLayout.Children.Add(disabledSlider);

this.Content = stackLayout;
```

## Common Scenarios

### Price Range with Custom Colors

```xaml
<sliders:SfRangeSlider Minimum="0"
                       Maximum="1000"
                       RangeStart="200"
                       RangeEnd="800"
                       Interval="100"
                       ShowLabels="True"
                       NumberFormat="$#">
    <sliders:SfRangeSlider.TrackStyle>
        <sliders:SliderTrackStyle ActiveFill="#4CAF50"
                                  InactiveFill="#BDBDBD"
                                  ActiveSize="10"
                                  InactiveSize="6" />
    </sliders:SfRangeSlider.TrackStyle>
</sliders:SfRangeSlider>
```

### Vertical Volume Control

```csharp
SfRangeSlider volumeSlider = new SfRangeSlider
{
    Orientation = SliderOrientation.Vertical,
    HeightRequest = 250,
    Minimum = 0,
    Maximum = 100,
    RangeStart = 30,
    RangeEnd = 70,
    Interval = 10,
    ShowLabels = true,
    ShowTicks = true
};

volumeSlider.TrackStyle.ActiveFill = new SolidColorBrush(Colors.Orange);
volumeSlider.TrackStyle.InactiveFill = new SolidColorBrush(Colors.LightGray);
```

## Best Practices

1. **Set meaningful ranges**: Choose Minimum/Maximum that match your data domain
2. **Balance track sizes**: Don't make active/inactive size differences too extreme
3. **Use consistent colors**: Match track colors to your app's theme
4. **Test both orientations**: Ensure your layout works for intended orientation
5. **Handle disabled state**: Provide clear visual feedback when slider is disabled
6. **Consider accessibility**: Ensure sufficient color contrast for visually impaired users

## Related Properties

- **Labels**: See [labels.md](labels.md) for label customization
- **Ticks**: See [ticks.md](ticks.md) for tick mark styling
- **Thumbs**: See [thumbs-and-overlays.md](thumbs-and-overlays.md) for thumb appearance
- **Selection**: See [intervals-and-selection.md](intervals-and-selection.md) for interaction behavior
