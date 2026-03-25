# Appointments in .NET MAUI Scheduler

## Table of Contents

- [Overview](#overview)
- [Appointment Basics](#appointment-basics)
- [Scheduler Appointment Source Mapping](#scheduler-appointment-source-mapping)
- [Creating Business Objects](#creating-business-objects)
- [Appointment Types](#appointment-types)
  - [Normal Appointments](#normal-appointments)
  - [Spanned Appointments](#spanned-appointments)
  - [All-Day Appointments](#all-day-appointments)
  - [Read-Only Appointments](#read-only-appointments)
- [Recurrence Appointments](#recurrence-appointments)
  - [Recurrence Rules (RRULE)](#recurrence-rules-rrule)
  - [Creating Recurrence Appointments](#creating-recurrence-appointments)
  - [Business Object Recurrence](#business-object-recurrence)
- [Recurrence Pattern Exceptions](#recurrence-pattern-exceptions)
  - [Exception Dates](#exception-dates)
  - [Exception Appointments](#exception-appointments)
- [Appointment UI Rendering](#appointment-ui-rendering)
- [Appointment Appearance Customization](#appointment-appearance-customization)
- [Appointment Selection](#appointment-selection)
- [Appointment Borders](#appointment-borders)
- [Complete Code Examples](#complete-code-examples)
- [Troubleshooting](#troubleshooting)

## Overview

The .NET MAUI Scheduler (SfScheduler) control provides comprehensive appointment management capabilities, supporting normal appointments, all-day appointments, spanned appointments, recurring appointments, and recurrence exceptions. Appointments can be created using the built-in `SchedulerAppointment` class or by mapping custom business objects.

## Appointment Basics

The `SchedulerAppointment` class represents a scheduled event with the following key properties:

- **StartTime**: DateTime when the appointment begins
- **EndTime**: DateTime when the appointment ends
- **Subject**: Title or description of the appointment
- **Notes**: Additional details about the appointment
- **Location**: Physical or virtual location
- **IsAllDay**: Boolean indicating if appointment spans entire day
- **Background**: Color/brush for appointment background
- **TextColor**: Color for appointment text
- **RecurrenceRule**: RRULE string for recurring appointments
- **RecurrenceId**: Links exception appointments to pattern
- **RecurrenceExceptionDates**: Dates to exclude from recurrence
- **IsReadOnly**: Prevents appointment modification

### Basic Appointment Example

```xml
<scheduler:SfScheduler x:Name="Scheduler" View="Week">
</scheduler:SfScheduler>
```

```csharp
// Create appointment collection
var appointments = new ObservableCollection<SchedulerAppointment>();

// Add appointment
appointments.Add(new SchedulerAppointment()
{
    StartTime = DateTime.Today.AddHours(9),
    EndTime = DateTime.Today.AddHours(11),
    Subject = "Client Meeting",
    Location = "Hutchison road",
    Background = Brush.Orange,
    TextColor = Colors.White
});

// Assign to scheduler
Scheduler.AppointmentsSource = appointments;
```

## Scheduler Appointment Source Mapping

The Scheduler supports binding to custom business objects through the `AppointmentMapping` property. This maps properties from your business class to scheduler appointment properties.

### Available Mapping Properties

| Property | Description |
|----------|-------------|
| StartTime | Maps to appointment start date/time |
| EndTime | Maps to appointment end date/time |
| Subject | Maps to appointment title |
| Id | Maps to unique appointment identifier |
| Background | Maps to appointment background color |
| IsAllDay | Maps to all-day appointment indicator |
| RecurrenceRule | Maps to recurrence pattern |
| RecurrenceId | Maps to exception appointment link |
| RecurrenceExceptionDates | Maps to excluded dates |
| Notes | Maps to additional notes |
| Location | Maps to appointment location |
| IsReadOnly | Maps to read-only status |
| TextColor | Maps to text color |
| StartTimeZone | Maps to start timezone |
| EndTimeZone | Maps to end timezone |
| Stroke | Maps to border color |

## Creating Business Objects

Create a custom business object class to represent appointments:

```csharp
public class Meeting : INotifyPropertyChanged
{
    public DateTime From { get; set; }
    public DateTime To { get; set; }
    public bool IsAllDay { get; set; }
    public string EventName { get; set; }
    public string Notes { get; set; }
    public TimeZoneInfo StartTimeZone { get; set; }
    public TimeZoneInfo EndTimeZone { get; set; }
    public Brush Background { get; set; }
    public Color TextColor { get; set; }
    public object RecurrenceId { get; set; }
    public object Id { get; set; }
    public string RecurrenceRule { get; set; }
    public ObservableCollection<DateTime> RecurrenceExceptions { get; set; }
    
    public event PropertyChangedEventHandler PropertyChanged;
}
```

### Mapping Business Object

```xml
<scheduler:SfScheduler x:Name="Scheduler" View="Week">
    <scheduler:SfScheduler.AppointmentMapping>
        <scheduler:SchedulerAppointmentMapping
            Subject="EventName"
            StartTime="From"
            EndTime="To"
            Background="Background"
            IsAllDay="IsAllDay"
            StartTimeZone="StartTimeZone"
            EndTimeZone="EndTimeZone"
            TextColorMapping="TextColor"
            Id="Id"
            RecurrenceId="RecurrenceId"
            RecurrenceRule="RecurrenceRule"
            RecurrenceExceptionDates="RecurrenceExceptions"/>
    </scheduler:SfScheduler.AppointmentMapping>
</scheduler:SfScheduler>
```

```csharp
// Create and populate meetings
var meetings = new ObservableCollection<Meeting>();

var meeting = new Meeting
{
    From = DateTime.Today.AddHours(9),
    To = DateTime.Today.AddHours(10),
    EventName = "Meeting",
    Background = Brush.Orange,
    TextColor = Colors.White,
    Id = 1
};

meetings.Add(meeting);
Scheduler.AppointmentsSource = meetings;
```

> **Important**: When publishing in AOT mode on iOS and macOS, add `[Preserve(AllMembers = true)]` attribute to the model class to maintain appointment binding.

## Appointment Types

### Normal Appointments

Standard appointments displayed within time slots based on their duration.

```csharp
appointments.Add(new SchedulerAppointment()
{
    StartTime = DateTime.Today.AddHours(9),
    EndTime = DateTime.Today.AddHours(11),
    Subject = "Client Meeting",
    Location = "Hutchison road"
});
```

### Spanned Appointments

Appointments lasting longer than 24 hours. These render in the all-day panel but don't block time slots.

```csharp
appointments.Add(new SchedulerAppointment()
{
    StartTime = DateTime.Today.AddHours(9),
    EndTime = DateTime.Today.AddDays(2).AddHours(11),
    Subject = "Conference",
    Background = Brush.Orange
});
```

### All-Day Appointments

Appointments scheduled for an entire day. Set `IsAllDay = true` or create appointments spanning exactly 24 hours.

```csharp
appointments.Add(new SchedulerAppointment()
{
    StartTime = DateTime.Today,
    EndTime = DateTime.Today.AddDays(1),
    Subject = "Holiday",
    IsAllDay = true,
    Background = Brush.Green
});
```

> **Note**: Appointments spanning exactly 24 hours (e.g., 12/13/2021 12:00 AM to 12/14/2021 12:00 AM) are automatically treated as all-day appointments.

### Read-Only Appointments

Prevent appointment modification by setting `IsReadOnly = true`:

```csharp
appointments.Add(new SchedulerAppointment()
{
    StartTime = DateTime.Today.AddHours(9),
    EndTime = DateTime.Today.AddHours(10),
    Subject = "Client Meeting",
    IsReadOnly = true,
    Background = Brush.Orange
});
```

## Recurrence Appointments

### Recurrence Rules (RRULE)

The `RecurrenceRule` property uses RRULE format to define recurring patterns:

| Property | Description | Example |
|----------|-------------|---------|
| FREQ | Repeat type (DAILY, WEEKLY, MONTHLY, YEARLY) | FREQ=DAILY;INTERVAL=1 |
| INTERVAL | Interval between occurrences | FREQ=DAILY;INTERVAL=2 |
| COUNT | Number of occurrences | FREQ=DAILY;INTERVAL=1;COUNT=10 |
| UNTIL | End date (format: YYYYMMDDTHHMMSSZ) | FREQ=DAILY;UNTIL=20200725T103059Z |
| BYDAY | Days of week (MO,TU,WE,TH,FR,SA,SU) | FREQ=WEEKLY;BYDAY=MO,WE |
| BYMONTHDAY | Day of month (1-31, or -1 for last day) | FREQ=MONTHLY;BYMONTHDAY=3 |
| BYMONTH | Month index (1-12) | FREQ=YEARLY;BYMONTH=6 |
| BYSETPOS | Week position in month | FREQ=MONTHLY;BYDAY=MO;BYSETPOS=2 |

### Creating Recurrence Appointments

```csharp
var appointment = new SchedulerAppointment()
{
    StartTime = DateTime.Today.AddHours(9),
    EndTime = DateTime.Today.AddHours(10),
    Subject = "Occurs every alternate day",
    Id = 1,
    RecurrenceRule = "FREQ=DAILY;INTERVAL=2;COUNT=10"
};

appointments.Add(appointment);
Scheduler.AppointmentsSource = appointments;
```

### Business Object Recurrence

```csharp
var meeting = new Meeting
{
    From = DateTime.Today.AddHours(9),
    To = DateTime.Today.AddHours(10),
    EventName = "Daily Standup",
    Background = Brush.Orange,
    TextColor = Colors.White,
    RecurrenceRule = "FREQ=DAILY;INTERVAL=1;COUNT=10",
    Id = 1
};

meetings.Add(meeting);
Scheduler.AppointmentsSource = meetings;
```

### RRULE Helper Methods

**Parse RRULE to get recurrence properties:**

```csharp
var dateTime = DateTime.Today.AddHours(10);
var recurrenceRule = "FREQ=DAILY;INTERVAL=1;COUNT=3";
var recurrenceProperties = SchedulerRecurrenceManager.ParseRRule(recurrenceRule, dateTime);

// Access properties
// recurrenceProperties.RecurrenceType = SchedulerRecurrenceType.Daily
// recurrenceProperties.Interval = 1
// recurrenceProperties.RecurrenceCount = 3
```

**Get occurrence dates from RRULE:**

```csharp
var dateTime = DateTime.Today.AddHours(10);
var recurrenceRule = "FREQ=DAILY;INTERVAL=1;COUNT=3";
var dateCollection = SchedulerRecurrenceManager.GetDateTimeOccurrences(recurrenceRule, dateTime);
```

**Get occurrence appointment:**

```csharp
var dateTime = new DateTime(2022, 7, 22, 9, 0, 0);
var occurrenceAppointment = SchedulerRecurrenceManager.GetOccurrenceAppointment(
    Scheduler, 
    appointment, 
    dateTime
);
```

**Get pattern appointment from occurrence:**

```csharp
var patternAppointment = SchedulerRecurrenceManager.GetPatternAppointment(
    Scheduler, 
    occurrenceAppointment
);
```

**Generate RRULE from properties:**

```csharp
var recurrenceProperties = new SchedulerRecurrenceInfo
{
    RecurrenceType = SchedulerRecurrenceType.Daily,
    Interval = 2,
    RecurrenceCount = 3
};

var startTime = DateTime.Today.AddHours(9);
var endTime = DateTime.Today.AddHours(10);
var recurrenceRule = SchedulerRecurrenceManager.GenerateRRule(
    recurrenceProperties, 
    startTime, 
    endTime
);
```

## Recurrence Pattern Exceptions

### Exception Dates

Remove specific occurrences from a recurrence pattern:

```csharp
var schedulerAppointment = new SchedulerAppointment()
{
    Id = 1,
    StartTime = DateTime.Today.AddHours(9),
    EndTime = DateTime.Today.AddHours(10),
    Subject = "Daily scrum meeting",
    RecurrenceRule = "FREQ=DAILY;INTERVAL=1;COUNT=10"
};

// Add exception dates
DateTime exceptionDate = schedulerAppointment.StartTime.AddDays(3).Date;
schedulerAppointment.RecurrenceExceptionDates = new ObservableCollection<DateTime>()
{
    exceptionDate
};

appointments.Add(schedulerAppointment);
```

### Exception Appointments

Create modified occurrences by adding exception appointments:

```csharp
// Pattern appointment
var schedulerAppointment = new SchedulerAppointment()
{
    Id = 1,
    StartTime = DateTime.Today.AddHours(9),
    EndTime = DateTime.Today.AddHours(10),
    Subject = "Daily scrum meeting",
    RecurrenceRule = "FREQ=DAILY;INTERVAL=1;COUNT=10"
};

// Add exception date
DateTime changedDate = schedulerAppointment.StartTime.AddDays(3).Date;
schedulerAppointment.RecurrenceExceptionDates = new ObservableCollection<DateTime>()
{
    changedDate
};

appointments.Add(schedulerAppointment);

// Exception appointment with modified time
var exceptionAppointment = new SchedulerAppointment()
{
    Id = 2,
    Subject = "Scrum meeting - Changed Occurrence",
    StartTime = new DateTime(changedDate.Year, changedDate.Month, changedDate.Day, 11, 0, 0),
    EndTime = new DateTime(changedDate.Year, changedDate.Month, changedDate.Day, 12, 0, 0),
    Background = Brush.DeepPink,
    TextColor = Colors.White,
    RecurrenceId = 1  // Links to pattern appointment
};

appointments.Add(exceptionAppointment);
```

> **Important Exception Appointment Rules:**
> - Exception appointment `RecurrenceId` must match pattern appointment `Id`
> - Exception appointments don't have `RecurrenceRule` (reset to empty)
> - Exception appointments must have different `Id` from pattern
> - Exception appointments are normal appointments, not recurring
> - `RecurrenceExceptionDates` must be in UTC timezone

## Appointment UI Rendering

### Suspend and Resume Updates

Improve performance when adding multiple appointments:

```csharp
// Suspend updates
Scheduler.SuspendAppointmentViewUpdate();

// Add multiple appointments
for (int i = 0; i < e.NewVisibleDates.Count; i++)
{
    var visibleDate = e.NewVisibleDates[i].Date;
    var scheduleAppointment = new SchedulerAppointment()
    {
        StartTime = visibleDate.AddHours(10),
        EndTime = visibleDate.AddHours(12),
        Subject = visibleDate.ToString("dd/MM/yyyy"),
        Background = Colors.Red
    };
    appointments.Add(scheduleAppointment);
}

// Resume updates
Scheduler.ResumeAppointmentViewUpdate();
```

## Appointment Appearance Customization

### Using Text Style

```csharp
var appointmentTextStyle = new SchedulerTextStyle()
{
    TextColor = Colors.Red,
    FontSize = 12
};

Scheduler.AppointmentTextStyle = appointmentTextStyle;
```

### Using DataTemplate

```xml
<scheduler:SfScheduler x:Name="Scheduler" View="Week">
    <scheduler:SfScheduler.DaysView>
        <scheduler:SchedulerDaysView>
            <scheduler:SchedulerDaysView.AppointmentTemplate>
                <DataTemplate>
                    <Grid Background="MediumPurple">
                        <Label Text="{Binding Subject}" 
                               TextColor="White" 
                               HorizontalOptions="Center" 
                               VerticalOptions="Center" 
                               FontFamily="Bold"/>
                    </Grid>
                </DataTemplate>
            </scheduler:SchedulerDaysView.AppointmentTemplate>
        </scheduler:SchedulerDaysView>
    </scheduler:SfScheduler.DaysView>
</scheduler:SfScheduler>
```

> **Note**: Custom data objects can be accessed via `SchedulerAppointment.DataItem` in templates.

### Using DataTemplateSelector

```xml
<Grid.Resources>
    <DataTemplate x:Key="normalDateTemplate">
        <Grid Background="LightGreen">
            <Label Text="{Binding Subject}" TextColor="Black"/>
        </Grid>
    </DataTemplate>
    <DataTemplate x:Key="todayDateTemplate">
        <Grid Background="MediumPurple">
            <Label Text="{Binding Subject}" TextColor="White"/>
        </Grid>
    </DataTemplate>
    <local:AppointmentTemplateSelector 
        x:Key="appointmentTemplateSelector" 
        TodayDateTemplate="{StaticResource todayDateTemplate}" 
        NormalDateTemplate="{StaticResource normalDateTemplate}"/>
</Grid.Resources>

<scheduler:SfScheduler x:Name="Scheduler" View="Week">
    <scheduler:SfScheduler.DaysView>
        <scheduler:SchedulerDaysView 
            AppointmentTemplate="{StaticResource appointmentTemplateSelector}"/>
    </scheduler:SfScheduler.DaysView>
</scheduler:SfScheduler>
```

```csharp
public class AppointmentTemplateSelector : DataTemplateSelector
{
    public DataTemplate NormalDateTemplate { get; set; }
    public DataTemplate TodayDateTemplate { get; set; }
    
    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        var schedulerAppointment = item as SchedulerAppointment;
        if (schedulerAppointment.ActualStartTime.Date == DateTime.Today.Date)
            return TodayDateTemplate;
        return NormalDateTemplate;
    }
}
```

## Appointment Selection

Customize the selection background:

```xml
<scheduler:SfScheduler x:Name="Scheduler" 
                       SelectedAppointmentBackground="Orange">
</scheduler:SfScheduler>
```

```csharp
Scheduler.SelectedAppointmentBackground = Brush.Orange;
```

## Appointment Borders

Add borders to all appointments:

```csharp
var appointmentBorderStyle = new SchedulerAppointmentBorderStyle()
{
    Stroke = Colors.Red,
    CornerRadius = 5,
    StrokeThickness = 2
};

Scheduler.AppointmentBorderStyle = appointmentBorderStyle;
```

### Individual Appointment Borders

```csharp
appointments.Add(new SchedulerAppointment()
{
    Subject = "Meeting",
    StartTime = DateTime.Now,
    EndTime = DateTime.Now.AddHours(1),
    Stroke = Colors.Red  // Individual border color
});
```

> **Note**: Appointment border styles don't apply to month view indicator mode or when using `AppointmentTemplate`.

## Complete Code Examples

### Full Business Object Example

```csharp
// Business object
public class Meeting : INotifyPropertyChanged
{
    private DateTime from;
    private DateTime to;
    private string eventName;
    
    public DateTime From
    {
        get => from;
        set
        {
            from = value;
            OnPropertyChanged();
        }
    }
    
    public DateTime To
    {
        get => to;
        set
        {
            to = value;
            OnPropertyChanged();
        }
    }
    
    public string EventName
    {
        get => eventName;
        set
        {
            eventName = value;
            OnPropertyChanged();
        }
    }
    
    public Brush Background { get; set; }
    public Color TextColor { get; set; }
    public string RecurrenceRule { get; set; }
    public object Id { get; set; }
    public object RecurrenceId { get; set; }
    public ObservableCollection<DateTime> RecurrenceExceptions { get; set; }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}

// ViewModel
public class SchedulerViewModel
{
    public ObservableCollection<Meeting> Meetings { get; set; }
    
    public SchedulerViewModel()
    {
        Meetings = new ObservableCollection<Meeting>();
        
        // Add normal appointment
        Meetings.Add(new Meeting
        {
            From = DateTime.Today.AddHours(9),
            To = DateTime.Today.AddHours(10),
            EventName = "Team Meeting",
            Background = Brush.Orange,
            TextColor = Colors.White,
            Id = 1
        });
        
        // Add recurring appointment
        Meetings.Add(new Meeting
        {
            From = DateTime.Today.AddHours(14),
            To = DateTime.Today.AddHours(15),
            EventName = "Daily Standup",
            Background = Brush.Blue,
            TextColor = Colors.White,
            RecurrenceRule = "FREQ=DAILY;INTERVAL=1;COUNT=10",
            Id = 2
        });
        
        // Add all-day appointment
        Meetings.Add(new Meeting
        {
            From = DateTime.Today,
            To = DateTime.Today.AddDays(1),
            EventName = "Conference",
            Background = Brush.Green,
            TextColor = Colors.White,
            Id = 3
        });
    }
}
```

## Troubleshooting

### Common Issues

**Issue**: Appointments not displaying  
**Solution**: Ensure `AppointmentsSource` is properly set and appointments have valid StartTime/EndTime values.

**Issue**: Business object properties not updating  
**Solution**: Implement `INotifyPropertyChanged` interface in business object class.

**Issue**: Recurrence appointments showing incorrectly  
**Solution**: Verify RRULE syntax is correct. Use `ParseRRule` method to validate.

**Issue**: Exception appointments not linking to pattern  
**Solution**: Ensure `RecurrenceId` matches pattern appointment `Id` and exception date is in `RecurrenceExceptionDates`.

**Issue**: AOT compilation breaks appointment binding (iOS/macOS)  
**Solution**: Add `[Preserve(AllMembers = true)]` attribute to model class.

### Performance Tips

1. Use `SuspendAppointmentViewUpdate()` when adding multiple appointments
2. Implement `INotifyPropertyChanged` only for properties that change dynamically
3. Use appointment templates judiciously as they can impact performance
4. Consider using `MinimumAppointmentDuration` to improve readability
5. Keep recurrence rules simple when possible

### Best Practices

1. Always set unique `Id` values for appointments
2. Use UTC timezone for `RecurrenceExceptionDates`
3. Validate RRULE strings before assigning
4. Handle null checks when working with custom business objects
5. Use proper color contrasts for appointment backgrounds and text
6. Test recurrence patterns thoroughly across different timezones
7. Implement proper error handling for appointment creation/modification

## Related Resources

- [Scheduler Getting Started](getting-started.md)
- [Day and Week Views](day-week-views.md)
- [Timeline Views](timeline-views.md)
- [Appointment Interactions](appointment-interactions.md)

## GitHub Samples

- [Scheduler Appointment Sample](https://github.com/SyncfusionExamples/maui-scheduler-examples/tree/main/GettingStarted)
- [Business Object Sample](https://github.com/SyncfusionExamples/maui-scheduler-examples/tree/main/BusinessObject)
- [Recurring Appointment Sample](https://github.com/SyncfusionExamples/maui-scheduler-examples/tree/main/RecurringAppointment)
- [Recurrence Exception Sample](https://github.com/SyncfusionExamples/maui-scheduler-examples/tree/main/RecursiveExceptionAppointment)
