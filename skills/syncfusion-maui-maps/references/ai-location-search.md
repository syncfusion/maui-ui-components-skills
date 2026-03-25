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

## Implementation Guide

### Step 1: Configure Azure OpenAI Service

Create a service class to manage Azure OpenAI connections and requests:

```csharp
using Azure;
using Azure.AI.OpenAI;

namespace YourApp.Services
{
    internal class AzureOpenAIService
    {
        // Azure OpenAI Configuration
        const string endpoint = "https://{YOUR_END_POINT}.openai.azure.com";
        const string deploymentName = "GPT-4O";
        const string imageDeploymentName = "DALL-E";
        string key = "YOUR_API_KEY";
        
        // OpenAI Client Properties
        OpenAIClient? client;
        ChatCompletionsOptions? chatCompletions;
        
        public OpenAIClient? Client => client;
        public string? ImageDeploymentName => imageDeploymentName;
        
        internal AzureOpenAIService()
        {
            // Initialize client connection
            this.client = new OpenAIClient(new Uri(endpoint), new AzureKeyCredential(key));
            
            // Configure chat completion options
            this.chatCompletions = new ChatCompletionsOptions()
            {
                DeploymentName = deploymentName,
                MaxTokens = 4000,
                Temperature = 0.7f,
                NucleusSamplingFactor = 0.95f,
                FrequencyPenalty = 0,
                PresencePenalty = 0
            };
        }
    }
}
```

**Key Configuration Parameters:**
- `endpoint`: Your Azure OpenAI resource endpoint URL
- `key`: Azure OpenAI API authentication key
- `deploymentName`: GPT-4O model deployment name for text generation
- `imageDeploymentName`: DALL-E model deployment for image generation
- `MaxTokens`: Maximum response length (4000 recommended)
- `Temperature`: Creativity level (0.7 = balanced)
- `NucleusSamplingFactor`: Diversity of responses (0.95 recommended)

### Step 2: Implement AI Query Methods

Add methods to the `AzureOpenAIService` class to retrieve location data and generate images:

```csharp
public async Task<string> GetResultsFromAI(string userPrompt)
{
    if (this.client != null && this.chatCompletions != null)
    {
        // Clear previous messages
        this.chatCompletions.Messages.Clear();
        
        // Add system message to set AI behavior
        this.chatCompletions.Messages.Add(
            new ChatRequestSystemMessage("You are a predictive analytics assistant specializing in geographic data.")
        );
        
        // Add user's location query
        this.chatCompletions.Messages.Add(
            new ChatRequestUserMessage(userPrompt)
        );
        
        try
        {
            // Call Azure OpenAI API
            var response = await client.GetChatCompletionsAsync(this.chatCompletions);
            
            // Extract and return the response content
            return response.Value.Choices[0].Message.Content;
        }
        catch (RequestFailedException ex)
        {
            System.Diagnostics.Debug.WriteLine($"Azure OpenAI API Error: {ex.Message}");
            return string.Empty;
        }
        catch (Exception ex)
        {
            System.Diagnostics.Debug.WriteLine($"Error getting AI results: {ex.Message}");
            return string.Empty;
        }
    }
    
    return string.Empty;
}

public async Task<Uri> GetImageFromAI(string? locationName)
{
    if (client == null || string.IsNullOrEmpty(locationName))
    {
        return new Uri("https://via.placeholder.com/1024x1024");
    }
    
    try
    {
        var imageGenerations = await client.GetImageGenerationsAsync(
            new ImageGenerationOptions()
            {
                Prompt = $"Share the {locationName} image. If the image is not available share common image based on the location",
                Size = ImageSize.Size1024x1024,
                Quality = ImageGenerationQuality.Standard,
                DeploymentName = imageDeploymentName,
                ResponseFormat = ImageGenerationResponseFormat.Url
            }
        );
        
        var imageUrl = imageGenerations.Value.Data[0].Url;
        return new Uri(imageUrl.ToString());
    }
    catch (Exception ex)
    {
        System.Diagnostics.Debug.WriteLine($"Error generating image: {ex.Message}");
        return new Uri("https://via.placeholder.com/1024x1024");
    }
}
```

**Method Purpose:**
- **`GetResultsFromAI`**: Queries Azure OpenAI GPT-4O model to extract geographic data from natural language
- **`GetImageFromAI`**: Generates location-specific images using DALL-E for enhanced visual experience

**Error Handling:**
- Catches `RequestFailedException` for API-specific errors
- Returns empty strings/placeholder URLs on failures
- Logs errors for debugging purposes

### Step 3: Create Custom Marker Model

Define a custom marker model that extends `MapMarker` to include additional metadata for AI-powered location display:

```csharp
using Syncfusion.Maui.Maps;

namespace YourApp.Models
{
    public class CustomMarker : MapMarker
    {
        public string? Name { get; set; }
        public string? Details { get; set; }
        public Uri? Image { get; set; }
        public string? Address { get; set; }
        public string? ImageName { get; set; }
    }
}
```

**Property Details:**

| Property | Type | Inherited | Purpose |
|----------|------|-----------|---------|
| `Latitude` | double | Yes (MapMarker) | Geographic latitude coordinate |
| `Longitude` | double | Yes (MapMarker) | Geographic longitude coordinate |
| `Name` | string? | No | Display name for location |
| `Details` | string? | No | Category or description text |
| `Image` | Uri? | No | AI-generated image URL |
| `Address` | string? | No | Full street address (null for broad locations) |
| `ImageName` | string? | No | Optional local image resource name |

**Usage Pattern:**
- For **specific locations** (hospitals, restaurants, landmarks): Include `Name`, `Details`, `Address`, coordinates
- For **broad locations** (cities, regions, countries): Include `Name`, `Details`, coordinates; set `Address` to null

### Step 4: Design Maps and Search UI with Syncfusion Controls

