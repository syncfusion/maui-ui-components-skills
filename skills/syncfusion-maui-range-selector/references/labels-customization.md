# Labels Customization

This guide covers how to configure and customize labels in the .NET MAUI Range Selector (SfRangeSelector), including enabling labels, formatting, placement, styling, and custom text.

## Table of Contents
- [Overview](#overview)
- [Show Labels](#show-labels)
- [Number Format](#number-format)
- [Label Placement](#label-placement)
- [Edge Labels Placement](#edge-labels-placement)
- [Label Style](#label-style)
- [Label Offset](#label-offset)
- [Custom Label Text](#custom-label-text)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)

## Overview

Labels display the values at specific intervals along the Range Selector track. Labels help users understand the numeric values they're selecting. You can customize:
- When and where labels appear
- How values are formatted (currency, percentage, etc.)
- Text styling (colors, fonts, sizes)
- Position offset from track

Labels are rendered based on the `Interval` property. For example, with Minimum=0, Maximum=10, Interval=2, labels appear at 0, 2, 4, 6, 8, 10.

## Show Labels

Enable labels using the `ShowLabels` property.

**Property:**
- `ShowLabels` (bool): Renders labels at each interval. Default: `false`

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="0"
                         Maximum="10"
                         RangeStart="2"
                         RangeEnd="8"
                         Interval="2"
                         ShowLabels="True"
                         ShowTicks="True">
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
SfRangeSelector rangeSelector = new SfRangeSelector
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

**Note:** If `ShowLabels = true` but `Interval = 0`, the interval is auto-calculated based on available space.

## Number Format

Format label text using the `NumberFormat` property. This adds prefixes, suffixes, or controls decimal places.

**Property:**
- `NumberFormat` (string): Format pattern for numeric labels. Default: `"0.##"`

**Common Formats:**
- `"$#"` - Currency prefix: $0, $2, $4
- `"#%"` - Percentage suffix: 0%, 2%, 4%
- `"0.00"` - Two decimal places: 0.00, 2.00, 4.00
- `"#.#"` - One decimal place: 0, 2, 4
- `"€ #"` - Euro prefix: € 0, € 2, € 4
- `"# kg"` - Unit suffix: 0 kg, 2 kg, 4 kg

**XAML - Currency:**
```xaml
<sliders:SfRangeSelector Minimum="0"
                         Maximum="100"
                         RangeStart="20"
                         RangeEnd="80"
                         Interval="20"
                         NumberFormat="$#"
                         ShowLabels="True"
                         ShowTicks="True">
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C# - Percentage:**
```csharp
SfRangeSelector rangeSelector = new SfRangeSelector
{
    Minimum = 0,
    Maximum = 100,
    RangeStart = 20,
    RangeEnd = 80,
    Interval = 20,
    NumberFormat = "#%",
    ShowLabels = true,
    ShowTicks = true
};
```

**Advanced Formatting:**
```xaml
<!-- Two decimal places with currency -->
<sliders:SfRangeSelector NumberFormat="$#.00" ... />
<!-- Result: $0.00, $20.00, $40.00 -->

<!-- Thousands separator -->
<sliders:SfRangeSelector Minimum="0" Maximum="10000" Interval="2000"
                         NumberFormat="#,0" ... />
<!-- Result: 0, 2,000, 4,000, 6,000, 8,000, 10,000 -->
```

## Label Placement

Control whether labels appear on ticks or between ticks using the `LabelsPlacement` property.

**Property:**
- `LabelsPlacement` (SliderLabelsPlacement): Label position relative to ticks. Default: `OnTicks`

**Values:**
- `OnTicks`: Labels align with tick marks
- `BetweenTicks`: Labels centered between tick marks

**XAML - Between Ticks:**
```xaml
<sliders:SfRangeSelector Minimum="0"
                         Maximum="10"
                         RangeStart="2"
                         RangeEnd="8"
                         Interval="2"
                         LabelsPlacement="BetweenTicks"
                         ShowLabels="True"
                         ShowTicks="True">
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
SfRangeSelector rangeSelector = new SfRangeSelector
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

**Visual Comparison:**

**OnTicks (default):**
```
  0     2     4     6     8     10
  |     |     |     |     |     |
```

**BetweenTicks:**
```
     1     3     5     7     9
  |     |     |     |     |     |
```

## Edge Labels Placement

Control the position of edge labels (first and last) using the `EdgeLabelsPlacement` property.

**Property:**
- `EdgeLabelsPlacement` (SliderEdgeLabelsPlacement): Position of edge labels. Default: `Default`

**Values:**
- `Default`: Labels at exact minimum/maximum positions
- `Inside`: Labels moved inside track bounds to prevent clipping

**Use Cases for Inside:**
- Prevent label text from being cut off at screen edges
- Align labels within a constrained container
- Used with `TrackExtent` for better visual balance

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="100"
                         Maximum="1000"
                         RangeStart="325"
                         RangeEnd="775"
                         Interval="225"
                         NumberFormat="$#"
                         ShowLabels="True"
                         ShowTicks="True"
                         EdgeLabelsPlacement="Inside">
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
SfRangeSelector rangeSelector = new SfRangeSelector
{
    Minimum = 100,
    Maximum = 1000,
    RangeStart = 325,
    RangeEnd = 775,
    Interval = 225,
    NumberFormat = "$#",
    EdgeLabelsPlacement = SliderEdgeLabelsPlacement.Inside,
    ShowLabels = true,
    ShowTicks = true
};
```

**With TrackExtent:**
If `TrackExtent > 0` and `EdgeLabelsPlacement = Inside`, labels are placed inside the extended track edges.

## Label Style

Customize label appearance using the `LabelStyle` property. You can style active and inactive labels differently.

**Properties:**
- **Active Labels** (between thumbs):
  - `ActiveTextColor` (Color): Text color
  - `ActiveFontSize` (double): Font size
  - `ActiveFontFamily` (string): Font family
  - `ActiveFontAttributes` (FontAttributes): Bold, Italic, or None
  
- **Inactive Labels** (outside thumbs):
  - `InactiveTextColor` (Color): Text color
  - `InactiveFontSize` (double): Font size
  - `InactiveFontFamily` (string): Font family
  - `InactiveFontAttributes` (FontAttributes): Bold, Italic, or None

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="0"
                         Maximum="10"
                         RangeStart="2"
                         RangeEnd="8"
                         Interval="2"
                         ShowLabels="True"
                         ShowTicks="True">
    <sliders:SfRangeSelector.LabelStyle>
        <sliders:SliderLabelStyle ActiveTextColor="#EE3F3F"
                                  InactiveTextColor="#F7B1AE"
                                  ActiveFontSize="16"
                                  InactiveFontSize="14"
                                  ActiveFontAttributes="Bold"
                                  InactiveFontAttributes="Italic" />
    </sliders:SfRangeSelector.LabelStyle>
    
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
SfRangeSelector rangeSelector = new SfRangeSelector
{
    Minimum = 0,
    Maximum = 10,
    RangeStart = 2,
    RangeEnd = 8,
    Interval = 2,
    ShowLabels = true,
    ShowTicks = true
};

rangeSelector.LabelStyle = new SliderLabelStyle
{
    ActiveTextColor = Color.FromArgb("#EE3F3F"),
    InactiveTextColor = Color.FromArgb("#F7B1AE"),
    ActiveFontSize = 16,
    InactiveFontSize = 14,
    ActiveFontAttributes = FontAttributes.Bold,
    InactiveFontAttributes = FontAttributes.Italic
};
```

**Custom Font Family:**
```xaml
<sliders:SfRangeSelector.LabelStyle>
    <sliders:SliderLabelStyle ActiveFontFamily="Roboto-Bold"
                              InactiveFontFamily="Roboto-Regular"
                              ActiveFontSize="14"
                              InactiveFontSize="12" />
</sliders:SfRangeSelector.LabelStyle>
```

## Label Offset

Adjust the vertical spacing between labels and the track using the `Offset` property.

**Property:**
- `Offset` (double): Distance in pixels between track and labels.
  - Default with ticks: `5.0`
  - Default without ticks: `15.0`

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="0"
                         Maximum="10"
                         RangeStart="2"
                         RangeEnd="8"
                         Interval="2"
                         ShowLabels="True"
                         ShowTicks="True">
    <sliders:SfRangeSelector.LabelStyle>
        <sliders:SliderLabelStyle Offset="10" />
    </sliders:SfRangeSelector.LabelStyle>
    
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
SfRangeSelector rangeSelector = new SfRangeSelector
{
    Minimum = 0,
    Maximum = 10,
    RangeStart = 2,
    RangeEnd = 8,
    Interval = 2,
    ShowLabels = true,
    ShowTicks = true
};

rangeSelector.LabelStyle = new SliderLabelStyle
{
    Offset = 10
};
```

**Use Cases:**
- Increase offset to prevent label overlap with ticks
- Decrease offset for compact layouts
- Adjust when using large fonts

## Custom Label Text

Customize label text dynamically using the `LabelCreated` event. This allows complete control over label text and styling.

**Event:**
- `LabelCreated`: Fires when each label is created

**Event Args (SliderLabelCreatedEventArgs):**
- `Text` (string): Get/set the label text
- `Style` (SliderLabelStyle): Get/set label style

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="2"
                         Maximum="10"
                         RangeStart="4"
                         RangeEnd="8"
                         Interval="2"
                         ShowLabels="True"
                         ShowTicks="True"
                         LabelCreated="OnLabelCreated">
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C# Event Handler:**
```csharp
private void OnLabelCreated(object sender, SliderLabelCreatedEventArgs e)
{
    // Add currency prefix
    e.Text = "$" + e.Text;
}
```

**Advanced Customization:**
```csharp
private void OnLabelCreated(object sender, SliderLabelCreatedEventArgs e)
{
    // Parse the numeric value
    if (double.TryParse(e.Text, out double value))
    {
        if (value < 0)
        {
            e.Text = $"({Math.Abs(value)})";  // Parentheses for negative
            e.Style.ActiveTextColor = Colors.Red;
        }
        else if (value == 0)
        {
            e.Text = "Zero";
        }
        else
        {
            e.Text = "+" + e.Text;
            e.Style.ActiveTextColor = Colors.Green;
        }
    }
}
```

**Custom Units:**
```csharp
private void OnLabelCreated(object sender, SliderLabelCreatedEventArgs e)
{
    if (double.TryParse(e.Text, out double value))
    {
        if (value >= 1000)
            e.Text = (value / 1000).ToString("0.#") + "K";
        else
            e.Text = value.ToString() + " m";
    }
}
// Result: 0 m, 500 m, 1K, 1.5K, 2K
```

## Complete Examples

### Price Range with Currency
```xaml
<sliders:SfRangeSelector Minimum="0"
                         Maximum="500"
                         RangeStart="100"
                         RangeEnd="400"
                         Interval="100"
                         NumberFormat="$#,0"
                         ShowLabels="True"
                         ShowTicks="True"
                         EdgeLabelsPlacement="Inside">
    <sliders:SfRangeSelector.LabelStyle>
        <sliders:SliderLabelStyle ActiveTextColor="#2196F3"
                                  InactiveTextColor="#90CAF9"
                                  ActiveFontSize="14"
                                  InactiveFontSize="12"
                                  ActiveFontAttributes="Bold" />
    </sliders:SfRangeSelector.LabelStyle>
    
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

### Percentage Range with Custom Styling
```xaml
<sliders:SfRangeSelector Minimum="0"
                         Maximum="100"
                         RangeStart="25"
                         RangeEnd="75"
                         Interval="25"
                         NumberFormat="#%"
                         ShowLabels="True"
                         ShowTicks="True">
    <sliders:SfRangeSelector.LabelStyle>
        <sliders:SliderLabelStyle ActiveTextColor="#4CAF50"
                                  InactiveTextColor="#A5D6A7"
                                  ActiveFontSize="16"
                                  InactiveFontSize="14"
                                  Offset="8" />
    </sliders:SfRangeSelector.LabelStyle>
    
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

## Best Practices

### Label Visibility
- Always use `ShowLabels="True"` when labels are important for value selection
- Set appropriate `Interval` to avoid label overcrowding
- Use `EdgeLabelsPlacement="Inside"` for constrained layouts

### Formatting
- Use `NumberFormat` for consistent value display
- Match format to data context (currency, percentage, units)
- Keep format strings simple and readable

### Styling
- Use contrasting colors for active/inactive labels
- Keep font sizes readable (minimum 12-14 for mobile)
- Use bold for active labels to emphasize selected range

### Custom Labels
- Use `LabelCreated` event for dynamic formatting requirements
- Avoid heavy computations in label event handlers
- Cache format strings or patterns when possible

### Layout
- Adjust `Offset` if labels overlap with ticks or dividers
- Consider `TrackExtent` if edge labels get clipped
- Test on different screen sizes to ensure readability
