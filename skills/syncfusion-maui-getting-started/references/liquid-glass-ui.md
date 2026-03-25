# Liquid Glass UI

## Overview

Liquid Glass UI is Syncfusion's premium visual design system for .NET MAUI, offering glassmorphism effects and modern aesthetics.

**Key Features:**
- Transparent, layered backgrounds
- Blur effects and shadows
- Smooth animations
- Premium, modern look
- Built-in dark mode support

**Availability:** Requires Syncfusion.Maui.Core v20.2.0+

## What is Liquid Glass UI?

Liquid Glass UI creates depth through:
- **Transparency:** Semi-transparent backgrounds
- **Blur:** Background blur effects (glassmorphism)
- **Layering:** Elevated surfaces with shadows
- **Subtle animations:** Smooth transitions

**Visual Style:**
- Contemporary, premium design
- Reduced visual clutter
- Focus on content hierarchy
- Platform-native feel

## Supported Controls

### Glassy Controls Available


**Regular Controls with Glassy Styling:**
- SfButton
- SfCard
- SfChipGroup
- SfListView
- SfDataGrid

Most Syncfusion controls support Liquid Glass styling through properties.

## Installation

Liquid Glass UI is included in Core package:

```bash
dotnet add package Syncfusion.Maui.Core
```

No separate package required.

## Basic Implementation

### Enable Liquid Glass Theme

**In App.xaml:**

```xml
<Application xmlns:syncTheme="clr-namespace:Syncfusion.Maui.Themes;assembly=Syncfusion.Maui.Core"
             x:Class="MyApp.App">
    <Application.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <!-- Enable Liquid Glass UI -->
                <syncTheme:SyncfusionThemeResourceDictionary VisualTheme="MaterialLight"/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

**Key Property:** `EnableLiquidGlassEffect="True"`

**Result:** Card with translucent background, blur effect, subtle shadow.

### Using SfButton

```xml
<core:SfGlassEffectView CornerRadius="20"
                                        EffectType="Clear"
                                        WidthRequest="100"
                                        Grid.Column="2"
                                        HeightRequest="40">
                    <button:SfButton Text="Follow"
                                     Background="Transparent"
                                     TextColor="White" />
                </core:SfGlassEffectView>
```

**Features:**
- Glass-like semi-transparency
- Smooth press animations
- Hover effects (desktop)

## Styling Existing Controls

Make regular controls "glassy" using style properties.

### Glassy Button (Regular SfButton)

```xml
<syncfusion:SfButton Text="Click Me"
                     EnableGlassEffect="True"
                     CornerRadius="20"
                     TextColor="White"
                     Background="#80007AFF"/>
```

**Key Property:** `EnableGlassEffect="True"`

## Complete Example

**Glassy Profile Page:**

```xml
<ContentPage xmlns:syncfusion="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core">
    
    <Grid>
    <Grid.Background>
        <LinearGradientBrush StartPoint="0,0"
                             EndPoint="0,1">
            <GradientStop Color="#0F4C75"
                          Offset="0.0" />
            <GradientStop Color="#3282B8"
                          Offset="0.5" />
            <GradientStop Color="#1B262C"
                          Offset="1.0" />
        </LinearGradientBrush>
    </Grid.Background>
    <Grid>
        <core:SfGlassEffectView EffectType="Clear"
                                CornerRadius="20"
                                HeightRequest="140"
                                WidthRequest="380">
            <button:SfButton Text="Regular Glass"
                             EnableLiquidGlassEffect="True"
                             FontSize="16"
                             WidthRequest="150"
                             HeightRequest="40"
                             Background="Transparent"
                             TextColor="Black" />
        </core:SfGlassEffectView>
    </Grid>
</Grid>
    
</ContentPage>
```
**Light mode:** Light glass with dark text  
**Dark mode:** Dark glass with light text

## Performance Considerations

**Blur effects impact performance:**
- Use sparingly on low-end devices
- Test on target platforms

## Design Best Practices

✅ **Use on layered content** over images/gradients

✅ **Maintain readability** with proper contrast

✅ **Combine with shadows** for depth

✅ **Keep content hierarchy** visible

❌ **Don't overuse** glass effects (loses impact)

❌ **Avoid on complex backgrounds** (readability issues)

❌ **Don't nest too many** glass layers (performance)

## Troubleshooting

**Glass effect not visible:**
- Verify `EnableLiquidGlass="True"` in control
- Check content is over contrasting background
- Test on physical device (emulators may not render blur)

**Performance issues:**
- Limit number of glassy elements
- Use fallback backgrounds on older devices

## When to Use Liquid Glass UI

**Good Use Cases:**
- Premium applications
- Content over images
- Modern, minimalist designs
- Focus on visual hierarchy

**Not Ideal For:**
- Performance-critical apps
- Accessibility-first designs (contrast issues)

## Related Files

- **Installation:** installation-nuget.md