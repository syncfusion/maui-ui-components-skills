# Templates and Content Customization in SfAIAssistView

Covers request/response item templates, template selectors for per-type rendering, full `ControlTemplate` replacement, and custom chat view overrides.

## Table of Contents
- [Request Item Template](#request-item-template)
- [Response Item Template](#response-item-template)
- [Control Template](#control-template)
- [Custom Chat View (CreateAssistChat)](#custom-chat-view-createassistchat)

---

## Request Item Template

Use `RequestItemTemplate` to customize the appearance of all request (user-sent) items, or use `RequestItemTemplateSelector` to apply different templates per item type.

### Simple DataTemplate

```xml
<ContentPage.Resources>
    <DataTemplate x:Key="customRequestTemplate">
        <Grid Padding="8">
            <Label Text="{Binding Text}" FontAttributes="Bold" />
        </Grid>
    </DataTemplate>
</ContentPage.Resources>

<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           AssistItems="{Binding AssistItems}"
                           RequestItemTemplate="{StaticResource customRequestTemplate}" />
```

### RequestItemTemplateSelector

Inherit from `RequestItemTemplateSelector` and override `OnSelectTemplate` to return different templates for different item types. Return `base.OnSelectTemplate(item, container)` to keep the built-in template for all other types.

#### Custom Item Model

```csharp
public class FileAssistItem : AssistItem, INotifyPropertyChanged
{
    private string fileName;
    private string fileType;

    public string FileName
    {
        get => fileName;
        set { fileName = value; OnPropertyChanged(nameof(FileName)); }
    }

    public string FileType
    {
        get => fileType;
        set { fileType = value; OnPropertyChanged(nameof(FileType)); }
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

#### Template Selector

```csharp
public class CustomRequestTemplateSelector : RequestItemTemplateSelector
{
    private readonly DataTemplate fileTemplate;

    public CustomRequestTemplateSelector()
    {
        fileTemplate = new DataTemplate(typeof(FileTemplate)); // your custom view
    }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        if (item is FileAssistItem)
            return fileTemplate;

        // Fall back to built-in template for standard AssistItems
        return base.OnSelectTemplate(item, container);
    }
}
```

#### Applying the Selector

```xml
<ContentPage.Resources>
    <local:CustomRequestTemplateSelector x:Key="requestSelector" />
</ContentPage.Resources>

<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           AssistItems="{Binding AssistItems}"
                           RequestItemTemplate="{StaticResource requestSelector}" />
```

```csharp
sfAIAssistView.RequestItemTemplate = new CustomRequestTemplateSelector();
```

#### ViewModel with Custom Item

```csharp
private async void GenerateAssistItems()
{
    // Custom FileAssistItem as request
    AssistItems.Add(new FileAssistItem
    {
        FileName = ".NET MAUI",
        FileType = "Document",
        IsRequested = true
    });

    await Task.Delay(1000);

    // Standard AssistItem as response
    AssistItems.Add(new AssistItem
    {
        Text = "You've uploaded a file about .NET MAUI. Ask me anything about it!",
        IsRequested = false
    });
}
```

---

## Response Item Template

Use `ResponseItemTemplate` to customize the appearance of all AI response items, or use `ResponseItemTemplateSelector` for per-type templates.

### ResponseItemTemplateSelector

Inherit from `ResponseItemTemplateSelector` and override `OnSelectTemplate`. Return `base.OnSelectTemplate` for standard items.

#### Custom Response Item Model

```csharp
public class DatePickerItem : AssistItem
{
    public DateTime SelectedDate { get; set; }
}
```

#### Template Selector

```csharp
public class CustomResponseTemplateSelector : ResponseItemTemplateSelector
{
    private readonly DataTemplate datePickerTemplate;

    public CustomResponseTemplateSelector()
    {
        datePickerTemplate = new DataTemplate(typeof(DatePickerTemplate)); // your custom view
    }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        if (item is DatePickerItem)
            return datePickerTemplate;

        // Fall back to built-in template for standard AssistItems
        return base.OnSelectTemplate(item, container);
    }
}
```

#### Applying the Selector

```xml
<ContentPage.Resources>
    <local:CustomResponseTemplateSelector x:Key="responseSelector" />
</ContentPage.Resources>

<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           AssistItems="{Binding AssistItems}"
                           ResponseItemTemplate="{StaticResource responseSelector}" />
```

```csharp
sfAIAssistView.ResponseItemTemplate = new CustomResponseTemplateSelector();
```

#### ViewModel with Custom Response

```csharp
private async void GenerateAssistItems()
{
    AssistItems.Add(new AssistItem { Text = "Yes, schedule with Dr. Harry tomorrow.", IsRequested = true });

    await Task.Delay(1000);

    // DatePickerItem rendered by custom template selector
    AssistItems.Add(new DatePickerItem
    {
        Text = "Choose a date for the consultation",
        IsRequested = false,
        SelectedDate = DateTime.Today
    });
}
```

---

## Control Template

`ControlTemplate` allows complete replacement of the `SfAIAssistView` visual structure. Use this when you need to composite the chat view with entirely custom views (e.g., a compose mode panel alongside the chat panel).

```xml
<local:CustomAssistView x:Name="sfAIAssistView"
                        AssistItems="{Binding AssistItems}">
    <local:CustomAssistView.ControlTemplate>
        <ControlTemplate>
            <ContentView>
                <Grid>
                    <!-- Built-in chat view, toggled by ViewModel -->
                    <ContentView IsVisible="{Binding IsActiveChatView}"
                                 Content="{TemplateBinding AssistChatView}"
                                 BindingContext="{TemplateBinding BindingContext}" />

                    <!-- Custom compose panel -->
                    <local:ComposeView IsVisible="{Binding IsActiveComposeView}"
                                      BindingContext="{TemplateBinding BindingContext}" />
                </Grid>
            </ContentView>
        </ControlTemplate>
    </local:CustomAssistView.ControlTemplate>
</local:CustomAssistView>
```

> **Note:** `AssistChatView` is a `TemplateBinding` property exposing the internal chat view so it can be hosted inside a custom `ControlTemplate`.

---

## Custom Chat View (CreateAssistChat)

Override `CreateAssistChat` on a subclass of `SfAIAssistView` to return a custom `AssistViewChat` instance. This gives full control over chat message display, input view visibility, and interaction behavior.

### Subclass SfAIAssistView

```csharp
public class CustomAIAssistView : SfAIAssistView
{
    protected override AssistViewChat CreateAssistChat()
    {
        // Return a custom AssistViewChat implementation
        return new CustomAssistViewChat(this);
    }
}
```

### Custom AssistViewChat

`AssistViewChat` is the internal chat panel. Override it to adjust behavior — for example, hiding the message input view.

```csharp
public class CustomAssistViewChat : AssistViewChat
{
    public CustomAssistViewChat(SfAIAssistView assistView) : base(assistView)
    {
        // Hide the default message input view
        this.ShowMessageInputView = false;
    }
}
```

### Usage in XAML

```xml
<local:CustomAIAssistView x:Name="sfAIAssistView"
                          AssistItems="{Binding AssistItems}" />
```

> **When to use:** Use `CreateAssistChat` when you need to inject custom interaction logic into the chat panel itself (e.g., removing the input bar and using a completely custom input view from outside the control).
