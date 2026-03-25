---
name: syncfusion-maui-segmented-control
description: Implements Syncfusion .NET MAUI Segmented Control (SfSegmentedControl). Use when working with segmented controls, segment buttons, tab-like selection, button groups, or filter buttons in MAUI applications. This skill covers installation, item population, selection indicators, appearance customization, layout configuration, disabled segments, and events.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Segmented Controls in .NET MAUI

The Syncfusion .NET MAUI Segmented Control (SfSegmentedControl) provides a linear set of segments containing text, icons, or both, allowing users to select from mutually exclusive options. Each segment functions as a discrete button, creating an intuitive way to switch between views, filter content, or select time periods.

## When to Use This Skill

Use this skill when you need to:

- **Implement view switchers:** Toggle between Day/Week/Month/Year views, list/grid layouts, or different data visualizations
- **Create filter buttons:** Allow users to filter content by category, type, or time period
- **Build tab-like navigation:** Provide segment-based navigation without traditional tabs
- **Design selection interfaces:** Enable users to choose from 2-7 mutually exclusive options
- **Customize segment appearance:** Style borders, backgrounds, colors, corner radius, or selection indicators
- **Configure selection behavior:** Set up programmatic selection, selection modes, or custom indicators
- **Handle segment events:** Respond to selection changes or segment taps
- **Support RTL layouts:** Implement right-to-left language support
- **Apply special effects:** Add liquid glass effects or custom animations

Common scenarios: Calendar view toggles, chart period selectors, sorting options, display mode switches, category filters, and settings toggles.

## Component Overview

**Key Features:**
- Multiple display formats (text, icon, or combination)
- Four selection indicator styles (Fill, Border, TopBorder, BottomBorder)
- Scrollable segments for large item counts
- Two selection modes (Single, SingleDeselect)
- Comprehensive appearance customization
- Disabled segment support
- RTL support
- Ripple effect animations
- DataTemplate support for custom layouts

**Package:** `Syncfusion.Maui.Buttons`  
**Namespace:** `Syncfusion.Maui.Buttons`  
**Control Class:** `SfSegmentedControl`

## Documentation and Navigation Guide

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)

Start here for initial setup:
- Installing Syncfusion.Maui.Buttons NuGet package
- Registering Syncfusion handlers (ConfigureSyncfusionCore)
- Basic control initialization in XAML and C#
- Multi-IDE setup (Visual Studio, VS Code, Rider)
- Namespace imports and minimal examples

### Populating Segment Items

📄 **Read:** [references/populating-items.md](references/populating-items.md)

For adding segments to the control:
- Using ItemsSource with string arrays
- Creating SfSegmentItem objects
- Adding text, icons, or text+icon combinations
- Image sources and font icon configuration
- Dynamic item population
- Data binding approaches

### Selection Features

📄 **Read:** [references/selection.md](references/selection.md)

For controlling selection behavior and appearance:
- Programmatic selection with SelectedIndex
- Selection indicator placements (Fill, Border, TopBorder, BottomBorder)
- Selection modes (Single, SingleDeselect)
- Customizing selected segment colors and backgrounds
- Border customization for selected segments
- Ripple effect animation (EnableRippleEffect)
- Per-item selection styling

### Appearance Customization

📄 **Read:** [references/customization.md](references/customization.md)

For styling the control:
- Border color and thickness (Stroke, StrokeThickness)
- Corner radius (CornerRadius, SegmentCornerRadius)
- Text styling (TextStyle, FontAttributes, FontSize)
- Segment background colors
- Per-item customization
- Separator visibility (ShowSeparator)
- DataTemplate customization (SegmentTemplate)
- Selected item templates with IsSelected binding

### Layout Configuration

📄 **Read:** [references/layout.md](references/layout.md)

For sizing and layout control:
- Segment width (SegmentWidth, per-item Width)
- Segment height (SegmentHeight)
- Visible segment count (VisibleSegmentsCount)
- Scrolling behavior
- Auto-sizing vs fixed dimensions

### Disabled Segments

📄 **Read:** [references/disabled-segments.md](references/disabled-segments.md)

For disabling specific segments:
- Setting IsEnabled per segment
- Visual feedback for disabled state
- Preventing user interaction
- Conditional disabling scenarios

### Events

📄 **Read:** [references/events.md](references/events.md)

For handling user interactions:
- SelectionChanged event
- SegmentTapped event
- Event arguments and accessing segment data
- Programmatic response to selection
- Event-driven UI updates

### Advanced Features

📄 **Read:** [references/advanced-features.md](references/advanced-features.md)

For special capabilities:
- Right-to-left (RTL) support and FlowDirection
- Liquid glass effect styling
- Custom visual effects
- Platform-specific considerations

