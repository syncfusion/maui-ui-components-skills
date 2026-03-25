---
name: syncfusion-maui-pyramid-charts
description: Implements Syncfusion .NET MAUI Pyramid Chart (SfPyramidChart) for visually representing hierarchical, proportional, and parts-to-whole data using pyramid-shaped segments. Use this for pyramid charts, hierarchical data visualization, proportional data display, or segment-based charts. This skill covers installation, data binding, legends, tooltips, data labels, appearance customization, and gradients.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing .NET MAUI Pyramid Charts

Guide users to implement Syncfusion .NET MAUI Pyramid Chart (SfPyramidChart), which visualizes proportions of a total in hierarchical segments, making it ideal for displaying parts-to-whole relationships in high-quality .NET MAUI applications. As a single-series chart without axes, data is represented as percentages where the sum of parts equals the whole.

## When to Use This Skill

Use this skill when the user needs to:

- **Create pyramid charts** to visualize hierarchical or proportional data in .NET MAUI applications
- **Display parts-to-whole relationships** where segments represent percentages of a total
- **Implement funnel-like visualizations** for sales stages, process flows, or demographic breakdowns
- **Add interactive features** like tooltips, legends, and data labels to pyramid charts
- **Customize appearance** with palettes, gradients, segment spacing, and orientation
- **Enable modern UI effects** like liquid glass styling for iOS/macOS applications
- **Export chart visualizations** as images (JPEG/PNG) or streams for reports and documents
- **Configure data binding** from view models to pyramid chart segments

## Component Overview

The **SfPyramidChart** control provides:
- Single-series hierarchical data visualization
- Interactive tooltips and legends with customization
- Rich data label placement options (Auto, Inner, Center, Outer)
- Custom palette brushes and gradient support
- Two pyramid modes: Linear (height-based) and Surface (area-based)
- Segment spacing control with GapRatio
- Vertical and horizontal orientation options
- Modern liquid glass visual effects (iOS/macOS .NET 10+)
- Export to image formats or streams
- Toggle series visibility through legend interaction
- Scrollable legends for overflow handling

## Documentation and Navigation Guide

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)

When the user needs to:
- Install the Syncfusion.Maui.Charts NuGet package
- Initialize SfPyramidChart in XAML or C#
- Create view models and bind data to the chart
- Configure ItemsSource, XBindingPath, and YBindingPath
- Understand basic pyramid chart setup and namespace imports
- See complete working examples with data binding

### Data Labels

📄 **Read:** [references/data-labels.md](references/data-labels.md)

When the user needs to:
- Enable and display data labels on pyramid segments
- Configure PyramidDataLabelSettings for label customization
- Control label placement (Auto, Inner, Center, Outer)
- Set label context to show X values or Y values
- Style labels with fonts, colors, backgrounds, borders, and corner radius
- Use series palette colors for label backgrounds
- Apply margins and customize label appearance

### Legend

📄 **Read:** [references/legend.md](references/legend.md)

When the user needs to:
- Add and configure ChartLegend to display segment information
- Customize legend label styles (font, color, size, attributes)
- Set legend icon types and customize icon appearance
- Position legends (Top, Bottom, Left, Right)
- Create floating legends with offset positioning
- Enable toggle series visibility through legend taps
- Customize legend layout with ItemsLayout
- Use custom legend item templates
- Handle LegendItemCreated events
- Override maximum size requests for legend view

### Tooltip

📄 **Read:** [references/tooltip.md](references/tooltip.md)

When the user needs to:
- Enable tooltips for displaying segment information on hover
- Configure ChartTooltipBehavior for tooltip customization
- Style tooltip appearance (background, fonts, colors, margins)
- Control tooltip display duration
- Create custom tooltip templates for advanced UI
- Bind tooltip content to item data properties

### Appearance and Customization

📄 **Read:** [references/appearance-customization.md](references/appearance-customization.md)

When the user needs to:
- Apply custom palette brushes to pyramid segments
- Use gradient fills (LinearGradientBrush, RadialGradientBrush)
- Switch between Linear and Surface pyramid modes
- Understand mode differences (height-based vs area-based)
- Control segment spacing with GapRatio property
- Implement visual styling best practices
- Create sophisticated gradient effects for segments

### Orientation and Visual Effects

📄 **Read:** [references/orientation-and-effects.md](references/orientation-and-effects.md)

When the user needs to:
- Change chart orientation from Vertical to Horizontal
- Implement liquid glass visual effects (iOS/macOS .NET 10+)
- Integrate SfGlassEffectView for modern UI styling
- Enable glass effects for tooltips
- Configure EffectType (Regular vs Clear glass appearance)
- Apply best practices for glass effects over backgrounds
- Use transparent backgrounds in custom tooltip templates

### Exporting

📄 **Read:** [references/exporting.md](references/exporting.md)

When the user needs to:
- Export pyramid charts as images (JPEG or PNG)
- Use SaveAsImage method for file export
- Understand platform-specific save locations
- Configure file writing permissions for Android and Windows Phone
- Set iOS photo album access permissions
- Export charts as streams using GetStreamAsync
- Integrate chart streams with PDF, Excel, or Word components
- Handle ImageFileFormat options

