# DataForm Settings and Configuration

## Table of Contents
- [Overview](#overview)
- [AutoGenerateItems Property](#autogenerateitems-property)
- [Editor Configuration](#editor-configuration)
- [Label Configuration](#label-configuration)
- [Editor Appearance](#editor-appearance)
- [Text Style Customization](#text-style-customization)
- [Manual Item Creation](#manual-item-creation)
- [ItemManager Customization](#itemmanager-customization)
- [Troubleshooting](#troubleshooting)

## Overview

DataForm settings control field generation, appearance, and behavior at both global and per-field levels.

**Configuration approaches:**
- **AutoGenerate:** Automatic field generation from DataObject properties
- **Manual:** Explicit field creation via `Items` collection
- **Hybrid:** AutoGenerate with event-based customization

**Key settings:**
- Field visibility and read-only state
- Label text and positioning
- Placeholder text and colors
- Text styles (font, size, color)
- Editor padding and background

## AutoGenerateItems Property

The `AutoGenerateItems` property controls whether fields are generated automatically from the DataObject.

### Auto-Generation Enabled (Default)

```csharp
var dataForm = new SfDataForm
{
    AutoGenerateItems = true, // Default: auto-generate from DataObject
    DataObject = new ContactInfo()
};
```

**Auto-generated editor types:**

| Property Type | Generated Editor | Attribute |
|--------------|------------------|-----------|
| `string` | `Entry` (single-line) | `[DataType(DataType.Text)]` |
| `string` | `Editor` (multi-line) | `[DataType(DataType.MultilineText)]` |
| `string` | `Entry` (masked) | `[DataType(DataType.Password)]` |
| `string` | `SfMaskedEntry` | `[DataType(DataType.PhoneNumber)]` |
| `int`, `double`, `float` | `SfNumericEntry` | - |
| `DateTime`, `DateTimeOffset`, `DateOnly` | `DatePicker` | `[DataType(DataType.Date/DateTime)]` |
| `TimeSpan`, `TimeOnly` | `TimePicker` | `[DataType(DataType.Time)]` |
| `bool` | `CheckBox` or `Switch` | - |
| `enum` | `SfComboBox`, `Picker`, or `RadioButton` | - |

### Cancel Auto-Generation for Specific Fields

**Using attributes:**
```csharp
using System.ComponentModel.DataAnnotations;

public class UserModel
{
    public string Username { get; set; }
    
    [Display(AutoGenerateField = false)]
    public int InternalId { get; set; } // Not generated
    
    [Bindable(false)]
    public DateTime LastModified { get; set; } // Not generated
}
```

**Using event:**
```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "InternalId")
    {
        e.Cancel = true; // Cancel generation
    }
};
```

### Auto-Generation Disabled

When `AutoGenerateItems = false`, manually create fields:

```csharp
var dataForm = new SfDataForm
{
    AutoGenerateItems = false,
    DataObject = new ContactInfo()
};

// Add fields manually (see Manual Item Creation section)
```

## Editor Configuration

### Read-Only Fields

Make specific fields non-editable:

**Using attributes:**
```csharp
using System.ComponentModel;
using System.ComponentModel.DataAnnotations;

public class OrderDetails
{
    public string ProductName { get; set; }
    
    [ReadOnly(true)]
    public string OrderId { get; set; } // Read-only
    
    [Editable(false)]
    public DateTime OrderDate { get; set; } // Read-only
}
```

**Using event:**
```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "OrderId")
    {
        e.DataFormItem.IsReadOnly = true;
    }
};
```

### Field Visibility

Show or hide specific fields:

**Using event:**
```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "InternalNote")
    {
        e.DataFormItem.IsVisible = false; // Hidden
    }
};
```

**Dynamic visibility:**
```csharp
private bool _showAdvancedFields = false;

dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "AdvancedOption")
    {
        e.DataFormItem.IsVisible = _showAdvancedFields;
    }
};

// Toggle visibility
private void ToggleAdvanced()
{
    _showAdvancedFields = !_showAdvancedFields;
    dataForm.Reload(); // Refresh to apply changes
}
```

### Placeholder Text

Set watermark text in editors:

**Using attributes:**
```csharp
public class RegistrationForm
{
    [Display(Prompt = "Enter your full name")]
    public string FullName { get; set; }
    
    [Display(Prompt = "john@example.com")]
    public string Email { get; set; }
}
```

**Using event:**
```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "Description")
    {
        e.DataFormItem.PlaceholderText = "Enter description";
        e.DataFormItem.PlaceholderColor = Colors.Gray;
    }
};
```

### Padding Configuration

Control spacing around fields:

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem != null)
    {
        // Uniform padding
        e.DataFormItem.Padding = new Thickness(10);
        
        // Or specific padding (left, top, right, bottom)
        e.DataFormItem.Padding = new Thickness(15, 10, 15, 10);
    }
};
```

### Background Color

Change field background:

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "HighlightedField")
    {
        e.DataFormItem.Background = Brush.LightYellow;
    }
};
```

## Label Configuration

### Label Text

Customize label text:

**Using attributes:**
```csharp
public class PersonalInfo
{
    [Display(Name = "First Name")]
    public string FirstName { get; set; }
    
    [Display(ShortName = "Email")] // ShortName takes priority
    public string EmailAddress { get; set; }
}
```

**Using event:**
```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "EmailAddress")
    {
        e.DataFormItem.LabelText = "Email";
    }
};
```

### Hide Label

Remove label for specific fields:

**Using attributes:**
```csharp
using Syncfusion.Maui.DataForm;

public class FormModel
{
    public string Username { get; set; }
    
    [DataFormDisplayOptions(ShowLabel = false)]
    public bool AcceptTerms { get; set; } // No label
}
```

**Using event:**
```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "AcceptTerms")
    {
        e.DataFormItem.ShowLabel = false;
    }
};
```

### Label Icon

Replace label text with custom icons:

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem != null)
    {
        if (e.DataFormItem.FieldName == "Email")
        {
            e.DataFormItem.LeadingView = new Label
            {
                Text = "\uf0e0", // FontAwesome envelope icon
                FontSize = 18,
                TextColor = Colors.Gray,
                FontFamily = "FontAwesome",
                HeightRequest = 24,
                VerticalTextAlignment = TextAlignment.End
            };
            e.DataFormItem.ShowLabel = false; // Hide text label
        }
    }
};
```

**Prerequisites:** Register icon font in `MauiProgram.cs`:

```csharp
builder.ConfigureFonts(fonts =>
{
    fonts.AddFont("FontAwesome.ttf", "FontAwesome");
});
```

**See also:** [layout.md](layout.md#custom-label-icons) for detailed icon setup.

### Layout Settings Per Field

Override global layout settings for specific fields:

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "Description")
    {
        e.DataFormItem.DefaultLayoutSettings = new DataFormDefaultLayoutSettings
        {
            LabelPosition = DataFormLabelPosition.Top, // Top label
            LabelWidth = new DataFormItemLength(100, DataFormItemLengthUnitType.Absolute),
            EditorWidth = new DataFormItemLength(300, DataFormItemLengthUnitType.Absolute)
        };
    }
};
```

## Editor Appearance

### Editor Height

Customize editor height:

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem != null)
    {
        if (e.DataFormItem.FieldName == "Comments")
        {
            e.DataFormItem.EditorHeight = 120; // Taller for multi-line
        }
        else if (e.DataFormItem.FieldName == "Code")
        {
            e.DataFormItem.EditorHeight = 40; // Compact
        }
    }
};
```

### Field Order

Control display order:

**Using attributes:**
```csharp
using System.ComponentModel.DataAnnotations;

public class ProductForm
{
    [Display(Order = 2)]
    public string ProductName { get; set; }
    
    [Display(Order = 0)]
    public string SKU { get; set; } // Displays first
    
    [Display(Order = 1)]
    public string Category { get; set; }
}
```

**Using event:**
```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem != null)
    {
        switch (e.DataFormItem.FieldName)
        {
            case "SKU":
                e.DataFormItem.RowOrder = 0;
                break;
            case "Category":
                e.DataFormItem.RowOrder = 1;
                break;
            case "ProductName":
                e.DataFormItem.RowOrder = 2;
                break;
        }
    }
};
```

**See also:** [layout.md](layout.md#field-ordering) for ordering in grid layouts.

## Text Style Customization

Customize fonts, sizes, and colors for labels, editors, and validation messages.

### Label Text Style

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "Username")
    {
        e.DataFormItem.LabelTextStyle = new DataFormTextStyle
        {
            FontSize = 16,
            TextColor = Colors.Blue,
            FontAttributes = FontAttributes.Bold
        };
    }
};
```

### Editor Text Style

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "Description")
    {
        e.DataFormItem.EditorTextStyle = new DataFormTextStyle
        {
            FontSize = 14,
            TextColor = Colors.Black,
            FontAttributes = FontAttributes.Italic,
            FontFamily = "OpenSans-Regular"
        };
    }
};
```

### Validation Message Styles

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "Email")
    {
        // Error message style
        e.DataFormItem.ErrorLabelTextStyle = new DataFormTextStyle
        {
            FontSize = 12,
            TextColor = Colors.Red,
            FontAttributes = FontAttributes.Italic
        };
        
        // Valid message style (positive feedback)
        e.DataFormItem.ValidMessageLabelTextStyle = new DataFormTextStyle
        {
            FontSize = 12,
            TextColor = Colors.Green,
            FontAttributes = FontAttributes.None
        };
    }
};
```

