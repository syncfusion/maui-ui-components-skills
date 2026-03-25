# Liquid Glass Effect

This guide covers the integration of the Liquid Glass Effect with SignaturePad, enabling modern, translucent UI design with adaptive color tinting and light refraction for a premium signature capture experience.

## Table of Contents
- [Overview](#overview)
- [Platform Requirements](#platform-requirements)
- [SfGlassEffectView Overview](#sfglasseffectview-overview)
- [Implementation Steps](#implementation-steps)
- [Configuration Options](#configuration-options)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

The Liquid Glass Effect introduces a modern, translucent design aesthetic that creates a sleek, glass-like user experience. When applied to SignaturePad, it provides:

- **Translucent Background:** See-through effect with subtle blur
- **Adaptive Color Tinting:** Dynamic color adaptation based on background
- **Light Refraction:** Realistic glass-like visual properties
- **Shadow Effects:** Optional depth and elevation
- **Modern UI:** Premium, contemporary appearance

This effect is ideal for applications requiring a sophisticated, modern interface while maintaining signature readability and functionality.

## Platform Requirements

### Minimum Requirements

The Liquid Glass Effect has specific platform and version requirements:

- **.NET Version:** .NET 10 or later
- **iOS:** iOS 26 or later
- **macOS:** macOS 26 or later
- **Android:** Not currently supported
- **Windows:** Not currently supported

**Important:** This is a platform-specific feature. Check platform compatibility before implementing:

```csharp
bool isGlassEffectSupported = DeviceInfo.Platform == DevicePlatform.iOS && 
                               DeviceInfo.Version.Major >= 26 ||
                               DeviceInfo.Platform == DevicePlatform.macOS && 
                               DeviceInfo.Version.Major >= 26;
```

## SfGlassEffectView Overview

The `SfGlassEffectView` is a container control that applies the glass effect to its content.

### Key Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `EffectType` | `LiquidGlassEffectType` | `Regular` | Type of glass effect (Regular, Prominent, etc.) |
| `EnableShadowEffect` | `bool` | `false` | Enable shadow for depth |
| `CornerRadius` | `double` | `0` | Corner radius for rounded glass |
| `Content` | `View` | `null` | Child view to apply effect to |

### Namespace

```csharp
using Syncfusion.Maui.Core;
```

### NuGet Package

The glass effect is part of `Syncfusion.Maui.Core`, which is automatically installed as a dependency of `Syncfusion.Maui.SignaturePad`.

## Implementation Steps

### Step 1: Wrap SignaturePad in SfGlassEffectView

The SignaturePad must be placed inside an `SfGlassEffectView` container to apply the effect.

#### XAML Implementation

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:core="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
             xmlns:signaturePad="clr-namespace:Syncfusion.Maui.SignaturePad;assembly=Syncfusion.Maui.SignaturePad"
             x:Class="MyApp.MainPage">
    
    <Grid>
        <!-- Background image or color -->
        <Image Source="wallpaper.png" Aspect="AspectFill" />
        
        <!-- Glass effect container -->
        <core:SfGlassEffectView CornerRadius="20"
                                HeightRequest="300"
                                Margin="20"
                                EffectType="Regular"
                                EnableShadowEffect="True">
            
            <!-- SignaturePad inside glass effect -->
            <signaturePad:SfSignaturePad x:Name="signaturePad"
                                          Background="Transparent"
                                          StrokeColor="#1F2937"
                                          MinimumStrokeThickness="1"
                                          MaximumStrokeThickness="4" />
        </core:SfGlassEffectView>
    </Grid>
    
</ContentPage>
```

#### C# Implementation

```csharp
using Syncfusion.Maui.Core;
using Syncfusion.Maui.SignaturePad;

public class GlassSignaturePage : ContentPage
{
    public GlassSignaturePage()
    {
        // Background image
        var backgroundImage = new Image
        {
            Source = "wallpaper.png",
            Aspect = Aspect.AspectFill
        };
        
        // Create SignaturePad
        var signaturePad = new SfSignaturePad
        {
            Background = Colors.Transparent,
            StrokeColor = Color.FromArgb("#1F2937"),
            MinimumStrokeThickness = 1,
            MaximumStrokeThickness = 4,
            HorizontalOptions = LayoutOptions.Fill,
            VerticalOptions = LayoutOptions.Fill
        };
        
        // Wrap in glass effect view
        var glassEffectView = new SfGlassEffectView
        {
            CornerRadius = 20,
            HeightRequest = 300,
            Margin = 20,
            EffectType = LiquidGlassEffectType.Regular,
            EnableShadowEffect = true,
            Content = signaturePad
        };
        
        // Layout
        var grid = new Grid();
        grid.Children.Add(backgroundImage);
        grid.Children.Add(glassEffectView);
        
        Content = grid;
    }
}
```

### Step 2: Set SignaturePad Background to Transparent

**Critical:** The SignaturePad's `Background` property must be set to `Transparent` for the glass effect to work properly.

```xml
<!-- XAML -->
<signaturePad:SfSignaturePad Background="Transparent" />
```

```csharp
// C#
signaturePad.Background = Colors.Transparent;
```

Without transparency, the glass effect will be obscured by the SignaturePad's solid background.

### Step 3: Configure Glass Effect Properties

Customize the appearance with various properties:

```xml
<core:SfGlassEffectView CornerRadius="20"
                        EffectType="Regular"
                        EnableShadowEffect="True"
                        HeightRequest="300"
                        Margin="20">
    <signaturePad:SfSignaturePad Background="Transparent"
                                  StrokeColor="#1F2937" />
</core:SfGlassEffectView>
```

## Configuration Options

### EffectType Property

Controls the intensity and style of the glass effect.

```csharp
public enum LiquidGlassEffectType
{
    Regular,    // Standard glass effect
    Prominent   // More pronounced effect (if available)
}
```

#### Examples

```xml
<!-- Regular effect (subtle) -->
<core:SfGlassEffectView EffectType="Regular">
    <signaturePad:SfSignaturePad Background="Transparent" />
</core:SfGlassEffectView>

<!-- Prominent effect (stronger) -->
<core:SfGlassEffectView EffectType="Prominent">
    <signaturePad:SfSignaturePad Background="Transparent" />
</core:SfGlassEffectView>
```

```csharp
// C#
glassEffectView.EffectType = LiquidGlassEffectType.Regular;
// or
glassEffectView.EffectType = LiquidGlassEffectType.Prominent;
```

### EnableShadowEffect Property

Adds depth and elevation with shadow.

```xml
<!-- With shadow -->
<core:SfGlassEffectView EnableShadowEffect="True">
    <signaturePad:SfSignaturePad Background="Transparent" />
</core:SfGlassEffectView>

<!-- Without shadow -->
<core:SfGlassEffectView EnableShadowEffect="False">
    <signaturePad:SfSignaturePad Background="Transparent" />
</core:SfGlassEffectView>
```

```csharp
// C#
glassEffectView.EnableShadowEffect = true;  // With shadow
glassEffectView.EnableShadowEffect = false; // Without shadow
```

### CornerRadius Property

Rounds the corners of the glass container.

```xml
<!-- Slightly rounded -->
<core:SfGlassEffectView CornerRadius="10">
    <signaturePad:SfSignaturePad Background="Transparent" />
</core:SfGlassEffectView>

<!-- Very rounded -->
<core:SfGlassEffectView CornerRadius="30">
    <signaturePad:SfSignaturePad Background="Transparent" />
</core:SfGlassEffectView>

<!-- Fully rounded (pill shape) -->
<core:SfGlassEffectView CornerRadius="150" HeightRequest="300">
    <signaturePad:SfSignaturePad Background="Transparent" />
</core:SfGlassEffectView>
```

```csharp
// C#
glassEffectView.CornerRadius = 20; // Rounded corners
```

### Stroke Color Considerations

Choose stroke colors that provide good contrast against the translucent background:

```csharp
// Dark stroke for light backgrounds
signaturePad.StrokeColor = Color.FromArgb("#1F2937"); // Dark gray

// Light stroke for dark backgrounds
signaturePad.StrokeColor = Colors.White;

// High contrast
signaturePad.StrokeColor = Colors.Black;
```

## Complete Examples

### Example 1: Modern Signature Card

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:core="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
             xmlns:signaturePad="clr-namespace:Syncfusion.Maui.SignaturePad;assembly=Syncfusion.Maui.SignaturePad">
    
    <Grid Padding="20">
        <!-- Gradient background -->
        <Grid.Background>
            <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
                <GradientStop Color="#667eea" Offset="0.0" />
                <GradientStop Color="#764ba2" Offset="1.0" />
            </LinearGradientBrush>
        </Grid.Background>
        
        <VerticalStackLayout Spacing="20" VerticalOptions="Center">
            <!-- Title -->
            <Label Text="Please Sign Below"
                   FontSize="24"
                   FontAttributes="Bold"
                   TextColor="White"
                   HorizontalOptions="Center" />
            
            <!-- Glass signature pad -->
            <core:SfGlassEffectView CornerRadius="25"
                                    HeightRequest="300"
                                    EffectType="Regular"
                                    EnableShadowEffect="True">
                <signaturePad:SfSignaturePad x:Name="signaturePad"
                                              Background="Transparent"
                                              StrokeColor="White"
                                              MinimumStrokeThickness="2"
                                              MaximumStrokeThickness="5" />
            </core:SfGlassEffectView>
            
            <!-- Action buttons -->
            <HorizontalStackLayout Spacing="15" HorizontalOptions="Center">
                <Button Text="Clear"
                        BackgroundColor="Transparent"
                        TextColor="White"
                        BorderColor="White"
                        BorderWidth="2"
                        CornerRadius="20"
                        Padding="30,10"
                        Clicked="OnClearClicked" />
                <Button Text="Save"
                        BackgroundColor="White"
                        TextColor="#667eea"
                        CornerRadius="20"
                        Padding="30,10"
                        Clicked="OnSaveClicked" />
            </HorizontalStackLayout>
        </VerticalStackLayout>
    </Grid>
    
</ContentPage>
```

### Example 2: Dynamic Platform Check

```csharp
using Syncfusion.Maui.Core;
using Syncfusion.Maui.SignaturePad;

public class AdaptiveSignaturePage : ContentPage
{
    public AdaptiveSignaturePage()
    {
        Content = CreateSignatureUI();
    }
    
    private View CreateSignatureUI()
    {
        var signaturePad = new SfSignaturePad
        {
            Background = Colors.Transparent,
            StrokeColor = Color.FromArgb("#1F2937"),
            MinimumStrokeThickness = 1,
            MaximumStrokeThickness = 4
        };
        
        // Check if glass effect is supported
        bool supportsGlassEffect = DeviceInfo.Platform == DevicePlatform.iOS ||
                                   DeviceInfo.Platform == DevicePlatform.macOS;
        
        if (supportsGlassEffect)
        {
            // Use glass effect on supported platforms
            return CreateGlassEffectLayout(signaturePad);
        }
        else
        {
            // Fallback to standard layout
            return CreateStandardLayout(signaturePad);
        }
    }
    
    private View CreateGlassEffectLayout(SfSignaturePad signaturePad)
    {
        var glassView = new SfGlassEffectView
        {
            CornerRadius = 20,
            HeightRequest = 300,
            Margin = 20,
            EffectType = LiquidGlassEffectType.Regular,
            EnableShadowEffect = true,
            Content = signaturePad
        };
        
        var grid = new Grid
        {
            Children = 
            {
                new Image { Source = "background.png", Aspect = Aspect.AspectFill },
                glassView
            }
        };
        
        return grid;
    }
    
    private View CreateStandardLayout(SfSignaturePad signaturePad)
    {
        signaturePad.Background = Colors.White;
        
        var border = new Border
        {
            Stroke = Colors.Gray,
            StrokeThickness = 2,
            Padding = 10,
            Margin = 20,
            Content = signaturePad
        };
        
        return border;
    }
}
```

### Example 3: Multiple Effect Styles

```xml
<ScrollView>
    <VerticalStackLayout Spacing="30" Padding="20">
        
        <!-- Regular effect with shadow -->
        <VerticalStackLayout Spacing="10">
            <Label Text="Regular with Shadow" FontAttributes="Bold" />
            <core:SfGlassEffectView CornerRadius="20"
                                    HeightRequest="200"
                                    EffectType="Regular"
                                    EnableShadowEffect="True">
                <signaturePad:SfSignaturePad Background="Transparent"
                                              StrokeColor="#1F2937" />
            </core:SfGlassEffectView>
        </VerticalStackLayout>
        
        <!-- Regular effect without shadow -->
        <VerticalStackLayout Spacing="10">
            <Label Text="Regular without Shadow" FontAttributes="Bold" />
            <core:SfGlassEffectView CornerRadius="20"
                                    HeightRequest="200"
                                    EffectType="Regular"
                                    EnableShadowEffect="False">
                <signaturePad:SfSignaturePad Background="Transparent"
                                              StrokeColor="#1F2937" />
            </core:SfGlassEffectView>
        </VerticalStackLayout>
        
        <!-- Prominent effect -->
        <VerticalStackLayout Spacing="10">
            <Label Text="Prominent Effect" FontAttributes="Bold" />
            <core:SfGlassEffectView CornerRadius="20"
                                    HeightRequest="200"
                                    EffectType="Prominent"
                                    EnableShadowEffect="True">
                <signaturePad:SfSignaturePad Background="Transparent"
                                              StrokeColor="#1F2937" />
            </core:SfGlassEffectView>
        </VerticalStackLayout>
        
    </VerticalStackLayout>
</ScrollView>
```

## Best Practices

### 1. Always Use Transparent Background

```csharp
// ✅ Correct
signaturePad.Background = Colors.Transparent;

// ❌ Incorrect - will hide glass effect
signaturePad.Background = Colors.White;
```

### 2. Choose Appropriate Stroke Colors

```csharp
// For light/colorful backgrounds
signaturePad.StrokeColor = Color.FromArgb("#1F2937"); // Dark gray/black

// For dark backgrounds
signaturePad.StrokeColor = Colors.White;
```

### 3. Platform Detection

```csharp
// Check platform before applying glass effect
if (DeviceInfo.Platform == DevicePlatform.iOS || 
    DeviceInfo.Platform == DevicePlatform.macOS)
{
    // Use glass effect
}
else
{
    // Use alternative styling
}
```

### 4. Performance Considerations

- Glass effects may impact performance on older devices
- Test on target devices to ensure smooth signature capture
- Consider providing a "Reduce motion" or "Simplified UI" option

### 5. Accessibility

- Ensure sufficient contrast between stroke color and background
- Provide alternative themes for users with visual impairments
- Test with accessibility features enabled

## Troubleshooting

### Issue: Glass Effect Not Visible

**Causes:**
- SignaturePad background is not transparent
- Platform not supported (Android/Windows)
- .NET version < 10

**Solution:**
```csharp
// 1. Set background to transparent
signaturePad.Background = Colors.Transparent;

// 2. Check platform support
bool isSupported = DeviceInfo.Platform == DevicePlatform.iOS ||
                   DeviceInfo.Platform == DevicePlatform.macOS;

// 3. Verify .NET version in project file
// <TargetFramework>net10.0-ios</TargetFramework>
```

### Issue: Signature Strokes Not Visible

**Cause:** Stroke color blends with background

**Solution:**
```csharp
// Use high-contrast stroke color
signaturePad.StrokeColor = Colors.Black; // or Colors.White
signaturePad.MinimumStrokeThickness = 2; // Increase thickness
```

### Issue: Poor Performance

**Cause:** Glass effect rendering overhead

**Solution:**
```csharp
// Disable shadow effect
glassEffectView.EnableShadowEffect = false;

// Or provide fallback for older devices
if (DeviceInfo.Version.Major < 26)
{
    // Use standard layout without glass effect
}
```

### Issue: Layout Issues

**Cause:** Incorrect sizing or positioning

**Solution:**
```xml
<!-- Ensure proper sizing -->
<core:SfGlassEffectView HeightRequest="300"
                        HorizontalOptions="Fill"
                        VerticalOptions="Fill">
    <signaturePad:SfSignaturePad HorizontalOptions="Fill"
                                  VerticalOptions="Fill" />
</core:SfGlassEffectView>
```

## Platform-Specific Notes

### iOS 26+
- Full support for all glass effect features
- Best performance on A12 Bionic or later
- Supports both light and dark mode adaptations

### macOS 26+
- Full support with native macOS styling
- Integrates well with macOS Sonoma design language
- Supports theme-aware rendering

### Android
- Not currently supported
- Use standard Border or Frame as alternative
- Consider themed backgrounds for visual appeal

### Windows
- Not currently supported
- Use Acrylic or standard styling as alternative
- Consider mica material for Windows 11 applications
