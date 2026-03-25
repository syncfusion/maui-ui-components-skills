# Date Formatting in .NET MAUI DatePicker

The .NET MAUI DatePicker provides extensive formatting options to display dates in various string formats. Customize the date format using the `Format` property with the `PickerDateFormat` enumeration.

## Overview

The Format property allows you to represent dates in different styles to match regional preferences, application requirements, or user preferences. The default format is `yyyy_MM_dd`.

## Available Date Formats

The DatePicker supports 20+ predefined formats through the `PickerDateFormat` enumeration:

### Day-Month Formats

**`dd_MM`** - Day and month in dd/MM format
- Example: `15/09`

**`dd_MMM`** - Day and abbreviated month in dd/MMM format
- Example: `15/Sep`

**`dd_MMMM`** - Day and full month name in dd/MMMM format
- Example: `15/September`

**`MM_dd`** - Month and day in MM/dd format
- Example: `09/15`

### Day-Month-Year Formats

**`dd_MM_yyyy`** - Day, month, year in dd/MM/yyyy format
- Example: `15/09/2023`

**`dd_MMM_yyyy`** - Day, abbreviated month, year in dd/MMM/yyyy format
- Example: `15/Sep/2023`

**`dd_MMMM_yyyy`** - Day, full month name, year in dd/MMMM/yyyy format
- Example: `15/September/2023`

**`M_d_yyyy`** - Month, day, year in M/d/yyyy format
- Example: `9/15/2023`

**`MM_dd_yyyy`** - Month, day, year in MM/dd/yyyy format (US format)
- Example: `09/15/2023`

**`MMM_dd_yyyy`** - Abbreviated month, day, year in MMM/dd/yyyy format
- Example: `Sep/15/2023`

**`MMMM_dd_yyyy`** - Full month name, day, year in MMMM/dd/yyyy format
- Example: `September/15/2023`

### Month-Year Formats

**`MM_yyyy`** - Month and year in MM/yyyy format
- Example: `09/2023`

**`MMM_yyyy`** - Abbreviated month and year in MMM/yyyy format
- Example: `Sep/2023`

**`MMMM_yyyy`** - Full month name and year in MMMM/yyyy format
- Example: `September/2023`

### Year-Month-Day Formats

**`yyyy_MM_dd`** - Year, month, day in yyyy/MM/dd format (ISO-like)
- Example: `2023/09/15`

**`yyyy_MMM_dd`** - Year, abbreviated month, day in yyyy/MMM/dd format
- Example: `2023/Sep/15`

**`yyyy_MMMM_dd`** - Year, full month name, day in yyyy/MMMM/dd format
- Example: `2023/September/15`

### Year-Month Formats

**`yyyy_MM`** - Year and month in yyyy/MM format
- Example: `2023/09`

**`yyyy_MMM`** - Year and abbreviated month in yyyy/MMM format
- Example: `2023/Sep`

**`yyyy_MMMM`** - Year and full month name in yyyy/MMMM format
- Example: `2023/September`

### Weekday Format

**`ddd_dd_MM_YYYY`** - Weekday abbreviation, day, month, year in ddd/dd/MM/YYYY format
- Example: `Fri/15/09/2023`

### Default Format

**`Default`** - Uses the default format based on the current culture
- Example: Varies by culture settings

## Setting Format in XAML

```xml
<picker:SfDatePicker x:Name="datePicker"
                     Format="MM_dd_yyyy">
</picker:SfDatePicker>
```

## Setting Format in C#

```csharp
SfDatePicker datePicker = new SfDatePicker()
{
    Format = PickerDateFormat.MM_dd_yyyy,
};

this.Content = datePicker;
```

## Format Examples

### Example 1: US Date Format

```xml
<picker:SfDatePicker x:Name="datePicker"
                     Format="MM_dd_yyyy"
                     SelectedDate="9/15/2023">
    <picker:SfDatePicker.HeaderView>
        <picker:PickerHeaderView Text="Select Date (MM/dd/yyyy)" Height="40" />
    </picker:SfDatePicker.HeaderView>
</picker:SfDatePicker>
```

**Result:** Displays as `09/15/2023`

### Example 2: European Date Format

```xml
<picker:SfDatePicker x:Name="datePicker"
                     Format="dd_MM_yyyy"
                     SelectedDate="15/09/2023">
    <picker:SfDatePicker.HeaderView>
        <picker:PickerHeaderView Text="Select Date (dd/MM/yyyy)" Height="40" />
    </picker:SfDatePicker.HeaderView>
</picker:SfDatePicker>
```

**Result:** Displays as `15/09/2023`

### Example 3: Long Month Name Format

```xml
<picker:SfDatePicker x:Name="datePicker"
                     Format="dd_MMMM_yyyy"
                     SelectedDate="15/09/2023">
    <picker:SfDatePicker.HeaderView>
        <picker:PickerHeaderView Text="Select Date" Height="40" />
    </picker:SfDatePicker.HeaderView>
</picker:SfDatePicker>
```

**Result:** Displays as `15/September/2023`

### Example 4: Month-Year Only

```xml
<picker:SfDatePicker x:Name="datePicker"
                     Format="MMM_yyyy"
                     SelectedDate="09/2023">
    <picker:SfDatePicker.HeaderView>
        <picker:PickerHeaderView Text="Select Month and Year" Height="40" />
    </picker:SfDatePicker.HeaderView>
</picker:SfDatePicker>
```

