# Items and Data Binding in SfAIAssistView

## Table of Contents
- [AssistItem Common Properties](#assistitem-common-properties)
- [Text Item](#text-item)
- [Hyperlink Item](#hyperlink-item)
- [Image Item](#image-item)
- [Card Item](#card-item)
- [Attachment Item](#attachment-item)
- [Tapped Events and Commands](#tapped-events-and-commands)
- [Error Response](#error-response)
- [Customizable Views](#customizable-views)
- [Custom Model Binding](#custom-model-binding)
- [Context Menus](#context-menus)

---

## AssistItem Common Properties

Every item in `SfAIAssistView` is an `AssistItem`. The following properties apply to all item types:

| Property | Type | Description |
|---|---|---|
| `Text` | `string` | The text content of the item |
| `IsRequested` | `bool` | `true` = user request (right-aligned), `false` = AI response (left-aligned) |
| `Profile` | `Profile` | User avatar and name. Set `Profile.Avatar` (ImageSource) and `Profile.Name` (string) |
| `DateTime` | `DateTime` | Timestamp shown on the item |
| `RequestItem` | `object` | Reference to the originating request item for a response |
| `Suggestion` | `AssistItemSuggestion` | Per-item suggestion list displayed below the response |
| `SuggestionHeaderText` | `string` | Label shown above the response suggestion list |
| `ShowAssistItemFooter` | `bool` | Show/hide the footer toolbar (Copy, Retry, Like, Dislike) for this item |
| `ErrorMessage` | `string` | Displays an error state instead of normal content |

```csharp
var item = new AssistItem
{
    Text = "Hello from AI",
    IsRequested = false,
    DateTime = DateTime.Now,
    ShowAssistItemFooter = true,
    Profile = new Profile
    {
        Name = "AI Assistant",
        Avatar = "ai_avatar.png"
    }
};
```

> Use `SfAIAssistView.CurrentUser` to set the profile for the person sending requests, so all request items share the same profile automatically.

---

## Text Item

`AssistItem` displays plain text content. It is the default item type for both requests and responses.

```xml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           AssistItems="{Binding AssistItems}" />
```

```csharp
// ViewModel.cs
private async void GenerateAssistItems()
{
    AssistItems.Add(new AssistItem
    {
        Text = "What is .NET MAUI?",
        IsRequested = true
    });

    await Task.Delay(1000).ConfigureAwait(true);

    AssistItems.Add(new AssistItem
    {
        Text = "MAUI stands for .NET Multi-platform App UI — a framework for " +
               "building cross-platform apps with a single C# codebase.",
        IsRequested = false
    });
}
```

---

## Hyperlink Item

`AssistHyperlinkItem` sends a URL as a message item. The control automatically fetches and displays the URL's thumbnail, title, and description.

```csharp
// ViewModel.cs
private async Task GetResultAsync(AssistItem requestItem)
{
    await Task.Delay(1000).ConfigureAwait(true);

    AssistItems.Add(new AssistHyperlinkItem
    {
        Text = "Here's a link to learn more about .NET MAUI:",
        Url = "https://dotnet.microsoft.com/en-us/apps/maui",
        IsRequested = false
    });
}
```

**Key property:** `Url` — the URL to display. Thumbnail, title, and description are fetched automatically from the URL metadata.

---

## Image Item

`AssistImageItem` displays an image as a conversation item.

```csharp
// ViewModel.cs
private async Task GetResultAsync(AssistItem requestItem)
{
    await Task.Delay(1000).ConfigureAwait(true);

    AssistItems.Add(new AssistImageItem
    {
        Text = "Here's the requested image:",
        Source = "bird01.png",         // ImageSource
        Size = new Size(300, 200),     // Display size
        Aspect = Aspect.AspectFit,
        IsRequested = false
    });
}
```

| Property | Type | Description |
|---|---|---|
| `Source` | `ImageSource` | Image source (file, URL, or resource) |
| `Size` | `Size` | Display width and height |
| `Aspect` | `Aspect` | Fill mode: `AspectFit`, `AspectFill`, `Fill` |

---

## Card Item

`AssistCardItem` displays a horizontal list of rich interactive cards. Each card can contain an image, title, subtitle, description, and action buttons.

```csharp
// ViewModel.cs
private void GenerateCards()
{
    var card1 = new Card
    {
        Title = "Miami",
        Subtitle = "Florida, USA",
        Description = "A vibrant city known for its beaches and nightlife.",
        Image = "miami.png"
    };
    card1.Buttons.Add(new CardButton { Title = "Choose", Value = "Miami" });

    var card2 = new Card
    {
        Title = "Las Vegas",
        Description = "The entertainment capital of the world.",
        Image = "lasvegas.png"
    };
    card2.Buttons.Add(new CardButton { Title = "Choose", Value = "Las Vegas" });

    AssistItems.Add(new AssistCardItem
    {
        Cards = new ObservableCollection<Card> { card1, card2 },
        IsRequested = false
    });
}
```

| Class | Property | Description |
|---|---|---|
| `Card` | `Image` | Card image source |
| `Card` | `Title` | Bold card title |
| `Card` | `Subtitle` | Secondary title text |
| `Card` | `Description` | Body description text |
| `Card` | `Buttons` | `IList<CardButton>` — action buttons on the card |
| `CardButton` | `Title` | Button label |
| `CardButton` | `Value` | Value passed when the button is tapped |

---

## Attachment Item

`AssistAttachmentItem` displays file or image attachments as a conversation item (distinct from the editor attachment preview).

```csharp
// ViewModel.cs
private async void GenerateAssistItems()
{
    var attachment1 = new AssistAttachment
    {
        FileName = "report.pdf",
        FileExtension = ".pdf",
        FilePath = "/local/path/report.pdf",
        FileSize = 204800,
    };

    AssistItems.Add(new AssistAttachmentItem
    {
        Text = "Please review these documents.",
        IsRequested = true,
        Attachments = new List<IAttachment> { attachment1 }
    });

    await Task.Delay(1000).ConfigureAwait(true);

    AssistItems.Add(new AssistItem
    {
        Text = "I have reviewed the documents. How can I help?",
        IsRequested = false
    });
}
```

`AssistAttachment` properties:

| Property | Description |
|---|---|
| `FileName` | Display name of the file |
| `FileSize` | Size in bytes |
| `FilePath` | Local file path |
| `FileExtension` | File extension (e.g., `.pdf`, `.xlsx`) |
| `FileContent` | `Stream` — raw file content |
| `FilePreviewIcon` | Custom preview icon for the file |

---

## Tapped Events and Commands

### ImageTapped / ImageTappedCommand

Triggered when a user taps an `AssistImageItem`. The `ImageTappedEventArgs` provides:
- `ImageItem` — the tapped `AssistImageItem`

```xml
<syncfusion:SfAIAssistView ImageTapped="OnImageTapped"
                           ImageTappedCommand="{Binding ImageTappedCommand}" />
```

```csharp
// Event
private void OnImageTapped(object sender, ImageTappedEventArgs e)
{
    DisplayAlert("Image", "Tapped: " + e.ImageItem.Source, "OK");
}

// Command (ViewModel)
ImageTappedCommand = new Command<object>(obj =>
{
    var args = obj as ImageTappedEventArgs;
    // args.ImageItem.Source
});
```

### CardTapped / CardTappedCommand

Triggered when a user taps a card or its button. The `CardTappedEventArgs` provides:
- `Card` — the selected `Card` object
- `Action` — the `CardButton` that was tapped
- `CardItem` — the parent `AssistCardItem`
- `Handled` — set `true` to suppress default behavior

```xml
<syncfusion:SfAIAssistView CardTapped="OnCardTapped"
                           CardTappedCommand="{Binding CardTappedCommand}" />
```

```csharp
// Event
private void OnCardTapped(object sender, CardTappedEventArgs e)
{
    DisplayAlert("Card", $"Selected: {e.Card.Title}, Action: {e.Action?.Value}", "OK");
}
```

### AttachmentTapped / AttachmentTappedCommand

Triggered when a user taps an attachment preview. The `AttachmentTappedEventArgs` provides:
- `Attachment` — the tapped `IAttachment`

```xml
<syncfusion:SfAIAssistView AttachmentTapped="OnAttachmentTapped"
                           AttachmentTappedCommand="{Binding AttachmentTappedCommand}" />
```

```csharp
// Event
private void OnAttachmentTapped(object sender, AttachmentTappedEventArgs e)
{
    DisplayAlert("Attachment", "Tapped: " + e.Attachment.FileName, "OK");
}
```

---

## Error Response

Display an error state on a response item by setting `AssistItem.ErrorMessage`. Use this when an AI service call fails.

```csharp
private async Task GetResultAsync(AssistItem requestItem)
{
    try
    {
        await Task.Delay(1000).ConfigureAwait(true);

        AssistItems.Add(new AssistItem
        {
            Text = "Here is the AI response.",
            IsRequested = false
        });
    }
    catch (Exception ex)
    {
        AssistItems.Add(new AssistItem
        {
            ErrorMessage = "An error occurred while processing your request. " +
                           "Please try again.",
            IsRequested = false
        });
    }
}
```

> When `ErrorMessage` is set, it is shown instead of `Text`. The normal item content is not rendered.

---

## Customizable Views

`SfAIAssistView` allows you to style or subclass individual content views without replacing the entire item template. Override `ControlTemplate` on the specific view type via a `Style` in `ResourceDictionary`.

### Request Views

| View Class | Represents |
|---|---|
| `RequestTextView` | User request text content |
| `RequestAssistImageView` | User request image content |
| `RequestHyperlinkUrlLabelView` | User request URL label area |
| `RequestHyperLinkDetailsViewFrameView` | User request URL details/preview frame |

### Response Views

| View Class | Represents |
|---|---|
| `ResponseTextView` | AI response text content |
| `ResponseAssistImageView` | AI response image content |
| `ResponseHyperlinkUrlLabelView` | AI response URL label area |
| `ResponseHyperLinkDetailsViewFrameView` | AI response URL details/preview frame |
| `ResponseCardView` | Container for card-based AI responses |
| `CardItemView` | A single card within a response |
| `CardButtonView` | An action button inside a card (exposes `Title`, `Value`) |

```xml
<ContentPage.Resources>
    <ResourceDictionary>
        <!-- Customize request text bubble -->
        <Style TargetType="syncfusion:RequestTextView">
            <Setter Property="ControlTemplate">
                <Setter.Value>
                    <ControlTemplate>
                        <Grid Padding="8" BackgroundColor="{DynamicResource SecondaryContainer}">
                            <Label Text="{Binding Text}"
                                   FontSize="13"
                                   TextColor="{DynamicResource OnSecondaryContainer}" />
                        </Grid>
                    </ControlTemplate>
                </Setter.Value>
            </Setter>
        </Style>

        <!-- Customize response text bubble -->
        <Style TargetType="syncfusion:ResponseTextView">
            <Setter Property="ControlTemplate">
                <Setter.Value>
                    <ControlTemplate>
                        <Grid Padding="10" BackgroundColor="{DynamicResource PrimaryContainer}">
                            <Label Text="{Binding Text}"
                                   FontSize="13"
                                   FontAttributes="Italic"
                                   TextColor="{DynamicResource OnPrimaryContainer}" />
                        </Grid>
                    </ControlTemplate>
                </Setter.Value>
            </Setter>
        </Style>
    </ResourceDictionary>
</ContentPage.Resources>

<syncfusion:SfAIAssistView AssistItems="{Binding AssistItems}" />
```

---

## Custom Model Binding

Use `ItemsSource` + `ItemsSourceConverter` when your data model doesn't use `AssistItem` directly. The converter maps your custom objects to `AssistItem` instances and back.

### 1. Define your model

```csharp
// Model.cs
public class ChatMessage : INotifyPropertyChanged
{
    public string Prompt { get; set; }
    public string Response { get; set; }
    public bool IsRequested { get; set; }
    public object PromptItem { get; set; }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void RaisePropertyChanged(string name) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

### 2. Implement IAssistItemConverter

```csharp
// AssistItemConverter.cs
public class AssistItemConverter : IAssistItemConverter
{
    public IAssistItem ConvertToAssistItem(object customItem, SfAIAssistView assistView)
    {
        var msg = customItem as ChatMessage;
        if (msg == null) return new AssistItem();

        return new AssistItem
        {
            Data = msg,                         // Stores original object reference
            IsRequested = msg.IsRequested,
            Text = msg.IsRequested ? msg.Prompt : msg.Response
        };
    }

    public object ConvertToData(object assistViewItem, SfAIAssistView assistView)
    {
        var item = assistViewItem as AssistItem;
        if (item == null) return new ChatMessage();

        return new ChatMessage
        {
            IsRequested = item.IsRequested,
            Prompt = item.IsRequested ? item.Text : null,
            Response = item.IsRequested ? null : item.Text
        };
    }
}
```

### 3. Bind in XAML

```xml
<ContentPage.Resources>
    <local:AssistItemConverter x:Key="converter" />
</ContentPage.Resources>

<syncfusion:SfAIAssistView
    ItemsSource="{Binding ChatMessages}"
    ItemsSourceConverter="{StaticResource converter}" />
```

### 4. Bind in C#

```csharp
sfAIAssistView.ItemsSource = viewModel.ChatMessages;
sfAIAssistView.ItemsSourceConverter = new AssistItemConverter();
```

> `AssistItem.Data` holds a reference to the original data object. Access it in templates or commands when you need the source model.

---

## Context Menus

Context menus allow custom actions on request or response items. They appear when the user taps the "More Options" icon on an item.

### Request Context Menu

Populate `RequestContextMenu` with `AssistContextMenuItem` instances. Each item inherits from `ActionButton` and exposes `Text`, `Icon`, `Command`, and `CommandParameter`. When the menu opens for a specific item, the control sets `AssistItem` on each menu item so commands can access the target.

```xml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           AssistItems="{Binding AssistItems}">
    <syncfusion:SfAIAssistView.RequestContextMenu>
        <syncfusion:AssistContextMenuItem
            Text="Edit"
            Command="{Binding EditCommand}" />
        <syncfusion:AssistContextMenuItem
            Text="Delete"
            Command="{Binding DeleteCommand}" />
    </syncfusion:SfAIAssistView.RequestContextMenu>
</syncfusion:SfAIAssistView>
```

```csharp
// Code-behind or ViewModel
sfAIAssistView.RequestContextMenu = new ObservableCollection<AssistContextMenuItem>
{
    new AssistContextMenuItem
    {
        Text = "Copy",
        Command = new Command<object>(param =>
        {
            var menuItem = param as AssistContextMenuItem;
            var assistItem = menuItem?.AssistItem as AssistItem;
            if (assistItem != null)
                Clipboard.SetTextAsync(assistItem.Text);
        })
    }
};
```

### Response Context Menu

`ResponseContextMenu` follows the same pattern but targets response items.

```xml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           AssistItems="{Binding AssistItems}">
    <syncfusion:SfAIAssistView.ResponseContextMenu>
        <syncfusion:AssistContextMenuItem
            Text="Copy"
            Command="{Binding CopyCommand}" />
        <syncfusion:AssistContextMenuItem
            Text="Retry"
            Command="{Binding RetryCommand}" />
    </syncfusion:SfAIAssistView.ResponseContextMenu>
</syncfusion:SfAIAssistView>
```

```csharp
sfAIAssistView.ResponseContextMenu = new ObservableCollection<AssistContextMenuItem>
{
    new AssistContextMenuItem { Text = "Copy", Command = copyCommand },
    new AssistContextMenuItem { Text = "Like",  Command = likeCommand }
};
```

### Custom Menu Item Template

Use `RequestContextMenuItemTemplate` / `ResponseContextMenuItemTemplate` to define a custom layout for each menu item. Bind `AssistItem` to `CommandParameter` to give the command access to the target item.

```xml
<syncfusion:SfAIAssistView.ResponseContextMenuItemTemplate>
    <DataTemplate>
        <Grid Padding="8">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="Auto" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>
            <Image Source="{Binding Icon}" HeightRequest="20" WidthRequest="20" />
            <Label Grid.Column="1" Text="{Binding Text}" VerticalOptions="Center" />
            <Grid.GestureRecognizers>
                <TapGestureRecognizer
                    Command="{Binding Command}"
                    CommandParameter="{Binding AssistItem}" />
            </Grid.GestureRecognizers>
        </Grid>
    </DataTemplate>
</syncfusion:SfAIAssistView.ResponseContextMenuItemTemplate>
```

Use `RequestContextMenuPanelTemplate` / `ResponseContextMenuPanelTemplate` to customize the popup panel container that wraps all menu items.

### ContextMenuOpening Event

Subscribe to `ContextMenuOpening` to modify the menu list or cancel it before it appears. The event provides `IList<AssistContextMenuItem>` — you can add, remove, or update items at runtime.

```csharp
sfAIAssistView.ContextMenuOpening += OnContextMenuOpening;

private void OnContextMenuOpening(object sender, ContextMenuOpeningEventArgs e)
{
    // Conditionally remove an item
    var retryItem = e.MenuItems.FirstOrDefault(m => m.Text == "Retry");
    if (retryItem != null)
        e.MenuItems.Remove(retryItem);

    // Cancel the menu entirely
    // e.Cancel = true;
}
```

### AssistContextMenuItem Properties

| Property | Type | Description |
|---|---|---|
| `Text` | `string` | Menu item label |
| `Icon` | `ImageSource` | Optional icon shown beside the text |
| `Command` | `ICommand` | Executed when the menu item is tapped |
| `CommandParameter` | `object` | Parameter passed to the command. If `null`, the `AssistContextMenuItem` itself is passed |
| `AssistItem` | `IAssistItem` | Set by the control — the item the menu was opened for |
