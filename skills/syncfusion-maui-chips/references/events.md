# Events in .NET MAUI Chips

Events in the Syncfusion .NET MAUI Chips control allow you to respond to user interactions such as selection changes, chip clicks, and item removal.

## SelectionChanging Event

The `SelectionChanging` event is triggered **before** a chip is selected. You can cancel the selection by setting the `Cancel` property to `true` in the event arguments.

**Supported Types:** Choice, Filter

### Event Arguments

**AddedItem:** The chip being selected  
**RemovedItem:** The chip being deselected (if any)  
**Cancel:** Set to `true` to prevent the selection

### Basic Usage

```xaml
<chip:SfChipGroup ItemsSource="{Binding Categories}"
                  DisplayMemberPath="Name"
                  ChipType="Choice"
                  SelectionChanging="ChipGroup_SelectionChanging">
    <!-- Visual states -->
</chip:SfChipGroup>
```

```csharp
private void ChipGroup_SelectionChanging(object sender, SelectionChangingEventArgs e)
{
    // Access the item being selected
    var newSelection = e.AddedItem as Category;
    
    // Access the item being deselected
    var oldSelection = e.RemovedItem as Category;
    
    // Log the change
    System.Diagnostics.Debug.WriteLine($"Changing from {oldSelection?.Name} to {newSelection?.Name}");
}
```

### Canceling Selection

You can prevent selection based on business logic:

```csharp
private void ChipGroup_SelectionChanging(object sender, SelectionChangingEventArgs e)
{
    var category = e.AddedItem as Category;
    
    // Prevent selection if category is disabled
    if (category != null && !category.IsEnabled)
    {
        e.Cancel = true;
        DisplayAlert("Disabled", $"{category.Name} is currently disabled", "OK");
    }
}
```

### Validation Example

```csharp
private void ChipGroup_SelectionChanging(object sender, SelectionChangingEventArgs e)
{
    var size = e.AddedItem as Size;
    
    // Validate size availability
    if (size != null && size.StockQuantity == 0)
    {
        e.Cancel = true;
        DisplayAlert("Out of Stock", $"Size {size.Name} is out of stock", "OK");
        return;
    }
    
    // Confirm selection for premium items
    if (size != null && size.IsPremium)
    {
        var result = DisplayAlert("Confirm", 
            $"Size {size.Name} requires premium. Continue?", 
            "Yes", "No");
        
        if (!result.Result)
        {
            e.Cancel = true;
        }
    }
}
```

## SelectionChanged Event

The `SelectionChanged` event is triggered **after** a chip selection has been completed. This event cannot be canceled.

**Supported Types:** Choice, Filter

### Event Arguments

**AddedItem:** The newly selected chip  
**RemovedItem:** The previously selected/deselected chip

### Basic Usage

```xaml
<chip:SfChipGroup ItemsSource="{Binding Sizes}"
                  DisplayMemberPath="Name"
                  ChipType="Choice"
                  SelectionChanged="ChipGroup_SelectionChanged">
    <!-- Visual states -->
</chip:SfChipGroup>
```

```csharp
private void ChipGroup_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var selectedSize = e.AddedItem as Size;
    
    if (selectedSize != null)
    {
        ResultLabel.Text = $"Selected: {selectedSize.Name}";
    }
}
```

### Choice Type Example

```csharp
private void ChipGroup_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var selectedItem = e.AddedItem as Product;
    var deselectedItem = e.RemovedItem as Product;
    
    // Update UI based on selection
    if (selectedItem != null)
    {
        ProductNameLabel.Text = selectedItem.Name;
        ProductPriceLabel.Text = $"${selectedItem.Price:F2}";
        ProductImage.Source = selectedItem.ImageUrl;
    }
    
    // Log deselection
    if (deselectedItem != null)
    {
        System.Diagnostics.Debug.WriteLine($"Deselected: {deselectedItem.Name}");
    }
}
```

### Filter Type Example

