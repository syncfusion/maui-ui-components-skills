# Content Management

## Table of Contents
- [Overview](#overview)
- [Setting Plain Text](#setting-plain-text)
- [Setting HTML Text](#setting-html-text)
- [Getting Selected Content](#getting-selected-content)
- [Placeholder Configuration](#placeholder-configuration)
- [Cursor Control](#cursor-control)
- [Focus Management](#focus-management)
- [Undo and Redo](#undo-and-redo)
- [Content Manipulation Patterns](#content-manipulation-patterns)
- [Best Practices](#best-practices)

## Overview

Content management in the Rich Text Editor involves loading, saving, manipulating, and navigating text content. The editor exposes a single public `Value` property for content access, paired with the `TValue` property of type `RichTextEditorValueType` that selects the underlying content model:

- `TValue="HTML"` (default) — `Value` holds an HTML string and is used for both plain and formatted text.
- `TValue="Schema"` — `Value` holds an `ObservableCollection<BlockNode>` representing the document structure.

The legacy `Text` and `HtmlText` properties are internal and not part of the public surface; use `Value` together with the appropriate `TValue` setting instead.

### Important: Cast the Value Property When Reading

The `Value` property is typed as `object` because it can hold either a `string` (HTML mode) or an `ObservableCollection<BlockNode>` (Schema mode). Always cast the value to the expected type before using it:

```csharp
// HTML mode — cast to string
string html = (string)richTextEditor.Value;

// Schema mode — cast to ObservableCollection<BlockNode>
var blocks = (ObservableCollection<BlockNode>)richTextEditor.Value;
```

If the cast fails at runtime, the editor's `TValue` was not set to the matching mode, or the underlying content type has been changed.

## Setting Plain Text

Use `Value` with `TValue="HTML"` to display or set plain, unformatted text. The editor stores the value as HTML under the hood, so the assigned string should be plain text without tags.

### XAML Usage

```xaml
<rte:SfRichTextEditor TValue="HTML"
                      Value="The rich text editor component is WYSIWYG editor that provides the best user experience to create and update the content" />
```

### C# Usage

```csharp
using Syncfusion.Maui.RichTextEditor;

SfRichTextEditor richTextEditor = new SfRichTextEditor
{
    TValue = RichTextEditorValueType.HTML
};
richTextEditor.Value = "The rich text editor component is WYSIWYG editor that provides the best user experience to create and update the content";
```

### Programmatic Text Loading

```csharp
public async Task LoadPlainTextFromFile(string filePath)
{
    try
    {
        string content = await File.ReadAllTextAsync(filePath);
        richTextEditor.Value = content;
    }
    catch (Exception ex)
    {
        // Handle error
        await DisplayAlert("Error", $"Failed to load file: {ex.Message}", "OK");
    }
}
```

### Getting Plain Text

```csharp
string value = (string)richTextEditor.Value;

// Save to file
await File.WriteAllTextAsync("output.txt", value);
```

### Use Cases for Plain Text

- Loading unformatted notes
- Simple text input scenarios
- Converting from plain text sources
- Stripping formatting from rich content

## Setting HTML Text

Use `Value` with `TValue="HTML"` (the default) to work with HTML formatted content.

### XAML Usage

```xaml
<rte:SfRichTextEditor TValue="HTML"
                      Value="The &lt;b&gt;rich text editor&lt;/b&gt; component is WYSIWYG editor that provides the best user experience to create and update the content" />
```

### C# Usage

```csharp
using Syncfusion.Maui.RichTextEditor;

SfRichTextEditor richTextEditor = new SfRichTextEditor
{
    TValue = RichTextEditorValueType.HTML
};
richTextEditor.Value = "The <b>rich text editor</b> component is WYSIWYG editor that provides the best user experience to create and update the content";
```

### Loading Formatted Content

```csharp
public void LoadFormattedContent()
{
    string htmlContent = @"
        <h1>Welcome to the Editor</h1>
        <p>This is a <b>bold</b> and <i>italic</i> text.</p>
        <ul>
            <li>First item</li>
            <li>Second item</li>
            <li>Third item</li>
        </ul>
        <p>Visit <a href='https://www.syncfusion.com'>Syncfusion</a> for more info.</p>
    ";

    richTextEditor.Value = htmlContent;
}
```

### Getting HTML Output

```csharp
string htmlContent = (string)richTextEditor.Value;

// Save to file
await File.WriteAllTextAsync("document.html", htmlContent);

// Send via API
await SendToServer(htmlContent);
```

### Email Composition Example

```csharp
public class EmailComposer
{
    private SfRichTextEditor editor;

    public EmailComposer()
    {
        editor = new SfRichTextEditor
        {
            TValue = RichTextEditorValueType.HTML,
            ShowToolbar = true,
            Placeholder = "Compose your email..."
        };
    }

    public async Task SendEmail(string to, string subject)
    {
        string htmlBody = (string)editor.Value;

        // Create email with HTML body
        var email = new EmailMessage
        {
            To = new List<string> { to },
            Subject = subject,
            BodyFormat = EmailBodyFormat.Html,
            Body = htmlBody
        };

        // Send email
        await SendEmailAsync(email);
    }
}
```

### Blog Post Editor Example

```csharp
public class BlogPostEditor
{
    private SfRichTextEditor editor;

    public async Task LoadPost(int postId)
    {
        // Fetch from database or API
        var post = await GetBlogPost(postId);
        editor.Value = post.Content;
    }

    public async Task SavePost(int postId)
    {
        string htmlContent = (string)editor.Value;

        // Save to database or API
        await UpdateBlogPost(postId, htmlContent);
    }
}
```

### HTML Validation

```csharp
public bool IsValidHtml(string html)
{
    try
    {
        richTextEditor.Value = html;
        return !string.IsNullOrEmpty((string)richTextEditor.Value);
    }
    catch
    {
        return false;
    }
}
```

## Getting Selected Content

Retrieve the HTML representation of the currently selected text.

### Method

```csharp
string selectedHtml = await richTextEditor.GetSelectedText();
```

### Usage Example

```csharp
public async Task CopySelectedContentToClipboard()
{
    string selectedText = await richTextEditor.GetSelectedText();

    if (!string.IsNullOrEmpty(selectedText))
    {
        await Clipboard.SetTextAsync(selectedText);
        await DisplayAlert("Copied", "Selected content copied to clipboard", "OK");
    }
    else
    {
        await DisplayAlert("Info", "No text selected", "OK");
    }
}
```

### Extract Selected for Processing

```csharp
public async Task<string> GetSelectedFormatted()
{
    string selectedHtml = await richTextEditor.GetSelectedText();

    // Process selected content
    if (!string.IsNullOrEmpty(selectedHtml))
    {
        // Apply transformations, translations, etc.
        return ProcessHtml(selectedHtml);
    }

    return string.Empty;
}
```

### Selection-Based Actions

```xaml
<StackLayout>
    <rte:SfRichTextEditor x:Name="richTextEditor" ShowToolbar="True" />

    <StackLayout Orientation="Horizontal" Spacing="10" Padding="10">
        <Button Text="Copy Selection" Clicked="OnCopySelection" />
        <Button Text="Delete Selection" Clicked="OnDeleteSelection" />
        <Button Text="Extract Selection" Clicked="OnExtractSelection" />
    </StackLayout>
</StackLayout>
```

```csharp
private async void OnCopySelection(object sender, EventArgs e)
{
    string selected = await richTextEditor.GetSelectedText();
    await Clipboard.SetTextAsync(selected);
}

private async void OnDeleteSelection(object sender, EventArgs e)
{
    string selected = await richTextEditor.GetSelectedText();
    if (!string.IsNullOrEmpty(selected))
    {
        // Clear selection by setting empty text
        // (Actual implementation may vary)
    }
}

private async void OnExtractSelection(object sender, EventArgs e)
{
    string selected = await richTextEditor.GetSelectedText();
    await DisplayAlert("Selection", selected, "OK");
}
```

## Placeholder Configuration

Display watermark text when the editor is empty.

### Properties

- **Placeholder** - The placeholder text to display
- **PlaceholderFontFamily** - Font family for placeholder
- **PlaceholderFontSize** - Font size for placeholder
- **PlaceholderColor** - Color of placeholder text

### XAML Configuration

```xaml
<rte:SfRichTextEditor Placeholder="Type your content here..."
                      PlaceholderFontFamily="Arial"
                      PlaceholderFontSize="14"
                      PlaceholderColor="Gray" />
```

### C# Configuration

```csharp
SfRichTextEditor richTextEditor = new SfRichTextEditor
{
    Placeholder = "Type your content here...",
    PlaceholderFontFamily = "Arial",
    PlaceholderFontSize = 14,
    PlaceholderColor = Colors.Gray
};
```

### Context-Specific Placeholders

```csharp
// Email composer
emailEditor.Placeholder = "Write your email message...";

// Note-taking
noteEditor.Placeholder = "Jot down your thoughts...";

// Blog editor
blogEditor.Placeholder = "Start writing your post...";

// Comment section
commentEditor.Placeholder = "Share your feedback...";

// Documentation
docsEditor.Placeholder = "Document your work here...";
```

### Styled Placeholder

```csharp
public void SetupStyledPlaceholder()
{
    richTextEditor.Placeholder = "Start typing your content here...";
    richTextEditor.PlaceholderFontFamily = "Georgia";
    richTextEditor.PlaceholderFontSize = 16;
    richTextEditor.PlaceholderColor = Color.FromRgb(150, 150, 150);
}
```

### Dynamic Placeholder

```csharp
public void UpdatePlaceholderByContext(string context)
{
    richTextEditor.Placeholder = context switch
    {
        "email" => "Compose your email...",
        "note" => "Write a note...",
        "comment" => "Add your comment...",
        "document" => "Create your document...",
        _ => "Start typing..."
    };
}
```

## Cursor Control

Programmatically control cursor position within the editor.

### Methods

- **MoveCursorToStart()** - Moves cursor to the beginning
- **MoveCursorToEnd()** - Moves cursor to the end

### Basic Usage

```csharp
// Move to start of content
richTextEditor.MoveCursorToStart();

// Move to end of content
richTextEditor.MoveCursorToEnd();
```

### Use Cases

**Append Content:**
```csharp
public void AppendSignature(string signature)
{
    // Move to end
    richTextEditor.MoveCursorToEnd();

    // Add signature HTML
    string currentHtml = (string)richTextEditor.Value;
    richTextEditor.Value = currentHtml + $"<br/><br/>{signature}";
}
```

**Prepend Header:**
```csharp
public void AddHeader(string header)
{
    // Move to start
    richTextEditor.MoveCursorToStart();

    // Add header
    string currentHtml = (string)richTextEditor.Value;
    richTextEditor.Value = $"<h2>{header}</h2><br/>{currentHtml}";
}
```

**Focus on Edit Area:**
```csharp
public void FocusForEditing()
{
    richTextEditor.Focus();
    richTextEditor.MoveCursorToEnd(); // Ready to type at end
}
```

## Focus Management

Control input focus programmatically.

### Methods

- **Focus()** - Sets focus to the editor
- **Unfocus()** - Removes focus from the editor

### Basic Usage

```csharp
// Set focus
richTextEditor.Focus();

// Remove focus
richTextEditor.Unfocus();
```

### Auto-Focus on Page Load

```csharp
public partial class EditorPage : ContentPage
{
    private SfRichTextEditor richTextEditor;

    public EditorPage()
    {
        InitializeComponent();

        // Auto-focus when page appears
        this.Appearing += OnPageAppearing;
    }

    private void OnPageAppearing(object sender, EventArgs e)
    {
        richTextEditor.Focus();
    }
}
```

### Validation-Based Focus

```csharp
public async Task<bool> ValidateAndSubmit()
{
    if (string.IsNullOrWhiteSpace((string)richTextEditor.Value))
    {
        await DisplayAlert("Validation", "Please enter content", "OK");
        richTextEditor.Focus(); // Focus for user to enter content
        return false;
    }

    richTextEditor.Unfocus(); // Remove focus before submission
    return true;
}
```

### Focus Management with Multiple Editors

```csharp
public class MultiEditorForm
{
    private SfRichTextEditor titleEditor;
    private SfRichTextEditor contentEditor;

    public void FocusNextEditor()
    {
        if (string.IsNullOrEmpty((string)titleEditor.Value))
        {
            titleEditor.Focus();
        }
        else
        {
            contentEditor.Focus();
        }
    }

    public void ClearAllFocus()
    {
        titleEditor.Unfocus();
        contentEditor.Unfocus();
    }
}
```

## Undo and Redo

Manage edit history programmatically.

### Methods

- **Undo()** - Reverts the last action
- **Redo()** - Re-applies the last undone action

### Basic Usage

```csharp
// Undo last action
richTextEditor.Undo();

// Redo last undone action
richTextEditor.Redo();
```

### Custom Undo/Redo Buttons

```xaml
<StackLayout Orientation="Horizontal" Spacing="5">
    <Button Text="↶ Undo" Clicked="OnUndoClicked" />
    <Button Text="↷ Redo" Clicked="OnRedoClicked" />
</StackLayout>

<rte:SfRichTextEditor x:Name="richTextEditor" ShowToolbar="False" />
```

```csharp
private void OnUndoClicked(object sender, EventArgs e)
{
    richTextEditor.Undo();
}

private void OnRedoClicked(object sender, EventArgs e)
{
    richTextEditor.Redo();
}
```

### Keyboard Shortcut Integration

```csharp
// Note: Undo/Redo keyboard shortcuts (Ctrl+Z, Ctrl+Y) are typically
// handled automatically by the Rich Text Editor control
// This is conceptual for custom implementations

public void HandleKeyboardShortcut(KeyEventArgs e)
{
    if (e.Key == Key.Z && e.Modifiers == KeyModifiers.Ctrl)
    {
        richTextEditor.Undo();
        e.Handled = true;
    }
    else if (e.Key == Key.Y && e.Modifiers == KeyModifiers.Ctrl)
    {
        richTextEditor.Redo();
        e.Handled = true;
    }
}
```

## Content Manipulation Patterns

### Auto-Save Pattern

```csharp
public class AutoSaveEditor
{
    private SfRichTextEditor editor;
    private System.Timers.Timer autoSaveTimer;

    public AutoSaveEditor()
    {
        editor = new SfRichTextEditor
        {
            TValue = RichTextEditorValueType.HTML
        };

        // Setup auto-save every 30 seconds
        autoSaveTimer = new System.Timers.Timer(30000);
        autoSaveTimer.Elapsed += OnAutoSave;
        autoSaveTimer.Start();
    }

    private async void OnAutoSave(object sender, System.Timers.ElapsedEventArgs e)
    {
        string content = (string)editor.Value;
        await SaveContent(content);
    }

    private async Task SaveContent(string html)
    {
        try
        {
            await File.WriteAllTextAsync("autosave.html", html);
        }
        catch (Exception ex)
        {
            // Handle save error
        }
    }
}
```

### Template Loading Pattern

```csharp
public class TemplateManager
{
    private SfRichTextEditor editor;
    private Dictionary<string, string> templates;

    public TemplateManager(SfRichTextEditor editor)
    {
        this.editor = editor;
        InitializeTemplates();
    }

    private void InitializeTemplates()
    {
        templates = new Dictionary<string, string>
        {
            ["email"] = "<p>Dear [Name],</p><p><br/></p><p>Best regards,<br/>[Your Name]</p>",
            ["memo"] = "<h2>Memo</h2><p><b>To:</b> [Recipient]</p><p><b>From:</b> [Sender]</p><p><b>Date:</b> [Date]</p><p><b>Subject:</b> [Subject]</p><p><br/></p>",
            ["report"] = "<h1>[Report Title]</h1><h2>Executive Summary</h2><p><br/></p><h2>Findings</h2><p><br/></p><h2>Recommendations</h2><p><br/></p>"
        };
    }

    public void LoadTemplate(string templateName)
    {
        if (templates.ContainsKey(templateName))
        {
            editor.Value = templates[templateName];
            editor.Focus();
        }
    }
}
```

### Content Sanitization Pattern

```csharp
public class ContentSanitizer
{
    public string SanitizeHtml(string rawHtml)
    {
        // Remove potentially dangerous tags
        string sanitized = rawHtml;

        // Remove script tags
        sanitized = System.Text.RegularExpressions.Regex.Replace(
            sanitized,
            @"<script[^>]*>.*?</script>",
            "",
            System.Text.RegularExpressions.RegexOptions.IgnoreCase |
            System.Text.RegularExpressions.RegexOptions.Singleline
        );

        // Remove event handlers
        sanitized = System.Text.RegularExpressions.Regex.Replace(
            sanitized,
            @"\s*on\w+\s*=\s*""[^""]*""",
            "",
            System.Text.RegularExpressions.RegexOptions.IgnoreCase
        );

        return sanitized;
    }

    public void LoadSafeContent(string untrustedHtml)
    {
        string safeHtml = SanitizeHtml(untrustedHtml);
        richTextEditor.Value = safeHtml;
    }
}
```

### Version History Pattern

```csharp
public class VersionHistory
{
    private List<string> history = new List<string>();
    private int currentVersion = -1;

    public void SaveVersion(string htmlContent)
    {
        // Remove any versions after current
        if (currentVersion < history.Count - 1)
        {
            history.RemoveRange(currentVersion + 1, history.Count - currentVersion - 1);
        }

        history.Add(htmlContent);
        currentVersion = history.Count - 1;
    }

    public string GetPreviousVersion()
    {
        if (currentVersion > 0)
        {
            currentVersion--;
            return history[currentVersion];
        }
        return null;
    }

    public string GetNextVersion()
    {
        if (currentVersion < history.Count - 1)
        {
            currentVersion++;
            return history[currentVersion];
        }
        return null;
    }
}
```

## Best Practices

### 1. Use HTML for Persistent Storage

Store formatted content as HTML for maximum fidelity:

```csharp
// Save
string htmlToStore = (string)richTextEditor.Value;
await SaveToDatabase(htmlToStore);

// Load
string htmlFromDatabase = await LoadFromDatabase();
richTextEditor.Value = htmlFromDatabase;
```

### 2. Validate Content Before Submission

```csharp
public async Task<bool> ValidateContent()
{
    if (string.IsNullOrWhiteSpace((string)richTextEditor.Value))
    {
        await DisplayAlert("Error", "Content cannot be empty", "OK");
        richTextEditor.Focus();
        return false;
    }

    if (((string)richTextEditor.Value).Length > 10000)
    {
        await DisplayAlert("Error", "Content exceeds maximum length", "OK");
        return false;
    }

    return true;
}
```

### 3. Handle Large Content Gracefully

```csharp
public async Task LoadLargeContent(string html)
{
    if (html.Length > 100000)
    {
        bool proceed = await DisplayAlert(
            "Large Content",
            "This document is very large and may take time to load. Continue?",
            "Yes",
            "No"
        );

        if (!proceed) return;
    }

    richTextEditor.Value = html;
}
```

### 4. Implement Auto-Save with Debouncing

```csharp
private System.Timers.Timer debounceTimer;

public void SetupAutoSave()
{
    richTextEditor.TextChanged += OnTextChangedForAutoSave;

    debounceTimer = new System.Timers.Timer(2000); // 2 second delay
    debounceTimer.AutoReset = false;
    debounceTimer.Elapsed += async (s, e) => await PerformAutoSave();
}

private void OnTextChangedForAutoSave(object sender, EventArgs e)
{
    debounceTimer.Stop();
    debounceTimer.Start();
}

private async Task PerformAutoSave()
{
    string content = (string)richTextEditor.Value;
    await SaveContentAsync(content);
}
```

### 5. Provide Clear Placeholder Guidance

```csharp
// Context-specific and helpful
richTextEditor.Placeholder = "Describe the issue in detail...";

// Not just generic
// richTextEditor.Placeholder = "Enter text";
```

### 6. Clear Content Safely

```csharp
public async Task ClearContent()
{
    if (!string.IsNullOrEmpty((string)richTextEditor.Value))
    {
        bool confirm = await DisplayAlert(
            "Confirm",
            "Clear all content?",
            "Yes",
            "No"
        );

        if (confirm)
        {
            richTextEditor.Value = string.Empty;
            richTextEditor.Focus();
        }
    }
}
```

### 7. Handle Focus on Mobile

```csharp
// On mobile, keyboard appearance can affect layout
#if ANDROID || IOS
    richTextEditor.Focused += (s, e) =>
    {
        // Adjust layout if needed when keyboard appears
    };

    richTextEditor.Unfocused += (s, e) =>
    {
        // Restore layout when keyboard dismisses
    };
#endif
```

## Next Steps

- Learn about [events](events-and-interactions.md) to track content changes
- Explore [images and tables](images-and-tables.md) for rich media content
- Implement [hyperlinks](hyperlinks.md) for interactive content
