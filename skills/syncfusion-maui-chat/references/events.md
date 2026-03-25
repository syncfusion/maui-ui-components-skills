# Events and Commands in .NET MAUI Chat (SfChat)

## Table of Contents
- [Event/Command Summary](#eventcommand-summary)
- [SendMessage / SendMessageCommand](#sendmessage--sendmessagecommand)
- [MessageTapped / MessageTappedCommand](#messagetapped--messagetappedcommand)
- [MessageDoubleTapped / MessageDoubleTappedCommand](#messagedoubletapped--messagedoubletappedcommand)
- [MessageLongPressed / MessageLongPressedCommand](#messagelongpressed--messagelongpressedcommand)
- [ImageTapped / ImageTappedCommand](#imagetapped--imagetappedcommand)
- [CardTapped / CardCommand](#cardtapped--cardcommand)
- [SuggestionItemSelected / SuggestionItemSelectedCommand](#suggestionitemselected--suggestionitemselectedcommand)
- [LoadMore / LoadMoreCommand](#loadmore--loadmorecommand)
- [MessagePinned / MessageUnpinned](#messagepinned--messageunpinned)
- [Pattern: Handling All Interactions in MVVM](#pattern-handling-all-interactions-in-mvvm)

---

SfChat exposes events and MVVM-friendly commands for every major user interaction. Each event has a corresponding command with the same `EventArgs` type passed as the command parameter.

---

## Event/Command Summary

| Event | Command | Fires When |
|---|---|---|
| `SendMessage` | `SendMessageCommand` | User taps Send or presses Enter |
| `MessageTapped` | `MessageTappedCommand` | User taps a message |
| `MessageDoubleTapped` | `MessageDoubleTappedCommand` | User double-taps a message |
| `MessageLongPressed` | `MessageLongPressedCommand` | User long-presses a message |
| `ImageTapped` | `ImageTappedCommand` | User taps an `ImageMessage` |
| `CardTapped` | `CardCommand` | User taps a card or card button |
| `SuggestionItemSelected` | `SuggestionItemSelectedCommand` | User taps a suggestion |
| `LoadMore` | `LoadMoreCommand` | Load more is triggered (manual or auto) |
| `MessagePinned` | — | A message is pinned |
| `MessageUnpinned` | — | A message is unpinned |

---

## SendMessage / SendMessageCommand

Fires when the user submits a message from the input editor.

⚠️ **CRITICAL**: By default, SfChat **automatically adds the user's message to the `Messages` collection**. You should **NOT** manually add it unless you set `e.Handled = true`. Only set `Handled = true` if you need full control over when the message appears.

```xml
<sfChat:SfChat SendMessage="OnSendMessage"
               SendMessageCommand="{Binding SendMessageCommand}" ... />
```

### Default Behavior (Recommended)
The user's message is automatically added by SfChat — use the handler for custom logic like backend calls or bot responses:

```csharp
private void OnSendMessage(object sender, SendMessageEventArgs e)
{
    var text = e.Message.Text;  // The message text entered by user
    
    // Do NOT add the message manually — SfChat already added it!
    // Just handle your custom logic here (e.g., send to backend, add bot response)
    
    MainThread.BeginInvokeOnMainThread(async () =>
    {
        await Task.Delay(500);
        Messages.Add(new TextMessage
        {
            Author = new Author { Name = "Bot" },
            Text = $"You said: {text}"
        });
    });
}
```

### Custom Handling (Set `Handled = true` for manual control)
Only set `Handled = true` if you want to prevent the default auto-add and manage message addition yourself:

```csharp
private void OnSendMessage(object sender, SendMessageEventArgs e)
{
    var text = e.Message.Text;
    
    // Prevent default — now YOU must add the message
    e.Handled = true;
    
    // Add it manually only after your validations pass
    if (IsMessageValid(text))
    {
        Messages.Add(e.Message);
    }
}
```

### Cancel sending (Prevent the message from appearing at all)
```csharp
private void OnSendMessage(object sender, SendMessageEventArgs e)
{
    if (e.Message.Text.Length > 500)
    {
        e.Handled = true; // Don't add to Messages
        // Show error: "Message too long"
    }
}
```

**MVVM (recommended default behavior):**
```csharp
public ICommand SendMessageCommand => new Command<object>(args =>
{
    var e = args as SendMessageEventArgs;
    // Message is already added by SfChat — just add bot response
    Messages.Add(new TextMessage
    {
        Author = new Author { Name = "Bot" },
        Text = $"Echo: {e.Message.Text}"
    });
});
```

---

## MessageTapped / MessageTappedCommand

Fires when the user taps any message. Provides the tapped message and tap position.

```xml
<sfChat:SfChat MessageTapped="OnMessageTapped" ... />
```
```csharp
private void OnMessageTapped(object sender, MessageTappedEventArgs e)
{
    var author = e.Message.Author.Name;
    var touchPoint = e.Position; // Point of interaction
    // Show context menu, highlight, etc.
}
```

**MVVM:**
```xml
<sfChat:SfChat MessageTappedCommand="{Binding MessageTappedCommand}" ... />
```
```csharp
public ICommand MessageTappedCommand => new Command<object>(args =>
{
    var e = args as MessageTappedEventArgs;
    // Handle tapped message
});
```

---

## MessageDoubleTapped / MessageDoubleTappedCommand

Fires on double-tap. Useful for reactions, editing, or copying message text.

```csharp
sfChat.MessageDoubleTapped += (sender, e) =>
{
    var message = e.Message as TextMessage;
    // e.g., copy message.Text to clipboard
};
```

---

## MessageLongPressed / MessageLongPressedCommand

Fires on long press. Useful for showing context menus (delete, forward, react).

```csharp
sfChat.MessageLongPressed += (sender, e) =>
{
    var message = e.Message;
    // Show action sheet: Delete / Forward / Copy
};
```

---

## ImageTapped / ImageTappedCommand

Fires when the user taps an `ImageMessage`. Access the tapped image via `e.Message`.

```csharp
sfChat.ImageTapped += (sender, e) =>
{
    var image = e.Message; // ImageMessage instance
    // Open full-screen viewer
};
```

---

## CardTapped / CardCommand

Fires when the user taps a `CardMessage` body or a `CardButton`.

```csharp
sfChat.CardTapped += (sender, e) =>
{
    var card = e.Card;       // Card that was tapped
    var button = e.Action;   // CardButton (null if card body was tapped)
    var msg = e.Message;     // The CardMessage

    // Prevent default: card title/button value added as new message
    e.Handled = true;
};
```

> If `e.Action` is null → the card body was tapped → `Card.Title` would be sent.  
> If `e.Action` is non-null → a button was tapped → `CardButton.Value` would be sent.  
> Set `e.Handled = true` to suppress both behaviours.

---

## SuggestionItemSelected / SuggestionItemSelectedCommand

Fires when the user selects a suggestion item.

```csharp
sfChat.SuggestionItemSelected += (sender, e) =>
{
    var selected = e.SelectedItem as Suggestion;
    var msg = e.Message;              // The message these suggestions belong to
    var type = e.SuggestionType;      // Inline or Outline

    e.HideAfterSelection = false;     // Keep suggestions visible
    e.CancelSendMessage = true;       // Don't auto-send; put text in editor instead
};
```

---

## LoadMore / LoadMoreCommand

Fires when the user reaches the top of the chat (manual button tap or auto scroll).

```csharp
public ICommand LoadMoreCommand => new Command<object>(async args =>
{
    IsBusy = true;
    await Task.Delay(1500);   // Simulate fetch
    InsertOlderMessages();
    IsBusy = false;
}, args => OldMessages.Count > 0);
```

---

## MessagePinned / MessageUnpinned

Fires when a message is pinned or unpinned via the long-press context action.

```csharp
sfChat.MessagePinned += (sender, e) =>
{
    var pinned = e.Message; // The message that was pinned
};

sfChat.MessageUnpinned += (sender, e) =>
{
    var unpinned = e.Message; // The message that was unpinned
};
```

---

## Pattern: Handling All Interactions in MVVM

```xml
<sfChat:SfChat Messages="{Binding Messages}"
               CurrentUser="{Binding CurrentUser}"
               SendMessageCommand="{Binding SendMessageCommand}"
               MessageTappedCommand="{Binding MessageTappedCommand}"
               MessageLongPressedCommand="{Binding MessageLongPressedCommand}"
               SuggestionItemSelectedCommand="{Binding SuggestionSelectedCommand}"
               LoadMoreCommand="{Binding LoadMoreCommand}"
               ImageTappedCommand="{Binding ImageTappedCommand}"
               CardCommand="{Binding CardTappedCommand}" />
```
