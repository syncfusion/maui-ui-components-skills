# Rating Shapes in .NET MAUI Rating

The `RatingShape` property allows you to customize the visual appearance of rating items. The `SfRating` control supports five predefined shapes and custom SVG paths for unlimited design possibilities.

## Available Rating Shapes

The Rating control provides the following shape options:

1. **Star** (default) - Classic five-pointed star
2. **Heart** - Heart symbol for favorites/likes
3. **Diamond** - Diamond/rhombus shape
4. **Circle** - Simple circular indicator
5. **Custom** - User-defined SVG path

> **Default:** `Star` shape

## RatingShape Enum

```csharp
public enum RatingShape
{
    Star,      // ★ Five-pointed star (default)
    Heart,     // ♥ Heart shape
    Diamond,   // ◆ Diamond/rhombus
    Circle,    // ● Circular shape
    Custom     // Custom SVG path
}
```

## Predefined Shapes

### Star Shape (Default)

The classic five-pointed star is the most recognizable rating symbol.

**Implementation:**

**XAML:**
```xml
<rating:SfRating RatingShape="Star" 
                 Value="4" 
                 ItemCount="5" />
```

**C#:**
```csharp
SfRating rating = new SfRating();
rating.RatingShape = RatingShape.Star;
rating.Value = 4;
```

**Best For:**
- General-purpose ratings
- Product reviews
- Quality assessments
- Default choice when unsure

### Heart Shape

The heart shape is perfect for expressing favorites, likes, or love.

**Implementation:**

**XAML:**
```xml
<rating:SfRating RatingShape="Heart" 
                 Value="3" 
                 ItemCount="5">
    <rating:SfRating.RatingSettings>
        <rating:RatingSettings RatedFill="Red" />
    </rating:SfRating.RatingSettings>
</rating:SfRating>
```

**C#:**
```csharp
SfRating rating = new SfRating();
rating.RatingShape = RatingShape.Heart;
rating.Value = 3;

RatingSettings settings = new RatingSettings();
settings.RatedFill = Colors.Red;
rating.RatingSettings = settings;
```

**Best For:**
- Favorite/like indicators
- Dating apps
- Social media reactions
- Emotional ratings
- Gift/wish list apps

### Diamond Shape

The diamond shape provides a unique, elegant rating indicator.

**Implementation:**

**XAML:**
```xml
<rating:SfRating RatingShape="Diamond" 
                 Value="4" 
                 ItemCount="5">
    <rating:SfRating.RatingSettings>
        <rating:RatingSettings RatedFill="#00CED1" />
    </rating:SfRating.RatingSettings>
</rating:SfRating>
```

**C#:**
```csharp
SfRating rating = new SfRating();
rating.RatingShape = RatingShape.Diamond;
rating.Value = 4;

RatingSettings settings = new RatingSettings();
settings.RatedFill = Color.FromArgb("#00CED1");
rating.RatingSettings = settings;
```

**Best For:**
- Luxury/premium products
- Jewelry ratings
- Achievement levels
- Distinction indicators
- Quality tiers

### Circle Shape

Simple circular indicators for minimal, modern designs.

**Implementation:**

**XAML:**
```xml
<rating:SfRating RatingShape="Circle" 
                 Value="3" 
                 ItemCount="5" />
```

**C#:**
```csharp
SfRating rating = new SfRating();
rating.RatingShape = RatingShape.Circle;
rating.Value = 3;
```

**Best For:**
- Minimalist designs
- Progress indicators
- Simple preference ratings
- Modern UI aesthetics
- Clean, uncluttered interfaces

## Custom Shapes

When predefined shapes don't meet your needs, use the `Custom` rating shape with SVG path data to create any design.

### How Custom Shapes Work

1. Set `RatingShape` to `Custom`
2. Provide SVG path data to the `Path` property
3. Ensure `ItemSize` matches your path's coordinate system

### Path Data Format

The `Path` property accepts standard SVG path commands:
- **M** = Move to
- **L** = Line to
- **C** = Cubic Bezier curve
- **Q** = Quadratic Bezier curve
- **Z** = Close path

### Example: Custom Bell Icon

**XAML:**
```xml
<rating:SfRating RatingShape="Custom" 
                 ItemSize="36"
                 Value="3"
                 Path="M17.5 35.5C19.9063 35.5 21.875 33.8846 21.875 31.9103H13.125C13.125 33.8846 15.0719 35.5 17.5 35.5ZM30.625 24.7308V15.7564C30.625 10.2462 27.0375 5.63334 20.7812 4.41282V3.19231C20.7812 1.70256 19.3156 0.5 17.5 0.5C15.6844 0.5 14.2188 1.70256 14.2188 3.19231V4.41282C7.94063 5.63334 4.375 10.2282 4.375 15.7564V24.7308L0 28.3205V30.1154H35V28.3205L30.625 24.7308Z" />
```

