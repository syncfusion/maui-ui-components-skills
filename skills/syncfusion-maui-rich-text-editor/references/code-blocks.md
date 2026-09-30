# Code Blocks

## Table of Contents
- [Overview](#overview)
- [Enable Code Block](#enable-code-block)
- [Toolbar Configuration](#toolbar-configuration)
- [Customizing Code Block Languages](#customizing-code-block-languages)
- [Use Cases and Patterns](#use-cases-and-patterns)
- [Best Practices](#best-practices)
- [Next Steps](#next-steps)

## Overview

The .NET MAUI Rich Text Editor (`SfRichTextEditor`) includes built-in support for inserting and managing code blocks. This feature enables developers and end users to embed formatted code snippets within rich text content while preserving structure, readability, and formatting consistency.

Code blocks are especially useful in applications that involve technical documentation, blogging platforms, or developer-centric tools.

## Enable Code Block

You can enable a code block using the `CodeBlock` toolbar item available in the rich text editor toolbar.

### XAML

```xaml
<rte:SfRichTextEditor ShowToolbar="True">
    <rte:SfRichTextEditor.ToolbarItems>
        <rte:RichTextToolbarItem Type="CodeBlock" />
    </rte:SfRichTextEditor.ToolbarItems>
</rte:SfRichTextEditor>
```

### C#

```csharp
using Syncfusion.Maui.RichTextEditor;

// Create the Rich Text Editor
SfRichTextEditor richTextEditor = new SfRichTextEditor
{
    ShowToolbar = true
};

// Add CodeBlock toolbar item
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem
{
    Type = RichTextToolbarOptions.CodeBlock
});
```

## Toolbar Configuration

The `CodeBlock` toolbar item is added to the `ToolbarItems` collection just like any other toolbar item. You can place it anywhere in the toolbar order, and combine it with `Separator` items for visual grouping.

### Combine with Other Items

```xaml
<rte:SfRichTextEditor ShowToolbar="True">
    <rte:SfRichTextEditor.ToolbarItems>
        <rte:RichTextToolbarItem Type="Bold" />
        <rte:RichTextToolbarItem Type="Italic" />
        <rte:RichTextToolbarItem Type="Underline" />
        <rte:RichTextToolbarItem Type="Separator" />
        <rte:RichTextToolbarItem Type="CodeBlock" />
    </rte:SfRichTextEditor.ToolbarItems>
</rte:SfRichTextEditor>
```

```csharp
using Syncfusion.Maui.RichTextEditor;

SfRichTextEditor richTextEditor = new SfRichTextEditor
{
    ShowToolbar = true
};

// Character formatting
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Bold });
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Italic });
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Underline });
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Separator });

// Code block
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.CodeBlock });
```

## Customizing Code Block Languages

The `CodeBlockLanguages` property allows you to customize the list of programming languages displayed in the code block language selection dropdown. By default, the editor provides a predefined set of common languages.

### Property Signature

```csharp
public IList<string> CodeBlockLanguages { get; set; }
```

### Property Type

- **Type** — `IList<string>`
- **Default** — A predefined set of common languages (see below)
- **Setter** — Replaces the language list shown in the code block dropdown

### Default Languages

The code block toolbar dropdown includes the following languages by default:
- Plain Text
- C#
- JavaScript
- TypeScript
- HTML
- CSS
- Java
- Python
- Ruby
- Go
- PHP
- Kotlin
- Swift
- Rust
- SQL

### XAML Configuration (Binding Approach)

In MAUI, you can bind `CodeBlockLanguages` to a collection defined in your code-behind or ViewModel:

**XAML:**
```xaml
<rte:SfRichTextEditor x:Name="richTextEditor" 
                      ShowToolbar="True"
                      CodeBlockLanguages="{Binding CustomLanguages}">
    <rte:SfRichTextEditor.ToolbarItems>
        <rte:RichTextToolbarItem Type="CodeBlock" />
    </rte:SfRichTextEditor.ToolbarItems>
</rte:SfRichTextEditor>
```

**Code-Behind or ViewModel:**
```csharp
public List<string> CustomLanguages { get; set; } = new List<string>
{
    "C#",
    "JavaScript",
    "Python",
    "HTML",
    "CSS",
    "XML"
};
```

Alternatively, configure `CodeBlockLanguages` in the code-behind using the approach shown in the C# Configuration section below.

### C# Configuration

```csharp
using Syncfusion.Maui.RichTextEditor;

// Create the Rich Text Editor
SfRichTextEditor richTextEditor = new SfRichTextEditor
{
    ShowToolbar = true
};

// Replace the default language list with a custom set
richTextEditor.CodeBlockLanguages = new List<string>
{
    "Plain Text",
    "C#",
    "JavaScript",
    "Python",
    "Go",
    "Rust",
    "YAML",
    "Terraform",
    "Docker"
};

// Add CodeBlock toolbar item
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem
{
    Type = RichTextToolbarOptions.CodeBlock
});
```

### Append to Default Languages

To keep the default languages and extend the list, retrieve the current collection and add entries:

```csharp
// Start with the current language list
var languages = richTextEditor.CodeBlockLanguages;

// Append custom languages
languages.Add("YAML");
languages.Add("Terraform");
languages.Add("Docker");

richTextEditor.CodeBlockLanguages = languages;
```

For full details on toolbar customization, see [Toolbar Configuration](toolbar.md).

## Use Cases and Patterns

- **Technical documentation** — Embed syntax-highlighted code samples alongside descriptive text.
- **Blogging platforms** — Allow authors to publish tutorials with properly formatted code blocks.
- **Developer-centric tools** — Support note-taking, Q&A, or knowledge-base apps where users share code.
- **Email composers** — Let technical users paste clean, formatted code snippets into messages.
- **Feedback and review forms** — Enable reviewers to attach code snippets to comments or bug reports.

## Best Practices

- Place the `CodeBlock` toolbar item near other formatting controls for quick access.
- Use `Separator` items to visually group the `CodeBlock` option from adjacent formatting tools.
- Combine code block support with HTML output (`HtmlText` property) to persist formatted snippets for later rendering.
- For content workflows that round-trip code through HTML, validate the resulting markup preserves the code block structure.
- When pairing code blocks with `TextChanged`, ensure downstream consumers handle multi-line preformatted content correctly.

## Next Steps

- Learn about [content management](content-management.md) for round-tripping code blocks through HTML
- Handle [events](events-and-interactions.md) for tracking when code blocks are added or edited
- Pair with [toolbar configuration](toolbar.md) for advanced toolbar customization
- Explore [formatting and customization](formatting-and-customization.md) for code-block-friendly default styles