Create the complete UI using `SfMaps` for visualization and `SfAutocomplete` for search input:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:maps="clr-namespace:Syncfusion.Maui.Maps;assembly=Syncfusion.Maui.Maps"
             xmlns:editors="clr-namespace:Syncfusion.Maui.Inputs;assembly=Syncfusion.Maui.Inputs"
             xmlns:local="clr-namespace:YourApp"
             x:Class="YourApp.Views.AILocationSearchPage"
             Title="AI Location Search">
    
    <Grid>
        <!-- AI-Powered Location Search Input -->
        <HorizontalStackLayout VerticalOptions="Start" 
                              HorizontalOptions="Start" 
                              WidthRequest="350" 
                              Margin="10"
                              ZIndex="1">
            
            <!-- Syncfusion Autocomplete for Natural Language Queries -->
            <editors:SfAutocomplete x:Name="autoComplete"
                                   IsClearButtonVisible="False"
                                   WidthRequest="350"
                                   HeightRequest="50"
                                   Placeholder="Search locations (e.g., Hospitals in New York)"
                                   Text="Hospitals in New York"
                                   TextColor="Black"
                                   PlaceholderColor="Gray"
                                   FontSize="14" />
            
            <!-- Search Button -->
            <Button x:Name="searchButton" 
                    Text="🔍"
                    Margin="-55,0,0,0"
                    BackgroundColor="Transparent"
                    BorderColor="Transparent"
                    FontSize="20"
                    HeightRequest="50"
                    WidthRequest="50"
                    Clicked="OnSearchClicked" />
        </HorizontalStackLayout>
        
        <!-- Loading Indicator -->
        <ActivityIndicator x:Name="busyIndicator"
                          IsRunning="False"
                          IsVisible="False"
                          Color="Blue"
                          VerticalOptions="Center"
                          HorizontalOptions="Center"
                          ZIndex="2" />
        
        <!-- Syncfusion .NET MAUI Maps Control -->
        <maps:SfMaps x:Name="maps">
            <maps:SfMaps.Layer>
                <!-- MapTileLayer: OpenStreetMap-based tile layer -->
                <maps:MapTileLayer x:Name="layer"
                                  UrlTemplate="https://tile.openstreetmap.org/{z}/{x}/{y}.png"
                                  CanCacheTiles="True"
                                  ShowMarkerTooltip="True"
                                  MarkerTemplate="{StaticResource MarkerTemplate}"
                                  MarkerTooltipTemplate="{StaticResource MarkerTemplateSelector}">
                    
                    <!-- Initial Map Center (Continental US) -->
                    <maps:MapTileLayer.Center>
                        <maps:MapLatLng Latitude="37.0902" 
                                       Longitude="-95.7129" />
                    </maps:MapTileLayer.Center>
                    
                    <!-- Zoom and Pan Behavior Configuration -->
                    <maps:MapTileLayer.ZoomPanBehavior>
                        <maps:MapZoomPanBehavior x:Name="zoomPanBehavior"
                                                ZoomLevel="4"
                                                MinZoomLevel="4"
                                                MaxZoomLevel="18"
                                                EnableDoubleTapZooming="True"
                                                EnablePinchZooming="True" />
                    </maps:MapTileLayer.ZoomPanBehavior>
                    
                    <!-- Marker Tooltip Settings -->
                    <maps:MapTileLayer.MarkerTooltipSettings>
                        <maps:MapTooltipSettings Background="Transparent" 
                                                Margin="0"
                                                Padding="0" />
                    </maps:MapTileLayer.MarkerTooltipSettings>
                </maps:MapTileLayer>
            </maps:SfMaps.Layer>
        </maps:SfMaps>
    </Grid>
</ContentPage>
```

**Key SfMaps Configuration Properties:**

| Property | Value | Purpose |
|----------|-------|---------|
| `UrlTemplate` | `"https://tile.openstreetmap.org/{z}/{x}/{y}.png"` | OpenStreetMap tile service URL |
| `CanCacheTiles` | `True` | Enables tile caching for better performance |
| `ShowMarkerTooltip` | `True` | Displays tooltips when markers are tapped |
| `Center` | `MapLatLng(37.0902, -95.7129)` | Initial map center (Continental US) |
| `ZoomLevel` | `4` | Initial zoom level (continental view) |
| `MinZoomLevel` | `4` | Minimum allowed zoom out |
| `MaxZoomLevel` | `18` | Maximum allowed zoom in |
| `EnableDoubleTapZooming` | `True` | Enables double-tap to zoom |
| `EnablePinchZooming` | `True` | Enables pinch gesture for zoom |

**SfAutocomplete Configuration:**
- Natural language input support
- Placeholder text guides user queries
- Integrated search button for query submission

### Step 5: Customize Marker Icons and Tooltip Templates

Define DataTemplates for marker visualization and rich tooltip displays using Syncfusion Maps template properties:

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        
        <!-- Marker Icon Template (MapTileLayer.MarkerTemplate) -->
        <DataTemplate x:Key="MarkerTemplate">
            <StackLayout IsClippedToBounds="False"
                        HorizontalOptions="Start"
                        VerticalOptions="Start"
                        HeightRequest="30"
                        WidthRequest="30">
                <Image Source="map_pin.png"
                      Scale="1"
                      Aspect="AspectFit"
                      HeightRequest="30"
                      WidthRequest="30" />
            </StackLayout>
        </DataTemplate>
        
        <!-- Detailed Tooltip Template (with address for specific locations) -->
        <DataTemplate x:Key="DetailTemplate">
            <Frame HasShadow="True" 
                  Margin="0" 
                  Padding="0" 
                  CornerRadius="10" 
                  WidthRequest="250"
                  BackgroundColor="White">
                <StackLayout Orientation="Vertical">
                    <!-- AI-Generated Location Image -->
                    <Image Source="{Binding DataItem.Image}" 
                          HeightRequest="120" 
                          Margin="0" 
                          WidthRequest="250" 
                          Aspect="AspectFill"/>
                    
                    <!-- Location Name -->
                    <Label Text="{Binding DataItem.Name}" 
                          FontAttributes="Bold" 
                          FontSize="12" 
                          LineBreakMode="WordWrap"
                          Padding="10,5,0,0"/>
                    
                    <!-- Location Details/Description -->
                    <Label Text="{Binding DataItem.Details}" 
                          LineBreakMode="WordWrap"
                          FontSize="10" 
                          Padding="10,0,0,0"/>
                    
                    <!-- Street Address -->
                    <Label Padding="10,0,0,5">
                        <Label.FormattedText>
                            <FormattedString>
                                <Span Text="📍 " FontSize="8"/>
                                <Span Text="{Binding DataItem.Address}" 
                                     FontSize="10"/>
                            </FormattedString>
                        </Label.FormattedText>
                    </Label>
                </StackLayout>
            </Frame>
        </DataTemplate>
        
        <!-- Simple Tooltip Template (no address for regions/cities) -->
        <DataTemplate x:Key="NormalTemplate">
            <Frame HasShadow="True" 
                  Margin="0" 
                  Padding="0" 
                  CornerRadius="10" 
                  WidthRequest="250"
                  BackgroundColor="White">
                <StackLayout Orientation="Vertical">
                    <!-- AI-Generated Location Image -->
                    <Image Source="{Binding DataItem.Image}" 
                          HeightRequest="120" 
                          Margin="0" 
                          WidthRequest="250" 
                          Aspect="AspectFill"/>
                    
                    <!-- Location Name -->
                    <Label Text="{Binding DataItem.Name}" 
                          FontAttributes="Bold" 
                          FontSize="12" 
                          LineBreakMode="WordWrap"
                          Padding="10,5,0,0"/>
                    
                    <!-- Location Details/Description -->
                    <Label Text="{Binding DataItem.Details}" 
                          LineBreakMode="WordWrap"
                          FontSize="10" 
                          Padding="10,0,0,5"/>
                </StackLayout>
            </Frame>
        </DataTemplate>
        
        <!-- DataTemplateSelector for Dynamic Tooltip Selection -->
        <local:MarkerTemplateSelector x:Key="MarkerTemplateSelector"
                                     DetailTemplate="{StaticResource DetailTemplate}"
                                     NormalTemplate="{StaticResource NormalTemplate}"/>
    </ResourceDictionary>
</ContentPage.Resources>
```

