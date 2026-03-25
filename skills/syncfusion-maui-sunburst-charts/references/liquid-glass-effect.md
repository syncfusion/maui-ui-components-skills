# Liquid Glass Effect in .NET MAUI Sunburst Chart

## Table of Contents
- [Overview](#overview)
- [Platform Requirements](#platform-requirements)
- [Applying Glass Effect to Chart](#applying-glass-effect-to-chart)
- [Enabling Glass Effect on Tooltips](#enabling-glass-effect-on-tooltips)
- [Complete Examples](#complete-examples)
- [Best Practices and Tips](#best-practices-and-tips)
- [Troubleshooting](#troubleshooting)

## Overview

The Liquid Glass Effect is a modern design style that provides a sleek, minimalist appearance with clean lines, subtle visual effects, and elegant styling. It features smooth rounded corners and sophisticated visual treatments that create a polished, professional look for charts.

**Key characteristics:**
- Translucent, frosted glass appearance
- Blurred background effect
- Subtle shadows and depth
- Modern iOS/macOS design aesthetic
- Enhanced visual hierarchy

**Available for:**
- Chart background (via SfGlassEffectView wrapper)
- Tooltips (via EnableLiquidGlassEffect property)

## Platform Requirements

The liquid glass effect has specific platform and version requirements:

**Minimum requirements:**
- **.NET version**: .NET 10 or later
- **iOS**: Version 26 or later
- **macOS**: Version 26 or later

**Not supported on:**
- Android
- Windows
- Earlier versions of iOS/macOS
- .NET 9 and earlier

**Fallback behavior:**
- On unsupported platforms, charts render normally without glass effects
- No errors or crashes occur
- Graceful degradation ensures cross-platform compatibility

## Applying Glass Effect to Chart

Wrap the sunburst chart in an `SfGlassEffectView` to apply a glass appearance to the chart surface. This creates a blurred or clear glass background effect.

### Installing Required Package

The `SfGlassEffectView` is available in the Syncfusion.Maui.Core package:

```xml
<PackageReference Include="Syncfusion.Maui.Core" Version="27.1.48" />
```

### Basic Glass Effect Implementation

**XAML:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sunburst="clr-namespace:Syncfusion.Maui.SunburstChart;assembly=Syncfusion.Maui.SunburstChart"
             xmlns:core="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
             x:Class="YourApp.SunburstPage">

    <core:SfGlassEffectView CornerRadius="20"
                           Padding="12"
                           EffectType="Regular"
                           EnableShadowEffect="True">

        <sunburst:SfSunburstChart ItemsSource="{Binding DataSource}"
                                  ValueMemberPath="Value">
            <sunburst:SfSunburstChart.Levels>
                <sunburst:SunburstHierarchicalLevel GroupMemberPath="Category"/>
                <sunburst:SunburstHierarchicalLevel GroupMemberPath="Subcategory"/>
            </sunburst:SfSunburstChart.Levels>
        </sunburst:SfSunburstChart>

    </core:SfGlassEffectView>

</ContentPage>
```

**C#:**
```csharp
using Syncfusion.Maui.Core;
using Syncfusion.Maui.SunburstChart;

SfSunburstChart chart = new SfSunburstChart();
chart.ItemsSource = viewModel.DataSource;
chart.ValueMemberPath = "Value";
// ... configure chart

var glassView = new SfGlassEffectView
{
    CornerRadius = 20,
    Padding = 12,
    EffectType = GlassEffectType.Regular,
    EnableShadowEffect = true,
    Content = chart
};

this.Content = glassView;
```

### SfGlassEffectView Properties

**CornerRadius:**
- Defines rounded corner radius
- Values: 0 (sharp corners) to large values (circular)
- Typical range: 10-30 for modern look
- Type: double

**Padding:**
- Space between glass border and chart content
- Provides visual breathing room
- Typical range: 8-16 units
- Type: Thickness

**EffectType:**
- **Regular**: Blurrier, frosted glass appearance
- **Clear**: Crisper, more transparent glass look
- Type: GlassEffectType enum

**EnableShadowEffect:**
- Adds depth with shadow beneath glass surface
- true: Shadow enabled (recommended)
- false: No shadow
- Type: bool

### Effect Type Comparison

**Regular Effect:**
```xml
<core:SfGlassEffectView EffectType="Regular">
    <!-- Blurred, frosted appearance -->
    <!-- More opaque -->
    <!-- Strong visual separation -->
</core:SfGlassEffectView>
```

**Use when:**
- Maximum visual distinction needed
- Overlaying busy backgrounds
- Emphasizing chart as primary element
- Creating depth and hierarchy

**Clear Effect:**
```xml
<core:SfGlassEffectView EffectType="Clear">
    <!-- Crisper, glassy appearance -->
    <!-- More transparent -->
    <!-- Subtle integration -->
</core:SfGlassEffectView>
```

**Use when:**
- Subtle, elegant appearance desired
- Background should remain visible
- Creating lightweight feel
- Integrating with existing design

## Enabling Glass Effect on Tooltips

Apply the liquid glass effect to chart tooltips for a cohesive modern design.

### Enabling Tooltip Glass Effect

Set both `EnableLiquidGlassEffect` and `EnableTooltip` properties to true:

**XAML:**
```xml
<sunburst:SfSunburstChart EnableLiquidGlassEffect="True"
                          EnableTooltip="True"
                          ItemsSource="{Binding DataSource}"
                          ValueMemberPath="Sales">
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Region"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Product"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
SfSunburstChart chart = new SfSunburstChart();
chart.EnableLiquidGlassEffect = true;
chart.EnableTooltip = true;
chart.ItemsSource = viewModel.DataSource;
chart.ValueMemberPath = "Sales";
// ... configure levels

this.Content = chart;
```

### Custom Tooltip Templates with Glass Effect

When using custom tooltip templates, set background to `Transparent` to allow the glass effect to show through:

**XAML:**
```xml
<sunburst:SfSunburstChart EnableLiquidGlassEffect="True"
                          EnableTooltip="True"
                          ItemsSource="{Binding DataSource}"
                          ValueMemberPath="Value"
                          TooltipTemplate="{StaticResource GlassTooltipTemplate}">
    
    <sunburst:SfSunburstChart.Resources>
        <ResourceDictionary>
            <DataTemplate x:Key="GlassTooltipTemplate">
                <StackLayout BackgroundColor="Transparent" Padding="12">
                    <!-- Transparent background allows glass effect to show -->
                    <Label Text="{Binding Item[0]}" 
                         TextColor="White" 
                         FontSize="14" 
                         FontAttributes="Bold"/>
                    <Label Text="{Binding Item[1], StringFormat='{0:N0}'}" 
                         TextColor="LightGray" 
                         FontSize="12"/>
                </StackLayout>
            </DataTemplate>
        </ResourceDictionary>
    </sunburst:SfSunburstChart.Resources>
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Category"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

**Important:**
- Set `BackgroundColor="Transparent"` on template root
- The glass effect provides its own background
- Opaque backgrounds will hide the glass effect
- Test on actual iOS/macOS devices

## Complete Examples

### Example 1: Full Glass Effect Dashboard

```xml
<ContentPage xmlns:sunburst="clr-namespace:Syncfusion.Maui.SunburstChart;assembly=Syncfusion.Maui.SunburstChart"
             xmlns:core="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core">

    <!-- Background image for glass effect to blur -->
    <Grid>
        <Image Source="background.jpg" Aspect="AspectFill"/>
        
        <core:SfGlassEffectView CornerRadius="25"
                               Padding="16"
                               EffectType="Regular"
                               EnableShadowEffect="True"
                               Margin="20"
                               VerticalOptions="Center">

            <sunburst:SfSunburstChart ItemsSource="{Binding SalesData}"
                                      ValueMemberPath="Revenue"
                                      EnableLiquidGlassEffect="True"
                                      EnableTooltip="True"
                                      ShowLabels="True"
                                      InnerRadius="0.5">

                <sunburst:SfSunburstChart.Title>
                    <Label Text="Sales Dashboard" 
                         FontSize="20" 
                         FontAttributes="Bold"
                         TextColor="White"/>
                </sunburst:SfSunburstChart.Title>

                <sunburst:SfSunburstChart.Levels>
                    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Region"/>
                    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Product"/>
                    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Model"/>
                </sunburst:SfSunburstChart.Levels>

            </sunburst:SfSunburstChart>

        </core:SfGlassEffectView>
    </Grid>

</ContentPage>
```

### Example 2: Clear Glass with Drill-Down

```xml
<core:SfGlassEffectView CornerRadius="20"
                       Padding="12"
                       EffectType="Clear"
                       EnableShadowEffect="True">

    <sunburst:SfSunburstChart ItemsSource="{Binding OrgData}"
                              ValueMemberPath="EmployeeCount"
                              EnableLiquidGlassEffect="True"
                              EnableTooltip="True"
                              EnableDrillDown="True"
                              Radius="0.9">

        <sunburst:SfSunburstChart.ToolbarSettings>
            <sunburst:SunburstToolbarSettings 
                HorizontalAlignment="End"
                VerticalAlignment="Start"
                IconBrush="White"
                Background="Transparent"/>
        </sunburst:SfSunburstChart.ToolbarSettings>

        <sunburst:SfSunburstChart.Levels>
            <sunburst:SunburstHierarchicalLevel GroupMemberPath="Division"/>
            <sunburst:SunburstHierarchicalLevel GroupMemberPath="Department"/>
            <sunburst:SunburstHierarchicalLevel GroupMemberPath="Team"/>
        </sunburst:SfSunburstChart.Levels>

    </sunburst:SfSunburstChart>

</core:SfGlassEffectView>
```

### Example 3: Glass Effect with Center View

```xml
<core:SfGlassEffectView CornerRadius="30"
                       Padding="20"
                       EffectType="Regular"
                       EnableShadowEffect="True">

    <sunburst:SfSunburstChart ItemsSource="{Binding BudgetData}"
                              ValueMemberPath="Amount"
                              EnableLiquidGlassEffect="True"
                              EnableTooltip="True"
                              InnerRadius="0.6">

        <sunburst:SfSunburstChart.CenterView>
            <VerticalStackLayout HorizontalOptions="Center" 
                                VerticalOptions="Center"
                                Spacing="8">
                <Label Text="Total Budget" 
                     FontSize="14" 
                     TextColor="White"
                     HorizontalTextAlignment="Center"/>
                <Label Text="{Binding TotalBudget, StringFormat='${0:N0}M'}" 
                     FontSize="28" 
                     FontAttributes="Bold"
                     TextColor="White"
                     HorizontalTextAlignment="Center"/>
            </VerticalStackLayout>
        </sunburst:SfSunburstChart.CenterView>

        <sunburst:SfSunburstChart.Levels>
            <sunburst:SunburstHierarchicalLevel GroupMemberPath="Department"/>
            <sunburst:SunburstHierarchicalLevel GroupMemberPath="Category"/>
        </sunburst:SfSunburstChart.Levels>

    </sunburst:SfSunburstChart>

</core:SfGlassEffectView>
```

### Example 4: Compact Mobile Glass Card

```xml
<core:SfGlassEffectView CornerRadius="15"
                       Padding="10"
                       EffectType="Clear"
                       EnableShadowEffect="True"
                       Margin="16">

    <sunburst:SfSunburstChart ItemsSource="{Binding Data}"
                              ValueMemberPath="Count"
                              EnableLiquidGlassEffect="True"
                              EnableTooltip="True"
                              Radius="0.85"
                              InnerRadius="0.4">

        <sunburst:SfSunburstChart.Levels>
            <sunburst:SunburstHierarchicalLevel GroupMemberPath="Type"/>
            <sunburst:SunburstHierarchicalLevel GroupMemberPath="Subtype"/>
        </sunburst:SfSunburstChart.Levels>

    </sunburst:SfSunburstChart>

</core:SfGlassEffectView>
```

## Best Practices and Tips

### When to Use Glass Effects

**Ideal scenarios:**
- iOS/macOS exclusive applications
- Modern, premium app designs
- Dashboard and analytics interfaces
- Apps with image/video backgrounds
- Design-forward consumer applications

**Not recommended for:**
- Cross-platform apps targeting Android/Windows primarily
- Business/enterprise apps requiring platform consistency
- Apps targeting older iOS/macOS versions
- Performance-critical applications

### Maximizing Visual Impact

**Background matters:**
- Glass effects are most visible over images or colorful backgrounds
- Use gradient or textured backgrounds
- Avoid plain white/solid color backgrounds
- Test with your actual app background

**Optimal settings:**
- **Regular effect**: Use over busy backgrounds for better separation
- **Clear effect**: Use over subtle backgrounds for elegance
- **CornerRadius**: 15-25 for modern look
- **Padding**: 12-16 for proper spacing

**Shadow usage:**
- Always enable shadows (EnableShadowEffect="True") for depth
- Shadows enhance the floating glass appearance
- Provides visual hierarchy and separation

### Content Tuning

**Text visibility:**
- Use white or light-colored text for best contrast
- Bold fonts work better with glass backgrounds
- Test readability on actual devices
- Consider text shadows if needed

**Chart colors:**
- Brighter, more saturated colors work best
- Avoid very pale pastels (low contrast)
- Test color schemes with glass effect enabled
- Consider accessibility and colorblindness

**Spacing:**
- Increase padding in SfGlassEffectView for breathing room
- Provide margin around glass container
- Don't overcrowd content
- Balance content density with elegance

### Tooltip Considerations

**Custom templates:**
- Always set background to Transparent
- Use light-colored text (white, light gray)
- Keep content minimal for clean appearance
- Test tooltip visibility on actual glass

**Standard tooltips:**
- EnableLiquidGlassEffect works automatically
- Default styling is optimized for glass
- Minimal configuration needed
- Consistent with chart aesthetics

## Platform-Specific Implementation

### iOS/macOS Only Code

Use conditional compilation or runtime checks for platform-specific features:

**C# with Platform Checks:**
```csharp
#if IOS || MACCATALYST
if (DeviceInfo.Version.Major >= 26)
{
    var glassView = new SfGlassEffectView
    {
        CornerRadius = 20,
        EffectType = GlassEffectType.Regular,
        EnableShadowEffect = true,
        Content = chart
    };
    this.Content = glassView;
}
else
{
    // Fallback for older versions
    this.Content = chart;
}
#else
// Non-Apple platforms
this.Content = chart;
#endif
```

**XAML with OnPlatform:**
```xml
<ContentPage.Content>
    <OnPlatform x:TypeArguments="View">
        <On Platform="iOS, MacCatalyst">
            <core:SfGlassEffectView CornerRadius="20" 
                                   EffectType="Regular"
                                   EnableShadowEffect="True">
                <sunburst:SfSunburstChart x:Name="chart"/>
            </core:SfGlassEffectView>
        </On>
        <On Platform="Android, WinUI">
            <sunburst:SfSunburstChart x:Name="chart"/>
        </On>
    </OnPlatform>
</ContentPage.Content>
```

## Troubleshooting

**Issue:** Glass effect not visible
- **Solution:** Verify platform requirements (.NET 10+, iOS/macOS 26+)
- Test on actual device, not simulator
- Ensure background isn't plain solid color
- Check that EffectType is set correctly

**Issue:** Tooltip glass effect not showing
- **Solution:** Verify `EnableLiquidGlassEffect="True"` is set
- Ensure `EnableTooltip="True"` is also set
- Check custom template has `BackgroundColor="Transparent"`
- Test on supported platform and version

**Issue:** Chart content not visible through glass
- **Solution:** EffectType="Regular" may be too opaque
- Try EffectType="Clear" for more transparency
- Adjust chart colors for better contrast
- Increase chart brightness

**Issue:** Performance issues with glass effect
- **Solution:** Glass effects use GPU rendering and may impact performance
- Test on target devices, not just high-end development machines
- Consider disabling on lower-end devices
- Simplify chart complexity if needed

**Issue:** Glass effect looks different than expected
- **Solution:** Glass appearance depends on background
- Test with appropriate background (image, gradient)
- Try both Regular and Clear effect types
- Verify on actual iOS/macOS hardware

**Issue:** Shadows not appearing
- **Solution:** Verify `EnableShadowEffect="True"` is set
- Check that container has adequate space for shadow rendering
- Shadows may not be visible on very light backgrounds
- Test on device, not simulator

**Issue:** Custom tooltip background visible instead of glass
- **Solution:** Ensure template root has `BackgroundColor="Transparent"`
- Remove any explicit background brushes
- Check nested elements aren't setting backgrounds
- Verify glass effect is enabled on chart

## Additional Resources

For detailed guidance on SfGlassEffectView, refer to the Syncfusion documentation:
- SfGlassEffectView Getting Started
- Liquid Glass UI Design Guidelines
- Platform-Specific Implementation Guide

**Note:** The liquid glass effect is part of Apple's design language and works best when following iOS/macOS design principles.
