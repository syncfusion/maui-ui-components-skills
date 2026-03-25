# Liquid Glass Effect in .NET MAUI Numeric Entry

This guide covers how to implement the modern Liquid Glass Effect for the Numeric Entry control, creating a sleek translucent design with adaptive color tinting and light refraction.

## Overview

The **Liquid Glass Effect** introduces a modern, translucent design that creates a glass-like appearance with:
- Adaptive color tinting
- Light refraction effects
- Translucent background
- Shadow effects
- Platform-native blur

**Platform Requirements:**
- ⚠️ **.NET 10** or later
- ⚠️ **iOS 26** or later
- ⚠️ **macOS 26** or later

**Note:** This feature is NOT available on Android or Windows platforms.

## Apply Liquid Glass Effect

Follow these steps to enable the Liquid Glass Effect for Numeric Entry:

### Step 1: Wrap Control in SfGlassEffectView

The Numeric Entry must be wrapped inside `SfGlassEffectView` to apply the glass effect.

```xml
<core:SfGlassEffectView>
    <editors:SfNumericEntry Value="1234.56"
                            CustomFormat="C2"
                            Background="Transparent" />
</core:SfGlassEffectView>
```

**Important:** 
- Add namespace: `xmlns:core="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"`
- The `SfGlassEffectView` is the container that applies the glass effect

### Step 2: Set Background to Transparent

For the glass effect to work properly, the Numeric Entry's `Background` must be set to `Transparent`.

```xml
<editors:SfNumericEntry Background="Transparent" />
```

**Why:** The transparent background allows the glass effect to show through, creating the tinted translucent appearance.

### Step 3: Configure Glass Effect Properties

Customize the glass effect using `SfGlassEffectView` properties:

| Property | Type | Description |
|----------|------|-------------|
| `EffectType` | LiquidGlassEffectType | Regular, Thick, Thin |
| `EnableShadowEffect` | bool | Enable/disable shadow |
| `CornerRadius` | double | Rounded corners |
| `HeightRequest` | double | Container height |

## Complete Implementation

### XAML Implementation

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:core="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
             xmlns:editors="clr-namespace:Syncfusion.Maui.Inputs;assembly=Syncfusion.Maui.Inputs"
             x:Class="NumericEntryApp.LiquidGlassPage">
    
    <Grid>
        <!-- Background Image for Glass Effect -->
        <Image Source="background.png"
               Aspect="AspectFill" />
        
        <!-- Numeric Entry with Glass Effect -->
        <core:SfGlassEffectView CornerRadius="20"
                                HeightRequest="40"
                                EffectType="Regular"
                                EnableShadowEffect="True"
                                VerticalOptions="Center"
                                Margin="20">
            
            <editors:SfNumericEntry Value="1234.56"
                                    CustomFormat="C2"
                                    Placeholder="Enter amount"
                                    Maximum="1000000"
                                    Minimum="0"
                                    Background="Transparent"
                                    ShowClearButton="True" />
            
        </core:SfGlassEffectView>
        
    </Grid>
</ContentPage>
```

### C# Implementation

```csharp
using Syncfusion.Maui.Core;
using Syncfusion.Maui.Inputs;

namespace NumericEntryApp;

public partial class LiquidGlassPage : ContentPage
{
    public LiquidGlassPage()
    {
        InitializeComponent();
        
        // Alternative: Create programmatically
        var grid = new Grid
        {
            BackgroundColor = Colors.Transparent
        };
        
        // Background image
        var image = new Image
        {
            Source = "background.png",
            Aspect = Aspect.AspectFill
        };
        grid.Children.Add(image);
        
        // Glass effect container
        var glassEffect = new SfGlassEffectView
        {
            CornerRadius = 20,
            HeightRequest = 40,
            EffectType = LiquidGlassEffectType.Regular,
            EnableShadowEffect = true,
            VerticalOptions = LayoutOptions.Center,
            Margin = new Thickness(20)
        };
        
        // Numeric Entry
        var numericEntry = new SfNumericEntry
        {
            Value = 1234.56,
            CustomFormat = "C2",
            Placeholder = "Enter amount",
            Maximum = 1_000_000,
            Minimum = 0,
            Background = Colors.Transparent,
            ShowClearButton = true
        };
        
        glassEffect.Content = numericEntry;
        grid.Children.Add(glassEffect);
        
        Content = grid;
    }
}
```

## Effect Types

The `EffectType` property controls the intensity of the glass blur effect.

### Regular (Default)

Standard glass effect with moderate blur.

```xml
<core:SfGlassEffectView EffectType="Regular">
    <editors:SfNumericEntry Background="Transparent" />
</core:SfGlassEffectView>
```

**Use case:** General purpose, balanced appearance

### Thick

Stronger blur effect, more opaque appearance.

```xml
<core:SfGlassEffectView EffectType="Thick">
    <editors:SfNumericEntry Background="Transparent" />