**DataTemplateSelector Implementation:**

```csharp
using Microsoft.Maui.Controls;

namespace YourApp.Selectors
{
    public class MarkerTemplateSelector : DataTemplateSelector
    {
        public DataTemplate? NormalTemplate { get; set; }
        public DataTemplate? DetailTemplate { get; set; }
        
        protected override DataTemplate? OnSelectTemplate(object item, BindableObject container)
        {
            var customMarker = (CustomMarker)item;
            
            // Select template based on Address presence
            // DetailTemplate: Shows address for specific locations
            // NormalTemplate: No address for regions/cities
            return customMarker.Address == null ? NormalTemplate : DetailTemplate;
        }
    }
}
```

**Template Selection Logic:**

| Marker Type | Address Property | Template Used | Display Elements |
|-------------|-----------------|---------------|------------------|
| Specific Location (Hospital, Restaurant) | Not null | `DetailTemplate` | Image, Name, Details, Address |
| Broad Location (City, Region) | null | `NormalTemplate` | Image, Name, Details |

**Key Syncfusion Maps Template Properties:**
- **`MarkerTemplate`**: Controls marker icon appearance (pin icon)
- **`MarkerTooltipTemplate`**: Controls tooltip content and layout (DataTemplateSelector)
- **Binding Context**: Tooltip templates bind to `DataItem` property of marker

### Step 6: Implement AI-Powered Search Logic

Integrate Azure OpenAI with Syncfusion Maps to process queries and display results:

