# Events in .NET MAUI PullToRefresh

## Overview

The PullToRefresh control provides three built-in events that give you fine-grained control over the pull-to-refresh lifecycle:

1. **Pulling**: Raised continuously while the user is pulling down
2. **Refreshing**: Raised when the user releases after pulling beyond the threshold
3. **Refreshed**: Raised when the refresh operation completes

These events enable you to track pull progress, control the refresh flow, and respond to refresh completion.

## Event Lifecycle

```
User starts pulling → Pulling (multiple times) → User releases → Refreshing → 
RefreshOperation → IsRefreshing = false → Refreshed
```

## Pulling Event

The `Pulling` event fires continuously as the user pulls down on the PullableContent. It provides real-time feedback about the pull progress.

### Event Signature

```csharp
public event EventHandler<PullingEventArgs> Pulling;
```

### PullingEventArgs Properties

| Property | Type | Description |
|----------|------|-------------|
| **Progress** | double | Gets the current pull progress value (0-100) |
| **Cancel** | bool | Set to `true` to cancel the pulling action |

### Basic Pulling Event

```xml
<syncfusion:SfPullToRefresh x:Name="pullToRefresh" 
                             Pulling="OnPulling">
    <syncfusion:SfPullToRefresh.PullableContent>
        <StackLayout>
            <Label x:Name="progressLabel" Text="Pull progress: 0%"/>
        </StackLayout>
    </syncfusion:SfPullToRefresh.PullableContent>
</syncfusion:SfPullToRefresh>
```

```csharp
private void OnPulling(object sender, PullingEventArgs args)
{
    // Update progress label
    progressLabel.Text = $"Pull progress: {args.Progress}%";
    
    // Optionally cancel pulling based on some condition
    // args.Cancel = true;
}
```

### Tracking Pull Progress

```csharp
private void OnPulling(object sender, PullingEventArgs args)
{
    var progress = args.Progress;
    
    // Update UI based on progress
    if (progress < 50)
    {
        statusLabel.Text = "Pull down to refresh";
        statusLabel.TextColor = Colors.Gray;
    }
    else if (progress < 100)
    {
        statusLabel.Text = "Keep pulling...";
        statusLabel.TextColor = Colors.Blue;
    }
    else
    {
        statusLabel.Text = "Release to refresh!";
        statusLabel.TextColor = Colors.Green;
    }
}
```

### Canceling Pull Action

Use the `Cancel` property to prevent the refresh from occurring:

```csharp
private bool isNetworkAvailable = true;

private void OnPulling(object sender, PullingEventArgs args)
{
    // Cancel pull if no network connection
    if (!isNetworkAvailable)
    {
        args.Cancel = true;
        DisplayAlert("No Connection", "Check your network and try again", "OK");
    }
}
```

### Conditional Pull Cancellation

```csharp
private DateTime lastRefreshTime = DateTime.Now;
private TimeSpan refreshCooldown = TimeSpan.FromSeconds(30);

private void OnPulling(object sender, PullingEventArgs args)
{
    // Prevent refresh if last refresh was recent
    var timeSinceLastRefresh = DateTime.Now - lastRefreshTime;
    
    if (timeSinceLastRefresh < refreshCooldown)
    {
        args.Cancel = true;
        
        var remainingSeconds = (refreshCooldown - timeSinceLastRefresh).TotalSeconds;
        statusLabel.Text = $"Please wait {remainingSeconds:F0} seconds";
    }
}
```

### Synchronizing with Custom Templates

Use the Pulling event to update custom template progress indicators:

```csharp
using Syncfusion.Maui.ProgressBar;

private SfCircularProgressBar progressBar;
private Label progressLabel;

private void OnPulling(object sender, PullingEventArgs args)
{
    if (progressBar != null)
    {
        // Update circular progress bar
        var absoluteProgress = Convert.ToInt32(Math.Abs(args.Progress));
        progressBar.Progress = absoluteProgress;
        progressBar.SetProgress(absoluteProgress, 1, Easing.CubicInOut);
        
        // Update progress label
        progressLabel.Text = args.Progress.ToString();
    }
}
```

