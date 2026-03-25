# DeepSeek AI Integration

Integrate DeepSeek AI with Syncfusion Smart Components.

## Setup

1. **Platform:** [platform.deepseek.com](https://platform.deepseek.com)
2. **API Keys:** [API Keys](https://platform.deepseek.com/api_keys)
3. **Pricing:** [Model Docs](https://api-docs.deepseek.com/quick_start/pricing)

## Implementation

**DeepSeekModels.cs:**
```csharp
public class DeepSeekMessage
{
    public string Role { get; set; }
    public string Content { get; set; }
}

public class DeepSeekChatRequest
{
    public string Model { get; set; }
    public float Temperature { get; set; }
    public List<DeepSeekMessage> Messages { get; set; }
}

public class DeepSeekChatResponse
{
    public List<DeepSeekChoice> Choices { get; set; }
}

public class DeepSeekChoice
{
    public DeepSeekMessage Message { get; set; }
}
```

**DeepSeekAIService.cs:**
```csharp
using Microsoft.Extensions.AI;
using System.Net.Http;
using System.Text;
using System.Text.Json;

public class DeepSeekAIService
{
    private readonly string _apiKey = "YOUR_DEEPSEEK_API_KEY";
    private readonly string _modelName = "deepseek-chat";
    private readonly string _endpoint = "https://api.deepseek.com/v1/chat/completions";
    private static readonly HttpClient HttpClient = new();

    public DeepSeekAIService()
    {
        HttpClient.DefaultRequestHeaders.Clear();
        HttpClient.DefaultRequestHeaders.Add("Authorization", $"Bearer {_apiKey}");
    }

    public async Task<string> CompleteAsync(List<ChatMessage> chatMessages)
    {
        var requestBody = new DeepSeekChatRequest
        {
            Model = _modelName,
            Temperature = 0.7f,
            Messages = chatMessages.Select(m => new DeepSeekMessage
            {
                Role = m.Role == ChatRole.User ? "user" : "system",
                Content = m.Text
            }).ToList()
        };

        var content = new StringContent(
            JsonSerializer.Serialize(requestBody),
            Encoding.UTF8,
            "application/json"
        );

        var response = await HttpClient.PostAsync(_endpoint, content);
        response.EnsureSuccessStatusCode();
        var responseString = await response.Content.ReadAsStringAsync();
        var responseObject = JsonSerializer.Deserialize<DeepSeekChatResponse>(responseString);
        return responseObject?.Choices?.FirstOrDefault()?.Message?.Content ?? "";
    }
}
```

**DeepSeekInferenceService.cs:**
```csharp
using Syncfusion.Maui.SmartComponents;

public class DeepSeekInferenceService : IChatInferenceService
{
    private readonly DeepSeekAIService _deepSeekService;

    public DeepSeekInferenceService(DeepSeekAIService deepSeekService)
    {
        _deepSeekService = deepSeekService;
    }

    public async Task<string> GenerateResponseAsync(List<ChatMessage> chatMessages)
    {
        return await _deepSeekService.CompleteAsync(chatMessages);
    }
}
```

## Registration

**MauiProgram.cs:**
```csharp
builder.Services.AddSingleton<DeepSeekAIService>();
builder.Services.AddSingleton<IChatInferenceService, DeepSeekInferenceService>();
```
