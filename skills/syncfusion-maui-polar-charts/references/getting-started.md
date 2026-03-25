# Getting Started with Syncfusion .NET MAUI Polar Charts

This guide walks through the installation, setup, and implementation of a basic Syncfusion .NET MAUI Polar Chart (SfPolarChart).

## Installation

### Step 1: Install NuGet Package

Install the Syncfusion .NET MAUI Charts package using one of these methods:

**Option A: Via Terminal**
```bash
dotnet add package Syncfusion.Maui.Charts
dotnet restore
```

**Option B: Via NuGet Package Manager**
1. Right-click project → **Manage NuGet Packages**
2. Search for `Syncfusion.Maui.Charts`
3. Click **Install**

**Option C: Via Package Manager Console**
```powershell
Install-Package Syncfusion.Maui.Charts
```

### Step 2: Register the Syncfusion Handler

**CRITICAL:** The `Syncfusion.Maui.Core` NuGet is a dependent package for all Syncfusion controls. You MUST register the handler in `MauiProgram.cs`.

**File:** `MauiProgram.cs`

```csharp
using Syncfusion.Maui.Core.Hosting;

    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            
            // Register Syncfusion Core - REQUIRED!
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
```
**⚠️ Important:** Call `ConfigureSyncfusionCore()` before `Build()`. Failure to register the handler will result in runtime errors.

## Creating Your First Polar Chart

### Step 1: Create Data Model

Define a data model to represent your chart data points:

```csharp
public class PlantModel
{
    public string Direction { get; set; }
    public double Tree { get; set; }
    public double Flower { get; set; }
    public double Weed { get; set; }
}
```

### Step 2: Create ViewModel

Create a ViewModel with sample data:

```csharp
using System.Collections.Generic;

public class PlantViewModel
{
    public List<PlantModel> PlantDetails { get; set; }

    public PlantViewModel()
    {
        PlantDetails = new List<PlantModel>
        {
            new PlantModel { Direction = "North", Tree = 80, Flower = 42, Weed = 63 },
            new PlantModel { Direction = "NorthEast", Tree = 85, Flower = 40, Weed = 70 },
            new PlantModel { Direction = "East", Tree = 78, Flower = 47, Weed = 65 },
            new PlantModel { Direction = "SouthEast", Tree = 90, Flower = 40, Weed = 70 },
            new PlantModel { Direction = "South", Tree = 78, Flower = 27, Weed = 47 },
            new PlantModel { Direction = "SouthWest", Tree = 83, Flower = 45, Weed = 65 },
            new PlantModel { Direction = "West", Tree = 79, Flower = 40, Weed = 58 },
            new PlantModel { Direction = "NorthWest", Tree = 88, Flower = 38, Weed = 73 }
        };
    }
}
```

### Step 3: Set BindingContext

Set the ViewModel as the page's BindingContext:

**XAML:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:chart="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts"
             xmlns:model="clr-namespace:YourNamespace"
             x:Class="YourNamespace.MainPage">

    <ContentPage.BindingContext>
        <model:PlantViewModel/>
    </ContentPage.BindingContext>
    
    <!-- Chart will go here -->
</ContentPage>
```

**C#:**
```csharp
public MainPage()
{
    InitializeComponent();
    this.BindingContext = new PlantViewModel();
}
```

### Step 4: Initialize Chart Axes

Polar charts require two axes:
- **PrimaryAxis**: X-axis (horizontal, typically Category or DateTime)
- **SecondaryAxis**: Y-axis (vertical, typically Numeric)

**XAML:**
```xml
<chart:SfPolarChart>
    <chart:SfPolarChart.PrimaryAxis>
        <chart:CategoryAxis/>
    </chart:SfPolarChart.PrimaryAxis>
    
    <chart:SfPolarChart.SecondaryAxis>
        <chart:NumericalAxis Maximum="100"/>
    </chart:SfPolarChart.SecondaryAxis>
</chart:SfPolarChart>
```

**C#:**
```csharp
SfPolarChart chart = new SfPolarChart();

// Configure primary axis (X-axis)
CategoryAxis primaryAxis = new CategoryAxis();
chart.PrimaryAxis = primaryAxis;

// Configure secondary axis (Y-axis)
NumericalAxis secondaryAxis = new NumericalAxis
{
    Maximum = 100
};
chart.SecondaryAxis = secondaryAxis;
```

### Step 5: Add Polar Series

Add a PolarLineSeries to display data:

**XAML:**
```xml
<chart:SfPolarChart>
    <chart:SfPolarChart.PrimaryAxis>
        <chart:CategoryAxis/>
    </chart:SfPolarChart.PrimaryAxis>
    
    <chart:SfPolarChart.SecondaryAxis>
        <chart:NumericalAxis Maximum="100"/>
    </chart:SfPolarChart.SecondaryAxis>
    
    <!-- Add series -->
    <chart:PolarLineSeries ItemsSource="{Binding PlantDetails}"
                           XBindingPath="Direction"
                           YBindingPath="Tree"
                           ShowMarkers="True"/>
</chart:SfPolarChart>
```

**C#:**
```csharp
SfPolarChart chart = new SfPolarChart();

// Configure axes
chart.PrimaryAxis = new CategoryAxis();
chart.SecondaryAxis = new NumericalAxis { Maximum = 100 };

// Create series
PolarLineSeries series = new PolarLineSeries
{
    ItemsSource = (new PlantViewModel()).PlantDetails,
    XBindingPath = "Direction",
    YBindingPath = "Tree",
    ShowMarkers = true
};

