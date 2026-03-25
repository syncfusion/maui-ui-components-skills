---
name: syncfusion-maui-combobox
description: Implements Syncfusion .NET MAUI ComboBox (SfComboBox) control. Use when working with ComboBox, dropdown selection, editable dropdown, searchable dropdown, or filterable dropdown in .NET MAUI. Covers basic ComboBox setup, editable/non-editable modes, single/multiple selection, filtering and searching, custom UI styling, and AI-powered smart search.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing .NET MAUI ComboBox

The Syncfusion .NET MAUI ComboBox (SfComboBox) is a powerful selection control that allows users to type a value or choose an option from a list of predefined options. It combines the functionality of a dropdown list with an editable text box, supporting data binding, filtering, searching, single/multiple selection, and extensive customization options.

## When to Use This Skill

Use this skill when the user needs to:

- **Implement dropdown selection controls** with editable or non-editable text input
- **Enable searchable dropdowns** with filtering and auto-complete functionality
- **Support multiple selection modes** with token or delimiter display
- **Bind data from collections** to dropdown lists with custom display formatting
- **Apply custom filtering logic** including AI-powered smart search
- **Customize dropdown UI** with headers, footers, item templates, and styling
- **Highlight matched text** during search operations
- **Implement load-more functionality** for large datasets
- **Create accessible selection controls** with proper automation support

## Component Overview

**Key Capabilities:**
- **Editable & Non-Editable Modes:** Toggle between allowing free text input or dropdown-only selection
- **Single & Multiple Selection:** Support for single item or multi-item selection with token/delimiter display
- **Filtering & Searching:** Built-in text filtering with StartsWith/Contains modes and custom filter logic
- **Data Binding:** Bind to any IEnumerable source with DisplayMemberPath and TextMemberPath
- **UI Customization:** Extensive styling options for dropdown, items, tokens, headers, footers
- **Smart Features:** AI-powered search, text highlighting, load-more, auto-sizing
- **Events & Methods:** Rich event model for selection changes, dropdown state, and user interactions

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and NuGet package setup (Syncfusion.Maui.Inputs)
- Registering Syncfusion Core handler (ConfigureSyncfusionCore)
- Basic ComboBox implementation in XAML and C#
- Data binding with ItemsSource, DisplayMemberPath, TextMemberPath
- Minimal working example

### Editing Modes
📄 **Read:** [references/editing-modes.md](references/editing-modes.md)
- IsEditable property for editable/non-editable modes
- IsClearButtonVisible for clear button control
- Text property for getting/setting input text
- CursorPosition management

### Selection
📄 **Read:** [references/selection.md](references/selection.md)
- Single selection (SelectedItem, SelectedIndex)
- Multiple selection (SelectionMode, SelectedItems)
- Multi-selection display modes (Token, Delimiter)
- TokensWrapMode (Wrap, None)
- SelectedValue and SelectedValuePath
- SelectionChanging and SelectionChanged events
- Programmatic selection and Clear method

### Filtering
📄 **Read:** [references/filtering.md](references/filtering.md)
- Enable filtering with IsFilteringEnabled
- Filter modes (StartsWith, Contains)
- Custom filtering with IComboBoxFilterBehavior
- Implementing GetMatchingIndexes for custom filter logic
- MinimumPrefixCharacters constraint

### Searching
📄 **Read:** [references/searching.md](references/searching.md)
- TextSearchMode configuration (StartsWith, Contains)
- Search based on DisplayMemberPath and TextMemberPath
- Edit mode searching behavior
- Prefix characters constraint (MinimumPrefixCharacters)

### UI Customization
📄 **Read:** [references/ui-customization.md](references/ui-customization.md)
- Placeholder and colors
- Clear button and dropdown icon customization
- Border styling (Stroke, ShowBorder)
- MaxDropDownHeight configuration
- ItemTemplate and DataTemplateSelector for custom item rendering
- Dropdown styling (Background, Stroke, StrokeThickness, CornerRadius, Shadow)
- Dropdown item styling (Font, Color, Height, Padding)
- DropDownPlacement (Top, Bottom, Auto, None)
- DropDownButtonSettings customization
- Token item styling
- Text alignment and cursor position
- ReturnType for keyboard behavior
- ShowSuggestionsOnFocus
- AutomationId support for UI testing

### Header and Footer
📄 **Read:** [references/header-footer.md](references/header-footer.md)
- Adding header view with ShowDropDownHeaderView
- Adding footer view with ShowDropDownFooterView
- Custom templates for headers and footers
- Height configuration

### Text Highlighting
📄 **Read:** [references/highlighting-text.md](references/highlighting-text.md)
- TextHighlightMode (FirstOccurrence, MultipleOccurrence)
- Customizing highlight color and font attributes
- Highlighting in StartsWith and Contains modes

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- AutoSizing with EnableAutoSize
- NoResultsFoundText and NoResultsFoundTemplate
- Load-more functionality (MaximumSuggestion, LoadMoreText, LoadMoreTemplate)
- IsDropDownOpen for programmatic control
- ReturnCommand and ReturnCommandParameter
- Liquid Glass Effect (EnableLiquidGlassEffect)

### Events and Methods
📄 **Read:** [references/events-and-methods.md](references/events-and-methods.md)
- SelectionChanging event (with Cancel option)
- SelectionChanged event (AddedItems, RemovedItems)
- ValueChanged event (OldValue, NewValue)
- Completed event
- DropDownOpening, DropDownOpened, DropDownClosed events
- ClearButtonClicked event
- LoadMoreButtonTapped event
- Clear() method

### AI-Powered Smart Searching
📄 **Read:** [references/ai-smart-searching.md](references/ai-smart-searching.md)
- Integrating Azure OpenAI for intelligent search
- Implementing IComboBoxFilterBehavior for AI filtering
- Prompt engineering for accurate results
- Async filtering with CancellationToken
- Fallback mechanisms (Soundex, Levenshtein algorithms)

