# Configure Chat Client with Smart Components

Complete guide for configuring Microsoft.Extensions.AI-compatible chat clients with Syncfusion Smart Components.

## Overview

Syncfusion Smart Components use a standardized chat inference service resolved from dependency injection. This guide covers Azure OpenAI, OpenAI, and Ollama configuration.

## Azure OpenAI

### Prerequisites

1. Deploy an Azure OpenAI Service resource and model
2. Obtain:
   - Azure OpenAI API key
   - Azure OpenAI endpoint URL
   - Deployment name (model name)

**Azure Portal:** [portal.azure.com](https://portal.azure.com)  
**Documentation:** [Azure OpenAI Quickstart](https://learn.microsoft.com/en-us/azure/ai-services/openai/quickstart)

### Installation

```bash
dotnet add package Microsoft.Extensions.AI
dotnet add package Microsoft.Extensions.AI.OpenAI
dotnet add package Azure.AI.OpenAI
```

### Configuration

**In MauiProgram.cs:**

```csharp
using Azure.AI.OpenAI;
using Microsoft.Extensions.AI;
using System.ClientModel;
using Syncfusion.Maui.SmartComponents.Hosting;

var builder = MauiApp.CreateBuilder();
builder
    .UseMauiApp<App>()
    .ConfigureSyncfusionCore();

// Azure OpenAI configuration
string azureOpenAIKey = "YOUR_AZURE_OPENAI_KEY";
string azureOpenAIEndpoint = "https://YOUR_RESOURCE.openai.azure.com/";
string azureOpenAIModel = "YOUR_DEPLOYMENT_NAME"; // e.g., "gpt-4o"

AzureOpenAIClient azureOpenAIClient = new AzureOpenAIClient(
    new Uri(azureOpenAIEndpoint),
    new ApiKeyCredential(azureOpenAIKey)
);

IChatClient azureOpenAIChatClient = azureOpenAIClient
    .GetChatClient(azureOpenAIModel)
    .AsIChatClient();

// Register with dependency injection
builder.Services.AddChatClient(azureOpenAIChatClient);

// Configure Syncfusion AI services
builder.ConfigureSyncfusionAIServices();

return builder.Build();
```

### Best Practices

- **Store credentials securely:** Use environment variables or Azure Key Vault
- **Monitor costs:** Azure OpenAI charges per token
- **Choose appropriate model:** GPT-4 for quality, GPT-3.5 for speed/cost
- **Set resource limits:** Configure quotas in Azure portal

## OpenAI

### Prerequisites

1. Create OpenAI account: [platform.openai.com](https://platform.openai.com)
2. Generate API key: [platform.openai.com/api-keys](https://platform.openai.com/api-keys)
3. Choose model (e.g., gpt-4o-mini, gpt-4)

### Installation

```bash
dotnet add package Microsoft.Extensions.AI
dotnet add package Microsoft.Extensions.AI.OpenAI
```

### Configuration

**In MauiProgram.cs:**

```csharp
using Microsoft.Extensions.AI;
using OpenAI;
using Syncfusion.Maui.SmartComponents.Hosting;

var builder = MauiApp.CreateBuilder();
builder
    .UseMauiApp<App>()
    .ConfigureSyncfusionCore();

// OpenAI configuration
string openAIApikey = "YOUR_OPENAI_API_KEY"; // starts with "sk-"
string openAIModel = "gpt-4o-mini"; // or "gpt-4", "gpt-3.5-turbo"

var openAIClient = new OpenAIClient(
    new ApiKeyCredential(openAIApikey),
    new OpenAIClientOptions
    {
        Endpoint = new Uri("https://api.openai.com/v1/")
    }
);

IChatClient openAIChatClient = openAIClient
    .GetChatClient(openAIModel)
    .AsIChatClient();

builder.Services.AddChatClient(openAIChatClient);
builder.ConfigureSyncfusionAIServices();

return builder.Build();
```

### Model Selection

| Model | Speed | Cost | Quality | Best For |
|-------|-------|------|---------|----------|
| gpt-4o | Fast | Medium | High | General purpose |
| gpt-4o-mini | Fastest | Low | Good | Simple completions |
| gpt-4-turbo | Medium | High | Highest | Complex tasks |
| gpt-3.5-turbo | Fast | Lowest | Moderate | Basic tasks |

## Ollama (Self-Hosted)

### Prerequisites

**Step 1: Install Ollama**
- Download from [ollama.com](https://ollama.com)
- Install for your platform (Windows, macOS, Linux)

**Step 2: Install Model**
```bash
# Install a model (examples)
ollama pull llama2:13b
ollama pull mistral:7b
ollama pull codellama:7b
```

**Step 3: Verify Installation**
```bash
# Check Ollama is running
curl http://localhost:11434

# List installed models
ollama list
```

### Installation (NuGet)

```bash
dotnet add package Microsoft.Extensions.AI
dotnet add package OllamaSharp
```

### Configuration

**In MauiProgram.cs:**

```csharp
using Microsoft.Extensions.AI;
using OllamaSharp;
using Syncfusion.Maui.SmartComponents.Hosting;

var builder = MauiApp.CreateBuilder();
builder
    .UseMauiApp<App>()
    .ConfigureSyncfusionCore();

// Ollama configuration
string modelName = "llama2:13b"; // or "mistral:7b", "codellama:7b"
string ollamaEndpoint = "http://localhost:11434";

IChatClient chatClient = new OllamaApiClient(
    ollamaEndpoint,
    modelName
);

builder.Services.AddChatClient(chatClient);
builder.ConfigureSyncfusionAIServices();

return builder.Build();
```

### Benefits

- **No API costs** - Runs entirely locally
- **Privacy** - Data never leaves your machine
- **Offline** - Works without internet
- **Customizable** - Use any compatible model

### Limitations

- **Hardware requirements** - Needs GPU for best performance
- **Model size** - Large models require significant disk space
- **Performance** - Slower than cloud APIs

### Popular Models

| Model | Size | Use Case |
|-------|------|----------|
| llama2:7b | ~4 GB | General purpose, fast |
| llama2:13b | ~7 GB | Better quality, slower |
| mistral:7b | ~4 GB | Efficient, good quality |
| codellama:7b | ~4 GB | Code generation |
| phi:2.7b | ~1.6 GB | Lightweight, fast |

## ConfigureSyncfusionAIServices()

This extension method registers Syncfusion's AI infrastructure:

```csharp
builder.ConfigureSyncfusionAIServices();
```

**What it does:**
- Registers `IChatInferenceService` implementation
- Configures Smart Components to use registered chat client
- Sets up dependency injection for AI features

**Must be called after:**
- `builder.Services.AddChatClient(chatClient)`

**Before using:**
- Any Syncfusion Smart Component (SfSmartTextEditor, SfAIAssistView)

## Environment-Specific Configuration

### Using appsettings.json

```json
{
  "AI": {
    "Provider": "OpenAI",
    "OpenAI": {
      "ApiKey": "sk-...",
      "Model": "gpt-4o-mini"
    },
    "Azure": {
      "ApiKey": "...",
      "Endpoint": "https://....openai.azure.com/",
      "DeploymentName": "gpt-4"
    }
  }
}
```

```csharp
var config = builder.Configuration;
var provider = config["AI:Provider"];

switch (provider)
{
    case "OpenAI":
        var key = config["AI:OpenAI:ApiKey"];
        var model = config["AI:OpenAI:Model"];
        // Configure OpenAI
        break;
    case "Azure":
        // Configure Azure
        break;
}
```

### Using Environment Variables

```csharp
// Read from environment
string apiKey = Environment.GetEnvironmentVariable("OPENAI_API_KEY");
string model = Environment.GetEnvironmentVariable("OPENAI_MODEL") ?? "gpt-4o-mini";
```

**Set in terminal:**
```bash
export OPENAI_API_KEY="sk-..."
export OPENAI_MODEL="gpt-4o-mini"
```

## Troubleshooting

### No Suggestions Displayed

**Check:**
1. `ConfigureSyncfusionAIServices()` is called
2. Chat client is registered correctly
3. API key is valid
4. Network connectivity (for cloud services)

### Authentication Errors (401)

**Solutions:**
- Verify API key is correct
- Check key hasn't expired
- Ensure proper endpoint URL
- Verify account has active subscription

### Connection Errors

**For cloud services:**
- Check internet connection
- Verify firewall isn't blocking requests
- Ensure correct endpoint URL

**For Ollama:**
- Verify Ollama is running: `curl http://localhost:11434`
- Check model is installed: `ollama list`
- Restart Ollama service

## Related Topics

- [Custom AI Service](custom-ai-service.md) - Implement your own AI provider
- [Claude Integration](claude-service.md) - Use Anthropic Claude
- [Gemini Integration](gemini-service.md) - Use Google Gemini
