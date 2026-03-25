# Getting Started with SfSmartTextEditor

This guide walks through adding the Syncfusion .NET MAUI Smart Text Editor to your project — from NuGet installation through to a running editor with AI suggestions.

## Step 1: Install the NuGet Package

The Smart Text Editor is part of the `Syncfusion.Maui.SmartComponents` package.

**Package Manager Console:**
```bash
Install-Package Syncfusion.Maui.SmartComponents
```

**CLI:**
```bash
dotnet add package Syncfusion.Maui.SmartComponents
```

Or search for `Syncfusion.Maui.SmartComponents` in the NuGet Package Manager UI and install the latest version.

> The `Syncfusion.Maui.Core` package is a required dependency and is installed automatically.

---

## Step 2: Register the Syncfusion Handler

In `MauiProgram.cs`, call `ConfigureSyncfusionCore()` to register all Syncfusion control handlers:

```csharp
using Syncfusion.Maui.Core.Hosting;

public static class MauiProgram
{
    public static MauiApp CreateMauiApp()
    {
        var builder = MauiApp.CreateBuilder();
        builder
            .UseMauiApp<App>()
            .ConfigureSyncfusionCore()
            .ConfigureFonts(fonts =>
            {
                fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
            });

        return builder.Build();
    }
}
```

---

## Step 3: Register the AI Service

To enable AI-powered suggestions, register a chat client and call `ConfigureSyncfusionAIServices()`:

```csharp
using Azure.AI.OpenAI;
using Microsoft.Extensions.AI;
using System.ClientModel;
using Syncfusion.Maui.Core.Hosting;
using Syncfusion.Maui.SmartComponents.Hosting;

public static class MauiProgram
{
    public static MauiApp CreateMauiApp()
    {
        var builder = MauiApp.CreateBuilder();
        builder.UseMauiApp<App>().ConfigureSyncfusionCore();

        // Configure Azure OpenAI
        AzureOpenAIClient azureClient = new AzureOpenAIClient(
            new Uri("<AZURE_ENDPOINT>"),
            new AzureKeyCredential("<AZURE_KEY>"));
        IChatClient chatClient = azureClient.GetChatClient("<DEPLOYMENT_NAME>").AsIChatClient();

        builder.Services.AddChatClient(chatClient);
        builder.ConfigureSyncfusionAIServices();

        return builder.Build();
    }
}
```

> For other providers (OpenAI, Ollama) see `ai-service-configuration.md`.
> For custom services (Claude, DeepSeek, Gemini, Groq) see `custom-ai-services.md` — these do **not** require `ConfigureSyncfusionAIServices()`.

---

## Step 4: Add SfSmartTextEditor to a Page

**XAML:**
```xml
<ContentPage
    xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:smarttexteditor="clr-namespace:Syncfusion.Maui.SmartComponents;assembly=Syncfusion.Maui.SmartComponents">

    <smarttexteditor:SfSmartTextEditor />
</ContentPage>
```

**C#:**
```csharp
using Syncfusion.Maui.SmartComponents;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        var editor = new SfSmartTextEditor();
        this.Content = editor;
    }
}
```

---

## Step 5: Configure UserRole and UserPhrases

`UserRole` tells the AI who is typing and what tone to use — this is the most important property for getting relevant suggestions. `UserPhrases` provides a list of reusable expressions for offline fallback.

```xml
<smarttexteditor:SfSmartTextEditor
    Placeholder="Type your reply..."
    UserRole="Support engineer responding to customer tickets">
    <smarttexteditor:SfSmartTextEditor.UserPhrases>
        <x:String>Thanks for reaching out.</x:String>
        <x:String>Please share a minimal reproducible sample.</x:String>
        <x:String>We'll update you as soon as we have more details.</x:String>
    </smarttexteditor:SfSmartTextEditor.UserPhrases>
</smarttexteditor:SfSmartTextEditor>
```

**C# equivalent:**
```csharp
var editor = new SfSmartTextEditor
{
    Placeholder = "Type your reply...",
    UserRole = "Support engineer responding to customer tickets",
    UserPhrases =
    {
        "Thanks for reaching out.",
        "Please share a minimal reproducible sample.",
        "We'll update you as soon as we have more details."
    }
};
```

---

## Offline Suggestions (No AI Service)

If no AI inference service is configured, the editor uses `UserPhrases` to generate suggestions locally. This is useful for:
- Environments with no network access
- Apps that don't require AI but still want phrase completion
- Testing without an API key

> Simply omit the `AddChatClient` / `ConfigureSyncfusionAIServices()` calls and populate `UserPhrases` with your phrases.

---

## Accepting Suggestions

Once the editor displays a suggestion (inline or popup), it can be accepted with:
- **Tab key** or **Right Arrow key** — on Windows and Mac
- **Tap/click the suggestion** — in Popup mode on all platforms

> Tab key acceptance is **not supported** on Android and iOS.
