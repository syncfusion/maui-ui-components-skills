---
name: syncfusion-maui-chat
description: Implements Syncfusion .NET MAUI Chat (SfChat) control in .NET MAUI applications. Use when working with chat interfaces, messaging UI, conversational interfaces, chat bubbles, or message threads. Covers message types (text, image, calendar, card), data binding, events, suggestions, typing indicators, time breaks, and swiping actions.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Syncfusion .NET MAUI Chat (SfChat)

The Syncfusion .NET MAUI Chat control (`SfChat`) delivers a contemporary conversational UI for building chatbot interfaces, customer support screens, and multi-user messaging experiences. It supports rich message types, real-time typing indicators, suggestions, load-more history, swiping, and deep styling customization.

## When to Use This Skill

- Building a chat UI, chatbot interface, or messaging screen in .NET MAUI
- Displaying conversations between two or more users
- Sending/receiving text, images, cards, hyperlinks, or date/time picker messages
- Implementing typing indicators, message suggestions, or load-more history
- Customizing message appearance, shapes, delivery states, or themes
- Adding swipe actions, time-break grouping, or attachment buttons
- Localizing the chat UI or enabling accessibility features

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- NuGet installation (`Syncfusion.Maui.Chat`)
- Handler registration in `MauiProgram.cs`
- Basic `SfChat` initialization in XAML and C#
- ViewModel setup with `Messages` and `CurrentUser`
- Binding messages to the chat control
- Running the application

### Messages
📄 **Read:** [references/messages.md](references/messages.md)
- `TextMessage`, `DatePickerMessage`, `TimePickerMessage`, `CalendarMessage`
- `HyperlinkMessage`, `ImageMessage`, `CardMessage`
- Delivery states (`ShowDeliveryState`, `DeliveryState` enum, custom icons)
- Pin message (`AllowPinning`, `PinnedMessages`, events, template)
- Message template and `ChatMessageTemplateSelector`
- Customizable incoming/outgoing views
- Message spacing, shape, timestamp format, avatar/author visibility
- Sending messages, keyboard, multiline input, hide input view

### Data Binding
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- Binding `ObservableCollection<object>` to `Messages`
- `CurrentUser` differentiation of sender/receiver
- Custom data models with `IMessage` / `ITextMessage`
- `ItemsSourceConverter` for external model binding
- Dynamically updating messages at runtime

### Suggestions
📄 **Read:** [references/suggestions.md](references/suggestions.md)
- Chat-level suggestions (`SfChat.Suggestions`)
- Message-level suggestions (`TextMessage.Suggestions`)
- `SuggestionItemSelected` event and command
- Customizing suggestion item templates

### Typing Indicator
📄 **Read:** [references/typing-indicator.md](references/typing-indicator.md)
- Enabling the typing indicator (`ShowTypingIndicator`)
- Setting author and message on `TypingIndicator`
- Customizing appearance
- Showing/hiding dynamically

### Load More
📄 **Read:** [references/load-more.md](references/load-more.md)
- Enabling load more (`LoadMoreBehavior`)
- `LoadMore` event and `LoadMoreCommand`
- Loading older messages on scroll
- `IsLazyLoading` property
- Disabling after all messages are loaded

### Events & Commands
📄 **Read:** [references/events.md](references/events.md)
- `SendMessage` / `SendMessageCommand`
- `ImageTapped` / `ImageTappedCommand`
- `CardTapped` / `CardCommand`
- `SuggestionItemSelected`
- `MessagePinned` / `MessageUnpinned`
- `LoadMore` / `LoadMoreCommand`
- Handling and cancelling event args

### Styles & Appearance
📄 **Read:** [references/styles.md](references/styles.md)
- Incoming and outgoing message styling
- Message input view styling
- Time-break and typing indicator styling
- Suggestion view styling
- `MessageShape` options
- Theme support (Material 3, Fluent)

### Accessibility & Localization
📄 **Read:** [references/accessibility-localization.md](references/accessibility-localization.md)
- WCAG 2.0 compliance
- Keyboard navigation and screen reader support
- `AutomationId` for UI testing
- Localization with `.resx` resource files
- Supported localizable strings
- RTL layout support

### Scrolling
📄 **Read:** [references/scrolling.md](references/scrolling.md)
- Programmatic scroll to a specific message (`ScrollToMessage`)
- Auto-scroll to bottom on new message (`CanAutoScrollToBottom`)
- Scroll to bottom floating button (`ShowScrollToBottomButton`)
- Customizing the scroll to bottom button (`ScrollToBottomButtonTemplate`)
- `Scrolled` event and `ChatScrolledEventArgs` (`IsBottomReached`, `IsTopReached`, `ScrollOffset`)

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- Message swiping (left/right, `SwipeStarted`, `SwipeEnded`, swipe templates)
- Time-break grouping (custom time-break template)
- Attachment button (adding, customizing, `AttachmentButtonView`)
- Liquid glass effect (enabling, platform support, customization)
- `MessageSpacing` configuration
- Hiding the message input view (`ShowMessageInputView`)

