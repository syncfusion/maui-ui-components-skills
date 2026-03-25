# Labels and Formatting

This guide covers label display, interval configuration, number formatting, and custom label creation for the .NET MAUI Slider.

## Table of Contents
- [Show Labels](#show-labels)
- [Interval Configuration](#interval-configuration)
- [Number Format](#number-format)
- [Custom Labels with LabelCreated Event](#custom-labels-with-labelcreated-event)
- [Label Styling](#label-styling)
- [Complete Examples](#complete-examples)

## Show Labels

The `ShowLabels` property renders labels at specified intervals along the slider track. Default value is `False`.

### Enabling Labels

```xml
<sliders:SfSlider Minimum="2"
                  Maximum="10"
                  Value="6"
                  Interval="2"
                  ShowLabels="True"
                  ShowTicks="True" />
```

```csharp
SfSlider slider = new SfSlider
{
    Minimum = 2,
    Maximum = 10,
    Value = 6,
    Interval = 2,
    ShowLabels = true,
    ShowTicks = true
};
```

**Result**: Labels will appear at values 2, 4, 6, 8, 10.

### Labels Without Ticks

You can show labels without ticks:

```xml
<sliders:SfSlider Minimum="0"
                  Maximum="100"
                  Value="50"
                  Interval="20"
                  ShowLabels="True"
                  ShowTicks="False" />
```

This creates a cleaner look with only labels visible.

## Interval Configuration

The `Interval` property determines the spacing between labels, ticks, and dividers. It defines at what value intervals these elements appear.

### Manual Interval

Explicitly set the interval value:

```xml
<sliders:SfSlider Minimum="0"
                  Maximum="100"
                  Interval="25"
                  ShowLabels="True" />
```

```csharp
SfSlider slider = new SfSlider
{
    Minimum = 0,
    Maximum = 100,
    Interval = 25,
    ShowLabels = true
};
```

**Result**: Labels at 0, 25, 50, 75, 100.

### Calculation

The slider renders labels at:
- Minimum
- Minimum + Interval
- Minimum + (2 × Interval)
- ... up to Maximum

**Example with Minimum = 10, Maximum = 50, Interval = 10:**
- Labels appear at: 10, 20, 30, 40, 50

### Auto Interval

If `Interval` is set to `0` and `ShowLabels`, `ShowTicks`, or `ShowDividers` is `True`, the interval is automatically calculated based on available space:

```xml
<sliders:SfSlider Minimum="0"
                  Maximum="100"
                  Value="50"
                  ShowLabels="True"
                  ShowTicks="True" />
```

```csharp
SfSlider slider = new SfSlider
{
    Minimum = 0,
    Maximum = 100,
    Value = 50,
    ShowLabels = true,
    ShowTicks = true
    // Interval = 0 (default), auto-calculated
};
```

The slider automatically determines an appropriate interval (e.g., 10, 20, or 25) based on screen size.

**Use Cases for Auto Interval:**
- Responsive designs where slider size varies
- Unknown screen sizes
- Quick prototyping

### Interval Guidelines

**Small Ranges (0-10):**
- Interval = 1 or 2

**Medium Ranges (0-100):**
- Interval = 10, 20, or 25

**Large Ranges (0-1000+):**
- Interval = 100, 200, or 500

**Avoid:**
- Too many labels (cluttered): Interval = 1 on 0-1000 range
- Too few labels (unclear): Interval = 500 on 0-100 range

## Number Format

The `NumberFormat` property formats label text using standard .NET numeric format strings.

### Basic Formatting

```xml
<sliders:SfSlider Minimum="2"
                  Maximum="10"
                  Value="6"
                  Interval="2"
                  NumberFormat="0.00"
                  ShowLabels="True" />
```

```csharp
SfSlider slider = new SfSlider
{
    Minimum = 2,
    Maximum = 10,
    Value = 6,
    Interval = 2,
    NumberFormat = "0.00",
    ShowLabels = true
};
```

**Result**: Labels show as 2.00, 4.00, 6.00, 8.00, 10.00

### Currency Format

```xml
<sliders:SfSlider Minimum="0"
                  Maximum="1000"
                  Value="500"
                  Interval="200"
                  NumberFormat="$#"
                  ShowLabels="True" />
```

```csharp
slider.NumberFormat = "$#";
```

**Result**: Labels show as $0, $200, $400, $600, $800, $1000

### Percentage Format

```xml
<sliders:SfSlider Minimum="0"
                  Maximum="100"
                  Value="50"
                  Interval="25"
                  NumberFormat="0'%'"
                  ShowLabels="True" />
```

```csharp
slider.NumberFormat = "0'%'";
```

**Result**: Labels show as 0%, 25%, 50%, 75%, 100%

**Note**: Use single quotes around literal characters like '%' to display them as-is.

### Custom Unit Formats

**Temperature:**
```xml
<sliders:SfSlider NumberFormat="0'°C'" />
```
Result: 10°C, 15°C, 20°C, 25°C

**Weight:**
```xml
<sliders:SfSlider NumberFormat="0'kg'" />
```
Result: 0kg, 20kg, 40kg, 60kg

**Distance:**
```xml
<sliders:SfSlider NumberFormat="0.0'km'" />
```
Result: 0.0km, 5.5km, 11.0km

### Decimal Precision

```xml
<!-- No decimals -->
<sliders:SfSlider NumberFormat="0" />

<!-- One decimal -->
<sliders:SfSlider NumberFormat="0.0" />

<!-- Two decimals -->
<sliders:SfSlider NumberFormat="0.00" />

<!-- Optional decimals (removes trailing zeros) -->
<sliders:SfSlider NumberFormat="0.##" />
```

**Examples:**
- `"0"`: 5, 10, 15
- `"0.0"`: 5.0, 10.0, 15.0
- `"0.00"`: 5.00, 10.00, 15.00
- `"0.##"`: 5, 10.5, 15.25 (trailing zeros removed)

### Common Number Format Patterns

| Format | Example Output | Use Case |
|--------|----------------|----------|
| `"0"` | 10 | Whole numbers |
| `"0.0"` | 10.5 | One decimal |
| `"0.00"` | 10.50 | Currency (with $ prefix) |
| `"0.##"` | 10.5 | Optional decimals |
| `"$#"` | $100 | Currency |
| `"0'%'"` | 50% | Percentage |
| `"0'°C'"` | 22°C | Temperature |
| `"0'kg'"` | 75kg | Weight |
| `"#,##0"` | 1,000 | Thousands separator |

## Custom Labels with LabelCreated Event

The `LabelCreated` event allows full customization of label text and style. This event fires for each label before it's rendered.

### Event Overview

```csharp
slider.LabelCreated += OnLabelCreated;

private void OnLabelCreated(object sender, SliderLabelCreatedEventArgs e)
{
    // e.Text - Current label text
    // e.Style - Label style (color, font, offset, etc.)
}
```

### SliderLabelCreatedEventArgs Properties

- **Text** (string): The label text to display (read/write)
- **Style** (SliderLabelStyle): Label styling options

### Custom Label Text

**XAML:**
```xml
<sliders:SfSlider Minimum="2"
                  Maximum="10"
                  Value="6"
                  Interval="2"
                  LabelCreated="OnLabelCreated"
                  ShowTicks="True"
                  ShowLabels="True" />
```

**C#:**
```csharp
SfSlider slider = new SfSlider
{
    Minimum = 2,
    Maximum = 10,
    Value = 6,
    Interval = 2,
    ShowTicks = true,
    ShowLabels = true
};
slider.LabelCreated += OnLabelCreated;

private void OnLabelCreated(object sender, SliderLabelCreatedEventArgs e)
{
    e.Text = "$" + e.Text;
}
```

**Result**: Labels show as $2, $4, $6, $8, $10

### Advanced Label Customization

**Example 1: Custom Text Based on Value**

```csharp
private void OnLabelCreated(object sender, SliderLabelCreatedEventArgs e)
{
    // Parse the label value
    if (double.TryParse(e.Text, out double value))
    {
        if (value <= 30)
            e.Text = "Low: " + value;
        else if (value <= 70)
            e.Text = "Med: " + value;
        else
            e.Text = "High: " + value;
    }
}
```

**Example 2: Text Labels Instead of Numbers**

```csharp
private void OnLabelCreated(object sender, SliderLabelCreatedEventArgs e)
{
    if (double.TryParse(e.Text, out double value))
    {
        e.Text = value switch
        {
            0 => "Off",
            25 => "Low",
            50 => "Medium",
            75 => "High",
            100 => "Max",
            _ => e.Text
        };
    }
}
```

**Example 3: Time Format**

```csharp
private void OnLabelCreated(object sender, SliderLabelCreatedEventArgs e)
{
    if (double.TryParse(e.Text, out double minutes))
    {
        int hours = (int)(minutes / 60);
        int mins = (int)(minutes % 60);
        e.Text = $"{hours:D2}:{mins:D2}";
    }
}
```

## Label Styling

Customize label appearance using `SliderLabelStyle` in the `LabelCreated` event or directly via the `LabelStyle` property.

### LabelStyle Property

```xml
<sliders:SfSlider ShowLabels="True" Interval="20">
    <sliders:SfSlider.LabelStyle>
        <sliders:SliderLabelStyle TextColor="#FF6B6B"
                                  FontSize="14"
                                  FontFamily="Arial"
                                  FontAttributes="Bold" />
    </sliders:SfSlider.LabelStyle>
</sliders:SfSlider>
```

```csharp
slider.LabelStyle.TextColor = Color.FromArgb("#FF6B6B");
slider.LabelStyle.FontSize = 14;
slider.LabelStyle.FontFamily = "Arial";
slider.LabelStyle.FontAttributes = FontAttributes.Bold;
```

### Dynamic Styling in LabelCreated

```csharp
private void OnLabelCreated(object sender, SliderLabelCreatedEventArgs e)
{
    if (double.TryParse(e.Text, out double value))
    {
        // Color code based on value
        if (value < 30)
            e.Style.TextColor = Colors.Blue;
        else if (value < 70)
            e.Style.TextColor = Colors.Orange;
        else
            e.Style.TextColor = Colors.Red;
        
        // Adjust font size
        e.Style.FontSize = 12;
        e.Style.FontAttributes = FontAttributes.Bold;
    }
}
```

### Label Offset

Adjust label position relative to the track:

```csharp
slider.LabelStyle.Offset = new Point(0, 10);  // Move 10 pixels down
```

## Complete Examples

### Example 1: Price Slider with Currency

```xml
<VerticalStackLayout Padding="20" Spacing="10">
    <Label Text="Select Price Range" FontSize="16" />
    
    <sliders:SfSlider Minimum="0"
                      Maximum="1000"
                      Value="500"
                      Interval="200"
                      NumberFormat="$#,##0"
                      ShowLabels="True"
                      ShowDividers="True">
        <sliders:SfSlider.LabelStyle>
            <sliders:SliderLabelStyle TextColor="#9C27B0"
                                      FontSize="13"
                                      FontAttributes="Bold" />
        </sliders:SfSlider.LabelStyle>
    </sliders:SfSlider>
</VerticalStackLayout>
```

### Example 2: Temperature with Custom Labels

**XAML:**
```xml
<sliders:SfSlider Minimum="0"
                  Maximum="100"
                  Value="50"
                  Interval="25"
                  LabelCreated="OnTemperatureLabelCreated"
                  ShowLabels="True"
                  ShowTicks="True" />
```

**C#:**
```csharp
private void OnTemperatureLabelCreated(object sender, SliderLabelCreatedEventArgs e)
{
    if (double.TryParse(e.Text, out double celsius))
    {
        double fahrenheit = (celsius * 9 / 5) + 32;
        e.Text = $"{celsius}°C\n{fahrenheit:F0}°F";
        e.Style.FontSize = 10;
    }
}
```

### Example 3: Rating Slider

```xml
<sliders:SfSlider Minimum="1"
                  Maximum="5"
                  Value="3"
                  Interval="1"
                  StepSize="1"
                  LabelCreated="OnRatingLabelCreated"
                  ShowLabels="True" />
```

```csharp
private void OnRatingLabelCreated(object sender, SliderLabelCreatedEventArgs e)
{
    e.Text = e.Text switch
    {
        "1" => "⭐",
        "2" => "⭐⭐",
        "3" => "⭐⭐⭐",
        "4" => "⭐⭐⭐⭐",
        "5" => "⭐⭐⭐⭐⭐",
        _ => e.Text
    };
    e.Style.FontSize = 16;
}
```

## Best Practices

### Interval Selection
- Balance between too many (cluttered) and too few (vague) labels
- Consider screen size and slider dimensions
- Use auto interval (0) for responsive designs

### Number Formatting
- Use `"0.##"` for optional decimals (cleaner)
- Use `"0.00"` for fixed decimals (currency, precision)
- Always quote literal characters: `'%'`, `'°C'`, `'kg'`

### Custom Labels
- Keep label text short (2-8 characters)
- Use consistent formatting across all labels
- Avoid emoji on platforms that don't support them well

### Performance
- Avoid complex calculations in `LabelCreated` event
- Cache computed values if the same label appears multiple times
- Use NumberFormat when possible instead of LabelCreated

## Troubleshooting

### Issue: Labels Not Showing

**Cause**: ShowLabels is False or Interval is not set  
**Solution**:
```xml
<sliders:SfSlider ShowLabels="True" Interval="10" />
```

### Issue: Label Format Not Applied

**Cause**: NumberFormat syntax error  
**Solution**: Use valid .NET numeric format strings. Quote literals:
```xml
NumberFormat="0'%'"  <!-- Correct -->
NumberFormat="0%"    <!-- Wrong -->
```

### Issue: LabelCreated Event Not Firing

**Cause**: Event not wired up or ShowLabels is False  
**Solution**:
```xml
<sliders:SfSlider ShowLabels="True" LabelCreated="OnLabelCreated" />
```

## Summary

Key label configuration properties:

- **ShowLabels**: Enable/disable label display
- **Interval**: Spacing between labels (0 for auto)
- **NumberFormat**: Format label text (currency, percentage, decimals, units)
- **LabelCreated**: Event for custom label text and styling
- **LabelStyle**: Global label appearance (color, font, size, offset)

Use NumberFormat for simple formatting, and LabelCreated event for advanced customization.
