# Pullable Content in .NET MAUI PullToRefresh

## Table of Contents
- [Overview](#overview)
- [PullableContent Property](#pullablecontent-property)
- [Basic Content Configuration](#basic-content-configuration)
- [Hosting .NET MAUI DataGrid](#hosting-net-maui-datagrid)
- [Hosting .NET MAUI ListView](#hosting-net-maui-listview)
- [Hosting Custom Views and Layouts](#hosting-custom-views-and-layouts)
- [Best Practices](#best-practices)
- [Common Scenarios](#common-scenarios)
- [Troubleshooting](#troubleshooting)

## Overview

The `PullableContent` property is the main view of the PullToRefresh control where you place the content that users can pull down to refresh. This can be any .NET MAUI view, including complex controls like ListView, DataGrid, CollectionView, or custom layouts.

## PullableContent Property

The `PullableContent` property accepts any View-derived type, making it highly flexible for various content types.

### Property Details

- **Type:** `View`
- **Purpose:** Defines the area where pull-to-refresh gesture is recognized
- **Usage:** Wrap your scrollable or refreshable content within this property

### Basic Syntax

```xml
<syncfusion:SfPullToRefresh>
    <syncfusion:SfPullToRefresh.PullableContent>
        <!-- Your content here -->
    </syncfusion:SfPullToRefresh.PullableContent>
</syncfusion:SfPullToRefresh>
```

## Basic Content Configuration

### Simple Label Content

```xml
<syncfusion:SfPullToRefresh x:Name="pullToRefresh"
                             PullingThreshold="120"
                             RefreshViewHeight="30"
                             RefreshViewThreshold="30"
                             RefreshViewWidth="30"
                             Refreshing="OnRefreshing">
    <syncfusion:SfPullToRefresh.PullableContent>
        <Label x:Name="monthLabel" 
               TextColor="Black" 
               FontSize="18"
               HorizontalTextAlignment="Center"   
               VerticalTextAlignment="Start" 
               Padding="20"/>
    </syncfusion:SfPullToRefresh.PullableContent>
</syncfusion:SfPullToRefresh>
```

### StackLayout with Multiple Elements

```xml
<syncfusion:SfPullToRefresh x:Name="pullToRefresh"
                             Refreshing="OnRefreshing">
    <syncfusion:SfPullToRefresh.PullableContent>
        <StackLayout BackgroundColor="White" 
                     Padding="20"
                     Spacing="15">
            <Label Text="Weather Information" 
                   FontSize="24" 
                   FontAttributes="Bold"/>
            <Label x:Name="temperatureLabel" 
                   Text="25°C" 
                   FontSize="48"/>
            <Label x:Name="conditionLabel" 
                   Text="Sunny" 
                   FontSize="18"/>
            <Label Text="Pull down to refresh" 
                   FontSize="12" 
                   TextColor="Gray"/>
        </StackLayout>
    </syncfusion:SfPullToRefresh.PullableContent>
</syncfusion:SfPullToRefresh>
```

```csharp
private async void OnRefreshing(object sender, EventArgs e)
{
    pullToRefresh.IsRefreshing = true;
    
    // Simulate weather data refresh
    await Task.Delay(1500);
    temperatureLabel.Text = $"{new Random().Next(15, 35)}°C";
    
    pullToRefresh.IsRefreshing = false;
}
```

## Hosting .NET MAUI DataGrid

PullToRefresh provides excellent support for refreshing data in Syncfusion's DataGrid control.

### Prerequisites

Install the DataGrid NuGet package:
```bash
dotnet add package Syncfusion.Maui.DataGrid
```

### Step-by-Step Implementation

#### 1. Add Namespace References

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sfgrid="clr-namespace:Syncfusion.Maui.DataGrid;assembly=Syncfusion.Maui.DataGrid"
             xmlns:pulltoRefresh="clr-namespace:Syncfusion.Maui.PullToRefresh;assembly=Syncfusion.Maui.PullToRefresh"
             x:Class="MyApp.MainPage">
```

#### 2. Define DataGrid as PullableContent

```xml
<pulltoRefresh:SfPullToRefresh x:Name="pullToRefresh"
                               RefreshViewHeight="50"
                               RefreshViewThreshold="30"
                               PullingThreshold="150"
                               RefreshViewWidth="50"
                               TransitionMode="SlideOnTop"
                               Refreshing="OnRefreshing">
    <pulltoRefresh:SfPullToRefresh.PullableContent>
        <sfgrid:SfDataGrid x:Name="dataGrid"
                          HeaderRowHeight="52"
                          RowHeight="48"
                          ItemsSource="{Binding Orders}"
                          AutoGenerateColumnsMode="None"
                          ColumnWidthMode="Fill">
            <sfgrid:SfDataGrid.Columns>
                <sfgrid:DataGridTextColumn MappingName="OrderID" 
                                          HeaderText="Order ID"/>
                <sfgrid:DataGridTextColumn MappingName="CustomerName" 
                                          HeaderText="Customer"/>
                <sfgrid:DataGridTextColumn MappingName="ShipCity" 
                                          HeaderText="City"/>
                <sfgrid:DataGridNumericColumn MappingName="Freight" 
                                             HeaderText="Freight"
                                             Format="C"/>
            </sfgrid:SfDataGrid.Columns>
        </sfgrid:SfDataGrid>
    </pulltoRefresh:SfPullToRefresh.PullableContent>
</pulltoRefresh:SfPullToRefresh>
```

#### 3. Handle Refresh Event for DataGrid

```csharp
using Syncfusion.Maui.DataGrid;
using Syncfusion.Maui.PullToRefresh;

namespace MyApp
{
    public partial class MainPage : ContentPage
    {
        private OrderViewModel viewModel;
        
        public MainPage()
        {
            InitializeComponent();
            viewModel = new OrderViewModel();
            this.BindingContext = viewModel;
        }

        private async void OnRefreshing(object sender, EventArgs e)
        {
            pullToRefresh.IsRefreshing = true;
            
            // Refresh data source
            await Task.Delay(2000);
            viewModel.RefreshOrders(10); // Add 10 new orders
            
            pullToRefresh.IsRefreshing = false;
        }
    }
}
```

### DataGrid with Push TransitionMode

To use Push mode instead of SlideOnTop:

```xml
<pulltoRefresh:SfPullToRefresh TransitionMode="Push"
                               Refreshing="OnRefreshing">
    <pulltoRefresh:SfPullToRefresh.PullableContent>
        <sfgrid:SfDataGrid x:Name="dataGrid"
                          ItemsSource="{Binding Orders}">
            <!-- Columns definition -->
        </sfgrid:SfDataGrid>
    </pulltoRefresh:SfPullToRefresh.PullableContent>
</pulltoRefresh:SfPullToRefresh>
```

**Visual Difference:**
- **SlideOnTop:** Refresh indicator appears above the DataGrid
- **Push:** Refresh indicator pushes the DataGrid down

## Hosting .NET MAUI ListView

ListView is another common use case for pull-to-refresh functionality, especially for lists of items.

### Prerequisites

Install the ListView NuGet package:
```bash
dotnet add package Syncfusion.Maui.ListView
```

### Step-by-Step Implementation

#### 1. Add Namespace References

```xml
<ContentPage xmlns:listView="clr-namespace:Syncfusion.Maui.ListView;assembly=Syncfusion.Maui.ListView"
             xmlns:pulltoRefresh="clr-namespace:Syncfusion.Maui.PullToRefresh;assembly=Syncfusion.Maui.PullToRefresh">
```

#### 2. Define ListView as PullableContent

```xml
<pulltoRefresh:SfPullToRefresh x:Name="pullToRefresh"
                               RefreshViewHeight="50"
                               RefreshViewThreshold="30"
                               PullingThreshold="150"
                               RefreshViewWidth="50"
                               TransitionMode="SlideOnTop"
                               Refreshing="OnRefreshing">
    <pulltoRefresh:SfPullToRefresh.PullableContent>
        <Grid>
            <listView:SfListView x:Name="listView"
                                ItemsSource="{Binding InboxItems}"
                                ItemSize="102"
                                SelectionMode="Single">
                <listView:SfListView.ItemTemplate>
                    <DataTemplate>
                        <Grid Padding="10">
                            <Grid.RowDefinitions>
                                <RowDefinition Height="Auto"/>
                                <RowDefinition Height="Auto"/>
                            </Grid.RowDefinitions>
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="60"/>
                                <ColumnDefinition Width="*"/>
                            </Grid.ColumnDefinitions>
                            
                            <Image Source="{Binding ProfileImage}" 
                                   Grid.Row="0" 
                                   Grid.RowSpan="2"
                                   Grid.Column="0"
                                   WidthRequest="50"
                                   HeightRequest="50"/>
                            
                            <Label Text="{Binding Name}" 
                                   Grid.Row="0" 
                                   Grid.Column="1"
                                   FontAttributes="Bold"
                                   FontSize="16"/>
                            
                            <Label Text="{Binding Message}" 
                                   Grid.Row="1" 
                                   Grid.Column="1"
                                   FontSize="14"
                                   TextColor="Gray"/>
                        </Grid>
                    </DataTemplate>
                </listView:SfListView.ItemTemplate>
            </listView:SfListView>
        </Grid>
    </pulltoRefresh:SfPullToRefresh.PullableContent>
</pulltoRefresh:SfPullToRefresh>
```

#### 3. Handle Refresh Event for ListView

```csharp
using Syncfusion.Maui.ListView;
using Syncfusion.Maui.PullToRefresh;

namespace MyApp
{
    public partial class MainPage : ContentPage
    {
        private InboxViewModel viewModel;
        
        public MainPage()
        {
            InitializeComponent();
            viewModel = new InboxViewModel();
            this.BindingContext = viewModel;
        }

        private async void OnRefreshing(object sender, EventArgs e)
        {
            pullToRefresh.IsRefreshing = true;
            
            // Simulate loading new messages
            await Task.Delay(2500);
            viewModel.AddNewItems(3); // Add 3 new inbox items
            
            pullToRefresh.IsRefreshing = false;
        }
    }
}
```

### ViewModel Example for ListView

```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;

public class InboxViewModel : INotifyPropertyChanged
{
    public ObservableCollection<InboxItem> InboxItems { get; set; }
    
    public InboxViewModel()
    {
        InboxItems = new ObservableCollection<InboxItem>();
        LoadInitialData();
    }
    
    private void LoadInitialData()
    {
        for (int i = 0; i < 20; i++)
        {
            InboxItems.Add(new InboxItem
            {
                Name = $"Contact {i}",
                Message = $"This is message {i}",
                ProfileImage = "profile.png"
            });
        }
    }
    
    public void AddNewItems(int count)
    {
        for (int i = 0; i < count; i++)
        {
            InboxItems.Insert(0, new InboxItem
            {
                Name = $"New Contact {i}",
                Message = $"New message {i}",
                ProfileImage = "profile.png"
            });
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
}

public class InboxItem
{
    public string Name { get; set; }
    public string Message { get; set; }
    public string ProfileImage { get; set; }
}
```

## Hosting Custom Views and Layouts

You can host any custom view or layout as PullableContent.

### ScrollView with Custom Content

```xml
<syncfusion:SfPullToRefresh x:Name="pullToRefresh"
                             Refreshing="OnRefreshing">
    <syncfusion:SfPullToRefresh.PullableContent>
        <ScrollView>
            <StackLayout Padding="20" Spacing="15">
                <Frame CornerRadius="10" HasShadow="True">
                    <StackLayout>
                        <Label Text="Dashboard" 
                               FontSize="24" 
                               FontAttributes="Bold"/>
                        <Label x:Name="statsLabel" 
                               Text="Loading stats..." 
                               FontSize="16"/>
                    </StackLayout>
                </Frame>
                
                <Frame CornerRadius="10" HasShadow="True">
                    <StackLayout>
                        <Label Text="Recent Activity" 
                               FontSize="20" 
                               FontAttributes="Bold"/>
                        <Label x:Name="activityLabel" 
                               Text="Loading activity..." 
                               FontSize="14"/>
                    </StackLayout>
                </Frame>
                
                <Frame CornerRadius="10" HasShadow="True">
                    <StackLayout>
                        <Label Text="Notifications" 
                               FontSize="20" 
                               FontAttributes="Bold"/>
                        <Label x:Name="notificationLabel" 
                               Text="Loading notifications..." 
                               FontSize="14"/>
                    </StackLayout>
                </Frame>
            </StackLayout>
        </ScrollView>
    </syncfusion:SfPullToRefresh.PullableContent>
</syncfusion:SfPullToRefresh>
```

### CollectionView (Built-in MAUI)

```xml
<syncfusion:SfPullToRefresh x:Name="pullToRefresh"
                             Refreshing="OnRefreshing">
    <syncfusion:SfPullToRefresh.PullableContent>
        <CollectionView ItemsSource="{Binding Products}">
            <CollectionView.ItemTemplate>
                <DataTemplate>
                    <Frame Margin="10" CornerRadius="8">
                        <StackLayout>
                            <Label Text="{Binding Name}" 
                                   FontSize="18" 
                                   FontAttributes="Bold"/>
                            <Label Text="{Binding Price, StringFormat='${0:F2}'}" 
                                   FontSize="16"/>
                        </StackLayout>
                    </Frame>
                </DataTemplate>
            </CollectionView.ItemTemplate>
        </CollectionView>
    </syncfusion:SfPullToRefresh.PullableContent>
</syncfusion:SfPullToRefresh>
```

## Best Practices

### 1. Use Observable Collections

Always use `ObservableCollection<T>` for data binding to ensure UI updates when items are added or removed:

```csharp
public ObservableCollection<Item> Items { get; set; } = new();
```

### 2. Async Data Loading

Perform data loading asynchronously to avoid blocking the UI:

```csharp
private async void OnRefreshing(object sender, EventArgs e)
{
    pullToRefresh.IsRefreshing = true;
    
    try
    {
        // Asynchronous data fetch
        var newData = await apiService.FetchDataAsync();
        viewModel.UpdateData(newData);
    }
    catch (Exception ex)
    {
        // Handle errors
        await DisplayAlert("Error", ex.Message, "OK");
    }
    finally
    {
        pullToRefresh.IsRefreshing = false;
    }
}
```

### 3. Provide Visual Feedback

Give users clear feedback during refresh operations:

```csharp
private async void OnRefreshing(object sender, EventArgs e)
{
    pullToRefresh.IsRefreshing = true;
    statusLabel.Text = "Refreshing...";
    
    await LoadDataAsync();
    
    statusLabel.Text = $"Last updated: {DateTime.Now:hh:mm tt}";
    pullToRefresh.IsRefreshing = false;
}
```

### 4. Handle Errors Gracefully

Always handle potential errors during refresh:

```csharp
private async void OnRefreshing(object sender, EventArgs e)
{
    pullToRefresh.IsRefreshing = true;
    
    try
    {
        await viewModel.RefreshDataAsync();
    }
    catch (HttpRequestException)
    {
        await DisplayAlert("Network Error", "Unable to refresh. Check your connection.", "OK");
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", $"Refresh failed: {ex.Message}", "OK");
    }
    finally
    {
        pullToRefresh.IsRefreshing = false;
    }
}
```

## Common Scenarios

### Scenario 1: Refresh with Timestamp

```csharp
private DateTime lastRefreshTime;

private async void OnRefreshing(object sender, EventArgs e)
{
    pullToRefresh.IsRefreshing = true;
    
    await viewModel.RefreshAsync();
    lastRefreshTime = DateTime.Now;
    timestampLabel.Text = $"Updated: {lastRefreshTime:g}";
    
    pullToRefresh.IsRefreshing = false;
}
```

### Scenario 2: Incremental Data Loading

```csharp
private async void OnRefreshing(object sender, EventArgs e)
{
    pullToRefresh.IsRefreshing = true;
    
    // Load only new items since last refresh
    var newItems = await apiService.GetNewItemsSince(lastRefreshTime);
    
    foreach (var item in newItems)
    {
        viewModel.Items.Insert(0, item);
    }
    
    pullToRefresh.IsRefreshing = false;
}
```

### Scenario 3: Full Data Replacement

```csharp
private async void OnRefreshing(object sender, EventArgs e)
{
    pullToRefresh.IsRefreshing = true;
    
    // Clear and reload all data
    viewModel.Items.Clear();
    var freshData = await apiService.GetAllItemsAsync();
    
    foreach (var item in freshData)
    {
        viewModel.Items.Add(item);
    }
    
    pullToRefresh.IsRefreshing = false;
}
```

## Troubleshooting

### Issue: PullableContent Not Scrolling

**Problem:** Content doesn't respond to pull gesture

**Solution:** Ensure the content is scrollable or tall enough to detect pull gestures:

```xml
<syncfusion:SfPullToRefresh.PullableContent>
    <ScrollView>
        <!-- Content here -->
    </ScrollView>
</syncfusion:SfPullToRefresh.PullableContent>
```

### Issue: Refresh Indicator Never Disappears

**Problem:** Progress animation runs indefinitely

**Solution:** Always set `IsRefreshing = false` after refresh completes:

```csharp
private async void OnRefreshing(object sender, EventArgs e)
{
    pullToRefresh.IsRefreshing = true;
    
    try
    {
        await LoadDataAsync();
    }
    finally
    {
        // Always execute, even if exception occurs
        pullToRefresh.IsRefreshing = false;
    }
}
```

### Issue: UI Not Updating After Refresh

**Problem:** Data refreshes but UI doesn't update

**Solution:** Use `ObservableCollection` and implement `INotifyPropertyChanged`:

```csharp
public class ViewModel : INotifyPropertyChanged
{
    private ObservableCollection<Item> items;
    
    public ObservableCollection<Item> Items
    {
        get => items;
        set
        {
            items = value;
            OnPropertyChanged(nameof(Items));
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Issue: Pull Gesture Not Detected

**Problem:** Pull-to-refresh doesn't trigger

**Solution:** Check that PullToRefresh has proper size/layout:

```xml
<syncfusion:SfPullToRefresh HorizontalOptions="FillAndExpand"
                             VerticalOptions="FillAndExpand">
    <!-- PullableContent -->
</syncfusion:SfPullToRefresh>
```
