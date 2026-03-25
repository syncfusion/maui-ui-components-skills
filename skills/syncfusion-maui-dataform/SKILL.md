---
name: syncfusion-maui-dataform
description: Implements Syncfusion .NET MAUI DataForm (SfDataForm). Use when creating data entry forms, edit forms, login forms, registration forms, or contact forms in MAUI. Covers form creation, data validation, editor customization, layout management, data binding, grouping, localization, built-in editors, custom editors, and validation modes.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing .NET MAUI DataForm (SfDataForm)

The Syncfusion .NET MAUI DataForm (SfDataForm) is a comprehensive form builder control that automatically generates data entry forms from data objects. It provides built-in editors, validation, flexible layouts, and extensive customization options for creating professional data collection and editing interfaces.

## When to Use This Skill

Use this skill when you need to:
- **Create data entry forms** (login, registration, contact, employee forms, etc.)
- **Edit data objects** with automatic form generation from model classes
- **Validate user input** with built-in or custom validation rules
- **Customize form editors** (text, numeric, date, dropdown, etc.)
- **Layout forms** in linear or grid arrangements
- **Group form fields** with collapsible sections
- **Localize forms** for multiple languages
- **Support RTL layouts** for right-to-left languages
- **Implement floating label designs** for modern UIs
- **Handle form commits** with different modes (LostFocus, PropertyChanged, Manual)

**Typical scenarios:**
- User registration and profile editing
- Business data entry (orders, invoices, contacts)
- Settings and configuration screens
- Survey and questionnaire forms
- Search and filter forms

## Component Overview

**Key Features:**
- **Auto-generation:** Automatically creates form editors from data object properties
- **10+ Built-in Editors:** Text, numeric, date, time, dropdown, checkbox, switch, masked entry, and more
- **Validation:** Built-in support for data annotations and custom validation
- **Flexible Layouts:** Linear or grid layouts with grouping support
- **Customization:** Custom editors, styling, and behavior modification
- **Localization:** Multi-language support with resource files
- **Accessibility:** RTL support and screen reader compatibility


## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

Learn the fundamentals of setting up and using DataForm:
- NuGet package installation for Visual Studio, VS Code, and Rider
- Handler registration in MauiProgram.cs
- Creating data objects and view models
- Binding the DataObject property
- First DataForm implementation
- DataForm sizing in StackLayout containers
- Basic configuration and setup

**When to read:** Starting a new project with DataForm, or need installation guidance.

---

### Built-in Editors
📄 **Read:** [references/built-in-editors.md](references/built-in-editors.md)

Understand the 10+ built-in editor types and automatic generation:
- Text editors (Text, Multiline, Password)
- Numeric editor (SfNumericEntry)
- Date and Time editors (DatePicker, TimePicker)
- Boolean editors (CheckBox, Switch)
- Selection editors (Picker, ComboBox, AutoComplete, RadioGroup)
- MaskedText editor for phone numbers and credit cards
- Editor generation rules based on data types
- DataFormItem types and input control mapping
- DataType attributes for editor selection

**When to read:** Need to understand which editor is generated for each property type, or want to see available editor options.

---

### Custom Editors
📄 **Read:** [references/custom-editors.md](references/custom-editors.md)

Learn to configure and create custom editors:
- Explicit editor configuration (switching between editor types)
- Changing editor types for properties
- Custom editor view implementation
- ItemManager customization
- InitializeDataEditor method override
- Custom control integration
- Editor property mapping and binding
- Advanced editor customization scenarios

**When to read:** Need to change the default editor for a property, or implement a completely custom editor control.

---

### Validation
📄 **Read:** [references/validation.md](references/validation.md)

Implement comprehensive data validation:
- Built-in validation attributes (Required, StringLength, Range, EmailAddress, etc.)
- Data annotation validation
- Validation modes (LostFocus, PropertyChanged, Manual)
- DataFormDateRange attribute for date validation
- Custom validation logic implementation
- Validation error display and styling
- Programmatic validation with Validate() method
- ValidationLabel customization
- Real-time vs on-submit validation

