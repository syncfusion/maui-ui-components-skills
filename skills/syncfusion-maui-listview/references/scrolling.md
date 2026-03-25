# Scrolling in .NET MAUI ListView

## Overview

SfListView provides programmatic scrolling control, scroll events, and options for customizing scroll behavior.

## Scroll Methods

### ScrollTo Method

Scroll to a specific offset position:

```csharp
// Scroll to 500 pixels from top
listView.ScrollTo(500);

// Scroll without animation
listView.ScrollTo(500, true); // disable animation: true
```

### ScrollToRowIndex

Scroll to a specific item by index:

```csharp
// Scroll to item at index 10
listView.ItemsLayout.ScrollToRowIndex(10);

// Scroll with position and animation
listView.ItemsLayout.ScrollToRowIndex(
    10, 
    ScrollToPosition.Start,  // Start, Center, End, MakeVisible
    true                     // animated
);
```

**ScrollToPosition Options:**
- `Start` - Item at top of viewport
- `Center` - Item centered in viewport
- `End` - Item at bottom of viewport
- `MakeVisible` - Scroll minimum distance to make item visible

### Scroll to Item

```csharp
// Find item index and scroll
var product = viewModel.Products.FirstOrDefault(p => p.Id == targetId);
if (product != null)
{
    var index = viewModel.Products.IndexOf(product);
    listView.ItemsLayout.ScrollToRowIndex(index, ScrollToPosition.Center, true);
}
```

### Scroll to Top

```csharp
private void ScrollToTop()
{
    listView.ScrollTo(0, true);
    // OR
    listView.ItemsLayout.ScrollToRowIndex(0, ScrollToPosition.Start, true);
}
```

### Scroll to Bottom

```csharp
private void ScrollToBottom()
{
    var lastIndex = viewModel.Items.Count - 1;
    listView.ItemsLayout.ScrollToRowIndex(lastIndex, ScrollToPosition.End, true);
}
```

## Scroll Properties

### ScrollBarVisibility

Control scrollbar appearance:

```xml
<syncfusion:SfListView ScrollBarVisibility="Always" />
```

```csharp
listView.ScrollBarVisibility = ScrollBarVisibility.Always;
```

**Options:**
- `Default` - Platform-specific behavior
- `Always` - Always visible
- `Never` - Hidden

### AllowSwiping with Scrolling

```xml
<syncfusion:SfListView AllowSwiping="True" 
                       ScrollBarVisibility="Always" />
```

Both swipe and scroll gestures work together.

## Programmatic Scrolling

### Scroll on Item Selection

```csharp
listView.SelectionChanged += (sender, e) =>
{
    if (e.AddedItems.Count > 0)
    {
        var selectedItem = e.AddedItems[0];
        var index = listView.DataSource.DisplayItems.IndexOf(selectedItem);
        
        // Scroll selected item to center
        listView.ItemsLayout.ScrollToRowIndex(index, ScrollToPosition.Center, true);
    }
};
```

### Scroll to Search Result

```csharp
private void OnSearchResultSelected(SearchResult result)
{
    var matchingItem = viewModel.Items.FirstOrDefault(i => i.Id == result.ItemId);
    
    if (matchingItem != null)
    {
        // Scroll to item
        var index = viewModel.Items.IndexOf(matchingItem);
        listView.ItemsLayout.ScrollToRowIndex(index, ScrollToPosition.Start, true);
        
        // Optionally select it
        listView.SelectedItem = matchingItem;
    }
}
```

### Auto-Scroll to New Items

```csharp
viewModel.Items.CollectionChanged += (sender, e) =>
{
    if (e.Action == NotifyCollectionChangedAction.Add)
    {
        // Scroll to bottom when new item added
        Device.BeginInvokeOnMainThread(() =>
        {
            var lastIndex = viewModel.Items.Count - 1;
            listView.ItemsLayout.ScrollToRowIndex(lastIndex, ScrollToPosition.End, true);
        });
    }
};
```

## ListView Without Virtualization

For scenarios where you need ListView inside a ScrollView (not recommended for large datasets).

### Setup

