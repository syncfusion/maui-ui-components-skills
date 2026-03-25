# Built-in Editors in .NET MAUI DataForm

Comprehensive guide to all 14 built-in editor types that DataForm automatically generates based on property data types.

## Table of Contents
- [Editor Overview](#editor-overview)
- [Editor Generation Rules](#editor-generation-rules)
- [Text Editors](#text-editors)
- [Numeric Editor](#numeric-editor)
- [MaskedText Editor](#maskedtext-editor)
- [Date and Time Editors](#date-and-time-editors)
- [Boolean Editors](#boolean-editors)
- [Selection Editors](#selection-editors)
- [Changing Default Editors](#changing-default-editors)
- [Troubleshooting](#troubleshooting)

## Editor Overview

DataForm supports 14 built-in editor types that are automatically generated based on property data types and attributes. Each editor uses an appropriate MAUI control optimized for the data type.

**Available Editors:**
1. Text (Entry)
2. Multiline Text (Editor)
3. Password (Entry with IsPassword)
4. Numeric (SfNumericEntry)
5. MaskedText (SfMaskedEntry)
6. Date (DatePicker)
7. Time (TimePicker)
8. CheckBox (SfCheckBox)
9. Switch (SfSwitch)
10. Picker (Picker)
11. ComboBox (SfComboBox)
12. AutoComplete (SfAutoComplete)
13. RadioGroup (SfRadioButton)
14. Segment (SfSegmentedControl)

## Editor Generation Rules

### Automatic Generation Table

| Property Type | Default Editor | DataFormItem Type | Input Control |
|--------------|----------------|-------------------|---------------|
| `string` | Text | DataFormTextItem | Entry |
| `int`, `double`, `float` | Numeric | DataFormNumericItem | SfNumericEntry |
| `bool` | CheckBox | DataFormCheckBoxItem | SfCheckBox |
| `DateTime`, `DateOnly`, `DateTimeOffset` | Date | DataFormDateItem | DatePicker |
| `TimeSpan`, `TimeOnly` | Time | DataFormTimeItem | TimePicker |
| `Enum` | Picker | DataFormPickerItem | Picker |

### Attribute-Based Generation

Use `[DataType]` attribute to override default editors:

| Attribute | Generated Editor | DataFormItem Type |
|-----------|------------------|-------------------|
| `[DataType(DataType.Text)]` | Text | DataFormTextItem |
| `[DataType(DataType.MultilineText)]` | Multiline Text | DataFormMultilineTextItem |
| `[DataType(DataType.Password)]` | Password | DataFormPasswordItem |
| `[DataType(DataType.PhoneNumber)]` | MaskedText | DataFormMaskedTextItem |
| `[DataType(DataType.CreditCard)]` | MaskedText | DataFormMaskedTextItem |
| `[DataType(DataType.Date)]` | Date | DataFormDateItem |
| `[DataType(DataType.DateTime)]` | Date | DataFormDateItem |
| `[DataType(DataType.Time)]` | Time | DataFormTimeItem |

### Example: Automatic Generation

```csharp
public class EmployeeModel
{
    public string Name { get; set; }                    // → Text editor
    public int Age { get; set; }                        // → Numeric editor
    public double Salary { get; set; }                  // → Numeric editor
    public bool IsFullTime { get; set; }                // → CheckBox editor
    public DateTime JoinDate { get; set; }              // → Date editor
    public TimeSpan WorkHours { get; set; }             // → Time editor
    public Department Department { get; set; }          // → Picker editor
}

public enum Department { IT, HR, Finance, Marketing }
```

## Text Editors

### Text Editor (Entry)

Default editor for `string` properties. Uses MAUI `Entry` control.

```csharp
public string Name { get; set; }

// Or explicitly with DataType attribute
[DataType(DataType.Text)]
public string Email { get; set; }
```

#### Customizing Keyboard Type

Change the keyboard type for better input experience:

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "Email" && 
        e.DataFormItem is DataFormTextItem textItem)
    {
        textItem.Keyboard = Keyboard.Email; // Email keyboard
    }
};
```

**Available Keyboard Types:**
- `Keyboard.Text` - Standard text
- `Keyboard.Email` - Email addresses
- `Keyboard.Url` - URLs
- `Keyboard.Numeric` - Numbers only
- `Keyboard.Telephone` - Phone numbers

#### Setting Maximum Length

Limit input length:

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "Username" && 
        e.DataFormItem is DataFormTextItem textItem)
    {
        textItem.MaxLength = 20; // Max 20 characters
    }
};
```

---

### Multiline Text Editor (Editor)

For long-form text input. Uses MAUI `Editor` control. Automatically expands/contracts based on content.

```csharp
[DataType(DataType.MultilineText)]
public string Address { get; set; }

[DataType(DataType.MultilineText)]
[Display(Name = "Comments", Prompt = "Enter your comments...")]
public string Comments { get; set; }
```

**Features:**
- Auto-height adjustment based on text wrapping
- Scrollable for very long content
- Supports line breaks
- Ideal for addresses, descriptions, notes

#### Customize Multiline Editor

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "Description" && 
        e.DataFormItem is DataFormMultilineTextItem multilineItem)
    {
        multilineItem.MaxLength = 500;
        multilineItem.Keyboard = Keyboard.Text;
    }
};
```

---

### Password Editor (Entry)

Masked text entry for passwords. Uses MAUI `Entry` with `IsPassword = true`.

```csharp
[DataType(DataType.Password)]
[Display(Name = "Password", Prompt = "Enter your password")]
public string Password { get; set; }

[DataType(DataType.Password)]
[Display(Name = "Confirm Password")]
public string ConfirmPassword { get; set; }
```

**Features:**
- Characters are masked (●●●●●●)
- Supports MaxLength
- Can change keyboard type

---

## Numeric Editor

For numeric input (`int`, `double`, `float`, `decimal`). Uses Syncfusion `SfNumericEntry`.

### Basic Usage

```csharp
public int Age { get; set; }
public double Salary { get; set; }
public decimal Amount { get; set; }
```

### Customizing Numeric Editor

```csharp
public decimal Price { get; set; }
public int Percentage { get; set; }

dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "Price" && 
        e.DataFormItem is DataFormNumericItem numericItem)
    {
        numericItem.AllowNull = true;
        numericItem.CustomFormat = "#,0.00"; // Currency format
        numericItem.Culture = new CultureInfo("en-US");
        numericItem.ShowClearButton = true;
        numericItem.Minimum = 0;
        numericItem.Maximum = 10000;
    }
    else if (e.DataFormItem?.FieldName == "Percentage" && 
             e.DataFormItem is DataFormNumericItem percentItem)
    {
        percentItem.CustomFormat = "P"; // Percentage format
        percentItem.Minimum = 0;
        percentItem.Maximum = 100;
    }
};
```

### Numeric Up-Down Editor

Add increment/decrement buttons:

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "Quantity" && 
        e.DataFormItem is DataFormNumericItem numericItem)
    {
        numericItem.UpDownPlacementMode = NumericEditorUpDownPlacementMode.Inline;
        numericItem.Minimum = 1;
        numericItem.Maximum = 100;
    }
};
```

**UpDownPlacementMode Options:**
- `Hidden` - No buttons (default)
- `Inline` - Buttons appear inline with entry

### Custom Number Formats

```csharp
// Currency: $1,234.56
numericItem.CustomFormat = "C";
numericItem.Culture = new CultureInfo("en-US");

// Percentage: 45.50%
numericItem.CustomFormat = "P";

// Custom decimal places: 1,234.5678
numericItem.CustomFormat = "#,0.0000";

// At least 2, max 4 decimals: 123.45 or 123.4567
numericItem.CustomFormat = "#,0.00##";
```

---

## MaskedText Editor

For formatted input like phone numbers and credit cards. Uses Syncfusion `SfMaskedEntry`.

### Basic Usage

```csharp
[DataType(DataType.PhoneNumber)]
public string PhoneNumber { get; set; }

[DataType(DataType.CreditCard)]
public string CreditCard { get; set; }
```

### Customizing Masked Editor

```csharp
[DataType(DataType.PhoneNumber)]
public string Phone { get; set; }

public decimal Amount { get; set; }

// Register custom masked editor for Amount
dataForm.RegisterEditor("Amount", DataFormEditorType.MaskedText);

dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "Phone" && 
        e.DataFormItem is DataFormMaskedTextItem maskedItem)
    {
        maskedItem.PromptChar = '#';
        maskedItem.MaskType = MaskedEditorMaskType.Simple;
        maskedItem.Mask = "000 000 0000"; // Format: 123 456 7890
        maskedItem.Culture = new CultureInfo("en-US");
        maskedItem.ClearButtonVisibility = MaskedEditorClearButtonVisibility.WhileEditing;
    }
    else if (e.DataFormItem?.FieldName == "Amount" && 
             e.DataFormItem is DataFormMaskedTextItem amountMask)
    {
        amountMask.PromptChar = 'X';
        amountMask.MaskType = MaskedEditorMaskType.Simple;
        amountMask.Mask = "000000.00"; // Format: 123456.78
        amountMask.ValueMaskFormat = MaskedEditorMaskFormat.IncludeLiterals;
    }
};
```

**Common Mask Patterns:**
- Phone: `"000 000 0000"` or `"(000) 000-0000"`
- Credit Card: `"0000 0000 0000 0000"`
- SSN: `"000-00-0000"`
- ZIP Code: `"00000"` or `"00000-0000"`

---

## Date and Time Editors

### Date Editor (DatePicker)

For date selection. Uses MAUI `DatePicker`.

```csharp
public DateTime BirthDate { get; set; }
public DateTime? EventDate { get; set; } // Nullable for optional dates

[DataType(DataType.Date)]
public DateTime JoinDate { get; set; }
```

#### Change Date Format

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "EventDate" && 
        e.DataFormItem is DataFormDateItem dateItem)
    {
        dateItem.Format = "dd/MM/yyyy"; // Day/Month/Year
        // or "MMMM dd, yyyy" for "January 15, 2026"
        // or "MMM dd" for "Jan 15"
    }
};
```

#### Set Min/Max Dates

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "AppointmentDate" && 
        e.DataFormItem is DataFormDateItem dateItem)
    {
        dateItem.MinimumDate = DateTime.Today; // Can't pick past dates
        dateItem.MaximumDate = DateTime.Today.AddMonths(3); // Max 3 months ahead
    }
};
```

**Use Case: Age Verification**
```csharp
// Must be 18+ years old
dateItem.MaximumDate = DateTime.Today.AddYears(-18);
dateItem.MinimumDate = DateTime.Today.AddYears(-100);
```

---

### Time Editor (TimePicker)

For time selection. Uses MAUI `TimePicker`.

```csharp
public TimeSpan WorkHours { get; set; }
public TimeOnly? MeetingTime { get; set; }

[DataType(DataType.Time)]
public TimeSpan StartTime { get; set; }
```

#### Change Time Format

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "StartTime" && 
        e.DataFormItem is DataFormTimeItem timeItem)
    {
        timeItem.Format = "HH:mm"; // 24-hour format: 14:30
        // or "hh:mm tt" for 12-hour: 02:30 PM
    }
};
```

---

## Boolean Editors

### CheckBox Editor (SfCheckBox)

Default editor for `bool` properties. Uses Syncfusion `SfCheckBox`.

```csharp
public bool IsActive { get; set; }
public bool AgreeToTerms { get; set; }

