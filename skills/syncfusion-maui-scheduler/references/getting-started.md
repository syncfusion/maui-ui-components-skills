# Getting Started with .NET MAUI Scheduler

This guide covers the complete setup process for the Syncfusion .NET MAUI Scheduler (SfScheduler), from installation to creating your first scheduling application with appointments.

## Table of Contents
- [Installation](#installation)
- [Register the Handler](#register-the-handler)
- [Add Scheduler to Your Page](#add-scheduler-to-your-page)
- [Change Scheduler Views](#change-scheduler-views)
- [Creating Your First Appointments](#creating-your-first-appointments)
- [Custom Business Object Mapping](#custom-business-object-mapping)
- [Basic Customization](#basic-customization)
- [Common Issues](#common-issues)

## Installation

### Step 1: Create a New .NET MAUI Project

#### Using Visual Studio 2022
1. Go to **File > New > Project**
2. Choose the **.NET MAUI App** template
3. Name your project (e.g., "MySchedulerApp")
4. Choose a location and click **Next**
5. Select the .NET framework version (7.0 or later)
6. Click **Create**

#### Using Visual Studio Code
1. Open the Command Palette (`Ctrl+Shift+P` / `Cmd+Shift+P`)
2. Type **.NET: New Project** and press Enter
3. Choose **.NET MAUI App** template
4. Select project location
5. Enter project name and press Enter
6. Click **Create project**

### Step 2: Install Syncfusion .NET MAUI Scheduler NuGet Package

#### Using Visual Studio/Rider
1. In **Solution Explorer**, right-click your project
2. Choose **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.Scheduler`
4. Install the latest version
5. Accept the license agreement
6. Ensure all dependencies install correctly

#### Using Command Line (Visual Studio Code)
```bash
# Navigate to your project directory
cd MySchedulerApp

# Install the package
dotnet add package Syncfusion.Maui.Scheduler

# Restore dependencies
dotnet restore
```

#### Verify Installation

Check your `.csproj` file should contain:

```xml
<ItemGroup>
  <PackageReference Include="Syncfusion.Maui.Scheduler" Version="27.*.*" />
</ItemGroup>
```

## Register the Handler

The `Syncfusion.Maui.Core` NuGet package is a dependency for all Syncfusion .NET MAUI controls. You **must** register the Syncfusion Core handler in your `MauiProgram.cs` file.

### Open MauiProgram.cs

Add the following code:

```csharp
using Syncfusion.Maui.Core.Hosting;

namespace MySchedulerApp
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            
            // ⚠️ CRITICAL: Register Syncfusion Core
            builder.ConfigureSyncfusionCore();
            
            builder
                .UseMauiApp<App>()
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                    fonts.AddFont("Segoe-mdl2.ttf", "SegoeMDL2");
                });

            return builder.Build();
        }
    }
}
```

**Important Notes:**
- `ConfigureSyncfusionCore()` MUST be called before `UseMauiApp<App>()`
- Missing this step will cause runtime errors
- This registration is required only once per application

## Add Scheduler to Your Page

### Step 1: Add XML Namespace

In your XAML file (e.g., `MainPage.xaml`), add the Scheduler namespace:

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:scheduler="clr-namespace:Syncfusion.Maui.Scheduler;assembly=Syncfusion.Maui.Scheduler"
             x:Class="MySchedulerApp.MainPage">
    
    <!-- Your content here -->
    
</ContentPage>
```

### Step 2: Add the Scheduler Control

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:scheduler="clr-namespace:Syncfusion.Maui.Scheduler;assembly=Syncfusion.Maui.Scheduler"
             x:Class="MySchedulerApp.MainPage">

    <scheduler:SfScheduler x:Name="Scheduler" />

</ContentPage>
```

### Using Code-Behind (C#)

Alternatively, create the Scheduler in code:

```csharp
using Syncfusion.Maui.Scheduler;

namespace MySchedulerApp
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            SfScheduler scheduler = new SfScheduler();
            this.Content = scheduler;
        }
    }
}
```

### Run Your Application

At this point, you should see an empty scheduler with the default **Day view** displaying the current date.

## Change Scheduler Views

The Scheduler provides nine different view modes. Set the view using the `View` property:

### In XAML

```xml
<scheduler:SfScheduler x:Name="Scheduler" View="Week" />
```

### In C#

```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Week;
this.Content = scheduler;
```

### Available View Modes

| View | Description |
|------|-------------|
| `Day` | Displays a single day (default) |
| `Week` | Displays 7 days of a week |
| `WorkWeek` | Displays only working days (Mon-Fri by default) |
| `Month` | Displays an entire month |
| `Agenda` | List view of appointments grouped by date |
| `TimelineDay` | Horizontal view of a single day |
| `TimelineWeek` | Horizontal view of a week |
| `TimelineWorkWeek` | Horizontal view of working days |
| `TimelineMonth` | Horizontal view of a month |

### Example: Switching Views

```xml
<VerticalStackLayout Padding="10">
    <HorizontalStackLayout Spacing="5">
        <Button Text="Day" Clicked="OnDayViewClicked" />
        <Button Text="Week" Clicked="OnWeekViewClicked" />
        <Button Text="Month" Clicked="OnMonthViewClicked" />
    </HorizontalStackLayout>
    
    <scheduler:SfScheduler x:Name="Scheduler" View="Week" />
