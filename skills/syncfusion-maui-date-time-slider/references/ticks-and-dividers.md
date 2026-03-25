# Ticks and Dividers in .NET MAUI DateTime Slider

## Table of Contents
- [Overview](#overview)
- [Major Ticks](#major-ticks)
- [Minor Ticks](#minor-ticks)
- [Tick Styling](#tick-styling)
- [Tick Offset](#tick-offset)
- [Dividers](#dividers)
- [Divider Styling](#divider-styling)
- [Disabled States](#disabled-states)

## Overview

Ticks and dividers are visual indicators that mark intervals along the slider track:

- **Major Ticks**: Appear at main intervals (e.g., every year)
- **Minor Ticks**: Appear between major ticks (e.g., every quarter)
- **Dividers**: Circular markers at major interval points

## Major Ticks

### Enable Major Ticks

Use `ShowTicks` property to display major ticks at intervals:

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2018-01-01"
                          Value="2014-01-01"
                          ShowTicks="True" />
```

**Without Interval (Auto-spaced):**

Ticks automatically distribute across the range.

**With Interval:**

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2020-01-01"
                          Value="2015-01-01"
                          Interval="2"
                          ShowTicks="True" />
```

**Result:** Major ticks at 2010, 2012, 2014, 2016, 2018, 2020

**C# Implementation:**

```csharp
var slider = new SfDateTimeSlider
{
    Minimum = new DateTime(2010, 01, 01),
    Maximum = new DateTime(2020, 01, 01),
    Value = new DateTime(2015, 01, 01),
    Interval = 2,
    ShowTicks = true
};
```

**Default:** `ShowTicks = false`

## Minor Ticks

Add smaller ticks between major ticks using `MinorTicksPerInterval`:

### Without Interval

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2018-01-01"
                          Value="2014-01-01"
                          MinorTicksPerInterval="7"
                          ShowTicks="True" />
```

### With Interval

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2020-01-01"
                          Value="2015-01-01"
                          Interval="2"
                          MinorTicksPerInterval="1"
                          ShowTicks="True" />
```

**Result:** For interval=2 years, 1 minor tick appears at the 1-year mark between each major tick.

**C# Implementation:**

```csharp
slider.MinorTicksPerInterval = 1;
```

**Default:** `MinorTicksPerInterval = 0` (no minor ticks)

## Tick Styling

### Major Tick Colors

Customize active and inactive major tick colors:

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2020-01-01"
                          Value="2015-01-01"
                          Interval="2"
                          ShowTicks="True">
    
    <sliders:SfDateTimeSlider.MajorTickStyle>
        <sliders:SliderTickStyle ActiveFill="#EE3F3F"
                                 InactiveFill="#F7B1AE" />
    </sliders:SfDateTimeSlider.MajorTickStyle>
    
</sliders:SfDateTimeSlider>
```

**C# Implementation:**

```csharp
slider.MajorTickStyle.ActiveFill = new SolidColorBrush(Color.FromArgb("#EE3F3F"));
slider.MajorTickStyle.InactiveFill = new SolidColorBrush(Color.FromArgb("#F7B1AE"));
```

- **Active Ticks**: Between `Minimum` and thumb
- **Inactive Ticks**: Between thumb and `Maximum`

### Minor Tick Colors

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2018-01-01"
                          Value="2014-01-01"
                          Interval="2"
                          ShowTicks="True"
                          MinorTicksPerInterval="1">
    
    <sliders:SfDateTimeSlider.MinorTickStyle>
        <sliders:SliderTickStyle ActiveFill="#EE3F3F"
                                 InactiveFill="#F7B1AE" />
    </sliders:SfDateTimeSlider.MinorTickStyle>
    
</sliders:SfDateTimeSlider>
```

**C# Implementation:**

```csharp
slider.MinorTickStyle.ActiveFill = new SolidColorBrush(Color.FromArgb("#EE3F3F"));
slider.MinorTickStyle.InactiveFill = new SolidColorBrush(Color.FromArgb("#F7B1AE"));
```

### Tick Size

Customize tick dimensions using `ActiveSize` and `InactiveSize`:

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2020-01-01"
                          Value="2015-01-01"
                          Interval="2"
                          ShowTicks="True"
                          MinorTicksPerInterval="1">
    
    <sliders:SfDateTimeSlider.MinorTickStyle>
        <sliders:SliderTickStyle ActiveSize="2,10"
                                 InactiveSize="2,10" />
    </sliders:SfDateTimeSlider.MinorTickStyle>
    
    <sliders:SfDateTimeSlider.MajorTickStyle>
        <sliders:SliderTickStyle ActiveSize="2,15"
                                 InactiveSize="2,15" />
    </sliders:SfDateTimeSlider.MajorTickStyle>
    
</sliders:SfDateTimeSlider>
```

**C# Implementation:**

```csharp
// Minor ticks: 2px wide, 10px tall
slider.MinorTickStyle.ActiveSize = new Size(2, 10);
slider.MinorTickStyle.InactiveSize = new Size(2, 10);

// Major ticks: 2px wide, 15px tall
slider.MajorTickStyle.ActiveSize = new Size(2, 15);
slider.MajorTickStyle.InactiveSize = new Size(2, 15);
```

**Default:** `Size(2.0, 8.0)`

**Format:** `Size(width, height)` or `"width,height"` in XAML

## Tick Offset

Adjust spacing between track and ticks:

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2020-01-01"
                          Value="2015-01-01"
                          Interval="2"
                          ShowTicks="True"
                          MinorTicksPerInterval="1">
    
    <sliders:SfDateTimeSlider.MinorTickStyle>
        <sliders:SliderTickStyle Offset="5" />
    </sliders:SfDateTimeSlider.MinorTickStyle>
    
    <sliders:SfDateTimeSlider.MajorTickStyle>
        <sliders:SliderTickStyle Offset="6" />
    </sliders:SfDateTimeSlider.MajorTickStyle>
    
</sliders:SfDateTimeSlider>
```

**C# Implementation:**

```csharp
slider.MinorTickStyle.Offset = 5;
slider.MajorTickStyle.Offset = 6;
```

**Default:** `3.0`

## Dividers

Dividers are circular markers that appear at major interval points on the track.

### Enable Dividers

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2018-01-01"
                          Value="2014-01-01"
                          Interval="2"
                          ShowDividers="True" />
```

**C# Implementation:**

```csharp
slider.ShowDividers = true;
```

**Result:** Divider circles appear at 2010, 2012, 2014, 2016, 2018

**Default:** `ShowDividers = false`

## Divider Styling

### Divider Radius

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2020-01-01"
                          Value="2015-01-01"
                          Interval="2"
                          ShowDividers="True">
    
    <sliders:SfDateTimeSlider.DividerStyle>
        <sliders:SliderDividerStyle ActiveRadius="3"
                                    InactiveRadius="7" />
    </sliders:SfDateTimeSlider.DividerStyle>
    
</sliders:SfDateTimeSlider>
```

**C# Implementation:**

```csharp
slider.DividerStyle.ActiveRadius = 3;
slider.DividerStyle.InactiveRadius = 7;
```

### Divider Colors

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2018-01-01"
                          Value="2014-01-01"
                          Interval="2"
                          ShowDividers="True">
    
    <sliders:SfDateTimeSlider.DividerStyle>
        <sliders:SliderDividerStyle ActiveRadius="7"
                                    InactiveRadius="7"
                                    ActiveFill="#EE3F3F"
                                    InactiveFill="#F7B1AE" />
    </sliders:SfDateTimeSlider.DividerStyle>
    
</sliders:SfDateTimeSlider>
```

**C# Implementation:**

```csharp
slider.DividerStyle.ActiveRadius = 7;
slider.DividerStyle.InactiveRadius = 7;
slider.DividerStyle.ActiveFill = new SolidColorBrush(Color.FromArgb("#EE3F3F"));
slider.DividerStyle.InactiveFill = new SolidColorBrush(Color.FromArgb("#F7B1AE"));
```

### Divider Stroke

Add borders to dividers:

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2018-01-01"
                          Value="2014-01-01"
                          Interval="2"
                          ShowDividers="True">
    
    <sliders:SfDateTimeSlider.DividerStyle>
        <sliders:SliderDividerStyle ActiveRadius="7"
                                    InactiveRadius="7"
                                    ActiveFill="#EE3F3F"
                                    InactiveFill="#F7B1AE"
                                    ActiveStrokeThickness="2"
                                    InactiveStrokeThickness="2"
                                    ActiveStroke="#FFD700"
                                    InactiveStroke="#FFD700" />
    </sliders:SfDateTimeSlider.DividerStyle>
    
</sliders:SfDateTimeSlider>
```

**C# Implementation:**

```csharp
slider.DividerStyle.ActiveStroke = new SolidColorBrush(Color.FromArgb("#FFD700"));
slider.DividerStyle.InactiveStroke = new SolidColorBrush(Color.FromArgb("#FFD700"));
slider.DividerStyle.ActiveStrokeThickness = 2;
slider.DividerStyle.InactiveStrokeThickness = 2;
```

## Disabled States

Customize tick and divider appearance when slider is disabled using Visual State Manager:

```xaml
<ContentPage.Resources>
    <Style TargetType="sliders:SfDateTimeSlider">
        <Setter Property="Minimum" Value="2010-01-01" />
        <Setter Property="Maximum" Value="2018-01-01" />
        <Setter Property="Value" Value="2014-01-01" />
        <Setter Property="Interval" Value="2" />
        <Setter Property="ShowTicks" Value="True" />
        <Setter Property="MinorTicksPerInterval" Value="2" />
        <Setter Property="ShowDividers" Value="True" />
        <Setter Property="VisualStateManager.VisualStateGroups">
            <VisualStateGroupList>
                <VisualStateGroup>
                    <!-- Enabled State -->
                    <VisualState x:Name="Default">
                        <VisualState.Setters>
                            <Setter Property="MajorTickStyle">
                                <Setter.Value>
                                    <sliders:SliderTickStyle ActiveSize="3,10"
                                                             InactiveSize="3,10"
                                                             ActiveFill="#EE3F3F"
                                                             InactiveFill="#F7B1AE" />
                                </Setter.Value>
                            </Setter>
                            <Setter Property="MinorTickStyle">
                                <Setter.Value>
                                    <sliders:SliderTickStyle ActiveSize="3,6"
                                                             InactiveSize="3,6"
                                                             ActiveFill="#EE3F3F"
                                                             InactiveFill="#F7B1AE" />
                                </Setter.Value>
                            </Setter>
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
                    
                    <!-- Disabled State -->
                    <VisualState x:Name="Disabled">
                        <VisualState.Setters>
                            <Setter Property="MajorTickStyle">
                                <Setter.Value>
                                    <sliders:SliderTickStyle ActiveSize="3,10"
                                                             InactiveSize="3,10"
                                                             ActiveFill="Gray"
                                                             InactiveFill="LightGray" />
                                </Setter.Value>
                            </Setter>
                            <Setter Property="MinorTickStyle">
                                <Setter.Value>
                                    <sliders:SliderTickStyle ActiveSize="3,6"
                                                             InactiveSize="3,6"
                                                             ActiveFill="Gray"
                                                             InactiveFill="LightGray" />
                                </Setter.Value>
                            </Setter>
                            <Setter Property="DividerStyle">
                                <Setter.Value>
                                    <sliders:SliderDividerStyle ActiveFill="Gray"
                                                                InactiveFill="LightGray"
                                                                ActiveRadius="5"
                                                                InactiveRadius="4" />
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

## Properties Reference

### MajorTickStyle / MinorTickStyle

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `ActiveFill` | Brush | Theme default | Color for ticks before thumb |
| `InactiveFill` | Brush | Theme default | Color for ticks after thumb |
| `ActiveSize` | Size | (2, 8) | Size of active ticks (width, height) |
| `InactiveSize` | Size | (2, 8) | Size of inactive ticks |
| `Offset` | double | 3.0 | Space between track and ticks |

### DividerStyle

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `ActiveRadius` | double | 2.0 | Radius of active dividers |
| `InactiveRadius` | double | 2.0 | Radius of inactive dividers |
| `ActiveFill` | Brush | Theme default | Fill color for active dividers |
| `InactiveFill` | Brush | Theme default | Fill color for inactive dividers |
| `ActiveStroke` | Brush | Transparent | Border color for active dividers |
| `InactiveStroke` | Brush | Transparent | Border color for inactive dividers |
| `ActiveStrokeThickness` | double | 0.0 | Border width for active dividers |
| `InactiveStrokeThickness` | double | 0.0 | Border width for inactive dividers |

## Common Patterns

### Pattern 1: Subtle Ticks

```csharp
slider.MajorTickStyle.ActiveSize = new Size(1, 8);
slider.MajorTickStyle.InactiveSize = new Size(1, 6);
slider.MajorTickStyle.ActiveFill = new SolidColorBrush(Colors.Gray);
slider.MajorTickStyle.InactiveFill = new SolidColorBrush(Colors.LightGray);
```

### Pattern 2: Prominent Dividers with Borders

```xaml
<sliders:SfDateTimeSlider.DividerStyle>
    <sliders:SliderDividerStyle ActiveRadius="8"
                                InactiveRadius="6"
                                ActiveFill="White"
                                InactiveFill="WhiteSmoke"
                                ActiveStroke="Blue"
                                InactiveStroke="LightBlue"
                                ActiveStrokeThickness="2"
                                InactiveStrokeThickness="1" />
</sliders:SfDateTimeSlider.DividerStyle>
```

### Pattern 3: No Minor Ticks, Only Dividers

```xaml
<sliders:SfDateTimeSlider ShowTicks="False"
                          ShowDividers="True"
                          MinorTicksPerInterval="0" />
```

## Best Practices

1. **Hierarchy**: Major ticks should be larger than minor ticks
2. **Consistency**: Use similar colors for ticks, dividers, and labels
3. **Clarity**: Avoid overcrowding with too many minor ticks
4. **Performance**: Excessive ticks can impact rendering performance
5. **Accessibility**: Ticks improve usability for precise value selection

## Next Steps

- **Labels**: Add formatted date labels at tick positions
- **Tooltip**: Display current value during interaction
- **Thumb Styling**: Customize the draggable thumb
