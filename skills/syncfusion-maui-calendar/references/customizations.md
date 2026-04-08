# Customization in .NET MAUI Calendar

The SfCalendar provides extensive customization options for cells, text styles, backgrounds, and selection appearances across all views. This comprehensive guide covers all customization capabilities.

## Table of Contents
- [Month Cell Customization](#month-cell-customization)
- [Special Date Highlighting](#special-date-highlighting)
- [Year, Decade, and Century View Customization](#year-decade-and-century-view-customization)
- [Selection Cell Customization](#selection-cell-customization)
- [CalendarTextStyle Properties](#calendartextstyle-properties)
- [Customization Priority Order](#customization-priority-order)

## Month Cell Customization

Customize all aspects of month view cells using the `MonthView` property.

### Month Dates (Regular Dates)

Style normal dates using `TextStyle` and `Background` properties.

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar" View="Month" Background="LightBlue">
    <calendar:SfCalendar.MonthView>
        <calendar:CalendarMonthView Background="White">
            <calendar:CalendarMonthView.TextStyle>
                <calendar:CalendarTextStyle TextColor="Black" 
                                           FontSize="14"
                                           FontFamily="Arial" />
            </calendar:CalendarMonthView.TextStyle>
        </calendar:CalendarMonthView>
    </calendar:SfCalendar.MonthView>
</calendar:SfCalendar>
```

**C#:**
```csharp
CalendarTextStyle textStyle = new CalendarTextStyle
{
    TextColor = Colors.Black,
    FontSize = 14,
    FontFamily = "Arial"
};

calendar.MonthView = new CalendarMonthView
{
    Background = Colors.White,
    TextStyle = textStyle
};
```

### Today Date

Highlight the current date with custom styling.

**XAML:**
```xml
<calendar:SfCalendar.MonthView>
    <calendar:CalendarMonthView TodayBackground="Pink">
        <calendar:CalendarMonthView.TodayTextStyle>
            <calendar:CalendarTextStyle TextColor="White" 
                                       FontSize="16"
                                       FontAttributes="Bold" />
        </calendar:CalendarMonthView.TodayTextStyle>
    </calendar:CalendarMonthView>
</calendar:SfCalendar.MonthView>
```

**C#:**
```csharp
calendar.MonthView = new CalendarMonthView
{
    TodayBackground = Colors.Pink,
    TodayTextStyle = new CalendarTextStyle
    {
        TextColor = Colors.White,
        FontSize = 16,
        FontAttributes = FontAttributes.Bold
    }
};
```

### Trailing and Leading Dates

Trailing dates are from the previous month; leading dates are from the next month.

**Show/Hide Trailing and Leading Dates:**
```csharp
// Hide trailing and leading dates
calendar.ShowTrailingAndLeadingDates = false;

// Show and customize them
calendar.ShowTrailingAndLeadingDates = true;
calendar.MonthView = new CalendarMonthView
{
    TrailingLeadingDatesBackground = Colors.LightGray.WithAlpha(0.3f),
    TrailingLeadingDatesTextStyle = new CalendarTextStyle
    {
        TextColor = Colors.Gray,
        FontSize = 12
    }
};
```

### Disabled Dates

Style dates that are disabled due to restrictions (MinimumDate, MaximumDate, EnablePastDates, SelectableDayPredicate).

**XAML:**
```xml
<calendar:SfCalendar.MonthView>
    <calendar:CalendarMonthView DisabledDatesBackground="LightGray">
        <calendar:CalendarMonthView.DisabledDatesTextStyle>
            <calendar:CalendarTextStyle TextColor="DarkGray" 
                                       FontSize="12" />
        </calendar:CalendarMonthView.DisabledDatesTextStyle>
    </calendar:CalendarMonthView>
</calendar:SfCalendar.MonthView>
```

**C#:**
```csharp
calendar.MonthView = new CalendarMonthView
{
    DisabledDatesBackground = Colors.LightGray,
    DisabledDatesTextStyle = new CalendarTextStyle
    {
        TextColor = Colors.DarkGray,
        FontSize = 12
    }
};
```

### Weekend Dates

Customize weekends (Saturday and Sunday by default).

**XAML:**
```xml
<calendar:SfCalendar.MonthView>
    <calendar:CalendarMonthView WeekendDatesBackground="#E2F9F3">
        <calendar:CalendarMonthView.WeekendDatesTextStyle>
            <calendar:CalendarTextStyle TextColor="Green" 
                                       FontSize="14" />
        </calendar:CalendarMonthView.WeekendDatesTextStyle>
    </calendar:CalendarMonthView>
</calendar:SfCalendar.MonthView>
```

**C#:**
```csharp
calendar.MonthView = new CalendarMonthView
{
    WeekendDays = new List<DayOfWeek>
    {
        DayOfWeek.Saturday,
        DayOfWeek.Sunday
    },
    WeekendDatesBackground = Color.FromArgb("#E2F9F3"),
    WeekendDatesTextStyle = new CalendarTextStyle
    {
        TextColor = Colors.Green,
        FontSize = 14
    }
};
```

**Custom Weekend Days:**
```csharp
// Set Friday and Saturday as weekends (Middle East)
calendar.MonthView.WeekendDays = new List<DayOfWeek>
{
    DayOfWeek.Friday,
    DayOfWeek.Saturday
};
```

### Complete Month View Customization Example

```csharp
CalendarTextStyle regularTextStyle = new CalendarTextStyle
{
    TextColor = Colors.Black,
    FontSize = 14
};

calendar.MinimumDate = DateTime.Now.AddDays(-15);
calendar.MaximumDate = DateTime.Now.AddDays(20);
calendar.Background = Colors.PaleGreen.WithAlpha(0.3f);
calendar.ShowTrailingAndLeadingDates = true;

calendar.MonthView = new CalendarMonthView
{
    WeekendDays = new List<DayOfWeek> { DayOfWeek.Sunday, DayOfWeek.Saturday },
    TextStyle = regularTextStyle,
    TodayBackground = Colors.Pink,
    TodayTextStyle = new CalendarTextStyle { TextColor = Colors.Black, FontSize = 14 },
    DisabledDatesBackground = Colors.Gray.WithAlpha(0.3f),
    DisabledDatesTextStyle = new CalendarTextStyle { TextColor = Colors.Gray, FontSize = 12 },
    TrailingLeadingDatesBackground = Colors.Red.WithAlpha(0.3f),
    TrailingLeadingDatesTextStyle = new CalendarTextStyle { TextColor = Colors.Red, FontSize = 12 },
    WeekendDatesBackground = Color.FromArgb("#E2F9F3"),
    WeekendDatesTextStyle = new CalendarTextStyle { TextColor = Colors.Green, FontSize = 14 }
};
```

## Special Date Highlighting

Use `SpecialDayPredicate` to highlight important dates with custom styling and icons.

### Basic Special Dates

```csharp
// Highlight specific dates
private List<DateTime> specialDates = new List<DateTime>
{
    new DateTime(2026, 12, 25), // Christmas
    new DateTime(2026, 1, 1),   // New Year
    new DateTime(2026, 7, 4)    // Independence Day
};

calendar.MonthView = new CalendarMonthView
{
    SpecialDayPredicate = (DateTime date) =>
    {
        if (specialDates.Contains(date.Date))
        {
            return new CalendarIconDetails
            {
                Icon = CalendarIcon.Dot,
                Fill = Colors.Red
            };
        }
        return null;
    },
    SpecialDatesBackground = Color.FromArgb("#FFEFD2"),
    SpecialDatesTextStyle = new CalendarTextStyle
    {
        TextColor = Colors.DarkRed,
        FontAttributes = FontAttributes.Bold
    }
};
```

### Special Date Icons

The `CalendarIcon` enum provides several icon options:

```csharp
public enum CalendarIcon
{
    Dot,   // • Dot/circle
    Triangle, // ▲ Triangle
    Square,   // ■ Square
    Heart,    // ♥ Heart
    Diamond,  // ♦ Diamond
    Bell,     // 🔔 Bell
    Star      // ★ Star
}
```

### Multiple Icons Example

```csharp
calendar.MonthView.SpecialDayPredicate = (DateTime date) =>
{
    // Christmas - Red Heart
    if (date.Date == new DateTime(2026, 12, 25))
    {
        return new CalendarIconDetails
        {
            Icon = CalendarIcon.Heart,
            Fill = Colors.Red
        };
    }
    // Birthday - Yellow Star
    else if (date.Date == new DateTime(2026, 6, 15))
    {
        return new CalendarIconDetails
        {
            Icon = CalendarIcon.Star,
            Fill = Colors.Gold
        };
    }
    // Meeting - Blue Diamond
    else if (IsMeetingDate(date))
    {
        return new CalendarIconDetails
        {
            Icon = CalendarIcon.Diamond,
            Fill = Colors.Blue
        };
    }
    // Deadline - Red Bell
    else if (IsDeadline(date))
    {
        return new CalendarIconDetails
        {
            Icon = CalendarIcon.Bell,
            Fill = Colors.Red
        };
    }
    
    return null; // No special icon
};
```

### Dynamic Special Dates

Update special dates dynamically using `UpdateSpecialDayPredicate()`:

```csharp
public List<DateTime> SpecialDatesCollection = new List<DateTime>();

public MainPage()
{
    calendar.ViewChanged += Calendar_ViewChanged;
    
    calendar.MonthView.SpecialDayPredicate = (DateTime date) =>
    {
        if (SpecialDatesCollection.Contains(date.Date))
        {
            return new CalendarIconDetails
            {
                Icon = CalendarIcon.Diamond,
                Fill = Colors.Red
            };
        }
        return null;
    };
}

private async void Calendar_ViewChanged(object sender, CalendarViewChangedEventArgs e)
{
    // Clear and reload special dates for new view
    SpecialDatesCollection.Clear();
    await LoadSpecialDatesFromAPIAsync(e.NewVisibleDates);
    
    // Force re-evaluation of special dates
    calendar.UpdateSpecialDayPredicate();
}

private async Task LoadSpecialDatesFromAPIAsync(CalendarDateRange range)
{
    var httpClient = new HttpClient();
    var requestData = new { StartDate = range.StartDate, EndDate = range.EndDate };
    var response = await httpClient.PostAsJsonAsync("url", requestData);
    
    if (response.IsSuccessStatusCode)
    {
        var apiResponse = await response.Content.ReadFromJsonAsync<ApiSpecialDatesResponse>();
        if (apiResponse?.SpecialDates != null)
        {
            foreach (var dateStr in apiResponse.SpecialDates)
            {
                if (DateTime.TryParse(dateStr, out var date))
                    SpecialDatesCollection.Add(date.Date);
            }
        }
    }
}
```

## Year, Decade, and Century View Customization

Customize Year, Decade, and Century views using the `YearView` property.

### Year View Customization

**Basic Styling:**
```csharp
CalendarTextStyle yearTextStyle = new CalendarTextStyle
{
    TextColor = Colors.Black,
    FontSize = 14
};

calendar.View = CalendarView.Year;
calendar.Background = Colors.LightBlue.WithAlpha(0.3f);

calendar.YearView = new CalendarYearView
{
    Background = Colors.White,
    TextStyle = yearTextStyle,
    TodayBackground = Colors.Pink,
    TodayTextStyle = new CalendarTextStyle { TextColor = Colors.White, FontAttributes = FontAttributes.Bold },
    DisabledDatesBackground = Colors.Gray.WithAlpha(0.3f),
    DisabledDatesTextStyle = new CalendarTextStyle { TextColor = Colors.Gray },
    LeadingDatesBackground = Colors.Red.WithAlpha(0.3f),
    LeadingDatesTextStyle = new CalendarTextStyle { TextColor = Colors.Red }
};
```

### Decade View Customization

```csharp
calendar.View = CalendarView.Decade;
calendar.MinimumDate = DateTime.Now.AddYears(-1);
calendar.MaximumDate = DateTime.Now.AddYears(8);
calendar.ShowTrailingAndLeadingDates = true;

calendar.YearView = new CalendarYearView
{
    TodayBackground = Colors.Pink,
    DisabledDatesBackground = Colors.Gray.WithAlpha(0.3f),
    LeadingDatesBackground = Colors.Red.WithAlpha(0.3f)
};
```

### Year View Month Format

Customize month display format in Year view:

**XAML:**
```xml
<calendar:SfCalendar View="Year">
    <calendar:SfCalendar.YearView>
        <calendar:CalendarYearView MonthFormat="MMMM" />
    </calendar:SfCalendar.YearView>
</calendar:SfCalendar>
```

**C#:**
```csharp
calendar.YearView = new CalendarYearView
{
    MonthFormat = "MMMM"  // Full month name (January, February, etc.)
    // MonthFormat = "MMM"  // Abbreviated (Jan, Feb, etc.)
    // MonthFormat = "MM"   // Numeric (01, 02, etc.)
};
```

## Selection Cell Customization

Customize the appearance of selected dates across all selection modes.

### Selection Properties

| Property | Applies To | Description |
|----------|-----------|-------------|
| `SelectionBackground` | Single, Multiple, Range (in-between) | Background color for selected dates |
| `SelectionTextStyle` | Single, Multiple, Range (start/end) | Text style for selected dates |
| `StartRangeSelectionBackground` | Range start date | Background for range start date |
| `EndRangeSelectionBackground` | Range end date | Background for range end date |
| `RangeTextStyle` | Range (in-between) | Text style for dates in range |

### Single and Multiple Selection Customization

```csharp
calendar.SelectionMode = CalendarSelectionMode.Multiple;
calendar.SelectionBackground = Colors.Pink;

calendar.MonthView.SelectionTextStyle = new CalendarTextStyle
{
    TextColor = Colors.White,
    FontSize = 14,
    FontAttributes = FontAttributes.Bold
};
```

### Range Selection Customization

```xml
<calendar:SfCalendar x:Name="calendar" 
                     SelectionMode="Range"
                     SelectionBackground="Pink"
                     StartRangeSelectionBackground="Purple"
                     EndRangeSelectionBackground="Purple">
    <calendar:SfCalendar.MonthView>
        <calendar:CalendarMonthView>
            <calendar:CalendarMonthView.SelectionTextStyle>
                <calendar:CalendarTextStyle TextColor="White" 
                                           FontAttributes="Bold" />
            </calendar:CalendarMonthView.SelectionTextStyle>
            <calendar:CalendarMonthView.RangeTextStyle>
                <calendar:CalendarTextStyle TextColor="Black" />
            </calendar:CalendarMonthView.RangeTextStyle>
        </calendar:CalendarMonthView>
    </calendar:SfCalendar.MonthView>
</calendar:SfCalendar>
```

**C#:**
```csharp
calendar.SelectionMode = CalendarSelectionMode.Range;
calendar.SelectionBackground = Colors.Pink;
calendar.StartRangeSelectionBackground = Colors.Purple;
calendar.EndRangeSelectionBackground = Colors.Purple;

calendar.MonthView.SelectionTextStyle = new CalendarTextStyle
{
    TextColor = Colors.White,
    FontAttributes = FontAttributes.Bold
};

calendar.MonthView.RangeTextStyle = new CalendarTextStyle
{
    TextColor = Colors.Black
};
```

### Complete Selection Customization Example

```csharp
// Set a default selected range
calendar.SelectedDateRange = new CalendarDateRange(
    DateTime.Now.AddDays(6),
    DateTime.Now.AddDays(17)
);

calendar.SelectionMode = CalendarSelectionMode.Range;
calendar.StartRangeSelectionBackground = Colors.Purple;
calendar.EndRangeSelectionBackground = Colors.Purple;
calendar.SelectionBackground = Colors.Pink;

calendar.MonthView.SelectionTextStyle = new CalendarTextStyle
{
    TextColor = Colors.White,
    FontSize = 14,
    FontAttributes = FontAttributes.Bold
};

calendar.MonthView.RangeTextStyle = new CalendarTextStyle
{
    TextColor = Colors.Black,
    FontSize = 14
};
```

## CalendarTextStyle Properties

The `CalendarTextStyle` class provides comprehensive text styling options.

### Available Properties

```csharp
public class CalendarTextStyle
{
    public Color TextColor { get; set; }
    public double FontSize { get; set; }
    public string FontFamily { get; set; }
    public FontAttributes FontAttributes { get; set; }  // None, Bold, Italic, Bold | Italic
    public bool FontAutoScalingEnabled { get; set; }
}
```

### Example Usage

```csharp
CalendarTextStyle customStyle = new CalendarTextStyle
{
    TextColor = Colors.DarkBlue,
    FontSize = 16,
    FontFamily = "Arial",
    FontAttributes = FontAttributes.Bold | FontAttributes.Italic
};

calendar.MonthView.TextStyle = customStyle;
```

## Customization Priority Order

When multiple customization properties apply to the same date, they are evaluated in this order (highest to lowest priority):

### Month View Priority

1. **SelectableDayPredicate dates** (disabled by predicate)
2. **Special dates** (SpecialDayPredicate)
3. **Disabled dates** (MinimumDate, MaximumDate, EnablePastDates)
4. **Today date**
5. **Weekend dates**
6. **Trailing and leading dates**
7. **Normal dates** (default TextStyle and Background)

### Year View Priority

1. **SelectableDayPredicate dates** (disabled)
2. **Disabled dates**
3. **Today date**
4. **Leading dates**
5. **Normal dates**

### Example of Priority

```csharp
// Date is both a weekend AND a special date
// Special date styling takes precedence over weekend styling
calendar.MonthView = new CalendarMonthView
{
    WeekendDatesBackground = Colors.LightBlue,  // Won't apply if date is special
    SpecialDayPredicate = (date) =>
    {
        if (date.DayOfWeek == DayOfWeek.Saturday && date.Day == 25)
        {
            return new CalendarIconDetails
            {
                Icon = CalendarIcon.Heart,
                Fill = Colors.Red
            };
        }
        return null;
    },
    SpecialDatesBackground = Colors.Yellow  // This will be applied
};
```

## Best Practices

1. **Use Consistent Styling:** Maintain consistent text styles and colors across all date types
2. **Ensure Readability:** Choose contrasting text colors against backgrounds
3. **Optimize Predicates:** Keep SpecialDayPredicate fast; it's called frequently
4. **Test on Different Platforms:** Colors may render differently on iOS, Android, and Windows
5. **Consider Accessibility:** Ensure sufficient color contrast and don't rely solely on color
6. **Use Alpha for Subtle Effects:** Use `.WithAlpha()` for softer background colors
7. **Update Dynamically:** Use `UpdateSpecialDayPredicate()` when data changes
