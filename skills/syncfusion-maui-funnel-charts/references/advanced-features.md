# Advanced Features in .NET MAUI Funnel Chart

This guide covers advanced customization features for `SfFunnelChart`, including orientation control, segment spacing, and the modern Liquid Glass Effect for enhanced visual appeal.

## Table of Contents
- [Orientation](#orientation)
- [Segment Spacing](#segment-spacing)
- [Liquid Glass Effect](#liquid-glass-effect)

## Orientation

The `Orientation` property controls the rendering direction of funnel segments. The default is `Vertical` (bottom to top), but you can switch to `Horizontal` (right to left).

### Available Orientations
- **Vertical** (default): Segments arranged from bottom to top
- **Horizontal**: Segments arranged from right to left

### XAML
```xaml
<chart:SfFunnelChart Orientation="Horizontal"
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
chart.Orientation = ChartOrientation.Horizontal;

this.Content = chart;
```

### When to Use Each Orientation

**Vertical (Default):**
- Traditional funnel representation
- Best for top-to-bottom process flows
- Works well with bottom-placed legends
- Ideal for portrait layouts

**Horizontal:**
- Left-to-right reading patterns
- Better for landscape orientations
- Works well with left/right-placed legends
- Suitable for timeline-style visualizations

### Complete Orientation Example

```xaml
<ContentPage xmlns:chart="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts">
    
    <VerticalStackLayout Spacing="20" Padding="10">
        
        <!-- Vertical Funnel -->
        <Border Stroke="Gray" StrokeThickness="1" Padding="10">
            <VerticalStackLayout>
                <Label Text="Vertical Orientation" 
                       FontSize="16" 
                       FontAttributes="Bold"
                       HorizontalOptions="Center"/>
                <chart:SfFunnelChart ItemsSource="{Binding Data}"
                                     XBindingPath="Stage"
                                     YBindingPath="Value"
                                     Orientation="Vertical"
                                     HeightRequest="300">
                    <chart:SfFunnelChart.Legend>
                        <chart:ChartLegend Placement="Bottom"/>
                    </chart:SfFunnelChart.Legend>
                </chart:SfFunnelChart>
            </VerticalStackLayout>
        </Border>
        
        <!-- Horizontal Funnel -->
        <Border Stroke="Gray" StrokeThickness="1" Padding="10">
            <VerticalStackLayout>
                <Label Text="Horizontal Orientation" 
                       FontSize="16" 
                       FontAttributes="Bold"
                       HorizontalOptions="Center"/>
                <chart:SfFunnelChart ItemsSource="{Binding Data}"
                                     XBindingPath="Stage"
                                     YBindingPath="Value"
                                     Orientation="Horizontal"
                                     HeightRequest="300">
                    <chart:SfFunnelChart.Legend>
                        <chart:ChartLegend Placement="Right"/>
                    </chart:SfFunnelChart.Legend>
                </chart:SfFunnelChart>
            </VerticalStackLayout>
        </Border>
        
    </VerticalStackLayout>
    
</ContentPage>
```

## Segment Spacing

The `GapRatio` property controls the gap between funnel segments. This creates visual separation, making individual stages more distinct.

### Properties
- **GapRatio** (double): Value between `0` and `1`
  - `0`: No gap (default)
  - `1`: Maximum gap

### XAML
```xaml
<chart:SfFunnelChart GapRatio="0.2"
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
chart.GapRatio = 0.2;

this.Content = chart;
```

### Gap Ratio Examples

#### No Gap (Default)
```xaml
<chart:SfFunnelChart GapRatio="0"/>
```
- Segments touch each other
- Traditional funnel appearance
- Best for emphasizing flow

#### Small Gap
```xaml
<chart:SfFunnelChart GapRatio="0.1"/>
```
- Subtle separation
- Maintains funnel shape
- Slightly improved segment distinction

#### Medium Gap
```xaml
<chart:SfFunnelChart GapRatio="0.2"/>
```
- Noticeable separation
- Good balance between flow and distinction
- Recommended for most use cases

#### Large Gap
```xaml
<chart:SfFunnelChart GapRatio="0.5"/>
```
- Significant separation
- Individual segments stand out
- May lose funnel metaphor

### Segment Spacing with Visual Enhancements

```xaml
<chart:SfFunnelChart ItemsSource="{Binding Data}"
                     XBindingPath="Stage"
                     YBindingPath="Count"
                     GapRatio="0.15"
                     ShowDataLabels="True">
    
    <chart:SfFunnelChart.DataLabelSettings>
        <chart:FunnelDataLabelSettings LabelPlacement="Center">
            <chart:FunnelDataLabelSettings.LabelStyle>
                <chart:ChartDataLabelStyle FontSize="14"
                                          FontAttributes="Bold"
                                          TextColor="White"/>
            </chart:FunnelDataLabelSettings.LabelStyle>
        </chart:FunnelDataLabelSettings>
    </chart:SfFunnelChart.DataLabelSettings>
    
</chart:SfFunnelChart>
```

## Liquid Glass Effect

The Liquid Glass Effect is a modern design style providing a sleek, minimalist appearance with smooth rounded corners and sophisticated visual treatments. It creates a polished, professional look for your charts.

> **Platform Requirements:**
> - **.NET 10** or later
> - **iOS 26+** or **macOS 26+**
> - Not supported on Android or Windows

### Features
- **Tooltip**: Applies a glassy appearance to tooltips
- **Chart Background**: Blurred or clear glass effect using `SfGlassEffectView`

### Enable Liquid Glass Effect for Tooltip

Set `EnableLiquidGlassEffect` and `EnableTooltip` to `true`:

#### XAML
```xaml
<chart:SfFunnelChart EnableLiquidGlassEffect="True"
                     EnableTooltip="True"
                     ItemsSource="{Binding Data}"
                     XBindingPath="XValue"
                     YBindingPath="YValue">
    <!-- Chart configuration -->
</chart:SfFunnelChart>
```

#### C#
```csharp
SfFunnelChart chart = new SfFunnelChart();
chart.ItemsSource = viewModel.Data;
chart.XBindingPath = "XValue";
chart.YBindingPath = "YValue";
chart.EnableLiquidGlassEffect = true;
chart.EnableTooltip = true;

this.Content = chart;
```

### Apply Glass Effect to Chart Background

Wrap the `SfFunnelChart` inside an `SfGlassEffectView` (from `Syncfusion.Maui.Core` package):

#### XAML
```xaml
<ContentPage xmlns:chart="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts"
             xmlns:core="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core">

    <core:SfGlassEffectView CornerRadius="20"
                            Padding="12"
                            EffectType="Regular"
                            EnableShadowEffect="True">

        <chart:SfFunnelChart ItemsSource="{Binding Data}"
                             XBindingPath="XValue"
                             YBindingPath="YValue"
                             EnableLiquidGlassEffect="True"
                             EnableTooltip="True"/>

    </core:SfGlassEffectView>

</ContentPage>
```

#### C#
```csharp
using Syncfusion.Maui.Charts;
using Syncfusion.Maui.Core;

SfFunnelChart chart = new SfFunnelChart();
chart.ItemsSource = viewModel.Data;
chart.XBindingPath = "XValue";
chart.YBindingPath = "YValue";
chart.EnableLiquidGlassEffect = true;
chart.EnableTooltip = true;

var glassView = new SfGlassEffectView
{
    CornerRadius = 20,
    Padding = 12,
    EffectType = GlassEffectType.Regular, // Regular (blurrier) or Clear (glassy)
    EnableShadowEffect = true,
    Content = chart
};

this.Content = glassView;
```

### SfGlassEffectView Properties

| Property | Type | Description |
|----------|------|-------------|
| `EffectType` | GlassEffectType | `Regular` (blurrier) or `Clear` (glassy) |
| `CornerRadius` | double | Rounded corner radius |
| `Padding` | Thickness | Inner padding |
| `EnableShadowEffect` | bool | Enable drop shadow |

### Effect Type Comparison

**Regular Effect:**
- More blurred background
- Softer appearance
- Better for colorful backgrounds
- Creates stronger depth perception

**Clear Effect:**
- Crisper, glassy look
- Subtle blur
- Better for simple backgrounds
- More modern aesthetic

### Complete Liquid Glass Example

```xaml
<ContentPage xmlns:chart="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts"
             xmlns:core="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
             xmlns:model="clr-namespace:YourApp.ViewModels">

    <!-- Background Image -->
    <Grid>
        <Image Source="background_gradient.png" 
               Aspect="AspectFill"/>
        
        <core:SfGlassEffectView CornerRadius="20"
                                Padding="15"
                                Margin="20"
                                EffectType="Regular"
                                EnableShadowEffect="True"
                                VerticalOptions="Center"
                                HorizontalOptions="Center">

            <chart:SfFunnelChart ItemsSource="{Binding Data}"
                                 XBindingPath="Stage"
                                 YBindingPath="Value"
                                 EnableLiquidGlassEffect="True"
                                 EnableTooltip="True"
                                 ShowDataLabels="True"
                                 WidthRequest="400"
                                 HeightRequest="500">

                <chart:SfFunnelChart.Title>
                    <Label Text="Sales Funnel"
                           FontSize="24"
                           FontAttributes="Bold"
                           HorizontalTextAlignment="Center"/>
                </chart:SfFunnelChart.Title>

                <chart:SfFunnelChart.BindingContext>
                    <model:SalesFunnelViewModel/>
                </chart:SfFunnelChart.BindingContext>

                <chart:SfFunnelChart.Legend>
                    <chart:ChartLegend Placement="Bottom"/>
                </chart:SfFunnelChart.Legend>

            </chart:SfFunnelChart>

        </core:SfGlassEffectView>
    </Grid>

</ContentPage>
```

### Best Practices for Liquid Glass Effect

1. **Background Selection:**
   - Use images or colorful backgrounds for maximum effect
   - Plain backgrounds reduce glass effect visibility
   - Gradient backgrounds work exceptionally well

2. **Effect Type:**
   - Use `Regular` for vibrant, colorful backgrounds
   - Use `Clear` for subtle, minimalist designs
   - Test both on your specific background

3. **Corner Radius & Padding:**
   - Balance content density with visual polish
   - Typical values: `CornerRadius="15-25"`, `Padding="12-20"`
   - Adjust based on chart size

4. **Custom Tooltip Template:**
   - When using `TooltipTemplate`, set background to `Transparent`
   - This allows the glass effect to show through
   - Avoid opaque backgrounds in custom tooltips

5. **Platform Compatibility:**
   - Check platform version before enabling
   - Provide fallback styling for unsupported platforms
   - Test on actual iOS/macOS devices

### Liquid Glass with Custom Tooltip Template

```xaml
<Grid.Resources>
    <DataTemplate x:Key="glassTooltip">
        <!-- Use transparent background to show glass effect -->
        <Frame Background="Transparent" Padding="10">
            <VerticalStackLayout Spacing="5">
                <Label Text="{Binding Item.XValue}"
                       TextColor="White"
                       FontSize="16"
                       FontAttributes="Bold"/>
                <Label Text="{Binding Item.YValue, StringFormat='{0:N0}'}"
                       TextColor="White"
                       FontSize="14"/>
            </VerticalStackLayout>
        </Frame>
    </DataTemplate>
</Grid.Resources>

<chart:SfFunnelChart EnableLiquidGlassEffect="True"
                     EnableTooltip="True"
                     TooltipTemplate="{StaticResource glassTooltip}"/>
```

## Combining Advanced Features

### Example: Horizontal Funnel with Spacing and Glass Effect

```xaml
<core:SfGlassEffectView CornerRadius="20"
                        Padding="15"
                        EffectType="Clear"
                        EnableShadowEffect="True">

    <chart:SfFunnelChart ItemsSource="{Binding Data}"
                         XBindingPath="Stage"
                         YBindingPath="Value"
                         Orientation="Horizontal"
                         GapRatio="0.15"
                         EnableLiquidGlassEffect="True"
                         EnableTooltip="True"
                         ShowDataLabels="True">

        <chart:SfFunnelChart.DataLabelSettings>
            <chart:FunnelDataLabelSettings LabelPlacement="Center"/>
        </chart:SfFunnelChart.DataLabelSettings>

        <chart:SfFunnelChart.Legend>
            <chart:ChartLegend Placement="Right"/>
        </chart:SfFunnelChart.Legend>

    </chart:SfFunnelChart>

</core:SfGlassEffectView>
```

## Performance Considerations

1. **Liquid Glass Effect:**
   - May impact performance on lower-end devices
   - Test on target devices
   - Consider disabling for complex layouts

2. **Segment Spacing:**
   - Minimal performance impact
   - Safe to use in all scenarios

3. **Orientation:**
   - No performance difference between orientations
   - Choose based on design requirements

## Troubleshooting

**Liquid Glass Effect not visible:**
- Verify platform requirements (.NET 10, iOS/macOS 26+)
- Ensure `EnableLiquidGlassEffect="True"` is set
- Check that chart is on a colorful background
- Confirm `Syncfusion.Maui.Core` package is installed

**Segments appear disconnected:**
- `GapRatio` might be too high
- Reduce value (e.g., from 0.5 to 0.2)
- Consider if spacing suits your design intent

**Orientation not changing:**
- Verify spelling: `Orientation="Horizontal"` (not `Horizontal`)
- Ensure property is set on `SfFunnelChart`
- Check that chart has sufficient space to render

**Glass effect looks wrong:**
- Try switching between `Regular` and `Clear` effect types
- Adjust `CornerRadius` and `Padding`
- Verify background provides sufficient contrast