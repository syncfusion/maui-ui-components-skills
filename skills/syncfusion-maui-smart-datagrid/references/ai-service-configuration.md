# AI Service Configuration

## Overview

The Smart DataGrid requires an AI service to process natural language commands. This guide covers configuring Azure OpenAI and alternative AI providers with Syncfusion Smart Components.

## Azure OpenAI Service Setup

Azure OpenAI is the recommended AI provider for Smart DataGrid operations. It provides reliable, enterprise-grade language processing.

### Prerequisites

1. **Azure Subscription** - Active Azure account
2. **OpenAI Resource** - Created in Azure portal
3. **Deployment** - GPT model deployed (e.g., gpt-4, gpt-35-turbo)
4. **Credentials** - API key and endpoint URL

### Step 1: Get Azure Credentials

From Azure Portal:

1. Go to **Azure OpenAI** resource
2. Click **Keys and Endpoint** in left sidebar
3. Copy **Key 1** (API key)
4. Copy **Endpoint** (URL like `https://your-resource.openai.azure.com/`)
5. Get **Deployment name** from your deployed model

### Step 2: Configure in MauiProgram.cs

```csharp
using Microsoft.Maui;
using Microsoft.Maui.Hosting;
using Syncfusion.Maui.Core.Hosting;
using Syncfusion.Maui.SmartComponents.Hosting;
using Azure.AI.OpenAI;
using Azure;

namespace YourApp
{
    public class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                });

            // Register Syncfusion core
            builder.ConfigureSyncfusionCore();

            // Azure OpenAI configuration
            string key = "your-azure-api-key";
            Uri azureEndPoint = new Uri("https://your-resource.openai.azure.com/");
            string deploymentName = "your-gpt-deployment-name";

            // Create Azure OpenAI client
            AzureOpenAIClient azureOpenAIClient = new AzureOpenAIClient(
                azureEndPoint, 
                new AzureKeyCredential(key)
            );

            // Get chat client
            IChatClient azureChatClient = azureOpenAIClient
                .GetChatClient(deploymentName)
                .AsIChatClient();

            // Register in dependency injection
            builder.Services.AddChatClient(azureChatClient);

            // Configure Syncfusion AI services
            builder.ConfigureSyncfusionAIServices();

            return builder.Build();
        }
    }
}
```

### Step 3: Secure Credential Management

Never hardcode credentials. Use Secrets Manager:

```csharp
// Using Microsoft.Extensions.Configuration.UserSecrets

public static MauiApp CreateMauiApp()
{
    var builder = MauiApp.CreateBuilder();
    builder.UseMauiApp<App>();

    // Load configuration from user secrets
    var configuration = new ConfigurationBuilder()
        .AddUserSecrets<MauiProgram>()
        .Build();

    builder.ConfigureSyncfusionCore();

    // Get credentials from secure storage
    string key = configuration["AzureOpenAI:Key"];
    string endpoint = configuration["AzureOpenAI:Endpoint"];
    string deployment = configuration["AzureOpenAI:Deployment"];

    AzureOpenAIClient azureOpenAIClient = new AzureOpenAIClient(
        new Uri(endpoint),
        new AzureKeyCredential(key)
    );

    IChatClient azureChatClient = azureOpenAIClient
        .GetChatClient(deployment)
        .AsIChatClient();

    builder.Services.AddChatClient(azureChatClient);
    builder.ConfigureSyncfusionAIServices();

    return builder.Build();
}
```

### Step 4: Runtime Configuration

For environment-specific credentials:

```csharp
public static class AIServiceConfig
{
    public static MauiAppBuilder ConfigureAIService(
        this MauiAppBuilder builder,
        string environment = "Production")
    {
        string key, endpoint, deployment;

        if (environment == "Development")
        {
            key = SecureStorage.Default.GetAsync("dev_ai_key").Result;
            endpoint = "https://dev-resource.openai.azure.com/";
            deployment = "dev-gpt-35";
        }
        else
        {
            key = SecureStorage.Default.GetAsync("prod_ai_key").Result;
            endpoint = "https://prod-resource.openai.azure.com/";
            deployment = "prod-gpt-4";
        }

        AzureOpenAIClient azureOpenAIClient = new AzureOpenAIClient(
            new Uri(endpoint),
            new AzureKeyCredential(key)
        );

        IChatClient azureChatClient = azureOpenAIClient
            .GetChatClient(deployment)
            .AsIChatClient();

        builder.Services.AddChatClient(azureChatClient);
        builder.ConfigureSyncfusionAIServices();

        return builder;
    }
}

// Usage in MauiProgram.cs
public static MauiApp CreateMauiApp()
{
    var builder = MauiApp.CreateBuilder();
    builder.UseMauiApp<App>();
    builder.ConfigureSyncfusionCore();

    #if DEBUG
    builder.ConfigureAIService("Development");
    #else
    builder.ConfigureAIService("Production");
    #endif

    return builder.Build();
}
```
