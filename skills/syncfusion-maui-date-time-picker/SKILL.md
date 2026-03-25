---
name: syncfusion-maui-date-time-picker
description: Implements Syncfusion .NET MAUI DateTimePicker (SfDateTimePicker) control. Use when implementing date and time selection, datetime pickers, or date-time input interfaces in .NET MAUI applications. Covers DateTimePicker setup, formatting dates/times, picker modes (dialog/relative), customization, date restrictions, intervals, events, localization, and accessibility.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Date Time Pickers

A comprehensive skill for implementing and customizing the Syncfusion .NET MAUI DateTimePicker (SfDateTimePicker) control. This component provides a flexible way to select dates, times, or both with extensive customization options.

## When to Use This Skill

Use this skill when you need to:
- Add date and time selection to .NET MAUI apps
- Implement appointment or booking systems
- Create scheduling interfaces with time slots
- Build forms requiring datetime input
- Add calendar pickers with time selection
- Implement date range restrictions
- Customize picker appearance (headers, footers, themes)
- Handle selection events and data binding
- Support multiple date/time formats
- Localize picker labels and buttons
- Implement accessible datetime selection

## Component Overview

The **SfDateTimePicker** is a versatile picker control that allows users to select:
- **Dates only** - Pick day, month, and year
- **Times only** - Pick hours, minutes, and seconds
- **Date and Time** - Combined selection
- **Formatted display** - 18 date formats and 9 time formats
- **Multiple display modes** - Inline, Dialog, or Relative Dialog
- **Date restrictions** - Min/max dates and blackout times
- **Custom intervals** - Skip days, months, hours, minutes
- **Full customization** - Headers, footers, columns, selection views
- **Localization** - Multi-language support

## Documentation and Navigation Guide

### Getting Started & Installation
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installing Syncfusion.Maui.Picker NuGet package
- Registering handler in MauiProgram.cs
- Basic DateTimePicker implementation (XAML & C#)
- Running your first application
- Minimal working examples

### Date and Time Formatting
📄 **Read:** [references/formatting.md](references/formatting.md)
- 18 predefined date formats (dd_MM_yyyy, yyyy_MM_dd, MMM_dd_yyyy, etc.)
- 9 predefined time formats (h_mm_tt, HH_mm_ss, hh_mm_ss_tt, etc.)
- Setting DateFormat and TimeFormat properties
- Custom format configurations
- Format selection best practices
- Display format examples

### Picker Modes and Display
📄 **Read:** [references/picker-modes.md](references/picker-modes.md)
- Default mode (inline display)
- Dialog mode (popup with overlay)
- RelativeDialog mode (positioned popup)
- IsOpen property for programmatic control
- 8 RelativePosition options (AlignTop, AlignBottom, AlignTopLeft, etc.)
- Mode selection guidance by use case
- Opening picker programmatically

### Customization
📄 **Read:** [references/customization.md](references/customization.md)
- Header customization (text, divider color, templates)
- HeaderTemplate and HeaderTemplateSelector
- Column header styling
- Footer customization (OK/Cancel buttons, text, visibility)
- Selection view appearance
- Background colors and text styles
- DataTemplate and DataTemplateSelector examples
- Advanced theming

### Date Restrictions
📄 **Read:** [references/date-restriction.md](references/date-restriction.md)
- MinimumDate property (lower bound restrictions)
- MaximumDate property (upper bound restrictions)
- BlackoutDateTimes (blocking specific dates and times)
- Validation rules
- Use cases (holidays, unavailable periods, business hours)
- Restriction patterns and best practices

### Intervals
📄 **Read:** [references/intervals.md](references/intervals.md)
- Date intervals (DayInterval, MonthInterval, YearInterval)
- Time intervals (HourInterval, MinuteInterval, SecondInterval, MilliSecondInterval)
- Combining multiple intervals
- Skip patterns for appointment booking
- Time slot configurations (15 min, 30 min intervals)
- Use cases and examples

### Events
📄 **Read:** [references/events.md](references/events.md)
- SelectionChanged event (NewValue, OldValue)
- Immediate vs confirmed selection (IsSelectionImmediate)
- Dialog mode events (Opened, Closing, Closed)
- Canceling selection in Closing event
- Event handling patterns
- Data binding with events
- MVVM integration

### Localization
📄 **Read:** [references/localization.md](references/localization.md)
- Localizable strings (Day, Month, Year, Hour, Minute, Second, OK, Cancel)
- Setting CurrentUICulture
- Creating resource files (.resx)
- Adding translations for multiple languages
- Resource file structure and naming
- French, Spanish, and other language examples
- Best practices for multi-language apps

### Accessibility
📄 **Read:** [references/accessibility.md](references/accessibility.md)
- WCAG compliance features
- Keyboard navigation support
- Screen reader compatibility
- Semantic properties and AutomationId
- Focus management
- Accessible date/time selection
- Testing accessibility