---

## Quick Start

**1. Install the NuGet package:**
```bash
dotnet add package Syncfusion.Maui.Chat
```

**2. Register the handler in `MauiProgram.cs`:**
```csharp
using Syncfusion.Maui.Core.Hosting;

builder.ConfigureSyncfusionCore();
```

**3. Add `SfChat` in XAML:**
```xml
<ContentPage xmlns:sfChat="clr-namespace:Syncfusion.Maui.Chat;assembly=Syncfusion.Maui.Chat"
             xmlns:local="clr-namespace:MyApp">
    <ContentPage.BindingContext>
        <local:ChatViewModel/>
    </ContentPage.BindingContext>
    <sfChat:SfChat Messages="{Binding Messages}"
                   CurrentUser="{Binding CurrentUser}" />
</ContentPage>
```

**4. Define the ViewModel:**
```csharp
using Syncfusion.Maui.Chat;

public class ChatViewModel : INotifyPropertyChanged
{
    public ObservableCollection<object> Messages { get; set; }
    public Author CurrentUser { get; set; }

    public ChatViewModel()
    {
        CurrentUser = new Author { Name = "Nancy" };
        Messages = new ObservableCollection<object>
        {
            new TextMessage
            {
                Author = CurrentUser,
                Text = "Hello! How can I help you today?"
            },
            new TextMessage
            {
                Author = new Author { Name = "Bot", Avatar = "bot.png" },
                Text = "Hi Nancy! I am here to assist you."
            }
        };
    }

    public event PropertyChangedEventHandler PropertyChanged;
}
```

---

## Common Patterns

### Sending a message and responding
```csharp
// In ViewModel
public ICommand SendMessageCommand => new Command<object>(OnSendMessage);

private void OnSendMessage(object args)
{
    var e = args as SendMessageEventArgs;
    
    // ⚠️ IMPORTANT: By default, SfChat automatically adds the user's message to the 
    // Messages collection. Do NOT manually add it unless you set e.Handled = true
    
    // Add bot response after user sends message
    MainThread.BeginInvokeOnMainThread(async () =>
    {
        await Task.Delay(500); // Simulate processing
        Messages.Add(new TextMessage
        {
            Author = new Author { Name = "Bot", Avatar = "bot.png" },
            Text = $"You said: {e.Message.Text}"
        });
    });
}
```

**If you need full control over message addition, set `Handled = true`:**
```csharp
private void OnSendMessage(object args)
{
    var e = args as SendMessageEventArgs;
    e.Handled = true; // Prevent SfChat from auto-adding the message
    
    // Now you must manually add the message
    if (e.Message is TextMessage textMessage)
    {
        Messages.Add(textMessage); // You add it
        
        // Then handle response...
    }
}
```

### Adding quick reply suggestions
```csharp
Messages.Add(new TextMessage
{
    Author = new Author { Name = "Bot" },
    Text = "How would you like to proceed?",
    Suggestions = new ObservableCollection<ISuggestion>
    {
        new Suggestion { Text = "Option A" },
        new Suggestion { Text = "Option B" }
    }
});
```

### Showing a typing indicator
```csharp
sfChat.ShowTypingIndicator = true;
sfChat.TypingIndicator = new TypingIndicator
{
    Authors = new ObservableCollection<Author>
    {
        new Author { Name = "Bot", Avatar = "bot.png" }
    },
    Text = "Bot is typing..."
};
```

---

## Key Properties

| Property | Type | Purpose |
|---|---|---|
| `Messages` | `ObservableCollection<object>` | Collection of all messages |
| `CurrentUser` | `Author` | Identifies the local user (outgoing messages) |
| `ShowTypingIndicator` | `bool` | Toggle typing indicator |
| `TypingIndicator` | `TypingIndicator` | Set who is typing |
| `ShowDeliveryState` | `bool` | Show sent/delivered/read/failed icons |
| `AllowPinning` | `bool` | Enable message pinning |
| `MessageShape` | `MessageShape` | Message bubble shape |
| `MessageSpacing` | `double` | Vertical gap between messages |
| `ShowMessageInputView` | `bool` | Show/hide the message editor |
| `LoadMoreBehavior` | `LoadMoreOption` | Enable load-more on scroll |
| `AllowMultilineInput` | `bool` | Allow multi-line message entry |
| `ShowKeyboardAlways` | `bool` | Keep keyboard open after send |
