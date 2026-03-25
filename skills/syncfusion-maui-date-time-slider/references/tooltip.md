# Tooltip in .NET MAUI DateTime Slider

This guide covers tooltip configuration and customization for displaying formatted date/time values during thumb interaction.

## Overview

The tooltip displays the current date/time value above the thumb during interaction. It provides immediate visual feedback about the selected value without requiring the user to look at external labels.

**Default Behavior:** Tooltip appears during thumb drag and hides when released.

## Basic Tooltip Configuration

### Enable Always-Visible Tooltip

Show tooltip permanently (even when not dragging):

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2018-01-01"
                          Value="2014-01-01">
    <sliders:SfDateTimeSlider.Tooltip>
        <sliders:SliderTooltip ShowAlways="True" />
    </sliders:SfDateTimeSlider.Tooltip>
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

slider.Tooltip.ShowAlways = true;
```

**Default:** `false` (tooltip shows only during drag)

## Tooltip Date Formatting

### Default Format

By default, tooltip uses slider's `DateFormat`:

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2020-01-01"
                          Value="2015-01-01"
                          DateFormat="yyyy" />
```

**Result:** Tooltip displays "2015"

### Custom Tooltip Format

Override tooltip format independently:

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2020-01-01"
                          Value="2015-06-15"
                          DateFormat="yyyy">
    <sliders:SfDateTimeSlider.Tooltip>
        <sliders:SliderTooltip DateFormat="MMM dd, yyyy" />
    </sliders:SfDateTimeSlider.Tooltip>
</sliders:SfDateTimeSlider>
```

**Result:** Tooltip displays "Jun 15, 2015" while labels show "2015"

**C# Implementation:**

```csharp
slider.Tooltip.DateFormat = "MMM dd, yyyy";
```

### Common Date Format Patterns

| Pattern | Example | Description |
|---------|---------|-------------|
| `"yyyy"` | 2024 | 4-digit year |
| `"MMM"` | Jan | Abbreviated month |
| `"MMMM"` | January | Full month name |
| `"dd"` | 08 | Day with leading zero |
| `"d"` | 8 | Day without leading zero |
| `"MMM dd"` | Jan 08 | Month + day |
| `"MMM yyyy"` | Jan 2024 | Month + year |
| `"MMM dd, yyyy"` | Jan 08, 2024 | Full date |
| `"MMMM d, yyyy"` | January 8, 2024 | Full date, no leading zero |
| `"M/d/yyyy"` | 1/8/2024 | Numeric date |
| `"yyyy-MM-dd"` | 2024-01-08 | ISO 8601 format |
| `"ddd, MMM dd"` | Mon, Jan 08 | Weekday + date |
| `"hh:mm tt"` | 03:30 PM | 12-hour time |
| `"HH:mm"` | 15:30 | 24-hour time |

## Tooltip Styling

### Background Color

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2018-01-01"
                          Value="2014-01-01">
    <sliders:SfDateTimeSlider.Tooltip>
        <sliders:SliderTooltip Fill="#2196F3" />
    </sliders:SfDateTimeSlider.Tooltip>
</sliders:SfDateTimeSlider>
```

**C# Implementation:**

```csharp
slider.Tooltip.Fill = new SolidColorBrush(Color.FromArgb("#2196F3"));
```

### Border Styling

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2018-01-01"
                          Value="2014-01-01">
    <sliders:SfDateTimeSlider.Tooltip>
        <sliders:SliderTooltip Fill="White"
                               Stroke="#EE3F3F"
                               StrokeThickness="2" />
    </sliders:SfDateTimeSlider.Tooltip>