```xml
<ScrollView>
    <syncfusion:SfListView x:Name="listView" 
                           ItemsSource="{Binding Items}"
                           Loaded="OnListViewLoaded" />
</ScrollView>
```

```csharp
using Syncfusion.Maui.ListView.Helpers;

private void OnListViewLoaded(object sender, EventArgs e)
{
    var container = listView.GetVisualContainer();
    var extent = (double)container.GetType()
        .GetRuntimeProperties()
        .FirstOrDefault(x => x.Name == "TotalExtent")
        ?.GetValue(container);
    
    if (extent.HasValue)
    {
        listView.HeightRequest = extent.Value;
    }
}
```

### With AutoFitMode

```xml
<ScrollView>
    <syncfusion:SfListView x:Name="listView"
                           AutoFitMode="Height"
                           ItemsSource="{Binding Items}"
                           Loaded="OnListViewLoaded" />
</ScrollView>
```

```csharp
using Syncfusion.Maui.ListView.Helpers;

private VisualContainer visualContainer;
private bool loaded = false;

private void OnListViewLoaded(object sender, EventArgs e)
{
    visualContainer = listView.GetVisualContainer();
    visualContainer.PropertyChanged += OnVisualContainerPropertyChanged;
    
    var extent = (double)visualContainer.GetType()
        .GetRuntimeProperties()
        .FirstOrDefault(x => x.Name == "TotalExtent")
        ?.GetValue(visualContainer);
    
    if (extent.HasValue)
    {
        listView.HeightRequest = extent.Value;
        loaded = true;
    }
}

private void OnVisualContainerPropertyChanged(object sender, PropertyChangedEventArgs e)
{
    if (e.PropertyName == "Height" && loaded && 
        listView.HeightRequest != visualContainer.Height)
    {
        listView.HeightRequest = visualContainer.Height;
    }
}
```

**Limitations:**
- ItemAppearing/ItemDisappearing events won't fire
- Keyboard navigation scrolling disabled
- Scrolling handled by parent ScrollView

## Performance Optimization

### Use Fixed ItemSize

```xml
<syncfusion:SfListView ItemSize="80" />
```

Fixed item size enables optimal scrolling performance.

### Avoid ScrollView Parent

```xml
<!-- ❌ AVOID -->
<ScrollView>
    <syncfusion:SfListView ItemsSource="{Binding Items}" />
</ScrollView>

<!-- ✓ CORRECT -->
<syncfusion:SfListView ItemsSource="{Binding Items}" />
```

ListView has built-in scrolling. Nested scroll views cause performance issues.

### Simplify Item Templates

```xml
<!-- ✓ GOOD: Simple, flat structure -->
<Grid Padding="10">
    <Label Text="{Binding Name}" />
</Grid>

<!-- ❌ AVOID: Deep nesting -->
<Grid>
    <StackLayout>
        <Frame>
            <StackLayout>
                <Grid>
                    <!-- Too deep -->
                </Grid>
            </StackLayout>
        </Frame>
    </StackLayout>
</Grid>
```

### Cache Item Size

```csharp
private Dictionary<object, double> itemSizeCache = new Dictionary<object, double>();

listView.QueryItemSize += (sender, e) =>
{
    if (itemSizeCache.ContainsKey(e.DataItem))
    {
        e.ItemSize = itemSizeCache[e.DataItem];
    }
    else
    {
        // Calculate size
        var item = e.DataItem as CustomItem;
        double size = CalculateItemSize(item);
        itemSizeCache[e.DataItem] = size;
        e.ItemSize = size;
    }
    
    e.Handled = true;
};
```

## Troubleshooting

**Issue:** ScrollToRowIndex not working
→ Ensure ListView has completed layout, try calling after Loaded event

**Issue:** Scroll position resets unexpectedly
→ Check if ItemsSource is being recreated instead of modified

**Issue:** Slow scrolling performance
→ Use fixed ItemSize, simplify templates, optimize images

**Issue:** Can't scroll programmatically in ScrollView
→ Remove ScrollView parent, ListView has built-in scrolling

**Issue:** ItemAppearing events not firing
→ Check if ListView is inside ScrollView (not supported in that scenario)
