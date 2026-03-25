---
name: syncfusion-maui-numeric-entry
description: Implements Syncfusion .NET MAUI Numeric Entry control. Use when working with numeric input with validation, formatting (currency, percentage, decimal), value increment/decrement buttons, or range restrictions. Ideal for price inputs, quantity selectors, percentage calculators, or numeric data entry with custom formats.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing .NET MAUI Numeric Entry

The Syncfusion .NET MAUI Numeric Entry control is designed to deliver a user-friendly and advanced input experience for numeric data. It supports a broad range of numeric formats, including currency, percentages, decimals, and other configurable number types. With its rich set of features, the control enhances the overall user experience while simplifying numeric input validation and formatting.

## When to Use This Skill

Use this skill when users need to:

- Accept numeric input with validation and formatting
- Create currency, percentage, or decimal input fields
- Implement value increment/decrement with up/down buttons
- Restrict numeric values within a minimum and maximum range
- Format numeric values with culture-specific patterns
- Provide numeric input with keyboard shortcuts (arrow keys, page keys)
- Build price entry fields, quantity selectors, or rating systems
- Create age inputs, discount calculators, or measurement fields
- Implement read-only numeric displays with button controls
- Add modern translucent glass effects to numeric inputs

## Component Overview

The **Syncfusion .NET MAUI Numeric Entry (SfNumericEntry)** is a specialized input control designed for numeric data entry with advanced features:

### Key Capabilities

- **Numeric Validation**: Restricts alphabetic input, validates on Enter key or focus loss
- **Format Support**: Currency (C), Percentage (P), Decimal (N), and custom formats
- **Value Controls**: Up/down buttons, keyboard arrows, mouse scrolling for value changes
- **Range Restrictions**: Enforce minimum and maximum value constraints
- **Customization**: Full control over appearance, fonts, colors, borders, and buttons
- **Culture Support**: Adapt to different regional number formats
- **Accessibility**: WCAG compliance with AutomationId support


## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

**When to read:** Setting up Numeric Entry for the first time, need basic implementation

**Topics covered:**
- Create .NET MAUI project
- Install Syncfusion.Maui.Inputs NuGet package
- Register handler in MauiProgram.cs with ConfigureSyncfusionCore()
- Add namespace and basic Numeric Entry control (XAML/C#)
- Edit values with validation on Enter key or focus loss
- Change number format with CustomFormat property
- Accept or restrict null values with AllowNull
- UI customization (PlaceholderColor, TextColor, Font properties)
- Caret and selection control (CursorPosition, SelectionLength)
- Methods: Focus(), Unfocus()
- Events: Focused, Unfocused with FocusEventArgs

### Basic Features and Configuration
📄 **Read:** [references/basic-features.md](references/basic-features.md)

**When to read:** Configuring placeholder, clear button, events, borders, or text alignment

**Topics covered:**
- Setting placeholder text with Placeholder property
- Clear button visibility (ShowClearButton) and color (ClearButtonColor)
- Custom clear button paths with ClearButtonPath
- Value change notifications with ValueChanged event
- Value change modes (OnLostFocus, OnKeyFocus)
- Completed event (return key pressed)
- ClearButtonClicked event
- Border customization (Stroke, ShowBorder)
- Text alignment (HorizontalTextAlignment, VerticalTextAlignment)
- Select all text on focus with SelectAllOnFocus
- Keyboard return type configuration with ReturnType
- Return commands (ReturnCommand, ReturnCommandParameter)
- AutomationId for UI testing and accessibility

### Value Formatting
📄 **Read:** [references/formatting.md](references/formatting.md)

**When to read:** Need to format values as currency, percentage, or custom numeric formats

**Topics covered:**
- Currency format (C2): $1,234.56
- Percentage format (P2): 45.50%
- Decimal format (N2): 1,234.56
- Custom format strings with 0 and # specifiers
- Integer digit formatting with zero placeholder
- Fractional digit formatting
- Custom format combinations (e.g., "$00.00##")
- Culture-based formatting with Culture property
- Percentage display modes (Value vs Compute)
- Maximum decimal digits with MaximumNumberDecimalDigits

### Value Restrictions
📄 **Read:** [references/restrictions.md](references/restrictions.md)

**When to read:** Need to enforce minimum/maximum values or prevent null values

**Topics covered:**
- Restrict null values with AllowNull property
- Behavior when AllowNull is false (returns 0 or Minimum)
- Restrict value within range using Minimum and Maximum
- Default values (double.MinValue, double.MaxValue)
- Prevent text editing with IsEditable property
- Value changes still allowed via buttons, keys, and scroll
- Range validation and edge cases

### UpDown Buttons
📄 **Read:** [references/updown-buttons.md](references/updown-buttons.md)

**When to read:** Implementing increment/decrement buttons or keyboard/mouse value changes

**Topics covered:**
- Increase/decrease with keyboard arrows and Page keys
- SmallChange (arrow keys, mouse scroll, up/down buttons)
- LargeChange (Page Up/Down keys)
- UpDown button placement modes:
  - Hidden (default, no buttons shown)
  - Inline (horizontal buttons)
  - InlineVertical (vertical stacked buttons)
- Button alignment (Left, Right, Both)
- Button order (UpThenDown, DownThenUp)
- Button color customization (UpDownButtonColor)
- Custom button templates (UpButtonTemplate, DownButtonTemplate)
- Auto-reverse behavior (restart at min/max with AutoReverse)

### Liquid Glass Effect
📄 **Read:** [references/liquid-glass-effect.md](references/liquid-glass-effect.md)

**When to read:** Implementing modern translucent glass design (.NET 10, iOS 26+, macOS 26+)

**Topics covered:**
- Liquid Glass Effect overview
- Wrap Numeric Entry in SfGlassEffectView
- Set Background to Transparent for glass effect
- Configure EffectType and EnableShadowEffect
- Platform requirements and limitations

