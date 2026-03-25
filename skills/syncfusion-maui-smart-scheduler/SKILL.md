---
name: syncfusion-maui-smart-scheduler
description: Implements Syncfusion .NET MAUI AI-Powered Scheduler (SfSmartScheduler). Use when implementing natural language appointment scheduling, AI-powered scheduling, resource-aware booking, or conflict detection in MAUI apps. Covers AI scheduling, natural language CRUD operations, resource booking, and appointment summarization.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# .NET MAUI AI-Powered Scheduler (SfSmartScheduler)

A comprehensive skill for implementing Syncfusion's AI-powered scheduler that combines traditional scheduling with AI-driven intent understanding, enabling users to create, update, delete, and explore appointments using natural language.

## When to Use This Skill

Use this skill when you need to:
- Implement natural language appointment scheduling in MAUI apps
- Enable users to create/update/delete appointments with plain language
- Build resource-aware booking systems (rooms, equipment, people)
- Add AI-powered conflict detection to scheduling applications
- Implement smart appointment summarization features
- Create conversational scheduling interfaces
- Customize assist view appearance and behavior
- Handle AI-powered scheduling events and responses
- Style AI assistant components

**Choose SfSmartScheduler over traditional Scheduler when:**
- Users prefer conversational interfaces over forms
- The app needs to understand context (current view, resources, availability)
- Conflict detection and resolution is critical
- Resource management and availability checking is required
- Quick appointment summarization is valuable

## Component Overview

The **SfSmartScheduler** combines the Syncfusion Scheduler with AI-driven natural language processing. Users can interact with their calendar through conversational prompts like:
- "Schedule team meeting tomorrow at 2pm"
- "Book conference room A for Friday afternoon"
- "Find free time slots for project review this week"
- "Summarize my meetings for tomorrow"

The component respects current view context, resource availability, detects conflicts, and provides intelligent suggestions—turning scheduling into a conversation rather than form-filling.

### Key Features

- **Natural-language CRUD:** Create, update, delete appointments using plain language—no structured forms required
- **Resource-aware booking:** Book resources while respecting availability and current scheduler filters
- **Conflict detection:** Identify overlapping appointments and propose resolutions
- **Smart summarization:** Generate concise summaries of upcoming or selected appointments
- **Adaptive assist panel:** Configurable height, width, and layout for phone, tablet, desktop
- **Customizable assist button:** Enable/disable or replace with custom templates
- **Event support:** Choose between automatic AI-driven changes or manual handling via events

## Documentation and Navigation Guide

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)

**When to read:** First-time setup, project initialization, AI service configuration

**Covers:**
- Creating new MAUI projects (Visual Studio, VS Code, Rider)
- Installing Syncfusion.Maui.SmartComponents NuGet package
- Registering handlers (`ConfigureSyncfusionCore()`)
- Configuring AI services (Azure OpenAI, OpenAI, Ollama)
- Basic SfSmartScheduler initialization (XAML and C#)
- First appointment creation with natural language

### Natural Language Operations

📄 **Read:** [references/natural-language-operations.md](references/natural-language-operations.md)

**When to read:** Implementing conversational appointment management, understanding supported language patterns

**Covers:**
- Natural-language CRUD operations (Create, Update, Delete)
- Plain language appointment creation
- Time, date, subject, and recurrence understanding
- Resource references in natural language
- Multi-operation commands
- Supported phrases and patterns
- Context-aware scheduling
- Examples: "Schedule team meeting tomorrow 2pm", "Move marketing review to Friday"

### Resource-Aware Features

📄 **Read:** [references/resource-aware-features.md](references/resource-aware-features.md)

**When to read:** Implementing resource booking, conflict detection, availability checking, appointment summarization

**Covers:**
- Resource-aware booking (rooms, equipment, people)
- Availability checking and validation
- Conflict detection for overlapping appointments
- Alternative resource suggestions when unavailable
- Adjacent time slot recommendations
- Resource filtering and current view context
- Conflict resolution proposals (reschedule, reassign, extend buffer times)
- Smart summarization of appointments
- Free time finder functionality

### Assist View Customization

📄 **Read:** [references/assist-view-customization.md](references/assist-view-customization.md)

**When to read:** Customizing assist panel appearance, layout, button templates, prompts

**Covers:**
- Enable/disable assist button with `EnableAssistButton`
- Custom assist button templates with `AssistButtonTemplate`
- Assist view height customization (`AssistViewHeight`)
- Assist view width customization (`AssistViewWidth`)
- Header text customization (`AssistViewHeaderText`)
- Placeholder text configuration (`Placeholder`)
- Custom AI prompts (`Prompt`)
- Suggested prompts configuration (`SuggestedPrompts`)
- Banner visibility (`ShowAssistViewBanner`)
- Adaptive layouts for phone, tablet, and desktop

### Events and Methods

📄 **Read:** [references/events-and-methods.md](references/events-and-methods.md)

**When to read:** Implementing custom event handling, programmatic assist view control

**Covers:**
- `AssistAppointmentResponseCompleted` event
- Event arguments: Appointment, Handled, AssistantResponse, Action
- Manual vs automatic appointment handling
- `ResetAssistView()` method
- `CloseAssistView()` method
- `OpenAssistView()` method
- Programmatic control examples
- Custom validation logic
- Intercepting AI-generated appointments

### Styling

📄 **Read:** [references/styling.md](references/styling.md)

**When to read:** Customizing assist view appearance, colors, fonts, branding

**Covers:**
- `AssistStyle` properties overview
- Placeholder color customization
- Assist view header styling (text color, background, font)
- Font customization (size, family, attributes)
- Auto-scaling font configuration
- Complete styling examples (XAML and C#)
- Theme integration
- Corporate branding considerations