</sliders:SfDateTimeSlider>
```

**C# Implementation:**

```csharp
slider.Tooltip.Fill = new SolidColorBrush(Colors.White);
slider.Tooltip.Stroke = new SolidColorBrush(Color.FromArgb("#EE3F3F"));
slider.Tooltip.StrokeThickness = 2;
```

### Text Color

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2018-01-01"
                          Value="2014-01-01">
    <sliders:SfDateTimeSlider.Tooltip>
        <sliders:SliderTooltip Fill="#2196F3"
                               TextColor="White" />
    </sliders:SfDateTimeSlider.Tooltip>
</sliders:SfDateTimeSlider>
```

**C# Implementation:**

```csharp
slider.Tooltip.TextColor = Colors.White;
```

### Font Customization

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2018-01-01"
                          Value="2014-01-01">
    <sliders:SfDateTimeSlider.Tooltip>
        <sliders:SliderTooltip FontSize="16"
                               FontFamily="Arial"
                               FontAttributes="Bold" />
    </sliders:SfDateTimeSlider.Tooltip>
</sliders:SfDateTimeSlider>
```

**C# Implementation:**

```csharp
slider.Tooltip.FontSize = 16;
slider.Tooltip.FontFamily = "Arial";
slider.Tooltip.FontAttributes = FontAttributes.Bold;
```

### Padding

Add spacing between text and tooltip border:

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2018-01-01"
                          Value="2014-01-01">
    <sliders:SfDateTimeSlider.Tooltip>
        <sliders:SliderTooltip Padding="12,8,12,8" />
    </sliders:SfDateTimeSlider.Tooltip>
</sliders:SfDateTimeSlider>
```

**C# Implementation:**

```csharp
slider.Tooltip.Padding = new Thickness(12, 8, 12, 8);
```

## Complete Tooltip Styling Example

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2020-01-01"
                          Value="2015-06-15"
                          Interval="2"
                          ShowLabels="True"
                          ShowTicks="True">
    <sliders:SfDateTimeSlider.Tooltip>
        <sliders:SliderTooltip ShowAlways="True"
                               DateFormat="MMMM d, yyyy"
                               Fill="#2196F3"
                               TextColor="White"
                               Stroke="White"
                               StrokeThickness="2"
                               FontSize="14"
                               FontFamily="Arial"
                               FontAttributes="Bold"
                               Padding="16,10,16,10" />
    </sliders:SfDateTimeSlider.Tooltip>
</sliders:SfDateTimeSlider>
```

**C# Equivalent:**

```csharp
var slider = new SfDateTimeSlider
{
    Minimum = new DateTime(2010, 01, 01),
    Maximum = new DateTime(2020, 01, 01),
    Value = new DateTime(2015, 06, 15),
    Interval = 2,
    ShowLabels = true,
    ShowTicks = true
};

slider.Tooltip.ShowAlways = true;
slider.Tooltip.DateFormat = "MMMM d, yyyy";
slider.Tooltip.Fill = new SolidColorBrush(Color.FromArgb("#2196F3"));
slider.Tooltip.TextColor = Colors.White;
slider.Tooltip.Stroke = new SolidColorBrush(Colors.White);
slider.Tooltip.StrokeThickness = 2;
slider.Tooltip.FontSize = 14;
slider.Tooltip.FontFamily = "Arial";
slider.Tooltip.FontAttributes = FontAttributes.Bold;
slider.Tooltip.Padding = new Thickness(16, 10, 16, 10);
```

## Custom Tooltip Content

Use the `TooltipLabelCreated` event to customize tooltip text:

```csharp
slider.TooltipLabelCreated += OnTooltipLabelCreated;

