# Selection and Interaction in .NET MAUI TreeMap

Enable user interaction by allowing selection of leaf items. Users can select one or multiple items, and the TreeMap highlights selected items and raises events for further handling.

## Overview

Selection allows users to interact with leaf items by tapping or clicking them. The TreeMap supports single and multiple selection modes with visual feedback.

**Key Features:**
- Single or multiple selection modes
- Visual highlighting of selected items
- SelectionChanged event for handling selection
- Programmatic selection control
- SelectedItems collection

**Basic Setup:**

```xml
<treemap:SfTreeMap DataSource="{Binding Data}"
                   PrimaryValuePath="Size"
                   SelectionMode="Single">
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Name" />
    </treemap:SfTreeMap.LeafItemSettings>
</treemap:SfTreeMap>
```

## Selection Modes

### SelectionMode Property

Controls how users can select leaf items.

**TreeMapSelectionMode Enum:**
- `None`: Selection disabled (default)
- `Single`: Select one item at a time
- `Multiple`: Select multiple items simultaneously

### None (Default)

No selection interaction available. Items cannot be selected.

```xml
<treemap:SfTreeMap SelectionMode="None" />
```

```csharp
treeMap.SelectionMode = TreeMapSelectionMode.None;
```

**Use when:**
- Pure visualization without interaction
- Read-only displays
- Selection not needed

### Single Selection

Users can select one item at a time. Selecting a new item deselects the previous one.

**XAML:**
```xml
<treemap:SfTreeMap DataSource="{Binding Products}"
                   PrimaryValuePath="Sales"
                   SelectionMode="Single">
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="ProductName" />
    </treemap:SfTreeMap.LeafItemSettings>
</treemap:SfTreeMap>
```

**C# Implementation:**
```csharp
treeMap.DataSource = products;
treeMap.PrimaryValuePath = "Sales";
treeMap.SelectionMode = TreeMapSelectionMode.Single;
treeMap.LeafItemSettings = new TreeMapLeafItemSettings { LabelPath = "ProductName" };
```

**Behavior:**
1. User taps/clicks an item → Item becomes selected
2. User taps/clicks a different item → Previous item deselects, new item selects
3. User taps/clicks the selected item → Item deselects

**Use when:**
- Highlighting one item for detailed view
- Master-detail pattern (selection shows detail elsewhere)
- Comparing one item at a time

### Multiple Selection

Users can select multiple items simultaneously.

**XAML:**
```xml
<treemap:SfTreeMap DataSource="{Binding Data}"
                   PrimaryValuePath="Value"
                   SelectionMode="Multiple">
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Name" />
    </treemap:SfTreeMap.LeafItemSettings>
</treemap:SfTreeMap>
```

**C# Implementation:**
```csharp
treeMap.SelectionMode = TreeMapSelectionMode.Multiple;
```

**Behavior:**
1. User taps/clicks an item → Item becomes selected (added to selection)
2. User taps/clicks another item → Both items selected
3. User taps/clicks a selected item → Item deselects (removed from selection)
4. No limit on number of selected items

**Use when:**
- Batch operations (delete, export, process multiple items)
- Comparing multiple items
- Filtering or categorizing groups
- Statistical analysis of selected subset

## SelectedItems Collection

The `SelectedItems` property holds all currently selected leaf items as a collection of data objects.

**Type:** `IList<object>`

### Reading Selected Items

**C#:**
```csharp
treeMap.SelectionMode = TreeMapSelectionMode.Multiple;

// Later, after user selections
var selectedItems = treeMap.SelectedItems;
foreach (var item in selectedItems)
{
    if (item is Product product)
    {
        Debug.WriteLine($"Selected: {product.Name} - ${product.Sales}");
    }
}
```

### Programmatic Selection

Set selected items programmatically by modifying the `SelectedItems` collection.

**Select Single Item:**
```csharp
public class Product
{
    public string Name { get; set; }
    public decimal Sales { get; set; }
}

// Select specific product
var productToSelect = products.FirstOrDefault(p => p.Name == "Laptop");
if (productToSelect != null)
{
    treeMap.SelectedItems.Clear();
    treeMap.SelectedItems.Add(productToSelect);
}
```

