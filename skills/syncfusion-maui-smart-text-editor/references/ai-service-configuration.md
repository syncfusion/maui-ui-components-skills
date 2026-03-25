# AI Service Configuration

## Table of Contents
- [Overview](#overview)
- [Azure OpenAI](#azure-openai)
- [OpenAI](#openai)
- [Ollama (Self-Hosted)](#ollama-self-hosted)
- [Choosing a Provider](#choosing-a-provider)

---

## Overview

`SfSmartTextEditor` uses a chat inference service resolved from dependency injection to generate suggestions. The built-in providers (Azure OpenAI, OpenAI, Ollama) are configured by registering an `IChatClient` and calling `ConfigureSyncfusionAIServices()` in `MauiProgram.cs`.

For custom providers (Claude, DeepSeek, Gemini, Groq), implement `IChatInferenceService` instead — see `custom-ai-services.md`.

---

## Azure OpenAI

Use Azure OpenAI when your organization already has an Azure subscription or requires data residency and compliance controls.

**Required NuGet packages:**
```bash
Install-Package Microsoft.Extensions.AI
Install-Package Microsoft.Extensions.AI.OpenAI
Install-Package Azure.AI.OpenAI
```

**MauiProgram.cs:**
```csharp
using Azure.AI.OpenAI;
using Microsoft.Extensions.AI;
using System.ClientModel;
using Syncfusion.Maui.Core.Hosting;
using Syncfusion.Maui.SmartComponents.Hosting;

var builder = MauiApp.CreateBuilder();
builder.UseMauiApp<App>().ConfigureSyncfusionCore();

string azureKey      = "AZURE_OPENAI_KEY";
string azureEndpoint = "AZURE_OPENAI_ENDPOINT";   // e.g. https://my-resource.openai.azure.com/
string deploymentName = "AZURE_OPENAI_MODEL";     // e.g. gpt-4o

AzureOpenAIClient azureClient = new AzureOpenAIClient(
    new Uri(azureEndpoint),
    new ApiKeyCredential(azureKey));

IChatClient chatClient = azureClient.GetChatClient(deploymentName).AsIChatClient();

builder.Services.AddChatClient(chatClient);
builder.ConfigureSyncfusionAIServices();

return builder.Build();
```

> Deploy an Azure OpenAI resource and model via the [Azure Portal](https://portal.azure.com) first. The `azureEndpoint`, `azureKey`, and `deploymentName` values are available in your resource's **Keys and Endpoint** blade.

---

## OpenAI

Use the direct OpenAI API when you don't need Azure infrastructure and want fast setup with a public API key.

**Required NuGet packages:**
```bash
Install-Package Microsoft.Extensions.AI
Install-Package Microsoft.Extensions.AI.OpenAI
```

**MauiProgram.cs:**
```csharp
using Microsoft.Extensions.AI;
using OpenAI;
using System.ClientModel;
using Syncfusion.Maui.Core.Hosting;
using Syncfusion.Maui.SmartComponents.Hosting;

var builder = MauiApp.CreateBuilder();
builder.UseMauiApp<App>().ConfigureSyncfusionCore();

string openAIKey   = "API-KEY";
string openAIModel = "gpt-4o-mini";

var openAIClient = new OpenAIClient(
    new ApiKeyCredential(openAIKey),
    new OpenAIClientOptions
    {
        Endpoint = new Uri("https://api.openai.com/v1/")
    });

IChatClient chatClient = openAIClient.GetChatClient(openAIModel).AsIChatClient();
builder.Services.AddChatClient(chatClient);
builder.ConfigureSyncfusionAIServices();

return builder.Build();
```

> Get your API key from [platform.openai.com/api-keys](https://platform.openai.com/api-keys). Recommended models: `gpt-4o-mini` (cost-efficient) or `gpt-4o` (higher quality).

---

## Ollama (Self-Hosted)

Ollama lets you run open-source models locally — useful for air-gapped environments, privacy-sensitive apps, or offline development.

**Setup steps:**
1. Download and install Ollama from [ollama.com](https://ollama.com)
2. Pull a model: `ollama pull llama3` (or any model from [ollama.com/library](https://ollama.com/library))
3. Ollama runs at `http://localhost:11434` by default

**Required NuGet packages:**
```bash
Install-Package Microsoft.Extensions.AI
Install-Package OllamaSharp
```

**MauiProgram.cs:**
```csharp
using Microsoft.Extensions.AI;
using OllamaSharp;
using Syncfusion.Maui.Core.Hosting;
using Syncfusion.Maui.SmartComponents.Hosting;

var builder = MauiApp.CreateBuilder();
builder.UseMauiApp<App>().ConfigureSyncfusionCore();

string modelName = "llama3";   // Must match the model you pulled
IChatClient chatClient = new OllamaApiClient("http://localhost:11434", modelName);

builder.Services.AddChatClient(chatClient);
builder.ConfigureSyncfusionAIServices();

return builder.Build();
```

> For mobile targets (Android/iOS), replace `localhost` with your machine's LAN IP so the device can reach the Ollama server over the network.

---

## Choosing a Provider

| Provider | Best For | Cost | Privacy |
|----------|----------|------|---------|
| **Azure OpenAI** | Enterprise apps, compliance-sensitive | Pay-per-token | Data stays in Azure region |
| **OpenAI** | Quick setup, personal/prototype projects | Pay-per-token | Data sent to OpenAI |
| **Ollama** | Offline, air-gapped, development | Free (self-hosted) | Fully local |
| **Custom (Claude/Gemini/etc.)** | Specific model preferences | Varies | Depends on provider |

> For custom providers, see `custom-ai-services.md` for implementation details. Custom services implement `IChatInferenceService` directly and do **not** call `ConfigureSyncfusionAIServices()`.