[Display(Name = "Registered Member")]
public bool IsRegistered { get; set; } = false;
```

#### Customize CheckBox Color

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "AgreeToTerms" && 
        e.DataFormItem is DataFormCheckBoxItem checkBoxItem)
    {
        checkBoxItem.Color = Colors.Green;
    }
};
```

---

### Switch Editor (SfSwitch)

Toggle switch for `bool` properties. Uses Syncfusion `SfSwitch`.

**Must register explicitly:**

```csharp
dataForm.RegisterEditor("IsEnabled", DataFormEditorType.Switch);
dataForm.RegisterEditor("DarkMode", DataFormEditorType.Switch);

[Display(Name = "Enable Notifications")]
public bool IsEnabled { get; set; } = true;

public bool DarkMode { get; set; }
```

#### Customize Switch Colors

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "DarkMode" && 
        e.DataFormItem is DataFormSwitchItem switchItem)
    {
        switchItem.ThumbColor = Colors.White;
        switchItem.OnColor = Colors.DarkSlateBlue;
    }
};
```

---

## Selection Editors

### Picker Editor (Picker)

Dropdown selection. Uses MAUI `Picker`. Auto-generated for `enum` types.

```csharp
public Department Department { get; set; }

public enum Department
{
    IT,
    HR,
    Finance,
    Marketing,
    Sales
}
```

#### Set ItemsSource for Non-Enum Properties

```csharp
public string Country { get; set; }

