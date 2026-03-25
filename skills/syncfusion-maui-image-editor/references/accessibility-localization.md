# Accessibility & Localization

This guide covers accessibility features, keyboard shortcuts, localization, and right-to-left (RTL) support in the .NET MAUI ImageEditor control.

## Table of Contents
- [Accessibility Overview](#accessibility-overview)
- [Keyboard Shortcuts](#keyboard-shortcuts)
- [Screen Reader Support](#screen-reader-support)
- [Localization](#localization)
- [Right-to-Left (RTL) Support](#right-to-left-rtl-support)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)
- [Next Steps](#next-steps)

## Accessibility Overview

The ImageEditor is designed to be accessible, providing voice descriptions of toolbar icons and sliders, keyboard navigation, and support for assistive technologies.

### Accessibility Features

- **Keyboard Shortcuts** - Complete keyboard navigation
- **Screen Reader Support** - Descriptive labels for all elements
- **WCAG Compliance** - Meets accessibility standards
- **Focus Indicators** - Clear visual focus states
- **Color Contrast** - Customizable for high contrast needs

### When to Prioritize Accessibility

- Government/public sector applications
- Healthcare and medical apps
- Educational platforms
- Enterprise applications
- Consumer apps with diverse user bases

## Keyboard Shortcuts

The ImageEditor supports comprehensive keyboard shortcuts for efficient operation without mouse/touch input.

### Toolbar Navigation

| Shortcut | Action |
|----------|--------|
| `Tab` | Move to next toolbar item |
| `Shift + Tab` | Move to previous toolbar item |
| `Enter` | Activate selected toolbar item (triggers `ToolbarItemSelected` event) |

### Editing Operations

| Shortcut | Action |
|----------|--------|
| `Ctrl + Z` | Undo (Command + Z on macOS) |
| `Ctrl + Y` | Redo (Command + Y on macOS) |
| `Ctrl + S` | Save image (Command + S on macOS) |
| `Ctrl + O` | Open image browser (Command + O on macOS) |

**Note:** Use `Command` key instead of `Ctrl` on macOS.

### Implementation

Keyboard shortcuts are automatically enabled. Handle the `ToolbarItemSelected` event to react to keyboard-triggered toolbar actions:

```xml
<imageEditor:SfImageEditor x:Name="imageEditor"
                          Source="photo.jpg"
                          ToolbarItemSelected="OnToolbarItemSelected" />
```

```csharp
private void OnToolbarItemSelected(object sender, ToolbarItemSelectedEventArgs e)
{
    // Responds to both keyboard and mouse/touch
    Console.WriteLine($"Item selected via keyboard or click: {e.ToolbarItem.Name}");
}
```

## Screen Reader Support

The ImageEditor provides descriptive labels for screen readers, making the interface accessible to visually impaired users.

### What's Announced

- **Toolbar Items** - Names and functions ("Save button", "Crop tool")
- **Sliders** - Current values and purposes ("Brightness: 50%")
- **Actions** - Operation results ("Image saved", "Annotation added")
- **State Changes** - Mode changes ("Crop mode activated")

### Semantic Properties

The control automatically provides semantic properties for accessibility. You can enhance this with custom automation properties:

```xml
<imageEditor:SfImageEditor Source="photo.jpg"
                          AutomationId="MainImageEditor"
                          SemanticProperties.Description="Photo editing control with toolbar"
                          SemanticProperties.Hint="Edit photos with crop, effects, and annotations" />
```

### Custom Accessibility Labels

```csharp
// Set accessible description
SemanticProperties.SetDescription(imageEditor, 
    "Main image editor with full editing capabilities");

// Set hint for screen readers
SemanticProperties.SetHint(imageEditor,
    "Use toolbar buttons to edit the image. Press Tab to navigate between tools.");
```

## Localization

Localize the ImageEditor interface to support multiple languages by providing translated resource files.

### Setup Localization

**Step 1: Set Application Culture**

In `App.xaml.cs`:

```csharp
using Syncfusion.Maui.ImageEditor;
using System.Resources;
using System.Globalization;

public partial class App : Application
{
    public App()
    {
        InitializeComponent();
        
        // Set desired culture
        CultureInfo.CurrentUICulture = new CultureInfo("fr-FR");
        
        // Set resource manager
        SfImageEditorResources.ResourceManager = new ResourceManager(
            "YourApp.Resources.SfImageEditor", 
            Application.Current.GetType().Assembly);
            
        MainPage = new MainPage();
    }
}
```

**Step 2: Add Default Resource File**

1. Add the default `SfImageEditor.resx` file to `Resources` folder
2. Download from Syncfusion or create with default strings

**Step 3: Add Localized Resource Files**

1. Right-click `Resources` folder → Add → New Item
2. Select Resource File
3. Name it `SfImageEditor.<culture>.resx` (e.g., `SfImageEditor.fr-FR.resx` for French)
4. Add Name/Value pairs with translations

### Resource File Example

**SfImageEditor.fr-FR.resx:**

| Name | Value (French) |
|------|----------------|
| Save | Enregistrer |
| Undo | Annuler |
| Redo | Rétablir |
| Crop | Rogner |
| Effects | Effets |
| Brightness | Luminosité |
| Contrast | Contraste |
| Cancel | Annuler |

### Supported Cultures

You can create resource files for any culture:
- `fr-FR` - French (France)
- `es-ES` - Spanish (Spain)
- `de-DE` - German (Germany)
- `ja-JP` - Japanese (Japan)
- `zh-CN` - Chinese (Simplified)
- `ar-SA` - Arabic (Saudi Arabia)
- etc.

### Sample Localization Code

```csharp
public class LocalizationHelper
{
    public static void SetCulture(string cultureCode)
    {
        CultureInfo culture = new CultureInfo(cultureCode);
        CultureInfo.CurrentUICulture = culture;
        CultureInfo.CurrentCulture = culture;
        
        // Update resource manager
        SfImageEditorResources.ResourceManager = new ResourceManager(
            "ImageEditorApp.Resources.SfImageEditor",
            typeof(App).Assembly);
    }
}

// Usage
LocalizationHelper.SetCulture("fr-FR");  // Switch to French
LocalizationHelper.SetCulture("es-ES");  // Switch to Spanish
```

## Right-to-Left (RTL) Support

The ImageEditor supports right-to-left layouts for languages like Arabic, Hebrew, and Persian.

### Enable RTL Layout

**XAML:**
```xml
<imageEditor:SfImageEditor Source="photo.jpg" 
                          FlowDirection="RightToLeft" />
```

**C#:**
```csharp
SfImageEditor imageEditor = new SfImageEditor();
imageEditor.Source = "photo.jpg";
imageEditor.FlowDirection = FlowDirection.RightToLeft;
this.Content = imageEditor;
```

### RTL Behavior

When RTL is enabled:
- Toolbar items display right-to-left
- Navigation flows from right to left
- Text alignment adjusts automatically
- Annotations render correctly for RTL text

### Combined RTL and Localization

```csharp
public void SetupArabicLocale()
{
    // Set Arabic culture
    CultureInfo.CurrentUICulture = new CultureInfo("ar-SA");
    
    // Set RTL layout
    imageEditor.FlowDirection = FlowDirection.RightToLeft;
    
    // Update resource manager
    SfImageEditorResources.ResourceManager = new ResourceManager(
        "YourApp.Resources.SfImageEditor",
        typeof(App).Assembly);
}
```

## Common Patterns

### Multi-Language Support

```csharp
public class LanguageManager
{
    private Dictionary<string, string> supportedLanguages = new()
    {
        { "en-US", "English" },
        { "fr-FR", "Français" },
        { "es-ES", "Español" },
        { "de-DE", "Deutsch" },
        { "ar-SA", "العربية" }
    };
    
    public void ChangeLanguage(string cultureCode)
    {
        // Set culture
        CultureInfo culture = new CultureInfo(cultureCode);
        CultureInfo.CurrentUICulture = culture;
        
        // Set RTL for RTL languages
        bool isRTL = culture.TextInfo.IsRightToLeft;
        imageEditor.FlowDirection = isRTL ? 
            FlowDirection.RightToLeft : 
            FlowDirection.LeftToRight;
        
        // Update resources
        SfImageEditorResources.ResourceManager = new ResourceManager(
            "YourApp.Resources.SfImageEditor",
            typeof(App).Assembly);
    }
    
    public List<string> GetSupportedLanguages() 
        => supportedLanguages.Values.ToList();
}
```

### Accessibility-Enhanced UI

```xml
<Grid RowDefinitions="*, Auto">
    <imageEditor:SfImageEditor x:Name="imageEditor"
                              Source="photo.jpg"
                              SemanticProperties.Description="Main photo editor"
                              SemanticProperties.Hint="Use keyboard shortcuts for quick access" />
    
    <VerticalStackLayout Grid.Row="1" Padding="10">
        <Label Text="Keyboard Shortcuts:" FontAttributes="Bold" />
        <Label Text="Ctrl+Z: Undo | Ctrl+Y: Redo | Ctrl+S: Save" />
        <Label Text="Tab: Next tool | Enter: Select tool" />
    </VerticalStackLayout>
</Grid>
```

### High Contrast Theme for Accessibility

```csharp
private void ApplyAccessibleTheme()
{
    // High contrast colors
    imageEditor.Background = Brush.SolidColorBrush(Colors.Black);
    imageEditor.SelectionStroke = Colors.Yellow;
    
    imageEditor.ToolbarSettings.Background = 
        Brush.SolidColorBrush(Colors.Black);
    imageEditor.ToolbarSettings.Stroke = Colors.White;
    
    // Large, clear color palette
    imageEditor.ToolbarSettings.ColorPalette.Clear();
    imageEditor.ToolbarSettings.ColorPalette.Add(Colors.White);
    imageEditor.ToolbarSettings.ColorPalette.Add(Colors.Yellow);
    imageEditor.ToolbarSettings.ColorPalette.Add(Colors.Cyan);
    imageEditor.ToolbarSettings.ColorPalette.Add(Colors.Magenta);
    
    // Semantic properties for screen readers
    SemanticProperties.SetDescription(imageEditor,
        "High contrast image editor optimized for accessibility");
}
```

### Language Picker Integration

```xml
<Grid RowDefinitions="Auto, *">
    <Picker x:Name="languagePicker"
            Title="Select Language"
            SelectedIndexChanged="OnLanguageChanged"
            Margin="10">
        <Picker.Items>
            <x:String>English</x:String>
            <x:String>Français</x:String>
            <x:String>Español</x:String>
            <x:String>العربية</x:String>
        </Picker.Items>
    </Picker>
    
    <imageEditor:SfImageEditor x:Name="imageEditor"
                              Grid.Row="1"
                              Source="photo.jpg" />
</Grid>
```

```csharp
private void OnLanguageChanged(object sender, EventArgs e)
{
    string[] cultureCodes = { "en-US", "fr-FR", "es-ES", "ar-SA" };
    int selectedIndex = languagePicker.SelectedIndex;
    
    if (selectedIndex >= 0)
    {
        string cultureCode = cultureCodes[selectedIndex];
        ChangeLanguage(cultureCode);
    }
}

private void ChangeLanguage(string cultureCode)
{
    CultureInfo culture = new CultureInfo(cultureCode);
    CultureInfo.CurrentUICulture = culture;
    
    // RTL for Arabic
    imageEditor.FlowDirection = culture.TextInfo.IsRightToLeft ?
        FlowDirection.RightToLeft :
        FlowDirection.LeftToRight;
    
    // Reload resources
    SfImageEditorResources.ResourceManager = new ResourceManager(
        "YourApp.Resources.SfImageEditor",
        typeof(App).Assembly);
        
    // Restart page to apply changes
    // Application.Current.MainPage = new MainPage();
}
```

### Screen Reader Announcements

```csharp
private void AnnounceForScreenReader(string message)
{
    #if ANDROID || IOS
    // Make announcements for screen readers
    SemanticScreenReader.Announce(message);
    #endif
}

// Usage
private void OnImageSaved(object sender, ImageSavedEventArgs e)
{
    AnnounceForScreenReader("Image saved successfully");
}

private void OnAnnotationAdded()
{
    AnnounceForScreenReader("Annotation added to image");
}
```

## Troubleshooting

### Issue: Keyboard Shortcuts Not Working

**Cause:** Focus not on ImageEditor or keyboard handling intercepted.

**Solution:**
```csharp
// Ensure ImageEditor has focus
imageEditor.Focus();
```

### Issue: Localization Not Applied

**Cause:** Resource manager not set or culture not changed.

**Solution:**
```csharp
// Verify culture is set
CultureInfo.CurrentUICulture = new CultureInfo("fr-FR");

// Verify resource manager path
SfImageEditorResources.ResourceManager = new ResourceManager(
    "YourApp.Resources.SfImageEditor",  // Must match namespace
    typeof(App).Assembly);
```

### Issue: RTL Layout Not Working

**Cause:** FlowDirection not set or platform not supporting RTL.

**Solution:**
```csharp
// Explicitly set RTL
imageEditor.FlowDirection = FlowDirection.RightToLeft;

// Verify it's applied
Console.WriteLine($"FlowDirection: {imageEditor.FlowDirection}");
```

### Issue: Resource File Not Found

**Cause:** Incorrect naming or location.

**Solution:**
- Ensure file named `SfImageEditor.<culture>.resx`
- Place in `Resources` folder
- Set Build Action to Embedded Resource
- Match namespace in resource manager path

### Issue: Screen Reader Not Announcing

**Cause:** Semantic properties not set or platform limitations.

**Solution:**
```csharp
// Set semantic properties
SemanticProperties.SetDescription(imageEditor, 
    "Image editor with toolbar");

// Use screen reader API directly
#if ANDROID || IOS
SemanticScreenReader.Announce("Action completed");
#endif
```

## Next Steps

- **Getting Started:** [getting-started.md](getting-started.md)
- **Styling & Customization:** [styling-customization.md](styling-customization.md)
- **Toolbar:** [toolbar.md](toolbar.md)
- **Events:** [events.md](events.md)