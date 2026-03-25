---
name: syncfusion-maui-checkbox
description: Implements Syncfusion .NET MAUI CheckBox (SfCheckBox) controls in MAUI applications. Use when implementing checkboxes, selection controls, or toggle buttons in .NET MAUI. Covers three-state checkboxes, indeterminate state, checkbox customization, checkbox events, visual states, and checkbox groups.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Checkboxes in .NET MAUI

The Syncfusion .NET MAUI CheckBox (SfCheckBox) is a selection control that allows users to choose one or more options from a set. It supports three states (checked, unchecked, indeterminate), extensive visual customization, and event-driven state management.

## When to Use This Skill

Use this skill when the user needs to:

- **Implement checkbox controls** in .NET MAUI applications
- **Add selection controls** for single or multiple choice scenarios
- **Create three-state checkboxes** with indeterminate state support
- **Customize checkbox appearance** (colors, shapes, sizes, text)
- **Handle checkbox events** (StateChanged, StateChanging)
- **Apply visual states** using VisualStateManager
- **Build checkbox groups** for multi-select scenarios (e.g., settings, forms, filters)
- **Implement parent-child checkbox hierarchies** with select-all functionality
- **Create terms and conditions** acceptance UI
- **Add accessible selection controls** with MAUI styling

## Component Overview

The SfCheckBox control provides:
- **Three-state support**: Checked, Unchecked, and Indeterminate states
- **Visual customization**: Colors, corner radius, stroke thickness, tick color
- **Text customization**: Font, color, alignment, line break modes
- **Event handling**: StateChanged and StateChanging events
- **Visual State Manager**: Custom appearance for each state
- **Animation control**: Enable/disable state change animations

## Documentation and Navigation Guide

### Getting Started and Installation

📄 **Read:** [references/getting-started.md](references/getting-started.md)

**When to read**: User is setting up SfCheckBox for the first time, needs installation steps, or wants basic implementation examples.

Topics covered:
- Installing Syncfusion.Maui.Buttons NuGet package
- Registering Syncfusion handler (ConfigureSyncfusionCore)
- Basic CheckBox implementation (XAML and C#)
- Setting checkbox caption text
- Understanding three states (Checked, Unchecked, Indeterminate)
- Single checkbox scenarios (terms of service, agreements)
- Multiple checkbox groups (multi-select options)
- Intermediate state with parent-child relationships
- Select-all checkbox patterns

### Visual Customization

📄 **Read:** [references/visual-customization.md](references/visual-customization.md)

**When to read**: User wants to customize the appearance of checkboxes, change colors, adjust sizes, modify text styling, or control animations.

Topics covered:
- Shape customization (CornerRadius for rounded checkboxes)
- State colors (CheckedColor, UncheckedColor)
- Stroke thickness customization
- Caption text appearance (TextColor, FontFamily, FontSize, FontAttributes, HorizontalTextAlignment)
- Tick color customization
- LineBreakMode options (wrap or truncate text)
- Size customization (ControlSize property)
- Font auto-scaling (FontAutoScalingEnabled)
- Animation control (EnabledAnimation)
- Content spacing between checkbox and text

### Events and State Management

📄 **Read:** [references/events.md](references/events.md)

**When to read**: User needs to respond to checkbox state changes, validate selections, or implement custom logic when checkboxes are toggled.

Topics covered:
- StateChanged event and StateChangedEventArgs
- StateChanging event (cancellable before state change)
- Event handling patterns in XAML and C#
- Programmatic state changes
- Event-driven UI updates
- Preventing state changes with Cancel property
- Coordinating multiple checkbox states

### Visual States with VisualStateManager

📄 **Read:** [references/visual-states.md](references/visual-states.md)

**When to read**: User wants to apply different visual appearances for checked, unchecked, or indeterminate states using VisualStateManager.

Topics covered:
- Visual State Manager overview for SfCheckBox
- Three visual states (Checked, Unchecked, Intermediate)
- XAML implementation with VisualStateGroups
- C# implementation with VisualState setters
- Customizing properties per state
- Complete examples for each state

