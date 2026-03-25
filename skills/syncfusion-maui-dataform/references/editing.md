# Editing and Commit Modes

## Table of Contents
- [Overview](#overview)
- [Commit Modes](#commit-modes)
- [Manual Commit](#manual-commit)
- [Value Converters](#value-converters)
- [Read-Only Mode](#read-only-mode)
- [Troubleshooting](#troubleshooting)

## Overview

DataForm editing behavior controls when and how user input is committed to the underlying data object.

**Key concepts:**
- **Commit:** Writing editor value back to DataObject property
- **Commit Mode:** Timing of automatic commits
- **Manual Commit:** Explicit save via `Commit()` method
- **Read-Only:** Disable editing globally or per-field

**When to customize commit behavior:**
- Validate before saving (manual commit with validation)
- Real-time synchronization (property changed commit)
- Form-style submission (manual commit on button click)
- Display-only forms (read-only mode)

## Commit Modes

The `CommitMode` property determines when editor values are automatically committed to the DataObject.

### LostFocus (Default)

Commits value when editor loses focus (user tabs or clicks away):

```xaml
<dataForm:SfDataForm 
    x:Name="dataForm"
    CommitMode="LostFocus">
</dataForm:SfDataForm>
```

```csharp
var dataForm = new SfDataForm
{
    CommitMode = DataFormCommitMode.LostFocus // Default
};
```

**Use cases:**
- Standard forms where users fill field-by-field
- Balance between immediate feedback and performance
- Allow users to complete typing before validation

**Behavior:**
- Value commits when user moves to next field
- Validation triggers on focus loss
- User sees errors after completing each field

### PropertyChanged

Commits value immediately on every change (each keystroke):

```xaml
<dataForm:SfDataForm 
    x:Name="dataForm"
    CommitMode="PropertyChanged">
</dataForm:SfDataForm>
```

```csharp
var dataForm = new SfDataForm
{
    CommitMode = DataFormCommitMode.PropertyChanged
};
```

**Use cases:**
- Real-time data binding and synchronization
- Live preview/calculation based on input
- Instant validation feedback
- Auto-save scenarios

**Behavior:**
- Value commits on each character typed
- Validation runs on every change
- DataObject always reflects current editor state

**⚠️ Performance consideration:** PropertyChanged mode triggers frequent commits and validations. Use cautiously with complex validation logic or large forms.

### Manual

Requires explicit `Commit()` call to save values:

```xaml
<dataForm:SfDataForm 
    x:Name="dataForm"
    CommitMode="Manual">
</dataForm:SfDataForm>
```

```csharp
var dataForm = new SfDataForm
{
    CommitMode = DataFormCommitMode.Manual
};
```

**Use cases:**
- Form submission with Save/Cancel buttons
- Validate entire form before committing
- Multi-step wizards with explicit progression
- Prevent partial data updates

**Behavior:**
- Editor values held in memory until `Commit()` called
- No automatic commits regardless of user actions
- Allows canceling changes by not committing

## Manual Commit

Explicitly save all field values using the `Commit()` method.

### Basic Manual Commit

```csharp
public class RegistrationPage : ContentPage
{
    private SfDataForm dataForm;
    
    public RegistrationPage()
    {
        dataForm = new SfDataForm
        {
            CommitMode = DataFormCommitMode.Manual,
            DataObject = new UserRegistration()
        };
        
        var saveButton = new Button { Text = "Save" };
        saveButton.Clicked += OnSaveClicked;
        
        var cancelButton = new Button { Text = "Cancel" };
        cancelButton.Clicked += OnCancelClicked;
        
        Content = new StackLayout
        {
            Children = { dataForm, saveButton, cancelButton }
        };
    }
    
    private void OnSaveClicked(object sender, EventArgs e)
    {
        // Commits all editor values to DataObject
        dataForm.Commit();
        
        // Now safe to save DataObject to database/API
        var model = dataForm.DataObject as UserRegistration;
        SaveToDatabase(model);
    }
    
    private void OnCancelClicked(object sender, EventArgs e)
    {
        // Don't commit - changes are discarded
        // Optionally reload original data
        dataForm.Reload();
    }
}
```

### Validation Before Commit

Manual commit mode triggers validation automatically:

```csharp
private async void OnSaveClicked(object sender, EventArgs e)
{
    // Commit() validates all fields before committing
    dataForm.Commit();
    
    // Check if validation passed
    bool isValid = dataForm.Validate();
    
    if (isValid)
    {
        var model = dataForm.DataObject as UserRegistration;
        await SaveToApiAsync(model);
        await DisplayAlert("Success", "Registration saved!", "OK");
        await Navigation.PopAsync();
    }
    else
    {
        await DisplayAlert("Validation Failed", "Please correct errors before saving.", "OK");
        
        // Scroll to first error
        var firstInvalidItem = dataForm.Items.FirstOrDefault(i => !i.IsValid);
        if (firstInvalidItem != null)
        {
            dataForm.ScrollTo(firstInvalidItem.FieldName);
        }
    }
}
```

### Conditional Commit

Commit only if specific conditions are met:

```csharp
private async void OnSubmitClicked(object sender, EventArgs e)
{
    // Check custom business rules
    var model = dataForm.DataObject as OrderForm;
    
    if (!model.AcceptedTerms)
    {
        await DisplayAlert("Terms Required", "Please accept terms and conditions.", "OK");
        return;
    }
    
    if (model.OrderTotal > model.CreditLimit)
    {
        bool proceed = await DisplayAlert(
            "Credit Limit Exceeded", 
            "Order exceeds credit limit. Proceed anyway?", 
            "Yes", 
            "No");
            
        if (!proceed)
            return;
    }
    
    // Commit only if all conditions satisfied
    dataForm.Commit();
    
    if (dataForm.Validate())
    {
        await SubmitOrderAsync(model);
    }
}
```

### Multi-Step Wizard with Manual Commit

```csharp
public class WizardPage : ContentPage
{
    private SfDataForm dataForm;
    private int currentStep = 0;
    
    private void OnNextClicked(object sender, EventArgs e)
    {
        // Validate current step before progressing
        bool isValid = dataForm.Validate();
        
        if (!isValid)
        {
            DisplayAlert("Validation Failed", "Complete current step first.", "OK");
            return;
        }
        
        currentStep++;
        
        if (currentStep >= totalSteps)
        {
            // Final step - commit all data
            dataForm.Commit();
            var model = dataForm.DataObject as WizardModel;
            SaveWizardData(model);
        }
        else
        {
            // Continue to next step without committing yet
            LoadStep(currentStep);
        }
    }
}
```

## Value Converters

Transform values between editor display format and DataObject storage format using `IValueConverter`.

### Basic Value Converter

Convert between string and DateTime:

```csharp
using System.Globalization;
using Syncfusion.Maui.DataForm;

public class StringToDateTimeConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        // Convert from DataObject (string) to editor (DateTime)
        if (value == null || string.IsNullOrEmpty(value.ToString()))
        {
            return DateTime.Now;
        }
        
        DateTime dateTime;
        if (DateTime.TryParse((string)value, out dateTime))
        {
            return dateTime;
        }
        
        return DateTime.Now;
    }
    
    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        // Convert from editor (DateTime) to DataObject (string)
        if (value is DateTime dateTime)
        {
            return dateTime.ToString("yyyy-MM-dd");
        }
        
        return string.Empty;
    }
}

// Apply converter to property
public class EventModel
{
    [DataFormValueConverter(typeof(StringToDateTimeConverter))]
    [DataType(DataType.Date)]
    public string EventDate { get; set; } // Stored as string, edited as DateTime
}
```

**Use case:** Database stores dates as strings, but users edit with DatePicker.

### Currency Converter

```csharp
public class DecimalToCurrencyConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        // Convert from DataObject (decimal) to editor display (formatted string)
        if (value is decimal amount)
        {
            return amount.ToString("C2", culture); // $1,234.56
        }
        return "$0.00";
    }
    
    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        // Convert from editor (string) to DataObject (decimal)
        if (value is string text)
        {
            // Remove currency symbols and parse
            string cleanText = text.Replace("$", "").Replace(",", "").Trim();
            if (decimal.TryParse(cleanText, out decimal result))
            {
                return result;
            }
        }
        return 0m;
    }
}

public class ProductModel
{
    [DataFormValueConverter(typeof(DecimalToCurrencyConverter))]
    public decimal Price { get; set; }
}
```

### Enum Display Name Converter

```csharp
public enum OrderStatus
{
    Pending,
    Processing,
    Shipped,
    Delivered
}

public class EnumToDisplayNameConverter : IValueConverter
{
    private static readonly Dictionary<OrderStatus, string> DisplayNames = new()
    {
        { OrderStatus.Pending, "Pending Payment" },
        { OrderStatus.Processing, "Processing Order" },
        { OrderStatus.Shipped, "Shipped - In Transit" },
        { OrderStatus.Delivered, "Delivered Successfully" }
    };
    
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value is OrderStatus status)
        {
            return DisplayNames.GetValueOrDefault(status, status.ToString());
        }
        return value?.ToString() ?? string.Empty;
    }
    
    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value is string displayName)
        {
            foreach (var kvp in DisplayNames)
            {
                if (kvp.Value == displayName)
                    return kvp.Key;
            }
        }
        return OrderStatus.Pending;
    }
}
```

**⚠️ AOT Compatibility:** For iOS/macOS AOT publishing, add `[Preserve(AllMembers = true)]` to converter:

```csharp
using Foundation;

[Preserve(AllMembers = true)]
public class StringToDateTimeConverter : IValueConverter
{
    // Implementation
}
```

**See also:** [data-annotations.md](data-annotations.md#dataformvalueconverter-attribute) for applying converters via attributes.

## Read-Only Mode

Disable editing globally or per-field to create display-only forms.

### Global Read-Only

Make entire form non-editable:

```xaml
<dataForm:SfDataForm 
    x:Name="dataForm"
    IsReadOnly="True">
</dataForm:SfDataForm>
```

```csharp
var dataForm = new SfDataForm
{
    IsReadOnly = true // All fields disabled
};
```

**Use cases:**
- View-only detail pages
- Confirmation screens before submission
- Historical record display

### Per-Field Read-Only Using Attributes

**Using `[ReadOnly]` attribute:**
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

**Using `[Editable]` attribute:**
```csharp
using System.ComponentModel.DataAnnotations;

public class UserProfile
{
    public string DisplayName { get; set; }
    
    [Editable(false)]
    public string Username { get; set; } // Cannot be edited
    
    [Editable(false)]
    public DateTime CreatedDate { get; set; } // Cannot be edited
}
```

**Attribute precedence:** `[ReadOnly]` takes higher priority than `[Editable]`:

```csharp
[ReadOnly(true)]
[Editable(true)]
public string Field { get; set; } // Field is READ-ONLY (ReadOnly wins)
```

### Per-Field Read-Only Using Event

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem != null)
    {
        // Make specific fields read-only
        if (e.DataFormItem.FieldName == "OrderId" || 
            e.DataFormItem.FieldName == "CreatedDate")
        {
            e.DataFormItem.IsReadOnly = true;
        }
    }
};
```

### Conditional Read-Only

Make fields read-only based on business logic:

```csharp
public class InvoiceForm : INotifyPropertyChanged
{
    private bool _isApproved;
    
    public bool IsApproved
    {
        get => _isApproved;
        set
        {
            _isApproved = value;
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(nameof(IsApproved)));
        }
    }
    
    public decimal Amount { get; set; }
    public string Notes { get; set; }
    
    public event PropertyChangedEventHandler PropertyChanged;
}

// In page code
dataForm.GenerateDataFormItem += (sender, e) =>
{
    var model = dataForm.DataObject as InvoiceForm;
    
    if (e.DataFormItem != null && model != null)
    {
        // Lock all fields if invoice is approved
        if (model.IsApproved)
        {
            e.DataFormItem.IsReadOnly = true;
        }
    }
};
```

### Toggle Read-Only Dynamically

```csharp
private bool _isEditMode = false;

private void OnToggleEditClicked(object sender, EventArgs e)
{
    _isEditMode = !_isEditMode;
    dataForm.IsReadOnly = !_isEditMode;
    
    editButton.Text = _isEditMode ? "Cancel" : "Edit";
    saveButton.IsVisible = _isEditMode;
}
```

## Troubleshooting

### Commit Not Saving Values

**Problem:** Called `Commit()` but DataObject not updated.

**Solutions:**
```csharp
// 1. Ensure CommitMode is Manual
dataForm.CommitMode = DataFormCommitMode.Manual;

// 2. Check DataObject is set and not null
if (dataForm.DataObject == null)
{
    dataForm.DataObject = new MyModel();
}

// 3. Verify property has setter
public class MyModel
{
    public string Name { get; set; } // ✅ Has setter
    // public string Name { get; } // ❌ No setter, cannot commit
}
```

### PropertyChanged Mode Performance Issues

**Problem:** Form laggy when typing with `CommitMode.PropertyChanged`.

**Solutions:**
```csharp
// Solution 1: Use LostFocus instead
dataForm.CommitMode = DataFormCommitMode.LostFocus;

// Solution 2: Optimize validation logic
// Avoid heavy operations in property setters or validators

// Solution 3: Use async validation sparingly
// See validation.md for performance tips
```

### Value Converter Not Working

**Problem:** Converter not transforming values.

**Solutions:**
```csharp
// 1. Verify converter is applied correctly
[DataFormValueConverter(typeof(StringToDateTimeConverter))]
public string EventDate { get; set; }

// 2. Ensure converter implements IValueConverter
public class MyConverter : IValueConverter // Must implement this
{
    public object Convert(...) { }
    public object ConvertBack(...) { }
}

// 3. Check ConvertBack returns correct type
public object ConvertBack(object value, Type targetType, ...)
{
    // Must return type matching property
    return value.ToString(); // If property is string
    // return (DateTime)value; // If property is DateTime
}

// 4. For iOS/macOS AOT, add Preserve attribute
[Preserve(AllMembers = true)]
public class MyConverter : IValueConverter { }
```

### Read-Only Fields Still Editable

**Problem:** `IsReadOnly = true` but field still editable.

**Solutions:**
```csharp
// 1. Check global IsReadOnly isn't overridden per-field
dataForm.IsReadOnly = true; // Global

dataForm.GenerateDataFormItem += (sender, e) =>
{
    // ❌ Don't override global setting:
    // e.DataFormItem.IsReadOnly = false;
};

// 2. Verify attribute spelling and namespace
using System.ComponentModel; // For ReadOnly
using System.ComponentModel.DataAnnotations; // For Editable

[ReadOnly(true)] // Correct
// [ReadonlyAttribute(true)] // ❌ Wrong

// 3. For event-based, ensure not null
if (e.DataFormItem != null) // Check not null
{
    e.DataFormItem.IsReadOnly = true;
}
```

### Manual Commit Validation Not Triggering

**Problem:** `Commit()` doesn't validate fields.

**Solution:**
```csharp
// Commit() automatically runs validation
dataForm.Commit();

// Check validation results
bool isValid = dataForm.Validate();

if (!isValid)
{
    // Show validation errors
    // Errors are already displayed in UI by Commit()
}

// If validation doesn't run, verify ValidationMode is set:
dataForm.ValidationMode = DataFormValidationMode.LostFocus; // or PropertyChanged
```

### Converter AOT Compilation Issue

**Problem:** Converter works in Debug but fails in Release (iOS/macOS).

**Solution:**
```csharp
// Add Preserve attribute for AOT compatibility
using Foundation;

[Preserve(AllMembers = true)]
public class StringToDateTimeConverter : IValueConverter
{
    [Preserve]
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        // Implementation
    }
    
    [Preserve]
    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        // Implementation
    }
}
```

---

**Related Topics:**
- [getting-started.md](getting-started.md) - Basic DataForm setup
- [validation.md](validation.md) - Validation modes and timing
- [data-annotations.md](data-annotations.md) - ReadOnly and Editable attributes
- [dataform-settings.md](dataform-settings.md) - IsReadOnly property configuration
