# Date Restrictions in .NET MAUI DatePicker

The .NET MAUI DatePicker provides comprehensive date restriction capabilities to control which dates users can select.

## Overview

Date restrictions help enforce business rules, validation requirements, and logical constraints in date selection scenarios such as:
- Preventing selection of past dates for future events
- Limiting date ranges for historical data entry
- Blocking specific dates (holidays, maintenance days, fully booked dates)
- Enforcing minimum age requirements

## Minimum Date

The `MinimumDate` property restricts selection to dates on or after the specified date. Users cannot select dates before the minimum date.

### Setting Minimum Date

#### XAML

```xml
<picker:SfDatePicker x:Name="datePicker"
                     MinimumDate="2020/01/01">
</picker:SfDatePicker>
```

#### C#

```csharp
SfDatePicker datePicker = new SfDatePicker();
datePicker.MinimumDate = new DateTime(2020, 1, 1);
this.Content = datePicker;
```

### Example: Prevent Past Date Selection

```xml
<picker:SfDatePicker x:Name="datePicker"
                     MinimumDate="{x:Static sys:DateTime.Now}">
    <picker:SfDatePicker.HeaderView>
        <picker:PickerHeaderView Text="Select Future Date" Height="40" />
    </picker:SfDatePicker.HeaderView>
</picker:SfDatePicker>
```

```csharp
datePicker.MinimumDate = DateTime.Now.Date;
```

### Example: Minimum Age Requirement (18 years)

```csharp
// User must be at least 18 years old
DateTime eighteenYearsAgo = DateTime.Now.AddYears(-18);
datePicker.MaximumDate = eighteenYearsAgo;
```

```xml
<picker:SfDatePicker x:Name="birthDatePicker">
    <picker:SfDatePicker.HeaderView>
        <picker:PickerHeaderView Text="Select Birth Date (18+ only)" Height="40" />
    </picker:SfDatePicker.HeaderView>
</picker:SfDatePicker>
```

```csharp
// In code-behind
birthDatePicker.MaximumDate = DateTime.Now.AddYears(-18).Date;
```

## Maximum Date

The `MaximumDate` property restricts selection to dates on or before the specified date. Users cannot select dates after the maximum date.

### Setting Maximum Date

#### XAML

```xml
<picker:SfDatePicker x:Name="datePicker"
                     MaximumDate="2030/12/31">
</picker:SfDatePicker>
```

#### C#

```csharp
SfDatePicker datePicker = new SfDatePicker();
datePicker.MaximumDate = new DateTime(2030, 12, 31);
this.Content = datePicker;
```

### Example: Historical Data Entry

```xml
<picker:SfDatePicker x:Name="datePicker"
                     MaximumDate="{x:Static sys:DateTime.Now}">
    <picker:SfDatePicker.HeaderView>
        <picker:PickerHeaderView Text="Select Past Date" Height="40" />
    </picker:SfDatePicker.HeaderView>
</picker:SfDatePicker>
```

```csharp
datePicker.MaximumDate = DateTime.Now.Date;
```

## Date Range Restriction

Combine `MinimumDate` and `MaximumDate` to create a specific date range.

### Example: Booking Window (7-30 days from now)

```xml
<picker:SfDatePicker x:Name="bookingDatePicker">
    <picker:SfDatePicker.HeaderView>
        <picker:PickerHeaderView Text="Select Booking Date" Height="40" />
    </picker:SfDatePicker.HeaderView>
</picker:SfDatePicker>
```

```csharp
// Allow bookings between 7 and 30 days from now
bookingDatePicker.MinimumDate = DateTime.Now.AddDays(7).Date;
bookingDatePicker.MaximumDate = DateTime.Now.AddDays(30).Date;
```

### Example: Current Year Only

```xml
<picker:SfDatePicker x:Name="datePicker"
                     MinimumDate="2023/01/01"
                     MaximumDate="2023/12/31">
    <picker:SfDatePicker.HeaderView>
        <picker:PickerHeaderView Text="Select Date (2023 Only)" Height="40" />
    </picker:SfDatePicker.HeaderView>
</picker:SfDatePicker>
```

