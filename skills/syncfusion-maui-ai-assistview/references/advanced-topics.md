# Advanced Topics in SfAIAssistView

Less-common but important features: text selection, stopping an in-progress AI response, and the response loader shimmer.

## Table of Contents
- [Text Selection](#text-selection)
- [Stop Responding](#stop-responding)
- [Response Loader](#response-loader)

---

## Text Selection

Allows users to select specific phrases or the full text of any request or response item using platform-native selection handles.

Text selection is **disabled by default**. Set `AllowTextSelection` to `true` to enable it.

```xml
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

```xml
<!-- Disable the stop button -->
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           EnableStopResponding="False" />
```

```csharp
sfAIAssistView.EnableStopResponding = false;
```

### StopRespondingText

Customize the label shown on the Stop Responding button.

```xml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           StopRespondingText="Cancel Response" />
```

```csharp
sfAIAssistView.StopRespondingText = "Cancel Response";
```

### StopRespondingTemplate

Fully replace the Stop Responding UI with a custom `DataTemplate`.

```xml
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

```xml
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

```xml
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

> **Pattern:** Set `cancelResponse = false` at the start of each request, then check it at each `await` boundary. When `StopRespondingCommand` fires, set `cancelResponse = true` — the next `if (cancelResponse) return` exits the pipeline cleanly.

---

## Response Loader

A shimmer placeholder is shown while the AI is generating a response, giving the user visual feedback that a response is pending.

The loader is **enabled by default** (`ShowResponseLoader = true`). Set it to `false` to hide the shimmer.

```xml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           AssistItems="{Binding AssistItems}"
                           ShowResponseLoader="False" />
```

```csharp
sfAIAssistView.ShowResponseLoader = false;
```

> **When to disable:** Useful when you have a custom loading indicator in your own UI layer and want to avoid showing duplicate loading states.
