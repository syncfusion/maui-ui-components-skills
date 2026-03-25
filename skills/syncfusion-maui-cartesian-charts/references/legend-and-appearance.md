# Legend and Appearance

## Table of Contents
- [Overview](#overview)
- [Legend Configuration](#legend-configuration)
- [Legend Item Visibility](#legend-item-visibility)
- [Legend Customization](#legend-customization)
- [Legend Placement](#legend-placement)
- [Legend Maximum Size Request](#legend-maximum-size-request)
- [Legend Item Template](#legend-item-template)
- [Legend Events](#legend-events)
- [Chart Appearance](#chart-appearance)
- [Complete Examples](#complete-examples)
- [Limitations](#limitations)

## Overview

The legend provides information about the data series in the chart. The `ChartLegend` class allows customization of legend appearance, placement, and behavior. Chart appearance can be customized using palette brushes and gradients to create visually appealing charts.

## Legend Configuration

### Enable Legend

Set the `Legend` property of `SfCartesianChart` to enable the legend.

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.Legend>
        <chart:ChartLegend/>
    </chart:SfCartesianChart.Legend>
    
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:ColumnSeries ItemsSource="{Binding Data}"
                       XBindingPath="Month"
                       YBindingPath="Product1"
                       Label="Product A"/>
    
    <chart:ColumnSeries ItemsSource="{Binding Data}"
                       XBindingPath="Month"
                       YBindingPath="Product2"
                       Label="Product B"/>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();
chart.Legend = new ChartLegend();
```

### Legend Visibility

Control legend visibility using the `IsVisible` property.

```xml
<chart:ChartLegend IsVisible="True"/>
```

```csharp
ChartLegend legend = new ChartLegend()
{
    IsVisible = true
};
```

### Toggle Series Visibility

Clicking legend items toggles series visibility.

```xml
<chart:ChartLegend ToggleSeriesVisibility="True"/>
```

```csharp
ChartLegend legend = new ChartLegend()
{
    ToggleSeriesVisibility = true
};
```

## Legend Item Visibility

The visibility of individual legend items for specific series can be controlled using the `IsVisibleOnLegend` property of the series. The default value for `IsVisibleOnLegend` is `true`.

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.Legend>
        <chart:ChartLegend/>
    </chart:SfCartesianChart.Legend>
    
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:LineSeries ItemsSource="{Binding Data}"
                     XBindingPath="Month"
                     YBindingPath="Value"
                     Label="Sales"
                     IsVisibleOnLegend="True"/>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();
chart.Legend = new ChartLegend();

LineSeries series = new LineSeries()
{
    ItemsSource = new ViewModel().Data,
    XBindingPath = "Month",
    YBindingPath = "Value",
    Label = "Sales",
    IsVisibleOnLegend = true
};

chart.Series.Add(series);
this.Content = chart;
```

## Legend Customization

### Legend Label Style

Customize legend labels using the `LabelStyle` property.

```xml
<chart:ChartLegend>
    <chart:ChartLegend.LabelStyle>
        <chart:ChartLegendLabelStyle TextColor="Black"
                                     FontSize="16"
                                     FontFamily="Arial"
                                     FontAttributes="Bold"
                                     Margin="5"/>
    </chart:ChartLegend.LabelStyle>
</chart:ChartLegend>
```

```csharp
ChartLegend legend = new ChartLegend();
legend.LabelStyle = new ChartLegendLabelStyle()
{
    TextColor = Colors.Black,
    FontSize = 16,
    FontFamily = "Arial",
    FontAttributes = FontAttributes.Bold,
    Margin = new Thickness(5)
};
```

### Legend Icon

Customize legend icon type using the series `LegendIcon` property.

```xml
<chart:ColumnSeries ItemsSource="{Binding Data}"
                   XBindingPath="Month"
                   YBindingPath="Value"
                   Label="Sales"
                   LegendIcon="Diamond"/>
```

```csharp
ColumnSeries series = new ColumnSeries()
{
    Label = "Sales",
    LegendIcon = ChartLegendIconType.Diamond
};
```

**Available Icon Types:**
- `Circle`
- `Rectangle`
- `Diamond`
- `HorizontalLine`
- `Pentagon`
- `Triangle`
- `SeriesType` (default - matches series type)

## Legend Placement

### Placement Property

Position the legend using the `Placement` property.

```xml
<chart:ChartLegend Placement="Top"/>
```

```csharp
ChartLegend legend = new ChartLegend()
{
    Placement = LegendPlacement.Top
};
```

**Available Placements:**
- `Top`
- `Bottom`
- `Left`
- `Right`

### Floating Legend

Create a floating legend using the `IsFloating` property with custom positioning.

```xml
<chart:ChartLegend IsFloating="True" 
                  OffsetX="10" 
                  OffsetY="10"/>
```

```csharp
ChartLegend legend = new ChartLegend()
{
    IsFloating = true,
    OffsetX = 10,
    OffsetY = 10
};
```

## Legend Maximum Size Request

To set the maximum size request for the legend view, override the `GetMaximumSizeCoefficient` protected method in `ChartLegend` class. The value should be between 0 and 1, representing the maximum size request, not the desired size for the legend items layout.

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.Legend>
        <local:LegendExt/>
    </chart:SfCartesianChart.Legend>
    
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:ColumnSeries ItemsSource="{Binding Data}"
                       XBindingPath="Month"
                       YBindingPath="Value"
                       Label="Sales"/>
</chart:SfCartesianChart>
```

```csharp
public class LegendExt : ChartLegend
{
    protected override double GetMaximumSizeCoefficient()
    {
        return 0.7;
    }
}

SfCartesianChart chart = new SfCartesianChart();
chart.Legend = new LegendExt();
this.Content = chart;
```

## Legend Item Template

Customize legend items with custom templates.

### ItemTemplate

```xml
<chart:ChartLegend>
    <chart:ChartLegend.ItemTemplate>
        <DataTemplate>
            <StackLayout Orientation="Horizontal" Padding="5">
                <BoxView Color="{Binding IconBrush}"
                         WidthRequest="15"
                         HeightRequest="15"
                         CornerRadius="3"
                         Margin="0,0,5,0"/>
                <Label Text="{Binding Label}"
                       FontSize="14"
                       VerticalTextAlignment="Center"/>
            </StackLayout>
        </DataTemplate>
    </chart:ChartLegend.ItemTemplate>
</chart:ChartLegend>
```

### ItemsLayout

Control legend item arrangement using the `ItemsLayout` property.

```xml
<chart:ChartLegend>
    <chart:ChartLegend.ItemsLayout>
        <FlexLayout Wrap="Wrap" 
                   JustifyContent="Start" 
                   AlignItems="Start"/>
    </chart:ChartLegend.ItemsLayout>
</chart:ChartLegend>
```

## Legend Events

### LegendItemCreated Event

The `LegendItemCreated` event is triggered when the chart legend item is created. The argument contains the `LegendItem` object. The following properties are present in `LegendItem`:

* `Text` – used to get or set the text of the label.
* `TextColor` – used to get or set the color of the label.
* `FontFamily` - used to get or set the font family for the legend label.
* `FontAttributes` - used to get or set the font style for the legend label.
* `FontSize` - used to get or set the font size for the legend label.
* `TextMargin` - used to get or set the margin size of labels.
* `IconBrush` - used to change the color of the legend icon.
* `IconType` - used to get or set the icon type for the legend icon.
* `IconHeight` - used to get or set the icon height of the legend icon.
* `IconWidth` - used to get or set the icon width of the legend icon.
* `IsToggled` - used to get or set the toggle visibility of the legend.
* `DisableBrush` - used to get or set the color of the legend when toggled.
* `Index` - used to get index position of the legend.
* `Item` - used to get the corresponding series for the legend item.

```csharp
ChartLegend legend = new ChartLegend();
legend.LegendItemCreated += Legend_LegendItemCreated;

private void Legend_LegendItemCreated(object sender, LegendItemEventArgs e)
{
    // Customize legend item
    if (e.LegendItem.Text == "Product A")
    {
        e.LegendItem.IconBrush = Colors.Red;
        e.LegendItem.TextColor = Colors.Red;
    }
}
```

## Chart Appearance

### Palette Brushes

Define a palette of colors for multiple series using the `PaletteBrushes` property at the chart level.

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.PaletteBrushes>
        <SolidColorBrush>#4CAF50</SolidColorBrush>
        <SolidColorBrush>#2196F3</SolidColorBrush>
        <SolidColorBrush>#FF9800</SolidColorBrush>
        <SolidColorBrush>#F44336</SolidColorBrush>
        <SolidColorBrush>#9C27B0</SolidColorBrush>
    </chart:SfCartesianChart.PaletteBrushes>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();
chart.PaletteBrushes = new List<Brush>
{
    new SolidColorBrush(Color.FromArgb("#4CAF50")),
    new SolidColorBrush(Color.FromArgb("#2196F3")),
    new SolidColorBrush(Color.FromArgb("#FF9800")),
    new SolidColorBrush(Color.FromArgb("#F44336"))
};
```

### Series-Level Palette

You can also define `PaletteBrushes` at the series level for segment-based coloring.

```xml
<chart:ColumnSeries ItemsSource="{Binding Data}"
                   XBindingPath="Category"
                   YBindingPath="Value">
    <chart:ColumnSeries.PaletteBrushes>
        <SolidColorBrush>#E91E63</SolidColorBrush>
        <SolidColorBrush>#3F51B5</SolidColorBrush>
        <SolidColorBrush>#00BCD4</SolidColorBrush>
    </chart:ColumnSeries.PaletteBrushes>
</chart:ColumnSeries>
```

### Custom Brushes

Apply custom brushes to individual series using the `Fill` property.

```xml
<chart:AreaSeries ItemsSource="{Binding Data}"
                 XBindingPath="Month"
                 YBindingPath="Value"
                 Fill="#80FF6347"/>
```

```csharp
AreaSeries series = new AreaSeries()
{
    Fill = new SolidColorBrush(Color.FromArgb("#80FF6347"))
};
```

### Gradient Application

Apply gradient brushes to series for visually rich charts.

#### Linear Gradient

```xml
<chart:ColumnSeries ItemsSource="{Binding Data}"
                   XBindingPath="Category"
                   YBindingPath="Value">
    <chart:ColumnSeries.Fill>
        <LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
            <GradientStop Color="#FFE5E5" Offset="0.0"/>
            <GradientStop Color="#FF6B6B" Offset="1.0"/>
        </LinearGradientBrush>
    </chart:ColumnSeries.Fill>
</chart:ColumnSeries>
```

```csharp
ColumnSeries series = new ColumnSeries();
LinearGradientBrush gradientBrush = new LinearGradientBrush
{
    StartPoint = new Point(0, 0),
    EndPoint = new Point(0, 1)
};
gradientBrush.GradientStops.Add(new GradientStop(Color.FromArgb("#FFE5E5"), 0.0f));
gradientBrush.GradientStops.Add(new GradientStop(Color.FromArgb("#FF6B6B"), 1.0f));
series.Fill = gradientBrush;
```

#### Radial Gradient

```xml
<chart:ColumnSeries ItemsSource="{Binding Data}"
                   XBindingPath="Category"
                   YBindingPath="Value">
    <chart:ColumnSeries.Fill>
        <RadialGradientBrush Center="0.5,0.5" Radius="0.8">
            <GradientStop Color="#FFFF99" Offset="0.0"/>
            <GradientStop Color="#FF9800" Offset="1.0"/>
        </RadialGradientBrush>
    </chart:ColumnSeries.Fill>
</chart:ColumnSeries>
```

## Complete Examples

### Legend with Custom Styling

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.Legend>
        <chart:ChartLegend Placement="Bottom"
                          ToggleSeriesVisibility="True"
                          Margin="10">
            <chart:ChartLegend.LabelStyle>
                <chart:ChartLegendLabelStyle TextColor="DarkSlateGray"
                                             FontSize="16"
                                             FontAttributes="Bold"/>
            </chart:ChartLegend.LabelStyle>
        </chart:ChartLegend>
    </chart:SfCartesianChart.Legend>
    
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:ColumnSeries ItemsSource="{Binding Data}"
                       XBindingPath="Month"
                       YBindingPath="Product1"
                       Label="Product A"
                       LegendIcon="Diamond"/>
    
    <chart:ColumnSeries ItemsSource="{Binding Data}"
                       XBindingPath="Month"
                       YBindingPath="Product2"
                       Label="Product B"
                       LegendIcon="Circle"/>
</chart:SfCartesianChart>
```

### Gradient Color Chart

```csharp
SfCartesianChart chart = new SfCartesianChart();

ColumnSeries series = new ColumnSeries()
{
    ItemsSource = new ViewModel().Data,
    XBindingPath = "Category",
    YBindingPath = "Value"
};

LinearGradientBrush gradient = new LinearGradientBrush
{
    StartPoint = new Point(0, 0),
    EndPoint = new Point(0, 1)
};
gradient.GradientStops.Add(new GradientStop(Color.FromArgb("#4CAF50"), 0.0f));
gradient.GradientStops.Add(new GradientStop(Color.FromArgb("#2E7D32"), 1.0f));

series.Fill = gradient;
chart.Series.Add(series);
```

## Limitations

* Do not add items explicitly.
* When using BindableLayouts, do not bind ItemsSource explicitly.
* For better UX, arrange items vertically for left and right dock positions, and vice versa for top and bottom dock positions.
