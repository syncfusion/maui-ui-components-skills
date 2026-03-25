# Stroke Customization

The SignaturePad control provides comprehensive stroke customization options to create realistic, handwritten signatures with varying appearance. This guide covers all stroke-related properties and best practices for achieving the desired visual effect.

## Table of Contents
- [Overview](#overview)
- [StrokeColor Property](#strokecolor-property)
- [Stroke Thickness: Dynamic vs Static](#stroke-thickness-dynamic-vs-static)
- [MinimumStrokeThickness and MaximumStrokeThickness](#minimumstrokethickness-and-maximumstrokethickness)
- [Background Property](#background-property)
- [Realistic Handwriting Algorithm](#realistic-handwriting-algorithm)
- [Best Practices](#best-practices)
- [Complete Examples](#complete-examples)

## Overview

SignaturePad offers two approaches to stroke customization:

1. **Dynamic Thickness (Realistic):** Use `MinimumStrokeThickness` and `MaximumStrokeThickness` for speed-based, realistic handwriting

Choose the approach based on your use case: realistic signatures vs. uniform drawing.

## StrokeColor Property

The `StrokeColor` property controls the color of signature strokes.

### Property Details
- **Type:** `Color`
- **Default:** `Colors.Black`
- **Supports:** All .NET MAUI color formats (named colors, hex, RGB, RGBA)

### XAML Example

```xml
<!-- Using named colors -->
<signaturePad:SfSignaturePad StrokeColor="Blue" />

<!-- Using hex colors -->
<signaturePad:SfSignaturePad StrokeColor="#FF5722" />

<!-- Using RGBA -->
<signaturePad:SfSignaturePad StrokeColor="#80FF5722" />
```

### C# Example

```csharp
// Using named colors
signaturePad.StrokeColor = Colors.Blue;

// Using hex string
signaturePad.StrokeColor = Color.FromArgb("#FF5722");

// Using RGB
signaturePad.StrokeColor = Color.FromRgb(255, 87, 34);

// Using RGBA (with transparency)
signaturePad.StrokeColor = Color.FromRgba(255, 87, 34, 0.5);
```

### Common Color Choices

```csharp
// Professional signatures
signaturePad.StrokeColor = Colors.Black;        // Classic
signaturePad.StrokeColor = Colors.DarkBlue;     // Formal
signaturePad.StrokeColor = Color.FromArgb("#1A237E"); // Deep blue

// Branded signatures
signaturePad.StrokeColor = Color.FromArgb("#FF5722"); // Brand color

// Light mode vs Dark mode
signaturePad.StrokeColor = Application.Current.RequestedTheme == AppTheme.Dark
    ? Colors.White
    : Colors.Black;
```

## Stroke Thickness: Dynamic vs Static

### Dynamic Thickness (Recommended for Signatures)

Uses `MinimumStrokeThickness` and `MaximumStrokeThickness` to create realistic, speed-based rendering:
- **Slow drawing** → Thicker strokes (up to maximum)
- **Fast drawing** → Thinner strokes (down to minimum)

**Use when:** Creating realistic handwritten signatures with natural variation.


## MinimumStrokeThickness and MaximumStrokeThickness

These properties work together to create the realistic handwriting algorithm.

### Property Details
- **Type:** `double`
- **Default Minimum:** `1`
- **Default Maximum:** `3`
- **Units:** Device-independent units (similar to pixels)
- **Behavior:** Dynamically adjusts based on drawing speed and pressure

### How It Works

The SignaturePad rendering algorithm:
1. Monitors the speed of drawing gestures
2. Calculates stroke thickness in real-time
3. **Slow/careful strokes** → Thickness approaches `MaximumStrokeThickness`
4. **Fast/quick strokes** → Thickness approaches `MinimumStrokeThickness`
5. Smooth transitions between thickness values

This creates a natural, handwritten appearance similar to pen-on-paper.

### XAML Example

```xml
<!-- Fine, precise signatures -->
<signaturePad:SfSignaturePad MinimumStrokeThickness="0.5"
                              MaximumStrokeThickness="2" />

<!-- Medium signatures (default-like) -->
<signaturePad:SfSignaturePad MinimumStrokeThickness="1"
                              MaximumStrokeThickness="4" />

<!-- Bold signatures -->
<signaturePad:SfSignaturePad MinimumStrokeThickness="2"
                              MaximumStrokeThickness="6" />

<!-- High variation for artistic effect -->
<signaturePad:SfSignaturePad MinimumStrokeThickness="0.5"
                              MaximumStrokeThickness="8" />
```

### C# Example

```csharp
// Fine signatures
var fineSig = new SfSignaturePad
{
    MinimumStrokeThickness = 0.5,
    MaximumStrokeThickness = 2
};

// Bold signatures
var boldSig = new SfSignaturePad
{
    MinimumStrokeThickness = 2,
    MaximumStrokeThickness = 6
};

// Dynamic adjustment based on device
var signaturePad = new SfSignaturePad
{
    MinimumStrokeThickness = DeviceInfo.Platform == DevicePlatform.Android ? 1 : 0.8,
    MaximumStrokeThickness = DeviceInfo.Platform == DevicePlatform.Android ? 5 : 4
};
```

### Recommended Ranges

| Use Case | Min Thickness | Max Thickness | Notes |
|----------|---------------|---------------|-------|
| **Fine signatures** | 0.5 - 1 | 2 - 3 | Detailed, professional |
| **Standard signatures** | 1 - 1.5 | 3 - 5 | Balanced, readable |
| **Bold signatures** | 2 - 3 | 5 - 8 | Prominent, high visibility |
| **Artistic/calligraphy** | 0.5 | 8 - 10 | Maximum variation |
| **Minimal variation** | 2 | 2.5 | Subtle dynamic effect |

### Tips for Best Results

```csharp
// Maintain a ratio between min and max
// Good: 1:3 or 1:4 ratio
signaturePad.MinimumStrokeThickness = 1;
signaturePad.MaximumStrokeThickness = 4;  // 1:4 ratio

// Avoid: Too close (minimal variation)
// Bad example:
// MinimumStrokeThickness = 2
// MaximumStrokeThickness = 2.2  // Only 0.2 difference

// Avoid: Too wide (may look inconsistent)
// Bad example:
// MinimumStrokeThickness = 0.5
// MaximumStrokeThickness = 15  // 1:30 ratio - too extreme
```
## Background Property

Control the background color or transparency of the SignaturePad.

### Property Details
- **Type:** `Brush`
- **Default:** `Colors.White`
- **Supports:** Solid colors, gradients, transparent

### XAML Example

```xml
<!-- White background (default) -->
<signaturePad:SfSignaturePad Background="White" />

<!-- Light gray background -->
<signaturePad:SfSignaturePad Background="#F5F5F5" />

<!-- Transparent (for overlays or glass effects) -->
<signaturePad:SfSignaturePad Background="Transparent" />
```

### C# Example

```csharp
// Solid color background
signaturePad.Background = Colors.White;

// Transparent for effects
signaturePad.Background = Colors.Transparent;

// Themed background
signaturePad.Background = Application.Current.RequestedTheme == AppTheme.Dark
    ? Color.FromArgb("#1E1E1E")
    : Colors.White;
```

### Use Cases

```csharp
// Overlay on image
signaturePad.Background = Colors.Transparent;

// High contrast for visibility
signaturePad.Background = Colors.White;
signaturePad.StrokeColor = Colors.Black;

// Paper-like appearance
signaturePad.Background = Color.FromArgb("#FFFEF5E7");  // Cream color
signaturePad.StrokeColor = Color.FromArgb("#1A237E");   // Dark blue
```

## Realistic Handwriting Algorithm

The SignaturePad uses a sophisticated algorithm to create realistic handwriting:

### How It Works

1. **Speed Detection:** Monitors the velocity of drawing gestures
2. **Thickness Calculation:** 
   - Fast strokes → Thinner (approaching minimum)
   - Slow strokes → Thicker (approaching maximum)
3. **Smooth Interpolation:** Gradual transitions between thickness values
4. **Pressure Simulation:** Mimics pen pressure based on gesture speed
5. **Anti-Aliasing:** Smooth, professional stroke rendering

### Visual Effect

```
Fast stroke:    ────────────── (thin, approaching MinimumStrokeThickness)
Medium stroke:  ━━━━━━━━━━━━━━ (medium thickness)
Slow stroke:    ██████████████ (thick, approaching MaximumStrokeThickness)
Natural sig:    ───━━━━███━━── (varies based on speed changes)
```

### Factors Affecting Appearance

- **Drawing speed:** Faster = thinner, Slower = thicker
- **Min/Max range:** Wider range = more dramatic variation
- **Input device:** Stylus vs finger vs mouse affects feel
- **Device performance:** Faster devices render more smoothly

## Best Practices

### Professional Signatures

```csharp
var professionalSignature = new SfSignaturePad
{
    StrokeColor = Colors.Black,
    MinimumStrokeThickness = 1,
    MaximumStrokeThickness = 3.5,
    Background = Colors.White
};
```

### High Visibility Signatures

```csharp
var boldSignature = new SfSignaturePad
{
    StrokeColor = Colors.DarkBlue,
    MinimumStrokeThickness = 2,
    MaximumStrokeThickness = 6,
    Background = Colors.White
};
```

### Themed Signatures (Light/Dark Mode)

```csharp
bool isDarkMode = Application.Current.RequestedTheme == AppTheme.Dark;

var themedSignature = new SfSignaturePad
{
    StrokeColor = isDarkMode ? Colors.White : Colors.Black,
    Background = isDarkMode ? Color.FromArgb("#1E1E1E") : Colors.White,
    MinimumStrokeThickness = 1,
    MaximumStrokeThickness = 4
};
```

### Transparent Overlay

```csharp
var overlaySignature = new SfSignaturePad
{
    Background = Colors.Transparent,
    StrokeColor = Colors.Red,
    MinimumStrokeThickness = 1.5,
    MaximumStrokeThickness = 4
};
```

## Complete Examples

### Example 1: Classic Black Signature

```xml
<signaturePad:SfSignaturePad x:Name="classicSignature"
                              StrokeColor="Black"
                              MinimumStrokeThickness="1"
                              MaximumStrokeThickness="3"
                              Background="White"
                              HeightRequest="200" />
```

### Example 2: Blue Corporate Signature

```csharp
var corporateSignature = new SfSignaturePad
{
    StrokeColor = Color.FromArgb("#1565C0"),  // Corporate blue
    MinimumStrokeThickness = 1.2,
    MaximumStrokeThickness = 4,
    Background = Colors.White,
    HeightRequest = 200
};
```

### Example 3: Dynamic Theme-Aware Signature

```csharp
public class ThemedSignaturePage : ContentPage
{
    private SfSignaturePad signaturePad;

    public ThemedSignaturePage()
    {
        signaturePad = new SfSignaturePad
        {
            HeightRequest = 200
        };

        UpdateTheme();

        // Listen for theme changes
        Application.Current.RequestedThemeChanged += (s, e) => UpdateTheme();

        Content = signaturePad;
    }

    private void UpdateTheme()
    {
        bool isDark = Application.Current.RequestedTheme == AppTheme.Dark;

        signaturePad.StrokeColor = isDark ? Colors.White : Colors.Black;
        signaturePad.Background = isDark 
            ? Color.FromArgb("#2C2C2C") 
            : Color.FromArgb("#FAFAFA");
    }
}
```

### Example 4: Multiple Signature Styles

```xml
<VerticalStackLayout Spacing="20" Padding="20">
    
    <!-- Fine Signature -->
    <Label Text="Fine Signature" FontAttributes="Bold" />
    <Border Stroke="Gray" StrokeThickness="1">
        <signaturePad:SfSignaturePad MinimumStrokeThickness="0.5"
                                      MaximumStrokeThickness="2"
                                      HeightRequest="150" />
    </Border>
    
    <!-- Standard Signature -->
    <Label Text="Standard Signature" FontAttributes="Bold" />
    <Border Stroke="Gray" StrokeThickness="1">
        <signaturePad:SfSignaturePad MinimumStrokeThickness="1"
                                      MaximumStrokeThickness="4"
                                      HeightRequest="150" />
    </Border>
    
    <!-- Bold Signature -->
    <Label Text="Bold Signature" FontAttributes="Bold" />
    <Border Stroke="Gray" StrokeThickness="1">
        <signaturePad:SfSignaturePad MinimumStrokeThickness="2"
                                      MaximumStrokeThickness="6"
                                      StrokeColor="DarkBlue"
                                      HeightRequest="150" />
    </Border>
    
</VerticalStackLayout>
```

## Troubleshooting

### Strokes appear too thin
- Increase `MinimumStrokeThickness` and `MaximumStrokeThickness`
- Try values like `MinimumStrokeThickness="2"` and `MaximumStrokeThickness="6"`

### Strokes have no variation
- Ensure you're using Min/Max thickness, not `StrokeWidth`
- Increase the gap between minimum and maximum values
- Recommended ratio: 1:3 or 1:4

### Strokes look pixelated
- This is a device/platform rendering issue
- Try slightly increasing stroke thickness
- Ensure device graphics drivers are up to date

### Color not appearing correctly
- Verify hex color format includes `#` prefix
- Check alpha channel for transparency: `Color.FromArgb("#80FF5722")`
- Ensure theme isn't overriding your colors
