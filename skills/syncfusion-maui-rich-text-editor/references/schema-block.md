# Schema Block

## Table of Contents
- [Overview](#overview)
- [Block-Based Document Model](#block-based-document-model)
- [Binding Schema Content](#binding-schema-content)
- [TValue and RichTextEditorValueType](#tvalue-and-richtexteditorvaluetype)
- [Block Nodes](#block-nodes)
- [Working with Lists](#working-with-lists)
- [Working with Hyperlinks](#working-with-hyperlinks)
- [Working with Images](#working-with-images)
- [Working with Code Blocks](#working-with-code-blocks)
- [Combining Block Nodes](#combining-block-nodes)
- [Two-Way Binding with the Value Property](#two-way-binding-with-the-value-property)
- [Use Cases and Patterns](#use-cases-and-patterns)
- [Troubleshooting](#troubleshooting)
- [Best Practices](#best-practices)
- [Next Steps](#next-steps)

## Overview

The .NET MAUI Rich Text Editor supports a **block-based document** in addition to its plain text and HTML content modes. To work with block-schema content, set the `TValue` property to `Schema` and bind a collection of block nodes to the `Value` property.

> N> The public content surface is the `Value` property in both modes. The legacy `Text` and `HtmlText` properties are internal and are not part of the public API; use `TValue` to choose between HTML and schema content, and read or write through `Value`.

### Reading and Writing the Value Property

The `Value` property is typed as `object` because it can hold either a `string` (HTML mode) or an `ObservableCollection<BlockNode>` (Schema mode). Always cast the value to the expected type before using it:

```csharp
// Schema mode — cast to ObservableCollection<BlockNode>
var blocks = (ObservableCollection<BlockNode>)richTextEditor.Value;
```

The same `Value` setter accepts either type, so a single property can be used across both content modes by switching the cast based on `TValue`.

The block schema represents document content as a collection of **strongly typed nodes**, enabling developers to create, load, and manipulate rich text content programmatically without using HTML. It is the recommended model when:

- You want to load, save, and validate structured content from a typed object model.
- You are integrating the editor with an existing document pipeline that already uses a node tree.
- You need to round-trip editor content through serialization formats that prefer JSON-like data over HTML markup.

The block schema supports the common document elements you expect from a rich text editor: paragraphs, headings, lists, hyperlinks, images, tables, code blocks, and inline text formatting.

## Block-Based Document Model

In the schema model, the document is an ordered collection of **block nodes**. Each block node has:

- A **type** (e.g., paragraph, heading, list, code block, image).
- A collection of **child nodes** (the inline content of the block).
- Optional **attributes** (e.g., heading level, language, link target).

Inline content inside a block is represented by **text nodes** and **mark nodes** (such as link marks) that decorate text.

The high-level structure looks like:

```text
Value (ObservableCollection<BlockNode>)
- HeadingNode
  - Attrs (HeadingAttrs)
  - Children
    - TextNode ("Title")
- ParagraphNode
  - Attrs (ParagraphAttrs)
  - Children
    - TextNode ("Visit ")
    - TextNode ("Syncfusion")
      - Marks
        - LinkMark
          - Attrs (LinkMarkAttrs)
- BulletListNode
  - Children
    - ListItemNode ...
- CodeBlockNode
  - Attrs (CodeBlockAttrs)
  - Children
    - TextNode ("Console.WriteLine(...)")
```

## Binding Schema Content

Bind a collection of block nodes to the `Value` property and set `TValue` to `Schema`.

### XAML

```xaml
<rte:SfRichTextEditor
    Value="{Binding BlockNodes}"
    TValue="Schema" />
```

### C# (ViewModel)

```csharp
using Syncfusion.Maui.RichTextEditor;
using System.Collections.ObjectModel;

public class ViewModel
{
    public ObservableCollection<BlockNode> BlockNodes { get; set; }

    public ViewModel()
    {
        BlockNodes = new ObservableCollection<BlockNode>
        {
            new HeadingNode
            {
                Attrs = new HeadingAttrs { Level = 1 },
                Children =
                {
                    new TextNode { Text = "Block Schema" }
                }
            },
            new ParagraphNode
            {
                Attrs = new ParagraphAttrs(),
                Children =
                {
                    new TextNode { Text = "This content is generated using block schema support." }
                }
            }
        };
    }
}
```

When the binding is evaluated, the editor renders the block nodes in document order. As the user edits the document, the `Value` collection is updated to reflect the changes.

## TValue and RichTextEditorValueType

The `TValue` property determines which content model the editor uses.

### TValue Property

- **XAML type** — `string` (the literal value of the enumeration entry as text)
- **C# type** — `RichTextEditorValueType`
- **Default value** — `HTML`

### RichTextEditorValueType Enumeration

| Value | Description |
|---|---|
| `HTML` | Use the `Value` property to hold an HTML string. This is the default mode and is appropriate for plain text and HTML markup. |
| `Schema` | Use the `Value` property to bind a collection of block nodes. |

### Switching to Schema Mode

```xaml
<rte:SfRichTextEditor TValue="Schema" Value="{Binding BlockNodes}" />
```

```csharp
using Syncfusion.Maui.RichTextEditor;

SfRichTextEditor richTextEditor = new SfRichTextEditor
{
    TValue = RichTextEditorValueType.Schema,
    Value = new ObservableCollection<BlockNode>
    {
        new ParagraphNode
        {
            Children = { new TextNode { Text = "Hello from the block schema." } }
        }
    }
};
```

> N> When `TValue` is set to `Schema`, content is read from and written to the `Value` property. The legacy `Text` and `HtmlText` properties are internal and are not used for the primary content model in this mode.

## Block Nodes

The block schema is composed of several node types. Use the ones that match the document structure you want to represent.

### ParagraphNode

Represents a normal paragraph.

```csharp
new ParagraphNode
{
    Attrs = new ParagraphAttrs(),
    Children =
    {
        new TextNode { Text = "This is a paragraph." }
    }
}
```

### HeadingNode

Represents a heading at a specific level.

```csharp
new HeadingNode
{
    Attrs = new HeadingAttrs { Level = 1 },
    Children = { new TextNode { Text = "Heading 1" } }
}
```

The `Level` attribute controls the heading level. Use values 1 through 6 to mirror the `Heading1` through `Heading6` paragraph formats.

### TextNode

Represents a run of text inside a block.

```csharp
new TextNode { Text = "Hello world" }
```

A `TextNode` can also carry **marks** (for example, a link) to apply inline formatting.

## Working with Lists

The block schema supports both bulleted and numbered lists through the `BulletListNode` and `OrderedListNode` elements. Each list contains `ListItemNode` children, and each `ListItemNode` contains one or more block nodes (typically a `ParagraphNode`).

```csharp
new BulletListNode
{
    Children =
    {
        new ListItemNode
        {
            Children =
            {
                new ParagraphNode
                {
                    Children = { new TextNode { Text = "Bullet item" } }
                }
            }
        }
    }
}
```

For numbered lists, replace `BulletListNode` with `OrderedListNode`:

```csharp
new OrderedListNode
{
    Children =
    {
        new ListItemNode
        {
            Children =
            {
                new ParagraphNode
                {
                    Children = { new TextNode { Text = "Step one" } }
                }
            }
        }
    }
}
```

> N> A `ListItemNode` can contain any block node (paragraph, heading, or even another list for nesting). Use a `ParagraphNode` as the most common pattern.

## Working with Hyperlinks

Hyperlinks are created using the `LinkMark`. Apply the mark to a `TextNode` by adding it to the node's `Marks` collection.

```csharp
new ParagraphNode
{
    Children =
    {
        new TextNode
        {
            Text = "Syncfusion",
            Marks =
            {
                new LinkMark
                {
                    Attrs = new LinkMarkAttrs
                    {
                        Href = "https://www.syncfusion.com",
                        Title = "Syncfusion"
                    }
                }
            }
        }
    }
}
```

`LinkMarkAttrs` accepts the target URL (`Href`) and an optional `Title` for the link.

## Working with Images

Images are inserted using the `ImageNode`. The `ImageAttrs` carry the source URL plus optional width and height.

```csharp
new ImageNode
{
    Attrs = new ImageAttrs
    {
        Src = "https://ej2.syncfusion.com/demos/src/block-editor/images/overview.png",
        Width = 100,
        Height = 100
    }
}
```

The `Src` value can be any URL the editor's image pipeline supports. Combine `ImageNode` with the editor's `ImageRequested` event when you need to provide local or streamed image content.

## Working with Code Blocks

The block schema supports syntax-highlighted code blocks through the `CodeBlockNode`. Use the `Language` attribute to specify the language and add a `TextNode` child for the code itself.

```csharp
new CodeBlockNode
{
    Attrs = new CodeBlockAttrs
    {
        Language = "csharp"
    },
    Children =
    {
        new TextNode { Text = "Console.WriteLine(\"Hello World\");" }
    }
}
```

Pair this with the `CodeBlock` toolbar item and the `CodeBlockLanguages` property to control which languages appear in the language dropdown. See [code blocks](code-blocks.md) for the toolbar side of the feature.

## Combining Block Nodes

A typical document combines several block-node types. The example below shows a heading, a paragraph with a link, a bulleted list, and a code block.

```csharp
using Syncfusion.Maui.RichTextEditor;
using System.Collections.ObjectModel;

public class ViewModel
{
    public ObservableCollection<BlockNode> BlockNodes { get; set; } = new()
    {
        new HeadingNode
        {
            Attrs = new HeadingAttrs { Level = 1 },
            Children = { new TextNode { Text = "Welcome" } }
        },
        new ParagraphNode
        {
            Children =
            {
                new TextNode { Text = "Visit " },
                new TextNode
                {
                    Text = "Syncfusion",
                    Marks =
                    {
                        new LinkMark
                        {
                            Attrs = new LinkMarkAttrs
                            {
                                Href = "https://www.syncfusion.com"
                            }
                        }
                    }
                },
                new TextNode { Text = " for documentation." }
            }
        },
        new BulletListNode
        {
            Children =
            {
                new ListItemNode
                {
                    Children =
                    {
                        new ParagraphNode
                        {
                            Children = { new TextNode { Text = "First item" } }
                        }
                    }
                },
                new ListItemNode
                {
                    Children =
                    {
                        new ParagraphNode
                        {
                            Children = { new TextNode { Text = "Second item" } }
                        }
                    }
                }
            }
        },
        new CodeBlockNode
        {
            Attrs = new CodeBlockAttrs { Language = "csharp" },
            Children = { new TextNode { Text = "Console.WriteLine(\"Hello\");" } }
        }
    };
}
```

Bind the `BlockNodes` collection to the `Value` property:

```xaml
<rte:SfRichTextEditor
    Value="{Binding BlockNodes}"
    TValue="Schema" />
```

## Two-Way Binding with the Value Property

The `Value` property supports two-way binding, which means edits made in the editor are reflected back in the bound `ObservableCollection<BlockNode>`. Use an `ObservableCollection<BlockNode>` (not a `List<BlockNode>`) so the binding and any reactive UI can react to changes.

```csharp
using System.ComponentModel;
using System.Runtime.CompilerServices;
using Syncfusion.Maui.RichTextEditor;
using System.Collections.ObjectModel;

public class DocumentViewModel : INotifyPropertyChanged
{
    private ObservableCollection<BlockNode> _blockNodes = new();

    public ObservableCollection<BlockNode> BlockNodes
    {
        get => _blockNodes;
        set
        {
            _blockNodes = value;
            OnPropertyChanged();
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged([CallerMemberName] string name = null)
        => PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

In XAML:

```xaml
<rte:SfRichTextEditor
    Value="{Binding BlockNodes}"
    TValue="Schema" />
```

> N> Changes made in the editor are reflected in the bound `Value` collection. The block schema is therefore a live model: any change in the editor updates the collection, and replacing items in the collection updates the editor.

### Reading the Value Property in Code

When reading the schema `Value` from C# (for example, to upload the current state or to inspect the document), cast it to the `ObservableCollection<BlockNode>` type:

```csharp
// After the user has edited the document, read the current schema
var currentBlocks = (ObservableCollection<BlockNode>)richTextEditor.Value;

// Iterate the structure
foreach (var block in currentBlocks)
{
    if (block is HeadingNode heading)
    {
        Debug.WriteLine($"Heading level: {heading.Attrs?.Level}");
    }
}
```

## Use Cases and Patterns

### Loading Documents from a Backend

A common scenario is loading a document that the backend already represents as a JSON-like node tree. The block schema maps naturally to this representation, so you can deserialize directly into the bound `ObservableCollection<BlockNode>`.

```csharp
using System.Text.Json;
using System.Collections.ObjectModel;
using Syncfusion.Maui.RichTextEditor;

public async Task LoadDocumentAsync(string json)
{
    var nodes = JsonSerializer.Deserialize<ObservableCollection<BlockNode>>(json);
    DocumentViewModel.BlockNodes = nodes ?? new ObservableCollection<BlockNode>();
}
```

### Saving Documents Back to a Backend

Serialize the bound `BlockNodes` collection back to JSON and post it to your API. Because the binding is two-way, the latest editor state is always available in the collection.

```csharp
using System.Text.Json;
using System.Collections.ObjectModel;
using Syncfusion.Maui.RichTextEditor;

public async Task SaveDocumentAsync()
{
    var json = JsonSerializer.Serialize(DocumentViewModel.BlockNodes);
    await apiClient.PostDocumentAsync(json);
}
```

### Building Templates from Block Schema

You can build reusable templates by composing block nodes once and copying them when a new document is created.

```csharp
using System.Collections.ObjectModel;
using Syncfusion.Maui.RichTextEditor;

public static class DocumentTemplates
{
    public static ObservableCollection<BlockNode> CreateBlank()
    {
        return new ObservableCollection<BlockNode>
        {
            new HeadingNode
            {
                Attrs = new HeadingAttrs { Level = 1 },
                Children = { new TextNode { Text = "Untitled" } }
            },
            new ParagraphNode
            {
                Children = { new TextNode { Text = "Start writing here..." } }
            }
        };
    }
}
```

### Combining Schema with Other Content

The schema model is independent of the toolbar. You can still enable the toolbar, configure toolbar items, and let users edit the document through the toolbar. The block schema simply provides a typed model for the resulting content.

```xaml
<rte:SfRichTextEditor
    ShowToolbar="True"
    TValue="Schema"
    Value="{Binding BlockNodes}">
    <rte:SfRichTextEditor.ToolbarItems>
        <rte:RichTextToolbarItem Type="Bold" />
        <rte:RichTextToolbarItem Type="Italic" />
        <rte:RichTextToolbarItem Type="Underline" />
        <rte:RichTextToolbarItem Type="Separator" />
        <rte:RichTextToolbarItem Type="NumberList" />
        <rte:RichTextToolbarItem Type="BulletList" />
        <rte:RichTextToolbarItem Type="CodeBlock" />
    </rte:SfRichTextEditor.ToolbarItems>
</rte:SfRichTextEditor>
```

## Troubleshooting

### The editor appears empty

- Confirm `TValue="Schema"` is set and `Value` is bound to a non-null `ObservableCollection<BlockNode>`.
- If you assign `Value` in code, do it after the `SfRichTextEditor` is added to the visual tree, or use a `Binding` so the editor receives the value through the binding system.

### Block nodes do not render the way you expect

- Verify the node types you are using match the supported schema. For example, a list must be a `BulletListNode` or `OrderedListNode` whose children are `ListItemNode` instances.
- For headings, set `Level` on `HeadingAttrs`. Omitting the level uses the default, which may not match the visual style you expect.

### Edits in the editor are not reflected in the bound collection

- Use `ObservableCollection<BlockNode>` for the bound property so changes propagate. A plain `List<BlockNode>` does not notify the editor about changes.
- Make sure the binding mode supports two-way updates. The default `Value` binding mode is two-way.

### Custom node types are ignored

- The block schema is a fixed set of node types provided by the editor (for example, `HeadingNode`, `ParagraphNode`, `BulletListNode`, `ImageNode`, `CodeBlockNode`). Use the documented types and attributes for guaranteed behavior.

### Switching from HTML to Schema

- Setting `TValue` to `Schema` switches the content model. Existing `Value` (HTML) content is not automatically converted into block nodes. Plan the migration by serializing your existing content into the schema model ahead of time, or load from a fresh source.

## Best Practices

### 1. Use `ObservableCollection<BlockNode>` for `Value`

Two-way binding and reactive UI scenarios require an observable collection. Use `ObservableCollection<BlockNode>` and raise `PropertyChanged` when you replace the collection itself.

### 2. Compose Block Nodes Outside the View

Build the initial `BlockNodes` collection in your ViewModel or a service. Avoid constructing nodes inside XAML code-behind or inline event handlers so the model stays testable and decoupled from the view.

### 3. Reuse Templates for Common Documents

For repeated document shapes (meeting notes, blog posts, bug reports), build a small set of reusable templates and clone them when creating new documents. This keeps the schema model small, predictable, and easy to validate.

### 4. Keep Node Identifiers Optional

Node identifiers are not required when creating block schema content. Add them only when you have a specific need to reference individual nodes from your own logic.

### 5. Validate the Schema at the Boundary

If your block schema content crosses an external boundary (a backend API, a JSON file, or a user-uploaded payload), validate the structure before assigning it to the bound `Value` property. Reject unknown node types, malformed attribute values, and unsafe URLs to keep the editor's state predictable.

### 6. Pair with `ImageRequested` for Local Images

When an `ImageNode` uses a local file path or a stream, use the `ImageRequested` event to supply the actual content. This keeps the schema model lightweight and avoids embedding large binary data directly in node attributes.

### 7. Document the Schema in Your Codebase

Treat the block schema as part of your data contract. Keep a small set of helper methods (`CreateHeading`, `CreateParagraphWithLink`, `CreateBulletList`) on top of the raw node types so the rest of your code does not have to remember the exact node hierarchy.

## Next Steps

- See [content management](content-management.md) for the HTML and plain-text content models you can switch between
- Review [code blocks](code-blocks.md) for the toolbar and language configuration that complements the `CodeBlockNode` schema element
- Explore [events and interactions](events-and-interactions.md) to react to edits made through the schema model
- Pair with [toolbar configuration](toolbar.md) to give users a familiar toolbar on top of the schema model
