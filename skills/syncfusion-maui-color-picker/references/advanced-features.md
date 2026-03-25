# Advanced Features

This guide covers advanced SfColorPicker features including localization for multi-language support and the Liquid Glass effect for modern visual aesthetics.

## Localization

Localization enables the Color Picker to display text in different languages based on the user's culture settings.

### Overview

The SfColorPicker can be localized by:
1. Setting the application's `CurrentUICulture`
2. Creating resource files (`.resx`) for each supported language
3. Registering the `ResourceManager` with `SfColorPickerResources`

### Localizable Elements

- Apply button text
- Cancel button text
- Recent colors label
- Input field labels (HEX, R, G, B, H, S, V, A)
- Validation messages

### Step 1: Set CurrentUICulture

Configure the culture in `App.xaml.cs`:

```csharp
using Syncfusion.Maui.Inputs;
using System.Globalization;
using System.Resources;

public partial class App : Application
{
    public App()
    {
        InitializeComponent();
        
        // Set culture (e.g., French)
        CultureInfo.CurrentUICulture = new CultureInfo("fr-FR");
        
        // Register resource manager
        // Replace ResXPath with actual path to your .resx file
        string resxPath = "ColorPickerLocalization.Resources.SfColorPicker";
        SfColorPickerResources.ResourceManager = new ResourceManager(
            resxPath, 
            Application.Current.GetType().Assembly
        );
        
        MainPage = new AppShell();
    }
}
```

### Step 2: Create Resource Files

#### File Naming Convention

Resource files must follow this pattern:
- **Default (English):** `SfColorPicker.resx`
- **French:** `SfColorPicker.fr-FR.resx`
- **Spanish:** `SfColorPicker.es-ES.resx`
- **German:** `SfColorPicker.de-DE.resx`
- **Japanese:** `SfColorPicker.ja-JP.resx`

#### File Location

Place resource files in a `Resources` folder in your project:

```
MyProject/
├── Resources/
│   ├── SfColorPicker.resx
│   ├── SfColorPicker.fr-FR.resx
│   ├── SfColorPicker.es-ES.resx
│   └── SfColorPicker.de-DE.resx
├── App.xaml
└── ...
```

#### Build Action

Set **Build Action** to `EmbeddedResource` for all `.resx` files:
1. Right-click the `.resx` file
2. Select **Properties**
3. Set **Build Action** to `EmbeddedResource`

### Step 3: Add Localized Strings

#### Creating a Resource File (Visual Studio)

1. Right-click the `Resources` folder
2. Select **Add > New Item**
3. Choose **Resources File** (.resx)
4. Name it `SfColorPicker.fr-FR.resx` (for French)
5. Click **Add**

#### Adding Name/Value Pairs

Open the `.resx` file in the Resource Designer and add key-value pairs:

**Example: French (fr-FR)**

| Name | Value |
|------|-------|
| Apply | Appliquer |
| Cancel | Annuler |
| RecentColors | Couleurs récentes |
| NoColor | Aucune couleur |
| Red | Rouge |
| Green | Vert |
| Blue | Bleu |
| Alpha | Alpha |
| Hue | Teinte |
| Saturation | Saturation |
| Value | Valeur |

**Example: Spanish (es-ES)**

| Name | Value |
|------|-------|
| Apply | Aplicar |
| Cancel | Cancelar |
| RecentColors | Colores recientes |
| NoColor | Sin color |
| Red | Rojo |
| Green | Verde |
| Blue | Azul |
| Alpha | Alfa |

### Complete Localization Example

#### French Localization

```csharp
// App.xaml.cs
using Syncfusion.Maui.Inputs;
using System.Globalization;
using System.Resources;

public partial class App : Application
{
    public App()
    {
        InitializeComponent();
        
        // Set French culture
        CultureInfo.CurrentUICulture = new CultureInfo("fr-FR");
        
        // Register resource manager
        SfColorPickerResources.ResourceManager = new ResourceManager(
            "MyColorPickerApp.Resources.SfColorPicker",
            typeof(App).Assembly
        );
        
        MainPage = new NavigationPage(new MainPage());
    }
}
```

