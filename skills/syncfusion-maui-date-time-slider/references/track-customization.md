# Track Customization in .NET MAUI DateTime Slider

The track is the horizontal or vertical line that represents the date range. This guide covers track colors, sizes, and styling options for the DateTime Slider.

## Track Anatomy

The DateTime Slider track has two distinct parts:

- **Active Track**: From `Minimum` to the thumb (current value)
- **Inactive Track**: From the thumb to `Maximum`

Each part can be styled independently to provide visual feedback about the selected value.

## Track Color Customization

### Basic Track Colors

Use `TrackStyle` to customize active and inactive track colors:

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2018-01-01"
                          Value="2014-01-01">
    <sliders:SfDateTimeSlider.TrackStyle>
        <sliders:SliderTrackStyle ActiveFill="#EE3F3F"
                                  InactiveFill="#F7B1AE" />
    </sliders:SfDateTimeSlider.TrackStyle>
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

slider.TrackStyle.ActiveFill = new SolidColorBrush(Color.FromArgb("#EE3F3F"));
slider.TrackStyle.InactiveFill = new SolidColorBrush(Color.FromArgb("#F7B1AE"));
```

**Visual Effect:**
- Active portion (2010-2014): Red (#EE3F3F)
- Inactive portion (2014-2018): Light pink (#F7B1AE)

### Using Theme Colors

```csharp
slider.TrackStyle.ActiveFill = new SolidColorBrush(Colors.Blue);
slider.TrackStyle.InactiveFill = new SolidColorBrush(Colors.LightBlue);
```

## Track Height/Size

Customize the thickness of active and inactive tracks:

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2020-01-01"
                          Value="2014-01-01">
    <sliders:SfDateTimeSlider.TrackStyle>
        <sliders:SliderTrackStyle ActiveSize="10"
                                  InactiveSize="8" />
    </sliders:SfDateTimeSlider.TrackStyle>
</sliders:SfDateTimeSlider>
```

**C# Implementation:**

```csharp
slider.TrackStyle.ActiveSize = 10;
slider.TrackStyle.InactiveSize = 8;
```

**Default Values:**
- `ActiveSize`: 8.0
- `InactiveSize`: 6.0

**Tip:** Making the active track thicker emphasizes the selected range.

## Track Extent

Extend the track beyond the default edges using `TrackExtent`. This adds padding at both ends of the track.

### Without TrackExtent

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2018-01-01"
                          Value="2014-01-01"
                          Interval="2"
                          ShowTicks="True" />
```

### With TrackExtent

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2018-01-01"
                          Value="2014-01-01"
                          Interval="2"
                          ShowTicks="True"
                          TrackExtent="25" />
```

**C# Implementation:**

```csharp
slider.TrackExtent = 25;
```

**Value:** Pixels to extend on each side (default: 0)

**Use Case:** Provides visual breathing room, especially when labels are placed inside with `EdgeLabelsPlacement="Inside"`.

## Complete Track Styling Example

Combine colors, sizes, and extent for a polished look:

```xaml
<sliders:SfDateTimeSlider Minimum="2020-01-01"
                          Maximum="2025-12-31"
                          Value="2023-06-15"
                          Interval="1"
                          IntervalType="Years"
                          ShowLabels="True"
                          ShowTicks="True"
                          TrackExtent="20">
    
    <sliders:SfDateTimeSlider.TrackStyle>
        <sliders:SliderTrackStyle ActiveFill="#2196F3"
                                  InactiveFill="#BBDEFB"
                                  ActiveSize="12"
                                  InactiveSize="10" />
    </sliders:SfDateTimeSlider.TrackStyle>
    
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
    ShowTicks = true,
    TrackExtent = 20
};

slider.TrackStyle.ActiveFill = new SolidColorBrush(Color.FromArgb("#2196F3"));
slider.TrackStyle.InactiveFill = new SolidColorBrush(Color.FromArgb("#BBDEFB"));
slider.TrackStyle.ActiveSize = 12;
slider.TrackStyle.InactiveSize = 10;
```

## Disabled Track State with Visual State Manager

Customize track appearance when the slider is disabled:

