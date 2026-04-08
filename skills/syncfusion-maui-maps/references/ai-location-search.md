# AI-Driven Smart Location Search

Integrate Azure OpenAI with Syncfusion .NET MAUI Maps (SfMaps) to enable intelligent, AI-powered location search functionality. This advanced feature allows users to search for places using natural language queries (e.g., "Hospitals in New York") and automatically visualizes relevant locations on the map with AI-generated images and details.

## Prerequisites

**Azure OpenAI Requirements:**
- Active Azure OpenAI service subscription
- Configured deployment (GPT-4O model recommended for text generation)
- Optional: DALL-E deployment for AI-generated location images
- API endpoint and authentication key

**NuGet Packages Required:**
```xml
<!-- .NET 9 compatible versions -->
<PackageReference Include="Azure.AI.OpenAI" Version="1.0.0-beta.12" />
<PackageReference Include="Syncfusion.Maui.Maps" Version="27.*" />
<PackageReference Include="Syncfusion.Maui.Inputs" Version="27.*" />
<PackageReference Include="Newtonsoft.Json" Version="13.0.3" />
```

**Namespace Declarations:**
```csharp
using Azure;
using Azure.AI.OpenAI;
using Syncfusion.Maui.Maps;
using Syncfusion.Maui.Inputs;
using Newtonsoft.Json.Linq;
using System.Collections.ObjectModel;
using Microsoft.Maui.Controls;
using Microsoft.Maui.Graphics;
```

## Use Cases

**AI location search is ideal for:**
- Natural language location queries ("Find coffee shops near Central Park")
- Smart multi-location discovery (multiple results from single query)
- Category-based search ("Museums in London")
- Regional and city-level queries ("Tourist attractions in Tokyo")
- Automated geocoding with rich metadata
- Enhanced user search experience with visual feedback

## Key APIs Used

### Azure OpenAI APIs

| API Class | Purpose |
|-----------|---------|
| `OpenAIClient` | Main client for Azure OpenAI service connection |
| `AzureKeyCredential` | Authenticates API requests with Azure key |
| `ChatCompletionsOptions` | Configures chat completion requests |
| `ChatRequestSystemMessage` | System-level instructions for AI model |
| `ChatRequestUserMessage` | User query to AI model |
| `ImageGenerationOptions` | Configuration for DALL-E image generation |

### Syncfusion Maps APIs

| API | Type | Purpose |
|-----|------|---------|
| `SfMaps` | Control | Main Maps control container |
| `MapTileLayer` | Layer | Tile-based map layer with OpenStreetMap support |
| `MapMarker` | Model | Base class for map markers with coordinates |
| `MapLatLng` | Model | Geographic coordinates (latitude/longitude) |
| `MapZoomPanBehavior` | Behavior | Controls zoom and pan interactions |
| `MapTooltipSettings` | Settings | Customizes marker tooltip appearance |
| `Markers` | Property | Collection of markers to display on map |
| `Center` | Property | Geographic center point of map view |
| `ShowMarkerTooltip` | Property | Enables/disables marker tooltips |
| `MarkerTemplate` | Property | DataTemplate for marker icon customization |
| `MarkerTooltipTemplate` | Property | DataTemplate for tooltip content |
| `EnableCenterAnimation` | Property | Animates map re-centering |
| `CanCacheTiles` | Property | Enables tile caching for performance |
| `UrlTemplate` | Property | OSM tile service URL pattern |

### Syncfusion Inputs APIs

| API | Type | Purpose |
|-----|------|---------|
| `SfAutocomplete` | Control | Search input control for user queries |
| `Text` | Property | Current text value in autocomplete |
| `IsClearButtonVisible` | Property | Shows/hides clear button |

For detailed guidance, please refer to our [documentation](https://help.syncfusion.com/maui/maps/ai-driven-smart-location-search)