// Method 1: Using IDataFormSourceProvider
public class DataFormItemsSourceProvider : IDataFormSourceProvider
{
    public object GetSource(string sourceName)
    {
        if (sourceName == "Country")
        {
            return new List<string> { "USA", "China", "India", "Japan" };
        }
        return new List<string>();
    }
}

dataForm.ItemsSourceProvider = new DataFormItemsSourceProvider();
dataForm.RegisterEditor("Country", DataFormEditorType.Picker);

// Method 2: Using GenerateDataFormItem Event
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "Country" && 
        e.DataFormItem is DataFormPickerItem pickerItem)
    {
        pickerItem.ItemsSource = new List<string> 
        { 
            "USA", "China", "India", "Japan" 
        };
    }
};
```

#### Complex Type Items (DisplayMemberPath)

```csharp
public string EmployeeId { get; set; }

public class Employee
{
    public int ID { get; set; }
    public string Name { get; set; }
}

// Provide items source
public class DataFormItemsSourceProvider : IDataFormSourceProvider
{
    public object GetSource(string sourceName)
    {
        if (sourceName == "EmployeeId")
        {
            return new List<Employee>
            {
                new Employee { ID = 1, Name = "John" },
                new Employee { ID = 2, Name = "Jane" },
                new Employee { ID = 3, Name = "Bob" }
            };
        }
        return new List<string>();
    }
}

