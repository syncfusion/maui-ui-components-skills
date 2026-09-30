# Time Break Separators in SfAIAssistView

`SfAIAssistView` can display time break separators to group conversation items by date and improve readability in long chat threads.

## Table of Contents
- [Show Time Break](#show-time-break)
- [Labels and Template](#labels-and-template)
- [Scenarios](#scenarios)

---

## Show Time Break

Use `ShowTimeBreak` to enable or disable date-based separators between messages from different dates. The default value is `false`.

```xaml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           ShowTimeBreak="True" />
```

```csharp
sfAIAssistView.ShowTimeBreak = true;
```

---

## Labels and Template

When enabled, the control uses smart labels in a hierarchical format:

- `Today` for the current date
- `Yesterday` for the previous date
- weekday names for recent dates
- full date strings for older messages

Use `TimeBreakTemplate` to replace the default separator UI with a custom `DataTemplate`.

```xaml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           ShowTimeBreak="True">
    <syncfusion:SfAIAssistView.TimeBreakTemplate>
        <DataTemplate>
            <Grid>
                <Label Text="Conversation Break" HorizontalOptions="Center" />
            </Grid>
        </DataTemplate>
    </syncfusion:SfAIAssistView.TimeBreakTemplate>
</syncfusion:SfAIAssistView>
```

The built-in label and separator styling can be customized with `AIAssistStyle` properties.

---

## Scenarios

- When `ShowTimeBreak` is `true`, date-based separator labels appear between grouped messages.
- When `ShowTimeBreak` is `false`, no time break separators are displayed.
- When messages exist for today, yesterday, and older dates, the labels display as `Today`, `Yesterday`, weekday names, or formatted dates.
- When a `TimeBreakTemplate` is assigned, the custom template is used instead of the built-in separator UI.

---

## Technical Notes

- **Default Template**: `DefaultConversationHistoryTimeBreakTemplate` is used when no custom template is assigned.
- **Label Localization**: built-in labels such as `Today` and `Yesterday` are localized..

### Bindable Properties

- `ShowTimeBreak` (`bool`, default: `false`) — Show or hide time break separators
- `TimeBreakTemplate` (`DataTemplate`) — Custom template for the time break area

### Styling Properties

- `TimeBreakLabelTextColor` (`Color`) — Text color of the separator label
- `TimeBreakLabelFontSize` (`double`) — Font size of the separator label
- `TimeBreakLabelFontFamily` (`string`) — Font family of the separator label
- `TimeBreakLabelFontAttributes` (`FontAttributes`) — Font styling of the separator label
- `TimeBreakSeparatorHeightRequest` (`double`) — Height of the separator line
- `TimeBreakSeparatorBackground` (`Brush`) — Background brush of the separator line
