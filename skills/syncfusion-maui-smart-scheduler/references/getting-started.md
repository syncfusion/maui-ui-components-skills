# Getting Started with .NET MAUI AI-Powered Scheduler

## Table of Contents
- [AI Service Requirements](#ai-service-requirements)
- [Creating a New MAUI Project](#creating-a-new-maui-project)
  - [Visual Studio](#visual-studio)
  - [Visual Studio Code](#visual-studio-code)
  - [JetBrains Rider](#jetbrains-rider)
- [Installing the NuGet Package](#installing-the-nuget-package)
- [Registering the Handler](#registering-the-handler)
- [Configuring AI Services](#configuring-ai-services)
  - [Azure OpenAI](#azure-openai)
  - [OpenAI](#openai)
  - [Ollama (Local)](#ollama-local)
- [Adding SfSmartScheduler](#adding-sfsmartscheduler)
- [First Appointment with Natural Language](#first-appointment-with-natural-language)
- [Troubleshooting](#troubleshooting)

---

## AI Service Requirements

You'll need access to at least one AI service:
- **Azure OpenAI** (recommended for production)
- **OpenAI API** (requires API key)
- **Ollama** (for local/offline scenarios)

---

## Creating a New MAUI Project

### Visual Studio

1. **Launch Visual Studio 2022**
2. **Create New Project:**
   - Go to **File > New > Project**
   - Search for **.NET MAUI App** template
   - Click **Next**

3. **Configure Project:**
   - **Project Name:** Enter your project name (e.g., `SmartSchedulerApp`)
   - **Location:** Choose your project directory
   - Click **Next**

4. **Framework Selection:**
   - Select **.NET 9.0** or later
   - Click **Create**

Visual Studio will create the project and restore NuGet packages automatically.

### Visual Studio Code

1. **Open Command Palette:**
   - Press `Ctrl+Shift+P` (Windows/Linux) or `Cmd+Shift+P` (Mac)
   - Type **.NET: New Project**
   - Press **Enter**

2. **Select Template:**
   - Choose **.NET MAUI App**
   - Select project location
   - Enter project name
   - Press **Enter**

3. **Create Project:**
   - Choose **Create project**
   - Wait for project creation and package restoration

4. **Open Terminal:**
   - Press `Ctrl+`` (backtick) to open integrated terminal
   - Navigate to project directory if needed

---

## Installing the NuGet Package

### Visual Studio

1. **Open Solution Explorer**
2. **Right-click on the project** (not solution)
3. **Select "Manage NuGet Packages"**
4. **Search for:** `Syncfusion.Maui.SmartComponents`
5. **Install:**
   - Select the package from search results
   - Click **Install**
   - Accept the license agreement
   - Wait for installation to complete

### Visual Studio Code

Open the integrated terminal (`Ctrl+``) and run:

```bash
dotnet add package Syncfusion.Maui.SmartComponents
```

Restore packages:
```bash
dotnet restore
```

### Package Manager Console (Visual Studio)

Alternatively, use Package Manager Console:
```powershell
Install-Package Syncfusion.Maui.SmartComponents
```

### Verify Installation

Check your `.csproj` file to confirm the package reference:

```xml
<ItemGroup>
  <PackageReference Include="Syncfusion.Maui.SmartComponents" Version="*" />
</ItemGroup>
```

---

## Registering the Handler

The `Syncfusion.Maui.Core` NuGet package is a required dependency for all Syncfusion MAUI controls. You must register the Syncfusion handler in your `MauiProgram.cs` file.

### Step 1: Open MauiProgram.cs

Navigate to your project root and open `MauiProgram.cs`.

### Step 2: Add Using Statement

Add the following using directive at the top:

```csharp
using Syncfusion.Maui.Core.Hosting;
```

### Step 3: Register Handler

Add `ConfigureSyncfusionCore()` to the builder configuration:

```csharp
using Syncfusion.Maui.Core.Hosting;

namespace SmartSchedulerApp
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            
            builder
                .UseMauiApp<App>()
                .ConfigureSyncfusionCore();  // ← Register Syncfusion handler
            
            builder.ConfigureFonts(fonts =>
            {
                fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                fonts.AddFont("Segoe-mdl2.ttf", "SegoeMDL2");
            });

            return builder.Build();
        }
    }
}
```

**Important:** `ConfigureSyncfusionCore()` must be called before building the app.

---

## Configuring AI Services

The SfSmartScheduler requires an AI service to understand natural language. Configure one of the following services.

### Azure OpenAI

**Recommended for production environments** due to enterprise features and reliability.

#### Step 1: Get Azure Credentials

From your Azure portal:
- **API Key:** Your Azure OpenAI key
- **Endpoint:** Your Azure OpenAI endpoint URL
- **Deployment Name:** Your model deployment name

#### Step 2: Add Using Statements

```csharp
using Syncfusion.Maui.SmartComponents.Hosting;
using Azure.AI.OpenAI;
using Azure;
using Microsoft.Extensions.AI;
```

#### Step 3: Configure Service

Add this configuration in `MauiProgram.cs`:

```csharp
using Syncfusion.Maui.Core.Hosting;
using Syncfusion.Maui.SmartComponents.Hosting;
using Azure.AI.OpenAI;
using Azure;
using Microsoft.Extensions.AI;

namespace SmartSchedulerApp
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            
            builder
                .UseMauiApp<App>()
                .ConfigureSyncfusionCore();
            
            builder.ConfigureFonts(fonts =>
            {
                fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                fonts.AddFont("Segoe-mdl2.ttf", "SegoeMDL2");
            });

            // Azure OpenAI Configuration
            string apiKey = "YOUR-AZURE-API-KEY";
            Uri endpoint = new Uri("https://your-resource.openai.azure.com/");
            string deploymentName = "gpt-4";

            AzureOpenAIClient azureClient = new AzureOpenAIClient(
                endpoint, 
                new AzureKeyCredential(apiKey)
            );
            
            IChatClient chatClient = azureClient
                .GetChatClient(deploymentName)
                .AsIChatClient();

            builder.Services.AddChatClient(chatClient);
            builder.ConfigureSyncfusionAIServices();

            return builder.Build();
        }
    }
}
```

**Security Best Practice:** Store credentials in app settings or environment variables, not hardcoded:

```csharp
string apiKey = Environment.GetEnvironmentVariable("AZURE_OPENAI_KEY");
string endpointUrl = Environment.GetEnvironmentVariable("AZURE_OPENAI_ENDPOINT");
string deployment = Environment.GetEnvironmentVariable("AZURE_OPENAI_DEPLOYMENT");
```

### OpenAI

For using OpenAI's API directly.

#### Step 1: Install OpenAI Package

```bash
dotnet add package Azure.AI.OpenAI
```

#### Step 2: Configure Service

```csharp
using OpenAI;
using Microsoft.Extensions.AI;
using Syncfusion.Maui.SmartComponents.Hosting;

// OpenAI Configuration
string apiKey = "YOUR-OPENAI-API-KEY";

OpenAIClient openAIClient = new OpenAIClient(apiKey);
IChatClient chatClient = openAIClient
    .GetChatClient("gpt-4")
    .AsIChatClient();

builder.Services.AddChatClient(chatClient);
builder.ConfigureSyncfusionAIServices();
```

### Ollama (Local)

For local/offline scenarios without external API dependencies.

#### Step 1: Install Ollama

Download and install [Ollama](https://ollama.ai/) on your development machine.

#### Step 2: Pull a Model

```bash
ollama pull llama3.1
```

#### Step 3: Start Ollama Service

```bash
ollama serve
```

#### Step 4: Install OllamaSharp Package

```bash
dotnet add package OllamaSharp
```

#### Step 5: Configure Service

```csharp
using OllamaSharp;
using Microsoft.Extensions.AI;
using Syncfusion.Maui.SmartComponents.Hosting;

// Ollama Configuration
Uri ollamaEndpoint = new Uri("http://localhost:11434");
string modelName = "llama3.1";

OllamaApiClient ollamaClient = new OllamaApiClient(ollamaEndpoint);
ollamaClient.SelectedModel = modelName;

IChatClient chatClient = ollamaClient.AsIChatClient();

builder.Services.AddChatClient(chatClient);
builder.ConfigureSyncfusionAIServices();
```

---

## Adding SfSmartScheduler

With the handler and AI service configured, you can now add the SfSmartScheduler to your UI.

### XAML Approach

#### Step 1: Add XML Namespace

Open `MainPage.xaml` and add the namespace declaration:

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:smartScheduler="clr-namespace:Syncfusion.Maui.SmartComponents;assembly=Syncfusion.Maui.SmartComponents"
             x:Class="SmartSchedulerApp.MainPage">
    
    <!-- Content goes here -->
    
</ContentPage>
```

#### Step 2: Add SfSmartScheduler Control

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:smartScheduler="clr-namespace:Syncfusion.Maui.SmartComponents;assembly=Syncfusion.Maui.SmartComponents"
             x:Class="SmartSchedulerApp.MainPage">

    <Grid>
        <smartScheduler:SfSmartScheduler x:Name="smartScheduler" />
    </Grid>

</ContentPage>
```

### C# Approach

#### Step 1: Add Using Statement

Open `MainPage.xaml.cs` and add:

```csharp
using Syncfusion.Maui.SmartComponents;
```

#### Step 2: Initialize Control

```csharp
using Syncfusion.Maui.SmartComponents;

namespace SmartSchedulerApp
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            SfSmartScheduler smartScheduler = new SfSmartScheduler();
            this.Content = smartScheduler;
        }
    }
}
```

### Run the Application

**Visual Studio:**
- Select your target platform (Android, iOS, Windows, Mac Catalyst)
- Press **F5** or click **Run**

**Visual Studio Code:**
```bash
dotnet build
dotnet run
```

**JetBrains Rider:**
- Select target platform from dropdown
- Click **Run** button or press **Shift+F10**

---

## First Appointment with Natural Language

Once your app is running, you can create your first appointment using natural language.

### Using the Assist View

1. **Click the Assist Button** (typically in the bottom-right corner)
2. **Type a natural language command** in the input field:
   - "Schedule team meeting tomorrow at 2pm"
   - "Add dentist appointment next Friday at 10am"
   - "Create project review on March 25th at 3pm"
3. **Press Enter** or click the send button
4. **Watch the AI create the appointment** automatically

### Example Natural Language Patterns

**Basic appointment:**
```
"Schedule standup meeting tomorrow at 9am"
```

**With duration:**
```
"Book client call on Friday from 2pm to 3:30pm"
```

**Recurring appointment:**
```
"Create weekly team sync every Monday at 10am"
```

**With location/resource:**
```
"Schedule meeting in conference room A tomorrow at 2pm"
```

### Verify Appointment Creation

After submitting your natural language request:
1. The appointment appears on the scheduler
2. You'll see confirmation in the assist view
3. The appointment respects your current view context (day, week, month)

---

## Troubleshooting

### Issue: "Handler not registered" error

**Symptoms:** Application crashes with handler registration error

**Solution:**
- Ensure `ConfigureSyncfusionCore()` is called in `MauiProgram.cs`
- Verify `using Syncfusion.Maui.Core.Hosting;` is added
- Check that it's called before `builder.Build()`

```csharp
builder
    .UseMauiApp<App>()
    .ConfigureSyncfusionCore();  // Must be here
```

### Issue: AI service not responding

**Symptoms:** Typing in assist view doesn't create appointments

**Solutions:**

**For Azure OpenAI:**
- Verify API key is correct and active
- Check endpoint URL format: `https://your-resource.openai.azure.com/`
- Confirm deployment name matches your Azure configuration
- Test connectivity to Azure endpoint

**For OpenAI:**
- Verify API key is valid
- Check API quota/limits on OpenAI dashboard
- Ensure internet connectivity

**For Ollama:**
- Verify Ollama service is running: `ollama serve`
- Check Ollama endpoint: `http://localhost:11434`
- Confirm model is downloaded: `ollama list`
- Test model: `ollama run llama3.1 "test"`

**General checks:**
- Ensure `ConfigureSyncfusionAIServices()` is called after `AddChatClient()`
- Verify no firewall blocking AI service connections
- Check application logs for AI service errors

### Issue: SfSmartScheduler not visible

**Symptoms:** Application runs but scheduler doesn't appear

**Solutions:**
- Check XML namespace declaration is correct
- Verify assembly name: `assembly=Syncfusion.Maui.SmartComponents`
- Ensure NuGet package is properly installed
- Check parent container has adequate height/width
- Try setting explicit height: `<smartScheduler:SfSmartScheduler HeightRequest="600" />`

### Issue: Package version conflicts

**Symptoms:** Build errors about package versions

**Solutions:**
- Update all Syncfusion packages to same version
- Run `dotnet restore` or `dotnet clean` then rebuild
- Clear NuGet cache: `dotnet nuget locals all --clear`
- Check `.csproj` for conflicting package versions

### Issue: "Could not load assembly" error

**Symptoms:** Runtime error loading Syncfusion assembly

**Solutions:**
- Clean and rebuild solution
- Delete `bin` and `obj` folders manually
- Verify package is installed in the correct project (not solution folder)
- Check target framework matches package compatibility

### Issue: Assist button not appearing

**Symptoms:** Scheduler loads but no assist button visible

**Solutions:**
- Verify `EnableAssistButton` is not set to `false`
- Check if custom `AssistButtonTemplate` has rendering issues
- Ensure adequate screen space for assist button placement
- Try running on different platforms (Android, iOS, Windows)

### Getting Help

If issues persist:
- Check [Syncfusion MAUI Documentation](https://help.syncfusion.com/maui/introduction/overview)
- Visit [Syncfusion Support Forum](https://www.syncfusion.com/forums/maui)
- Review [GitHub Issues](https://github.com/syncfusion/maui-demos)
- Contact Syncfusion Direct-Trac support (with license)

---

## Next Steps

Now that you have the basic setup working:

1. **Explore natural language operations:** Learn advanced CRUD patterns
   - Read [natural-language-operations.md](natural-language-operations.md)

2. **Implement resource-aware features:** Add conflict detection and smart booking
   - Read [resource-aware-features.md](resource-aware-features.md)

3. **Customize the assist view:** Brand the UI to match your app
   - Read [assist-view-customization.md](assist-view-customization.md)

4. **Handle events programmatically:** Add custom validation and logic
   - Read [events-and-methods.md](events-and-methods.md)

5. **Style your scheduler:** Apply colors, fonts, and themes
   - Read [styling.md](styling.md)
