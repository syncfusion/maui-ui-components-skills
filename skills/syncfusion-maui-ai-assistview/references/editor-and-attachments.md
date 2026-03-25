# Editor and Attachments in SfAIAssistView

Covers customizing the message input editor, editing previous requests, file attachments, quick action buttons, and the send (request) button.

## Table of Contents
- [Editor Configuration](#editor-configuration)
- [Edit Option (Editing Previous Requests)](#edit-option-editing-previous-requests)
- [Attachments](#attachments)
- [Action Buttons](#action-buttons)
- [Request Button](#request-button)

---

## Editor Configuration

### InputText

`InputText` gets or sets the current text in the editor programmatically.

```csharp
sfAIAssistView.InputText = "Pre-filled message";
```

### EditorViewTemplate

Fully replaces the default editor layout with a custom `DataTemplate`.

```xml
<ContentPage.Resources>
    <DataTemplate x:Key="editorViewTemplate">
        <Grid>
            <Editor x:Name="editor" Placeholder="Type Message..." />
        </Grid>
    </DataTemplate>
</ContentPage.Resources>

<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           EditorViewTemplate="{StaticResource editorViewTemplate}" />
```

```csharp
sfAIAssistView.EditorViewTemplate = new DataTemplate(() =>
{
    var grid = new Grid();
    var editor = new Editor { Placeholder = "Type Message..." };
    grid.Children.Add(editor);
    return grid;
});
```

### RequestEditor

Access and style the underlying editor control directly in code-behind (C# only).

```csharp
sfAIAssistView.RequestEditor.PlaceholderColor = Colors.Gray;
sfAIAssistView.RequestEditor.FontSize = 14;
```

### RequestEditorView

Use `RequestEditorView` to access and customize the full editor view wrapper (visual elements and overall appearance).

---

## Edit Option (Editing Previous Requests)

Users can edit a previously sent request and resubmit it to get a revised AI response.

**Interaction behavior:**
- **Mobile (Android / iOS):** Tap a request item to reveal the Edit option
- **Desktop (Windows / macOS):** Hover over a request item to reveal the Edit icon

When the Edit icon is tapped, the request text is placed back into the input editor (`InputView`) for the user to revise and resubmit.

> No additional properties need to be set — the edit option is built in.

---

## Attachments

`SfAIAssistView` supports file and image attachments shown as previews inside the editor before the message is sent.

### AssistAttachment Properties

| Property | Type | Description |
|---|---|---|
| `FileName` | `string` | Display name of the file |
| `FileSize` | `long` | File size in bytes |
| `FilePath` | `string` | Local file path |
| `FileExtension` | `string` | File type extension (e.g., `.pdf`) |
| `FileContent` | `Stream` | File content stream |
| `FilePreviewIcon` | `ImageSource` | Custom preview icon for the file |

### Binding Attachments

```xml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           Attachments="{Binding Attachments}" />
```

### ViewModel — Picking Files

```csharp
using Syncfusion.Maui.AIAssistView;

public class AIAssistViewModel : INotifyPropertyChanged
{
    private ObservableCollection<IAttachment> attachments;

    public ObservableCollection<IAttachment> Attachments
    {
        get => attachments;
        set { attachments = value; OnPropertyChanged(nameof(Attachments)); }
    }

    public ICommand UploadCommand { get; }

    public AIAssistViewModel()
    {
        attachments = new ObservableCollection<IAttachment>();
        UploadCommand = new Command(async () => await UploadFilesAsync());
    }

    private async Task UploadFilesAsync()
    {
        var results = await FilePicker.Default.PickMultipleAsync();
        if (results == null) return;

        foreach (var file in results)
        {
            var stream = await file.OpenReadAsync();
            attachments.Add(new AssistAttachment
            {
                FileName = file.FileName,
                FileSize = stream.CanSeek ? stream.Length : 0,
                FilePath = file.FullPath ?? string.Empty,
                FileExtension = Path.GetExtension(file.FileName) ?? string.Empty,
                FileContent = stream
            });
        }
    }
}
```

### MaxAttachmentCount

Limits the number of attachments. Default is `10`.

```xml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           Attachments="{Binding Attachments}"
                           MaxAttachmentCount="5" />
```

```csharp
sfAIAssistView.MaxAttachmentCount = 5;
```

### AttachmentItemTemplate

Customizes the preview chip shown for each attachment in the editor.

```xml
<ContentPage.Resources>
    <DataTemplate x:Key="attachmentItemTemplate">
        <Grid Padding="6">
            <Label Text="{Binding FileName}" FontSize="12" />
        </Grid>
    </DataTemplate>
</ContentPage.Resources>

<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           Attachments="{Binding Attachments}"
                           AttachmentItemTemplate="{StaticResource attachmentItemTemplate}" />
```

---

## Action Buttons

A quick action icon inside the editor that opens a popup of configurable actions (e.g., upload image, search web).

### Enable Action Buttons

```xml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           ShowActionButtons="True" />
```

```csharp
sfAIAssistView.ShowActionButtons = true;
```

### ActionButton Properties

| Property | Type | Description |
|---|---|---|
| `Text` | `string` | Label shown in the popup |
| `Icon` | `ImageSource` | Icon shown in the popup |
| `Command` | `ICommand` | Executed when the action is tapped |
| `CommandParameter` | `object` | Parameter passed to the command |

### Configuring ActionButtons

```xml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           ShowActionButtons="True"
                           AssistItems="{Binding AssistItems}">
    <syncfusion:SfAIAssistView.ActionButtons>
        <syncfusion:ActionButton BindingContext="{x:Reference viewModel}"
                                 Text="Upload images"
                                 Icon="image.png"
                                 Command="{Binding UploadCommand}" />
        <syncfusion:ActionButton BindingContext="{x:Reference viewModel}"
                                 Text="Search in web"
                                 Icon="web.png"
                                 Command="{Binding SearchCommand}" />
    </syncfusion:SfAIAssistView.ActionButtons>
</syncfusion:SfAIAssistView>
```

```csharp
sfAIAssistView.ShowActionButtons = true;
sfAIAssistView.ActionButtons = new ObservableCollection<ActionButton>
{
    new ActionButton
    {
        BindingContext = this.viewModel,
        Text = "Upload images",
        Icon = ImageSource.FromFile("image.png"),
        Command = viewModel.UploadCommand
    },
    new ActionButton
    {
        BindingContext = this.viewModel,
        Text = "Search in web",
        Icon = ImageSource.FromFile("web.png"),
        Command = viewModel.SearchCommand
    }
};
```

### ActionButtonIcon and ActionButtonPosition

`ActionButtonIcon` sets the icon for the quick action button in the editor (the icon users tap to open the popup).  
`ActionButtonPosition` controls placement: `ActionButtonPosition.Start` or `ActionButtonPosition.End`.

```xml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           ShowActionButtons="True"
                           ActionButtonIcon="dotmenu.png"
                           ActionButtonPosition="Start">
    <syncfusion:SfAIAssistView.ActionButtons>
        ...
    </syncfusion:SfAIAssistView.ActionButtons>
</syncfusion:SfAIAssistView>
```

```csharp
sfAIAssistView.ActionButtonIcon = ImageSource.FromFile("dotmenu.png");
sfAIAssistView.ActionButtonPosition = ActionButtonPosition.Start;
```

---

## Request Button

The send (request) button submits the current input text as a request.

### RequestButtonIcon

Sets a custom `ImageSource` for the send button icon.

```xml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           AssistItems="{Binding AssistItems}">
    <syncfusion:SfAIAssistView.RequestButtonIcon>
        <FontImageSource Glyph="&#xe809;"
                         FontFamily="MauiMaterialAssets"
                         Color="Black" />
    </syncfusion:SfAIAssistView.RequestButtonIcon>
</syncfusion:SfAIAssistView>
```

```csharp
sfAIAssistView.RequestButtonIcon = new FontImageSource
{
    Glyph = "\ue809",
    FontFamily = "MauiMaterialAssets",
    Color = Colors.Black
};
```

### RequestButtonTemplate

Fully replaces the send button with a custom `DataTemplate`.

```xml
<ContentPage.Resources>
    <DataTemplate x:Key="requestButtonTemplate">
        <Grid>
            <Label Text="&#xe791;"
                   FontFamily="MauiMaterialAssets"
                   FontSize="24"
                   HorizontalOptions="Center"
                   VerticalOptions="Center" />
        </Grid>
    </DataTemplate>
</ContentPage.Resources>

<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           AssistItems="{Binding AssistItems}"
                           RequestButtonTemplate="{StaticResource requestButtonTemplate}" />
```

```csharp
sfAIAssistView.RequestButtonTemplate = new DataTemplate(() =>
{
    var grid = new Grid();
    var label = new Label
    {
        Text = "\ue791",
        FontFamily = "MauiMaterialAssets",
        FontSize = 24,
        HorizontalOptions = LayoutOptions.Center,
        VerticalOptions = LayoutOptions.Center
    };
    grid.Children.Add(label);
    return grid;
});
```
