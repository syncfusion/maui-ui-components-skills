# Suggestions in .NET MAUI Chat (SfChat)

## Table of Contents
- [Message-Level Suggestions](#message-level-suggestions)
- [Chat-Level Suggestions](#chat-level-suggestions)
- [Handling Suggestion Selection](#handling-suggestion-selection)
- [Keep Suggestions Visible After Selection](#keep-suggestions-visible-after-selection)
- [Prevent Auto-Send on Selection](#prevent-auto-send-on-selection)
- [Clearing Suggestions After Selection](#clearing-suggestions-after-selection)

---

SfChat supports displaying a list of quick-reply options as suggestions — either inline below a specific message or at the bottom of the entire chat control. Tapping a suggestion automatically sends it as an outgoing message.

---

## Message-Level Suggestions

Attach suggestions directly to any message using `Message.Suggestions`. The suggestions appear below that specific message.

```csharp
// ViewModel.cs
var chatSuggestions = new ChatSuggestions();
chatSuggestions.Items = new ObservableCollection<ISuggestion>
{
    new Suggestion { Text = "Airways 1" },
    new Suggestion { Text = "Airways 2" },
    new Suggestion { Text = "Airways 3" }
};

messages.Add(new TextMessage
{
    Author = new Author { Name = "Travel Bot", Avatar = "bot.png" },
    Text = "Here are some flight options:",
    Suggestions = chatSuggestions
});
```

### With Images in Suggestions

Use the `Suggestion(text, image)` constructor to include icons:

```csharp
chatSuggestions.Items = new ObservableCollection<ISuggestion>
{
    new Suggestion("Airways 1", "flight1.png"),
    new Suggestion("Airways 2", "flight2.png"),
    new Suggestion("Airways 3", "flight3.png")
};
```

### Suggestion Orientation

Display suggestions horizontally (default) or vertically:

```csharp
chatSuggestions.Orientation = SuggestionsOrientation.Vertical;
```

---

## Chat-Level Suggestions

Show a persistent suggestion bar at the bottom of the entire chat control using `SfChat.Suggestions`.

**XAML:**
```xml
<sfChat:SfChat Messages="{Binding Messages}"
               CurrentUser="{Binding CurrentUser}"
               Suggestions="{Binding ChatSuggestions}" />
```

**ViewModel:**
```csharp
public ChatSuggestions ChatSuggestions { get; set; }

public ViewModel()
{
    ChatSuggestions = new ChatSuggestions();
    ChatSuggestions.Items = new ObservableCollection<ISuggestion>
    {
        new Suggestion("Yes"),
        new Suggestion("No"),
        new Suggestion("Maybe")
    };
}
```

> Chat-level suggestions persist across all messages. To show contextual suggestions only for a specific message, use message-level suggestions instead.

---

## Handling Suggestion Selection

`SuggestionItemSelected` fires when the user taps a suggestion. `SuggestionItemSelectedEventArgs` provides:

| Property | Description |
|---|---|
| `SelectedItem` | The tapped `ISuggestion` item |
| `Message` | The message the suggestion belongs to |
| `SuggestionType` | `Inline` (message-level) or `Outline` (chat-level) |
| `HideAfterSelection` | Whether to hide the list after selection (default: `true`) |
| `CancelSendMessage` | Whether to prevent the suggestion from being auto-sent (default: `false`) |

**Event approach:**
```csharp
sfChat.SuggestionItemSelected += (sender, e) =>
{
    var selected = e.SelectedItem as Suggestion;
    var text = selected?.Text;
    // Respond based on selection
    messages.Add(new TextMessage
    {
        Author = new Author { Name = "Bot" },
        Text = $"You selected: {text}"
    });
};
```

**MVVM command approach:**
```xml
<sfChat:SfChat SuggestionItemSelectedCommand="{Binding SuggestionItemSelectedCommand}" ... />
```
```csharp
public ICommand SuggestionItemSelectedCommand => new Command<object>(args =>
{
    var e = args as SuggestionItemSelectedEventArgs;
    var text = (e.SelectedItem as Suggestion)?.Text;
    // Handle selected suggestion
});
```

---

## Keep Suggestions Visible After Selection

By default, the suggestion list hides after selection. To keep it open:

```csharp
sfChat.SuggestionItemSelected += (sender, e) =>
{
    e.HideAfterSelection = false; // Keep list visible
};
```

---

## Prevent Auto-Send on Selection

By default, selecting a suggestion immediately sends it as a message. To instead populate the editor for editing before sending:

```csharp
sfChat.SuggestionItemSelected += (sender, e) =>
{
    e.CancelSendMessage = true; // Places text in editor instead of sending
};
```

---

## Clearing Suggestions After Selection

To remove chat-level suggestions after a choice is made:

```csharp
sfChat.SuggestionItemSelected += (sender, e) =>
{
    sfChat.Suggestions = null; // Remove the chat-level suggestion bar
};
```
