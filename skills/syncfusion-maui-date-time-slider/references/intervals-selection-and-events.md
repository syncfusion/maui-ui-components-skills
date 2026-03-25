# Intervals, Selection, and Events in .NET MAUI DateTime Slider

This guide covers interval configuration, discrete selection behavior, event handling, and MVVM command support in the DateTime Slider.

## Table of Contents

- [Intervals](#intervals)
- [Discrete Selection](#discrete-selection)
- [Events](#events)
- [MVVM Commands](#mvvm-commands)
- [LiquidGlass Effect](#liquidglass-effect)
- [Properties Reference](#properties-reference)

## Intervals

Intervals define the spacing between ticks and labels on the slider track.

### Basic Interval Configuration

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2020-01-01"
                          Value="2015-01-01"
                          Interval="2"
                          ShowLabels="True"
                          ShowTicks="True" />
```

**C# Implementation:**

```csharp
var slider = new SfDateTimeSlider
{
    Minimum = new DateTime(2010, 01, 01),
    Maximum = new DateTime(2020, 01, 01),
    Value = new DateTime(2015, 01, 01),
    Interval = 2,
    ShowLabels = true,
    ShowTicks = true
};
```

**Result:** Labels and ticks appear every 2 years (2010, 2012, 2014, etc.)

### Interval Type

Control the unit of intervals:

```xaml
<sliders:SfDateTimeSlider Minimum="2020-01-01"
                          Maximum="2020-12-31"
                          Value="2020-06-15"
                          Interval="2"
                          IntervalType="Months"
                          ShowLabels="True"
                          DateFormat="MMM" />
```

**C# Implementation:**

```csharp
slider.IntervalType = SliderDateIntervalType.Months;
```

**Available Types:**
- `Years` (default)
- `Months`
- `Days`
- `Hours`
- `Minutes`
- `Seconds`

**Example: Daily Intervals**

```csharp
var dailySlider = new SfDateTimeSlider
{
    Minimum = new DateTime(2024, 03, 01),
    Maximum = new DateTime(2024, 03, 31),
    Value = new DateTime(2024, 03, 15),
    Interval = 5,
    IntervalType = SliderDateIntervalType.Days,
    ShowLabels = true,
    DateFormat = "MMM dd"
};
```

**Result:** Labels every 5 days (Mar 01, Mar 06, Mar 11, etc.)

**Example: Hourly Intervals**

```csharp
var hourlySlider = new SfDateTimeSlider
{
    Minimum = new DateTime(2024, 03, 13, 00, 00, 00),
    Maximum = new DateTime(2024, 03, 13, 23, 59, 59),
    Value = new DateTime(2024, 03, 13, 12, 00, 00),
    Interval = 3,
    IntervalType = SliderDateIntervalType.Hours,
    ShowLabels = true,
    DateFormat = "hh tt"
};
```

**Result:** Labels every 3 hours (12 AM, 03 AM, 06 AM, etc.)

## Discrete Selection

Discrete selection restricts thumb movement to specific interval positions.

### Enable Discrete Selection

Use `StepDuration` to snap thumb to interval boundaries:

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2020-01-01"
                          Value="2014-01-01"
                          Interval="2"
                          StepDuration="2"
                          ShowLabels="True"
                          ShowTicks="True" />
```

**C# Implementation:**

```csharp
slider.StepDuration = new SliderStepDuration(years: 2);
```

**Behavior:** Thumb jumps to exact 2-year intervals (2010, 2012, 2014, etc.) - cannot stop between intervals.

### Step Duration by Interval Type

```csharp
// Years
slider.StepDuration = new SliderStepDuration(years: 1);

// Months
slider.StepDuration = new SliderStepDuration(months: 3);

// Days
slider.StepDuration = new SliderStepDuration(days: 7);

// Hours
slider.StepDuration = new SliderStepDuration(hours: 4);

// Minutes
slider.StepDuration = new SliderStepDuration(minutes: 15);

// Seconds
slider.StepDuration = new SliderStepDuration(seconds: 30);
```

### Discrete Month Selection Example

```csharp
var monthPicker = new SfDateTimeSlider
{
    Minimum = new DateTime(2024, 01, 01),
    Maximum = new DateTime(2024, 12, 31),
    Value = new DateTime(2024, 06, 01),
    Interval = 1,
    IntervalType = SliderDateIntervalType.Months,
    StepDuration = new SliderStepDuration(months: 1),
    ShowLabels = true,
    DateFormat = "MMM"
};
```

**Result:** Thumb snaps to first day of each month (Jan 1, Feb 1, Mar 1, etc.)

### Discrete Day Selection Example

```csharp
var weekdayPicker = new SfDateTimeSlider
{
    Minimum = new DateTime(2024, 03, 01),
    Maximum = new DateTime(2024, 03, 31),
    Value = new DateTime(2024, 03, 11),
    Interval = 7,
    IntervalType = SliderDateIntervalType.Days,
    StepDuration = new SliderStepDuration(days: 1),
    ShowLabels = true,
    ShowTicks = true,
    DateFormat = "dd"
};
```

**Result:** Thumb can stop on any day, but ticks/labels show every 7 days.

## Events

DateTime Slider provides events for tracking value changes throughout the interaction lifecycle.

### ValueChangeStart

Fires when user begins dragging the thumb:

```csharp
slider.ValueChangeStart += OnValueChangeStart;

private void OnValueChangeStart(object sender, EventArgs e)
{
    var currentValue = slider.Value;
    Console.WriteLine($"Drag started at: {currentValue:yyyy-MM-dd}");
    
    // Show loading indicator, disable other controls, etc.
}
```

**Use Cases:**
- Show visual feedback that interaction has begun
- Save initial value for comparison
- Disable related UI elements during drag

### ValueChanging

Fires continuously during thumb drag (real-time updates):

```csharp
slider.ValueChanging += OnValueChanging;

private void OnValueChanging(object sender, EventArgs e)
{
    var currentValue = slider.Value;
    Console.WriteLine($"Current value: {currentValue:yyyy-MM-dd}");
    
    // Update preview, calculate differences, etc.
}
```

**Use Cases:**
- Update preview content in real-time
- Show calculated values based on current selection
- Validate selection as user drags

**⚠️ Performance Warning:** This event fires frequently. Keep handlers lightweight.

### ValueChanged

Fires continuously during thumb drag (same as `ValueChanging`):

```csharp
slider.ValueChanged += OnValueChanged;

private void OnValueChanged(object sender, EventArgs e)
{
    var newValue = slider.Value;
    UpdatePreview(newValue);
}
```

### ValueChangeEnd

Fires when user releases the thumb (drag complete):

```csharp
slider.ValueChangeEnd += OnValueChangeEnd;

private void OnValueChangeEnd(object sender, EventArgs e)
{
    var finalValue = slider.Value;
    Console.WriteLine($"Final selection: {finalValue:yyyy-MM-dd}");
    
    // Save value, trigger API call, navigate, etc.
}
```

**Use Cases:**
- Save final value to database
- Trigger data refresh based on new selection
- Re-enable UI elements
- Navigate to new page with selected date

### Deferred Update Pattern

Use `EnableDeferredUpdate` to prevent continuous updates during drag:

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2020-01-01"
                          Value="2015-01-01"
                          EnableDeferredUpdate="True"
                          ValueChangeEnd="OnValueChangeEnd" />
```

**C# Implementation:**

```csharp
slider.EnableDeferredUpdate = true;
slider.ValueChangeEnd += OnValueChangeEnd;

private void OnValueChangeEnd(object sender, EventArgs e)
{
    // Only triggered when drag completes
    LoadData(slider.Value);
}
```

**Benefit:** Prevents expensive operations (API calls, database queries) from running continuously during drag.

### Complete Event Example

```csharp
var slider = new SfDateTimeSlider
{
    Minimum = new DateTime(2020, 01, 01),
    Maximum = new DateTime(2024, 12, 31),
    Value = new DateTime(2022, 06, 15),
    Interval = 1,
    IntervalType = SliderDateIntervalType.Years,
    EnableDeferredUpdate = true
};

DateTime initialValue;

slider.ValueChangeStart += (s, e) =>
{
    initialValue = slider.Value;
    StatusLabel.Text = "Selecting...";
};

slider.ValueChanging += (s, e) =>
{
    var diff = (slider.Value - initialValue).Days;
    PreviewLabel.Text = $"{Math.Abs(diff)} days difference";
};

slider.ValueChangeEnd += (s, e) =>
{
    StatusLabel.Text = $"Selected: {slider.Value:MMM d, yyyy}";
    await LoadDataForDate(slider.Value);
};
```

## MVVM Commands

Use commands for cleaner MVVM architecture.

### DragStartedCommand

```csharp
// ViewModel
public ICommand DragStartedCommand { get; }

public MyViewModel()
{
    DragStartedCommand = new Command(() =>
    {
        IsLoading = true;
        InitialValue = SelectedDate;
    });
}
```

**XAML Binding:**

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2020-01-01"
                          Value="{Binding SelectedDate}"
                          DragStartedCommand="{Binding DragStartedCommand}" />
```

### DragCompletedCommand

```csharp
// ViewModel
public ICommand DragCompletedCommand { get; }

public MyViewModel()
{
    DragCompletedCommand = new Command(async () =>
    {
        IsLoading = false;
        await LoadDataForDate(SelectedDate);
    });
}
```

**XAML Binding:**

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2020-01-01"
                          Value="{Binding SelectedDate}"
                          DragCompletedCommand="{Binding DragCompletedCommand}" />
```

### Command Parameters

Pass custom data to commands:

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2020-01-01"
                          Value="{Binding SelectedDate}"
                          DragStartedCommand="{Binding SliderDragStartedCommand}"
                          DragStartedCommandParameter="YearFilter"
                          DragCompletedCommand="{Binding SliderDragCompletedCommand}"
                          DragCompletedCommandParameter="YearFilter" />
```

**ViewModel:**

```csharp
public ICommand SliderDragStartedCommand { get; }
public ICommand SliderDragCompletedCommand { get; }

public MyViewModel()
{
    SliderDragStartedCommand = new Command<string>((filterType) =>
    {
        Console.WriteLine($"Started drag for {filterType}");
    });
    
    SliderDragCompletedCommand = new Command<string>(async (filterType) =>
    {
        await ApplyFilter(filterType, SelectedDate);
    });
}
```

### Complete MVVM Example

```csharp
// ViewModel
public class DateFilterViewModel : INotifyPropertyChanged
{
    private DateTime _selectedDate = new DateTime(2022, 06, 15);
    private bool _isLoading;
    
    public DateTime SelectedDate
    {
        get => _selectedDate;
        set
        {
            _selectedDate = value;
            OnPropertyChanged(nameof(SelectedDate));
        }
    }
    
    public bool IsLoading
    {
        get => _isLoading;
        set
        {
            _isLoading = value;
            OnPropertyChanged(nameof(IsLoading));
        }
    }
    
    public ICommand DragStartedCommand { get; }
    public ICommand DragCompletedCommand { get; }
    
    public DateFilterViewModel()
    {
        DragStartedCommand = new Command(OnDragStarted);
        DragCompletedCommand = new Command(OnDragCompleted);
    }
    
    private void OnDragStarted()
    {
        IsLoading = true;
    }
    
    private async void OnDragCompleted()
    {
        await Task.Delay(500); // Simulate API call
        IsLoading = false;
        
        // Trigger data refresh
        MessagingCenter.Send(this, "DateChanged", SelectedDate);
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**XAML:**

```xaml
<ContentPage.BindingContext>
    <local:DateFilterViewModel />
</ContentPage.BindingContext>

<VerticalStackLayout>
    <ActivityIndicator IsRunning="{Binding IsLoading}" />
    
    <sliders:SfDateTimeSlider Minimum="2020-01-01"
                              Maximum="2024-12-31"
                              Value="{Binding SelectedDate}"
                              Interval="1"
                              IntervalType="Years"
                              ShowLabels="True"
                              DragStartedCommand="{Binding DragStartedCommand}"
                              DragCompletedCommand="{Binding DragCompletedCommand}" />
    
    <Label Text="{Binding SelectedDate, StringFormat='Selected: {0:MMMM d, yyyy}'}" />
</VerticalStackLayout>
```

## LiquidGlass Effect

Enable a smooth, fluid animation effect during thumb drag:

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2020-01-01"
                          Value="2015-01-01"
                          EnableLiquidGlassEffect="True"
                          Interval="2"
                          ShowTicks="True"
                          ShowLabels="True" />
```

**C# Implementation:**

```csharp
slider.EnableLiquidGlassEffect = true;
```

**Effect:** Track appears to "flow" like liquid as thumb moves, creating a premium visual experience.

**Performance Note:** May impact performance on lower-end devices. Test before enabling in production.

## Properties Reference

### Intervals

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Interval` | double | 0 (auto-calculated) | Spacing between ticks/labels |
| `IntervalType` | SliderDateIntervalType | Years | Unit of intervals (Years, Months, Days, Hours, Minutes, Seconds) |

### Discrete Selection

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `StepDuration` | SliderStepDuration | null (continuous) | Snap intervals for discrete selection |

### Events

| Event | Description |
|-------|-------------|
| `ValueChangeStart` | Fires when thumb drag begins |
| `ValueChanging` | Fires continuously during thumb drag |
| `ValueChanged` | Fires continuously during thumb drag (same as ValueChanging) |
| `ValueChangeEnd` | Fires when thumb drag completes |

### Commands

| Property | Type | Description |
|----------|------|-------------|
| `DragStartedCommand` | ICommand | Command executed when drag begins |
| `DragStartedCommandParameter` | object | Parameter passed to DragStartedCommand |
| `DragCompletedCommand` | ICommand | Command executed when drag completes |
| `DragCompletedCommandParameter` | object | Parameter passed to DragCompletedCommand |

### Other

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `EnableDeferredUpdate` | bool | false | Prevent `ValueChanged` firing during drag (only fires on `ValueChangeEnd`) |
| `EnableLiquidGlassEffect` | bool | false | Enable fluid animation effect |

## Common Patterns

### Pattern 1: Year Picker with Discrete Selection

```csharp
var yearPicker = new SfDateTimeSlider
{
    Minimum = new DateTime(2000, 01, 01),
    Maximum = new DateTime(2030, 12, 31),
    Value = new DateTime(2024, 01, 01),
    Interval = 5,
    IntervalType = SliderDateIntervalType.Years,
    StepDuration = new SliderStepDuration(years: 1),
    ShowLabels = true,
    ShowTicks = true,
    DateFormat = "yyyy",
    EnableDeferredUpdate = true
};

yearPicker.ValueChangeEnd += async (s, e) =>
{
    await LoadYearlyReport(yearPicker.Value.Year);
};
```

### Pattern 2: Date Range Filter with MVVM

```csharp
// ViewModel
public DateTime FilterDate { get; set; } = DateTime.Today;
public ICommand FilterChangedCommand { get; }

public MyViewModel()
{
    FilterChangedCommand = new Command(async () =>
    {
        await RefreshData(FilterDate);
    });
}

// XAML
<sliders:SfDateTimeSlider Minimum="2024-01-01"
                          Maximum="2024-12-31"
                          Value="{Binding FilterDate}"
                          Interval="7"
                          IntervalType="Days"
                          DateFormat="MMM dd"
                          DragCompletedCommand="{Binding FilterChangedCommand}" />
```

### Pattern 3: Real-time Preview with Deferred Save

```csharp
slider.ValueChanging += (s, e) =>
{
    // Update preview in real-time
    PreviewLabel.Text = $"Preview: {slider.Value:MMM d, yyyy}";
};

slider.EnableDeferredUpdate = true;
slider.ValueChangeEnd += async (s, e) =>
{
    // Save only when drag completes
    await SaveSelection(slider.Value);
};
```

## Best Practices

1. **Discrete Selection**: Use `StepDuration` for date pickers where precision is critical
2. **Performance**: Enable `EnableDeferredUpdate` when `ValueChanged` triggers expensive operations
3. **MVVM**: Prefer commands over events for cleaner architecture
4. **Event Handlers**: Keep `ValueChanging` handlers lightweight (fires frequently)
5. **Intervals**: Match `Interval` and `IntervalType` to your data granularity
6. **LiquidGlass**: Test on target devices before enabling (performance impact)

## Next Steps

- **Tooltip**: Display formatted dates during interaction
- **Custom Labels**: Use `LabelCreated` event for context-specific text
- **Track Styling**: Coordinate visual feedback with event states
