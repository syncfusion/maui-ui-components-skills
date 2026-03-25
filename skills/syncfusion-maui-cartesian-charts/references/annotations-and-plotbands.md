# Annotations and PlotBands

## Table of Contents
- [Overview](#overview)
- [Annotations](#annotations)
  - [Adding Annotations](#adding-annotations)
  - [Positioning Annotations](#positioning-annotations)
  - [Text Annotation](#text-annotation)
  - [Shape Annotations](#shape-annotations)
  - [View Annotation](#view-annotation)
  - [Multiple Axes Support](#multiple-axes-support)
- [PlotBands](#plotbands)
  - [Numerical PlotBand](#numerical-plotband)
  - [DateTime PlotBand](#datetime-plotband)
  - [Recursive PlotBand](#recursive-plotband)
  - [Segmented PlotBand](#segmented-plotband)
  - [Plot Lines](#plot-lines)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

Annotations and PlotBands allow you to mark and highlight specific areas of interest in the chart. Annotations add text, shapes, or custom views to specific points, while PlotBands shade regions in the background at regular or custom intervals.

**Annotation Types:**
- Text annotations
- Shape annotations (Rectangle, Ellipse, Line, Vertical Line, Horizontal Line)
- View annotations (custom views)

**PlotBand Types:**
- Numerical PlotBands
- DateTime PlotBands

## Annotations

### Adding Annotations

Add annotations to the chart's `Annotations` collection:

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:SfCartesianChart.Annotations>
        <chart:EllipseAnnotation X1="2" X2="4" Y1="10" Y2="15" 
                                Text="Ellipse"
                                Fill="#4CAF50"
                                Stroke="Green"
                                StrokeWidth="2"/>
    </chart:SfCartesianChart.Annotations>
    
    <chart:LineSeries ItemsSource="{Binding Data}"
                     XBindingPath="Category"
                     YBindingPath="Value"/>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();

var ellipse = new EllipseAnnotation()
{
    X1 = 2,
    Y1 = 10,
    X2 = 4,
    Y2 = 15,
    Text = "Ellipse",
    Fill = new SolidColorBrush(Color.FromArgb("#4CAF50")),
    Stroke = new SolidColorBrush(Colors.Green),
    StrokeWidth = 2
};

chart.Annotations.Add(ellipse);
```

### Positioning Annotations

**Coordinate Units:**
- `Axis` (default): Position based on axis values
- `Pixel`: Position based on pixel coordinates

```xml
<chart:RectangleAnnotation X1="0" Y1="100" X2="300" Y2="400" 
                          CoordinateUnit="Pixel"
                          Text="Pixel Position"/>
```

```csharp
var rectangle = new RectangleAnnotation()
{
    X1 = 0,
    Y1 = 100,
    X2 = 300,
    Y2 = 400,
    CoordinateUnit = ChartCoordinateUnit.Pixel,
    Text = "Pixel Position"
};
```

### Text Annotation

Add simple text at specific points:

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.Annotations>
        <chart:TextAnnotation X1="2" Y1="25" 
                             Text="Important Point">
            <chart:TextAnnotation.LabelStyle>
                <chart:ChartAnnotationLabelStyle TextColor="Blue"
                                                 FontSize="16"
                                                 FontAttributes="Bold"
                                                 Background="LightYellow"/>
            </chart:TextAnnotation.LabelStyle>
        </chart:TextAnnotation>
    </chart:SfCartesianChart.Annotations>
</chart:SfCartesianChart>
```

```csharp
var textAnnotation = new TextAnnotation()
{
    X1 = 2,
    Y1 = 25,
    Text = "Important Point",
    LabelStyle = new ChartAnnotationLabelStyle()
    {
        TextColor = Colors.Blue,
        FontSize = 16,
        FontAttributes = FontAttributes.Bold,
        Background = new SolidColorBrush(Colors.LightYellow)
    }
};
```

### Shape Annotations

#### Rectangle Annotation

```xml
<chart:RectangleAnnotation X1="1" Y1="40" X2="2" Y2="20"
                          Fill="#20FF5722"
                          Stroke="#FF5722"
                          StrokeWidth="2"
                          Text="Target Range"/>
```

```csharp
var rectangle = new RectangleAnnotation()
{
    X1 = 1,
    Y1 = 40,
    X2 = 2,
    Y2 = 20,
    Fill = new SolidColorBrush(Color.FromArgb("#20FF5722")),
    Stroke = new SolidColorBrush(Color.FromArgb("#FF5722")),
    StrokeWidth = 2,
    Text = "Target Range"
};
```

#### Ellipse Annotation

```xml
<chart:EllipseAnnotation X1="2" X2="4" Y1="10" Y2="15"
                        Width="20" Height="20"
                        Fill="#204CAF50"
                        Stroke="Green"
                        StrokeWidth="2"
                        Text="Focus Area"/>
```

```csharp
var ellipse = new EllipseAnnotation()
{
    X1 = 2,
    Y1 = 10,
    X2 = 4,
    Y2 = 15,
    Width = 20,
    Height = 20,
    Fill = new SolidColorBrush(Color.FromArgb("#204CAF50")),
    Stroke = new SolidColorBrush(Colors.Green),
    StrokeWidth = 2,
    Text = "Focus Area"
};
```

#### Line Annotation

```xml
<chart:LineAnnotation X1="0.5" Y1="10" X2="3.5" Y2="20"
                     Stroke="Red"
                     StrokeWidth="2"
                     StrokeDashArray="5,5"
                     Text="Trend Line"/>
```

```csharp
var line = new LineAnnotation()
{
    X1 = 0.5,
    Y1 = 10,
    X2 = 3.5,
    Y2 = 20,
    Stroke = new SolidColorBrush(Colors.Red),
    StrokeWidth = 2,
    Text = "Trend Line"
};
line.StrokeDashArray = new DoubleCollection { 5, 5 };
```

#### Vertical and Horizontal Line Annotations

```xml
<chart:SfCartesianChart.Annotations>
    <chart:VerticalLineAnnotation X1="2.5"
                                 Stroke="Blue"
                                 StrokeWidth="2"
                                 ShowAxisLabel="True"
                                 LineCap="Arrow">
        <chart:VerticalLineAnnotation.AxisLabelStyle>
            <chart:ChartLabelStyle TextColor="Blue"
                                  FontSize="12"
                                  Background="LightBlue"/>
        </chart:VerticalLineAnnotation.AxisLabelStyle>
    </chart:VerticalLineAnnotation>
    
    <chart:HorizontalLineAnnotation Y1="25"
                                   Stroke="Red"
                                   StrokeWidth="2"
                                   ShowAxisLabel="True"
                                   LineCap="Arrow"/>
</chart:SfCartesianChart.Annotations>
```

```csharp
var verticalLine = new VerticalLineAnnotation()
{
    X1 = 2.5,
    Stroke = new SolidColorBrush(Colors.Blue),
    StrokeWidth = 2,
    ShowAxisLabel = true,
    LineCap = ChartLineCap.Arrow,
    AxisLabelStyle = new ChartLabelStyle()
    {
        TextColor = Colors.Blue,
        FontSize = 12,
        Background = new SolidColorBrush(Colors.LightBlue)
    }
};

var horizontalLine = new HorizontalLineAnnotation()
{
    Y1 = 25,
    Stroke = new SolidColorBrush(Colors.Red),
    StrokeWidth = 2,
    ShowAxisLabel = true,
    LineCap = ChartLineCap.Arrow
};
```

### View Annotation

Add custom views as annotations:

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.Annotations>
        <chart:ViewAnnotation X1="3" Y1="30"
                             VerticalAlignment="Center"
                             HorizontalAlignment="Center">
            <chart:ViewAnnotation.View>
                <Border BackgroundColor="White"
                       Stroke="Orange"
                       StrokeThickness="2"
                       Padding="10">
                    <VerticalStackLayout>
                        <Image Source="warning.png" 
                              WidthRequest="24" 
                              HeightRequest="24"/>
                        <Label Text="Warning!" 
                              TextColor="Orange"
                              FontSize="12"
                              FontAttributes="Bold"/>
                    </VerticalStackLayout>
                </Border>
            </chart:ViewAnnotation.View>
        </chart:ViewAnnotation>
    </chart:SfCartesianChart.Annotations>
</chart:SfCartesianChart>
```

```csharp
var viewAnnotation = new ViewAnnotation()
{
    X1 = 3,
    Y1 = 30,
    VerticalAlignment = ChartAlignment.Center,
    HorizontalAlignment = ChartAlignment.Center,
    View = new Border()
    {
        BackgroundColor = Colors.White,
        Stroke = new SolidColorBrush(Colors.Orange),
        StrokeThickness = 2,
        Padding = 10,
        Content = new VerticalStackLayout()
        {
            Children =
            {
                new Image() { Source = "warning.png", WidthRequest = 24, HeightRequest = 24 },
                new Label() { Text = "Warning!", TextColor = Colors.Orange, FontSize = 12, FontAttributes = FontAttributes.Bold }
            }
        }
    }
};
```

### Multiple Axes Support

Specify which axis to use for positioning:

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
        <chart:NumericalAxis Name="SecondaryAxis" 
                           CrossesAt="{Static x:Double.MaxValue}"/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:SfCartesianChart.Annotations>
        <chart:EllipseAnnotation X1="2" X2="4" Y1="10" Y2="15"
                                YAxisName="SecondaryAxis"
                                Text="On Secondary Axis"/>
    </chart:SfCartesianChart.Annotations>
</chart:SfCartesianChart>
```

## PlotBands

PlotBands shade specific regions in the plot area background.

### Numerical PlotBand

For NumericalAxis, CategoryAxis, and DateTimeCategoryAxis:

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis>
            <chart:NumericalAxis.PlotBands>
                <chart:NumericalPlotBandCollection>
                    <chart:NumericalPlotBand Start="24" End="28"
                                            Fill="#20FFA500"
                                            Stroke="Orange"
                                            StrokeWidth="1"
                                            Text="Target Range">
                        <chart:NumericalPlotBand.LabelStyle>
                            <chart:ChartPlotBandLabelStyle TextColor="Orange"
                                                          FontSize="12"/>
                        </chart:NumericalPlotBand.LabelStyle>
                    </chart:NumericalPlotBand>
                </chart:NumericalPlotBandCollection>
            </chart:NumericalAxis.PlotBands>
        </chart:NumericalAxis>
    </chart:SfCartesianChart.YAxes>
</chart:SfCartesianChart>
```

```csharp
NumericalAxis numericalAxis = new NumericalAxis();
NumericalPlotBandCollection plotBands = new NumericalPlotBandCollection();

NumericalPlotBand plotBand = new NumericalPlotBand
{
    Start = 24,
    End = 28,
    Fill = new SolidColorBrush(Color.FromArgb("#20FFA500")),
    Stroke = new SolidColorBrush(Colors.Orange),
    StrokeWidth = 1,
    Text = "Target Range",
    LabelStyle = new ChartPlotBandLabelStyle()
    {
        TextColor = Colors.Orange,
        FontSize = 12
    }
};

plotBands.Add(plotBand);
numericalAxis.PlotBands = plotBands;
```

### DateTime PlotBand

For DateTimeAxis:

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:DateTimeAxis>
            <chart:DateTimeAxis.PlotBands>
                <chart:DateTimePlotBandCollection>
                    <chart:DateTimePlotBand Start="2023-04-01"
                                           End="2023-06-01"
                                           Fill="#20FF5722"
                                           Text="Q2 2023"/>
                </chart:DateTimePlotBandCollection>
            </chart:DateTimeAxis.PlotBands>
        </chart:DateTimeAxis>
    </chart:SfCartesianChart.XAxes>
</chart:SfCartesianChart>
```

```csharp
DateTimeAxis dateTimeAxis = new DateTimeAxis();
DateTimePlotBandCollection plotBands = new DateTimePlotBandCollection();

DateTimePlotBand plotBand = new DateTimePlotBand
{
    Start = new DateTime(2023, 04, 01),
    End = new DateTime(2023, 06, 01),
    Fill = new SolidColorBrush(Color.FromArgb("#20FF5722")),
    Text = "Q2 2023"
};

plotBands.Add(plotBand);
dateTimeAxis.PlotBands = plotBands;
```

### Recursive PlotBand

Draw plot bands at regular intervals:

```xml
<chart:NumericalAxis>
    <chart:NumericalAxis.PlotBands>
        <chart:NumericalPlotBandCollection>
            <chart:NumericalPlotBand Start="20" End="22"
                                    IsRepeatable="True"
                                    RepeatEvery="4"
                                    RepeatUntil="32"
                                    Fill="LightGray"/>
        </chart:NumericalPlotBandCollection>
    </chart:NumericalAxis.PlotBands>
</chart:NumericalAxis>
```

```csharp
NumericalPlotBand plotBand = new NumericalPlotBand
{
    Start = 20,
    End = 22,
    IsRepeatable = true,
    RepeatEvery = 4,
    RepeatUntil = 32,
    Fill = new SolidColorBrush(Colors.LightGray)
};
```

### Segmented PlotBand

Limit plot band to specific associated axis range:

```xml
<chart:NumericalAxis>
    <chart:NumericalAxis.PlotBands>
        <chart:NumericalPlotBandCollection>
            <chart:NumericalPlotBand Start="20" End="22"
                                    AssociatedAxisStart="0"
                                    AssociatedAxisEnd="2"
                                    Fill="#B300E190"
                                    Text="Low"/>
            
            <chart:NumericalPlotBand Start="25" End="27"
                                    AssociatedAxisStart="4.3"
                                    AssociatedAxisEnd="6.8"
                                    Fill="#B3FCD404"
                                    Text="Average"/>
        </chart:NumericalPlotBandCollection>
    </chart:NumericalAxis.PlotBands>
</chart:NumericalAxis>
```

### Plot Lines

Create plot lines by setting Start = End:

```xml
<chart:NumericalPlotBand Start="24" End="24"
                        Stroke="Red"
                        StrokeWidth="2"
                        Text="Threshold"/>
```

## Best Practices

### Annotation Usage

1. **Clear Purpose**: Use annotations to highlight important data
2. **Avoid Clutter**: Don't overuse annotations
3. **Consistent Styling**: Maintain visual consistency
4. **Meaningful Text**: Provide descriptive annotation text
5. **Coordinate Units**: Choose appropriate coordinate units for positioning

### PlotBand Guidelines

1. **Transparency**: Use semi-transparent fills for better readability
2. **Meaningful Ranges**: Highlight significant data ranges
3. **Text Labels**: Add descriptive text to explain plot bands
4. **Color Contrast**: Ensure plot bands are visible against background
5. **Recursive Bands**: Use for periodic patterns in data

### Common Patterns

**Threshold Marking:**
```xml
<chart:HorizontalLineAnnotation Y1="100"
                               Stroke="Red"
                               StrokeWidth="2"
                               ShowAxisLabel="True"
                               Text="Maximum Threshold"/>
```

**Region Highlighting:**
```xml
<chart:NumericalPlotBand Start="80" End="100"
                        Fill="#2000E190"
                        Text="Optimal Range"/>
```

## Troubleshooting

**Issue**: Annotation not visible
- **Solution**: Check X1, Y1 coordinates are within axis range
- **Solution**: Verify `IsVisible` property is true
- **Solution**: Ensure annotation is added to Annotations collection

**Issue**: PlotBand not showing
- **Solution**: Verify Start/End values are within axis range
- **Solution**: Check Fill has opacity (alpha channel)
- **Solution**: Ensure PlotBands added to correct axis

**Issue**: Annotation positioned incorrectly
- **Solution**: Verify `CoordinateUnit` setting (Axis vs Pixel)
- **Solution**: Check axis names for multi-axis charts
- **Solution**: Ensure axis visible range includes annotation coordinates

**Issue**: Text not readable in annotation
- **Solution**: Adjust LabelStyle TextColor for contrast
- **Solution**: Add Background color to label
- **Solution**: Increase FontSize for visibility

**Issue**: PlotBand text cut off
- **Solution**: Use shorter text or adjust LabelStyle properties
- **Solution**: Consider using OffsetX/OffsetY for positioning
- **Solution**: Adjust HorizontalTextAlignment and VerticalTextAlignment
