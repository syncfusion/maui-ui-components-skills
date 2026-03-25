---
name: syncfusion-maui-rich-text-editor
description: Implements Syncfusion .NET MAUI Rich Text Editor (SfRichTextEditor) for WYSIWYG text editing with formatting, images, tables, and hyperlinks. Use when building rich text editors, document editors, email composers, blog post editors, or messaging apps with formatting. Covers text styling, toolbar formatting, images, tables, hyperlinks, and HTML output.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion .NET MAUI Rich Text Editors

The Syncfusion .NET MAUI Rich Text Editor (SfRichTextEditor) provides a powerful WYSIWYG editing experience for creating richly formatted text content. It offers comprehensive formatting tools, customizable toolbar, image and table insertion, hyperlink management, and returns valid HTML markup. This skill guides you through implementing rich text editors for messaging apps, email composers, blog editors, note-taking applications, and any scenario requiring formatted text input.

## When to Use This Skill

Use this skill when you need to:

- **Implement WYSIWYG text editors** in .NET MAUI applications with formatting capabilities
- **Create email composers or messaging apps** that support rich text formatting (bold, italic, lists, etc.)
- **Build blog editors, CMS interfaces, or forum post editors** with HTML output
- **Add note-taking or document editing features** with formatting toolbar
- **Enable feedback forms, review sections, or comment areas** with rich content support
- **Implement text editors with images, tables, and hyperlinks** embedded in content
- **Customize toolbar items and appearance** for specific editing scenarios
- **Handle formatted text programmatically** with methods for bold, italic, alignment, colors, etc.
- **Manage user interactions** through events (text changes, hyperlink clicks, format changes)
- **Apply advanced features** like auto-sizing or Liquid Glass Effect for modern UI

## Component Overview

- **WYSIWYG Editing** - What-you-see-is-what-you-get interface for formatted text
- **Comprehensive Formatting** - Bold, italic, underline, strikethrough, fonts, colors, alignment, lists, indentation
- **Customizable Toolbar** - Rich, configurable toolbar with show/hide/custom items
- **Images & Tables** - Seamless insertion and formatting of images and tables
- **Hyperlink Support** - Insert, edit, and remove hyperlinks with click events
- **Programmatic Control** - Methods for applying formatting, managing content, and controlling editor state
- **Events** - FormatChanged, TextChanged, HyperlinkClicked, Focused/Unfocused for reactive UI
- **HTML Output** - Returns valid HTML markup for storage or transmission
- **AutoSize** - Dynamic height adjustment based on content
- **Liquid Glass Effect** - Modern Cupertino-style visual effects (iOS/macOS)

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installing the Syncfusion.Maui.RichTextEditor NuGet package
- Adding SfRichTextEditor to XAML and C#
- Namespace registration and basic setup
- Enabling the toolbar (ShowToolbar property)
- Customizing toolbar items with ToolbarItems collection
- First working example with formatted text

### Toolbar Configuration
📄 **Read:** [references/toolbar.md](references/toolbar.md)
- Positioning toolbar (top or bottom) with ToolbarPosition property
- Available toolbar items (Bold, Italic, FontFamily, FontSize, lists, images, tables, etc.)
- Adding or removing specific toolbar items
- Separator items for visual grouping
- Inline link tooltip functionality
- Customizing toolbar appearance with ToolbarSettings
- Background color, text color, separator styling
- Scroll button visibility and icon customization
- Platform-specific toolbar behaviors

### Formatting and Customization
📄 **Read:** [references/formatting-and-customization.md](references/formatting-and-customization.md)
- Customizing editor appearance (background, border, word wrap)
- Programmatic formatting methods (ToggleBold, ToggleItalic, ToggleUnderline, etc.)
- Character formatting (bold, italic, underline, strikethrough, subscript, superscript)
- List formatting (bullet and numbered lists)
- Text alignment (left, right, center, justify)
- Applying font family, font size, text color, and highlight color
- Paragraph formatting and heading styles
- Indentation control (increase/decrease)
- Default text styles for new content

### Content Management
📄 **Read:** [references/content-management.md](references/content-management.md)
- Setting plain text with Text property
- Setting HTML formatted text with HtmlText property
- Getting selected HTML content (GetSelectedText method)
- Configuring placeholder text and styling
- Programmatic cursor control (MoveCursorToStart, MoveCursorToEnd)
- Focus and unfocus methods
- Undo and redo functionality
- Content manipulation patterns and best practices

### Images and Tables
📄 **Read:** [references/images-and-tables.md](references/images-and-tables.md)
- Image insertion overview and ImageRequested event
- Inserting images from gallery, file picker, stream, or URI
- RichTextEditorImageSource configuration (format, size, source)
- Platform-specific permissions (MacCatalyst file access)
- Table insertion from toolbar with row/column dialog
- Programmatic table insertion with InsertTable method
- Specifying table dimensions
- Examples for both images and tables

### Hyperlinks
📄 **Read:** [references/hyperlinks.md](references/hyperlinks.md)
- Inserting hyperlinks with InsertHyperlink method
- Display text and URL parameters
- Editing existing hyperlinks with EditHyperlink method
- Removing hyperlinks while preserving text (RemoveHyperlink)
- Link quick tooltip behavior
- HyperlinkClicked event for handling link interactions
- Extracting link URL and display text from events
- Use cases and patterns for hyperlink management

### Events and Interactions
📄 **Read:** [references/events-and-interactions.md](references/events-and-interactions.md)
- FormatChanged event for tracking formatting state
- TextChanged event with old and new HTML content
- HyperlinkClicked event with URL and display text
- Focused and Unfocused events for input state
- Event subscription patterns in XAML and C#
- Using events for contextual UI updates
- Combining events with programmatic formatting
- Real-world event handling scenarios

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- AutoSize functionality with EnableAutoSize property
- Dynamic height adjustment based on content
- Layout considerations for auto-sizing editors
- Liquid Glass Effect with EnableLiquidGlassEffect (iOS/macOS)
- Platform requirements (.NET 10, iOS/macOS 26+)
- Cupertino theme integration and transparent backgrounds
- Customizing toolbar and editor corner radius
- Theme keys for visual customization
- Combining advanced features for modern UI

