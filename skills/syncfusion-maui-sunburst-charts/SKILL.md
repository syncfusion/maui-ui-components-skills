---
name: syncfusion-maui-sunburst-charts
description: Implements Syncfusion .NET MAUI Sunburst Chart (SfSunburstChart) for hierarchical data visualization using a radial, multi-level circular layout. Use this for sunburst charts, radial hierarchical charts, multi-level pie charts, or hierarchical data visualization with drill-down capabilities. Ideal for visualizing organizational structures, file systems, or nested categorical data.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing .NET MAUI Sunburst Charts

Guide users to implement Syncfusion .NET MAUI Sunburst Chart (SfSunburstChart), a powerful hierarchical data visualization component that displays multi-level categorical data in a radial layout. Each ring represents a level in the hierarchy, with segments sized proportionally to their values. The component supports interactive drill-down, customizable appearance, data labels, tooltips, selection, and modern liquid glass effects.

## When to Use This Skill

Use the Syncfusion .NET MAUI Sunburst Chart when you need to:

- **Visualize hierarchical data** with 2-5 levels (organizational charts, file systems, product categories)
- **Show proportional relationships** where segment size represents value or count
- **Enable drill-down exploration** of large hierarchical datasets with interactive navigation
- **Display nested categorical data** like sales by region → department → product
- **Create radial visualizations** as an alternative to tree maps or nested pie charts
- **Build interactive dashboards** with multi-level data exploration capabilities

The sunburst chart is ideal when users need to understand both the hierarchy structure and the relative proportions at each level.

## Component Overview

The SfSunburstChart presents hierarchical data in concentric rings, where:
- **Center** represents the root or can display custom content
- **Inner rings** represent higher-level categories
- **Outer rings** represent detailed subcategories
- **Segment size** is determined by the value property
- **Drill-down** allows focusing on specific branches

**Key capabilities:**
- Hierarchical level configuration with GroupMemberPath
- Customizable angles, radius, and inner radius
- Data labels with rotation and overflow modes
- Interactive tooltips with custom templates
- Selection highlighting with multiple display modes
- Drill-down with animated toolbar navigation
- Center view for displaying summary information
- Liquid glass effect for modern iOS/macOS styling

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- NuGet package installation and setup
- Basic sunburst chart implementation
- Creating hierarchical data models
- Configuring ItemsSource and ValueMemberPath
- Adding hierarchical levels with GroupMemberPath
- Enabling title, legend, tooltips, and labels
- Complete working example

### Visual Customization

#### Appearance Configuration
📄 **Read:** [references/appearance.md](references/appearance.md)
- Adjusting start and end angles for partial circles
- Customizing radius and inner radius
- Configuring stroke color and width
- Creating donut-style visualizations

#### Data Labels
📄 **Read:** [references/data-labels.md](references/data-labels.md)
- Enabling and positioning data labels
- Overflow modes (Trim, Hide)
- Rotation modes (Angle, Normal)
- Font customization (family, size, color, attributes)

### Interactivity Features

#### Tooltips
📄 **Read:** [references/tooltips.md](references/tooltips.md)
- Enabling tooltips for segment details
- Customizing tooltip appearance
- Creating custom tooltip templates
- Configuring display duration and styling

#### Selection
📄 **Read:** [references/selection.md](references/selection.md)
- Selection types (Single, Child, Group, Parent)
- Display modes (Brush, Opacity, Stroke)
- Selection events (SelectionChanging, SelectionChanged)
- Highlighting selected segments and related hierarchies

#### Drill-Down
📄 **Read:** [references/drill-down.md](references/drill-down.md)
- Enabling interactive drill-down navigation
- Toolbar alignment and positioning
- Customizing drill-down toolbar appearance
- Zoom-back and reset operations

### Advanced Features

#### Center View
📄 **Read:** [references/center-view.md](references/center-view.md)
- Adding custom views to chart center
- Using CenterHoleSize for proper sizing
- Creating summary displays in center
- Complex layouts with borders and labels

#### Liquid Glass Effect
📄 **Read:** [references/liquid-glass-effect.md](references/liquid-glass-effect.md)
- Modern glass design styling (.NET 10+)
- Wrapping charts in SfGlassEffectView
- Enabling glass effect on tooltips
- iOS/macOS platform-specific features

