# Orientation and Liquid Glass Effects

## Table of Contents
- [Chart Orientation](#chart-orientation)
- [Liquid Glass Effect Overview](#liquid-glass-effect-overview)
- [Platform Requirements](#platform-requirements)
- [Applying Glass Effect to Chart](#applying-glass-effect-to-chart)
- [Glass Effect for Tooltips](#glass-effect-for-tooltips)
- [Best Practices](#best-practices)
- [Complete Examples](#complete-examples)

## Chart Orientation

The `Orientation` property controls the rendering direction of pyramid segments. Two orientations are available: **Vertical** (default) and **Horizontal**.

### Vertical Orientation (Default)

Segments are arranged from top to bottom, with the widest segment at the bottom.

**XAML:**
```xaml
<chart:SfPyramidChart ItemsSource="{Binding Data}"
                      XBindingPath="Name"
                      YBindingPath="Value"
                      Orientation="Vertical"/>
```

**C#:**
```csharp
SfPyramidChart chart = new SfPyramidChart();
chart.ItemsSource = viewModel.Data;
chart.XBindingPath = "Name";
chart.YBindingPath = "Value";
chart.Orientation = ChartOrientation.Vertical;
this.Content = chart;
```

**Best For:**
- Traditional pyramid visualization
- Portrait-oriented layouts
- Standard funnel representations
- Most common use case

### Horizontal Orientation

Segments are arranged from right to left, with the widest segment on the right.

**XAML:**
```xaml
<chart:SfPyramidChart ItemsSource="{Binding Data}"
                      XBindingPath="Name"
                      YBindingPath="Value"
                      Orientation="Horizontal"/>
```

**C#:**
```csharp
SfPyramidChart chart = new SfPyramidChart();
chart.ItemsSource = viewModel.Data;
chart.XBindingPath = "Name";
chart.YBindingPath = "Value";
chart.Orientation = ChartOrientation.Horizontal;
this.Content = chart;
```

**Best For:**
- Landscape-oriented layouts
- Emphasizing left-to-right flow
- Dashboards with horizontal space
- Alternative visual presentation

### Orientation Selection Guide

**Use Vertical Orientation when:**
- Following traditional pyramid chart conventions
- Working in portrait mode or vertical layouts
- Representing hierarchical data (top to bottom)
- Space is limited horizontally

**Use Horizontal Orientation when:**
- Chart is in a landscape layout
- Emphasizing process flow (left to right)
- More horizontal space is available
- Creating unique visual presentations

### Orientation with Other Features

```xaml
<chart:SfPyramidChart ItemsSource="{Binding Data}"
                      XBindingPath="Name"
                      YBindingPath="Value"
                      Orientation="Horizontal"
                      GapRatio="0.15"
                      ShowDataLabels="True">
    
    <chart:SfPyramidChart.Legend>
        <chart:ChartLegend Placement="Right"/>
    </chart:SfPyramidChart.Legend>
    
</chart:SfPyramidChart>
```

## Liquid Glass Effect Overview

The Liquid Glass Effect is a modern design style that provides a sleek, minimalist appearance with clean lines, subtle visual effects, and elegant styling. It features smooth rounded corners and sophisticated visual treatments creating a polished, professional look.

**Key Features:**
- Glassy, translucent appearance
- Subtle blur effects
- Modern, elegant styling
- Platform-specific optimization
- Enhanced tooltip aesthetics

**Note:** Liquid glass effects are only supported on **iOS** and **macOS** platforms running **.NET 10** or later, with OS versions **26+**.

## Platform Requirements

### Supported Platforms

| Platform | Minimum Version | .NET Version |
|----------|----------------|--------------|
| **iOS** | iOS 26+ | .NET 10 |
| **macOS** | macOS 26+ | .NET 10 |
| Windows | Not supported | N/A |
| Android | Not supported | N/A |

### Checking Platform Support

Before enabling liquid glass effects, verify platform compatibility:

```csharp
bool IsLiquidGlassSupported()
{
    #if IOS || MACCATALYST
        return DeviceInfo.Platform == DevicePlatform.iOS || 
               DeviceInfo.Platform == DevicePlatform.macOS;
    #else
        return false;
    #endif
}

// Usage
if (IsLiquidGlassSupported())
{
    chart.EnableLiquidGlassEffect = true;
}
```

## Applying Glass Effect to Chart

### Using SfGlassEffectView

Wrap the `SfPyramidChart` inside an `SfGlassEffectView` to give the chart surface a glass appearance. `SfGlassEffectView` is available in the `Syncfusion.Maui.Core` package.

**Installation:**
```
Install-Package Syncfusion.Maui.Core
```

**Namespace Import:**
```xaml
xmlns:core="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
```

### Basic Glass Effect Implementation

**XAML:**
```xaml
<core:SfGlassEffectView CornerRadius="20"
                        Padding="12"
                        EffectType="Regular"
                        EnableShadowEffect="True">
    
    <chart:SfPyramidChart ItemsSource="{Binding Data}"
                          XBindingPath="Name"
                          YBindingPath="Value"
                          ShowDataLabels="True"/>
    
</core:SfGlassEffectView>
```

**C#:**
```csharp
using Syncfusion.Maui.Core;
using Syncfusion.Maui.Charts;

SfPyramidChart chart = new SfPyramidChart();
chart.ItemsSource = viewModel.Data;
chart.XBindingPath = "Name";
chart.YBindingPath = "Value";

var glassEffect = new SfGlassEffectView
{
    CornerRadius = 20,
    Padding = 12,
    EffectType = GlassEffectType.Regular,
    EnableShadowEffect = true,
    Content = chart
};

this.Content = glassEffect;
```

### SfGlassEffectView Properties

| Property | Type | Description |
|----------|------|-------------|
| **EffectType** | GlassEffectType | Regular (blurred) or Clear (glassy) |
| **CornerRadius** | double | Rounded corner radius |
| **Padding** | Thickness | Space inside the glass container |
| **EnableShadowEffect** | bool | Enable drop shadow |
| **Content** | View | The chart to wrap |

### Effect Types

**Regular Effect (Blurrier):**
```xaml
<core:SfGlassEffectView EffectType="Regular">
    <chart:SfPyramidChart/>
</core:SfGlassEffectView>
```

**Clear Effect (Glassier):**
```xaml
<core:SfGlassEffectView EffectType="Clear">
    <chart:SfPyramidChart/>
</core:SfGlassEffectView>
```

### Full Styling Options

```xaml
<core:SfGlassEffectView CornerRadius="25"
                        Padding="20"
                        Margin="10"
                        EffectType="Regular"
                        EnableShadowEffect="True"
                        BackgroundColor="Transparent">
    
    <chart:SfPyramidChart ItemsSource="{Binding Data}"
                          XBindingPath="Name"
                          YBindingPath="Value"
                          ShowDataLabels="True"
                          EnableTooltip="True"/>
    
</core:SfGlassEffectView>
```

## Glass Effect for Tooltips

Enable liquid glass effect for tooltips by setting the `EnableLiquidGlassEffect` property to `true`.

### Basic Tooltip Glass Effect

**XAML:**
```xaml
<chart:SfPyramidChart ItemsSource="{Binding Data}"
                      XBindingPath="Name"
                      YBindingPath="Value"
                      EnableLiquidGlassEffect="True"
                      EnableTooltip="True"/>
```

**C#:**
```csharp
SfPyramidChart chart = new SfPyramidChart();
chart.ItemsSource = viewModel.Data;
chart.XBindingPath = "Name";
chart.YBindingPath = "Value";
chart.EnableLiquidGlassEffect = true;
chart.EnableTooltip = true;
this.Content = chart;
```

**Result:** Tooltips will have a glass-like appearance with subtle blur effects.

### Custom Tooltip Template with Glass Effect

When using custom `TooltipTemplate`, set the background to `Transparent` to allow the glass effect to show through:

**XAML:**
```xaml
<Grid>
    <Grid.Resources>
        <DataTemplate x:Key="glassTooltipTemplate">
            <Border BackgroundColor="Transparent" Padding="15">
                <StackLayout Spacing="5">
                    <Label Text="{Binding Item.Name}"
                           TextColor="White"
                           FontSize="14"
                           FontAttributes="Bold"/>
                    <Label Text="{Binding Item.Value, StringFormat='Value: {0:N0}'}"
                           TextColor="LightGray"
                           FontSize="12"/>
                </StackLayout>
            </Border>
        </DataTemplate>
    </Grid.Resources>

    <chart:SfPyramidChart EnableLiquidGlassEffect="True"
                          EnableTooltip="True"
                          TooltipTemplate="{StaticResource glassTooltipTemplate}"/>
</Grid>
```

**Important:** Always use `BackgroundColor="Transparent"` in custom templates to ensure the glass effect is visible.

### Platform-Conditional Glass Effect

```csharp
SfPyramidChart chart = new SfPyramidChart();
chart.ItemsSource = viewModel.Data;
chart.XBindingPath = "Name";
chart.YBindingPath = "Value";
chart.EnableTooltip = true;

// Enable glass effect only on supported platforms
#if IOS || MACCATALYST
    chart.EnableLiquidGlassEffect = true;
#endif

this.Content = chart;
```

## Best Practices

### Glass Effect Backgrounds

- **Most Visible**: Glass effects work best over images or colorful backgrounds
- **Contrast**: Ensure content remains readable with glass effect applied
- **Layering**: Use glass effect on foreground elements over interesting backgrounds

### Effect Type Selection

**Use Regular (Blurrier) when:**
- You want a more pronounced glass effect
- Background has busy patterns
- Creating a softer, dreamy aesthetic
- Emphasizing depth and layering

**Use Clear (Glassier) when:**
- You want subtle transparency
- Background should be more visible
- Creating a crisp, modern look
- Content behind glass is important

### Corner Radius and Padding

**Corner Radius Guidelines:**
- **10-15**: Subtle rounding
- **20-25**: Balanced modern look (recommended)
- **30+**: Prominent rounded appearance

**Padding Guidelines:**
- **10-15**: Tight spacing
- **15-20**: Comfortable spacing (recommended)
- **20+**: Generous spacing

### Performance Considerations

- Glass effects use GPU acceleration
- Minimal performance impact on modern devices
- Test on older devices if targeting broad audience
- Consider fallback for unsupported platforms

### Design Tips

1. **Background Choice**: Use images or gradients behind glass effects for maximum impact
2. **Content Density**: Balance content and visual polish with padding
3. **Consistency**: Use consistent corner radius across app
4. **Accessibility**: Ensure text remains readable with glass effects
5. **Testing**: Test on actual iOS/macOS devices for accurate rendering

## Complete Examples

### Example 1: Full Glass Effect Implementation

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:chart="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts"
             xmlns:core="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
             xmlns:local="clr-namespace:YourNamespace">
    
    <Grid>
        <!-- Background Image for Glass Effect -->
        <Image Source="background.jpg" Aspect="AspectFill"/>
        
        <!-- Glass Container with Chart -->
        <core:SfGlassEffectView CornerRadius="25"
                                Padding="20"
                                Margin="20"
                                EffectType="Regular"
                                EnableShadowEffect="True"
                                VerticalOptions="Center"
                                HorizontalOptions="Center"
                                WidthRequest="400"
                                HeightRequest="500">
            
            <chart:SfPyramidChart ItemsSource="{Binding Data}"
                                  XBindingPath="Name"
                                  YBindingPath="Value"
                                  ShowDataLabels="True"
                                  EnableTooltip="True"
                                  EnableLiquidGlassEffect="True"
                                  GapRatio="0.1">
                
                <chart:SfPyramidChart.Title>
                    <Label Text="Sales Pipeline" 
                           FontSize="18" 
                           FontAttributes="Bold"
                           TextColor="White"
                           HorizontalOptions="Center"/>
                </chart:SfPyramidChart.Title>
                
                <chart:SfPyramidChart.Legend>
                    <chart:ChartLegend Placement="Bottom"/>
                </chart:SfPyramidChart.Legend>
                
            </chart:SfPyramidChart>
            
        </core:SfGlassEffectView>
    </Grid>
    
</ContentPage>
```

### Example 2: Horizontal Orientation with Glass Effect

```csharp
// Background
var background = new Image 
{ 
    Source = "gradient_background.png", 
    Aspect = Aspect.AspectFill 
};

// Create chart
var chart = new SfPyramidChart
{
    ItemsSource = viewModel.Data,
    XBindingPath = "Name",
    YBindingPath = "Value",
    Orientation = ChartOrientation.Horizontal,
    ShowDataLabels = true,
    EnableTooltip = true,
    EnableLiquidGlassEffect = true,
    GapRatio = 0.15
};

chart.Legend = new ChartLegend { Placement = LegendPlacement.Right };

// Wrap in glass effect
var glassEffect = new SfGlassEffectView
{
    CornerRadius = 20,
    Padding = 15,
    Margin = 20,
    EffectType = GlassEffectType.Clear,
    EnableShadowEffect = true,
    Content = chart
};

// Layout
var grid = new Grid();
grid.Children.Add(background);
grid.Children.Add(glassEffect);

this.Content = grid;
```

### Example 3: Platform-Conditional Implementation

```csharp
public class PyramidChartPage : ContentPage
{
    public PyramidChartPage()
    {
        var chart = CreateChart();
        Content = IsGlassEffectSupported() ? CreateGlassView(chart) : chart;
    }
    
    private SfPyramidChart CreateChart()
    {
        var chart = new SfPyramidChart
        {
            ItemsSource = new ViewModel().Data,
            XBindingPath = "Name",
            YBindingPath = "Value",
            ShowDataLabels = true,
            EnableTooltip = true
        };
        
        if (IsGlassEffectSupported())
        {
            chart.EnableLiquidGlassEffect = true;
        }
        
        return chart;
    }
    
    private View CreateGlassView(SfPyramidChart chart)
    {
        var background = new Image 
        { 
            Source = "background.jpg", 
            Aspect = Aspect.AspectFill 
        };
        
        var glassEffect = new SfGlassEffectView
        {
            CornerRadius = 25,
            Padding = 20,
            Margin = 20,
            EffectType = GlassEffectType.Regular,
            EnableShadowEffect = true,
            Content = chart
        };
        
        var grid = new Grid();
        grid.Children.Add(background);
        grid.Children.Add(glassEffect);
        
        return grid;
    }
    
    private bool IsGlassEffectSupported()
    {
        #if IOS || MACCATALYST
            return DeviceInfo.Platform == DevicePlatform.iOS || 
                   DeviceInfo.Platform == DevicePlatform.macOS;
        #else
            return false;
        #endif
    }
}
```

## Troubleshooting

### Issue: Glass Effect Not Showing

**Solutions:**
- Verify platform is iOS or macOS with version 26+
- Confirm .NET 10 or later is being used
- Check Syncfusion.Maui.Core package is installed
- Ensure device/emulator supports the feature
- Test on physical device (not all simulators support effects)

### Issue: Chart Not Visible Through Glass

**Solutions:**
- Ensure glass view has sufficient size
- Check padding doesn't hide chart content
- Verify chart has data bound correctly
- Use lighter EffectType (Clear vs Regular)

### Issue: Tooltip Template Not Showing Glass Effect

**Solution:**
```xaml
<!-- Ensure template background is Transparent -->
<Border BackgroundColor="Transparent">
    <!-- Template content -->
</Border>
```