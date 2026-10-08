# Toolbar Grouping

## Table of Contents
- [Overview](#overview)
- [Understanding Grouped Toolbar Items](#understanding-grouped-toolbar-items)
- [Enabling Grouped Toolbar Items](#enabling-grouped-toolbar-items)
- [IsGrouped Property](#isgrouped-property)
- [Group Behavior and UX](#group-behavior-and-ux)
- [Choosing Items for a Grouped Toolbar](#choosing-items-for-a-grouped-toolbar)
- [Toolbar Position Interaction](#toolbar-position-interaction)
- [Combined Customization](#combined-customization)
- [Use Cases and Patterns](#use-cases-and-patterns)
- [Troubleshooting](#troubleshooting)
- [Best Practices](#best-practices)
- [Next Steps](#next-steps)

## Overview

The Rich Text Editor introduces an **Overlay (Grouped) Toolbar** mode that enhances the mobile editing experience. When the `IsGrouped` property is enabled, related toolbar items are organized into grouped overlay menus that appear context-aware near the selected content. This produces a cleaner, more intuitive interface while optimizing screen space on mobile platforms and allows users to quickly access formatting commands without interrupting their editing workflow.

The Overlay Toolbar is designed primarily for touch-driven scenarios where screen real estate is limited and the user benefits from contextual, on-demand formatting menus.

## Understanding Grouped Toolbar Items

In the default (non-grouped) toolbar, every item in the `ToolbarItems` collection is rendered inline in a single horizontal strip. While this is intuitive on desktop, it forces mobile users to either scroll horizontally or settle for a cramped layout.

The **grouped toolbar** flips this interaction:

- Each **item in the `ToolbarItems` collection** maps to a **separate overlay menu** that appears as a floating, context-aware menu near the user's selection.
- The menus are revealed when the user needs them (e.g., when text is selected or the editor is focused), instead of permanently occupying toolbar space.
- Inside each overlay menu, related formatting actions are surfaced together, giving the user quick access to logical groups of commands.

This is conceptually similar to the floating bubble toolbars seen in modern document editors and word-processing apps, but it is driven by the explicit items you add to `ToolbarItems`.

## Enabling Grouped Toolbar Items

Enable grouped toolbar items by setting the `IsGrouped` property on `SfRichTextEditor` to `true` and providing a `ToolbarItems` collection that defines the groups you want to expose.

### XAML

```xaml
<rte:SfRichTextEditor x:Name="richTextEditor"
                      ShowToolbar="True"
                      IsGrouped="True">
    <rte:SfRichTextEditor.ToolbarItems>
        <rte:RichTextToolbarItem Type="Bold" />
        <rte:RichTextToolbarItem Type="Italic" />
        <rte:RichTextToolbarItem Type="Underline" />
        <rte:RichTextToolbarItem Type="BulletList" />
        <rte:RichTextToolbarItem Type="NumberList" />
        <rte:RichTextToolbarItem Type="Alignment" />
        <rte:RichTextToolbarItem Type="Hyperlink" />
        <rte:RichTextToolbarItem Type="Undo" />
        <rte:RichTextToolbarItem Type="Redo" />
    </rte:SfRichTextEditor.ToolbarItems>
</rte:SfRichTextEditor>
```

### C#

```csharp
using Syncfusion.Maui.RichTextEditor;

SfRichTextEditor richTextEditor = new SfRichTextEditor
{
    ShowToolbar = true,
    IsGrouped = true
};

richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Bold });
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Italic });
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Underline });
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.NumberList });
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.BulletList });
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Alignment });
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Hyperlink });
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Undo });
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Redo });
```

When `IsGrouped` is `true`, the editor renders overlay groups instead of the inline toolbar strip.

> N> The `IsGrouped` property is independent of `ShowToolbar`. When `IsGrouped` is enabled, the editor still relies on the toolbar (and `ShowToolbar` should remain `true`) to expose formatting actions, but renders them as overlay menus.

## IsGrouped Property

### Property Signature

```csharp
public bool IsGrouped { get; set; }
```

### Property Details

- **Type** — `bool`
- **Default value** — `false`
- **Setter** — Toggles between the standard inline toolbar and the grouped overlay toolbar

### Property Behavior

| Value | Behavior |
|---|---|
| `false` (default) | The toolbar renders as a single, horizontal strip with all items in the `ToolbarItems` collection. |
| `true` | Related items from `ToolbarItems` are organized into grouped overlay menus that appear contextually. |

### Switching at Runtime

Because `IsGrouped` is a regular bindable property, you can switch between the two layouts at runtime in response to platform, screen size, or user preference.

```csharp
#if ANDROID || IOS
    richTextEditor.IsGrouped = true;   // Overlay on mobile
#else
    richTextEditor.IsGrouped = false;  // Inline on desktop
#endif
```

```xaml
<rte:SfRichTextEditor IsGrouped="{Binding UseGroupedToolbar}" />
```

## Group Behavior and UX

When `IsGrouped` is `true`:

- **Context-aware menus** — Overlay groups appear near the selected content rather than in a fixed toolbar row.
- **Logical grouping** — Each item you add to `ToolbarItems` becomes a grouping anchor; related actions are surfaced together inside its overlay menu.
- **Optimized screen space** — The interface stays uncluttered, leaving the editor content area as the focus.
- **Uninterrupted editing** — Users can keep editing while the menus remain accessible, instead of having to scroll to find a formatting control.
- **Touch-friendly** — The overlay menus are sized and positioned for finger interaction, which is especially useful on Android and iOS.

> N> The grouped overlay toolbar is most effective when paired with a focused, minimal `ToolbarItems` collection. Adding every available toolbar item dilutes the benefit because each item is still surfaced as a separate group.

## Choosing Items for a Grouped Toolbar

Because each `ToolbarItems` entry becomes its own group, prefer a small, focused set of high-impact actions.

### Recommended Grouped Set

```xaml
<rte:SfRichTextEditor.ToolbarItems>
    <rte:RichTextToolbarItem Type="Bold" />
    <rte:RichTextToolbarItem Type="Italic" />
    <rte:RichTextToolbarItem Type="Underline" />
    <rte:RichTextToolbarItem Type="BulletList" />
    <rte:RichTextToolbarItem Type="NumberList" />
    <rte:RichTextToolbarItem Type="Alignment" />
    <rte:RichTextToolbarItem Type="Hyperlink" />
    <rte:RichTextToolbarItem Type="Undo" />
    <rte:RichTextToolbarItem Type="Redo" />
</rte:SfRichTextEditor.ToolbarItems>
```

This set covers the most common editing actions for mobile use while keeping the overlay focused.

### Items Best Suited for Grouped Toolbar

- **Character formatting** — `Bold`, `Italic`, `Underline`, `Strikethrough`
- **Lists** — `NumberList`, `BulletList`
- **Alignment** — `Alignment`
- **Links** — `Hyperlink`
- **History** — `Undo`, `Redo`

### Items Less Suited for Grouped Toolbar

Some items render a dedicated dropdown or picker UI that already consumes significant space. Adding them as groups in a touch-first overlay can introduce extra taps and reduce the experience:

- `FontFamily` and `FontSize` (font pickers)
- `TextColor` and `HighlightColor` (color pickers)
- `ParagraphFormat` (paragraph style picker)
- `Image`, `Table`, `CodeBlock` (insert a complex node)

Prefer the inline toolbar for these items so the dedicated picker UI remains the focus.

## Toolbar Position Interaction

`IsGrouped` works with both `ToolbarPosition` values:

- **Top** — The grouped overlay menus are anchored near the top of the editor surface.
- **Bottom** — The grouped overlay menus are anchored near the bottom, which is often the most ergonomic position for thumb reach on phones.

```xaml
<rte:SfRichTextEditor ShowToolbar="True"
                      IsGrouped="True"
                      ToolbarPosition="Bottom">
    <rte:SfRichTextEditor.ToolbarItems>
        <rte:RichTextToolbarItem Type="Bold" />
        <rte:RichTextToolbarItem Type="Italic" />
        <rte:RichTextToolbarItem Type="Underline" />
        <rte:RichTextToolbarItem Type="Alignment" />
        <rte:RichTextToolbarItem Type="Hyperlink" />
    </rte:SfRichTextEditor.ToolbarItems>
</rte:SfRichTextEditor>
```

> N> On Android and iOS the default `ToolbarPosition` is `Bottom`, which is a strong match for the overlay toolbar's mobile-first design. On Windows and macOS the default is `Top`.

## Combined Customization

The grouped toolbar integrates with the rest of the editor's customization surface. You can still configure `ToolbarSettings`, default text styles, placeholder text, and event handlers in the same way as the inline toolbar.

### XAML with ToolbarSettings

```xaml
<rte:SfRichTextEditor x:Name="richTextEditor"
                      ShowToolbar="True"
                      IsGrouped="True"
                      Placeholder="Compose a message...">
    <rte:SfRichTextEditor.ToolbarSettings>
        <rte:RichTextEditorToolbarSettings BackgroundColor="#F5F5F5"
                                           TextColor="#202020"
                                           SelectionColor="#0A76FF" />
    </rte:SfRichTextEditor.ToolbarSettings>
    <rte:SfRichTextEditor.ToolbarItems>
        <rte:RichTextToolbarItem Type="Bold" />
        <rte:RichTextToolbarItem Type="Italic" />
        <rte:RichTextToolbarItem Type="Underline" />
        <rte:RichTextToolbarItem Type="BulletList" />
        <rte:RichTextToolbarItem Type="NumberList" />
        <rte:RichTextToolbarItem Type="Alignment" />
        <rte:RichTextToolbarItem Type="Hyperlink" />
    </rte:SfRichTextEditor.ToolbarItems>
</rte:SfRichTextEditor>
```

### C# with ViewModel Binding

```csharp
using Syncfusion.Maui.RichTextEditor;

public class EditorViewModel
{
    public bool UseGroupedToolbar { get; set; } = true;
    public Color ToolbarBackground { get; set; } = Color.FromArgb("#F5F5F5");
    public Color ToolbarAccent { get; set; } = Color.FromArgb("#0A76FF");
}

public partial class EditorPage : ContentPage
{
    public EditorPage()
    {
        InitializeComponent();

        var viewModel = new EditorViewModel();
        BindingContext = viewModel;

        richTextEditor.IsGrouped = viewModel.UseGroupedToolbar;
        richTextEditor.ToolbarSettings = new RichTextEditorToolbarSettings
        {
            BackgroundColor = viewModel.ToolbarBackground,
            SelectionColor = viewModel.ToolbarAccent
        };
    }
}
```

## Use Cases and Patterns

### Mobile Messaging Composer

A chat or email composer on Android and iOS benefits from a focused, touch-friendly overlay toolbar.

```xaml
<rte:SfRichTextEditor IsGrouped="True"
                      ToolbarPosition="Bottom"
                      ShowToolbar="True"
                      Placeholder="Type a message...">
    <rte:SfRichTextEditor.ToolbarItems>
        <rte:RichTextToolbarItem Type="Bold" />
        <rte:RichTextToolbarItem Type="Italic" />
        <rte:RichTextToolbarItem Type="Underline" />
        <rte:RichTextToolbarItem Type="BulletList" />
        <rte:RichTextToolbarItem Type="NumberList" />
        <rte:RichTextToolbarItem Type="Hyperlink" />
        <rte:RichTextToolbarItem Type="Undo" />
        <rte:RichTextToolbarItem Type="Redo" />
    </rte:SfRichTextEditor.ToolbarItems>
</rte:SfRichTextEditor>
```

### Comment or Reply Form

Compact comment boxes on social or feedback screens are a strong fit for the grouped toolbar. The overlay groups stay out of the way until the user actively edits.

```xaml
<ScrollView>
    <VerticalStackLayout Spacing="12" Padding="12">
        <Label Text="Add a comment" FontAttributes="Bold" />
        <rte:SfRichTextEditor IsGrouped="True"
                              ShowToolbar="True"
                              Placeholder="Share your thoughts...">
            <rte:SfRichTextEditor.ToolbarItems>
                <rte:RichTextToolbarItem Type="Bold" />
                <rte:RichTextToolbarItem Type="Italic" />
                <rte:RichTextToolbarItem Type="BulletList" />
                <rte:RichTextToolbarItem Type="Hyperlink" />
            </rte:SfRichTextEditor.ToolbarItems>
        </rte:SfRichTextEditor>
        <Button Text="Post comment" HorizontalOptions="End" />
    </VerticalStackLayout>
</ScrollView>
```

### Adapting Layout by Platform

Use a platform conditional to switch between grouped and inline toolbars based on the form factor.

```csharp
using Syncfusion.Maui.RichTextEditor;

SfRichTextEditor richTextEditor = new SfRichTextEditor
{
    ShowToolbar = true
};

#if ANDROID || IOS
    richTextEditor.IsGrouped = true;
    richTextEditor.ToolbarPosition = RichTextEditorToolbarPosition.Bottom;
#else
    richTextEditor.IsGrouped = false;
    richTextEditor.ToolbarPosition = RichTextEditorToolbarPosition.Top;
#endif

richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Bold });
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Italic });
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Underline });
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Alignment });
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Hyperlink });
```

## Troubleshooting

### Grouped toolbar is not appearing

- Confirm `ShowToolbar` is `true` and `IsGrouped` is `true` on the same `SfRichTextEditor` instance.
- Ensure you have populated `ToolbarItems`. When `ToolbarItems` is empty, the editor has nothing to surface as overlay groups.

### Overlay menus feel too crowded

- Reduce the number of items in `ToolbarItems`. Each item becomes its own group.
- Move dense pickers (`FontFamily`, `FontSize`, `TextColor`, `ParagraphFormat`, `Image`, `Table`, `CodeBlock`) out of the grouped configuration and use the inline toolbar for them.

### Overlay menus feel out of place on desktop

- The grouped toolbar is optimized for touch and small screens. On Windows and macOS, prefer the default inline toolbar by leaving `IsGrouped` at `false` (or set it to `false` explicitly).
- Use a platform conditional (`#if ANDROID || IOS`) to enable `IsGrouped` only on mobile targets.

### Users cannot find a specific formatting option

- Surface only the most common options as groups and consider complementing the grouped toolbar with a custom button that opens a dedicated formatting dialog for the less common options.

## Best Practices

### 1. Use a Focused Item Set

Each entry in `ToolbarItems` becomes a group. Aim for **5 to 9** carefully chosen groups to keep the overlay usable and avoid overwhelming the user.

### 2. Enable the Grouped Toolbar on Mobile Only

The grouped toolbar shines on touch devices. Use platform conditionals to keep the inline toolbar on desktop and the grouped toolbar on Android and iOS.

### 3. Keep Heavy Pickers Inline

Leave `FontFamily`, `FontSize`, `TextColor`, `HighlightColor`, `ParagraphFormat`, `Image`, `Table`, and `CodeBlock` in the inline toolbar because they already have rich dedicated UIs. Reserve the grouped toolbar for actions that map cleanly to a single tap.

### 4. Pair with `ToolbarPosition="Bottom"` on Phones

Bottom placement aligns with thumb reach and pairs naturally with the overlay toolbar's mobile-first design. Top placement is a better match when the editor lives below a header that should remain visible.

### 5. Combine with `ToolbarSettings` for Branding

Use `ToolbarSettings` to match the grouped toolbar's accent colors to your app's brand. This keeps the overlay visually consistent with the rest of the editor.

### 6. Test the Touch Experience

Verify the grouped toolbar on the smallest target screen size (e.g., a 5-inch phone). Ensure the overlay menus are large enough to tap and do not obstruct the content being edited.

### 7. Provide a Path to Advanced Formatting

If your editor needs advanced features (tables, code blocks, images), keep the inline toolbar as a fallback or expose them through a custom "More" entry to keep the grouped overlay focused.

## Next Steps

- Review [toolbar configuration](toolbar.md) for `ToolbarPosition`, `ToolbarItems`, and `ToolbarSettings` details
- Explore [formatting and customization](formatting-and-customization.md) for the actions surfaced by the grouped toolbar
- See [code blocks](code-blocks.md) for combining inline and grouped toolbars
- Pair with [events and interactions](events-and-interactions.md) to react to user actions in the grouped toolbar
