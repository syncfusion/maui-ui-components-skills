# Events and Commands in SfAIAssistView

Core interaction events and commands for responding to user taps, long presses, AI requests, and item actions.

## Table of Contents
- [Item Tapped](#item-tapped)
- [Item Long Pressed](#item-long-pressed)
- [Request Event](#request-event)
- [Copy, Retry, and Rating Commands](#copy-retry-and-rating-commands)

---

## Item Tapped

Raised when the user taps any item in the chat list.

### ItemTappedEventArgs

| Property | Type | Description |
|---|---|---|
| `Item` | `IAssistItem` | The tapped item |
| `Position` | `Point` | Touch position when the item was tapped |

### Event

```xml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           ItemTapped="OnItemTapped" />
```

```csharp
private void OnItemTapped(object sender, ItemTappedEventArgs e)
{
    // e.Item — the tapped IAssistItem
    // e.Position — touch coordinates
    DisplayAlert("Tapped", $"Item: {e.Item.Text}", "OK");
}
```

### Command (MVVM)

```xml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           ItemTappedCommand="{Binding TappedCommand}" />
```

```csharp
public class AIAssistViewModel : INotifyPropertyChanged
{
    public Command<object> TappedCommand { get; }

    public AIAssistViewModel()
    {
        TappedCommand = new Command<object>(OnItemTapped);
    }

    private void OnItemTapped(object obj)
    {
        var args = obj as ItemTappedEventArgs;
        // args.Item, args.Position
    }
}
```

---

## Item Long Pressed

Raised when the user long-presses any item in the chat list.

### ItemLongPressedEventArgs

| Property | Type | Description |
|---|---|---|
| `Item` | `IAssistItem` | The long-pressed item |
| `Position` | `Point` | Touch position of the long press |

### Event

```xml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           ItemLongPressed="OnItemLongPressed" />
```

```csharp
private void OnItemLongPressed(object sender, ItemLongPressedEventArgs e)
{
    // e.Item — the long-pressed IAssistItem
    // e.Position — touch coordinates
    DisplayAlert("Long Pressed", $"Item: {e.Item.Text}", "OK");
}
```

### Command (MVVM)

```xml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           ItemLongPressedCommand="{Binding LongPressedCommand}" />
```

```csharp
public class AIAssistViewModel : INotifyPropertyChanged
{
    public Command<object> LongPressedCommand { get; }

    public AIAssistViewModel()
    {
        LongPressedCommand = new Command<object>(OnItemLongPressed);
    }

    private void OnItemLongPressed(object obj)
    {
        var args = obj as ItemLongPressedEventArgs;
        // args.Item, args.Position
    }
}
```

---

## Request Event

Raised when the user sends a message (taps the send button or submits from the editor). This is the primary hook for calling an AI service and appending a response.

### RequestEventArgs

| Property | Type | Description |
|---|---|---|
| `RequestItem` | `IAssistItem` | The item the user just sent |
| `Handled` | `bool` | Set to `true` to indicate the request has been handled |

### Event

```xml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           Request="OnRequest" />
```

```csharp
private async void OnRequest(object sender, RequestEventArgs e)
{
    // Call AI service with e.RequestItem.Text
    var responseText = await GetAIResponseAsync(e.RequestItem.Text);

    var response = new AssistItem
    {
        Text = responseText,
        IsRequested = false,
        RequestItem = e.RequestItem
    };
    AssistItems.Add(response);
    e.Handled = true;
}
```

### Command (MVVM)

```xml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           AssistItems="{Binding AssistItems}"
                           RequestCommand="{Binding RequestCommand}" />
```

```csharp
public class AIAssistViewModel : INotifyPropertyChanged
{
    public ICommand RequestCommand { get; }

    public AIAssistViewModel()
    {
        RequestCommand = new Command<object>(ExecuteRequest);
    }

    private async void ExecuteRequest(object obj)
    {
        var args = obj as RequestEventArgs;
        var requestText = args.RequestItem.Text;

        // Call AI and append response
        var responseText = await GetAIResponseAsync(requestText);
        AssistItems.Add(new AssistItem
        {
            Text = responseText,
            IsRequested = false,
            RequestItem = args.RequestItem
        });
        args.Handled = true;
    }
}
```

---

## Copy, Retry, and Rating Commands

These commands are triggered by action icons shown on response items. They are command-only (no corresponding events).

### ItemCopyCommand

Executed when the user taps the **copy** icon on a response item.

```xml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           ItemCopyCommand="{Binding CopyCommand}" />
```

```csharp
public Command<object> CopyCommand { get; } = new Command<object>(obj =>
{
    // obj is the AssistItem being copied
    // implement clipboard logic here
});
```

### ItemRetryCommand

Executed when the user taps the **retry** icon on a response item (re-sends the original request).

```xml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           ItemRetryCommand="{Binding RetryCommand}" />
```

```csharp
public Command<object> RetryCommand { get; } = new Command<object>(async obj =>
{
    // obj is the AssistItem — re-send the associated request
    var item = obj as AssistItem;
    var responseText = await GetAIResponseAsync(item.RequestItem.Text);
    AssistItems.Add(new AssistItem
    {
        Text = responseText,
        IsRequested = false,
        RequestItem = item.RequestItem
    });
});
```
## Item Rating Changed

Raised when the user long-presses any item in the chat list.

### ItemRatingChangedCommand

Executed when the user changes the rating (like / dislike) of a response item.

### ItemRatingChangedEventArgs

| Property | Type | Description |
|---|---|---|
| `ResponseItem ` | `IAssistItem` | Instance of corresponding AssistItem class |
| `IsLiked` | `Boolean` | Holds the value of IsLiked property |



```xml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           ItemRatingChangedCommand="{Binding RatingChangedCommand}" />
```

```csharp
public Command<object> RatingChangedCommand { get; } = new Command<object>(obj =>
{
    // obj contains the updated rating info
    // log rating, send feedback to service, etc.
});
```
