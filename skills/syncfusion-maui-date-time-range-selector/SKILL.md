---
name: syncfusion-maui-date-time-range-selector
description: Implements Syncfusion .NET MAUI DateTime Range Selector (SfDateTimeRangeSelector) for interactive date/time range selection with chart integration. Use when working with range selectors, date range pickers, time range selection, chart range filtering, or interactive range controls. Covers range selection UI, filtering chart data by date range, and selecting time periods.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing .NET MAUI Date Time Range Selector

The Syncfusion .NET MAUI Date Time Range Selector (`SfDateTimeRangeSelector`) is a highly interactive UI control that enables users to select a range of DateTime values within specified minimum and maximum limits. It's designed to work seamlessly with charts and other visual content, providing an intuitive way to filter and explore time-based data.

## When to Use This Skill

Use this skill when user need to:

- **Implement date/time range selection** - Allow users to select a date or time range
- **Filter chart data interactively** - Enable users to zoom or filter charts by selecting time periods
- **Create time period selectors** - Build UI for selecting quarters, months, weeks, or custom periods
- **Add range sliders for dates** - Implement slider controls that work with DateTime values
- **Build data exploration tools** - Create interactive controls for analyzing time-series data
- **Customize range selector appearance** - Style labels, ticks, thumbs, tracks, and regions
- **Handle range selection events** - Respond to user interactions with range changes
- **Integrate with charts** - Embed Syncfusion Charts or other content within the range selector

## Component Overview

- DateTime range selection with draggable thumbs
- Chart content integration for data visualization
- Customizable labels, ticks, and dividers
- Discrete selection with step intervals
- Tooltip support with custom formatting
- Rich styling options for all visual elements
- Region highlighting (active/inactive areas)
- Multiple drag behaviors
- Event-driven architecture with commands
- Liquid Glass effect for modern UI (iOS 26+, macOS 26+)

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- NuGet package installation (`Syncfusion.Maui.Sliders`)
- Handler registration (`ConfigureSyncfusionCore`)
- Basic DateTime Range Selector implementation
- Adding chart content
- Minimum, Maximum, RangeStart, RangeEnd configuration

### Interval and Formatting
📄 **Read:** [references/interval-and-formatting.md](references/interval-and-formatting.md)
- Date interval configuration (`Interval`, `IntervalType`)
- Interval types (Years, Months, Days, Hours, Minutes, Seconds)
- Auto interval calculation
- Date format patterns (`DateFormat` property)
- Custom interval scenarios

### Labels Configuration
📄 **Read:** [references/labels.md](references/labels.md)
- Enabling labels (`ShowLabels`)
- Date format customization
- Label placement (`LabelsPlacement`: OnTicks, BetweenTicks)
- Edge labels placement (`EdgeLabelsPlacement`)
- Label styling (colors, fonts, sizes for active/inactive)
- Label offset customization
- Custom label text via `LabelCreated` event

### Ticks and Dividers
📄 **Read:** [references/ticks-and-dividers.md](references/ticks-and-dividers.md)
- Major ticks configuration (`ShowTicks`)
- Minor ticks (`MinorTicksPerInterval`)
- Tick styling (colors, sizes, offsets)
- Dividers display (`ShowDividers`)
- Divider customization (radius, colors, strokes)
- Active/inactive tick states

### Track and Regions
📄 **Read:** [references/track-and-regions.md](references/track-and-regions.md)
- Track properties (Minimum, Maximum, range values)
- Track color customization (active/inactive)
- Track height (`ActiveSize`, `InactiveSize`)
- Track extent configuration
- Region fill and stroke colors
- Region stroke thickness
- Active/inactive region differentiation

### Thumb and Overlay
📄 **Read:** [references/thumb-and-overlay.md](references/thumb-and-overlay.md)
- Thumb size configuration (`Radius`)
- Thumb fill and stroke colors
- Thumb overlap handling (`OverlapStroke`)
- Thumb overlay customization
- Overlay size and colors
- Interactive visual feedback

### Tooltips
📄 **Read:** [references/tooltip.md](references/tooltip.md)
- Enabling tooltips
- ShowAlways mode for persistent display
- Tooltip styling (fill, stroke, padding)
- Text customization (color, font, size)
- Date format in tooltips
- Custom tooltip text via `TooltipLabelCreated` event

### Selection and Interaction
📄 **Read:** [references/selection-and-interaction.md](references/selection-and-interaction.md)
- Discrete selection (`StepDuration`)
- Interval selection mode (`EnableIntervalSelection`)
- Drag behaviors (OnThumb, BetweenThumbs, Both)
- Deferred updates (`EnableDeferredUpdate`, `DeferredUpdateDelay`)
- Touch and mouse interaction
- IsInversed property

### Events and Commands
📄 **Read:** [references/events-and-commands.md](references/events-and-commands.md)
- ValueChangeStart/End events
- ValueChanging event with `DateTimeRangeSelectorValueChangingEventArgs`
- ValueChanged event with `DateTimeRangeSelectorValueChangedEventArgs`
- LabelCreated and TooltipLabelCreated events
- DragStartedCommand and DragCompletedCommand
- Command parameters
- MVVM pattern implementation

### Advanced: Liquid Glass Effect
📄 **Read:** [references/liquid-glass-effect.md](references/liquid-glass-effect.md)
- Modern translucent glass-like UI effect
- `EnableLiquidGlassEffect` property
- Platform requirements (.NET 10, iOS 26+, macOS 26+)
- Visual design principles
- Implementation examples

