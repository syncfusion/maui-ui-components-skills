# Custom AI Service Integration

Guide for implementing custom AI services using the `IChatInferenceService` interface.

## Overview

The `IChatInferenceService` interface provides a standardized way to integrate any AI service with Syncfusion Smart Components.

## IChatInferenceService Interface

```csharp
using Syncfusion.Maui.SmartComponents;

public interface IChatInferenceService
{
    Task<string> GenerateResponseAsync(List<ChatMessage> chatMessages);
}
```

**Parameters:**
- `chatMessages` - List of messages with role (User/Assistant) and text

**Returns:**
- AI-generated response as string

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

public class CustomResponse
{
    public List<Choice> Choices { get; set; }
}

public class Choice
{
    public Message Message { get; set; }
}

public class Message
{
    public string Content { get; set; }
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

## Testing Custom Integration

### Step 1: Create Test Page

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:smartComponents="clr-namespace:Syncfusion.Maui.SmartComponents;assembly=Syncfusion.Maui.SmartComponents"
             x:Class="YourApp.TestPage">
    
    <smartComponents:SfSmartTextEditor
        Placeholder="Type to test AI suggestions..."
        SuggestionDisplayMode="Inline" />
</ContentPage>
```

### Step 2: Run and Verify

1. Type text in the editor
2. AI suggestions should appear
3. Check for errors in debug output

### Step 3: Debug Issues

```csharp
public async Task<string> GenerateResponseAsync(List<ChatMessage> chatMessages)
{
    try
    {
        System.Diagnostics.Debug.WriteLine($"Received {chatMessages.Count} messages");
        var response = await _customService.CompleteAsync(chatMessages);
        System.Diagnostics.Debug.WriteLine($"Response: {response}");
        return response;
    }
    catch (Exception ex)
    {
        System.Diagnostics.Debug.WriteLine($"Error: {ex.Message}");
        return "Error generating response";
    }
}
```

## Best Practices

1. **Error handling** - Wrap API calls in try-catch
2. **Timeouts** - Configure HttpClient timeout
3. **Retries** - Implement retry logic for transient failures
4. **Caching** - Cache responses for identical requests
5. **Logging** - Log requests/responses for debugging
6. **Rate limiting** - Respect API rate limits

## Troubleshooting

### No Response Generated

**Check:**
- `GenerateResponseAsync` is being called
- No exceptions are thrown
- API returns valid response
- Response parsing is correct

### Slow Performance

**Solutions:**
- Increase HttpClient timeout
- Use faster AI model
- Implement response streaming
- Cache common responses

## Related Topics

- [Configure Chat Client](configure-chat-client.md) - Standard providers
- [Claude Integration](claude-service.md) - Complete Claude example
