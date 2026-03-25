# Paging and Virtualization

## Table of Contents
- [Paging](#paging)
  - [Enable Paging](#enable-paging)
  - [Page Size](#page-size)
  - [Page Navigation](#page-navigation)
  - [Page Count](#page-count)
  - [On-Demand Paging](#on-demand-paging)
  - [Numeric Button Shapes](#numeric-button-shapes)
  - [Generating Numeric Buttons](#generating-numeric-buttons)
  - [Customizing Button Size and Font Size](#customizing-button-size-and-font-size)
  - [Display Mode](#display-mode)
  - [Auto Ellipsis Mode](#auto-ellipsis-mode)
  - [Customize AutoEllipsisText](#customize-autoellipsistext)
  - [Programmatically Switch Pages](#programmatically-switch-pages)
  - [Orientation](#orientation)
  - [Paging Events](#paging-events)
  - [Paging Limitations](#paging-limitations)
- [Load More](#load-more)
- [Pull to Refresh](#pull-to-refresh)
- [Data Virtualization](#data-virtualization)

## Paging

Add `Syncfusion.Maui.DataGrid.DataPager` package for paging support.

### Enable Paging

```xml
<ContentPage xmlns:datapager="clr-namespace:Syncfusion.Maui.DataGrid.DataPager;assembly=Syncfusion.Maui.DataGrid"
>
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
        
        <datapager:SfDataPager x:Name ="dataPager"
                           Grid.Row="1"
                           PageSize="15" 
                           NumericButtonCount="10"
                           Source="{Binding OrdersInfo}">
      </datapager:SfDataPager>      
      <syncfusion:SfDataGrid x:Name="dataGrid"
                         Grid.Row="0"
                         SelectionMode="Single"
                         ItemsSource="{Binding Source={x:Reference dataPager}, Path=PagedSource }"  
                         >
      </syncfusion:SfDataGrid>
    </Grid>
</ContentPage>
```

### Page Size

```xml
<datapager:SfDataPager PageSize="25" />
```

```csharp
dataPager.PageSize = 25;
```

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
```

### Page Count

```csharp
int totalPages = dataPager.PageCount;
```

### On-Demand Paging

In normal Paging, the entire data collection is loaded initially into the `SfDataPager`. However, the control also allows for dynamically loading the data for the current page by setting [SfDataPager.UseOnDemandPaging](https://help.syncfusion.com/cr/maui/Syncfusion.Maui.DataGrid.DataPager.SfDataPager.html#Syncfusion_Maui_DataGrid_DataPager_SfDataPager_UseOnDemandPaging) to `true`.

To load the current page item dynamically, hook into the `OnDemandLoading` event. In this event, use the `LoadDynamicItems` method to load data for the corresponding page. The event contains `StartIndex` (page start index) and `PageSize` (number of items to load).

```xml
<datapager:SfDataPager x:Name ="dataPager"
                       Grid.Row="1"
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

**Performance Tip:** To enhance performance and avoid caching previous page data, call `ResetCache()` in the `OnDemandLoading` event:

```csharp
private void dataPager_OnDemandLoading(object sender, OnDemandLoadingEventArgs e)
{
    dataPager.LoadDynamicItems(e.StartIndex, viewModel.OrdersInfo.Skip(e.StartIndex).Take(e.PageSize));
    (dataPager.PagedSource as PagedCollectionView).ResetCache();
}
```

> **Note:** In on-demand paging, do not assign a value to the `Source` property. Define an integer value for the `PageCount` property to generate the required numeric buttons.

### Numeric Button Shapes

```xml
<datapager:SfDataPager x:Name="dataPager"
                       ButtonShape="Rectangle"
                       PageSize="15"
                       Source="{Binding OrdersInfo}">
</datapager:SfDataPager>
```

```csharp
dataPager.ButtonShape = DataPagerButtonShape.Rectangle;
```

### Generating Numeric Buttons

```xml
<datapager:SfDataPager x:Name="dataPager"
                       NumericButtonsGenerateMode="Auto"
                       PageSize="15"
                       Source="{Binding OrdersInfo}">
</datapager:SfDataPager>
```

```csharp
dataPager.NumericButtonsGenerateMode = DataPagerNumericButtonsGenerateMode.Auto;
```

> **Note:** The size of the `SfDataPager` is automatically adjusted based on available screen size if the view cannot accommodate the numeric buttons specified in the `NumericButtonCount` property.

### Customizing Button Size and Font Size

```xml
<datapager:SfDataPager x:Name="dataPager"
                       PageSize="15"
                       ButtonSize="60"
                       ButtonFontSize="21"
                       Source="{Binding OrdersInfo}">
</datapager:SfDataPager>
```

```csharp
dataPager.ButtonSize = 60;
dataPager.ButtonFontSize = 21;
```

### Display Mode

**Available Display Modes:**
- `None` - Do not display any page buttons
- `First` - Displays only the first page button
- `Last` - Displays only the last page button
- `Previous` - Displays only the previous page button
- `Next` - Displays only the next page button
- `Numeric` - Displays only the numeric page buttons
- `FirstLast` - Displays the first and last page buttons
- `PreviousNext` - Displays the previous and next page buttons
- `FirstLastNumeric` - Displays the first, last, and numeric page buttons
- `PreviousNextNumeric` - Displays the previous, next, and numeric page buttons
- `FirstLastPreviousNext` - Displays all navigation buttons without numeric buttons
- `FirstLastPreviousNextNumeric` - Displays all buttons

```xml
<datapager:SfDataPager x:Name="dataPager"
                       PageSize="15"
                       DisplayMode="FirstLastNumeric"
                       Source="{Binding OrdersInfo}">
</datapager:SfDataPager>
```

```csharp
dataPager.DisplayMode = DataPagerDisplayMode.FirstLastNumeric;
```

### Auto Ellipsis Mode

```xml
<datapager:SfDataPager x:Name="dataPager"
                       PageSize="15"
                       AutoEllipsisMode="After"
                       Source="{Binding OrdersInfo}">
</datapager:SfDataPager>
```

```csharp
dataPager.AutoEllipsisMode = DataPagerEllipsisMode.After;
```

### Customize AutoEllipsisText

```xml
<datapager:SfDataPager x:Name="dataPager"
                       PageSize="15"
                       AutoEllipsisMode="After"
                       AutoEllipsisText="..."
                       Source="{Binding OrdersInfo}">
</datapager:SfDataPager>
```

```csharp
dataPager.AutoEllipsisMode = DataPagerEllipsisMode.After;
dataPager.AutoEllipsisText = "***";
```

### Programmatically Switch Pages

#### Move to First Page

```csharp
dataPager.MoveToFirstPage();
```

#### Move to Last Page

```csharp
dataPager.MoveToLastPage();
```

#### Move to Next Page

```csharp
dataPager.MoveToNextPage();
```

#### Move to Previous Page

```csharp
dataPager.MoveToPreviousPage();
```

#### Move to Specific Page

```csharp
// Navigate to page 3
dataPager.MoveToPage(3);

// Navigate to page 3 with animation
dataPager.MoveToPage(3, 500, true);
```

### Orientation

```xml
<datapager:SfDataPager x:Name="dataPager"
                       PageSize="15"
                       Orientation="Vertical"
                       Source="{Binding OrdersInfo}">
</datapager:SfDataPager>
```

```csharp
dataPager.Orientation = DataPagerScrollOrientation.Vertical;
```

### Paging Events

#### PageChanging

```xml
<datapager:SfDataPager x:Name ="dataPager"
                       Grid.Row="1"
                       PageSize="15"
                       PageChanging="dataPager_PageChanging"
                       Source="{Binding OrdersInfo}">
</datapager:SfDataPager>
```

```csharp
private void dataPager_PageChanging(object sender, Syncfusion.Maui.DataGrid.DataPager.PageChangingEventArgs e)
{
    var oldPageIndex = e.OldPageIndex;
    var newPageIndex = e.NewPageIndex;
    // Handle page changing logic
}
```

#### PageChanged

```xml
<datapager:SfDataPager x:Name ="dataPager"
                       Grid.Row="1"
                       PageSize="15"
                       PageChanged="dataPager_PageChanged"
                       Source="{Binding OrdersInfo}">
</datapager:SfDataPager>
```

```csharp
private void dataPager_PageChanged(object sender, Syncfusion.Maui.DataGrid.DataPager.PageChangedEventArgs e)
{
    var oldPageIndex = e.OldPageIndex;
    var newPageIndex = e.NewPageIndex;
    // Handle page changed logic
}
```

#### Pager Style Customization

**Available Style Properties:**
- `DataPagerBackgroundColor` - Background color of the SfDataPager
- `NavigationButtonBackgroundColor` - Background color of navigation buttons
- `NavigationButtonDisableBackgroundColor` - Background color when disabled
- `NavigationButtonDisableIconColor` - Icon color when disabled
- `NavigationButtonIconColor` - Icon color of navigation buttons
- `NumericButtonBackgroundColor` - Background color for numeric buttons
- `NumericButtonSelectionBackgroundColor` - Background color of selected numeric button
- `NumericButtonSelectionTextColor` - Text color of selected numeric button
- `NumericButtonTextColor` - Text color of numeric buttons

```xml
<datapager:SfDataPager x:Name="dataPager"
                       PageSize="15"
                       Source="{Binding OrdersInfo}">
    <datapager:SfDataPager.DefaultStyle>
        <datapager:DataPagerStyle NumericButtonSelectionBackgroundColor="Pink"
                                  NumericButtonBackgroundColor="Purple"
                                  NavigationButtonBackgroundColor="LightBlue"
                                  NavigationButtonIconColor="Teal">
        </datapager:DataPagerStyle>
    </datapager:SfDataPager.DefaultStyle>
</datapager:SfDataPager>
```

```csharp
dataPager.DefaultStyle.NumericButtonSelectionBackgroundColor = Colors.Red;
dataPager.DefaultStyle.NumericButtonBackgroundColor = Colors.GreenYellow;
dataPager.DefaultStyle.NavigationButtonBackgroundColor = Colors.Black;
dataPager.DefaultStyle.NavigationButtonIconColor = Colors.White;
```

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

```xml
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
```xml
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
dataPager.PageIndexChanged += (s, e) =>
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

## Next Steps

- Read [performance-events.md](performance-events.md) for optimization
- Read [advanced-features.md](advanced-features.md) for more features
