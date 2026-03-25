# Trendlines

## Overview

Trendlines are visual representations of the linear relationship between data points in a series. They show the overall direction and trend of data by fitting a line or curve through the data points, helping identify patterns and make predictions.

**Available Trendline Types:**
- Linear
- Exponential
- Logarithmic
- Power
- Polynomial
- MovingAverage

## Basic Trendline Usage

Add trendlines to series using XAML or C#:

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:ColumnSeries ItemsSource="{Binding Data}"
                       XBindingPath="X"
                       YBindingPath="Y">
        <chart:ColumnSeries.Trendlines>
            <chart:LinearTrendline Stroke="Blue" StrokeWidth="2"/>
        </chart:ColumnSeries.Trendlines>
    </chart:ColumnSeries>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();

ColumnSeries series = new ColumnSeries 
{ 
    ItemsSource = new ViewModel().Data, 
    XBindingPath = "X", 
    YBindingPath = "Y" 
};

series.Trendlines.Add(new LinearTrendline
{
    Stroke = new SolidColorBrush(Colors.Blue),
    StrokeWidth = 2
});

chart.Series.Add(series);
```

## Trendline Types

### Linear Trendline

Best for data that moves in a consistent direction (steadily up or down):

```xml
<chart:ColumnSeries ItemsSource="{Binding Data}" 
                   XBindingPath="X" 
                   YBindingPath="Y">
    <chart:ColumnSeries.Trendlines>
        <chart:LinearTrendline Stroke="Blue" 
                              StrokeWidth="2"
                              Label="Linear Trend"/>
    </chart:ColumnSeries.Trendlines>
</chart:ColumnSeries>
```

```csharp
series.Trendlines.Add(new LinearTrendline 
{ 
    Stroke = new SolidColorBrush(Colors.Blue), 
    StrokeWidth = 2,
    Label = "Linear Trend"
});
```

### Logarithmic Trendline

Shows data that changes quickly at first and then levels off:

```xml
<chart:ColumnSeries ItemsSource="{Binding Data}" 
                   XBindingPath="X" 
                   YBindingPath="Y">
    <chart:ColumnSeries.Trendlines>
        <chart:LogarithmicTrendline Stroke="Green" 
                                   StrokeWidth="2"
                                   Label="Logarithmic"/>
    </chart:ColumnSeries.Trendlines>
</chart:ColumnSeries>
```

```csharp
series.Trendlines.Add(new LogarithmicTrendline 
{ 
    Stroke = new SolidColorBrush(Colors.Green), 
    StrokeWidth = 2,
    Label = "Logarithmic"
});
```

### Exponential Trendline

Shows data that grows or shrinks at an increasingly fast rate (requires positive values):

```xml
<chart:ColumnSeries ItemsSource="{Binding Data}" 
                   XBindingPath="X" 
                   YBindingPath="Y">
    <chart:ColumnSeries.Trendlines>
        <chart:ExponentialTrendline Stroke="Orange" 
                                   StrokeWidth="2"
                                   Label="Exponential"/>
    </chart:ColumnSeries.Trendlines>
</chart:ColumnSeries>
```

```csharp
series.Trendlines.Add(new ExponentialTrendline 
{ 
    Stroke = new SolidColorBrush(Colors.Orange), 
    StrokeWidth = 2,
    Label = "Exponential"
});
```

### Power Trendline

Models data that accelerates at different rates (use positive values):

```xml
<chart:ColumnSeries ItemsSource="{Binding Data}" 
                   XBindingPath="X" 
                   YBindingPath="Y">
    <chart:ColumnSeries.Trendlines>
        <chart:PowerTrendline Stroke="Purple" 
                             StrokeWidth="2"
                             Label="Power"/>
    </chart:ColumnSeries.Trendlines>
</chart:ColumnSeries>
```

```csharp
series.Trendlines.Add(new PowerTrendline 
{ 
    Stroke = new SolidColorBrush(Colors.Purple), 
    StrokeWidth = 2,
    Label = "Power"
});
```

### Polynomial Trendline

Curved line that follows ups and downs in data (use `Order` property to control curve):

```xml
<chart:ColumnSeries ItemsSource="{Binding Data}" 
                   XBindingPath="X" 
                   YBindingPath="Y">
    <chart:ColumnSeries.Trendlines>
        <chart:PolynomialTrendline Order="3" 
                                  Stroke="Red" 
                                  StrokeWidth="2"
                                  Label="Polynomial (Order 3)"/>
    </chart:ColumnSeries.Trendlines>
