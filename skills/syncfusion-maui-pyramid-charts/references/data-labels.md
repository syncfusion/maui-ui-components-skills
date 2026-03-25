# Data Labels in Pyramid Charts

## Table of Contents
- [Overview](#overview)
- [Enabling Data Labels](#enabling-data-labels)
- [Data Label Settings](#data-label-settings)
- [Label Placement](#label-placement)
- [Label Context](#label-context)
- [Label Styling](#label-styling)
- [Using Series Palette](#using-series-palette)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)

## Overview

Data labels display values related to pyramid chart segments, making it easier for users to understand the data at a glance. You can display values from data points (X or Y values) or other custom properties from the data source.

**Key Features:**
- Enable/disable labels with a single property
- Multiple placement options (Auto, Inner, Center, Outer)
- Control what data is displayed (X values or Y values)
- Full styling control (fonts, colors, backgrounds, borders)
- Option to use segment colors for label backgrounds

## Enabling Data Labels

Set the `ShowDataLabels` property to `true` to display data labels on pyramid segments. The default value is `false`.

### XAML

```xaml
<chart:SfPyramidChart ItemsSource="{Binding Data}"
                      XBindingPath="Name"
                      YBindingPath="Value"
                      ShowDataLabels="True"/>
```

### C#

```csharp
SfPyramidChart chart = new SfPyramidChart();
chart.ItemsSource = viewModel.Data;
chart.XBindingPath = "Name";
chart.YBindingPath = "Value";
chart.ShowDataLabels = true;
this.Content = chart;
```

**Result:** Data labels will appear on each pyramid segment showing the Y values by default.

## Data Label Settings

Customize data labels using the `DataLabelSettings` property with a `PyramidDataLabelSettings` instance.

### Basic Configuration

**XAML:**
```xaml
<chart:SfPyramidChart ShowDataLabels="True">
    <chart:SfPyramidChart.DataLabelSettings>
        <chart:PyramidDataLabelSettings LabelPlacement="Outer"/>
    </chart:SfPyramidChart.DataLabelSettings>
</chart:SfPyramidChart>
```

**C#:**
```csharp
chart.ShowDataLabels = true;
chart.DataLabelSettings = new PyramidDataLabelSettings()
{
    LabelPlacement = DataLabelPlacement.Outer
};
```

## Label Placement

Control where labels appear relative to segments using the `LabelPlacement` property.

### Available Placement Options

| Placement | Description |
|-----------|-------------|
| **Auto** | System determines best placement based on available space |
| **Inner** | Labels placed inside segments near the edge |
| **Center** | Labels centered within segments |
| **Outer** | Labels placed outside segments with connector lines |

### Placement Examples

**Outer Placement (Recommended for Small Segments):**
```xaml
<chart:SfPyramidChart ShowDataLabels="True">
    <chart:SfPyramidChart.DataLabelSettings>
        <chart:PyramidDataLabelSettings LabelPlacement="Outer"/>
    </chart:SfPyramidChart.DataLabelSettings>
</chart:SfPyramidChart>
```

**Center Placement (Best for Large Segments):**
```xaml
<chart:SfPyramidChart ShowDataLabels="True">
    <chart:SfPyramidChart.DataLabelSettings>
        <chart:PyramidDataLabelSettings LabelPlacement="Center"/>
    </chart:SfPyramidChart.DataLabelSettings>
</chart:SfPyramidChart>
```

**Inner Placement:**
```xaml
<chart:SfPyramidChart ShowDataLabels="True">
    <chart:SfPyramidChart.DataLabelSettings>
        <chart:PyramidDataLabelSettings LabelPlacement="Inner"/>
    </chart:SfPyramidChart.DataLabelSettings>
</chart:SfPyramidChart>
```

**Auto Placement (Default):**
```xaml
<chart:SfPyramidChart ShowDataLabels="True">
    <chart:SfPyramidChart.DataLabelSettings>
        <chart:PyramidDataLabelSettings LabelPlacement="Auto"/>
    </chart:SfPyramidChart.DataLabelSettings>
</chart:SfPyramidChart>
```

## Label Context

The `Context` property determines which data value is displayed in the label.

### Available Context Options

| Context | Description | Example Output |
|---------|-------------|----------------|
| **YValue** | Display numeric Y value (default) | "37", "29", "21" |
| **XValue** | Display category X value | "Stage A", "Stage B" |

### Context Examples

**Show Y Values (Numeric Data):**
```xaml
<chart:SfPyramidChart ShowDataLabels="True">
    <chart:SfPyramidChart.DataLabelSettings>
        <chart:PyramidDataLabelSettings Context="YValue"/>
    </chart:SfPyramidChart.DataLabelSettings>
</chart:SfPyramidChart>
```

**Show X Values (Category Names):**
```xaml
<chart:SfPyramidChart ShowDataLabels="True">
    <chart:SfPyramidChart.DataLabelSettings>
        <chart:PyramidDataLabelSettings Context="XValue"/>
    </chart:SfPyramidChart.DataLabelSettings>
</chart:SfPyramidChart>
```

**C# Example:**
```csharp
chart.DataLabelSettings = new PyramidDataLabelSettings()
{
    Context = PyramidDataLabelContext.XValue
};
```

## Label Styling

Customize the appearance of data labels using the `LabelStyle` property with a `ChartDataLabelStyle` instance.

### Available Style Properties

| Property | Type | Description |
|----------|------|-------------|
| **TextColor** | Color | Text color of the label |
| **Background** | Brush | Background color of the label |
| **FontSize** | double | Font size of the label text |
| **FontFamily** | string | Font family for the label text |
| **FontAttributes** | FontAttributes | Font style (Bold, Italic, None) |
| **Margin** | Thickness | Space around the label |
| **Stroke** | Brush | Border color of the label |
| **StrokeWidth** | double | Border thickness |
| **CornerRadius** | CornerRadius | Rounded corners for label background |

### Basic Styling Example

**XAML:**
```xaml
<chart:SfPyramidChart ShowDataLabels="True">
    <chart:SfPyramidChart.DataLabelSettings>
        <chart:PyramidDataLabelSettings LabelPlacement="Outer">
            <chart:PyramidDataLabelSettings.LabelStyle>
                <chart:ChartDataLabelStyle TextColor="White"
                                          Background="DarkBlue"
                                          FontSize="14"
                                          FontAttributes="Bold"
                                          Margin="5"/>
            </chart:PyramidDataLabelSettings.LabelStyle>
        </chart:PyramidDataLabelSettings>
    </chart:SfPyramidChart.DataLabelSettings>
</chart:SfPyramidChart>
```

**C#:**
```csharp
ChartDataLabelStyle labelStyle = new ChartDataLabelStyle()
{
    TextColor = Colors.White,
    Background = Colors.DarkBlue,
    FontSize = 14,
    FontAttributes = FontAttributes.Bold,
    Margin = 5
};

chart.DataLabelSettings = new PyramidDataLabelSettings()
{
    LabelPlacement = DataLabelPlacement.Outer,
    LabelStyle = labelStyle
};
```

### Advanced Styling with Border and Corner Radius

```xaml
<chart:SfPyramidChart ShowDataLabels="True">
    <chart:SfPyramidChart.DataLabelSettings>
        <chart:PyramidDataLabelSettings LabelPlacement="Center">
            <chart:PyramidDataLabelSettings.LabelStyle>
                <chart:ChartDataLabelStyle TextColor="Black"
                                          Background="White"
                                          FontSize="16"
                                          FontAttributes="Bold"
                                          Margin="10"
                                          Stroke="DarkGray"
                                          StrokeWidth="2"
                                          CornerRadius="8"/>
            </chart:PyramidDataLabelSettings.LabelStyle>
        </chart:PyramidDataLabelSettings>
    </chart:SfPyramidChart.DataLabelSettings>
</chart:SfPyramidChart>
```

## Using Series Palette

Set `UseSeriesPalette` to `true` to automatically apply segment colors to label backgrounds.

### XAML Example

```xaml
<chart:SfPyramidChart ShowDataLabels="True">
    <chart:SfPyramidChart.DataLabelSettings>
        <chart:PyramidDataLabelSettings LabelPlacement="Outer" 
                                        Context="XValue" 
                                        UseSeriesPalette="True">
            <chart:PyramidDataLabelSettings.LabelStyle>
                <chart:ChartDataLabelStyle TextColor="White"
                                          FontSize="14"
                                          FontAttributes="Bold"
                                          Margin="5"/>
            </chart:PyramidDataLabelSettings.LabelStyle>
        </chart:PyramidDataLabelSettings>
    </chart:SfPyramidChart.DataLabelSettings>
</chart:SfPyramidChart>
```

### C# Example

```csharp
chart.DataLabelSettings = new PyramidDataLabelSettings()
{
    LabelPlacement = DataLabelPlacement.Outer,
    Context = PyramidDataLabelContext.XValue,
    UseSeriesPalette = true,
    LabelStyle = new ChartDataLabelStyle()
    {
        TextColor = Colors.White,
        FontSize = 14,
        FontAttributes = FontAttributes.Bold,
        Margin = 5
    }
};
```

**Result:** Each label background will match its corresponding segment color, creating a cohesive visual design.

## Complete Examples

### Example 1: Outer Labels with Category Names

```xaml
<chart:SfPyramidChart ItemsSource="{Binding Data}"
                      XBindingPath="Name"
                      YBindingPath="Value"
                      ShowDataLabels="True">
    
    <chart:SfPyramidChart.DataLabelSettings>
        <chart:PyramidDataLabelSettings LabelPlacement="Outer" 
                                        Context="XValue" 
                                        UseSeriesPalette="True">
            <chart:PyramidDataLabelSettings.LabelStyle>
                <chart:ChartDataLabelStyle TextColor="White"
                                          FontSize="12"
                                          Margin="3"/>
            </chart:PyramidDataLabelSettings.LabelStyle>
        </chart:PyramidDataLabelSettings>
    </chart:SfPyramidChart.DataLabelSettings>
    
</chart:SfPyramidChart>
```

### Example 2: Center Labels with Numeric Values

```csharp
SfPyramidChart chart = new SfPyramidChart();
chart.ItemsSource = viewModel.Data;
chart.XBindingPath = "Name";
chart.YBindingPath = "Value";
chart.ShowDataLabels = true;

chart.DataLabelSettings = new PyramidDataLabelSettings()
{
    LabelPlacement = DataLabelPlacement.Center,
    Context = PyramidDataLabelContext.YValue,
    LabelStyle = new ChartDataLabelStyle()
    {
        TextColor = Colors.White,
        FontSize = 16,
        FontAttributes = FontAttributes.Bold,
        Background = new SolidColorBrush(Colors.Black.WithAlpha(0.5f)),
        CornerRadius = 5,
        Margin = 8
    }
};
```

### Example 3: Custom Styled Labels with Border

```xaml
<chart:SfPyramidChart ShowDataLabels="True">
    <chart:SfPyramidChart.DataLabelSettings>
        <chart:PyramidDataLabelSettings LabelPlacement="Inner">
            <chart:PyramidDataLabelSettings.LabelStyle>
                <chart:ChartDataLabelStyle TextColor="DarkBlue"
                                          Background="LightYellow"
                                          FontSize="14"
                                          FontFamily="Arial"
                                          FontAttributes="Italic"
                                          Margin="6"
                                          Stroke="Orange"
                                          StrokeWidth="1.5"
                                          CornerRadius="10"/>
            </chart:PyramidDataLabelSettings.LabelStyle>
        </chart:PyramidDataLabelSettings>
    </chart:SfPyramidChart.DataLabelSettings>
</chart:SfPyramidChart>
```

## Best Practices

### Placement Selection

- **Use Outer placement** for small segments where text won't fit inside
- **Use Center placement** for large segments to emphasize data
- **Use Inner placement** for a clean look with medium-sized segments
- **Use Auto placement** when segment sizes vary significantly

### Context Selection

- **Use YValue** to emphasize numeric data and comparisons
- **Use XValue** when category names are more important than values
- Consider showing both by using custom templates if needed

### Styling Guidelines

- **Contrast:** Ensure text color contrasts well with background
- **Font Size:** Use 12-16px for readability; avoid tiny fonts (<10px)
- **UseSeriesPalette:** Enable for cohesive color design, but ensure text remains readable
- **Margins:** Add 3-5px margin for breathing room
- **Borders:** Use subtle borders to separate labels from segments

### Performance Considerations

- Data labels add minimal overhead
- Avoid excessive custom styling that requires complex rendering
- Test on target devices to ensure labels render clearly

## Common Issues and Solutions

### Issue: Labels Overlapping

**Solution:**
- Switch to Outer placement
- Reduce font size
- Enable Auto placement to let system optimize

### Issue: Text Not Visible on Dark Segments

**Solution:**
- Set `TextColor` to white or light color
- Add a semi-transparent background
- Enable `UseSeriesPalette` with light text color

### Issue: Labels Cut Off

**Solution:**
- Increase chart padding or margins
- Use Outer placement with more space
- Reduce font size or label margin

## Related Features

- **Tooltips**: See [tooltip.md](tooltip.md) for hover-based data display
- **Legend**: See [legend.md](legend.md) for segment identification
- **Styling**: See [appearance-customization.md](appearance-customization.md) for overall appearance