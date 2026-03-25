# Selection

## Table of Contents
- [Selection Modes](#selection-modes)
- [Selection Events](#selection-events)
- [Programmatic Selection](#programmatic-selection)
- [Get Selected Items](#get-selected-items)

## Selection Modes

```xml
<syncfusion:SfDataGrid SelectionMode="Single" />  <!-- Default -->
<syncfusion:SfDataGrid SelectionMode="Multiple" />
<syncfusion:SfDataGrid SelectionMode="SingleDeselect" />
<syncfusion:SfDataGrid SelectionMode="Extended" />
<syncfusion:SfDataGrid SelectionMode="None" />
```

**Modes:**
- `Single` - Select one row (default)
- `Multiple` - Select multiple rows
- `SingleDeselect` - Select one, tap again to deselect
- `Extended` - select multiple rows or cells by dragging the mouse or by using the key modifiers Ctrl and Shift.
- `None` - No selection allowed

## Navigation Mode

```xml
<syncfusion:SfDataGrid NavigationMode="Cell" />
<syncfusion:SfDataGrid NavigationMode="Row" />
<syncfusion:SfDataGrid NavigationMode="Any" />
```

**NavigationMode:**
- `Cell` - Individual cell navigation (required for editing)
- `Row` - Entire row navigation
- `Any` - Allows selection of both rows and cells. Tapping on the row header selects the entire row. Tapping on a cell selects only that particular cell.

## Selection Events

### SelectionChanging

Triggered before selection changes:

```csharp
dataGrid.SelectionChanging += DataGrid_SelectionChanging;

private void DataGrid_SelectionChanging(object sender, DataGridSelectionChangingEventArgs e)
{
    // e.AddedRows - Rows being added to selection
    // e.RemovedRows - Rows being removed from selection
    // e.Cancel - Set true to prevent selection change
    
    // Prevent selecting specific rows
    foreach (var item in e.AddedRows)
    {
        var order = item as OrderInfo;
        if (order.IsLocked)
        {
            e.Cancel = true;
            return;
        }
    }
}
```

### SelectionChanged

Triggered after selection changes:

```csharp
dataGrid.SelectionChanged += DataGrid_SelectionChanged;

private void DataGrid_SelectionChanged(object sender, DataGridSelectionChangedEventArgs e)
{
    // e.AddedRows - Newly selected rows
    // e.RemovedRows - Deselected rows
    
    foreach (var item in e.AddedRows)
    {
        var order = item as OrderInfo;
        Debug.WriteLine($"Selected: {order.OrderID}");
    }
}
```

## Programmatic Selection

### Select Row

```csharp
// Select by index
dataGrid.SelectedIndex = 2;

// Select by item
var orderToSelect = viewModel.Orders[5];
dataGrid.SelectedRow = orderToSelect;
```

### Select Multiple Rows

```csharp
// Select multiple items
dataGrid.SelectedRows.Add(viewModel.Orders[0]);
dataGrid.SelectedRows.Add(viewModel.Orders[2]);
dataGrid.SelectedRows.Add(viewModel.Orders[4]);
```

### Select All

```csharp
dataGrid.SelectAll();
```

### Clear Selection

```csharp
dataGrid.ClearSelection();

// Or
dataGrid.SelectedRows.Clear();
```

## Get Selected Items

```csharp
// Get currently selected item
var selectedOrder = dataGrid.SelectedRow as OrderInfo;

// Get all selected items
var selectedOrders = dataGrid.SelectedRows.Cast<OrderInfo>().ToList();

// Get selected index
int selectedIndex = dataGrid.SelectedIndex;
```

## Keyboard Navigation (Windows)

On Windows platform, keyboard navigation is supported:
- Arrow keys - Navigate cells/rows
- Page Up/Down - Scroll page
- Ctrl+Home/End - Go to first/last cell

## Troubleshooting

### Selection Not Working

**Solutions:**
- Don't use `SelectionMode="None"`
- Check `SelectionChanging` event isn't canceling
- Verify data source is set

### Multiple Selection Not Working

**Solutions:**
- Set `SelectionMode="Multiple"`
- Use Ctrl/Cmd key when clicking (platform-dependent)

## Next Steps

- Read [editing.md](editing.md) for cell editing
- Read [sorting-filtering.md](sorting-filtering.md) for data operations
