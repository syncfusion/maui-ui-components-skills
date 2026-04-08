# Loading Markdown Content from Different Sources

## Table of Contents
- [Overview](#overview)
- [From String](#from-string)
- [From Local File](#from-local-file)
- [From Embedded Resource](#from-embedded-resource)
- [From URL](#from-url)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

The SfMarkdownViewer control supports loading Markdown content from multiple sources, giving you flexibility in how you structure and deliver content in your application. Each source type has specific use cases and implementation patterns.

### Source Types Comparison

| Source Type | Use Case | Pros | Cons |
|-------------|----------|------|------|
| **String** | Dynamic content, user input | Fast, no I/O | Memory overhead for large content |
| **Local File** | User documents, local storage | Easy to update, familiar file system | Platform-specific paths |
| **Embedded Resource** | App documentation, static content | Bundled with app, no external dependencies | Increases app size |
| **URL** | Remote docs, changelogs | Always up-to-date, centralized | Requires network, slower, can fail |

## From String

The simplest approach is to assign a Markdown-formatted string directly to the `Source` property.

### XAML Implementation

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:markdown="clr-namespace:Syncfusion.Maui.MarkdownViewer;assembly=Syncfusion.Maui.MarkdownViewer"
             x:Class="MyApp.MainPage">

    <markdown:SfMarkdownViewer>
        <markdown:SfMarkdownViewer.Source>
            <x:String>
                <![CDATA[
# My Document Title

This is **bold** text and this is *italic* text.

## Section 1
Content for section 1.

## Section 2
Content for section 2.

### Subsection
- List item 1
- List item 2
- List item 3
                ]]>
            </x:String>
        </markdown:SfMarkdownViewer.Source>
    </markdown:SfMarkdownViewer>

</ContentPage>
```

**Key Points:**
- Use `<![CDATA[...]]>` in XAML to avoid XML parsing issues with Markdown special characters
- Preserve line breaks for proper Markdown rendering
- Watch for indentation — excessive indentation may be interpreted as code blocks

### C# Implementation

```csharp
using Syncfusion.Maui.MarkdownViewer;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        SfMarkdownViewer markdownViewer = new SfMarkdownViewer();
        
        // Option 1: Inline string
        markdownViewer.Source = "# Hello\n\nThis is **Markdown** content.";
        
        // Option 2: Verbatim string (recommended for multiline)
        markdownViewer.Source = @"
# Welcome

This is a **multiline** Markdown document.

## Features
- Feature 1
- Feature 2
- Feature 3
";
        
        Content = markdownViewer;
    }
}
```

### Dynamic Content with Data Binding

```csharp
// ViewModel
public class DocumentViewModel : INotifyPropertyChanged
{
    private string _markdownContent;
    
    public string MarkdownContent
    {
        get => _markdownContent;
        set
        {
            _markdownContent = value;
            OnPropertyChanged();
        }
    }
    
    public void LoadContent()
    {
        MarkdownContent = @"
# Dynamic Document

Content loaded at: " + DateTime.Now.ToString("g") + @"

This content can be updated dynamically.
";
    }
    
    // INotifyPropertyChanged implementation...
}
```

```xml
<!-- XAML with binding -->
<markdown:SfMarkdownViewer Source="{Binding MarkdownContent}" />
```

### Use Cases
- Displaying user-generated content
- Building Markdown strings dynamically
- Real-time preview while editing
- Localized content strings

## From Local File

Load Markdown content from `.md` files stored on the device's file system.

### Basic Implementation

```csharp
using Syncfusion.Maui.MarkdownViewer;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        SfMarkdownViewer markdownViewer = new SfMarkdownViewer();
        
        // Path to local file
        string filePath = @"C:\Documents\MyDocument.md";
        
        // Read and assign content
        if (File.Exists(filePath))
        {
            string markdownContent = File.ReadAllText(filePath);
            markdownViewer.Source = markdownContent;
        }
        else
        {
            markdownViewer.Source = "# Error\n\nFile not found.";
        }
        
        Content = markdownViewer;
    }
}
```

### Cross-Platform File Paths

Use `FileSystem` API for platform-agnostic file access:

```csharp
public async Task LoadFromAppDataAsync()
{
    string fileName = "document.md";
    string filePath = Path.Combine(FileSystem.AppDataDirectory, fileName);
    
    if (File.Exists(filePath))
    {
        string content = await File.ReadAllTextAsync(filePath);
        markdownViewer.Source = content;
    }
}
```

### Platform-Specific Paths

```csharp
public string GetPlatformPath()
{
    // Windows
    #if WINDOWS
    return @"C:\Users\Username\Documents\file.md";
    #endif
    
    // Android
    #if ANDROID
    return "/storage/emulated/0/Documents/file.md";
    #endif
    
    // iOS/macOS
    #if IOS || MACCATALYST
    return Path.Combine(
        Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments), 
        "file.md"
    );
    #endif
    
    return string.Empty;
}
```

**Note:** These paths work with .NET 9 MAUI on all supported platforms.

### File Picker Integration

```csharp
public async Task PickAndLoadFileAsync()
{
    try
    {
        var result = await FilePicker.PickAsync(new PickOptions
        {
            FileTypes = new FilePickerFileType(
                new Dictionary<DevicePlatform, IEnumerable<string>>
                {
                    { DevicePlatform.iOS, new[] { "public.text", "md" } },
                    { DevicePlatform.Android, new[] { "text/*" } },
                    { DevicePlatform.WinUI, new[] { ".md", ".markdown", ".txt" } },
                    { DevicePlatform.macOS, new[] { "md", "markdown", "txt" } }
                })
        });
        
        if (result != null)
        {
            using var stream = await result.OpenReadAsync();
            using var reader = new StreamReader(stream);
            string content = await reader.ReadToEndAsync();
            markdownViewer.Source = content;
        }
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", $"Unable to load file: {ex.Message}", "OK");
    }
}
```

### Use Cases
- Opening user documents
- Loading saved notes
- Importing Markdown files
- Document editing applications

## From Embedded Resource

Embed `.md` files in your project and load them as resources. This is ideal for static content like documentation or help files.

### Step 1: Add File to Project

1. Add your `.md` file to the `Resources` folder (or any subfolder)
2. Ensure the file's build action is set appropriately for MAUI

In `.csproj`, MAUI automatically treats files in `Resources/Raw/` as raw assets:

```xml
<ItemGroup>
    <MauiAsset Include="Resources\Raw\**" />
