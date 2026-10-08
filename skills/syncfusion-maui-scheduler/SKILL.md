---
name: syncfusion-maui-scheduler
description: Implements Syncfusion .NET MAUI Scheduler (SfScheduler). Use when creating scheduling applications, calendar views, appointment management, event planning, or resource scheduling in .NET MAUI. Covers scheduler views, appointment management, recurring events, timeline views, hierarchical resources, ICS import/export, quick info template, floating action button, adaptive UI, agenda layout mode, and month agenda view.
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
---

# Implementing Scheduler

The Syncfusion .NET MAUI Scheduler (SfScheduler) is a comprehensive scheduling component that provides nine different built-in view modes for displaying and managing appointments efficiently. It supports day, week, workweek, month, agenda, and timeline views with features like recurring appointments, drag-and-drop, resizing, hierarchical resource management, ICS import/export, adaptive UI, floating action button, quick info template, and time zone support.

## When to Use This Skill

Use this skill when you need to:
- Create scheduling or calendar applications in .NET MAUI
- Display appointments across different time periods (day, week, month)
- Implement recurring events with complex patterns
- Build resource-based scheduling systems (meeting rooms, employees, equipment)
- Organize resources in a hierarchical (tree) structure
- Restrict overlapping appointments within the same time range
- Render major and minor tick marks inside time-slot cells
- Export and import appointments using the iCalendar (ICS) standard
- Adjust the rendered height of appointments in Month view
- Open the appointment editor programmatically (Add, Edit, Delete, QuickInfo)
- Build adaptive resource rows that expand to fit appointments
- Show an inline agenda pane inside the Month view
- Enable an adaptive UI on Windows/macOS for mobile-style drawers and headers
- Display week numbers in Timeline Month and Agenda views
- Force the Agenda view into mobile or desktop layout
- Customize the day text format inside Month cells
- Add a floating action button to create a new appointment
- Replace the default quick info popup with a custom template
- Customize the empty-day (week header) template in Agenda view
- Customize the day header template in Agenda view
- Create timeline views for project planning or horizontal scheduling
- Add drag-and-drop or resizable appointments
- Implement multi-timezone appointment management
- Create custom appointment editors or tooltips
- Build agenda views or month calendars
- Handle appointment reminders and notifications
- Support multiple calendar types (Gregorian, Hijri, etc.)
- Implement load-on-demand for large appointment datasets

## Component Overview

The Scheduler control provides:
- **9 Built-in Views**: Day, Week, WorkWeek, Month, Agenda, TimelineDay, TimelineWeek, TimelineWorkWeek, TimelineMonth
- **Appointment Types**: Normal, All-Day, Spanned, Recurring appointments with exception handling
- **Interactive Features**: Drag-and-drop, resizing, custom editors, tooltips, cell selection
- **Resource Management**: Multiple resources with hierarchical grouping, adaptive row heights, and adaptive UI for desktop
- **Advanced Features**: Time zones, localization, load-on-demand, reminders, special time regions, ICS import/export, programmatic editor popups, floating action button
- **Customization**: Flexible styling, templates, working hours, date restrictions, month and minor tick time format, day string format, mobile/desktop agenda layout

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

When the user needs to:
- Install and set up the Syncfusion .NET MAUI Scheduler
- Register the scheduler handler in MauiProgram.cs
- Create their first scheduler with basic configuration
- Add simple appointments to the scheduler
- Understand view modes and basic properties
- Set up the initial project structure
- Import necessary NuGet packages

### Appointments Management
📄 **Read:** [references/appointments.md](references/appointments.md)

When the user needs to:
- Create and configure appointments
- Map custom business objects to appointments
- Implement normal, all-day, or spanned appointments
- Create recurring appointments with patterns (daily, weekly, monthly, yearly)
- Use recurrence rules (RRULE syntax)
- Handle recurrence exceptions (skip or modify specific occurrences)
- Customize appointment properties (subject, notes, location, colors)
- Bind appointment sources to custom data collections
- Understand appointment rendering order

### Day, Week, and WorkWeek Views
📄 **Read:** [references/day-week-views.md](references/day-week-views.md)

