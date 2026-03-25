# Date Restrictions in .NET MAUI Calendar

The SfCalendar provides powerful date restriction capabilities to control which dates users can select or navigate to. This guide covers all date restriction properties and validation techniques.

## Overview

Date restrictions allow you to:
- Set minimum and maximum selectable dates
- Disable past dates
- Disable specific dates using custom logic
- Prevent navigation beyond certain date boundaries
- Disable weekends or business-specific dates

## Minimum Date

The `MinimumDate` property restricts backward navigation and date selection. Users cannot select or navigate to dates before the minimum date.

### Set Minimum Date

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar" 
                     View="Month"
                     MinimumDate="2026-01-01" />
```

**C#:**
```csharp
// Set minimum date to 5 days ago
calendar.MinimumDate = DateTime.Now.AddDays(-5);

// Set minimum date to specific date
calendar.MinimumDate = new DateTime(2026, 1, 1);

// Set minimum date to today
calendar.MinimumDate = DateTime.Today;
```

### Behavior

- Dates before `MinimumDate` appear disabled (grayed out)
- Users cannot select dates before `MinimumDate`
- Swipe navigation stops at `MinimumDate`
- Month/Year view navigation is restricted

### Example: Last 30 Days Only

```csharp
// Allow selection only for the last 30 days
calendar.MinimumDate = DateTime.Today.AddDays(-30);
calendar.MaximumDate = DateTime.Today;
```

## Maximum Date

The `MaximumDate` property restricts forward navigation and date selection. Users cannot select or navigate to dates after the maximum date.

### Set Maximum Date

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar" 
                     View="Month"
                     MaximumDate="2026-12-31" />
```

**C#:**
```csharp
// Set maximum date to 5 days from now
calendar.MaximumDate = DateTime.Now.AddDays(5);

// Set maximum date to end of current year
calendar.MaximumDate = new DateTime(DateTime.Today.Year, 12, 31);

// Set maximum date to 90 days from today
calendar.MaximumDate = DateTime.Today.AddDays(90);
```

### Behavior

- Dates after `MaximumDate` appear disabled (grayed out)
- Users cannot select dates after `MaximumDate`
- Swipe navigation stops at `MaximumDate`
- Month/Year view navigation is restricted

### Example: Next 60 Days Only

```csharp
// Allow selection only for the next 60 days
calendar.MinimumDate = DateTime.Today;
calendar.MaximumDate = DateTime.Today.AddDays(60);
```

## Enable Past Dates

The `EnablePastDates` property controls whether dates before today can be selected.

**Default:** `true` (past dates are enabled)

### Disable Past Dates

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar" 
                     View="Month"
                     EnablePastDates="False" />
```

**C#:**
```csharp
// Disable dates before today
calendar.EnablePastDates = false;
```

### Behavior

When `EnablePastDates = false`:
- All dates before today appear disabled
- Users can only select today or future dates
- Equivalent to setting `MinimumDate = DateTime.Today`

### Example: Appointment Booking (Future Dates Only)

```csharp
// Only allow future appointment bookings
calendar.EnablePastDates = false;
calendar.SelectionMode = CalendarSelectionMode.Single;
```

## Selectable Day Predicate

The `SelectableDayPredicate` property allows custom date validation logic. It's a function that returns `true` if a date is selectable, or `false` if it should be disabled.

**Signature:**
```csharp
public Func<DateTime, bool> SelectableDayPredicate { get; set; }
```

### Basic Usage

```csharp
calendar.SelectableDayPredicate = (DateTime date) =>
{
    // Return true if date is selectable, false to disable
    return date.DayOfWeek != DayOfWeek.Sunday;
};
```

### Example 1: Disable Weekends

```csharp
calendar.SelectableDayPredicate = (DateTime date) =>
{
    // Disable Saturdays and Sundays
    return date.DayOfWeek != DayOfWeek.Saturday && 
           date.DayOfWeek != DayOfWeek.Sunday;
};
```

### Example 2: Disable Specific Dates

```csharp
// List of holidays or unavailable dates
private List<DateTime> disabledDates = new List<DateTime>
{
    new DateTime(2026, 12, 25), // Christmas
    new DateTime(2026, 1, 1),   // New Year
    new DateTime(2026, 7, 4)    // Independence Day
};

calendar.SelectableDayPredicate = (DateTime date) =>
{
    // Disable specific dates
    return !disabledDates.Contains(date.Date);
};
```

### Example 3: Business Days Only

```csharp
calendar.SelectableDayPredicate = (DateTime date) =>
{
    // Enable Monday-Friday only
    return date.DayOfWeek >= DayOfWeek.Monday && 
           date.DayOfWeek <= DayOfWeek.Friday;
};
```

### Example 4: Every Other Day

```csharp
calendar.SelectableDayPredicate = (DateTime date) =>
{
    // Enable only even-numbered days
    return date.Day % 2 == 0;
};
```

### Example 5: Complex Business Logic

```csharp
private HashSet<DateTime> holidays = LoadHolidays();
private HashSet<DateTime> fullyBookedDates = LoadBookedDates();

