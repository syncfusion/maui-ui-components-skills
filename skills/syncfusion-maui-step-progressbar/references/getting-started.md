# Getting Started with .NET MAUI StepProgressBar

This guide covers installation, setup, and basic implementation of the Syncfusion .NET MAUI Step ProgressBar (SfStepProgressBar) control.

## Table of Contents
- [Overview](#overview)
- [Installation](#installation)
- [Register the Handler](#register-the-handler)
- [Add StepProgressBar Control](#add-stepprogressbar-control)
- [Populate StepProgressBar Items](#populate-stepprogressbar-items)
- [Set Active Step](#set-active-step)
- [Complete Working Example](#complete-working-example)
- [Troubleshooting](#troubleshooting)

## Overview

This guide walks through setting up the Syncfusion .NET MAUI StepProgressBar control from scratch. The StepProgressBar displays progress through multiple steps in a sequential process, perfect for order tracking, registration forms, checkout flows, and multi-stage workflows.

## Installation

### Step 1: Create a New .NET MAUI Project

**Visual Studio:**
1. Go to **File > New > Project**
2. Choose the **.NET MAUI App** template
3. Name the project and choose a location
4. Click **Next**, select the .NET framework version, and click **Create**

**Visual Studio Code:**
1. Open command palette: `Ctrl+Shift+P`
2. Type **.NET:New Project** and press Enter
3. Choose **.NET MAUI App** template
4. Select project location, type project name, and press **Enter**
5. Choose **Create project**

### Step 2: Install NuGet Package

**Option 1: NuGet Package Manager (Visual Studio/Rider)**
1. Right-click the project in Solution Explorer
2. Choose **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.ProgressBar`
4. Install the latest version
5. Ensure dependencies are installed correctly

**Option 2: Command Line**
```bash
dotnet add package Syncfusion.Maui.ProgressBar
```

**Option 3: Package Reference (Manual)**

Add to your `.csproj` file:
```xml
<ItemGroup>
    <PackageReference Include="Syncfusion.Maui.ProgressBar" Version="*" />
</ItemGroup>
```

Then run:
```bash
dotnet restore
```

## Register the Handler

The `Syncfusion.Maui.Core` NuGet package is automatically installed as a dependency. You must register the Syncfusion handler in your `MauiProgram.cs` file.

### MauiProgram.cs Configuration

```csharp
using Syncfusion.Maui.Core.Hosting;

namespace YourAppNamespace
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();

            // Register Syncfusion handler - REQUIRED
            builder.ConfigureSyncfusionCore();

            builder
                .UseMauiApp<App>()
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                    fonts.AddFont("OpenSans-Semibold.ttf", "OpenSansSemibold");
                });

            return builder.Build();
        }
    }
}
```

**Important:** The `ConfigureSyncfusionCore()` call is mandatory for all Syncfusion .NET MAUI controls. Without it, controls will not render properly.

## Add StepProgressBar Control

### Import Namespace

First, import the Syncfusion.Maui.ProgressBar namespace:

**XAML:**
```xml
xmlns:stepProgressBar="clr-namespace:Syncfusion.Maui.ProgressBar;assembly=Syncfusion.Maui.ProgressBar"
```

**C#:**
```csharp
using Syncfusion.Maui.ProgressBar;
```

### Initialize the Control

**XAML Implementation:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:stepProgressBar="clr-namespace:Syncfusion.Maui.ProgressBar;assembly=Syncfusion.Maui.ProgressBar"
             x:Class="YourNamespace.MainPage">
    
    <stepProgressBar:SfStepProgressBar />
    
</ContentPage>
```

**C# Implementation:**
```csharp
using Syncfusion.Maui.ProgressBar;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        SfStepProgressBar stepProgressBar = new SfStepProgressBar();
        this.Content = stepProgressBar;
    }
}
```

At this point, the control is added but empty. You need to populate it with items.

## Populate StepProgressBar Items

The StepProgressBar uses an `ItemsSource` property that accepts a collection of `StepProgressBarItem` objects.

### Step 1: Create a ViewModel

Create a ViewModel class with an ObservableCollection:

```csharp
using System.Collections.ObjectModel;
using Syncfusion.Maui.ProgressBar;

namespace YourNamespace
{
    public class ViewModel
    {

        /// <summary>
        /// The Step progress bar item collection.
        /// </summary>
        private ObservableCollection<StepProgressBarItem> stepProgressItem;

        /// <summary>
        /// The Step progress bar item collection.
        /// </summary>
        public ObservableCollection<StepProgressBarItem> StepProgressItem
        {
            get
            {
                return stepProgressItem;
            }
            set
            {
                stepProgressItem = value;
            }
        }
        
        public ViewModel()
        {
            StepProgressItem = new ObservableCollection<StepProgressBarItem>();
            StepProgressItem.Add(new StepProgressBarItem() { PrimaryText = "Cart" });
            StepProgressItem.Add(new StepProgressBarItem() { PrimaryText = "Address" });
            StepProgressItem.Add(new StepProgressBarItem() { PrimaryText = "Delivery" });
            StepProgressItem.Add(new StepProgressBarItem() { PrimaryText = "Ordered" });
        }
    }
}
```

### Step 2: Bind to ItemsSource

**XAML with Binding:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:stepProgressBar="clr-namespace:Syncfusion.Maui.ProgressBar;assembly=Syncfusion.Maui.ProgressBar"
             xmlns:local="clr-namespace:YourNamespace"
             x:Class="YourNamespace.MainPage">
    
    <ContentPage.BindingContext>
        <local:ViewModel />
    </ContentPage.BindingContext>
    
    <stepProgressBar:SfStepProgressBar 
        VerticalOptions="Center"
        HorizontalOptions="Center"
        ItemsSource="{Binding StepProgressItem}" />
        
</ContentPage>
```

**C# with Direct Assignment:**
```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        ViewModel viewModel = new ViewModel();
        
        SfStepProgressBar stepProgressBar = new SfStepProgressBar()
        {
            VerticalOptions = LayoutOptions.Center,
            HorizontalOptions = LayoutOptions.Center,
            ItemsSource = viewModel.StepProgressItem
        };
        
        this.Content = stepProgressBar;
    }
}
```

Now your StepProgressBar displays 4 steps with labels: Cart, Address, Delivery, Ordered.

## Set Active Step

Use `ActiveStepIndex` and `ActiveStepProgressValue` to indicate current progress.

### ActiveStepIndex

The `ActiveStepIndex` property specifies which step is currently active (0-based index).

- **Index < 0**: First step marked as "Not Started"
- **Index = 0**: First step is "In Progress", others "Not Started"
- **Index = 2**: Steps 0-1 are "Completed", step 2 is "In Progress", step 3+ are "Not Started"
- **Index >= item count**: All steps marked as "Completed"

### ActiveStepProgressValue

The `ActiveStepProgressValue` property sets the progress percentage (0-100) within the active step. This shows partial completion of the current step.

**Example:**
```xml
<stepProgressBar:SfStepProgressBar 
    ItemsSource="{Binding StepProgressItem}"
    ActiveStepIndex="2"
    ActiveStepProgressValue="60" />
```

This configuration:
- Steps 0-1 (Cart, Address): Completed ✓
- Step 2 (Delivery): In Progress at 60%
- Step 3 (Ordered): Not Started

**C# Example:**
```csharp
SfStepProgressBar stepProgressBar = new SfStepProgressBar()
{
    ItemsSource = viewModel.StepProgressItem,
    ActiveStepIndex = 2,
    ActiveStepProgressValue = 60
};
```

## Complete Working Example

Here's a full working example with all components:

### MainPage.xaml
```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:stepProgressBar="clr-namespace:Syncfusion.Maui.ProgressBar;assembly=Syncfusion.Maui.ProgressBar"
             xmlns:local="clr-namespace:StepProgressBarDemo"
             x:Class="StepProgressBarDemo.MainPage">
    
    <ContentPage.BindingContext>
        <local:ViewModel />
    </ContentPage.BindingContext>
    
    <Grid Padding="20">
        <stepProgressBar:SfStepProgressBar 
            x:Name="stepProgress"
            VerticalOptions="Center"
            HorizontalOptions="Center"
            Orientation="Horizontal"
            LabelSpacing="12"
            ActiveStepIndex="2"
            ActiveStepProgressValue="60"
            ProgressAnimationDuration="2500"
            ItemsSource="{Binding StepProgressItem}" />
    </Grid>
    
</ContentPage>
```

### ViewModel.cs
```csharp
using System.Collections.ObjectModel;
using Syncfusion.Maui.ProgressBar;

namespace StepProgressBarDemo
{
    public class ViewModel
    {
        public ObservableCollection<StepProgressBarItem> StepProgressItem { get; set; }
        
        public ViewModel()
        {
            StepProgressItem = new ObservableCollection<StepProgressBarItem>
            {
                new StepProgressBarItem() { PrimaryText = "Cart" },
                new StepProgressBarItem() { PrimaryText = "Address" },
                new StepProgressBarItem() { PrimaryText = "Delivery" },
                new StepProgressBarItem() { PrimaryText = "Payment" },
                new StepProgressBarItem() { PrimaryText = "Ordered" }
            };
        }
    }
}
```

### MauiProgram.cs
```csharp
using Syncfusion.Maui.Core.Hosting;

namespace StepProgressBarDemo
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

### Result

This creates a horizontal StepProgressBar with 5 steps:
- **Cart**: Completed (green with checkmark)
- **Address**: Completed (green with checkmark)
- **Delivery**: In Progress at 60% (orange with progress indicator)
- **Payment**: Not Started (gray)
- **Ordered**: Not Started (gray)

The progress bar animates smoothly over 2.5 seconds.

## Troubleshooting

### Issue: Control Not Rendering

**Problem:** StepProgressBar appears empty or not visible.

**Solutions:**
1. **Check handler registration:** Ensure `ConfigureSyncfusionCore()` is called in MauiProgram.cs
2. **Verify ItemsSource:** Make sure ItemsSource is bound to a non-empty collection
3. **Check layout:** Add `VerticalOptions="Center"` and `HorizontalOptions="Center"` or set explicit HeightRequest/WidthRequest
4. **NuGet package:** Verify Syncfusion.Maui.ProgressBar is installed correctly

### Issue: Steps Not Showing Labels

**Problem:** Steps display but no text labels appear.

**Solutions:**
1. **Set PrimaryText:** Ensure each StepProgressBarItem has `PrimaryText` property set
2. **Check LabelSpacing:** Increase LabelSpacing if labels are too close
3. **Verify font:** Ensure font family is available if using custom fonts

### Issue: Progress Not Animating

**Problem:** Progress changes instantly without animation.

**Solutions:**
1. **Set ProgressAnimationDuration:** Default is 1000ms, increase for slower animation
2. **Check ActiveStepProgressValue:** Ensure value changes from 0-100 to see animation
3. **Platform limitation:** Some platforms may have reduced animations if battery saver is on

### Issue: All Steps Show as Completed

**Problem:** All steps appear green/completed regardless of ActiveStepIndex.

**Solutions:**
1. **Check ActiveStepIndex value:** If >= item count, all steps marked completed
2. **Verify item count:** Ensure ItemsSource has correct number of items
3. **Index bounds:** ActiveStepIndex should be 0 to (ItemsSource.Count - 1)

### Issue: Build Errors with Syncfusion

**Problem:** Compilation errors related to Syncfusion namespace.

**Solutions:**
1. **Restore packages:** Run `dotnet restore` in terminal
2. **Clean and rebuild:** Clean solution, delete bin/obj folders, rebuild
3. **Check package version:** Ensure all Syncfusion packages have same version number
4. **License:** Add Syncfusion license key in App.xaml.cs constructor (if using licensed version)

### Performance Tip

For optimal performance with many steps (10+):
- Use virtualization if supported in future versions
- Limit ProgressAnimationDuration to 1000-2000ms
- Avoid complex DataTemplates unless necessary
- Use simple ContentTypes (Tick/Dot/Cross) over images when possible