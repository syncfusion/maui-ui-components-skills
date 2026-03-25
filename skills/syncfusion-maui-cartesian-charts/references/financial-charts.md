# Financial Charts

Financial charts are specialized visualizations designed for displaying stock prices, forex data, and other financial information. These charts represent price movements using Open, High, Low, and Close (OHLC) values.

## Overview

Syncfusion .NET MAUI Cartesian Chart provides two financial chart types:
- **Candle (Candlestick) Charts**: Visual rectangles (candles) showing price movements
- **OHLC Charts**: Line-based representation with tick marks for open/close prices

## Data Structure for Financial Charts

Financial charts require five data points per item:

```csharp
public class StockData
{
    public string Date { get; set; }     // X-axis value (time period)
    public double Open { get; set; }     // Opening price
    public double High { get; set; }     // Highest price in period
    public double Low { get; set; }      // Lowest price in period
    public double Close { get; set; }    // Closing price
}
```

### Sample Data

```csharp
public class StockViewModel
{
    public ObservableCollection<StockData> StockPrices { get; set; }
    
    public StockViewModel()
    {
        StockPrices = new ObservableCollection<StockData>
        {
            new StockData { Date = "2024-01", High = 50, Low = 40, Open = 47, Close = 45 },
            new StockData { Date = "2024-02", High = 50, Low = 35, Open = 45, Close = 40 },
            new StockData { Date = "2024-03", High = 40, Low = 30, Open = 37, Close = 40 },
            new StockData { Date = "2024-04", High = 50, Low = 35, Open = 40, Close = 45 },
            new StockData { Date = "2024-05", High = 45, Low = 30, Open = 35, Close = 32 },
            new StockData { Date = "2024-06", High = 50, Low = 35, Open = 40, Close = 45 },
            new StockData { Date = "2024-07", High = 40, Low = 31, Open = 36, Close = 34 },
            new StockData { Date = "2024-08", High = 48, Low = 38, Open = 43, Close = 40 },
            new StockData { Date = "2024-09", High = 55, Low = 45, Open = 48, Close = 50 },
            new StockData { Date = "2024-10", High = 45, Low = 30, Open = 35, Close = 40 }
        };
    }
}
```

## Candle Chart (Candlestick)

Candle charts display price movements as rectangular "candles" with wicks. The body represents the open-close range, while wicks show the high-low range.

### Basic Candle Chart

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:CandleSeries ItemsSource="{Binding StockPrices}"
                       XBindingPath="Date"
                       Open="Open"
                       High="High"
                       Low="Low"
                       Close="Close"/>
</chart:SfCartesianChart>
```

```csharp
CandleSeries series = new CandleSeries()
{
    ItemsSource = new StockViewModel().StockPrices,
    XBindingPath = "Date",
    Open = "Open",       // Property name for open value
    High = "High",       // Property name for high value
    Low = "Low",         // Property name for low value
    Close = "Close"      // Property name for close value
};
chart.Series.Add(series);
```

### Understanding Candlestick Components

**Candle Body:**
- Filled/colored rectangle between open and close prices
- **Bullish candle** (price increased): Close > Open
- **Bearish candle** (price decreased): Close < Open

**Wicks (Shadows):**
- Upper wick: High to max(Open, Close)
- Lower wick: Low to min(Open, Close)

### Bull and Bear Colors

Customize colors for bullish (price up) and bearish (price down) candles:

```xml
<chart:CandleSeries ItemsSource="{Binding StockPrices}"
                   XBindingPath="Date"
                   Open="Open"
                   High="High"
                   Low="Low"
                   Close="Close"
                   BullishFill="Green"
                   BearishFill="Red"/>
```

```csharp
CandleSeries series = new CandleSeries()
{
    ItemsSource = data,
    XBindingPath = "Date",
    Open = "Open",
    High = "High",
    Low = "Low",
    Close = "Close",
    BullishFill = new SolidColorBrush(Colors.Green),  // Price increased
    BearishFill = new SolidColorBrush(Colors.Red)     // Price decreased
};
```

**Traditional Colors:**
- Bullish: Green (Western) or Red (Asian markets)
- Bearish: Red (Western) or Green (Asian markets)

### Solid vs Hollow Candles

Control whether candles are filled or hollow using `EnableSolidCandle`:

```xml
<chart:CandleSeries ItemsSource="{Binding StockPrices}"
                   XBindingPath="Date"
                   Open="Open"
                   High="High"
                   Low="Low"
                   Close="Close"
                   EnableSolidCandle="True"
                   BullishFill="LightGreen"
                   BearishFill="LightCoral"/>
