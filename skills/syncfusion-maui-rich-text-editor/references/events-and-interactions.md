# Events and Interactions

## Table of Contents
- [Overview](#overview)
- [FormatChanged Event](#formatchanged-event)
- [TextChanged Event](#textchanged-event)
- [HyperlinkClicked Event](#hyperlinkclicked-event)
- [Focused and Unfocused Events](#focused-and-unfocused-events)
- [Event Subscription Patterns](#event-subscription-patterns)
- [Real-World Scenarios](#real-world-scenarios)
- [Best Practices](#best-practices)

## Overview

The Rich Text Editor provides several events to track user interactions and content changes. These events enable reactive UI updates, auto-save functionality, validation, analytics, and custom workflows based on editor state.

## FormatChanged Event

### Overview

The `FormatChanged` event fires when the formatting status changes in the editor. This occurs when:
- User selects text with different formatting
- Cursor moves to text with different formatting
- Formatting is applied or removed

### Event Signature

```csharp
void FormatChanged(object sender, RichTextEditorFormatChangedEventArgs e)
```

### XAML Subscription

```xml
<rte:SfRichTextEditor x:Name="richTextEditor"
                      ShowToolbar="True"
                      FormatChanged="OnFormatChanged" />
```

### C# Subscription

```csharp
richTextEditor.FormatChanged += OnFormatChanged;

private void OnFormatChanged(object sender, RichTextEditorFormatChangedEventArgs e)
{
    // Handle format changes
    Debug.WriteLine("Format changed");
}
```

### Use Cases

**Update Custom Toolbar UI:**
```csharp
private void OnFormatChanged(object sender, RichTextEditorFormatChangedEventArgs e)
{
    // Update custom formatting buttons to reflect current state
    // (Event arguments contain current formatting information)
    UpdateFormattingButtons();
}

private void UpdateFormattingButtons()
{
    // Update bold button state
    // Update italic button state
    // Update underline button state
    // etc.
}
```

**Track Formatting Usage:**
```csharp
private Dictionary<string, int> formatUsage = new Dictionary<string, int>();

private void OnFormatChanged(object sender, RichTextEditorFormatChangedEventArgs e)
{
    // Track which formats are used most
    string formatType = "format_type"; // Extract from event args
    
    if (formatUsage.ContainsKey(formatType))
        formatUsage[formatType]++;
    else
        formatUsage[formatType] = 1;
}
```

**Contextual Formatting Hints:**
```csharp
private void OnFormatChanged(object sender, RichTextEditorFormatChangedEventArgs e)
{
    // Show formatting hints based on current selection
    // For example: "Heading 1 selected"
    UpdateFormatStatusLabel();
}
```

## TextChanged Event

### Overview

The `TextChanged` event fires whenever the content in the editor changes. This includes:
- User typing or deleting text
- Pasting content
- Programmatic content changes
- Formatting changes that affect HTML structure

### Event Signature

```csharp
void TextChanged(object sender, RichTextEditorTextChangedEventArgs e)
```

### Event Arguments

- **OldText** - HTML content before the change
- **NewText** - HTML content after the change

### XAML Subscription

```xml
<rte:SfRichTextEditor x:Name="richTextEditor"
                      ShowToolbar="True"
                      TextChanged="OnTextChanged" />
```

### C# Subscription

```csharp
richTextEditor.TextChanged += OnTextChanged;

private void OnTextChanged(object sender, RichTextEditorTextChangedEventArgs e)
{
    string oldHtml = e.OldText;
    string newHtml = e.NewText;
    
    // Handle text changes
    Debug.WriteLine($"Old: {oldHtml}");
    Debug.WriteLine($"New: {newHtml}");
}
```

### Auto-Save Implementation

```csharp
private System.Timers.Timer autoSaveTimer;
private bool hasUnsavedChanges = false;

public void SetupAutoSave()
{
    richTextEditor.TextChanged += OnTextChangedForAutoSave;
    
    // Auto-save every 30 seconds
    autoSaveTimer = new System.Timers.Timer(30000);
    autoSaveTimer.Elapsed += async (s, e) => await PerformAutoSave();
    autoSaveTimer.Start();
}

private void OnTextChangedForAutoSave(object sender, RichTextEditorTextChangedEventArgs e)
{
    hasUnsavedChanges = true;
}

private async Task PerformAutoSave()
{
    if (hasUnsavedChanges)
    {
        string content = richTextEditor.HtmlText;
        await SaveContentAsync(content);
        hasUnsavedChanges = false;
        
        MainThread.BeginInvokeOnMainThread(() =>
        {
            DisplayAlert("Auto-Saved", "Your changes have been saved", "OK");
        });
    }
}
```

### Character Count Tracker

```csharp
private Label characterCountLabel;

private void OnTextChanged(object sender, RichTextEditorTextChangedEventArgs e)
{
    string plainText = richTextEditor.Text;
    int charCount = plainText?.Length ?? 0;
    
    characterCountLabel.Text = $"Characters: {charCount}";
}
```

### Word Count Tracker

```csharp
private Label wordCountLabel;

private void OnTextChanged(object sender, RichTextEditorTextChangedEventArgs e)
{
    string plainText = richTextEditor.Text;
    
    if (string.IsNullOrWhiteSpace(plainText))
    {
        wordCountLabel.Text = "Words: 0";
        return;
    }
    
    int wordCount = plainText.Split(new[] { ' ', '\n', '\r', '\t' }, 
        StringSplitOptions.RemoveEmptyEntries).Length;
    
    wordCountLabel.Text = $"Words: {wordCount}";
}
```

### Content Validation

```csharp
private const int MaxCharacters = 5000;

private void OnTextChanged(object sender, RichTextEditorTextChangedEventArgs e)
{
    string plainText = richTextEditor.Text;
    int charCount = plainText?.Length ?? 0;
    
    if (charCount > MaxCharacters)
    {
        warningLabel.Text = $"Warning: Character limit exceeded ({charCount}/{MaxCharacters})";
        warningLabel.TextColor = Colors.Red;
        warningLabel.IsVisible = true;
    }
    else
    {
        warningLabel.IsVisible = false;
    }
}
```

### Undo/Redo State Management

```csharp
private Stack<string> undoStack = new Stack<string>();
private Stack<string> redoStack = new Stack<string>();

private void OnTextChanged(object sender, RichTextEditorTextChangedEventArgs e)
{
    // Save old state for undo
    if (!string.IsNullOrEmpty(e.OldText))
    {
        undoStack.Push(e.OldText);
    }
    
    // Clear redo stack on new change
    redoStack.Clear();
    
    UpdateUndoRedoButtons();
}

private void UpdateUndoRedoButtons()
{
    undoButton.IsEnabled = undoStack.Count > 0;
    redoButton.IsEnabled = redoStack.Count > 0;
}
```

### Content Change Logging

```csharp
private void OnTextChanged(object sender, RichTextEditorTextChangedEventArgs e)
{
    string oldHtml = e.OldText;
    string newHtml = e.NewText;
    
    // Log changes for audit trail
    LogContentChange(DateTime.Now, oldHtml, newHtml);
}

private void LogContentChange(DateTime timestamp, string oldContent, string newContent)
{
    // Save to database or file for audit purposes
    Debug.WriteLine($"[{timestamp}] Content changed");
}
```

## HyperlinkClicked Event

### Overview

The `HyperlinkClicked` event fires when a user taps a hyperlink within the editor content.

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

private async void OnHyperlinkClicked(object sender, RichTextEditorHyperlinkClickedEventArgs e)
{
    string url = e.URL;
    string displayText = e.DisplayText;
    
    // Open link
    await Launcher.OpenAsync(url);
}
```

### Custom Link Handling

```csharp
private async void OnHyperlinkClicked(object sender, RichTextEditorHyperlinkClickedEventArgs e)
{
    string url = e.URL;
    string displayText = e.DisplayText;
    
    // Confirm before opening external links
    bool shouldOpen = await DisplayAlert(
        "Open Link",
        $"Open '{displayText}'?",
        "Yes",
        "No"
    );
    
    if (shouldOpen)
    {
        try
        {
            await Launcher.OpenAsync(url);
        }
        catch (Exception ex)
        {
            await DisplayAlert("Error", $"Failed to open link: {ex.Message}", "OK");
        }
    }
}
```

### Link Analytics

```csharp
private async void OnHyperlinkClicked(object sender, RichTextEditorHyperlinkClickedEventArgs e)
{
    string url = e.URL;
    string displayText = e.DisplayText;
    
    // Track link clicks for analytics
    await TrackLinkClick(url, displayText);
    
    // Open link
    await Launcher.OpenAsync(url);
}

private async Task TrackLinkClick(string url, string text)
{
    // Send to analytics service
    await AnalyticsService.TrackEvent("link_clicked", new Dictionary<string, string>
    {
        ["url"] = url,
        ["text"] = text,
        ["timestamp"] = DateTime.Now.ToString()
    });
}
```

For more hyperlink examples, see [hyperlinks.md](hyperlinks.md).

## Focused and Unfocused Events

### Overview

These events fire when the editor gains or loses input focus.

### Focused Event

Fires when the editor receives focus.

**XAML:**
```xml
<rte:SfRichTextEditor x:Name="richTextEditor"
                      Focused="OnEditorFocused" />
```

**C#:**
```csharp
richTextEditor.Focused += OnEditorFocused;

private void OnEditorFocused(object sender, EventArgs e)
{
    // Handle editor focused
    Debug.WriteLine("Editor focused");
}
```

### Unfocused Event

Fires when the editor loses focus.

**XAML:**
```xml
<rte:SfRichTextEditor x:Name="richTextEditor"
                      Unfocused="OnEditorUnfocused" />
```

**C#:**
```csharp
richTextEditor.Unfocused += OnEditorUnfocused;

private void OnEditorUnfocused(object sender, EventArgs e)
{
    // Handle editor unfocused
    Debug.WriteLine("Editor unfocused");
}
```

### Focus-Based Auto-Save

```csharp
private async void OnEditorUnfocused(object sender, EventArgs e)
{
    // Save content when user leaves the editor
    string content = richTextEditor.HtmlText;
    await SaveContentAsync(content);
}
```

### Focus Indication

```csharp
private Border editorBorder;

private void OnEditorFocused(object sender, EventArgs e)
{
    // Highlight border when focused
    editorBorder.BorderColor = Colors.Blue;
    editorBorder.BorderThickness = 2;
}

private void OnEditorUnfocused(object sender, EventArgs e)
{
    // Reset border when unfocused
    editorBorder.BorderColor = Colors.LightGray;
    editorBorder.BorderThickness = 1;
}
```

### Keyboard Management

```csharp
private void OnEditorFocused(object sender, EventArgs e)
{
    // Adjust layout for keyboard on mobile
#if ANDROID || IOS
    scrollView.ScrollToAsync(richTextEditor, ScrollToPosition.MakeVisible, true);
#endif
}

private void OnEditorUnfocused(object sender, EventArgs e)
{
    // Restore layout when keyboard dismisses
#if ANDROID || IOS
    scrollView.ScrollToAsync(0, 0, true);
#endif
}
```

## Event Subscription Patterns

### Subscribe in Constructor

```csharp
public class EditorPage : ContentPage
{
    private SfRichTextEditor richTextEditor;
    
    public EditorPage()
    {
        InitializeComponent();
        
        richTextEditor = new SfRichTextEditor();
        
        // Subscribe to events
        richTextEditor.TextChanged += OnTextChanged;
        richTextEditor.FormatChanged += OnFormatChanged;
        richTextEditor.HyperlinkClicked += OnHyperlinkClicked;
        richTextEditor.Focused += OnEditorFocused;
        richTextEditor.Unfocused += OnEditorUnfocused;
    }
}
```

### Unsubscribe on Cleanup

```csharp
protected override void OnDisappearing()
{
    base.OnDisappearing();
    
    // Unsubscribe to prevent memory leaks
    richTextEditor.TextChanged -= OnTextChanged;
    richTextEditor.FormatChanged -= OnFormatChanged;
    richTextEditor.HyperlinkClicked -= OnHyperlinkClicked;
    richTextEditor.Focused -= OnEditorFocused;
    richTextEditor.Unfocused -= OnEditorUnfocused;
}
```

### Conditional Event Handling

```csharp
private bool isLoading = false;

private void OnTextChanged(object sender, RichTextEditorTextChangedEventArgs e)
{
    // Ignore changes during loading
    if (isLoading) return;
    
    // Process text changes
    HandleTextChange(e.NewText);
}

public async Task LoadContent(string html)
{
    isLoading = true;
    richTextEditor.HtmlText = html;
    await Task.Delay(100); // Allow rendering
    isLoading = false;
}
```

## Real-World Scenarios

### Complete Auto-Save with Debouncing

```csharp
public class AutoSaveEditor
{
    private SfRichTextEditor editor;
    private System.Timers.Timer debounceTimer;
    private bool hasUnsavedChanges = false;
    private Label statusLabel;
    
    public AutoSaveEditor(SfRichTextEditor editor, Label statusLabel)
    {
        this.editor = editor;
        this.statusLabel = statusLabel;
        
        // Setup debounce timer (2 seconds)
        debounceTimer = new System.Timers.Timer(2000);
        debounceTimer.AutoReset = false;
        debounceTimer.Elapsed += async (s, e) => await PerformSave();
        
        // Subscribe to text changes
        editor.TextChanged += OnTextChanged;
    }
    
    private void OnTextChanged(object sender, RichTextEditorTextChangedEventArgs e)
    {
        hasUnsavedChanges = true;
        
        // Reset timer on each change
        debounceTimer.Stop();
        debounceTimer.Start();
        
        MainThread.BeginInvokeOnMainThread(() =>
        {
            statusLabel.Text = "Unsaved changes...";
            statusLabel.TextColor = Colors.Orange;
        });
    }
    
    private async Task PerformSave()
    {
        if (!hasUnsavedChanges) return;
        
        try
        {
            string content = editor.HtmlText;
            await SaveToServer(content);
            
            hasUnsavedChanges = false;
            
            MainThread.BeginInvokeOnMainThread(() =>
            {
                statusLabel.Text = $"Saved at {DateTime.Now:HH:mm:ss}";
                statusLabel.TextColor = Colors.Green;
            });
        }
        catch (Exception ex)
        {
            MainThread.BeginInvokeOnMainThread(() =>
            {
                statusLabel.Text = "Save failed!";
                statusLabel.TextColor = Colors.Red;
            });
        }
    }
    
    private async Task SaveToServer(string content)
    {
        // Implement save logic
        await Task.Delay(500); // Simulate network call
    }
}
```

### Content Statistics Dashboard

```csharp
public class ContentStatistics
{
    private SfRichTextEditor editor;
    private Label charCountLabel;
    private Label wordCountLabel;
    private Label readTimeLabel;
    
    public ContentStatistics(SfRichTextEditor editor, 
        Label charCount, Label wordCount, Label readTime)
    {
        this.editor = editor;
        this.charCountLabel = charCount;
        this.wordCountLabel = wordCount;
        this.readTimeLabel = readTime;
        
        editor.TextChanged += OnTextChanged;
    }
    
    private void OnTextChanged(object sender, RichTextEditorTextChangedEventArgs e)
    {
        UpdateStatistics();
    }
    
    private void UpdateStatistics()
    {
        string plainText = editor.Text;
        
        if (string.IsNullOrWhiteSpace(plainText))
        {
            charCountLabel.Text = "0 characters";
            wordCountLabel.Text = "0 words";
            readTimeLabel.Text = "0 min read";
            return;
        }
        
        // Character count
        int charCount = plainText.Length;
        charCountLabel.Text = $"{charCount:N0} characters";
        
        // Word count
        int wordCount = plainText.Split(new[] { ' ', '\n', '\r', '\t' }, 
            StringSplitOptions.RemoveEmptyEntries).Length;
        wordCountLabel.Text = $"{wordCount:N0} words";
        
        // Estimated read time (200 words per minute)
        int readMinutes = (int)Math.Ceiling(wordCount / 200.0);
        readTimeLabel.Text = $"{readMinutes} min read";
    }
}
```

### Collaborative Editing Indicator

```csharp
public class CollaborativeEditor
{
    private SfRichTextEditor editor;
    private Label statusLabel;
    private string userId;
    
    public CollaborativeEditor(SfRichTextEditor editor, Label statusLabel, string userId)
    {
        this.editor = editor;
        this.statusLabel = statusLabel;
        this.userId = userId;
        
        editor.TextChanged += OnTextChanged;
        editor.Focused += OnFocused;
        editor.Unfocused += OnUnfocused;
    }
    
    private async void OnTextChanged(object sender, RichTextEditorTextChangedEventArgs e)
    {
        // Broadcast changes to other users
        await BroadcastChange(userId, e.NewText);
    }
    
    private async void OnFocused(object sender, EventArgs e)
    {
        // Notify others that this user is editing
        await NotifyEditing(userId, true);
        statusLabel.Text = "You are editing";
        statusLabel.TextColor = Colors.Green;
    }
    
    private async void OnUnfocused(object sender, EventArgs e)
    {
        // Notify others that this user stopped editing
        await NotifyEditing(userId, false);
        statusLabel.Text = "Not editing";
        statusLabel.TextColor = Colors.Gray;
    }
    
    private async Task BroadcastChange(string userId, string content)
    {
        // Send to collaboration server
        await Task.CompletedTask;
    }
    
    private async Task NotifyEditing(string userId, bool isEditing)
    {
        // Notify collaboration server
        await Task.CompletedTask;
    }
}
```

## Best Practices

### 1. Debounce Expensive Operations

```csharp
// Don't save on every keystroke
private void OnTextChanged(object sender, RichTextEditorTextChangedEventArgs e)
{
    debounceTimer.Stop();
    debounceTimer.Start(); // Reset timer
}
```

### 2. Unsubscribe from Events

```csharp
protected override void OnDisappearing()
{
    base.OnDisappearing();
    
    // Prevent memory leaks
    richTextEditor.TextChanged -= OnTextChanged;
    richTextEditor.FormatChanged -= OnFormatChanged;
}
```

### 3. Handle Events on Main Thread

```csharp
private async void OnTextChanged(object sender, RichTextEditorTextChangedEventArgs e)
{
    // UI updates must happen on main thread
    MainThread.BeginInvokeOnMainThread(() =>
    {
        statusLabel.Text = "Content changed";
    });
}
```

### 4. Avoid Heavy Processing in Event Handlers

```csharp
private void OnTextChanged(object sender, RichTextEditorTextChangedEventArgs e)
{
    // Queue heavy work for background thread
    Task.Run(async () =>
    {
        await ProcessContentAsync(e.NewText);
    });
}
```

### 5. Provide User Feedback

```csharp
private void OnTextChanged(object sender, RichTextEditorTextChangedEventArgs e)
{
    // Show saving indicator
    savingIndicator.IsVisible = true;
    
    // Perform save
    SaveContent();
    
    // Hide indicator
    savingIndicator.IsVisible = false;
}
```

### 6. Handle Errors Gracefully

```csharp
private async void OnHyperlinkClicked(object sender, RichTextEditorHyperlinkClickedEventArgs e)
{
    try
    {
        await Launcher.OpenAsync(e.URL);
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", $"Failed to open link: {ex.Message}", "OK");
    }
}
```

### 7. Use Event Aggregation for Complex Scenarios

```csharp
public class EditorEventAggregator
{
    public event EventHandler<string> ContentSaved;
    public event EventHandler<int> CharacterCountChanged;
    public event EventHandler<string> FormatApplied;
    
    public void OnContentSaved(string content)
    {
        ContentSaved?.Invoke(this, content);
    }
    
    public void OnCharacterCountChanged(int count)
    {
        CharacterCountChanged?.Invoke(this, count);
    }
    
    public void OnFormatApplied(string format)
    {
        FormatApplied?.Invoke(this, format);
    }
}
```

## Next Steps

- Explore [content management](content-management.md) for saving/loading based on events
- Learn about [advanced features](advanced-features.md) for AutoSize and visual effects
- Review [formatting and customization](formatting-and-customization.md) to combine with events
