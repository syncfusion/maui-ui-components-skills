# Customization

## Table of Contents
- [Text Content](#text-content)
- [Text Style](#text-style)
- [Placeholder](#placeholder)
- [Suggestion Text Color](#suggestion-text-color)
- [Suggestion Popup Background](#suggestion-popup-background)
- [Maximum Input Length](#maximum-input-length)

---

## Text Content

Use the `Text` property to pre-load content or bind the editor to a ViewModel field.

**XAML (static):**
```xml
<smarttexteditor:SfSmartTextEditor
    Text="Thank you for contacting us." />
```

**XAML (data binding):**
```xml
<smarttexteditor:SfSmartTextEditor
    Text="{Binding ReplyText}" />
```

**C#:**
```csharp
var editor = new SfSmartTextEditor
{
    Text = "Thank you for contacting us."
};
```

> When bound to a ViewModel, changes the user makes are reflected back through the binding — no additional event wiring is needed for basic two-way sync.

---

## Text Style

Customize the font and color of the editor's typed text using the `TextStyle` property with a `SmartTextEditorStyle` object.

**XAML:**
```xml
<smarttexteditor:SfSmartTextEditor>
    <smarttexteditor:SfSmartTextEditor.TextStyle>
        <smarttexteditor:SmartTextEditorStyle
            FontSize="16"
            TextColor="Blue" />
    </smarttexteditor:SfSmartTextEditor.TextStyle>
</smarttexteditor:SfSmartTextEditor>
```

**C#:**
```csharp
var editor = new SfSmartTextEditor
{
    TextStyle = new SmartTextEditorStyle
    {
        FontSize = 16,
        TextColor = Colors.Blue
    }
};
```

`SmartTextEditorStyle` supports the same font properties you'd use on a standard MAUI `Label`: `FontSize`, `TextColor`, `FontFamily`, `FontAttributes`.

---

## Placeholder

Add guiding hint text for the empty state. Customize the placeholder color to ensure readability against your background.

**XAML:**
```xml
<smarttexteditor:SfSmartTextEditor
    Placeholder="Type your message..."
    PlaceholderColor="#7E57C2" />
```

**C#:**
```csharp
var editor = new SfSmartTextEditor
{
    Placeholder = "Type your message...",
    PlaceholderColor = Color.FromArgb("#7E57C2")
};
```

> Choose a `PlaceholderColor` with enough contrast against your background, but lighter than your main `TextColor` so users can distinguish placeholder from actual input.

---

## Suggestion Text Color

The `SuggestionTextColor` property controls the color of the predicted text shown inline or in the popup. Use this to make suggestions visually distinct from typed text while staying on-brand.

**XAML:**
```xml
<smarttexteditor:SfSmartTextEditor
    SuggestionTextColor="SkyBlue" />
```

**C#:**
```csharp
var editor = new SfSmartTextEditor
{
    SuggestionTextColor = Colors.SkyBlue
};
```

**Tip:** For dark themes, a lighter muted color (e.g., `#90CAF9`) works well. For light themes, a medium gray or accent color reads clearly without competing with typed text.

---

## Suggestion Popup Background

When `SuggestionDisplayMode` is `Popup`, use `SuggestionPopupBackground` to set the popup's background brush to match your app's design language.

**XAML:**
```xml
<smarttexteditor:SfSmartTextEditor
    SuggestionDisplayMode="Popup"
    SuggestionPopupBackground="#0078D4" />
```

**C#:**
```csharp
var editor = new SfSmartTextEditor
{
    SuggestionDisplayMode = SuggestionDisplayMode.Popup,
    SuggestionPopupBackground = Color.FromArgb("#0078D4")
};
```

> This property has no visible effect in `Inline` mode since there is no popup to color.

---

## Maximum Input Length

`MaxLength` limits the total number of characters a user can type. The editor enforces this limit at input time — characters beyond the limit are not accepted.

**XAML:**
```xml
<smarttexteditor:SfSmartTextEditor
    MaxLength="500" />
```

**C#:**
```csharp
var editor = new SfSmartTextEditor
{
    MaxLength = 500
};
```

**Common use cases:**
- SMS / character-limited messages
- Form fields with database column constraints
- Social-style posts with visible character counters (combine with `TextChanged` event to display remaining count)

---

## Combining Customizations

Here is a fully customized editor example:

```xml
<smarttexteditor:SfSmartTextEditor
    Placeholder="Compose your reply..."
    PlaceholderColor="#9E9E9E"
    SuggestionDisplayMode="Popup"
    SuggestionTextColor="#42A5F5"
    SuggestionPopupBackground="#1E1E2E"
    MaxLength="300"
    UserRole="Customer support agent">
    <smarttexteditor:SfSmartTextEditor.TextStyle>
        <smarttexteditor:SmartTextEditorStyle
            FontSize="14"
            TextColor="#FFFFFF" />
    </smarttexteditor:SfSmartTextEditor.TextStyle>
</smarttexteditor:SfSmartTextEditor>
```
