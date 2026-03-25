# Range Charts

## Overview

Range charts visualize data points with high and low values, displaying the range or interval between two values. They are particularly useful for showing price fluctuations, temperature ranges, or any data with upper and lower bounds.

**Available Range Chart Types:**
- **Range Area**: Area filled between high and low values
- **Range Column**: Vertical columns showing range
- **Spline Range Area**: Smooth curved area between ranges

All range charts require two Y values for each data point: a high value and a low value.

## Range Area Chart

Range area charts display the area between two lines representing high and low values, making it easy to visualize ranges and compare multiple datasets.

### Basic Implementation

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:RangeAreaSeries ItemsSource="{Binding TemperatureData}"
                           XBindingPath="Month"
                           High="MaxTemp"
                           Low="MinTemp"/>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();

CategoryAxis primaryAxis = new CategoryAxis();
chart.XAxes.Add(primaryAxis);

NumericalAxis secondaryAxis = new NumericalAxis();
chart.YAxes.Add(secondaryAxis);

RangeAreaSeries series = new RangeAreaSeries()
{
    ItemsSource = new ViewModel().TemperatureData,
    XBindingPath = "Month",
    High = "MaxTemp",
    Low = "MinTemp"
};

chart.Series.Add(series);
this.Content = chart;
```

### Data Model

Range charts require a data model with high and low properties:

```csharp
public class TemperatureModel
{
    public string Month { get; set; }
    public double MaxTemp { get; set; }
    public double MinTemp { get; set; }
}

public class ViewModel
{
    public ObservableCollection<TemperatureModel> TemperatureData { get; set; }
    
    public ViewModel()
    {
        TemperatureData = new ObservableCollection<TemperatureModel>
        {
            new TemperatureModel { Month = "Jan", MaxTemp = 25, MinTemp = 15 },
            new TemperatureModel { Month = "Feb", MaxTemp = 27, MinTemp = 17 },
            new TemperatureModel { Month = "Mar", MaxTemp = 30, MinTemp = 20 },
            new TemperatureModel { Month = "Apr", MaxTemp = 32, MinTemp = 22 },
            new TemperatureModel { Month = "May", MaxTemp = 35, MinTemp = 25 },
            new TemperatureModel { Month = "Jun", MaxTemp = 38, MinTemp = 28 }
        };
    }
}
```

### Enable Markers

Display markers at both high and low data points:

```xml
<chart:RangeAreaSeries ItemsSource="{Binding TemperatureData}"
                       XBindingPath="Month"
                       High="MaxTemp"
                       Low="MinTemp"
                       ShowMarkers="True"/>
```

```csharp
RangeAreaSeries series = new RangeAreaSeries()
{
    ItemsSource = new ViewModel().TemperatureData,
    XBindingPath = "Month",
    High = "MaxTemp",
    Low = "MinTemp",
    ShowMarkers = true
};
```

### Marker Customization

Customize marker appearance for both high and low points:

```xml
<chart:RangeAreaSeries ItemsSource="{Binding TemperatureData}"
                       XBindingPath="Month"
                       High="MaxTemp"
                       Low="MinTemp"
                       ShowMarkers="True">
    <chart:RangeAreaSeries.MarkerSettings>
        <chart:ChartMarkerSettings Type="Diamond"
                                   Fill="Red"
                                   Stroke="DarkRed"
                                   StrokeWidth="2"
                                   Height="10"
                                   Width="10"/>
    </chart:RangeAreaSeries.MarkerSettings>
</chart:RangeAreaSeries>
```

```csharp
ChartMarkerSettings markerSettings = new ChartMarkerSettings()
{
    Type = ShapeType.Diamond,
    Fill = Colors.Red,
    Stroke = Colors.DarkRed,
    StrokeWidth = 2,
    Height = 10,
    Width = 10
};

