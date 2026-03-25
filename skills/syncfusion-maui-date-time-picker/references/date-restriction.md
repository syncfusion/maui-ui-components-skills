# Date Restrictions

## Table of Contents
- [Overview](#overview)
- [Minimum Date](#minimum-date)
- [Maximum Date](#maximum-date)
- [Blackout DateTimes](#blackout-datetimes)
- [Validation Patterns](#validation-patterns)
- [Use Cases](#use-cases)

## Overview

The DateTimePicker provides powerful date and time restriction features to control user selection. You can set minimum and maximum bounds and block specific dates or times from being selected.

**Key Properties:**
- `MinimumDate` - Earliest selectable date/time
- `MaximumDate` - Latest selectable date/time
- `BlackoutDateTimes` - List of blocked dates and times

## Minimum Date

The `MinimumDate` property restricts selection to dates and times on or after the specified value. Users cannot select any date/time before this limit.

**Important**: `MinimumDate` value must be less than `MaximumDate`.

### Basic Usage

**XAML:**
```xaml
<picker:SfDateTimePicker 
    x:Name="picker"
    MinimumDate="2024-01-01 08:00:00" />
```

**C#:**
```csharp
var picker = new SfDateTimePicker
{
    MinimumDate = new DateTime(2024, 1, 1, 8, 0, 0)
};
```

### Set to Today

Prevent selection of past dates:

```xaml
<picker:SfDateTimePicker 
    MinimumDate="{x:Static sys:DateTime.Now}" />
```

```csharp
var picker = new SfDateTimePicker
{
    MinimumDate = DateTime.Now
};
```

### Set to Start of Today

Allow selection from midnight today:

```csharp
var picker = new SfDateTimePicker
{
    MinimumDate = DateTime.Today // Midnight of current day
};
```

### Business Hours Example

Restrict to business hours starting tomorrow:

```csharp
var picker = new SfDateTimePicker
{
    MinimumDate = DateTime.Today.AddDays(1).AddHours(9), // 9 AM tomorrow
    TimeFormat = PickerTimeFormat.h_mm_tt
};
```

### Data Binding Minimum Date

```xaml
<picker:SfDateTimePicker 
    MinimumDate="{Binding EarliestAllowedDate}" />
```

```csharp
public class AppointmentViewModel
{
    public DateTime EarliestAllowedDate { get; set; } = DateTime.Today;
}
```

## Maximum Date

The `MaximumDate` property restricts selection to dates and times on or before the specified value. Users cannot select any date/time after this limit.

### Basic Usage

**XAML:**
```xaml
<picker:SfDateTimePicker 
    x:Name="picker"
    MaximumDate="2024-12-31 23:59:59" />
```

**C#:**
```csharp
var picker = new SfDateTimePicker
{
    MaximumDate = new DateTime(2024, 12, 31, 23, 59, 59)
};
```

### Set Relative Maximum

**30 Days from Now:**
```csharp
var picker = new SfDateTimePicker
{
    MaximumDate = DateTime.Now.AddDays(30)
};
```

**End of This Year:**
```csharp
var picker = new SfDateTimePicker
{
    MaximumDate = new DateTime(DateTime.Now.Year, 12, 31)
};
```

**One Year from Today:**
```csharp
var picker = new SfDateTimePicker
{
    MaximumDate = DateTime.Today.AddYears(1)
};
```

### Data Binding Maximum Date

```xaml
<picker:SfDateTimePicker 
    MaximumDate="{Binding LatestAllowedDate}" />
```

```csharp
public class BookingViewModel
{
    public DateTime LatestAllowedDate { get; set; } = DateTime.Today.AddMonths(3);
}
```

## Min and Max Together

Combine minimum and maximum to create a valid date range:

### Example 1: Booking Window

Allow bookings only for the next 3 months:

```xaml
<picker:SfDateTimePicker 
    MinimumDate="{x:Static sys:DateTime.Today}"
    MaximumDate="{Binding ThreeMonthsFromNow}" />
```

```csharp
public DateTime ThreeMonthsFromNow => DateTime.Today.AddMonths(3);
```

### Example 2: Historical Date Range

Select dates from last year only:

```csharp
var picker = new SfDateTimePicker
{
    MinimumDate = new DateTime(DateTime.Now.Year - 1, 1, 1),
    MaximumDate = new DateTime(DateTime.Now.Year - 1, 12, 31)
};
```

### Example 3: Working Hours This Week

```csharp
var picker = new SfDateTimePicker
{
    MinimumDate = DateTime.Today.AddHours(8), // 8 AM today
    MaximumDate = DateTime.Today.AddDays(5).AddHours(17), // 5 PM Friday
    HourInterval = 1
};
```

## Blackout DateTimes

The `BlackoutDateTimes` property allows you to block specific dates and times from selection. This is useful for blocking holidays, weekends, unavailable time slots, or maintenance periods.

**Features:**
- Block entire days
- Block specific time slots within days
- Multiple blocked periods
- Useful for availability calendars

### Block Entire Dates

Block specific dates (e.g., holidays):

```xaml
<picker:SfDateTimePicker x:Name="picker">
    <picker:SfDateTimePicker.BlackoutDateTimes>
        <x:Array Type="{x:Type sys:DateTime}">
            <sys:DateTime>2024-12-25</sys:DateTime>  <!-- Christmas -->
            <sys:DateTime>2024-01-01</sys:DateTime>  <!-- New Year -->
            <sys:DateTime>2024-07-04</sys:DateTime>  <!-- Independence Day -->
        </x:Array>
    </picker:SfDateTimePicker.BlackoutDateTimes>
</picker:SfDateTimePicker>
```

### Block Specific Time Slots

Block particular hours on specific days:

```xaml
<picker:SfDateTimePicker>
    <picker:SfDateTimePicker.BlackoutDateTimes>
        <x:Array Type="{x:Type sys:DateTime}">
            <sys:DateTime>2024-08-15 12:00:00</sys:DateTime>
            <sys:DateTime>2024-08-15 13:00:00</sys:DateTime>
            <sys:DateTime>2024-08-15 14:00:00</sys:DateTime>
        </x:Array>
    </picker:SfDateTimePicker.BlackoutDateTimes>
</picker:SfDateTimePicker>
```

### C# Blackout List

```csharp
var picker = new SfDateTimePicker();

var blackoutDates = new List<DateTime>
{
    new DateTime(2024, 12, 25), // Christmas
    new DateTime(2024, 12, 26), // Boxing Day
    new DateTime(2024, 1, 1),   // New Year
    new DateTime(2024, 7, 4)    // Independence Day
};

picker.BlackoutDateTimes = blackoutDates;
```

### Dynamic Blackout Dates

Block weekends:

```csharp
var blackoutDates = new List<DateTime>();
var startDate = DateTime.Today;
var endDate = DateTime.Today.AddMonths(3);

for (var date = startDate; date <= endDate; date = date.AddDays(1))
{
    if (date.DayOfWeek == DayOfWeek.Saturday || date.DayOfWeek == DayOfWeek.Sunday)
    {
        blackoutDates.Add(date);
    }
}

picker.BlackoutDateTimes = blackoutDates;
```

### Block Lunch Hours

Block 12-2 PM every day for a month:

```csharp
var blackoutTimes = new List<DateTime>();
var startDate = DateTime.Today;

for (int day = 0; day < 30; day++)
{
    var currentDate = startDate.AddDays(day);
    
    // Block 12 PM to 2 PM
    blackoutTimes.Add(currentDate.AddHours(12));
    blackoutTimes.Add(currentDate.AddHours(13));
}

picker.BlackoutDateTimes = blackoutTimes;
```

### Data Binding Blackout Dates

```xaml
<picker:SfDateTimePicker 
    BlackoutDateTimes="{Binding UnavailableDates}" />
```

```csharp
public class BookingViewModel : INotifyPropertyChanged
{
    public List<DateTime> UnavailableDates { get; set; } = new List<DateTime>
    {
        new DateTime(2024, 12, 25),
        new DateTime(2024, 1, 1)
    };
}
```

## Validation Patterns

### Pattern 1: Future Dates Only

```csharp
var picker = new SfDateTimePicker
{
    MinimumDate = DateTime.Now,
    MaximumDate = DateTime.Now.AddYears(1)
};
```

### Pattern 2: Past Dates Only (Date of Birth)

```csharp
var picker = new SfDateTimePicker
{
    MinimumDate = DateTime.Today.AddYears(-120), // 120 years ago
    MaximumDate = DateTime.Today.AddYears(-18)   // Must be 18+
};
```

### Pattern 3: Business Days Only

```csharp
var picker = new SfDateTimePicker
{
    MinimumDate = DateTime.Today
};

// Block weekends
var blackoutDates = new List<DateTime>();
for (var date = DateTime.Today; date <= DateTime.Today.AddMonths(3); date = date.AddDays(1))
{
    if (date.DayOfWeek == DayOfWeek.Saturday || date.DayOfWeek == DayOfWeek.Sunday)
    {
        blackoutDates.Add(date);
    }
}
picker.BlackoutDateTimes = blackoutDates;
```

### Pattern 4: Working Hours Only

```csharp
var picker = new SfDateTimePicker
{
    MinimumDate = DateTime.Today.AddHours(9),  // 9 AM
    MaximumDate = DateTime.Today.AddHours(17), // 5 PM
    HourInterval = 1,
    MinuteInterval = 30
};
```

### Pattern 5: Appointment Window (2 weeks advance booking)

```csharp
var picker = new SfDateTimePicker
{
    MinimumDate = DateTime.Today.AddDays(days: 1), // Tomorrow
    MaximumDate = DateTime.Today.AddDays(14)       // 2 weeks out
};
```

## Use Cases

### Use Case 1: Hotel Booking

```csharp
var checkInPicker = new SfDateTimePicker
{
    MinimumDate = DateTime.Today.AddDays(1),     // Check-in tomorrow or later
    MaximumDate = DateTime.Today.AddYears(1),    // Up to 1 year advance
    DateFormat = PickerDateFormat.dd_MMM_yyyy
};

var checkOutPicker = new SfDateTimePicker
{
    MinimumDate = DateTime.Today.AddDays(2),     // Check-out min 1 day after
    MaximumDate = DateTime.Today.AddYears(1).AddDays(1),
    DateFormat = PickerDateFormat.dd_MMM_yyyy
};
```

### Use Case 2: Doctor Appointment Scheduling

```csharp
var appointmentPicker = new SfDateTimePicker
{
    MinimumDate = DateTime.Today.AddDays(1).AddHours(9), // Tomorrow at 9 AM
    MaximumDate = DateTime.Today.AddDays(30).AddHours(17), // 30 days, up to 5 PM
    HourInterval = 1,
    MinuteInterval = 30,
    TimeFormat = PickerTimeFormat.h_mm_tt
};

// Block lunch hour (12-1 PM) for all days
var blackoutTimes = new List<DateTime>();
for (int day = 1; day <= 30; day++)
{
    var date = DateTime.Today.AddDays(day);
    blackoutTimes.Add(date.AddHours(12)); // Noon
    blackoutTimes.Add(date.AddHours(12).AddMinutes(30)); // 12:30 PM
}
appointmentPicker.BlackoutDateTimes = blackoutTimes;
```

### Use Case 3: Event Registration Deadline

```csharp
var registrationPicker = new SfDateTimePicker
{
    MinimumDate = DateTime.Now,
    MaximumDate = new DateTime(2024, 8, 15, 23, 59, 59), // Event deadline
    DateFormat = PickerDateFormat.dd_MMMM_yyyy,
    TimeFormat = PickerTimeFormat.h_mm_tt
};
```

### Use Case 4: Age Verification (Must be 18+)

```csharp
var dobPicker = new SfDateTimePicker
{
    MinimumDate = DateTime.Today.AddYears(-120),
    MaximumDate = DateTime.Today.AddYears(-18), // Must be at least 18
    DateFormat = PickerDateFormat.dd_MM_yyyy
};
```

### Use Case 5: Maintenance Window

```csharp
var maintenancePicker = new SfDateTimePicker
{
    MinimumDate = DateTime.Today,
    MaximumDate = DateTime.Today.AddDays(7)
};

// Block business hours (9 AM - 5 PM)
var blackoutTimes = new List<DateTime>();
for (int day = 0; day < 7; day++)
{
    var date = DateTime.Today.AddDays(day);
    for (int hour = 9; hour <= 17; hour++)
    {
        blackoutTimes.Add(date.AddHours(hour));
    }
}
maintenancePicker.BlackoutDateTimes = blackoutTimes;
```

## Important Notes

1. **Validation**: MinimumDate must be less than MaximumDate
2. **BlackoutDateTimes**: Can include both entire dates and specific times
3. **User Experience**: Provide clear messages about restrictions
4. **Performance**: Large blackout lists may impact performance
5. **Selection Behavior**: Users cannot select blocked dates/times
6. **Default Values**: Ensure SelectedDate is within allowed range

## Best Practices

1. **Clear Communication**: Inform users about date restrictions
2. **Reasonable Ranges**: Don't make ranges too restrictive
3. **Dynamic Updates**: Update restrictions based on context
4. **Validation Feedback**: Show helpful messages for invalid selections
5. **Business Logic**: Align restrictions with business rules
6. **Testing**: Test edge cases (min/max boundaries, blackout conflicts)
