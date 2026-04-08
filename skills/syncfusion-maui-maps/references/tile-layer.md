# Tile Layer in .NET MAUI Maps (SfMaps)

The tile layer renders the tiles returned from web map tile services such as Azure Maps, Bing Maps, OpenStreetMaps, Google Maps, TomTom, etc.

## Overview

The MapTileLayer displays map tiles from web map tile service providers. It needs to be added to the Layer property in SfMaps control. Tiles are dynamically loaded based on the current Center coordinates and zoom level.

**Use tile layer when:**
- Displaying street maps or satellite imagery
- Need detailed basemaps for context
- Overlaying markers on standard map views
- Integrating with existing tile services
- Real-time or frequently updated map data

**Advantages:**
- Rich, detailed cartography
- Regular updates from providers
- No need to maintain map data locally
- Multiple map styles (road, satellite, terrain)
- Dynamic tile loading for performance

**Considerations:**
- Requires internet connectivity
- Provider terms of service and rate limits
- Some providers require API keys/subscriptions
- Limited offline capabilities without custom implementation

## Setting URL Template

The URL of the tile providers must be set in the MapTileLayer.UrlTemplate property. The UrlTemplate property accepts the URL in WMTS (Web Map Tile Service) format i.e. {z} — zoom level, {x} and {y} — tile coordinates. This URL might vary slightly depending on the providers.

**Placeholders:**
- `{z}`: Zoom level (typically 0-20)
- `{x}`: Tile X coordinate
- `{y}`: Tile Y coordinate

The SfMaps control will replace the {z}, {x}, and {y} internally based on the current Center and zoom level.

> **Note:** Some of the providers may need a subscription key. Please include them in the UrlTemplate itself, as mentioned in the above example. Please note that the format may vary between each map provider. You can check the exact URL format needed for the providers on their official websites.

## Adding OpenStreetMap

The OpenStreetMap is one of the tile/image providers which can be used free of cost. It returns map tiles for the requested coordinates for every request. The URL format of the OSM map provider is shown in the below code sample.

> **Note:** Though the OpenStreetMap is free of cost, we recommend you check the licensing terms and conditions once before using it.

### Key APIs Used
- SfMaps - Main maps control
- MapTileLayer - Tile layer implementation
- UrlTemplate - Tile service URL pattern

### Basic OSM Implementation

**XAML:**
```xml
<maps:SfMaps>
    <maps:SfMaps.Layer>
        <maps:MapTileLayer UrlTemplate="url" />
    </maps:SfMaps.Layer>
</maps:SfMaps>
```

**C#:**
```csharp
using Syncfusion.Maui.Maps;

SfMaps map = new SfMaps();
MapTileLayer tileLayer = new MapTileLayer();
tileLayer.UrlTemplate = "url"; // For e.g  "https://tile.openstreetmap.org/{z}/{x}/{y}.png"
map.Layer = tileLayer;
this.Content = map;
```

### OSM Licensing Requirements

**Important:** OpenStreetMap is free but has usage requirements:
- **Attribution required:** Display "© OpenStreetMap contributors"
- **Fair use policy:** Respect tile usage policies
- **Heavy usage:** Consider running your own tile server or use commercial alternatives

## Adding Bing Maps

An additional step is required for the Bing maps. The format of the required URL varies from the other tile services. Hence, we have added a top-level GetBingUrl method which returns the URL in the required format. 

The subscription key is needed for bing maps. You can create an API key by bing map official page and append this key to the bing map URL before passing it to the GetBingUrl method. You can use the URL returned from this method to pass it to the UrlTemplate property.

Some of the providers provide different map types. For example, Bing Maps provide map types like Road, Aerial, AerialWithLabels etc. These types too can be passed in the UrlTemplate itself, as shown in the following example. You can check the official websites of the tile providers to know about the available types and the code for them.

### Key APIs Used
- MapTileLayer.GetBingUrl - Static method to convert Bing metadata URL to tile URL template
- MapTileLayer.UrlTemplate - Tile service URL pattern

### Implementation

**C#:**
```csharp
using Syncfusion.Maui.Maps;

public MainPage()
{
    InitializeComponent();
    SfMaps map = new SfMaps();
    MapTileLayer tileLayer = new MapTileLayer();
    this.GenerateBing(tileLayer);
    map.Layer = tileLayer;
    this.Content = map;
}

private async Task GenerateBing(MapTileLayer tileLayer)
{
    tileLayer.UrlTemplate = await MapTileLayer.GetBingUrl("https://dev.virtualearth.net/REST/V1/Imagery/Metadata/RoadOnDemand?output=json&uriScheme=https&include=ImageryProviders&key=subscription_key") + "?name=bingName";
}
```

### Bing Maps Types

Change the imagery set in the metadata URL to access different map types:

