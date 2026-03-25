# Labels in DateTime Range Selector

## Table of Contents
- [Overview](#overview)
- [Show Labels](#show-labels)
- [Date Format](#date-format)
- [Label Placement](#label-placement)
- [Edge Labels Placement](#edge-labels-placement)
- [Label Styling](#label-styling)
- [Label Offset](#label-offset)
- [Custom Label Text](#custom-label-text)

## Overview

Labels display date/time values at specified intervals along the track. They provide context for the selected range and help users understand the time scale.

## Show Labels

Enable labels using the `ShowLabels` property. The default value is `False`.

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2018-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2016-01-01"
                                 Interval="2" 
                                 ShowLabels="True"
                                 ShowTicks="True">
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

```csharp
rangeSelector.ShowLabels = true;
rangeSelector.Interval = 2;
```

## Date Format

Customize the date format using the `DateFormat` property with standard .NET format strings.

### Common Formats

```xaml
<!-- Hours format -->
<sliders:SfDateTimeRangeSelector Minimum="2000-01-01T09:00:00" 
                                 Maximum="2000-01-01T17:00:00" 
                                 RangeStart="2000-01-01T11:00:00" 
                                 RangeEnd="2000-01-01T15:00:00" 
                                 IntervalType="Hours" 
                                 Interval="2" 
                                 DateFormat="h tt"
                                 ShowLabels="True">
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

```csharp
rangeSelector.DateFormat = "h tt"; // "9 AM", "11 AM", etc.
rangeSelector.IntervalType = SliderDateIntervalType.Hours;
```

### Format Examples

| DateFormat | Output Example |
|------------|----------------|
| `yyyy` | 2024 |
| `MMM` | Jan |
| `MMM yyyy` | Jan 2024 |
| `dd/MM/yyyy` | 15/01/2024 |
| `h tt` | 2 PM |
| `HH:mm` | 14:30 |
| `ddd, MMM dd` | Mon, Jan 15 |

## Label Placement

Control whether labels appear on ticks or between ticks using the `LabelsPlacement` property.

### On Ticks (Default)

Labels align with tick marks:

```xaml
<sliders:SfDateTimeRangeSelector LabelsPlacement="OnTicks"
                                 ShowLabels="True"
                                 ShowTicks="True">
    <!-- Content -->
</sliders:SfDateTimeRangeSelector>
```

### Between Ticks

Labels appear between tick marks:

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2011-01-01"
                                 Maximum="2016-01-01"
                                 RangeStart="2012-01-01"
                                 RangeEnd="2015-01-01"
                                 Interval="1"
                                 LabelsPlacement="BetweenTicks"
                                 ShowLabels="True"
                                 ShowTicks="True">
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

```csharp
rangeSelector.LabelsPlacement = SliderLabelsPlacement.BetweenTicks;
```

## Edge Labels Placement

The `EdgeLabelsPlacement` property controls how the first and last labels are positioned.

### Default Placement

Labels follow normal interval positioning:

```xaml
<sliders:SfDateTimeRangeSelector EdgeLabelsPlacement="Default" />
```

### Inside Placement

First and last labels are moved inside the track bounds when `TrackExtent` > 0:

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2002-01-07"
                                 Maximum="2002-01-13"
                                 RangeStart="2002-01-09"
                                 RangeEnd="2002-01-11"
                                 Interval="1"
                                 IntervalType="Days"
                                 DateFormat="ddd" 
                                 EdgeLabelsPlacement="Inside" 
                                 ShowLabels="True" 
                                 ShowTicks="True">
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

```csharp
rangeSelector.EdgeLabelsPlacement = SliderEdgeLabelsPlacement.Inside;
```

**Use Case:** Prevents edge labels from being cut off at screen edges.

## Label Styling

Customize label appearance using the `LabelStyle` property. You can style active and inactive labels differently.

### Active vs Inactive Labels

- **Active labels**: Between start and end thumbs
- **Inactive labels**: Outside the selected range

### Text Color

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2018-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2016-01-01"
                                 Interval="2"  
                                 ShowLabels="True" 
                                 ShowTicks="True">
    <sliders:SfDateTimeRangeSelector.LabelStyle>
        <sliders:SliderLabelStyle ActiveTextColor="#EE3F3F" 
                                  InactiveTextColor="#F7B1AE" />
    </sliders:SfDateTimeRangeSelector.LabelStyle>
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

```csharp
rangeSelector.LabelStyle.ActiveTextColor = Color.FromArgb("#EE3F3F");
rangeSelector.LabelStyle.InactiveTextColor = Color.FromArgb("#F7B1AE");
```

### Font Properties

```xaml
<sliders:SfDateTimeRangeSelector.LabelStyle>
    <sliders:SliderLabelStyle ActiveTextColor="#EE3F3F" 
                              InactiveTextColor="#F7B1AE" 
                              ActiveFontSize="16" 
                              InactiveFontSize="14"
                              ActiveFontFamily="Arial"
                              InactiveFontFamily="Arial"
                              ActiveFontAttributes="Bold" 
                              InactiveFontAttributes="Italic" />
</sliders:SfDateTimeRangeSelector.LabelStyle>
```

```csharp
rangeSelector.LabelStyle.ActiveFontSize = 16;
rangeSelector.LabelStyle.InactiveFontSize = 14;
rangeSelector.LabelStyle.ActiveFontAttributes = FontAttributes.Bold;
rangeSelector.LabelStyle.InactiveFontAttributes = FontAttributes.Italic;
rangeSelector.LabelStyle.ActiveFontFamily = "Arial";
rangeSelector.LabelStyle.InactiveFontFamily = "Arial";
```

### Complete Styling Example

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2018-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2016-01-01"
                                 Interval="2"  
                                 ShowLabels="True" 
                                 ShowTicks="True">
    <sliders:SfDateTimeRangeSelector.LabelStyle>
        <sliders:SliderLabelStyle ActiveTextColor="#EE3F3F" 
                                  InactiveTextColor="#999999" 
                                  ActiveFontAttributes="Bold" 
                                  InactiveFontAttributes="Normal" 
                                  ActiveFontSize="16" 
                                  InactiveFontSize="14"
                                  Offset="10" />
    </sliders:SfDateTimeRangeSelector.LabelStyle>
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

## Label Offset

Adjust the space between ticks and labels using the `Offset` property.

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2018-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2016-01-01"
                                 Interval="2" 
                                 ShowLabels="True" 
                                 ShowTicks="True">
    <sliders:SfDateTimeRangeSelector.LabelStyle>
        <sliders:SliderLabelStyle Offset="10" />
    </sliders:SfDateTimeRangeSelector.LabelStyle>
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

```csharp
rangeSelector.LabelStyle.Offset = 10;
```

**Default Values:**
- `Offset = 5.0` when `ShowTicks = true`
- `Offset = 15.0` when `ShowTicks = false`

## Custom Label Text

Customize label text using the `LabelCreated` event. This allows you to format labels in ways not possible with `DateFormat` alone.

### Basic Custom Labels

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01"
                                 Maximum="2011-01-01"
                                 RangeStart="2010-04-01"
                                 RangeEnd="2010-10-01"
                                 Interval="3"
                                 DateFormat="MMM"
                                 IntervalType="Months"
                                 LabelsPlacement="BetweenTicks"
                                 ShowTicks="True"
                                 ShowLabels="True"
                                 LabelCreated="OnLabelCreated">
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

```csharp
private void OnLabelCreated(object sender, SliderLabelCreatedEventArgs e)
{
    // Convert month abbreviations to quarters
    if (e.Text == "Jan")
    {
        e.Text = "Q1";
    }
    else if (e.Text == "Apr")
    {
        e.Text = "Q2";
    }
    else if (e.Text == "Jul")
    {
        e.Text = "Q3";
    }
    else if (e.Text == "Oct")
    {
        e.Text = "Q4";
    }
}
```

### Advanced Custom Styling

```csharp
private void OnLabelCreated(object sender, SliderLabelCreatedEventArgs e)
{
    // Customize both text and style
    e.Text = "Year " + e.Text;
    
    // Customize style
    e.Style = new SliderLabelStyle()
    {
        ActiveTextColor = Colors.Blue,
        InactiveTextColor = Colors.Gray,
        ActiveFontSize = 14,
        InactiveFontSize = 12
    };
}
```

### Event Arguments Properties

| Property | Type | Description |
|----------|------|-------------|
| `Text` | string | Label text to display |
| `Style` | SliderLabelStyle | Label style (color, font, size, offset) |

## Common Label Patterns

### Pattern 1: Quarterly Labels

```csharp
// XAML: IntervalType="Months" Interval="3"
private void OnLabelCreated(object sender, SliderLabelCreatedEventArgs e)
{
    int month = DateTime.Parse(e.Text).Month;
    int quarter = (month - 1) / 3 + 1;
    e.Text = $"Q{quarter}";
}
```

### Pattern 2: Fiscal Year Labels

```csharp
private void OnLabelCreated(object sender, SliderLabelCreatedEventArgs e)
{
    DateTime date = DateTime.Parse(e.Text);
    int fiscalYear = date.Month >= 7 ? date.Year + 1 : date.Year;
    e.Text = $"FY{fiscalYear}";
}
```

### Pattern 3: Abbreviated Weekday Labels

```csharp
// XAML: IntervalType="Days" Interval="1"
private void OnLabelCreated(object sender, SliderLabelCreatedEventArgs e)
{
    DateTime date = DateTime.Parse(e.Text);
    e.Text = date.ToString("ddd"); // Mon, Tue, Wed...
}
```

## Best Practices

1. **Match Format to Interval**: Use year format for yearly intervals, month format for monthly intervals, etc.
2. **Consider Label Length**: Shorter formats prevent overlap on small screens
3. **Use Edge Placement**: Set `EdgeLabelsPlacement="Inside"` when labels might be cut off
4. **Style for Clarity**: Use contrasting colors for active/inactive labels
5. **Custom Labels for Special Cases**: Use `LabelCreated` event for business-specific formatting (quarters, fiscal years, etc.)

## Troubleshooting

**Issue:** Labels overlap
- **Solution:** Increase `Interval`, use shorter `DateFormat`, or enable auto interval

**Issue:** Labels cut off at edges
- **Solution:** Set `EdgeLabelsPlacement="Inside"` and/or increase `TrackExtent`

**Issue:** Labels too close/far from track
- **Solution:** Adjust `LabelStyle.Offset` property

**Issue:** Custom labels not showing
- **Solution:** Ensure `LabelCreated` event is properly wired up and `ShowLabels="True"`

**Issue:** Active/inactive colors not working
- **Solution:** Verify range is set (`RangeStart` and `RangeEnd` differ from `Minimum`/`Maximum`)

## Related Properties

- `ShowTicks` - Display tick marks at label positions
- `Interval` - Spacing between labels
- `DateFormat` - Format pattern for date display
- `EdgeLabelsPlacement` - First/last label positioning
- `LabelsPlacement` - Labels on or between ticks
