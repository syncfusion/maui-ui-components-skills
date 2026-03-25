# Getting Started with .NET MAUI Popup

## Table of Contents
- [Installation](#installation)
- [Handler Registration](#handler-registration)
- [Basic Popup Implementation](#basic-popup-implementation)
- [Displaying the Popup](#displaying-the-popup)
- [Closing the Popup](#closing-the-popup)
- [Native Embedding](#native-embedding)
- [Troubleshooting](#troubleshooting)

## Installation

### Visual Studio

**Step 1: Create or Open Project**
- Go to **File > New > Project** and choose **.NET MAUI App** template
- Or open your existing .NET MAUI project

**Step 2: Install NuGet Package**
1. Right-click the project in **Solution Explorer**
2. Choose **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.Popup`
4. Install the latest version
5. Accept the license agreement

### Visual Studio Code

**Step 1: Create or Open Project**
- Press `Ctrl+Shift+P` (or `Cmd+Shift+P` on Mac)
- Type **.NET:New Project** and press Enter
- Choose **.NET MAUI App** template

**Step 2: Install NuGet Package**
```bash
dotnet add package Syncfusion.Maui.Popup
```

### JetBrains Rider

**Step 1: Create or Open Project**
- Go to **File > New Solution**
- Select **.NET (C#)** and choose **.NET MAUI App** template

**Step 2: Install NuGet Package**
1. Right-click the project in **Solution Explorer**
2. Choose **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.Popup`
4. Install the latest version

Or use the terminal:
```bash
dotnet add package Syncfusion.Maui.Popup
dotnet restore
```

## Handler Registration

The `Syncfusion.Maui.Core` package is a required dependency for all Syncfusion .NET MAUI controls. You must register the Syncfusion handler in your `MauiProgram.cs` file.

### Step 1: Import Namespace

Add the following using statement at the top of `MauiProgram.cs`:

```csharp
using Syncfusion.Maui.Core.Hosting;
```

### Step 2: Register Handler

Call `ConfigureSyncfusionCore()` in the `CreateMauiApp` method:

```csharp
using Microsoft.Maui.Controls.Hosting;
using Microsoft.Maui.Hosting;
using Syncfusion.Maui.Core.Hosting;

namespace GettingStarted
{
    public class MauiProgram 
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                    fonts.AddFont("OpenSans-Semibold.ttf", "OpenSansSemibold");
                });

            // Register Syncfusion handler
            builder.ConfigureSyncfusionCore();
            
            return builder.Build();
        }
    }
}
```

> **Important:** Without this registration, the popup control will not function and you'll encounter runtime errors.

## Basic Popup Implementation

### XAML Approach

**Step 1: Import Namespace**

Add the Syncfusion.Maui.Popup namespace to your XAML page:

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sfPopup="clr-namespace:Syncfusion.Maui.Popup;assembly=Syncfusion.Maui.Popup"
             x:Class="GettingStarted.MainPage">
```

**Step 2: Add SfPopup Control**

Place the `SfPopup` control in your page layout:

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sfPopup="clr-namespace:Syncfusion.Maui.Popup;assembly=Syncfusion.Maui.Popup"
             x:Class="GettingStarted.MainPage" 
             Padding="0,40,0,0">
    <StackLayout x:Name="mainLayout">
        <Button x:Name="clickToShowPopup" 
                Text="Show Popup" 
                VerticalOptions="Start" 
                HorizontalOptions="Center"
                Clicked="ClickToShowPopup_Clicked" />
        
        <sfPopup:SfPopup x:Name="popup" />
    </StackLayout>
</ContentPage>
```

### C# Code-Behind Approach

If you prefer to create the popup entirely in C#:

```csharp
using Syncfusion.Maui.Popup;

namespace GettingStarted
{
    public partial class MainPage : ContentPage
    {
        SfPopup popup;

        public MainPage()
        {
            InitializeComponent();
            
            // Create popup programmatically
            popup = new SfPopup();
            
            // Add to the layout (not necessary if just calling Show())
            // mainLayout.Children.Add(popup);
        }

        private void ClickToShowPopup_Clicked(object sender, EventArgs e)
        {
            popup.Show();
        }
    }
}
```

## Displaying the Popup

There are two primary ways to display a popup:

### Method 1: Using IsOpen Property

```csharp
private void ClickToShowPopup_Clicked(object sender, EventArgs e)
{
    // Set IsOpen to true to display the popup at the center
    popup.IsOpen = true;
}
```

XAML binding example:
```xml
<sfPopup:SfPopup x:Name="popup" IsOpen="True" />
```

### Method 2: Using Show() Method

```csharp
private void ClickToShowPopup_Clicked(object sender, EventArgs e)
{
    // Call Show() method to display the popup at the center
    popup.Show();
}
```

### Complete Working Example

**MainPage.xaml:**
```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sfPopup="clr-namespace:Syncfusion.Maui.Popup;assembly=Syncfusion.Maui.Popup"
             x:Class="GettingStarted.MainPage" 
             Padding="0,40,0,0">
    <StackLayout x:Name="mainLayout">
        <Button x:Name="clickToShowPopup" 
                Text="Click To Show Popup" 
                VerticalOptions="Start" 
                HorizontalOptions="Center"
                Clicked="ClickToShowPopup_Clicked" />
        
        <sfPopup:SfPopup x:Name="popup" />
    </StackLayout>
</ContentPage>
```

**MainPage.xaml.cs:**
```csharp
using Syncfusion.Maui.Popup;

namespace GettingStarted
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
        }

        private void ClickToShowPopup_Clicked(object sender, EventArgs e)
        {
            // Display the popup at the center
            popup.Show();
        }
    }
}
```

## Closing the Popup

### Method 1: Using Dismiss() Method

```csharp
private void ClickToDismissPopup_Clicked(object sender, EventArgs e)
{
    // Close the popup programmatically
    popup.Dismiss();
}
```

### Method 2: Using IsOpen Property

```csharp
private void ClickToDismissPopup_Clicked(object sender, EventArgs e)
{
    // Set IsOpen to false to close the popup
    popup.IsOpen = false;
}
```

### User-Initiated Close

Users can close the popup by:
- Tapping the close button (if `ShowCloseButton="True"`)
- Tapping the accept or decline buttons (if footer is shown)
- Tapping the overlay background (default behavior)

## Native Embedding

The SfPopup supports native embedding scenarios where you embed MAUI content in native Android or iOS applications.

### Android Native Embedding

Create the popup by passing the current `Activity` and `IMauiContext`:

```csharp
using Android.App;
using Android.Content.PM;
using Android.OS;
using Microsoft.Maui.Controls.Embedding;
using Microsoft.Maui.Platform;
using Syncfusion.Maui.Core.Hosting;
using Syncfusion.Maui.Popup;

namespace AndroidApp
{
    [Activity(Theme = "@style/Maui.SplashTheme", 
              MainLauncher = true, 
              LaunchMode = LaunchMode.SingleTop)]
    public class MainActivity : Activity
    {
        SfPopup? popup;
        Microsoft.Maui.Controls.Button? button;

        protected override void OnCreate(Bundle? savedInstanceState)
        {
            base.OnCreate(savedInstanceState);

            // Create MAUI app builder
            MauiAppBuilder builder = MauiApp.CreateBuilder();
            builder.UseMauiEmbeddedApp<Microsoft.Maui.Controls.Application>();
            builder.ConfigureSyncfusionCore();
            MauiApp mauiApp = builder.Build();

            // Create MAUI context
            MauiContext _mauiContext = new MauiContext(mauiApp.Services, this);

            // Create layout
            Microsoft.Maui.Controls.StackLayout views = new();
            views.HeightRequest = 700;

            // Create button
            button = new Microsoft.Maui.Controls.Button();
            button.Text = "Click";
            button.HeightRequest = 50;

            // Create popup with Activity and MauiContext
            popup = new SfPopup(this, _mauiContext);

            button.Clicked += (s, e) =>
            {
                popup.Show();
            };

            views.Children.Add(button);
            views.Children.Add(popup);

            // Convert MAUI view to Android native view
            Android.Views.View nativeview = views.ToPlatform(_mauiContext);            
            SetContentView(nativeview);
        }
    }
}
```

### iOS Native Embedding

Create the popup using the native `UIWindow` and `MauiContext`:

```csharp
using Foundation;
using Microsoft.Maui.Controls.Embedding;
using UIKit;
using Syncfusion.Maui.Popup;
using Syncfusion.Maui.Core.Hosting;
using Microsoft.Maui.Platform;

namespace iOSApp
{
    [Register("SceneDelegate")]
    public class SceneDelegate : UIResponder, IUIWindowSceneDelegate
    {
        [Export("window")]
        public UIWindow? Window { get; set; }

        [Export("scene:willConnectToSession:options:")]
        public void WillConnect(UIScene scene, UISceneSession session, 
                                UISceneConnectionOptions connectionOptions)
        {
            if (scene is UIWindowScene windowScene)
            {
                Window ??= new UIWindow(windowScene);
                var viewController = new UIViewController();

                // Create MAUI app builder
                MauiAppBuilder builder = MauiApp.CreateBuilder();
                builder.UseMauiEmbeddedApp<Microsoft.Maui.Controls.Application>();
                builder.ConfigureSyncfusionCore();

                MauiApp mauiApp = builder.Build();
                MauiContext _mauiContext = new MauiContext(mauiApp.Services);

                // Create layout
                Microsoft.Maui.Controls.StackLayout views = new();

                // Create popup with UIWindow and MauiContext
                var popup = new SfPopup(Window, _mauiContext);

                var button = new Microsoft.Maui.Controls.Button();
                button.Text = "Click";
                button.HeightRequest = 50;
                button.Clicked += (s, e) =>
                {
                    popup.Show();
                };

                views.Children.Add(button);
                views.Children.Add(popup);

                // Convert MAUI view to iOS native view
                var nativeView = views.ToPlatform(_mauiContext);
                nativeView.Frame = viewController.View!.Frame;
                nativeView.BackgroundColor = UIColor.SystemBackground;
                viewController.View.AddSubview(nativeView);

                Window.RootViewController = viewController;
                Window.MakeKeyAndVisible();
            }
        }
    }
}
```

## Troubleshooting

### Issue: Popup doesn't appear when Show() is called

**Solution:**
- Verify that `ConfigureSyncfusionCore()` is registered in `MauiProgram.cs`
- Check that the popup control is added to the visual tree
- Ensure the button click event is properly wired up

### Issue: NullReferenceException when accessing popup

**Solution:**
- Verify the popup is initialized before calling methods
- Check that the `x:Name` attribute matches the code-behind reference
- Ensure `InitializeComponent()` is called in the constructor

### Issue: NuGet restore fails

**Solution:**
```bash
dotnet restore
dotnet clean
dotnet build
```

### Issue: "Could not find type" error

**Solution:**
- Ensure the XML namespace is correctly defined:
  ```xml
  xmlns:sfPopup="clr-namespace:Syncfusion.Maui.Popup;assembly=Syncfusion.Maui.Popup"
  ```
- Clean and rebuild the project

### Issue: Popup appears but content is empty

**Solution:**
- Set a `Message` property or provide a `ContentTemplate`
- Check that `HeightRequest` and `WidthRequest` are appropriate values

## Next Steps

Now that you have a basic popup working, explore:
- **Positioning**: Display popup at specific locations or relative to views
- **Layout Customization**: Add custom headers, footers, and content
- **Animations**: Configure entry and exit animations
- **Events**: Handle popup lifecycle events
- **Modal Behavior**: Configure overlay and interaction settings
