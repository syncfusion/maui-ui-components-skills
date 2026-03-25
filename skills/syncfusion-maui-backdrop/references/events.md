# Events — .NET MAUI Backdrop Page

## BackLayerStateChanged Event

The `BackLayerStateChanged` event fires on `SfBackdropPage` whenever the back layer transitions between the **revealed** and **concealed** states.

This event fires for all three reveal/conceal methods:
- Programmatic (`IsBackLayerRevealed`)
- Toolbar icon tap
- Swipe/fling gesture on the front layer

### Event Arguments

| Property | Type | Description |
|---|---|---|
| `Percentage` | `double` | The percentage of the front layer's `RevealedHeight` that is currently visible (0 = fully concealed, 100 = fully revealed) |

---

## Wiring the Event

### XAML

```xml
<backdrop:SfBackdropPage
    xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="BackdropGettingStarted.BackdropSamplePage"
    Title="Menu"
    xmlns:backdrop="clr-namespace:Syncfusion.Maui.Backdrop;assembly=Syncfusion.Maui.Backdrop"
    BackLayerStateChanged="SfBackdropPage_BackLayerStateChanged">
</backdrop:SfBackdropPage>
```

### C# Code-Behind

```csharp
private void SfBackdropPage_BackLayerStateChanged(object sender, BackLayerStateChangedEventArgs e)
{
    // e.Percentage: 0.0 (concealed) to 100.0 (fully revealed)
    double revealPercentage = e.Percentage;

    // Example: log the state
    System.Diagnostics.Debug.WriteLine($"Back layer reveal: {revealPercentage}%");
}
```

### Subscribing in C# (without XAML)

```csharp
public BackdropSamplePage()
{
    InitializeComponent();
    this.BackLayerStateChanged += OnBackLayerStateChanged;
}

private void OnBackLayerStateChanged(object sender, BackLayerStateChangedEventArgs e)
{
    // Handle state change
}
```

---

## Practical Use Cases

### Animate UI elements on reveal

```csharp
private async void OnBackLayerStateChanged(object sender, BackLayerStateChangedEventArgs e)
{
    if (e.Percentage >= 100)
    {
        // Back layer fully revealed — update UI
        await myLabel.FadeTo(0, 200);
    }
    else if (e.Percentage <= 0)
    {
        // Back layer fully concealed — restore UI
        await myLabel.FadeTo(1, 200);
    }
}
```

### Track partial swipe gestures

```csharp
private void OnBackLayerStateChanged(object sender, BackLayerStateChangedEventArgs e)
{
    // Update a progress indicator as user swipes
    progressBar.Progress = e.Percentage / 100.0;
}
```

---

## Gotchas

- The event fires **during the animation**, so `Percentage` may have intermediate values between 0 and 100 during a swipe gesture — not just at 0 or 100.
- Avoid heavy operations inside this event handler to prevent animation jank.
