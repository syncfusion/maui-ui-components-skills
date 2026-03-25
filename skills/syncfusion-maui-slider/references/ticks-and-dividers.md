# Ticks and Dividers

This guide covers tick marks and dividers configuration for the .NET MAUI Slider, including major ticks, minor ticks, and divider styling.

## Table of Contents
- [Show Major Ticks](#show-major-ticks)
- [Show Minor Ticks](#show-minor-ticks)
- [Tick Styling](#tick-styling)
- [Show Dividers](#show-dividers)
- [Divider Radius](#divider-radius)
- [Divider Colors](#divider-colors)
- [Divider Stroke](#divider-stroke)
- [Disabled Dividers with Visual State Manager](#disabled-dividers-with-visual-state-manager)
- [Complete Examples](#complete-examples)

## Show Major Ticks

Major ticks are shapes that appear at interval points along the track. They visually indicate the major interval divisions.

### Enabling Major Ticks

The `ShowTicks` property enables major ticks. Default value is `False`.

**Without Interval (Auto-calculated):**

```xml
<sliders:SfSlider ShowTicks="True" />
```

```csharp
SfSlider slider = new SfSlider
{
    ShowTicks = true
};
```

The ticks will be rendered at automatically calculated intervals.

**With Interval:**

```xml
<sliders:SfSlider Interval="0.2"
                  ShowTicks="True" />
```

```csharp
SfSlider slider = new SfSlider
{
    Interval = 0.2,
    ShowTicks = true
};
```

### Tick Positioning

For example, if:
- Minimum = 0.0
- Maximum = 10.0  
- Interval = 2.0

Major ticks will render at: 0.0, 2.0, 4.0, 6.0, 8.0, 10.0

### Ticks with Labels

Combine ticks with labels for better clarity:

```xml
<sliders:SfSlider Minimum="0"
                  Maximum="10"
                  Value="5"
                  Interval="2"
                  ShowTicks="True"
                  ShowLabels="True" />
```

## Show Minor Ticks

Minor ticks are smaller tick marks between major ticks, providing finer visual granularity.

### MinorTicksPerInterval Property

The `MinorTicksPerInterval` property specifies how many minor ticks appear between each major tick. Default value is `0`.

**Without Interval (Auto-calculated):**

```xml
<sliders:SfSlider ShowTicks="True"
                  MinorTicksPerInterval="4" />
```

```csharp
SfSlider slider = new SfSlider
{
    ShowTicks = true,
    MinorTicksPerInterval = 4
};
```

**With Interval:**

```xml
<sliders:SfSlider Interval="0.25"
                  ShowTicks="True"
                  MinorTicksPerInterval="1" />
```

```csharp
SfSlider slider = new SfSlider
{
    Interval = 0.25,
    ShowTicks = true,
    MinorTicksPerInterval = 1
};
```

### Minor Tick Calculation

If:
- Minimum = 0.0
- Maximum = 10.0
- Interval = 2.0
- MinorTicksPerInterval = 1

**Major ticks** at: 0.0, 2.0, 4.0, 6.0, 8.0, 10.0  
**Minor ticks** at: 1.0, 3.0, 5.0, 7.0, 9.0

With `MinorTicksPerInterval = 3`:
- Between 0.0 and 2.0: minor ticks at 0.5, 1.0, 1.5
- Between 2.0 and 4.0: minor ticks at 2.5, 3.0, 3.5
- And so on...

### Use Cases

- **MinorTicksPerInterval = 0**: No minor ticks (clean look)
- **MinorTicksPerInterval = 1**: One minor tick between majors (common)
- **MinorTicksPerInterval = 4**: Four minor ticks between majors (precise)

## Tick Styling

Customize tick appearance using `MajorTickStyle` and `MinorTickStyle`.

### Major Tick Style

```xml
<sliders:SfSlider Interval="0.25" ShowTicks="True">
    <sliders:SfSlider.MajorTickStyle>
        <sliders:SliderTickStyle ActiveSize="10"
                                 InactiveSize="10"
                                 ActiveFill="#EE3F3F"
                                 InactiveFill="#88EE3F3F" />
    </sliders:SfSlider.MajorTickStyle>
</sliders:SfSlider>
```

```csharp
SfSlider slider = new SfSlider
{
    Interval = 0.25,
    ShowTicks = true
};

slider.MajorTickStyle.ActiveSize = 10;
slider.MajorTickStyle.InactiveSize = 10;
slider.MajorTickStyle.ActiveFill = new SolidColorBrush(Color.FromArgb("#EE3F3F"));
slider.MajorTickStyle.InactiveFill = new SolidColorBrush(Color.FromArgb("#88EE3F3F"));
```

### Minor Tick Style

```xml
<sliders:SfSlider Interval="0.25" ShowTicks="True" MinorTicksPerInterval="1">
    <sliders:SfSlider.MinorTickStyle>
        <sliders:SliderTickStyle ActiveSize="6"
                                 InactiveSize="6"
                                 ActiveFill="#EE3F3F"
                                 InactiveFill="#88EE3F3F" />
    </sliders:SfSlider.MinorTickStyle>
</sliders:SfSlider>
```

```csharp
slider.MinorTickStyle.ActiveSize = 6;
slider.MinorTickStyle.InactiveSize = 6;
slider.MinorTickStyle.ActiveFill = new SolidColorBrush(Color.FromArgb("#EE3F3F"));
slider.MinorTickStyle.InactiveFill = new SolidColorBrush(Color.FromArgb("#88EE3F3F"));
```

### SliderTickStyle Properties

- **ActiveSize**: Length of ticks in active region
- **InactiveSize**: Length of ticks in inactive region
- **ActiveFill**: Color of ticks in active region
- **InactiveFill**: Color of ticks in inactive region

**Active region**: From Minimum to current Value  
**Inactive region**: From current Value to Maximum

## Show Dividers

Dividers are circular markers at interval points along the track. They provide a different visual style than ticks.

### Enabling Dividers

The `ShowDividers` property renders dividers at interval points. Default value is `False`.

```xml
<sliders:SfSlider Interval="0.25"
                  ShowDividers="True" />
```

```csharp
SfSlider slider = new SfSlider
{
    Interval = 0.25,
    ShowDividers = true
};
```

### Divider Positioning

For example, if:
- Minimum = 0.0
- Maximum = 10.0
- Interval = 2.0

Dividers will render at: 0.0, 2.0, 4.0, 6.0, 8.0, 10.0

### Dividers vs Ticks

- **Ticks**: Line markers (vertical or horizontal)
- **Dividers**: Circular markers
- Can use both simultaneously or choose one

```xml
<!-- Both ticks and dividers -->
<sliders:SfSlider Interval="2"
                  ShowTicks="True"
                  ShowDividers="True"
                  ShowLabels="True" />
```

## Divider Radius

Control divider size using `ActiveRadius` and `InactiveRadius`.

### Setting Divider Radius

```xml
<sliders:SfSlider Interval="0.25" ShowDividers="True">
    <sliders:SfSlider.DividerStyle>
        <sliders:SliderDividerStyle ActiveRadius="3"
                                    InactiveRadius="7" />
    </sliders:SfSlider.DividerStyle>
</sliders:SfSlider>
```

```csharp
SfSlider slider = new SfSlider
{
    Interval = 0.25,
    ShowDividers = true
};

slider.DividerStyle.ActiveRadius = 3;
slider.DividerStyle.InactiveRadius = 7;
```

### Radius Guidelines

- **Small**: 2-4 pixels (subtle markers)
- **Medium**: 5-7 pixels (standard)
- **Large**: 8-10 pixels (prominent markers)

**Common pattern**: Smaller active radius, larger inactive radius for visual emphasis on inactive region.

## Divider Colors

Customize divider colors using `ActiveFill` and `InactiveFill`.

### Setting Colors

```xml
<sliders:SfSlider Interval="0.25" ShowDividers="True">
    <sliders:SfSlider.DividerStyle>
        <sliders:SliderDividerStyle ActiveRadius="7"
                                    InactiveRadius="7"
                                    ActiveFill="#EE3F3F"
                                    InactiveFill="#F7B1AE" />
    </sliders:SfSlider.DividerStyle>
</sliders:SfSlider>
```

```csharp
slider.DividerStyle.ActiveRadius = 7;
slider.DividerStyle.InactiveRadius = 7;
slider.DividerStyle.ActiveFill = new SolidColorBrush(Color.FromArgb("#EE3F3F"));
slider.DividerStyle.InactiveFill = new SolidColorBrush(Color.FromArgb("#F7B1AE"));
```

### Color Combinations

**Example 1: High Contrast**
```xml
<sliders:SliderDividerStyle ActiveFill="#FF4081"
                            InactiveFill="#E0E0E0" />
```

**Example 2: Monochrome with Transparency**
```xml
<sliders:SliderDividerStyle ActiveFill="#2196F3"
                            InactiveFill="#882196F3" />
```

**Example 3: Brand Colors**
```csharp
slider.DividerStyle.ActiveFill = new SolidColorBrush(Colors.Purple);
slider.DividerStyle.InactiveFill = new SolidColorBrush(Colors.LightGray);
```

## Divider Stroke

Add borders to dividers using stroke properties.

### Stroke Thickness

Control border width with `ActiveStrokeThickness` and `InactiveStrokeThickness`:

```xml
<sliders:SfSlider Interval="0.25" ShowDividers="True">
    <sliders:SfSlider.DividerStyle>
        <sliders:SliderDividerStyle ActiveRadius="7"
                                    InactiveRadius="7"
                                    ActiveFill="#EE3F3F"
                                    InactiveFill="#F7B1AE"
                                    ActiveStrokeThickness="2"
                                    InactiveStrokeThickness="2"
                                    ActiveStroke="#FFD700"
                                    InactiveStroke="#FFD700" />
    </sliders:SfSlider.DividerStyle>
</sliders:SfSlider>
```

```csharp
slider.DividerStyle.ActiveRadius = 7;
slider.DividerStyle.InactiveRadius = 7;
slider.DividerStyle.ActiveFill = new SolidColorBrush(Color.FromArgb("#EE3F3F"));
slider.DividerStyle.InactiveFill = new SolidColorBrush(Color.FromArgb("#F7B1AE"));
slider.DividerStyle.ActiveStrokeThickness = 2;
slider.DividerStyle.InactiveStrokeThickness = 2;
slider.DividerStyle.ActiveStroke = new SolidColorBrush(Color.FromArgb("#FFD700"));
slider.DividerStyle.InactiveStroke = new SolidColorBrush(Color.FromArgb("#FFD700"));
```

### SliderDividerStyle Properties

- **ActiveRadius**: Radius of dividers in active region
- **InactiveRadius**: Radius of dividers in inactive region
- **ActiveFill**: Fill color of dividers in active region
- **InactiveFill**: Fill color of dividers in inactive region
- **ActiveStroke**: Border color of dividers in active region
- **InactiveStroke**: Border color of dividers in inactive region
- **ActiveStrokeThickness**: Border width of dividers in active region
- **InactiveStrokeThickness**: Border width of dividers in inactive region

### Creating Outlined Dividers

```xml
<sliders:SliderDividerStyle ActiveRadius="8"
                            InactiveRadius="8"
                            ActiveFill="Transparent"
                            InactiveFill="Transparent"
                            ActiveStroke="#FF6B6B"
                            InactiveStroke="#BDBDBD"
                            ActiveStrokeThickness="3"
                            InactiveStrokeThickness="2" />
```

This creates hollow circle dividers with colored borders.

## Disabled Dividers with Visual State Manager

Use Visual State Manager (VSM) to customize divider appearance in disabled state.

### Complete Example

```xml
<ContentPage.Resources>
    <Style TargetType="sliders:SfSlider">
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
        <sliders:SfSlider />
        
        <Label Text="Disabled" Padding="24,10" />
        <sliders:SfSlider IsEnabled="False" />
    </VerticalStackLayout>
</ContentPage.Content>
```

### C# Implementation

```csharp
VerticalStackLayout stackLayout = new();
SfSlider enabledSlider = new()
{
    Interval = 0.25,
    ShowDividers = true
};
SfSlider disabledSlider = new()
{
    IsEnabled = false,
    Interval = 0.25,
    ShowDividers = true
};

VisualStateGroupList visualStateGroupList = new();
VisualStateGroup commonStateGroup = new();

// Default State
VisualState enabledState = new() { Name = "Default" };
enabledState.Setters.Add(new Setter
{
    Property = SfSlider.DividerStyleProperty,
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
    Property = SfSlider.DividerStyleProperty,
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
    Property = SfSlider.TrackStyleProperty,
    Value = new SliderTrackStyle
    {
        ActiveFill = Colors.Gray,
        InactiveFill = Colors.LightGray
    }
});
disabledState.Setters.Add(new Setter
{
    Property = SfSlider.ThumbStyleProperty,
    Value = new SliderThumbStyle { Fill = Colors.Gray }
});

commonStateGroup.States.Add(enabledState);
commonStateGroup.States.Add(disabledState);
visualStateGroupList.Add(commonStateGroup);
VisualStateManager.SetVisualStateGroups(enabledSlider, visualStateGroupList);
VisualStateManager.SetVisualStateGroups(disabledSlider, visualStateGroupList);

stackLayout.Children.Add(new Label { Text = "Enabled", Padding = new Thickness(24, 10) });
stackLayout.Children.Add(enabledSlider);
stackLayout.Children.Add(new Label { Text = "Disabled", Padding = new Thickness(24, 10) });
stackLayout.Children.Add(disabledSlider);

Content = stackLayout;
```

## Complete Examples

### Example 1: Slider with Ticks and Labels

```xml
<sliders:SfSlider Minimum="0"
                  Maximum="100"
                  Value="60"
                  Interval="20"
                  ShowTicks="True"
                  ShowLabels="True"
                  MinorTicksPerInterval="1">
    <sliders:SfSlider.MajorTickStyle>
        <sliders:SliderTickStyle ActiveSize="8"
                                 InactiveSize="8"
                                 ActiveFill="#4CAF50"
                                 InactiveFill="#C8E6C9" />
    </sliders:SfSlider.MajorTickStyle>
    <sliders:SfSlider.MinorTickStyle>
        <sliders:SliderTickStyle ActiveSize="5"
                                 InactiveSize="5"
                                 ActiveFill="#4CAF50"
                                 InactiveFill="#C8E6C9" />
    </sliders:SfSlider.MinorTickStyle>
</sliders:SfSlider>
```

### Example 2: Slider with Dividers

```xml
<sliders:SfSlider Minimum="0"
                  Maximum="10"
                  Value="5"
                  Interval="2"
                  ShowDividers="True"
                  ShowLabels="True">
    <sliders:SfSlider.DividerStyle>
        <sliders:SliderDividerStyle ActiveRadius="6"
                                    InactiveRadius="6"
                                    ActiveFill="#FF6B6B"
                                    InactiveFill="#FFE5E5"
                                    ActiveStroke="White"
                                    InactiveStroke="White"
                                    ActiveStrokeThickness="2"
                                    InactiveStrokeThickness="1" />
    </sliders:SfSlider.DividerStyle>
</sliders:SfSlider>
```

### Example 3: Combined Ticks, Dividers, and Labels

```xml
<sliders:SfSlider Minimum="0"
                  Maximum="100"
                  Value="75"
                  Interval="25"
                  ShowTicks="True"
                  ShowDividers="True"
                  ShowLabels="True"
                  NumberFormat="0'%'"
                  MinorTicksPerInterval="4">
    <sliders:SfSlider.TrackStyle>
        <sliders:SliderTrackStyle ActiveFill="#2196F3"
                                  InactiveFill="#BBDEFB" />
    </sliders:SfSlider.TrackStyle>
    <sliders:SfSlider.MajorTickStyle>
        <sliders:SliderTickStyle ActiveSize="10"
                                 InactiveSize="10" />
    </sliders:SfSlider.MajorTickStyle>
    <sliders:SfSlider.MinorTickStyle>
        <sliders:SliderTickStyle ActiveSize="6"
                                 InactiveSize="6" />
    </sliders:SfSlider.MinorTickStyle>
    <sliders:SfSlider.DividerStyle>
        <sliders:SliderDividerStyle ActiveRadius="5"
                                    InactiveRadius="5"
                                    ActiveFill="#2196F3"
                                    InactiveFill="#E3F2FD" />
    </sliders:SfSlider.DividerStyle>
</sliders:SfSlider>
```

## Best Practices

### Choosing Between Ticks and Dividers
- **Ticks**: Better for precise measurements, technical interfaces
- **Dividers**: Better for visual appeal, consumer apps
- **Both**: Use sparingly to avoid visual clutter

### Interval Configuration
- Match interval with the granularity needed
- Avoid too many markers (cluttered)
- Use MinorTicksPerInterval for finer detail

### Styling Guidelines
- Active elements should have stronger colors than inactive
- Keep tick/divider sizes proportional to track thickness
- Use consistent styling across all sliders in your app

### Performance
- Limit the number of ticks/dividers (avoid Interval = 1 on large ranges)
- Use auto interval for responsive designs
- Prefer dividers over ticks if both achieve the same visual result (simpler rendering)

## Troubleshooting

### Issue: Ticks Not Showing

**Cause**: ShowTicks is False or Interval not set  
**Solution**:
```xml
<sliders:SfSlider ShowTicks="True" Interval="10" />
```

### Issue: Minor Ticks Not Visible

**Cause**: MinorTicksPerInterval is 0 or minor tick size too small  
**Solution**:
```xml
<sliders:SfSlider ShowTicks="True" MinorTicksPerInterval="1">
    <sliders:SfSlider.MinorTickStyle>
        <sliders:SliderTickStyle ActiveSize="6" InactiveSize="6" />
    </sliders:SfSlider.MinorTickStyle>
</sliders:SfSlider>
```

### Issue: Dividers Not Appearing

**Cause**: ShowDividers is False or radius is 0  
**Solution**:
```xml
<sliders:SfSlider ShowDividers="True" Interval="10">
    <sliders:SfSlider.DividerStyle>
        <sliders:SliderDividerStyle ActiveRadius="5" InactiveRadius="5" />
    </sliders:SfSlider.DividerStyle>
</sliders:SfSlider>
```

## Summary

Key properties for ticks and dividers:

**Ticks:**
- `ShowTicks`: Enable major ticks
- `MinorTicksPerInterval`: Number of minor ticks between majors
- `MajorTickStyle`: Style for major ticks (size, color)
- `MinorTickStyle`: Style for minor ticks (size, color)

**Dividers:**
- `ShowDividers`: Enable dividers
- `DividerStyle.ActiveRadius/InactiveRadius`: Size of dividers
- `DividerStyle.ActiveFill/InactiveFill`: Fill colors
- `DividerStyle.ActiveStroke/InactiveStroke`: Border colors
- `DividerStyle.ActiveStrokeThickness/InactiveStrokeThickness`: Border width

Use Visual State Manager for disabled state customization.
