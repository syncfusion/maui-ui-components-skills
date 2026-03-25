# Applying Themes

Basic setup and configuration of themes in Syncfusion .NET MAUI applications.

## Overview

Syncfusion provides built-in theme support through `SyncfusionThemeResourceDictionary`, which automatically merges all control-specific style dictionaries. This eliminates the need to manually import style resources for each control.

## Available Themes

Syncfusion .NET MAUI supports four built-in themes:

### Material Design Themes
- **MaterialLight** - Material Design light theme (default)
- **MaterialDark** - Material Design dark theme

### Cupertino Themes
- **CupertinoLight** - Apple iOS light theme
- **CupertinoDark** - Apple iOS dark theme

**When to use Material vs Cupertino:**
- **Material**: Apps, cross-platform with Material design language
- **Cupertino**: iOS/macOS-focused apps, native Apple look and feel

## Basic Setup

### Step 1: Add Namespace

Add the Syncfusion themes namespace to your App.xaml:

```xml
<Application xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncTheme="clr-namespace:Syncfusion.Maui.Themes;assembly=Syncfusion.Maui.Core"
             x:Class="YourApp.App">
    <!-- Content here -->
</Application>
```

### Step 2: Add Theme Resource Dictionary

In `Application.Resources`, add the theme dictionary:

```xml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <syncTheme:SyncfusionThemeResourceDictionary VisualTheme="MaterialLight"/>
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

### Step 3: That's It!

All Syncfusion controls in your application now automatically use the theme. No per-control configuration needed.

## Theme Examples

### Material Dark Theme

```xml
<Application xmlns:syncTheme="clr-namespace:Syncfusion.Maui.Themes;assembly=Syncfusion.Maui.Core"
             x:Class="YourApp.App">
    <Application.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <syncTheme:SyncfusionThemeResourceDictionary VisualTheme="MaterialDark"/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

### Cupertino Light Theme

```xml
<Application xmlns:syncTheme="clr-namespace:Syncfusion.Maui.Themes;assembly=Syncfusion.Maui.Core"
             x:Class="YourApp.App">
    <Application.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <syncTheme:SyncfusionThemeResourceDictionary VisualTheme="CupertinoLight"/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

## Platform-Specific Themes

Apply different themes based on platform:

```xml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <OnPlatform x:TypeArguments="ResourceDictionary">
                <!-- iOS/macOS: Cupertino theme -->
                <On Platform="iOS, MacCatalyst">
                    <syncTheme:SyncfusionThemeResourceDictionary VisualTheme="CupertinoLight"/>
                </On>
                
                <!-- Android: Material theme -->
                <On Platform="Android">
                    <syncTheme:SyncfusionThemeResourceDictionary VisualTheme="MaterialLight"/>
                </On>
                
                <!-- Windows: Material theme -->
                <On Platform="WinUI">
                    <syncTheme:SyncfusionThemeResourceDictionary VisualTheme="MaterialLight"/>
                </On>
            </OnPlatform>
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

## Theme Resource Dictionary Structure

The `SyncfusionThemeResourceDictionary` contains:

1. **Primary Theme Colors**: Base colors for all controls
2. **Control-Specific Keys**: Colors for specific UI elements
3. **Component Styles**: Complete style definitions for each control

### What Gets Automatically Merged

When you add `SyncfusionThemeResourceDictionary`, you get:
- All control theme dictionaries (Charts, DataGrid, Calendar, etc.)
- Consistent color scheme across components
- State-specific colors (normal, hover, pressed, disabled)
- Typography settings

### Without Automatic Merging (Old Approach)

Before `SyncfusionThemeResourceDictionary`, you had to manually add:

```xml
<!-- DON'T DO THIS - Use SyncfusionThemeResourceDictionary instead -->
<ResourceDictionary Source="Syncfusion.Maui.Charts.Themes.xaml"/>
<ResourceDictionary Source="Syncfusion.Maui.DataGrid.Themes.xaml"/>
<ResourceDictionary Source="Syncfusion.Maui.Calendar.Themes.xaml"/>
<!-- ...and many more -->
```

## VisualTheme Property

The `VisualTheme` property accepts these values:

```csharp
public enum SfVisuals
{
    MaterialLight,  // Default
    MaterialDark,
    CupertinoLight,
    CupertinoDark
}
```

**Usage in XAML:**
```xml
<syncTheme:SyncfusionThemeResourceDictionary VisualTheme="MaterialDark"/>
```

**Usage in Code:**
```csharp
var themeDict = new SyncfusionThemeResourceDictionary
{
    VisualTheme = SfVisuals.MaterialDark
};
Application.Current.Resources.MergedDictionaries.Add(themeDict);
```

## Verifying Theme Application

To verify the theme is applied:

1. **Visual Check**: Controls should show theme colors
2. **Resource Check**: Access theme keys in code:

```csharp
var buttonBgColor = Application.Current.Resources["SfButtonNormalBackground"];
Console.WriteLine($"Button background: {buttonBgColor}");
```

3. **Control Inspection**: Use Live Visual Tree (Windows) or UI Inspector to verify styles

## Common Setup Mistakes

### Missing Namespace
❌ **Incorrect:**
```xml
<Application xmlns="http://schemas.microsoft.com/dotnet/2021/maui">
    <!-- syncTheme namespace not declared -->
    <Application.Resources>
        <syncTheme:SyncfusionThemeResourceDictionary VisualTheme="MaterialDark"/>
    </Application.Resources>
</Application>
```

✅ **Correct:**
```xml
<Application xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:syncTheme="clr-namespace:Syncfusion.Maui.Themes;assembly=Syncfusion.Maui.Core">
    <Application.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <syncTheme:SyncfusionThemeResourceDictionary VisualTheme="MaterialDark"/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

### Missing ResourceDictionary Wrappers
❌ **Incorrect:**
```xml
<Application.Resources>
    <syncTheme:SyncfusionThemeResourceDictionary VisualTheme="MaterialDark"/>
</Application.Resources>
```

✅ **Correct:**
```xml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <syncTheme:SyncfusionThemeResourceDictionary VisualTheme="MaterialDark"/>
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

### Invalid Theme Name
❌ **Incorrect:**
```xml
<syncTheme:SyncfusionThemeResourceDictionary VisualTheme="Dark"/>
```

✅ **Correct:**
```xml
<syncTheme:SyncfusionThemeResourceDictionary VisualTheme="MaterialDark"/>
```

## Prerequisites

**Required NuGet Package:**
```
Syncfusion.Maui.Core
```

This package is automatically included when you install any Syncfusion MAUI control package.

## Best Practices

1. **Set Once**: Apply theme in App.xaml, not in individual pages
2. **Choose Appropriate Theme**: Material or Cupertino themes
3. **Test Both Light and Dark**: Verify appearance in both modes
4. **Platform Consistency**: Use OnPlatform for platform-specific themes
5. **Clean Rebuild**: After adding theme, clean and rebuild solution

## Next Steps

- **Runtime Switching**: See [theme-switching.md](theme-switching.md)
- **Customization**: See [overriding-themes.md](overriding-themes.md)
- **Custom Themes**: See [creating-custom-themes.md](creating-custom-themes.md)
- **Theme Keys**: See [theme-keys-reference.md](theme-keys-reference.md)