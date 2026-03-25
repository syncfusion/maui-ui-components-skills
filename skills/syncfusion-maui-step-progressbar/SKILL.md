---
name: syncfusion-maui-step-progressbar
description: Implements Syncfusion .NET MAUI StepProgressBar (SfStepProgressBar) control. Use when implementing step progress visualization, multi-step process tracking, or sequential progress indication. This skill covers step appearance customization, step states (completed/in-progress/not-started), orientation options, tooltips, and accessibility for order tracking, registration forms, or checkout processes.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion .NET MAUI StepProgressBar

A comprehensive guide for implementing and customizing the Syncfusion .NET MAUI StepProgressBar (SfStepProgressBar) control. This skill covers step progress visualization, multi-step process tracking, progress indication through sequential steps, customizing step appearance and content, handling step states (completed/in-progress/not-started), orientation options, tooltips, events, accessibility, RTL support, and the liquid glass effect.

## When to Use This Skill

Use this skill when the user needs to:

- Implement a step-by-step progress indicator in .NET MAUI applications
- Display multi-step process visualization (order tracking, user registration, checkout flows)
- Show progress through sequential stages with visual feedback
- Customize step appearance (shapes, colors, content types, animations)
- Handle step states: completed, in-progress, or not-started
- Configure horizontal or vertical orientation for step display
- Add interactive tooltips to steps for additional context
- Implement step tapped events and status change handling
- Support right-to-left (RTL) layouts for internationalization
- Apply modern liquid glass effects to step progress bars
- Create accessible step progress experiences

## Component Overview

The **Syncfusion .NET MAUI StepProgressBar (SfStepProgressBar)** is a visual control that displays progress through multiple steps in a sequential process. It provides:

- **Multi-step visualization**: Display 3-10+ steps with clear progression
- **Three progress states**: Completed, In Progress, and Not Started
- **Flexible orientation**: Horizontal or vertical layouts
- **Rich customization**: Shapes (Circle/Square), content types (Tick/Cross/Dot/Numbering/Images), colors, sizes
- **Text descriptions**: Primary and secondary text with formatted text support
- **Interactive features**: Tooltips, tapped events, status change events
- **Accessibility**: Screen reader support and inclusive design
- **Advanced effects**: Liquid glass effect for modern UI aesthetics
- **RTL support**: Right-to-left rendering for internationalization

Common use cases include order tracking systems, multi-page registration forms, checkout flows, onboarding tutorials, survey progress, and installation wizards.

## Documentation and Navigation Guide

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)

When to read: User is setting up StepProgressBar for the first time, needs installation steps, or wants a basic working example.

Topics covered:
- Installing Syncfusion.Maui.ProgressBar NuGet package
- Registering the handler with ConfigureSyncfusionCore()
- Adding SfStepProgressBar to XAML or C#
- Populating items with ItemsSource and ObservableCollection
- Setting ActiveStepIndex and ActiveStepProgressValue
- Creating a ViewModel with StepProgressBarItem collection
- Basic horizontal progress bar example

### Descriptions and Labels

📄 **Read:** [references/descriptions-and-labels.md](references/descriptions-and-labels.md)

When to read: User needs to add text labels, customize label positioning, add images to steps, or use formatted text.

Topics covered:
- PrimaryText and SecondaryText properties
- PrimaryFormattedText and SecondaryFormattedText for rich text
- LabelSpacing to control space between step and text
- LabelPosition options (Start, End, Top, Bottom)
- ImageSource for adding images to step content
- Font icons with FontImageSource
- Position-specific label behaviors in horizontal/vertical modes

### Orientation

📄 **Read:** [references/orientation.md](references/orientation.md)

When to read: User wants to change from horizontal to vertical layout or needs guidance on orientation options.

Topics covered:
- Horizontal orientation (default)
- Vertical orientation
- Setting the Orientation property
- Visual differences between orientations
- When to use each orientation

### Customization

📄 **Read:** [references/customization.md](references/customization.md)

When to read: User needs to customize step shapes, colors, content types, animations, templates, or per-step styling.

Topics covered:
- Step shape types (Circle, Square) with ShapeType property
- Content types (Numbering, Tick, Cross, Dot) with ContentType property
- Animation duration with ProgressAnimationDuration
- Progress bar background color customization
- Custom progress track size per step with ProgressTrackSize
- StepSettings for completed, in-progress, and not-started states
- Background, ContentFillColor, ProgressColor, Stroke properties
- TextStyle customization (color, font, attributes)
- StepTemplate with DataTemplate for custom step visuals
- DataTemplateSelector for conditional step templates
- PrimaryTextTemplate and SecondaryTextTemplate for custom labels
- Complete customization examples

### Events

📄 **Read:** [references/events.md](references/events.md)

When to read: User needs to handle step tapped interactions or respond to step status changes.

Topics covered:
- StepTapped event for handling step tap interactions
- StepTappedEventArgs properties
- StepStatusChanged event for tracking status changes
- StepStatusChangedEventArgs properties
- Event handler implementation examples

### Tooltips

📄 **Read:** [references/tooltips.md](references/tooltips.md)

When to read: User wants to add tooltips to steps, customize tooltip appearance, or use custom tooltip templates.

Topics covered:
- Enabling tooltips with ShowToolTip property
- Setting ToolTipText on StepProgressBarItem
- ToolTipSettings for appearance customization
- Background, Stroke, Duration, and TextStyle properties
- ToolTipTemplate with DataTemplate for custom tooltip designs
- Tooltip visibility duration control
- Custom tooltip layouts and content

### Accessibility

📄 **Read:** [references/accessibility.md](references/accessibility.md)

When to read: User needs to ensure accessibility compliance or understand screen reader support.

Topics covered:
- Accessibility features overview
- Screen reader announcement format
- Step number, text, and status announcements
- Inclusive design considerations

### Right-to-Left Support

📄 **Read:** [references/right-to-left.md](references/right-to-left.md)

When to read: User needs to support RTL languages (Arabic, Hebrew, etc.) or mirror the layout.

Topics covered:
- FlowDirection property set to RightToLeft
- RTL rendering behavior
- Layout mirroring for internationalization

### Liquid Glass Effect

📄 **Read:** [references/liquid-glass-effect.md](references/liquid-glass-effect.md)

When to read: User wants to apply modern liquid glass visual effects (.NET 10+, macOS 26+, iOS 26+).

Topics covered:
- SfGlassEffectView integration
- Wrapping StepProgressBar in glass effect view
- Setting transparent background for glass effect
- Platform and version requirements
- Complete implementation with gradient background

