# Events and Performance Optimization

## Table of Contents
- [Lifecycle Events](#lifecycle-events)
- [Interaction Events](#interaction-events)
- [Item Appearance Events](#item-appearance-events)
- [Performance Optimization](#performance-optimization)
- [RefreshView Method](#refreshview-method)
- [Troubleshooting Performance](#troubleshooting-performance)

## Lifecycle Events

### Loaded Event

Raised when ListView completes loading:

```csharp
listView.Loaded += (sender, e) =>
{
    // ListView is fully loaded and rendered
    // Safe to perform operations on the ListView
    Debug.WriteLine("ListView loaded successfully");
    
    // Example: Scroll to specific item
    listView.ScrollToRowIndex(5);
};
```

**Common Uses:**
- Initial scroll positioning
- Post-load data operations
- Analytics tracking
- UI adjustments after render

## Interaction Events

### ItemTapped Event

Raised when item is tapped:

```csharp
listView.ItemTapped += (sender, e) =>
{
    var tappedItem = e.DataItem as Product;
    var index = e.ItemIndex;
    var itemData = e.ItemData;
    
    Debug.WriteLine($"Tapped: {tappedItem.Name} at index {index}");
    
    // Navigate to detail page
    Navigation.PushAsync(new ProductDetailPage(tappedItem));
};
```

**Event Arguments:**
- `DataItem`: The tapped item object
- `ItemIndex`: Index of the tapped item
- `ItemData`: Additional data (if any)

### ItemDoubleTapped Event

Raised when item is double-tapped:

```csharp
listView.ItemDoubleTapped += (sender, e) =>
{
    var item = e.DataItem as Product;
    
    // Quick action on double-tap
    item.IsFavorite = !item.IsFavorite;
    
    Debug.WriteLine($"Double-tapped: {item.Name}");
};
```

**Use Cases:**
- Toggle favorite/bookmark
- Quick edit mode
- Expand/collapse inline details

### ItemRightTapped Event

Raised on right-click (desktop platforms):

```csharp
listView.ItemRightTapped += (sender, e) =>
{
    var item = e.DataItem as Product;
    
    // Show context menu
    ShowContextMenu(item, e.Position);
};
```

**Platform Support:**
- Windows
- macOS
- Not available on mobile

### ItemLongPress Event

Raised when item is long-pressed (500ms default):

```csharp
listView.ItemLongPress += (sender, e) =>
{
    var item = e.DataItem as Product;
    
    // Show action sheet
    DisplayActionSheet(
        $"Actions for {item.Name}",
        "Cancel",
        "Delete",
        "Edit", "Share", "Favorite"
    );
};
```

**Common Patterns:**
```csharp
using Syncfusion.Maui.ListView;

// Selection mode toggle
private void OnItemLongPress(object sender, ItemLongPressEventArgs e)
{
    if (listView.SelectionMode == SelectionMode.None)
    {
        // Enable selection mode
        listView.SelectionMode = SelectionMode.Multiple;
        listView.SelectedItems.Add(e.DataItem);
        
        // Show selection toolbar
        selectionToolbar.IsVisible = true;
    }
}

// Context menu
private async void OnItemLongPress(object sender, ItemLongPressEventArgs e)
{
    var item = e.DataItem as Product;
    
    var action = await DisplayActionSheet(
        "Choose action",
        "Cancel",
        "Delete",
        "Edit", "Duplicate", "Share"
    );
    
    switch (action)
    {
        case "Delete":
            viewModel.Products.Remove(item);
            break;
        case "Edit":
            await Navigation.PushAsync(new EditProductPage(item));
            break;
        case "Duplicate":
            viewModel.DuplicateProduct(item);
            break;
        case "Share":
            await Share.RequestAsync(new ShareTextRequest
            {
                Text = $"Check out {item.Name}",
                Title = "Share Product"
            });
            break;
    }
}
```

### SelectionChanging Event

Raised before selection changes (cancelable):

```csharp
listView.SelectionChanging += (sender, e) =>
{
    var addedItems = e.AddedItems;
    var removedItems = e.RemovedItems;
    
    // Validate selection
    foreach (var item in addedItems)
    {
        var product = item as Product;
        if (product != null && !product.IsSelectable)
        {
            // Cancel selection
            e.Cancel = true;
            DisplayAlert("Error", "This item cannot be selected", "OK");
            return;
        }
    }
};
```

### SelectionChanged Event

Raised after selection changes:

```csharp
listView.SelectionChanged += (sender, e) =>
{
    var addedItems = e.AddedItems;
    var removedItems = e.RemovedItems;
    var currentSelection = e.CurrentSelection;
    
    Debug.WriteLine($"Selected: {addedItems.Count}, Deselected: {removedItems.Count}");
    Debug.WriteLine($"Total selected: {currentSelection.Count}");
    
    // Update UI
    UpdateSelectionCount(currentSelection.Count);
    
    // Enable/disable action buttons
    deleteButton.IsEnabled = currentSelection.Count > 0;
};
```

### SwipeStarting, Swiping, SwipeEnded Events

Track swipe gestures:

```csharp
listView.SwipeStarting += (sender, e) =>
{
    Debug.WriteLine($"Swipe starting on item: {e.ItemIndex}");
};

listView.Swiping += (sender, e) =>
{
    var offset = e.Offset;
    var direction = e.SwipeDirection;
    
    // Update visual feedback based on swipe progress
    Debug.WriteLine($"Swiping {direction}: {offset}");
};

listView.SwipeEnded += (sender, e) =>
{
    var item = e.DataItem as Product;
    
    if (e.SwipeOffset > 100)
    {
        // Trigger action (delete, archive, etc.)
        viewModel.DeleteProduct(item);
    }
};
```

### ScrollStateChanged Event

Monitor scroll state changes:

```csharp
listView.ScrollStateChanged += (sender, e) =>
{
    var state = e.ScrollState;
    
    switch (state)
    {
        case ScrollState.Idle:
            Debug.WriteLine("Scrolling stopped");
            // Hide scroll-to-top button if at top
            break;
            
        case ScrollState.Dragging:
            Debug.WriteLine("User is dragging");
            // Hide keyboard or tooltips
            break;
            
        case ScrollState.Fling:
            Debug.WriteLine("Fling/momentum scrolling");
            break;
            
        case ScrollState.Programmatic:
            Debug.WriteLine("Programmatic scroll");
            break;
    }
};
```

## Item Appearance Events

### ItemAppearing Event

Raised when item is about to appear in viewport:

```csharp
listView.ItemAppearing += (sender, e) =>
{
    var item = e.DataItem as Product;
    
    Debug.WriteLine($"Item appearing: {item.Name}");
};
```

**Use Cases:**
- Infinite scroll/pagination
- Lazy load images
- Analytics tracking
- Preload related data

### ItemDisappearing Event

Raised when item leaves viewport:

```csharp
listView.ItemDisappearing += (sender, e) =>
{
    var item = e.DataItem as Product;
    
    Debug.WriteLine($"Item disappearing: {item.Name}");
    
    // Cleanup or pause operations
};
```

**Use Cases:**
- Pause videos/animations
- Cancel pending operations
- Free resources
- Track viewing duration

## Performance Optimization

### 1. Use IList<T> Instead of IEnumerable<T>

```csharp
// ✓ GOOD - Direct indexing
public IList<Product> Products { get; set; } = new List<Product>();
public ObservableCollection<Product> Products { get; set; }

// ❌ SLOW - Requires enumeration
public IEnumerable<Product> Products { get; set; }
```

**Why:** ListView uses indexing for virtualization. IEnumerable requires enumeration to access items by index.

### 2. Use BeginInit() and EndInit() for Bulk Operations

```csharp
// ✓ EFFICIENT - Single refresh
listView.DataSource.BeginInit();

foreach (var product in newProducts)
{
    viewModel.Products.Add(product);
}

listView.DataSource.EndInit();
listView.RefreshView();

// ❌ SLOW - Refreshes on each add
foreach (var product in newProducts)
{
    viewModel.Products.Add(product);
}
```

### 3. Avoid Complex ItemTemplates

```xml
<!-- ✓ GOOD - Simple, flat structure -->
<DataTemplate>
    <Grid Padding="10" ColumnDefinitions="Auto,*,Auto">
        <Image Grid.Column="0" Source="{Binding ImageUrl}" />
        <StackLayout Grid.Column="1">
            <Label Text="{Binding Name}" />
            <Label Text="{Binding Price}" />
        </StackLayout>
        <Button Grid.Column="2" Text="Add" />
    </Grid>
</DataTemplate>

<!-- ❌ SLOW - Nested, complex hierarchy -->
<DataTemplate>
    <Frame>
        <Grid>
            <StackLayout>
                <Frame>
                    <Grid>
                        <StackLayout>
                            <!-- Deep nesting -->
                        </StackLayout>
                    </Grid>
                </Frame>
            </StackLayout>
        </Grid>
    </Frame>
</DataTemplate>
```

**Tips:**
- Keep view hierarchy shallow (3-4 levels max)
- Avoid nested Frames/Borders
- Use Grid instead of nested StackLayouts
- Minimize bindings per item

### 4. Set Fixed ItemSize

```csharp
// ✓ FAST - No measurement needed
listView.ItemSize = 80;

// ❌ SLOWER - Measures each item
listView.AutoFitMode = AutoFitMode.Height;
```

**Use AutoFit only when:**
- Item heights vary significantly
- Dynamic content sizing is required

### 5. Avoid ScrollView as Parent

```xml
<!-- ❌ WRONG - Disables virtualization -->
<ScrollView>
    <syncfusion:SfListView ItemsSource="{Binding Products}" />
</ScrollView>

<!-- ✓ CORRECT - ListView handles scrolling -->
<syncfusion:SfListView ItemsSource="{Binding Products}" />
```

**Why:** ListView has built-in virtualization. Wrapping in ScrollView forces all items to render.

### 6. Use CachingStrategy for Complex Templates

```csharp
// Enable view recycling
listView.CachingStrategy = CachingStrategy.RecycleTemplate;
```

### 7. Optimize Data Binding

```csharp
// ✓ GOOD - Implement INotifyPropertyChanged only for changing properties
public class Product : INotifyPropertyChanged
{
    public int Id { get; set; } // No INPC needed
    
    private string name;
    public string Name
    {
        get => name;
        set { name = value; OnPropertyChanged(); } // INPC for changing property
    }
}

// ❌ UNNECESSARY - INPC on immutable properties
private int id;
public int Id
{
    get => id;
    set { id = value; OnPropertyChanged(); }
}
```

### 8. Defer Image Loading

```csharp
// Use CachedImage or FFImageLoading
<ffimageloading:CachedImage Source="{Binding ImageUrl}"
                            DownsampleToViewSize="True"
                            CacheDuration="30" />
```

### 9. Limit Data Loading

```csharp
// Load data incrementally
private const int PageSize = 20;

public async Task LoadPageAsync(int page)
{
    var items = await dataService.GetItemsAsync(page, PageSize);
    
    listView.DataSource.BeginInit();
    
    foreach (var item in items)
    {
        Products.Add(item);
    }
    
    listView.DataSource.EndInit();
    listView.RefreshView();
}
```

### 10. Disable Animations for Large Datasets

```csharp
// Disable animations during bulk updates
listView.ItemSpacingX = 0;
listView.ItemSpacingY = 0;
listView.AllowSwiping = false; // If not needed

// Re-enable after load
await LoadDataAsync();
listView.AllowSwiping = true;
```

## RefreshView Method

Force ListView to refresh its layout and items:

```csharp
// Refresh entire ListView
listView.RefreshView();

// Refresh range of items
listView.RefreshListViewItem(startIndex, endIndex);
```

**When to Use:**
```csharp
// After property changes that don't trigger INPC
product.UpdateInternalState();
listView.RefreshItem(Products.IndexOf(product), Products.IndexOf(product));

// After bulk data modifications
listView.DataSource.BeginInit();
// ... modify data ...
listView.DataSource.EndInit();
listView.RefreshView();

// After changing ItemTemplate
listView.ItemTemplate = newTemplate;
listView.RefreshView();

// After visibility/layout changes
listView.IsVisible = true;
listView.RefreshView();
```

## Troubleshooting Performance

### Issue: Scrolling Lag/Stuttering

**Causes:**
- Complex ItemTemplate with deep nesting
- Image loading without caching
- Synchronous operations in ItemAppearing
- AutoFitMode with variable heights

**Solutions:**
```csharp
// 1. Set fixed ItemSize
listView.ItemSize = 100;

// 2. Simplify ItemTemplate
// Reduce view hierarchy depth

// 3. Use async image loading
// Implement CachedImage

// 4. Defer expensive operations
listView.ItemAppearing += async (sender, e) =>
{
    await Task.Delay(50); // Defer to next frame
    // Load data
};
```

### Issue: High Memory Usage

**Causes:**
- Not disposing event handlers
- Image caching without limits
- Large datasets loaded at once

**Solutions:**
```csharp
// 1. Unsubscribe from events
protected override void OnDisappearing()
{
    listView.ItemTapped -= OnItemTapped;
    listView.ItemAppearing -= OnItemAppearing;
    base.OnDisappearing();
}

// 2. Limit image cache
CachedImage.MaxCacheSize = 50; // FFImageLoading

// 3. Implement paging/virtualization
LoadOnlyVisibleItems();
```

### Issue: Items Not Updating

**Causes:**
- Not implementing INotifyPropertyChanged
- Using List<T> instead of ObservableCollection<T>
- Forgetting RefreshView() after manual changes

**Solutions:**
```csharp
// 1. Use ObservableCollection
public ObservableCollection<Product> Products { get; set; }

// 2. Implement INPC on Model
public class Product : INotifyPropertyChanged
{
    private string name;
    public string Name
    {
        get => name;
        set { name = value; OnPropertyChanged(); }
    }
}

// 3. Call RefreshView after changes
product.Name = "Updated";
listView.RefreshItem(index);
```

### Issue: ItemTapped Event Not Firing

**Causes:**
- SelectionMode set to None
- Event handler not subscribed
- Child control consuming gestures

**Solutions:**
```csharp
// 1. Set SelectionMode
listView.SelectionMode = SelectionMode.Single;

// 2. Subscribe to event
listView.ItemTapped += OnItemTapped;

// 3. Use TapGestureRecognizer on specific elements
<Button Text="Details" Clicked="OnDetailsClicked" />
```

### Performance Checklist

- [ ] Use IList<T> or ObservableCollection<T> for ItemsSource
- [ ] Set fixed ItemSize (avoid AutoFit if possible)
- [ ] Keep ItemTemplate view hierarchy shallow (<4 levels)
- [ ] Implement INotifyPropertyChanged only on changing properties
- [ ] Use BeginInit/EndInit for bulk operations
- [ ] Avoid ScrollView parent
- [ ] Use cached image loading (FFImageLoading, CachedImage)
- [ ] Defer expensive operations in ItemAppearing
- [ ] Implement paging for large datasets
- [ ] Unsubscribe from events in OnDisappearing
- [ ] Test with realistic data volumes (1000+ items)
- [ ] Profile with platform-specific tools (Xcode Instruments, Android Profiler)

## Best Practices

1. **Always unsubscribe from events** in OnDisappearing or when disposing
2. **Use ItemAppearing for infinite scroll** instead of ScrollStateChanged
3. **Prefer SelectionChanged over ItemTapped** for selection-based actions
4. **Use Commands for MVVM** instead of code-behind event handlers
5. **Test performance with 1000+ items** to identify bottlenecks
6. **Profile before optimizing** - measure, don't guess
7. **Set ItemSize when possible** - avoid dynamic sizing overhead
8. **Implement virtualization** for large datasets (paging, load more)

## Related Documentation

- [Data Operations](data-operations.md) - Sorting, filtering, grouping
- [Selection](selection.md) - Selection modes and behaviors
- [Load More and Pull-to-Refresh](load-more-pull-refresh.md) - Infinite scroll patterns
- [MVVM Patterns](mvvm-patterns.md) - Commands and data binding
