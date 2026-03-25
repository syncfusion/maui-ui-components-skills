# Styling & Customization

This guide covers visual customization, theming, and advanced styling options for the .NET MAUI ImageEditor control.

## Table of Contents
- [Styling Overview](#styling-overview)
- [Control Background](#control-background)
- [Selection Appearance](#selection-appearance)
- [Toolbar Styling](#toolbar-styling)
- [Liquid Glass Effect](#liquid-glass-effect)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)
- [Next Steps](#next-steps)

## Styling Overview

The ImageEditor provides various customization options to match your app's design system and branding.

### Customizable Elements

- **Background** - Control background color/brush
- **Selection Stroke** - Annotation selection border color
- **Toolbar Appearance** - Toolbar colors and transparency
- **Annotation Colors** - Color palette customization
- **Liquid Glass Effect** - Modern translucent UI (iOS 26+, macOS 26+)

## Control Background

Customize the ImageEditor background using the `Background` property:

### Solid Color Background

```xml
<imageEditor:SfImageEditor Source="photo.jpg" 
                          Background="#F0F0F0" />
```

```csharp
SfImageEditor imageEditor = new SfImageEditor();
imageEditor.Source = "photo.jpg";
imageEditor.Background = Colors.LightGray;
```

### Transparent Background

```xml
<imageEditor:SfImageEditor Source="photo.jpg" 
                          Background="Transparent" />
```

```csharp
imageEditor.Background = Colors.Transparent;
```

**When to use:**
- Match app theme
- Implement custom backgrounds
- Layer with other UI elements
- Liquid glass effect integration

## Selection Appearance

Customize the appearance of selected annotations using the `SelectionStroke` property:

### Selection Stroke Color

```xml
<imageEditor:SfImageEditor Source="photo.jpg"
                          SelectionStroke="#AE97FF" />
```

```csharp
SfImageEditor imageEditor = new SfImageEditor();
imageEditor.Source = "photo.jpg";
imageEditor.SelectionStroke = Color.FromArgb("#AE97FF");
```

**Default:** Platform default selection color

**When to customize:**
- Brand consistency
- High contrast needs
- Accessibility requirements
- Theme matching

## Toolbar Styling

Customize toolbar appearance using `ImageEditorToolbarSettings`:

### Background and Stroke

```xml
<imageEditor:SfImageEditor Source="photo.jpg">
    <imageEditor:SfImageEditor.ToolbarSettings>
        <imageEditor:ImageEditorToolbarSettings 
            Background="#2C2C2C"
            Stroke="#444444" />
    </imageEditor:SfImageEditor.ToolbarSettings>
</imageEditor:SfImageEditor>
```

```csharp
SfImageEditor imageEditor = new SfImageEditor();
imageEditor.Source = "photo.jpg";
imageEditor.ToolbarSettings.Background = Brush.SolidColorBrush(Color.FromArgb("#2C2C2C"));
imageEditor.ToolbarSettings.Stroke = Color.FromArgb("#444444");
```

### Transparent Toolbar

```xml
<imageEditor:SfImageEditor Source="photo.jpg">
    <imageEditor:SfImageEditor.ToolbarSettings>
        <imageEditor:ImageEditorToolbarSettings 
            Background="Transparent"
            Stroke="Transparent" />
    </imageEditor:SfImageEditor.ToolbarSettings>
</imageEditor:SfImageEditor>
```

**When to use:**
- Overlay-style UI
- Minimal design
- Liquid glass effect
- Custom toolbar backgrounds

## Liquid Glass Effect

Apply modern translucent glass-like styling (iOS 26+, macOS 26+, .NET 10).

### Requirements

- **Platform:** macOS 26+ or iOS 26+
- **Framework:** .NET 10
- **NuGet:** `Syncfusion.Maui.Core` (for `SfGlassEffectView`)

### Basic Implementation

```xml
<Grid BackgroundColor="Transparent">
    <core:SfGlassEffectView EffectType="Regular"
                            CornerRadius="20">
        <imageEditor:SfImageEditor x:Name="imageEditor"
                                   Background="Transparent"
                                   SelectionStroke="#AE97FF"
                                   Source="photo.png"
                                   EnableLiquidGlassEffect="True">
            <imageEditor:SfImageEditor.ToolbarSettings>
                <imageEditor:ImageEditorToolbarSettings 
                    Background="Transparent"
                    Stroke="Transparent"/>
            </imageEditor:SfImageEditor.ToolbarSettings>
        </imageEditor:SfImageEditor>
    </core:SfGlassEffectView>
</Grid>
```

### Code-Behind Implementation

```csharp
using Syncfusion.Maui.Core;
using Syncfusion.Maui.ImageEditor;

var grid = new Grid
{
    BackgroundColor = Colors.Transparent
};

var glassView = new SfGlassEffectView
{
    CornerRadius = 20,
    EffectType = LiquidGlassEffectType.Regular
};

var imageEditor = new SfImageEditor
{
    Background = Colors.Transparent,
    EnableLiquidGlassEffect = true,
    SelectionStroke = Color.FromArgb("#AE97FF"),
    Source = ImageSource.FromFile("photo.png"),
    ToolbarSettings = new ImageEditorToolbarSettings
    {
        Background = Colors.Transparent,
        Stroke = Colors.Transparent
    }
};

glassView.Content = imageEditor;
grid.Children.Add(glassView);
this.Content = grid;
```

### Key Properties for Liquid Glass

1. **Wrap in SfGlassEffectView** - Required container
2. **EnableLiquidGlassEffect="True"** - Enables effect on ImageEditor
3. **Background="Transparent"** - Both ImageEditor and Toolbar
4. **CornerRadius** - Rounds glass effect edges

**When to use:**
- Modern, premium UI aesthetics
- iOS/macOS native feel
- Contemporary design systems
- High-end app experiences

## Common Patterns

### Dark Theme Styling

```xml
<imageEditor:SfImageEditor Source="photo.jpg"
                          Background="#1E1E1E"
                          SelectionStroke="#BB86FC">
    <imageEditor:SfImageEditor.ToolbarSettings>
        <imageEditor:ImageEditorToolbarSettings 
            Background="#2D2D2D"
            Stroke="#404040">
            <imageEditor:ImageEditorToolbarSettings.ColorPalette>
                <Color>#BB86FC</Color>
                <Color>#03DAC6</Color>
                <Color>#CF6679</Color>
                <Color>#018786</Color>
                <Color>#3700B3</Color>
            </imageEditor:ImageEditorToolbarSettings.ColorPalette>
        </imageEditor:ImageEditorToolbarSettings>
    </imageEditor:SfImageEditor.ToolbarSettings>
</imageEditor:SfImageEditor>
```

### Light Theme Styling

```xml
<imageEditor:SfImageEditor Source="photo.jpg"
                          Background="#FFFFFF"
                          SelectionStroke="#6200EE">
    <imageEditor:SfImageEditor.ToolbarSettings>
        <imageEditor:ImageEditorToolbarSettings 
            Background="#F5F5F5"
            Stroke="#E0E0E0">
            <imageEditor:ImageEditorToolbarSettings.ColorPalette>
                <Color>#6200EE</Color>
                <Color>#03DAC5</Color>
                <Color>#B00020</Color>
                <Color>#018786</Color>
                <Color>#3700B3</Color>
            </imageEditor:ImageEditorToolbarSettings.ColorPalette>
        </imageEditor:ImageEditorToolbarSettings>
    </imageEditor:SfImageEditor.ToolbarSettings>
</imageEditor:SfImageEditor>
```

### Brand Color System

```csharp
private void ApplyBrandStyling()
{
    // Primary brand color
    Color primaryColor = Color.FromArgb("#FF6B35");
    Color secondaryColor = Color.FromArgb("#004E89");
    Color accentColor = Color.FromArgb("#F7B801");
    
    // Apply to ImageEditor
    imageEditor.SelectionStroke = primaryColor;
    
    // Toolbar styling
    imageEditor.ToolbarSettings.Background = Brush.SolidColorBrush(secondaryColor);
    imageEditor.ToolbarSettings.Stroke = accentColor;
    
    // Custom color palette
    imageEditor.ToolbarSettings.ColorPalette.Clear();
    imageEditor.ToolbarSettings.ColorPalette.Add(primaryColor);
    imageEditor.ToolbarSettings.ColorPalette.Add(secondaryColor);
    imageEditor.ToolbarSettings.ColorPalette.Add(accentColor);
    imageEditor.ToolbarSettings.ColorPalette.Add(Color.FromArgb("#1A659E"));
    imageEditor.ToolbarSettings.ColorPalette.Add(Color.FromArgb("#00AFB9"));
}
```

### Dynamic Theme Switching

```csharp
private void ApplyTheme(bool isDarkMode)
{
    if (isDarkMode)
    {
        // Dark theme
        imageEditor.Background = Brush.SolidColorBrush(Color.FromArgb("#1E1E1E"));
        imageEditor.SelectionStroke = Color.FromArgb("#BB86FC");
        imageEditor.ToolbarSettings.Background = 
            Brush.SolidColorBrush(Color.FromArgb("#2D2D2D"));
        imageEditor.ToolbarSettings.Stroke = Color.FromArgb("#404040");
    }
    else
    {
        // Light theme
        imageEditor.Background = Brush.SolidColorBrush(Colors.White);
        imageEditor.SelectionStroke = Color.FromArgb("#6200EE");
        imageEditor.ToolbarSettings.Background = 
            Brush.SolidColorBrush(Color.FromArgb("#F5F5F5"));
        imageEditor.ToolbarSettings.Stroke = Color.FromArgb("#E0E0E0");
    }
}
```

### Minimal Transparent Design

```xml
<Grid>
    <Image Source="background_pattern.png" Aspect="Fill" />
    
    <imageEditor:SfImageEditor Source="photo.jpg"
                              Background="Transparent"
                              SelectionStroke="White">
        <imageEditor:SfImageEditor.ToolbarSettings>
            <imageEditor:ImageEditorToolbarSettings 
                Background="#80000000"
                Stroke="Transparent" />
        </imageEditor:SfImageEditor.ToolbarSettings>
    </imageEditor:SfImageEditor>
</Grid>
```

### High Contrast Accessibility

```csharp
private void ApplyHighContrast()
{
    // High contrast colors
    imageEditor.Background = Brush.SolidColorBrush(Colors.Black);
    imageEditor.SelectionStroke = Colors.Yellow;
    
    imageEditor.ToolbarSettings.Background = 
        Brush.SolidColorBrush(Colors.Black);
    imageEditor.ToolbarSettings.Stroke = Colors.White;
    
    // High contrast palette
    imageEditor.ToolbarSettings.ColorPalette.Clear();
    imageEditor.ToolbarSettings.ColorPalette.Add(Colors.White);
    imageEditor.ToolbarSettings.ColorPalette.Add(Colors.Yellow);
    imageEditor.ToolbarSettings.ColorPalette.Add(Colors.Cyan);
    imageEditor.ToolbarSettings.ColorPalette.Add(Colors.Magenta);
    imageEditor.ToolbarSettings.ColorPalette.Add(Colors.Lime);
}
```

### Conditional Liquid Glass Effect

```csharp
private void SetupImageEditor()
{
    #if IOS || MACCATALYST
    if (DeviceInfo.Version.Major >= 26)
    {
        // Use liquid glass effect
        ApplyLiquidGlassEffect();
    }
    else
    {
        // Fallback styling
        ApplyStandardStyling();
    }
    #else
    ApplyStandardStyling();
    #endif
}

private void ApplyLiquidGlassEffect()
{
    var glassView = new SfGlassEffectView
    {
        EffectType = LiquidGlassEffectType.Regular,
        CornerRadius = 20
    };
    
    imageEditor.EnableLiquidGlassEffect = true;
    imageEditor.Background = Colors.Transparent;
    imageEditor.ToolbarSettings.Background = Colors.Transparent;
    imageEditor.ToolbarSettings.Stroke = Colors.Transparent;
    
    glassView.Content = imageEditor;
    RootGrid.Children.Add(glassView);
}

private void ApplyStandardStyling()
{
    imageEditor.Background = Brush.SolidColorBrush(Colors.White);
    imageEditor.ToolbarSettings.Background = 
        Brush.SolidColorBrush(Color.FromArgb("#F5F5F5"));
}
```

## Troubleshooting

### Issue: Colors Not Applying

**Cause:** Invalid color format or property not set.

**Solution:**
```csharp
// Use valid color format
imageEditor.SelectionStroke = Color.FromArgb("#FF6B35");
// Not: imageEditor.SelectionStroke = "#FF6B35";
```

### Issue: Toolbar Styling Not Visible

**Cause:** Toolbar hidden or transparent overlapping.

**Solution:**
```csharp
// Ensure toolbar is visible
imageEditor.ShowToolbar = true;

// Use visible colors
imageEditor.ToolbarSettings.Background = 
    Brush.SolidColorBrush(Colors.Gray);
```

### Issue: Liquid Glass Effect Not Working

**Cause:** Platform/version not supported or missing wrapper.

**Solution:**
```xml
<!-- Ensure wrapped in SfGlassEffectView -->
<core:SfGlassEffectView>
    <imageEditor:SfImageEditor EnableLiquidGlassEffect="True" />
</core:SfGlassEffectView>
```

Check platform and version:
```csharp
#if IOS || MACCATALYST
if (DeviceInfo.Version.Major < 26)
{
    // Not supported
}
#endif
```

### Issue: Transparent Background Shows Artifacts

**Cause:** Parent container has non-transparent background.

**Solution:**
```xml
<!-- Set Grid background to transparent -->
<Grid BackgroundColor="Transparent">
    <imageEditor:SfImageEditor Background="Transparent" />
</Grid>
```

## Next Steps

- **Getting Started:** [getting-started.md](getting-started.md)
- **Toolbar Customization:** [toolbar.md](toolbar.md)
- **Accessibility & Localization:** [accessibility-localization.md](accessibility-localization.md)
- **Annotations:** [annotations.md](annotations.md)