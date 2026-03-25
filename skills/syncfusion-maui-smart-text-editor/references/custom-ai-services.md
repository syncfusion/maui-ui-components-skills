# Custom AI Services

## Table of Contents
- [IChatInferenceService Interface](#ichatinferenceservice-interface)
- [Claude AI](#claude-ai)
- [DeepSeek](#deepseek)
- [Gemini](#gemini)
- [Groq](#groq)
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

## Claude AI

Use Anthropic's Claude models for high-quality, context-aware completions.

**Setup:**
1. Create an account at [console.anthropic.com](https://console.anthropic.com)
2. Generate an API key under **API Keys**
3. Review available models at [docs.anthropic.com/claude/docs/models-overview](https://docs.anthropic.com/claude/docs/models-overview)

### Request / Response Models (`ClaudeModels.cs`)

```csharp
public class ClaudeChatRequest
{
    public string? Model { get; set; }
    public int Max_tokens { get; set; }
    public List<ClaudeMessage>? Messages { get; set; }
    public List<string>? Stop_sequences { get; set; }
}

public class ClaudeMessage
{
    public string? Role { get; set; }
    public string? Content { get; set; }
}

public class ClaudeChatResponse
{
    public List<ClaudeContentBlock>? Content { get; set; }
}

public class ClaudeContentBlock
{
    public string? Text { get; set; }
}
```

### AI Service (`ClaudeAIService.cs`)

```csharp
using System.Net;
using System.Text;
using System.Text.Json;
using Microsoft.Extensions.AI;

public class ClaudeAIService
{
    private readonly string _apiKey    = "YOUR_CLAUDE_API_KEY";
    private readonly string _modelName = "claude-3-5-sonnet-20241022";
    private readonly string _endpoint  = "https://api.anthropic.com/v1/messages";

    private static readonly HttpClient HttpClient = new(new SocketsHttpHandler
    {
        PooledConnectionLifetime = TimeSpan.FromMinutes(30),
        EnableMultipleHttp2Connections = true
    }) { DefaultRequestVersion = HttpVersion.Version20 };

    private static readonly JsonSerializerOptions JsonOptions = new()
    {
        PropertyNamingPolicy = JsonNamingPolicy.CamelCase
    };

    public ClaudeAIService()
    {
        if (!HttpClient.DefaultRequestHeaders.Contains("x-api-key"))
        {
            HttpClient.DefaultRequestHeaders.Clear();
            HttpClient.DefaultRequestHeaders.Add("x-api-key", _apiKey);
            HttpClient.DefaultRequestHeaders.Add("anthropic-version", "2023-06-01");
        }
    }

    public async Task<string> CompleteAsync(List<ChatMessage> chatMessages)
    {
        var request = new ClaudeChatRequest
        {
            Model       = _modelName,
            Max_tokens  = 1000,
            Messages    = chatMessages.Select(m => new ClaudeMessage
            {
                Role    = m.Role == ChatRole.User ? "user" : "assistant",
                Content = m.Text
            }).ToList(),
            Stop_sequences = new List<string> { "END_INSERTION", "NEED_INFO", "END_RESPONSE" }
        };

        var content = new StringContent(
            JsonSerializer.Serialize(request, JsonOptions), Encoding.UTF8, "application/json");

        var response = await HttpClient.PostAsync(_endpoint, content);
        response.EnsureSuccessStatusCode();
        var json = await response.Content.ReadAsStringAsync();
        var result = JsonSerializer.Deserialize<ClaudeChatResponse>(json, JsonOptions);
        return result?.Content?.FirstOrDefault()?.Text ?? "No response from Claude.";
    }
}
```

### Inference Service + Registration

```csharp
// ClaudeInferenceService.cs
using Syncfusion.Maui.SmartComponents;

public class ClaudeInferenceService : IChatInferenceService
{
    private readonly ClaudeAIService _claude;
    public ClaudeInferenceService(ClaudeAIService claude) => _claude = claude;

    public Task<string> GenerateResponseAsync(List<ChatMessage> messages)
        => _claude.CompleteAsync(messages);
}
```

```csharp
// MauiProgram.cs — no ConfigureSyncfusionAIServices() needed
builder.Services.AddSingleton<ClaudeAIService>();
builder.Services.AddSingleton<IChatInferenceService, ClaudeInferenceService>();
```

---

## DeepSeek

DeepSeek offers cost-effective models via an OpenAI-compatible Chat Completions endpoint.

**Setup:** Create an account at [platform.deepseek.com](https://platform.deepseek.com) and generate an API key.

### Request / Response Models (`DeepSeekModels.cs`)

```csharp
public class DeepSeekMessage      { public string? Role { get; set; } public string? Content { get; set; } }
public class DeepSeekChatRequest  { public string? Model { get; set; } public float Temperature { get; set; } public List<DeepSeekMessage>? Messages { get; set; } }
public class DeepSeekChatResponse { public List<DeepSeekChoice>? Choices { get; set; } }
public class DeepSeekChoice       { public DeepSeekMessage? Message { get; set; } }
```

### AI Service + Registration

```csharp
// DeepSeekAIService.cs
using Microsoft.Extensions.AI;
using System.Net; using System.Text; using System.Text.Json;

public class DeepSeekAIService
{
    private readonly string _apiKey    = "YOUR_DEEPSEEK_KEY";
    private readonly string _modelName = "deepseek-chat";
    private readonly string _endpoint  = "https://api.deepseek.com/v1/chat/completions";

    private static readonly HttpClient HttpClient = new(new SocketsHttpHandler
        { PooledConnectionLifetime = TimeSpan.FromMinutes(30) })
        { DefaultRequestVersion = HttpVersion.Version20 };
    private static readonly JsonSerializerOptions JsonOptions = new()
        { PropertyNamingPolicy = JsonNamingPolicy.CamelCase };

    public DeepSeekAIService()
    {
        if (!HttpClient.DefaultRequestHeaders.Contains("Authorization"))
        {
            HttpClient.DefaultRequestHeaders.Clear();
            HttpClient.DefaultRequestHeaders.Add("Authorization", $"Bearer {_apiKey}");
        }
    }

    public async Task<string> CompleteAsync(List<ChatMessage> messages)
    {
        var request = new DeepSeekChatRequest
        {
            Model       = _modelName,
            Temperature = 0.7f,
            Messages    = messages.Select(m => new DeepSeekMessage
            {
                Role    = m.Role == ChatRole.User ? "user" : "system",
                Content = m.Text
            }).ToList()
        };
        var payload  = new StringContent(JsonSerializer.Serialize(request, JsonOptions), Encoding.UTF8, "application/json");
        var response = await HttpClient.PostAsync(_endpoint, payload);
        response.EnsureSuccessStatusCode();
        var json   = await response.Content.ReadAsStringAsync();
        var result = JsonSerializer.Deserialize<DeepSeekChatResponse>(json, JsonOptions);
        return result?.Choices?.FirstOrDefault()?.Message?.Content ?? "No response from DeepSeek.";
    }
}
```

```csharp
// DeepSeekInferenceService.cs + MauiProgram.cs registration
public class DeepSeekInferenceService : IChatInferenceService
{
    private readonly DeepSeekAIService _service;
    public DeepSeekInferenceService(DeepSeekAIService service) => _service = service;
    public Task<string> GenerateResponseAsync(List<ChatMessage> messages) => _service.CompleteAsync(messages);
}

// MauiProgram.cs
builder.Services.AddSingleton<DeepSeekAIService>();
builder.Services.AddSingleton<IChatInferenceService, DeepSeekInferenceService>();
```

---

## Gemini

Google's Gemini models via the Generative Language API. Includes configurable safety settings.

**Setup:** Get an API key from [Google AI Studio](https://ai.google.dev/gemini-api/docs/api-key).

### AI Service + Registration

```csharp
// GeminiService.cs
using System.Net; using System.Text; using System.Text.Json;
using Microsoft.Extensions.AI;

public class GeminiService
{
    private readonly string _apiKey    = "YOUR_GEMINI_KEY";
    private readonly string _modelName = "gemini-2.0-flash";
    private readonly string _endpoint  = "https://generativelanguage.googleapis.com/v1beta/models/";

    private static readonly HttpClient HttpClient = new(new SocketsHttpHandler
        { PooledConnectionLifetime = TimeSpan.FromMinutes(30) })
        { DefaultRequestVersion = HttpVersion.Version20 };
    private static readonly JsonSerializerOptions JsonOptions = new()
        { PropertyNamingPolicy = JsonNamingPolicy.CamelCase };

    public GeminiService()
    {
        HttpClient.DefaultRequestHeaders.Clear();
        HttpClient.DefaultRequestHeaders.Add("x-goog-api-key", _apiKey);
    }

    public async Task<string> CompleteAsync(List<ChatMessage> messages)
    {
        var requestUri = $"{_endpoint}{_modelName}:generateContent";
        var contents   = messages.Select(m => new
        {
            role  = m.Role == ChatRole.User ? "user" : "model",
            parts = new[] { new { text = m.Text } }
        }).ToList();

        var parameters = new
        {
            contents,
            generationConfig = new { maxOutputTokens = 2000, stopSequences = new[] { "END_INSERTION", "NEED_INFO" } },
            safetySettings   = new[]
            {
                new { category = "HARM_CATEGORY_HARASSMENT",        threshold = "BLOCK_ONLY_HIGH" },
                new { category = "HARM_CATEGORY_HATE_SPEECH",       threshold = "BLOCK_ONLY_HIGH" },
                new { category = "HARM_CATEGORY_SEXUALLY_EXPLICIT", threshold = "BLOCK_ONLY_HIGH" },
                new { category = "HARM_CATEGORY_DANGEROUS_CONTENT", threshold = "BLOCK_ONLY_HIGH" }
            }
        };

        var payload  = new StringContent(JsonSerializer.Serialize(parameters, JsonOptions), Encoding.UTF8, "application/json");
        var response = await HttpClient.PostAsync(requestUri, payload);
        response.EnsureSuccessStatusCode();
        var json     = await response.Content.ReadAsStringAsync();
        using var doc = JsonDocument.Parse(json);
        return doc.RootElement
            .GetProperty("candidates")[0]
            .GetProperty("content")
            .GetProperty("parts")[0]
            .GetProperty("text")
            .GetString() ?? "No response from Gemini.";
    }
}
```

```csharp
// MauiProgram.cs
public class GeminiInferenceService : IChatInferenceService
{
    private readonly GeminiService _service;
    public GeminiInferenceService(GeminiService service) => _service = service;
    public Task<string> GenerateResponseAsync(List<ChatMessage> messages) => _service.CompleteAsync(messages);
}

builder.Services.AddSingleton<GeminiService>();
builder.Services.AddSingleton<IChatInferenceService, GeminiInferenceService>();
```

---

## Groq

Groq delivers very low-latency inference using an OpenAI-compatible Chat Completions API.

**Setup:** Create an API key at [console.groq.com](https://console.groq.com). Review available models at [console.groq.com/docs/models](https://console.groq.com/docs/models).

### AI Service + Registration

```csharp
// GroqService.cs
using Microsoft.Extensions.AI;
using System.Net; using System.Text; using System.Text.Json;

public class GroqService
{
    private readonly string _apiKey    = "YOUR_GROQ_KEY";
    private readonly string _modelName = "llama3-8b-8192";
    private readonly string _endpoint  = "https://api.groq.com/openai/v1/chat/completions";

    private static readonly HttpClient HttpClient = new(new SocketsHttpHandler
        { PooledConnectionLifetime = TimeSpan.FromMinutes(30) })
        { DefaultRequestVersion = HttpVersion.Version20 };
    private static readonly JsonSerializerOptions JsonOptions = new()
        { PropertyNamingPolicy = JsonNamingPolicy.CamelCase };

    public GroqService()
    {
        if (!HttpClient.DefaultRequestHeaders.Contains("Authorization"))
        {
            HttpClient.DefaultRequestHeaders.Clear();
            HttpClient.DefaultRequestHeaders.Add("Authorization", $"Bearer {_apiKey}");
        }
    }

    public async Task<string> CompleteAsync(List<ChatMessage> messages)
    {
        var request = new
        {
            model    = _modelName,
            messages = messages.Select(m => new
            {
                role    = m.Role == ChatRole.User ? "user" : "assistant",
                content = m.Text
            }).ToList(),
            stop = new[] { "END_INSERTION", "NEED_INFO", "END_RESPONSE" }
        };
        var payload  = new StringContent(JsonSerializer.Serialize(request, JsonOptions), Encoding.UTF8, "application/json");
        var response = await HttpClient.PostAsync(_endpoint, payload);
        response.EnsureSuccessStatusCode();
        var json     = await response.Content.ReadAsStringAsync();
        using var doc = JsonDocument.Parse(json);
        return doc.RootElement
            .GetProperty("choices")[0]
            .GetProperty("message")
            .GetProperty("content")
            .GetString() ?? "No response from Groq.";
    }
}
```

```csharp
// MauiProgram.cs
public class GroqInferenceService : IChatInferenceService
{
    private readonly GroqService _service;
    public GroqInferenceService(GroqService service) => _service = service;
    public Task<string> GenerateResponseAsync(List<ChatMessage> messages) => _service.CompleteAsync(messages);
}

builder.Services.AddSingleton<GroqService>();
builder.Services.AddSingleton<IChatInferenceService, GroqInferenceService>();
```

---

## Troubleshooting

**No suggestions appear after registering a custom service:**
- Confirm `IChatInferenceService` is registered as a singleton in `MauiProgram.cs`
- Ensure `GenerateResponseAsync` returns a non-empty string
- Check that you have **not** called `ConfigureSyncfusionAIServices()` alongside the custom service — it is not needed and may conflict
- Add logging inside `GenerateResponseAsync` to confirm it is being called and getting a valid response from your API

**Suggestions appear but are irrelevant:**
- Review the `UserRole` property — a more specific role description produces better suggestions
- Verify the AI service is receiving the full `chatMessages` list, not just the latest message
