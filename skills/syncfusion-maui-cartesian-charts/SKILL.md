---
name: syncfusion-maui-cartesian-charts
description: Implements Syncfusion .NET MAUI Cartesian Chart (SfCartesianChart) for data visualization with 20+ chart types including line, area, column, bar, financial, statistical, and stacked charts. Use when implementing cartesian charts, line charts, area charts, or financial charts (candle, OHLC). Covers chart axes, legends, tooltips, trackball, zooming and panning, chart annotations, and trendlines.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing .NET MAUI Cartesian Charts

Guide users to implement the Syncfusion .NET MAUI Cartesian Chart (SfCartesianChart), a comprehensive data visualization solution with over 20 chart types, interactive features, and extensive customization options. Use this skill to implement charts ranging from simple line graphs to complex financial and statistical visualizations.

## When to Use This Skill

Use this skill when the user needs to:
- **Visualize Data:** Display quantitative data with line, area, column, bar, or scatter charts
- **Financial Analysis:** Create candle or OHLC charts for stock price visualization
- **Statistical Data:** Plot box-and-whisker, histogram, or error bar charts
- **Comparative Analysis:** Use stacked charts (area, column, line) for comparing multiple datasets
- **Range Visualization:** Show high-low data with range area or range column charts
- **Interactive Dashboards:** Implement zooming, panning, selection, and trackball features
- **Time-Series Data:** Plot temporal data with datetime axes and auto-scrolling
- **Custom Visualizations:** Add annotations, plotbands, trendlines, and custom styling

## Component Overview

The **SfCartesianChart** control provides:
- **20+ Chart Types:** Line, Area, Column, Bar, Scatter, Bubble, Candle, OHLC, Histogram, Box-and-Whisker, Stacked variations, Range charts, Waterfall, and more
- **Flexible Axes:** Category, Numerical, DateTime, Logarithmic with full customization
- **Interactive Features:** Zooming, panning, selection, tooltips, trackball, crosshair
- **Rich Customization:** Data labels, legends, annotations, plotbands, trendlines, appearance
- **Data Handling:** Empty point modes, data binding, dynamic updates
- **Export & Localization:** Image export, multi-language support

## Documentation and Navigation Guide

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)
- NuGet package installation (`Syncfusion.Maui.Charts`)
- Basic SfCartesianChart initialization in XAML and C#
- Creating data models and view models
- Axis setup (XAxes, YAxes)
- Adding your first series with data binding
- Complete working example

### Chart Types

#### Basic Chart Types
📄 **Read:** [references/basic-chart-types.md](references/basic-chart-types.md)
- Line, Area, Column, Bar, Scatter, Bubble charts
- When to use each chart type
- Implementation examples
- Common properties and styling

#### Financial Charts
📄 **Read:** [references/financial-charts.md](references/financial-charts.md)
- Candle charts (candlestick visualization)
- OHLC (Open-High-Low-Close) charts
- Financial data structure and binding
- Bullish/bearish styling

#### Statistical Charts
📄 **Read:** [references/statistical-charts.md](references/statistical-charts.md)
- Box and Whisker charts
- Histogram charts
- Error Bar charts
- Statistical data requirements

#### Stacked Charts
📄 **Read:** [references/stacked-charts.md](references/stacked-charts.md)
- Stacked Area (normal and 100%)
- Stacked Column (normal and 100%)
- Stacked Line (normal and 100%)
- Grouping series for stacking
- Percentage vs absolute values

#### Range Charts
📄 **Read:** [references/range-charts.md](references/range-charts.md)
- Range Area charts
- Range Column charts
- Spline Range Area charts
- High-Low data binding

#### Advanced Chart Types
📄 **Read:** [references/advanced-chart-types.md](references/advanced-chart-types.md)
- Fastline (optimized for large datasets)
- Step Area and Step Line
- Waterfall charts
- Performance considerations

### Axis Configuration

#### Axis Basics
📄 **Read:** [references/axis-configuration.md](references/axis-configuration.md)
- Axis types (Category, Numerical, DateTime, Logarithmic)
- XAxes and YAxes collections
- Axis labels and formatting
- Axis titles
- Multiple axes setup
- Custom label customization

#### Axis Customization
📄 **Read:** [references/axis-customization.md](references/axis-customization.md)
- Gridlines (major, minor) styling
- Tick lines configuration
- Axis padding
- Range padding
- Auto-scrolling delta
- Axis positioning

### Data Visualization & Interaction

#### Data Labels
📄 **Read:** [references/data-labels.md](references/data-labels.md)
- Enabling and positioning data labels
- Label placement (Inner, Outer, Auto)
- Label formatting and templates
- Styling and appearance
- Release mode display configuration

#### Tooltips and Interactive Features
📄 **Read:** [references/tooltips-and-interaction.md](references/tooltips-and-interaction.md)
- Tooltip configuration and customization
- Trackball feature for multi-series comparison
- Crosshair for precise value reading
- Tooltip templates
- Interactive display modes

#### Zooming and Panning
📄 **Read:** [references/zooming-and-panning.md](references/zooming-and-panning.md)
- Zoom modes (Pinch, Selection, MouseWheel)
- Panning configuration
- ZoomPanBehavior setup
- Programmatic zoom control
- Reset zoom functionality

#### Selection
📄 **Read:** [references/selection.md](references/selection.md)
- Selection types (Single, Multiple, Series)
- DataPointSelectionBehavior
- SeriesSelectionBehavior
- Selection events
- Custom selection styling

### Customization & Styling

#### Legend and Appearance
📄 **Read:** [references/legend-and-appearance.md](references/legend-and-appearance.md)
- Legend configuration and positioning
- Legend item templates
- Chart appearance customization
- Color palettes
- Themes and liquid glass effect

#### Annotations and PlotBands
📄 **Read:** [references/annotations-and-plotbands.md](references/annotations-and-plotbands.md)
- Annotation types (Text, View, Shape)
- Adding and positioning annotations
- PlotBand configuration for highlighting regions
- Use cases and examples

#### Custom Styling
📄 **Read:** [references/customization-and-styling.md](references/customization-and-styling.md)
- Series appearance customization
- Adding custom labels
- Color and gradient fills
- Border and corner radius
- Template customization

### Data Handling & Advanced Features

#### Trendlines
📄 **Read:** [references/trendlines.md](references/trendlines.md)
- Trendline types (Linear, Exponential, Logarithmic, Polynomial, Power, Moving Average)
- Adding trendlines to series
- Forecast and backcast
- Trendline customization

#### Data Point Management
📄 **Read:** [references/data-handling.md](references/data-handling.md)
- Empty points handling
- Empty point modes (Gap, Zero, Average, Drop)
- Getting data points by region
- Touch position detection
- Data manipulation

#### Export and Localization
📄 **Read:** [references/exporting-and-localization.md](references/exporting-and-localization.md)
- Exporting charts as images
- Export formats and options
- Localization configuration
- Culture-specific formatting
- Resource files

