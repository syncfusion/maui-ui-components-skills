# Input and Picker Controls Migration: Xamarin.Forms to .NET MAUI

Migration guide for input and picker controls from Xamarin.Forms to .NET MAUI.

## Table of Contents
- [SfMaskedEntry Migration](#sfmaskedentry-migration)
- [SfComboBox Migration](#sfcombobox-migration)
- [SfAutocomplete Migration](#sfautocomplete-migration)
- [SfPicker Migration](#sfpicker-migration)
- [SfNumericEntry Migration](#sfnumericentry-migration)
- [SfTextInputLayout Migration](#sftextinputlayout-migration)

## SfMaskedEntry Migration

### Component Rename

| Xamarin | MAUI |
|---------|------|
| SfMaskedEdit | SfMaskedEntry |

### Namespace Changes

```csharp
// Xamarin
using Syncfusion.XForms.MaskedEdit;

// MAUI
using Syncfusion.Maui.Inputs;
```

### Migration Example

**Xamarin:**
```xml
<maskedEdit:SfMaskedEdit Mask="000-000-0000"
                         MaskType="Simple"
                         Value="{Binding PhoneNumber}"/>
```

**.NET MAUI:**
```xml
<inputs:SfMaskedEntry Mask="000-000-0000"
                      MaskType="Simple"
                      Value="{Binding PhoneNumber}"/>
```

## SfComboBox Migration

### Namespace Changes

```csharp
// Xamarin
using Syncfusion.XForms.ComboBox;

// MAUI
using Syncfusion.Maui.Inputs;
```

### Key Changes

Most properties maintained with minor updates:
- Improved dropdown behavior
- Enhanced filtering
- Better item template support

### Migration Example

**Xamarin:**
```xml
<comboBox:SfComboBox DataSource="{Binding Countries}"
                     DisplayMemberPath="Name"
                     AllowFiltering="True"
                     IsEditableMode="True"/>
```

**.NET MAUI:**
```xml
<inputs:SfComboBox ItemsSource="{Binding Countries}"
                   DisplayMemberPath="Name"
                   IsFilteringEnabled="True"
                   IsEditable="True"/>
```

**Property Changes:**

| Xamarin | MAUI |
|---------|------|
| `DataSource` | `ItemsSource` |
| `AllowFiltering` | `IsFilteringEnabled` |
| `IsEditableMode` | `IsEditable` |

## SfAutocomplete Migration

### Namespace Changes

```csharp
// Xamarin
using Syncfusion.SfAutoComplete.XForms;

// MAUI
using Syncfusion.Maui.Inputs;
```

### Migration Example

**Xamarin:**
```xml
<autoComplete:SfAutoComplete DataSource="{Binding Cities}"
                             SuggestionMode="StartsWith"
                             MaximumSuggestion="5"/>
```

**.NET MAUI:**
```xml
<inputs:SfAutocomplete ItemsSource="{Binding Cities}"
                       TextSearchMode="StartsWith"
                       MaxDropDownHeight="200"/>
```

**Property Changes:**

| Xamarin | MAUI |
|---------|------|
| `DataSource` | `ItemsSource` |
| `SuggestionMode` | `TextSearchMode` |

## SfPicker Migration

### Namespace Changes

```csharp
// Xamarin
using Syncfusion.XForms.Pickers;

// MAUI
using Syncfusion.Maui.Picker;
```

### Migration Example

**Xamarin:**
```xml
<picker:SfPicker ItemsSource="{Binding Colors}"
                 SelectedItem="{Binding SelectedColor}"
                 ColumnHeaderText="Select Color"/>
```

**.NET MAUI:**
```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.Columns>
        <picker:PickerColumn
            HeaderText="Select Color"
            ItemsSource="{Binding Colors}"
            SelectedIndex="1" />
    </syncfusion:SfPicker.Columns>
</syncfusion:SfPicker>
```

Most of the properties are changed.

## SfNumericEntry Migration

### Component Merge

| Xamarin | MAUI |
|---------|------|
| SfNumericTextBox | SfNumericEntry |
| SfNumericUpDown | SfNumericEntry |

Both Xamarin controls merged into a single MAUI control.

### Namespace Changes

```csharp
// Xamarin
using Syncfusion.SfNumericTextBox.XForms;
using Syncfusion.SfNumericUpDown.XForms;

// MAUI
using Syncfusion.Maui.Inputs;
```

### Migration Example

**Xamarin:**
```xml
<numericTextBox:SfNumericTextBox Value="{Binding Price}"
                                 Minimum="0"
                                 Maximum="10000"
                                 FormatString="C"/>
```

**.NET MAUI:**
```xml
<inputs:SfNumericEntry Value="{Binding Price}"
                       Minimum="0"
                       Maximum="10000"
                       CustomFormat="C"/>
```

**Property Changes:**

| Xamarin | MAUI |
|---------|------|
|`FormatString` | `CustomFormat` |

## SfTextInputLayout Migration

### Namespace Changes

```csharp
// Xamarin
using Syncfusion.XForms.TextInputLayout;

// MAUI
using Syncfusion.Maui.Core;
```

### Migration Example

**Xamarin:**
```xml
<textInputLayout:SfTextInputLayout Hint="Email">
    <Entry Text="{Binding Email}"/>
</textInputLayout:SfTextInputLayout>
```

**.NET MAUI:**
```xml
<inputLayout:SfTextInputLayout Hint="Email">
    <Entry Text="{Binding Email}"/>
</inputLayout:SfTextInputLayout>
```

## Common Migration Patterns

### Data Source Binding

**Xamarin:**
```csharp
comboBox.DataSource = countries;
autoComplete.DataSource = cities;
```

**.NET MAUI:**
```csharp
comboBox.ItemsSource = countries;
autoComplete.ItemsSource = cities;
```

### Input Validation

**Xamarin:**
```csharp
maskedEdit.ValueChanged += (s, e) =>
{
    if (e.Value.Length == 10)
        ValidateInput(e.Value);
};
```

**.NET MAUI:**
```csharp
maskedEntry.ValueChanged += (s, e) =>
{
    if (e.NewValue?.Length == 10)
        ValidateInput(e.NewValue);
};
```

## Troubleshooting

### Issue: SfMaskedEdit not found

**Solution:** Renamed to `SfMaskedEntry`:
```csharp
// Change
using Syncfusion.XForms.MaskedEdit;
SfMaskedEdit edit = new SfMaskedEdit();

// To
using Syncfusion.Maui.Inputs;
SfMaskedEntry entry = new SfMaskedEntry();
```

### Issue: DataSource property not found

**Solution:** Use `ItemsSource`:
```csharp
// Change
comboBox.DataSource = items;

// To
comboBox.ItemsSource = items;
```

### Issue: SfNumericTextBox not found

**Solution:** Use `SfNumericEntry`:
```csharp
// Change
using Syncfusion.SfNumericTextBox.XForms;
SfNumericTextBox textBox = new SfNumericTextBox();

// To
using Syncfusion.Maui.Inputs;
SfNumericEntry entry = new SfNumericEntry();
```

## Next Steps

1. Update NuGet package: `Syncfusion.Maui.Inputs` and `Syncfusion.Maui.Picker`
2. Update namespaces
3. Replace SfMaskedEdit → SfMaskedEntry
4. Replace DataSource → ItemsSource
5. Merge SfNumericTextBox/SfNumericUpDown → SfNumericEntry
6. Test input validation and data binding
