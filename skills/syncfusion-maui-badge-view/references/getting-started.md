# Getting Started with Badge View

This guide covers the setup, installation, and basic implementation of the Syncfusion .NET MAUI Badge View (SfBadgeView) control.

## Installation

### Step 1: Install NuGet Package

The Badge View is part of the `Syncfusion.Maui.Core` package.

**Via NuGet Package Manager (Visual Studio):**
1. Right-click your project in Solution Explorer
2. Select **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.Core`
4. Click **Install**

**Via .NET CLI:**
```bash
dotnet add package Syncfusion.Maui.Core
```

**Via Package Manager Console:**
```powershell
Install-Package Syncfusion.Maui.Core
```

### Step 2: Register Syncfusion Handler

In your `MauiProgram.cs` file, register the Syncfusion Core handler:

```csharp
using Microsoft.Extensions.Logging;
using Syncfusion.Maui.Core.Hosting;

namespace YourApp
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .ConfigureSyncfusionCore()  // Register Syncfusion handler
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                    fonts.AddFont("OpenSans-Semibold.ttf", "OpenSansSemibold");
                });

#if DEBUG
            builder.Logging.AddDebug();
#endif

            return builder.Build();
        }
    }
}
```

**Important:** The `ConfigureSyncfusionCore()` call is required for all Syncfusion .NET MAUI controls.

## Basic Badge View Implementation

### Add Namespace

Import the Syncfusion.Maui.Core namespace in your XAML or C# files.

**XAML:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:badge="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
             x:Class="YourApp.MainPage">
    <!-- Badge views go here -->
</ContentPage>
```

**C#:**
```csharp
using Syncfusion.Maui.Core;
```

### Create a Simple Badge View

**XAML:**
```xml
<badge:SfBadgeView BadgeText="5" 
                   HorizontalOptions="Center" 
                   VerticalOptions="Center">
    <badge:SfBadgeView.Content>
        <Button Text="Notifications" 
                WidthRequest="120" 
                HeightRequest="60"/>
    </badge:SfBadgeView.Content>
</badge:SfBadgeView>
```

**C#:**
```csharp
public MainPage()
{
    InitializeComponent();
    
    var badgeView = new SfBadgeView
    {
        BadgeText = "5",
        HorizontalOptions = LayoutOptions.Center,
        VerticalOptions = LayoutOptions.Center
    };
    
    var button = new Button
    {
        Text = "Notifications",
        WidthRequest = 120,
        HeightRequest = 60
    };
    
    badgeView.Content = button;
    Content = badgeView;
}
```

## Adding Badge Text

The `BadgeText` property displays text or numbers on the badge overlay.

**XAML:**
```xml
<badge:SfBadgeView BadgeText="20">
    <badge:SfBadgeView.Content>
        <Button Text="Messages" WidthRequest="100" HeightRequest="50"/>
    </badge:SfBadgeView.Content>
</badge:SfBadgeView>
```

**C#:**
```csharp
var badgeView = new SfBadgeView();
badgeView.BadgeText = "20";

var button = new Button 
{ 
    Text = "Messages",
    WidthRequest = 100,
    HeightRequest = 50
};
badgeView.Content = button;
```

**Common badge text patterns:**
- Notification counts: `"12"`, `"99+"`, `"1k+"`
- Status text: `"New"`, `"Hot"`, `"Sale"`
- Short labels: `"VIP"`, `"Pro"`

## Adding Content to Badge View

The `Content` property defines what the badge overlays on. You can add any MAUI view:

### Badge on Button

```xml
<badge:SfBadgeView BadgeText="3">
    <badge:SfBadgeView.Content>
        <Button Text="Cart" 
                BackgroundColor="LightBlue"
                WidthRequest="120" 
                HeightRequest="60"/>
    </badge:SfBadgeView.Content>
</badge:SfBadgeView>
```

### Badge on Image

```xml
<badge:SfBadgeView BadgeText="8">
    <badge:SfBadgeView.Content>
        <Image Source="notification_icon.png" 
               WidthRequest="50" 
               HeightRequest="50"/>
    </badge:SfBadgeView.Content>
</badge:SfBadgeView>
```

### Badge on Label

```xml
<badge:SfBadgeView BadgeText="New">
    <badge:SfBadgeView.Content>
        <Label Text="Product Name"
               BackgroundColor="LightGray"
               Padding="10"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center"
               WidthRequest="100"
               HeightRequest="60"/>
    </badge:SfBadgeView.Content>
</badge:SfBadgeView>
```