**Select Multiple Items:**
```csharp
// Select products with sales over $100,000
var highSalesProducts = products.Where(p => p.Sales > 100000);
foreach (var product in highSalesProducts)
{
    treeMap.SelectedItems.Add(product);
}
```

**Clear Selection:**
```csharp
treeMap.SelectedItems.Clear();
```

### Binding SelectedItems

You can bind `SelectedItems` to a ViewModel property for MVVM scenarios.

**ViewModel:**
```csharp
public class DashboardViewModel : INotifyPropertyChanged
{
    private ObservableCollection<Product> _selectedProducts = new ObservableCollection<Product>();
    
    public ObservableCollection<Product> SelectedProducts
    {
        get => _selectedProducts;
        set
        {
            _selectedProducts = value;
            OnPropertyChanged();
        }
    }
    
    public ObservableCollection<Product> Products { get; set; }
    
    // INotifyPropertyChanged implementation
    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**XAML:**
```xml
<treemap:SfTreeMap DataSource="{Binding Products}"
                   PrimaryValuePath="Sales"
                   SelectionMode="Multiple"
                   SelectedItems="{Binding SelectedProducts}">
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Name" />
    </treemap:SfTreeMap.LeafItemSettings>
</treemap:SfTreeMap>

<!-- Display selected count elsewhere -->
<Label Text="{Binding SelectedProducts.Count, StringFormat='Selected: {0} items'}" />
```

## SelectionChanged Event

Raised whenever the selection changes (items added or removed).

### Event Arguments

**TreeMapSelectionChangedEventArgs Properties:**
- `OldItems`: Previously selected items (before change)
- `NewItems`: Newly selected items (after change)

### Subscribing to SelectionChanged

**XAML:**
```xml
<treemap:SfTreeMap DataSource="{Binding Data}"
                   PrimaryValuePath="Value"
                   SelectionMode="Single"
                   SelectionChanged="OnTreeMapSelectionChanged">
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Name" />
    </treemap:SfTreeMap.LeafItemSettings>
</treemap:SfTreeMap>
```

**Code-Behind:**
```csharp
private void OnTreeMapSelectionChanged(object sender, TreeMapSelectionChangedEventArgs e)
{
    // Items that were selected before this change
    foreach (var oldItem in e.OldItems)
    {
        Debug.WriteLine($"Deselected: {oldItem}");
    }
    
    // Items that are now selected
    foreach (var newItem in e.NewItems)
    {
        if (newItem is Product product)
        {
            Debug.WriteLine($"Selected: {product.Name}");
        }
    }
}
```

### C# Event Subscription

```csharp
treeMap.SelectionChanged += (sender, e) =>
{
    var selectedCount = treeMap.SelectedItems.Count;
    Debug.WriteLine($"Selection changed. Total selected: {selectedCount}");
    
    foreach (var item in e.NewItems)
    {
        if (item is Product product)
        {
            // Handle newly selected product
            ShowProductDetails(product);
        }
    }
};
```

### Event Usage Scenarios

**Update Detail View:**
```csharp
private void OnTreeMapSelectionChanged(object sender, TreeMapSelectionChangedEventArgs e)
{
    if (e.NewItems.Count > 0 && e.NewItems[0] is Product selectedProduct)
    {
        // Update detail panel with selected product info
        ProductDetailView.BindingContext = selectedProduct;
        ProductDetailView.IsVisible = true;
    }
    else
    {
        // No selection, hide detail panel
        ProductDetailView.IsVisible = false;
    }
}
```

**Calculate Selected Total:**
```csharp
private void OnTreeMapSelectionChanged(object sender, TreeMapSelectionChangedEventArgs e)
{
    decimal totalSales = 0;
    foreach (var item in treeMap.SelectedItems)
    {
        if (item is Product product)
        {
            totalSales += product.Sales;
        }
    }
    
    TotalLabel.Text = $"Selected Total: ${totalSales:N2}";
}
```

**Enable/Disable Actions:**
```csharp
private void OnTreeMapSelectionChanged(object sender, TreeMapSelectionChangedEventArgs e)
{
    var hasSelection = treeMap.SelectedItems.Count > 0;
    
    DeleteButton.IsEnabled = hasSelection;
    ExportButton.IsEnabled = hasSelection;
    CompareButton.IsEnabled = treeMap.SelectedItems.Count >= 2;
}
```

## Selection Highlighting

Selected items are automatically highlighted with a visual indicator (border, overlay, or opacity change).

### Default Highlight Behavior

When a leaf item is selected:
- Visual indicator appears (e.g., border, overlay)
- Automatic contrast adjustment for visibility

**No additional configuration needed** for default highlighting.

### Customizing Highlight Appearance

While the TreeMap provides default highlighting, you can customize appearance through:

1. **Brush Settings**: Use contrasting colors that show selection clearly
2. **Borders**: Ensure `Stroke` and `StrokeWidth` provide visible boundaries
3. **Spacing**: Adequate spacing helps distinguish selected items

**Example - Clear Visual Boundaries:**
```xml
<treemap:SfTreeMap DataSource="{Binding Data}"
                   PrimaryValuePath="Value"
                   SelectionMode="Single">
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Name"
                                         Spacing="5"
                                         Stroke="DarkGray"
                                         StrokeWidth="2" />
    </treemap:SfTreeMap.LeafItemSettings>
