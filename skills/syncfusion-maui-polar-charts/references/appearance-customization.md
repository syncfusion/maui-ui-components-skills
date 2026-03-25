# Appearance Customization

## Table of Contents
- [Overview](#overview)
- [Default Palette](#default-palette)
- [Custom Palette Brushes](#custom-palette-brushes)
- [Gradient Brushes](#gradient-brushes)
- [Series-Specific Styling](#series-specific-styling)
- [Plotting Area Customization](#plotting-area-customization)
- [Liquid Glass Effect](#liquid-glass-effect)
- [Common Patterns](#common-patterns)

## Overview

Customize polar chart appearance using:
- **Palette Brushes** - Predefined or custom color schemes
- **Gradient Brushes** - Linear or radial gradients
- **Series Properties** - Individual series styling
- **Plot Area Background** - Custom backgrounds, watermarks
- **Liquid Glass Effect** - Modern glassy appearance (iOS/macOS)

## Default Palette

Syncfusion applies default brushes to series automatically:

```xml
<chart:SfPolarChart>
    <!-- Series automatically get colors from default palette -->
    <chart:PolarLineSeries ItemsSource="{Binding Data1}" 
                           XBindingPath="Category" 
                           YBindingPath="Value"/>
    <chart:PolarLineSeries ItemsSource="{Binding Data2}" 
                           XBindingPath="Category" 
                           YBindingPath="Value"/>
    <chart:PolarLineSeries ItemsSource="{Binding Data3}" 
                           XBindingPath="Category" 
                           YBindingPath="Value"/>
</chart:SfPolarChart>
```

## Custom Palette Brushes

### Chart-Level Palette

Apply custom colors to all series:

```csharp
SfPolarChart chart = new SfPolarChart();

List<Brush> customBrushes = new List<Brush>
{
    new SolidColorBrush(Color.FromArgb("#25E739")),
    new SolidColorBrush(Color.FromArgb("#F4890B")),
    new SolidColorBrush(Color.FromArgb("#E2227E"))
};

chart.PaletteBrushes = customBrushes;
```

**XAML:**
```xml
<chart:SfPolarChart x:Name="chart" PaletteBrushes="{Binding CustomBrushes}">
    <!-- Series -->
</chart:SfPolarChart>
```

```csharp
// In ViewModel
public List<Brush> CustomBrushes { get; set; } = new List<Brush>
{
    new SolidColorBrush(Color.FromArgb("#25E739")),
    new SolidColorBrush(Color.FromArgb("#F4890B")),
    new SolidColorBrush(Color.FromArgb("#E2227E"))
};
```

## Gradient Brushes

### Linear Gradient

```csharp
LinearGradientBrush gradientBrush = new LinearGradientBrush();
gradientBrush.GradientStops = new GradientStopCollection
{
    new GradientStop { Offset = 0, Color = Color.FromRgb(123, 176, 249) },
    new GradientStop { Offset = 1, Color = Color.FromRgb(168, 234, 238) }
};

// Apply to chart palette
chart.PaletteBrushes = new List<Brush> { gradientBrush };
```

### Multiple Gradient Series

```csharp
List<Brush> gradients = new List<Brush>();

// Gradient 1
LinearGradientBrush gradient1 = new LinearGradientBrush();
gradient1.GradientStops = new GradientStopCollection
{
    new GradientStop { Offset = 1, Color = Color.FromRgb(168, 234, 238) },
    new GradientStop { Offset = 0, Color = Color.FromRgb(123, 176, 249) }
};
gradients.Add(gradient1);

// Gradient 2
LinearGradientBrush gradient2 = new LinearGradientBrush();
gradient2.GradientStops = new GradientStopCollection
{
    new GradientStop { Offset = 1, Color = Color.FromRgb(221, 214, 243) },
    new GradientStop { Offset = 0, Color = Color.FromRgb(250, 172, 168) }
};
gradients.Add(gradient2);

// Gradient 3
LinearGradientBrush gradient3 = new LinearGradientBrush();
gradient3.GradientStops = new GradientStopCollection
{
    new GradientStop { Offset = 1, Color = Color.FromRgb(255, 231, 199) },
    new GradientStop { Offset = 0, Color = Color.FromRgb(252, 182, 159) }
};
gradients.Add(gradient3);

chart.PaletteBrushes = gradients;
```

## Series-Specific Styling

### PolarLineSeries Styling

```csharp
PolarLineSeries series = new PolarLineSeries
{
    ItemsSource = data,
    XBindingPath = "Category",
    YBindingPath = "Value",
    Stroke = new SolidColorBrush(Colors.Blue),  // Line color
    StrokeWidth = 3,                             // Line thickness
    StrokeDashArray = new double[] { 5, 2 },    // Dashed line
    Opacity = 0.8                                // Transparency
};
```

### PolarAreaSeries Styling

```csharp
PolarAreaSeries series = new PolarAreaSeries
{
    ItemsSource = data,
    XBindingPath = "Category",
    YBindingPath = "Value",
    Fill = new SolidColorBrush(Colors.LightBlue),  // Fill color
    Opacity = 0.6,                                   // Transparency
    Stroke = new SolidColorBrush(Colors.Blue),      // Border color
    StrokeWidth = 2                                  // Border thickness
};
```

### Individual Series Colors

Override palette for specific series:

```csharp
// Series 1 - Custom blue
series1.Stroke = new SolidColorBrush(Color.FromArgb("#2196F3"));

// Series 2 - Custom red
series2.Stroke = new SolidColorBrush(Color.FromArgb("#F44336"));

// Series 3 - Custom green
series3.Stroke = new SolidColorBrush(Color.FromArgb("#4CAF50"));
```

## Plotting Area Customization

Add custom views to the chart plot area:

### Watermark

```xml
<chart:SfPolarChart>
    <chart:SfPolarChart.PlotAreaBackgroundView>
        <AbsoluteLayout>
            <Label Text="CONFIDENTIAL"
                   Rotation="340"
                   FontSize="80"
                   FontAttributes="Bold,Italic"
                   TextColor="Gray"
                   Margin="10,0,0,0"
                   AbsoluteLayout.LayoutBounds="0.5,0.5,-1,-1"
                   AbsoluteLayout.LayoutFlags="PositionProportional"
                   Opacity="0.3"/>
        </AbsoluteLayout>
    </chart:SfPolarChart.PlotAreaBackgroundView>
</chart:SfPolarChart>
```

### Copyright Notice

```xml
<chart:SfPolarChart>
    <chart:SfPolarChart.PlotAreaBackgroundView>
        <AbsoluteLayout>
            <Label Text="Copyright @ 2001 - 2024 Syncfusion Inc"
                   FontSize="18"
                   AbsoluteLayout.LayoutBounds="1,1,-1,-1"
                   AbsoluteLayout.LayoutFlags="PositionProportional"
                   Opacity="0.4"/>
        </AbsoluteLayout>
    </chart:SfPolarChart.PlotAreaBackgroundView>
</chart:SfPolarChart>
```

### Background Gradient

```xml
<chart:SfPolarChart>
    <chart:SfPolarChart.PlotAreaBackgroundView>
        <Grid>
            <Grid.Background>
                <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
                    <GradientStop Color="#E3F2FD" Offset="0.0"/>
                    <GradientStop Color="#FFFFFF" Offset="1.0"/>
                </LinearGradientBrush>
            </Grid.Background>
        </Grid>
    </chart:SfPolarChart.PlotAreaBackgroundView>
</chart:SfPolarChart>
```

## Liquid Glass Effect

Modern glassy appearance for iOS and macOS (.NET 10+).

### Apply to Chart Background

Wrap chart in `SfGlassEffectView`:

```xml
<core:SfGlassEffectView CornerRadius="20"
                        Padding="12"
                        EffectType="Regular"
                        EnableShadowEffect="True">
    <chart:SfPolarChart>
        <!-- Series -->
    </chart:SfPolarChart>
</core:SfGlassEffectView>
```

**C#:**
```csharp
SfPolarChart chart = new SfPolarChart();
// Configure chart...

var glassEffect = new SfGlassEffectView
{
    CornerRadius = 20,
    Padding = 12,
    EffectType = GlassEffectType.Regular,  // Regular or Clear
    EnableShadowEffect = true,
    Content = chart
};
```

### Enable for Tooltip

```csharp
chart.EnableLiquidGlassEffect = true;
```

**XAML:**
```xml
<chart:SfPolarChart EnableLiquidGlassEffect="True">
    <chart:SfPolarChart.TooltipBehavior>
        <chart:ChartTooltipBehavior/>
    </chart:SfPolarChart.TooltipBehavior>
    
    <chart:PolarLineSeries EnableTooltip="True" .../>
</chart:SfPolarChart>
```

**Best Practices:**
- Use over images or colorful backgrounds for maximum effect
- Set `EffectType="Regular"` for blurrier look
- Set `EffectType="Clear"` for glassy look
- For custom tooltip templates, use `Transparent` background

**Learn more:** [liquid-glass-effect documentation](https://help.syncfusion.com/maui/liquid-glass-ui/getting-started)

## Common Patterns

### Pattern 1: Professional Blue Theme

```csharp
List<Brush> bluePalette = new List<Brush>
{
    new SolidColorBrush(Color.FromArgb("#1976D2")),  // Dark blue
    new SolidColorBrush(Color.FromArgb("#2196F3")),  // Blue
    new SolidColorBrush(Color.FromArgb("#64B5F6")),  // Light blue
    new SolidColorBrush(Color.FromArgb("#90CAF9"))   // Lighter blue
};

chart.PaletteBrushes = bluePalette;
```

### Pattern 2: High-Contrast Series

```csharp
// Distinct colors for clear differentiation
series1.Stroke = new SolidColorBrush(Color.FromArgb("#FF0000"));  // Red
series2.Stroke = new SolidColorBrush(Color.FromArgb("#00FF00"));  // Green
series3.Stroke = new SolidColorBrush(Color.FromArgb("#0000FF"));  // Blue
```

### Pattern 3: Subtle Area Series with Gradients

```csharp
PolarAreaSeries series = new PolarAreaSeries();

LinearGradientBrush gradient = new LinearGradientBrush
{
    StartPoint = new Point(0, 0),
    EndPoint = new Point(0, 1)
};
gradient.GradientStops.Add(new GradientStop 
{ 
    Color = Color.FromArgb("#80E3F2FD"), 
    Offset = 0 
});
gradient.GradientStops.Add(new GradientStop 
{ 
    Color = Color.FromArgb("#10E3F2FD"), 
    Offset = 1 
});

series.Fill = gradient;
series.Stroke = new SolidColorBrush(Color.FromArgb("#2196F3"));
series.StrokeWidth = 2;
```

### Pattern 4: Dark Theme

```csharp
chart.BackgroundColor = Color.FromArgb("#212121");  // Dark background

List<Brush> darkPalette = new List<Brush>
{
    new SolidColorBrush(Color.FromArgb("#64FFDA")),  // Teal
    new SolidColorBrush(Color.FromArgb("#FFD740")),  // Yellow
    new SolidColorBrush(Color.FromArgb("#FF6E40"))   // Orange
};

chart.PaletteBrushes = darkPalette;

// Light axis labels for dark background
chart.PrimaryAxis.LabelStyle = new ChartAxisLabelStyle 
{ 
    TextColor = Colors.White 
};
chart.SecondaryAxis.LabelStyle = new ChartAxisLabelStyle 
{ 
    TextColor = Colors.White 
};
```

## Troubleshooting

### Custom Colors Not Applying

**Problem:** Set custom palette but series use default colors.

**Solution:**
```csharp
// Ensure palette is set on chart
chart.PaletteBrushes = customBrushes;

// Or set directly on series
series.Stroke = new SolidColorBrush(Colors.Blue);
```

### Gradients Not Visible

**Problem:** Gradient applied but appears as solid color.

**Solution:**
```csharp
// Ensure gradient has multiple stops
gradient.GradientStops = new GradientStopCollection
{
    new GradientStop { Offset = 0, Color = Colors.Blue },
    new GradientStop { Offset = 1, Color = Colors.LightBlue }
};

// Check opacity isn't hiding gradient
series.Opacity = 1.0;
```

### Plot Area Background Not Showing

**Problem:** PlotAreaBackgroundView set but not visible.

**Solution:**
```xml
<!-- Ensure view has size -->
<chart:SfPolarChart.PlotAreaBackgroundView>
    <Grid HeightRequest="400" WidthRequest="400">
        <!-- Content -->
    </Grid>
</chart:SfPolarChart.PlotAreaBackgroundView>
```

## Related Topics

- **Series Types**: [series-types.md](series-types.md) - Series-specific styling
- **Legend**: [legend-tooltip.md](legend-tooltip.md) - Legend icon colors
- **Markers**: [data-labels-markers.md](data-labels-markers.md) - Marker colors