**Result:** Displays as `Sep/2023`

### Example 5: ISO-Like Format

```xml
<picker:SfDatePicker x:Name="datePicker"
                     Format="yyyy_MM_dd"
                     SelectedDate="2023/09/15">
    <picker:SfDatePicker.HeaderView>
        <picker:PickerHeaderView Text="Select Date (yyyy/MM/dd)" Height="40" />
    </picker:SfDatePicker.HeaderView>
</picker:SfDatePicker>
```

**Result:** Displays as `2023/09/15`

## Dynamic Format Switching

Change the format dynamically based on user preferences or application logic:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }
    
    private void OnFormatChanged(object sender, EventArgs e)
    {
        // Get selected format from picker or settings
        var selectedFormat = formatPicker.SelectedItem as string;
        
        switch (selectedFormat)
        {
            case "US Format":
                datePicker.Format = PickerDateFormat.MM_dd_yyyy;
                break;
            case "European Format":
                datePicker.Format = PickerDateFormat.dd_MM_yyyy;
                break;
            case "ISO Format":
                datePicker.Format = PickerDateFormat.yyyy_MM_dd;
                break;
            case "Long Format":
                datePicker.Format = PickerDateFormat.dd_MMMM_yyyy;
                break;
            default:
                datePicker.Format = PickerDateFormat.Default;
                break;
        }
    }
}
```

## Format with Custom Column Headers

Combine format settings with custom column headers for clarity:

```xml
<picker:SfDatePicker x:Name="datePicker"
                     Format="MMM_dd_yyyy">
    <picker:SfDatePicker.ColumnHeaderView>
        <picker:DatePickerColumnHeaderView DayHeaderText="Day"
                                           MonthHeaderText="Month"
                                           YearHeaderText="Year" />
    </picker:SfDatePicker.ColumnHeaderView>
</picker:SfDatePicker>
```

## Format Behavior Notes

### Month Display
- **M** - Displays single digit month without leading zero (1, 2, ..., 12)
- **MM** - Displays two-digit month with leading zero (01, 02, ..., 12)
- **MMM** - Displays abbreviated month name (Jan, Feb, ..., Dec)
- **MMMM** - Displays full month name (January, February, ..., December)

### Day Display
- **d** - Displays single digit day without leading zero (1, 2, ..., 31)
- **dd** - Displays two-digit day with leading zero (01, 02, ..., 31)
- **ddd** - Displays abbreviated weekday name (Mon, Tue, ..., Sun)

### Year Display
- **yy** - Displays two-digit year (23 for 2023)
- **yyyy** - Displays four-digit year (2023)

## Best Practices

### 1. Match User's Region
Choose formats that match the user's expected date format based on their locale:
- **US users:** `MM_dd_yyyy`
- **European users:** `dd_MM_yyyy`
- **ISO format:** `yyyy_MM_dd` (universal)

### 2. Provide Clear Headers
When using less common formats, provide clear header text:

```xml
<picker:PickerHeaderView Text="Select Date (dd/MM/yyyy)" Height="40" />
```

### 3. Consider Input Context
- **Birth dates:** Use full year format (`yyyy`)
- **Expiry dates:** Month-year format (`MM_yyyy`) is often sufficient
- **Appointments:** Include weekday for clarity (`ddd_dd_MM_YYYY`)

### 4. Test with Different Locales
Test your format choices with different culture settings to ensure proper display.

## Common Use Cases

### Birth Date Selection
```xml
<picker:SfDatePicker Format="dd_MMMM_yyyy"
                     MaximumDate="{x:Static sys:DateTime.Now}">
    <picker:SfDatePicker.HeaderView>
        <picker:PickerHeaderView Text="Select Birth Date" Height="40" />
    </picker:SfDatePicker.HeaderView>
</picker:SfDatePicker>
```

### Credit Card Expiry
```xml
<picker:SfDatePicker Format="MM_yyyy"
                     MinimumDate="{x:Static sys:DateTime.Now}">
    <picker:SfDatePicker.HeaderView>
        <picker:PickerHeaderView Text="Expiry Date" Height="40" />
    </picker:SfDatePicker.HeaderView>
</picker:SfDatePicker>
```

### Appointment Scheduling
```xml
<picker:SfDatePicker Format="ddd_dd_MM_YYYY"
                     MinimumDate="{x:Static sys:DateTime.Now}">
    <picker:SfDatePicker.HeaderView>
        <picker:PickerHeaderView Text="Select Appointment Date" Height="40" />
    </picker:SfDatePicker.HeaderView>
</picker:SfDatePicker>
```

## Troubleshooting

### Issue: Format not displaying correctly
**Solution:** Ensure you're using the correct `PickerDateFormat` enum value, not a string.

**Wrong:**
```csharp
datePicker.Format = "MM_dd_yyyy"; // This won't work
```

**Correct:**
```csharp
datePicker.Format = PickerDateFormat.MM_dd_yyyy;
```

### Issue: Month names not localized
**Solution:** Month names automatically use the current culture. Set `CurrentUICulture` in App.xaml.cs for localization.

## Related Topics

- **Localization** - Learn how to localize month and day names
- **Customization** - Customize the appearance of date columns
- **Date Restrictions** - Combine formats with date range restrictions
