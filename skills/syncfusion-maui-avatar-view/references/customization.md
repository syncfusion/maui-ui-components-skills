# Customization in .NET MAUI Avatar View

The SfAvatarView control provides extensive customization options for appearance, including aspect ratios, colors, gradients, sizing, and font properties.

## Table of Contents

- [Aspect Ratio Control](#aspect-ratio-control)
- [Color Customization](#color-customization)
  - [Stroke Color](#stroke-color)
  - [Background Color](#background-color)
  - [Automatic Color Modes](#automatic-color-modes)
  - [Gradient Backgrounds](#gradient-backgrounds)
- [Sizing Properties](#sizing-properties)
  - [Width and Height](#width-and-height)
  - [Stroke Thickness](#stroke-thickness)
  - [Corner Radius](#corner-radius)
  - [Content Padding](#content-padding)
- [Font Customization](#font-customization)
  - [Font Size](#font-size)
  - [Font Family](#font-family)
  - [Font Attributes](#font-attributes)
  - [Font Auto-Scaling](#font-auto-scaling)
- [Best Practices](#best-practices)
- [Common Customization Patterns](#common-customization-patterns)

## Aspect Ratio Control

The `Aspect` property controls how images fit within the avatar view bounds. This is particularly important for Custom image content types.

### Available Aspect Modes

| Mode | Behavior | Use Case |
|------|----------|----------|
| **AspectFit** | Fits entire image, adds space if needed | Preserve full image visibility |
| **AspectFill** | Fills display, clips image while preserving aspect | Common for profile pictures |
| **Fill** | Stretches to fill display | May cause distortion, use carefully |
| **Center** | Centers image at original size | Small logos or icons |

### Implementation

**XAML:**
```xaml
<sfavatar:SfAvatarView 
    ContentType="Custom"
    ImageSource="person.png"
    Aspect="AspectFit"
    AvatarShape="Circle"
    WidthRequest="80"
    HeightRequest="80"
    StrokeThickness="1"
    Stroke="Black"
    HorizontalOptions="Center"
    VerticalOptions="Center" />
```

**C#:**
```csharp
var avatarView = new SfAvatarView
{
    ContentType = ContentType.Custom,
    ImageSource = "person.png",
    Aspect = Aspect.AspectFit,
    AvatarShape = AvatarShape.Circle,
    WidthRequest = 80,
    HeightRequest = 80,
    Stroke = Colors.Black,
    StrokeThickness = 1,
    HorizontalOptions = LayoutOptions.Center,
    VerticalOptions = LayoutOptions.Center
};
```

### Choosing the Right Aspect

**AspectFit** - Best for:
- Images with important edges (logos, icons)
- When you can't risk cropping content
- Non-square images in circular avatars

**AspectFill** (Default) - Best for:
- Profile photos
- Standard avatars
- When slight cropping is acceptable

**Fill** - Use sparingly:
- Only when aspect ratio matches exactly
- Background patterns
- Decorative elements

**Center** - Best for:
- Small icons or badges
- Images smaller than avatar size
- Precise positioning needs

## Color Customization

### Stroke Color

The `Stroke` property defines the border color around the avatar.

**XAML:**
```xaml
<sfavatar:SfAvatarView 
    ContentType="Custom"
    ImageSource="user.png"
    AvatarShape="Circle"
    AvatarSize="Large"
    Stroke="Red"
    StrokeThickness="2"
    HorizontalOptions="Center"
    VerticalOptions="Center" />
```

**C#:**
```csharp
var avatarView = new SfAvatarView
{
    ContentType = ContentType.Custom,
    ImageSource = "user.png",
    AvatarShape = AvatarShape.Circle,
    AvatarSize = AvatarSize.Large,
    Stroke = Colors.Red,
    StrokeThickness = 2,
    HorizontalOptions = LayoutOptions.Center,
    VerticalOptions = LayoutOptions.Center
};
```

**Common Stroke Colors:**
```csharp
// Subtle border
avatar.Stroke = Colors.LightGray;

// Accent color
avatar.Stroke = Color.FromArgb("#007AFF");

// Status indicators
avatar.Stroke = Colors.Green;  // Online
avatar.Stroke = Colors.Orange; // Away
avatar.Stroke = Colors.Gray;   // Offline
```

### Background Color

Set the background color using `Background` or `BackgroundColor` properties.

#### Using Background Property

**XAML:**
```xaml
<sfavatar:SfAvatarView 
    ContentType="Initials"
    AvatarName="Alex"
    AvatarShape="Circle"
    AvatarSize="Large"
    Background="Bisque"
    InitialsColor="Black"
    StrokeThickness="1"
    Stroke="Black" />
```

**C#:**
```csharp
var avatarView = new SfAvatarView
{
    ContentType = ContentType.Initials,
    AvatarName = "Alex",
    AvatarShape = AvatarShape.Circle,
    AvatarSize = AvatarSize.Large,
    Background = Colors.Bisque,
    InitialsColor = Colors.Black,
    Stroke = Colors.Black,
    StrokeThickness = 1
};
```

#### Using BackgroundColor Property

**XAML:**
```xaml
<sfavatar:SfAvatarView 
    ContentType="Initials"
    AvatarName="Alex"
    BackgroundColor="Bisque"
    InitialsColor="Black"
    AvatarColorMode="Default" />
```

**C#:**
```csharp
var avatarView = new SfAvatarView
{
    ContentType = ContentType.Initials,
    AvatarName = "Alex",
    BackgroundColor = Colors.Bisque,
    InitialsColor = Colors.Black,
    AvatarColorMode = AvatarColorMode.Default
};
```

**Note:** When using explicit background colors, set `AvatarColorMode` to `Default`.

### Automatic Color Modes

The `AvatarColorMode` property provides automatic color schemes for initials avatars.

#### Default Color Mode

Uses the explicitly set Background or BackgroundColor:

```csharp
var avatarView = new SfAvatarView
{
    ContentType = ContentType.Initials,
    AvatarName = "Sarah",
    AvatarColorMode = AvatarColorMode.Default,
    Background = Colors.Purple,
    InitialsColor = Colors.White
};
```

#### Dark Background Mode

Applies dark tones to both background and text automatically:

**XAML:**
```xaml
<sfavatar:SfAvatarView 
    ContentType="Initials"
    InitialsType="DoubleCharacter"
    AvatarName="Alex"
    AvatarShape="Circle"
    AvatarSize="Large"
    AvatarColorMode="DarkBackground"
    StrokeThickness="1"
    Stroke="Black"
    HorizontalOptions="Center"
    VerticalOptions="Center" />
```

**C#:**
```csharp
var avatarView = new SfAvatarView
{
    ContentType = ContentType.Initials,
    InitialsType = InitialsType.DoubleCharacter,
    AvatarName = "Alex",
    AvatarShape = AvatarShape.Circle,
    AvatarSize = AvatarSize.Large,
    AvatarColorMode = AvatarColorMode.DarkBackground,
    Stroke = Colors.Black,
    StrokeThickness = 1,
    HorizontalOptions = LayoutOptions.Center,
    VerticalOptions = LayoutOptions.Center
};
```

**Result:** Dark background with light text for high contrast.

#### Light Background Mode

Applies light tones to both background and text automatically:

**XAML:**
```xaml
<sfavatar:SfAvatarView 
    ContentType="Initials"
    InitialsType="DoubleCharacter"
    AvatarName="Alex"
    AvatarShape="Circle"
    AvatarSize="Large"
    AvatarColorMode="LightBackground"
    Stroke="Black"
    StrokeThickness="1"
    HorizontalOptions="Center"
    VerticalOptions="Center" />
```

**C#:**
```csharp
var avatarView = new SfAvatarView
{
    ContentType = ContentType.Initials,
    InitialsType = InitialsType.DoubleCharacter,
    AvatarName = "Alex",
    AvatarShape = AvatarShape.Circle,
    AvatarSize = AvatarSize.Large,
    AvatarColorMode = AvatarColorMode.LightBackground,
    Stroke = Colors.Black,
    StrokeThickness = 1,
    HorizontalOptions = LayoutOptions.Center,
    VerticalOptions = LayoutOptions.Center
};
```

**Result:** Light background with dark text for softer appearance.

### Gradient Backgrounds

Use `LinearGradientBrush` to create gradient backgrounds.

**XAML:**
```xaml
<sfavatar:SfAvatarView 
    ContentType="Initials"
    AvatarName="Alex"
    InitialsType="DoubleCharacter"
    AvatarShape="Circle"
    AvatarSize="Large"
    StrokeThickness="1"
    Stroke="Black"
    HorizontalOptions="Center"
    VerticalOptions="Center">
    <sfavatar:SfAvatarView.Background>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,0">
            <GradientStop Color="#2F9BDF" Offset="0"/>
            <GradientStop Color="#51F1F2" Offset="1"/>
        </LinearGradientBrush>
    </sfavatar:SfAvatarView.Background>
</sfavatar:SfAvatarView>
```

**C#:**
```csharp
var avatarView = new SfAvatarView
{
    ContentType = ContentType.Initials,
    InitialsType = InitialsType.DoubleCharacter,
    AvatarName = "Alex",
    AvatarShape = AvatarShape.Circle,
    AvatarSize = AvatarSize.Large,
    Stroke = Colors.Black,
    StrokeThickness = 1,
    Background = new LinearGradientBrush
    {
        StartPoint = new Point(0, 0),
        EndPoint = new Point(1, 0),
        GradientStops = new GradientStopCollection
        {
            new GradientStop { Color = Color.FromArgb("#2F9BDF"), Offset = 0 },
            new GradientStop { Color = Color.FromArgb("#51F1F2"), Offset = 1 }
        }
    }
};
```

#### Gradient Directions

**Horizontal (Left to Right):**
```csharp
StartPoint = new Point(0, 0)
EndPoint = new Point(1, 0)
```

**Vertical (Top to Bottom):**
```csharp
StartPoint = new Point(0, 0)
EndPoint = new Point(0, 1)
```

**Diagonal:**
```csharp
StartPoint = new Point(0, 0)
EndPoint = new Point(1, 1)
```

#### Multi-Color Gradients

```csharp
var gradientBrush = new LinearGradientBrush
{
    StartPoint = new Point(0, 0),
    EndPoint = new Point(1, 1),
    GradientStops = new GradientStopCollection
    {
        new GradientStop { Color = Color.FromArgb("#FF6B6B"), Offset = 0 },
        new GradientStop { Color = Color.FromArgb("#FFA500"), Offset = 0.5f },
        new GradientStop { Color = Color.FromArgb("#4ECDC4"), Offset = 1 }
    }
};

avatar.Background = gradientBrush;
```

## Sizing Properties

### Width and Height

Control the exact size of the avatar view:

**XAML:**
```xaml
<sfavatar:SfAvatarView 
    ContentType="Custom"
    ImageSource="user.png"
    WidthRequest="120"
    HeightRequest="120"
    CornerRadius="60" />
```

**C#:**
```csharp
var avatarView = new SfAvatarView
{
    ContentType = ContentType.Custom,
    ImageSource = "user.png",
    WidthRequest = 120,
    HeightRequest = 120,
    CornerRadius = 60
};
```

**Common Sizes:**
- Small: 40x40
- Medium: 60x60
- Large: 80x80
- Extra Large: 120x120
- Profile Page: 150x150 or larger

### Stroke Thickness

Control the width of the border:

**XAML:**
```xaml
<sfavatar:SfAvatarView 
    ContentType="Custom"
    ImageSource="user.png"
    AvatarShape="Circle"
    AvatarSize="Large"
    Stroke="Black"
    StrokeThickness="4"
    HorizontalOptions="Center"
    VerticalOptions="Center" />
```

**C#:**
```csharp
var avatarView = new SfAvatarView
{
    ContentType = ContentType.Custom,
    ImageSource = "user.png",
    AvatarShape = AvatarShape.Circle,
    AvatarSize = AvatarSize.Large,
    Stroke = Colors.Black,
    StrokeThickness = 4,
    HorizontalOptions = LayoutOptions.Center,
    VerticalOptions = LayoutOptions.Center
};
```

**Guidelines:**
- Subtle border: 1-2px
- Standard border: 2-3px
- Prominent border: 4-6px
- Avoid > 8px (becomes too dominant)

### Corner Radius

Create rounded corners or perfect circles:

**XAML:**
```xaml
<sfavatar:SfAvatarView 
    ContentType="Custom"
    ImageSource="user.png"
    WidthRequest="60"
    HeightRequest="60"
    CornerRadius="20"
    StrokeThickness="1"
    Stroke="Black"
    HorizontalOptions="Center"
    VerticalOptions="Center" />
```

**C#:**
```csharp
var avatarView = new SfAvatarView
{
    ContentType = ContentType.Custom,
    ImageSource = "user.png",
    WidthRequest = 60,
    HeightRequest = 60,
    CornerRadius = 20,
    Stroke = Colors.Black,
    StrokeThickness = 1,
    HorizontalOptions = LayoutOptions.Center,
    VerticalOptions = LayoutOptions.Center
};
```

**Common Patterns:**
- Perfect circle: CornerRadius = Width/2
- Rounded square: CornerRadius = Width/8 to Width/6
- Slight rounding: CornerRadius = 4-8
- Square: CornerRadius = 0

### Content Padding

Add spacing between the stroke and the content:

**XAML:**
```xaml
<sfavatar:SfAvatarView 
    ContentType="Custom"
    ImageSource="twitter.png"
    AvatarShape="Circle"
    Stroke="Black"
    StrokeThickness="1"
    ContentPadding="10"
    HorizontalOptions="Center"
    VerticalOptions="Center" />
```

**C#:**
```csharp
var avatarView = new SfAvatarView
{
    ContentType = ContentType.Custom,
    ImageSource = "twitter.png",
    AvatarShape = AvatarShape.Circle,
    Stroke = Colors.Black,
    StrokeThickness = 1,
    ContentPadding = 10,
    HorizontalOptions = LayoutOptions.Center,
    VerticalOptions = LayoutOptions.Center
};
```

**Use Cases:**
- Logo badges: Higher padding (8-15px)
- Standard avatars: No padding or minimal (0-4px)
- Icon avatars: Medium padding (6-10px)

## Font Customization

Font properties apply to Initials content type.

### Font Size

**XAML:**
```xaml
<sfavatar:SfAvatarView 
    ContentType="Initials"
    AvatarName="Alex"
    AvatarShape="Circle"
    FontSize="24"
    InitialsColor="White"
    Background="Navy" />
```

**C#:**
```csharp
var avatarView = new SfAvatarView
{
    ContentType = ContentType.Initials,
    AvatarName = "Alex",
    AvatarShape = AvatarShape.Circle,
    FontSize = 24,
    InitialsColor = Colors.White,
    Background = Colors.Navy
};
```

**Size Guidelines:**
- Small avatars (40-50px): FontSize 14-16
- Medium avatars (60-80px): FontSize 18-24
- Large avatars (100+px): FontSize 28-36

### Font Family

**XAML:**
```xaml
<sfavatar:SfAvatarView 
    ContentType="Initials"
    AvatarName="Alex"
    AvatarShape="Circle"
    FontFamily="OpenSansSemibold"
    InitialsColor="White"
    Background="Navy" />
```

**C#:**
```csharp
var avatarView = new SfAvatarView
{
    ContentType = ContentType.Initials,
    AvatarName = "Alex",
    AvatarShape = AvatarShape.Circle,
    FontFamily = "OpenSansSemibold",
    InitialsColor = Colors.White,
    Background = Colors.Navy
};
```

**Note:** Font must be registered in `MauiProgram.cs` or available system-wide.

### Font Attributes

**XAML:**
```xaml
<sfavatar:SfAvatarView 
    ContentType="Initials"
    AvatarName="Alex"
    AvatarShape="Circle"
    FontAttributes="Bold"
    InitialsColor="White"
    Background="Navy" />
```

**C#:**
```csharp
var avatarView = new SfAvatarView
{
    ContentType = ContentType.Initials,
    AvatarName = "Alex",
    AvatarShape = AvatarShape.Circle,
    FontAttributes = FontAttributes.Bold,
    InitialsColor = Colors.White,
    Background = Colors.Navy
};
```

**Options:**
- `FontAttributes.None` - Regular weight
- `FontAttributes.Bold` - Bold text (recommended for initials)
- `FontAttributes.Italic` - Italic text (rarely used for avatars)

### Font Auto-Scaling

Enable automatic font scaling based on OS accessibility settings:

**XAML:**
```xaml
<sfavatar:SfAvatarView 
    ContentType="Initials"
    InitialsType="DoubleCharacter"
    AvatarName="Alex"
    WidthRequest="50"
    HeightRequest="50"
    FontAttributes="Bold"
    FontAutoScalingEnabled="True"
    CornerRadius="25" />
```

**C#:**
```csharp
var avatarView = new SfAvatarView
{
    ContentType = ContentType.Initials,
    InitialsType = InitialsType.DoubleCharacter,
    AvatarName = "Alex",
    WidthRequest = 50,
    HeightRequest = 50,
    FontAttributes = FontAttributes.Bold,
    FontAutoScalingEnabled = true,
    CornerRadius = 25
};
```

**Benefits:**
- Respects user's text size preferences
- Improves accessibility
- Better user experience for visually impaired users

**Default:** `false` (maintains consistent design)

## Best Practices

### Color Accessibility

1. **Ensure sufficient contrast** between initials and background
   ```csharp
   // Good contrast
   InitialsColor = Colors.White
   Background = Colors.Navy
   
   // Poor contrast (avoid)
   InitialsColor = Colors.LightGray
   Background = Colors.White
   ```

2. **Test with different color modes**
   - Light and dark themes
   - High contrast modes
   - Color blindness simulators

### Sizing Consistency

1. **Use consistent sizes within contexts**
   ```csharp
   // Chat list
   var listAvatarSize = 40;
   
   // Chat detail
   var detailAvatarSize = 60;
   
   // Profile page
   var profileAvatarSize = 120;
   ```

2. **Maintain aspect ratios**
   - Always use square dimensions for circles
   - WidthRequest = HeightRequest for most cases

### Performance

1. **Avoid excessive customization**
   - Don't create thousands of unique gradient avatars
   - Cache commonly used configurations

2. **Optimize images**
   - Use appropriately sized source images
   - Consider image compression

3. **Limit gradient complexity**
   - Stick to 2-3 gradient stops
   - More stops = more rendering cost

## Common Customization Patterns

### Pattern 1: Elegant Profile Avatar

```csharp
var avatar = new SfAvatarView
{
    ContentType = ContentType.Custom,
    ImageSource = "profile.png",
    AvatarShape = AvatarShape.Circle,
    WidthRequest = 120,
    HeightRequest = 120,
    Stroke = Colors.LightGray,
    StrokeThickness = 3,
    Aspect = Aspect.AspectFill
};
```

### Pattern 2: Bold Initials Avatar

```csharp
var avatar = new SfAvatarView
{
    ContentType = ContentType.Initials,
    InitialsType = InitialsType.DoubleCharacter,
    AvatarName = userName,
    AvatarShape = AvatarShape.Circle,
    AvatarSize = AvatarSize.Large,
    AvatarColorMode = AvatarColorMode.DarkBackground,
    FontAttributes = FontAttributes.Bold,
    FontSize = 24
};
```

### Pattern 3: Gradient Brand Avatar

```csharp
var avatar = new SfAvatarView
{
    ContentType = ContentType.Initials,
    AvatarName = companyName,
    AvatarShape = AvatarShape.Circle,
    FontSize = 28,
    FontAttributes = FontAttributes.Bold,
    InitialsColor = Colors.White,
    Background = new LinearGradientBrush
    {
        StartPoint = new Point(0, 0),
        EndPoint = new Point(1, 1),
        GradientStops = new GradientStopCollection
        {
            new GradientStop { Color = brandColorPrimary, Offset = 0 },
            new GradientStop { Color = brandColorSecondary, Offset = 1 }
        }
    }
};
```

### Pattern 4: Subtle Icon Avatar

```csharp
var avatar = new SfAvatarView
{
    ContentType = ContentType.Custom,
    ImageSource = "icon.png",
    AvatarShape = AvatarShape.Circle,
    WidthRequest = 50,
    HeightRequest = 50,
    Background = Colors.White,
    ContentPadding = 12,
    Stroke = Color.FromArgb("#E0E0E0"),
    StrokeThickness = 1
};
```