RangeAreaSeries series = new RangeAreaSeries()
{
    ItemsSource = new ViewModel().TemperatureData,
    XBindingPath = "Month",
    High = "MaxTemp",
    Low = "MinTemp",
    ShowMarkers = true,
    MarkerSettings = markerSettings
};
```

## Range Column Chart

Range column charts use vertical columns where the height represents the difference between high and low values.

### Basic Implementation

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:RangeColumnSeries ItemsSource="{Binding PriceData}"
                             XBindingPath="Product"
                             High="HighPrice"
                             Low="LowPrice"/>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();

CategoryAxis primaryAxis = new CategoryAxis();
chart.XAxes.Add(primaryAxis);

NumericalAxis secondaryAxis = new NumericalAxis();
chart.YAxes.Add(secondaryAxis);

RangeColumnSeries series = new RangeColumnSeries()
{
    ItemsSource = new ViewModel().PriceData,
    XBindingPath = "Product",
    High = "HighPrice",
    Low = "LowPrice"
};

chart.Series.Add(series);
this.Content = chart;
```

### Data Model for Range Column

```csharp
public class PriceModel
{
    public string Product { get; set; }
    public double HighPrice { get; set; }
    public double LowPrice { get; set; }
}

public class ViewModel
{
    public ObservableCollection<PriceModel> PriceData { get; set; }
    
    public ViewModel()
    {
        PriceData = new ObservableCollection<PriceModel>
        {
            new PriceModel { Product = "Product A", HighPrice = 150, LowPrice = 100 },
            new PriceModel { Product = "Product B", HighPrice = 180, LowPrice = 120 },
            new PriceModel { Product = "Product C", HighPrice = 200, LowPrice = 140 },
            new PriceModel { Product = "Product D", HighPrice = 170, LowPrice = 110 },
            new PriceModel { Product = "Product E", HighPrice = 220, LowPrice = 160 }
        };
    }
}
```

### Spacing and Width

Control the appearance of range columns:

```xml
<chart:RangeColumnSeries ItemsSource="{Binding PriceData}"
                         XBindingPath="Product"
                         High="HighPrice"
                         Low="LowPrice"
                         Spacing="0.2"
                         Width="0.7"/>
```

```csharp
RangeColumnSeries series = new RangeColumnSeries()
{
    ItemsSource = new ViewModel().PriceData,
    XBindingPath = "Product",
    High = "HighPrice",
    Low = "LowPrice",
    Spacing = 0.2,   // Space between columns (0-1)
    Width = 0.7      // Column width (0-1)
};
```

**Property Details:**
- `Spacing`: Value from 0 to 1, where 1 = 100% spacing, 0 = no spacing
- `Width`: Value from 0 to 1, where 1 = full width, 0.8 = 80% width (default)

### Complete Customization Example

```xml
<chart:RangeColumnSeries ItemsSource="{Binding PriceData}"
                         XBindingPath="Product"
                         High="HighPrice"
                         Low="LowPrice"
                         Spacing="0.3"
                         Width="0.6"
                         Fill="#4CAF50"
                         Stroke="DarkGreen"
                         StrokeWidth="2"
                         CornerRadius="5"/>
```

```csharp
RangeColumnSeries series = new RangeColumnSeries()
{
    ItemsSource = new ViewModel().PriceData,
    XBindingPath = "Product",
    High = "HighPrice",
    Low = "LowPrice",
    Spacing = 0.3,
    Width = 0.6,
    Fill = new SolidColorBrush(Color.FromArgb("#4CAF50")),
    Stroke = new SolidColorBrush(Colors.DarkGreen),
    StrokeWidth = 2,
    CornerRadius = new CornerRadius(5)
};
```

## Spline Range Area Chart

Spline range area charts display smooth curves between high and low values, providing a more fluid visualization of range data.

### Basic Implementation

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:SplineRangeAreaSeries ItemsSource="{Binding WeatherData}"
                                 XBindingPath="Day"
                                 High="HighTemp"
                                 Low="LowTemp"/>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();

CategoryAxis primaryAxis = new CategoryAxis();
chart.XAxes.Add(primaryAxis);

NumericalAxis secondaryAxis = new NumericalAxis();
chart.YAxes.Add(secondaryAxis);