**C#:**
```csharp
SfRating rating = new SfRating();
rating.ItemSize = 36;
rating.RatingShape = RatingShape.Custom;
rating.Path = "M17.5 35.5C19.9063 35.5 21.875 33.8846 21.875 31.9103H13.125C13.125 33.8846 15.0719 35.5 17.5 35.5ZM30.625 24.7308V15.7564C30.625 10.2462 27.0375 5.63334 20.7812 4.41282V3.19231C20.7812 1.70256 19.3156 0.5 17.5 0.5C15.6844 0.5 14.2188 1.70256 14.2188 3.19231V4.41282C7.94063 5.63334 4.375 10.2282 4.375 15.7564V24.7308L0 28.3205V30.1154H35V28.3205L30.625 24.7308Z";
```

**Best For:**
- Notification ratings
- Alert importance levels
- Custom branding
- Unique app themes

### Example: Custom Thumbs Up

```xml
<rating:SfRating RatingShape="Custom" 
                 ItemSize="40"
                 Value="4"
                 Path="M21 8h-5.5c-.83 0-1.5-.67-1.5-1.5S14.67 5 15.5 5h4c.28 0 .5-.22.5-.5s-.22-.5-.5-.5h-4c-1.38 0-2.5 1.12-2.5 2.5 0 .57.19 1.09.51 1.51L7 11v13h13.5c1.93 0 3.5-1.57 3.5-3.5V8zm-2 10.5c0 .83-.67 1.5-1.5 1.5H9V12.9l6.4-3.4h.1v9z" />
```

**Best For:**
- Like/dislike ratings
- Approval indicators
- Feedback mechanisms
- Social voting

### Creating Custom Paths

**Tools for generating SVG paths:**
1. **Figma** - Export as SVG, extract path data
2. **Adobe Illustrator** - Export SVG, copy path
3. **Inkscape** - Free, open-source SVG editor
4. **SVG Path Editor** - Online tools like svg-path-editor.com

**Steps:**
1. Design your icon in an SVG editor
2. Export as SVG
3. Open the SVG file in a text editor
4. Copy the `d` attribute value from the `<path>` element
5. Paste into the `Path` property

### Path Sizing Guidelines

> **Important:** The `ItemSize` should match the path's coordinate system for proper display.

If your SVG path has dimensions 35x35:
```xml
<rating:SfRating RatingShape="Custom" 
                 ItemSize="35"  <!-- Match path dimensions -->
                 Path="..." />
```

Common path dimensions:
- **24x24** - Material Design icons
- **32x32** - Standard icon size
- **48x48** - Larger icons
- **Custom** - Match your design

## Shape Comparison Examples

### Side-by-Side Comparison

```xml
<VerticalStackLayout Padding="20" Spacing="20">
    
    <!-- Star Shape -->
    <StackLayout Spacing="5">
        <Label Text="Star (Default)" FontAttributes="Bold" />
        <rating:SfRating RatingShape="Star" Value="4" ItemSize="35" />
    </StackLayout>
    
    <!-- Heart Shape -->
    <StackLayout Spacing="5">
        <Label Text="Heart" FontAttributes="Bold" />
        <rating:SfRating RatingShape="Heart" Value="4" ItemSize="35">
            <rating:SfRating.RatingSettings>
                <rating:RatingSettings RatedFill="Red" />
            </rating:SfRating.RatingSettings>
        </rating:SfRating>
    </StackLayout>
    
    <!-- Diamond Shape -->
    <StackLayout Spacing="5">
        <Label Text="Diamond" FontAttributes="Bold" />
        <rating:SfRating RatingShape="Diamond" Value="4" ItemSize="35">
            <rating:SfRating.RatingSettings>
                <rating:RatingSettings RatedFill="DeepSkyBlue" />
            </rating:SfRating.RatingSettings>
        </rating:SfRating>
    </StackLayout>
    
    <!-- Circle Shape -->
    <StackLayout Spacing="5">
        <Label Text="Circle" FontAttributes="Bold" />
        <rating:SfRating RatingShape="Circle" Value="4" ItemSize="35" />
    </StackLayout>
    
    <!-- Custom Shape -->
    <StackLayout Spacing="5">
        <Label Text="Custom (Bell)" FontAttributes="Bold" />
        <rating:SfRating RatingShape="Custom" Value="4" ItemSize="36"
                         Path="M17.5 35.5C19.9063 35.5 21.875 33.8846 21.875 31.9103H13.125C13.125 33.8846 15.0719 35.5 17.5 35.5ZM30.625 24.7308V15.7564C30.625 10.2462 27.0375 5.63334 20.7812 4.41282V3.19231C20.7812 1.70256 19.3156 0.5 17.5 0.5C15.6844 0.5 14.2188 1.70256 14.2188 3.19231V4.41282C7.94063 5.63334 4.375 10.2282 4.375 15.7564V24.7308L0 28.3205V30.1154H35V28.3205L30.625 24.7308Z" />
    </StackLayout>
    
</VerticalStackLayout>
```

