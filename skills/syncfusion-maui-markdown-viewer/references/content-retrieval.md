# Content Retrieval Methods

This guide covers the methods available for programmatically retrieving and transforming Markdown content from SfMarkdownViewer.

## Overview

SfMarkdownViewer provides three built-in methods to access the displayed Markdown content in different formats:

| Method | Return Type | Use Case |
|--------|-------------|----------|
| `GetMarkdownText()` | `string` | Retrieve original Markdown source |
| `GetHtmlText()` | `string` | Convert Markdown to HTML |
| `GetText()` | `string` | Extract plain text without formatting |

These methods allow you to:
- Export content in different formats
- Process or transform Markdown programmatically
- Extract text for search/indexing
- Save content to files or databases
- Share content in various formats

## GetMarkdownText()

Retrieves the raw Markdown content exactly as assigned to the `Source` property.

### Basic Usage

```csharp
using Syncfusion.Maui.MarkdownViewer;

public partial class MainPage : ContentPage
{
    private SfMarkdownViewer markdownViewer;
    
    public MainPage()
    {
        InitializeComponent();
        
        markdownViewer = new SfMarkdownViewer();
        markdownViewer.Source = "# Hello\n\nThis is **Markdown** content.";
        Content = markdownViewer;
    }
    
    private void OnGetMarkdownClicked(object sender, EventArgs e)
    {
        string markdown = markdownViewer.GetMarkdownText();
        // Result: "# Hello\n\nThis is **Markdown** content."
        
        DisplayAlert("Markdown", markdown, "OK");
    }
}
```

### Use Cases

**1. Save to File**
```csharp
public async Task SaveMarkdownAsync()
{
    string markdown = markdownViewer.GetMarkdownText();
    string filePath = Path.Combine(FileSystem.AppDataDirectory, "document.md");
    await File.WriteAllTextAsync(filePath, markdown);
}
```

**2. Copy to Clipboard**
```csharp
public async Task CopyMarkdownAsync()
{
    string markdown = markdownViewer.GetMarkdownText();
    await Clipboard.SetTextAsync(markdown);
    await DisplayAlert("Success", "Markdown copied to clipboard", "OK");
}
```

**3. Share Content**
```csharp
public async Task ShareMarkdownAsync()
{
    string markdown = markdownViewer.GetMarkdownText();
    await Share.RequestAsync(new ShareTextRequest
    {
        Text = markdown,
        Title = "Share Markdown"
    });
}
```

**4. Validate Content**
```csharp
public bool HasContent()
{
    string markdown = markdownViewer.GetMarkdownText();
    return !string.IsNullOrWhiteSpace(markdown);
}

public int GetWordCount()
{
    string markdown = markdownViewer.GetMarkdownText();
    return markdown.Split(new[] { ' ', '\n', '\r' }, StringSplitOptions.RemoveEmptyEntries).Length;
}
```

**5. Version Control**
```csharp
public class DocumentVersion
{
    public string Content { get; set; }
    public DateTime Timestamp { get; set; }
}

private List<DocumentVersion> _versions = new();

public void SaveVersion()
{
    string markdown = markdownViewer.GetMarkdownText();
    _versions.Add(new DocumentVersion
    {
        Content = markdown,
        Timestamp = DateTime.Now
    });
}

public void RestoreVersion(int index)
{
    if (index >= 0 && index < _versions.Count)
    {
        markdownViewer.Source = _versions[index].Content;
    }
}
```

## GetHtmlText()

Converts the Markdown content to HTML format, preserving all formatting and structure.

### Basic Usage

```csharp
public partial class MainPage : ContentPage
{
    private SfMarkdownViewer markdownViewer;
    
    public MainPage()
    {
        InitializeComponent();
        
        markdownViewer = new SfMarkdownViewer();
        markdownViewer.Source = @"
# Welcome

This is **bold** and this is *italic*.

- Item 1
- Item 2
";
        Content = markdownViewer;
    }
    
    private void OnGetHtmlClicked(object sender, EventArgs e)
    {
        string html = markdownViewer.GetHtmlText();
        /* Result:
        <h1>Welcome</h1>
        <p>This is <strong>bold</strong> and this is <em>italic</em>.</p>
        <ul>
        <li>Item 1</li>
        <li>Item 2</li>
        </ul>
        */
        
        DisplayAlert("HTML", html, "OK");
    }
}
```

### Use Cases

