# Performance and Events

## Table of Contents
- [Performance Optimization](#performance-optimization)
- [Grid Events](#grid-events)
- [Event Handling Patterns](#event-handling-patterns)

## Performance Optimization

### Large Dataset Best Practices

1. **Use Paging** for datasets > 1000 items
2. **Avoid Complex Templates** - Use built-in column types
3. **Enable Data Virtualization** (enabled by default)
4. **Limit Sorting/Grouping** on very large datasets
5. **Use ObservableCollection** for better performance with INPC

### Optimize Column Templates

```xml
<!-- Avoid -->
<syncfusion:DataGridTemplateColumn>
    <syncfusion:DataGridTemplateColumn.CellTemplate>
        <DataTemplate>
            <Grid>
                <Image Source="{Binding LargeImage}" />
                <Label Text="{Binding ComplexCalculation}" />
            </Grid>
        </DataTemplate>
    </syncfusion:DataGridTemplateColumn.CellTemplate>
</syncfusion:DataGridTemplateColumn>

<!-- Prefer -->
<syncfusion:DataGridTextColumn MappingName="SimpleProperty" />
```

### Memory Management

```csharp
// Clear references when done
dataGrid.ItemsSource = null;
dataGrid.Columns.Clear();
```

## Grid Events

### Grid Loaded

```csharp
dataGrid.DataGridLoaded  += DataGrid_Loaded;

private void DataGrid_Loaded(object sender, EventArgs e)
{
    // Grid is fully loaded
    ConfigureGridSettings();
}
```

### Selection Events

See [selection.md](selection.md#selection-events)

### Editing Events

See [editing.md](editing.md#editing-events)

### Column Events

```csharp
// Column resizing
dataGrid.ColumnResizing += DataGrid_ColumnResizing;

// Column dragging
dataGrid.QueryColumnDragging += DataGrid_QueryColumnDragging;
```

### Row Events

```csharp
// Row height query
dataGrid.QueryRowHeight += DataGrid_QueryRowHeight;

// Row dragging
dataGrid.QueryRowDragging += DataGrid_QueryRowDragging;

// Row swiping
dataGrid.SwipeStarted += DataGrid_SwipeStarted;
dataGrid.SwipeEnded += DataGrid_SwipeEnded;
```

### Cell Events

```Xml
// Cell tapped
<syncfusion:SfDataGrid x:Name="dataGrid"
                   CellTapped="dataGrid_CellTapped"
                   ItemsSource="{Binding OrderInfoCollection}" />
```

```csharp
// Cell tapped
dataGrid.CellTapped += DataGrid_CellTapped;

private void dataGrid_CellTapped(object sender, DataGridCellTappedEventArgs e)
{
    var rowIndex = e.RowColumnIndex.RowIndex;
    var rowData = e.RowData;
    var columnIndex = e.RowColumnIndex.ColumnIndex;
    var column = e.Column;
}
```

```Xml
// Cell double tapped
<syncfusion:SfDataGrid x:Name="dataGrid"
                   CellDoubleTapped="dataGrid_CellDoubleTapped"
                   ItemsSource="{Binding OrderInfoCollection}" /> 
```
```csharp
// Cell double tapped
dataGrid.CellDoubleTapped += DataGrid_CellDoubleTapped;

private void dataGrid_CellDoubleTapped(object sender, DataGridCellDoubleTappedEventArgs e)
{
    var rowIndex = e.RowColumnIndex.RowIndex;
    var rowData = e.RowData;
    var columnIndex = e.RowColumnIndex.ColumnIndex;
    var column = e.Column;
}

```xml
<syncfusion:SfDataGrid x:Name="dataGrid"
                   CellLongPress="dataGrid_CellLongPress"
                   ItemsSource="{Binding OrderInfoCollection}" />
```

```csharp

// Cell long pressed
dataGrid.CellLongPress += DataGrid_CellLongPress;

private void dataGrid_CellLongPress(object sender, DataGridCellLongPressEventArgs e)
{
    var rowIndex = e.RowColumnIndex.RowIndex;
    var rowData = e.RowData;
    var columnIndex = e.RowColumnIndex.ColumnIndex;
    var column = e.Column;
}
```

```xml
<syncfusion:SfDataGrid x:Name="dataGrid"
                   CellRightTapped="SfDataGrid_CellRightTapped"
                   ItemsSource="{Binding OrderInfoCollection}" />
```

```csharp

dataGrid.CellRightTapped += SfDataGrid_CellRightTapped;

private void SfDataGrid_CellRightTapped(object sender, DataGridCellRightTappedEventArgs e)
{
    var rowIndex = e.RowColumnIndex.RowIndex;
    var rowData = e.RowData;
    var columnIndex = e.RowColumnIndex.ColumnIndex;
    var column = e.Column;
    var pointerDeviceType = e.PointerDeviceType;
}
```
## Event Handling Patterns

### Pattern 1: Handle Cell Tap

```csharp
dataGrid.CellTapped += (s, e) =>
{
    var order = e.RowData as OrderInfo;
    var columnName = e.Column.MappingName;
    
    DisplayAlert("Cell Tapped", 
        $"Order: {order.OrderID}, Column: {columnName}", "OK");
};
```

### Pattern 2: Handle Double Tap for Quick Edit

```csharp
dataGrid.CellDoubleTapped += (s, e) =>
{
    if (e.Column.MappingName == "Status")
    {
        var order = e.RowData as OrderInfo;
        order.Status = order.Status == "Active" ? "Inactive" : "Active";
    }
};
```

### Pattern 3: Lazy Loading on Scroll

```csharp
dataGrid.DataGridLoaded += (s, e) =>
{
    dataGrid.View.CollectionChanged += (sender, args) =>
    {
        if (args.Action == NotifyCollectionChangedAction.Add)
        {
            // Load more data when needed
            LoadMoreDataIfNeeded();
        }
    };
};
```

## Performance Monitoring  

```csharp
var stopwatch = System.Diagnostics.Stopwatch.StartNew();

// Load data
dataGrid.ItemsSource = largeDataset;

stopwatch.Stop();
Debug.WriteLine($"Load time: {stopwatch.ElapsedMilliseconds}ms");
```

## Next Steps

- Read [paging-virtualization.md](paging-virtualization.md) for data loading
- Read [advanced-features.md](advanced-features.md) for more features
