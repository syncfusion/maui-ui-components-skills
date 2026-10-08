# Getting Started with Interactive Viewer

## Table of Contents
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Handler Registration](#handler-registration)
- [Basic Setup](#basic-setup)
- [Complete Example](#complete-example)

## Prerequisites

Before implementing the Interactive Viewer, ensure your development environment is properly configured:

1. **Install .NET 9 SDK** or later from [Microsoft's download page](https://dotnet.microsoft.com/en-us/download/dotnet/9.0)
2. **Set up .NET MAUI** with one of these IDEs:
   - Visual Studio 2022 v17.12 or later
   - Visual Studio Code with MAUI extensions
   - JetBrains Rider 2024.3 or later
3. **Create a new .NET MAUI Application** using your preferred IDE

## Installation

### Via Visual Studio

1. Right-click your project in Solution Explorer
2. Select **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.InteractiveViewer`
4. Click **Install** and wait for dependencies to resolve
5. The project will automatically restore

### Via Visual Studio Code

```bash
# Navigate to your project directory
cd YourMauiProject

# Install the NuGet package
dotnet add package Syncfusion.Maui.InteractiveViewer

# Restore dependencies
dotnet restore
```

### Via JetBrains Rider

1. Right-click your project in Solution Explorer
2. Select **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.InteractiveViewer`
4. Click **Install** and wait for completion

## Handler Registration

Register the Syncfusion core handler in your `MauiProgram.cs` file:

```c#
using Syncfusion.Maui.Core.Hosting;

namespace YourNamespace;

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
            })
            .ConfigureSyncfusionCore();  // ← Add this line
        
        return builder.Build();
    }
}
```

## Basic Setup

### Step 1: Add Namespace to Your Page/View

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:interactiveViewer="clr-namespace:Syncfusion.Maui.InteractiveViewer;assembly=Syncfusion.Maui.InteractiveViewer"
             x:Class="YourNamespace.MainPage"
             Title="Interactive Viewer">
```

### Step 2: Add the InteractiveViewer Control

```xaml
<interactiveViewer:SfInteractiveViewer x:Name="viewer">
    <Image Source="image.png" Aspect="AspectFit" />
</interactiveViewer:SfInteractiveViewer>
```

### Step 3: Access from Code-Behind (Optional)

```c#
using Syncfusion.Maui.InteractiveViewer;

namespace YourNamespace;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }

    // You can now access viewer in your code-behind
    private void DoSomething()
    {
        // Use viewer here
    }
}
```

## Complete Example

### Full XAML Page

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:interactiveViewer="clr-namespace:Syncfusion.Maui.InteractiveViewer;assembly=Syncfusion.Maui.InteractiveViewer"
             x:Class="InteractiveViewerDemo.MainPage"
             Title="Image Viewer">
    
    <Grid RowDefinitions="*,Auto" Padding="10">
        <!-- Interactive Viewer -->
        <interactiveViewer:SfInteractiveViewer x:Name="viewer"
                                               Grid.Row="0">
            <Image Source="sample_image.png" 
                   Aspect="AspectFit" />
        </interactiveViewer:SfInteractiveViewer>
        
        <!-- Control Buttons -->
        <StackLayout Grid.Row="1" 
                     Orientation="Horizontal" 
                     Spacing="10"
                     Padding="0,10">
            <Button Text="Rotate" 
                    Clicked="OnRotateClicked"
                    HorizontalOptions="FillAndExpand" />
            <Button Text="Reset" 
                    Clicked="OnResetClicked"
                    HorizontalOptions="FillAndExpand" />
        </StackLayout>
    </Grid>
</ContentPage>
```

### Code-Behind

```c#
using Syncfusion.Maui.InteractiveViewer;

namespace InteractiveViewerDemo;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }

    private void OnRotateClicked(object sender, EventArgs e)
    {
        viewer.Rotate();
    }

    private void OnResetClicked(object sender, EventArgs e)
    {
        viewer.Reset();
    }
}
```

## Next Steps

- **Zooming & Panning** - Learn how to control zoom levels and pan behavior
- **Rotation** - Master 90-degree content rotation
- **Reset** - Understand how to restore default view
- **Events** - Handle user interactions programmatically
