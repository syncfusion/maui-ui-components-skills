# Localization

## Overview

Localization enables DataForm to display labels, prompts, validation messages, and UI text in multiple languages based on the user's culture.

**Key concepts:**
- **Resource files (.resx):** Store translated strings
- **CurrentUICulture:** Determines active language
- **ResourceType:** Links attributes to resource files
- **Runtime localization:** Dynamic language switching

**When to localize:**
- Multi-language applications
- International user base
- Region-specific compliance
- Accessibility requirements

## Setting Culture

Set application culture in `App.xaml.cs`:

```csharp
using System.Globalization;

public partial class App : Application
{
    public App()
    {
        InitializeComponent();
        
        // Set culture to French
        var culture = new CultureInfo("fr");
        CultureInfo.CurrentUICulture = culture;
        
        MainPage = new AppShell();
    }
}
```

**Common culture codes:**
- `"en"` - English
- `"fr"` - French
- `"es"` - Spanish
- `"de"` - German
- `"zh-CN"` - Chinese (Simplified)
- `"ja"` - Japanese

## Creating Resource Files

### Step 1: Add Resource File

1. Right-click `Resources` folder → Add → New Item
2. Select "Resource File"
3. Name: `DataFormLocalization.<culture>.resx`
   - Example: `DataFormLocalization.fr.resx` (French)
   - Example: `DataFormLocalization.es.resx` (Spanish)
4. Set **Build Action** to `Embedded Resource`

### Step 2: Add Translations

In Resource Designer, add Name/Value pairs:

| Name | Value (English) | Value (French) |
|------|----------------|----------------|
| FirstName | First Name | Prénom |
| LastName | Last Name | Nom de famille |
| Email | Email Address | Adresse e-mail |
| PromptText | Enter your name | Entrez votre nom |
| ErrorMessage | Field is required | Le champ est obligatoire |
| ValidMessage | Valid input | Entrée valide |

## Localizing Display Values

### Using Attributes

```csharp
using System.ComponentModel.DataAnnotations;

public class ContactInfo
{
    [Display(
        Name = "FirstName", // Resource key, not literal text
        Prompt = "PromptText",
        GroupName = "PersonalInfo",
        ResourceType = typeof(DataFormLocalization))] // Link to resource file
    public string FirstName { get; set; }
    
    [Display(
        Name = "Email",
        Prompt = "EmailPrompt",
        ResourceType = typeof(DataFormLocalization))]
    public string Email { get; set; }
}
```

**How it works:**
- `Name = "FirstName"` looks up key "FirstName" in resource file
- `ResourceType = typeof(DataFormLocalization)` specifies which .resx file
- Value displayed based on `CurrentUICulture`

### Using Event

```csharp
dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem != null)
    {
        if (e.DataFormItem.FieldName == "FirstName")
        {
            e.DataFormItem.LabelText = DataFormLocalization.FirstName;
            e.DataFormItem.PlaceholderText = DataFormLocalization.PromptText;
        }
        
        if (e.DataFormItem.GroupName == "Details")
        {
            e.DataFormItem.GroupName = DataFormLocalization.GroupName;
        }
    }
};
```

## Localizing Validation Messages

### Using Attributes

```csharp
using System.ComponentModel.DataAnnotations;

public class RegistrationForm
{
    [Required(
        ErrorMessage = "Required", // Fallback message
        ErrorMessageResourceName = "ErrorMessage", // Resource key
        ErrorMessageResourceType = typeof(DataFormLocalization))] // Resource file
    [Display(
        Name = "FirstName",
        ResourceType = typeof(DataFormLocalization))]
    public string FirstName { get; set; }
    
    [StringLength(50,
        ErrorMessage = "Max 50 characters",
        ErrorMessageResourceName = "MaxLengthError",
        ErrorMessageResourceType = typeof(DataFormLocalization))]
    public string LastName { get; set; }
}
```

### Using Event

```csharp
dataForm.ValidateProperty += (sender, e) =>
{
    if (e.PropertyName == nameof(LocalizationModel.LastName))
    {
        if (e.IsValid)
        {
            e.ValidMessage = DataFormLocalization.ValidMessage;
        }
        else
        {
            e.ErrorMessage = DataFormLocalization.ErrorMessage;
        }
    }
};
```

## Localizing Picker Items

For enum-based pickers, localize enum value display names:

```csharp
public enum Gender
{
    Male,
    Female,
    Other
}

public class UserProfile
{
    [Display(ResourceType = typeof(DataFormLocalization))]
    public Gender Gender { get; set; }
}

dataForm.RegisterEditor("Gender", DataFormEditorType.RadioGroup);
```

