---
name: syncfusion-maui-picker
description: Implements Syncfusion .NET MAUI Picker (SfPicker) control. Use when working with picker controls, item pickers, selection pickers, multi-column pickers, or dialog pickers in .NET MAUI applications. This skill covers installation, configuration, modes (Default, Dialog, RelativeDialog), header/footer customization, column handling, data binding, events, and accessibility.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing .NET MAUI Picker (SfPicker)

The Syncfusion .NET MAUI Picker (SfPicker) control allows users to select items from a customizable picker interface with support for multiple columns, different display modes, and extensive customization options. It's ideal for implementing date pickers, time pickers, color pickers, country pickers, and any scenario requiring item selection from a scrollable list.

## When to Use This Skill

Use this skill when you need to:

- **Implement item selection interfaces** in .NET MAUI applications using Syncfusion Picker
- **Create custom pickers** such as date pickers, time pickers, color pickers, or country pickers
- **Display picker controls** in Default, Dialog, or RelativeDialog modes
- **Configure multi-column pickers** for complex data selection scenarios
- **Customize picker appearance** including headers, footers, selection views, and column headers
- **Bind data** to picker columns using ItemsSource and ObservableCollection
- **Handle picker events** such as SelectionChanged, OkButtonClicked, or CancelButtonClicked
- **Implement accessibility features** for picker controls
- **Localize picker content** for multi-language support
- **Apply advanced features** like looping, item templates, or liquid glass effects

## Component Overview

The .NET MAUI Picker (SfPicker) provides:

- **Multiple display modes**: Default (inline), Dialog (popup), and RelativeDialog (positioned popup)
- **Multi-column support**: Display multiple columns with independent data sources
- **Customizable views**: Header, footer, selection view, and column headers
- **Rich data binding**: Support for simple collections and complex objects
- **Event-driven architecture**: Events and commands for user interactions
- **Accessibility support**: WCAG compliance and screen reader compatibility
- **Localization**: Multi-language and culture-specific formatting
- **Advanced customization**: Item templates, styling, and visual effects

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

When the user needs to:
- Install and set up the Syncfusion .NET MAUI Picker
- Add the NuGet package (Syncfusion.Maui.Picker)
- Register the Syncfusion handler (ConfigureSyncfusionCore)
- Create a basic picker implementation in XAML or C#
- Set initial height and width properties

### Picker Modes and Display
📄 **Read:** [references/picker-modes.md](references/picker-modes.md)

When the user needs to:
- Choose between Default, Dialog, or RelativeDialog modes
- Display picker as a popup dialog
- Position picker relative to specific UI elements
- Configure RelativePosition options (AlignTop, AlignTopLeft, AlignBottomRight, etc.)
- Set RelativeView for custom positioning
- Control popup size with PopupWidth and PopupHeight
- Programmatically open/close picker using IsOpen property

### Column Configuration
📄 **Read:** [references/columns.md](references/columns.md)

When the user needs to:
- Add single or multiple columns to the picker
- Use DisplayMemberPath for displaying specific object properties
- Customize column width
- Set SelectedIndex and SelectedItem properties
- Bind ItemsSource to data collections
- Configure column divider color for multi-column pickers
- Manage dynamic column addition and removal

### Header, Footer, and Selection Views
📄 **Read:** [references/header-footer-selection.md](references/header-footer-selection.md)

When the user needs to:
- Add and customize header view (PickerHeaderView)
- Set header text and height
- Configure footer view with OK and Cancel buttons
- Enable/disable ShowOkButton property
- Customize footer button text (OkButtonText, CancelButtonText)
- Style selection view appearance
- Use IsSelectionImmediate for instant selection updates

### Column Headers
📄 **Read:** [references/column-headers.md](references/column-headers.md)

When the user needs to:
- Add column header view (PickerColumnHeaderView)
- Set HeaderText property for individual columns
- Configure column header height
- Customize column header styling
- Implement multi-column header layouts

### Populating Items and Data Binding
📄 **Read:** [references/populating-items.md](references/populating-items.md)

When the user needs to:
- Bind ItemsSource to ObservableCollection
- Create and populate data sources
- Configure binding context in XAML and C#
- Bind complex objects with custom properties
- Handle dynamic data updates and refreshes
- Implement MVVM data binding patterns

### Events and Commands
📄 **Read:** [references/events-commands.md](references/events-commands.md)

When the user needs to:
- Handle Opened, Closing, and Closed events
- Respond to SelectionChanged events
- Process OkButtonClicked and CancelButtonClicked events
- Use SelectionChangedCommand with MVVM
- Implement AcceptCommand and DeclineCommand
- Access event arguments and parameters
- Cancel picker closing with Closing event

### Customization and Styling
📄 **Read:** [references/customization.md](references/customization.md)

When the user needs to:
- Apply item templates for custom item appearance
- Customize visual styling (colors, fonts, backgrounds)
- Configure text styles and formatting
- Implement visual states
- Apply custom themes and styles

### Accessibility
📄 **Read:** [references/accessibility.md](references/accessibility.md)

When the user needs to:
- Ensure WCAG compliance
- Implement keyboard navigation support
- Configure screen reader compatibility
- Add ARIA attributes for accessibility
- Manage focus behavior
- Follow accessibility best practices

### Localization
📄 **Read:** [references/localization.md](references/localization.md)

When the user needs to:
- Implement multi-language support
- Set up resource files for localization
- Configure culture-specific formatting
- Handle right-to-left (RTL) languages

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)

When the user needs to:
- Enable looping (circular scrolling)
- Configure picker text display modes
- Implement data template selectors
- Apply liquid glass effects
- Optimize picker performance
- Handle complex custom scenarios
- Troubleshoot edge cases

