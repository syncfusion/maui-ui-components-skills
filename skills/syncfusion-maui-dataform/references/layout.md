# Layout Configuration

## Table of Contents
- [Overview](#overview)
- [Linear Layout](#linear-layout)
- [Grid Layout](#grid-layout)
- [Label Configuration](#label-configuration)
- [Editor Width and Height](#editor-width-and-height)
- [Row and Column Spanning](#row-and-column-spanning)
- [Field Ordering](#field-ordering)
- [Editor Visibility](#editor-visibility)
- [Programmatic Scrolling](#programmatic-scrolling)
- [Troubleshooting](#troubleshooting)

## Overview

The .NET MAUI DataForm supports flexible layout configurations including linear and grid arrangements, label positioning, and customizable field dimensions.

**Layout capabilities:**
- **Linear layout:** One field per row (default)
- **Grid layout:** Multiple fields per row using `ColumnCount`
- **Label positioning:** Top or left of editor
- **Field sizing:** Row/column spanning, custom widths/heights
- **Field ordering:** Control display order of fields
- **Responsive layouts:** Adapt to screen sizes

## Linear Layout

By default, DataForm arranges fields linearly with one field per row.

### Linear Layout with Left Labels

```xaml
<dataForm:SfDataForm x:Name="dataForm">
    <dataForm:SfDataForm.DefaultLayoutSettings>
        <dataForm:DataFormDefaultLayoutSettings 
            LabelPosition="Left"/>
    </dataForm:SfDataForm.DefaultLayoutSettings>
</dataForm:SfDataForm>
```

```csharp
var dataForm = new SfDataForm
{
    DefaultLayoutSettings = new DataFormDefaultLayoutSettings
    {
        LabelPosition = DataFormLabelPosition.Left // Default
    }
};
```

**Result:** Labels appear to the left of editors, one field per row.

### Linear Layout with Top Labels

```xaml
<dataForm:SfDataForm x:Name="dataForm">
    <dataForm:SfDataForm.DefaultLayoutSettings>
        <dataForm:DataFormDefaultLayoutSettings 
            LabelPosition="Top"/>
    </dataForm:SfDataForm.DefaultLayoutSettings>
</dataForm:SfDataForm>
```

```csharp
var dataForm = new SfDataForm
{
    DefaultLayoutSettings = new DataFormDefaultLayoutSettings
    {
        LabelPosition = DataFormLabelPosition.Top
    }
};
```

**Result:** Labels appear above editors, one field per row with more vertical space.

## Grid Layout

Grid layout allows multiple fields per row by setting the `ColumnCount` property.

### Basic Grid Layout

```xaml
<dataForm:SfDataForm 
    x:Name="dataForm"
    ColumnCount="2">
</dataForm:SfDataForm>
```

```csharp
var dataForm = new SfDataForm
{
    ColumnCount = 2 // Two fields per row
};
```

### Three-Column Layout

```csharp
public class ContactForm
{
    public string FirstName { get; set; }
    public string MiddleName { get; set; }
    public string LastName { get; set; }
    
    public string Email { get; set; }
    public string Phone { get; set; }
    public string Website { get; set; }
}

// In page code
var dataForm = new SfDataForm
{
    ColumnCount = 3, // Three fields per row
    DataObject = new ContactForm()
};
```

**Result:** Fields arranged in 3 columns:
- Row 1: FirstName | MiddleName | LastName
- Row 2: Email | Phone | Website

### Field Order Within Rows

Control column position using `ItemsOrderInRow`:

```csharp
using Syncfusion.Maui.DataForm;

public class LayoutForm
{
    [DataFormDisplayOptions(RowOrder = 0, ItemsOrderInRow = 0)]
    public string Field1 { get; set; }
    
    [DataFormDisplayOptions(RowOrder = 0, ItemsOrderInRow = 1)]
    public string Field2 { get; set; }
    
    [DataFormDisplayOptions(RowOrder = 0, ItemsOrderInRow = 2)]
    public string Field3 { get; set; }
}

// In page code
var dataForm = new SfDataForm
{
    ColumnCount = 3,
    DataObject = new LayoutForm()
};
```

**Result:** All three fields in row 0, ordered: Field1, Field2, Field3.

**⚠️ Note:** `ColumnCount` applies to the entire DataForm, not individual groups. For group-specific column counts, see [grouping.md](grouping.md).

## Label Configuration

### Label Positioning

Position labels either to the left or top of editors:

```csharp
// Left positioning (default)
dataForm.DefaultLayoutSettings.LabelPosition = DataFormLabelPosition.Left;

// Top positioning
dataForm.DefaultLayoutSettings.LabelPosition = DataFormLabelPosition.Top;
```

### Per-Field Label Position

Override label position for specific fields:

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "Description")
    {
        // Description field has top label, others use default
        e.DataFormItem.DefaultLayoutSettings = new DataFormDefaultLayoutSettings
        {
            LabelPosition = DataFormLabelPosition.Top
        };
    }
};
```

### Label Visibility

Hide labels for specific fields:

**Using attributes:**
```csharp
using Syncfusion.Maui.DataForm;

public class FormModel
{
    public string Username { get; set; }
    
    [DataFormDisplayOptions(ShowLabel = false)]
    public bool AcceptTerms { get; set; } // No label shown
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

**Use case:** Checkboxes often don't need separate labels when the control itself contains text.

### Custom Label Icons

Replace label text with icons using `LeadingView`:

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem != null)
    {
        if (e.DataFormItem.FieldName == "Name")
        {
            e.DataFormItem.LeadingView = new Label
            {
                Text = "\uf007", // FontAwesome user icon
                FontSize = 18,
                TextColor = Colors.Gray,
                FontFamily = "FontAwesome",
                HeightRequest = 24,
                VerticalTextAlignment = TextAlignment.End
            };
            e.DataFormItem.ShowLabel = false; // Hide text label
        }
        else if (e.DataFormItem.FieldName == "Email")
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
            e.DataFormItem.ShowLabel = false;
        }
    }
};
```

**Prerequisites:** Add FontAwesome or icon font to your MAUI app's `Fonts` folder and register in `MauiProgram.cs`:

```csharp
builder.ConfigureFonts(fonts =>
{
    fonts.AddFont("FontAwesome.ttf", "FontAwesome");
});
```

## Editor Width and Height

### Label and Editor Width

Control proportional width distribution between labels and editors:

**Using Star Units (Proportional):**
```xaml
<dataForm:SfDataForm x:Name="dataForm">
    <dataForm:SfDataForm.DefaultLayoutSettings>
        <dataForm:DataFormDefaultLayoutSettings
            LabelPosition="Left"
            LabelWidth="0.4*"
            EditorWidth="0.6*"/>
    </dataForm:SfDataForm.DefaultLayoutSettings>
</dataForm:SfDataForm>
```

```csharp
dataForm.DefaultLayoutSettings.LabelWidth = new DataFormItemLength(0.4, DataFormItemLengthUnitType.Star);
dataForm.DefaultLayoutSettings.EditorWidth = new DataFormItemLength(0.6, DataFormItemLengthUnitType.Star);
```

**Default:** 40% label, 60% editor (0.4* / 0.6*)

**Using Absolute Units:**
```csharp
// Fixed pixel widths
dataForm.DefaultLayoutSettings.LabelWidth = new DataFormItemLength(150, DataFormItemLengthUnitType.Absolute);
dataForm.DefaultLayoutSettings.EditorWidth = new DataFormItemLength(400, DataFormItemLengthUnitType.Absolute);
```

**Common configurations:**

| Use Case | Label Width | Editor Width | Reason |
|----------|-------------|--------------|--------|
| Short labels | 0.3* | 0.7* | More space for input |
| Long labels | 0.5* | 0.5* | Equal distribution |
| Icon labels | 50px | 0.9* | Small fixed icon space |
| Mobile portrait | 0.4* | 0.6* | Default, balanced |
| Tablet/desktop | 200px | 600px | Fixed sizes for wide screens |

**⚠️ Note:** `LabelWidth` only applies when `LabelPosition` is `Left`. Top-positioned labels use full width.

### Editor Height

Customize editor height for specific fields:

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem != null)
    {
        // Multi-line text editor needs more height
        if (e.DataFormItem.FieldName == "Comments")
        {
            e.DataFormItem.EditorHeight = 120;
        }
        
        // Compact single-line editor
        if (e.DataFormItem.FieldName == "Code")
        {
            e.DataFormItem.EditorHeight = 40;
        }
    }
};
```

**Default editor height:** 48 pixels

**Use cases:**
- Multi-line text: 100-150 pixels
- Compact forms: 36-40 pixels
- Accessibility (large touch targets): 60+ pixels

## Row and Column Spanning

### Row Span

Increase a field's vertical size across multiple rows:

**Using attributes:**
```csharp
using Syncfusion.Maui.DataForm;

public class ProductForm
{
    public string ProductName { get; set; }
    
    [DataFormDisplayOptions(RowSpan = 3)]
    public string Description { get; set; } // Spans 3 rows
    
    public decimal Price { get; set; }
    public int Stock { get; set; }
}
```

**Using event:**
```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "Description")
    {
        e.DataFormItem.RowSpan = 3; // Editor occupies 3 rows vertically
    }
};
```

**Result:** Description editor spans 3 row heights, allowing taller multi-line input.

### Column Span

When using grid layout, fields can span multiple columns:

**Using attributes:**
```csharp
using Syncfusion.Maui.DataForm;

public class EventForm
{
    [DataFormDisplayOptions(ColumnSpan = 2)]
    public string EventTitle { get; set; } // Spans 2 columns
    
    public DateTime StartDate { get; set; }
    public DateTime EndDate { get; set; }
    
    [DataFormDisplayOptions(ColumnSpan = 2)]
    public string Description { get; set; } // Spans 2 columns
}

// In page code
var dataForm = new SfDataForm
{
    ColumnCount = 2,
    DataObject = new EventForm()
};
```

**Result:**
- Row 1: EventTitle (spans both columns)
- Row 2: StartDate | EndDate (each 1 column)
- Row 3: Description (spans both columns)

**Using event:**
```csharp
dataForm.ColumnCount = 3;

dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem != null)
    {
        // Header fields span all columns
        if (e.DataFormItem.FieldName == "Title" || 
            e.DataFormItem.FieldName == "Description")
        {
            e.DataFormItem.ColumnSpan = 3;
        }
    }
};
```

### Complex Spanning Layout

```csharp
public class RegistrationForm
{
    [DataFormDisplayOptions(RowOrder = 0, ColumnSpan = 2)]
    public string FullName { get; set; } // Row 0, spans 2 columns
    
    [DataFormDisplayOptions(RowOrder = 1, ItemsOrderInRow = 0)]
    public string Email { get; set; } // Row 1, column 0
    
    [DataFormDisplayOptions(RowOrder = 1, ItemsOrderInRow = 1)]
    public string Phone { get; set; } // Row 1, column 1
    
    [DataFormDisplayOptions(RowOrder = 2, ColumnSpan = 2, RowSpan = 2)]
    public string Address { get; set; } // Row 2-3, spans 2 columns
    
    [DataFormDisplayOptions(RowOrder = 4, ColumnSpan = 2)]
    public string Comments { get; set; } // Row 4, spans 2 columns
}

var dataForm = new SfDataForm
{
    ColumnCount = 2,
    DataObject = new RegistrationForm()
};
```

**Visual layout:**
```
┌─────────────────────┐
│ FullName (2 cols)   │  Row 0
├──────────┬──────────┤
│ Email    │ Phone    │  Row 1
├──────────┴──────────┤
│                     │
│ Address (2x2)       │  Rows 2-3
│                     │
├─────────────────────┤
│ Comments (2 cols)   │  Row 4
└─────────────────────┘
```

## Field Ordering

Control the display order of fields in the DataForm.

### Using Display Order Attribute

```csharp
using System.ComponentModel.DataAnnotations;

public class ProductForm
{
    [Display(Order = 2)]
    public string ProductName { get; set; }
    
    [Display(Order = 0)]
    public string SKU { get; set; } // Displays first
    
    [Display(Order = 1)]
    public string Category { get; set; } // Displays second
    
    [Display(Order = 3)]
    public decimal Price { get; set; }
}
```

**Result order:** SKU → Category → ProductName → Price

### Using RowOrder Attribute

```csharp
using Syncfusion.Maui.DataForm;

public class FormModel
{
    [DataFormDisplayOptions(RowOrder = 1)]
    public string Address { get; set; }
    
    [DataFormDisplayOptions(RowOrder = 0)]
    public string Name { get; set; } // Displays first
}
```

### Using Event-Based Ordering

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem != null)
    {
        switch (e.DataFormItem.FieldName)
        {
            case "Email":
                e.DataFormItem.RowOrder = 0;
                break;
            case "Name":
                e.DataFormItem.RowOrder = 1;
                break;
            case "Phone":
                e.DataFormItem.RowOrder = 2;
                break;
        }
    }
};
```

**Best practice:** Use consistent ordering approach (all attributes OR all event-based) for maintainability.

## Editor Visibility

Control field visibility dynamically:

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "InternalId")
    {
        e.DataFormItem.IsVisible = false; // Hide field
    }
};
```

**Dynamic visibility based on conditions:**

```csharp
public class ConditionalForm : INotifyPropertyChanged
{
    private bool _isBusinessAccount;
    
    public bool IsBusinessAccount
    {
        get => _isBusinessAccount;
        set
        {
            _isBusinessAccount = value;
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(nameof(IsBusinessAccount)));
        }
    }
    
    public string CompanyName { get; set; }
    
    public event PropertyChangedEventHandler PropertyChanged;
}

// In page code
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "CompanyName")
    {
        var model = dataForm.DataObject as ConditionalForm;
        e.DataFormItem.IsVisible = model?.IsBusinessAccount ?? false;
    }
};

// Update visibility when checkbox changes
var model = dataForm.DataObject as ConditionalForm;
if (model != null)
{
    model.PropertyChanged += (s, e) =>
    {
        if (e.PropertyName == nameof(ConditionalForm.IsBusinessAccount))
        {
            // Refresh DataForm to apply visibility changes
            dataForm.Reload();
        }
    };
}
```

## Programmatic Scrolling

Scroll to a specific field programmatically:

```csharp
// Scroll to field by property name
dataForm.ScrollTo("EmailAddress");
```

**Use cases:**
- Scroll to first validation error after form submission
- Focus on specific field after user action
- Navigate to field from external link/button

**Example: Scroll to validation error**

```csharp
private void SubmitButton_Clicked(object sender, EventArgs e)
{
    bool isValid = dataForm.Validate();
    
    if (!isValid)
    {
        // Get first invalid field
        var firstInvalidItem = dataForm.Items
            .FirstOrDefault(item => !item.IsValid);
        
        if (firstInvalidItem != null)
        {
            dataForm.ScrollTo(firstInvalidItem.FieldName);
        }
    }
}
```

**Animation:** Scrolling is animated by default for smooth UX.

## Troubleshooting

### Grid Layout Not Working

**Problem:** Fields not arranging in columns despite `ColumnCount` set.

**Solutions:**
```csharp
// 1. Ensure ColumnCount is set before DataObject
dataForm.ColumnCount = 2;
dataForm.DataObject = new MyModel(); // Set after ColumnCount

// 2. Check if fields are in groups
// ColumnCount doesn't apply to groups automatically
// See grouping.md for group-specific column layouts

// 3. Verify AutoGenerateItems
dataForm.AutoGenerateItems = true; // Must be true
```

### Label Width Not Applying

**Problem:** `LabelWidth` property has no effect.

**Solutions:**
```csharp
// LabelWidth only works with Left label position
dataForm.DefaultLayoutSettings.LabelPosition = DataFormLabelPosition.Left;
dataForm.DefaultLayoutSettings.LabelWidth = new DataFormItemLength(0.3, DataFormItemLengthUnitType.Star);

// Top label position ignores LabelWidth:
// ❌ Won't work
dataForm.DefaultLayoutSettings.LabelPosition = DataFormLabelPosition.Top;
dataForm.DefaultLayoutSettings.LabelWidth = new DataFormItemLength(200, DataFormItemLengthUnitType.Absolute);
```

### Row/Column Span Not Visible

**Problem:** Field with `RowSpan` or `ColumnSpan` not displaying correctly.

**Solutions:**
```csharp
// 1. Ensure ColumnCount is set for ColumnSpan
dataForm.ColumnCount = 2; // Required for ColumnSpan to work

// 2. Check if span exceeds available rows/columns
[DataFormDisplayOptions(ColumnSpan = 3)] // ❌ If ColumnCount = 2
[DataFormDisplayOptions(ColumnSpan = 2)] // ✅ Valid for ColumnCount = 2

// 3. Verify editor height for RowSpan
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "Description")
    {
        e.DataFormItem.RowSpan = 2;
        e.DataFormItem.EditorHeight = 100; // Set sufficient height
    }
};
```

### Field Order Not Changing

**Problem:** `[Display(Order)]` or `RowOrder` not affecting field sequence.

**Solutions:**
```csharp
// 1. Don't mix Display.Order and DataFormDisplayOptions.RowOrder
// Choose one approach consistently

// Using Display.Order (all fields must have Order):
[Display(Order = 0)]
public string Field1 { get; set; }

[Display(Order = 1)]
public string Field2 { get; set; }

// ❌ Don't mix with:
[DataFormDisplayOptions(RowOrder = 2)]
public string Field3 { get; set; }

// 2. Ensure all fields have order specified
// If some fields lack Order, they appear in property declaration order
```

### LeadingView Icon Not Showing

**Problem:** Custom icon in `LeadingView` not displaying.

**Solutions:**
```csharp
// 1. Verify font registration in MauiProgram.cs
public static MauiApp CreateMauiApp()
{
    var builder = MauiApp.CreateBuilder();
    builder
        .UseMauiApp<App>()
        .ConfigureFonts(fonts =>
        {
            fonts.AddFont("FontAwesome.ttf", "FontAwesome"); // Must be registered
        });
    return builder.Build();
}

// 2. Check font file is in Resources/Fonts/ folder

// 3. Verify glyph code is correct
e.DataFormItem.LeadingView = new Label
{
    Text = "\uf007", // Correct FontAwesome glyph code
    FontFamily = "FontAwesome" // Must match registered name
};
```

### Editor Height Not Applying

**Problem:** Custom `EditorHeight` not taking effect.

**Solutions:**
```csharp
// Set EditorHeight in GenerateDataFormItem event
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "Description")
    {
        e.DataFormItem.EditorHeight = 120; // Must be in event handler
    }
};

// ❌ Setting directly on DataFormItem after generation won't work
var item = dataForm.Items.First(i => i.FieldName == "Description");
item.EditorHeight = 120; // Too late, won't apply
```

### Scroll To Not Working

**Problem:** `ScrollTo()` method not scrolling to field.

**Solutions:**
```csharp
// 1. Ensure field name matches exactly (case-sensitive)
dataForm.ScrollTo("EmailAddress"); // Property name must match exactly

// 2. Field must be visible
// Check IsVisible is true

// 3. Call after DataForm is fully loaded
await Task.Delay(100); // Give DataForm time to render
dataForm.ScrollTo("EmailAddress");
```

---

**Related Topics:**
- [getting-started.md](getting-started.md) - Basic DataForm setup
- [grouping.md](grouping.md) - Group-specific layout configuration
- [data-annotations.md](data-annotations.md) - Layout-related attributes (RowOrder, ColumnSpan, etc.)
- [dataform-settings.md](dataform-settings.md) - Global DataForm appearance settings
- [floating-label-layout.md](floating-label-layout.md) - Floating label animation and layout