#### SfColorPicker.fr-FR.resx

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
  <data name="Apply" xml:space="preserve">
    <value>Appliquer</value>
  </data>
  <data name="Cancel" xml:space="preserve">
    <value>Annuler</value>
  </data>
  <data name="RecentColors" xml:space="preserve">
    <value>Couleurs récentes</value>
  </data>
  <data name="NoColor" xml:space="preserve">
    <value>Aucune couleur</value>
  </data>
  <data name="Red" xml:space="preserve">
    <value>Rouge</value>
  </data>
  <data name="Green" xml:space="preserve">
    <value>Vert</value>
  </data>
  <data name="Blue" xml:space="preserve">
    <value>Bleu</value>
  </data>
</root>
```

### Dynamic Culture Switching

Allow users to change language at runtime:

```csharp
public class LanguageService
{
    public void SetCulture(string cultureCode)
    {
        var culture = new CultureInfo(cultureCode);
        CultureInfo.CurrentUICulture = culture;
        CultureInfo.CurrentCulture = culture;
        
        // Trigger UI refresh
        // You may need to recreate pages or use messaging
    }
}

// Usage
var languageService = new LanguageService();

// Switch to Spanish
languageService.SetCulture("es-ES");

// Switch to German
languageService.SetCulture("de-DE");
```

### Troubleshooting Localization

#### Issue: Localization not working

**Solutions:**
1. Verify `Build Action` is set to `EmbeddedResource`
2. Check namespace in `ResourceManager` matches your project
3. Ensure `CurrentUICulture` is set before creating any UI
4. Verify resource file naming follows `SfColorPicker.<culture>.resx` pattern

#### Issue: Some strings not translating

**Solution:** Ensure all required keys are present in the `.resx` file. Missing keys fall back to default (English).

#### Issue: Resource file not found

**Solution:** Check the full namespace path:

```csharp
// Format: <AssemblyName>.Resources.SfColorPicker
SfColorPickerResources.ResourceManager = new ResourceManager(
    "MyApp.Resources.SfColorPicker",  // Full namespace
    typeof(App).Assembly
);
```

## Liquid Glass Effect

The Liquid Glass effect (also called acrylic or glass morphism) provides a modern, frosted translucent appearance that blends with the background.

### Requirements

- **.NET 10** or later
- **iOS 26** or **macOS 26**
- Not supported on Android/Windows

### Enabling Liquid Glass

```xaml
<Grid>
    <!-- Background for glass effect visibility -->
    <Image Source="wallpaper.jpg" Aspect="AspectFill"/>
    
    <inputs:SfColorPicker EnableLiquidGlassEffect="True"/>
</Grid>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    EnableLiquidGlassEffect = true
};
```

### When to Use

- **Modern UI designs** requiring premium aesthetics
- **Over rich backgrounds** (images, gradients)
- **iOS/macOS apps** where platform support is guaranteed
- **Premium applications** targeting latest platform versions

### Behavior and Tips

1. **Visibility:** Effect is most noticeable over colorful or detailed backgrounds

2. **Performance:** May impact performance on lower-end devices; test thoroughly

3. **Platform Detection:** Check platform before enabling:

```csharp
#if IOS || MACCATALYST
    if (DeviceInfo.Version.Major >= 26)
    {
        colorPicker.EnableLiquidGlassEffect = true;
    }
#endif
```

4. **Enhanced Styling:** Combine with transparent slider thumb for cohesive look:

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    EnableLiquidGlassEffect = true,
    SliderThumbStroke = Colors.Transparent,
    SliderThumbFill = new SolidColorBrush(Colors.White)
};
```

### Complete Liquid Glass Example

```xaml
<Grid>
    <!-- Rich background -->
    <Image Source="gradient_background.jpg" 
           Aspect="AspectFill"/>
    
    <!-- Color picker with glass effect -->
    <inputs:SfColorPicker x:Name="colorPicker"
                          EnableLiquidGlassEffect="True"
                          ColorMode="Spectrum"
                          VerticalOptions="Center"
                          HorizontalOptions="Center"
                          PopupBackground="Transparent"
                          SliderThumbStroke="Transparent"
                          SliderThumbFill="White"/>
</Grid>
```

