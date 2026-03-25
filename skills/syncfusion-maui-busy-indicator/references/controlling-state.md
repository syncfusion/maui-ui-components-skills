# Controlling Animation State

The `IsRunning` property controls whether the Busy Indicator animation is active. This guide covers starting, stopping, and managing the indicator's visibility and state.

## IsRunning Property Overview

The `IsRunning` property is a boolean that determines whether the busy indicator is visible and animating:

- `true` - Indicator is visible and animating
- `false` - Indicator is hidden and not animating (default)

**Default Value:** `false`

## Starting the Animation

### Basic Usage

**XAML:**
```xml
<core:SfBusyIndicator x:Name="busyIndicator"
                      AnimationType="CircularMaterial"
                      IsRunning="True" />
```

**C#:**
```csharp
SfBusyIndicator busyIndicator = new SfBusyIndicator
{
    AnimationType = AnimationType.CircularMaterial,
    IsRunning = true
};
```

### Starting Programmatically

```csharp
// Start the animation
busyIndicator.IsRunning = true;
```

## Stopping the Animation

### Stopping Programmatically

```csharp
// Stop the animation
busyIndicator.IsRunning = false;
```

### Complete Start/Stop Example

```csharp
private async void OnLoadDataClicked(object sender, EventArgs e)
{
    // Start animation
    busyIndicator.IsRunning = true;
    
    try
    {
        // Perform long-running operation
        await LoadDataAsync();
    }
    finally
    {
        // Always stop animation, even if an error occurs
        busyIndicator.IsRunning = false;
    }
}
```

## Default Behavior

When `IsRunning` is `false`:
- The indicator is completely hidden from view
- No animation is running
- The control does not block interaction with underlying UI
- No resources are consumed by the animation

**Important:** The default value of `IsRunning` is `false`, so the indicator will not appear unless explicitly set to `true`.

```xml
<!-- This indicator will NOT be visible on page load -->
<core:SfBusyIndicator AnimationType="CircularMaterial" />

<!-- This indicator WILL be visible on page load -->
<core:SfBusyIndicator AnimationType="CircularMaterial" IsRunning="True" />
```

## Dynamic State Control

### Toggle State

```csharp
private void OnToggleClicked(object sender, EventArgs e)
{
    busyIndicator.IsRunning = !busyIndicator.IsRunning;
}
```

### Conditional State

```csharp
public void SetBusyState(bool isBusy)
{
    busyIndicator.IsRunning = isBusy;
    
    // Optionally disable other UI elements
    submitButton.IsEnabled = !isBusy;
    cancelButton.IsEnabled = !isBusy;
}
```

## Binding to ViewModel

### Basic ViewModel Binding

```csharp
public class DataViewModel : INotifyPropertyChanged
{
    private bool _isLoading;
    
    public bool IsLoading
    {
        get => _isLoading;
        set
        {
            _isLoading = value;
            OnPropertyChanged(nameof(IsLoading));
        }
    }
    
    public async Task LoadDataAsync()
    {
        IsLoading = true;
        
        try
        {
            await FetchDataFromServerAsync();
        }
        finally
        {
            IsLoading = false;
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected virtual void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**XAML:**
```xml
<core:SfBusyIndicator IsRunning="{Binding IsLoading}"
                      AnimationType="CircularMaterial"
                      Title="Loading data..." />
```

### Multiple Operations

```csharp
public class MainViewModel : INotifyPropertyChanged
{
    private bool _isLoadingUsers;
    private bool _isLoadingSettings;
    
    public bool IsLoadingUsers
    {
        get => _isLoadingUsers;
        set { _isLoadingUsers = value; OnPropertyChanged(nameof(IsLoadingUsers)); }
    }
    
    public bool IsLoadingSettings
    {
        get => _isLoadingSettings;
        set { _isLoadingSettings = value; OnPropertyChanged(nameof(IsLoadingSettings)); }
    }
    
