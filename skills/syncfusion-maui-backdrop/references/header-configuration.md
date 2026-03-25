# Header Configuration — .NET MAUI Backdrop Page

The backdrop page header (with reveal/conceal icons) is only visible when `SfBackdropPage` is placed inside a `NavigationPage`. This document covers how to set it up and customize the icons and text.

---

## Setup: Wrap in NavigationPage

Set the backdrop page as a child of `NavigationPage` in `App.xaml.cs`. You can also apply `NavigationPage` styling properties here.

```csharp
// App.xaml.cs
public App()
{
    MainPage = new NavigationPage(new BackdropSamplePage())
    {
        BarBackgroundColor = Colors.DarkSlateBlue,
        BarTextColor = Colors.White
    };
}
```

> Without `NavigationPage`, no header or toolbar icons will appear. The back layer can still be revealed programmatically or via swipe.

---

## Default Icons

### In NavigationPage

When the backdrop is inside a `NavigationPage`, the toolbar shows:
- **Hamburger icon** — reveals the back layer
- **Close icon (×)** — conceals the back layer

### In FlyoutPage

When the backdrop is inside a `FlyoutPage`, the icons change to:
- **Down arrow (↓)** — reveals the back layer
- **Up arrow (↑)** — conceals the back layer

---

## Custom Icon Images

Replace the default icons with custom images using `OpenIconImageSource` and `CloseIconImageSource`. Accepted image sources:

- `FileImageSource` (local file, e.g., `open.png`)
- `UriImageSource` (remote URL)
- `StreamImageSource`
- Resource-based (`FromResource`)

**XAML:**
```xml
<backdrop:SfBackdropPage
    xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="BackdropGettingStarted.BackdropSamplePage"
    xmlns:backdrop="clr-namespace:Syncfusion.Maui.Backdrop;assembly=Syncfusion.Maui.Backdrop"
    OpenIconImageSource="open.png"
    CloseIconImageSource="close.png">
</backdrop:SfBackdropPage>
```

**C#:**
```csharp
using Syncfusion.Maui.Backdrop;

public partial class BackdropSamplePage : SfBackdropPage
{
    public BackdropSamplePage()
    {
        InitializeComponent();
        this.OpenIconImageSource = "open.png";
        this.CloseIconImageSource = "close.png";
    }
}
```

> Place image files in the `Resources/Images` folder of your MAUI project so they resolve correctly on all platforms.

---

## Custom Icon Text

Display a text label alongside (or instead of) the toolbar icon using `OpenText` and `CloseText`.

**XAML:**
```xml
<backdrop:SfBackdropPage
    xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="BackdropGettingStarted.BackdropSamplePage"
    xmlns:backdrop="clr-namespace:Syncfusion.Maui.Backdrop;assembly=Syncfusion.Maui.Backdrop"
    OpenText="Show Menu"
    CloseText="Hide Menu">
</backdrop:SfBackdropPage>
```

**C#:**
```csharp
public partial class BackdropSamplePage : SfBackdropPage
{
    public BackdropSamplePage()
    {
        InitializeComponent();
        this.OpenText = "Show Menu";
        this.CloseText = "Hide Menu";
    }
}
```

---

## Summary of Header Properties

| Property | Type | Description |
|---|---|---|
| `OpenIconImageSource` | `ImageSource` | Icon displayed when back layer is concealed (tap to reveal) |
| `CloseIconImageSource` | `ImageSource` | Icon displayed when back layer is revealed (tap to conceal) |
| `OpenText` | `string` | Text label shown with the open/reveal icon |
| `CloseText` | `string` | Text label shown with the close/conceal icon |

---

## Gotchas

- The header toolbar **only appears when inside `NavigationPage`**. Forgetting this is the most common setup issue.
- `BarBackgroundColor` and `BarTextColor` are properties of `NavigationPage`, not `SfBackdropPage`.
- Icon text (`OpenText`/`CloseText`) may not display on all platforms with all icon configurations — test on target platforms.