```csharp
// Or dynamically set to current year
int currentYear = DateTime.Now.Year;
datePicker.MinimumDate = new DateTime(currentYear, 1, 1);
datePicker.MaximumDate = new DateTime(currentYear, 12, 31);
```

### Example: Last 90 Days

```csharp
datePicker.MinimumDate = DateTime.Now.AddDays(-90).Date;
datePicker.MaximumDate = DateTime.Now.Date;
```

## Blackout Dates

The `BlackoutDates` property allows you to specify a collection of specific dates that cannot be selected. This is useful for blocking holidays, maintenance days, or already booked dates.

### Setting Blackout Dates

#### XAML

```xml
<picker:SfDatePicker x:Name="datePicker">
    <picker:SfDatePicker.BlackoutDates>
        <x:DateTime>2023-09-10</x:DateTime>
        <x:DateTime>2023-09-15</x:DateTime>
        <x:DateTime>2023-09-20</x:DateTime>
        <x:DateTime>2023-09-25</x:DateTime>
    </picker:SfDatePicker.BlackoutDates>
</picker:SfDatePicker>
```

#### C#

```csharp
SfDatePicker datePicker = new SfDatePicker();
datePicker.BlackoutDates.Add(new DateTime(2023, 9, 10));
datePicker.BlackoutDates.Add(new DateTime(2023, 9, 15));
datePicker.BlackoutDates.Add(new DateTime(2023, 9, 20));
datePicker.BlackoutDates.Add(new DateTime(2023, 9, 25));
this.Content = datePicker;
```

**Important Note:** The Selection View is not applicable when blackout dates are set.

### Example: Block Weekends

```csharp
public void BlockWeekends(SfDatePicker datePicker, DateTime startDate, DateTime endDate)
{
    for (DateTime date = startDate; date <= endDate; date = date.AddDays(1))
    {
        if (date.DayOfWeek == DayOfWeek.Saturday || date.DayOfWeek == DayOfWeek.Sunday)
        {
            datePicker.BlackoutDates.Add(date);
        }
    }
}

// Usage
BlockWeekends(datePicker, DateTime.Now, DateTime.Now.AddMonths(1));
```

### Example: Block Holidays

```csharp
public void BlockHolidays(SfDatePicker datePicker)
{
    // US Federal Holidays 2023
    datePicker.BlackoutDates.Add(new DateTime(2023, 1, 1));   // New Year's Day
    datePicker.BlackoutDates.Add(new DateTime(2023, 7, 4));   // Independence Day
    datePicker.BlackoutDates.Add(new DateTime(2023, 12, 25)); // Christmas Day
    // Add more holidays as needed
}
```

### Example: Dynamic Blackout Dates from API

```csharp
public async Task LoadUnavailableDates(SfDatePicker datePicker)
{
    // Fetch unavailable dates from API
    var unavailableDates = await bookingService.GetUnavailableDatesAsync();
    
    datePicker.BlackoutDates.Clear();
    foreach (var date in unavailableDates)
    {
        datePicker.BlackoutDates.Add(date);
    }
}
```

## Complete Examples

### Example 1: Appointment Booking System

```xml
<StackLayout Padding="20" Spacing="15">
    <Label Text="Book Your Appointment" 
           FontSize="22" 
           FontAttributes="Bold"/>
    
    <picker:SfDatePicker x:Name="appointmentPicker"
                         SelectionChanged="OnAppointmentDateSelected">
        <picker:SfDatePicker.HeaderView>
            <picker:PickerHeaderView Text="Select Appointment Date" Height="40" />
        </picker:SfDatePicker.HeaderView>
        <picker:SfDatePicker.FooterView>
            <picker:PickerFooterView ShowOkButton="True" Height="40" />
        </picker:SfDatePicker.FooterView>
    </picker:SfDatePicker>
    
    <Label x:Name="selectedDateLabel" 
           Text="No date selected" 
           FontSize="16"/>
</StackLayout>
```

