# Data Handling

## Table of Contents
- [Overview](#overview)
- [Empty Points](#empty-points)
  - [Empty Point Mode](#empty-point-mode)
  - [Empty Point Customization](#empty-point-customization)
- [Data Point Collection Methods](#data-point-collection-methods)
  - [GetDataPoints by Rectangle](#getdatapoints-by-rectangle)
  - [GetDataPoints by Range](#getdatapoints-by-range)
- [Touch Position](#touch-position)
  - [OnTouchDown](#ontouchdown)
  - [OnTouchMove](#ontouchmove)
  - [OnTouchUp](#ontouchup)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

Data handling in Cartesian Charts includes managing empty/missing data points, retrieving data points within specific regions, and capturing touch interactions. These features enable robust data visualization and user interaction.

## Empty Points

Empty points represent missing or null data in a series. They can occur when data is unavailable, improperly formatted, or explicitly set as null or double.NaN.

### Defining Empty Points

```csharp
public class ViewModel
{
    public ObservableCollection<Model> ProductSales { get; set; }
    
    public ViewModel()
    {
        ProductSales = new ObservableCollection<Model>();
        ProductSales.Add(new Model() { Product = "Electronics", Sales = 60 });
        ProductSales.Add(new Model() { Product = "Clothing", Sales = 40 });
        ProductSales.Add(new Model() { Product = "Groceries", Sales = double.NaN }); // Empty point
        ProductSales.Add(new Model() { Product = "Furniture", Sales = 70 });
        ProductSales.Add(new Model() { Product = "Toys", Sales = 30 });
        ProductSales.Add(new Model() { Product = "Sports", Sales = double.NaN }); // Empty point
        ProductSales.Add(new Model() { Product = "Books", Sales = 50 });
    }
}
```

### Empty Point Mode

The `EmptyPointMode` property specifies how empty points are handled:

**Available Modes:**
- `None`: Empty points not rendered (default)
- `Zero`: Replace empty points with zero
- `Average`: Replace with average of surrounding points

#### Zero Mode

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:LineSeries ItemsSource="{Binding ProductSales}"
                     XBindingPath="Product"
                     YBindingPath="Sales"
                     EmptyPointMode="Zero"/>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();

LineSeries series = new LineSeries()
{
    ItemsSource = new ViewModel().ProductSales,
    XBindingPath = "Product",
    YBindingPath = "Sales",
    EmptyPointMode = EmptyPointMode.Zero
};

chart.Series.Add(series);
```

#### Average Mode

```xml
<chart:ColumnSeries ItemsSource="{Binding ProductSales}"
                   XBindingPath="Product"
                   YBindingPath="Sales"
                   EmptyPointMode="Average"/>
```

```csharp
ColumnSeries series = new ColumnSeries()
{
    ItemsSource = new ViewModel().ProductSales,
    XBindingPath = "Product",
    YBindingPath = "Sales",
    EmptyPointMode = EmptyPointMode.Average
};
```

### Empty Point Customization

Customize appearance of empty points using `EmptyPointSettings`:

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:LineSeries ItemsSource="{Binding ProductSales}"
                     XBindingPath="Product"
                     YBindingPath="Sales"
                     Fill="#3068F7"
                     StrokeWidth="2"
                     ShowMarkers="True"
                     ShowDataLabels="True"
                     EmptyPointMode="Average">
        <chart:LineSeries.EmptyPointSettings>
            <chart:EmptyPointSettings Fill="Orange" 
                                     Stroke="Red"
                                     StrokeWidth="2"/>
        </chart:LineSeries.EmptyPointSettings>
    </chart:LineSeries>
</chart:SfCartesianChart>
```

```csharp
LineSeries series = new LineSeries()
{
    ItemsSource = new ViewModel().ProductSales,
    XBindingPath = "Product",
    YBindingPath = "Sales",
    Fill = new SolidColorBrush(Color.FromArgb("#3068F7")),
    StrokeWidth = 2,
    ShowMarkers = true,
    ShowDataLabels = true,
    EmptyPointMode = EmptyPointMode.Average
};

EmptyPointSettings emptyPointSettings = new EmptyPointSettings()
{
    Fill = new SolidColorBrush(Colors.Orange),
    Stroke = new SolidColorBrush(Colors.Red),
    StrokeWidth = 2
};

series.EmptyPointSettings = emptyPointSettings;
```

**EmptyPointSettings Properties:**
- `Fill`: Fill color for empty points
- `Stroke`: Stroke color for empty points
- `StrokeWidth`: Stroke thickness for empty points

## Data Point Collection Methods

CartesianSeries provides methods to retrieve data points within specific regions.

### GetDataPoints by Rectangle

Get data points that fall inside a rectangular region:

```csharp
public class MainPage : ContentPage
{
    private SfCartesianChart chart;
    private LineSeries series;
    
    public MainPage()
    {
        chart = new SfCartesianChart();
        series = new LineSeries()
        {
            ItemsSource = new ViewModel().Data,
            XBindingPath = "XValue",
            YBindingPath = "YValue"
        };
        chart.Series.Add(series);
        
        // Define rectangular region
        Rect rectangle = new Rect(50, 50, 200, 150);
        
        // Get data points in region
        List<object> dataPoints = series.GetDataPoints(rectangle);
        
        // Process retrieved data points
        foreach (var point in dataPoints)
        {
            // Handle each data point
        }
        
        Content = chart;
    }
}
```

### GetDataPoints by Range

Get data points from specified axis visible range:

```csharp
public class MainPage : ContentPage
{
    private SfCartesianChart chart;
    private LineSeries series;
    
    public MainPage()
    {
        chart = new SfCartesianChart();
        series = new LineSeries()
        {
            ItemsSource = new ViewModel().Data,
            XBindingPath = "XValue",
            YBindingPath = "YValue"
        };
        chart.Series.Add(series);
        
        // Define coordinate ranges
        double startX = 2;
        double endX = 8;
        double startY = 10;
        double endY = 50;
        
        // Get data points in range
        List<object> dataPoints = series.GetDataPoints(startX, endX, startY, endY);
        
        // Process retrieved data points
        foreach (var point in dataPoints)
        {
            // Handle each data point
        }
        
        Content = chart;
    }
}
```

### Using SeriesBounds

Get the visible plotting region at runtime:

```csharp
// Get series visible bounds
Rect seriesBounds = chart.SeriesBounds;

// Use bounds for calculations
double width = seriesBounds.Width;
double height = seriesBounds.Height;
```

## Touch Position

Handle touch interactions using `ChartInteractiveBehavior`:

### Custom Interactive Behavior

```csharp
public class ChartInteractiveExt : ChartInteractiveBehavior
{
    protected override void OnTouchDown(ChartBase chart, float pointX, float pointY)
    {
        base.OnTouchDown(chart, pointX, pointY);
        
        // Handle touch down
        System.Diagnostics.Debug.WriteLine($"Touch Down: X={pointX}, Y={pointY}");
    }

    protected override void OnTouchMove(ChartBase chart, float pointX, float pointY)
    {
        base.OnTouchMove(chart, pointX, pointY);
        
        // Handle touch move
        System.Diagnostics.Debug.WriteLine($"Touch Move: X={pointX}, Y={pointY}");
    }

    protected override void OnTouchUp(ChartBase chart, float pointX, float pointY)
    {
        base.OnTouchUp(chart, pointX, pointY);
        
        // Handle touch up
        System.Diagnostics.Debug.WriteLine($"Touch Up: X={pointX}, Y={pointY}");
    }
}
```

### Adding Interactive Behavior

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.InteractiveBehavior>
        <local:ChartInteractiveExt/>
    </chart:SfCartesianChart.InteractiveBehavior>
    
    <chart:SfCartesianChart.XAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:LineSeries ItemsSource="{Binding Data}"
                     XBindingPath="X"
                     YBindingPath="Y"/>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart();
ChartInteractiveExt interactiveExt = new ChartInteractiveExt();
chart.InteractiveBehavior = interactiveExt;
```

### OnTouchDown

Called when user initiates touch:

```csharp
protected override void OnTouchDown(ChartBase chart, float pointX, float pointY)
{
    base.OnTouchDown(chart, pointX, pointY);
    
    // Example: Highlight nearest data point
    if (chart is SfCartesianChart cartesianChart)
    {
        var series = cartesianChart.Series[0] as CartesianSeries;
        var rect = new Rect(pointX - 10, pointY - 10, 20, 20);
        var points = series?.GetDataPoints(rect);
        
        if (points?.Count > 0)
        {
            // Handle selection
        }
    }
}
```

### OnTouchMove

Called during touch drag:

```csharp
protected override void OnTouchMove(ChartBase chart, float pointX, float pointY)
{
    base.OnTouchMove(chart, pointX, pointY);
    
    // Example: Update custom crosshair
    UpdateCrosshair(pointX, pointY);
}

private void UpdateCrosshair(float x, float y)
{
    // Custom crosshair logic
}
```

### OnTouchUp

Called when touch ends:

```csharp
protected override void OnTouchUp(ChartBase chart, float pointX, float pointY)
{
    base.OnTouchUp(chart, pointX, pointY);
    
    // Example: Finalize selection
    FinalizeSelection();
}

private void FinalizeSelection()
{
    // Selection finalization logic
}
```

## Best Practices

### Empty Point Handling

1. **Choose Appropriate Mode**: Select mode based on data context
   - `Zero`: When zero is a meaningful value
   - `Average`: For smooth interpolation
   - `None`: When gaps should be visible

2. **Visual Distinction**: Make empty points visually distinct
3. **Data Validation**: Validate data before binding to chart
4. **Documentation**: Document how empty points are handled
5. **User Communication**: Inform users about missing data

### Data Point Retrieval

1. **Performance**: Use specific regions to limit data retrieval
2. **Coordinate Verification**: Ensure coordinates within axis range
3. **Null Checks**: Always check for null/empty collections
4. **Bounds Awareness**: Use SeriesBounds for accurate positioning
5. **Efficient Filtering**: Retrieve only needed data points

### Touch Interaction

1. **Override Carefully**: Call base methods when overriding
2. **Performance**: Keep touch handlers lightweight
3. **Responsiveness**: Provide immediate visual feedback
4. **Platform Testing**: Test on all target platforms
5. **Gesture Conflicts**: Avoid conflicts with built-in gestures

### Common Patterns

**Empty Point Handling with Customization:**
```xml
<chart:LineSeries EmptyPointMode="Average"
                 ShowMarkers="True">
    <chart:LineSeries.EmptyPointSettings>
        <chart:EmptyPointSettings Fill="Orange" StrokeWidth="2"/>
    </chart:LineSeries.EmptyPointSettings>
</chart:LineSeries>
```

**Selection by Region:**
```csharp
Rect selectionRect = new Rect(x - 5, y - 5, 10, 10);
List<object> selectedPoints = series.GetDataPoints(selectionRect);
```

## Troubleshooting

**Issue**: Empty points still showing gaps
- **Solution**: Set `EmptyPointMode` to `Zero` or `Average`
- **Solution**: Verify EmptyPointMode property is set correctly
- **Solution**: Check data source for null values

**Issue**: EmptyPointSettings not applied
- **Solution**: Ensure EmptyPointMode is not `None`
- **Solution**: Verify EmptyPointSettings properly configured
- **Solution**: Check Fill/Stroke properties have values

**Issue**: GetDataPoints returns empty list
- **Solution**: Verify rectangle/range overlaps with data
- **Solution**: Check axis ranges include requested coordinates
- **Solution**: Ensure series has valid data points

**Issue**: Touch events not firing
- **Solution**: Verify InteractiveBehavior added to chart
- **Solution**: Call base methods in override implementations
- **Solution**: Check for gesture recognizer conflicts

**Issue**: Incorrect coordinates in touch events
- **Solution**: Coordinates are in pixels, not axis values
- **Solution**: Use appropriate conversion if needed
- **Solution**: Verify chart layout is complete

**Issue**: Empty points showing wrong color
- **Solution**: Set Fill property in EmptyPointSettings
- **Solution**: Check series PaletteBrushes not overriding
- **Solution**: Verify EmptyPointSettings assigned to series

**Issue**: Performance issues with GetDataPoints
- **Solution**: Limit region size for retrieval
- **Solution**: Cache results when appropriate
- **Solution**: Use specific ranges instead of large rectangles
