# Advanced Features in .NET MAUI Carousel

## Table of Contents
- [Load More Functionality](#load-more-functionality)
  - [Enabling Load More](#enabling-load-more)
  - [Load More Items Count](#load-more-items-count)
  - [Customizing Load More View](#customizing-load-more-view)
  - [Load More Event](#load-more-event)
- [UI Virtualization](#ui-virtualization)
  - [Enabling Virtualization](#enabling-virtualization)
  - [Performance Benefits](#performance-benefits)
  - [Best Practices](#best-practices-for-virtualization)
- [Transformation Effects](#transformation-effects)
  - [Rotation Transforms](#rotation-transforms)
  - [Scale Transforms](#scale-transforms)
  - [Combined Effects](#combined-effects)
- [Performance Optimization](#performance-optimization)
- [Complete Examples](#complete-examples)

---

## Load More Functionality

The Load More feature allows you to dynamically load additional items when the user reaches the end of the carousel, providing better performance and user experience for large datasets.

### Enabling Load More

Use the `AllowLoadMore` property to enable load more functionality:

**XAML:**
```xml
<carousel:SfCarousel x:Name="carousel"
                     ItemsSource="{Binding ImageCollection}"
                     ItemTemplate="{StaticResource itemTemplate}"
                     AllowLoadMore="True"
                     ItemHeight="170"
                     ItemWidth="270"/>
```

**C#:**
```csharp
var carousel = new SfCarousel
{
    AllowLoadMore = true,
    ItemHeight = 170,
    ItemWidth = 270
};

carousel.ItemTemplate = itemTemplate;
carousel.SetBinding(SfCarousel.ItemsSourceProperty, "ImageCollection");
```

**Default:** `AllowLoadMore = false`

**When enabled:**
- A "Load More" view appears at the end of the carousel
- Tapping it triggers loading of additional items
- The LoadMore event is raised
- New items are appended to the ItemsSource

### Load More Items Count

The `LoadMoreItemsCount` property specifies how many items to load when the load more button is tapped.

**XAML:**
```xml
<carousel:SfCarousel AllowLoadMore="True"
                     LoadMoreItemsCount="10"
                     ItemsSource="{Binding ImageCollection}"
                     ItemTemplate="{StaticResource itemTemplate}"/>
```

**C#:**
```csharp
var carousel = new SfCarousel
{
    AllowLoadMore = true,
    LoadMoreItemsCount = 10
};
```

**Recommended values:**
- **Images/Photos:** 5-10 items per load
- **Text cards:** 10-20 items per load
- **Complex templates:** 5-10 items per load
- **Simple items:** 15-25 items per load

**Example with ViewModel:**
```csharp
public class CarouselViewModel
{
    public ObservableCollection<CarouselModel> ImageCollection { get; set; }
    private List<CarouselModel> allImages; // Complete dataset
    private int currentIndex = 0;
    
    public CarouselViewModel()
    {
        ImageCollection = new ObservableCollection<CarouselModel>();
        allImages = LoadAllImagesFromSource(); // Load complete data
        
        // Load initial batch
        LoadMoreImages();
    }
    
    public void LoadMoreImages()
    {
        int itemsToLoad = 10;
        int endIndex = Math.Min(currentIndex + itemsToLoad, allImages.Count);
        
        for (int i = currentIndex; i < endIndex; i++)
        {
            ImageCollection.Add(allImages[i]);
        }
        
        currentIndex = endIndex;
    }
    
    public bool HasMoreItems => currentIndex < allImages.Count;
}
```

### Customizing Load More View

The `LoadMoreView` property allows you to customize the appearance of the load more button.

**XAML:**
```xml
<carousel:SfCarousel AllowLoadMore="True"
                     ItemsSource="{Binding ImageCollection}"
                     ItemTemplate="{StaticResource itemTemplate}">
    <carousel:SfCarousel.LoadMoreView>
        <Grid BackgroundColor="LightGray" 
              Padding="20">
            <StackLayout HorizontalOptions="Center" 
                         VerticalOptions="Center">
                <Label Text="Load More Items" 
                       FontSize="16"
                       TextColor="DarkBlue"
                       FontAttributes="Bold"/>
                <Label Text="Tap to load 10 more" 
                       FontSize="12"
                       TextColor="Gray"/>
            </StackLayout>
        </Grid>
    </carousel:SfCarousel.LoadMoreView>
</carousel:SfCarousel>
```

**C# (Custom View):**
```csharp
var carousel = new SfCarousel
{
    AllowLoadMore = true
};

var loadMoreView = new Grid
{
    BackgroundColor = Colors.LightGray,
    Padding = 20
};

var stackLayout = new StackLayout
{
    HorizontalOptions = LayoutOptions.Center,
    VerticalOptions = LayoutOptions.Center
};

stackLayout.Children.Add(new Label
{
    Text = "Load More Items",
    FontSize = 16,
    TextColor = Colors.DarkBlue,
    FontAttributes = FontAttributes.Bold
});

stackLayout.Children.Add(new Label
{
    Text = "Tap to load 10 more",
    FontSize = 12,
    TextColor = Colors.Gray
});

loadMoreView.Children.Add(stackLayout);
carousel.LoadMoreView = loadMoreView;
```

**Custom Load More with Icon:**
```xml
<carousel:SfCarousel.LoadMoreView>
    <Frame CornerRadius="25" 
           HasShadow="True"
           BackgroundColor="White"
           Padding="15">
        <StackLayout Orientation="Horizontal" Spacing="10">
            <Image Source="refresh_icon.png" 
                   HeightRequest="24" 
                   WidthRequest="24"/>
            <Label Text="Load More" 
                   VerticalOptions="Center"
                   FontSize="14"
                   FontAttributes="Bold"/>
        </StackLayout>
    </Frame>
</carousel:SfCarousel.LoadMoreView>
```

### Load More Event

The `LoadMore` event is raised when the load more view is tapped.

**XAML:**
```xml
<carousel:SfCarousel AllowLoadMore="True"
                     LoadMoreItemsCount="10"
                     LoadMore="OnLoadMore"
                     ItemsSource="{Binding ImageCollection}"
                     ItemTemplate="{StaticResource itemTemplate}"/>
```

**C# Event Handler:**
```csharp
private void OnLoadMore(object sender, EventArgs e)
{
    var viewModel = BindingContext as CarouselViewModel;
    
    if (viewModel != null && viewModel.HasMoreItems)
    {
        viewModel.LoadMoreImages();
    }
    else
    {
        // All items loaded
        carousel.AllowLoadMore = false;
        DisplayAlert("Info", "All items loaded", "OK");
    }
}
```

**Complete Example with Loading Indicator:**
```csharp
private bool isLoading = false;

private async void OnLoadMore(object sender, EventArgs e)
{
    if (isLoading) return;
    
    isLoading = true;
    
    // Show loading indicator
    loadingIndicator.IsVisible = true;
    loadingIndicator.IsRunning = true;
    
    try
    {
        var viewModel = BindingContext as CarouselViewModel;
        
        // Simulate async data loading
        await Task.Run(() =>
        {
            Task.Delay(500).Wait(); // Simulate network delay
            
            MainThread.BeginInvokeOnMainThread(() =>
            {
                viewModel?.LoadMoreImages();
            });
        });
        
        // Check if more items available
        if (!viewModel.HasMoreItems)
        {
            carousel.AllowLoadMore = false;
        }
    }
    finally
    {
        isLoading = false;
        loadingIndicator.IsVisible = false;
        loadingIndicator.IsRunning = false;
    }
}
```

## UI Virtualization

UI Virtualization improves performance by creating UI elements only for visible items, recycling them as the user scrolls.

### Enabling Virtualization

Use the `EnableVirtualization` property:

**XAML:**
```xml
<carousel:SfCarousel EnableVirtualization="True"
                     ItemsSource="{Binding LargeImageCollection}"
                     ItemTemplate="{StaticResource itemTemplate}"
                     ItemHeight="200"
                     ItemWidth="300"/>
```

**C#:**
```csharp
var carousel = new SfCarousel
{
    EnableVirtualization = true,
    ItemHeight = 200,
    ItemWidth = 300
};

carousel.SetBinding(SfCarousel.ItemsSourceProperty, "LargeImageCollection");
```

**Default:** `EnableVirtualization = false`

### Performance Benefits

**Without Virtualization:**
- All items created upfront
- High memory usage for large datasets
- Slower initial load time
- Better for <50 items

**With Virtualization:**
- Only visible items created
- Low memory footprint
- Fast initial load
- Essential for >50 items

**Memory comparison (1000 images):**
```
Without Virtualization: ~500MB memory
With Virtualization: ~50MB memory
```

### When to Enable Virtualization

| Item Count | Recommendation |
|------------|----------------|
| < 20 items | Not needed |
| 20-50 items | Optional (minimal impact) |
| 50-100 items | Recommended |
| 100+ items | **Required** |

### Best Practices for Virtualization

1. **Set explicit ItemHeight and ItemWidth:**
   ```csharp
   carousel.ItemHeight = 200; // Required for virtualization
   carousel.ItemWidth = 300;  // Required for virtualization
   ```

2. **Optimize ItemTemplate:**
   ```xml
   <!-- Good: Simple, efficient -->
   <DataTemplate x:Key="efficientTemplate">
       <Image Source="{Binding Image}" Aspect="AspectFit"/>
   </DataTemplate>
   
   <!-- Avoid: Complex nested layouts -->
   <DataTemplate x:Key="complexTemplate">
       <Grid>
           <Grid>
               <Grid>
                   <!-- Too many nested levels -->
               </Grid>
           </Grid>
       </Grid>
   </DataTemplate>
   ```

3. **Use cached images:**
   ```csharp
   // Enable image caching
   var image = new Image
   {
       CachingEnabled = true,
       CacheValidity = TimeSpan.FromDays(1)
   };
   ```

4. **Combine with Load More:**
   ```xml
   <carousel:SfCarousel EnableVirtualization="True"
                        AllowLoadMore="True"
                        LoadMoreItemsCount="20"
                        ItemsSource="{Binding ImageCollection}"
                        ItemTemplate="{StaticResource itemTemplate}"/>
   ```

## Transformation Effects

### Rotation Transforms

Apply rotation to items in Default ViewMode:

**XAML:**
```xml
<carousel:SfCarousel ViewMode="Default"
                     RotationAngle="45"
                     ItemsSource="{Binding ImageCollection}"
                     ItemTemplate="{StaticResource itemTemplate}"
                     ItemHeight="200"
                     ItemWidth="300"/>
```

**C#:**
```csharp
var carousel = new SfCarousel
{
    ViewMode = ViewMode.Default,
    RotationAngle = 45,
    ItemHeight = 200,
    ItemWidth = 300
};
```

**Rotation values:**
- **0°:** No rotation (flat)
- **15-30°:** Subtle depth
- **30-60°:** Noticeable 3D effect
- **60-90°:** Strong perspective

### Scale Transforms

While not directly supported, you can apply scale in ItemTemplate:

```xml
<DataTemplate x:Key="scaledTemplate">
    <Grid>
        <Frame Scale="0.9" CornerRadius="10">
            <Image Source="{Binding Image}" Aspect="AspectFit"/>
        </Frame>
    </Grid>
</DataTemplate>
```

**Dynamic scaling based on selection:**
```csharp
carousel.SelectionChanged += (s, e) =>
{
    // Scale up selected item
    foreach (var child in carousel.Children)
    {
        if (child is Frame frame)
        {
            frame.ScaleTo(child == selectedItem ? 1.0 : 0.85, 250);
        }
    }
};
```

### Combined Effects

Combine rotation, offset, and custom transforms:

```xml
<carousel:SfCarousel ViewMode="Default"
                     RotationAngle="35"
                     Offset="250"
                     Duration="800"
                     ItemsSource="{Binding ImageCollection}"
                     ItemHeight="250"
                     ItemWidth="350">
    <carousel:SfCarousel.ItemTemplate>
        <DataTemplate>
            <Grid>
                <Frame CornerRadius="15" 
                       HasShadow="True"
                       Padding="5">
                    <Image Source="{Binding Image}" 
                           Aspect="AspectFill"/>
                </Frame>
            </Grid>
        </DataTemplate>
    </carousel:SfCarousel.ItemTemplate>
</carousel:SfCarousel>
```

## Performance Optimization

### Complete Optimization Strategy

```csharp
public class OptimizedCarouselPage : ContentPage
{
    public OptimizedCarouselPage()
    {
        InitializeComponent();
        
        var carousel = new SfCarousel
        {
            // Enable virtualization for large datasets
            EnableVirtualization = true,
            
            // Load more for dynamic loading
            AllowLoadMore = true,
            LoadMoreItemsCount = 15,
            
            // Optimize dimensions
            ItemHeight = 200,
            ItemWidth = 300,
            
            // Reasonable animation
            Duration = 500,
            
            // Use linear mode for better performance
            ViewMode = ViewMode.Linear
        };
        
        // Simple, efficient template
        var itemTemplate = new DataTemplate(() =>
        {
            var image = new Image
            {
                Aspect = Aspect.AspectFit,
                CachingEnabled = true
            };
            image.SetBinding(Image.SourceProperty, "Image");
            return image;
        });
        
        carousel.ItemTemplate = itemTemplate;
        carousel.SetBinding(SfCarousel.ItemsSourceProperty, "ImageCollection");
        
        this.Content = carousel;
    }
}
```

### Monitoring Performance

```csharp
// Track memory usage
long memoryBefore = GC.GetTotalMemory(false);

// After carousel loads
long memoryAfter = GC.GetTotalMemory(false);
long memoryUsed = (memoryAfter - memoryBefore) / 1024 / 1024; // MB

Debug.WriteLine($"Memory used by carousel: {memoryUsed} MB");
```

## Complete Examples

### Example 1: Large Photo Gallery

```xml
<carousel:SfCarousel EnableVirtualization="True"
                     AllowLoadMore="True"
                     LoadMoreItemsCount="20"
                     LoadMore="OnLoadMorePhotos"
                     ViewMode="Linear"
                     ItemHeight="250"
                     ItemWidth="350"
                     ItemsSource="{Binding Photos}"
                     ItemTemplate="{StaticResource photoTemplate}">
    <carousel:SfCarousel.LoadMoreView>
        <Frame BackgroundColor="LightGray" Padding="15">
            <Label Text="Load 20 More Photos" 
                   HorizontalOptions="Center"
                   FontAttributes="Bold"/>
        </Frame>
    </carousel:SfCarousel.LoadMoreView>
</carousel:SfCarousel>
```

### Example 2: Infinite Scrolling Product Catalog

```csharp
public class ProductCarouselPage : ContentPage
{
    private SfCarousel carousel;
    private ProductViewModel viewModel;
    
    public ProductCarouselPage()
    {
        viewModel = new ProductViewModel();
        BindingContext = viewModel;
        
        carousel = new SfCarousel
        {
            EnableVirtualization = true,
            AllowLoadMore = true,
            LoadMoreItemsCount = 10,
            ViewMode = ViewMode.Linear,
            ItemHeight = 300,
            ItemWidth = 250,
            Duration = 500
        };
        
        carousel.LoadMore += OnLoadMoreProducts;
        carousel.SetBinding(SfCarousel.ItemsSourceProperty, "Products");
        
        this.Content = carousel;
    }
    
    private async void OnLoadMoreProducts(object sender, EventArgs e)
    {
        await viewModel.LoadMoreProductsAsync();
        
        if (!viewModel.HasMoreProducts)
        {
            carousel.AllowLoadMore = false;
        }
    }
}
```

## Troubleshooting

**Load More not appearing:**
→ Ensure `AllowLoadMore="True"` is set
→ Verify ItemsSource has items
→ Check if HasMoreItems logic is correct

**Virtualization not improving performance:**
→ Set explicit ItemHeight and ItemWidth
→ Simplify ItemTemplate (remove nested layouts)
→ Ensure dataset is large enough (>50 items)

**Memory still high with virtualization:**
→ Check for image caching issues
→ Reduce image sizes before binding
→ Use image optimization libraries

**Load More event not firing:**
→ Verify LoadMore event is wired up
→ Check if AllowLoadMore is still true
→ Ensure user can reach the end of carousel
