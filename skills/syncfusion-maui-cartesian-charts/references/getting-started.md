# Getting Started with .NET MAUI Cartesian Chart

This guide walks through the essential steps to install, configure, and create your first Syncfusion .NET MAUI Cartesian Chart (SfCartesianChart).

## Installation

### Step 1: Install NuGet Package

1. In **Solution Explorer**, right-click your project and choose **Manage NuGet Packages**
2. Search for `Syncfusion.Maui.Charts`
3. Install the latest version
4. Ensure all dependencies are installed correctly

**Package:** `Syncfusion.Maui.Charts`

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

## Basic Chart Initialization

### Step 1: Add Namespace

Import the Charts namespace in your XAML or C# file:

**XAML:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:chart="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts">
```

**C#:**
```csharp
using Syncfusion.Maui.Charts;
```

### Step 2: Initialize SfCartesianChart

**XAML:**
```xml
<chart:SfCartesianChart/>
```

**C#:**
```csharp
SfCartesianChart chart = new SfCartesianChart();
this.Content = chart;
```

## Creating Data Model and ViewModel

### Step 1: Define Data Model

Create a model class to represent your data points:

```csharp
public class PersonModel
{
    public string Name { get; set; }
    public double Height { get; set; }
}
```

### Step 2: Create ViewModel

Create a ViewModel with observable data:

```csharp
using System.Collections.ObjectModel;

public class PersonViewModel
{
    public ObservableCollection<PersonModel> Data { get; set; }

    public PersonViewModel()
    {
        Data = new ObservableCollection<PersonModel>
        {
            new PersonModel { Name = "David", Height = 170 },
            new PersonModel { Name = "Michael", Height = 96 },
            new PersonModel { Name = "Steve", Height = 65 },
            new PersonModel { Name = "Joel", Height = 182 },
            new PersonModel { Name = "Bob", Height = 134 }
        };
    }
}
```

### Step 3: Set BindingContext

**XAML:**
```xml
<ContentPage xmlns:model="clr-namespace:YourNamespace">
    <ContentPage.BindingContext>
        <model:PersonViewModel/>
    </ContentPage.BindingContext>
</ContentPage>
```

**C#:**
```csharp
this.BindingContext = new PersonViewModel();
```

## Setting Up Axes

Cartesian charts require both X and Y axes. Use the `XAxes` and `YAxes` collections to add axes.

### Common Axis Types

- **CategoryAxis**: For categorical data (names, months, etc.)
- **NumericalAxis**: For numeric values
- **DateTimeAxis**: For date/time data
- **LogarithmicAxis**: For logarithmic scale

### Basic Axis Setup

**XAML:**
```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis>
            <chart:CategoryAxis.Title>
                <chart:ChartAxisTitle Text="Name"/>
            </chart:CategoryAxis.Title>
        </chart:CategoryAxis>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis>
            <chart:NumericalAxis.Title>
                <chart:ChartAxisTitle Text="Height (cm)"/>
            </chart:NumericalAxis.Title>
        </chart:NumericalAxis>
    </chart:SfCartesianChart.YAxes>
</chart:SfCartesianChart>
```

**C#:**
```csharp
SfCartesianChart chart = new SfCartesianChart();

// X Axis
CategoryAxis primaryAxis = new CategoryAxis();
primaryAxis.Title = new ChartAxisTitle
{
    Text = "Name"
};
chart.XAxes.Add(primaryAxis);

// Y Axis
NumericalAxis secondaryAxis = new NumericalAxis();
secondaryAxis.Title = new ChartAxisTitle
{
    Text = "Height (cm)"
};
chart.YAxes.Add(secondaryAxis);
```

## Adding Your First Series

Series define the chart type and bind to your data. Use `XBindingPath` and `YBindingPath` to map data properties.

### ColumnSeries Example

**XAML:**
```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:ColumnSeries ItemsSource="{Binding Data}"
                       XBindingPath="Name"
                       YBindingPath="Height"
                       ShowDataLabels="True"
                       EnableTooltip="True"/>
</chart:SfCartesianChart>
```

**C#:**
```csharp
ColumnSeries series = new ColumnSeries
{
    ItemsSource = (new PersonViewModel()).Data,
    XBindingPath = "Name",
    YBindingPath = "Height",
    ShowDataLabels = true,
    EnableTooltip = true
};

