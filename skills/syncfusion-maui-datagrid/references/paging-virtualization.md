# Paging and Virtualization

## Table of Contents
- [Paging](#paging)
  - [Enable Paging (DataGrid + DataPager integration)](#enable-paging-datagrid--datapager-integration)
  - [Page Size](#page-size)
  - [Page Navigation](#page-navigation)
  - [Page Count](#page-count)
  - [On-Demand Paging](#on-demand-paging)
  - [Pager Configuration (button shape, size, display mode, ellipsis, orientation, style, events)](#pager-configuration-button-shape-size-display-mode-ellipsis-orientation-style-events)
  - [Paging Events (DataGrid integration hook)](#paging-events-datagrid-integration-hook)
- [Load More](#load-more)
- [Pull to Refresh](#pull-to-refresh)
- [Data Virtualization](#data-virtualization)
- [Scroll Orientation](#scroll-orientation)
- [Shrink Wrap Rows and Columns](#shrink-wrap-rows-and-columns)

## Paging

The DataGrid integrates with the standalone **Syncfusion .NET MAUI DataPager** (`SfDataPager`) control for paging support. The DataPager is now shipped as a **separate NuGet package** — `Syncfusion.Maui.DataPager` — and has its own dedicated skill with full configuration reference.

**Package:** `Syncfusion.Maui.DataPager`

**Namespace (XAML):**
```xaml
xmlns:datapager="clr-namespace:Syncfusion.Maui.DataPager;assembly=Syncfusion.Maui.DataPager"
```

**C# using:**
```csharp
using Syncfusion.Maui.DataPager;
```

> **Migration note:** If you are upgrading from an older release where the pager lived inside `Syncfusion.Maui.DataGrid`, update the namespace from `clr-namespace:Syncfusion.Maui.DataGrid.DataPager;assembly=Syncfusion.Maui.DataGrid` to `clr-namespace:Syncfusion.Maui.DataPager;assembly=Syncfusion.Maui.DataPager`, and add the `Syncfusion.Maui.DataPager` package. The types (`SfDataPager`, `DataPagerStyle`, `PageChangingEventArgs`, `PageChangedEventArgs`, `OnDemandLoadingEventArgs`, `DataPagerButtonShape`, `DataPagerDisplayMode`, `DataPagerEllipsisMode`, `DataPagerScrollOrientation`, `DataPagerNumericButtonsGenerateMode`) moved to the `Syncfusion.Maui.DataPager` namespace.

### Enable Paging (DataGrid + DataPager integration)

The integration pattern is simple: bind the full collection to `SfDataPager.Source`, then bind `SfDataPager.PagedSource` to the DataGrid's `ItemsSource`.

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.DataGrid;assembly=Syncfusion.Maui.DataGrid"
             xmlns:datapager="clr-namespace:Syncfusion.Maui.DataPager;assembly=Syncfusion.Maui.DataPager"
             x:Class="DataGridDemo.MainPage">

    <ContentPage.BindingContext>
        <local:OrderInfoViewModel x:Name="viewModel"/>
    </ContentPage.BindingContext>

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <syncfusion:SfDataGrid x:Name="dataGrid"
                               Grid.Row="0"
                               SelectionMode="Single"
                               ItemsSource="{Binding Source={x:Reference dataPager}, Path=PagedSource}">
        </syncfusion:SfDataGrid>

        <Border Grid.Row="1" Padding="5">
            <datapager:SfDataPager x:Name="dataPager"
                                   PageSize="15"
                                   NumericButtonCount="10"
                                   Source="{Binding OrdersInfo}">
            </datapager:SfDataPager>
        </Border>
    </Grid>
</ContentPage>
```

**C# equivalent:**

```csharp
using Syncfusion.Maui.DataGrid;
using Syncfusion.Maui.DataPager;

SfDataPager dataPager = new SfDataPager();
dataPager.PageSize = 15;
dataPager.NumericButtonCount = 10;
dataPager.Source = viewModel.OrdersInfo;

SfDataGrid dataGrid = new SfDataGrid();
dataGrid.ItemsSource = dataPager.PagedSource;
```

### Page Size

```xaml
<datapager:SfDataPager PageSize="25" />
```

```csharp
dataPager.PageSize = 25;
```

> **Note:** `PageSize` must not be 0 — setting it to 0 throws an `ArgumentException`.

### Page Navigation

```csharp
// Next page
dataPager.MoveToNextPage();

// Previous page
dataPager.MoveToPreviousPage();

// First page
dataPager.MoveToFirstPage();

// Last page
dataPager.MoveToLastPage();

// Specific page
dataPager.MoveToPage(3);

// Specific page with animation (page, durationMs, animate)
dataPager.MoveToPage(3, 500, true);
```

These methods handle boundary conditions gracefully — calling `MoveToNextPage()` on the last page (or `MoveToPreviousPage()` on the first page) keeps the pager on the current page without throwing exceptions.

### Page Count

```csharp
int totalPages = dataPager.PageCount;
```

### On-Demand Paging

For large or remote data, set `UseOnDemandPaging` to `true` and load only the current page in the `OnDemandLoading` event using `LoadDynamicItems`.

```xaml
<datapager:SfDataPager x:Name="dataPager"
                       PageSize="15"
                       NumericButtonCount="10"
                       PageCount="10"
                       OnDemandLoading="dataPager_OnDemandLoading"
                       UseOnDemandPaging="True">
</datapager:SfDataPager>
```

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        dataPager.PageSize = 15;
        dataPager.NumericButtonCount = 10;
        dataPager.PageCount = 10;
        dataPager.UseOnDemandPaging = true;
        dataPager.OnDemandLoading += dataPager_OnDemandLoading;
    }

    private void dataPager_OnDemandLoading(object sender, OnDemandLoadingEventArgs e)
    {
        dataPager.LoadDynamicItems(e.StartIndex, viewModel.OrdersInfo.Skip(e.StartIndex).Take(e.PageSize));
    }
}
```

**Performance Tip:** To reduce memory when working with very large datasets, call `ResetCache()` in the `OnDemandLoading` event to discard cached pages except the current one:

```csharp
private void dataPager_OnDemandLoading(object sender, OnDemandLoadingEventArgs e)
{
    dataPager.LoadDynamicItems(e.StartIndex, viewModel.OrdersInfo.Skip(e.StartIndex).Take(e.PageSize));
    (dataPager.PagedSource as PagedCollectionView).ResetCache();
}
```

> **Note:** In on-demand paging, do not assign a value to the `Source` property. Set the `PageCount` property to the total number of pages to generate the required numeric buttons (e.g., 1000 items / 15 per page → `PageCount = 67`).

### Pager Configuration (button shape, size, display mode, ellipsis, orientation, style, events)

All DataPager appearance and behavior configuration — `ButtonShape`, `NumericButtonsGenerateMode`, `ButtonSize`, `ButtonFontSize`, `DisplayMode`, `AutoEllipsisMode`, `AutoEllipsisText`, `Orientation`, `DataPagerStyle` / `DefaultStyle` (colors and button templates), and the `PageChanging` / `PageChanged` events — is documented in the dedicated DataPager skill.

➡️ **For full DataPager configuration, see the `syncfusion-maui-datapager` skill:**
- `getting-started.md` — install the `Syncfusion.Maui.DataPager` package, register the handler, model/viewmodel setup
- `paging-modes.md` — normal vs on-demand paging, `LoadDynamicItems`, `ResetCache`, programmatic navigation
- `customization.md` — `ButtonShape`, `NumericButtonsGenerateMode`, `ButtonSize`/`ButtonFontSize`, `DisplayMode`, `AutoEllipsisMode`/`AutoEllipsisText`, `Orientation`
- `appearance.md` — `DataPagerStyle` colors and navigation button templates
- `events.md` — `PageChanging` and `PageChanged` event handlers

**Quick reference — most-used pager properties:**

| Property | Purpose |
|---|---|
| `Source` | Full data collection (normal paging) |
| `PagedSource` | Bind to DataGrid `ItemsSource` |
| `PageSize` | Items per page (must be > 0) |
| `PageCount` | Total pages (on-demand paging, instead of `Source`) |
| `NumericButtonCount` | Number of numeric buttons shown |
| `UseOnDemandPaging` | Enable on-demand loading |
| `DisplayMode` | Which buttons (First/Last/Prev/Next/Numeric) are visible |
| `Orientation` | `Horizontal` (default) or `Vertical` |
| `DefaultStyle` | `DataPagerStyle` for colors and button templates |

### Paging Events (DataGrid integration hook)

When the pager's page changes, you typically refresh DataGrid-dependent UI (headers, summaries, selection state). Wire `PageChanged` for post-navigation reactions, or `PageChanging` for pre-navigation validation:

```csharp
// React after the page has changed
dataPager.PageChanged += (s, e) =>
{
    // e.OldPageIndex, e.NewPageIndex
    UpdateGridHeader(e.NewPageIndex);
};

// Validate before the page changes
dataPager.PageChanging += (s, e) =>
{
    // e.OldPageIndex, e.NewPageIndex
    if (HasUnsavedChanges())
    {
        // optionally warn the user before navigation
    }
};
```

> **Event signature note:** With the separate package, the event-args types live in `Syncfusion.Maui.DataPager`. Use `Syncfusion.Maui.DataPager.PageChangingEventArgs` and `Syncfusion.Maui.DataPager.PageChangedEventArgs` in your handler signatures.


## Load More

Implement load more feature to load records to the items source when scroll view reaches top or bottom position:

```csharp
dataGrid.AllowLoadMore = true;
dataGrid.LoadMoreCommand = new Command(ExecuteLoadMoreCommand);

private async void ExecuteLoadMoreCommand()
{
    this.dataGrid.IsBusy = true;
    await Task.Delay(new TimeSpan(0, 0, 5));
    viewModel.LoadMoreItems();
    this.dataGrid.IsBusy = false;
}
```

## Pull to Refresh

Enable pull-to-refresh gesture:

```xaml
<syncfusion:SfDataGrid AllowPullToRefresh="True"
                       PullToRefreshCommand="{Binding RefreshCommand}" />
```

```csharp
dataGrid.AllowPullToRefresh = true;
Command RefreshCommand = new Command(ExecutePullToRefreshCommand);
dataGrid.PullToRefreshCommand = RefreshCommand;

private async void ExecutePullToRefreshCommand()
{
    this.dataGrid.IsBusy = true;
    await Task.Delay(new TimeSpan(0, 0, 5));
    viewModel.ItemsSourceRefresh();
    this.dataGrid.IsBusy = false;
}

//ViewModel.cs
public void ItemsSourceRefresh()
{
    int count = random.Next (1, 10);
    for (int i = 1; i <= count; i++) 
    {
        this.OrdersInfo!.Insert(0, new OrderInfo()
        {
            OrderID = i,
            CustomerID = this.customerID[this.random.Next(15)],
            EmployeeID = this.random.Next(1700, 1800),
        });
    }        
}
```

## Data Virtualization

DataGrid provides support to handle the large amount of data through built-in virtualization feature. With Data virtualization, the record entries will be created in the runtime only upon scrolling to the vertical end which increases the performance of grid loading time.
```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding EmployeeDetails}"
                       EnableDataVirtualization="True"/>
```
```csharp
dataGrid.EnableDataVirtualization = true;
```
### Optimize for Large Datasets

```csharp
// Use simple template columns to improve performance
dataGrid.AutoGenerateColumnsMode = AutoGenerateColumnsMode.None;

// Avoid complex CellTemplates for large datasets
// Use built-in column types when possible
```

### LiveDataUpdateMode

Control how grid responds to data changes:

```csharp
dataGrid.Loaded += (s, e) =>
{
    dataGrid.View.LiveDataUpdateMode = LiveDataUpdateMode.AllowDataShaping;
};
```

**Modes:**
- `Default` - No automatic updates
- `AllowSummaryUpdate` - Update summaries on data change
- `AllowDataShaping` - Update sorting, filtering, grouping on data change

## Performance Tips

1. **Use ObservableCollection** for better performance with INPC
2. **Limit complex templates** - Use built-in column types
3. **Enable paging** for datasets > 1000 items
4. **Avoid excessive sorting/grouping** on large datasets
5. **Defer loading** - Use Load More instead of loading all data upfront

## Common Patterns

### Pattern 1: Paged Grid

```csharp
// Load first page
var firstPage = LoadPageFromServer(pageIndex: 0, pageSize: 50);
viewModel.Orders = new ObservableCollection<OrderInfo>(firstPage);

// Handle page changes
dataPager.PageChanged += (s, e) =>
{
    var page = LoadPageFromServer(e.NewPageIndex, dataPager.PageSize);
    viewModel.Orders.Clear();
    foreach (var item in page)
    {
        viewModel.Orders.Add(item);
    }
};
```

### Pattern 2: Infinite Scroll

```csharp
dataGrid.LoadMoreCommand = new Command(async () =>
{
    dataGrid.IsBusy = true;
    
    var nextBatch = await LoadNextBatchAsync();
    foreach (var item in nextBatch)
    {
        viewModel.Orders.Add(item);
    }
    
    dataGrid.IsBusy = false;
});
```

## Scroll to Row and Column

Navigate to specific rows and columns programmatically with customizable scroll positions.

### Scroll to Row Index

```csharp
// Scroll to row 15, make it visible
dataGrid.ScrollToRowIndex(15, ScrollToPosition.MakeVisible, true);

// Scroll to row 15, position at start of view
dataGrid.ScrollToRowIndex(15, ScrollToPosition.Start, true);

// Scroll to row 15, position at end of view
dataGrid.ScrollToRowIndex(15, ScrollToPosition.End, true);

// Scroll to row 15, position at center of view
dataGrid.ScrollToRowIndex(15, ScrollToPosition.Center, true);
```

### Scroll to Column Index

```csharp
// Scroll to column 3
dataGrid.ScrollToColumnIndex(3, ScrollToPosition.MakeVisible, true);

// Scroll to specific column with start position
dataGrid.ScrollToColumnIndex(2, ScrollToPosition.Start, true);
```

### Scroll to Row Using Data Object

```csharp
// Scroll to a specific order data object
OrderInfo targetOrder = viewModel.Orders[10];

// Scroll to row at start position with animation
dataGrid.ScrollToRow(targetOrder, ScrollToPosition.Start, true);

// Scroll to row centered in view
dataGrid.ScrollToRow(targetOrder, ScrollToPosition.Center, true);

// Scroll to make row visible without animation
dataGrid.ScrollToRow(targetOrder, ScrollToPosition.MakeVisible, false);
```

### Scroll to Column Using DataGridColumn

```csharp
// Scroll to a specific column by reference
DataGridColumn customerColumn = dataGrid.Columns["CustomerID"];

// Scroll to column at start position with animation
dataGrid.ScrollToColumn(customerColumn, ScrollToPosition.Start, true);

// Scroll to column at center with animation
dataGrid.ScrollToColumn(customerColumn, ScrollToPosition.Center, true);

// Scroll without animation
dataGrid.ScrollToColumn(customerColumn, ScrollToPosition.MakeVisible, false);
```

## Scroll Orientation

The `ScrollOrientation` property controls the direction in which the grid can be scrolled. The default value is `Both`.

**Supported values:**
- `Both` (default) — Enables both vertical and horizontal scrolling.
- `Vertical` — Enables vertical scrolling only.
- `Horizontal` — Enables horizontal scrolling only.
- `Neither` — Disables scrolling.

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding Orders}"
                       ScrollOrientation="Vertical">
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.ScrollOrientation = ScrollOrientation.Vertical;
```

Use `Vertical` when you have many rows but want to prevent horizontal panning, or `Horizontal` to lock vertical scrolling. `Neither` is useful when the grid is embedded in an outer scroll container that should own all scrolling.

## Shrink Wrap Rows and Columns

When the height or width of the DataGrid is unbounded (infinite), the DataGrid sets its height or width to 300 by default. Enable `ShrinkWrapRows` to size the grid's height to the available rows, and `ShrinkWrapColumns` to size the grid's width to the available columns.

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding Orders}"
                       ShrinkWrapRows="True"
                       ShrinkWrapColumns="True">
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.ShrinkWrapRows = true;
dataGrid.ShrinkWrapColumns = true;
```

> **Performance note:** Shrink wrapping is considerably more expensive than specifying a fixed height or width because the DataGrid must measure all rows or columns to determine its size. Use these properties only when the DataGrid contains a relatively small number of rows and columns.

## Next Steps

- Read [performance-events.md](performance-events.md) for optimization
- Read [advanced-features.md](advanced-features.md) for more features
