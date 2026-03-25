# Tooltip in DateTime Range Selector

## Table of Contents
- [Overview](#overview)
- [Enable Tooltip](#enable-tooltip)
- [Show Always Mode](#show-always-mode)
- [Tooltip Styling](#tooltip-styling)
- [Custom Tooltip Text](#custom-tooltip-text)
- [Common Patterns](#common-patterns)

## Overview

Tooltips display the current date/time value when users interact with the thumbs. They appear:
- On drag (default behavior)
- Always visible (ShowAlways mode)
- With customizable text and styling


## Show Always Mode

Keep tooltips permanently visible using the `SliderTooltip.ShowAlways` property.

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2018-01-01"
                                 >
    <sliders:SfDateTimeRangeSelector.Tooltip>
    <sliders:SliderTooltip ShowAlways="True" />
</sliders:SfDateTimeRangeSelector.Tooltip>
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

**Use Cases:**
- Display selected range at all times
- Dashboard widgets where range visibility is critical
- Data visualization apps requiring constant date reference

## Tooltip Styling

Customize tooltip appearance using the `Tooltip` property.

### Tooltip Fill Color

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2018-01-01"
                                 >
    <sliders:SfDateTimeRangeSelector.Tooltip>
        <sliders:SliderTooltip Fill="#2196F3" />
    </sliders:SfDateTimeRangeSelector.Tooltip>
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

```csharp
rangeSelector.Tooltip.Fill = new SolidColorBrush(Color.FromArgb("#2196F3"));
```

### Tooltip Text Color

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2018-01-01"
                                 >
    <sliders:SfDateTimeRangeSelector.Tooltip>
        <sliders:SliderTooltip Fill="#2196F3" 
                                   TextColor="#FFFFFF" />
    </sliders:SfDateTimeRangeSelector.Tooltip>
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

```csharp
rangeSelector.Tooltip.TextColor = Colors.White;
```

### Tooltip Stroke

Add a border to the tooltip:

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2018-01-01"
                                 >
    <sliders:SfDateTimeRangeSelector.Tooltip>
        <sliders:SliderTooltip Fill="#FFFFFF" 
                                   TextColor="#2196F3"
                                   Stroke="#2196F3" 
                                   StrokeThickness="2" />
    </sliders:SfDateTimeRangeSelector.Tooltip>
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

```csharp
rangeSelector.Tooltip.Fill = new SolidColorBrush(Colors.White);
rangeSelector.Tooltip.TextColor = Color.FromArgb("#2196F3");
rangeSelector.Tooltip.Stroke = new SolidColorBrush(Color.FromArgb("#2196F3"));
rangeSelector.Tooltip.StrokeThickness = 2;
```

### Tooltip Font Properties

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2018-01-01"
                                 >
    <sliders:SfDateTimeRangeSelector.Tooltip>
        <sliders:SliderTooltip Fill="#2196F3" 
                                   TextColor="#FFFFFF"
                                   FontSize="14" 
                                   FontAttributes="Bold"
                                   FontFamily="Arial" />
    </sliders:SfDateTimeRangeSelector.Tooltip>
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

```csharp
rangeSelector.Tooltip.FontSize = 14;
rangeSelector.Tooltip.FontAttributes = FontAttributes.Bold;
rangeSelector.Tooltip.FontFamily = "Arial";
```

### Tooltip Padding

Adjust space inside the tooltip:

```xaml
<sliders:SfDateTimeRangeSelector.Tooltip>
    <sliders:SliderTooltip Fill="#2196F3" 
                               TextColor="#FFFFFF"
                               Padding="10,5,10,5" />
</sliders:SfDateTimeRangeSelector.Tooltip>
```

```csharp
rangeSelector.Tooltip.Padding = new Thickness(10, 5, 10, 5);
```

**Format:** `Left, Top, Right, Bottom`

### Complete Tooltip Styling

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2018-01-01"
                                 >
    <sliders:SfDateTimeRangeSelector.Tooltip>
        <sliders:SliderTooltip Fill="#2196F3" 
                                   TextColor="#FFFFFF"
                                   Stroke="#1565C0" 
                                   StrokeThickness="2"
                                   FontSize="14" 
                                   FontAttributes="Bold"
                                   Padding="12,6,12,6"
                                   ShowAlways="True" />
    </sliders:SfDateTimeRangeSelector.Tooltip>
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

## Custom Tooltip Text

Customize tooltip text using the `TooltipLabelCreated` event. This allows formatting beyond the `DateFormat` property.

### Basic Custom Tooltip

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01"
                                 Maximum="2018-01-01"
                                 RangeStart="2012-01-01"
                                 RangeEnd="2016-01-01"
                                 Interval="2"
                                 ShowLabels="True"
                                 
                                 TooltipLabelCreated="OnTooltipLabelCreated">
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

```csharp
private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    // Format as "Year: 2012"
    DateTime date = (DateTime)e.Value;
    e.Text = $"Year: {date.Year}";
}
```

### Advanced Custom Tooltip

```csharp
private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    DateTime date = (DateTime)e.Value;
    
    // Custom formatting based on interval type
    if (rangeSelector.IntervalType == SliderDateIntervalType.Years)
    {
        e.Text = $"Year: {date.Year}";
    }
    else if (rangeSelector.IntervalType == SliderDateIntervalType.Months)
    {
        e.Text = date.ToString("MMM yyyy");
    }
    else if (rangeSelector.IntervalType == SliderDateIntervalType.Days)
    {
        e.Text = date.ToString("ddd, MMM dd");
    }
    else
    {
        e.Text = date.ToString("g"); // General date/time
    }
}
```

### Event Arguments Properties

| Property | Type | Description |
|----------|------|-------------|
| `Value` | object | Current DateTime value |
| `Text` | string | Tooltip text to display |
| `Style` | SliderTooltip | Tooltip style (color, font, padding) |

### Custom Tooltip with Dynamic Styling

```csharp
private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    DateTime date = (DateTime)e.Value;
    e.Text = date.ToString("MMM dd, yyyy");
    
    // Change tooltip color based on date range
    if (date.Year < 2015)
    {
        e.Style = new SliderTooltip()
        {
            Fill = new SolidColorBrush(Colors.Red),
            TextColor = Colors.White
        };
    }
    else
    {
        e.Style = new SliderTooltip()
        {
            Fill = new SolidColorBrush(Colors.Green),
            TextColor = Colors.White
        };
    }
}
```

## Common Patterns

### Pattern 1: Standard Tooltip (On Drag)

```xaml
<sliders:SfDateTimeRangeSelector >
    <sliders:SfDateTimeRangeSelector.Tooltip>
        <sliders:SliderTooltip Fill="#2196F3" 
                                   TextColor="#FFFFFF" />
    </sliders:SfDateTimeRangeSelector.Tooltip>
</sliders:SfDateTimeRangeSelector>
```

**Use Case:** Standard interaction feedback during dragging.

### Pattern 2: Always Visible Tooltip

```xaml
<sliders:SfDateTimeRangeSelector >
    <sliders:SfDateTimeRangeSelector.Tooltip>
        <sliders:SliderTooltip Fill="#2196F3" 
                                   TextColor="#FFFFFF"
                                   ShowAlways="True" />
    </sliders:SfDateTimeRangeSelector.Tooltip>
</sliders:SfDateTimeRangeSelector>
```

**Use Case:** Dashboard displays, read-only views, constant range visibility.

### Pattern 3: Outlined Tooltip

```xaml
<sliders:SfDateTimeRangeSelector >
    <sliders:SfDateTimeRangeSelector.Tooltip>
        <sliders:SliderTooltip Fill="#FFFFFF" 
                                   TextColor="#2196F3"
                                   Stroke="#2196F3" 
                                   StrokeThickness="2"
                                   ShowAlways="True" />
    </sliders:SfDateTimeRangeSelector.Tooltip>
</sliders:SfDateTimeRangeSelector>
```

**Use Case:** Light backgrounds, minimal design aesthetic.

### Pattern 4: Custom Formatted Tooltip

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01"
                                 Maximum="2020-01-01"
                                 RangeStart="2012-01-01"
                                 RangeEnd="2018-01-01"
                                 IntervalType="Years"
                                 
                                 TooltipLabelCreated="OnTooltipLabelCreated">
    <sliders:SfDateTimeRangeSelector.Tooltip>
        <sliders:SliderTooltip Fill="#FF5722" 
                                   TextColor="#FFFFFF"
                                   FontSize="16"
                                   FontAttributes="Bold" />
    </sliders:SfDateTimeRangeSelector.Tooltip>
</sliders:SfDateTimeRangeSelector>
```

```csharp
private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    DateTime date = (DateTime)e.Value;
    e.Text = $"FY {date.Year}"; // Fiscal Year format
}
```

**Use Case:** Business-specific formatting (fiscal years, quarters, etc.).

### Pattern 5: Contextual Tooltip Colors

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2020-01-01"
                                 Maximum="2024-01-01"
                                 RangeStart="2021-01-01"
                                 RangeEnd="2023-01-01"
                                 
                                 TooltipLabelCreated="OnTooltipLabelCreated">
</sliders:SfDateTimeRangeSelector>
```

```csharp
private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    DateTime date = (DateTime)e.Value;
    e.Text = date.ToString("MMM yyyy");
    
    // Color based on performance zones
    if (date.Year == 2020)
    {
        e.Style = new SliderTooltip() { Fill = new SolidColorBrush(Colors.Red), TextColor = Colors.White };
    }
    else if (date.Year == 2021 || date.Year == 2022)
    {
        e.Style = new SliderTooltip() { Fill = new SolidColorBrush(Colors.Orange), TextColor = Colors.White };
    }
    else
    {
        e.Style = new SliderTooltip() { Fill = new SolidColorBrush(Colors.Green), TextColor = Colors.White };
    }
}
```

**Use Case:** Visual feedback based on date ranges (performance zones, status indicators).

### Pattern 6: Multi-Line Tooltip

```csharp
private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    DateTime date = (DateTime)e.Value;
    
    // Multi-line format
    e.Text = $"{date:MMMM d, yyyy}\n{date:dddd}";
    
    e.Style = new SliderTooltip()
    {
        Fill = new SolidColorBrush(Color.FromArgb("#2196F3")),
        TextColor = Colors.White,
        FontSize = 12,
        Padding = new Thickness(12, 8, 12, 8)
    };
}
```

**Output:**
```
January 15, 2023
Monday
```

**Use Case:** Display additional context (day of week, quarter, week number).

## Best Practices

1. **Always Show for Static Displays**: Use `ShowAlways="True"` for dashboard widgets
2. **Color Contrast**: Ensure 4.5:1 contrast ratio between `Fill` and `TextColor`
3. **Font Size**: Use 12-14px for readability
4. **Padding**: Add sufficient padding (8-12px horizontal, 4-8px vertical)
5. **Custom Formatting**: Use `TooltipLabelCreated` for business-specific formats
6. **Match Theme**: Coordinate tooltip colors with thumb and track colors
7. **Stroke for Light Backgrounds**: Add stroke when using white/light fill colors

## Troubleshooting

**Issue:** Tooltip not showing
- **Solution:** Ensure `` is set

**Issue:** Tooltip disappears immediately
- **Solution:** Set `Tooltip.ShowAlways="True"` for permanent visibility

**Issue:** Tooltip text is default format, not custom
- **Solution:** Verify `TooltipLabelCreated` event is properly wired up

**Issue:** Tooltip text color not visible
- **Solution:** Check contrast between `Fill` and `TextColor` (use white text on dark fill)

**Issue:** Tooltip text cut off
- **Solution:** Increase `Padding` or reduce `FontSize`

**Issue:** Custom tooltip style not applying
- **Solution:** Set `e.Style` in `TooltipLabelCreated` event, not just individual properties

**Issue:** Tooltip position overlaps thumb
- **Solution:** This is expected behavior; tooltip automatically positions above thumb

## Related Properties

- `DateFormat` - Default date format for tooltip text
- `ThumbStyle` - Thumb appearance (tooltip appears above thumb)
- `RangeStart` / `RangeEnd` - Values displayed in tooltips
- `Minimum` / `Maximum` - Range bounds for tooltip values
- `EnableDeferredUpdate` - Affects when tooltip updates during drag
