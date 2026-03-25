---
name: syncfusion-maui-range-slider
description: Implements Syncfusion .NET MAUI Range Slider (SfRangeSlider) for selecting numeric range values with dual thumbs. Use this for range selection controls, dual-value sliders, price range selectors, date range filters, or scenarios requiring minimum-maximum value selection with interactive thumbs.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing .NET MAUI Range Slider

The Syncfusion .NET MAUI Range Slider (SfRangeSlider) is a highly interactive UI control that allows users to select a range of values between a defined minimum and maximum. It offers a rich set of features, including track customization, labels, ticks, dividers, and tooltips, enabling the creation of intuitive and visually appealing range selection experiences.

## When to Use This Skill

Use this skill when the user needs to:

- Implement a range slider with dual thumbs for selecting minimum and maximum values
- Create price range selectors (e.g., filter products between $50-$500)
- Build date/time range selection controls
- Implement volume, brightness, or temperature range controls
- Create numeric range filters for data visualization
- Build custom range input controls with labels, ticks, and tooltips
- Implement discrete or continuous range selection with step values
- Customize range slider appearance with colors, sizes, and styles
- Handle range value changes with events and commands
- Create accessible range selection interfaces with ARIA support

**Key capabilities**: Dual-thumb interaction, numeric ranges, intervals, discrete selection, custom styling, tooltips, labels, ticks, dividers, events, MVVM commands, RTL support, and liquid glass effects.

## Component Overview

The Syncfusion .NET MAUI Range Slider (`SfRangeSlider`) is an interactive UI control for selecting a range of values between a minimum and maximum limit. It provides dual thumbs that users drag to define start and end values, with rich visual features including track styling, labels, ticks, dividers, and tooltips.

**Package**: `Syncfusion.Maui.Sliders`  
**Namespace**: `Syncfusion.Maui.Sliders`  
**Control**: `SfRangeSlider`

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)  
When implementing your first range slider. Covers:
- NuGet package installation (Syncfusion.Maui.Sliders)
- Handler registration (ConfigureSyncfusionCore)
- Basic SfRangeSlider initialization
- Namespace imports and minimal examples
- IDE-specific setup (Visual Studio, VS Code, Rider)

### Track Customization
📄 **Read:** [references/track.md](references/track.md)  
When customizing the slider track appearance and behavior. Covers:
- Setting Minimum, Maximum, RangeStart, RangeEnd
- Track colors (ActiveFill, InactiveFill)
- Track sizing (ActiveSize, InactiveSize)
- Track extent for edge extension
- Orientation (Horizontal, Vertical)
- IsInversed property for reversed direction

### Thumbs and Overlays
📄 **Read:** [references/thumbs-and-overlays.md](references/thumbs-and-overlays.md)  
When customizing thumb appearance and interaction feedback. Covers:
- Thumb size (Radius)
- Custom thumb icons (StartThumbIcon, EndThumbIcon)
- Thumb colors (Fill, Stroke, StrokeThickness)
- OverlapStroke for overlapping thumbs
- Thumb overlay configuration
- Visual states for disabled thumbs

### Labels and Formatting
📄 **Read:** [references/labels.md](references/labels.md)  
When displaying and formatting value labels. Covers:
- ShowLabels property
- NumberFormat for custom formatting
- Label placement (OnTicks, BetweenTicks)
- Edge labels placement
- Label styles (colors, fonts, offset)
- LabelCreated event for dynamic formatting

### Ticks Configuration
📄 **Read:** [references/ticks.md](references/ticks.md)  
When adding visual markers on the track. Covers:
- ShowTicks for major ticks
- MinorTicksPerInterval for minor ticks
- Tick colors (ActiveFill, InactiveFill)
- Tick sizes (ActiveSize, InactiveSize)
- Tick offset and styling
- Visual states for disabled ticks

### Dividers
📄 **Read:** [references/dividers.md](references/dividers.md)  
When adding visual separators at interval points. Covers:
- ShowDividers property
- Divider radius and colors
- Divider stroke and thickness
- DividerStyle configuration
- Visual states for disabled dividers

### Intervals and Selection
📄 **Read:** [references/intervals-and-selection.md](references/intervals-and-selection.md)  
When configuring interval behavior and selection modes. Covers:
- Interval property for spacing
- StepSize for discrete selection
- EnableIntervalSelection
- DragBehavior (OnThumb, BetweenThumbs, Both)
- EnableDeferredUpdate for performance
- DeferredUpdateDelay configuration

### Tooltips
📄 **Read:** [references/tooltips.md](references/tooltips.md)  
When displaying value tooltips during interaction. Covers:
- Enabling tooltips (SliderTooltip)
- ShowAlways property
- Tooltip styling (Fill, Stroke, TextColor)
- Font and padding customization
- TooltipLabelCreated event for formatting

### Events and Commands
📄 **Read:** [references/events-and-commands.md](references/events-and-commands.md)  
When handling value changes and user interactions. Covers:
- ValueChangeStart, ValueChanging, ValueChanged, ValueChangeEnd
- LabelCreated and TooltipLabelCreated events
- DragStartedCommand, DragCompletedCommand
- Command parameters and MVVM patterns
- Event arguments and custom logic

### Liquid Glass Effect
📄 **Read:** [references/liquid-glass-effect.md](references/liquid-glass-effect.md)  
When implementing modern translucent design. Covers:
- EnableLiquidGlassEffect property
- Platform requirements (.NET 10, iOS 26, macOS 26)
- Background image setup
- Visual interaction feedback
- Use cases and best practices

