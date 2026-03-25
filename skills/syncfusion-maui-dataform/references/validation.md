# Data Validation in .NET MAUI DataForm

Comprehensive guide to validating user input with built-in validation attributes, validation modes, and custom validation logic.

## Table of Contents
- [Overview](#overview)
- [Built-in Validation Attributes](#built-in-validation-attributes)
- [Validation Modes](#validation-modes)
- [Manual Validation](#manual-validation)
- [Valid Messages](#valid-messages)
- [Validation Events](#validation-events)
- [Showing/Hiding Validation Labels](#showinghiding-validation-labels)
- [Customizing Validation Appearance](#customizing-validation-appearance)
- [Custom Validation Logic](#custom-validation-logic)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

DataForm automatically validates user input and displays error messages below editors when validation fails. Validation ensures data integrity before saving to your data object.

**Key Features:**
- **Built-in validation** via data annotation attributes
- **Three validation modes**: LostFocus, PropertyChanged, Manual
- **Customizable error/valid messages**
- **Event-based custom validation**
- **Styling for validation labels**

## Built-in Validation Attributes

### Required Validation

```csharp
[Required(ErrorMessage = "Name is required")]
public string Name { get; set; }

[Required(AllowEmptyStrings = false, ErrorMessage = "Email should not be empty")]
public string Email { get; set; }
```

**Key Properties:**
- `ErrorMessage` - Custom error message
- `AllowEmptyStrings` - If false, empty strings fail validation

---

### StringLength Validation

```csharp
[StringLength(50, ErrorMessage = "Name cannot exceed 50 characters")]
public string Name { get; set; }

[StringLength(100, MinimumLength = 8, ErrorMessage = "Password must be 8-100 characters")]
public string Password { get; set; }
```

**Use Cases:**
- Username length limits
- Password requirements
- Description character limits

---

### Range Validation

```csharp
[Range(18, 100, ErrorMessage = "Age must be between 18 and 100")]
public int Age { get; set; }

[Range(0.0, 999999.99, ErrorMessage = "Amount must be between 0 and 999,999.99")]
public decimal Amount { get; set; }
```

---

### RegularExpression Validation

```csharp
[RegularExpression(@"^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$", 
    ErrorMessage = "Invalid email format")]
public string Email { get; set; }

[RegularExpression(@"^(?=.*[a-z])(?=.*[A-Z])[a-zA-Z\d]{8,}$",
    ErrorMessage = "Password must contain uppercase, lowercase, and be 8+ characters")]
public string Password { get; set; }

[RegularExpression(@"^\d{3}-\d{3}-\d{4}$",
    ErrorMessage = "Phone format: 123-456-7890")]
public string PhoneNumber { get; set; }
```

---

### EmailAddress Validation

```csharp
[EmailAddress(ErrorMessage = "Invalid email address")]
public string Email { get; set; }
```

---

### Phone Validation

```csharp
[Phone(ErrorMessage = "Invalid phone number")]
public string PhoneNumber { get; set; }
```

---

### Url Validation

```csharp
[Url(ErrorMessage = "Invalid URL")]
public string Website { get; set; }
```

---

### CreditCard Validation

```csharp
[CreditCard(ErrorMessage = "Invalid credit card number")]
public string CardNumber { get; set; }
```

---

### DataFormDateRange Validation

Syncfusion-specific attribute for date validation:

```csharp
[DataType(DataType.Date)]
[DataFormDateRange(MinimumDate = "01/01/2022", MaximumDate = "31/12/2026", 
    ErrorMessage = "Date must be between 2022 and 2026")]
public DateTime JoinDate { get; set; }

// Current year only
[DataFormDateRange(MinimumDate = "01/01/2026", MaximumDate = "31/12/2026",
    ErrorMessage = "Please select a date in 2026")]
public DateTime EventDate { get; set; }
```

---

### Multiple Validations

Combine multiple validation attributes:

```csharp
public class UserRegistration
{
    [Required(ErrorMessage = "Username is required")]
    [StringLength(20, MinimumLength = 3, ErrorMessage = "Username must be 3-20 characters")]
    [RegularExpression(@"^[a-zA-Z0-9_]+$", ErrorMessage = "Username can only contain letters, numbers, and underscores")]
    public string Username { get; set; }
    
    [Required(ErrorMessage = "Email is required")]
    [EmailAddress(ErrorMessage = "Invalid email format")]
    public string Email { get; set; }
    
    [Required(ErrorMessage = "Password is required")]
    [StringLength(100, MinimumLength = 8, ErrorMessage = "Password must be at least 8 characters")]
    [RegularExpression(@"^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$",
        ErrorMessage = "Password must contain uppercase, lowercase, number, and special character")]
    public string Password { get; set; }
    
    [Required(ErrorMessage = "Age is required")]
    [Range(18, 120, ErrorMessage = "Age must be between 18 and 120")]
    public int Age { get; set; }
}
```

## Validation Modes

Control when validation occurs using `ValidationMode` property.

### LostFocus (Default)

Validates when editor loses focus (user tabs away or taps another field).

```xml
<dataForm:SfDataForm x:Name="dataForm"
                     ValidationMode="LostFocus"
                     DataObject="{Binding User}"/>
```

```csharp
dataForm.ValidationMode = DataFormValidationMode.LostFocus;
```

**Best for:** Most forms - validates after user finishes typing.

---

### PropertyChanged

Validates immediately on every character typed.

```xml
<dataForm:SfDataForm x:Name="dataForm"
                     ValidationMode="PropertyChanged"
                     DataObject="{Binding User}"/>
```

```csharp
dataForm.ValidationMode = DataFormValidationMode.PropertyChanged;
```

**Best for:** 
- Real-time feedback forms
- Password strength indicators
- Username availability checks

**Note:** May feel aggressive to users; use sparingly.

---

### Manual

No automatic validation. You control when to validate by calling `Validate()`.

```xml
<dataForm:SfDataForm x:Name="dataForm"
                     ValidationMode="Manual"
                     DataObject="{Binding User}"/>

<Button Text="Submit" Clicked="OnSubmitClicked"/>
```

```csharp
private void OnSubmitClicked(object sender, EventArgs e)
{
    if (dataForm.Validate())
    {
        dataForm.Commit();
        // Save data
        DisplayAlert("Success", "Form submitted successfully", "OK");
    }
    else
    {
        DisplayAlert("Error", "Please fix validation errors", "OK");
    }
}
```

**Best for:**
- Submit-button forms
- Wizard/multi-step forms
- Forms where validation shouldn't interrupt user flow

## Manual Validation

### Validate All Fields

```csharp
bool isFormValid = dataForm.Validate();

if (isFormValid)
{
    // Proceed with save
}
else
{
    // Show error message
}
```

### Validate Specific Fields

```csharp
// Validate only email and password
bool isValid = dataForm.Validate(new List<string> { "Email", "Password" });

// Validate single field
bool isEmailValid = dataForm.Validate(new List<string> { "Email" });
```

### Form Submission Pattern

```csharp
private async void OnSaveClicked(object sender, EventArgs e)
{
    // Validate form
    if (!dataForm.Validate())
    {
        await DisplayAlert("Validation Error", 
            "Please correct the errors before saving", 
            "OK");
        return;
    }
    
    // Commit changes to data object
    dataForm.Commit();
    
    // Save to database
    try
    {
        await SaveToDatabase(dataForm.DataObject);
        await DisplayAlert("Success", "Data saved successfully", "OK");
        await Navigation.PopAsync();
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", $"Failed to save: {ex.Message}", "OK");
    }
}
```

## Valid Messages

Show success messages when validation passes:

```csharp
[DataFormDisplayOptions(ValidMessage = "Password strength is good")]
[Required(ErrorMessage = "Password is required")]
[RegularExpression(@"^(?=.*[a-z])(?=.*[A-Z])[a-zA-Z\d]{8,}$",
    ErrorMessage = "Password must contain uppercase and lowercase letters, 8+ characters")]
public string Password { get; set; }

[DataFormDisplayOptions(ValidMessage = "Email format is correct")]
[Required(ErrorMessage = "Email is required")]
[EmailAddress(ErrorMessage = "Invalid email format")]
public string Email { get; set; }
```

**When to use:**
- Password strength indicators
- Confirmation of correct input
- Encouraging user feedback

## Validation Events

### ValidateForm Event

Triggered after manual validation call. Get validation details for entire form:

```csharp
dataForm.ValidateForm += OnValidateForm;

private void OnValidateForm(object sender, DataFormValidateFormEventArgs e)
{
    // Access form data
    object dataObject = e.DataObject;
    
    // Get all values
    Dictionary<string, object> values = e.NewValues;
    
    // Get error messages
    string allErrors = e.ErrorMessage;
    
    Debug.WriteLine($"Form valid: {string.IsNullOrEmpty(allErrors)}");
    Debug.WriteLine($"Errors: {allErrors}");
}
```

**Note:** Only fires when `Validate()` is called manually.

---

### ValidateProperty Event

Triggered for each property during validation. Implement custom validation logic:

```csharp
dataForm.ValidateProperty += OnValidateProperty;

private void OnValidateProperty(object sender, DataFormValidatePropertyEventArgs e)
{
    string propertyName = e.PropertyName;
    object newValue = e.NewValue;
    object currentValue = e.CurrentValue;
    bool isValid = e.IsValid;
    
    // Custom validation for specific fields
    if (propertyName == "Username")
    {
        string username = newValue?.ToString() ?? "";
        
        // Check if username already exists (example)
        if (IsUsernameTaken(username))
        {
            e.IsValid = false;
            e.ErrorMessage = "Username is already taken";
        }
    }
    else if (propertyName == "ConfirmPassword")
    {
        // Get password from data object
        var password = GetPropertyValue("Password");
        var confirmPassword = newValue?.ToString();
        
        if (password != confirmPassword)
        {
            e.IsValid = false;
            e.ErrorMessage = "Passwords do not match";
        }
        else
        {
            e.IsValid = true;
            e.ValidMessage = "Passwords match";
        }
    }
    else if (propertyName == "Age")
    {
        if (newValue is int age)
        {
            if (age >= 18 && age <= 25)
            {
                e.ValidMessage = "Eligible for youth discount!";
            }
        }
    }
}

private bool IsUsernameTaken(string username)
{
    // Check database or API
    return existingUsernames.Contains(username);
}
```

### Advanced Custom Validation

```csharp
dataForm.ValidateProperty += OnValidateProperty;

private void OnValidateProperty(object sender, DataFormValidatePropertyEventArgs e)
{
    // Complex business rule validation
    if (e.PropertyName == "EndDate")
    {
        var startDate = GetPropertyValue("StartDate") as DateTime?;
        var endDate = e.NewValue as DateTime?;
        
        if (startDate.HasValue && endDate.HasValue)
        {
            if (endDate <= startDate)
            {
                e.IsValid = false;
                e.ErrorMessage = "End date must be after start date";
            }
            else if ((endDate.Value - startDate.Value).Days > 365)
            {
                e.IsValid = false;
                e.ErrorMessage = "Date range cannot exceed 1 year";
            }
        }
    }
    
    // Async validation (username availability)
    if (e.PropertyName == "Email")
    {
        string email = e.NewValue?.ToString() ?? "";
        
        // Note: Can't use async in event, so use background task
        Task.Run(async () =>
        {
            bool isAvailable = await CheckEmailAvailability(email);
            
            Device.BeginInvokeOnMainThread(() =>
            {
                if (!isAvailable)
                {
                    e.IsValid = false;
                    e.ErrorMessage = "Email is already registered";
                    dataForm.Validate(new List<string> { "Email" });
                }
            });
        });
    }
}
```

## Showing/Hiding Validation Labels

### Hide Error Label

```xml
<dataForm:SfDataForm x:Name="dataForm" AutoGenerateItems="False">
    <dataForm:SfDataForm.Items>
        <dataForm:DataFormTextItem FieldName="Name" 
                                   ShowErrorLabel="False"/>
    </dataForm:SfDataForm.Items>
</dataForm:SfDataForm>
```

```csharp
// In GenerateDataFormItem event
if (e.DataFormItem?.FieldName == "Name")
{
    e.DataFormItem.ShowErrorLabel = false;
}
```

---

### Hide Valid Message Label

```xml
<dataForm:DataFormTextItem FieldName="Password" 
                           ShowValidMessageLabel="False"/>
```

```csharp
if (e.DataFormItem?.FieldName == "Password")
{
    e.DataFormItem.ShowValidMessageLabel = false;
}
```

## Customizing Validation Appearance

### Global Error Label Style

```xml
<dataForm:SfDataForm x:Name="dataForm">
    <dataForm:SfDataForm.ErrorLabelTextStyle>
        <dataForm:DataFormTextStyle FontSize="12" 
                                    FontAttributes="Italic" 
                                    TextColor="Red" 
                                    FontFamily="Roboto"/>
    </dataForm:SfDataForm.ErrorLabelTextStyle>
</dataForm:SfDataForm>
```

```csharp
dataForm.ErrorLabelTextStyle = new DataFormTextStyle
{
    TextColor = Colors.DarkRed,
    FontSize = 12,
    FontAttributes = FontAttributes.Bold,
    FontFamily = "Roboto"
};
```

---

### Per-Field Error Label Style

```csharp
dataForm.GenerateDataFormItem += OnGenerateDataFormItem;

private void OnGenerateDataFormItem(object sender, GenerateDataFormItemEventArgs e)
{
    if (e.DataFormItem?.FieldName == "Password")
    {
        e.DataFormItem.ErrorLabelTextStyle = new DataFormTextStyle
        {
            TextColor = Colors.OrangeRed,
            FontSize = 10,
            FontAttributes = FontAttributes.Italic,
            FontFamily = "Roboto"
        };
    }
}
```

---

### Global Valid Message Style

```xml
<dataForm:SfDataForm x:Name="dataForm">
    <dataForm:SfDataForm.ValidMessageLabelTextStyle>
        <dataForm:DataFormTextStyle FontSize="11" 
                                    TextColor="Green" 
                                    FontFamily="Roboto"/>
    </dataForm:SfDataForm.ValidMessageLabelTextStyle>
</dataForm:SfDataForm>
```

```csharp
dataForm.ValidMessageLabelTextStyle = new DataFormTextStyle
{
    TextColor = Colors.DarkGreen,
    FontSize = 11,
    FontAttributes = FontAttributes.Italic,
    FontFamily = "Roboto"
};
```

---

### Per-Field Valid Message Style

```csharp
private void OnGenerateDataFormItem(object sender, GenerateDataFormItemEventArgs e)
{
    if (e.DataFormItem?.FieldName == "Email")
    {
        e.DataFormItem.ValidMessageLabelTextStyle = new DataFormTextStyle
        {
            TextColor = Colors.SeaGreen,
            FontSize = 10,
            FontFamily = "Roboto"
        };
    }
}
```

## Custom Validation Logic

### Password Match Validation

```csharp
public class RegistrationModel
{
    [Required]
    [DataType(DataType.Password)]
    public string Password { get; set; }
    
    [Required]
    [DataType(DataType.Password)]
    [Display(Name = "Confirm Password")]
    public string ConfirmPassword { get; set; }
}

// In code-behind
dataForm.ValidateProperty += OnValidateProperty;

private void OnValidateProperty(object sender, DataFormValidatePropertyEventArgs e)
{
    if (e.PropertyName == "ConfirmPassword")
    {
        var model = dataForm.DataObject as RegistrationModel;
        if (model != null && model.Password != e.NewValue?.ToString())
        {
            e.IsValid = false;
            e.ErrorMessage = "Passwords do not match";
        }
    }
}
```

---

### Date Range Validation

```csharp
dataForm.ValidateProperty += OnValidateProperty;

private void OnValidateProperty(object sender, DataFormValidatePropertyEventArgs e)
{
    if (e.PropertyName == "CheckOutDate")
    {
        var model = dataForm.DataObject as Booking;
        var checkIn = model?.CheckInDate;
        var checkOut = e.NewValue as DateTime?;
        
        if (checkIn.HasValue && checkOut.HasValue)
        {
            if (checkOut <= checkIn)
            {
                e.IsValid = false;
                e.ErrorMessage = "Check-out must be after check-in date";
            }
            else if ((checkOut.Value - checkIn.Value).Days < 1)
            {
                e.IsValid = false;
                e.ErrorMessage = "Minimum stay is 1 night";
            }
        }
    }
}
```

---

### Conditional Required Field

```csharp
dataForm.ValidateProperty += OnValidateProperty;

private void OnValidateProperty(object sender, DataFormValidatePropertyEventArgs e)
{
    var model = dataForm.DataObject as OrderModel;
    
    // If shipping method is "Pickup", address is not required
    // If shipping method is "Delivery", address IS required
    if (e.PropertyName == "ShippingAddress")
    {
        if (model?.ShippingMethod == "Delivery")
        {
            string address = e.NewValue?.ToString() ?? "";
            if (string.IsNullOrWhiteSpace(address))
            {
                e.IsValid = false;
                e.ErrorMessage = "Shipping address is required for delivery";
            }
        }
    }
}
```

## Best Practices

### 1. Always Provide Clear Error Messages

```csharp
// ❌ Bad - generic message
[Required(ErrorMessage = "Required")]

// ✅ Good - specific, actionable message
[Required(ErrorMessage = "Please enter your email address")]
```

### 2. Validate Before Committing

```csharp
private void OnSaveClicked(object sender, EventArgs e)
{
    // ❌ Wrong - commit without validation
    dataForm.Commit();
    SaveData();
    
    // ✅ Correct - validate then commit
    if (dataForm.Validate())
    {
        dataForm.Commit();
        SaveData();
    }
}
```

### 3. Use Appropriate Validation Mode

- **LostFocus**: Most forms (default, good balance)
- **PropertyChanged**: Password strength, username availability
- **Manual**: Submit-button forms, wizards

### 4. Combine Multiple Validations

```csharp
// Strong password validation
[Required(ErrorMessage = "Password is required")]
[StringLength(100, MinimumLength = 8, ErrorMessage = "Password must be 8-100 characters")]
[RegularExpression(@"^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$",
    ErrorMessage = "Password must contain uppercase, lowercase, number, and special character")]
[DataFormDisplayOptions(ValidMessage = "Password strength: Strong")]
public string Password { get; set; }
```

### 5. Test Edge Cases

- Empty strings
- Null values
- Maximum lengths
- Boundary values (min/max)
- Special characters
- Whitespace-only input

## Troubleshooting

### Issue: Validation Not Working

**Symptoms:** No error messages appear for invalid data.

**Solutions:**
1. Check ValidationMode is not Manual (or call Validate())
2. Verify attributes are from `System.ComponentModel.DataAnnotations`
3. Ensure properties are public with getters and setters
4. Check ErrorMessage is provided in attributes

```csharp
// ❌ Wrong namespace
using SomeOtherNamespace.Required;

// ✅ Correct namespace
using System.ComponentModel.DataAnnotations;
```

---

### Issue: ValidateProperty Event Not Firing

**Symptoms:** Custom validation in event never executes.

**Solutions:**
1. Ensure event is subscribed before setting DataObject
2. Check ValidationMode isn't Manual (unless you call Validate())
3. Verify property name matches exactly (case-sensitive)

```csharp
// ✅ Correct order
dataForm.ValidateProperty += OnValidateProperty;
dataForm.DataObject = model;
```

---

### Issue: Error Message Not Showing

**Symptoms:** Validation fails but no message displays.

**Solutions:**
1. Check `ShowErrorLabel` is true (default)
2. Verify `ErrorMessage` property is set in attribute
3. Ensure DataForm has enough height for error label

---

### Issue: Custom Validation Overridden by Attributes

**Symptoms:** Your custom validation in ValidateProperty event is ignored.

**Solution:** Attribute validation runs first. If attributes pass, then ValidateProperty fires. Set `e.IsValid = false` to override:

```csharp
private void OnValidateProperty(object sender, DataFormValidatePropertyEventArgs e)
{
    // This overrides attribute validation result
    e.IsValid = false;
    e.ErrorMessage = "Custom error";
}
```

---

## Next Steps

- **[Data Annotations](data-annotations.md)** - Learn all annotation attributes
- **[Editing](editing.md)** - Control when changes are committed
- **[Custom Editors](custom-editors.md)** - Implement custom validation in custom editors

## Additional Resources

- [GitHub Sample - Manual Validation](https://github.com/SyncfusionExamples/maui-dataform/tree/master/ManualValidation)
- [API Documentation](https://help.syncfusion.com/cr/maui/Syncfusion.Maui.DataForm.html)
- [.NET Data Annotations](https://learn.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations)
