# Bubbles in .NET MAUI Maps (SfMaps)

## Table of Contents
- [Overview](#overview)
- [Enable Bubbles](#enable-bubbles)
- [Bubble Tooltips](#bubble-tooltips)
- [Bubble Color Customization](#bubble-color-customization)
- [Appearance Customization](#appearance-customization)
- [Key APIs Used](#key-apis-used)
- [API Summary](#api-summary)

## Overview

Bubbles can be rendered in different colors and sizes based on the data values of their assigned shape. You can add information to shapes such as population density, number of users, and more.

**Use bubbles for:**
- Population density visualization
- Sales/revenue by region
- Statistical data representation
- Comparative analysis across regions
- Multi-variable geographic data

**Key features:**
- Size proportional to data values
- Color mapping for additional dimensions
- Customizable appearance properties
- Tooltip support
- Hover effects

## Enable Bubbles

You can enable bubbles using the `ShowBubbles` property. You can customize bubbles using the `BubbleSettings` property. The `SizeValuePath` property is used to specify the value based on which the bubble's size has to be rendered.

### XAML Implementation

```xaml
<map:SfMaps>
    <map:SfMaps.Layer>
        <map:MapShapeLayer ShapesSource="url"
                           DataSource="{Binding Data}"
                           PrimaryValuePath="State" 
                           ShapeDataField="name" 
                           ShowBubbles="True">

            <map:MapShapeLayer.BubbleSettings>
                <map:MapBubbleSettings ColorValuePath="Population" 
                                       SizeValuePath="Population" 
                                       Fill="DarkViolet"
                                       MinSize="30"
                                       MaxSize="80">
                </map:MapBubbleSettings>
            </map:MapShapeLayer.BubbleSettings>
        </map:MapShapeLayer>
    </map:SfMaps.Layer>
</map:SfMaps>
```

### C# Implementation

```csharp
using Syncfusion.Maui.Maps;
using System.Collections.ObjectModel;

namespace MauiMapsExample;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        ViewModel viewModel = new ViewModel();
        this.BindingContext = viewModel;
        
        SfMaps maps = new SfMaps();
        MapShapeLayer layer = new MapShapeLayer();
        layer.ShapesSource = MapSource.FromUri("url"); // For e.g "https://cdn.syncfusion.com/maps/map-data/world-map.json"
        layer.DataSource = viewModel.Data;
        layer.PrimaryValuePath = "State";
        layer.ShapeDataField = "name";
        layer.ShowBubbles = true;

        MapBubbleSettings bubbleSetting = new MapBubbleSettings()
        {
            ColorValuePath = "Population",
            SizeValuePath = "Population",
            Fill = Colors.DarkViolet,
            MinSize = 30,
            MaxSize = 80
        };

        layer.BubbleSettings = bubbleSetting;
        maps.Layer = layer;
        this.Content = maps;
    }
}

public class ViewModel
{
    public ObservableCollection<Model> Data { get; set; }
    
    public ViewModel()
    {
        Data = new ObservableCollection<Model>();
        Data.Add(new Model("India", 21));
        Data.Add(new Model("United States", 58));
        Data.Add(new Model("Kazakhstan", 41));
        Data.Add(new Model("Italy", 48));
        Data.Add(new Model("Korea", 14));
        Data.Add(new Model("China", 23));
    }
}

public class Model
{
    public string State { get; set; }
    public int Population { get; set; }
    
    public Model(string state, int population)
    {
        State = state;
        Population = population;
    }
}
```

### Key APIs Used

| API | Type | Description |
|-----|------|-------------|
| `MapShapeLayer.ShowBubbles` | Property | Enables or disables bubble rendering on the map |
| `MapShapeLayer.BubbleSettings` | Property | Gets or sets the MapBubbleSettings for customizing bubbles |
| `MapBubbleSettings.SizeValuePath` | Property | Specifies the property path for bubble size values |
| `MapBubbleSettings.ColorValuePath` | Property | Specifies the property path for bubble color values |
| `MapBubbleSettings.MinSize` | Property | Sets the minimum bubble radius (default: 20.0) |
| `MapBubbleSettings.MaxSize` | Property | Sets the maximum bubble radius (default: 50.0) |
| `MapBubbleSettings.Fill` | Property | Sets the background color of bubbles |

## Bubble Tooltips

You can enable tooltip for the bubbles using the `ShowBubbleTooltip` property. It can be used to clearly indicate the information about the currently interacted bubble.

### XAML Implementation

```xaml
<map:SfMaps>
    <map:SfMaps.Layer>
        <map:MapShapeLayer ShapesSource="url"
                           DataSource="{Binding Data}"
                           PrimaryValuePath="State" 
                           ShapeDataField="name" 
                           ShapeHoverFill="Transparent" 
                           ShapeHoverStroke="Transparent"
                           ShowBubbles="True"
                           ShowBubbleTooltip="True">

            <map:MapShapeLayer.BubbleSettings>
                <map:MapBubbleSettings ColorValuePath="Population" 
                                       SizeValuePath="Population" 
                                       Fill="DarkViolet"
                                       MinSize="30"
                                       MaxSize="80">
                </map:MapBubbleSettings>
            </map:MapShapeLayer.BubbleSettings>
        </map:MapShapeLayer>
    </map:SfMaps.Layer>
</map:SfMaps>
```

### C# Implementation

```csharp
using Syncfusion.Maui.Maps;
using System.Collections.ObjectModel;

namespace MauiMapsExample;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        ViewModel viewModel = new ViewModel();
        this.BindingContext = viewModel;
        
        SfMaps maps = new SfMaps();
        MapShapeLayer layer = new MapShapeLayer();
        layer.ShapesSource = MapSource.FromUri("url"); // For e.g "https://cdn.syncfusion.com/maps/map-data/world-map.json"
        layer.DataSource = viewModel.Data;
        layer.PrimaryValuePath = "State";
        layer.ShapeDataField = "name";
        layer.ShapeHoverFill = Colors.Transparent;
        layer.ShapeHoverStroke = Colors.Transparent;
        layer.ShowBubbles = true;
        layer.ShowBubbleTooltip = true;

        MapBubbleSettings bubbleSetting = new MapBubbleSettings()
        {
            ColorValuePath = "Population",
            SizeValuePath = "Population",
            Fill = Colors.DarkViolet,
            MinSize = 30,
            MaxSize = 80
        };

        layer.BubbleSettings = bubbleSetting;
        maps.Layer = layer;
        this.Content = maps;
    }
}

public class ViewModel
{
    public ObservableCollection<Model> Data { get; set; }
    
    public ViewModel()
    {
        Data = new ObservableCollection<Model>();
        Data.Add(new Model("India", 21));
        Data.Add(new Model("United States", 58));
        Data.Add(new Model("Kazakhstan", 41));
        Data.Add(new Model("Italy", 48));
        Data.Add(new Model("Korea", 14));
        Data.Add(new Model("China", 23));
    }
}

public class Model
{
    public string State { get; set; }
    public int Population { get; set; }
    
    public Model(string state, int population)
    {
        State = state;
        Population = population;
    }
}
```

### Key APIs Used

| API | Type | Description |
|-----|------|-------------|
| `MapShapeLayer.ShowBubbleTooltip` | Property | Enables or disables tooltip for bubbles on hover/tap |
| `MapShapeLayer.ShapeHoverFill` | Property | Sets the hover fill color for shapes (use Transparent to avoid interference) |
| `MapShapeLayer.ShapeHoverStroke` | Property | Sets the hover stroke color for shapes |

## Bubble Color Customization

You can customize the bubble color based on the value from the `ColorValuePath` property. If it provides direct color value then it applies to bubbles straightaway. Otherwise, you must provide `ColorMappings`.

The value from the `ColorValuePath` will be used for comparison in the `EqualColorMapping.Value` or `RangeColorMapping.From` and `RangeColorMapping.To`. Then, the `ColorMapping.Color` will be applied to the respective bubble.

### Equal Color Mapping

```xaml
<map:SfMaps>
    <map:SfMaps.Layer>
        <map:MapShapeLayer ShapesSource="url"
                           ShowDataLabels="True"
                           DataSource="{Binding Data}" 
                           PrimaryValuePath="State" 
                           ShapeDataField="name" 
                           ShowBubbles="True">
            
            <map:MapShapeLayer.BubbleSettings>
                <map:MapBubbleSettings ColorValuePath="Population" 
                                       SizeValuePath="Population" 
                                       MinSize="30"
                                       MaxSize="80">
                    <map:MapBubbleSettings.ColorMappings>
                        <map:EqualColorMapping Color="DarkViolet" Value="51"/>
                        <map:EqualColorMapping Color="Orange" Value="58"/>
                        <map:EqualColorMapping Color="Yellow" Value="41"/>
                        <map:EqualColorMapping Color="LightGreen" Value="48"/>
                        <map:EqualColorMapping Color="Green" Value="14"/>
                        <map:EqualColorMapping Color="Aqua" Value="23"/>
                    </map:MapBubbleSettings.ColorMappings>
                </map:MapBubbleSettings>
            </map:MapShapeLayer.BubbleSettings>

            <map:MapShapeLayer.DataLabelSettings>
                <map:MapDataLabelSettings DataLabelPath="State"
                                          OverflowMode="None">
                    <map:MapDataLabelSettings.DataLabelStyle>
                        <map:MapLabelStyle FontSize="12" 
                                           TextColor="Red" 
                                           FontAttributes="Italic"/>
                    </map:MapDataLabelSettings.DataLabelStyle>
                </map:MapDataLabelSettings>
            </map:MapShapeLayer.DataLabelSettings>
        </map:MapShapeLayer>
    </map:SfMaps.Layer>
</map:SfMaps>
```

### C# Implementation with Color Mappings

```csharp
using Syncfusion.Maui.Maps;
using System.Collections.ObjectModel;

namespace MauiMapsExample;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        ViewModel viewModel = new ViewModel();
        this.BindingContext = viewModel;

        SfMaps maps = new SfMaps();
        MapShapeLayer layer = new MapShapeLayer();
        layer.ShapesSource = MapSource.FromUri("url"); // For e.g "https://cdn.syncfusion.com/maps/map-data/world-map.json"
        layer.DataSource = viewModel.Data;
        layer.PrimaryValuePath = "State";
        layer.ShapeDataField = "name";
        layer.ShowBubbles = true;
        layer.ShowDataLabels = true;

        MapBubbleSettings bubbleSetting = new MapBubbleSettings()
        {
            ColorValuePath = "Population",
            SizeValuePath = "Population",
            Opacity = 1,
            MinSize = 30,
            MaxSize = 80,
        };
        
        bubbleSetting.ColorMappings.Add(new EqualColorMapping()
        {
            Color = Colors.DarkViolet,
            Value = "21"
        });
        bubbleSetting.ColorMappings.Add(new EqualColorMapping()
        {
            Color = Colors.Orange,
            Value = "58"
        });
        bubbleSetting.ColorMappings.Add(new EqualColorMapping()
        {
            Color = Colors.Yellow,
            Value = "41"
        });
        bubbleSetting.ColorMappings.Add(new EqualColorMapping()
        {
            Color = Colors.LightGreen,
            Value = "48"
        });
        bubbleSetting.ColorMappings.Add(new EqualColorMapping()
        {
            Color = Colors.Green,
            Value = "14"
        });
        bubbleSetting.ColorMappings.Add(new EqualColorMapping()
        {
            Color = Colors.Aqua,
            Value = "23"
        });

        layer.BubbleSettings = bubbleSetting;
        layer.ShapeColorValuePath = "Population";

        layer.DataLabelSettings = new MapDataLabelSettings()
        {
            DataLabelPath = "State",
            OverflowMode = MapLabelOverflowMode.None,
            DataLabelStyle = new MapLabelStyle()
            {
                FontSize = 12,
                FontAttributes = FontAttributes.Italic,
                TextColor = Colors.Red
            },
        };

        maps.Layer = layer;
        this.Content = maps;
    }
}
    
public class ViewModel
{
    public ObservableCollection<Model> Data { get; set; }
    
    public ViewModel()
    {
        Data = new ObservableCollection<Model>();
        Data.Add(new Model("India", 21));
        Data.Add(new Model("United States", 58));
        Data.Add(new Model("Kazakhstan", 41));
        Data.Add(new Model("Italy", 48));
        Data.Add(new Model("Korea", 14));
        Data.Add(new Model("China", 23));
    }
}

public class Model
{
    public string State { get; set; }
    public int Population { get; set; }
    
    public Model(string state, int population)
    {
        State = state;
        Population = population;
    }
}
```

### Key APIs Used

| API | Type | Description |
|-----|------|-------------|
| `MapBubbleSettings.ColorValuePath` | Property | Specifies the property path for bubble color values |
| `MapBubbleSettings.ColorMappings` | Collection | Collection of color mappings for customizing bubble colors |
| `EqualColorMapping` | Class | Maps specific values to colors for bubbles |
| `EqualColorMapping.Value` | Property | The value to match from ColorValuePath |
| `EqualColorMapping.Color` | Property | The color to apply when value matches |
| `RangeColorMapping` | Class | Maps value ranges to colors for bubbles |
| `RangeColorMapping.From` | Property | The start of the value range |
| `RangeColorMapping.To` | Property | The end of the value range |

## Appearance Customization

You can customize the appearance of bubbles using various properties available in `MapBubbleSettings`.

### Customizable Properties

- **MinSize** - Change the minimum radius of the bubbles using the `MinSize` property. The default value is `20.0`.
- **MaxSize** - Change the maximum radius of the bubbles using the `MaxSize` property. The default value is `50.0`.
- **Fill** - Change the background color of the bubbles using the `Fill` property.
- **Stroke** - Change the stroke color of the bubbles using the `Stroke` property.
- **StrokeThickness** - Change the stroke width of the bubbles using the `StrokeThickness` property.
- **HoverFill** - Change the hover color of the bubbles using the `HoverFill` property.
- **HoverStroke** - Change the hover stroke color of the bubbles using the `HoverStroke` property.
- **HoverStrokeThickness** - Change the hover stroke thickness of the bubbles using the `HoverStrokeThickness` property.

### XAML Implementation

```xaml
<map:SfMaps>
    <map:SfMaps.Layer>
        <map:MapShapeLayer ShapesSource="url"
                           DataSource="{Binding Data}"
                           PrimaryValuePath="State" 
                           ShapeDataField="name"
                           ShapeHoverFill="Transparent" 
                           ShapeHoverStroke="Transparent"
                           ShowBubbles="True"
                           ShowBubbleTooltip="True">

            <map:MapShapeLayer.BubbleSettings>
                <map:MapBubbleSettings ColorValuePath="Population" 
                                       SizeValuePath="Population" 
                                       Fill="Green"
                                       Stroke="DarkGreen"
                                       StrokeThickness="2"
                                       HoverFill="Blue"
                                       HoverStroke="DarkBlue"
                                       HoverStrokeThickness="3"
                                       MinSize="30"
                                       MaxSize="80">
                </map:MapBubbleSettings>
            </map:MapShapeLayer.BubbleSettings>
        </map:MapShapeLayer>
    </map:SfMaps.Layer>
</map:SfMaps>
```

### C# Implementation

```csharp
using Syncfusion.Maui.Maps;
using System.Collections.ObjectModel;

namespace MauiMapsExample;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        ViewModel viewModel = new ViewModel();
        this.BindingContext = viewModel;
        
        SfMaps maps = new SfMaps();
        MapShapeLayer layer = new MapShapeLayer();
        layer.ShapesSource = MapSource.FromUri("url"); // For e.g "https://cdn.syncfusion.com/maps/map-data/world-map.json"
        layer.DataSource = viewModel.Data;
        layer.PrimaryValuePath = "State";
        layer.ShapeDataField = "name";
        layer.ShapeHoverFill = Colors.Transparent;
        layer.ShapeHoverStroke = Colors.Transparent;
        layer.ShowBubbles = true;
        layer.ShowBubbleTooltip = true;

        MapBubbleSettings bubbleSetting = new MapBubbleSettings()
        {
            ColorValuePath = "Population",
            SizeValuePath = "Population",
            Fill = Colors.Green,
            Stroke = Colors.DarkGreen,
            StrokeThickness = 2,
            HoverFill = Colors.Blue,
            HoverStroke = Colors.DarkBlue,
            HoverStrokeThickness = 3,
            MinSize = 30,
            MaxSize = 80
        };

        layer.BubbleSettings = bubbleSetting;
        maps.Layer = layer;
        this.Content = maps;
    }
}

public class ViewModel
{
    public ObservableCollection<Model> Data { get; set; }
    
    public ViewModel()
    {
        Data = new ObservableCollection<Model>();
        Data.Add(new Model("India", 21));
        Data.Add(new Model("United States", 58));
        Data.Add(new Model("Kazakhstan", 41));
        Data.Add(new Model("Italy", 48));
        Data.Add(new Model("Korea", 14));
        Data.Add(new Model("China", 23));
    }
}

public class Model
{
    public string State { get; set; }
    public int Population { get; set; }
    
    public Model(string state, int population)
    {
        State = state;
        Population = population;
    }
}
```

### Key APIs Used

| API | Type | Description |
|-----|------|-------------|
| `MapBubbleSettings.Fill` | Property | Sets the background color of bubbles |
| `MapBubbleSettings.Stroke` | Property | Sets the stroke (border) color of bubbles |
| `MapBubbleSettings.StrokeThickness` | Property | Sets the width of the bubble stroke |
| `MapBubbleSettings.HoverFill` | Property | Sets the fill color when hovering over bubbles |
| `MapBubbleSettings.HoverStroke` | Property | Sets the stroke color when hovering over bubbles |
| `MapBubbleSettings.HoverStrokeThickness` | Property | Sets the stroke thickness when hovering over bubbles |
| `MapBubbleSettings.Opacity` | Property | Sets the transparency level (0.0 to 1.0) |

## Key APIs Used

This section provides a comprehensive overview of all Syncfusion .NET MAUI SfMaps APIs used for bubble implementation.

### MapShapeLayer APIs

| API | Type | Description |
|-----|------|-------------|
| `ShowBubbles` | Property | Enables or disables bubble rendering on the map layer |
| `BubbleSettings` | Property | Gets or sets MapBubbleSettings for customizing bubble appearance |
| `ShowBubbleTooltip` | Property | Enables or disables tooltip display when hovering/tapping bubbles |

### MapBubbleSettings APIs

| API | Type | Description |
|-----|------|-------------|
| `SizeValuePath` | Property | Specifies the data property path for determining bubble sizes |
| `ColorValuePath` | Property | Specifies the data property path for determining bubble colors |
| `MinSize` | Property | Sets the minimum radius for bubbles (default: 20.0) |
| `MaxSize` | Property | Sets the maximum radius for bubbles (default: 50.0) |
| `Fill` | Property | Sets the background fill color of bubbles |
| `Stroke` | Property | Sets the stroke (border) color of bubbles |
| `StrokeThickness` | Property | Sets the width of the bubble stroke |
| `Opacity` | Property | Sets the transparency level of bubbles (0.0 to 1.0) |
| `HoverFill` | Property | Sets the fill color when hovering over bubbles |
| `HoverStroke` | Property | Sets the stroke color when hovering over bubbles |
| `HoverStrokeThickness` | Property | Sets the stroke thickness when hovering over bubbles |
| `ColorMappings` | Collection | Collection of color mappings (EqualColorMapping or RangeColorMapping) |

### Color Mapping APIs

| API | Type | Description |
|-----|------|-------------|
| `EqualColorMapping` | Class | Maps specific values to colors for bubbles |
| `EqualColorMapping.Value` | Property | The exact value to match from ColorValuePath |
| `EqualColorMapping.Color` | Property | The color to apply when the value matches |
| `RangeColorMapping` | Class | Maps value ranges to colors for bubbles |
| `RangeColorMapping.From` | Property | The start value of the range |
| `RangeColorMapping.To` | Property | The end value of the range |
| `RangeColorMapping.Color` | Property | The color to apply when value falls in range |

## API Summary

### Required Namespace

```csharp
using Syncfusion.Maui.Maps;
```

## Key Takeaways

✅ **Core Syncfusion Maps APIs:**
- `MapShapeLayer.ShowBubbles` - Property to enable bubble visualization on map shapes
- `MapBubbleSettings` - Configuration class for customizing bubble appearance and behavior
- `SizeValuePath` - Property specifying data field for proportional bubble sizing
- `ColorValuePath` - Property specifying data field for bubble coloring with color mappings
- `MinSize` / `MaxSize` - Properties defining bubble radius range (defaults: 20.0 / 50.0)

✅ **Essential Concepts:**
- Bubbles visualize data values as proportionally-sized circles on geographic shapes
- Size is driven by numeric data values via `SizeValuePath` property
- Colors can be applied directly from data or through `EqualColorMapping` / `RangeColorMapping`
- Bubbles support tooltips, hover effects, and opacity control for enhanced interactivity
- Work exclusively with `MapShapeLayer` (not tile layer) for shape-based data visualization

✅ **Implementation Patterns:**
- Enable bubbles: Set `ShowBubbles=true` and configure `BubbleSettings` with size/color paths
- Direct coloring: Use `ColorValuePath` with Color property in data model
- Equal mapping: Use `EqualColorMapping` for categorical data (High/Low, regions)
- Range mapping: Use `RangeColorMapping` for continuous numeric data (population ranges)
- Interactive bubbles: Enable `ShowBubbleTooltip` for hover/tap information display

✅ **Best Practices:**
- Set appropriate `MinSize`/`MaxSize` values based on map size and data range
- Ensure all data values are covered in color mappings to avoid unmapped bubbles
- Use `Fill`, `Stroke`, and `StrokeThickness` for consistent bubble appearance
- Apply `HoverFill` and `HoverStroke` for clear interactive feedback
- Set `ShapeHoverFill` / `ShapeHoverStroke` to Transparent to prevent shape hover interference
- Keep bubble color schemes consistent with data meaning (sequential, categorical, diverging)