## Refreshing Event

The `Refreshing` event fires when the user releases the pull gesture after exceeding the pulling threshold. This is where you perform your data refresh operations.

### Event Signature

```csharp
public event EventHandler Refreshing;
```

### Basic Refreshing Event

```xml
<syncfusion:SfPullToRefresh x:Name="pullToRefresh" 
                             Refreshing="OnRefreshing">
    <syncfusion:SfPullToRefresh.PullableContent>
        <ListView ItemsSource="{Binding Items}"/>
    </syncfusion:SfPullToRefresh.PullableContent>
</syncfusion:SfPullToRefresh>
```

```csharp
private async void OnRefreshing(object sender, EventArgs e)
{
    // Start refresh indicator
    pullToRefresh.IsRefreshing = true;
    
    // Perform refresh operation
    await LoadDataAsync();
    
    // Stop refresh indicator
    pullToRefresh.IsRefreshing = false;
}
```

### Refreshing with Try-Catch

```csharp
private async void OnRefreshing(object sender, EventArgs e)
{
    pullToRefresh.IsRefreshing = true;
    
    try
    {
        // Attempt to refresh data
        await apiService.RefreshDataAsync();
        statusLabel.Text = $"Updated: {DateTime.Now:hh:mm tt}";
    }
    catch (HttpRequestException)
    {
        await DisplayAlert("Error", "Unable to refresh. Check your connection.", "OK");
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", $"Refresh failed: {ex.Message}", "OK");
    }
    finally
    {
        // Always stop refresh indicator
        pullToRefresh.IsRefreshing = false;
    }
}
```

### Refreshing Multiple Data Sources

```csharp
private async void OnRefreshing(object sender, EventArgs e)
{
    pullToRefresh.IsRefreshing = true;
    
    try
    {
        // Refresh multiple sources in parallel
        var weatherTask = RefreshWeatherAsync();
        var newsTask = RefreshNewsAsync();
        var stocksTask = RefreshStocksAsync();
        
        await Task.WhenAll(weatherTask, newsTask, stocksTask);
        
        statusLabel.Text = "All data refreshed";
    }
    catch (Exception ex)
    {
        statusLabel.Text = $"Error: {ex.Message}";
    }
    finally
    {
        pullToRefresh.IsRefreshing = false;
    }
}

private async Task RefreshWeatherAsync()
{
    await Task.Delay(1000);
    // Refresh weather data
}

private async Task RefreshNewsAsync()
{
    await Task.Delay(1500);
    // Refresh news data
}

private async Task RefreshStocksAsync()
{
    await Task.Delay(800);
    // Refresh stock data
}
```

### Simulated Delay for Testing

```csharp
private async void OnRefreshing(object sender, EventArgs e)
{
    pullToRefresh.IsRefreshing = true;
    
    // Simulate network delay
    await Task.Delay(2000);
    
    // Update UI
    timestampLabel.Text = $"Last refresh: {DateTime.Now:g}";
    
    pullToRefresh.IsRefreshing = false;
}
```

### Event Runs Until IsRefreshing = false

**Important:** The Refreshing event continues until you explicitly set `IsRefreshing = false`. If you forget this, the refresh indicator will run indefinitely.

```csharp
// BAD: Refresh indicator never stops
private async void OnRefreshing(object sender, EventArgs e)
{
    await LoadDataAsync();
    // Missing: pullToRefresh.IsRefreshing = false;
}

// GOOD: Always set IsRefreshing to false
private async void OnRefreshing(object sender, EventArgs e)
{
    pullToRefresh.IsRefreshing = true;
    await LoadDataAsync();
    pullToRefresh.IsRefreshing = false; // ✓ Always include this
}
```

## Refreshed Event

The `Refreshed` event fires after the refresh operation completes, specifically after `IsRefreshing` is set to `false`.

