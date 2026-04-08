# Interaction Features

## Table of Contents
- [Zoom and Pan Overview](#zoom-and-pan-overview)
- [Enabling Zoom](#enabling-zoom)
- [Enabling Pan](#enabling-pan)
- [Zoom Level Configuration](#zoom-level-configuration)
- [Advanced Zoom Features](#advanced-zoom-features)
- [Shape Selection](#shape-selection)
- [Tooltips](#tooltips)
- [Best Practices](#best-practices)

## Zoom and Pan Overview

Enable interactive zoom and pan to allow users to explore map details. Users can zoom by pinching (touch devices) or scrolling the mouse wheel, and pan by dragging.

**Use zoom and pan for:**
- Detailed map exploration
- Focusing on specific regions
- Interactive data discovery
- Multi-level geographic visualization

**Applies to:** Both tile layer and shape layer

## Enabling Zoom

Enable zooming with the `EnableZooming` property on `MapZoomPanBehavior`. Default is `true`.

### Basic Zoom Setup

**Shape Layer:**

```xaml
<map:SfMaps>
    <map:SfMaps.Layer>
        <map:MapShapeLayer ShapesSource="url">
            <map:MapShapeLayer.ZoomPanBehavior>
                <map:MapZoomPanBehavior ZoomLevel="1" />
            </map:MapShapeLayer.ZoomPanBehavior>
        </map:MapShapeLayer>
    </map:SfMaps.Layer>
</map:SfMaps>
```

```csharp
SfMaps maps = new SfMaps();
MapShapeLayer layer = new MapShapeLayer();
layer.ShapesSource = MapSource.FromUri("url"); // For e.g "https://cdn.syncfusion.com/maps/map-data/world-map.json"

MapZoomPanBehavior zoomPanBehavior = new MapZoomPanBehavior
{
    ZoomLevel = 1
};
layer.ZoomPanBehavior = zoomPanBehavior;

maps.Layer = layer;
this.Content = maps;
```

**Tile Layer:**

```xaml
<map:SfMaps>
    <map:SfMaps.Layer>
        <map:MapTileLayer UrlTemplate="url">
            <map:MapTileLayer.ZoomPanBehavior>
                <map:MapZoomPanBehavior ZoomLevel="1" />
            </map:MapTileLayer.ZoomPanBehavior>
        </map:MapTileLayer>
    </map:SfMaps.Layer>
</map:SfMaps>
```

```csharp
SfMaps map = new SfMaps();
MapTileLayer tileLayer = new MapTileLayer();
tileLayer.UrlTemplate = "url"; // For e.g "https://tile.openstreetmap.org/{z}/{x}/{y}.png"

MapZoomPanBehavior zoomPanBehavior = new MapZoomPanBehavior
{
    ZoomLevel = 1,
    EnableZooming = true
};
tileLayer.ZoomPanBehavior = zoomPanBehavior;

map.Layer = tileLayer;
this.Content = map;
```

**Zoom interactions:**
- **Touch:** Pinch to zoom in/out
- **Mouse:** Scroll wheel to zoom
- **Trackpad:** Pinch gesture

## Enabling Pan

Enable panning with the `EnablePanning` property. Default is `true`. Panning allows users to navigate the map when zoomed in.

```xaml
<map:SfMaps>
    <map:SfMaps.Layer>
        <map:MapTileLayer UrlTemplate="url">
            <map:MapTileLayer.ZoomPanBehavior>
                <map:MapZoomPanBehavior ZoomLevel="2" EnablePanning="True"/>
            </map:MapTileLayer.ZoomPanBehavior>
        </map:MapTileLayer>
    </map:SfMaps.Layer>
</map:SfMaps>
```

```csharp
MapZoomPanBehavior zoomPanBehavior = new MapZoomPanBehavior
{
    ZoomLevel = 2,
    EnablePanning = true
};
tileLayer.ZoomPanBehavior = zoomPanBehavior;
```

**Pan interactions:**
- **Touch:** Drag with finger
- **Mouse:** Click and drag

## Zoom Level Configuration

### Setting Initial Zoom Level

Set the initial zoom level with the `ZoomLevel` property. Default is `1` (shows full viewport for tile layer).

```xaml
<map:MapTileLayer UrlTemplate="url">
    <map:MapTileLayer.Center>
        <map:MapLatLng Latitude="27.1751" Longitude="78.0421"/>
    </map:MapTileLayer.Center>
    <map:MapTileLayer.ZoomPanBehavior>
        <map:MapZoomPanBehavior ZoomLevel="5" />
    </map:MapTileLayer.ZoomPanBehavior>
</map:MapTileLayer>
```

```csharp
MapTileLayer tileLayer = new MapTileLayer();
tileLayer.UrlTemplate = "url"; // For e.g "https://tile.openstreetmap.org/{z}/{x}/{y}.png"
tileLayer.Center = new MapLatLng(27.1751, 78.0421);

MapZoomPanBehavior zoomPanBehavior = new MapZoomPanBehavior
{
    ZoomLevel = 5
};
tileLayer.ZoomPanBehavior = zoomPanBehavior;
```

### Min and Max Zoom Levels

Restrict zoom range with `MinZoomLevel` and `MaxZoomLevel`. Defaults: min=1, max=15.

**Note:** For `MapTileLayer`, maximum zoom level depends on the tile provider (check provider documentation).

```xaml
<map:MapTileLayer UrlTemplate="url">
    <map:MapTileLayer.Center>
        <map:MapLatLng Latitude="27.1751" Longitude="78.0421"/>
    </map:MapTileLayer.Center>
    <map:MapTileLayer.ZoomPanBehavior>
        <map:MapZoomPanBehavior ZoomLevel="5" 
                                MinZoomLevel="3"
                                MaxZoomLevel="10" />
    </map:MapTileLayer.ZoomPanBehavior>
</map:MapTileLayer>
```

```csharp
MapZoomPanBehavior zoomPanBehavior = new MapZoomPanBehavior
{
    ZoomLevel = 5,
    MinZoomLevel = 3,
    MaxZoomLevel = 10
};
tileLayer.ZoomPanBehavior = zoomPanBehavior;
```

**Use cases:**
- Prevent over-zooming into low-quality tiles
- Limit zoom-out to maintain detail visibility
- Control user navigation boundaries

## Advanced Zoom Features

### Auto Zoom by Distance (Radius)

Calculate zoom level automatically based on radius and distance type.

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <DataTemplate x:Key="MapMarkerTemplate">
            <StackLayout HorizontalOptions="StartAndExpand" VerticalOptions="Center">
                <Image Source="map.png" Scale="1" Aspect="AspectFit"
                       HeightRequest="35" WidthRequest="25" />
            </StackLayout>
        </DataTemplate>
    </ResourceDictionary>
</ContentPage.Resources>

<map:SfMaps>
    <map:SfMaps.Layer>
        <map:MapTileLayer Radius="5"
                          DistanceType="Kilometer"
                          MarkerTemplate="{StaticResource MapMarkerTemplate}"
                          UrlTemplate="url">
            
            <map:MapTileLayer.Center>
                <map:MapLatLng Latitude="38.909804" Longitude="-77.043442"/>
            </map:MapTileLayer.Center>
            
            <map:MapTileLayer.ZoomPanBehavior>
                <map:MapZoomPanBehavior ZoomLevel="1" />
            </map:MapTileLayer.ZoomPanBehavior>
            
            <map:MapTileLayer.Markers>
                <map:MapMarkerCollection>
                    <map:MapMarker Latitude="38.909804" Longitude="-77.043442" />
                </map:MapMarkerCollection>
            </map:MapTileLayer.Markers>
        </map:MapTileLayer>
    </map:SfMaps.Layer>
</map:SfMaps>
```

```csharp
MapTileLayer tileLayer = new MapTileLayer
{
    UrlTemplate = "url", // For e.g "https://tile.openstreetmap.org/{z}/{x}/{y}.png"
    Radius = 5,
    DistanceType = MapDistanceType.Kilometer,  // Kilometer, Mile, Meter
    Center = new MapLatLng(38.909804, -77.043442)
};

MapZoomPanBehavior zoomPanBehavior = new MapZoomPanBehavior { ZoomLevel = 1 };
tileLayer.ZoomPanBehavior = zoomPanBehavior;

MapMarker mapMarker = new MapMarker
{
    Latitude = 38.909804,
    Longitude = -77.043442
};
tileLayer.Markers = new MapMarkerCollection { mapMarker };
```

**Properties:**
- `Radius`: Distance value
- `DistanceType`: Kilometer (default), Mile, or Meter

**Use case:** Automatically zoom to show a specific area around a location (e.g., 5km radius around a store)

### Auto Zoom by Geo-Bounds

Calculate zoom level automatically to fit geographic boundaries.

```xaml
<map:SfMaps>
    <map:SfMaps.Layer>
        <map:MapTileLayer MarkerTemplate="{StaticResource MapMarkerTemplate}"
                          UrlTemplate="url">
            
            <map:MapTileLayer.ZoomPanBehavior>
                <map:MapZoomPanBehavior MaxZoomLevel="19" />
            </map:MapTileLayer.ZoomPanBehavior>
            
            <map:MapTileLayer.Markers>
                <map:MapMarkerCollection>
                    <map:MapMarker Latitude="38.909804" Longitude="-77.043442" />
                    <map:MapMarker Latitude="38.909148" Longitude="-77.043610" />
                </map:MapMarkerCollection>
            </map:MapTileLayer.Markers>
            
            <map:MapTileLayer.LatLngBounds>
                <map:MapLatLngBounds>
                    <map:MapLatLngBounds.Northeast>
                        <map:MapLatLng>
                            <x:Arguments>
                                <x:Double>38.909804</x:Double>
                                <x:Double>-77.043442</x:Double>
                            </x:Arguments>
                        </map:MapLatLng>
                    </map:MapLatLngBounds.Northeast>
                    <map:MapLatLngBounds.Southwest>
                        <map:MapLatLng>
                            <x:Arguments>
                                <x:Double>38.909148</x:Double>
                                <x:Double>-77.043610</x:Double>
                            </x:Arguments>
                        </map:MapLatLng>
                    </map:MapLatLngBounds.Southwest>
                </map:MapLatLngBounds>
            </map:MapTileLayer.LatLngBounds>
        </map:MapTileLayer>
    </map:SfMaps.Layer>
</map:SfMaps>
```

```csharp
MapLatLngBounds bounds = new MapLatLngBounds
{
    Northeast = new MapLatLng(38.909804, -77.043442),
    Southwest = new MapLatLng(38.909148, -77.043610)
};
tileLayer.LatLngBounds = bounds;
```

**Use case:** Automatically frame multiple markers or regions in view

**Priority:** If both `LatLngBounds` and `Radius` are set, `Radius` takes precedence.

### Double Tap Zooming

Enable zooming by double-tapping. Default is `false`.

```xaml
<map:MapTileLayer UrlTemplate="url">
    <map:MapTileLayer.ZoomPanBehavior>
        <map:MapZoomPanBehavior ZoomLevel="2" EnableDoubleTapZooming="True"/>
    </map:MapTileLayer.ZoomPanBehavior>
</map:MapTileLayer>
```

```csharp
MapZoomPanBehavior zoomPanBehavior = new MapZoomPanBehavior
{
    ZoomLevel = 2,
    EnableDoubleTapZooming = true
};
```

**Applies to:** Both tile layer and shape layer

### Zoom Animation

Enable smooth animation during zoom level changes. Default is `true`.

```xaml
<map:MapTileLayer UrlTemplate="url"
                  EnableZoomingAnimation="True" />
```

```csharp
tileLayer.EnableZoomingAnimation = true;
```

**Applies to:** Both tile layer and shape layer

### Center Animation

Enable smooth animation when center position changes. Default is `true`.

```xaml
<map:MapTileLayer UrlTemplate="url"
                  EnableCenterAnimation="True" />
```

```csharp
tileLayer.EnableCenterAnimation = true;
```

**Applies to:** Tile layer only

### ZoomLevelChanging Event

Respond to zoom level changes or cancel zooming.

```xaml
<map:MapTileLayer UrlTemplate="url"
                  ZoomLevelChanging="MapTileLayer_ZoomLevelChanging" />
```

```csharp
private void MapTileLayer_ZoomLevelChanging(object sender, ZoomLevelChangingEventArgs e)
{
    var oldLevel = e.OldZoomLevel;     // Previous zoom level
    var newLevel = e.NewZoomLevel;     // New zoom level
    
    // Cancel zooming at specific level
    if (e.OldZoomLevel == 10)
    {
        e.Cancel = true;
    }
}
```

**Event arguments:**
- `OldZoomLevel`: Previous zoom level
- `NewZoomLevel`: Target zoom level
- `Cancel`: Set to `true` to prevent zoom

## Shape Selection

Highlight shapes by tapping. Useful for user interaction and data exploration.

### Enabling Selection

```xaml
<map:MapShapeLayer x:Name="layer"
                   ShapesSource="url"
                   SelectedShapeFill="#6189ff"
                   ShapeSelected="layer_ShapeSelected" 
                   ShapeStrokeThickness="0"
                   EnableSelection="True" />
```

```csharp
public MainPage()
{
    InitializeComponent();
    layer.ShapesSource = MapSource.FromUri("url"); // For e.g "https://cdn.syncfusion.com/maps/map-data/australia.json"
    layer.EnableSelection = true;
    layer.SelectedShapeFill = Color.FromArgb("#6189ff");
}

private void layer_ShapeSelected(object sender, ShapeSelectedEventArgs e)
{
    // Handle selection event
    // Access selected shape data via e
}
```

**Note:** Selection allows only one shape at a time. Default is `false`.

### Selection Appearance

Customize selected shape appearance:

```csharp
MapShapeLayer layer = new MapShapeLayer
{
    ShapesSource = MapSource.FromUri("url"), // For e.g "https://cdn.syncfusion.com/maps/map-data/australia.json"
    DataSource = viewModel.Data,
    PrimaryValuePath = "Country",
    ShapeDataField = "STATE_NAME",
    ShapeColorValuePath = "Color",
    
    // Selection appearance
    EnableSelection = true,
    SelectedShapeFill = Color.FromRgb(26, 53, 219),
    SelectedShapeStroke = Colors.DarkGray,
    SelectedShapeStrokeThickness = 1
};
```

**Properties:**
- `SelectedShapeFill`: Background color of selected shape (if null, uses saturated color; if transparent, no visual change)
- `SelectedShapeStroke`: Border color
- `SelectedShapeStrokeThickness`: Border width

## Tooltips

Show information when users tap or hover over shapes, bubbles, or markers.

### Shape Tooltip

```xaml
<map:MapShapeLayer ShapesSource="url"
                   DataSource="{Binding Data}"
                   PrimaryValuePath="State" 
                   ShapeDataField="name" 
                   ShowShapeToolTip="True" />
```

```csharp
MapShapeLayer layer = new MapShapeLayer
{
    ShapesSource = MapSource.FromUri("url"), // For e.g "https://cdn.syncfusion.com/maps/map-data/world-map.json"
    DataSource = viewModel.Data,
    PrimaryValuePath = "State",
    ShapeDataField = "name",
    ShowShapeToolTip = true
};
```

### Bubble Tooltip

```xaml
<map:MapShapeLayer ShapesSource="url"
                   DataSource="{Binding Data}"
                   PrimaryValuePath="State" 
                   ShapeDataField="name" 
                   ShapeHoverFill="Transparent" 
                   ShapeHoverStroke="Transparent"
                   ShowBubbles="True"
                   ShowBubbleTooltip="True">
    
    <map:MapShapeLayer.BubbleSettings>
        <map:MapBubbleSettings ColorValuePath="Population" 
                               SizeValuePath="Population" 
                               Fill="DarkViolet"
                               MinSize="30"
                               MaxSize="80" />
    </map:MapShapeLayer.BubbleSettings>
</map:MapShapeLayer>
```

### Marker Tooltip

```xaml
<map:MapShapeLayer ShapesSource="url"
                   ShapeStroke="DarkGrey"
                   ShapeHoverFill="Transparent" 
                   ShapeHoverStroke="Transparent" 
                   ShowMarkerTooltip="True">
    
    <map:MapShapeLayer.Markers>
        <map:MapMarkerCollection>
            <map:MapMarker Latitude="1454.6" Longitude="36.0" 
                          IconWidth="20" IconHeight="20" IconType="Diamond" />
            <map:MapMarker Latitude="34.0479" Longitude="100.6124" 
                          IconWidth="20" IconHeight="20" IconType="Circle" />
        </map:MapMarkerCollection>
    </map:MapShapeLayer.Markers>
</map:MapShapeLayer>
```

### Tooltip Appearance

Customize tooltip styling with `MapTooltipSettings`:

```xaml
<map:MapShapeLayer ShowShapeToolTip="True">
    <map:MapShapeLayer.ShapeTooltipSettings>
        <map:MapTooltipSettings Background="#002080"
                                Duration="00:00:02"
                                Padding="2">
            <map:MapTooltipSettings.TextStyle>
                <map:MapLabelStyle FontSize="14"
                                   TextColor="White"
                                   FontAttributes="Bold" />
            </map:MapTooltipSettings.TextStyle>
        </map:MapTooltipSettings>
    </map:MapShapeLayer.ShapeTooltipSettings>
</map:MapShapeLayer>
```

```csharp
layer.ShapeTooltipSettings = new MapTooltipSettings
{
    Background = Color.FromArgb("#002080"),
    Duration = new TimeSpan(0, 0, 2),  // Display for 2 seconds
    Padding = new Thickness(2),
    TextStyle = new MapLabelStyle
    {
        FontSize = 14,
        TextColor = Colors.White,
        FontAttributes = FontAttributes.Bold
    }
};
```

**Properties:**
- `Background`: Tooltip background color
- `Padding`: Padding around text
- `Duration`: Display duration
- `TextStyle`: Font styling (size, color, attributes, family)

### Custom Tooltip Template

Use `DataTemplate` for custom tooltip layouts:

```xaml
<map:MapShapeLayer ShowShapeTooltip="True">
    <map:MapShapeLayer.ShapeTooltipTemplate>
        <DataTemplate>
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition />
                    <RowDefinition />
                    <RowDefinition />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition />
                    <ColumnDefinition />
                </Grid.ColumnDefinitions>
                
                <Image Source="flag.png" Grid.Column="0" Grid.Row="0"
                       WidthRequest="20" HeightRequest="20" />
                <Label Text="{Binding DataItem.Continent}"
                       TextColor="White" Grid.Column="1" Grid.Row="0" Padding="10" />
                <Label Grid.Row="2" Grid.ColumnSpan="2"
                       Text="{Binding DataItem.Area}" TextColor="White" />
            </Grid>
        </DataTemplate>
    </map:MapShapeLayer.ShapeTooltipTemplate>
</map:MapShapeLayer>
```

**BindingContext:** `MapTooltipInfo` with `DataItem` property
- **Shape/Bubble tooltip:** `DataItem` holds the underlying data object
- **Marker tooltip:** `DataItem` holds the `MapMarker` object

**Tooltip template properties:**
- `ShapeTooltipTemplate`: Custom template for shape tooltips
- `BubbleTooltipTemplate`: Custom template for bubble tooltips
- `MarkerTooltipTemplate`: Custom template for marker tooltips

### Tooltip Template Selector

Use `DataTemplateSelector` to apply different templates based on data:

```csharp
public class MarkerTemplateSelector : DataTemplateSelector
{
    public DataTemplate Template1 { get; set; }
    public DataTemplate Template2 { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        var marker = (CustomMarker)item;
        return marker.Name == "South africa" ? Template1 : Template2;
    }
}
```

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <DataTemplate x:Key="SouthAfricaTemplate">
            <Label Text="{Binding DataItem.Name}" TextColor="Red" />
        </DataTemplate>

        <DataTemplate x:Key="SouthAmericaTemplate">
            <Label Text="{Binding DataItem.Name}" TextColor="White" />
        </DataTemplate>

        <local:MarkerTemplateSelector x:Key="MarkerTemplateSelector"
                                      Template1="{StaticResource SouthAfricaTemplate}"
                                      Template2="{StaticResource SouthAmericaTemplate}" />
    </ResourceDictionary>
</ContentPage.Resources>

<map:MapShapeLayer ShowMarkerTooltip="True"
                   MarkerTooltipTemplate="{StaticResource MarkerTemplateSelector}">
    <map:MapShapeLayer.Markers>
        <map:MapMarkerCollection>
            <local:CustomMarker Name="South africa" Latitude="20.5595" Longitude="22.9375" />
            <local:CustomMarker Name="South America" Latitude="195.4915" Longitude="-50.7832" />
        </map:MapMarkerCollection>
    </map:MapShapeLayer.Markers>
</map:MapShapeLayer>
```

## Best Practices

### 1. Set Appropriate Zoom Constraints

```csharp
// Restrict zoom for tile quality
zoomPanBehavior.MinZoomLevel = 2;   // Prevent excessive zoom-out
zoomPanBehavior.MaxZoomLevel = 15;  // Tile provider limit

// For shape layers with limited detail
zoomPanBehavior.MaxZoomLevel = 10;
```

### 2. Use Auto Zoom for Dynamic Data

```csharp
// Auto-fit markers
var bounds = CalculateBounds(markerList);
tileLayer.LatLngBounds = bounds;

// Or use radius for coverage area
tileLayer.Radius = coverageRadius;
tileLayer.DistanceType = MapDistanceType.Kilometer;
```

### 3. Disable Interactions When Not Needed

```csharp
// Read-only map
zoomPanBehavior.EnableZooming = false;
zoomPanBehavior.EnablePanning = false;
layer.EnableSelection = false;
```

### 4. Combine Selection with Tooltips

```csharp
// Show details on selection
layer.EnableSelection = true;
layer.ShowShapeToolTip = true;
layer.SelectedShapeFill = Colors.LightBlue;

layer.ShapeSelected += (s, e) =>
{
    // Update UI or load detailed data
    var selectedData = e.SelectedData;
    // ...
};
```

### 5. Optimize Tooltip Performance

```csharp
// Use simpler tooltips for many shapes
layer.ShapeTooltipSettings = new MapTooltipSettings
{
    Duration = new TimeSpan(0, 0, 1)  // Short duration
};

// Avoid complex DataTemplates for large datasets
```

### 6. Control Zoom Animation

```csharp
// Disable for better performance on low-end devices
tileLayer.EnableZoomingAnimation = false;
tileLayer.EnableCenterAnimation = false;

// Enable for better UX on capable devices
tileLayer.EnableZoomingAnimation = true;
```

### 7. Validate Zoom Changes

```csharp
layer.ZoomLevelChanging += (s, e) =>
{
    // Prevent zoom beyond data availability
    if (e.NewZoomLevel > maxDataZoomLevel)
    {
        e.Cancel = true;
        ShowMessage("No data available at this zoom level");
    }
};
```

## Common Use Cases

### Use Case 1: Store Locator with Auto Zoom

```csharp
// Show store location with 5km coverage
tileLayer.Center = new MapLatLng(storeLatitude, storeLongitude);
tileLayer.Radius = 5;
tileLayer.DistanceType = MapDistanceType.Kilometer;

// Add marker
tileLayer.Markers = new MapMarkerCollection
{
    new MapMarker { Latitude = storeLatitude, Longitude = storeLongitude }
};
tileLayer.ShowMarkerTooltip = true;
```

### Use Case 2: Interactive Region Explorer

```csharp
// Enable exploration
layer.EnableSelection = true;
layer.ShowShapeToolTip = true;
layer.ZoomPanBehavior = new MapZoomPanBehavior
{
    EnableZooming = true,
    EnablePanning = true,
    MinZoomLevel = 1,
    MaxZoomLevel = 8
};

// Handle selection
layer.ShapeSelected += (s, e) =>
{
    NavigateToRegionDetails(e.SelectedData);
};
```

### Use Case 3: Delivery Route with Restricted Pan

```csharp
// Set bounds to delivery area
tileLayer.LatLngBounds = new MapLatLngBounds
{
    Northeast = deliveryArea.NorthEast,
    Southwest = deliveryArea.SouthWest
};

// Allow zoom but limit pan
tileLayer.ZoomPanBehavior = new MapZoomPanBehavior
{
    EnableZooming = true,
    EnablePanning = true,
    MinZoomLevel = 10,  // Stay local
    MaxZoomLevel = 16
};
```

## Key Takeaways

✅ **Core Syncfusion Maps APIs:**
- `MapZoomPanBehavior` - Configuration class for zoom and pan interactions with `ZoomLevel`, `EnableZooming`, `EnablePanning`
- `MinZoomLevel` / `MaxZoomLevel` - Properties to restrict zoom range (defaults: 1 / 15)
- `Radius` + `DistanceType` - Auto-zoom by distance (Kilometer, Mile, Meter)
- `LatLngBounds` (Northeast/Southwest) - Auto-zoom to fit geographic boundaries
- `EnableDoubleTapZooming` - Property to enable zoom via double-tap gesture

✅ **Essential Concepts:**
- Zoom and pan enable interactive map exploration on both tile and shape layers
- Zoom interactions: pinch gesture (touch), scroll wheel (mouse), trackpad pinch
- Pan interactions: drag gesture (touch and mouse) for navigating when zoomed
- Auto-zoom features calculate optimal zoom level based on radius or geographic bounds
- Selection and tooltips enhance interactivity by responding to user taps/hovers

✅ **Implementation Patterns:**
- Basic interaction: Create `MapZoomPanBehavior`, set `ZoomLevel`, attach to layer's `ZoomPanBehavior` property
- Restrict zoom: Set `MinZoomLevel`/`MaxZoomLevel` to prevent over-zoom or excessive zoom-out
- Auto-fit markers: Use `LatLngBounds` with Northeast/Southwest coordinates to frame multiple points
- Coverage area: Use `Radius` with `DistanceType` to show specific distance around a location
- Shape interaction: Enable `EnableSelection` + `ShowShapeToolTip` for interactive region exploration

✅ **Best Practices:**
- Set appropriate zoom constraints based on tile provider limits and data detail level
- Use auto-zoom features (Radius, LatLngBounds) for dynamic content rather than fixed zoom levels
- Disable interactions (`EnableZooming=false`, `EnablePanning=false`) for read-only static maps
- Combine selection with tooltips for rich user interaction and data discovery
- Disable animations (`EnableZoomingAnimation=false`) on low-end devices for better performance
- Use `ZoomLevelChanging` event to validate zoom changes and prevent navigation beyond data availability

## Next Steps

- **Add markers for interaction** → [markers.md](markers.md)
- **Implement shape data binding** → [shape-layer.md](shape-layer.md)

