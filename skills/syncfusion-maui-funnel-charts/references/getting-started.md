# Getting Started with .NET MAUI Funnel Chart

This guide walks you through setting up and implementing your first Syncfusion .NET MAUI Funnel Chart (SfFunnelChart). Funnel charts are ideal for visualizing data as progressively decreasing segments, commonly used for sales pipelines, conversion funnels, and process stages.

## Step 1: Install the NuGet Package

Install the `Syncfusion.Maui.Charts` package in your .NET MAUI project:

### Option 1: NuGet Package Manager UI
1. In **Solution Explorer**, right-click the project and choose **Manage NuGet Packages**
2. Search for `Syncfusion.Maui.Charts`
3. Install the latest version
4. Ensure dependencies are installed and the project is restored

### Option 2: .NET CLI
```bash
dotnet add package Syncfusion.Maui.Charts
```

### Option 3: Package Manager Console
```powershell
Install-Package Syncfusion.Maui.Charts
```

> **Note:** `Syncfusion.Maui.Core` is automatically installed as a required dependency. Ensure handler registration is configured in `MauiProgram.cs` (see parent library's [getting-started](../../getting-started/) guide).

### Step 2: Register the Syncfusion Handler

**CRITICAL:** The `Syncfusion.Maui.Core` NuGet is a dependent package for all Syncfusion controls. You MUST register the handler in `MauiProgram.cs`.

**File:** `MauiProgram.cs`

```csharp
using Syncfusion.Maui.Core.Hosting;

namespace MyCardApp
{
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
}
```

**Important:** Without `ConfigureSyncfusionCore()`, the chart control will not work.

## Step 3: Import the Namespace

Import the `Syncfusion.Maui.Charts` namespace in your XAML or C# code:

### XAML
```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:chart="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts">
    <!-- Chart goes here -->
</ContentPage>
```

### C#
```csharp
using Syncfusion.Maui.Charts;
```

## Step 4: Initialize SfFunnelChart

### XAML Approach
```xaml
<ContentPage xmlns:chart="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts">
    <chart:SfFunnelChart/>
</ContentPage>
```

### C# Approach
```csharp
using Syncfusion.Maui.Charts;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        SfFunnelChart chart = new SfFunnelChart();
        this.Content = chart;
    }
}
```

## Step 5: Create Data Models and ViewModels

Define a data model representing each segment in the funnel:

```csharp
public class FunnelDataModel
{
    public string XValue { get; set; }
    public double YValue { get; set; }
}
```

Create a ViewModel that provides the data collection:

```csharp
public class SalesFunnelViewModel
{
    public List<FunnelDataModel> Data { get; set; }

    public SalesFunnelViewModel()
    {
        Data = new List<FunnelDataModel>()
        {
            new FunnelDataModel { XValue = "Prospects", YValue = 320 },
            new FunnelDataModel { XValue = "Inquiries", YValue = 290 },
            new FunnelDataModel { XValue = "Applicants", YValue = 245 },
            new FunnelDataModel { XValue = "Admits", YValue = 190 },
            new FunnelDataModel { XValue = "Enrolled", YValue = 175 }
        };
    }
}
```

## Step 6: Bind Data to the Chart

Set the chart's `BindingContext` to the ViewModel and bind data using `ItemsSource`, `XBindingPath`, and `YBindingPath`:

### XAML with BindingContext
```xaml
<ContentPage xmlns:chart="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts"
             xmlns:model="clr-namespace:YourNamespace.ViewModels">

    <chart:SfFunnelChart ItemsSource="{Binding Data}" 
                         XBindingPath="XValue" 
                         YBindingPath="YValue">
        
        <chart:SfFunnelChart.BindingContext>
            <model:SalesFunnelViewModel/>
        </chart:SfFunnelChart.BindingContext>
        
    </chart:SfFunnelChart>
    
</ContentPage>
```

### C# Approach
```csharp
SfFunnelChart chart = new SfFunnelChart();
SalesFunnelViewModel viewModel = new SalesFunnelViewModel();
chart.BindingContext = viewModel;

chart.ItemsSource = viewModel.Data;
chart.XBindingPath = "XValue";
chart.YBindingPath = "YValue";

this.Content = chart;
```

## Step 7: Add Chart Title

Provide context to your chart with a descriptive title using the `Title` property:

### XAML
```xaml
<chart:SfFunnelChart ItemsSource="{Binding Data}" 
                     XBindingPath="XValue" 
                     YBindingPath="YValue">
    
    <chart:SfFunnelChart.Title>
        <Label Text="School Admission Funnel"/>
    </chart:SfFunnelChart.Title>
    
</chart:SfFunnelChart>
```

### C#
```csharp
chart.Title = new Label()
{
    Text = "School Admission Funnel"
};
```

## Step 8: Enable Data Labels and Tooltips

Make your chart more informative by enabling data labels and tooltips:

### XAML
```xaml
<chart:SfFunnelChart ItemsSource="{Binding Data}" 
                     XBindingPath="XValue" 
                     YBindingPath="YValue"
                     ShowDataLabels="True"
                     EnableTooltip="True">
    <!-- Other chart configurations -->
</chart:SfFunnelChart>
```

### C#
```csharp
chart.ShowDataLabels = true;
chart.EnableTooltip = true;
```

## Step 9: Add Legend

Display a legend to help users identify each funnel segment:

### XAML
```xaml
<chart:SfFunnelChart ItemsSource="{Binding Data}" 
                     XBindingPath="XValue" 
                     YBindingPath="YValue">
    
    <chart:SfFunnelChart.Legend>
        <chart:ChartLegend/>
    </chart:SfFunnelChart.Legend>
    
</chart:SfFunnelChart>
```

### C#
```csharp
chart.Legend = new ChartLegend();
```

## Complete Working Example

### XAML
```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="FunnelChartDemo.MainPage"
             xmlns:chart="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts"
             xmlns:model="clr-namespace:FunnelChartDemo.ViewModels">

    <chart:SfFunnelChart ItemsSource="{Binding Data}" 
                         XBindingPath="XValue" 
                         YBindingPath="YValue"
                         ShowDataLabels="True" 
                         EnableTooltip="True">

        <chart:SfFunnelChart.Title>
            <Label Text="School Admission Funnel"/>
        </chart:SfFunnelChart.Title>

        <chart:SfFunnelChart.BindingContext>
            <model:SalesFunnelViewModel/>
        </chart:SfFunnelChart.BindingContext>

        <chart:SfFunnelChart.Legend>
            <chart:ChartLegend/>
        </chart:SfFunnelChart.Legend>

    </chart:SfFunnelChart>

</ContentPage>
```

### C# Code-Behind
```csharp
using Syncfusion.Maui.Charts;

namespace FunnelChartDemo;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        SfFunnelChart chart = new SfFunnelChart();
        
        chart.Title = new Label { Text = "School Admission Funnel" };
        chart.Legend = new ChartLegend();
        
        SalesFunnelViewModel viewModel = new SalesFunnelViewModel();
        chart.BindingContext = viewModel;
        
        chart.ItemsSource = viewModel.Data;
        chart.XBindingPath = "XValue";
        chart.YBindingPath = "YValue";
        chart.EnableTooltip = true;
        chart.ShowDataLabels = true;
        
        this.Content = chart;
    }
}
```

### ViewModel
```csharp
using System.Collections.Generic;

namespace FunnelChartDemo.ViewModels
{
    public class SalesFunnelViewModel
    {
        public List<FunnelDataModel> Data { get; set; }

        public SalesFunnelViewModel()
        {
            Data = new List<FunnelDataModel>()
            {
                new FunnelDataModel { XValue = "Prospects", YValue = 320 },
                new FunnelDataModel { XValue = "Inquiries", YValue = 290 },
                new FunnelDataModel { XValue = "Applicants", YValue = 245 },
                new FunnelDataModel { XValue = "Admits", YValue = 190 },
                new FunnelDataModel { XValue = "Enrolled", YValue = 175 }
            };
        }
    }

    public class FunnelDataModel
    {
        public string XValue { get; set; }
        public double YValue { get; set; }
    }
}
```

## Key Properties Reference

| Property | Type | Description |
|----------|------|-------------|
| `ItemsSource` | IEnumerable | Data collection for the chart |
| `XBindingPath` | string | Property name for segment labels (category) |
| `YBindingPath` | string | Property name for segment values |
| `ShowDataLabels` | bool | Enable/disable data labels |
| `EnableTooltip` | bool | Enable/disable tooltips |
| `Title` | Label | Chart title |
| `Legend` | ChartLegend | Legend configuration |

## Next Steps

Now that you have a basic funnel chart running, explore these advanced features:

- **[Data Labels](data-labels.md)** - Customize label placement, context, and styling
- **[Appearance](appearance.md)** - Apply custom colors and gradients
- **[Legend](legend.md)** - Configure legend placement, icons, and templates
- **[Tooltip](tooltip.md)** - Customize tooltip templates and behavior
- **[Advanced Features](advanced-features.md)** - Orientation, spacing, and visual effects

## Troubleshooting

**Chart not displaying:**
- Verify the NuGet package is installed correctly
- Ensure `ConfigureSyncfusionCore()` is called in `MauiProgram.cs` (see parent library getting-started)
- Check that `ItemsSource`, `XBindingPath`, and `YBindingPath` are set
- Verify your ViewModel is correctly bound to the chart

**Data not showing:**
- Ensure the ViewModel's `Data` property is populated before binding
- Verify property names in `XBindingPath` and `YBindingPath` match your model
- Check for binding errors in the debug output

**Build errors:**
- Run `dotnet restore` to restore NuGet packages
- Clean and rebuild the solution
- Verify you're targeting .NET 9 or later