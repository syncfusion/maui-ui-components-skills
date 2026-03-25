# Labels in .NET MAUI DateTime Slider

## Table of Contents
- [Overview](#overview)
- [Showing Labels](#showing-labels)
- [Date Format](#date-format)
- [Label Placement](#label-placement)
- [Edge Labels Placement](#edge-labels-placement)
- [Label Style Customization](#label-style-customization)
- [Label Offset](#label-offset)
- [Custom Label Text](#custom-label-text)
- [Disabled Labels](#disabled-labels)

## Overview

Labels display formatted date/time values at specified intervals along the slider track. They provide visual reference points for users to understand the current selection.

## Showing Labels

Enable labels using the `ShowLabels` property:

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2018-01-01"
                          Value="2014-01-01"
                          Interval="2"
                          ShowLabels="True"
                          ShowTicks="True" />
```

**C# Implementation:**

```csharp
var slider = new SfDateTimeSlider
{
    Minimum = new DateTime(2010, 01, 01),
    Maximum = new DateTime(2018, 01, 01),
    Value = new DateTime(2014, 01, 01),
    Interval = 2,
    ShowTicks = true,
    ShowLabels = true
};
```

**Default:** `ShowLabels = false`

## Date Format

Customize date/time display using the `DateFormat` property with standard .NET date format strings:

### Year Format Example

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2018-01-01"
                          Value="2014-01-01"
                          Interval="2"
                          IntervalType="Years"
                          DateFormat="yyyy"
                          ShowLabels="True"
                          ShowTicks="True" />
```

**Result:** Labels display as "2010", "2012", "2014", "2016", "2018"

### Time Format Example

```xaml
<sliders:SfDateTimeSlider Minimum="2000-01-01T09:00:00"
                          Maximum="2000-01-01T17:00:00"
                          Value="2000-01-01T13:00:00"
                          ShowLabels="True"
                          ShowTicks="True"
                          IntervalType="Hours"
                          Interval="2"
                          DateFormat="h tt" />
```

**Result:** Labels display as "9 AM", "11 AM", "1 PM", "3 PM", "5 PM"

### Month Format Example

```xaml
<sliders:SfDateTimeSlider Minimum="2023-01-01"
                          Maximum="2023-12-31"
                          Value="2023-06-15"
                          Interval="2"
                          IntervalType="Months"
                          DateFormat="MMM"
                          ShowLabels="True"
                          ShowTicks="True" />
```

**Result:** "Jan", "Mar", "May", "Jul", "Sep", "Nov"

### Common DateFormat Patterns

| Pattern | Example Output | Use Case |
|---------|----------------|----------|
| `"yyyy"` | 2023 | Years only |
| `"MMM"` | Jan | Month abbreviation |
| `"MMMM"` | January | Full month name |
| `"MMM yyyy"` | Jan 2023 | Month and year |
| `"MM/dd/yyyy"` | 01/15/2023 | US date format |
| `"dd/MM/yyyy"` | 15/01/2023 | European date format |
| `"h:mm tt"` | 3:45 PM | 12-hour time |
| `"HH:mm"` | 15:45 | 24-hour time |
| `"ddd, MMM d"` | Wed, Jan 15 | Day, month, date |

**Default:** `string.Empty` (auto-generated based on interval)

## Label Placement

Control where labels appear relative to ticks using `LabelsPlacement`:

### On Ticks (Default)

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2014-01-01"
                          Value="2012-01-01"
                          Interval="1"
                          LabelsPlacement="OnTicks"
                          ShowLabels="True"
                          ShowTicks="True" />
```

Labels align directly with major ticks.

### Between Ticks

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2014-01-01"
                          Value="2012-01-01"
                          Interval="1"
                          LabelsPlacement="BetweenTicks"
                          ShowLabels="True"
                          ShowTicks="True" />
```

Labels appear centered between major ticks.

**C# Implementation:**

```csharp
slider.LabelsPlacement = SliderLabelsPlacement.BetweenTicks;
```

## Edge Labels Placement

Control how first and last labels are positioned:

### Default Placement

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2018-01-01"
                          Value="2014-01-01"
                          Interval="2"
                          EdgeLabelsPlacement="Default"
                          ShowLabels="True"
                          ShowTicks="True" />
```

Edge labels are placed based on intervals, may extend beyond track.

### Inside Placement

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2018-01-01"
                          Value="2014-01-01"
                          Interval="2"
                          EdgeLabelsPlacement="Inside"
                          ShowLabels="True"
                          ShowTicks="True" />
```

First and last labels are moved inside the track bounds.

**With TrackExtent:**

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2018-01-01"
                          Value="2014-01-01"
                          Interval="2"
                          TrackExtent="20"
                          EdgeLabelsPlacement="Inside"
                          ShowLabels="True"
                          ShowTicks="True" />
```

When `TrackExtent > 0` and `EdgeLabelsPlacement="Inside"`, labels are placed within the extended track area.

## Label Style Customization

Customize active and inactive label appearance:

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2018-01-01"
                          Value="2014-01-01"
                          Interval="2"
                          ShowTicks="True"
                          ShowLabels="True">
    
    <sliders:SfDateTimeSlider.LabelStyle>
        <sliders:SliderLabelStyle ActiveTextColor="#EE3F3F"
                                  InactiveTextColor="#F7B1AE"
                                  ActiveFontAttributes="Italic"
                                  InactiveFontAttributes="Bold"
                                  ActiveFontSize="16"
                                  InactiveFontSize="16" />
    </sliders:SfDateTimeSlider.LabelStyle>
    
</sliders:SfDateTimeSlider>
```

**C# Implementation:**

```csharp
slider.LabelStyle.ActiveTextColor = Color.FromArgb("#EE3F3F");
slider.LabelStyle.InactiveTextColor = Color.FromArgb("#F7B1AE");
slider.LabelStyle.ActiveFontSize = 16;
slider.LabelStyle.InactiveFontSize = 16;
slider.LabelStyle.ActiveFontAttributes = FontAttributes.Italic;
slider.LabelStyle.InactiveFontAttributes = FontAttributes.Bold;
```

### Active vs Inactive Labels

- **Active Labels**: Between `Minimum` and thumb (current value)
- **Inactive Labels**: Between thumb and `Maximum`

### Available Style Properties

| Property | Type | Description |
|----------|------|-------------|
| `ActiveTextColor` | Color | Color of labels before thumb |
| `InactiveTextColor` | Color | Color of labels after thumb |
| `ActiveFontSize` | double | Font size for active labels |
| `InactiveFontSize` | double | Font size for inactive labels |
| `ActiveFontFamily` | string | Font family for active labels |
| `InactiveFontFamily` | string | Font family for inactive labels |
| `ActiveFontAttributes` | FontAttributes | Bold, Italic, None for active |
| `InactiveFontAttributes` | FontAttributes | Bold, Italic, None for inactive |
| `Offset` | double | Space between ticks and labels |

## Label Offset

Adjust spacing between ticks and labels:

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2018-01-01"
                          Value="2014-01-01"
                          Interval="2"
                          ShowTicks="True"
                          ShowLabels="True">
    
    <sliders:SfDateTimeSlider.LabelStyle>
        <sliders:SliderLabelStyle Offset="15" />
    </sliders:SfDateTimeSlider.LabelStyle>
    
</sliders:SfDateTimeSlider>
```

**C# Implementation:**

```csharp
slider.LabelStyle.Offset = 15;
```

**Default Values:**
- With ticks (`ShowTicks=True`): 5.0
- Without ticks: 15.0

## Custom Label Text

Use the `LabelCreated` event to customize label text dynamically:

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2011-01-01"
                          Value="2010-07-01"
                          Interval="3"
                          DateFormat="MMM"
                          ShowTicks="True"
                          LabelsPlacement="BetweenTicks"
                          IntervalType="Months"
                          LabelCreated="OnLabelCreated"
                          ShowLabels="True" />
```

**Event Handler:**

```csharp
private void OnLabelCreated(object sender, SliderLabelCreatedEventArgs e)
{
    if (e.Text == "Jan")
        e.Text = "Quarter 1";
    else if (e.Text == "Apr")
        e.Text = "Quarter 2";
    else if (e.Text == "Jul")
        e.Text = "Quarter 3";
    else if (e.Text == "Oct")
        e.Text = "Quarter 4";
}
```

### SliderLabelCreatedEventArgs Properties

| Property | Type | Description |
|----------|------|-------------|
| `Text` | string | Label text (read/write) |
| `Style` | SliderLabelStyle | Label style (read/write) |

**Use Cases:**
- Display quarters instead of months
- Show fiscal year labels
- Add custom prefixes/suffixes
- Localize date labels

## Disabled Labels

Customize label appearance when slider is disabled using Visual State Manager:

```xaml
<ContentPage.Resources>
    <Style TargetType="sliders:SfDateTimeSlider">
        <Setter Property="Minimum" Value="2010-01-01" />
        <Setter Property="Maximum" Value="2018-01-01" />
        <Setter Property="Value" Value="2014-01-01" />
        <Setter Property="Interval" Value="2" />
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

<VerticalStackLayout>
    <Label Text="Enabled" Padding="10" />
    <sliders:SfDateTimeSlider />
    
    <Label Text="Disabled" Padding="10" />
    <sliders:SfDateTimeSlider IsEnabled="False" />
</VerticalStackLayout>
```

## Common Patterns

### Pattern 1: Emphasize Current Month

```csharp
private void OnLabelCreated(object sender, SliderLabelCreatedEventArgs e)
{
    // Highlight current month in bold
    if (e.Text == DateTime.Now.ToString("MMM"))
    {
        e.Style = new SliderLabelStyle
        {
            ActiveFontAttributes = FontAttributes.Bold,
            InactiveFontAttributes = FontAttributes.Bold,
            ActiveFontSize = 18,
            InactiveFontSize = 18
        };
    }
}
```

### Pattern 2: Color-Coded Seasons

```csharp
private void OnLabelCreated(object sender, SliderLabelCreatedEventArgs e)
{
    var seasonColors = new Dictionary<string, Color>
    {
        {"Dec", Colors.Blue}, {"Jan", Colors.Blue}, {"Feb", Colors.Blue},  // Winter
        {"Mar", Colors.Green}, {"Apr", Colors.Green}, {"May", Colors.Green}, // Spring
        {"Jun", Colors.Orange}, {"Jul", Colors.Orange}, {"Aug", Colors.Orange}, // Summer
        {"Sep", Colors.Brown}, {"Oct", Colors.Brown}, {"Nov", Colors.Brown} // Fall
    };
    
    if (seasonColors.TryGetValue(e.Text, out var color))
    {
        e.Style = new SliderLabelStyle
        {
            ActiveTextColor = color,
            InactiveTextColor = color.WithAlpha(0.5f)
        };
    }
}
```

### Pattern 3: Minimal Labels (Custom Font)

```xaml
<sliders:SfDateTimeSlider.LabelStyle>
    <sliders:SliderLabelStyle ActiveTextColor="#333333"
                              InactiveTextColor="#999999"
                              ActiveFontSize="10"
                              InactiveFontSize="10"
                              ActiveFontFamily="RobotoMono"
                              InactiveFontFamily="RobotoMono" />
</sliders:SfDateTimeSlider.LabelStyle>
```

## Best Practices

1. **Readability**: Ensure font size is at least 12px for accessibility
2. **Contrast**: Use sufficient color contrast between labels and background
3. **Format Consistency**: Match DateFormat with IntervalType (e.g., "yyyy" for Years)
4. **Label Density**: Avoid overcrowding by adjusting Interval
5. **Responsive Design**: Test label overflow on different screen sizes

## Next Steps

- **Ticks & Dividers**: Add visual markers between labels
- **Tooltip**: Display formatted dates during interaction
- **Events**: Handle LabelCreated for advanced customization
