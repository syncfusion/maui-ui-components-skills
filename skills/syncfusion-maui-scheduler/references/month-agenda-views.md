# Month and Agenda Views in .NET MAUI Scheduler

## Table of Contents
- [Month View](#month-view)
  - [Overview](#overview)
  - [Month Appointment Display Mode](#month-appointment-display-mode)
  - [Number of Weeks](#number-of-weeks)
  - [View Header Customization](#view-header-customization)
  - [Cell Appearance](#cell-appearance)
  - [Month Cell Template](#month-cell-template)
- [Agenda View](#agenda-view)
  - [Overview](#agenda-view-overview)
  - [Agenda View Header](#agenda-view-header)
  - [Date Format](#date-format)
  - [Appearance Customization](#appearance-customization)
- [Troubleshooting](#troubleshooting)

## Month View

### Overview

Month view displays appointments for an entire month. Appointments are arranged within cells, with configurable display modes to optimize space and readability.

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Month">
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Month;
this.Content = scheduler;
```

### Month Appointment Display Mode

Control how appointments appear in month cells:

**Options:**
- `Indicator`: Show colored indicators
- `Appointment`: Show full appointment rectangles
- `None`: Hide appointments

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Month">
    <scheduler:SfScheduler.MonthView>
        <scheduler:SchedulerMonthView AppointmentDisplayMode="Appointment" />
    </scheduler:SfScheduler.MonthView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Month;
scheduler.MonthView.AppointmentDisplayMode = SchedulerMonthAppointmentDisplayMode.Appointment;
this.Content = scheduler;
```

**Default:** Indicator

**Display Mode Details:**

**Indicator Mode:**
- Shows small dots
- Maximum 4 indicators per day
- Space-efficient
- Good for dense schedules

**Appointment Mode:**
- Shows appointment rectangles with text
- More detailed information
- Better for sparse schedules
- Supports appointment templates

**None Mode:**
- Hides all appointments
- Shows only calendar grid
- Useful for custom implementations

### Number of Weeks

Configure visible weeks in month view:

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Month">
    <scheduler:SfScheduler.MonthView>
        <scheduler:SchedulerMonthView NumberOfVisibleWeeks="2" />
    </scheduler:SfScheduler.MonthView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Month;
scheduler.MonthView.NumberOfVisibleWeeks = 2;
this.Content = scheduler;
```

**Default:** 6
**Allowed Values:** 1 to 6

**Week Calculation:**
- If visible weeks < weeks in month: show specified weeks
- If visible weeks ≥ weeks in month: show all weeks
- Week starts on FirstDayOfWeek setting

### View Header Customization

#### View Header Text Formatting

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Month">
    <scheduler:SfScheduler.MonthView>
        <scheduler:SchedulerMonthView>
            <scheduler:SchedulerMonthView.ViewHeaderSettings>
                <scheduler:SchedulerViewHeaderSettings DayFormat="ddd" />
            </scheduler:SchedulerMonthView.ViewHeaderSettings>
        </scheduler:SchedulerMonthView>
    </scheduler:SfScheduler.MonthView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Month;
scheduler.MonthView.ViewHeaderSettings.DayFormat = "ddd";
this.Content = scheduler;
```

**Common Formats:**
- "ddd": Mon, Tue, Wed
- "dddd": Monday, Tuesday, Wednesday
- "dd": Mo, Tu, We

#### View Header Height

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Month">
    <scheduler:SfScheduler.MonthView>
        <scheduler:SchedulerMonthView>
            <scheduler:SchedulerMonthView.ViewHeaderSettings>
                <scheduler:SchedulerViewHeaderSettings Height="100" />
            </scheduler:SchedulerMonthView.ViewHeaderSettings>
        </scheduler:SchedulerMonthView>
    </scheduler:SfScheduler.MonthView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Month;
scheduler.MonthView.ViewHeaderSettings.Height = 100;
this.Content = scheduler;
```

#### View Header Appearance

```csharp
this.Scheduler.View = SchedulerView.Month;
var dayTextStyle = new SchedulerTextStyle()
{
    TextColor = Colors.Red,
    FontSize = 14,
};
this.Scheduler.MonthView.ViewHeaderSettings.DayTextStyle = dayTextStyle;
this.Scheduler.MonthView.ViewHeaderSettings.Background = Brush.LightGray;
```

#### View Header Template

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Month">
    <scheduler:SfScheduler.MonthView>
        <scheduler:SchedulerMonthView>
            <scheduler:SchedulerMonthView.ViewHeaderTemplate>
                <DataTemplate>
                    <Grid Background="LightBlue">
                        <Label Text="{Binding StringFormat='{0:ddd}'}" 
                               HorizontalOptions="Center" 
                               VerticalOptions="Center" 
                               TextColor="DarkBlue" 
                               FontSize="16" 
                               FontAttributes="Bold"/>
                    </Grid>
                </DataTemplate>
            </scheduler:SchedulerMonthView.ViewHeaderTemplate>
        </scheduler:SchedulerMonthView>
    </scheduler:SfScheduler.MonthView>
</scheduler:SfScheduler>
```

**Note:** BindingContext is `DateTime` representing the day of week.

### Cell Appearance

#### Show Trailing and Leading Dates

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Month">
    <scheduler:SfScheduler.MonthView>
        <scheduler:SchedulerMonthView ShowTrailingAndLeadingDates="False" />
    </scheduler:SfScheduler.MonthView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Month;
scheduler.MonthView.ShowTrailingAndLeadingDates = false;
this.Content = scheduler;
```

**Default:** True
**False:** Shows only current month dates

#### Show Agenda View

Enable quick agenda view access:

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Month">
    <scheduler:SfScheduler.MonthView>
        <scheduler:SchedulerMonthView ShowAgendaView="True" />
    </scheduler:SfScheduler.MonthView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Month;
scheduler.MonthView.ShowAgendaView = true;
this.Content = scheduler;
```

**Default:** False
**Behavior:** Agenda view appears at bottom, shows selected date appointments

#### Customize Appearance

```csharp
this.Scheduler.View = SchedulerView.Month;
var todayBackground = new SchedulerMonthCellStyle()
{
    TodayBackground = Brush.LightBlue,
    TodayTextStyle = new SchedulerTextStyle()
    {
        TextColor = Colors.Black,
        FontSize = 14,
    }
};
this.Scheduler.MonthView.CellStyle = todayBackground;
```

**Available Properties:**
- `Background`: Normal cell background
- `TodayBackground`: Today cell background
- `LeadingMonthBackground`: Leading dates background
- `TrailingMonthBackground`: Trailing dates background
- `TextStyle`: Date text style
- `TodayTextStyle`: Today date text style
- `LeadingMonthTextStyle`: Leading dates text style
- `TrailingMonthTextStyle`: Trailing dates text style

### Month Cell Template

Fully customize month cell appearance:

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Month">
    <scheduler:SfScheduler.MonthView>
        <scheduler:SchedulerMonthView>
            <scheduler:SchedulerMonthView.CellTemplate>
                <DataTemplate>
                    <Grid>
                        <Label Text="{Binding DateTime, StringFormat='{0:dd}'}" 
                               HorizontalOptions="Center" 
                               VerticalOptions="Center" 
                               TextColor="Black" 
                               FontSize="16">
                            <Label.Triggers>
                                <DataTrigger TargetType="Label" 
                                           Binding="{Binding DateTime}" 
                                           Value="{x:Static system:DateTime.Today}">
                                    <Setter Property="TextColor" Value="Red"/>
                                    <Setter Property="FontAttributes" Value="Bold"/>
                                </DataTrigger>
                            </Label.Triggers>
                        </Label>
                    </Grid>
                </DataTemplate>
            </scheduler:SchedulerMonthView.CellTemplate>
        </scheduler:SchedulerMonthView>
    </scheduler:SfScheduler.MonthView>
</scheduler:SfScheduler>
```

**BindingContext Properties:**
- `DateTime`: Cell date
- `Appointments`: List of appointments in the cell

**Complex Example with Appointments:**

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Month">
    <scheduler:SfScheduler.MonthView>
        <scheduler:SchedulerMonthView>
            <scheduler:SchedulerMonthView.CellTemplate>
                <DataTemplate>
                    <Grid RowDefinitions="Auto,*">
                        <!-- Date Header -->
                        <Label Grid.Row="0" 
                               Text="{Binding DateTime, StringFormat='{0:dd}'}" 
                               HorizontalOptions="End" 
                               VerticalOptions="Start" 
                               Margin="5"
                               TextColor="Black" 
                               FontSize="14"/>
                        
                        <!-- Appointments -->
                        <CollectionView Grid.Row="1" 
                                      ItemsSource="{Binding Appointments}" 
                                      VerticalOptions="Fill">
                            <CollectionView.ItemTemplate>
                                <DataTemplate>
                                    <Grid Margin="2" 
                                          BackgroundColor="{Binding Background}" 
                                          Padding="4,2">
                                        <Label Text="{Binding Subject}" 
                                               TextColor="White" 
                                               FontSize="10"/>
                                    </Grid>
                                </DataTemplate>
                            </CollectionView.ItemTemplate>
                        </CollectionView>
                    </Grid>
                </DataTemplate>
            </scheduler:SchedulerMonthView.CellTemplate>
        </scheduler:SchedulerMonthView>
    </scheduler:SfScheduler.MonthView>
</scheduler:SfScheduler>
```

## Agenda View

### Agenda View Overview

Agenda view displays appointments in a list format, grouped by date. It provides a chronological view of upcoming appointments.

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Agenda">
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Agenda;
this.Content = scheduler;
```

**Features:**
- List-based appointment display
- Date grouping headers
- Scrollable interface
- Efficient for viewing many appointments

### Agenda View Header

The header shows the currently selected date and allows navigation:

#### Date Picker

Toggle date picker visibility:

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Agenda">
    <scheduler:SfScheduler.AgendaView>
        <scheduler:SchedulerAgendaView ShowDatePicker="False" />
    </scheduler:SfScheduler.AgendaView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Agenda;
scheduler.AgendaView.ShowDatePicker = false;
this.Content = scheduler;
```

**Default:** True

#### Days Count

Set number of days to display:

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Agenda">
    <scheduler:SfScheduler.AgendaView>
        <scheduler:SchedulerAgendaView DaysCount="7" />
    </scheduler:SfScheduler.AgendaView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Agenda;
scheduler.AgendaView.DaysCount = 7;
this.Content = scheduler;
```

**Default:** 30

### Date Format

Customize header date format:

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Agenda">
    <scheduler:SfScheduler.AgendaView>
        <scheduler:SchedulerAgendaView DateFormat="MMMM dd, yyyy" />
    </scheduler:SfScheduler.AgendaView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Agenda;
scheduler.AgendaView.DateFormat = "MMMM dd, yyyy";
this.Content = scheduler;
```

**Common Formats:**
- "MMMM dd": January 15
- "dd MMM yyyy": 15 Jan 2024
- "dddd, MMMM dd": Monday, January 15

### Appearance Customization

#### Agenda View Header Style

```csharp
this.Scheduler.View = SchedulerView.Agenda;
var headerTextStyle = new SchedulerTextStyle()
{
    TextColor = Colors.White,
    FontSize = 16,
};
this.Scheduler.AgendaView.HeaderTextStyle = headerTextStyle;
this.Scheduler.AgendaView.HeaderBackground = Brush.DarkBlue;
```

#### Date Text Style

```csharp
this.Scheduler.View = SchedulerView.Agenda;
var dateTextStyle = new SchedulerTextStyle()
{
    TextColor = Colors.DarkBlue,
    FontSize = 14,
    FontAttributes = FontAttributes.Bold,
};
this.Scheduler.AgendaView.DateTextStyle = dateTextStyle;
```

#### Appointment Text Style

```csharp
this.Scheduler.View = SchedulerView.Agenda;
var appointmentTextStyle = new SchedulerTextStyle()
{
    TextColor = Colors.Black,
    FontSize = 12,
};
this.Scheduler.AgendaView.AppointmentTextStyle = appointmentTextStyle;
```

#### Time Text Style

```csharp
this.Scheduler.View = SchedulerView.Agenda;
var timeTextStyle = new SchedulerTextStyle()
{
    TextColor = Colors.Gray,
    FontSize = 10,
};
this.Scheduler.AgendaView.TimeTextStyle = timeTextStyle;
```

#### Today Background

```csharp
this.Scheduler.View = SchedulerView.Agenda;
this.Scheduler.AgendaView.TodayBackground = Brush.LightYellow;
```

#### Agenda Item Template

Full customization of agenda items:

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Agenda">
    <scheduler:SfScheduler.AgendaView>
        <scheduler:SchedulerAgendaView>
            <scheduler:SchedulerAgendaView.AppointmentTemplate>
                <DataTemplate>
                    <Grid Padding="10" BackgroundColor="{Binding Background}">
                        <Grid.RowDefinitions>
                            <RowDefinition Height="Auto"/>
                            <RowDefinition Height="Auto"/>
                        </Grid.RowDefinitions>
                        
                        <Label Grid.Row="0" 
                               Text="{Binding Subject}" 
                               TextColor="White" 
                               FontSize="14" 
                               FontAttributes="Bold"/>
                        
                        <Label Grid.Row="1" 
                               TextColor="White" 
                               FontSize="12">
                            <Label.Text>
                                <MultiBinding StringFormat="{}{0:hh:mm tt} - {1:hh:mm tt}">
                                    <Binding Path="StartTime"/>
                                    <Binding Path="EndTime"/>
                                </MultiBinding>
                            </Label.Text>
                        </Label>
                    </Grid>
                </DataTemplate>
            </scheduler:SchedulerAgendaView.AppointmentTemplate>
        </scheduler:SchedulerAgendaView>
    </scheduler:SfScheduler.AgendaView>
</scheduler:SfScheduler>
```

**BindingContext:** `SchedulerAppointment` object

**Available Properties:**
- `Subject`: Appointment subject
- `StartTime`: Start date/time
- `EndTime`: End date/time
- `Background`: Appointment color
- `IsAllDay`: All-day flag
- `Location`: Location text
- `Notes`: Notes text
- Custom properties from business objects

## Troubleshooting

### Month View Issues

**Issue:** Appointments not visible in month view
**Solution:**
- Check AppointmentDisplayMode setting
- Verify appointments are within visible date range
- Ensure appointment Background is set
- Check if appointments overlap (max 4 indicators shown)

**Issue:** Trailing/leading dates showing incorrectly
**Solution:**
- Set ShowTrailingAndLeadingDates appropriately
- Verify calendar culture settings
- Check FirstDayOfWeek configuration

**Issue:** Month view showing too many weeks
**Solution:**
- Set NumberOfVisibleWeeks to desired value (1-6)
- Ensure property is set before DisplayDate
- Check month has requested weeks

### Agenda View Issues

**Issue:** Agenda view empty
**Solution:**
- Check DaysCount property
- Verify appointments exist in date range
- Ensure DisplayDate is correct
- Check appointment binding

**Issue:** Date picker not working
**Solution:**
- Set ShowDatePicker to true
- Verify touch input is enabled
- Check z-index and overlapping elements

**Issue:** Custom template not rendering
**Solution:**
- Verify DataTemplate syntax
- Check binding paths
- Ensure BindingContext is correct
- Test with simple template first

### Performance Optimization

**Month View:**
1. Use Indicator mode for dense schedules
2. Limit NumberOfVisibleWeeks if possible
3. Avoid complex cell templates
4. Optimize appointment data binding

**Agenda View:**
1. Limit DaysCount to reasonable number
2. Keep AppointmentTemplate simple
3. Use data virtualization for large lists
4. Avoid heavy operations in templates

### Best Practices

**Month View:**
- Use Indicator mode for 10+ appointments per day
- Use Appointment mode for sparse schedules
- Set NumberOfVisibleWeeks based on screen size
- Provide clear visual cues for today's date

**Agenda View:**
- Set DaysCount based on use case (7 for weekly, 30 for monthly)
- Use consistent date formatting
- Provide clear time display
- Group related appointments visually

**General:**
- Test on different screen sizes
- Maintain consistent styling across views
- Use appropriate date formats for locale
- Handle empty state appropriately

## Related Topics

- Day and Week Views
- Timeline Views
- Appointments
- Localization
- Date Navigation

## Sample Code Repository

View complete samples on GitHub:
- [Month View Samples](https://github.com/SyncfusionExamples/maui-scheduler-examples)
- [Agenda View Samples](https://github.com/SyncfusionExamples/maui-scheduler-examples)
- [Custom Cell Templates](https://github.com/SyncfusionExamples/maui-scheduler-examples)
