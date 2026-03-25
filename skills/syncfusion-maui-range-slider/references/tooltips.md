# Tooltips in .NET MAUI Range Slider

## Overview

Tooltips in the .NET MAUI Range Slider (`SfRangeSlider`) display the current value of thumbs during interaction. They provide immediate visual feedback and help users understand the exact value they're selecting. This reference covers all aspects of configuring and customizing tooltips.

## Table of Contents
- [Overview](#overview)
- [Enabling Tooltips](#enabling-tooltips)
- [Tooltip Display Modes](#tooltip-display-modes)
  - [ShowAlways Property](#showalways-property)
- [Tooltip Styling](#tooltip-styling)
  - [Colors and Borders](#colors-and-borders)
  - [Text Styling](#text-styling)
  - [Padding](#padding)
- [Tooltip Formatting](#tooltip-formatting)
  - [NumberFormat](#numberformat)
  - [TooltipLabelCreated Event](#tooltiplabelcreated-event)
- [Common Scenarios](#common-scenarios)
- [Best Practices](#best-practices)
- [Related References](#related-references)

## Enabling Tooltips

### Tooltip Property

Enable tooltips by setting the `Tooltip` property to a `SliderTooltip` instance.

**Type:** `SliderTooltip`  
**Default:** `null` (tooltips disabled)

**XAML Example:**
```xaml
<sliders:SfRangeSlider>
    <sliders:SfRangeSlider.Tooltip>
        <sliders:SliderTooltip />
    </sliders:SfRangeSlider.Tooltip>
</sliders:SfRangeSlider>
```

**C# Example:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider();
rangeSlider.Tooltip = new SliderTooltip();
```

**Behavior:**
- Tooltip appears when thumb is pressed/dragged
- Displays formatted value based on `NumberFormat`
- Automatically positioned above/below thumb
- Separate tooltips for start and end thumbs

## Tooltip Display Modes

### ShowAlways Property

The `ShowAlways` property controls whether tooltips are always visible or only during interaction.

**Type:** `bool`  
**Default:** `false`

**When `false` (Default):**
- Tooltip appears only when thumb is pressed or dragged
- Disappears when touch is released

**When `true`:**
- Tooltip always visible
- Remains visible even without user interaction
- Updates dynamically as thumbs move

**XAML Example:**
```xaml
<sliders:SfRangeSlider>
    <sliders:SfRangeSlider.Tooltip>
        <sliders:SliderTooltip ShowAlways="True" />
    </sliders:SfRangeSlider.Tooltip>
</sliders:SfRangeSlider>
```

**C# Example:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider();
rangeSlider.Tooltip = new SliderTooltip
{
    ShowAlways = true
};
```

## Tooltip Styling

### Colors and Borders

Customize tooltip appearance using fill, stroke, and stroke thickness properties.

**Properties:**
- `Fill` - Background color of tooltip
- `Stroke` - Border color of tooltip
- `StrokeThickness` - Border width

**Type:** 
- `Fill`: `Brush`
- `Stroke`: `Brush`
- `StrokeThickness`: `double`

**XAML Example:**
```xaml
<sliders:SfRangeSlider>
    <sliders:SfRangeSlider.Tooltip>
        <sliders:SliderTooltip Fill="#DFD8F7"
                               Stroke="#512BD4"
                               StrokeThickness="2" />
    </sliders:SfRangeSlider.Tooltip>
</sliders:SfRangeSlider>
```

**C# Example:**
```csharp
rangeSlider.Tooltip = new SliderTooltip
{
    Fill = new SolidColorBrush(Color.FromArgb("#DFD8F7")),
    Stroke = new SolidColorBrush(Color.FromArgb("#512BD4")),
    StrokeThickness = 2
};
```

### Text Styling

Customize tooltip text appearance using font and color properties.

**Properties:**
- `TextColor` - Color of tooltip text
- `FontSize` - Size of tooltip text
- `FontFamily` - Font family for tooltip text
- `FontAttributes` - Font styling (Bold, Italic, etc.)

**Type:**
- `TextColor`: `Color`
- `FontSize`: `double`
- `FontFamily`: `string`
- `FontAttributes`: `FontAttributes`

**XAML Example:**
```xaml
<sliders:SfRangeSlider>
    <sliders:SfRangeSlider.Tooltip>
        <sliders:SliderTooltip TextColor="#512BD4"
                               FontSize="14"
                               FontFamily="Arial"
                               FontAttributes="Bold" />
    </sliders:SfRangeSlider.Tooltip>
</sliders:SfRangeSlider>
```

**C# Example:**
```csharp
rangeSlider.Tooltip = new SliderTooltip
{
    TextColor = Color.FromArgb("#512BD4"),
    FontSize = 14,
    FontFamily = "Arial",
    FontAttributes = FontAttributes.Bold
};
```

### Padding

Control internal spacing within the tooltip using the `Padding` property.

**Type:** `Thickness`  
**Default:** Platform-specific

**XAML Example:**
```xaml
<sliders:SfRangeSlider>
    <sliders:SfRangeSlider.Tooltip>
        <sliders:SliderTooltip Padding="12,12" />
    </sliders:SfRangeSlider.Tooltip>
</sliders:SfRangeSlider>
```

**C# Example:**
```csharp
rangeSlider.Tooltip = new SliderTooltip
{
    Padding = new Thickness(12, 12)
};
```

**Complete Styling Example:**
```xaml
<sliders:SfRangeSlider>
    <sliders:SfRangeSlider.Tooltip>
        <sliders:SliderTooltip Fill="#DFD8F7"
                               Stroke="#512BD4"
                               StrokeThickness="2"
                               TextColor="#512BD4"
                               FontSize="14"
                               FontAttributes="Bold"
                               FontFamily="Arial"
                               Padding="12,12"
                               ShowAlways="False" />
    </sliders:SfRangeSlider.Tooltip>
</sliders:SfRangeSlider>
```

```csharp
rangeSlider.Tooltip = new SliderTooltip
{
    Fill = new SolidColorBrush(Color.FromArgb("#DFD8F7")),
    Stroke = new SolidColorBrush(Color.FromArgb("#512BD4")),
    StrokeThickness = 2,
    TextColor = Color.FromArgb("#512BD4"),
    FontSize = 14,
    FontAttributes = FontAttributes.Bold,
    FontFamily = "Arial",
    Padding = new Thickness(12, 12),
    ShowAlways = false
};
```

## Tooltip Formatting

### NumberFormat

The `NumberFormat` property formats the numeric value displayed in the tooltip.

**Type:** `string`  
**Default:** Inherits from slider's `NumberFormat` or `"0.##"`

**Common Format Patterns:**
- `"0.##"` - Up to 2 decimal places
- `"0.00"` - Always 2 decimal places
- `"C0"` - Currency with no decimals
- `"P0"` - Percentage with no decimals
- `"N2"` - Number with 2 decimals and thousand separators

**XAML Example:**
```xaml
<sliders:SfRangeSlider>
    <sliders:SfRangeSlider.Tooltip>
        <sliders:SliderTooltip NumberFormat="C0" />
    </sliders:SfRangeSlider.Tooltip>
</sliders:SfRangeSlider>
```

**C# Example:**
```csharp
rangeSlider.Tooltip = new SliderTooltip
{
    NumberFormat = "C0"
};
```

### TooltipLabelCreated Event

The `TooltipLabelCreated` event provides full control over tooltip text and styling for each tooltip.

**Event Args:** `SliderTooltipLabelCreatedEventArgs`

**Properties:**
- `Text` - The tooltip text
- `TextColor` - Text color
- `FontSize` - Font size
- `FontFamily` - Font family
- `FontAttributes` - Font attributes

**XAML Example:**
```xaml
<sliders:SfRangeSlider>
    <sliders:SfRangeSlider.Tooltip>
        <sliders:SliderTooltip TooltipLabelCreated="OnTooltipLabelCreated" />
    </sliders:SfRangeSlider.Tooltip>
</sliders:SfRangeSlider>
```

**C# Event Handler:**
```csharp
private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    e.Text = "$" + e.Text;
}
```

**Complete Example:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    Minimum = 0,
    Maximum = 100,
    RangeStart = 20,
    RangeEnd = 80
};

rangeSlider.Tooltip = new SliderTooltip();
rangeSlider.Tooltip.TooltipLabelCreated += OnTooltipLabelCreated;

private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    // Add prefix
    e.Text = "$" + e.Text;
    
    // Customize per value
    if (double.TryParse(e.Text.Replace("$", ""), out double value))
    {
        if (value >= 50)
        {
            e.TextColor = Colors.Green;
            e.FontAttributes = FontAttributes.Bold;
        }
        else
        {
            e.TextColor = Colors.Red;
            e.FontAttributes = FontAttributes.Italic;
        }
    }
}
```

**Advanced Formatting:**
```csharp
private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    if (double.TryParse(e.Text, out double value))
    {
        // Convert to time format
        int hours = (int)value;
        int minutes = (int)((value - hours) * 60);
        e.Text = $"{hours:00}:{minutes:00}";
    }
}
```

## Common Scenarios

### Currency Tooltip
```xaml
<sliders:SfRangeSlider Minimum="0" Maximum="1000">
    <sliders:SfRangeSlider.Tooltip>
        <sliders:SliderTooltip NumberFormat="C0" />
    </sliders:SfRangeSlider.Tooltip>
</sliders:SfRangeSlider>
```

### Percentage Display
```xaml
<sliders:SfRangeSlider Minimum="0" Maximum="100">
    <sliders:SfRangeSlider.Tooltip>
        <sliders:SliderTooltip NumberFormat="0'%'" />
    </sliders:SfRangeSlider.Tooltip>
</sliders:SfRangeSlider>
```

### Always Visible Tooltips
```xaml
<sliders:SfRangeSlider>
    <sliders:SfRangeSlider.Tooltip>
        <sliders:SliderTooltip ShowAlways="True"
                               Fill="White"
                               Stroke="Black"
                               StrokeThickness="1" />
    </sliders:SfRangeSlider.Tooltip>
</sliders:SfRangeSlider>
```

### Themed Tooltip
```csharp
rangeSlider.Tooltip = new SliderTooltip
{
    Fill = new SolidColorBrush(Colors.Black),
    Stroke = new SolidColorBrush(Colors.Transparent),
    TextColor = Colors.White,
    FontSize = 12,
    FontAttributes = FontAttributes.Bold,
    Padding = new Thickness(8, 6)
};
```

### Custom Unit Display
```csharp
private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    e.Text = e.Text + " kg";
}
```

### Conditional Formatting
```csharp
private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    if (double.TryParse(e.Text, out double value))
    {
        if (value < 30)
        {
            e.Text = "Low: " + e.Text;
            e.TextColor = Colors.Red;
        }
        else if (value > 70)
        {
            e.Text = "High: " + e.Text;
            e.TextColor = Colors.Green;
        }
        else
        {
            e.Text = "Normal: " + e.Text;
            e.TextColor = Colors.Blue;
        }
    }
}
```

## Best Practices

1. **ShowAlways Usage**:
   - Use `ShowAlways="True"` for critical value display
   - Keep `ShowAlways="False"` for cleaner UI when space is limited
   - Consider device type (more practical on tablets than phones)

2. **Styling Guidelines**:
   - Ensure sufficient contrast between tooltip background and text
   - Use semi-transparent fills for overlapping tooltips
   - Match tooltip style with overall app theme

3. **Font Sizing**:
   - Mobile: 12-14px
   - Tablet: 14-16px
   - Ensure readability without excessive size

4. **Padding**:
   - Minimum 8-10px for comfortable reading
   - Increase padding for larger font sizes
   - Consider touch target accessibility

5. **NumberFormat vs Event**:
   - Use `NumberFormat` for simple formatting (currency, percentage)
   - Use `TooltipLabelCreated` event for complex logic or conditional formatting
   - Avoid heavy computation in event handler (affects performance)

6. **Color Choices**:
   - High contrast for readability
   - Consistent with slider theme
   - Test in both light and dark modes

7. **Performance**:
   - Minimize logic in `TooltipLabelCreated` event
   - Cache formatting resources if possible
   - Avoid creating objects in event handler

8. **Accessibility**:
   - Ensure text color has sufficient contrast (WCAG AA minimum)
   - Use clear, readable fonts
   - Consider users with color blindness

9. **User Experience**:
   - Tooltips should enhance, not obstruct
   - Avoid tooltip overlap when possible
   - Clear value communication without clutter

## Related References

- [labels.md](./labels.md) - Label formatting similar to tooltips
- [events-and-commands.md](./events-and-commands.md) - TooltipLabelCreated event details
- [thumbs-and-overlays.md](./thumbs-and-overlays.md) - Thumb interaction with tooltips
- [intervals-and-selection.md](./intervals-and-selection.md) - Value selection affecting tooltip display
