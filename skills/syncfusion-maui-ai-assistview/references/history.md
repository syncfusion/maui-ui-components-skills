# Conversation History in SfAIAssistView

`SfAIAssistView` includes a built-in conversation history panel accessible from the toolbar. It lets users revisit and restore past chat sessions.

## Table of Contents
- [Enable Conversation History](#enable-conversation-history)
- [Binding Conversation Items](#binding-conversation-items)
- [Conversation Header and Empty View](#conversation-header-and-empty-view)
- [Handling Conversation Item Taps](#handling-conversation-item-taps)

---

## Enable Conversation History

The history feature is enabled by default. Set `EnableConversationHistory` to `false` to disable the history panel in the toolbar.

```xml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           EnableConversationHistory="True" />
```

```csharp
sfAIAssistView.EnableConversationHistory = true;
```

---

## Binding Conversation Items

Use `ConversationItemsSource` to populate the history panel. Bind an `ObservableCollection<AssistConversationItem>` from your ViewModel. The collection also updates at runtime when new requests are made.

### AssistConversationItem Properties

| Property | Type | Description |
|---|---|---|
| `Title` | `string` | Display title for the conversation entry |
| `DateTime` | `DateTime` | Timestamp shown in the history list |
| `AssistItems` | `ObservableCollection<IAssistItem>` | The full message thread for this conversation |

### ViewModel

```csharp
using Syncfusion.Maui.AIAssistView;

public class AIAssistViewModel : INotifyPropertyChanged
{
    private ObservableCollection<AssistConversationItem> conversationItems;

    public ObservableCollection<AssistConversationItem> ConversationItems
    {
        get => conversationItems;
        set
        {
            conversationItems = value;
            OnPropertyChanged(nameof(ConversationItems));
        }
    }

    public AIAssistViewModel()
    {
        conversationItems = new ObservableCollection<AssistConversationItem>();
        InitializeConversationHistory();
    }

    private void InitializeConversationHistory()
    {
        DateTime baseTime = DateTime.Now;

        string[] topics = { "What is .NET MAUI?", "Types of Listening" };
        string[] responses =
        {
            "MAUI stands for .NET Multi-platform App UI. It allows cross-platform apps from a single codebase.",
            "Common types of listening are Active listening and Passive listening."
        };

        for (int i = 0; i < topics.Length; i++)
        {
            var request = new AssistItem
            {
                Text = topics[i],
                IsRequested = true,
                DateTime = baseTime.AddDays(-i)
            };

            var response = new AssistItem
            {
                Text = responses[i],
                IsRequested = false,
                DateTime = baseTime.AddDays(-i),
                RequestItem = request
            };

            conversationItems.Add(new AssistConversationItem
            {
                Title = topics[i],
                DateTime = baseTime.AddDays(-i),
                AssistItems = new ObservableCollection<IAssistItem> { request, response }
            });
        }
    }
}
```

### XAML Binding

```xml
<ContentPage xmlns:syncfusion="clr-namespace:Syncfusion.Maui.AIAssistView;assembly=Syncfusion.Maui.AIAssistView"
             xmlns:local="clr-namespace:MyApp.ViewModels">

    <ContentPage.BindingContext>
        <local:AIAssistViewModel />
    </ContentPage.BindingContext>

    <syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                               AssistItems="{Binding AssistItems}"
                               ConversationItemsSource="{Binding ConversationItems}" />
</ContentPage>
```

### C# Binding

```csharp
var viewModel = new AIAssistViewModel();
sfAIAssistView.AssistItems = viewModel.AssistItems;
sfAIAssistView.ConversationItemsSource = viewModel.ConversationItems;
```

---

## Conversation Header and Empty View

### ConversationHeaderText

Sets the label shown above the history list. Defaults to `string.Empty`.

```xml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           ConversationHeaderText="Chat History" />
```

```csharp
sfAIAssistView.ConversationHeaderText = "Chat History";
```

### ConversationEmptyView

Shown when there are no conversation items. Accepts a `string` or a custom view.

```xml
<!-- String empty view -->
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           AssistItems="{Binding AssistItems}"
                           ConversationEmptyView="No conversations available" />
```

```csharp
// String
sfAIAssistView.ConversationEmptyView = "No conversations available";

// Custom view
sfAIAssistView.ConversationEmptyView = new Label
{
    Text = "No conversations yet.",
    HorizontalOptions = LayoutOptions.Center,
    VerticalOptions = LayoutOptions.Center,
    TextColor = Colors.Gray
};
```

---

## Handling Conversation Item Taps

When a user selects an item in the history panel, `ConversationItemTapped` and `ConversationItemTappedCommand` are raised.

### ConversationItemTappedEventArgs

| Property | Type | Description |
|---|---|---|
| `ConversationItem` | `AssistConversationItem` | The selected conversation entry |
| `Handled` | `bool` | Set to `true` to suppress the default behavior (automatically show the conversation items). Default: `false` |

### Event

```xml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           ConversationItemTapped="OnConversationItemTapped" />
```

```csharp
sfAIAssistView.ConversationItemTapped += OnConversationItemTapped;

private void OnConversationItemTapped(object sender, ConversationItemTappedEventArgs e)
{
    // e.ConversationItem — the selected history entry
    // e.Handled — set true to prevent the default restore behavior
    var selected = e.ConversationItem;
    sfAIAssistView.AssistItems = selected.AssistItems;
    e.Handled = true;
}
```

### Command (MVVM)

```xml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           ConversationItemTappedCommand="{Binding ConversationItemTappedCommand}" />
```

```csharp
public class AIAssistViewModel : INotifyPropertyChanged
{
    public ICommand ConversationItemTappedCommand { get; }

    public AIAssistViewModel()
    {
        ConversationItemTappedCommand = new Command<ConversationItemTappedEventArgs>(OnConversationItemTapped);
    }

    private void OnConversationItemTapped(ConversationItemTappedEventArgs e)
    {
        // Restore session from e.ConversationItem
        AssistItems = e.ConversationItem.AssistItems;
    }
}
```
