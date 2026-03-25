# Accessibility and Localization in .NET MAUI DatePicker

The .NET MAUI DatePicker provides comprehensive accessibility and localization support to ensure the control is usable by all users across different regions and languages.

## Table of Contents
- [Accessibility Features](#accessibility-features)
- [Keyboard Navigation](#keyboard-navigation)
- [Localization](#localization)

## Accessibility Features

The DatePicker control supports accessibility for screen readers and assistive technologies through interaction with headers, column headers, footers, and picker items.

### Header Layout Accessibility

The DatePicker header text can be localized and is accessible to screen readers.

```xml
<picker:SfDatePicker x:Name="datePicker">
    <picker:SfDatePicker.HeaderView>
        <picker:PickerHeaderView Text="Select Date" Height="40" />
    </picker:SfDatePicker.HeaderView>
</picker:SfDatePicker>
```

Screen readers will announce: "Select Date"

### Column Header Layout Accessibility

The DatePicker column headers are accessible and can be customized with localized text.

**Default Values:**
- **DayHeaderText:** "Day"
- **MonthHeaderText:** "Month"
- **YearHeaderText:** "Year"

```xml
<picker:SfDatePicker x:Name="datePicker">
    <picker:SfDatePicker.ColumnHeaderView>
        <picker:DatePickerColumnHeaderView DayHeaderText="Day"
                                           MonthHeaderText="Month"
                                           YearHeaderText="Year" />
    </picker:SfDatePicker.ColumnHeaderView>
</picker:SfDatePicker>
```

Screen readers will announce column headers as users navigate between columns.

### Footer Layout Accessibility

The footer validation buttons (OK and Cancel) are accessible with localized text.

**Default Values:**
- **OkButtonText:** "OK"
- **CancelButtonText:** "Cancel"

```xml
<picker:SfDatePicker x:Name="datePicker">
    <picker:SfDatePicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True"
                                 OkButtonText="OK"
                                 CancelButtonText="Cancel"
                                 Height="40" />
    </picker:SfDatePicker.FooterView>
</picker:SfDatePicker>
```

Screen readers will announce: "OK button" and "Cancel button"

### Picker Items Accessibility

The picker items are accessible and announced based on the date format.

**Format Examples:**
| Format | Display | Screen Reader Announcement |
|--------|---------|---------------------------|
| d, M | 1 | "One" |
| dd, MM | 01 | "Zero one" |
| MMM | Jan | "January" (abbreviated) |
| MMMM | January | "January" |
| yyyy | 2023 | "Two thousand twenty-three" |

```xml
<picker:SfDatePicker x:Name="datePicker"
                     Format="MMM_dd_yyyy">
</picker:SfDatePicker>
```

As users scroll through dates, screen readers announce each date component.

## Keyboard Navigation

The DatePicker supports full keyboard navigation for accessibility.

### Keyboard Shortcuts

| Key | Action |
|-----|--------|
| **Tab** | Focus the picker |
| **Enter** | Opens the selected picker |
| **DownArrow** | Selects an item from the currently expanded list by moving downwards |
| **UpArrow** | Selects an item from the currently expanded list by moving upwards |
| **RightArrow / Tab** | Navigates through the selected item in the right direction (Day → Month → Year) |
| **LeftArrow / Shift+Tab** | Navigates through the selected item in the left direction (Year → Month → Day) |
| **Esc / Enter** | Exit and commit selection |

### Keyboard Navigation Example

```xml
<StackLayout>
    <Label Text="Use Tab to focus, Arrow keys to navigate, Enter to select"/>
    
    <picker:SfDatePicker x:Name="datePicker"
                         Mode="Dialog">
        <picker:SfDatePicker.HeaderView>
            <picker:PickerHeaderView Text="Select Date (Keyboard Accessible)" Height="40" />
        </picker:SfDatePicker.HeaderView>
        <picker:SfDatePicker.FooterView>
            <picker:PickerFooterView ShowOkButton="True" Height="40" />
        </picker:SfDatePicker.FooterView>
    </picker:SfDatePicker>
    
    <Button Text="Open Picker (Enter)" />
</StackLayout>
```

### Navigation Flow

1. **Tab** to focus on the DatePicker button or control
2. **Enter** to open the picker dialog
3. **Left/Right Arrow** or **Tab/Shift+Tab** to move between Day, Month, Year columns
4. **Up/Down Arrow** to scroll through values in the current column
5. **Enter** or **Esc** to close and commit/cancel the selection

## Localization

Localization allows you to translate the DatePicker UI elements into different languages for specific cultures.

### Localizable Strings

The DatePicker supports localization for:
- **Day** - Day column header text
- **Month** - Month column header text
- **Year** - Year column header text
- **OK** - OK button text
- **Cancel** - Cancel button text

### Setting CurrentUICulture

Change the application culture by setting `CurrentUICulture` in the `App.xaml.cs` file.

```csharp
using Syncfusion.Maui.Picker;
using System.Globalization;
using System.Resources;

namespace DatePickerApp
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();
            
            // Set culture to French (France)
            CultureInfo.CurrentUICulture = new CultureInfo("fr-FR");
            
            // Set resource manager
            // ResXPath => Full path of the resx file
            // Example: "DatePickerApp.Resources.SfDatePicker"
            SfPickerResources.ResourceManager = new ResourceManager(
                "ResxPath", 
                Application.Current.GetType().Assembly);
            
            MainPage = new MainPage();
        }
    }
}
```

### Creating Resource Files

Follow these steps to localize the DatePicker:

#### Step 1: Create Resources Folder

Create a new folder named `Resources` in your application project.

#### Step 2: Add Resource File

1. Right-click on the `Resources` folder
2. Select **Add > New Item**
3. In the Add New Item wizard, select **Resource File**
4. Name the file as `SfDatePicker.<culture-name>.resx`
   - Example: `SfDatePicker.fr-FR.resx` for French
   - Example: `SfDatePicker.es-ES.resx` for Spanish
   - Example: `SfDatePicker.de-DE.resx` for German

The culture name indicates the language and country (format: `language-COUNTRY`).

#### Step 3: Configure Resource File

Set the resource file's **Build Action** to **EmbeddedResource**.

#### Step 4: Add Name/Value Pairs

Add the localizable strings in the Resource Designer:

**For French (fr-FR):**
| Name | Value |
|------|-------|
| Day | Jour |
| Month | Mois |
| Year | Année |
| OK | D'accord |
| Cancel | Annuler |

**For Spanish (es-ES):**
| Name | Value |
|------|-------|
| Day | Día |
| Month | Mes |
| Year | Año |
| OK | Aceptar |
| Cancel | Cancelar |

**For German (de-DE):**
| Name | Value |
|------|-------|
| Day | Tag |
| Month | Monat |
| Year | Jahr |
| OK | OK |
| Cancel | Abbrechen |

### Example Resource File Structure

```
MyApp/
├── Resources/
│   ├── SfDatePicker.resx (default/English)
│   ├── SfDatePicker.fr-FR.resx (French)
│   ├── SfDatePicker.es-ES.resx (Spanish)
│   ├── SfDatePicker.de-DE.resx (German)
│   └── SfDatePicker.ja-JP.resx (Japanese)
```

### Dynamic Culture Switching

Allow users to switch languages at runtime:

```csharp
public class CultureManager
{
    public static void SetCulture(string cultureName)
    {
        var culture = new CultureInfo(cultureName);
        CultureInfo.CurrentUICulture = culture;
        CultureInfo.CurrentCulture = culture;
        
        // Reload the current page to apply changes
        Application.Current.MainPage = new NavigationPage(new MainPage());
    }
}
```

```xml
<StackLayout Padding="20" Spacing="10">
    <Label Text="Select Language:" FontSize="18"/>
    
    <Picker x:Name="languagePicker" 
            SelectedIndexChanged="OnLanguageChanged">
        <Picker.Items>
            <x:String>English (en-US)</x:String>
            <x:String>French (fr-FR)</x:String>
            <x:String>Spanish (es-ES)</x:String>
            <x:String>German (de-DE)</x:String>
        </Picker.Items>
    </Picker>
    
    <picker:SfDatePicker x:Name="datePicker">
        <picker:SfDatePicker.HeaderView>
            <picker:PickerHeaderView Height="40" />
        </picker:SfDatePicker.HeaderView>
        <picker:SfDatePicker.FooterView>
            <picker:PickerFooterView ShowOkButton="True" Height="40" />
        </picker:SfDatePicker.FooterView>
    </picker:SfDatePicker>
</StackLayout>
```

```csharp
private void OnLanguageChanged(object sender, EventArgs e)
{
    var selectedLanguage = languagePicker.SelectedItem as string;
    
    string cultureName = selectedLanguage switch
    {
        "English (en-US)" => "en-US",
        "French (fr-FR)" => "fr-FR",
        "Spanish (es-ES)" => "es-ES",
        "German (de-DE)" => "de-DE",
        _ => "en-US"
    };
    
    CultureManager.SetCulture(cultureName);
}
```

### Month and Weekday Localization

Month names and weekday names are automatically localized based on the current culture:

```csharp
// Set culture to Spanish
CultureInfo.CurrentUICulture = new CultureInfo("es-ES");

// Month names will display as: enero, febrero, marzo, abril, etc.
// Weekday names will display as: lunes, martes, miércoles, etc.
```

### Date Format Localization

Use the `Default` format to automatically use culture-specific date formats:

```xml
<picker:SfDatePicker x:Name="datePicker"
                     Format="Default">
</picker:SfDatePicker>
```

**Examples:**
- **en-US:** 9/15/2023 (MM/dd/yyyy)
- **fr-FR:** 15/09/2023 (dd/MM/yyyy)
- **de-DE:** 15.09.2023 (dd.MM.yyyy)
- **ja-JP:** 2023/09/15 (yyyy/MM/dd)

## Complete Localization Example

### App.xaml.cs

```csharp
using Syncfusion.Maui.Picker;
using System.Globalization;
using System.Resources;

namespace LocalizedDatePickerApp
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();
            
            // Set default culture
            SetCulture("fr-FR");
            
            MainPage = new AppShell();
        }
        
        public static void SetCulture(string cultureName)
        {
            var culture = new CultureInfo(cultureName);
            CultureInfo.CurrentUICulture = culture;
            CultureInfo.CurrentCulture = culture;
            
            // Set resource manager for Syncfusion controls
            SfPickerResources.ResourceManager = new ResourceManager(
                "LocalizedDatePickerApp.Resources.SfDatePicker",
                typeof(App).Assembly);
        }
    }
}
```

### MainPage.xaml

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:picker="clr-namespace:Syncfusion.Maui.Picker;assembly=Syncfusion.Maui.Picker"
             x:Class="LocalizedDatePickerApp.MainPage">
    
    <StackLayout Padding="20" Spacing="20">
        <Label Text="Multi-Language Date Picker" 
               FontSize="24" 
               FontAttributes="Bold"/>
        
        <Label Text="Select Language:"/>
        <Picker x:Name="languagePicker" 
                SelectedIndex="0"
                SelectedIndexChanged="OnLanguageChanged">
            <Picker.Items>
                <x:String>English</x:String>
                <x:String>French</x:String>
                <x:String>Spanish</x:String>
                <x:String>German</x:String>
                <x:String>Japanese</x:String>
            </Picker.Items>
        </Picker>
        
        <picker:SfDatePicker x:Name="datePicker"
                             Format="Default">
            <picker:SfDatePicker.HeaderView>
                <picker:PickerHeaderView Height="40" />
            </picker:SfDatePicker.HeaderView>
            <picker:SfDatePicker.FooterView>
                <picker:PickerFooterView ShowOkButton="True" Height="40" />
            </picker:SfDatePicker.FooterView>
        </picker:SfDatePicker>
    </StackLayout>
    
</ContentPage>
```

### MainPage.xaml.cs

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }
    
    private void OnLanguageChanged(object sender, EventArgs e)
    {
        string cultureName = languagePicker.SelectedIndex switch
        {
            0 => "en-US",
            1 => "fr-FR",
            2 => "es-ES",
            3 => "de-DE",
            4 => "ja-JP",
            _ => "en-US"
        };
        
        App.SetCulture(cultureName);
        
        // Reload the page to apply localization
        Application.Current.MainPage = new NavigationPage(new MainPage());
    }
}
```

## Best Practices

1. **Provide Resource Files for Target Markets** - Create resource files for all languages you plan to support

2. **Test with Different Cultures** - Test your application with different culture settings to ensure proper localization

3. **Use Default Format** - Use `Format="Default"` to automatically adapt to regional date formats

4. **Accessibility Labels** - Provide clear, descriptive text for headers and buttons

5. **Keyboard Support** - Ensure all functionality is accessible via keyboard

6. **Screen Reader Testing** - Test with actual screen readers to verify accessibility

7. **Right-to-Left Languages** - Test with RTL languages (Arabic, Hebrew) using the RTL support features

## Troubleshooting

### Issue: Localization not working
**Solution:** Verify that:
- Resource files are set to **Build Action: EmbeddedResource**
- Resource file names follow the pattern `SfDatePicker.<culture>.resx`
- `SfPickerResources.ResourceManager` is set correctly in App.xaml.cs

### Issue: Month names not localized
**Solution:** Month names are localized automatically by the system. Ensure `CurrentUICulture` is set correctly.

### Issue: Keyboard navigation not working
**Solution:** Ensure the DatePicker is focusable and not disabled. Check that no other control is capturing keyboard input.

## Related Topics

- **Customization** - Customize localized text appearance
- **Formatting** - Use culture-specific date formats
- **Events** - Handle events for accessibility features
