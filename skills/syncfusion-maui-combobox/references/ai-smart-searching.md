# AI-Powered Smart Searching in .NET MAUI ComboBox

## Table of Contents
- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Implementation Steps](#implementation-steps)
- [Azure OpenAI Integration](#azure-openai-integration)
- [Custom Filter Behavior](#custom-filter-behavior)
- [Fallback Mechanisms](#fallback-mechanisms)
- [Complete Implementation Example](#complete-implementation-example)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

The Syncfusion .NET MAUI ComboBox can be enhanced with AI-powered semantic searching using Azure OpenAI embeddings. This enables intelligent searching that understands context, synonyms, and semantic meaning rather than just exact text matching.

**Benefits:**
- Semantic understanding (e.g., "car" matches "automobile")
- Typo tolerance
- Context-aware searching
- Better user experience for complex datasets

## Prerequisites

### NuGet Packages

Install the required NuGet packages:

```xml
<PackageReference Include="Azure.AI.OpenAI" Version="1.0.0-beta.17" />
<PackageReference Include="Microsoft.Extensions.Http" Version="8.0.0" />
```

**PowerShell:**
```powershell
Install-Package Azure.AI.OpenAI -Version 1.0.0-beta.17
Install-Package Microsoft.Extensions.Http -Version 8.0.0
```

**.NET CLI:**
```bash
dotnet add package Azure.AI.OpenAI --version 1.0.0-beta.17
dotnet add package Microsoft.Extensions.Http --version 8.0.0
```

### Azure OpenAI Setup

1. Create an Azure OpenAI resource in Azure Portal
2. Deploy an embeddings model (e.g., `text-embedding-ada-002`)
3. Obtain the following credentials:
   - **API Key**
   - **Endpoint URL**
   - **Deployment Name**

**Secure Storage:**
```csharp
// Store in app settings or secure storage
public static class AzureOpenAIConfig
{
    public static string ApiKey => SecureStorage.GetAsync("AzureOpenAI_ApiKey").Result;
    public static string Endpoint => "https://your-resource.openai.azure.com/";
    public static string DeploymentName => "text-embedding-ada-002";
}
```

## Implementation Steps

### Step 1: Create Data Model

**C#:**
```csharp
public class SocialMedia
{
    public string Name { get; set; }
    public string Category { get; set; }
    
    // Store embedding for AI search
    public float[] Embedding { get; set; }
}
```

### Step 2: Initialize Azure OpenAI Client

**C#:**
```csharp
using Azure;
using Azure.AI.OpenAI;

public class AISearchService
{
    private readonly OpenAIClient _openAIClient;
    private readonly string _deploymentName;
    
    public AISearchService(string endpoint, string apiKey, string deploymentName)
    {
        _openAIClient = new OpenAIClient(new Uri(endpoint), new AzureKeyCredential(apiKey));
        _deploymentName = deploymentName;
    }
    
    public async Task<float[]> GetEmbeddingAsync(string text)
    {
        var embeddingsOptions = new EmbeddingsOptions(_deploymentName, new List<string> { text });
        var response = await _openAIClient.GetEmbeddingsAsync(embeddingsOptions);
        return response.Value.Data[0].Embedding.ToArray();
    }
}
```

### Step 3: Precompute Embeddings for Items

**C#:**
```csharp
public class ViewModel
{
    private readonly AISearchService _aiSearchService;
    public ObservableCollection<SocialMedia> SocialMedias { get; set; }
    
    public ViewModel()
    {
        _aiSearchService = new AISearchService(
            AzureOpenAIConfig.Endpoint,
            AzureOpenAIConfig.ApiKey,
            AzureOpenAIConfig.DeploymentName
        );
        
        SocialMedias = new ObservableCollection<SocialMedia>();
        _ = InitializeDataAsync();
    }
    
    private async Task InitializeDataAsync()
    {
        var items = new List<SocialMedia>
        {
            new SocialMedia { Name = "Facebook", Category = "Social Network" },
            new SocialMedia { Name = "Twitter", Category = "Microblogging" },
            new SocialMedia { Name = "Instagram", Category = "Photo Sharing" },
            new SocialMedia { Name = "LinkedIn", Category = "Professional Network" },
            new SocialMedia { Name = "YouTube", Category = "Video Platform" },
            new SocialMedia { Name = "TikTok", Category = "Short Video" },
            new SocialMedia { Name = "WhatsApp", Category = "Messaging" }
        };
        
        // Precompute embeddings for all items
        foreach (var item in items)
        {
            item.Embedding = await _aiSearchService.GetEmbeddingAsync(item.Name);
        }
        
        foreach (var item in items)
        {
            SocialMedias.Add(item);
        }
    }
}
```

## Azure OpenAI Integration

### Step 4: Implement Custom Filter Behavior

Create a custom filter class that implements `IComboBoxFilterBehavior`.

**C#:**
```csharp
using Syncfusion.Maui.Inputs;

public class AIComboBoxFilterBehavior : IComboBoxFilterBehavior
{
    private readonly AISearchService _aiSearchService;
    
    public AIComboBoxFilterBehavior(AISearchService aiSearchService)
    {
        _aiSearchService = aiSearchService;
    }
    
    public async Task<object> GetMatchingIndexes(
        SfComboBox source, 
        ComboBoxFilterInfo filterInfo, 
        CancellationToken cancellationToken)
    {
        var searchText = filterInfo.Text;
        var items = source.ItemsSource?.Cast<SocialMedia>().ToList();
        
        if (string.IsNullOrWhiteSpace(searchText) || items == null || items.Count == 0)
        {
            return Enumerable.Range(0, items?.Count ?? 0);
        }
        
        try
        {
            // Get embedding for search text
            var searchEmbedding = await _aiSearchService.GetEmbeddingAsync(searchText);
            
            // Calculate similarity scores
            var scoredItems = items.Select((item, index) => new
            {
                Index = index,
                Item = item,
                Similarity = CalculateCosineSimilarity(searchEmbedding, item.Embedding)
            })
            .OrderByDescending(x => x.Similarity)
            .ToList();
            
            // Return top matches (similarity > 0.7)
            var matchingIndexes = scoredItems
                .Where(x => x.Similarity > 0.7)
                .Select(x => x.Index)
                .ToList();
            
            return matchingIndexes;
        }
        catch (Exception ex)
        {
            // Fallback to default behavior on error
            System.Diagnostics.Debug.WriteLine($"AI Search Error: {ex.Message}");
            return GetFallbackMatches(searchText, items);
        }
    }
    
    private float CalculateCosineSimilarity(float[] vector1, float[] vector2)
    {
        if (vector1.Length != vector2.Length)
            return 0;
        
        float dotProduct = 0;
        float magnitude1 = 0;
        float magnitude2 = 0;
        
        for (int i = 0; i < vector1.Length; i++)
        {
            dotProduct += vector1[i] * vector2[i];
            magnitude1 += vector1[i] * vector1[i];
            magnitude2 += vector2[i] * vector2[i];
        }
        
        magnitude1 = (float)Math.Sqrt(magnitude1);
        magnitude2 = (float)Math.Sqrt(magnitude2);
        
        if (magnitude1 == 0 || magnitude2 == 0)
            return 0;
        
        return dotProduct / (magnitude1 * magnitude2);
    }
    
    private List<int> GetFallbackMatches(string searchText, List<SocialMedia> items)
    {
        // Simple contains fallback
        return items
            .Select((item, index) => new { Index = index, Item = item })
            .Where(x => x.Item.Name.Contains(searchText, StringComparison.OrdinalIgnoreCase))
            .Select(x => x.Index)
            .ToList();
    }
}
```

### Step 5: Configure ComboBox with AI Filter

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    Placeholder="Search with AI"
                    IsEditable="True"
                    IsFilteringEnabled="True"
                    TextSearchMode="Contains" />
```

**C#:**
```csharp
public partial class MainPage : ContentPage
{
    private readonly AISearchService _aiSearchService;
    
    public MainPage()
    {
        InitializeComponent();
        
        _aiSearchService = new AISearchService(
            AzureOpenAIConfig.Endpoint,
            AzureOpenAIConfig.ApiKey,
            AzureOpenAIConfig.DeploymentName
        );
        
        // Set custom AI filter behavior
        comboBox.FilterBehavior = new AIComboBoxFilterBehavior(_aiSearchService);
        
        BindingContext = new ViewModel();
    }
}
```

## Custom Filter Behavior

### Advanced Filter with Caching

Improve performance by caching embeddings:

**C#:**
```csharp
public class CachedAIFilterBehavior : IComboBoxFilterBehavior
{
    private readonly AISearchService _aiSearchService;
    private readonly Dictionary<string, float[]> _embeddingCache;
    
    public CachedAIFilterBehavior(AISearchService aiSearchService)
    {
        _aiSearchService = aiSearchService;
        _embeddingCache = new Dictionary<string, float[]>();
    }
    
    public async Task<object> GetMatchingIndexes(
        SfComboBox source, 
        ComboBoxFilterInfo filterInfo, 
        CancellationToken cancellationToken)
    {
        var searchText = filterInfo.Text.ToLowerInvariant();
        
        // Check cache first
        if (!_embeddingCache.ContainsKey(searchText))
        {
            _embeddingCache[searchText] = await _aiSearchService.GetEmbeddingAsync(searchText);
        }
        
        var searchEmbedding = _embeddingCache[searchText];
        
        // Continue with similarity calculation...
        var items = source.ItemsSource?.Cast<SocialMedia>().ToList();
        if (items == null) return new List<int>();
        
        var matches = items
            .Select((item, index) => new
            {
                Index = index,
                Similarity = CalculateCosineSimilarity(searchEmbedding, item.Embedding)
            })
            .Where(x => x.Similarity > 0.7)
            .OrderByDescending(x => x.Similarity)
            .Select(x => x.Index)
            .ToList();
        
        return matches;
    }
    
    private float CalculateCosineSimilarity(float[] vector1, float[] vector2)
    {
        // Same implementation as before
        // ...
    }
}
```

## Fallback Mechanisms

### Hybrid Search with Multiple Fallbacks

Implement multiple fallback strategies:

**C#:**
```csharp
public class HybridAIFilterBehavior : IComboBoxFilterBehavior
{
    private readonly AISearchService _aiSearchService;
    
    public async Task<object> GetMatchingIndexes(
        SfComboBox source, 
        ComboBoxFilterInfo filterInfo, 
        CancellationToken cancellationToken)
    {
        var searchText = filterInfo.Text;
        var items = source.ItemsSource?.Cast<SocialMedia>().ToList();
        
        if (items == null || items.Count == 0)
            return new List<int>();
        
        try
        {
            // Try AI-powered search first
            var aiMatches = await GetAIMatchesAsync(searchText, items);
            if (aiMatches.Any())
                return aiMatches;
        }
        catch
        {
            // AI failed, continue to fallbacks
        }
        
        // Fallback 1: Soundex (phonetic matching)
        var soundexMatches = GetSoundexMatches(searchText, items);
        if (soundexMatches.Any())
            return soundexMatches;
        
        // Fallback 2: Levenshtein distance (typo tolerance)
        var levenshteinMatches = GetLevenshteinMatches(searchText, items);
        if (levenshteinMatches.Any())
            return levenshteinMatches;
        
        // Fallback 3: Simple contains
        return GetContainsMatches(searchText, items);
    }
    
    private async Task<List<int>> GetAIMatchesAsync(string searchText, List<SocialMedia> items)
    {
        var searchEmbedding = await _aiSearchService.GetEmbeddingAsync(searchText);
        
        return items
            .Select((item, index) => new
            {
                Index = index,
                Similarity = CalculateCosineSimilarity(searchEmbedding, item.Embedding)
            })
            .Where(x => x.Similarity > 0.7)
            .OrderByDescending(x => x.Similarity)
            .Select(x => x.Index)
            .ToList();
    }
    
    private List<int> GetSoundexMatches(string searchText, List<SocialMedia> items)
    {
        var searchSoundex = Soundex(searchText);
        
        return items
            .Select((item, index) => new { Index = index, Item = item })
            .Where(x => Soundex(x.Item.Name) == searchSoundex)
            .Select(x => x.Index)
            .ToList();
    }
    
    private List<int> GetLevenshteinMatches(string searchText, List<SocialMedia> items)
    {
        return items
            .Select((item, index) => new
            {
                Index = index,
                Distance = LevenshteinDistance(searchText.ToLower(), item.Name.ToLower())
            })
            .Where(x => x.Distance <= 3) // Allow up to 3 character differences
            .OrderBy(x => x.Distance)
            .Select(x => x.Index)
            .ToList();
    }
    
    private List<int> GetContainsMatches(string searchText, List<SocialMedia> items)
    {
        return items
            .Select((item, index) => new { Index = index, Item = item })
            .Where(x => x.Item.Name.Contains(searchText, StringComparison.OrdinalIgnoreCase))
            .Select(x => x.Index)
            .ToList();
    }
    
    // Soundex algorithm
    private string Soundex(string text)
    {
        if (string.IsNullOrWhiteSpace(text)) return "";
        
        text = text.ToUpper();
        var soundex = text[0].ToString();
        
        var codes = new Dictionary<char, char>
        {
            {'B','1'},{'F','1'},{'P','1'},{'V','1'},
            {'C','2'},{'G','2'},{'J','2'},{'K','2'},{'Q','2'},{'S','2'},{'X','2'},{'Z','2'},
            {'D','3'},{'T','3'},
            {'L','4'},
            {'M','5'},{'N','5'},
            {'R','6'}
        };
        
        char lastCode = codes.ContainsKey(text[0]) ? codes[text[0]] : '0';
        
        for (int i = 1; i < text.Length && soundex.Length < 4; i++)
        {
            if (codes.ContainsKey(text[i]))
            {
                var code = codes[text[i]];
                if (code != lastCode)
                {
                    soundex += code;
                    lastCode = code;
                }
            }
            else
            {
                lastCode = '0';
            }
        }
        
        return soundex.PadRight(4, '0');
    }
    
    // Levenshtein distance algorithm
    private int LevenshteinDistance(string s, string t)
    {
        int n = s.Length;
        int m = t.Length;
        int[,] d = new int[n + 1, m + 1];
        
        if (n == 0) return m;
        if (m == 0) return n;
        
        for (int i = 0; i <= n; i++) d[i, 0] = i;
        for (int j = 0; j <= m; j++) d[0, j] = j;
        
        for (int i = 1; i <= n; i++)
        {
            for (int j = 1; j <= m; j++)
            {
                int cost = (t[j - 1] == s[i - 1]) ? 0 : 1;
                d[i, j] = Math.Min(Math.Min(d[i - 1, j] + 1, d[i, j - 1] + 1), d[i - 1, j - 1] + cost);
            }
        }
        
        return d[n, m];
    }
    
    private float CalculateCosineSimilarity(float[] vector1, float[] vector2)
    {
        // Implementation from previous example
        // ...
    }
}
```

## Complete Implementation Example

**MainPage.xaml:**
```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:editors="clr-namespace:Syncfusion.Maui.Inputs;assembly=Syncfusion.Maui.Inputs"
             x:Class="ComboBoxAI.MainPage">
    <VerticalStackLayout Padding="20" Spacing="20">
        <Label Text="AI-Powered ComboBox Search"
               FontSize="20"
               FontAttributes="Bold"
               HorizontalOptions="Center"/>
        
        <Label Text="Type to search (supports typos, synonyms, and semantic matches)"
               FontSize="12"
               TextColor="Gray"
               HorizontalOptions="Center"/>
        
        <editors:SfComboBox x:Name="comboBox"
                            ItemsSource="{Binding SocialMedias}"
                            DisplayMemberPath="Name"
                            TextMemberPath="Name"
                            Placeholder="Search social media platforms"
                            IsEditable="True"
                            IsFilteringEnabled="True"
                            HeightRequest="50" />
        
        <ActivityIndicator x:Name="loadingIndicator"
                           IsRunning="{Binding IsLoading}"
                           IsVisible="{Binding IsLoading}"
                           Color="Blue"/>
    </VerticalStackLayout>
</ContentPage>
```

## Best Practices

1. **Precompute Embeddings:**
   - Generate embeddings for all items during initialization
   - Store embeddings in your data model or database
   - Only compute search text embedding in real-time

2. **Implement Caching:**
   - Cache frequently searched terms
   - Use appropriate cache expiration policies

3. **Handle Errors Gracefully:**
   - Always have fallback mechanisms
   - Log errors for debugging
   - Show user-friendly error messages

4. **Optimize Performance:**
   - Use asynchronous operations
   - Cancel previous requests when new ones are made
   - Limit the number of API calls

5. **Tune Similarity Threshold:**
   - Experiment with similarity thresholds (0.6-0.8 typically works well)
   - Adjust based on your use case and data

6. **Secure API Keys:**
   - Never hardcode API keys
   - Use secure storage or environment variables
   - Rotate keys regularly

## Troubleshooting

### Issue: Slow Search Performance

**Solution:**
- Precompute and cache item embeddings
- Implement request debouncing
- Use smaller embedding models if accuracy permits

### Issue: Poor Match Quality

**Solution:**
- Lower similarity threshold (e.g., from 0.7 to 0.6)
- Include more context in embeddings (e.g., Name + Category)
- Use fallback mechanisms

### Issue: API Rate Limits

**Solution:**
- Implement request throttling
- Cache search results
- Use batch embedding requests during initialization

### Issue: High API Costs

**Solution:**
- Cache computed embeddings
- Use smaller embedding models
- Implement smart caching strategies

## Related Topics

- [Filtering](filtering.md) - Standard filtering with IComboBoxFilterBehavior
- [Searching](searching.md) - Text search modes and behavior
- [Getting Started](getting-started.md) - Basic ComboBox setup
- [Advanced Features](advanced-features.md) - Additional customization options
