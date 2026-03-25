---
name: syncfusion-maui-autocomplete
description: Implements and configure Syncfusion .NET MAUI Autocomplete (SfAutocomplete) control for intelligent text input with suggestions. Use when implementing autocomplete, autosuggest, type-ahead search, smart search, or filtered dropdowns in .NET MAUI applications. Covers data binding, filtering modes, custom search logic, AI-powered search, multi-selection with tokens, and UI customization.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing .NET MAUI Autocomplete

The Syncfusion .NET MAUI Autocomplete (SfAutocomplete) is a powerful text input control that provides intelligent suggestions as users type, enabling fast data entry and enhanced search experiences with filtering, multi-selection, and customizable UI.

## When to Use This Skill

Use this skill when the user needs to:
- **Search-as-you-type interfaces** - Filter and display suggestions while users type
- **Smart data entry forms** - Autocomplete user input from predefined lists
- **Multi-selection input** - Select multiple items with token/chip display
- **Filtered dropdowns** - Search and filter large datasets efficiently
- **AI-powered search** - Implement intelligent search with Azure OpenAI integration
- **Type-ahead functionality** - Provide predictive text suggestions
- **Searchable combobox alternatives** - Enhanced dropdown with search capabilities
- **Tag input controls** - Multi-selection with visual tokens

## Component Overview

The SfAutocomplete control offers:

- **Intelligent Filtering** - StartsWith and Contains search modes with custom filters
- **Flexible Selection** - Single or multiple selection with token/delimiter display
- **Data Binding** - Bind to any data source with DisplayMemberPath and TextMemberPath
- **Custom Search Logic** - Implement IAutocompleteFilterBehavior for advanced filtering
- **AI Integration** - Azure OpenAI support for smart, context-aware searching
- **Rich Customization** - Templates, styling, headers, footers, and highlighting
- **Performance** - Async loading, pagination with LoadMore, and efficient rendering
- **Accessibility** - AutomationId support, keyboard navigation, screen reader friendly

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

When implementing basic autocomplete functionality:
- Installation and NuGet package setup (Syncfusion.Maui.Inputs)
- Handler registration with ConfigureSyncfusionCore
- Basic control implementation in XAML and C#
- Data binding with ItemsSource, DisplayMemberPath, TextMemberPath
- Placeholder configuration and basic properties
- AutomationId for UI testing

### Searching and Filtering
📄 **Read:** [references/searching-filtering.md](references/searching-filtering.md)

When implementing search and filter logic:
- Search based on DisplayMemberPath and TextMemberPath
- TextSearchMode (StartsWith, Contains) configuration
- MinimumPrefixCharacters for search delay
- Custom filtering with IAutocompleteFilterBehavior
- Custom search highlighting with IAutocompleteSearchBehavior
- Async item loading with cancellation support
- ShowSuggestionsOnFocus behavior

### Selection
📄 **Read:** [references/selection.md](references/selection.md)

When handling item selection:
- Single vs Multiple selection modes
- SelectedItem, SelectedItems properties
- SelectedValue and SelectedValuePath
- MultiSelectionDisplayMode (Token, Delimiter)
- TokensWrapMode (Wrap, None) for token layout
- SelectionChanging and SelectionChanged events
- Clear button configuration
- Clear() method for programmatic clearing

### UI Customization
📄 **Read:** [references/ui-customization.md](references/ui-customization.md)

When customizing appearance and behavior:
- Placeholder styling (text, color)
- Clear button customization (icon, color, path)
- Dropdown styling (height, width, placement, background)
- Item templates and text styling
- Token styling for multi-selection
- Border and stroke customization
- Text alignment and cursor position
- ReturnType and keyboard behavior
- Events (Completed, DropDownOpening, ValueChanged, etc.)

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)

When implementing advanced functionality:
- Header and footer views in dropdown
- Text highlighting (FirstOccurrence, MultipleOccurrence)
- No results found customization
- AutoSizing for dynamic control height
- Liquid Glass Effect for modern UI (.NET 10+)

### AI-Powered Features & LoadMore
📄 **Read:** [references/ai-powered-loadmore.md](references/ai-powered-loadmore.md)

When implementing intelligent search or pagination:
- Azure OpenAI integration for smart search
- Custom filtering with AI service
- Prompt engineering for accurate results
- Offline fallback with Soundex/Levenshtein
- LoadMore feature with MaximumSuggestion
- LoadMore customization (text, template, events)

### Troubleshooting
📄 **Read:** [references/troubleshooting.md](references/troubleshooting.md)

When encountering issues:
- Platform-specific limitations and workarounds
- AOT publishing considerations
- Performance optimization for large datasets
- Common issues and solutions

