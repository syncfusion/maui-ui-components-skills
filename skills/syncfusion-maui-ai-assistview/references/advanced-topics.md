# Advanced Topics in SfAIAssistView

Less-common but important features: text selection in conversation items and stopping in-progress AI responses.

## Table of Contents
- [Text Selection](#text-selection)
- [Stop Responding](#stop-responding)

---

## Text Selection

Allows users to select specific phrases or the full text of any request or response item using platform-native selection handles.

Text selection is **disabled by default**. Set `AllowTextSelection` to `true` to enable it.

```xaml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           AllowTextSelection="True" />
```

```csharp
sfAIAssistView.AllowTextSelection = true;
```

> **Platform behavior:** Selection handles and the copy menu match the native behavior of each platform (Android, iOS, Windows, macOS).

---

## Stop Responding

The Stop Responding button appears while the AI is generating a response. Tapping it lets users cancel an ongoing response that is no longer needed.

The button is **visible by default** (`EnableStopResponding = true`).

### Enable / Disable

```xaml
<!-- Disable the stop button -->
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           EnableStopResponding="False" />
```

```csharp
sfAIAssistView.EnableStopResponding = false;
```

### StopRespondingIcon

Customize the icon shown on the Stop Responding button.

```xaml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView">
    <syncfusion:SfAIAssistView.StopRespondingIcon>
        <FontImageSource Glyph="&#xe80f;"
                         FontFamily="MauiMaterialAssets"
                         Color="Black" />
    </syncfusion:SfAIAssistView.StopRespondingIcon>
</syncfusion:SfAIAssistView>
```

```csharp
sfAIAssistView.StopRespondingIcon = new FontImageSource
{
    Glyph = "\ue80f",
    FontFamily = "MauiMaterialAssets",
    Color = Colors.Black
};
```

### StopRespondingTemplate

Fully replace the Stop Responding UI with a custom `DataTemplate`.

```xaml
<ContentPage.Resources>
    <DataTemplate x:Key="stopTemplate">
        <Grid>
            <HorizontalStackLayout Spacing="8" HorizontalOptions="Center">
                <Image Source="stop_icon.png" WidthRequest="20" HeightRequest="20" />
                <Label Text="Stop" VerticalOptions="Center" />
            </HorizontalStackLayout>
        </Grid>
    </DataTemplate>
</ContentPage.Resources>

<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           StopRespondingTemplate="{StaticResource stopTemplate}" />
```

```csharp
sfAIAssistView.StopRespondingTemplate = new DataTemplate(() =>
{
    var stack = new HorizontalStackLayout { Spacing = 8, HorizontalOptions = LayoutOptions.Center };
    stack.Children.Add(new Image { Source = "stop_icon.png", WidthRequest = 20, HeightRequest = 20 });
    stack.Children.Add(new Label { Text = "Stop", VerticalOptions = LayoutOptions.Center });
    return stack;
});
```

### StopResponding Event and Command

Raised when the user taps the Stop Responding button. Use a `CancelResponse` flag to halt the ongoing AI call inside your request handler.

#### Event

```xaml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           StopResponding="OnStopResponding" />
```

```csharp
sfAIAssistView.StopResponding += OnStopResponding;

private void OnStopResponding(object sender, EventArgs e)
{
    // Signal the request handler to stop
    CancelResponse = true;
}
```

#### Command (MVVM) with CancelResponse Pattern

```xaml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           AssistItems="{Binding AssistItems}"
                           RequestCommand="{Binding RequestCommand}"
                           StopRespondingCommand="{Binding StopRespondingCommand}" />
```

```csharp
public class AIAssistViewModel : INotifyPropertyChanged
{
    private bool cancelResponse;

    public ICommand RequestCommand { get; }
    public ICommand StopRespondingCommand { get; }

    public AIAssistViewModel()
    {
        RequestCommand = new Command(ExecuteRequest);
        StopRespondingCommand = new Command(ExecuteStopResponding);
    }

    private void ExecuteStopResponding()
    {
        cancelResponse = true;

        // Optionally add a cancellation notice to the chat
        AssistItems.Add(new AssistItem
        {
            Text = "Response cancelled.",
            IsRequested = false,
            ShowAssistItemFooter = false
        });
    }

    private async void ExecuteRequest()
    {
        cancelResponse = false;
        await GetAIResultAsync();
    }

    private async Task GetAIResultAsync()
    {
        // Check the flag before each async step
        if (cancelResponse) return;

        var result = await CallAIServiceAsync();

        if (cancelResponse) return;

        AssistItems.Add(new AssistItem
        {
            Text = result,
            IsRequested = false
        });
    }
}
```

> **Customization:** Use `StopRespondingIcon`, `StopRespondingTemplate`, `StopResponding`, and `StopRespondingCommand` to tailor the stop action to your app.
