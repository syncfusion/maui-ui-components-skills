# Intervals and Looping in .NET MAUI DatePicker

The .NET MAUI DatePicker provides interval and looping features to customize date selection behavior and create specialized date pickers for specific use cases.

## Date Intervals

Date intervals allow you to specify gaps between selectable values in the Day, Month, and Year columns. This is useful for creating pickers that show only specific date increments.

### Day Interval

The `DayInterval` property sets the interval between day values. For example, an interval of 2 shows only odd or even days.

#### XAML

```xml
<picker:SfDatePicker x:Name="datePicker"
                     DayInterval="2">
</picker:SfDatePicker>
```

#### C#

```csharp
SfDatePicker datePicker = new SfDatePicker()
{
    DayInterval = 2
};

this.Content = datePicker;
```

**Result:** Displays days as 1, 3, 5, 7, 9, 11, 13, 15, 17, 19, 21, 23, 25, 27, 29, 31

#### Example: Weekly Selection (Every 7 Days)

```xml
<picker:SfDatePicker x:Name="datePicker"
                     DayInterval="7">
    <picker:SfDatePicker.HeaderView>
        <picker:PickerHeaderView Text="Select Weekly Date" Height="40" />
    </picker:SfDatePicker.HeaderView>
</picker:SfDatePicker>
```

**Use Case:** Selecting weekly meeting dates, weekly report dates

### Month Interval

The `MonthInterval` property sets the interval between month values.

#### XAML

```xml
<picker:SfDatePicker x:Name="datePicker"
                     MonthInterval="2">
</picker:SfDatePicker>
```

#### C#

```csharp
SfDatePicker datePicker = new SfDatePicker()
{
    MonthInterval = 2
};

this.Content = datePicker;
```

**Result:** Displays months as Jan, Mar, May, Jul, Sep, Nov

#### Example: Quarterly Selection

```xml
<picker:SfDatePicker x:Name="datePicker"
                     MonthInterval="3"
                     Format="MMM_yyyy">
    <picker:SfDatePicker.HeaderView>
        <picker:PickerHeaderView Text="Select Quarter" Height="40" />
    </picker:SfDatePicker.HeaderView>
</picker:SfDatePicker>
```

**Use Case:** Quarterly reports, quarterly reviews, seasonal planning

#### Example: Bi-Annual Selection

```xml
<picker:SfDatePicker x:Name="datePicker"
                     MonthInterval="6"
                     Format="MMM_yyyy">
    <picker:SfDatePicker.HeaderView>
        <picker:PickerHeaderView Text="Select Semi-Annual Period" Height="40" />
    </picker:SfDatePicker.HeaderView>
</picker:SfDatePicker>
```

**Result:** Shows Jan, Jul or Feb, Aug depending on the selected date

**Use Case:** Semi-annual reviews, biannual subscriptions

### Year Interval

The `YearInterval` property sets the interval between year values.

#### XAML

```xml
<picker:SfDatePicker x:Name="datePicker"
                     YearInterval="2">
</picker:SfDatePicker>
```

#### C#

```csharp
SfDatePicker datePicker = new SfDatePicker()
{
    YearInterval = 2
};

this.Content = datePicker;
```

**Result:** Displays years as 2020, 2022, 2024, 2026, etc.

#### Example: Olympic Years (Every 4 Years)

```xml
<picker:SfDatePicker x:Name="datePicker"
                     YearInterval="4"
                     Format="yyyy"
                     MinimumDate="2000/01/01"
                     MaximumDate="2040/12/31">
    <picker:SfDatePicker.HeaderView>
        <picker:PickerHeaderView Text="Select Olympic Year" Height="40" />
    </picker:SfDatePicker.HeaderView>
</picker:SfDatePicker>
```

**Use Case:** Recurring multi-year events

### Combined Intervals

You can combine day, month, and year intervals for specialized date pickers.

#### Example: Bi-Weekly, Quarterly Selection

```xml
<picker:SfDatePicker x:Name="datePicker"
                     DayInterval="14"
                     MonthInterval="3"
                     YearInterval="1">
    <picker:SfDatePicker.HeaderView>
        <picker:PickerHeaderView Text="Select Date" Height="40" />
    </picker:SfDatePicker.HeaderView>
</picker:SfDatePicker>
```

#### Example: Custom Business Cycle

```csharp
// Every 5 days, every 2 months, every 2 years
datePicker.DayInterval = 5;
datePicker.MonthInterval = 2;
datePicker.YearInterval = 2;
```

## Looping Support

The `EnableLooping` property allows seamless navigation in the date picker. When enabled, scrolling past the last item loops back to the first item, and vice versa.

### Enabling Looping

#### XAML

```xml
<picker:SfDatePicker x:Name="datePicker"
                     EnableLooping="True">
</picker:SfDatePicker>
```

#### C#

```csharp
SfDatePicker datePicker = new SfDatePicker()
{
    EnableLooping = true
};

this.Content = datePicker;
```

