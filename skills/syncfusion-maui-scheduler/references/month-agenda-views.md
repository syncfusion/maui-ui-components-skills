# Month and Agenda Views in .NET MAUI Scheduler

## Table of Contents
- [Month View](#month-view)
  - [Overview](#overview)
  - [Month Appointment Display Mode](#month-appointment-display-mode)
  - [Adjust Appointment Height in Month View](#adjust-appointment-height-in-month-view)
  - [Customize Day Text Format in Month View](#customize-day-text-format-in-month-view)
  - [Month Agenda View](#month-agenda-view)
  - [Inline Appointments in Month View](#inline-appointments-in-month-view)
  - [Number of Weeks](#number-of-weeks)
  - [Non Working Days](#non-working-days)
  - [View Header Customization](#view-header-customization)
  - [Cell Appearance](#cell-appearance)
  - [Date Text Positioning](#date-text-positioning)
  - [Month Cell Template](#month-cell-template)
- [Agenda View](#agenda-view)
  - [Overview](#agenda-view-overview)
  - [Agenda View Headers](#agenda-view-headers)
    - [Month Header](#month-header)
    - [Week Header](#week-header)
    - [Day Header](#day-header)
  - [Agenda View Layout Mode (Mobile or Desktop)](#agenda-view-layout-mode-mobile-or-desktop)
  - [Show or Hide Empty Days](#show-or-hide-empty-days)
  - [Empty Day (Week Header) Template](#empty-day-week-header-template)
  - [Day Header Template](#day-header-template)
  - [Month Header Template](#month-header-template)
  - [Appointment Time Format](#appointment-time-format)
  - [No Events Text Style](#no-events-text-style)
  - [Agenda Item Template](#agenda-item-template)
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
- `Indicator`: Show colored indicators (dots)
- `Text`: Show appointments as rectangles with subject text
- `None`: Hide appointments

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Month">
    <scheduler:SfScheduler.MonthView>
        <scheduler:SchedulerMonthView AppointmentDisplayMode="Text" />
    </scheduler:SfScheduler.MonthView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Month;
scheduler.MonthView.AppointmentDisplayMode = SchedulerMonthAppointmentDisplayMode.Text;
this.Content = scheduler;
```

**Default:** Text

**Display Mode Details:**

**Text Mode (default):**
- Shows appointment rectangles with subject text
- More detailed information
- Better for sparse schedules
- Supports `AppointmentTemplate` for custom rendering
- Number of appointments rendered per day is bounded by `AppointmentIndicatorCount`

**Indicator Mode:**
- Shows small colored dots
- Maximum `AppointmentIndicatorCount` dots per day (default `5`)
- Space-efficient
- Good for dense schedules

**None Mode:**
- Hides all appointments
- Shows only the calendar grid
- Useful for custom implementations

### Adjust Appointment Height in Month View

Use `AppointmentHeight` on `SchedulerMonthView` to control the rendered height (in pixels) of each appointment rendered as a text rectangle inside a Month cell. The default value of `-1` lets the scheduler compute the height automatically based on the available cell area.

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Month">
    <scheduler:SfScheduler.MonthView>
        <scheduler:SchedulerMonthView AppointmentDisplayMode="Text" AppointmentHeight="36" />
    </scheduler:SfScheduler.MonthView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Month;
scheduler.MonthView.AppointmentDisplayMode = SchedulerMonthAppointmentDisplayMode.Text;
scheduler.MonthView.AppointmentHeight = 36; // pixels
this.Content = scheduler;
```

**Default:** `-1` (auto)

**Notes:**
- Only applies when `AppointmentDisplayMode` is `Text`.
- Use a higher value for richer appointment rendering, or a smaller value to fit more appointments per cell.

### Customize Day Text Format in Month View

Use `DayStringFormat` on `SchedulerMonthView` to format the day number text that appears inside each Month cell. The default value is an empty string (the scheduler uses its built-in default day number format).

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Month">
    <scheduler:SfScheduler.MonthView>
        <scheduler:SchedulerMonthView DayStringFormat="d" />
    </scheduler:SfScheduler.MonthView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Month;
scheduler.MonthView.DayStringFormat = "d";
this.Content = scheduler;
```

**Common formats:**
- `"d"`: day number without leading zero (1, 2, … 31)
- `"dd"`: day number with leading zero (01, 02, … 31)

### Month Agenda View

Display an agenda pane inline beneath the month grid so the user can see the appointments of a specific date while still seeing the full month.

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

**Customize agenda pane height:**

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Month">
    <scheduler:SfScheduler.MonthView>
        <scheduler:SchedulerMonthView ShowAgendaView="True" AgendaViewHeight="200" />
    </scheduler:SfScheduler.MonthView>
</scheduler:SfScheduler>
```

**Customize agenda pane style:**

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Month">
    <scheduler:SfScheduler.MonthView>
        <scheduler:SchedulerMonthView ShowAgendaView="True">
            <scheduler:SchedulerMonthView.AgendaViewStyle>
                <scheduler:MonthAgendaViewStyle Background="LightYellow" />
            </scheduler:SchedulerMonthView.AgendaViewStyle>
        </scheduler:SchedulerMonthView>
    </scheduler:SfScheduler.MonthView>
</scheduler:SfScheduler>
```

```csharp
this.Scheduler.MonthView.AgendaViewStyle = new MonthAgendaViewStyle
{
    Background = Colors.LightYellow
};
```

**Behavior:**
- The agenda pane shows the appointments of the currently tapped month date.
- The pane uses the same agenda layout as the standalone Agenda view.
- Configure `AgendaViewHeight` to set a fixed height or leave at `-1` for auto-height.
- Configure `AgendaViewStyle` to change background, text style, and other visuals.

### Inline Appointments in Month View

Display appointments inline in Month view by setting `ShowAppointmentsInline` to `true`. Tapping a date cell expands a scrollable list of that day’s appointments beneath the tapped row.

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Month">
    <scheduler:SfScheduler.MonthView>
        <scheduler:SchedulerMonthView ShowAppointmentsInline="True" />
    </scheduler:SfScheduler.MonthView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Month;
scheduler.MonthView.ShowAppointmentsInline = true;
this.Content = scheduler;
```

#### Appointment time format in inline view

Use `TimeTextFormat` in `MonthInlineViewStyle` to format the inline appointment time text. The default format is `hh:mm tt`.

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Month">
    <scheduler:SfScheduler.MonthView>
        <scheduler:SchedulerMonthView ShowAppointmentsInline="True">
            <scheduler:SchedulerMonthView.MonthInlineViewStyle>
                <scheduler:MonthInlineViewStyle TimeTextFormat="HH:mm" />
            </scheduler:SchedulerMonthView.MonthInlineViewStyle>
        </scheduler:SchedulerMonthView>
    </scheduler:SfScheduler.MonthView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Month;
scheduler.MonthView.ShowAppointmentsInline = true;
scheduler.MonthView.MonthInlineViewStyle = new MonthInlineViewStyle()
{
    TimeTextFormat = "HH:mm"
};
```

#### Appointment height in inline view

Use `ItemHeight` in `MonthInlineViewStyle` to set the height of each inline appointment item. The default value is `50`.

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Month">
    <scheduler:SfScheduler.MonthView>
        <scheduler:SchedulerMonthView ShowAppointmentsInline="True">
            <scheduler:SchedulerMonthView.MonthInlineViewStyle>
                <scheduler:MonthInlineViewStyle ItemHeight="70" />
            </scheduler:SchedulerMonthView.MonthInlineViewStyle>
        </scheduler:SchedulerMonthView>
    </scheduler:SfScheduler.MonthView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Month;
scheduler.MonthView.ShowAppointmentsInline = true;
scheduler.MonthView.MonthInlineViewStyle = new MonthInlineViewStyle()
{
    ItemHeight = 70
};
```

#### Inline appointments appearance

##### Customize inline appointments appearance using TextStyle

Use `MonthInlineViewStyle` to customize the inline view background, text style, and item layout.

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Month">
    <scheduler:SfScheduler.MonthView>
        <scheduler:SchedulerMonthView ShowAppointmentsInline="True">
            <scheduler:SchedulerMonthView.MonthInlineViewStyle>
                <scheduler:MonthInlineViewStyle Background="Yellow">
                    <scheduler:MonthInlineViewStyle.TextStyle>
                        <scheduler:SchedulerTextStyle TextColor="White" FontSize="14" />
                    </scheduler:MonthInlineViewStyle.TextStyle>
                </scheduler:MonthInlineViewStyle>
            </scheduler:SchedulerMonthView.MonthInlineViewStyle>
        </scheduler:SchedulerMonthView>
    </scheduler:SfScheduler.MonthView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Month;
scheduler.MonthView.ShowAppointmentsInline = true;
scheduler.MonthView.MonthInlineViewStyle = new MonthInlineViewStyle()
{
    Background = Colors.Yellow,
    TextStyle = new SchedulerTextStyle()
    {
        TextColor = Colors.White,
        FontSize = 14,
    }
};
```

##### Customize inline appointments appearance using DateTemplate

Use `MonthInlineViewItemTemplate` to provide a custom `DataTemplate` for each inline appointment item. The binding context is the `SchedulerAppointment` instance.

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Month">
    <scheduler:SfScheduler.MonthView>
        <scheduler:SchedulerMonthView ShowAppointmentsInline="True">
            <scheduler:SchedulerMonthView.MonthInlineViewItemTemplate>
                <DataTemplate>
                    <Grid BackgroundColor="MediumOrchid" Padding="8">
                        <HorizontalStackLayout HorizontalOptions="Center" VerticalOptions="Center" Spacing="6">
                            <Label Text="&#xE71D;"
                                   FontFamily="MauiMaterialAssets"
                                   TextColor="White"
                                   VerticalOptions="Center" />
                            <Label Text="{Binding Subject}"
                                   TextColor="White"
                                   VerticalOptions="Center" />
                        </HorizontalStackLayout>
                    </Grid>
                </DataTemplate>
            </scheduler:SchedulerMonthView.MonthInlineViewItemTemplate>
        </scheduler:SchedulerMonthView>
    </scheduler:SfScheduler.MonthView>
</scheduler:SfScheduler>
```

#### MonthInlineAppointmentTapped

Use the `MonthInlineAppointmentTapped` event to respond when the user taps an appointment in the inline view. The event args provide the tapped appointment via `Appointment` and the selected date via `SelectedDate`.

```xaml
<scheduler:SfScheduler x:Name="Scheduler"
                       View="Month"
                       MonthInlineAppointmentTapped="Scheduler_MonthInlineAppointmentTapped" />
```

```csharp
this.Scheduler.MonthInlineAppointmentTapped += Scheduler_MonthInlineAppointmentTapped;

private void Scheduler_MonthInlineAppointmentTapped(object sender, MonthInlineAppointmentTappedEventArgs e)
{
    var appointment = e.Appointment;
    var selectedDate = e.SelectedDate;
}
```

**Notes:**
- Inline appointments appear only when there is enough room; six-week layouts keep the inline view inactive.
- Empty dates display a built-in "No Events" message.

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

### Non Working Days

Specify which days of the week are treated as non-working days in month view:

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Month">
    <scheduler:SfScheduler.MonthView>
        <scheduler:SchedulerMonthView NonWorkingDays="Saturday,Sunday" />
    </scheduler:SfScheduler.MonthView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Month;
scheduler.MonthView.NonWorkingDays = SchedulerMonthWeekDays.Saturday | SchedulerMonthWeekDays.Sunday;
this.Content = scheduler;
```

**Default:** `SchedulerMonthWeekDays.None`

**Supported Values:**
- `None`
- `Monday` to `Sunday`
- Combined values such as `Saturday | Sunday`

#### Show or Hide Non Working Days

The `HideNonWorkingDays` property controls whether non-working-day cells are shown. Set it to `false` (default) to keep them visible with non-working styling, or `true` to hide them.

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Month">
    <scheduler:SfScheduler.MonthView>
        <scheduler:SchedulerMonthView NonWorkingDays="Saturday,Sunday" HideNonWorkingDays="True" />
    </scheduler:SfScheduler.MonthView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Month;
scheduler.MonthView.NonWorkingDays = SchedulerMonthWeekDays.Saturday | SchedulerMonthWeekDays.Sunday;
scheduler.MonthView.HideNonWorkingDays = true;
this.Content = scheduler;
```

#### Customize Non Working Day Appearance

Use `SchedulerMonthCellStyle` to visually distinguish non-working days through `MonthView.CellStyle`.

**Relevant Properties:**
- `NonWorkingDaysBackground`: Sets the background for non-working-day cells.
- `NonWorkingDaysTextStyle`: Sets the text style for non-working-day dates.
- `SchedulerMonthCellStyle`: Customizes month-cell visuals such as background, today date, leading/trailing dates, and non-working-day appearance.

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Month;
scheduler.MonthView.NonWorkingDays = SchedulerMonthWeekDays.Saturday | SchedulerMonthWeekDays.Sunday;
scheduler.MonthView.CellStyle = new SchedulerMonthCellStyle
{
    NonWorkingDaysBackground = Brush.LightPink,
    NonWorkingDaysTextStyle = new SchedulerTextStyle
    {
        TextColor = Colors.DarkRed,
        FontSize = 12
    }
};
```

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Month">
    <scheduler:SfScheduler.MonthView>
        <scheduler:SchedulerMonthView NonWorkingDays="Saturday,Sunday">
            <scheduler:SchedulerMonthView.CellStyle>
                <scheduler:SchedulerMonthCellStyle NonWorkingDaysBackground="LightPink">
                    <scheduler:SchedulerMonthCellStyle.NonWorkingDaysTextStyle>
                        <scheduler:SchedulerTextStyle TextColor="DarkRed" FontSize="12" />
                    </scheduler:SchedulerMonthCellStyle.NonWorkingDaysTextStyle>
                </scheduler:SchedulerMonthCellStyle>
            </scheduler:SchedulerMonthView.CellStyle>
        </scheduler:SchedulerMonthView>
    </scheduler:SfScheduler.MonthView>
</scheduler:SfScheduler>
```

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

### Date Text Positioning

Adjust the horizontal alignment of date numbers in month cells for better readability using `DateHorizontalAlignment`.

**Available values:** `Left`, `Center`, `Right`, and `Justified`

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Month">
    <scheduler:SfScheduler.MonthView>
        <scheduler:SchedulerMonthView DateHorizontalAlignment="Left" />
    </scheduler:SfScheduler.MonthView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Month;
scheduler.MonthView.DateHorizontalAlignment = HorizontalAlignment.Left;
this.Content = scheduler;
```

**Default:** `Center`

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

Agenda view displays appointments in a list format, grouped by date. It provides a chronological view of upcoming appointments. The agenda view is configured through `SchedulerAgendaView`, which is exposed as the `AgendaView` property on `SfScheduler`.

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Agenda">
    <scheduler:SfScheduler.AgendaView>
        <scheduler:SchedulerAgendaView />
    </scheduler:SfScheduler.AgendaView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Agenda;
this.Content = scheduler;
```

**Features:**
- List-based appointment display
- Date grouping headers (month, week, day)
- Scrollable interface
- Efficient for viewing many appointments
- Configurable layout mode (mobile or desktop)
- Customizable empty-day and date header templates

### Agenda View Headers

The Agenda view has three configurable headers: **month**, **week**, and **day**. Each header is configured through its own settings class and exposes a `DataTemplate` for full custom UI.

#### Month Header

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Agenda">
    <scheduler:SfScheduler.AgendaView>
        <scheduler:SchedulerAgendaView>
            <scheduler:SchedulerAgendaView.MonthHeaderSettings>
                <scheduler:SchedulerMonthHeaderSettings Background="LightBlue"
                                                         DateFormat="MMMM yyyy"
                                                         Height="40" />
            </scheduler:SchedulerAgendaView.MonthHeaderSettings>
        </scheduler:SchedulerAgendaView>
    </scheduler:SfScheduler.AgendaView>
</scheduler:SfScheduler>
```

```csharp
this.scheduler.View = SchedulerView.Agenda;
this.scheduler.AgendaView.MonthHeaderSettings.Background = Brush.LightBlue;
this.scheduler.AgendaView.MonthHeaderSettings.DateFormat = "MMMM yyyy";
this.scheduler.AgendaView.MonthHeaderSettings.Height = 40;
```

#### Week Header

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Agenda">
    <scheduler:SfScheduler.AgendaView>
        <scheduler:SchedulerAgendaView>
            <scheduler:SchedulerAgendaView.WeekHeaderSettings>
                <scheduler:SchedulerWeekHeaderSettings Background="LightGreen"
                                                       DateFormat="MMM dd"
                                                       Height="40" />
            </scheduler:SchedulerAgendaView.WeekHeaderSettings>
        </scheduler:SchedulerAgendaView>
    </scheduler:SfScheduler.AgendaView>
</scheduler:SfScheduler>
```

```csharp
this.scheduler.View = SchedulerView.Agenda;
this.scheduler.AgendaView.WeekHeaderSettings.Background = Brush.LightGreen;
this.scheduler.AgendaView.WeekHeaderSettings.DateFormat = "MMM dd";
this.scheduler.AgendaView.WeekHeaderSettings.Height = 40;
```

#### Day Header

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Agenda">
    <scheduler:SfScheduler.AgendaView>
        <scheduler:SchedulerAgendaView>
            <scheduler:SchedulerAgendaView.DayHeaderSettings>
                <scheduler:SchedulerDayHeaderSettings Background="LightYellow"
                                                     DayFormat="dddd"
                                                     DateFormat="MMM dd"
                                                     Width="100" />
            </scheduler:SchedulerAgendaView.DayHeaderSettings>
        </scheduler:SchedulerAgendaView>
    </scheduler:SfScheduler.AgendaView>
</scheduler:SfScheduler>
```

```csharp
this.scheduler.View = SchedulerView.Agenda;
this.scheduler.AgendaView.DayHeaderSettings.Background = Brush.LightYellow;
this.scheduler.AgendaView.DayHeaderSettings.DayFormat = "dddd";
this.scheduler.AgendaView.DayHeaderSettings.DateFormat = "MMM dd";
this.scheduler.AgendaView.DayHeaderSettings.Width = 100;
```

### Agenda View Layout Mode (Mobile or Desktop)

Use `AgendaViewLayoutMode` on `SchedulerAgendaView` to force the agenda view into a specific layout. The default value is `Auto`, which lets the scheduler choose the layout based on the available width and the platform.

**Options:**
- `Auto`: Layout is determined automatically based on the available width and the platform.
- `Mobile`: Always use the mobile (single-column, week-grouped) layout.
- `Desktop`: Always use the desktop (multi-column, day-grouped) layout.

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Agenda">
    <scheduler:SfScheduler.AgendaView>
        <scheduler:SchedulerAgendaView LayoutMode="Desktop" />
    </scheduler:SfScheduler.AgendaView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Agenda;
scheduler.AgendaView.LayoutMode = AgendaViewLayoutMode.Desktop;
this.Content = scheduler;
```

**Default:** `Auto`

**Use cases:**
- Force a single-column mobile experience on tablets or desktop window resize.
- Force a multi-column desktop experience on small phones (less common).
- Lock the layout for consistent UX across device orientations.

### Show or Hide Empty Days

Use `HideEmptyDays` to hide dates in Agenda view that do not contain any appointments. Days without appointments are dropped from the visible list (mainly applies to mobile layout).

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Agenda">
    <scheduler:SfScheduler.AgendaView>
        <scheduler:SchedulerAgendaView HideEmptyDays="True" />
    </scheduler:SfScheduler.AgendaView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Agenda;
scheduler.AgendaView.HideEmptyDays = true;
this.Content = scheduler;
```

**Default:** False

**Behavior:**
- Hides empty days/weeks in mobile layout
- Keeps non-empty dates visible
- Desktop layout continues to render with its existing behavior

### Empty Day (Week Header) Template

Replace the built-in empty-day placeholder in the Agenda view with a custom `DataTemplate` using `WeekHeaderTemplate`. The `BindingContext` is an `AgendaViewWeekHeaderDetails` instance that exposes the start and end dates of the week.

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Agenda">
    <scheduler:SfScheduler.AgendaView>
        <scheduler:SchedulerAgendaView>
            <scheduler:SchedulerAgendaView.WeekHeaderTemplate>
                <DataTemplate>
                    <Grid BackgroundColor="LightGray">
                        <Label HorizontalOptions="Center"
                               VerticalOptions="Center"
                               FontSize="14"
                               TextColor="Black">
                            <Label.FormattedText>
                                <FormattedString>
                                    <Span Text="{Binding StartDate, StringFormat='{0:MMM dd}'}" />
                                    <Span Text=" - " />
                                    <Span Text="{Binding EndDate, StringFormat='{0:MMM dd, yyyy}'}" />
                                </FormattedString>
                            </Label>
                        </Grid>
                </DataTemplate>
            </scheduler:SchedulerAgendaView.WeekHeaderTemplate>
        </scheduler:SchedulerAgendaView>
    </scheduler:SfScheduler.AgendaView>
</scheduler:SfScheduler>
```

**BindingContext:** `AgendaViewWeekHeaderDetails` with `StartDate` and `EndDate` properties.

### Day Header Template

Replace the built-in day header in the Agenda view with a custom `DataTemplate` using `DayHeaderTemplate`. The `BindingContext` is the `DateTime` value for the day being rendered.

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Agenda">
    <scheduler:SfScheduler.AgendaView>
        <scheduler:SchedulerAgendaView>
            <scheduler:SchedulerAgendaView.DayHeaderTemplate>
                <DataTemplate>
                    <Grid BackgroundColor="LightGreen">
                        <VerticalStackLayout HorizontalOptions="Center"
                                              VerticalOptions="Center">
                            <Label FontSize="14"
                                   TextColor="Gray"
                                   HorizontalTextAlignment="Center"
                                   Text="{Binding StringFormat='{0:ddd}'}" />
                            <Label FontSize="20"
                                   FontAttributes="Bold"
                                   TextColor="Black"
                                   HorizontalTextAlignment="Center"
                                   Text="{Binding StringFormat='{0:dd}'}" />
                        </VerticalStackLayout>
                    </Grid>
                </DataTemplate>
            </scheduler:SchedulerAgendaView.DayHeaderTemplate>
        </scheduler:SchedulerAgendaView>
    </scheduler:SfScheduler.AgendaView>
</scheduler:SfScheduler>
```

**BindingContext:** `DateTime` for the rendered day.

### Month Header Template

Replace the built-in month header in the Agenda view with a custom `DataTemplate` using `MonthHeaderTemplate`. The `BindingContext` is an `AgendaMonthHeaderTemplate` instance that exposes the month date.

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Agenda">
    <scheduler:SfScheduler.AgendaView>
        <scheduler:SchedulerAgendaView>
            <scheduler:SchedulerAgendaView.MonthHeaderTemplate>
                <DataTemplate>
                    <Grid>
                        <Label x:Name="label"
                               HorizontalOptions="Start"
                               Background="LightBlue"
                               VerticalOptions="Start"
                               TextColor="Black"
                               FontSize="16"
                               Text="{Binding StringFormat='{0:MMMM yyyy}'}" />
                    </Grid>
                </DataTemplate>
            </scheduler:SchedulerAgendaView.MonthHeaderTemplate>
        </scheduler:SchedulerAgendaView>
    </scheduler:SfScheduler.AgendaView>
</scheduler:SfScheduler>
```

### Appointment Time Format

Use `AppointmentTimeFormat` to customize the format string used to render appointment start times inside the agenda view. The default value is `"hh:mm tt"`.

```xaml
<scheduler:SfScheduler x:Name="Scheduler" View="Agenda">
    <scheduler:SfScheduler.AgendaView>
        <scheduler:SchedulerAgendaView AppointmentTimeFormat="HH:mm" />
    </scheduler:SfScheduler.AgendaView>
</scheduler:SfScheduler>
```

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Agenda;
scheduler.AgendaView.AppointmentTimeFormat = "HH:mm";
this.Content = scheduler;
```

**Default:** `"hh:mm tt"`

### No Events Text Style

Use `NoEventsTextStyle` to customize the appearance of the placeholder text shown when an agenda day has no appointments.

```csharp
this.Scheduler.View = SchedulerView.Agenda;
this.Scheduler.AgendaView.NoEventsTextStyle = new SchedulerTextStyle
{
    TextColor = Colors.Red,
    FontSize = 14,
    FontAttributes = FontAttributes.Italic
};
```

### Agenda Item Template

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

**Issue:** Non-working days are not appearing as expected
**Solution:**
- Verify `NonWorkingDays` is set to the correct day values
- Check whether `HideNonWorkingDays` is enabled
- Ensure the month view is using the intended `CellStyle` for non-working-day appearance

### Agenda View Issues

**Issue:** Agenda view empty
**Solution:**
- Verify appointments exist in the displayed date range
- Ensure `DisplayDate` is correct
- Check appointment binding
- If you are hiding empty days, ensure appointments exist on the expected dates

**Issue:** Custom template not rendering
**Solution:**
- Verify `DataTemplate` syntax
- Check binding paths against the `BindingContext` (`DateTime` for day header, `SchedulerAppointment` for appointment, `AgendaViewWeekHeaderDetails` for week header)
- Test with simple template first

**Issue:** Agenda always uses mobile layout on desktop
**Solution:**
- Set `AgendaView.LayoutMode` to `Desktop` or `Auto`

**Issue:** Agenda always uses desktop layout on small phones
**Solution:**
- Set `AgendaView.LayoutMode` to `Mobile` or `Auto`

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
- Use Text mode (default) for sparse schedules
- Set `NumberOfVisibleWeeks` based on screen size
- Use `AppointmentHeight` to balance density vs. detail
- Provide clear visual cues for today's date

**Agenda View:**
- Use `LayoutMode="Auto"` for adaptive UX, or set `Mobile`/`Desktop` to lock the layout
- Use consistent date formatting across month/week/day headers
- Use `DayHeaderTemplate` and `WeekHeaderTemplate` for branded look
- Use `HideEmptyDays` to keep the mobile agenda compact
- Group related appointments visually via `AppointmentTemplate`

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