</chart:ColumnSeries>
```

```csharp
series.Trendlines.Add(new PolynomialTrendline 
{ 
    Order = 3, 
    Stroke = new SolidColorBrush(Colors.Red), 
    StrokeWidth = 2,
    Label = "Polynomial (Order 3)"
});
```

### Moving Average Trendline

Smooths out small bumps by averaging nearby points (use `Period` property):

```xml
<chart:ColumnSeries ItemsSource="{Binding Data}" 
                   XBindingPath="X" 
                   YBindingPath="Y">
    <chart:ColumnSeries.Trendlines>
        <chart:MovingAverageTrendline Period="5" 
                                     Stroke="Brown" 
                                     StrokeWidth="2"
                                     Label="5-Period Moving Average"/>
    </chart:ColumnSeries.Trendlines>
</chart:ColumnSeries>
```

```csharp
series.Trendlines.Add(new MovingAverageTrendline 
{ 
    Period = 5, 
    Stroke = new SolidColorBrush(Colors.Brown), 
    StrokeWidth = 2,
    Label = "5-Period Moving Average"
});
```

## Forecasting

Extend trendlines beyond existing data to predict future or past values.

### Forward Forecasting

Extend trendline into the future:

```xml
<chart:LinearTrendline ForwardForecast="5" 
                      Stroke="Blue" 
                      StrokeWidth="2"
                      Label="Forecast Next 5 Points"/>
```

```csharp
var trendline = new LinearTrendline 
{ 
    ForwardForecast = 5, 
    Stroke = new SolidColorBrush(Colors.Blue), 
    StrokeWidth = 2,
    Label = "Forecast Next 5 Points"
};
```

### Backward Forecasting

Extend trendline into the past:

```xml
<chart:LinearTrendline BackwardForecast="3" 
                      Stroke="Blue" 
                      StrokeWidth="2"
                      Label="Historical Projection"/>
```

```csharp
var trendline = new LinearTrendline 
{ 
    BackwardForecast = 3, 
    Stroke = new SolidColorBrush(Colors.Blue), 
    StrokeWidth = 2,
    Label = "Historical Projection"
};
```

### Combined Forecasting

```xml
<chart:LinearTrendline ForwardForecast="5" 
                      BackwardForecast="3" 
                      Stroke="Blue" 
                      StrokeWidth="2"/>
```

## Customization

### Basic Styling

```xml
<chart:LinearTrendline Stroke="Black" 
                      StrokeWidth="2" 
                      StrokeDashArray="5,6"
                      Label="Custom Styled"/>
```

```csharp
var trendline = new LinearTrendline 
{ 
    Stroke = new SolidColorBrush(Colors.Black), 
    StrokeWidth = 2,
    Label = "Custom Styled"
};
trendline.StrokeDashArray = new DoubleCollection { 5, 6 };
```

### Markers

Add markers to highlight points along the trendline:

```xml
<chart:LinearTrendline ShowMarkers="True">
    <chart:LinearTrendline.MarkerSettings>
        <chart:ChartMarkerSettings Width="8" 
                                  Height="8" 
                                  Type="Circle" 
                                  Fill="Purple" 
                                  Stroke="Blue"
                                  StrokeWidth="2"/>
    </chart:LinearTrendline.MarkerSettings>
</chart:LinearTrendline>
```

```csharp
var trendline = new LinearTrendline() { ShowMarkers = true };
trendline.MarkerSettings = new ChartMarkerSettings 
{ 
    Width = 8, 
    Height = 8, 
    Stroke = new SolidColorBrush(Colors.Blue), 
    Fill = new SolidColorBrush(Colors.Purple), 
    Type = ShapeType.Circle,
    StrokeWidth = 2
};
```

### Tooltip and Trackball

Enable interactive features:

```xml
<chart:LinearTrendline EnableTooltip="True" 
                      ShowTrackballLabel="True"
                      Stroke="Blue"
                      StrokeWidth="2"/>
