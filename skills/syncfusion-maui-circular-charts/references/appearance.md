# Appearance Customization in .NET MAUI Circular Charts

Customize the visual appearance of circular charts using colors, gradients, strokes, and other styling options.

## Table of Contents
- [Custom Palette Brushes](#custom-palette-brushes)
- [Gradient Colors](#gradient-colors)
- [Stroke Customization](#stroke-customization)
- [Opacity](#opacity)
- [Radial Bar Specific Styling](#radial-bar-specific-styling)
- [Plotting Area Customization](#plotting-area-customization)

## Custom Palette Brushes

Define custom colors for chart segments using the `PaletteBrushes` property.

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:PieSeries ItemsSource="{Binding Data}"
                     XBindingPath="XValue"
                     YBindingPath="YValue"
                     PaletteBrushes="{Binding CustomBrushes}"/>
</chart:SfCircularChart>
```

**C# ViewModel:**
```csharp
public class ViewModel
{
    public ObservableCollection<Model> Data { get; set; }
    public List<Brush> CustomBrushes { get; set; }
    
    public ViewModel()
    {
        CustomBrushes = new List<Brush>
        {
            new SolidColorBrush(Color.FromRgb(38, 198, 218)),
            new SolidColorBrush(Color.FromRgb(0, 188, 212)),
            new SolidColorBrush(Color.FromRgb(0, 172, 193)),
            new SolidColorBrush(Color.FromRgb(0, 151, 167)),
            new SolidColorBrush(Color.FromRgb(0, 131, 143))
        };
    }
}
```

**Direct Assignment:**
```csharp
PieSeries series = new PieSeries();
series.PaletteBrushes = new List<Brush>
{
    new SolidColorBrush(Colors.Red),
    new SolidColorBrush(Colors.Blue),
    new SolidColorBrush(Colors.Green),
    new SolidColorBrush(Colors.Orange)
};
```

## Gradient Colors

Apply linear or radial gradients to chart segments.

### Linear Gradient

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:PieSeries ItemsSource="{Binding Data}"
                     XBindingPath="XValue"
                     YBindingPath="YValue"
                     PaletteBrushes="{Binding GradientBrushes}"/>
</chart:SfCircularChart>
```

**C# ViewModel:**
```csharp
public List<Brush> GradientBrushes { get; set; }

public ViewModel()
{
    GradientBrushes = new List<Brush>();
    
    // Gradient 1: Orange to Pink
    LinearGradientBrush gradient1 = new LinearGradientBrush();
    gradient1.GradientStops = new GradientStopCollection
    {
        new GradientStop { Offset = 1, Color = Color.FromRgb(255, 231, 199) },
        new GradientStop { Offset = 0, Color = Color.FromRgb(252, 182, 159) }
    };
    
    // Gradient 2: Yellow to Gold
    LinearGradientBrush gradient2 = new LinearGradientBrush();
    gradient2.GradientStops = new GradientStopCollection
    {
        new GradientStop { Offset = 1, Color = Color.FromRgb(250, 221, 125) },
        new GradientStop { Offset = 0, Color = Color.FromRgb(252, 204, 45) }
    };
    
    // Gradient 3: Purple to Pink
    LinearGradientBrush gradient3 = new LinearGradientBrush();
    gradient3.GradientStops = new GradientStopCollection
    {
        new GradientStop { Offset = 1, Color = Color.FromRgb(221, 214, 243) },
        new GradientStop { Offset = 0, Color = Color.FromRgb(250, 172, 168) }
    };
    
    // Gradient 4: Cyan to Blue
    LinearGradientBrush gradient4 = new LinearGradientBrush();
    gradient4.GradientStops = new GradientStopCollection
    {
        new GradientStop { Offset = 1, Color = Color.FromRgb(168, 234, 238) },
        new GradientStop { Offset = 0, Color = Color.FromRgb(123, 176, 249) }
    };
    
    GradientBrushes.Add(gradient1);
    GradientBrushes.Add(gradient2);
    GradientBrushes.Add(gradient3);
    GradientBrushes.Add(gradient4);
}
```

### Radial Gradient

```csharp
RadialGradientBrush radialGradient = new RadialGradientBrush();
radialGradient.Center = new Point(0.5, 0.5);
radialGradient.Radius = 0.8;
radialGradient.GradientStops = new GradientStopCollection
{
    new GradientStop { Offset = 0, Color = Colors.LightBlue },
    new GradientStop { Offset = 1, Color = Colors.DarkBlue }
};

series.PaletteBrushes = new List<Brush> { radialGradient };
```

## Stroke Customization

Add borders to chart segments using `Stroke` and `StrokeWidth`.

### Basic Stroke

**XAML:**
```xml
<chart:PieSeries ItemsSource="{Binding Data}"
                 XBindingPath="Product"
                 YBindingPath="Value"
                 Stroke="White"
                 StrokeWidth="2"/>
```

**C#:**
```csharp
PieSeries series = new PieSeries
{
    Stroke = Colors.White,
    StrokeWidth = 2
};
```

### Different Stroke Colors

```csharp
series.Stroke = new SolidColorBrush(Color.FromArgb("#333333"));
series.StrokeWidth = 1.5;
```

## Opacity

Control segment transparency using the `Opacity` property (0.0 to 1.0).

**XAML:**
```xml
<chart:PieSeries ItemsSource="{Binding Data}"
                 XBindingPath="Category"
                 YBindingPath="Value"
                 Opacity="0.8"/>
```

**C#:**
```csharp
series.Opacity = 0.8;  // 80% opaque, 20% transparent
```

- **Opacity = 1.0** (default): Fully opaque
- **Opacity = 0.5**: Semi-transparent
- **Opacity = 0.0**: Fully transparent (invisible)

## Radial Bar Specific Styling

RadialBarSeries has additional appearance properties.

### Gap Ratio

Control spacing between radial bars (default: 0.2, range: 0-1).

**XAML:**
```xml
<chart:RadialBarSeries ItemsSource="{Binding Data}"
                       XBindingPath="Metric"
                       YBindingPath="Score"
                       GapRatio="0.4"/>
```

**C#:**
```csharp
RadialBarSeries series = new RadialBarSeries
{
    GapRatio = 0.4  // 40% spacing between bars
};
```

### Maximum Value

Set the maximum value for radial bar range.

**XAML:**
```xml
<chart:RadialBarSeries MaximumValue="100"/>
```

**C#:**
```csharp
series.MaximumValue = 100;  // All bars scale to 100
```

### Cap Style

Define the shape of bar endpoints.

**XAML:**
```xml
<chart:RadialBarSeries CapStyle="BothCurve"/>
```

**C#:**
```csharp
series.CapStyle = CapStyle.BothCurve;
```

**Options:**
- `BothFlat`: Flat on both ends (default)
- `BothCurve`: Rounded on both ends
- `StartCurve`: Rounded at start, flat at end
- `EndCurve`: Flat at start, rounded at end

### Track Customization

Customize the background track for radial bars.

**XAML:**
```xml
<chart:RadialBarSeries TrackFill="#FFF7ED"
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

| Property | Description |
|----------|-------------|
| **TrackFill** | Background color of the track |
| **TrackStroke** | Border color of the track |
| **TrackStrokeWidth** | Border thickness of the track |

## Plotting Area Customization

Add custom views to the chart's plot area background.

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.PlotAreaBackgroundView>
        <AbsoluteLayout>
            <Border Stroke="Red"
                    StrokeThickness="2"
                    AbsoluteLayout.LayoutBounds="0,0,1,1"
                    AbsoluteLayout.LayoutFlags="All"/>
            
            <Label Text="Copyright @ 2001 - 2026 Syncfusion Inc"
                   FontSize="18"
                   AbsoluteLayout.LayoutBounds="1,1,-1,-1"
                   AbsoluteLayout.LayoutFlags="PositionProportional"
                   Opacity="0.4"/>
            
            <Label Text="CONFIDENTIAL"
                   Rotation="340"
                   FontSize="80"
                   FontAttributes="Bold,Italic"
                   TextColor="Gray"
                   Margin="10,0,0,0"
                   AbsoluteLayout.LayoutBounds="0.5,0.5,-1,-1"
                   AbsoluteLayout.LayoutFlags="PositionProportional"
                   Opacity="0.3"/>
        </AbsoluteLayout>
    </chart:SfCircularChart.PlotAreaBackgroundView>
</chart:SfCircularChart>
```

**C#:**
```csharp
SfCircularChart chart = new SfCircularChart();
AbsoluteLayout layout = new AbsoluteLayout();

// Add border
var border = new Border
{
    Stroke = Colors.Red,
    StrokeThickness = 2
};
AbsoluteLayout.SetLayoutBounds(border, new Rect(0, 0, 1, 1));
AbsoluteLayout.SetLayoutFlags(border, AbsoluteLayoutFlags.All);
layout.Children.Add(border);

// Add copyright
var copyright = new Label
{
    Text = "Copyright @ 2001 - 2026 Syncfusion Inc",
    FontSize = 18,
    Opacity = 0.4
};
AbsoluteLayout.SetLayoutBounds(copyright, new Rect(1, 1, -1, -1));
AbsoluteLayout.SetLayoutFlags(copyright, AbsoluteLayoutFlags.PositionProportional);
layout.Children.Add(copyright);

// Add watermark
var watermark = new Label
{
    Text = "CONFIDENTIAL",
    Rotation = 340,
    FontSize = 80,
    FontAttributes = FontAttributes.Bold,
    TextColor = Colors.Gray,
    Opacity = 0.3
};
AbsoluteLayout.SetLayoutBounds(watermark, new Rect(0.5, 0.5, -1, -1));
AbsoluteLayout.SetLayoutFlags(watermark, AbsoluteLayoutFlags.PositionProportional);
layout.Children.Add(watermark);

chart.PlotAreaBackgroundView = layout;
```

## Complete Examples

### Example 1: Pie Chart with Custom Colors and Strokes

```xml
<chart:SfCircularChart>
    <chart:PieSeries ItemsSource="{Binding Data}"
                     XBindingPath="Category"
                     YBindingPath="Value"
                     PaletteBrushes="{Binding CustomColors}"
                     Stroke="White"
                     StrokeWidth="3"
                     Opacity="0.9"/>
</chart:SfCircularChart>
```

### Example 2: Radial Bar with Styled Track

```csharp
RadialBarSeries series = new RadialBarSeries
{
    ItemsSource = data,
    XBindingPath = "Metric",
    YBindingPath = "Score",
    MaximumValue = 100,
    GapRatio = 0.3,
    CapStyle = CapStyle.BothCurve,
    TrackFill = new SolidColorBrush(Colors.LightGray),
    TrackStroke = new SolidColorBrush(Colors.Gray),
    TrackStrokeWidth = 1,
    Stroke = Colors.White,
    StrokeWidth = 2
};
```

### Example 3: Gradient Doughnut Chart

```csharp
DoughnutSeries series = new DoughnutSeries
{
    ItemsSource = data,
    XBindingPath = "Product",
    YBindingPath = "Sales",
    InnerRadius = 0.6
};

// Create gradient brushes
List<Brush> gradients = new List<Brush>();

for (int i = 0; i < 5; i++)
{
    LinearGradientBrush gradient = new LinearGradientBrush
    {
        StartPoint = new Point(0, 0),
        EndPoint = new Point(1, 1),
        GradientStops = new GradientStopCollection
        {
            new GradientStop { Offset = 0, Color = GetStartColor(i) },
            new GradientStop { Offset = 1, Color = GetEndColor(i) }
        }
    };
    gradients.Add(gradient);
}

series.PaletteBrushes = gradients;
```

## Best Practices

1. **Color Selection**: Choose colors with sufficient contrast for accessibility
2. **Gradients**: Use subtle gradients to avoid overwhelming the visualization
3. **Strokes**: Add white or light strokes to separate segments clearly
4. **Opacity**: Avoid very low opacity values that make segments hard to see
5. **Radial Bar Gaps**: Use 0.2-0.4 gap ratio for balanced spacing
6. **Track Colors**: Use muted track colors that don't compete with data colors
