# Chart Types in .NET MAUI Circular Charts

Syncfusion .NET MAUI Circular Charts support three chart types: PieSeries, DoughnutSeries, and RadialBarSeries. Each type serves different visualization needs.

## Table of Contents
- [PieSeries (Pie Chart)](#pieseries-pie-chart)
- [DoughnutSeries (Doughnut Chart)](#doughnutseries-doughnut-chart)
- [RadialBarSeries (Radial Bar Chart)](#radialbarseries-radial-bar-chart)
- [Common Properties](#common-properties)
- [Semi-Circular Charts](#semi-circular-charts)
- [Sizing Charts](#sizing-charts)

## PieSeries (Pie Chart)

Pie charts divide a circle into slices to show proportional data. Each slice represents a category's contribution to the whole.

### Basic Pie Chart

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:PieSeries ItemsSource="{Binding Data}" 
                     XBindingPath="Product" 
                     YBindingPath="SalesRate"/>
</chart:SfCircularChart>
```

**C#:**
```csharp
SfCircularChart chart = new SfCircularChart();

PieSeries series = new PieSeries
{
    ItemsSource = viewModel.Data,
    XBindingPath = "Product",
    YBindingPath = "SalesRate"
};

chart.Series.Add(series);
this.Content = chart;
```

### When to Use Pie Charts

- Showing proportions of a whole (percentages that add up to 100%)
- Comparing 3-7 categories (more segments become hard to distinguish)
- Displaying market share, budget allocation, or survey results
- When exact values are less important than relative proportions

## DoughnutSeries (Doughnut Chart)

Doughnut charts are similar to pie charts but with a circular hole in the center. The center can display additional information or remain empty for aesthetic purposes.

### Basic Doughnut Chart

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:DoughnutSeries ItemsSource="{Binding Data}" 
                          XBindingPath="Product" 
                          YBindingPath="SalesRate"/>
</chart:SfCircularChart>
```

**C#:**
```csharp
SfCircularChart chart = new SfCircularChart();

DoughnutSeries series = new DoughnutSeries
{
    ItemsSource = viewModel.Data,
    XBindingPath = "Product",
    YBindingPath = "SalesRate"
};

chart.Series.Add(series);
this.Content = chart;
```

### Inner Radius

The `InnerRadius` property defines the size of the center hole. Values range from 0 to 1 (default is 0.4).

**XAML:**
```xml
<chart:DoughnutSeries ItemsSource="{Binding Data}"
                      XBindingPath="Product"
                      YBindingPath="SalesRate"
                      InnerRadius="0.7"/>
```

**C#:**
```csharp
DoughnutSeries series = new DoughnutSeries
{
    ItemsSource = viewModel.Data,
    XBindingPath = "Product",
    YBindingPath = "SalesRate",
    InnerRadius = 0.7
};
```

- **InnerRadius = 0.4** (default): Balanced appearance
- **InnerRadius = 0.6-0.7**: Thin ring, more center space
- **InnerRadius = 0.2-0.3**: Thick ring, less center space

### Center View

Display custom content in the center of the doughnut using the `CenterView` property.

**XAML:**
```xml
<chart:DoughnutSeries ItemsSource="{Binding Data}"
                      XBindingPath="Name"
                      YBindingPath="Value">
    <chart:DoughnutSeries.CenterView>
        <StackLayout>
            <Label Text="Total Sales" 
                   FontAttributes="Bold"
                   HorizontalOptions="Center"/>
            <Label Text="{Binding TotalValue}" 
                   FontSize="24"
                   HorizontalOptions="Center"/>
        </StackLayout>
    </chart:DoughnutSeries.CenterView>
</chart:DoughnutSeries>
```

**C#:**
```csharp
DoughnutSeries series = new DoughnutSeries
{
    ItemsSource = data,
    XBindingPath = "Name",
    YBindingPath = "Value"
};

StackLayout centerContent = new StackLayout();
centerContent.Children.Add(new Label { Text = "Total Sales", FontAttributes = FontAttributes.Bold });
centerContent.Children.Add(new Label { Text = "357,580", FontSize = 24 });

series.CenterView = centerContent;
```

### CenterHoleSize Property

The `CenterHoleSize` property returns the diameter of the center hole in pixels, useful for sizing center content proportionally.

**XAML:**
```xml
<chart:DoughnutSeries ItemsSource="{Binding Data}"
                      XBindingPath="Name"
                      YBindingPath="Value">
    <chart:DoughnutSeries.CenterView>
        <Border HeightRequest="{Binding CenterHoleSize}" 
                WidthRequest="{Binding CenterHoleSize}"
                StrokeShape="RoundRectangle 200">
            <StackLayout>
                <Label Text="Total :"/>
                <Label Text="357,580 km²"/>
            </StackLayout>
        </Border>
    </chart:DoughnutSeries.CenterView>
</chart:DoughnutSeries>
```

### When to Use Doughnut Charts

- When you need to display summary information in the center
- For multiple concentric rings of data (nested doughnuts)
- When you want a more modern aesthetic than pie charts
- To reduce visual weight and improve readability with many segments

## RadialBarSeries (Radial Bar Chart)

Radial bar charts display each data point as a separate circular progress bar, useful for comparing values across categories or showing progress metrics.

### Basic Radial Bar Chart

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:RadialBarSeries ItemsSource="{Binding Data}" 
                           XBindingPath="Product" 
                           YBindingPath="SalesRate"/>
</chart:SfCircularChart>
```

**C#:**
```csharp
SfCircularChart chart = new SfCircularChart();

RadialBarSeries series = new RadialBarSeries
{
    ItemsSource = viewModel.Data,
    XBindingPath = "Product",
    YBindingPath = "SalesRate"
};

chart.Series.Add(series);
this.Content = chart;
```

### Key Radial Bar Properties

#### InnerRadius

Sets the starting point of the radial bars (default: 0.4).

```csharp
RadialBarSeries series = new RadialBarSeries
{
    InnerRadius = 0.2  // Bars start closer to center
};
```

#### MaximumValue

Defines the maximum value for the radial bar range. By default, it uses the maximum Y-value from the data.

```csharp
RadialBarSeries series = new RadialBarSeries
{
    MaximumValue = 100  // All bars scale to 100
};
```

#### GapRatio

Controls spacing between radial bars (default: 0.2, range: 0-1).

**XAML:**
```xml
<chart:RadialBarSeries ItemsSource="{Binding Data}"
                       XBindingPath="Product"
                       YBindingPath="SalesRate"
                       GapRatio="0.4"/>
```

- **GapRatio = 0.1**: Minimal spacing, bars closer together
- **GapRatio = 0.4**: Wider spacing between bars
- **GapRatio = 0.6**: Very wide spacing

#### CapStyle

Specifies the shape of bar endpoints (default: BothFlat).

**Options:**
- `BothFlat`: Flat on both ends
- `BothCurve`: Rounded on both ends
- `StartCurve`: Rounded at start, flat at end
- `EndCurve`: Flat at start, rounded at end

**XAML:**
```xml
<chart:RadialBarSeries ItemsSource="{Binding Data}"
                       XBindingPath="Product"
                       YBindingPath="SalesRate"
                       CapStyle="BothCurve"/>
```

**C#:**
```csharp
RadialBarSeries series = new RadialBarSeries
{
    CapStyle = CapStyle.BothCurve
};
```

### Track Customization

Radial bars display a background track showing the full range. Customize it with these properties:

**XAML:**
```xml
<chart:RadialBarSeries ItemsSource="{Binding Data}"
                       XBindingPath="Product"
                       YBindingPath="SalesRate"
                       TrackFill="#FFF7ED"
                       TrackStroke="#FED7AA"
                       TrackStrokeWidth="1"/>
```

**C#:**
```csharp
RadialBarSeries series = new RadialBarSeries
{
    TrackFill = new SolidColorBrush(Color.FromArgb("#FFF7ED")),
    TrackStroke = new SolidColorBrush(Color.FromArgb("#FED7AA")),
    TrackStrokeWidth = 1
};
```

### Center View for Radial Bar

Similar to doughnut charts, radial bars support center content:

**XAML:**
```xml
<chart:RadialBarSeries ItemsSource="{Binding Data}"
                       XBindingPath="Product"
                       YBindingPath="SalesRate"
                       MaximumValue="100"
                       CapStyle="BothCurve">
    <chart:RadialBarSeries.CenterView>
        <StackLayout HeightRequest="{Binding CenterHoleSize}"
                     WidthRequest="{Binding CenterHoleSize}">
            <Image Source="person.png"/>
            <Label Text="Performance" HorizontalOptions="Center"/>
        </StackLayout>
    </chart:RadialBarSeries.CenterView>
</chart:RadialBarSeries>
```

### When to Use Radial Bar Charts

- Displaying KPIs or performance metrics (completion percentages)
- Comparing multiple metrics at once (skill levels, ratings)
- Progress tracking across categories
- When you want to emphasize individual values rather than proportions

## Common Properties

These properties apply to all circular chart types:

### Radius

Controls the overall size of the chart (default: 0.8, range: 0-1).

**XAML:**
```xml
<chart:PieSeries ItemsSource="{Binding Data}"
                 XBindingPath="Product"
                 YBindingPath="SalesRate"
                 Radius="0.9"/>
```

**C#:**
```csharp
series.Radius = 0.5;  // Smaller chart (50% of available space)
```

- **Radius = 0.8** (default): Leaves margin around chart
- **Radius = 1.0**: Chart fills entire area
- **Radius = 0.5**: Half-size chart with more margin

## Semi-Circular Charts

Create semi-circular or quarter-circular charts using `StartAngle` and `EndAngle` properties (measured in degrees, 0-360).

### Semi-Circle (180°)

**XAML:**
```xml
<chart:PieSeries ItemsSource="{Binding Data}"
                 XBindingPath="Product"
                 YBindingPath="SalesRate"
                 StartAngle="180"
                 EndAngle="360"/>
```

**C#:**
```csharp
PieSeries series = new PieSeries
{
    StartAngle = 180,  // Start from left
    EndAngle = 360     // End at right (bottom half)
};
```

### Quarter Circle (90°)

```csharp
series.StartAngle = 0;    // Start from top
series.EndAngle = 90;     // End at right (top-right quarter)
```

### Three-Quarter Circle (270°)

```csharp
series.StartAngle = 0;
series.EndAngle = 270;
```

### Angle Reference Points

- **0°/360°**: Top center
- **90°**: Right center
- **180°**: Bottom center
- **270°**: Left center

### Semi-Circular Examples for Each Type

**Semi-Doughnut:**
```xml
<chart:DoughnutSeries ItemsSource="{Binding Data}"
                      XBindingPath="Product"
                      YBindingPath="SalesRate"
                      StartAngle="180"
                      EndAngle="360"
                      InnerRadius="0.6"/>
```

**Semi-Radial Bar:**
```xml
<chart:RadialBarSeries ItemsSource="{Binding Data}"
                       XBindingPath="Product"
                       YBindingPath="SalesRate"
                       StartAngle="180"
                       EndAngle="360"/>
```

## Sizing Charts

### Controlling Chart Size with Radius

```csharp
// Large chart
series.Radius = 0.95;

// Medium chart (default)
series.Radius = 0.8;

// Small chart
series.Radius = 0.5;
```

### Combining Radius with Inner Radius (Doughnut/Radial Bar)

```csharp
DoughnutSeries series = new DoughnutSeries
{
    Radius = 0.9,        // Outer size
    InnerRadius = 0.5    // Inner hole size
};
```

## Choosing the Right Chart Type

| Chart Type | Best For | Avoid When |
|------------|----------|------------|
| **Pie** | Simple proportions, 3-7 categories | Many categories (>7), need for center content |
| **Doughnut** | Displaying center information, modern aesthetic | Very simple data (pie is sufficient) |
| **Radial Bar** | Multiple KPIs, progress tracking, comparisons | Part-to-whole relationships (use pie/doughnut) |

## Complete Examples

### Pie Chart with Custom Radius

```xml
<chart:SfCircularChart>
    <chart:PieSeries ItemsSource="{Binding Data}"
                     XBindingPath="Category"
                     YBindingPath="Value"
                     Radius="0.85"
                     ShowDataLabels="True"/>
</chart:SfCircularChart>
```

### Doughnut with Center Summary

```xml
<chart:SfCircularChart>
    <chart:DoughnutSeries ItemsSource="{Binding Data}"
                          XBindingPath="Category"
                          YBindingPath="Value"
                          InnerRadius="0.6"
                          Radius="0.9">
        <chart:DoughnutSeries.CenterView>
            <VerticalStackLayout HorizontalOptions="Center">
                <Label Text="Total Revenue" FontSize="12"/>
                <Label Text="$1.2M" FontSize="24" FontAttributes="Bold"/>
            </VerticalStackLayout>
        </chart:DoughnutSeries.CenterView>
    </chart:DoughnutSeries>
</chart:SfCircularChart>
```

### Radial Bar with Track and Curved Caps

```csharp
RadialBarSeries series = new RadialBarSeries
{
    ItemsSource = data,
    XBindingPath = "Metric",
    YBindingPath = "Score",
    MaximumValue = 100,
    InnerRadius = 0.3,
    GapRatio = 0.3,
    CapStyle = CapStyle.BothCurve,
    TrackFill = new SolidColorBrush(Colors.LightGray),
    TrackStroke = new SolidColorBrush(Colors.Gray),
    TrackStrokeWidth = 1
};
```

### Semi-Circular Gauge

```xml
<chart:SfCircularChart>
    <chart:RadialBarSeries ItemsSource="{Binding Data}"
                           XBindingPath="Metric"
                           YBindingPath="Value"
                           StartAngle="180"
                           EndAngle="360"
                           MaximumValue="100"
                           CapStyle="BothCurve"
                           InnerRadius="0.5"/>
</chart:SfCircularChart>
```

## Summary

- **PieSeries**: Standard circular charts for showing proportions
- **DoughnutSeries**: Pie charts with center hole for additional content
- **RadialBarSeries**: Circular progress bars for multi-metric comparisons
- **Radius**: Controls overall chart size (0-1)
- **InnerRadius**: Controls center hole size for doughnut/radial bar (0-1)
- **StartAngle/EndAngle**: Create semi-circular or custom arc charts
- **CenterView**: Display content in the center of doughnut/radial bar
