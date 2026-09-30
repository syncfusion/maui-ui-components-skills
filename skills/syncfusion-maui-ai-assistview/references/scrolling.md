# Scroll to Bottom in SfAIAssistView

`SfAIAssistView` provides an optional scroll-to-bottom button and an `AutoScrollBehavior` property that help users stay aligned with the latest request or response in a long conversation.

---

## ShowScrollToBottomButton

The button is hidden by default. Set `ShowScrollToBottomButton` to `true` to enable it. The button appears when the user scrolls away from the bottom and disappears once they are at the latest message.

```xaml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           AssistItems="{Binding AssistItems}"
                           ShowScrollToBottomButton="True" />
```

```csharp
sfAIAssistView.ShowScrollToBottomButton = true;
```

---

## ScrollToBottomButtonTemplate

Use `ScrollToBottomButtonTemplate` to replace the default button appearance with a fully custom `DataTemplate`.

### XAML

```xaml
<ContentPage.Resources>
    <DataTemplate x:Key="scrollToBottomTemplate">
        <Grid WidthRequest="40" HeightRequest="40">
            <Border StrokeShape="Ellipse"
                    BackgroundColor="#6200EE"
                    Padding="8">
                <Label Text="&#x2193;"
                       TextColor="White"
                       FontSize="18"
                       HorizontalOptions="Center"
                       VerticalOptions="Center" />
            </Border>
        </Grid>
    </DataTemplate>
</ContentPage.Resources>

<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           AssistItems="{Binding AssistItems}"
                           ShowScrollToBottomButton="True"
                           ScrollToBottomButtonTemplate="{StaticResource scrollToBottomTemplate}" />
```

### C#

```csharp
sfAIAssistView.ShowScrollToBottomButton = true;
sfAIAssistView.ScrollToBottomButtonTemplate = new DataTemplate(() =>
{
    var grid = new Grid { WidthRequest = 40, HeightRequest = 40 };

    var border = new Border
    {
        StrokeShape = new Ellipse(),
        BackgroundColor = Color.FromArgb("#6200EE"),
        Padding = new Thickness(8)
    };

    var label = new Label
    {
        Text = "\u2193",
        TextColor = Colors.White,
        FontSize = 18,
        HorizontalOptions = LayoutOptions.Center,
        VerticalOptions = LayoutOptions.Center
    };

    border.Content = label;
    grid.Children.Add(border);
    return grid;
});
```

---

## AutoScrollBehavior

Use `AutoScrollBehavior` to control whether the conversation view automatically scrolls to the latest user request or the most recent AI response. The default value is `ScrollToLastResponse`.

```xaml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           AssistItems="{Binding AssistItems}"
                           AutoScrollBehavior="ScrollToLastRequest" />
```

```csharp
sfAIAssistView.AutoScrollBehavior = AssistViewScrollBehavior.ScrollToLastRequest;
```

### Scenarios

- When `AutoScrollBehavior` is set to `ScrollToLastRequest`, new content scrolls to the latest user request.
- When `AutoScrollBehavior` is set to `ScrollToLastResponse`, new content scrolls to the most recent AI response.
