---
name: syncfusion-maui-circular-charts
description: Implements Syncfusion .NET MAUI Circular Charts (SfCircularChart) including pie, doughnut, and radial bar chart types. Use when implementing circular data visualization, pie charts, doughnut charts, or radial bar charts. Ideal for percentage breakdowns, category comparisons, part-to-whole relationships, or displaying proportional data in circular format.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing .NET MAUI Circular Charts

This skill guides you through implementing Syncfusion .NET MAUI Circular Charts (SfCircularChart), which provides pie, doughnut, and radial bar chart visualizations for displaying part-to-whole relationships and data distributions.

## When to Use This Skill

Use this skill when you need to:
- **Display proportional data** - Show how parts contribute to a whole (market share, budget allocation, survey results)
- **Create pie charts** - Standard circular charts divided into slices
- **Implement doughnut charts** - Circular charts with a center hole, optionally displaying additional information
- **Build radial bar charts** - Compare values across categories with circular progress indicators
- **Show data distributions** - Visualize percentage breakdowns or category comparisons
- **Enable chart interactions** - Add tooltips, explode effects, selection, and legends
- **Group small values** - Automatically combine small data segments into an "Others" category
- **Export visualizations** - Save charts as images or PDFs
- **Customize appearance** - Style colors, strokes, spacing, and visual effects

## Component Overview

**SfCircularChart** is the .NET MAUI control for circular data visualization with these capabilities:

- **Three chart types**: PieSeries, DoughnutSeries, RadialBarSeries
- **Data binding**: Bind to collections with XBindingPath and YBindingPath
- **Interactive features**: Tooltips, explode, selection, legends
- **Customization**: Colors, strokes, spacing, angles, radius control
- **Data labels**: Show values/percentages on segments
- **Grouping**: Combine small values into "Others" category
- **Center content**: Display custom views in doughnut/radial bar centers
- **Export**: Save as PNG, JPEG, or PDF

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- NuGet package installation (Syncfusion.Maui.Charts)
- Basic SfCircularChart setup and initialization
- Creating data models and view models
- Binding data to chart series
- Adding title, legend, and data labels
- Complete minimal working example

### Chart Types
📄 **Read:** [references/chart-types.md](references/chart-types.md)
- PieSeries - Standard pie chart implementation
- DoughnutSeries - Doughnut chart with inner radius and center view
- RadialBarSeries - Circular progress/comparison bars
- Radius property for sizing charts
- StartAngle and EndAngle for semi-circular charts
- InnerRadius configuration for doughnut and radial bar
- CenterView and CenterHoleSize for center content
- Code examples for all chart types

### Data Labels
📄 **Read:** [references/data-labels.md](references/data-labels.md)
- Enabling and configuring data labels
- Label positioning and alignment
- Custom label templates
- Smart label positioning to prevent overlap
- Label connectors for external labels
- Label formatting (values, percentages, custom format)
- Styling and appearance customization

### Legend
📄 **Read:** [references/legend.md](references/legend.md)
- Enabling ChartLegend
- Legend positioning (top, bottom, left, right)
- Legend icon types and customization
- Custom legend templates
- Toggle visibility functionality
- Legend styling and appearance

### Appearance and Styling
📄 **Read:** [references/appearance.md](references/appearance.md)
- PaletteBrushes for segment colors
- Stroke and StrokeWidth for borders
- Opacity settings for transparency
- GapRatio for segment spacing (radial bar)
- MaximumValue for radial bar range
- CapStyle customization (flat, curved ends)
- Track customization for radial bar (TrackFill, TrackStroke)
- Custom brushes and gradient fills

### Tooltip
📄 **Read:** [references/tooltip.md](references/tooltip.md)
- Enabling tooltips with EnableTooltip
- Default tooltip behavior
- Custom tooltip templates
- Tooltip content customization
- Styling tooltip appearance
- Interactive hover behavior

### Explode Feature
📄 **Read:** [references/explode.md](references/explode.md)
- Exploding chart segments outward
- ExplodeIndex to specify which segments explode
- ExplodeRadius for explosion distance
- ExplodeOnTouch for user interaction
- Exploding multiple segments
- Visual emphasis and separation

### Selection
📄 **Read:** [references/selection.md](references/selection.md)
- Enabling selection behavior
- SelectionBehavior configuration
- Selection modes (single, multiple)
- Handling selected index
- Visual feedback for selection
- Multi-segment selection

### Data Grouping
📄 **Read:** [references/groupto.md](references/groupto.md)
- GroupTo property for automatic grouping
- Combining small values into "Others" category
- Setting grouping threshold
- Custom group labels and appearance
- Improving chart readability with many small segments

### Exporting Charts
📄 **Read:** [references/exporting.md](references/exporting.md)
- Exporting as PNG or JPEG images
- Exporting as PDF documents
- Using SaveAsImage and GetStreamAsync methods
- Stream-based export
- Export quality and resolution settings
- Code examples for each export format

