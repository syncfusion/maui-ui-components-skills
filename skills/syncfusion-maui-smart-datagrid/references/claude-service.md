# Claude AI Integration

Integrate Anthropic Claude AI with Syncfusion Smart Components.

## Setup

1. **Create Account:** [console.anthropic.com](https://console.anthropic.com)
2. **Get API Key:** [API Keys](https://console.anthropic.com/settings/keys)
3. **Model Docs:** [Claude Models](https://docs.anthropic.com/claude/docs/models-overview)

## Implementation

**ClaudeModels.cs:**
```csharp
public class ClaudeChatRequest
{
    public string Model { get; set; }
    public int Max_tokens { get; set; }
    public List<ClaudeMessage> Messages { get; set; }
}

public class ClaudeMessage
{
    public string Role { get; set; }
    public string Content { get; set; }
}

public class ClaudeChatResponse
{
    public List<ClaudeContentBlock> Content { get; set; }
}

public class ClaudeContentBlock
{
    public string Text { get; set; }
}
```

**ClaudeAIService.cs:**
```csharp
using System.Net.Http;
using System.Text;
using System.Text.Json;
using Microsoft.Extensions.AI;

public class ClaudeAIService
{
    private readonly string _apiKey = "YOUR_CLAUDE_API_KEY";
    private readonly string _modelName = "claude-3-5-sonnet-20241022";
    private readonly string _endpoint = "YOUR_END_POINT_KEY";  // For e.g -"https://api.anthropic.com/v1/messages"
    private static readonly HttpClient HttpClient = new();

    public ClaudeAIService()
    {
        HttpClient.DefaultRequestHeaders.Clear();
        HttpClient.DefaultRequestHeaders.Add("x-api-key", _apiKey);
        HttpClient.DefaultRequestHeaders.Add("anthropic-version", "2023-06-01");
    }

    public async Task<string> CompleteAsync(List<ChatMessage> chatMessages)
    {
        var requestBody = new ClaudeChatRequest
        {
            Model = _modelName,
            Max_tokens = 1000,
            Messages = chatMessages.Select(m => new ClaudeMessage
            {
                Role = m.Role == ChatRole.User ? "user" : "assistant",
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
        var responseObject = JsonSerializer.Deserialize<ClaudeChatResponse>(responseString);
        return responseObject?.Content?.FirstOrDefault()?.Text ?? "";
    }
}
```

**ClaudeInferenceService.cs:**
```csharp
using Syncfusion.Maui.SmartComponents;

public class ClaudeInferenceService : IChatInferenceService
{
    private readonly ClaudeAIService _claudeService;

    public ClaudeInferenceService(ClaudeAIService claudeService)
    {
        _claudeService = claudeService;
    }

    public async Task<string> GenerateResponseAsync(List<ChatMessage> chatMessages)
    {
        return await _claudeService.CompleteAsync(chatMessages);
    }
}
```

## Registration

**MauiProgram.cs:**
```csharp
using Syncfusion.Maui.Core.Hosting;
using Syncfusion.Maui.SmartComponents;

builder.Services.AddSingleton<ClaudeAIService>();
builder.Services.AddSingleton<IChatInferenceService, ClaudeInferenceService>();
```

## Models

- claude-3-5-sonnet-20241022 (recommended)
- claude-3-opus-20240229 (most capable)
- claude-3-haiku-20240307 (fastest)
