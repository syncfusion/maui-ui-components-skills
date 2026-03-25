# Getting Started with .NET MAUI DateTime Range Slider

This guide covers installation, setup, and basic implementation of the Syncfusion .NET MAUI DateTime Range Slider (`SfDateTimeRangeSlider`) control.

## Table of Contents
- [Overview](#overview)
- [Step 1: Install NuGet Package](#step-1-install-nuget-package)
- [Step 2: Register Handler](#step-2-register-handler)
- [Step 3: Add Namespace](#step-3-add-namespace)
- [Step 4: Create Basic Range Slider](#step-4-create-basic-range-slider)
- [Enable Labels](#enable-labels)
- [Enable Ticks](#enable-ticks)
- [Orientation](#orientation)
- [Inverse Direction](#inverse-direction)
- [Label Formatting](#label-formatting)
- [Troubleshooting](#troubleshooting)

## Overview

The `SfDateTimeRangeSlider` is a highly interactive UI control that allows users to select a range of DateTime values within a minimum and maximum limit using dual thumbs.

**Key Features:**
- DateTime support with flexible intervals
- Horizontal and vertical orientations
- Customizable labels, ticks, and dividers
- Interactive tooltips
- Discrete selection mode
- Rich styling options

## Step 1: Install NuGet Package

### Visual Studio

1. Right-click your project in Solution Explorer
2. Select **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.Sliders`
4. Install the latest version
5. Ensure dependencies are installed correctly

### Command Line

```bash
dotnet add package Syncfusion.Maui.Sliders
```

### VS Code

```bash
dotnet add package Syncfusion.Maui.Sliders
dotnet restore
```

## Step 2: Register Handler

The `Syncfusion.Maui.Core` NuGet package is a dependency for all Syncfusion .NET MAUI controls. You **must** register the handler in your `MauiProgram.cs` file.

**MauiProgram.cs:**

```csharp
using Microsoft.Maui;
using Microsoft.Maui.Hosting;
using Microsoft.Maui.Controls.Compatibility;
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
                .ConfigureSyncfusionCore()  // ← Register Syncfusion handlers
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                });

            return builder.Build();
        }
    }
}
```

**⚠️ Important:** Without `ConfigureSyncfusionCore()`, the control will not render properly.

## Step 3: Add Namespace

### XAML

Add the namespace declaration in your XAML file:

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sliders="clr-namespace:Syncfusion.Maui.Sliders;assembly=Syncfusion.Maui.Sliders"
             x:Class="YourApp.MainPage">
```

### C#

Add the using statement:

```csharp
using Syncfusion.Maui.Sliders;
```

## Step 4: Create Basic Range Slider

### XAML Example

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sliders="clr-namespace:Syncfusion.Maui.Sliders;assembly=Syncfusion.Maui.Sliders"
             x:Class="YourApp.MainPage">

    <sliders:SfDateTimeRangeSlider 
        Minimum="2010-01-01" 
        Maximum="2018-01-01" 
        RangeStart="2012-01-01" 
        RangeEnd="2016-01-01" />

</ContentPage>
```

### C# Example

```csharp
using Syncfusion.Maui.Sliders;

namespace YourApp
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
            rangeSlider.Minimum = new DateTime(2010, 01, 01);
            rangeSlider.Maximum = new DateTime(2018, 01, 01);
            rangeSlider.RangeStart = new DateTime(2012, 01, 01);
            rangeSlider.RangeEnd = new DateTime(2016, 01, 01);
            
            Content = rangeSlider;
        }
    }
}
```

**Result:** A basic horizontal range slider with dual thumbs for selecting a DateTime range.

## Enable Labels

The `ShowLabels` property renders labels at given intervals.

### XAML

```xaml
<sliders:SfDateTimeRangeSlider 
    Minimum="2010-01-01" 
    Maximum="2018-01-01" 
    RangeStart="2012-01-01" 
    RangeEnd="2016-01-01"
    Interval="2" 
    ShowLabels="True" />
```

### C#

```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2018, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2016, 01, 01);
rangeSlider.Interval = 2;
rangeSlider.ShowLabels = true;
```

**Note:** Set `Interval` to control the spacing between labels.

## Enable Ticks

The `ShowTicks` property renders major ticks. Use `MinorTicksPerInterval` to add minor ticks.

### XAML

```xaml
<sliders:SfDateTimeRangeSlider 
    Minimum="2010-01-01" 
    Maximum="2018-01-01" 
    RangeStart="2012-01-01" 
    RangeEnd="2016-01-01"
    Interval="2" 
    ShowLabels="True"
    ShowTicks="True"
    MinorTicksPerInterval="1" />
```

### C#

```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2018, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2016, 01, 01);
rangeSlider.Interval = 2;
rangeSlider.ShowLabels = true;
rangeSlider.ShowTicks = true;
rangeSlider.MinorTicksPerInterval = 1;
```

## Orientation

The `Orientation` property allows horizontal or vertical display.

### Vertical Orientation

```xaml
<sliders:SfDateTimeRangeSlider 
    Minimum="2010-01-01" 
    Maximum="2018-01-01" 
    RangeStart="2012-01-01" 
    RangeEnd="2016-01-01"
    ShowTicks="True" 
    ShowLabels="True"
    Interval="2" 
    MinorTicksPerInterval="1" 
    Orientation="Vertical" />
```

```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Orientation = SliderOrientation.Vertical;
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2018, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2016, 01, 01);
rangeSlider.ShowLabels = true;
rangeSlider.ShowTicks = true;
rangeSlider.Interval = 2;
rangeSlider.MinorTicksPerInterval = 1;
```

## Inverse Direction

The `IsInversed` property inverts the slider direction.

### XAML

```xaml
<sliders:SfDateTimeRangeSlider 
    Minimum="2010-01-01" 
    Maximum="2018-01-01" 
    RangeStart="2012-01-01" 
    RangeEnd="2016-01-01"
    Interval="2" 
    ShowTicks="True"
    ShowLabels="True"  
    MinorTicksPerInterval="1" 
    IsInversed="True" />
