# Field Grouping

## Table of Contents
- [Overview](#overview)
- [Creating Groups](#creating-groups)
- [Field Order Within Groups](#field-order-within-groups)
- [Group Layout Configuration](#group-layout-configuration)
- [Group Expand/Collapse](#group-expandcollapse)
- [Group Visibility](#group-visibility)
- [Group Header Customization](#group-header-customization)
- [Troubleshooting](#troubleshooting)

## Overview

The .NET MAUI DataForm supports organizing related fields into collapsible/expandable groups for better organization and user experience.

**When to use grouping:**
- Organize related fields (Personal Info, Address, Payment, etc.)
- Reduce visual clutter in large forms
- Provide progressive disclosure (collapse less-important sections)
- Create logical sections with distinct layouts per group

**Features:**
- Expand/collapse groups by tapping header
- Independent layout configuration per group (linear/grid)
- Customizable group headers (text, background, styling)
- Control field order within groups
- Show/hide entire groups dynamically

## Creating Groups

Group fields using the `[Display]` attribute's `GroupName` property or via the `GenerateDataFormItem` event.

### Using Attributes

```csharp
using System.ComponentModel.DataAnnotations;

public class ContactInfo
{
    [Display(GroupName = "Personal Information")]
    public string FirstName { get; set; }
    
    [Display(GroupName = "Personal Information")]
    public string MiddleName { get; set; }
    
    [Display(GroupName = "Personal Information")]
    public string LastName { get; set; }
    
    [Display(GroupName = "Contact Details")]
    public string Email { get; set; }
    
    [Display(GroupName = "Contact Details")]
    public string PhoneNumber { get; set; }
    
    [Display(GroupName = "Contact Details")]
    public string Address { get; set; }
}

var dataForm = new SfDataForm
{
    DataObject = new ContactInfo()
};
```

**Result:**
- Group 1: "Personal Information" (FirstName, MiddleName, LastName)
- Group 2: "Contact Details" (Email, PhoneNumber, Address)

### Using Event

Assign groups dynamically in the `GenerateDataFormItem` event:

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem != null)
    {
        if (e.DataFormItem.FieldName == "FirstName" || 
            e.DataFormItem.FieldName == "MiddleName" || 
            e.DataFormItem.FieldName == "LastName")
        {
            e.DataFormItem.GroupName = "Personal Information";
        }
        else if (e.DataFormItem.FieldName == "Email" || 
                 e.DataFormItem.FieldName == "PhoneNumber" || 
                 e.DataFormItem.FieldName == "Address")
        {
            e.DataFormItem.GroupName = "Contact Details";
        }
    }
};
```

### Renaming Group Headers

Change the displayed group name in the event:

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem != null)
    {
        if (e.DataFormItem.GroupName == "PersonalInfo")
        {
            e.DataFormItem.GroupName = "Personal Information"; // User-friendly name
        }
    }
};
```

## Field Order Within Groups

Control the order of fields within each group using `Display.Order` or `DataFormDisplayOptions.RowOrder`.

### Using Display.Order Attribute

```csharp
using System.ComponentModel.DataAnnotations;

public class ContactInfo
{
    [Display(GroupName = "Name", Order = 0)]
    public string FirstName { get; set; } // First in group
    
    [Display(GroupName = "Name", Order = 1)]
    public string MiddleName { get; set; } // Second in group
    
    [Display(GroupName = "Name", Order = 2)]
    public string LastName { get; set; } // Third in group
}
```

### Using DataFormDisplayOptions.RowOrder

```csharp
using Syncfusion.Maui.DataForm;
using System.ComponentModel.DataAnnotations;

public class ContactInfo
{
    [Display(GroupName = "Details")]
    [DataFormDisplayOptions(RowOrder = 0)]
    public string FirstName { get; set; }
    
    [Display(GroupName = "Details")]
    [DataFormDisplayOptions(RowOrder = 2)]
    public string LastName { get; set; }
    
    [Display(GroupName = "Details")]
    [DataFormDisplayOptions(RowOrder = 1)]
    public string MiddleName { get; set; }
}
```

**Result order within "Details" group:** FirstName → MiddleName → LastName

### Using Event

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem != null && e.DataFormItem.GroupName == "Name")
    {
        switch (e.DataFormItem.FieldName)
        {
            case "FirstName":
                e.DataFormItem.RowOrder = 0;
                break;
            case "MiddleName":
                e.DataFormItem.RowOrder = 1;
                break;
            case "LastName":
                e.DataFormItem.RowOrder = 2;
                break;
        }
    }
};
```

## Group Layout Configuration

Each group can have its own layout (linear or grid) independent of other groups and non-grouped fields.

### Linear Layout (Default)

By default, groups use linear layout (one field per row):

```csharp
public class RegistrationForm
{
    [Display(GroupName = "Login Credentials")]
    public string Username { get; set; }
    
    [Display(GroupName = "Login Credentials")]
    public string Password { get; set; }
    
    [Display(GroupName = "Login Credentials")]
    public string ConfirmPassword { get; set; }
}

// No additional configuration needed for linear layout
```

### Grid Layout Per Group

Set `ColumnCount` on specific groups in the `GenerateDataFormItem` event:

```csharp
public class ContactInfo
{
    [Display(GroupName = "Name")]
    public string FirstName { get; set; }
    
    [Display(GroupName = "Name")]
    public string MiddleName { get; set; }
    
    [Display(GroupName = "Name")]
    public string LastName { get; set; }
    
    [Display(GroupName = "Contact")]
    public string Address { get; set; }
    
    [Display(GroupName = "Contact")]
    public string PhoneNumber { get; set; }
}

// Configure group layouts
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormGroupItem != null)
    {
        if (e.DataFormGroupItem.Name == "Name")
        {
            e.DataFormGroupItem.ColumnCount = 3; // 3 columns for Name group
        }
        else if (e.DataFormGroupItem.Name == "Contact")
        {
            e.DataFormGroupItem.ColumnCount = 2; // 2 columns for Contact group
        }
    }
};
```

**Result:**
- **Name group:** FirstName | MiddleName | LastName (3 columns)
- **Contact group:** Address | PhoneNumber (2 columns)

### Mixed Group and Non-Grouped Layouts

```csharp
public class MixedForm
{
    // Non-grouped field
    public string Title { get; set; }
    
    [Display(GroupName = "Details")]
    public string Field1 { get; set; }
    
    [Display(GroupName = "Details")]
    public string Field2 { get; set; }
    
    [Display(GroupName = "Details")]
    public string Field3 { get; set; }
}

var dataForm = new SfDataForm
{
    ColumnCount = 2, // Applies to non-grouped fields only
    DataObject = new MixedForm()
};

dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormGroupItem?.Name == "Details")
    {
        e.DataFormGroupItem.ColumnCount = 3; // Group has its own layout
    }
};
```

**Result:**
- Title field: Uses DataForm's `ColumnCount = 2` (non-grouped)
- "Details" group: 3-column layout (independent)

### Field Order Within Grid Rows

Control column position within a row using `ItemsOrderInRow`:

```csharp
using Syncfusion.Maui.DataForm;
using System.ComponentModel.DataAnnotations;

