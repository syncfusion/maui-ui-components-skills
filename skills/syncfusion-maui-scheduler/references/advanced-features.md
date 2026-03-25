# Advanced Features in .NET MAUI Scheduler

## Table of Contents

- [Overview](#overview)
- [Events](#events)
  - [Interaction Events](#interaction-events)
  - [View Changed Event](#view-changed-event)
  - [Commands (MVVM Support)](#commands-mvvm-support)
- [Load On Demand](#load-on-demand)
  - [QueryAppointments Event](#queryappointments-event)
  - [QueryAppointments Command](#queryappointments-command)
  - [Busy Indicator](#busy-indicator)
- [Appointment Reminders](#appointment-reminders)
  - [Enable Reminders](#enable-reminders)
  - [Reminder Configuration](#reminder-configuration)
  - [ReminderAlertOpening Event](#reminderalertopening-event)
  - [Dismiss and Snooze](#dismiss-and-snooze)
- [Time Zone Support](#time-zone-support)
  - [Appointments in Different Time Zones](#appointments-in-different-time-zones)
  - [Scheduler Time Zone](#scheduler-time-zone)
  - [Client vs Scheduler Time Zone](#client-vs-scheduler-time-zone)
- [Complete Examples](#complete-examples)
- [Troubleshooting](#troubleshooting)

## Overview

The .NET MAUI Scheduler provides advanced features for handling user interactions, loading appointments efficiently, managing reminders, and supporting multiple time zones. These features enable complex scheduling scenarios and improve performance with large datasets.

## Events

### Interaction Events

The scheduler provides events to handle user interactions with scheduler elements.

#### Tapped Event

Triggered when any scheduler element is tapped:

```xml
<scheduler:SfScheduler x:Name="scheduler" 
                       Tapped="OnSchedulerTapped">
</scheduler:SfScheduler>
```

```csharp
scheduler.Tapped += OnSchedulerTapped;

private void OnSchedulerTapped(object sender, SchedulerTappedEventArgs e)
{
    var appointments = e.Appointments;     // Tapped appointments
    var selectedDate = e.Date;             // Tapped date
    var element = e.Element;               // Tapped element
    var weekNumber = e.WeekNumber;         // Tapped week number
    
    // Handle tap logic
    if (appointments != null && appointments.Count > 0)
    {
        // Handle appointment tap
    }
    else
    {
        // Handle cell tap
    }
}
```

**Event args properties:**
- **Appointments**: Selected appointments
- **Date**: Selected date
- **Element**: Scheduler element tapped
- **WeekNumber**: Week number (if applicable)

#### DoubleTapped Event

Triggered when scheduler element is double-tapped:

```csharp
scheduler.DoubleTapped += OnSchedulerDoubleTapped;

private void OnSchedulerDoubleTapped(object sender, SchedulerDoubleTappedEventArgs e)
{
    var appointments = e.Appointments;
    var selectedDate = e.Date;
    var element = e.Element;
    
    // Open appointment details on double-tap
    if (appointments != null && appointments.Count > 0)
    {
        OpenAppointmentDetails(appointments[0]);
    }
}
```

#### LongPressed Event

Triggered when scheduler element is long-pressed:

```csharp
scheduler.LongPressed += OnSchedulerLongPressed;

private void OnSchedulerLongPressed(object sender, SchedulerLongPressedEventArgs e)
{
    var appointments = e.Appointments;
    var selectedDate = e.Date;
    var element = e.Element;
    
    // Show context menu on long press
    ShowContextMenu(e.Date, appointments);
}
```

#### SelectionChanged Event

Triggered when cell selection changes:

```csharp
scheduler.SelectionChanged += OnSchedulerSelectionChanged;

private void OnSchedulerSelectionChanged(object sender, SchedulerSelectionChangedEventArgs e)
{
    var oldDateTime = e.OldValue;
    var newDateTime = e.NewValue;
    
    // Update selected date display
    UpdateSelectedDateDisplay(newDateTime);
}
```

**Note:** `Tapped` event fires first, followed by `SelectionChanged`.

### View Changed Event

Notifies when the scheduler view changes (swipe or view switch):

```xml
<scheduler:SfScheduler x:Name="scheduler" 
                       ViewChanged="OnSchedulerViewChanged">
</scheduler:SfScheduler>
```

```csharp
scheduler.ViewChanged += OnSchedulerViewChanged;

private void OnSchedulerViewChanged(object sender, SchedulerViewChangedEventArgs e)
{
    var oldVisibleDates = e.OldVisibleDates;
    var newVisibleDates = e.NewVisibleDates;
    var oldView = e.OldView;
    var newView = e.NewView;
    
    // Load appointments for new visible dates
    LoadAppointmentsForDateRange(newVisibleDates);
}
```

**Event args properties:**
- **OldVisibleDates**: Previous visible date range
- **NewVisibleDates**: New visible date range
- **OldView**: Previous scheduler view
- **NewView**: Current scheduler view

**Note:** When view changes, `SelectableDayPredicate` is called to determine selectable cells.

### Commands (MVVM Support)

Scheduler supports MVVM pattern with commands for all events.

#### TappedCommand

```xml
<scheduler:SfScheduler x:Name="scheduler"
                       TappedCommand="{Binding SchedulerTappedCommand}">
    <scheduler:SfScheduler.BindingContext>
        <local:SchedulerViewModel/>
    </scheduler:SfScheduler.BindingContext>
</scheduler:SfScheduler>
```

```csharp
public class SchedulerViewModel
{
    public ICommand SchedulerTappedCommand { get; set; }
    
    public SchedulerViewModel()
    {
        SchedulerTappedCommand = new Command<SchedulerTappedEventArgs>(ExecuteTapped);
    }
    
    private void ExecuteTapped(SchedulerTappedEventArgs args)
    {
        var selectedDate = args.Date;
        // Handle tap in ViewModel
    }
}
```

#### Other Commands

- **DoubleTappedCommand**: `Command<SchedulerDoubleTappedEventArgs>`
- **LongPressedCommand**: `Command<SchedulerLongPressedEventArgs>`
- **ViewChangedCommand**: `Command<SchedulerViewChangedEventArgs>`
- **SelectionChangedCommand**: `Command<SchedulerSelectionChangedEventArgs>`

## Load On Demand

Improve performance by loading appointments only for visible dates.

### QueryAppointments Event

Triggered when view or visible dates change:

```xml
<scheduler:SfScheduler x:Name="scheduler" 
                       View="Week"
                       QueryAppointments="OnSchedulerQueryAppointments">
</scheduler:SfScheduler>
```

```csharp
scheduler.QueryAppointments += OnSchedulerQueryAppointments;

private async void OnSchedulerQueryAppointments(object sender, SchedulerQueryAppointmentsEventArgs e)
{
    // Show loading indicator
    scheduler.ShowBusyIndicator = true;
    
    // Simulate async data load
    await Task.Delay(500);
    
    // Generate/fetch appointments for visible dates
    var appointments = GenerateAppointments(e.VisibleDates);
    
    // For Agenda view, add to existing collection
    if (scheduler.View == SchedulerView.Agenda)
    {
        foreach (var appointment in appointments)
        {
            ((ObservableCollection<SchedulerAppointment>)scheduler.AppointmentsSource).Add(appointment);
        }
    }
    else
    {
        // For other views, replace entire collection
        scheduler.AppointmentsSource = appointments;
    }
    
    // Hide loading indicator
    scheduler.ShowBusyIndicator = false;
}

private ObservableCollection<SchedulerAppointment> GenerateAppointments(List<DateTime> visibleDates)
{
    var appointments = new ObservableCollection<SchedulerAppointment>();
    var random = new Random();
    
    foreach (var date in visibleDates)
    {
        // Create appointments for each visible date
        for (int i = 0; i < random.Next(1, 5); i++)
        {
            appointments.Add(new SchedulerAppointment
            {
                StartTime = date.AddHours(random.Next(9, 16)),
                EndTime = date.AddHours(random.Next(10, 17)),
                Subject = $"Appointment {i + 1}",
                Background = GetRandomColor()
            });
        }
    }
    
    return appointments;
}
```

**Trigger conditions:**
- `ViewChanged` event triggers `QueryAppointments`
- Adding/removing appointments in visible dates does NOT trigger (already loaded)
- **Agenda view**: Add to existing `AppointmentsSource`
- **Other views**: Reset `AppointmentsSource` for new range

### QueryAppointments Command

MVVM pattern for load on demand:

```xml
<scheduler:SfScheduler x:Name="scheduler"
                       View="Week"
                       AppointmentsSource="{Binding Events}"
                       ShowBusyIndicator="{Binding ShowBusyIndicator}"
                       QueryAppointmentsCommand="{Binding QueryAppointmentsCommand}">
    <scheduler:SfScheduler.BindingContext>
        <local:LoadOnDemandViewModel/>
    </scheduler:SfScheduler.BindingContext>
</scheduler:SfScheduler>
```

```csharp
public class LoadOnDemandViewModel : INotifyPropertyChanged
{
    private bool showBusyIndicator;
    private ObservableCollection<SchedulerAppointment> events;
    
    public ICommand QueryAppointmentsCommand { get; set; }
    
    public ObservableCollection<SchedulerAppointment> Events
    {
        get => events;
        set
        {
            events = value;
            RaisePropertyChanged(nameof(Events));
        }
    }
    
    public bool ShowBusyIndicator
    {
        get => showBusyIndicator;
        set
        {
            showBusyIndicator = value;
            RaisePropertyChanged(nameof(ShowBusyIndicator));
        }
    }
    
    public LoadOnDemandViewModel()
    {
        QueryAppointmentsCommand = new Command<object>(LoadMoreAppointments);
    }
    
    private async void LoadMoreAppointments(object obj)
    {
        ShowBusyIndicator = true;
        await Task.Delay(1000);
        
        var args = (SchedulerQueryAppointmentsEventArgs)obj;
        Events = GenerateAppointments(args.VisibleDates);
        
        ShowBusyIndicator = false;
    }
    
    // INotifyPropertyChanged implementation
    public event PropertyChangedEventHandler PropertyChanged;
    private void RaisePropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Busy Indicator

Show loading animation during appointment load:

```xml
<scheduler:SfScheduler x:Name="scheduler" 
                       ShowBusyIndicator="true">
</scheduler:SfScheduler>
```

```csharp
// Start busy indicator
scheduler.ShowBusyIndicator = true;

// Load data
await LoadAppointmentsAsync();

// Stop busy indicator
scheduler.ShowBusyIndicator = false;
```

#### Custom Busy Indicator

```xml
<scheduler:SfScheduler x:Name="scheduler" ShowBusyIndicator="true">
    <scheduler:SfScheduler.BusyIndicatorTemplate>
        <DataTemplate>
            <Grid Background="LightGray" Opacity="0.3">
                <VerticalStackLayout HorizontalOptions="Center" 
                                    VerticalOptions="Center"
                                    Spacing="10">
                    <ActivityIndicator IsRunning="true" 
                                      Color="Blue"
                                      HeightRequest="40"
                                      WidthRequest="40"/>
                    <Label Text="Loading appointments..." 
                          TextColor="Blue"
                          FontSize="14"/>
                </VerticalStackLayout>
            </Grid>
        </DataTemplate>
    </scheduler:SfScheduler.BusyIndicatorTemplate>
</scheduler:SfScheduler>
```

## Appointment Reminders

### Enable Reminders

```xml
<scheduler:SfScheduler x:Name="scheduler" 
                       View="Week"
                       EnableReminder="true"
                       ReminderAlertOpening="OnReminderAlertOpening">
</scheduler:SfScheduler>
```

```csharp
scheduler.EnableReminder = true;
scheduler.ReminderAlertOpening += OnReminderAlertOpening;
```

### Reminder Configuration

Add reminders to appointments using `SchedulerReminder`:

```csharp
var appointments = new ObservableCollection<SchedulerAppointment>();

// Normal appointment with reminder
appointments.Add(new SchedulerAppointment
{
    StartTime = DateTime.Now.AddMinutes(30),
    EndTime = DateTime.Now.AddHours(1),
    Subject = "Team Meeting",
    Reminders = new ObservableCollection<SchedulerReminder>
    {
        new SchedulerReminder 
        { 
            TimeBeforeStart = new TimeSpan(0, 15, 0) // 15 minutes before
        }
    }
});

// All-day appointment with reminder
appointments.Add(new SchedulerAppointment
{
    StartTime = DateTime.Today.AddDays(1),
    EndTime = DateTime.Today.AddDays(1),
    Subject = "Conference",
    IsAllDay = true,
    Reminders = new ObservableCollection<SchedulerReminder>
    {
        new SchedulerReminder 
        { 
            TimeBeforeStart = new TimeSpan(1, 0, 0, 0) // 1 day before
        }
    }
});

// Recurring appointment with reminder
appointments.Add(new SchedulerAppointment
{
    StartTime = DateTime.Today.AddHours(9),
    EndTime = DateTime.Today.AddHours(10),
    Subject = "Daily Standup",
    RecurrenceRule = "FREQ=DAILY;COUNT=30",
    Reminders = new ObservableCollection<SchedulerReminder>
    {
        new SchedulerReminder 
        { 
            TimeBeforeStart = new TimeSpan(0, 5, 0) // 5 minutes before
        }
    }
});

scheduler.AppointmentsSource = appointments;
```

**SchedulerReminder properties:**
- **TimeBeforeStart**: Time interval before appointment start
- **AlertTime**: Calculated reminder time (read-only)
- **Appointment**: Associated appointment (read-only)
- **DataItem**: Custom reminder data object (read-only)
- **IsDismissed**: Whether reminder is dismissed

#### Business Object Mapping

```csharp
// Custom reminder class
public class CustomReminder
{
    public TimeSpan TimeBeforeStart { get; set; }
    public bool IsDismissed { get; set; }
}

// Custom appointment class
public class Meeting
{
    public DateTime From { get; set; }
    public DateTime To { get; set; }
    public string EventName { get; set; }
    public ObservableCollection<CustomReminder> Reminders { get; set; }
}
```

**Mapping:**

```xml
<scheduler:SfScheduler EnableReminder="true">
    <scheduler:SfScheduler.AppointmentMapping>
        <scheduler:SchedulerAppointmentMapping
            Subject="EventName"
            StartTime="From"
            EndTime="To"
            Reminders="Reminders">
            <scheduler:SchedulerAppointmentMapping.ReminderMapping>
                <scheduler:SchedulerReminderMapping 
                    TimeBeforeStart="TimeBeforeStart"
                    IsDismissed="IsDismissed"/>
            </scheduler:SchedulerAppointmentMapping.ReminderMapping>
        </scheduler:SchedulerAppointmentMapping>
    </scheduler:SfScheduler.AppointmentMapping>
</scheduler:SfScheduler>
```

**Note:** Add `[Preserve(AllMembers = true)]` to custom classes for AOT (iOS/macOS).

### ReminderAlertOpening Event

Triggered when reminder time reaches:

```csharp
private async void OnReminderAlertOpening(object sender, ReminderAlertOpeningEventArgs e)
{
    var reminders = e.Reminders;
    
    if (reminders.Count > 0)
    {
        var reminder = reminders[0];
        var appointment = reminder.Appointment;
        
        // Show reminder dialog
        bool snooze = await DisplayAlert(
            "Reminder",
            $"{appointment.Subject}\n{appointment.StartTime:g}",
            "Snooze",
            "Dismiss"
        );
        
        if (!snooze)
        {
            // Dismiss reminder
            DismissReminder(reminder);
        }
        else
        {
            // Snooze for 5 minutes
            SnoozeReminder(reminder, TimeSpan.FromMinutes(5));
        }
    }
}
```

### Dismiss and Snooze

#### Dismiss Reminder

**Normal appointment:**

```csharp
private void DismissReminder(SchedulerReminder reminder)
{
    reminder.IsDismissed = true;
}
```

**Recurring appointment (current occurrence only):**

```csharp
private void DismissRecurringReminder(SchedulerReminder reminder)
{
    var appointments = scheduler.AppointmentsSource as ObservableCollection<SchedulerAppointment>;
    var patternAppointment = appointments.FirstOrDefault(x => x.Id == reminder.Appointment.Id);
    
    if (patternAppointment != null)
    {
        DateTime occurrenceDate = reminder.Appointment.StartTime;
        
        // Add exception date
        patternAppointment.RecurrenceExceptionDates = new ObservableCollection<DateTime>
        {
            occurrenceDate
        };
        
        // Create exception appointment with dismissed reminder
        var exceptionAppointment = new SchedulerAppointment
        {
            Id = Guid.NewGuid().GetHashCode(),
            Subject = patternAppointment.Subject,
            StartTime = occurrenceDate,
            EndTime = reminder.Appointment.EndTime,
            Background = patternAppointment.Background,
            RecurrenceId = patternAppointment.Id,
            Reminders = new ObservableCollection<SchedulerReminder>
            {
                new SchedulerReminder 
                { 
                    TimeBeforeStart = reminder.TimeBeforeStart,
                    IsDismissed = true 
                }
            }
        };
        
        appointments.Add(exceptionAppointment);
    }
}
```

#### Snooze Reminder

```csharp
private void SnoozeReminder(SchedulerReminder reminder, TimeSpan snoozeTime)
{
    var appointment = reminder.Appointment;
    
    // Future appointment
    if (appointment.ActualStartTime > DateTime.Now && !appointment.IsAllDay)
    {
        reminder.TimeBeforeStart = appointment.StartTime - reminder.AlertTime - snoozeTime;
    }
    // All-day appointment
    else if (appointment.IsAllDay)
    {
        reminder.TimeBeforeStart = appointment.StartTime.Date.AddSeconds(DateTime.Now.Second) 
                                  - DateTime.Now - snoozeTime;
    }
    // Overdue appointment
    else
    {
        reminder.TimeBeforeStart = appointment.StartTime.AddSeconds(DateTime.Now.Second) 
                                  - DateTime.Now - snoozeTime;
    }
    
    // Handle recurring appointments
    if (!string.IsNullOrEmpty(appointment.RecurrenceRule))
    {
        // Similar to dismiss, create exception appointment with snoozed reminder
        CreateSnoozeException(reminder, snoozeTime);
    }
}
```

## Time Zone Support

### Appointments in Different Time Zones

Create appointments in specific time zones:

```csharp
var appointments = new ObservableCollection<SchedulerAppointment>();

// New York time zone appointment
appointments.Add(new SchedulerAppointment
{
    Subject = "New York Meeting",
    StartTime = DateTime.Now,
    EndTime = DateTime.Now.AddHours(1),
    StartTimeZone = TimeZoneInfo.FindSystemTimeZoneById("America/New_York"),
    EndTimeZone = TimeZoneInfo.FindSystemTimeZoneById("America/New_York")
});

// London time zone appointment
appointments.Add(new SchedulerAppointment
{
    Subject = "London Conference",
    StartTime = DateTime.Now.AddDays(1),
    EndTime = DateTime.Now.AddDays(1).AddHours(2),
    StartTimeZone = TimeZoneInfo.FindSystemTimeZoneById("Europe/London"),
    EndTimeZone = TimeZoneInfo.FindSystemTimeZoneById("Europe/London")
});

scheduler.AppointmentsSource = appointments;
```

**Properties:**
- **StartTime/EndTime**: Appointment time (converted based on time zone)
- **ActualStartTime/ActualEndTime**: Actual rendering time in scheduler's time zone

**Note:** 
- All-day appointments: Time zone not applicable (set to 12:00 AM)
- Recurring appointments: Recalculated for new time zone
- Daylight saving time supported

### Scheduler Time Zone

Set scheduler's time zone to display all appointments in specific zone:

```xml
<scheduler:SfScheduler x:Name="scheduler" 
                       TimeZone="{Binding SchedulerTimeZone}">
</scheduler:SfScheduler>
```

```csharp
// Set scheduler to display in Tokyo time
scheduler.TimeZone = TimeZoneInfo.FindSystemTimeZoneById("Asia/Tokyo");

// All appointments will be displayed in Tokyo time
```

### Client vs Scheduler Time Zone

**Scenario 1: Client's local time zone (default)**
- Scheduler time zone: Not set (uses device local time)
- Appointment time zone: Set to specific zone
- Result: Appointments displayed in device's local time relative to appointment's time zone

**Scenario 2: Specific scheduler time zone**
- Scheduler time zone: Set to specific zone
- Appointment time zone: Set or null
- Result: Appointments displayed in scheduler's time zone

## Complete Examples

### Example 1: Load On Demand with Events

```csharp
public class SchedulerPage : ContentPage
{
    private SfScheduler scheduler;
    
    public SchedulerPage()
    {
        scheduler = new SfScheduler
        {
            View = SchedulerView.Week,
            ShowBusyIndicator = false
        };
        
        scheduler.QueryAppointments += OnQueryAppointments;
        scheduler.ViewChanged += OnViewChanged;
        scheduler.Tapped += OnTapped;
        
        Content = scheduler;
    }
    
    private async void OnQueryAppointments(object sender, SchedulerQueryAppointmentsEventArgs e)
    {
        scheduler.ShowBusyIndicator = true;
        
        // Fetch from database/API
        var appointments = await FetchAppointmentsFromDatabaseAsync(
            e.VisibleDates.First(), 
            e.VisibleDates.Last()
        );
        
        scheduler.AppointmentsSource = appointments;
        scheduler.ShowBusyIndicator = false;
    }
    
    private void OnViewChanged(object sender, SchedulerViewChangedEventArgs e)
    {
        Debug.WriteLine($"View changed from {e.OldView} to {e.NewView}");
    }
    
    private void OnTapped(object sender, SchedulerTappedEventArgs e)
    {
        if (e.Appointments != null && e.Appointments.Count > 0)
        {
            ShowAppointmentDetails(e.Appointments[0]);
        }
    }
}
```

### Example 2: Reminder System

```csharp
public class ReminderScheduler : ContentPage
{
    public ReminderScheduler()
    {
        var scheduler = new SfScheduler
        {
            View = SchedulerView.Week,
            EnableReminder = true
        };
        
        scheduler.ReminderAlertOpening += OnReminderAlert;
        
        // Add appointments with reminders
        var appointments = new ObservableCollection<SchedulerAppointment>
        {
            new SchedulerAppointment
            {
                Subject = "Team Standup",
                StartTime = DateTime.Now.AddMinutes(10),
                EndTime = DateTime.Now.AddMinutes(25),
                Reminders = new ObservableCollection<SchedulerReminder>
                {
                    new SchedulerReminder { TimeBeforeStart = TimeSpan.FromMinutes(5) }
                }
            }
        };
        
        scheduler.AppointmentsSource = appointments;
        Content = scheduler;
    }
    
    private async void OnReminderAlert(object sender, ReminderAlertOpeningEventArgs e)
    {
        foreach (var reminder in e.Reminders)
        {
            bool snooze = await DisplayAlert(
                "Reminder",
                $"{reminder.Appointment.Subject}\n" +
                $"Starting at {reminder.Appointment.StartTime:t}",
                "Snooze (5 min)",
                "Dismiss"
            );
            
            if (snooze)
            {
                SnoozeReminder(reminder, TimeSpan.FromMinutes(5));
            }
            else
            {
                reminder.IsDismissed = true;
            }
        }
    }
}
```

### Example 3: Multi-Time Zone Scheduler

```csharp
public class GlobalScheduler : ContentPage
{
    public GlobalScheduler()
    {
        var scheduler = new SfScheduler
        {
            View = SchedulerView.Week,
            TimeZone = TimeZoneInfo.FindSystemTimeZoneById("America/New_York")
        };
        
        var appointments = new ObservableCollection<SchedulerAppointment>
        {
            // NY office meeting
            new SchedulerAppointment
            {
                Subject = "NY Office Meeting",
                StartTime = new DateTime(2026, 4, 15, 9, 0, 0),
                EndTime = new DateTime(2026, 4, 15, 10, 0, 0),
                StartTimeZone = TimeZoneInfo.FindSystemTimeZoneById("America/New_York"),
                EndTimeZone = TimeZoneInfo.FindSystemTimeZoneById("America/New_York"),
                Background = Colors.Blue
            },
            // London office meeting
            new SchedulerAppointment
            {
                Subject = "London Office Meeting",
                StartTime = new DateTime(2026, 4, 15, 14, 0, 0),
                EndTime = new DateTime(2026, 4, 15, 15, 0, 0),
                StartTimeZone = TimeZoneInfo.FindSystemTimeZoneById("Europe/London"),
                EndTimeZone = TimeZoneInfo.FindSystemTimeZoneById("Europe/London"),
                Background = Colors.Green
            },
            // Tokyo office meeting
            new SchedulerAppointment
            {
                Subject = "Tokyo Office Meeting",
                StartTime = new DateTime(2026, 4, 16, 10, 0, 0),
                EndTime = new DateTime(2026, 4, 16, 11, 0, 0),
                StartTimeZone = TimeZoneInfo.FindSystemTimeZoneById("Asia/Tokyo"),
                EndTimeZone = TimeZoneInfo.FindSystemTimeZoneById("Asia/Tokyo"),
                Background = Colors.Red
            }
        };
        
        scheduler.AppointmentsSource = appointments;
        Content = scheduler;
    }
}
```

## Troubleshooting

### Event Issues

**Problem:** Events not firing  
**Solution:** Verify event handlers are attached before scheduler is loaded

**Problem:** Multiple event fires  
**Solution:** Check for duplicate event subscriptions; unsubscribe before re-subscribing

**Problem:** Tapped vs SelectionChanged order  
**Solution:** Remember `Tapped` fires before `SelectionChanged`; handle accordingly

### Load On Demand Issues

**Problem:** QueryAppointments not triggering  
**Solution:** 
- Verify `ViewChanged` is occurring
- Check if appointments already loaded for visible dates
- Ensure not modifying same visible date range

**Problem:** Busy indicator not showing  
**Solution:** 
- Set `ShowBusyIndicator = true` before async operation
- Set `ShowBusyIndicator = false` after operation completes
- Ensure UI thread updates

**Problem:** Performance degradation  
**Solution:**
- Reset `AppointmentsSource` for non-Agenda views (don't accumulate)
- Use `ObservableCollection` for automatic UI updates
- Limit appointment count per query

### Reminder Issues

**Problem:** Reminders not triggering  
**Solution:** 
- Verify `EnableReminder="true"`
- Check `TimeBeforeStart` is positive
- Ensure appointment time is in future
- Verify `ReminderAlertOpening` event is subscribed

**Problem:** Recurring appointment reminders incorrect  
**Solution:** Create exception appointments for dismissed/snoozed occurrences

**Problem:** Snooze not working  
**Solution:** Recalculate `TimeBeforeStart` based on current time and appointment type (future/overdue/all-day)

### Time Zone Issues

**Problem:** Appointments showing wrong time  
**Solution:**
- Verify `StartTimeZone`/`EndTimeZone` are set correctly
- Check scheduler `TimeZone` property
- Use `ActualStartTime`/`ActualEndTime` for rendering time

**Problem:** Time zone not found  
**Solution:** Use correct time zone IDs from the supported list (e.g., "America/New_York", not "EST")

**Problem:** Daylight saving time issues  
**Solution:** Framework handles DST automatically; ensure time zone IDs are correct

---

**Related References:**
- [Getting Started](getting-started.md) - Initial setup
- [Appointments](appointments.md) - Creating appointments
- [Appointment Interactions](appointment-interactions.md) - Drag, resize, editor
- [Resources and Calendars](resources-calendars.md) - Resource management
