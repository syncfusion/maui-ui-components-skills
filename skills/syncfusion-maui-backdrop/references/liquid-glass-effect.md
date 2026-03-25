# Liquid Glass Effect — .NET MAUI Backdrop Page

The Liquid Glass Effect applies a modern translucent design with adaptive color tinting and light refraction to the front or back layer of `SfBackdropPage`, creating a sleek, glass-like appearance.

---

## Platform Requirements

> ⚠️ **This feature has strict platform and framework requirements:**
> - **iOS 26 or higher**
> - **macOS 26 or higher**
> - **.NET 10** (not available in .NET 9 or earlier)

The effect will have no visual impact on unsupported platforms (Android, Windows, or older iOS/macOS versions).

---

## How to Enable

### Step 1: Enable `EnableLiquidGlassEffect`

Set the `EnableLiquidGlassEffect` property to `true` on either `BackdropFrontLayer` or `BackdropBackLayer`.

### Step 2: Set Background to Transparent

To achieve the glass look, set the layer's (or its content's) `Background` to `Transparent`. The system will then treat the background as a tinted color for the glass effect.

---

## Apply to the Front Layer

**XAML:**
```xml
<backdrop:SfBackdropPage.FrontLayer>
    <backdrop:BackdropFrontLayer EnableLiquidGlassEffect="True"/>
</backdrop:SfBackdropPage.FrontLayer>
```

**C#:**
```csharp
this.FrontLayer = new BackdropFrontLayer();
this.FrontLayer.EnableLiquidGlassEffect = true;
```

---

## Apply to the Back Layer

**XAML:**
```xml
<backdrop:SfBackdropPage.BackLayer>
    <backdrop:BackdropBackLayer EnableLiquidGlassEffect="True">
        <Grid Background="Transparent">
            <!-- back layer content -->
        </Grid>
    </backdrop:BackdropBackLayer>
</backdrop:SfBackdropPage.BackLayer>
```

---

## Full Example (Front Layer + Gradient Back Layer)

```xml
<backdrop:SfBackdropPage
    xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="AcrylicBackdropPage"
    xmlns:backdrop="clr-namespace:Syncfusion.Maui.Backdrop;assembly=Syncfusion.Maui.Backdrop">

    <!-- Back Layer with gradient background -->
    <backdrop:SfBackdropPage.BackLayer>
        <backdrop:BackdropBackLayer>
            <Grid>
                <Grid.Background>
                    <LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
                        <GradientStop Color="#0F4C75" Offset="0.0"/>
                        <GradientStop Color="#3282B8" Offset="0.5"/>
                        <GradientStop Color="#1B262C" Offset="1.0"/>
                    </LinearGradientBrush>
                </Grid.Background>
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto"/>
                </Grid.RowDefinitions>
                <CollectionView Background="Transparent">
                    <CollectionView.ItemsSource>
                        <x:Array Type="{x:Type x:String}">
                            <x:String>Appetizers</x:String>
                            <x:String>Soups</x:String>
                            <x:String>Desserts</x:String>
                            <x:String>Salads</x:String>
                        </x:Array>
                    </CollectionView.ItemsSource>
                </CollectionView>
            </Grid>
        </backdrop:BackdropBackLayer>
    </backdrop:SfBackdropPage.BackLayer>

    <!-- Front Layer with liquid glass effect -->
    <backdrop:SfBackdropPage.FrontLayer>
        <backdrop:BackdropFrontLayer EnableLiquidGlassEffect="True"/>
    </backdrop:SfBackdropPage.FrontLayer>

</backdrop:SfBackdropPage>
```

**C# equivalent:**
```csharp
this.BackLayer = new BackdropBackLayer()
{
    Content = new Grid()
    {
        Background = new LinearGradientBrush()
        {
            StartPoint = new Point(0, 0),
            EndPoint = new Point(0, 1),
            GradientStops =
            {
                new GradientStop { Color = Color.FromArgb("#0F4C75"), Offset = 0.0f },
                new GradientStop { Color = Color.FromArgb("#3282B8"), Offset = 0.5f },
                new GradientStop { Color = Color.FromArgb("#1B262C"), Offset = 1.0f }
            }
        },
        RowDefinitions = { new RowDefinition { Height = GridLength.Auto } },
        Children =
        {
            new CollectionView
            {
                ItemsSource = new string[] { "Appetizers", "Soups", "Desserts", "Salads" }
            }
        }
    }
};

this.FrontLayer = new BackdropFrontLayer();
this.FrontLayer.EnableLiquidGlassEffect = true;
```

---

## Gotchas

- This feature requires **.NET 10** — it will not compile or run on .NET 9 projects.
- Only supported on **iOS 26+** and **macOS 26+**. On Android and Windows, the property is accepted but has no visual effect.
- For the glass effect to be visible, ensure the back layer (or content behind the front layer) has a rich, colorful background — a plain white background won't produce a noticeable glass look.
- Setting `Background="Transparent"` on the layer content is key; a solid background color will prevent the refraction effect from showing through.
