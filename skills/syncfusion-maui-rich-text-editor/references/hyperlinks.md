# Hyperlinks

## Table of Contents
- [Overview](#overview)
- [Inserting Hyperlinks](#inserting-hyperlinks)
- [Editing Hyperlinks](#editing-hyperlinks)
- [Removing Hyperlinks](#removing-hyperlinks)
- [Link Quick Tooltip](#link-quick-tooltip)
- [HyperlinkClicked Event](#hyperlinkclicked-event)
- [Use Cases and Patterns](#use-cases-and-patterns)
- [Best Practices](#best-practices)

## Overview

The Rich Text Editor provides comprehensive hyperlink management capabilities. You can insert, edit, and remove hyperlinks programmatically, and handle user interactions when links are clicked within the editor content.

## Inserting Hyperlinks

### Method Signature

```csharp
void InsertHyperlink(string displayText, string url)
```

### Parameters

- **displayText** - The visible text for the hyperlink
- **url** - The target URL the hyperlink points to

### Basic Usage

```csharp
// Insert a hyperlink
richTextEditor.InsertHyperlink("Syncfusion", "https://www.syncfusion.com");

// Insert another hyperlink
richTextEditor.InsertHyperlink("Google", "https://www.google.com");

// Insert link with different text
richTextEditor.InsertHyperlink("Click here", "https://example.com");
```

### Insert Hyperlink with Button

```xml
<StackLayout>
    <Button Text="Insert Link" Clicked="OnInsertLinkClicked" />
    <rte:SfRichTextEditor x:Name="richTextEditor" ShowToolbar="True" />
</StackLayout>
```

```csharp
private async void OnInsertLinkClicked(object sender, EventArgs e)
{
    string displayText = await DisplayPromptAsync("Link Text", "Enter text to display:");
    if (string.IsNullOrEmpty(displayText)) return;
    
    string url = await DisplayPromptAsync("URL", "Enter URL:");
    if (string.IsNullOrEmpty(url)) return;
    
    richTextEditor.InsertHyperlink(displayText, url);
}
```

### Insert Hyperlink Dialog

```csharp
public async Task ShowInsertHyperlinkDialog()
{
    // Prompt for display text
    string displayText = await DisplayPromptAsync(
        "Insert Hyperlink",
        "Enter text to display:",
        placeholder: "Link text"
    );
    
    if (string.IsNullOrWhiteSpace(displayText))
        return;
    
    // Prompt for URL
    string url = await DisplayPromptAsync(
        "Insert Hyperlink",
        "Enter URL:",
        placeholder: "https://example.com"
    );
    
    if (string.IsNullOrWhiteSpace(url))
        return;
    
    // Validate URL
    if (!url.StartsWith("http://") && !url.StartsWith("https://"))
    {
        url = "https://" + url;
    }
    
    // Insert hyperlink
    richTextEditor.InsertHyperlink(displayText, url);
}
```

### Insert from Clipboard

```csharp
public async Task InsertHyperlinkFromClipboard()
{
    // Get URL from clipboard
    string clipboardText = await Clipboard.GetTextAsync();
    
    if (Uri.TryCreate(clipboardText, UriKind.Absolute, out Uri uri))
    {
        // Valid URL in clipboard
        string displayText = await DisplayPromptAsync(
            "Insert Link",
            "Enter display text:",
            initialValue: uri.Host
        );
        
        if (!string.IsNullOrEmpty(displayText))
        {
            richTextEditor.InsertHyperlink(displayText, clipboardText);
        }
    }
    else
    {
        await DisplayAlert("Invalid URL", "Clipboard does not contain a valid URL", "OK");
    }
}
```

### Common Link Types

```csharp
public class CommonLinks
{
    private SfRichTextEditor editor;
    
    public CommonLinks(SfRichTextEditor editor)
    {
        this.editor = editor;
    }
    
    public void InsertWebLink(string text, string url)
    {
        editor.InsertHyperlink(text, url);
    }
    
    public void InsertEmailLink(string email)
    {
        editor.InsertHyperlink(email, $"mailto:{email}");
    }
    
    public void InsertPhoneLink(string phoneNumber)
    {
        // Remove formatting for tel: link
        string telNumber = phoneNumber.Replace(" ", "").Replace("-", "").Replace("(", "").Replace(")", "");
        editor.InsertHyperlink(phoneNumber, $"tel:{telNumber}");
    }
    
    public void InsertSmsLink(string phoneNumber)
    {
        string telNumber = phoneNumber.Replace(" ", "").Replace("-", "").Replace("(", "").Replace(")", "");
        editor.InsertHyperlink($"SMS {phoneNumber}", $"sms:{telNumber}");
    }
}

// Usage
var commonLinks = new CommonLinks(richTextEditor);
commonLinks.InsertWebLink("Visit our site", "https://example.com");
commonLinks.InsertEmailLink("support@example.com");
commonLinks.InsertPhoneLink("+1 (555) 123-4567");
commonLinks.InsertSmsLink("+1 (555) 123-4567");
```

## Editing Hyperlinks

### Method Signature

```csharp
void EditHyperlink(string text, string oldUrl, string newUrl)
```

### Parameters

- **text** - The display text of the hyperlink to edit
- **oldUrl** - The current URL of the hyperlink
- **newUrl** - The new URL to replace the old one

### Basic Usage

```csharp
// Edit an existing hyperlink's URL
richTextEditor.EditHyperlink("Example", "https://example.com", "https://www.google.com");

// Update company link
richTextEditor.EditHyperlink("Syncfusion", "https://syncfusion.com", "https://www.syncfusion.com");
```

### Edit Hyperlink Dialog

```csharp
public async Task ShowEditHyperlinkDialog(string currentText, string currentUrl)
{
    // Prompt for new URL
    string newUrl = await DisplayPromptAsync(
        "Edit Hyperlink",
        "Enter new URL:",
        initialValue: currentUrl
    );
    
    if (string.IsNullOrWhiteSpace(newUrl) || newUrl == currentUrl)
        return;
    
    // Validate URL
    if (!newUrl.StartsWith("http://") && !newUrl.StartsWith("https://"))
    {
        newUrl = "https://" + newUrl;
    }
    
    // Edit hyperlink
    richTextEditor.EditHyperlink(currentText, currentUrl, newUrl);
}
```

### Update Link Target

```csharp
public void UpdateLinkTarget(string displayText, string oldTarget, string newTarget)
{
    try
    {
        richTextEditor.EditHyperlink(displayText, oldTarget, newTarget);
    }
    catch (Exception ex)
    {
        DisplayAlert("Error", $"Failed to update link: {ex.Message}", "OK");
    }
}
```

## Removing Hyperlinks

### Method Signature

```csharp
void RemoveHyperlink(string text, string url)
```

### Parameters

- **text** - The display text of the hyperlink to remove
- **url** - The URL of the hyperlink to remove

### Basic Usage

```csharp
// Remove a hyperlink (text remains as plain text)
richTextEditor.RemoveHyperlink("Example", "https://www.google.com");

// Remove another hyperlink
richTextEditor.RemoveHyperlink("Syncfusion", "https://www.syncfusion.com");
```

**Note:** Removing a hyperlink preserves the display text as plain text; only the link is removed.

### Remove with Confirmation

```csharp
public async Task RemoveHyperlinkWithConfirmation(string text, string url)
{
    bool confirm = await DisplayAlert(
        "Remove Link",
        $"Remove hyperlink from '{text}'?",
        "Yes",
        "No"
    );
    
    if (confirm)
    {
        richTextEditor.RemoveHyperlink(text, url);
    }
}
```

### Batch Remove Links

```csharp
public class LinkManager
{
    private SfRichTextEditor editor;
    private List<(string text, string url)> trackedLinks = new List<(string, string)>();
    
    public LinkManager(SfRichTextEditor editor)
    {
        this.editor = editor;
    }
    
    public void TrackLink(string text, string url)
    {
        trackedLinks.Add((text, url));
    }
    
    public void RemoveAllTrackedLinks()
    {
        foreach (var (text, url) in trackedLinks)
        {
            editor.RemoveHyperlink(text, url);
        }
        trackedLinks.Clear();
    }
    
    public void RemoveLinksByDomain(string domain)
    {
        var linksToRemove = trackedLinks.Where(l => l.url.Contains(domain)).ToList();
        
        foreach (var (text, url) in linksToRemove)
        {
            editor.RemoveHyperlink(text, url);
            trackedLinks.Remove((text, url));
        }
    }
}
```

## Link Quick Tooltip

### Overview

When users click on a hyperlink within the editor content, a quick tooltip automatically appears with three actions:

1. **Open** - Opens the link in the default browser
2. **Edit Link** - Allows modifying the link URL or display text
3. **Remove Link** - Deletes the hyperlink (preserves text)

### Behavior

- Appears automatically when clicking any link in the editor
- Auto-dismisses after 2 seconds if no interaction
- Provides quick access without full dialogs

### Enabling Link Tooltip

The link quick tooltip is enabled automatically when you include the Hyperlink toolbar item:

```xml
<rte:SfRichTextEditor ShowToolbar="True">
    <rte:SfRichTextEditor.ToolbarItems>
        <rte:RichTextToolbarItem Type="Hyperlink" />
    </rte:SfRichTextEditor.ToolbarItems>
</rte:SfRichTextEditor>
```

## HyperlinkClicked Event

### Overview

The `HyperlinkClicked` event fires when a user taps a hyperlink within the editor content. Use this event to handle link navigation, logging, or custom actions.

### Event Signature

```csharp
void HyperlinkClicked(object sender, RichTextEditorHyperlinkClickedEventArgs e)
```

### Event Arguments

- **URL** - The target URL of the clicked hyperlink
- **DisplayText** - The visible text of the hyperlink

### XAML Subscription

```xml
<rte:SfRichTextEditor x:Name="richTextEditor"
                      ShowToolbar="True"
                      HyperlinkClicked="OnHyperlinkClicked" />
```

### C# Subscription

```csharp
richTextEditor.HyperlinkClicked += OnHyperlinkClicked;

private void OnHyperlinkClicked(object sender, RichTextEditorHyperlinkClickedEventArgs e)
{
    string url = e.URL;
    string displayText = e.DisplayText;
    
    // Handle link click
    DisplayAlert("Link Clicked", $"Text: {displayText}\nURL: {url}", "OK");
}
```

### Open Link in Browser

```csharp
private async void OnHyperlinkClicked(object sender, RichTextEditorHyperlinkClickedEventArgs e)
{
    string url = e.URL;
    
    try
    {
        await Launcher.OpenAsync(url);
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", $"Failed to open link: {ex.Message}", "OK");
    }
}
```

### Confirm Before Opening

```csharp
private async void OnHyperlinkClicked(object sender, RichTextEditorHyperlinkClickedEventArgs e)
{
    string url = e.URL;
    string displayText = e.DisplayText;
    
    bool shouldOpen = await DisplayAlert(
        "Open Link",
        $"Open '{displayText}'?\n\n{url}",
        "Open",
        "Cancel"
    );
    
    if (shouldOpen)
    {
        await Launcher.OpenAsync(url);
    }
}
```

### Log Link Clicks

```csharp
private void OnHyperlinkClicked(object sender, RichTextEditorHyperlinkClickedEventArgs e)
{
    string url = e.URL;
    string displayText = e.DisplayText;
    
    // Log the click
    LogLinkClick(url, displayText, DateTime.Now);
    
    // Open the link
    Launcher.OpenAsync(url);
}

private void LogLinkClick(string url, string text, DateTime timestamp)
{
    // Save to database, analytics, etc.
    Debug.WriteLine($"[{timestamp}] Link clicked: {text} -> {url}");
}
```

### Handle Special Link Types

```csharp
private async void OnHyperlinkClicked(object sender, RichTextEditorHyperlinkClickedEventArgs e)
{
    string url = e.URL;
    
    if (url.StartsWith("mailto:"))
    {
        // Handle email link
        await HandleEmailLink(url);
    }
    else if (url.StartsWith("tel:"))
    {
        // Handle phone link
        await HandlePhoneLink(url);
    }
    else if (url.StartsWith("http://") || url.StartsWith("https://"))
    {
        // Handle web link
        await Launcher.OpenAsync(url);
    }
    else
    {
        await DisplayAlert("Unsupported", "This link type is not supported", "OK");
    }
}

private async Task HandleEmailLink(string mailtoUrl)
{
    string email = mailtoUrl.Replace("mailto:", "");
    
    var message = new EmailMessage
    {
        To = new List<string> { email }
    };
    
    await Email.ComposeAsync(message);
}

private async Task HandlePhoneLink(string telUrl)
{
    string phoneNumber = telUrl.Replace("tel:", "");
    
    try
    {
        PhoneDialer.Open(phoneNumber);
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", $"Failed to dial: {ex.Message}", "OK");
    }
}
```

## Use Cases and Patterns

### Reference Links in Documentation

```csharp
public class DocumentationEditor
{
    private SfRichTextEditor editor;
    
    public void InsertApiReference(string className, string apiUrl)
    {
        editor.InsertHyperlink($"{className} API", apiUrl);
    }
    
    public void InsertExternalReference(string title, string url)
    {
        editor.InsertHyperlink($"📖 {title}", url);
    }
}
```

### Social Media Links

```csharp
public void AddSocialMediaLinks()
{
    richTextEditor.InsertHyperlink("Follow us on Twitter", "https://twitter.com/yourcompany");
    richTextEditor.InsertHyperlink("Like us on Facebook", "https://facebook.com/yourcompany");
    richTextEditor.InsertHyperlink("Connect on LinkedIn", "https://linkedin.com/company/yourcompany");
}
```

### Email Signature with Links

```csharp
public string CreateEmailSignature(string name, string email, string website)
{
    richTextEditor.HtmlText = $@"
        <p><b>{name}</b><br/>
        Software Engineer<br/>";
    
    richTextEditor.InsertHyperlink(email, $"mailto:{email}");
    richTextEditor.HtmlText += "<br/>";
    richTextEditor.InsertHyperlink(website, website);
    richTextEditor.HtmlText += "</p>";
    
    return richTextEditor.HtmlText;
}
```

### Footer with Legal Links

```csharp
public void InsertFooterLinks()
{
    richTextEditor.HtmlText = "<p><small>";
    richTextEditor.InsertHyperlink("Privacy Policy", "https://example.com/privacy");
    richTextEditor.HtmlText += " | ";
    richTextEditor.InsertHyperlink("Terms of Service", "https://example.com/terms");
    richTextEditor.HtmlText += " | ";
    richTextEditor.InsertHyperlink("Contact Us", "https://example.com/contact");
    richTextEditor.HtmlText += "</small></p>";
}
```

### Internal Navigation Links

```csharp
public void InsertInternalLink(string sectionName, string anchor)
{
    richTextEditor.InsertHyperlink($"Go to {sectionName}", $"#{anchor}");
}

// Usage
InsertInternalLink("Introduction", "intro");
InsertInternalLink("Getting Started", "getting-started");
InsertInternalLink("Conclusion", "conclusion");
```

## Best Practices

### 1. Validate URLs Before Inserting

```csharp
public async Task<bool> InsertValidatedHyperlink(string text, string url)
{
    if (!Uri.TryCreate(url, UriKind.Absolute, out Uri validUri))
    {
        await DisplayAlert("Invalid URL", "Please enter a valid URL", "OK");
        return false;
    }
    
    richTextEditor.InsertHyperlink(text, url);
    return true;
}
```

### 2. Auto-Add Protocol

```csharp
public void InsertHyperlinkWithProtocol(string text, string url)
{
    if (!url.StartsWith("http://") && !url.StartsWith("https://"))
    {
        url = "https://" + url;
    }
    
    richTextEditor.InsertHyperlink(text, url);
}
```

### 3. Provide Descriptive Link Text

```csharp
// ✓ Good
richTextEditor.InsertHyperlink("Download the user manual", "https://example.com/manual.pdf");

// ✗ Avoid
richTextEditor.InsertHyperlink("Click here", "https://example.com/manual.pdf");
```

### 4. Handle External Links Safely

```csharp
private async void OnHyperlinkClicked(object sender, RichTextEditorHyperlinkClickedEventArgs e)
{
    string url = e.URL;
    
    // Warn for external links
    if (url.StartsWith("http") && !url.Contains("yourcompany.com"))
    {
        bool proceed = await DisplayAlert(
            "External Link",
            "This will open an external website. Continue?",
            "Yes",
            "No"
        );
        
        if (!proceed) return;
    }
    
    await Launcher.OpenAsync(url);
}
```

### 5. Track Link Usage

```csharp
public class LinkAnalytics
{
    private Dictionary<string, int> linkClicks = new Dictionary<string, int>();
    
    public void RecordClick(string url)
    {
        if (linkClicks.ContainsKey(url))
            linkClicks[url]++;
        else
            linkClicks[url] = 1;
    }
    
    public string GetMostClickedLink()
    {
        return linkClicks.OrderByDescending(kv => kv.Value).FirstOrDefault().Key;
    }
}
```

### 6. Provide Link Preview

```csharp
private async void OnHyperlinkClicked(object sender, RichTextEditorHyperlinkClickedEventArgs e)
{
    string url = e.URL;
    string displayText = e.DisplayText;
    
    // Show link preview before opening
    string action = await DisplayActionSheet(
        $"{displayText}\n{url}",
        "Cancel",
        null,
        "Open in Browser",
        "Copy Link",
        "Edit Link"
    );
    
    switch (action)
    {
        case "Open in Browser":
            await Launcher.OpenAsync(url);
            break;
        case "Copy Link":
            await Clipboard.SetTextAsync(url);
            break;
        case "Edit Link":
            await ShowEditHyperlinkDialog(displayText, url);
            break;
    }
}
```

### 7. Support Keyboard Shortcuts

```csharp
// Conceptual - actual implementation depends on platform
public void SetupLinkShortcut()
{
    // Ctrl+K / Cmd+K typically triggers link insertion
    // This is often handled automatically by the Rich Text Editor
}
```

### 8. Test Link Accessibility

Ensure links are accessible:
- Use descriptive text (not "click here")
- Ensure sufficient color contrast
- Test with screen readers
- Support keyboard navigation

## Next Steps

- Explore [events and interactions](events-and-interactions.md) for more event handling
- Learn about [content management](content-management.md) for saving HTML with links
- Implement [images and tables](images-and-tables.md) to complement hyperlinks
