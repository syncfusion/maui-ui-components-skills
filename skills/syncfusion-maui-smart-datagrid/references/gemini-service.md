# Google Gemini AI Integration

Integrate Google Gemini with Syncfusion Smart Components.

## Setup

1. **Get API Key:** [Google AI Studio](https://ai.google.dev/gemini-api/docs/api-key)
2. **Models:** [Gemini Models](https://ai.google.dev/gemini-api/docs/models)

## Implementation

**GeminiModels.cs:**
```csharp
public class Part { public string Text { get; set; } }
public class Content { public Part[] Parts { get; init; } = Array.Empty<Part>(); }
public class Candidate { public Content Content { get; init; } = new(); }
public class GeminiResponseObject { public Candidate[] Candidates { get; init; } = Array.Empty<Candidate>(); }

public class ResponseContent
{
    public List<Part> Parts { get; init; }
    public string Role { get; init; }
    public ResponseContent(string text, string role)
    {
        Parts = new List<Part> { new Part { Text = text } };
        Role = role;
    }
}

public class GeminiChatParameters
{
    public List<ResponseContent> Contents { get; init; } = new();
    public GenerationConfig GenerationConfig { get; init; } = new();
}

public class GenerationConfig
{
    public int MaxOutputTokens { get; init; } = 2048;
}
```

**GeminiService.cs:**
```csharp
using System.Net.Http;
using System.Text;
using System.Text.Json;
using Microsoft.Extensions.AI;

public class GeminiService
{
    private readonly string _apiKey = "YOUR_GEMINI_API_KEY";
    private readonly string _modelName = "gemini-2.0-flash";
    private readonly string _endpoint = "https://generativelanguage.googleapis.com/v1beta/models/";
    private static readonly HttpClient HttpClient = new();

    public GeminiService()
    {
        HttpClient.DefaultRequestHeaders.Clear();
        HttpClient.DefaultRequestHeaders.Add("x-goog-api-key", _apiKey);
    }

    public async Task<string> CompleteAsync(List<ChatMessage> chatMessages)
    {
        var requestUri = $"{_endpoint}{_modelName}:generateContent";
        var contents = chatMessages.Select(m => 
            new ResponseContent(m.Text, m.Role == ChatRole.User ? "user" : "model")
        ).ToList();
        
        var parameters = new GeminiChatParameters
        {
            Contents = contents,
            GenerationConfig = new GenerationConfig { MaxOutputTokens = 2000 }
        };

        var payload = new StringContent(
            JsonSerializer.Serialize(parameters),
            Encoding.UTF8,
            "application/json"
        );

        var response = await HttpClient.PostAsync(requestUri, payload);
        response.EnsureSuccessStatusCode();
        var json = await response.Content.ReadAsStringAsync();
        var result = JsonSerializer.Deserialize<GeminiResponseObject>(json);
        return result?.Candidates?.FirstOrDefault()?.Content?.Parts?.FirstOrDefault()?.Text ?? "";
    }
}
```

**GeminiInferenceService.cs:**
```csharp
using Syncfusion.Maui.SmartComponents;

public class GeminiInferenceService : IChatInferenceService
{
    private readonly GeminiService _geminiService;

    public GeminiInferenceService(GeminiService geminiService)
    {
        _geminiService = geminiService;
    }

    public async Task<string> GenerateResponseAsync(List<ChatMessage> chatMessages)
    {
        return await _geminiService.CompleteAsync(chatMessages);
    }
}
```

## Registration

**MauiProgram.cs:**
```csharp
builder.Services.AddSingleton<GeminiService>();
builder.Services.AddSingleton<IChatInferenceService, GeminiInferenceService>();
```