public class ContactInfo
{
    [Display(GroupName = "Name")]
    [DataFormDisplayOptions(RowOrder = 0, ItemsOrderInRow = 0)]
    public string FirstName { get; set; } // Row 0, Column 0
    
    [Display(GroupName = "Name")]
    [DataFormDisplayOptions(RowOrder = 0, ItemsOrderInRow = 2)]
    public string LastName { get; set; } // Row 0, Column 2
    
    [Display(GroupName = "Name")]
    [DataFormDisplayOptions(RowOrder = 0, ItemsOrderInRow = 1)]
    public string MiddleName { get; set; } // Row 0, Column 1
}

dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormGroupItem?.Name == "Name")
    {
        e.DataFormGroupItem.ColumnCount = 3;
    }
};
```

**Result:** FirstName | MiddleName | LastName (ordered by `ItemsOrderInRow`)

**See also:** [layout.md](layout.md) for detailed layout configuration.

## Group Expand/Collapse

Control group expansion state and behavior.

### Default State (Expanded)

By default, all groups are expanded on load:

```csharp
public class DefaultExpandedForm
{
    [Display(GroupName = "Section 1")]
    public string Field1 { get; set; }
    
    [Display(GroupName = "Section 2")]
    public string Field2 { get; set; }
}

// Both groups are expanded by default
```

### Load Group Collapsed

Collapse specific groups initially:

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormGroupItem != null)
    {
        if (e.DataFormGroupItem.Name == "Advanced Settings")
        {
            e.DataFormGroupItem.IsExpanded = false; // Collapsed on load
        }
    }
};
```

