---
name: syncfusion-maui-linear-gauge
description: Implements Syncfusion .NET MAUI Linear Gauge (SfLinearGauge) for data visualization with scales, pointers, and ranges. Use when creating linear gauges, displaying measurements on linear scales, showing progress indicators, or building thermometer-style visualizations. Covers gauge pointer configuration, bar pointers or shape markers, draggable gauge pointers, and animated gauge transitions.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Linear Gauges in .NET MAUI

The Syncfusion .NET MAUI Linear Gauge (SfLinearGauge) is a powerful data visualization control for displaying values on a linear scale. Use it to create intuitive measurement displays, progress indicators, thermometer-style visualizations, and interactive slider controls in your .NET MAUI applications.

## When to Use This Skill

Use this skill when you need to:

- **Display measurements on linear scales** - Temperature, pressure, volume, speed, or any linear metric
- **Create progress indicators** - Download progress, task completion, loading states with visual feedback
- **Build interactive gauges** - User-adjustable sliders, draggable value selectors
- **Show performance ranges** - Color-coded zones (e.g., red/yellow/green performance indicators)
- **Visualize multiple data points** - Multiple pointers on the same scale for comparison
- **Implement vertical or horizontal displays** - Thermometers, fuel gauges, battery indicators
- **Create animated visualizations** - Smooth transitions when values change
- **Build mirrored gauge layouts** - RTL support or symmetric dual displays

## Component Overview

**Key Capabilities:**
- **Flexible orientation** - Vertical or horizontal layout
- **Customizable scale** - Adjustable min/max values, intervals, appearance
- **Multiple pointer types** - Bar pointers, shape markers, custom content markers
- **Visual ranges** - Color-coded zones to highlight specific value ranges
- **Interactive gestures** - Drag and swipe to change pointer values
- **Rich customization** - Labels, ticks, styling, gradients, corner styles
- **Animation support** - Smooth value transitions and loading animations
- **Mirror effect** - RTL layouts and symmetric designs

## Documentation and Navigation Guide

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)

**When to read:** First-time setup, installation, or basic implementation

Topics covered:
- Installing Syncfusion.Maui.Gauges NuGet package
- Registering Syncfusion handler in MauiProgram.cs
- Creating your first linear gauge
- Adding basic scale elements
- Adding ranges and pointers
- Complete working example
- Troubleshooting setup issues

### Scale Configuration

📄 **Read:** [references/scale-configuration.md](references/scale-configuration.md)

**When to read:** Configuring gauge scale properties, min/max values, intervals, or appearance

Topics covered:
- Default scale behavior (0-100)
- Customizing minimum and maximum values
- Setting scale intervals
- Scale appearance (thickness, fill color, edge styles)
- Orientation (vertical vs horizontal)
- Inverse scale (reversed value direction)
- Scale positioning and offset
- Common scale patterns

### Labels and Ticks

📄 **Read:** [references/labels-and-ticks.md](references/labels-and-ticks.md)

**When to read:** Customizing scale labels, formatting text, or configuring tick marks

Topics covered:
- Label positioning and alignment
- Label text formatting (number formats, custom text)
- Label styling (font, color, size)
- Label offset and visibility
- Major ticks configuration
- Minor ticks configuration
- Tick positioning, length, and style
- Hiding ticks or labels
- Complete customization examples

### Ranges

📄 **Read:** [references/ranges.md](references/ranges.md)

**When to read:** Adding visual ranges, color-coded zones, or performance indicators

Topics covered:
- Range overview and purpose
- Basic range implementation
- Range shape customization (start/mid/end width)
- Range positioning (start/end values)
- Range styling (solid fills, gradients)
- Multiple ranges on same scale
- Range positioning relative to scale
- Corner styles and appearance
- Performance zones and color coding examples

### Bar Pointer

📄 **Read:** [references/bar-pointer.md](references/bar-pointer.md)

**When to read:** Implementing bar-style pointers (filled progress bars)

Topics covered:
- Bar pointer overview
- Basic bar pointer implementation
- Setting pointer value and position
- Bar styling (fill, gradient, opacity)
- Corner styles for bars
- Bar offset and positioning
- Animation on value changes
- Multiple bar pointers
- Pointer events
- Common bar pointer patterns

### Marker Pointers

📄 **Read:** [references/marker-pointers.md](references/marker-pointers.md)

**When to read:** Adding shape markers or custom content pointers

Topics covered:
- Shape marker pointer (built-in shapes)
  - Available shapes: circle, triangle, diamond, inverted triangle, rectangle
  - Shape size, fill, and stroke customization
  - Shape positioning and offset
- Content marker pointer (custom content)
  - Using images as pointers
  - Text-based pointers
  - Custom views as pointers
  - Content alignment and positioning
- Multiple marker pointers on same scale
- Marker animation effects
- Common marker patterns and use cases

### Interaction

📄 **Read:** [references/interaction.md](references/interaction.md)

**When to read:** Enabling user interaction, drag gestures, or pointer events

Topics covered:
- Interaction overview
- Enabling IsInteractive property
- Drag gesture handling
- Swipe gesture handling
- ValueChangeStarted event
- ValueChanging event (real-time updates)
- ValueChangeCompleted event
- Constraining pointer movement
- Interactive pointer examples
- Multi-pointer interaction scenarios

### Animation and Effects

📄 **Read:** [references/animation-and-effects.md](references/animation-and-effects.md)

**When to read:** Adding animations, mirror effects, or special visual effects

Topics covered:
- Animation overview
- Enabling animation on load
- Animation duration and easing
- Pointer animation on value changes
- Range animation effects
- Mirror gauge effect (IsMirrored)
- RTL support and mirrored layouts
- Orientation changes and transitions
- Complete animation examples

