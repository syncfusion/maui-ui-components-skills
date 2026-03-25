# Styling in SfAIAssistView

All style keys are applied via `SyncfusionThemeDictionary`. Every section requires `SfAIAssistViewTheme` = `"CustomTheme"` as the trigger key.

## Table of Contents
- [How to Apply Styles](#how-to-apply-styles)
- [Background Styling](#background-styling)
- [Request Item Styling](#request-item-styling)
- [Response Item Styling](#response-item-styling)
- [Hyperlink Item Styling](#hyperlink-item-styling)
- [Card Item Styling](#card-item-styling)
- [Editor Styling](#editor-styling)
- [Input View Background](#input-view-background)
- [Send Button Styling](#send-button-styling)
- [Action View Styling](#action-view-styling)
- [Action Button Styling](#action-button-styling)
- [Stop Responding Styling](#stop-responding-styling)
- [Suggestions Styling](#suggestions-styling)
- [Common Suggestions Styling](#common-suggestions-styling)
- [Response Suggestion Header Styling](#response-suggestion-header-styling)
- [AutoComplete Suggestion Styling](#autocomplete-suggestion-styling)
- [Scroll to Bottom Button Styling](#scroll-to-bottom-button-styling)
- [Text Selection Highlight](#text-selection-highlight)

---

## How to Apply Styles

All style keys must be set inside a `SyncfusionThemeDictionary` with `SfAIAssistViewTheme` = `"CustomTheme"`. Without the theme key, overrides are ignored.

```xml
xmlns:syncTheme="clr-namespace:Syncfusion.Maui.Themes;assembly=Syncfusion.Maui.Core"

<ContentPage.Resources>
    <syncTheme:SyncfusionThemeDictionary>
        <syncTheme:SyncfusionThemeDictionary.MergedDictionaries>
            <ResourceDictionary>
                <x:String x:Key="SfAIAssistViewTheme">CustomTheme</x:String>
                <!-- Add style keys here -->
            </ResourceDictionary>
        </syncTheme:SyncfusionThemeDictionary.MergedDictionaries>
    </syncTheme:SyncfusionThemeDictionary>
</ContentPage.Resources>
```

**C# equivalent (all sections):**
```csharp
var dictionary = new ResourceDictionary();
dictionary.Add("SfAIAssistViewTheme", "CustomTheme");
// Add keys...
this.Resources.Add(dictionary);
```

---

## Background Styling

Set `SfAIAssistView.Background` directly. For the color to show, also set the theme key `SfAIAssistViewBackground` to `transparent`.

### Solid Color
```xml
<ResourceDictionary>
    <x:String x:Key="SfAIAssistViewTheme">CustomTheme</x:String>
    <Color x:Key="SfAIAssistViewBackground">transparent</Color>
</ResourceDictionary>

<syncfusion:SfAIAssistView Background="#94b6ec" ... />
```

### Image Background
```xml
<Grid>
    <Image Source="backgroundimage.jpg" Aspect="AspectFill" />
    <syncfusion:SfAIAssistView Background="Transparent" ... />
</Grid>
```

### Gradient Background
```xml
<syncfusion:SfAIAssistView ...>
    <syncfusion:SfAIAssistView.Background>
        <LinearGradientBrush>
            <GradientStop Color="SkyBlue" Offset="0.0" />
            <GradientStop Color="LightCyan" Offset="0.25" />
            <GradientStop Color="SteelBlue" Offset="0.5" />
            <GradientStop Color="LightGray" Offset="1.0" />
        </LinearGradientBrush>
    </syncfusion:SfAIAssistView.Background>
</syncfusion:SfAIAssistView>
```

---

## Request Item Styling

| Key | Description |
|---|---|
| `SfAIAssistViewRequestItemTextColor` | Text color of the request item |
| `SfAIAssistViewRequestItemAuthorTextColor` | Author name text color |
| `SfAIAssistViewRequestItemBackground` | Background color of the request bubble |
| `SfAIAssistViewRequestItemFontFamily` | Font family |
| `SfAIAssistViewRequestItemFontAttributes` | Font attributes |
| `SfAIAssistViewRequestItemFontSize` | Font size |
| `SfAIAssistViewRequestItemAuthorFontFamily` | Author name font family |
| `SfAIAssistViewRequestItemAuthorFontAttributes` | Author name font attributes |
| `SfAIAssistViewRequestItemAuthorFontSize` | Author name font size |

```xml
<Color x:Key="SfAIAssistViewRequestItemTextColor">Gray</Color>
<Color x:Key="SfAIAssistViewRequestItemAuthorTextColor">Gray</Color>
<Color x:Key="SfAIAssistViewRequestItemBackground">#eee479</Color>
<x:String x:Key="SfAIAssistViewRequestItemFontFamily">Roboto-Medium</x:String>
<FontAttributes x:Key="SfAIAssistViewRequestItemFontAttributes">Italic</FontAttributes>
<x:Double x:Key="SfAIAssistViewRequestItemFontSize">16</x:Double>
```

---

## Response Item Styling

| Key | Description |
|---|---|
| `SfAIAssistViewResponseItemTextColor` | Text color of the response item |
| `SfAIAssistViewResponseItemAuthorTextColor` | Author name text color |
| `SfAIAssistViewResponseItemBackground` | Background color of the response bubble |
| `SfAIAssistViewResponseItemFontFamily` | Font family |
| `SfAIAssistViewResponseItemFontAttributes` | Font attributes |
| `SfAIAssistViewResponseItemFontSize` | Font size |
| `SfAIAssistViewResponseItemAuthorFontFamily` | Author name font family |
| `SfAIAssistViewResponseItemAuthorFontAttributes` | Author name font attributes |
| `SfAIAssistViewResponseItemAuthorFontSize` | Author name font size |

```xml
<Color x:Key="SfAIAssistViewResponseItemTextColor">Gray</Color>
<Color x:Key="SfAIAssistViewResponseItemBackground">#eee479</Color>
<Color x:Key="SfAIAssistViewResponseItemAuthorTextColor">Gray</Color>
<x:String x:Key="SfAIAssistViewResponseItemFontFamily">Roboto-Medium</x:String>
<FontAttributes x:Key="SfAIAssistViewResponseItemFontAttributes">Italic</FontAttributes>
<x:Double x:Key="SfAIAssistViewResponseItemFontSize">16</x:Double>
```

---

## Hyperlink Item Styling

| Key | Description |
|---|---|
| `SfAIAssistViewRequestHyperlinkColor` | URL text color in a request hyperlink item |
| `SfAIAssistViewResponseHyperlinkColor` | URL text color in a response hyperlink item |
| `SfAIAssistViewHyperlinkDescriptionTextColor` | Meta description text color |
| `SfAIAssistViewHyperlinkDescriptionBackground` | Meta description background |
| `SfAIAssistViewHyperlinkMetaTitleTextColor` | Meta title text color |

```xml
<Color x:Key="SfAIAssistViewRequestHyperlinkColor">#94b6ec</Color>
<Color x:Key="SfAIAssistViewHyperlinkMetaTitleTextColor">#f29d0a</Color>
<Color x:Key="SfAIAssistViewHyperlinkDescriptionTextColor">Black</Color>
<Color x:Key="SfAIAssistViewHyperlinkDescriptionBackground">#dde9cc</Color>
```

---

## Card Item Styling

| Key | Description |
|---|---|
| `SfAIAssistViewCardBackground` | Card background color |
| `SfAIAssistViewCardStroke` | Card border color |
| `SfAIAssistViewCardTitleTextColor` | Title text color |
| `SfAIAssistViewCardTitleFontFamily` | Title font family |
| `SfAIAssistViewCardTitleFontSize` | Title font size |
| `SfAIAssistViewCardTitleFontAttributes` | Title font attributes |
| `SfAIAssistViewCardSubtitleTextColor` | Subtitle text color |
| `SfAIAssistViewCardSubtitleFontFamily` | Subtitle font family |
| `SfAIAssistViewCardSubtitleFontSize` | Subtitle font size |
| `SfAIAssistViewCardSubtitleFontAttributes` | Subtitle font attributes |
| `SfAIAssistViewCardDescriptionTextColor` | Description text color |
| `SfAIAssistViewCardDescriptionFontFamily` | Description font family |
| `SfAIAssistViewCardDescriptionFontSize` | Description font size |
| `SfAIAssistViewCardDescriptionFontAttributes` | Description font attributes |
| `SfAIAssistViewCardButtonBackground` | Card button background |
| `SfAIAssistViewCardButtonStroke` | Card button border |
| `SfAIAssistViewCardButtonTextColor` | Card button text color |
| `SfAIAssistViewCardButtonFontFamily` | Card button font family |
| `SfAIAssistViewCardButtonFontSize` | Card button font size |
| `SfAIAssistViewCardButtonFontAttributes` | Card button font attributes |

```xml
<Color x:Key="SfAIAssistViewCardBackground">#94b6ec</Color>
<Color x:Key="SfAIAssistViewCardStroke">#f29d0a</Color>
<Color x:Key="SfAIAssistViewCardTitleTextColor">Black</Color>
<x:Double x:Key="SfAIAssistViewCardTitleFontSize">16</x:Double>
<FontAttributes x:Key="SfAIAssistViewCardTitleFontAttributes">Bold</FontAttributes>
```

---

## Editor Styling

| Key | Description |
|---|---|
| `SfAIAssistViewEditorTextColor` | Text color in the editor |
| `SfAIAssistViewEditorPlaceholderTextColor` | Placeholder text color |
| `SfAIAssistViewEditorStroke` | Editor border color |
| `SfAIAssistViewEditorBackground` | Editor background color |
| `SfAIAssistViewEditorFontFamily` | Editor font family |
| `SfAIAssistViewEditorFontAttributes` | Editor font attributes |
| `SfAIAssistViewEditorFontSize` | Editor font size |

```xml
<Color x:Key="SfAIAssistViewEditorPlaceholderTextColor">Blue</Color>
<Color x:Key="SfAIAssistViewEditorTextColor">Black</Color>
<Color x:Key="SfAIAssistViewEditorBackground">LightGreen</Color>
<Color x:Key="SfAIAssistViewEditorStroke">Black</Color>
<x:Double x:Key="SfAIAssistViewEditorFontSize">16</x:Double>
```

---

## Input View Background

| Key | Description |
|---|---|
| `SfAIAssistViewInputViewBackground` | Background color of the entire input area (editor + send button row) |

```xml
<Color x:Key="SfAIAssistViewInputViewBackground">#94b6ec</Color>
```

---

## Send Button Styling

| Key | Description |
|---|---|
| `SfAIAssistViewDisabledSendButtonIconColor` | Send button icon color when disabled |
| `SfAIAssistViewDisabledSendButtonColor` | Send button background color when disabled |

```xml
<Color x:Key="SfAIAssistViewDisabledSendButtonIconColor">Purple</Color>
<Color x:Key="SfAIAssistViewDisabledSendButtonColor">LightGreen</Color>
```

---

## Action View Styling

The action view includes the copy, retry, like, and dislike icons on response items.

| Key | Description |
|---|---|
| `SfAIAssistViewNormalActionViewColor` | Background color in normal state |
| `SfAIAssistViewHoverActionViewColor` | Background color on hover |
| `SfAIAssistViewPressedActionViewColor` | Background color when pressed |
| `SfAIAssistViewNormalActionViewIconColor` | Icon color (copy/retry/like/dislike) |
| `SfAIAssistViewSelectedLikeIconColor` | Like icon color when selected |
| `SfAIAssistViewSelectedDisLikeIconColor` | Dislike icon color when selected |

```xml
<Color x:Key="SfAIAssistViewNormalActionViewColor">Blue</Color>
<Color x:Key="SfAIAssistViewHoverActionViewColor">LightGray</Color>
<Color x:Key="SfAIAssistViewPressedActionViewColor">DarkGray</Color>
<Color x:Key="SfAIAssistViewNormalActionViewIconColor">Black</Color>
<Color x:Key="SfAIAssistViewSelectedLikeIconColor">Green</Color>
<Color x:Key="SfAIAssistViewSelectedDisLikeIconColor">Red</Color>
```

---

## Action Button Styling

The quick action popup button that opens the `ActionButtons` list.

| Key | Description |
|---|---|
| `SfAIAssistViewActionButtonBackground` | Action trigger button background |
| `SfAIAssistViewActionButtonIconColor` | Action trigger button icon color |
| `SfAIAssistViewActionButtonViewTextColor` | Text color of items in the popup |
| `SfAIAssistViewActionButtonsPopupBackground` | Popup background color |

```xml
<Color x:Key="SfAIAssistViewActionButtonBackground">Orange</Color>
<Color x:Key="SfAIAssistViewActionButtonIconColor">White</Color>
<Color x:Key="SfAIAssistViewActionButtonViewTextColor">Black</Color>
<Color x:Key="SfAIAssistViewActionButtonsPopupBackground">LightGray</Color>
```

---

## Stop Responding Styling

| Key | Description |
|---|---|
| `SfAIAssistViewStopRespondingBackground` | Background color |
| `SfAIAssistViewStopRespondingIconColor` | Icon color |
| `SfAIAssistViewStopRespondingTextColor` | Label text color |
| `SfAIAssistViewStopRespondingFontFamily` | Font family |
| `SfAIAssistViewStopRespondingFontSize` | Font size |
| `SfAIAssistViewStopRespondingFontAttributes` | Font attributes |
| `SfAIAssistViewStopRespondingStroke` | Border/stroke color |
| `SfAIAssistViewStopRespondingStrokeThickness` | Stroke thickness |

```xml
<Color x:Key="SfAIAssistViewStopRespondingIconColor">Red</Color>
<Color x:Key="SfAIAssistViewStopRespondingTextColor">DarkBlue</Color>
<x:String x:Key="SfAIAssistViewStopRespondingFontFamily">Segoe UI</x:String>
<x:Double x:Key="SfAIAssistViewStopRespondingFontSize">14</x:Double>
<SolidColorBrush x:Key="SfAIAssistViewStopRespondingStroke">Violet</SolidColorBrush>
<x:Double x:Key="SfAIAssistViewStopRespondingStrokeThickness">2</x:Double>
```

---

## Suggestions Styling

Styles for response item suggestion chips.

| Key | Description |
|---|---|
| `SfAIAssistViewSuggestionItemTextColor` | Chip text color |
| `SfAIAssistViewSuggestionItemBackground` | Chip background color |
| `SfAIAssistViewSuggestionBackground` | Suggestions list background color |
| `SfAIAssistViewSuggestionItemFontFamily` | Chip font family |
| `SfAIAssistViewSuggestionItemFontAttributes` | Chip font attributes |
| `SfAIAssistViewSuggestionItemFontSize` | Chip font size |

```xml
<Color x:Key="SfAIAssistViewSuggestionItemTextColor">Blue</Color>
<Color x:Key="SfAIAssistViewSuggestionItemBackground">#d9d9d9</Color>
<Color x:Key="SfAIAssistViewSuggestionBackground">Violet</Color>
<x:Double x:Key="SfAIAssistViewSuggestionItemFontSize">16</x:Double>
```

---

## Common Suggestions Styling

Styles for the header/common suggestion chips (requires `ShowHeader=true`).

| Key | Description |
|---|---|
| `SfAIAssistViewHeaderSuggestionBackground` | Background of the common suggestions area |
| `SfAIAssistViewHeaderSuggestionItemStroke` | Chip border color |
| `SfAIAssistViewHeaderSuggestionItemStrokeThickness` | Chip border thickness |
| `SfAIAssistViewHeaderSuggestionItemBackground` | Chip background color |
| `SfAIAssistViewHeaderSuggestionItemTextColor` | Chip text color |
| `SfAIAssistViewHeaderSuggestionItemFontSize` | Chip font size |
| `SfAIAssistViewHeaderSuggestionItemFontFamily` | Chip font family |
| `SfAIAssistViewHeaderSuggestionItemFontAttribute` | Chip font attributes |

```xml
<Color x:Key="SfAIAssistViewHeaderSuggestionBackground">LightSkyBlue</Color>
<Color x:Key="SfAIAssistViewHeaderSuggestionItemStroke">BlueViolet</Color>
<x:Double x:Key="SfAIAssistViewHeaderSuggestionItemStrokeThickness">2</x:Double>
<Color x:Key="SfAIAssistViewHeaderSuggestionItemBackground">White</Color>
<Color x:Key="SfAIAssistViewHeaderSuggestionItemTextColor">Blue</Color>
<x:Double x:Key="SfAIAssistViewHeaderSuggestionItemFontSize">16</x:Double>
```

---

## Response Suggestion Header Styling

Styles for the `SuggestionHeaderText` label shown above response suggestion chips.

| Key | Description |
|---|---|
| `SfAIAssistViewSuggestionHeaderTextColor` | Header label text color |
| `SfAIAssistViewSuggestionHeaderFontSize` | Header label font size |
| `SfAIAssistViewSuggestionHeaderFontFamily` | Header label font family |
| `SfAIAssistViewSuggestionHeaderFontAttributes` | Header label font attributes |

```xml
<Color x:Key="SfAIAssistViewSuggestionHeaderTextColor">DarkBlue</Color>
<x:Double x:Key="SfAIAssistViewSuggestionHeaderFontSize">14</x:Double>
<x:String x:Key="SfAIAssistViewSuggestionHeaderFontFamily">Roboto-Medium</x:String>
<FontAttributes x:Key="SfAIAssistViewSuggestionHeaderFontAttributes">Bold</FontAttributes>
```

---

## AutoComplete Suggestion Styling

Styles for the autocomplete overlay that appears as the user types in the editor.

| Key | Description |
|---|---|
| `SfAIAssistViewAutoCompleteSuggestionBackground` | Overlay background |
| `SfAIAssistViewAutoCompleteSuggestionItemBackground` | Item background |
| `SfAIAssistViewAutoCompleteSuggestionItemTextColor` | Item text color |
| `SfAIAssistViewAutoCompleteSuggestionItemFontFamily` | Item font family |
| `SfAIAssistViewAutoCompleteSuggestionItemFontSize` | Item font size |
| `SfAIAssistViewAutoCompleteSuggestionItemFontAttributes` | Item font attributes |

```xml
<Color x:Key="SfAIAssistViewAutoCompleteSuggestionBackground">Orange</Color>
<Color x:Key="SfAIAssistViewAutoCompleteSuggestionItemBackground">LightSkyBlue</Color>
<Color x:Key="SfAIAssistViewAutoCompleteSuggestionItemTextColor">Green</Color>
<x:Double x:Key="SfAIAssistViewAutoCompleteSuggestionItemFontSize">20</x:Double>
<FontAttributes x:Key="SfAIAssistViewAutoCompleteSuggestionItemFontAttributes">Bold</FontAttributes>
```

---

## Scroll to Bottom Button Styling

| Key | Description |
|---|---|
| `SfAIAssistViewScrollToBottomButtonBackground` | Button background color |
| `SfAIAssistViewScrollToBottomButtonIconColor` | Button icon color |

```xml
<Color x:Key="SfAIAssistViewScrollToBottomButtonBackground">Orange</Color>
<Color x:Key="SfAIAssistViewScrollToBottomButtonIconColor">White</Color>
```

---

## Text Selection Highlight

| Key | Description |
|---|---|
| `SfAIAssistViewSelectionTextHighLightColor` | Color used to highlight selected text |

```xml
<Color x:Key="SfAIAssistViewSelectionTextHighLightColor">Orange</Color>
```

```csharp
dictionary.Add("SfAIAssistViewSelectionTextHighLightColor", Colors.Orange);
```
