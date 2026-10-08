# Text Transformation

## Table of Contents
- [Overview](#overview)
- [TextTransform Toolbar Item](#texttransform-toolbar-item)
- [Adding Text Transformation to the Toolbar](#adding-text-transformation-to-the-toolbar)
- [Selection-Based Transformation](#selection-based-transformation)
- [Programmatic Uppercase and Lowercase Methods](#programmatic-uppercase-and-lowercase-methods)
- [Common Transformation Scenarios](#common-transformation-scenarios)
- [Combining with Other Formatting](#combining-with-other-formatting)
- [Grouped Toolbar Integration](#grouped-toolbar-integration)
- [Use Cases and Patterns](#use-cases-and-patterns)
- [Troubleshooting](#troubleshooting)
- [Best Practices](#best-practices)
- [Next Steps](#next-steps)

## Overview

The Rich Text Editor includes a **TextTransform** toolbar item that lets users change the case of the selected text directly from the toolbar. Text transformation is the fastest way for users to enforce consistent capitalization in messages, blog posts, comments, and any other rich text content without having to re-type or paste through an external utility.

The TextTransform action works on the user's current text selection, so it integrates naturally with the editor's selection-based formatting model and complements character-level formatting like bold, italic, and underline.

> N> All examples in this reference read and write content through the public `Value` property together with `TValue="HTML"`. The legacy `Text` and `HtmlText` properties are internal and not part of the public surface; use `Value` for both the seed content and any post-transformation verification.

## TextTransform Toolbar Item

`TextTransform` is exposed as an entry in the `RichTextToolbarOptions` enumeration. Like every other toolbar option, it is added to the `ToolbarItems` collection to make it available in the editor toolbar.

### Available Toolbar Options Related to Character Formatting

The following character-level options are available in `RichTextToolbarOptions` and can be combined with `TextTransform` in the same `ToolbarItems` collection:

- `Bold`, `Italic`, `Underline`, `Strikethrough`
- `SubScript`, `SuperScript`
- **`TextTransform`** — applies a text-case transformation to the selection

```csharp
public enum RichTextToolbarOptions
{
    Bold,
    Italic,
    Underline,
    Strikethrough,
    SubScript,
    SuperScript,
    TextTransform,
    // ...other options
}
```

> N> The `TextTransform` toolbar item groups the available case-change actions under a single toolbar entry. End users choose the specific transformation (such as UPPERCASE or lowercase) from the menu that opens when the item is tapped.

## Adding Text Transformation to the Toolbar

Because populating the `ToolbarItems` collection **replaces** the default toolbar items, you must explicitly add `TextTransform` whenever you want it to appear.

### XAML

```xaml
<rte:SfRichTextEditor ShowToolbar="True">
    <rte:SfRichTextEditor.ToolbarItems>
        <rte:RichTextToolbarItem Type="Bold" />
        <rte:RichTextToolbarItem Type="Italic" />
        <rte:RichTextToolbarItem Type="Underline" />
        <rte:RichTextToolbarItem Type="Separator" />
        <rte:RichTextToolbarItem Type="TextTransform" />
    </rte:SfRichTextEditor.ToolbarItems>
</rte:SfRichTextEditor>
```

### C#

```csharp
using Syncfusion.Maui.RichTextEditor;

SfRichTextEditor richTextEditor = new SfRichTextEditor
{
    ShowToolbar = true
};

richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Bold });
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Italic });
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Underline });
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Separator });
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.TextTransform });
```

## Selection-Based Transformation

The TextTransform toolbar action operates on the user's current text selection inside the editor. This means:

- The user selects the text they want to convert.
- They tap the `TextTransform` toolbar item.
- They pick the desired transformation (for example, UPPERCASE) from the menu that opens.
- The selection is replaced in-place with the transformed text, preserving the rest of the document.

This mirrors how the rest of the editor's character formatting works, so users do not need a separate mental model to apply case changes.

### Transformation Workflow

1. **Focus the editor** and place the caret in a paragraph.
2. **Select the text** to transform (drag the selection handles or use Shift+Arrow on desktop).
3. **Tap the TextTransform toolbar item.**
4. **Choose the transformation** from the available options.
5. The selection is replaced and the editor's `Value` property (with `TValue="HTML"`) reflects the new content.

> N> The TextTransform action requires an active text selection. If no text is selected when the item is tapped, the editor does not change any content. Guide users to select the target text first.

## Programmatic Uppercase and Lowercase Methods

In addition to the toolbar-based `TextTransform` action, the Rich Text Editor exposes two instance methods that let you trigger the same transformations from code:

- `Uppercase()` — converts the currently selected text to UPPERCASE.
- `Lowercase()` — converts the currently selected text to lowercase.

Both methods follow the editor\'s selection-based model: they operate on the current text selection inside the editor and leave the rest of the document untouched. They are the programmatic counterparts of the UPPERCASE and lowercase options exposed under the `TextTransform` toolbar item.

### Method Signatures

```csharp
public void Uppercase();
public void Lowercase();
```

Neither method takes parameters and neither returns a value. The result is reflected in the editor immediately and in the `Value` property (with `TValue="HTML"`) after the transformation runs.

### Basic Usage

```csharp
using Syncfusion.Maui.RichTextEditor;

SfRichTextEditor richTextEditor = new SfRichTextEditor
{
    TValue = RichTextEditorValueType.HTML,
    ShowToolbar = true
};

richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Bold });
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.TextTransform });

// Seed the editor with content
richTextEditor.Value = "<p>hello world</p>";

// (User selects "hello" in the editor)

// Convert the selected text programmatically to UPPERCASE
richTextEditor.Uppercase();

// (User changes the selection to "WORLD")

// Convert the selected text programmatically to lowercase
richTextEditor.Lowercase();
```

> N> The methods require the editor to be focused and to have an active text selection. If no text is selected, the editor does not change any content (the call is a no-op). Always ensure the selection is set before invoking these methods.

### Using Uppercase and Lowercase from Button Click Handlers

The most common use case is to wire the methods to a custom button or context-menu action:

```xaml
<StackLayout>
    <rte:SfRichTextEditor x:Name="richTextEditor" ShowToolbar="True" />

    <StackLayout Orientation="Horizontal" Spacing="8" Padding="8">
        <Button Text="UPPERCASE" Clicked="OnUppercaseClicked" />
        <Button Text="lowercase" Clicked="OnLowercaseClicked" />
    </StackLayout>
</StackLayout>
```

```csharp
private void OnUppercaseClicked(object sender, EventArgs e)
{
    richTextEditor.Uppercase();
}

private void OnLowercaseClicked(object sender, EventArgs e)
{
    richTextEditor.Lowercase();
}
```

This is useful when you want to keep the editor\'s toolbar minimal and expose the transformations in your own UI, or when the user takes an action in a side panel (for example, normalizing a heading) that you want to apply through the editor\'s selection model.

### Pairing with the Value Property

After either method runs, the `Value` property reflects the updated content. Because `Value` is typed as `object`, cast it to `string` when reading the HTML content:

```csharp
richTextEditor.Value = "<p>hello world</p>";

// Select "hello" programmatically (or have the user do it),
// then call Uppercase():
richTextEditor.Uppercase();

// Read the resulting HTML through the Value property
string html = (string)richTextEditor.Value;
Debug.WriteLine(html); // <p>HELLO world</p>
```

The two methods are also fully compatible with the `TextChanged` event, so any auto-save or validation logic that runs in response to user edits will also fire when the transformations are applied programmatically.

### Use Cases for the Programmatic Methods

- **Sidebar formatting actions** — surface the transformations in a context panel, AI assistant suggestion, or accessibility toolbar without putting the `TextTransform` toolbar item in the editor.
- **Template and snippet normalization** — load a document, programmatically select a heading or template placeholder, and apply `Uppercase()` to enforce a style.
- **Voice and shortcut integrations** — bind `Uppercase()` and `Lowercase()` to keyboard shortcuts, voice commands, or hardware keyboard accelerators that exist outside the editor\'s toolbar.
- **Automated document cleanup** — run a batch transformation (for example, lowercase all product codes) as part of an import or normalization step.

## Common Transformation Scenarios

### Forcing UPPERCASE Headings

A common case in messaging apps, email composers, and announcement forms is converting a section title to UPPERCASE for emphasis.

```xaml
<rte:SfRichTextEditor ShowToolbar="True">
    <rte:SfRichTextEditor.ToolbarItems>
        <rte:RichTextToolbarItem Type="Bold" />
        <rte:RichTextToolbarItem Type="Italic" />
        <rte:RichTextToolbarItem Type="Separator" />
        <rte:RichTextToolbarItem Type="TextTransform" />
    </rte:SfRichTextEditor.ToolbarItems>
</rte:SfRichTextEditor>
```

User flow:
1. Type `Weekly Update`.
2. Select the text.
3. Tap `TextTransform`, then choose UPPERCASE.
4. Result: `WEEKLY UPDATE`.

### Normalizing Lowercase Content

For lowercase normalization (for example, when users paste content from a source that uses ALL CAPS and you want a clean, sentence-case style):

1. Select the text in the editor.
2. Tap `TextTransform`, then choose lowercase.
3. The selection is replaced with the lowercase form of the same text.

### Sentence Case Cleanup

In editors where users mix case styles (uppercase titles, lowercase bodies), the `TextTransform` toolbar item gives them a single place to switch the case of any selected range. Combine it with `Separator` items so it sits in its own logical group.

```xaml
<rte:SfRichTextEditor ShowToolbar="True">
    <rte:SfRichTextEditor.ToolbarItems>
        <rte:RichTextToolbarItem Type="Bold" />
        <rte:RichTextToolbarItem Type="Italic" />
        <rte:RichTextToolbarItem Type="Underline" />
        <rte:RichTextToolbarItem Type="Separator" />
        <rte:RichTextToolbarItem Type="TextTransform" />
        <rte:RichTextToolbarItem Type="Separator" />
        <rte:RichTextToolbarItem Type="Undo" />
        <rte:RichTextToolbarItem Type="Redo" />
    </rte:SfRichTextEditor.ToolbarItems>
</rte:SfRichTextEditor>
```

## Combining with Other Formatting

Text transformation plays well with the rest of the editor's character formatting because it does not lock the result. After applying a transformation, you can still apply bold, italic, color, or any other style to the same range.

### Mixed Character Formatting with Transformation

```xaml
<rte:SfRichTextEditor ShowToolbar="True">
    <rte:SfRichTextEditor.ToolbarItems>
        <rte:RichTextToolbarItem Type="Bold" />
        <rte:RichTextToolbarItem Type="Italic" />
        <rte:RichTextToolbarItem Type="Underline" />
        <rte:RichTextToolbarItem Type="Strikethrough" />
        <rte:RichTextToolbarItem Type="SubScript" />
        <rte:RichTextToolbarItem Type="SuperScript" />
        <rte:RichTextToolbarItem Type="Separator" />
        <rte:RichTextToolbarItem Type="TextTransform" />
    </rte:SfRichTextEditor.ToolbarItems>
</rte:SfRichTextEditor>
```

Typical flow:
1. Select a word.
2. Apply **Bold** with the `Bold` toolbar item.
3. Apply **UPPERCASE** with the `TextTransform` toolbar item.
4. The selection is now both bold and uppercase.

### TextTransform and the Programmatic API

The TextTransform action is also reflected through the editor's `Value` property (with `TValue="HTML"`), so you can verify the result programmatically:

```csharp
using Syncfusion.Maui.RichTextEditor;

SfRichTextEditor richTextEditor = new SfRichTextEditor
{
    ShowToolbar = true
};

richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Bold });
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.TextTransform });

// Seed the editor with sample content
richTextEditor.Value = "<p>Hello World</p>";

// After the user selects "Hello" and applies a UPPERCASE transformation,
// the Value property reflects the new content:
Debug.WriteLine((string)richTextEditor.Value);
```

## Grouped Toolbar Integration

The `TextTransform` toolbar item works with the **grouped overlay toolbar** just like any other option. When `IsGrouped` is `true`, the `TextTransform` entry becomes one of the overlay groups.

```xaml
<rte:SfRichTextEditor ShowToolbar="True"
                      IsGrouped="True"
                      ToolbarPosition="Bottom">
    <rte:SfRichTextEditor.ToolbarItems>
        <rte:RichTextToolbarItem Type="Bold" />
        <rte:RichTextToolbarItem Type="Italic" />
        <rte:RichTextToolbarItem Type="Underline" />
        <rte:RichTextToolbarItem Type="TextTransform" />
        <rte:RichTextToolbarItem Type="Hyperlink" />
        <rte:RichTextToolbarItem Type="Undo" />
        <rte:RichTextToolbarItem Type="Redo" />
    </rte:SfRichTextEditor.ToolbarItems>
</rte:SfRichTextEditor>
```

This is a strong fit for mobile composers where users frequently reformat or clean up text on the fly. See [toolbar grouping](toolbar-grouping.md) for more details on the grouped toolbar.

## Use Cases and Patterns

### Email and Messaging Apps

Make sure users can quickly fix the case of a name, brand, or product reference without retyping. Combine `TextTransform` with the basic formatting controls:

```xaml
<rte:SfRichTextEditor.ToolbarItems>
    <rte:RichTextToolbarItem Type="Bold" />
    <rte:RichTextToolbarItem Type="Italic" />
    <rte:RichTextToolbarItem Type="Underline" />
    <rte:RichTextToolbarItem Type="Separator" />
    <rte:RichTextToolbarItem Type="TextTransform" />
    <rte:RichTextToolbarItem Type="Separator" />
    <rte:RichTextToolbarItem Type="Hyperlink" />
</rte:SfRichTextEditor.ToolbarItems>
```

### Note-Taking and Knowledge Base Editors

When users paste content from various sources, the case may not be consistent. The TextTransform toolbar item gives them a quick way to normalize capitalization in headings or quoted passages.

### Comments and Reviews

For comment forms and review sections, exposing `TextTransform` lets reviewers emphasize feedback (for example, by selecting a sentence and applying UPPERCASE) without needing to retype.

### Blog and CMS Editors

In blog and CMS editors, `TextTransform` is useful for normalizing titles, subtitles, and quoted content pulled from external sources. Place it near the other character formatting controls:

```xaml
<rte:SfRichTextEditor.ToolbarItems>
    <rte:RichTextToolbarItem Type="Undo" />
    <rte:RichTextToolbarItem Type="Redo" />
    <rte:RichTextToolbarItem Type="Separator" />
    <rte:RichTextToolbarItem Type="Bold" />
    <rte:RichTextToolbarItem Type="Italic" />
    <rte:RichTextToolbarItem Type="Underline" />
    <rte:RichTextToolbarItem Type="Strikethrough" />
    <rte:RichTextToolbarItem Type="Separator" />
    <rte:RichTextToolbarItem Type="TextTransform" />
    <rte:RichTextToolbarItem Type="Separator" />
    <rte:RichTextToolbarItem Type="Hyperlink" />
    <rte:RichTextToolbarItem Type="Image" />
</rte:SfRichTextEditor.ToolbarItems>
```

## Troubleshooting

### TextTransform is not visible in the toolbar

- The `ToolbarItems` collection is not populated. Populating the collection **replaces** the default items, so `TextTransform` will not appear unless you add it explicitly.
- Add `<rte:RichTextToolbarItem Type="TextTransform" />` (XAML) or `new RichTextToolbarItem { Type = RichTextToolbarOptions.TextTransform }` (C#).

### Tapping TextTransform does nothing

- The user has no text selected. Ask the user to select the target text first, then tap `TextTransform`.
- The editor is in read-only mode. Confirm that `ReadOnly` is `false`.

### The transformation seems to be ignored

- Make sure the editor has focus before selecting text and tapping the toolbar item.
- If you are observing the `Value` property, it reflects the current content, so a stale read can mislead you. Re-read after the user confirms the transformation.

### Undo a transformation

- Use the `Undo` toolbar item, or programmatically call `richTextEditor.Undo()`, to revert the most recent transformation, just like any other character formatting change.

### Uppercase() or Lowercase() does not change the content

- The editor does not have focus. Call `Focus()` before invoking the methods.
- There is no active text selection. Select the target range first; the methods are no-ops on an empty selection.
- The editor is read-only (`IsReadOnly` / `ReadOnly` is `true`). Confirm the editor allows edits.

### Reading the editor after Uppercase() or Lowercase()

- `Value` is typed as `object`. Cast to `string` to inspect the resulting HTML:
  ```csharp
  string html = (string)richTextEditor.Value;
  ```
- Subscribe to `TextChanged` to react to the transformation asynchronously rather than reading `Value` immediately after the call.

## Best Practices

### 1. Group TextTransform with Other Character Formatting

Use `Separator` items to visually group `TextTransform` with the rest of the character formatting (Bold, Italic, Underline). This matches the user's mental model: case changes belong to character formatting.

```xaml
<rte:RichTextToolbarItem Type="Bold" />
<rte:RichTextToolbarItem Type="Italic" />
<rte:RichTextToolbarItem Type="Underline" />
<rte:RichTextToolbarItem Type="Separator" />
<rte:RichTextToolbarItem Type="TextTransform" />
```

### 2. Surface TextTransform on Mobile

Case changes are a common cleanup step on mobile. When you enable the grouped overlay toolbar (`IsGrouped="True"`), include `TextTransform` so users can access it without leaving the editor surface.

### 3. Always Require a Selection

Because the action operates on the current selection, ensure the user can clearly see and select the text. Highlight selection handles on touch devices and consider showing a hint when nothing is selected.

### 4. Pair with Undo and Redo

Place `Undo` and `Redo` in the toolbar so the user can quickly reverse a transformation. Undo also reverts the selection change made by `TextTransform` along with the text change.

### 5. Test with Real Content

Verify the `TextTransform` action behaves correctly with mixed-language content, special characters, and content that already includes other formatting (bold, italic, links). Selections inside links, code blocks, or images should be handled consistently with how the rest of the editor treats selections.

### 6. Document It in the UI

If your app's audience is non-technical, consider adding a tooltip or onboarding hint that explains the `TextTransform` action. Because it is a single toolbar entry that opens a menu of options, users may not immediately discover the available transformations.

### 7. Choose Toolbar or Programmatic Methods Based on the UX

Use the `TextTransform` toolbar item when you want case changes to be a first-class part of the editor surface (best for messaging, blog, and CMS editors). Use the `Uppercase()` and `Lowercase()` instance methods when you want to surface the transformations in a side panel, keyboard shortcut, voice command, or automated pipeline without adding an extra toolbar item.

### 8. Always Cast the Value Property When Reading

Because `Value` is typed as `object`, always cast to `string` when you read the HTML content programmatically — both when verifying a toolbar transformation and when reading after calling `Uppercase()` or `Lowercase()`:

```csharp
string html = (string)richTextEditor.Value;
```

## Next Steps

- Review [toolbar configuration](toolbar.md) for the full list of toolbar options and `ToolbarSettings` details
- See [toolbar grouping](toolbar-grouping.md) to combine `TextTransform` with the grouped overlay toolbar
- Explore [formatting and customization](formatting-and-customization.md) for the broader character formatting API
- Pair with [events and interactions](events-and-interactions.md) to react to the editor's `TextChanged` event after a transformation