**1. Export to HTML File**
```csharp
public async Task ExportToHtmlAsync()
{
    string html = markdownViewer.GetHtmlText();
    
    // Add HTML wrapper
    string fullHtml = $@"
<!DOCTYPE html>
<html>
<head>
    <meta charset=""UTF-8"">
    <meta name=""viewport"" content=""width=device-width, initial-scale=1.0"">
    <title>Exported Document</title>
    <style>
        body {{
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 40px auto;
            padding: 20px;
        }}
        h1 {{ color: #2C3E50; }}
        h2 {{ color: #34495E; }}
        code {{ background: #F4F4F4; padding: 2px 6px; border-radius: 4px; }}
        table {{ border-collapse: collapse; width: 100%; }}
        th, td {{ border: 1px solid #DDD; padding: 12px; text-align: left; }}
        th {{ background: #F2F2F2; }}
    </style>
</head>
<body>
{html}
</body>
</html>";
    
    string filePath = Path.Combine(FileSystem.AppDataDirectory, "document.html");
    await File.WriteAllTextAsync(filePath, fullHtml);
    
    await DisplayAlert("Success", $"HTML saved to: {filePath}", "OK");
}
```

**2. Email Content**
```csharp
public async Task EmailHtmlAsync()
{
    string html = markdownViewer.GetHtmlText();
    
    var message = new EmailMessage
    {
        Subject = "Document",
        Body = html,
        BodyFormat = EmailBodyFormat.Html,
        To = new List<string> { "recipient@example.com" }
    };
    
    try
    {
        await Email.ComposeAsync(message);
    }
    catch (FeatureNotSupportedException)
    {
        await DisplayAlert("Error", "Email is not supported on this device", "OK");
    }
}
```

**3. Display in WebView**
```csharp
public void ShowInWebView()
{
    string html = markdownViewer.GetHtmlText();
    
    // Create a new page with WebView
    var webViewPage = new ContentPage();
    var webView = new WebView
    {
        Source = new HtmlWebViewSource
        {
            Html = $@"
<!DOCTYPE html>
<html>
<head>
    <meta name=""viewport"" content=""width=device-width, initial-scale=1.0"">
    <style>
        body {{ padding: 20px; font-family: sans-serif; }}
    </style>
</head>
<body>{html}</body>
</html>"
        }
    };
    webViewPage.Content = webView;
    
    Navigation.PushAsync(webViewPage);
}
```

**4. Generate PDF (with additional library)**
```csharp
// Requires a PDF library like Syncfusion.Pdf or similar
public async Task ExportToPdfAsync()
{
    string html = markdownViewer.GetHtmlText();
    
    // Use your PDF library to convert HTML to PDF
    // Example with a hypothetical PDF converter:
    // byte[] pdfBytes = HtmlToPdfConverter.Convert(html);
    // string filePath = Path.Combine(FileSystem.AppDataDirectory, "document.pdf");
    // await File.WriteAllBytesAsync(filePath, pdfBytes);
}
```

**5. Rich Text Editor Integration**
```csharp
public void OpenInRichTextEditor()
{
    string html = markdownViewer.GetHtmlText();
    
    // If you have a rich text editor that accepts HTML
    // richTextEditor.HtmlContent = html;
}
```

## GetText()

Extracts plain text content, removing all Markdown formatting (headings, bold, italic, links, etc.).

### Basic Usage

```csharp
public partial class MainPage : ContentPage
{
    private SfMarkdownViewer markdownViewer;
    
    public MainPage()
    {
        InitializeComponent();
        
        markdownViewer = new SfMarkdownViewer();
        markdownViewer.Source = @"
# Welcome

This is **bold** and this is *italic*.";
        Content = markdownViewer;
    }
    
    private void OnGetTextClicked(object sender, EventArgs e)
    {
        string plainText = markdownViewer.GetText();
        /* Result:
        Welcome
        
        This is bold and this is italic.
        
        Visit our site
        */
        
        DisplayAlert("Plain Text", plainText, "OK");
    }
}
```

### Use Cases

**1. Search Indexing**
```csharp
public class SearchIndex
{
    private Dictionary<string, string> _index = new();
    
    public void IndexDocument(string documentId, SfMarkdownViewer viewer)
    {
        string plainText = viewer.GetText();
        _index[documentId] = plainText.ToLower();
    }
    
    public List<string> Search(string query)
    {
        query = query.ToLower();
        return _index
            .Where(kvp => kvp.Value.Contains(query))
            .Select(kvp => kvp.Key)
            .ToList();
    }
}
```

**2. Text-to-Speech**
```csharp
public async Task ReadAloudAsync()
{
    string plainText = markdownViewer.GetText();
    
    try
    {
        await TextToSpeech.SpeakAsync(plainText);
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", $"Text-to-speech failed: {ex.Message}", "OK");
    }
}
```

