---
name: syncfusion-maui-switch
description: Implements and customize Syncfusion .NET MAUI Switch (SfSwitch) controls. Use when working with switches, toggle switches, on/off controls, or binary state controls in .NET MAUI applications. Covers switch customization (colors, sizes, icons), state changes, indeterminate states, and visual state managers.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion .NET MAUI Switch (SfSwitch)

The Syncfusion .NET MAUI Switch (SfSwitch) control provides an efficient way to toggle between states with rich customization options, multiple state support, and advanced visual styling capabilities.

## When to Use This Skill

Use this skill when you need to:
- Implement toggle switches or on/off controls in .NET MAUI applications
- Create binary state controls with visual feedback
- Implement indeterminate states for work-in-progress scenarios
- Customize switch appearance (colors, sizes, corner radius, borders)
- Add custom icons or paths inside the switch thumb
- Handle state change events with validation or cancellation logic
- Apply advanced styling using Visual State Manager
- Support right-to-left (RTL) layouts for internationalization
- Implement modern UI effects like liquid glass morphism
- Provide accessible toggle controls with visual state feedback

## Component Overview

The SfSwitch control offers:
- **Multiple States:** On, Off, Indeterminate, and their disabled variants
- **Rich Customization:** Colors, sizes, borders, corner radius, and custom icons
- **Event Handling:** StateChanged and StateChanging events with cancellation support
- **Visual State Manager:** Advanced state-based styling for hovered, pressed, and disabled states
- **Modern Features:** Liquid glass effect for premium UI experiences
- **RTL Support:** Bidirectional layout support for global applications
- **Theming:** Material, Cupertino, and Fluent design themes

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

Use this when you need to:
- Install the Syncfusion.Maui.Buttons NuGet package
- Register the handler with ConfigureSyncfusionCore
- Create your first basic switch implementation
- Set up switch in both XAML and C#
- Perform actions based on state changes

### Switch States
📄 **Read:** [references/switch-states.md](references/switch-states.md)

Use this when you need to:
- Understand all available switch states
- Implement On, Off, and default states
- Enable and use indeterminate state (IsOn = null)
- Create disabled state variants (disabled On/Off/Indeterminate)
- Control state transitions programmatically
- Set initial state values

### Events
📄 **Read:** [references/events.md](references/events.md)

Use this when you need to:
- Handle state change notifications (StateChanged event)
- Validate or cancel state transitions (StateChanging event)
- Access old and new state values in event handlers
- Implement conditional state changes
- Trigger actions when switch state changes
- Prevent unwanted state transitions

### Customization
📄 **Read:** [references/customization.md](references/customization.md)

Use this when you need to:
- Customize track and thumb colors
- Adjust switch dimensions (width, height)
- Configure border colors and thickness
- Set corner radius for rounded appearance
- Add custom icons or paths to the thumb
- Apply different colors to different states
- Create branded or themed switch designs

### Visual State Manager
📄 **Read:** [references/visual-state-manager.md](references/visual-state-manager.md)

Use this when you need to:
- Apply advanced state-based styling
- Customize hovered state appearance (OnHovered, OffHovered, IndeterminateHovered)
- Style pressed states (OnPressed, OffPressed, IndeterminatePressed)
- Configure disabled state visuals (OnDisabled, OffDisabled, IndeterminateDisabled)
- Implement complex visual transitions
- Create interactive hover and press effects

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)

Use this when you need to:
- Enable liquid glass effect for modern UI (EnableLiquidGlassEffect)
- Implement right-to-left (RTL) layout support
- Support bidirectional layouts for internationalization
- Apply glass morphism effects over vibrant backgrounds
- Handle platform-specific features (.NET 10+, iOS 26, macOS 26)

