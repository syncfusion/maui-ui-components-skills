# Data Labels in .NET MAUI Funnel Chart

Data labels display values related to funnel chart segments, helping users quickly identify segment data without hovering. You can display values from data points (x, y) or other custom properties, with extensive customization options for placement, styling, and context.

## Table of Contents
- [Enable Data Labels](#enable-data-labels)
- [Data Label Customization](#data-label-customization)
- [Label Placement](#label-placement)
- [Label Context](#label-context)
- [UseSeriesPalette](#useseriespalette)
- [Label Style Properties](#label-style-properties)
- [Complete Examples](#complete-examples)

## Enable Data Labels

Set the `ShowDataLabels` property to `true` on the `SfFunnelChart` to display data labels. The default value is `false`.

### XAML
```xaml
<chart:SfFunnelChart ShowDataLabels="True"
                     ItemsSource="{Binding Data}"
                     XBindingPath="XValue"
                     YBindingPath="YValue">
    <!-- Chart configuration -->
</chart:SfFunnelChart>
```

### C#
```csharp
SfFunnelChart chart = new SfFunnelChart();
chart.ItemsSource = viewModel.Data;
chart.XBindingPath = "XValue";
chart.YBindingPath = "YValue";
chart.ShowDataLabels = true;

this.Content = chart;
```

## Data Label Customization

Customize data labels using the `DataLabelSettings` property with a `FunnelDataLabelSettings` instance. This provides control over placement, context, styling, and more.

### Basic Customization

```xaml
<chart:SfFunnelChart ShowDataLabels="True">
    <chart:SfFunnelChart.DataLabelSettings>
        <chart:FunnelDataLabelSettings LabelPlacement="Outer" 
                                       Context="XValue" 
                                       UseSeriesPalette="True"/>
    </chart:SfFunnelChart.DataLabelSettings>
</chart:SfFunnelChart>
```

```csharp
chart.ShowDataLabels = true;
chart.DataLabelSettings = new FunnelDataLabelSettings()
{
    LabelPlacement = DataLabelPlacement.Outer,
    Context = FunnelDataLabelContext.XValue,
    UseSeriesPalette = true
};
```

## Label Placement

The `LabelPlacement` property controls where data labels appear relative to funnel segments. Available options:

| Placement | Description |
|-----------|-------------|
| `Auto` | Automatically determines the best position (default) |
| `Inner` | Labels appear inside segments |
| `Center` | Labels appear at the center of segments |
| `Outer` | Labels appear outside segments |

### Auto Placement (Default)
```xaml
<chart:FunnelDataLabelSettings LabelPlacement="Auto"/>
```

### Inner Placement
```xaml
<chart:FunnelDataLabelSettings LabelPlacement="Inner"/>
```

```csharp
chart.DataLabelSettings = new FunnelDataLabelSettings()
{
    LabelPlacement = DataLabelPlacement.Inner
};
```

### Center Placement
```xaml
<chart:FunnelDataLabelSettings LabelPlacement="Center"/>
```

```csharp
chart.DataLabelSettings = new FunnelDataLabelSettings()
{
    LabelPlacement = DataLabelPlacement.Center
};
```

### Outer Placement
```xaml
<chart:FunnelDataLabelSettings LabelPlacement="Outer"/>
```

```csharp
chart.DataLabelSettings = new FunnelDataLabelSettings()
{
    LabelPlacement = DataLabelPlacement.Outer
};
```

## Label Context

The `Context` property determines what data to display in the label. Options include:

| Context | Description |
|---------|-------------|
| `YValue` | Display the Y-axis value (default) |
| `XValue` | Display the X-axis label/category |

### Display Y Values (Numeric Data)
```xaml
<chart:FunnelDataLabelSettings Context="YValue"/>
```

```csharp
chart.DataLabelSettings = new FunnelDataLabelSettings()
{
    Context = FunnelDataLabelContext.YValue
};
```

### Display X Values (Category Labels)
```xaml
<chart:FunnelDataLabelSettings Context="XValue"/>
```

```csharp
chart.DataLabelSettings = new FunnelDataLabelSettings()
{
    Context = FunnelDataLabelContext.XValue
};
```

## UseSeriesPalette

Set `UseSeriesPalette` to `true` to apply the segment's color to the data label background, creating visual consistency.

### XAML
```xaml
<chart:FunnelDataLabelSettings UseSeriesPalette="True"/>
```

### C#
```csharp
chart.DataLabelSettings = new FunnelDataLabelSettings()
{
    UseSeriesPalette = true
};
```

## Label Style Properties

The `LabelStyle` property provides fine-grained control over label appearance through `ChartDataLabelStyle`:

| Property | Type | Description |
|----------|------|-------------|
| `Margin` | Thickness | Spacing around the label |
| `Background` | Brush | Label background color |
| `FontAttributes` | FontAttributes | Font style (Bold, Italic, None) |
| `FontSize` | double | Font size |
| `Stroke` | Brush | Border color |
| `StrokeWidth` | double | Border thickness |
| `CornerRadius` | CornerRadius | Rounded corners |
| `TextColor` | Color | Text color |

### Basic Label Styling

```xaml
<chart:SfFunnelChart ShowDataLabels="True">
    <chart:SfFunnelChart.DataLabelSettings>
        <chart:FunnelDataLabelSettings LabelPlacement="Outer">
            <chart:FunnelDataLabelSettings.LabelStyle>
                <chart:ChartDataLabelStyle Margin="5"
                                          FontSize="14"
                                          TextColor="Black"
                                          FontAttributes="Bold"/>
            </chart:FunnelDataLabelSettings.LabelStyle>
        </chart:FunnelDataLabelSettings>
    </chart:SfFunnelChart.DataLabelSettings>
</chart:SfFunnelChart>
```

### Advanced Label Styling with Background and Border

```xaml
<chart:ChartDataLabelStyle Margin="8"
                          FontSize="16"
                          TextColor="White"
                          FontAttributes="Bold"
                          Background="#2196F3"
                          Stroke="#1976D2"
                          StrokeWidth="2"
                          CornerRadius="8"/>
```

```csharp
ChartDataLabelStyle labelStyle = new ChartDataLabelStyle()
{
    Margin = 8,
    FontSize = 16,
    TextColor = Colors.White,
    FontAttributes = FontAttributes.Bold,
    Background = new SolidColorBrush(Color.FromArgb("#2196F3")),
    Stroke = new SolidColorBrush(Color.FromArgb("#1976D2")),
    StrokeWidth = 2,
    CornerRadius = new CornerRadius(8)
};

chart.DataLabelSettings = new FunnelDataLabelSettings()
{
    LabelPlacement = DataLabelPlacement.Outer,
    LabelStyle = labelStyle
};
```

## Complete Examples

### Example 1: Outer Labels with Segment Colors

```xaml
<chart:SfFunnelChart ShowDataLabels="True"
                     ItemsSource="{Binding Data}"
                     XBindingPath="Stage"
                     YBindingPath="Count">
    
    <chart:SfFunnelChart.DataLabelSettings>
        <chart:FunnelDataLabelSettings LabelPlacement="Outer" 
                                       Context="YValue" 
                                       UseSeriesPalette="True">
            <chart:FunnelDataLabelSettings.LabelStyle>
                <chart:ChartDataLabelStyle Margin="5"
                                          FontSize="14"
                                          TextColor="White"
                                          FontAttributes="Bold"/>
            </chart:FunnelDataLabelSettings.LabelStyle>
        </chart:FunnelDataLabelSettings>
    </chart:SfFunnelChart.DataLabelSettings>
    
</chart:SfFunnelChart>
```

### Example 2: Inner Labels with Custom Background

```csharp
SfFunnelChart chart = new SfFunnelChart();
chart.ItemsSource = viewModel.Data;
chart.XBindingPath = "Stage";
chart.YBindingPath = "Count";
chart.ShowDataLabels = true;

ChartDataLabelStyle labelStyle = new ChartDataLabelStyle()
{
    Margin = 3,
    FontSize = 12,
    TextColor = Colors.White,
    Background = new SolidColorBrush(Colors.Black.WithAlpha(0.7f)),
    CornerRadius = new CornerRadius(4)
};

chart.DataLabelSettings = new FunnelDataLabelSettings()
{
    LabelPlacement = DataLabelPlacement.Inner,
    Context = FunnelDataLabelContext.YValue,
    LabelStyle = labelStyle
};

this.Content = chart;
```

### Example 3: Center Labels Showing Category Names

```xaml
<chart:SfFunnelChart ShowDataLabels="True">
    <chart:SfFunnelChart.DataLabelSettings>
        <chart:FunnelDataLabelSettings LabelPlacement="Center" 
                                       Context="XValue">
            <chart:FunnelDataLabelSettings.LabelStyle>
                <chart:ChartDataLabelStyle FontSize="16"
                                          TextColor="White"
                                          FontAttributes="Bold"/>
            </chart:FunnelDataLabelSettings.LabelStyle>
        </chart:FunnelDataLabelSettings>
    </chart:SfFunnelChart.DataLabelSettings>
</chart:SfFunnelChart>
```

### Example 4: Auto Placement with Bordered Labels

```csharp
chart.ShowDataLabels = true;

ChartDataLabelStyle labelStyle = new ChartDataLabelStyle()
{
    Margin = 6,
    FontSize = 15,
    TextColor = Colors.Black,
    FontAttributes = FontAttributes.Bold,
    Background = new SolidColorBrush(Colors.White),
    Stroke = new SolidColorBrush(Colors.Gray),
    StrokeWidth = 1,
    CornerRadius = new CornerRadius(6)
};

chart.DataLabelSettings = new FunnelDataLabelSettings()
{
    LabelPlacement = DataLabelPlacement.Auto,
    Context = FunnelDataLabelContext.YValue,
    LabelStyle = labelStyle
};
```

## Best Practices

1. **Choose Appropriate Placement:**
   - Use `Outer` for cleaner segments with external labels
   - Use `Inner` or `Center` when space is limited
   - Use `Auto` to let the chart decide optimal placement

2. **Context Selection:**
   - Display `YValue` when numeric data is important (counts, percentages)
   - Display `XValue` when category names need emphasis
   - Consider using tooltips for detailed information instead of cluttering labels

3. **Color Contrast:**
   - Ensure `TextColor` contrasts well with `Background` or segment colors
   - Use `UseSeriesPalette` with caution—verify text remains readable

4. **Font Sizing:**
   - Keep `FontSize` between 12-16 for readability
   - Adjust based on segment size and label count

5. **Avoid Overlap:**
   - If labels overlap with `Outer` placement, consider `Inner` or reduce `FontSize`
   - Use `Margin` to add spacing between labels and segments

## Troubleshooting

**Labels not visible:**
- Verify `ShowDataLabels="True"` is set on `SfFunnelChart`
- Check that `TextColor` contrasts with background
- Ensure `FontSize` is not too small

**Labels overlap:**
- Try different `LabelPlacement` options
- Reduce `FontSize` or label `Margin`
- Consider showing fewer segments or increasing chart height

**Label colors don't match segments:**
- Set `UseSeriesPalette="True"` to match segment colors
- Or manually set `Background` in `LabelStyle` to specific colors