**When to read:** Need to validate form input, show error messages, or implement custom validation rules.

---

### Data Annotations
📄 **Read:** [references/data-annotations.md](references/data-annotations.md)

Configure DataForm behavior declaratively with attributes:
- Display attributes (Name, Prompt, GroupName, AutoGenerateField)
- Order attribute for field ordering
- Formatting attributes (DisplayFormat)
- ReadOnly and Editable attributes
- DataType attribute for editor type selection
- Visibility control attributes
- Custom attribute implementation
- Attribute combinations and precedence rules

**When to read:** Want to configure DataForm using attributes on model properties, or need to control field generation.

---

### Layout
📄 **Read:** [references/layout.md](references/layout.md)

Organize form fields with flexible layout options:
- Linear layout (one field per row, default)
- Grid layout with ColumnCount property
- Label positioning (Left, Top)
- ItemsOrderInRow for custom column ordering
- Row and column span configuration
- Layout customization techniques
- Responsive layout strategies
- Per-group layout configuration

**When to read:** Need to arrange form fields in multiple columns, or customize the form layout structure.

---

### Grouping
📄 **Read:** [references/grouping.md](references/grouping.md)

Group related form fields together:
- GroupName attribute for field grouping
- Group header customization
- Collapsible/expandable groups
- Per-group layout (linear vs grid)
- Group ordering and appearance
- GenerateDataFormItem event for dynamic groups
- Nested grouping strategies

**When to read:** Need to organize form fields into logical sections or collapsible groups.

---

### DataForm Settings
📄 **Read:** [references/dataform-settings.md](references/dataform-settings.md)

Configure global DataForm properties and appearance:
- AutoGenerateItems property (auto vs manual generation)
- Items collection for manual item creation
- Label settings (width, position, styling)
- Editor height and spacing configuration
- Padding and margin properties
- Background and visual appearance
- ItemManager customization for global behavior
- Theme integration

**When to read:** Need to customize the overall appearance or behavior of the DataForm control.

---

### Editing Behavior
📄 **Read:** [references/editing.md](references/editing.md)

Control when and how data is committed:
- Commit modes (LostFocus, PropertyChanged, Manual)
- CommitMode property configuration
- Programmatic commit with Commit() method
- Read-only fields and disabled editors
- ValueChanged event handling
- Updating data objects programmatically
- Two-way data binding behavior
- Form reset and cancel operations

**When to read:** Need to control when form changes are saved to the data object, or implement save/cancel logic.

---

### Floating Label Layout
📄 **Read:** [references/floating-label-layout.md](references/floating-label-layout.md)

Implement modern floating label designs:
- Floating label feature overview
- Enabling floating labels on editors
- Label animation behavior (float on focus)
- Float mode configuration options
- Styling floating labels
- Best practices for floating label UX
- Compatibility with different editor types

**When to read:** Want to implement Material Design-style floating labels for a modern look.

---

### Localization
📄 **Read:** [references/localization.md](references/localization.md)

Support multiple languages in forms:
- Localization overview and setup
- Resource file (.resx) configuration
- Culture-specific strings and messages
- Localized error messages
- Localized labels and prompts
- Runtime language switching
- Platform-specific localization considerations

**When to read:** Need to create forms in multiple languages or support international users.

---

### Right-to-Left Support
📄 **Read:** [references/right-to-left.md](references/right-to-left.md)

Implement RTL layout for Arabic, Hebrew, and other RTL languages:
- RTL support overview
- Enabling RTL layout with FlowDirection
- RTL behavior for editors and controls
- RTL for labels and validation messages
- Cultural considerations for RTL
- Testing and debugging RTL layouts

**When to read:** Supporting right-to-left languages like Arabic or Hebrew.

---