</treemap:SfTreeMap>
```

The increased spacing and border width make selection highlighting more prominent.

## Practical Examples

### Example 1: Single Selection with Detail View

**XAML:**
```xml
<Grid RowDefinitions="*, Auto">
    <treemap:SfTreeMap Grid.Row="0"
                       DataSource="{Binding Products}"
                       PrimaryValuePath="Sales"
                       SelectionMode="Single"
                       SelectionChanged="OnProductSelected">
        <treemap:SfTreeMap.LeafItemSettings>
            <treemap:TreeMapLeafItemSettings LabelPath="Name" />
        </treemap:SfTreeMap.LeafItemSettings>
    </treemap:SfTreeMap>
    
    <Frame Grid.Row="1" 
           x:Name="DetailPanel"
           IsVisible="False"
           BackgroundColor="LightGray"
           Padding="15"
           Margin="10">
        <VerticalStackLayout Spacing="8">
            <Label x:Name="DetailName" 
                   FontSize="18" 
                   FontAttributes="Bold" />
            <Label x:Name="DetailSales" 
                   FontSize="15" />
            <Label x:Name="DetailCategory" 
                   FontSize="15" />
        </VerticalStackLayout>
    </Frame>
</Grid>
```

**Code-Behind:**
```csharp
private void OnProductSelected(object sender, TreeMapSelectionChangedEventArgs e)
{
    if (e.NewItems.Count > 0 && e.NewItems[0] is Product product)
    {
        DetailPanel.IsVisible = true;
        DetailName.Text = product.Name;
        DetailSales.Text = $"Sales: ${product.Sales:N2}";
        DetailCategory.Text = $"Category: {product.Category}";
    }
    else
    {
        DetailPanel.IsVisible = false;
    }
}
```

### Example 2: Multiple Selection with Batch Actions

**XAML:**
```xml
<Grid RowDefinitions="Auto,*,Auto">
    <Label Grid.Row="0" 
           x:Name="SelectionCountLabel"
           Text="No items selected"
           Padding="10"
           FontSize="14"
           FontAttributes="Bold" />
    
    <treemap:SfTreeMap Grid.Row="1"
                       x:Name="ProductTreeMap"
                       DataSource="{Binding Products}"
                       PrimaryValuePath="Sales"
                       SelectionMode="Multiple"
                       SelectionChanged="OnSelectionChanged">
        <treemap:SfTreeMap.LeafItemSettings>
            <treemap:TreeMapLeafItemSettings LabelPath="Name" />
        </treemap:SfTreeMap.LeafItemSettings>
    </treemap:SfTreeMap>
    
    <HorizontalStackLayout Grid.Row="2" 
                           Spacing="10" 
                           Padding="10">
        <Button x:Name="DeleteButton"
                Text="Delete Selected"
                IsEnabled="False"
                Clicked="OnDeleteClicked" />
        <Button x:Name="ExportButton"
                Text="Export Selected"
                IsEnabled="False"
                Clicked="OnExportClicked" />
        <Button Text="Clear Selection"
                Clicked="OnClearClicked" />
    </HorizontalStackLayout>
