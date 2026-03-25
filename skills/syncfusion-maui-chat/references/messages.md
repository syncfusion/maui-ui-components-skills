# Messages in .NET MAUI Chat (SfChat)

## Table of Contents
- [Message Base Properties](#message-base-properties)
- [TextMessage](#textmessage)
- [DatePickerMessage](#datepickermessage)
- [TimePickerMessage](#timepickermessage)
- [CalendarMessage](#calendarmessage)
- [HyperlinkMessage](#hyperlinkmessage)
- [ImageMessage](#imagemessage)
- [CardMessage](#cardmessage)
- [Delivery States](#delivery-states)
- [Pin Message](#pin-message)
- [Message Templates](#message-templates)
- [Customizable Views](#customizable-views)
- [Message Appearance](#message-appearance)
- [Sending Messages](#sending-messages)

---

## Message Base Properties

All message types share these common properties from `MessageBase`:

| Property | Purpose |
|---|---|
| `Author` | Sender — set `Author.Name` and `Author.Avatar` |
| `DateTime` | Timestamp displayed on the message |
| `DeliveryState` | Delivery status (Sent, Delivered, Read, Failed) |
| `IsPinned` | Mark message as pinned on initial load |

```csharp
new TextMessage
{
    Author = new Author { Name = "Andrea", Avatar = "andrea.png" },
    DateTime = DateTime.Now,
    Text = "Hello!"
}
```

---

## TextMessage

`TextMessage` displays plain text in a message bubble. It is the most common message type.

```csharp
messages.Add(new TextMessage
{
    Author = currentUser,
    Text = "Hi! Our team is launching a new mobile app.",
});

messages.Add(new TextMessage
{
    Author = new Author { Name = "Andrea", Avatar = "andrea.png" },
    Text = "That's great news!",
});
```

---

## DatePickerMessage

`DatePickerMessage` renders an inline date picker. When the user selects a date, it is automatically added as a `TextMessage` and the `SendMessage` event fires.

```csharp
messages.Add(new DatePickerMessage
{
    Author = new Author { Name = "Travel Bot", Avatar = "bot.png" },
    Text = "Select your departure date",
    SelectedDate = DateTime.Now
});
```

**Handle the selected date in `SendMessage`:**
```csharp
private void sfChat_SendMessage(object sender, SendMessageEventArgs e)
{
    var selectedDate = e.Message.Text; // The chosen date as a string
}
```

---

## TimePickerMessage

`TimePickerMessage` renders an inline time picker. When a time is selected, it is added as a `TextMessage` and the `SendMessage` event fires.

```csharp
messages.Add(new TimePickerMessage
{
    Author = new Author { Name = "Health Care", Avatar = "healthcare.png" },
    Text = "Select a time to meet Dr. Harry",
    SelectedTime = new TimeSpan(8, 30, 0)
});
```

**Handle the selected time:**
```csharp
private void sfChat_SendMessage(object sender, SendMessageEventArgs e)
{
    var selectedTime = e.Message.Text; // The chosen time as a string
}
```

---

## CalendarMessage

`CalendarMessage` renders a full calendar view inline. The date selected is added as a `TextMessage`.

```csharp
messages.Add(new CalendarMessage
{
    Author = new Author { Name = "Health Care" },
    Text = "Select a convenient date to meet Dr. Harry",
    SelectedDate = DateTime.Now
});
```

---

## HyperlinkMessage

`HyperlinkMessage` sends a URL as a message. SfChat automatically fetches and displays the link's thumbnail, title, and description.

```csharp
messages.Add(new HyperlinkMessage
{
    Author = new Author { Name = "Michale", Avatar = "michale.png" },
    Text = "Check out this link to get started",
    Url = "https://dotnet.microsoft.com/en-us/apps/maui"
});
```

> Unlike other message types, `HyperlinkMessage` can also be an **outgoing** message — just set `Author` to `CurrentUser`.

---

## ImageMessage

`ImageMessage` displays an image in the chat. Control display with `Source`, `Size`, and `Aspect`.

```csharp
messages.Add(new ImageMessage
{
    Author = new Author { Name = "Michale", Avatar = "michale.png" },
    Source = "car1.jpg",
    Aspect = Aspect.AspectFill,
    DateTime = DateTime.Now
});
```

> Like `HyperlinkMessage`, `ImageMessage` can also be outgoing — set `Author` to `CurrentUser`.

### ImageTapped Event

Handle image taps to show full-screen previews or sharing options:

```xml
<sfChat:SfChat ImageTapped="sfChat_ImageTapped" ... />
```
```csharp
private void sfChat_ImageTapped(object sender, ImageTappedEventArgs e)
{
    if (e.Message.Source == ImageSource.FromFile("car1.png"))
    {
        // Show full screen or share options
    }
}
```

### ImageTappedCommand (MVVM)

```xml
<sfChat:SfChat ImageTappedCommand="{Binding ImageTappedCommand}" ... />
```
```csharp
public ICommand ImageTappedCommand => new Command<object>(args =>
{
    var tappedImage = (args as ImageTappedEventArgs).Message;
    // Handle accordingly
});
```

---

## CardMessage

`CardMessage` displays a horizontal scrollable list of interactive cards — each with an image, title, subtitle, description, and action buttons. Useful for bot frameworks and rich choices.

```csharp
var cards = new ObservableCollection<Card>
{
    new Card
    {
        Title = "Miami",
        Description = "Miami is the cultural center of South Florida.",
        Image = "miami.png",
        Buttons = { new CardButton { Title = "Choose", Value = "Miami" } }
    },
    new Card
    {
        Title = "Las Vegas",
        Description = "An internationally renowned resort city.",
        Image = "lasvegas.png",
        Buttons = { new CardButton { Title = "Choose", Value = "Las Vegas" } }
    }
};

messages.Add(new CardMessage
{
    Author = new Author { Name = "Travel Bot", Avatar = "bot.png" },
    Cards = cards
});
```

**Card properties:**

| Property | Purpose |
|---|---|
| `Card.Title` | Card heading |
| `Card.Subtitle` | Secondary heading |
| `Card.Description` | Body text |
| `Card.Image` | Card image source |
| `Card.Buttons` | Collection of `CardButton` actions |
| `CardButton.Title` | Button label |
| `CardButton.Value` | Text added as message when button is tapped |

### CardTapped Event

Fires when a card or its button is tapped. `CardTappedEventArgs.Action` is non-null only when a button is tapped.

```xml
<sfChat:SfChat CardTapped="sfChat_CardTapped" ... />
```
```csharp
private void sfChat_CardTapped(object sender, CardTappedEventArgs e)
{
    // Prevent the card title/button value from being added as a message
    e.Handled = true;

    var card = e.Card;           // The tapped card
    var button = e.Action;       // The tapped button (null if card body was tapped)
    var message = e.Message;     // The CardMessage instance
}
```

### CardCommand (MVVM)

```xml
<sfChat:SfChat CardCommand="{Binding CardTappedCommand}" ... />
```
```csharp
public ICommand CardTappedCommand => new Command<object>(args =>
{
    var e = args as CardTappedEventArgs;
    e.Handled = true; // Stop default behaviour
});
```

---

## Delivery States

Show message delivery status indicators (sent, delivered, read, failed).

**Enable delivery states:**
```xml
<sfChat:SfChat ShowDeliveryState="True" ... />
```

**Set state on a message:**
```csharp
var msg = new TextMessage
{
    Author = currentUser,
    Text = "Hello!",
    DeliveryState = DeliveryStates.Sent
};
```

**Available states:**

| State | Meaning |
|---|---|
| `None` | No indicator shown (default) |
| `Sent` | Message sent by current user |
| `Delivered` | Message delivered to recipient |
| `Read` | Message read by recipient |
| `Failed` | Message delivery failed |

**Simulate state progression:**
```csharp
private async void UpdateDeliveryState(TextMessage message)
{
    await Task.Delay(1000);
    message.DeliveryState = DeliveryStates.Delivered;
    await Task.Delay(1000);
    message.DeliveryState = DeliveryStates.Read;
}
```

**Custom delivery icons:**
```xml
<sfChat:SfChat ShowDeliveryState="True"
               SentIcon="senticon.png"
               DeliveredIcon="deliveredicon.png"
               ReadIcon="readicon.png"
               FailedIcon="failedicon.png" ... />
```

---

## Pin Message

Allow users to pin messages for quick reference.

**Enable pinning:**
```xml
<sfChat:SfChat AllowPinning="True" ... />
```

**Pre-pin a message on load:**
```csharp
new TextMessage { ..., IsPinned = true }
```

**Access pinned messages collection:**
```csharp
var pinned = sfChat.PinnedMessages; // Read-only collection
```

**Hide the pinned messages container:**
```xml
<sfChat:SfChat AllowPinning="True" ShowPinnedMessagesContainer="False" ... />
```

**Adjust container height:**
```xml
<sfChat:SfChat AllowPinning="True" PinnedContainerHeight="80" ... />
```

**Custom pinned message template:**
```xml
<sfChat:SfChat AllowPinning="True"
               PinnedMessageTemplate="{StaticResource MyPinnedTemplate}" ... />
```

**Pin/unpin events:**
```xml
<sfChat:SfChat MessagePinned="OnMessagePinned"
               MessageUnpinned="OnMessageUnpinned" ... />
```
```csharp
private void OnMessagePinned(object sender, MessagePinnedEventArgs e)
{
    // e.Message is the pinned message
}
private void OnMessageUnpinned(object sender, MessageUnpinnedEventArgs e)
{
    // e.Message is the unpinned message
}
```

---

## Message Templates

Load a fully custom view for any message using `MessageTemplate`.

```csharp
sfChat.MessageTemplate = new ChatMessageTemplateSelector(sfChat);
```

**Custom selector (full override):**
```csharp
public class ChatMessageTemplateSelector : DataTemplateSelector
{
    private readonly DataTemplate incoming;
    private readonly DataTemplate outgoing;
    private readonly DataTemplate rating;
    private SfChat sfChat;

    public ChatMessageTemplateSelector(SfChat sfChat)
    {
        this.sfChat = sfChat;
        incoming = new DataTemplate(typeof(IncomingView));
        outgoing = new DataTemplate(typeof(OutgoingView));
        rating = new DataTemplate(typeof(RatingView));
    }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        var message = item as IMessage;
        if (message?.Author == sfChat.CurrentUser) return outgoing;
        if ((item as ITextMessage)?.Text == "Rate your experience") return rating;
        return incoming;
    }
}
```

**Selector extending default templates (preferred for partial override):**
```csharp
public class MyTemplateSelector : ChatMessageTemplateSelector
{
    private readonly DataTemplate customOutgoing;
    public MyTemplateSelector(SfChat sfChat) : base(sfChat)
    {
        customOutgoing = new DataTemplate(typeof(MyOutgoingView));
    }

    protected override DataTemplate? OnSelectTemplate(object item, BindableObject container)
    {
        if (item is ITextMessage msg && msg.Text == "Thank you")
            return customOutgoing;
        return base.OnSelectTemplate(item, container); // Use defaults otherwise
    }
}
```

---

## Customizable Views

Target specific parts of incoming/outgoing messages using `ControlTemplate` styles:

| View Class | Targets |
|---|---|
| `IncomingMessageContentView` | Incoming message bubble content |
| `OutgoingMessageContentView` | Outgoing message bubble content |
| `IncomingMessageAuthorView` | Incoming author name area |
| `OutgoingMessageAuthorView` | Outgoing author name area |
| `IncomingMessageAvatarView` | Incoming avatar area |
| `OutgoingMessageAvatarView` | Outgoing avatar area |
| `IncomingMessageTimestampView` | Incoming timestamp area |
| `OutgoingMessageTimestampView` | Outgoing timestamp area |
| `CardButtonView` | Card action button |
| `ChatImageView` | Image message view |
| `MessageSuggestionView` | Per-message suggestion list |
| `ChatSuggestionView` | Chat-level suggestion list |

```xml
<ContentPage.Resources>
    <Style TargetType="sfChat:IncomingMessageContentView">
        <Setter Property="ControlTemplate">
            <Setter.Value>
                <ControlTemplate>
                    <Grid>
                        <Label Text="{Binding Text}" Padding="10"
                               FontSize="12" FontAttributes="Italic"/>
                    </Grid>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</ContentPage.Resources>
```

---

## Message Appearance

### Message Shape
```xml
<sfChat:SfChat MessageShape="DualTearDrop" ... />
```

### Message Spacing
```xml
<sfChat:SfChat MessageSpacing="24" ... />
```

### Timestamp Format
```xml
<sfChat:SfChat IncomingMessageTimestampFormat="hh:mm tt"
               OutgoingMessageTimestampFormat="hh:mm tt" ... />
```

### Avatar and Author Name Visibility

**Show avatar/name for outgoing messages (hidden by default):**
```xml
<sfChat:SfChat ShowOutgoingMessageAvatar="True"
               ShowOutgoingMessageAuthorName="True" ... />
```

**Hide avatar/name for incoming messages (shown by default):**
```xml
<sfChat:SfChat ShowIncomingMessageAvatar="False"
               ShowIncomingMessageAuthorName="False" ... />
```

> When no avatar image is set, SfChat automatically shows the author's initials.

---

## Sending Messages

The user types in the message input editor at the bottom and taps **Send** (or presses Enter on desktop). This fires `SendMessage` event and **automatically adds the message to the `Messages` collection by default**.

> ⚠️ **IMPORTANT**: The message is added automatically by SfChat **unless you explicitly handle the event by setting `e.Handled = true`**. Only set `Handled = true` if you want to prevent the default behavior and manage the message manually.

### Default Behavior (Recommended for simple apps)
By default, SfChat automatically adds the sent message to your `Messages` collection:

```csharp
// No handler needed — SfChat handles message addition automatically
public ICommand SendMessageCommand => new Command<object>(OnSendMessage);

private void OnSendMessage(object args)
{
    var sendMessageEventArgs = args as SendMessageEventArgs;
    if (sendMessageEventArgs?.Message is TextMessage textMessage)
    {
        // Message is ALREADY added to Messages collection by SfChat
        // Only add your custom logic here (e.g., send to backend, log, etc.)
        
        MainThread.BeginInvokeOnMainThread(async () =>
        {
            await Task.Delay(1000);
            // Add bot response or other logic
        });
    }
}
```

### Custom Handling (Set `Handled = true` to manage messages yourself)
If you need full control over when and how messages are added:

```csharp
public ICommand SendMessageCommand => new Command<object>(OnSendMessage);

private void OnSendMessage(object args)
{
    var sendMessageEventArgs = args as SendMessageEventArgs;
    
    // Prevent SfChat from adding the message automatically
    sendMessageEventArgs.Handled = true;
    
    if (sendMessageEventArgs?.Message is TextMessage textMessage)
    {
        // Now YOU must manually add it to Messages
        messages.Add(textMessage);
        
        // Then handle bot response or backend logic
        MainThread.BeginInvokeOnMainThread(async () =>
        {
            await Task.Delay(1000);
            messages.Add(new TextMessage
            {
                Author = new Author { Name = "Bot", Avatar = "bot.png" },
                Text = $"You said: {textMessage.Text}"
            });
        });
    }
}
```

### Cancel a message from being sent (Prevent from appearing at all)
```csharp
// Event approach
sfChat.SendMessage += (sender, e) =>
{
    e.Handled = true; // Prevents adding to Messages
};
```

**MVVM command approach:**
```xml
<sfChat:SfChat SendMessageCommand="{Binding SendMessageCommand}" ... />
```

### Keyboard Behaviour

```xml
<!-- Keep keyboard open after send (default: true) -->
<sfChat:SfChat ShowKeyboardAlways="False" ... />
```

### Single-Line Input

```xml
<!-- Prevent multi-line; shows Send button on keyboard -->
<sfChat:SfChat AllowMultilineInput="False" ... />
```

### Hide the Input Editor

```xml
<!-- Useful for read-only chat or bot-only conversations -->
<sfChat:SfChat ShowMessageInputView="False" ... />
```
