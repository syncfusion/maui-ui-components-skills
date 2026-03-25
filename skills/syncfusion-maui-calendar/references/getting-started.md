# Getting Started with .NET MAUI Calendar

This guide covers the installation, setup, and basic usage of the Syncfusion .NET MAUI Calendar (SfCalendar) control.

## Installation Steps

### Step 1: Create a .NET MAUI Project

**Visual Studio 2022:**
1. Go to **File > New > Project**
2. Choose the **.NET MAUI App** template
3. Name the project and choose a location
4. Click **Next**, select the .NET framework version, and click **Create**

### Step 2: Install Syncfusion.Maui.Calendar NuGet Package

**Option A: Visual Studio NuGet Package Manager**
1. In **Solution Explorer**, right-click the project
2. Choose **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.Calendar`
4. Install the latest version
5. Ensure dependencies are installed correctly

**Option B: Package Manager Console**
```powershell
Install-Package Syncfusion.Maui.Calendar
```

**Option C: .NET CLI**
```bash
dotnet add package Syncfusion.Maui.Calendar
```

**Option D: Manual .csproj Edit**
```xml
<ItemGroup>
    <PackageReference Include="Syncfusion.Maui.Calendar" Version="*" />
</ItemGroup>
```

After adding, run `dotnet restore` to ensure all dependencies are installed.

### Step 3: Register the Syncfusion Handler

The `Syncfusion.Maui.Core` NuGet is a dependent package for all Syncfusion controls. Register the handler in `MauiProgram.cs`:

```csharp
using Syncfusion.Maui.Core.Hosting;

namespace MyCalendarApp
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();

            // Register Syncfusion Core Handler
            builder.ConfigureSyncfusionCore();
            
            builder
                .UseMauiApp<App>()
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                    fonts.AddFont("OpenSans-Semibold.ttf", "OpenSansSemibold");
                });

            return builder.Build();
        }
    }
}
```

**Important:** The `ConfigureSyncfusionCore()` call is **required** for all Syncfusion MAUI controls.

## Basic Implementation

### Step 4: Add SfCalendar to Your Page

**XAML Implementation:**

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:calendar="clr-namespace:Syncfusion.Maui.Calendar;assembly=Syncfusion.Maui.Calendar"
             x:Class="MyCalendarApp.MainPage">
    
    <calendar:SfCalendar x:Name="calendar" />
    
</ContentPage>
```

**C# Implementation:**

```csharp
using Syncfusion.Maui.Calendar;

namespace MyCalendarApp
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            SfCalendar calendar = new SfCalendar();
            this.Content = calendar;
        }
    }
}
```

This creates a basic calendar with default settings:
- **View:** Month view
- **Selection Mode:** Single date selection
- **Display Date:** Current date

## First Day of Week

Customize the starting day of the week using the `FirstDayOfWeek` property. The default is Sunday.

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar">
    <calendar:SfCalendar.MonthView>
        <calendar:CalendarMonthView FirstDayOfWeek="Monday" />
    </calendar:SfCalendar.MonthView>
</calendar:SfCalendar>
```

**C#:**
```csharp
calendar.MonthView = new CalendarMonthView
{
    FirstDayOfWeek = DayOfWeek.Monday
};
```

**Available Values:**
- `Sunday` (default)
- `Monday`
- `Tuesday`
- `Wednesday`
- `Thursday`
- `Friday`
- `Saturday`

## Corner Radius

Customize the corner radius of the calendar using the `CornerRadius` property. The default value is `20`.

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar" 
                     View="Month"
                     CornerRadius="15" />
```

**C#:**
```csharp
calendar.CornerRadius = 15;
```

**Examples:**
```csharp
// Sharp corners
calendar.CornerRadius = 0;

// Rounded corners (default)
calendar.CornerRadius = 20;

// Custom radius per corner (Top-Left, Top-Right, Bottom-Left, Bottom-Right)
calendar.CornerRadius = new CornerRadius(20, 0, 0, 20);
```

## Quick Configuration Examples

### Example 1: Month View with Monday Start

```xml
<calendar:SfCalendar x:Name="calendar" View="Month">
    <calendar:SfCalendar.MonthView>
        <calendar:CalendarMonthView FirstDayOfWeek="Monday" />
    </calendar:SfCalendar.MonthView>
</calendar:SfCalendar>
```

### Example 2: Calendar with Custom Corner Radius

```csharp
SfCalendar calendar = new SfCalendar
{
    View = CalendarView.Month,
    CornerRadius = 10,
    BackgroundColor = Colors.White
};
```

### Example 3: Single Selection with Custom Starting Date

```csharp
SfCalendar calendar = new SfCalendar
{
    View = CalendarView.Month,
    SelectionMode = CalendarSelectionMode.Single,
    DisplayDate = new DateTime(2026, 6, 1)
};
```

## Troubleshooting

### Issue: Calendar Not Displaying

**Solution:**
1. Verify `ConfigureSyncfusionCore()` is called in `MauiProgram.cs`
2. Ensure the NuGet package is installed correctly
3. Check that the namespace is imported: `xmlns:calendar="clr-namespace:Syncfusion.Maui.Calendar;assembly=Syncfusion.Maui.Calendar"`
4. Verify the project targets .NET 9 or later

### Issue: "The type or namespace name 'Syncfusion' could not be found"

**Solution:**
1. Restore NuGet packages: `dotnet restore`
2. Clean and rebuild the solution
3. Verify the package is listed in the `.csproj` file
4. Check NuGet package sources in Visual Studio settings

## Next Steps

Now that you have the calendar set up, explore these features:
- **Views:** Learn about Month, Year, Decade, and Century views
- **Selection Modes:** Implement Single, Multiple, or Range selection
- **Date Restrictions:** Set minimum/maximum dates and custom validation
- **Customization:** Style cells, backgrounds, and text
- **Events:** Handle user interactions with calendar events