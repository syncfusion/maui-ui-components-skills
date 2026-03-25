---
name: syncfusion-maui-scheduler
description: Implements Syncfusion .NET MAUI Scheduler (SfScheduler). Use when creating scheduling applications, calendar views, appointment management, event planning, or resource scheduling in .NET MAUI. Covers scheduler views, appointment management, recurring events, and timeline views.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Scheduler

The Syncfusion .NET MAUI Scheduler (SfScheduler) is a comprehensive scheduling component that provides nine different built-in view modes for displaying and managing appointments efficiently. It supports day, week, month, agenda, and timeline views with features like recurring appointments, drag-and-drop, resizing, resource management, and time zone support.

## When to Use This Skill

Use this skill when you need to:
- Create scheduling or calendar applications in .NET MAUI
- Display appointments across different time periods (day, week, month)
- Implement recurring events with complex patterns
- Build resource-based scheduling systems (meeting rooms, employees, equipment)
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
- **Resource Management**: Multiple resources with grouping and customization
- **Advanced Features**: Time zones, localization, load-on-demand, reminders, special time regions
- **Customization**: Flexible styling, templates, working hours, date restrictions

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
- Set up working hours and non-working hours
- Create special time regions (blocking time intervals)
- Customize time slot appearance
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
- Configure time rulers in timeline views
- Create horizontal scheduling interfaces
- Handle scrolling and navigation in timeline views

### Month and Agenda Views
📄 **Read:** [references/month-agenda-views.md](references/month-agenda-views.md)

When the user needs to:
- Configure Month view with appointments
- Customize month cells appearance
- Set up Agenda view for list-based appointment display
- Customize agenda view date and time formats
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
- Configure appointment tooltips
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
- Implement custom navigation controls

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)

When the user needs to:
- Implement load-on-demand for large datasets
- Configure appointment reminders
- Handle multiple time zones
- Convert appointments between time zones
- Use scheduler events (Tapped, SelectionChanged, ViewChanged)
- Implement liquid glass effect for visual enhancement
- Handle appointment loading efficiently
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

