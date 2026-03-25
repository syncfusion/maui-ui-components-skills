# Getting Started with .NET MAUI Switch

## Table of Contents
- [Installation Steps](#installation-steps)
  - [Step 1: Create a New .NET MAUI Project](#step-1-create-a-new-net-maui-project)
  - [Step 2: Install the NuGet Package](#step-2-install-the-nuget-package)
  - [Step 3: Register the Syncfusion Handler](#step-3-register-the-syncfusion-handler)
- [Basic Switch Implementation](#basic-switch-implementation)
- [Setting Initial State](#setting-initial-state)
- [Performing Actions Based on State](#performing-actions-based-on-state)
- [Complete Quick Start Example](#complete-quick-start-example)
- [Common Issues and Troubleshooting](#common-issues-and-troubleshooting)

This guide walks you through setting up and implementing the Syncfusion .NET MAUI Switch (SfSwitch) control in your .NET MAUI application.

## Installation Steps

### Step 1: Create a New .NET MAUI Project

**Visual Studio:**
1. Go to **File > New > Project**
2. Select the **.NET MAUI App** template
3. Name your project (e.g., "SwitchExample")
4. Choose a location and click **Next**
5. Select the .NET framework version (9.0 or later)
6. Click **Create**

### Step 2: Install the NuGet Package

**Option A: Using NuGet Package Manager UI**
1. In **Solution Explorer**, right-click your project
2. Select **Manage NuGet Packages**
3. Click the **Browse** tab
4. Search for `Syncfusion.Maui.Buttons`
5. Select the package from the results
6. Click **Install** to add it to your project
7. Accept the license agreement
8. Wait for installation to complete and ensure all dependencies are restored

**Option B: Using Package Manager Console**
```powershell
Install-Package Syncfusion.Maui.Buttons
```

**Option C: Using .NET CLI**
```bash
dotnet add package Syncfusion.Maui.Buttons
```

### Step 3: Register the Syncfusion Handler

The Syncfusion.Maui.Core NuGet package is automatically installed as a dependency. You must register the Syncfusion handler in your `MauiProgram.cs` file.

**MauiProgram.cs:**
```csharp
using Microsoft.Extensions.Logging;
using Syncfusion.Maui.Core.Hosting;

namespace SwitchExample
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .ConfigureSyncfusionCore()  // ← Register Syncfusion handler
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

**Critical:** The `ConfigureSyncfusionCore()` method must be called before `Build()` is invoked.

## Basic Switch Implementation

### Step 4: Add the Switch Control

**Option A: XAML Implementation**

1. Open your `MainPage.xaml` file
2. Add the Syncfusion namespace at the top:

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:buttons="clr-namespace:Syncfusion.Maui.Buttons;assembly=Syncfusion.Maui.Buttons"
             x:Class="SwitchExample.MainPage">
    
    <VerticalStackLayout Padding="30" Spacing="25">
        <Label Text="Toggle Switch Example" 
               FontSize="18" 
               FontAttributes="Bold"/>
        
        <buttons:SfSwitch x:Name="sfSwitch" />
    </VerticalStackLayout>
</ContentPage>
```

**Option B: C# Implementation**

In your `MainPage.xaml.cs` or code-behind file:

```csharp
using Syncfusion.Maui.Buttons;

namespace SwitchExample
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            // Create switch control
            SfSwitch sfSwitch = new SfSwitch();
            
            // Set as page content
            this.Content = sfSwitch;
        }
    }
}
```

**Result:** A basic switch control will appear with default styling and behavior.

## Setting Initial State

You can set the initial state of the switch using the `IsOn` property:

**XAML:**
```xml
<!-- Switch in On state -->
<buttons:SfSwitch IsOn="True" />

<!-- Switch in Off state (default) -->
<buttons:SfSwitch IsOn="False" />
```

**C#:**
```csharp
SfSwitch onSwitch = new SfSwitch { IsOn = true };
SfSwitch offSwitch = new SfSwitch { IsOn = false };
```

## Performing Actions Based on State

Use the `StateChanged` event to respond when the switch state changes.

**XAML:**
```xml
<buttons:SfSwitch x:Name="sfSwitch" 
                  StateChanged="OnSwitchStateChanged"/>
```

**Code-behind (MainPage.xaml.cs):**
```csharp
using Syncfusion.Maui.Buttons;

namespace SwitchExample
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
        }
        
        private void OnSwitchStateChanged(object sender, SwitchStateChangedEventArgs e)
        {
            bool? newValue = e.NewValue;
            bool? oldValue = e.OldValue;
            
            // Perform action based on state
            if (newValue == true)
            {
                DisplayAlert("Switch", "Switched ON", "OK");
            }
            else if (newValue == false)
            {
                DisplayAlert("Switch", "Switched OFF", "OK");
            }
        }
    }
}
```

**C# Only (No XAML):**
```csharp
public MainPage()
{
    InitializeComponent();
    
    SfSwitch sfSwitch = new SfSwitch();
    sfSwitch.StateChanged += OnSwitchStateChanged;
    
    this.Content = sfSwitch;
}

private void OnSwitchStateChanged(object sender, SwitchStateChangedEventArgs e)
{
    DisplayAlert("Success", $"State changed to: {e.NewValue}", "OK");
}
```

## Complete Quick Start Example

Here's a complete working example that demonstrates basic switch usage with state handling:

**MainPage.xaml:**
```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:buttons="clr-namespace:Syncfusion.Maui.Buttons;assembly=Syncfusion.Maui.Buttons"
             x:Class="SwitchExample.MainPage">

    <VerticalStackLayout Padding="30" Spacing="25">
        <Label Text="Notification Settings" 
               FontSize="22" 
               FontAttributes="Bold"
               HorizontalOptions="Center"/>
        
        <HorizontalStackLayout Spacing="15">
            <Label Text="Enable Notifications" 
                   VerticalOptions="Center"
                   FontSize="16"/>
            <buttons:SfSwitch x:Name="notificationSwitch" 
                              IsOn="True"
                              StateChanged="OnNotificationSwitchChanged"/>
        </HorizontalStackLayout>
        
        <Label x:Name="statusLabel" 
               Text="Notifications are enabled"
               FontSize="14"
               TextColor="Green"/>
    </VerticalStackLayout>
</ContentPage>
```

**MainPage.xaml.cs:**
```csharp
using Syncfusion.Maui.Buttons;

namespace SwitchExample
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
        }
        
        private void OnNotificationSwitchChanged(object sender, SwitchStateChangedEventArgs e)
        {
            if (e.NewValue == true)
            {
                statusLabel.Text = "Notifications are enabled";
                statusLabel.TextColor = Colors.Green;
            }
            else
            {
                statusLabel.Text = "Notifications are disabled";
                statusLabel.TextColor = Colors.Red;
            }
        }
    }
}
```

## Common Issues and Troubleshooting

### Issue 1: Switch Not Appearing
**Symptom:** The switch control doesn't render on the page.

**Solution:**
- Ensure `ConfigureSyncfusionCore()` is called in `MauiProgram.cs`
- Verify the NuGet package is properly installed
- Check that the namespace is correctly imported
- Clean and rebuild your solution

### Issue 2: Events Not Firing
**Symptom:** StateChanged event handler is not called.

**Solution:**
- Verify the event handler method signature matches the event signature
- Ensure the event is properly wired up in XAML or C#
- Check that the switch is not in a disabled state

### Issue 3: Runtime Errors
**Symptom:** Application crashes when using the switch.

**Solution:**
- Ensure all Syncfusion dependencies are installed (check `Syncfusion.Maui.Core`)
- Verify .NET MAUI workload is properly installed
- Update to the latest Syncfusion package version
- Check platform-specific requirements are met