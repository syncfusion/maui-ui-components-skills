# Events

This guide covers event handling for user interactions with the Segmented Control.

## Overview

The SfSegmentedControl provides two main events to respond to user interactions:
- **SelectionChanged** - Fires when the selected segment changes
- **SegmentTapped** - Fires whenever any segment is tapped (selected or not)

Use these events to update UI, trigger navigation, filter data, or perform other actions based on user selections.

## SelectionChanged Event

The `SelectionChanged` event fires when the user selects a different segment or when the `SelectedIndex` property changes programmatically.

### Event Arguments

`SelectionChangedEventArgs` provides the following properties:

| Property | Type | Description |
|----------|------|-------------|
| **OldIndex** | int | Previously selected segment index (-1 if none) |
| **NewIndex** | int | Currently selected segment index (-1 if deselected) |
| **OldValue** | object | Previously selected item (string or SfSegmentItem) |
| **NewValue** | object | Currently selected item (string or SfSegmentItem) |

### Basic Usage

**XAML:**
```xml
<buttons:SfSegmentedControl SelectionChanged="OnSelectionChanged">
    <buttons:SfSegmentedControl.ItemsSource>
        <x:Array Type="{x:Type x:String}">
            <x:String>Day</x:String>
            <x:String>Week</x:String>
            <x:String>Month</x:String>
            <x:String>Year</x:String>
        </x:Array>
    </buttons:SfSegmentedControl.ItemsSource>
</buttons:SfSegmentedControl>
```

**C# Event Handler:**
```csharp
private void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var oldIndex = e.OldIndex;      // Previously selected index
    var newIndex = e.NewIndex;      // Currently selected index
    var oldValue = e.OldValue;      // Previous item (string or SfSegmentItem)
    var newValue = e.NewValue;      // Current item (string or SfSegmentItem)
    
    // Access as string (if using string array)
    string selectedPeriod = newValue?.ToString();
    
    // Update UI or perform actions
    Console.WriteLine($"Selection changed from {oldValue} to {newValue}");
}
```

### Attaching in Code

```csharp
var segmentedControl = new SfSegmentedControl
{
    ItemsSource = new List<string> { "Day", "Week", "Month", "Year" }
};

segmentedControl.SelectionChanged += OnSelectionChanged;
```

### Accessing Selected Item Details

When using `SfSegmentItem` objects, cast the `NewValue` to access item properties:

```csharp
private void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (e.NewValue is SfSegmentItem segmentItem)
    {
        string text = segmentItem.Text;
        ImageSource icon = segmentItem.ImageSource;
        bool isEnabled = segmentItem.IsEnabled;
        
        Console.WriteLine($"Selected: {text}");
    }
}
```

## SegmentTapped Event

The `SegmentTapped` event fires every time a segment is tapped, regardless of whether it's already selected.

### Event Arguments

`SegmentTappedEventArgs` provides:

| Property | Type | Description |
|----------|------|-------------|
| **SegmentItem** | SfSegmentItem | The tapped segment item |

### Basic Usage

**XAML:**
```xml
<buttons:SfSegmentedControl SegmentTapped="OnSegmentTapped">
    <buttons:SfSegmentedControl.ItemsSource>
        <x:Array Type="{x:Type x:String}">
            <x:String>Home</x:String>
            <x:String>Search</x:String>
            <x:String>Profile</x:String>
        </x:Array>
    </buttons:SfSegmentedControl.ItemsSource>
</buttons:SfSegmentedControl>
```

**C# Event Handler:**
```csharp
private void OnSegmentTapped(object sender, SegmentTappedEventArgs e)
{
    var tappedItem = e.SegmentItem;
    
    if (tappedItem != null)
    {
        string text = tappedItem.Text;
        Console.WriteLine($"Tapped: {text}");
    }
}
```

### Difference from SelectionChanged

| Event | Fires When | Use Case |
|-------|-----------|----------|
| **SelectionChanged** | Selection changes (different segment selected) | Update content views, filter data, change state |
| **SegmentTapped** | Any segment tapped (including already selected) | Analytics, feedback, double-tap actions |

**Example:**
```csharp
// User taps "Day" (not selected) → Both SegmentTapped and SelectionChanged fire
// User taps "Day" again (already selected) → Only SegmentTapped fires
```

## Common Patterns

### Updating Content Based on Selection

```csharp
private void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (e.NewValue == null) return;
    
    string selectedView = e.NewValue.ToString();
    
    switch (selectedView)
    {
        case "Day":
            contentView.Content = new DayView();
            break;
        case "Week":
            contentView.Content = new WeekView();
            break;
        case "Month":
            contentView.Content = new MonthView();
            break;
        case "Year":
            contentView.Content = new YearView();
            break;
    }
}
```

### Filtering Data

```csharp
public partial class MainPage : ContentPage
{
    private ObservableCollection<DataItem> allData;
    private ObservableCollection<DataItem> filteredData;
    
    private void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
    {
        string filter = e.NewValue?.ToString();
        
        if (filter == "All")
        {
            filteredData = new ObservableCollection<DataItem>(allData);
        }
        else
        {
            filteredData = new ObservableCollection<DataItem>(
                allData.Where(item => item.Category == filter)
            );
        }
        
        dataListView.ItemsSource = filteredData;
    }
}
```

### Navigation