</core:SfGlassEffectView>
```

**Use case:** High contrast backgrounds, important fields

### Thin

Lighter blur effect, more transparent appearance.

```xml
<core:SfGlassEffectView EffectType="Thin">
    <editors:SfNumericEntry Background="Transparent" />
</core:SfGlassEffectView>
```

**Use case:** Subtle effects, minimal backgrounds

## Shadow Effect

The `EnableShadowEffect` property adds depth with a shadow beneath the control.

### With Shadow (Recommended)

```xml
<core:SfGlassEffectView EnableShadowEffect="True">
    <editors:SfNumericEntry Background="Transparent" />
</core:SfGlassEffectView>
```

**Behavior:** Adds soft shadow for depth and elevation

### Without Shadow

```xml
<core:SfGlassEffectView EnableShadowEffect="False">
    <editors:SfNumericEntry Background="Transparent" />
</core:SfGlassEffectView>
```

**Behavior:** Flat appearance, no shadow

## Corner Radius

Customize rounded corners with the `CornerRadius` property.

```xml
<!-- Sharp corners -->
<core:SfGlassEffectView CornerRadius="0">
    <editors:SfNumericEntry Background="Transparent" />
</core:SfGlassEffectView>

<!-- Slightly rounded -->
<core:SfGlassEffectView CornerRadius="10">
    <editors:SfNumericEntry Background="Transparent" />
</core:SfGlassEffectView>

<!-- Very rounded -->
<core:SfGlassEffectView CornerRadius="25">
    <editors:SfNumericEntry Background="Transparent" />
</core:SfGlassEffectView>

<!-- Pill shape -->
<core:SfGlassEffectView CornerRadius="50">
    <editors:SfNumericEntry Background="Transparent" />
</core:SfGlassEffectView>
```

**Recommendation:** Use 15-25 for modern, approachable design

## Multiple Numeric Entry with Glass Effect

### Vertical Stack

```xml
<Grid>
    <Image Source="background.png" Aspect="AspectFill" />
    
    <VerticalStackLayout Padding="20"
                         Spacing="15"
                         VerticalOptions="Center">
        
        <!-- Price -->
        <core:SfGlassEffectView CornerRadius="20"
                                HeightRequest="50"
                                EffectType="Regular"
                                EnableShadowEffect="True">
            <editors:SfNumericEntry Value="99.99"
                                    CustomFormat="C2"
                                    Placeholder="Price"
                                    Background="Transparent" />
        </core:SfGlassEffectView>
        
        <!-- Quantity -->
        <core:SfGlassEffectView CornerRadius="20"
                                HeightRequest="50"
                                EffectType="Regular"
                                EnableShadowEffect="True">
            <editors:SfNumericEntry Value="1"
                                    CustomFormat="N0"
                                    Placeholder="Quantity"
                                    Background="Transparent" />
        </core:SfGlassEffectView>
        
        <!-- Discount -->
        <core:SfGlassEffectView CornerRadius="20"
                                HeightRequest="50"
                                EffectType="Regular"
                                EnableShadowEffect="True">
            <editors:SfNumericEntry Value="10"
                                    CustomFormat="N0'%'"
                                    Placeholder="Discount"
                                    Background="Transparent" />
        </core:SfGlassEffectView>
        
    </VerticalStackLayout>
</Grid>
```

### Form Layout

```xml
<Grid>
    <Image Source="background.png" Aspect="AspectFill" />
    
    <ScrollView>
        <VerticalStackLayout Padding="20" Spacing="20">
            
            <Label Text="Order Details"
                   FontSize="24"
                   FontAttributes="Bold"
                   TextColor="White"
                   Margin="0,0,0,10" />
            
            <!-- Product Price -->
            <StackLayout Spacing="5">
                <Label Text="Product Price:"
                       TextColor="White"
                       FontAttributes="Bold" />
                <core:SfGlassEffectView CornerRadius="15"
                                        HeightRequest="45"
                                        EffectType="Regular"
                                        EnableShadowEffect="True">
                    <editors:SfNumericEntry Value="299.99"
                                            CustomFormat="C2"
                                            Minimum="0.01"
                                            AllowNull="False"
                                            Background="Transparent"
                                            ShowClearButton="True" />
                </core:SfGlassEffectView>
            </StackLayout>
            
            <!-- Quantity -->
            <StackLayout Spacing="5">
                <Label Text="Quantity:"
                       TextColor="White"
                       FontAttributes="Bold" />
                <core:SfGlassEffectView CornerRadius="15"
                                        HeightRequest="45"
                                        EffectType="Regular"
                                        EnableShadowEffect="True">
                    <editors:SfNumericEntry Value="1"
                                            CustomFormat="N0"
                                            Minimum="1"
                                            AllowNull="False"
                                            Background="Transparent"
                                            UpDownPlacementMode="Inline" />
                </core:SfGlassEffectView>
            </StackLayout>
            
            <!-- Discount -->
            <StackLayout Spacing="5">
                <Label Text="Discount %:"
                       TextColor="White"
                       FontAttributes="Bold" />
                <core:SfGlassEffectView CornerRadius="15"
                                        HeightRequest="45"
                                        EffectType="Regular"
                                        EnableShadowEffect="True">
                    <editors:SfNumericEntry CustomFormat="N0'%'"
                                            Minimum="0"
                                            Maximum="100"
                                            AllowNull="True"
                                            Placeholder="No discount"
                                            Background="Transparent" />
                </core:SfGlassEffectView>
            </StackLayout>
            
        </VerticalStackLayout>
    </ScrollView>
