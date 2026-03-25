# Column Operations

## Table of Contents
- [Column Resizing](#column-resizing)
- [Column Drag and Drop](#column-drag-and-drop)
- [Stacked Headers](#stacked-headers)
- [Unbound Columns](#unbound-columns)
- [Freeze Panes](#freeze-panes)

## Column Resizing

Enable users to resize columns by dragging the column header borders.

### Enable Column Resizing

```xml
<syncfusion:SfDataGrid AllowResizingColumns="True"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.AllowResizingColumns = true;
```

**Features:**
- Drag right border of column header to resize
- Resizing indicator shows during operation
- Respects `MinimumWidth` and `MaximumWidth` constraints

**Hide Column by Resizing:**
Set `MinimumWidth="0"` to allow hiding via resize:

```xml
<syncfusion:DataGridTextColumn MappingName="Notes"
                               MinimumWidth="0" />
```

### Resizing Events

Handle the `ColumnResizing` event:

```csharp
dataGrid.ColumnResizing += DataGrid_ColumnResizing;

private void DataGrid_ColumnResizing(object sender, DataGridColumnResizingEventArgs e)
{
    // e.Index - Column index being resized
    // e.NewValue - Current width during resize
    // e.ResizingState - Progress state (Started, Progressing, Completed)
    // e.Cancel - Set true to cancel resizing
}
```

**Cancel Resizing for Specific Column:**

```csharp
private void DataGrid_ColumnResizing(object sender, DataGridColumnResizingEventArgs e)
{
    // Don't allow resizing the first column
    if (e.Index == 0)
    {
        e.Cancel = true;
    }
}
```

**Limit Width During Resize:**

```csharp
private void DataGrid_ColumnResizing(object sender, DataGridColumnResizingEventArgs e)
{
    // Don't allow width greater than 200
    if (e.NewValue > 200)
    {
        e.Cancel = true;
    }
}
```

### Customize Resizing Indicator

Change the indicator color:

```xml
<syncfusion:SfDataGrid AllowResizingColumns="True">
    <syncfusion:SfDataGrid.DefaultStyle>
        <syncfusion:DataGridStyle ColumnResizingIndicatorColor="DeepPink" />
    </syncfusion:SfDataGrid.DefaultStyle>
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.DefaultStyle.ColumnResizingIndicatorColor = Colors.Blue;
```

## Column Drag and Drop

Reorder columns by dragging column headers.

### Enable Column Drag-Drop

```xml
<syncfusion:SfDataGrid AllowDraggingColumn="True"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.AllowDraggingColumn = true;
```

**Features:**
- Drag column header to new position
- Visual drag view shows during operation
- Drop indicators show target position

### Drag-Drop Events

Handle the `QueryColumnDragging` event:

```csharp
dataGrid.QueryColumnDragging += DataGrid_QueryColumnDragging;

private void DataGrid_QueryColumnDragging(object sender, DataGridQueryColumnDraggingEventArgs e)
{
    // e.From - Index of dragged column
    // e.To - Target drop index
    // e.DraggingAction - DragStarted, Dragging, DragEnded
    // e.DraggingPosition - Current drag position (Point)
    // e.CanAutoScroll - Enable/disable auto-scroll
    // e.Cancel - Set true to cancel operation
}
```

**Cancel Dragging Specific Column:**

```csharp
private void DataGrid_QueryColumnDragging(object sender, DataGridQueryColumnDraggingEventArgs e)
{
    // Don't allow dragging the OrderID column (index 0)
    if (e.From == 0 && e.DraggingAction == DataGridDragAction.DragStarted)
    {
        e.Cancel = true;
    }
}
```

**Cancel Dropping at Specific Position:**

```csharp
private void DataGrid_QueryColumnDragging(object sender, DataGridQueryColumnDraggingEventArgs e)
{
    // Don't allow dropping at index 3
    if (e.To == 3 && e.DraggingAction == DataGridDragAction.DragEnded)
    {
        e.Cancel = true;
    }
}
```

**Cancel Dragging Between Ranges:**

```csharp
private void DataGrid_QueryColumnDragging(object sender, DataGridQueryColumnDraggingEventArgs e)
{
    // Don't allow dropping in columns 3-5
    if (e.To >= 3 && e.To <= 5 && 
        (e.DraggingAction == DataGridDragAction.Dragging || 
         e.DraggingAction == DataGridDragAction.DragEnded))
    {
        e.Cancel = true;
    }
}
```

### Cancel Drag-Drop with Frozen Columns

**Prevent Dragging Frozen Columns:**

```csharp
dataGrid.FrozenColumnCount = 2;

private void DataGrid_QueryColumnDragging(object sender, DataGridQueryColumnDraggingEventArgs e)
{
    // Don't allow dragging frozen columns
    if (e.From < dataGrid.FrozenColumnCount && 
        e.DraggingAction == DataGridDragAction.DragStarted)
    {
        e.Cancel = true;
    }
}
```

**Prevent Dropping Frozen Columns to Non-Frozen Area:**

```csharp
private void DataGrid_QueryColumnDragging(object sender, DataGridQueryColumnDraggingEventArgs e)
{
    // Frozen column can't be dropped in non-frozen area
    if (e.From < dataGrid.FrozenColumnCount && 
        e.To >= dataGrid.FrozenColumnCount &&
        e.DraggingAction == DataGridDragAction.DragEnded)
    {
        e.Cancel = true;
    }
}
```

### Customize Drag View Appearance

```xml
<syncfusion:SfDataGrid AllowDraggingColumn="True">
    <syncfusion:SfDataGrid.DefaultStyle>
        <syncfusion:DataGridStyle ColumnDragViewTextColor="White"
                                  ColumnDragViewBackgroundColor="#FF0074E3"
                                  ColumnDraggingIndicatorLineColor="DeepPink" />
    </syncfusion:SfDataGrid.DefaultStyle>
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.DefaultStyle.ColumnDragViewTextColor = Colors.White;
dataGrid.DefaultStyle.ColumnDragViewBackgroundColor = Color.FromArgb("#FF0074E3");
dataGrid.DefaultStyle.ColumnDraggingIndicatorLineColor = Colors.DeepPink;
```

## Stacked Headers

Create multi-level column headers by grouping columns under stacked headers.

### Add Stacked Headers

```xml
<syncfusion:SfDataGrid ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.StackedHeaderRows>
        <!-- Level 1: Group all order columns -->
        <syncfusion:DataGridStackedHeaderRow>
            <syncfusion:DataGridStackedHeaderRow.Columns>
                <syncfusion:DataGridStackedColumn
                    ColumnMappingNames="OrderID,OrderDate,CustomerID,ShipCountry"
                    Text="Order Shipment Details"
                    MappingName="ShipmentDetails" />
            </syncfusion:DataGridStackedHeaderRow.Columns>
        </syncfusion:DataGridStackedHeaderRow>
        
        <!-- Level 2: Sub-groups -->
        <syncfusion:DataGridStackedHeaderRow>
            <syncfusion:DataGridStackedHeaderRow.Columns>
                <syncfusion:DataGridStackedColumn
                    ColumnMappingNames="OrderID,OrderDate"
                    Text="Order Details"
                    MappingName="OrderDetails" />
                <syncfusion:DataGridStackedColumn
                    ColumnMappingNames="CustomerID,ShipCountry"
                    Text="Customer Details"
                    MappingName="CustomerDetails" />
            </syncfusion:DataGridStackedHeaderRow.Columns>
        </syncfusion:DataGridStackedHeaderRow>
    </syncfusion:SfDataGrid.StackedHeaderRows>
</syncfusion:SfDataGrid>
```

**Code-Behind:**

```csharp
// First level - Group all
var stackedHeaderRow1 = new DataGridStackedHeaderRow();
stackedHeaderRow1.Columns.Add(new DataGridStackedColumn
{
    ColumnMappingNames = "OrderID,OrderDate,CustomerID,ShipCountry",
    Text = "Order Shipment Details",
    MappingName = "ShipmentDetails"
});
dataGrid.StackedHeaderRows.Add(stackedHeaderRow1);

// Second level - Sub-groups
var stackedHeaderRow2 = new DataGridStackedHeaderRow();
stackedHeaderRow2.Columns.Add(new DataGridStackedColumn
{
    ColumnMappingNames = "OrderID,OrderDate",
    Text = "Order Details",
    MappingName = "OrderDetails"
});
stackedHeaderRow2.Columns.Add(new DataGridStackedColumn
{
    ColumnMappingNames = "CustomerID,ShipCountry",
    Text = "Customer Details",
    MappingName = "CustomerDetails"
});
dataGrid.StackedHeaderRows.Add(stackedHeaderRow2);
```

**Key Properties:**
- `ColumnMappingNames` - Comma-separated column names to group
- `Text` - Text displayed in stacked header
- `MappingName` - Unique identifier for the stacked column

### Add/Remove Child Columns Dynamically

**Add Column:**

```csharp
var childColumns = dataGrid.StackedHeaderRows[0].Columns[0].ColumnMappingNames;
dataGrid.StackedHeaderRows[0].Columns[0].ColumnMappingNames = childColumns + ",Freight";
```

**Remove Column:**

```csharp
var columns = dataGrid.StackedHeaderRows[0].Columns[0].ColumnMappingNames.Split(',').ToList();
columns.Remove("OrderID");
dataGrid.StackedHeaderRows[0].Columns[0].ColumnMappingNames = string.Join(",", columns);
```

### Stacked Header Height

**Set Header Row Height:**

```xml
<syncfusion:SfDataGrid HeaderRowHeight="60" />
```

```csharp
dataGrid.HeaderRowHeight = 60;
```

**Dynamic Height Using QueryRowHeight:**

```csharp
using Syncfusion.Maui.DataGrid.Helper;

dataGrid.QueryRowHeight += DataGrid_QueryRowHeight;

private void DataGrid_QueryRowHeight(object sender, DataGridQueryRowHeightEventArgs e)
{
    // Set height for stacked header rows (rows before header index)
    if (e.RowIndex < dataGrid.GetHeaderIndex())
    {
        e.Height = 50;
        e.Handled = true;
    }
}
```

### Customize Stacked Header Appearance

```xml
<syncfusion:SfDataGrid.DefaultStyle>
    <syncfusion:DataGridStyle StackedHeaderRowBackground="#0074E3"
                              StackedHeaderRowTextColor="White"
                              StackedHeaderRowFontSize="16"
                              StackedHeaderRowFontFamily="Helvetica Neue"
                              StackedHeaderRowFontAttributes="Bold" />
</syncfusion:SfDataGrid.DefaultStyle>
```

```csharp
dataGrid.DefaultStyle.StackedHeaderRowBackground = Color.FromArgb("#0074E3");
dataGrid.DefaultStyle.StackedHeaderRowTextColor = Colors.White;
dataGrid.DefaultStyle.StackedHeaderRowFontSize = 16;
dataGrid.DefaultStyle.StackedHeaderRowFontFamily = "Helvetica Neue";
dataGrid.DefaultStyle.StackedHeaderRowFontAttributes = FontAttributes.Bold;
```

### Data Annotations for Stacked Headers

Group columns using `Display.GroupName`:

```csharp
public class OrderInfo
{
    [Display(GroupName = "Order Details")]
    public int OrderID { get; set; }

    [Display(GroupName = "Order Details")]
    public DateTime OrderDate { get; set; }

    [Display(GroupName = "Customer Details")]
    public string CustomerID { get; set; }

    [Display(GroupName = "Customer Details")]
    public string ShipCountry { get; set; }
}
```

## Unbound Columns

Add columns not bound to data source properties with custom values.

### Create Unbound Column

```xml
<syncfusion:SfDataGrid ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.Columns>
        <syncfusion:DataGridTextColumn MappingName="OrderID" />
        <syncfusion:DataGridTextColumn MappingName="CustomerID" />
        
        <!-- Unbound column -->
         <syncfusion:DataGridUnboundColumn 
                        MappingName="DiscountPrice"
                        HeaderText="SUM"
                        Expression="Price1+Price2"
                        Format="C" />
    </syncfusion:SfDataGrid.Columns>
</syncfusion:SfDataGrid>
```

**Populate Unbound Column:**

Use `QueryUnboundColumnValue` event:

```csharp
dataGrid.QueryUnboundColumnValue += DataGrid_QueryUnboundColumnValue;

private void DataGrid_QueryUnboundColumnValue(object? sender, DataGridUnboundColumnEventArgs e)
{
    if (e.UnboundAction == DataGridUnboundActions.QueryData)
    {
        var Price1 = Convert.ToInt16(e.Record.GetType().GetProperty("Price1").GetValue(e.Record));
        var Price2 = Convert.ToInt16(e.Record.GetType().GetProperty("Price2").GetValue(e.Record));
        e.Value = Price1 - Price2;
    }
}
```

**Unbound Column with Expression:**

```xml
<syncfusion:DataGridUnboundColumn MappingName="TotalPrice"
                               HeaderText="Total"
                               Expression="UnitPrice * Quantity" />
```

**Supported Operators:**
- Arithmetic: `+`, `-`, `*`, `/`
- Comparison: `<`, `>`, `<=`, `>=`, `==`, `!=`
- Logical: `AND`, `OR`

**Example Expressions:**
```csharp
Expression="UnitPrice * Quantity"
Expression="Freight + Tax"
Expression="Quantity > 10 ? UnitPrice * 0.9 : UnitPrice"
```

## Freeze Panes

Freeze columns to keep them visible while scrolling horizontally.

### Freeze Columns from Left

```xml
<syncfusion:SfDataGrid FrozenColumnCount="2"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.FrozenColumnCount = 2; // Freeze first 2 columns
```

**Features:**
- Frozen columns remain visible when scrolling horizontally
- Vertical line separator shows between frozen/non-frozen columns
- Commonly used for ID or important columns

### Freeze Rows from Top

```xml
<syncfusion:SfDataGrid FrozenRowCount="3"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.FrozenRowCount = 3; // Freeze first 3 rows
```

### Customize Freeze Pane Line

```xml
<syncfusion:SfDataGrid FrozenColumnCount="1">
    <syncfusion:SfDataGrid.DefaultStyle>
        <syncfusion:DataGridStyle FreezePaneLineColor="Red"
                                  FreezePaneLineStrokeThickness="3" />
    </syncfusion:SfDataGrid.DefaultStyle>
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.DefaultStyle.FreezePaneLineColor = Colors.Red;
dataGrid.DefaultStyle.FreezePaneLineStrokeThickness = 3;
```

### Footer Columns and Rows

Freeze columns/rows from the right/bottom:

```xml
<syncfusion:SfDataGrid FooterFrozenColumnCount="1"
                       FooterFrozenRowCount="2" />
```

```csharp
dataGrid.FooterFrozenColumnCount = 1; // Freeze last column from right
dataGrid.FooterFrozenRowCount = 2;  // Freeze last 2 rows from bottom
```

## Troubleshooting

### Column Resizing Not Working

**Problem:** Cannot resize columns.

**Solutions:**
- Ensure `AllowResizingColumns="True"`
- Check column doesn't have explicit `Width` set with `ColumnWidthMode="None"`
- Verify `MinimumWidth` and `MaximumWidth` are not equal

###Column Drag-Drop Not Working

**Problem:** Cannot drag columns.

**Solutions:**
- Ensure `AllowDraggingColumn="True"`
- Check `QueryColumnDragging` event isn't canceling operation
- Frozen columns might have restrictions

### Stacked Headers Not Showing

**Problem:** Stacked headers don't appear.

**Solutions:**
- Verify `ColumnMappingNames` match actual column `MappingName` values
- Check spelling and case sensitivity
- Ensure columns exist in grid

### Unbound Column Shows Empty

**Problem:** Unbound column doesn't show values.

**Solutions:**
- Implement `QueryUnboundColumnValue` event
- Set `e.Handled = true` after assigning value
- For expressions, verify property names and syntax

### Frozen Columns Don't Stay Fixed

**Problem:** Frozen columns scroll with others.

**Solutions:**
- Verify `FrozenColumnCount` is set correctly
- Check that count doesn't exceed total column count
- Restart if changed at runtime: reassign `ItemsSource`

## Next Steps

- Read [editing.md](editing.md) for cell editing features
- Read [sorting-filtering.md](sorting-filtering.md) for data operations
- Read [styling-customization.md](styling-customization.md) for appearance
