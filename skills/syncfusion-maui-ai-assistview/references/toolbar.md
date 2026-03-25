# Toolbar in SfAIAssistView

The toolbar appears at the top of `SfAIAssistView` and provides session-level controls including a title, a New Chat button, and an optional Temporary Chat mode.

## Table of Contents
- [Show and Configure Toolbar](#show-and-configure-toolbar)
- [New Chat Button](#new-chat-button)
- [Temporary Chat Mode](#temporary-chat-mode)
- [Chat Mode Events](#chat-mode-events)

---

## Show and Configure Toolbar

The toolbar is visible by default (`ShowToolbar = true`). Use `ToolbarTitle` to set a string title and `ToolbarHeight` to control the toolbar area height.

| Property | Type | Default | Description |
|---|---|---|---|
| `ShowToolbar` | `bool` | `true` | Shows or hides the entire toolbar |
| `ToolbarTitle` | `string` | `""` | Text displayed in the toolbar |
| `ToolbarHeight` | `double` | platform default | Custom height for the toolbar area |

### XAML

```xml
<syncfusion:SfAIAssistView
    ShowToolbar="True"
    ToolbarTitle="AI AssistView"
    ToolbarHeight="50" />
```

### C#

```csharp
sfAIAssistView.ShowToolbar = true;
sfAIAssistView.ToolbarTitle = "AI AssistView";
sfAIAssistView.ToolbarHeight = 50;
```

---

## New Chat Button

The toolbar includes a **New Chat** button. When tapped, it opens a new chat session while preserving the previous session in the conversation history. The user can return to prior conversations via the history panel.

> **Tip:** To use conversation history navigation, enable `EnableConversationHistory`. See `references/history.md` for full history configuration.

---

## Temporary Chat Mode

Temporary Chat provides an ephemeral conversation surface for quick, non-persistent interactions. When the user activates Temporary Chat from the toolbar:

1. The active `AssistItems` collection is cleared
2. A banner is displayed above the chat to indicate the temporary state
3. When the user exits temporary mode, the original session and `EmptyViewTemplate` are restored

### Properties

| Property | Type | Default | Description |
|---|---|---|---|
| `EnableTemporaryChat` | `bool` | `true` | Adds the Temporary Chat option to the toolbar's New Chat button |
| `TemporaryChatBannerText` | `string` | `""` | Banner text shown during temporary mode (used when no custom template is provided) |
| `TemporaryChatBannerTemplate` | `DataTemplate` | `null` | Custom view to replace the default temporary-mode banner |

> **Note:** Setting `EnableTemporaryChat = true` adds the Temporary Chat entry to the New Chat button menu. Clicking it routes new requests to a fresh `AssistItems` collection and displays the banner.

### XAML

```xml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           EnableTemporaryChat="True"
                           TemporaryChatBannerText="This chat will not be saved" />
```

### C#

```csharp
sfAIAssistView.EnableTemporaryChat = true;
sfAIAssistView.TemporaryChatBannerText = "This chat will not be saved";
```

### Custom Banner Template

```xml
<ContentPage.Resources>
    <DataTemplate x:Key="temporaryBannerTemplate">
        <HorizontalStackLayout Spacing="8" Padding="12,8">
            <Image Source="lock_icon.png" WidthRequest="18" HeightRequest="18" />
            <Label Text="Temporary chat — nothing will be saved."
                   FontSize="13"
                   VerticalOptions="Center" />
        </HorizontalStackLayout>
    </DataTemplate>
</ContentPage.Resources>

<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           EnableTemporaryChat="True"
                           TemporaryChatBannerTemplate="{StaticResource temporaryBannerTemplate}" />
```

---

## Chat Mode Events

`SfAIAssistView` raises two events when the user changes chat mode via the toolbar.

### ChatModeChanging

Raised **before** the mode transition. Use `e.Cancel = true` to prevent the switch.

```csharp
sfAIAssistView.ChatModeChanging += OnChatModeChanging;

private void OnChatModeChanging(object sender, ChatModeChangingEventArgs e)
{
    // e.ChatMode — the mode the control is about to transition TO
    if (e.ChatMode == ChatMode.TemporaryChat)
    {
        // Optionally cancel the switch
        e.Cancel = true;
    }
}
```

**`ChatModeChangingEventArgs` properties:**

| Property | Type | Description |
|---|---|---|
| `ChatMode` | `ChatMode` | The mode being switched to (`NewChat` or `TemporaryChat`) |
| `Cancel` | `bool` | Set to `true` to cancel the transition |

### ChatModeChanged

Raised **after** the mode transition completes.

```csharp
sfAIAssistView.ChatModeChanged += OnChatModeChanged;

private void OnChatModeChanged(object sender, ChatModeChangedEventArgs e)
{
    // e.ChatMode — the mode the control has transitioned to
    if (e.ChatMode == ChatMode.TemporaryChat)
    {
        // Temporary chat is now active — update UI or reset local state
    }
    else
    {
        // New chat mode is active — restore saved templates or state if needed
    }
}
```

**`ChatModeChangedEventArgs` properties:**

| Property | Type | Description |
|---|---|---|
| `ChatMode` | `ChatMode` | The mode that is now active (`NewChat` or `TemporaryChat`) |

### XAML Event Wiring

```xml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           EnableTemporaryChat="True"
                           ChatModeChanging="OnChatModeChanging"
                           ChatModeChanged="OnChatModeChanged" />
```
