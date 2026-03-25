# Getting Started with .NET MAUI Sunburst Chart

This guide covers the complete setup and basic implementation of the Syncfusion .NET MAUI Sunburst Chart, from package installation to creating a fully functional hierarchical visualization with data, title, legend, tooltips, and data labels.

## Package Installation

### Step 1: Add NuGet Package

1. In **Solution Explorer**, right-click your project and select **Manage NuGet Packages**
2. Search for `Syncfusion.Maui.SunburstChart`
3. Install the latest version
4. Ensure all dependencies are installed correctly and the project is restored.

## Step 2: Register the Syncfusion Handler

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

Add the sunburst chart namespace to your XAML or C# file:

**XAML:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sunburst="clr-namespace:Syncfusion.Maui.SunburstChart;assembly=Syncfusion.Maui.SunburstChart"
             x:Class="YourApp.MainPage">
    
    <sunburst:SfSunburstChart/>
    
</ContentPage>
```

**C#:**
```csharp
using Syncfusion.Maui.SunburstChart;

namespace SunburstGettingStarted
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            SfSunburstChart sunburst = new SfSunburstChart();
            this.Content = sunburst;
        }
    }
}
```

## Creating the Data Model

### Step 1: Define Data Model

Create a model class that represents hierarchical data:

```csharp
public class SunburstModel
{
    public string Country { get; set; }
    public string JobDescription { get; set; }
    public string JobGroup { get; set; }
    public double EmployeesCount { get; set; }
}
```

**Key properties:**
- **Categorical properties** (Country, JobDescription, JobGroup): Used for grouping at different hierarchy levels
- **Numeric property** (EmployeesCount): Used for calculating segment sizes

### Step 2: Create View Model

Initialize a collection of data in your view model:

```csharp
using System.Collections.ObjectModel;

public class SunburstViewModel
{
    public ObservableCollection<SunburstModel> DataSource { get; set; }

    public SunburstViewModel()
    {
        this.DataSource = new ObservableCollection<SunburstModel>
        {
            new SunburstModel { Country = "USA", JobDescription = "Sales", JobGroup = "Executive", EmployeesCount = 50 },
            new SunburstModel { Country = "USA", JobDescription = "Sales", JobGroup = "Analyst", EmployeesCount = 40 },
            new SunburstModel { Country = "USA", JobDescription = "Marketing", JobGroup = "", EmployeesCount = 40 },
            new SunburstModel { Country = "USA", JobDescription = "Technical", JobGroup = "Testers", EmployeesCount = 35 },
            new SunburstModel { Country = "USA", JobDescription = "Technical", JobGroup = "Developers", EmployeesCount = 175 },
            new SunburstModel { Country = "USA", JobDescription = "Management", JobGroup = "", EmployeesCount = 40 },
            new SunburstModel { Country = "USA", JobDescription = "Accounts", JobGroup = "", EmployeesCount = 60 },
            
            new SunburstModel { Country = "India", JobDescription = "Technical", JobGroup = "Testers", EmployeesCount = 33 },
            new SunburstModel { Country = "India", JobDescription = "Technical", JobGroup = "Developers", EmployeesCount = 125 },
            new SunburstModel { Country = "India", JobDescription = "HR Executives", JobGroup = "", EmployeesCount = 70 },
            new SunburstModel { Country = "India", JobDescription = "Accounts", JobGroup = "", EmployeesCount = 45 },
            
            new SunburstModel { Country = "Germany", JobDescription = "Sales", JobGroup = "Executive", EmployeesCount = 30 },
            new SunburstModel { Country = "Germany", JobDescription = "Sales", JobGroup = "Analyst", EmployeesCount = 40 },
            new SunburstModel { Country = "Germany", JobDescription = "Marketing", JobGroup = "", EmployeesCount = 50 },
            new SunburstModel { Country = "Germany", JobDescription = "Technical", JobGroup = "Testers", EmployeesCount = 40 },
            new SunburstModel { Country = "Germany", JobDescription = "Technical", JobGroup = "Developers", EmployeesCount = 60 },
            new SunburstModel { Country = "Germany", JobDescription = "Management", JobGroup = "", EmployeesCount = 40 },
            new SunburstModel { Country = "Germany", JobDescription = "Accounts", JobGroup = "", EmployeesCount = 55 },
            
            new SunburstModel { Country = "UK", JobDescription = "Technical", JobGroup = "Testers", EmployeesCount = 96 },
            new SunburstModel { Country = "UK", JobDescription = "Technical", JobGroup = "Developers", EmployeesCount = 55 },
            new SunburstModel { Country = "UK", JobDescription = "HR Executives", JobGroup = "", EmployeesCount = 60 },
            new SunburstModel { Country = "UK", JobDescription = "Accounts", JobGroup = "", EmployeesCount = 45 }
        };
    }
}
```

### Step 3: Set Binding Context

Bind the view model to your page:

**XAML:**
```xml
<ContentPage xmlns:model="clr-namespace:YourApp.ViewModels">
    <ContentPage.BindingContext>
        <model:SunburstViewModel/>
    </ContentPage.BindingContext>
