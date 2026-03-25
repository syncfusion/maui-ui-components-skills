---
name: syncfusion-maui-text-input-layout
description: Implements Syncfusion .NET MAUI Text Input Layout (SfTextInputLayout). Use when adding decorative elements like floating labels, icons, assistive labels, or containers to Entry, Editor, Autocomplete, or ComboBox controls. Covers text input layout setup, hint labels, helper text, error messages, character counter, and password visibility toggle.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing .NET MAUI Text Input Layout

A comprehensive skill for implementing and customizing the Syncfusion .NET MAUI Text Input Layout control. This control adds decorative elements like floating labels, icons, and assistive labels on top of input views.

## When to Use This Skill

Use this skill when users need to:
- Add floating hint labels to input fields
- Display helper text, error messages, or character counters
- Implement container types (Filled, Outlined, None) for input fields
- Add leading or trailing icons to input views
- Toggle password visibility in Entry controls
- Apply visual states (Normal, Focused, Error) to inputs
- Customize label styles, colors, and fonts
- Support multiple input views (Entry, Editor, Autocomplete, ComboBox, Picker)
- Implement form validation with error states
- Add assistive labels for accessibility
- Create Material Design-style input fields

## Component Overview

**SfTextInputLayout** wraps input controls and provides:
- **Floating Labels** — Hint that animates to top on focus
- **Container Types** — Filled (background), Outlined (border), None
- **Assistive Labels** — Helper text, error messages, character counter
- **Custom Icons** — Leading/trailing views with positioning
- **Visual States** — Normal, Focused, Error, Disabled
- **Password Toggle** — Show/hide password characters
- **RTL Support** — Right-to-left text flow
- **Multiple Input Views** — Entry, Editor, Autocomplete, ComboBox, Picker, DatePicker, TimePicker, Masked Entry, Numeric Entry

**Key Features:**
- Material Design-style input decoration
- Floating hint label animation
- Helper and error text display
- Character counting with max length
- Leading/trailing icon positioning (Inside/Outside)
- Password visibility toggling
- Visual State Manager integration
- Customizable fonts, colors, and strokes
- Reserved space for assistive labels

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- NuGet package installation (`Syncfusion.Maui.Core`)
- Handler registration (`ConfigureSyncfusionCore`)
- Namespace setup (XAML and C#)
- Basic implementation with Entry
- Adding hint labels
- Password visibility toggle basics
- First working example

### Container Types and Styling
📄 **Read:** [references/container-types.md](references/container-types.md)
- ContainerType property (Filled, Outlined, None)
- Filled container with background
- Outlined container with border
- ContainerBackground customization
- OutlineCornerRadius for rounded corners
- InputViewPadding for spacing
- Visual examples per type

### Hints and Assistive Labels
📄 **Read:** [references/hints-and-labels.md](references/hints-and-labels.md)
- Hint/floating label (Hint, ShowHint)
- Fixed hint position (IsHintAlwaysFloated)
- Helper text (HelperText, ShowHelperText)
- Error text (ErrorText, HasError)
- Character counter (CharMaxLength, ShowCharCount)
- Reserved space (ReserveSpaceForAssistiveLabels)
- Label styling (HintLabelStyle, HelperLabelStyle, ErrorLabelStyle)
- Font customization (FontFamily, FontSize, FontAttributes)

### Custom Icons and Views
📄 **Read:** [references/custom-icons.md](references/custom-icons.md)
- Leading view (LeadingView, LeadingViewPosition)
- Trailing view (TrailingView, TrailingViewPosition)
- Inside vs Outside positioning
- Font icons and unicode
- Custom view integration
- Event handling for icon actions
- Common icon patterns (calendar, search, clear)

### States, Colors, and Theming
📄 **Read:** [references/states-and-colors.md](references/states-and-colors.md)
- Visual states (Normal, Focused, Error, Disabled)
- Stroke property for borders
- CurrentActiveColor property
- Visual State Manager integration
- Stroke thickness (FocusedStrokeThickness, UnfocusedStrokeThickness)
- Container background colors
- Label text colors
- Theme integration

### Advanced Features and Events
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- Password visibility toggle (EnablePasswordVisibilityToggle)
- PasswordVisibilityToggled event
- Right-to-left support (FlowDirection)
- Supported input views (Entry, Editor, Autocomplete, ComboBox, etc.)
- Platform-specific limitations
- How-to scenarios
- Troubleshooting