## Complete Styled Examples

### Example 1: Heart Rating with Custom Colors

```xml
<rating:SfRating RatingShape="Heart" 
                 Value="5" 
                 ItemCount="5"
                 ItemSize="50"
                 ItemSpacing="8">
    <rating:SfRating.RatingSettings>
        <rating:RatingSettings RatedFill="#FF1744"
                               UnratedFill="#FFEBEE"
                               RatedStroke="#C51162"
                               RatedStrokeThickness="2" />
    </rating:SfRating.RatingSettings>
</rating:SfRating>
```

### Example 2: Diamond Luxury Rating

```xml
<rating:SfRating RatingShape="Diamond" 
                 Value="4" 
                 ItemCount="5"
                 ItemSize="45"
                 ItemSpacing="10">
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

### Example 3: Minimalist Circle Rating

```xml
<rating:SfRating RatingShape="Circle" 
                 Value="3" 
                 ItemCount="5"
                 ItemSize="30"
                 ItemSpacing="15">
    <rating:SfRating.RatingSettings>
        <rating:RatingSettings RatedFill="#2196F3"
                               UnratedFill="Transparent"
                               UnratedStroke="#90CAF9"
                               UnratedStrokeThickness="2" />
    </rating:SfRating.RatingSettings>
</rating:SfRating>
```

## Shape Selection Guide

| Shape | Personality | Industry | User Familiarity |
|-------|-------------|----------|------------------|
| **Star** | Universal, trustworthy | E-commerce, general | Very High |
| **Heart** | Emotional, personal | Social, dating, gifts | High |
| **Diamond** | Premium, elegant | Luxury, jewelry | Medium |
| **Circle** | Modern, minimal | Tech, apps | Medium |
| **Custom** | Unique, branded | Varies | Depends |

### When to Choose Each Shape

**Choose Star when:**
- Building standard rating systems
- Following industry conventions
- Maximum user familiarity is important
- Uncertain which shape to use

**Choose Heart when:**
- Rating involves emotions or preferences
- Building social features (likes, favorites)
- App targets personal connections
- Expressing love/affection/favorites

**Choose Diamond when:**
- Premium or luxury brand
- Distinguishing tiers/levels
- Elegant, sophisticated aesthetic
- Rating high-value items

**Choose Circle when:**
- Minimalist design philosophy
- Modern, clean interface
- Simple progress/level indicators
- Technical or professional apps

**Choose Custom when:**
- Unique branding requirements
- Specific domain needs (e.g., thumbs, flags, emojis)
- Standing out from competitors
- Matching existing design system

## Combining Shapes with Other Features

### Half-Precision Heart Rating

```xml
<rating:SfRating RatingShape="Heart" 
                 Value="3.5" 
                 Precision="Half"
                 ItemCount="5">
    <rating:SfRating.RatingSettings>
        <rating:RatingSettings RatedFill="Pink" />
    </rating:SfRating.RatingSettings>
</rating:SfRating>
```

### Read-Only Custom Shape Display

```xml
<rating:SfRating RatingShape="Custom" 
                 Value="4"
                 IsReadOnly="True"
                 ItemSize="36"
                 Path="..." />
```

## Best Practices

1. **Match Your Brand**: Choose shapes that align with your app's personality and brand identity
2. **Consider Context**: Hearts for favorites, stars for quality, circles for progress
3. **Maintain Consistency**: Use the same shape throughout your app for the same type of rating
4. **Size Appropriately**: Ensure shapes are large enough to be tapped accurately (minimum 35-40 pixels)
5. **Test Custom Paths**: Verify custom shapes render correctly on all target platforms
6. **Provide Visual Feedback**: Combine shapes with appropriate colors for clear rated/unrated distinction

## Troubleshooting

### Custom Shape Not Displaying
- **Solution**: Verify `RatingShape="Custom"` is set
- **Solution**: Check that `Path` property contains valid SVG path data
- **Solution**: Ensure `ItemSize` matches the path coordinate system

### Custom Shape Looks Distorted
- **Solution**: Match `ItemSize` to your SVG path's viewBox dimensions
- **Solution**: Verify path uses absolute coordinates, not relative

### Shape Appears Too Small/Large
- **Solution**: Adjust `ItemSize` property
- **Solution**: For custom shapes, scale your SVG path data proportionally

## Next Steps

- **Appearance Styling**: Customize colors, strokes, and spacing → [appearance-styling.md](appearance-styling.md)
- **Interactive Features**: Handle events and user interactions → [interactive-features.md](interactive-features.md)
- **Precision Modes**: Learn about rating accuracy options → [precision-modes.md](precision-modes.md)
