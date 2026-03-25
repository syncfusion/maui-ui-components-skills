# Data Binding in .NET MAUI Chat (SfChat)

## Table of Contents
- [Two Approaches to Binding Messages](#two-approaches-to-binding-messages)
- [Approach 1: Binding with Messages (Standard)](#approach-1-binding-with-messages-standard)
- [Approach 2: Binding with ItemsSource + ItemsSourceConverter](#approach-2-binding-with-itemssource--itemssourceconverter)
- [Understanding CurrentUser in Data Binding](#understanding-currentuser-in-data-binding)
- [Dynamic Updates](#dynamic-updates)
- [Accessing the Original Data Object](#accessing-the-original-data-object)

---

## Two Approaches to Binding Messages

SfChat supports two data binding strategies:

| Approach | When to Use |
|---|---|
| **`Messages` + `ObservableCollection<object>`** | New projects using Syncfusion message types directly |
| **`ItemsSource` + `ItemsSourceConverter`** | Existing data models you want to map to chat messages |

---

## Approach 1: Binding with `Messages` (Standard)

The simplest approach — bind an `ObservableCollection<object>` directly to the `Messages` property. Add Syncfusion message types (`TextMessage`, `ImageMessage`, etc.) directly to the collection.

```xml
<sfChat:SfChat Messages="{Binding Messages}"
               CurrentUser="{Binding CurrentUser}" />
```

```csharp
public class ChatViewModel : INotifyPropertyChanged
{
    public ObservableCollection<object> Messages { get; set; }
    public Author CurrentUser { get; set; }

    public ChatViewModel()
    {
        CurrentUser = new Author { Name = "Nancy" };
        Messages = new ObservableCollection<object>
        {
            new TextMessage { Author = CurrentUser, Text = "Hello!" },
            new TextMessage
            {
                Author = new Author { Name = "Bot", Avatar = "bot.png" },
                Text = "Hi Nancy, how can I help?"
            }
        };
    }

    public event PropertyChangedEventHandler PropertyChanged;
}
```

**Add messages dynamically at runtime:**
```csharp
Messages.Add(new TextMessage
{
    Author = new Author { Name = "Bot", Avatar = "bot.png" },
    Text = "Here is your response."
});
```

> Use `ObservableCollection<object>` (not `List<T>`) so the UI automatically updates when items are added or removed.

---

## Approach 2: Binding with `ItemsSource` + `ItemsSourceConverter`

Use this when you have an existing data model and want to avoid rewriting your collection as Syncfusion message types. SfChat maps your objects to/from chat messages via a converter.

### Step 1: Define your data model

```csharp
// Model.cs
public class MessageModel
{
    public Author User { get; set; }
    public string Text { get; set; }
    public ChatSuggestions Suggestions { get; set; }
}
```

### Step 2: Set up the ViewModel

```csharp
// ViewModel.cs
public class ViewModel : INotifyPropertyChanged
{
    public ObservableCollection<MessageModel> MessageCollection { get; set; }
    public Author CurrentUser { get; set; }

    public ViewModel()
    {
        CurrentUser = new Author { Name = "Stevan" };
        MessageCollection = new ObservableCollection<MessageModel>();
        GenerateMessages();
    }

    private void GenerateMessages()
    {
        MessageCollection.Add(new MessageModel
        {
            User = CurrentUser,
            Text = "Hi! Our team is launching a new mobile app."
        });
        MessageCollection.Add(new MessageModel
        {
            User = new Author { Name = "Andrea", Avatar = "andrea.png" },
            Text = "That's great!"
        });
    }

    public event PropertyChangedEventHandler PropertyChanged;
}
```

### Step 3: Implement `IChatMessageConverter`

The converter bridges your data model and SfChat's internal message types. Implement two methods:

- `ConvertToChatMessage` — converts your model → a chat message (called when items are loaded or added to `ItemsSource`)
- `ConvertToData` — converts a chat message → your model (called when the user sends a new message)

```csharp
// MessageConverter.cs
public class MessageConverter : IChatMessageConverter
{
    public IMessage ConvertToChatMessage(object data, SfChat chat)
    {
        var item = data as MessageModel;
        var message = new TextMessage
        {
            Text = item.Text,
            Author = item.User,
            Data = item  // Store original object for reference
        };
        if (item.Suggestions != null)
            message.Suggestions = item.Suggestions;
        return message;
    }

    public object ConvertToData(object chatMessage, SfChat chat)
    {
        var item = chatMessage as TextMessage;
        var model = new MessageModel
        {
            Text = item.Text,
            User = chat.CurrentUser
        };
        if (chat.Suggestions != null)
            model.Suggestions = chat.Suggestions;
        return model;
    }
}
```

### Step 4: Bind in XAML

```xml
<ContentPage.Resources>
    <local:MessageConverter x:Key="MessageConverter"/>
</ContentPage.Resources>

<sfChat:SfChat CurrentUser="{Binding CurrentUser}"
               ItemsSource="{Binding MessageCollection}"
               ItemsSourceConverter="{StaticResource MessageConverter}" />
```

### Step 4 (alternative): Bind in C#

```csharp
var sfChat = new SfChat();
var viewModel = new ViewModel();
sfChat.CurrentUser = viewModel.CurrentUser;
sfChat.ItemsSource = viewModel.MessageCollection;
sfChat.ItemsSourceConverter = new MessageConverter();
this.Content = sfChat;
```

---

## Understanding CurrentUser in Data Binding

`CurrentUser` determines which messages are rendered as outgoing (right side) vs. incoming (left side):

- Message `Author` == `CurrentUser` → **outgoing**
- Message `Author` != `CurrentUser` → **incoming**

```csharp
// Outgoing — matches CurrentUser
new TextMessage { Author = currentUser, Text = "Hello!" }

// Incoming — different author
new TextMessage { Author = new Author { Name = "Support" }, Text = "How can I help?" }
```

> Always initialize and bind `CurrentUser` before `Messages` / `ItemsSource` to ensure correct rendering on first load.

---

## Dynamic Updates

Both binding approaches support live updates:

```csharp
// Add a new message at runtime (updates UI immediately)
Messages.Add(new TextMessage
{
    Author = new Author { Name = "Bot" },
    Text = "Processing your request..."
});

// Remove a message
Messages.Remove(Messages[0]);

// Clear all messages
Messages.Clear();
```

With `ItemsSource`, modifying `MessageCollection` triggers `ConvertToChatMessage` automatically for new items.

---

## Accessing the Original Data Object

When using `ItemsSourceConverter`, SfChat stores a reference to the original data object on the message via the `Data` property:

```csharp
// Set in ConvertToChatMessage:
message.Data = item; // item is your MessageModel

// Retrieve later from an event handler:
private void sfChat_SendMessage(object sender, SendMessageEventArgs e)
{
    var original = (e.Message as TextMessage)?.Data as MessageModel;
}
```
