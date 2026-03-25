---
name: syncfusion-maui-calendar
description: Implements and customize Syncfusion .NET MAUI Calendar (SfCalendar) control. Use when implementing calendar functionality, date selection (single, multiple, range), or calendar views (month, year, decade, century). Covers date restrictions (min/max dates, EnablePastDates, SelectableDayPredicate), calendar customization, events (ViewChanged, SelectionChanged, Tapped), and header/footer customization.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing .NET MAUI Calendar (SfCalendar)

A comprehensive skill for implementing and customizing the Syncfusion .NET MAUI Calendar control. The SfCalendar provides powerful date selection capabilities with multiple view modes, extensive customization options, and flexible date restrictions.

## When to Use This Skill

Use this skill when you need to:
- Implement a calendar control in .NET MAUI applications
- Enable date selection (single date, multiple dates, or date ranges)
- Display calendar views (Month, Year, Decade, Century)
- Restrict date selection with min/max dates or custom validation
- Customize calendar appearance (backgrounds, text styles)
- Handle calendar events (selection changes, view changes, taps)
- Configure week numbers, first day of week, or trailing/leading dates
- Implement localization or RTL support for calendars
- Add special date highlighting or weekend styling
- Navigate programmatically between dates or views

## Component Overview

**SfCalendar** is a feature-rich calendar control that allows users to:
- Select single dates, multiple dates, or date ranges
- Navigate through Month, Year, Decade, and Century views
- Restrict dates using MinimumDate, MaximumDate, EnablePastDates, or SelectableDayPredicate
- Customize every aspect of the calendar's appearance
- Respond to user interactions through comprehensive events

**Key Capabilities:**
- 4 calendar views for flexible navigation
- 3 selection modes (Single, Multiple, Range)
- Comprehensive date restriction options
- Full customization (dates, today, trailing/leading)
- Rich event system for user interactions
- Localization and globalization support
- RTL (Right-to-Left) language support

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and NuGet package setup (Syncfusion.Maui.Calendar)
- Handler registration in MauiProgram.cs
- Basic SfCalendar initialization in XAML and C#
- First day of week customization
- Corner radius configuration

### Calendar Views
📄 **Read:** [references/calendar-views.md](references/calendar-views.md)
- Month view (default, most common)
- Year view for month selection
- Decade view for year selection
- Century view for decade selection
- NumberOfVisibleWeeks property
- Week number display (ShowWeekNumber)
- View navigation and switching

### Date Selection
📄 **Read:** [references/date-selection.md](references/date-selection.md)
- Single selection mode (one date at a time)
- Multiple selection mode (select multiple dates)
- Range selection mode (select date ranges)
- EnableSwipeSelection for range selection
- RangeSelectionDirection (Forward, Backward, Both, None)
- Programmatic selection with SelectedDate/SelectedDates/SelectedRange
- Selection behavior in different views

### Date Restrictions
📄 **Read:** [references/date-restrictions.md](references/date-restrictions.md)
- MinimumDate property (restrict backward navigation)
- MaximumDate property (restrict forward navigation)
- EnablePastDates property (disable dates before today)
- SelectableDayPredicate (custom date validation logic)
- Disabling weekends or specific dates
- Date validation patterns

### Date Navigation
📄 **Read:** [references/date-navigation.md](references/date-navigation.md)
- DisplayDate property for programmatic navigation
- Programmatic view switching
- AllowViewNavigation property
- NavigateToAdjacentMonth (leading/trailing date navigation)
- Swipe gestures for month/year navigation
- Header tap navigation between views

### Calendar Modes and Display Options
📄 **Read:** [references/calendar-modes.md](references/calendar-modes.md)
- Default mode (inline display)
- Dialog mode (centered popup)
- RelativeDialog mode (positioned popup)
- CalendarRelativePosition enum (AlignTop, AlignBottom, AlignToLeftOf, AlignToRightOf, etc.)
- IsOpen property for programmatic control
- PopupWidth and PopupHeight customization
- Date picker patterns

### Customization and Styling
📄 **Read:** [references/customizations.md](references/customizations.md)
- Month cell customization (dates, today, trailing/leading)
- Text styles (TextStyle, TodayTextStyle, DisabledDatesTextStyle)
- Background colors for all cell states
- Weekend date styling
- Special date highlighting with icons (Dot, Heart, Diamond, Star, Bell)
- Year, Decade, and Century view customization
- CalendarTextStyle properties (TextColor, FontSize, FontFamily)

### Events and Interactions
📄 **Read:** [references/events.md](references/events.md)
- ViewChanged event (view/date range changes)
- SelectionChanged event (date selection changes)
- Tapped event (single tap on dates)
- DoubleTapped event (double tap on dates)
- LongPressed event (long press on dates)
- Event arguments and data
- Event handling patterns

### Headers and Footers
📄 **Read:** [references/headers-footers.md](references/headers-footers.md)
- HeaderView customization (height, background, text style)
- Header text format customization
- ShowNavigationArrows property
- HeaderTemplate for custom header UI
- FooterView customization
- Custom footer templates

### Localization and Advanced Features
📄 **Read:** [references/localization-advanced.md](references/localization-advanced.md)
- Globalization and localization setup
- CurrentUICulture configuration
- Resource file creation for multiple languages
- RTL (Right-to-Left) support
- CalendarIdentifier (Gregorian, Hijri, etc.)
- Calendar modes
- Liquid glass effect
- Advanced visual customization