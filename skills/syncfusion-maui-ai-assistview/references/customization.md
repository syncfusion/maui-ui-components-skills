# Customization in SfAIAssistView

Covers empty view display, localization of built-in strings, RTL support, and the Liquid Glass Effect for iOS/macOS 26+.

---

## Empty View

`EmptyView` is displayed when `AssistItems` contains no items. Accepts a `string` or any `View`.

```xml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           AssistItems="{Binding AssistItems}"
                           EmptyView="Ask AI Anything" />
```

```csharp
sfAIAssistView.EmptyView = "Ask AI Anything";
```

### EmptyViewTemplate

Fully customizes the empty state layout. The `EmptyViewTemplate` is only rendered when `EmptyView` is also set.

```xml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           AssistItems="{Binding AssistItems}"
                           EmptyView="No Items">
    <syncfusion:SfAIAssistView.EmptyViewTemplate>
        <DataTemplate>
            <Grid RowDefinitions="45,30"
                  RowSpacing="10"
                  HorizontalOptions="Center"
                  VerticalOptions="Center">

                <Border Background="#6C4EC2"
                        Stroke="#CAC4D0"
                        HorizontalOptions="Center">
                    <Border.StrokeShape>
                        <RoundRectangle CornerRadius="12" />
                    </Border.StrokeShape>
                    <Label Text="&#xe7e1;"
                           FontSize="24"
                           FontFamily="MauiSampleFontIcon"
                           TextColor="White"
                           HeightRequest="45" WidthRequest="45"
                           HorizontalTextAlignment="Center"
                           VerticalTextAlignment="Center" />
                </Border>

                <Label Grid.Row="1"
                       Text="Hi, How can I help you!"
                       FontSize="20"
                       FontFamily="Roboto-Regular"
                       HorizontalOptions="Center" />
            </Grid>
        </DataTemplate>
    </syncfusion:SfAIAssistView.EmptyViewTemplate>
</syncfusion:SfAIAssistView>
```

```csharp
sfAIAssistView.EmptyView = "No Items";
sfAIAssistView.EmptyViewTemplate = new DataTemplate(() =>
{
    var grid = new Grid
    {
        RowDefinitions =
        {
            new RowDefinition { Height = 45 },
            new RowDefinition { Height = 30 }
        },
        RowSpacing = 10,
        HorizontalOptions = LayoutOptions.Center,
        VerticalOptions = LayoutOptions.Center
    };

    var border = new Border
    {
        Background = Color.FromArgb("#6C4EC2"),
        Stroke = Color.FromArgb("#CAC4D0"),
        HorizontalOptions = LayoutOptions.Center,
        StrokeShape = new RoundRectangle { CornerRadius = 12 }
    };
    border.Content = new Label
    {
        Text = "\ue7e1",
        FontSize = 24,
        FontFamily = "MauiSampleFontIcon",
        TextColor = Colors.White,
        WidthRequest = 45,
        HeightRequest = 45,
        HorizontalTextAlignment = TextAlignment.Center,
        VerticalTextAlignment = TextAlignment.Center
    };

    var label = new Label
    {
        Text = "Hi, How can I help you!",
        FontSize = 20,
        FontFamily = "Roboto-Regular",
        HorizontalOptions = LayoutOptions.Center
    };
    Grid.SetRow(label, 1);

    grid.Children.Add(border);
    grid.Children.Add(label);
    return grid;
});
```

> **Note:** `EmptyViewTemplate` will not render unless `EmptyView` is also set.

---

## Localization

Translate built-in UI strings (e.g., "Stop Responding", toolbar labels) to other languages using `.resx` resource files.

### Steps

1. Create a `Resources` folder in your app project.
2. Add a resource file named `SfAIAssistView.<culture>.resx` — e.g., `SfAIAssistView.es.resx` for Spanish.
3. Set the file's **Build Action** to `EmbeddedResource`.
4. Add Name/Value pairs for each string key you want to override.
5. Apply the culture in `App.xaml.cs`:

```csharp
using System.Globalization;
using System.Resources;
using Syncfusion.Maui.AIAssistView;

public partial class App : Application
{
    public App()
    {
        InitializeComponent();
        CultureInfo.CurrentUICulture = new CultureInfo("es");
        SfAIAssistViewResources.ResourceManager = new ResourceManager(
            "MyApp.Resources.SfAIAssistView",
            Application.Current!.GetType().Assembly);
        MainPage = new MainPage();
    }
}
```

> The `ResourceManager` namespace (`"MyApp.Resources.SfAIAssistView"`) must match your app's assembly name and the path to the `.resx` file.

---

## RTL Support

Enable right-to-left layout by setting `FlowDirection` on the control or its parent page.

```xml
<syncfusion:SfAIAssistView x:Name="sfAIAssistView"
                           AssistItems="{Binding AssistItems}"
                           FlowDirection="RightToLeft" />
```

```csharp
sfAIAssistView.FlowDirection = FlowDirection.RightToLeft;
```

To apply RTL to the entire page:
```xml
<ContentPage FlowDirection="RightToLeft">
    ...
</ContentPage>
```

---

## Liquid Glass Effect

Applies a modern translucent glass-like appearance with adaptive color tinting and light refraction.

> **Platform support:** iOS 26+ and macOS 26+ only. Requires **.NET 10**.

### Steps

1. Wrap `SfAIAssistView` inside `SfGlassEffectView` from `Syncfusion.Maui.Core`.
2. Set `SfAIAssistView.Background` to `Transparent`.
3. Set `EnableLiquidGlassEffect` to `true`.

### XAML

```xml
xmlns:core="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
xmlns:assistView="clr-namespace:Syncfusion.Maui.AIAssistView;assembly=Syncfusion.Maui.AIAssistView"

<Grid Background="#FF54A3CD">
    <core:SfGlassEffectView EffectType="Regular"
                            CornerRadius="20">
        <assistView:SfAIAssistView x:Name="sfAIAssistView"
                                   Background="Transparent"
                                   EnableLiquidGlassEffect="True" />
    </core:SfGlassEffectView>
</Grid>
```

### C#

```csharp
using Syncfusion.Maui.Core;
using Syncfusion.Maui.AIAssistView;

var grid = new Grid { Background = Color.FromArgb("#FF54A3CD") };

var glassView = new SfGlassEffectView
{
    CornerRadius = 20,
    EffectType = LiquidGlassEffectType.Regular
};

var aiAssistView = new SfAIAssistView
{
    Background = Colors.Transparent,
    EnableLiquidGlassEffect = true
};

glassView.Content = aiAssistView;
grid.Children.Add(glassView);
this.Content = grid;
```
