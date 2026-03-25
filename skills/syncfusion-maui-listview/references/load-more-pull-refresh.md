# Load More and Pull to Refresh in .NET MAUI ListView

## Table of Contents
- [Overview](#overview)
- [Load More Feature](#load-more-feature)
- [Pull to Refresh](#pull-to-refresh)
- [Best Practices for Large Datasets](#best-practices-for-large-datasets)

## Overview

SfListView supports loading additional data when reaching the end of the list (Load More) and refreshing data by pulling down from the top (Pull to Refresh).

## Load More Feature

Load additional items when the user scrolls to the bottom of the list.

### Load More Position

The `SfListView` enables the Load More view by setting the `SfListView.LoadMoreOption` and `SfListView.LoadMoreCommand` properties. The Load More view can be displayed either at the start or the end of the list by using the `SfListView.LoadMorePosition` property.

- **Start**: Positions the Load More view at the start of the list.
- **End**: Positions the Load More view at the end of the list when the user reaches the end. This is the default value.

Example (Load more at the end):

```xml
<syncfusion:SfListView LoadMoreOption="Auto"
                       LoadMorePosition="End"
                       LoadMoreCommand="{Binding LoadMoreItemsCommand}"
                       IsLazyLoading="{Binding IsLoading}" />
```

Example (Load more at the start with manual mode):

```xml
<syncfusion:SfListView LoadMoreOption="Manual"
                       LoadMorePosition="Start"
                       LoadMoreCommand="{Binding LoadMoreItemsCommand}"
                       IsLazyLoading="{Binding IsLoading}" />
```

### Show Loading Indicator with IsLazyLoading

The LoadMore Indicator will be displayed when loading more items. Use the `SfListView.IsLazyLoading` property to control the visibility of the loading indicator and manage the load more button state.

**How to Use IsLazyLoading:**
- Set `IsLazyLoading = true` **before** adding items to the list (starts loading)
- Set `IsLazyLoading = false` **after** adding items (stops loading)
- Bind the `IsLazyLoading` property to a ViewModel property that tracks loading state
- You can interchange the visibility of the button and busy indicator when creating the load more view

**Important Notes:**
- For Manual and Auto LoadMoreOptions, the LoadMoreCommand can fire even when the list starts empty
- The property should be bound to your ViewModel's loading state property
- Always set it to `false` in a `finally` block to ensure the indicator is hidden even if errors occur

### Auto Load More

```xml
<syncfusion:SfListView LoadMoreOption="Auto"
                       LoadMoreCommand="{Binding LoadMoreItemsCommand}"
                       IsLazyLoading="{Binding IsLoading}" />
```

```csharp
using Syncfusion.Maui.ListView;

listView.LoadMoreOption = LoadMoreOption.Auto;
listView.LoadMoreCommand = new Command(ExecuteLoadMore);
listView.IsLazyLoading = false; // Set to true when loading, false when done
```

Automatically loads more items when scrolling reaches the bottom. Use `IsLazyLoading` to show/hide loading indicator.

### Manual Load More

```xml
<syncfusion:SfListView LoadMoreOption="Manual"
                       LoadMoreCommand="{Binding LoadMoreItemsCommand}"
                       IsLazyLoading="{Binding IsLoading}" />
```

Shows a "Load More" button that users must tap to load additional items. The command can fire even when the list starts empty. Use `IsLazyLoading` property to control visibility of loading indicator and button.

### AutoOnScroll

```xml
<syncfusion:SfListView LoadMoreOption="AutoOnScroll"
                       LoadMoreCommand="{Binding LoadMoreItemsCommand}"
                       IsLazyLoading="{Binding IsLoading}" />
```

Loads more items only when user scrolls, not on initial load. Use `IsLazyLoading` property to control the visibility of loading indicator when scrolling triggers load more.

### Disable Load More

```xml
<syncfusion:SfListView LoadMoreOption="None" />
```

### LoadMoreCommand Implementation

> ⚠️ **IMPORTANT:** Use `IsLazyLoading` property to control loading indicator visibility during load more operations, NOT `IsBusy`. The `IsLazyLoading` property should be set to `true` before loading and `false` after completion.

```csharp
public class ProductViewModel : INotifyPropertyChanged
{
    private ObservableCollection<Product> products;
    private bool isLoading;
    private int currentPage = 1;
    private const int PageSize = 20;
    
    public ObservableCollection<Product> Products
    {
        get => products;
        set { products = value; OnPropertyChanged(); }
    }
    
    public bool IsLoading
    {
        get => isLoading;
        set { isLoading = value; OnPropertyChanged(); }
    }
    
    public Command LoadMoreItemsCommand { get; }
    
    public ProductViewModel()
    {
        Products = new ObservableCollection<Product>();
        LoadMoreItemsCommand = new Command(async () => await LoadMoreItems());
        
        // Load initial data
        LoadInitialData();
    }
    
    private void LoadInitialData()
    {
        for (int i = 1; i <= PageSize; i++)
        {
            Products.Add(new Product 
            { 
                Id = i, 
                Name = $"Product {i}" 
            });
        }
    }
    
    private async Task LoadMoreItems()
    {
        if (IsLoading) return;
        
        IsLoading = true;  // This binds to IsLazyLoading to show loading indicator
        
        try
        {
            // Simulate API call
            await Task.Delay(1000);
            
            currentPage++;
            int startId = (currentPage - 1) * PageSize + 1;
            
            for (int i = 0; i < PageSize; i++)
            {
                Products.Add(new Product 
                { 
                    Id = startId + i, 
                    Name = $"Product {startId + i}" 
                });
            }
        }
        finally
        {
            IsLoading = false;  // This binds to IsLazyLoading to hide loading indicator
        }
    }
    
    // INotifyPropertyChanged implementation
    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Custom Load More Template

```xml
<syncfusion:SfListView LoadMoreOption="Manual" IsLazyLoading="{Binding IsLoading}">
    <syncfusion:SfListView.LoadMoreTemplate>
        <DataTemplate>
            <Grid Padding="20" HeightRequest="60">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                
                <!-- Show loading indicator when IsLazyLoading is true -->
                <ActivityIndicator Grid.Column="0"
                                   IsRunning="{Binding IsLoading}"
                                   IsVisible="{Binding IsLoading}"
                                   VerticalOptions="Center"
                                   Margin="0,0,10,0" />
                
                <!-- Show button when not loading (IsLoading = false) -->
                <Button Grid.Column="1"
                        Text="Load More Items"
                        Command="{Binding LoadMoreCommand}"
                        IsEnabled="{Binding IsLoading, Converter={StaticResource InverseBoolConverter}}"
                        HorizontalOptions="Center" />
            </Grid>
        </DataTemplate>
    </syncfusion:SfListView.LoadMoreTemplate>
</syncfusion:SfListView>
```

### Detect End of Data

```csharp
private bool hasMoreItems = true;

private async Task LoadMoreItems()
{
    if (IsLoading || !hasMoreItems) return;
    
    IsLoading = true;  // Binds to IsLazyLoading
    
    try
    {
        var newItems = await apiService.GetProductsAsync(currentPage, PageSize);
        
        if (newItems.Count < PageSize)
        {
            // Last page reached
            hasMoreItems = false;
            listView.LoadMoreOption = LoadMoreOption.None;
        }
        
        foreach (var item in newItems)
        {
            Products.Add(item);
        }
        
        currentPage++;
    }
    finally
    {
        IsLoading = false;  // Binds to IsLazyLoading
    }
}
```

## Pull to Refresh

Enable users to refresh data by pulling down from the top of the list.

### Basic Pull to Refresh
Place `SfListView` inside the `SfPullToRefresh` and set the PullableContent of `SfPullToRefresh` as `SfListView`.

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
                xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                x:Class="RefreshableListView.MainPage"
                xmlns:ListView="clr-namespace:Syncfusion.Maui.ListView;assembly=Syncfusion.Maui.ListView"
                xmlns:pulltoRefresh="clr-namespace:Syncfusion.Maui.PullToRefresh;assembly=Syncfusion.Maui.PullToRefresh"
                xmlns:local="clr-namespace:RefreshableListView">

        <ContentPage.Behaviors>
            <local:ListViewPullToRefreshBehavior />
        </ContentPage.Behaviors>

        <ContentPage.Content>
            <Grid>
                <pulltoRefresh:SfPullToRefresh x:Name="pullToRefresh"
                                            RefreshViewHeight="50"
                                            RefreshViewThreshold="30"
                                            PullingThreshold="150"
                                            RefreshViewWidth="50"
                                            TransitionMode="SlideOnTop"
                                            IsRefreshing="False">
                    <pulltoRefresh:SfPullToRefresh.PullableContent>
                        <Grid x:Name="mainGrid">
                            <ListView:SfListView Grid.Row="1"
                                                x:Name="listView"
                                                ItemsSource="{Binding InboxInfos}"
                                                ItemSize="102"
                                                SelectionMode="Single"
                                                ScrollBarVisibility="Always"
                                                AutoFitMode="Height">
                                . . . 
                                . . . .

                            </ListView:SfListView>
                        </Grid>
                    </pulltoRefresh:SfPullToRefresh.PullableContent>
                </pulltoRefresh:SfPullToRefresh>
            </Grid>
        </ContentPage.Content>
    </ContentPage>
```

```csharp
using Syncfusion.Maui.DataSource;
using Syncfusion.Maui.DataSource.Extensions;
using Syncfusion.Maui.ListView;
using Syncfusion.Maui.PullToRefresh;

    namespace RefreshableListView
    {
        protected override void OnAttachedTo(ContentPage bindable)
        {
            ViewModel = new ListViewInboxInfoViewModel();
            bindable.BindingContext = ViewModel;
            pullToRefresh = bindable.FindByName<SfPullToRefresh>("pullToRefresh");
            ListView = bindable.FindByName<SfListView>("listView");
            pullToRefresh.Refreshing += PullToRefresh_Refreshing;

            base.OnAttachedTo(bindable);
        }

        private async void PullToRefresh_Refreshing(object? sender, EventArgs e)
        {
            pullToRefresh!.IsRefreshing = true;
            await Task.Delay(2500);
            ViewModel!.AddItemsRefresh(3);
            pullToRefresh.IsRefreshing = false;
        }
    }
```

## Best Practices for Large Datasets

### 1. Use Virtualization

ListView already uses virtualization, but ensure ItemSize is set for optimal performance:

```xml
<syncfusion:SfListView ItemSize="100" />
```

### 2. Implement Paging with SfDataPager

Load data in chunks (pages) rather than all at once.

### 3. Optimize Item Templates

Keep templates simple for better scrolling performance:

```xml
<!-- GOOD: Simple template -->
<Grid Padding="10">
    <Label Text="{Binding Name}" />
</Grid>

<!-- AVOID: Complex nested layouts -->
<Grid>
    <Frame>
        <StackLayout>
            <Grid>
                <!-- Too many nested levels -->
            </Grid>
        </StackLayout>
    </Frame>
</Grid>
```

### 4. Image Optimization

- Use appropriate image sizes
- Enable image caching
- Consider lazy loading images

```xml
<Image Source="{Binding ThumbnailUrl}" 
       CachingEnabled="True"
       HeightRequest="80" 
       WidthRequest="80" />
```

### 5. Debounce Rapid Refreshes

```csharp
private CancellationTokenSource refreshCts;

private async Task RefreshData()
{
    refreshCts?.Cancel();
    refreshCts = new CancellationTokenSource();
    
    IsRefreshing = true;
    
    try
    {
        await Task.Delay(300, refreshCts.Token); // Debounce
        
        // Fetch and update data
        var freshData = await apiService.GetProductsAsync(1, PageSize);
        
        Products.Clear();
        foreach (var product in freshData)
        {
            Products.Add(product);
        }
    }
    catch (TaskCanceledException)
    {
        // Refresh was cancelled, ignore
    }
    finally
    {
        IsRefreshing = false;
    }
}
```

### 6. Handle Errors Gracefully

```csharp
private async Task LoadMoreItems()
{
    if (isLoadingMore || !hasMoreItems) return;
    
    isLoadingMore = true;
    
    try
    {
        var newItems = await FetchProductsAsync(currentPage, PageSize);
        
        foreach (var item in newItems)
        {
            Products.Add(item);
        }
        
        currentPage++;
    }
    catch (Exception ex)
    {
        // Show error message
        await Application.Current.MainPage.DisplayAlert(
            "Error", 
            "Failed to load more items. Please try again.", 
            "OK");
        
        // Log error
        System.Diagnostics.Debug.WriteLine($"Load more error: {ex.Message}");
    }
    finally
    {
        isLoadingMore = false;
    }
}
```

### 7. Use BeginInit/EndInit for Batch Updates

```csharp
private async Task RefreshData()
{
    IsRefreshing = true;
    
    try
    {
        var freshData = await apiService.GetProductsAsync(1, PageSize);
        
        // Batch update for better performance
        Products.Clear();
        
        listView.DataSource?.BeginInit();
        
        foreach (var product in freshData)
        {
            Products.Add(product);
        }
        
        listView.DataSource?.EndInit();
    }
    finally
    {
        IsRefreshing = false;
    }
}
```

## Troubleshooting

**Issue:** `IsBusy` property not found error
→ ❌ **DO NOT** use `IsBusy` property on SfListView. Use `IsLazyLoading` instead for load more operations, and `IsBusy` for pull-to-refresh operations.

**Issue:** Loading indicator not showing during load more
→ Verify `IsLazyLoading` is bound to your ViewModel property, check that you set it to `true` before loading and `false` after loading

**Issue:** Load more not triggering
→ Verify `LoadMoreOption` is not `None`, check `LoadMoreCommand` is bound correctly, ensure `IsLazyLoading` is properly managed

**Issue:** Pull to refresh not working
→ Ensure ListView is set as PullableContent for PushToRefresh component.

**Issue:** Refresh indicator stuck
→ Always set IsRefreshing/IsBusy to false in finally block

**Issue:** Duplicate items loading
→ Check for race conditions, implement proper loading flags

**Issue:** Poor performance with large lists
→ Use fixed `ItemSize`, simplify templates, optimize images, implement proper paging with `PageSize`

**Issue:** Load More command fires but nothing happens
→ Verify that `IsLazyLoading` is being set correctly in the command execution, check that items are being added to the `Products` collection