dataForm.RegisterEditor("EmployeeId", DataFormEditorType.Picker);
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "EmployeeId" && 
        e.DataFormItem is DataFormPickerItem pickerItem)
    {
        pickerItem.DisplayMemberPath = "Name"; // Show name
        pickerItem.SelectedValuePath = "ID";   // Store ID
    }
};
```

---

### ComboBox Editor (SfComboBox)

Searchable dropdown with filtering. Uses Syncfusion `SfComboBox`.

**Must register explicitly:**

```csharp
public string City { get; set; }

dataForm.RegisterEditor("City", DataFormEditorType.ComboBox);
dataForm.ItemsSourceProvider = new DataFormItemsSourceProvider();

public class DataFormItemsSourceProvider : IDataFormSourceProvider
{
    public object GetSource(string sourceName)
    {
        if (sourceName == "City")
        {
            return new List<string> 
            { 
                "New York", "Los Angeles", "Chicago", "Houston", 
                "Phoenix", "Philadelphia", "San Antonio" 
            };
        }
        return new List<string>();
    }
}
```

#### Enable Editing/Filtering

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "City" && 
        e.DataFormItem is DataFormComboBoxItem comboBoxItem)
    {
        comboBoxItem.IsEditable = true; // Enable typing
        comboBoxItem.TextSearchMode = DataFormTextSearchMode.Contains;
        comboBoxItem.MaxDropDownHeight = 250;
    }
};
```