**See also:** [validation.md](validation.md#validation-label-appearance-customization) for comprehensive validation styling.

### Global Text Style Configuration

Apply styles to all fields, then override specific ones:

```csharp
private DataFormTextStyle defaultLabelStyle = new DataFormTextStyle
{
    FontSize = 14,
    TextColor = Colors.DarkGray,
    FontAttributes = FontAttributes.Bold
};

private DataFormTextStyle defaultEditorStyle = new DataFormTextStyle
{
    FontSize = 14,
    TextColor = Colors.Black
};

dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem != null)
    {
        // Apply default styles
        e.DataFormItem.LabelTextStyle = defaultLabelStyle;
        e.DataFormItem.EditorTextStyle = defaultEditorStyle;
        
        // Override for specific fields
        if (e.DataFormItem.FieldName == "Title")
        {
            e.DataFormItem.LabelTextStyle = new DataFormTextStyle
            {
                FontSize = 18,
                TextColor = Colors.Navy,
                FontAttributes = FontAttributes.Bold
            };
        }
    }
};
```

## Manual Item Creation

When `AutoGenerateItems = false`, create fields explicitly via the `Items` collection.

### Basic Manual Creation

```csharp
var dataForm = new SfDataForm
{
    AutoGenerateItems = false,
    DataObject = new ContactInfo()
};

dataForm.Items.Add(new DataFormTextItem
{
    FieldName = nameof(ContactInfo.FirstName),
    LabelText = "First Name",
    PlaceholderText = "Enter first name"
});

dataForm.Items.Add(new DataFormTextItem
{
    FieldName = nameof(ContactInfo.LastName),
    LabelText = "Last Name",
    PlaceholderText = "Enter last name"
});

dataForm.Items.Add(new DataFormTextItem
{
    FieldName = nameof(ContactInfo.Email),
    LabelText = "Email Address",
    PlaceholderText = "you@example.com",
    Keyboard = Keyboard.Email
});
```

### Different Editor Types

```csharp
dataForm.AutoGenerateItems = false;

// Text editor
dataForm.Items.Add(new DataFormTextItem
{
    FieldName = "Name"
});

// Multi-line text editor
dataForm.Items.Add(new DataFormMultilineItem
{
    FieldName = "Description",
    EditorHeight = 100
});

// Password editor
dataForm.Items.Add(new DataFormPasswordItem
{
    FieldName = "Password"
});

// Numeric editor
dataForm.Items.Add(new DataFormNumericItem
{
    FieldName = "Age"
});

// Date picker
dataForm.Items.Add(new DataFormDateItem
{
    FieldName = "BirthDate"
});

// Time picker
dataForm.Items.Add(new DataFormTimeItem
{
    FieldName = "AppointmentTime"
});

// Checkbox
dataForm.Items.Add(new DataFormCheckBoxItem
{
    FieldName = "AcceptTerms",
    LabelText = "I accept the terms and conditions"
});

// ComboBox for enum
dataForm.Items.Add(new DataFormComboBoxItem
{
    FieldName = "Gender",
    ItemsSource = Enum.GetValues(typeof(Gender)).Cast<Gender>().ToList()
});
```

### Manual Configuration with Full Options

```csharp
dataForm.Items.Add(new DataFormTextItem
{
    FieldName = "Username",
    LabelText = "Username",
    PlaceholderText = "Enter username",
    PlaceholderColor = Colors.Gray,
    IsReadOnly = false,
    IsVisible = true,
    ShowLabel = true,
    RowOrder = 0,
    Padding = new Thickness(10),
    Background = Brush.White,
    LabelTextStyle = new DataFormTextStyle
    {
        FontSize = 14,
        TextColor = Colors.Black,
        FontAttributes = FontAttributes.Bold
    },
    EditorTextStyle = new DataFormTextStyle
    {
        FontSize = 14,
        TextColor = Colors.DarkGray
    }
});
```

**When to use manual creation:**
- Need fine-grained control over field order and appearance
- DataObject has many properties but only want to display a few
- Want to add fields not in DataObject
- Building dynamic forms based on user input or configuration

## ItemManager Customization

The `ItemManager` provides hooks to customize editor initialization and behavior.

### Custom ItemManager

```csharp
public class CustomDataFormItemManager : DataFormItemManager
{
    public override void InitializeDataEditor(DataFormItem dataFormItem, View editor)
    {
        base.InitializeDataEditor(dataFormItem, editor);
        
        // Customize Entry editors
        if (editor is Entry entry)
        {
            entry.TextTransform = TextTransform.None;
            entry.VerticalTextAlignment = TextAlignment.Center;
            entry.ClearButtonVisibility = ClearButtonVisibility.WhileEditing;
        }
        
        // Customize Editor (multi-line)
        else if (editor is Editor editorControl)
        {
            editorControl.AutoSize = EditorAutoSizeOption.TextChanges;
        }
        
        // Customize DatePicker
        else if (editor is DatePicker datePicker)
        {
            datePicker.MinimumDate = DateTime.Today;
            datePicker.MaximumDate = DateTime.Today.AddYears(10);
        }
        
        // Customize Syncfusion NumericEntry
        else if (editor is Syncfusion.Maui.Inputs.SfNumericEntry numericEntry)
        {
            numericEntry.ShowClearButton = true;
            numericEntry.CustomFormat = "0.00";
        }
    }
}

// Apply custom ItemManager
dataForm.ItemManager = new CustomDataFormItemManager();
```

### Conditional Editor Customization

```csharp
public class ConditionalItemManager : DataFormItemManager
{
    public override void InitializeDataEditor(DataFormItem dataFormItem, View editor)
    {
        base.InitializeDataEditor(dataFormItem, editor);
        
        // Field-specific customization
        if (dataFormItem.FieldName == "Email" && editor is Entry emailEntry)
        {
            emailEntry.Keyboard = Keyboard.Email;
            emailEntry.TextTransform = TextTransform.Lowercase;
        }
        else if (dataFormItem.FieldName == "PostalCode" && editor is Entry postalEntry)
        {
            postalEntry.Keyboard = Keyboard.Numeric;
            postalEntry.MaxLength = 10;
        }
        else if (dataFormItem.FieldName == "Comments" && editor is Editor commentsEditor)
        {
            commentsEditor.Placeholder = "Enter your feedback here...";
            commentsEditor.MaxLength = 500;
        }
    }
}
```

### Advanced: Access Underlying Syncfusion Controls

```csharp
public class AdvancedItemManager : DataFormItemManager
{
    public override void InitializeDataEditor(DataFormItem dataFormItem, View editor)
    {
        base.InitializeDataEditor(dataFormItem, editor);
        
        // Customize Syncfusion ComboBox
        if (editor is Syncfusion.Maui.Inputs.SfComboBox comboBox)
        {
            comboBox.TextSearchMode = Syncfusion.Maui.Inputs.ComboBoxTextSearchMode.Contains;
            comboBox.ShowClearButton = true;
            comboBox.MaxDropDownHeight = 300;
        }
        
        // Customize Syncfusion AutoComplete
        else if (editor is Syncfusion.Maui.Inputs.SfAutoComplete autoComplete)
        {
            autoComplete.TextSearchMode = Syncfusion.Maui.Inputs.AutoCompleteTextSearchMode.StartsWith;
            autoComplete.MaximumSuggestionCount = 10;
        }
    }
}
```

**Use cases:**
- Apply platform-specific keyboard types
- Set input constraints (max length, allowed characters)
- Configure complex Syncfusion controls
- Add event handlers to editors

## Troubleshooting

### GenerateDataFormItem Not Firing

**Problem:** Event handler not called when DataForm loads.

**Solutions:**
```csharp
// 1. Ensure AutoGenerateItems is enabled
dataForm.AutoGenerateItems = true; // Required for event

// 2. Subscribe before setting DataObject
dataForm.GenerateDataFormItem += OnGenerateDataFormItem;
dataForm.DataObject = new MyModel(); // Set after subscribing

// 3. Check DataObject is not null
if (dataForm.DataObject == null)
{
    dataForm.DataObject = new MyModel();
}
```

### Field Settings Not Applying

**Problem:** Changes in `GenerateDataFormItem` not visible.

**Solutions:**
```csharp
// 1. Check if you're modifying the correct item
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "ExpectedFieldName") // Exact match
    {
        e.DataFormItem.IsReadOnly = true;
    }
};

// 2. Don't cancel the item generation
// ❌ e.Cancel = true; // This prevents the field from showing

// 3. For manual items, set properties directly
if (dataForm.AutoGenerateItems == false)
{
    var item = dataForm.Items.FirstOrDefault(i => i.FieldName == "Username");
    if (item != null)
    {
        item.IsReadOnly = true; // Direct modification
    }
}
```

### Placeholder Not Showing

**Problem:** `PlaceholderText` not visible in editor.

**Solutions:**
```csharp
// 1. Ensure field value is empty/null
// Placeholder only shows when editor is empty

// 2. Check editor type supports placeholders
// Entry, Editor, SfNumericEntry: ✅ Support placeholders
// DatePicker, TimePicker: ❌ No placeholder support

// 3. Verify PlaceholderColor is not same as background
e.DataFormItem.PlaceholderText = "Enter value";
e.DataFormItem.PlaceholderColor = Colors.Gray; // Contrasting color
```

### Manual Items Not Binding

**Problem:** Manually created items not syncing with DataObject.

**Solutions:**
```csharp
// Ensure FieldName matches property name exactly (case-sensitive)
public class Person
{
    public string FirstName { get; set; } // Property name
}

dataForm.Items.Add(new DataFormTextItem
{
    FieldName = nameof(Person.FirstName) // ✅ Use nameof for type safety
    // FieldName = "firstname" // ❌ Case mismatch
});
```

### ItemManager Not Applying

**Problem:** Custom `ItemManager` changes not taking effect.

**Solutions:**
```csharp
// 1. Set ItemManager before setting DataObject
dataForm.ItemManager = new CustomDataFormItemManager();
dataForm.DataObject = new MyModel(); // Set after ItemManager

// 2. Call base.InitializeDataEditor
public override void InitializeDataEditor(DataFormItem dataFormItem, View editor)
{
    base.InitializeDataEditor(dataFormItem, editor); // Required
    
    // Your customization here
}

// 3. Check editor type casting
if (editor is Entry entry) // Correct type check
{
    entry.MaxLength = 50;
}
```

### Text Style Not Changing

**Problem:** `LabelTextStyle` or `EditorTextStyle` not applying.

**Solutions:**
```csharp
// 1. Create new DataFormTextStyle, don't modify existing
e.DataFormItem.LabelTextStyle = new DataFormTextStyle // ✅ New instance
{
    FontSize = 16,
    TextColor = Colors.Blue
};

// ❌ Don't do this:
// var style = e.DataFormItem.LabelTextStyle;
// style.FontSize = 16; // Won't trigger update

// 2. Set all required properties
e.DataFormItem.EditorTextStyle = new DataFormTextStyle
{
    FontSize = 14, // Required
    TextColor = Colors.Black, // Required
    FontAttributes = FontAttributes.None // Optional
    // FontFamily: optional
};
```

### Field Order Not Changing

**Problem:** `RowOrder` or `Display.Order` not affecting sequence.

**Solutions:**
```csharp
// 1. Ensure all fields have Order/RowOrder (don't mix)
[Display(Order = 0)]
public string Field1 { get; set; }

[Display(Order = 1)]
public string Field2 { get; set; }

[Display(Order = 2)]
public string Field3 { get; set; }

// ❌ Don't leave some without Order:
// public string Field4 { get; set; }

// 2. Use consistent approach (all attributes OR all event-based)
```

---

**Related Topics:**
- [getting-started.md](getting-started.md) - Basic DataForm setup
- [layout.md](layout.md) - Field positioning and layout
- [data-annotations.md](data-annotations.md) - Attribute-based configuration
- [validation.md](validation.md) - Validation message styling
- [built-in-editors.md](built-in-editors.md) - Available editor types
- [custom-editors.md](custom-editors.md) - Creating custom editor controls