### Event Signature

```csharp
public event EventHandler Refreshed;
```

### Basic Refreshed Event

```xml
<syncfusion:SfPullToRefresh x:Name="pullToRefresh" 
                             Refreshing="OnRefreshing"
                             Refreshed="OnRefreshed">
    <syncfusion:SfPullToRefresh.PullableContent>
        <ListView ItemsSource="{Binding Items}"/>
    </syncfusion:SfPullToRefresh.PullableContent>
</syncfusion:SfPullToRefresh>
```

```csharp
private async void OnRefreshing(object sender, EventArgs e)
{
    pullToRefresh.IsRefreshing = true;
    await LoadDataAsync();
    pullToRefresh.IsRefreshing = false;
}

private void OnRefreshed(object sender, EventArgs e)
{
    // Perform post-refresh actions
    DisplayAlert("Success", "Data refreshed successfully", "OK");
}
```

### Post-Refresh Analytics

```csharp
private DateTime refreshStartTime;

private void OnRefreshing(object sender, EventArgs e)
{
    refreshStartTime = DateTime.Now;
    pullToRefresh.IsRefreshing = true;
    
    await LoadDataAsync();
    
    pullToRefresh.IsRefreshing = false;
}

private void OnRefreshed(object sender, EventArgs e)
{
    // Calculate refresh duration
    var duration = DateTime.Now - refreshStartTime;
    
    // Log analytics
    analyticsService.LogRefresh(duration);
    
    // Update status
    statusLabel.Text = $"Refreshed in {duration.TotalSeconds:F1}s";
}
```

### Showing Success Messages

```csharp
private int itemsAdded = 0;

private async void OnRefreshing(object sender, EventArgs e)
{
    pullToRefresh.IsRefreshing = true;
    
    var newItems = await apiService.GetNewItemsAsync();
    itemsAdded = newItems.Count;
    
    foreach (var item in newItems)
    {
        viewModel.Items.Insert(0, item);
    }
    
    pullToRefresh.IsRefreshing = false;
}

private async void OnRefreshed(object sender, EventArgs e)
{
    if (itemsAdded > 0)
    {
        await DisplayAlert("Success", $"{itemsAdded} new items loaded", "OK");
    }
    else
    {
        await DisplayAlert("Info", "No new items available", "OK");
    }
}
```

### Triggering Animations

```csharp
private void OnRefreshed(object sender, EventArgs e)
{
    // Animate success indicator
    successIcon.Opacity = 0;
    successIcon.IsVisible = true;
    
    successIcon.FadeTo(1, 250);
    Task.Delay(2000).ContinueWith(_ =>
    {
        MainThread.BeginInvokeOnMainThread(() =>
        {
            successIcon.FadeTo(0, 250).ContinueWith(__ =>
            {
                successIcon.IsVisible = false;
            });
        });
    });
}
```

## Complete Event Example

Here's a comprehensive example using all three events together:

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.PullToRefresh;assembly=Syncfusion.Maui.PullToRefresh"
             x:Class="MyApp.EventsPage">
    
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        
        <StackLayout Grid.Row="0" Padding="20" BackgroundColor="LightBlue">
            <Label x:Name="pullStatusLabel" 
                   Text="Pull to refresh"
                   FontSize="14"
                   HorizontalOptions="Center"/>
            <ProgressBar x:Name="pullProgressBar" 
                        Progress="0"
                        ProgressColor="Blue"/>
        </StackLayout>
        
        <syncfusion:SfPullToRefresh Grid.Row="1"
                                     x:Name="pullToRefresh"
                                     Pulling="OnPulling"
                                     Refreshing="OnRefreshing"
                                     Refreshed="OnRefreshed">
            <syncfusion:SfPullToRefresh.PullableContent>
                <CollectionView x:Name="itemsView"
                               ItemsSource="{Binding Items}">
                    <CollectionView.ItemTemplate>
                        <DataTemplate>
                            <Frame Margin="10" Padding="15">
                                <Label Text="{Binding Title}" FontSize="16"/>
                            </Frame>
                        </DataTemplate>
                    </CollectionView.ItemTemplate>
                </CollectionView>
            </syncfusion:SfPullToRefresh.PullableContent>
        </syncfusion:SfPullToRefresh>
        
        <StackLayout Grid.Row="2" Padding="20" BackgroundColor="LightGray">
            <Label x:Name="statusLabel" 
                   Text="Ready"
                   FontSize="12"
                   HorizontalOptions="Center"/>
        </StackLayout>
    </Grid>
    
