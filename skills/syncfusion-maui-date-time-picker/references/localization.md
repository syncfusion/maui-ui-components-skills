# Localization

This guide explains how to localize the DateTimePicker for different languages and cultures.

## Overview

Localization allows you to translate the DateTimePicker's UI elements into different languages, making your app accessible to global audiences.

**Localizable Strings:**
- Day, Month, Year
- Hour, Minute, Second, Meridiem (AM/PM)
- OK button text
- Cancel button text

**Localization Method:** Resource files (.resx) with culture-specific translations

## Localizable Components

The following picker elements can be localized:

| Component | Default (English) | Description |
|-----------|-------------------|-------------|
| Day | Day | Day column header |
| Month | Month | Month column header |
| Year | Year | Year column header |
| Hour | Hour | Hour column header |
| Minute | Minute | Minute column header |
| Second | Second | Second column header |
| Meridiem | AM/PM | AM/PM indicator |
| OK | OK | Confirm button |
| Cancel | Cancel | Cancel button |

## Setting CurrentUICulture

Set the application's culture in `App.xaml.cs`:

```csharp
using Syncfusion.Maui.Picker;
using System.Globalization;
using System.Resources;

namespace DateTimePickerApp
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();
            
            // Set culture to French (France)
            CultureInfo.CurrentUICulture = new CultureInfo("fr-FR");
            
            // Set resource manager path
            // Replace "DateTimePickerApp" with your namespace
            // Replace "Resources.SfDateTimePicker" with your resource file path
            SfPickerResources.ResourceManager = new ResourceManager(
                "DateTimePickerApp.Resources.SfDateTimePicker", 
                Application.Current.GetType().Assembly);
            
            MainPage = new MainPage();
        }
    }
}
```

## Creating Resource Files

### Step 1: Create Resources Folder

1. Right-click on your project in Solution Explorer
2. Select **Add > New Folder**
3. Name it **Resources**

### Step 2: Add Resource File

1. Right-click on the **Resources** folder
2. Select **Add > New Item**
3. Search for **Resource File** template
4. Name the file using pattern: `SfDateTimePicker.<culture-code>.resx`
   - Example: `SfDateTimePicker.fr-FR.resx` (French)
   - Example: `SfDateTimePicker.es-ES.resx` (Spanish)
   - Example: `SfDateTimePicker.de-DE.resx` (German)
5. Click **Add**

### Step 3: Configure Resource File Properties

1. Select the resource file in Solution Explorer
2. In Properties window, set:
   - **Build Action**: `EmbeddedResource`
   - **Custom Tool**: Leave empty or use default

### Step 4: Add Name/Value Pairs

Open the `.resx` file in the designer and add these key-value pairs:

**Example for French (fr-FR):**

| Name | Value (French) |
|------|----------------|
| Day | Jour |
| Month | Mois |
| Year | Année |
| Hour | Heure |
| Minute | Minute |
| Second | Seconde |
| Meridiem | AM/PM |
| OK | OK |
| Cancel | Annuler |

## Supported Cultures

Common culture codes:

| Culture Code | Language/Region |
|--------------|-----------------|
| en-US | English (United States) |
| en-GB | English (United Kingdom) |
| fr-FR | French (France) |
| es-ES | Spanish (Spain) |
| de-DE | German (Germany) |
| it-IT | Italian (Italy) |
| pt-BR | Portuguese (Brazil) |
| zh-CN | Chinese (Simplified, China) |
| ja-JP | Japanese (Japan) |
| ko-KR | Korean (Korea) |
| ar-SA | Arabic (Saudi Arabia) |
| ru-RU | Russian (Russia) |
| hi-IN | Hindi (India) |

## Example Resource Files

### French (fr-FR)

**File**: `Resources/SfDateTimePicker.fr-FR.resx`

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
  <data name="Day" xml:space="preserve">
    <value>Jour</value>
  </data>
  <data name="Month" xml:space="preserve">
    <value>Mois</value>
  </data>
  <data name="Year" xml:space="preserve">
    <value>Année</value>
  </data>
  <data name="Hour" xml:space="preserve">
    <value>Heure</value>
  </data>
  <data name="Minute" xml:space="preserve">
    <value>Minute</value>
  </data>
  <data name="Second" xml:space="preserve">
    <value>Seconde</value>
  </data>
  <data name="Meridiem" xml:space="preserve">
    <value>AM/PM</value>
  </data>
  <data name="OK" xml:space="preserve">
    <value>OK</value>
  </data>
  <data name="Cancel" xml:space="preserve">
    <value>Annuler</value>
  </data>
</root>
```

### Spanish (es-ES)

**File**: `Resources/SfDateTimePicker.es-ES.resx`

| Name | Value (Spanish) |
|------|-----------------|
| Day | Día |
| Month | Mes |
| Year | Año |
| Hour | Hora |
| Minute | Minuto |
| Second | Segundo |
| Meridiem | AM/PM |
| OK | Aceptar |
| Cancel | Cancelar |

### German (de-DE)

**File**: `Resources/SfDateTimePicker.de-DE.resx`

| Name | Value (German) |
|------|----------------|
| Day | Tag |
| Month | Monat |
| Year | Jahr |
| Hour | Stunde |
| Minute | Minute |
| Second | Sekunde |
| Meridiem | AM/PM |
| OK | OK |
| Cancel | Abbrechen |

### Chinese Simplified (zh-CN)

**File**: `Resources/SfDateTimePicker.zh-CN.resx`

| Name | Value (Chinese) |
|------|-----------------|
| Day | 日 |
| Month | 月 |
| Year | 年 |
| Hour | 时 |
| Minute | 分 |
| Second | 秒 |
| Meridiem | 上午/下午 |
| OK | 确定 |
| Cancel | 取消 |

## Dynamic Culture Switching

Allow users to change language at runtime:

```csharp
public void SwitchToFrench()
{
    CultureInfo.CurrentUICulture = new CultureInfo("fr-FR");
    
    // Recreate the page to apply changes
    Application.Current.MainPage = new NavigationPage(new MainPage());
}

