# Paging Modes and Navigation

## Table of Contents
- [Paging Modes](#paging-modes)
- [Normal Paging](#normal-paging)
- [On-Demand Paging](#on-demand-paging)
  - [LoadDynamicItems](#loaddynamicitems)
  - [PageCount for On-Demand Paging](#pagecount-for-on-demand-paging)
  - [ResetCache for Memory Optimization](#resetcache-for-memory-optimization)
- [Programmatic Navigation](#programmatic-navigation)
  - [MoveToFirstPage](#movetofirstpage)
  - [MoveToLastPage](#movetolastpage)
  - [MoveToNextPage](#movetonextpage)
  - [MoveToPreviousPage](#movetopreviouspage)
  - [MoveToPage](#movetopage)
- [Boundary Behavior](#boundary-behavior)
- [Choosing a Paging Mode](#choosing-a-paging-mode)

## Paging Modes

The DataPager supports two paging modes:

- **NormalPaging:** Loads the entire data collection into the `SfDataPager` upfront. The pager slices the collection into pages automatically.
- **OnDemandPaging:** Loads data for the current page dynamically. Use this when the full collection is large or fetched from a remote source.

## Normal Paging

In normal paging, bind the full data collection to `SfDataPager.Source`. The pager internally creates `SfDataPager.PagedSource`, which exposes the current page's data. Bind `PagedSource` to a display control's `ItemsSource`.

**Key properties:**
- `Source`: The full data collection.
- `PageSize`: Number of rows per page. Must not be 0 (throws `ArgumentException`).
- `NumericButtonCount`: Number of numeric buttons displayed.

**XAML:**

```xaml
<ContentPage.BindingContext>
    <local:OrderInfoViewModel x:Name="viewModel"/>
</ContentPage.BindingContext>

<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    <Border Grid.Row="1" Padding="5">
        <pager:SfDataPager x:Name="dataPager"
                           PageSize="15"
                           NumericButtonCount="10"
                           Source="{Binding Orders}">
        </pager:SfDataPager>
    </Border>
    <syncfusion:SfDataGrid x:Name="dataGrid"
                           Grid.Row="0"
                           ItemsSource="{Binding Source={x:Reference dataPager}, Path=PagedSource}">
    </syncfusion:SfDataGrid>
</Grid>
```

**C#:**

```csharp
using Syncfusion.Maui.DataGrid;
using Syncfusion.Maui.DataPager;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        SfDataPager dataPager = new SfDataPager();
        OrderInfoViewModel viewModel = new OrderInfoViewModel();
        dataPager.PageSize = 15;
        dataPager.NumericButtonCount = 10;
        dataPager.Source = viewModel.Orders;

        SfDataGrid dataGrid = new SfDataGrid();
        dataGrid.ItemsSource = dataPager.PagedSource;

        Border border = new Border();
        border.Padding = new Thickness(5);
        border.Content = dataPager;

        Grid grid = new Grid();
        grid.RowDefinitions.Add(new RowDefinition() { Height = GridLength.Star });
        grid.RowDefinitions.Add(new RowDefinition() { Height = GridLength.Auto });
        grid.Children.Add(dataGrid);
        grid.Children.Add(border);
        grid.SetRow(dataGrid, 0);
        grid.SetRow(border, 1);
        this.Content = grid;
    }
}
```

> **Note:** The `SfDataPager.PageSize` property should not be assigned with value 0. Setting `PageSize` to 0 will throw an `ArgumentException`.

## On-Demand Paging

In normal paging, the data collection is loaded entirely into the `SfDataPager` initially. For large datasets or remote data, set `SfDataPager.UseOnDemandPaging` to `true` to load the current page's items dynamically.

Hook into the `OnDemandLoading` event and use the `LoadDynamicItems` method to load data for the corresponding page.

**OnDemandLoadingEventArgs members:**
- `StartIndex`: The start index of the corresponding page.
- `PageSize`: The number of items to be loaded for that page.

The `OnDemandLoading` event is triggered whenever the pager moves to a page that has not been loaded yet.

**XAML:**

```xaml
<ContentPage.BindingContext>
    <local:OrderInfoViewModel x:Name="viewModel"/>
</ContentPage.BindingContext>

<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    <Border Grid.Row="1" Padding="5">
        <pager:SfDataPager x:Name="dataPager"
                           PageSize="15"
                           NumericButtonCount="10"
                           Source="{Binding Orders}"
                           OnDemandLoading="dataPager_OnDemandLoading"
                           UseOnDemandPaging="True">
        </pager:SfDataPager>
    </Border>
    <syncfusion:SfDataGrid x:Name="dataGrid"
                           Grid.Row="0"
                           ItemsSource="{Binding Source={x:Reference dataPager}, Path=PagedSource}">
    </syncfusion:SfDataGrid>
</Grid>
```

**C#:**

```csharp
SfDataPager dataPager = new SfDataPager();
OrderInfoViewModel viewModel = new OrderInfoViewModel();
dataPager.PageSize = 15;
dataPager.NumericButtonCount = 10;
dataPager.Source = viewModel.Orders;
dataPager.UseOnDemandPaging = true;
dataPager.OnDemandLoading += dataPager_OnDemandLoading;

SfDataGrid dataGrid = new SfDataGrid();
dataGrid.ItemsSource = dataPager.PagedSource;

Border border = new Border();
border.Padding = new Thickness(5);
border.Content = dataPager;

Grid grid = new Grid();
grid.RowDefinitions.Add(new RowDefinition() { Height = GridLength.Star });
grid.RowDefinitions.Add(new RowDefinition() { Height = GridLength.Auto });
grid.Children.Add(dataGrid);
grid.Children.Add(border);
grid.SetRow(dataGrid, 0);
grid.SetRow(border, 1);
this.Content = grid;
```

### LoadDynamicItems

Inside the `OnDemandLoading` handler, call `LoadDynamicItems` with the start index and the items for that page:

```csharp
private void dataPager_OnDemandLoading(object sender, OnDemandLoadingEventArgs e)
{
    dataPager.LoadDynamicItems(e.StartIndex, viewModel.Orders.Skip(e.StartIndex).Take(e.PageSize));
}
```

### PageCount for On-Demand Paging

> **Note:** In on-demand paging, you should not assign a value to the `Source` property. Instead, set the `PageCount` property to the total number of pages needed to display all data. This generates the required numeric buttons in the view. For example, if you have 1000 items and a page size of 15, set `PageCount` to 67.

```csharp
dataPager.UseOnDemandPaging = true;
dataPager.PageSize = 15;
dataPager.PageCount = 67; // 1000 items / 15 per page, rounded up
dataPager.OnDemandLoading += dataPager_OnDemandLoading;
```

### ResetCache for Memory Optimization

When using `OnDemandPaging`, `SfDataPager.PagedSource` loads only the current page data. Upon navigation to another page, the `OnDemandLoading` event fires and loads another set of data, but the pager maintains the previous page data in cache. When you navigate back to a previously viewed page, `OnDemandLoading` is not fired again — the cached data is loaded directly.

For improved performance with very large datasets or limited memory, call `Syncfusion.Data.PagedCollectionView.ResetCache()` in the `OnDemandLoading` event to discard cached pages except the current one. This reduces memory consumption but requires reloading data when navigating back to previously viewed pages.

```csharp
private void dataPager_OnDemandLoading(object sender, OnDemandLoadingEventArgs e)
{
    dataPager.LoadDynamicItems(e.StartIndex, viewModel.Orders.Skip(e.StartIndex).Take(e.PageSize));
    (dataPager.PagedSource as PagedCollectionView).ResetCache();
}
```

**When to use ResetCache:**
- Very large datasets where holding all visited pages in memory is costly.
- Mobile applications with tight memory constraints.
- Scenarios where re-fetching data is cheap (e.g., a fast local source or a cacheable API).

## Programmatic Navigation

The DataPager provides methods to navigate programmatically. These methods are useful for custom UI (e.g., a separate "Next" button outside the pager) or keyboard shortcuts.

### MoveToFirstPage

The `MoveToFirstPage()` method navigates to the first page.

```csharp
dataPager.MoveToFirstPage();
```

### MoveToLastPage

The `MoveToLastPage()` method navigates to the last page.

```csharp
dataPager.MoveToLastPage();
```

### MoveToNextPage

The `MoveToNextPage()` method navigates to the next page.

```csharp
dataPager.MoveToNextPage();
```

### MoveToPreviousPage

The `MoveToPreviousPage()` method navigates to the previous page.

```csharp
dataPager.MoveToPreviousPage();
```

### MoveToPage

The `MoveToPage(Int32)` method navigates to a specific page by index.

```csharp
dataPager.MoveToPage(5);
```

You can also navigate to a page with animation using the overload `MoveToPage(Int32, Int32, Boolean)`, where the second parameter specifies the duration in milliseconds and the Boolean parameter indicates whether to animate the transition.

```csharp
dataPager.MoveToPage(5, 300, true); // navigate to page 5 with a 300ms animation
```

## Boundary Behavior

When calling `MoveToNextPage()` on the last page, the pager remains on the last page. Similarly, calling `MoveToPreviousPage()` on the first page keeps the pager on the first page. These methods handle boundary conditions gracefully without throwing exceptions.

This means you can wire up navigation buttons without additional boundary checks:

```csharp
private void NextButton_Clicked(object sender, EventArgs e)
{
    dataPager.MoveToNextPage(); // safe even on the last page
}
```

## Choosing a Paging Mode

| Scenario | Recommended Mode | Why |
|---|---|---|
| Small-to-medium in-memory collection | Normal paging | Simplest setup; bind `Source` and read `PagedSource` |
| Large collection (10k+ items) loaded locally | Normal paging | The collection is already in memory; paging is just slicing |
| Remote/API data, page fetched on navigation | On-demand paging | Avoids loading all data upfront; use `OnDemandLoading` + `LoadDynamicItems` |
| Very large dataset with memory constraints | On-demand paging + `ResetCache()` | Discards visited pages to keep memory low |
| Need total page count without loading data | On-demand paging with `PageCount` | Set `PageCount` instead of `Source` to generate numeric buttons |
