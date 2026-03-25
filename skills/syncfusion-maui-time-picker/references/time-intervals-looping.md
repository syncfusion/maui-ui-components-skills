# Time Intervals and Looping in .NET MAUI TimePicker

## Table of Contents
- [Overview](#overview)
- [HourInterval Property](#hourinterval-property)
- [MinuteInterval Property](#minuteinterval-property)
- [SecondInterval Property](#secondinterval-property)
- [MilliSecondInterval Property](#millisecondinterval-property)
- [EnableLooping Property](#enablelooping-property)
- [Combining Intervals](#combining-intervals)
- [Practical Examples](#practical-examples)
- [Best Practices](#best-practices)

## Overview

The TimePicker provides interval properties to control which time values are available for selection. Instead of showing every possible time value, you can specify intervals to show only specific increments (e.g., every 15 minutes, every 30 seconds).

Additionally, the `EnableLooping` property allows seamless navigation from the last item back to the first item in a continuous loop.

**Available Interval Properties:**
- `HourInterval` - Interval between hours
- `MinuteInterval` - Interval between minutes
- `SecondInterval` - Interval between seconds
- `MilliSecondInterval` - Interval between milliseconds
- `EnableLooping` - Enable continuous scrolling

## HourInterval Property

Controls the interval between hour values in the picker.

**Property:**
```csharp
public int HourInterval { get; set; }
```

**Default:** `1` (every hour: 0, 1, 2, ... 23)

**Valid Range:** 1 to 23

### Basic Hour Interval Example

**XAML:**
```xml
<picker:SfTimePicker x:Name="timePicker"
                     HourInterval="2"
                     Format="HH_mm" />
```

**C#:**
```csharp
SfTimePicker timePicker = new SfTimePicker()
{
    HourInterval = 2,  // Shows: 0, 2, 4, 6, 8, 10, 12, 14, 16, 18, 20, 22
    Format = PickerTimeFormat.HH_mm
};

this.Content = timePicker;
```

**Available Hours:** 00, 02, 04, 06, 08, 10, 12, 14, 16, 18, 20, 22

### Example: 3-Hour Intervals

```xml
<picker:SfTimePicker x:Name="shiftPicker"
                     HourInterval="3"
                     Format="HH_mm"
                     SelectedTime="09:00:00">
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView Text="Shift Start Time" Height="40" />
    </picker:SfTimePicker.HeaderView>
</picker:SfTimePicker>
```

**Available Hours:** 00, 03, 06, 09, 12, 15, 18, 21

**Use Cases:**
- Shift scheduling (6-hour or 8-hour shifts)
- Broad time selection (morning/afternoon/evening)
- Reducing time selection complexity

## MinuteInterval Property

Controls the interval between minute values in the picker.

**Property:**
```csharp
public int MinuteInterval { get; set; }
```

**Default:** `1` (every minute: 0, 1, 2, ... 59)

**Valid Range:** 1 to 59

### Basic Minute Interval Example

**XAML:**
```xml
<picker:SfTimePicker x:Name="timePicker"
                     MinuteInterval="15"
                     Format="hh_mm_tt" />
```

**C#:**
```csharp
SfTimePicker timePicker = new SfTimePicker()
{
    MinuteInterval = 15,  // Shows: 00, 15, 30, 45
    Format = PickerTimeFormat.hh_mm_tt
};

this.Content = timePicker;
```

**Available Minutes:** 00, 15, 30, 45

### Common Minute Intervals

**5-Minute Intervals:**
```xml
<picker:SfTimePicker MinuteInterval="5" />
<!-- Shows: 00, 05, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55 -->
```

**10-Minute Intervals:**
```xml
<picker:SfTimePicker MinuteInterval="10" />
<!-- Shows: 00, 10, 20, 30, 40, 50 -->
```

**15-Minute Intervals:**
```xml
<picker:SfTimePicker MinuteInterval="15" />
<!-- Shows: 00, 15, 30, 45 -->
```

**30-Minute Intervals:**
```xml
<picker:SfTimePicker MinuteInterval="30" />
<!-- Shows: 00, 30 -->
```

### Example: Appointment Booking (15-minute slots)

```xml
<picker:SfTimePicker x:Name="appointmentPicker"
                     MinuteInterval="15"
                     Format="hh_mm_tt"
                     MinimumTime="09:00:00"
                     MaximumTime="17:00:00"
                     SelectedTime="09:00:00">
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView Text="Appointment Time" Height="40" />
    </picker:SfTimePicker.HeaderView>
    <picker:SfTimePicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True" Height="40" />
    </picker:SfTimePicker.FooterView>
</picker:SfTimePicker>
```

**Available Times:** 09:00, 09:15, 09:30, 09:45, 10:00, ... 16:45, 17:00

**Use Cases:**
- Appointment scheduling
- Meeting planners
- Reservation systems
- Calendar applications

## SecondInterval Property

Controls the interval between second values in the picker.

**Property:**
```csharp
public int SecondInterval { get; set; }
```

**Default:** `1` (every second: 0, 1, 2, ... 59)

**Valid Range:** 1 to 59

### Basic Second Interval Example

**XAML:**
```xml
<picker:SfTimePicker x:Name="timePicker"
                     SecondInterval="10"
                     Format="HH_mm_ss" />
```

**C#:**
```csharp
SfTimePicker timePicker = new SfTimePicker()
{
    SecondInterval = 10,  // Shows: 00, 10, 20, 30, 40, 50
    Format = PickerTimeFormat.HH_mm_ss
};

this.Content = timePicker;
```

**Available Seconds:** 00, 10, 20, 30, 40, 50

### Example: Timer with 15-second Intervals

```xml
<picker:SfTimePicker x:Name="timerPicker"
                     SecondInterval="15"
                     Format="mm_ss"
                     SelectedTime="00:05:00">
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView Text="Countdown Timer" Height="40" />
    </picker:SfTimePicker.HeaderView>
</picker:SfTimePicker>
```

**Available Seconds:** 00, 15, 30, 45

**Use Cases:**
- Countdown timers
- Exercise/workout tracking
- Process timing
- Cooking timers

## MilliSecondInterval Property

Controls the interval between millisecond values in the picker.

**Property:**
```csharp
public int MilliSecondInterval { get; set; }
```

**Default:** `1` (every millisecond: 0, 1, 2, ... 999)

**Valid Range:** 1 to 999

### Basic MilliSecond Interval Example

**XAML:**
```xml
<picker:SfTimePicker x:Name="timePicker"
                     MilliSecondInterval="100"
                     Format="HH_mm_ss_fff" />
```

**C#:**
```csharp
SfTimePicker timePicker = new SfTimePicker()
{
    MilliSecondInterval = 100,  // Shows: 000, 100, 200, ... 900
    Format = PickerTimeFormat.HH_mm_ss_fff
};

this.Content = timePicker;
```

**Available Milliseconds:** 000, 100, 200, 300, 400, 500, 600, 700, 800, 900

### Example: Stopwatch with 100ms Precision

```xml
<picker:SfTimePicker x:Name="stopwatchPicker"
                     MilliSecondInterval="100"
                     Format="mm_ss_fff"
                     SelectedTime="00:00:00">
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView Text="Lap Time" Height="40" />
    </picker:SfTimePicker.HeaderView>
</picker:SfTimePicker>
```

**Use Cases:**
- Sports timing
- Race lap times
- Scientific measurements
- High-precision logging

### Example: 250ms Intervals

```csharp
SfTimePicker precisePicker = new SfTimePicker()
{
    MilliSecondInterval = 250,  // Shows: 000, 250, 500, 750
    Format = PickerTimeFormat.ss_fff
};

this.Content = precisePicker;
```

## EnableLooping Property

Enables continuous scrolling where the picker seamlessly navigates from the last item to the first item and vice versa.

**Property:**
```csharp
public bool EnableLooping { get; set; }
```

**Type:** `bool`

**Default:** `false`

**Behavior:**
- When `true`: Last item connects to first item (continuous loop)
- When `false`: Scrolling stops at first and last items

### Basic Looping Example

**XAML:**
```xml
<picker:SfTimePicker x:Name="timePicker"
                     EnableLooping="True"
                     Format="hh_mm_tt" />
```

**C#:**
```csharp
SfTimePicker timePicker = new SfTimePicker()
{
    EnableLooping = true,
    Format = PickerTimeFormat.hh_mm_tt
};

this.Content = timePicker;
```

### Looping Behavior

**With EnableLooping = true:**
- Scroll down from 23:59 → wraps to 00:00
- Scroll up from 00:00 → wraps to 23:59
- Smooth, continuous scrolling experience
- No "dead ends" at min/max values

**With EnableLooping = false (default):**
- Scroll stops at minimum value
- Scroll stops at maximum value
- Cannot wrap around from last to first

### Example: Alarm Clock with Looping

```xml
<picker:SfTimePicker x:Name="alarmPicker"
                     EnableLooping="True"
                     MinuteInterval="5"
                     Format="hh_mm_tt"
                     SelectedTime="07:00:00">
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView Text="Set Alarm" Height="40" />
    </picker:SfTimePicker.HeaderView>
    <picker:SfTimePicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True" Height="40" />
    </picker:SfTimePicker.FooterView>
</picker:SfTimePicker>
```

**Use Cases:**
- Clock/alarm applications
- Circular time selection
- Improved user experience for time scrolling
- 24-hour time pickers

## Combining Intervals

You can combine multiple intervals for precise time control.

### Example 1: Hour + Minute Intervals

```xml
<picker:SfTimePicker x:Name="meetingPicker"
                     HourInterval="1"
                     MinuteInterval="30"
                     Format="hh_mm_tt"
                     MinimumTime="08:00:00"
                     MaximumTime="18:00:00">
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView Text="Meeting Time" Height="40" />
    </picker:SfTimePicker.HeaderView>
</picker:SfTimePicker>
```

**Available Times:**
- 08:00, 08:30
- 09:00, 09:30
- ... (every hour and half-hour)
- 17:00, 17:30
- 18:00

### Example 2: Minute + Second Intervals for Timer

```xml
<picker:SfTimePicker x:Name="workoutTimer"
                     MinuteInterval="5"
                     SecondInterval="30"
                     Format="mm_ss"
                     EnableLooping="False">
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView Text="Exercise Duration" Height="40" />
    </picker:SfTimePicker.HeaderView>
</picker:SfTimePicker>
```

**Available Times:**
- Minutes: 00, 05, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55
- Seconds: 00, 30

**Combinations:** 00:00, 00:30, 05:00, 05:30, 10:00, 10:30, etc.

### Example 3: All Intervals Combined

```csharp
SfTimePicker detailedPicker = new SfTimePicker()
{
    HourInterval = 2,
    MinuteInterval = 15,
    SecondInterval = 10,
    MilliSecondInterval = 500,
    Format = PickerTimeFormat.HH_mm_ss_fff,
    EnableLooping = true
};

this.Content = detailedPicker;
```

**Result:**
- Hours: 00, 02, 04, 06, 08, 10, 12, 14, 16, 18, 20, 22
- Minutes: 00, 15, 30, 45
- Seconds: 00, 10, 20, 30, 40, 50
- Milliseconds: 000, 500

## Practical Examples

### Example 1: Restaurant Reservation (30-minute slots, business hours)

```xml
<picker:SfTimePicker x:Name="reservationPicker"
                     MinuteInterval="30"
                     Format="hh_mm_tt"
                     MinimumTime="11:00:00"
                     MaximumTime="22:00:00"
                     SelectedTime="18:00:00"
                     EnableLooping="False">
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView Text="Reservation Time" Height="40" />
    </picker:SfTimePicker.HeaderView>
    <picker:SfTimePicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True" Height="40" />
    </picker:SfTimePicker.FooterView>
</picker:SfTimePicker>
```

**Available Slots:** 11:00, 11:30, 12:00, ... 21:30, 22:00

### Example 2: Parking Duration (15-minute increments)

```xml
<picker:SfTimePicker x:Name="parkingDuration"
                     HourInterval="1"
                     MinuteInterval="15"
                     Format="H_mm"
                     MinimumTime="00:00:00"
                     MaximumTime="12:00:00"
                     SelectedTime="02:00:00">
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView Text="Parking Duration" Height="40" />
    </picker:SfTimePicker.HeaderView>
</picker:SfTimePicker>
```

**Available Durations:** 0:00, 0:15, 0:30, ... 11:45, 12:00

### Example 3: Flight Time (5-minute intervals)

```xml
<picker:SfTimePicker x:Name="flightTimePicker"
                     MinuteInterval="5"
                     Format="HH_mm"
                     EnableLooping="True"
                     SelectedTime="00:00:00">
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView Text="Departure Time" Height="40" />
    </picker:SfTimePicker.HeaderView>
</picker:SfTimePicker>
```

**Available Times:** Every 5 minutes (00:00, 00:05, 00:10, ... 23:55)

### Example 4: Medication Schedule (Every 4 hours)

```xml
<picker:SfTimePicker x:Name="medicationPicker"
                     HourInterval="4"
                     Format="hh_mm_tt"
                     SelectedTime="08:00:00"
                     EnableLooping="True">
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView Text="Medication Time" Height="40" />
    </picker:SfTimePicker.HeaderView>
</picker:SfTimePicker>
```

**Available Times:** 12:00 AM, 04:00 AM, 08:00 AM, 12:00 PM, 04:00 PM, 08:00 PM

### Example 5: Workout Rest Timer (10-second intervals)

```csharp
SfTimePicker restTimer = new SfTimePicker()
{
    SecondInterval = 10,
    Format = PickerTimeFormat.ss_fff,
    MinimumTime = new TimeSpan(0, 0, 10),
    MaximumTime = new TimeSpan(0, 2, 0),
    SelectedTime = new TimeSpan(0, 0, 30),
    EnableLooping = false
};

restTimer.HeaderView = new PickerHeaderView()
{
    Text = "Rest Duration (seconds)",
    Height = 40
};

this.Content = restTimer;
```

**Available Times:** 10, 20, 30, 40, 50, 60, 70, 80, ... 120 seconds

## Best Practices

### 1. Match Intervals with Format

Ensure the format displays the interval component:

```xml
<!-- Good: Minute interval with minute format -->
<picker:SfTimePicker MinuteInterval="15" Format="hh_mm_tt" />

<!-- Ineffective: Second interval but format doesn't show seconds -->
<picker:SfTimePicker SecondInterval="30" Format="hh_mm_tt" />

<!-- Correct: Second interval with seconds in format -->
<picker:SfTimePicker SecondInterval="30" Format="hh_mm_ss_tt" />
```

### 2. Choose Appropriate Intervals

**For appointments:** 15 or 30-minute intervals
```xml
<picker:SfTimePicker MinuteInterval="15" />
```

**For general time selection:** 5-minute intervals
```xml
<picker:SfTimePicker MinuteInterval="5" />
```

**For duration/timers:** Minute + second intervals
```xml
<picker:SfTimePicker MinuteInterval="1" SecondInterval="10" />
```

### 3. Enable Looping for Circular Selection

Enable looping for clock-style time selection:

```xml
<picker:SfTimePicker EnableLooping="True" Format="hh_mm_tt" />
```

Disable looping when there are clear boundaries:

```xml
<picker:SfTimePicker EnableLooping="False" 
                     MinimumTime="09:00:00" 
                     MaximumTime="17:00:00" />
```

### 4. Combine with Time Restrictions

Use intervals with min/max times for business logic:

```csharp
// Office hours with 30-minute meeting slots
SfTimePicker officePicker = new SfTimePicker()
{
    MinuteInterval = 30,
    MinimumTime = new TimeSpan(9, 0, 0),   // 9:00 AM
    MaximumTime = new TimeSpan(17, 0, 0),  // 5:00 PM
    Format = PickerTimeFormat.hh_mm_tt,
    EnableLooping = false
};
```

### 5. Test User Experience

Large intervals reduce scrolling but may miss desired times:

```xml
<!-- Too coarse for appointments -->
<picker:SfTimePicker MinuteInterval="60" />

<!-- More user-friendly -->
<picker:SfTimePicker MinuteInterval="15" />
```

## Interval Validation

### Valid Interval Ranges
- **HourInterval:** 1 to 23
- **MinuteInterval:** 1 to 59
- **SecondInterval:** 1 to 59
- **MilliSecondInterval:** 1 to 999

### Invalid Intervals
Setting intervals outside valid ranges may cause unexpected behavior:

```csharp
// Invalid - will be clamped or cause errors
timePicker.HourInterval = 25;      // Max is 23
timePicker.MinuteInterval = 100;   // Max is 59
timePicker.SecondInterval = -5;    // Must be positive
```

## Summary

Intervals and looping provide powerful control over time selection:

- **HourInterval** - Control hour increments (shift schedules, broad selections)
- **MinuteInterval** - Most commonly used (appointments, meetings, reservations)
- **SecondInterval** - For timers and duration tracking
- **MilliSecondInterval** - High-precision timing needs
- **EnableLooping** - Seamless continuous scrolling for better UX
- **Combine intervals** for precise time control
- **Match format with intervals** to display relevant components

Use intervals to simplify time selection and improve user experience by showing only relevant time values.
