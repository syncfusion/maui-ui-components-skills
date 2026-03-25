# Getting Started with .NET MAUI DockLayout

## Table of Contents
- [Creating a New .NET MAUI Project](#creating-a-new-net-maui-project)
- [Installing the Syncfusion .NET MAUI Core Package](#installing-the-syncfusion-net-maui-core-package)
- [Registering the Syncfusion Core Handler](#registering-the-syncfusion-core-handler)
- [Initializing the DockLayout Control](#initializing-the-docklayout-control)
- [Setting Dock Positions for Child Views](#setting-dock-positions-for-child-views)
- [Quick Reference Video](#quick-reference-video)
- [Sample Project](#sample-project)
- [Next Steps](#next-steps)
- [Common Issues](#common-issues)

This guide provides complete setup instructions for implementing the Syncfusion .NET MAUI DockLayout (SfDockLayout) control in your applications, covering installation, and basic usage across Visual Studio, Visual Studio Code, and JetBrains Rider.

## Creating a New .NET MAUI Project

### Visual Studio
1. Go to **File > New > Project**
2. Select the **.NET MAUI App** template
3. Click **Next**
4. Enter a project name and location
5. Click **Create**

### Visual Studio Code
1. Open the command palette by pressing `Ctrl+Shift+P`
2. Type **.NET:New Project** and press **Enter**
3. Choose the **.NET MAUI App** template
4. Select the project location
5. Type the project name and press **Enter**
6. Choose **Create project**

### JetBrains Rider
1. Go to **File > New Solution**
2. Select **.NET (C#)** and choose the **.NET MAUI App** template
3. Enter the Project Name, Solution Name, and Location
4. Select the .NET framework version
5. Click **Create**

## Installing the Syncfusion .NET MAUI Core Package

The DockLayout control is part of the `Syncfusion.Maui.Core` package.

### Visual Studio
1. Right-click on the project in **Solution Explorer**
2. Choose **Manage NuGet Packages**
3. Search for **Syncfusion.Maui.Core**
4. Install the latest version
5. Ensure all dependent packages are installed and the project builds successfully

### Visual Studio Code
1. Press <kbd>Ctrl</kbd> + <kbd>`</kbd> (backtick) to open the integrated terminal
2. Ensure you're in the project root directory where your .csproj file is located
3. Run the command:
   ```bash
   dotnet add package Syncfusion.Maui.Core
   ```
4. To ensure all dependencies are installed, run:
   ```bash
   dotnet restore
   ```

### JetBrains Rider
1. In **Solution Explorer**, right-click the project
2. Choose **Manage NuGet Packages**
3. Search for **Syncfusion.Maui.Core**
4. Install the latest version
5. If needed, manually run in the Terminal:
   ```bash
   dotnet restore
   ```

## Registering the Syncfusion Core Handler

The `Syncfusion.Maui.Core` NuGet package is a dependency for all Syncfusion controls in .NET MAUI. You must register the handler in your **MauiProgram.cs** file.

### Step 1: Import the Namespace
Add the following using statement at the top of **MauiProgram.cs**:

```csharp
using Syncfusion.Maui.Core.Hosting;
```

### Step 2: Configure Syncfusion Core
In the `CreateMauiApp()` method, call `.ConfigureSyncfusionCore()`:

```csharp
using Microsoft.Extensions.Logging;
using Syncfusion.Maui.Core.Hosting;

namespace DockLayoutGettingStarted
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
                });

            return builder.Build();
        }
    }
}
```

**Important:** Without this registration, Syncfusion controls will not function properly.

## Initializing the DockLayout Control

### Import the Namespace

#### XAML
Add the Syncfusion namespace to your ContentPage:

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sf="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
             x:Class="DockLayoutGettingStarted.MainPage">
   
     <sf:SfDockLayout />

</ContentPage>
```

#### C#
Add the using statement and create an instance:

```csharp
using Syncfusion.Maui.Core;

namespace DockLayoutGettingStarted
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();  
            SfDockLayout dockLayout = new SfDockLayout();
            Content = dockLayout;       
        }
    }   
}
```

## Setting Dock Positions for Child Views

Child views inside the SfDockLayout can be arranged using the `Dock` attached property. This property allows you to dock elements to specific edges: **Top**, **Bottom**, **Left**, **Right**, or set to **None** to remain non-docked and fill the remaining space.

### Complete Example

#### XAML

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sf="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
             x:Class="DockLayoutGettingStarted.MainPage">

   <ContentPage.Content>
        <sf:SfDockLayout>
            <!-- Left sidebar -->
            <Label Text="Left" 
                   WidthRequest="80" 
                   sf:SfDockLayout.Dock="Left" 
                   Background="#E57373" />
            
            <!-- Right sidebar -->
            <Label Text="Right" 
                   WidthRequest="80" 
                   sf:SfDockLayout.Dock="Right" 
                   Background="#BA68C8" />
            
            <!-- Top header -->
            <Label Text="Top" 
                   HeightRequest="80" 
                   sf:SfDockLayout.Dock="Top" 
                   Background="#F06292" />
            
            <!-- Bottom footer -->
            <Label Text="Bottom" 
                   HeightRequest="80"  
                   sf:SfDockLayout.Dock="Bottom" 
                   Background="#9575CD" />
            
            <!-- Center content (automatically expands to fill remaining space) -->
            <Label Text="None" 
                   BackgroundColor="#64B5F6" />
        </sf:SfDockLayout>
    </ContentPage.Content>
    
</ContentPage>
```

#### C#

```csharp
using Syncfusion.Maui.Core;

namespace DockLayoutGettingStarted
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            SfDockLayout dockLayout = new SfDockLayout();
            
            // Add left element
            dockLayout.Children.Add(
                new Label 
                { 
                    Text = "Left", 
                    WidthRequest = 80, 
                    Background = Color.FromArgb("#E57373") 
                }, 
                Dock.Left
            );
            
            // Add right element
            dockLayout.Children.Add(
                new Label 
                { 
                    Text = "Right", 
                    WidthRequest = 80, 
                    Background = Color.FromArgb("#BA68C8") 
                }, 
                Dock.Right
            );
            
            // Add top element
            dockLayout.Children.Add(
                new Label 
                { 
                    Text = "Top", 
                    HeightRequest = 80, 
                    Background = Color.FromArgb("#F06292") 
                }, 
                Dock.Top
            );
            
            // Add bottom element
            dockLayout.Children.Add(
                new Label 
                { 
                    Text = "Bottom", 
                    HeightRequest = 80, 
                    Background = Color.FromArgb("#9575CD") 
                }, 
                Dock.Bottom
            );
            
            // Add center element (no dock position, fills remaining space)
            dockLayout.Children.Add(
                new Label 
                { 
                    Text = "None", 
                    Background = Color.FromArgb("#64B5F6") 
                }
            );
            
            Content = dockLayout;   
        }
    }
}
```

### How It Works

1. **Docked Elements**: Elements with explicit dock positions (Top, Bottom, Left, Right) are positioned along the specified edge
2. **Sizing**: Docked elements need either `WidthRequest` (for Left/Right) or `HeightRequest` (for Top/Bottom)
3. **Last Child**: By default, the last child element (or any element with `Dock.None`) automatically expands to fill the remaining space
4. **Order Matters**: Elements are docked in the order they are added, which affects how space is allocated

## Quick Reference Video

For a visual walkthrough of the DockLayout setup process, watch the [official Syncfusion video tutorial](https://www.youtube.com/watch?v=g2NU8b_9aAg).

## Sample Project

Access a complete getting started sample from the [Syncfusion GitHub repository](https://github.com/SyncfusionExamples/GettingStarted_DockLayout_MAUI).

## Next Steps

- **Configure spacing**: See [spacing-layout.md](spacing-layout.md) for HorizontalSpacing and VerticalSpacing
- **Advanced docking**: See [docking-features.md](docking-features.md) for GetDock/SetDock methods
- **RTL support**: See [advanced-features.md](advanced-features.md) for Right-to-Left layouts

## Common Issues

### Build Errors After Installing Package
- Ensure you called `.ConfigureSyncfusionCore()` in MauiProgram.cs
- Run `dotnet restore` to ensure all dependencies are installed
- Clean and rebuild the project

### Control Not Rendering
- Verify the namespace is correctly imported (`xmlns:sf` in XAML or `using Syncfusion.Maui.Core` in C#)
- Check that Syncfusion handler is registered in MauiProgram.cs
- Ensure the NuGet package version is compatible with your .NET version

### Elements Overlapping
- Verify docked elements have appropriate `WidthRequest` or `HeightRequest` values
- Check the order in which children are added (docking order matters)
- See [docking-features.md](docking-features.md) for detailed positioning behavior
