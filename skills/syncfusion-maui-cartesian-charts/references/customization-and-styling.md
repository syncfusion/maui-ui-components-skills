# Customization and Styling

## Table of Contents
- [Overview](#overview)
- [Series Styling](#series-styling)
- [Border and Corner Radius](#border-and-corner-radius)
- [Plot Area Customization](#plot-area-customization)
- [Common Styling Patterns](#common-styling-patterns)

## Overview

Syncfusion .NET MAUI Cartesian Chart provides extensive customization options to style charts according to your application's design requirements. You can apply custom styling to series, borders, plot area backgrounds, and more to create visually appealing and branded charts.

## Series Styling

### Fill and Stroke

Customize individual series appearance:

```xml
<chart:LineSeries ItemsSource="{Binding Data}"
                 XBindingPath="Month"
                 YBindingPath="Sales"
                 Fill="Transparent"
                 Stroke="#2196F3"
                 StrokeWidth="3"/>
```

```csharp
LineSeries series = new LineSeries
{
    ItemsSource = data,
    XBindingPath = "Month",
    YBindingPath = "Sales",
    Fill = Colors.Transparent,
    Stroke = new SolidColorBrush(Color.FromRgb(33, 150, 243)),
    StrokeWidth = 3
};
```

### Opacity

Control transparency:

```xml
<chart:AreaSeries ItemsSource="{Binding Data}"
                 XBindingPath="Month"
                 YBindingPath="Value"
                 Fill="#4CAF50"
                 Opacity="0.6"/>
```

```csharp
AreaSeries series = new AreaSeries
{
    ItemsSource = data,
    XBindingPath = "Month",
    YBindingPath = "Value",
    Fill = new SolidColorBrush(Color.FromRgb(76, 175, 80)),
    Opacity = 0.6
};
```
## Border and Corner Radius

### Series Borders

Add borders to series segments:

```xml
<chart:ColumnSeries ItemsSource="{Binding Data}"
                   XBindingPath="Category"
                   YBindingPath="Value"
                   Fill="#9C27B0"
                   Stroke="#7B1FA2"
                   StrokeWidth="2"/>
```

```csharp
ColumnSeries series = new ColumnSeries
{
    ItemsSource = data,
    XBindingPath = "Category",
    YBindingPath = "Value",
    Fill = new SolidColorBrush(Color.FromRgb(156, 39, 176)),
    Stroke = new SolidColorBrush(Color.FromRgb(123, 31, 162)),
    StrokeWidth = 2
};
```

### Corner Radius for Columns

```xml
<chart:ColumnSeries ItemsSource="{Binding Data}"
                   XBindingPath="Category"
                   YBindingPath="Value"
                   CornerRadius="10,10,0,0"/>
```

```csharp
ColumnSeries series = new ColumnSeries
{
    ItemsSource = data,
    XBindingPath = "Category",
    YBindingPath = "Value",
    CornerRadius = new CornerRadius(10, 10, 0, 0)  // Top-left, Top-right, Bottom-right, Bottom-left
};
```

## Plot Area Customization

The `PlotAreaBackgroundView` property allows you to add custom backgrounds, watermarks, or any visual element to the chart's plot area (the area where data is rendered).

### Add Background Image

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.PlotAreaBackgroundView>
        <Image Source="background.png" 
               Aspect="AspectFill"
               Opacity="0.3"/>
    </chart:SfCartesianChart.PlotAreaBackgroundView>
    
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

```csharp
SfCartesianChart chart = new SfCartesianChart();

Image backgroundImage = new Image
{
    Source = "background.png",
    Aspect = Aspect.AspectFill,
    Opacity = 0.3
};

chart.PlotAreaBackgroundView = backgroundImage;
```

### Add Watermark

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.PlotAreaBackgroundView>
        <Label Text="CONFIDENTIAL"
               FontSize="48"
               TextColor="LightGray"
               Opacity="0.2"
               Rotation="-45"
               HorizontalOptions="Center"
               VerticalOptions="Center"/>
    </chart:SfCartesianChart.PlotAreaBackgroundView>
    
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

```csharp
SfCartesianChart chart = new SfCartesianChart();

Label watermark = new Label
{
    Text = "CONFIDENTIAL",
    FontSize = 48,
    TextColor = Colors.LightGray,
    Opacity = 0.2,
    Rotation = -45,
    HorizontalOptions = LayoutOptions.Center,
    VerticalOptions = LayoutOptions.Center
};

chart.PlotAreaBackgroundView = watermark;
```

### Custom Background with Gradient

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.PlotAreaBackgroundView>
        <BoxView>
            <BoxView.Background>
                <LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
                    <GradientStop Color="#E3F2FD" Offset="0.0"/>
                    <GradientStop Color="White" Offset="1.0"/>
                </LinearGradientBrush>
            </BoxView.Background>
        </BoxView>
    </chart:SfCartesianChart.PlotAreaBackgroundView>
    
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:AreaSeries ItemsSource="{Binding Data}"
                     XBindingPath="Month"
                     YBindingPath="Value"
                     Fill="#2196F3"
                     Opacity="0.6"/>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();

LinearGradientBrush gradientBrush = new LinearGradientBrush
{
    StartPoint = new Point(0, 0),
    EndPoint = new Point(0, 1)
};
gradientBrush.GradientStops.Add(new GradientStop(Color.FromArgb("#E3F2FD"), 0.0f));
gradientBrush.GradientStops.Add(new GradientStop(Colors.White, 1.0f));

BoxView background = new BoxView
{
    Background = gradientBrush
};

chart.PlotAreaBackgroundView = background;
```

### Complex Custom View

You can add any .NET MAUI view to the plot area background:

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.PlotAreaBackgroundView>
        <Grid>
            <BoxView Color="#F5F5F5"/>
            <Image Source="logo.png" 
                   Aspect="AspectFit"
                   Opacity="0.1"
                   HorizontalOptions="Center"
                   VerticalOptions="Center"
                   WidthRequest="200"
                   HeightRequest="200"/>
        </Grid>
    </chart:SfCartesianChart.PlotAreaBackgroundView>
    
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:ColumnSeries ItemsSource="{Binding Data}"
                       XBindingPath="Quarter"
                       YBindingPath="Revenue"
                       Label="Quarterly Revenue"/>
</chart:SfCartesianChart>
```

## Common Styling Patterns

### Pattern 1: Rounded Column Chart

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
                       YBindingPath="Value"
                       Fill="#2196F3"
                       CornerRadius="8,8,0,0"
                       Stroke="#1976D2"
                       StrokeWidth="2"/>
</chart:SfCartesianChart>
```

### Pattern 2: Dark Mode Chart

```xml
<chart:SfCartesianChart BackgroundColor="#1e1e1e">
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis>
            <chart:CategoryAxis.LabelStyle>
                <chart:ChartAxisLabelStyle TextColor="White"/>
            </chart:CategoryAxis.LabelStyle>
            <chart:CategoryAxis.AxisLineStyle>
                <chart:ChartLineStyle Stroke="Gray"/>
            </chart:CategoryAxis.AxisLineStyle>
        </chart:CategoryAxis>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis>
            <chart:NumericalAxis.LabelStyle>
                <chart:ChartAxisLabelStyle TextColor="White"/>
            </chart:NumericalAxis.LabelStyle>
            <chart:NumericalAxis.AxisLineStyle>
                <chart:ChartLineStyle Stroke="Gray"/>
            </chart:NumericalAxis.AxisLineStyle>
            <chart:NumericalAxis.MajorGridLineStyle>
                <chart:ChartLineStyle Stroke="#2d2d2d"/>
            </chart:NumericalAxis.MajorGridLineStyle>
        </chart:NumericalAxis>
    </chart:SfCartesianChart.YAxes>
    
    <chart:LineSeries ItemsSource="{Binding Data}"
                     XBindingPath="Month"
                     YBindingPath="Value"
                     Stroke="#61DAFB"
                     StrokeWidth="3"
                     ShowMarkers="True">
        <chart:LineSeries.MarkerSettings>
            <chart:ChartMarkerSettings Fill="#61DAFB"
                                      Stroke="White"
                                      StrokeWidth="2"
                                      Width="10"
                                      Height="10"/>
        </chart:LineSeries.MarkerSettings>
    </chart:LineSeries>
</chart:SfCartesianChart>
```

### Pattern 3: Chart with Watermark

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.PlotAreaBackgroundView>
        <Grid>
            <Image Source="company_logo.png" 
                   Aspect="AspectFit"
                   Opacity="0.15"
                   HorizontalOptions="Center"
                   VerticalOptions="Center"
                   WidthRequest="150"/>
        </Grid>
    </chart:SfCartesianChart.PlotAreaBackgroundView>
    
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:ColumnSeries ItemsSource="{Binding Data}"
                       XBindingPath="Category"
                       YBindingPath="Value"
                       Fill="#4CAF50"/>
</chart:SfCartesianChart>
```

### Pattern 4: Multi-Series with Distinct Styles

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
    
    <!-- Actual values as columns -->
    <chart:ColumnSeries ItemsSource="{Binding ActualData}"
                       XBindingPath="Month"
                       YBindingPath="Value"
                       Label="Actual"
                       Fill="#2196F3"
                       CornerRadius="5,5,0,0"/>
    
    <!-- Target as dashed line -->
    <chart:LineSeries ItemsSource="{Binding TargetData}"
                     XBindingPath="Month"
                     YBindingPath="Value"
                     Label="Target"
                     Stroke="#FF5722"
                     StrokeWidth="2"
                     StrokeDashArray="5,3"/>
</chart:SfCartesianChart>
```

## Tips and Best Practices

1. **Corner Radius**: Use corner radius on columns and bars for modern, polished appearance
2. **Border Contrast**: Add borders to series for better definition, especially on light backgrounds
3. **Plot Area Backgrounds**: Keep plot area backgrounds subtle (low opacity) to avoid obscuring data
4. **Watermark Placement**: Position watermarks centrally and rotate them to minimize data obstruction
5. **Dark Mode Support**: Adjust stroke colors, borders, and background elements appropriately for dark mode
6. **Consistent Styling**: Maintain consistent stroke widths and corner radius values across series
7. **Test on Devices**: Test styling appearance on actual devices as rendering may vary
8. **Performance**: Complex plot area backgrounds can impact performance on lower-end devices

## Common Gotchas

### Corner Radius and Series Types

- Corner radius only applies to column, bar, and similar rectangular series
- Not supported on line, area, or point-based series
- Order matters: Top-left, Top-right, Bottom-right, Bottom-left

### Plot Area Background Layers

- Plot area background renders behind chart series
- Z-order cannot be changed - background is always behind data
- Complex views (with many children) can impact rendering performance

### Stroke Width Scaling

- Stroke width does not scale with chart size automatically
- May need to adjust stroke width for different screen sizes
- Consider using responsive values for different device form factors

### Opacity and Visibility

When using low opacity for backgrounds or series:
- May create unintended color blending effects
- Can reduce contrast and readability
- Test with actual data patterns to ensure clarity
