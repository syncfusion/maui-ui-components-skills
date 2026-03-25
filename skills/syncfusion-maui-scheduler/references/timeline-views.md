# Timeline Views in .NET MAUI Scheduler

## Table of Contents
- [Overview](#overview)
- [Timeline View Types](#timeline-view-types)
- [Number of Visible Days](#number-of-visible-days)
- [Time Interval Configuration](#time-interval-configuration)
- [Time Interval Width](#time-interval-width)
- [Flexible Working Days](#flexible-working-days)
- [Hide Non-Working Days](#hide-non-working-days-in-timeline-month)
- [Flexible Working Hours](#flexible-working-hours)
- [Special Time Regions](#special-time-regions)
- [Full Screen Scheduler](#full-screen-scheduler)
- [Current Time Indicator](#current-time-indicator)
- [Time Ruler Height](#time-ruler-height)
- [Minimum Appointment Duration](#minimum-appointment-duration)
- [View Header Customization](#view-header-customization)
- [Time Text Formatting](#time-text-formatting)
- [Troubleshooting](#troubleshooting)

## Overview

Timeline views in .NET MAUI Scheduler display appointments horizontally across time slots. These views are ideal for viewing appointments across extended time periods and multiple resources.

## Timeline View Types

**Timeline Day**: Single day displayed horizontally
**Timeline Week**: Full week displayed horizontally  
**Timeline WorkWeek**: Working days displayed horizontally
**Timeline Month**: All days of a month displayed horizontally

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="TimelineWeek">
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.TimelineWeek;
this.Content = scheduler;
```

## Number of Visible Days

Customize visible days in timeline views using `NumberOfVisibleDays`:

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="TimelineWeek">
    <scheduler:SfScheduler.TimelineView>
        <scheduler:SchedulerTimelineView NumberOfVisibleDays="3"/>
    </scheduler:SfScheduler.TimelineView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.TimelineWeek;
scheduler.TimelineView.NumberOfVisibleDays = 3;
this.Content = scheduler;
```

## Time Interval Configuration

Set time intervals between time slots:

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="TimelineWeek">
    <scheduler:SfScheduler.TimelineView>
        <scheduler:SchedulerTimelineView TimeInterval="2:0:0" />
    </scheduler:SfScheduler.TimelineView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.TimelineWeek;
scheduler.TimelineView.TimeInterval = new TimeSpan(2, 0, 0);
this.Content = scheduler;
```

**Note:** To modify `TimeInterval` in minutes, set `TimeRulerFormat` to "hh:mm".

## Time Interval Width

Customize the width of each time slot:

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="TimelineWeek">
    <scheduler:SfScheduler.TimelineView>
        <scheduler:SchedulerTimelineView TimeIntervalWidth="120" />
    </scheduler:SfScheduler.TimelineView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.TimelineWeek;
scheduler.TimelineView.TimeIntervalWidth = 120;
this.Content = scheduler;
```

## Flexible Working Days

Configure non-working days for timeline views:

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="TimelineWorkWeek">
    <scheduler:SfScheduler.TimelineView>
        <scheduler:SchedulerTimelineView NonWorkingDays="Monday,Wednesday" />
    </scheduler:SfScheduler.TimelineView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.TimelineWeek;
scheduler.TimelineView.NonWorkingDays = SchedulerWeekDays.Monday | SchedulerWeekDays.Wednesday;
this.Content = scheduler;
```

**Note:** Timeline WorkWeek view displays only working days.

## Hide Non-Working Days in Timeline Month

Control visibility of non-working days in Timeline Month view:

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="TimelineMonth">
    <scheduler:SfScheduler.TimelineView>
        <scheduler:SchedulerTimelineView NonWorkingDays="Monday,Wednesday" 
                                        HideNonWorkingDays="True"/>
    </scheduler:SfScheduler.TimelineView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.TimelineMonth;
scheduler.TimelineView.NonWorkingDays = SchedulerWeekDays.Monday | SchedulerWeekDays.Wednesday;
scheduler.TimelineView.HideNonWorkingDays = true;
this.Content = scheduler;
```

**Default:** False
**Applies to:** TimelineMonth view only

## Flexible Working Hours

Define visible time range:

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="TimelineWeek">
    <scheduler:SfScheduler.TimelineView>
        <scheduler:SchedulerTimelineView StartHour="9" EndHour="16" />
    </scheduler:SfScheduler.TimelineView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.TimelineWeek;
scheduler.TimelineView.StartHour = 9;
scheduler.TimelineView.EndHour = 16;
this.Content = scheduler;
```

**Default Values:** StartHour = 0, EndHour = 24

**Important Notes:**
- Values represent hours (0-24)
- Time slots = (EndHour - StartHour) * 60 / TimeInterval
- Total minutes % TimeInterval must = 0

## Special Time Regions

Highlight and restrict specific time periods:

### Basic Time Region

```csharp
this.Scheduler.View = SchedulerView.TimelineWeek;
this.Scheduler.TimelineView.TimeRegions = this.GetTimeRegion();

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

```csharp
var timeRegion = new SchedulerTimeRegion()
{
    StartTime = DateTime.Today.Date.AddHours(13),
    EndTime = DateTime.Today.Date.AddHours(14),
    Text = "Lunch",
    EnablePointerInteraction = false, // Disable touch interaction
};
```

**Restrictions:**
- Only restricts touch interaction
- Does not prevent programmatic selection
- Does not restrict appointment rendering

### Recurring Time Regions

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

### Customize Time Region Appearance

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
<scheduler:SfScheduler x:Name="Scheduler" View="TimelineWeek">
    <scheduler:SfScheduler.TimelineView>
        <scheduler:SchedulerTimelineView>
            <scheduler:SchedulerTimelineView.TimeRegionTemplate>
                <DataTemplate>
                    <Grid Background="MediumPurple">
                        <Label HorizontalOptions="Center" 
                               TextColor="White" 
                               VerticalOptions="Center" 
                               Text="{Binding Text}" />
                    </Grid>
                </DataTemplate>
            </scheduler:SchedulerTimelineView.TimeRegionTemplate>
        </scheduler:SchedulerTimelineView>
    </scheduler:SfScheduler.TimelineView>
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
    <scheduler:SfScheduler x:Name="Scheduler" View="TimelineWeek">
        <scheduler:SfScheduler.TimelineView>
            <scheduler:SchedulerTimelineView TimeRegionTemplate="{StaticResource timeRegionTemplateSelector}"/>
        </scheduler:SfScheduler.TimelineView>
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

**Note:** BindingContext is `SchedulerTimeRegion`.

## Full Screen Scheduler

Auto-fit timeline views to screen width:

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="TimelineDay">
    <scheduler:SfScheduler.TimelineView>
        <scheduler:SchedulerTimelineView TimeIntervalWidth="-1" />
    </scheduler:SfScheduler.TimelineView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.TimelineDay;
scheduler.TimelineView.TimeIntervalWidth = -1;
this.Content = scheduler;
```

## Current Time Indicator

### Show/Hide Indicator

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="TimelineWeek">
    <scheduler:SfScheduler.TimelineView>
        <scheduler:SchedulerTimelineView ShowCurrentTimeIndicator="False"/>
    </scheduler:SfScheduler.TimelineView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.TimelineWeek;
scheduler.TimelineView.ShowCurrentTimeIndicator = false;
this.Content = scheduler;
```

**Default:** True

### Customize Indicator Appearance

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="TimelineWeek">
    <scheduler:SfScheduler.TimelineView>
        <scheduler:SchedulerTimelineView CurrentTimeIndicatorBrush="Green" />
    </scheduler:SfScheduler.TimelineView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.TimelineWeek;
scheduler.TimelineView.CurrentTimeIndicatorBrush = Brush.Green;
this.Content = scheduler;
```

## Time Ruler Height

Customize time ruler height:

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="TimelineWeek">
    <scheduler:SfScheduler.TimelineView>
        <scheduler:SchedulerTimelineView TimeRulerHeight="100" />
    </scheduler:SfScheduler.TimelineView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.TimelineWeek;
scheduler.TimelineView.TimeRulerHeight = 100;
this.Content = scheduler;
```

## Minimum Appointment Duration

Set minimum display width for appointments:

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="TimelineWeek">
    <scheduler:SfScheduler.TimelineView>
        <scheduler:SchedulerTimelineView MinimumAppointmentDuration="0:30:0" />
    </scheduler:SfScheduler.TimelineView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.TimelineWeek;
scheduler.TimelineView.MinimumAppointmentDuration = new TimeSpan(0, 30, 0);
this.Content = scheduler;
```

**Behavior:**
- Applied when appointment duration < MinimumAppointmentDuration
- Not applied to all-day appointments
- If greater than TimeInterval, TimeInterval is used

## View Header Customization

### View Header Text Formatting

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="TimelineWeek">
    <scheduler:SfScheduler.TimelineView>
        <scheduler:SchedulerTimelineView>
            <scheduler:SchedulerTimelineView.ViewHeaderSettings>
                <scheduler:SchedulerViewHeaderSettings DayFormat="dddd" 
                                                       DateFormat="MMMM dd"/>
            </scheduler:SchedulerTimelineView.ViewHeaderSettings>
        </scheduler:SchedulerTimelineView>
    </scheduler:SfScheduler.TimelineView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.TimelineWeek;
scheduler.TimelineView.ViewHeaderSettings.DayFormat = "dddd";
scheduler.TimelineView.ViewHeaderSettings.DateFormat = "MMMM dd";
this.Content = scheduler;
```

### View Header Height

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="TimelineWeek">
    <scheduler:SfScheduler.TimelineView>
        <scheduler:SchedulerTimelineView>
            <scheduler:SchedulerTimelineView.ViewHeaderSettings>
                <scheduler:SchedulerViewHeaderSettings Height="100" />
            </scheduler:SchedulerTimelineView.ViewHeaderSettings>
        </scheduler:SchedulerTimelineView>
    </scheduler:SfScheduler.TimelineView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.TimelineWeek;
scheduler.TimelineView.ViewHeaderSettings.Height = 100;
this.Content = scheduler;
```

### Customize View Header Appearance

#### Using Text Style

```csharp
this.Scheduler.View = SchedulerView.TimelineWeek;
var dateTextStyle = new SchedulerTextStyle()
{
    TextColor = Colors.Red,
    FontSize = 12,
};
this.Scheduler.TimelineView.ViewHeaderSettings.DateTextStyle = dateTextStyle;

var dayTextStyle = new SchedulerTextStyle()
{
    TextColor = Colors.Red,
    FontSize = 12,
};
this.Scheduler.TimelineView.ViewHeaderSettings.DayTextStyle = dayTextStyle;
this.Scheduler.TimelineView.ViewHeaderSettings.Background = Brush.LightGreen;
```

#### Using DataTemplate

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="TimelineWeek">
    <scheduler:SfScheduler.TimelineView>
        <scheduler:SchedulerTimelineView>
            <scheduler:SchedulerTimelineView.ViewHeaderTemplate>
                <DataTemplate>
                    <Grid Background="MediumPurple">
                        <Label HorizontalOptions="Start" 
                               VerticalOptions="Center" 
                               TextColor="White">
                            <Label.Text>
                                <MultiBinding StringFormat="{}{0:dd} {1:ddd}">
                                    <Binding />
                                    <Binding />
                                </MultiBinding>
                            </Label.Text>
                            <Label.Triggers>
                                <DataTrigger TargetType="Label" 
                                           Binding="{Binding}" 
                                           Value="{x:Static system:DateTime.Today}">
                                    <Setter Property="TextColor" Value="Orange"/>
                                </DataTrigger>
                            </Label.Triggers>
                        </Label>
                    </Grid>
                </DataTemplate>
            </scheduler:SchedulerTimelineView.ViewHeaderTemplate>
        </scheduler:SchedulerTimelineView>
    </scheduler:SfScheduler.TimelineView>
</scheduler:SfScheduler>
```

**Note:** BindingContext is `DateTime`.

#### Using DataTemplateSelector

```xaml
<Grid>
    <Grid.Resources>
        <DataTemplate x:Key="normalDateTemplate">
            <Grid Background="MediumPurple">
                <Label HorizontalOptions="Start" 
                       VerticalOptions="Center" 
                       TextColor="White">
                    <Label.Text>
                        <MultiBinding StringFormat="{}{0:dd} {1:ddd}">
                            <Binding />
                            <Binding />
                        </MultiBinding>
                    </Label.Text>
                </Label>
            </Grid>
        </DataTemplate>
        <DataTemplate x:Key="todayDateTemplate">
            <Grid Background="MediumPurple">
                <Label HorizontalOptions="Start" 
                       VerticalOptions="Center" 
                       TextColor="Yellow">
                    <Label.Text>
                        <MultiBinding StringFormat="{}{0:dd} {1:ddd}">
                            <Binding />
                            <Binding />
                        </MultiBinding>
                    </Label.Text>
                </Label>
            </Grid>
        </DataTemplate>
        <local:ViewHeaderTemplateSelector x:Key="viewHeaderTemplateSelector" 
                                          TodayDateTemplate="{StaticResource todayDateTemplate}" 
                                          NormalDateTemplate="{StaticResource normalDateTemplate}"/>
    </Grid.Resources>
    <scheduler:SfScheduler x:Name="Scheduler" View="TimelineWeek">
        <scheduler:SfScheduler.TimelineView>
            <scheduler:SchedulerTimelineView ViewHeaderTemplate="{StaticResource viewHeaderTemplateSelector}" />
        </scheduler:SfScheduler.TimelineView>
    </scheduler:SfScheduler>
</Grid>
```

```csharp
public class ViewHeaderTemplateSelector : DataTemplateSelector
{
    public DataTemplate NormalDateTemplate { get; set; }
    public DataTemplate TodayDateTemplate { get; set; }
    
    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        var dateTime = (DateTime)item;
        if (dateTime.Date == DateTime.Today.Date)
            return TodayDateTemplate;
        else
            return NormalDateTemplate;
    }
}
```

## Time Text Formatting

Customize time label format:

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="TimelineWeek">
    <scheduler:SfScheduler.TimelineView>
        <scheduler:SchedulerTimelineView TimeFormat="hh:mm" />
    </scheduler:SfScheduler.TimelineView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.TimelineWeek;
scheduler.TimelineView.TimeFormat = "hh:mm";
this.Content = scheduler;
```

**Default Format:** "hh:mm tt"

### Customize Time Ruler Text Style

```csharp
this.Scheduler.View = SchedulerView.TimelineWeek;
var timeRulerTextStyle = new SchedulerTextStyle()
{
    TextColor = Colors.Red,
    FontSize = 12,
};
this.Scheduler.TimelineView.TimeRulerTextStyle = timeRulerTextStyle;
```

## Troubleshooting

### Common Issues and Solutions

**Issue:** Timeline view not scrolling smoothly
**Solution:**
- Reduce number of visible days
- Optimize appointment templates
- Use reasonable time intervals

**Issue:** Time slots not aligning correctly
**Solution:**
- Verify TimeInterval divides evenly into total minutes
- Check StartHour and EndHour calculations
- Ensure TimeIntervalWidth is appropriate

**Issue:** Current time indicator not visible
**Solution:**
- Set ShowCurrentTimeIndicator to true
- Verify CurrentTimeIndicatorBrush color
- Check that current time is within visible hours

**Issue:** View header displaying wrong dates
**Solution:**
- Verify date format strings
- Check culture settings
- Ensure DisplayDate is set correctly

### Performance Optimization

1. **Large Date Ranges:**
   - Use load on demand
   - Limit NumberOfVisibleDays
   - Implement virtual scrolling

2. **Multiple Resources:**
   - Use VisibleResourceCount
   - Optimize resource templates
   - Minimize complex bindings

3. **Custom Templates:**
   - Keep templates lightweight
   - Avoid nested layouts
   - Use cached images

### Best Practices

1. **Time Configuration:**
   - Set StartHour/EndHour for business hours
   - Use standard time intervals (15, 30, 60 minutes)
   - Ensure TimeInterval aligns with business requirements

2. **View Selection:**
   - Timeline Day: Single day detailed view
   - Timeline Week: Week overview
   - Timeline Month: Month overview
   - Timeline WorkWeek: Business week focus

3. **Customization:**
   - Maintain consistent styling
   - Test on different screen sizes
   - Use data templates judiciously

4. **Special Regions:**
   - Mark recurring blocks (lunch, meetings)
   - Use clear visual indicators
   - Add descriptive text

## Related Topics

- Day and Week Views
- Resource View
- Appointments
- Load on Demand
- Localization

## Sample Code Repository

View complete samples on GitHub:
- [Scheduler Getting Started](https://github.com/SyncfusionExamples/maui-scheduler-examples/tree/main/GettingStarted)
- [Highlight Working Hours](https://github.com/SyncfusionExamples/maui-scheduler-examples/tree/main/HighlightWorkingHour)
- [Highlight Non-Working Hours](https://github.com/SyncfusionExamples/maui-scheduler-examples/tree/main/HighlightNonWorkingHour)
