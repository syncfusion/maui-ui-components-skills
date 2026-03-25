# Reveal Height & Corner Shape Customization — .NET MAUI Backdrop Page

## Table of Contents
- [Reveal Height: BackLayerRevealOption](#reveal-height-backlayerrevealoption)
- [RevealedHeight on the Front Layer](#revealedheight-on-the-front-layer)
- [Corner Shape: EdgeShape](#corner-shape-edgeshape)
- [Corner Radius Customization](#corner-radius-customization)
- [Combined Example](#combined-example)

---

## Reveal Height: BackLayerRevealOption

Controls how far the front layer moves down when the back layer is revealed.

| Option | Behavior |
|---|---|
| `Fill` | Front layer moves to the full page height minus `RevealedHeight` (default) |
| `Auto` | Front layer moves only as far as the back layer content height |

Use `Auto` when the back layer content is short (e.g., a short menu) and you don't want excess empty space. Use `Fill` when the back layer should occupy the full screen background.

**XAML:**
```xml
<backdrop:SfBackdropPage
    xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="BackdropGettingStarted.BackdropSamplePage"
    Title="Menu"
    xmlns:backdrop="clr-namespace:Syncfusion.Maui.Backdrop;assembly=Syncfusion.Maui.Backdrop"
    BackLayerRevealOption="Auto"
    IsBackLayerRevealed="True">

    <backdrop:SfBackdropPage.BackLayer>
        <backdrop:BackdropBackLayer>
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto"/>
                </Grid.RowDefinitions>
                <CollectionView>
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

    <backdrop:SfBackdropPage.FrontLayer>
        <backdrop:BackdropFrontLayer>
            <Grid BackgroundColor="WhiteSmoke" />
        </backdrop:BackdropFrontLayer>
    </backdrop:SfBackdropPage.FrontLayer>

</backdrop:SfBackdropPage>
```

**C#:**
```csharp
this.IsBackLayerRevealed = true;
this.BackLayerRevealOption = RevealOption.Auto;

this.BackLayer = new BackdropBackLayer
{
    Content = new Grid
    {
        RowDefinitions =
        {
            new RowDefinition { Height = GridLength.Auto }
        },
        Children =
        {
            new CollectionView
            {
                ItemsSource = new string[] { "Appetizers", "Soups", "Desserts", "Salads" }
            }
        }
    }
};

this.FrontLayer = new BackdropFrontLayer()
{
    Content = new Grid
    {
        BackgroundColor = Colors.WhiteSmoke,
    }
};
```

---

## RevealedHeight on the Front Layer

`RevealedHeight` defines a minimum peeking height for the front layer when the back layer is fully revealed. This ensures the front layer is never completely hidden — a portion is always visible at the bottom.

```xml
<backdrop:BackdropFrontLayer RevealedHeight="60">
    <Grid BackgroundColor="WhiteSmoke" />
</backdrop:BackdropFrontLayer>
```

```csharp
this.FrontLayer = new BackdropFrontLayer
{
    RevealedHeight = 60,
    Content = new Grid { BackgroundColor = Colors.WhiteSmoke }
};
```

> The swipe/fling gesture on the front layer is only handled within the `RevealedHeight` region at the top of the front layer.

---

## Corner Shape: EdgeShape

Customize the top corners of the front layer using the `EdgeShape` property on `BackdropFrontLayer`.

| Value | Appearance |
|---|---|
| `Curve` | Rounded top-left and top-right corners (default) |
| `Flat` | Cut/flat top corners |

> Only the **top-left** and **top-right** corners are shaped. Bottom corners are always square.

**XAML:**
```xml
<!-- Curve (default) -->
<backdrop:BackdropFrontLayer EdgeShape="Curve">
    <Grid />
</backdrop:BackdropFrontLayer>

<!-- Flat -->
<backdrop:BackdropFrontLayer EdgeShape="Flat">
    <Grid />
</backdrop:BackdropFrontLayer>
```

**C#:**
```csharp
this.FrontLayer = new BackdropFrontLayer
{
    EdgeShape = EdgeShape.Flat,
    Content = new Grid()
};
```

---

## Corner Radius Customization

Set `LeftCornerRadius` and `RightCornerRadius` independently on `BackdropFrontLayer` for asymmetric designs.

**XAML:**
```xml
<backdrop:SfBackdropPage.FrontLayer>
    <backdrop:BackdropFrontLayer
        LeftCornerRadius="30"
        RightCornerRadius="0"
        EdgeShape="Flat">
        <Grid />
    </backdrop:BackdropFrontLayer>
</backdrop:SfBackdropPage.FrontLayer>
```

**C#:**
```csharp
this.FrontLayer = new BackdropFrontLayer()
{
    Content = new Grid(),
    LeftCornerRadius = 30,
    RightCornerRadius = 0,
    EdgeShape = EdgeShape.Flat
};
```

---

## Combined Example

Full page with auto reveal height, custom corner radius, and peeking front layer:

```xml
<backdrop:SfBackdropPage
    xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="MyApp.BackdropSamplePage"
    Title="Menu"
    xmlns:backdrop="clr-namespace:Syncfusion.Maui.Backdrop;assembly=Syncfusion.Maui.Backdrop"
    BackLayerRevealOption="Auto">

    <backdrop:SfBackdropPage.BackLayer>
        <backdrop:BackdropBackLayer>
            <StackLayout Padding="16">
                <Label Text="Navigation" FontAttributes="Bold" />
                <Label Text="Home" />
                <Label Text="Profile" />
                <Label Text="Settings" />
            </StackLayout>
        </backdrop:BackdropBackLayer>
    </backdrop:SfBackdropPage.BackLayer>

    <backdrop:SfBackdropPage.FrontLayer>
        <backdrop:BackdropFrontLayer
            EdgeShape="Curve"
            LeftCornerRadius="20"
            RightCornerRadius="20"
            RevealedHeight="80">
            <Grid BackgroundColor="White" />
        </backdrop:BackdropFrontLayer>
    </backdrop:SfBackdropPage.FrontLayer>

</backdrop:SfBackdropPage>
```

---

## Property Summary

| Property | On | Description |
|---|---|---|
| `BackLayerRevealOption` | `SfBackdropPage` | `Fill` or `Auto` reveal height mode |
| `RevealedHeight` | `BackdropFrontLayer` | Minimum visible height of front layer when back layer is open |
| `EdgeShape` | `BackdropFrontLayer` | `Curve` or `Flat` top corners |
| `LeftCornerRadius` | `BackdropFrontLayer` | Radius of the top-left corner |
| `RightCornerRadius` | `BackdropFrontLayer` | Radius of the top-right corner |
