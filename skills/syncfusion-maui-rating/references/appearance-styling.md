# Appearance and Styling in .NET MAUI Rating

The `SfRating` control provides extensive customization options for appearance and styling. You can control sizes, spacing, colors, strokes, and backgrounds to match your app's design perfectly.

## Table of Contents
- [Size and Spacing Configuration](#size-and-spacing-configuration)
- [RatingSettings Overview](#ratingsettings-overview)
- [Fill Colors](#fill-colors)
- [Stroke Configuration](#stroke-configuration)
- [Background Color](#background-color)
- [Complete Styling Examples](#complete-styling-examples)
- [Design Best Practices](#design-best-practices)

## Size and Spacing Configuration

### ItemSize Property

The `ItemSize` property controls the size of each rating item in device-independent units.

> **Default:** 50

**XAML:**
```xml
<rating:SfRating ItemSize="40" />
```

**C#:**
```csharp
SfRating rating = new SfRating();
rating.ItemSize = 40;
```

**Common Sizes:**
- **Small (30-35)**: Compact layouts, list items
- **Medium (40-50)**: Default, balanced size
- **Large (55-70)**: Emphasis, hero sections
- **Extra Large (75+)**: Marketing, showcase

**Example with Different Sizes:**
```xml
<VerticalStackLayout Spacing="15">
    <Label Text="Small (30px)" />
    <rating:SfRating ItemSize="30" Value="4" />
    
    <Label Text="Medium (45px)" />
    <rating:SfRating ItemSize="45" Value="4" />
    
    <Label Text="Large (60px)" />
    <rating:SfRating ItemSize="60" Value="4" />
</VerticalStackLayout>
```

### ItemSpacing Property

The `ItemSpacing` property sets the horizontal space between rating items.

> **Default:** 5

**XAML:**
```xml
<rating:SfRating ItemSpacing="10" />
```

**C#:**
```csharp
SfRating rating = new SfRating();
rating.ItemSpacing = 10;
```

**Spacing Guidelines:**
- **Tight (2-5)**: Compact, space-constrained layouts
- **Normal (5-10)**: Default, comfortable spacing
- **Loose (10-15)**: Spacious, easy touch targets
- **Very Loose (15+)**: Emphasis on individual items

**Example:**
```xml
<VerticalStackLayout Spacing="15">
    <Label Text="Tight Spacing (3px)" />
    <rating:SfRating ItemSpacing="3" Value="4" ItemSize="40" />
    
    <Label Text="Normal Spacing (8px)" />
    <rating:SfRating ItemSpacing="8" Value="4" ItemSize="40" />
    
    <Label Text="Loose Spacing (15px)" />
    <rating:SfRating ItemSpacing="15" Value="4" ItemSize="40" />
</VerticalStackLayout>
```

### ItemCount Property

The `ItemCount` property determines the number of rating items displayed.

> **Default:** 5

**XAML:**
```xml
<rating:SfRating ItemCount="5" />
```

**C#:**
```csharp
SfRating rating = new SfRating();
rating.ItemCount = 5;
```

**Common Counts:**
- **3 items**: Simple (Low/Medium/High)
- **5 items**: Standard (Most common)
- **7 items**: Detailed scale
- **10 items**: Precise measurements

## RatingSettings Overview

The `RatingSettings` class provides comprehensive styling options for both rated and unrated items.

### RatingSettings Structure

```csharp
public class RatingSettings
{
    public Color RatedFill { get; set; }           // Fill color for rated items
    public Color UnratedFill { get; set; }         // Fill color for unrated items
    public Color RatedStroke { get; set; }         // Stroke color for rated items
    public Color UnratedStroke { get; set; }       // Stroke color for unrated items
    public double RatedStrokeThickness { get; set; }    // Stroke thickness for rated
    public double UnratedStrokeThickness { get; set; }  // Stroke thickness for unrated
}
```

### Basic RatingSettings Usage

**XAML:**
```xml
<rating:SfRating Value="3">
    <rating:SfRating.RatingSettings>
        <rating:RatingSettings RatedFill="Gold"
                               UnratedFill="LightGray"
                               RatedStroke="Orange"
                               UnratedStroke="Gray"
                               RatedStrokeThickness="2"
                               UnratedStrokeThickness="1" />
    </rating:SfRating.RatingSettings>
</rating:SfRating>
```

**C#:**
```csharp
SfRating rating = new SfRating();
rating.Value = 3;

RatingSettings settings = new RatingSettings
{
    RatedFill = Colors.Gold,
    UnratedFill = Colors.LightGray,
    RatedStroke = Colors.Orange,
    UnratedStroke = Colors.Gray,
    RatedStrokeThickness = 2,
    UnratedStrokeThickness = 1
};

rating.RatingSettings = settings;
```

## Fill Colors

Fill colors determine the interior color of rating items.

### RatedFill Property

Sets the fill color for rated (selected) items.

**XAML:**
```xml
<rating:SfRating Value="4">
    <rating:SfRating.RatingSettings>
        <rating:RatingSettings RatedFill="#FFD700" />
    </rating:SfRating.RatingSettings>
</rating:SfRating>
```

**C#:**
```csharp
SfRating rating = new SfRating();
rating.Value = 4;

RatingSettings settings = new RatingSettings();
settings.RatedFill = Color.FromArgb("#FFD700"); // Gold
rating.RatingSettings = settings;
```

**Popular RatedFill Colors:**
```xml
<!-- Gold (Classic) -->
<rating:RatingSettings RatedFill="#FFD700" />

<!-- Orange (Warm) -->
<rating:RatingSettings RatedFill="#FF9800" />

<!-- Red (Emphasis) -->
<rating:RatingSettings RatedFill="#F44336" />

<!-- Blue (Professional) -->
<rating:RatingSettings RatedFill="#2196F3" />

<!-- Green (Positive) -->
<rating:RatingSettings RatedFill="#4CAF50" />
```

### UnratedFill Property

Sets the fill color for unrated (unselected) items.

**XAML:**
```xml
<rating:SfRating Value="3">
    <rating:SfRating.RatingSettings>
        <rating:RatingSettings UnratedFill="#E0E0E0" />
    </rating:SfRating.RatingSettings>
</rating:SfRating>
```

**C#:**
```csharp
RatingSettings settings = new RatingSettings();
settings.UnratedFill = Color.FromArgb("#E0E0E0"); // Light Gray
rating.RatingSettings = settings;
```

**Common Approaches:**
- **Light Gray**: Subtle, professional
- **Transparent**: Outline-only style
- **Very Light Color**: Tinted appearance
- **White**: High contrast on dark backgrounds

**Example Combinations:**
```xml
<!-- Gold & Light Gray (Traditional) -->
<rating:RatingSettings RatedFill="#FFD700" UnratedFill="#E0E0E0" />

<!-- Red & Pink (Hearts) -->
<rating:RatingSettings RatedFill="#F44336" UnratedFill="#FFEBEE" />

<!-- Blue & Light Blue (Cool) -->
<rating:RatingSettings RatedFill="#2196F3" UnratedFill="#E3F2FD" />

<!-- Outline Only (Transparent Unrated) -->
<rating:RatingSettings RatedFill="#FF9800" UnratedFill="Transparent" />
```

## Stroke Configuration

Strokes add outlines to rating items for enhanced visibility and style.

### RatedStroke Property

Sets the stroke (border) color for rated items.

**XAML:**
```xml
<rating:SfRating Value="3">
    <rating:SfRating.RatingSettings>
        <rating:RatingSettings RatedStroke="#FFA500" />
    </rating:SfRating.RatingSettings>
</rating:SfRating>
```

**C#:**
```csharp
RatingSettings settings = new RatingSettings();
settings.RatedStroke = Color.FromArgb("#FFA500"); // Orange
rating.RatingSettings = settings;
```

### UnratedStroke Property

Sets the stroke color for unrated items.

**XAML:**
```xml
<rating:SfRating Value="3">
    <rating:SfRating.RatingSettings>
        <rating:RatingSettings UnratedStroke="#9E9E9E" />
    </rating:SfRating.RatingSettings>
</rating:SfRating>
```

**C#:**
```csharp
RatingSettings settings = new RatingSettings();
settings.UnratedStroke = Color.FromArgb("#9E9E9E"); // Gray
rating.RatingSettings = settings;
```

### Stroke Thickness

Control the width of the stroke outline.

#### RatedStrokeThickness Property

**XAML:**
```xml
<rating:SfRating Value="3">
    <rating:SfRating.RatingSettings>
        <rating:RatingSettings RatedStroke="#FFA500"
                               RatedStrokeThickness="3" />
    </rating:SfRating.RatingSettings>
</rating:SfRating>
```

**C#:**
```csharp
RatingSettings settings = new RatingSettings();
settings.RatedStroke = Color.FromArgb("#FFA500");
settings.RatedStrokeThickness = 3;
rating.RatingSettings = settings;
```

#### UnratedStrokeThickness Property

**XAML:**
```xml
<rating:SfRating Value="3">
    <rating:SfRating.RatingSettings>
        <rating:RatingSettings UnratedStroke="#9E9E9E"
                               UnratedStrokeThickness="2" />
    </rating:SfRating.RatingSettings>
</rating:SfRating>
```

**C#:**
```csharp
RatingSettings settings = new RatingSettings();
settings.UnratedStroke = Color.FromArgb("#9E9E9E");
settings.UnratedStrokeThickness = 2;
rating.RatingSettings = settings;
```

**Thickness Guidelines:**
- **1-2**: Subtle outline
- **2-3**: Standard, visible border
- **3-5**: Bold, emphasized outline
- **5+**: Very thick, decorative border

### Stroke Style Examples

**Example 1: Outline Only Style**
```xml
<rating:SfRating Value="3" ItemSize="50">
    <rating:SfRating.RatingSettings>
        <rating:RatingSettings RatedFill="Transparent"
                               UnratedFill="Transparent"
                               RatedStroke="#FF9800"
                               UnratedStroke="#E0E0E0"
                               RatedStrokeThickness="3"
                               UnratedStrokeThickness="2" />
    </rating:SfRating.RatingSettings>
</rating:SfRating>
```

**Example 2: Filled with Contrasting Stroke**
```xml
<rating:SfRating Value="4" ItemSize="45">
    <rating:SfRating.RatingSettings>
        <rating:RatingSettings RatedFill="#FFD700"
                               UnratedFill="#F5F5F5"
                               RatedStroke="#FF6F00"
                               UnratedStroke="#BDBDBD"
                               RatedStrokeThickness="2"
                               UnratedStrokeThickness="1" />
    </rating:SfRating.RatingSettings>
</rating:SfRating>
```

**Example 3: Bold Emphasis**
```xml
<rating:SfRating Value="5" ItemSize="55">
    <rating:SfRating.RatingSettings>
        <rating:RatingSettings RatedFill="#4CAF50"
                               UnratedFill="#E8F5E9"
                               RatedStroke="#2E7D32"
                               UnratedStroke="#A5D6A7"
                               RatedStrokeThickness="4"
                               UnratedStrokeThickness="2" />
    </rating:SfRating.RatingSettings>
</rating:SfRating>
```

## Background Color

Set the background color for the entire rating control.

**XAML:**
```xml
<rating:SfRating Value="3" 
                 BackgroundColor="#F5F5F5"
                 Padding="10" />
```

**C#:**
```csharp
SfRating rating = new SfRating();
rating.Value = 3;
rating.BackgroundColor = Color.FromArgb("#F5F5F5");
```

> **Default:** Transparent

**Use Cases:**
- Highlight rating area on cards
- Distinguish rating section
- Provide contrast on images
- Match container backgrounds

## Complete Styling Examples

### Example 1: Classic Gold Rating

```xml
<rating:SfRating Value="4"
                 ItemCount="5"
                 ItemSize="50"
                 ItemSpacing="8">
    <rating:SfRating.RatingSettings>
        <rating:RatingSettings RatedFill="#FFD700"
                               UnratedFill="#E0E0E0"
                               RatedStroke="#FFA500"
                               UnratedStroke="#BDBDBD"
                               RatedStrokeThickness="2"
                               UnratedStrokeThickness="1" />
    </rating:SfRating.RatingSettings>
</rating:SfRating>
```

### Example 2: Modern Minimalist

```xml
<rating:SfRating Value="3.5"
                 ItemCount="5"
                 ItemSize="40"
                 ItemSpacing="12"
                 Precision="Half"
                 RatingShape="Circle">
    <rating:SfRating.RatingSettings>
        <rating:RatingSettings RatedFill="#2196F3"
                               UnratedFill="Transparent"
                               UnratedStroke="#90CAF9"
                               UnratedStrokeThickness="2" />
    </rating:SfRating.RatingSettings>
</rating:SfRating>
```

### Example 3: Heart Favorites (Red/Pink)

```xml
<rating:SfRating Value="4"
                 ItemCount="5"
                 ItemSize="45"
                 ItemSpacing="10"
                 RatingShape="Heart">
    <rating:SfRating.RatingSettings>
        <rating:RatingSettings RatedFill="#F44336"
                               UnratedFill="#FFEBEE"
                               RatedStroke="#C62828"
                               RatedStrokeThickness="2" />
    </rating:SfRating.RatingSettings>
</rating:SfRating>
```

### Example 4: Luxury Diamond Rating

```xml
<rating:SfRating Value="5"
                 ItemCount="5"
                 ItemSize="50"
                 ItemSpacing="15"
                 RatingShape="Diamond">
    <rating:SfRating.RatingSettings>
        <rating:RatingSettings RatedFill="#B9F6CA"
                               UnratedFill="#E8F5E9"
                               RatedStroke="#00C853"
                               UnratedStroke="#81C784"
                               RatedStrokeThickness="3"
                               UnratedStrokeThickness="1" />
    </rating:SfRating.RatingSettings>
</rating:SfRating>
```

### Example 5: Dark Theme Rating

```xml
<rating:SfRating Value="3"
                 ItemCount="5"
                 ItemSize="45"
                 ItemSpacing="8"
                 BackgroundColor="#212121"
                 Padding="15">
    <rating:SfRating.RatingSettings>
        <rating:RatingSettings RatedFill="#FFD54F"
                               UnratedFill="#424242"
                               RatedStroke="#FFC107"
                               UnratedStroke="#757575"
                               RatedStrokeThickness="2"
                               UnratedStrokeThickness="1" />
    </rating:SfRating.RatingSettings>
</rating:SfRating>
```

### Example 6: Outline-Only Style

```xml
<rating:SfRating Value="4"
                 ItemCount="5"
                 ItemSize="55"
                 ItemSpacing="10">
    <rating:SfRating.RatingSettings>
        <rating:RatingSettings RatedFill="Transparent"
                               UnratedFill="Transparent"
                               RatedStroke="#FF5722"
                               UnratedStroke="#BCAAA4"
                               RatedStrokeThickness="4"
                               UnratedStrokeThickness="2" />
    </rating:SfRating.RatingSettings>
</rating:SfRating>
```

## Design Best Practices

### Color Contrast

1. **Ensure Sufficient Contrast**: Rated items should be clearly distinguishable from unrated items
2. **Consider Accessibility**: Aim for WCAG AA contrast ratio (4.5:1 minimum)
3. **Test on Different Backgrounds**: Verify visibility on light and dark themes
4. **Use Strokes for Definition**: Add strokes to improve shape recognition

### Color Psychology

| Color | Meaning | Best For |
|-------|---------|----------|
| **Gold/Yellow** | Excellence, quality | Product ratings, awards |
| **Red** | Love, passion, urgency | Hearts, favorites, emphasis |
| **Green** | Success, positive | Approval, eco-friendly |
| **Blue** | Trust, professional | Business apps, corporate |
| **Orange** | Energy, enthusiasm | Creative, fun apps |
| **Purple** | Luxury, premium | High-end products |

### Size and Spacing

1. **Touch Targets**: Ensure items are at least 44x44 for easy tapping (iOS) or 48x48 (Android)
2. **Comfortable Spacing**: Use 8-12px spacing for balanced layouts
3. **Context-Appropriate Size**: Larger for hero sections, smaller for lists
4. **Responsive Sizing**: Consider screen size and density

### Consistency

1. **Unified Theme**: Match rating colors to your app's color palette
2. **Consistent Sizing**: Use same ItemSize throughout app for same rating type
3. **Shape Selection**: Stick with one shape per rating context
4. **Standard Patterns**: Follow platform conventions when appropriate

### Performance

1. **Avoid Complex Custom Paths**: Simple shapes perform better
2. **Limit Stroke Thickness**: Very thick strokes may impact rendering
3. **Use Solid Colors**: Gradients are not directly supported
4. **Test on Target Devices**: Verify performance on lower-end devices

## Dynamic Styling with Data Binding

```xml
<rating:SfRating Value="{Binding CurrentRating}"
                 ItemSize="{Binding RatingSize}">
    <rating:SfRating.RatingSettings>
        <rating:RatingSettings RatedFill="{Binding RatedColor}"
                               UnratedFill="{Binding UnratedColor}" />
    </rating:SfRating.RatingSettings>
</rating:SfRating>
```

```csharp
public class RatingViewModel : INotifyPropertyChanged
{
    private double _currentRating = 4.0;
    private Color _ratedColor = Colors.Gold;
    
    public double CurrentRating
    {
        get => _currentRating;
        set { _currentRating = value; OnPropertyChanged(); }
    }
    
    public Color RatedColor
    {
        get => _ratedColor;
        set { _ratedColor = value; OnPropertyChanged(); }
    }
    
    // Additional properties and INotifyPropertyChanged implementation
}
```

## Troubleshooting

### Colors Not Appearing
- **Solution**: Ensure RatingSettings is properly assigned to the Rating control
- **Solution**: Check color format (hex, named colors, Color.FromArgb)
- **Solution**: Verify properties are set on RatedFill/UnratedFill, not Fill

### Strokes Not Visible
- **Solution**: Set StrokeThickness > 0
- **Solution**: Ensure stroke color contrasts with fill color
- **Solution**: Check that Stroke properties are set, not just Fill

### Custom Colors Look Different on Platforms
- **Solution**: Use consistent color formats (hex or RGBA)
- **Solution**: Test on all target platforms
- **Solution**: Be aware of platform-specific rendering differences

## Next Steps

- **Interactive Features**: Handle events and user interactions → [interactive-features.md](interactive-features.md)
- **Rating Shapes**: Explore different visual shapes → [rating-shapes.md](rating-shapes.md)
- **Precision Modes**: Learn about rating accuracy → [precision-modes.md](precision-modes.md)