```csharp
public partial class AppointmentPage : ContentPage
{
    public AppointmentPage()
    {
        InitializeComponent();
        SetupDateRestrictions();
    }
    
    private void SetupDateRestrictions()
    {
        // Allow appointments 1-60 days from now
        appointmentPicker.MinimumDate = DateTime.Now.AddDays(1).Date;
        appointmentPicker.MaximumDate = DateTime.Now.AddDays(60).Date;
        
        // Block weekends
        BlockWeekends();
        
        // Block known holidays
        BlockHolidays();
    }
    
    private void BlockWeekends()
    {
        DateTime start = appointmentPicker.MinimumDate;
        DateTime end = appointmentPicker.MaximumDate;
        
        for (DateTime date = start; date <= end; date = date.AddDays(1))
        {
            if (date.DayOfWeek == DayOfWeek.Saturday || 
                date.DayOfWeek == DayOfWeek.Sunday)
            {
                appointmentPicker.BlackoutDates.Add(date);
            }
        }
    }
    
    private void BlockHolidays()
    {
        // Add holidays within the date range
        var holidays = new List<DateTime>
        {
            new DateTime(2023, 12, 25), // Christmas
            new DateTime(2024, 1, 1),   // New Year
            // Add more holidays
        };
        
        foreach (var holiday in holidays)
        {
            if (holiday >= appointmentPicker.MinimumDate && 
                holiday <= appointmentPicker.MaximumDate)
            {
                appointmentPicker.BlackoutDates.Add(holiday);
            }
        }
    }
    
    private void OnAppointmentDateSelected(object sender, DatePickerSelectionChangedEventArgs e)
    {
        if (e.NewValue != null)
        {
            selectedDateLabel.Text = $"Selected: {e.NewValue:dddd, MMMM dd, yyyy}";
        }
    }
}
```

### Example 2: Age Verification Form

```xml
<StackLayout Padding="20" Spacing="15">
    <Label Text="Enter Birth Date" 
           FontSize="18" 
           FontAttributes="Bold"/>
    
    <Label Text="You must be 18 or older" 
           FontSize="14" 
           TextColor="Gray"/>
    
    <picker:SfDatePicker x:Name="birthDatePicker"
                         SelectionChanged="OnBirthDateSelected">
        <picker:SfDatePicker.HeaderView>
            <picker:PickerHeaderView Text="Select Birth Date" Height="40" />
        </picker:SfDatePicker.HeaderView>
    </picker:SfDatePicker>
    
    <Label x:Name="ageLabel" 
           FontSize="14"/>
    
    <Button x:Name="submitButton" 
            Text="Submit" 
            IsEnabled="False"
            Clicked="OnSubmit"/>
</StackLayout>
```

```csharp
public partial class AgeVerificationPage : ContentPage
{
    public AgeVerificationPage()
    {
        InitializeComponent();
        
        // Maximum date is 18 years ago (minimum age requirement)
        birthDatePicker.MaximumDate = DateTime.Now.AddYears(-18).Date;
        
        // Minimum date is 120 years ago (reasonable maximum age)
        birthDatePicker.MinimumDate = DateTime.Now.AddYears(-120).Date;
    }
    
    private void OnBirthDateSelected(object sender, DatePickerSelectionChangedEventArgs e)
    {
        if (e.NewValue != null)
        {
            int age = CalculateAge(e.NewValue.Value);
            ageLabel.Text = $"Age: {age} years";
            submitButton.IsEnabled = age >= 18;
        }
    }
    
    private int CalculateAge(DateTime birthDate)
    {
        int age = DateTime.Now.Year - birthDate.Year;
        if (DateTime.Now < birthDate.AddYears(age))
            age--;
        return age;
    }
    
    private void OnSubmit(object sender, EventArgs e)
    {
        // Process form submission
    }
}
```

### Example 3: Travel Booking (Date Range + Blackout Dates)

