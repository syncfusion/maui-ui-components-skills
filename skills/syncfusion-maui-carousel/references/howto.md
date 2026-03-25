# How-To for .NET MAUI Carousel

## How-To: Perform Operations During Item Change

This section shows how to execute code when the carousel item changes.

### Using SelectionChanged Event

The `SelectionChanged` event is raised whenever the SelectedIndex changes, whether by user swipe or programmatic change.

**XAML:**
```xml
<carousel:SfCarousel x:Name="carousel"
                     ItemsSource="{Binding ImageCollection}"
                     ItemTemplate="{StaticResource itemTemplate}"
                     SelectionChanged="OnSelectionChanged"
                     ItemHeight="200"
                     ItemWidth="300"/>
```

**C# Event Handler:**
```csharp
private void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var carousel = sender as SfCarousel;
    
    if (carousel != null)
    {
        int oldIndex = e.OldIndex;
        int newIndex = e.NewIndex;
        
        Debug.WriteLine($"Selection changed from {oldIndex} to {newIndex}");
        
        // Perform operations based on new selection
        UpdateRelatedContent(newIndex);
    }
}
```

### Example 1: Update Related Content

Update other UI elements when carousel item changes:

```csharp
private void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var viewModel = BindingContext as ProductViewModel;
    
    if (viewModel != null && e.NewIndex >= 0 && e.NewIndex < viewModel.Products.Count)
    {
        var selectedProduct = viewModel.Products[e.NewIndex];
        
        // Update details panel
        productTitleLabel.Text = selectedProduct.Name;
        productPriceLabel.Text = $"${selectedProduct.Price:F2}";
        productDescriptionLabel.Text = selectedProduct.Description;
        
        // Update "Add to Cart" button
        addToCartButton.IsEnabled = selectedProduct.InStock;
        addToCartButton.Text = selectedProduct.InStock ? "Add to Cart" : "Out of Stock";
    }
}
```

### Example 2: Load Related Data

Fetch additional data when a new item is selected:

```csharp
private async void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var viewModel = BindingContext as ProductViewModel;
    
    if (viewModel != null && e.NewIndex >= 0)
    {
        var selectedProduct = viewModel.Products[e.NewIndex];
        
        // Show loading indicator
        loadingIndicator.IsVisible = true;
        loadingIndicator.IsRunning = true;
        
        try
        {
            // Load related products
            var relatedProducts = await ProductService.GetRelatedProductsAsync(selectedProduct.Id);
            viewModel.RelatedProducts.Clear();
            foreach (var product in relatedProducts)
            {
                viewModel.RelatedProducts.Add(product);
            }
            
            // Load reviews
            var reviews = await ReviewService.GetProductReviewsAsync(selectedProduct.Id);
            viewModel.Reviews.Clear();
            foreach (var review in reviews)
            {
                viewModel.Reviews.Add(review);
            }
        }
        finally
        {
            loadingIndicator.IsVisible = false;
            loadingIndicator.IsRunning = false;
        }
    }
}
```

### Example 3: Track Analytics

Log user behavior for analytics:

```csharp
private void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var carousel = sender as SfCarousel;
    var viewModel = BindingContext as ImageViewModel;
    
    if (carousel != null && viewModel != null && e.NewIndex >= 0)
    {
        var selectedItem = viewModel.Images[e.NewIndex];
        
        // Track page view
        AnalyticsService.TrackEvent("Carousel_ItemView", new Dictionary<string, string>
        {
            { "ItemIndex", e.NewIndex.ToString() },
            { "ItemId", selectedItem.Id },
            { "ItemName", selectedItem.Name },
            { "PreviousIndex", e.OldIndex.ToString() },
            { "NavigationType", e.OldIndex < e.NewIndex ? "Forward" : "Backward" }
        });
        
        Debug.WriteLine($"User viewed: {selectedItem.Name}");
    }
}
```

### Example 4: Save User Progress

Automatically save the user's position:

```csharp
private void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (e.NewIndex >= 0)
    {
        // Save to preferences
        Preferences.Set("LastCarouselIndex", e.NewIndex);
        Preferences.Set("LastViewedTime", DateTime.Now.ToString());
        
        Debug.WriteLine($"Progress saved: Index {e.NewIndex}");
    }
}

// Restore on page load
protected override void OnAppearing()
{
    base.OnAppearing();
    
    int savedIndex = Preferences.Get("LastCarouselIndex", 0);
    
    // Restore without animation
    int originalDuration = carousel.Duration;
    carousel.Duration = 0;
    carousel.SelectedIndex = savedIndex;
    carousel.Duration = originalDuration;
    
    Debug.WriteLine($"Restored to index: {savedIndex}");
}
```

### Example 5: Update Page Indicator

