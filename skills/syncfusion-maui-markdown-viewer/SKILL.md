---
name: syncfusion-maui-markdown-viewer
description: Implements Syncfusion .NET MAUI MarkdownViewer (SfMarkdownViewer) for rendering Markdown content with full formatting support. Use when displaying markdown files, documentation, release notes, or help content in MAUI apps. Covers markdown rendering, appearance customization, CSS styling, and content sources (string/file/URL/resource).
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion .NET MAUI MarkdownViewer

This skill guides you through implementing the SfMarkdownViewer control, a lightweight and efficient UI component for rendering Markdown content with full formatting support in .NET MAUI applications across mobile and desktop platforms.

## When to Use This Skill

Use this skill when you need to:
- Display Markdown-formatted content in a .NET MAUI application
- Render in-app documentation, user guides, or help content
- Show release notes, changelogs, or feature updates
- Present FAQs, troubleshooting guides, or support articles
- Load Markdown from strings, local files, embedded resources, or URLs
- Customize the appearance of rendered Markdown (fonts, colors, spacing)
- Apply custom CSS styling for advanced theming
- Retrieve or convert Markdown content programmatically (to HTML or plain text)
- Create documentation viewers, note-taking apps, or content portals

## Component Overview

**SfMarkdownViewer** provides:
- **Standard Markdown Support** — Renders headings, bold/italic text, lists, tables, images, code blocks, links, and more
- **Flexible Input Sources** — Load content from strings, `.md` files, embedded resources, or remote URLs
- **Appearance Customization** — Control fonts, colors, and spacing via `MarkdownStyleSettings`
- **Custom CSS Styling** — Apply advanced theming using raw CSS rules
- **Content Retrieval** — Access raw Markdown, convert to HTML, or extract plain text
- **Smooth Scrolling** — Fluid navigation through large documents across all platforms
- **Cross-Platform** — Consistent rendering on iOS, Android, Windows, and macOS

**Typical Use Cases:**
- In-app documentation and feature tours
- Release notes and version update displays
- Help sections and support portals
- Interactive content with links
- Styled content presentation

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

When you need to:
- Install the `Syncfusion.Maui.MarkdownViewer` NuGet package
- Register the Syncfusion handler in `MauiProgram.cs`
- Create your first SfMarkdownViewer control (XAML or C#)
- Add basic Markdown content to display

### Loading Content from Different Sources
📄 **Read:** [references/loading-content.md](references/loading-content.md)

When you need to:
- Load Markdown from a string variable
- Read Markdown from a local `.md` file
- Load embedded resource files from your project
- Fetch Markdown from a remote URL
- Handle async loading scenarios
- Choose the right content source for your use case

### Appearance Customization
📄 **Read:** [references/appearance-customization.md](references/appearance-customization.md)

When you need to:
- Customize heading sizes and colors (H1, H2, H3)
- Change body text font size and color
- Style table headers, data cells, and backgrounds
- Use the `MarkdownStyleSettings` class
- Match Markdown appearance with your app's theme
- Understand property-based styling approach

### Custom CSS Styling
📄 **Read:** [references/custom-css-styling.md](references/custom-css-styling.md)

When you need to:
- Apply advanced styling beyond basic properties
- Override default styles with custom CSS rules
- Style images (borders, shadows, rounded corners, sizing)
- Customize table appearance (striped rows, borders, padding)
- Control scrollbar visibility and styling
- Implement branded content designs
- Understand CSS precedence over properties

### Content Retrieval
📄 **Read:** [references/content-retrieval.md](references/content-retrieval.md)

When you need to:
- Retrieve the raw Markdown text programmatically
- Convert Markdown content to HTML format
- Extract plain text without formatting
- Use `GetMarkdownText()`, `GetHtmlText()`, or `GetText()` methods
- Process or transform Markdown content in code

### Troubleshooting
📄 **Read:** [references/troubleshooting.md](references/troubleshooting.md)

When you encounter:
- Handler not registered errors
- NuGet package installation issues
- Styles not applying correctly
- CSS not overriding properties
- Font size unit problems
- Image loading failures
- URL loading issues
- Cross-platform rendering differences

