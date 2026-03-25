# Data Operations in .NET MAUI ListView

## Table of Contents
- [Overview](#overview)
- [Sorting](#sorting)
- [Filtering](#filtering)
- [Grouping](#grouping)
- [Combining Operations](#combining-operations)
- [Performance Considerations](#performance-considerations)

## Overview

SfListView provides powerful data manipulation capabilities without requiring data source re-binding:

- **Sorting** - Order items by property values (ascending/descending, custom comparers)
- **Filtering** - Show subset of items based on predicates
- **Grouping** - Organize items into categories with headers

All operations work with the `DataSource` property and update the view efficiently.

## Sorting

Sort items using `SortDescriptor` objects added to `DataSource.SortDescriptors` collection.

### Basic Sorting

```csharp
using Syncfusion.Maui.DataSource;

// Sort by BookName in ascending order
listView.DataSource.SortDescriptors.Add(new SortDescriptor
{
    PropertyName = "BookName",
    Direction = ListSortDirection.Ascending
});
```

```xml
<syncfusion:SfListView x:Name="listView" ItemsSource="{Binding Books}">
    <syncfusion:SfListView.DataSource>
        <data:DataSource>
            <data:DataSource.SortDescriptors>
                <data:SortDescriptor PropertyName="BookName" Direction="Ascending" />
            </data:DataSource.SortDescriptors>
        </data:DataSource>
    </syncfusion:SfListView.DataSource>
</syncfusion:SfListView>
```

### SortDescriptor Properties

| Property | Type | Description |
|----------|------|-------------|
| `PropertyName` | string | Name of the property to sort by |
| `Direction` | ListSortDirection | Ascending or Descending |
| `Comparer` | IComparer | Custom comparison logic (optional) |

### Descending Sort

```csharp
listView.DataSource.SortDescriptors.Add(new SortDescriptor
{
    PropertyName = "Price",
    Direction = ListSortDirection.Descending
});
```

### Multiple Sort Descriptors

Sort by multiple properties with priority order:

```csharp
// Primary sort: Category (ascending)
listView.DataSource.SortDescriptors.Add(new SortDescriptor
{
    PropertyName = "Category",
    Direction = ListSortDirection.Ascending
});

// Secondary sort: Price (descending)
listView.DataSource.SortDescriptors.Add(new SortDescriptor
{
    PropertyName = "Price",
    Direction = ListSortDirection.Descending
});
```

### Custom Comparer

Create custom sorting logic with `IComparer`:

```csharp
public class CustomSortComparer : IComparer<object>
{
    public int Compare(object x, object y)
    {
        var item1 = x as BookInfo;
        var item2 = y as BookInfo;
        
        if (item1 == null || item2 == null)
            return 0;
        
        // Custom logic: Sort by length of BookName
        return item1.BookName.Length.CompareTo(item2.BookName.Length);
    }
}

// Apply custom comparer
listView.DataSource.SortDescriptors.Add(new SortDescriptor
{
    PropertyName = "BookName",
    Comparer = new CustomSortComparer()
});
```

### Dynamic Sorting (User Selection)

```xml
<StackLayout>
    <Picker x:Name="sortPicker" 
            Title="Sort By"
            SelectedIndexChanged="OnSortChanged">
        <Picker.Items>
            <x:String>Name (A-Z)</x:String>
            <x:String>Name (Z-A)</x:String>
            <x:String>Price (Low-High)</x:String>
            <x:String>Price (High-Low)</x:String>
        </Picker.Items>
    </Picker>
    
    <syncfusion:SfListView x:Name="listView" ItemsSource="{Binding Products}" />
</StackLayout>
```

```csharp
private void OnSortChanged(object sender, EventArgs e)
{
    listView.DataSource.SortDescriptors.Clear();
    
    switch (sortPicker.SelectedIndex)
    {
        case 0: // Name A-Z
            listView.DataSource.SortDescriptors.Add(new SortDescriptor
            {
                PropertyName = "Name",
                Direction = ListSortDirection.Ascending
            });
            break;
        case 1: // Name Z-A
            listView.DataSource.SortDescriptors.Add(new SortDescriptor
            {
                PropertyName = "Name",
                Direction = ListSortDirection.Descending
            });
            break;
        case 2: // Price Low-High
            listView.DataSource.SortDescriptors.Add(new SortDescriptor
            {
                PropertyName = "Price",
                Direction = ListSortDirection.Ascending
            });
            break;
        case 3: // Price High-Low
            listView.DataSource.SortDescriptors.Add(new SortDescriptor
            {
                PropertyName = "Price",
                Direction = ListSortDirection.Descending
            });
            break;
    }
}
```

## Filtering

Filter items by setting a predicate function to `DataSource.Filter` property.

### Basic Filtering

```csharp
// Show only items where Price > 20
listView.DataSource.Filter = (obj) =>
{
    var product = obj as ProductInfo;
    return product != null && product.Price > 20;
};

listView.DataSource.RefreshFilter();
```

**Important:** Call `RefreshFilter()` after setting/changing the filter.

### Search Bar Filter

```xml
<StackLayout>
    <SearchBar x:Name="searchBar"
               Placeholder="Search products..."
               TextChanged="OnSearchTextChanged" />
    
    <syncfusion:SfListView x:Name="listView" ItemsSource="{Binding Products}" />
</StackLayout>
```

```csharp
private void OnSearchTextChanged(object sender, TextChangedEventArgs e)
{
    if (listView.DataSource != null)
    {
        listView.DataSource.Filter = FilterProducts;
        listView.DataSource.RefreshFilter();
    }
}

private bool FilterProducts(object obj)
{
    var searchText = searchBar.Text;
    
    // Show all if search is empty
    if (string.IsNullOrWhiteSpace(searchText))
        return true;
    
    var product = obj as ProductInfo;
    if (product == null)
        return false;
    
    // Case-insensitive search in Name or Description
    return product.Name.Contains(searchText, StringComparison.OrdinalIgnoreCase) ||
           product.Description.Contains(searchText, StringComparison.OrdinalIgnoreCase);
}
```

### Multi-Criteria Filter

```csharp
private string categoryFilter = "All";
private decimal minPrice = 0;
private decimal maxPrice = 1000;

private bool FilterProducts(object obj)
{
    var product = obj as ProductInfo;
    if (product == null)
        return false;
    
    // Check category
    if (categoryFilter != "All" && product.Category != categoryFilter)
        return false;
    
    // Check price range
    if (product.Price < minPrice || product.Price > maxPrice)
        return false;
    
    return true;
}

// Update filter when criteria changes
private void OnCategoryChanged(object sender, EventArgs e)
{
    categoryFilter = categoryPicker.SelectedItem?.ToString() ?? "All";
    listView.DataSource.RefreshFilter();
}

private void OnPriceRangeChanged(object sender, EventArgs e)
{
    minPrice = minSlider.Value;
    maxPrice = maxSlider.Value;
    listView.DataSource.RefreshFilter();
}
```

### Remove Filter

```csharp
// Remove filter to show all items
listView.DataSource.Filter = null;
listView.DataSource.RefreshFilter();
```

### Filter Based on Another ListView Selection

```csharp
private void OnCategorySelected(object sender, ItemTappedEventArgs e)
{
    var selectedCategory = e.DataItem as CategoryInfo;
    
    // Filter products by selected category
    productsListView.DataSource.Filter = (obj) =>
    {
        var product = obj as ProductInfo;
        return product != null && product.CategoryId == selectedCategory.Id;
    };
    
    productsListView.DataSource.RefreshFilter();
}
```

## Grouping

Group items using `GroupDescriptor` objects added to `DataSource.GroupDescriptors` collection.

### Basic Grouping

```csharp
listView.DataSource.GroupDescriptors.Add(new GroupDescriptor
{
    PropertyName = "Category"
});
```

```xml
<syncfusion:SfListView x:Name="listView" ItemsSource="{Binding Products}">
    <syncfusion:SfListView.DataSource>
        <data:DataSource>
            <data:DataSource.GroupDescriptors>
                <data:GroupDescriptor PropertyName="Category" />
            </data:DataSource.GroupDescriptors>
        </data:DataSource>
    </syncfusion:SfListView.DataSource>
</syncfusion:SfListView>
```

### GroupDescriptor Properties

| Property | Type | Description |
|----------|------|-------------|
| `PropertyName` | string | Property to group by |
| `KeySelector` | Func<object, object> | Custom key selection logic |
| `Comparer` | IComparer | Custom group ordering |

### Sticky Group Headers

```xml
<syncfusion:SfListView x:Name="listView"
                       IsStickyGroupHeader="True"
                       ItemsSource="{Binding Contacts}">
    <syncfusion:SfListView.DataSource>
        <data:DataSource>
            <data:DataSource.GroupDescriptors>
                <data:GroupDescriptor PropertyName="FirstLetter" />
            </data:DataSource.GroupDescriptors>
        </data:DataSource>
    </syncfusion:SfListView.DataSource>
</syncfusion:SfListView>
```

### Custom Group Key Selector

```csharp
listView.DataSource.GroupDescriptors.Add(new GroupDescriptor
{
    PropertyName = "Price",
    KeySelector = (obj) =>
    {
        var product = obj as ProductInfo;
        if (product == null)
            return "Unknown";
        
        // Group by price range
        if (product.Price < 20)
            return "Budget ($0-$20)";
        else if (product.Price < 50)
            return "Mid-Range ($20-$50)";
        else
            return "Premium ($50+)";
    }
});
```

### Custom Group Header Template

```xml
<syncfusion:SfListView.GroupHeaderTemplate>
    <DataTemplate>
        <Grid BackgroundColor="#E0E0E0" Padding="10,5">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="Auto" />
            </Grid.ColumnDefinitions>
            
            <Label Text="{Binding Key}" 
                   FontAttributes="Bold" 
                   FontSize="16" 
                   TextColor="Black"
                   VerticalOptions="Center" />
            
            <Label Grid.Column="1" 
                   Text="{Binding Count, StringFormat='{0} items'}" 
                   FontSize="12" 
                   TextColor="Gray"
                   VerticalOptions="Center" />
        </Grid>
    </DataTemplate>
</syncfusion:SfListView.GroupHeaderTemplate>
```

### Multiple Level Grouping

```csharp
// Group by Category, then by Price Range
listView.DataSource.GroupDescriptors.Add(new GroupDescriptor
{
    PropertyName = "Category"
});

listView.DataSource.GroupDescriptors.Add(new GroupDescriptor
{
    PropertyName = "Price",
    KeySelector = (obj) =>
    {
        var product = obj as ProductInfo;
        return product.Price < 50 ? "Under $50" : "$50 and above";
    }
});
```

### Expand/Collapse Groups Programmatically

```csharp
// Collapse all groups
listView.CollapseAll();

// Expand all groups
listView.ExpandAll();

// Collapse specific group
var group = listView.DataSource.Groups[0];
listView.CollapseGroup(group);

// Expand specific group
listView.ExpandGroup(group);
```

### Group Expand/Collapse Events

```csharp
listView.GroupExpanding += (sender, e) =>
{
    // Cancel expansion for specific groups
    if (e.Groups[0].Key.ToString() == "Hidden Category")
        e.Cancel = true;
};

listView.GroupExpanded += (sender, e) =>
{
    // Handle after group expanded
    Debug.WriteLine($"Group {e.Groups[0].Key} expanded");
};
```

## Combining Operations

You can use sorting, filtering, and grouping together.

### Example: Filtered, Sorted, and Grouped List

```csharp
// 1. Filter: Show only in-stock products
listView.DataSource.Filter = (obj) =>
{
    var product = obj as ProductInfo;
    return product != null && product.InStock;
};

// 2. Sort: By price ascending
listView.DataSource.SortDescriptors.Add(new SortDescriptor
{
    PropertyName = "Price",
    Direction = ListSortDirection.Ascending
});

// 3. Group: By category
listView.DataSource.GroupDescriptors.Add(new GroupDescriptor
{
    PropertyName = "Category"
});

// Apply changes
listView.DataSource.RefreshFilter();
```

### Order of Operations

1. **Filter** is applied first (reduces data set)
2. **Sort** is applied to filtered results
3. **Group** is applied to sorted, filtered results

This order ensures optimal performance.

## Performance Considerations

### Optimize Filter Performance

```csharp
// BAD: Complex operations in filter
listView.DataSource.Filter = (obj) =>
{
    var product = obj as ProductInfo;
    // Avoid expensive operations
    var details = await GetProductDetailsAsync(product.Id); // ❌ Async call
    return details.IsAvailable;
};

// GOOD: Use pre-calculated or cached values
listView.DataSource.Filter = (obj) =>
{
    var product = obj as ProductInfo;
    return product.IsAvailable; // ✓ Simple property check
};
```

### Batch Updates with BeginInit/EndInit

```csharp
// Prevent multiple refreshes during bulk updates
listView.DataSource.BeginInit();

listView.DataSource.SortDescriptors.Clear();
listView.DataSource.SortDescriptors.Add(new SortDescriptor { PropertyName = "Name" });

listView.DataSource.GroupDescriptors.Clear();
listView.DataSource.GroupDescriptors.Add(new GroupDescriptor { PropertyName = "Category" });

listView.DataSource.Filter = FilterFunction;

listView.DataSource.EndInit(); // Single refresh here
```

### Use LiveDataUpdateMode

```csharp
// Update UI automatically when data changes
listView.DataSource.LiveDataUpdateMode = LiveDataUpdateMode.AllowDataShaping;
```

Options:
- `Default` - Updates when notified
- `AllowDataShaping` - Automatically applies sort/filter/group

### Avoid Frequent RefreshFilter Calls

```csharp
// BAD: Refresh on every keystroke
searchBar.TextChanged += (s, e) =>
{
    listView.DataSource.RefreshFilter(); // ❌ Too frequent
};

// GOOD: Debounce or wait for user to finish typing
private CancellationTokenSource searchCts;

searchBar.TextChanged += async (s, e) =>
{
    searchCts?.Cancel();
    searchCts = new CancellationTokenSource();
    
    try
    {
        await Task.Delay(300, searchCts.Token); // Wait 300ms
        listView.DataSource.RefreshFilter(); // ✓ Debounced
    }
    catch (TaskCanceledException) { }
};
```

## Complete Example: Product List with All Operations

```xml
<StackLayout>
    <SearchBar x:Name="searchBar" 
               Placeholder="Search products..."
               TextChanged="OnSearchChanged" />
    
    <Picker x:Name="categoryPicker"
            Title="Category"
            SelectedIndexChanged="OnCategoryChanged" />
    
    <Picker x:Name="sortPicker"
            Title="Sort By"
            SelectedIndexChanged="OnSortChanged" />
    
    <syncfusion:SfListView x:Name="listView"
                           ItemsSource="{Binding Products}"
                           IsStickyGroupHeader="True"
                           ItemSize="80">
        <syncfusion:SfListView.GroupHeaderTemplate>
            <DataTemplate>
                <Label Text="{Binding Key}" 
                       BackgroundColor="LightGray"
                       Padding="10"
                       FontAttributes="Bold" />
            </DataTemplate>
        </syncfusion:SfListView.GroupHeaderTemplate>
    </syncfusion:SfListView>
</StackLayout>
```

```csharp
public partial class ProductListPage : ContentPage
{
    public ProductListPage()
    {
        InitializeComponent();
        InitializeDataOperations();
    }
    
    private void InitializeDataOperations()
    {
        // Default grouping by category
        listView.DataSource.GroupDescriptors.Add(new GroupDescriptor
        {
            PropertyName = "Category"
        });
    }
    
    private void OnSearchChanged(object sender, TextChangedEventArgs e)
    {
        listView.DataSource.Filter = FilterProducts;
        listView.DataSource.RefreshFilter();
    }
    
    private bool FilterProducts(object obj)
    {
        var product = obj as ProductInfo;
        if (product == null) return false;
        
        var searchText = searchBar.Text;
        if (string.IsNullOrWhiteSpace(searchText))
            return true;
        
        return product.Name.Contains(searchText, StringComparison.OrdinalIgnoreCase);
    }
    
    private void OnSortChanged(object sender, EventArgs e)
    {
        listView.DataSource.SortDescriptors.Clear();
        
        if (sortPicker.SelectedIndex == 0)
        {
            listView.DataSource.SortDescriptors.Add(new SortDescriptor
            {
                PropertyName = "Name",
                Direction = ListSortDirection.Ascending
            });
        }
        else if (sortPicker.SelectedIndex == 1)
        {
            listView.DataSource.SortDescriptors.Add(new SortDescriptor
            {
                PropertyName = "Price",
                Direction = ListSortDirection.Ascending
            });
        }
    }
}
```
