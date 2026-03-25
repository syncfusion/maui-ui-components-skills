---
name: syncfusion-maui-theming
description: Themes and styles Syncfusion .NET MAUI components. Use when working with SyncfusionThemeResourceDictionary, MaterialLight, MaterialDark, CupertinoLight, CupertinoDark, visual themes, or theme switching. This skill covers theme customization, color overrides, theme keys, dark mode, light mode, and branding.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Theming in Syncfusion .NET MAUI

Complete guide for applying, customizing, and creating themes across all Syncfusion .NET MAUI components.

## When to Use This Skill

Use this skill when you need to:
- Apply light or dark themes (MaterialLight, MaterialDark, CupertinoLight, CupertinoDark)
- Configure `SyncfusionThemeResourceDictionary` in applications
- Switch themes dynamically at runtime
- Override default theme colors for specific controls
- Customize control-specific styles and appearances
- Create completely custom themes from scratch
- Apply consistent branding across all components
- Implement system theme synchronization
- Troubleshoot theme-related styling issues
- Find theme keys for specific UI elements

## Component Overview

Syncfusion's theming system provides a unified way to style all MAUI components through the `SyncfusionThemeResourceDictionary`. This centralized approach offers:

- **Automatic Merging**: Control-specific styles automatically included
- **Consistent Appearance**: Uniform look across all components
- **Easy Theme Switching**: Change themes at runtime
- **Flexible Customization**: Override specific keys or create custom themes
- **Platform Themes**: Material and Cupertino design support

**Available Themes:**
- `MaterialLight` - Material Design light theme
- `MaterialDark` - Material Design dark theme
- `CupertinoLight` - Apple iOS light theme
- `CupertinoDark` - Apple iOS dark theme

## Documentation and Navigation Guide

### Basic Theme Setup
📄 **Read:** [references/applying-themes.md](references/applying-themes.md)
- Installing and configuring SyncfusionThemeResourceDictionary
- Available theme options (Material, Cupertino)
- Setting up themes in App.xaml
- Automatic merging of control styles
- Platform-specific theme application
- VisualTheme property usage

### Runtime Theme Switching
📄 **Read:** [references/theme-switching.md](references/theme-switching.md)
- Dynamically changing themes at runtime
- ViewModel pattern for theme management
- Removing and adding theme dictionaries
- User preference persistence
- Syncing with system/OS theme
- Edge cases and troubleshooting

### Customizing Default Themes
📄 **Read:** [references/overriding-themes.md](references/overriding-themes.md)
- Overriding specific theme color keys
- Control-specific key customization
- Primary vs control-specific keys
- Merged dictionaries layer order
- Selective customization strategies
- Common branding scenarios

### Creating Custom Themes
📄 **Read:** [references/creating-custom-themes.md](references/creating-custom-themes.md)
- Building themes from scratch
- Theme key registration pattern
- Custom color scheme definition
- Complete theme implementation
- Corporate branding themes
- Accessibility themes (high contrast)

### Theme Keys Reference
📄 **Read:** [references/theme-keys-reference.md](references/theme-keys-reference.md)
- Key naming conventions
- Structure: Sf{ControlName}{Element}{Property}
- Common control keys by category
- Finding keys for any control
- Official documentation references