```xaml
<ContentPage.Resources>
    <Style TargetType="sliders:SfDateTimeSlider">
        <Setter Property="Minimum" Value="2010-01-01" />
        <Setter Property="Maximum" Value="2018-01-01" />
        <Setter Property="Value" Value="2014-01-01" />
        <Setter Property="VisualStateManager.VisualStateGroups">
            <VisualStateGroupList>
                <VisualStateGroup>
                    <!-- Enabled State (Default) -->
                    <VisualState x:Name="Default">
                        <VisualState.Setters>
                            <Setter Property="TrackStyle">
                                <Setter.Value>
                                    <sliders:SliderTrackStyle ActiveSize="8"
                                                              InactiveSize="6"
                                                              ActiveFill="#EE3F3F"
                                                              InactiveFill="#F7B1AE" />
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
var enabledSlider = new SfDateTimeSlider
{
    Minimum = new DateTime(2010, 01, 01),
    Maximum = new DateTime(2018, 01, 01),
    Value = new DateTime(2014, 01, 01)
};

var disabledSlider = new SfDateTimeSlider
{
    Minimum = new DateTime(2010, 01, 01),
    Maximum = new DateTime(2018, 01, 01),
    Value = new DateTime(2014, 01, 01),
    IsEnabled = false
};

// Configure Visual State Manager
var visualStateGroupList = new VisualStateGroupList();
var commonStateGroup = new VisualStateGroup();

// Default State
var defaultState = new VisualState { Name = "Default" };
defaultState.Setters.Add(new Setter
{
    Property = SfDateTimeSlider.TrackStyleProperty,
    Value = new SliderTrackStyle
    {
        ActiveFill = Color.FromArgb("#EE3F3F"),
        InactiveFill = Color.FromArgb("#F7B1AE"),
        ActiveSize = 8,
        InactiveSize = 6
    }
});

// Disabled State
var disabledState = new VisualState { Name = "Disabled" };
disabledState.Setters.Add(new Setter
{
    Property = SfDateTimeSlider.TrackStyleProperty,
    Value = new SliderTrackStyle
    {
        ActiveFill = Colors.Gray,
        InactiveFill = Colors.LightGray,
        ActiveSize = 10,
        InactiveSize = 8
    }
});

commonStateGroup.States.Add(defaultState);
commonStateGroup.States.Add(disabledState);
visualStateGroupList.Add(commonStateGroup);

VisualStateManager.SetVisualStateGroups(enabledSlider, visualStateGroupList);
VisualStateManager.SetVisualStateGroups(disabledSlider, visualStateGroupList);
```

## TrackStyle Properties Reference

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `ActiveFill` | Brush | Theme default | Color of active track (Minimum to thumb) |
| `InactiveFill` | Brush | Theme default | Color of inactive track (thumb to Maximum) |
| `ActiveSize` | double | 8.0 | Height (horizontal) or width (vertical) of active track |
| `InactiveSize` | double | 6.0 | Height (horizontal) or width (vertical) of inactive track |

## Common Patterns

### Pattern 1: Emphasized Active Track

Make the selected portion stand out:

```csharp
slider.TrackStyle.ActiveFill = new SolidColorBrush(Colors.Green);
slider.TrackStyle.InactiveFill = new SolidColorBrush(Colors.LightGray);
slider.TrackStyle.ActiveSize = 14;
slider.TrackStyle.InactiveSize = 6;
```

### Pattern 2: Gradient Effect (Using Multiple Sliders)

For a gradient appearance, layer multiple sliders with opacity:

```xaml
<Grid>
    <!-- Background gradient slider -->
    <sliders:SfDateTimeSlider Minimum="2020-01-01"
                              Maximum="2025-12-31"
                              Value="2023-06-15"
                              IsEnabled="False"
                              Opacity="0.3">
        <sliders:SfDateTimeSlider.TrackStyle>
            <sliders:SliderTrackStyle ActiveFill="Blue" InactiveFill="LightBlue" />
        </sliders:SfDateTimeSlider.TrackStyle>
    </sliders:SfDateTimeSlider>
    
    <!-- Interactive slider -->
    <sliders:SfDateTimeSlider Minimum="2020-01-01"
                              Maximum="2025-12-31"
                              Value="2023-06-15">
        <sliders:SfDateTimeSlider.TrackStyle>
            <sliders:SliderTrackStyle ActiveFill="DarkBlue" InactiveFill="Transparent" />
        </sliders:SfDateTimeSlider.TrackStyle>
    </sliders:SfDateTimeSlider>
</Grid>
```

### Pattern 3: Minimal Track

Subtle, thin track for compact UIs:

```csharp
slider.TrackStyle.ActiveSize = 4;
slider.TrackStyle.InactiveSize = 2;
slider.TrackStyle.ActiveFill = new SolidColorBrush(Color.FromArgb("#333333"));
slider.TrackStyle.InactiveFill = new SolidColorBrush(Color.FromArgb("#E0E0E0"));
```

## Best Practices

1. **Contrast**: Ensure sufficient contrast between active and inactive tracks
2. **Accessibility**: Use colors that are distinguishable for colorblind users
3. **Size Ratio**: Keep `ActiveSize` ≥ `InactiveSize` for better visual hierarchy
4. **Theme Consistency**: Match track colors with your app's color scheme
5. **Disabled State**: Always provide visual feedback when slider is disabled

## Next Steps

- **Labels**: Add and customize date labels
- **Ticks & Dividers**: Add visual markers at intervals
- **Thumb Styling**: Customize the draggable thumb appearance
