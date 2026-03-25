# Toolbar Configuration

## Table of Contents
- [Overview](#overview)
- [Toolbar Position](#toolbar-position)
- [Available Toolbar Items](#available-toolbar-items)
- [Adding and Removing Items](#adding-and-removing-items)
- [Toolbar Item Categories](#toolbar-item-categories)
- [Inline Link Tooltip](#inline-link-tooltip)
- [Toolbar Appearance Customization](#toolbar-appearance-customization)
- [Platform-Specific Behaviors](#platform-specific-behaviors)
- [Best Practices](#best-practices)

## Overview

The Rich Text Editor toolbar provides quick access to formatting operations. It's highly customizable, allowing you to control which items appear, their order, and the toolbar's visual style. By default, the toolbar includes a comprehensive set of formatting tools, but you can tailor it to your specific use case.

## Toolbar Position

Control where the toolbar appears relative to the content area using the `ToolbarPosition` property.

### Available Positions

- **Top** - Toolbar appears above the editor content
- **Bottom** - Toolbar appears below the editor content

### Default Behavior by Platform

- **Windows/macOS:** Top (follows desktop convention)
- **Android/iOS:** Bottom (optimized for thumb reach on mobile)

### XAML Configuration

```xml
<!-- Position at top -->
<rte:SfRichTextEditor ToolbarPosition="Top" ShowToolbar="True" />

<!-- Position at bottom -->
<rte:SfRichTextEditor ToolbarPosition="Bottom" ShowToolbar="True" />
```

### C# Configuration

```csharp
SfRichTextEditor richTextEditor = new SfRichTextEditor();
richTextEditor.ShowToolbar = true;
richTextEditor.ToolbarPosition = RichTextEditorToolbarPosition.Bottom;
```

### When to Use Each Position

**Top Position:**
- Desktop applications where toolbar is expected at top
- Applications with fixed footer navigation
- When editor takes full vertical space

**Bottom Position:**
- Mobile-first applications for thumb accessibility
- Applications where content should be immediately visible
- When keyboard appears (keeps toolbar accessible)

## Available Toolbar Items

The Rich Text Editor supports the following toolbar item types through the `RichTextToolbarOptions` enumeration:

### Character Formatting
- **Bold** - Toggle bold text
- **Italic** - Toggle italic text
- **Underline** - Toggle underline
- **Strikethrough** - Toggle strikethrough text
- **SubScript** - Toggle subscript (H₂O)
- **SuperScript** - Toggle superscript (x²)

### Text Styling
- **FontFamily** - Font family picker dropdown
- **FontSize** - Font size picker dropdown
- **TextColor** - Text color picker
- **HighlightColor** - Background highlight color picker

### Paragraph Formatting
- **ParagraphFormat** - Paragraph style dropdown (Normal, Heading 1-6)
- **Alignment** - Text alignment options (left, center, right, justify)

### Lists and Indentation
- **NumberList** - Toggle numbered list
- **BulletList** - Toggle bulleted list
- **IncreaseIndent** - Increase paragraph indentation
- **DecreaseIndent** - Decrease paragraph indentation

### Media and Links
- **Hyperlink** - Insert/edit hyperlink
- **Image** - Insert image
- **Table** - Insert table

### History
- **Undo** - Undo last action
- **Redo** - Redo last undone action

### Visual
- **Separator** - Visual separator line between toolbar items

## Adding and Removing Items

### Using Default Toolbar

Enable the toolbar without specifying items to get all default options:

```xml
<rte:SfRichTextEditor ShowToolbar="True" />
```

This includes all toolbar items listed above.

### Customizing Toolbar Items

Populate the `ToolbarItems` collection to specify exactly which items appear:

```xml
<rte:SfRichTextEditor ShowToolbar="True">
    <rte:SfRichTextEditor.ToolbarItems>
        <rte:RichTextToolbarItem Type="Bold" />
        <rte:RichTextToolbarItem Type="Italic" />
        <rte:RichTextToolbarItem Type="Underline" />
        <rte:RichTextToolbarItem Type="Separator" />
        <rte:RichTextToolbarItem Type="NumberList" />
        <rte:RichTextToolbarItem Type="BulletList" />
        <rte:RichTextToolbarItem Type="Separator" />
        <rte:RichTextToolbarItem Type="Hyperlink" />
        <rte:RichTextToolbarItem Type="Image" />
    </rte:SfRichTextEditor.ToolbarItems>
</rte:SfRichTextEditor>
```

### C# Dynamic Configuration

```csharp
SfRichTextEditor richTextEditor = new SfRichTextEditor();
richTextEditor.ShowToolbar = true;

// Clear default items
richTextEditor.ToolbarItems.Clear();

// Add specific items
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Bold });
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Italic });
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Underline });
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Separator });
richTextEditor.ToolbarItems.Add(new RichTextToolbarItem { Type = RichTextToolbarOptions.Hyperlink });
```

## Toolbar Item Categories

### Minimal Toolbar (Basic Formatting Only)

For simple text editing needs:

```xml
<rte:SfRichTextEditor.ToolbarItems>
    <rte:RichTextToolbarItem Type="Bold" />
    <rte:RichTextToolbarItem Type="Italic" />
    <rte:RichTextToolbarItem Type="Underline" />
</rte:SfRichTextEditor.ToolbarItems>
```

### Standard Toolbar (Common Formatting)

For typical document editing:

```xml
<rte:SfRichTextEditor.ToolbarItems>
    <!-- Character Formatting -->
    <rte:RichTextToolbarItem Type="Bold" />
    <rte:RichTextToolbarItem Type="Italic" />
    <rte:RichTextToolbarItem Type="Underline" />
    <rte:RichTextToolbarItem Type="Strikethrough" />
    <rte:RichTextToolbarItem Type="Separator" />
    
    <!-- Text Styling -->
    <rte:RichTextToolbarItem Type="FontFamily" />
    <rte:RichTextToolbarItem Type="FontSize" />
    <rte:RichTextToolbarItem Type="TextColor" />
    <rte:RichTextToolbarItem Type="Separator" />
    
    <!-- Lists -->
    <rte:RichTextToolbarItem Type="NumberList" />
    <rte:RichTextToolbarItem Type="BulletList" />
    <rte:RichTextToolbarItem Type="Separator" />
    
    <!-- Links -->
    <rte:RichTextToolbarItem Type="Hyperlink" />
</rte:SfRichTextEditor.ToolbarItems>
```

### Full Toolbar (All Features)

For advanced document editing with media:

```xml
<rte:SfRichTextEditor.ToolbarItems>
    <!-- History -->
    <rte:RichTextToolbarItem Type="Undo" />
    <rte:RichTextToolbarItem Type="Redo" />
    <rte:RichTextToolbarItem Type="Separator" />
    
    <!-- Character Formatting -->
    <rte:RichTextToolbarItem Type="Bold" />
    <rte:RichTextToolbarItem Type="Italic" />
    <rte:RichTextToolbarItem Type="Underline" />
    <rte:RichTextToolbarItem Type="Strikethrough" />
    <rte:RichTextToolbarItem Type="SubScript" />
    <rte:RichTextToolbarItem Type="SuperScript" />
    <rte:RichTextToolbarItem Type="Separator" />
    
    <!-- Text Styling -->
    <rte:RichTextToolbarItem Type="FontFamily" />
    <rte:RichTextToolbarItem Type="FontSize" />
    <rte:RichTextToolbarItem Type="TextColor" />
    <rte:RichTextToolbarItem Type="HighlightColor" />
    <rte:RichTextToolbarItem Type="Separator" />
    
    <!-- Paragraph -->
    <rte:RichTextToolbarItem Type="ParagraphFormat" />
    <rte:RichTextToolbarItem Type="Alignment" />
    <rte:RichTextToolbarItem Type="Separator" />
    
    <!-- Lists and Indentation -->
    <rte:RichTextToolbarItem Type="NumberList" />
    <rte:RichTextToolbarItem Type="BulletList" />
    <rte:RichTextToolbarItem Type="IncreaseIndent" />
    <rte:RichTextToolbarItem Type="DecreaseIndent" />
    <rte:RichTextToolbarItem Type="Separator" />
    
    <!-- Media -->
    <rte:RichTextToolbarItem Type="Hyperlink" />
    <rte:RichTextToolbarItem Type="Image" />
    <rte:RichTextToolbarItem Type="Table" />
</rte:SfRichTextEditor.ToolbarItems>
```

### Email/Messaging Toolbar

Optimized for communication apps:

```xml
<rte:SfRichTextEditor.ToolbarItems>
    <rte:RichTextToolbarItem Type="Bold" />
    <rte:RichTextToolbarItem Type="Italic" />
    <rte:RichTextToolbarItem Type="Underline" />
    <rte:RichTextToolbarItem Type="Separator" />
    <rte:RichTextToolbarItem Type="FontFamily" />
    <rte:RichTextToolbarItem Type="FontSize" />
    <rte:RichTextToolbarItem Type="TextColor" />
    <rte:RichTextToolbarItem Type="Separator" />
    <rte:RichTextToolbarItem Type="NumberList" />
    <rte:RichTextToolbarItem Type="BulletList" />
    <rte:RichTextToolbarItem Type="Separator" />
    <rte:RichTextToolbarItem Type="Hyperlink" />
    <rte:RichTextToolbarItem Type="Image" />
</rte:SfRichTextEditor.ToolbarItems>
```

### Note-Taking Toolbar

Simplified for quick notes:

```xml
<rte:SfRichTextEditor.ToolbarItems>
    <rte:RichTextToolbarItem Type="Bold" />
    <rte:RichTextToolbarItem Type="Italic" />
    <rte:RichTextToolbarItem Type="Separator" />
    <rte:RichTextToolbarItem Type="HighlightColor" />
    <rte:RichTextToolbarItem Type="Separator" />
    <rte:RichTextToolbarItem Type="NumberList" />
    <rte:RichTextToolbarItem Type="BulletList" />
    <rte:RichTextToolbarItem Type="Separator" />
    <rte:RichTextToolbarItem Type="Hyperlink" />
</rte:SfRichTextEditor.ToolbarItems>
```

## Inline Link Tooltip

When users click on a hyperlink in the editor content, a quick tooltip appears with link actions.

### Tooltip Features

The link quick tooltip provides three actions:
1. **Open** - Opens the link in default browser
2. **Edit Link** - Modifies the link URL or display text
3. **Remove Link** - Deletes the hyperlink (keeps text)

### Behavior

- Appears automatically when clicking a link in the editor
- Auto-dismisses after 2 seconds if no interaction
- Provides quick access without opening full dialogs

### Example Scenario

```xml
<rte:SfRichTextEditor ShowToolbar="True">
    <rte:SfRichTextEditor.ToolbarItems>
        <rte:RichTextToolbarItem Type="Hyperlink" />
    </rte:SfRichTextEditor.ToolbarItems>
</rte:SfRichTextEditor>
```

Users can:
1. Insert a link using the Hyperlink toolbar button
2. Click the inserted link to see the tooltip
3. Choose Open, Edit, or Remove from the tooltip

## Toolbar Appearance Customization

Customize the toolbar's visual style using the `ToolbarSettings` property.

### Available Customization Properties

The `RichTextEditorToolbarSettings` object provides these properties:

- **BackgroundColor** - Toolbar background color or brush
- **TextColor** - Color of toolbar item icons
- **SelectionColor** - Color for toolbar text hover and selection
- **IsScrollButtonVisible** - Show/hide scroll buttons for overflowing items
- **SeparatorColor** - Color of separator lines
- **SeparatorThickness** - Thickness of separator lines
- **ForwardIconBackground** - Background color of forward scroll icon
- **ForwardIconColor** - Color of forward scroll icon
- **BackwardIconBackground** - Background color of backward scroll icon
- **BackwardIconColor** - Color of backward scroll icon

### Basic Customization Example (XAML)

```xml
<rte:SfRichTextEditor ShowToolbar="True">
    <rte:SfRichTextEditor.ToolbarSettings>
        <rte:RichTextEditorToolbarSettings 
            BackgroundColor="LightGray"
            TextColor="Black"
            SelectionColor="DodgerBlue"
            SeparatorColor="Gray"
            SeparatorThickness="2" />
    </rte:SfRichTextEditor.ToolbarSettings>
</rte:SfRichTextEditor>
```

### Comprehensive Customization (XAML)

```xml
<rte:SfRichTextEditor ShowToolbar="True">
    <rte:SfRichTextEditor.ToolbarSettings>
        <rte:RichTextEditorToolbarSettings 
            BackgroundColor="SkyBlue"
            TextColor="Orange"
            SelectionColor="Brown"
            IsScrollButtonVisible="True"
            SeparatorColor="Brown"
            SeparatorThickness="5"
            ForwardIconBackground="Blue"
            ForwardIconColor="Green"
            BackwardIconBackground="Yellow"
            BackwardIconColor="Green" />
    </rte:SfRichTextEditor.ToolbarSettings>
</rte:SfRichTextEditor>
```

### C# Customization

```csharp
SfRichTextEditor richTextEditor = new SfRichTextEditor();
richTextEditor.ShowToolbar = true;

richTextEditor.ToolbarSettings = new RichTextEditorToolbarSettings
{
    BackgroundColor = Colors.LightGray,
    TextColor = Colors.Black,
    SelectionColor = Colors.DodgerBlue,
    IsScrollButtonVisible = true,
    SeparatorColor = Colors.Gray,
    SeparatorThickness = 2,
    ForwardIconBackground = Colors.Blue,
    ForwardIconColor = Colors.White,
    BackwardIconBackground = Colors.Blue,
    BackwardIconColor = Colors.White
};
```

### Dark Mode Toolbar

```csharp
richTextEditor.ToolbarSettings = new RichTextEditorToolbarSettings
{
    BackgroundColor = Color.FromRgb(30, 30, 30),
    TextColor = Colors.White,
    SelectionColor = Color.FromRgb(0, 120, 215),
    SeparatorColor = Color.FromRgb(60, 60, 60),
    SeparatorThickness = 1
};
```

### Branded Toolbar

```csharp
// Company brand colors
richTextEditor.ToolbarSettings = new RichTextEditorToolbarSettings
{
    BackgroundColor = Color.FromRgb(255, 87, 34), // Company orange
    TextColor = Colors.White,
    SelectionColor = Color.FromRgb(255, 152, 0), // Light orange
    SeparatorColor = Colors.White,
    SeparatorThickness = 1
};
```

## Platform-Specific Behaviors

### Windows and macOS

- Toolbar defaults to **Top** position
- Scroll buttons appear when toolbar items overflow
- Keyboard shortcuts work with toolbar actions (Ctrl+B for Bold, etc.)
- Font family picker shows system fonts

### Android and iOS

- Toolbar defaults to **Bottom** position
- Optimized for touch and thumb reach
- Toolbar items may scroll horizontally on smaller screens
- Native font picker integration

### Universal Considerations

```csharp
// Platform-specific toolbar position
#if ANDROID || IOS
    richTextEditor.ToolbarPosition = RichTextEditorToolbarPosition.Bottom;
#else
    richTextEditor.ToolbarPosition = RichTextEditorToolbarPosition.Top;
#endif
```

Or let the control use its default (automatically optimized per platform):

```csharp
// Don't set ToolbarPosition - uses platform default
SfRichTextEditor richTextEditor = new SfRichTextEditor
{
    ShowToolbar = true
};
```

## Best Practices

### 1. Use Separators for Visual Grouping

Group related items with separators for better usability:

```xml
<rte:RichTextToolbarItem Type="Bold" />
<rte:RichTextToolbarItem Type="Italic" />
<rte:RichTextToolbarItem Type="Underline" />
<rte:RichTextToolbarItem Type="Separator" />
<rte:RichTextToolbarItem Type="NumberList" />
<rte:RichTextToolbarItem Type="BulletList" />
```

### 2. Limit Toolbar Items on Mobile

For mobile apps, use fewer items to avoid horizontal scrolling:

```csharp
#if ANDROID || IOS
    // Mobile: minimal toolbar
    AddToolbarItems(richTextEditor, minimal: true);
#else
    // Desktop: full toolbar
    AddToolbarItems(richTextEditor, minimal: false);
#endif
```

### 3. Place Frequently Used Items First

Order items by usage frequency:
1. Bold, Italic, Underline (most common)
2. Lists and alignment
3. Links and media
4. Advanced formatting

### 4. Consider Your Use Case

Match toolbar to application purpose:
- **Email:** Focus on text formatting and links
- **Blog:** Include headings, images, tables
- **Notes:** Keep it minimal (bold, italic, lists, highlights)
- **CMS:** Full toolbar with all options

### 5. Customize Appearance to Match Theme

Align toolbar colors with app branding:

```csharp
// Match app's primary and secondary colors
richTextEditor.ToolbarSettings = new RichTextEditorToolbarSettings
{
    BackgroundColor = Application.Current.Resources["PrimaryColor"] as Color,
    TextColor = Application.Current.Resources["OnPrimaryColor"] as Color,
    SelectionColor = Application.Current.Resources["SecondaryColor"] as Color
};
```

### 6. Test Scroll Behavior

If your toolbar has many items:
- Enable scroll buttons: `IsScrollButtonVisible="True"`
- Or reduce items for specific platforms
- Test on smallest target screen size

### 7. Provide Keyboard Shortcuts

Supplement toolbar with keyboard shortcuts for power users:
- Bold: Ctrl+B / Cmd+B
- Italic: Ctrl+I / Cmd+I
- Underline: Ctrl+U / Cmd+U
- These work automatically with toolbar items

## Next Steps

- Learn about [formatting and customization](formatting-and-customization.md) for programmatic styling
- Explore [content management](content-management.md) for loading/saving content
- Implement [events](events-and-interactions.md) to respond to toolbar actions