**Use case:** Hide advanced/optional sections by default to reduce form complexity.

### Disable Expand/Collapse

Prevent users from expanding/collapsing specific groups:

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormGroupItem != null)
    {
        if (e.DataFormGroupItem.Name == "Required Information")
        {
            e.DataFormGroupItem.AllowExpandCollapse = false; // Always visible, can't collapse
        }
    }
};
```

**Use case:** Keep critical sections always visible (e.g., required fields).

### Programmatically Toggle Expansion

Control group state programmatically:

```csharp
private void CollapseAllGroups()
{
    dataForm.GenerateDataFormItem += (sender, e) =>
    {
        if (e.DataFormGroupItem != null)
        {
            e.DataFormGroupItem.IsExpanded = false;
        }
    };
    
    // Reload DataForm to apply changes
    dataForm.Reload();
}

private void ExpandSpecificGroup(string groupName)
{
    dataForm.GenerateDataFormItem += (sender, e) =>
    {
        if (e.DataFormGroupItem?.Name == groupName)
        {
            e.DataFormGroupItem.IsExpanded = true;
        }
    };
    
    dataForm.Reload();
}
```

## Group Visibility

Show or hide entire groups dynamically.

### Hide Group Completely

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormGroupItem != null)
    {
        if (e.DataFormGroupItem.Name == "Advanced Options")
        {
            e.DataFormGroupItem.IsVisible = false; // Group hidden
        }
    }
};
```

**Result:** "Advanced Options" group and all its fields are hidden.

### Conditional Group Visibility

Show groups based on user role or other conditions:

```csharp
public class UserSettings
{
    public bool IsAdministrator { get; set; }
    
    [Display(GroupName = "Basic Settings")]
    public string Theme { get; set; }
    
    [Display(GroupName = "Admin Settings")]
    public bool AllowUserRegistration { get; set; }
    
    [Display(GroupName = "Admin Settings")]
    public int MaxUsers { get; set; }
}

dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormGroupItem != null)
    {
        var model = dataForm.DataObject as UserSettings;
        
        if (e.DataFormGroupItem.Name == "Admin Settings")
        {
            // Show admin settings only for administrators
            e.DataFormGroupItem.IsVisible = model?.IsAdministrator ?? false;
        }
    }
};
```

### Dynamic Visibility Updates

Update visibility when model changes:

```csharp
public class DynamicForm : INotifyPropertyChanged
{
    private bool _showAdvanced;
    
    public bool ShowAdvanced
    {
        get => _showAdvanced;
        set
        {
            _showAdvanced = value;
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(nameof(ShowAdvanced)));
        }
    }
    
    [Display(GroupName = "Advanced")]
    public string AdvancedOption { get; set; }
    
    public event PropertyChangedEventHandler PropertyChanged;
}

var model = new DynamicForm();
dataForm.DataObject = model;

dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormGroupItem?.Name == "Advanced")
    {
        e.DataFormGroupItem.IsVisible = model.ShowAdvanced;
    }
};

// React to property changes
model.PropertyChanged += (s, e) =>
{
    if (e.PropertyName == nameof(DynamicForm.ShowAdvanced))
    {
        dataForm.Reload(); // Refresh to apply visibility
    }
};
```

## Group Header Customization

Customize group header appearance and content.

### Header Background Color

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormGroupItem != null)
    {
        if (e.DataFormGroupItem.Name == "Personal Information")
        {
            e.DataFormGroupItem.HeaderBackground = Brush.LightBlue;
        }
        else if (e.DataFormGroupItem.Name == "Payment Details")
        {
            e.DataFormGroupItem.HeaderBackground = Brush.LightGreen;
        }
    }
};
```

**Use case:** Color-code groups by category for visual organization.

### Header Text Style

Customize font, color, and formatting:

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormGroupItem != null)
    {
        if (e.DataFormGroupItem.Name == "Important Section")
        {
            e.DataFormGroupItem.HeaderTextStyle = new DataFormTextStyle
            {
                TextColor = Colors.Red,
                FontSize = 16,
                FontAttributes = FontAttributes.Bold
            };
        }
        else
        {
            e.DataFormGroupItem.HeaderTextStyle = new DataFormTextStyle
            {
                TextColor = Colors.Gray,
                FontSize = 14,
                FontAttributes = FontAttributes.Italic
            };
        }
    }
};
```

