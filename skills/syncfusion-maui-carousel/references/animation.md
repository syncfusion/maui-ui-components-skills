# Animation in .NET MAUI Carousel

The SfCarousel control provides animation support to create smooth transitions when navigating between items. The Duration property controls how long the animation takes to complete.

## Duration Property

The Duration property specifies the time (in milliseconds) taken to animate an item from its current position to the selected item position in Default mode.

**XAML:**
```xml
<carousel:SfCarousel x:Name="carousel"
                     ItemsSource="{Binding ImageCollection}"
                     ItemTemplate="{StaticResource itemTemplate}" 
                     Duration="1000"/>
```

**C#:**
```csharp
var carousel = new SfCarousel();
carousel.SetBinding(SfCarousel.ItemsSourceProperty, "ImageCollection");
carousel.ItemTemplate = itemTemplate;
carousel.Duration = 1000;
```

**Default value:** 600 milliseconds

## Animation Timing Values

Different duration values create different user experiences:

### Fast Animation (200-400ms)
```csharp
carousel.Duration = 300;
```

**Characteristics:**
- Snappy, responsive feel
- Minimal delay between transitions
- Modern, quick interaction

**When to use:**
- Apps requiring fast navigation
- Frequently swiped content
- Power users who navigate quickly
- News feeds, social media

### Normal Animation (500-700ms)
```csharp
carousel.Duration = 600; // Default
```

**Characteristics:**
- Balanced speed and smoothness
- Comfortable viewing experience
- Not too fast, not too slow

**When to use:**
- General-purpose carousels
- Product galleries
- Image viewers
- Default recommendation for most apps

### Slow Animation (800-1200ms)
```csharp
carousel.Duration = 1000;
```

**Characteristics:**
- Smooth, elegant transitions
- Emphasizes movement
- Gives users time to see transition
- Premium, polished feel

**When to use:**
- Hero sections with featured content
- High-quality image galleries
- Luxury brand apps
- Tutorial/onboarding screens

### Very Slow Animation (1200ms+)
```csharp
carousel.Duration = 1500;
```

**Characteristics:**
- Dramatic, cinematic effect
- Very noticeable transitions
- Can feel sluggish if overused

**When to use:**
- Special showcase sections
- Artistic or creative apps
- Infrequent navigation scenarios
- Auto-play with long intervals

## Complete Examples

### Example 1: Quick Navigation Carousel

For fast-paced content browsing:

```xml
<carousel:SfCarousel ItemsSource="{Binding NewsFeed}"
                     ItemTemplate="{StaticResource newsTemplate}"
                     Duration="350"
                     ItemHeight="200"
                     ItemWidth="300"
                     ViewMode="Linear"/>
```

```csharp
var carousel = new SfCarousel
{
    Duration = 350,
    ViewMode = ViewMode.Linear,
    ItemHeight = 200,
    ItemWidth = 300
};
```

**Use case:** News articles, social media posts

### Example 2: Standard Image Gallery

Balanced animation for general use:

```xml
<carousel:SfCarousel ItemsSource="{Binding Photos}"
                     ItemTemplate="{StaticResource photoTemplate}"
                     Duration="600"
                     ItemHeight="250"
                     ItemWidth="350"
                     ViewMode="Default"
                     Offset="200"
                     RotationAngle="30"/>
```

**Use case:** Photo albums, product images

### Example 3: Premium Showcase

Elegant slow animations for featured content:

```xml
<carousel:SfCarousel ItemsSource="{Binding FeaturedProducts}"
                     ItemTemplate="{StaticResource premiumTemplate}"
                     Duration="1100"
                     ItemHeight="300"
                     ItemWidth="400"
                     ViewMode="Default"
                     Offset="250"
                     RotationAngle="35"/>
```

**Use case:** Luxury products, featured content, hero sections

### Example 4: Tutorial/Onboarding

Slow, deliberate transitions for instructional content:

```xml
<carousel:SfCarousel ItemsSource="{Binding OnboardingSteps}"
                     ItemTemplate="{StaticResource tutorialTemplate}"
                     Duration="900"
                     ItemHeight="400"
                     ItemWidth="320"
                     ViewMode="Linear"/>
```

**Use case:** App tutorials, feature walkthroughs, onboarding

## Programmatic Animation Control

### Dynamic Duration Changes

Change animation speed based on context:

```csharp
private void OnUserExperienceChanged(string level)
{
    switch (level)
    {
        case "Beginner":
            carousel.Duration = 800; // Slower for beginners
            break;
        case "Intermediate":
            carousel.Duration = 600; // Standard speed
            break;
        case "Advanced":
            carousel.Duration = 300; // Faster for power users
            break;
    }
}
```

### Adaptive Animation Based on Device

```csharp
public MainPage()
{
    InitializeComponent();
    
    // Adjust animation speed based on device performance
    if (DeviceInfo.Idiom == DeviceIdiom.Phone)
    {
        carousel.Duration = 500; // Faster on mobile
    }
    else if (DeviceInfo.Idiom == DeviceIdiom.Tablet)
    {
        carousel.Duration = 650; // Slightly slower on tablet
    }
    else // Desktop
    {
        carousel.Duration = 800; // Slower on desktop (larger screens)
    }
}
```

### Conditional Animation

Disable animation for specific scenarios:

```csharp
// Instant navigation (no animation)
carousel.Duration = 0;

// Navigate to specific item
carousel.SelectedIndex = 3;

// Restore animation
carousel.Duration = 600;
```