</ItemGroup>
```

Or explicitly define:

```xml
<ItemGroup>
    <MauiAsset Include="Resources\UserGuide.md" />
</ItemGroup>
```

### Step 2: Load the Embedded Resource

```csharp
using System.Text;
using Syncfusion.Maui.MarkdownViewer;

public partial class MainPage : ContentPage
{
    private SfMarkdownViewer markdownViewer;
    
    public MainPage()
    {
        InitializeComponent();
        
        markdownViewer = new SfMarkdownViewer();
        _ = LoadEmbeddedResourceAsync();
        Content = markdownViewer;
    }
    
    private async Task LoadEmbeddedResourceAsync()
    {
        try
        {
            // Load from Resources/Raw folder
            using Stream stream = await FileSystem.OpenAppPackageFileAsync("UserGuide.md");
            using StreamReader reader = new StreamReader(stream, Encoding.UTF8);
            string markdownContent = await reader.ReadToEndAsync();
            
            markdownViewer.Source = markdownContent;
        }
        catch (Exception ex)
        {
            markdownViewer.Source = $"# Error\n\nFailed to load resource: {ex.Message}";
        }
    }
}
```

### Loading from Subfolders

If your file is in a subfolder like `Resources/Raw/Docs/UserGuide.md`:

```csharp
using Stream stream = await FileSystem.OpenAppPackageFileAsync("Docs/UserGuide.md");
```

### Multiple Resource Files

```csharp
public async Task LoadDocumentAsync(string documentName)
{
    string fileName = $"{documentName}.md";
    
    try
    {
        using Stream stream = await FileSystem.OpenAppPackageFileAsync(fileName);
        using StreamReader reader = new StreamReader(stream, Encoding.UTF8);
        string content = await reader.ReadToEndAsync();
        markdownViewer.Source = content;
    }
    catch (FileNotFoundException)
    {
        markdownViewer.Source = $"# Document Not Found\n\nThe document '{documentName}' could not be loaded.";
    }
}

// Usage
await LoadDocumentAsync("GettingStarted");
await LoadDocumentAsync("FAQ");
await LoadDocumentAsync("Troubleshooting");
```

### Use Cases
- Built-in app documentation
- Help files and tutorials
- License agreements
- Static content that ships with the app
- Offline-first applications

## From URL

Load Markdown content from a remote web server or repository. Content is fetched over HTTP/HTTPS.

### Basic URL Loading

```xml
<!-- Direct URL in XAML -->
<markdown:SfMarkdownViewer 
    Source="https://raw.githubusercontent.com/SyncfusionExamples/GettingStarted_DockLayout_MAUI/refs/heads/master/README.md" />
```

```csharp
// Direct URL in C#
markdownViewer.Source = "https://raw.githubusercontent.com/SyncfusionExamples/GettingStarted_DockLayout_MAUI/refs/heads/master/README.md";
```

### Advanced URL Loading with Error Handling

```csharp
public async Task LoadFromUrlAsync(string url)
{
    try
    {
        using HttpClient client = new HttpClient();
        client.Timeout = TimeSpan.FromSeconds(30);
        
        string content = await client.GetStringAsync(url);
        markdownViewer.Source = content;
    }
    catch (HttpRequestException ex)
    {
        markdownViewer.Source = $"# Network Error\n\nFailed to load content: {ex.Message}";
    }
    catch (TaskCanceledException)
    {
        markdownViewer.Source = "# Timeout\n\nThe request took too long to complete.";
    }
}
```

### Loading with Progress Indicator

```csharp
public async Task LoadUrlWithProgressAsync(string url)
{
    // Show loading indicator
    loadingIndicator.IsVisible = true;
    markdownViewer.IsVisible = false;
    
    try
    {
        using HttpClient client = new HttpClient();
        string content = await client.GetStringAsync(url);
        
        markdownViewer.Source = content;
        markdownViewer.IsVisible = true;
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", $"Failed to load: {ex.Message}", "OK");
    }
    finally
    {
        loadingIndicator.IsVisible = false;
    }
}
```

### Caching Remote Content

```csharp
private Dictionary<string, string> _contentCache = new();