</VerticalStackLayout>
```

```csharp
private void OnDayViewClicked(object sender, EventArgs e)
{
    Scheduler.View = SchedulerView.Day;
}

private void OnWeekViewClicked(object sender, EventArgs e)
{
    Scheduler.View = SchedulerView.Week;
}

private void OnMonthViewClicked(object sender, EventArgs e)
{
    Scheduler.View = SchedulerView.Month;
}
```

## Creating Your First Appointments

### Using SchedulerAppointment Class

The simplest way to add appointments is using the built-in `SchedulerAppointment` class:

```csharp
using Syncfusion.Maui.Scheduler;
using System.Collections.ObjectModel;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        // Create appointment collection
        var appointments = new ObservableCollection<SchedulerAppointment>();
        
        // Add a simple appointment
        appointments.Add(new SchedulerAppointment
        {
            StartTime = DateTime.Today.AddHours(9),
            EndTime = DateTime.Today.AddHours(11),
            Subject = "Team Meeting",
            Location = "Conference Room A"
        });
        
        // Add another appointment
        appointments.Add(new SchedulerAppointment
        {
            StartTime = DateTime.Today.AddHours(14),
            EndTime = DateTime.Today.AddHours(15),
            Subject = "Project Review",
            Notes = "Quarterly review with stakeholders"
        });
        
        // Bind to the Scheduler
        Scheduler.AppointmentsSource = appointments;
    }
}
```

### SchedulerAppointment Properties

| Property | Type | Description |
|----------|------|-------------|
| `StartTime` | DateTime | Start date and time (required) |
| `EndTime` | DateTime | End date and time (required) |
| `Subject` | string | Appointment title/subject (required) |
| `Location` | string | Location information |
| `Notes` | string | Additional notes or description |
| `IsAllDay` | bool | Whether it's an all-day event |
| `Background` | Brush | Background color |
| `TextColor` | Color | Text color |
| `RecurrenceRule` | string | Recurrence pattern (RRULE format) |
| `RecurrenceId` | object | Links exception to recurring appointment |
| `RecurrenceExceptionDates` | ObservableCollection\<DateTime\> | Dates to exclude from recurrence |
| `Id` | object | Unique identifier |
| `IsReadOnly` | bool | Prevents editing |

### Complete Example in XAML + Code-Behind

**MainPage.xaml:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:scheduler="clr-namespace:Syncfusion.Maui.Scheduler;assembly=Syncfusion.Maui.Scheduler"
             x:Class="MySchedulerApp.MainPage">

    <scheduler:SfScheduler x:Name="Scheduler" View="Week" />

</ContentPage>
```

**MainPage.xaml.cs:**
```csharp
using Syncfusion.Maui.Scheduler;
using System.Collections.ObjectModel;

namespace MySchedulerApp
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            CreateAppointments();
        }

        private void CreateAppointments()
        {
            var appointments = new ObservableCollection<SchedulerAppointment>
            {
                new SchedulerAppointment
                {
                    StartTime = DateTime.Today.AddHours(9),
                    EndTime = DateTime.Today.AddHours(10),
                    Subject = "Daily Standup",
                    Background = Brush.LightBlue
                },
                new SchedulerAppointment
                {
                    StartTime = DateTime.Today.AddHours(11),
                    EndTime = DateTime.Today.AddHours(12, 30),
                    Subject = "Client Presentation",
                    Location = "Meeting Room 2",
                    Background = Brush.Orange
                },
                new SchedulerAppointment
                {
                    StartTime = DateTime.Today.AddHours(14),
                    EndTime = DateTime.Today.AddHours(15),
                    Subject = "Code Review",
                    Notes = "Review PR #245",
                    Background = Brush.LightGreen
                }
            };

            Scheduler.AppointmentsSource = appointments;
        }
    }
}
```

## Custom Business Object Mapping

For real-world applications, you'll typically have your own data models. The Scheduler supports mapping custom business objects to appointments.

### Step 1: Create Your Business Object

