---
name: syncfusion-maui-masked-entry
description: Implements Syncfusion .NET MAUI Masked Entry (SfMaskedEntry) control for restricted, formatted text input with mask patterns. Use when working with masked input, text masks, formatted entry, phone number input, date input with format, or validation with masks. Covers Simple masks, RegEx masks, prompt characters, and automatic formatting with validation.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing .NET MAUI Masked Entry

The Syncfusion .NET MAUI Masked Entry (SfMaskedEntry) is an advanced input control that restricts and formats user input using customizable mask patterns. It provides automatic validation, formatting, and visual feedback for structured data entry like phone numbers, dates, emails, product keys, and more.

## When to Use This Skill

Use this skill when users need to:

- **Formatted Input Fields**: Phone numbers, dates, times, SSN, credit cards, ZIP codes, product keys
- **Restricted Character Entry**: Only accept specific characters (digits, letters, alphanumeric)
- **Pattern Validation**: Validate input against fixed or variable-length patterns
- **Automatic Formatting**: Apply separators, delimiters, and formatting as user types
- **Culture-Specific Input**: Handle currency symbols, date/time separators, decimal formats
- **Password Fields with Masking**: Show/hide characters with customizable delay
- **Email and URL Validation**: Use RegEx patterns for complex validation
- **Data Entry Forms**: Improve UX with guided input and instant feedback
- **Accessibility Requirements**: Provide clear input expectations with prompts and validation

## Component Overview

**Key Capabilities:**
- Two mask types: **Simple** (fixed patterns) and **RegEx** (flexible patterns)
- Customizable prompt characters showing expected input positions
- Flexible value formatting (include/exclude prompts and literals)
- Real-time validation with multiple validation modes
- Rich event system for monitoring and controlling input
- Extensive customization (colors, fonts, borders, clear button)
- Culture support for international formats
- Password masking with visibility control
- Modern Liquid Glass effect integration
- Full accessibility and automation support

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and NuGet package setup
- Handler registration in MauiProgram.cs
- Basic MaskedEntry implementation
- Setting mask patterns and types
- Configuring prompt characters
- Setting and retrieving values
- First working example

### Mask Types and Patterns
📄 **Read:** [references/mask-types.md](references/mask-types.md)
- Simple mask type and elements (0, 9, #, L, ?, C, A, <, >)
- RegEx mask type and patterns
- Complete element reference tables
- Examples: phone numbers, dates, emails, product keys
- Case conversion and special characters
- When to use Simple vs RegEx
- Performance considerations

### Value Formatting and Display
📄 **Read:** [references/value-formatting.md](references/value-formatting.md)
- ValueMaskFormat options
- ExcludePromptAndLiterals (typed only)
- IncludePrompt (with prompt chars)
- IncludeLiterals (with separators)
- IncludePromptAndLiterals (all characters)
- Use cases for data storage vs display
- Visual comparisons and examples

### Validation and Events
📄 **Read:** [references/validation-and-events.md](references/validation-and-events.md)
- ValidationMode (KeyPress, LostFocus)
- HasError property for validation status
- Focused/Unfocused events
- Focus() and Unfocus() methods
- ValueChanging event (cancelable, with IsValid, NewValue, OldValue, Cancel)
- ValueChanged event (with IsMaskCompleted)
- Completed event (return key)
- ClearButtonClicked event
- Event lifecycle and best practices

### Customization and Styling
📄 **Read:** [references/customization.md](references/customization.md)
- Clear button configuration (visibility, color, custom path)
- Font properties (size, attributes, family)
- Text and placeholder styling
- Border customization (stroke, visibility)
- Background and transparency
- Cursor position control
- Keyboard configuration
- SelectAllOnFocus behavior
- IsReadOnly mode
- Return key handling (ReturnType, ReturnCommand)
- AutomationId support for UI testing
- Platform-specific considerations

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- Culture support (currency, date/time separators, decimal/group separators)
- HidePromptOnLeave for cleaner UI
- Password masking (PasswordChar, PasswordDelayDuration)
- Liquid Glass Effect integration
- RTL support
- Accessibility features
- Performance optimization
- Edge cases and troubleshooting