public async Task LoadWithCacheAsync(string url)
{
    // Check cache first
    if (_contentCache.TryGetValue(url, out string cachedContent))
    {
        markdownViewer.Source = cachedContent;
        return;
    }
    
    // Load from URL
    try
    {
        using HttpClient client = new HttpClient();
        string content = await client.GetStringAsync(url);
        
        // Store in cache
        _contentCache[url] = content;
        
        markdownViewer.Source = content;
    }
    catch (Exception ex)
    {
        markdownViewer.Source = $"# Error\n\n{ex.Message}";
    }
}
```

### Common URL Patterns

```csharp
// GitHub raw content
string githubUrl = "https://raw.githubusercontent.com/dotnet/docs-maui/refs/heads/main/docs/what-is-maui.md";

```

### Use Cases
- Displaying remote documentation
- Loading changelogs from repository
- Showing release notes
- Content management systems
- Real-time content updates

## Best Practices

### 1. Use Async Loading for Files and URLs

```csharp
// ✅ Good - async loading
private async Task LoadContentAsync()
{
    string content = await File.ReadAllTextAsync(filePath);
    markdownViewer.Source = content;
}

// ❌ Bad - blocking the UI thread
private void LoadContent()
{
    string content = File.ReadAllText(filePath);
    markdownViewer.Source = content;
}
```

### 2. Handle Encoding Properly

```csharp
// Always specify UTF-8 for Markdown files
string content = await File.ReadAllTextAsync(filePath, Encoding.UTF8);

// For streams
using StreamReader reader = new StreamReader(stream, Encoding.UTF8);
```

### 3. Implement Error Handling

```csharp
try
{
    string content = await LoadContentAsync();
    markdownViewer.Source = content;
}
catch (FileNotFoundException)
{
    markdownViewer.Source = "# File Not Found\n\nThe requested file does not exist.";
}
catch (UnauthorizedAccessException)
{
    markdownViewer.Source = "# Access Denied\n\nYou don't have permission to access this file.";
}
catch (Exception ex)
{
    markdownViewer.Source = $"# Error\n\n{ex.Message}";
}
```

### 4. Provide User Feedback

```csharp
public async Task LoadWithFeedbackAsync(string source)
{
    statusLabel.Text = "Loading...";
    
    try
    {
        string content = await LoadFromSourceAsync(source);
        markdownViewer.Source = content;
        statusLabel.Text = "Loaded successfully";
    }
    catch (Exception ex)
    {
        statusLabel.Text = $"Error: {ex.Message}";
    }
}
```

### 5. Validate Content Before Loading

```csharp
public bool IsValidMarkdown(string content)
{
    return !string.IsNullOrWhiteSpace(content) && content.Length > 0;
}

public async Task SafeLoadAsync(string filePath)
{
    string content = await File.ReadAllTextAsync(filePath);
    
    if (IsValidMarkdown(content))
    {
        markdownViewer.Source = content;
    }
    else
    {
        markdownViewer.Source = "# Empty Document\n\nNo content to display.";
    }
}
```

## Troubleshooting

### Issue: File Not Found

**Symptom:** Exception when loading from local file or embedded resource

**Solutions:**
- Verify the file path is correct and case-sensitive (especially on Android/iOS)
- For embedded resources, ensure build action is set correctly
- Check that file exists in the expected location
- Use `File.Exists()` before attempting to read

### Issue: Network Timeout

**Symptom:** Loading from URL takes too long or fails

**Solutions:**
- Increase the timeout: `client.Timeout = TimeSpan.FromSeconds(60);`
- Implement retry logic
- Cache content for offline access
- Show loading indicator to user

### Issue: Content Not Rendering

**Symptom:** MarkdownViewer shows blank even after loading

**Solutions:**
- Check that `Source` property is actually set
- Verify content is valid Markdown
- Ensure content is not an empty string
- Check for exceptions in loading code

### Issue: Special Characters Display Incorrectly

**Symptom:** Non-ASCII characters show as boxes or incorrect symbols

**Solutions:**
- Always use `Encoding.UTF8` when reading files
- Ensure source files are saved with UTF-8 encoding
- For HTTP requests, check `Content-Type` header for encoding

### Issue: XAML CDATA Parsing Error

**Symptom:** Build error or runtime exception with inline XAML content

**Solutions:**
- Ensure `<![CDATA[` and `]]>` are properly placed
- Don't include `]]>` in your Markdown content (escape if needed)
- Consider loading from code-behind instead for complex content
