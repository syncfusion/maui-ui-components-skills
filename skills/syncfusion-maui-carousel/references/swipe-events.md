# Swipe Events in .NET MAUI Carousel

## Table of Contents
- [Overview](#overview)
- [SwipeStarted Event](#swipestarted-event)
- [Swiping Event](#swiping-event)
- [SwipeEnded Event](#swipeended-event)
- [Event Arguments](#event-arguments)
- [Complete Examples](#complete-examples)
- [Common Use Cases](#common-use-cases)

---

## Overview

The SfCarousel control provides three swipe-related events that allow you to respond to user gestures:

1. **SwipeStarted** - Raised when a swipe gesture begins
2. **Swiping** - Raised continuously during the swipe gesture
3. **SwipeEnded** - Raised when the swipe gesture completes

These events enable you to create interactive experiences, track user behavior, provide feedback, and implement custom navigation logic.

## SwipeStarted Event

The `SwipeStarted` event is raised when the user begins a swipe gesture on the carousel.

**XAML:**
```xml
<carousel:SfCarousel x:Name="carousel"
                     ItemsSource="{Binding ImageCollection}"
                     ItemTemplate="{StaticResource itemTemplate}"
                     SwipeStarted="OnSwipeStarted"
                     ItemHeight="200"
                     ItemWidth="300"/>
```

**C#:**
```csharp
var carousel = new SfCarousel
{
    ItemHeight = 200,
    ItemWidth = 300
};

carousel.SwipeStarted += OnSwipeStarted;
carousel.SetBinding(SfCarousel.ItemsSourceProperty, "ImageCollection");
```

**Event Handler:**
```csharp
private void OnSwipeStarted(object sender, EventArgs e)
{
    Debug.WriteLine("User started swiping");
    
    // Provide visual feedback
    swipeFeedbackLabel.Text = "Swiping...";
    swipeFeedbackLabel.IsVisible = true;
    
    // Track analytics
    AnalyticsService.TrackEvent("Carousel_SwipeStarted");
}
```

**When to use:**
- Show loading indicators
- Pause auto-play
- Track user engagement
- Provide visual feedback

## Swiping Event

The `Swiping` event is raised continuously while the user is actively swiping.

**XAML:**
```xml
<carousel:SfCarousel x:Name="carousel"
                     ItemsSource="{Binding ImageCollection}"
                     ItemTemplate="{StaticResource itemTemplate}"
                     Swiping="OnSwiping"
                     ItemHeight="200"
                     ItemWidth="300"/>
```

**C#:**
```csharp
carousel.Swiping += OnSwiping;
```

**Event Handler:**
```csharp
private void OnSwiping(object sender, EventArgs e)
{
    Debug.WriteLine("Swiping in progress");
    
    // Update progress indicator
    UpdateSwipeProgress();
    
    // Provide haptic feedback (optional)
    HapticFeedback.Perform(HapticFeedbackType.Click);
}
```

**Warning:** This event fires frequently during a swipe. Keep the handler lightweight to avoid performance issues.

**When to use:**
- Real-time progress indicators
- Live preview updates
- Haptic feedback
- Gesture tracking

**Performance tip:**
```csharp
private DateTime lastSwipingUpdate = DateTime.MinValue;

private void OnSwiping(object sender, EventArgs e)
{
    // Throttle updates to every 100ms
    if ((DateTime.Now - lastSwipingUpdate).TotalMilliseconds < 100)
        return;
    
    lastSwipingUpdate = DateTime.Now;
    
    // Perform updates
    UpdateSwipeIndicator();
}
```

## SwipeEnded Event

The `SwipeEnded` event is raised when the swipe gesture completes and the carousel settles on an item.

**XAML:**
```xml
<carousel:SfCarousel x:Name="carousel"
                     ItemsSource="{Binding ImageCollection}"
                     ItemTemplate="{StaticResource itemTemplate}"
                     SwipeEnded="OnSwipeEnded"
                     ItemHeight="200"
                     ItemWidth="300"/>
```

**C#:**
```csharp
carousel.SwipeEnded += OnSwipeEnded;
```

**Event Handler:**
```csharp
private void OnSwipeEnded(object sender, EventArgs e)
{
    Debug.WriteLine("Swipe completed");
    
    // Hide feedback
    swipeFeedbackLabel.IsVisible = false;
    
    // Update related UI
    UpdateContentBasedOnSelection();
    
    // Resume auto-play if enabled
    ResumeAutoPlay();
    
    // Track analytics
    AnalyticsService.TrackEvent("Carousel_SwipeCompleted");
}
```

**When to use:**
- Update related content
- Resume auto-play
- Save state
- Track navigation
- Load additional data

## Event Arguments

All three swipe events provide `EventArgs` (basic event data). To access carousel state, use the carousel's properties:

```csharp
private void OnSwipeEnded(object sender, EventArgs e)
{
    var carousel = sender as SfCarousel;
    
    if (carousel != null)
    {
        int currentIndex = carousel.SelectedIndex;
        var currentItem = carousel.ItemsSource.Cast<object>().ElementAtOrDefault(currentIndex);
        
        Debug.WriteLine($"Settled on index: {currentIndex}");
        Debug.WriteLine($"Current item: {currentItem}");
    }
}
```

## Complete Examples

### Example 1: Pause Auto-Play During Swipe

```csharp
public class AutoPlayCarouselPage : ContentPage
{
    private SfCarousel carousel;
    private System.Timers.Timer autoPlayTimer;
    private bool isUserSwiping = false;
    
    public AutoPlayCarouselPage()
    {
        InitializeComponent();
        
        carousel = this.FindByName<SfCarousel>("carousel");
        carousel.SwipeStarted += OnSwipeStarted;
        carousel.SwipeEnded += OnSwipeEnded;
        
        StartAutoPlay();
    }
    
    private void OnSwipeStarted(object sender, EventArgs e)
    {
        isUserSwiping = true;
        autoPlayTimer?.Stop();
        Debug.WriteLine("Auto-play paused");
    }
    
    private void OnSwipeEnded(object sender, EventArgs e)
    {
        isUserSwiping = false;
        
        // Resume auto-play after 2 seconds
        Task.Delay(2000).ContinueWith(t =>
        {
            if (!isUserSwiping)
            {
                MainThread.BeginInvokeOnMainThread(() =>
                {
                    autoPlayTimer?.Start();
                    Debug.WriteLine("Auto-play resumed");
                });
            }
        });
    }
    
    private void StartAutoPlay()
    {
        autoPlayTimer = new System.Timers.Timer(3000);
        autoPlayTimer.Elapsed += (s, e) =>
        {
            if (!isUserSwiping)
            {
                MainThread.BeginInvokeOnMainThread(() =>
                {
                    int nextIndex = (carousel.SelectedIndex + 1) % 
                                    carousel.ItemsSource.Cast<object>().Count();
                    carousel.SelectedIndex = nextIndex;
                });
            }
        };
        autoPlayTimer.Start();
    }
    
    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        autoPlayTimer?.Stop();
        autoPlayTimer?.Dispose();
    }
}
```

### Example 2: Update Content Based on Selection

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    
    <!-- Carousel -->
    <carousel:SfCarousel Grid.Row="1"
                         x:Name="carousel"
                         ItemsSource="{Binding Products}"
                         ItemTemplate="{StaticResource productTemplate}"
                         SwipeEnded="OnProductSwipeEnded"
                         ItemHeight="250"
                         ItemWidth="300"/>
    
    <!-- Product Details (updated on swipe) -->
    <Frame Grid.Row="2" Padding="20">
        <StackLayout>
            <Label x:Name="productNameLabel" 
                   FontSize="20" 
                   FontAttributes="Bold"/>
            <Label x:Name="productPriceLabel" 
                   FontSize="16" 
                   TextColor="Green"/>
            <Label x:Name="productDescriptionLabel" 
                   FontSize="14"/>
        </StackLayout>
    </Frame>
</Grid>
```

```csharp
private void OnProductSwipeEnded(object sender, EventArgs e)
{
    var carousel = sender as SfCarousel;
    var viewModel = BindingContext as ProductViewModel;
    
    if (carousel != null && viewModel != null)
    {
        var currentProduct = viewModel.Products[carousel.SelectedIndex];
        
        // Update detail panel
        productNameLabel.Text = currentProduct.Name;
        productPriceLabel.Text = $"${currentProduct.Price:F2}";
        productDescriptionLabel.Text = currentProduct.Description;
        
        // Animate detail panel
        var detailFrame = productNameLabel.Parent.Parent as Frame;
        detailFrame.FadeTo(0, 150).ContinueWith(t =>
        {
            MainThread.BeginInvokeOnMainThread(() =>
            {
                detailFrame.FadeTo(1, 150);
            });
        });
    }
}
```

### Example 3: Progress Indicator with Swipe Events

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    
    <!-- Progress indicator -->
    <StackLayout Grid.Row="0" 
                 Orientation="Horizontal" 
                 HorizontalOptions="Center"
                 Padding="10">
        <Label x:Name="progressLabel" 
               Text="1 / 5" 
               FontSize="14"
               TextColor="Gray"/>
    </StackLayout>
    
    <!-- Carousel -->
    <carousel:SfCarousel Grid.Row="1"
                         x:Name="carousel"
                         ItemsSource="{Binding Images}"
                         ItemTemplate="{StaticResource imageTemplate}"
                         SwipeEnded="OnSwipeEnded"
                         ItemHeight="300"
                         ItemWidth="400"/>
    
    <!-- Dot indicators -->
    <StackLayout Grid.Row="2" 
                 x:Name="dotIndicators"
                 Orientation="Horizontal"
                 HorizontalOptions="Center"
                 Spacing="10"
                 Padding="20"/>
</Grid>
```

```csharp
public partial class ImageGalleryPage : ContentPage
{
    public ImageGalleryPage()
    {
        InitializeComponent();
        CreateDotIndicators();
        UpdateIndicators(0);
    }
    
    private void CreateDotIndicators()
    {
        int itemCount = (BindingContext as ImageViewModel)?.Images.Count ?? 0;
        
        for (int i = 0; i < itemCount; i++)
        {
            var dot = new BoxView
            {
                WidthRequest = 8,
                HeightRequest = 8,
                CornerRadius = 4,
                BackgroundColor = Colors.LightGray
            };
            dotIndicators.Children.Add(dot);
        }
    }
    
    private void OnSwipeEnded(object sender, EventArgs e)
    {
        var carousel = sender as SfCarousel;
        if (carousel != null)
        {
            UpdateIndicators(carousel.SelectedIndex);
        }
    }
    
    private void UpdateIndicators(int selectedIndex)
    {
        int totalItems = (BindingContext as ImageViewModel)?.Images.Count ?? 0;
        
        // Update text indicator
        progressLabel.Text = $"{selectedIndex + 1} / {totalItems}";
        
        // Update dot indicators
        for (int i = 0; i < dotIndicators.Children.Count; i++)
        {
            if (dotIndicators.Children[i] is BoxView dot)
            {
                dot.BackgroundColor = i == selectedIndex 
                    ? Colors.DarkBlue 
                    : Colors.LightGray;
                dot.WidthRequest = i == selectedIndex ? 10 : 8;
                dot.HeightRequest = i == selectedIndex ? 10 : 8;
            }
        }
    }
}
```

### Example 4: Swipe Gesture Tracking

```csharp
public class CarouselAnalyticsPage : ContentPage
{
    private int swipeCount = 0;
    private DateTime sessionStart;
    
    public CarouselAnalyticsPage()
    {
        InitializeComponent();
        sessionStart = DateTime.Now;
        
        carousel.SwipeStarted += OnSwipeStarted;
        carousel.Swiping += OnSwiping;
        carousel.SwipeEnded += OnSwipeEnded;
    }
    
    private void OnSwipeStarted(object sender, EventArgs e)
    {
        Debug.WriteLine($"[{DateTime.Now:HH:mm:ss}] Swipe started");
    }
    
    private void OnSwiping(object sender, EventArgs e)
    {
        // Track during swipe (throttled)
    }
    
    private void OnSwipeEnded(object sender, EventArgs e)
    {
        swipeCount++;
        var carousel = sender as SfCarousel;
        
        if (carousel != null)
        {
            var sessionDuration = DateTime.Now - sessionStart;
            
            // Log analytics
            AnalyticsService.TrackEvent("Carousel_Swipe", new Dictionary<string, string>
            {
                { "CurrentIndex", carousel.SelectedIndex.ToString() },
                { "TotalSwipes", swipeCount.ToString() },
                { "SessionDuration", sessionDuration.TotalSeconds.ToString("F0") },
                { "EngagementRate", (swipeCount / sessionDuration.TotalMinutes).ToString("F2") }
            });
            
            Debug.WriteLine($"Total swipes: {swipeCount}, Session: {sessionDuration.TotalSeconds:F0}s");
        }
    }
}
```

## Common Use Cases

### Use Case 1: Tutorial/Onboarding Navigation

Track user progress through onboarding screens:

```csharp
private int currentStep = 0;
private int totalSteps = 5;

private void OnSwipeEnded(object sender, EventArgs e)
{
    currentStep = carousel.SelectedIndex;
    
    // Update UI
    stepLabel.Text = $"Step {currentStep + 1} of {totalSteps}";
    
    // Show/hide navigation buttons
    previousButton.IsVisible = currentStep > 0;
    nextButton.Text = currentStep == totalSteps - 1 ? "Get Started" : "Next";
    
    // Track completion
    if (currentStep == totalSteps - 1)
    {
        AnalyticsService.TrackEvent("Onboarding_Completed");
    }
}
```

### Use Case 2: Content Preloading

Preload adjacent content for smooth experience:

```csharp
private void OnSwipeEnded(object sender, EventArgs e)
{
    var carousel = sender as SfCarousel;
    var viewModel = BindingContext as ContentViewModel;
    
    if (carousel != null && viewModel != null)
    {
        int currentIndex = carousel.SelectedIndex;
        
        // Preload previous and next items
        if (currentIndex > 0)
        {
            viewModel.PreloadItem(currentIndex - 1);
        }
        if (currentIndex < viewModel.Items.Count - 1)
        {
            viewModel.PreloadItem(currentIndex + 1);
        }
    }
}
```

### Use Case 3: Save User Progress

Automatically save user's position:

```csharp
private void OnSwipeEnded(object sender, EventArgs e)
{
    var carousel = sender as SfCarousel;
    
    if (carousel != null)
    {
        // Save current position
        Preferences.Set("LastCarouselIndex", carousel.SelectedIndex);
        Preferences.Set("LastViewedTime", DateTime.Now.ToString());
        
        Debug.WriteLine($"Progress saved: Index {carousel.SelectedIndex}");
    }
}

// Restore on load
protected override void OnAppearing()
{
    base.OnAppearing();
    
    int lastIndex = Preferences.Get("LastCarouselIndex", 0);
    carousel.SelectedIndex = lastIndex;
}
```

### Use Case 4: Dynamic Content Loading

Load content based on navigation:

```csharp
private async void OnSwipeEnded(object sender, EventArgs e)
{
    var carousel = sender as SfCarousel;
    var viewModel = BindingContext as DynamicViewModel;
    
    if (carousel != null && viewModel != null)
    {
        int currentIndex = carousel.SelectedIndex;
        
        // Check if approaching end
        if (currentIndex >= viewModel.Items.Count - 3 && viewModel.HasMoreItems)
        {
            // Load more items
            await viewModel.LoadMoreItemsAsync();
        }
    }
}
```

## Best Practices

1. **Keep handlers lightweight:** Avoid heavy processing in event handlers, especially in Swiping
2. **Use SwipeEnded for state updates:** Most updates should happen after swipe completes
3. **Throttle Swiping events:** If using Swiping, throttle updates to avoid performance issues
4. **Track user engagement:** Use events for analytics to understand user behavior
5. **Provide feedback:** Give users visual confirmation that their gesture was registered
6. **Coordinate with auto-play:** Pause auto-play during user interaction
7. **Save state:** Use SwipeEnded to save user progress or preferences

## Troubleshooting

**Events not firing:**
→ Verify event handlers are wired up correctly in XAML or C#
→ Check if carousel has items (`ItemsSource` is not empty)
→ Ensure carousel is visible and not disabled

**Swiping event causing performance issues:**
→ Implement throttling (limit updates to every 100-200ms)
→ Move heavy processing to SwipeEnded instead
→ Avoid complex UI updates during Swiping

**Auto-play conflicts with swipe events:**
→ Pause auto-play timer in SwipeStarted
→ Resume in SwipeEnded after a delay
→ Track isUserSwiping flag to prevent conflicts

**State not updating correctly:**
→ Use SwipeEnded instead of SwipeStarted for state updates
→ Ensure you're accessing SelectedIndex after swipe completes
→ Use MainThread.BeginInvokeOnMainThread for UI updates from timers
