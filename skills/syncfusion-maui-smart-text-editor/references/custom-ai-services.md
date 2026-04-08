# Custom AI Services

## Table of Contents
- [IChatInferenceService Interface](#ichatinferenceservice-interface)
- [Implementation Steps](#implementation-steps)
- [Complete Example](#complete-example)
- [Troubleshooting](#troubleshooting)

---

## IChatInferenceService Interface

When using a provider not supported by `Microsoft.Extensions.AI` out of the box, implement `IChatInferenceService` to bridge the editor and your AI backend. The interface has a single method:

```csharp
using Syncfusion.Maui.SmartComponents;

public interface IChatInferenceService
{
    Task<string> GenerateResponseAsync(List<ChatMessage> chatMessages);
}
```

- **`chatMessages`** — contains the user's text and context history
- **Return value** — the AI-generated completion string inserted as a suggestion

**Key difference from built-in providers:** When you register an `IChatInferenceService` implementation, you do **not** call `ConfigureSyncfusionAIServices()` in `MauiProgram.cs` — the custom service takes over entirely.

**Registration pattern (all custom services):**
```csharp
builder.Services.AddSingleton<IChatInferenceService, YourCustomService>();
```

---

## Implementation Steps

### Step 1: Create AI Service Class

Implement your AI provider's API communication:

```csharp
using Microsoft.Extensions.AI;

public class MockAIService
{
    public async Task<string> CompleteAsync(List<ChatMessage> chatMessages)
    {
        // Implement your AI API call here
        // Example: Mock response
        await Task.Delay(500); // Simulate API delay
        
        var lastMessage = chatMessages.LastOrDefault();
        return $"AI Response to: {lastMessage?.Text}";
    }
}
```

### Step 2: Create Inference Service Wrapper

Implement `IChatInferenceService`:

```csharp
using Syncfusion.Maui.SmartComponents;

public class MockInferenceService : IChatInferenceService
{
    private readonly MockAIService _aiService;
    
    public MockInferenceService(MockAIService aiService)
    {
        _aiService = aiService;
    }
    
    public async Task<string> GenerateResponseAsync(List<ChatMessage> chatMessages)
    {
        return await _aiService.CompleteAsync(chatMessages);
    }
}
```

### Step 3: Register Services

In `MauiProgram.cs`:

```csharp
using Syncfusion.Maui.Core.Hosting;
using Syncfusion.Maui.SmartComponents;

var builder = MauiApp.CreateBuilder();
builder
    .UseMauiApp<App>()
    .ConfigureSyncfusionCore();

// Register custom AI services
builder.Services.AddSingleton<MockAIService>();
builder.Services.AddSingleton<IChatInferenceService, MockInferenceService>();

return builder.Build();
```

## Complete Example

**Full custom implementation:**

```csharp
// CustomAIService.cs
using System.Net.Http;
using System.Text;
using System.Text.Json;
using Microsoft.Extensions.AI;

public class CustomAIService
{
    private readonly string _apiKey;
    private readonly string _endpoint;
    private static readonly HttpClient HttpClient = new();
    
    public CustomAIService(string apiKey, string endpoint)
    {
        _apiKey = apiKey;
        _endpoint = endpoint;
    }
    
    public async Task<string> CompleteAsync(List<ChatMessage> messages)
    {
        var request = new
        {
            messages = messages.Select(m => new 
            {
                role = m.Role == ChatRole.User ? "user" : "assistant",
                content = m.Text
            }),
            max_tokens = 1000
        };
        
        var content = new StringContent(
            JsonSerializer.Serialize(request),
            Encoding.UTF8,
            "application/json"
        );
        
        HttpClient.DefaultRequestHeaders.Clear();
        HttpClient.DefaultRequestHeaders.Add("Authorization", $"Bearer {_apiKey}");
        
        var response = await HttpClient.PostAsync(_endpoint, content);
        response.EnsureSuccessStatusCode();
        
        var responseString = await response.Content.ReadAsStringAsync();
        var result = JsonSerializer.Deserialize<CustomResponse>(responseString);
        
        return result?.Choices?.FirstOrDefault()?.Message?.Content ?? "";
    }
}

public class CustomMessage
{
    public string Role { get; set; }
    public string Content { get; set; }
}

public class CustomChatRequest
{
    public string Model { get; set; }
    public float Max_tokens { get; set; }
    public List<CustomMessage>? Messages { get; set; }
}

public class CustomChoice
{
	public CustomMessage Message { get; set; }
}

public class CustomChatResponse
{
	public List<CustomChoice> Choices { get; set; }
}

// CustomInferenceService.cs
using Syncfusion.Maui.SmartComponents;

public class CustomInferenceService : IChatInferenceService
{
    private readonly CustomAIService _customService;
    
    public CustomInferenceService(CustomAIService customService)
    {
        _customService = customService;
    }
    
    public async Task<string> GenerateResponseAsync(List<ChatMessage> chatMessages)
    {
        return await _customService.CompleteAsync(chatMessages);
    }
}
```

## Troubleshooting

**No suggestions appear after registering a custom service:**
- Confirm `IChatInferenceService` is registered as a singleton in `MauiProgram.cs`
- Ensure `GenerateResponseAsync` returns a non-empty string
- Check that you have **not** called `ConfigureSyncfusionAIServices()` alongside the custom service — it is not needed and may conflict
- Add logging inside `GenerateResponseAsync` to confirm it is being called and getting a valid response from your API

**Suggestions appear but are irrelevant:**
- Review the `UserRole` property — a more specific role description produces better suggestions
- Verify the AI service is receiving the full `chatMessages` list, not just the latest message
