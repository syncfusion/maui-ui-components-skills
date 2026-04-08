# Markers in .NET MAUI Maps (SfMaps)

## Table of Contents
- [Overview](#overview)
- [Key APIs Used](#key-apis-used)
- [Adding Markers to Shape Layer](#adding-markers-to-shape-layer)
- [Adding Markers to Tile Layer](#adding-markers-to-tile-layer)
- [Built-in Icon Types](#built-in-icon-types)
- [Appearance Customization](#appearance-customization)
- [Marker Positioning and Alignment](#marker-positioning-and-alignment)
- [Custom Marker Templates](#custom-marker-templates)
- [Marker Tooltip Customization](#marker-tooltip-customization)
- [Template Selectors](#template-selectors)
- [Best Practices](#best-practices)

## Overview

Markers can be used to denote locations on the map. It is possible to use the built-in symbols or display custom content at a specific latitude and longitude on a map using Syncfusion .NET MAUI Maps control.

Markers can be displayed on:
- `MapShapeLayer` - For shape-based maps with GeoJSON data
- `MapTileLayer` - For tile-based maps with online map tiles

Use markers for:
- Location pins (stores, landmarks, addresses)
- Points of interest visualization
- User locations tracking
- Event locations marking
- Custom geographic data points

## Key APIs Used

### MapMarker Class

The `MapMarker` class represents a marker on the map with geographic coordinates and visual properties.

| API | Type | Description |
|-----|------|-------------|
| `Latitude` | Property (double) | Gets or sets the latitude coordinate of the marker |
| `Longitude` | Property (double) | Gets or sets the longitude coordinate of the marker |
| `IconType` | Property (MapIconType) | Gets or sets the built-in icon shape (Circle, Diamond, Rectangle, Square) |
| `IconFill` | Property (Brush) | Gets or sets the fill color of the marker icon |
| `IconStroke` | Property (Brush) | Gets or sets the border color of the marker icon |
| `IconStrokeThickness` | Property (double) | Gets or sets the border thickness of the marker icon (default: 1.0) |
| `IconWidth` | Property (double) | Gets or sets the width of the marker icon (default: 8.0) |
| `IconHeight` | Property (double) | Gets or sets the height of the marker icon (default: 8.0) |
| `HorizontalAlignment` | Property (MapAlignment) | Gets or sets the horizontal alignment relative to coordinates |
| `VerticalAlignment` | Property (MapAlignment) | Gets or sets the vertical alignment relative to coordinates |
| `Offset` | Property (Point) | Gets or sets the pixel offset from the aligned position |

### MapMarkerCollection Class

Collection class for managing multiple `MapMarker` objects on a map layer.

| API | Type | Description |
|-----|------|-------------|
| `Add(MapMarker)` | Method | Adds a marker to the collection |
| `Remove(MapMarker)` | Method | Removes a marker from the collection |
| `Clear()` | Method | Removes all markers from the collection |

### MapLayer Properties

Properties available on both `MapShapeLayer` and `MapTileLayer` for marker management.

| API | Type | Description |
|-----|------|-------------|
| `Markers` | Property (MapMarkerCollection) | Gets or sets the collection of markers displayed on the layer |
| `MarkerTemplate` | Property (DataTemplate) | Gets or sets the template for customizing marker appearance |
| `ShowMarkerTooltip` | Property (bool) | Gets or sets whether to display tooltips on marker interaction |
| `MarkerTooltipTemplate` | Property (DataTemplate) | Gets or sets the template for customizing marker tooltip content |

### MapIconType Enumeration

Built-in icon shapes for markers.

| Value | Description |
|-------|-------------|
| `Circle` | Circular marker icon (default) |
| `Diamond` | Diamond-shaped marker icon |
| `Rectangle` | Rectangular marker icon |
| `Square` | Square marker icon |
| `Triangle` | Triangular marker icon |

### MapAlignment Enumeration

Alignment options for marker positioning.

| Value | Description |
|-------|-------------|
| `Start` | Align to the start (left/top) of the coordinate |
| `Center` | Align to the center of the coordinate (default) |
| `End` | Align to the end (right/bottom) of the coordinate |

## Adding Markers to Shape Layer

You can show markers at any position on the map by providing latitude and longitude position to the `MapMarker`, which is added to the `Markers` collection.

### XAML Implementation

```xaml
<map:SfMaps>
    <map:SfMaps.Layer>
        <map:MapShapeLayer ShapesSource="url"
                   ShapeStroke="DarkGrey">
            <map:MapShapeLayer.Markers>
                <map:MapMarkerCollection>
                    <map:MapMarker Latitude="-14.235004"
                                   Longitude="-51.92528"
                                   IconType="Circle" 
                                   IconFill="#00ccff"
                                   IconWidth="15"
                                   IconHeight="15" />
                    <map:MapMarker Latitude="51.16569"
                                   Longitude="10.451526"
                                   IconType="Circle" 
                                   IconFill="#00ccff"
                                   IconWidth="15"
                                   IconHeight="15" />
                    <map:MapMarker Latitude="-25.274398"
                                   Longitude="133.775136"
                                   IconType="Circle" 
                                   IconFill="#00ccff"
                                   IconWidth="15"
                                   IconHeight="15" />
                    <map:MapMarker Latitude="20.593684"
                                   Longitude="78.96288"
                                   IconType="Circle" 
                                   IconFill="#00ccff"
                                   IconWidth="15"
                                   IconHeight="15" />
                    <map:MapMarker Latitude="61.52401"
                                   Longitude="105.318756"
                                   IconType="Circle"
                                   IconFill="#00ccff"
                                   IconWidth="15"
                                   IconHeight="15" />
                </map:MapMarkerCollection>
            </map:MapShapeLayer.Markers>
        </map:MapShapeLayer>
    </map:SfMaps.Layer>
</map:SfMaps>
```

### C# Implementation

```csharp
using Syncfusion.Maui.Maps;
using Microsoft.Maui.Controls;
using Microsoft.Maui.Graphics;

namespace MauiMapsApp;

public partial class MapMarkerPage : ContentPage
{
    public MapMarkerPage()
    {
        InitializeComponent();
        
        MapShapeLayer layer = new MapShapeLayer();
        layer.ShapesSource = MapSource.FromUri("url"); // For e.g "https://cdn.syncfusion.com/maps/map-data/world-map.json"
        layer.ShapeStroke = Colors.DarkGrey;

        MapMarker mapMarker1 = new MapMarker
        {
            Latitude = -14.235004,
            Longitude = -51.92528,
            IconHeight = 15,
            IconWidth = 15,
            IconType = MapIconType.Circle,
            IconFill = Color.FromRgb(0, 204, 255)
        };

        MapMarker mapMarker2 = new MapMarker
        {
            Latitude = 51.16569,
            Longitude = 10.451526,
            IconHeight = 15,
            IconWidth = 15,
            IconType = MapIconType.Circle,
            IconFill = Color.FromRgb(0, 204, 255)
        };

        MapMarker mapMarker3 = new MapMarker
        {
            Latitude = -25.274398,
            Longitude = 133.775136,
            IconHeight = 15,
            IconWidth = 15,
            IconType = MapIconType.Circle,
            IconFill = Color.FromRgb(0, 204, 255)
        };

        MapMarker mapMarker4 = new MapMarker
        {
            Latitude = 20.593684,
            Longitude = 78.96288,
            IconHeight = 15,
            IconWidth = 15,
            IconType = MapIconType.Circle,
            IconFill = Color.FromRgb(0, 204, 255)
        };

        MapMarker mapMarker5 = new MapMarker
        {
            Latitude = 61.52401,
            Longitude = 105.318756,
            IconHeight = 15,
            IconWidth = 15,
            IconType = MapIconType.Circle,
            IconFill = Color.FromRgb(0, 204, 255)
        };

        MapMarkerCollection mapMarkers = new MapMarkerCollection();
        mapMarkers.Add(mapMarker1);
        mapMarkers.Add(mapMarker2);
        mapMarkers.Add(mapMarker3);
        mapMarkers.Add(mapMarker4);
        mapMarkers.Add(mapMarker5);
        
        layer.Markers = mapMarkers;

        SfMaps map = new SfMaps();
        map.Layer = layer;
        
        this.Content = map;
    }
}
```

### Key APIs Used

- `MapShapeLayer` - The layer that displays geographic shapes from GeoJSON data
- `MapMarker` - Represents a marker with latitude/longitude coordinates
- `MapMarkerCollection` - Collection for managing multiple markers
- `MapIconType.Circle` - Built-in circular icon type for markers
- `IconFill` - Property to set the marker fill color
- `IconWidth` / `IconHeight` - Properties to set marker dimensions

## Adding Markers to Tile Layer

You can show markers at any position on the tile layer by providing latitude and longitude position to the `MapMarker`, which is added to the `Markers` collection.

### XAML Implementation

```xaml
<map:SfMaps>
    <map:SfMaps.Layer>
        <map:MapTileLayer UrlTemplate="url">
            <map:MapTileLayer.Markers>
                <map:MapMarkerCollection>
                    <map:MapMarker Latitude="-14.235004"
                                   Longitude="-51.92528"
                                   IconType="Circle"
                                   IconFill="#2f98f3"
                                   IconWidth="15"
                                   IconHeight="15" />
                    <map:MapMarker Latitude="51.16569"
                                   Longitude="10.451526"
                                   IconType="Circle"
                                   IconFill="#2f98f3"
                                   IconWidth="15"
                                   IconHeight="15" />
                    <map:MapMarker Latitude="-25.274398"
                                   Longitude="133.775136"
                                   IconType="Circle"
                                   IconFill="#2f98f3"
                                   IconWidth="15"
                                   IconHeight="15" />
                    <map:MapMarker Latitude="20.593684"
                                   Longitude="78.96288"
                                   IconType="Circle"
                                   IconFill="#2f98f3"
                                   IconWidth="15"
                                   IconHeight="15" />
                    <map:MapMarker Latitude="61.52401"
                                   Longitude="105.318756"
                                   IconType="Circle"
                                   IconFill="#2f98f3"
                                   IconWidth="15"
                                   IconHeight="15" />
                </map:MapMarkerCollection>
            </map:MapTileLayer.Markers>
        </map:MapTileLayer>
    </map:SfMaps.Layer>
</map:SfMaps>
```

### C# Implementation

```csharp
using Syncfusion.Maui.Maps;
using Microsoft.Maui.Controls;
using Microsoft.Maui.Graphics;

namespace MauiMapsApp;

public partial class TileMapMarkerPage : ContentPage
{
    public TileMapMarkerPage()
    {
        InitializeComponent();
        
        MapTileLayer tileLayer = new MapTileLayer();
        tileLayer.UrlTemplate = "url"; // For e.g  "https://tile.openstreetmap.org/{z}/{x}/{y}.png"

        MapMarker mapMarker1 = new MapMarker
        {
            Latitude = -14.235004,
            Longitude = -51.92528,
            IconHeight = 15,
            IconWidth = 15,
            IconType = MapIconType.Circle,
            IconFill = Color.FromRgb(47, 152, 243)
        };

        MapMarker mapMarker2 = new MapMarker
        {
            Latitude = 51.16569,
            Longitude = 10.451526,
            IconHeight = 15,
            IconWidth = 15,
            IconType = MapIconType.Circle,
            IconFill = Color.FromRgb(47, 152, 243)
        };

        MapMarker mapMarker3 = new MapMarker
        {
            Latitude = -25.274398,
            Longitude = 133.775136,
            IconHeight = 15,
            IconWidth = 15,
            IconType = MapIconType.Circle,
            IconFill = Color.FromRgb(47, 152, 243)
        };

        MapMarker mapMarker4 = new MapMarker
        {
            Latitude = 20.593684,
            Longitude = 78.96288,
            IconHeight = 15,
            IconWidth = 15,
            IconType = MapIconType.Circle,
            IconFill = Color.FromRgb(47, 152, 243)
        };

        MapMarker mapMarker5 = new MapMarker
        {
            Latitude = 61.52401,
            Longitude = 105.318756,
            IconHeight = 15,
            IconWidth = 15,
            IconType = MapIconType.Circle,
            IconFill = Color.FromRgb(47, 152, 243)
        };

        MapMarkerCollection mapMarkers = new MapMarkerCollection();
        mapMarkers.Add(mapMarker1);
        mapMarkers.Add(mapMarker2);
        mapMarkers.Add(mapMarker3);
        mapMarkers.Add(mapMarker4);
        mapMarkers.Add(mapMarker5);
        
        tileLayer.Markers = mapMarkers;

        SfMaps map = new SfMaps();
        map.Layer = tileLayer;
        
        this.Content = map;
    }
}
```

### Key APIs Used

- `MapTileLayer` - The layer that displays online map tiles
- `UrlTemplate` - Property to specify the tile URL pattern
- `MapMarker` - Represents a marker with latitude/longitude coordinates
- `MapMarkerCollection` - Collection for managing multiple markers
- `Markers` - Property on MapTileLayer to set the marker collection

> **Note:** Refer to `MapMarkerCollection` for the collection of `MapMarker` objects.

## Built-in Icon Types

The `IconType` property provides five built-in marker shapes through the `MapIconType` enumeration:

### Available Icon Types

| Icon Type | Description | Common Use Cases |
|-----------|-------------|------------------|
| `Circle` | Circular marker icon (default) | General-purpose markers, location pins, simple points |
| `Diamond` | Diamond-shaped marker icon | Special locations, highlighted points, differentiating markers |
| `Rectangle` | Rectangular marker icon | Buildings, rectangular areas, landmarks |
| `Square` | Square marker icon | Uniform markers, grid-based locations |
| `Triangle` | Triangular marker icon | Direction indicators, warning markers, special points |

### Example Usage

```xaml
<!-- Circle marker -->
<map:MapMarker Latitude="37.7749" 
               Longitude="-122.4194"
               IconType="Circle"
               IconFill="Red"
               IconWidth="20"
               IconHeight="20" />

<!-- Diamond marker -->
<map:MapMarker Latitude="40.7128" 
               Longitude="-74.0060"
               IconType="Diamond"
               IconFill="Blue"
               IconWidth="20"
               IconHeight="20" />

<!-- Rectangle marker -->
<map:MapMarker Latitude="51.5074" 
               Longitude="-0.1278"
               IconType="Rectangle"
               IconFill="Green"
               IconWidth="20"
               IconHeight="20" />

<!-- Square marker -->
<map:MapMarker Latitude="35.6762" 
               Longitude="139.6503"
               IconType="Square"
               IconFill="Orange"
               IconWidth="20"
               IconHeight="20" />

<!-- Triangle marker -->
<map:MapMarker Latitude="-33.8688" 
               Longitude="151.2093"
               IconType="Triangle"
               IconFill="Purple"
               IconWidth="20"
               IconHeight="20" />
```

```csharp
using Syncfusion.Maui.Maps;
using Microsoft.Maui.Graphics;

// Circle marker
var circleMarker = new MapMarker
{
    Latitude = 37.7749,
    Longitude = -122.4194,
    IconType = MapIconType.Circle,
    IconFill = Colors.Red,
    IconWidth = 20,
    IconHeight = 20
};

// Diamond marker
var diamondMarker = new MapMarker
{
    Latitude = 40.7128,
    Longitude = -74.0060,
    IconType = MapIconType.Diamond,
    IconFill = Colors.Blue,
    IconWidth = 20,
    IconHeight = 20
};

// Triangle marker
var triangleMarker = new MapMarker
{
    Latitude = 35.6762,
    Longitude = 139.6503,
    IconType = MapIconType.Triangle,
    IconFill = Colors.Purple,
    IconWidth = 20,
    IconHeight = 20
};
```

## Appearance Customization

You can customize the built-in markers appearance using the `IconType`, `IconFill`, `IconStroke`, `IconStrokeThickness`, `IconWidth`, and `IconHeight` properties of the `MapMarker`.

### Icon Customization Properties

| Property | Type | Default Value | Description |
|----------|------|---------------|-------------|
| `IconType` | MapIconType | Circle | Built-in icon shape |
| `IconFill` | Brush | Color.FromRgb(138, 69, 175) | Fill color of the icon |
| `IconStroke` | Brush | null | Border color of the icon |
| `IconStrokeThickness` | double | 1.0 | Border thickness of the icon |
| `IconWidth` | double | 8.0 | Width of the icon |
| `IconHeight` | double | 8.0 | Height of the icon |

### XAML Implementation

```xaml
<map:SfMaps>
    <map:SfMaps.Layer>
        <map:MapShapeLayer ShapesSource="url"
                           ShapeStroke="DarkGrey">
            <map:MapShapeLayer.Markers>
                <map:MapMarkerCollection>
                    <map:MapMarker Latitude="-14.235004"
                                   Longitude="-51.92528"
                                   IconType="Triangle"
                                   IconFill="LightGreen"
                                   IconStroke="Green"
                                   IconStrokeThickness="2"
                                   IconWidth="20"
                                   IconHeight="20" />
                    <map:MapMarker Latitude="51.16569"
                                   Longitude="10.451526"
                                   IconType="Triangle"
                                   IconFill="LightGreen"
                                   IconStroke="Green"
                                   IconStrokeThickness="2"
                                   IconWidth="20"
                                   IconHeight="20" />
                    <map:MapMarker Latitude="-25.274398"
                                   Longitude="133.775136"
                                   IconType="Triangle"
                                   IconFill="LightGreen"
                                   IconStroke="Green"
                                   IconStrokeThickness="2"
                                   IconWidth="20"
                                   IconHeight="20" />
                    <map:MapMarker Latitude="20.593684"
                                   Longitude="78.96288"
                                   IconType="Triangle"
                                   IconFill="LightGreen"
                                   IconStroke="Green"
                                   IconStrokeThickness="2"
                                   IconWidth="20"
                                   IconHeight="20" />
                    <map:MapMarker Latitude="61.52401"
                                   Longitude="105.318756"
                                   IconType="Triangle"
                                   IconFill="LightGreen"
                                   IconStroke="Green"
                                   IconStrokeThickness="2"
                                   IconWidth="20"
                                   IconHeight="20" />
                </map:MapMarkerCollection>
            </map:MapShapeLayer.Markers>
        </map:MapShapeLayer>
    </map:SfMaps.Layer>
</map:SfMaps>
```

### C# Implementation

```csharp
using Syncfusion.Maui.Maps;
using Microsoft.Maui.Controls;
using Microsoft.Maui.Graphics;

namespace MauiMapsApp;

public partial class CustomMarkerPage : ContentPage
{
    public CustomMarkerPage()
    {
        InitializeComponent();
        
        MapShapeLayer layer = new MapShapeLayer();
        layer.ShapesSource = MapSource.FromUri("url"); // For e.g  "https://cdn.syncfusion.com/maps/map-data/world-map.json"
        layer.ShapeStroke = Colors.DarkGrey;

        MapMarker mapMarker1 = new MapMarker
        {
            Latitude = -14.235004,
            Longitude = -51.92528,
            IconHeight = 20,
            IconWidth = 20,
            IconType = MapIconType.Triangle,
            IconFill = Colors.LightGreen,
            IconStroke = Colors.Green,
            IconStrokeThickness = 2
        };

        MapMarker mapMarker2 = new MapMarker
        {
            Latitude = 51.16569,
            Longitude = 10.451526,
            IconHeight = 20,
            IconWidth = 20,
            IconType = MapIconType.Triangle,
            IconFill = Colors.LightGreen,
            IconStroke = Colors.Green,
            IconStrokeThickness = 2
        };

        MapMarker mapMarker3 = new MapMarker
        {
            Latitude = -25.274398,
            Longitude = 133.775136,
            IconHeight = 20,
            IconWidth = 20,
            IconType = MapIconType.Triangle,
            IconFill = Colors.LightGreen,
            IconStroke = Colors.Green,
            IconStrokeThickness = 2
        };

        MapMarker mapMarker4 = new MapMarker
        {
            Latitude = 20.593684,
            Longitude = 78.96288,
            IconHeight = 20,
            IconWidth = 20,
            IconType = MapIconType.Triangle,
            IconFill = Colors.LightGreen,
            IconStroke = Colors.Green,
            IconStrokeThickness = 2
        };

        MapMarker mapMarker5 = new MapMarker
        {
            Latitude = 61.52401,
            Longitude = 105.318756,
            IconHeight = 20,
            IconWidth = 20,
            IconType = MapIconType.Triangle,
            IconFill = Colors.LightGreen,
            IconStroke = Colors.Green,
            IconStrokeThickness = 2
        };

        MapMarkerCollection mapMarkers = new MapMarkerCollection();
        mapMarkers.Add(mapMarker1);
        mapMarkers.Add(mapMarker2);
        mapMarkers.Add(mapMarker3);
        mapMarkers.Add(mapMarker4);
        mapMarkers.Add(mapMarker5);
        
        layer.Markers = mapMarkers;

        SfMaps map = new SfMaps();
        map.Layer = layer;
        
        this.Content = map;
    }
}
```

## Marker Positioning and Alignment

You can change the position of the marker from the given coordinate using the `HorizontalAlignment` and `VerticalAlignment` properties. You can also adjust the marker position using the `Offset` property.

### Horizontal Alignment

```xaml
<!-- Left of coordinate -->
<map:MapMarker Latitude="37.7749" 
               Longitude="-122.4194"
               HorizontalAlignment="Start"
               IconType="Circle"
               IconFill="Red"
               IconWidth="15"
               IconHeight="15" />

<!-- Centered on coordinate (default) -->
<map:MapMarker Latitude="40.7128" 
               Longitude="-74.0060"
               HorizontalAlignment="Center"
               IconType="Circle"
               IconFill="Blue"
               IconWidth="15"
               IconHeight="15" />

<!-- Right of coordinate -->
<map:MapMarker Latitude="51.5074" 
               Longitude="-0.1278"
               HorizontalAlignment="End"
               IconType="Circle"
               IconFill="Green"
               IconWidth="15"
               IconHeight="15" />
```

```csharp
using Syncfusion.Maui.Maps;
using Microsoft.Maui.Graphics;

var marker1 = new MapMarker
{
    Latitude = 37.7749,
    Longitude = -122.4194,
    HorizontalAlignment = MapAlignment.Start,
    IconType = MapIconType.Circle,
    IconFill = Colors.Red,
    IconWidth = 15,
    IconHeight = 15
};

var marker2 = new MapMarker
{
    Latitude = 40.7128,
    Longitude = -74.0060,
    HorizontalAlignment = MapAlignment.Center,  // Default
    IconType = MapIconType.Circle,
    IconFill = Colors.Blue,
    IconWidth = 15,
    IconHeight = 15
};

var marker3 = new MapMarker
{
    Latitude = 51.5074,
    Longitude = -0.1278,
    HorizontalAlignment = MapAlignment.End,
    IconType = MapIconType.Circle,
    IconFill = Colors.Green,
    IconWidth = 15,
    IconHeight = 15
};
```

### Vertical Alignment

```xaml
<!-- Above coordinate -->
<map:MapMarker Latitude="35.6762" 
               Longitude="139.6503"
               VerticalAlignment="Start"
               IconType="Diamond"
               IconFill="Orange"
               IconWidth="15"
               IconHeight="15" />

<!-- Centered on coordinate (default) -->
<map:MapMarker Latitude="-33.8688" 
               Longitude="151.2093"
               VerticalAlignment="Center"
               IconType="Diamond"
               IconFill="Purple"
               IconWidth="15"
               IconHeight="15" />

<!-- Below coordinate (pin-style) -->
<map:MapMarker Latitude="55.7558" 
               Longitude="37.6173"
               VerticalAlignment="End"
               IconType="Diamond"
               IconFill="Pink"
               IconWidth="15"
               IconHeight="15" />
```

```csharp
using Syncfusion.Maui.Maps;
using Microsoft.Maui.Graphics;

var marker1 = new MapMarker
{
    Latitude = 35.6762,
    Longitude = 139.6503,
    VerticalAlignment = MapAlignment.Start,
    IconType = MapIconType.Diamond,
    IconFill = Colors.Orange,
    IconWidth = 15,
    IconHeight = 15
};

var marker2 = new MapMarker
{
    Latitude = -33.8688,
    Longitude = 151.2093,
    VerticalAlignment = MapAlignment.Center,  // Default
    IconType = MapIconType.Diamond,
    IconFill = Colors.Purple,
    IconWidth = 15,
    IconHeight = 15
};

var marker3 = new MapMarker
{
    Latitude = 55.7558,
    Longitude = 37.6173,
    VerticalAlignment = MapAlignment.End,  // Pin-style
    IconType = MapIconType.Diamond,
    IconFill = Colors.Pink,
    IconWidth = 15,
    IconHeight = 15
};
```

### Offset Positioning

Fine-tune marker position with pixel offsets:

```xaml
<!-- Offset 10 pixels right, 20 pixels down -->
<map:MapMarker Latitude="37.7749" 
               Longitude="-122.4194"
               Offset="10,20"
               IconType="Square"
               IconFill="Teal"
               IconWidth="15"
               IconHeight="15" />
```

```csharp
using Syncfusion.Maui.Maps;
using Microsoft.Maui.Graphics;

var marker = new MapMarker
{
    Latitude = 37.7749,
    Longitude = -122.4194,
    Offset = new Point(10, 20),  // X: 10px right, Y: 20px down
    IconType = MapIconType.Square,
    IconFill = Colors.Teal,
    IconWidth = 15,
    IconHeight = 15
};
```

### Combined Alignment and Offset Example

```csharp
using Syncfusion.Maui.Maps;
using Microsoft.Maui.Graphics;

// Pin-style marker: Icon points to exact location
var pinMarker = new MapMarker
{
    Latitude = 37.7749,
    Longitude = -122.4194,
    VerticalAlignment = MapAlignment.End,  // Marker above point
    Offset = new Point(0, -5),  // Lift marker up 5 pixels
    IconType = MapIconType.Triangle,
    IconFill = Colors.Red,
    IconWidth = 20,
    IconHeight = 20
};
```

## Custom Marker Templates

You can show custom markers using the `MarkerTemplate` property of the `MapShapeLayer` which returns a DataTemplate to customize marker appearance.

### Simple Image Marker

**XAML Implementation:**

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:map="clr-namespace:Syncfusion.Maui.Maps;assembly=Syncfusion.Maui.Maps"
             x:Class="MauiMapsApp.CustomMarkerPage">
    
    <ContentPage.Resources>
        <ResourceDictionary>
            <DataTemplate x:Key="MarkerTemplate">
                <StackLayout HorizontalOptions="StartAndExpand"
                             VerticalOptions="Center">
                    <Image Source="map.png"
                           HeightRequest="30"
                           WidthRequest="30" />
                </StackLayout>
            </DataTemplate>
        </ResourceDictionary>
    </ContentPage.Resources>

    <map:SfMaps>
        <map:SfMaps.Layer>
            <map:MapShapeLayer ShapesSource="url"
                               ShapeStroke="DarkGrey"
                               MarkerTemplate="{StaticResource MarkerTemplate}">
                <map:MapShapeLayer.Markers>
                    <map:MapMarkerCollection>
                        <map:MapMarker Latitude="20.5595"
                                       Longitude="22.9375"
                                       HorizontalAlignment="Center"
                                       VerticalAlignment="Start" />
                        <map:MapMarker Latitude="21.7679"
                                       Longitude="78.8718"
                                       HorizontalAlignment="Center"
                                       VerticalAlignment="Start" />
                        <map:MapMarker Latitude="133.7751"
                                       Longitude="25.2744"
                                       HorizontalAlignment="Center"
                                       VerticalAlignment="Start" />
                        <map:MapMarker Latitude="60.2551"
                                       Longitude="84.5260"
                                       HorizontalAlignment="Center"
                                       VerticalAlignment="Start" />
                        <map:MapMarker Latitude="195.4915"
                                       Longitude="-50.7832"
                                       HorizontalAlignment="Center"
                                       VerticalAlignment="Start" />
                    </map:MapMarkerCollection>
                </map:MapShapeLayer.Markers>
            </map:MapShapeLayer>
        </map:SfMaps.Layer>
    </map:SfMaps>
</ContentPage>
```

**C# Implementation:**

```csharp
using Syncfusion.Maui.Maps;
using Microsoft.Maui.Controls;

namespace MauiMapsApp;

public partial class CustomMarkerPage : ContentPage
{
    public CustomMarkerPage()
    {
        InitializeComponent();
        
        MapShapeLayer layer = new MapShapeLayer();
        layer.ShapesSource = MapSource.FromUri("url"); // For e.g  "https://cdn.syncfusion.com/maps/map-data/world-map.json"
        layer.ShapeStroke = Colors.DarkGrey;

        MapMarker mapMarker1 = new MapMarker
        {
            Latitude = 20.5595,
            Longitude = 22.9375,
            HorizontalAlignment = MapAlignment.Center,
            VerticalAlignment = MapAlignment.Start
        };

        MapMarker mapMarker2 = new MapMarker
        {
            Latitude = 21.7679,
            Longitude = 78.8718,
            HorizontalAlignment = MapAlignment.Center,
            VerticalAlignment = MapAlignment.Start
        };

        MapMarker mapMarker3 = new MapMarker
        {
            Latitude = 133.7751,
            Longitude = 25.2744,
            HorizontalAlignment = MapAlignment.Center,
            VerticalAlignment = MapAlignment.Start
        };

        MapMarker mapMarker4 = new MapMarker
        {
            Latitude = 60.2551,
            Longitude = 84.5260,
            HorizontalAlignment = MapAlignment.Center,
            VerticalAlignment = MapAlignment.Start
        };

        MapMarker mapMarker5 = new MapMarker
        {
            Latitude = 195.4915,
            Longitude = -50.7832,
            HorizontalAlignment = MapAlignment.Center,
            VerticalAlignment = MapAlignment.Start
        };

        MapMarkerCollection mapMarkers = new MapMarkerCollection();
        mapMarkers.Add(mapMarker1);
        mapMarkers.Add(mapMarker2);
        mapMarkers.Add(mapMarker3);
        mapMarkers.Add(mapMarker4);
        mapMarkers.Add(mapMarker5);
        
        layer.Markers = mapMarkers;
        layer.MarkerTemplate = CreateMarkerTemplate();
        
        SfMaps map = new SfMaps();
        map.Layer = layer;
        
        this.Content = map;
    }
    
    private DataTemplate CreateMarkerTemplate()
    {
        return new DataTemplate(() =>
        {
            var stackLayout = new StackLayout
            {
                HorizontalOptions = LayoutOptions.Start,
                VerticalOptions = LayoutOptions.Center
            };
            
            var image = new Image
            {
                Source = "map.png",
                WidthRequest = 30,
                HeightRequest = 30
            };
            
            stackLayout.Children.Add(image);
            return stackLayout;
        });
    }
}
```

## Marker Tooltip Customization

You can customize the marker tooltip using the `MarkerTooltipTemplate` property. This allows you to display custom information when users interact with markers.

### Custom Model Class

First, create a custom marker class that extends `MapMarker`:

```csharp
using Syncfusion.Maui.Maps;

namespace MauiMapsApp;

public class CustomMarker : MapMarker
{
    public string Name { get; set; }
    public string Area { get; set; }
}
```

### XAML Implementation

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:map="clr-namespace:Syncfusion.Maui.Maps;assembly=Syncfusion.Maui.Maps"
             xmlns:local="clr-namespace:MauiMapsApp"
             x:Class="MauiMapsApp.MarkerTooltipPage">
    
    <ContentPage.Resources>
        <ResourceDictionary>
            <DataTemplate x:Key="MapMarkerTemplate">
                <StackLayout HorizontalOptions="StartAndExpand"
                             VerticalOptions="Center">
                    <Image Source="map.png"
                           HeightRequest="30"
                           WidthRequest="30" />
                </StackLayout>
            </DataTemplate>
        </ResourceDictionary>
    </ContentPage.Resources>

    <map:SfMaps>
        <map:SfMaps.Layer>
            <map:MapShapeLayer ShapesSource="url"
                               ShapeStroke="DarkGrey"
                               ShapeHoverFill="Transparent"
                               ShapeHoverStroke="Transparent"
                               MarkerTemplate="{StaticResource MapMarkerTemplate}"
                               ShowMarkerTooltip="True">

                <map:MapShapeLayer.Markers>
                    <map:MapMarkerCollection>
                        <local:CustomMarker Name="South Africa"
                                            Area="38,570,000 sq. km."
                                            Latitude="20.5595"
                                            Longitude="22.9375" />
                        <local:CustomMarker Name="India"
                                            Area="30,370,000 sq. km."
                                            Latitude="21.7679"
                                            Longitude="78.8718" />
                        <local:CustomMarker Name="Europe"
                                            Area="20,370,000 sq. km."
                                            Latitude="133.7751"
                                            Longitude="25.2744" />
                        <local:CustomMarker Name="Asia"
                                            Area="50,570,000 sq. km."
                                            Latitude="60.2551"
                                            Longitude="84.5260" />
                        <local:CustomMarker Name="South America"
                                            Area="30,370,000 sq. km."
                                            Latitude="195.4915"
                                            Longitude="-50.7832" />
                    </map:MapMarkerCollection>
                </map:MapShapeLayer.Markers>

                <map:MapShapeLayer.MarkerTooltipTemplate>
                    <DataTemplate>
                        <Grid Padding="10" WidthRequest="150">
                            <Grid.RowDefinitions>
                                <RowDefinition Height="Auto" />
                                <RowDefinition Height="Auto" />
                                <RowDefinition Height="Auto" />
                            </Grid.RowDefinitions>
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="30" />
                                <ColumnDefinition Width="Auto" />
                            </Grid.ColumnDefinitions>
                            
                            <Image Source="flag.png"
                                   Grid.Column="0"
                                   Grid.Row="0"
                                   WidthRequest="20"
                                   HeightRequest="20" />
                            <Label Text="{Binding Name}"
                                   TextColor="White"
                                   Grid.Column="1"
                                   Grid.Row="0"
                                   Padding="10" />
                            <Label Grid.Row="2"
                                   Grid.ColumnSpan="2"
                                   Text="{Binding Area}"
                                   TextColor="White" />
                        </Grid>
                    </DataTemplate>
                </map:MapShapeLayer.MarkerTooltipTemplate>

            </map:MapShapeLayer>
        </map:SfMaps.Layer>
    </map:SfMaps>
</ContentPage>
```

### C# Implementation

```csharp
using Syncfusion.Maui.Maps;
using Microsoft.Maui.Controls;
using Microsoft.Maui.Graphics;

namespace MauiMapsApp;

public partial class MarkerTooltipPage : ContentPage
{
    public MarkerTooltipPage()
    {
        InitializeComponent();
        
        MapShapeLayer layer = new MapShapeLayer();
        layer.ShapesSource = MapSource.FromUri("url"); // For e.g  "https://cdn.syncfusion.com/maps/map-data/world-map.json"
        layer.ShapeStroke = Colors.DarkGrey;
        layer.ShapeHoverFill = Colors.Transparent;
        layer.ShapeHoverStroke = Colors.Transparent;
        
        CustomMarker marker1 = new CustomMarker
        {
            Latitude = 20.5595,
            Longitude = 22.9375,
            Name = "South Africa",
            Area = "38,570,000 sq. km."
        };

        CustomMarker marker2 = new CustomMarker
        {
            Latitude = 21.7679,
            Longitude = 78.8718,
            Name = "India",
            Area = "30,370,000 sq. km."
        };

        CustomMarker marker3 = new CustomMarker
        {
            Latitude = 133.7751,
            Longitude = 25.2744,
            Name = "Europe",
            Area = "20,370,000 sq. km."
        };

        CustomMarker marker4 = new CustomMarker
        {
            Latitude = 60.2551,
            Longitude = 84.5260,
            Name = "Asia",
            Area = "50,570,000 sq. km."
        };

        CustomMarker marker5 = new CustomMarker
        {
            Latitude = 195.4915,
            Longitude = -50.7832,
            Name = "South America",
            Area = "30,370,000 sq. km."
        };

        MapMarkerCollection mapMarkers = new MapMarkerCollection();
        mapMarkers.Add(marker1);
        mapMarkers.Add(marker2);
        mapMarkers.Add(marker3);
        mapMarkers.Add(marker4);
        mapMarkers.Add(marker5);

        layer.Markers = mapMarkers;
        layer.MarkerTemplate = CreateMarkerTemplate();
        layer.MarkerTooltipTemplate = CreateTooltipTemplate();
        layer.ShowMarkerTooltip = true;
        
        SfMaps map = new SfMaps();
        map.Layer = layer;
        
        this.Content = map;
    }
    
    private DataTemplate CreateMarkerTemplate()
    {
        return new DataTemplate(() =>
        {
            var stackLayout = new StackLayout
            {
                HorizontalOptions = LayoutOptions.Start,
                VerticalOptions = LayoutOptions.Center
            };
            
            var image = new Image
            {
                Source = "map.png",
                WidthRequest = 30,
                HeightRequest = 30
            };
            
            stackLayout.Children.Add(image);
            return stackLayout;
        });
    }
    
    private DataTemplate CreateTooltipTemplate()
    {
        return new DataTemplate(() =>
        {
            var grid = new Grid
            {
                Padding = new Thickness(10),
                WidthRequest = 150,
                RowDefinitions =
                {
                    new RowDefinition { Height = GridLength.Auto },
                    new RowDefinition { Height = GridLength.Auto },
                    new RowDefinition { Height = GridLength.Auto }
                },
                ColumnDefinitions =
                {
                    new ColumnDefinition { Width = new GridLength(30) },
                    new ColumnDefinition { Width = GridLength.Auto }
                }
            };
            
            var flagImage = new Image
            {
                Source = "flag.png",
                WidthRequest = 20,
                HeightRequest = 20
            };
            grid.SetRow(flagImage, 0);
            grid.SetColumn(flagImage, 0);
            
            var nameLabel = new Label
            {
                TextColor = Colors.White,
                Padding = new Thickness(10)
            };
            nameLabel.SetBinding(Label.TextProperty, "Name");
            grid.SetRow(nameLabel, 0);
            grid.SetColumn(nameLabel, 1);
            
            var areaLabel = new Label
            {
                TextColor = Colors.White
            };
            areaLabel.SetBinding(Label.TextProperty, "Area");
            grid.SetRow(areaLabel, 2);
            grid.SetColumnSpan(areaLabel, 2);
            
            grid.Children.Add(flagImage);
            grid.Children.Add(nameLabel);
            grid.Children.Add(areaLabel);
            
            return grid;
        });
    }
}

public class CustomMarker : MapMarker
{
    public string Name { get; set; }
    public string Area { get; set; }
}
```

## Template Selectors

You can use a DataTemplateSelector to customize the appearance of each marker with different templates based on specific conditions. Choose a DataTemplate for each item at runtime based on data-bound property values.

### Custom Model Class

```csharp
using Syncfusion.Maui.Maps;

namespace MauiMapsApp;

public class CustomMarkerData : MapMarker
{
    public string Area { get; set; }
    public double Population { get; set; }
}
```

### Template Selector Class

```csharp
using Microsoft.Maui.Controls;

namespace MauiMapsApp;

public class MarkerTemplateSelector : DataTemplateSelector
{
    public DataTemplate Template1 { get; set; }
    public DataTemplate Template2 { get; set; }
    
    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        return ((CustomMarkerData)item).Population < 30 ? Template1 : Template2;
    }
}
```

### XAML Implementation

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:map="clr-namespace:Syncfusion.Maui.Maps;assembly=Syncfusion.Maui.Maps"
             xmlns:local="clr-namespace:MauiMapsApp"
             x:Class="MauiMapsApp.MarkerSelectorPage">
    
    <ContentPage.Resources>
        <ResourceDictionary>
            <DataTemplate x:Key="MarkerTemplate1">
                <StackLayout IsClippedToBounds="false"
                             HorizontalOptions="StartAndExpand"
                             VerticalOptions="Center"
                             HeightRequest="30">
                    <Label Text="{Binding Latitude}"
                           Scale="1"
                           TextColor="White"
                           BackgroundColor="Blue"
                           HorizontalOptions="StartAndExpand"
                           VerticalOptions="Center" />
                </StackLayout>
            </DataTemplate>
            
            <DataTemplate x:Key="MarkerTemplate2">
                <StackLayout IsClippedToBounds="false"
                             HorizontalOptions="StartAndExpand"
                             VerticalOptions="Center"
                             HeightRequest="30">
                    <Label Text="{Binding Latitude}"
                           Scale="1"
                           TextColor="White"
                           BackgroundColor="Red"
                           HorizontalOptions="StartAndExpand"
                           VerticalOptions="Center" />
                </StackLayout>
            </DataTemplate>
            
            <local:MarkerTemplateSelector x:Key="MarkerTemplateSelector"
                                          Template1="{StaticResource MarkerTemplate1}"
                                          Template2="{StaticResource MarkerTemplate2}" />
        </ResourceDictionary>
    </ContentPage.Resources>

    <map:SfMaps>
        <map:SfMaps.Layer>
            <map:MapShapeLayer ShapesSource="url"
                               ShapeStroke="DarkGrey"
                               MarkerTemplate="{StaticResource MarkerTemplateSelector}">
                               
                <map:MapShapeLayer.Markers>
                    <map:MapMarkerCollection>
                        <local:CustomMarkerData Latitude="21.7679"
                                                Longitude="78.8718"
                                                Area="10,370,000 sq. km."
                                                Population="15" />
                        <local:CustomMarkerData Latitude="133.7751"
                                                Longitude="25.2744"
                                                Area="20,370,000 sq. km."
                                                Population="31" />
                        <local:CustomMarkerData Latitude="60.2551"
                                                Longitude="84.5260"
                                                Area="50,570,000 sq. km."
                                                Population="26" />
                        <local:CustomMarkerData Latitude="195.4915"
                                                Longitude="-50.7832"
                                                Area="30,370,000 sq. km."
                                                Population="40" />
                    </map:MapMarkerCollection>
                </map:MapShapeLayer.Markers>

            </map:MapShapeLayer>
        </map:SfMaps.Layer>
    </map:SfMaps>
</ContentPage>
```

### C# Implementation

```csharp
using Syncfusion.Maui.Maps;
using Microsoft.Maui.Controls;
using Microsoft.Maui.Graphics;

namespace MauiMapsApp;

public partial class MarkerSelectorPage : ContentPage
{
    public MarkerSelectorPage()
    {
        InitializeComponent();
        
        MapShapeLayer layer = new MapShapeLayer();
        layer.ShapesSource = MapSource.FromUri("url");
        layer.ShapeStroke = Colors.DarkGrey;
        
        var markers = new MapMarkerCollection
        {
            new CustomMarkerData
            {
                Latitude = 21.7679,
                Longitude = 78.8718,
                Area = "10,370,000 sq. km.",
                Population = 15
            },
            new CustomMarkerData
            {
                Latitude = 133.7751,
                Longitude = 25.2744,
                Area = "20,370,000 sq. km.",
                Population = 31
            },
            new CustomMarkerData
            {
                Latitude = 60.2551,
                Longitude = 84.5260,
                Area = "50,570,000 sq. km.",
                Population = 26
            },
            new CustomMarkerData
            {
                Latitude = 195.4915,
                Longitude = -50.7832,
                Area = "30,370,000 sq. km.",
                Population = 40
            }
        };
        
        layer.Markers = markers;
        layer.MarkerTemplate = new MarkerTemplateSelector
        {
            Template1 = CreateTemplate1(),
            Template2 = CreateTemplate2()
        };
        
        SfMaps map = new SfMaps();
        map.Layer = layer;
        
        this.Content = map;
    }
    
    private DataTemplate CreateTemplate1()
    {
        return new DataTemplate(() =>
        {
            var stackLayout = new StackLayout
            {
                IsClippedToBounds = false,
                HorizontalOptions = LayoutOptions.Start,
                VerticalOptions = LayoutOptions.Center,
                HeightRequest = 30
            };
            
            var label = new Label
            {
                Scale = 1,
                TextColor = Colors.White,
                BackgroundColor = Colors.Blue,
                HorizontalOptions = LayoutOptions.Start,
                VerticalOptions = LayoutOptions.Center
            };
            label.SetBinding(Label.TextProperty, "Latitude");
            
            stackLayout.Children.Add(label);
            return stackLayout;
        });
    }
    
    private DataTemplate CreateTemplate2()
    {
        return new DataTemplate(() =>
        {
            var stackLayout = new StackLayout
            {
                IsClippedToBounds = false,
                HorizontalOptions = LayoutOptions.Start,
                VerticalOptions = LayoutOptions.Center,
                HeightRequest = 30
            };
            
            var label = new Label
            {
                Scale = 1,
                TextColor = Colors.White,
                BackgroundColor = Colors.Red,
                HorizontalOptions = LayoutOptions.Start,
                VerticalOptions = LayoutOptions.Center
            };
            label.SetBinding(Label.TextProperty, "Latitude");
            
            stackLayout.Children.Add(label);
            return stackLayout;
        });
    }
}

public class CustomMarkerData : MapMarker
{
    public string Area { get; set; }
    public double Population { get; set; }
}

public class MarkerTemplateSelector : DataTemplateSelector
{
    public DataTemplate Template1 { get; set; }
    public DataTemplate Template2 { get; set; }
    
    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        return ((CustomMarkerData)item).Population < 30 ? Template1 : Template2;
    }
}
```

## Best Practices

### 1. Performance Optimization

When working with large numbers of markers:

```csharp
using Syncfusion.Maui.Maps;
using System.Collections.Generic;
using System.Linq;

// Limit the number of visible markers
const int MaxVisibleMarkers = 100;

var visibleMarkers = allMarkers
    .Take(MaxVisibleMarkers)
    .ToList();

var markerCollection = new MapMarkerCollection();
foreach (var marker in visibleMarkers)
{
    markerCollection.Add(marker);
}

layer.Markers = markerCollection;
```

### 2. Consistent Icon Sizing

Maintain consistent icon sizes for better visual hierarchy:

```csharp
using Syncfusion.Maui.Maps;
using Microsoft.Maui.Graphics;

const double StandardIconSize = 20;
const double LargeIconSize = 30;
const double SmallIconSize = 15;

// Primary markers (larger)
var primaryMarker = new MapMarker
{
    Latitude = location.Lat,
    Longitude = location.Lng,
    IconWidth = LargeIconSize,
    IconHeight = LargeIconSize,
    IconType = MapIconType.Circle,
    IconFill = Colors.Red
};

// Secondary markers (standard)
var secondaryMarker = new MapMarker
{
    Latitude = location.Lat,
    Longitude = location.Lng,
    IconWidth = StandardIconSize,
    IconHeight = StandardIconSize,
    IconType = MapIconType.Circle,
    IconFill = Colors.Blue
};
```

### 3. Color Contrast for Visibility

Ensure markers are visible against different map backgrounds:

```csharp
using Syncfusion.Maui.Maps;
using Microsoft.Maui.Graphics;

// Always add stroke for better visibility
var marker = new MapMarker
{
    Latitude = 37.7749,
    Longitude = -122.4194,
    IconType = MapIconType.Circle,
    IconFill = Colors.Red,
    IconStroke = Colors.White,  // Contrast border
    IconStrokeThickness = 2,    // Visible stroke
    IconWidth = 20,
    IconHeight = 20
};
```

### 4. Proper Namespace Declarations

Always include complete namespace declarations:

```csharp
using Syncfusion.Maui.Maps;           // Core Maps APIs
using Microsoft.Maui.Controls;        // MAUI controls
using Microsoft.Maui.Graphics;        // Color and Point types
using System;                         // Uri class
```

### 5. Dynamic Marker Updates

Use `MapMarkerCollection` for dynamic marker management:

```csharp
using Syncfusion.Maui.Maps;
using Microsoft.Maui.Graphics;

public class LiveTrackingService
{
    private MapShapeLayer _layer;
    private MapMarkerCollection _markers;
    
    public void Initialize(MapShapeLayer layer)
    {
        _layer = layer;
        _markers = new MapMarkerCollection();
        _layer.Markers = _markers;
    }
    
    public void AddMarker(double lat, double lng, Color color)
    {
        var marker = new MapMarker
        {
            Latitude = lat,
            Longitude = lng,
            IconType = MapIconType.Circle,
            IconFill = color,
            IconWidth = 15,
            IconHeight = 15
        };
        _markers.Add(marker);
    }
    
    public void RemoveMarker(MapMarker marker)
    {
        _markers.Remove(marker);
    }
    
    public void ClearAllMarkers()
    {
        _markers.Clear();
    }
}
```

## Key Takeaways

✅ **Core Syncfusion Maps APIs:**
- `MapMarker` - Class for creating markers with `Latitude`, `Longitude`, and visual properties
- `MapMarkerCollection` - Collection class for managing multiple markers via `Add()`, `Remove()`, `Clear()`
- `IconType` (MapIconType) - Built-in shapes: Circle, Diamond, Rectangle, Square, Triangle
- `HorizontalAlignment` / `VerticalAlignment` - Positioning relative to coordinates (Start, Center, End)
- `MarkerTemplate` / `MarkerTooltipTemplate` - DataTemplates for custom marker appearance and tooltips

✅ **Essential Concepts:**
- Markers denote specific locations on maps with geographic coordinates
- Work on both `MapShapeLayer` (GeoJSON maps) and `MapTileLayer` (online tile maps)
- Built-in icon types provide quick implementation; custom templates enable rich visualizations
- Alignment and offset properties control precise marker positioning
- Template selectors enable conditional marker appearance based on data

✅ **Implementation Patterns:**
- Basic markers: Set `Latitude`, `Longitude`, `IconType`, `IconFill`, `IconWidth`, `IconHeight`
- Custom markers: Use `MarkerTemplate` with DataTemplate for images or complex layouts
- Interactive markers: Enable `ShowMarkerTooltip` and set `MarkerTooltipTemplate` for data display
- Dynamic markers: Create custom marker class extending `MapMarker` with additional properties
- Conditional styling: Implement `DataTemplateSelector` to apply different templates based on data

✅ **Best Practices:**
- Use consistent icon sizes for visual hierarchy (standard: 15-20px, primary: 25-30px)
- Add `IconStroke` with contrasting color for better visibility on varied backgrounds
- Limit visible markers to ~100 for optimal performance; filter or cluster for more
- Create custom marker classes extending `MapMarker` for additional data properties
- Use `VerticalAlignment.End` with negative offset for pin-style markers
- Always include complete namespace declarations (`Syncfusion.Maui.Maps`, `Microsoft.Maui.Graphics`)

## Related Topics

- [Tile Layer](tile-layer.md) - Display markers on tile-based maps
- [Shape Layer](shape-layer.md) - Display markers on shape-based maps
- [Data Labels](data-labels.md) - Add labels to map elements
- [Tooltip](tooltip.md) - Configure tooltip behavior
- [Zoom and Pan](zoom-pan.md) - Control map navigation with markers