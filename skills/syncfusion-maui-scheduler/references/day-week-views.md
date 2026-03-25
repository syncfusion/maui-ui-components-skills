# Day and Week Views in .NET MAUI Scheduler

## Table of Contents
- [Overview](#overview)
- [View Types](#view-types)
- [Number of Visible Days](#number-of-visible-days)
- [Time Interval Configuration](#time-interval-configuration)
- [Time Interval Height](#time-interval-height)
- [Flexible Working Days](#flexible-working-days)
- [Flexible Working Hours](#flexible-working-hours)
- [Special Time Regions](#special-time-regions)
  - [Selection Restriction](#selection-restriction)
  - [Recurring Time Regions](#recurring-time-regions)
  - [Recurrence Exception Dates](#recurrence-exception-dates)
  - [Customize Appearance](#customize-special-time-region-appearance)
- [Full Screen Scheduler](#full-screen-scheduler)
- [Current Time Indicator](#current-time-indicator)
- [Time Ruler Customization](#time-ruler-customization)
- [View Header Customization](#view-header-customization)
- [Minimum Appointment Duration](#minimum-appointment-duration)
- [All-Day Appointment Templates](#all-day-appointment-templates)
- [Troubleshooting](#troubleshooting)

## Overview

The .NET MAUI Scheduler provides Day, Week, and WorkWeek views to display appointments in time slots. These views show appointments arranged vertically in their respective time slots based on their duration.

### View Types

**Day View**: Displays a single day of the Scheduler.
**Week View**: Displays all days of a week (Sunday through Saturday).
**WorkWeek View**: Displays only working days (Monday through Friday by default).

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Week">
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Week;
this.Content = scheduler;
```

## Number of Visible Days

Customize the number of visible days in day, week, and workweek views using the `NumberOfVisibleDays` property.

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Week">
    <scheduler:SfScheduler.DaysView>
        <scheduler:SchedulerDaysView NumberOfVisibleDays="3"/>
    </scheduler:SfScheduler.DaysView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Week;
scheduler.DaysView.NumberOfVisibleDays = 3;
this.Content = scheduler;
```

**Key Properties:**
- `NumberOfVisibleDays`: Number of days to display (default varies by view)
- Valid values: 1 to 7 days

## Time Interval Configuration

### Change Time Interval

Customize the time interval between slots using the `TimeInterval` property.

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Week">
    <scheduler:SfScheduler.DaysView>
        <scheduler:SchedulerDaysView TimeInterval="2:0:0" />
    </scheduler:SfScheduler.DaysView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Week;
scheduler.DaysView.TimeInterval = new TimeSpan(2, 0, 0);
this.Content = scheduler;
```

**Note:** To modify `TimeInterval` in minutes, change `TimeRulerFormat` to "hh:mm".

## Time Interval Height

Customize the height of each time slot cell.

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Week">
    <scheduler:SfScheduler.DaysView>
        <scheduler:SchedulerDaysView TimeIntervalHeight="120"/>
    </scheduler:SfScheduler.DaysView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Week;
scheduler.DaysView.TimeIntervalHeight = 120;
this.Content = scheduler;
```

**Default Value:** 50

## Flexible Working Days

Configure working and non-working days using the `NonWorkingDays` property.

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="WorkWeek">
    <scheduler:SfScheduler.DaysView>
        <scheduler:SchedulerDaysView NonWorkingDays="Monday,Wednesday" />
    </scheduler:SfScheduler.DaysView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.WorkWeek;
scheduler.DaysView.NonWorkingDays = SchedulerWeekDays.Monday | SchedulerWeekDays.Wednesday;
this.Content = scheduler;
```

**Important:**
- `NonWorkingDays` applies only to WorkWeek and Timeline WorkWeek views
- Other views display all days

## Flexible Working Hours

Define the visible time range using `StartHour` and `EndHour` properties.

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Week">
    <scheduler:SfScheduler.DaysView>
        <scheduler:SchedulerDaysView StartHour="9" EndHour="16" />
    </scheduler:SfScheduler.DaysView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Week;
scheduler.DaysView.StartHour = 9;
scheduler.DaysView.EndHour = 16;
this.Content = scheduler;
```

**Default Values:** StartHour = 0, EndHour = 24

**Important Notes:**
- Values represent hours (0-24)
- No need for decimal points if not setting minutes
- Time slots calculated: (EndHour - StartHour) * 60 / TimeInterval

## Special Time Regions

Restrict user interaction and highlight specific time periods using `TimeRegions`.

### Basic Time Region

```csharp
this.Scheduler.View = SchedulerView.Week;
this.Scheduler.DaysView.TimeRegions = this.GetTimeRegion();

private ObservableCollection<SchedulerTimeRegion> GetTimeRegion()
{
    var timeRegions = new ObservableCollection<SchedulerTimeRegion>();
    var timeRegion = new SchedulerTimeRegion()
    {
        StartTime = DateTime.Today.Date.AddHours(13),
        EndTime = DateTime.Today.Date.AddHours(14),
        Text = "Lunch",
        EnablePointerInteraction = false,
    };
    timeRegions.Add(timeRegion);
    return timeRegions;
}
```

### Selection Restriction

Control touch interaction using `EnablePointerInteraction`:

```csharp
var timeRegion = new SchedulerTimeRegion()
{
    StartTime = DateTime.Today.Date.AddHours(13),
    EndTime = DateTime.Today.Date.AddHours(14),
    Text = "Lunch",
    EnablePointerInteraction = false, // Disable selection
};
```

**Note:** This only restricts region interaction, not:
- Programmatic selection
- Appointment interaction in the region
- Appointment rendering

### Recurring Time Regions

Create recurring time regions using `RecurrenceRule`:

```csharp
var timeRegion = new SchedulerTimeRegion()
{
    StartTime = DateTime.Today.Date.AddHours(13),
    EndTime = DateTime.Today.Date.AddHours(14),
    Text = "Lunch",
    EnablePointerInteraction = false,
    RecurrenceRule = "FREQ=DAILY;INTERVAL=1",
};
```

### Recurrence Exception Dates

Exclude specific occurrences using `RecurrenceExceptionDates`:

```csharp
var recurrenceExceptionDates = DateTime.Now.Date.AddDays(3);
var timeRegion = new SchedulerTimeRegion()
{
    StartTime = DateTime.Today.Date.AddHours(13),
    EndTime = DateTime.Today.Date.AddHours(14),
    Text = "Lunch",
    EnablePointerInteraction = false,
    RecurrenceRule = "FREQ=DAILY;INTERVAL=1",
    RecurrenceExceptionDates = new ObservableCollection<DateTime>()
    {
        recurrenceExceptionDates,
    }
};
```

### Customize Special Time Region Appearance

#### Using Style

```csharp
var textStyle = new SchedulerTextStyle()
{
    TextColor = Colors.DarkBlue,
    FontSize = 14,
};

var timeRegion = new SchedulerTimeRegion()
{
    StartTime = DateTime.Today.Date.AddHours(13),
    EndTime = DateTime.Today.Date.AddHours(14),
    Text = "Lunch",
    EnablePointerInteraction = false,
    Background = Brush.Orange,
    TextStyle = textStyle
};
```

#### Using DataTemplate

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Week">
    <scheduler:SfScheduler.DaysView>
        <scheduler:SchedulerDaysView>
            <scheduler:SchedulerDaysView.TimeRegionTemplate>
                <DataTemplate>
                    <Grid Background="MediumPurple">
                        <Label HorizontalOptions="Center" 
                               FontSize="10" 
                               TextColor="Yellow" 
                               VerticalOptions="Center" 
                               Text="{Binding Text}" />
                    </Grid>
                </DataTemplate>
            </scheduler:SchedulerDaysView.TimeRegionTemplate>
        </scheduler:SchedulerDaysView>
    </scheduler:SfScheduler.DaysView>
</scheduler:SfScheduler>
```

#### Using DataTemplateSelector

```xaml
<Grid>
    <Grid.Resources>
        <DataTemplate x:Key="timeRegiontemplate">
            <Grid Background="LightCyan" Opacity="0.5">
                <Label HorizontalOptions="Center" 
                       TextColor="Red" 
                       Text="{Binding Text}" 
                       VerticalOptions="Center" />
            </Grid>
        </DataTemplate>
        <DataTemplate x:Key="timeRegiontemplate1">
            <Grid Background="Lightgreen" Opacity="0.5">
                <Label HorizontalOptions="Center" 
                       TextColor="Orange" 
                       Text="{Binding Text}" 
                       VerticalOptions="Center" />
            </Grid>
        </DataTemplate>
        <local:TimeRegionTemplateSelector x:Key="timeRegionTemplateSelector" 
                                          TimeRegionsTemplate="{StaticResource timeRegiontemplate}" 
                                          TimeRegionsTemplate1="{StaticResource timeRegiontemplate1}" />
    </Grid.Resources>
    <scheduler:SfScheduler x:Name="Scheduler" View="Week">
        <scheduler:SfScheduler.DaysView>
            <scheduler:SchedulerDaysView TimeRegionTemplate="{StaticResource timeRegionTemplateSelector}" />
        </scheduler:SfScheduler.DaysView>
    </scheduler:SfScheduler>
</Grid>
```

```csharp
public class TimeRegionTemplateSelector : DataTemplateSelector
{
    public DataTemplate TimeRegionsTemplate { get; set; }
    public DataTemplate TimeRegionsTemplate1 { get; set; }
    
    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        var timeRegionDetails = item as SchedulerTimeRegion;
        if (timeRegionDetails.EnablePointerInteraction)
            return TimeRegionsTemplate;
        return TimeRegionsTemplate1;
    }
}
```

## Full Screen Scheduler

Auto-fit views to screen height by setting `TimeIntervalHeight` to -1:

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Week">
    <scheduler:SfScheduler.DaysView>
        <scheduler:SchedulerDaysView TimeIntervalHeight="-1"/>
    </scheduler:SfScheduler.DaysView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Week;
scheduler.DaysView.TimeIntervalHeight = -1;
this.Content = scheduler;
```

## Current Time Indicator

### Show/Hide Current Time Indicator

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Week">
    <scheduler:SfScheduler.DaysView>
        <scheduler:SchedulerDaysView ShowCurrentTimeIndicator="False"/>
    </scheduler:SfScheduler.DaysView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Week;
scheduler.DaysView.ShowCurrentTimeIndicator = false;
this.Content = scheduler;
```

**Default:** True

### Customize Current Time Indicator

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Week">
    <scheduler:SfScheduler.DaysView>
        <scheduler:SchedulerDaysView CurrentTimeIndicatorBrush="Blue"/>
    </scheduler:SfScheduler.DaysView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Week;
scheduler.DaysView.CurrentTimeIndicatorBrush = Brush.Blue;
this.Content = scheduler;
```

## Time Ruler Customization

### Change Time Ruler Width

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Week">
    <scheduler:SfScheduler.DaysView>
        <scheduler:SchedulerDaysView TimeRulerWidth="120"/>
    </scheduler:SfScheduler.DaysView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Week;
scheduler.DaysView.TimeRulerWidth = 120;
this.Content = scheduler;
```

### Time Ruler Text Formatting

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Week">
    <scheduler:SfScheduler.DaysView>
        <scheduler:SchedulerDaysView TimeFormat="hh:mm"/>
    </scheduler:SfScheduler.DaysView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Week;
scheduler.DaysView.TimeFormat = "hh:mm";
this.Content = scheduler;
```

**Default Format:** "hh:mm tt"

### Customize Time Ruler Text Style

```csharp
this.Scheduler.View = SchedulerView.Week;
var timeRulerTextStyle = new SchedulerTextStyle()
{
    TextColor = Colors.Red,
    FontSize = 12,
};
this.Scheduler.DaysView.TimeRulerTextStyle = timeRulerTextStyle;
```

## View Header Customization

### View Header Text Formatting

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Week">
    <scheduler:SfScheduler.DaysView>
        <scheduler:SchedulerDaysView>
            <scheduler:SchedulerDaysView.ViewHeaderSettings>
                <scheduler:SchedulerViewHeaderSettings DayFormat="dddd"
                                                       DateFormat="dd" />
            </scheduler:SchedulerDaysView.ViewHeaderSettings>
        </scheduler:SchedulerDaysView>
    </scheduler:SfScheduler.DaysView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Week;
scheduler.DaysView.ViewHeaderSettings.DayFormat = "dddd";
scheduler.DaysView.ViewHeaderSettings.DateFormat = "dd";
this.Content = scheduler;
```

### View Header Height

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Week">
    <scheduler:SfScheduler.DaysView>
        <scheduler:SchedulerDaysView>
            <scheduler:SchedulerDaysView.ViewHeaderSettings>
                <scheduler:SchedulerViewHeaderSettings Height="100" />
            </scheduler:SchedulerDaysView.ViewHeaderSettings>
        </scheduler:SchedulerDaysView>
    </scheduler:SfScheduler.DaysView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Week;
scheduler.DaysView.ViewHeaderSettings.Height = 100;
this.Content = scheduler;
```

### Customize View Header Appearance

#### Using Text Style

```csharp
this.Scheduler.View = SchedulerView.Week;
var dateTextStyle = new SchedulerTextStyle()
{
    TextColor = Colors.Red,
    FontSize = 12,
};
this.Scheduler.DaysView.ViewHeaderSettings.DateTextStyle = dateTextStyle;

var dayTextStyle = new SchedulerTextStyle()
{
    TextColor = Colors.Red,
    FontSize = 12,
};
this.Scheduler.DaysView.ViewHeaderSettings.DayTextStyle = dayTextStyle;
this.Scheduler.DaysView.ViewHeaderSettings.Background = Brush.LightGreen;
```

#### Using DataTemplate

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="WorkWeek">
    <scheduler:SfScheduler.DaysView>
        <scheduler:SchedulerDaysView>
            <scheduler:SchedulerDaysView.ViewHeaderTemplate>
                <DataTemplate>
                    <StackLayout Orientation="Vertical" Background="MediumPurple">
                        <Label HorizontalOptions="Center" 
                               VerticalOptions="Center" 
                               Text="{Binding StringFormat='{0:dd}'}" 
                               FontSize="Small" 
                               FontFamily="Bold" 
                               TextColor="White">
                            <Label.Triggers>
                                <DataTrigger TargetType="Label" 
                                           Binding="{Binding}" 
                                           Value="{x:Static system:DateTime.Today}">
                                    <Setter Property="TextColor" Value="Orange"/>
                                </DataTrigger>
                            </Label.Triggers>
                        </Label>
                        <Label HorizontalOptions="Center" 
                               VerticalOptions="Center" 
                               Text="{Binding StringFormat='{0:ddd}'}" 
                               FontSize="Small" 
                               FontFamily="Bold" 
                               TextColor="White">
                            <Label.Triggers>
                                <DataTrigger TargetType="Label" 
                                           Binding="{Binding}" 
                                           Value="{x:Static system:DateTime.Today}">
                                    <Setter Property="TextColor" Value="Orange"/>
                                </DataTrigger>
                            </Label.Triggers>
                        </Label>
                    </StackLayout>
                </DataTemplate>
            </scheduler:SchedulerDaysView.ViewHeaderTemplate>
        </scheduler:SchedulerDaysView>
    </scheduler:SfScheduler.DaysView>
</scheduler:SfScheduler>
```

**Note:** BindingContext of `ViewHeaderTemplate` is `DateTime`.

## Minimum Appointment Duration

Set minimum display height for appointments:

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Week">
    <scheduler:SfScheduler.DaysView>
        <scheduler:SchedulerDaysView MinimumAppointmentDuration="0:30:0" />
    </scheduler:SfScheduler.DaysView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Week;
scheduler.DaysView.MinimumAppointmentDuration = new TimeSpan(0, 30, 0);
this.Content = scheduler;
```

**Behavior:**
- Applied when appointment duration is less than `MinimumAppointmentDuration`
- Not applied to all-day appointments
- If less than `TimeInterval`, `TimeInterval` value is used

## All-Day Appointment Templates

### All-Day Appointment Template

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Week">
    <scheduler:SfScheduler.DaysView>
        <scheduler:SchedulerDaysView>
            <scheduler:SchedulerDaysView.AllDayAppointmentTemplate>
                <DataTemplate>
                    <Grid Background="MediumPurple">
                        <Label Text="{Binding Subject}" 
                               TextColor="White" 
                               HorizontalOptions="Center" 
                               VerticalOptions="Center" 
                               FontSize="12" 
                               FontFamily="Bold"/>
                    </Grid>
                </DataTemplate>
            </scheduler:SchedulerDaysView.AllDayAppointmentTemplate>
        </scheduler:SchedulerDaysView>
    </scheduler:SfScheduler.DaysView>
</scheduler:SfScheduler>
```

### More Appointments Template

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Week">
    <scheduler:SfScheduler.DaysView>
        <scheduler:SchedulerDaysView>
            <scheduler:SchedulerDaysView.MoreAppointmentsTemplate>
                <DataTemplate>
                    <StackLayout Background="#EAEAEA" 
                               HorizontalOptions="FillAndExpand" 
                               VerticalOptions="FillAndExpand">
                        <Label Text="{Binding}" 
                               TextColor="Black" 
                               HorizontalTextAlignment="Center" 
                               VerticalTextAlignment="Center"/>
                    </StackLayout>
                </DataTemplate>
            </scheduler:SchedulerDaysView.MoreAppointmentsTemplate>
        </scheduler:SchedulerDaysView>
    </scheduler:SfScheduler.DaysView>
</scheduler:SfScheduler>
```

## Troubleshooting

### Common Issues and Solutions

**Issue:** Time slots not displaying correctly
**Solution:** 
- Verify `TimeInterval` results in integer value when dividing total minutes
- Check `StartHour` and `EndHour` values

**Issue:** Current time indicator not visible
**Solution:** 
- Ensure `ShowCurrentTimeIndicator` is set to true
- Verify `CurrentTimeIndicatorBrush` is not transparent

**Issue:** Special time regions not restricting selection
**Solution:** 
- Set `EnablePointerInteraction` to false
- Note that this only restricts touch interaction, not programmatic selection

**Issue:** View header not displaying custom format
**Solution:** 
- Use correct date/time format strings
- Ensure format is compatible with current culture

### Performance Optimization

1. **Large Data Sets:**
   - Use load on demand for appointments
   - Limit visible date range

2. **Custom Templates:**
   - Keep template complexity minimal
   - Avoid heavy computations in template selectors

3. **Time Regions:**
   - Minimize number of overlapping regions
   - Use recurring regions instead of multiple single regions

### Best Practices

1. **Time Configuration:**
   - Set appropriate `StartHour` and `EndHour` for your business hours
   - Use reasonable `TimeInterval` values (15, 30, 60 minutes)

2. **Working Days:**
   - Configure `NonWorkingDays` to match business operations
   - Apply only to WorkWeek view for consistency

3. **Customization:**
   - Use consistent styling across all views
   - Test custom templates on different screen sizes

4. **Special Regions:**
   - Use recurring patterns for regular blocks (lunch, breaks)
   - Add clear text labels to time regions

## Related Topics

- Getting Started with Scheduler
- Appointments in Scheduler
- Timeline Views
- Resource View
- Localization

## Sample Code Repository

View complete samples on GitHub:
- [Scheduler Getting Started](https://github.com/SyncfusionExamples/maui-scheduler-examples/tree/main/GettingStarted)
- [Highlight Working Hours](https://github.com/SyncfusionExamples/maui-scheduler-examples/tree/main/HighlightWorkingHour)
- [Highlight Non-Working Hours](https://github.com/SyncfusionExamples/maui-scheduler-examples/tree/main/HighlightNonWorkingHour)
