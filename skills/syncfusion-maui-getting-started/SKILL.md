---
name: syncfusion-maui-getting-started
description: Sets up and configures Syncfusion .NET MAUI components. Use when setting up, configuring, installing, or licensing Syncfusion .NET MAUI applications. Covers installation (NuGet, installers), licensing (generation, registration, errors), themes (Material, Cupertino, customization), AI service integration (Azure OpenAI, Claude, Gemini, DeepSeek, Groq), and package upgrades.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Getting Started with Syncfusion .NET MAUI

A comprehensive guide for installing, configuring, and setting up your development environment for Syncfusion .NET MAUI components

## When to Use This Skill

**ALWAYS use this skill immediately** when the user needs to:

- **Install or Setup:** NuGet packages, installers (web, offline, Mac), installation troubleshooting
- **License:** Generate license keys, register keys in applications, resolve licensing errors, trial to paid upgrades
- **Theme:** Apply Material/Cupertino themes, light/dark modes, customize theme colors, override theme keys
- **AI Integration:** Configure Azure OpenAI, Claude, Gemini, DeepSeek, Groq, or custom AI services for Smart Components
- **Tooling:** Use Visual Studio or VS Code extensions, project templates, code snippets, Essential UI Kit
- **Advanced:** Implement Liquid Glass UI, upgrade NuGet packages, handle breaking changes
- **Troubleshoot:** Installation errors, licensing issues, theme problems, AI service configuration

This skill contains cross-cutting concerns that apply to all Syncfusion .NET MAUI components and applications.

---

## Overview of Syncfusion .NET MAUI

Syncfusion .NET MAUI (Multi-platform App UI) is a comprehensive collection of high-performance, feature-rich UI components for building native cross-platform applications. Built on Microsoft's .NET MAUI framework, it enables developers to create modern, beautiful applications with a single codebase targeting:

- **Windows** (desktop applications)
- **macOS** (desktop applications)
- **iOS** (mobile applications)
- **Android** (mobile applications)

### Key Capabilities

- **70+ Components:** Grids, charts, gauges, editors, calendars, schedulers, and more
- **Native Performance:** Platform-specific optimizations for smooth UX
- **Consistent API:** Single codebase works across all platforms
- **Theme Support:** Material Design and Cupertino (iOS-style) themes with light/dark modes
- **AI-Powered:** Smart Components with AI service integration
- **Accessibility:** WCAG-compliant with screen reader support
- **Localization:** RTL and multi-language support

### Component Categories

Syncfusion .NET MAUI includes components across multiple categories:
- Data Visualization (Charts, Gauges, TreeMap)
- Grids & Data (DataGrid, ListView, TreeView)
- Editors & Inputs (TextBox, ComboBox, DatePicker, Autocomplete)
- Calendars & Scheduling (Calendar, Scheduler, DateRangePicker)
- Navigation (TabView, TreeView, Accordion, Carousel)
- Layout (Cards, Chips, Avatar, Backdrop)
- Buttons & Indicators (Button, Checkbox, RadioButton, Badge, Busy Indicator)
- File Formats (PDF, Excel, Word, PowerPoint)

---

## Platform Support & Requirements

### Supported Platforms

| Platform | Minimum Version | Recommended |
|----------|----------------|-------------|
| **Windows** | Windows 10 version 1809+ | Windows 11 |
| **macOS** | macOS 10.15+ | macOS 12+ |
| **iOS** | iOS 11+ | iOS 15+ |
| **Android** | Android 5.0 (API 21)+ | Android 12+ (API 31+) |

### Framework Compatibility

Syncfusion .NET MAUI supports:
- **.NET 6** (initial MAUI support)
- **.NET 7** (enhanced features)
- **.NET 8** (LTS with full feature set)
- **.NET 9** (latest features)
**.NET 10** (latest features)

### Development Environment

**Windows Development:**
- Visual Studio 2022 (v17.3+) for .NET 9, or Visual Studio 2026 for .NET 10
- .NET MAUI workload installed
- Android SDK (for Android development)

**macOS Development:**
- Visual Studio Code with .NET MAUI extension
- Xcode 13+ (for iOS/macOS development)
- .NET MAUI workload installed

**Cross-Platform:**
- Visual Studio Code with .NET MAUI extension
- .NET SDK 9.0+ (or 10.0+ for .NET 10 features)

### System Requirements