```csharp
using Syncfusion.Maui.Maps;
using Newtonsoft.Json.Linq;
using System.Collections.ObjectModel;
using System.Globalization;

namespace YourApp.Views
{
    public partial class AILocationSearchPage : ContentPage
    {
        // Azure OpenAI Service Instance
        private readonly AzureOpenAIService azureAIHelper;
        
        // Custom Markers Collection for SfMaps
        private ObservableCollection<CustomMarker>? customMarkers;
        
        public AILocationSearchPage()
        {
            InitializeComponent();
            
            // Initialize Azure OpenAI service
            azureAIHelper = new AzureOpenAIService();
            
            // Initialize markers collection
            customMarkers = new ObservableCollection<CustomMarker>();
        }

        private async void OnSearchClicked(object sender, EventArgs e)
        {
            // Validate input
            if (string.IsNullOrWhiteSpace(autoComplete.Text))
            {
                await DisplayAlert("Input Required", "Please enter a location to search", "OK");
                return;
            }
            
            // Trigger AI search
            await GetRecommendationAsync(autoComplete.Text);
        }
        
        private async Task GetRecommendationAsync(string userQuery)
        {
            try
            {
                // Show loading indicator
                if (busyIndicator != null)
                {
                    busyIndicator.IsVisible = true;
                    busyIndicator.IsRunning = true;
                }
                
                // Construct AI prompt for structured location data
                string prompt = $"Given location name: {userQuery}" +
                    $"\nSome conditions need to follow:" +
                    $"\nCheck the location name is just a state, city, capital or region, then retrieve the following fields: location name, detail, latitude, longitude, and set address value as null" +
                    $"\nOtherwise, retrieve minimum 5 to 6 entries with following fields: location's name, details, latitude, longitude, address." +
                    $"\nThe return format should be the following JSON format: markercollections[Name, Details, Latitude, Longitude, Address]" +
                    $"\nRemove ```json and remove ``` if it is there in the code." +
                    $"\nProvide JSON format details only, No need any explanation.";
                
                // Call Azure OpenAI API
                var returnMessage = await azureAIHelper.GetResultsFromAI(prompt);
                
                if (string.IsNullOrEmpty(returnMessage))
                {
                    await DisplayAlert("Error", "Failed to get results from AI service", "OK");
                    return;
                }
                
                // Parse JSON response from AI
                var jsonObj = JObject.Parse(returnMessage);
                
                // Clear existing markers
                this.customMarkers?.Clear();
                
                // Process each location from AI response
                foreach (var marker in jsonObj["markercollections"])
                {
                    CustomMarker customMarker = new CustomMarker
                    {
                        Name = (string)marker["Name"],
                        Details = (string)marker["Details"],
                        Address = (string)marker["Address"],
                        Latitude = StringToDoubleConverter((string)marker["Latitude"]),
                        Longitude = StringToDoubleConverter((string)marker["Longitude"])
                    };
                    
                    // Generate AI-powered image for location
                    if (azureAIHelper.Client != null)
                    {
                        customMarker.Image = await azureAIHelper.GetImageFromAI(customMarker.Name);
                        customMarker.ImageName = string.Empty;
                    }
                    
                    // Add to markers collection
                    this.customMarkers?.Add(customMarker);
                }
                
                // Update SfMaps with new markers
                layer.Markers = this.customMarkers;
                layer.EnableCenterAnimation = true;
                
                // Center map on first marker location
                if (this.customMarkers != null && this.customMarkers.Count > 0)
                {
                    var firstMarker = this.customMarkers[0];
                    layer.Center = new MapLatLng
                    {
                        Latitude = firstMarker.Latitude,
                        Longitude = firstMarker.Longitude
                    };
                    
                    // Set appropriate zoom level for location view
                    if (azureAIHelper.Client != null)
                    {
                        zoomPanBehavior.ZoomLevel = 10;
                    }
                }
                
                // Show success message
                await DisplayAlert("Search Complete", 
                    $"Found {this.customMarkers?.Count ?? 0} locations", "OK");
            }
            catch (Newtonsoft.Json.JsonException ex)
            {
                await DisplayAlert("Parse Error", 
                    $"Failed to parse AI response: {ex.Message}", "OK");
            }
            catch (Exception ex)
            {
                await DisplayAlert("Error", 
                    $"An error occurred: {ex.Message}", "OK");
            }
            finally
            {
                // Hide loading indicator
                if (busyIndicator != null)
                {
                    busyIndicator.IsVisible = false;
                    busyIndicator.IsRunning = false;
                }
            }
        }
        
        private double StringToDoubleConverter(string value)
        {
            if (double.TryParse(value, NumberStyles.Any, CultureInfo.InvariantCulture, out double result))
                return result;
            
            return 0;
        }
    }
}
```

**Key Syncfusion Maps APIs Used in Search Logic:**

| API | Property/Method | Purpose |
|-----|----------------|---------|
| `MapTileLayer.Markers` | Property | Sets collection of `CustomMarker` objects to display |
| `MapTileLayer.Center` | Property | Sets geographic center using `MapLatLng` |
| `MapTileLayer.EnableCenterAnimation` | Property | Enables smooth animation when re-centering |
| `MapZoomPanBehavior.ZoomLevel` | Property | Controls map zoom level (4=continental, 10=city) |
| `MapLatLng` | Constructor | Creates coordinate object with Latitude/Longitude |

**AI-to-Maps Workflow:**
1. **User Input**: Natural language query via `SfAutocomplete`
2. **AI Processing**: Azure OpenAI returns structured JSON location data
3. **Data Parsing**: JSON parsed into `CustomMarker` collection
4. **Image Generation**: DALL-E generates location-specific images
5. **Map Update**: Markers assigned to `MapTileLayer.Markers` property
6. **View Centering**: Map centers on first marker with animation
7. **Zoom Adjustment**: Zoom level set to 10 for optimal city/location view

## AI Prompt Engineering for Location Data

The structured prompt ensures Azure OpenAI returns properly formatted geographic data compatible with Syncfusion Maps APIs.

### Prompt Structure

**Complete Prompt Template:**
```csharp
string prompt = $"Given location name: {userQuery}" +
    $"\nSome conditions need to follow:" +
    $"\nCheck the location name is just a state, city, capital or region, then retrieve the following fields: location name, detail, latitude, longitude, and set address value as null" +
    $"\nOtherwise, retrieve minimum 5 to 6 entries with following fields: location's name, details, latitude, longitude, address." +
    $"\nThe return format should be the following JSON format: markercollections[Name, Details, Latitude, Longitude, Address]" +
    $"\nRemove ```json and remove ``` if it is there in the code." +
    $"\nProvide JSON format details only, No need any explanation.";
```

### Prompt Instructions Analysis

| Instruction | Purpose | Syncfusion Maps Benefit |
|-------------|---------|------------------------|
| Differentiate broad vs specific locations | Determines whether to return single or multiple entries | Proper marker density on map |
| Set Address to null for regions | Indicates broad location type | Template selector uses this for tooltip selection |
| Return 5-6 entries for specific queries | Provides multiple relevant locations | Rich visualization with multiple markers |
| Include latitude/longitude | Geographic coordinates | Direct mapping to `MapMarker.Latitude/Longitude` |
| Remove JSON code markers | Clean JSON output | Direct parsing without preprocessing |
| No explanations | JSON-only response | Efficient parsing and error reduction |

### Expected JSON Response Format

**For Specific Location Query** (e.g., "Hospitals in New York"):
```json
{
  "markercollections": [
    {
      "Name": "Mount Sinai Hospital",
      "Details": "Major academic medical center",
      "Latitude": "40.7902",
      "Longitude": "-73.9524",
      "Address": "1 Gustave L. Levy Place, New York, NY 10029"
    },
    {
      "Name": "NewYork-Presbyterian Hospital",
      "Details": "Teaching hospital affiliated with Columbia and Cornell",
      "Latitude": "40.7677",
      "Longitude": "-73.9538",
      "Address": "525 East 68th Street, New York, NY 10065"
    },
    {
      "Name": "NYU Langone Health",
      "Details": "Comprehensive medical center",
      "Latitude": "40.7425",
      "Longitude": "-73.9738",
      "Address": "550 First Avenue, New York, NY 10016"
    }
  ]
}
```

