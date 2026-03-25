# Getting Started with Pyramid Charts

This guide covers the installation, initialization, and basic setup of the Syncfusion .NET MAUI Pyramid Chart (SfPyramidChart).

## Installation and Package Setup

### Step 1: Install NuGet Package

Install the Syncfusion.Maui.Charts package from NuGet:

1. In **Solution Explorer**, right-click the project and choose **Manage NuGet Packages**
2. Search for `Syncfusion.Maui.Charts`
3. Install the latest version
4. Ensure all dependencies are installed correctly and the project is restored

**Package Name:** `Syncfusion.Maui.Charts`

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

### Step 3: Import Namespace

Import the Charts namespace in your XAML or C# files:

**XAML:**
```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:chart="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts">
    
</ContentPage>
```

**C#:**
```csharp
using Syncfusion.Maui.Charts;
```

## Initialize SfPyramidChart

### XAML Initialization

```xaml
<ContentPage xmlns:chart="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts">
    
    <chart:SfPyramidChart/>
    
</ContentPage>
```

### C# Initialization

```csharp
using Syncfusion.Maui.Charts;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        SfPyramidChart chart = new SfPyramidChart();
        this.Content = chart;
    }
}
```

## Creating the Data Model

Define a data model that represents each segment in the pyramid chart:

```csharp
public class StageModel
{
    public string Name { get; set; }
    public double Value { get; set; }
}
```

**Key Points:**
- `Name` represents the category or stage name (X value)
- `Value` represents the numeric data (Y value)
- Properties can be named differently but must be mapped via binding paths

## Creating the ViewModel

Create a ViewModel class that provides the data collection:

```csharp
public class StageViewModel
{
    public List<StageModel> Data { get; set; }

    public StageViewModel()
    {
        Data = new List<StageModel>()
        {
            new StageModel() { Name = "Stage A", Value = 12 },
            new StageModel() { Name = "Stage B", Value = 21 },
            new StageModel() { Name = "Stage C", Value = 29 },
            new StageModel() { Name = "Stage D", Value = 37 }
        };
    }
}
```

**Best Practices:**
- Use `ObservableCollection<T>` if data will change dynamically
- Use `List<T>` for static data that won't change after initialization
- Sort data in descending order for typical pyramid visualization (largest to smallest)

## Setting the BindingContext

### XAML Approach

Add the namespace for your ViewModel:

```xaml
<ContentPage xmlns:chart="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts"
             xmlns:model="clr-namespace:YourNamespace">

    <chart:SfPyramidChart>
        <chart:SfPyramidChart.BindingContext>
            <model:StageViewModel/>
        </chart:SfPyramidChart.BindingContext>
    </chart:SfPyramidChart>
    
</ContentPage>
```

### C# Approach

```csharp
SfPyramidChart chart = new SfPyramidChart();
StageViewModel viewModel = new StageViewModel();
chart.BindingContext = viewModel;
this.Content = chart;
```

## Populating the Chart with Data

Bind the data collection to the chart using three key properties:

### Key Binding Properties

1. **ItemsSource**: The data collection
2. **XBindingPath**: Property name for category/stage names
3. **YBindingPath**: Property name for numeric values

### XAML Data Binding

```xaml
<chart:SfPyramidChart ItemsSource="{Binding Data}" 
                      XBindingPath="Name" 
                      YBindingPath="Value">
    
    <chart:SfPyramidChart.BindingContext>
        <model:StageViewModel/>
    </chart:SfPyramidChart.BindingContext>
    
</chart:SfPyramidChart>
```

### C# Data Binding

```csharp
SfPyramidChart chart = new SfPyramidChart();
StageViewModel viewModel = new StageViewModel();

chart.BindingContext = viewModel;
chart.ItemsSource = viewModel.Data;
chart.XBindingPath = "Name";
chart.YBindingPath = "Value";

this.Content = chart;
```

## Complete Working Example

### Complete XAML Implementation

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ChartGettingStarted.MainPage"
             xmlns:chart="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts"
             xmlns:model="clr-namespace:ChartGettingStarted">

    <chart:SfPyramidChart ItemsSource="{Binding Data}" 
                          ShowDataLabels="True" 
                          EnableTooltip="True"
                          XBindingPath="Name"         
                          YBindingPath="Value">

        <chart:SfPyramidChart.Title>
            <Label Text="Pyramid Stages"/>
        </chart:SfPyramidChart.Title>

        <chart:SfPyramidChart.BindingContext>
            <model:StageViewModel/>
        </chart:SfPyramidChart.BindingContext>

        <chart:SfPyramidChart.Legend>
            <chart:ChartLegend/>
        </chart:SfPyramidChart.Legend>

    </chart:SfPyramidChart>

</ContentPage>
```

### Complete C# Implementation

```csharp
using Syncfusion.Maui.Charts;

namespace ChartGettingStarted
{
    public partial class MainPage : ContentPage
    {   
        public MainPage()
        {
            InitializeComponent();
            
            SfPyramidChart chart = new SfPyramidChart();
            
            chart.Title = new Label()
            {
                Text = "Pyramid Stages"
            };
            
            chart.Legend = new ChartLegend();
            
            StageViewModel viewModel = new StageViewModel();
            chart.BindingContext = viewModel;
            
            chart.ItemsSource = viewModel.Data;
            chart.XBindingPath = "Name";
            chart.YBindingPath = "Value";
            chart.EnableTooltip = true;
            chart.ShowDataLabels = true;
            
            this.Content = chart;
        }
    }
}
```

### Complete ViewModel Implementation

```csharp
using System.Collections.Generic;

namespace ChartGettingStarted
{
    public class StageModel
    {
        public string Name { get; set; }
        public double Value { get; set; }
    }

    public class StageViewModel
    {
        public List<StageModel> Data { get; set; }

        public StageViewModel()
        {
            Data = new List<StageModel>()
            {
                new StageModel() { Name = "Stage A", Value = 12 },
                new StageModel() { Name = "Stage B", Value = 21 },
                new StageModel() { Name = "Stage C", Value = 29 },
                new StageModel() { Name = "Stage D", Value = 37 }
            };
        }
    }
}
```

## Common Issues and Troubleshooting

### Issue: Chart Not Displaying

**Possible Causes:**
- ItemsSource is null or empty
- XBindingPath or YBindingPath doesn't match property names (case-sensitive)
- BindingContext not set correctly
- Chart not added to visual tree

**Solutions:**
- Verify data is loaded in ViewModel
- Check property name spelling and casing
- Ensure BindingContext is set before or along with data binding
- Make sure chart is set as Content or added to a layout container

### Issue: NuGet Package Installation Failed

**Solutions:**
- Check internet connection
- Clear NuGet cache: `dotnet nuget locals all --clear`
- Verify package compatibility with your .NET MAUI version
- Check for sufficient disk space

### Issue: Data Not Updating

**Solutions:**
- Use `ObservableCollection<T>` instead of `List<T>` for dynamic data
- Implement `INotifyPropertyChanged` on data models if individual properties change
- Call `OnPropertyChanged()` when data changes

## Next Steps

Now that you have a basic pyramid chart running:

- **Add Data Labels**: See [data-labels.md](data-labels.md) to display values on segments
- **Configure Legend**: See [legend.md](legend.md) to add segment identification
- **Enable Tooltips**: See [tooltip.md](tooltip.md) for interactive data display
- **Customize Appearance**: See [appearance-customization.md](appearance-customization.md) for colors and styles
- **Change Orientation**: See [orientation-and-effects.md](orientation-and-effects.md) for layout options
- **Export Charts**: See [exporting.md](exporting.md) to save as images or streams