chart.Series.Add(series);
```

## Complete Working Example

### XAML Implementation

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:chart="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts"
             xmlns:model="clr-namespace:YourNamespace">

    <ContentPage.BindingContext>
        <model:PersonViewModel/>
    </ContentPage.BindingContext>

    <chart:SfCartesianChart>
        <chart:SfCartesianChart.Title>
            <Label Text="Height Comparison" HorizontalOptions="Fill" HorizontalTextAlignment="Center"/>
        </chart:SfCartesianChart.Title>

        <chart:SfCartesianChart.Legend>
            <chart:ChartLegend/>
        </chart:SfCartesianChart.Legend>

        <chart:SfCartesianChart.XAxes>
            <chart:CategoryAxis>
                <chart:CategoryAxis.Title>
                    <chart:ChartAxisTitle Text="Name"/>
                </chart:CategoryAxis.Title>
            </chart:CategoryAxis>
        </chart:SfCartesianChart.XAxes>

        <chart:SfCartesianChart.YAxes>
            <chart:NumericalAxis>
                <chart:NumericalAxis.Title>
                    <chart:ChartAxisTitle Text="Height (cm)"/>
                </chart:NumericalAxis.Title>
            </chart:NumericalAxis>
        </chart:SfCartesianChart.YAxes>

        <chart:ColumnSeries ItemsSource="{Binding Data}"
                           XBindingPath="Name"
                           YBindingPath="Height"
                           EnableTooltip="True"
                           ShowDataLabels="True"
                           Label="Height">
            <chart:ColumnSeries.DataLabelSettings>
                <chart:CartesianDataLabelSettings LabelPlacement="Inner"/>
            </chart:ColumnSeries.DataLabelSettings>
        </chart:ColumnSeries>
    </chart:SfCartesianChart>
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
            
            this.BindingContext = new PersonViewModel();
            
            SfCartesianChart chart = new SfCartesianChart();
            
            // Title
            chart.Title = new Label
            {
                Text = "Height Comparison",
                HorizontalOptions = LayoutOptions.Fill,
                HorizontalTextAlignment = TextAlignment.Center
            };
            
            // Legend
            chart.Legend = new ChartLegend();
            
            // X Axis
            CategoryAxis primaryAxis = new CategoryAxis();
            primaryAxis.Title = new ChartAxisTitle { Text = "Name" };
            chart.XAxes.Add(primaryAxis);
            
            // Y Axis
            NumericalAxis secondaryAxis = new NumericalAxis();
            secondaryAxis.Title = new ChartAxisTitle { Text = "Height (cm)" };
            chart.YAxes.Add(secondaryAxis);
            
            // Series
            ColumnSeries series = new ColumnSeries
            {
                ItemsSource = (new PersonViewModel()).Data,
                XBindingPath = "Name",
                YBindingPath = "Height",
                ShowDataLabels = true,
                EnableTooltip = true,
                Label = "Height",
                DataLabelSettings = new CartesianDataLabelSettings
                {
                    LabelPlacement = DataLabelPlacement.Inner
                }
            };
            
            chart.Series.Add(series);
            this.Content = chart;
        }
    }
}
```

## Data Binding Concepts

### ItemsSource

The `ItemsSource` property binds to your data collection. Use `ObservableCollection<T>` for automatic UI updates when data changes.

### XBindingPath and YBindingPath

These properties specify which data model properties to use for X and Y values:

```csharp
// Model properties
public class SalesData
{
    public string Month { get; set; }  // Used for XBindingPath
    public double Sales { get; set; }   // Used for YBindingPath
}

// Series binding
<chart:ColumnSeries ItemsSource="{Binding Data}"
                   XBindingPath="Month"
                   YBindingPath="Sales"/>
```

## Common Gotchas

### Missing Namespace Registration

**Problem:** Chart doesn't render or throws runtime errors.

**Solution:** Ensure you've called `ConfigureSyncfusionCore()` in `MauiProgram.cs`:

```csharp
builder.ConfigureSyncfusionCore();
```

### No Data Displayed

**Problem:** Chart renders but shows no data points.

**Solutions:**
1. Verify `ItemsSource` has data
2. Check `XBindingPath` and `YBindingPath` match property names exactly (case-sensitive)
3. Ensure BindingContext is set correctly
4. Use `ObservableCollection` for dynamic data

### Axes Not Visible

**Problem:** Chart area appears but no axes or data.

**Solution:** Verify both XAxes and YAxes collections have at least one axis:

```xml
<chart:SfCartesianChart.XAxes>
    <chart:CategoryAxis/>
</chart:SfCartesianChart.XAxes>

<chart:SfCartesianChart.YAxes>
    <chart:NumericalAxis/>
</chart:SfCartesianChart.YAxes>
```

## Next Steps

After getting your basic chart running:

1. **Explore Chart Types**: Try LineSeries, AreaSeries, or other series types
2. **Add Interactivity**: Enable tooltips, trackball, zooming, and selection
3. **Customize Appearance**: Configure colors, legends, data labels
4. **Advanced Features**: Add annotations, trendlines, plotbands

## Quick Reference

### Minimum Required Code (XAML)

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    <chart:ColumnSeries ItemsSource="{Binding Data}"
                       XBindingPath="Category"
                       YBindingPath="Value"/>
</chart:SfCartesianChart>
```

### Minimum Required Code (C#)

```csharp
SfCartesianChart chart = new SfCartesianChart();
chart.XAxes.Add(new CategoryAxis());
chart.YAxes.Add(new NumericalAxis());
chart.Series.Add(new ColumnSeries
{
    ItemsSource = data,
    XBindingPath = "Category",
    YBindingPath = "Value"
});
this.Content = chart;
```