SplineRangeAreaSeries series = new SplineRangeAreaSeries()
{
    ItemsSource = new ViewModel().WeatherData,
    XBindingPath = "Day",
    High = "HighTemp",
    Low = "LowTemp"
};

chart.Series.Add(series);
this.Content = chart;
```

### Spline Types

The `Type` property controls the spline curve rendering:

**1. Natural (Default)**

Natural spline creates smooth curves:

```xml
<chart:SplineRangeAreaSeries ItemsSource="{Binding WeatherData}"
                             XBindingPath="Day"
                             High="HighTemp"
                             Low="LowTemp"
                             Type="Natural"/>
```

**2. Monotonic**

Monotonic spline ensures curves don't create false peaks or valleys:

```xml
<chart:SplineRangeAreaSeries ItemsSource="{Binding WeatherData}"
                             XBindingPath="Day"
                             High="HighTemp"
                             Low="LowTemp"
                             Type="Monotonic"/>
```

**3. Cardinal**

Cardinal spline provides controllable curve tension:

```xml
<chart:SplineRangeAreaSeries ItemsSource="{Binding WeatherData}"
                             XBindingPath="Day"
                             High="HighTemp"
                             Low="LowTemp"
                             Type="Cardinal"/>
```

**4. Clamped**

Clamped spline controls curve behavior at endpoints:

```xml
<chart:SplineRangeAreaSeries ItemsSource="{Binding WeatherData}"
                             XBindingPath="Day"
                             High="HighTemp"
                             Low="LowTemp"
                             Type="Clamped"/>
```

### Complete Implementation with Spline Type

```csharp
SplineRangeAreaSeries series = new SplineRangeAreaSeries()
{
    ItemsSource = new ViewModel().WeatherData,
    XBindingPath = "Day",
    High = "HighTemp",
    Low = "LowTemp",
    Type = SplineType.Cardinal
};

chart.Series.Add(series);
```

### Enable Markers for Spline Range

```xml
<chart:SplineRangeAreaSeries ItemsSource="{Binding WeatherData}"
                             XBindingPath="Day"
                             High="HighTemp"
                             Low="LowTemp"
                             ShowMarkers="True"/>
```

```csharp
SplineRangeAreaSeries series = new SplineRangeAreaSeries()
{
    ItemsSource = new ViewModel().WeatherData,
    XBindingPath = "Day",
    High = "HighTemp",
    Low = "LowTemp",
    ShowMarkers = true
};
```

### Marker Customization for Spline

```xml
<chart:SplineRangeAreaSeries ItemsSource="{Binding WeatherData}"
                             XBindingPath="Day"
                             High="HighTemp"
                             Low="LowTemp"
                             ShowMarkers="True">
    <chart:SplineRangeAreaSeries.MarkerSettings>
        <chart:ChartMarkerSettings Type="Circle"
                                   Fill="Orange"
                                   Stroke="DarkOrange"
                                   StrokeWidth="2"
                                   Height="8"
                                   Width="8"/>
    </chart:SplineRangeAreaSeries.MarkerSettings>
</chart:SplineRangeAreaSeries>
```

```csharp
ChartMarkerSettings markerSettings = new ChartMarkerSettings()
{
    Type = ShapeType.Circle,
    Fill = Colors.Orange,
    Stroke = Colors.DarkOrange,
    StrokeWidth = 2,
    Height = 8,
    Width = 8
};

SplineRangeAreaSeries series = new SplineRangeAreaSeries()
{
    ItemsSource = new ViewModel().WeatherData,
    XBindingPath = "Day",
    High = "HighTemp",
    Low = "LowTemp",
    ShowMarkers = true,
    MarkerSettings = markerSettings
};
```

## Best Practices

### When to Use Each Type

**Range Area:**
- Visualize continuous ranges over time
- Show temperature, price, or value fluctuations
- Emphasize the spread between values
- Multiple overlapping ranges comparison

**Range Column:**
- Display discrete range data
- Compare ranges across categories
- Show periodic range measurements
- Fewer data points with clear separation

**Spline Range Area:**
- Smooth continuous range visualization
- Reduce visual noise in fluctuating data
- Emphasize trends over exact values
- Scientific or analytical presentations

### Data Structure Guidelines

1. **Consistent Data Types**: Ensure High and Low properties are numeric
2. **Logical Ordering**: High values should always be >= Low values
3. **Complete Data**: Both High and Low must be present for each point
4. **Null Handling**: Consider default values or filtering null entries

### Complete ViewModel Example

```csharp
public class RangeDataModel
{
    public string Category { get; set; }
    public double HighValue { get; set; }
    public double LowValue { get; set; }
}

