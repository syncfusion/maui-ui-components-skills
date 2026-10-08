---
name: syncfusion-maui-ai-assistview
description: Implements Syncfusion® .NET MAUI AI AssistView (SfAIAssistView) for AI-powered interactive chat interfaces. Use when working with AI chat interfaces, AI AssistView, SfAIAssistView, conversational UI, or chat with AI responses. Covers integrating AI services, creating chat UIs, implementing conversation flows, managing AI requests/responses, and displaying AI suggestions.
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
---

# Implementing AI AssistView in .NET MAUI

The Syncfusion .NET MAUI AI AssistView (SfAIAssistView) is a comprehensive control for integrating AI services into .NET MAUI applications. It provides a user-friendly conversational interface with built-in support for requests, responses, suggestions, conversation history, multiple agents, toast notifications, and extensive customization options.

## When to Use This Skill

Use this skill when you need to:

- **Integrate AI chat interfaces** in .NET MAUI applications
- **Display conversational UI** with user requests and AI responses
- **Implement AI-powered assistants** with suggestion prompts
- **Create chat-based interactions** with conversation history
- **Build intelligent applications** that interact with AI services
- **Customize chat appearance** with templates, themes, and styling
- **Handle user interactions** through events, commands, and data binding
- **Manage conversation flows** with headers, toolbars, and empty states
- **Implement localization** for multi-language AI chat applications
- **Work with multiple AI agents** for specialized assistant experiences
- **Show contextual toast notifications** for non-blocking feedback
- **Design responsive AI interfaces** with .NET MAUI cross-platform support

**Common Scenarios:**
- Customer support chatbots, AI assistants, help desk interfaces
- Conversational AI applications, virtual agents, interactive assistants
- Chat-based content generation, Q&A applications, knowledge bases
- Personal productivity assistants, note-taking with AI, writing helpers
- Educational platforms with AI tutors, interactive learning interfaces
- Any real-time conversational UI with AI-powered responses

## Component Overview

**SfAIAssistView** is a comprehensive, feature-rich conversational control that:
- Displays AI-powered chat interfaces with user requests and AI responses
- Supports multiple content types (text, images, hyperlinks, cards, attachments)
- Provides flexible data binding with observable collections and MVVM patterns
- Enables customizable request/response display with templates and selectors
- Supports multiple AI agents with selected-agent customization
- Supports toast notifications for success, warning, and error feedback
- Includes smart suggestion system (header, response-specific, footer prompts)
- Offers toolbar and chat mode management with temporary sessions
- Maintains conversation history with timestamps and interaction tracking
- Handles rich user interactions (tapping, long-press, context menus)
- Provides customizable message input editor with file attachments and actions
- Supports structured prompt composition, disclaimer messages, and time break separators
- Features auto-scrolling, scroll-to-bottom button, voice input, and audio playback
- Supports comprehensive styling, theming, and platform-specific effects

---

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- NuGet package installation and setup
- Handler registration in MauiProgram.cs
- XAML namespace import and control initialization
- ViewModel setup with observable collections
- First request/response cycle implementation
- Common setup issues and troubleshooting

### Agents
📄 **Read:** [references/agents.md](references/agents.md)
- Multiple AI agents within a single chat experience
- Agent collection, selection, and editor `@` lookup
- Selected agent visibility and template customization

### Items and Data Binding
📄 **Read:** [references/items-and-data-binding.md](references/items-and-data-binding.md)
- Item properties (text, profile, timestamp, error messages)
- Item types (text, hyperlinks, images, cards, attachments)
- Item interaction events and commands
- Error response display and handling
- Custom item view templates
- Custom model binding and converters
- Context menus for requests and responses

### Suggestions
📄 **Read:** [references/suggestions.md](references/suggestions.md)
- Header-level suggestion prompts
- Response-specific suggestions
- Footer suggestions above the editor
- Suggestion selection events and commands
- Cancellation and submission behaviors

### Header
📄 **Read:** [references/header.md](references/header.md)
- Header visibility and customization
- Header text and template configuration
- Displaying suggestions in headers

### Toolbar
📄 **Read:** [references/toolbar.md](references/toolbar.md)
- Toolbar visibility and title customization
- New Chat button implementation
- Temporary chat mode support
- Chat mode switching events

### History
📄 **Read:** [references/history.md](references/history.md)
- Enabling conversation history
- Conversation item structure (title, timestamp, messages, pin state)
- Filtering, pinning, renaming, and deleting conversations
- History interaction handling and restore behavior

### Events and Commands
📄 **Read:** [references/events.md](references/events.md)
- Item tapped and long-pressed events
- Request events and command handling
- Footer action commands (copy, retry, rating)

### Editor and Attachments
📄 **Read:** [references/editor-and-attachments.md](references/editor-and-attachments.md)
- Custom editor layouts and templates
- Expandable request editor for long prompts
- Editing previous requests
- File attachment support, limits, and image preview
- Attachment button modes and action buttons configuration
- Send button customization

### Templates and Content Types
📄 **Read:** [references/templates-and-content.md](references/templates-and-content.md)
- Request and response item templates
- Template selectors for dynamic templates
- Full control template customization
- Custom chat rendering implementation

### Scrolling
📄 **Read:** [references/scrolling.md](references/scrolling.md)
- Auto-scroll to latest messages
- Auto-scroll behavior for latest request or response
- Scroll-to-bottom button functionality
- Button appearance customization

### Prompt Composition
📄 **Read:** [references/prompt-composition.md](references/prompt-composition.md)
- System, context, and part-based prompt composition
- Deterministic composition order and filtering
- Prompt composing event inspection

### Prompt Library
📄 **Read:** [references/prompt-library.md](references/prompt-library.md)
- Browse, search, and pick from a catalog of predefined prompts
- `SfPromptLibrary.ItemsSource` and the `PromptItem` model
- Handle selection via the `PromptSelected` event or `PromptSelectedCommand`

### Disclaimer Message
📄 **Read:** [references/disclaimer-message.md](references/disclaimer-message.md)
- Disclaimer text below the editor
- Visibility and footer spacing behavior

### Time Break Separators
📄 **Read:** [references/time-break-separators.md](references/time-break-separators.md)
- Date-based message grouping
- Smart Today / Yesterday / weekday labels
- Custom time break template and styling

### Voice Support
📄 **Read:** [references/voice-support.md](references/voice-support.md)
- Microphone button voice input
- Speech recognition and editor text injection
- Response audio playback controls
- Platform permissions and fallback behavior

### Toast Notifications
📄 **Read:** [references/toast-notifications.md](references/toast-notifications.md)
- Non-blocking success, warning, and error feedback

### Advanced Topics
📄 **Read:** [references/advanced-topics.md](references/advanced-topics.md)
- Text selection support
- Stop responding functionality
- Performance optimization

### AutoComplete Suggestions
📄 **Read:** [references/autocomplete-suggestions.md](references/autocomplete-suggestions.md)
- AutoComplete suggestions while typing
- Prefix and timing configuration
- Suggestion data binding
- Selection handling and behaviors
- Custom suggestion templates
- State monitoring and server integration

### Styling
📄 **Read:** [references/styling.md](references/styling.md)
- Theme key application
- Style keys for all UI elements
- Customizing request/response appearance
- Platform-specific styling effects

### Customization
📄 **Read:** [references/customization.md](references/customization.md)
- Empty state views
- Localization (RESX)
- Right-to-left (RTL) support
- Disabling image previews via `AllowImagePreview` (default `true`)
- Platform-specific effects (iOS liquid glass)

---

