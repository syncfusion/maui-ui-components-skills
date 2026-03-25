# Floating Label Layout

## Overview

Floating label layout provides a modern, material-design-inspired form appearance with animated labels, container styles, and enhanced visual feedback.

**Key features:**
- Animated labels that float above field when focused/filled
- Three container types: Outlined, Filled, None
- Leading and trailing icons/views
- Password visibility toggle
- Assistive labels (hint, helper, validation)
- Customizable colors and styling

**When to use floating labels:**
- Modern, material design aesthetic
- Forms with many fields (saves vertical space)
- Mobile-first applications
- Enhanced UX with visual feedback

## Enabling Floating Label Layout

Set `LayoutType` to `TextInputLayout`:

```xaml
<dataForm:SfDataForm 
    x:Name="dataForm"
    LayoutType="TextInputLayout">
</dataForm:SfDataForm>
```

```csharp
var dataForm = new SfDataForm
{
    LayoutType = DataFormLayoutType.TextInputLayout
};
```

### Per-Field Layout Type

```csharp
dataForm.LayoutType = DataFormLayoutType.Default; // Global default

dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "Email")
    {
        e.DataFormItem.LayoutType = DataFormLayoutType.TextInputLayout; // Override for Email field
    }
};
```

## Container Types

### Outlined (Default)

Rounded border around field with floating label:

```xaml
<dataForm:SfDataForm LayoutType="TextInputLayout">
    <dataForm:SfDataForm.TextInputLayoutSettings>
        <dataForm:TextInputLayoutSettings ContainerType="Outlined"/>
    </dataForm:SfDataForm.TextInputLayoutSettings>
</dataForm:SfDataForm>
```

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem != null)
    {
        e.DataFormItem.LayoutType = DataFormLayoutType.TextInputLayout;
        e.DataFormItem.TextInputLayoutSettings = new TextInputLayoutSettings
        {
            ContainerType = TextInputLayoutContainerType.Outlined
        };
    }
};
```

### Filled

Solid background fill with bottom border:

```xaml
<dataForm:SfDataForm LayoutType="TextInputLayout">
    <dataForm:SfDataForm.TextInputLayoutSettings>
        <dataForm:TextInputLayoutSettings ContainerType="Filled"/>
    </dataForm:SfDataForm.TextInputLayoutSettings>
</dataForm:SfDataForm>
```

### None

No border or background, minimal style:

```xaml
<dataForm:SfDataForm LayoutType="TextInputLayout">
    <dataForm:SfDataForm.TextInputLayoutSettings>
        <dataForm:TextInputLayoutSettings ContainerType="None"/>
    </dataForm:SfDataForm.TextInputLayoutSettings>
</dataForm:SfDataForm>
```

## Leading and Trailing Views

### Leading View (Left Icon)

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "Email")
    {
        e.DataFormItem.LayoutType = DataFormLayoutType.TextInputLayout;
        e.DataFormItem.ShowLeadingView = true;
        e.DataFormItem.LeadingView = new Label
        {
            Text = "\uf0e0", // FontAwesome envelope icon
            FontSize = 18,
            TextColor = Colors.Gray,
            FontFamily = "FontAwesome",
            HeightRequest = 24,
            VerticalTextAlignment = TextAlignment.End
        };
    }
};
```

### Trailing View (Right Icon)

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem?.FieldName == "Search")
    {
        e.DataFormItem.LayoutType = DataFormLayoutType.TextInputLayout;
        e.DataFormItem.ShowTrailingView = true;
        e.DataFormItem.TrailingView = new Label
        {
            Text = "\uf002", // FontAwesome search icon
            FontSize = 18,
            TextColor = Colors.Gray,
            FontFamily = "FontAwesome",
            HeightRequest = 24,
            VerticalTextAlignment = TextAlignment.End
        };
    }
};
```

### View Positioning

```csharp
// Position leading view outside container
e.DataFormItem.LeadingViewPosition = TextInputLayoutViewPosition.Outside;