```csharp
public class TravelBookingViewModel
{
    public SfDatePicker DeparturePicker { get; set; }
    public SfDatePicker ReturnPicker { get; set; }
    
    public void Initialize()
    {
        SetupDeparturePicker();
        SetupReturnPicker();
        LoadBlackoutDates();
    }
    
    private void SetupDeparturePicker()
    {
        // Can depart starting tomorrow, up to 1 year from now
        DeparturePicker.MinimumDate = DateTime.Now.AddDays(1).Date;
        DeparturePicker.MaximumDate = DateTime.Now.AddYears(1).Date;
        
        DeparturePicker.SelectionChanged += OnDepartureSelected;
    }
    
    private void SetupReturnPicker()
    {
        // Initially disabled until departure is selected
        ReturnPicker.IsEnabled = false;
    }
    
    private void OnDepartureSelected(object sender, DatePickerSelectionChangedEventArgs e)
    {
        if (e.NewValue != null)
        {
            // Return must be at least 1 day after departure
            ReturnPicker.MinimumDate = e.NewValue.Value.AddDays(1);
            ReturnPicker.MaximumDate = DeparturePicker.MaximumDate;
            ReturnPicker.IsEnabled = true;
            
            // Copy blackout dates to return picker
            ReturnPicker.BlackoutDates.Clear();
            foreach (var date in DeparturePicker.BlackoutDates)
            {
                if (date > e.NewValue.Value)
                {
                    ReturnPicker.BlackoutDates.Add(date);
                }
            }
        }
    }
    
    private async void LoadBlackoutDates()
    {
        // Load unavailable dates from service
        var unavailableDates = await travelService.GetUnavailableDatesAsync();
        
        foreach (var date in unavailableDates)
        {
            DeparturePicker.BlackoutDates.Add(date);
        }
    }
}
```

## Validation and Error Handling

### Validate Date Range

```csharp
public bool ValidateDateRange(SfDatePicker datePicker)
{
    if (datePicker.MinimumDate > datePicker.MaximumDate)
    {
        throw new ArgumentException("MinimumDate must be less than or equal to MaximumDate");
    }
    return true;
}
```

### Validate Selected Date

```csharp
private void ValidateSelectedDate(object sender, DatePickerSelectionChangedEventArgs e)
{
    if (e.NewValue != null)
    {
        if (e.NewValue < datePicker.MinimumDate || e.NewValue > datePicker.MaximumDate)
        {
            DisplayAlert("Invalid Date", 
                        $"Please select a date between {datePicker.MinimumDate:d} and {datePicker.MaximumDate:d}", 
                        "OK");
            datePicker.SelectedDate = null;
        }
    }
}
```

## Best Practices

1. **Set MinimumDate Before MaximumDate** - Always set MinimumDate before MaximumDate to avoid validation errors

2. **Provide Clear Feedback** - Display the allowed date range in headers or labels

3. **Validate Inputs** - Always validate date selections, especially when dates are set programmatically

4. **Handle Nulls** - Check for null SelectedDate values before processing

5. **Performance with Blackout Dates** - Be mindful of performance when adding many blackout dates. Consider limiting the range or loading dates on-demand.

6. **User Communication** - Clearly communicate why certain dates are unavailable

## Common Patterns

### Pattern: Future Dates Only
```csharp
datePicker.MinimumDate = DateTime.Now.Date;
```

### Pattern: Past Dates Only
```csharp
datePicker.MaximumDate = DateTime.Now.Date;
```

### Pattern: Current Month Only
```csharp
var now = DateTime.Now;
datePicker.MinimumDate = new DateTime(now.Year, now.Month, 1);
datePicker.MaximumDate = new DateTime(now.Year, now.Month, DateTime.DaysInMonth(now.Year, now.Month));
```

### Pattern: Specific Year Range
```csharp
datePicker.MinimumDate = new DateTime(2020, 1, 1);
datePicker.MaximumDate = new DateTime(2025, 12, 31);
```

## Troubleshooting

### Issue: MinimumDate/MaximumDate not working
**Solution:** Ensure MinimumDate <= MaximumDate. The MinimumDate must be less than or equal to MaximumDate.

### Issue: BlackoutDates not blocking selection
**Solution:** Verify dates are added correctly with the correct DateTime values (use `.Date` property to ignore time component).

### Issue: Selection view disappears with blackout dates
**Solution:** This is expected behavior. Selection view is not applicable when blackout dates are set.

## Related Topics

- **Formatting** - Combine restrictions with appropriate date formats
- **Events** - Handle SelectionChanged to validate date selections
- **Intervals** - Use intervals with date restrictions for specific use cases