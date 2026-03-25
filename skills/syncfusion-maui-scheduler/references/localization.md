# Localization in .NET MAUI Scheduler

## Overview

Localization translates the scheduler's built-in strings into different languages to suit specific cultures. The .NET MAUI Scheduler supports localization through resource files (.resx) that define translated strings for built-in UI text.

The scheduler localizes the following built-in strings:

| String | Description |
|--------|-------------|
| Day | Day view name |
| Week | Week view name |
| WorkWeek | WorkWeek view name |
| Month | Month view name |
| TimelineDay | TimelineDay view name |
| TimelineWeek | TimelineWeek view name |
| TimelineWorkWeek | TimelineWorkWeek view name |
| TimelineMonth | TimelineMonth view name |
| Agenda | Agenda view name |
| Today | Today button/label text |
| NoEvents | Text shown when no appointments exist |
| AllDay | All-day indicator text |

## Setting Application Culture

Configure application culture in `App.xaml.cs`:

```csharp
using Syncfusion.Maui.Scheduler;
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
        // ResXPath: Full path of the resx file
        // Example: "MauiSchedulerApp.Resources.SfScheduler"
        SfScheduleResources.ResourceManager = new ResourceManager(
            "MauiSchedulerApp.Resources.SfScheduler", 
            Application.Current.GetType().Assembly
        );
        
        MainPage = new MainPage();
    }
}
```

**Key elements:**
- **CultureInfo.CurrentUICulture**: Sets the application's UI culture
- **SfScheduleResources.ResourceManager**: Links resource files to scheduler
- **ResXPath**: Full namespace path to resource file

## Localization Implementation

### Step 1: Add Default Resource File

1. Create `Resources` folder in your project (if not exists)
2. Add default scheduler resource file `SfScheduler.resx` to `Resources` folder
3. Set **Build Action** to `EmbeddedResource`

**Default resource file structure:**

| Name | Value |
|------|-------|
| Day | Day |
| Week | Week |
| WorkWeek | Work Week |
| Month | Month |
| TimelineDay | Timeline Day |
| TimelineWeek | Timeline Week |
| TimelineWorkWeek | Timeline Work Week |
| TimelineMonth | Timeline Month |
| Agenda | Agenda |
| Today | Today |
| NoEvents | No events |
| AllDay | All day |

### Step 2: Create Culture-Specific Resource Files

1. Right-click on `Resources` folder
2. Select **Add** → **New Item**
3. Choose **Resource File**
4. Name file with culture code: `SfScheduler.<culture-code>.resx`

**Examples:**
- French: `SfScheduler.fr-FR.resx`
- German: `SfScheduler.de-DE.resx`
- Spanish: `SfScheduler.es-ES.resx`
- Japanese: `SfScheduler.ja-JP.resx`
- Chinese (Simplified): `SfScheduler.zh-CN.resx`

### Step 3: Add Translations

Open the culture-specific resource file and add Name/Value pairs:

**Example: French (SfScheduler.fr-FR.resx)**

| Name | Value |
|------|-------|
| Day | Jour |
| Week | Semaine |
| WorkWeek | Semaine de travail |
| Month | Mois |
| TimelineDay | Chronologie Jour |
| TimelineWeek | Chronologie Semaine |
| TimelineWorkWeek | Chronologie Semaine de travail |
| TimelineMonth | Chronologie Mois |
| Agenda | Ordre du jour |
| Today | Aujourd'hui |
| NoEvents | Aucun événement |
| AllDay | Toute la journée |

### Step 4: Configure Resource Properties

Ensure each resource file has:
- **Build Action**: `EmbeddedResource`
- **Custom Tool**: `PublicResXFileCodeGenerator` (default resource file only)

## Complete Examples

### Example 1: French Localization

**Project Structure:**
```
MauiSchedulerApp/
├── App.xaml.cs
├── Resources/
│   ├── SfScheduler.resx (default)
│   └── SfScheduler.fr-FR.resx (French)
```

**App.xaml.cs:**

