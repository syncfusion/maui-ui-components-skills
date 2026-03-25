# Getting Started — .NET MAUI Backdrop Page

## Table of Contents
- [Step 1: Install NuGet Package](#step-1-install-nuget-package)
- [Step 2: Register the Handler](#step-2-register-the-handler)
- [Step 3: Initialize SfBackdropPage](#step-3-initialize-sfbackdroppage)
- [Step 4: Add Back Layer Content](#step-4-add-back-layer-content)
- [Step 5: Add Front Layer Content](#step-5-add-front-layer-content)
- [Step 6: Reveal and Conceal the Back Layer](#step-6-reveal-and-conceal-the-back-layer)

---

## Step 1: Install NuGet Package

Install the **Syncfusion.Maui.Backdrop** NuGet package.

**Via NuGet Package Manager UI:**
1. Right-click the project in Solution Explorer → **Manage NuGet Packages**
2. Search for `Syncfusion.Maui.Backdrop`
3. Install the latest version

**Via CLI:**
```bash
dotnet add package Syncfusion.Maui.Backdrop
```

> `Syncfusion.Maui.Core` is automatically installed as a dependency — it is required for all Syncfusion MAUI controls.

---

## Step 2: Register the Handler

Register the Syncfusion core handler in `MauiProgram.cs`. This is required for all Syncfusion .NET MAUI controls.

```csharp
// MauiProgram.cs
using Syncfusion.Maui.Core.Hosting;

namespace GettingStarted
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();

            builder.ConfigureSyncfusionCore();

            builder
                .UseMauiApp<App>()
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                });

            return builder.Build();
        }
    }
}
```

---

## Step 3: Initialize SfBackdropPage

Make the page inherit from `SfBackdropPage` instead of `ContentPage`.

**XAML:**
```xml
<backdrop:SfBackdropPage
    xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="BackdropGettingStarted.BackdropSamplePage"
    Title="Menu"
    xmlns:backdrop="clr-namespace:Syncfusion.Maui.Backdrop;assembly=Syncfusion.Maui.Backdrop">

</backdrop:SfBackdropPage>
```

**C#:**
```csharp
using Syncfusion.Maui.Backdrop;

namespace BackdropGettingStarted;

public partial class BackdropSamplePage : SfBackdropPage
{
    public BackdropSamplePage()
    {
        InitializeComponent();
        this.Title = "Menu";
    }
}
```

> The `Title` and `ToolbarItems` properties from the base `Page` class can be used to customize the header appearance.

---

## Step 4: Add Back Layer Content

The back layer holds actionable or contextual content (navigation menus, filters) that appears behind the front layer. It fills the entire background or only as much as its content height.

**XAML:**
```xml
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
```

**C#:**
```csharp
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
```

> To set a background color on the back layer, apply `BackgroundColor` to the **content** (e.g., the `Grid`) instead of `BackdropBackLayer` itself.

---

## Step 5: Add Front Layer Content

The front layer always appears in front of the back layer, spans the full width, and holds the primary content of the page.

**XAML:**
```xml
<backdrop:SfBackdropPage.FrontLayer>
    <backdrop:BackdropFrontLayer>
        <Grid BackgroundColor="WhiteSmoke" />
    </backdrop:BackdropFrontLayer>
</backdrop:SfBackdropPage.FrontLayer>
```

**C#:**
```csharp
this.FrontLayer = new BackdropFrontLayer()
{
    Content = new Grid
    {
        BackgroundColor = Colors.WhiteSmoke,
    }
};
```

---

## Step 6: Reveal and Conceal the Back Layer

Three ways to reveal/conceal the back layer:

### Programmatically
Set `IsBackLayerRevealed` to `true` to reveal, `false` to conceal.

```xml
<backdrop:SfBackdropPage ... IsBackLayerRevealed="True">
```
```csharp
this.IsBackLayerRevealed = true;
```

### Touch Interaction (Toolbar Icon)
- The **Hamburger icon** reveals the back layer
- The **Close icon** (×) conceals it
- In a **FlyoutPage**, these are replaced by expand (↓) and collapse (↑) icons

> The page header and toolbar icons are only visible when the backdrop page is a child of `NavigationPage`.

### Swipe / Fling Action
- **Swipe down** on the front layer to reveal the back layer
- **Swipe up** on the front layer to conceal it
- The swipe interaction is handled within the `RevealedHeight` region at the top of the front layer

---

## Wrapping in NavigationPage (App.xaml.cs)

The header toolbar (with reveal/conceal icons) is only shown when the backdrop page is wrapped in a `NavigationPage`.

```csharp
// App.xaml.cs
public App()
{
    MainPage = new NavigationPage(new BackdropSamplePage());
}
```