**Available Bing Map Types:**

| Map Type | Imagery Set Value | Description |
|----------|------------------|-------------|
| Road | `RoadOnDemand` | Standard road map (default) |
| Aerial | `Aerial` | Satellite imagery |
| Aerial with Labels | `AerialWithLabelsOnDemand` | Satellite with street labels |

**Example - Different Map Types:**

```csharp
using Syncfusion.Maui.Maps;

// Road map (default)
string roadUrl = await MapTileLayer.GetBingUrl(
    "https://dev.virtualearth.net/REST/V1/Imagery/Metadata/RoadOnDemand?output=json&uriScheme=https&include=ImageryProviders&key=YOUR_KEY");

// Aerial (satellite) imagery
string aerialUrl = await MapTileLayer.GetBingUrl(
    "https://dev.virtualearth.net/REST/V1/Imagery/Metadata/Aerial?output=json&uriScheme=https&include=ImageryProviders&key=YOUR_KEY");

// Aerial with labels
string aerialLabelsUrl = await MapTileLayer.GetBingUrl(
    "https://dev.virtualearth.net/REST/V1/Imagery/Metadata/AerialWithLabelsOnDemand?output=json&uriScheme=https&include=ImageryProviders&key=YOUR_KEY");
```

## Other Map Tile Providers

The tile layer is not limited or specific to any tile providers mentioned here. It supports requesting tiles from any tile providers using the unique URL for respective tile providers and renders them.

