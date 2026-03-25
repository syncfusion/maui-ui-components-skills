# Advanced Features

## Table of Contents
- [Message Swiping](#message-swiping)
  - [Enable Swiping](#enable-swiping)
  - [Swipe Templates](#swipe-templates)
  - [Swipe Events](#swipe-events)
  - [Reset Swipe Programmatically](#reset-swipe-programmatically)
- [Time Break](#time-break)
  - [Enable Time Break](#enable-time-break)
  - [Sticky Time Break](#sticky-time-break)
  - [Custom Time Break Template](#custom-time-break-template)
- [Attachment Button](#attachment-button)
  - [Show Attachment Button](#show-attachment-button)
  - [Attachment Button Event and Command](#attachment-button-event-and-command)
  - [Custom Attachment Button Template](#custom-attachment-button-template)
- [Liquid Glass Effect](#liquid-glass-effect)
- [Key Properties](#key-properties)

---

## Message Swiping

### Enable Swiping

Set `AllowSwiping="True"` to allow users to swipe left or right on messages. Use `MaxSwipeOffset` to limit how far the swipe travels.

```xaml
<sfchat:SfChat x:Name="sfChat"
               Messages="{Binding Messages}"
               CurrentUser="{Binding CurrentUser}"
               AllowSwiping="True"
               MaxSwipeOffset="130" />
```

```csharp
sfChat.AllowSwiping = true;
sfChat.MaxSwipeOffset = 130;
```

---

### Swipe Templates

- `StartSwipeTemplate` — shown when swiping **right** (start direction)
- `EndSwipeTemplate` — shown when swiping **left** (end direction)

```xaml
<sfchat:SfChat AllowSwiping="True">
    <sfchat:SfChat.StartSwipeTemplate>
        <DataTemplate>
            <Grid BackgroundColor="#009EDA" Padding="9">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="50"/>
                    <ColumnDefinition Width="50"/>
                </Grid.ColumnDefinitions>
                <Image Grid.Column="0" Source="edit.png" HorizontalOptions="Center"/>
                <Label Grid.Column="1" Text="EDIT" TextColor="White"
                       VerticalTextAlignment="Center"/>
            </Grid>
        </DataTemplate>
    </sfchat:SfChat.StartSwipeTemplate>

    <sfchat:SfChat.EndSwipeTemplate>
        <DataTemplate>
            <Grid BackgroundColor="#DC595F" Padding="4">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="40"/>
                    <ColumnDefinition Width="60"/>
                </Grid.ColumnDefinitions>
                <Image Grid.Column="0" Source="delete.png" HorizontalOptions="Center"/>
                <Label Grid.Column="1" Text="DELETE" TextColor="White"
                       VerticalTextAlignment="Center"/>
            </Grid>
        </DataTemplate>
    </sfchat:SfChat.EndSwipeTemplate>
</sfchat:SfChat>
```

---

### Swipe Events

| Event | Args | Key Properties |
|---|---|---|
| `SwipeStarted` | `MessageSwipeStartedEventArgs` | `Message`, `SwipeDirection`, `Cancel` |
| `Swiping` | `MessageSwipingEventArgs` | `Message`, `SwipeDirection`, `SwipeOffSet`, `Handled` |
| `SwipeEnded` | `MessageSwipeEndedEventArgs` | `Message`, `SwipeDirection`, `SwipeOffSet` |

**Cancel a swipe before it starts:**
```csharp
private void sfChat_SwipeStarted(object sender, MessageSwipeStartedEventArgs e)
{
    // Prevent swiping the second message
    if (sfChat.Messages.IndexOf(e.Message) == 1)
        e.Cancel = true;
}
```

**Freeze swipe offset mid-swipe:**
```csharp
private void sfChat_Swiping(object sender, MessageSwipingEventArgs e)
{
    // Lock swipe at 70 for the second message
    if (sfChat.Messages.IndexOf(e.Message) == 1 && e.SwipeOffset > 70)
        e.Handled = true;
}
```

**React when swipe completes:**
```csharp
private void sfChat_SwipeEnded(object sender, MessageSwipeEndedEventArgs e)
{
    if (e.SwipeOffset > 100)
        sfChat.ResetSwipeOffset();
}
```

---

### Reset Swipe Programmatically

Call `ResetSwipeOffset()` to dismiss the swipe view without user interaction:

```csharp
sfChat.ResetSwipeOffset();
```

Typically called in `SwipeEnded` to snap back if the offset didn't reach a meaningful threshold.

---

## Time Break

### Enable Time Break

`ShowTimeBreak="True"` groups messages by date, inserting a separator row between groups.

```xaml
<sfchat:SfChat ShowTimeBreak="True"
               Messages="{Binding Messages}"
               CurrentUser="{Binding CurrentUser}" />
```

```csharp
sfChat.ShowTimeBreak = true;
```

> ⚠️ **Incompatible with `LoadMoreOption`** — do not use `ShowTimeBreak="True"` together with load-more; the grouping and lazy loading conflict.

---

### Sticky Time Break

`StickyTimeBreak="True"` keeps the current date separator pinned at the top while scrolling, similar to section headers in a contacts list.

```xaml
<sfchat:SfChat ShowTimeBreak="True"
               StickyTimeBreak="True" />
```

```csharp
sfChat.ShowTimeBreak = true;
sfChat.StickyTimeBreak = true;
```

---

### Custom Time Break Template

Use `TimeBreakTemplate` to supply a `DataTemplateSelector` (or `DataTemplate`) that renders each date separator row. The binding context is a `GroupResult` whose `Key` is the date string.

```xaml
<ContentPage.Resources>
    <local:TimeBreakTemplateSelector x:Key="timeBreakTemplateSelector"/>
</ContentPage.Resources>

<sfchat:SfChat ShowTimeBreak="True"
               TimeBreakTemplate="{StaticResource timeBreakTemplateSelector}"
               Messages="{Binding Messages}"
               CurrentUser="{Binding CurrentUser}" />
```

```csharp
// DataTemplateSelector that returns different templates for Today/Yesterday/older dates
internal class TimeBreakTemplateSelector : DataTemplateSelector
{
    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        string dateString = (item as GroupResult).Key.ToString();
        DateTime groupedDate = DateTime.ParseExact(dateString, "d/M/yyyy",
                                   CultureInfo.InvariantCulture);
        string label = groupedDate.Date == DateTime.Now.Date ? "TODAY"
                     : groupedDate.Date == DateTime.Now.Date.AddDays(-1) ? "Yesterday"
                     : groupedDate.ToString("dd MMMM yyyy");

        var lbl = new Label { Text = label, HorizontalTextAlignment = TextAlignment.Center };
        return new DataTemplate(() => new Border { Content = lbl });
    }
}
```

---

## Attachment Button

### Show Attachment Button

Show the attachment button to the left of the message editor:

```xaml
<sfchat:SfChat ShowAttachmentButton="True"
               Messages="{Binding Messages}"
               CurrentUser="{Binding CurrentUser}" />
```

```csharp
sfChat.ShowAttachmentButton = true;
```

---

### Attachment Button Event and Command

Use `AttachmentButtonClicked` (event) or `AttachmentButtonCommand` (MVVM) to respond when the attachment button is tapped. Typically used to open a file/image picker and inject a message.

**Event:**
```xaml
<sfchat:SfChat ShowAttachmentButton="True"
               AttachmentButtonClicked="Chat_AttachmentButtonClicked" />
```
```csharp
private void Chat_AttachmentButtonClicked(object sender, EventArgs e)
{
    // Open gallery or file picker, then add a message
    this.chat.Messages.Add(new ImageMessage()
    {
        Source = "photo.jpg",
        Author = new Author() { Name = "Alice", Avatar = "alice.png" },
        Text = "Shared photo",
    });
}
```

**Command (MVVM):**
```xaml
<sfchat:SfChat ShowAttachmentButton="True"
               AttachmentButtonCommand="{Binding AttachmentButtonCommand}"
               AttachmentButtonCommandParameter="myParam" />
```
```csharp
// ViewModel
public ICommand AttachmentButtonCommand { get; set; }

public ViewModel()
{
    AttachmentButtonCommand = new Command(OnAttachmentTapped);
}

private void OnAttachmentTapped(object args)
{
    Messages.Add(new ImageMessage()
    {
        Source = "photo.jpg",
        Author = new Author() { Name = "Alice", Avatar = "alice.png" },
        Text = "Shared photo",
    });
}
```

---

### Custom Attachment Button Template

Replace the default attachment icon with any custom view using `AttachmentButtonTemplate`. Specify `WidthRequest` explicitly when showing multiple icons.

```xaml
<sfchat:SfChat ShowAttachmentButton="True">
    <sfchat:SfChat.AttachmentButtonTemplate>
        <DataTemplate>
            <StackLayout WidthRequest="58" HeightRequest="17" Orientation="Horizontal">
                <Image Source="AttachmentIcon.jpg" WidthRequest="22" HeightRequest="17"
                       Margin="0,0,8,0"/>
                <Image Source="CameraIcon.jpg" WidthRequest="22" HeightRequest="17"/>
            </StackLayout>
        </DataTemplate>
    </sfchat:SfChat.AttachmentButtonTemplate>
</sfchat:SfChat>
```

---

## Liquid Glass Effect

Applies a translucent, glass-like visual to the entire chat UI. The effect wraps the control in `SfGlassEffectView` from `Syncfusion.Maui.Core`.

> **Platform requirements:** macOS 26+ or iOS 26+, .NET 10 only.

```xaml
xmlns:core="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
xmlns:chat="clr-namespace:Syncfusion.Maui.Chat;assembly=Syncfusion.Maui.Chat"

<Grid Background="#FF54A3CD">
    <core:SfGlassEffectView EffectType="Regular" CornerRadius="20">
        <chat:SfChat x:Name="chat"
                     Background="Transparent"
                     EnableLiquidGlassEffect="True"
                     Messages="{Binding Messages}"
                     CurrentUser="{Binding CurrentUser}" />
    </core:SfGlassEffectView>
</Grid>
```

```csharp
using Syncfusion.Maui.Core;
using Syncfusion.Maui.Chat;

var glassView = new SfGlassEffectView
{
    CornerRadius = 20,
    EffectType = LiquidGlassEffectType.Regular
};

var chat = new SfChat
{
    Background = Colors.Transparent,
    EnableLiquidGlassEffect = true,
};

glassView.Content = chat;
```

**Tips:**
- Set `Background="Transparent"` on `SfChat` so the glass tint shows through.
- The colorized background (e.g., `#FF54A3CD`) belongs on the parent `Grid`, not on `SfChat`.
- The glass effect is applied to all dependent sub-controls automatically.

---

## Key Properties

| Property | Type | Default | Description |
|---|---|---|---|
| `AllowSwiping` | `bool` | `false` | Enables swipe gestures on messages |
| `MaxSwipeOffset` | `double` | — | Maximum swipe travel distance in pixels |
| `StartSwipeTemplate` | `DataTemplate` | — | Template shown on right swipe |
| `EndSwipeTemplate` | `DataTemplate` | — | Template shown on left swipe |
| `ShowTimeBreak` | `bool` | `false` | Groups messages by date with separator rows |
| `StickyTimeBreak` | `bool` | `false` | Pins current date header while scrolling |
| `TimeBreakTemplate` | `DataTemplate` | — | Custom renderer for date separator rows |
| `ShowAttachmentButton` | `bool` | `false` | Shows the attachment button in the editor bar |
| `AttachmentButtonTemplate` | `DataTemplate` | — | Custom view replacing the default attachment icon |
| `AttachmentButtonCommand` | `ICommand` | — | MVVM command triggered by attachment button tap |
| `AttachmentButtonCommandParameter` | `object` | — | Parameter passed to `AttachmentButtonCommand` |
| `EnableLiquidGlassEffect` | `bool` | `false` | Applies translucent glass effect (iOS/macOS 26+, .NET 10) |
