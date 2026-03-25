# View Modes and Layout in .NET MAUI Carousel

The SfCarousel control supports two distinct view modes that change how items are arranged and displayed: Linear and Default (3D perspective).

## View Modes

### Linear Mode

Linear mode displays carousel items in a stacked horizontal layout without 3D perspective effects.

**XAML:**
```xml
<carousel:SfCarousel x:Name="carousel"  
                     ItemsSource="{Binding ImageCollection}"
                     ItemTemplate="{StaticResource itemTemplate}" 
                     ViewMode="Linear"
                     ItemHeight="170"
                     ItemWidth="270"/>
```

**C#:**
```csharp
var carousel = new SfCarousel
{
    ViewMode = ViewMode.Linear,
    ItemHeight = 170,
    ItemWidth = 270
};

carousel.ItemTemplate = itemTemplate;
carousel.SetBinding(SfCarousel.ItemsSourceProperty, "ImageCollection");
```

**Characteristics:**
- Items arranged in a flat, horizontal row
- No rotation or 3D effects
- Simple left-to-right navigation
- Modern, clean appearance
- Better for text-heavy content

**When to use Linear:**
- Product galleries with descriptions
- Onboarding tutorial screens
- Horizontal scrolling cards
- Dashboard widgets
- Banner carousels

### Default Mode (3D Perspective)

Default mode creates a 3D perspective effect where items appear to rotate around a central axis.

**XAML:**
```xml
<carousel:SfCarousel x:Name="carousel"  
                     ItemsSource="{Binding ImageCollection}"
                     ItemTemplate="{StaticResource itemTemplate}" 
                     ViewMode="Default"
                     ItemHeight="170"
                     ItemWidth="270"/>
```

**C#:**
```csharp
var carousel = new SfCarousel
{
    ViewMode = ViewMode.Default,
    ItemHeight = 170,
    ItemWidth = 270
};

carousel.ItemTemplate = itemTemplate;
carousel.SetBinding(SfCarousel.ItemsSourceProperty, "ImageCollection");
```

**Characteristics:**
- 3D perspective with depth
- Items rotate as they move
- Visual hierarchy (selected item prominent)
- Eye-catching visual effect
- Better for image-focused content

**When to use Default:**
- Photo galleries
- Album covers
- Product showcases (images)
- Interactive image browsing
- Visual content exploration

## Layout Properties for Default Mode

### Offset

The Offset property specifies the spacing between unselected items in Default mode.

**XAML:**
```xml
<carousel:SfCarousel ViewMode="Default"
                     Offset="200"
                     ItemsSource="{Binding ImageCollection}"
                     ItemTemplate="{StaticResource itemTemplate}"/>
```

**C#:**
```csharp
var carousel = new SfCarousel
{
    ViewMode = ViewMode.Default,
    Offset = 200
};
```

**Effects:**
- **Smaller Offset (100-150):** Items closer together, more compact
- **Medium Offset (200-300):** Balanced spacing
- **Larger Offset (400+):** Items spread far apart, more dramatic

**Example values:**
```csharp
// Compact layout
carousel.Offset = 120;

// Balanced (recommended)
carousel.Offset = 200;

// Dramatic spacing
carousel.Offset = 350;
```

### Rotation Angle

The RotationAngle property rotates all items by a specified angle in Default mode.

**XAML:**
```xml
<carousel:SfCarousel ViewMode="Default"
                     RotationAngle="45"
                     ItemsSource="{Binding ImageCollection}"
                     ItemTemplate="{StaticResource itemTemplate}"/>
```

**C#:**
```csharp
var carousel = new SfCarousel
{
    ViewMode = ViewMode.Default,
    RotationAngle = 45
};
```

**Common angles:**
```csharp
// Slight tilt (subtle effect)
carousel.RotationAngle = 15;

// Medium rotation (visible but balanced)
carousel.RotationAngle = 30;

// Strong rotation (dramatic effect)
carousel.RotationAngle = 60;

// Extreme rotation (very tilted)
carousel.RotationAngle = 75;
```

**Visual impact:**
- **0°:** No rotation, flat items
- **15-30°:** Subtle depth effect
- **30-60°:** Noticeable 3D perspective
- **60-90°:** Strong perspective (may reduce readability)

**Combining Offset and RotationAngle:**
```xml
<carousel:SfCarousel ViewMode="Default"
                     Offset="250"
                     RotationAngle="35"
                     ItemsSource="{Binding ImageCollection}"
                     ItemTemplate="{StaticResource itemTemplate}"
                     ItemHeight="200"
                     ItemWidth="300"/>
```

## Layout Properties for Both Modes

### ItemSpacing

Controls the space between items (works in both modes).

**XAML:**
```xml
<carousel:SfCarousel ViewMode="Linear"
                     ItemSpacing="10"
                     ItemsSource="{Binding ImageCollection}"
                     ItemTemplate="{StaticResource itemTemplate}"/>
```

**C#:**
```csharp
var carousel = new SfCarousel
{
    ViewMode = ViewMode.Linear,
    ItemSpacing = 10
};
```

**Recommended values:**
- **Tight spacing:** 2-5 pixels
- **Normal spacing:** 10-15 pixels
- **Loose spacing:** 20-30 pixels

### ItemHeight and ItemWidth

