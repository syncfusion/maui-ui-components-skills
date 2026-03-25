# Advanced Features in .NET MAUI Kanban Board

## Table of Contents
- [Overview](#overview)
- [Flow Direction (RTL Support)](#flow-direction-rtl-support)
- [Localization](#localization)
- [Resource Management](#resource-management)
- [Combining RTL with Localization](#combining-rtl-with-localization)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)
- [Next Steps](#next-steps)

## Overview

This reference covers advanced features for the SfKanban control:

- **Flow Direction** - Right-to-left (RTL) support for international applications
- **Localization** - Multi-language support
- **Resource Management** - Translating UI elements

## Flow Direction (RTL Support)

The SfKanban control supports right-to-left (RTL) layout for languages like Arabic, Hebrew, Urdu, and Persian.

### Setting Flow Direction

**XAML:**
```xml
<kanban:SfKanban FlowDirection="RightToLeft"
                 ItemsSource="{Binding Cards}">
    <kanban:SfKanban.Columns>
        <kanban:KanbanColumn Title="للقيام به" Categories="Open" />
        <kanban:KanbanColumn Title="قيد التقدم" Categories="In Progress" />
        <kanban:KanbanColumn Title="منتهي" Categories="Done" />
    </kanban:SfKanban.Columns>
</kanban:SfKanban>
```

**C#:**
```csharp
kanban.FlowDirection = FlowDirection.RightToLeft;
```

### FlowDirection Values

| Value | Description | Use For |
|-------|-------------|---------|
| `LeftToRight` | Default left-to-right layout | English, French, German, Spanish, etc. |
| `RightToLeft` | Right-to-left layout | Arabic, Hebrew, Urdu, Persian, etc. |

### Visual Result

**LeftToRight:**
```
┌────────────┬────────────┬────────────┐
│ To Do      │ In Progress│ Done       │
└────────────┴────────────┴────────────┘
```

**RightToLeft:**
```
┌────────────┬────────────┬────────────┐
│ Done       │ In Progress│ To Do      │
└────────────┴────────────┴────────────┘
```

### When to Use RTL

- Application targets RTL language users
- Multi-language application with RTL support
- Cultural appropriateness required
- Accessibility compliance

### Example: Dynamic Flow Direction

```csharp
public void SetFlowDirection(string cultureName)
{
    var culture = new CultureInfo(cultureName);
    
    kanban.FlowDirection = culture.TextInfo.IsRightToLeft
        ? FlowDirection.RightToLeft
        : FlowDirection.LeftToRight;
}

// Usage
SetFlowDirection("ar-SA");  // Arabic - RTL
SetFlowDirection("en-US");  // English - LTR
```

## Localization

Localization allows translating the kanban control's UI elements to different languages.

### Setup Steps

**Step 1: Create Resource Files**

Add resource files to your project:

```
Resources/
├── SfKanban.resx              (Default English)
├── SfKanban.fr-FR.resx        (French)
├── SfKanban.de-DE.resx        (German)
├── SfKanban.ar-SA.resx        (Arabic)
└── SfKanban.zh-CN.resx        (Chinese)
```

**Step 2: Set Build Action**

- Right-click each .resx file
- Properties → Build Action: **EmbeddedResource**

**Step 3: Configure in App.xaml.cs**

```csharp
using Syncfusion.Maui.Kanban;
using System.Globalization;
using System.Resources;

public partial class App : Application
{
    public App()
    {
        InitializeComponent();
        
        // Set application culture
        CultureInfo.CurrentUICulture = new CultureInfo("fr-FR");
        
        // Set resource manager
        var resxPath = "MyApp.Resources.SfKanban";
        SfKanbanResources.ResourceManager = new ResourceManager(
            resxPath, 
            Application.Current.GetType().Assembly);
        
        MainPage = new AppShell();
    }
}
```

### Resource File Content

**SfKanban.resx (English - Default):**
```xml
<data name="ItemsCount" xml:space="preserve">
    <value>Items: {0}</value>
</data>
<data name="MinimumLimit" xml:space="preserve">
    <value>Min: {0}</value>
</data>
<data name="MaximumLimit" xml:space="preserve">
    <value>Max: {0}</value>
</data>
```

**SfKanban.fr-FR.resx (French):**
```xml
<data name="ItemsCount" xml:space="preserve">
    <value>Éléments: {0}</value>
</data>
<data name="MinimumLimit" xml:space="preserve">
    <value>Min: {0}</value>
</data>
<data name="MaximumLimit" xml:space="preserve">
    <value>Max: {0}</value>
</data>
```

### Dynamic Culture Switching

```csharp
public class LocalizationService
{
    public void SetCulture(string cultureName)
    {
        var culture = new CultureInfo(cultureName);
        CultureInfo.CurrentUICulture = culture;
        
        // Restart or refresh UI to apply changes
    }
    
    public List<CultureInfo> GetAvailableCultures()
    {
        return new List<CultureInfo>
        {
            new CultureInfo("en-US"),
            new CultureInfo("fr-FR"),
            new CultureInfo("de-DE"),
            new CultureInfo("ar-SA"),
            new CultureInfo("zh-CN")
        };
    }
}
```

### Example: Language Selector

```xml
<Picker x:Name="languagePicker"
        Title="Select Language"
        SelectedIndexChanged="OnLanguageChanged">
    <Picker.ItemsSource>
        <x:Array Type="{x:Type x:String}">
            <x:String>English (en-US)</x:String>
            <x:String>Français (fr-FR)</x:String>
            <x:String>Deutsch (de-DE)</x:String>
            <x:String>العربية (ar-SA)</x:String>
        </x:Array>
    </Picker.ItemsSource>
</Picker>
```

```csharp
private void OnLanguageChanged(object sender, EventArgs e)
{
    var picker = (Picker)sender;
    var cultureName = picker.SelectedIndex switch
    {
        0 => "en-US",
        1 => "fr-FR",
        2 => "de-DE",
        3 => "ar-SA",
        _ => "en-US"
    };
    
    _localizationService.SetCulture(cultureName);
    
    // Set RTL if Arabic
    if (cultureName == "ar-SA")
    {
        kanban.FlowDirection = FlowDirection.RightToLeft;
    }
    else
    {
        kanban.FlowDirection = FlowDirection.LeftToRight;
    }
    
    // Refresh app (may require restart)
    Application.Current.MainPage = new AppShell();
}
```

## Combining RTL and Localization

For complete international support, combine both features:

```csharp
public void ConfigureForCulture(string cultureName)
{
    var culture = new CultureInfo(cultureName);
    
    // Set culture
    CultureInfo.CurrentUICulture = culture;
    
    // Set flow direction
    kanban.FlowDirection = culture.TextInfo.IsRightToLeft
        ? FlowDirection.RightToLeft
        : FlowDirection.LeftToRight;
    
    // Load localized resources
    var resxPath = $"MyApp.Resources.SfKanban";
    SfKanbanResources.ResourceManager = new ResourceManager(
        resxPath,
        Application.Current.GetType().Assembly);
}

// Usage
ConfigureForCulture("ar-SA");  // Arabic: RTL + Arabic resources
ConfigureForCulture("fr-FR");  // French: LTR + French resources
```

## Best Practices

### RTL Support

1. **Test with actual RTL languages** - Don't just reverse LTR
2. **Consider layout implications** - Images, icons may need mirroring
3. **Check text alignment** - Ensure text aligns correctly
4. **Test card templates** - Custom templates may need RTL adjustments

### Localization

1. **Use resource files** - Don't hardcode strings
2. **Provide fallbacks** - Default to English if translation missing
3. **Test all translations** - Verify text fits in UI
4. **Consider date/time formats** - Use culture-specific formatting
5. **Handle pluralization** - Some languages have complex plural rules

### Culture-Specific Formatting

```csharp
public class LocalizedCardViewModel
{
    public string FormattedDueDate
    {
        get
        {
            var culture = CultureInfo.CurrentUICulture;
            return DueDate.ToString("D", culture);  // Long date pattern
        }
    }
    
    public string FormattedPriority
    {
        get
        {
            // Load from resources
            return Resources.GetString($"Priority_{Priority}");
        }
    }
}
```

## Common Use Cases

### Use Case 1: Multi-Region Application

```csharp
// Detect user's region and configure
var userCulture = CultureInfo.CurrentCulture;
ConfigureForCulture(userCulture.Name);
```

### Use Case 2: User Preference

```csharp
// Load from user preferences
var preferredLanguage = await _settingsService.GetPreferredLanguageAsync();
ConfigureForCulture(preferredLanguage);
```

### Use Case 3: App Settings

```xml
<ContentPage>
    <StackLayout>
        <Label Text="Language Settings" />
        <Picker x:Name="languagePicker" />
        <Switch x:Name="rtlSwitch" />
        <Button Text="Apply" Clicked="OnApplySettings" />
    </StackLayout>
</ContentPage>
```

## Troubleshooting

### Issue: RTL not applying

**Check:**
1. `FlowDirection` property is set
2. Applied to correct control (SfKanban)
3. No parent override

### Issue: Localization not working

**Check:**
1. Resource files have correct build action (EmbeddedResource)
2. ResourceManager path is correct
3. Culture name matches file name (case-sensitive)
4. Resource keys match in all files

**Debug:**
```csharp
Debug.WriteLine($"Current Culture: {CultureInfo.CurrentUICulture.Name}");
Debug.WriteLine($"Resource Manager: {SfKanbanResources.ResourceManager}");
```

### Issue: Text not displaying in RTL

**Solution:** Ensure font supports RTL characters:
```xml
<Label Text="{Binding ArabicText}"
       FontFamily="NotoSansArabic" />
```

## Next Steps

- **Customize appearance:** See [customization.md](customization.md)
- **Handle events:** See [events.md](events.md)
- **Configure columns:** See [columns.md](columns.md)