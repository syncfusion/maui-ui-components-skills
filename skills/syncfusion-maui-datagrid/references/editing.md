# Editing

## Table of Contents
- [Enable Editing](#enable-editing)
- [Column-Level Editing](#column-level-editing)
- [Edit Modes](#edit-modes)
- [Editing Events](#editing-events)
- [Programmatic Editing](#programmatic-editing)
- [Common Editing Patterns](#Common-Editing-Patterns)
- [Troubleshooting](#troubleshooting)

## Enable Editing

To enable cell editing in the DataGrid, three properties must be configured:

```xml
<syncfusion:SfDataGrid AllowEditing="True"
                       NavigationMode="Cell"
                       SelectionMode="Single"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.AllowEditing = true;
dataGrid.NavigationMode = NavigationMode.Cell;
dataGrid.SelectionMode = SelectionMode.Single; // Or Multiple
```

**Required Settings:**
1. `AllowEditing="True"` - Enables editing functionality
2. `NavigationMode="Cell"` - Must be Cell (not Row)
3. `SelectionMode` - Must NOT be None (use Single, Multiple, or SingleDeselect)

## Column-Level Editing

Control editing for specific columns:

```xml
<syncfusion:SfDataGrid AllowEditing="True">
    <syncfusion:SfDataGrid.Columns>
        <!-- Read-only column -->
        <syncfusion:DataGridNumericColumn MappingName="OrderID"
                                          AllowEditing="False" />
        
        <!-- Editable column -->
        <syncfusion:DataGridTextColumn MappingName="CustomerID"
                                       AllowEditing="True" />
    </syncfusion:SfDataGrid.Columns>
</syncfusion:SfDataGrid>
```

**Note:** `DataGridColumn.AllowEditing` takes priority over `SfDataGrid.AllowEditing`.

## Edit Modes

### Edit Tap Action

Control how users enter edit mode:

```xml
<syncfusion:SfDataGrid EditTapAction="OnTap" />  <!-- Single tap -->
<syncfusion:SfDataGrid EditTapAction="OnDoubleTap" />  <!-- Default: Double tap -->
```

```csharp
// Enter edit mode on single tap
dataGrid.EditTapAction = TapAction.OnTap;

// Enter edit mode on double tap (default)
dataGrid.EditTapAction = TapAction.OnDoubleTap;
```

### Lost Focus Behavior

Control what happens when grid loses focus:

```xml
<syncfusion:SfDataGrid LostFocusBehavior="EndEditCurrentCell" />
```

```csharp
// Default: DiscardChanges (editing canceled)
dataGrid.LostFocusBehavior = DataGridLostFocusBehavior.Default;

// Commit changes when grid loses focus
dataGrid.LostFocusBehavior = DataGridLostFocusBehavior.EndEditCurrentCell;
```

**Options:**
- `Default` (Default) - Cancel editing, don't save changes
- `EndEditCurrentCell` - Commit changes when focus moves away

**Note:** Applies only to `DataGridNumericColumn` and `DataGridTextColumn`.

## Editing Events

### CurrentCellBeginEdit

Triggered when cell is about to enter edit mode:

```csharp
dataGrid.CurrentCellBeginEdit += DataGrid_CurrentCellBeginEdit;

private void DataGrid_CurrentCellBeginEdit(object sender, DataGridCurrentCellBeginEditEventArgs e)
{
    // e.Column - Column being edited
    // e.RowColumnIndex - Cell position
    // e.Cancel - Set true to prevent editing
    // Cancel editing for specific conditions
   // Editing prevented for the cell at RowColumnIndex(2,2).
    if (e.RowColumnIndex == new Syncfusion.Maui.GridCommon.ScrollAxis.RowColumnIndex(2, 2)) 
        e.Cancel = true;
}
```

### CellValueChanged

Triggered when cell value changes during editing:

```csharp
dataGrid.CellValueChanged += DataGrid_CellValueChanged;

private void DataGrid_CellValueChanged(object sender, DataGridCellValueChangedEventArgs e)
{
    // e.Column - Column being edited
    // e.RowColumnIndex - Cell position  
    // e.NewValue - New value entered
    // e.RowData - Data object
    
    // Perform calculations or validation
    if (e.Column.MappingName == "Quantity")
    {
        var order = e.RowData as OrderInfo;
        if (order != null)
        {
            order.Total = order.UnitPrice * (int)e.NewValue;
        }
    }
}
```

### CurrentCellEndEdit

Triggered when cell editing ends:

```csharp
dataGrid.CurrentCellEndEdit += DataGrid_CurrentCellEndEdit;

private void DataGrid_CurrentCellEndEdit(object sender, DataGridCurrentCellEndEditEventArgs e)
{
    // e.Cancel - Set true to cancel commit
    // e.NewValue - New value entered
    // e.OldValue - Old value before editing
    // e.RowColumnIndex - Cell position
    
    // Perform final validation using RowColumnIndex to get row data
    var rowIndex = e.RowColumnIndex.RowIndex;
    if (rowIndex >= 0 && rowIndex < dataGrid.ItemsSource.Cast<object>().Count())
    {
        var order = dataGrid.ItemsSource.Cast<OrderInfo>().ElementAt(rowIndex);
        if (order != null && e.NewValue is int newQuantity && newQuantity <= 0)
        {
            DisplayAlert("Error", "Quantity must be greater than 0", "OK");
            e.Cancel = true;
        }
    }
}
```

## Programmatic Editing

### Begin Edit

Enter edit mode programmatically:

```csharp
// Edit specific cell
dataGrid.BeginEdit(rowIndex: 2, columnIndex: 1);
```

### End Edit

Commit current cell changes:

```csharp
dataGrid.EndEdit();
```

### Cancel Edit

Cancel current cell editing:

```csharp
dataGrid.CancelEdit();
```

## Common Editing Patterns

### Pattern 1: Conditional Editing

```csharp
dataGrid.CurrentCellBeginEdit += (s, e) =>
{
    // Get row data from ItemsSource using RowColumnIndex
    var rowIndex = e.RowColumnIndex.RowIndex;
    if (rowIndex >= 0)
    {
        var order = dataGrid.ItemsSource.Cast<OrderInfo>().ElementAt(rowIndex);
        
        // Don't allow editing shipped orders
        if (order?.Status == "Shipped")
        {
            e.Cancel = true;
            DisplayAlert("Info", "Cannot edit shipped orders", "OK");
        }
    }
    
    // Don't allow editing OrderID
    if (e.Column.MappingName == "OrderID")
    {
        e.Cancel = true;
    }
};
```

### Pattern 2: Cascading Updates

```csharp
dataGrid.CellValueChanged += (s, e) =>
{
    var order = e.RowData as OrderInfo;
    
    if (e.Column.MappingName == "Quantity" || e.Column.MappingName == "UnitPrice")
    {
        // Auto-calculate total
        order.Total = order.Quantity * order.UnitPrice;
    }
    
    if (e.Column.MappingName == "Discount")
    {
        // Apply discount
        order.FinalPrice = order.Total * (1 - order.Discount / 100);
    }
};
```

### Pattern 3: Validation Before Commit

```csharp
dataGrid.CurrentCellEndEdit += (s, e) =>
{
    // Validate the new value before commit
    if (e.NewValue is int newQuantity && newQuantity <= 0)
    {
        DisplayAlert("Error", "Quantity must be positive", "OK");
        e.Cancel = true;
        return;
    }
    
    // Access row data through ItemsSource using RowColumnIndex
    var rowIndex = e.RowColumnIndex.RowIndex;
    if (rowIndex >= 0)
    {
        var order = dataGrid.ItemsSource.Cast<OrderInfo>().ElementAt(rowIndex);
        if (order != null && newQuantity > order.StockLevel)
        {
            DisplayAlert("Warning", "Quantity exceeds stock level", "OK");
            e.Cancel = true;
        }
    }
};
```

### Pattern 4: Auto-Complete During Edit

```csharp
dataGrid.CellValueChanged += (s, e) =>
{
    if (e.Column.MappingName == "CustomerID")
    {
        var customerId = e.NewValue?.ToString();
        if (!string.IsNullOrEmpty(customerId))
        {
            // Auto-fill customer details
            var customer = GetCustomerById(customerId);
            if (customer != null)
            {
                var order = e.RowData as OrderInfo;
                order.CustomerName = customer.Name;
                order.ShipAddress = customer.Address;
            }
        }
    }
};
```

## Troubleshooting

### Editing Not Working

**Problem:** Cannot edit cells despite AllowEditing="True".

**Solutions:**
1. Check `NavigationMode="Cell"` (not Row)
2. Verify `SelectionMode` is not None
3. Check column's `AllowEditing` property (shouldn't be False)
4. For CheckBox columns, set `AllowEditing="True"` explicitly

### Keyboard Not Appearing

**Problem:** Keyboard doesn't show when editing.

**Solutions:**
1. Ensure you're on a physical device or emulator with soft keyboard
2. Check `EditTapAction` settings
3. Try single-tapping vs double-tapping based on `EditTapAction`

### Changes Not Saving

**Problem:** Edits don't persist in data source.

**Solutions:**
1. Ensure data model implements `INotifyPropertyChanged`
2. Check property setters are raising `PropertyChanged` event
3. For collections, use `ObservableCollection<T>`
4. Verify `EndEdit()` is called (not canceled)

### Can't Edit Specific Column

**Problem:** One column won't accept edits.

** Solutions:**
1. Check `DataGridColumn.AllowEditing` for that column
2. Verify column MappingName matches a writable property
3. Check `CurrentCellBeginEdit` event isn't canceling
4. For template columns, built-in editing isn't supported

### Edits Lost When Scrolling

**Problem:** Changes disappear after scrolling.

**Solutions:**
1. Implement `INotifyPropertyChanged` properly
2. Use `ObservableCollection<T>` for ItemsSource
3. Call `EndEdit()` before scrolling programmatically
4. Check `LostFocusBehavior` setting

### Invalid Values Accepted

**Problem:** Grid accepts wrong data types or invalid values.

**Solutions:**
1. Implement validation in `CurrentCellEndEdit`
2. Use `e.Cancel = true` to reject invalid input
3. Implement `IDataError Info` or `INotifyDataErrorInfo`
4. Add validation in property setters

## Next Steps

- Read [data-validation.md](data-validation.md) for validation techniques
- Read [selection.md](selection.md) for selection behaviors
- Read [sorting-filtering.md](sorting-filtering.md) for data operations
