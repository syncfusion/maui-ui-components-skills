# Navigation and Date Restrictions in .NET MAUI Scheduler

## Table of Contents

- [Overview](#overview)
- [Date Navigation](#date-navigation)
  - [Programmatic Navigation](#programmatic-navigation)
  - [Forward and Backward Navigation](#forward-and-backward-navigation)
  - [Date Selection](#date-selection)
  - [View Navigation](#view-navigation)
- [Date Picker](#date-picker)
- [View Switching](#view-switching)
  - [Allowed Views](#allowed-views)
  - [Show/Hide View Buttons](#showhide-view-buttons)
- [Date Restrictions](#date-restrictions)
  - [Minimum Date Time](#minimum-date-time)
  - [Maximum Date Time](#maximum-date-time)
  - [Selectable Day Predicate (Blackout Dates)](#selectable-day-predicate-blackout-dates)
  - [Disabled Date Appearance](#disabled-date-appearance)
- [Header Customization](#header-customization)
  - [Header Height](#header-height)
  - [Header Date Format](#header-date-format)
  - [Header Appearance](#header-appearance)
  - [Custom Header Template](#custom-header-template)
- [Complete Examples](#complete-examples)
- [Troubleshooting](#troubleshooting)

## Overview

The .NET MAUI Scheduler provides comprehensive date navigation and restriction capabilities, allowing you to control how users navigate through dates, restrict date ranges, and customize the header appearance. These features work across all scheduler views.

## Date Navigation

### Programmatic Navigation

Navigate to specific dates using the `DisplayDate` property:

```xml
<scheduler:SfScheduler x:Name="scheduler" 
                       View="Week"
                       DisplayDate="{Binding SelectedDisplayDate}">
</scheduler:SfScheduler>
```

```csharp
// Navigate to a specific date
scheduler.DisplayDate = DateTime.Today.AddMonths(-1).AddHours(9);

// Navigate to today
scheduler.DisplayDate = DateTime.Today;

// Navigate to specific date and time
scheduler.DisplayDate = new DateTime(2026, 4, 15, 10, 0, 0);
```

**Note:** When navigating before minimum date or beyond maximum date, the date resets to minimum or maximum respectively.

### Forward and Backward Navigation

#### Forward Method

Navigate to the next immediate date/period:

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition/>
        <RowDefinition Height="50"/>
    </Grid.RowDefinitions>
    
    <scheduler:SfScheduler x:Name="scheduler" View="Week"/>
    
    <Button Text="Forward" 
           Clicked="OnForwardClicked" 
           Grid.Row="1"/>
</Grid>
```

```csharp
private void OnForwardClicked(object sender, EventArgs e)
{
    scheduler.Forward();
}
```

**Behavior by view:**
- **Month**: Next month
- **Week/WorkWeek**: Next week
- **Day**: Next day
- **Timeline views**: Next period

#### Backward Method

Navigate to the previous immediate date/period:

```csharp
private void OnBackwardClicked(object sender, EventArgs e)
{
    scheduler.Backward();
}
```

**Behavior by view:**
- **Month**: Previous month
- **Week/WorkWeek**: Previous week
- **Day**: Previous day
- **Timeline views**: Previous period

### Date Selection

Programmatically select dates using `SelectedDate`:

```xml
<scheduler:SfScheduler x:Name="scheduler" 
                       View="Week"
                       SelectedDate="{Binding SelectedDate}">
</scheduler:SfScheduler>
```

```csharp
// Select today
scheduler.SelectedDate = DateTime.Today.AddHours(9);

// Select specific date
scheduler.SelectedDate = new DateTime(2026, 4, 15, 14, 0, 0);
```

**Note:** Cannot select dates before `MinimumDateTime` or after `MaximumDateTime`.

### View Navigation

Enable quick navigation to Day or Timeline Day view by tapping on month cells or view headers:

```xml
<scheduler:SfScheduler x:Name="scheduler" 
                       View="Month"
                       AllowViewNavigation="true">
</scheduler:SfScheduler>
```

```csharp
scheduler.AllowViewNavigation = true;
```

**Navigation behavior:**
- **Month view**: Tap cell → Navigate to Day view for that date
- **Week/WorkWeek view**: Tap header → Navigate to Day view
- **Timeline Month/Week**: Tap header → Navigate to Timeline Day view

**Not applicable for:** Day and Timeline Day views (already most detailed views)

## Date Picker

Enable date picker in header for quick date selection:

```xml
<scheduler:SfScheduler x:Name="scheduler" 
                       View="Week"
                       ShowDatePickerButton="true">
</scheduler:SfScheduler>
```

```csharp
scheduler.ShowDatePickerButton = true;
```

**Features:**
- Quickly switch between months, years, decades, centuries
- Direct jump to specific date
- Displays in scheduler header

## View Switching

### Allowed Views

Configure which views users can switch between:

```xml
<scheduler:SfScheduler x:Name="scheduler" 
                       View="Week"
                       AllowedViews="Day,Week,WorkWeek,Month,TimelineDay,TimelineWeek,TimelineWorkWeek,TimelineMonth">
</scheduler:SfScheduler>
```

```csharp
scheduler.AllowedViews = SchedulerViews.Day | 
                        SchedulerViews.Week | 
                        SchedulerViews.WorkWeek | 
                        SchedulerViews.Month | 
                        SchedulerViews.TimelineDay | 
                        SchedulerViews.TimelineWeek | 
                        SchedulerViews.TimelineWorkWeek | 
                        SchedulerViews.TimelineMonth;
```

**Available views:**
- Day
- Week
- WorkWeek
- Month
- Agenda
- TimelineDay
- TimelineWeek
- TimelineWorkWeek
- TimelineMonth

**Default:** `SchedulerViews.Default` (no view buttons displayed)

### Show/Hide View Buttons

Control visibility of view switching buttons:

```xml
<scheduler:SfScheduler x:Name="scheduler" 
                       AllowedViews="Day,Week,Month"
                       ShowAllowedViews="false">
</scheduler:SfScheduler>
```

```csharp
scheduler.AllowedViews = SchedulerViews.Day | SchedulerViews.Week | SchedulerViews.Month;
scheduler.ShowAllowedViews = false; // Hide view buttons
```

**Note:** When `ShowAllowedViews="false"`:
- View buttons are hidden in header
- `AllowViewNavigation` still works if configured
- Cleaner interface for scenarios without view switching

## Date Restrictions

### Minimum Date Time

Restrict backward navigation and disable dates before minimum:

```xml
<scheduler:SfScheduler x:Name="scheduler" 
                       View="Month"
                       MinimumDateTime="{Binding MinDate}">
</scheduler:SfScheduler>
```

```csharp
// Restrict to 3 months before today
scheduler.MinimumDateTime = DateTime.Today.AddMonths(-3);

// Restrict to start of year
scheduler.MinimumDateTime = new DateTime(DateTime.Today.Year, 1, 1);
```

**Default:** `DateTime.MinValue`

**Effects:**
- Dates before minimum are disabled
- Cannot swipe/navigate before minimum
- Selection blocked before minimum

### Maximum Date Time

Restrict forward navigation and disable dates after maximum:

```xml
<scheduler:SfScheduler x:Name="scheduler" 
                       View="Month"
                       MaximumDateTime="{Binding MaxDate}">
</scheduler:SfScheduler>
```

```csharp
// Restrict to 3 months after today
scheduler.MaximumDateTime = DateTime.Today.AddMonths(3).AddHours(23);

// Restrict to end of year
scheduler.MaximumDateTime = new DateTime(DateTime.Today.Year, 12, 31, 23, 59, 59);
```

**Default:** `DateTime.MaxValue`

**Effects:**
- Dates after maximum are disabled
- Cannot swipe/navigate beyond maximum
- Selection blocked after maximum

### Selectable Day Predicate (Blackout Dates)

Define custom logic to disable specific dates:

```csharp
// Disable weekends
scheduler.SelectableDayPredicate = (date) =>
{
    if (date.DayOfWeek == DayOfWeek.Sunday || 
        date.DayOfWeek == DayOfWeek.Saturday)
    {
        return false; // Disable
    }
    return true; // Enable
};
```

**Common use cases:**

```csharp
// Disable holidays
scheduler.SelectableDayPredicate = (date) =>
{
    var holidays = new List<DateTime> 
    { 
        new DateTime(2026, 1, 1),  // New Year
        new DateTime(2026, 7, 4),   // Independence Day
        new DateTime(2026, 12, 25)  // Christmas
    };
    return !holidays.Contains(date.Date);
};

// Disable past dates
scheduler.SelectableDayPredicate = (date) =>
{
    return date.Date >= DateTime.Today;
};

// Disable first and last day of each month
scheduler.SelectableDayPredicate = (date) =>
{
    return date.Day != 1 && date.Day != DateTime.DaysInMonth(date.Year, date.Month);
};

// Combine multiple conditions
scheduler.SelectableDayPredicate = (date) =>
{
    // Disable weekends and past dates
    bool isWeekend = date.DayOfWeek == DayOfWeek.Sunday || 
                    date.DayOfWeek == DayOfWeek.Saturday;
    bool isPast = date.Date < DateTime.Today;
    
    return !isWeekend && !isPast;
};
```

### Disabled Date Appearance

Customize appearance of disabled dates:

```xml
<scheduler:SfScheduler x:Name="scheduler"
                       DisabledDateBackground="LightGray">
    <scheduler:SfScheduler.DisabledDateTextStyle>
        <scheduler:SchedulerTextStyle TextColor="Red"
                                     FontSize="12"
                                     FontAttributes="Italic"/>
    </scheduler:SfScheduler.DisabledDateTextStyle>
</scheduler:SfScheduler>
```

```csharp
// Customize disabled date appearance
scheduler.DisabledDateTextStyle = new SchedulerTextStyle()
{
    TextColor = Colors.Red,
    FontSize = 12,
    FontAttributes = FontAttributes.Italic
};

scheduler.DisabledDateBackground = Colors.LightGray;
```

**Applies to:**
- Dates before `MinimumDateTime`
- Dates after `MaximumDateTime`
- Dates disabled by `SelectableDayPredicate`

**Note:** `DisabledDateBackground` not applicable for month cells and view header cells.

## Header Customization

### Header Height

Customize header height:

```xml
<scheduler:SfScheduler x:Name="scheduler" View="Week">
    <scheduler:SfScheduler.HeaderView>
        <scheduler:SchedulerHeaderView Height="100"/>
    </scheduler:SfScheduler.HeaderView>
</scheduler:SfScheduler>
```

```csharp
scheduler.HeaderView.Height = 100;
```

**Default:** 50

### Header Date Format

Customize date format displayed in header:

```xml
<scheduler:SfScheduler x:Name="scheduler" View="Week">
    <scheduler:SfScheduler.HeaderView>
        <scheduler:SchedulerHeaderView TextFormat="MMM yy"/>
    </scheduler:SfScheduler.HeaderView>
</scheduler:SfScheduler>
```

```csharp
scheduler.HeaderView.TextFormat = "MMM yy";         // Jan 26
scheduler.HeaderView.TextFormat = "MMMM yyyy";      // January 2026 (default)
scheduler.HeaderView.TextFormat = "dd MMM yyyy";    // 15 Jan 2026
scheduler.HeaderView.TextFormat = "ddd, MMM dd";    // Mon, Jan 15
```

**Default:** `"MMMM yyyy"`

### Header Appearance

Customize header background and text style:

```xml
<scheduler:SfScheduler x:Name="scheduler" View="Week">
    <scheduler:SfScheduler.HeaderView>
        <scheduler:SchedulerHeaderView Background="LightGreen">
            <scheduler:SchedulerHeaderView.TextStyle>
                <scheduler:SchedulerTextStyle TextColor="DarkBlue"
                                             FontSize="16"
                                             FontAttributes="Bold"
                                             FontFamily="OpenSansSemibold"/>
            </scheduler:SchedulerHeaderView.TextStyle>
        </scheduler:SchedulerHeaderView>
    </scheduler:SfScheduler.HeaderView>
</scheduler:SfScheduler>
```

```csharp
scheduler.HeaderView.Background = Colors.LightGreen;

scheduler.HeaderView.TextStyle = new SchedulerTextStyle()
{
    TextColor = Colors.DarkBlue,
    FontSize = 16,
    FontAttributes = FontAttributes.Bold,
    FontFamily = "OpenSansSemibold"
};
```

### Custom Header Template

Create fully custom header layouts:

```xml
<scheduler:SfScheduler x:Name="scheduler" View="Week">
    <scheduler:SfScheduler.HeaderView>
        <scheduler:SchedulerHeaderView>
            <scheduler:SchedulerHeaderView.HeaderTemplate>
                <DataTemplate>
                    <Grid Background="LightGreen" Padding="10">
                        <!-- Date range display -->
                        <Label HorizontalOptions="Center" 
                              VerticalOptions="Center"
                              TextColor="DarkBlue"
                              FontSize="16"
                              FontAttributes="Bold">
                            <Label.Text>
                                <MultiBinding StringFormat="{}{0:MMM dd, yyyy} - {1:MMM dd, yyyy}">
                                    <Binding Path="StartDate"/>
                                    <Binding Path="EndDate"/>
                                </MultiBinding>
                            </Label.Text>
                        </Label>
                        
                        <!-- Custom text -->
                        <Label HorizontalOptions="Center" 
                              VerticalOptions="End" 
                              Text="{Binding Text}" 
                              TextColor="Red"
                              FontSize="12"/>
                    </Grid>
                </DataTemplate>
            </scheduler:SchedulerHeaderView.HeaderTemplate>
        </scheduler:SchedulerHeaderView>
    </scheduler:SfScheduler.HeaderView>
</scheduler:SfScheduler>
```

**BindingContext:** `SchedulerHeaderDetails`

**Available properties:**
- **StartDate**: First date in current view
- **EndDate**: Last date in current view
- **Text**: Default header text

#### Using DataTemplateSelector

Choose header template dynamically based on conditions:

```xml
<Grid>
    <Grid.Resources>
        <!-- Today template (highlighted) -->
        <DataTemplate x:Key="todayTemplate">
            <Grid Background="LightBlue">
                <Label HorizontalOptions="Center" VerticalOptions="Center">
                    <Label.Text>
                        <MultiBinding StringFormat="{}{0:MMM dd, yyyy} - {1:MMM dd, yyyy}">
                            <Binding Path="StartDate"/>
                            <Binding Path="EndDate"/>
                        </MultiBinding>
                    </Label.Text>
                </Label>
                <Label HorizontalOptions="Center" 
                      VerticalOptions="End" 
                      Text="{Binding Text}" 
                      TextColor="Red"/>
            </Grid>
        </DataTemplate>
        
        <!-- Normal template -->
        <DataTemplate x:Key="normalTemplate">
            <Grid Background="LightGreen">
                <Label HorizontalOptions="Center" VerticalOptions="Center">
                    <Label.Text>
                        <MultiBinding StringFormat="{}{0:MMM dd, yyyy} - {1:MMM dd, yyyy}">
                            <Binding Path="StartDate"/>
                            <Binding Path="EndDate"/>
                        </MultiBinding>
                    </Label.Text>
                </Label>
                <Label HorizontalOptions="Center" 
                      VerticalOptions="End" 
                      Text="{Binding Text}" 
                      TextColor="Orange"/>
            </Grid>
        </DataTemplate>
        
        <local:HeaderTemplateSelector x:Key="headerSelector" 
                                     TodayTemplate="{StaticResource todayTemplate}"  
                                     NormalTemplate="{StaticResource normalTemplate}"/>
    </Grid.Resources>
    
    <scheduler:SfScheduler x:Name="scheduler" View="Week">
        <scheduler:SfScheduler.HeaderView>
            <scheduler:SchedulerHeaderView HeaderTemplate="{StaticResource headerSelector}"/>
        </scheduler:SfScheduler.HeaderView>
    </scheduler:SfScheduler>
</Grid>
```

```csharp
public class HeaderTemplateSelector : DataTemplateSelector
{
    public DataTemplate TodayTemplate { get; set; }
    public DataTemplate NormalTemplate { get; set; }
    
    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        var headerDetails = item as SchedulerHeaderDetails;
        
        if (headerDetails != null)
        {
            // Use TodayTemplate if current date falls within range
            if (headerDetails.StartDate.Date <= DateTime.Today && 
                headerDetails.EndDate.Date >= DateTime.Today)
            {
                return TodayTemplate;
            }
        }
        
        return NormalTemplate;
    }
}
```

**Note:** Data template selectors can impact performance due to view conversion overhead.

## Complete Examples

### Example 1: Business Hours Scheduler with Restrictions

```csharp
public class BusinessScheduler : ContentPage
{
    public BusinessScheduler()
    {
        var scheduler = new SfScheduler
        {
            View = SchedulerView.Week,
            ShowDatePickerButton = true,
            AllowViewNavigation = true
        };
        
        // Restrict to business dates (3 months back to 6 months forward)
        scheduler.MinimumDateTime = DateTime.Today.AddMonths(-3);
        scheduler.MaximumDateTime = DateTime.Today.AddMonths(6);
        
        // Disable weekends
        scheduler.SelectableDayPredicate = (date) =>
        {
            return date.DayOfWeek != DayOfWeek.Saturday && 
                   date.DayOfWeek != DayOfWeek.Sunday;
        };
        
        // Customize disabled date appearance
        scheduler.DisabledDateTextStyle = new SchedulerTextStyle
        {
            TextColor = Colors.Gray,
            FontSize = 12
        };
        scheduler.DisabledDateBackground = Colors.LightGray;
        
        // Customize header
        scheduler.HeaderView.Height = 60;
        scheduler.HeaderView.TextFormat = "MMMM dd, yyyy";
        scheduler.HeaderView.Background = Colors.DarkBlue;
        scheduler.HeaderView.TextStyle = new SchedulerTextStyle
        {
            TextColor = Colors.White,
            FontSize = 18,
            FontAttributes = FontAttributes.Bold
        };
        
        Content = scheduler;
    }
}
```

### Example 2: Healthcare Appointment Scheduler

```xml
<scheduler:SfScheduler x:Name="scheduler" 
                       View="Day"
                       ShowDatePickerButton="true"
                       AllowViewNavigation="true"
                       AllowedViews="Day,Week,Month"
                       DisplayDate="{Binding CurrentDate}"
                       SelectedDate="{Binding SelectedAppointmentDate}">
    
    <!-- Header customization -->
    <scheduler:SfScheduler.HeaderView>
        <scheduler:SchedulerHeaderView Height="70" 
                                      Background="MediumSeaGreen"
                                      TextFormat="dddd, MMMM dd, yyyy">
            <scheduler:SchedulerHeaderView.TextStyle>
                <scheduler:SchedulerTextStyle TextColor="White" 
                                             FontSize="16" 
                                             FontAttributes="Bold"/>
            </scheduler:SchedulerHeaderView.TextStyle>
        </scheduler:SchedulerHeaderView>
    </scheduler:SfScheduler.HeaderView>
</scheduler:SfScheduler>
```

```csharp
// Code-behind restrictions
public partial class HealthcareScheduler : ContentPage
{
    public HealthcareScheduler()
    {
        InitializeComponent();
        
        // Only allow future appointments (no past booking)
        scheduler.MinimumDateTime = DateTime.Today;
        
        // Allow booking up to 3 months in advance
        scheduler.MaximumDateTime = DateTime.Today.AddMonths(3);
        
        // Disable Sundays (clinic closed)
        scheduler.SelectableDayPredicate = (date) =>
        {
            return date.DayOfWeek != DayOfWeek.Sunday;
        };
    }
}
```

## Troubleshooting

### Navigation Issues

**Problem:** Forward/Backward not working  
**Solution:** Verify not at minimum/maximum date limits

**Problem:** Cannot navigate to specific date  
**Solution:** Ensure date is within `MinimumDateTime` and `MaximumDateTime` range

**Problem:** View navigation (AllowViewNavigation) not working  
**Solution:** 
- Not supported for Day and Timeline Day views
- Ensure `AllowViewNavigation="true"`
- Check that view is in `AllowedViews`

### Date Selection Issues

**Problem:** Cannot select certain dates  
**Solution:** Check `SelectableDayPredicate`, `MinimumDateTime`, and `MaximumDateTime` settings

**Problem:** SelectedDate not updating  
**Solution:** Ensure date is selectable (not disabled) and use two-way binding if binding to ViewModel

### View Switching Issues

**Problem:** View buttons not showing  
**Solution:** 
- Set `AllowedViews` to include desired views
- Verify `ShowAllowedViews="true"` (default)
- Check `AllowedViews` is not set to `SchedulerViews.Default`

**Problem:** Cannot switch between certain views  
**Solution:** Add all desired views to `AllowedViews` property

### Date Restriction Issues

**Problem:** Dates not being disabled  
**Solution:** 
- Verify `SelectableDayPredicate` logic returns `false` for dates to disable
- Ensure `MinimumDateTime`/`MaximumDateTime` are set correctly
- Check that dates fall outside the min/max range

**Problem:** DisabledDateBackground not showing  
**Solution:** Property not applicable for month cells and view header cells (by design)

### Header Customization Issues

**Problem:** Header template not rendering  
**Solution:** 
- Verify `BindingContext` is `SchedulerHeaderDetails`
- Check DataTemplate markup is valid
- Ensure property paths (`StartDate`, `EndDate`, `Text`) are correct

**Problem:** Header date format not changing  
**Solution:** Set `TextFormat` property on `SchedulerHeaderView`, not on scheduler itself

---

**Related References:**
- [Getting Started](getting-started.md) - Initial setup
- [Day, Week, and WorkWeek Views](day-week-views.md) - View configuration
- [Month and Agenda Views](month-agenda-views.md) - Month view behavior
- [Advanced Features](advanced-features.md) - Events and callbacks
