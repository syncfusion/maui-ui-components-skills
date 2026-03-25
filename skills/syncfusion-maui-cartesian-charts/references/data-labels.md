# Data Labels

## Table of Contents
- [Overview](#overview)
- [Basic Data Labels](#basic-data-labels)
- [Label Template](#label-template)
- [Label Positioning](#label-positioning)
- [Label Styling](#label-styling)
- [Data Labels in Release Mode](#data-labels-in-release-mode)
- [Best Practices](#best-practices)

## Overview

Data labels display values of data points directly on the chart, making it easier to read exact values without referring to axes. Syncfusion .NET MAUI Cartesian Chart provides extensive customization options for data labels.

## Basic Data Labels

### Enable Data Labels

```xml
<chart:LineSeries ItemsSource="{Binding Data}"
                 XBindingPath="Month"
                 YBindingPath="Value"
                 ShowDataLabels="True"/>
```

```csharp
LineSeries series = new LineSeries()
{
    ItemsSource = new ViewModel().Data,
    XBindingPath = "Month",
    YBindingPath = "Value",
    ShowDataLabels = true
};
```

### Multiple Series with Labels

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:ColumnSeries ItemsSource="{Binding Data1}"
                        XBindingPath="Category"
                        YBindingPath="Value"
                        ShowDataLabels="True"
                        Label="Series 1"/>
    
    <chart:ColumnSeries ItemsSource="{Binding Data2}"
                        XBindingPath="Category"
                        YBindingPath="Value"
                        ShowDataLabels="True"
                        Label="Series 2"/>
</chart:SfCartesianChart>
```

## Label Template

### Custom Label Template

```xml
<chart:LineSeries ItemsSource="{Binding Data}"
                 XBindingPath="Month"
                 YBindingPath="Value"
                 ShowDataLabels="True">
    <chart:LineSeries.DataLabelSettings>
        <chart:CartesianDataLabelSettings>
            <chart:CartesianDataLabelSettings.LabelTemplate>
                <DataTemplate>
                    <StackLayout Orientation="Horizontal">
                        <Label Text="$" 
                               TextColor="White"
                               FontSize="12"/>
                        <Label Text="{Binding Item.Value, StringFormat='{0:N0}'}"
                               TextColor="White"
                               FontSize="12"
                               FontAttributes="Bold"/>
                    </StackLayout>
                </DataTemplate>
            </chart:CartesianDataLabelSettings.LabelTemplate>
        </chart:CartesianDataLabelSettings>
    </chart:LineSeries.DataLabelSettings>
</chart:LineSeries>
```

### Advanced Template with Icon

```xml
<chart:ColumnSeries ItemsSource="{Binding Data}"
                    XBindingPath="Category"
                    YBindingPath="Value"
                    ShowDataLabels="True">
    <chart:ColumnSeries.DataLabelSettings>
        <chart:CartesianDataLabelSettings>
            <chart:CartesianDataLabelSettings.LabelTemplate>
                <DataTemplate>
                    <Grid>
                        <Grid.RowDefinitions>
                            <RowDefinition Height="Auto"/>
                            <RowDefinition Height="Auto"/>
                        </Grid.RowDefinitions>
                        
                        <Image Source="icon.png"
                               HeightRequest="16"
                               WidthRequest="16"
                               Grid.Row="0"/>
                        
                        <Label Text="{Binding Item.Value}"
                               TextColor="Black"
                               FontSize="11"
                               Grid.Row="1"
                               HorizontalTextAlignment="Center"/>
                    </Grid>
                </DataTemplate>
            </chart:CartesianDataLabelSettings.LabelTemplate>
        </chart:CartesianDataLabelSettings>
    </chart:ColumnSeries.DataLabelSettings>
</chart:ColumnSeries>
```

## Label Positioning

### Label Position

```xml
<chart:ColumnSeries ItemsSource="{Binding Data}"
                    XBindingPath="Category"
                    YBindingPath="Value"
                    ShowDataLabels="True">
    <chart:ColumnSeries.DataLabelSettings>
        <chart:CartesianDataLabelSettings LabelPlacement="Top"/>
    </chart:ColumnSeries.DataLabelSettings>
</chart:ColumnSeries>
```

```csharp
CartesianDataLabelSettings labelSettings = new CartesianDataLabelSettings()
{
    LabelPlacement = DataLabelPlacement.Top
};

ColumnSeries series = new ColumnSeries()
{
    ItemsSource = new ViewModel().Data,
    XBindingPath = "Category",
    YBindingPath = "Value",
    ShowDataLabels = true,
    DataLabelSettings = labelSettings
};
```

**Placement Options:**
- `Auto`: Automatic positioning (default)
- `Inner`: Inside the data point
- `Outer`: Outside the data point
- `Top`: Above the data point
- `Bottom`: Below the data point
- `Center`: Centered on data point

### Label Alignment

```xml
<chart:LineSeries ItemsSource="{Binding Data}"
                 XBindingPath="Month"
                 YBindingPath="Value"
                 ShowDataLabels="True">
    <chart:LineSeries.DataLabelSettings>
        <chart:CartesianDataLabelSettings LabelPlacement="Top"
                                          OffsetX="5"
                                          OffsetY="-10"/>
    </chart:LineSeries.DataLabelSettings>
</chart:LineSeries>
```

```csharp
CartesianDataLabelSettings labelSettings = new CartesianDataLabelSettings()
{
    LabelPlacement = DataLabelPlacement.Top,
    OffsetX = 5,
    OffsetY = -10
};
```

### Column Chart Label Positioning

The alignment of data labels inside the series is defined by using the BarAlignment property. 

```xml
<chart:ColumnSeries ItemsSource="{Binding Data}"
                 XBindingPath="Category"
                 YBindingPath="Value"
                 ShowDataLabels="True">
    <chart:ColumnSeries.DataLabelSettings>
        <chart:CartesianDataLabelSettings LabelPlacement="Inner"
                                          BarAlignment="Middle"/>
    </chart:ColumnSeries.DataLabelSettings>
</chart:ColumnSeries>
```
* **Top** - Positions the data label at the top edge point of a chart segment.
* **Middle** - Positions the data label at the center point of a chart segment.
* **Bottom** - Positions the data label at the bottom edge point of a chart segment.

**Note**: This behavior varies based on the chart series type.

## Label Styling

### Basic Styling

```xml
<chart:ColumnSeries ItemsSource="{Binding Data}"
                    XBindingPath="Category"
                    YBindingPath="Value"
                    ShowDataLabels="True">
    <chart:ColumnSeries.DataLabelSettings>
        <chart:CartesianDataLabelSettings LabelStyle="{StaticResource labelStyle}"/>
    </chart:ColumnSeries.DataLabelSettings>
</chart:ColumnSeries>
```

Define style in resources:

```xml
<chart:SfCartesianChart.Resources>
    <chart:ChartDataLabelStyle x:Key="labelStyle"
                               TextColor="White"
                               FontSize="14"
                               FontAttributes="Bold"
                               Background="#4CAF50"
                               Margin="3"
                               CornerRadius="4"/>
</chart:SfCartesianChart.Resources>
```

### Complete Label Styling

```xml
<chart:LineSeries ItemsSource="{Binding Data}"
                 XBindingPath="Month"
                 YBindingPath="Value"
                 ShowDataLabels="True">
    <chart:LineSeries.DataLabelSettings>
        <chart:CartesianDataLabelSettings>
            <chart:CartesianDataLabelSettings.LabelStyle>
                <chart:ChartDataLabelStyle TextColor="Blue"
                                           FontSize="12"
                                           FontAttributes="Italic"
                                           FontFamily="Arial"
                                           Background="LightBlue"
                                           Stroke="DarkBlue"
                                           StrokeWidth="1"
                                           Margin="5"
                                           CornerRadius="5"/>
            </chart:CartesianDataLabelSettings.LabelStyle>
        </chart:CartesianDataLabelSettings>
    </chart:LineSeries.DataLabelSettings>
</chart:LineSeries>
```

```csharp
ChartDataLabelStyle labelStyle = new ChartDataLabelStyle()
{
    TextColor = Colors.Blue,
    FontSize = 12,
    FontAttributes = FontAttributes.Italic,
    FontFamily = "Arial",
    Background = new SolidColorBrush(Colors.LightBlue),
    Stroke = new SolidColorBrush(Colors.DarkBlue),
    StrokeWidth = 1,
    Margin = new Thickness(5),
    CornerRadius = new CornerRadius(5)
};

CartesianDataLabelSettings labelSettings = new CartesianDataLabelSettings()
{
    LabelStyle = labelStyle
};

LineSeries series = new LineSeries()
{
    ItemsSource = new ViewModel().Data,
    XBindingPath = "Month",
    YBindingPath = "Value",
    ShowDataLabels = true,
    DataLabelSettings = labelSettings
};
```

### Connector Lines

For pie-like arrangements or when labels are far from points:

```xml
<chart:LineSeries ItemsSource="{Binding Data}"
                 XBindingPath="Month"
                 YBindingPath="Value"
                 ShowDataLabels="True">
    <chart:LineSeries.DataLabelSettings>
        <chart:CartesianDataLabelSettings ShowConnectorLine="True"
                                          ConnectorLineStyle="{StaticResource connectorStyle}"/>
    </chart:LineSeries.DataLabelSettings>
</chart:LineSeries>
```
## Best Practices

### When to Use Data Labels

**Use data labels when:**
- Exact values are important
- Chart has few data points (<20)
- Comparing specific values
- Presentation or reporting scenarios

**Avoid data labels when:**
- Chart has many data points (>30)
- Trend is more important than values
- Labels would overlap
- Interactive tooltips are available

### Label Positioning Guidelines

1. **Column/Bar Charts**: Place labels on top (column) or end (bar)
2. **Line Charts**: Place above or below points
3. **Area Charts**: Place at peaks or use templates
4. **Stacked Charts**: Place inside segments with sufficient space

### Formatting Best Practices

1. **Consistency**: Use same format across all series
2. **Precision**: Show necessary decimals only
3. **Units**: Include units when needed ($, %, kg, etc.)
4. **Abbreviation**: Use K, M, B for large numbers

### Styling Guidelines

1. **Contrast**: Ensure good contrast with background
2. **Size**: 10-14pt for most cases
3. **Background**: Use subtle backgrounds to improve readability
4. **Borders**: Optional; use for emphasis

### Performance Optimization

1. Limit data points when using labels
2. Avoid complex templates for large datasets
3. Use simple text over images when possible
4. Disable labels for drill-down/detail views

### Common Use Cases

**Financial Data:**
```xml
<chart:ColumnSeries ShowDataLabels="True"
                    LabelFormat="C0">
    <chart:ColumnSeries.DataLabelSettings>
        <chart:CartesianDataLabelSettings LabelPlacement="Top">
            <chart:CartesianDataLabelSettings.LabelStyle>
                <chart:ChartDataLabelStyle TextColor="Green"
                                           FontSize="12"
                                           FontAttributes="Bold"/>
            </chart:CartesianDataLabelSettings.LabelStyle>
        </chart:CartesianDataLabelSettings>
    </chart:ColumnSeries.DataLabelSettings>
</chart:ColumnSeries>
```

**Percentage Data:**
```xml
<chart:LineSeries ShowDataLabels="True"
                 LabelFormat="P1">
    <chart:LineSeries.DataLabelSettings>
        <chart:CartesianDataLabelSettings LabelPlacement="Top"
                                          OffsetY="-5"/>
    </chart:LineSeries.DataLabelSettings>
</chart:LineSeries>
```

**Abbreviated Large Numbers:**
```xml
<chart:ColumnSeries ShowDataLabels="True">
    <chart:ColumnSeries.DataLabelSettings>
        <chart:CartesianDataLabelSettings>
            <chart:CartesianDataLabelSettings.LabelTemplate>
                <DataTemplate>
                    <Label>
                        <Label.Text>
                            <MultiBinding StringFormat="{}{0:0.0}K">
                                <Binding Path="Item.Value" 
                                         Converter="{StaticResource DivideByThousandConverter}"/>
                            </MultiBinding>
                        </Label.Text>
                    </Label>
                </DataTemplate>
            </chart:CartesianDataLabelSettings.LabelTemplate>
        </chart:CartesianDataLabelSettings>
    </chart:ColumnSeries.DataLabelSettings>
</chart:ColumnSeries>
```

### Troubleshooting

**Issue**: Labels overlapping
- **Solution**: Reduce data points, rotate labels, or use smaller font

**Issue**: Labels cut off at chart edges
- **Solution**: Add chart margin or use PlotOffset on axes

**Issue**: Labels not showing in release mode
- **Solution**: Add [Preserve] attributes and configure trimming

**Issue**: Custom template not binding
- **Solution**: Verify binding path and data context

**Issue**: Labels obscuring data points
- **Solution**: Adjust OffsetY/OffsetX or change LabelPlacement

**Issue**: Performance issues with many labels
- **Solution**: Reduce data points or disable labels for dense data