**For Broad Location Query** (e.g., "New York City"):
```json
{
  "markercollections": [
    {
      "Name": "New York City",
      "Details": "The largest city in the United States",
      "Latitude": "40.7128",
      "Longitude": "-74.0060",
      "Address": null
    }
  ]
}
```

### Mapping JSON to Syncfusion Maps

**Data Flow:**
```
AI JSON Response → CustomMarker Model → MapTileLayer.Markers Collection → SfMaps Visualization
```

**Property Mapping:**

| JSON Field | CustomMarker Property | Syncfusion Maps Usage |
|------------|----------------------|----------------------|
| `Name` | `Name` | Tooltip title, marker identification |
| `Details` | `Details` | Tooltip description text |
| `Latitude` | `Latitude` (inherited from MapMarker) | Marker Y-coordinate positioning |
| `Longitude` | `Longitude` (inherited from MapMarker) | Marker X-coordinate positioning |
| `Address` | `Address` | Tooltip template selection, address display |

## Example Query Scenarios

### Scenario 1: Category-Based Search (Specific Locations)

**User Query:** `"Hospitals in New York"`

**AI Response:** 5-6 hospital entries with complete data

**Syncfusion Maps Display:**
- **Markers**: Multiple pin markers at hospital coordinates
- **Tooltip Template**: `DetailTemplate` (includes address)
- **Zoom Level**: 10 (city-level view)
- **Center**: First hospital coordinates
- **Visual Elements**: AI-generated hospital images, names, descriptions, street addresses

**Key APIs Activated:**
- `MapTileLayer.Markers` = Collection of 5-6 `CustomMarker` objects
- `MapTileLayer.Center` = First marker's `MapLatLng`
- `MarkerTooltipTemplate` = `DetailTemplate` (Address ≠ null)

---

### Scenario 2: Broad Location Search (Region/City)

**User Query:** `"Tokyo"`

**AI Response:** Single entry for Tokyo city center

**Syncfusion Maps Display:**
- **Markers**: Single pin marker at city center
- **Tooltip Template**: `NormalTemplate` (no address field)
- **Zoom Level**: 10 (regional view)
- **Center**: City center coordinates (35.6762, 139.6503)
- **Visual Elements**: AI-generated city image, name, description

**Key APIs Activated:**
- `MapTileLayer.Markers` = Single `CustomMarker` object
- `MapTileLayer.Center` = City center `MapLatLng`
- `MarkerTooltipTemplate` = `NormalTemplate` (Address = null)

---

### Scenario 3: Tourist Attractions

**User Query:** `"Famous landmarks in Paris"`

**AI Response:** Eiffel Tower, Louvre Museum, Notre-Dame Cathedral, Arc de Triomphe, Sacré-Cœur

**Syncfusion Maps Display:**
- **Markers**: 5 pin markers at landmark locations
- **Tooltip Template**: `DetailTemplate` with addresses
- **Zoom Level**: 10 (city-level view)
- **Center**: Eiffel Tower coordinates
- **Visual Elements**: AI-generated landmark images, historical descriptions, addresses

---

### Scenario 4: Business Search

**User Query:** `"Coffee shops in Seattle"`

**AI Response:** 5-6 popular coffee shop locations

**Syncfusion Maps Display:**
- **Markers**: Multiple markers clustered in downtown area
- **Tooltip Template**: `DetailTemplate` with street addresses
- **Zoom Level**: 10 (detailed city view)
- **Interactive**: Tap markers to see AI-generated shop images and details

## Best Practices and Optimization

### 1. Input Validation

Validate user queries before calling Azure OpenAI:

```csharp
private async void OnSearchClicked(object sender, EventArgs e)
{
    // Check for empty input
    if (string.IsNullOrWhiteSpace(autoComplete.Text))
    {
        await DisplayAlert("Input Required", "Please enter a location to search", "OK");
        return;
    }
    
    // Check for minimum query length
    if (autoComplete.Text.Length < 3)
    {
        await DisplayAlert("Invalid Query", "Please enter at least 3 characters", "OK");
        return;
    }
    
    await GetRecommendationAsync(autoComplete.Text);
}
```

---

### 2. Robust JSON Parsing and Validation

Handle AI response parsing with comprehensive error checking:

```csharp
private async Task GetRecommendationAsync(string userQuery)
{
    try
    {
        var returnMessage = await azureAIHelper.GetResultsFromAI(prompt);
        
        // Validate response
        if (string.IsNullOrEmpty(returnMessage))
        {
            await DisplayAlert("No Response", "AI service returned empty response", "OK");
            return;
        }
        
        // Parse JSON
        var jsonObj = JObject.Parse(returnMessage);
        
        // Validate JSON structure
        if (jsonObj["markercollections"] == null)
        {
            await DisplayAlert("Invalid Format", "AI response missing 'markercollections' field", "OK");
            return;
        }
        
        // Check for results
        if (!jsonObj["markercollections"].Any())
        {
            await DisplayAlert("No Results", "No locations found for your query", "OK");
            return;
        }
        
        // Process markers...
    }
    catch (Newtonsoft.Json.JsonException ex)
    {
        await DisplayAlert("Parse Error", $"Failed to parse location data: {ex.Message}", "OK");
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", $"An error occurred: {ex.Message}", "OK");
    }
}
```

---

### 3. Optimize DALL-E Image Loading

Load images asynchronously to avoid blocking map display:

