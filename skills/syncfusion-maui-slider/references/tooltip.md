# Tooltip

This guide covers tooltip configuration and customization for the .NET MAUI Slider, including enabling tooltips, formatting, and custom tooltip text.

## Table of Contents
- [Enable Tooltip](#enable-tooltip)
- [Show Always](#show-always)
- [Tooltip Number Format](#tooltip-number-format)
- [Custom Tooltip with TooltipLabelCreated Event](#custom-tooltip-with-tooltiplabelcreated-event)
- [Tooltip Styling](#tooltip-styling)
- [Complete Examples](#complete-examples)

## Enable Tooltip

The tooltip displays the current slider value when the user interacts with the thumb. It provides immediate visual feedback.

### Basic Tooltip Setup

Create a `SliderTooltip` instance and assign it to the `Tooltip` property:

**XAML:**
```xml
<sliders:SfSlider>
    <sliders:SfSlider.Tooltip>
        <sliders:SliderTooltip />
    </sliders:SfSlider.Tooltip>
</sliders:SfSlider>
```

**C#:**
```csharp
SfSlider slider = new SfSlider
{
    Tooltip = new SliderTooltip()
};
```

**Behavior**: The tooltip appears when the user presses/drags the thumb, and disappears when released.

### Default Tooltip Format

By default, the tooltip displays the slider value formatted according to the slider's `NumberFormat` property (or "0.##" if not specified).

```xml
<sliders:SfSlider Minimum="0"
                  Maximum="100"
                  Value="50"
                  NumberFormat="0">
    <sliders:SfSlider.Tooltip>
        <sliders:SliderTooltip />
    </sliders:SfSlider.Tooltip>
</sliders:SfSlider>
```

Tooltip will display: 50

## Show Always

The `ShowAlways` property controls whether the tooltip is always visible or only during interaction.

### Always Visible Tooltip

```xml
<sliders:SfSlider>
    <sliders:SfSlider.Tooltip>
        <sliders:SliderTooltip ShowAlways="True" />
    </sliders:SfSlider.Tooltip>
</sliders:SfSlider>
```

```csharp
SfSlider slider = new SfSlider
{
    Tooltip = new SliderTooltip
    {
        ShowAlways = true
    }
};
```

**Behavior**: The tooltip remains visible at all times, not just during interaction.

### Interactive Tooltip (Default)

```xml
<sliders:SfSlider>
    <sliders:SfSlider.Tooltip>
        <sliders:SliderTooltip ShowAlways="False" />
    </sliders:SfSlider.Tooltip>
</sliders:SfSlider>
```

```csharp
SfSlider slider = new SfSlider
{
    Tooltip = new SliderTooltip
    {
        ShowAlways = false  // Default behavior
    }
};
```

**Behavior**: The tooltip appears only when the thumb is pressed or dragged.

### Use Cases

- **ShowAlways="True"**: Dashboard displays, always-visible value indicators
- **ShowAlways="False"**: Standard sliders where tooltip is only needed during adjustment

## Tooltip Number Format

The `NumberFormat` property on `SliderTooltip` allows independent formatting of tooltip text, separate from slider labels.

### Basic Number Formatting

```xml
<sliders:SfSlider Minimum="0"
                  Maximum="100"
                  Value="50">
    <sliders:SfSlider.Tooltip>
        <sliders:SliderTooltip NumberFormat="0.00" />
    </sliders:SfSlider.Tooltip>
</sliders:SfSlider>
```

```csharp
slider.Tooltip = new SliderTooltip
{
    NumberFormat = "0.00"
};
```

**Result**: Tooltip displays "50.00"

### Currency Format

```xml
<sliders:SfSlider.Tooltip>
    <sliders:SliderTooltip NumberFormat="$#,##0.00" />
</sliders:SfSlider.Tooltip>
```

**Result**: Tooltip displays "$1,250.00" for value 1250

### Percentage Format

```xml
<sliders:SfSlider.Tooltip>
    <sliders:SliderTooltip NumberFormat="0'%'" />
</sliders:SfSlider.Tooltip>
```

**Result**: Tooltip displays "75%" for value 75

### Custom Unit Formats

**Temperature:**
```xml
<sliders:SliderTooltip NumberFormat="0.0'°C'" />
```
Result: "22.5°C"

**Weight:**
```xml
<sliders:SliderTooltip NumberFormat="0.0'kg'" />
```
Result: "75.5kg"

**Distance:**
```xml
<sliders:SliderTooltip NumberFormat="0.##'km'" />
```
Result: "12.5km"

### Independent Formatting

Slider labels and tooltip can have different formats:

```xml
<sliders:SfSlider Minimum="0"
                  Maximum="100"
                  Value="50"
                  Interval="25"
                  NumberFormat="0"
                  ShowLabels="True">
    <sliders:SfSlider.Tooltip>
        <sliders:SliderTooltip NumberFormat="0.00'%'" />
    </sliders:SfSlider.Tooltip>
</sliders:SfSlider>
```

**Result**: 
- Labels show: 0, 25, 50, 75, 100
- Tooltip shows: 50.00%

## Custom Tooltip with TooltipLabelCreated Event

The `TooltipLabelCreated` event provides full control over tooltip text and styling.

### Event Setup

**XAML:**
```xml
<sliders:SfSlider>
    <sliders:SfSlider.Tooltip>
        <sliders:SliderTooltip TooltipLabelCreated="OnTooltipLabelCreated" />
    </sliders:SfSlider.Tooltip>
</sliders:SfSlider>
```

**C#:**
```csharp
SfSlider slider = new SfSlider
{
    Tooltip = new SliderTooltip()
};
((SliderTooltip)slider.Tooltip).TooltipLabelCreated += OnTooltipLabelCreated;

private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    e.Text = "$" + e.Text;
}
```

### SliderTooltipLabelCreatedEventArgs Properties

- **Text** (string): The tooltip text to display (read/write)
- **TextColor** (Color): Tooltip text color
- **FontSize** (double): Font size of tooltip text
- **FontFamily** (string): Font family of tooltip text
- **FontAttributes** (FontAttributes): Font attributes (Bold, Italic, None)

### Custom Text Examples

**Example 1: Add Prefix/Suffix**

```csharp
private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    e.Text = "$" + e.Text;
}
```

**Example 2: Conditional Text**

```csharp
private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    if (double.TryParse(e.Text, out double value))
    {
        if (value < 33)
            e.Text = "Low: " + value;
        else if (value < 67)
            e.Text = "Medium: " + value;
        else
            e.Text = "High: " + value;
    }
}
```

**Example 3: Text Labels**

```csharp
private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    if (double.TryParse(e.Text, out double value))
    {
        e.Text = value switch
        {
            0 => "Off",
            25 => "Low",
            50 => "Medium",
            75 => "High",
            100 => "Maximum",
            _ => value.ToString("0")
        };
    }
}
```

**Example 4: Time Format**

```csharp
private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    if (double.TryParse(e.Text, out double seconds))
    {
        TimeSpan time = TimeSpan.FromSeconds(seconds);
        e.Text = time.ToString(@"mm\:ss");
    }
}
```

## Tooltip Styling

Customize tooltip appearance using the `TooltipLabelCreated` event.

### Text Color

```csharp
private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    e.TextColor = Color.FromArgb("#FF6B6B");
}
```

### Font Size

```csharp
private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    e.FontSize = 16;
}
```

### Font Family

```csharp
private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    e.FontFamily = "Arial";
}
```

### Font Attributes

```csharp
private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    e.FontAttributes = FontAttributes.Bold;
    // Or: FontAttributes.Italic
    // Or: FontAttributes.Bold | FontAttributes.Italic
}
```

### Complete Styling Example

```csharp
private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    // Custom text
    e.Text = "$" + e.Text;
    
    // Custom styling
    e.TextColor = Colors.White;
    e.FontSize = 14;
    e.FontFamily = "Arial";
    e.FontAttributes = FontAttributes.Bold;
}
```

### Conditional Styling

```csharp
private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    if (double.TryParse(e.Text, out double value))
    {
        // Color code based on value
        if (value < 30)
        {
            e.TextColor = Colors.Blue;
            e.Text = "Cold: " + value + "°C";
        }
        else if (value < 60)
        {
            e.TextColor = Colors.Orange;
            e.Text = "Warm: " + value + "°C";
        }
        else
        {
            e.TextColor = Colors.Red;
            e.Text = "Hot: " + value + "°C";
        }
        
        e.FontSize = 14;
        e.FontAttributes = FontAttributes.Bold;
    }
}
```

## Complete Examples

### Example 1: Always-Visible Tooltip with Currency

```xml
<VerticalStackLayout Padding="20" Spacing="10">
    <Label Text="Price Selector" FontSize="16" />
    
    <sliders:SfSlider Minimum="0"
                      Maximum="1000"
                      Value="500"
                      Interval="200"
                      ShowLabels="True">
        <sliders:SfSlider.Tooltip>
            <sliders:SliderTooltip ShowAlways="True"
                                   NumberFormat="$#,##0" />
        </sliders:SfSlider.Tooltip>
    </sliders:SfSlider>
</VerticalStackLayout>
```

### Example 2: Temperature Slider with Custom Tooltip

**XAML:**
```xml
<sliders:SfSlider Minimum="0"
                  Maximum="100"
                  Value="22"
                  Interval="20"
                  ShowLabels="True">
    <sliders:SfSlider.Tooltip>
        <sliders:SliderTooltip TooltipLabelCreated="OnTemperatureTooltipCreated" />
    </sliders:SfSlider.Tooltip>
</sliders:SfSlider>
```

**C#:**
```csharp
private void OnTemperatureTooltipCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    if (double.TryParse(e.Text, out double celsius))
    {
        double fahrenheit = (celsius * 9 / 5) + 32;
        e.Text = $"{celsius:F1}°C / {fahrenheit:F1}°F";
        e.FontSize = 12;
    }
}
```

### Example 3: Volume Slider with Percentage

```xml
<sliders:SfSlider Minimum="0"
                  Maximum="100"
                  Value="75"
                  Interval="25"
                  NumberFormat="0"
                  ShowLabels="True"
                  ShowTicks="True">
    <sliders:SfSlider.Tooltip>
        <sliders:SliderTooltip NumberFormat="0'%'" />
    </sliders:SfSlider.Tooltip>
</sliders:SfSlider>
```

### Example 4: Time Slider (Media Player)

**XAML:**
```xml
<sliders:SfSlider Minimum="0"
                  Maximum="300"
                  Value="120"
                  Interval="60"
                  ShowLabels="True">
    <sliders:SfSlider.Tooltip>
        <sliders:SliderTooltip TooltipLabelCreated="OnTimeTooltipCreated" />
    </sliders:SfSlider.Tooltip>
</sliders:SfSlider>
```

**C#:**
```csharp
private void OnTimeTooltipCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    if (double.TryParse(e.Text, out double seconds))
    {
        TimeSpan time = TimeSpan.FromSeconds(seconds);
        e.Text = time.ToString(@"mm\:ss");
        e.FontFamily = "Courier New";
        e.FontSize = 14;
    }
}
```

## Best Practices

### When to Use ShowAlways
- **True**: Dashboard widgets, constant value display needed
- **False**: Standard UI where tooltip during interaction is sufficient

### Number Formatting
- Use `NumberFormat` for simple formatting (currency, percentage, units)
- Use `TooltipLabelCreated` for complex text transformations
- Keep tooltip text concise (< 20 characters for best UX)

### Styling Guidelines
- Ensure tooltip text has good contrast with tooltip background
- Use larger font sizes (12-16) for readability
- Bold text improves readability in tooltips
- Avoid very long tooltip text (splits or truncates)

### Performance
- Avoid heavy calculations in `TooltipLabelCreated` (fires frequently during drag)
- Cache computed values if possible
- Use `NumberFormat` instead of `TooltipLabelCreated` when simple formatting suffices

## Troubleshooting

### Issue: Tooltip Not Visible

**Cause**: Tooltip not instantiated  
**Solution**: Create and assign SliderTooltip:
```xml
<sliders:SfSlider.Tooltip>
    <sliders:SliderTooltip />
</sliders:SfSlider.Tooltip>
```

### Issue: Tooltip Format Not Applied

**Cause**: NumberFormat syntax error or incorrect format string  
**Solution**: Use valid .NET numeric format strings. Quote literals:
```xml
<sliders:SliderTooltip NumberFormat="0'%'" />  <!-- Correct -->
<sliders:SliderTooltip NumberFormat="0%" />    <!-- Wrong -->
```

### Issue: TooltipLabelCreated Event Not Firing

**Cause**: Event not wired correctly or Tooltip is null  
**Solution**: Ensure tooltip exists and event is attached:
```xml
<sliders:SfSlider.Tooltip>
    <sliders:SliderTooltip TooltipLabelCreated="OnTooltipLabelCreated" />
</sliders:SfSlider.Tooltip>
```

### Issue: Tooltip Always Showing (Unwanted)

**Cause**: ShowAlways is True  
**Solution**: Set ShowAlways to False:
```xml
<sliders:SliderTooltip ShowAlways="False" />
```

## Summary

Key tooltip properties and features:

- **Tooltip**: Assign a `SliderTooltip` instance to enable tooltip
- **ShowAlways**: Control whether tooltip is always visible or only during interaction
- **NumberFormat**: Format tooltip text (currency, percentage, decimals, units)
- **TooltipLabelCreated**: Event for custom tooltip text and styling
- **SliderTooltipLabelCreatedEventArgs**: Provides Text, TextColor, FontSize, FontFamily, FontAttributes

Use `NumberFormat` for simple formatting, and `TooltipLabelCreated` for advanced customization.
