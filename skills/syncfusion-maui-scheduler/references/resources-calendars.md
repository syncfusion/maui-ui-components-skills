# Resources and Calendar Types in .NET MAUI Scheduler

## Table of Contents

- [Overview](#overview)
- [Resources](#resources)
  - [Creating Resources](#creating-resources)
  - [Assigning Resources to Appointments](#assigning-resources-to-appointments)
  - [Multiple Resource Sharing](#multiple-resource-sharing)
  - [Resource Grouping](#resource-grouping)
  - [Business Object Binding](#business-object-binding)
- [Resource View Configuration](#resource-view-configuration)
  - [Visible Resource Count](#visible-resource-count)
  - [Resource Header Dimensions](#resource-header-dimensions)
  - [Minimum Row Height](#minimum-row-height)
- [Resource Appearance Customization](#resource-appearance-customization)
  - [Text Style](#text-style)
  - [Header Template](#header-template)
  - [Mobile-Specific Customization](#mobile-specific-customization)
- [Special Time Regions for Resources](#special-time-regions-for-resources)
- [Calendar Types](#calendar-types)
  - [Supported Calendars](#supported-calendars)
  - [Setting Calendar Type](#setting-calendar-type)
  - [DateTime Values in Calendar Types](#datetime-values-in-calendar-types)
- [Complete Examples](#complete-examples)
- [Troubleshooting](#troubleshooting)

## Overview

The .NET MAUI Scheduler provides powerful resource management capabilities, allowing you to group appointments by resources such as employees, meeting rooms, equipment, or any other categorizable entity. Additionally, the scheduler supports multiple calendar systems including Gregorian, Hijri, Hebrew, and others for globalization scenarios.

## Resources

### Creating Resources

Create resources using the built-in `SchedulerResource` class with `Name`, `Id`, `Background`, and `Foreground` properties:

```xml
<scheduler:SfScheduler x:Name="scheduler" View="TimelineWeek">
    <scheduler:SfScheduler.ResourceView>
        <scheduler:SchedulerResourceView Resources="{Binding Resources}"/>
    </scheduler:SfScheduler.ResourceView>
</scheduler:SfScheduler>
```

```csharp
// Create resources collection
var resources = new ObservableCollection<SchedulerResource>()
{
    new SchedulerResource() 
    { 
        Name = "Sophia", 
        Foreground = Colors.Blue, 
        Background = Colors.Green, 
        Id = "1000" 
    },
    new SchedulerResource() 
    { 
        Name = "Zoey Addison",  
        Foreground = Colors.Blue, 
        Background = Colors.Green, 
        Id = "1001" 
    },
    new SchedulerResource() 
    { 
        Name = "James William",  
        Foreground = Colors.Blue, 
        Background = Colors.Green, 
        Id = "1002" 
    },
};

// Assign to scheduler
scheduler.ResourceView.Resources = resources;
```

**Resource display:**
- **Day/Week/WorkWeek views**: Resources displayed **horizontally** on desktop, **adaptive header** on mobile
- **Timeline views**: Resources displayed **vertically**

### Assigning Resources to Appointments

Associate appointments with resources by setting `ResourceIds`:

```csharp
var appointments = new ObservableCollection<SchedulerAppointment>();

appointments.Add(new SchedulerAppointment()
{
    StartTime = DateTime.Today.AddHours(9),
    EndTime = DateTime.Today.AddHours(11),
    Subject = "Client Meeting",
    Location = "Hutchison road",
    ResourceIds = new ObservableCollection<object>() { "1000" } // Sophia's resource
});

scheduler.AppointmentsSource = appointments;
```

### Multiple Resource Sharing

Multiple resources can share the same appointment. Changes to the appointment reflect across all resources simultaneously:

```csharp
var appointment = new SchedulerAppointment()
{
    StartTime = DateTime.Today.AddHours(9),
    EndTime = DateTime.Today.AddHours(11),
    Subject = "Team Meeting",
    Location = "Conference Room",
    // Shared across multiple resources
    ResourceIds = new ObservableCollection<object>() { "1000", "1001", "1002" }
};
```

**Use case:** Team meetings, shared equipment reservations, multi-attendee appointments.

### Resource Grouping

#### Desktop: Grouping by Resource (Default)

By default, `ResourceGroupType` is set to `Resource`, arranging dates under each resource:

```xml
<scheduler:SfScheduler x:Name="scheduler" View="Day">
    <scheduler:SfScheduler.ResourceView>
        <scheduler:SchedulerResourceView ResourceGroupType="Resource"/>
    </scheduler:SfScheduler.ResourceView>
</scheduler:SfScheduler>
```

```csharp
scheduler.ResourceView.ResourceGroupType = SchedulerResourceGroupType.Resource;
```

**Layout:** Resource 1 (All dates) | Resource 2 (All dates) | Resource 3 (All dates)

#### Desktop: Grouping by Date

Set `ResourceGroupType` to `Date` to arrange resources under each date:

```xml
<scheduler:SfScheduler x:Name="scheduler" View="Day">
    <scheduler:SfScheduler.ResourceView>
        <scheduler:SchedulerResourceView ResourceGroupType="Date"/>
    </scheduler:SfScheduler.ResourceView>
</scheduler:SfScheduler>
```

```csharp
scheduler.ResourceView.ResourceGroupType = SchedulerResourceGroupType.Date;
```

**Layout:** Date 1 (All resources) | Date 2 (All resources) | Date 3 (All resources)

#### Mobile: Adaptive Resource Header

On mobile platforms, resources are grouped under an adaptive header/drawer for Day, Week, and WorkWeek views.

### Business Object Binding

Map custom business objects to scheduler resources using `SchedulerResourceMapping`:

**Step 1: Create custom resource class**

```csharp
[Preserve(AllMembers = true)] // Required for AOT on iOS/macOS
public class Employee
{
    public string Name { get; set; }
    public object Id { get; set; }
    public Brush BackgroundColor { get; set; }
    public Brush ForegroundColor { get; set; }
    public string ImageName { get; set; } // Optional: for custom templates
}
```

**Step 2: Configure mapping**

```xml
<scheduler:SfScheduler x:Name="scheduler" View="TimelineWeek">
    <scheduler:SfScheduler.ResourceView>
        <scheduler:SchedulerResourceView Resources="{Binding Employees}">
            <scheduler:SchedulerResourceView.Mapping>
                <scheduler:SchedulerResourceMapping Name="Name"
                                                   Id="Id"
                                                   Background="BackgroundColor"
                                                   Foreground="ForegroundColor"/>
            </scheduler:SchedulerResourceView.Mapping>
        </scheduler:SchedulerResourceView>
    </scheduler:SfScheduler.ResourceView>
</scheduler:SfScheduler>
```

```csharp
scheduler.ResourceView.Mapping = new SchedulerResourceMapping
{
    Name = "Name",
    Id = "Id",
    Background = "BackgroundColor",
    Foreground = "ForegroundColor"
};
```

**Step 3: Create and assign resources**

```csharp
var employees = new ObservableCollection<Employee>()
{
    new Employee 
    { 
        Name = "Sophia", 
        BackgroundColor = Colors.Blue, 
        Id = "1000", 
        ForegroundColor = Colors.Green 
    },
    new Employee 
    { 
        Name = "Zoey Addison", 
        BackgroundColor = Colors.Gold, 
        Id = "1001", 
        ForegroundColor = Colors.White 
    },
};

scheduler.ResourceView.Resources = employees;
```

**Step 4: Map resource IDs to appointments**

```csharp
// Custom appointment class
public class Meeting
{
    public string EventName { get; set; }
    public DateTime From { get; set; }
    public DateTime To { get; set; }
    public ObservableCollection<object> Resources { get; set; }
}

// Configure appointment mapping
scheduler.AppointmentMapping = new SchedulerAppointmentMapping
{
    Subject = "EventName",
    StartTime = "From",
    EndTime = "To",
    ResourceIds = "Resources" // Map to Resources property
};

// Create appointment with resources
var meetings = new ObservableCollection<Meeting>()
{
    new Meeting
    {
        EventName = "Meeting",
        From = DateTime.Today.AddHours(10),
        To = DateTime.Today.AddHours(11),
        Resources = new ObservableCollection<object> { "1000", "1001" }
    }
};

scheduler.AppointmentsSource = meetings;
```

## Resource View Configuration

### Visible Resource Count

Control the number of resources displayed simultaneously using `VisibleResourceCount`:

**Day/Week/WorkWeek views:**

```xml
<scheduler:SfScheduler View="Day">
    <scheduler:SfScheduler.ResourceView>
        <scheduler:SchedulerResourceView VisibleResourceCount="6"/>
    </scheduler:SfScheduler.ResourceView>
</scheduler:SfScheduler>
```

```csharp
scheduler.ResourceView.VisibleResourceCount = 6;
```

**Timeline views:**

```csharp
scheduler.ResourceView.VisibleResourceCount = 4;
```

**Special values:**
- **0**: Removes resource view, shows plain scheduler
- **-1** (default): Horizontal view shows 3 resources; Timeline view calculates based on viewport

### Resource Header Dimensions

#### Days View: Header Height

Customize resource header height in Day/Week/WorkWeek views:

```xml
<scheduler:SfScheduler View="Day">
    <scheduler:SfScheduler.ResourceView>
        <scheduler:SchedulerResourceView ResourceHeaderHeight="100"/>
    </scheduler:SfScheduler.ResourceView>
</scheduler:SfScheduler>
```

```csharp
scheduler.ResourceView.ResourceHeaderHeight = 100;
```

#### Timeline Views: Header Width

Customize resource header width in Timeline views:

```xml
<scheduler:SfScheduler View="TimelineDay">
    <scheduler:SfScheduler.ResourceView>
        <scheduler:SchedulerResourceView ResourceHeaderWidth="250"/>
    </scheduler:SfScheduler.ResourceView>
</scheduler:SfScheduler>
```

```csharp
scheduler.ResourceView.ResourceHeaderWidth = 250;
```

### Minimum Row Height

Set minimum row height for resources in Timeline views:

```xml
<scheduler:SfScheduler View="TimelineWeek">
    <scheduler:SfScheduler.ResourceView>
        <scheduler:SchedulerResourceView MinimumRowHeight="90"/>
    </scheduler:SfScheduler.ResourceView>
</scheduler:SfScheduler>
```

```csharp
scheduler.ResourceView.MinimumRowHeight = 100;
```

**Behavior:**
- Resources auto-expand from minimum height based on appointment count
- Default calculation: Viewport height > 400 → height = viewport / min(resource count, 4)
- If MinimumRowHeight < default, default is used

## Resource Appearance Customization

### Text Style

Customize resource header text style:

```xml
<scheduler:SfScheduler.ResourceView>
    <scheduler:SchedulerResourceView>
        <scheduler:SchedulerResourceView.TextStyle>
            <scheduler:SchedulerTextStyle TextColor="DarkBlue"
                                         FontSize="16"
                                         FontAttributes="Bold"
                                         FontFamily="OpenSansSemibold"/>
        </scheduler:SchedulerResourceView.TextStyle>
    </scheduler:SchedulerResourceView>
</scheduler:SfScheduler.ResourceView>
```

```csharp
scheduler.ResourceView.TextStyle = new SchedulerTextStyle
{
    TextColor = Colors.DarkBlue,
    FontSize = 16,
    FontAttributes = FontAttributes.Bold,
    FontFamily = "OpenSansSemibold"
};
```

### Header Template

Create custom resource header layouts using `HeaderTemplate`:

```xml
<scheduler:SfScheduler View="TimelineMonth">
    <scheduler:SfScheduler.ResourceView>
        <scheduler:SchedulerResourceView Resources="{Binding Employees}">
            <scheduler:SchedulerResourceView.HeaderTemplate>
                <DataTemplate>
                    <VerticalStackLayout Padding="5" 
                                        VerticalOptions="Center" 
                                        HorizontalOptions="Fill">
                        <!-- Profile image with colored border -->
                        <Border StrokeThickness="5"
                               Stroke="{Binding Background}"
                               HorizontalOptions="Center"
                               HeightRequest="70"
                               WidthRequest="70">
                            <Border.StrokeShape>
                                <RoundRectangle CornerRadius="150"/>
                            </Border.StrokeShape>
                            <Image Source="{Binding DataItem.ImageName}"
                                  HeightRequest="55"
                                  WidthRequest="55"
                                  Aspect="Fill"/>
                        </Border>
                        
                        <!-- Employee name -->
                        <Label Text="{Binding Name}" 
                              TextColor="Black" 
                              FontSize="12" 
                              HorizontalTextAlignment="Center"/>
                    </VerticalStackLayout>
                </DataTemplate>
            </scheduler:SchedulerResourceView.HeaderTemplate>
            
            <scheduler:SchedulerResourceView.Mapping>
                <scheduler:SchedulerResourceMapping Name="Name"
                                                   Id="Id"
                                                   Background="Background"
                                                   Foreground="Foreground"/>
            </scheduler:SchedulerResourceView.Mapping>
        </scheduler:SchedulerResourceView>
    </scheduler:SfScheduler.ResourceView>
</scheduler:SfScheduler>
```

**BindingContext:** `SchedulerResource` object
**Access custom data:** Use `DataItem` property (e.g., `{Binding DataItem.ImageName}`)

### Mobile-Specific Customization

#### Hamburger Icon Color

```xml
<scheduler:SfScheduler View="Day">
    <scheduler:SfScheduler.ResourceView>
        <scheduler:SchedulerResourceView HamburgerIconColor="Red"/>
    </scheduler:SfScheduler.ResourceView>
</scheduler:SfScheduler>
```

```csharp
scheduler.ResourceView.HamburgerIconColor = Colors.Red;
```

#### Drawer Resource Selection Color

```xml
<scheduler:SchedulerResourceView DrawerResourceSelectionColor="DodgerBlue"/>
```

```csharp
scheduler.ResourceView.DrawerResourceSelectionColor = Colors.DodgerBlue;
```

#### Drawer Background

```xml
<scheduler:SchedulerResourceView DrawerBackground="LightGoldenrodYellow"/>
```

```csharp
scheduler.ResourceView.DrawerBackground = Colors.LightGoldenrodYellow;
```

#### Adaptive Header Template

Customize the mobile adaptive header:

```xml
<scheduler:SfScheduler.ResourceView>
    <scheduler:SchedulerResourceView>
        <scheduler:SchedulerResourceView.AdaptiveHeaderTemplate>
            <DataTemplate>
                <Grid Padding="8" BackgroundColor="PaleGreen">
                    <HorizontalStackLayout Spacing="8">
                        <!-- Hamburger menu icon -->
                        <Label Text="☰"
                              FontSize="16"
                              TextColor="Red">
                            <Label.GestureRecognizers>
                                <TapGestureRecognizer Tapped="OnTapped"/>
                            </Label.GestureRecognizers>
                        </Label>
                        
                        <!-- Resource name -->
                        <Label Text="{Binding Resource.Name}"
                              FontAttributes="Bold"
                              FontSize="14" 
                              TextColor="DarkViolet"/>
                    </HorizontalStackLayout>
                </Grid>
            </DataTemplate>
        </scheduler:SchedulerResourceView.AdaptiveHeaderTemplate>
    </scheduler:SchedulerResourceView>
</scheduler:SfScheduler.ResourceView>
```

```csharp
private void OnTapped(object sender, TappedEventArgs e)
{
    if (sender is Label label && 
        label.BindingContext is SchedulerAdaptiveResource resource)
    {
        resource.ToggleResourceDrawerView(); // Toggle drawer visibility
    }
}
```

**BindingContext:** `SchedulerAdaptiveResource`
**Method:** `ToggleResourceDrawerView()` - Toggles drawer visibility

#### Drawer Resource Template

Customize drawer resource item appearance:

```xml
<ContentPage.Resources>
    <DataTemplate x:Key="drawerResourceTemplate">
        <Grid Background="{Binding DataItem.BackgroundBrush}">
            <Label Margin="10" 
                  FontAttributes="Bold"  
                  Text="{Binding DataItem.Name}" 
                  TextColor="{Binding DataItem.ForegroundBrush}"/>
        </Grid>
    </DataTemplate>
</ContentPage.Resources>

<scheduler:SfScheduler>
    <scheduler:SfScheduler.ResourceView>
        <scheduler:SchedulerResourceView 
            DrawerResourceTemplate="{StaticResource drawerResourceTemplate}">
            <scheduler:SchedulerResourceView.Mapping>
                <scheduler:SchedulerResourceMapping Id="Id"
                                                   Name="Name"
                                                   Background="BackgroundColor"
                                                   Foreground="ForegroundColor"/>
            </scheduler:SchedulerResourceView.Mapping>
        </scheduler:SchedulerResourceView>
    </scheduler:SfScheduler.ResourceView>
</scheduler:SfScheduler>
```

**BindingContext:** `SchedulerResource`
**Access custom data:** Use `DataItem` property

## Special Time Regions for Resources

Highlight resource availability with special time regions (e.g., lunch breaks, meetings, non-working hours):

### Timeline Views

```csharp
scheduler.ResourceView.Resources = resources;
scheduler.TimelineView.TimeRegions = GetTimeRegion();

private ObservableCollection<SchedulerTimeRegion> GetTimeRegion()
{
    var timeRegions = new ObservableCollection<SchedulerTimeRegion>();
    
    var lunchBreak = new SchedulerTimeRegion()
    {
        StartTime = DateTime.Today.Date.AddHours(13),
        EndTime = DateTime.Today.Date.AddHours(14),
        Text = "Lunch",
        EnablePointerInteraction = false,
        // Apply to specific resources
        ResourceIds = new ObservableCollection<object>() { "1000", "1001", "1002" }
    };
    
    timeRegions.Add(lunchBreak);
    return timeRegions;
}
```

### Days View

```csharp
scheduler.DaysView.TimeRegions = GetTimeRegion();
```

**Properties:**
- **StartTime/EndTime**: Time range
- **Text**: Description
- **EnablePointerInteraction**: Allow user interaction
- **ResourceIds**: Apply to specific resources

## Calendar Types

### Supported Calendars

The scheduler supports multiple calendar systems:

**Supported:**
- GregorianCalendar
- HebrewCalendar
- HijriCalendar (Islamic calendar)
- KoreanCalendar
- TaiwanCalendar
- ThaiCalendar
- UmAlQuraCalendar
- PersianCalendar
- JulianCalendar
- JapaneseCalendar

**Not Supported:**
- Lunar type calendars (Gezer, Haida, Igbo, Javanese, Maramataka, Nepal Sambat, Yoruba)

### Setting Calendar Type

```xml
<scheduler:SfScheduler x:Name="scheduler"  
                       View="TimelineMonth" 
                       CalendarType="Hijri">
</scheduler:SfScheduler>
```

```csharp
scheduler.CalendarType = CalendarType.Hijri;
```

**Note:** FlowDirection automatically updates based on CalendarType. Override by setting `FlowDirection` after `CalendarType`.

### DateTime Values in Calendar Types

When using non-Gregorian calendars, DateTime values (appointments, special time regions, etc.) can be specified in two ways:

#### Method 1: Explicit Calendar Type

Specify dates with explicit calendar type:

```csharp
var appointments = new ObservableCollection<SchedulerAppointment>();

appointments.Add(new SchedulerAppointment()
{
    Subject = "Meeting",
    // Explicit Hijri calendar dates
    StartTime = new DateTime(1443, 02, 22, 10, 0, 0, new HijriCalendar()),
    EndTime = new DateTime(1443, 02, 22, 11, 0, 0, new HijriCalendar()),
});

scheduler.AppointmentsSource = appointments;
```

#### Method 2: System Date Conversion

Specify dates using system DateTime; scheduler converts to calendar type:

```csharp
var appointments = new ObservableCollection<SchedulerAppointment>();

appointments.Add(new SchedulerAppointment()
{
    Subject = "Meeting",
    // System dates automatically converted to Hijri
    StartTime = new DateTime(2021, 09, 29, 10, 0, 0, 0),
    EndTime = new DateTime(2021, 09, 29, 11, 0, 0, 0),
});

scheduler.AppointmentsSource = appointments;
```

**Applies to:**
- Appointment StartTime/EndTime
- SpecialTimeRegion StartTime/EndTime
- DisplayDate
- SelectedDate
- SelectableDayPredicate

## Complete Examples

### Example 1: Employee Scheduling System

```xml
<scheduler:SfScheduler x:Name="scheduler" 
                       View="Week"
                       AppointmentsSource="{Binding Appointments}">
    <scheduler:SfScheduler.ResourceView>
        <scheduler:SchedulerResourceView Resources="{Binding Employees}"
                                        ResourceGroupType="Resource"
                                        VisibleResourceCount="5"
                                        ResourceHeaderHeight="80">
            <scheduler:SchedulerResourceView.Mapping>
                <scheduler:SchedulerResourceMapping Name="Name"
                                                   Id="EmployeeId"
                                                   Background="DepartmentColor"
                                                   Foreground="TextColor"/>
            </scheduler:SchedulerResourceView.Mapping>
            
            <scheduler:SchedulerResourceView.TextStyle>
                <scheduler:SchedulerTextStyle FontSize="14" 
                                             FontAttributes="Bold"/>
            </scheduler:SchedulerResourceView.TextStyle>
        </scheduler:SchedulerResourceView>
    </scheduler:SfScheduler.ResourceView>
</scheduler:SfScheduler>
```

### Example 2: Meeting Room Booking with Multiple Calendars

```csharp
public class MeetingRoomScheduler : ContentPage
{
    public MeetingRoomScheduler()
    {
        var scheduler = new SfScheduler
        {
            View = SchedulerView.TimelineWeek,
            CalendarType = CalendarType.Hijri // Islamic calendar
        };
        
        // Create room resources
        var rooms = new ObservableCollection<SchedulerResource>
        {
            new SchedulerResource 
            { 
                Name = "Conference Room A", 
                Id = "room-a", 
                Background = Colors.LightBlue 
            },
            new SchedulerResource 
            { 
                Name = "Conference Room B", 
                Id = "room-b", 
                Background = Colors.LightGreen 
            },
        };
        
        scheduler.ResourceView.Resources = rooms;
        scheduler.ResourceView.VisibleResourceCount = 2;
        scheduler.ResourceView.ResourceHeaderWidth = 200;
        
        // Add lunch break time region
        scheduler.TimelineView.TimeRegions = new ObservableCollection<SchedulerTimeRegion>
        {
            new SchedulerTimeRegion
            {
                StartTime = DateTime.Today.AddHours(12),
                EndTime = DateTime.Today.AddHours(13),
                Text = "Lunch Break",
                EnablePointerInteraction = false,
                ResourceIds = new ObservableCollection<object> { "room-a", "room-b" }
            }
        };
        
        Content = scheduler;
    }
}
```

## Troubleshooting

### Resource Issues

**Problem:** Resources not displaying  
**Solution:** Verify `Resources` collection is assigned to `ResourceView.Resources` and contains valid `Id` values

**Problem:** Appointments not showing under resources  
**Solution:** Ensure `ResourceIds` in appointments match resource `Id` values exactly (case-sensitive)

**Problem:** Resource grouping not working on mobile  
**Solution:** `ResourceGroupType` is for desktop only. Mobile uses adaptive header/drawer by default

**Problem:** Custom business objects not mapping  
**Solution:** 
- Verify `SchedulerResourceMapping` property names match custom class exactly
- Add `[Preserve(AllMembers = true)]` to custom class for AOT (iOS/macOS)

### Resource View Configuration Issues

**Problem:** VisibleResourceCount not working  
**Solution:** 
- Horizontal resource view (Day/Week): Only works on Windows/macOS
- Timeline resource view: Works on all platforms
- Set to -1 for default behavior

**Problem:** Resource header too small/large  
**Solution:** 
- Days view: Adjust `ResourceHeaderHeight`
- Timeline view: Adjust `ResourceHeaderWidth`
- Check if custom templates override dimensions

### Calendar Type Issues

**Problem:** Dates not converting correctly  
**Solution:** 
- Ensure `CalendarType` is set before assigning appointments
- Use explicit calendar type in DateTime constructor for clarity
- Verify calendar is in supported list (not lunar calendars)

**Problem:** FlowDirection incorrect for RTL calendars  
**Solution:** Set `FlowDirection` property after setting `CalendarType`

### Template Issues

**Problem:** HeaderTemplate not rendering  
**Solution:** 
- BindingContext is `SchedulerResource`
- Access custom data via `DataItem` property
- Verify DataTemplate markup is valid

**Problem:** AdaptiveHeaderTemplate hamburger not working  
**Solution:** Call `ToggleResourceDrawerView()` on `SchedulerAdaptiveResource` instance from BindingContext

---

**Related References:**
- [Getting Started](getting-started.md) - Initial setup
- [Appointments](appointments.md) - Creating appointments with resources
- [Day, Week, and WorkWeek Views](day-week-views.md) - Resource layout in days views
- [Timeline Views](timeline-views.md) - Resource layout in timeline views
- [Advanced Features](advanced-features.md) - Special time regions
