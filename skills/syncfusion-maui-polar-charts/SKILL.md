---
name: syncfusion-maui-polar-charts
description: Implements Syncfusion .NET MAUI Polar Charts (SfPolarChart) for visualizing data in polar coordinates. Use when working with polar charts, radar charts, spider charts, web charts, or circular data visualization. Ideal for displaying data in terms of values and angles, creating line or area series in polar layouts, or comparing multiple data series radially.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion .NET MAUI Polar Charts

Guide users to implement Syncfusion .NET MAUI Polar Chart (SfPolarChart), which enables visualization of data in polar coordinates, representing values and angles in circular layouts. Also known as radar, spider, web, star, or cobweb charts, these visualizations are ideal for comparing multiple data series radially and displaying cyclic or directional data patterns.

## When to Use This Skill

Use this skill when the user needs to:

- **Create polar or radar charts** - Visualize data in circular/angular layouts
- **Compare multiple data series radially** - Display 2-3+ series simultaneously in polar format
- **Display cyclic or directional data** - Compass directions, seasonal patterns, performance metrics across categories
- **Switch between polar and radar styles** - Circle grid lines (polar) vs polygon grid lines (radar/spider)
- **Implement line or area series** - PolarLineSeries or PolarAreaSeries
- **Customize polar chart appearance** - Axes, markers, data labels, legends, tooltips, colors
- **Configure polar angles** - Adjust start angle for different orientations (0°, 90°, 180°, 270°)
- **Export polar charts** - Save charts as images for reports or documentation

## Component Overview

**Key Features:**
- **Series Types**: Polar line and polar area series with multiple series support
- **Grid Line Types**: Circle (polar chart) or Polygon (radar/spider chart)
- **Axis Types**: Category, Numeric, and DateTime axes
- **Customization**: Data labels, markers, legends, tooltips, colors, liquid glass effects
- **Rendering Position**: Adjustable polar angles for different orientations
- **Export**: Save charts to image formats (PNG, JPEG, BMP)
- **Interactive**: Closed/open series, marker customization, responsive design

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

When the user needs to:
- Install and set up Syncfusion .NET MAUI Charts package
- Create their first polar chart
- Understand basic chart structure (SfPolarChart, PrimaryAxis, SecondaryAxis)
- Initialize view model and data binding
- Add series to the chart
- See complete working example

### Series Types
📄 **Read:** [references/series-types.md](references/series-types.md)

When the user needs to:
- Choose between polar line and polar area series
- Understand differences between PolarLineSeries and PolarAreaSeries
- Switch grid line types (Circle vs Polygon) for polar vs radar appearance
- Configure closed vs open series (IsClosed property)
- Display multiple series simultaneously
- Compare series types and choose the right one

### Axis Configuration
📄 **Read:** [references/axis-configuration.md](references/axis-configuration.md)

When the user needs to:
- Configure PrimaryAxis or SecondaryAxis
- Use Category, Numeric, or DateTime axis types
- Customize axis labels, formatting, rotation
- Style axis lines and tick lines
- Configure grid lines (major and minor)
- Set axis title and positioning
- Adjust axis ranges and intervals

### Data Labels and Markers
📄 **Read:** [references/data-labels-markers.md](references/data-labels-markers.md)

When the user needs to:
- Enable and customize data labels on series points
- Position and format data labels
- Create custom data label templates
- Enable markers (ShowMarkers property)
- Customize marker shapes, sizes, colors
- Configure MarkerSettings properties

### Legend and Tooltip
📄 **Read:** [references/legend-tooltip.md](references/legend-tooltip.md)

When the user needs to:
- Add and position legend
- Customize legend items and appearance
- Create custom legend templates
- Enable interactive tooltips
- Customize tooltip content and appearance
- Create custom tooltip templates

### Appearance Customization
📄 **Read:** [references/appearance-customization.md](references/appearance-customization.md)

When the user needs to:
- Customize chart colors and palettes
- Apply series-specific styling
- Configure background and borders
- Enable liquid glass effect for modern appearance
- Integrate with app themes
- Apply custom CSS or styling patterns

### Rendering Position
📄 **Read:** [references/rendering-position.md](references/rendering-position.md)

When the user needs to:
- Adjust polar chart start angle
- Rotate chart orientation (0°, 90°, 180°, 270°)
- Understand StartAngle property usage
- Position data for specific compass orientations
- Create charts with different angular alignments

### Exporting
📄 **Read:** [references/exporting.md](references/exporting.md)

When the user needs to:
- Export charts to image formats
- Save charts as PNG, JPEG, or BMP
- Configure export settings
- Implement export functionality
- Generate chart images for reports