    // Combined property for global loading indicator
    public bool IsAnyLoading => IsLoadingUsers || IsLoadingSettings;
}
```

**XAML:**
```xml
<!-- Show when ANY operation is running -->
<core:SfBusyIndicator IsRunning="{Binding IsAnyLoading}" />

<!-- Separate indicators for different sections -->
<core:SfBusyIndicator IsRunning="{Binding IsLoadingUsers}" />
<core:SfBusyIndicator IsRunning="{Binding IsLoadingSettings}" />
```

## Event-Driven State Management

### Button Click Events

```csharp
private async void OnLoginClicked(object sender, EventArgs e)
{
    busyIndicator.IsRunning = true;
    
    try
    {
        var success = await AuthService.LoginAsync(username, password);
        
        if (success)
        {
            await Navigation.PushAsync(new HomePage());
        }
        else
        {
            await DisplayAlert("Error", "Invalid credentials", "OK");
        }
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", ex.Message, "OK");
    }
    finally
    {
        busyIndicator.IsRunning = false;
    }
}
```

### Page Lifecycle Events

```csharp
protected override async void OnAppearing()
{
    base.OnAppearing();
    
    busyIndicator.IsRunning = true;
    
    try
    {
        await LoadPageDataAsync();
    }
    finally
    {
        busyIndicator.IsRunning = false;
    }
}
```

### Pull-to-Refresh

```csharp
private async void OnRefreshing(object sender, EventArgs e)
{
    busyIndicator.IsRunning = true;
    
    try
    {
        await RefreshDataAsync();
    }
    finally
    {
        busyIndicator.IsRunning = false;
        refreshView.IsRefreshing = false;
    }
}
```

## Common Patterns

### Pattern 1: Form Submission

```xml
<StackLayout>
    <Entry Placeholder="Username" />
    <Entry Placeholder="Password" IsPassword="True" />
    <Button Text="Submit" Clicked="OnSubmitClicked" />
    
    <core:SfBusyIndicator x:Name="busyIndicator"
                          IsRunning="False"
                          AnimationType="CircularMaterial"
                          Title="Submitting..."
                          OverlayFill="#88000000" />
</StackLayout>
```

```csharp
private async void OnSubmitClicked(object sender, EventArgs e)
{
    busyIndicator.IsRunning = true;
    
    try
    {
        await SubmitFormAsync();
        await DisplayAlert("Success", "Form submitted", "OK");
    }
    finally
    {
        busyIndicator.IsRunning = false;
    }
}
```

### Pattern 2: Search with Debounce

```csharp
private CancellationTokenSource _searchCts;

private async void OnSearchTextChanged(object sender, TextChangedEventArgs e)
{
    // Cancel previous search
    _searchCts?.Cancel();
    _searchCts = new CancellationTokenSource();
    
    try
    {
        // Debounce delay
        await Task.Delay(300, _searchCts.Token);
        
        // Start loading
        busyIndicator.IsRunning = true;
        
        // Perform search
        await SearchAsync(e.NewTextValue, _searchCts.Token);
    }
    catch (TaskCanceledException)
    {
        // Search was cancelled, ignore
    }
    finally
    {
        busyIndicator.IsRunning = false;
    }
}
```

### Pattern 3: Sequential Operations

```csharp
public async Task ProcessWorkflowAsync()
{
    busyIndicator.IsRunning = true;
    busyIndicator.Title = "Initializing...";
    
    try
    {
        await InitializeAsync();
        
        busyIndicator.Title = "Processing data...";
        await ProcessDataAsync();
        
        busyIndicator.Title = "Saving results...";
        await SaveResultsAsync();
        
        busyIndicator.Title = "Finalizing...";
        await FinalizeAsync();
    }
    finally
    {
        busyIndicator.IsRunning = false;
    }
}
```

### Pattern 4: Timeout Handling

```csharp
public async Task LoadWithTimeoutAsync()
{
    busyIndicator.IsRunning = true;
    
    try
    {
        using var cts = new CancellationTokenSource(TimeSpan.FromSeconds(30));
        await LoadDataAsync(cts.Token);
    }
    catch (OperationCanceledException)
    {
        await DisplayAlert("Timeout", "Operation took too long", "OK");
    }
    finally
    {
        busyIndicator.IsRunning = false;
    }
}
```

## Best Practices

### Always Use Try-Finally

Ensure the indicator is always stopped, even when errors occur:

```csharp
// ✓ Good
busyIndicator.IsRunning = true;
try
{
    await DoWorkAsync();
}
finally
{
    busyIndicator.IsRunning = false;
}