### Group Header Padding

Control spacing around group editors:

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormGroupItem != null)
    {
        if (e.DataFormGroupItem.Name == "Compact Section")
        {
            e.DataFormGroupItem.ItemsPadding = 10; // Less padding
        }
        else
        {
            e.DataFormGroupItem.ItemsPadding = 20; // More padding
        }
    }
};
```

**`ItemsPadding`:** Distance between group header and editors, or between editors and group borders.

### Custom Header Template

Replace default header with custom XAML template:

```xaml
<dataForm:SfDataForm x:Name="dataForm">
    <dataForm:SfDataForm.GroupHeaderTemplate>
        <DataTemplate>
            <Grid BackgroundColor="MediumPurple" Padding="10">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto"/>
                    <ColumnDefinition Width="*"/>
                </Grid.ColumnDefinitions>
                
                <Image Grid.Column="0"
                       Source="group_icon.png"
                       WidthRequest="24"
                       HeightRequest="24"/>
                
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontSize="16"
                       FontAttributes="Bold"
                       TextColor="White"
                       VerticalOptions="Center"
                       Margin="10,0,0,0"/>
            </Grid>
        </DataTemplate>
    </dataForm:SfDataForm.GroupHeaderTemplate>
</dataForm:SfDataForm>
```

**BindingContext:** The template's `BindingContext` is the `DataFormGroupItem`, so you can bind to:
- `Name`: Group name
- `IsExpanded`: Expansion state
- `IsVisible`: Visibility state

**Advanced template with icons:**

```xaml
<dataForm:SfDataForm.GroupHeaderTemplate>
    <DataTemplate>
        <Frame BackgroundColor="LightGray" 
               Padding="12" 
               Margin="0,5,0,5"
               CornerRadius="8"
               HasShadow="True">
            <StackLayout Orientation="Horizontal">
                <!-- Expand/collapse icon based on IsExpanded -->
                <Label Text="{Binding IsExpanded, Converter={StaticResource BoolToIconConverter}}"
                       FontFamily="FontAwesome"
                       FontSize="16"
                       VerticalOptions="Center"/>
                
                <Label Text="{Binding Name}"
                       FontSize="18"
                       FontAttributes="Bold"
                       TextColor="Black"
                       VerticalOptions="Center"
                       Margin="10,0,0,0"/>
            </StackLayout>
        </Frame>
    </DataTemplate>
</dataForm:SfDataForm.GroupHeaderTemplate>
```

**Note:** You'll need to create a `BoolToIconConverter` to convert `IsExpanded` to appropriate icon glyphs.

## Troubleshooting

### Groups Not Appearing

**Problem:** Fields not grouped despite `GroupName` attribute.

**Solutions:**
```csharp
// 1. Verify AutoGenerateItems is enabled
dataForm.AutoGenerateItems = true; // Required for attributes

// 2. Check GroupName spelling consistency
[Display(GroupName = "ContactInfo")] // ❌ Different spelling
[Display(GroupName = "Contact Info")] // ✅ Consistent spelling

// 3. Ensure fields are in same group
[Display(GroupName = "Personal")] // Group 1
public string FirstName { get; set; }

[Display(GroupName = "Personal")] // Same group
public string LastName { get; set; }
```

### Group ColumnCount Not Working

**Problem:** Group fields not arranging in grid layout despite `ColumnCount` set.

**Solutions:**
```csharp
// ColumnCount must be set on DataFormGroupItem, not SfDataForm
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormGroupItem?.Name == "MyGroup")
    {
        e.DataFormGroupItem.ColumnCount = 2; // ✅ Correct
    }
};

