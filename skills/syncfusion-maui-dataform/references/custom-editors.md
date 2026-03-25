# Custom Editors in .NET MAUI DataForm

Guide to creating custom editors explicitly, implementing IDataFormEditor interface, and managing custom data dictionaries.

## Table of Contents
- [Overview](#overview)
- [Explicit Editor Creation](#explicit-editor-creation)
- [Managing Items Collection](#managing-items-collection)
- [Custom Editor with IDataFormEditor](#custom-editor-with-idataformeditor)
- [Custom Data Dictionary](#custom-data-dictionary)
- [ItemManager Customization](#itemmanager-customization)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

While DataForm auto-generates editors based on data types, you have full control to:
- **Explicitly create editors** without auto-generation
- **Implement custom editor views** using IDataFormEditor
- **Manage items programmatically** (add, remove, replace)
- **Work with custom data sources** like dictionaries

**When to use custom editors:**
- Need complete control over form structure
- Working with non-standard data sources (dictionaries, dynamic objects)
- Implementing specialized input controls
- Conditional editor rendering based on business logic

## Explicit Editor Creation

### Disable Auto-Generation

Set `AutoGenerateItems` to `false` and manually define editors:

```xml
<dataForm:SfDataForm x:Name="dataForm" 
                     DataObject="{Binding ContactInfo}"
                     AutoGenerateItems="False">
    <dataForm:SfDataForm.Items>
        <dataForm:DataFormTextItem FieldName="Name"/>
        <dataForm:DataFormPasswordItem FieldName="Password"/>
        <dataForm:DataFormDateItem FieldName="BirthDate"/>
        <dataForm:DataFormNumericItem FieldName="Age"/>
    </dataForm:SfDataForm.Items>
</dataForm:SfDataForm>
```

**Important:** When `AutoGenerateItems="False"`:
- `GenerateDataFormItem` event will NOT fire
- You must manually specify all editors
- Use `FieldName` to bind each editor to data object properties

### Code-Behind Approach

```csharp
SfDataForm dataForm = new SfDataForm();
dataForm.DataObject = new ContactInfo();
dataForm.AutoGenerateItems = false;

ObservableCollection<DataFormItem> items = new ObservableCollection<DataFormItem>();
items.Add(new DataFormTextItem() { FieldName = "Name" });
items.Add(new DataFormPasswordItem() { FieldName = "Password" });
items.Add(new DataFormMultilineItem() { FieldName = "Address" });
items.Add(new DataFormDateItem() { FieldName = "BirthDate" });

dataForm.Items = items;
this.Content = dataForm;
```

### With Groups

```xml
<dataForm:SfDataForm x:Name="dataForm" 
                     DataObject="{Binding ContactInfo}"
                     AutoGenerateItems="False">
    <dataForm:SfDataForm.Items>
        <!-- Basic Info -->
        <dataForm:DataFormTextItem FieldName="Name"/>
        <dataForm:DataFormTextItem FieldName="Email"/>
        
        <!-- Address Group -->
        <dataForm:DataFormGroupItem Name="Address">
            <dataForm:DataFormGroupItem.Items>
                <dataForm:DataFormMultilineItem FieldName="Street"/>
                <dataForm:DataFormTextItem FieldName="City"/>
                <dataForm:DataFormTextItem FieldName="State"/>
                <dataForm:DataFormTextItem FieldName="ZipCode"/>
                <dataForm:DataFormAutoCompleteItem FieldName="Country" 
                                                    ItemsSource="{Binding Countries}"/>
            </dataForm:DataFormGroupItem.Items>
        </dataForm:DataFormGroupItem>
    </dataForm:SfDataForm.Items>
</dataForm:SfDataForm>
```

### Adding Custom View as Editor

Use `DataFormCustomItem` to embed any view:

```xml
<dataForm:SfDataForm x:Name="dataForm" 
                     DataObject="{Binding Profile}"
                     AutoGenerateItems="False">
    <dataForm:SfDataForm.Items>
        <!-- Custom Image Picker -->
        <dataForm:DataFormCustomItem FieldName="ProfileImage" 
                                      ShowLabel="False">
            <dataForm:DataFormCustomItem.EditorView>
                <VerticalStackLayout>
                    <Image Source="{Binding ProfileImage}" 
                           HeightRequest="100"
                           WidthRequest="100"
                           Aspect="AspectFill"/>
                    <Button Text="Change Photo" 
                            Command="{Binding ChangePhotoCommand}"/>
                </VerticalStackLayout>
            </dataForm:DataFormCustomItem.EditorView>
        </dataForm:DataFormCustomItem>
        
        <dataForm:DataFormTextItem FieldName="Name"/>
        <dataForm:DataFormTextItem FieldName="Bio"/>
    </dataForm:SfDataForm.Items>
</dataForm:SfDataForm>
```

## Managing Items Collection

### Dynamically Add Editors

```csharp
dataForm.AutoGenerateItems = false;

// Add editors at runtime
dataForm.Items.Add(new DataFormTextItem() { FieldName = "State" });
dataForm.Items.Add(new DataFormTextItem() { FieldName = "Country" });
dataForm.Items.Add(new DataFormNumericItem() { FieldName = "ZipCode" });
```

### Remove Editors

```csharp
// Remove by index
dataForm.Items.RemoveAt(2);

// Remove by field name
var itemToRemove = dataForm.Items.FirstOrDefault(i => i.FieldName == "MiddleName");
if (itemToRemove != null)
{
    dataForm.Items.Remove(itemToRemove);
}
```

### Clear All Editors

```csharp
dataForm.Items.Clear();
```

### Replace Editors

```csharp
// Replace editor at index
DataFormTextItem newItem = new DataFormTextItem() 
{ 
    FieldName = "Age",
    PlaceholderText = "Enter your age"
};
dataForm.Items[2] = newItem;

// Replace with different editor type
DataFormDateItem dateItem = new DataFormDateItem() 
{ 
    FieldName = "BirthDate",
    RowOrder = 2,
    ItemsOrderInRow = 1,
    ColumnSpan = 2
};
dataForm.Items[3] = dateItem;
```

### Add Group Dynamically

```csharp
DataFormGroupItem addressGroup = new DataFormGroupItem();
addressGroup.Name = "Address";
addressGroup.Items = new ObservableCollection<DataFormItem>();

addressGroup.Items.Add(new DataFormTextItem() { FieldName = "Street" });
addressGroup.Items.Add(new DataFormTextItem() { FieldName = "City" });
addressGroup.Items.Add(new DataFormTextItem() { FieldName = "State" });
addressGroup.Items.Add(new DataFormTextItem() { FieldName = "ZipCode" });

dataForm.Items.Add(addressGroup);
```

### Conditional Editor Creation

```csharp
public void BuildForm(UserRole role)
{
    dataForm.Items.Clear();
    dataForm.AutoGenerateItems = false;
    
    // Common fields
    dataForm.Items.Add(new DataFormTextItem() { FieldName = "Name" });
    dataForm.Items.Add(new DataFormTextItem() { FieldName = "Email" });
    
    // Role-specific fields
    if (role == UserRole.Admin)
    {
        dataForm.Items.Add(new DataFormTextItem() { FieldName = "AdminCode" });
        dataForm.Items.Add(new DataFormSwitchItem() { FieldName = "CanManageUsers" });
    }
    else if (role == UserRole.Manager)
    {
        dataForm.Items.Add(new DataFormTextItem() { FieldName = "Department" });
        dataForm.Items.Add(new DataFormSwitchItem() { FieldName = "CanApproveExpenses" });
    }
}
```

## Custom Editor with IDataFormEditor

Implement `IDataFormEditor` for complete custom editor control.

### Basic Implementation

```csharp
public class NumericTextEditor : IDataFormEditor 
{ 
    private SfDataForm dataForm;
    private DataFormCustomItem? dataFormCustomItem;

    public NumericTextEditor(SfDataForm dataForm)
    {
        this.dataForm = dataForm;
    }

    public View CreateEditorView(DataFormItem dataFormItem) 
    { 
        // Create custom entry that only accepts numbers
        Entry inputView = new Entry();
        inputView.Keyboard = Keyboard.Numeric;
        inputView.Placeholder = dataFormItem.PlaceholderText;
        
        // Apply DataForm text styles
        DataFormTextStyle textStyle = this.dataForm.EditorTextStyle;
        inputView.TextColor = textStyle.TextColor;
        inputView.FontSize = textStyle.FontSize;
        inputView.FontFamily = textStyle.FontFamily;
        inputView.FontAttributes = textStyle.FontAttributes;
        
        inputView.TextChanged += this.OnViewTextChanged;
        
        this.dataFormCustomItem = (DataFormCustomItem)dataFormItem;
        this.dataFormCustomItem.EditorValue = string.Empty;
        
        return inputView;
    }

    public void CommitValue(DataFormItem dataFormItem, View view)
    {
        if (view is Entry numericEntry)
        {
            double numericValue;
            double.TryParse(numericEntry.Text, out numericValue);
            dataFormItem.SetValue(numericValue);
        }
    }

    public void UpdateReadyOnly(DataFormItem dataFormItem)
    {
        // Handle read-only state if needed
    }

    private void ValidateValue(DataFormItem dataFormItem)
    {
        this.dataForm.Validate(new List<string>() { dataFormItem.FieldName });
    }

    private void OnViewTextChanged(object? sender, TextChangedEventArgs e)
    {
        if (sender is not Entry numericEntry || dataFormCustomItem == null)
        {
            return;
        }

        // Remove non-numeric characters
        string? numericText = Regex.Replace(numericEntry.Text, "[^0-9]+", string.Empty);
        if (numericText != numericEntry.Text)
        {
            numericEntry.Text = numericText;
            return;
        }

        dataFormCustomItem.EditorValue = numericText;
        this.ValidateValue(dataFormCustomItem);
        this.CommitValue(dataFormCustomItem, numericEntry);
    }
}
```

### Register Custom Editor

```csharp
// Register for a specific field
dataForm.RegisterEditor("PhoneNumber", new NumericTextEditor(dataForm));
```

### Slider Custom Editor Example

```csharp
public class SliderEditor : IDataFormEditor
{
    private SfDataForm dataForm;
    private DataFormCustomItem? dataFormCustomItem;

    public SliderEditor(SfDataForm dataForm)
    {
        this.dataForm = dataForm;
    }

    public View CreateEditorView(DataFormItem dataFormItem)
    {
        var stackLayout = new VerticalStackLayout();
        
        // Label to show current value
        var valueLabel = new Label
        {
            HorizontalOptions = LayoutOptions.Center,
            FontSize = 18,
            TextColor = Colors.Black
        };
        
        // Slider control
        var slider = new Slider
        {
            Minimum = 0,
            Maximum = 100,
            Value = 50
        };
        
        valueLabel.Text = $"Value: {slider.Value:F0}";
        
        slider.ValueChanged += (s, e) =>
        {
            valueLabel.Text = $"Value: {e.NewValue:F0}";
            if (dataFormCustomItem != null)
            {
                dataFormCustomItem.EditorValue = e.NewValue;
                CommitValue(dataFormCustomItem, slider);
            }
        };
        
        stackLayout.Children.Add(valueLabel);
        stackLayout.Children.Add(slider);
        
        this.dataFormCustomItem = (DataFormCustomItem)dataFormItem;
        this.dataFormCustomItem.EditorValue = slider.Value;
        
        return stackLayout;
    }

    public void CommitValue(DataFormItem dataFormItem, View view)
    {
        if (dataFormCustomItem?.EditorValue is double value)
        {
            dataFormItem.SetValue((int)value);
        }
    }

    public void UpdateReadyOnly(DataFormItem dataFormItem)
    {
    }
}

// Usage
dataForm.RegisterEditor("Rating", new SliderEditor(dataForm));
```

## Custom Data Dictionary

Work with dictionaries instead of strongly-typed objects.

### Setup Dictionary Data Source

```csharp
Dictionary<string, object> dictionary = new Dictionary<string, object>();
dictionary.Add("Name", "John");
dictionary.Add("Age", 30);
dictionary.Add("Email", "john@example.com");
dictionary.Add("IsActive", true);

// Create items manually
ObservableCollection<DataFormItem> items = new ObservableCollection<DataFormItem>();
foreach (var key in dictionary.Keys)
{
    DataFormItem? dataFormItem = null;
    
    if (key == "Name" || key == "Email")
    {
        dataFormItem = new DataFormTextItem()
        {
            FieldName = key,
            LabelText = key
        };
    }
    else if (key == "Age")
    {
        dataFormItem = new DataFormNumericItem()
        {
            FieldName = key,
            LabelText = key
        };
    }
    else if (key == "IsActive")
    {
        dataFormItem = new DataFormCheckBoxItem()
        {
            FieldName = key,
            LabelText = key
        };
    }

    if (dataFormItem != null)
    {
        items.Add(dataFormItem);
    }
}

dataForm.Items = items;
dataForm.AutoGenerateItems = false;
dataForm.ItemManager = new DictionaryItemManager(dictionary);
```

## ItemManager Customization

### Custom ItemManager for Dictionary

```csharp
public class DictionaryItemManager : DataFormItemManager
{
    private Dictionary<string, object> dataFormDictionary;

    public DictionaryItemManager(Dictionary<string, object> dictionary)
    {
        dataFormDictionary = dictionary;
    }

    public override object GetValue(DataFormItem dataFormItem)
    {
        // Retrieve value from dictionary
        if (dataFormDictionary.ContainsKey(dataFormItem.FieldName))
        {
            return dataFormDictionary[dataFormItem.FieldName];
        }
        return null;
    }

    public override void SetValue(DataFormItem dataFormItem, object value)
    {
        // Save value to dictionary
        dataFormDictionary[dataFormItem.FieldName] = value;
    }
}

// Apply custom ItemManager
dataForm.ItemManager = new DictionaryItemManager(dictionary);
```

### Custom ItemManager with Validation

```csharp
public class ValidatedItemManager : DataFormItemManager
{
    private Dictionary<string, object> data;
    private Dictionary<string, List<string>> errors;

    public ValidatedItemManager(Dictionary<string, object> data)
    {
        this.data = data;
        this.errors = new Dictionary<string, List<string>>();
    }

    public override object GetValue(DataFormItem dataFormItem)
    {
        return data.ContainsKey(dataFormItem.FieldName) 
            ? data[dataFormItem.FieldName] 
            : null;
    }

    public override void SetValue(DataFormItem dataFormItem, object value)
    {
        // Custom validation
        var validationErrors = ValidateValue(dataFormItem.FieldName, value);
        
        if (validationErrors.Count == 0)
        {
            data[dataFormItem.FieldName] = value;
            errors.Remove(dataFormItem.FieldName);
        }
        else
        {
            errors[dataFormItem.FieldName] = validationErrors;
        }
    }

    private List<string> ValidateValue(string fieldName, object value)
    {
        var errors = new List<string>();
        
        if (fieldName == "Email" && value is string email)
        {
            if (!email.Contains("@"))
            {
                errors.Add("Invalid email format");
            }
        }
        else if (fieldName == "Age" && value is int age)
        {
            if (age < 18 || age > 100)
            {
                errors.Add("Age must be between 18 and 100");
            }
        }
        
        return errors;
    }
}
```

## Best Practices

### 1. Always Set FieldName
```csharp
// ❌ Wrong - missing FieldName
items.Add(new DataFormTextItem());

// ✅ Correct
items.Add(new DataFormTextItem() { FieldName = "Name" });
```

### 2. Disable AutoGenerate When Using Explicit Items
```csharp
// ❌ Wrong - AutoGenerate still enabled
dataForm.Items = manualItems; // May cause conflicts

// ✅ Correct
dataForm.AutoGenerateItems = false;
dataForm.Items = manualItems;
```

### 3. Update EditorValue for Manual Validation
```csharp
// Required for ValidationMode.Manual
dataFormCustomItem.EditorValue = currentValue;
```

### 4. Handle Null Values
```csharp
public override object GetValue(DataFormItem dataFormItem)
{
    if (dataFormDictionary.ContainsKey(dataFormItem.FieldName))
    {
        return dataFormDictionary[dataFormItem.FieldName] ?? string.Empty;
    }
    return string.Empty; // Return default instead of null
}
```

### 5. Dispose Resources in Custom Editors
```csharp
public void Dispose()
{
    if (customView != null)
    {
        customView.SomeEvent -= OnSomeEvent;
        customView = null;
    }
}
```

## Troubleshooting

### Issue: Explicit Items Not Showing

**Symptoms:** DataForm is empty when using explicit items.

**Solutions:**
1. Verify `AutoGenerateItems="False"` is set
2. Check `FieldName` matches property name exactly (case-sensitive)
3. Ensure `DataObject` is bound and not null
4. Verify items are added to `Items` collection

```csharp
// Debug: Check items count
Debug.WriteLine($"DataForm has {dataForm.Items.Count} items");
```

---

### Issue: Custom Editor Not Committing Values

**Symptoms:** User input doesn't save to data object.

**Solutions:**
1. Ensure `CommitValue()` is implemented correctly
2. Call `dataFormItem.SetValue(value)` in `CommitValue`
3. For manual commit mode, update `EditorValue` property
4. Check if validation is preventing commit

---

### Issue: GenerateDataFormItem Event Not Firing

**Symptoms:** Event handler never executes.

**Cause:** `GenerateDataFormItem` only fires when `AutoGenerateItems="True"`.

**Solution:** Use explicit item configuration instead:
```csharp
// Instead of relying on event:
dataForm.AutoGenerateItems = false;
dataForm.Items.Add(new DataFormTextItem() 
{ 
    FieldName = "Name",
    PlaceholderText = "Enter name" // Set properties directly
});
```

---

### Issue: Dictionary Values Not Updating

**Symptoms:** Form displays but changes don't persist to dictionary.

**Solutions:**
1. Verify custom `ItemManager` is assigned
2. Check `SetValue()` implementation
3. Ensure dictionary keys match `FieldName` exactly

```csharp
// Debug in SetValue
public override void SetValue(DataFormItem dataFormItem, object value)
{
    Debug.WriteLine($"Setting {dataFormItem.FieldName} = {value}");
    dataFormDictionary[dataFormItem.FieldName] = value;
}
```

---

## Next Steps

- **[Validation](validation.md)** - Add validation to custom editors
- **[DataForm Settings](dataform-settings.md)** - Configure global editor settings
- **[Layout](layout.md)** - Organize custom editors in layouts

## Additional Resources

- [GitHub Sample - Explicit Items](https://github.com/SyncfusionExamples/maui-dataform/tree/master/ExplicitDataFormItems)
- [GitHub Sample - Custom Dictionary](https://github.com/SyncfusionExamples/maui-dataform/tree/master/CustomDataDictionarySample)
- [GitHub Sample - Custom Editor](https://github.com/SyncfusionExamples/maui-dataform/tree/master/CustomEditorSample)
