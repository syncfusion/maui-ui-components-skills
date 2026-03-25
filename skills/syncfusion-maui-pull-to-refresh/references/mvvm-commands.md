# MVVM and Commands in .NET MAUI PullToRefresh

## Table of Contents
- [Overview](#overview)
- [MVVM Compatibility](#mvvm-compatibility)
- [IsRefreshing Property Binding](#isrefreshing-property-binding)
- [RefreshCommand Property](#refreshcommand-property)
- [RefreshCommandParameter](#refreshcommandparameter)
- [CanExecute Method](#canexecute-method)
- [Complete MVVM Examples](#complete-mvvm-examples)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

The SfPullToRefresh control is fully MVVM (Model-View-ViewModel) compatible, allowing you to implement pull-to-refresh functionality without code-behind. You can bind the refresh state to ViewModel properties and execute refresh logic through commands.

## MVVM Compatibility

PullToRefresh supports MVVM through two key mechanisms:

1. **IsRefreshing Property Binding**: Two-way bind the refresh state to a ViewModel property
2. **RefreshCommand**: Execute ViewModel logic when pull-to-refresh is triggered

### Benefits of MVVM Approach

- Separation of UI and business logic
- Unit testable refresh logic
- Reusable ViewModel across platforms
- Cleaner code organization
- Better maintainability

## IsRefreshing Property Binding

Bind the `IsRefreshing` property to a ViewModel property for two-way refresh state management.

### XAML Binding

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.PullToRefresh;assembly=Syncfusion.Maui.PullToRefresh"
             xmlns:local="clr-namespace:MyApp"
             x:Class="MyApp.MainPage">
    
    <ContentPage.BindingContext>
        <local:WeatherViewModel />
    </ContentPage.BindingContext>
    
    <syncfusion:SfPullToRefresh IsRefreshing="{Binding IsRefreshing}"
                                 RefreshCommand="{Binding RefreshCommand}">
        <syncfusion:SfPullToRefresh.PullableContent>
            <StackLayout BackgroundColor="#00AFF9" Padding="20">
                <Label Text="New York Temperature"
                       FontSize="Large"
                       TextColor="White"
                       Margin="20"/>
                <Label Text="{Binding Temperature}"
                       FontSize="Large"
                       TextColor="White"
                       Margin="20"
                       HeightRequest="100"/>
            </StackLayout>
        </syncfusion:SfPullToRefresh.PullableContent>
    </syncfusion:SfPullToRefresh>
    
</ContentPage>
```

### ViewModel Implementation

```csharp
using System.ComponentModel;
using System.Runtime.CompilerServices;
using System.Windows.Input;

namespace MyApp
{
    public class WeatherViewModel : INotifyPropertyChanged
    {
        private bool isRefreshing;
        private string temperature;
        private Random random = new Random();
        private string[] temperatureArray = { "22°C", "25°C", "18°C", "30°C", "15°C", "28°C" };
        
        public WeatherViewModel()
        {
            Temperature = "25°C";
            RefreshCommand = new Command(async () => await RefreshAsync());
        }
        
        public bool IsRefreshing
        {
            get => isRefreshing;
            set
            {
                if (isRefreshing != value)
                {
                    isRefreshing = value;
                    OnPropertyChanged();
                }
            }
        }
        
        public string Temperature
        {
            get => temperature;
            set
            {
                if (temperature != value)
                {
                    temperature = value;
                    OnPropertyChanged();
                }
            }
        }
        
        public ICommand RefreshCommand { get; }
        
        private async Task RefreshAsync()
        {
            IsRefreshing = true;
            
            // Simulate API call
            await Task.Delay(1000);
            
            // Update temperature
            int index = random.Next(0, temperatureArray.Length);
            Temperature = temperatureArray[index];
            
            IsRefreshing = false;
        }
        
        public event PropertyChangedEventHandler PropertyChanged;
        
        protected virtual void OnPropertyChanged([CallerMemberName] string propertyName = null)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

### Two-Way Binding Behavior

- **User pulls down** → `RefreshCommand` executes → ViewModel sets `IsRefreshing = true`
- **Refresh completes** → ViewModel sets `IsRefreshing = false` → Progress indicator hides
- **Programmatic refresh** → ViewModel sets `IsRefreshing = true` → UI updates automatically

## RefreshCommand Property

The `RefreshCommand` executes when the user completes a pull-to-refresh gesture (pulling beyond threshold and releasing).

### Basic Command Setup

```xml
<syncfusion:SfPullToRefresh RefreshCommand="{Binding RefreshCommand}">
    <syncfusion:SfPullToRefresh.PullableContent>
        <ListView ItemsSource="{Binding Items}"/>
    </syncfusion:SfPullToRefresh.PullableContent>
</syncfusion:SfPullToRefresh>
```

### ViewModel with Command

```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;
using System.Windows.Input;

public class ItemsViewModel : INotifyPropertyChanged
{
    public ObservableCollection<Item> Items { get; set; }
    public ICommand RefreshCommand { get; }
    
    private bool isRefreshing;
    public bool IsRefreshing
    {
        get => isRefreshing;
        set
        {
            isRefreshing = value;
            OnPropertyChanged(nameof(IsRefreshing));
        }
    }
    
    public ItemsViewModel()
    {
        Items = new ObservableCollection<Item>();
        LoadInitialData();
        
        RefreshCommand = new Command(async () => await ExecuteRefreshAsync());
    }
    
    private void LoadInitialData()
    {
        for (int i = 0; i < 20; i++)
        {
            Items.Add(new Item { Title = $"Item {i}", Description = $"Description {i}" });
        }
    }
    
    private async Task ExecuteRefreshAsync()
    {
        IsRefreshing = true;
        
        try
        {
            // Simulate API call
            await Task.Delay(2000);
            
            // Add new items at the top
            for (int i = 0; i < 5; i++)
            {
                Items.Insert(0, new Item 
                { 
                    Title = $"New Item {i}", 
                    Description = $"Fresh content {i}" 
                });
            }
        }
        catch (Exception ex)
        {
            // Handle errors
            System.Diagnostics.Debug.WriteLine($"Refresh failed: {ex.Message}");
        }
        finally
        {
            IsRefreshing = false;
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}

public class Item
{
    public string Title { get; set; }
    public string Description { get; set; }
}
```

### Command Execution Flow

1. User pulls down on PullableContent
2. User releases after exceeding PullingThreshold
3. `RefreshCommand.CanExecute()` is checked
4. If `CanExecute()` returns `true`, `RefreshCommand.Execute()` is called
5. ViewModel performs refresh logic
6. ViewModel sets `IsRefreshing = false` to hide indicator

## RefreshCommandParameter

Pass data to the RefreshCommand using `RefreshCommandParameter`.

### XAML with Parameter

```xml
<syncfusion:SfPullToRefresh RefreshCommand="{Binding RefreshCommand}"
                             RefreshCommandParameter="WeatherData">
    <syncfusion:SfPullToRefresh.PullableContent>
        <Label Text="{Binding Data}"/>
    </syncfusion:SfPullToRefresh.PullableContent>
</syncfusion:SfPullToRefresh>
```

### ViewModel Handling Parameter

```csharp
public class ViewModel : INotifyPropertyChanged
{
    public ICommand RefreshCommand { get; }
    
    public ViewModel()
    {
        RefreshCommand = new Command<string>(async (parameter) => 
        {
            await RefreshAsync(parameter);
        });
    }
    
    private async Task RefreshAsync(string dataType)
    {
        IsRefreshing = true;
        
        // Use parameter to determine what to refresh
        if (dataType == "WeatherData")
        {
            await RefreshWeatherAsync();
        }
        else if (dataType == "NewsData")
        {
            await RefreshNewsAsync();
        }
        
        IsRefreshing = false;
    }
    
    private async Task RefreshWeatherAsync()
    {
        await Task.Delay(1000);
        // Refresh weather data
    }
    
    private async Task RefreshNewsAsync()
    {
        await Task.Delay(1000);
        // Refresh news data
    }
}
```

### Binding to ViewModel Property as Parameter

```xml
<syncfusion:SfPullToRefresh RefreshCommand="{Binding RefreshCommand}"
                             RefreshCommandParameter="{Binding SelectedCategory}">
    <!-- PullableContent -->
</syncfusion:SfPullToRefresh>
```

```csharp
private string selectedCategory;
public string SelectedCategory
{
    get => selectedCategory;
    set
    {
        selectedCategory = value;
        OnPropertyChanged(nameof(SelectedCategory));
    }
}

public ICommand RefreshCommand { get; }

public ViewModel()
{
    RefreshCommand = new Command<string>(async (category) =>
    {
        await RefreshCategoryAsync(category);
    });
}

private async Task RefreshCategoryAsync(string category)
{
    IsRefreshing = true;
    
    // Refresh based on category
    var items = await apiService.GetItemsByCategoryAsync(category);
    Items.Clear();
    foreach (var item in items)
    {
        Items.Add(item);
    }
    
    IsRefreshing = false;
}
```

## CanExecute Method

The `CanExecute()` method of `RefreshCommand` controls whether the refresh action can proceed.

### Implementing CanExecute

```csharp
using System.Windows.Input;

public class ViewModel : INotifyPropertyChanged
{
    private bool canRefresh = true;
    
    public bool CanRefresh
    {
        get => canRefresh;
        set
        {
            if (canRefresh != value)
            {
                canRefresh = value;
                OnPropertyChanged(nameof(CanRefresh));
                ((Command)RefreshCommand).ChangeCanExecute();
            }
        }
    }
    
    public ICommand RefreshCommand { get; }
    
    public ViewModel()
    {
        RefreshCommand = new Command(
            execute: async () => await RefreshAsync(),
            canExecute: () => CanRefresh
        );
    }
    
    private async Task RefreshAsync()
    {
        IsRefreshing = true;
        CanRefresh = false; // Prevent concurrent refreshes
        
        try
        {
            await LoadDataAsync();
        }
        finally
        {
            IsRefreshing = false;
            CanRefresh = true; // Allow refresh again
        }
    }
}
```

### Canceling Pull Based on Conditions

```csharp
public class ViewModel : INotifyPropertyChanged
{
    private bool isOnline = true;
    
    public bool IsOnline
    {
        get => isOnline;
        set
        {
            isOnline = value;
            OnPropertyChanged(nameof(IsOnline));
            ((Command)RefreshCommand).ChangeCanExecute();
        }
    }
    
    public ICommand RefreshCommand { get; }
    
    public ViewModel()
    {
        RefreshCommand = new Command(
            execute: async () => await RefreshAsync(),
            canExecute: () => IsOnline // Only allow refresh when online
        );
    }
    
    private async Task RefreshAsync()
    {
        if (!IsOnline)
        {
            // This won't execute if CanExecute returns false
            return;
        }
        
        IsRefreshing = true;
        await LoadOnlineDataAsync();
        IsRefreshing = false;
    }
}
```

**Result:** If `CanExecute()` returns `false`, the pulling action is canceled and `RefreshCommand` does not execute.

## Complete MVVM Examples

### Example 1: News Feed with MVVM

```xml
<ContentPage xmlns:syncfusion="clr-namespace:Syncfusion.Maui.PullToRefresh;assembly=Syncfusion.Maui.PullToRefresh"
             xmlns:local="clr-namespace:MyApp">
    
    <ContentPage.BindingContext>
        <local:NewsFeedViewModel />
    </ContentPage.BindingContext>
    
    <Grid>
        <syncfusion:SfPullToRefresh IsRefreshing="{Binding IsRefreshing}"
                                     RefreshCommand="{Binding RefreshCommand}"
                                     TransitionMode="Push">
            <syncfusion:SfPullToRefresh.PullableContent>
                <CollectionView ItemsSource="{Binding NewsArticles}">
                    <CollectionView.ItemTemplate>
                        <DataTemplate>
                            <Frame Margin="10" Padding="15">
                                <StackLayout>
                                    <Label Text="{Binding Title}" 
                                           FontAttributes="Bold"
                                           FontSize="18"/>
                                    <Label Text="{Binding Summary}" 
                                           FontSize="14"
                                           TextColor="Gray"/>
                                    <Label Text="{Binding PublishedDate, StringFormat='{0:MMM dd, yyyy}'}" 
                                           FontSize="12"
                                           TextColor="DarkGray"/>
                                </StackLayout>
                            </Frame>
                        </DataTemplate>
                    </CollectionView.ItemTemplate>
                </CollectionView>
            </syncfusion:SfPullToRefresh.PullableContent>
        </syncfusion:SfPullToRefresh>
        
        <Label Text="{Binding StatusMessage}"
               HorizontalOptions="Center"
               VerticalOptions="End"
               Margin="20"/>
    </Grid>
    
</ContentPage>
```

```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;
using System.Windows.Input;

public class NewsFeedViewModel : INotifyPropertyChanged
{
    private bool isRefreshing;
    private string statusMessage;
    
    public ObservableCollection<NewsArticle> NewsArticles { get; set; }
    
    public bool IsRefreshing
    {
        get => isRefreshing;
        set
        {
            isRefreshing = value;
            OnPropertyChanged(nameof(IsRefreshing));
        }
    }
    
    public string StatusMessage
    {
        get => statusMessage;
        set
        {
            statusMessage = value;
            OnPropertyChanged(nameof(StatusMessage));
        }
    }
    
    public ICommand RefreshCommand { get; }
    
    public NewsFeedViewModel()
    {
        NewsArticles = new ObservableCollection<NewsArticle>();
        RefreshCommand = new Command(async () => await RefreshNewsAsync());
        
        LoadInitialData();
    }
    
    private void LoadInitialData()
    {
        for (int i = 0; i < 10; i++)
        {
            NewsArticles.Add(new NewsArticle
            {
                Title = $"News Article {i}",
                Summary = $"Summary of article {i}",
                PublishedDate = DateTime.Now.AddHours(-i)
            });
        }
        
        StatusMessage = $"Last updated: {DateTime.Now:hh:mm tt}";
    }
    
    private async Task RefreshNewsAsync()
    {
        IsRefreshing = true;
        StatusMessage = "Refreshing...";
        
        try
        {
            // Simulate API call
            await Task.Delay(2000);
            
            // Add new articles at the top
            for (int i = 0; i < 3; i++)
            {
                NewsArticles.Insert(0, new NewsArticle
                {
                    Title = $"Latest News {i}",
                    Summary = $"Breaking news summary {i}",
                    PublishedDate = DateTime.Now
                });
            }
            
            StatusMessage = $"Last updated: {DateTime.Now:hh:mm tt}";
        }
        catch (Exception ex)
        {
            StatusMessage = $"Error: {ex.Message}";
        }
        finally
        {
            IsRefreshing = false;
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}

public class NewsArticle
{
    public string Title { get; set; }
    public string Summary { get; set; }
    public DateTime PublishedDate { get; set; }
}
```

### Example 2: Email Inbox with MVVM

```csharp
public class InboxViewModel : INotifyPropertyChanged
{
    private bool isRefreshing;
    private bool canRefresh = true;
    private int unreadCount;
    
    public ObservableCollection<EmailMessage> Messages { get; set; }
    
    public bool IsRefreshing
    {
        get => isRefreshing;
        set
        {
            isRefreshing = value;
            OnPropertyChanged(nameof(IsRefreshing));
        }
    }
    
    public int UnreadCount
    {
        get => unreadCount;
        set
        {
            unreadCount = value;
            OnPropertyChanged(nameof(UnreadCount));
        }
    }
    
    public ICommand RefreshCommand { get; }
    
    public InboxViewModel()
    {
        Messages = new ObservableCollection<EmailMessage>();
        RefreshCommand = new Command(
            execute: async () => await CheckNewMailAsync(),
            canExecute: () => canRefresh
        );
        
        LoadMessages();
    }
    
    private void LoadMessages()
    {
        // Load initial messages
        for (int i = 0; i < 15; i++)
        {
            Messages.Add(new EmailMessage
            {
                From = $"sender{i}@example.com",
                Subject = $"Email Subject {i}",
                Preview = $"Email preview text {i}...",
                IsUnread = i < 3
            });
        }
        
        UpdateUnreadCount();
    }
    
    private async Task CheckNewMailAsync()
    {
        IsRefreshing = true;
        canRefresh = false;
        ((Command)RefreshCommand).ChangeCanExecute();
        
        try
        {
            // Simulate checking for new mail
            await Task.Delay(1500);
            
            // Add new unread messages
            int newMessages = new Random().Next(0, 5);
            for (int i = 0; i < newMessages; i++)
            {
                Messages.Insert(0, new EmailMessage
                {
                    From = $"newsender{i}@example.com",
                    Subject = $"New Email {i}",
                    Preview = $"New message preview {i}...",
                    IsUnread = true
                });
            }
            
            UpdateUnreadCount();
        }
        finally
        {
            IsRefreshing = false;
            canRefresh = true;
            ((Command)RefreshCommand).ChangeCanExecute();
        }
    }
    
    private void UpdateUnreadCount()
    {
        UnreadCount = Messages.Count(m => m.IsUnread);
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}

public class EmailMessage
{
    public string From { get; set; }
    public string Subject { get; set; }
    public string Preview { get; set; }
    public bool IsUnread { get; set; }
}
```

## Best Practices

### 1. Always Implement INotifyPropertyChanged

Ensure ViewModel implements `INotifyPropertyChanged` for proper data binding:

```csharp
public class BaseViewModel : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
    
    protected bool SetProperty<T>(ref T storage, T value, [CallerMemberName] string propertyName = null)
    {
        if (EqualityComparer<T>.Default.Equals(storage, value))
            return false;
            
        storage = value;
        OnPropertyChanged(propertyName);
        return true;
    }
}
```

### 2. Use Try-Finally for IsRefreshing

Always reset `IsRefreshing` even if errors occur:

```csharp
private async Task RefreshAsync()
{
    IsRefreshing = true;
    
    try
    {
        await LoadDataAsync();
    }
    catch (Exception ex)
    {
        // Handle error
    }
    finally
    {
        IsRefreshing = false; // Always executes
    }
}
```

### 3. Use ObservableCollection

Use `ObservableCollection<T>` for automatic UI updates:

```csharp
public ObservableCollection<Item> Items { get; set; } = new();
```

### 4. Prevent Concurrent Refreshes

Use CanExecute to prevent multiple simultaneous refreshes:

```csharp
private bool canRefresh = true;

RefreshCommand = new Command(
    execute: async () => await RefreshAsync(),
    canExecute: () => canRefresh
);

private async Task RefreshAsync()
{
    canRefresh = false;
    ((Command)RefreshCommand).ChangeCanExecute();
    
    // Refresh logic
    
    canRefresh = true;
    ((Command)RefreshCommand).ChangeCanExecute();
}
```

### 5. Provide User Feedback

Update status messages during refresh:

```csharp
StatusMessage = "Refreshing...";
await RefreshAsync();
StatusMessage = $"Updated: {DateTime.Now:hh:mm tt}";
```

## Troubleshooting

### Issue: Command Not Executing

**Solution:** Ensure command is properly bound and CanExecute returns true:

```csharp
RefreshCommand = new Command(
    execute: async () => await RefreshAsync(),
    canExecute: () => true // Verify this returns true
);
```

### Issue: IsRefreshing Not Updating UI

**Solution:** Implement INotifyPropertyChanged correctly:

```csharp
private bool isRefreshing;
public bool IsRefreshing
{
    get => isRefreshing;
    set
    {
        if (isRefreshing != value)
        {
            isRefreshing = value;
            OnPropertyChanged(nameof(IsRefreshing)); // Must raise event
        }
    }
}
```

### Issue: Items Not Updating in UI

**Solution:** Use ObservableCollection:

```csharp
// Wrong
public List<Item> Items { get; set; } = new();

// Correct
public ObservableCollection<Item> Items { get; set; } = new();
```
