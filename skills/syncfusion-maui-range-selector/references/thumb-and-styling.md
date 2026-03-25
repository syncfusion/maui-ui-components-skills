# Thumb and Styling

This guide covers thumb customization, overlay styling, visual regions, and the Liquid Glass effect in the .NET MAUI Range Selector (SfRangeSelector).

## Table of Contents
- [Overview](#overview)
- [Thumb Customization](#thumb-customization)
- [Thumb Overlay Styling](#thumb-overlay-styling)
- [Visual Regions](#visual-regions)
- [Liquid Glass Effect](#liquid-glass-effect)
- [Complete Examples](#complete-examples)

## Overview

**Thumbs** are the draggable elements users interact with to select range values. **Overlays** provide visual feedback during thumb interaction. **Regions** are colored areas behind the content that highlight active/inactive ranges.

## Thumb Customization

Customize thumb appearance using the `ThumbStyle` property.

**Properties:**
- `Radius` (double): Thumb size (radius). Default: `10.0`
- `Fill` (Brush): Thumb fill color
- `Stroke` (Brush): Thumb border color
- `StrokeThickness` (double): Thumb border width. Default: `0`
- `OverlapStroke` (Brush): Border color when thumbs overlap

### Thumb Size

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="100"
                         RangeStart="25" RangeEnd="75">
    <sliders:SfRangeSelector.ThumbStyle>
        <sliders:SliderThumbStyle Radius="15" />
    </sliders:SfRangeSelector.ThumbStyle>
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
rangeSelector.ThumbStyle = new SliderThumbStyle
{
    Radius = 15
};
```

### Thumb Color

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="100"
                         RangeStart="25" RangeEnd="75">
    <sliders:SfRangeSelector.ThumbStyle>
        <sliders:SliderThumbStyle Fill="#EE3F3F" />
    </sliders:SfRangeSelector.ThumbStyle>
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
rangeSelector.ThumbStyle = new SliderThumbStyle
{
    Fill = new SolidColorBrush(Color.FromArgb("#EE3F3F"))
};
```

### Thumb Stroke

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="100"
                         RangeStart="25" RangeEnd="75">
    <sliders:SfRangeSelector.ThumbStyle>
        <sliders:SliderThumbStyle Fill="#FFFFFF"
                                  Stroke="#2196F3"
                                  StrokeThickness="2" />
    </sliders:SfRangeSelector.ThumbStyle>
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
rangeSelector.ThumbStyle = new SliderThumbStyle
{
    Fill = new SolidColorBrush(Colors.White),
    Stroke = new SolidColorBrush(Color.FromArgb("#2196F3")),
    StrokeThickness = 2
};
```

### Overlap Stroke

Change thumb border color when thumbs overlap (touch each other).

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="100"
                         RangeStart="45" RangeEnd="55">
    <sliders:SfRangeSelector.ThumbStyle>
        <sliders:SliderThumbStyle Fill="#2196F3"
                                  Stroke="#0D47A1"
                                  StrokeThickness="2"
                                  OverlapStroke="#FFD700" />
    </sliders:SfRangeSelector.ThumbStyle>
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**Use Case:** Provides visual feedback when range becomes very narrow (thumbs touch).

## Thumb Overlay Styling

The overlay is a circular ripple effect that appears around thumbs during interaction.

**Properties:**
- `Radius` (double): Overlay size. Default: `24.0`
- `Fill` (Brush): Overlay color

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="100"
                         RangeStart="25" RangeEnd="75">
    <sliders:SfRangeSelector.ThumbOverlayStyle>
        <sliders:SliderThumbOverlayStyle Radius="20"
                                         Fill="#4D2196F3" />
    </sliders:SfRangeSelector.ThumbOverlayStyle>
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
rangeSelector.ThumbOverlayStyle = new SliderThumbOverlayStyle
{
    Radius = 20,
    Fill = new SolidColorBrush(Color.FromArgb("#4D2196F3"))  // Semi-transparent
};
```

**Tips:**
- Use semi-transparent colors for subtle effect
- Radius should be larger than thumb radius
- Typical range: 18-30 pixels

## Visual Regions

Regions are colored areas behind the content that highlight the active and inactive ranges.

**Properties:**
- `ActiveRegionFill` (Brush): Background color for active region
- `InactiveRegionFill` (Brush): Background color for inactive regions
- `ActiveRegionStroke` (Brush): Border color for active region
- `InactiveRegionStroke` (Brush): Border color for inactive regions
- `ActiveRegionStrokeThickness` (Thickness): Border width for active region
- `InactiveRegionStrokeThickness` (Thickness): Border width for inactive regions

### Region Colors

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="100"
                         RangeStart="25" RangeEnd="75"
                         ActiveRegionFill="#40FFFF00"
                         InactiveRegionFill="#33FF8A00">
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
SfRangeSelector rangeSelector = new SfRangeSelector
{
    Minimum = 0,
    Maximum = 100,
    RangeStart = 25,
    RangeEnd = 75,
    ActiveRegionFill = new SolidColorBrush(Color.FromArgb("#40FFFF00")),
    InactiveRegionFill = new SolidColorBrush(Color.FromArgb("#33FF8A00"))
};
```

### Region Stroke

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="100"
                         RangeStart="25" RangeEnd="75"
                         ActiveRegionFill="#40FFFF00"
                         InactiveRegionFill="#33FF8A00"
                         ActiveRegionStroke="#FFFF00"
                         InactiveRegionStroke="#B8860B">
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
rangeSelector.ActiveRegionStroke = new SolidColorBrush(Color.FromArgb("#FFFF00"));
rangeSelector.InactiveRegionStroke = new SolidColorBrush(Color.FromArgb("#B8860B"));
```

### Region Stroke Thickness

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="100"
                         RangeStart="25" RangeEnd="75"
                         ActiveRegionFill="#40FFFF00"
                         InactiveRegionFill="#33FF8A00"
                         ActiveRegionStroke="#A52A2A"
                         InactiveRegionStroke="#A52A2A"
                         ActiveRegionStrokeThickness="3,0,3,0"
                         InactiveRegionStrokeThickness="0,3,0,3">
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**Use Cases:**
- Highlight selected chart data range
- Emphasize active vs inactive areas
- Create visual separation for data segments

## Liquid Glass Effect

The Liquid Glass Effect provides a modern, translucent design with adaptive color tinting and light refraction.

**Property:**
- `EnableLiquidGlassEffect` (bool): Enable liquid glass effect. Default: `false`

**Requirements:**
- **.NET 10 or later**
- **iOS 26+** or **macOS 26+**

**XAML:**
```xaml
<Grid>
    <Image Source="Wallpaper.png" Aspect="AspectFill" />
    
    <sliders:SfRangeSelector Minimum="10"
                             Maximum="20"
                             RangeStart="13"
                             RangeEnd="17"
                             EnableLiquidGlassEffect="True">
        <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
    </sliders:SfRangeSelector>
</Grid>
```

**C#:**
```csharp
var grid = new Grid
{
    BackgroundColor = Colors.Transparent
};

var image = new Image
{
    Source = "Wallpaper.png",
    Aspect = Aspect.AspectFill
};
grid.Children.Add(image);

var rangeSelector = new SfRangeSelector
{
    Minimum = 10,
    Maximum = 20,
    RangeStart = 13,
    RangeEnd = 17,
    EnableLiquidGlassEffect = true
};

grid.Children.Add(rangeSelector);
this.Content = grid;
```

**Behavior:**
- Applies translucent glass-like effect to control
- Blurs and tints background content
- Provides responsive interaction feedback
- Most visible on thumbs during press/drag

**Best Practices:**
- Use with visually rich backgrounds (images, gradients)
- Test on target platforms (iOS 26+, macOS 26+)
- Combine with appropriate thumb and track styling

## Complete Examples

### Material Design Style
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="100"
                         RangeStart="30" RangeEnd="70">
    <!-- Track -->
    <sliders:SfRangeSelector.TrackStyle>
        <sliders:SliderTrackStyle ActiveFill="#2196F3"
                                  InactiveFill="#BBDEFB"
                                  ActiveSize="4"
                                  InactiveSize="4" />
    </sliders:SfRangeSelector.TrackStyle>
    
    <!-- Thumb -->
    <sliders:SfRangeSelector.ThumbStyle>
        <sliders:SliderThumbStyle Radius="10"
                                  Fill="#2196F3" />
    </sliders:SfRangeSelector.ThumbStyle>
    
    <!-- Overlay -->
    <sliders:SfRangeSelector.ThumbOverlayStyle>
        <sliders:SliderThumbOverlayStyle Radius="24"
                                         Fill="#332196F3" />
    </sliders:SfRangeSelector.ThumbOverlayStyle>
    
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

### Highlighted Regions with Borders
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="100"
                         RangeStart="20" RangeEnd="80"
                         ActiveRegionFill="#204CAF50"
                         InactiveRegionFill="#20FF5722"
                         ActiveRegionStroke="#4CAF50"
                         InactiveRegionStroke="#FF5722"
                         ActiveRegionStrokeThickness="2"
                         InactiveRegionStrokeThickness="1">
    <sliders:SfRangeSelector.ThumbStyle>
        <sliders:SliderThumbStyle Radius="12"
                                  Fill="White"
                                  Stroke="#4CAF50"
                                  StrokeThickness="3" />
    </sliders:SfRangeSelector.ThumbStyle>
    
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

### Premium Style with Liquid Glass
```xaml
<Grid>
    <Image Source="premium_background.jpg" Aspect="AspectFill" />
    
    <sliders:SfRangeSelector Minimum="0" Maximum="100"
                             RangeStart="25" RangeEnd="75"
                             EnableLiquidGlassEffect="True">
        <sliders:SfRangeSelector.TrackStyle>
            <sliders:SliderTrackStyle ActiveFill="#80FFFFFF"
                                      InactiveFill="#40FFFFFF"
                                      ActiveSize="6"
                                      InactiveSize="6" />
        </sliders:SfRangeSelector.TrackStyle>
        
        <sliders:SfRangeSelector.ThumbStyle>
            <sliders:SliderThumbStyle Radius="14"
                                      Fill="#FFFFFF"
                                      Stroke="#E0E0E0"
                                      StrokeThickness="1" />
        </sliders:SfRangeSelector.ThumbStyle>
        
        <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
    </sliders:SfRangeSelector>
</Grid>
```

### Dark Theme
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="100"
                         RangeStart="30" RangeEnd="70"
                         ActiveRegionFill="#2000BCD4"
                         InactiveRegionFill="#10607D8B">
    <sliders:SfRangeSelector.TrackStyle>
        <sliders:SliderTrackStyle ActiveFill="#00BCD4"
                                  InactiveFill="#607D8B"
                                  ActiveSize="6"
                                  InactiveSize="4" />
    </sliders:SfRangeSelector.TrackStyle>
    
    <sliders:SfRangeSelector.ThumbStyle>
        <sliders:SliderThumbStyle Radius="12"
                                  Fill="#00BCD4"
                                  Stroke="#006064"
                                  StrokeThickness="2" />
    </sliders:SfRangeSelector.ThumbStyle>
    
    <sliders:SfRangeSelector.ThumbOverlayStyle>
        <sliders:SliderThumbOverlayStyle Radius="26"
                                         Fill="#4000BCD4" />
    </sliders:SfRangeSelector.ThumbOverlayStyle>
    
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```
