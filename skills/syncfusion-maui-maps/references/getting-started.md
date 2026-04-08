# Getting Started with .NET MAUI Maps

## Table of Contents
- [Installation](#installation)
- [Handler Registration](#handler-registration)
- [Creating Your First Shape Layer Map](#creating-your-first-shape-layer-map)
- [Loading Shape Data from local](#loading-shape-data-from-various-sources)
- [Creating Your First Tile Layer Map](#creating-your-first-tile-layer-map)
- [Basic Configuration](#basic-configuration)
- [Troubleshooting Initial Setup](#troubleshooting-initial-setup)

## Installation

### Step 1: Install NuGet Package

**Option A: Package Manager Console**
```bash
Install-Package Syncfusion.Maui.Maps
```

**Option B: .NET CLI**
```bash
dotnet add package Syncfusion.Maui.Maps
```

**Option C: Visual Studio NuGet Manager**
1. Right-click project → **Manage NuGet Packages**
2. Search for **Syncfusion.Maui.Maps**
3. Install the latest stable version

**Package:** `Syncfusion.Maui.Maps`
**Dependencies:** Automatically includes `Syncfusion.Maui.Core`

### Step 2: Verify Installation

Check your `.csproj` file contains:
```xml
<ItemGroup>
  <PackageReference Include="Syncfusion.Maui.Maps" Version="27.1.48" />
  <!-- Syncfusion.Maui.Core is automatically included as a dependency -->
</ItemGroup>
```

**Note:** Version number may vary. Always use the latest stable version available.

## Handler Registration

### Required: Register Syncfusion Core Handler

Open `MauiProgram.cs` and register the Syncfusion core handler:

```csharp
using Microsoft.Maui;
using Microsoft.Maui.Hosting;
using Microsoft.Maui.Controls.Hosting;
using Syncfusion.Maui.Core.Hosting;

namespace YourApp;

public static class MauiProgram
{
    public static MauiApp CreateMauiApp()
    {
        var builder = MauiApp.CreateBuilder();
        builder
            .UseMauiApp<App>()
            .ConfigureSyncfusionCore()  // ← Required for all Syncfusion controls
            .ConfigureFonts(fonts =>
            {
                fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                fonts.AddFont("OpenSans-Semibold.ttf", "OpenSansSemibold");
            });

        return builder.Build();
    }
}
```

**⚠️ Critical:** `ConfigureSyncfusionCore()` is mandatory for all Syncfusion .NET MAUI controls. Without it:
- Maps will not render properly
- Controls may crash at runtime
- Licensing validation will not work

**API Reference:**
- `ConfigureSyncfusionCore()` - Extension method from `Syncfusion.Maui.Core.Hosting` namespace


### Creating Your First Shape Layer Map (Local / Trusted Data – Recommended)

Use local or embedded map data to avoid consuming untrusted third‑party content.

### Step 1: Add Namespace

**XAML:**
```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:map="clr-namespace:Syncfusion.Maui.Maps;assembly=Syncfusion.Maui.Maps"
             x:Class="YourApp.MapPage">

</ContentPage>
```

**C# Code-behind:**
```csharp
using Syncfusion.Maui.Maps;
```

### Step 2: Create Basic Shape Layer Map

**XAML:**
```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:map="clr-namespace:Syncfusion.Maui.Maps;assembly=Syncfusion.Maui.Maps"
             x:Class="YourApp.MapPage">
    
    <map:SfMaps>
        <map:SfMaps.Layer>
            <map:MapShapeLayer ShapesSource="url"
                               ShapeStroke="DarkGray"
                               ShapeFill="LightGray" />
        </map:SfMaps.Layer>
    </map:SfMaps>
</ContentPage>
```

**C#:**
```csharp
using Syncfusion.Maui.Maps;
using Microsoft.Maui.Controls;

namespace YourApp
{
    public class MapPage : ContentPage
    {
        public MapPage()
        {
            SfMaps maps = new SfMaps();
            MapShapeLayer layer = new MapShapeLayer
            {           
                ShapesSource = MapSource.FromResource(
                "url"
                ), // For e.g "https://cdn.syncfusion.com/maps/map-data/world-map.json"
                ShapeStroke = Colors.DarkGray,
                ShapeFill = Colors.LightGray
            };
            
            maps.Layer = layer;
            Content = maps;
        }
    }
}
```

**Result:** Displays a world map with light gray shapes and dark gray borders.

**Key APIs Used:**
- `SfMaps` - Main maps control
- `MapShapeLayer` - Layer for rendering GeoJSON/shapefile shapes
- `ShapesSource` - Property to set the map data source
- `ShapeStroke` - Border color of shapes
- `ShapeFill` - Fill color of shapes

### Step 3: Add Data Binding

Create a ViewModel with data:

```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;

namespace YourApp.ViewModels
{
    public class MapViewModel
    {
        public ObservableCollection<Country> Countries { get; set; }
        public MapViewModel()
        {
            Countries = new ObservableCollection<Country>
            {
                new Country { Name = "United States", Code = "USA", Population = 331900000 },
                new Country { Name = "China", Code = "CHN", Population = 1444216000 },
                new Country { Name = "India", Code = "IND", Population = 1393409000 },
                new Country { Name = "Brazil", Code = "BRA", Population = 214300000 },
                new Country { Name = "Indonesia", Code = "IDN", Population = 276361000 },
                new Country { Name = "Pakistan", Code = "PAK", Population = 225200000 }
            };
        }   
    }
    
    public class Country
    {
        public string Name { get; set; }
        public string Code { get; set; }
        public long Population { get; set; }
    }
}
```

Bind to the map:

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:map="clr-namespace:Syncfusion.Maui.Maps;assembly=Syncfusion.Maui.Maps"
             xmlns:local="clr-namespace:YourApp.ViewModels"
             x:Class="YourApp.MapPage">
    
    <ContentPage.BindingContext>
        <local:MapViewModel />
    </ContentPage.BindingContext>

    <map:SfMaps>
        <map:SfMaps.Layer>
            <map:MapShapeLayer      ShapesSource="url"
                               DataSource="{Binding Countries}"
                               PrimaryValuePath="Name"
                               ShapeDataField="name"
                               ShapeColorValuePath="Population"
                               ShowDataLabels="True">
                
                <map:MapShapeLayer.DataLabelSettings>
                    <map:MapDataLabelSettings DataLabelPath="Code" />
                </map:MapShapeLayer.DataLabelSettings>
            </map:MapShapeLayer>
        </map:SfMaps.Layer>
    </map:SfMaps>
</ContentPage>
```

**Explanation:**
- `DataSource`: Your data collection bound from ViewModel
- `PrimaryValuePath`: Property in your data model ("Name") - case-sensitive
- `ShapeDataField`: Property in GeoJSON data ("name") - must match GeoJSON property exactly
- `ShapeColorValuePath`: Property used for color mapping ("Population")
- `ShowDataLabels`: Enables data labels on shapes
- Maps matches `PrimaryValuePath` with  `ShapeDataField` - to bind data to shapes

**Key Data Binding APIs:**
- `DataSource` - Collection of data objects
- `PrimaryValuePath` - Model property for matching
- `ShapeDataField` - Shape property for matching
- `ShapeColorValuePath` - Property for color value mapping
- `ShowDataLabels` - Boolean to show/hide labels
- `DataLabelSettings` - Configuration for label appearance
- `DataLabelPath` - Property to display in labels

## Loading Shape Data from local

The Maps control supports loading  shapefile data from local:


### 1. From Local File

**Best for:** Offline maps or pre-downloaded data

```csharp
// Absolute path
layer.ShapesSource = MapSource.FromFile(@"C:\Data\usa-states.json");

// Relative path (from app directory)
layer.ShapesSource = MapSource.FromFile("Assets/usa-states.json");
```

**API:** `MapSource.FromFile(string filePath)`
- Returns: `MapSource` object
- Supports both absolute and relative paths
- Works with .json, .geojson, and .shp files

### 2. From Embedded Resource

**Best for:** Maps bundled with your app (no internet required)

**Step 1:** Add file to project and set **Build Action** to **EmbeddedResource**

1. Add `world-map.json` to `Assets` folder
2. Right-click → Properties
3. Set Build Action: **EmbeddedResource**

**Step 2:** Load the resource:
```csharp
// Resource ID format: {AssemblyName}.{FolderPath}.{FileName}
layer.ShapesSource = MapSource.FromResource("url");
```

**API:** `MapSource.FromResource(string resourceId)`
- Returns: `MapSource` object
- Resource ID is case-sensitive
- No path separators (use dots)
- Assembly name must match your project

**Troubleshooting Resource ID:**
```csharp
// Get all embedded resource names
var assembly = Assembly.GetExecutingAssembly();
var resources = assembly.GetManifestResourceNames();
foreach (var resource in resources)
{
    System.Diagnostics.Debug.WriteLine(resource);
}
```
### Supported Formats

- **GeoJSON** (`.json`, `.geojson`) - Recommended
- **Shapefile** (`.shp`) - Requires associated `.dbf` and `.shx` files

## Creating Your First Tile Layer Map

Tile layers render raster map tiles.

### Example

**XAML:**
```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:map="clr-namespace:Syncfusion.Maui.Maps;assembly=Syncfusion.Maui.Maps"
             x:Class="YourApp.TileMapPage">
    
    <map:SfMaps>
        <map:SfMaps.Layer>
            <map:MapTileLayer UrlTemplate="url">
                <map:MapTileLayer.Center>
                    <map:MapLatLng Latitude="37.7749" Longitude="-122.4194" />
                </map:MapTileLayer.Center>
                <map:MapTileLayer.ZoomPanBehavior>
                    <map:MapZoomPanBehavior ZoomLevel="12" />
                </map:MapTileLayer.ZoomPanBehavior>
            </map:MapTileLayer>
        </map:SfMaps.Layer>
    </map:SfMaps>
</ContentPage>
```

**C#:**
```csharp
using Syncfusion.Maui.Maps;

namespace YourApp
{
    public class TileMapPage : ContentPage
    {
        public TileMapPage()
        {
            SfMaps maps = new SfMaps();
            MapTileLayer tileLayer = new MapTileLayer
            {
                 UrlTemplate = "url" // For e.g "https://tile.openstreetmap.org/{z}/{x}/{y}.png"
                Center = new MapLatLng(37.7749, -122.4194), // San Francisco
                ZoomPanBehavior = new MapZoomPanBehavior
                {
                    ZoomLevel = 12
                }
            };

            maps.Layer = tileLayer;
            Content = maps;
        }
    }
}
```

**URL Template Format:**
- `{z}`: Zoom level (0-20 depending on provider)
- `{x}`: Tile X coordinate
- `{y}`: Tile Y coordinate

The control automatically replaces these placeholders based on viewport.

**Key Tile Layer APIs:**
- `MapTileLayer` - Layer for rendering map tiles
- `Center` - Initial center point (MapLatLng)
- `ZoomLevel` - Initial zoom level (1-20)
- `MapLatLng` - Latitude/Longitude coordinate class
- `MinZoomLevel` - Minimum allowed zoom
- `MaxZoomLevel` - Maximum allowed zoom

### Adding Markers to Tile Layer

**XAML:**
```xaml
<map:MapTileLayer UrlTemplate="url">
    <map:MapTileLayer.Center>
        <map:MapLatLng Latitude="37.7749" Longitude="-122.4194" />
    </map:MapTileLayer.Center>
    <map:MapTileLayer.ZoomPanBehavior>
        <map:MapZoomPanBehavior ZoomLevel="12" />
    </map:MapTileLayer.ZoomPanBehavior>
    
    <map:MapTileLayer.Markers>
        <map:MapMarkerCollection>
            <map:MapMarker Latitude="37.7749" 
                          Longitude="-122.4194"
                          IconType="Circle"
                          IconFill="Red"
                          IconHeight="20"
                          IconWidth="20" />
            <map:MapMarker Latitude="37.8044" 
                          Longitude="-122.2712"
                          IconType="Diamond"
                          IconFill="Blue"
                          IconHeight="20"
                          IconWidth="20" />
        </map:MapMarkerCollection>
    </map:MapTileLayer.Markers>
</map:MapTileLayer>
```

**C#:**
```csharp
MapTileLayer tileLayer = new MapTileLayer
{
    UrlTemplate = "url", // For e.g "https://tile.openstreetmap.org/{z}/{x}/{y}.png"
    Center = new MapLatLng(37.7749, -122.4194),
    ZoomPanBehavior = new MapZoomPanBehavior
    {
        ZoomLevel = 12
    }
};

MapMarker marker1 = new MapMarker
{
    Latitude = 37.7749,
    Longitude = -122.4194,
    IconType = MapIconType.Circle,
    IconFill = Colors.Red,
    IconHeight = 20,
    IconWidth = 20
};

MapMarker marker2 = new MapMarker
{
    Latitude = 37.8044,
    Longitude = -122.2712,
    IconType = MapIconType.Diamond,
    IconFill = Colors.Blue,
    IconHeight = 20,
    IconWidth = 20
};

tileLayer.Markers = new MapMarkerCollection { marker1, marker2 };
```

**Marker APIs:**
- `MapMarker` - Marker/pin object
- `Latitude` - Marker latitude position
- `Longitude` - Marker longitude position
- `IconType` - Shape: Circle, Diamond, Rectangle, Square, etc.
- `IconFill` - Marker fill color
- `IconStroke` - Marker border color
- `IconHeight` - Marker height in pixels
- `IconWidth` - Marker width in pixels
- `MapMarkerCollection` - Collection of markers

## Basic Configuration

### Setting Map Size

Maps automatically fill available space. Control size via container:

```xaml
<map:SfMaps HeightRequest="400" WidthRequest="600">
    <!-- Layer configuration -->
</map:SfMaps>
```

**C#:**
```csharp
SfMaps maps = new SfMaps
{
    HeightRequest = 400,
    WidthRequest = 600
};
```

**Size Properties:**
- `HeightRequest` - Requested height in pixels
- `WidthRequest` - Requested width in pixels
- `MinimumHeightRequest` - Minimum height
- `MinimumWidthRequest` - Minimum width

### Styling Shapes

```xaml
<map:MapShapeLayer ShapesSource="url" 
                   ShapeFill="#E0F7FA"
                   ShapeStroke="#00838F"
                   ShapeStrokeThickness="1"
                   ShapeHoverFill="#B2EBF2"
                   ShapeHoverStroke="#006064"
                   ShapeHoverStrokeThickness="2" />
```

**C#:**
```csharp
MapShapeLayer layer = new MapShapeLayer
{
    ShapesSource = MapSource.FromFile("url"), 
    ShapeFill = Color.FromArgb("#E0F7FA"),
    ShapeStroke = Color.FromArgb("#00838F"),
    ShapeStrokeThickness = 1,
    ShapeHoverFill = Color.FromArgb("#B2EBF2"),
    ShapeHoverStroke = Color.FromArgb("#006064"),
    ShapeHoverStrokeThickness = 2
};
```

**Shape Styling APIs:**
- `ShapeFill` - Fill color for shapes
- `ShapeStroke` - Border color for shapes
- `ShapeStrokeThickness` - Border width in pixels
- `ShapeHoverFill` - Fill color when hovered/tapped
- `ShapeHoverStroke` - Border color when hovered/tapped
- `ShapeHoverStrokeThickness` - Border width when hovered/tapped

### Background and Padding

```xaml
<map:SfMaps BackgroundColor="White"
            Padding="10,20,10,20">
    <map:SfMaps.Layer>
        <map:MapShapeLayer ShapesSource="url" />
    </map:SfMaps.Layer>
</map:SfMaps>
```

**C#:**
```csharp
SfMaps maps = new SfMaps
{
    BackgroundColor = Colors.White,
    Padding = new Thickness(10, 20, 10, 20)
};
```

**Layout APIs:**
- `BackgroundColor` - Background color of map control
- `Padding` - Inner spacing (Left, Top, Right, Bottom)
- `Margin` - Outer spacing

## Troubleshooting Initial Setup

### Issue: Maps Not Rendering

**Symptoms:** Blank screen, no map visible

**Solutions:**
1. ✅ Verify `ConfigureSyncfusionCore()` is called in `MauiProgram.cs`
2. ✅ Check NuGet package is installed
3. ✅ Ensure namespace is imported: `xmlns:map="clr-namespace:Syncfusion.Maui.Maps;assembly=Syncfusion.Maui.Maps"`
4. ✅ Verify ShapesSource URL is accessible (test in browser)

### Issue: Shapes Not Loading

**Symptoms:** Map control renders but shapes are missing

**Solutions:**
1. ✅ Verify GeoJSON/shapefile URL or path is correct
2. ✅ Check network connectivity for remote URLs
3. ✅ For embedded resources, verify Build Action is set to **EmbeddedResource**
4. ✅ Check resource ID format matches: `{AssemblyName}.{Path}.{FileName}`

### Issue: Data Not Binding to Shapes

**Symptoms:** Shapes render but data (colors, labels) not applied

**Solutions:**
1. ✅ Verify `PrimaryValuePath` matches your model property name (case-sensitive)
2. ✅ Verify `ShapeDataField` matches GeoJSON property name (check GeoJSON structure)
3. ✅ Ensure `DataSource` is properly bound to ViewModel
4. ✅ Check that data values exist in both DataSource and GeoJSON

**Example mismatch:**
```csharp
// Model property: "CountryName"
// GeoJSON property: "name"
PrimaryValuePath = "CountryName"  // ✓ Matches model
ShapeDataField = "name"            // ✓ Matches GeoJSON
```

### Issue: Android Deployment Errors

**Symptoms:** Build succeeds but crashes on Android

**Solutions:**
1. ✅ Add linker preservation for data models:
   ```csharp
   using System.Diagnostics.CodeAnalysis;
   
   [DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicProperties)]
   public class Country
   {
       public string Name { get; set; }
       public string Code { get; set; }
       public long Population { get; set; }
   }
   ```

2. ✅ Set linking behavior in .csproj:
   ```xml
   <PropertyGroup Condition="'$(Configuration)' == 'Debug'">
       <AndroidLinkMode>None</AndroidLinkMode>
   </PropertyGroup>
   ```

3. ✅ Ensure `Platforms/Android/AndroidManifest.xml` has internet permission:
   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <manifest xmlns:android="http://schemas.android.com/apk/res/android">
       <application android:allowBackup="true" />
       <uses-permission android:name="android.permission.INTERNET" />
       <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
   </manifest>
   ```

4. ✅ For iOS AOT compilation, add `[Preserve]` attribute:
   ```csharp
   using Foundation;
   
   [Preserve(AllMembers = true)]
   public class Country
   {
       public string Name { get; set; }
       public string Code { get; set; }
       public long Population { get; set; }
   }
   ```

### Issue: Tile Layer Shows Gray Tiles

**Symptoms:** Tile layer renders with placeholder gray tiles

**Solutions:**
1. ✅ Verify internet connectivity
2. ✅ Check tile service is operational (test URL in browser)
3. ✅ Ensure subscription key is included (Bing, Azure, Google, TomTom)
4. ✅ Review tile provider's terms of service and rate limits
5. ✅ Check for CORS restrictions (web platforms)

## Common APIs Summary

### Core Control APIs

| API | Type | Description |
|-----|------|-------------|
| `SfMaps` | Class | Main maps control container |
| `Layer` | Property | Sets the map layer (shape or tile) |
| `BackgroundColor` | Property | Background color of map |
| `HeightRequest` | Property | Requested height |
| `WidthRequest` | Property | Requested width |

### Shape Layer APIs

| API | Type | Description |
|-----|------|-------------|
| `MapShapeLayer` | Class | Layer for GeoJSON/shapefile rendering |
| `ShapesSource` | Property | Map data source (MapSource) |
| `DataSource` | Property | Collection for data binding |
| `PrimaryValuePath` | Property | Model property for matching |
| `ShapeDataField` | Property | GeoJSON property for matching |
| `ShapeColorValuePath` | Property | Property for color mapping |
| `ShapeFill` | Property | Default shape fill color |
| `ShapeStroke` | Property | Shape border color |
| `ShapeStrokeThickness` | Property | Border thickness |
| `ShowDataLabels` | Property | Enable/disable data labels |
| `ShowShapeTooltip` | Property | Enable/disable tooltips |

### Tile Layer APIs

| API | Type | Description |
|-----|------|-------------|
| `MapTileLayer` | Class | Layer for raster tile rendering |
| `UrlTemplate` | Property | Tile provider URL pattern |
| `Center` | Property | Initial center (MapLatLng) |
| `ZoomLevel` | Property | Initial zoom level (1-20) |
| `MinZoomLevel` | Property | Minimum zoom allowed |
| `MaxZoomLevel` | Property | Maximum zoom allowed |

### Data Source APIs

| API | Type | Description |
|-----|------|-------------|
| `MapSource.FromFile()` | Static Method | Load from file path |
| `MapSource.FromResource()` | Static Method | Load from embedded resource |

### Marker APIs

| API | Type | Description |
|-----|------|-------------|
| `MapMarker` | Class | Marker/pin object |
| `Latitude` | Property | Marker latitude |
| `Longitude` | Property | Marker longitude |
| `IconType` | Property | Marker shape type |
| `IconFill` | Property | Marker fill color |
| `IconStroke` | Property | Marker border color |
| `IconHeight` | Property | Marker height |
| `IconWidth` | Property | Marker width |
| `MapMarkerCollection` | Class | Collection of markers |
| `MapLatLng` | Class | Coordinate class |

### Data Label APIs

| API | Type | Description |
|-----|------|-------------|
| `DataLabelSettings` | Property | Label configuration |
| `MapDataLabelSettings` | Class | Settings for labels |
| `DataLabelPath` | Property | Property to display |

## Next Steps

- **Data binding and color mapping** → [shape-layer.md](shape-layer.md)
- **Tile providers (Bing, Google)** → [tile-layer.md](tile-layer.md)
- **Adding markers and pins** → [markers.md](markers.md)
- **Bubble visualization** → [bubbles.md](bubbles.md)
- **Legends and data labels** → [legends.md](legends.md), [data-labels.md](data-labels.md)
- **User interactions (zoom, pan, selection)** → [interaction-features.md](interaction-features.md)
- **Sublayers and overlays** → [sublayers.md](sublayers.md)
