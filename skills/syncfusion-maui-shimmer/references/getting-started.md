# Getting Started with .NET MAUI Shimmer

## Step 1: Create a .NET MAUI Project

In Visual Studio:
1. Go to **File > New > Project**
2. Select **.NET MAUI App** template
3. Name the project and click **Next**
4. Select your .NET framework version and click **Create**

---

## Step 2: Install the NuGet Package

`SfShimmer` is part of `Syncfusion.Maui.Core`. Install it via:

**Package Manager UI:**
Search for `Syncfusion.Maui.Core` and install the latest version.

**CLI:**
```bash
dotnet add package Syncfusion.Maui.Core
```

**Package Manager Console:**
```powershell
Install-Package Syncfusion.Maui.Core
```

---

## Step 3: Register the Handler

In `MauiProgram.cs`, call `ConfigureSyncfusionCore()` — this is required for all Syncfusion MAUI controls:

```csharp
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

> **Note:** Without this registration, the shimmer control will not render.

---

## Step 4: Add SfShimmer to a Page

Import the namespace and place the control in your page.

### XAML

```xml
<ContentPage
    xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:shimmer="clr-namespace:Syncfusion.Maui.Shimmer;assembly=Syncfusion.Maui.Core"
    x:Class="GettingStarted.MainPage">

    <shimmer:SfShimmer VerticalOptions="Fill" />

</ContentPage>
```

### C#

```csharp
using Syncfusion.Maui.Shimmer;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        SfShimmer shimmer = new SfShimmer();
        this.Content = shimmer;
    }
}
```

By default, `SfShimmer` renders the `CirclePersona` built-in type with `IsActive = true`.

---

## Step 5: Add Content and Control Loading State

`SfShimmer` wraps your actual content. Set `IsActive = false` to hide the shimmer and display the content beneath it.

### XAML

```xml
<shimmer:SfShimmer x:Name="shimmer"
                   Type="CirclePersona"
                   VerticalOptions="Fill"
                   IsActive="true">
    <shimmer:SfShimmer.Content>
        <StackLayout>
            <Label Text="Content is loaded!" />
        </StackLayout>
    </shimmer:SfShimmer.Content>
</shimmer:SfShimmer>
```

### C#

```csharp
SfShimmer shimmer = new SfShimmer()
{
    IsActive = true,
    VerticalOptions = LayoutOptions.Fill,
    Content = new Label
    {
        Text = "Content is loaded!"
    }
};
this.Content = shimmer;
```

### Toggling the shimmer after data loads

```csharp
// In your page or ViewModel:
shimmer.IsActive = true;   // Show shimmer while loading

var result = await FetchDataAsync();
UpdateUI(result);

shimmer.IsActive = false;  // Reveal actual content
```

> **How it works:** When `IsActive = true`, the shimmer animation overlays the content area. Setting it to `false` hides the animation and shows the `Content` child.
