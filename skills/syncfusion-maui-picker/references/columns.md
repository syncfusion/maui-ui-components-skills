# Dealing with Columns in .NET MAUI Picker

The Syncfusion .NET MAUI Picker supports both single-column and multi-column configurations, allowing you to create simple to complex selection interfaces.

## Table of Contents
- [Column Basics](#column-basics)
- [DisplayMemberPath](#displaymemberpath)
- [Column Customization Properties](#column-customization-properties)
- [Multi-Column Pickers](#multi-column-pickers)
- [Column Divider](#column-divider)
- [Common Scenarios](#common-scenarios)

## Column Basics

Columns are added to the picker using the `Columns` collection. Each column is represented by a `PickerColumn` object with its own data source and configuration.

### Single Column Example

```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.Columns>
        <picker:PickerColumn ItemsSource="{Binding DataSource}" />
    </picker:SfPicker.Columns>
</picker:SfPicker>
```

```csharp
ObservableCollection<string> languages = new ObservableCollection<string> 
{ 
    "Spanish", "French", "Tamil", "English", "German" 
};

picker.Columns.Add(new PickerColumn()
{
    ItemsSource = languages
});
```

## DisplayMemberPath

When binding to a collection of complex objects, use `DisplayMemberPath` to specify which property should be displayed in the picker.

### Example with Complex Objects

**Model Class:**
```csharp
public class CountryInfo
{
    public string Language { get; set; }
    public string StateName { get; set; }
}
```

**Data Source:**
```csharp
ObservableCollection<CountryInfo> countryDetails = new ObservableCollection<CountryInfo>
{
    new CountryInfo { Language = "Tamil", StateName = "Tamil Nadu" },
    new CountryInfo { Language = "Hindi", StateName = "Uttar Pradesh" },
    new CountryInfo { Language = "Bengali", StateName = "West Bengal" },
    new CountryInfo { Language = "Telugu", StateName = "Andhra Pradesh" },
    new CountryInfo { Language = "Marathi", StateName = "Maharashtra" },
    new CountryInfo { Language = "Kannada", StateName = "Karnataka" },
    new CountryInfo { Language = "Gujarati", StateName = "Gujarat" },
    new CountryInfo { Language = "Punjabi", StateName = "Punjab" },
    new CountryInfo { Language = "Odia", StateName = "Odisha" },
    new CountryInfo { Language = "Malayalam", StateName = "Kerala" },
    new CountryInfo { Language = "Assamese", StateName = "Assam" },
};
```

**Picker Configuration:**
```csharp
PickerColumn pickerColumn = new PickerColumn()
{
    DisplayMemberPath = "Language",
    HeaderText = "Select Languages",
    ItemsSource = countryDetails,
    SelectedIndex = 1,
};

picker.Columns.Add(pickerColumn);
```

**XAML Binding:**
```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.Columns>
        <picker:PickerColumn DisplayMemberPath="Language"
                            HeaderText="Select Languages"
                            ItemsSource="{Binding CountryDetails}"
                            SelectedIndex="1" />
    </picker:SfPicker.Columns>
</picker:SfPicker>
```

**Key Points:**
- `DisplayMemberPath` accesses the property name from your object
- Without `DisplayMemberPath`, the picker displays the object's `ToString()` value
- Works with any public property on your model class

## Column Customization Properties

### Width Customization

Control the width of individual columns using the `Width` property.

**Default value:** `-1` (auto-sized)

```csharp
picker.Columns[0].Width = 150;
```

```xml
<picker:PickerColumn Width="150"
                    ItemsSource="{Binding DataSource}" />
```

**Width Behavior:**
- `-1`: Column auto-sizes based on content
- Positive value: Fixed width in pixels
- Each column can have different widths

### SelectedIndex Customization

Set which item is initially selected using `SelectedIndex`.

**Default value:** `0` (first item)

```csharp
picker.Columns[0].SelectedIndex = 5;
```

```xml
<picker:PickerColumn SelectedIndex="5"
                    ItemsSource="{Binding DataSource}" />
```

**Key Points:**
- Zero-based index
- Must be within the range of `ItemsSource`
- Changes when user selects a different item

### SelectedItem Customization

Get or set the currently selected item object using `SelectedItem`.

```csharp
picker.Columns[0].SelectedItem = "India";
```

```xml
<picker:PickerColumn SelectedItem="{Binding SelectedCountry}"
                    ItemsSource="{Binding DataSource}" />
```

**Important Notes:**
- If both `SelectedItem` and `SelectedIndex` are set, `SelectedIndex` takes precedence for display
- In multi-column pickers, if one column's `SelectedItem` is set to `null`, all columns' `SelectedItem` values become `null`
- Use in MVVM scenarios for two-way binding

### HeaderText Customization

Add descriptive text to column headers.

**Default value:** `string.Empty`

```csharp
picker.Columns[0].HeaderText = "Languages";
```

```xml
<picker:PickerColumn HeaderText="Languages"
                    ItemsSource="{Binding DataSource}" />
```

**Note:** Column header must be enabled by setting `ColumnHeaderView.Height` > 0.

### ItemsSource Customization

Bind data to each column using `ItemsSource`.

**Default value:** `null`

```csharp
ObservableCollection<string> languages = new ObservableCollection<string> 
{ 
    "Spanish", "French", "Tamil", "English", "German", 
    "Chinese", "Telugu", "Japanese", "Arabic", "Russian" 
};

picker.Columns[0].ItemsSource = languages;
```

```xml
<picker:PickerColumn ItemsSource="{Binding Languages}" />
```

**Key Points:**
- Supports `ObservableCollection`, `List`, `IEnumerable`
- Changes to `ObservableCollection` automatically update the picker
- Each column can have its own independent data source

## Multi-Column Pickers

Create pickers with multiple columns for related data selection.

### Two-Column Example

```csharp
// First column - Countries
ObservableCollection<string> countryNames = new ObservableCollection<string>
{
    "Canada", "United States", "India", "United Kingdom", 
    "Australia", "Germany", "France", "Japan", "China", "Brazil"
};

PickerColumn countryColumn = new PickerColumn()
{
    HeaderText = "Select Country",
    ItemsSource = countryNames,
    SelectedIndex = 1,
};

// Second column - Cities
ObservableCollection<string> cityNames = new ObservableCollection<string>
{
    "Chennai", "Mumbai", "Delhi", "Kolkata", "Bangalore", 
    "Hyderabad", "Pune", "Ahmedabad", "Jaipur", "Lucknow"
};

PickerColumn cityColumn = new PickerColumn()
{
    HeaderText = "Select City",
    ItemsSource = cityNames,
    SelectedIndex = 1,
};

picker.Columns.Add(countryColumn);
picker.Columns.Add(cityColumn);
```

**XAML:**
```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.Columns>
        <picker:PickerColumn HeaderText="Select Country"
                            ItemsSource="{Binding Countries}"
                            SelectedIndex="1" />
        <picker:PickerColumn HeaderText="Select City"
                            ItemsSource="{Binding Cities}"
                            SelectedIndex="1" />
    </picker:SfPicker.Columns>
</picker:SfPicker>
```

### Three-Column Example (Date Picker Style)

```csharp
// Month column
ObservableCollection<string> months = new ObservableCollection<string>
{
    "January", "February", "March", "April", "May", "June",
    "July", "August", "September", "October", "November", "December"
};

// Day column
ObservableCollection<int> days = new ObservableCollection<int>();
for (int i = 1; i <= 31; i++)
{
    days.Add(i);
}

// Year column
ObservableCollection<int> years = new ObservableCollection<int>();
for (int i = 2020; i <= 2030; i++)
{
    years.Add(i);
}

picker.Columns.Add(new PickerColumn { HeaderText = "Month", ItemsSource = months });
picker.Columns.Add(new PickerColumn { HeaderText = "Day", ItemsSource = days });
picker.Columns.Add(new PickerColumn { HeaderText = "Year", ItemsSource = years });
```

## Column Divider

Customize the divider line between columns in multi-column pickers using `ColumnDividerColor`.

```xml
<picker:SfPicker x:Name="picker"
                 ColumnDividerColor="Red">
    <picker:SfPicker.Columns>
        <picker:PickerColumn ItemsSource="{Binding Countries}" />
        <picker:PickerColumn ItemsSource="{Binding Cities}" />
    </picker:SfPicker.Columns>
</picker:SfPicker>
```

```csharp
SfPicker picker = new SfPicker();
picker.ColumnDividerColor = Colors.Red;
```

**Key Points:**
- Only visible when picker has 2+ columns
- Helps visually separate different data categories
- Accepts any `Color` value

## Common Scenarios

### Scenario 1: Simple List Selection

```csharp
// Single column with simple strings
ObservableCollection<string> colors = new ObservableCollection<string>
{
    "Red", "Blue", "Green", "Yellow", "Purple"
};

picker.Columns.Add(new PickerColumn
{
    HeaderText = "Colors",
    ItemsSource = colors,
    SelectedIndex = 0
});
```

### Scenario 2: Dependent Columns (Cascading)

```csharp
// Country column
PickerColumn countryColumn = new PickerColumn
{
    HeaderText = "Country",
    ItemsSource = countries
};

// City column (updates based on country)
PickerColumn cityColumn = new PickerColumn
{
    HeaderText = "City",
    ItemsSource = cities
};

picker.Columns.Add(countryColumn);
picker.Columns.Add(cityColumn);

// Update cities when country changes
picker.SelectionChanged += (s, e) =>
{
    if (e.NewValue != null && e.NewValue.Count > 0)
    {
        var selectedCountry = e.NewValue[0] as PickerColumn;
        // Update cities based on selected country
        UpdateCitiesForCountry(selectedCountry.SelectedItem);
    }
};
```

### Scenario 3: Mixed Width Columns

```csharp
// Wide column for names, narrow columns for codes
picker.Columns.Add(new PickerColumn 
{ 
    HeaderText = "Country Name",
    ItemsSource = countryNames,
    Width = 200  // Wider
});

picker.Columns.Add(new PickerColumn 
{ 
    HeaderText = "Code",
    ItemsSource = countryCodes,
    Width = 80   // Narrower
});
```

### Scenario 4: Time Picker with 3 Columns

```csharp
// Hour (12-hour format)
ObservableCollection<int> hours = new ObservableCollection<int>();
for (int i = 1; i <= 12; i++) hours.Add(i);

// Minutes
ObservableCollection<string> minutes = new ObservableCollection<string>();
for (int i = 0; i < 60; i++) minutes.Add(i.ToString("D2"));

// AM/PM
ObservableCollection<string> ampm = new ObservableCollection<string> { "AM", "PM" };

picker.Columns.Add(new PickerColumn { HeaderText = "Hour", ItemsSource = hours, Width = 80 });
picker.Columns.Add(new PickerColumn { HeaderText = "Minute", ItemsSource = minutes, Width = 80 });
picker.Columns.Add(new PickerColumn { HeaderText = "", ItemsSource = ampm, Width = 80 });
```

## Best Practices

1. **Use appropriate data types:**
   - Use `ObservableCollection` for dynamic data that may change
   - Use `List` for static data that won't change

2. **Set meaningful header text:**
   - Helps users understand what each column represents
   - Essential for multi-column pickers

3. **Choose appropriate widths:**
   - Use `-1` for auto-sizing when content is similar
   - Set explicit widths for consistent appearance
   - Ensure all columns fit within the picker width

4. **Handle empty data sources:**
   - Validate that `ItemsSource` is not null or empty
   - Provide default values or placeholder text

5. **Initialize SelectedIndex carefully:**
   - Ensure index is within valid range
   - Consider setting to `-1` if no selection is needed initially

## Troubleshooting

### Issue: DisplayMemberPath not working

**Solution:**
- Verify the property name exactly matches your model class
- Ensure the property is public
- Check that `ItemsSource` contains the correct type

### Issue: SelectedIndex not updating

**Solution:**
- Ensure index is within the valid range of `ItemsSource`
- Check if `ItemsSource` is null or empty
- Verify the column is properly added to `Columns` collection

### Issue: Column divider not visible

**Solution:**
- Ensure picker has at least 2 columns
- Verify `ColumnDividerColor` is set to a visible color
- Check that columns have width

### Issue: Multi-column layout appears cramped

**Solution:**
- Adjust individual column widths
- Increase picker's `WidthRequest`
- Consider reducing the number of columns
- Use shorter header text