</Grid>
```

**Code-Behind:**
```csharp
private void OnSelectionChanged(object sender, TreeMapSelectionChangedEventArgs e)
{
    var count = ProductTreeMap.SelectedItems.Count;
    
    SelectionCountLabel.Text = count == 0 
        ? "No items selected" 
        : $"{count} item(s) selected";
    
    DeleteButton.IsEnabled = count > 0;
    ExportButton.IsEnabled = count > 0;
}

private void OnDeleteClicked(object sender, EventArgs e)
{
    var itemsToDelete = ProductTreeMap.SelectedItems.Cast<Product>().ToList();
    
    // Confirm deletion
    var confirm = DisplayAlert("Confirm Delete", 
        $"Delete {itemsToDelete.Count} items?", 
        "Yes", "No");
    
    if (confirm.Result)
    {
        foreach (var product in itemsToDelete)
        {
            Products.Remove(product);
        }
        ProductTreeMap.SelectedItems.Clear();
    }
}

private void OnExportClicked(object sender, EventArgs e)
{
    var selectedProducts = ProductTreeMap.SelectedItems.Cast<Product>().ToList();
    ExportToFile(selectedProducts);
}

private void OnClearClicked(object sender, EventArgs e)
{
    ProductTreeMap.SelectedItems.Clear();
}
```

### Example 3: Programmatic Selection on Load

```csharp
public partial class DashboardPage : ContentPage
{
    public DashboardPage()
    {
        InitializeComponent();
        
        // Load data
        ProductTreeMap.DataSource = GetProducts();
        ProductTreeMap.PrimaryValuePath = "Sales";
        ProductTreeMap.SelectionMode = TreeMapSelectionMode.Multiple;
        
        // Auto-select top 3 products by sales
        AutoSelectTopProducts();
    }
    
    private void AutoSelectTopProducts()
    {
        var products = (ProductTreeMap.DataSource as IEnumerable<Product>);
        if (products == null) return;
        
        var topThree = products.OrderByDescending(p => p.Sales).Take(3);
        
        ProductTreeMap.SelectedItems.Clear();
        foreach (var product in topThree)
        {
            ProductTreeMap.SelectedItems.Add(product);
        }
    }
}
```

### Example 4: Filter Selection by Criteria

**XAML:**
```xml
<Grid RowDefinitions="Auto,*">
    <HorizontalStackLayout Grid.Row="0" Spacing="10" Padding="10">
        <Button Text="Select High Sales (>$50K)"
                Clicked="OnSelectHighSales" />
        <Button Text="Select Low Sales (<$10K)"
                Clicked="OnSelectLowSales" />
        <Button Text="Clear"
                Clicked="OnClearSelection" />
    </HorizontalStackLayout>
    
    <treemap:SfTreeMap Grid.Row="1"
                       x:Name="SalesTreeMap"
                       DataSource="{Binding Products}"
                       PrimaryValuePath="Sales"
                       SelectionMode="Multiple">
        <treemap:SfTreeMap.LeafItemSettings>
            <treemap:TreeMapLeafItemSettings LabelPath="Name" />
        </treemap:SfTreeMap.LeafItemSettings>
    </treemap:SfTreeMap>
</Grid>
```

**Code-Behind:**
```csharp
private void OnSelectHighSales(object sender, EventArgs e)
{
    var products = SalesTreeMap.DataSource as IEnumerable<Product>;
    if (products == null) return;
    
    var highSales = products.Where(p => p.Sales > 50000);
    
    SalesTreeMap.SelectedItems.Clear();
    foreach (var product in highSales)
    {
        SalesTreeMap.SelectedItems.Add(product);
    }
}

private void OnSelectLowSales(object sender, EventArgs e)
{
    var products = SalesTreeMap.DataSource as IEnumerable<Product>;
    if (products == null) return;
    
    var lowSales = products.Where(p => p.Sales < 10000);
    
    SalesTreeMap.SelectedItems.Clear();
    foreach (var product in lowSales)
    {
        SalesTreeMap.SelectedItems.Add(product);
    }
}

