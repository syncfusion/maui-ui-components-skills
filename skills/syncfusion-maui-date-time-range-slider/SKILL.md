---
name: syncfusion-maui-date-time-range-slider
description: Implements Syncfusion .NET MAUI DateTime Range Slider (SfDateTimeRangeSlider) control. Use when implementing range sliders, date range pickers, time range selectors, dual-thumb sliders, or interactive range selection controls in .NET MAUI applications. This skill covers installation, configuration, track styling, labels, ticks, dividers, tooltips, thumb customization, and the Liquid Glass Effect.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing .NET MAUI Date Time Range Slider

This skill provides comprehensive guidance for implementing the Syncfusion .NET MAUI DateTime Range Slider (`SfDateTimeRangeSlider`) control, a highly interactive UI component that enables users to select a range of DateTime values with dual thumbs.

## When to Use This Skill

Use this skill when the user needs to:

- Implement a **range slider** for selecting DateTime ranges in .NET MAUI apps
- Create **date range pickers** with visual track, thumbs, and labels
- Build **time range selectors** with hour/minute/second granularity
- Add **dual-thumb sliders** for min-max date/time selection
- Implement **interactive calendars** with range selection via slider
- Customize **track, thumb, tick, label, divider, or tooltip** appearance
- Configure **discrete stepping** through DateTime values
- Handle **value change events** or implement **MVVM commands**
- Apply the modern **Liquid Glass Effect** on iOS 26+/macOS 26+ platforms
- Support **horizontal and vertical orientations** with RTL
- Style **active/inactive regions** separately for selected ranges

This skill covers the complete API surface including installation, configuration, styling, events, commands, and advanced features like deferred updates and interval selection.

## Component Overview

**Control:** `SfDateTimeRangeSlider` (Syncfusion .NET MAUI Range Slider)  
**Namespace:** `Syncfusion.Maui.Sliders`  
**Package:** `Syncfusion.Maui.Sliders` (NuGet)  
**Platform Support:** .NET MAUI (iOS, Android, macOS, Windows)

### Key Features

- **DateTime Support** - Select DateTime ranges with built-in date/time formatting
- **Dual Thumbs** - Start and end thumbs for range selection
- **Flexible Intervals** - Years, Months, Days, Hours, Minutes, Seconds
- **Labels & Ticks** - Customizable labels, major/minor ticks, and dividers
- **Tooltips** - Interactive tooltips showing selected values
- **Orientation** - Horizontal and vertical layouts with inverse support
- **Discrete Mode** - Step through values at specific intervals
- **Drag Behaviors** - OnThumb, BetweenThumbs, or Both modes
- **Rich Styling** - Separate active/inactive styling for all elements
- **Events & Commands** - Complete lifecycle events with MVVM support
- **Liquid Glass Effect** - Modern translucent design (iOS 26+/macOS 26+/.NET 10+)
- **Visual State Manager** - Enabled/Disabled state customization

## Documentation and Navigation Guide

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)

When you need:
- Installation and package setup
- Handler registration (ConfigureSyncfusionCore)
- Basic DateTime Range Slider implementation
- Minimal working example
- Quick start code (XAML and C#)
- Initial configuration troubleshooting

### Track and Range Configuration

📄 **Read:** [references/track-and-range.md](references/track-and-range.md)

When you need:
- Set Minimum and Maximum DateTime values
- Configure RangeStart and RangeEnd (selected range)
- Change orientation (Horizontal/Vertical)
- Inverse slider direction
- Customize track colors (ActiveFill/InactiveFill)
- Adjust track height/size
- Extend track at edges (TrackExtent)
- Style disabled states with Visual State Manager

### Labels, Intervals, and Formatting

📄 **Read:** [references/interval-and-labels.md](references/interval-and-labels.md)

When you need:
- Configure interval values and types (Years, Months, Days, etc.)
- Enable auto-interval calculation
- Format DateTime labels (DateFormat strings)
- Show/hide labels
- Position labels (OnTicks vs BetweenTicks)
- Place edge labels (Default vs Inside)
- Style active/inactive labels separately
- Customize label colors, fonts, and sizes
- Adjust label offset from ticks
- Create custom label text via LabelCreated event

### Ticks and Dividers

📄 **Read:** [references/ticks-and-dividers.md](references/ticks-and-dividers.md)

When you need:
- Show/hide major and minor ticks
- Configure ticks per interval
- Customize tick colors (active/inactive)
- Adjust tick sizes
- Set tick offset from track
- Enable dividers on the track
- Style divider radius, colors, and strokes
- Apply Visual State Manager for disabled states

### Tooltips and Thumbs

📄 **Read:** [references/tooltip-and-thumb.md](references/tooltip-and-thumb.md)

When you need:
- Enable and configure tooltips
- Show tooltips always (without interaction)
- Style tooltip appearance (fill, stroke, text)
- Format tooltip DateTime values
- Customize tooltip text via TooltipLabelCreated event
- Configure thumb size, colors, and stroke
- Style thumb overlap behavior
- Customize thumb overlay (interaction halo)
- Apply disabled states

### Selection and Interaction

📄 **Read:** [references/selection-and-interaction.md](references/selection-and-interaction.md)

When you need:
- Enable discrete selection (StepDuration)
- Configure interval selection mode
- Set drag behavior (OnThumb, BetweenThumbs, Both)
- Enable deferred updates for performance
- Adjust deferred update delay
- Handle touch and gesture interactions
- Understand different selection patterns

### Events and Commands

📄 **Read:** [references/events-and-commands.md](references/events-and-commands.md)

When you need:
- Handle value change lifecycle events
- Listen to ValueChangeStart, ValueChanging, ValueChanged, ValueChangeEnd
- Access event arguments (OldStart, OldEnd, NewStart, NewEnd)
- Cancel value changes
- Customize labels via LabelCreated event
- Customize tooltips via TooltipLabelCreated event
- Implement MVVM with DragStartedCommand/DragCompletedCommand
- Use command parameters
- Enable Liquid Glass Effect (platform-specific)

