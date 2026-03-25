---
name: syncfusion-maui-treemap
description: Implements Syncfusion .NET MAUI TreeMap (SfTreeMap) for visualizing hierarchical data with rectangles sized and colored by values. Use when implementing hierarchical data visualization, heat maps, squarified layouts, multi-level data grouping, or brush settings for hierarchical displays. This skill covers installation, data binding, layout types, hierarchical levels with GroupPath, legend configuration, tooltips, and TreeMap customization.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion .NET MAUI TreeMap

The Syncfusion .NET MAUI TreeMap (SfTreeMap) is a powerful data visualization control that represents hierarchical data using nested rectangles. Each rectangle's size and color are determined by underlying data values, making it ideal for visualizing large datasets with hierarchical structure, such as organizational charts, file systems, population data, sales distributions, and more.

## When to Use This Skill

Use this skill when you need to:

- **Visualize hierarchical data** with nested rectangles where size represents data magnitude
- **Display multi-level categorized data** (e.g., sales by region → country → product)
- **Create heat maps** showing data distribution across categories with color-coded values
- **Implement squarified or slice-and-dice layouts** for optimal space utilization
- **Show data with legends and tooltips** for interactive exploration
- **Enable selection and interaction** with hierarchical data items
- **Apply various color schemes** (uniform, range-based, desaturation, or palette) to represent data values
- **Customize appearance** of leaf items, group headers, borders, labels, and more

Common scenarios include population visualization, sales analytics, file storage analysis, organizational hierarchies, budget breakdowns, and any domain requiring proportional area representation of hierarchical data.

## Component Overview

**Key Features:**

- **Data Binding**: Bind to hierarchical collections with automatic visualization
- **Layout Types**: Squarified (default), SliceAndDiceAuto, SliceAndDiceHorizontal, SliceAndDiceVertical
- **Hierarchical Levels**: Multi-level support using GroupPath for nested data structures
- **Brush Settings**: Four types - Uniform, Range, Desaturation, and Palette for flexible coloring
- **Legend**: Customizable legend with icon types, placement, and text styling
- **Tooltip**: Interactive tooltips displaying data details on hover/tap
- **Selection**: Single or multiple selection modes with customizable highlighting
- **Accessibility**: Built-in keyboard navigation, screen reader support, and WCAG compliance
- **Customization**: Extensive styling options for leaf items, headers, borders, labels, and spacing

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- NuGet package installation (Syncfusion.Maui.TreeMap)
- Handler registration (ConfigureSyncfusionCore in MauiProgram.cs)
- Basic TreeMap implementation in XAML and C#
- Data model and view model setup
- DataSource binding and PrimaryValuePath configuration
- Adding labels with LabelPath
- Complete minimal working example

**When to read**: Start here for initial setup and first TreeMap implementation.

### Layout Configuration
📄 **Read:** [references/layouts.md](references/layouts.md)
- Squarified layout (optimal aspect ratio, default)
- SliceAndDiceAuto layout (adaptive direction)
- SliceAndDiceHorizontal layout (horizontal slicing)
- SliceAndDiceVertical layout (vertical slicing)
- LayoutType property usage
- Choosing the right layout for your data
- Visual comparison of layout types

**When to read**: When you need to change the TreeMap layout algorithm or understand which layout works best for your data structure.

### Hierarchical Levels
📄 **Read:** [references/hierarchical-levels.md](references/hierarchical-levels.md)
- Understanding levels in TreeMap
- GroupPath property for multi-level categorization
- Adding multiple levels (n-level support)
- Level appearance customization (Spacing, HeaderHeight, Background)
- Stroke and StrokeWidth for level borders
- TextStyle for header text
- Group item brush settings (PaletteBrushSettings)

**When to read**: When implementing multi-level hierarchical data (e.g., Continent → Country → City) or customizing group header appearance.

### Color and Brush Settings
📄 **Read:** [references/brush-settings.md](references/brush-settings.md)
- UniformBrushSettings (single color for all items)
- RangeBrushSettings (color by value ranges with From/To)
- DesaturationBrushSettings (gradient by saturation levels)
- PaletteBrushSettings (multiple colors from a palette)
- RangeColorValuePath configuration
- LegendLabel for range definitions
- When to use each brush type

**When to read**: When you need to apply color schemes to visualize data values, ranges, or categories.

### Leaf Item Customization
📄 **Read:** [references/leaf-item-customization.md](references/leaf-item-customization.md)
- LabelPath for displaying text on leaf items
- Spacing between leaf items
- Stroke and StrokeWidth for leaf borders
- TextStyle configuration (color, font, size, attributes)
- Label overflow modes (wrap, trim)
- Label template customization
- ShowLabels property

**When to read**: When customizing the appearance and labeling of individual data items (leaf nodes).

### Legend Configuration
📄 **Read:** [references/legend.md](references/legend.md)
- Enabling legend with ShowLegend property
- Legend placement (Top, Bottom, Left, Right)
- LegendSettings configuration
- Icon types (Circle, Rectangle, Diamond, Triangle, etc.)
- Icon size customization
- Text style for legend items
- Legend item template
- Integration with RangeBrushSettings

**When to read**: When adding a legend to explain color coding or value ranges in the TreeMap.

### Tooltip Implementation
📄 **Read:** [references/tooltip.md](references/tooltip.md)
- Enabling tooltip with ShowToolTip property
- Default tooltip behavior
- Tooltip template customization
- TooltipTemplate property usage
- Data context in tooltips
- Styling tooltip appearance
- Tooltips for leaf items vs group items

**When to read**: When adding interactive tooltips to display additional data on hover or tap.

### Selection and Interaction
📄 **Read:** [references/selection-interaction.md](references/selection-interaction.md)
- SelectionMode property (None, Single, Multiple)
- Single vs multiple selection behavior
- SelectedItems collection
- SelectionChanged event handling
- Highlight settings (HighlightOnSelection)
- Selection stroke and fill customization
- Programmatic selection

**When to read**: When implementing user interaction features like selecting items, highlighting, or responding to selection events.

### Accessibility and Events
📄 **Read:** [references/accessibility-events.md](references/accessibility-events.md)
- Accessibility support overview
- Keyboard navigation support
- WCAG compliance features
- ItemSelected and ItemDeselected events
- ItemTapped event
- Drilldown functionality
- Right-to-left (RTL) support
- Semantic properties for accessibility

**When to read**: When implementing accessible TreeMaps, handling events, or supporting RTL languages.