### Platform-Specific Implementation

```csharp
public class ModernColorPickerPage : ContentPage
{
    public ModernColorPickerPage()
    {
        // Background
        var backgroundImage = new Image
        {
            Source = "modern_gradient.png",
            Aspect = Aspect.AspectFill
        };
        
        // Color picker
        var colorPicker = new SfColorPicker
        {
            VerticalOptions = LayoutOptions.Center,
            HorizontalOptions = LayoutOptions.Center
        };
        
        // Enable liquid glass on supported platforms
#if IOS || MACCATALYST
        if (DeviceInfo.Version.Major >= 26)
        {
            colorPicker.EnableLiquidGlassEffect = true;
            colorPicker.SliderThumbStroke = Colors.Transparent;
            colorPicker.SliderThumbFill = new SolidColorBrush(Colors.White);
        }
#endif
        
        Content = new Grid
        {
            Children = { backgroundImage, colorPicker }
        };
    }
}
```

### Background Recommendations

For optimal Liquid Glass visibility:

**✅ Good Backgrounds:**
- Gradient images
- Colorful photographs
- Textured backgrounds
- Vibrant illustrations

**❌ Avoid:**
- Plain solid colors
- Pure white or black backgrounds
- Low-contrast images
- Very busy/detailed patterns (may reduce clarity)

### Accessibility Considerations

The Liquid Glass effect may reduce contrast and readability. Provide alternatives:

```csharp
public class AccessibleColorPickerPage : ContentPage
{
    public AccessibleColorPickerPage()
    {
        var colorPicker = new SfColorPicker();
        
        // Check if reduced transparency is enabled (accessibility setting)
        bool useReducedTransparency = Preferences.Get("UseHighContrast", false);
        
        if (!useReducedTransparency && DeviceInfo.Platform == DevicePlatform.iOS)
        {
            colorPicker.EnableLiquidGlassEffect = true;
        }
        else
        {
            // Use solid background for better accessibility
            colorPicker.PopupBackground = Colors.White;
        }
        
        Content = colorPicker;
    }
}
```

## Combining Advanced Features

### Localized Color Picker with Liquid Glass

```csharp
// App.xaml.cs
public partial class App : Application
{
    public App()
    {
        InitializeComponent();
        
        // Setup localization
        CultureInfo.CurrentUICulture = new CultureInfo("fr-FR");
        SfColorPickerResources.ResourceManager = new ResourceManager(
            "MyApp.Resources.SfColorPicker",
            typeof(App).Assembly
        );
        
        MainPage = new NavigationPage(new MainPage());
    }
}

// MainPage.xaml.cs
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        // Create color picker
        var colorPicker = new SfColorPicker();
        
        // Enable glass effect on supported platforms
#if IOS || MACCATALYST
        if (DeviceInfo.Version.Major >= 26)
        {
            colorPicker.EnableLiquidGlassEffect = true;
        }
#endif
        
        Content = new Grid
        {
            Children =
            {
                new Image { Source = "background.jpg", Aspect = Aspect.AspectFill },
                colorPicker
            }
        };
    }
}
```

## Best Practices

### Localization

1. **Fallback:** Always provide a default `.resx` file (English) as fallback
2. **Completeness:** Ensure all keys exist in every localized `.resx` file
3. **Testing:** Test each supported culture thoroughly
4. **RTL Support:** Consider right-to-left languages (Arabic, Hebrew)
5. **Date/Number Formats:** Culture affects more than just text

### Liquid Glass

1. **Platform Check:** Always verify platform support before enabling
2. **Performance:** Test on target devices, especially older ones
3. **Backgrounds:** Use visually rich backgrounds to showcase effect
4. **Contrast:** Ensure UI elements remain readable
5. **Fallback:** Provide non-glass alternative for unsupported platforms