public class ViewModel : INotifyPropertyChanged
{
    private ObservableCollection<RangeDataModel> _rangeData;
    
    public ObservableCollection<RangeDataModel> RangeData
    {
        get => _rangeData;
        set
        {
            _rangeData = value;
            OnPropertyChanged(nameof(RangeData));
        }
    }
    
    public ViewModel()
    {
        LoadData();
    }
    
    private void LoadData()
    {
        RangeData = new ObservableCollection<RangeDataModel>();
        
        // Sample data with validation
        var data = new[]
        {
            (Category: "Q1", High: 85.0, Low: 45.0),
            (Category: "Q2", High: 92.0, Low: 52.0),
            (Category: "Q3", High: 78.0, Low: 38.0),
            (Category: "Q4", High: 95.0, Low: 55.0)
        };
        
        foreach (var item in data)
        {
            if (item.High >= item.Low)
            {
                RangeData.Add(new RangeDataModel
                {
                    Category = item.Category,
                    HighValue = item.High,
                    LowValue = item.Low
                });
            }
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Common Gotchas

1. **High < Low Values**: Always validate that High >= Low before binding
2. **Missing Properties**: Both High and Low binding paths must exist in data model
3. **Property Names**: Binding paths are case-sensitive
4. **Null Values**: Range charts don't handle null gracefully; filter or provide defaults
5. **Axis Range**: Ensure Y-axis range accommodates both high and low values

### Performance Tips

1. **Data Volume**: Range charts handle 50-100 points efficiently
2. **Update Strategy**: Use ObservableCollection for dynamic updates
3. **Marker Count**: Limit markers for large datasets (>50 points)
4. **Spline Complexity**: Natural/Cardinal types are more computationally intensive

### Styling and Customization

**Complete Styled Range Area:**

```xml
<chart:RangeAreaSeries ItemsSource="{Binding Data}"
                       XBindingPath="Category"
                       High="HighValue"
                       Low="LowValue"
                       Fill="#80E3F2FD"
                       Stroke="#2196F3"
                       StrokeWidth="2"
                       ShowMarkers="True">
    <chart:RangeAreaSeries.MarkerSettings>
        <chart:ChartMarkerSettings Type="Diamond"
                                   Fill="White"
                                   Stroke="#2196F3"
                                   StrokeWidth="2"
                                   Height="10"
                                   Width="10"/>
    </chart:RangeAreaSeries.MarkerSettings>
</chart:RangeAreaSeries>
```

**Complete Styled Range Column:**

```xml
<chart:RangeColumnSeries ItemsSource="{Binding Data}"
                         XBindingPath="Category"
                         High="HighValue"
                         Low="LowValue"
                         Fill="#4CAF50"
                         Stroke="#2E7D32"
                         StrokeWidth="2"
                         Width="0.7"
                         Spacing="0.2"
                         CornerRadius="8,8,0,0"/>
```

### Troubleshooting

**Issue**: Range not displaying correctly
- **Solution**: Verify High >= Low for all data points

**Issue**: Markers not appearing
- **Solution**: Set `ShowMarkers="True"` explicitly

**Issue**: Spline curves too sharp
- **Solution**: Try `Type="Monotonic"` or `Type="Cardinal"`

**Issue**: Columns too narrow/wide
- **Solution**: Adjust `Width` property (0-1 range)

**Issue**: Overlapping columns in multi-series
- **Solution**: Use `Spacing` property or reduce `Width`