```csharp
using Syncfusion.Maui.Scheduler;
using System.Globalization;
using System.Resources;

public partial class App : Application
{
    public App()
    {
        InitializeComponent();
        
        // Set French culture
        CultureInfo.CurrentUICulture = new CultureInfo("fr-FR");
        
        // Link resource file
        SfScheduleResources.ResourceManager = new ResourceManager(
            "MauiSchedulerApp.Resources.SfScheduler", 
            Application.Current.GetType().Assembly
        );
        
        MainPage = new MainPage();
    }
}
```

**SfScheduler.fr-FR.resx:**
```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
  <data name="Day" xml:space="preserve">
    <value>Jour</value>
  </data>
  <data name="Week" xml:space="preserve">
    <value>Semaine</value>
  </data>
  <data name="WorkWeek" xml:space="preserve">
    <value>Semaine de travail</value>
  </data>
  <data name="Month" xml:space="preserve">
    <value>Mois</value>
  </data>
  <data name="Today" xml:space="preserve">
    <value>Aujourd'hui</value>
  </data>
  <data name="NoEvents" xml:space="preserve">
    <value>Aucun événement</value>
  </data>
  <data name="AllDay" xml:space="preserve">
    <value>Toute la journée</value>
  </data>
</root>
```

**MainPage.xaml:**

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:scheduler="clr-namespace:Syncfusion.Maui.Scheduler;assembly=Syncfusion.Maui.Scheduler"
             x:Class="MauiSchedulerApp.MainPage">
    <scheduler:SfScheduler x:Name="scheduler" View="Week"/>
</ContentPage>
```

### Example 2: Multi-Language Support

**Project Structure:**
```
MauiSchedulerApp/
├── App.xaml.cs
├── MainPage.xaml
├── Resources/
│   ├── SfScheduler.resx (default - English)
│   ├── SfScheduler.fr-FR.resx (French)
│   ├── SfScheduler.de-DE.resx (German)
│   └── SfScheduler.es-ES.resx (Spanish)
```

**App.xaml.cs (dynamic culture):**

```csharp
using Syncfusion.Maui.Scheduler;
using System.Globalization;
using System.Resources;

public partial class App : Application
{
    public App()
    {
        InitializeComponent();
        
        // Set culture based on device settings
        var deviceCulture = CultureInfo.CurrentCulture;
        CultureInfo.CurrentUICulture = deviceCulture;
        
        // Link resource file
        SfScheduleResources.ResourceManager = new ResourceManager(
            "MauiSchedulerApp.Resources.SfScheduler", 
            Application.Current.GetType().Assembly
        );
        
        MainPage = new MainPage();
    }
}
```

**SfScheduler.de-DE.resx (German):**

| Name | Value |
|------|-------|
| Day | Tag |
| Week | Woche |
| WorkWeek | Arbeitswoche |
| Month | Monat |
| Today | Heute |
| NoEvents | Keine Ereignisse |
| AllDay | Ganztägig |

**SfScheduler.es-ES.resx (Spanish):**

| Name | Value |
|------|-------|
| Day | Día |
| Week | Semana |
| WorkWeek | Semana laboral |
| Month | Mes |
| Today | Hoy |
| NoEvents | Sin eventos |
| AllDay | Todo el día |

### Example 3: User-Selectable Language

**MainPage.xaml:**

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:scheduler="clr-namespace:Syncfusion.Maui.Scheduler;assembly=Syncfusion.Maui.Scheduler"
             x:Class="MauiSchedulerApp.MainPage">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        
        <!-- Language selector -->
        <Picker x:Name="languagePicker" 
                Grid.Row="0"
                Margin="10"
                SelectedIndexChanged="OnLanguageChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type x:String}">
                    <x:String>English</x:String>
                    <x:String>French</x:String>
                    <x:String>German</x:String>
                    <x:String>Spanish</x:String>
                </x:Array>
            </Picker.ItemsSource>
        </Picker>
        
        <!-- Scheduler -->
        <scheduler:SfScheduler x:Name="scheduler" 
                              Grid.Row="1"
                              View="Week"/>
    </Grid>
</ContentPage>
```

