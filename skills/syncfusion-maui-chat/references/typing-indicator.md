# Typing Indicator in .NET MAUI Chat (SfChat)

## Table of Contents
- [Enabling the Typing Indicator](#enabling-the-typing-indicator)
- [Avatar Display Type](#avatar-display-type)
- [Showing/Hiding Dynamically](#showinghiding-dynamically)
- [Detecting User Typing](#detecting-user-typing)
- [Adjusting Indicator Height](#adjusting-indicator-height)
- [Key Properties](#key-properties)

---

The typing indicator displays an animated indicator showing that another user is composing a message. This is a common pattern in chat apps to signal real-time activity.

---

## Enabling the Typing Indicator

Set `ShowTypingIndicator="True"` and bind a `ChatTypingIndicator` instance to `TypingIndicator`:

**XAML:**
```xml
<sfChat:SfChat Messages="{Binding Messages}"
               CurrentUser="{Binding CurrentUser}"
               TypingIndicator="{Binding TypingIndicator}"
               ShowTypingIndicator="{Binding ShowTypingIndicator}" />
```

**ViewModel:**
```csharp
public class ChatViewModel : INotifyPropertyChanged
{
    private bool showTypingIndicator;
    private ChatTypingIndicator typingIndicator;

    public bool ShowTypingIndicator
    {
        get => showTypingIndicator;
        set { showTypingIndicator = value; OnPropertyChanged(nameof(ShowTypingIndicator)); }
    }

    public ChatTypingIndicator TypingIndicator
    {
        get => typingIndicator;
        set { typingIndicator = value; OnPropertyChanged(nameof(TypingIndicator)); }
    }

    public ChatViewModel()
    {
        TypingIndicator = new ChatTypingIndicator();
        TypingIndicator.Authors.Add(new Author { Name = "Harrison", Avatar = "harrison.png" });
        TypingIndicator.Text = "Harrison is typing...";
        ShowTypingIndicator = true;
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

---

## Avatar Display Type

Control whether the typing indicator shows an image or initials:

```csharp
// Show the author's avatar image (default)
TypingIndicator.AvatarViewType = AvatarViewType.Image;

// Show the author's initials instead
TypingIndicator.AvatarViewType = AvatarViewType.Text;
```

---

## Showing/Hiding Dynamically

Toggle the typing indicator in response to real-time events (e.g., SignalR messages, API responses):

```csharp
// Show when bot starts processing
ShowTypingIndicator = true;
TypingIndicator.Authors.Clear();
TypingIndicator.Authors.Add(new Author { Name = "Support Bot", Avatar = "bot.png" });
TypingIndicator.Text = "Support Bot is typing...";

// Hide after bot response is ready
ShowTypingIndicator = false;
Messages.Add(new TextMessage
{
    Author = new Author { Name = "Support Bot", Avatar = "bot.png" },
    Text = "Here is your answer!"
});
```

---

## Detecting User Typing

Use the `Editor` property to listen for when the current user starts or stops typing — useful for sending typing notifications to other users:

```csharp
// MainPage.xaml.cs
public MainPage()
{
    InitializeComponent();
    // User started typing
    sfChat.Editor.TextChanged += (s, e) =>
    {
        if (!string.IsNullOrEmpty(e.NewTextValue))
            SendTypingNotificationToServer(isTyping: true);
    };

    // User stopped typing (lost focus)
    sfChat.Editor.Unfocused += (s, e) =>
    {
        SendTypingNotificationToServer(isTyping: false);
    };
}
```

---

## Adjusting Indicator Height

```csharp
sfChat.TypingIndicatorViewHeight = 60; // Default is auto-sized
```

---

## Key Properties

| Property | Type | Purpose |
|---|---|---|
| `ShowTypingIndicator` | `bool` | Show or hide the indicator |
| `TypingIndicator` | `ChatTypingIndicator` | The indicator instance |
| `TypingIndicator.Authors` | `ObservableCollection<Author>` | Who is typing |
| `TypingIndicator.Text` | `string` | Text shown next to avatar |
| `TypingIndicator.AvatarViewType` | `AvatarViewType` | `Image` or `Text` (initials) |
| `TypingIndicatorViewHeight` | `double` | Height of the indicator row |