</ContentPage>
```

**C#:**
```csharp
this.BindingContext = new SunburstViewModel();
```

## Populating the Chart with Data

### Configure ItemsSource and ValueMemberPath

Bind the data source and specify which property determines segment size:

**XAML:**
```xml
<sunburst:SfSunburstChart ItemsSource="{Binding DataSource}" 
                          ValueMemberPath="EmployeesCount">
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
SfSunburstChart sunburst = new SfSunburstChart();
sunburst.ItemsSource = (new SunburstViewModel()).DataSource;
sunburst.ValueMemberPath = "EmployeesCount";
```

**Key properties:**
- **ItemsSource**: The collection of data items
- **ValueMemberPath**: Property name used to calculate segment sizes (must be numeric)

### Configure Hierarchical Levels

Add hierarchical levels using SunburstHierarchicalLevel. Each level groups data by the specified property:

**XAML:**
```xml
<sunburst:SfSunburstChart ItemsSource="{Binding DataSource}" 
                          ValueMemberPath="EmployeesCount">
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Country"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="JobDescription"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="JobGroup"/>
    </sunburst:SfSunburstChart.Levels>
    
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
sunburst.Levels.Add(new SunburstHierarchicalLevel() { GroupMemberPath = "Country" });
sunburst.Levels.Add(new SunburstHierarchicalLevel() { GroupMemberPath = "JobDescription" });
sunburst.Levels.Add(new SunburstHierarchicalLevel() { GroupMemberPath = "JobGroup" });
```

**How it works:**
- First level (innermost ring): Groups by Country
- Second level: Groups by JobDescription within each Country
- Third level (outermost ring): Groups by JobGroup within each JobDescription
- Each segment size is proportional to the sum of EmployeesCount values

## Adding Chart Features

### Add Title

**XAML:**
```xml
<sunburst:SfSunburstChart>
    <sunburst:SfSunburstChart.Title>
        <Label Text="Employees Count" FontSize="18" FontAttributes="Bold"/>
    </sunburst:SfSunburstChart.Title>
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
sunburst.Title = new Label
{
    Text = "Employees Count",
    FontSize = 18,
    FontAttributes = FontAttributes.Bold
};
```

### Add Legend

**XAML:**
```xml
<sunburst:SfSunburstChart>
    <sunburst:SfSunburstChart.Legend>
        <sunburst:SunburstLegend/>
    </sunburst:SfSunburstChart.Legend>
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
sunburst.Legend = new SunburstLegend();
```

### Enable Tooltips

**XAML:**
```xml
<sunburst:SfSunburstChart EnableTooltip="True">
```

**C#:**
```csharp
sunburst.EnableTooltip = true;
```

### Enable Data Labels

**XAML:**
```xml
<sunburst:SfSunburstChart ShowLabels="True">
```

**C#:**
```csharp
sunburst.ShowLabels = true;
```

## Complete Example

Here's a complete, working example combining all elements:

**XAML:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sunburst="clr-namespace:Syncfusion.Maui.SunburstChart;assembly=Syncfusion.Maui.SunburstChart"
             xmlns:model="clr-namespace:SunburstGettingStarted"
             x:Class="SunburstGettingStarted.MainPage">

    <sunburst:SfSunburstChart ItemsSource="{Binding DataSource}" 
                              ShowLabels="True"  
                              EnableTooltip="True"
                              ValueMemberPath="EmployeesCount">

        <sunburst:SfSunburstChart.BindingContext>
            <model:SunburstViewModel/>
        </sunburst:SfSunburstChart.BindingContext>

        <sunburst:SfSunburstChart.Title>
            <Label Text="Employees Count" FontSize="18" FontAttributes="Bold"/>
        </sunburst:SfSunburstChart.Title>

        <sunburst:SfSunburstChart.Legend>
            <sunburst:SunburstLegend/>
        </sunburst:SfSunburstChart.Legend> 

        <sunburst:SfSunburstChart.Levels>
            <sunburst:SunburstHierarchicalLevel GroupMemberPath="Country"/>
            <sunburst:SunburstHierarchicalLevel GroupMemberPath="JobDescription"/>
            <sunburst:SunburstHierarchicalLevel GroupMemberPath="JobGroup"/>
        </sunburst:SfSunburstChart.Levels>

    </sunburst:SfSunburstChart>

</ContentPage>
```