**When to use instant navigation:**
- Programmatic updates
- Restoring saved state
- Bulk operations
- Search results

## Animation with Auto-Play

Combine with timer for automatic carousel rotation:

```csharp
public class CarouselPage : ContentPage
{
    private SfCarousel carousel;
    private System.Timers.Timer autoPlayTimer;
    
    public CarouselPage()
    {
        InitializeComponent();
        
        carousel = new SfCarousel
        {
            Duration = 800,
            ItemsSource = viewModel.ImageCollection,
            ItemTemplate = itemTemplate
        };
        
        StartAutoPlay();
        
        this.Content = carousel;
    }
    
    private void StartAutoPlay()
    {
        autoPlayTimer = new System.Timers.Timer(3000); // 3 seconds
        autoPlayTimer.Elapsed += (s, e) =>
        {
            MainThread.BeginInvokeOnMainThread(() =>
            {
                int nextIndex = (carousel.SelectedIndex + 1) % 
                                carousel.ItemsSource.Cast<object>().Count();
                carousel.SelectedIndex = nextIndex;
            });
        };
        autoPlayTimer.Start();
    }
    
    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        autoPlayTimer?.Stop();
    }
}
```

**Timing recommendations for auto-play:**
- Animation Duration: 800ms
- Auto-advance Interval: 3000-5000ms (3-5 seconds)
- Ensure interval > duration for smooth experience

## Animation Performance Considerations

### Optimal Duration Ranges

| Device Type | Recommended Range | Rationale |
|-------------|-------------------|-----------|
| **Phone** | 300-600ms | Smaller screens, faster feel |
| **Tablet** | 500-800ms | Medium screens, balanced |
| **Desktop** | 600-1000ms | Larger screens, can be slower |

### Performance Impact

**Shorter durations (<400ms):**
- ✅ Less CPU/GPU usage
- ✅ More responsive
- ⚠️ May appear abrupt

**Longer durations (>800ms):**
- ⚠️ More CPU/GPU usage during animation
- ⚠️ Can feel sluggish
- ✅ Smoother perceived motion

### Optimization Tips

1. **Match ViewMode and Duration:**
   ```csharp
   // Linear mode: faster animations
   carousel.ViewMode = ViewMode.Linear;
   carousel.Duration = 400;
   
   // Default mode (3D): slower animations look better
   carousel.ViewMode = ViewMode.Default;
   carousel.Duration = 700;
   ```

2. **Disable animation for programmatic changes:**
   ```csharp
   // Store original
   int originalDuration = carousel.Duration;
   
   // Disable temporarily
   carousel.Duration = 0;
   carousel.SelectedIndex = targetIndex;
   
   // Restore
   carousel.Duration = originalDuration;
   ```

3. **Consider content type:**
   - **Images:** 600-800ms (users want to see image clearly)
   - **Text cards:** 400-600ms (faster reading cadence)
   - **Videos/Rich media:** 500-700ms (balanced)

## User Preferences

Allow users to control animation speed:

```csharp
public class CarouselViewModel
{
    public int AnimationSpeed { get; set; } = 600;
    
    public void SetAnimationSpeed(string preference)
    {
        AnimationSpeed = preference switch
        {
            "Off" => 0,
            "Fast" => 300,
            "Normal" => 600,
            "Slow" => 900,
            _ => 600
        };
    }
}
```

```xml
<StackLayout>
    <Label Text="Animation Speed"/>
    <Picker SelectedIndexChanged="OnAnimationSpeedChanged">
        <Picker.Items>
            <x:String>Off</x:String>
            <x:String>Fast</x:String>
            <x:String>Normal</x:String>
            <x:String>Slow</x:String>
        </Picker.Items>
    </Picker>
    
    <carousel:SfCarousel Duration="{Binding AnimationSpeed}"
                         ItemsSource="{Binding ImageCollection}"
                         ItemTemplate="{StaticResource itemTemplate}"/>
</StackLayout>
```

## Accessibility Considerations

**Reduce motion preferences:**
```csharp
// Check system reduce motion preference
bool reduceMotion = Preferences.Get("ReduceMotion", false);

if (reduceMotion)
{
    carousel.Duration = 0; // Disable animations
}
else
{
    carousel.Duration = 600; // Normal animations
}
```

**Why this matters:**
- Some users experience motion sickness
- Vestibular disorders can be triggered by animations
- Respecting system preferences improves accessibility

## Best Practices

1. **Default to 600ms** unless you have a specific reason to change
2. **Test on actual devices** - emulators may not reflect real performance
3. **Match duration to content type** - images can be slower, text faster
4. **Respect user preferences** - allow customization when possible
5. **Consider context** - hero sections can be slower, utility carousels faster
6. **Test with users** - perceived speed varies by audience
7. **Use 0ms for programmatic changes** to avoid jarring effects

## Troubleshooting

**Animation feels too slow:**
→ Decrease Duration value (try 400-500ms)

**Animation feels too abrupt:**
→ Increase Duration value (try 700-900ms)

**Animation stutters or lags:**
→ Check if ViewMode=Default with complex items (3D transforms are GPU-intensive)
→ Optimize ItemTemplate complexity
→ Enable UI virtualization for large datasets

**No animation visible:**
→ Check if Duration is set to 0
→ Verify items are actually changing (check SelectedIndex)

**Animation speed inconsistent:**
→ Ensure Duration isn't being changed elsewhere in code
→ Check for performance issues affecting frame rate
