---
name: syncfusion-maui-smart-datagrid
description: Implements Syncfusion .NET MAUI Smart DataGrid (SfSmartDataGrid) with AI-powered features. Use when working with SfSmartDataGrid, AI-powered grid operations, natural language data commands, AI sorting, AI filtering, or conversational grid interaction. Covers Azure OpenAI grid integration and natural language commands for grids.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion MAUI Smart DataGrid

A comprehensive skill for implementing and customizing the Syncfusion .NET MAUI Smart DataGrid (SfSmartDataGrid) control. The Smart DataGrid combines traditional tabular data display with AI-powered natural language capabilities, enabling users to perform complex operations like sorting, filtering, grouping, and highlighting using simple text commands. It displays and manipulates data in a tabular format with advanced support for intelligent data operations, AI-assisted features, customizable assistant views, and event-driven architecture.

## When to Use This Skill

Use this skill when you need to:

- **Add a Smart DataGrid to .NET MAUI applications** that requires AI-assisted data operations
- **Enable natural language commands** for users to sort, filter, group, and highlight data without coding
- **Configure Azure OpenAI or other AI providers** to power intelligent grid operations
- **Customize the AssistView UI** with templates and styling for appearance customization
- **Implement AI-powered features** like intelligent sorting with multi-column support and advanced filtering
- **Manage grid operations programmatically** using methods like `GetResponseAsync()` and events
- **Style toolbar, buttons, and assistant views** to match application design requirements

## Component Overview

The `SfSmartDataGrid` (Syncfusion .NET MAUI Smart DataGrid) is an advanced data grid control that combines traditional grid functionality with AI-powered natural language processing. Users can interact with data using simple text commands instead of manual UI navigation.

**Key Capabilities:**
- **AI-Assisted Operations:** Sort, filter, group, and highlight data using natural language prompts
- **Multi-Column Operations:** Apply complex operations in a single command (e.g., sort by multiple columns)
- **Customizable AssistView:** Configure suggestions, prompts, and initial commands
- **Flexible Styling:** Customize toolbar, buttons, and assistant view appearance with templates
- **Event-Driven Architecture:** Handle requests, opening, and closing events for AI operations
- **AI Service Integration:** Configure Azure OpenAI, Gemini, DeepSeek, or Groq services

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Install Syncfusion.Maui.SmartComponents NuGet package
- Register handlers in MauiProgram.cs
- Configure Azure AI service with credentials
- Create basic Smart DataGrid with data binding
- Add OrderInfo data model and repository
- Enable AI-assisted operations
- Running and testing the application

### AI-Powered Features
📄 **Read:** [references/ai-powered-features.md](references/ai-powered-features.md)
- Natural language sorting (single and multi-column)
- Intelligent grouping with hierarchical support
- Advanced filtering with conditions (AND/OR logic)
- Highlighting rows and cells with custom colors
- Clearing operations (sort, filter, group, highlight)
- Example prompts and practical use cases

### Customization and Configuration
📄 **Read:** [references/customization.md](references/customization.md)
- Set SuggestedPrompts for predefined suggestions
- Configure Prompt for auto-execution on startup
- Enable/disable smart actions with EnableSmartActions
- Show AssistView programmatically with ShowAssistView()
- Apply AI operations dynamically using GetResponseAsync()
- Handle AssistViewRequest events
- Manage AssistViewOpening and AssistViewClosing events

### Appearance and Styling
📄 **Read:** [references/appearance-styling.md](references/appearance-styling.md)
- Style toolbar with background, stroke, and thickness
- Create custom toolbar layout with ToolbarTemplate
- Configure AssistButton appearance and icon color
- Define custom AssistButton and icon templates
- Customize AssistView popup, header, and colors
- Create AssistView header, banner, and editor templates
- Styling SmartAssistStyle properties

### AI Service Configuration
📄 **Read:** [references/ai-service-configuration.md](references/ai-service-configuration.md)
- Configure Azure OpenAI service setup
- Register ConfigureSyncfusionAIServices()
- Set API key, endpoint, and deployment name
- AI service requirements and prerequisites
- Supporting alternative AI providers
- Troubleshooting AI service connectivity

### Configure Chat Client
📄 **Read:** [references/configure-chat-client.md](references/configure-chat-client.md)
- Configure Microsoft.Extensions.AI-compatible chat clients
- Azure OpenAI setup and configuration
- OpenAI API integration with model selection
- Self-hosted Ollama configuration and benefits
- Environment-specific configuration with appsettings.json
- Using environment variables for credentials
- Troubleshooting authentication and connection errors

### Custom AI Service Integration
📄 **Read:** [references/custom-ai-service.md](references/custom-ai-service.md)
- Implement custom AI services using IChatInferenceService interface
- Create AI service class and inference service wrapper
- Register custom services in MauiProgram.cs
- Complete custom implementation example
- Testing custom integration
- Best practices for error handling and logging
- Troubleshooting response generation and performance

### Claude AI Integration
📄 **Read:** [references/claude-service.md](references/claude-service.md)
- Create Anthropic Claude account and get API key
- Implement ClaudeAIService and ClaudeInferenceService
- Register Claude service in MauiProgram.cs
- Available Claude models and capabilities

### Google Gemini Integration
📄 **Read:** [references/gemini-service.md](references/gemini-service.md)
- Get Google AI Studio API key for Gemini
- Implement GeminiService and GeminiInferenceService
- Configure Gemini models and generation settings
- Register Gemini service in MauiProgram.cs

### Groq AI Integration
📄 **Read:** [references/groq-service.md](references/groq-service.md)
- Set up Groq console and API key
- Implement GroqService and GroqInferenceService
- Configure Groq models with OpenAI-compatible endpoint
- Register Groq service in MauiProgram.cs

### DeepSeek AI Integration
📄 **Read:** [references/deepseek-service.md](references/deepseek-service.md)
- Set up DeepSeek platform and API key
- Implement DeepSeekAIService and DeepSeekInferenceService
- Configure DeepSeek models and temperature settings
- Register DeepSeek service in MauiProgram.cs