```

### C#

```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2018, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2016, 01, 01);
rangeSlider.Interval = 2;
rangeSlider.ShowLabels = true;
rangeSlider.ShowTicks = true;
rangeSlider.MinorTicksPerInterval = 1;
rangeSlider.IsInversed = true;
```

## Label Formatting

Use `DateFormat` to customize how DateTime labels appear.

### XAML

```xaml
<sliders:SfDateTimeRangeSlider 
    Minimum="2010-01-01"
    Maximum="2011-01-01"
    RangeStart="2010-04-01"
    RangeEnd="2010-10-01"
    DateFormat="MMM"
    IntervalType="Months"
    ShowTicks="True"
    MinorTicksPerInterval="1"
    ShowLabels="True"
    Interval="2" />
```

### C#

```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2011, 01, 01);
rangeSlider.RangeStart = new DateTime(2010, 04, 01);
rangeSlider.RangeEnd = new DateTime(2010, 10, 01);
rangeSlider.ShowLabels = true;
rangeSlider.Interval = 2;
rangeSlider.ShowTicks = true;
rangeSlider.MinorTicksPerInterval = 1;
rangeSlider.DateFormat = "MMM";
rangeSlider.IntervalType = SliderDateIntervalType.Months;
```

**Common DateFormat strings:**
- `"yyyy"` - Year (2024)
- `"MMM"` - Month abbreviation (Jan, Feb)
- `"MMM yyyy"` - Month and year (Jan 2024)
- `"h tt"` - Hour with AM/PM (9 AM)
- `"hh:mm"` - Hour:minute (09:30)

## Troubleshooting

### Issue: Control Not Displaying

**Symptoms:** Range slider doesn't appear on screen

**Solutions:**
1. ✅ Verify `Syncfusion.Maui.Sliders` NuGet package is installed
2. ✅ Ensure `ConfigureSyncfusionCore()` is called in `MauiProgram.cs`
3. ✅ Check namespace is correctly added: `xmlns:sliders="clr-namespace:Syncfusion.Maui.Sliders;assembly=Syncfusion.Maui.Sliders"`
4. ✅ Verify Minimum < Maximum
5. ✅ Check that RangeStart and RangeEnd are within Minimum-Maximum range

### Issue: Labels Not Showing

**Symptoms:** Labels don't appear even with `ShowLabels="True"`

**Solutions:**
1. ✅ Set `Interval` to a value > 0
2. ✅ Verify `DateFormat` is valid
3. ✅ Check that `IntervalType` matches your date range
4. ✅ Ensure label color isn't the same as background

### Issue: Build Errors

**Symptoms:** Compilation fails with missing assembly errors

**Solutions:**
1. ✅ Clean and rebuild the solution
2. ✅ Delete `bin/` and `obj/` folders
3. ✅ Run `dotnet restore`
4. ✅ Verify all Syncfusion dependencies are installed
5. ✅ Check for version conflicts in NuGet packages

### Issue: Runtime Exceptions

**Symptoms:** App crashes when displaying range slider

**Solutions:**
1. ✅ Ensure handler registration is before `UseMauiApp<App>()`
2. ✅ Verify DateTime values are valid (not DateTime.Min or DateTime.Max)
3. ✅ Check that RangeStart <= RangeEnd
4. ✅ Verify Interval is appropriate for the Minimum-Maximum range

## Next Steps

- **Track Configuration:** Read [track-and-range.md](track-and-range.md) for advanced track customization
- **Label Styling:** See [interval-and-labels.md](interval-and-labels.md) for label formatting and styling
- **Ticks and Dividers:** Check [ticks-and-dividers.md](ticks-and-dividers.md) for tick customization
- **Tooltips:** Review [tooltip-and-thumb.md](tooltip-and-thumb.md) for tooltip configuration
- **Events:** Explore [events-and-commands.md](events-and-commands.md) for handling value changes

## Summary

You now have a working DateTime Range Slider implementation! The basic setup requires:

1. ✅ Install `Syncfusion.Maui.Sliders` NuGet package
2. ✅ Call `ConfigureSyncfusionCore()` in `MauiProgram.cs`
3. ✅ Add namespace to XAML or using statement to C#
4. ✅ Set `Minimum`, `Maximum`, `RangeStart`, and `RangeEnd`
5. ✅ Optionally enable labels, ticks, and tooltips

From here, you can customize the appearance, handle events, and configure advanced features as needed.
