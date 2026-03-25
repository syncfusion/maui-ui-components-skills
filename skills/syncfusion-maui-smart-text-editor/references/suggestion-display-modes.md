# Suggestion Display Modes

`SfSmartTextEditor` supports two modes for showing AI completions as you type: **Inline** and **Popup**. Choose based on the platform and the typing experience you want to deliver.

---

## Overview

| Mode | Appearance | Best For |
|------|-----------|----------|
| `Inline` | Suggestion appears in place after the caret, matching editor text style | Desktop (Windows, Mac) — uninterrupted flow |
| `Popup` | Compact overlay/bubble near the caret | Touch devices (Android, iOS) — tap to accept |

**Platform defaults (applied automatically):**
- **Windows / Mac Catalyst** → `Inline`
- **Android / iOS** → `Popup`

Override the default by setting `SuggestionDisplayMode` explicitly.

---

## Inline Mode

Inline mode renders the predicted text directly in the editor, continuing from where the caret is. It feels like a natural extension of typing — ideal for desktop environments where the keyboard is primary.

**XAML:**
```xml
<ContentPage
    xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:smarttexteditor="clr-namespace:Syncfusion.Maui.SmartComponents;assembly=Syncfusion.Maui.SmartComponents">

    <smarttexteditor:SfSmartTextEditor
        Placeholder="Start typing..."
        UserRole="Email author responding to customer inquiries"
        SuggestionDisplayMode="Inline" />
</ContentPage>
```

**C#:**
```csharp
using Syncfusion.Maui.SmartComponents;

var editor = new SfSmartTextEditor
{
    Placeholder = "Start typing...",
    UserRole = "Email author responding to customer inquiries",
    SuggestionDisplayMode = SuggestionDisplayMode.Inline
};
```

**Accepting inline suggestions:**
- Press **Tab** or **Right Arrow** to accept
- Continue typing to dismiss and get a new suggestion

> Tab key acceptance is **not supported on Android and iOS** even if Inline mode is forced.

---

## Popup Mode

Popup mode shows a small overlay near the caret. The user can tap/click the popup or press a key to accept. This is the preferred mode for touch-based devices where tapping a floating suggestion feels natural.

**XAML:**
```xml
<ContentPage
    xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:smarttexteditor="clr-namespace:Syncfusion.Maui.SmartComponents;assembly=Syncfusion.Maui.SmartComponents">

    <smarttexteditor:SfSmartTextEditor
        Placeholder="Start typing..."
        UserRole="Email author responding to customer inquiries"
        SuggestionDisplayMode="Popup" />
</ContentPage>
```

**C#:**
```csharp
var editor = new SfSmartTextEditor
{
    Placeholder = "Start typing...",
    UserRole = "Email author responding to customer inquiries",
    SuggestionDisplayMode = SuggestionDisplayMode.Popup
};
```

**Accepting popup suggestions:**
- **Tap or click** the popup bubble
- Press **Right Arrow** key

---

## Platform Limitations

| Platform | Inline | Popup | Tab Key Acceptance |
|----------|--------|-------|-------------------|
| Windows | ✅ (default) | ✅ | ✅ |
| Mac Catalyst | ✅ (default) | ✅ | ✅ |
| Android | ✅ (manual) | ✅ (default) | ❌ Not supported |
| iOS | ✅ (manual) | ✅ (default) | ❌ Not supported |

---

## Choosing Between Modes

- **Building for desktop only?** Leave the default — Inline is applied automatically.
- **Building for mobile only?** Leave the default — Popup is applied automatically.
- **Cross-platform app?** Let the platform default handle it, or override per-platform using `OnPlatform`.

**Using OnPlatform to override per target:**
```xml
<smarttexteditor:SfSmartTextEditor
    Placeholder="Type here...">
    <smarttexteditor:SfSmartTextEditor.SuggestionDisplayMode>
        <OnPlatform x:TypeArguments="smarttexteditor:SuggestionDisplayMode">
            <On Platform="WinUI, MacCatalyst" Value="Inline" />
            <On Platform="Android, iOS" Value="Popup" />
        </OnPlatform>
    </smarttexteditor:SfSmartTextEditor.SuggestionDisplayMode>
</smarttexteditor:SfSmartTextEditor>
```

---

## Customizing Popup Appearance

When using Popup mode, you can style the popup background to match your app theme. See `customization.md` for `SuggestionPopupBackground` and `SuggestionTextColor`.
