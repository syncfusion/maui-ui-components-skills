---
name: syncfusion-maui-funnel-charts
description: Implements and customize Syncfusion .NET MAUI Funnel Charts (SfFunnelChart). Use when implementing funnel charts, sales funnel visualization, conversion funnel analysis, process stage visualization, or marketing funnel tracking. Covers funnel chart implementation, customization, data labels, legends, tooltips, segment spacing, and orientation.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Funnel Charts in .NET MAUI

A comprehensive skill for implementing and customizing Syncfusion .NET MAUI Funnel Charts (SfFunnelChart). Funnel charts visualize data as progressively decreasing segments, ideal for representing stages in a process like sales pipelines, conversion funnels, or marketing campaigns.

## When to Use This Skill

Use this skill when you need to:
- **Create funnel charts** to visualize process stages (sales, marketing, conversion)
- **Display hierarchical data** with progressively narrowing segments
- **Show conversion rates** or drop-off analysis
- **Implement customizable funnel** visualizations with data labels and legends
- **Export funnel charts** as images

## Component Overview

**SfFunnelChart** is a .NET MAUI control that creates beautiful funnel segments to analyze various stages in a process. Key capabilities include:

- **User Interaction:** Selection, tooltips, and legend toggling
- **Data Labels:** Display values with customizable placement and styling
- **Legend Support:** Scrollable legends with item identification
- **Customization:** Appearance, colors, gradients, spacing, and effects
- **Orientation:** Vertical or horizontal funnel display
- **Exporting:** Save charts as images

> **Notice:** After Volume 1 2025 (Mid March 2025), feature enhancements for this control will no longer be available in the Syncfusion package. Please switch to the **Syncfusion Toolkit for .NET MAUI** for continued support.

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- NuGet package installation (`Syncfusion.Maui.Charts`)
- Basic SfFunnelChart implementation (XAML and C#)
- Creating view models and data models
- Data binding with ItemsSource, XBindingPath, YBindingPath
- Adding title, legend, tooltips, and data labels
- Complete working example

### Data Labels
📄 **Read:** [references/data-labels.md](references/data-labels.md)
- Enabling data labels (ShowDataLabels property)
- Label placement options (Auto, Inner, Center, Outer)
- Context configuration (XValue, YValue display)
- Label style customization (font, colors, margins, borders)
- UseSeriesPalette for segment-colored labels

### Appearance and Customization
📄 **Read:** [references/appearance.md](references/appearance.md)
- Custom PaletteBrushes for segment colors
- Applying gradients (LinearGradientBrush, RadialGradientBrush)
- Creating custom color schemes
- GradientStops configuration
- Visual design best practices

### Legend
📄 **Read:** [references/legend.md](references/legend.md)
- Defining and initializing ChartLegend
- Legend visibility and placement (Top, Bottom, Left, Right)
- Label style customization
- Legend icon types
- Floating legend with OffsetX/OffsetY
- Toggle series visibility
- Items layout and ItemTemplate
- LegendItemCreated event

### Tooltip
📄 **Read:** [references/tooltip.md](references/tooltip.md)
- Enabling tooltips (EnableTooltip property)
- Tooltip template customization
- Binding context and data formatting
- Custom tooltip appearance

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- Orientation (Vertical vs Horizontal)
- Segment spacing configuration
- Liquid Glass Effect implementation
- Gap ratio and size settings
- Visual effects and polish

### Exporting
📄 **Read:** [references/exporting.md](references/exporting.md)
- Export chart as image
- Export format options
- Sharing and saving charts