private void OnClearSelection(object sender, EventArgs e)
{
    SalesTreeMap.SelectedItems.Clear();
}
```

## Troubleshooting

### Issue 1: Selection Not Working

**Symptoms:** Tapping items doesn't select them

**Solutions:**
1. Verify `SelectionMode` is not `None`
2. Ensure TreeMap is not disabled
3. Check that items are leaf items (not group headers)
4. Confirm touch/mouse input is reaching the TreeMap

```xml
<!-- Ensure selection is enabled -->
<treemap:SfTreeMap SelectionMode="Single" />
```

### Issue 2: SelectedItems Always Empty

**Symptoms:** `SelectedItems.Count` is always 0

**Solutions:**
1. Check that `SelectionMode` is `Single` or `Multiple` (not `None`)
2. Verify you're checking SelectedItems after user interaction or programmatic selection
3. Ensure data source is properly bound
4. Check for timing issues (accessing before selection occurs)

```csharp
// Subscribe to SelectionChanged to confirm selection is working
treeMap.SelectionChanged += (s, e) =>
{
    Debug.WriteLine($"Selected count: {treeMap.SelectedItems.Count}");
};
```

### Issue 3: SelectionChanged Not Firing

**Symptoms:** Event handler never called

**Solutions:**
1. Verify event is properly subscribed (XAML or C#)
2. Check for exceptions in event handler (catch and log)
3. Ensure `SelectionMode` is not `None`
4. Confirm event handler signature matches delegate

```csharp
// Correct signature
private void OnSelectionChanged(object sender, TreeMapSelectionChangedEventArgs e)
{
    // Handler code
}
```

### Issue 4: Programmatic Selection Not Visible

**Symptoms:** Added items to SelectedItems but no visual feedback

**Solutions:**
1. Ensure items added to SelectedItems exist in DataSource
2. Verify object references match (same instance)
3. Check that TreeMap has rendered before setting selection
4. Use `Device.BeginInvokeOnMainThread` if setting from background thread

```csharp
// Ensure item is from the data source
var dataSource = treeMap.DataSource as List<Product>;
var itemToSelect = dataSource.FirstOrDefault(p => p.Id == targetId);

if (itemToSelect != null)
{
    treeMap.SelectedItems.Clear();
    treeMap.SelectedItems.Add(itemToSelect);  // Use exact object from DataSource
}
```

### Issue 5: Multiple Selection Behaving Like Single

**Symptoms:** Only one item selects at a time despite `SelectionMode="Multiple"`

**Solutions:**
1. Double-check `SelectionMode` property is actually set to `Multiple`
2. Verify no code is clearing selection on each interaction
3. Check for duplicate TreeMap instances

```csharp
// Verify selection mode
Debug.WriteLine($"Selection Mode: {treeMap.SelectionMode}");
// Should print: Selection Mode: Multiple
```

### Issue 6: Cannot Deselect Items

**Symptoms:** Selected items can't be unselected

**Solutions:**
1. Tap/click the selected item again (toggles selection)
2. Use `SelectedItems.Clear()` programmatically
3. Set `SelectedItems.Remove(item)` for specific item
4. Verify `SelectionMode` allows deselection

```csharp
// Deselect specific item
if (treeMap.SelectedItems.Contains(itemToDeselect))
{
    treeMap.SelectedItems.Remove(itemToDeselect);
}

// Deselect all
treeMap.SelectedItems.Clear();
```

## Related Topics

- [Tooltip Configuration](tooltip.md) - Adding interactive tooltips
- [Accessibility and Events](accessibility-events.md) - Additional event handling
- [Getting Started](getting-started.md) - Basic TreeMap setup

## Summary

- Enable selection with `SelectionMode`: None, Single, or Multiple
- Single mode: Select one item at a time (previous deselects)
- Multiple mode: Select multiple items simultaneously
- Access selected items via `SelectedItems` collection
- Programmatically select items by adding to `SelectedItems`
- Handle `SelectionChanged` event for selection updates
- Event provides `OldItems` (deselected) and `NewItems` (selected)
- Selected items automatically highlighted visually
- Use selection for detail views, batch operations, filtering, and comparisons
- Clear selection with `SelectedItems.Clear()`
- Bind `SelectedItems` to ViewModel for MVVM scenarios
- Ensure items added to SelectedItems exist in DataSource (same object reference)