When the user needs to:
- Configure Day, Week, or WorkWeek views
- Set the number of visible days
- Customize time intervals between time slots
- Configure time rulers and time labels
- Configure major and minor ticks in the time ruler
- Set up working hours and non-working hours
- Create special time regions (blocking time intervals)
- Customize time slot appearance
- Customize the height of all-day appointments in the all-day panel of day views
- Render appointments spanning more than 24 hours in the all-day panel or within timeslot cells in day views
- Configure view headers
- Handle time slot sizing and customization

### Timeline Views
📄 **Read:** [references/timeline-views.md](references/timeline-views.md)

When the user needs to:
- Implement Timeline Day, Week, WorkWeek, or Month views
- Display appointments on a horizontal time axis
- Configure visible days in timeline views
- Set time intervals for timeline slots
- Customize viewport height
- Configure time rulers and major/minor ticks in timeline views
- Create horizontal scheduling interfaces
- Add special time regions in Timeline Month view
- Handle scrolling and navigation in timeline views

### Month and Agenda Views
📄 **Read:** [references/month-agenda-views.md](references/month-agenda-views.md)

When the user needs to:
- Configure Month view with appointments
- Customize month cells appearance
- Display appointments inline in Month view
- Adjust the rendered height of each appointment inside a Month cell
- Customize the day number text format inside a Month cell
- Show a month agenda panel beneath the month grid that lists the selected date's appointments
- Align date text in Month view
- Set up Agenda view for list-based appointment display
- Force the Agenda view to use a mobile or desktop layout
- Customize agenda view date and time formats
- Hide weeks that do not contain any appointments in Agenda view
- Provide a custom template for the empty-day placeholder or day header in Agenda view
- Handle appointment grouping by weeks
- Configure selected date display
- Customize month view indicators
- Combine different view modes

### Appointment Interactions
📄 **Read:** [references/appointment-interactions.md](references/appointment-interactions.md)

When the user needs to:
- Enable drag-and-drop for appointments
- Allow appointment resizing
- Create custom appointment editors
- Programmatically open the Add, Edit, Delete, or QuickInfo popup from code
- Configure appointment tooltips
- Customize the Quick Info popup with a custom `DataTemplate`
- Show a Floating Action Button that opens the Add appointment editor
- Restrict overlapping appointments in the same time range
- Implement cell selection
- Customize selection appearance
- Handle interaction events (tap, drag, resize)
- Disable specific interactions
- Create custom editor forms

### Resources and Calendar Types
📄 **Read:** [references/resources-calendars.md](references/resources-calendars.md)

When the user needs to:
- Implement resource-based scheduling (rooms, employees, equipment)
- Add and configure multiple resources
- Group appointments by resources
- Group resources in Month view on Windows and macOS
- Enable adaptive resource grouping in Month view on Android and iOS
- Build hierarchical (parent/child) resource trees for Timeline views
- Use the adaptive UI (mobile-style drawer) on desktop platforms (Windows, macOS)
- Use adaptive row heights so each resource row auto-sizes to the tallest appointment
- Customize resource headers and appearance
- Implement different calendar types (Gregorian, Hijri)
- Switch between calendar systems
- Handle resource-specific appointments

### Navigation and Date Restrictions
📄 **Read:** [references/navigation-restrictions.md](references/navigation-restrictions.md)

When the user needs to:
- Implement programmatic date navigation
- Set minimum and maximum date restrictions
- Prevent navigation beyond specific dates
- Customize header view and date formats
- Handle view switching (between Day, Week, Month, etc.)
- Configure navigation buttons
- Enable or disable touch and swipe-based navigation
- Implement custom navigation controls

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)

When the user needs to:
- Implement load-on-demand for large datasets
- Configure appointment reminders
- Handle multiple time zones
- Convert appointments between time zones
- Export the appointments collection to an iCalendar (`.ics`) file
- Import appointments from an iCalendar (`.ics`) file
- Display ISO week numbers in Timeline Month and Agenda views
- Use scheduler events (Tapped, SelectionChanged, ViewChanged)
- Implement liquid glass effect for visual enhancement
- Handle appointment loading efficiently
- Add context menu support for timeslot cells and appointments
- Enable right-click interaction support with events and commands
- Create event handlers for user interactions

### Localization
📄 **Read:** [references/localization.md](references/localization.md)

When the user needs to:
- Localize scheduler UI to different languages
- Customize date and time formats
- Configure culture-specific settings
- Implement RTL (right-to-left) support
- Customize day and month names
- Handle regional date formatting