private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    var date = e.Date;
    
    // Custom format: "Q1 2024" for quarters
    int quarter = (date.Month - 1) / 3 + 1;
    e.Text = $"Q{quarter} {date.Year}";
}
```

**Example 2: Relative Time**

```csharp
private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    var date = e.Date;
    var today = DateTime.Today;
    
    if (date.Date == today)
        e.Text = "Today";
    else if (date.Date == today.AddDays(1))
        e.Text = "Tomorrow";
    else if (date.Date == today.AddDays(-1))
        e.Text = "Yesterday";
    else
        e.Text = date.ToString("MMM dd, yyyy");
}
```

**Example 3: Days Until Event**

```csharp
private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    var eventDate = new DateTime(2024, 12, 25); // Christmas
    var daysUntil = (eventDate - e.Date).Days;
    
    e.Text = daysUntil >= 0
        ? $"{daysUntil} days until event"
        : $"{Math.Abs(daysUntil)} days after event";
}
```

## Properties Reference

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `ShowAlways` | bool | false | Show tooltip permanently (not just during drag) |
| `DateFormat` | string | Slider's DateFormat | Date format pattern for tooltip text |
| `Fill` | Brush | Theme default | Background color of tooltip |
| `Stroke` | Brush | Transparent | Border color of tooltip |
| `StrokeThickness` | double | 0.0 | Border width of tooltip |
| `TextColor` | Color | Theme default | Color of tooltip text |
| `FontSize` | double | 14.0 | Font size of tooltip text |
| `FontFamily` | string | Default font | Font family name |
| `FontAttributes` | FontAttributes | None | Bold, Italic, or Bold+Italic |
| `Padding` | Thickness | 8,4,8,4 | Space between text and tooltip border |

## Common Patterns

### Pattern 1: Material Design Tooltip

```csharp
slider.Tooltip.Fill = new SolidColorBrush(Color.FromArgb("#616161"));
slider.Tooltip.TextColor = Colors.White;
slider.Tooltip.FontSize = 14;
slider.Tooltip.Padding = new Thickness(12, 8, 12, 8);
slider.Tooltip.DateFormat = "MMM d, yyyy";
```

### Pattern 2: Outlined Tooltip

```csharp
slider.Tooltip.Fill = new SolidColorBrush(Colors.White);
slider.Tooltip.Stroke = new SolidColorBrush(Color.FromArgb("#DDDDDD"));
slider.Tooltip.StrokeThickness = 1;
slider.Tooltip.TextColor = Colors.Black;
slider.Tooltip.Padding = new Thickness(12, 8, 12, 8);
```

### Pattern 3: Always-Visible Year Display

```csharp
slider.Tooltip.ShowAlways = true;
slider.Tooltip.DateFormat = "yyyy";
slider.Tooltip.FontSize = 18;
slider.Tooltip.FontAttributes = FontAttributes.Bold;
slider.Tooltip.Fill = new SolidColorBrush(Color.FromArgb("#2196F3"));
slider.Tooltip.TextColor = Colors.White;
```

## Best Practices

1. **Format Consistency**: Match tooltip format to slider's IntervalType (Years → "yyyy", Months → "MMM yyyy", etc.)
2. **Readability**: Use sufficient contrast between text and background
3. **Padding**: Add enough padding (8-16px) for comfortable reading
4. **ShowAlways**: Enable for year/month selection where precision is less critical
5. **Custom Text**: Use `TooltipLabelCreated` for localized or context-specific formats
6. **Font Size**: 12-16px works well for most screen sizes

## Troubleshooting

### Tooltip Not Appearing
- Tooltip only shows during thumb drag by default
- Set `ShowAlways="True"` for persistent display
- Ensure tooltip isn't hidden by other UI elements

### Tooltip Text Truncated
- Increase `Padding` to give more space
- Reduce `FontSize` if text is too long
- Use shorter `DateFormat` (e.g., "M/d/yy" instead of "MMMM d, yyyy")

### Tooltip Color Invisible
- Check contrast between `Fill` and background
- Verify `TextColor` contrasts with `Fill`
- Ensure `Fill` isn't transparent

### Tooltip Format Not Applied
- Verify `DateFormat` uses valid .NET date format specifiers
- Check for typos in format string
- Use `TooltipLabelCreated` event for complex formats

## Next Steps

- **Events**: Handle `TooltipLabelCreated` for dynamic content
- **Thumb Styling**: Coordinate tooltip and thumb colors
- **Intervals**: Configure discrete selection with tooltip feedback
