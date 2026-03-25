# Styles in .NET MAUI Chat (SfChat)

## Table of Contents
- [How Styling Works](#how-styling-works)
- [Chat Background](#chat-background)
- [Incoming Message Styling](#incoming-message-styling)
- [Outgoing Message Styling](#outgoing-message-styling)
- [Message Input View and Editor Styling](#message-input-view-and-editor-styling)
- [Typing Indicator Styling](#typing-indicator-styling)
- [Time Break View Styling](#time-break-view-styling)
- [Suggestions Styling](#suggestions-styling)
- [Send Button Styling](#send-button-styling)
- [Attachment Button Styling](#attachment-button-styling)
- [Delivery State Styling](#delivery-state-styling)
- [Message Type Styling](#message-type-styling)
- [Load More Styling](#load-more-styling)

---

## How Styling Works

SfChat uses Syncfusion's theme dictionary system. Override built-in resource keys inside a `SyncfusionThemeDictionary` to apply custom styles. Always include `SfChatTheme` set to `"CustomTheme"` as the first entry.

```xml
xmlns:syncTheme="clr-namespace:Syncfusion.Maui.Themes;assembly=Syncfusion.Maui.Core"

<ContentPage.Resources>
    <syncTheme:SyncfusionThemeDictionary>
        <syncTheme:SyncfusionThemeDictionary.MergedDictionaries>
            <ResourceDictionary>
                <x:String x:Key="SfChatTheme">CustomTheme</x:String>
                <!-- Add your custom keys here -->
            </ResourceDictionary>
        </syncTheme:SyncfusionThemeDictionary.MergedDictionaries>
    </syncTheme:SyncfusionThemeDictionary>
</ContentPage.Resources>
```

**C# equivalent:**
```csharp
var dict = new ResourceDictionary();
dict.Add("SfChatTheme", "CustomTheme");
dict.Add("SfChatIncomingMessageBackground", Colors.LightYellow);
this.Resources.Add(dict);
```

---

## Chat Background

### Solid color background
```xml
<sfChat:SfChat Background="#94b6ec" ... />
```
> To extend the background into the message input area, also set `SfChatMessageInputViewBackground` to `Transparent`.

### Image background
```xml
<Grid>
    <Image Source="background.jpg" Aspect="AspectFill" />
    <sfChat:SfChat Background="Transparent" ... />
</Grid>
```

### Gradient background
```xml
<sfChat:SfChat ...>
    <sfChat:SfChat.Background>
        <LinearGradientBrush>
            <GradientStop Color="SkyBlue" Offset="0.0" />
            <GradientStop Color="LightCyan" Offset="0.5" />
            <GradientStop Color="LightGray" Offset="1.0" />
        </LinearGradientBrush>
    </sfChat:SfChat.Background>
</sfChat:SfChat>
```

---

## Incoming Message Styling

| Key | Description |
|---|---|
| `SfChatIncomingMessageTextColor` | Message text color |
| `SfChatIncomingMessageBackground` | Bubble background color |
| `SfChatIncomingMessageAuthorTextColor` | Author name text color |
| `SfChatIncomingMessageTimestampTextColor` | Timestamp text color |
| `SfChatIncomingMessageFontFamily` | Font family |
| `SfChatIncomingMessageFontSize` | Font size |
| `SfChatIncomingMessageFontAttributes` | Font attributes (Bold, Italic) |
| `SfChatIncomingMessageAuthorFontFamily` | Author name font family |
| `SfChatIncomingMessageAuthorFontSize` | Author name font size |
| `SfChatIncomingMessageTimestampFontFamily` | Timestamp font family |
| `SfChatIncomingMessageTimestampFontSize` | Timestamp font size |

```xml
<ResourceDictionary>
    <x:String x:Key="SfChatTheme">CustomTheme</x:String>
    <Color x:Key="SfChatIncomingMessageTextColor">Gray</Color>
    <Color x:Key="SfChatIncomingMessageBackground">#eee479</Color>
    <Color x:Key="SfChatIncomingMessageAuthorTextColor">Gray</Color>
    <x:Double x:Key="SfChatIncomingMessageFontSize">16</x:Double>
</ResourceDictionary>
```

---

## Outgoing Message Styling

| Key | Description |
|---|---|
| `SfChatOutgoingMessageTextColor` | Message text color |
| `SfChatOutgoingMessageBackground` | Bubble background color |
| `SfChatOutgoingMessageAuthorTextColor` | Author name text color |
| `SfChatOutgoingMessageTimestampTextColor` | Timestamp text color |
| `SfChatOutgoingMessageFontFamily` | Font family |
| `SfChatOutgoingMessageFontSize` | Font size |
| `SfChatOutgoingMessageFontAttributes` | Font attributes |
| `SfChatOutgoingMessageTimestampFontSize` | Timestamp font size |

```xml
<ResourceDictionary>
    <x:String x:Key="SfChatTheme">CustomTheme</x:String>
    <Color x:Key="SfChatOutgoingMessageTextColor">White</Color>
    <Color x:Key="SfChatOutgoingMessageBackground">#0078D4</Color>
    <x:Double x:Key="SfChatOutgoingMessageFontSize">15</x:Double>
</ResourceDictionary>
```

---

## Message Input View and Editor Styling

### Message input view (container)
```xml
<Color x:Key="SfChatMessageInputViewBackground">#f0f0f0</Color>
```

### Editor (text entry field)

| Key | Description |
|---|---|
| `SfChatEditorTextColor` | Text color |
| `SfChatEditorPlaceholderTextColor` | Placeholder text color |
| `SfChatEditorBackground` | Background color |
| `SfChatEditorStroke` | Border color |
| `SfChatFocusedEditorStroke` | Border color when focused |
| `SfChatEditorFontFamily` | Font family |
| `SfChatEditorFontSize` | Font size |
| `SfChatEditorFontAttributes` | Font attributes |

```xml
<ResourceDictionary>
    <x:String x:Key="SfChatTheme">CustomTheme</x:String>
    <Color x:Key="SfChatEditorTextColor">Black</Color>
    <Color x:Key="SfChatEditorBackground">White</Color>
    <Color x:Key="SfChatEditorPlaceholderTextColor">Gray</Color>
    <Color x:Key="SfChatEditorStroke">#cccccc</Color>
    <x:Double x:Key="SfChatEditorFontSize">14</x:Double>
</ResourceDictionary>
```

---

## Typing Indicator Styling

| Key | Description |
|---|---|
| `SfChatTypingIndicatorTextColor` | Text color |
| `SfChatTypingIndicatorBackground` | Background color |
| `SfChatTypingIndicatorFontFamily` | Font family |
| `SfChatTypingIndicatorFontSize` | Font size |
| `SfChatTypingIndicatorFontAttributes` | Font attributes |

```xml
<Color x:Key="SfChatTypingIndicatorTextColor">Blue</Color>
<Color x:Key="SfChatTypingIndicatorBackground">#eee479</Color>
<x:Double x:Key="SfChatTypingIndicatorFontSize">13</x:Double>
```

---

## Time Break View Styling

| Key | Description |
|---|---|
| `SfChatTimeBreakViewTextColor` | Text color |
| `SfChatTimeBreakViewBackground` | Background color |
| `SfChatTimeBreakViewStroke` | Border color |
| `SfChatTimeBreakViewFontFamily` | Font family |
| `SfChatTimeBreakViewFontSize` | Font size |
| `SfChatTimeBreakViewFontAttributes` | Font attributes |

```xml
<Color x:Key="SfChatTimeBreakViewTextColor">Blue</Color>
<Color x:Key="SfChatTimeBreakViewBackground">#e2f9cd</Color>
<Color x:Key="SfChatTimeBreakViewStroke">LimeGreen</Color>
```

---

## Suggestions Styling

| Key | Description |
|---|---|
| `SfChatSuggestionListItemTextColor` | Item text color |
| `SfChatSuggestionListItemBackground` | Item background color |
| `SfChatSuggestionListBackground` | List container background |
| `SfChatSuggestionListItemStroke` | Item border color |
| `SfChatSuggestionListItemFontFamily` | Font family |
| `SfChatSuggestionListItemFontSize` | Font size |
| `SfChatSuggestionListItemFontAttributes` | Font attributes |

```xml
<Color x:Key="SfChatSuggestionListItemTextColor">Blue</Color>
<Color x:Key="SfChatSuggestionListItemBackground">#d9d9d9</Color>
<Color x:Key="SfChatSuggestionListBackground">Violet</Color>
```

---

## Send Button Styling

| Key | Description |
|---|---|
| `SfChatSendButtonColor` | Icon color (enabled) |
| `SfChatSendButtonDisabledColor` | Icon color (disabled) |
| `SfChatSendButtonBackground` | Background (enabled) |
| `SfChatDisabledSendButtonBackground` | Background (disabled) |
| `SfChatHoveredSendButtonBackground` | Background (hovered) |
| `SfChatPressedSendButtonBackground` | Background (pressed) |

```xml
<Color x:Key="SfChatSendButtonColor">DeepPink</Color>
<Color x:Key="SfChatSendButtonBackground">SkyBlue</Color>
<Color x:Key="SfChatSendButtonDisabledColor">Purple</Color>
```

---

## Attachment Button Styling

| Key | Description |
|---|---|
| `SfChatAttachmentButtonColor` | Icon color |
| `SfChatAttachmentBackground` | Background |
| `SfChatHoveredAttachmentBackground` | Hovered background |
| `SfChatPressedAttachmentBackground` | Pressed background |
| `SfChatHoveredAttachmentButtonColor` | Icon color when hovered |

```xml
<Color x:Key="SfChatAttachmentButtonColor">Orange</Color>
```

---

## Delivery State Styling

| Key | Description |
|---|---|
| `SfChatDeliveryStateSentIconColor` | Sent icon color |
| `SfChatDeliveryStateDeliveredIconColor` | Delivered icon color |
| `SfChatDeliveryStateReadIconColor` | Read icon color |
| `SfChatDeliveryStateFailedIconColor` | Failed icon color |

```xml
<Color x:Key="SfChatDeliveryStateSentIconColor">DarkGray</Color>
<Color x:Key="SfChatDeliveryStateDeliveredIconColor">DarkGray</Color>
<Color x:Key="SfChatDeliveryStateReadIconColor">Blue</Color>
<Color x:Key="SfChatDeliveryStateFailedIconColor">Red</Color>
```

---

## Message Type Styling

### Calendar message
```xml
<Color x:Key="SfChatCalendarBackground">White</Color>
<Color x:Key="SfChatCalendarStroke">Black</Color>
```

### Date picker message
```xml
<Color x:Key="SfChatDatePickerBackground">SkyBlue</Color>
<Color x:Key="SfChatDatePickerTextColor">White</Color>
<Color x:Key="SfChatDatePickerIconColor">Blue</Color>
<Color x:Key="SfChatDatePickerStroke">Black</Color>
```

### Time picker message
```xml
<Color x:Key="SfChatTimePickerBackground">SkyBlue</Color>
<Color x:Key="SfChatTimePickerTextColor">White</Color>
<Color x:Key="SfChatTimePickerIconColor">Blue</Color>
<Color x:Key="SfChatTimePickerStroke">Black</Color>
```

### Hyperlink message
```xml
<Color x:Key="SfChatIncomingHyperlinkColor">#94b6ec</Color>
<Color x:Key="SfChatOutgoingHyperlinkColor">#0056b3</Color>
<Color x:Key="SfChatHyperlinkMetaTitleTextColor">#f29d0a</Color>
<Color x:Key="SfChatHyperlinkDescriptionTextColor">Black</Color>
<Color x:Key="SfChatHyperlinkDescriptionBackground">#dde9cc</Color>
```

### Image message
```xml
<Color x:Key="SfChatIncomingImageStroke">LightGray</Color>
<Color x:Key="SfChatOutgoingImageStroke">LightGray</Color>
```

---

## Load More Styling

| Key | Description |
|---|---|
| `SfChatLoadMoreBackground` | Background color |
| `SfChatLoadMoreTextColor` | Text color |
| `SfChatLoadMoreStroke` | Border color |
| `SfChatLoadMoreIndicatorColor` | Spinner indicator color |

```xml
<Color x:Key="SfChatLoadMoreBackground">White</Color>
<Color x:Key="SfChatLoadMoreTextColor">Blue</Color>
<Color x:Key="SfChatLoadMoreStroke">LightGray</Color>
<Color x:Key="SfChatLoadMoreIndicatorColor">Blue</Color>
```
