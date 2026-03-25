# Getting Started with .NET MAUI Range Slider

## Table of Contents
- [Overview](#overview)
- [Installation](#installation)
- [Handler Registration](#handler-registration)
- [Basic Implementation](#basic-implementation)
- [Adding Features](#adding-features)
- [Complete Example](#complete-example)

## Overview

The Syncfusion .NET MAUI Range Slider (`SfRangeSlider`) is a highly interactive UI control for selecting a range of values between minimum and maximum limits. This guide covers installation, setup, and basic implementation.

## Installation

### Step 1: Create a New .NET MAUI Project

**Visual Studio 2026:**
1. Go to **File > New > Project**
2. Choose **.NET MAUI App** template
3. Name the project and choose a location
4. Select the .NET framework version and click **Create**

**Visual Studio Code:**
1. Press **Ctrl+Shift+P** (Cmd+Shift+P on Mac)
2. Type **.NET:New Project** and press Enter
3. Choose **.NET MAUI App** template
4. Select project location and name
5. Choose **Create project**

**JetBrains Rider:**
1. Go to **File > New Solution**
2. Select **.NET (C#)** and choose **.NET MAUI App** template
3. Enter Project Name, Solution Name, and Location
4. Select .NET framework version and click **Create**

### Step 2: Install Syncfusion MAUI Sliders NuGet Package

**Visual Studio 2026:**
1. In **Solution Explorer**, right-click the project
2. Choose **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.Sliders`
4. Install the latest version

**Visual Studio Code:**
```bash
dotnet add package Syncfusion.Maui.Sliders
dotnet restore
```

**JetBrains Rider:**
1. In **Solution Explorer**, right-click the project
2. Choose **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.Sliders`
4. Install the latest version
5. If needed, open Terminal and run: `dotnet restore`

## Handler Registration

The `Syncfusion.Maui.Core` NuGet package is a dependency for all Syncfusion .NET MAUI controls. Register the handler in **MauiProgram.cs**:

```csharp
using Microsoft.Maui;
using Microsoft.Maui.Hosting;
using Microsoft.Maui.Controls.Hosting;
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

            return builder.Build();
        }
    }
}
```

**Key point**: The `.ConfigureSyncfusionCore()` call is required for all Syncfusion MAUI controls.

## Basic Implementation

### Step 1: Add Namespace

**XAML:**
```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sliders="clr-namespace:Syncfusion.Maui.Sliders;assembly=Syncfusion.Maui.Sliders"
             x:Class="YourApp.MainPage">
    <!-- Content here -->
</ContentPage>
```

**C#:**
```csharp
using Syncfusion.Maui.Sliders;
```

### Step 2: Initialize Range Slider

**XAML:**
```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sliders="clr-namespace:Syncfusion.Maui.Sliders;assembly=Syncfusion.Maui.Sliders"
             x:Class="YourApp.MainPage">
    
    <sliders:SfRangeSlider />
    
</ContentPage>
```

**C#:**
```csharp
using Syncfusion.Maui.Sliders;

namespace YourApp
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            SfRangeSlider rangeSlider = new SfRangeSlider();
            this.Content = rangeSlider;
        }
    }
}
```

**Result**: A basic range slider with default values (Minimum: 0, Maximum: 1, RangeStart: 0, RangeEnd: 1).

## Adding Features

### Enable Labels

Display value labels at intervals:

**XAML:**
```xaml
<sliders:SfRangeSlider Minimum="0"
                       Maximum="10"
                       RangeStart="2"
                       RangeEnd="8"
                       Interval="2"
                       ShowLabels="True" />
```

**C#:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    Minimum = 0,
    Maximum = 10,
    RangeStart = 2,
    RangeEnd = 8,
    Interval = 2,
    ShowLabels = true
};
```

### Enable Ticks

Add tick marks at intervals:

**XAML:**
```xaml
<sliders:SfRangeSlider Minimum="0"
                       Maximum="10"
                       RangeStart="2"
                       RangeEnd="8"
                       Interval="2"
                       ShowLabels="True"
                       ShowTicks="True"
                       MinorTicksPerInterval="1" />
```

**C#:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    Minimum = 0,
    Maximum = 10,
    RangeStart = 2,
    RangeEnd = 8,
    Interval = 2,
    ShowLabels = true,
    ShowTicks = true,
    MinorTicksPerInterval = 1  // Add minor ticks between major ticks
};
```

### Set Orientation

Change slider to vertical orientation:

**XAML:**
```xaml
<sliders:SfRangeSlider Minimum="0"
                       Maximum="10"
                       RangeStart="2"
                       RangeEnd="8"
                       Interval="2"
                       ShowLabels="True"
                       ShowTicks="True"
                       Orientation="Vertical"
                       HeightRequest="300" />
```

**C#:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    Orientation = SliderOrientation.Vertical,
    HeightRequest = 300,
    Minimum = 0,
    Maximum = 10,
    RangeStart = 2,
    RangeEnd = 8,
    Interval = 2,
    ShowLabels = true,
    ShowTicks = true
};
```

### Inverse the Slider

Reverse the slider direction:

**XAML:**
```xaml
<sliders:SfRangeSlider Minimum="0"
                       Maximum="10"
                       RangeStart="2"
                       RangeEnd="8"
                       Interval="2"
                       ShowLabels="True"
                       ShowTicks="True"
                       IsInversed="True" />
```

**C#:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    Minimum = 0,
    Maximum = 10,
    RangeStart = 2,
    RangeEnd = 8,
    Interval = 2,
    ShowLabels = true,
    ShowTicks = true,
    IsInversed = true
};
```

### Format Labels

Add prefix or suffix to labels using NumberFormat:

**XAML:**
```xaml
<sliders:SfRangeSlider Minimum="20"
                       Maximum="100"
                       RangeStart="40"
                       RangeEnd="80"
                       Interval="20"
                       ShowLabels="True"
                       ShowTicks="True"
                       NumberFormat="$#" />
```

**C#:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    Minimum = 20,
    Maximum = 100,
    RangeStart = 40,
    RangeEnd = 80,
    Interval = 20,
    ShowLabels = true,
    ShowTicks = true,
    NumberFormat = "$#"  // Displays as $20, $40, etc.
};
```

## Complete Example

Here's a fully configured range slider with common features:

**XAML:**
```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sliders="clr-namespace:Syncfusion.Maui.Sliders;assembly=Syncfusion.Maui.Sliders"
             x:Class="YourApp.MainPage">
    
    <VerticalStackLayout Padding="20" Spacing="20">
        
        <Label Text="Price Range Filter"
               FontSize="18"
               FontAttributes="Bold" />
        
        <sliders:SfRangeSlider Minimum="0"
                               Maximum="1000"
                               RangeStart="200"
                               RangeEnd="800"
                               Interval="200"
                               ShowLabels="True"
                               ShowTicks="True"
                               ShowDividers="True"
                               MinorTicksPerInterval="1"
                               NumberFormat="$#">
            
            <sliders:SfRangeSlider.Tooltip>
                <sliders:SliderTooltip NumberFormat="$#" />
            </sliders:SfRangeSlider.Tooltip>
            
        </sliders:SfRangeSlider>
        
        <Label x:Name="ResultLabel"
               Text="Range: $200 - $800"
               HorizontalOptions="Center" />
        
    </VerticalStackLayout>
    
</ContentPage>
```

**C# Code-behind with Event Handling:**
```csharp
using Syncfusion.Maui.Sliders;

namespace YourApp
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
        }
    }
}
```

**C# (Programmatic Creation):**
```csharp
using Syncfusion.Maui.Sliders;

namespace YourApp
{
    public partial class MainPage : ContentPage
    {
        private Label resultLabel;
        
        public MainPage()
        {
            InitializeComponent();
            
            var layout = new VerticalStackLayout
            {
                Padding = new Thickness(20),
                Spacing = 20
            };
            
            layout.Children.Add(new Label
            {
                Text = "Price Range Filter",
                FontSize = 18,
                FontAttributes = FontAttributes.Bold
            });
            
            var rangeSlider = new SfRangeSlider
            {
                Minimum = 0,
                Maximum = 1000,
                RangeStart = 200,
                RangeEnd = 800,
                Interval = 200,
                ShowLabels = true,
                ShowTicks = true,
                ShowDividers = true,
                MinorTicksPerInterval = 1,
                NumberFormat = "$#",
                Tooltip = new SliderTooltip { NumberFormat = "$#" }
            };
            
            rangeSlider.ValueChanged += OnRangeSliderValueChanged;
            layout.Children.Add(rangeSlider);
            
            resultLabel = new Label
            {
                Text = "Range: $200 - $800",
                HorizontalOptions = LayoutOptions.Center
            };
            layout.Children.Add(resultLabel);
            
            this.Content = layout;
        }
        
        private void OnRangeSliderValueChanged(object sender, RangeSliderValueChangedEventArgs e)
        {
            resultLabel.Text = $"Range: ${e.NewValue.Start:F0} - ${e.NewValue.End:F0}";
        }
    }
}
```

## Next Steps

Now that you have a basic range slider working:

1. **Customize appearance**: See [track.md](track.md), [thumbs-and-overlays.md](thumbs-and-overlays.md)
2. **Add labels and ticks**: See [labels.md](labels.md), [ticks.md](ticks.md)
3. **Configure selection**: See [intervals-and-selection.md](intervals-and-selection.md)
4. **Handle events**: See [events-and-commands.md](events-and-commands.md)
5. **Add tooltips**: See [tooltips.md](tooltips.md)

## Troubleshooting

**Issue**: Range slider not appearing
- Verify `ConfigureSyncfusionCore()` is called in MauiProgram.cs
- Check namespace is correctly imported
- Ensure NuGet package is installed and restored

**Issue**: Labels/ticks not showing
- Set `ShowLabels="True"` and/or `ShowTicks="True"`
- Set `Interval` property (or leave at 0 for auto-calculation)

**Issue**: Thumbs not draggable
- Ensure `IsEnabled="True"` (default)
- Check that RangeStart and RangeEnd are within Minimum and Maximum bounds

## Resources

- [Official Documentation](https://help.syncfusion.com/maui/range-slider/overview)
- [API Reference](https://help.syncfusion.com/cr/maui/Syncfusion.Maui.Sliders.SfRangeSlider.html)
- [Sample Browser](https://github.com/syncfusion/maui-demos)