// ❌ This only affects non-grouped fields:
dataForm.ColumnCount = 2;
```

### Field Order Not Changing

**Problem:** Fields not respecting `Display.Order` or `RowOrder`.

**Solutions:**
```csharp
// Ensure all fields in group have Order specified
[Display(GroupName = "Name", Order = 0)]
public string FirstName { get; set; }

[Display(GroupName = "Name", Order = 1)]
public string MiddleName { get; set; }

// ❌ Don't leave some fields without Order:
// public string LastName { get; set; }

[Display(GroupName = "Name", Order = 2)]
public string LastName { get; set; } // ✅ All have Order
```

### Group Header Not Customizing

**Problem:** `HeaderBackground` or `HeaderTextStyle` not applying.

**Solutions:**
```csharp
// Ensure setting properties on DataFormGroupItem, not DataFormItem
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormGroupItem != null) // ✅ Check for group item
    {
        e.DataFormGroupItem.HeaderBackground = Brush.LightBlue;
    }
    
    // ❌ Not on data item:
    // if (e.DataFormItem != null) { ... }
};
```

### IsExpanded Not Working

**Problem:** Setting `IsExpanded = false` doesn't collapse group.

**Solutions:**
```csharp
// Set IsExpanded in GenerateDataFormItem event
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormGroupItem?.Name == "Advanced")
    {
        e.DataFormGroupItem.IsExpanded = false;
    }
};

// To change dynamically, call Reload() after updating:
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormGroupItem?.Name == "Advanced")
    {
        e.DataFormGroupItem.IsExpanded = shouldExpand; // Your condition
    }
};

dataForm.Reload(); // Apply changes
```

### Group Visibility Not Updating

**Problem:** Changing `IsVisible` doesn't hide/show group.

**Solutions:**
```csharp
// Always call Reload() after changing visibility
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormGroupItem?.Name == "Optional")
    {
        e.DataFormGroupItem.IsVisible = showOptional;
    }
};

dataForm.Reload(); // Required to apply visibility changes

// For reactive updates:
model.PropertyChanged += (s, e) =>
{
    if (e.PropertyName == nameof(Model.ShowOptional))
    {
        dataForm.Reload(); // Refresh on model change
    }
};
```

### ItemsOrderInRow Not Working

**Problem:** Fields not arranging in correct column order.

**Solutions:**
```csharp
// 1. Ensure group has ColumnCount set
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormGroupItem?.Name == "Name")
    {
        e.DataFormGroupItem.ColumnCount = 3; // Required
    }
};

// 2. Verify all fields in row have same RowOrder
[DataFormDisplayOptions(RowOrder = 0, ItemsOrderInRow = 0)]
public string Field1 { get; set; }

[DataFormDisplayOptions(RowOrder = 0, ItemsOrderInRow = 1)]
public string Field2 { get; set; }

[DataFormDisplayOptions(RowOrder = 0, ItemsOrderInRow = 2)]
public string Field3 { get; set; }

// ❌ Mixing RowOrder breaks ItemsOrderInRow:
// [DataFormDisplayOptions(RowOrder = 1, ItemsOrderInRow = 1)]
```

### GroupHeaderTemplate Not Showing

**Problem:** Custom `GroupHeaderTemplate` not rendering.

**Solutions:**
```xaml
<!-- 1. Verify template is on SfDataForm, not other controls -->
<dataForm:SfDataForm x:Name="dataForm">
    <dataForm:SfDataForm.GroupHeaderTemplate>
        <DataTemplate>
            <!-- Template content -->
        </DataTemplate>
    </dataForm:SfDataForm.GroupHeaderTemplate>
</dataForm:SfDataForm>

<!-- 2. Check BindingContext is DataFormGroupItem -->
<DataTemplate>
    <Label Text="{Binding Name}"/> <!-- Must bind to DataFormGroupItem properties -->
</DataTemplate>

<!-- 3. Ensure group exists (at least one field has GroupName) -->
```

---

**Related Topics:**
- [getting-started.md](getting-started.md) - Basic DataForm setup
- [layout.md](layout.md) - Layout configuration for fields and groups
- [data-annotations.md](data-annotations.md) - Display.GroupName and DataFormDisplayOptions attributes
- [dataform-settings.md](dataform-settings.md) - Global DataForm appearance settings
