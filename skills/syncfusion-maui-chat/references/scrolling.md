# Scrolling in SfChat

Covers programmatic scrolling, auto-scroll behavior, the scroll-to-bottom button, and the `Scrolled` event in the Syncfusion .NET MAUI Chat control (`SfChat`).

## Table of Contents
- [Scroll to a Specific Message](#scroll-to-a-specific-message)
- [Auto-Scroll to Bottom](#auto-scroll-to-bottom)
- [Scroll to Bottom Button](#scroll-to-bottom-button)
  - [Customizing the Scroll to Bottom Button](#customizing-the-scroll-to-bottom-button)
- [Scrolled Event](#scrolled-event)

---

## Scroll to a Specific Message

Use `SfChat.ScrollToMessage(object)` to programmatically scroll to any message in the collection.

**XAML:**
```xml
<StackLayout>
    <Button x:Name="ScrollTo" Text="Scroll to message" HeightRequest="100" Clicked="ScrollTo_Clicked" />
    <sfChat:SfChat x:Name="sfChat"
        Messages="{Binding Messages}"
        CurrentUser="{Binding CurrentUser}"
        CanAutoScrollToBottom="False"/>
</StackLayout>
```

**C#:**
```csharp
private void ScrollTo_Clicked(object sender, EventArgs e)
{
    // Scroll to the sixth message in the collection.
    this.sfChat.ScrollToMessage(this.viewModel.Messages[5]);
}
```

> Pass the actual message object (e.g., a `TextMessage` instance) — not the index. Use the index only to look up the object from your `Messages` collection.

---

## Auto-Scroll to Bottom

By default, `SfChat` scrolls to the bottom whenever a new message is added. To disable this behavior, set `CanAutoScrollToBottom` to `false`.

**XAML:**
```xml
<sfChat:SfChat x:Name="sfChat"
               Messages="{Binding Messages}"
               CurrentUser="{Binding CurrentUser}"
               CanAutoScrollToBottom="False"/>
```

Use this when you want to preserve the user's scroll position after new messages arrive (e.g., the user is reading older messages).

---

## Scroll to Bottom Button

Display a floating button that lets users jump back to the latest message after scrolling up. Enable it with `ShowScrollToBottomButton="True"`.

**XAML:**
```xml
<sfChat:SfChat x:Name="sfChat"
               Messages="{Binding Messages}"
               CurrentUser="{Binding CurrentUser}"
               ShowScrollToBottomButton="True"/>
```

**C#:**
```csharp
SfChat sfChat = new SfChat();
ViewModel viewModel = new ViewModel();
sfChat.Messages = viewModel.Messages;
sfChat.CurrentUser = viewModel.CurrentUser;
sfChat.ShowScrollToBottomButton = true;
```

The button appears automatically when the user scrolls up through older messages and hides once they reach the bottom.

### Customizing the Scroll to Bottom Button

Replace the default button appearance using `ScrollToBottomButtonTemplate`.

**XAML:**
```xml
<ContentPage.Resources>
    <ResourceDictionary>
        <DataTemplate x:Key="scrollToBottomButtonTemplate">
            <Grid>
                <Label Text="↓"
                       FontSize="30"
                       FontAttributes="Bold"
                       HorizontalOptions="Center"
                       VerticalOptions="Center" />
            </Grid>
        </DataTemplate>
    </ResourceDictionary>
</ContentPage.Resources>

<sfChat:SfChat x:Name="sfChat"
               Messages="{Binding Messages}"
               CurrentUser="{Binding CurrentUser}"
               ShowScrollToBottomButton="True"
               ScrollToBottomButtonTemplate="{StaticResource scrollToBottomButtonTemplate}"/>
```

**C#:**
```csharp
sfChat.ShowScrollToBottomButton = true;
sfChat.ScrollToBottomButtonTemplate = new DataTemplate(() =>
{
    var grid = new Grid();
    var label = new Label
    {
        Text = "↓",
        FontSize = 30,
        FontAttributes = FontAttributes.Bold,
        HorizontalOptions = LayoutOptions.Center,
        VerticalOptions = LayoutOptions.Center
    };
    grid.Children.Add(label);
    return grid;
});
```

---

## Scrolled Event

The `Scrolled` event fires whenever the chat is scrolled. Use `ChatScrolledEventArgs` to get the current scroll offset and whether the list has reached the top or bottom.

**Key use case:** Disable auto-scroll while the user is reading older messages, then re-enable it once they scroll back to the bottom.

**XAML:**
```xml
<sfChat:SfChat x:Name="sfChat"
               Messages="{Binding Messages}"
               CurrentUser="{Binding CurrentUser}"
               Scrolled="sfChat_Scrolled"/>
```

**C#:**
```csharp
sfChat.Scrolled += sfChat_Scrolled;

private void sfChat_Scrolled(object? sender, ChatScrolledEventArgs e)
{
    // Only auto-scroll to bottom when the user is already at the bottom.
    sfChat.CanAutoScrollToBottom = e.IsBottomReached;
}
```

**`ChatScrolledEventArgs` members:**

| Member | Description |
|---|---|
| `IsBottomReached` | `true` when scrolled to the end of the message list |
| `IsTopReached` | `true` when scrolled to the top of the message list |
| `ScrollOffset` | Current vertical scroll offset |
