# Time Formatting in .NET MAUI TimePicker

## Table of Contents
- [Overview](#overview)
- [Format Property](#format-property)
- [Predefined Time Formats](#predefined-time-formats)
- [24-Hour Formats](#24-hour-formats)
- [12-Hour Formats with AM/PM](#12-hour-formats-with-ampm)
- [Partial Time Formats](#partial-time-formats)
- [Default Culture Format](#default-culture-format)
- [Format Usage Examples](#format-usage-examples)
- [Switching Formats Dynamically](#switching-formats-dynamically)
- [Best Practices](#best-practices)

## Overview

The TimePicker allows you to customize how time is displayed using the `Format` property. This property accepts predefined formats from the `PickerTimeFormat` enumeration, offering 14 different time display options.

**Default Format:** `HH_mm_ss` (24-hour format: hours:minutes:seconds)

## Format Property

**Namespace:** `Syncfusion.Maui.Picker`

**Property:**
```csharp
public PickerTimeFormat Format { get; set; }
```

**Type:** `PickerTimeFormat` (enum)

**XAML:**
```xml
<picker:SfTimePicker Format="hh_mm_tt" />
```

**C#:**
```csharp
timePicker.Format = PickerTimeFormat.hh_mm_tt;
```

## Predefined Time Formats

### Format Legend
- `H` = Hour (0-23) without leading zero
- `HH` = Hour (00-23) with leading zero
- `h` = Hour (1-12) without leading zero
- `hh` = Hour (01-12) with leading zero
- `mm` = Minutes (00-59) with leading zero
- `ss` = Seconds (00-59) with leading zero
- `fff` = Milliseconds (000-999)
- `tt` = AM/PM designator

## 24-Hour Formats

### 1. H_mm
**Format:** `H:mm`  
**Example:** `9:30`, `14:45`  
**Description:** 24-hour format without leading zero for hours, no seconds

```xml
<picker:SfTimePicker Format="H_mm" />
```

```csharp
timePicker.Format = PickerTimeFormat.H_mm;
// Displays: 9:30, 14:45
```

---

### 2. H_mm_ss
**Format:** `H:mm:ss`  
**Example:** `9:30:15`, `14:45:30`  
**Description:** 24-hour format without leading zero for hours, includes seconds

```xml
<picker:SfTimePicker Format="H_mm_ss" />
```

```csharp
timePicker.Format = PickerTimeFormat.H_mm_ss;
// Displays: 9:30:15, 14:45:30
```

---

### 3. HH_mm
**Format:** `HH:mm`  
**Example:** `09:30`, `14:45`  
**Description:** 24-hour format with leading zero for hours, no seconds

```xml
<picker:SfTimePicker Format="HH_mm" />
```

```csharp
timePicker.Format = PickerTimeFormat.HH_mm;
// Displays: 09:30, 14:45
```

---

### 4. HH_mm_ss (Default)
**Format:** `HH:mm:ss`  
**Example:** `09:30:15`, `14:45:30`  
**Description:** 24-hour format with leading zero for hours, includes seconds

```xml
<picker:SfTimePicker Format="HH_mm_ss" />
```

```csharp
timePicker.Format = PickerTimeFormat.HH_mm_ss;
// Displays: 09:30:15, 14:45:30
```

---

### 5. HH_mm_ss_fff
**Format:** `HH:mm:ss.fff`  
**Example:** `09:30:15.500`, `14:45:30.250`  
**Description:** 24-hour format with milliseconds

```xml
<picker:SfTimePicker Format="HH_mm_ss_fff" />
```

```csharp
timePicker.Format = PickerTimeFormat.HH_mm_ss_fff;
// Displays: 09:30:15.500, 14:45:30.250
```

## 12-Hour Formats with AM/PM

### 6. h_mm_tt
**Format:** `h:mm tt`  
**Example:** `9:30 AM`, `2:45 PM`  
**Description:** 12-hour format without leading zero, no seconds

```xml
<picker:SfTimePicker Format="h_mm_tt" />
```

```csharp
timePicker.Format = PickerTimeFormat.h_mm_tt;
// Displays: 9:30 AM, 2:45 PM
```

---

### 7. h_mm_ss_tt
**Format:** `h:mm:ss tt`  
**Example:** `9:30:15 AM`, `2:45:30 PM`  
**Description:** 12-hour format without leading zero, includes seconds

```xml
<picker:SfTimePicker Format="h_mm_ss_tt" />
```

```csharp
timePicker.Format = PickerTimeFormat.h_mm_ss_tt;
// Displays: 9:30:15 AM, 2:45:30 PM
```

---

### 8. hh_mm_tt
**Format:** `hh:mm tt`  
**Example:** `09:30 AM`, `02:45 PM`  
**Description:** 12-hour format with leading zero, no seconds

```xml
<picker:SfTimePicker Format="hh_mm_tt" />
```

```csharp
timePicker.Format = PickerTimeFormat.hh_mm_tt;
// Displays: 09:30 AM, 02:45 PM
```

---

### 9. hh_mm_ss_tt
**Format:** `hh:mm:ss tt`  
**Example:** `09:30:15 AM`, `02:45:30 PM`  
**Description:** 12-hour format with leading zero, includes seconds

```xml
<picker:SfTimePicker Format="hh_mm_ss_tt" />
```

```csharp
timePicker.Format = PickerTimeFormat.hh_mm_ss_tt;
// Displays: 09:30:15 AM, 02:45:30 PM
```

---

### 10. hh_mm_ss_fff_tt
**Format:** `hh:mm:ss.fff tt`  
**Example:** `09:30:15.500 AM`, `02:45:30.250 PM`  
**Description:** 12-hour format with milliseconds and AM/PM

```xml
<picker:SfTimePicker Format="hh_mm_ss_fff_tt" />
```

```csharp
timePicker.Format = PickerTimeFormat.hh_mm_ss_fff_tt;
// Displays: 09:30:15.500 AM, 02:45:30.250 PM
```

---

### 11. hh_tt
**Format:** `hh tt`  
**Example:** `09 AM`, `02 PM`  
**Description:** 12-hour format showing only hours with AM/PM

```xml
<picker:SfTimePicker Format="hh_tt" />
```

```csharp
timePicker.Format = PickerTimeFormat.hh_tt;
// Displays: 09 AM, 02 PM
```

## Partial Time Formats

### 12. mm_ss
**Format:** `mm:ss`  
**Example:** `30:15`, `45:30`  
**Description:** Minutes and seconds only (useful for timers/countdowns)

```xml
<picker:SfTimePicker Format="mm_ss" />
```

```csharp
timePicker.Format = PickerTimeFormat.mm_ss;
// Displays: 30:15, 45:30
```

---

### 13. mm_ss_fff
**Format:** `mm:ss.fff`  
**Example:** `30:15.500`, `45:30.250`  
**Description:** Minutes, seconds, and milliseconds

```xml
<picker:SfTimePicker Format="mm_ss_fff" />
```

```csharp
timePicker.Format = PickerTimeFormat.mm_ss_fff;
// Displays: 30:15.500, 45:30.250
```

---

### 14. ss_fff
**Format:** `ss.fff`  
**Example:** `15.500`, `30.250`  
**Description:** Seconds and milliseconds only

```xml
<picker:SfTimePicker Format="ss_fff" />
```

```csharp
timePicker.Format = PickerTimeFormat.ss_fff;
// Displays: 15.500, 30.250
```

## Default Culture Format

### 15. Default
**Format:** Culture-dependent  
**Example:** Varies by system culture  
**Description:** Uses the device's current culture time format

```xml
<picker:SfTimePicker Format="Default" />
```

```csharp
timePicker.Format = PickerTimeFormat.Default;
// Displays based on device culture settings
// US: 9:30:15 AM
// Germany: 09:30:15
// Japan: 09:30:15
```

## Format Usage Examples

### Example 1: Appointment Scheduler (12-hour)
```xml
<picker:SfTimePicker x:Name="appointmentPicker"
                     Format="hh_mm_tt"
                     SelectedTime="09:30:00">
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView Text="Appointment Time" Height="40" />
    </picker:SfTimePicker.HeaderView>
</picker:SfTimePicker>
```

**Result:** `09:30 AM`

---

### Example 2: Military/24-Hour Time
```xml
<picker:SfTimePicker x:Name="militaryPicker"
                     Format="HH_mm"
                     SelectedTime="14:30:00">
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView Text="Shift Time" Height="40" />
    </picker:SfTimePicker.HeaderView>
</picker:SfTimePicker>
```

**Result:** `14:30`

---

### Example 3: Stopwatch/Timer with Milliseconds
```xml
<picker:SfTimePicker x:Name="stopwatchPicker"
                     Format="mm_ss_fff"
                     SelectedTime="00:05:30.500">
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView Text="Lap Time" Height="40" />
    </picker:SfTimePicker.HeaderView>
</picker:SfTimePicker>
```

**Result:** `05:30.500`

---

### Example 4: Alarm Clock
```xml
<picker:SfTimePicker x:Name="alarmPicker"
                     Format="h_mm_tt"
                     SelectedTime="06:00:00">
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView Text="Set Alarm" Height="40" />
    </picker:SfTimePicker.HeaderView>
</picker:SfTimePicker>
```

**Result:** `6:00 AM`

## Switching Formats Dynamically

You can change the format programmatically based on user preference or application logic:

**XAML:**
```xml
<StackLayout Padding="20" Spacing="10">
    
    <Label Text="Time Format Selector" FontSize="18" FontAttributes="Bold" />
    
    <Picker x:Name="formatPicker" 
            Title="Select Format"
            SelectedIndexChanged="OnFormatChanged">
        <Picker.Items>
            <x:String>12-Hour (9:30 AM)</x:String>
            <x:String>24-Hour (09:30)</x:String>
            <x:String>With Seconds (9:30:15 AM)</x:String>
            <x:String>Military (14:30)</x:String>
        </Picker.Items>
    </Picker>
    
    <picker:SfTimePicker x:Name="timePicker"
                         Format="hh_mm_tt"
                         HeightRequest="250" />
    
</StackLayout>
```

**C#:**
```csharp
private void OnFormatChanged(object sender, EventArgs e)
{
    switch (formatPicker.SelectedIndex)
    {
        case 0: // 12-Hour
            timePicker.Format = PickerTimeFormat.h_mm_tt;
            break;
        case 1: // 24-Hour
            timePicker.Format = PickerTimeFormat.HH_mm;
            break;
        case 2: // With Seconds
            timePicker.Format = PickerTimeFormat.h_mm_ss_tt;
            break;
        case 3: // Military
            timePicker.Format = PickerTimeFormat.HH_mm_ss;
            break;
    }
}
```

## Best Practices

### 1. Choose Format Based on Use Case
- **Appointments/Alarms:** Use `h_mm_tt` or `hh_mm_tt` (12-hour with AM/PM)
- **International/Business:** Use `HH_mm` or `HH_mm_ss` (24-hour format)
- **Timers/Stopwatches:** Use `mm_ss` or `mm_ss_fff`
- **Precise Timing:** Use formats with `fff` for milliseconds

### 2. Consider User Locale
```csharp
// Respect user's system preference
if (CultureInfo.CurrentCulture.DateTimeFormat.ShortTimePattern.Contains("tt"))
{
    // User prefers 12-hour format
    timePicker.Format = PickerTimeFormat.hh_mm_tt;
}
else
{
    // User prefers 24-hour format
    timePicker.Format = PickerTimeFormat.HH_mm;
}
```

### 3. Match Format with Intervals
When using intervals, ensure format displays the relevant components:

```xml
<!-- Minute intervals: use format without seconds -->
<picker:SfTimePicker Format="hh_mm_tt" 
                     MinuteInterval="15" />

<!-- Second intervals: include seconds in format -->
<picker:SfTimePicker Format="hh_mm_ss_tt" 
                     SecondInterval="30" />

<!-- Millisecond intervals: include milliseconds -->
<picker:SfTimePicker Format="HH_mm_ss_fff" 
                     MilliSecondInterval="100" />
```

### 4. Display Format Consistency
Maintain consistent format across your application's time pickers for better UX.

### 5. Format and Column Display
The format determines which columns are displayed:
- `HH_mm` → Shows Hour and Minute columns
- `HH_mm_ss` → Shows Hour, Minute, and Second columns
- `HH_mm_ss_fff` → Shows Hour, Minute, Second, and Millisecond columns
- `hh_mm_tt` → Shows Hour, Minute, and AM/PM columns

## Format Comparison Table

| Format | Display Example | Use Case | Columns Shown |
|--------|----------------|----------|---------------|
| `H_mm` | 9:30 | Simple 24-hour | Hour, Minute |
| `HH_mm` | 09:30 | Standard 24-hour | Hour, Minute |
| `HH_mm_ss` | 09:30:15 | Full 24-hour | Hour, Minute, Second |
| `h_mm_tt` | 9:30 AM | Simple 12-hour | Hour, Minute, AM/PM |
| `hh_mm_tt` | 09:30 AM | Standard 12-hour | Hour, Minute, AM/PM |
| `hh_mm_ss_tt` | 09:30:15 AM | Full 12-hour | Hour, Minute, Second, AM/PM |
| `HH_mm_ss_fff` | 09:30:15.500 | Precise 24-hour | Hour, Minute, Second, Millisecond |
| `hh_mm_ss_fff_tt` | 09:30:15.500 AM | Precise 12-hour | Hour, Minute, Second, Millisecond, AM/PM |
| `mm_ss` | 30:15 | Timer/Duration | Minute, Second |
| `mm_ss_fff` | 30:15.500 | Precise Timer | Minute, Second, Millisecond |
| `ss_fff` | 15.500 | Lap Time | Second, Millisecond |
| `hh_tt` | 09 AM | Hour Only | Hour, AM/PM |

## Edge Cases and Considerations

### 1. Format Does Not Affect SelectedTime Value
The `Format` property only affects display. The `SelectedTime` property always contains the full TimeSpan value:

```csharp
timePicker.Format = PickerTimeFormat.hh_mm_tt; // Display format
timePicker.SelectedTime = new TimeSpan(14, 30, 45); // Internal value

// Displays: 02:30 PM
// SelectedTime.Value: 14:30:45 (full precision maintained)
```

### 2. Partial Formats and Full Time Values
Even with partial formats like `mm_ss`, you can still set and retrieve the full TimeSpan:

```csharp
timePicker.Format = PickerTimeFormat.mm_ss;
timePicker.SelectedTime = new TimeSpan(2, 30, 15); // 2 hours, 30 minutes, 15 seconds

// Displays: 30:15 (only minutes and seconds shown)
// SelectedTime.Value: 02:30:15 (complete value preserved)
```

### 3. Culture-Dependent AM/PM Display
The AM/PM designator text adapts to the device culture when using `tt` formats:
- English: AM/PM
- Spanish: a. m./p. m.
- German: vorm./nachm.

## Summary

The TimePicker's format system provides flexibility for various time display scenarios:
- **14 predefined formats** cover most common use cases
- **24-hour formats** for international applications
- **12-hour formats with AM/PM** for regional preferences
- **Partial formats** for timers and specialized time entry
- **Millisecond precision** for high-accuracy requirements
- **Culture-aware formatting** with Default format option

Choose the format that best matches your application's requirements and user expectations.
