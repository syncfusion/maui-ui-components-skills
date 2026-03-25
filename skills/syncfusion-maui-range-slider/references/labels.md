# Labels in .NET MAUI Range Slider

## Table of Contents
- [Overview](#overview)
- [Core Properties](#core-properties)
  - [ShowLabels](#showlabels)
  - [NumberFormat](#numberformat)
  - [LabelsPlacement](#labelsplacement)
  - [EdgeLabelsPlacement](#edgelabelsplacement)
- [Label Styling](#label-styling)
  - [Active Label Styling](#active-label-styling)
  - [Inactive Label Styling](#inactive-label-styling)
  - [Label Fonts](#label-fonts)
- [Label Positioning](#label-positioning)
  - [Label Offset](#label-offset)
- [Events](#events)
  - [LabelCreated Event](#labelcreated-event)
- [Visual State Management](#visual-state-management)
  - [Disabled State](#disabled-state)
- [Common Scenarios](#common-scenarios)
- [Best Practices](#best-practices)
- [Related References](#related-references)

## Overview

Labels in the .NET MAUI Range Slider (`SfRangeSlider`) provide visual indicators for values along the track at specified intervals. They help users understand the scale and current selection of the slider. This reference covers all aspects of configuring and customizing labels.

## Core Properties

### ShowLabels

The `ShowLabels` property controls whether labels are displayed on the slider.

**Type:** `bool`  
**Default:** `false`

**XAML Example:**
```xaml
<sliders:SfRangeSlider Minimum="0"
                       Maximum="10"
                       RangeStart="2"
                       RangeEnd="8"
                       Interval="2"
                       ShowLabels="True"
                       ShowTicks="True" />
```

**C# Example:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    Minimum = 0,
    Maximum = 10,
    RangeStart = 2,
    RangeEnd = 8,
    Interval = 2,
    ShowLabels = true,
    ShowTicks = true
};
```

### NumberFormat

The `NumberFormat` property formats numeric label text using standard .NET format strings.

**Type:** `string`  
**Default:** `"0.##"`

**Common Format Patterns:**
- `"0.##"` - Default, shows up to 2 decimal places
- `"$#"` - Currency symbol with no decimals
- `"0.00"` - Always shows 2 decimal places
- `"P0"` - Percentage format with no decimals
- `"N2"` - Number format with 2 decimals and thousand separators

**XAML Example:**
```xaml
<sliders:SfRangeSlider Minimum="2"
                       Maximum="10"
                       RangeStart="4"
                       RangeEnd="8"
                       Interval="2"
                       NumberFormat="$#"
                       ShowLabels="True"
                       ShowTicks="True" />
```

**C# Example:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    Minimum = 2,
    Maximum = 10,
    RangeStart = 4,
    RangeEnd = 8,
    Interval = 2,
    NumberFormat = "$#",
    ShowLabels = true,
    ShowTicks = true
};
```

### LabelsPlacement

The `LabelsPlacement` property determines whether labels appear on tick marks or between them.

**Type:** `SliderLabelsPlacement`  
**Default:** `SliderLabelsPlacement.OnTicks`

**Values:**
- `OnTicks` - Labels aligned with tick marks
- `BetweenTicks` - Labels positioned between tick marks

**XAML Example:**
```xaml
<sliders:SfRangeSlider Minimum="0"
                       Maximum="10"
                       RangeStart="2"
                       RangeEnd="8"
                       Interval="2"
                       LabelsPlacement="BetweenTicks"
                       ShowLabels="True"
                       ShowTicks="True" />
```

**C# Example:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    Minimum = 0,
    Maximum = 10,
    RangeStart = 2,
    RangeEnd = 8,
    Interval = 2,
    LabelsPlacement = SliderLabelsPlacement.BetweenTicks,
    ShowLabels = true,
    ShowTicks = true
};
```

### EdgeLabelsPlacement

The `EdgeLabelsPlacement` property controls how the first and last labels are positioned relative to track boundaries.

**Type:** `SliderEdgeLabelsPlacement`  
**Default:** `SliderEdgeLabelsPlacement.Default`

**Values:**
- `Default` - Labels positioned based on intervals
- `Inside` - Labels positioned inside track boundaries

**XAML Example:**
```xaml
<sliders:SfRangeSlider Minimum="100"
                       Maximum="1000"
                       Interval="225"
                       RangeStart="325"
                       RangeEnd="775"
                       NumberFormat="$##.#"
                       ShowLabels="True"
                       ShowTicks="True"
                       EdgeLabelsPlacement="Inside" />
```

**C# Example:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    Minimum = 100,
    Maximum = 1000,
    RangeStart = 325,
    RangeEnd = 775,
    Interval = 225,
    NumberFormat = "$##.#",
    EdgeLabelsPlacement = SliderEdgeLabelsPlacement.Inside,
    ShowLabels = true,
    ShowTicks = true
};
```

## Label Styling

### Active Label Styling

Active labels are those within the selected range (between start and end thumbs).

**Properties:**
- `ActiveTextColor` - Color of active label text
- `ActiveFontSize` - Font size for active labels
- `ActiveFontFamily` - Font family for active labels
- `ActiveFontAttributes` - Font attributes (Bold, Italic, etc.)

**XAML Example:**
```xaml
<sliders:SfRangeSlider Minimum="0"
                       Maximum="10"
                       Interval="2"
                       RangeStart="2"
                       RangeEnd="8"
                       ShowLabels="True"
                       ShowTicks="True">
    <sliders:SfRangeSlider.LabelStyle>
        <sliders:SliderLabelStyle ActiveTextColor="#EE3F3F"
                                  ActiveFontAttributes="Italic"
                                  ActiveFontSize="16" />
    </sliders:SfRangeSlider.LabelStyle>
</sliders:SfRangeSlider>
```

**C# Example:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    Minimum = 0,
    Maximum = 10,
    RangeStart = 2,
    RangeEnd = 8,
    Interval = 2,
    ShowLabels = true,
    ShowTicks = true
};
rangeSlider.LabelStyle.ActiveTextColor = Color.FromArgb("#EE3F3F");
rangeSlider.LabelStyle.ActiveFontSize = 16;
rangeSlider.LabelStyle.ActiveFontAttributes = FontAttributes.Italic;
```

### Inactive Label Styling

Inactive labels are those outside the selected range.

**Properties:**
- `InactiveTextColor` - Color of inactive label text
- `InactiveFontSize` - Font size for inactive labels
- `InactiveFontFamily` - Font family for inactive labels
- `InactiveFontAttributes` - Font attributes for inactive labels

**XAML Example:**
```xaml
<sliders:SfRangeSlider Minimum="0"
                       Maximum="10"
                       Interval="2"
                       RangeStart="2"
                       RangeEnd="8"
                       ShowLabels="True"
                       ShowTicks="True">
    <sliders:SfRangeSlider.LabelStyle>
        <sliders:SliderLabelStyle InactiveTextColor="#F7B1AE"
                                  InactiveFontAttributes="Italic"
                                  InactiveFontSize="16" />
    </sliders:SfRangeSlider.LabelStyle>
</sliders:SfRangeSlider>
```

**C# Example:**
```csharp
rangeSlider.LabelStyle.InactiveTextColor = Color.FromArgb("#F7B1AE");
rangeSlider.LabelStyle.InactiveFontSize = 16;
rangeSlider.LabelStyle.InactiveFontAttributes = FontAttributes.Italic;
```

### Label Fonts

**Complete Styling Example:**
```xaml
<sliders:SfRangeSlider.LabelStyle>
    <sliders:SliderLabelStyle ActiveTextColor="#EE3F3F"
                              InactiveTextColor="#F7B1AE"
                              ActiveFontAttributes="Bold"
                              InactiveFontAttributes="Italic"
                              ActiveFontSize="16"
                              InactiveFontSize="14"
                              ActiveFontFamily="Arial"
                              InactiveFontFamily="Arial" />
</sliders:SfRangeSlider.LabelStyle>
```

**C# Complete Example:**
```csharp
rangeSlider.LabelStyle = new SliderLabelStyle
{
    ActiveTextColor = Color.FromArgb("#EE3F3F"),
    InactiveTextColor = Color.FromArgb("#F7B1AE"),
    ActiveFontSize = 16,
    InactiveFontSize = 14,
    ActiveFontAttributes = FontAttributes.Bold,
    InactiveFontAttributes = FontAttributes.Italic,
    ActiveFontFamily = "Arial",
    InactiveFontFamily = "Arial"
};
```

## Label Positioning

### Label Offset

The `Offset` property adjusts vertical spacing between labels and ticks/track.

**Type:** `double`  
**Default:** `5.0` (with ticks), `15.0` (without ticks)

**XAML Example:**
```xaml
<sliders:SfRangeSlider Minimum="0"
                       Maximum="10"
                       RangeStart="2"
                       RangeEnd="8"
                       Interval="2" 
                       ShowLabels="True" 
                       ShowTicks="True">
    <sliders:SfRangeSlider.LabelStyle>
        <sliders:SliderLabelStyle Offset="10" />
    </sliders:SfRangeSlider.LabelStyle>
</sliders:SfRangeSlider>
```

**C# Example:**
```csharp
rangeSlider.LabelStyle.Offset = 10;
```

## Events

### LabelCreated Event

The `LabelCreated` event allows customization of individual label text before display.

**Event Args:** `SliderLabelCreatedEventArgs`
- `Text` - The label text to display
- `Style` - The label style properties

**XAML Example:**
```xaml
<sliders:SfRangeSlider Minimum="2"
                       Maximum="10"
                       RangeStart="4"
                       RangeEnd="8"
                       Interval="2"
                       ShowLabels="True"
                       ShowTicks="True"
                       LabelCreated="OnLabelCreated" />
```

**C# Event Handler:**
```csharp
private void OnLabelCreated(object sender, SliderLabelCreatedEventArgs e)
{
    e.Text = "$" + e.Text;
}
```

**Complete C# Example:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    Minimum = 2,
    Maximum = 10,
    RangeStart = 4,
    RangeEnd = 8,
    Interval = 2,
    ShowLabels = true,
    ShowTicks = true
};
rangeSlider.LabelCreated += OnLabelCreated;
```

## Visual State Management

### Disabled State

Customize label appearance when the slider is disabled using Visual State Manager.

**XAML Example:**
```xaml
<ContentPage.Resources>
    <Style TargetType="sliders:SfRangeSlider">
        <Setter Property="Interval" Value="0.25" />
        <Setter Property="ShowTicks" Value="True" />
        <Setter Property="ShowLabels" Value="True" />
        <Setter Property="VisualStateManager.VisualStateGroups">
            <VisualStateGroupList>
                <VisualStateGroup>
                    <VisualState x:Name="Default">
                        <VisualState.Setters>
                            <Setter Property="LabelStyle">
                                <Setter.Value>
                                    <sliders:SliderLabelStyle ActiveTextColor="#EE3F3F"
                                                              InactiveTextColor="#F7B1AE"
                                                              ActiveFontSize="16"
                                                              InactiveFontSize="14"
                                                              ActiveFontAttributes="Bold"
                                                              InactiveFontAttributes="Italic" />
                                </Setter.Value>
                            </Setter>
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState x:Name="Disabled">
                        <VisualState.Setters>
                            <Setter Property="LabelStyle">
                                <Setter.Value>
                                    <sliders:SliderLabelStyle ActiveTextColor="Gray"
                                                              InactiveTextColor="LightGray"
                                                              ActiveFontSize="14"
                                                              InactiveFontSize="16"
                                                              ActiveFontAttributes="Italic"
                                                              InactiveFontAttributes="Bold" />
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
VisualStateGroupList visualStateGroupList = new VisualStateGroupList();
VisualStateGroup commonStateGroup = new VisualStateGroup();

// Default State
VisualState defaultState = new VisualState { Name = "Default" };
defaultState.Setters.Add(new Setter
{
    Property = SfRangeSlider.LabelStyleProperty,
    Value = new SliderLabelStyle
    {
        ActiveFontSize = 16,
        InactiveFontSize = 14,
        ActiveTextColor = Color.FromArgb("#EE3F3F"),
        InactiveTextColor = Color.FromArgb("#F7B1AE"),
        ActiveFontAttributes = FontAttributes.Bold,
        InactiveFontAttributes = FontAttributes.Italic
    }
});

// Disabled State
VisualState disabledState = new VisualState { Name = "Disabled" };
disabledState.Setters.Add(new Setter
{
    Property = SfRangeSlider.LabelStyleProperty,
    Value = new SliderLabelStyle
    {
        ActiveFontSize = 14,
        InactiveFontSize = 16,
        ActiveTextColor = Colors.Gray,
        InactiveTextColor = Colors.LightGray,
        ActiveFontAttributes = FontAttributes.Italic,
        InactiveFontAttributes = FontAttributes.Bold
    }
});

commonStateGroup.States.Add(defaultState);
commonStateGroup.States.Add(disabledState);
visualStateGroupList.Add(commonStateGroup);
VisualStateManager.SetVisualStateGroups(rangeSlider, visualStateGroupList);
```

## Common Scenarios

### Currency Formatting
```xaml
<sliders:SfRangeSlider NumberFormat="C0" ShowLabels="True" />
```

### Percentage Display
```xaml
<sliders:SfRangeSlider NumberFormat="P0" ShowLabels="True" />
```

### Custom Prefix/Suffix with Event
```csharp
private void OnLabelCreated(object sender, SliderLabelCreatedEventArgs e)
{
    e.Text = e.Text + " kg";
}
```

### Emphasizing Active Range
```xaml
<sliders:SfRangeSlider.LabelStyle>
    <sliders:SliderLabelStyle ActiveTextColor="Blue"
                              InactiveTextColor="LightGray"
                              ActiveFontSize="18"
                              InactiveFontSize="14"
                              ActiveFontAttributes="Bold" />
</sliders:SfRangeSlider.LabelStyle>
```

## Best Practices

1. **Always set Interval**: Labels require an interval to display. Set `Interval` property or let it auto-calculate.

2. **Match Label and Tick Settings**: Enable both `ShowLabels` and `ShowTicks` for better visual alignment.

3. **Format Consistently**: Use `NumberFormat` for simple formatting, `LabelCreated` event for complex customization.

4. **Consider Readability**: Ensure sufficient contrast between active/inactive label colors and background.

5. **Optimize Label Density**: Avoid too many labels by setting appropriate intervals based on slider width.

6. **Use EdgeLabelsPlacement Wisely**: Set to `Inside` when labels might overflow slider boundaries.

7. **Font Size Guidelines**: Keep font sizes between 12-18 for optimal readability on mobile devices.

8. **Offset Adjustment**: Adjust `Offset` if labels overlap with ticks or thumbs.

## Related References

- [ticks.md](./ticks.md) - Tick marks configuration
- [intervals-and-selection.md](./intervals-and-selection.md) - Interval settings
- [events-and-commands.md](./events-and-commands.md) - LabelCreated event details
- [thumbs-and-overlays.md](./thumbs-and-overlays.md) - Thumb customization
- [tooltips.md](./tooltips.md) - Tooltip label formatting
