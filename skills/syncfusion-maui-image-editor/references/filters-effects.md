# Image Filters & Effects

This guide covers applying visual effects and filters to images in the .NET MAUI ImageEditor control, including brightness, contrast, blur, saturation, and more.

## Table of Contents
- [Effects Overview](#effects-overview)
- [Available Effects](#available-effects)
- [Brightness Effect](#brightness-effect)
- [Contrast Effect](#contrast-effect)
- [Exposure Effect](#exposure-effect)
- [Saturation Effect](#saturation-effect)
- [Hue Effect](#hue-effect)
- [Blur Effect](#blur-effect)
- [Sharpen Effect](#sharpen-effect)
- [Opacity Effect](#opacity-effect)
- [Applying Multiple Effects](#applying-multiple-effects)
- [Saving and Canceling Effects](#saving-and-canceling-effects)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)
- [Next Steps](#next-steps)

## Effects Overview

The ImageEditor control provides powerful image effect capabilities allowing you to adjust various visual properties of images programmatically or through the built-in toolbar.

### When to Use Image Effects

- **Photo Enhancement:** Improving image quality and appearance
- **Color Correction:** Adjusting colors and tones
- **Artistic Effects:** Creating specific visual styles
- **Image Preprocessing:** Preparing images for other operations

### Effect Application Workflow

```csharp
// 1. Apply effect (preview mode)
imageEditor.ImageEffect(ImageEffect.Brightness, 0.5);

// 2. Preview the result

// 3. Save the effect (makes it permanent)
imageEditor.SaveEdits();

// OR Cancel to revert
imageEditor.CancelEdits();
```

**Important:** The `ImageEffect` method only applies the effect to the preview. Call `SaveEdits()` to make the effect permanent.

## Available Effects

The `ImageEffect` enum includes:

- **Brightness** - Adjust lightness/darkness
- **Contrast** - Adjust difference between light and dark
- **Exposure** - Alter overall brightness levels
- **Saturation** - Control color intensity
- **Hue** - Change color tone
- **Blur** - Create soft, unfocused appearance
- **Sharpen** - Enhance clarity and definition
- **Opacity** - Control transparency
- **None** - Remove all unsaved effects

## Brightness Effect

Adjusts the overall lightness or darkness of the image.

**Value Range:** -1 to 1 (default: 0)
- Negative values: Darker
- Positive values: Brighter
- 0: Original brightness

### Basic Usage

```csharp
// Darken the image
imageEditor.ImageEffect(ImageEffect.Brightness, -0.6);

// Brighten the image
imageEditor.ImageEffect(ImageEffect.Brightness, 0.5);
```

### Complete Example

```xml
<Grid RowDefinitions="0.9*, 0.1*">
    <imageEditor:SfImageEditor x:Name="imageEditor" Source="photo.jpg" />
    <Button Grid.Row="1" Text="Brightness" Clicked="OnBrightnessClicked" />
</Grid>
```

```csharp
private void OnBrightnessClicked(object sender, EventArgs e)
{
    imageEditor.ImageEffect(ImageEffect.Brightness, -0.6);
}
```

**When to use:**
- Correcting underexposed or overexposed photos
- Creating dramatic lighting effects
- Adjusting image visibility

## Contrast Effect

Increases or decreases the difference between light and dark areas.

**Value Range:** -1 to 1 (default: 0)
- Negative values: Lower contrast (flatter)
- Positive values: Higher contrast (more distinct)
- 0: Original contrast

### Basic Usage

```csharp
// Decrease contrast
imageEditor.ImageEffect(ImageEffect.Contrast, -0.8);

// Increase contrast
imageEditor.ImageEffect(ImageEffect.Contrast, 0.7);
```

### Complete Example

```xml
<Grid RowDefinitions="0.9*, 0.1*">
    <imageEditor:SfImageEditor x:Name="imageEditor" Source="photo.jpg" />
    <Button Grid.Row="1" Text="Contrast" Clicked="OnContrastClicked" />
</Grid>
```

```csharp
private void OnContrastClicked(object sender, EventArgs e)
{
    imageEditor.ImageEffect(ImageEffect.Contrast, -0.8);
}
```

**When to use:**
- Making images more visually distinct
- Correcting washed-out photos
- Creating high-impact visuals

## Exposure Effect

Alters the overall brightness and darkness levels, similar to camera exposure settings.

**Value Range:** -1 to 1 (default: 0)
- Negative values: Underexposed (darker)
- Positive values: Overexposed (brighter)
- 0: Normal exposure

### Basic Usage

```csharp
// Reduce exposure
imageEditor.ImageEffect(ImageEffect.Exposure, -0.4);

// Increase exposure
imageEditor.ImageEffect(ImageEffect.Exposure, 0.6);
```

### Complete Example

```xml
<Grid RowDefinitions="0.9*, 0.1*">
    <imageEditor:SfImageEditor x:Name="imageEditor" Source="photo.jpg" />
    <Button Grid.Row="1" Text="Exposure" Clicked="OnExposureClicked" />
</Grid>
```

```csharp
private void OnExposureClicked(object sender, EventArgs e)
{
    imageEditor.ImageEffect(ImageEffect.Exposure, -0.4);
}
```

**When to use:**
- Correcting exposure problems
- Creating specific lighting moods
- Balancing image brightness

## Saturation Effect

Enhances or reduces the intensity and vividness of colors.

**Value Range:** -1 to 1 (default: 0)
- Negative values: Less saturated (toward grayscale)
- Positive values: More saturated (vivid colors)
- 0: Original saturation
- -1: Grayscale

### Basic Usage

```csharp
// Desaturate (move toward grayscale)
imageEditor.ImageEffect(ImageEffect.Saturation, -0.8);

// Boost saturation
imageEditor.ImageEffect(ImageEffect.Saturation, 0.6);

// Complete grayscale
imageEditor.ImageEffect(ImageEffect.Saturation, -1);
```

### Complete Example

```xml
<Grid RowDefinitions="0.9*, 0.1*">
    <imageEditor:SfImageEditor x:Name="imageEditor" Source="photo.jpg" />
    <Button Grid.Row="1" Text="Saturation" Clicked="OnSaturationClicked" />
</Grid>
```

```csharp
private void OnSaturationClicked(object sender, EventArgs e)
{
    imageEditor.ImageEffect(ImageEffect.Saturation, -0.8);
}
```

**When to use:**
- Creating black and white effects
- Enhancing color vibrancy
- Toning down oversaturated images
- Creating vintage or muted looks

## Hue Effect

Changes the overall color tone by shifting the color spectrum.

**Value Range:** -1 to 1 (default: 0)
- Shifts colors along the spectrum
- 0: Original colors

### Basic Usage

```csharp
// Shift hue
imageEditor.ImageEffect(ImageEffect.Hue, 0.2);

// Different hue shift
imageEditor.ImageEffect(ImageEffect.Hue, -0.5);
```

### Complete Example

```xml
<Grid RowDefinitions="0.9*, 0.1*">
    <imageEditor:SfImageEditor x:Name="imageEditor" Source="photo.jpg" />
    <Button Grid.Row="1" Text="Hue" Clicked="OnHueClicked" />
</Grid>
```

```csharp
private void OnHueClicked(object sender, EventArgs e)
{
    imageEditor.ImageEffect(ImageEffect.Hue, 0.2);
}
```

**When to use:**
- Color correction
- Creating artistic color schemes
- Matching color tones across images
- Special color effects

## Blur Effect

Creates a soft and unfocused appearance by reducing sharpness.

**Value Range:** 0 to 1 (default: 0)
- 0: No blur (sharp)
- 1: Maximum blur
- Values increase blur intensity

### Basic Usage

```csharp
// Moderate blur
imageEditor.ImageEffect(ImageEffect.Blur, 0.5);

// Light blur
imageEditor.ImageEffect(ImageEffect.Blur, 0.2);

// Heavy blur
imageEditor.ImageEffect(ImageEffect.Blur, 0.9);
```

### Complete Example

```xml
<Grid RowDefinitions="0.9*, 0.1*">
    <imageEditor:SfImageEditor x:Name="imageEditor" Source="photo.jpg" />
    <Button Grid.Row="1" Text="Blur" Clicked="OnBlurClicked" />
</Grid>
```

```csharp
private void OnBlurClicked(object sender, EventArgs e)
{
    imageEditor.ImageEffect(ImageEffect.Blur, 0.5);
}
```

**When to use:**
- Creating depth-of-field effects
- De-emphasizing backgrounds
- Privacy (blurring faces or sensitive info)
- Artistic soft focus effects

## Sharpen Effect

Enhances the clarity and definition of edges and details.

**Value Range:** 0 to 6 (default: 0)
- 0: No sharpening
- Higher values: Increased sharpness
- 6: Maximum sharpening

### Basic Usage

```csharp
// Moderate sharpening
imageEditor.ImageEffect(ImageEffect.Sharpen, 0.5);

// Strong sharpening
imageEditor.ImageEffect(ImageEffect.Sharpen, 3.0);
```

### Complete Example

```xml
<Grid RowDefinitions="0.9*, 0.1*">
    <imageEditor:SfImageEditor x:Name="imageEditor" Source="photo.jpg" />
    <Button Grid.Row="1" Text="Sharpen" Clicked="OnSharpenClicked" />
</Grid>
```

```csharp
private void OnSharpenClicked(object sender, EventArgs e)
{
    imageEditor.ImageEffect(ImageEffect.Sharpen, 0.5);
}
```

**When to use:**
- Correcting slightly blurry images
- Enhancing detail in photos
- Improving text readability
- Making edges more defined

## Opacity Effect

Controls the transparency or visibility of the image.

**Value Range:** 0 to 1 (default: 1)
- 0: Fully transparent
- 1: Fully opaque
- Values between: Semi-transparent

### Basic Usage

```csharp
// Semi-transparent
imageEditor.ImageEffect(ImageEffect.Opacity, 0.5);

// Very transparent
imageEditor.ImageEffect(ImageEffect.Opacity, 0.2);
```

### Complete Example

```xml
<Grid RowDefinitions="0.9*, 0.1*">
    <imageEditor:SfImageEditor x:Name="imageEditor" Source="photo.jpg" />
    <Button Grid.Row="1" Text="Opacity" Clicked="OnOpacityClicked" />
</Grid>
```

```csharp
private void OnOpacityClicked(object sender, EventArgs e)
{
    imageEditor.ImageEffect(ImageEffect.Opacity, 0.5);
}
```

**When to use:**
- Creating watermarks
- Blending images
- Fading effects
- Overlay preparations

## Applying Multiple Effects

Apply multiple effects sequentially:

```csharp
private void ApplyMultipleEffects()
{
    // Apply brightness
    imageEditor.ImageEffect(ImageEffect.Brightness, 0.2);
    imageEditor.SaveEdits();
    
    // Apply contrast
    imageEditor.ImageEffect(ImageEffect.Contrast, 0.3);
    imageEditor.SaveEdits();
    
    // Apply saturation
    imageEditor.ImageEffect(ImageEffect.Saturation, -0.4);
    imageEditor.SaveEdits();
}
```

**Important:** Call `SaveEdits()` after each effect to make it permanent before applying the next effect.

## Saving and Canceling Effects

### Saving Effects

Make effects permanent using `SaveEdits()`:

```xml
<Grid RowDefinitions="0.9*, 0.1*">
    <imageEditor:SfImageEditor x:Name="imageEditor" Source="photo.jpg" />
    <Button Grid.Row="1" Text="Save Effects" Clicked="OnSaveEditsClicked" />
</Grid>
```

```csharp
private void OnSaveEditsClicked(object sender, EventArgs e)
{
    imageEditor.SaveEdits();
}
```

### Canceling Effects

Revert unsaved effects using `CancelEdits()` or `ImageEffect.None`:

**Method 1: CancelEdits**
```csharp
private void OnCancelEditsClicked(object sender, EventArgs e)
{
    imageEditor.CancelEdits();
}
```

**Method 2: ImageEffect.None**
```csharp
private void OnRemoveEffectsClicked(object sender, EventArgs e)
{
    imageEditor.ImageEffect(ImageEffect.None, 0);
}
```

**Important:** If you don't call `SaveEdits()`, effects will be reset on the next action.

## Common Patterns

### Photo Enhancement Preset

```csharp
private void EnhancePhoto()
{
    // Brighten slightly
    imageEditor.ImageEffect(ImageEffect.Brightness, 0.1);
    imageEditor.SaveEdits();
    
    // Increase contrast
    imageEditor.ImageEffect(ImageEffect.Contrast, 0.2);
    imageEditor.SaveEdits();
    
    // Boost saturation
    imageEditor.ImageEffect(ImageEffect.Saturation, 0.15);
    imageEditor.SaveEdits();
    
    // Light sharpening
    imageEditor.ImageEffect(ImageEffect.Sharpen, 0.3);
    imageEditor.SaveEdits();
}
```

### Black and White Conversion

```csharp
private void ConvertToBlackAndWhite()
{
    // Remove all color
    imageEditor.ImageEffect(ImageEffect.Saturation, -1);
    imageEditor.SaveEdits();
    
    // Adjust contrast for better B&W
    imageEditor.ImageEffect(ImageEffect.Contrast, 0.3);
    imageEditor.SaveEdits();
}
```

### Vintage Effect

```csharp
private void ApplyVintageEffect()
{
    // Reduce saturation
    imageEditor.ImageEffect(ImageEffect.Saturation, -0.4);
    imageEditor.SaveEdits();
    
    // Adjust exposure
    imageEditor.ImageEffect(ImageEffect.Exposure, -0.2);
    imageEditor.SaveEdits();
    
    // Lower contrast
    imageEditor.ImageEffect(ImageEffect.Contrast, -0.3);
    imageEditor.SaveEdits();
}
```

### Soft Glow Effect

```csharp
private void ApplySoftGlow()
{
    // Increase brightness
    imageEditor.ImageEffect(ImageEffect.Brightness, 0.3);
    imageEditor.SaveEdits();
    
    // Add blur
    imageEditor.ImageEffect(ImageEffect.Blur, 0.2);
    imageEditor.SaveEdits();
    
    // Boost saturation
    imageEditor.ImageEffect(ImageEffect.Saturation, 0.2);
    imageEditor.SaveEdits();
}
```

### Effect with Slider Control

```xml
<Grid RowDefinitions="0.8*, 0.1*, 0.1*">
    <imageEditor:SfImageEditor x:Name="imageEditor" Source="photo.jpg" />
    
    <Slider Grid.Row="1" 
            Minimum="-1" 
            Maximum="1" 
            Value="0"
            ValueChanged="OnBrightnessSliderChanged" />
    
    <HorizontalStackLayout Grid.Row="2" HorizontalOptions="Center">
        <Button Text="Save" Clicked="OnSaveClicked" Margin="5" />
        <Button Text="Cancel" Clicked="OnCancelClicked" Margin="5" />
    </HorizontalStackLayout>
</Grid>
```

```csharp
private void OnBrightnessSliderChanged(object sender, ValueChangedEventArgs e)
{
    imageEditor.ImageEffect(ImageEffect.Brightness, e.NewValue);
}

private void OnSaveClicked(object sender, EventArgs e)
{
    imageEditor.SaveEdits();
}

private void OnCancelClicked(object sender, EventArgs e)
{
    imageEditor.CancelEdits();
}
```

## Troubleshooting

### Issue: Effect Not Visible

**Cause:** Value is too subtle or at default (0).

**Solution:**
```csharp
// Use more extreme values to see effect
imageEditor.ImageEffect(ImageEffect.Brightness, 0.8);
```

### Issue: Effects Lost After Other Actions

**Cause:** Effects not saved with `SaveEdits()`.

**Solution:**
```csharp
// Always save effects you want to keep
imageEditor.ImageEffect(ImageEffect.Contrast, 0.5);
imageEditor.SaveEdits();  // Make it permanent
```

### Issue: Cannot Undo Effect

**Cause:** Effect was already saved with `SaveEdits()`.

**Solution:** Use Undo/Redo functionality:
```csharp
// Undo the last saved change
imageEditor.Undo();
```

### Issue: Sharpen Effect Too Strong

**Cause:** Value too high (remember range is 0-6, not 0-1).

**Solution:**
```csharp
// Use moderate values for sharpen
imageEditor.ImageEffect(ImageEffect.Sharpen, 0.5);  // Not 5!
```

### Issue: Multiple Effects Conflicting

**Cause:** Applying contradicting effects without saving.

**Solution:**
```csharp
// Save each effect before applying next
imageEditor.ImageEffect(ImageEffect.Brightness, 0.3);
imageEditor.SaveEdits();  // Save first

imageEditor.ImageEffect(ImageEffect.Contrast, 0.2);
imageEditor.SaveEdits();  // Save second
```

## Next Steps

- **Saving Images:** [save-serialization.md](save-serialization.md)
- **Undo/Redo:** [undo-redo.md](undo-redo.md)
- **Toolbar Customization:** [toolbar.md](toolbar.md)
- **Getting Started:** [getting-started.md](getting-started.md)