**Default Value:** `false`

### Behavior with Looping Enabled

**Without Looping:**
- Day column: 1, 2, 3, ..., 30, 31 (stops)
- Month column: Jan, Feb, ..., Nov, Dec (stops)
- Year column: 2000, 2001, ..., 2049, 2050 (stops)

**With Looping:**
- Day column: ..., 30, 31, 1, 2, 3, ... (continuous)
- Month column: ..., Nov, Dec, Jan, Feb, ... (continuous)
- Year column: ..., 2049, 2050, 2000, 2001, ... (continuous)

### Example: Looping with Date Range

```xml
<picker:SfDatePicker x:Name="datePicker"
                     EnableLooping="True"
                     MinimumDate="2020/01/01"
                     MaximumDate="2025/12/31">
    <picker:SfDatePicker.HeaderView>
        <picker:PickerHeaderView Text="Select Date" Height="40" />
    </picker:SfDatePicker.HeaderView>
</picker:SfDatePicker>
```

When scrolling past December 2025, it loops back to January 2020.

## Complete Examples

### Example 1: Bi-Weekly Appointment Scheduler

```xml
<StackLayout Padding="20" Spacing="15">
    <Label Text="Schedule Bi-Weekly Appointment" 
           FontSize="20" 
           FontAttributes="Bold"/>
    
    <Label Text="Appointments available every 2 weeks starting Monday" 
           FontSize="14" 
           TextColor="Gray"/>
    
    <picker:SfDatePicker x:Name="appointmentPicker"
                         DayInterval="14"
                         EnableLooping="True"
                         MinimumDate="2023/09/04"
                         SelectionChanged="OnAppointmentSelected">
        <picker:SfDatePicker.HeaderView>
            <picker:PickerHeaderView Text="Select Appointment Date" Height="40" />
        </picker:SfDatePicker.HeaderView>
        <picker:SfDatePicker.FooterView>
            <picker:PickerFooterView ShowOkButton="True" Height="40" />
        </picker:SfDatePicker.FooterView>
    </picker:SfDatePicker>
    
    <Label x:Name="selectedDateLabel" 
           FontSize="16"/>
</StackLayout>
```

```csharp
private void OnAppointmentSelected(object sender, DatePickerSelectionChangedEventArgs e)
{
    if (e.NewValue != null)
    {
        selectedDateLabel.Text = $"Appointment: {e.NewValue:dddd, MMMM dd, yyyy}";
    }
}
```

### Example 2: Quarterly Report Selection

```xml
<StackLayout Padding="20" Spacing="15">
    <Label Text="Select Reporting Quarter" 
           FontSize="20" 
           FontAttributes="Bold"/>
    
    <picker:SfDatePicker x:Name="quarterPicker"
                         MonthInterval="3"
                         Format="MMM_yyyy"
                         EnableLooping="True"
                         SelectionChanged="OnQuarterSelected">
        <picker:SfDatePicker.ColumnHeaderView>
            <picker:DatePickerColumnHeaderView MonthHeaderText="Quarter"
                                               YearHeaderText="Year"/>
        </picker:SfDatePicker.ColumnHeaderView>
        <picker:SfDatePicker.HeaderView>
            <picker:PickerHeaderView Text="Select Quarter" Height="40" />
        </picker:SfDatePicker.HeaderView>
    </picker:SfDatePicker>
    
    <Label x:Name="quarterLabel" 
           FontSize="16"/>
</StackLayout>
```

```csharp
private void OnQuarterSelected(object sender, DatePickerSelectionChangedEventArgs e)
{
    if (e.NewValue != null)
    {
        int quarter = ((e.NewValue.Value.Month - 1) / 3) + 1;
        quarterLabel.Text = $"Q{quarter} {e.NewValue.Value.Year}";
    }
}
```

### Example 3: Fiscal Year Selector (Every 2 Years)

```xml
<StackLayout Padding="20" Spacing="15">
    <Label Text="Select Fiscal Year" 
           FontSize="20" 
           FontAttributes="Bold"/>
    
    <Label Text="Fiscal years occur every 2 years" 
           FontSize="14" 
           TextColor="Gray"/>
    
    <picker:SfDatePicker x:Name="fiscalYearPicker"
                         YearInterval="2"
                         Format="yyyy_MM"
                         EnableLooping="True"
                         MinimumDate="2020/01/01"
                         MaximumDate="2040/12/31"
                         SelectionChanged="OnFiscalYearSelected">
        <picker:SfDatePicker.HeaderView>
            <picker:PickerHeaderView Text="Select Fiscal Year" Height="40" />
        </picker:SfDatePicker.HeaderView>
    </picker:SfDatePicker>
    
    <Label x:Name="fiscalYearLabel" 
           FontSize="16"/>
</StackLayout>
```

```csharp
private void OnFiscalYearSelected(object sender, DatePickerSelectionChangedEventArgs e)
{
    if (e.NewValue != null)
    {
        fiscalYearLabel.Text = $"Fiscal Year: {e.NewValue.Value.Year}";
    }
}
```

