# AI Service Configuration

## Table of Contents
- [Overview](#overview)
- [Azure OpenAI](#azure-openai)

---

## Overview

`SfSmartTextEditor` uses a chat inference service resolved from dependency injection to generate suggestions. The built-in providers (eg: Azure OpenAI) are configured by registering an `IChatClient` and calling `ConfigureSyncfusionAIServices()` in `MauiProgram.cs`.

For custom providers — see `custom-ai-services.md`.

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
string azureEndpoint = "AZURE_OPENAI_ENDPOINT";
string deploymentName = "AZURE_OPENAI_MODEL";

AzureOpenAIClient azureClient = new AzureOpenAIClient(
    new Uri(azureEndpoint),
    new ApiKeyCredential(azureKey));

IChatClient chatClient = azureClient.GetChatClient(deploymentName).AsIChatClient();

builder.Services.AddChatClient(chatClient);
builder.ConfigureSyncfusionAIServices();

return builder.Build();
```

> Deploy an Azure OpenAI resource and model via the `Azure Portal` first. The `azureEndpoint`, `azureKey`, and `deploymentName` values are available in your resource's **Keys and Endpoint** blade.

---

> For custom providers, see `custom-ai-services.md` for implementation details.
