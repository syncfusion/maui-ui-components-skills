# Events and Commands in .NET MAUI Range Slider

## Table of Contents
- [Overview](#overview)
- [Value Change Events](#value-change-events)
  - [ValueChangeStart](#valuechangestart)
  - [ValueChanging](#valuechanging)
  - [ValueChanged](#valuechanged)
  - [ValueChangeEnd](#valuechangeend)
- [Customization Events](#customization-events)
  - [LabelCreated](#labelcreated)
  - [TooltipLabelCreated](#tooltiplabelcreated)
- [Commands](#commands)
  - [DragStartedCommand](#dragstartedcommand)
  - [DragCompletedCommand](#dragcompletedcommand)
- [Event Arguments](#event-arguments)
- [Common Scenarios](#common-scenarios)
- [Best Practices](#best-practices)
- [Related References](#related-references)

## Overview

The .NET MAUI Range Slider (`SfRangeSlider`) provides comprehensive event and command support for tracking user interactions, customizing display elements, and implementing MVVM patterns. This reference covers all events, commands, their parameters, and usage patterns.

## Value Change Events

### ValueChangeStart

The `ValueChangeStart` event fires when the user begins interacting with a thumb (tap or mouse down).

**Event Args:** `EventArgs`

**When It Fires:**
- User taps or presses down on a thumb
- Before any thumb movement begins
- Once per interaction session

**XAML Example:**
```xaml
<sliders:SfRangeSlider ValueChangeStart="OnValueChangeStart" />
```

**C# Example:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider();
rangeSlider.ValueChangeStart += OnValueChangeStart;

private void OnValueChangeStart(object sender, EventArgs e)
{
    // Interaction started
    Console.WriteLine("User started interacting with slider");
}
```

**Use Cases:**
- Start loading indicators
- Pause animations or updates
- Log interaction start
- Disable related UI elements

### ValueChanging

The `ValueChanging` event fires continuously as the user drags a thumb.

**Event Args:** `RangeSliderValueChangingEventArgs`

**Properties:**
- `NewRangeStart` - The new start thumb value
- `NewRangeEnd` - The new end thumb value
- `Cancel` - Set to `true` to prevent the value change

**When It Fires:**
- Continuously during thumb drag
- Before values are actually updated
- Can be used to validate or cancel changes

**XAML Example:**
```xaml
<sliders:SfRangeSlider ValueChanging="OnValueChanging" />
```

**C# Example:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider();
rangeSlider.ValueChanging += OnValueChanging;

private void OnValueChanging(object sender, RangeSliderValueChangingEventArgs e)
{
    // Access proposed new values
    double newStart = e.NewRangeStart;
    double newEnd = e.NewRangeEnd;
    
    // Update dependent UI
    UpdateDependentControls(newStart, newEnd);
}
```

**Validation Example:**
```csharp
private void OnValueChanging(object sender, RangeSliderValueChangingEventArgs e)
{
    // Enforce minimum range width
    double minRange = 10;
    if (e.NewRangeEnd - e.NewRangeStart < minRange)
    {
        e.Cancel = true;
    }
}
```

**With Deferred Updates:**
- When `EnableDeferredUpdate="True"`, this event fires after `DeferredUpdateDelay` expires
- Immediate on touch release regardless of delay

### ValueChanged

The `ValueChanged` event fires when the thumb values have actually changed.

**Event Args:** `RangeSliderValueChangedEventArgs`

**Properties:**
- `NewRangeStart` - The new start thumb value
- `NewRangeEnd` - The new end thumb value
- `OldRangeStart` - The previous start thumb value
- `OldRangeEnd` - The previous end thumb value

**When It Fires:**
- After thumb values have been updated
- During continuous dragging (unless deferred)
- On touch release

**XAML Example:**
```xaml
<sliders:SfRangeSlider ValueChanged="OnValueChanged" />
```

**C# Example:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider();
rangeSlider.ValueChanged += OnValueChanged;

private void OnValueChanged(object sender, RangeSliderValueChangedEventArgs e)
{
    double newStart = e.NewRangeStart;
    double newEnd = e.NewRangeEnd;
    double oldStart = e.OldRangeStart;
    double oldEnd = e.OldRangeEnd;
    
    Console.WriteLine($"Range changed from [{oldStart}, {oldEnd}] to [{newStart}, {newEnd}]");
}
```

**Filtering Example:**
```csharp
private void OnValueChanged(object sender, RangeSliderValueChangedEventArgs e)
{
    // Filter data based on new range
    var filteredData = dataSource
        .Where(item => item.Value >= e.NewRangeStart && item.Value <= e.NewRangeEnd)
        .ToList();
    
    UpdateChart(filteredData);
}
```

### ValueChangeEnd

The `ValueChangeEnd` event fires when the user completes interaction with a thumb (tap or mouse up).

**Event Args:** `EventArgs`

**When It Fires:**
- User releases thumb (tap up, mouse up)
- After all value changes complete
- Once per interaction session

**XAML Example:**
```xaml
<sliders:SfRangeSlider ValueChangeEnd="OnValueChangeEnd" />
```

**C# Example:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider();
rangeSlider.ValueChangeEnd += OnValueChangeEnd;

private void OnValueChangeEnd(object sender, EventArgs e)
{
    // Interaction completed
    Console.WriteLine("User finished interacting with slider");
}
```

**Use Cases:**
- Stop loading indicators
- Resume animations or updates
- Save final values to storage
- Trigger API calls
- Re-enable related UI elements

**Complete Event Lifecycle Example:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider();
rangeSlider.ValueChangeStart += OnValueChangeStart;
rangeSlider.ValueChanging += OnValueChanging;
rangeSlider.ValueChanged += OnValueChanged;
rangeSlider.ValueChangeEnd += OnValueChangeEnd;

private void OnValueChangeStart(object sender, EventArgs e)
{
    loadingIndicator.IsVisible = true;
    Console.WriteLine("1. Start");
}

private void OnValueChanging(object sender, RangeSliderValueChangingEventArgs e)
{
    Console.WriteLine($"2. Changing: [{e.NewRangeStart}, {e.NewRangeEnd}]");
}

private void OnValueChanged(object sender, RangeSliderValueChangedEventArgs e)
{
    Console.WriteLine($"3. Changed: [{e.NewRangeStart}, {e.NewRangeEnd}]");
}

private void OnValueChangeEnd(object sender, EventArgs e)
{
    loadingIndicator.IsVisible = false;
    SaveValues();
    Console.WriteLine("4. End");
}
```

## Customization Events

### LabelCreated

The `LabelCreated` event allows customization of individual label text and style.

**Event Args:** `SliderLabelCreatedEventArgs`

**Properties:**
- `Text` - The label text to display
- `Style` - The label style (SliderLabelStyle)

**XAML Example:**
```xaml
<sliders:SfRangeSlider Minimum="2"
                       Maximum="10"
                       RangeStart="4"
                       RangeEnd="8"
                       Interval="2"
                       ShowLabels="True"
                       ShowTicks="True"
                       LabelCreated="OnLabelCreated" />
```

**C# Example:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    Minimum = 2,
    Maximum = 10,
    RangeStart = 4,
    RangeEnd = 8,
    Interval = 2,
    ShowLabels = true,
    ShowTicks = true
};
rangeSlider.LabelCreated += OnLabelCreated;

private void OnLabelCreated(object sender, SliderLabelCreatedEventArgs e)
{
    e.Text = "$" + e.Text;
}
```

**Advanced Customization:**
```csharp
private void OnLabelCreated(object sender, SliderLabelCreatedEventArgs e)
{
    // Parse the value
    if (double.TryParse(e.Text, out double value))
    {
        // Custom formatting
        if (value >= 100)
        {
            e.Text = $"${value / 1000:0.0}K";
        }
        else
        {
            e.Text = "$" + e.Text;
        }
        
        // Custom styling per value
        if (value >= 50)
        {
            e.Style = new SliderLabelStyle
            {
                ActiveTextColor = Colors.Green,
                InactiveTextColor = Colors.LightGreen,
                ActiveFontSize = 14,
                InactiveFontSize = 12
            };
        }
    }
}
```

### TooltipLabelCreated

The `TooltipLabelCreated` event allows customization of tooltip text and styling.

**Event Args:** `SliderTooltipLabelCreatedEventArgs`

**Properties:**
- `Text` - The tooltip text
- `TextColor` - Text color
- `FontSize` - Font size
- `FontFamily` - Font family
- `FontAttributes` - Font attributes (Bold, Italic)

**XAML Example:**
```xaml
<sliders:SfRangeSlider>
    <sliders:SfRangeSlider.Tooltip>
        <sliders:SliderTooltip TooltipLabelCreated="OnTooltipLabelCreated" />
    </sliders:SfRangeSlider.Tooltip>
</sliders:SfRangeSlider>
```

**C# Example:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider();
rangeSlider.Tooltip = new SliderTooltip();
rangeSlider.Tooltip.TooltipLabelCreated += OnTooltipLabelCreated;

private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    e.Text = "$" + e.Text;
}
```

**Dynamic Styling Example:**
```csharp
private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    if (double.TryParse(e.Text, out double value))
    {
        if (value < 30)
        {
            e.Text = "Low: $" + value;
            e.TextColor = Colors.Red;
            e.FontAttributes = FontAttributes.Bold;
        }
        else if (value > 70)
        {
            e.Text = "High: $" + value;
            e.TextColor = Colors.Green;
            e.FontAttributes = FontAttributes.Bold;
        }
        else
        {
            e.Text = "$" + value;
            e.TextColor = Colors.Blue;
            e.FontAttributes = FontAttributes.None;
        }
    }
}
```

## Commands

### DragStartedCommand

The `DragStartedCommand` executes when thumb dragging begins.

**Type:** `ICommand`  
**Command Parameter:** `DragStartedCommandParameter` (object)

**XAML Example:**
```xaml
<ContentPage.BindingContext>
    <local:ViewModel x:Name="viewModel" />
</ContentPage.BindingContext>

<ContentPage.Content>
    <sliders:SfRangeSlider DragStartedCommand="{Binding DragStartedCommand}" />
</ContentPage.Content>
```

**ViewModel Example:**
```csharp
public class ViewModel
{
    public ICommand DragStartedCommand { get; }

    public ViewModel()
    {
        DragStartedCommand = new Command(OnDragStarted);
    }

    private void OnDragStarted(object obj)
    {
        Debug.WriteLine("Drag started");
    }
}
```

**With Command Parameter:**
```xaml
<sliders:SfRangeSlider DragStartedCommand="{Binding DragStartedCommand}"
                       DragStartedCommandParameter="StartRange" />
```

```csharp
private void OnDragStarted(object parameter)
{
    string context = parameter as string;
    Debug.WriteLine($"Drag started in context: {context}");
}
```

### DragCompletedCommand

The `DragCompletedCommand` executes when thumb dragging completes.

**Type:** `ICommand`  
**Command Parameter:** `DragCompletedCommandParameter` (object)

**XAML Example:**
```xaml
<ContentPage.BindingContext>
    <local:ViewModel x:Name="viewModel" />
</ContentPage.BindingContext>

<ContentPage.Content>
    <sliders:SfRangeSlider DragCompletedCommand="{Binding DragCompletedCommand}" />
</ContentPage.Content>
```

**ViewModel Example:**
```csharp
public class ViewModel
{
    public ICommand DragCompletedCommand { get; }

    public ViewModel()
    {
        DragCompletedCommand = new Command(OnDragCompleted);
    }

    private void OnDragCompleted(object obj)
    {
        Debug.WriteLine("Drag completed");
    }
}
```

**With Command Parameter:**
```xaml
<sliders:SfRangeSlider DragCompletedCommand="{Binding DragCompletedCommand}"
                       DragCompletedCommandParameter="EndRange" />
```

```csharp
private void OnDragCompleted(object parameter)
{
    string context = parameter as string;
    Debug.WriteLine($"Drag completed in context: {context}");
}
```

**Complete MVVM Example:**
```csharp
public class RangeFilterViewModel : INotifyPropertyChanged
{
    private double rangeStart;
    private double rangeEnd;

    public double RangeStart
    {
        get => rangeStart;
        set
        {
            rangeStart = value;
            OnPropertyChanged();
            FilterData();
        }
    }

    public double RangeEnd
    {
        get => rangeEnd;
        set
        {
            rangeEnd = value;
            OnPropertyChanged();
            FilterData();
        }
    }

    public ICommand DragStartedCommand { get; }
    public ICommand DragCompletedCommand { get; }

    public RangeFilterViewModel()
    {
        DragStartedCommand = new Command(OnDragStarted);
        DragCompletedCommand = new Command(OnDragCompleted);
    }

    private void OnDragStarted(object obj)
    {
        // Pause live updates
        IsLiveUpdateEnabled = false;
    }

    private void OnDragCompleted(object obj)
    {
        // Resume live updates and save
        IsLiveUpdateEnabled = true;
        SaveFilterSettings();
    }

    private void FilterData()
    {
        if (IsLiveUpdateEnabled)
        {
            // Filter logic
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

## Event Arguments

### RangeSliderValueChangingEventArgs

Used by `ValueChanging` event.

**Properties:**
- `NewRangeStart` (double) - Proposed new start value
- `NewRangeEnd` (double) - Proposed new end value
- `Cancel` (bool) - Set to true to prevent change

### RangeSliderValueChangedEventArgs

Used by `ValueChanged` event.

**Properties:**
- `NewRangeStart` (double) - New start value
- `NewRangeEnd` (double) - New end value
- `OldRangeStart` (double) - Previous start value
- `OldRangeEnd` (double) - Previous end value

### SliderLabelCreatedEventArgs

Used by `LabelCreated` event.

**Properties:**
- `Text` (string) - Label text
- `Style` (SliderLabelStyle) - Label style

### SliderTooltipLabelCreatedEventArgs

Used by `TooltipLabelCreated` event.

**Properties:**
- `Text` (string) - Tooltip text
- `TextColor` (Color) - Text color
- `FontSize` (double) - Font size
- `FontFamily` (string) - Font family
- `FontAttributes` (FontAttributes) - Font attributes

## Common Scenarios

### Real-time Filtering
```csharp
rangeSlider.ValueChanged += (sender, e) =>
{
    var filtered = items.Where(item => 
        item.Price >= e.NewRangeStart && 
        item.Price <= e.NewRangeEnd);
    listView.ItemsSource = filtered;
};
```

### Deferred API Call
```csharp
rangeSlider.EnableDeferredUpdate = true;
rangeSlider.DeferredUpdateDelay = 500;
rangeSlider.ValueChanged += async (sender, e) =>
{
    await UpdateServerFilterAsync(e.NewRangeStart, e.NewRangeEnd);
};
```

### Progress Tracking
```csharp
rangeSlider.ValueChangeStart += (s, e) => progressBar.IsVisible = true;
rangeSlider.ValueChangeEnd += (s, e) => progressBar.IsVisible = false;
```

### Value Validation
```csharp
rangeSlider.ValueChanging += (sender, e) =>
{
    if (e.NewRangeEnd - e.NewRangeStart < minimumRangeWidth)
    {
        e.Cancel = true;
        DisplayAlert("Invalid Range", "Range must be at least " + minimumRangeWidth, "OK");
    }
};
```

### Custom Label Formatting
```csharp
rangeSlider.LabelCreated += (sender, e) =>
{
    if (double.TryParse(e.Text, out double hours))
    {
        int h = (int)hours;
        int m = (int)((hours - h) * 60);
        e.Text = $"{h:00}:{m:00}";
    }
};
```

## Best Practices

1. **Event vs Command**: 
   - Use events for direct UI updates
   - Use commands for MVVM architecture
   - Commands better for testability

2. **Performance**:
   - Avoid heavy computation in `ValueChanging` (fires frequently)
   - Use `EnableDeferredUpdate` for expensive operations
   - Consider async operations in `ValueChanged`

3. **Validation**:
   - Use `ValueChanging` for cancellable validation
   - Provide user feedback when canceling changes
   - Validate both start and end values

4. **Resource Management**:
   - Unsubscribe from events when disposing
   - Be careful with closures in event handlers
   - Avoid memory leaks from long-lived subscriptions

5. **User Feedback**:
   - Use `ValueChangeStart/End` for loading indicators
   - Update related UI in `ValueChanged`
   - Provide immediate visual feedback

6. **Customization Events**:
   - Keep `LabelCreated` logic simple
   - Cache formatting resources
   - Avoid creating objects per label

7. **MVVM Pattern**:
   - Bind to `RangeStart` and `RangeEnd` properties
   - Use commands for interaction tracking
   - Keep ViewModels testable

8. **Error Handling**:
   - Wrap async operations in try-catch
   - Handle cancellation gracefully
   - Provide user feedback for errors

## Related References

- [intervals-and-selection.md](./intervals-and-selection.md) - EnableDeferredUpdate and DeferredUpdateDelay
- [labels.md](./labels.md) - LabelCreated event usage
- [tooltips.md](./tooltips.md) - TooltipLabelCreated event usage
- [thumbs-and-overlays.md](./thumbs-and-overlays.md) - Thumb interaction events
