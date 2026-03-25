---
name: syncfusion-maui-circular-progressbar
description: Implements and customize Syncfusion .NET MAUI Circular ProgressBar (SfCircularProgressBar) components. Use when working with circular progress bars, circular progress indicators, radial progress, progress circles, or arc progress in .NET MAUI applications. Covers determinate/indeterminate progress states, animated progress indicators, segmented circular progress, and custom angles.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Circular ProgressBars in .NET MAUI

The Syncfusion .NET MAUI Circular ProgressBar (SfCircularProgressBar) displays progress in a circular shape with rich customization options. It supports determinate and indeterminate states, smooth animations, segmented progress, gradient colors, custom angles, and center content. Perfect for task completion, loading indicators, dashboards, and progress tracking.

## When to Use This Skill

Use this skill when the user needs to:

- **Display circular progress indicators** for tasks, downloads, uploads, or operations
- **Implement determinate progress** with known completion percentages (0-100%)
- **Show indeterminate progress** when duration is unknown (loading, processing)
- **Create segmented progress** for multi-step workflows or sequential tasks
- **Customize appearance** with angles, colors, gradients, thickness, or corner styles
- **Add animations** with easing effects for smooth progress transitions
- **Display custom content** at the center (text, icons, buttons, images)
- **Handle progress events** to respond to value changes or completion
- **Build semi-circle or arc progress** indicators with custom angles
- **Visualize progress ranges** with multiple colors for different states

## Component Overview

The SfCircularProgressBar control provides:

- **Flexible States:** Determinate (known progress) and indeterminate (unknown duration)
- **Visual Customization:** Angles, thickness, colors, gradients, corner styles
- **Segmentation:** Split progress into multiple segments with configurable gaps
- **Animation Support:** Smooth transitions with customizable duration and easing
- **Range Configuration:** Custom min/max values (default 0-100)
- **Center Content:** Add any MAUI view (text, buttons, images) to the center
- **Events:** ProgressChanged and ProgressCompleted for interaction
- **Rich Gradient Support:** Multiple color ranges with solid or gradient transitions

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and NuGet package setup
- Register Syncfusion handler in MauiProgram.cs
- Basic circular progress bar implementation (XAML & C#)
- First working example

### States and Progress Modes
📄 **Read:** [references/states.md](references/states.md)
- Determinate state (default, for known progress)
- Indeterminate state (for unknown duration)
- When to use each state
- Combining determinate and indeterminate modes

### Segments
📄 **Read:** [references/segments.md](references/segments.md)
- Visualizing multiple sequential tasks
- SegmentCount property for splitting progress
- Gap customization between segments
- Use cases for segmented progress

### Appearance Customization
📄 **Read:** [references/appearance.md](references/appearance.md)
- Angle customization (StartAngle, EndAngle) for semi-circles and arcs
- Range colors with GradientStops (solid and gradient)
- Thickness and radius configuration (Pixel vs Factor)
- Corner style customization (flat, curved)
- Progress and track color customization

### Animation
📄 **Read:** [references/animation.md](references/animation.md)
- Animation duration for determinate and indeterminate states
- Easing effects (Linear, CubicInOut, BounceIn, etc.)
- SetProgress() method with custom animation
- Controlling animation speed and behavior

### Range Configuration
📄 **Read:** [references/range.md](references/range.md)
- Defining custom progress ranges
- Minimum and Maximum properties
- Using factor values (0-1) vs percentages (0-100)

### Custom Content
📄 **Read:** [references/custom-content.md](references/custom-content.md)
- Adding views to the center (Content property)
- Custom text with data binding
- Buttons for controlling progress
- Images and icons as indicators
- Layout examples

### Events
📄 **Read:** [references/events.md](references/events.md)
- ProgressChanged event for value changes
- ProgressCompleted event for completion
- Handling event arguments
- Dynamic behavior based on progress

