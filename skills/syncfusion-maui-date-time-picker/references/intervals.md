# Date and Time Intervals

## Table of Contents
- [Overview](#overview)
- [Date Intervals](#date-intervals)
- [Time Intervals](#time-intervals)
- [Combining Intervals](#combining-intervals)
- [Common Interval Patterns](#common-interval-patterns)
- [Use Cases](#use-cases)

## Overview

The DateTimePicker provides interval properties that control the increment steps for date and time values. Instead of showing every single day, hour, or minute, you can show values at specific intervals.

**Available Interval Properties:**
- **Date intervals**: `DayInterval`, `MonthInterval`, `YearInterval`
- **Time intervals**: `HourInterval`, `MinuteInterval`, `SecondInterval`, `MilliSecondInterval`

**Default**: All intervals are `1` (show every value)

## Date Intervals

### Day Interval

The `DayInterval` property sets the increment between day values in the picker.

**XAML:**
```xaml
<picker:SfDateTimePicker 
    x:Name="picker"
    DayInterval="2" />
```

**C#:**
```csharp
var picker = new SfDateTimePicker
{
    DayInterval = 2  // Skip every other day: 1, 3, 5, 7, etc.
};
```

**Result**: Shows days 1, 3, 5, 7, 9... (skip even-numbered days)

**Use Case**: Bi-weekly selection, odd/even day scheduling

### Month Interval

The `MonthInterval` property sets the increment between month values.

**XAML:**
```xaml
<picker:SfDateTimePicker 
    x:Name="picker"
    MonthInterval="2" />
```

**C#:**
```csharp
var picker = new SfDateTimePicker
{
    MonthInterval = 2  // Show every other month: Jan, Mar, May, etc.
};
```

**Result**: Shows Jan, Mar, May, Jul, Sep, Nov

**Use Case**: Quarterly reporting, bi-monthly billing

### Year Interval

The `YearInterval` property sets the increment between year values.

**XAML:**
```xaml
<picker:SfDateTimePicker 
    x:Name="picker"
    YearInterval="2" />
```

**C#:**
```csharp
var picker = new SfDateTimePicker
{
    YearInterval = 2  // Show every other year: 2020, 2022, 2024, etc.
};
```

**Result**: Shows 2020, 2022, 2024, 2026...

**Use Case**: Olympic years, biennial events

### Every Fifth Day

```csharp
var picker = new SfDateTimePicker
{
    DayInterval = 5  // Show days: 5, 10, 15, 20, 25, 30
};
```

### Quarterly Months

```csharp
var picker = new SfDateTimePicker
{
    MonthInterval = 3  // Show Jan, Apr, Jul, Oct
};
```

## Time Intervals

### Hour Interval

The `HourInterval` property sets the increment between hour values.

**XAML:**
```xaml
<picker:SfDateTimePicker 
    x:Name="picker"
    HourInterval="2" />
```

**C#:**
```csharp
var picker = new SfDateTimePicker
{
    HourInterval = 2  // Show every 2 hours: 12 AM, 2 AM, 4 AM, etc.
};
```

**Result**: Shows 12 AM, 2 AM, 4 AM, 6 AM, 8 AM, 10 AM, 12 PM, 2 PM...

**Use Case**: Appointment slots every 2 hours

### Minute Interval

The `MinuteInterval` property sets the increment between minute values.

**XAML:**
```xaml
<picker:SfDateTimePicker 
    x:Name="picker"
    MinuteInterval="15" />
```

**C#:**
```csharp
var picker = new SfDateTimePicker
{
    MinuteInterval = 15  // Show every 15 minutes: 00, 15, 30, 45
};
```

**Result**: Shows 00, 15, 30, 45

**Use Case**: Common appointment intervals (15-minute slots)

### Second Interval

The `SecondInterval` property sets the increment between second values.

**XAML:**
```xaml
<picker:SfDateTimePicker 
    x:Name="picker"
    SecondInterval="10" />
```

**C#:**
```csharp
var picker = new SfDateTimePicker
{
    SecondInterval = 10  // Show every 10 seconds: 00, 10, 20, 30, 40, 50
};
```

**Result**: Shows 00, 10, 20, 30, 40, 50

**Use Case**: Countdown timers, event timing

### MilliSecond Interval

The `MilliSecondInterval` property sets the increment between millisecond values.

**XAML:**
```xaml
<picker:SfDateTimePicker 
    x:Name="picker"
    MilliSecondInterval="100" />
```

**C#:**
```csharp
var picker = new SfDateTimePicker
{
    MilliSecondInterval = 100  // Show every 100ms: 0, 100, 200, ...
};
```

**Use Case**: High-precision timing, sports timing

## Combining Intervals

You can combine multiple intervals to create sophisticated selection patterns.

### Example 1: Bi-weekly Appointments Every 30 Minutes

```csharp
var picker = new SfDateTimePicker
{
    DayInterval = 14,      // Every 2 weeks
    HourInterval = 1,      // Every hour
    MinuteInterval = 30,   // Every 30 minutes: 00, 30
    TimeFormat = PickerTimeFormat.h_mm_tt
};
```

**Result**: Every 14th day, hours 1-12, minutes 00 and 30

### Example 2: Quarterly Reports on First Day

```csharp
var picker = new SfDateTimePicker
{
    DayInterval = 1,       // First day only (use MinDate/MaxDate to enforce)
    MonthInterval = 3,     // Quarterly: Jan, Apr, Jul, Oct
    YearInterval = 1
};
```

### Example 3: Hourly Appointments

```csharp
var picker = new SfDateTimePicker
{
    HourInterval = 1,      // Every hour
    MinuteInterval = 60,   // Hide minutes (only show :00)
    SecondInterval = 60,   // Hide seconds
    TimeFormat = PickerTimeFormat.h_mm_tt
};
```

### Example 4: Half-Hour Slots

```csharp
var picker = new SfDateTimePicker
{
    MinuteInterval = 30,   // 00 and 30
    SecondInterval = 60,   // Hide seconds
    TimeFormat = PickerTimeFormat.h_mm_tt
};
```

### Example 5: Every 15 Minutes During Business Hours

```csharp
var picker = new SfDateTimePicker
{
    MinimumDate = DateTime.Today.AddHours(9),   // 9 AM
    MaximumDate = DateTime.Today.AddHours(17),  // 5 PM
    HourInterval = 1,
    MinuteInterval = 15,   // 00, 15, 30, 45
    TimeFormat = PickerTimeFormat.h_mm_tt
};
```

## Common Interval Patterns

### Pattern 1: 15-Minute Appointment Slots

```csharp
var picker = new SfDateTimePicker
{
    MinuteInterval = 15,
    SecondInterval = 60,
    TimeFormat = PickerTimeFormat.h_mm_tt
};
```
**Displays**: 9:00 AM, 9:15 AM, 9:30 AM, 9:45 AM, 10:00 AM...

### Pattern 2: 30-Minute Appointment Slots

```csharp
var picker = new SfDateTimePicker
{
    MinuteInterval = 30,
    SecondInterval = 60,
    TimeFormat = PickerTimeFormat.h_mm_tt
};
```
**Displays**: 9:00 AM, 9:30 AM, 10:00 AM, 10:30 AM...

### Pattern 3: Hourly Slots

```csharp
var picker = new SfDateTimePicker
{
    HourInterval = 1,
    MinuteInterval = 60,  // Only show :00
    SecondInterval = 60,
    TimeFormat = PickerTimeFormat.h_mm_tt
};
```
**Displays**: 9:00 AM, 10:00 AM, 11:00 AM, 12:00 PM...

### Pattern 4: Every 2 Hours

```csharp
var picker = new SfDateTimePicker
{
    HourInterval = 2,
    MinuteInterval = 60,
    SecondInterval = 60,
    TimeFormat = PickerTimeFormat.h_mm_tt
};
```
**Displays**: 8:00 AM, 10:00 AM, 12:00 PM, 2:00 PM, 4:00 PM...

### Pattern 5: Weekly Selection

```csharp
var picker = new SfDateTimePicker
{
    DayInterval = 7,
    DateFormat = PickerDateFormat.dd_MMM_yyyy
};
```
**Displays**: Every 7th day from start date

### Pattern 6: Bi-monthly Selection

```csharp
var picker = new SfDateTimePicker
{
    MonthInterval = 2,
    DateFormat = PickerDateFormat.MMM_yyyy
};
```
**Displays**: Jan, Mar, May, Jul, Sep, Nov

### Pattern 7: 5-Minute Intervals

```csharp
var picker = new SfDateTimePicker
{
    MinuteInterval = 5,
    SecondInterval = 60,
    TimeFormat = PickerTimeFormat.HH_mm
};
```
**Displays**: 00, 05, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55

### Pattern 8: 10-Minute Intervals

```csharp
var picker = new SfDateTimePicker
{
    MinuteInterval = 10,
    SecondInterval = 60,
    TimeFormat = PickerTimeFormat.HH_mm
};
```
**Displays**: 00, 10, 20, 30, 40, 50

## Use Cases

### Use Case 1: Doctor's Office Appointments

```csharp
var appointmentPicker = new SfDateTimePicker
{
    MinimumDate = DateTime.Today.AddDays(1).AddHours(9),  // Tomorrow, 9 AM
    MaximumDate = DateTime.Today.AddDays(30).AddHours(17), // 30 days, 5 PM
    HourInterval = 1,
    MinuteInterval = 15,         // 15-minute slots
    SecondInterval = 60,
    DateFormat = PickerDateFormat.dd_MMM_yyyy,
    TimeFormat = PickerTimeFormat.h_mm_tt
};
```

### Use Case 2: Conference Room Booking

```csharp
var roomPicker = new SfDateTimePicker
{
    MinuteInterval = 30,         // Half-hour slots
    SecondInterval = 60,
    HourInterval = 1,
    MinimumDate = DateTime.Today.AddHours(8),   // 8 AM
    MaximumDate = DateTime.Today.AddHours(18),  // 6 PM
    TimeFormat = PickerTimeFormat.h_mm_tt
};
```

### Use Case 3: Fitness Class Schedule

```csharp
var classPicker = new SfDateTimePicker
{
    DayInterval = 1,
    HourInterval = 2,            // Classes every 2 hours
    MinuteInterval = 60,
    SecondInterval = 60,
    MinimumDate = DateTime.Today.AddHours(6),   // 6 AM
    MaximumDate = DateTime.Today.AddHours(20),  // 8 PM
    TimeFormat = PickerTimeFormat.h_mm_tt
};
```

### Use Case 4: Restaurant Reservations

```csharp
var reservationPicker = new SfDateTimePicker
{
    MinuteInterval = 30,         // 30-minute slots
    SecondInterval = 60,
    MinimumDate = DateTime.Today.AddHours(17),  // Dinner starts at 5 PM
    MaximumDate = DateTime.Today.AddHours(22),  // Last seating at 10 PM
    DateFormat = PickerDateFormat.dd_MMM_yyyy,
    TimeFormat = PickerTimeFormat.h_mm_tt
};
```

### Use Case 5: Car Service Appointment

```csharp
var servicePicker = new SfDateTimePicker
{
    DayInterval = 1,
    HourInterval = 1,
    MinuteInterval = 60,         // Hourly appointments
    SecondInterval = 60,
    MinimumDate = DateTime.Today.AddDays(1).AddHours(8),   // Tomorrow, 8 AM
    MaximumDate = DateTime.Today.AddDays(14).AddHours(17), // 2 weeks, 5 PM
    TimeFormat = PickerTimeFormat.h_mm_tt
};
```

### Use Case 6: Quarterly Business Review

```csharp
var reviewPicker = new SfDateTimePicker
{
    MonthInterval = 3,           // Quarterly
    DayInterval = 1,
    DateFormat = PickerDateFormat.MMMM_yyyy,
    MinimumDate = new DateTime(DateTime.Now.Year, 1, 1),
    MaximumDate = new DateTime(DateTime.Now.Year, 12, 31)
};
```

### Use Case 7: Webinar Scheduling (Every 2 Hours)

```csharp
var webinarPicker = new SfDateTimePicker
{
    HourInterval = 2,
    MinuteInterval = 60,
    SecondInterval = 60,
    TimeFormat = PickerTimeFormat.h_mm_tt,
    MinimumDate = DateTime.Today.AddHours(10),  // 10 AM
    MaximumDate = DateTime.Today.AddHours(18)   // 6 PM
};
```

## Dynamic Interval Changes

Change intervals based on user selection or other conditions:

```csharp
// Switch to 15-minute intervals for morning
private void OnMorningSelected(object sender, EventArgs e)
{
    dateTimePicker.MinuteInterval = 15;
}

// Switch to 30-minute intervals for afternoon
private void OnAfternoonSelected(object sender, EventArgs e)
{
    dateTimePicker.MinuteInterval = 30;
}
```

## Best Practices

1. **User Experience**: Choose intervals that make sense for your use case
2. **Consistency**: Keep intervals consistent throughout your app
3. **Clarity**: Larger intervals = less scrolling, but less precision
4. **Business Logic**: Align intervals with business rules (e.g., 30-min appointments)
5. **Performance**: Smaller intervals may require more scrolling
6. **Combine Wisely**: Use multiple intervals together for fine control
7. **Hide Unused**: Set interval to max value (e.g., 60 for minutes) to effectively hide that component

## Common Combinations

**Standard Appointment**: Day=1, Hour=1, Minute=15, Second=60  
**Hourly Booking**: Day=1, Hour=1, Minute=60, Second=60  
**Half-Day Slots**: Day=1, Hour=12, Minute=60, Second=60  
**Weekly Selection**: Day=7, Month=1, Year=1  
**Quarterly Meetings**: Day=1, Month=3, Year=1
