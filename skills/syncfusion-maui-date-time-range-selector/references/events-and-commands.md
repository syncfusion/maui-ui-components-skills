# Events and Commands in DateTime Range Selector

## Table of Contents
- [Overview](#overview)
- [Value Change Events](#value-change-events)
- [Label and Tooltip Events](#label-and-tooltip-events)
- [Commands](#commands)
- [Event Arguments](#event-arguments)
- [MVVM Pattern](#mvvm-pattern)
- [Common Patterns](#common-patterns)

## Overview

The DateTime Range Selector provides comprehensive event support for tracking user interactions and value changes:

**Value Change Events:**
- `ValueChangeStart` - Fired when drag begins
- `ValueChanging` - Fired continuously during drag
- `ValueChanged` - Fired when value changes
- `ValueChangeEnd` - Fired when drag completes

**Customization Events:**
- `LabelCreated` - Customize label text and style
- `TooltipLabelCreated` - Customize tooltip text and style

**Commands (MVVM):**
- `DragStartedCommand` - Command executed when drag starts
- `DragCompletedCommand` - Command executed when drag completes

## Value Change Events

### ValueChangeStart

Fires when the user begins dragging a thumb.

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2018-01-01"
                                 ValueChangeStart="OnValueChangeStart">
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

```csharp
private void OnValueChangeStart(object sender, EventArgs e)
{
    // Show loading indicator
    LoadingIndicator.IsVisible = true;
}
```

**Use Cases:**
- Show loading indicator
- Log interaction start
- Disable other UI elements
- Prepare for data query

### ValueChanging

Fires continuously during thumb drag, before the value is updated.

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2018-01-01"
                                 ValueChanging="OnValueChanging">
    <charts:SfCartesianChart />
</sliders:SfDateTimeRangeSelector>
```

```csharp
private void OnValueChanging(object sender, DateTimeRangeSelectorValueChangingEventArgs e)
{
    var newStart = (DateTime)e.NewRangeStart;
    var newEnd = (DateTime)e.NewRangeEnd;
    
    // Cancel the change if invalid
    if (newEnd.Subtract(newStart).TotalDays < 30)
    {
        e.Cancel = true; // Require minimum 30-day range
    }
}
```

**Use Cases:**
- Validate range before accepting
- Enforce minimum/maximum range size
- Cancel invalid selections
- Preview changes without committing

### ValueChanged

Fires when the range value changes.

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2018-01-01"
                                 ValueChanged="OnValueChanged">
    <charts:SfCartesianChart />
</sliders:SfDateTimeRangeSelector>
```

```csharp
private async void OnValueChanged(object sender, DateTimeRangeSelectorValueChangedEventArgs e)
{
    var start = (DateTime)e.NewRangeStart;
    var end = (DateTime)e.NewRangeEnd;
    
    // Update data based on new range
    await LoadDataForRange(start, end);
}
```

**Behavior:**
- **EnableDeferredUpdate = False**: Fires continuously during drag
- **EnableDeferredUpdate = True**: Fires once after drag completes

**Use Cases:**
- Load data for new range
- Update chart/visualization
- Save selection to preferences
- Trigger analytics

### ValueChangeEnd

Fires when the user releases the thumb after dragging.

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2018-01-01"
                                 ValueChangeEnd="OnValueChangeEnd">
    <charts:SfCartesianChart />
</sliders:SfDateTimeRangeSelector>
```

```csharp
private void OnValueChangeEnd(object sender, EventArgs e)
{
    // Hide loading indicator
    LoadingIndicator.IsVisible = false;
    
    // Log final selection
    LogRangeSelection(rangeSelector.RangeStart, rangeSelector.RangeEnd);
}
```

**Use Cases:**
- Hide loading indicators
- Log completed interaction
- Re-enable UI elements
- Finalize data operations

### Complete Event Flow Example

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2018-01-01"
                                 ValueChangeStart="OnValueChangeStart"
                                 ValueChanging="OnValueChanging"
                                 ValueChanged="OnValueChanged"
                                 ValueChangeEnd="OnValueChangeEnd">
    <charts:SfCartesianChart />
</sliders:SfDateTimeRangeSelector>
```

```csharp
private void OnValueChangeStart(object sender, EventArgs e)
{
    Debug.WriteLine("1. Drag started");
    LoadingIndicator.IsVisible = true;
}

private void OnValueChanging(object sender, DateTimeRangeSelectorValueChangingEventArgs e)
{
    Debug.WriteLine($"2. Changing: {e.NewValue.Start} to {e.NewValue.End}");
    
    // Enforce minimum 1-year range
    var start = (DateTime)e.NewRangeStart;
    var end = (DateTime)e.NewRangeEnd;
    if (end.Subtract(start).TotalDays < 365)
    {
        e.Cancel = true;
    }
}

private async void OnValueChanged(object sender, DateTimeRangeSelectorValueChangedEventArgs e)
{
    Debug.WriteLine($"3. Changed to: {e.NewValue.Start} to {e.NewValue.End}");
    await LoadDataForRange((DateTime)e.NewValue.Start, (DateTime)e.NewValue.End);
}

private void OnValueChangeEnd(object sender, EventArgs e)
{
    Debug.WriteLine("4. Drag completed");
    LoadingIndicator.IsVisible = false;
}
```

## Label and Tooltip Events

### LabelCreated

Customize label text and style for each label on the track.

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01"
                                 Maximum="2011-01-01"
                                 RangeStart="2010-04-01"
                                 RangeEnd="2010-10-01"
                                 Interval="3"
                                 IntervalType="Months"
                                 ShowLabels="True"
                                 LabelCreated="OnLabelCreated">
    <charts:SfCartesianChart />
</sliders:SfDateTimeRangeSelector>
```

```csharp
private void OnLabelCreated(object sender, SliderLabelCreatedEventArgs e)
{
    // Convert month labels to quarters
    DateTime date = DateTime.Parse(e.Text);
    int quarter = (date.Month - 1) / 3 + 1;
    e.Text = $"Q{quarter}";
    
    // Customize style
    e.Style = new SliderLabelStyle()
    {
        ActiveTextColor = Colors.Blue,
        InactiveTextColor = Colors.Gray,
        ActiveFontSize = 14
    };
}
```

**See:** [labels.md](labels.md#custom-label-text) for more examples.

### TooltipLabelCreated

Customize tooltip text and style when thumbs are dragged.

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01"
                                 Maximum="2020-01-01"
                                 RangeStart="2012-01-01"
                                 RangeEnd="2018-01-01"
                                 TooltipLabelCreated="OnTooltipLabelCreated">
    <charts:SfCartesianChart />
</sliders:SfDateTimeRangeSelector>
```

```csharp
private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    DateTime date = (DateTime)e.Value;
    e.Text = $"Year: {date.Year}";
    
    // Custom style based on value
    if (date.Year < 2015)
    {
        e.Style = new SliderTooltipStyle()
        {
            Fill = new SolidColorBrush(Colors.Red),
            TextColor = Colors.White
        };
    }
    else
    {
        e.Style = new SliderTooltipStyle()
        {
            Fill = new SolidColorBrush(Colors.Green),
            TextColor = Colors.White
        };
    }
}
```

**See:** [tooltip.md](tooltip.md#custom-tooltip-text) for more examples.

## Commands

Commands provide MVVM support for thumb drag interactions.

### DragStartedCommand

Executes when user begins dragging a thumb.

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2018-01-01"
                                 DragStartedCommand="{Binding DragStartedCommand}">
    <charts:SfCartesianChart />
</sliders:SfDateTimeRangeSelector>
```

**ViewModel:**
```csharp
public class RangeSelectorViewModel : INotifyPropertyChanged
{
    public ICommand DragStartedCommand { get; set; }
    
    public RangeSelectorViewModel()
    {
        DragStartedCommand = new Command(OnDragStarted);
    }
    
    private void OnDragStarted()
    {
        IsLoading = true;
        Debug.WriteLine("Drag started via command");
    }
    
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
    
    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### DragCompletedCommand

Executes when user releases the thumb after dragging.

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="{Binding RangeStart, Mode=TwoWay}" 
                                 RangeEnd="{Binding RangeEnd, Mode=TwoWay}"
                                 DragCompletedCommand="{Binding DragCompletedCommand}">
    <charts:SfCartesianChart />
</sliders:SfDateTimeRangeSelector>
```

**ViewModel:**
```csharp
public class RangeSelectorViewModel : INotifyPropertyChanged
{
    public ICommand DragCompletedCommand { get; set; }
    
    private DateTime _rangeStart;
    public DateTime RangeStart
    {
        get => _rangeStart;
        set
        {
            _rangeStart = value;
            OnPropertyChanged(nameof(RangeStart));
        }
    }
    
    private DateTime _rangeEnd;
    public DateTime RangeEnd
    {
        get => _rangeEnd;
        set
        {
            _rangeEnd = value;
            OnPropertyChanged(nameof(RangeEnd));
        }
    }
    
    public RangeSelectorViewModel()
    {
        RangeStart = new DateTime(2012, 1, 1);
        RangeEnd = new DateTime(2018, 1, 1);
        
        DragCompletedCommand = new Command(async () => await OnDragCompleted());
    }
    
    private async Task OnDragCompleted()
    {
        IsLoading = false;
        await LoadDataForRange(RangeStart, RangeEnd);
        Debug.WriteLine($"Drag completed: {RangeStart:yyyy-MM-dd} to {RangeEnd:yyyy-MM-dd}");
    }
    
    private async Task LoadDataForRange(DateTime start, DateTime end)
    {
        // Load data logic
        await Task.Delay(500); // Simulate data load
    }
    
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
    
    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

## Event Arguments

### DateTimeRangeSelectorValueChangingEventArgs

Used in `ValueChanging` event.

| Property | Type | Description |
|----------|------|-------------|
| `OldValue` | SliderRange | Previous range (Start, End) |
| `NewValue` | SliderRange | Proposed new range (Start, End) |
| `Cancel` | bool | Set to true to prevent value change |

**Example:**
```csharp
private void OnValueChanging(object sender, DateTimeRangeSelectorValueChangingEventArgs e)
{
    var newStart = (DateTime)e.NewRangeStart;
    var newEnd = (DateTime)e.NewRangeEnd;
    
    // Access range duration
    TimeSpan oldDuration = oldEnd - oldStart;
    TimeSpan newDuration = newEnd - newStart;
    
    // Cancel if new range is too small
    if (newDuration.TotalDays < 30)
    {
        e.Cancel = true;
    }
}
```

### SliderValueChangedEventArgs

Used in `ValueChanged` event.

| Property | Type | Description |
|----------|------|-------------|
| `OldValue` | SliderRange | Previous range (Start, End) |
| `NewValue` | SliderRange | New range (Start, End) |

**Example:**
```csharp
private void OnValueChanged(object sender, DateTimeRangeSelectorValueChangedEventArgs e)
{
    var start = (DateTime)e.NewRangeStart;
    var end = (DateTime)e.NewRangeEnd;
    
    Debug.WriteLine($"Range changed from {start} to {end}");
}
```

### SliderLabelCreatedEventArgs

Used in `LabelCreated` event.

| Property | Type | Description |
|----------|------|-------------|
| `Text` | string | Label text to display |
| `Style` | SliderLabelStyle | Label style (colors, fonts, offset) |

### SliderTooltipLabelCreatedEventArgs

Used in `TooltipLabelCreated` event.

| Property | Type | Description |
|----------|------|-------------|
| `Value` | object | Current DateTime value |
| `Text` | string | Tooltip text to display |
| `Style` | SliderTooltipStyle | Tooltip style (fill, text color, fonts) |

## MVVM Pattern

Complete MVVM example with two-way binding and commands:

**XAML:**
```xaml
<ContentPage xmlns:sliders="clr-namespace:Syncfusion.Maui.Sliders;assembly=Syncfusion.Maui.Sliders"
             xmlns:charts="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts"
             xmlns:local="clr-namespace:YourNamespace"
             x:Class="YourNamespace.RangeSelectorPage">
    <ContentPage.BindingContext>
        <local:RangeSelectorViewModel />
    </ContentPage.BindingContext>
    
    <Grid>
        <sliders:SfDateTimeRangeSelector Minimum="{Binding Minimum}" 
                                         Maximum="{Binding Maximum}" 
                                         RangeStart="{Binding RangeStart, Mode=TwoWay}" 
                                         RangeEnd="{Binding RangeEnd, Mode=TwoWay}"
                                         Interval="{Binding Interval}"
                                         ShowLabels="True"
                                         ShowTicks="True"
                                         
                                         EnableDeferredUpdate="True"
                                         DragStartedCommand="{Binding DragStartedCommand}"
                                         DragCompletedCommand="{Binding DragCompletedCommand}">
            <charts:SfCartesianChart>
                <!-- Chart content -->
            </charts:SfCartesianChart>
        </sliders:SfDateTimeRangeSelector>
        
        <ActivityIndicator IsRunning="{Binding IsLoading}" 
                          IsVisible="{Binding IsLoading}"
                          VerticalOptions="Center"
                          HorizontalOptions="Center" />
    </Grid>
</ContentPage>
```

**ViewModel:**
```csharp
public class RangeSelectorViewModel : INotifyPropertyChanged
{
    private DateTime _minimum;
    public DateTime Minimum
    {
        get => _minimum;
        set { _minimum = value; OnPropertyChanged(nameof(Minimum)); }
    }
    
    private DateTime _maximum;
    public DateTime Maximum
    {
        get => _maximum;
        set { _maximum = value; OnPropertyChanged(nameof(Maximum)); }
    }
    
    private DateTime _rangeStart;
    public DateTime RangeStart
    {
        get => _rangeStart;
        set { _rangeStart = value; OnPropertyChanged(nameof(RangeStart)); }
    }
    
    private DateTime _rangeEnd;
    public DateTime RangeEnd
    {
        get => _rangeEnd;
        set { _rangeEnd = value; OnPropertyChanged(nameof(RangeEnd)); }
    }
    
    private int _interval;
    public int Interval
    {
        get => _interval;
        set { _interval = value; OnPropertyChanged(nameof(Interval)); }
    }
    
    private bool _isLoading;
    public bool IsLoading
    {
        get => _isLoading;
        set { _isLoading = value; OnPropertyChanged(nameof(IsLoading)); }
    }
    
    public ICommand DragStartedCommand { get; }
    public ICommand DragCompletedCommand { get; }
    
    public RangeSelectorViewModel()
    {
        // Initialize values
        Minimum = new DateTime(2010, 1, 1);
        Maximum = new DateTime(2020, 1, 1);
        RangeStart = new DateTime(2012, 1, 1);
        RangeEnd = new DateTime(2018, 1, 1);
        Interval = 2;
        
        // Initialize commands
        DragStartedCommand = new Command(OnDragStarted);
        DragCompletedCommand = new Command(async () => await OnDragCompletedAsync());
    }
    
    private void OnDragStarted()
    {
        IsLoading = true;
    }
    
    private async Task OnDragCompletedAsync()
    {
        // Load data for new range
        await LoadDataAsync(RangeStart, RangeEnd);
        IsLoading = false;
    }
    
    private async Task LoadDataAsync(DateTime start, DateTime end)
    {
        // Simulate data loading
        await Task.Delay(1000);
        Debug.WriteLine($"Data loaded for {start:yyyy-MM-dd} to {end:yyyy-MM-dd}");
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

## Common Patterns

### Pattern 1: Validate Minimum Range

```csharp
private void OnValueChanging(object sender, DateTimeRangeSelectorValueChangingEventArgs e)
{
    var start = (DateTime)e.NewRangeStart;
    var end = (DateTime)e.NewRangeEnd;
    
    // Require minimum 30-day range
    if (end.Subtract(start).TotalDays < 30)
    {
        e.Cancel = true;
        DisplayAlert("Invalid Range", "Please select at least 30 days", "OK");
    }
}
```

### Pattern 2: Validate Maximum Range

```csharp
private void OnValueChanging(object sender, DateTimeRangeSelectorValueChangingEventArgs e)
{
    var start = (DateTime)e.NewRangeStart;
    var end = (DateTime)e.NewRangeEnd;
    
    // Limit to maximum 2-year range
    if (end.Subtract(start).TotalDays > 730)
    {
        e.Cancel = true;
        DisplayAlert("Invalid Range", "Range cannot exceed 2 years", "OK");
    }
}
```

### Pattern 3: Deferred Data Loading

```xaml
<sliders:SfDateTimeRangeSelector EnableDeferredUpdate="True"
                                 ValueChanged="OnValueChanged">
    <charts:SfCartesianChart />
</sliders:SfDateTimeRangeSelector>
```

```csharp
private async void OnValueChanged(object sender, DateTimeRangeSelectorValueChangedEventArgs e)
{
    var start = (DateTime)e.NewRangeStart;
    var end = (DateTime)e.NewRangeEnd;
    
    // This only fires once when drag completes
    await LoadDataFromDatabase(start, end);
}
```

### Pattern 4: Real-time Preview (No Deferred Update)

```xaml
<sliders:SfDateTimeRangeSelector EnableDeferredUpdate="False"
                                 ValueChanged="OnValueChanged">
    <charts:SfCartesianChart />
</sliders:SfDateTimeRangeSelector>
```

```csharp
private void OnValueChanged(object sender, DateTimeRangeSelectorValueChangedEventArgs e)
{
    var start = (DateTime)e.NewRangeStart;
    var end = (DateTime)e.NewRangeEnd;
    
    // Update preview immediately (use cached data)
    UpdateChartPreview(start, end);
}
```

### Pattern 5: Analytics Tracking

```csharp
private void OnValueChangeEnd(object sender, EventArgs e)
{
    var start = rangeSelector.RangeStart;
    var end = rangeSelector.RangeEnd;
    var duration = end.Subtract(start);
    
    // Log to analytics
    Analytics.TrackEvent("RangeSelected", new Dictionary<string, string>
    {
        { "Start", start.ToString("yyyy-MM-dd") },
        { "End", end.ToString("yyyy-MM-dd") },
        { "DurationDays", duration.TotalDays.ToString("F0") }
    });
}
```

### Pattern 6: Prevent Overlapping with Busy Periods

```csharp
private readonly DateTime[] _busyDates = 
{
    new DateTime(2023, 12, 25), // Christmas
    new DateTime(2024, 1, 1)    // New Year
};

private void OnValueChanging(object sender, DateTimeRangeSelectorValueChangingEventArgs e)
{
    var start = (DateTime)e.NewRangeStart;
    var end = (DateTime)e.NewRangeEnd;
    
    // Check if range includes busy dates
    foreach (var busyDate in _busyDates)
    {
        if (busyDate >= start && busyDate <= end)
        {
            e.Cancel = true;
            DisplayAlert("Unavailable", $"Range cannot include {busyDate:MMM d}", "OK");
            break;
        }
    }
}
```

## Best Practices

1. **Use EnableDeferredUpdate**: Always enable for database queries or API calls
2. **Validate in ValueChanging**: Prevent invalid selections before they happen
3. **Load Data in ValueChanged**: Trigger data loading when range changes
4. **Show Indicators**: Use ValueChangeStart/End to show/hide loading indicators
5. **Use Commands for MVVM**: Prefer commands over events in MVVM architecture
6. **Cancel Invalid Ranges**: Set `e.Cancel = true` in ValueChanging for validation
7. **Avoid Heavy Operations in ValueChanging**: It fires frequently during drag

## Troubleshooting

**Issue:** ValueChanged fires too many times during drag
- **Solution:** Set `EnableDeferredUpdate="True"` to fire only on drag completion

**Issue:** Validation in ValueChanged doesn't prevent invalid selection
- **Solution:** Move validation to `ValueChanging` event and use `e.Cancel = true`

**Issue:** Commands not firing
- **Solution:** Verify BindingContext is set and command properties use `{Binding CommandName}`

**Issue:** Data loads multiple times unnecessarily
- **Solution:** Use `EnableDeferredUpdate="True"` and load data in ValueChanged or DragCompletedCommand

**Issue:** Can't access updated RangeStart/RangeEnd in events
- **Solution:** Use `e.NewValue.Start` and `e.NewValue.End` from event arguments

**Issue:** LabelCreated event not firing
- **Solution:** Ensure `ShowLabels="True"` and `Interval` is set

## Related Properties

- `EnableDeferredUpdate` - Controls when ValueChanged fires
- `RangeStart` / `RangeEnd` - Bindable properties for MVVM
- `ShowLabels` - Required for LabelCreated event
- `ShowTooltip` - Required for TooltipLabelCreated event