```csharp
// Display markers immediately without waiting for images
layer.Markers = this.customMarkers;
layer.EnableCenterAnimation = true;

// Load images asynchronously in background
foreach (var marker in this.customMarkers)
{
    if (azureAIHelper.Client != null)
    {
        _ = Task.Run(async () =>
        {
            try
            {
                var image = await azureAIHelper.GetImageFromAI(marker.Name);
                
                // Update on main thread
                Device.BeginInvokeOnMainThread(() =>
                {
                    marker.Image = image;
                });
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine($"Image load error: {ex.Message}");
                
                // Set placeholder on error
                Device.BeginInvokeOnMainThread(() =>
                {
                    marker.Image = new Uri("https://via.placeholder.com/250x120");
                });
            }
        });
    }
    else
    {
        // Use placeholder if DALL-E unavailable
        marker.Image = new Uri("https://via.placeholder.com/250x120");
    }
}
```

---

### 4. Implement Response Caching

Cache AI responses to improve performance and reduce API costs:

```csharp
public partial class AILocationSearchPage : ContentPage
{
    // Cache for storing previous search results
    private readonly Dictionary<string, List<CustomMarker>> searchCache = new();
    private const int MaxCacheEntries = 20;
    
    private async Task GetRecommendationAsync(string userQuery)
    {
        // Normalize query for cache key
        string cacheKey = userQuery.Trim().ToLowerInvariant();
        
        // Check cache first
        if (searchCache.ContainsKey(cacheKey))
        {
            this.customMarkers = new ObservableCollection<CustomMarker>(searchCache[cacheKey]);
            
            // Update map with cached data
            layer.Markers = this.customMarkers;
            layer.EnableCenterAnimation = true;
            
            if (this.customMarkers.Count > 0)
            {
                var firstMarker = this.customMarkers[0];
                layer.Center = new MapLatLng(firstMarker.Latitude, firstMarker.Longitude);
                zoomPanBehavior.ZoomLevel = 10;
            }
            
            return;
        }
        
        // Fetch from AI (code from Step 6)...
        
        // Add to cache after successful fetch
        if (this.customMarkers != null && this.customMarkers.Count > 0)
        {
            // Limit cache size
            if (searchCache.Count >= MaxCacheEntries)
            {
                var oldestKey = searchCache.Keys.First();
                searchCache.Remove(oldestKey);
            }
            
            searchCache[cacheKey] = this.customMarkers.ToList();
        }
    }
}
```

**Cache Benefits:**
- Instant results for repeated queries
- Reduced Azure OpenAI API calls
- Lower costs and improved responsiveness

---

### 5. User Feedback and Progress Indication

Provide clear feedback during AI processing:

```csharp
private async Task GetRecommendationAsync(string userQuery)
{
    try
    {
        // Show loading with message
        busyIndicator.IsVisible = true;
        busyIndicator.IsRunning = true;
        
        // Optional: Update status label
        // statusLabel.Text = "Searching with AI...";
        
        // AI processing...
        
        // Show success message with count
        if (this.customMarkers != null)
        {
            await DisplayAlert("Search Complete", 
                $"Found {this.customMarkers.Count} location{(this.customMarkers.Count != 1 ? "s" : "")}", 
                "OK");
        }
    }
    finally
    {
        // Always hide loading indicator
        busyIndicator.IsVisible = false;
        busyIndicator.IsRunning = false;
        // statusLabel.Text = string.Empty;
    }
}
```

---

### 6. Handle Azure OpenAI API Errors

Implement comprehensive error handling for API calls:

```csharp
public async Task<string> GetResultsFromAI(string userPrompt)
{
    try
    {
        var response = await client.GetChatCompletionsAsync(this.chatCompletions);
        return response.Value.Choices[0].Message.Content;
    }
    catch (Azure.RequestFailedException ex) when (ex.Status == 429)
    {
        // Rate limit exceeded
        System.Diagnostics.Debug.WriteLine("Rate limit exceeded. Please try again later.");
        return string.Empty;
    }
    catch (Azure.RequestFailedException ex) when (ex.Status == 401)
    {
        // Authentication error
        System.Diagnostics.Debug.WriteLine("Authentication failed. Check your API key.");
        return string.Empty;
    }
    catch (Azure.RequestFailedException ex)
    {
        // Other API errors
        System.Diagnostics.Debug.WriteLine($"Azure OpenAI API Error ({ex.Status}): {ex.Message}");
        return string.Empty;
    }
    catch (HttpRequestException ex)
    {
        // Network errors
        System.Diagnostics.Debug.WriteLine($"Network error: {ex.Message}");
        return string.Empty;
    }
    catch (Exception ex)
    {
        System.Diagnostics.Debug.WriteLine($"Unexpected error: {ex.Message}");
        return string.Empty;
    }
}
```

---

### 7. Validate Marker Coordinates

Ensure valid geographic coordinates before adding markers:

```csharp
foreach (var marker in jsonObj["markercollections"])
{
    CustomMarker customMarker = new CustomMarker
    {
        Name = (string)marker["Name"],
        Details = (string)marker["Details"],
        Address = (string)marker["Address"],
        Latitude = StringToDoubleConverter((string)marker["Latitude"]),
        Longitude = StringToDoubleConverter((string)marker["Longitude"])
    };
    
    // Validate coordinates
    if (customMarker.Latitude == 0 && customMarker.Longitude == 0)
    {
        System.Diagnostics.Debug.WriteLine($"Invalid coordinates for {customMarker.Name}");
        continue; // Skip invalid marker
    }
    
    // Validate coordinate ranges
    if (customMarker.Latitude < -90 || customMarker.Latitude > 90 ||
        customMarker.Longitude < -180 || customMarker.Longitude > 180)
    {
        System.Diagnostics.Debug.WriteLine($"Out of range coordinates for {customMarker.Name}");
        continue;
    }
    
    this.customMarkers?.Add(customMarker);
}
```

## Troubleshooting Common Issues

### Issue 1: No Markers Displayed on Map

**Symptoms:** Map loads but no markers appear after search

**Possible Causes & Solutions:**

**Cause A:** Invalid coordinates from AI response
```csharp
// Solution: Validate coordinates before adding
if (customMarker.Latitude == 0 && customMarker.Longitude == 0)
{
    System.Diagnostics.Debug.WriteLine($"Skipping invalid marker: {customMarker.Name}");
    continue; // Skip invalid marker
}

if (Math.Abs(customMarker.Latitude) > 90 || Math.Abs(customMarker.Longitude) > 180)
{
    System.Diagnostics.Debug.WriteLine($"Out of range coordinates: {customMarker.Name}");
    continue;
}
```