For Filter type (multi-selection), track all selected items:

```csharp
private List<Category> selectedCategories = new List<Category>();

private void ChipGroup_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    // Add newly selected item
    if (e.AddedItem != null)
    {
        var category = e.AddedItem as Category;
        if (!selectedCategories.Contains(category))
        {
            selectedCategories.Add(category);
        }
    }
    
    // Remove deselected item
    if (e.RemovedItem != null)
    {
        var category = e.RemovedItem as Category;
        selectedCategories.Remove(category);
    }
    
    // Update filtered results
    ApplyFilters();
}

private void ApplyFilters()
{
    if (selectedCategories.Count == 0)
    {
        // Show all items
        FilteredItems = new ObservableCollection<Item>(AllItems);
    }
    else
    {
        // Filter by selected categories
        var filtered = AllItems.Where(item => 
            selectedCategories.Any(cat => cat.Id == item.CategoryId));
        FilteredItems = new ObservableCollection<Item>(filtered);
    }
}
```

### ViewModel Binding Pattern

```csharp
// ViewModel
public class ProductViewModel : INotifyPropertyChanged
{
    private Product selectedProduct;
    
    public ObservableCollection<Product> Products { get; set; }
    
    public Product SelectedProduct
    {
        get { return selectedProduct; }
        set 
        { 
            selectedProduct = value;
            OnPropertyChanged(nameof(SelectedProduct));
            OnProductSelected();
        }
    }
    
    private void OnProductSelected()
    {
        if (SelectedProduct != null)
        {
            // Trigger additional logic
            LoadProductDetails(SelectedProduct.Id);
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

```csharp
// Code-behind
private void ChipGroup_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var viewModel = this.BindingContext as ProductViewModel;
    viewModel.SelectedProduct = e.AddedItem as Product;
}
```

## ChipClicked Event

The `ChipClicked` event is triggered when any chip is clicked, regardless of chip type.

**Supported Types:** All (Input, Choice, Filter, Action)

### Basic Usage

```xaml
<chip:SfChipGroup ItemsSource="{Binding Items}"
                  DisplayMemberPath="Name"
                  ChipClicked="ChipGroup_ChipClicked">
</chip:SfChipGroup>
```

```csharp
private void ChipGroup_ChipClicked(object sender, EventArgs e)
{
    // Generic chip click handling
    System.Diagnostics.Debug.WriteLine("A chip was clicked");
}
```

### Use Cases

**Analytics Tracking:**
```csharp
private void ChipGroup_ChipClicked(object sender, EventArgs e)
{
    // Track user interactions
    AnalyticsService.TrackEvent("ChipClicked", new Dictionary<string, string>
    {
        { "Timestamp", DateTime.Now.ToString() },
        { "Screen", "CategorySelection" }
    });
}
```

**Audio Feedback:**
```csharp
private void ChipGroup_ChipClicked(object sender, EventArgs e)
{
    // Play click sound
    AudioService.PlaySound("chip_click.mp3");
}
```

## ItemRemoved Event

The `ItemRemoved` event is triggered when a chip is removed from the group (via close button).

**Supported Types:** Input only

### Event Arguments

**RemovedItem:** The item that was removed from the collection

### Basic Usage

```xaml
<chip:SfChipGroup ItemsSource="{Binding Tags}"
                  DisplayMemberPath="Name"
                  ChipType="Input"
                  ItemRemoved="ChipGroup_ItemRemoved">