public void SwitchToSpanish()
{
    CultureInfo.CurrentUICulture = new CultureInfo("es-ES");
    Application.Current.MainPage = new NavigationPage(new MainPage());
}

public void SwitchToEnglish()
{
    CultureInfo.CurrentUICulture = new CultureInfo("en-US");
    Application.Current.MainPage = new NavigationPage(new MainPage());
}
```

## Complete Localization Example

### App.xaml.cs

```csharp
using Syncfusion.Maui.Picker;
using System.Globalization;
using System.Resources;

namespace MultiLangApp
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();
            
            // Get saved language preference or use device culture
            var savedCulture = Preferences.Get("AppLanguage", 
                CultureInfo.CurrentCulture.Name);
            
            SetCulture(savedCulture);
            
            MainPage = new AppShell();
        }
        
        public static void SetCulture(string cultureCode)
        {
            var culture = new CultureInfo(cultureCode);
            CultureInfo.CurrentUICulture = culture;
            CultureInfo.CurrentCulture = culture;
            
            SfPickerResources.ResourceManager = new ResourceManager(
                "MultiLangApp.Resources.SfDateTimePicker",
                Application.Current.GetType().Assembly);
        }
    }
}
```

### Settings Page with Language Picker

```xaml
<ContentPage Title="Settings">
    <StackLayout Padding="20">
        <Label Text="Language" FontSize="16" FontAttributes="Bold" />
        
        <Picker x:Name="languagePicker"
                Title="Select Language"
                SelectedIndexChanged="OnLanguageChanged">
            <Picker.Items>
                <x:String>English</x:String>
                <x:String>Français</x:String>
                <x:String>Español</x:String>
                <x:String>Deutsch</x:String>
            </Picker.Items>
        </Picker>
    </StackLayout>
</ContentPage>
```

```csharp
private void OnLanguageChanged(object sender, EventArgs e)
{
    var cultureCodes = new[] { "en-US", "fr-FR", "es-ES", "de-DE" };
    var selectedIndex = languagePicker.SelectedIndex;
    
    if (selectedIndex >= 0)
    {
        var cultureCode = cultureCodes[selectedIndex];
        
        // Save preference
        Preferences.Set("AppLanguage", cultureCode);
        
        // Apply culture
        App.SetCulture(cultureCode);
        
        // Restart app to apply changes
        Application.Current.MainPage = new AppShell();
    }
}
```

## Fallback Behavior

If a specific culture resource file is not found, the DateTimePicker falls back to:

1. Parent culture (e.g., `fr-FR` → `fr`)
2. English (default)

**Example**: If `SfDateTimePicker.fr-CA.resx` doesn't exist, it will use `SfDateTimePicker.fr.resx`

## Best Practices

1. **Complete Translations**: Translate all strings for each supported language
2. **Test Thoroughly**: Test with actual native speakers
3. **Resource Naming**: Use clear, consistent naming for resource files
4. **Build Action**: Always set to `EmbeddedResource`
5. **RTL Support**: Consider right-to-left languages (Arabic, Hebrew)
6. **Culture Codes**: Use standard ISO culture codes
7. **Defaults**: Provide English as fallback
8. **Date/Time Formats**: Consider using culture-specific formats automatically
9. **Testing**: Test language switching without restarting app if possible
10. **User Preference**: Save and restore user's language choice

## Troubleshooting

### Issue: Localization Not Working

**Check:**
1. Resource file Build Action is `EmbeddedResource`
2. ResourceManager path matches your actual namespace and file location
3. Culture code in filename matches `CurrentUICulture` setting
4. All required string names are present in resource file

### Issue: Wrong Language Displayed

**Solution**: Verify `CurrentUICulture` is set before initializing MainPage

### Issue: Some Strings Not Translated

**Solution**: Ensure all keys (Day, Month, Year, Hour, Minute, Second, Meridiem, OK, Cancel) are in the resource file

## Date and Time Format Localization

Combine localized labels with culture-specific date/time formats:

```csharp
// Set culture
CultureInfo.CurrentUICulture = new CultureInfo("fr-FR");

// French format automatically used
var picker = new SfDateTimePicker
{
    DateFormat = PickerDateFormat.Default, // Uses culture format
    TimeFormat = PickerTimeFormat.Default  // Uses culture format
};
```

This approach ensures both labels and formats match the user's culture.

## Additional Resources

- **Microsoft Culture Names**: [docs.microsoft.com/globalization](https://docs.microsoft.com/globalization)
- **ISO 639 Language Codes**: Standard 2-letter language codes
- **ISO 3166 Country Codes**: Standard 2-letter country codes
- **Culture Format**: `language-COUNTRY` (e.g., `en-US`, `fr-CA`)
