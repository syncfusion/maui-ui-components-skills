# Maps Migration: Xamarin.Forms to .NET MAUI

Migration guide for SfMaps control from Xamarin.Forms to .NET MAUI.

## Table of Contents
- [Overview](#overview)
- [Namespace Changes](#namespace-changes)
- [Key Changes](#key-changes)
- [Layer Migration](#layer-migration)
- [Marker Migration](#marker-migration)
- [Data Binding Changes](#data-binding-changes)
- [Migration Examples](#migration-examples)
- [Troubleshooting](#troubleshooting)

## Overview

SfMaps control migrates with updated APIs for layer configuration, marker handling, and data binding.

## Namespace Changes

```csharp
// Xamarin
using Syncfusion.SfMaps.XForms;

// MAUI
using Syncfusion.Maui.Maps;
```

## Key Changes

- Improved layer configuration
- Enhanced marker and data visualization
- Better shape file handling
- Updated GeoJSON support
- Improved zoom and pan interactions

## Layer Migration

### ShapeFileLayer

**Xamarin:**
```xml
<maps:SfMaps>
    <maps:SfMaps.Layers>
        <maps:ShapeFileLayer Uri="world-map.shp">
            <maps:ShapeFileLayer.ShapeSettings>
                <maps:ShapeSetting ShapeFill="LightBlue"
                                   ShapeStroke="Black"
                                   ShapeStrokeWidth="1"/>
            </maps:ShapeFileLayer.ShapeSettings>
        </maps:ShapeFileLayer>
    </maps:SfMaps.Layers>
</maps:SfMaps>
```

**.NET MAUI:**
```xml
<maps:SfMaps>
    <maps:SfMaps.Layer>
        <maps:MapShapeLayer ShapesSource="world-map.shp">
            <maps:MapShapeLayer.ShapeSettings>
                <maps:MapShapeSettings Fill="LightBlue"
                                       Stroke="Black"
                                       StrokeThickness="1"/>
            </maps:MapShapeLayer.ShapeSettings>
        </maps:MapShapeLayer>
    </maps:SfMaps.Layer>
</maps:SfMaps>
```

### Property Changes

| Xamarin | MAUI | Description |
|---------|------|-------------|
| `ShapeFill` | `Fill` | Shape fill color |
| `ShapeStroke` | `Stroke` | Shape border color |
| `ShapeStrokeWidth` | `StrokeThickness` | Border thickness |
| `Uri` | `ShapesSource` | Shape file source |

## Marker Migration

**Xamarin:**
```xml
<maps:ShapeFileLayer>
    <maps:ShapeFileLayer.Markers>
        <maps:MapMarker Latitude="37.7749"
                       Longitude="-122.4194"
                       Label="San Francisco"/>
    </maps:ShapeFileLayer.Markers>
</maps:ShapeFileLayer>
```

**.NET MAUI:**
```xml
<maps:MapShapeLayer>
    <maps:MapShapeLayer.Markers>
        <maps:MapMarker Latitude="37.7749"
                       Longitude="-122.4194"/>
    </maps:MapShapeLayer.Markers>
</maps:MapShapeLayer>
```

**Property Changes:**

| Xamarin | MAUI |
|---------|------|
| `Label` | No alternate API. Use `MarkerTemplate` instead |

## Data Binding Changes

**Xamarin:**
```csharp
shapeLayer.ItemsSource = countries;
shapeLayer.ShapeIDPath = "CountryCode";
shapeLayer.ShapeIDTableField = "ISO_CODE";
```

**.NET MAUI:**
```csharp
shapeLayer.DataSource = countries;
shapeLayer.PrimaryValuePath = "CountryCode";
shapeLayer.ShapeDataField = "ISO_CODE";
```

**Property Changes:**

| Xamarin | MAUI |
|---------|------|
| `ItemsSource` | `DataSource` |
| `ShapeIDPath` | `PrimaryValuePath` |
| `ShapeIDTableField` | `ShapeDataField` |

## Migration Examples

### Complete Map Setup

**Xamarin:**
```csharp
SfMaps maps = new SfMaps();

ShapeFileLayer layer = new ShapeFileLayer();
layer.Uri = "world-map.shp";
layer.ItemsSource = viewModel.Countries;
layer.ShapeIDPath = "Name";
layer.ShapeIDTableField = "NAME";

ShapeSetting shapeSetting = new ShapeSetting();
shapeSetting.ShapeFill = Color.LightBlue;
shapeSetting.ShapeStroke = Color.Black;
shapeSetting.ShapeStrokeWidth = 1;
layer.ShapeSettings = shapeSetting;

maps.Layers.Add(layer);
```

**.NET MAUI:**
```csharp
SfMaps maps = new SfMaps();

MapShapeLayer layer = new MapShapeLayer();
layer.ShapesSource = MapSource.FromResource("world-map.shp");
layer.DataSource = viewModel.Countries;
layer.PrimaryValuePath = "Name";
layer.ShapeDataField = "NAME";

MapShapeSettings shapeSettings = new MapShapeSettings();
shapeSettings.Fill = Colors.LightBlue;
shapeSettings.Stroke = Colors.Black;
shapeSettings.StrokeThickness = 1;
layer.ShapeSettings = shapeSettings;

maps.Layer = layer;
```

### Adding Markers

**Both Xamarin and MAUI (Not similar):**
```csharp
MapMarker marker = new MapMarker
{
    Latitude = 37.7749,
    Longitude = -122.4194,
    Text = "San Francisco"  // "Label" in Xamarin, not available in MAUI
};
layer.Markers.Add(marker);
```

### Color Mapping

**Xamarin:**
```xml
<maps:ShapeFileLayer.ShapeSettings>
    <maps:ShapeSetting>
        <maps:ShapeSetting.ColorMappings>
            <maps:RangeColorMapping From="0" To="100" Color="Green"/>
            <maps:RangeColorMapping From="100" To="200" Color="Yellow"/>
        </maps:ShapeSetting.ColorMappings>
    </maps:ShapeSetting>
</maps:ShapeFileLayer.ShapeSettings>
```

**.NET MAUI:**
```xml
<maps:MapShapeLayer.ColorMappings>
    <maps:RangeColorMapping From="0" To="100" Fill="Green"/>
    <maps:RangeColorMapping From="100" To="200" Fill="Yellow"/>
</maps:MapShapeLayer.ColorMappings>
```

## Troubleshooting

### Issue: Uri property not found

**Solution:** Use `ShapesSource`:
```csharp
// Change
layer.Uri = "world-map.shp";

// To
layer.ShapesSource = MapSource.FromResource("world-map.shp");
```

### Issue: ItemsSource not working

**Solution:** Use `DataSource`:
```csharp
// Change
layer.ItemsSource = data;

// To
layer.DataSource = data;
```

### Issue: ShapeIDPath/ShapeIDTableField not found

**Solution:** Use `PrimaryValuePath` and `ShapeDataField`:
```csharp
// Change
layer.ShapeIDPath = "Name";
layer.ShapeIDTableField = "NAME";

// To
layer.PrimaryValuePath = "Name";
layer.ShapeDataField = "NAME";
```

### Issue: Color properties not accepting Color values

**Solution:** Use Brush or implicit color conversion:
```csharp
// Change
shapeSetting.ShapeFill = Color.Blue;

// To
shapeSettings.Fill = Colors.Blue;
// Or
shapeSettings.Fill = new SolidColorBrush(Colors.Blue);
```

## Next Steps

1. Update NuGet package: `Syncfusion.Maui.Maps`
2. Update namespaces
3. Replace `Uri` → `ShapesSource`
4. Replace `ItemsSource` → `DataSource`
5. Update shape binding properties
6. Update Color → Brush properties
7. Test map rendering and interactions
