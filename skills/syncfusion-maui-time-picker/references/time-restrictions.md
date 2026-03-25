# Time Restrictions in .NET MAUI TimePicker

## Table of Contents
- [Overview](#overview)
- [MinimumTime Property](#minimumtime-property)
- [MaximumTime Property](#maximumtime-property)
- [BlackoutTimes Property](#blackouttimes-property)
- [Combining Restrictions](#combining-restrictions)
- [Common Use Cases](#common-use-cases)
- [Best Practices](#best-practices)

Time restrictions allow you to limit which time values users can select. The TimePicker provides three restriction mechanisms: minimum time, maximum time, and blackout times.

## Overview

**Available Restriction Properties:**
- `MinimumTime` - Earliest selectable time
- `MaximumTime` - Latest selectable time
- `BlackoutTimes` - Collection of specific times to disable

These restrictions are essential for implementing business logic such as:
- Office hours (9 AM - 5 PM)
- Appointment availability
- Blocked time slots
- Time range validation

## MinimumTime Property

The `MinimumTime` property sets the earliest time that can be selected. Times before this value will be disabled.

**Property:**
```csharp
public TimeSpan MinimumTime { get; set; }
```

**Type:** `TimeSpan`

**Default:** `TimeSpan.Zero` (00:00:00)

**Constraint:** Must be less than `MaximumTime`

### Basic MinimumTime Example

**XAML:**
```xml
<picker:SfTimePicker x:Name="timePicker"
                     MinimumTime="07:00:00"
                     Format="hh_mm_tt">
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView Text="Select Time" Height="40" />
    </picker:SfTimePicker.HeaderView>
</picker:SfTimePicker>
```

**C#:**
```csharp
SfTimePicker timePicker = new SfTimePicker()
{
    MinimumTime = new TimeSpan(7, 0, 0),  // 7:00 AM
    Format = PickerTimeFormat.hh_mm_tt
};

this.Content = timePicker;
```

**Result:** Users cannot select times before 7:00 AM

### Example: Business Hours Start Time

```xml
<picker:SfTimePicker x:Name="workStartPicker"
                     MinimumTime="09:00:00"
                     MaximumTime="17:00:00"
                     Format="hh_mm_tt"
                     MinuteInterval="15"
                     SelectedTime="09:00:00">
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView Text="Work Start Time" Height="40" />
    </picker:SfTimePicker.HeaderView>
</picker:SfTimePicker>
```

**Available Times:** 9:00 AM to 5:00 PM only

### Setting MinimumTime Programmatically

```csharp
// Set minimum time to 8:30 AM
timePicker.MinimumTime = new TimeSpan(8, 30, 0);

// Set minimum time to current time (for future time selection)
timePicker.MinimumTime = DateTime.Now.TimeOfDay;

// Set minimum time from a DateTime object
DateTime minDateTime = new DateTime(2026, 1, 1, 10, 0, 0);
timePicker.MinimumTime = minDateTime.TimeOfDay;
```

## MaximumTime Property

The `MaximumTime` property sets the latest time that can be selected. Times after this value will be disabled.

**Property:**
```csharp
public TimeSpan MaximumTime { get; set; }
```

**Type:** `TimeSpan`

**Default:** `new TimeSpan(23, 59, 59)` (23:59:59)

**Constraint:** Must be greater than `MinimumTime`

### Basic MaximumTime Example

**XAML:**
```xml
<picker:SfTimePicker x:Name="timePicker"
                     MaximumTime="20:00:00"
                     Format="hh_mm_tt">
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView Text="Select Time" Height="40" />
    </picker:SfTimePicker.HeaderView>
</picker:SfTimePicker>
```

**C#:**
```csharp
SfTimePicker timePicker = new SfTimePicker()
{
    MaximumTime = new TimeSpan(20, 0, 0),  // 8:00 PM
    Format = PickerTimeFormat.hh_mm_tt
};

this.Content = timePicker;
```

**Result:** Users cannot select times after 8:00 PM

### Example: Delivery Time Window

```xml
<picker:SfTimePicker x:Name="deliveryPicker"
                     MinimumTime="08:00:00"
                     MaximumTime="20:00:00"
                     Format="hh_mm_tt"
                     MinuteInterval="30"
                     SelectedTime="12:00:00">
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView Text="Preferred Delivery Time" Height="40" />
    </picker:SfTimePicker.HeaderView>
    <picker:SfTimePicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True" Height="40" />
    </picker:SfTimePicker.FooterView>
</picker:SfTimePicker>
```

**Available Times:** 8:00 AM to 8:00 PM in 30-minute intervals

### Setting MaximumTime Programmatically

```csharp
// Set maximum time to 6:00 PM
timePicker.MaximumTime = new TimeSpan(18, 0, 0);

// Set maximum time to current time (for past time selection)
timePicker.MaximumTime = DateTime.Now.TimeOfDay;

// Set maximum time 2 hours from now
timePicker.MaximumTime = DateTime.Now.AddHours(2).TimeOfDay;
```

## Min/Max Time Together

Combine both properties to define a specific time range.

### Example: Office Hours (9 AM - 5 PM)

**XAML:**
```xml
<picker:SfTimePicker x:Name="officeHoursPicker"
                     MinimumTime="09:00:00"
                     MaximumTime="17:00:00"
                     Format="hh_mm_tt"
                     MinuteInterval="15">
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView Text="Office Hours Only" Height="40" />
    </picker:SfTimePicker.HeaderView>
</picker:SfTimePicker>
```

**C#:**
```csharp
SfTimePicker officeHoursPicker = new SfTimePicker()
{
    MinimumTime = new TimeSpan(9, 0, 0),   // 9:00 AM
    MaximumTime = new TimeSpan(17, 0, 0),  // 5:00 PM
    Format = PickerTimeFormat.hh_mm_tt,
    MinuteInterval = 15
};

this.Content = officeHoursPicker;
```

### Example: Night Shift (10 PM - 6 AM)

```csharp
// Note: For overnight ranges, use two pickers or handle logic in code
SfTimePicker nightShiftStart = new SfTimePicker()
{
    MinimumTime = new TimeSpan(22, 0, 0),  // 10:00 PM
    MaximumTime = new TimeSpan(23, 59, 59), // 11:59 PM
    Format = PickerTimeFormat.HH_mm
};

SfTimePicker nightShiftEnd = new SfTimePicker()
{
    MinimumTime = new TimeSpan(0, 0, 0),   // 12:00 AM
    MaximumTime = new TimeSpan(6, 0, 0),   // 6:00 AM
    Format = PickerTimeFormat.HH_mm
};
```

## BlackoutTimes Property

The `BlackoutTimes` property allows you to disable specific time values, preventing users from selecting them. This is useful for blocking unavailable appointment slots or restricted times.

**Property:**
```csharp
public IList<TimeSpan> BlackoutTimes { get; set; }
```

**Type:** `IList<TimeSpan>`

**Default:** Empty collection

**Note:** Selection view is not displayed when BlackoutTimes are set

### Basic BlackoutTimes Example

**XAML:**
```xml
<picker:SfTimePicker x:Name="timePicker"
                     Format="hh_mm_tt">
    <picker:SfTimePicker.BlackoutTimes>
        <x:TimeSpan>12:00:00</x:TimeSpan>
        <x:TimeSpan>12:30:00</x:TimeSpan>
        <x:TimeSpan>13:00:00</x:TimeSpan>
    </picker:SfTimePicker.BlackoutTimes>
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView Text="Available Times" Height="40" />
    </picker:SfTimePicker.HeaderView>
</picker:SfTimePicker>
```

**C#:**
```csharp
SfTimePicker timePicker = new SfTimePicker()
{
    Format = PickerTimeFormat.hh_mm_tt
};

// Add blocked times
timePicker.BlackoutTimes.Add(new TimeSpan(12, 0, 0));  // 12:00 PM
timePicker.BlackoutTimes.Add(new TimeSpan(12, 30, 0)); // 12:30 PM
timePicker.BlackoutTimes.Add(new TimeSpan(13, 0, 0));  // 1:00 PM

this.Content = timePicker;
```

**Result:** 12:00 PM, 12:30 PM, and 1:00 PM are disabled (grayed out)

### Example: Lunch Break Blocking

```xml
<picker:SfTimePicker x:Name="appointmentPicker"
                     MinimumTime="09:00:00"
                     MaximumTime="17:00:00"
                     MinuteInterval="30"
                     Format="hh_mm_tt">
    <picker:SfTimePicker.BlackoutTimes>
        <!-- Block lunch time: 12:00 PM - 1:30 PM -->
        <x:TimeSpan>12:00:00</x:TimeSpan>
        <x:TimeSpan>12:30:00</x:TimeSpan>
        <x:TimeSpan>13:00:00</x:TimeSpan>
        <x:TimeSpan>13:30:00</x:TimeSpan>
    </picker:SfTimePicker.BlackoutTimes>
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView Text="Appointment Time" Height="40" />
    </picker:SfTimePicker.HeaderView>
</picker:SfTimePicker>
```

### Dynamic BlackoutTimes from Booked Appointments

```csharp
public class AppointmentPage : ContentPage
{
    private SfTimePicker appointmentPicker;
    
    public AppointmentPage()
    {
        appointmentPicker = new SfTimePicker()
        {
            MinimumTime = new TimeSpan(9, 0, 0),
            MaximumTime = new TimeSpan(17, 0, 0),
            MinuteInterval = 30,
            Format = PickerTimeFormat.hh_mm_tt
        };
        
        // Load booked appointments and block those times
        LoadBookedAppointments();
        
        this.Content = appointmentPicker;
    }
    
    private void LoadBookedAppointments()
    {
        // Simulate fetching booked appointment times from database
        List<TimeSpan> bookedTimes = new List<TimeSpan>
        {
            new TimeSpan(9, 0, 0),   // 9:00 AM - booked
            new TimeSpan(10, 30, 0), // 10:30 AM - booked
            new TimeSpan(14, 0, 0),  // 2:00 PM - booked
            new TimeSpan(15, 30, 0)  // 3:30 PM - booked
        };
        
        // Add to blackout times
        foreach (var time in bookedTimes)
        {
            appointmentPicker.BlackoutTimes.Add(time);
        }
    }
}
```

### Example: Blocking Multiple Time Ranges

```csharp
private void BlockMultipleTimeRanges()
{
    // Block morning break: 10:00 AM - 10:30 AM
    timePicker.BlackoutTimes.Add(new TimeSpan(10, 0, 0));
    timePicker.BlackoutTimes.Add(new TimeSpan(10, 15, 0));
    timePicker.BlackoutTimes.Add(new TimeSpan(10, 30, 0));
    
    // Block lunch: 12:00 PM - 1:00 PM
    timePicker.BlackoutTimes.Add(new TimeSpan(12, 0, 0));
    timePicker.BlackoutTimes.Add(new TimeSpan(12, 15, 0));
    timePicker.BlackoutTimes.Add(new TimeSpan(12, 30, 0));
    timePicker.BlackoutTimes.Add(new TimeSpan(12, 45, 0));
    timePicker.BlackoutTimes.Add(new TimeSpan(13, 0, 0));
    
    // Block afternoon break: 3:00 PM - 3:15 PM
    timePicker.BlackoutTimes.Add(new TimeSpan(15, 0, 0));
    timePicker.BlackoutTimes.Add(new TimeSpan(15, 15, 0));
}
```

### Clearing BlackoutTimes

```csharp
// Clear all blackout times
timePicker.BlackoutTimes.Clear();

// Remove specific blackout time
timePicker.BlackoutTimes.Remove(new TimeSpan(12, 0, 0));
```

## Practical Use Cases

### Use Case 1: Doctor's Appointment Scheduler

```xml
<picker:SfTimePicker x:Name="doctorAppointment"
                     MinimumTime="08:00:00"
                     MaximumTime="18:00:00"
                     MinuteInterval="20"
                     Format="hh_mm_tt"
                     SelectedTime="09:00:00">
    <picker:SfTimePicker.BlackoutTimes>
        <!-- Doctor not available -->
        <x:TimeSpan>12:20:00</x:TimeSpan>
        <x:TimeSpan>12:40:00</x:TimeSpan>
        <x:TimeSpan>13:00:00</x:TimeSpan>
        <x:TimeSpan>13:20:00</x:TimeSpan>
    </picker:SfTimePicker.BlackoutTimes>
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView Text="Select Appointment Time" Height="40" />
    </picker:SfTimePicker.HeaderView>
    <picker:SfTimePicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True" Height="40" />
    </picker:SfTimePicker.FooterView>
</picker:SfTimePicker>
```

### Use Case 2: Restaurant Reservation System

```csharp
SfTimePicker reservationPicker = new SfTimePicker()
{
    MinimumTime = new TimeSpan(11, 0, 0),  // Opens at 11 AM
    MaximumTime = new TimeSpan(22, 0, 0),  // Last seating at 10 PM
    MinuteInterval = 30,
    Format = PickerTimeFormat.hh_mm_tt
};

// Block fully booked time slots
reservationPicker.BlackoutTimes.Add(new TimeSpan(18, 0, 0));  // 6:00 PM - full
reservationPicker.BlackoutTimes.Add(new TimeSpan(18, 30, 0)); // 6:30 PM - full
reservationPicker.BlackoutTimes.Add(new TimeSpan(19, 0, 0));  // 7:00 PM - full

reservationPicker.HeaderView = new PickerHeaderView()
{
    Text = "Reservation Time",
    Height = 40
};

this.Content = reservationPicker;
```

### Use Case 3: Meeting Room Booking

```csharp
public void SetupMeetingRoomPicker(List<TimeSpan> bookedSlots)
{
    SfTimePicker meetingPicker = new SfTimePicker()
    {
        MinimumTime = new TimeSpan(8, 0, 0),
        MaximumTime = new TimeSpan(20, 0, 0),
        MinuteInterval = 60,  // 1-hour slots
        Format = PickerTimeFormat.HH_mm
    };
    
    // Block already booked slots
    foreach (var slot in bookedSlots)
    {
        meetingPicker.BlackoutTimes.Add(slot);
    }
    
    // Always block lunch time
    meetingPicker.BlackoutTimes.Add(new TimeSpan(12, 0, 0));
    meetingPicker.BlackoutTimes.Add(new TimeSpan(13, 0, 0));
    
    this.Content = meetingPicker;
}
```

### Use Case 4: Gym Class Schedule

```xml
<picker:SfTimePicker x:Name="gymClassPicker"
                     MinimumTime="06:00:00"
                     MaximumTime="21:00:00"
                     HourInterval="1"
                     Format="hh_mm_tt">
    <picker:SfTimePicker.BlackoutTimes>
        <!-- No classes during these hours -->
        <x:TimeSpan>12:00:00</x:TimeSpan>
        <x:TimeSpan>13:00:00</x:TimeSpan>
        <x:TimeSpan>14:00:00</x:TimeSpan>
    </picker:SfTimePicker.BlackoutTimes>
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView Text="Class Time" Height="40" />
    </picker:SfTimePicker.HeaderView>
</picker:SfTimePicker>
```

## Time Restriction Validation

### Validation Rules

1. **MinimumTime < MaximumTime**
   ```csharp
   // Valid
   timePicker.MinimumTime = new TimeSpan(9, 0, 0);
   timePicker.MaximumTime = new TimeSpan(17, 0, 0);
   
   // Invalid - will cause issues
   timePicker.MinimumTime = new TimeSpan(17, 0, 0);
   timePicker.MaximumTime = new TimeSpan(9, 0, 0);
   ```

2. **Only Hour and Minute Components Considered**
   ```csharp
   // Seconds are ignored in min/max validation
   timePicker.MinimumTime = new TimeSpan(9, 30, 45);  // Treated as 9:30
   timePicker.MaximumTime = new TimeSpan(17, 15, 30); // Treated as 17:15
   ```

3. **BlackoutTimes Must Fall Within Min/Max Range**
   ```csharp
   timePicker.MinimumTime = new TimeSpan(9, 0, 0);
   timePicker.MaximumTime = new TimeSpan(17, 0, 0);
   
   // This blackout time will have no effect (outside range)
   timePicker.BlackoutTimes.Add(new TimeSpan(20, 0, 0));
   ```

### Validation Helper

```csharp
public class TimePickerValidator
{
    public static bool ValidateTimeRange(TimeSpan min, TimeSpan max)
    {
        if (min >= max)
        {
            throw new ArgumentException("MinimumTime must be less than MaximumTime");
        }
        return true;
    }
    
    public static bool ValidateBlackoutTime(TimeSpan blackoutTime, TimeSpan min, TimeSpan max)
    {
        if (blackoutTime < min || blackoutTime > max)
        {
            System.Diagnostics.Debug.WriteLine($"Warning: BlackoutTime {blackoutTime} is outside min/max range");
            return false;
        }
        return true;
    }
}

// Usage
TimePickerValidator.ValidateTimeRange(minTime, maxTime);
```

## Combining Restrictions with Intervals

Time restrictions work seamlessly with interval properties:

```xml
<picker:SfTimePicker MinimumTime="09:00:00"
                     MaximumTime="17:00:00"
                     MinuteInterval="30"
                     Format="hh_mm_tt">
    <picker:SfTimePicker.BlackoutTimes>
        <x:TimeSpan>12:00:00</x:TimeSpan>
        <x:TimeSpan>12:30:00</x:TimeSpan>
    </picker:SfTimePicker.BlackoutTimes>
</picker:SfTimePicker>
```

**Result:**
- Available times: 9:00, 9:30, 10:00, 10:30, 11:00, 11:30, 1:00, 1:30, ... 5:00
- Blocked times: 12:00, 12:30
- Times outside 9 AM - 5 PM: Not available

## Best Practices

### 1. Always Validate Min < Max

```csharp
private void SetTimeRange(TimeSpan min, TimeSpan max)
{
    if (min >= max)
    {
        throw new ArgumentException("Minimum time must be less than maximum time");
    }
    
    timePicker.MinimumTime = min;
    timePicker.MaximumTime = max;
}
```

### 2. Handle SelectedTime Outside Range

```csharp
private void EnsureSelectedTimeInRange()
{
    if (timePicker.SelectedTime.HasValue)
    {
        if (timePicker.SelectedTime.Value < timePicker.MinimumTime)
        {
            timePicker.SelectedTime = timePicker.MinimumTime;
        }
        else if (timePicker.SelectedTime.Value > timePicker.MaximumTime)
        {
            timePicker.SelectedTime = timePicker.MaximumTime;
        }
    }
}
```

### 3. Clear BlackoutTimes When Refreshing

```csharp
private void RefreshAvailableSlots(List<TimeSpan> newBookedSlots)
{
    // Clear existing blackout times
    timePicker.BlackoutTimes.Clear();
    
    // Add new blackout times
    foreach (var slot in newBookedSlots)
    {
        timePicker.BlackoutTimes.Add(slot);
    }
}
```

### 4. Provide User Feedback

```csharp
private void ShowTimeRestrictionMessage()
{
    var minTimeStr = timePicker.MinimumTime.ToString(@"hh\:mm tt");
    var maxTimeStr = timePicker.MaximumTime.ToString(@"hh\:mm tt");
    
    DisplayAlert("Available Hours", 
                 $"Please select a time between {minTimeStr} and {maxTimeStr}", 
                 "OK");
}
```

## Summary

Time restrictions provide essential control over selectable time values:

- **MinimumTime** - Set earliest selectable time (business hours start, availability window)
- **MaximumTime** - Set latest selectable time (business hours end, cutoff time)
- **BlackoutTimes** - Disable specific times (booked slots, breaks, unavailable periods)
- **Validation** - Ensure MinimumTime < MaximumTime, handle edge cases
- **Combined with intervals** - Create precise, business-logic-driven time selection
- **Dynamic updates** - Refresh restrictions based on real-time availability

Use time restrictions to implement robust scheduling, booking, and time selection workflows that respect business rules and availability constraints.
