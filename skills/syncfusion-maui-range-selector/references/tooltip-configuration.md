# Tooltip Configuration

This guide covers how to configure and customize tooltips in the .NET MAUI Range Selector (SfRangeSelector).

## Table of Contents
- [Overview](#overview)
- [Enabling Tooltip](#enabling-tooltip)
- [Show Always](#show-always)
- [Tooltip Styling](#tooltip-styling)
- [Custom Tooltip Text](#custom-tooltip-text)
- [Complete Examples](#complete-examples)

## Overview

Tooltips display the current value when users interact with thumbs. They provide clear visual feedback about the selected values during dragging.

## Enabling Tooltip

Enable tooltips by setting the `Tooltip` property to a `SliderTooltip` instance.

**Property:**
- `Tooltip` (SliderTooltip): Tooltip configuration. Default: `null` (disabled)

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="100"
                         RangeStart="25" RangeEnd="75">
    <sliders:SfRangeSelector.Tooltip>
        <sliders:SliderTooltip />
    </sliders:SfRangeSelector.Tooltip>
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
SfRangeSelector rangeSelector = new SfRangeSelector
{
    Minimum = 0,
    Maximum = 100,
    RangeStart = 25,
    RangeEnd = 75,
    Tooltip = new SliderTooltip()
};
```

**Default Behavior:**
- Appears when dragging thumbs
- Hides when drag ends
- Shows formatted value using `NumberFormat`

## Show Always

Display tooltips persistently using the `ShowAlways` property.

**Property:**
- `ShowAlways` (bool): Keep tooltips visible. Default: `false`

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="100"
                         RangeStart="25" RangeEnd="75">
    <sliders:SfRangeSelector.Tooltip>
        <sliders:SliderTooltip ShowAlways="True" />
    </sliders:SfRangeSelector.Tooltip>
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
rangeSelector.Tooltip = new SliderTooltip
{
    ShowAlways = true
};
```

**Use Cases:**
- Always display current range values
- Dashboard widgets
- Prominent value indication

## Tooltip Styling

Customize tooltip appearance using `SliderTooltip` properties.

**Properties:**
- `Fill` (Brush): Background color
- `Stroke` (Brush): Border color
- `StrokeThickness` (double): Border width. Default: `0`
- `Position` (SliderTooltipPosition): Position relative to thumb
- `TextColor` (Color): Text color
- `FontSize` (double): Text size
- `FontAttributes` (FontAttributes): Bold, Italic, or None
- `FontFamily` (string): Font family name
- `Padding` (Thickness): Internal padding
- `NumberFormat` (string): Value format pattern

### Basic Styling

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="100"
                         RangeStart="25" RangeEnd="75">
    <sliders:SfRangeSelector.Tooltip>
        <sliders:SliderTooltip Fill="#DFD8F7"
                               Stroke="#512BD4"
                               StrokeThickness="2"
                               TextColor="#512BD4"
                               FontSize="14"
                               FontAttributes="Bold"
                               Padding="12,12" />
    </sliders:SfRangeSelector.Tooltip>
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
rangeSelector.Tooltip = new SliderTooltip
{
    Fill = new SolidColorBrush(Color.FromArgb("#DFD8F7")),
    Stroke = new SolidColorBrush(Color.FromArgb("#512BD4")),
    StrokeThickness = 2,
    TextColor = Color.FromArgb("#512BD4"),
    FontSize = 14,
    FontAttributes = FontAttributes.Bold,
    Padding = new Thickness(12, 12)
};
```

### Number Format in Tooltip

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="1000"
                         RangeStart="200" RangeEnd="800">
    <sliders:SfRangeSelector.Tooltip>
        <sliders:SliderTooltip NumberFormat="$#,0"
                               FontSize="16" />
    </sliders:SfRangeSelector.Tooltip>
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**Common Formats:**
- `"$#,0"` → $1,000
- `"#%"` → 50%
- `"0.00"` → 50.00
- `"€ #"` → € 50

## Custom Tooltip Text

Customize tooltip text dynamically using the `TooltipLabelCreated` event.

**Event:**
- `TooltipLabelCreated`: Fires when tooltip text is created

**Event Args (SliderTooltipLabelCreatedEventArgs):**
- `Text` (string): Get/set tooltip text
- `TextColor` (Color): Get/set text color
- `FontSize` (double): Get/set font size
- `FontFamily` (string): Get/set font family
- `FontAttributes` (FontAttributes): Get/set font attributes

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="2" Maximum="10"
                         RangeStart="4" RangeEnd="8"
                         Interval="2">
    <sliders:SfRangeSelector.Tooltip>
        <sliders:SliderTooltip TooltipLabelCreated="OnTooltipLabelCreated" />
    </sliders:SfRangeSelector.Tooltip>
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C# Event Handler:**
```csharp
private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    // Add currency prefix
    e.Text = "$" + e.Text;
}
```

**Advanced Customization:**
```csharp
private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    if (double.TryParse(e.Text, out double value))
    {
        if (value < 25)
        {
            e.Text = "Low: " + value;
            e.TextColor = Colors.Red;
        }
        else if (value > 75)
        {
            e.Text = "High: " + value;
            e.TextColor = Colors.Green;
        }
        else
        {
            e.Text = "Mid: " + value;
            e.TextColor = Colors.Blue;
        }
    }
}
```

**Unit Conversion:**
```csharp
private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    if (double.TryParse(e.Text, out double celsius))
    {
        double fahrenheit = (celsius * 9 / 5) + 32;
        e.Text = $"{celsius}°C / {fahrenheit:F1}°F";
    }
}
```

## Complete Examples

### Styled Tooltip with Currency
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="10000"
                         RangeStart="2000" RangeEnd="8000"
                         Interval="2000"
                         ShowLabels="True"
                         ShowTicks="True">
    <sliders:SfRangeSelector.Tooltip>
        <sliders:SliderTooltip NumberFormat="$#,0"
                               Fill="#2196F3"
                               TextColor="White"
                               FontSize="16"
                               FontAttributes="Bold"
                               StrokeThickness="0"
                               Padding="16,12"
                               ShowAlways="False" />
    </sliders:SfRangeSelector.Tooltip>
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

### Always-Visible Tooltip
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="100"
                         RangeStart="40" RangeEnd="60">
    <sliders:SfRangeSelector.Tooltip>
        <sliders:SliderTooltip ShowAlways="True"
                               NumberFormat="#%"
                               Fill="#4CAF50"
                               TextColor="White"
                               FontSize="14"
                               Padding="10,8" />
    </sliders:SfRangeSelector.Tooltip>
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

### Custom Tooltip with Event
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="24"
                         RangeStart="8" RangeEnd="18"
                         Interval="4">
    <sliders:SfRangeSelector.Tooltip>
        <sliders:SliderTooltip TooltipLabelCreated="OnTooltipLabelCreated"
                               Fill="#FF5722"
                               TextColor="White"
                               FontSize="14"
                               Padding="12,10" />
    </sliders:SfRangeSelector.Tooltip>
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

```csharp
private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    if (double.TryParse(e.Text, out double hour))
    {
        string period = hour < 12 ? "AM" : "PM";
        int displayHour = (int)hour % 12;
        if (displayHour == 0) displayHour = 12;
        e.Text = $"{displayHour}:00 {period}";
    }
}
// Result: "8:00 AM", "12:00 PM", "6:00 PM"
```