**3. Word/Character Count**
```csharp
public class DocumentStatistics
{
    public int WordCount { get; set; }
    public int CharacterCount { get; set; }
    public int CharacterCountNoSpaces { get; set; }
    public int ParagraphCount { get; set; }
    public int SentenceCount { get; set; }
}

public DocumentStatistics GetStatistics()
{
    string text = markdownViewer.GetText();
    
    return new DocumentStatistics
    {
        WordCount = text.Split(new[] { ' ', '\n', '\r', '\t' }, 
            StringSplitOptions.RemoveEmptyEntries).Length,
        CharacterCount = text.Length,
        CharacterCountNoSpaces = text.Replace(" ", "").Replace("\n", "").Replace("\r", "").Length,
        ParagraphCount = text.Split(new[] { "\n\n" }, StringSplitOptions.RemoveEmptyEntries).Length,
        SentenceCount = text.Split(new[] { '.', '!', '?' }, 
            StringSplitOptions.RemoveEmptyEntries).Length
    };
}
```

**4. SMS/Short Message Export**
```csharp
public async Task SendAsSmsAsync()
{
    string plainText = markdownViewer.GetText();
    
    // Truncate if needed
    if (plainText.Length > 160)
    {
        plainText = plainText.Substring(0, 157) + "...";
    }
    
    try
    {
        var message = new SmsMessage
        {
            Body = plainText
        };
        await Sms.ComposeAsync(message);
    }
    catch (FeatureNotSupportedException)
    {
        await DisplayAlert("Error", "SMS is not supported on this device", "OK");
    }
}
```

**5. Translation**
```csharp
public async Task TranslateContentAsync(string targetLanguage)
{
    string plainText = markdownViewer.GetText();
    
    // Use a translation API/service
    // string translated = await TranslationService.TranslateAsync(plainText, targetLanguage);
    
    // Display translated text
    // translatedViewer.Source = translated;
}
```

**6. Spell Check**
```csharp
public List<string> GetMisspelledWords()
{
    string text = markdownViewer.GetText();
    string[] words = text.Split(new[] { ' ', '\n', '\r', '\t', '.', ',', '!', '?' }, 
        StringSplitOptions.RemoveEmptyEntries);
    
    var misspelled = new List<string>();
    foreach (var word in words)
    {
        // Use your spell check library
        // if (!SpellChecker.IsCorrect(word))
        // {
        //     misspelled.Add(word);
        // }
    }
    
    return misspelled;
}
```

## Complete Example: Multi-Format Export

Here's a comprehensive example that exports content in all three formats:

```csharp
public partial class ExportPage : ContentPage
{
    private SfMarkdownViewer markdownViewer;
    
    public ExportPage()
    {
        InitializeComponent();
        
        markdownViewer = new SfMarkdownViewer();
        markdownViewer.Source = @"
# Sample Document

This is a **sample** document with *formatting*.

## Features
- Bullet points
- **Bold text**
- *Italic text*

## Table

| Column 1 | Column 2 |
|----------|----------|
| Data 1   | Data 2   |
| Data 3   | Data 4   |
";
        
        var exportMarkdownButton = new Button { Text = "Export as Markdown" };
        exportMarkdownButton.Clicked += OnExportMarkdownClicked;
        
        var exportHtmlButton = new Button { Text = "Export as HTML" };
        exportHtmlButton.Clicked += OnExportHtmlClicked;
        
        var exportTextButton = new Button { Text = "Export as Plain Text" };
        exportTextButton.Clicked += OnExportTextClicked;
        
        var exportAllButton = new Button { Text = "Export All Formats" };
        exportAllButton.Clicked += OnExportAllClicked;
        
        Content = new StackLayout
        {
            Children =
            {
                markdownViewer,
                exportMarkdownButton,
                exportHtmlButton,
                exportTextButton,
                exportAllButton
            }
        };
    }
    
    private async void OnExportMarkdownClicked(object sender, EventArgs e)
    {
        string markdown = markdownViewer.GetMarkdownText();
        string filePath = Path.Combine(FileSystem.AppDataDirectory, "export.md");
        await File.WriteAllTextAsync(filePath, markdown);
        await DisplayAlert("Success", $"Markdown saved to:\n{filePath}", "OK");
    }
    
    private async void OnExportHtmlClicked(object sender, EventArgs e)
    {
        string html = markdownViewer.GetHtmlText();
        string fullHtml = $@"
<!DOCTYPE html>
<html>
<head>
    <meta charset=""UTF-8"">
    <title>Exported Document</title>
    <style>
        body {{ font-family: Arial, sans-serif; padding: 20px; max-width: 800px; margin: auto; }}
        h1 {{ color: #2C3E50; }}
        table {{ border-collapse: collapse; width: 100%; }}
        th, td {{ border: 1px solid #DDD; padding: 8px; }}
    </style>
</head>
<body>
{html}
</body>
</html>";
        
        string filePath = Path.Combine(FileSystem.AppDataDirectory, "export.html");
        await File.WriteAllTextAsync(filePath, fullHtml);
        await DisplayAlert("Success", $"HTML saved to:\n{filePath}", "OK");
    }
    
    private async void OnExportTextClicked(object sender, EventArgs e)
    {
        string text = markdownViewer.GetText();
        string filePath = Path.Combine(FileSystem.AppDataDirectory, "export.txt");
        await File.WriteAllTextAsync(filePath, text);
        await DisplayAlert("Success", $"Plain text saved to:\n{filePath}", "OK");
    }
    
    private async void OnExportAllClicked(object sender, EventArgs e)
    {
        try
        {
            string baseDir = FileSystem.AppDataDirectory;
            
            // Export Markdown
            string markdown = markdownViewer.GetMarkdownText();
            await File.WriteAllTextAsync(Path.Combine(baseDir, "export.md"), markdown);
            
            // Export HTML
            string html = markdownViewer.GetHtmlText();
            string fullHtml = $@"
<!DOCTYPE html>
<html>
<head>
    <meta charset=""UTF-8"">
    <title>Exported Document</title>
</head>
<body>
{html}
</body>
</html>";
            await File.WriteAllTextAsync(Path.Combine(baseDir, "export.html"), fullHtml);
            
            // Export Plain Text
            string text = markdownViewer.GetText();
            await File.WriteAllTextAsync(Path.Combine(baseDir, "export.txt"), text);
            
            await DisplayAlert("Success", 
                $"All formats exported to:\n{baseDir}\n\n- export.md\n- export.html\n- export.txt", 
                "OK");
        }
        catch (Exception ex)
        {
            await DisplayAlert("Error", $"Export failed: {ex.Message}", "OK");
        }
    }
}
```