**Cause B:** Markers collection not assigned to MapTileLayer
```csharp
// Solution: Ensure assignment after population
layer.Markers = this.customMarkers; // Must be explicitly set
```

**Cause C:** MarkerTemplate not defined in resources
```csharp
// Solution: Verify DataTemplate exists in XAML
<DataTemplate x:Key="MarkerTemplate">
    <Image Source="map_pin.png" HeightRequest="30" WidthRequest="30" />
</DataTemplate>
```

---

### Issue 2: Slow Image Loading / UI Freezing

**Symptoms:** Map becomes unresponsive during image generation

**Cause:** Synchronous DALL-E image loading blocks UI thread

**Solution:** Load images asynchronously after displaying markers
```csharp
// Display markers immediately
layer.Markers = this.customMarkers;
layer.Center = new MapLatLng(firstMarker.Latitude, firstMarker.Longitude);

// Load images in background without blocking
foreach (var marker in this.customMarkers)
{
    _ = Task.Run(async () =>
    {
        try
        {
            var imageUri = await azureAIHelper.GetImageFromAI(marker.Name);
            
            // Update on main thread
            Device.BeginInvokeOnMainThread(() =>
            {
                marker.Image = imageUri;
            });
        }
        catch
        {
            // Set placeholder on failure
            Device.BeginInvokeOnMainThread(() =>
            {
                marker.Image = new Uri("https://via.placeholder.com/250x120");
            });
        }
    });
}
```

**Alternative:** Disable DALL-E and use placeholder images
```csharp
// For faster loading without AI images
customMarker.Image = new Uri("https://via.placeholder.com/250x120?text=" + 
    Uri.EscapeDataString(customMarker.Name));
```

---

### Issue 3: Map Not Centering on Search Results

**Symptoms:** Markers appear but map doesn't center on them

**Cause A:** Center set before markers collection populated
```csharp
// Incorrect: Setting center too early
layer.Markers = this.customMarkers;
var firstMarker = this.customMarkers[0]; // May be null
layer.Center = new MapLatLng(firstMarker.Latitude, firstMarker.Longitude);
```

**Solution:** Verify collection has items before centering
```csharp
// Correct: Check collection first
layer.Markers = this.customMarkers;

if (this.customMarkers != null && this.customMarkers.Count > 0)
{
    Device.BeginInvokeOnMainThread(() =>
    {
        var firstMarker = this.customMarkers[0];
        layer.Center = new MapLatLng(firstMarker.Latitude, firstMarker.Longitude);
        layer.EnableCenterAnimation = true;
        zoomPanBehavior.ZoomLevel = 10;
    });
}
```

**Cause B:** Animation disabled
```csharp
// Solution: Enable center animation
layer.EnableCenterAnimation = true; // Must be true for smooth centering
```

---

### Issue 4: JSON Parsing Errors

**Symptoms:** Exception thrown when parsing AI response

**Cause:** AI returns malformed JSON or includes code markers

**Solution A:** Handle parsing exceptions
```csharp
try
{
    var jsonObj = JObject.Parse(returnMessage);
}
catch (Newtonsoft.Json.JsonException ex)
{
    System.Diagnostics.Debug.WriteLine($"JSON Parse Error: {ex.Message}");
    System.Diagnostics.Debug.WriteLine($"AI Response: {returnMessage}");
    await DisplayAlert("Parse Error", "Failed to parse AI response", "OK");
    return;
}
```

**Solution B:** Clean AI response before parsing
```csharp
// Remove code markers if present
string cleanedResponse = returnMessage
    .Replace("```json", "")
    .Replace("```", "")
    .Trim();

var jsonObj = JObject.Parse(cleanedResponse);
```

---

### Issue 5: Azure OpenAI API Failures

**Symptoms:** Empty responses or error messages from AI service

**Cause A:** Invalid API credentials
```csharp
// Solution: Verify configuration
const string endpoint = "https://YOUR-RESOURCE.openai.azure.com"; // Check endpoint
string key = "YOUR_API_KEY"; // Verify key is correct
```

**Cause B:** Rate limiting (429 errors)
```csharp
// Solution: Implement retry logic with exponential backoff
public async Task<string> GetResultsFromAI(string userPrompt, int maxRetries = 3)
{
    for (int attempt = 0; attempt < maxRetries; attempt++)
    {
        try
        {
            var response = await client.GetChatCompletionsAsync(this.chatCompletions);
            return response.Value.Choices[0].Message.Content;
        }
        catch (Azure.RequestFailedException ex) when (ex.Status == 429)
        {
            if (attempt < maxRetries - 1)
            {
                int delayMs = (int)Math.Pow(2, attempt) * 1000; // 1s, 2s, 4s
                await Task.Delay(delayMs);
            }
            else
            {
                throw; // Rethrow on final attempt
            }
        }
    }
    
    return string.Empty;
}
```

**Cause C:** Quota exceeded
- Check Azure OpenAI dashboard for usage limits
- Upgrade tier or wait for quota reset

---

### Issue 6: Tooltips Not Displaying

**Symptoms:** Markers visible but tooltips don't appear on tap

**Cause A:** ShowMarkerTooltip disabled
```csharp
// Solution: Enable tooltip display
<maps:MapTileLayer ShowMarkerTooltip="True" ... />
```

**Cause B:** MarkerTooltipTemplate not set
```csharp
// Solution: Assign template
<maps:MapTileLayer MarkerTooltipTemplate="{StaticResource MarkerTemplateSelector}" ... />
```

**Cause C:** DataItem binding context incorrect
```csharp
// Solution: Bind to DataItem property in template
<Label Text="{Binding DataItem.Name}" /> <!-- Correct -->
<Label Text="{Binding Name}" /> <!-- Incorrect -->
```

---

### Issue 7: Memory Leaks with Large Marker Collections

**Symptoms:** App memory usage increases with each search

**Solution:** Clear previous markers before adding new ones
```csharp
// Clear old markers to prevent memory buildup
if (layer.Markers != null)
{
    layer.Markers.Clear();
}

