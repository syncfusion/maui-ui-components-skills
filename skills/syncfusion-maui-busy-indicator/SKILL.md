---
name: syncfusion-maui-busy-indicator
description: Implements Syncfusion .NET MAUI Busy Indicator (SfBusyIndicator) for showing loading states, data processing, and app activity indicators. Use when displaying loading indicators, busy animations, activity spinners, or processing feedback in .NET MAUI apps. Covers busy indicator setup, animations, and visual feedback during long-running operations.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Busy Indicators

The Syncfusion .NET MAUI Busy Indicator (SfBusyIndicator) provides visual feedback during app loading, data processing, and other time-consuming operations. It offers 7 built-in animation types, extensive customization options, and a simple API for controlling loading states.

## When to Use This Skill

Use this skill when you need to:

- **Display loading states** during data fetching, API calls, or initialization
- **Show processing feedback** for long-running operations
- **Implement activity indicators** with custom animations and styling
- **Add busy overlays** to prevent user interaction during processing
- **Create branded loading experiences** with custom colors, sizes, and titles
- **Control animation states** dynamically based on application logic
- **Provide visual feedback** that improves perceived performance

**Common scenarios:** Login screens, data synchronization, file uploads, search operations, report generation, async operations, and any task requiring user patience.

## Component Overview

**Key Capabilities:**
- **7 Animation Types:** CircularMaterial, LinearMaterial, Cupertino, SingleCircle, DoubleCircle, Globe, HorizontalPulsingBox
- **Full Customization:** Colors, sizes, speeds, overlay backgrounds
- **Title Support:** Configurable text with font customization and placement options
- **State Control:** Start/stop animations programmatically with IsRunning property
- **Responsive Design:** Auto-scaling fonts and adaptive sizing
- **Overlay Support:** Optional background overlays with gradient support

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

**When to read:** First-time implementation, project setup, package installation

**What you'll learn:**
- Installing Syncfusion.Maui.Core NuGet package
- Registering handlers in MauiProgram.cs
- Creating your first busy indicator
- Basic XAML and C# implementation

### Animation Types
📄 **Read:** [references/animation-types.md](references/animation-types.md)

**When to read:** Choosing visual style, implementing specific animation types

**What you'll learn:**
- All 7 built-in animation types with examples
- CircularMaterial (Android Material Design style)
- LinearMaterial (horizontal progress bar style)
- Cupertino (iOS-style spinner)
- SingleCircle, DoubleCircle (concentric circle animations)
- Globe (3D globe rotation effect)
- HorizontalPulsingBox (pulsing box animation)
- Visual comparisons and use cases

### Customization
📄 **Read:** [references/customization.md](references/customization.md)

**When to read:** Styling indicators, adjusting colors/sizes, setting animation speed

**What you'll learn:**
- Indicator color customization (IndicatorColor)
- Overlay background styling (OverlayFill)
- Gradient backgrounds with Brush types
- Animation duration control (DurationFactor: 0-1)
- Indicator sizing (SizeFactor: 0-1)
- Complete styling examples

### Title Configuration
📄 **Read:** [references/title-configuration.md](references/title-configuration.md)

**When to read:** Adding text labels, customizing fonts, positioning titles

**What you'll learn:**
- Setting title text (Title property)
- Text color customization (TextColor)
- Title placement (Top, Bottom, None)
- Spacing control (TitleSpacing)
- Font customization (size, attributes, family)
- Auto-scaling support

### Controlling Animation State
📄 **Read:** [references/controlling-state.md](references/controlling-state.md)

**When to read:** Starting/stopping animations, managing visibility, dynamic control

**What you'll learn:**
- IsRunning property usage
- Starting and stopping animations
- Default behavior (false)
- Binding to ViewModel properties
- Event-driven state management

