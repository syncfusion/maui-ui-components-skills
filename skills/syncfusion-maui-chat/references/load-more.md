# Load More in .NET MAUI Chat (SfChat)

## Table of Contents
- [Load More Options](#load-more-options)
- [Manual Load More](#manual-load-more)
- [Automatic Load More](#automatic-load-more)
- [Custom Load More Template](#custom-load-more-template)
- [Key Properties](#key-properties)

---

The load-more feature lets users fetch older message history by scrolling to the top of the chat — either automatically or via a manual button tap.

> ⚠️ Load More is **not compatible** with `ShowTimeBreak="True"`. Disable time-break grouping when using load more.

---

## Load More Options

| `LoadMoreBehavior` | Behaviour |
|---|---|
| `LoadMoreOption.Manual` | A "Load More" button appears at the top. Tapping it triggers `LoadMoreCommand`. |
| `LoadMoreOption.Auto` | Busy indicator appears automatically when the user scrolls to the top. `LoadMoreCommand` runs without a button tap. |
| `LoadMoreOption.None` | Disables load more. Set this to cancel an in-progress auto load. |

---

## Manual Load More

**XAML:**
```xml
<sfChat:SfChat LoadMoreBehavior="Manual"
               LoadMoreCommand="{Binding LoadMoreCommand}"
               IsLazyLoading="{Binding IsBusy}"
               Messages="{Binding Messages}"
               CurrentUser="{Binding CurrentUser}" />
```

**ViewModel:**
```csharp
public class LoadMoreViewModel : INotifyPropertyChanged
{
    private bool isBusy;
    public ICommand LoadMoreCommand { get; set; }

    public bool IsBusy
    {
        get => isBusy;
        set { isBusy = value; OnPropertyChanged(nameof(IsBusy)); }
    }

    public LoadMoreViewModel()
    {
        LoadMoreCommand = new Command<object>(LoadMoreItems, CanLoadMoreItems);
    }

    private bool CanLoadMoreItems(object obj)
    {
        // Return false when there are no more messages to load
        return OldMessages.Count > 0;
    }

    private async void LoadMoreItems(object obj)
    {
        try
        {
            IsBusy = true;             // Show busy indicator
            await Task.Delay(1500);    // Simulate network request
            InsertOlderMessages();
        }
        finally
        {
            IsBusy = false;            // Hide busy indicator
        }
    }

    private void InsertOlderMessages()
    {
        // Insert up to 10 older messages at the top
        for (int i = 0; i < 10 && OldMessages.Count > 0; i++)
        {
            var msg = OldMessages[OldMessages.Count - 1];
            Messages.Insert(0, msg);
            OldMessages.Remove(msg);
        }
    }
}
```

---

## Automatic Load More

With `Auto`, the command executes as soon as the user reaches the top — no button needed.

```xml
<sfChat:SfChat LoadMoreBehavior="Auto"
               LoadMoreCommand="{Binding LoadMoreCommand}"
               IsLazyLoading="{Binding IsBusy}"
               Messages="{Binding Messages}"
               CurrentUser="{Binding CurrentUser}" />
```

**Stop auto load when all messages are loaded:**
```csharp
private bool CanLoadMoreItems(object obj)
{
    if (OldMessages.Count > 0)
        return true;

    // No more messages — cancel auto load
    LoadMoreBehavior = LoadMoreOption.None;
    return false;
}
```

> When `LoadMoreBehavior` is `Auto`, `IsLazyLoading` is `true` by default. Set `LoadMoreBehavior = LoadMoreOption.None` to stop and remove the loading view.

---

## Custom Load More Template

Replace the default button/indicator with a fully custom view using `LoadMoreTemplate`:

```xml
<sfChat:SfChat LoadMoreCommand="{Binding LoadMoreCommand}"
               IsLazyLoading="{Binding IsBusy}"
               LoadMoreBehavior="{Binding LoadMoreBehavior}"
               Messages="{Binding Messages}"
               CurrentUser="{Binding CurrentUser}">
    <sfChat:SfChat.LoadMoreTemplate>
        <DataTemplate>
            <Grid HeightRequest="50" HorizontalOptions="Center">
                <!-- Show button when not loading -->
                <Label Text="Fetch Older Messages"
                       IsVisible="{Binding IsLazyLoading,
                                   Converter={StaticResource InverseBool},
                                   Source={x:Reference sfChat}}" />
                <!-- Show spinner when loading -->
                <ActivityIndicator IsRunning="{Binding IsLazyLoading, Source={x:Reference sfChat}}"
                                   IsVisible="{Binding IsLazyLoading, Source={x:Reference sfChat}}" />
            </Grid>
        </DataTemplate>
    </sfChat:SfChat.LoadMoreTemplate>
</sfChat:SfChat>
```

---

## Key Properties

| Property | Type | Purpose |
|---|---|---|
| `LoadMoreBehavior` | `LoadMoreOption` | `Manual`, `Auto`, or `None` |
| `LoadMoreCommand` | `ICommand` | Executed to fetch older messages |
| `LoadMoreCommandParameter` | `object` | Optional parameter passed to command |
| `IsLazyLoading` | `bool` | Shows busy indicator when `true` |
| `LoadMoreTemplate` | `DataTemplate` | Custom load more view |