### Badge on Custom View

```csharp
var badgeView = new SfBadgeView { BadgeText = "5" };

// Create a custom frame with icon
var frame = new Frame
{
    WidthRequest = 60,
    HeightRequest = 60,
    CornerRadius = 30,
    Padding = 10,
    BackgroundColor = Colors.LightBlue,
    Content = new Image { Source = "user_avatar.png" }
};

badgeView.Content = frame;
```

## Accessibility with ScreenReaderText

The `ScreenReaderText` property provides accessible descriptions for screen readers.

**XAML:**
```xml
<badge:SfBadgeView BadgeText="12" 
                   ScreenReaderText="You have 12 unread messages">
    <badge:SfBadgeView.Content>
        <Button Text="Messages"/>
    </badge:SfBadgeView.Content>
</badge:SfBadgeView>
```

**C#:**
```csharp
var badgeView = new SfBadgeView
{
    BadgeText = "12",
    ScreenReaderText = "You have 12 unread messages"
};
```

**Best practices for ScreenReaderText:**
- Provide context: "You have 5 notifications" instead of just "5"
- Include action hints: "3 new friend requests pending review"
- Update dynamically when BadgeText changes
- Use clear, concise language

## Basic Badge Settings

Configure badge appearance using `BadgeSettings`:

```xml
<badge:SfBadgeView BadgeText="10">
    <badge:SfBadgeView.Content>
        <Button Text="Inbox"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings Type="Primary" 
                            Position="TopRight"
                            Animation="Scale"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

**Key BadgeSettings properties for beginners:**
- `Type`: Predefined color (Primary, Error, Success, Warning, Info)
- `Position`: Where badge appears (TopRight, TopLeft, BottomRight, etc.)
- `Animation`: Badge animation (Scale or None)

## Complete Example: Notification Badge

Here's a complete implementation showing a notification button with badge:

**XAML:**
```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:badge="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
             x:Class="BadgeApp.MainPage">

    <VerticalStackLayout Spacing="20" 
                         Padding="30"
                         VerticalOptions="Center">
        
        <badge:SfBadgeView BadgeText="5"
                           ScreenReaderText="You have 5 unread notifications"
                           HorizontalOptions="Center">
            <badge:SfBadgeView.Content>
                <Button Text="Notifications"
                        WidthRequest="150"
                        HeightRequest="60"
                        BackgroundColor="#2196F3"
                        TextColor="White"
                        FontSize="16"/>
            </badge:SfBadgeView.Content>
            <badge:SfBadgeView.BadgeSettings>
                <badge:BadgeSettings Type="Error"
                                    Position="TopRight"
                                    Animation="Scale"/>
            </badge:SfBadgeView.BadgeSettings>
        </badge:SfBadgeView>
        
    </VerticalStackLayout>

</ContentPage>
```

**C# Code-Behind:**
```csharp
using Syncfusion.Maui.Core;

namespace BadgeApp;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }
}
```

## Troubleshooting

### Badge Not Appearing

**Problem:** Badge view doesn't show on the screen.

**Solutions:**
1. Verify `ConfigureSyncfusionCore()` is called in `MauiProgram.cs`
2. Check that `BadgeText` is set (or `Icon` property for icon badges)
3. Ensure Content property has a valid view
4. Verify HorizontalOptions and VerticalOptions are set correctly

### Handler Registration Error

**Problem:** Exception "Handler not found for control"

**Solution:**
Add the Syncfusion handler registration:
```csharp
.ConfigureSyncfusionCore()
```
in `MauiProgram.cs` before `.Build()` is called.

### NuGet Package Not Found

**Problem:** Cannot find `Syncfusion.Maui.Core` package.

**Solutions:**
1. Check your NuGet package sources include nuget.org
2. Try clearing NuGet cache: `dotnet nuget locals all --clear`
3. Verify internet connection
4. Check for typos in package name

### Badge Text Not Visible

**Problem:** Badge appears but text is not visible.

**Solutions:**
1. Check TextColor matches or contrasts with Background
2. Verify FontSize is not too small
3. Ensure BadgeText is not empty or null
4. Check if custom Background is transparent

### Content Not Showing

**Problem:** Badge appears but content view is missing.

**Solutions:**
1. Set explicit WidthRequest and HeightRequest on content
2. Ensure content view is not null
3. Check content view's IsVisible property
4. Verify parent layout constraints
