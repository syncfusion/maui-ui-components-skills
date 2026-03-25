# Date Navigation in .NET MAUI Calendar

The SfCalendar provides multiple navigation methods to move between dates and views programmatically or through user interaction. This guide covers all navigation capabilities.

## Table of Contents
- [Display Date (Programmatic Date Navigation)](#display-date-programmatic-date-navigation)
- [View Navigation (Switch Between Views)](#view-navigation-switch-between-views)
- [Allow View Navigation](#allow-view-navigation)
- [Navigate to Adjacent Months](#navigate-to-adjacent-months)
- [Swipe Navigation](#swipe-navigation)
- [Header Tap Navigation](#header-tap-navigation)

## Display Date (Programmatic Date Navigation)

The `DisplayDate` property controls which date is currently displayed in the calendar. Use this property to navigate to a specific date programmatically.

**Default:** `DateTime.Now`

### Set Display Date

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar" 
                     View="Month"
                     DisplayDate="2026-12-25" />
```

**C#:**
```csharp
// Navigate to Christmas 2026
calendar.DisplayDate = new DateTime(2026, 12, 25);

// Navigate to 2 months from now
calendar.DisplayDate = DateTime.Now.AddMonths(2);

// Navigate to specific date
calendar.DisplayDate = new DateTime(2027, 6, 15);
```

### Navigate to Today

```csharp
// Jump back to today's date
calendar.DisplayDate = DateTime.Today;
```

### Navigate Relative to Current Display

```csharp
// Move forward one month from current display
calendar.DisplayDate = calendar.DisplayDate.AddMonths(1);

// Move backward one year
calendar.DisplayDate = calendar.DisplayDate.AddYears(-1);

// Move forward 7 days
calendar.DisplayDate = calendar.DisplayDate.AddDays(7);
```

### Example: Navigation Buttons

```xml
<StackLayout>
    <HorizontalStackLayout HorizontalOptions="Center" Spacing="10">
        <Button Text="Previous Month" Clicked="OnPreviousMonth" />
        <Button Text="Today" Clicked="OnToday" />
        <Button Text="Next Month" Clicked="OnNextMonth" />
    </HorizontalStackLayout>
    
    <calendar:SfCalendar x:Name="calendar" View="Month" />
</StackLayout>
```

```csharp
private void OnPreviousMonth(object sender, EventArgs e)
{
    calendar.DisplayDate = calendar.DisplayDate.AddMonths(-1);
}

private void OnToday(object sender, EventArgs e)
{
    calendar.DisplayDate = DateTime.Today;
}

private void OnNextMonth(object sender, EventArgs e)
{
    calendar.DisplayDate = calendar.DisplayDate.AddMonths(1);
}
```

## View Navigation (Switch Between Views)

The `View` property determines which calendar view is displayed (Month, Year, Decade, or Century). Change this property to switch views programmatically.

### Switch Views Programmatically

```csharp
// Switch to Year view
calendar.View = CalendarView.Year;

// Switch to Month view
calendar.View = CalendarView.Month;

// Switch to Decade view
calendar.View = CalendarView.Decade;

// Switch to Century view
calendar.View = CalendarView.Century;
```

### Example: View Picker

```xml
<StackLayout>
    <Picker x:Name="viewPicker" 
            Title="Select View"
            SelectedIndexChanged="OnViewChanged">
        <Picker.Items>
            <x:String>Month</x:String>
            <x:String>Year</x:String>
            <x:String>Decade</x:String>
            <x:String>Century</x:String>
        </Picker.Items>
    </Picker>
    
    <calendar:SfCalendar x:Name="calendar" />
</StackLayout>
```

```csharp
private void OnViewChanged(object sender, EventArgs e)
{
    calendar.View = viewPicker.SelectedIndex switch
    {
        0 => CalendarView.Month,
        1 => CalendarView.Year,
        2 => CalendarView.Decade,
        3 => CalendarView.Century,
        _ => CalendarView.Month
    };
}
```

### Cycle Through Views

```csharp
private void CycleToNextView()
{
    calendar.View = calendar.View switch
    {
        CalendarView.Month => CalendarView.Year,
        CalendarView.Year => CalendarView.Decade,
        CalendarView.Decade => CalendarView.Century,
        CalendarView.Century => CalendarView.Month,
        _ => CalendarView.Month
    };
}
```

## Allow View Navigation

The `AllowViewNavigation` property controls whether users can navigate between views by tapping on cells or the header.

**Default:** `true`

### When AllowViewNavigation = true

Users can navigate through views automatically:
- **Century View:** Tap decade → Decade view
- **Decade View:** Tap year → Year view
- **Year View:** Tap month → Month view
- **Month View:** Tap header → Year view (then Decade, Century)

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar" 
                     View="Month"
                     AllowViewNavigation="True" />
```

This is the default behavior and provides intuitive drill-down navigation.

### When AllowViewNavigation = false

Users cannot navigate views by tapping. They can only select dates/cells in the current view.

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar" 
                     View="Year"
                     AllowViewNavigation="False"
                     SelectionMode="Single" />
```

**Use Case:** When you want users to select a month/year/decade without drilling down to individual dates.

```csharp
// Allow month selection without navigating to dates
calendar.View = CalendarView.Year;
calendar.AllowViewNavigation = false;

calendar.SelectionChanged += (s, e) =>
{
    if (e.NewValue is DateTime selectedDate)
    {
        int month = selectedDate.Month;
        int year = selectedDate.Year;
        Console.WriteLine($"User selected: {month}/{year}");
    }
};
```

## Navigate to Adjacent Months

The `NavigateToAdjacentMonth` property enables navigation to the previous or next month by tapping on leading or trailing dates.

**Default:** `false`

### Enable Adjacent Month Navigation

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar" 
                     View="Month"
                     NavigateToAdjacentMonth="True" />
```

**C#:**
```csharp
calendar.NavigateToAdjacentMonth = true;
```

### Behavior

When enabled:
- **Tap trailing date (from previous month):** Calendar navigates to the previous month
- **Tap leading date (from next month):** Calendar navigates to the next month

### Example

```
Current view: March 2026
┌────────────────────────────┐
│ 23 24 25 26 27 28  1  ← Tap "1" (April 1)
│  2  3  4  5  6  7  8
│  9 10 11 12 13 14 15
│ ...
└────────────────────────────┘

Result: Calendar navigates to April 2026
```

### Use Cases

- **Quick navigation:** Users can jump months by clicking adjacent dates
- **Date range selection:** When selecting ranges across month boundaries
- **Calendar browsing:** Natural browsing experience without using navigation arrows

## Swipe Navigation

Users can swipe left/right to navigate between months (in Month view) or years (in Year/Decade/Century views).

### Swipe Behavior

**Month View:**
- **Swipe Left:** Navigate to next month
- **Swipe Right:** Navigate to previous month

**Year View:**
- **Swipe Left:** Navigate to next year
- **Swipe Right:** Navigate to previous year

**Decade/Century Views:**
- **Swipe Left:** Navigate to next decade/century
- **Swipe Right:** Navigate to previous decade/century

### Swipe Restrictions

Swipe navigation respects date restrictions:
- Cannot swipe past `MinimumDate`
- Cannot swipe past `MaximumDate`

```csharp
calendar.MinimumDate = new DateTime(2026, 1, 1);
calendar.MaximumDate = new DateTime(2026, 12, 31);

// User can only swipe within 2026
// Swipes beyond January or December 2026 are ignored
```

## Header Tap Navigation

When `AllowViewNavigation` is enabled, users can tap the calendar header to navigate up through view hierarchies.

### Header Navigation Flow

**Month View → Year View:**
```
┌──────────────────────┐
│   March 2026  ↑      │ ← Tap header
│                       │
│  Su Mo Tu We Th Fr Sa │
└──────────────────────┘
         ↓
┌──────────────────────┐
│     2026       ↑      │
│ Jan  Feb  Mar  Apr   │
│ May  Jun  Jul  Aug   │
│ Sep  Oct  Nov  Dec   │
└──────────────────────┘
```

**Year View → Decade View:**
```
┌──────────────────────┐
│     2026       ↑      │ ← Tap header
│ Jan  Feb  Mar  Apr   │
└──────────────────────┘
         ↓
┌──────────────────────┐
│   2020-2031    ↑      │
│ 2020 2021 2022 2023  │
│ 2024 2025 2026 2027  │
│ 2028 2029 2030 2031  │
└──────────────────────┘
```

**Decade View → Century View:**
```
┌──────────────────────┐
│   2020-2031    ↑      │ ← Tap header
└──────────────────────┘
         ↓
┌──────────────────────┐
│ 1990-2101            │
│ 1990-1999 2000-2009  │
│ 2010-2019 2020-2029  │
│ 2030-2039 2040-2049  │
└──────────────────────┘
```

### Disable Header Navigation

Set `AllowViewNavigation` to `false` to disable header tap navigation:

```csharp
calendar.AllowViewNavigation = false;
// Header taps no longer navigate views
```

## Navigation Examples

### Example 1: Custom Navigation Bar

```xml
<StackLayout>
    <Grid ColumnDefinitions="Auto,*,Auto" Margin="10">
        <Button Grid.Column="0" Text="◀" Clicked="OnPrevious" />
        <Label Grid.Column="1" x:Name="dateLabel" 
               HorizontalOptions="Center" VerticalOptions="Center"
               Text="{Binding DisplayDate, Source={x:Reference calendar}, StringFormat='{0:MMMM yyyy}'}" />
        <Button Grid.Column="2" Text="▶" Clicked="OnNext" />
    </Grid>
    
    <calendar:SfCalendar x:Name="calendar" View="Month" />
</StackLayout>
```

```csharp
private void OnPrevious(object sender, EventArgs e)
{
    calendar.DisplayDate = calendar.DisplayDate.AddMonths(-1);
}

private void OnNext(object sender, EventArgs e)
{
    calendar.DisplayDate = calendar.DisplayDate.AddMonths(1);
}
```

### Example 2: Jump to Specific Month

```csharp
private void JumpToMonth(int month, int year)
{
    calendar.DisplayDate = new DateTime(year, month, 1);
    calendar.View = CalendarView.Month;
}

// Jump to December 2026
JumpToMonth(12, 2026);
```

### Example 3: Year Selector Without Date Selection

```csharp
calendar.View = CalendarView.Decade;
calendar.AllowViewNavigation = false;
calendar.SelectionMode = CalendarSelectionMode.Single;

calendar.SelectionChanged += (s, e) =>
{
    if (e.NewValue is DateTime selectedDate)
    {
        int selectedYear = selectedDate.Year;
        // Process selected year
        ProcessYear(selectedYear);
    }
};
```

## Best Practices

1. **Set Initial DisplayDate:** Navigate to relevant dates when calendar opens
2. **Respect Date Restrictions:** Navigation respects MinimumDate/MaximumDate
3. **Combine with Selection:** Use DisplayDate to show context around selected dates
4. **Provide Navigation Controls:** Add custom buttons for non-touch devices
5. **Use AllowViewNavigation Wisely:** Enable for date selection, disable for month/year pickers
