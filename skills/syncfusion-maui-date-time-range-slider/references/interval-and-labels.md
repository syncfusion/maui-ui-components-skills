# Interval and Labels Configuration

This guide covers interval configuration, label display, formatting, placement, and styling for the DateTime Range Slider.

## Table of Contents
- [Overview](#overview)
- [Date Interval](#date-interval)
- [Auto Interval](#auto-interval)
- [Show Labels](#show-labels)
- [Date Format](#date-format)
- [Label Placement](#label-placement)
- [Edge Labels Placement](#edge-labels-placement)
- [Label Style](#label-style)
- [Label Offset](#label-offset)
- [Custom Label Text](#custom-label-text)
- [Disabled Labels (Visual State Manager)](#disabled-labels-visual-state-manager)

## Overview

Labels display DateTime values at specified intervals along the track. The `Interval`, `IntervalType`, and `DateFormat` properties control when and how labels appear.

## Date Interval

Labels, ticks, and dividers render based on `Interval`, `Minimum`, and `Maximum` properties.

### Properties

- `Interval` (double) - Step between labels/ticks (default: 0)
- `IntervalType` (SliderDateIntervalType) - Unit of interval
  - `Years`
  - `Months`
  - `Days`
  - `Hours`
  - `Minutes`
  - `Seconds`
- `DateFormat` (string) - Label format pattern

### Example: Yearly Intervals

```xaml
<sliders:SfDateTimeRangeSlider 
    Minimum="2000-01-01" 
    Maximum="2004-01-01" 
    RangeStart="2001-01-01" 
    RangeEnd="2003-01-01"
    Interval="1" 
    IntervalType="Years"
    DateFormat="yyyy" 
    ShowLabels="True" 
    ShowTicks="True" 
    ShowDividers="True" />
```

```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2000, 01, 01);
rangeSlider.Maximum = new DateTime(2004, 01, 01);
rangeSlider.RangeStart = new DateTime(2001, 01, 01); 
rangeSlider.RangeEnd = new DateTime(2003, 01, 01);            
rangeSlider.Interval = 1;
rangeSlider.IntervalType = SliderDateIntervalType.Years;
rangeSlider.DateFormat = "yyyy";
rangeSlider.ShowLabels = true;
rangeSlider.ShowTicks = true;
rangeSlider.ShowDividers = true;
```

**Result:** Labels at 2000, 2001, 2002, 2003, 2004

## Auto Interval

When `ShowLabels`, `ShowTicks`, or `ShowDividers` is `True` but `Interval = 0`, the interval is automatically calculated based on available space.

### Example

```xaml
<sliders:SfDateTimeRangeSlider 
    Minimum="2000-01-01" 
    Maximum="2004-01-01" 
    RangeStart="2001-01-01" 
    RangeEnd="2003-01-01"
    ShowLabels="True" 
    ShowTicks="True" 
    ShowDividers="True" />
```

```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2000, 01, 01);
rangeSlider.Maximum = new DateTime(2004, 01, 01);
rangeSlider.RangeStart = new DateTime(2001, 01, 01); 
rangeSlider.RangeEnd = new DateTime(2003, 01, 01);            
rangeSlider.ShowLabels = true;
rangeSlider.ShowTicks = true;
rangeSlider.ShowDividers = true;
```

**Behavior:** Interval, IntervalType, and DateFormat are automatically determined based on the date range and control width.

## Show Labels

Enable label display with `ShowLabels` property.

### Property

- `ShowLabels` (bool) - Display labels (default: false)

### Example

```xaml
<sliders:SfDateTimeRangeSlider 
    Minimum="2010-01-01" 
    Maximum="2018-01-01" 
    RangeStart="2012-01-01" 
    RangeEnd="2016-01-01"
    Interval="2" 
    ShowLabels="True"
    ShowTicks="True" />
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
```

## Date Format

Format label text using standard .NET date format strings.

### Property

- `DateFormat` (string) - Format pattern (default: string.Empty)

### Common Formats

| Format | Example Output | Use Case |
|--------|----------------|----------|
| `"yyyy"` | 2024 | Years |
| `"MMM"` | Jan | Month abbreviation |
| `"MMM yyyy"` | Jan 2024 | Month and year |
| `"dd MMM"` | 15 Jan | Day and month |
| `"h tt"` | 9 AM | Hour with AM/PM |
| `"hh:mm"` | 09:30 | Hour and minute |
| `"hh:mm tt"` | 09:30 AM | Full time |

### Example: Hour Format

```xaml
<sliders:SfDateTimeRangeSlider 
    Minimum="2000-01-01T09:00:00" 
    Maximum="2000-01-01T17:00:00" 
    RangeStart="2000-01-01T11:00:00" 
    RangeEnd="2000-01-01T15:00:00" 
    Interval="2"
    IntervalType="Hours"
    ShowLabels="True"  
    DateFormat="h tt"
    ShowTicks="True" />
```

```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2000, 01, 01, 09, 00, 00);
rangeSlider.Maximum = new DateTime(2000, 01, 01, 17, 00, 00);
rangeSlider.RangeStart = new DateTime(2000, 01, 01, 11, 00, 00);
rangeSlider.RangeEnd = new DateTime(2000, 01, 01, 15, 00, 00);
rangeSlider.Interval = 2;
rangeSlider.IntervalType = SliderDateIntervalType.Hours;
rangeSlider.DateFormat = "h tt";
rangeSlider.ShowLabels = true;
rangeSlider.ShowTicks = true;
```

**Result:** Labels show "9 AM", "11 AM", "1 PM", "3 PM", "5 PM"

## Label Placement

Position labels on ticks or between ticks.

### Property

- `LabelsPlacement` (SliderLabelsPlacement)
  - `OnTicks` (default) - Labels align with tick marks
  - `BetweenTicks` - Labels center between ticks

### Example

```xaml
<sliders:SfDateTimeRangeSlider 
    Minimum="2011-01-01"
    Maximum="2016-01-01"
    RangeStart="2012-01-01"
    RangeEnd="2015-01-01"
    Interval="1"
    LabelsPlacement="BetweenTicks"
    ShowLabels="True"
    ShowTicks="True" />
```

```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2011, 01, 01);
rangeSlider.Maximum = new DateTime(2016, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2015, 01, 01);
rangeSlider.Interval = 1;
rangeSlider.LabelsPlacement = SliderLabelsPlacement.BetweenTicks;
rangeSlider.ShowLabels = true;
rangeSlider.ShowTicks = true;
```

## Edge Labels Placement

Control placement of the first and last labels.

### Property

- `EdgeLabelsPlacement` (SliderEdgeLabelsPlacement)
  - `Default` - Labels positioned based on intervals
  - `Inside` - First/last labels moved inside track bounds

### Example

```xaml
<sliders:SfDateTimeRangeSlider 
    Minimum="2010-01-01" 
    Maximum="2018-01-01" 
    RangeStart="2012-01-01" 
    RangeEnd="2016-01-01"
    Interval="2"
    ShowLabels="True"
    ShowTicks="True"
    EdgeLabelsPlacement="Inside" />
```

```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2018, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2016, 01, 01);
rangeSlider.Interval = 2;
rangeSlider.EdgeLabelsPlacement = SliderEdgeLabelsPlacement.Inside;
rangeSlider.ShowLabels = true;
rangeSlider.ShowTicks = true;
```

**Use Case:** Prevents edge labels from being cut off when `TrackExtent` is used.

## Label Style

Customize active and inactive label appearance.

### Properties (SliderLabelStyle)

**Active Labels** (between thumbs):
- `ActiveTextColor` (Color)
- `ActiveFontSize` (double)
- `ActiveFontFamily` (string)
- `ActiveFontAttributes` (FontAttributes) - Bold, Italic

**Inactive Labels** (outside thumbs):
- `InactiveTextColor` (Color)
- `InactiveFontSize` (double)
- `InactiveFontFamily` (string)
- `InactiveFontAttributes` (FontAttributes)

### Example

```xaml
<sliders:SfDateTimeRangeSlider 
    Minimum="2010-01-01" 
    Maximum="2018-01-01" 
    RangeStart="2012-01-01" 
    RangeEnd="2016-01-01" 
    Interval="2" 
    ShowLabels="True" 
    ShowTicks="True">

    <sliders:SfDateTimeRangeSlider.LabelStyle>
        <sliders:SliderLabelStyle 
            ActiveTextColor="#EE3F3F" 
            InactiveTextColor="#F7B1AE" 
            ActiveFontAttributes="Italic" 
            InactiveFontAttributes="Bold" 
            ActiveFontSize="16" 
            InactiveFontSize="16" />
    </sliders:SfDateTimeRangeSlider.LabelStyle>
    
</sliders:SfDateTimeRangeSlider>
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
rangeSlider.LabelStyle.ActiveTextColor = Color.FromArgb("#EE3F3F");
rangeSlider.LabelStyle.InactiveTextColor = Color.FromArgb("#F7B1AE");
rangeSlider.LabelStyle.ActiveFontSize = 16;
rangeSlider.LabelStyle.InactiveFontSize = 16;
rangeSlider.LabelStyle.ActiveFontAttributes = FontAttributes.Italic;
rangeSlider.LabelStyle.InactiveFontAttributes = FontAttributes.Bold;
```

## Label Offset

Adjust spacing between ticks and labels.

### Property

- `Offset` (double) - Distance in pixels
  - Default: 5.0 (with ticks)
  - Default: 15.0 (without ticks)

### Example

```xaml
<sliders:SfDateTimeRangeSlider 
    Minimum="2010-01-01" 
    Maximum="2018-01-01" 
    RangeStart="2012-01-01" 
    RangeEnd="2016-01-01" 
    Interval="2" 
    ShowLabels="True" 
    ShowTicks="True">
  
    <sliders:SfDateTimeRangeSlider.LabelStyle>
        <sliders:SliderLabelStyle Offset="10" />
    </sliders:SfDateTimeRangeSlider.LabelStyle>

</sliders:SfDateTimeRangeSlider>
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
rangeSlider.LabelStyle.Offset = 10;
```

## Custom Label Text

Use the `LabelCreated` event to fully customize label text and style.

### Event

- `LabelCreated` (EventHandler<SliderLabelCreatedEventArgs>)
  - `Text` (string) - Modify label text
  - `Style` (SliderLabelStyle) - Modify label appearance

### Example: Quarter Labels

```xaml
<sliders:SfDateTimeRangeSlider 
    Minimum="2010-01-01"
    Maximum="2011-01-01"
    RangeStart="2010-04-01"
    RangeEnd="2010-10-01"
    Interval="3"
    DateFormat="MMM"
    ShowTicks="True"
    LabelsPlacement="BetweenTicks"
    IntervalType="Months"
    LabelCreated="OnLabelCreated"
    ShowLabels="True" />
```

```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider()
{
    Minimum = new DateTime(2010, 01, 01),
    Maximum = new DateTime(2011, 01, 01),
    RangeStart = new DateTime(2010, 04, 01),
    RangeEnd = new DateTime(2010, 10, 01),
    Interval = 3,
    DateFormat = "MMM",
    IntervalType = SliderDateIntervalType.Months,
    LabelsPlacement = SliderLabelsPlacement.BetweenTicks,
    ShowTicks = true,
    ShowLabels = true,
};
rangeSlider.LabelCreated += OnLabelCreated;

private void OnLabelCreated(object sender, SliderLabelCreatedEventArgs e)
{
    if (e.Text == "Jan")
        e.Text = "Quarter 1";
    else if (e.Text == "Apr")
        e.Text = "Quarter 2";
    else if (e.Text == "Jul")
        e.Text = "Quarter 3";
    else
        e.Text = "Quarter 4";
}
```

**Result:** Labels show "Quarter 1", "Quarter 2", "Quarter 3", "Quarter 4"

## Disabled Labels (Visual State Manager)

Customize label appearance when `IsEnabled = false`.

### Example

```xaml
<ContentPage.Resources>
    <Style TargetType="sliders:SfDateTimeRangeSlider">
        <Setter Property="Minimum" Value="2010-01-01" />
        <Setter Property="Maximum" Value="2018-01-01" />
        <Setter Property="RangeStart" Value="2012-01-01" />
        <Setter Property="RangeEnd" Value="2016-01-01" />
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
                                    <sliders:SliderLabelStyle 
                                        ActiveTextColor="#EE3F3F"
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
                                    <sliders:SliderLabelStyle 
                                        ActiveTextColor="Gray"
                                        InactiveTextColor="LightGray"
                                        ActiveFontSize="14"
                                        InactiveFontSize="16"
                                        ActiveFontAttributes="Italic"
                                        InactiveFontAttributes="Bold" />
                                </Setter.Value>
                            </Setter>
                            <Setter Property="ThumbStyle">
                                <Setter.Value>
                                    <sliders:SliderThumbStyle Fill="Gray" />
                                </Setter.Value>
                            </Setter>
                            <Setter Property="TrackStyle">
                                <Setter.Value>
                                    <sliders:SliderTrackStyle 
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
        <Label Text="Enabled" Padding="10" />
        <sliders:SfDateTimeRangeSlider />
        
        <Label Text="Disabled" Padding="10" />
        <sliders:SfDateTimeRangeSlider IsEnabled="False" />
    </VerticalStackLayout>
</ContentPage.Content>
```

## Summary

**Interval Properties:**

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Interval` | double | 0 | Step between labels/ticks |
| `IntervalType` | SliderDateIntervalType | - | Years, Months, Days, Hours, Minutes, Seconds |
| `DateFormat` | string | Empty | Label format pattern |
| `ShowLabels` | bool | false | Display labels |
| `LabelsPlacement` | SliderLabelsPlacement | OnTicks | Label positioning |
| `EdgeLabelsPlacement` | SliderEdgeLabelsPlacement | Default | Edge label handling |

**LabelStyle Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `ActiveTextColor` | Color | Text color between thumbs |
| `InactiveTextColor` | Color | Text color outside thumbs |
| `ActiveFontSize` | double | Font size between thumbs |
| `InactiveFontSize` | double | Font size outside thumbs |
| `ActiveFontFamily` | string | Font family between thumbs |
| `InactiveFontFamily` | string | Font family outside thumbs |
| `ActiveFontAttributes` | FontAttributes | Bold/Italic between thumbs |
| `InactiveFontAttributes` | FontAttributes | Bold/Italic outside thumbs |
| `Offset` | double | Distance from ticks (5.0 or 15.0) |

**Next:** See [ticks-and-dividers.md](ticks-and-dividers.md) for tick customization.