**Resource file entries:**
| Name | Value (English) | Value (French) |
|------|----------------|----------------|
| Male | Male | Homme |
| Female | Female | Femme |
| Other | Other | Autre |

## Runtime Language Switching

```csharp
public class LocalizationService
{
    public static void SetCulture(string cultureCode)
    {
        var culture = new CultureInfo(cultureCode);
        CultureInfo.CurrentUICulture = culture;
        
        // Reload DataForm to apply new culture
        // Call from page/view model:
        // dataForm.Reload();
    }
}

// In UI
private void OnLanguageChanged(object sender, EventArgs e)
{
    string selectedLanguage = languagePicker.SelectedItem as string;
    
    switch (selectedLanguage)
    {
        case "English":
            LocalizationService.SetCulture("en");
            break;
        case "French":
            LocalizationService.SetCulture("fr");
            break;
        case "Spanish":
            LocalizationService.SetCulture("es");
            break;
    }
    
    // Refresh DataForm
    dataForm.Reload();
}
```

## Example: Complete Localized Form

**Model:**
```csharp
public class RegistrationForm
{
    [Display(
        Name = "FirstName",
        Prompt = "FirstNamePrompt",
        GroupName = "PersonalInfo",
        ResourceType = typeof(DataFormLocalization))]
    [Required(
        ErrorMessageResourceName = "FirstNameRequired",
        ErrorMessageResourceType = typeof(DataFormLocalization))]
    [DataFormDisplayOptions(ValidMessage = "FirstNameValid")]
    public string FirstName { get; set; }
    
    [Display(
        Name = "Email",
        Prompt = "EmailPrompt",
        ResourceType = typeof(DataFormLocalization))]
    [EmailAddress(
        ErrorMessageResourceName = "InvalidEmail",
        ErrorMessageResourceType = typeof(DataFormLocalization))]
    public string Email { get; set; }
}
```

**Resource File (DataFormLocalization.resx - English):**
| Name | Value |
|------|-------|
| FirstName | First Name |
| FirstNamePrompt | Enter your first name |
| FirstNameRequired | First name is required |
| FirstNameValid | Valid name |
| Email | Email Address |
| EmailPrompt | your@email.com |
| InvalidEmail | Invalid email format |
| PersonalInfo | Personal Information |

**Resource File (DataFormLocalization.fr.resx - French):**
| Name | Value |
|------|-------|
| FirstName | Prénom |
| FirstNamePrompt | Entrez votre prénom |
| FirstNameRequired | Le prénom est obligatoire |
| FirstNameValid | Nom valide |
| Email | Adresse e-mail |
| EmailPrompt | votre@email.com |
| InvalidEmail | Format d'e-mail invalide |
| PersonalInfo | Informations personnelles |

## Troubleshooting

### Localization Not Working

**Solutions:**
```csharp
// 1. Verify resource file Build Action
// Right-click .resx file → Properties → Build Action: Embedded Resource

// 2. Check culture is set before DataForm loads
CultureInfo.CurrentUICulture = new CultureInfo("fr");
dataForm.DataObject = new MyModel(); // Set after culture

// 3. Ensure ResourceType is correct
[Display(
    Name = "FirstName",
    ResourceType = typeof(MyProject.Resources.DataFormLocalization))] // Full namespace

// 4. Verify resource key exists and matches
// Check .resx file has entry with exact Name value
```

### Resource Keys Not Found

**Solutions:**
```csharp
// Keys are case-sensitive
[Display(Name = "FirstName")] // ✅
[Display(Name = "firstname")] // ❌ Won't match "FirstName" key

// Ensure no typos
[Display(Name = "FistName")] // ❌ Typo
[Display(Name = "FirstName")] // ✅ Correct
```

### Localization Not Updating After Culture Change

**Solutions:**
```csharp
// Must reload DataForm after changing culture
CultureInfo.CurrentUICulture = new CultureInfo("es");
dataForm.Reload(); // Required to apply new culture
```

### Resource File Not Embedded

**Solutions:**
```csharp
// 1. Right-click .resx file in Solution Explorer
// 2. Properties → Build Action → Set to "Embedded Resource"
// 3. Rebuild project
```

---

**Related Topics:**
- [getting-started.md](getting-started.md) - Basic DataForm setup
- [data-annotations.md](data-annotations.md) - Display and validation attributes
- [validation.md](validation.md) - Validation message customization
