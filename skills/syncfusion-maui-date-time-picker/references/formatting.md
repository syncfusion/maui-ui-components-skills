# Date and Time Formatting

## Table of Contents
- [Overview](#overview)
- [Date Formats](#date-formats)
- [Time Formats](#time-formats)
- [Setting Formats](#setting-formats)
- [Format Selection Guide](#format-selection-guide)
- [Custom Display Examples](#custom-display-examples)

## Overview

The DateTimePicker provides extensive formatting options to display dates and times in different string formats. This allows you to match regional preferences, application requirements, and user expectations.

**Key Properties:**
- `DateFormat` - Controls how dates are displayed (18 predefined formats)
- `TimeFormat` - Controls how times are displayed (9 predefined formats)
- Default: `yyyy_MM_dd` for dates, `h_mm_tt` for times

## Date Formats

The `DateFormat` property accepts values from the `PickerDateFormat` enumeration. The default format is `yyyy_MM_dd`.

### Available Date Formats

| Format | Pattern | Example | Use Case |
|--------|---------|---------|----------|
| `dd_MM` | dd/MM | 15/03 | Day and month only |
| `dd_MM_yyyy` | dd/MM/yyyy | 15/03/2024 | European format |
| `dd_MMM_yyyy` | dd/MMM/yyyy | 15/Mar/2024 | Short month name |
| `dd_MMMM_yyyy` | dd/MMMM/yyyy | 15/March/2024 | Full month name |
| `dd_MMM` | dd/MMM | 15/Mar | Short month, no year |
| `dd_MMMM` | dd/MMMM | 15/March | Full month, no year |
| `M_d_yyyy` | M/d/yyyy | 3/15/2024 | US format (short) |
| `MM_dd_yyyy` | MM/dd/yyyy | 03/15/2024 | US format (padded) |
| `MM_dd` | MM/dd | 03/15 | Month and day only |
| `MM_yyyy` | MM/yyyy | 03/2024 | Month and year |
| `MMM_yyyy` | MMM/yyyy | Mar/2024 | Short month and year |
| `MMMM_yyyy` | MMMM/yyyy | March/2024 | Full month and year |
| `MMM_dd_yyyy` | MMM/dd/yyyy | Mar/15/2024 | US with short month |
| `MMMM_dd_yyyy` | MMMM/dd/yyyy | March/15/2024 | US with full month |
| `yyyy_MM_dd` | yyyy/MM/dd | 2024/03/15 | ISO-like format (default) |
| `yyyy_MM` | yyyy/MM | 2024/03 | Year and month |
| `yyyy_MMM` | yyyy/MMM | 2024/Mar | Year with short month |
| `yyyy_MMMM` | yyyy/MMMM | 2024/March | Year with full month |
| `yyyy_MMM_dd` | yyyy/MMM/dd | 2024/Mar/15 | ISO with short month |
| `yyyy_MMMM_dd` | yyyy/MMMM/dd | 2024/March/15 | ISO with full month |
| `ddd_dd_MM_YYYY` | ddd/dd/MM/YYYY | Thu/15/03/2024 | With weekday |
| `Default` | Culture-based | Varies | Uses current culture |

### Date Format Examples

**European Format (dd/MM/yyyy):**
```xaml
<picker:SfDateTimePicker DateFormat="dd_MM_yyyy" />
```
Displays: 15/03/2024

**US Format (MM/dd/yyyy):**
```xaml
<picker:SfDateTimePicker DateFormat="MM_dd_yyyy" />
```
Displays: 03/15/2024

**ISO Format (yyyy/MM/dd):**
```xaml
<picker:SfDateTimePicker DateFormat="yyyy_MM_dd" />
```
Displays: 2024/03/15

**Friendly Format with Full Month:**
```xaml
<picker:SfDateTimePicker DateFormat="dd_MMMM_yyyy" />
```
Displays: 15/March/2024

**Short Format with Month Name:**
```xaml
<picker:SfDateTimePicker DateFormat="dd_MMM_yyyy" />
```
Displays: 15/Mar/2024

**Month and Year Only:**
```xaml
<picker:SfDateTimePicker DateFormat="MMMM_yyyy" />
```
Displays: March/2024

**With Weekday:**
```xaml
<picker:SfDateTimePicker DateFormat="ddd_dd_MM_YYYY" />
```
Displays: Thu/15/03/2024

## Time Formats

The `TimeFormat` property accepts values from the `PickerTimeFormat` enumeration. The default format is `h_mm_tt`.

### Available Time Formats

| Format | Pattern | Example | Use Case |
|--------|---------|---------|----------|
| `h_mm_tt` | h:mm tt | 2:30 PM | 12-hour with AM/PM (default) |
| `hh_mm_tt` | hh:mm tt | 02:30 PM | 12-hour padded with AM/PM |
| `H_mm` | H:mm | 14:30 | 24-hour format (short) |
| `HH_mm` | HH:mm | 14:30 | 24-hour format (padded) |
| `h_mm_ss_tt` | h:mm:ss tt | 2:30:45 PM | 12-hour with seconds |
| `hh_mm_ss_tt` | hh:mm:ss tt | 02:30:45 PM | 12-hour padded with seconds |
| `H_mm_ss` | H:mm:ss | 14:30:45 | 24-hour with seconds (short) |
| `HH_mm_ss` | HH:mm:ss | 14:30:45 | 24-hour with seconds (padded) |
| `Default` | Culture-based | Varies | Uses current culture |

### Time Format Examples

**12-Hour Format with AM/PM:**
```xaml
<picker:SfDateTimePicker TimeFormat="h_mm_tt" />
```
Displays: 2:30 PM

**12-Hour Padded:**
```xaml
<picker:SfDateTimePicker TimeFormat="hh_mm_tt" />
```
Displays: 02:30 PM

**24-Hour Format:**
```xaml
<picker:SfDateTimePicker TimeFormat="HH_mm" />
```
Displays: 14:30

**With Seconds (12-Hour):**
```xaml
<picker:SfDateTimePicker TimeFormat="hh_mm_ss_tt" />
```
Displays: 02:30:45 PM

**With Seconds (24-Hour):**
```xaml
<picker:SfDateTimePicker TimeFormat="HH_mm_ss" />
```
Displays: 14:30:45

## Setting Formats

### XAML

```xaml
<picker:SfDateTimePicker 
    x:Name="picker"
    DateFormat="dd_MMM_yyyy"
    TimeFormat="hh_mm_tt" />
```

### C#

```csharp
var picker = new SfDateTimePicker
{
    DateFormat = PickerDateFormat.dd_MMM_yyyy,
    TimeFormat = PickerTimeFormat.hh_mm_tt
};
```

### Setting Both Formats

```xaml
<picker:SfDateTimePicker 
    DateFormat="MMMM_dd_yyyy"
    TimeFormat="h_mm_ss_tt"
    SelectedDate="2024-03-15 14:30:45" />
```

Result: "March/15/2024" for date, "2:30:45 PM" for time

## Format Selection Guide

### By Region

**United States:**
```csharp
DateFormat = PickerDateFormat.MM_dd_yyyy;  // 03/15/2024
TimeFormat = PickerTimeFormat.h_mm_tt;      // 2:30 PM
```

**Europe:**
```csharp
DateFormat = PickerDateFormat.dd_MM_yyyy;  // 15/03/2024
TimeFormat = PickerTimeFormat.HH_mm;        // 14:30
```

**International (ISO-like):**
```csharp
DateFormat = PickerDateFormat.yyyy_MM_dd;  // 2024/03/15
TimeFormat = PickerTimeFormat.HH_mm;        // 14:30
```

### By Use Case

**User-Friendly Appointments:**
```csharp
DateFormat = PickerDateFormat.dd_MMM_yyyy;    // 15/Mar/2024
TimeFormat = PickerTimeFormat.h_mm_tt;         // 2:30 PM
```

**Formal Documents:**
```csharp
DateFormat = PickerDateFormat.dd_MMMM_yyyy;   // 15/March/2024
TimeFormat = PickerTimeFormat.hh_mm_tt;        // 02:30 PM
```

**Technical/Logging:**
```csharp
DateFormat = PickerDateFormat.yyyy_MM_dd;     // 2024/03/15
TimeFormat = PickerTimeFormat.HH_mm_ss;        // 14:30:45
```

**Calendar Views (Month/Year):**
```csharp
DateFormat = PickerDateFormat.MMMM_yyyy;      // March/2024
```

**Time-Only Selection:**
```csharp
TimeFormat = PickerTimeFormat.h_mm_tt;        // 2:30 PM
// Hide date picker if needed
```

## Custom Display Examples

### Example 1: Appointment Booking

```xaml
<StackLayout>
    <Label Text="Select Appointment Date:" />
    <picker:SfDateTimePicker 
        x:Name="appointmentPicker"
        DateFormat="MMM_dd_yyyy"
        TimeFormat="h_mm_tt"
        SelectedDate="{Binding AppointmentDateTime, Mode=TwoWay}" />
    <Label Text="{Binding AppointmentDateTime, StringFormat='Appointment: {0:MMMM dd, yyyy at h:mm tt}'}" />
</StackLayout>
```

Display: "Appointment: March 15, 2024 at 2:30 PM"

### Example 2: International Event

```xaml
<picker:SfDateTimePicker 
    DateFormat="yyyy_MMM_dd"
    TimeFormat="HH_mm"
    SelectedDate="2024-03-15 14:30:00" />
```

Display: "2024/Mar/15" and "14:30"

### Example 3: Birthday Selection (No Time)

```xaml
<picker:SfDateTimePicker 
    DateFormat="dd_MMMM_yyyy"
    SelectedDate="1990-05-20" />
```

Display: "20/May/1990"

### Example 4: Meeting Scheduler with Seconds

```xaml
<picker:SfDateTimePicker 
    DateFormat="ddd_dd_MM_YYYY"
    TimeFormat="hh_mm_ss_tt"
    SelectedDate="2024-03-15 14:30:45" />
```

Display: "Thu/15/03/2024" and "02:30:45 PM"

### Example 5: Month Picker

```xaml
<picker:SfDateTimePicker 
    DateFormat="MMMM_yyyy"
    SelectedDate="2024-03-01" />
```

Display: "March/2024"

## Dynamic Format Changes

You can change formats at runtime:

```csharp
// Switch to European format
private void OnSwitchToEuropeanFormat(object sender, EventArgs e)
{
    dateTimePicker.DateFormat = PickerDateFormat.dd_MM_yyyy;
    dateTimePicker.TimeFormat = PickerTimeFormat.HH_mm;
}

// Switch to US format
private void OnSwitchToUSFormat(object sender, EventArgs e)
{
    dateTimePicker.DateFormat = PickerDateFormat.MM_dd_yyyy;
    dateTimePicker.TimeFormat = PickerTimeFormat.h_mm_tt;
}
```

## Format with Data Binding

### ViewModel

```csharp
public class SettingsViewModel : INotifyPropertyChanged
{
    private PickerDateFormat _selectedDateFormat = PickerDateFormat.dd_MMM_yyyy;
    
    public PickerDateFormat SelectedDateFormat
    {
        get => _selectedDateFormat;
        set
        {
            _selectedDateFormat = value;
            OnPropertyChanged();
        }
    }
    
    private PickerTimeFormat _selectedTimeFormat = PickerTimeFormat.h_mm_tt;
    
    public PickerTimeFormat SelectedTimeFormat
    {
        get => _selectedTimeFormat;
        set
        {
            _selectedTimeFormat = value;
            OnPropertyChanged();
        }
    }
}
```

### XAML

```xaml
<picker:SfDateTimePicker 
    DateFormat="{Binding SelectedDateFormat}"
    TimeFormat="{Binding SelectedTimeFormat}" />
```

## Best Practices

1. **Match User Expectations**: Use formats familiar to your target audience
2. **Consistency**: Use the same format throughout your app
3. **Localization**: Consider using `Default` format for culture-aware apps
4. **Clarity**: Include month names for better readability when possible
5. **Context**: Use appropriate detail level (with/without seconds)
6. **Testing**: Test formats with different dates to ensure clarity (e.g., 01/02/2024 ambiguity)

## Common Patterns

### Pattern 1: US-Friendly
```csharp
DateFormat = PickerDateFormat.MMM_dd_yyyy;  // Mar/15/2024
TimeFormat = PickerTimeFormat.h_mm_tt;      // 2:30 PM
```

### Pattern 2: European-Friendly
```csharp
DateFormat = PickerDateFormat.dd_MMM_yyyy;  // 15/Mar/2024
TimeFormat = PickerTimeFormat.HH_mm;        // 14:30
```

### Pattern 3: Formal/Professional
```csharp
DateFormat = PickerDateFormat.dd_MMMM_yyyy; // 15/March/2024
TimeFormat = PickerTimeFormat.hh_mm_tt;     // 02:30 PM
```

### Pattern 4: Technical/ISO-Style
```csharp
DateFormat = PickerDateFormat.yyyy_MM_dd;   // 2024/03/15
TimeFormat = PickerTimeFormat.HH_mm_ss;     // 14:30:45
```
