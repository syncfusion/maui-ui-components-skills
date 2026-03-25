# Data Validation

## Table of Contents
- [Overview](#overview)
- [Cell Value Validation](#cell-value-validation)
- [Row-Level Validation](#row-level-validation)
- [Custom Validation Logic](#custom-validation-logic)
- [IDataErrorInfo Support](#idataerrorinfo-support)
- [INotifyDataErrorInfo Support](#inotifydataerrorinfo-support)
- [Validation Display](#validation-display)

## Overview

DataGrid supports data validation to ensure data integrity. Validation can occur at:
- **Cell level** - Validate individual cell values
- **Row level** - Validate entire row data
- **Custom logic** - Implement complex business rules

## Cell Value Validation

### Using CellValidating Event

```csharp
dataGrid.CellValidating += dataGrid_CellValidating;

private void dataGrid_CellValidating(object sender, DataGridCellValidatingEventArgs args)
{
    if (args.NewValue.ToString().Equals("Brazil"))
    {
        args.IsValid = false;
        args.ErrorMessage = "Brazil cannot be passed";
    }
}
```

### Using CellValidated Event

```csharp
dataGrid.CellValidated += dataGrid_CellValidated;

private void dataGrid_CellValidated(object sender, DataGridCellValidatedEventArgs args)
{

}
```

## Row-Level Validation

Validate entire row before committing:

```csharp
dataGrid.RowValidating += dataGrid_RowValidating;

void dataGrid_RowValidating(object sender, DataGridRowValidatingEventArgs args)
{
    var data = args.RowData.GetType().GetProperty("ShipCountry").GetValue(args.RowData);

    if (data != null && data.ToString().Equals("Austria"))
    {
        args.IsValid = false;
        args.ErrorMessages.Add("ShipCountry", "Austria cannot be passed");
    }
}
```

```csharp
this.dataGrid.RowValidated += dataGrid_RowValidated;

private void dataGrid_RowValidated(object sender, DataGridRowValidatedEventArgs args)
{

}
```

## Custom Validation Logic

### Property Setter Validation

```csharp
public class OrderInfo : INotifyPropertyChanged
{
    private int quantity;
    
    public int Quantity
    {
        get { return quantity; }
        set
        {
            if (value <= 0)
                throw new ArgumentException("Quantity must be greater than 0");
                
            quantity = value;
            OnPropertyChanged(nameof(Quantity));
        }
    }
    
    private decimal unitPrice;
    
    public decimal UnitPrice
    {
        get { return unitPrice; }
        set
        {
            if (value < 0)
                throw new ArgumentException("Price cannot be negative");
                
            unitPrice = value;
            OnPropertyChanged(nameof(UnitPrice));
        }
    }
}
```

### Validation Attributes

```csharp
using System.ComponentModel.DataAnnotations;

public class OrderInfo
{
    [Required(ErrorMessage = "Order ID is required")]
    [Range(1, 999999, ErrorMessage = "Order ID must be between 1 and 999999")]
    public int OrderID { get; set; }
    
    [Required(ErrorMessage = "Customer ID is required")]
    [StringLength(10, ErrorMessage = "Customer ID cannot exceed 10 characters")]
    public string CustomerID { get; set; }
    
    [Range(1, 1000, ErrorMessage = "Quantity must be between 1 and 1000")]
    public int Quantity { get; set; }
    
    [Range(0.01, 10000, ErrorMessage = "Price must be between 0.01 and 10000")]
    public decimal UnitPrice { get; set; }
    
    [EmailAddress(ErrorMessage = "Invalid email format")]
    public string Email { get; set; }
}
```

## IDataErrorInfo Support

Implement `IDataErrorInfo` for built-in validation support:
Enable built-in validation support by setting SfDataGrid.ValidationMode or DataGridColumn.ValidationMode property to InEdit or InView.

```csharp
using System.ComponentModel;

public class OrderInfo : INotifyPropertyChanged, IDataErrorInfo
{
    public int OrderID { get; set; }
    public string CustomerID { get; set; }
    public int Quantity { get; set; }
    public decimal UnitPrice { get; set; }
    
    // Indexer for property-specific errors
    public string this[string columnName]
    {
        get
        {
            string error = null;
            
            switch (columnName)
            {
                case nameof(OrderID):
                    if (OrderID <= 0)
                        error = "Order ID must be greater than 0";
                    break;
                    
                case nameof(CustomerID):
                    if (string.IsNullOrWhiteSpace(CustomerID))
                        error = "Customer ID is required";
                    else if (CustomerID.Length > 10)
                        error = "Customer ID cannot exceed 10 characters";
                    break;
                    
                case nameof(Quantity):
                    if (Quantity <= 0)
                        error = "Quantity must be greater than 0";
                    else if (Quantity > 1000)
                        error = "Quantity cannot exceed 1000";
                    break;
                    
                case nameof(UnitPrice):
                    if (UnitPrice < 0)
                        error = "Price cannot be negative";
                    break;
            }
            
            return error;
        }
    }
    
    [Display(AutoGenerateField = false)]
    // Entity-level error
    public string Error
    {
        get
        {
            return string.Empty;
        }
    }
    
    // INotifyPropertyChanged implementation
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

## INotifyDataErrorInfo Support

For more advanced validation scenarios: Enable built-in validation support by setting SfDataGrid.ValidationMode or DataGridColumn.ValidationMode property to InEdit or InView.

```csharp
public class OrderInfo : INotifyDataErrorInfo
{
    private List<string> errors = new List<string>(); 

    private string shipCountry;

    public string ShipCountry
    {
        get { return shipCountry; }
        set { shipCountry = value; }
    }

    public System.Collections.IEnumerable GetErrors(string propertyName)
    {

        if (!propertyName.Equals("ShipCity"))
            return null;

        if (this.ShipCity.Contains("Graz") || this.ShipCity.Contains("Montréal"))
            errors.Add("Delivery not available for " + this.ShipCity);
        return errors;
    }

    [Display(AutoGenerateField = false)]

    public bool HasErrors
    {
        get
        {
            return false;
        }
    }

    public event EventHandler<DataErrorsChangedEventArgs> ErrorsChanged;
}
```

## Validation Display

### Show Validation Errors

Display errors to users:

```csharp
dataGrid.CellValidating += async (s, e) =>
{
    var order = e.RowData as OrderInfo;
    var errors = ValidateOrder(order);
    
    if (errors.Any())
    {
        await DisplayAlert("Validation Errors", 
            string.Join("\n", errors), "OK");
        e.IsValid = false;
    }
};

private List<string> ValidateOrder(OrderInfo order)
{
    var errors = new List<string>();
    
    if (order.Quantity <= 0)
        errors.Add("Quantity must be greater than 0");
        
    if (string.IsNullOrWhiteSpace(order.CustomerID))
        errors.Add("Customer ID is required");
        
    if (order.UnitPrice < 0)
        errors.Add("Price cannot be negative");
        
    return errors;
}
```

## Common Validation Patterns

### Pattern 1: Range Validation

```csharp
private bool ValidateRange(decimal value, decimal min, decimal max, string fieldName)
{
    if (value < min || value > max)
    {
        DisplayAlert("Validation Error", 
            $"{fieldName} must be between {min} and {max}", "OK");
        return false;
    }
    return true;
}

// Usage
dataGrid.CellValidating += (s, e) =>
{
    if (e.Column.MappingName == "Discount")
    {
        var order = e.RowData as OrderInfo;
        if (!ValidateRange(order.Discount, 0, 100, "Discount"))
            e.IsValid = false;
    }
};
```

### Pattern 2: Required Field Validation

```csharp
private bool ValidateRequired(string value, string fieldName)
{
    if (string.IsNullOrWhiteSpace(value))
    {
        DisplayAlert("Validation Error", 
            $"{fieldName} is required", "OK");
        return false;
    }
    return true;
}
```

### Pattern 3: Format Validation

```csharp
private bool ValidateEmail(string email)
{
    var emailPattern = @"^[^@\s]+@[^@\s]+\.[^@\s]+$";
    if (!Regex.IsMatch(email, emailPattern))
    {
        DisplayAlert("Validation Error", "Invalid email format", "OK");
        return false;
    }
    return true;
}
```

### Pattern 4: Cross-Field Validation

```csharp
dataGrid.CellValidating += (s, e) =>
{
    var order = e.RowData as OrderInfo;
    
    // Ship date must be after order date
    if (order.ShipDate <= order.OrderDate)
    {
        DisplayAlert("Validation Error", 
            "Ship date must be after order date", "OK");
        e.IsValid = false;
    }
    
    // Discount price must be less than unit price
    if (order.DiscountPrice >= order.UnitPrice)
    {
        DisplayAlert("Validation Error", 
            "Discount price must be less than unit price", "OK");
        e.IsValid = false;
    }
};
```

## Troubleshooting

### Validation Not Working

**Problem:** Validation doesn't prevent invalid data.

**Solutions:**
- Set `e.IsValid = false` in validation event
- Ensure event handlers are registered
- Check validation logic is correct

### Error Messages Not Showing

**Problem:** Users don't see validation errors.

**Solutions:**
- Use `DisplayAlert` or similar for user feedback
- Implement `IDataErrorInfo` for built-in error display
- Check error messages are being generated

### Validation Too Strict

**Problem:** Valid data rejected.

**Solutions:**
- Review validation logic
- Check range boundaries (inclusive vs exclusive)
- Test edge cases thoroughly

## Next Steps

- Read [editing.md](editing.md) for editing fundamentals
- Read [advanced-features.md](advanced-features.md) for complex scenarios
