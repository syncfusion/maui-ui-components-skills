# Disclaimer Message in SfAIAssistView

`SfAIAssistView` can show a short disclaimer message below the request editor. The disclaimer is useful for guidance text, caveats, or short usage notes.

## Table of Contents
- [Disclaimer Text](#disclaimer-text)
- [Visibility and Layout](#visibility-and-layout)
- [Scenarios](#scenarios)

---

## Disclaimer Text

Use `DisclaimerText` to define the message shown below the editor.

```xaml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           DisclaimerText="AI responses may be inaccurate. Please verify important information." />
```

```csharp
sfAIAssistView.DisclaimerText = "AI responses may be inaccurate. Please verify important information.";
```

The default value is an empty string.

---

## Visibility and Layout

The disclaimer area is displayed only when `DisclaimerText` contains visible content. When the text is null, empty, or whitespace, the disclaimer area is hidden.

When visible, the control updates the editor footer layout so the disclaimer does not overlap the input area.

## Scenarios

- When `DisclaimerText` has visible content, the disclaimer appears below the editor.
- When `DisclaimerText` is empty or whitespace, the disclaimer area is hidden.
- When the disclaimer becomes visible, the footer spacing adjusts to keep the input area readable.

---

## Technical Notes

- **Visibility Logic**: `DisclaimerView` hides itself when the supplied text is null, empty, or whitespace.
- **Layout Behavior**: `AssistViewChat` updates the input footer margin based on whether `DisclaimerText` has visible content.
- **Text Layout**: The disclaimer label is centered, word-wrapped, and limited to two lines.

### Bindable Properties

- `DisclaimerText` (`string`, default: empty string) — Disclaimer message shown below the editor