calendar.SelectableDayPredicate = (DateTime date) =>
{
    // Disable weekends
    if (date.DayOfWeek == DayOfWeek.Saturday || date.DayOfWeek == DayOfWeek.Sunday)
        return false;
    
    // Disable holidays
    if (holidays.Contains(date.Date))
        return false;
    
    // Disable fully booked dates
    if (fullyBookedDates.Contains(date.Date))
        return false;
    
    // Enable all other dates
    return true;
};
```

## Combining Restrictions

You can combine multiple restriction properties for comprehensive date control.

### Example 1: Limited Date Range with Weekend Restriction

```csharp
// Allow selection only for next 30 business days
calendar.MinimumDate = DateTime.Today;
calendar.MaximumDate = DateTime.Today.AddDays(30);
calendar.SelectableDayPredicate = (DateTime date) =>
{
    // Disable weekends within the allowed range
    return date.DayOfWeek != DayOfWeek.Saturday && 
           date.DayOfWeek != DayOfWeek.Sunday;
};
```

### Example 2: Booking System with Multiple Restrictions

```csharp
calendar.EnablePastDates = false; // No past bookings
calendar.MaximumDate = DateTime.Today.AddDays(90); // 90-day booking window

calendar.SelectableDayPredicate = (DateTime date) =>
{
    // Disable Sundays (maintenance day)
    if (date.DayOfWeek == DayOfWeek.Sunday)
        return false;
    
    // Disable specific blocked dates
    if (IsBlockedDate(date))
        return false;
    
    return true;
};
```

### Example 3: Range Restriction with Custom Validation

```xml
<calendar:SfCalendar x:Name="calendar"
                     SelectionMode="Range"
                     EnablePastDates="False"
                     MinimumDate="{Binding TodayDate}"
                     MaximumDate="{Binding MaxBookingDate}" />
```

```csharp
calendar.SelectableDayPredicate = (DateTime date) =>
{
    // Only allow weekdays for range bookings
    return date.DayOfWeek != DayOfWeek.Saturday && 
           date.DayOfWeek != DayOfWeek.Sunday;
};
```

## Styling Disabled Dates

Disabled dates can be styled using `DisabledDatesTextStyle` and `DisabledDatesBackground` in the MonthView. See the Customization documentation for details.

```csharp
calendar.MonthView = new CalendarMonthView
{
    DisabledDatesTextStyle = new CalendarTextStyle
    {
        TextColor = Colors.Gray,
        FontSize = 12
    },
    DisabledDatesBackground = Colors.LightGray.WithAlpha(0.3f)
};
```

## Important Notes

### SelectableDayPredicate Called on View Change

The `SelectableDayPredicate` function is called whenever the calendar view changes (swipe, view navigation, etc.). Ensure your predicate function is:
- **Fast:** Avoid expensive operations (database calls, complex calculations)
- **Consistent:** Same input should always return same output
- **Thread-safe:** May be called from different threads

### Predicate Performance Tips

```csharp
// ✅ GOOD: Use HashSet for fast lookups
private HashSet<DateTime> disabledDates = new HashSet<DateTime>(holidays);

calendar.SelectableDayPredicate = (DateTime date) =>
{
    return !disabledDates.Contains(date.Date); // O(1) lookup
};

// ❌ BAD: Linear search through list
private List<DateTime> disabledDates = new List<DateTime>(holidays);

calendar.SelectableDayPredicate = (DateTime date) =>
{
    return !disabledDates.Contains(date.Date); // O(n) lookup
};
```

### Order of Evaluation

Date restrictions are evaluated in this order:
1. `MinimumDate` check
2. `MaximumDate` check  
3. `EnablePastDates` check (if false)
4. `SelectableDayPredicate` check

If any check fails, the date is disabled.

## Best Practices

1. **Use MinimumDate/MaximumDate for Simple Ranges:** These are more performant than SelectableDayPredicate

2. **Optimize SelectableDayPredicate:** Use HashSet for date lookups, avoid complex logic

3. **Provide Visual Feedback:** Style disabled dates clearly so users understand restrictions

4. **Consider Business Logic:** Align restrictions with real-world constraints (booking windows, availability)

5. **Test Edge Cases:** Verify behavior at date boundaries (month/year transitions, leap years)

6. **Update Dynamically:** Update restrictions when underlying data changes (bookings, availability)

```csharp
// Example: Update disabled dates when bookings change
private void OnBookingsUpdated()
{
    fullyBookedDates = LoadBookedDates();
    
    // Force calendar to re-evaluate dates
    calendar.DisplayDate = calendar.DisplayDate;
}
```