## Best Practices

### 1. Check for Content Before Retrieving

```csharp
public string SafeGetMarkdown()
{
    string markdown = markdownViewer.GetMarkdownText();
    return string.IsNullOrWhiteSpace(markdown) ? "# Empty Document" : markdown;
}
```

### 2. Handle Large Content

```csharp
public async Task ExportLargeDocumentAsync()
{
    // For very large documents, consider async processing
    string markdown = await Task.Run(() => markdownViewer.GetMarkdownText());
    await File.WriteAllTextAsync("large-document.md", markdown);
}
```

### 3. Provide User Feedback

```csharp
public async Task ExportWithFeedbackAsync()
{
    var indicator = new ActivityIndicator { IsRunning = true };
    // Show indicator to user
    
    try
    {
        string html = markdownViewer.GetHtmlText();
        await File.WriteAllTextAsync("document.html", html);
        await DisplayAlert("Success", "Document exported successfully", "OK");
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", $"Export failed: {ex.Message}", "OK");
    }
    finally
    {
        indicator.IsRunning = false;
    }
}
```

### 4. Sanitize Output for External Use

```csharp
public string GetSafeHtml()
{
    string html = markdownViewer.GetHtmlText();
    // Sanitize if needed for security
    // html = HtmlSanitizer.Sanitize(html);
    return html;
}
```

## Common Scenarios

### Scenario 1: Save and Restore

```csharp
public async Task SaveDocumentAsync(string fileName)
{
    string markdown = markdownViewer.GetMarkdownText();
    string filePath = Path.Combine(FileSystem.AppDataDirectory, fileName);
    await File.WriteAllTextAsync(filePath, markdown);
}

public async Task LoadDocumentAsync(string fileName)
{
    string filePath = Path.Combine(FileSystem.AppDataDirectory, fileName);
    if (File.Exists(filePath))
    {
        string markdown = await File.ReadAllTextAsync(filePath);
        markdownViewer.Source = markdown;
    }
}
```

### Scenario 2: Content Comparison

```csharp
public bool HasContentChanged(string originalMarkdown)
{
    string currentMarkdown = markdownViewer.GetMarkdownText();
    return !string.Equals(originalMarkdown, currentMarkdown, StringComparison.Ordinal);
}
```

### Scenario 3: Content Analysis

```csharp
public Dictionary<string, int> AnalyzeContent()
{
    string text = markdownViewer.GetText();
    
    return new Dictionary<string, int>
    {
        ["Words"] = text.Split(' ', StringSplitOptions.RemoveEmptyEntries).Length,
        ["Characters"] = text.Length,
        ["Lines"] = text.Split('\n').Length
    };
}
```