</Grid>
```

## Best Practices

### 1. Choose Appropriate Background

**Good backgrounds:**
- High-quality images
- Gradients
- Subtle patterns
- Scenic photos

**Poor backgrounds:**
- Solid colors (effect not visible)
- Low contrast images
- Busy, complex patterns

### 2. Set Background to Transparent

Always set `Background="Transparent"` on the Numeric Entry:

```xml
<editors:SfNumericEntry Background="Transparent" />
```

### 3. Use Consistent Corner Radius

Maintain consistent `CornerRadius` across all glass controls in the same view:

```xml
<!-- All controls use CornerRadius="20" -->
<core:SfGlassEffectView CornerRadius="20">...</core:SfGlassEffectView>
<core:SfGlassEffectView CornerRadius="20">...</core:SfGlassEffectView>
<core:SfGlassEffectView CornerRadius="20">...</core:SfGlassEffectView>
```

### 4. Enable Shadow for Depth

Use `EnableShadowEffect="True"` to add visual depth:

```xml
<core:SfGlassEffectView EnableShadowEffect="True">
    ...
</core:SfGlassEffectView>
```

### 5. Adjust Height Appropriately

Set `HeightRequest` to accommodate the Numeric Entry:

```xml
<!-- Standard height -->
<core:SfGlassEffectView HeightRequest="40">
    ...
</core:SfGlassEffectView>

<!-- Larger for better touch targets -->
<core:SfGlassEffectView HeightRequest="50">
    ...
</core:SfGlassEffectView>
```

### 6. Consider Text Contrast

Ensure text remains readable against the background:

```xml
<editors:SfNumericEntry Background="Transparent"
                        TextColor="White"  <!-- Adjust for background -->
                        PlaceholderColor="LightGray" />
```

## Platform Availability

### Supported Platforms

✅ **iOS 26+** - Full support
✅ **macOS 26+** - Full support

### Unsupported Platforms

❌ **Android** - Not available
❌ **Windows** - Not available
❌ **iOS < 26** - Not available
❌ **macOS < 26** - Not available

### Platform Detection

Check platform support before applying glass effect:

```csharp
public bool IsGlassEffectSupported()
{
    #if IOS || MACCATALYST
        if (DeviceInfo.Version.Major >= 26)
        {
            return true;
        }
    #endif
    return false;
}
```

**Fallback for unsupported platforms:**

```csharp
if (IsGlassEffectSupported())
{
    // Use SfGlassEffectView
    var glassEffect = new SfGlassEffectView
    {
        Content = numericEntry
    };
}
else
{
    // Use standard styling
    numericEntry.Background = Colors.White;
    numericEntry.Stroke = Colors.Gray;
}
```

## Troubleshooting

### Glass Effect Not Visible

**Problem:** Glass effect doesn't appear

**Solutions:**
1. Verify platform requirements (.NET 10, iOS/macOS 26+)
2. Check `Background="Transparent"` is set on Numeric Entry
3. Ensure background image/content exists behind the control
4. Try different `EffectType` values

### Text Not Readable

**Problem:** Text is hard to read against background

**Solutions:**
1. Adjust `TextColor` property
2. Use darker or lighter background image
3. Change `EffectType` to `Thick` for more opacity
4. Add text shadow or outline (if supported)

### Performance Issues

**Problem:** UI feels sluggish with glass effects

**Solutions:**
1. Limit number of glass effect controls
2. Use simpler background images
3. Reduce `CornerRadius` complexity
4. Disable shadow on less important controls

## Summary

This reference covered:
- ✅ Liquid Glass Effect overview and requirements
- ✅ Wrapping Numeric Entry in SfGlassEffectView
- ✅ Setting transparent background
- ✅ Effect types (Regular, Thick, Thin)
- ✅ Shadow effects and corner radius
- ✅ Multiple controls with glass effect
- ✅ Best practices and platform availability
- ✅ Troubleshooting common issues

**Platform Support:** .NET 10 with iOS 26+ or macOS 26+ only

---

**End of Numeric Entry Reference Documentation**
