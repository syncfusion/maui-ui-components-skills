---
name: syncfusion-maui-radio-button
description: Implements Syncfusion .NET MAUI Radio Button (SfRadioButton) for mutually exclusive selection controls. Use when implementing radio buttons, selection controls with mutually exclusive options, or grouped radio buttons in .NET MAUI applications. Covers radio button state management, grouping mechanisms (GroupKey or SfRadioGroup), events, and visual customization.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing .NET MAUI Radio Buttons

Guide for implementing Syncfusion .NET MAUI Radio Button (SfRadioButton) - a selection control that allows users to select one option from a set with checked and unchecked states.

## When to Use This Skill

Use this skill when you need to:
- Implement radio buttons for mutually exclusive selections in .NET MAUI
- Create grouped radio button sets where only one option can be selected
- Handle radio button state changes and user interactions
- Customize radio button appearance (colors, sizing, text styling)
- Implement GroupKey-based or SfRadioGroup-based grouping
- Work with radio button events (StateChanged, StateChanging)
- Apply VisualStateManager to radio buttons
- Create forms with single-selection options (payment methods, preferences, etc.)

## Component Overview

The Syncfusion .NET MAUI Radio Button (SfRadioButton) provides:
- **Two States:** Checked and unchecked with visual feedback
- **Grouping:** Multiple approaches (SfRadioGroup, GroupKey) for mutual exclusion
- **Events:** StateChanged and StateChanging for interaction handling
- **Customization:** Extensive styling options for colors, text, sizing
- **Visual States:** VisualStateManager integration for advanced styling

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installing Syncfusion.Maui.Buttons NuGet package
- Registering the Syncfusion handler in MauiProgram.cs
- Creating your first SfRadioButton (XAML and C#)
- Setting caption text with the Text property
- Managing radio button state with IsChecked
- Basic SfRadioGroup usage for mutual exclusion
- Minimal working examples to get started quickly

### Grouping and Organization
📄 **Read:** [references/grouping.md](references/grouping.md)
- GroupKey approach: Group radio buttons across different layouts
- CheckedItem property: Retrieve the currently selected radio button
- SfRadioGroup container: Automatic mutual exclusion management
- CheckedChanged event: Respond to selection changes in groups
- Orientation options: Vertical (default) or horizontal layouts
- SelectedValue property: Bind to selected radio button values
- Choosing between GroupKey and SfRadioGroup approaches
- Multiple independent groups in the same view
- Advanced grouping scenarios and patterns

### Events and Interaction
📄 **Read:** [references/events.md](references/events.md)
- StateChanged event: Detect when radio button state changes
- StateChangedEventArgs: Access new state information
- StateChanging event: Intercept and cancel state changes
- Event handler implementation patterns (XAML and C#)
- Preventing unwanted state changes with event cancellation
- Event timing and lifecycle considerations
- Practical scenarios: validation, confirmation dialogs
- Responding to user interactions programmatically

### Visual Customization
📄 **Read:** [references/visual-customization.md](references/visual-customization.md)
- CheckedColor and UncheckedColor: Customize state colors
- StrokeThickness: Adjust radio button border (platform considerations)
- Text appearance: TextColor, FontFamily, FontAttributes, FontSize
- HorizontalTextAlignment: Control text positioning
- LineBreakMode: Text wrapping and truncation options
- ControlSize: Adjust radio button dimensions
- ContentSpacing: Control spacing between button and text
- FontAutoScalingEnabled: Automatic font scaling for accessibility
- EnabledAnimation: Control state change animations
- Complete styling examples and best practices

### Visual States
📄 **Read:** [references/visual-states.md](references/visual-states.md)
- VisualStateManager integration with SfRadioButton
- Available visual states: Checked and Unchecked
- Defining visual states in XAML
- Creating visual states programmatically in C#
- Customizing appearance for each state
- Applying multiple property setters per state
- Complete working examples
- Best practices for visual state management

