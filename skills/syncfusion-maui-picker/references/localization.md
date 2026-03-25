# Localization in .NET MAUI Picker

Learn how to localize the Syncfusion .NET MAUI Picker control to support multiple languages and cultures.

## Table of Contents
- [Overview](#overview)
- [Setting Up Localization](#setting-up-localization)
- [Changing Application Culture](#changing-application-culture)
- [RTL Support](#rtl-support)
- [Complete Example](#complete-example)

## Overview

Localization is the process of translating application resources into different languages for specific cultures. The Picker control can be fully localized by adding resource files and setting the application culture.

## Setting Up Localization

### Step 1: Create Resource Files

1. Create a new folder named `Resources` in your project (if it doesn't exist)

2. Right-click on the `Resources` folder
3. Select **Add > New Item**
4. Choose **Resource File** option
5. Name the file using the format: `SfPicker.<culture-code>.resx`
   - Example: `SfPicker.fr-FR.resx` for French (France)
   - Example: `SfPicker.es-ES.resx` for Spanish (Spain)
   - Example: `SfPicker.de-DE.resx` for German (Germany)

**Culture Code Format:** `<language>-<COUNTRY>`
- `fr-FR`: French (France)
- `en-US`: English (United States)
- `de-DE`: German (Germany)
- `ja-JP`: Japanese (Japan)
- `zh-CN`: Chinese (China)

### Step 2: Configure Resource File

1. Set the **Build Action** to `EmbeddedResource`
2. Open the resource file in the Resource Designer
3. Add Name/Value pairs for localized strings

**Example: SfPicker.fr-FR.resx**

| Name | Value (French) |
|------|----------------|
| OkButtonText | D'accord |
| CancelButtonText | Annuler |
| HeaderText | Sélectionnez |

### Step 3: Set Application Culture

Configure the culture in `App.xaml.cs`:

```csharp
using System.Resources;
using System.Globalization;
using Syncfusion.Maui.Picker;

namespace YourApp;

public partial class App : Application
{
    public App()
    {
        InitializeComponent();

        // Set the culture
        CultureInfo.CurrentUICulture = new CultureInfo("fr-FR");
        
        // Set the resource manager
        // Replace "YourApp.Resources.SfPicker" with your actual namespace and resource path
        SfPickerResources.ResourceManager = new ResourceManager(
            "YourApp.Resources.SfPicker", 
            Application.Current.GetType().Assembly);

        MainPage = new AppShell();
    }
}
```

## Complete Localization Example

### Project Structure

```
YourApp/
├── Resources/
│   ├── SfPicker.resx (default/English)
│   ├── SfPicker.fr-FR.resx (French)
│   ├── SfPicker.es-ES.resx (Spanish)
│   └── SfPicker.de-DE.resx (German)
└── App.xaml.cs
```

### Default Resource File (SfPicker.resx)

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
    <data name="OkButtonText" xml:space="preserve">
        <value>OK</value>
    </data>
    <data name="CancelButtonText" xml:space="preserve">
        <value>Cancel</value>
    </data>
    <data name="SelectText" xml:space="preserve">
        <value>Select</value>
    </data>
</root>
```

### French Resource File (SfPicker.fr-FR.resx)

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
    <data name="OkButtonText" xml:space="preserve">
        <value>D'accord</value>
    </data>
    <data name="CancelButtonText" xml:space="preserve">
        <value>Annuler</value>
    </data>
    <data name="SelectText" xml:space="preserve">
        <value>Sélectionner</value>
    </data>
</root>
```

### Spanish Resource File (SfPicker.es-ES.resx)

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
    <data name="OkButtonText" xml:space="preserve">
        <value>Aceptar</value>
    </data>
    <data name="CancelButtonText" xml:space="preserve">
        <value>Cancelar</value>
    </data>
    <data name="SelectText" xml:space="preserve">
        <value>Seleccionar</value>
    </data>
</root>
```

## Localizing Custom Content

### Localizing Header and Column Text

**Create Custom Resource File:**

**AppResources.resx (English):**
```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
    <data name="PickerHeaderText" xml:space="preserve">
        <value>Select a Color</value>
    </data>
    <data name="ColumnHeaderText" xml:space="preserve">
        <value>Colors</value>
    </data>
</root>
```

**AppResources.fr-FR.resx (French):**
```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
    <data name="PickerHeaderText" xml:space="preserve">
        <value>Sélectionnez une couleur</value>
    </data>
    <data name="ColumnHeaderText" xml:space="preserve">
        <value>Couleurs</value>
    </data>
</root>
```

**Resource Manager Class:**
```csharp
using System.Globalization;
using System.Resources;

namespace YourApp.Resources
{
    public static class AppResources
    {
        private static ResourceManager resourceManager;

        static AppResources()
        {
            resourceManager = new ResourceManager(
                "YourApp.Resources.AppResources", 
                typeof(AppResources).Assembly);
        }

        public static string PickerHeaderText => 
            resourceManager.GetString("PickerHeaderText", CultureInfo.CurrentUICulture);

        public static string ColumnHeaderText => 
            resourceManager.GetString("ColumnHeaderText", CultureInfo.CurrentUICulture);
    }
}
```

**XAML Usage:**
```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.HeaderView>
        <picker:PickerHeaderView Text="{x:Static local:AppResources.PickerHeaderText}" 
                                 Height="40" />
    </picker:SfPicker.HeaderView>
    
    <picker:SfPicker.Columns>
        <picker:PickerColumn HeaderText="{x:Static local:AppResources.ColumnHeaderText}"
                            ItemsSource="{Binding Colors}" />
    </picker:SfPicker.Columns>
</picker:SfPicker>
```

## Localizing Data Items

### Localizing Picker Items

**Color Resources (AppResources.resx):**
```xml
<data name="Color_Red" xml:space="preserve">
    <value>Red</value>
</data>
<data name="Color_Blue" xml:space="preserve">
    <value>Blue</value>
</data>
<data name="Color_Green" xml:space="preserve">
    <value>Green</value>
</data>
```

**Color Resources (AppResources.fr-FR.resx):**
```xml
<data name="Color_Red" xml:space="preserve">
    <value>Rouge</value>
</data>
<data name="Color_Blue" xml:space="preserve">
    <value>Bleu</value>
</data>
<data name="Color_Green" xml:space="preserve">
    <value>Vert</value>
</data>
```

**ViewModel:**
```csharp
public class ColorPickerViewModel
{
    public ObservableCollection<string> Colors { get; set; }

    public ColorPickerViewModel()
    {
        Colors = new ObservableCollection<string>
        {
            AppResources.Color_Red,
            AppResources.Color_Blue,
            AppResources.Color_Green
        };
    }
}
```

## Dynamic Culture Switching

### Runtime Culture Change

```csharp
public class LocalizationService
{
    public void SetCulture(string cultureCode)
    {
        CultureInfo newCulture = new CultureInfo(cultureCode);
        
        // Set the UI culture
        CultureInfo.CurrentUICulture = newCulture;
        CultureInfo.CurrentCulture = newCulture;
        
        // Update resource manager
        SfPickerResources.ResourceManager = new ResourceManager(
            "YourApp.Resources.SfPicker", 
            Application.Current.GetType().Assembly);
        
        // Reload the current page to apply changes
        Application.Current.MainPage = new AppShell();
    }
}
```

**Usage in ViewModel:**
```csharp
public class SettingsViewModel
{
    private LocalizationService localizationService;

    public ICommand ChangeCultureCommand { get; set; }

    public SettingsViewModel()
    {
        localizationService = new LocalizationService();
        ChangeCultureCommand = new Command<string>(ChangeCulture);
    }

    private void ChangeCulture(string cultureCode)
    {
        localizationService.SetCulture(cultureCode);
    }
}
```

**XAML:**
```xml
<VerticalStackLayout Spacing="10">
    <Button Text="English" 
            Command="{Binding ChangeCultureCommand}"
            CommandParameter="en-US"/>
    <Button Text="Français" 
            Command="{Binding ChangeCultureCommand}"
            CommandParameter="fr-FR"/>
    <Button Text="Español" 
            Command="{Binding ChangeCultureCommand}"
            CommandParameter="es-ES"/>
</VerticalStackLayout>
```

## Right-to-Left (RTL) Support

### Enabling RTL for RTL Languages

For languages like Arabic (`ar`) or Hebrew (`he`), enable RTL layout:

**App.xaml.cs:**
```csharp
public App()
{
    InitializeComponent();

    // Set culture
    var culture = new CultureInfo("ar-SA"); // Arabic
    CultureInfo.CurrentUICulture = culture;
    CultureInfo.CurrentCulture = culture;

    // Enable RTL if needed
    if (culture.TextInfo.IsRightToLeft)
    {
        // Platform-specific RTL handling
        MainPage = new AppShell
        {
            FlowDirection = FlowDirection.RightToLeft
        };
    }
    else
    {
        MainPage = new AppShell();
    }
}
```

## Supported Cultures

Common culture codes:

| Language | Culture Code | Region |
|----------|-------------|---------|
| English | en-US | United States |
| French | fr-FR | France |
| Spanish | es-ES | Spain |
| German | de-DE | Germany |
| Italian | it-IT | Italy |
| Portuguese | pt-BR | Brazil |
| Japanese | ja-JP | Japan |
| Chinese (Simplified) | zh-CN | China |
| Chinese (Traditional) | zh-TW | Taiwan |
| Korean | ko-KR | South Korea |
| Arabic | ar-SA | Saudi Arabia |
| Hebrew | he-IL | Israel |
| Russian | ru-RU | Russia |
| Hindi | hi-IN | India |

## Best Practices

1. **Always provide a default resource file** (without culture code)
2. **Use neutral culture files** (`fr.resx`) for common translations across regions
3. **Test all supported cultures** before release
4. **Localize all user-visible text** including headers, buttons, and error messages
5. **Consider date and number formatting** based on culture
6. **Handle RTL languages** appropriately
7. **Use translation services** for accuracy
8. **Keep resource keys consistent** across all culture files

## Common Issues and Solutions

### Issue: Localization not working

**Solution:**
- Verify resource file Build Action is `EmbeddedResource`
- Check culture code format is correct
- Ensure ResourceManager path matches your project structure
- Verify CurrentUICulture is set before UI initialization

### Issue: Missing translations

**Solution:**
- Ensure all resource keys exist in all culture files
- Provide fallback to default resource file
- Add logging to detect missing keys

### Issue: Text not updating after culture change

**Solution:**
- Reload the page or recreate the picker
- Use INotifyPropertyChanged for dynamic updates
- Restart the app for culture changes to take full effect

## Complete Localized Example

**Picker with Full Localization:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:picker="clr-namespace:Syncfusion.Maui.Picker;assembly=Syncfusion.Maui.Picker"
             xmlns:local="clr-namespace:YourApp.Resources"
             x:Class="YourApp.PickerPage">
    
    <picker:SfPicker x:Name="picker"
                     HeightRequest="350">
        
        <picker:SfPicker.HeaderView>
            <picker:PickerHeaderView Text="{x:Static local:AppResources.PickerHeaderText}" 
                                     Height="40" />
        </picker:SfPicker.HeaderView>
        
        <picker:SfPicker.Columns>
            <picker:PickerColumn HeaderText="{x:Static local:AppResources.ColumnHeaderText}"
                                ItemsSource="{Binding LocalizedItems}" />
        </picker:SfPicker.Columns>
        
        <picker:SfPicker.ColumnHeaderView>
            <picker:PickerColumnHeaderView Height="40" />
        </picker:SfPicker.ColumnHeaderView>
        
        <picker:SfPicker.FooterView>
            <picker:PickerFooterView ShowOkButton="True" Height="40" />
            <!-- OK and Cancel button text are automatically localized -->
        </picker:SfPicker.FooterView>
        
    </picker:SfPicker>
    
</ContentPage>
```

This implementation ensures that your Picker control is fully localized and ready for international users.