```csharp
private async void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    string destination = e.NewValue?.ToString();
    
    switch (destination)
    {
        case "Home":
            await Navigation.PushAsync(new HomePage());
            break;
        case "Search":
            await Navigation.PushAsync(new SearchPage());
            break;
        case "Profile":
            await Navigation.PushAsync(new ProfilePage());
            break;
    }
}
```

### Loading Data Asynchronously

```csharp
private CancellationTokenSource cancellationTokenSource;

private async void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    // Cancel previous load if still running
    cancellationTokenSource?.Cancel();
    cancellationTokenSource = new CancellationTokenSource();
    
    string period = e.NewValue?.ToString();
    
    try
    {
        loadingIndicator.IsVisible = true;
        
        var data = await LoadDataForPeriod(period, cancellationTokenSource.Token);
        
        chartView.ItemsSource = data;
    }
    catch (OperationCanceledException)
    {
        // Load was cancelled, ignore
    }
    finally
    {
        loadingIndicator.IsVisible = false;
    }
}

private async Task<List<DataPoint>> LoadDataForPeriod(string period, CancellationToken token)
{
    // Simulate API call
    await Task.Delay(1000, token);
    return new List<DataPoint>(); // Return actual data
}
```

### Analytics and Tracking

```csharp
private void OnSegmentTapped(object sender, SegmentTappedEventArgs e)
{
    var tappedItem = e.SegmentItem;
    
    // Track every tap for analytics
    Analytics.TrackEvent("SegmentTapped", new Dictionary<string, string>
    {
        { "Segment", tappedItem.Text },
        { "Timestamp", DateTime.Now.ToString() }
    });
}

private void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    // Track only selection changes
    Analytics.TrackEvent("SegmentSelectionChanged", new Dictionary<string, string>
    {
        { "From", e.OldValue?.ToString() ?? "None" },
        { "To", e.NewValue?.ToString() ?? "None" }
    });
}
```

### Confirming Selection Change

```csharp
private int previousIndex = 0;

private async void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var segmentedControl = (SfSegmentedControl)sender;
    
    bool confirmed = await DisplayAlert(
        "Change View?", 
        $"Switch from {e.OldValue} to {e.NewValue}?", 
        "Yes", "No"
    );
    
    if (!confirmed)
    {
        // Revert selection
        segmentedControl.SelectedIndex = previousIndex;
        return;
    }
    
    previousIndex = e.NewIndex;
    
    // Proceed with selection change
    UpdateView(e.NewValue?.ToString());
}
```

### Handling Deselection (SingleDeselect Mode)

```csharp
private void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (e.NewIndex == -1)
    {
        // Segment was deselected
        contentView.Content = new DefaultView();
        return;
    }
    
    // Normal selection handling
    UpdateView(e.NewValue?.ToString());
}
```

## Best Practices

### Event Handler Efficiency

- Keep event handlers lightweight and fast
- Avoid heavy computations in event handlers
- Use async/await for long-running operations
- Implement cancellation for async operations

### Null Checking

Always check for null values before accessing event arguments:

```csharp
private void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (e.NewValue == null)
    {
        // Handle deselection or initialization
        return;
    }
    
    // Safe to proceed
    string selectedItem = e.NewValue.ToString();
}
```

### Avoid Circular Updates

Prevent infinite loops when updating SelectedIndex programmatically:

```csharp
private bool isUpdatingSelection = false;

private void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (isUpdatingSelection) return;
    
    try
    {
        isUpdatingSelection = true;
        
        // Perform actions that might change SelectedIndex
        UpdateRelatedControls(e.NewIndex);
    }
    finally
    {
        isUpdatingSelection = false;
    }
}
```

### Unsubscribe When Disposing

Prevent memory leaks by unsubscribing from events:

```csharp
protected override void OnDisappearing()
{
    base.OnDisappearing();
    
    segmentedControl.SelectionChanged -= OnSelectionChanged;
    segmentedControl.SegmentTapped -= OnSegmentTapped;
}
```

## Troubleshooting

### Event Not Firing

**Cause:** Event not wired up or control not initialized  
**Solution:** Verify event handler is attached in XAML or code-behind

### Event Fires Multiple Times

**Cause:** Event handler attached multiple times  
**Solution:** Unsubscribe before subscribing:
```csharp
segmentedControl.SelectionChanged -= OnSelectionChanged;
segmentedControl.SelectionChanged += OnSelectionChanged;
```

### NewValue is Null

**Cause:** Deselection in SingleDeselect mode or initialization  
**Solution:** Check for null before accessing:
```csharp
if (e.NewValue != null)
{
    string value = e.NewValue.ToString();
}
```

### Cannot Cast NewValue

**Cause:** Attempting to cast string to SfSegmentItem (or vice versa)  
**Solution:** Check the type before casting:
```csharp
if (e.NewValue is string stringValue)
{
    // Handle string
}
else if (e.NewValue is SfSegmentItem segmentItem)
{
    // Handle SfSegmentItem
}
```

### SelectionChanged Fires on Initialization

**Cause:** SelectedIndex defaults to 0, triggering event  
**Solution:** Set a flag to ignore initial event or handle gracefully:
```csharp
private bool isInitialized = false;

private void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (!isInitialized)
    {
        isInitialized = true;
        return;
    }
    
    // Handle subsequent changes
}
```

## Next Steps

- **Selection features:** See [selection.md](selection.md) for selection modes and indicators
- **Customization:** See [customization.md](customization.md) for styling based on selection
- **Layout:** See [layout.md](layout.md) for sizing and scrolling configuration