```csharp
public class Meeting
{
    public DateTime From { get; set; }
    public DateTime To { get; set; }
    public string Title { get; set; }
    public string Room { get; set; }
    public string Description { get; set; }
    public Brush Color { get; set; }
    public bool AllDay { get; set; }
}
```

### Step 2: Configure AppointmentMapping

**In XAML:**
```xml
<scheduler:SfScheduler x:Name="Scheduler" View="Week">
    <scheduler:SfScheduler.AppointmentMapping>
        <scheduler:SchedulerAppointmentMapping
            StartTime="From"
            EndTime="To"
            Subject="Title"
            Location="Room"
            Notes="Description"
            Background="Color"
            IsAllDay="AllDay" />
    </scheduler:SfScheduler.AppointmentMapping>
</scheduler:SfScheduler>
```

**In C#:**
```csharp
SfScheduler scheduler = new SfScheduler();
scheduler.View = SchedulerView.Week;

SchedulerAppointmentMapping mapping = new SchedulerAppointmentMapping
{
    StartTime = "From",
    EndTime = "To",
    Subject = "Title",
    Location = "Room",
    Notes = "Description",
    Background = "Color",
    IsAllDay = "AllDay"
};

scheduler.AppointmentMapping = mapping;
```

### Step 3: Bind Your Custom Data

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        var meetings = new ObservableCollection<Meeting>
        {
            new Meeting
            {
                From = DateTime.Today.AddHours(10),
                To = DateTime.Today.AddHours(11),
                Title = "Sprint Planning",
                Room = "Conference Room A",
                Color = Brush.Blue,
                AllDay = false
            },
            new Meeting
            {
                From = DateTime.Today.AddHours(15),
                To = DateTime.Today.AddHours(16),
                Title = "Design Review",
                Room = "Design Studio",
                Color = Brush.Purple,
                AllDay = false
            }
        };
        
        Scheduler.AppointmentsSource = meetings;
    }
}
```

### Using ViewModel Pattern (MVVM)

For better separation of concerns, use a ViewModel:

**SchedulerViewModel.cs:**
```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;

public class SchedulerViewModel : INotifyPropertyChanged
{
    private ObservableCollection<Meeting> _meetings;
    
    public ObservableCollection<Meeting> Meetings
    {
        get => _meetings;
        set
        {
            _meetings = value;
            OnPropertyChanged(nameof(Meetings));
        }
    }
    
    public SchedulerViewModel()
    {
        LoadMeetings();
    }
    