// ✗ Bad - indicator stays visible if error occurs
busyIndicator.IsRunning = true;
await DoWorkAsync();
busyIndicator.IsRunning = false;
```

### Provide User Feedback

Don't leave users wondering what's happening:

```csharp
// ✓ Good - title explains what's happening
busyIndicator.Title = "Loading user data...";
busyIndicator.IsRunning = true;

// ✗ Bad - no context
busyIndicator.IsRunning = true;
```

### Avoid Very Short Displays

If an operation completes very quickly, the indicator may "flash" on screen:

```csharp
public async Task LoadDataAsync()
{
    var startTime = DateTime.Now;
    busyIndicator.IsRunning = true;
    
    try
    {
        await FetchDataAsync();
        
        // Ensure indicator shows for at least 500ms
        var elapsed = DateTime.Now - startTime;
        var minimumDelay = TimeSpan.FromMilliseconds(500);
        
        if (elapsed < minimumDelay)
        {
            await Task.Delay(minimumDelay - elapsed);
        }
    }
    finally
    {
        busyIndicator.IsRunning = false;
    }
}
```

### Disable Interaction During Loading

Prevent users from triggering operations while loading:

```csharp
private async void OnLoadClicked(object sender, EventArgs e)
{
    loadButton.IsEnabled = false;
    busyIndicator.IsRunning = true;
    
    try
    {
        await LoadDataAsync();
    }
    finally
    {
        busyIndicator.IsRunning = false;
        loadButton.IsEnabled = true;
    }
}
```

## Troubleshooting

### Issue: Indicator Never Appears

**Cause:** `IsRunning` is not set to `true`

**Solution:**
```csharp
// Ensure IsRunning is explicitly set
busyIndicator.IsRunning = true;
```

### Issue: Indicator Never Disappears

**Cause:** `IsRunning` is not set back to `false`, or an exception prevented the finally block

**Solution:**
```csharp
// Always use try-finally
try
{
    await DoWorkAsync();
}
finally
{
    busyIndicator.IsRunning = false;
}
```

### Issue: Indicator Briefly Flashes

**Cause:** Operation completes too quickly

**Solution:**
```csharp
// Add minimum display time
var minDelay = Task.Delay(500);
var dataTask = LoadDataAsync();
await Task.WhenAll(minDelay, dataTask);
```

### Issue: Binding Not Working

**Cause:** ViewModel doesn't implement `INotifyPropertyChanged`

**Solution:**
```csharp
public class ViewModel : INotifyPropertyChanged
{
    private bool _isLoading;
    
    public bool IsLoading
    {
        get => _isLoading;
        set
        {
            _isLoading = value;
            OnPropertyChanged(nameof(IsLoading)); // Critical!
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

## Summary

The `IsRunning` property provides simple, straightforward control over the Busy Indicator's visibility and animation state. Follow these key principles:

1. **Default is false** - Indicator is hidden unless explicitly shown
2. **Always use try-finally** - Ensure indicator is stopped even on errors
3. **Bind to ViewModels** - Use data binding for clean, maintainable code
4. **Provide context** - Update titles to explain what's happening
5. **Prevent interaction** - Disable UI elements during loading operations
6. **Handle edge cases** - Consider timeouts, cancellation, and minimum display times
