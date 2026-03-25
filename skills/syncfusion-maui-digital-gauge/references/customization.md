# Customization Options for .NET MAUI DigitalGauge

## Table of Contents
- [Overview](#overview)
- [Character Size Customization](#character-size-customization)
- [Character Spacing](#character-spacing)
- [Segment Stroke Customization](#segment-stroke-customization)
- [Segment Stroke Width](#segment-stroke-width)
- [Disabled Segment Styling](#disabled-segment-styling)
- [Background Color](#background-color)
- [Complete Styling Examples](#complete-styling-examples)
- [Styling Best Practices](#styling-best-practices)
- [Responsive Design](#responsive-design)

## Overview

The DigitalGauge provides extensive customization options to control the appearance of digital characters. You can customize:
- Character dimensions (height and width)
- Spacing between characters
- Active segment colors
- Segment stroke width
- Disabled segment appearance
- Background color

These properties allow you to create displays that match your application's design aesthetic, from classic LED displays to modern digital interfaces.

## Character Size Customization

Control the size of digital characters using `CharacterHeight` and `CharacterWidth` properties.

### CharacterHeight

Sets the height of each character in pixels.

**Default Value:** 60

**XAML:**
```xml
<gauge:SfDigitalGauge Text="12345" 
                      CharacterHeight="100" />
```

**C#:**
```csharp
SfDigitalGauge digitalGauge = new SfDigitalGauge();
digitalGauge.Text = "12345";
digitalGauge.CharacterHeight = 100;
```

### CharacterWidth

Sets the width of each character in pixels.

**Default Value:** 40

**XAML:**
```xml
<gauge:SfDigitalGauge Text="12345" 
                      CharacterWidth="70" />
```

**C#:**
```csharp
digitalGauge.CharacterWidth = 70;
```

### Combined Height and Width

```xml
<gauge:SfDigitalGauge Text="SYNCFUSION" 
                      CharacterHeight="90" 
                      CharacterWidth="70"
                      CharacterType="SixteenSegment" />
```

### Size Recommendations

| Display Type | Small | Medium | Large | Extra Large |
|--------------|-------|--------|-------|-------------|
| **Height** | 40-60 | 70-90 | 100-120 | 130+ |
| **Width** | 25-40 | 50-65 | 70-85 | 90+ |
| **Use Case** | Compact UI | Standard | Dashboard | Hero Display |

### Responsive Sizing Example

```csharp
public class ResponsiveGaugePage : ContentPage
{
    private SfDigitalGauge digitalGauge;
    
    public ResponsiveGaugePage()
    {
        digitalGauge = new SfDigitalGauge
        {
            Text = "12:30",
            CharacterType = DigitalGaugeCharacterType.SevenSegment,
            CharacterStroke = Colors.Red
        };
        
        // Adjust size based on screen
        SetSizeForDevice();
        
        this.Content = digitalGauge;
    }
    
    private void SetSizeForDevice()
    {
        var displayInfo = DeviceDisplay.MainDisplayInfo;
        var width = displayInfo.Width / displayInfo.Density;
        
        if (width < 400) // Phone portrait
        {
            digitalGauge.CharacterHeight = 70;
            digitalGauge.CharacterWidth = 45;
        }
        else if (width < 800) // Phone landscape / small tablet
        {
            digitalGauge.CharacterHeight = 100;
            digitalGauge.CharacterWidth = 65;
        }
        else // Tablet / Desktop
        {
            digitalGauge.CharacterHeight = 140;
            digitalGauge.CharacterWidth = 90;
        }
    }
}
```

### Aspect Ratio Considerations

Maintain proper character proportions:
- **Classic LCD ratio:** Width = Height × 0.6 to 0.7
- **Wide characters:** Width = Height × 0.8 to 0.9
- **Square characters:** Width = Height

```csharp
// Classic ratio
digitalGauge.CharacterHeight = 100;
digitalGauge.CharacterWidth = 65; // 0.65 ratio

// Wide characters
digitalGauge.CharacterHeight = 100;
digitalGauge.CharacterWidth = 85; // 0.85 ratio
```

## Character Spacing

Control the space between characters using the `CharacterSpacing` property.

**Default Value:** 0 (characters are adjacent)

### Basic Spacing

**XAML:**
```xml
<gauge:SfDigitalGauge Text="01-01-24" 
                      CharacterType="EightCrossEightDotMatrix"
                      CharacterSpacing="10" />
```

**C#:**
```csharp
SfDigitalGauge digitalGauge = new SfDigitalGauge();
digitalGauge.Text = "01-01-24";
digitalGauge.CharacterSpacing = 10;
digitalGauge.CharacterType = DigitalGaugeCharacterType.EightCrossEightDotMatrix;
```

### Spacing Recommendations

| Spacing | Effect | Use Case |
|---------|--------|----------|
| 0-5 | Tight | Compact displays, limited space |
| 5-10 | Standard | Most readable for general use |
| 10-20 | Loose | Emphasis, large displays |
| 20+ | Wide | Special effects, very large screens |

### Complete Spacing Example

```xml
<VerticalStackLayout Padding="20" Spacing="20">
    
    <Label Text="No Spacing (0)" />
    <gauge:SfDigitalGauge Text="HELLO" 
                          CharacterSpacing="0"
                          CharacterType="SixteenSegment"
                          CharacterHeight="70"
                          CharacterWidth="55" />
    
    <Label Text="Standard Spacing (8)" />
    <gauge:SfDigitalGauge Text="HELLO" 
                          CharacterSpacing="8"
                          CharacterType="SixteenSegment"
                          CharacterHeight="70"
                          CharacterWidth="55" />
    
    <Label Text="Wide Spacing (15)" />
    <gauge:SfDigitalGauge Text="HELLO" 
                          CharacterSpacing="15"
                          CharacterType="SixteenSegment"
                          CharacterHeight="70"
                          CharacterWidth="55" />
    
</VerticalStackLayout>
```

### Dynamic Spacing

```csharp
public class SpacingDemoPage : ContentPage
{
    private SfDigitalGauge gauge;
    private Slider spacingSlider;
    
    public SpacingDemoPage()
    {
        gauge = new SfDigitalGauge
        {
            Text = "SPACING",
            CharacterType = DigitalGaugeCharacterType.SixteenSegment,
            CharacterHeight = 90,
            CharacterWidth = 70,
            CharacterSpacing = 5
        };
        
        spacingSlider = new Slider
        {
            Minimum = 0,
            Maximum = 30,
            Value = 5
        };
        spacingSlider.ValueChanged += (s, e) =>
        {
            gauge.CharacterSpacing = e.NewValue;
        };
        
        var layout = new VerticalStackLayout
        {
            Padding = new Thickness(20),
            Spacing = 20,
            Children =
            {
                new Label { Text = "Adjust Spacing", FontSize = 20 },
                gauge,
                new Label { Text = "Spacing Control" },
                spacingSlider
            }
        };
        
        this.Content = layout;
    }
}
```

## Segment Stroke Customization

The `CharacterStroke` property controls the color of active (lit) segments.

### Basic Color Setting

**XAML:**
```xml
<gauge:SfDigitalGauge Text="12345" 
                      CharacterStroke="Purple" />
```

**C#:**
```csharp
digitalGauge.CharacterStroke = Colors.Purple;
```

### Named Colors

```xml
<VerticalStackLayout Spacing="15">
    
    <!-- Red display -->
    <gauge:SfDigitalGauge Text="ERROR" 
                          CharacterStroke="Red"
                          CharacterType="FourteenSegment"
                          CharacterHeight="80"
                          CharacterWidth="60" />
    
    <!-- Green display -->
    <gauge:SfDigitalGauge Text="SUCCESS" 
                          CharacterStroke="Green"
                          CharacterType="SixteenSegment"
                          CharacterHeight="80"
                          CharacterWidth="60" />
    
    <!-- Blue display -->
    <gauge:SfDigitalGauge Text="INFO" 
                          CharacterStroke="Blue"
                          CharacterType="FourteenSegment"
                          CharacterHeight="80"
                          CharacterWidth="60" />
    
    <!-- Orange warning -->
    <gauge:SfDigitalGauge Text="WARNING" 
                          CharacterStroke="Orange"
                          CharacterType="SixteenSegment"
                          CharacterHeight="80"
                          CharacterWidth="60" />
    
</VerticalStackLayout>
```

### Hex Colors

```xml
<gauge:SfDigitalGauge Text="CUSTOM" 
                      CharacterStroke="#FF6B35"
                      CharacterType="SixteenSegment" />
```

```csharp
digitalGauge.CharacterStroke = Color.FromArgb("#FF6B35");
```

### RGB Colors

```csharp
// Using FromRgb
digitalGauge.CharacterStroke = Color.FromRgb(255, 107, 53);

// Using FromRgba with transparency
digitalGauge.CharacterStroke = Color.FromRgba(255, 107, 53, 200);
```

### Classic LED Colors

```csharp
public static class LEDColors
{
    public static Color ClassicRed => Color.FromArgb("#FF0000");
    public static Color ClassicGreen => Color.FromArgb("#00FF00");
    public static Color ClassicAmber => Color.FromArgb("#FFBF00");
    public static Color ClassicOrange => Color.FromArgb("#FF8000");
    public static Color ClassicCyan => Color.FromArgb("#00FFFF");
    public static Color NeonBlue => Color.FromArgb("#4D4DFF");
    public static Color NeonPink => Color.FromArgb("#FF1493");
}

// Usage
digitalGauge.CharacterStroke = LEDColors.ClassicAmber;
```

### Dynamic Color Changes

```csharp
public class ColorChangingGauge : ContentPage
{
    private SfDigitalGauge gauge;
    private int counter = 0;
    
    public ColorChangingGauge()
    {
        gauge = new SfDigitalGauge
        {
            CharacterType = DigitalGaugeCharacterType.SevenSegment,
            CharacterHeight = 120,
            CharacterWidth = 75,
            StrokeWidth = 4,
            BackgroundColor = Colors.Black
        };
        
        this.Content = gauge;
        
        // Update color and value every second
        Device.StartTimer(TimeSpan.FromSeconds(1), () =>
        {
            counter++;
            gauge.Text = counter.ToString("D5");
            gauge.CharacterStroke = GetColorForValue(counter);
            return true;
        });
    }
    
    private Color GetColorForValue(int value)
    {
        return (value % 3) switch
        {
            0 => Colors.Red,
            1 => Colors.Green,
            2 => Colors.Blue,
            _ => Colors.White
        };
    }
}
```

## Segment Stroke Width

The `StrokeWidth` property controls the thickness of character segments.

**Default Value:** 1

### Basic Width Setting

**XAML:**
```xml
<gauge:SfDigitalGauge Text="12345" 
                      StrokeWidth="3" />
```

**C#:**
```csharp
digitalGauge.StrokeWidth = 3;
```

### Width Recommendations

| Width | Effect | Use Case |
|-------|--------|----------|
| 1-2 | Thin | Delicate, high resolution displays |
| 2-3 | Standard | Most displays, good balance |
| 3-5 | Bold | Emphasis, visibility from distance |
| 5+ | Extra Bold | Large displays, special effects |

### Stroke Width Examples

```xml
<VerticalStackLayout Spacing="20">
    
    <Label Text="Thin (Width: 1)" />
    <gauge:SfDigitalGauge Text="THIN" 
                          StrokeWidth="1"
                          CharacterType="SixteenSegment"
                          CharacterHeight="70"
                          CharacterWidth="55" />
    
    <Label Text="Standard (Width: 2)" />
    <gauge:SfDigitalGauge Text="STANDARD" 
                          StrokeWidth="2"
                          CharacterType="SixteenSegment"
                          CharacterHeight="70"
                          CharacterWidth="55" />
    
    <Label Text="Bold (Width: 4)" />
    <gauge:SfDigitalGauge Text="BOLD" 
                          StrokeWidth="4"
                          CharacterType="SixteenSegment"
                          CharacterHeight="70"
                          CharacterWidth="55" />
    
</VerticalStackLayout>
```

### Combined Stroke Color and Width

```xml
<gauge:SfDigitalGauge Text="12:30:45" 
                      CharacterType="SevenSegment"
                      CharacterHeight="120"
                      CharacterWidth="75"
                      CharacterStroke="Red"
                      StrokeWidth="5"
                      BackgroundColor="Black" />
```

## Disabled Segment Styling

When a segment is not active (disabled), you can control its appearance using `DisabledSegmentStroke` and `DisabledSegmentAlpha`.

### DisabledSegmentStroke

Sets the color of inactive segments.

**XAML:**
```xml
<gauge:SfDigitalGauge Text="12345" 
                      DisabledSegmentStroke="LightSkyBlue" />
```

**C#:**
```csharp
digitalGauge.DisabledSegmentStroke = Colors.LightSkyBlue;
```

### DisabledSegmentAlpha

Sets the opacity of disabled segments (0.0 to 1.0).
- **0.0**: Completely transparent (invisible)
- **1.0**: Fully opaque

**Default Value:** Varies by platform

**XAML:**
```xml
<gauge:SfDigitalGauge Text="12345" 
                      DisabledSegmentAlpha="0.1" />
```

**C#:**
```csharp
digitalGauge.DisabledSegmentAlpha = 0.1;
```

### Combined Disabled Segment Styling

```xml
<gauge:SfDigitalGauge Text="12345" 
                      CharacterType="SevenSegment"
                      CharacterHeight="100"
                      CharacterWidth="65"
                      CharacterStroke="LimeGreen"
                      DisabledSegmentStroke="DarkGreen"
                      DisabledSegmentAlpha="0.15"
                      StrokeWidth="3"
                      BackgroundColor="Black" />
```

### Classic LED Effect

Create an authentic LED display appearance:

```xml
<gauge:SfDigitalGauge Text="88:88:88" 
                      CharacterType="SevenSegment"
                      CharacterHeight="120"
                      CharacterWidth="75"
                      CharacterStroke="Red"
                      DisabledSegmentStroke="DarkRed"
                      DisabledSegmentAlpha="0.1"
                      StrokeWidth="4"
                      BackgroundColor="#1A0000" />
```

### Disabled Segment Visibility Examples

```xml
<VerticalStackLayout Spacing="20">
    
    <Label Text="Hidden Disabled Segments (Alpha: 0)" />
    <gauge:SfDigitalGauge Text="123" 
                          DisabledSegmentAlpha="0"
                          CharacterType="SevenSegment"
                          CharacterHeight="80"
                          CharacterWidth="50" />
    
    <Label Text="Faint Disabled Segments (Alpha: 0.1)" />
    <gauge:SfDigitalGauge Text="123" 
                          DisabledSegmentAlpha="0.1"
                          DisabledSegmentStroke="Gray"
                          CharacterType="SevenSegment"
                          CharacterHeight="80"
                          CharacterWidth="50" />
    
    <Label Text="Visible Disabled Segments (Alpha: 0.3)" />
    <gauge:SfDigitalGauge Text="123" 
                          DisabledSegmentAlpha="0.3"
                          DisabledSegmentStroke="LightGray"
                          CharacterType="SevenSegment"
                          CharacterHeight="80"
                          CharacterWidth="50" />
    
</VerticalStackLayout>
```

## Background Color

Set the background color of the entire gauge using the standard `BackgroundColor` property.

### Basic Background

**XAML:**
```xml
<gauge:SfDigitalGauge Text="12345" 
                      BackgroundColor="Blue" />
```

**C#:**
```csharp
digitalGauge.BackgroundColor = Colors.Blue;
```

### Classic Dark Background

```xml
<gauge:SfDigitalGauge Text="12:30:45" 
                      CharacterType="SevenSegment"
                      CharacterHeight="100"
                      CharacterWidth="65"
                      CharacterStroke="Red"
                      BackgroundColor="Black"
                      StrokeWidth="3" />
```

### Themed Backgrounds

```csharp
public class ThemedGaugePage : ContentPage
{
    private SfDigitalGauge gauge;
    
    public ThemedGaugePage()
    {
        gauge = new SfDigitalGauge
        {
            Text = "THEME",
            CharacterType = DigitalGaugeCharacterType.SixteenSegment,
            CharacterHeight = 90,
            CharacterWidth = 70,
            StrokeWidth = 3
        };
        
        ApplyDarkTheme();
        
        this.Content = gauge;
    }
    
    private void ApplyDarkTheme()
    {
        gauge.BackgroundColor = Color.FromArgb("#1A1A1A");
        gauge.CharacterStroke = Color.FromArgb("#00FF00");
        gauge.DisabledSegmentStroke = Color.FromArgb("#003300");
        gauge.DisabledSegmentAlpha = 0.2;
    }
    
    private void ApplyLightTheme()
    {
        gauge.BackgroundColor = Color.FromArgb("#F5F5F5");
        gauge.CharacterStroke = Color.FromArgb("#333333");
        gauge.DisabledSegmentStroke = Color.FromArgb("#CCCCCC");
        gauge.DisabledSegmentAlpha = 0.3;
    }
    
    private void ApplyNeonTheme()
    {
        gauge.BackgroundColor = Colors.Black;
        gauge.CharacterStroke = Color.FromArgb("#FF00FF");
        gauge.DisabledSegmentStroke = Color.FromArgb("#330033");
        gauge.DisabledSegmentAlpha = 0.15;
    }
}
```

## Complete Styling Examples

### Classic Digital Clock

```xml
<gauge:SfDigitalGauge Text="12:30:45" 
                      CharacterType="SevenSegment"
                      CharacterHeight="120"
                      CharacterWidth="75"
                      CharacterSpacing="8"
                      CharacterStroke="Red"
                      StrokeWidth="5"
                      DisabledSegmentStroke="DarkRed"
                      DisabledSegmentAlpha="0.1"
                      BackgroundColor="Black" />
```

### Modern Status Display

```xml
<gauge:SfDigitalGauge Text="ONLINE" 
                      CharacterType="SixteenSegment"
                      CharacterHeight="90"
                      CharacterWidth="70"
                      CharacterSpacing="10"
                      CharacterStroke="LimeGreen"
                      StrokeWidth="3"
                      DisabledSegmentStroke="Gray"
                      DisabledSegmentAlpha="0.2"
                      BackgroundColor="#2A2A2A" />
```

### Retro Calculator Display

```xml
<gauge:SfDigitalGauge Text="000000" 
                      CharacterType="SevenSegment"
                      CharacterHeight="80"
                      CharacterWidth="50"
                      CharacterSpacing="5"
                      CharacterStroke="#32CD32"
                      StrokeWidth="2"
                      DisabledSegmentStroke="#0A3A0A"
                      DisabledSegmentAlpha="0.15"
                      BackgroundColor="#1A3A1A" />
```

### Industrial Panel Display

```xml
<gauge:SfDigitalGauge Text="TEMP: 72.5" 
                      CharacterType="EightCrossEightDotMatrix"
                      CharacterHeight="70"
                      CharacterWidth="60"
                      CharacterSpacing="6"
                      CharacterStroke="Orange"
                      StrokeWidth="2"
                      BackgroundColor="#333333" />
```

## Styling Best Practices

### Contrast and Visibility

1. **High Contrast**: Use contrasting colors between active segments and background
   ```csharp
   // Good contrast
   gauge.CharacterStroke = Colors.White;
   gauge.BackgroundColor = Colors.Black;
   ```

2. **Disabled Segment Visibility**: Keep disabled segments subtle but visible
   ```csharp
   gauge.DisabledSegmentAlpha = 0.1; // Subtle
   ```

3. **Color Combinations**: Classic combinations work best
   - Red on black
   - Green on dark green
   - Cyan on black
   - Orange on dark background

### Sizing Guidelines

1. **Readable from Distance**: Larger sizes for dashboards
   ```csharp
   gauge.CharacterHeight = 120;
   gauge.CharacterWidth = 80;
   ```

2. **Compact Displays**: Smaller sizes for dense UIs
   ```csharp
   gauge.CharacterHeight = 50;
   gauge.CharacterWidth = 35;
   ```

3. **Maintain Proportions**: Keep width 60-70% of height
   ```csharp
   gauge.CharacterHeight = 100;
   gauge.CharacterWidth = 65; // 65% ratio
   ```

### Performance Tips

1. **Avoid Frequent Updates**: Update text sparingly
2. **Reuse Instances**: Don't recreate gauges unnecessarily
3. **Appropriate Sizing**: Don't make gauges unnecessarily large
4. **Simple Backgrounds**: Solid colors perform better than gradients

## Responsive Design

### Adaptive Sizing

```csharp
public class ResponsiveDigitalGauge : ContentPage
{
    protected override void OnSizeAllocated(double width, double height)
    {
        base.OnSizeAllocated(width, height);
        
        if (Content is SfDigitalGauge gauge && width > 0)
        {
            // Scale based on available width
            double scaleFactor = width / 400.0; // Base width
            
            gauge.CharacterHeight = 80 * scaleFactor;
            gauge.CharacterWidth = 50 * scaleFactor;
            gauge.CharacterSpacing = 5 * scaleFactor;
            gauge.StrokeWidth = 2 * Math.Max(1, scaleFactor);
        }
    }
}
```

### Platform-Specific Adjustments

```csharp
private void ApplyPlatformStyles()
{
    if (DeviceInfo.Platform == DevicePlatform.Android)
    {
        gauge.StrokeWidth = 2;
    }
    else if (DeviceInfo.Platform == DevicePlatform.iOS)
    {
        gauge.StrokeWidth = 3;
    }
    else if (DeviceInfo.Platform == DevicePlatform.WinUI)
    {
        gauge.StrokeWidth = 2;
    }
}
```