</ContentPage>
```

```csharp
using Syncfusion.Maui.PullToRefresh;

namespace MyApp
{
    public partial class EventsPage : ContentPage
    {
        private ViewModel viewModel;
        private DateTime lastRefreshTime;
        private int refreshCount = 0;
        
        public EventsPage()
        {
            InitializeComponent();
            viewModel = new ViewModel();
            this.BindingContext = viewModel;
        }
        
        private void OnPulling(object sender, PullingEventArgs args)
        {
            // Update progress bar and status
            var progress = args.Progress / 100.0;
            pullProgressBar.Progress = progress;
            
            if (args.Progress < 50)
            {
                pullStatusLabel.Text = $"Pull down ({args.Progress}%)";
                pullStatusLabel.TextColor = Colors.Gray;
            }
            else if (args.Progress < 100)
            {
                pullStatusLabel.Text = $"Keep pulling ({args.Progress}%)";
                pullStatusLabel.TextColor = Colors.Blue;
            }
            else
            {
                pullStatusLabel.Text = "Release to refresh!";
                pullStatusLabel.TextColor = Colors.Green;
            }
            
            // Optional: Cancel pull if recently refreshed
            var timeSinceRefresh = DateTime.Now - lastRefreshTime;
            if (timeSinceRefresh < TimeSpan.FromSeconds(5))
            {
                args.Cancel = true;
                pullStatusLabel.Text = "Please wait before refreshing again";
                pullStatusLabel.TextColor = Colors.Red;
            }
        }
        
        private async void OnRefreshing(object sender, EventArgs e)
        {
            pullToRefresh.IsRefreshing = true;
            pullStatusLabel.Text = "Refreshing...";
            statusLabel.Text = "Loading new data...";
            
            try
            {
                // Simulate data refresh
                await Task.Delay(2000);
                
                // Add new items
                for (int i = 0; i < 3; i++)
                {
                    viewModel.Items.Insert(0, new Item 
                    { 
                        Title = $"New Item {DateTime.Now.Ticks}" 
                    });
                }
                
                refreshCount++;
                lastRefreshTime = DateTime.Now;
                statusLabel.Text = $"Refresh #{refreshCount} completed at {lastRefreshTime:hh:mm:ss tt}";
            }
            catch (Exception ex)
            {
                statusLabel.Text = $"Error: {ex.Message}";
                await DisplayAlert("Error", "Failed to refresh data", "OK");
            }
            finally
            {
                pullToRefresh.IsRefreshing = false;
            }
        }
        
        private async void OnRefreshed(object sender, EventArgs e)
        {
            // Reset pull status
            pullStatusLabel.Text = "Pull to refresh";
            pullStatusLabel.TextColor = Colors.Gray;
            pullProgressBar.Progress = 0;
            
            // Show success notification
            await DisplayAlert("Success", 
                $"Data refreshed successfully! Total refreshes: {refreshCount}", 
                "OK");
            
            // Log analytics (if needed)
            System.Diagnostics.Debug.WriteLine($"Refresh completed: {DateTime.Now}");
        }
    }
    
    // ViewModel
    public class ViewModel : INotifyPropertyChanged
    {
        public ObservableCollection<Item> Items { get; set; }
        