    private void LoadMeetings()
    {
        Meetings = new ObservableCollection<Meeting>
        {
            new Meeting
            {
                From = DateTime.Today.AddHours(9),
                To = DateTime.Today.AddHours(10),
                Title = "Team Sync",
                Room = "Virtual",
                Color = Brush.LightBlue,
                AllDay = false
            }
            // Add more meetings...
        };
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**MainPage.xaml:**
```xml
<ContentPage ...>
    <ContentPage.BindingContext>
        <local:SchedulerViewModel />
    </ContentPage.BindingContext>

    <scheduler:SfScheduler x:Name="Scheduler" 
                           View="Week"
                           AppointmentsSource="{Binding Meetings}">
        <scheduler:SfScheduler.AppointmentMapping>
            <scheduler:SchedulerAppointmentMapping
                StartTime="From"
                EndTime="To"
                Subject="Title"
                Location="Room"
                Background="Color"
                IsAllDay="AllDay" />
        </scheduler:SfScheduler.AppointmentMapping>
    </scheduler:SfScheduler>
</ContentPage>
```

## Basic Customization

### Change First Day of Week

```xml
<scheduler:SfScheduler x:Name="Scheduler" FirstDayOfWeek="Monday" />
```

```csharp
Scheduler.FirstDayOfWeek = DayOfWeek.Monday;
```

### Customize Today Highlight

```xml
<scheduler:SfScheduler x:Name="Scheduler" TodayHighlightBrush="Orange" />
```

```csharp
Scheduler.TodayHighlightBrush = Brush.Orange;
```

### Customize Cell Borders

```xml
<scheduler:SfScheduler x:Name="Scheduler" CellBorderBrush="LightGray" />
```

```csharp
Scheduler.CellBorderBrush = Brush.LightGray;
```

### Show/Hide Navigation Arrows

```xml
<scheduler:SfScheduler x:Name="Scheduler" ShowNavigationArrows="True" />
```

```csharp
Scheduler.ShowNavigationArrows = true; // Shows Today button and arrow navigation
```

### Show Week Numbers

```xml
<scheduler:SfScheduler x:Name="Scheduler" ShowWeekNumber="True" />
```

```csharp
Scheduler.ShowWeekNumber = true;
```

### Set Background Color

```xml
<scheduler:SfScheduler x:Name="Scheduler" BackgroundColor="WhiteSmoke" />
```

```csharp
Scheduler.BackgroundColor = Colors.WhiteSmoke;
```

## Common Issues

### Issue 1: Appointments Not Showing

**Symptoms:** Appointments are added but don't appear in the Scheduler.

**Solutions:**
- Verify `StartTime` and `EndTime` are set correctly
- Ensure dates fall within the visible date range
- Check that `AppointmentsSource` is assigned
- Verify the current view mode displays that time period
- Ensure `EndTime` is after `StartTime`

```csharp
// ❌ Wrong: EndTime before StartTime
new SchedulerAppointment
{
    StartTime = DateTime.Today.AddHours(10),
    EndTime = DateTime.Today.AddHours(9) // Wrong!
};

// ✅ Correct
new SchedulerAppointment
{
    StartTime = DateTime.Today.AddHours(9),
    EndTime = DateTime.Today.AddHours(10)
};
```

### Issue 2: Handler Not Registered Error

**Error:** Runtime exception about missing handler or Syncfusion controls not initializing.

**Solution:** Ensure `ConfigureSyncfusionCore()` is called in `MauiProgram.cs`:

```csharp
public static MauiApp CreateMauiApp()
{
    var builder = MauiApp.CreateBuilder();
    builder.ConfigureSyncfusionCore(); // ⚠️ MUST be here
    // ... rest of configuration
}
```

### Issue 3: Custom Objects Not Mapping

**Symptoms:** Custom business objects don't display in Scheduler.

**Solutions:**
- Verify `AppointmentMapping` property names match your class **exactly** (case-sensitive)
- Ensure your business object has `DateTime` properties for start and end times
- Check that `AppointmentsSource` is bound correctly

```csharp
// Your class
public class Meeting
{
    public DateTime From { get; set; } // Property name: "From"
}

// Mapping must match exactly
<scheduler:SchedulerAppointmentMapping StartTime="From" ... />
```

### Issue 4: View Not Updating After Data Changes

**Symptoms:** Changes to appointment collection don't reflect in UI.

**Solution:** Use `ObservableCollection<T>` for automatic updates:

```csharp
// ❌ Wrong: Regular List won't notify UI
var appointments = new List<SchedulerAppointment>();

// ✅ Correct: ObservableCollection notifies UI of changes
var appointments = new ObservableCollection<SchedulerAppointment>();
```

### Issue 5: NuGet Package Installation Fails

**Error:** Package restore errors or dependency conflicts.

**Solutions:**
- Clear NuGet cache: `dotnet nuget locals all --clear`
- Ensure you're using .NET 9 or later
- Check internet connection
- Try installing from Package Manager Console: `Install-Package Syncfusion.Maui.Scheduler`

## Next Steps

Now that you have a working Scheduler with basic appointments, explore these topics:

- **Recurring Appointments:** [appointments.md](appointments.md) - Create daily, weekly, monthly recurring events
- **View Customization:** [day-week-views.md](day-week-views.md) - Customize time slots, working hours, special regions
- **Timeline Views:** [timeline-views.md](timeline-views.md) - Implement horizontal timeline layouts
- **Appointment Interactions:** [appointment-interactions.md](appointment-interactions.md) - Enable drag-drop, resizing, custom editors
- **Resource Management:** [resources-calendars.md](resources-calendars.md) - Multiple resources, calendar types

## Complete Starter Example

Here's a complete, copy-paste-ready example to get started:

**MauiProgram.cs:**
```csharp
using Syncfusion.Maui.Core.Hosting;

namespace MySchedulerApp
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            builder.ConfigureSyncfusionCore();
            builder.UseMauiApp<App>();
            return builder.Build();
        }
    }
}
```

**MainPage.xaml:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:scheduler="clr-namespace:Syncfusion.Maui.Scheduler;assembly=Syncfusion.Maui.Scheduler"
             x:Class="MySchedulerApp.MainPage">

    <scheduler:SfScheduler x:Name="Scheduler" View="Week" />

</ContentPage>
```

**MainPage.xaml.cs:**
```csharp
using Syncfusion.Maui.Scheduler;
using System.Collections.ObjectModel;

namespace MySchedulerApp
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            var appointments = new ObservableCollection<SchedulerAppointment>
            {
                new SchedulerAppointment
                {
                    StartTime = DateTime.Today.AddHours(10),
                    EndTime = DateTime.Today.AddHours(11),
                    Subject = "Team Meeting",
                    Background = Brush.Blue
                }
            };
            
            Scheduler.AppointmentsSource = appointments;
        }
    }
}
```

That's it! You now have a fully functional .NET MAUI Scheduler application.
