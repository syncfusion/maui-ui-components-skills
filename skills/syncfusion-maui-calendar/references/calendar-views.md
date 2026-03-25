# Calendar Views in .NET MAUI Calendar

The SfCalendar provides four different view types for displaying and selecting dates. Each view serves a specific navigation and selection purpose.

## Table of Contents
- [Month View](#month-view)
- [Year View](#year-view)
- [Decade View](#decade-view)
- [Century View](#century-view)
- [View Property](#view-property)
- [Number of Visible Weeks](#number-of-visible-weeks)
- [Week Numbers](#week-numbers)
- [Allow View Navigation](#allow-view-navigation)

## Month View

The Month view displays the days of the current month along with some days from the previous and next months. This is the **default view** when the calendar is initialized.

**Key Features:**
- Shows dates of the current month
- Displays trailing dates (previous month) and leading dates (next month)
- Current date is highlighted by default
- Most commonly used view for date selection

### Basic Month View

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar" 
                     View="Month" />
```

**C#:**
```csharp
calendar.View = CalendarView.Month;
```

The calendar initially displays the current date when opened in Month view.

## Year View

The Year view displays all 12 months of the year in a grid format, allowing users to quickly navigate to a specific month.

**Key Features:**
- Shows all months (Jan-Dec) of the current year
- Allows quick month selection
- Used for month-level navigation
- When tapped (with `AllowViewNavigation=true`), navigates to Month view

### Basic Year View

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar" 
                     View="Year" />
```

**C#:**
```csharp
calendar.View = CalendarView.Year;
```

**Selection Behavior:**
- When `AllowViewNavigation = true`: Tapping a month navigates to Month view for that month
- When `AllowViewNavigation = false`: You can select months directly; the SelectionChanged event returns the first date of the selected month (e.g., selecting "Dec" returns `01-12-2026`)

## Decade View

The Decade view displays 12 years (typically the current decade plus adjacent years) for quick year selection.

**Key Features:**
- Shows years in groups of 12
- Covers roughly one decade
- Used for year-level navigation
- When tapped (with `AllowViewNavigation=true`), navigates to Year view

### Basic Decade View

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar" 
                     View="Decade" />
```

**C#:**
```csharp
calendar.View = CalendarView.Decade;
```

**Selection Behavior:**
- When `AllowViewNavigation = true`: Tapping a year navigates to Year view
- When `AllowViewNavigation = false`: You can select years directly; the SelectionChanged event returns the first date of the selected year (e.g., selecting "2026" returns `01-01-2026`)

**Range Selection Example:**
When range selection is enabled and you select years 2022-2025, the returned range is `01-01-2022` to `31-12-2025`.

## Century View

The Century view displays decades (groups of 10 years) for long-range navigation spanning a century.

**Key Features:**
- Shows decades (e.g., "2020-2029", "2030-2039")
- Used for century-level navigation
- When tapped (with `AllowViewNavigation=true`), navigates to Decade view

### Basic Century View

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar" 
                     View="Century" />
```

**C#:**
```csharp
calendar.View = CalendarView.Century;
```

**Selection Behavior:**
- When `AllowViewNavigation = true`: Tapping a decade navigates to Decade view
- When `AllowViewNavigation = false`: You can select decades directly; the SelectionChanged event returns the first date of the selected decade (e.g., selecting "2020-2029" returns `01-01-2020`)

**Range Selection Example:**
When range selection is enabled and you select "2020-2029" to "2030-2039", the returned range is `01-01-2020` to `31-12-2039`.

## View Property

Switch between views using the `View` property. This property determines which calendar view is currently displayed.

### View Enumeration Values

```csharp
public enum CalendarView
{
    Month,
    Year,
    Decade,
    Century
}
```

### Programmatic View Switching

```csharp
// Switch to Year view
calendar.View = CalendarView.Year;

// Switch to Month view
calendar.View = CalendarView.Month;

// Switch to Decade view
calendar.View = CalendarView.Decade;

// Switch to Century view
calendar.View = CalendarView.Century;
```

### Dynamic View Switching Example

```csharp
private void OnViewButtonClicked(object sender, EventArgs e)
{
    // Cycle through views
    calendar.View = calendar.View switch
    {
        CalendarView.Month => CalendarView.Year,
        CalendarView.Year => CalendarView.Decade,
        CalendarView.Decade => CalendarView.Century,
        CalendarView.Century => CalendarView.Month,
        _ => CalendarView.Month
    };
}
```

## Number of Visible Weeks

In Month view, customize the number of visible weeks using the `NumberOfVisibleWeeks` property. By default, this is set to `6`.

**Valid Values:** 1-6 weeks

### Custom Visible Weeks

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar" View="Month">
    <calendar:SfCalendar.MonthView>
        <calendar:CalendarMonthView NumberOfVisibleWeeks="3" />
    </calendar:SfCalendar.MonthView>
</calendar:SfCalendar>
```

**C#:**
```csharp
calendar.MonthView = new CalendarMonthView
{
    NumberOfVisibleWeeks = 3
};
```

**Use Cases:**
- `NumberOfVisibleWeeks = 1`: Show only one week (compact view)
- `NumberOfVisibleWeeks = 3`: Show three weeks (mid-size view)
- `NumberOfVisibleWeeks = 6`: Show full month with adjacent dates (default)

## Week Numbers

Display ISO week numbers alongside the calendar dates using the `ShowWeekNumber` property. Week numbers are displayed based on the ISO 8601 standard.

### Enable Week Numbers

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar" View="Month">
    <calendar:SfCalendar.MonthView>
        <calendar:CalendarMonthView ShowWeekNumber="True" />
    </calendar:SfCalendar.MonthView>
</calendar:SfCalendar>
```

**C#:**
```csharp
calendar.MonthView = new CalendarMonthView
{
    ShowWeekNumber = true
};
```

By default, `ShowWeekNumber` is `false`.

### Week Number Appearance

Customize the appearance of week numbers using the `WeekNumberStyle` property.

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar" View="Month">
    <calendar:SfCalendar.MonthView>
        <calendar:CalendarMonthView ShowWeekNumber="True">
            <calendar:CalendarMonthView.WeekNumberStyle>
                <calendar:CalendarWeekNumberStyle Background="DeepSkyBlue">
                    <calendar:CalendarWeekNumberStyle.TextStyle>
                        <calendar:CalendarTextStyle TextColor="White" 
                                                   FontSize="12"
                                                   FontAttributes="Bold" />
                    </calendar:CalendarWeekNumberStyle.TextStyle>
                </calendar:CalendarWeekNumberStyle>
            </calendar:CalendarMonthView.WeekNumberStyle>
        </calendar:CalendarMonthView>
    </calendar:SfCalendar.MonthView>
</calendar:SfCalendar>
```

**C#:**
```csharp
CalendarTextStyle weekNumberTextStyle = new CalendarTextStyle
{
    TextColor = Colors.White,
    FontSize = 12,
    FontAttributes = FontAttributes.Bold
};

calendar.MonthView = new CalendarMonthView
{
    ShowWeekNumber = true,
    WeekNumberStyle = new CalendarWeekNumberStyle
    {
        Background = Colors.DeepSkyBlue,
        TextStyle = weekNumberTextStyle
    }
};
```

**Customization Options:**
- `Background`: Background color of the week number column
- `TextStyle`: Font size, color, family, and attributes

## Allow View Navigation

The `AllowViewNavigation` property controls whether users can navigate between views by tapping cells or headers.

**Default:** `true` (navigation enabled)

### When AllowViewNavigation = true

Users can navigate through views by tapping:
- **Century View:** Tap a decade → navigate to Decade view
- **Decade View:** Tap a year → navigate to Year view
- **Year View:** Tap a month → navigate to Month view
- **Month View:** Tap header → navigate to Year view (then Decade, then Century)

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar" 
                     View="Month"
                     AllowViewNavigation="True" />
```

### When AllowViewNavigation = false

Users can select cells directly in Year, Decade, and Century views without navigating.

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar" 
                     View="Year"
                     AllowViewNavigation="False"
                     SelectionMode="Single" />
```

**Use Case:** Allow users to select a month, year, or decade directly without drilling down to individual dates.

**Selection Behavior Examples:**

1. **Year View with AllowViewNavigation=false:**
   - Select "Dec 2026" → SelectionChanged returns `DateTime(2026, 12, 1)`
   
2. **Decade View with AllowViewNavigation=false:**
   - Select "2026" → SelectionChanged returns `DateTime(2026, 1, 1)`
   
3. **Century View with AllowViewNavigation=false:**
   - Select "2020-2029" → SelectionChanged returns `DateTime(2020, 1, 1)`

## View Navigation Examples

### Example 1: Drill-Down Navigation

```csharp
// Start at Century view for long-range navigation
calendar.View = CalendarView.Century;
calendar.AllowViewNavigation = true;

// User taps: "2020-2029" → "2026" → "March" → selects a specific date
```

### Example 2: Month Selection (No Date Selection)

```csharp
// Allow month selection without drilling to dates
calendar.View = CalendarView.Year;
calendar.AllowViewNavigation = false;
calendar.SelectionMode = CalendarSelectionMode.Single;

calendar.SelectionChanged += (s, e) =>
{
    // User selects "June" → e.NewValue = DateTime(2026, 6, 1)
    var selectedMonth = ((DateTime)e.NewValue).Month;
    Console.WriteLine($"Selected month: {selectedMonth}");
};
```

### Example 3: Year Range Selection

```csharp
// Select a range of years
calendar.View = CalendarView.Decade;
calendar.AllowViewNavigation = false;
calendar.SelectionMode = CalendarSelectionMode.Range;

calendar.SelectionChanged += (s, e) =>
{
    if (e.NewValue is CalendarDateRange range)
    {
        // User selects 2024-2026
        // range.StartDate = DateTime(2024, 1, 1)
        // range.EndDate = DateTime(2026, 12, 31)
    }
};
```

## Best Practices

1. **Default to Month View:** Most users expect to see a month calendar first
2. **Enable View Navigation:** Keep `AllowViewNavigation=true` for better UX unless you specifically need direct selection
3. **Week Numbers for Planning:** Enable week numbers for business applications and scheduling tools
4. **Adjust Visible Weeks:** Use fewer visible weeks on mobile for better screen real estate
5. **Combine with DisplayDate:** Set `DisplayDate` to show a specific time period when the calendar opens
