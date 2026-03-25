# Getting Started with .NET MAUI PullToRefresh

## Table of Contents
- [Overview](#overview)
- [Step 1: Create a New .NET MAUI Project](#step-1-create-a-new-net-maui-project)
- [Step 2: Install the Syncfusion MAUI PullToRefresh NuGet Package](#step-2-install-the-syncfusion-maui-pulltorefresh-nuget-package)
- [Step 3: Register the Handler](#step-3-register-the-handler)
- [Step 4: Add a Basic PullToRefresh](#step-4-add-a-basic-pulltorefresh)
- [Step 5: Define the PullableContent](#step-5-define-the-pullablecontent)
- [Step 6: Running the Application](#step-6-running-the-application)
- [TransitionMode Comparison](#transitionmode-comparison)
- [Important Notes](#important-notes)

## Overview

This guide walks through setting up and configuring a Syncfusion .NET MAUI PullToRefresh control in your application. The PullToRefresh control provides an interactive way for users to refresh content by pulling down on a view.

## Step 1: Create a New .NET MAUI Project

### Using Visual Studio

1. Open Visual Studio 2022
2. Select **File > New > Project**
3. Search for ".NET MAUI App" template
4. Enter Project Name and Location
5. Select .NET 9.0 or later as the framework
6. Click **Create**

### Using JetBrains Rider

1. Go to **File > New Solution**
2. Select **.NET (C#)** and choose the **.NET MAUI App** template
3. Enter the Project Name, Solution Name, and Location
4. Select the .NET framework version
5. Click **Create**

### Using Command Line

```bash
dotnet new maui -n MyPullToRefreshApp
cd MyPullToRefreshApp
```

## Step 2: Install the Syncfusion MAUI PullToRefresh NuGet Package

### Using Visual Studio

1. In **Solution Explorer**, right-click the project
2. Select **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.PullToRefresh`
4. Install the latest version

### Using Package Manager Console

```powershell
Install-Package Syncfusion.Maui.PullToRefresh
```

### Using .NET CLI

```bash
dotnet add package Syncfusion.Maui.PullToRefresh
```

### Using JetBrains Rider

1. In **Solution Explorer**, right-click the project
2. Choose **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.PullToRefresh` and install the latest version
4. If needed, manually restore: `dotnet restore`

## Step 3: Register the Handler

The `Syncfusion.Maui.Core` NuGet package is a dependency for all Syncfusion MAUI controls. You must register the Syncfusion handler in `MauiProgram.cs`.

### MauiProgram.cs Configuration

```csharp
using Microsoft.Maui;
using Microsoft.Maui.Hosting;
using Microsoft.Maui.Controls.Hosting;
using Syncfusion.Maui.Core.Hosting;

namespace MyPullToRefreshApp
{
    public static class MauiProgram
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

            // Register Syncfusion Core Handler (REQUIRED)
            builder.ConfigureSyncfusionCore();
            
            return builder.Build();
        }
    }
}
```

**Important:** The `ConfigureSyncfusionCore()` call is mandatory for all Syncfusion MAUI controls to function properly.

## Step 4: Add a Basic PullToRefresh

### XAML Implementation

Open `MainPage.xaml` and add the PullToRefresh control:

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.PullToRefresh;assembly=Syncfusion.Maui.PullToRefresh"
             x:Class="MyPullToRefreshApp.MainPage">

    <syncfusion:SfPullToRefresh x:Name="pullToRefresh" />
    
</ContentPage>
```

### C# Code Implementation

Alternatively, you can create the control entirely in code:

```csharp
using Syncfusion.Maui.PullToRefresh;

namespace MyPullToRefreshApp
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            // Create PullToRefresh control
            SfPullToRefresh pullToRefresh = new SfPullToRefresh();
            
            this.Content = pullToRefresh;
        }
    }
}
```

## Step 5: Define the PullableContent

The `PullableContent` property defines the view that users can pull to refresh. This is where you place your scrollable content.

### Basic PullableContent Example

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.PullToRefresh;assembly=Syncfusion.Maui.PullToRefresh"
             x:Class="MyPullToRefreshApp.MainPage">

    <syncfusion:SfPullToRefresh x:Name="pullToRefresh"
                                 Refreshing="PullToRefresh_Refreshing">
        <syncfusion:SfPullToRefresh.PullableContent>
            <StackLayout BackgroundColor="LightBlue"
                         Padding="20">
                <Label Text="Pull down to refresh this content"
                       FontSize="18"
                       HorizontalOptions="Center"
                       VerticalOptions="Center"/>
            </StackLayout>
        </syncfusion:SfPullToRefresh.PullableContent>
    </syncfusion:SfPullToRefresh>
    
</ContentPage>
```

### Handling the Refreshing Event

In the code-behind file (`MainPage.xaml.cs`), handle the refresh action:

```csharp
using Syncfusion.Maui.PullToRefresh;

namespace MyPullToRefreshApp
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
        }

        private async void PullToRefresh_Refreshing(object sender, EventArgs e)
        {
            // Start the refresh indicator
            pullToRefresh.IsRefreshing = true;
            
            // Simulate data refresh operation (e.g., API call)
            await Task.Delay(2000);
            
            // Stop the refresh indicator
            pullToRefresh.IsRefreshing = false;
        }
    }
}
```

### Understanding IsRefreshing Property

- **Set to `true`:** Shows the progress indicator and begins the refresh animation
- **Set to `false`:** Hides the progress indicator and ends the refresh animation

The typical pattern is:
1. User pulls down on PullableContent
2. `Refreshing` event fires
3. Set `IsRefreshing = true` to show progress
4. Perform data refresh operation (async)
5. Set `IsRefreshing = false` to hide progress

## Step 6: Running the Application

### Visual Studio
Press **F5** or click **Run** to build and run the application.

### Command Line
```bash
dotnet build
dotnet run
```

### Testing the Pull-to-Refresh

1. Launch the application on your target platform
2. Pull down on the content area
3. The progress indicator appears
4. Release when the indicator is fully visible
5. The refresh animation plays for 2 seconds (simulated delay)
6. Content refreshes and the indicator disappears

## TransitionMode Comparison

The PullToRefresh supports two transition modes that control how the refresh indicator appears.

### SlideOnTop Mode (Default)

The refresh indicator slides on top of the pullable content.

```xml
<syncfusion:SfPullToRefresh x:Name="pullToRefresh"
                             TransitionMode="SlideOnTop"
                             Refreshing="PullToRefresh_Refreshing">
    <syncfusion:SfPullToRefresh.PullableContent>
        <StackLayout>
            <Label Text="Content with SlideOnTop mode" />
        </StackLayout>
    </syncfusion:SfPullToRefresh.PullableContent>
</syncfusion:SfPullToRefresh>
```

**Behavior:** The refresh view appears above the content, covering it during the pull.

### Push Mode

The refresh indicator pushes the content down as it appears.

```xml
<syncfusion:SfPullToRefresh x:Name="pullToRefresh"
                             TransitionMode="Push"
                             Refreshing="PullToRefresh_Refreshing">
    <syncfusion:SfPullToRefresh.PullableContent>
        <StackLayout>
            <Label Text="Content with Push mode" />
        </StackLayout>
    </syncfusion:SfPullToRefresh.PullableContent>
</syncfusion:SfPullToRefresh>
```

**Behavior:** The refresh view pushes the content down, and both move simultaneously.

### Choosing a Mode

- **SlideOnTop:** Better for overlaying refresh indicators without shifting content
- **Push:** Better when you want the content to move down with the indicator, providing a more integrated feel

## Important Notes

### Size and Layout Requirements

**Critical:** PullToRefresh does not have an intrinsic size. When loading inside layouts, you MUST set:
- Size explicitly (`HeightRequest`, `WidthRequest`)
- OR use `LayoutOptions` (e.g., `HorizontalOptions="FillAndExpand"`, `VerticalOptions="FillAndExpand"`)

**Example with Layout Options:**
```xml
<syncfusion:SfPullToRefresh HorizontalOptions="FillAndExpand"
                             VerticalOptions="FillAndExpand">
    <!-- PullableContent here -->
</syncfusion:SfPullToRefresh>
```

**Example with Explicit Size:**
```xml
<syncfusion:SfPullToRefresh HeightRequest="500"
                             WidthRequest="400">
    <!-- PullableContent here -->
</syncfusion:SfPullToRefresh>
```

### Common Pitfalls

1. **Forgetting ConfigureSyncfusionCore():** The control won't render without this registration
2. **Not setting IsRefreshing to false:** The refresh indicator will run indefinitely
3. **No size/layout options:** Control won't display properly in complex layouts
4. **Async without await:** Not awaiting async operations can cause premature completion

## Next Steps

Now that you have a basic PullToRefresh working:

- Explore **pullable-content.md** to learn how to host ListView, DataGrid, or custom views
- Check **customization.md** for appearance and behavior customization options
- Review **templates.md** for creating custom refresh indicators
- See **mvvm-commands.md** for MVVM pattern implementation
- Read **events.md** for advanced event handling scenarios
