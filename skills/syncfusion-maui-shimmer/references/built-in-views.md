# Built-in View Types for .NET MAUI Shimmer

`SfShimmer` ships with 7 pre-designed shimmer layouts via the `Type` property (`ShimmerType` enum). Use these when you want a polished loading placeholder without building a custom layout.

---

## Available Types

| Type | Best Used For |
|---|---|
| `CirclePersona` | User profile cards with circular avatar (default) |
| `SquarePersona` | User profile cards with square avatar |
| `Profile` | Detailed profile page with header and body lines |
| `Article` | Blog post or article content with heading + paragraphs |
| `Video` | Video player with thumbnail and description |
| `Feed` | Social media or news feed cards |
| `Shopping` | Product grid tiles with image, name, price |

---

## Setting the Type

### XAML

```xml
<shimmer:SfShimmer x:Name="shimmer"
                   Type="Article"
                   VerticalOptions="Fill">
    <StackLayout>
        <Label Text="Content is loaded!" />
    </StackLayout>
</shimmer:SfShimmer>
```

### C#

```csharp
SfShimmer shimmer = new SfShimmer()
{
    Type = ShimmerType.Article,
    VerticalOptions = LayoutOptions.Fill,
    Content = new Label
    {
        Text = "Content is loaded!"
    }
};
this.Content = shimmer;
```

---

## Type Examples

### CirclePersona (Default)
Renders a circular avatar on the left with text lines on the right — ideal for contact or user lists.

```xml
<shimmer:SfShimmer Type="CirclePersona" RepeatCount="4" VerticalOptions="Fill" />
```

### Article
Renders a wide heading block followed by several paragraph-width lines — suitable for news feeds or blog previews.

```xml
<shimmer:SfShimmer Type="Article" RepeatCount="2" VerticalOptions="Fill" />
```

### Shopping
Renders a square image block above two short text lines — suitable for product grids and e-commerce.

```xml
<shimmer:SfShimmer Type="Shopping" RepeatCount="6" VerticalOptions="Fill" />
```

### Feed
Renders a feed-card style block suitable for social timelines.

```xml
<shimmer:SfShimmer Type="Feed" RepeatCount="3" VerticalOptions="Fill" />
```

---

## Choosing Between Built-in and Custom

Use a **built-in type** when:
- The placeholder shape matches one of the 7 layouts above
- You want a zero-configuration shimmer

Use a **custom view** (see `custom-views.md`) when:
- Your UI has a unique layout not covered by built-in types
- You need precise shape matching (circles, rounded rectangles, specific dimensions)