        public ViewModel()
        {
            Items = new ObservableCollection<Item>();
            
            // Load initial data
            for (int i = 0; i < 20; i++)
            {
                Items.Add(new Item { Title = $"Item {i}" });
            }
        }
        
        public event PropertyChangedEventHandler PropertyChanged;
    }
    
    public class Item
    {
        public string Title { get; set; }
    }
}
```

## Event Handling Best Practices

### 1. Always Handle Exceptions

```csharp
private async void OnRefreshing(object sender, EventArgs e)
{
    pullToRefresh.IsRefreshing = true;
    
    try
    {
        await LoadDataAsync();
    }
    catch (Exception ex)
    {
        // Handle and log error
        System.Diagnostics.Debug.WriteLine($"Refresh error: {ex}");
    }
    finally
    {
        pullToRefresh.IsRefreshing = false;
    }
}
```

### 2. Provide User Feedback

Use the Pulling event to give real-time feedback:

```csharp
private void OnPulling(object sender, PullingEventArgs args)
{
    statusLabel.Text = args.Progress < 100 
        ? $"Pull progress: {args.Progress}%" 
        : "Release to refresh!";
}
```

### 3. Cancel Appropriately

Only cancel pulls when necessary (network issues, cooldown periods, etc.):

```csharp
private void OnPulling(object sender, PullingEventArgs args)
{
    if (!IsConnectedToNetwork())
    {
        args.Cancel = true;
        statusLabel.Text = "No network connection";
    }
}
```

### 4. Use Async/Await Properly

Always await asynchronous operations:

```csharp
private async void OnRefreshing(object sender, EventArgs e)
{
    pullToRefresh.IsRefreshing = true;
    
    await Task.WhenAll(
        RefreshDataAsync(),
        UpdateUIAsync()
    );
    
    pullToRefresh.IsRefreshing = false;
}
```

### 5. Track Event Metrics

Use events to collect analytics:

```csharp
private void OnRefreshed(object sender, EventArgs e)
{
    analyticsService.TrackEvent("RefreshCompleted", new Dictionary<string, string>
    {
        { "Duration", refreshDuration.TotalSeconds.ToString() },
        { "ItemsLoaded", newItemsCount.ToString() },
        { "Timestamp", DateTime.Now.ToString() }
    });
}
```

## Common Use Cases

### Use Case 1: Rate Limiting

Prevent too-frequent refreshes:

```csharp
private DateTime lastRefresh = DateTime.MinValue;
private TimeSpan minRefreshInterval = TimeSpan.FromSeconds(30);

private void OnPulling(object sender, PullingEventArgs args)
{
    if (DateTime.Now - lastRefresh < minRefreshInterval)
    {
        args.Cancel = true;
        var remaining = minRefreshInterval - (DateTime.Now - lastRefresh);
        statusLabel.Text = $"Wait {remaining.Seconds}s";
    }
}
```

### Use Case 2: Progress Tracking

Track and display detailed progress:

```csharp
private void OnPulling(object sender, PullingEventArgs args)
{
    progressBar.Progress = args.Progress / 100.0;
    progressLabel.Text = $"{args.Progress:F0}%";
}
```

### Use Case 3: Conditional Refresh

Refresh only when certain conditions are met:

```csharp
private void OnPulling(object sender, PullingEventArgs args)
{
    if (!userIsLoggedIn)
    {
        args.Cancel = true;
        statusLabel.Text = "Please log in to refresh";
    }
}
```

### Use Case 4: Multi-Step Refresh

Perform multiple refresh operations:

```csharp
private async void OnRefreshing(object sender, EventArgs e)
{
    pullToRefresh.IsRefreshing = true;
    
    statusLabel.Text = "Step 1: Loading user data...";
    await RefreshUserDataAsync();
    
    statusLabel.Text = "Step 2: Loading messages...";
    await RefreshMessagesAsync();
    
    statusLabel.Text = "Step 3: Loading settings...";
    await RefreshSettingsAsync();
    
    pullToRefresh.IsRefreshing = false;
}
```
