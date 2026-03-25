---
name: syncfusion-maui-expander
description: Implements the Syncfusion .NET MAUI Expander (SfExpander) control for collapsible and expandable content sections. Use when working with expanders, collapsible sections, accordions, expandable panels, or expand/collapse functionality in .NET MAUI applications. Covers space-efficient layouts, header customization, expand/collapse animations, and liquid glass effects.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing .NET MAUI Expander (SfExpander)

The Syncfusion .NET MAUI Expander control provides collapsible and expandable content sections for creating space-efficient layouts. Users can tap headers to reveal or hide content with smooth animations, making it ideal for accordion layouts, FAQs, and organized data displays.

## When to Use This Skill

Use this skill when you need to:

- **Implement collapsible sections** for space-efficient layouts and organized content display
- **Create accordion-style interfaces** for FAQs, settings panels, or grouped data
- **Build invoice or receipt views** with expandable sections (date, items, payment, address)
- **Design dynamic header/content layouts** where users tap to reveal more information
- **Add animated expand/collapse functionality** with customizable duration and easing
- **Handle expand/collapse events** for controlling user interactions and state management
- **Customize header appearance** with backgrounds, icon positions, and Visual State Manager styling
- **Apply modern liquid glass effects** for premium translucent designs in high-end applications

## Component Overview

- Interactive expand/collapse with single tap on header
- Fully customizable header and content view layouts
- Smooth animations with configurable duration and easing
- Event system for Expanding, Expanded, Collapsing, and Collapsed states
- Programmatic control via `IsExpanded` property
- Visual State Manager support for dynamic styling
- Header appearance customization (background, icon position, colors)
- Modern liquid glass effects (.NET 10+, macOS 26+, iOS 26+)
- Event cancellation for Expanding and Collapsing actions
- Platform-optimized rendering and animations

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

When to read: First-time setup, installation, basic implementation
- Installing Syncfusion.Maui.Expander NuGet package
- Registering Syncfusion handler (ConfigureSyncfusionCore)
- Creating basic SfExpander instance
- Defining Header and Content views
- Using IsExpanded property to control initial state
- Complete invoice example with multiple expanders
- Running your first expander application

### Header and Content Customization
📄 **Read:** [references/header-content-customization.md](references/header-content-customization.md)

When to read: Customizing header and content sections with layouts and views
- Loading any UI view in Header and Content properties
- Using Grid layouts with icons and labels
- Icon integration with font families
- Multi-expander layouts (invoice with multiple sections)
- Best practices for content structure
- Avoiding common pitfalls (Label wrapping in containers)

### Animation and Events
📄 **Read:** [references/animation-events.md](references/animation-events.md)

When to read: Controlling animation behavior and handling expand/collapse events
- AnimationDuration property (default: 300ms)
- AnimationEasing property (Linear, SinOut, etc.)
- Programmatic control with IsExpanded property
- Expanding event: Triggered before expansion (cancellable)
- Expanded event: Triggered after expansion completes
- Collapsing event: Triggered before collapse (cancellable)
- Collapsed event: Triggered after collapse completes
- Event handler examples with cancellation logic

### Appearance and Styling
📄 **Read:** [references/appearance-styling.md](references/appearance-styling.md)

When to read: Customizing header appearance, colors, icons, and Visual State Manager styling
- HeaderIconPosition property (Start, End)
- HeaderBackground color customization
- HeaderIconColor customization
- Visual State Manager (VSM) states: Expanded, Collapsed, PointerOver, Normal
- Dynamic appearance changes based on expander state
- Complete VSM examples with color transitions

### Liquid Glass Effect
📄 **Read:** [references/liquid-glass-effect.md](references/liquid-glass-effect.md)

When to read: Applying modern translucent designs with adaptive color tinting
- When to use liquid glass effect (premium UX applications)
- Wrapping SfExpander inside SfGlassEffectView
- Setting EnableLiquidGlassEffect="True" on expander
- Using Background="Transparent" for glass effect
- Complete invoice example with liquid glass
- Platform requirements: .NET 10+, macOS 26+, iOS 26+
- Multiple expanders with consistent glass effect