this.customMarkers?.Clear();

// Add new markers...
this.customMarkers = new ObservableCollection<CustomMarker>();
```

**Additional:** Dispose of images when clearing
```csharp
foreach (var marker in this.customMarkers)
{
    marker.Image = null; // Release image references
}
this.customMarkers.Clear();
```

## Performance Optimization Tips

### 1. Tile Caching

Enable tile caching to reduce network requests:
```csharp
<maps:MapTileLayer CanCacheTiles="True" ... />
```

### 2. Limit Marker Count

Restrict number of markers for better performance:
```csharp
// In prompt: "retrieve minimum 5 to 6 entries"
// Or filter in code:
const int MaxMarkers = 10;
var limitedMarkers = jsonObj["markercollections"].Take(MaxMarkers);
```

### 3. Image Size Optimization

Use appropriate image sizes for markers:
```csharp
new ImageGenerationOptions()
{
    Size = ImageSize.Size256x256, // Smaller for better performance
    Quality = ImageGenerationQuality.Standard
}
```

### 4. Debounce Search Input

Prevent excessive API calls from rapid typing:
```csharp
private CancellationTokenSource? searchCts;
private const int DebounceDelayMs = 500;

private async void OnSearchClicked(object sender, EventArgs e)
{
    // Cancel previous search
    searchCts?.Cancel();
    searchCts = new CancellationTokenSource();
    
    try
    {
        await Task.Delay(DebounceDelayMs, searchCts.Token);
        await GetRecommendationAsync(autoComplete.Text);
    }
    catch (TaskCanceledException)
    {
        // Search was cancelled
    }
}
```

## API Summary Reference

### Syncfusion.Maui.Maps Key APIs

| API | Category | Purpose in AI Integration |
|-----|----------|---------------------------|
| `SfMaps` | Control | Root container for map visualization |
| `MapTileLayer` | Layer | OpenStreetMap tile layer for base map |
| `MapMarker` | Model | Base class for location markers with coordinates |
| `CustomMarker` | Model (Custom) | Extended marker with AI-generated metadata |
| `MapLatLng` | Model | Geographic coordinate representation |
| `MapZoomPanBehavior` | Behavior | Controls zoom levels and pan interactions |
| `MapTooltipSettings` | Settings | Tooltip appearance configuration |
| `Markers` | Property | Collection binding for marker display |
| `Center` | Property | Map center coordinate |
| `ShowMarkerTooltip` | Property | Toggle tooltip visibility |
| `MarkerTemplate` | Property | DataTemplate for marker icon |
| `MarkerTooltipTemplate` | Property | DataTemplate for tooltip content |
| `EnableCenterAnimation` | Property | Smooth re-centering animation |
| `CanCacheTiles` | Property | Enable/disable tile caching |
| `UrlTemplate` | Property | Tile service URL pattern |
| `ZoomLevel` | Property | Current zoom level (4-18 range) |

### Azure.AI.OpenAI Key APIs

| API | Purpose |
|-----|---------|
| `OpenAIClient` | Main Azure OpenAI service client |
| `AzureKeyCredential` | API authentication |
| `ChatCompletionsOptions` | Chat configuration |
| `ChatRequestSystemMessage` | System instructions |
| `ChatRequestUserMessage` | User query |
| `GetChatCompletionsAsync` | Send chat request |
| `ImageGenerationOptions` | Image generation config |
| `GetImageGenerationsAsync` | Generate images with DALL-E |

## Complete Code Sample

**Full working implementation available at:**
[GitHub Repository: Integrating AI-Driven Location Search into .NET MAUI Maps](https://github.com/SyncfusionExamples/Integrating-AI-Driven-Location-Search-into-.NET-MAUI-Maps)

**Sample includes:**
- Complete Azure OpenAI integration
- Custom marker model with AI metadata
- XAML UI with SfMaps and SfAutocomplete
- Template selectors for tooltips
- Error handling and validation
- Image loading optimization
- Response caching implementation

## Related Documentation

**Syncfusion .NET MAUI Maps:**
- [Getting Started with Maps](getting-started.md) - Basic Maps setup
- [Tile Layer Configuration](tile-layer.md) - OSM tile layer details
- [Marker Customization](markers.md) - Advanced marker templates
- [Tooltip Implementation](tooltip.md) - Tooltip customization guide
- [Zoom and Pan](zoom-pan.md) - Zoom behavior configuration

**AI Integration:**
- [Azure OpenAI Documentation](https://learn.microsoft.com/en-us/azure/ai-services/openai/) - Official Azure OpenAI docs
- [GPT-4O Model Guide](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/models#gpt-4o) - Model capabilities
- [DALL-E Image Generation](https://learn.microsoft.com/en-us/azure/ai-services/openai/how-to/dall-e) - Image generation guide

## Key Takeaways

✅ **Integration Points:**
- Azure OpenAI provides natural language → geographic data conversion
- Syncfusion SfMaps visualizes AI-generated location data
- DALL-E enhances UX with AI-generated location images

✅ **Essential Components:**
- `AzureOpenAIService` for AI communication
- `CustomMarker` extending `MapMarker` for rich metadata
- `MapTileLayer.Markers` for dynamic marker binding
- DataTemplate selectors for context-aware tooltips

✅ **Best Practices:**
- Validate all coordinates before adding markers
- Load images asynchronously to prevent UI blocking
- Implement response caching to reduce API costs
- Handle API errors with retry logic
- Provide clear user feedback during processing

✅ **Performance Tips:**
- Enable tile caching (`CanCacheTiles="True"`)
- Limit marker count (5-6 recommended)
- Use appropriate image sizes
- Debounce search input
- Clear old markers before adding new ones