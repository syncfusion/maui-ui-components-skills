# Charts Migration: Xamarin.Forms  to .NET MAUI

Migration guide for all Syncfusion chart controls from Xamarin.Forms SfChart to .NET MAUI specialized chart controls.

## Table of Contents
- [Overview](#overview)
- [Chart Type Divisions](#chart-type-divisions)
- [SfCartesianChart Migration](#sfcartesianchart-migration)
- [SfCircularChart Migration](#sfcircularchart-migration)
- [SfPolarChart Migration](#sfpolarchart-migration)
- [SfFunnelChart Migration](#sffunnelchart-migration)
- [SfPyramidChart Migration](#sfpyramidchart-migration)
- [Common Chart API Changes](#common-chart-api-changes)
- [Migration Examples](#migration-examples)

## Overview

Syncfusion's Xamarin.Forms `SfChart` has been divided into **five specialized chart controls** in .NET MAUI for better user experience, performance, and maintainability. The charts were rebuilt from scratch using upgraded .NET MAUI graphics libraries.

### Why the Split?

- **Better Performance**: Each chart type optimized for its specific use case
- **Clearer API**: Specialized properties for each chart family
- **Improved Maintainability**: Focused documentation and updates
- **Enhanced User Experience**: Intuitive control selection

## Chart Type Divisions

| Xamarin.Forms | .NET MAUI | Series Types |
|---------------|-----------|--------------|
| SfChart | **SfCartesianChart** |Line, Area, Column, Bar, Spline, StepLine, etc.|
| SfChart | **SfCircularChart** | Pie, Doughnut |
| SfChart | **SfPolarChart** | Polar, Radar |
| SfChart | **SfFunnelChart** | Funnel |
| SfChart | **SfPyramidChart** | Pyramid |

## SfCartesianChart Migration

### Namespace Changes

```csharp
// Xamarin.Forms
using Syncfusion.SfChart.XForms;

// .NET MAUI
using Syncfusion.Maui.Charts;
```

### Initialization

**Xamarin.Forms:**
```xml
<chart:SfChart>
    <chart:SfChart.PrimaryAxis>
        <chart:CategoryAxis/>
    </chart:SfChart.PrimaryAxis>
    <chart:SfChart.SecondaryAxis>
        <chart:NumericalAxis/>
    </chart:SfChart.SecondaryAxis>
   
    <chart:LineSeries ItemsSource="{Binding Data}" 
                      XBindingPath="Month" 
                      YBindingPath="Value"/>
</chart:SfChart>
```

**.NET MAUI:**
```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:LineSeries ItemsSource="{Binding Data}"
                      XBindingPath="Month"
                      YBindingPath="Value"/>
</chart:SfCartesianChart>
```

### Key Property Changes

| Xamarin SfChart | MAUI SfCartesianChart | Description |
|-----------------|----------------------|-------------|
| `PrimaryAxis` | `XAxes` | X-axis collection |
| `SecondaryAxis` | `YAxes` | Y-axis collection |
| `ColorModel`, `CustomBrushes` | `PaletteBrushes` | Color palette |
| `SideBySideSeriesPlacement` | `EnableSideBySideSeriesPlacement` | Control series placement |
| `ChartBehaviors` | `TooltipBehavior`, `TrackballBehavior`, `SelectionBehavior`, `ZoomPanBehavior` | Separated behaviors |

### Axis Changes

| Xamarin | MAUI | Description |
|---------|------|-------------|
| `LabelRotationAngle` | `LabelRotation` | Label rotation angle |
| `OpposedPosition` | `CrossesAt` | Axis crossing position |
| n/a | `CrossAxisName` | Cross axis reference |
| `PlotOffset` | `PlotOffsetStart`, `PlotOffsetEnd` | Split into two properties |
| `ShowTrackballInfo` | `ShowTrackballLabel` | Trackball label visibility |

### Code Migration Example

**Xamarin:**
```csharp
SfChart chart = new SfChart();
chart.PrimaryAxis = new CategoryAxis();
chart.SecondaryAxis = new NumericalAxis();

LineSeries series = new LineSeries();
series.ItemsSource = viewModel.Data;
series.XBindingPath = "Month";
series.YBindingPath = "Value";
chart.Series.Add(series);
```

.**NET MAUI:**
```csharp
SfCartesianChart chart = new SfCartesianChart();
chart.XAxes.Add(new CategoryAxis());
chart.YAxes.Add(new NumericalAxis());

LineSeries series = new LineSeries();
series.ItemsSource = viewModel.Data;
series.XBindingPath = "Month";
series.YBindingPath = "Value";
chart.Series.Add(series);
```

## SfCircularChart Migration

### Namespace Changes

```csharp
// Same as Cartesian
using Syncfusion.Maui.Charts;
```

### Initialization

**Xamarin.Forms:**
```xml
<chart:SfChart>
    <chart:PieSeries ItemsSource="{Binding Data}" 
                     XBindingPath="Category" 
                     YBindingPath="Value"/>
</chart:SfChart>
```

**.NET MAUI:**
```xml
<chart:SfCircularChart>
    <chart:PieSeries ItemsSource="{Binding Data}"
                     XBindingPath="Category"
                     YBindingPath="Value"/>
</chart:SfCircularChart>
```

### Key Property Changes

| Xamarin SfChart | MAUI SfCircularChart | Description |
|-----------------|---------------------|-------------|
| `ColorModel` | `PaletteBrushes` | Color palette |
| `DataMarker` | `ShowDataLabels`, `DataLabelSettings` | Data label configuration |
| `Color` | `Fill` | Series fill color |

### Series-Specific Changes

**Pie & Doughnut Series:**
- `Color` → `Fill` (Brush instead of Color)
- `DataMarker` split into `ShowDataLabels` (bool) and `DataLabelSettings` (object)
- Better label positioning options
- Enhanced selection capabilities

## SfPolarChart Migration

### Initialization

**.NET MAUI:**
```xml
<chart:SfPolarChart>
    <chart:SfPolarChart.PrimaryAxis>
        <chart:CategoryAxis/>
    </chart:SfPolarChart.PrimaryAxis>
    <chart:SfPolarChart.SecondaryAxis>
        <chart:NumericalAxis/>
    </chart:SfPolarChart.SecondaryAxis>
    
    <chart:PolarLineSeries ItemsSource="{Binding Data}"
                           XBindingPath="Direction"
                           YBindingPath="Value"/>
</chart:SfPolarChart>
```

### Key Changes

- Same axis structure as Cartesian charts (`PrimaryAxis`, `SecondaryAxis`)
- Specialized series: `PolarLineSeries`, `PolarAreaSeries`, `RadarLineSeries`, `RadarAreaSeries`
- Grid line customization specific to polar charts

## SfFunnelChart Migration

### Initialization

**.NET MAUI:**
```xml
<chart:SfFunnelChart ItemsSource="{Binding Data}"
                     XBindingPath="Category"
                     YBindingPath="Value">
    <chart:SfFunnelChart.DataLabelSettings>
        <chart:FunnelDataLabelSettings/>
    </chart:SfFunnelChart.DataLabelSettings>
</chart:SfFunnelChart>
```

### Key Changes

- Simpler structure (no scales/axes needed)
- `FunnelMode` property for funnel shape customization
- Improved data label positioning
- Better neck customization

## SfPyramidChart Migration

### Initialization

**.NET MAUI:**
```xml
<chart:SfPyramidChart ItemsSource="{Binding Data}"
                      XBindingPath="Category"
                      YBindingPath="Value">
    <chart:SfPyramidChart.DataLabelSettings>
        <chart:PyramidDataLabelSettings/>
    </chart:SfPyramidChart.DataLabelSettings>
</chart:SfPyramidChart>
```

### Key Changes

- Simpler structure (no scales/axes needed)
- `PyramidMode` property for pyramid shape customization
- Enhanced selection support

## Common Chart API Changes

### Legend

**Property Changes:**

| Xamarin | MAUI | Description |
|---------|------|-------------|
| `DockPosition` | `Placement` | Legend position |
| `BackgroundColor` | `Background` | Background brush |
| `StrokeColor` | `Stroke` | Border color |

### Data Labels

**Xamarin:**
```xml
<chart:LineSeries>
    <chart:LineSeries.DataMarker>
        <chart:ChartDataMarker ShowLabel="True"/>
    </chart:LineSeries.DataMarker>
</chart:LineSeries>
```

**.NET MAUI:**
```xml
<chart:LineSeries ShowDataLabels="True">
    <chart:LineSeries.DataLabelSettings>
        <chart:CartesianDataLabelSettings/>
    </chart:LineSeries.DataLabelSettings>
</chart:LineSeries>
```

### Tooltip

**Xamarin:**
```xml
<chart:SfChart>
    <chart:SfChart.ChartBehaviors>
        <chart:ChartTooltipBehavior/>
    </chart:SfChart.ChartBehaviors>
</chart:SfChart>
```

**.NET MAUI:**
```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.TooltipBehavior>
        <chart:ChartTooltipBehavior/>
    </chart:SfCartesianChart.TooltipBehavior>
</chart:SfCartesianChart>
```

### Zooming & Panning

**Xamarin:**
```xml
<chart:ChartBehaviors>
    <chart:ChartZoomPanBehavior EnablePanning="True" EnableZooming="True"/>
</chart:ChartBehaviors>
```

**.NET MAUI:**
```xml
<chart:SfCartesianChart.ZoomPanBehavior>
    <chart:ChartZoomPanBehavior EnablePanning="True" EnableZooming="True"/>
</chart:SfCartesianChart.ZoomPanBehavior>
```

### Selection

**Xamarin:**
```xml
<chart:ChartBehaviors>
    <chart:ChartSelectionBehavior/>
</chart:ChartBehaviors>
```

**.NET MAUI:**
```xml
<chart:SfCartesianChart.SelectionBehavior>
    <chart:SeriesSelectionBehavior/>
</chart:SfCartesianChart.SelectionBehavior>
```

## Migration Examples

### Complete Cartesian Chart Migration

**Xamarin:**
```csharp
SfChart chart = new SfChart();
chart.Title = new ChartTitle { Text = "Sales Analysis" };
chart.Legend = new ChartLegend { DockPosition = LegendPlacement.Top };

chart.PrimaryAxis = new CategoryAxis { Title = new ChartAxisTitle { Text = "Month" } };
chart.SecondaryAxis = new NumericalAxis { Title = new ChartAxisTitle { Text = "Sales" } };

ColumnSeries series = new ColumnSeries();
series.ItemsSource = viewModel.Data;
series.XBindingPath = "Month";
series.YBindingPath = "Sales";
series.Label = "2023";
series.DataMarker = new ChartDataMarker { ShowLabel = true };
chart.Series.Add(series);

chart.ChartBehaviors.Add(new ChartTooltipBehavior());
```

**.NET MAUI:**
```csharp
SfCartesianChart chart = new SfCartesianChart();
chart.Title = new ChartTitle { Text = "Sales Analysis" };
chart.Legend = new ChartLegend { Placement = LegendPlacement.Top };

CategoryAxis xAxis = new CategoryAxis { Title = new ChartAxisTitle { Text = "Month" } };
chart.XAxes.Add(xAxis);

NumericalAxis yAxis = new NumericalAxis { Title = new ChartAxisTitle { Text = "Sales" } };
chart.YAxes.Add(yAxis);

ColumnSeries series = new ColumnSeries();
series.ItemsSource = viewModel.Data;
series.XBindingPath = "Month";
series.YBindingPath = "Sales";
series.Label = "2023";
series.ShowDataLabels = true;
chart.Series.Add(series);

chart.TooltipBehavior = new ChartTooltipBehavior();
```

### Complete Circular Chart Migration

**Xamarin:**
```csharp
SfChart chart = new SfChart();
PieSeries series = new PieSeries();
series.ItemsSource = viewModel.Data;
series.XBindingPath = "Category";
series.YBindingPath = "Value";
series.DataMarker = new ChartData Marker { ShowLabel = true };
chart.Series.Add(series);
```

**.NET MAUI:**
```csharp
SfCircularChart chart = new SfCircularChart();
PieSeries series = new PieSeries();
series.ItemsSource = viewModel.Data;
series.XBindingPath = "Category";
series.YBindingPath = "Value";
series.ShowDataLabels = true;
series.DataLabelSettings = new CircularDataLabelSettings();
chart.Series.Add(series);
```

## Troubleshooting

### Issue: Cannot find SfChart in MAUI

**Solution:** `SfChart` no longer exists. Use the specific chart type:
- `SfCartesianChart` for line, column, bar, area, etc.
- `SfCircularChart` for pie, doughnut
- `SfPolarChart` for polar, radar
- `SfFunnelChart` for funnel
- `SfPyramidChart` for pyramid

### Issue: PrimaryAxis/SecondaryAxis not found

**Solution:** Use `XAxes` and `YAxes` collections in Cartesian and Polar charts.

### Issue: DataMarker property not found

**Solution:** Use `ShowDataLabels` property and `DataLabelSettings` for configuration.

### Issue: ChartBehaviors collection not found

**Solution:** Use specific behavior properties:
- `TooltipBehavior`
- `SelectionBehavior`
- `ZoomPanBehavior`
-`TrackballBehavior`

## Next Steps

1. Identify which chart types you're using (likely becoming SfCartesianChart)
2. Update NuGet package: `Syncfusion.Maui.Charts`
3. Update namespaces
4. Replace control names in XAML
5. Update axis properties (`PrimaryAxis` → `XAxes`, `SecondaryAxis` → `YAxes`)
6. Update behavior syntax
7. Test each chart individually
