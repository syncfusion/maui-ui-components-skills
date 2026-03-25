# Groq AI Integration

Integrate Groq (OpenAI-compatible) with Syncfusion Smart Components.

## Setup

1. **Console:** [console.groq.com](https://console.groq.com)
2. **Models:** [Groq Models](https://console.groq.com/docs/models)
3. **API:** OpenAI-compatible endpoint

## Implementation

**GroqModels.cs:**
```csharp
public class Message
{
    public string Role { get; set; }
    public string Content { get; set; }
}

public class GroqChatParameters
{
    public string Model { get; set; }
    public List<Message> Messages { get; set; }
}

public class GroqResponseObject
{
    public List<Choice> Choices { get; set; }
}

public class Choice
{
    public Message Message { get; set; }
}
```

**GroqService.cs:**
```csharp
using Microsoft.Extensions.AI;
using System.Net.Http;
using System.Text;
using System.Text.Json;

public class GroqService
{
    private readonly string _apiKey = "YOUR_GROQ_API_KEY";
    private readonly string _modelName = "llama3-8b-8192";
    private readonly string _endpoint = "https://api.groq.com/openai/v1/chat/completions";
    private static readonly HttpClient HttpClient = new();

    public GroqService()
    {
        HttpClient.DefaultRequestHeaders.Clear();
        HttpClient.DefaultRequestHeaders.Add("Authorization", $"Bearer {_apiKey}");
    }

    public async Task<string> CompleteAsync(List<ChatMessage> chatMessages)
    {
        var requestPayload = new GroqChatParameters
        {
            Model = _modelName,
            Messages = chatMessages.Select(m => new Message
            {
                Role = m.Role == ChatRole.User ? "user" : "assistant",
                Content = m.Text
            }).ToList()
        };

        var content = new StringContent(
            JsonSerializer.Serialize(requestPayload),
            Encoding.UTF8,
            "application/json"
        );

        var response = await HttpClient.PostAsync(_endpoint, content);
        response.EnsureSuccessStatusCode();
        var responseString = await response.Content.ReadAsStringAsync();
        var responseObject = JsonSerializer.Deserialize<GroqResponseObject>(responseString);
        return responseObject?.Choices?.FirstOrDefault()?.Message?.Content ?? "";
    }
}
```

**GroqInferenceService.cs:**
```csharp
using Syncfusion.Maui.SmartComponents;

public class GroqInferenceService : IChatInferenceService
{
    private readonly GroqService _groqService;

    public GroqInferenceService(GroqService groqService)
    {
        _groqService = groqService;
    }

    public async Task<string> GenerateResponseAsync(List<ChatMessage> chatMessages)
    {
        return await _groqService.CompleteAsync(chatMessages);
    }
}
```

## Registration

**MauiProgram.cs:**
```csharp
builder.Services.AddSingleton<GroqService>();
builder.Services.AddSingleton<IChatInferenceService, GroqInferenceService>();
```

## Models

- llama3-8b-8192 (fast, efficient)
- mixtral-8x7b-32768 (large context)
- gemma-7b-it (instruction-tuned)