// Position trailing view inside container
e.DataFormItem.TrailingViewPosition = TextInputLayoutViewPosition.Inside;
```

## Password Visibility Toggle

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem is DataFormPasswordItem passwordItem)
    {
        passwordItem.LayoutType = DataFormLayoutType.TextInputLayout;
        passwordItem.EnablePasswordVisibilityToggle = true; // Default: true
    }
};
```

## Assistive Labels

### Helper Text

```csharp
// Show helper text (placeholder/prompt)
e.DataFormItem.TextInputLayoutSettings = new TextInputLayoutSettings
{
    ShowHelperText = true // Default: true
};

// Hide helper text
e.DataFormItem.TextInputLayoutSettings = new TextInputLayoutSettings
{
    ShowHelperText = false
};
```

## Styling and Customization

### Outline Corner Radius

```csharp
e.DataFormItem.TextInputLayoutSettings = new TextInputLayoutSettings
{
    OutlineCornerRadius = 12 // Default: 4
};
```

### Stroke Colors

```csharp
e.DataFormItem.TextInputLayoutSettings = new TextInputLayoutSettings
{
    Stroke = Colors.Blue, // Unfocused border color
    FocusedStroke = Colors.Green // Focused border color
};
```

### Stroke Thickness

```csharp
e.DataFormItem.TextInputLayoutSettings = new TextInputLayoutSettings
{
    UnfocusedStrokeThickness = 1.0,
    FocusedStrokeThickness = 2.0
};
```

### Text Styles

**Hint label (floating label):**
```csharp
e.DataFormItem.LabelTextStyle = new DataFormTextStyle
{
    TextColor = Colors.Blue,
    FontSize = 14,
    FontAttributes = FontAttributes.Bold
};
```

**Helper text:**
```csharp
e.DataFormItem.TextInputLayoutSettings = new TextInputLayoutSettings
{
    HelperTextStyle = new DataFormTextStyle
    {
        TextColor = Colors.Gray,
        FontSize = 12,
        FontAttributes = FontAttributes.Italic
    }
};
```

**Error message:**
```csharp
e.DataFormItem.ErrorLabelTextStyle = new DataFormTextStyle
{
    TextColor = Colors.Red,
    FontSize = 12,
    FontAttributes = FontAttributes.Bold
};
```

**Valid message:**
```csharp
e.DataFormItem.ValidMessageLabelTextStyle = new DataFormTextStyle
{
    TextColor = Colors.Green,
    FontSize = 12
};
```

## Supported Editors

Floating label layout works with:
- Text editor (Entry)
- Password editor
- MultilineText editor (Editor)
- ComboBox editor
- AutoComplete editor
- Picker editor
- Date picker
- Time picker (not supported on Windows)
- Custom editors

**Not supported:**
- RadioGroup editor
- CheckBox editor
- Switch editor

## Troubleshooting

### Floating Labels Not Showing

**Solutions:**
```csharp
// Ensure LayoutType is TextInputLayout
e.DataFormItem.LayoutType = DataFormLayoutType.TextInputLayout;

// Check label text is set
e.DataFormItem.LabelText = "Field Label"; // Required
```

### Leading/Trailing View Not Visible

**Solutions:**
```csharp
// Enable view visibility
e.DataFormItem.ShowLeadingView = true;
e.DataFormItem.ShowTrailingView = true;

// Ensure view is not null
e.DataFormItem.LeadingView = new Label { Text = "Icon" };
```

### Container Border Not Showing

**Solutions:**
```csharp
// Verify container type
e.DataFormItem.TextInputLayoutSettings = new TextInputLayoutSettings
{
    ContainerType = TextInputLayoutContainerType.Outlined // Not None
};

// Check stroke color contrast
e.DataFormItem.TextInputLayoutSettings = new TextInputLayoutSettings
{
    Stroke = Colors.Black // Ensure visible color
};
```

---

**Related Topics:**
- [getting-started.md](getting-started.md) - Basic DataForm setup
- [layout.md](layout.md) - Standard layout configuration
- [dataform-settings.md](dataform-settings.md) - Global appearance settings