</chip:SfChipGroup>
```

```csharp
private void ChipGroup_ItemRemoved(object sender, ItemRemovedEventArgs e)
{
    var removedTag = e.RemovedItem as Tag;
    
    if (removedTag != null)
    {
        System.Diagnostics.Debug.WriteLine($"Removed tag: {removedTag.Name}");
    }
}
```

### Confirming Removal

```csharp
private async void ChipGroup_ItemRemoved(object sender, ItemRemovedEventArgs e)
{
    var tag = e.RemovedItem as Tag;
    
    if (tag != null)
    {
        // Log removal
        await LogRemovalAsync(tag.Id, tag.Name);
        
        // Show confirmation
        await DisplayAlert("Removed", $"Tag '{tag.Name}' removed", "OK");
    }
}
```

### Syncing with ViewModel

```csharp
private void ChipGroup_ItemRemoved(object sender, ItemRemovedEventArgs e)
{
    var viewModel = this.BindingContext as ViewModel;
    var removedTag = e.RemovedItem as Tag;
    
    // The item is already removed from ItemsSource by the control
    // Perform additional cleanup if needed
    if (removedTag != null)
    {
        viewModel.OnTagRemoved(removedTag);
    }
}
```

## CloseButtonClicked Event

The `CloseButtonClicked` event is triggered when the close button on a chip is clicked. This event is available on both `SfChip` and `SfChipGroup`.

**Supported Types:** Input (for SfChipGroup), or any SfChip with ShowCloseButton=True

### SfChip Usage

```xaml
<chip:SfChip x:Name="chip"
             Text="Removable"
             ShowCloseButton="True"
             CloseButtonClicked="Chip_CloseButtonClicked" />
```

```csharp
private async void Chip_CloseButtonClicked(object sender, EventArgs e)
{
    var result = await DisplayAlert("Confirm", 
        "Remove this chip?", 
        "Yes", "No");
    
    if (result)
    {
        // Remove chip from parent layout
        var chip = sender as SfChip;
        var parent = chip?.Parent as Layout;
        parent?.Children.Remove(chip);
    }
}
```

### Custom Confirmation

```csharp
private async void Chip_CloseButtonClicked(object sender, EventArgs e)
{
    var chip = sender as SfChip;
    
    bool confirmed = await DisplayAlert(
        "Remove Tag",
        $"Are you sure you want to remove '{chip.Text}'?",
        "Remove",
        "Cancel");
    
    if (confirmed)
    {
        // Remove from parent
        if (chip.Parent is SfChipGroup chipGroup)
        {
            chipGroup.Items.Remove(chip);
        }
        else if (chip.Parent is Layout layout)
        {
            layout.Children.Remove(chip);
        }
    }
}
```

## Event Combination Patterns

### Multi-Event Handling

```csharp
// Track all chip interactions
private void ChipGroup_SelectionChanging(object sender, SelectionChangingEventArgs e)
{
    LogEvent("SelectionChanging", e.AddedItem, e.RemovedItem);
}

private void ChipGroup_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    LogEvent("SelectionChanged", e.AddedItem, e.RemovedItem);
    UpdateUI(e.AddedItem);
}

private void ChipGroup_ChipClicked(object sender, EventArgs e)
{
    LogEvent("ChipClicked", null, null);
}

private void LogEvent(string eventName, object added, object removed)
{
    System.Diagnostics.Debug.WriteLine(
        $"[{DateTime.Now:HH:mm:ss}] {eventName} - Added: {added}, Removed: {removed}");
}
```

### Complete Filter Example

```xaml
<chip:SfChipGroup ItemsSource="{Binding Filters}"
                  DisplayMemberPath="Name"
                  ChipType="Filter"
                  SelectionChanging="Filters_SelectionChanging"
                  SelectionChanged="Filters_SelectionChanged"
                  ChipClicked="Filters_ChipClicked">
    <!-- Visual states -->
</chip:SfChipGroup>

<Button Text="Clear All" Clicked="ClearFilters_Clicked" />
<Label x:Name="FilterCountLabel" Text="0 filters active" />
```

```csharp
private void Filters_SelectionChanging(object sender, SelectionChangingEventArgs e)
{
    var filter = e.AddedItem as Filter;
    
    // Validate filter availability
    if (filter != null && !filter.IsAvailable)
    {
        e.Cancel = true;
        DisplayAlert("Unavailable", 
            $"Filter '{filter.Name}' is not available for this data set", 
            "OK");
    }
}

