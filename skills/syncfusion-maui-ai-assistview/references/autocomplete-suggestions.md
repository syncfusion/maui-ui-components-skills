# AutoComplete Suggestions

AutoComplete suggestions in AI AssistView provide a real-time overlay of matching suggestions as users type, improving prompt quality and accelerating interactions. The overlay appears after typing a minimum number of characters and dynamically filters based on each keystroke.

## Table of Contents
- [When to Use AutoComplete Suggestions](#when-to-use-autocomplete-suggestions)
- [Basic Setup](#basic-setup)
  - [Defining AutoComplete Suggestions](#defining-autocomplete-suggestions)
  - [XAML Configuration](#xaml-configuration)
- [Configuration Properties](#configuration-properties)
  - [MinimumPrefixCharacters](#minimumprefixcharacters)
  - [SuggestionOpenDelay](#suggestionopendelay)
  - [CancelRequest](#cancelrequest)
- [Handling Selection](#handling-selection)
  - [ItemSelectedCommand](#itemselectedcommand)
- [Custom Templates](#custom-templates)
  - [AutoSuggestionTemplate](#autosuggestiontemplate)
- [Monitoring Overlay State](#monitoring-overlay-state)
  - [IsOpen Property](#isopen-property)
- [Common Patterns](#common-patterns)
  - [Pattern 1: Dynamic Suggestions Based on Context](#pattern-1-dynamic-suggestions-based-on-context)
  - [Pattern 2: Combined with Regular Suggestions](#pattern-2-combined-with-regular-suggestions)
  - [Pattern 3: Server-Based AutoComplete](#pattern-3-server-based-autocomplete)
- [Edge Cases and Troubleshooting](#edge-cases-and-troubleshooting)
  - [Overlay Not Appearing](#overlay-not-appearing)
  - [Performance Issues with Large Lists](#performance-issues-with-large-lists)
  - [Suggestion Not Matching User Input](#suggestion-not-matching-user-input)
  - [CancelRequest Not Working as Expected](#cancelrequest-not-working-as-expected)

## When to Use AutoComplete Suggestions

Use AutoComplete suggestions when:
- **Guiding users** towards well-formed prompts
- **Accelerating input** with pre-defined common queries
- **Reducing errors** by suggesting correct syntax or phrasing
- **Improving AI response quality** through structured prompts
- **Providing contextual help** based on what the user is typing
- **Supporting complex queries** with template-based suggestions

## Basic Setup

### Defining AutoComplete Suggestions

Create a collection of `ISuggestion` items and bind it to the `AutoSuggestions` property:

```csharp
// ViewModel
using Syncfusion.Maui.AIAssistView;
using System.Collections.ObjectModel;

public class AIAssistViewModel : INotifyPropertyChanged
{
    private ObservableCollection<ISuggestion> autoCompleteSuggestions;

    public AIAssistViewModel()
    {
        AutoCompleteSuggestions = new ObservableCollection<ISuggestion>()
        {
            new AssistSuggestion() { Text = "What is .NET MAUI?" },
            new AssistSuggestion() { Text = "How do I get started with AI AssistView?" },
            new AssistSuggestion() { Text = "Explain data binding in .NET MAUI" },
            new AssistSuggestion() { Text = "Show me code examples" },
            new AssistSuggestion() { Text = "What are the key features?" }
        };
    }

    public ObservableCollection<ISuggestion> AutoCompleteSuggestions
    {
        get => autoCompleteSuggestions;
        set
        {
            autoCompleteSuggestions = value;
            RaisePropertyChanged(nameof(AutoCompleteSuggestions));
        }
    }

    // INotifyPropertyChanged implementation
    public event PropertyChangedEventHandler PropertyChanged;
    protected void RaisePropertyChanged(string propertyName) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
}
```

### XAML Configuration

```xml
<syncfusion:SfAIAssistView x:Name="aiAssistView">
    <syncfusion:SfAIAssistView.AutoSuggestionOverlay>
        <syncfusion:AutoSuggestionOverlay 
            AutoSuggestions="{Binding AutoCompleteSuggestions}" />
    </syncfusion:SfAIAssistView.AutoSuggestionOverlay>
</syncfusion:SfAIAssistView>
```

**Result:** As the user types, matching suggestions appear in an overlay. Once they type enough characters (default: 1), the filtered list displays.

---

## Configuration Properties

### MinimumPrefixCharacters

Controls how many characters must be typed before the autocomplete overlay appears. Default is `1`.

**When to use:**
- Set to `2-3` for large suggestion lists to avoid premature filtering
- Set to `1` for small, focused suggestion sets
- Increase for network-based lookups to reduce API calls

```xml
<syncfusion:AutoSuggestionOverlay 
    AutoSuggestions="{Binding AutoCompleteSuggestions}"
    MinimumPrefixCharacters="3" />
```

```csharp
// Code-behind
aiAssistView.AutoSuggestionOverlay = new AutoSuggestionOverlay()
{
    AutoSuggestions = viewModel.AutoCompleteSuggestions,
    MinimumPrefixCharacters = 3
};
```

**Use case:** If you have 100+ suggestions, requiring 3 characters prevents showing too many irrelevant matches initially.

### SuggestionOpenDelay

Sets the delay (in milliseconds) before triggering the suggestion query after the user stops typing. Default is `200` ms.

**When to use:**
- Increase to `300-500` ms to reduce filtering operations when users type quickly
- Keep default for responsive, real-time filtering
- Helps with performance when suggestions involve complex filtering

```xml
<syncfusion:AutoSuggestionOverlay 
    AutoSuggestions="{Binding AutoCompleteSuggestions}"
    SuggestionOpenDelay="300" />
```

```csharp
aiAssistView.AutoSuggestionOverlay = new AutoSuggestionOverlay()
{
    AutoSuggestions = viewModel.AutoCompleteSuggestions,
    SuggestionOpenDelay = 300
};
```

**Use case:** When filtering involves API calls or heavy computation, a 300-500ms delay prevents excessive processing.

### CancelRequest

Controls whether selecting a suggestion submits it immediately or populates the editor for review. Default is `false`.

- **`false`** (default): Selected suggestion is placed in the editor; user can review/edit before submitting
- **`true`**: Selected suggestion is immediately sent as a request without confirmation

```xml
<!-- Immediate submission -->
<syncfusion:AutoSuggestionOverlay 
    AutoSuggestions="{Binding AutoCompleteSuggestions}"
    CancelRequest="false" />
```

```csharp
aiAssistView.AutoSuggestionOverlay = new AutoSuggestionOverlay()
{
    AutoSuggestions = viewModel.AutoCompleteSuggestions,
    CancelRequest = false  // Submit immediately on selection
};
```

**When to use:**
- Set `false` for quick, one-tap submission (FAQ scenarios)
- Keep `true` when users might want to modify suggestions before sending

---

## Handling Selection

### ItemSelectedCommand

Execute custom logic when a suggestion is selected:

```xml
<syncfusion:AutoSuggestionOverlay 
    AutoSuggestions="{Binding AutoCompleteSuggestions}"
    ItemSelectedCommand="{Binding SuggestionSelectedCommand}" />
```

```csharp
// ViewModel
public ICommand SuggestionSelectedCommand { get; set; }

public AIAssistViewModel()
{
    SuggestionSelectedCommand = new Command<ISuggestion>(OnSuggestionSelected);
}

private void OnSuggestionSelected(ISuggestion selectedSuggestion)
{
    // Log analytics
    Console.WriteLine($"User selected: {selectedSuggestion.Text}");
    
    // Custom logic: pre-fill context, trigger actions, etc.
    if (selectedSuggestion.Text.Contains("example"))
    {
        // Load example data
        LoadExamples();
    }
}
```

**Use cases:**
- Track which suggestions are most used
- Pre-load related data based on selection
- Customize behavior per suggestion type
- Trigger side effects (navigation, state changes)

---

## Custom Templates

### AutoSuggestionTemplate

Customize the visual appearance of each suggestion row with icons, secondary text, or custom layouts:

```xml
<ContentPage.Resources>
    <DataTemplate x:Key="autoSuggestionTemplate">
        <Grid Padding="12,8">
            <StackLayout Spacing="2">
                <Label Text="{Binding Text}"
                       FontSize="14"
                       FontAttributes="Bold"
                       TextColor="#333333" />
                <Label Text="{Binding Description}"
                       FontSize="12"
                       TextColor="#666666" />
            </StackLayout>
        </Grid>
    </DataTemplate>
</ContentPage.Resources>

<syncfusion:SfAIAssistView>
    <syncfusion:SfAIAssistView.AutoSuggestionOverlay>
        <syncfusion:AutoSuggestionOverlay 
            AutoSuggestions="{Binding AutoCompleteSuggestions}"
            AutoSuggestionTemplate="{StaticResource autoSuggestionTemplate}" />
    </syncfusion:SfAIAssistView.AutoSuggestionOverlay>
</syncfusion:SfAIAssistView>
```

**Code-behind template creation:**

```csharp
private DataTemplate CreateSuggestionTemplate()
{
    return new DataTemplate(() =>
    {
        var grid = new Grid
        {
            Padding = new Thickness(12, 8),
        };

        var label = new Label 
        { 
            FontSize = 14,
            FontAttributes = FontAttributes.Bold,
            TextColor = Color.FromArgb("#333333"),
            VerticalOptions = LayoutOptions.Center 
        };
        label.SetBinding(Label.TextProperty, "Text");

        grid.Add(label);
        return grid;
    });
}

// Apply template
aiAssistView.AutoSuggestionOverlay.AutoSuggestionTemplate = CreateSuggestionTemplate();
```

---

## Monitoring Overlay State

### IsOpen Property

The `IsOpen` property (read-only) indicates whether the autocomplete overlay is currently visible:

```csharp
var autoOverlay = aiAssistView.AutoSuggestionOverlay;

if (autoOverlay.IsOpen)
{
    Console.WriteLine("Autocomplete overlay is visible");
    // Adjust UI layout if needed
}
else
{
    Console.WriteLine("Autocomplete overlay is hidden");
}
```

**Use cases:**
- Adjust bottom margin when overlay appears
- Disable other UI elements during suggestion browsing
- Track user interaction patterns
- Debug suggestion behavior

**⚠️ Note:** `IsOpen` is managed internally by the control and cannot be set directly.

---

## Common Patterns

### Pattern 1: Dynamic Suggestions Based on Context

Filter suggestions based on conversation state:

```csharp
private void UpdateAutoCompleteSuggestions(string context)
{
    if (context.Contains("code"))
    {
        AutoCompleteSuggestions = new ObservableCollection<ISuggestion>()
        {
            new AssistSuggestion { Text = "Show C# example" },
            new AssistSuggestion { Text = "Show XAML example" },
            new AssistSuggestion { Text = "Explain code structure" }
        };
    }
    else if (context.Contains("design"))
    {
        AutoCompleteSuggestions = new ObservableCollection<ISuggestion>()
        {
            new AssistSuggestion { Text = "UI best practices" },
            new AssistSuggestion { Text = "Color schemes" },
            new AssistSuggestion { Text = "Layout patterns" }
        };
    }
}
```

### Pattern 2: Combined with Regular Suggestions

Use both AutoComplete (as-you-type) and regular suggestions (displayed below header):

```xml
<syncfusion:SfAIAssistView 
    Suggestions="{Binding CommonSuggestions}">
    <syncfusion:SfAIAssistView.AutoSuggestionOverlay>
        <syncfusion:AutoSuggestionOverlay 
            AutoSuggestions="{Binding AutoCompleteSuggestions}" />
    </syncfusion:SfAIAssistView.AutoSuggestionOverlay>
</syncfusion:SfAIAssistView>
```

**Strategy:**
- **Regular Suggestions:** Broad, common queries shown at start
- **AutoComplete Suggestions:** Contextual, filtered as user types

**Best practices:**
- Use `SuggestionOpenDelay` of 300-500ms to reduce API calls
- Implement cancellation tokens to abort stale requests
- Cache frequently used suggestions locally
- Show loading indicator if needed

---

## Edge Cases and Troubleshooting

### Overlay Not Appearing

**Cause:** Not enough characters typed or suggestions list is empty.

**Solution:**
- Check `MinimumPrefixCharacters` setting (default: 1)
- Verify `AutoSuggestions` collection has items
- Ensure user input matches suggestion text (case-insensitive by default)

```csharp
// Debug
Console.WriteLine($"Min chars: {autoOverlay.MinimumPrefixCharacters}");
Console.WriteLine($"Suggestion count: {AutoCompleteSuggestions.Count}");
Console.WriteLine($"Overlay open: {autoOverlay.IsOpen}");
```

### Performance Issues with Large Lists

**Cause:** Filtering thousands of suggestions in real-time.

**Solution:**
- Increase `MinimumPrefixCharacters` to 2-3
- Increase `SuggestionOpenDelay` to 300-500ms
- Use virtualization by limiting displayed suggestions to top N matches
- Pre-filter on server side for network-based suggestions

### Suggestion Not Matching User Input

**Cause:** Suggestion text doesn't contain typed characters.

**Solution:** Ensure suggestions contain keywords users are likely to type:

```csharp
// ❌ Bad: User types "how", won't match "Getting started guide"
new AssistSuggestion { Text = "Getting started guide" }

// ✅ Good: Contains "how"
new AssistSuggestion { Text = "How to get started with AI AssistView" }
```

### CancelRequest Not Working as Expected

**Symptom:** Suggestion always/never submits immediately regardless of setting.

**Solution:**
- Verify `CancelRequest` property value
- Check if `ItemSelectedCommand` is interfering
- Ensure `RequestCommand` or `Request` event is wired up

```csharp
// Verify configuration
Console.WriteLine($"CancelRequest: {autoOverlay.CancelRequest}");
```

---