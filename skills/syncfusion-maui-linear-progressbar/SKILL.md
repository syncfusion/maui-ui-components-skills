---
name: syncfusion-maui-linear-progressbar
description: Implements Syncfusion .NET MAUI Linear ProgressBar (SfLinearProgressBar) control. Use when working with progress bars, progress indicators, loading indicators, determinate/indeterminate progress, or buffer progress visualization. Covers progress tracking, multi-segment progress, animated progress, and gradient progress bars.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Linear ProgressBars

A comprehensive guide for implementing and customizing the Syncfusion .NET MAUI Linear ProgressBar (SfLinearProgressBar) control. This component displays progress of tasks with rectangular shapes, determinate and indeterminate states, segments, smooth animations, and customizable visuals including range colors.

## When to Use This Skill

Use this skill when you need to:

- **Implement progress indicators** for file uploads, downloads, or data processing tasks
- **Show determinate progress** with known completion percentages (0-100%)
- **Display indeterminate progress** when task duration is unknown or cannot be calculated
- **Visualize buffer states** showing both primary and secondary progress (e.g., video buffering)
- **Create multi-segment progress bars** for sequential or multi-step tasks
- **Customize progress appearance** with colors, gradients, sizes, corner radius, or animations
- **Apply range-based colors** to show different progress zones (success, warning, danger)
- **Add progress animations** with custom durations and easing effects
- **Handle progress events** to respond when progress changes or completes
- **Implement modern UI effects** like liquid glass effect for translucent designs

## Component Overview

The **SfLinearProgressBar** is a .NET MAUI control that shows task progress in a rectangular horizontal bar. It supports:

- **Multiple States**: Determinate (known progress), Indeterminate (unknown duration), Buffer (dual progress)
- **Segments**: Split progress into multiple sections for sequential tasks
- **Rich Customization**: Colors, gradients, thickness, padding, corner radius
- **Smooth Animations**: Configurable duration and easing functions
- **Range Colors**: Different colors for different progress ranges
- **Events**: Track progress changes and completion
- **Modern Effects**: Liquid glass effect for sleek translucent UI

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

When you need to:
- Install and set up the Linear ProgressBar control
- Add the NuGet package (Syncfusion.Maui.ProgressBar)
- Register the Syncfusion Core handler
- Create your first basic progress bar
- Understand namespace imports and initialization

### Progress States
📄 **Read:** [references/states.md](references/states.md)

When you need to:
- Understand determinate state (default, known progress)
- Enable indeterminate state for unknown duration tasks
- Implement buffer state with secondary progress
- Choose the right state for your use case
- Combine states for complex scenarios

### Segments
📄 **Read:** [references/segments.md](references/segments.md)

When you need to:
- Visualize multiple sequential tasks in one progress bar
- Split progress bar into segments using SegmentCount
- Customize spacing between segments with SegmentGapWidth
- Show gradual multi-step progress

### Appearance Customization
📄 **Read:** [references/appearance.md](references/appearance.md)

When you need to:
- Apply range colors with GradientStops (solid or gradient)
- Customize track, progress, and secondary progress height
- Adjust padding around progress indicators
- Set corner radius for rounded edges
- Change colors of progress fill, track fill, and secondary progress

### Animation
📄 **Read:** [references/animation.md](references/animation.md)

When you need to:
- Configure animation duration for different states
- Apply easing effects (Linear, CubicInOut, BounceIn, etc.)
- Use SetProgress() method for one-time animation overrides
- Customize indeterminate animation behavior
- Create smooth, interactive progress transitions

### Range Customization
📄 **Read:** [references/range.md](references/range.md)

When you need to:
- Define custom Minimum and Maximum values
- Use factor values (0.0 to 1.0) instead of percentages
- Adjust progress range for specific scenarios
- Work with non-standard progress scales

### Events
📄 **Read:** [references/events.md](references/events.md)

When you need to:
- Handle ProgressChanged event to respond to progress updates
- Handle ProgressCompleted event when progress reaches maximum
- Dynamically change appearance based on progress value
- Implement custom logic triggered by progress milestones

### Liquid Glass Effect
📄 **Read:** [references/liquid-glass-effect.md](references/liquid-glass-effect.md)

When you need to:
- Apply modern translucent glass effect to progress bars
- Wrap progress bar in SfGlassEffectView
- Configure transparent backgrounds for glass appearance
- Implement sleek, contemporary UI designs
- Check platform requirements (macOS 26+, iOS 26+, .NET 10)

