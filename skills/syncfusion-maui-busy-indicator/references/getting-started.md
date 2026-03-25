# Getting Started with Busy Indicator

This guide walks through setting up and implementing the Syncfusion .NET MAUI Busy Indicator (SfBusyIndicator) in your application, from initial project setup to your first working implementation.

## Step 1: Create a .NET MAUI Project

### Using Visual Studio

1. Open Visual Studio and go to **File > New > Project**
2. Search for and select the **.NET MAUI App** template
3. Click **Next**
4. Enter your **Project Name** (e.g., "BusyIndicatorDemo")
5. Choose a **Location** for your project
6. Click **Next**
7. Select the **.NET framework version** (9.0 or later recommended)
8. Click **Create**

### Using .NET CLI

```bash
dotnet new maui -n BusyIndicatorDemo
cd BusyIndicatorDemo
```

## Step 2: Install Syncfusion.Maui.Core NuGet Package

The Busy Indicator is part of the Syncfusion.Maui.Core package, which is a dependency package for all Syncfusion .NET MAUI controls.

### Using Visual Studio

1. In **Solution Explorer**, right-click your project
2. Select **Manage NuGet Packages**
3. Click the **Browse** tab
4. Search for **`Syncfusion.Maui.Core`**
5. Select the package and click **Install**
6. Accept the license agreement
7. Wait for the package and its dependencies to install

### Using Package Manager Console

```powershell
Install-Package Syncfusion.Maui.Core
```

### Using .NET CLI

```bash
dotnet add package Syncfusion.Maui.Core
```

**Important:** Ensure you install the latest stable version compatible with your .NET MAUI version.

## Step 3: Register the Syncfusion Handler

After installing the package, you must register the Syncfusion Core handler in your `MauiProgram.cs` file.

### Modify MauiProgram.cs

```csharp
using Microsoft.Extensions.Logging;
using Syncfusion.Maui.Core.Hosting;  // Add this using statement

namespace BusyIndicatorDemo
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .ConfigureSyncfusionCore()  // Add this line
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

**Key changes:**
1. Add `using Syncfusion.Maui.Core.Hosting;` at the top
2. Call `.ConfigureSyncfusionCore()` in the builder chain

## Step 4: Implement Your First Busy Indicator

### XAML Implementation

1. Open `MainPage.xaml`
2. Add the Syncfusion namespace
3. Add the SfBusyIndicator control

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:core="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
             x:Class="BusyIndicatorDemo.MainPage">

    <ContentPage.Content>
        <core:SfBusyIndicator x:Name="busyIndicator"
                              IsRunning="True" />
    </ContentPage.Content>
</ContentPage>
```

### C# Implementation

Alternatively, create the Busy Indicator entirely in code:

```csharp
using Syncfusion.Maui.Core;

namespace BusyIndicatorDemo
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            var busyIndicator = new SfBusyIndicator
            {
                IsRunning = true
            };
            
            Content = busyIndicator;
        }
    }
}
```

## Step 5: Set an Animation Type

The default animation type is CircularMaterial. You can specify a different animation type using the `AnimationType` property.

### XAML Example

```xml
<core:SfBusyIndicator x:Name="busyIndicator"
                      IsRunning="True"
                      AnimationType="CircularMaterial" />
```

### C# Example

```csharp
var busyIndicator = new SfBusyIndicator
{
    IsRunning = true,
    AnimationType = AnimationType.CircularMaterial
};
```

**Available animation types:**
- `CircularMaterial` - Android Material Design style (default)
- `LinearMaterial` - Horizontal progress bar style
- `Cupertino` - iOS-style spinner
- `SingleCircle` - Single rotating circle
- `DoubleCircle` - Two concentric rotating circles
- `Globe` - 3D globe rotation effect
- `HorizontalPulsingBox` - Pulsing box animation

## Complete Working Example

Here's a complete working example with a button to toggle the busy state:

### MainPage.xaml

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:core="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
             x:Class="BusyIndicatorDemo.MainPage">

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <!-- Content area -->
        <StackLayout Grid.Row="0" Padding="20" VerticalOptions="Center">
            <Label Text="Welcome to Busy Indicator Demo"
                   FontSize="24"
                   HorizontalOptions="Center" />
            <Label Text="Click the button below to simulate loading"
                   HorizontalOptions="Center"
                   Margin="0,10,0,0" />
        </StackLayout>

        <!-- Button -->
        <Button Grid.Row="1"
                Text="Simulate Loading"
                Clicked="OnLoadClicked"
                Margin="20" />

        <!-- Busy Indicator Overlay -->
        <core:SfBusyIndicator x:Name="busyIndicator"
                              Grid.RowSpan="2"
                              IsRunning="False"
                              AnimationType="CircularMaterial" />
    </Grid>
</ContentPage>
```

### MainPage.xaml.cs

```csharp
namespace BusyIndicatorDemo
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
        }

        private async void OnLoadClicked(object sender, EventArgs e)
        {
            // Show busy indicator
            busyIndicator.IsRunning = true;

            try
            {
                // Simulate a long-running operation
                await Task.Delay(3000);
                
                await DisplayAlert("Success", "Operation completed!", "OK");
            }
            finally
            {
                // Hide busy indicator
                busyIndicator.IsRunning = false;
            }
        }
    }
}
```

## Running the Application

1. Select your target platform (Android, iOS, Windows, or macOS)
2. Press **F5** or click **Debug > Start Debugging**
3. The application should launch with the Busy Indicator visible
4. Click the "Simulate Loading" button to see the indicator in action

## Troubleshooting

### Issue: Busy Indicator not appearing

**Solution:**
- Verify `ConfigureSyncfusionCore()` is called in `MauiProgram.cs`
- Ensure `IsRunning` is set to `true`
- Check that the control is not hidden behind other elements
- Verify the Syncfusion.Maui.Core package is properly installed

### Issue: Namespace not found error

**Solution:**
- Add `using Syncfusion.Maui.Core;` in C# files
- Add `xmlns:core="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"` in XAML files
- Rebuild the solution

### Issue: License validation error

**Solution:**
- Register your Syncfusion license key in `MauiProgram.cs` before `CreateMauiApp()`:
```csharp
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
```

### Issue: Control not rendering on specific platform

**Solution:**
- Ensure platform-specific dependencies are installed
- Clean and rebuild the solution
- Check minimum platform version requirements
