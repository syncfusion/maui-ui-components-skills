---
name: syncfusion-maui-date-picker
description: Implements Syncfusion .NET MAUI DatePicker (SfDatePicker) control for date selection. Use when implementing date pickers, adding date selection UI, or working with SfDatePicker control. Covers date picker appearance customization (headers, footers, columns), date formatting (20+ predefined formats), date restrictions (min/max dates, blackout dates), and picker modes (Dialog, RelativeDialog).
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion .NET MAUI DatePicker (SfDatePicker)

Syncfusion .NET MAUI DatePicker (SfDatePicker) control allows you to select dates with a visually rich, customizable picker interface. The control supports dialog and drop-down modes, extensive customization options, date restrictions, formatting, localization, and accessibility features.

## When to Use This Skill

Use this skill when you need to:

- **Implement date selection** - Add date picker controls to .NET MAUI applications
- **Format dates** - Display dates in various formats (20+ predefined options)
- **Restrict date selection** - Set minimum/maximum dates, blackout specific dates
- **Customize appearance** - Style headers, footers, column headers, selection views
- **Handle date events** - Respond to date selection, dialog open/close, OK/Cancel actions
- **Configure picker modes** - Use Dialog, RelativeDialog, or Default display modes
- **Add intervals** - Set day/month/year intervals for selection
- **Enable accessibility** - Support keyboard navigation and localization
- **Apply modern effects** - Implement liquid glass effect (.NET 10+)

## Component Overview

**Key Capabilities:**
- **Date Selection** - Interactive date picker with day, month, year columns
- **Multiple Formats** - 20+ predefined date formats (dd/MM/yyyy, MM/dd/yyyy, etc.)
- **Picker Modes** - Dialog, RelativeDialog with customizable positions
- **Customization** - Headers, footers, column headers, selection views, colors, fonts
- **Date Restrictions** - Min/max dates, blackout dates collection
- **Intervals** - Day, month, year intervals for custom date ranges
- **Events & Commands** - SelectionChanged, dialog events, footer button events
- **Accessibility** - Keyboard navigation, screen reader support, localization
- **Advanced** - Text display modes (Fade, Shrink), liquid glass effect

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and NuGet package setup
- Handler registration in MauiProgram.cs
- Basic DatePicker implementation (XAML & C#)
- Setting header, footer, height, width
- SelectedDate property and clear selection

### Date Formatting
📄 **Read:** [references/formatting.md](references/formatting.md)
- 20+ predefined date formats
- Format property usage
- Common format patterns (dd_MM_yyyy, MM_dd_yyyy, etc.)
- Custom format examples

### Picker Modes and Display
📄 **Read:** [references/picker-modes.md](references/picker-modes.md)
- Dialog mode (popup display)
- RelativeDialog mode (positioned relative to view)
- 8 relative positions (AlignTop, AlignBottom, etc.)
- IsOpen property for programmatic control
- Custom popup size (PopupWidth, PopupHeight)

### Customization and Styling
📄 **Read:** [references/customizations.md](references/customizations.md)
- Header customization (text, divider, background, template)
- Column header customization (day/month/year headers)
- Footer customization (OK/Cancel buttons)
- Selection view customization (corner radius, stroke, padding)
- Column divider colors
- Close button and icon
- Column-specific text styles and widths
- DataTemplateSelector support

### Date Restrictions
📄 **Read:** [references/date-restrictions.md](references/date-restrictions.md)
- MinimumDate property
- MaximumDate property
- BlackoutDates collection
- Date range validation
- Preventing selection beyond ranges

### Intervals and Looping
📄 **Read:** [references/intervals-and-looping.md](references/intervals-and-looping.md)
- DayInterval property
- MonthInterval property
- YearInterval property
- EnableLooping for seamless navigation

### Events and Commands
📄 **Read:** [references/events-and-commands.md](references/events-and-commands.md)
- SelectionChanged event (NewValue, OldValue)
- Dialog mode events (Opened, Closing, Closed)
- Footer events (OkButtonClicked, CancelButtonClicked)
- SelectionChangedCommand, AcceptCommand, DeclineCommand
- Event handling patterns

### Accessibility and Localization
📄 **Read:** [references/accessibility-localization.md](references/accessibility-localization.md)
- Accessibility features for headers, footers, picker items
- Keyboard navigation (Tab, Enter, Arrow keys)
- Localization setup and resource files
- CurrentUICulture configuration
- Multilingual support

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- Text display modes (Default, Fade, Shrink, FadeAndShrink)
- Liquid glass effect integration
- SfGlassEffectView setup
- EnableLiquidGlassEffect property
- Platform requirements (.NET 10, macOS 26+, iOS 26+)