For other map providers like TomTom, MapBox, etc., you can check the respective official websites and provide the URL in the format mentioned in the [Setting URL template](#setting-url-template) section.

### TomTom Maps

Below is the example of adding TomTom map. You can get the TomTom API key from tomtom official page.

**Key APIs Used:**
- MapTileLayer - Tile layer implementation
- UrlTemplate - Tile service URL pattern

**C#:**
```csharp
using Syncfusion.Maui.Maps;

SfMaps map = new SfMaps();
MapTileLayer tileLayer = new MapTileLayer();
tileLayer.UrlTemplate = "https://api.tomtom.com/map/1/tile/basic/main/{z}/{x}/{y}.png?key=subscription_key";
map.Layer = tileLayer;
this.Content = map;
```

### Other Providers

**MapBox:**
- Available styles: streets-v11, satellite-v9, outdoors-v11, dark-v10, light-v10

**Google Maps:**
- Requires API key and has specific terms of service
- Review Google Maps Platform terms and pricing before use
- URL Format varies by map type (road, satellite, hybrid)

## Changing the Center Latitude and Longitude

The center position by setting the MapTileLayer.Center property. It represents the center position of the map layer. Based on the size of the SfMaps control, Center and zoom level, the number of initial tiles needed in the viewport alone will be rendered. 

**Default value:** `MapLatLng(0.0, 0.0)`

### Key APIs Used
- MapTileLayer.Center - Gets or sets the center coordinate
- MapLatLng - Represents geographical coordinates

### Setting Map Center

**XAML:**
```xml
<map:SfMaps>
    <map:SfMaps.Layer>
        <map:MapTileLayer UrlTemplate="url">
            <map:MapTileLayer.Center>
                <map:MapLatLng Latitude="27.175014"
                               Longitude="78.042152">
                </map:MapLatLng>
            </map:MapTileLayer.Center>
        </map:MapTileLayer>
    </map:SfMaps.Layer>
</map:SfMaps>
```

**C#:**
```csharp
using Syncfusion.Maui.Maps;

    SfMaps map = new SfMaps();
    MapTileLayer tileLayer = new MapTileLayer();
    tileLayer.UrlTemplate = "url";// For e.g  "https://tile.openstreetmap.org/{z}/{x}/{y}.png"
    tileLayer.Center = new MapLatLng(27.175014, 78.042152);
    map.Layer = tileLayer;
    this.Content = map;
```

### MapLatLng Properties

| Property | Type | Range | Description |
|----------|------|-------|-------------|
| `Latitude` | double | -90 to 90 | North-South position (positive = North) |
| `Longitude` | double | -180 to 180 | East-West position (positive = East) |

### Common Locations Examples

```csharp
using Syncfusion.Maui.Maps;

// New York City
tileLayer.Center = new MapLatLng(40.7128, -74.0060);

// London
tileLayer.Center = new MapLatLng(51.5074, -0.1278);

// Tokyo
tileLayer.Center = new MapLatLng(35.6762, 139.6503);

// Sydney
tileLayer.Center = new MapLatLng(-33.8688, 151.2093);
```

## Cache Tile Images in Application Memory

The CanCacheTiles property is used to decide whether the tile images should be cached in application memory or not. The default value of the CanCacheTiles is `false`.

While enabling the CanCacheTiles, we need to set the tile server name to maintain the folder to store cache tiles in the MapTileLayer.UrlTemplate property. The default tile server name to store the tile cache is OSM. 

Here, you can replace the serverName as per your wish.

### Key APIs Used
- `MapTileLayer.CanCacheTiles` - Enables/disables tile caching

**XAML:**
```xml
<map:SfMaps>
    <map:SfMaps.Layer>
        <map:MapTileLayer UrlTemplate="http://api.tomtom.com/map/1/tile/basic/main/{z}/{x}/{y}.png?key=subscription_key?name=tomtom"
                          CanCacheTiles="True">
        </map:MapTileLayer>
    </map:SfMaps.Layer>
</map:SfMaps>
```

**C#:**
```csharp
using Syncfusion.Maui.Maps;

SfMaps map = new SfMaps();
MapTileLayer tileLayer = new MapTileLayer();
tileLayer.UrlTemplate = "http://api.tomtom.com/map/1/tile/basic/main/{z}/{x}/{y}.png?key=subscription_key?name=tomtom";
tileLayer.CanCacheTiles = true;
map.Layer = tileLayer;
this.Content = map;
```

## Clear Cached Tile Images from Application Memory

The DeleteTilesFromCache method is used to clear the cached tile images from the application cache memory.

### Key APIs Used
- `MapTileLayer.DeleteTilesFromCache` - Method to clear tile cache

**XAML:**
```xml
<maps:SfMaps>
    <maps:SfMaps.Layer>
        <maps:MapTileLayer x:Name="tileLayer" 
                          UrlTemplate="url" />
    </maps:SfMaps.Layer>
</maps:SfMaps>
```

**C#:**
```csharp
using Syncfusion.Maui.Maps;

tileLayer.DeleteTilesFromCache();
```

## Markers

You can add markers in the tile layer. The procedure is very similar to the shape layer. 

## CenterChanged Event

The CenterChanged event is triggered while zooming and panning the maps.

### Key APIs Used
- MapTileLayer.CenterChanged - Event triggered during zoom and pan
- CenterChangedEventArgs - Event arguments containing viewport bounds

**XAML:**
```xml
    <maps:SfMaps>
    <maps:SfMaps.Layer>
        <maps:MapTileLayer CenterChanged="MapTileLayer_CenterChanged" 
                          UrlTemplate="url" />
    </maps:SfMaps.Layer>
</maps:SfMaps>
```

**C#:**
```csharp
using Syncfusion.Maui.Maps;

private void MapTileLayer_CenterChanged(object sender, CenterChangedEventArgs e)
{
    var center = e.Center;
    var topLeft = e.TopLeft;
    var topRight = e.TopRight;
    var bottomLeft = e.BottomLeft;
    var bottomRight = e.BottomRight;
}
```

## MapTileLayer API Summary

### Properties

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `UrlTemplate` | string | Tile service URL with {z}/{x}/{y} placeholders | Required |
| `Center` | MapLatLng | Map center coordinates | MapLatLng(0, 0) |
| `CanCacheTiles` | bool | Enable/disable tile caching | false |
| `Markers` | MapMarkerCollection | Markers to display on the layer | Empty |

### Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `GetBingUrl` | Task\<string\> | Static method to convert Bing metadata URL to tile URL template |
| `DeleteTilesFromCache | void | Clears cached tile images from application memory |

### Events

| Event | Event Args | Description |
|-------|------------|-------------|
| `CenterChanged` | CenterChangedEventArgs | Triggered while zooming and panning the maps |

## Best Practices

### 1. API Key Security

**Avoid hardcoding API keys in source code:**

```csharp
using Syncfusion.Maui.Maps;

// ❌ BAD - Hardcoded API key
tileLayer.UrlTemplate = "https://api.provider.com/tiles/{z}/{x}/{y}?key=pk.abc123xyz";

// ✅ GOOD - Use secure storage
string apiKey = await SecureStorage.GetAsync("map_api_key");
tileLayer.UrlTemplate = $"https://api.provider.com/tiles/{{z}}/{{x}}/{{y}}?key={apiKey}";
```

**Use .NET MAUI Secure Storage for API keys:**
```csharp
using Microsoft.Maui.Storage;
using Syncfusion.Maui.Maps;

// Store key securely (typically done once during setup)
await SecureStorage.SetAsync("map_api_key", "pk.abc123xyz");

// Retrieve key when needed
string apiKey = await SecureStorage.GetAsync("map_api_key");
MapTileLayer tileLayer = new MapTileLayer
{
    UrlTemplate = $"https://tile.provider.com/{{z}}/{{x}}/{{y}}.png?key={apiKey}"
};
```

### 2. Handle Tile Loading Failures

```csharp
using Syncfusion.Maui.Maps;

public async Task<MapTileLayer> CreateTileLayerAsync()
{
    MapTileLayer tileLayer = new MapTileLayer();
    
    try
    {
        string bingMetadataUrl = "https://dev.virtualearth.net/REST/V1/Imagery/Metadata/RoadOnDemand?output=json&uriScheme=https&include=ImageryProviders&key=YOUR_KEY";
        string urlTemplate = await MapTileLayer.GetBingUrl(bingMetadataUrl);
        tileLayer.UrlTemplate = urlTemplate;
    }
    catch (Exception ex)
    {
        // Fallback to OpenStreetMap
        tileLayer.UrlTemplate = "url"; // For e.g  "https://tile.openstreetmap.org/{z}/{x}/{y}.png"
        await Application.Current.MainPage.DisplayAlert("Warning", 
            "Could not load Bing Maps, using OpenStreetMap", "OK");
    }
    
    return tileLayer;
}
```

### 3. Respect Rate Limits and Usage Policies

Most tile providers have rate limits and usage policies:
- **Cache tiles** using `CanCacheTiles` property if allowed by provider terms
- **Monitor usage** through provider dashboards
- **Consider paid tiers** for high-traffic applications
- **Review terms of service** for each tile provider

### 4. Display Attribution

Display proper attribution for map data as required by most tile providers:

**XAML:**
```xml
<Grid>
    <map:SfMaps>
        <map:SfMaps.Layer>
            <map:MapTileLayer UrlTemplate="url" />
        </map:SfMaps.Layer>
    </map:SfMaps>
    
    <Label Text="© OpenStreetMap contributors"
           VerticalOptions="End"
           HorizontalOptions="End"
           Margin="8"
           FontSize="10"
           TextColor="Gray"
           BackgroundColor="#80FFFFFF"
           Padding="4,2" />
</Grid>
```

## Troubleshooting

### Issue: Gray Tiles or Missing Tiles

**Common causes and solutions:**
1. ✅ **Verify internet connectivity** - Tile layers require active internet connection
2. ✅ **Test tile URL** - Replace {z}, {x}, {y} with actual values (e.g., 10/163/395) and test in browser
3. ✅ **Check API key** - Verify API key is valid and active with the provider
4. ✅ **Verify subscription** - Ensure your account/subscription is active
5. ✅ **Check rate limits** - You may have exceeded the provider's rate limit or quota
6. ✅ **Review provider status** - Check the tile provider's service status page

### Issue: Bing Maps Not Loading

**Solutions:**
1. ✅ Verify your Bing Maps API key is correct and active
2. ✅ Ensure you're using `MapTileLayer.GetBingUrl` method (not direct URL)
3. ✅ Check Bing Maps Dev Center for key status and permissions
4. ✅ Verify the key has Maps API enabled
5. ✅ Confirm internet connectivity is available

### Issue: Tiles Loading Slowly

**Performance optimization:**
1. ✅ Use provider's CDN or regional servers for better performance
2. ✅ Check network speed and connectivity quality
3. ✅ Enable tile caching with CanCacheTiles property
4. ✅ Start with a lower zoom level (fewer tiles to load initially)
5. ✅ Consider providers with faster tile servers

## Key Takeaways

✅ **Core Syncfusion Maps APIs:**
- `MapTileLayer` - Displays map tiles from web services (OpenStreetMap, Azure Maps, Bing Maps, TomTom)
- `UrlTemplate` - Tile URL pattern with `{z}/{x}/{y}` placeholders for dynamic tile loading
- `MapTileLayer.GetBingUrl()` - Static method to convert Bing metadata URL to tile URL template
- `Center` (MapLatLng) - Sets map center coordinates for initial view
- `CanCacheTiles` / `DeleteTilesFromCache()` - Properties/methods for tile caching management

✅ **Essential Concepts:**
- Tile layers dynamically load map tiles based on zoom level and center coordinates
- Different providers (OSM, Bing, Azure, TomTom, MapBox) have different URL formats and requirements
- Tile caching improves performance by storing tiles in application memory
- API keys are required for most commercial tile providers (Bing, TomTom, MapBox)

✅ **Implementation Patterns:**
- For OpenStreetMap: Set `UrlTemplate` directly with tile URL pattern
- For Bing Maps: Use `GetBingUrl()` with metadata URL, then set returned template
- For other providers: Construct URL with provider-specific format and API key
- Enable caching with `CanCacheTiles=true` and server name parameter in URL

✅ **Best Practices:**
- Store API keys securely using .NET MAUI SecureStorage, never hardcode them
- Display proper attribution as required by tile providers (especially OpenStreetMap)
- Enable tile caching for better performance and reduced network usage
- Test tile URLs in browser by replacing placeholders with actual values
- Handle tile loading failures gracefully with fallback providers
- Respect provider rate limits and usage policies to avoid service interruption
