# Accessibility and Localization

## Table of Contents
- [Accessibility Overview](#accessibility-overview)
- [Element Accessibility Labels](#element-accessibility-labels)
- [Localization Overview](#localization-overview)
- [Setting the Application Culture](#setting-the-application-culture)
- [Creating Localized Resource Files](#creating-localized-resource-files)
- [Key Properties](#key-properties)

---

## Accessibility Overview

`SfChat` provides built-in accessibility support so screen readers and assistive technologies can identify interactive elements. Core interactive elements (send button, attachment button, load more) have default accessibility descriptors that assistive technologies read aloud.

---

## Element Accessibility Labels

The following table lists the default accessibility descriptions for each element:

| Element | AutomationId / Label | Screen Reader Text |
|---|---|---|
| `AvatarView` | Avatar | "Avatar" |
| `loadMoreView` | Load more button | "Load more button, Double tap to load more messages" |
| `AttachmentIconView` | Attachment button | "Attachment Button, Double tap to activate" |
| `SendIconView` | Send button | "Send Button, Double tap to send message" |
| `Calendar View` | Calendar message | "Calendar" |
| `Card View` | Card message | "Cards" |
| `Suggestions View` | Suggestions area | "Suggestions" |

These labels are applied automatically — no extra code is required for default accessibility support.

---

## Localization Overview

`SfChat` supports localization by providing `.resx` resource files. You can translate any built-in UI string by supplying a culture-specific resource file and setting `CurrentUICulture` at app startup.

**Required NuGet:** `Syncfusion.Maui.Chat`

---

## Setting the Application Culture

Set `CultureInfo.CurrentUICulture` and register the custom `ResourceManager` in `App.xaml.cs` **before** `MainPage` is assigned.

```csharp
// App.xaml.cs
using Syncfusion.Maui.Chat;
using System.Globalization;
using System.Resources;

public partial class App : Application
{
    public App()
    {
        InitializeComponent();

        // Set French culture
        CultureInfo.CurrentUICulture = new CultureInfo("fr-FR");

        // Point SfChat to the embedded resource file
        SfChatResources.ResourceManager = new ResourceManager(
            "MauiChat.Resources.SfChat",          // Base name: <namespace>.Resources.SfChat
            Application.Current!.GetType().Assembly);

        MainPage = new MainPage();
    }
}
```

> **Note:** The namespace segment (`MauiChat.Resources.SfChat`) must match your project namespace + `Resources` folder + resource file base name.

---

## Creating Localized Resource Files

### Step-by-step

1. Create a folder named `Resources` at the project root.
2. Add a new **Resource File** named `SfChat.<culture>.resx`  
   - Example: `SfChat.fr-FR.resx` for French (France)
   - The culture code suffix (`fr-FR`, `de-DE`, `ja-JP`, etc.) determines which culture it applies to.
3. Set **Build Action** to `EmbeddedResource`.
4. Add Name/Value pairs for each string you want to translate.

### Example: French resource file

| Name | Value (English default) | Value (French) |
|---|---|---|
| `LoadMore` | Load more | Charger plus |
| `TypeAMessage` | Type a message... | Tapez un message... |

> The exact resource key names correspond to the strings used internally by `SfChat`. Refer to the Syncfusion GitHub samples for the complete list of localizable keys.

### File naming convention

```
Resources/
├── SfChat.resx           ← Default/fallback strings
├── SfChat.fr-FR.resx     ← French (France)
├── SfChat.de-DE.resx     ← German (Germany)
└── SfChat.ja-JP.resx     ← Japanese (Japan)
```

### Runtime behavior

- If `CurrentUICulture` matches a `.resx` file, those strings are used.
- If no matching `.resx` exists, the default `SfChat.resx` is used as fallback.
- Culture must be set **before** the chat page is loaded.

---

## Key Properties

| Property / API | Type | Description |
|---|---|---|
| `CultureInfo.CurrentUICulture` | `CultureInfo` | Sets the active UI culture for the application |
| `SfChatResources.ResourceManager` | `ResourceManager` | Points SfChat to your custom `.resx` resource file |
| Build Action: `EmbeddedResource` | — | Required for `.resx` files to be embedded in the assembly |