### Example 4: Weekly Meeting Scheduler with Looping

```xml
<StackLayout Padding="20" Spacing="15">
    <Label Text="Schedule Weekly Team Meeting" 
           FontSize="20" 
           FontAttributes="Bold"/>
    
    <picker:SfDatePicker x:Name="weeklyMeetingPicker"
                         DayInterval="7"
                         EnableLooping="True"
                         MinimumDate="2023/09/04"
                         MaximumDate="2024/12/31"
                         Format="ddd_dd_MM_YYYY">
        <picker:SfDatePicker.HeaderView>
            <picker:PickerHeaderView Text="Select Meeting Date (Mondays)" Height="40" />
        </picker:SfDatePicker.HeaderView>
        <picker:SfDatePicker.FooterView>
            <picker:PickerFooterView ShowOkButton="True" Height="40" />
        </picker:SfDatePicker.FooterView>
    </picker:SfDatePicker>
</StackLayout>
```

### Example 5: Medication Reminder (Every 3 Days)

```csharp
public class MedicationReminderViewModel
{
    public SfDatePicker MedicationPicker { get; set; }
    
    public void Initialize()
    {
        MedicationPicker = new SfDatePicker
        {
            DayInterval = 3,
            EnableLooping = true,
            MinimumDate = DateTime.Now.Date,
            MaximumDate = DateTime.Now.AddMonths(6).Date
        };
        
        MedicationPicker.HeaderView = new PickerHeaderView
        {
            Text = "Select Next Dose Date",
            Height = 40
        };
        
        MedicationPicker.SelectionChanged += OnMedicationDateSelected;
    }
    
    private void OnMedicationDateSelected(object sender, DatePickerSelectionChangedEventArgs e)
    {
        if (e.NewValue != null)
        {
            // Set reminder for next dose
            SetMedicationReminder(e.NewValue.Value);
        }
    }
    
    private void SetMedicationReminder(DateTime doseDate)
    {
        // Implementation for setting reminder
    }
}
```

## Use Cases

### Day Intervals
- **2 days:** Alternate day schedules
- **3 days:** Tri-daily activities
- **5 days:** Weekday-only selections
- **7 days:** Weekly schedules
- **14 days:** Bi-weekly appointments
- **15 days:** Semi-monthly payments

### Month Intervals
- **2 months:** Bi-monthly reports
- **3 months:** Quarterly reviews, seasonal planning
- **6 months:** Semi-annual checkups, biannual subscriptions
- **12 months:** Annual events (though this would show only one month)

### Year Intervals
- **2 years:** Biennial events
- **4 years:** Olympic years, quadrennial elections
- **5 years:** Quinquennial surveys
- **10 years:** Decennial census

### Looping
- **Infinite scrolling:** Better user experience for continuous date ranges
- **Compact ranges:** When working with limited date ranges where looping makes navigation easier
- **Repeating schedules:** Recurring events that cycle through dates

## Best Practices

1. **Clear Communication:** When using intervals, clearly indicate to users which dates are available

2. **Appropriate Intervals:** Choose intervals that match your business logic:
   - Don't use arbitrary intervals that confuse users
   - Match intervals to real-world patterns (weeks, quarters, etc.)

3. **Combine with Date Restrictions:** Use intervals with MinimumDate and MaximumDate for controlled selection

4. **Test User Experience:** Test looping behavior to ensure it provides a good user experience

5. **Format Appropriately:** Use date formats that match the interval granularity:
   - Weekly intervals: Include day name (ddd_dd_MM_YYYY)
   - Monthly intervals: Month-year format (MMM_yyyy)
   - Yearly intervals: Year only (yyyy)

6. **Document Behavior:** Clearly document the interval behavior in your UI or help text

## Performance Considerations

- **Large Intervals:** Using large intervals (e.g., YearInterval="10") significantly reduces the number of items in the picker, improving performance

- **Looping with Large Ranges:** Be cautious when combining looping with very large date ranges as it may create usability issues

## Troubleshooting

### Issue: Intervals not working as expected
**Solution:** Ensure interval values are positive integers. Interval values of 0 or negative numbers may cause unexpected behavior.

### Issue: Looping doesn't work
**Solution:** Verify that `EnableLooping="True"` is set. Check that there's a sufficient date range for looping to be meaningful.

### Issue: Desired dates not appearing
**Solution:** Check your interval settings. For example, with DayInterval="7" starting from day 1, you'll see days 1, 8, 15, 22, 29. Adjust the MinimumDate to get the desired starting point.

### Issue: Looping causes confusion
**Solution:** Consider disabling looping if users find the circular navigation confusing. Provide clear visual feedback about the date range.

## Related Topics

- **Date Restrictions** - Combine intervals with date restrictions for precise control
- **Formatting** - Use appropriate formats with intervals
- **Customization** - Customize column headers to clarify interval meanings