Set the dimensions for all carousel items.

```xml
<carousel:SfCarousel ItemHeight="250"
                     ItemWidth="350"
                     ItemsSource="{Binding ImageCollection}"
                     ItemTemplate="{StaticResource itemTemplate}"/>
```

**Device-specific recommendations:**

**Mobile (Portrait):**
```csharp
carousel.ItemHeight = 180;
carousel.ItemWidth = 250;
```

**Mobile (Landscape):**
```csharp
carousel.ItemHeight = 150;
carousel.ItemWidth = 280;
```

**Tablet:**
```csharp
carousel.ItemHeight = 300;
carousel.ItemWidth = 400;
```

**Desktop:**
```csharp
carousel.ItemHeight = 350;
carousel.ItemWidth = 500;
```

## Complete Examples

### Example 1: Simple Linear Gallery

```xml
<carousel:SfCarousel ViewMode="Linear"
                     ItemsSource="{Binding Products}"
                     ItemTemplate="{StaticResource productTemplate}"
                     ItemHeight="200"
                     ItemWidth="300"
                     ItemSpacing="15"
                     HeightRequest="300"/>
```

**Use case:** Horizontal product showcase with descriptions

### Example 2: 3D Image Gallery

```xml
<carousel:SfCarousel ViewMode="Default"
                     ItemsSource="{Binding Photos}"
                     ItemTemplate="{StaticResource photoTemplate}"
                     ItemHeight="250"
                     ItemWidth="350"
                     Offset="200"
                     RotationAngle="30"
                     HeightRequest="400"/>
```

**Use case:** Photo album with depth effect

### Example 3: Compact Linear List

```xml
<carousel:SfCarousel ViewMode="Linear"
                     ItemsSource="{Binding Cards}"
                     ItemTemplate="{StaticResource cardTemplate}"
                     ItemHeight="150"
                     ItemWidth="200"
                     ItemSpacing="5"
                     HeightRequest="200"/>
```

**Use case:** Compact horizontal scrolling cards

### Example 4: Dramatic 3D Showcase

```xml
<carousel:SfCarousel ViewMode="Default"
                     ItemsSource="{Binding FeaturedItems}"
                     ItemTemplate="{StaticResource featureTemplate}"
                     ItemHeight="300"
                     ItemWidth="400"
                     Offset="300"
                     RotationAngle="50"
                     HeightRequest="500"/>
```

**Use case:** Hero section with dramatic visual effect

## Responsive Layout

Adapt layout based on device size:

```csharp
public MainPage()
{
    InitializeComponent();
    
    var carousel = new SfCarousel
    {
        ItemsSource = viewModel.ImageCollection,
        ItemTemplate = itemTemplate
    };
    
    // Adjust based on device
    if (DeviceInfo.Idiom == DeviceIdiom.Phone)
    {
        carousel.ViewMode = ViewMode.Linear;
        carousel.ItemHeight = 180;
        carousel.ItemWidth = 250;
        carousel.ItemSpacing = 10;
    }
    else if (DeviceInfo.Idiom == DeviceIdiom.Tablet)
    {
        carousel.ViewMode = ViewMode.Default;
        carousel.ItemHeight = 300;
        carousel.ItemWidth = 400;
        carousel.Offset = 220;
        carousel.RotationAngle = 35;
    }
    else // Desktop
    {
        carousel.ViewMode = ViewMode.Default;
        carousel.ItemHeight = 350;
        carousel.ItemWidth = 500;
        carousel.Offset = 280;
        carousel.RotationAngle = 40;
    }
    
    this.Content = carousel;
}
```

## Mode Comparison

| Feature | Linear Mode | Default Mode |
|---------|-------------|--------------|
| **Layout** | Flat horizontal | 3D perspective |
| **Rotation** | None | Yes |
| **Offset Support** | No | Yes |
| **RotationAngle Support** | No | Yes |
| **ItemSpacing** | Yes | Yes |
| **Best For** | Text content, cards | Images, photos |
| **Visual Complexity** | Simple | Complex |
| **Performance** | Slightly better | Good |

## Best Practices

**For Linear Mode:**
- Use for content with text and images
- Keep ItemSpacing consistent (10-15px)
- Ensure ItemWidth fits content comfortably
- Good for horizontal scrolling lists

**For Default Mode:**
- Use for pure image galleries
- Balance Offset and RotationAngle
- Start with Offset=200, RotationAngle=30, then adjust
- Avoid text-heavy content (readability issues)

**Performance:**
- Linear mode: Slightly more efficient
- Default mode: More GPU usage (3D transforms)
- Both: Use UI virtualization for large datasets

**Accessibility:**
- Linear mode: Better for screen readers
- Default mode: Ensure selected item is clearly distinguishable
- Both: Provide clear labels and navigation

## Troubleshooting

**Items overlapping in Default mode:**
→ Increase Offset value (try 250-300)

**Too much empty space in Default mode:**
→ Decrease Offset value (try 150-180)

**Items too tilted to read in Default mode:**
→ Reduce RotationAngle (try 20-30 degrees)

**Linear mode not showing spacing:**
→ Set ItemSpacing property explicitly (try 10-15)

**Layout doesn't look right on different devices:**
→ Implement responsive sizing based on DeviceInfo.Idiom