```

```csharp
var trendline = new LinearTrendline 
{ 
    EnableTooltip = true, 
    ShowTrackballLabel = true,
    Stroke = new SolidColorBrush(Colors.Blue),
    StrokeWidth = 2
};
```

## Multiple Trendlines

Compare different trend models on same data:

```xml
<chart:ColumnSeries ItemsSource="{Binding Data}" 
                   XBindingPath="X" 
                   YBindingPath="Y">
    <chart:ColumnSeries.Trendlines>
        <chart:LinearTrendline Stroke="RoyalBlue" 
                              StrokeWidth="2" 
                              Label="Linear"/>
        
        <chart:PolynomialTrendline Order="3" 
                                  Stroke="Orange" 
                                  StrokeWidth="2" 
                                  Label="Polynomial (3)"/>
        
        <chart:MovingAverageTrendline Period="5" 
                                     Stroke="Green" 
                                     StrokeWidth="2" 
                                     Label="MovingAverage (5)"/>
    </chart:ColumnSeries.Trendlines>
</chart:ColumnSeries>
```

```csharp
series.Trendlines.Add(new LinearTrendline 
{ 
    Stroke = new SolidColorBrush(Colors.RoyalBlue), 
    StrokeWidth = 2, 
    Label = "Linear" 
});

series.Trendlines.Add(new PolynomialTrendline 
{ 
    Order = 3, 
    Stroke = new SolidColorBrush(Colors.Orange),
    StrokeWidth = 2, 
    Label = "Polynomial (3)" 
});

series.Trendlines.Add(new MovingAverageTrendline 
{ 
    Period = 5, 
    Stroke = new SolidColorBrush(Colors.Green),
    StrokeWidth = 2, 
    Label = "MovingAverage (5)" 
});
```

## Best Practices

### Choosing the Right Trendline

1. **Linear**: For data with consistent growth or decline
2. **Exponential**: For accelerating growth (population, viral spread)
3. **Logarithmic**: For rapid initial growth that slows down
4. **Power**: For scientific measurements with varying rates
5. **Polynomial**: For data with multiple peaks and valleys
6. **Moving Average**: To smooth out noise and see main trend

### Forecasting Guidelines

1. **Forward Forecast**: Predict future trends based on historical data
2. **Backward Forecast**: Understand historical patterns
3. **Reasonable Limits**: Don't forecast too far beyond data range
4. **Validation**: Compare forecasts with actual data when available
5. **Multiple Models**: Use multiple trendlines to compare predictions

### Styling Best Practices

1. **Distinct Colors**: Use different colors for multiple trendlines
2. **Appropriate Width**: Make trendlines visible but not overwhelming
3. **Dash Patterns**: Use dashed lines to distinguish from data series
4. **Labels**: Always label trendlines in legend
5. **Contrast**: Ensure trendlines visible against chart background

### Common Patterns

**Sales Trend Analysis:**
```xml
<chart:LinearTrendline ForwardForecast="3"
                      Stroke="Blue"
                      StrokeWidth="2"
                      Label="Sales Projection"/>
```

**Smoothing Volatile Data:**
```xml
<chart:MovingAverageTrendline Period="7"
                             Stroke="Green"
                             StrokeWidth="2"
                             Label="7-Day Average"/>
```

## Troubleshooting

**Issue**: Trendline not visible
- **Solution**: Check `Stroke` and `StrokeWidth` properties
- **Solution**: Verify trendline added to series Trendlines collection
- **Solution**: Ensure series has valid data points

**Issue**: Exponential/Power trendline error
- **Solution**: Ensure all Y values are positive
- **Solution**: Check for zero or negative values in data
- **Solution**: Use different trendline type if data contains negatives

**Issue**: Polynomial trendline too wavy
- **Solution**: Reduce `Order` property value (default is 2)
- **Solution**: Higher order = more curves, lower order = smoother

**Issue**: Moving average not smooth enough
- **Solution**: Increase `Period` property (more points averaged)
- **Solution**: Default period is 2, try 5, 7, or higher

**Issue**: Forecast extends too far
- **Solution**: Adjust `ForwardForecast` or `BackwardForecast` values
- **Solution**: Forecast accuracy decreases with distance from data

**Issue**: Multiple trendlines hard to distinguish
- **Solution**: Use different colors for each trendline
- **Solution**: Apply different dash patterns
- **Solution**: Add descriptive labels
- **Solution**: Enable legend to show all trendlines

**Issue**: Trendline tooltip not showing
- **Solution**: Set `EnableTooltip="True"`
- **Solution**: Add ChartTooltipBehavior to chart behaviors
- **Solution**: Check tooltip template if using custom template
