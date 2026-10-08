# Row Operations

## Table of Contents
- [Row Height](#row-height)
- [Row Drag and Drop](#row-drag-and-drop)
- [Row Swiping](#row-swiping)
- [Adding and Deleting Rows](#adding-and-deleting-rows)
- [Unbound Rows](#unbound-rows)

## Row Height

### Default Row Height

```xaml
<syncfusion:SfDataGrid RowHeight="50" />
```

```csharp
dataGrid.RowHeight = 50;
```

### Default Header Row Height

```xaml
<syncfusion:SfDataGrid HeaderRowHeight="50" />
```

```csharp
dataGrid.HeaderRowHeight = 50;
```

### Auto Row Height

Use `QueryRowHeight` event for dynamic row heights:

```csharp
dataGrid.QueryRowHeight += DataGrid_QueryRowHeight;

private void DataGrid_QueryRowHeight(object sender, DataGridQueryRowHeightEventArgs e)
{
    if (e.RowIndex > 0) // Skip header
    {
        // Set height based on content
        e.Height = 60;
        e.Handled = true;
    }
}
```

### Row Height Based on Data

```csharp
private void DataGrid_QueryRowHeight(object sender, DataGridQueryRowHeightEventArgs e)
{
    if (e.RowIndex > dataGrid.GetHeaderIndex())
    {
       e.Height = e.GetIntrinsicRowHeight(e.RowIndex);
       e.Handled = true;
    }
}
```

## Row Drag and Drop

### Enable Row Drag-Drop

```xaml
<syncfusion:SfDataGrid AllowDraggingRow="True" />
```

```csharp
dataGrid.AllowDraggingRow = true;
```

### Row Dragging Events

```csharp
dataGrid.QueryRowDragging += DataGrid_QueryRowDragging;

private void DataGrid_QueryRowDragging(object sender, DataGridQueryRowDraggingEventArgs e)
{
    // e.From - Source row index
    // e.To - Target row index  
    // e.DraggingAction - DragStarted, Dragging, DragEnded
    // e.Cancel - Set true to cancel
    // e.CanAutoScroll - sets true if auto scroll can happen when reach the top or bottom, else false
    // e.CurrentRowData - Data corresponding to the to the current position of the RowDragView
    // e.RowData - Data of the dragged row
    // e.Position - x and y coordinates of RowDragView
    
    // Cancel dragging specific rows
    if (e.From == 0 && e.DraggingAction == DataGridDragAction.DragStarted)
    {
        e.Cancel = true;
    }
}
```

### Multi-Row Dragging

Drag multiple selected rows simultaneously:

```xaml
<syncfusion:SfDataGrid AllowDraggingRow="True"
                       SelectionMode="Multiple"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.AllowDraggingRow = true;
dataGrid.SelectionMode = DataGridSelectionMode.Multiple;
```

**Multi-Row Dragging Features:**
- Select multiple rows
- Drag all selected rows together
- Drop at single target location
- All selected rows move to drop position

### Row Drag Animation

When the `AllowDraggingRowAnimation` property is enabled, `SfDataGrid` displays a drag animation that visually moves the selected row during the drag operation — the row follows the pointer as it is dragged. Unlike the default drag behavior (which uses a drag indicator), the row itself is animated and moved, making it easier to identify the row being reordered. The default value is `false`.

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding Orders}"
                       AllowDraggingRow="True"
                       AllowDraggingRowAnimation="True">
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.AllowDraggingRow = true;
dataGrid.AllowDraggingRowAnimation = true;
```

Use this for a smoother, more intuitive drag-and-drop experience. Combine with `RowDragDropTemplate` to customize the appearance of the animated row.

## Row Resizing

Interactively resize rows by dragging row header borders.

### Enable Row Resizing

```xaml
<syncfusion:SfDataGrid ShowRowHeader="True"
                       AllowResizingRows="True"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.ShowRowHeader = true;
dataGrid.AllowResizingRows = true;
```

**Features:**
- Long-press on row header bottom edge to resize
- Resized heights are cached during scrolling
- Set `ShowRowHeader="True"` to enable row resizing UI
- Requires a row header column

### Row Resize Modes

Control when row height is applied:

**OnMoved Mode** - Live height update while resizing:

```xaml
<syncfusion:SfDataGrid ShowRowHeader="True"
                       AllowResizingRows="True"
                       RowResizeMode="OnMoved"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.RowResizeMode = DataGridRowResizeMode.OnMoved;
```

**OnTouchUp Mode** - Apply height after release (default):

```xaml
<syncfusion:SfDataGrid ShowRowHeader="True"
                       AllowResizingRows="True"
                       RowResizeMode="OnTouchUp"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.RowResizeMode = DataGridRowResizeMode.OnTouchUp;
```

### Row Resizing Events

Handle the `RowResizing` event:

```csharp
dataGrid.RowResizing += DataGrid_RowResizing;

private void DataGrid_RowResizing(object sender, DataGridRowResizingEventArgs e)
{
    // e.RowIndex - Index of row being resized
    // e.RowData - Data object of the row
    // e.NewValue - Requested height
    // e.Cancel - Set true to cancel resize
    
    // Enforce minimum height
    if (e.NewValue < 40)
    {
        e.Cancel = true;
    }
}
```

## Row Swiping

### Enable Row Swiping

```xaml
<syncfusion:SfDataGrid AllowSwiping="True" />
```

### Swipe Templates

```xaml
<syncfusion:SfDataGrid AllowSwiping="True">
       <syncfusion:SfDataGrid.LeftSwipeTemplate>
                <DataTemplate>
                    <Grid BackgroundColor="#6750A4"
                            Padding="0" ColumnDefinitions="*, *">
                        <Label Grid.Column="0"
                                   Text="EDIT"
                                   HorizontalTextAlignment="End"
                                   TextColor="#FFFFFF"
                                   VerticalTextAlignment="Center"
                                   LineBreakMode="NoWrap"
                                   BackgroundColor="Transparent"/>
                        <Image Grid.Column="1"
                                   Source="edit.png"
                                   HeightRequest="18"
                                   WidthRequest="18"
                                   Margin="12,0,0,0"
                                   HorizontalOptions="Start"
                                   VerticalOptions="Center"/>
                    </Grid>
                </DataTemplate>
            </syncfusion:SfDataGrid.LeftSwipeTemplate>
</syncfusion:SfDataGrid>
```

### Swipe Events

```csharp
dataGrid.SwipeStarting += DataGrid_SwipeStarting;
dataGrid.SwipeEnded += DataGrid_SwipeEnded;

private void DataGrid_SwipeEnded(object sender, DataGridSwipeEndedEventArgs e)
{
    // e.RowIndex - Swiped row index
    // e.SwipeDirection - Left or Right
    
    var order = e.RowData as OrderInfo;
    
    if (e.SwipeDirection == DataGridSwipeDirection.Right)
    {
        // Approve action
        order.IsApproved = true;
    }
    else
    {
        // Delete action
        viewModel.Orders.Remove(order);
    }
}
```

## Adding and Deleting Rows

### Add Row Programmatically

```csharp
var newOrder = new OrderInfo
{
    OrderID = 1001,
    CustomerID = "NEWCUST",
    OrderDate = DateTime.Now
};

viewModel.Orders.Add(newOrder);
```

### Delete Row

```csharp
// Delete selected row
var selectedOrder = dataGrid.SelectedRow as OrderInfo;
if (selectedOrder != null)
{
    viewModel.Orders.Remove(selectedOrder);
}

// Delete by index
var orderToDelete = viewModel.Orders[5];
viewModel.Orders.Remove(orderToDelete);
```

### Add New Row UI

```csharp
dataGrid.SelectionMode = DataGridSelectionMode.Single;
dataGrid.NavigationMode = DataGridNavigationMode.Cell;
dataGrid.AllowEditing = true;
dataGrid.AddNewRowPosition = DataGridAddNewRowPosition.Top;
dataGrid.AddNewRowInitiating += DataGrid_AddNewRowInitiating;

private void DataGrid_AddNewRowInitiating(object sender, DataGridAddNewRowInitiatingEventArgs e)
{
    // Initialize new row with default values
    var newRow = e.Object as OrderInfo;
    newRow.OrderDate = DateTime.Now;
    newRow.Status = "Pending";
}
```

## Unbound Rows

Add rows not bound to data source:

```csharp
// Add summary row at top
dataGrid.UnboundRows.Add(new DataGridUnboundRow
{
    Position = DataGridUnboundRowPosition.Top
});

// Add summary row at bottom
dataGrid.UnboundRows.Add(new DataGridUnboundRow
{
    Position = DataGridUnboundRowPosition.Bottom
});
```

### Populate Unbound Row

```csharp
dataGrid.QueryUnboundRow += Datagrid_QueryUnboundRow;

private void DataGrid_QueryUnboundRow(object sender, DataGridUnboundRowEventArgs e)
{
    // e.CellType - Gets the cell type associated with the unbound row cell renderers for unbound row cell.
    // e.Column - Gets the column associated with the unbound row cell renderers for unbound row cell.
    // e.UnboundAction - Gets the value that specifies the action for triggered the event.
    // e.GridUnboundRow - Gets the associated DataGridUnboundRow of the cell that triggered the event.
    // e.Value - Gets or sets the value for the unbound row cell.
    
    if (e.RowColumnIndex.ColumnIndex == 0)
    {
        e.Value = (dataGrid.CurrentRow as OrderInfo).OrderID;
    }
    else if (e.RowColumnIndex.ColumnIndex == 1)
    {
        e.Value = (dataGrid.CurrentRow as OrderInfo).CustomerID;
    }
    else if (e.RowColumnIndex.ColumnIndex == 2)
    {
        e.Value = (dataGrid.CurrentRow as OrderInfo).ShipCountry;
    }
    e.Handled = true;
}
```

## Common Patterns

### Pattern 1: Swipe to Delete with Confirmation

```csharp
dataGrid.SwipeEnded += async (s, e) =>
{
    if (e.SwipeDirection == DataGridSwipeDirection.Left)
    {
        var order = e.RowData as OrderInfo;
        bool confirm = await DisplayAlert("Confirm", 
            $"Delete order {order.OrderID}?", "Yes", "No");
        
        if (confirm)
        {
            viewModel.Orders.Remove(order);
        }
    }
};
```

### Pattern 2: Alternating Row Heights

```csharp
dataGrid.QueryRowHeight += (s, e) =>
{
    if (e.RowIndex > dataGrid.GetHeaderIndex())
    {
        e.Height = (e.RowIndex % 2 == 0) ? 50 : 60;
        e.Handled = true;
    }
};
```

## Next Steps

- Read [editing.md](editing.md) for cell editing
- Read [selection.md](selection.md) for selection features
