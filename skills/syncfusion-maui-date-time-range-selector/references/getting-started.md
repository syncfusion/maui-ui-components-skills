# Getting Started with .NET MAUI DateTime Range Selector

This guide walks you through setting up and configuring a DateTime Range Selector (`SfDateTimeRangeSelector`) in your .NET MAUI application across different development environments.

## Step 1: Create a New MAUI Project

### Visual Studio 2026
1. Go to **File > New > Project**
2. Choose the **.NET MAUI App** template
3. Name the project and choose a location, click **Next**
4. Select the .NET framework version and click **Create**

### Visual Studio Code
1. Press **Ctrl+Shift+P** and type **.NET:New Project**
2. Choose **.NET MAUI App** template
3. Select project location and enter project name
4. Choose **Create project**

### JetBrains Rider
1. Go to **File > New Solution**
2. Select .NET (C#) and choose **.NET MAUI App** template
3. Enter Project Name, Solution Name, and Location
4. Select .NET framework version and click **Create**

## Step 2: Install the Syncfusion MAUI Sliders NuGet Package

### Visual Studio 2026
1. In **Solution Explorer**, right-click the project
2. Choose **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.Sliders`
4. Install the latest version
5. Ensure dependencies are installed and project is restored

### Visual Studio Code
1. Press **Ctrl+`** to open integrated terminal
2. Navigate to project root directory (where .csproj is)
3. Run: `dotnet add package Syncfusion.Maui.Sliders`
4. Run: `dotnet restore` to ensure all dependencies are installed

### JetBrains Rider
1. In **Solution Explorer**, right-click project
2. Choose **Manage NuGet Packages**
3. Search for `Syncfusion.Maui.Sliders`
4. Install the latest version
5. If needed, open Terminal and run: `dotnet restore`

## Step 3: Register the Handler

The `Syncfusion.Maui.Core` NuGet package is a required dependency for all Syncfusion .NET MAUI controls. Register the handler in **MauiProgram.cs**:

```csharp
using Microsoft.Maui;
using Microsoft.Maui.Hosting;
using Microsoft.Maui.Controls.Compatibility;
using Microsoft.Maui.Controls.Hosting;
using Microsoft.Maui.Controls.Xaml;
using Syncfusion.Maui.Core.Hosting;

namespace RangeSelector
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

> **Why this is needed:** Syncfusion MAUI controls use .NET MAUI Handlers for platform rendering. Without registering the Syncfusion core handler, components will not render at runtime.

## Step 4: Add a Basic DateTime Range Selector

### XAML Implementation

Import the namespace and initialize the DateTime Range Selector:

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sliders="clr-namespace:Syncfusion.Maui.Sliders;assembly=Syncfusion.Maui.Sliders"
             x:Class="YourNamespace.MainPage">
    
    <sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                     Maximum="2018-01-01" 
                                     RangeStart="2012-01-01" 
                                     RangeEnd="2016-01-01" />
</ContentPage>
```

### C# Implementation

```csharp
using Syncfusion.Maui.Sliders;

namespace GettingStarted
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            SfDateTimeRangeSelector rangeSelector = new SfDateTimeRangeSelector();
            rangeSelector.Minimum = new DateTime(2010, 01, 01);
            rangeSelector.Maximum = new DateTime(2018, 01, 01);
            rangeSelector.RangeStart = new DateTime(2012, 01, 01);
            rangeSelector.RangeEnd = new DateTime(2016, 01, 01);
            
            Content = rangeSelector;
        }
    }
}
```

## Adding Content to the DateTime Range Selector

The `Content` property allows you to add any control within the DateTime Range Selector. Typically, charts are added for data visualization and filtering.

### With Chart Content

```xaml
<ContentPage xmlns:sliders="clr-namespace:Syncfusion.Maui.Sliders;assembly=Syncfusion.Maui.Sliders"
             xmlns:charts="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts"
             xmlns:local="clr-namespace:YourNamespace">
    
    <ContentPage.BindingContext>
        <local:ViewModel />
    </ContentPage.BindingContext>

    <sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                     Maximum="2018-01-01" 
                                     RangeStart="2012-01-01" 
                                     RangeEnd="2016-01-01">
        
        <charts:SfCartesianChart>
            <charts:SfCartesianChart.XAxes>
                <charts:DateTimeAxis IsVisible="False"
                                     ShowMajorGridLines="False" />
            </charts:SfCartesianChart.XAxes>

            <charts:SfCartesianChart.YAxes>
                <charts:NumericalAxis IsVisible="False"
                                      ShowMajorGridLines="False" />
            </charts:SfCartesianChart.YAxes>

            <charts:SfCartesianChart.Series>
                <charts:SplineAreaSeries ItemsSource="{Binding ChartData}"
                                         XBindingPath="Date"
                                         YBindingPath="Value" />
            </charts:SfCartesianChart.Series>
        </charts:SfCartesianChart>
    
    </sliders:SfDateTimeRangeSelector>
</ContentPage>
```

### C# with Chart Content

```csharp
SfCartesianChart chart = new SfCartesianChart();

DateTimeAxis primaryAxis = new DateTimeAxis();
primaryAxis.IsVisible = false;
chart.XAxes.Add(primaryAxis);

NumericalAxis secondaryAxis = new NumericalAxis();
secondaryAxis.IsVisible = false;
chart.YAxes.Add(secondaryAxis);

SplineAreaSeries series = new SplineAreaSeries()
{
    ItemsSource = new ViewModel().ChartData,
    XBindingPath = "Date",
    YBindingPath = "Value",
};
chart.Series.Add(series);

SfDateTimeRangeSelector rangeSelector = new SfDateTimeRangeSelector()
{
    Minimum = new DateTime(2010, 01, 01),
    Maximum = new DateTime(2018, 01, 01),
    RangeStart = new DateTime(2012, 01, 01),
    RangeEnd = new DateTime(2016, 01, 01),
    Content = chart,
};

this.Content = rangeSelector;
```

> **Tip:** Hide the chart's axes (`IsVisible="False"`, `ShowMajorGridLines="False"`) so the chart appears as a clean background behind the selector.

## Enable Labels and Ticks

Display labels and tick marks at specified intervals:

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2018-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2016-01-01"
                                 Interval="2" 
                                 ShowLabels="True"
                                 ShowTicks="True">
    <charts:SfCartesianChart>
        <!-- Chart configuration -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

```csharp
rangeSelector.Interval = 2;
rangeSelector.ShowLabels = true;
rangeSelector.ShowTicks = true;
```

## Enable Minor Ticks

Add minor ticks between major ticks:

```xaml
<sliders:SfDateTimeRangeSelector Interval="2" 
                                 ShowTicks="True" 
                                 MinorTicksPerInterval="1">
    <!-- Content -->
</sliders:SfDateTimeRangeSelector>
```

```csharp
rangeSelector.ShowTicks = true;
rangeSelector.MinorTicksPerInterval = 1;
```

## Inverse the Range Selector

Reverse the direction using the `IsInversed` property:

```xaml
<sliders:SfDateTimeRangeSelector IsInversed="True" 
                                 ShowLabels="True" 
                                 ShowTicks="True" />
```

```csharp
rangeSelector.IsInversed = true;
```

## Format Labels

Customize date format for labels:

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01"
                                 Maximum="2011-01-01"
                                 RangeStart="2010-04-01"
                                 RangeEnd="2010-10-01"
                                 DateFormat="MMM"
                                 IntervalType="Months"
                                 Interval="2"
                                 ShowLabels="True">
    <!-- Content -->
</sliders:SfDateTimeRangeSelector>
```

```csharp
rangeSelector.DateFormat = "MMM";
rangeSelector.IntervalType = SliderDateIntervalType.Months;
rangeSelector.Interval = 2;
rangeSelector.ShowLabels = true;
```

## Core Properties Summary

| Property | Type | Description |
|----------|------|-------------|
| `Minimum` | DateTime | Minimum selectable date/time value |
| `Maximum` | DateTime | Maximum selectable date/time value |
| `RangeStart` | DateTime | Start value of selected range |
| `RangeEnd` | DateTime | End value of selected range |
| `Interval` | double | Spacing between labels/ticks (0 = auto) |
| `IntervalType` | SliderDateIntervalType | Years, Months, Days, Hours, Minutes, Seconds |
| `DateFormat` | string | Format pattern for date display |
| `ShowLabels` | bool | Display date labels on track |
| `ShowTicks` | bool | Display tick marks on track |
| `MinorTicksPerInterval` | int | Minor ticks between major ticks |
| `ShowDividers` | bool | Display divider lines |
| `IsInversed` | bool | Reverse track direction |
| `Content` | View | Embedded content (typically charts) |

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Component doesn't render | Ensure `ConfigureSyncfusionCore()` is called in `MauiProgram.cs` |
| NuGet restore fails | Run `dotnet restore` manually in terminal |
| `SfDateTimeRangeSelector` not found in XAML | Verify the `xmlns:sliders` namespace declaration |
| Chart content not visible | Check `ItemsSource` binding and ViewModel data |
| Thumbs not dragging | Ensure `Minimum`/`Maximum` are set with valid DateTime range |
| Labels show as numbers | Set `DateFormat` property to format DateTime values |

## Next Steps

- **Interval Configuration**: Read [interval-and-formatting.md](interval-and-formatting.md) to learn about interval types and date formatting
- **Label Customization**: Read [labels.md](labels.md) for label styling and placement options
- **Visual Elements**: Read [ticks-and-dividers.md](ticks-and-dividers.md) for tick and divider customization
- **Styling**: Explore track, thumb, tooltip, and region customization in respective reference files
