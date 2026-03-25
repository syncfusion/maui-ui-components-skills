---
name: syncfusion-maui-radial-gauge
description: Implements Syncfusion .NET MAUI Radial Gauge (SfRadialGauge) components. Use when working with radial gauges, circular gauges, speedometers, temperature monitors, meter gauges, or circular data visualization in .NET MAUI applications. This skill covers installation, axis configuration, ranges, pointer types (needle, shape, content, range), annotations, and animation.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion .NET MAUI Radial Gauges

## Overview

The Syncfusion .NET MAUI Radial Gauge (SfRadialGauge) is a versatile data visualization control that displays numerical values on a circular scale. It's ideal for creating speedometers, temperature monitors, dashboards, progress indicators, compasses, and other circular metric displays.

## When to Use This Skill

Use this skill when the user needs to:
- **Create circular gauges** - Speedometers, tachometers, temperature gauges, pressure meters
- **Build dashboard visualizations** - KPI displays, metric monitoring, status indicators
- **Show progress** - Circular progress bars, completion indicators, loading states
- **Display ranges** - Visual zones (safe/warning/danger), threshold indicators
- **Implement interactive controls** - Draggable dials, adjustable settings, user input controls
- **Add animated transitions** - Smooth value changes, real-time data updates
- **Build compass/direction displays** - Navigation aids, orientation indicators

## Component Overview
 
The **SfRadialGauge** control provides:
- **Multiple Pointer Types**: 4 pointer styles (Needle, Shape, Content, Range) for versatile value indication
- **Flexible Axis Configuration**: Customizable scale ranges, angles (full circle, semi-circle, arc), labels, and ticks
- **Visual Range Indicators**: Color-coded zones with gradient support for safe/warning/danger visualization
- **Custom Annotations**: Place text, images, or any .NET MAUI view at specific positions using angle or axis value
- **Animation System**: Smooth pointer transitions with configurable duration and easing functions
- **Interactive Controls**: Draggable pointers with ValueChanged/ValueChanging events for user input
- **Multiple Axes Support**: Display several axes on one gauge for complex multi-metric visualizations
- **Styling Options**: Comprehensive customization of colors, gradients, thickness (pixel or factor-based sizing)

## Key Features at a Glance

- **Flexible Axes** - Linear/custom scales, customizable labels, ticks, and styling
- **Visual Ranges** - Color-coded zones for quick value interpretation
- **4 Pointer Types** - Needle, Shape (marker), Content (custom views), Range (arc)
- **Multiple Pointers** - Display multiple values on the same gauge
- **Annotations** - Add text, images, or custom views at specific positions
- **Animation** - Smooth transitions with customizable easing
- **Interactive** - Draggable pointers for user input
- **Highly Customizable** - Full control over appearance and behavior

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

When the user needs to:
- Install and set up the Radial Gauge package
- Configure NuGet packages and dependencies
- Register Syncfusion handlers in MauiProgram.cs
- Create their first basic radial gauge
- Understand the basic structure and components
- Get a working example quickly

### Axis Configuration
📄 **Read:** [references/axes.md](references/axes.md)

When the user needs to:
- Configure axis scale (minimum, maximum, interval)
- Customize axis start and end angles (circular, semi-circular, arc)
- Style axis lines (thickness, colors, gradients)
- Configure labels (position, rotation, formatting, custom text)
- Customize ticks (major/minor, length, position, styling)
- Add background content to the gauge
- Create multiple axes on a single gauge
- Understand axis positioning and factor-based layouts

### Ranges and Visual Indicators
📄 **Read:** [references/ranges.md](references/ranges.md)

When the user needs to:
- Create color-coded ranges (e.g., safe/warning/danger zones)
- Set range start and end values
- Style ranges (colors, gradients, width)
- Position ranges (offset, inside/outside axis)
- Add multiple ranges to visualize zones
- Customize range caps (rounded, flat)
- Create reverse order ranges for 360° gauges
- Build speedometer-style indicators

### Pointers (All Types)
📄 **Read:** [references/pointers.md](references/pointers.md)

When the user needs to:
- **Needle Pointer** - Classic gauge needle with knob and tail
- **Shape Pointer** - Marker symbols (circle, diamond, triangle, etc.)
- **Content Pointer** - Custom views (text, images, complex layouts)
- **Range Pointer** - Colored arc that fills from start to value
- Set pointer values and animate transitions
- Add multiple pointers to one gauge
- Customize pointer appearance (colors, sizes, shapes)
- Position pointers (offset from axis)
- Choose the right pointer type for the use case

### Annotations
📄 **Read:** [references/annotation.md](references/annotation.md)

When the user needs to:
- Add text labels to display values or units
- Place images or icons on the gauge
- Add custom views at specific positions
- Position annotations using angle or axis value
- Control radial distance (position factor)
- Create multiple annotations
- Display dynamic content based on pointer values
- Build complex gauge layouts with labels and status indicators

### Animation and Interaction
📄 **Read:** [references/animation-and-interaction.md](references/animation-and-interaction.md)

When the user needs to:
- Enable smooth pointer animations
- Configure animation duration and easing
- Create spring or bounce effects
- Make pointers draggable for user input
- Handle pointer value changes
- Respond to user interactions
- Build interactive controls (volume, brightness, settings)
- Optimize animation performance