📄 **For complete system requirements, read:** [references/introduction-overview.md](references/introduction-overview.md)

---

## Getting Started Guide

### Quick Start (3 Steps)

**Step 1: Install NuGet Package**

```bash
dotnet add package Syncfusion.Maui.Core
# Or install specific component packages as needed
```

**Step 2: Register License Key** (Required for v20.2.0.x+)

In `MauiProgram.cs`, add before `CreateMauiApp()`:

```csharp
public static class MauiProgram
{
    public static MauiApp CreateMauiApp()
    {
        // Register Syncfusion license
        string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
        if (!string.IsNullOrEmpty(licenseKey))
        {   
            Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense(licenseKey);
        }
        
        var builder = MauiApp.CreateBuilder();
        // ... rest of configuration
    }
}
```

**Step 3: Apply Theme** (Optional but recommended)

In `App.xaml`:

```xml
<Application xmlns:syncTheme="clr-namespace:Syncfusion.Maui.Themes;assembly=Syncfusion.Maui.Core">
    <Application.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <syncTheme:SyncfusionThemeResourceDictionary VisualTheme="MaterialLight"/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

**📄 For detailed installation instructions, read:** [references/installation-nuget.md](references/installation-nuget.md)

---

## Documentation and Navigation Guide

### 1. Getting Started & Installation

**When to read these files:** User needs to install Syncfusion, troubleshoot installation, or understand platform requirements.

📄 **[references/introduction-overview.md](references/introduction-overview.md)**
- What is Syncfusion .NET MAUI
- Cross-platform capabilities and architecture
- Complete component ecosystem overview
- Supported platforms (Windows, macOS, iOS, Android)
- Detailed system requirements
- Framework compatibility (.NET 6, 7, 8, 9, 10)
- Development environment setup

📄 **[references/installation-nuget.md](references/installation-nuget.md)**
- Installing NuGet packages via Package Manager UI
- Installing via .NET CLI (`dotnet add package`)
- Package Manager Console commands
- Managing package dependencies
- Version management and updates
- Verifying package installation

📄 **[references/installation-installers.md](references/installation-installers.md)**
- Web installer: download and installation process
- Offline installer: download and installation process
- Mac installer: download and installation process
- Installer comparison (when to use each)
- Installer vs NuGet packages
- Post-installation verification

📄 **[references/installation-errors.md](references/installation-errors.md)**
- Common installation errors and solutions
- Package restore failures
- Version conflicts
- Platform-specific installation issues
- Troubleshooting steps

### 2. Tooling & IDE Integration

**When to read these files:** User needs Visual Studio or VS Code extensions, project templates, code snippets, or tooling features.

📄 **[references/visual-studio-integration.md](references/visual-studio-integration.md)**
- Syncfusion .NET MAUI Visual Studio extension overview
- Extension download and installation
- Project templates and Template Studio
- Toolbox control for drag-and-drop
- Essential UI Kit integration
- Code snippet library
- Syncfusion notifications and updates
- Using project creation wizards

📄 **[references/vscode-integration.md](references/vscode-integration.md)**
- Syncfusion .NET MAUI VS Code extension overview
- Extension download and installation from marketplace
- Creating projects with VS Code extension
- Code snippets for MAUI components
- Essential UI Kit for VS Code
- Command palette integration
- Debugging MAUI apps in VS Code

📄 **[references/icons-reference.md](references/icons-reference.md)**
- Syncfusion icon library for MAUI
- Using built-in icons in applications
- Material icons support
- Icon font resources
- Icon sizing and styling
- Custom icon integration

### 3. Advanced Features

**When to read these files:** User needs Liquid Glass UI effects or package upgrade guidance.

📄 **[references/liquid-glass-ui.md](references/liquid-glass-ui.md)**
- Liquid Glass UI overview and design philosophy
- Getting started with Liquid Glass UI
- Available glassy controls (Button, Card, Entry, etc.)
- Styling and blur effects
- Transparency and frosted glass effects
- Implementation examples and patterns
- Customization options

📄 **[references/upgrade-packages.md](references/upgrade-packages.md)**
- How to upgrade Syncfusion MAUI NuGet packages
- Upgrading via Package Manager UI
- Upgrading via .NET CLI
- Version compatibility considerations
- Handling breaking changes between versions
- Migration guides for major updates
- Testing after upgrades
- Rollback strategies

---