chart.Series.Add(series);
this.Content = chart;
```

## Complete Working Example

### XAML Implementation

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:chart="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts"
             xmlns:model="clr-namespace:ChartGettingStarted"
             x:Class="ChartGettingStarted.MainPage">

    <ContentPage.BindingContext>
        <model:PlantViewModel/>
    </ContentPage.BindingContext>

    <chart:SfPolarChart>
        <chart:SfPolarChart.PrimaryAxis>
            <chart:CategoryAxis/>
        </chart:SfPolarChart.PrimaryAxis>

        <chart:SfPolarChart.SecondaryAxis>
            <chart:NumericalAxis Maximum="100"/>
        </chart:SfPolarChart.SecondaryAxis>

        <!-- Multiple series for comparison -->
        <chart:PolarLineSeries ItemsSource="{Binding PlantDetails}" 
                               XBindingPath="Direction" 
                               YBindingPath="Tree"
                               Label="Tree"
                               ShowMarkers="True"/>
        
        <chart:PolarLineSeries ItemsSource="{Binding PlantDetails}" 
                               XBindingPath="Direction" 
                               YBindingPath="Weed"
                               Label="Weed"
                               ShowMarkers="True"/>

        <chart:PolarLineSeries ItemsSource="{Binding PlantDetails}" 
                               XBindingPath="Direction" 
                               YBindingPath="Flower"
                               Label="Flower"
                               ShowMarkers="True"/>
    </chart:SfPolarChart>
</ContentPage>
```

### C# Implementation

```csharp
using Syncfusion.Maui.Charts;

namespace ChartGettingStarted
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            // Create chart
            SfPolarChart chart = new SfPolarChart();
            
            // Configure axes
            chart.PrimaryAxis = new CategoryAxis();
            chart.SecondaryAxis = new NumericalAxis { Maximum = 100 };
            
            // Get data
            PlantViewModel viewModel = new PlantViewModel();
            
            // Add multiple series
            chart.Series.Add(new PolarLineSeries
            {
                ItemsSource = viewModel.PlantDetails,
                XBindingPath = "Direction",
                YBindingPath = "Tree",
                Label = "Tree",
                ShowMarkers = true
            });
            
            chart.Series.Add(new PolarLineSeries
            {
                ItemsSource = viewModel.PlantDetails,
                XBindingPath = "Direction",
                YBindingPath = "Weed",
                Label = "Weed",
                ShowMarkers = true
            });
            
            chart.Series.Add(new PolarLineSeries
            {
                ItemsSource = viewModel.PlantDetails,
                XBindingPath = "Direction",
                YBindingPath = "Flower",
                Label = "Flower",
                ShowMarkers = true
            });
            
            this.Content = chart;
        }
    }
}
```

## Key Properties Explained

### XBindingPath and YBindingPath

These properties map your data model properties to chart coordinates:

- **XBindingPath**: Property name for X-axis values (e.g., "Direction", "Category")
- **YBindingPath**: Property name for Y-axis values (e.g., "Tree", "Value")

**Example:**
```csharp
PolarLineSeries series = new PolarLineSeries
{
    ItemsSource = data,
    XBindingPath = "Direction",  // Maps to PlantModel.Direction
    YBindingPath = "Tree"        // Maps to PlantModel.Tree
};
```

### ItemsSource

The `ItemsSource` property binds the series to your data collection:

```csharp
// Bind to collection
series.ItemsSource = viewModel.PlantDetails;

// Or use data binding in XAML
ItemsSource="{Binding PlantDetails}"
```

## Common Issues and Solutions

### Issue: Chart Not Displaying

**Possible Causes:**
1. Missing `ConfigureSyncfusionCore()` in `MauiProgram.cs`
2. Incorrect namespace imports
3. BindingContext not set
4. ItemsSource is null or empty

**Solution:**
```csharp
// Verify handler registration
builder.ConfigureSyncfusionCore();

// Check namespace
using Syncfusion.Maui.Charts;

// Ensure BindingContext is set
this.BindingContext = new PlantViewModel();

// Verify data exists
Console.WriteLine($"Data count: {viewModel.PlantDetails.Count}");
```

### Issue: Data Binding Not Working

**Problem:** Series shows no data even though ItemsSource has values.

**Solution:** Verify property names match exactly (case-sensitive):
```csharp
// Data model
public class PlantModel
{
    public string Direction { get; set; }  // Property name
    public double Tree { get; set; }
}

// Series binding - must match exactly
XBindingPath = "Direction"  // ✓ Correct
XBindingPath = "direction"  // ✗ Wrong - case mismatch
```

### Issue: Axis Not Showing Proper Range

**Problem:** Y-axis range doesn't fit the data properly.

**Solution:** Set explicit Maximum and Minimum:
```csharp
NumericalAxis secondaryAxis = new NumericalAxis
{
    Minimum = 0,
    Maximum = 100,
    Interval = 20
};
```

## Next Steps

Now that you have a basic polar chart running:

1. **Explore Series Types** - Read [series-types.md](series-types.md) to learn about PolarLineSeries vs PolarAreaSeries
2. **Customize Axes** - Read [axis-configuration.md](axis-configuration.md) for axis customization options
3. **Add Interactivity** - Read [legend-tooltip.md](legend-tooltip.md) to add legends and tooltips
4. **Style Your Chart** - Read [appearance-customization.md](appearance-customization.md) for styling options

## Additional Resources

- **GitHub Sample**: [Creating a Getting Started application for .NET MAUI Polar Chart](https://github.com/SyncfusionExamples/Creating-a-Getting-Started-application-for-NET-MAUI-Polar-Chart)
- **API Documentation**: [SfPolarChart API Reference](https://help.syncfusion.com/cr/maui/Syncfusion.Maui.Charts.SfPolarChart.html)
- **Licensing**: [Syncfusion Licensing Guide](https://help.syncfusion.com/maui/licensing)
