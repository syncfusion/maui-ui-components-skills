# Date Selection in .NET MAUI Calendar

The SfCalendar supports three selection modes: Single, Multiple, and Range. This guide covers all selection modes, programmatic selection, and selection behavior across different views.

## Table of Contents
- [Selection Modes Overview](#selection-modes-overview)
- [Single Selection](#single-selection)
- [Multiple Selection](#multiple-selection)
- [Range Selection](#range-selection)
- [Range Selection Direction](#range-selection-direction)
- [Enable Swipe Selection](#enable-swipe-selection)
- [Programmatic Selection](#programmatic-selection)
- [Selection in Non-Month Views](#selection-in-non-month-views)
- [Selection Changed Event](#selection-changed-event)

## Selection Modes Overview

The `SelectionMode` property determines how users can select dates in the calendar.

```csharp
public enum CalendarSelectionMode
{
    Single,    // Select one date at a time
    Multiple,  // Select multiple individual dates
    Range      // Select a continuous range of dates
}
```

**Default:** `CalendarSelectionMode.Single`

## Single Selection

In Single selection mode, users can select only one date at a time. Selecting a new date automatically deselects the previously selected date.

### Enable Single Selection

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar" 
                     View="Month"
                     SelectionMode="Single" />
```

**C#:**
```csharp
calendar.SelectionMode = CalendarSelectionMode.Single;
```

### Access Selected Date

Use the `SelectedDate` property to get or set the selected date:

```csharp
// Get selected date
DateTime? selectedDate = calendar.SelectedDate;

// Set selected date programmatically
calendar.SelectedDate = new DateTime(2026, 12, 25);
```

**Note:** `SelectedDate` is nullable. It's `null` when no date is selected.

### Single Selection Example

```csharp
calendar.SelectionMode = CalendarSelectionMode.Single;
calendar.SelectedDate = DateTime.Today;

calendar.SelectionChanged += (s, e) =>
{
    if (e.NewValue is DateTime newDate)
    {
        Console.WriteLine($"Selected date: {newDate:yyyy-MM-dd}");
    }
};
```

## Multiple Selection

In Multiple selection mode, users can select multiple individual dates. Tapping a selected date again will deselect it.

### Enable Multiple Selection

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar" 
                     View="Month"
                     SelectionMode="Multiple" />
```

**C#:**
```csharp
calendar.SelectionMode = CalendarSelectionMode.Multiple;
```

### Access Selected Dates

Use the `SelectedDates` property (an `ObservableCollection<DateTime>`) to get or set selected dates:

```csharp
// Get all selected dates
ObservableCollection<DateTime> selectedDates = calendar.SelectedDates;

// Set selected dates programmatically
calendar.SelectedDates = new ObservableCollection<DateTime>
{
    new DateTime(2026, 12, 25),
    new DateTime(2026, 12, 26),
    new DateTime(2026, 12, 31)
};
```

### Multiple Selection Example

```csharp
calendar.SelectionMode = CalendarSelectionMode.Multiple;

calendar.SelectionChanged += (s, e) =>
{
    if (e.NewValue is ObservableCollection<DateTime> dates)
    {
        Console.WriteLine($"Total selected dates: {dates.Count}");
        foreach (var date in dates)
        {
            Console.WriteLine($"  - {date:yyyy-MM-dd}");
        }
    }
};
```

### Toggle Date Selection

```csharp
private void ToggleDate(DateTime date)
{
    if (calendar.SelectedDates.Contains(date))
    {
        // Deselect
        calendar.SelectedDates.Remove(date);
    }
    else
    {
        // Select
        calendar.SelectedDates.Add(date);
    }
}
```

## Range Selection

In Range selection mode, users can select a continuous range of dates. The selection includes a start date, an end date, and all dates in between.

### Enable Range Selection

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar" 
                     View="Month"
                     SelectionMode="Range" />
```

**C#:**
```csharp
calendar.SelectionMode = CalendarSelectionMode.Range;
```

### Access Selected Date Range

Use the `SelectedDateRange` property to get or set the selected date range:

```csharp
// Get selected range
CalendarDateRange selectedDateRange = calendar.SelectedDateRange;
DateTime? startDate = selectedDateRange?.StartDate;
DateTime? endDate = selectedDateRange?.EndDate;

// Set selected range programmatically
calendar.SelectedDateRange = new CalendarDateRange(
    new DateTime(2026, 12, 25),
    new DateTime(2026, 12, 31)
);
```

### Range Selection Example

```csharp
calendar.SelectionMode = CalendarSelectionMode.Range;

calendar.SelectionChanged += (s, e) =>
{
    if (e.NewValue is CalendarDateRange range)
    {
        Console.WriteLine($"Start: {range.StartDate:yyyy-MM-dd}");
        Console.WriteLine($"End: {range.EndDate:yyyy-MM-dd}");
        
        // Calculate number of days in range
        if (range.EndDate.HasValue)
        {
            int days = (range.EndDate.Value - range.StartDate).Days + 1;
            Console.WriteLine($"Days selected: {days}");
        }
    }
};
```

### Range Selection Behavior

1. **First Tap:** Sets the start date
2. **Second Tap:** Sets the end date (creates a range)
3. **Third Tap:** Resets and starts a new range

```csharp
// User taps Dec 20 → Start date set to Dec 20
// User taps Dec 25 → End date set to Dec 25 (range: Dec 20-25)
// User taps Dec 30 → Range resets, start date set to Dec 30
```

## Range Selection Direction

Control the direction of range selection using the `RangeSelectionDirection` property. This determines which dates can be selected after the start date.

### Direction Enumeration

```csharp
public enum CalendarRangeSelectionDirection
{
    Default,   // Both forward and backward (no restriction)
    Forward,   // Only dates after start date
    Backward,  // Only dates before start date
    Both,      // Same as Default
    None       // Disable range selection (acts like single selection)
}
```

**Default:** `CalendarRangeSelectionDirection.Default`

### Forward Direction

Only dates **after** the start date can be selected. Dates before the start date are disabled.

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar" 
                     View="Month"
                     SelectionMode="Range"
                     RangeSelectionDirection="Forward" />
```

**C#:**
```csharp
calendar.SelectionMode = CalendarSelectionMode.Range;
calendar.RangeSelectionDirection = CalendarRangeSelectionDirection.Forward;
```

**Behavior:**
- User taps Dec 20 → Start date = Dec 20
- Dates before Dec 20 become disabled
- User can only select Dec 21 or later as end date

### Backward Direction

Only dates **before** the start date can be selected. Dates after the start date are disabled.

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar" 
                     View="Month"
                     SelectionMode="Range"
                     RangeSelectionDirection="Backward" />
```

**C#:**
```csharp
calendar.SelectionMode = CalendarSelectionMode.Range;
calendar.RangeSelectionDirection = CalendarRangeSelectionDirection.Backward;
```

**Behavior:**
- User taps Dec 20 → Start date = Dec 20
- Dates after Dec 20 become disabled
- User can only select Dec 19 or earlier as end date

### Both Direction

Users can select dates in either direction (same as Default).

```csharp
calendar.RangeSelectionDirection = CalendarRangeSelectionDirection.Both;
```

### None Direction

Disables range selection behavior. The calendar behaves like Single selection mode even when `SelectionMode` is set to Range.

```csharp
calendar.RangeSelectionDirection = CalendarRangeSelectionDirection.None;
```

## Enable Swipe Selection

Enable swipe gestures for range selection using the `EnableSwipeSelection` property. When enabled, users can swipe across dates to select a range.

**Default:** `false`

### Enable Swipe Gesture

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar" 
                     View="Month"
                     SelectionMode="Range"
                     EnableSwipeSelection="True" />
```

**C#:**
```csharp
calendar.SelectionMode = CalendarSelectionMode.Range;
calendar.EnableSwipeSelection = true;
```

**Behavior:**
- Tap and hold on start date
- Swipe finger to end date
- Release to complete selection

**Note:** Swipe selection only works when `SelectionMode` is set to `Range`.

## Programmatic Selection

Set selections programmatically using the appropriate properties for each selection mode.

### Programmatic Single Selection

```csharp
// Select today
calendar.SelectedDate = DateTime.Today;

// Select a specific date
calendar.SelectedDate = new DateTime(2026, 12, 25);

// Clear selection
calendar.SelectedDate = null;
```

### Programmatic Multiple Selection

```csharp
// Select multiple dates
calendar.SelectedDates = new ObservableCollection<DateTime>
{
    DateTime.Today,
    DateTime.Today.AddDays(1),
    DateTime.Today.AddDays(7),
    DateTime.Today.AddDays(14)
};

// Add a date to existing selection
calendar.SelectedDates.Add(new DateTime(2026, 12, 31));

// Remove a date
calendar.SelectedDates.Remove(DateTime.Today);

// Clear all selections
calendar.SelectedDates.Clear();
```

### Programmatic Range Selection

```csharp
// Select a date range
calendar.SelectedDateRange = new CalendarDateRange(
    startDate: new DateTime(2026, 12, 1),
    endDate: new DateTime(2026, 12, 31)
);

// Select a 7-day range starting today
calendar.SelectedDateRange = new CalendarDateRange(
    DateTime.Today,
    DateTime.Today.AddDays(6)
);

// Clear range selection
calendar.SelectedDateRange = null;
```

## Selection in Non-Month Views

When `AllowViewNavigation` is set to `false`, you can select cells in Year, Decade, and Century views.

### Year View Selection

When selecting a month in Year view:
- **Single/Multiple:** Returns the **first date of the month** (e.g., selecting "Dec" returns `12/1/2026`)
- **Range:** Returns the **first date of start month** to **last date of end month**

```csharp
calendar.View = CalendarView.Year;
calendar.AllowViewNavigation = false;
calendar.SelectionMode = CalendarSelectionMode.Range;

// User selects Sep-Dec range
// Result: StartDate = 09/1/2026, EndDate = 12/31/2026
```

### Decade View Selection

When selecting a year in Decade view:
- **Single/Multiple:** Returns the **first date of the year** (e.g., selecting "2026" returns `1/1/2026`)
- **Range:** Returns the **first date of start year** to **last date of end year**

```csharp
calendar.View = CalendarView.Decade;
calendar.AllowViewNavigation = false;
calendar.SelectionMode = CalendarSelectionMode.Range;

// User selects 2024-2026 range
// Result: StartDate = 1/1/2024, EndDate = 12/31/2026
```

### Century View Selection

When selecting a decade in Century view:
- **Single/Multiple:** Returns the **first date of the decade** (e.g., selecting "2020-2029" returns `1/1/2020`)
- **Range:** Returns the **first date of start decade** to **last date of end decade**

```csharp
calendar.View = CalendarView.Century;
calendar.AllowViewNavigation = false;
calendar.SelectionMode = CalendarSelectionMode.Range;

// User selects 2020-2029 to 2030-2039 range
// Result: StartDate = 1/1/2020, EndDate = 12/31/2039
```

## Selection Changed Event

The `SelectionChanged` event fires whenever the selected date(s) change. See the Events documentation for detailed information on handling this event.

### Basic Event Handling

```csharp
calendar.SelectionChanged += OnSelectionChanged;

private void OnSelectionChanged(object sender, CalendarSelectionChangedEventArgs e)
{
    // e.OldValue: Previous selection
    // e.NewValue: New selection (DateTime, ObservableCollection<DateTime>, or CalendarDateRange)
    
    switch (calendar.SelectionMode)
    {
        case CalendarSelectionMode.Single:
            if (e.NewValue is DateTime date)
                Console.WriteLine($"Single: {date:d}");
            break;
            
        case CalendarSelectionMode.Multiple:
            if (e.NewValue is ObservableCollection<DateTime> dates)
                Console.WriteLine($"Multiple: {dates.Count} dates");
            break;
            
        case CalendarSelectionMode.Range:
            if (e.NewValue is CalendarDateRange range)
                Console.WriteLine($"Range: {range.StartDate:d} to {range.EndDate:d}");
            break;
    }
}
```

## Best Practices

1. **Choose the Right Mode:**
   - Use **Single** for simple date pickers
   - Use **Multiple** for vacation/event planners
   - Use **Range** for booking systems and date range filters

2. **Validate Selections:**
   - Check for null values when using `SelectedDate` or `SelectedRange`
   - Validate that ranges have both start and end dates

3. **Provide Feedback:**
   - Use `SelectionChanged` event to update UI or perform validation
   - Show selected date count for Multiple mode
   - Display range duration for Range mode

4. **Combine with Restrictions:**
   - Use `MinimumDate` and `MaximumDate` with Range selection
   - Use `SelectableDayPredicate` to disable invalid dates
   - Use `RangeSelectionDirection` to guide user selection

5. **Enable Swipe for Better UX:**
   - Enable `EnableSwipeSelection` for range mode on touch devices
   - Provides faster date range selection