Sync page dots with carousel selection:

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    
    <carousel:SfCarousel Grid.Row="0"
                         x:Name="carousel"
                         ItemsSource="{Binding Images}"
                         ItemTemplate="{StaticResource imageTemplate}"
                         SelectionChanged="OnSelectionChanged"/>
    
    <StackLayout Grid.Row="1" 
                 x:Name="pageIndicators"
                 Orientation="Horizontal"
                 HorizontalOptions="Center"
                 Spacing="8"
                 Padding="20"/>
</Grid>
```

```csharp
public partial class CarouselPage : ContentPage
{
    public CarouselPage()
    {
        InitializeComponent();
        CreatePageIndicators();
    }
    
    private void CreatePageIndicators()
    {
        var viewModel = BindingContext as ImageViewModel;
        int itemCount = viewModel?.Images.Count ?? 0;
        
        for (int i = 0; i < itemCount; i++)
        {
            var dot = new BoxView
            {
                WidthRequest = 10,
                HeightRequest = 10,
                CornerRadius = 5,
                BackgroundColor = Colors.LightGray
            };
            pageIndicators.Children.Add(dot);
        }
        
        // Highlight first dot
        UpdatePageIndicators(0);
    }
    
    private void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
    {
        UpdatePageIndicators(e.NewIndex);
    }
    
    private void UpdatePageIndicators(int selectedIndex)
    {
        for (int i = 0; i < pageIndicators.Children.Count; i++)
        {
            if (pageIndicators.Children[i] is BoxView dot)
            {
                // Animate the change
                dot.FadeTo(i == selectedIndex ? 1.0 : 0.5, 200);
                dot.BackgroundColor = i == selectedIndex ? Colors.Blue : Colors.LightGray;
                
                if (i == selectedIndex)
                {
                    dot.ScaleTo(1.2, 200);
                }
                else
                {
                    dot.ScaleTo(1.0, 200);
                }
            }
        }
    }
}
```

### Example 6: Preload Adjacent Content

Optimize performance by preloading content for adjacent items:

```csharp
private async void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var viewModel = BindingContext as GalleryViewModel;
    
    if (viewModel != null)
    {
        int currentIndex = e.NewIndex;
        int totalItems = viewModel.Images.Count;
        
        // Preload previous item
        if (currentIndex > 0)
        {
            await viewModel.PreloadImageAsync(currentIndex - 1);
        }
        
        // Preload next item
        if (currentIndex < totalItems - 1)
        {
            await viewModel.PreloadImageAsync(currentIndex + 1);
        }
        
        Debug.WriteLine($"Preloaded adjacent images for index {currentIndex}");
    }
}
```

## SelectionChanged vs SwipeEnded

Both events can be used to respond to item changes, but they have different triggers:

| Event | Triggered When | Use Case |
|-------|----------------|----------|
| **SelectionChanged** | SelectedIndex changes (user swipe OR programmatic) | Always want to know about selection changes |
| **SwipeEnded** | User completes a swipe gesture | Only care about user-initiated navigation |

**Example - Using both:**
```csharp
private void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    // Always update UI when selection changes
    UpdateSelectedItemDetails(e.NewIndex);
}

private void OnSwipeEnded(object sender, EventArgs e)
{
    // Only track analytics for user swipes
    AnalyticsService.TrackEvent("User_Swiped_Carousel");
}
```

## Best Practices

1. **Use SelectionChanged for UI updates** - Always fires when selection changes
2. **Use SwipeEnded for user-specific tracking** - Only fires for user gestures
3. **Avoid heavy processing** - Keep event handlers lightweight
4. **Check index bounds** - Always validate `e.NewIndex` before accessing collections
5. **Provide feedback** - Update UI to acknowledge selection changes
6. **Save state** - Persist user position for better UX
7. **Preload content** - Load adjacent items for smoother experience

## Troubleshooting

**SelectionChanged not firing:**
→ Verify event is wired up in XAML or C#
→ Check if SelectedIndex is actually changing
→ Ensure carousel has items

**Event fires multiple times:**
→ Check if you're setting SelectedIndex programmatically in the handler (causes recursion)
→ Use a flag to prevent re-entry if needed

**Old index is incorrect:**
→ `e.OldIndex` may be -1 if this is the first selection
→ Always check for -1 before using old index

**Performance issues:**
→ Move heavy processing to async tasks
→ Use debouncing for frequent changes
→ Preload content instead of loading on-demand

## Additional Resources

- **Performance:** See [advanced-features.md](advanced-features.md) for UI virtualization
- **Events:** See [swipe-events.md](swipe-events.md) for SwipeStarted, Swiping, SwipeEnded
- **Data Binding:** See [populating-data.md](populating-data.md) for ObservableCollection usage
