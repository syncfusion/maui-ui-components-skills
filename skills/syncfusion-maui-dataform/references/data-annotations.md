# Data Annotations

## Table of Contents
- [Overview](#overview)
- [Display Attribute](#display-attribute)
- [Validation Attributes](#validation-attributes)
- [Editable and ReadOnly Attributes](#editable-and-readonly-attributes)
- [DataType Attribute](#datatype-attribute)
- [Custom Syncfusion Attributes](#custom-syncfusion-attributes)
- [Attribute Precedence](#attribute-precedence)
- [Troubleshooting](#troubleshooting)

## Overview

Data annotations in .NET MAUI DataForm provide a declarative way to configure field behavior, validation, and appearance using attributes from `System.ComponentModel.DataAnnotations` and `Syncfusion.Maui.DataForm` assemblies.

**When to use data annotations:**
- Configure field properties declaratively on the model class
- Define validation rules at the model level
- Specify display options (labels, prompts, grouping)
- Control field generation and editability
- Implement localization through resource files

**Benefits:**
- Centralized configuration on the model
- Reusable across different forms
- Type-safe and compile-time checked
- Follows standard .NET patterns

## Display Attribute

The `[Display]` attribute controls field appearance and behavior in the DataForm.

### Name Property

Specifies the label text displayed next to the editor:

```csharp
using System.ComponentModel.DataAnnotations;

public class ContactInfo
{
    [Display(Name = "First Name")]
    public string FirstName { get; set; }
    
    [Display(Name = "Last Name")]
    public string LastName { get; set; }
    
    [Display(Name = "Email Address")]
    public string Email { get; set; }
}
```

### ShortName Property

Takes higher priority than `Name` for label text:

```csharp
public class UserProfile
{
    [Display(Name = "User's First Name", ShortName = "First Name")]
    public string FirstName { get; set; }
    // Label will display "First Name" (ShortName takes priority)
}
```

### Prompt Property

Specifies watermark/placeholder text for the editor:

```csharp
public class RegistrationForm
{
    [Display(Prompt = "Enter your full name")]
    public string FullName { get; set; }
    
    [Display(Prompt = "john@example.com")]
    public string Email { get; set; }
    
    [Display(Prompt = "Enter a strong password")]
    public string Password { get; set; }
}
```

### GroupName Property

Groups related fields together in the DataForm:

```csharp
public class EmployeeDetails
{
    [Display(Name = "First Name", GroupName = "Personal Information")]
    public string FirstName { get; set; }
    
    [Display(Name = "Last Name", GroupName = "Personal Information")]
    public string LastName { get; set; }
    
    [Display(Name = "Date of Birth", GroupName = "Personal Information")]
    public DateTime DateOfBirth { get; set; }
    
    [Display(Name = "Job Title", GroupName = "Employment Details")]
    public string JobTitle { get; set; }
    
    [Display(Name = "Department", GroupName = "Employment Details")]
    public string Department { get; set; }
}
```

**See also:** [grouping.md](grouping.md) for advanced grouping features.

### Order Property

Controls the display order of fields:

```csharp
public class ProductForm
{
    [Display(Order = 0)]
    public string ProductName { get; set; }
    
    [Display(Order = 1)]
    public string Category { get; set; }
    
    [Display(Order = 2)]
    public decimal Price { get; set; }
    
    [Display(Order = 3)]
    public string Description { get; set; }
}
```

### AutoGenerateField Property

Controls whether the field is automatically generated:

```csharp
public class UserAccount
{
    public string Username { get; set; }
    
    [Display(AutoGenerateField = false)]
    public string InternalId { get; set; } // Hidden from DataForm
    
    [Display(AutoGenerateField = false)]
    public DateTime LastModified { get; set; } // Hidden from DataForm
}
```

**Alternative:** Use `[Bindable(false)]` attribute for same behavior.

### ResourceType Property

Enables localization using resource files (.resx):

```csharp
public class LocalizedForm
{
    [Display(
        Name = "FirstNameLabel",
        Prompt = "FirstNamePrompt",
        ResourceType = typeof(Resources.AppResources))]
    public string FirstName { get; set; }
    
    [Display(
        Name = "EmailLabel",
        Prompt = "EmailPrompt",
        ResourceType = typeof(Resources.AppResources))]
    public string Email { get; set; }
}
```

**See also:** [localization.md](localization.md) for complete localization setup.

## Validation Attributes

### Required Attribute

Marks a field as mandatory:

```csharp
using System.ComponentModel.DataAnnotations;

public class UserRegistration
{
    [Required(ErrorMessage = "Name is required")]
    public string Name { get; set; }
    
    [Required(AllowEmptyStrings = false, ErrorMessage = "Email cannot be empty")]
    public string Email { get; set; }
    
    [Required]
    public string Password { get; set; } // Uses default error message
}
```

### MinLength and MaxLength Attributes

Specify string length constraints:

```csharp
public class PasswordForm
{
    [MinLength(8, ErrorMessage = "Password must be at least 8 characters")]
    public string Password { get; set; }
    
    [MaxLength(50, ErrorMessage = "Username cannot exceed 50 characters")]
    public string Username { get; set; }
    
    [MinLength(10, ErrorMessage = "Description too short")]
    [MaxLength(500, ErrorMessage = "Description too long")]
    public string Description { get; set; }
}
```

### StringLength Attribute

Combines min and max length constraints:

```csharp
public class CommentForm
{
    [StringLength(200, MinimumLength = 10, 
        ErrorMessage = "Comment must be between 10 and 200 characters")]
    public string Comment { get; set; }
    
    [StringLength(100, ErrorMessage = "Name should not exceed 100 characters")]
    public string Name { get; set; }
}
```

### EnumDataType Attribute

Validates enum field values:

```csharp
public enum Gender
{
    Male,
    Female,
    Other,
    PreferNotToSay
}

public class PersonalInfo
{
    [EnumDataType(typeof(Gender), ErrorMessage = "Please select a valid gender")]
    public Gender Gender { get; set; }
}
```

### Range Attribute

Validates numeric ranges:

```csharp
using System.ComponentModel.DataAnnotations;

public class ProductOrder
{
    [Range(1, 100, ErrorMessage = "Quantity must be between 1 and 100")]
    public int Quantity { get; set; }
    
    [Range(0.01, 10000.00, ErrorMessage = "Price must be between $0.01 and $10,000")]
    public decimal Price { get; set; }
    
    [Range(18, 120, ErrorMessage = "Age must be between 18 and 120")]
    public int Age { get; set; }
}
```

**See also:** [validation.md](validation.md) for custom validators and advanced validation patterns.

## Editable and ReadOnly Attributes

### Editable Attribute

Controls whether a field can be edited:

```csharp
using System.ComponentModel;

public class UserProfile
{
    public string Username { get; set; }
    
    [Editable(false)]
    public string Email { get; set; } // Display-only field
    
    [Editable(false)]
    public DateTime CreatedDate { get; set; } // Display-only field
}
```

### ReadOnly Attribute

Marks a field as read-only:

```csharp
using System.ComponentModel;

public class OrderDetails
{
    public string ProductName { get; set; }
    
    [ReadOnly(true)]
    public string OrderId { get; set; } // Cannot be edited
    
    [ReadOnly(true)]
    public DateTime OrderDate { get; set; } // Cannot be edited
}
```

### Bindable Attribute

Controls field auto-generation:

```csharp
using System.ComponentModel;

public class InternalModel
{
    public string PublicField { get; set; }
    
    [Bindable(false)]
    public string InternalField { get; set; } // Not generated in DataForm
    
    [Bindable(false)]
    public Guid Id { get; set; } // Hidden from form
}
```

### Attribute Precedence

When both `ReadOnly` and `Editable` are applied:

```csharp
public class PrecedenceExample
{
    // ReadOnly takes higher priority
    [ReadOnly(true)]
    [Editable(true)]
    public string Field1 { get; set; } // Field will be READ-ONLY
    
    [ReadOnly(false)]
    [Editable(false)]
    public string Field2 { get; set; } // Field will be EDITABLE (ReadOnly wins)
}
```

**Rule:** `ReadOnlyAttribute` takes higher priority than `EditableAttribute`.

## DataType Attribute

The `[DataType]` attribute determines the editor type generated for a field.

### Supported DataType Values

```csharp
using System.ComponentModel.DataAnnotations;

public class FormWithEditors
{
    [DataType(DataType.Text)]
    public string Name { get; set; } // Single-line text editor
    
    [DataType(DataType.MultilineText)]
    public string Address { get; set; } // Multi-line text editor
    
    [DataType(DataType.Password)]
    public string Password { get; set; } // Password editor (masked)
    
    [DataType(DataType.EmailAddress)]
    public string Email { get; set; } // Email editor with keyboard
    
    [DataType(DataType.PhoneNumber)]
    public string Phone { get; set; } // Phone number editor
    
    [DataType(DataType.Date)]
    public DateTime BirthDate { get; set; } // Date picker (no time)
    
    [DataType(DataType.DateTime)]
    public DateTime AppointmentDateTime { get; set; } // Date + Time picker
    
    [DataType(DataType.Time)]
    public TimeSpan MeetingTime { get; set; } // Time picker only
}
```

### Editor Selection Based on DataType

| DataType | Generated Editor |
|----------|-----------------|
| `Text` | Single-line text entry |
| `MultilineText` | Multi-line text entry (TextArea) |
| `Password` | Masked text entry |
| `EmailAddress` | Text entry with email keyboard |
| `PhoneNumber` | Text entry with numeric keyboard |
| `Date` | Date picker without time |
| `DateTime` | Date picker with time |
| `Time` | Time picker |

**See also:** 
- [built-in-editors.md](built-in-editors.md) for all available editors
- [custom-editors.md](custom-editors.md) for creating custom editor types

## Custom Syncfusion Attributes

Custom attributes from `Syncfusion.Maui.DataForm` assembly provide extended functionality.

### DataFormDisplayOptions Attribute

Controls layout and visual options:

```csharp
using Syncfusion.Maui.DataForm;

public class LayoutExample
{
    [DataFormDisplayOptions(RowOrder = 0)]
    public string FieldOne { get; set; }
    
    [DataFormDisplayOptions(RowOrder = 1, ItemsOrderInRow = 0)]
    public string FieldTwo { get; set; }
    
    [DataFormDisplayOptions(RowOrder = 1, ItemsOrderInRow = 1)]
    public string FieldThree { get; set; } // Same row as FieldTwo
    
    [DataFormDisplayOptions(RowSpan = 2, ColumnSpan = 2)]
    public string LargeField { get; set; } // Spans 2 rows x 2 columns
    
    [DataFormDisplayOptions(ShowLabel = false)]
    public bool AcceptTerms { get; set; } // No label shown
    
    [DataFormDisplayOptions(ValidMessage = "Email format is correct!")]
    public string Email { get; set; } // Shows positive validation message
}
```

**Properties:**
- `RowOrder`: Specifies row position (0-based)
- `ItemsOrderInRow`: Position within a row (for multiple fields per row)
- `RowSpan`: Number of rows the field spans
- `ColumnSpan`: Number of columns the field spans
- `ShowLabel`: Whether to display the label
- `ValidMessage`: Positive message shown when validation passes

**See also:** [layout.md](layout.md) for comprehensive layout configuration.

### DataFormValueConverter Attribute

Applies a custom converter to transform values:

```csharp
using Syncfusion.Maui.DataForm;
using System.Globalization;

// Define converter
public class StringToDateConverter : IDataFormValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value is string dateString && DateTime.TryParse(dateString, out DateTime date))
        {
            return date;
        }
        return DateTime.Now;
    }
    
    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value is DateTime date)
        {
            return date.ToString("yyyy-MM-dd");
        }
        return string.Empty;
    }
}

// Apply converter
public class EventModel
{
    [DataFormValueConverter(typeof(StringToDateConverter))]
    public string EventDate { get; set; } // Stored as string, edited as DateTime
}
```

**⚠️ AOT Compatibility (iOS/macOS):**
When publishing in AOT mode, add `[Preserve(AllMembers = true)]` to the converter class:

```csharp
using Foundation; // iOS/macOS

[Preserve(AllMembers = true)]
public class StringToDateConverter : IDataFormValueConverter
{
    // Converter implementation
}
```

### DataFormDateRange Attribute

Constrains date/time picker ranges:

```csharp
using Syncfusion.Maui.DataForm;

public class EventForm
{
    // Basic date range
    [DataFormDateRange(MinimumDate = "2024-01-01", MaximumDate = "2024-12-31")]
    public DateTime EventDate { get; set; }
    
    // Custom display format
    [DataFormDateRange(
        DisplayFormat = "yyyy/MM/dd",
        MinimumDate = "2024/03/01",
        MaximumDate = "2024/03/31")]
    public DateTime MarchEventDate { get; set; }
    
    // Only allow future dates
    [DataFormDateRange(MinimumDate = "2024-03-20")]
    public DateTime FutureDate { get; set; }
}
```

**Properties:**
- `MinimumDate`: Earliest selectable date (string format)
- `MaximumDate`: Latest selectable date (string format)
- `DisplayFormat`: Format string for parsing min/max dates

**Common patterns:**

```csharp
// Today onwards
[DataFormDateRange(MinimumDate = "2024-03-20")] // Update to current date

// Specific year
[DataFormDateRange(MinimumDate = "2024-01-01", MaximumDate = "2024-12-31")]

// Last 30 days
[DataFormDateRange(MinimumDate = "2024-02-19", MaximumDate = "2024-03-20")]
```

## Attribute Precedence

When multiple attributes affect the same behavior, precedence rules apply:

### Label Text Priority
1. `Display.ShortName` (highest)
2. `Display.Name`
3. Property name (fallback)

```csharp
[Display(Name = "User's Full Name", ShortName = "Name")]
public string FullName { get; set; }
// Label displays "Name" (ShortName wins)
```

### Editability Priority
1. `ReadOnly` attribute (highest)
2. `Editable` attribute
3. Default (editable)

```csharp
[ReadOnly(true)]
[Editable(true)]
public string Field { get; set; }
// Field is READ-ONLY (ReadOnly wins)
```

### Field Visibility Priority
1. `Display.AutoGenerateField = false` (highest)
2. `Bindable(false)`
3. Manual `Items` collection override
4. Default (auto-generated)

## Troubleshooting

### Fields Not Displaying

**Problem:** Field with attributes not appearing in DataForm.

**Solutions:**
```csharp
// Check AutoGenerateField
[Display(AutoGenerateField = true)] // Explicitly enable

// Check Bindable
[Bindable(true)] // Ensure not set to false

// Verify DataObject binding
dataForm.DataObject = new MyModel(); // Must be set
```

### Validation Not Working

**Problem:** Validation attributes not triggering.

**Solutions:**
```csharp
// 1. Ensure ValidationMode is enabled
dataForm.ValidationMode = DataFormValidationMode.LostFocus;

// 2. Check Required with AllowEmptyStrings
[Required(AllowEmptyStrings = false, ErrorMessage = "Field required")]

// 3. Verify property type matches validator
[Range(1, 100)] // Only works with numeric types
public int Quantity { get; set; }
```

**See also:** [validation.md](validation.md) for comprehensive validation troubleshooting.

### Custom Attributes Not Applied

**Problem:** `DataFormDisplayOptions` or other custom attributes ignored.

**Solutions:**
```csharp
// 1. Verify namespace import
using Syncfusion.Maui.DataForm;

// 2. Check AutoGenerateItems is enabled
dataForm.AutoGenerateItems = true; // Required for attributes

// 3. Ensure not overridden in GenerateDataFormItem event
// If handling GenerateDataFormItem, attributes may be overridden
```

### AOT Compilation Issues (iOS/macOS)

**Problem:** Converter class not working after publishing in AOT mode.

**Solution:**
```csharp
using Foundation; // For Preserve attribute

[Preserve(AllMembers = true)]
public class MyConverter : IDataFormValueConverter
{
    // Implementation
}
```

### Localization Not Working

**Problem:** ResourceType not loading localized strings.

**Solutions:**
```csharp
// 1. Verify resource file exists and is embedded
// Build Action: Embedded Resource

// 2. Check ResourceType path
[Display(
    Name = "FirstNameLabel",
    ResourceType = typeof(MyProject.Resources.AppResources))] // Full type path

// 3. Ensure resource keys match exactly (case-sensitive)
```

**See also:** [localization.md](localization.md) for complete resource file setup.

### Date Range Not Constraining

**Problem:** `DataFormDateRange` not limiting date selection.

**Solutions:**
```csharp
// 1. Check DisplayFormat matches date strings
[DataFormDateRange(
    DisplayFormat = "yyyy-MM-dd", // Must match format below
    MinimumDate = "2024-03-01",
    MaximumDate = "2024-03-31")]

// 2. Verify property type is DateTime
public DateTime EventDate { get; set; } // Not string

// 3. Ensure valid date strings
[DataFormDateRange(MinimumDate = "2024-13-01")] // ❌ Invalid month
[DataFormDateRange(MinimumDate = "2024-03-01")] // ✅ Valid
```

### Order Attribute Not Sorting

**Problem:** Fields not appearing in specified order.

**Solutions:**
```csharp
// 1. Ensure all fields have Order or none do (don't mix)
[Display(Order = 0)]
public string Field1 { get; set; }

[Display(Order = 1)]
public string Field2 { get; set; }

// Don't mix ordered and unordered:
// ❌ public string Field3 { get; set; } // No Order specified

// 2. Use RowOrder in DataFormDisplayOptions for finer control
[DataFormDisplayOptions(RowOrder = 0)]
public string Field { get; set; }
```

---

**Related Topics:**
- [getting-started.md](getting-started.md) - Basic DataForm setup
- [validation.md](validation.md) - Custom validation and validators
- [layout.md](layout.md) - Layout and positioning configuration
- [grouping.md](grouping.md) - Field grouping strategies
- [localization.md](localization.md) - Resource file and multi-language support