```

```csharp
CandleSeries series = new CandleSeries()
{
    ItemsSource = data,
    XBindingPath = "Date",
    Open = "Open",
    High = "High",
    Low = "Low",
    Close = "Close",
    EnableSolidCandle = true,  // All candles filled
    BullishFill = new SolidColorBrush(Colors.LightGreen),
    BearishFill = new SolidColorBrush(Colors.LightCoral)
};
```

**When `EnableSolidCandle = false` (default):**
- Bullish candles are hollow (outlined only)
- Bearish candles are filled

**When `EnableSolidCandle = true`:**
- Both bullish and bearish candles are filled

### Candle Width and Spacing

```xml
<chart:CandleSeries ItemsSource="{Binding StockPrices}"
                   XBindingPath="Date"
                   Open="Open"
                   High="High"
                   Low="Low"
                   Close="Close"
                   Width="0.8"
                   Spacing="0.1"/>
```

- **Width**: 0 to 1 (0.8 = 80% of available width)
- **Spacing**: 0 to 1 (0.1 = 10% gap between candles)

## OHLC Chart

OHLC charts use lines and tick marks instead of candles to represent the same data.

### Basic OHLC Chart

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:HiLoOpenCloseSeries ItemsSource="{Binding StockPrices}"
                              XBindingPath="Date"
                              Open="Open"
                              High="High"
                              Low="Low"
                              Close="Close"/>
</chart:SfCartesianChart>
```

```csharp
HiLoOpenCloseSeries series = new HiLoOpenCloseSeries()
{
    ItemsSource = new StockViewModel().StockPrices,
    XBindingPath = "Date",
    Open = "Open",
    High = "High",
    Low = "Low",
    Close = "Close"
};
chart.Series.Add(series);
```

### OHLC Components

**Vertical Line:** Connects High to Low  
**Left Tick:** Open price  
**Right Tick:** Close price

### Bull and Bear Colors for OHLC

```xml
<chart:HiLoOpenCloseSeries ItemsSource="{Binding StockPrices}"
                          XBindingPath="Date"
                          Open="Open"
                          High="High"
                          Low="Low"
                          Close="Close"
                          BullishFill="Blue"
                          BearishFill="Orange"/>
```

```csharp
HiLoOpenCloseSeries series = new HiLoOpenCloseSeries()
{
    ItemsSource = data,
    XBindingPath = "Date",
    Open = "Open",
    High = "High",
    Low = "Low",
    Close = "Close",
    BullishFill = new SolidColorBrush(Colors.Blue),
    BearishFill = new SolidColorBrush(Colors.Orange)
};
```

## Choosing Between Candle and OHLC

### Use Candle Charts When:
- Visual clarity is priority
- Need quick pattern recognition
- Displaying to non-technical audiences
- Screen space allows larger visualization
- Emphasizing price body (open-close range)

### Use OHLC Charts When:
- Data density is high (many time periods)
- Technical analysis with multiple indicators
- Minimalist visualization preferred
- Limited screen space
- Focus on high-low range

## Complete Financial Chart Example

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:chart="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts"
             xmlns:local="clr-namespace:YourNamespace">

    <ContentPage.BindingContext>
        <local:StockViewModel/>
    </ContentPage.BindingContext>

    <chart:SfCartesianChart>
        <chart:SfCartesianChart.Title>
            <Label Text="Stock Price Analysis" FontSize="18" FontAttributes="Bold"/>
        </chart:SfCartesianChart.Title>

        <chart:SfCartesianChart.XAxes>
            <chart:DateTimeAxis IntervalType="Months" EdgeLabelsDrawingMode="Shift">
                <chart:DateTimeAxis.Title>
                    <chart:ChartAxisTitle Text="Date"/>
                </chart:DateTimeAxis.Title>
            </chart:DateTimeAxis>
        </chart:SfCartesianChart.XAxes>

        <chart:SfCartesianChart.YAxes>
            <chart:NumericalAxis>
                <chart:NumericalAxis.Title>
                    <chart:ChartAxisTitle Text="Price ($)"/>
                </chart:NumericalAxis.Title>
            </chart:NumericalAxis>
        </chart:SfCartesianChart.YAxes>

        <chart:SfCartesianChart.TrackballBehavior>
            <chart:ChartTrackballBehavior ShowLabel="True"/>
        </chart:SfCartesianChart.TrackballBehavior>

        <chart:CandleSeries ItemsSource="{Binding StockPrices}"
                           XBindingPath="Date"
                           Open="Open"
                           High="High"
                           Low="Low"
                           Close="Close"
                           BullishFill="#26A69A"
                           BearishFill="#EF5350"
                           EnableTooltip="True"/>
    </chart:SfCartesianChart>
