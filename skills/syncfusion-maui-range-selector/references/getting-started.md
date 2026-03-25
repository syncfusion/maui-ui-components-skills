# Getting Started — SfRangeSelector (.NET MAUI)

## Table of Contents
- [Step 1: Install NuGet Package](#step-1-install-nuget-package)
- [Step 2: Register the Handler](#step-2-register-the-handler)
- [Step 3: Add a Basic Range Selector](#step-3-add-a-basic-range-selector)
- [Step 4: Add Content (Chart)](#step-4-add-content-chart)
- [IDE-Specific Instructions](#ide-specific-instructions)
- [Troubleshooting](#troubleshooting)

---

## Step 1: Install NuGet Package

The Range Selector is part of the `Syncfusion.Maui.Sliders` package.

**Via Terminal / CLI:**
```bash
dotnet add package Syncfusion.Maui.Sliders
dotnet restore
```

**Via Visual Studio NuGet Manager:**
1. Right-click project → **Manage NuGet Packages**
2. Search: `Syncfusion.Maui.Sliders`
3. Install the latest version

**Via JetBrains Rider:**
1. Right-click project → **Manage NuGet Packages**
2. Search and install `Syncfusion.Maui.Sliders`
3. If restore doesn't trigger: `dotnet restore` in the terminal

---

## Step 2: Register the Handler

`Syncfusion.Maui.Core` is a required dependency — it provides the handler infrastructure for all Syncfusion MAUI controls. Call `ConfigureSyncfusionCore()` in `MauiProgram.cs`:

```csharp
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
                .ConfigureSyncfusionCore()   // ← Required
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

---

## Step 3: Add a Basic Range Selector

**XAML:**
```xaml
<ContentPage
    xmlns:sliders="clr-namespace:Syncfusion.Maui.Sliders;assembly=Syncfusion.Maui.Sliders">

    <sliders:SfRangeSelector />

</ContentPage>
```

**C#:**
```csharp
using Syncfusion.Maui.Sliders;

namespace GettingStarted
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            SfRangeSelector rangeSelector = new SfRangeSelector();
            content = rangeSelector;
        }
    }
}
```

This renders a bare range selector with default min/max values and two draggable thumbs.

---

## Step 4: Add Content (Chart)

The `Content` property embeds any MAUI view inside the selector — most commonly a chart. The chart visually represents the data being filtered by the range.

**XAML with SfCartesianChart:**
```xaml
<ContentPage
    xmlns:sliders="clr-namespace:Syncfusion.Maui.Sliders;assembly=Syncfusion.Maui.Sliders"
    xmlns:charts="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts"
    xmlns:local="clr-namespace:MyApp">

    <ContentPage.BindingContext>
        <local:ViewModel />
    </ContentPage.BindingContext>

    <sliders:SfRangeSelector Minimum="10"
                             Maximum="20"
                             RangeStart="13"
                             RangeEnd="17">
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
                <charts:SplineAreaSeries ItemsSource="{Binding Source}"
                                         XBindingPath="X"
                                         YBindingPath="Y" />
            </charts:SfCartesianChart.Series>

        </charts:SfCartesianChart>
    </sliders:SfRangeSelector>

</ContentPage>
```

**C# equivalent:**
```csharp
SfRangeSelector rangeSelector = new SfRangeSelector();
rangeSelector.Minimum = 10;
rangeSelector.Maximum = 20;
rangeSelector.RangeStart = 13;
rangeSelector.RangeEnd = 17;

SfCartesianChart chart = new SfCartesianChart();
DateTimeAxis primaryAxis = new DateTimeAxis();
chart.XAxes = primaryAxis;
NumericalAxis secondaryAxis = new NumericalAxis();
chart.YAxes = secondaryAxis;

SplineAreaSeries series = new SplineAreaSeries();
series.ItemsSource = (new ViewModel()).Source;
series.XBindingPath = "X";
series.YBindingPath = "Y";
chart.Series.Add(series);

rangeSelector.Content = chart;
```

> **Tip:** Hide the chart's axes (`IsVisible="False"`, `ShowMajorGridLines="False"`) so the chart appears as a clean background behind the selector thumbs.

---

## IDE-Specific Instructions

### Visual Studio Code
1. Open the Command Palette (`Ctrl+Shift+P`) → **.NET: New Project** → **.NET MAUI App**
2. Install the package: `dotnet add package Syncfusion.Maui.Sliders`
3. Restore: `dotnet restore`

### JetBrains Rider
1. **File > New Solution** → **.NET MAUI App**
2. Right-click project → **Manage NuGet Packages** → search `Syncfusion.Maui.Sliders`
3. If restore fails: open terminal → `dotnet restore`

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Component doesn't render | Ensure `ConfigureSyncfusionCore()` is called in `MauiProgram.cs` |
| NuGet restore fails | Run `dotnet restore` manually in terminal |
| `SfRangeSelector` not found in XAML | Verify the `xmlns:sliders` namespace declaration |
| Chart content not visible | Check `ItemsSource` binding and ViewModel data |
| Thumbs not dragging | Ensure `Minimum`/`Maximum` are set with a valid range |