**MainPage.xaml.cs:**

```csharp
using Syncfusion.Maui.Scheduler;
using System.Globalization;
using System.Resources;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        languagePicker.SelectedIndex = 0;
    }
    
    private void OnLanguageChanged(object sender, EventArgs e)
    {
        var picker = (Picker)sender;
        string selectedLanguage = picker.SelectedItem.ToString();
        
        string cultureCode = selectedLanguage switch
        {
            "French" => "fr-FR",
            "German" => "de-DE",
            "Spanish" => "es-ES",
            _ => "en-US" // Default to English
        };
        
        // Update culture
        CultureInfo.CurrentUICulture = new CultureInfo(cultureCode);
        
        // Reload resource manager
        SfScheduleResources.ResourceManager = new ResourceManager(
            "MauiSchedulerApp.Resources.SfScheduler",
            Application.Current.GetType().Assembly
        );
        
        // Refresh scheduler to apply new culture
        RefreshScheduler();
    }
    
    private void RefreshScheduler()
    {
        // Save current state
        var currentView = scheduler.View;
        var currentDate = scheduler.DisplayDate;
        var appointments = scheduler.AppointmentsSource;
        
        // Reset scheduler
        scheduler.View = SchedulerView.Day; // Temporary change
        scheduler.View = currentView;       // Restore
        scheduler.DisplayDate = currentDate;
        scheduler.AppointmentsSource = appointments;
    }
}
```

## Culture Codes Reference

Common culture codes for localization:

| Language | Culture Code |
|----------|--------------|
| English (US) | en-US |
| English (UK) | en-GB |
| French | fr-FR |
| German | de-DE |
| Spanish | es-ES |
| Italian | it-IT |
| Portuguese | pt-BR |
| Japanese | ja-JP |
| Chinese (Simplified) | zh-CN |
| Chinese (Traditional) | zh-TW |
| Korean | ko-KR |
| Russian | ru-RU |
| Arabic | ar-SA |
| Hindi | hi-IN |
| Dutch | nl-NL |
| Swedish | sv-SE |

## Troubleshooting

### Localization Not Applied

**Problem:** Scheduler still shows English text  
**Solution:**
- Verify resource file **Build Action** is `EmbeddedResource`
- Check resource file naming: `SfScheduler.<culture-code>.resx`
- Ensure `CultureInfo.CurrentUICulture` is set **before** scheduler initialization
- Verify `SfScheduleResources.ResourceManager` path matches project namespace
- Check that culture code matches device/application culture

### Resource File Not Found

**Problem:** Application throws exception about missing resource file  
**Solution:**
- Verify namespace path in `ResourceManager` constructor
- Check that resource files are in `Resources` folder
- Ensure default `SfScheduler.resx` file exists
- Rebuild project to embed resources

### Partial Translation

**Problem:** Some strings translated, others not  
**Solution:**
- Verify all required Name/Value pairs exist in culture-specific resource file
- Check spelling of Name entries (must match exactly)
- Ensure resource file saved after editing
- Rebuild application

### Culture Not Switching

**Problem:** Language doesn't change at runtime  
**Solution:**
- Reload `SfScheduleResources.ResourceManager` after culture change
- Refresh scheduler by temporarily changing view and restoring
- Consider restarting application for full culture change

### Wrong Culture Applied

**Problem:** Scheduler shows unexpected language  
**Solution:**
- Check `CultureInfo.CurrentUICulture` value
- Verify culture code format (e.g., "fr-FR" not "fr")
- Ensure device culture settings if using `CultureInfo.CurrentCulture`
- Debug resource file loading with fallback mechanisms

---

**Related References:**
- [Getting Started](getting-started.md) - Initial setup
- [Day and Week Views](day-week-views.md) - View configuration
- [Navigation and Restrictions](navigation-restrictions.md) - Date navigation