**C#:**
```csharp
using Syncfusion.Maui.SunburstChart;

namespace SunburstGettingStarted
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            SfSunburstChart sunburst = new SfSunburstChart();
            sunburst.ItemsSource = (new SunburstViewModel()).DataSource;
            sunburst.ValueMemberPath = "EmployeesCount";

            sunburst.Title = new Label
            {
                Text = "Employees Count",
                FontSize = 18,
                FontAttributes = FontAttributes.Bold
            };

            sunburst.Legend = new SunburstLegend();

            sunburst.Levels.Add(new SunburstHierarchicalLevel() { GroupMemberPath = "Country" });
            sunburst.Levels.Add(new SunburstHierarchicalLevel() { GroupMemberPath = "JobDescription" });
            sunburst.Levels.Add(new SunburstHierarchicalLevel() { GroupMemberPath = "JobGroup" });

            sunburst.EnableTooltip = true;
            sunburst.ShowLabels = true;
            
            this.Content = sunburst;
        }
    }
}
```

## Key Concepts

### Hierarchical Data Structure

The sunburst chart requires flat data with properties representing different hierarchy levels:

```
Country (Level 1) → JobDescription (Level 2) → JobGroup (Level 3)
    USA → Sales → Executive (50 employees)
    USA → Sales → Analyst (40 employees)
    USA → Technical → Developers (175 employees)
```

### Segment Size Calculation

Each segment's size is calculated by:
1. Summing all ValueMemberPath values for items in that group
2. Calculating the proportion relative to the parent segment
3. Rendering the arc angle proportional to this value

### Level Ordering

The order in which you add hierarchical levels matters:
- First level added = innermost ring
- Last level added = outermost ring
- Parent-child relationships are maintained automatically

## Common Issues and Solutions

**Issue:** Chart displays but no segments appear
- **Solution:** Verify ItemsSource is bound and contains data
- Check that ValueMemberPath property exists and contains numeric values
- Ensure at least one hierarchical level is configured

**Issue:** Some hierarchy levels are missing
- **Solution:** Check that GroupMemberPath properties exist on all data items
- Verify property names match exactly (case-sensitive)
- Empty or null values will skip that level for those items

**Issue:** Segments appear too small or large
- **Solution:** Adjust Radius property (default 0.9, range 0-1)
- Use InnerRadius to control center hole size
- Check that value data is in appropriate range (not too small/large)

**Issue:** Labels or tooltips not showing
- **Solution:** Set ShowLabels="True" and EnableTooltip="True"
- Check that there's enough space for labels (increase Radius if needed)
- For tooltips, ensure you're tapping/hovering over segments

## Next Steps

Now that you have a basic sunburst chart working, explore:
- **Appearance customization**: Adjust angles, radius, colors, and strokes
- **Data labels**: Configure overflow modes, rotation, and styling
- **Tooltips**: Customize appearance and create custom templates
- **Selection**: Enable segment selection with highlighting
- **Drill-down**: Add interactive navigation for deep hierarchies
- **Center view**: Display custom content in the chart center