**TextSearchMode Options:**
- `StartsWith` - Filter items that start with typed text (default)
- `Contains` - Filter items that contain typed text anywhere

---

### AutoComplete Editor (SfAutoComplete)

Text input with auto-suggestions. Uses Syncfusion `SfAutoComplete`.

**Must register explicitly:**

```csharp
public string Country { get; set; }

dataForm.RegisterEditor("Country", DataFormEditorType.AutoComplete);

dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "Country" && 
        e.DataFormItem is DataFormAutoCompleteItem autoCompleteItem)
    {
        autoCompleteItem.ItemsSource = new List<string>
        {
            "Indonesia", "Italy", "India", "Iran", "Iraq",
            "Uganda", "Ukraine", "Canada", "Australia",
            "Uzbekistan", "France", "United Kingdom", "United States"
        };
        autoCompleteItem.TextSearchMode = DataFormTextSearchMode.StartsWith;
        autoCompleteItem.MaxDropDownHeight = 200;
    }
};
```

---

### RadioGroup Editor (SfRadioButton)

Radio button group for single selection. Uses Syncfusion `SfRadioButton`.

**Must register explicitly:**

```csharp
public string ContactMethod { get; set; }

dataForm.RegisterEditor("ContactMethod", DataFormEditorType.RadioGroup);
dataForm.ItemsSourceProvider = new DataFormItemsSourceProvider();

public class DataFormItemsSourceProvider : IDataFormSourceProvider
{
    public object GetSource(string sourceName)
    {
        if (sourceName == "ContactMethod")
        {
            return new List<string> { "Email", "Phone", "SMS" };
        }
        return new List<string>();
    }
}
```

**For Enum Types (Auto ItemsSource):**
```csharp
public Priority Priority { get; set; }

public enum Priority { Low, Medium, High, Urgent }

dataForm.RegisterEditor("Priority", DataFormEditorType.RadioGroup);
```

---

### Segment Editor (SfSegmentedControl)

Segmented button group. Uses Syncfusion `SfSegmentedControl`.

**Must register explicitly:**

```csharp
public string ViewMode { get; set; }

dataForm.RegisterEditor("ViewMode", DataFormEditorType.Segment);
dataForm.ItemsSourceProvider = new DataFormItemsSourceProvider();

public class DataFormItemsSourceProvider : IDataFormSourceProvider
{
    public object GetSource(string sourceName)
    {
        if (sourceName == "ViewMode")
        {
            return new List<string> { "List", "Grid", "Card" };
        }
        return new List<string>();
    }
}
```

---

## Changing Default Editors

### Change Editor for All Properties of a Type

Change the default editor for all properties of a specific data type:

```csharp
// Use multiline text for ALL string properties
dataForm.RegisterEditor(typeof(string), DataFormEditorType.MultilineText);

// Use switch for ALL bool properties
dataForm.RegisterEditor(typeof(bool), DataFormEditorType.Switch);
```

### Change Editor for Specific Property

Override the editor for a single property:

```csharp
// Use Switch instead of CheckBox for "IsActive"
dataForm.RegisterEditor("IsActive", DataFormEditorType.Switch);

// Use ComboBox instead of Picker for "Department"
dataForm.RegisterEditor("Department", DataFormEditorType.ComboBox);

// Use RadioGroup instead of Picker for enum
dataForm.RegisterEditor("Priority", DataFormEditorType.RadioGroup);
```

### Example: Mixed Editor Types

