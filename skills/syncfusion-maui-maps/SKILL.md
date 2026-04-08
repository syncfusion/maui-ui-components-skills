---
name: syncfusion-maui-maps
description: Implements Syncfusion .NET MAUI Maps (SfMaps) control. Use when implementing maps, visualizing geographic data, or displaying shape layers and tile layers (OpenStreetMap, Azure maps, Google Maps, Bing Maps). Covers markers, bubbles, legends, data labels, tooltips, sublayers, zoom and pan features, or spatial data visualization in .NET MAUI applications.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing .NET MAUI Maps (SfMaps)

A comprehensive skill for implementing Syncfusion's .NET MAUI Maps control. The SfMaps control is a powerful data visualization component for displaying statistical information across geographical areas with highly interactive and customizable features.

## When to Use This Skill

Use this skill when you need to:

- **Install and configure** Syncfusion .NET MAUI Maps (SfMaps) control
- **Display geographic data** using shape layers with shapefile data
- **Integrate tile-based maps** from providers.
- **Add markers** to denote locations with built-in symbols or custom content
- **Visualize data** with bubbles, color mapping, and choropleth maps
- **Display legends** for shapes or bubbles with customizable appearance
- **Show data labels** on map shapes with smart positioning
- **Add sublayers** (circle, arc, line, polygon, polyline) for additional data visualization
- **Enable interactions** like zoom, pan, shape selection, and tooltips
- **Implement AI-driven location search** for smart geocoding and search

## Component Overview

**Syncfusion .NET MAUI Maps** provides two primary layer types:

1. **Shape Layer (`MapShapeLayer`)**: Renders vector maps from GeoJSON or shapefile data
2. **Tile Layer (`MapTileLayer`)**: Renders raster map tiles from web map tile services

**Key Features:**
- Multiple layer support with sublayers
- Markers with built-in and custom templates
- Bubble visualization for data representation
- Color mapping (equal and range-based)
- Interactive legends
- Data labels with smart positioning
- Zoom and pan capabilities
- Shape selection and tooltips
- AI-driven location search

**Platforms Supported:** Android, iOS, Windows, macOS

## Documentation and Navigation Guide

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)

When to read: First-time setup, installation, basic map creation

**Topics covered:**
- NuGet package installation (`Syncfusion.Maui.Maps`)
- Handler registration in `MauiProgram.cs`
- Creating your first map with shape layer
- Loading shapefile data from local.
- Creating your first map with tile layer
- Basic configuration and map display

### Layer Types

#### Shape Layer

📄 **Read:** [references/shape-layer.md](references/shape-layer.md)

When to read: Working with vector maps, shapefile data, choropleth maps, data binding

**Topics covered:**
- `MapShapeLayer` overview and configuration
- Loading shape data 
- Data source binding with `DataSource`, `PrimaryValuePath`, and `ShapeDataField`
- Color mapping strategies (EqualColorMapping, RangeColorMapping)
- Shape styling (fill, stroke, hover states)
- Shape customization and theming
- AOT mode considerations and preservation attributes

#### Tile Layer

📄 **Read:** [references/tile-layer.md](references/tile-layer.md)

When to read: Integrating OpenStreetMap, Bing Maps, or other tile providers

**Topics covered:**
- `MapTileLayer` overview
- URL template format and WMTS specification
- Subscription key configuration
- Map center and zoom settings
- Map type variations (Road, Aerial, etc.)

### Visual Elements

#### Markers

📄 **Read:** [references/markers.md](references/markers.md)

When to read: Adding location markers, pins, or custom markers to maps

**Topics covered:**
- `MapMarker` overview and `MapMarkerCollection`
- Adding markers to shape layers
- Adding markers to tile layers
- Built-in icon types (Circle, Diamond, Rectangle, Square)
- Marker positioning with latitude/longitude
- Icon customization (size, color, fill, stroke)
- Custom marker templates with `MarkerTemplate`
- Marker alignment, offset, and anchoring
- Marker tooltips
- Marker events and selection

#### Bubbles

📄 **Read:** [references/bubbles.md](references/bubbles.md)

When to read: Visualizing data with size/color-coded bubbles on shapes

**Topics covered:**
- Bubble visualization overview
- Enabling bubbles with `ShowBubbles`
- `BubbleSettings` configuration
- Size-based bubbles with `SizeValuePath`, `MinSize`, `MaxSize`
- Color-based bubbles with `ColorValuePath`
- Bubble styling (fill, stroke, opacity)
- Bubble tooltips
- Bubble color mapping
- Multiple bubble series

#### Data Labels

📄 **Read:** [references/data-labels.md](references/data-labels.md)

When to read: Displaying text labels on map shapes

**Topics covered:**
- Data label overview and purpose
- Enabling labels with `ShowDataLabels`
- `DataLabelSettings` configuration
- `DataLabelPath` for binding label text
- Label styling (font, color, size, weight)
- Label positioning and alignment
- Smart label behavior (trim, hide on overflow)
- Custom label templates
- Label interaction and events

#### Legends

📄 **Read:** [references/legends.md](references/legends.md)

When to read: Adding legends for shapes or bubbles

**Topics covered:**
- Legend overview and `MapLegend` configuration
- Shape legend with `SourceType.Shape`
- Bubble legend with `SourceType.Bubble`
- Legend placement (Top, Bottom, Left, Right)
- Legend text customization from `ColorMapping.Text`
- Icon customization (size, type, spacing)
- Legend positioning and padding
- Text style configuration with `TextStyle`
- Custom legend templates
- Legend visibility and toggling

### Advanced Features

#### Sublayers

📄 **Read:** [references/sublayers.md](references/sublayers.md)

When to read: Adding multiple layers, circles, arcs, lines, polygons, or polylines

**Topics covered:**
- Sublayer overview and types
- `MapShapeSublayer` for additional shape layers
- `MapCircle` for radius-based circles
- `MapArc` for connecting two points with arcs
- `MapLine` for simple line connections
- `MapPolygon` for custom polygon shapes
- `MapPolyline` for multi-point line paths
- Sublayer data binding and collections
- Sublayer styling and appearance
- Multiple sublayers and stacking
- Layer z-index and ordering

#### Interaction Features

📄 **Read:** [references/interaction-features.md](references/interaction-features.md)

When to read: Enabling zoom, pan, selection, or tooltips

**Topics covered:**
- **Zoom and Pan:**
  - `EnableZooming` and `EnablePanning` properties
  - `ZoomPanBehavior` configuration
  - `MinZoomLevel` and `MaxZoomLevel`
  - Programmatic zoom methods
  - Zooming to specific regions
- **Selection:**
  - Shape selection with `EnableSelection`
  - Selection appearance customization
  - Selection events and handlers
- **Tooltips:**
  - `ShowShapeTooltip` for shape tooltips
  - Tooltip templates and styling
  - Tooltips for markers and bubbles
  - Custom tooltip content

#### AI-Driven Location Search

📄 **Read:** [references/ai-location-search.md](references/ai-location-search.md)

When to read: Implementing smart location search and geocoding

**Topics covered:**
- AI-driven location search overview
- Smart search capabilities
- Location autocomplete
- Geocoding integration
- Search configuration
- Search result handling
- Custom search providers

