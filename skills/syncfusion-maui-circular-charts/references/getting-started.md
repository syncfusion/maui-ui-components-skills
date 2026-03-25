# Getting Started with .NET MAUI Circular Charts

This guide covers the installation, setup, and basic implementation of Syncfusion .NET MAUI Circular Charts (SfCircularChart).

## Installation

### Step 1: Install NuGet Package

1. In **Solution Explorer**, right-click your project and choose **Manage NuGet Packages**
2. Search for `Syncfusion.Maui.Charts`
3. Install the latest version
4. Ensure dependencies are restored correctly

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

## Basic Implementation

### Step 1: Import Namespace

Add the Syncfusion.Maui.Charts namespace to your XAML or C# file:

**XAML:**
```xml
<ContentPage xmlns:chart="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts">
```

**C#:**
```csharp
using Syncfusion.Maui.Charts;
```

### Step 2: Initialize SfCircularChart

Create an instance of the SfCircularChart control:

**XAML:**
```xml
<chart:SfCircularChart/>
```

**C#:**
```csharp
SfCircularChart chart = new SfCircularChart();
this.Content = chart;
```

## Creating Data Model and ViewModel

### Step 1: Define Data Model

Create a data model class with properties for category and value:

```csharp
public class SalesModel
{
    public string Product { get; set; }
    public double SalesRate { get; set; }
}
```

### Step 2: Create ViewModel

Initialize a collection of data in your ViewModel:

```csharp
public class SalesViewModel
{
    public List<SalesModel> Data { get; set; }

    public SalesViewModel()
    {
        Data = new List<SalesModel>()
        {
            new SalesModel { Product = "iPad", SalesRate = 25 },
            new SalesModel { Product = "iPhone", SalesRate = 35 },
            new SalesModel { Product = "MacBook", SalesRate = 15 },
            new SalesModel { Product = "Mac", SalesRate = 5 },
            new SalesModel { Product = "Others", SalesRate = 20 }
        };
    }
}
```

### Step 3: Set BindingContext

Assign the ViewModel as the chart's BindingContext:

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.BindingContext>
        <model:SalesViewModel/>
    </chart:SfCircularChart.BindingContext>
</chart:SfCircularChart>
```

**C#:**
```csharp
SfCircularChart chart = new SfCircularChart();
chart.BindingContext = new SalesViewModel();
```

## Adding a Series

### Understanding XBindingPath and YBindingPath

- **XBindingPath**: Property name from your data model for category labels (e.g., "Product")
- **YBindingPath**: Property name from your data model for values (e.g., "SalesRate")

### Add PieSeries

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.BindingContext>
        <model:SalesViewModel/>
    </chart:SfCircularChart.BindingContext>

    <chart:PieSeries ItemsSource="{Binding Data}" 
                     XBindingPath="Product" 
                     YBindingPath="SalesRate"/>
</chart:SfCircularChart>
```

**C#:**
```csharp
SfCircularChart chart = new SfCircularChart();
SalesViewModel viewModel = new SalesViewModel();
chart.BindingContext = viewModel;

PieSeries series = new PieSeries
{
    ItemsSource = viewModel.Data,
    XBindingPath = "Product",
    YBindingPath = "SalesRate"
};

chart.Series.Add(series);
this.Content = chart;
```

## Adding Chart Elements

### Add Title

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.Title>
        <Label Text="PRODUCT SALES"/>
    </chart:SfCircularChart.Title>
</chart:SfCircularChart>
```

**C#:**
```csharp
chart.Title = new Label
{
    Text = "PRODUCT SALES"
};
```

### Add Legend

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.Legend>
        <chart:ChartLegend/>
    </chart:SfCircularChart.Legend>
</chart:SfCircularChart>
```

**C#:**
```csharp
chart.Legend = new ChartLegend();
```

### Enable Data Labels

**XAML:**
```xml
<chart:PieSeries ItemsSource="{Binding Data}"
                 XBindingPath="Product"
                 YBindingPath="SalesRate"
                 ShowDataLabels="True"/>
```

**C#:**
```csharp
series.ShowDataLabels = true;
```

### Enable Tooltips

**XAML:**
```xml
<chart:PieSeries ItemsSource="{Binding Data}"
                 XBindingPath="Product"
                 YBindingPath="SalesRate"
                 EnableTooltip="True"/>
```

**C#:**
```csharp
series.EnableTooltip = true;
```

## Complete Working Example

### XAML Implementation

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ChartGettingStarted.MainPage"
             xmlns:chart="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts"
             xmlns:model="clr-namespace:ChartGettingStarted">

    <chart:SfCircularChart>
        <chart:SfCircularChart.Title>
            <Label Text="PRODUCT SALES"/>
        </chart:SfCircularChart.Title>

        <chart:SfCircularChart.BindingContext>
            <model:SalesViewModel/>
        </chart:SfCircularChart.BindingContext>

        <chart:SfCircularChart.Legend>
            <chart:ChartLegend/>
        </chart:SfCircularChart.Legend>

        <chart:PieSeries ItemsSource="{Binding Data}"
                         XBindingPath="Product" 
                         YBindingPath="SalesRate"
                         ShowDataLabels="True"
                         EnableTooltip="True"/>
    </chart:SfCircularChart>
    
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
            SfCircularChart chart = new SfCircularChart();
            
            chart.Title = new Label
            {
                Text = "PRODUCT SALES"
            };
            
            chart.Legend = new ChartLegend();
            
            SalesViewModel viewModel = new SalesViewModel();
            chart.BindingContext = viewModel;

            PieSeries series = new PieSeries
            {
                ItemsSource = viewModel.Data,
                XBindingPath = "Product",
                YBindingPath = "SalesRate",
                EnableTooltip = true,
                ShowDataLabels = true
            };
            
            chart.Series.Add(series);
            this.Content = chart;
        }
    }

    // Data Model
    public class SalesModel
    {
        public string Product { get; set; }
        public double SalesRate { get; set; }
    }

    // ViewModel
    public class SalesViewModel
    {
        public List<SalesModel> Data { get; set; }

        public SalesViewModel()
        {
            Data = new List<SalesModel>()
            {
                new SalesModel { Product = "iPad", SalesRate = 25 },
                new SalesModel { Product = "iPhone", SalesRate = 35 },
                new SalesModel { Product = "MacBook", SalesRate = 15 },
                new SalesModel { Product = "Mac", SalesRate = 5 },
                new SalesModel { Product = "Others", SalesRate = 20 }
            };
        }
    }
}
```

## Common Issues and Solutions

### Issue: Chart Not Displaying

**Solution:**
- Ensure `ConfigureSyncfusionCore()` is called in `MauiProgram.cs`
- Verify NuGet package is installed correctly
- Check that namespace is imported
- Confirm data binding paths match your model properties

### Issue: Empty Chart with No Data

**Solution:**
- Verify `ItemsSource` is bound to a non-empty collection
- Check `XBindingPath` and `YBindingPath` property names match exactly
- Ensure BindingContext is set correctly
- Verify data model properties are public

### Issue: Series Not Appearing

**Solution:**
- Confirm series is added to the `Series` collection
- Check that YBindingPath values are numeric and not null
- Ensure the chart has sufficient size to render

## Next Steps

- Explore different chart types in **chart-types.md** (Pie, Doughnut, Radial Bar)
- Customize data labels using **data-labels.md**
- Add interactivity with **tooltip.md** and **selection.md**
- Style your chart with **appearance.md**
- Learn about legend customization in **legend.md**