</ContentPage>
```

## Common Patterns

### Pattern 1: Stock Price with DateTime Axis

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.XAxes>
        <chart:DateTimeAxis IntervalType="Days" Interval="1"/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:CandleSeries ItemsSource="{Binding DailyPrices}"
                       XBindingPath="DateTime"
                       Open="Open"
                       High="High"
                       Low="Low"
                       Close="Close"/>
</chart:SfCartesianChart>
```

### Pattern 2: Multiple Timeframes

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.Legend>
        <chart:ChartLegend/>
    </chart:SfCartesianChart.Legend>
    
    <chart:SfCartesianChart.XAxes>
        <chart:DateTimeAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:CandleSeries ItemsSource="{Binding StockA}"
                       XBindingPath="Date"
                       Open="Open" High="High" Low="Low" Close="Close"
                       Label="Stock A"
                       BullishFill="Green"
                       BearishFill="Red"/>
</chart:SfCartesianChart>
```

### Pattern 3: Financial Chart with Zooming

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.ZoomPanBehavior>
        <chart:ChartZoomPanBehavior EnablePinchZooming="True"
                                    EnableDoubleTap="True"
                                    EnablePanning="True"/>
    </chart:SfCartesianChart.ZoomPanBehavior>
    
    <chart:SfCartesianChart.XAxes>
        <chart:DateTimeAxis/>
    </chart:SfCartesianChart.XAxes>
    
    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>
    
    <chart:CandleSeries ItemsSource="{Binding StockPrices}"
                       XBindingPath="Date"
                       Open="Open" High="High" Low="Low" Close="Close"/>
</chart:SfCartesianChart>
```

## Data Validation

### Handling Invalid Data

Financial charts require valid OHLC relationships:

```csharp
public class StockData
{
    private double _open, _high, _low, _close;
    
    public double Open
    {
        get => _open;
        set => _open = value;
    }
    
    public double High
    {
        get => _high;
        set => _high = Math.Max(value, Math.Max(_open, _close)); // High >= Open, Close
    }
    
    public double Low
    {
        get => _low;
        set => _low = Math.Min(value, Math.Min(_open, _close));  // Low <= Open, Close
    }
    
    public double Close
    {
        get => _close;
        set => _close = value;
    }
}
```

### Data Validation Rules

1. **High >= Max(Open, Close)**: High must be highest value
2. **Low <= Min(Open, Close)**: Low must be lowest value
3. **All values > 0**: For price data (unless shorting scenarios)

## Tips and Best Practices

1. **Use DateTimeAxis** for time-series financial data
2. **Enable trackball** for precise value inspection
3. **Add zooming** for large datasets
4. **Consistent color scheme**: Stick to market conventions
5. **Validate OHLC relationships** before binding
6. **Consider data frequency**: Daily, hourly, minute charts need different approaches
7. **Performance**: Use FastLineSeries for very large datasets with trendlines
8. **Tooltip customization**: Show all OHLC values in tooltip

## Common Gotchas

### Property Names Are Strings

```csharp
// ✓ Correct - property names as strings
Open = "Open"
High = "High"

// ✗ Incorrect - trying to use actual values
Open = 45.0   // This won't work
```

### DateTime vs Category Axis

```xml
<!-- For actual DateTime objects -->
<chart:DateTimeAxis IntervalType="Days"/>

<!-- For date strings -->
<chart:CategoryAxis/>
```

### Bull/Bear Logic

- **Bullish**: Close > Open (price went up)
- **Bearish**: Close < Open (price went down)
- **Doji**: Close == Open (no net change)

### Data Sorting

Ensure data is sorted chronologically for proper visualization:

```csharp
StockPrices = new ObservableCollection<StockData>(
    rawData.OrderBy(d => d.Date)
);
```
