# Calendar and Scheduling Migration: Xamarin.Forms to .NET MAUI

Migration guide for calendar and scheduling controls from Xamarin.Forms to .NET MAUI.

## Table of Contents
- [Overview](#overview)
- [SfCalendar Migration](#sfcalendar-migration)
- [SfScheduler Migration](#sfscheduler-migration)
- [SfDatePicker Migration](#sfdatepicker-migration)
- [SfTimePicker Migration](#sftimepicker-migration)
- [SfDateTimeRangeSelector Migration](#sfdatetimerangeselector-migration)

## Overview

Calendar and scheduling controls have been updated with improved selection models and appointment management in MAUI.

## SfCalendar Migration

### Important Note About Appointments

**.NET MAUI SfCalendar** is selection-focused and **does NOT manage appointments** like Xamarin SfCalendar. For appointment/event management, use **.NET MAUI SfScheduler** instead.

### Namespace Changes

```csharp
// Xamarin
using Syncfusion.SfCalendar.XForms;

// MAUI
using Syncfusion.Maui.Calendar;
```

### Key Class Changes

| Xamarin | MAUI | Description |
|---------|------|-------------|
| `SelectionRange` | `CalendarDateRange` | Date range holder |
| `MonthViewSettings` | `CalendarMonthView` | Month view config |
| `YearViewSettings` | `CalendarYearView` | Year view config |
| `CalendarTappedEventArgs` | `CalendarTappedEventArgs` | Tap event args |
| `DayCellHoldingEventArgs` | `CalendarLongPressedEventArgs` | Long press event |
| `Month ChangedEventArgs` | `CalendarViewChangedEventArgs` | View changed event |
| `SelectionChangedEventArgs` | `CalendarSelectionChangedEventArgs` | Selection event |
| `ViewModeChangedEventArgs` | `CalendarViewChangedEventArgs` | View mode event |

### Removed Features

| Xamarin Feature | MAUI Status |
|----------------|-------------|
| `MonthLabelSettings` | Use properties from `MonthView` |
| `CalendarInlineEvent` | Not supported |
| `MonthEventParameters` | Not supported |
| Inline view | Not supported |
| Appointment management | Use SfScheduler instead |

### Migration Example

**Xamarin:**
```xml
<calendar:SfCalendar SelectionMode="Range"
                     ViewMode="MonthView"
                     ShowLeadingAndTrailingDays="True">
    <calendar:SfCalendar.MonthViewSettings>
        <calendar:MonthViewSettings DayHeaderFormat="EEE"/>
    </calendar:SfCalendar.MonthViewSettings>
</calendar:SfCalendar>
```

**.NET MAUI:**
```xml
<calendar:SfCalendar SelectionMode="Range"
                     View="Month"
                     ShowTrailingAndLeadingDates="True">
    <calendar:SfCalendar.MonthView>
        <calendar:CalendarMonthView DayFormat="ddd"/>
    </calendar:SfCalendar.MonthView>
</calendar:SfCalendar>
```

### Selection Handling

**Xamarin:**
```csharp
calendar.SelectionChanged += (s, e) =>
{
    SelectionRange range = e.NewValue as SelectionRange;
    DateTime start = range.StartDate;
    DateTime end = range.EndDate;
};
```

**.NET MAUI:**
```csharp
calendar.SelectionChanged += (s, e) =>
{
    CalendarDateRange range = e.NewValue as CalendarDateRange;
    DateTime? start = range.StartDate;
    DateTime? end = range.EndDate;
};
```

## SfScheduler Migration

### Component Rename

| Xamarin | MAUI |
|---------|------|
| SfSchedule | SfScheduler |

### Namespace Changes

```csharp
// Xamarin
using Syncfusion.SfSchedule.XForms;

// MAUI
using Syncfusion.Maui.Scheduler;
```

### Key Changes

- Improved appointment data binding
- Enhanced view navigation
- Better resource grouping
- Updated recurrence support
- Improved time zone handling

### Migration Example

**Xamarin:**
```xml
<schedule:SfSchedule ScheduleView="WeekView"
                     DataSource="{Binding Appointments}"/>
```

**.NET MAUI:**
```xml
<scheduler:SfScheduler View="Week"
                       AppointmentsSource="{Binding Appointments}"/>
```

### Appointment Mapping

**Xamarin:**
```csharp
schedule.AppointmentMapping = new ScheduleAppointmentMapping
{
    SubjectMapping = "Subject",
    StartTimeMapping = "From",
    EndTimeMapping = "To",
    ColorMapping = "Color"
};
```

**.NET MAUI:**
```csharp
scheduler.AppointmentMapping = new SchedulerAppointmentMapping
{
    Subject = "Subject",
    StartTime = "From",
    EndTime = "To",
    Background = "Color"
};
```

## SfDatePicker Migration

### Namespace Changes

```csharp
// Xamarin
using Syncfusion.XForms.Pickers;

// MAUI
using Syncfusion.Maui.Picker;
```

### Key Changes

Most properties maintained with minor naming updates for consistency.

### Migration Example

**Xamarin:**
```xml
<picker:SfDatePicker Date="{Binding SelectedDate}"
                     MinimumDate="{Binding MinDate}"
                     MaximumDate="{Binding MaxDate}"
                     Format="dd/MM/yyyy"/>
```

**.NET MAUI:**
```xml
<picker:SfDatePicker SelectedDate="{Binding SelectedDate}"
                     MinimumDate="{Binding MinDate}"
                     MaximumDate="{Binding MaxDate}"
                     Format="dd/MM/yyyy"/>
```

**Property Changes:**

| Xamarin | MAUI |
|---------|------|
| `Date` | `SelectedDate` |
| Other properties | Mostly unchanged |

## SfTimePicker Migration

### Namespace Changes

```csharp
// Same as DatePicker
using Syncfusion.Maui.Picker;
```

### Migration Example

**Xamarin:**
```xml
<picker:SfTimePicker Time="{Binding SelectedTime}"
                     Format="HH:mm"
                     ShowColumnHeader="True"/>
```

**.NET MAUI:**
```xml
<picker:SfTimePicker SelectedTime="{Binding SelectedTime}"
                     Format="HH:mm"
                     ShowColumnHeader="True"/>
```

**Property Changes:**

| Xamarin | MAUI |
|---------|------|
| `Time` | `SelectedTime` |

## SfDateTimeRangeSelector Migration

### Component Rename

| Xamarin | MAUI |
|---------|------|
| SfDateTimeRangeNavigator | SfDateTimeRangeSelector |

### Namespace Changes

```csharp
// Xamarin
using Syncfusion.SfChart.XForms;

// MAUI
using Syncfusion.Maui.Sliders;
```

### Migration Example

**Xamarin:**
```xml
<chart:SfDateTimeRangeNavigator Minimum="{Binding StartDate}"
                                Maximum="{Binding EndDate}"
                                RangeStart="{Binding RangeStart}"
                                RangeEnd="{Binding RangeEnd}"/>
```

**.NET MAUI:**
```xml
<sliders:SfDateTimeRangeSelector Minimum="{Binding StartDate}"
                                 Maximum="{Binding EndDate}"
                                 RangeStart="{Binding RangeStart}"
                                 RangeEnd="{Binding RangeEnd}"/>
```

## Common Migration Patterns

### Date Selection Binding

**Xamarin:**
```csharp
calendar.SelectedDate = DateTime.Now;
datePicker.Date = DateTime.Today;
```

**.NET MAUI:**
```csharp
calendar.SelectedDate = DateTime.Now;
datePicker.SelectedDate = DateTime.Today;
```

### Range Selection

**Xamarin:**
```csharp
var range = new SelectionRange(start DateTime, endDateTime);
calendar.SelectedRange = range;
```

**.NET MAUI:**
```csharp
var range = new CalendarDateRange(startDateTime, endDateTime);
calendar.SelectedDateRange = range;
```

### View Mode Changes

**Xamarin:**
```csharp
calendar.ViewMode = ViewMode.MonthView;
calendar.ViewMode = ViewMode.YearView;
```

**.NET MAUI:**
```csharp
calendar.View = CalendarView.Month;
calendar.View = CalendarView.Year;
```

## Troubleshooting

### Issue: Appointments not showing in Calendar

**Solution:** MAUI SfCalendar doesn't support appointments. Use SfScheduler:
```xml
<!-- Change from SfCalendar to SfScheduler for appointments -->
<scheduler:SfScheduler View="Month"
                       AppointmentsSource="{Binding Events}"/>
```

### Issue: SfSchedule not found

**Solution:** Renamed to `SfScheduler`:
```csharp
// Change
using Syncfusion.SfSchedule.XForms;
SfSchedule schedule = new SfSchedule();

// To
using Syncfusion.Maui.Scheduler;
SfScheduler scheduler = new SfScheduler();
```

### Issue: Date property not found

**Solution:** Use `SelectedDate`:
```csharp
// Change
datePicker.Date = DateTime.Now;

// To
datePicker.SelectedDate = DateTime.Now;
```

### Issue: SelectionRange not found

**Solution:** Use `CalendarDateRange`:
```csharp
// Change
SelectionRange range = new SelectionRange(start, end);

// To
CalendarDateRange range = new CalendarDateRange(start, end);
```

### Issue: SfDateTimeRangeNavigator not found

**Solution:** Renamed to `SfDateTimeRangeSelector` and moved namespace:
```csharp
// Change
using Syncfusion.SfChart.XForms;
SfDateTimeRangeNavigator nav = new SfDateTimeRangeNavigator();

// To
using Syncfusion.Maui.Sliders;
SfDateTimeRangeSelector selector = new SfDateTimeRangeSelector();
```

## Next Steps

1. Update NuGet packages:
   - `Syncfusion.Maui.Calendar`
   - `Syncfusion.Maui.Scheduler`
   - `Syncfusion.Maui.Picker`
   - `Syncfusion.Maui.Sliders`
2. Update namespaces
3. Replace SfSchedule → SfScheduler
4. Update Date → SelectedDate in pickers
5. Replace SelectionRange → CalendarDateRange
6. Move appointment logic from Calendar to Scheduler
7. Test date selection and navigation
