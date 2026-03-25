---
name: syncfusion-maui-smart-text-editor
description: Implements and configure the Syncfusion .NET MAUI AI-Powered Smart Text Editor (SfSmartTextEditor). Use when working with SfSmartTextEditor, AI-powered text input, predictive text suggestions, inline or popup suggestions, or UserRole/UserPhrases configurations. Covers suggestion display modes, custom AI services (Claude, DeepSeek, Gemini, Groq), Azure OpenAI integration, and customization options.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion .NET MAUI Smart Text Editor

`SfSmartTextEditor` is a multiline text input control that accelerates typing with AI-powered predictive suggestions. It integrates with Azure OpenAI, OpenAI, Ollama, or any custom AI service via `IChatInferenceService`, and falls back to your custom `UserPhrases` list when AI is unavailable. Suggestions appear inline or as a popup near the caret and can be accepted with Tab or Right Arrow.

## When to Use This Skill

- **Add a multiline text input** with AI-powered autocomplete to a .NET MAUI app
- **Configure suggestion display** as inline (desktop) or popup (touch devices)
- **Integrate Azure OpenAI**, OpenAI, Ollama, Claude, DeepSeek, Gemini, or Groq
- **Implement a custom AI service** via `IChatInferenceService`
- **Customize text style**, placeholder, suggestion colors, popup background, or character limits
- **Respond to text change events** or MVVM commands
- **Apply the liquid glass visual effect** to the editor

## Component Overview

The **SfSmartTextEditor** control provides:
- **AI-Powered Predictions**: Context-aware completions via Azure OpenAI, OpenAI, Ollama, or custom IChatInferenceService integration
- **Flexible Suggestion Display**: Inline mode (desktop-first seamless flow) or Popup mode (touch-first overlay) with platform-specific defaults
- **Offline Fallback**: Custom UserPhrases library for AI-free suggestions when network or API is unavailable
- **Smart Context Engine**: UserRole property shapes suggestion tone and relevance based on typing scenario
- **Multi-Input Acceptance**: Tab/Right Arrow keyboard shortcuts (desktop) and tap/click gestures (all platforms)
- **Character Limit Validation**: MaxLength enforcement to prevent input beyond specified constraints
- **Rich Customization**: TextStyle, placeholder colors, suggestion preview colors, and popup background styling
- **MVVM Support**: Two-way Text binding, TextChanged event, and TextChangedCommand for reactive data flow
- **Modern Effects**: Liquid Glass Effect support for translucent designs (iOS/macOS 26+, .NET 10)

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- NuGet package installation (`Syncfusion.Maui.SmartComponents`)
- Handler registration with `ConfigureSyncfusionCore()` in `MauiProgram.cs`
- AI service registration with `ConfigureSyncfusionAIServices()`
- Adding `SfSmartTextEditor` in XAML and C#
- Configuring `UserRole` and `UserPhrases` for context-aware suggestions
- Offline fallback behavior when AI is unavailable

### Suggestion Display Modes
📄 **Read:** [references/suggestion-display-modes.md](references/suggestion-display-modes.md)
- Inline mode — suggestion rendered in-place after the caret (desktop-first)
- Popup mode — compact overlay near caret (touch-first, Android/iOS)
- Platform defaults and when to override them
- Accepting suggestions with Tab or Right Arrow keys
- Platform limitations (Tab key not supported on Android/iOS)

### Customization
📄 **Read:** [references/customization.md](references/customization.md)
- Setting and data-binding editor text via `Text` property
- Font and color via `TextStyle` (`SmartTextEditorStyle`)
- Placeholder text and `PlaceholderColor`
- `SuggestionTextColor` — style the suggestion preview text
- `SuggestionPopupBackground` — background color of popup suggestions
- `MaxLength` — enforce a character limit

### Events
📄 **Read:** [references/events.md](references/events.md)
- `TextChanged` event with `OldTextValue` and `NewTextValue`
- XAML event subscription and C# handler pattern
- `TextChangedCommand` for MVVM / data-binding scenarios
- ViewModel command setup example

### AI Service Configuration (Azure OpenAI / OpenAI / Ollama)
📄 **Read:** [references/ai-service-configuration.md](references/ai-service-configuration.md)
- Azure OpenAI — required NuGet packages, endpoint/key/model configuration
- OpenAI — API key and model setup
- Ollama — self-hosted local model configuration
- Registering the chat client and `ConfigureSyncfusionAIServices()` in `MauiProgram.cs`
- Choosing the right provider for your scenario

### Custom AI Services (Claude / DeepSeek / Gemini / Groq)
📄 **Read:** [references/custom-ai-services.md](references/custom-ai-services.md)
- `IChatInferenceService` interface and when to use it
- Claude AI — request/response models, service class, registration
- DeepSeek — chat completions integration and registration
- Gemini — Google AI Studio setup, safety settings, registration
- Groq — low-latency OpenAI-compatible endpoint, registration
- No need to call `ConfigureSyncfusionAIServices()` when using custom services
- Troubleshooting when no suggestions appear

### Liquid Glass Effect
📄 **Read:** [references/liquid-glass-effect.md](references/liquid-glass-effect.md)
- Prerequisites: .NET 10, macOS 26+ or iOS 26+
- Wrapping `SfSmartTextEditor` in `SfGlassEffectView`
- Enabling via `EnableLiquidGlassEffect="True"`
- Setting `Background="Transparent"` for correct glass tinting
- Full XAML and C# examples