```csharp
public class UserSettings
{
    public bool DarkMode { get; set; }           // Default: CheckBox
    public bool Notifications { get; set; }      // Change to: Switch
    public bool AutoSave { get; set; }           // Change to: Switch
    public string Theme { get; set; }            // Change to: RadioGroup
}

// Register custom editors
dataForm.RegisterEditor("Notifications", DataFormEditorType.Switch);
dataForm.RegisterEditor("AutoSave", DataFormEditorType.Switch);
dataForm.RegisterEditor("Theme", DataFormEditorType.RadioGroup);
```

---

## Troubleshooting

### Issue: Wrong Editor Type Generated

**Symptoms:** Unexpected editor appears for a property.

**Solutions:**
1. Check property data type matches expected editor
2. Use `[DataType]` attribute to specify editor type
3. Use `RegisterEditor()` to explicitly set editor
4. Verify attribute is from `System.ComponentModel.DataAnnotations`

```csharp
// ❌ Wrong - no attribute, generates text editor
public string PhoneNumber { get; set; }

// ✅ Correct - generates masked text editor
[DataType(DataType.PhoneNumber)]
public string PhoneNumber { get; set; }
```

---

### Issue: ComboBox/AutoComplete Items Not Showing

**Symptoms:** Dropdown/suggestions are empty.

**Solutions:**
1. Verify `ItemsSourceProvider` is set on DataForm
2. Check `GetSource()` returns correct list for property name
3. Ensure property name matches exactly (case-sensitive)
4. For enum: verify enum type is accessible

```csharp
// ❌ Wrong - typo in property name
if (sourceName == "Citys") // Should be "City"

// ✅ Correct - exact match
if (sourceName == "City")
```

---

### Issue: Enum Picker Empty in AOT Mode (iOS/macOS)

**Symptoms:** Enum picker works in debug but empty in release AOT builds.

**Solution:** ItemsSource for enums is not supported in AOT. Use `IDataFormSourceProvider`:

```csharp
public Department Department { get; set; }

public enum Department { IT, HR, Finance }

// Convert enum to list manually
public class DataFormItemsSourceProvider : IDataFormSourceProvider
{
    public object GetSource(string sourceName)
    {
        if (sourceName == "Department")
        {
            return Enum.GetNames(typeof(Department)).ToList();
        }
        return new List<string>();
    }
}
```

---

### Issue: Numeric Editor Doesn't Accept Decimal Input

**Symptoms:** Can only enter integers, not decimals.

**Solution:** Property type must be `double`, `float`, or `decimal`, not `int`:

```csharp
// ❌ Wrong - int doesn't allow decimals
public int Price { get; set; }

// ✅ Correct - decimal allows decimal input
public decimal Price { get; set; }
```

---

### Issue: DisplayMemberPath Not Working for Complex Types

**Symptoms:** Complex objects show type name instead of property value.

**Solutions:**
1. Add `[Preserve(AllMembers = true)]` to both model classes (for AOT)
2. Verify `DisplayMemberPath` and `SelectedValuePath` are set correctly
3. Check property names match exactly (case-sensitive)

```csharp
[Preserve(AllMembers = true)]
public class ContactInfo
{
    public string EmployeeId { get; set; }
}

[Preserve(AllMembers = true)]
public class Employee
{
    public int ID { get; set; }
    public string Name { get; set; }
}
```

---

## Next Steps

- **[Custom Editors](custom-editors.md)** - Create completely custom editor controls
- **[Validation](validation.md)** - Add input validation to editors
- **[Data Annotations](data-annotations.md)** - Configure editors with attributes
- **[DataForm Settings](dataform-settings.md)** - Customize editor appearance

## Additional Resources

- [GitHub Sample - Data Editors](https://github.com/SyncfusionExamples/maui-dataform/tree/master/DataFormEditors)
- [API Documentation](https://help.syncfusion.com/cr/maui/Syncfusion.Maui.DataForm.html)
- [Video Tutorial](https://www.youtube.com/watch?v=Fv__sIKRsIA)
