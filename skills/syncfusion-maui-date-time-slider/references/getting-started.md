# Getting Started with .NET MAUI DateTime Slider

This guide covers installation, basic setup, and initial configuration of the Syncfusion .NET MAUI DateTime Slider (SfDateTimeSlider) control.

## Installation

### Step 1: Install Syncfusion.Maui.Sliders NuGet Package

**Visual Studio / JetBrains Rider:**
1. Right-click the project in Solution Explorer
2. Select **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.Sliders`
4. Install the latest version
5. Ensure all dependencies are installed and the project is restored

**Visual Studio Code / Terminal:**
```bash
dotnet add package Syncfusion.Maui.Sliders
dotnet restore
```

### Step 2: Register the Handler

The `Syncfusion.Maui.Core` NuGet package is a dependency for all Syncfusion .NET MAUI controls. You must register the handler for Syncfusion Core in `MauiProgram.cs`.

**MauiProgram.cs:**

```csharp
using Microsoft.Maui;
using Microsoft.Maui.Hosting;
using Microsoft.Maui.Controls.Hosting;
using Syncfusion.Maui.Core.Hosting;

namespace MyApp
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .ConfigureSyncfusionCore()  // ⚠️ REQUIRED
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                });

            return builder.Build();
        }
    }
}
```

**⚠️ Critical:** Without `ConfigureSyncfusionCore()`, the DateTime Slider will not render correctly.

## Basic Implementation

### Minimal DateTime Slider (XAML)

**MainPage.xaml:**

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sliders="clr-namespace:Syncfusion.Maui.Sliders;assembly=Syncfusion.Maui.Sliders"
             x:Class="MyApp.MainPage">
    <Grid>
        <sliders:SfDateTimeSlider Minimum="2010-01-01"
                                  Maximum="2018-01-01"
                                  Value="2014-01-01" />
    </Grid>
</ContentPage>
```

### Minimal DateTime Slider (C#)

**MainPage.xaml.cs:**

```csharp
using Syncfusion.Maui.Sliders;

namespace MyApp
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            var slider = new SfDateTimeSlider
            {
                Minimum = new DateTime(2010, 01, 01),
                Maximum = new DateTime(2018, 01, 01),
                Value = new DateTime(2014, 01, 01)
            };
            
            Content = new Grid { Children = { slider } };
        }
    }
}
```

## Core Properties

### Minimum, Maximum, and Value

These three properties define the DateTime range and current selection:

```csharp
var slider = new SfDateTimeSlider
{
    Minimum = new DateTime(2010, 01, 01),  // Start date
    Maximum = new DateTime(2025, 12, 31),  // End date
    Value = new DateTime(2020, 06, 15)     // Selected date
};
```

**Rules:**
- `Minimum` must be less than `Maximum`
- `Value` must be between `Minimum` and `Maximum`
- Default values are `null` (must be set)

## Adding Labels and Ticks

### Enable Labels

Display date labels at intervals:

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2018-01-01"
                          Value="2014-01-01"
                          ShowLabels="True"
                          Interval="2" />
```

**Result:** Labels appear at 2-year intervals: 2010, 2012, 2014, 2016, 2018.

### Enable Ticks

Add visual tick marks:

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2018-01-01"
                          Value="2014-01-01"
                          ShowLabels="True"
                          ShowTicks="True"
                          Interval="2"
                          MinorTicksPerInterval="1" />
```

**Components:**
- **ShowTicks**: Displays major ticks at each interval
- **MinorTicksPerInterval**: Adds smaller ticks between major ticks

## Orientation

### Horizontal (Default)

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2018-01-01"
                          Value="2014-01-01"
                          Orientation="Horizontal" />
```

### Vertical

```xaml
<sliders:SfDateTimeSlider Orientation="Vertical"
                          Minimum="2010-01-01"
                          Maximum="2018-01-01"
                          Value="2014-01-01"
                          ShowLabels="True"
                          ShowTicks="True"
                          Interval="2"
                          MinorTicksPerInterval="1" />
```

**C# Implementation:**

```csharp
slider.Orientation = SliderOrientation.Vertical;
```

## Inverse Direction

Reverse the slider direction to show maximum on the left (or top for vertical):

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2018-01-01"
                          Value="2014-01-01"
                          IsInversed="True"
                          ShowLabels="True"
                          ShowTicks="True"
                          Interval="2"
                          MinorTicksPerInterval="1" />
```

**Result:** Slider range flows from 2018 → 2010 instead of 2010 → 2018.

## Date Formatting

Customize how dates are displayed in labels:

```xaml
<sliders:SfDateTimeSlider Minimum="2010-01-01"
                          Maximum="2011-01-01"
                          Value="2010-07-01"
                          DateFormat="MMM"
                          IntervalType="Months"
                          ShowTicks="True"
                          MinorTicksPerInterval="1"
                          ShowLabels="True"
                          Interval="2" />
```

**Result:** Labels display as "Jan", "Mar", "May", "Jul", etc.

**Common DateFormat patterns:**
- `"yyyy"` → 2023
- `"MMM yyyy"` → Jan 2023
- `"MM/dd/yyyy"` → 01/15/2023
- `"h:mm tt"` → 3:45 PM
- `"HH:mm"` → 15:45

## Complete Example with All Features

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sliders="clr-namespace:Syncfusion.Maui.Sliders;assembly=Syncfusion.Maui.Sliders"
             x:Class="MyApp.MainPage">
    <Grid Padding="20">
        <sliders:SfDateTimeSlider Minimum="2020-01-01"
                                  Maximum="2025-12-31"
                                  Value="2023-06-15"
                                  Interval="1"
                                  IntervalType="Years"
                                  DateFormat="yyyy"
                                  ShowLabels="True"
                                  ShowTicks="True"
                                  MinorTicksPerInterval="3"
                                  Orientation="Horizontal"
                                  IsInversed="False" />
    </Grid>
</ContentPage>
```

**C# Equivalent:**

```csharp
using Syncfusion.Maui.Sliders;

public class MainPage : ContentPage
{
    public MainPage()
    {
        var slider = new SfDateTimeSlider
        {
            Minimum = new DateTime(2020, 01, 01),
            Maximum = new DateTime(2025, 12, 31),
            Value = new DateTime(2023, 06, 15),
            Interval = 1,
            IntervalType = SliderDateIntervalType.Years,
            DateFormat = "yyyy",
            ShowLabels = true,
            ShowTicks = true,
            MinorTicksPerInterval = 3,
            Orientation = SliderOrientation.Horizontal,
            IsInversed = false
        };
        
        Content = new Grid
        {
            Padding = new Thickness(20),
            Children = { slider }
        };
    }
}
```

## Troubleshooting

### Slider Not Visible
- Verify `ConfigureSyncfusionCore()` is called in MauiProgram.cs
- Ensure Minimum < Maximum
- Check that Value is between Minimum and Maximum

### Labels Not Showing
- Set `ShowLabels="True"`
- Set `Interval` to a non-zero value
- Verify `DateFormat` is valid

### Ticks Not Visible
- Set `ShowTicks="True"`
- Set `Interval` to a non-zero value

## Next Steps

- **Track Customization**: Learn about track colors, sizes, and styles
- **Labels**: Advanced label formatting, placement, and styling
- **Ticks & Dividers**: Customize tick appearance and add dividers
- **Tooltip**: Display tooltips during interaction
- **Events**: Handle value changes and implement MVVM patterns
