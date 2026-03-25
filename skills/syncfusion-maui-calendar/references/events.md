# Events in .NET MAUI Calendar

The SfCalendar provides five comprehensive events to handle user interactions: ViewChanged, SelectionChanged, Tapped, DoubleTapped, and LongPressed. This guide covers all events and their usage patterns.

## Table of Contents
- [ViewChanged Event](#viewchanged-event)
- [SelectionChanged Event](#selectionchanged-event)
- [Tapped Event](#tapped-event)
- [DoubleTapped Event](#doubletapped-event)
- [LongPressed Event](#longpressed-event)
- [Commands (MVVM Pattern)](#commands-mvvm-pattern)
- [Event Order and Lifecycle](#event-order-and-lifecycle)

## ViewChanged Event

The `ViewChanged` event fires when the calendar view changes (swipe to previous/next month, switch between views, etc.).

### Event Arguments

**CalendarViewChangedEventArgs Properties:**
- `NewVisibleDates` (CalendarDateRange): The new visible date range
- `OldVisibleDates` (CalendarDateRange): The previous visible date range
- `NewView` (CalendarView): The new calendar view (Month/Year/Decade/Century)
- `OldView` (CalendarView): The previous calendar view

### Basic Usage

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar" 
                     ViewChanged="OnCalendarViewChanged" />
```

**C#:**
```csharp
calendar.ViewChanged += OnCalendarViewChanged;

private void OnCalendarViewChanged(object sender, CalendarViewChangedEventArgs e)
{
    // Access old and new visible dates
    var oldVisibleDates = e.OldVisibleDates;
    var newVisibleDates = e.NewVisibleDates;
    
    // Access old and new views
    var oldCalendarView = e.OldView;
    var newCalendarView = e.NewView;
    
    Console.WriteLine($"View changed from {oldCalendarView} to {newCalendarView}");
    Console.WriteLine($"Date range: {newVisibleDates.StartDate:d} to {newVisibleDates.EndDate:d}");
}
```

### Use Cases

**1. Load Data for Visible Range:**
```csharp
private async void OnCalendarViewChanged(object sender, CalendarViewChangedEventArgs e)
{
    // Load appointments/events for the new visible date range
    var startDate = e.NewVisibleDates.StartDate;
    var endDate = e.NewVisibleDates.EndDate;
    
    await LoadEventsForDateRange(startDate, endDate.Value);
}
```

**2. Update Special Dates Dynamically:**
```csharp
private async void OnCalendarViewChanged(object sender, CalendarViewChangedEventArgs e)
{
    specialDates.Clear();
    await LoadSpecialDatesFromAPI(e.NewVisibleDates);
    calendar.UpdateSpecialDayPredicate();
}
```

**3. Track User Navigation:**
```csharp
private void OnCalendarViewChanged(object sender, CalendarViewChangedEventArgs e)
{
    if (e.NewView == CalendarView.Month && e.OldView == CalendarView.Year)
    {
        Console.WriteLine("User drilled down from Year to Month view");
    }
}
```

### Important Note

The `SelectableDayPredicate` function is automatically re-evaluated when the view changes. Ensure your predicate is performant.

## SelectionChanged Event

The `SelectionChanged` event fires when the user selects or deselects dates.

### Event Arguments

**CalendarSelectionChangedEventArgs Properties:**
- `NewValue` (object): The new selection (DateTime, ObservableCollection<DateTime>, or CalendarDateRange)
- `OldValue` (object): The previous selection

### Basic Usage

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar" 
                     SelectionChanged="OnCalendarSelectionChanged" />
```

**C#:**
```csharp
calendar.SelectionChanged += OnCalendarSelectionChanged;

private void OnCalendarSelectionChanged(object sender, CalendarSelectionChangedEventArgs e)
{
    var oldDateTime = e.OldValue;
    var newDateTime = e.NewValue;
    
    // Handle based on selection mode
    switch (calendar.SelectionMode)
    {
        case CalendarSelectionMode.Single:
            if (newDateTime is DateTime date)
                Console.WriteLine($"Selected: {date:d}");
            break;
            
        case CalendarSelectionMode.Multiple:
            if (newDateTime is ObservableCollection<DateTime> dates)
                Console.WriteLine($"Selected {dates.Count} dates");
            break;
            
        case CalendarSelectionMode.Range:
            if (newDateTime is CalendarDateRange range)
                Console.WriteLine($"Range: {range.StartDate:d} to {range.EndDate:d}");
            break;
    }
}
```

### Selection Mode Patterns

**Single Selection:**
```csharp
private void OnSelectionChanged(object sender, CalendarSelectionChangedEventArgs e)
{
    if (e.NewValue is DateTime selectedDate)
    {
        // Validate selection
        if (selectedDate.DayOfWeek == DayOfWeek.Sunday)
        {
            DisplayAlert("Invalid", "Sundays are not available", "OK");
            calendar.SelectedDate = e.OldValue as DateTime?;
        }
        else
        {
            // Process valid selection
            ProcessSelectedDate(selectedDate);
        }
    }
}
```

**Multiple Selection:**
```csharp
private void OnSelectionChanged(object sender, CalendarSelectionChangedEventArgs e)
{
    if (e.NewValue is ObservableCollection<DateTime> selectedDates)
    {
        // Limit selection count
        if (selectedDates.Count > 10)
        {
            DisplayAlert("Limit", "Maximum 10 dates allowed", "OK");
            var lastAdded = selectedDates.Last();
            calendar.SelectedDates.Remove(lastAdded);
        }
        
        UpdateSummary($"{selectedDates.Count} dates selected");
    }
}
```

**Range Selection:**
```csharp
private void OnSelectionChanged(object sender, CalendarSelectionChangedEventArgs e)
{
    if (e.NewValue is CalendarDateRange range && range.EndDate.HasValue)
    {
        var days = (range.EndDate.Value - range.StartDate).Days + 1;
        
        // Validate range length
        if (days > 30)
        {
            DisplayAlert("Invalid", "Maximum 30-day range allowed", "OK");
            calendar.SelectedRange = e.OldValue as CalendarDateRange;
        }
        else
        {
            UpdateSummary($"{days} days selected");
            CalculateTotalCost(range.StartDate, range.EndDate.Value);
        }
    }
}
```

### Important Note

The `Tapped` event fires first, followed by the `SelectionChanged` event.

## Tapped Event

The `Tapped` event fires when the user taps on a calendar element (date, week number, header, etc.).

### Event Arguments

**CalendarTappedEventArgs Properties:**
- `Date` (DateTime?): The tapped date (null if tapping non-date elements)
- `Element` (CalendarElement): The tapped element type
- `WeekNumber` (int?): The tapped week number (if ShowWeekNumber is true)

**CalendarElement Enum:**
```csharp
public enum CalendarElement
{
    CalendarCell,    // Date cell tapped
    Header,          // Calendar header tapped
    WeekNumber       // Week number cell tapped
}
```

### Basic Usage

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar" 
                     Tapped="OnCalendarTapped" />
```

**C#:**
```csharp
calendar.Tapped += OnCalendarTapped;

private void OnCalendarTapped(object sender, CalendarTappedEventArgs e)
{
    var selectedDate = e.Date;
    var calendarElement = e.Element;
    var weekNumber = e.WeekNumber;
    
    if (e.Element == CalendarElement.CalendarCell && e.Date.HasValue)
    {
        Console.WriteLine($"Tapped date: {e.Date.Value:d}");
    }
    else if (e.Element == CalendarElement.WeekNumber)
    {
        Console.WriteLine($"Tapped week number: {e.WeekNumber}");
    }
    else if (e.Element == CalendarElement.Header)
    {
        Console.WriteLine("Header tapped");
    }
}
```

### Use Cases

**1. Show Details on Tap:**
```csharp
private async void OnCalendarTapped(object sender, CalendarTappedEventArgs e)
{
    if (e.Element == CalendarElement.CalendarCell && e.Date.HasValue)
    {
        var events = await GetEventsForDate(e.Date.Value);
        if (events.Count > 0)
        {
            await ShowEventDetails(events);
        }
    }
}
```

**2. Custom Week Number Handling:**
```csharp
private void OnCalendarTapped(object sender, CalendarTappedEventArgs e)
{
    if (e.Element == CalendarElement.WeekNumber && e.WeekNumber.HasValue)
    {
        // Select all dates in the tapped week
        var weekStart = GetWeekStartDate(e.Date.Value, e.WeekNumber.Value);
        SelectWeekDates(weekStart);
    }
}
```

## DoubleTapped Event

The `DoubleTapped` event fires when the user double-taps on a calendar element.

### Event Arguments

**CalendarDoubleTappedEventArgs Properties:**
- `Date` (DateTime?): The double-tapped date
- `Element` (CalendarElement): The double-tapped element type
- `WeekNumber` (int?): The double-tapped week number

### Basic Usage

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar" 
                     DoubleTapped="OnCalendarDoubleTapped" />
```

**C#:**
```csharp
calendar.DoubleTapped += OnCalendarDoubleTapped;

private void OnCalendarDoubleTapped(object sender, CalendarDoubleTappedEventArgs e)
{
    var selectedDate = e.Date;
    var calendarElement = e.Element;
    var weekNumber = e.WeekNumber;
    
    if (e.Element == CalendarElement.CalendarCell && e.Date.HasValue)
    {
        Console.WriteLine($"Double-tapped date: {e.Date.Value:d}");
    }
}
```

### Use Cases

**Quick Action on Double-Tap:**
```csharp
private async void OnCalendarDoubleTapped(object sender, CalendarDoubleTappedEventArgs e)
{
    if (e.Element == CalendarElement.CalendarCell && e.Date.HasValue)
    {
        // Double-tap to create new event
        await CreateEventForDate(e.Date.Value);
    }
}
```

## LongPressed Event

The `LongPressed` event fires when the user long-presses on a calendar element.

### Event Arguments

**CalendarLongPressedEventArgs Properties:**
- `Date` (DateTime?): The long-pressed date
- `Element` (CalendarElement): The long-pressed element type
- `WeekNumber` (int?): The long-pressed week number

### Basic Usage

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar" 
                     LongPressed="OnCalendarLongPressed" />
```

**C#:**
```csharp
calendar.LongPressed += OnCalendarLongPressed;

private void OnCalendarLongPressed(object sender, CalendarLongPressedEventArgs e)
{
    var selectedDate = e.Date;
    var calendarElement = e.Element;
    var weekNumber = e.WeekNumber;
    
    if (e.Element == CalendarElement.CalendarCell && e.Date.HasValue)
    {
        Console.WriteLine($"Long-pressed date: {e.Date.Value:d}");
    }
}
```

### Use Cases

**Context Menu on Long Press:**
```csharp
private async void OnCalendarLongPressed(object sender, CalendarLongPressedEventArgs e)
{
    if (e.Element == CalendarElement.CalendarCell && e.Date.HasValue)
    {
        var action = await DisplayActionSheet(
            $"Options for {e.Date.Value:d}",
            "Cancel",
            null,
            "Add Event",
            "Mark as Special",
            "View Details"
        );
        
        switch (action)
        {
            case "Add Event":
                await AddEvent(e.Date.Value);
                break;
            case "Mark as Special":
                MarkAsSpecial(e.Date.Value);
                break;
            case "View Details":
                await ViewDetails(e.Date.Value);
                break;
        }
    }
}
```

## Commands (MVVM Pattern)

All events have corresponding command properties for MVVM scenarios.

### Available Commands

- `ViewChangedCommand`
- `SelectionChangedCommand`
- `TappedCommand`
- `DoubleTappedCommand`
- `LongPressedCommand`

### XAML Binding

```xml
<calendar:SfCalendar x:Name="calendar"
                     ViewChangedCommand="{Binding ViewChangedCommand}"
                     SelectionChangedCommand="{Binding SelectionChangedCommand}"
                     TappedCommand="{Binding TappedCommand}">
    <calendar:SfCalendar.BindingContext>
        <local:CalendarViewModel />
    </calendar:SfCalendar.BindingContext>
</calendar:SfCalendar>
```

### ViewModel Implementation

```csharp
public class CalendarViewModel : INotifyPropertyChanged
{
    public ICommand ViewChangedCommand { get; set; }
    public ICommand SelectionChangedCommand { get; set; }
    public ICommand TappedCommand { get; set; }
    
    public CalendarViewModel()
    {
        ViewChangedCommand = new Command<CalendarViewChangedEventArgs>(OnViewChanged);
        SelectionChangedCommand = new Command<CalendarSelectionChangedEventArgs>(OnSelectionChanged);
        TappedCommand = new Command<CalendarTappedEventArgs>(OnTapped);
    }
    
    private void OnViewChanged(CalendarViewChangedEventArgs args)
    {
        // Handle view change
        Debug.WriteLine($"View changed to {args.NewView}");
    }
    
    private void OnSelectionChanged(CalendarSelectionChangedEventArgs args)
    {
        // Handle selection change
        SelectedDate = args.NewValue as DateTime?;
    }
    
    private void OnTapped(CalendarTappedEventArgs args)
    {
        // Handle tap
        if (args.Date.HasValue)
        {
            Debug.WriteLine($"Tapped: {args.Date.Value:d}");
        }
    }
    
    private DateTime? _selectedDate;
    public DateTime? SelectedDate
    {
        get => _selectedDate;
        set
        {
            _selectedDate = value;
            OnPropertyChanged();
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```
## Best Practices

1. **Keep Event Handlers Fast:** Events fire frequently; avoid heavy processing
2. **Use Async for API Calls:** Don't block the UI thread
3. **Validate in SelectionChanged:** Implement business logic validation
4. **Use Commands for MVVM:** Keep View and ViewModel decoupled
5. **Handle Null Values:** Check for null dates when tapping non-date elements
6. **Debounce Rapid Events:** Consider debouncing for ViewChanged if doing expensive operations
7. **Test on Real Devices:** Touch event behavior may differ between platforms
