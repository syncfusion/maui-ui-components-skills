# Liquid Glass Effect

The Liquid Glass Effect gives `SfSmartTextEditor` a modern translucent, glass-like appearance with adaptive color tinting and light refraction. It uses `SfGlassEffectView` from `Syncfusion.Maui.Core` to wrap the editor.

---

## Prerequisites

| Requirement | Minimum Version |
|-------------|----------------|
| .NET | .NET 10 |
| macOS | macOS 26 or higher |
| iOS | iOS 26 or higher |
| Android / Windows | Not supported |

> This feature is **only available on .NET 10** and requires macOS 26+ or iOS 26+. It will not render on Android or Windows.

---

## How to Apply

There are three steps to enable the liquid glass effect:

1. **Wrap** `SfSmartTextEditor` inside `SfGlassEffectView`
2. **Set** `EnableLiquidGlassEffect="True"` on the editor
3. **Set** `Background="Transparent"` so the glass tinting works correctly

---

## XAML Example

```xml
<ContentPage
    xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:smarttexteditor="clr-namespace:Syncfusion.Maui.SmartComponents;assembly=Syncfusion.Maui.SmartComponents"
    xmlns:core="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
    x:Class="MyApp.AcrylicEditorPage">

    <Grid>
        <!-- Gradient background behind the glass -->
        <Grid.Background>
            <LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
                <GradientStop Color="#0F4C75" Offset="0.0" />
                <GradientStop Color="#3282B8" Offset="0.5" />
                <GradientStop Color="#1B262C" Offset="1.0" />
            </LinearGradientBrush>
        </Grid.Background>

        <!-- Glass effect container -->
        <core:SfGlassEffectView
            EffectType="Regular"
            CornerRadius="15"
            WidthRequest="350"
            HeightRequest="200">

            <smarttexteditor:SfSmartTextEditor
                x:Name="smartTextEditor"
                EnableLiquidGlassEffect="True"
                Background="Transparent"
                SuggestionDisplayMode="Popup"
                Placeholder="Type your reply..."
                UserRole="Support engineer responding to customer tickets">
                <smarttexteditor:SfSmartTextEditor.UserPhrases>
                    <x:String>Thanks for reaching out.</x:String>
                    <x:String>Please share a minimal reproducible sample.</x:String>
                    <x:String>We'll update you as soon as we have more details.</x:String>
                </smarttexteditor:SfSmartTextEditor.UserPhrases>
            </smarttexteditor:SfSmartTextEditor>

        </core:SfGlassEffectView>
    </Grid>
</ContentPage>
```

---

## C# Example

```csharp
// Gradient background grid
var mainGrid = new Grid
{
    Background = new LinearGradientBrush
    {
        StartPoint = new Point(0, 0),
        EndPoint   = new Point(0, 1),
        GradientStops =
        {
            new GradientStop { Color = Color.FromArgb("#0F4C75"), Offset = 0.0f },
            new GradientStop { Color = Color.FromArgb("#3282B8"), Offset = 0.5f },
            new GradientStop { Color = Color.FromArgb("#1B262C"), Offset = 1.0f }
        }
    }
};

// Glass container
var glassView = new SfGlassEffectView
{
    EffectType    = LiquidGlassEffectType.Regular,
    CornerRadius  = 15,
    WidthRequest  = 350,
    HeightRequest = 200
};

// Smart Text Editor with glass enabled
var editor = new SfSmartTextEditor
{
    EnableLiquidGlassEffect = true,
    Background              = Colors.Transparent,
    SuggestionDisplayMode   = SuggestionDisplayMode.Popup,
    Placeholder             = "Type your reply...",
    UserRole                = "Support engineer responding to customer tickets"
};

glassView.Content = editor;
mainGrid.Children.Add(glassView);
this.Content = mainGrid;
```

---

## Key Properties

| Property | Where Set | Purpose |
|----------|-----------|---------|
| `EnableLiquidGlassEffect` | `SfSmartTextEditor` | Activates the glass effect and applies it to dependent controls |
| `Background` | `SfSmartTextEditor` | Set to `Transparent` for correct glass tinting |
| `EffectType` | `SfGlassEffectView` | Glass variant — `Regular` is the standard choice |
| `CornerRadius` | `SfGlassEffectView` | Rounds the glass container corners |

---

## Tips

- Place a visually rich background (gradient, image, blurred content) behind the `SfGlassEffectView` to get the full frosted-glass look. A plain white/black background produces minimal effect.
- `EffectType="Regular"` works for most use cases. Refer to the [Liquid Glass Getting Started documentation](https://help.syncfusion.com/maui/liquid-glass-ui/getting-started) for other effect types.
- The effect is also applied automatically to the suggestion popup when `EnableLiquidGlassEffect` is `true`.