private void Filters_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    UpdateFilterCount();
    ApplyFilters();
}

private void Filters_ChipClicked(object sender, EventArgs e)
{
    // Provide haptic feedback
    HapticFeedback.Perform(HapticFeedbackType.Click);
}

private void UpdateFilterCount()
{
    var chipGroup = FindByName<SfChipGroup>("ChipGroup");
    var viewModel = this.BindingContext as ViewModel;
    
    int count = viewModel.Filters.Count(f => f.IsSelected);
    FilterCountLabel.Text = $"{count} filter{(count != 1 ? "s" : "")} active";
}

private async void ClearFilters_Clicked(object sender, EventArgs e)
{
    var viewModel = this.BindingContext as ViewModel;
    
    foreach (var filter in viewModel.Filters)
    {
        filter.IsSelected = false;
    }
    
    UpdateFilterCount();
    await ApplyFilters();
}
```

## Common Scenarios

### Scenario 1: Track Selection History

```csharp
private Stack<Product> selectionHistory = new Stack<Product>();

private void ChipGroup_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (e.AddedItem != null)
    {
        var product = e.AddedItem as Product;
        selectionHistory.Push(product);
        
        // Limit history size
        if (selectionHistory.Count > 10)
        {
            var list = selectionHistory.ToList();
            list.RemoveAt(list.Count - 1);
            selectionHistory = new Stack<Product>(list.Reverse<Product>());
        }
    }
}

private void UndoSelection()
{
    if (selectionHistory.Count > 1)
    {
        selectionHistory.Pop(); // Remove current
        var previous = selectionHistory.Peek();
        
        // Reselect previous
        var chipGroup = FindByName<SfChipGroup>("ChipGroup");
        // Trigger selection programmatically
    }
}
```

### Scenario 2: Dependent Selections

```csharp
// Category selection affects subcategory availability
private void CategoryChips_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var selectedCategory = e.AddedItem as Category;
    
    if (selectedCategory != null)
    {
        // Update subcategories based on selected category
        var viewModel = this.BindingContext as ViewModel;
        viewModel.LoadSubcategories(selectedCategory.Id);
    }
}

private void SubcategoryChips_SelectionChanging(object sender, SelectionChangingEventArgs e)
{
    var subcategory = e.AddedItem as Subcategory;
    var viewModel = this.BindingContext as ViewModel;
    
    // Ensure subcategory belongs to selected category
    if (subcategory != null && 
        viewModel.SelectedCategory != null &&
        subcategory.CategoryId != viewModel.SelectedCategory.Id)
    {
        e.Cancel = true;
        DisplayAlert("Invalid Selection", 
            "Please select a subcategory from the selected category", 
            "OK");
    }
}
```

### Scenario 3: Auto-Save on Change

```csharp
private async void Preferences_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var preference = e.AddedItem as Preference;
    
    if (preference != null)
    {
        // Auto-save to preferences
        await SavePreferenceAsync(preference);
        
        // Show confirmation
        ToastService.Show($"Saved: {preference.Name}");
    }
}

private async Task SavePreferenceAsync(Preference preference)
{
    try
    {
        await PreferencesService.SaveAsync(preference);
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", 
            $"Failed to save preference: {ex.Message}", 
            "OK");
    }
}
```

## Event Type Support Summary

| Event | Input | Choice | Filter | Action |
|-------|-------|--------|--------|--------|
| SelectionChanging | ❌ | ✅ | ✅ | ❌ |
| SelectionChanged | ❌ | ✅ | ✅ | ❌ |
| ChipClicked | ✅ | ✅ | ✅ | ✅ |
| ItemRemoved | ✅ | ❌ | ❌ | ❌ |
| CloseButtonClicked | ✅ | ❌ | ❌ | ❌ |

**Note:** For Action type chips, use the `Command` property instead of selection events (see customization.md).
