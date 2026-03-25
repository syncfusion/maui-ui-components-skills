# Suggestions in SfAIAssistView

## Table of Contents
- [Common Suggestions (Header)](#common-suggestions-header)
- [Response Item Suggestions](#response-item-suggestions)
- [Footer Suggestions (Editor)](#footer-suggestions-editor)
- [SuggestionItemSelected Event and Command](#suggestionitemselected-event-and-command)

---

## Common Suggestions (Header)

Common suggestions appear under the header view as quick-start prompts. They help users explore topics without typing.

> **Requirement:** `ShowHeader` must be `true` for `Suggestions` to be visible.

### Displaying Common Suggestions

Populate `SfAIAssistView.Suggestions` with a list of `AssistSuggestion` objects. Each suggestion has a `Text` label and an optional `ImageSource` icon.

```csharp
// ViewModel.cs
using Syncfusion.Maui.AIAssistView;

public class ViewModel : INotifyPropertyChanged
{
    private ObservableCollection<ISuggestion> suggestions;

    public ObservableCollection<ISuggestion> Suggestions
    {
        get => suggestions;
        set
        {
            suggestions = value;
            RaisePropertyChanged(nameof(Suggestions));
        }
    }

    public ViewModel()
    {
        Suggestions = new ObservableCollection<ISuggestion>
        {
            new AssistSuggestion { Text = "Ownership",     ImageSource = "ownership.png" },
            new AssistSuggestion { Text = "Brainstorming", ImageSource = "brainstorming.png" },
            new AssistSuggestion { Text = "Listening",     ImageSource = "listening.png" },
            new AssistSuggestion { Text = "Resilience",    ImageSource = "resilience.png" }
        };
    }
}
```

```xml
<!-- MainPage.xaml -->
<syncfusion:SfAIAssistView
    AssistItems="{Binding AssistItems}"
    Suggestions="{Binding Suggestions}"
    ShowHeader="True" />
```

```csharp
// Code-behind
sfAIAssistView.Suggestions = viewModel.Suggestions;
sfAIAssistView.ShowHeader = true;
```

### Common Suggestion Template

Use `SuggestionTemplate` to fully customize the appearance of each suggestion chip in the header area.

```xml
<ContentPage.Resources>
    <DataTemplate x:Key="suggestionTemplate">
        <Border Padding="8,6" StrokeShape="RoundRectangle 20">
            <HorizontalStackLayout Spacing="6">
                <Image Source="{Binding ImageSource}"
                       HeightRequest="18" WidthRequest="18"
                       IsVisible="{Binding ImageSource, Converter={StaticResource NotNullConverter}}" />
                <Label Text="{Binding Text}"
                       VerticalOptions="Center"
                       FontSize="13" />
            </HorizontalStackLayout>
        </Border>
    </DataTemplate>
</ContentPage.Resources>

<syncfusion:SfAIAssistView
    AssistItems="{Binding AssistItems}"
    Suggestions="{Binding Suggestions}"
    SuggestionTemplate="{StaticResource suggestionTemplate}"
    ShowHeader="True" />
```

```csharp
sfAIAssistView.SuggestionTemplate = CreateSuggestionTemplate();

private DataTemplate CreateSuggestionTemplate()
{
    return new DataTemplate(() =>
    {
        var border = new Border { Padding = new Thickness(8, 6) };
        var label = new Label { FontSize = 13 };
        label.SetBinding(Label.TextProperty, "Text");
        border.Content = label;
        return border;
    });
}
```

---

## Response Item Suggestions

Response item suggestions appear below a specific AI response, offering the user follow-up options. They are supported for all response item types.

### Displaying Response Item Suggestions

Assign an `AssistItemSuggestion` to `AssistItem.Suggestion`. The `AssistItemSuggestion.Items` collection holds the `AssistSuggestion` list.

```csharp
// ViewModel.cs
private async Task GetResultAsync(AssistItem requestItem)
{
    await Task.Delay(1000).ConfigureAwait(true);

    var responseItem = new AssistItem
    {
        Text = "MAUI is a cross-platform UI framework for iOS, Android, macOS, and Windows.",
        IsRequested = false
    };

    // Build the suggestion list
    var itemSuggestion = new AssistItemSuggestion
    {
        Items = new ObservableCollection<ISuggestion>
        {
            new AssistSuggestion { Text = "Get started with .NET MAUI" },
            new AssistSuggestion { Text = "Build your first MAUI app" },
            new AssistSuggestion { Text = "Compare MAUI vs Xamarin.Forms" }
        }
    };

    responseItem.Suggestion = itemSuggestion;
    AssistItems.Add(responseItem);
}
```

### Adding Images to Response Suggestions

```csharp
new AssistSuggestion
{
    Text = "Get started with .NET MAUI",
    ImageSource = "learn_more.png"
}
```

### Changing Suggestion Orientation

`AssistItemSuggestion.Orientation` controls whether suggestions stack vertically (default) or scroll horizontally.

```csharp
var itemSuggestion = new AssistItemSuggestion
{
    Orientation = SuggestionsOrientation.Horizontal,  // or Vertical (default)
    Items = suggestions
};
```

### Changing Item Spacing

```csharp
var itemSuggestion = new AssistItemSuggestion
{
    ItemSpacing = 12,   // default is 8
    Items = suggestions
};
```

### Suggestion Header Text

Display a label above the suggestion list for a response item using `AssistItem.SuggestionHeaderText`.

```csharp
responseItem.SuggestionHeaderText = "Related Topics";
responseItem.Suggestion = itemSuggestion;
```

### Response Suggestion Template

Use `ResponseSuggestionTemplate` on `SfAIAssistView` to customize the appearance of individual suggestion chips in response items.

```xml
<ContentPage.Resources>
    <DataTemplate x:Key="responseSuggestionTemplate">
        <Border Padding="10,6" BackgroundColor="#EDE7F6" StrokeShape="RoundRectangle 16">
            <Label Text="{Binding Text}" TextColor="#4A148C" FontSize="12" />
        </Border>
    </DataTemplate>
</ContentPage.Resources>

<syncfusion:SfAIAssistView
    AssistItems="{Binding AssistItems}"
    ResponseSuggestionTemplate="{StaticResource responseSuggestionTemplate}" />
```

```csharp
sfAIAssistView.ResponseSuggestionTemplate = CreateResponseSuggestionTemplate();

private DataTemplate CreateResponseSuggestionTemplate()
{
    return new DataTemplate(() =>
    {
        var border = new Border
        {
            Padding = new Thickness(10, 6),
            BackgroundColor = Color.FromArgb("#EDE7F6")
        };
        var label = new Label { FontSize = 12 };
        label.SetBinding(Label.TextProperty, "Text");
        border.Content = label;
        return border;
    });
}
```

---

## Footer Suggestions (Editor)

Footer suggestions appear above the input editor area. They let users quickly compose messages from predefined prompts without typing.

### Displaying Footer Suggestions

Bind `FooterSuggestions` to an `IList<ISuggestion>`.

```xml
<syncfusion:SfAIAssistView
    AssistItems="{Binding AssistItems}"
    FooterSuggestions="{Binding FooterSuggestions}" />
```

```csharp
// ViewModel.cs
public ObservableCollection<ISuggestion> FooterSuggestions { get; set; }

public ViewModel()
{
    FooterSuggestions = new ObservableCollection<ISuggestion>
    {
        new AssistSuggestion { Text = "Summarize this" },
        new AssistSuggestion { Text = "Translate to Spanish" },
        new AssistSuggestion { Text = "Make it shorter" }
    };
}
```

```csharp
// Code-behind
sfAIAssistView.FooterSuggestions = viewModel.FooterSuggestions;
```

### Footer Suggestion Template

Use `FooterSuggestionTemplate` to define a custom layout for each footer suggestion chip.

```xml
<ContentPage.Resources>
    <DataTemplate x:Key="footerSuggestionTemplate">
        <Border Padding="10,6" BackgroundColor="#F3F3F3" StrokeShape="RoundRectangle 14">
            <Label Text="{Binding Text}" FontSize="12" TextColor="#333" />
        </Border>
    </DataTemplate>
</ContentPage.Resources>

<syncfusion:SfAIAssistView
    FooterSuggestions="{Binding FooterSuggestions}"
    FooterSuggestionTemplate="{StaticResource footerSuggestionTemplate}" />
```

```csharp
sfAIAssistView.FooterSuggestionTemplate = new DataTemplate(() =>
{
    var border = new Border { Padding = new Thickness(10, 6) };
    var label = new Label { FontSize = 12 };
    label.SetBinding(Label.TextProperty, new Binding("Text"));
    border.Content = label;
    return border;
});
```

---

## SuggestionItemSelected Event and Command

When a user taps any suggestion (header, response, or footer), `SuggestionItemSelected` / `SuggestionItemSelectedCommand` fires.

`SuggestionItemSelectedEventArgs` provides:

| Property | Type | Description |
|---|---|---|
| `SelectedItem` | `ISuggestion` | The suggestion the user tapped |
| `RequestItem` | `IAssistItem` | The request item that was sent (if auto-sent) |
| `CancelRequest` | `bool` | Set to `true` to prevent the suggestion from being auto-sent as a request |

### Using the Event

```csharp
sfAIAssistView.SuggestionItemSelected += OnSuggestionItemSelected;

private void OnSuggestionItemSelected(object sender, SuggestionItemSelectedEventArgs e)
{
    var selected = e.SelectedItem as AssistSuggestion;
    Console.WriteLine($"Selected: {selected?.Text}");

    // Prevent the suggestion from being auto-submitted
    // e.CancelRequest = true;
}
```

### Using the Command (MVVM)

```xml
<syncfusion:SfAIAssistView
    AssistItems="{Binding AssistItems}"
    SuggestionItemSelectedCommand="{Binding SuggestionItemSelectedCommand}" />
```

```csharp
// ViewModel.cs
public ICommand SuggestionItemSelectedCommand { get; }

public ViewModel()
{
    SuggestionItemSelectedCommand = new Command<object>(OnSuggestionSelected);
}

private void OnSuggestionSelected(object obj)
{
    var args = obj as SuggestionItemSelectedEventArgs;
    if (args == null) return;

    var text = (args.SelectedItem as AssistSuggestion)?.Text;

    // Place the suggestion text in the editor without auto-submitting
    args.CancelRequest = true;
    sfAIAssistView.InputText = text;
}
```

### Preventing Auto-Send

By default, tapping a suggestion immediately sends it as a request item. Set `CancelRequest = true` in the event or command handler to stop this and handle the selection manually (for example, populate the editor instead).

```csharp
private void OnSuggestionItemSelected(object sender, SuggestionItemSelectedEventArgs e)
{
    // Put suggestion text in editor rather than auto-sending
    e.CancelRequest = true;
    sfAIAssistView.InputText = (e.SelectedItem as AssistSuggestion)?.Text;
}
```
