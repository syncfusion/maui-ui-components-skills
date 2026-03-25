# Visual Styles in .NET MAUI Avatar View

The SfAvatarView control supports built-in visual styles that provide consistent sizing and shapes across your application. This guide covers Custom, Circle, and Square visual styles with their size variations.

## Table of Contents

- [Overview](#overview)
- [Visual Style Types](#visual-style-types)
- [Custom Style](#custom-style)
- [Circle Styles](#circle-styles)
  - [Size Variations](#circle-size-variations)
  - [Implementation Examples](#circle-implementation)
- [Square Styles](#square-styles)
  - [Size Variations](#square-size-variations)
  - [Implementation Examples](#square-implementation)
- [Choosing Visual Styles](#choosing-visual-styles)
- [Combining Styles with Other Properties](#combining-styles-with-other-properties)

## Overview

Visual styles in SfAvatarView are controlled by two main properties:

- **AvatarShape** - Determines the shape (`Custom`, `Circle`, `Square`)
- **AvatarSize** - Determines the size preset (`ExtraSmall`, `Small`, `Medium`, `Large`, `ExtraLarge`)

These properties work together to create consistent, predefined avatar appearances without manually setting width, height, and corner radius.

## Visual Style Types

| Style | Description | Best For |
|-------|-------------|----------|
| **Custom** | Manually defined dimensions | Precise control, unique sizes |
| **Circle** | Circular avatars with size presets | Profile pictures, user avatars |
| **Square** | Square avatars with size presets | App icons, badges, groups |

**Default:** `Custom` style with manual dimension control

## Custom Style

The **Custom** style gives you complete control over dimensions and shape by manually setting properties.

### When to Use Custom Style

- Need exact pixel dimensions
- Responsive layouts with dynamic sizing
- Unique shapes beyond circles and squares
- Per-avatar customization required

### Implementation

**XAML:**
```xaml
<sfavatar:SfAvatarView 
    ContentType="Custom"
    ImageSource="user.png"
    WidthRequest="75"
    HeightRequest="75"
    CornerRadius="15"
    Stroke="Gray"
    StrokeThickness="2" />
```

**C#:**
```csharp
var avatarView = new SfAvatarView
{
    ContentType = ContentType.Custom,
    ImageSource = "user.png",
    WidthRequest = 75,
    HeightRequest = 75,
    CornerRadius = 15,
    Stroke = Colors.Gray,
    StrokeThickness = 2
};
```

### Key Points

- Must manually set `WidthRequest`, `HeightRequest`, and `CornerRadius`
- Full flexibility in dimensions
- No preset sizes applied
- Default when `AvatarShape` is not specified

## Circle Styles

Circle styles provide five predefined circular avatar sizes.

### Circle Size Variations

| Size | Typical Dimensions | Use Case |
|------|-------------------|----------|
| **ExtraSmall** | ~32px | Dense lists, tags |
| **Small** | ~48px | Compact lists, comments |
| **Medium** | ~64px | Standard lists, cards |
| **Large** | ~80px | Detail views, headers |
| **ExtraLarge** | ~96px+ | Profile pages, hero sections |

**Note:** Exact dimensions are managed by the control based on platform and theme.

### Circle Implementation

#### Single Circle Avatar

**XAML:**
```xaml
<sfavatar:SfAvatarView 
    ContentType="Custom"
    ImageSource="user.png"
    AvatarShape="Circle"
    AvatarSize="Large"
    Stroke="Black"
    StrokeThickness="1"
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
    StrokeThickness = 1,
    HorizontalOptions = LayoutOptions.Center,
    VerticalOptions = LayoutOptions.Center
};
```

#### All Circle Sizes Example

**XAML:**
```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <Style x:Key="CircleAvatarStyle" TargetType="sfavatar:SfAvatarView">
            <Setter Property="VerticalOptions" Value="Center"/>
            <Setter Property="HorizontalOptions" Value="Center"/>
            <Setter Property="ContentType" Value="Custom"/>
            <Setter Property="ImageSource" Value="user.png"/>
            <Setter Property="Stroke" Value="Black"/>
            <Setter Property="StrokeThickness" Value="1"/>
            <Setter Property="AvatarShape" Value="Circle"/>
        </Style>
    </ResourceDictionary>
</ContentPage.Resources>

<StackLayout Orientation="Vertical" Spacing="20" Padding="20">
    
    <!-- Extra Large Circle -->
    <StackLayout HorizontalOptions="Center">
        <sfavatar:SfAvatarView 
            AvatarSize="ExtraLarge"
            Style="{StaticResource CircleAvatarStyle}"/>
        <Label Text="ExtraLarge" 
               HorizontalOptions="Center"
               FontAttributes="Bold"
               FontSize="10"/>
    </StackLayout>
    
    <!-- Large Circle -->
    <StackLayout HorizontalOptions="Center">
        <sfavatar:SfAvatarView 
            AvatarSize="Large"
            Style="{StaticResource CircleAvatarStyle}"/>
        <Label Text="Large" 
               HorizontalOptions="Center"
               FontAttributes="Bold"
               FontSize="10"/>
    </StackLayout>
    
    <!-- Medium Circle -->
    <StackLayout HorizontalOptions="Center">
        <sfavatar:SfAvatarView 
            AvatarSize="Medium"
            Style="{StaticResource CircleAvatarStyle}"/>
        <Label Text="Medium" 
               HorizontalOptions="Center"
               FontAttributes="Bold"
               FontSize="10"/>
    </StackLayout>
    
    <!-- Small Circle -->
    <StackLayout HorizontalOptions="Center">
        <sfavatar:SfAvatarView 
            AvatarSize="Small"
            Style="{StaticResource CircleAvatarStyle}"/>
        <Label Text="Small" 
               HorizontalOptions="Center"
               FontAttributes="Bold"
               FontSize="10"/>
    </StackLayout>
    
    <!-- Extra Small Circle -->
    <StackLayout HorizontalOptions="Center">
        <sfavatar:SfAvatarView 
            AvatarSize="ExtraSmall"
            Style="{StaticResource CircleAvatarStyle}"/>
        <Label Text="ExtraSmall" 
               HorizontalOptions="Center"
               FontAttributes="Bold"
               FontSize="10"/>
    </StackLayout>
    
</StackLayout>
```

**C# Implementation:**
```csharp
var mainLayout = new StackLayout
{
    Orientation = StackOrientation.Vertical,
    Spacing = 20,
    Padding = 20,
    HorizontalOptions = LayoutOptions.Center
};

// Helper method to create avatar with label
StackLayout CreateAvatarWithLabel(AvatarSize size, string label)
{
    var avatar = new SfAvatarView
    {
        ContentType = ContentType.Custom,
        ImageSource = "user.png",
        AvatarShape = AvatarShape.Circle,
        AvatarSize = size,
        Stroke = Colors.Black,
        StrokeThickness = 1,
        HorizontalOptions = LayoutOptions.Center,
        VerticalOptions = LayoutOptions.Center
    };
    
    var labelView = new Label
    {
        Text = label,
        HorizontalOptions = LayoutOptions.Center,
        FontAttributes = FontAttributes.Bold,
        FontSize = 10
    };
    
    var container = new StackLayout
    {
        HorizontalOptions = LayoutOptions.Center
    };
    container.Children.Add(avatar);
    container.Children.Add(labelView);
    
    return container;
}

// Add all sizes
mainLayout.Children.Add(CreateAvatarWithLabel(AvatarSize.ExtraLarge, "ExtraLarge"));
mainLayout.Children.Add(CreateAvatarWithLabel(AvatarSize.Large, "Large"));
mainLayout.Children.Add(CreateAvatarWithLabel(AvatarSize.Medium, "Medium"));
mainLayout.Children.Add(CreateAvatarWithLabel(AvatarSize.Small, "Small"));
mainLayout.Children.Add(CreateAvatarWithLabel(AvatarSize.ExtraSmall, "ExtraSmall"));

Content = mainLayout;
```

### Circle Style Use Cases

**ExtraSmall Circles:**
- Comment author avatars
- Mention chips
- Reaction indicators
- Dense activity feeds

**Small Circles:**
- List item avatars (contacts, messages)
- Compact navigation elements
- Secondary user indicators

**Medium Circles:**
- Standard list views
- Card headers
- Search results
- Chat messages

**Large Circles:**
- Detail page headers
- Active conversation indicator
- Feature highlights
- Navigation drawer profile

**ExtraLarge Circles:**
- Profile pages
- User settings
- Account management
- Hero sections

## Square Styles

Square styles provide five predefined square avatar sizes with slightly rounded corners.

### Square Size Variations

| Size | Typical Dimensions | Use Case |
|------|-------------------|----------|
| **ExtraSmall** | ~32px | App icons, badges |
| **Small** | ~48px | Thumbnails, gallery items |
| **Medium** | ~64px | Media items, attachments |
| **Large** | ~80px | Feature images, tiles |
| **ExtraLarge** | ~96px+ | Hero images, covers |

### Square Implementation

#### Single Square Avatar

**XAML:**
```xaml
<sfavatar:SfAvatarView 
    ContentType="Custom"
    ImageSource="app_icon.png"
    AvatarShape="Square"
    AvatarSize="Medium"
    Stroke="Gray"
    StrokeThickness="2"
    HorizontalOptions="Center"
    VerticalOptions="Center" />
```

**C#:**
```csharp
var avatarView = new SfAvatarView
{
    ContentType = ContentType.Custom,
    ImageSource = "app_icon.png",
    AvatarShape = AvatarShape.Square,
    AvatarSize = AvatarSize.Medium,
    Stroke = Colors.Gray,
    StrokeThickness = 2,
    HorizontalOptions = LayoutOptions.Center,
    VerticalOptions = LayoutOptions.Center
};
```

#### All Square Sizes Example

**XAML:**
```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <Style x:Key="SquareAvatarStyle" TargetType="sfavatar:SfAvatarView">
            <Setter Property="VerticalOptions" Value="Center"/>
            <Setter Property="HorizontalOptions" Value="Center"/>
            <Setter Property="ContentType" Value="Custom"/>
            <Setter Property="ImageSource" Value="app_icon.png"/>
            <Setter Property="Stroke" Value="Black"/>
            <Setter Property="StrokeThickness" Value="2"/>
            <Setter Property="AvatarShape" Value="Square"/>
        </Style>
    </ResourceDictionary>
</ContentPage.Resources>

<Grid RowDefinitions="*,*" 
      ColumnDefinitions="*,*,*,*,*"
      HorizontalOptions="Center"
      VerticalOptions="Center">
    
    <!-- Row 0: Avatars -->
    <sfavatar:SfAvatarView Grid.Row="0" Grid.Column="4"
                           AvatarSize="ExtraLarge"
                           Style="{StaticResource SquareAvatarStyle}"/>
    
    <sfavatar:SfAvatarView Grid.Row="0" Grid.Column="3"
                           AvatarSize="Large"
                           Style="{StaticResource SquareAvatarStyle}"/>
    
    <sfavatar:SfAvatarView Grid.Row="0" Grid.Column="2"
                           AvatarSize="Medium"
                           Style="{StaticResource SquareAvatarStyle}"/>
    
    <sfavatar:SfAvatarView Grid.Row="0" Grid.Column="1"
                           AvatarSize="Small"
                           Style="{StaticResource SquareAvatarStyle}"/>
    
    <sfavatar:SfAvatarView Grid.Row="0" Grid.Column="0"
                           AvatarSize="ExtraSmall"
                           Style="{StaticResource SquareAvatarStyle}"/>
    
    <!-- Row 1: Labels -->
    <Label Grid.Row="1" Grid.Column="4" 
           Text="ExtraLarge"
           FontAttributes="Bold"
           FontSize="10"
           HorizontalOptions="Center"
           VerticalOptions="Center"/>
    
    <Label Grid.Row="1" Grid.Column="3" 
           Text="Large"
           FontAttributes="Bold"
           FontSize="10"
           HorizontalOptions="Center"
           VerticalOptions="Center"/>
    
    <Label Grid.Row="1" Grid.Column="2" 
           Text="Medium"
           FontAttributes="Bold"
           FontSize="10"
           HorizontalOptions="Center"
           VerticalOptions="Center"/>
    
    <Label Grid.Row="1" Grid.Column="1" 
           Text="Small"
           FontAttributes="Bold"
           FontSize="10"
           HorizontalOptions="Center"
           VerticalOptions="Center"/>
    
    <Label Grid.Row="1" Grid.Column="0" 
           Text="ExtraSmall"
           FontAttributes="Bold"
           FontSize="10"
           HorizontalOptions="Center"
           VerticalOptions="Center"/>
</Grid>
```

**C# Implementation:**
```csharp
var grid = new Grid
{
    HorizontalOptions = LayoutOptions.Center,
    VerticalOptions = LayoutOptions.Center
};

grid.RowDefinitions.Add(new RowDefinition { Height = GridLength.Star });
grid.RowDefinitions.Add(new RowDefinition { Height = GridLength.Star });

for (int i = 0; i < 5; i++)
{
    grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Star });
}

// Create avatars
var sizes = new[] 
{ 
    (AvatarSize.ExtraSmall, "ExtraSmall", 0),
    (AvatarSize.Small, "Small", 1),
    (AvatarSize.Medium, "Medium", 2),
    (AvatarSize.Large, "Large", 3),
    (AvatarSize.ExtraLarge, "ExtraLarge", 4)
};

foreach (var (size, label, column) in sizes)
{
    var avatar = new SfAvatarView
    {
        ContentType = ContentType.Custom,
        ImageSource = "app_icon.png",
        AvatarShape = AvatarShape.Square,
        AvatarSize = size,
        Stroke = Colors.Black,
        StrokeThickness = 2,
        HorizontalOptions = LayoutOptions.Center,
        VerticalOptions = LayoutOptions.Center
    };
    
    grid.Add(avatar, column, 0);
    
    var labelView = new Label
    {
        Text = label,
        FontAttributes = FontAttributes.Bold,
        FontSize = 10,
        HorizontalOptions = LayoutOptions.Center,
        VerticalOptions = LayoutOptions.Center
    };
    
    grid.Add(labelView, column, 1);
}

Content = grid;
```

### Square Style Use Cases

**ExtraSmall Squares:**
- App badges
- File type indicators
- Category icons
- Status indicators

**Small Squares:**
- Attachment thumbnails
- Gallery previews
- Quick action tiles
- Mini cards

**Medium Squares:**
- Photo galleries
- Document previews
- Media playlists
- Product thumbnails

**Large Squares:**
- Album covers
- Featured images
- Tile navigation
- Dashboard widgets

**ExtraLarge Squares:**
- Hero images
- Cover photos
- Banner images
- Feature showcases

## Choosing Visual Styles

### Circle vs Square Decision Matrix

| Content Type | Recommended Shape | Reason |
|--------------|-------------------|---------|
| Profile photos | Circle | Traditional, friendly |
| Team/group | Circle | Personal connection |
| App icons | Square | Standard convention |
| Media content | Square | Efficient space use |
| Documents | Square | Represents files |
| Contacts | Circle | Personal touch |
| Organizations | Square or Circle | Brand preference |

### When to Use Preset Sizes

**Use AvatarSize presets when:**
- Building consistent UI across features
- Following design system guidelines
- Need responsive scaling
- Want platform-appropriate sizing
- Rapid prototyping

**Use Custom sizing when:**
- Exact pixel dimensions required
- Responsive layouts with calculations
- Unique design requirements
- Animation or transform scenarios
- Per-item size variations

## Combining Styles with Other Properties

### Preset Sizes with Custom Colors

```csharp
var avatar = new SfAvatarView
{
    ContentType = ContentType.Initials,
    AvatarName = "John Doe",
    AvatarShape = AvatarShape.Circle,
    AvatarSize = AvatarSize.Large,
    Background = Colors.Navy,
    InitialsColor = Colors.White,
    Stroke = Colors.Gold,
    StrokeThickness = 3
};
```

### Preset Sizes with Content Types

```csharp
// Image content
var imageAvatar = new SfAvatarView
{
    ContentType = ContentType.Custom,
    ImageSource = "photo.png",
    AvatarShape = AvatarShape.Circle,
    AvatarSize = AvatarSize.Medium
};

// Initials content
var initialsAvatar = new SfAvatarView
{
    ContentType = ContentType.Initials,
    AvatarName = "Sarah",
    AvatarShape = AvatarShape.Circle,
    AvatarSize = AvatarSize.Medium
};

// Avatar character
var characterAvatar = new SfAvatarView
{
    ContentType = ContentType.AvatarCharacter,
    AvatarCharacter = AvatarCharacter.Avatar5,
    AvatarShape = AvatarShape.Square,
    AvatarSize = AvatarSize.Large
};
```

### Mixing Preset and Custom Styles

```csharp
// Use preset for consistent base size
var avatar = new SfAvatarView
{
    AvatarShape = AvatarShape.Circle,
    AvatarSize = AvatarSize.Large
};

// Then customize specific properties
avatar.Stroke = brandColor;
avatar.StrokeThickness = 4;
avatar.ContentPadding = 5;
```

## Style Best Practices

1. **Consistency is key** - Pick a style system and stick to it throughout your app
2. **Use presets for standard cases** - Save custom sizing for special needs
3. **Respect platform conventions** - Circles for people, squares for content
4. **Test on multiple devices** - Preset sizes scale better across screens
5. **Document your choices** - Create a style guide for your team

## Common Patterns

### Adaptive Size by Context

```csharp
public SfAvatarView CreateContextualAvatar(string context, string userName)
{
    AvatarSize size = context switch
    {
        "list" => AvatarSize.Small,
        "detail" => AvatarSize.Large,
        "profile" => AvatarSize.ExtraLarge,
        _ => AvatarSize.Medium
    };
    
    return new SfAvatarView
    {
        ContentType = ContentType.Initials,
        AvatarName = userName,
        AvatarShape = AvatarShape.Circle,
        AvatarSize = size
    };
}
```

### Consistent Avatar Factory

```csharp
public class AvatarFactory
{
    private readonly Color _brandColor;
    
    public AvatarFactory(Color brandColor)
    {
        _brandColor = brandColor;
    }
    
    public SfAvatarView CreateCircleAvatar(AvatarSize size, string name)
    {
        return new SfAvatarView
        {
            ContentType = ContentType.Initials,
            AvatarName = name,
            AvatarShape = AvatarShape.Circle,
            AvatarSize = size,
            Stroke = _brandColor,
            StrokeThickness = 2,
            AvatarColorMode = AvatarColorMode.DarkBackground
        };
    }
    
    public SfAvatarView CreateSquareAvatar(AvatarSize size, string imagePath)
    {
        return new SfAvatarView
        {
            ContentType = ContentType.Custom,
            ImageSource = imagePath,
            AvatarShape = AvatarShape.Square,
            AvatarSize = size,
            Stroke = _brandColor,
            StrokeThickness = 2
        };
    }
}
```
