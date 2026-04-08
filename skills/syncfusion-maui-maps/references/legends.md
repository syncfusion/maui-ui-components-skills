# Legends in .NET MAUI Maps (SfMaps)

## Table of Contents
- [Overview](#overview)
- [Key APIs Used](#key-apis-used)
- [API Summary](#api-summary)
- [Shape Legend](#shape-legend)
- [Bubble Legend](#bubble-legend)
- [Legend Text Customization](#legend-text-customization)
- [Legend Position](#legend-position)
- [Appearance Customization](#appearance-customization)
- [Customize Items Layout](#customize-items-layout)
- [Customize Items Template](#customize-items-template)
- [Best Practices](#best-practices)

## Overview

Using a legend, you can provide clear information on the data plotted on the map. The .NET MAUI Maps control provides powerful legend capabilities for both shape and bubble layers, allowing you to display color mappings with descriptive labels that help users understand what different colors or sizes represent on the map.

**Use legends for:**
- Explaining color-coded regions (choropleth maps)
- Describing bubble color/size meanings
- Providing data classification keys
- Enhancing map readability

**Supported legend types:**
- **Shape Legend:** Based on shape layer color mappings
- **Bubble Legend:** Based on bubble settings color mappings

## Key APIs Used

### Core Classes
- `MapLegend` - Represents the legend control for the map
- `MapShapeLayer.Legend` - Property to set the legend for shape layer
- `MapLabelStyle` - Defines text styling for legend labels
- `LegendSourceType` - Enumeration for legend source (Shape, Bubble)
- `LegendPlacement` - Enumeration for legend position (Top, Left, Right, Bottom)

### Properties
- `MapLegend.SourceType` - Specifies whether legend shows shape or bubble mappings
- `MapLegend.Placement` - Controls legend position on the map
- `MapLegend.TextStyle` - Customizes legend text appearance
- `MapLegend.IconType` - Sets legend icon shape
- `MapLegend.IconSize` - Defines size of legend icons
- `MapLegend.Spacing` - Controls spacing between legend items
- `MapLegend.Padding` - Sets padding around legend
- `MapLegend.ItemsLayout` - Custom layout for legend items
- `MapLegend.ItemTemplate` - Custom template for legend items
- `ColorMapping.Text` - Text displayed in legend for each color mapping

### Required Namespaces
```csharp
using Syncfusion.Maui.Maps;
using Syncfusion.Maui.Core;
using System.Collections.ObjectModel;
```

## Shape Legend

You can show shape legend by setting the `MapShapeLayer.Legend` property as `MapLegend` with `SourceType` as `Shape`. The legend item's default text is displayed based on the value of `ColorMappings.Text` property. The default value of the `Legend` property is `null` and hence the legend will not be shown by default.

### Key APIs Used
- `MapLegend` - Legend control class
- `MapShapeLayer.Legend` - Property to attach legend to layer
- `LegendSourceType.Shape` - Enumeration value for shape legend
- `MapLegend.Placement` - Property to set legend position
- `EqualColorMapping.Text` - Text displayed in legend

### Basic Shape Legend with EqualColorMapping

**XAML:**
```xaml
<map:SfMaps>
    <map:SfMaps.Layer>
        <map:MapShapeLayer DataSource="{Binding Data}"
                           ShapesSource="url"
                           PrimaryValuePath="State" 
                           ShapeDataField="name" 
                           ShapeStroke="DarkGrey">
                
            <map:MapShapeLayer.ColorMappings>
                <map:EqualColorMapping Color="LightGray"
                                       Value="51"
                                       Text="India" />
                <map:EqualColorMapping Color="LightGray"
                                       Value="58"
                                       Text="United States" />
                <map:EqualColorMapping Color="LightGray"
                                       Value="41"
                                       Text="Kazakhstan" />
                <map:EqualColorMapping Color="LightGray"
                                       Value="48"
                                       Text="Italy" />
                <map:EqualColorMapping Color="LightGray"
                                       Value="14"
                                       Text="Korea" />
                <map:EqualColorMapping Color="LightGray"
                                       Value="23"
                                       Text="China" />
            </map:MapShapeLayer.ColorMappings>
    
            <map:MapShapeLayer.Legend>
                <map:MapLegend SourceType="Shape"
                               Placement="Top" />
            </map:MapShapeLayer.Legend>
                
        </map:MapShapeLayer>
    </map:SfMaps.Layer>
</map:SfMaps>
```

**C# (.NET 9):**
```csharp
using Syncfusion.Maui.Maps;
using Syncfusion.Maui.Core;
using System.Collections.ObjectModel;

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
    layer.ShapeStroke = Colors.DarkGrey;

    layer.ColorMappings.Add(new EqualColorMapping()
    {
        Color = Colors.LightGray,
        Value = "51",
        Text = "India"
    });
    layer.ColorMappings.Add(new EqualColorMapping()
    {
        Color = Colors.LightGray,
        Value = "58",
        Text = "United States"
    });
    layer.ColorMappings.Add(new EqualColorMapping()
    {
        Color = Colors.LightGray,
        Value = "41",
        Text = "Kazakhstan"
    });
    layer.ColorMappings.Add(new EqualColorMapping()
    {
        Color = Colors.LightGray,
        Value = "48",
        Text = "Italy"
    });
    layer.ColorMappings.Add(new EqualColorMapping()
    {
        Color = Colors.LightGray,
        Value = "14",
        Text = "Korea"
    });
    layer.ColorMappings.Add(new EqualColorMapping()
    {
        Color = Colors.LightGray,
        Value = "23",
        Text = "China"
    });

    MapLegend legendSet = new MapLegend();
    legendSet.SourceType = LegendSourceType.Shape;
    legendSet.Placement = Syncfusion.Maui.Core.LegendPlacement.Top;
    layer.Legend = legendSet;

    maps.Layer = layer;
    this.Content = maps;
}

public class ViewModel
{
    public ObservableCollection<Model> Data { get; set; }
    
    public ViewModel()
    {
        Data = new ObservableCollection<Model>();
        Data.Add(new Model("India", 51));
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

**Legend text** comes from the `Text` property of color mappings:
```csharp
new EqualColorMapping 
{ 
    Color = Colors.LightGray,
    Value = "51",
    Text = "India"  // ← Shows in legend
}
```

## Bubble Legend

You can show bubble legend by setting the `MapShapeLayer.Legend` property as `SourceType.Bubble`. By default, the legend item's text is displayed based on the `ColorMapping.Text` value.

### Key APIs Used
- `MapLegend` - Legend control class
- `LegendSourceType.Bubble` - Enumeration value for bubble legend
- `MapBubbleSettings.ColorMappings` - Color mappings for bubbles
- `MapShapeLayer.ShowBubbles` - Property to enable bubble display
- `MapBubbleSettings` - Configuration for bubble appearance

### Basic Bubble Legend

**XAML:**
```xaml
<map:SfMaps>
    <map:SfMaps.Layer>
        <map:MapShapeLayer DataSource="{Binding Data}"  
                           ShapesSource="url"
                           ShowBubbles="True" 
                           PrimaryValuePath="State" 
                           ShapeDataField="name" 
                           ShapeStroke="DarkGray">
    
            <map:MapShapeLayer.BubbleSettings>
                <map:MapBubbleSettings ColorValuePath="Population"
                                       SizeValuePath="Population"
                                       MinSize="30"
                                       MaxSize="80">
    
                    <map:MapBubbleSettings.ColorMappings>
                        <map:EqualColorMapping Color="LightGreen"
                                               Value="21"
                                               Text="India" />
                        <map:EqualColorMapping Color="LightGreen"
                                               Value="58"
                                               Text="United States" />
                        <map:EqualColorMapping Color="LightGreen"
                                               Value="41"
                                               Text="Kazakhstan" />
                        <map:EqualColorMapping Color="LightGreen"
                                               Value="48"
                                               Text="Italy" />
                        <map:EqualColorMapping Color="LightGreen"
                                               Value="14"
                                               Text="Korea" />
                        <map:EqualColorMapping Color="LightGreen"
                                               Value="23"
                                               Text="China" />
                    </map:MapBubbleSettings.ColorMappings>
                </map:MapBubbleSettings>
            </map:MapShapeLayer.BubbleSettings>
                
            <map:MapShapeLayer.Legend>
                <map:MapLegend SourceType="Bubble" Placement="Top" />
            </map:MapShapeLayer.Legend>
                
        </map:MapShapeLayer>
    </map:SfMaps.Layer>
</map:SfMaps>
```

**C# (.NET 9):**
```csharp
using Syncfusion.Maui.Maps;
using Syncfusion.Maui.Core;
using System.Collections.ObjectModel;

public MainPage()
{
    InitializeComponent();
    ViewModel viewModel = new ViewModel();
    this.BindingContext = viewModel;

    MapShapeLayer layer = new MapShapeLayer();
    layer.DataSource = viewModel.Data;
    layer.PrimaryValuePath = "State";
    layer.ShapesSource = MapSource.FromUri("url"); // For e.g "https://cdn.syncfusion.com/maps/map-data/world-map.json"
    layer.ShapeDataField = "name";
    layer.ShowBubbles = true;
    layer.ShapeStroke = Colors.DarkGrey;
    
    MapBubbleSettings bubbleSetting = new MapBubbleSettings()
    {
        ColorValuePath = "Population",
        SizeValuePath = "Population",
        MinSize = 30,
        MaxSize = 80,
    };

    bubbleSetting.ColorMappings.Add(new EqualColorMapping()
    {
        Color = Colors.LightGreen,
        Value = "21",
        Text = "India"
    });
    bubbleSetting.ColorMappings.Add(new EqualColorMapping()
    {
        Color = Colors.LightGreen,
        Value = "58",
        Text = "United States"
    });
    bubbleSetting.ColorMappings.Add(new EqualColorMapping()
    {
        Color = Colors.LightGreen,
        Value = "41",
        Text = "Kazakhstan"
    });
    bubbleSetting.ColorMappings.Add(new EqualColorMapping()
    {
        Color = Colors.LightGreen,
        Value = "48",
        Text = "Italy"
    });
    bubbleSetting.ColorMappings.Add(new EqualColorMapping()
    {
        Color = Colors.LightGreen,
        Value = "14",
        Text = "Korea"
    });
    bubbleSetting.ColorMappings.Add(new EqualColorMapping()
    {
        Color = Colors.LightGreen,
        Value = "23",
        Text = "China"
    });

    layer.BubbleSettings = bubbleSetting;
    MapLegend legendSet = new MapLegend();
    legendSet.SourceType = LegendSourceType.Bubble;
    legendSet.Placement = Syncfusion.Maui.Core.LegendPlacement.Top;
    layer.Legend = legendSet;

    SfMaps maps = new SfMaps();
    maps.Layer = layer;
    this.Content = maps;
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

## Legend Text Customization

You can customize the legend item's text style using the `MapLegend.TextStyle` property.

### Key APIs Used
- `MapLegend.TextStyle` - Property to customize legend text
- `MapLabelStyle` - Class for text styling configuration
- `MapLabelStyle.FontSize` - Font size property
- `MapLabelStyle.TextColor` - Text color property
- `MapLabelStyle.FontFamily` - Font family property
- `MapLabelStyle.FontAttributes` - Font attributes (Italic, Bold)

### Text Style Customization Example

**XAML:**
```xaml
<map:SfMaps>
    <map:SfMaps.Layer>
        <map:MapShapeLayer DataSource="{Binding Data}"
                           ShapesSource="url"
                           PrimaryValuePath="State"
                           ShapeDataField="name"
                           ShapeStroke="DarkGray"
                           ShapeColorValuePath="Population">

            <map:MapShapeLayer.ColorMappings>
                <map:RangeColorMapping Color="Red"
                                       From="0"
                                       To="100"
                                       Text="0 - 100/km" />
                <map:RangeColorMapping Color="LightGreen"
                                       From="101"
                                       To="200"
                                       Text="100 - 200/km" />
                <map:RangeColorMapping Color="Blue"
                                       From="201"
                                       To="300"
                                       Text="200 - 300/km" />
                <map:RangeColorMapping Color="Orange"
                                       From="301"
                                       To="400"
                                       Text="300 - 400/km" />
                <map:RangeColorMapping Color="Teal"
                                       From="401"
                                       To="500"
                                       Text="400 - 500/km" />
                <map:RangeColorMapping Color="Purple"
                                       From="501"
                                       To="600"
                                       Text="500 - 600/km" />
            </map:MapShapeLayer.ColorMappings>

            <map:MapShapeLayer.Legend>
                <map:MapLegend SourceType="Shape"
                               Placement="Top">
                    <map:MapLegend.TextStyle>
                        <map:MapLabelStyle FontSize="16"
                                           TextColor="Black"
                                           FontFamily="Times"
                                           FontAttributes="Italic" />
                    </map:MapLegend.TextStyle>
                </map:MapLegend>
            </map:MapShapeLayer.Legend>

        </map:MapShapeLayer>
    </map:SfMaps.Layer>
</map:SfMaps>
```

**C# (.NET 9):**
```csharp
using Syncfusion.Maui.Maps;
using Syncfusion.Maui.Core;
using System.Collections.ObjectModel;

public MainPage()
{
    InitializeComponent();
    ViewModel viewModel = new ViewModel();
    this.BindingContext = viewModel;

    MapShapeLayer layer = new MapShapeLayer();
    layer.ShapesSource = MapSource.FromUri("url");
    layer.DataSource = viewModel.Data;
    layer.PrimaryValuePath = "State";
    layer.ShapeDataField = "name";
    layer.ShapeStroke = Colors.DarkGrey;
    layer.ShapeColorValuePath = "Population";

    layer.ColorMappings.Add(new RangeColorMapping() 
    {
        Color = Colors.Red,
        From = 0,
        To = 100,
        Text = "0 - 100/km"
    });
    layer.ColorMappings.Add(new RangeColorMapping() 
    {
        Color = Colors.LightGreen,
        From = 101,
        To = 200,
        Text = "100 - 200/km"
    });
    layer.ColorMappings.Add(new RangeColorMapping()
    { 
        Color = Colors.Blue,
        From = 201,
        To = 300,
        Text = "200 - 300/km"
    });
    layer.ColorMappings.Add(new RangeColorMapping()
    {
        Color = Colors.Orange,
        From = 301,
        To = 400,
        Text = "300 - 400/km"
    });
    layer.ColorMappings.Add(new RangeColorMapping()
    {
        Color = Colors.Teal,
        From = 401,
        To = 500,
        Text = "400 - 500/km"
    });
    layer.ColorMappings.Add(new RangeColorMapping()
    {
        Color = Colors.Purple,
        From = 501,
        To = 600,
        Text = "500 - 600/km"
    });

    MapLegend legendSet = new MapLegend();
    legendSet.SourceType = LegendSourceType.Shape;
    legendSet.Placement = Syncfusion.Maui.Core.LegendPlacement.Top;

    MapLabelStyle mapLabelStyle = new MapLabelStyle();
    mapLabelStyle.TextColor = Colors.Black;
    mapLabelStyle.FontSize = 16;
    mapLabelStyle.FontFamily = "Times";
    mapLabelStyle.FontAttributes = FontAttributes.Italic;
    
    legendSet.TextStyle = mapLabelStyle;
    layer.Legend = legendSet;

    SfMaps maps = new SfMaps();
    maps.Layer = layer;
    this.Content = maps;
}

public class ViewModel
{
    public ObservableCollection<Model> Data { get; set; }
    
    public ViewModel()
    {
        Data = new ObservableCollection<Model>();
        Data.Add(new Model("India", 205));
        Data.Add(new Model("United States", 190));
        Data.Add(new Model("Kazakhstan", 37));
        Data.Add(new Model("Italy", 201));
        Data.Add(new Model("Korea", 512));
        Data.Add(new Model("Japan", 335));
        Data.Add(new Model("Cuba", 103));
        Data.Add(new Model("China", 148));
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

## Legend Position

You can position the legend items in different directions using the `MapLegend.Placement` property. The default value of the `Placement` property is `Placement.Top`. The possible values are `Left`, `Right`, `Top`, and `Bottom`.

### Key APIs Used
- `MapLegend.Placement` - Property to set legend position
- `LegendPlacement.Top` - Positions legend at top (default)
- `LegendPlacement.Bottom` - Positions legend at bottom
- `LegendPlacement.Left` - Positions legend at left
- `LegendPlacement.Right` - Positions legend at right

### Placement Options

**XAML:**
```xaml
<map:SfMaps>
    <map:SfMaps.Layer>
        <map:MapShapeLayer ShapesSource="url"
                           DataSource="{Binding Data}"
                           PrimaryValuePath="State"
                           ShapeDataField="name"
                           ShapeStroke="DarkGray"
                           ShapeColorValuePath="Population">

            <map:MapShapeLayer.ColorMappings>
                <map:RangeColorMapping Color="Red"
                                       From="0"
                                       To="100"
                                       Text="0 - 100/km" />
                <map:RangeColorMapping Color="LightGreen"
                                       From="101"
                                       To="200"
                                       Text="100 - 200/km" />
                <map:RangeColorMapping Color="Blue"
                                       From="201"
                                       To="300"
                                       Text="200 - 300/km" />
                <map:RangeColorMapping Color="Orange"
                                       From="301"
                                       To="400"
                                       Text="300 - 400/km" />
                <map:RangeColorMapping Color="Teal"
                                       From="401"
                                       To="500"
                                       Text="400 - 500/km" />
                <map:RangeColorMapping Color="Purple"
                                       From="501"
                                       To="600"
                                       Text="500 - 600/km" />
            </map:MapShapeLayer.ColorMappings>

            <map:MapShapeLayer.Legend>
                <map:MapLegend SourceType="Shape"
                               Placement="Right" />
            </map:MapShapeLayer.Legend>

        </map:MapShapeLayer>
    </map:SfMaps.Layer>
</map:SfMaps>
```

**C# (.NET 9):**
```csharp
using Syncfusion.Maui.Maps;
using Syncfusion.Maui.Core;
using System.Collections.ObjectModel;

public MainPage()
{
    InitializeComponent();
    ViewModel viewModel = new ViewModel();
    this.BindingContext = viewModel;

    MapShapeLayer layer = new MapShapeLayer();
    layer.ShapesSource = MapSource.FromUri("url");
    layer.DataSource = viewModel.Data;
    layer.PrimaryValuePath = "State";
    layer.ShapeDataField = "name";
    layer.ShapeStroke = Colors.DarkGrey;
    layer.ShapeColorValuePath = "Population";

    layer.ColorMappings.Add(new RangeColorMapping() 
    {
        Color = Colors.Red,
        From = 0,
        To = 100,
        Text = "0 - 100/km"
    });
    layer.ColorMappings.Add(new RangeColorMapping() 
    {
        Color = Colors.LightGreen,
        From = 101,
        To = 200,
        Text = "100 - 200/km"
    });
    layer.ColorMappings.Add(new RangeColorMapping()
    { 
        Color = Colors.Blue,
        From = 201,
        To = 300,
        Text = "200 - 300/km"
    });
    layer.ColorMappings.Add(new RangeColorMapping()
    {
        Color = Colors.Orange,
        From = 301,
        To = 400,
        Text = "300 - 400/km"
    });
    layer.ColorMappings.Add(new RangeColorMapping()
    {
        Color = Colors.Teal,
        From = 401,
        To = 500,
        Text = "400 - 500/km"
    });
    layer.ColorMappings.Add(new RangeColorMapping()
    {
        Color = Colors.Purple,
        From = 501,
        To = 600,
        Text = "500 - 600/km"
    });

    MapLegend legendSet = new MapLegend();
    legendSet.SourceType = LegendSourceType.Shape;
    legendSet.Placement = Syncfusion.Maui.Core.LegendPlacement.Right;

    MapLabelStyle mapLabelStyle = new MapLabelStyle();
    mapLabelStyle.TextColor = Colors.Black;
    mapLabelStyle.FontSize = 16;
    mapLabelStyle.FontFamily = "Times";
    mapLabelStyle.FontAttributes = FontAttributes.Italic;
    
    legendSet.TextStyle = mapLabelStyle;
    layer.Legend = legendSet;

    SfMaps maps = new SfMaps();
    maps.Layer = layer;
    this.Content = maps;
}

public class ViewModel
{
    public ObservableCollection<Model> Data { get; set; }
    
    public ViewModel()
    {
        Data = new ObservableCollection<Model>();
        Data.Add(new Model("India", 205));
        Data.Add(new Model("United States", 190));
        Data.Add(new Model("Kazakhstan", 37));
        Data.Add(new Model("Italy", 201));
        Data.Add(new Model("Korea", 512));
        Data.Add(new Model("Japan", 335));
        Data.Add(new Model("Cuba", 103));
        Data.Add(new Model("China", 148));
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

**Recommendations:**
- **Top:** Default position, doesn't obscure map content
- **Bottom:** Good alternative horizontal layout
- **Right:** Traditional placement for vertical legends
- **Left:** Use when right side has other UI elements

## Appearance Customization

Customize the legend items using the `IconType` and `IconSize` properties.

### Key APIs Used
- `MapLegend.IconType` - Property to change icon shape
- `MapLegend.IconSize` - Property to change icon size
- `ShapeType` - Enumeration with values: Circle, Rectangle, Square, Diamond

**IconType:** Used to change the icon shape. The default value of the `IconType` property is `ShapeType.Rectangle`. The possible values are `Circle`, `Rectangle`, `Square`, and `Diamond`.

**IconSize:** Used to change the size of the icon. The default value of `IconSize` property is Size(12.0, 12.0).

### Icon Customization Example

**XAML:**
```xaml
<map:SfMaps>
    <map:SfMaps.Layer>
        <map:MapShapeLayer ShapesSource="url"
                           DataSource="{Binding Data}"
                           PrimaryValuePath="State"
                           ShapeDataField="name"
                           ShapeStroke="DarkGray"
                           ShapeColorValuePath="Population">

            <map:MapShapeLayer.ColorMappings>
                <map:RangeColorMapping Color="Red"
                                       From="0"
                                       To="100"
                                       Text="0 - 100/km" />
                <map:RangeColorMapping Color="LightGreen"
                                       From="101"
                                       To="200"
                                       Text="100 - 200/km" />
                <map:RangeColorMapping Color="Blue"
                                       From="201"
                                       To="300"
                                       Text="200 - 300/km" />
                <map:RangeColorMapping Color="Orange"
                                       From="301"
                                       To="400"
                                       Text="300 - 400/km" />
                <map:RangeColorMapping Color="Teal"
                                       From="401"
                                       To="500"
                                       Text="400 - 500/km" />
                <map:RangeColorMapping Color="Purple"
                                       From="501"
                                       To="600"
                                       Text="500 - 600/km" />
            </map:MapShapeLayer.ColorMappings>

            <map:MapShapeLayer.Legend>
                <map:MapLegend SourceType="Shape"
                               Placement="Top"
                               IconSize="20,20"
                               IconType="Diamond" />
            </map:MapShapeLayer.Legend>

        </map:MapShapeLayer>
    </map:SfMaps.Layer>
</map:SfMaps>
```

**C# (.NET 9):**
```csharp
using Syncfusion.Maui.Maps;
using Syncfusion.Maui.Core;
using System.Collections.ObjectModel;

public MainPage()
{
    InitializeComponent();
    ViewModel viewModel = new ViewModel();
    this.BindingContext = viewModel;

    MapShapeLayer layer = new MapShapeLayer();
    layer.ShapesSource = MapSource.FromUri("url");
    layer.DataSource = viewModel.Data;
    layer.PrimaryValuePath = "State";
    layer.ShapeDataField = "name";
    layer.ShapeStroke = Colors.DarkGrey;
    layer.ShapeColorValuePath = "Population";

    layer.ColorMappings.Add(new RangeColorMapping() 
    {
        Color = Colors.Red,
        From = 0,
        To = 100,
        Text = "0 - 100/km"
    });
    layer.ColorMappings.Add(new RangeColorMapping() 
    {
        Color = Colors.LightGreen,
        From = 101,
        To = 200,
        Text = "100 - 200/km"
    });
    layer.ColorMappings.Add(new RangeColorMapping()
    { 
        Color = Colors.Blue,
        From = 201,
        To = 300,
        Text = "200 - 300/km"
    });
    layer.ColorMappings.Add(new RangeColorMapping()
    {
        Color = Colors.Orange,
        From = 301,
        To = 400,
        Text = "300 - 400/km"
    });
    layer.ColorMappings.Add(new RangeColorMapping()
    {
        Color = Colors.Teal,
        From = 401,
        To = 500,
        Text = "400 - 500/km"
    });
    layer.ColorMappings.Add(new RangeColorMapping()
    {
        Color = Colors.Purple,
        From = 501,
        To = 600,
        Text = "500 - 600/km"
    });

    MapLegend legendSet = new MapLegend();
    legendSet.SourceType = LegendSourceType.Shape;
    legendSet.Placement = Syncfusion.Maui.Core.LegendPlacement.Top;
    legendSet.IconSize = new Size(20, 20);
    legendSet.IconType = Syncfusion.Maui.Core.ShapeType.Diamond;
    
    layer.Legend = legendSet;

    SfMaps maps = new SfMaps();
    maps.Layer = layer;
    this.Content = maps;
}

public class ViewModel
{
    public ObservableCollection<Model> Data { get; set; }
    
    public ViewModel()
    {
        Data = new ObservableCollection<Model>();
        Data.Add(new Model("India", 205));
        Data.Add(new Model("United States", 190));
        Data.Add(new Model("Kazakhstan", 37));
        Data.Add(new Model("Italy", 201));
        Data.Add(new Model("Korea", 512));
        Data.Add(new Model("Japan", 335));
        Data.Add(new Model("Cuba", 103));
        Data.Add(new Model("China", 148));
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

**Icon Type Options:**
| Icon Type | Description |
|-----------|-------------|
| `Rectangle` | Default rectangular icon shape |
| `Circle` | Circular icon shape |
| `Square` | Square icon shape |
| `Diamond` | Diamond-shaped icon |

**Icon Size Recommendations:**
- Small (12×12): Default, compact legends with many items
- Medium (16×16-20×20): Better visibility, good balance
- Large (24×24+): Emphasis, better accessibility

## Customize Items Layout

The `ItemsLayout` property is used to customize the arrangement and position of each legend item. The default value is null. This property accepts any layout type.

### Key APIs Used
- `MapLegend.ItemsLayout` - Property to set custom layout for legend items
- `FlexLayout` - Layout type for flexible arrangement
- `StackLayout` - Layout type for linear arrangement
- Layout properties: `Margin`, `Background`, `HorizontalOptions`

### Items Layout Example

**XAML:**
```xaml
<map:SfMaps x:Name="maps">
    <map:SfMaps.BindingContext>
        <local:LegendViewModel />
    </map:SfMaps.BindingContext>
            
    <map:SfMaps.Resources>
        <FlexLayout x:Key="legendLayout" 
                    HorizontalOptions="Start"
                    Margin="10"
                    Background="LightBlue"/>
    </map:SfMaps.Resources>

    <map:SfMaps.Layer>
        <map:MapShapeLayer x:Name="mapShapeLayer" 
                           ShapesSource="url"
                           DataSource="{Binding Data}"
                           PrimaryValuePath="State"
                           ShapeDataField="name" 
                           ShapeStroke="DarkGray"
                           ShapeColorValuePath="Population">

            <map:MapShapeLayer.ColorMappings>
                <map:RangeColorMapping Color="#809fff"
                                       From="0" To="100"
                                       Text="0-100"/>
                <map:RangeColorMapping Color="#3366ff" 
                                       From="100" 
                                       To="500" 
                                       Text="100-500"/>
                <map:RangeColorMapping Color="#0039e6" 
                                       From="500" 
                                       To="1000" 
                                       Text="500-1000"/>
                <map:RangeColorMapping Color="#002db3" 
                                       From="1000" 
                                       To="5000" 
                                       Text="1000-5000"/>
                <map:RangeColorMapping Color="#001a66"
                                       From="5000"
                                       To="50000"
                                       Text="5000-50000"/>
            </map:MapShapeLayer.ColorMappings>

            <map:MapShapeLayer.Legend>
                <map:MapLegend ItemsLayout="{StaticResource legendLayout}">
                    <map:MapLegend.TextStyle>
                        <map:MapLabelStyle FontSize="12" 
                                           TextColor="Crimson" />
                    </map:MapLegend.TextStyle>
                </map:MapLegend>
            </map:MapShapeLayer.Legend>
        </map:MapShapeLayer>
    </map:SfMaps.Layer>
</map:SfMaps>
```

**C# (.NET 9):**
```csharp
using Syncfusion.Maui.Maps;
using Syncfusion.Maui.Core;
using System.Collections.ObjectModel;

public MainPage()
{
    InitializeComponent();
    ViewModel viewModel = new ViewModel();
    this.BindingContext = viewModel;
    
    MapShapeLayer layer = new MapShapeLayer();
    layer.ShapesSource = MapSource.FromUri("url");
    layer.DataSource = viewModel.Data;
    layer.PrimaryValuePath = "State";
    layer.ShapeDataField = "name";
    layer.ShapeStroke = Colors.DarkGrey;
    layer.ShapeColorValuePath = "Population";

    layer.ColorMappings.Add(new RangeColorMapping()
    {
        Color = Color.FromArgb("#809fff"),
        From = 0,
        To = 100,
        Text = "0-100"
    });
    layer.ColorMappings.Add(new RangeColorMapping()
    {
        Color = Color.FromArgb("#3366ff"),
        From = 100,
        To = 500,
        Text = "100-500"
    });
    layer.ColorMappings.Add(new RangeColorMapping()
    {
        Color = Color.FromArgb("#0039e6"),
        From = 500,
        To = 1000,
        Text = "500-1000"
    });
    layer.ColorMappings.Add(new RangeColorMapping()
    {
        Color = Color.FromArgb("#002db3"),
        From = 1000,
        To = 5000,
        Text = "1000-5000"
    });
    layer.ColorMappings.Add(new RangeColorMapping()
    {
        Color = Color.FromArgb("#001a66"),
        From = 5000,
        To = 50000,
        Text = "5000-50000"
    });

    MapLegend legendSet = new MapLegend();
    legendSet.SourceType = LegendSourceType.Shape;
    legendSet.Placement = Syncfusion.Maui.Core.LegendPlacement.Top;
    legendSet.ItemsLayout = new FlexLayout
    {
        Margin = new Thickness(10),
        Background = new SolidColorBrush(Colors.LightBlue),
    };

    layer.Legend = legendSet;
    SfMaps maps = new SfMaps();
    maps.Layer = layer;
    this.Content = maps;
}

public class ViewModel
{
    public ObservableCollection<Model> Data { get; set; }
    
    public ViewModel()
    {
        Data = new ObservableCollection<Model>();
        Data.Add(new Model("India", 205));
        Data.Add(new Model("United States", 190));
        Data.Add(new Model("Kazakhstan", 37));
        Data.Add(new Model("Italy", 201));
        Data.Add(new Model("Korea", 512));
        Data.Add(new Model("Japan", 335));
        Data.Add(new Model("Cuba", 103));
        Data.Add(new Model("China", 148));
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

## Customize Items Template

You can customize the appearance of legend items with your template by using `ItemTemplate` property of `MapLegend`.

### Key APIs Used
- `MapLegend.ItemTemplate` - Property to set custom template for legend items
- `DataTemplate` - Template definition for custom UI
- `LegendItem` - Data type of template binding context
- Binding properties: `IconBrush`, `Text`

**Note:** The BindingContext of the ItemTemplate is the corresponding underlying legend item provided in the MapLegend class.

### Items Template Example

**XAML:**
```xaml
<map:SfMaps x:Name="maps">
    <map:SfMaps.BindingContext>
        <local:LegendViewModel />
    </map:SfMaps.BindingContext>
        
    <map:SfMaps.Resources>
        <DataTemplate x:Key="legendTemplate" x:DataType="core:LegendItem">
            <Grid ColumnDefinitions="Auto,Auto" Padding="5,0,5,0" Margin="2">
                <BoxView Grid.Column="0"
                         Color="{Binding IconBrush}"
                         HorizontalOptions="Center"  
                         VerticalOptions="Center"
                         CornerRadius="4"
                         HeightRequest="10"
                         WidthRequest="10" />
                <Label Grid.Column="1"
                       FontSize="13"
                       VerticalTextAlignment="Center"
                       Text="{Binding Text}"
                       HorizontalOptions="Start"
                       HorizontalTextAlignment="Center"
                       Padding="5" />
            </Grid>
        </DataTemplate>
    </map:SfMaps.Resources>

    <map:SfMaps.Layer>
        <map:MapShapeLayer x:Name="mapShapeLayer" 
                           ShapesSource="url"
                           DataSource="{Binding Data}"
                           PrimaryValuePath="State"
                           ShapeDataField="name"
                           ShapeStroke="DarkGray"
                           ShapeColorValuePath="Population">

            <map:MapShapeLayer.ColorMappings>
                <map:RangeColorMapping Color="#809fff"
                                       From="0" To="100"
                                       Text="0-100"/>
                <map:RangeColorMapping Color="#3366ff" 
                                       From="100" 
                                       To="500" 
                                       Text="100-500"/>
                <map:RangeColorMapping Color="#0039e6" 
                                       From="500" 
                                       To="1000" 
                                       Text="500-1000"/>
                <map:RangeColorMapping Color="#002db3" 
                                       From="1000" 
                                       To="5000" 
                                       Text="1000-5000"/>
                <map:RangeColorMapping Color="#001a66"
                                       From="5000"
                                       To="50000"
                                       Text="5000-50000"/>
            </map:MapShapeLayer.ColorMappings>

            <map:MapShapeLayer.Legend>
                <map:MapLegend ItemTemplate="{StaticResource legendTemplate}">
                    <map:MapLegend.TextStyle>
                        <map:MapLabelStyle FontSize="12" 
                                           TextColor="Crimson" />
                    </map:MapLegend.TextStyle>
                </map:MapLegend>
            </map:MapShapeLayer.Legend>
        </map:MapShapeLayer>
    </map:SfMaps.Layer>
</map:SfMaps>
```

**Available bindings in ItemTemplate:**
- `{Binding IconBrush}` - Color from color mapping
- `{Binding Text}` - Text from color mapping

## Best Practices

### 1. Clear, Concise Legend Text

Always use concise, descriptive text for legend items:

```csharp
// ❌ BAD - Too verbose
new RangeColorMapping 
{ 
    Text = "Population between 100 million and 500 million people" 
}

// ✅ GOOD - Concise
new RangeColorMapping 
{ 
    Text = "100M - 500M" 
}
```

### 2. Appropriate Placement

Choose placement based on map orientation and available space:

```csharp
// For wide maps
legendSet.Placement = Syncfusion.Maui.Core.LegendPlacement.Top;

// For tall maps or limited horizontal space
legendSet.Placement = Syncfusion.Maui.Core.LegendPlacement.Right;
```

### 3. Limit Legend Items

Keep the number of legend items manageable:

**Recommendations:**
- 3-5 items: Ideal for clarity
- 6-7 items: Acceptable
- 8+ items: Consider consolidating ranges or using gradient

```csharp
// ✅ GOOD - Well-chosen ranges (5 items)
layer.ColorMappings.Add(new RangeColorMapping { From = 0, To = 100, Text = "0-100" });
layer.ColorMappings.Add(new RangeColorMapping { From = 100, To = 500, Text = "100-500" });
layer.ColorMappings.Add(new RangeColorMapping { From = 500, To = 1000, Text = "500-1000" });
layer.ColorMappings.Add(new RangeColorMapping { From = 1000, To = 5000, Text = "1000-5000" });
layer.ColorMappings.Add(new RangeColorMapping { From = 5000, To = 50000, Text = "5000+" });
```

### 4. Color Scheme Consistency

Use appropriate color schemes for your data type:

```csharp
// ✅ Sequential scheme for continuous data (e.g., population density)
Color.FromArgb("#FFF5EB") → Color.FromArgb("#FD8D3C") → Color.FromArgb("#D94801")

// ✅ Categorical scheme for discrete categories (e.g., regions)
Colors.Red, Colors.Blue, Colors.Green, Colors.Orange
```

### 5. Accessibility and Readability

Ensure legend text is readable with good contrast:

```csharp
MapLabelStyle mapLabelStyle = new MapLabelStyle
{
    TextColor = Colors.Black,           // High contrast
    FontSize = 14,                      // Readable size
    FontAttributes = FontAttributes.Bold // Better legibility
};
legendSet.TextStyle = mapLabelStyle;
```

### 6. Set Legend Source Type Correctly

Always match the `SourceType` to the data being visualized:

```csharp
// For shape color mappings
legendSet.SourceType = LegendSourceType.Shape;

// For bubble color mappings
legendSet.SourceType = LegendSourceType.Bubble;
```

### 7. Provide Descriptive Text in Color Mappings

Always include the `Text` property in color mappings:

```csharp
// ❌ BAD - No text
new RangeColorMapping 
{ 
    Color = Colors.Blue,
    From = 0,
    To = 100
}

// ✅ GOOD - Descriptive text
new RangeColorMapping 
{ 
    Color = Colors.Blue,
    From = 0,
    To = 100,
    Text = "0-100/km²"
}
```

## Common Use Cases

### Use Case 1: Choropleth Map with Population Density

**Scenario:** Display population density across countries with a shape legend.

```csharp
using Syncfusion.Maui.Maps;
using Syncfusion.Maui.Core;

MapShapeLayer layer = new MapShapeLayer();
layer.ShapesSource = MapSource.FromUri("url");
layer.ShapeColorValuePath = "Density";
// Sequential color scheme for density
layer.ColorMappings.Add(new RangeColorMapping 
{ 
    Color = Color.FromArgb("#FFF5EB"), 
    From = 0, 
    To = 50,
    Text = "0-50/km²"
});
layer.ColorMappings.Add(new RangeColorMapping 
{ 
    Color = Color.FromArgb("#FDD0A2"), 
    From = 50, 
    To = 100,
    Text = "50-100/km²"
});
layer.ColorMappings.Add(new RangeColorMapping 
{ 
    Color = Color.FromArgb("#FD8D3C"), 
    From = 100, 
    To = 200,
    Text = "100-200/km²"
});
layer.ColorMappings.Add(new RangeColorMapping 
{ 
    Color = Color.FromArgb("#D94801"), 
    From = 200, 
    To = 1000,
    Text = "200+/km²"
});

MapLegend legend = new MapLegend 
{ 
    SourceType = LegendSourceType.Shape,
    Placement = Syncfusion.Maui.Core.LegendPlacement.Bottom,
    IconSize = new Size(16, 16)
};
layer.Legend = legend;
```

### Use Case 2: Categorical Data with Equal Color Mapping

**Scenario:** Display regions/continents with distinct colors.

```csharp
using Syncfusion.Maui.Maps;
using Syncfusion.Maui.Core;

MapShapeLayer layer = new MapShapeLayer();
layer.ShapesSource = MapSource.FromUri("url");// For e.g "https://cdn.syncfusion.com/maps/map-data/world-map.json"

// Categorical color scheme for continents
layer.ColorMappings.Add(new EqualColorMapping 
{ 
    Color = Color.FromArgb("#FF6B6B"), 
    Value = "North America",
    Text = "N. America"
});
layer.ColorMappings.Add(new EqualColorMapping 
{ 
    Color = Color.FromArgb("#4ECDC4"), 
    Value = "Europe",
    Text = "Europe"
});
layer.ColorMappings.Add(new EqualColorMapping 
{ 
    Color = Color.FromArgb("#45B7D1"), 
    Value = "Asia",
    Text = "Asia"
});
layer.ColorMappings.Add(new EqualColorMapping 
{ 
    Color = Color.FromArgb("#FFA07A"), 
    Value = "Africa",
    Text = "Africa"
});

MapLegend legend = new MapLegend 
{ 
    SourceType = LegendSourceType.Shape,
    Placement = Syncfusion.Maui.Core.LegendPlacement.Right,
    IconType = Syncfusion.Maui.Core.ShapeType.Circle
};
layer.Legend = legend;
```

### Use Case 3: Bubble Legend with Economic Data

**Scenario:** Display countries with bubbles sized by population and colored by economy type.

```csharp
using Syncfusion.Maui.Maps;
using Syncfusion.Maui.Core;

MapShapeLayer layer = new MapShapeLayer();
layer.ShapesSource = MapSource.FromUri("url");// For e.g "https://cdn.syncfusion.com/maps/map-data/world-map.json"
layer.ShowBubbles = true;

MapBubbleSettings bubbleSettings = new MapBubbleSettings
{
    SizeValuePath = "Population",
    ColorValuePath = "EconomyType"
};

// Color by economy type
bubbleSettings.ColorMappings.Add(new EqualColorMapping 
{ 
    Color = Colors.Green, 
    Value = "Developed",
    Text = "Developed"
});
bubbleSettings.ColorMappings.Add(new EqualColorMapping 
{ 
    Color = Colors.Yellow, 
    Value = "Developing",
    Text = "Developing"
});
bubbleSettings.ColorMappings.Add(new EqualColorMapping 
{ 
    Color = Colors.Red, 
    Value = "Underdeveloped",
    Text = "Underdeveloped"
});

layer.BubbleSettings = bubbleSettings;

MapLegend legend = new MapLegend 
{ 
    SourceType = LegendSourceType.Bubble,
    Placement = Syncfusion.Maui.Core.LegendPlacement.Top,
    IconSize = new Size(18, 18)
};
layer.Legend = legend;
```

## Troubleshooting

### Issue: Legend Not Showing

**Common Causes:**
1. `Legend` property not set on `MapShapeLayer`
2. `SourceType` not specified
3. Color mappings missing `Text` property
4. For bubble legend, `ShowBubbles` not set to true

**Solutions:**
```csharp
// ✅ Ensure all required properties are set
layer.Legend = new MapLegend 
{ 
    SourceType = LegendSourceType.Shape  // Must be set
};

// ✅ Ensure color mappings have Text
new RangeColorMapping 
{ 
    Color = Colors.Blue,
    From = 0,
    To = 100,
    Text = "0-100"  // Required
}

// ✅ For bubble legend, enable bubbles
layer.ShowBubbles = true;
```

### Issue: Legend Text Not Displaying

**Solution:**
Verify that each color mapping has a `Text` property:

```csharp
// ❌ Missing Text property
layer.ColorMappings.Add(new RangeColorMapping 
{ 
    Color = Colors.Blue,
    From = 0,
    To = 100
});

// ✅ Text property included
layer.ColorMappings.Add(new RangeColorMapping 
{ 
    Color = Colors.Blue,
    From = 0,
    To = 100,
    Text = "0-100"
});
```

### Issue: Wrong Legend Showing (Shape vs Bubble)

**Solution:**
Ensure `SourceType` matches the visualization:

```csharp
// For shape colors
legendSet.SourceType = LegendSourceType.Shape;

// For bubble colors
legendSet.SourceType = LegendSourceType.Bubble;
```

### Issue: Legend Items Overlapping or Cut Off

**Solutions:**
1. Change placement for more space
2. Reduce icon size
3. Use shorter text labels
4. Reduce number of color mappings

```csharp
// Adjust icon size
legendSet.IconSize = new Size(12, 12);

// Change placement for more space
legendSet.Placement = Syncfusion.Maui.Core.LegendPlacement.Right;

// Use shorter text
new RangeColorMapping { Text = "0-100" };  // Instead of "0 to 100 per square km"
```

## Key Takeaways

✅ **Core Syncfusion Maps APIs:**
- `MapLegend` - Main legend control with `SourceType`, `Placement`, and styling properties
- `MapShapeLayer.Legend` - Property to attach legend to map layer for color mapping display
- `LegendSourceType` - Enumeration for legend type (Shape for shapes, Bubble for bubbles)
- `LegendPlacement` - Enumeration for position (Top, Bottom, Left, Right)
- `ColorMapping.Text` - Property that defines the text displayed for each legend item

✅ **Essential Concepts:**
- Legends explain color-coded regions or bubble meanings on maps with descriptive labels
- Shape legends display shape layer color mappings; bubble legends display bubble color mappings
- Legend items are automatically generated from `ColorMappings` collection on the layer
- Text for legend items comes from the `Text` property of `EqualColorMapping` or `RangeColorMapping`
- Legends support customization: icon type, icon size, text styling, and placement

✅ **Implementation Patterns:**
- Shape legend: Set `Legend.SourceType = Shape` and add color mappings with `Text` property
- Bubble legend: Set `Legend.SourceType = Bubble` and configure bubble settings with color mappings
- Positioning: Use `Placement` property (Top/Bottom for horizontal, Left/Right for vertical)
- Styling: Customize `TextStyle` (font, color, size) and `IconType`/`IconSize` for appearance
- Layout control: Use `ItemsLayout` for custom arrangement or `ItemTemplate` for custom item UI

✅ **Best Practices:**
- Always include `Text` property in color mappings for legend item labels (required)
- Use concise, descriptive text (e.g., "100M-500M" instead of "Population between 100 and 500 million")
- Limit legend items to 3-7 for clarity; consolidate ranges if more are needed
- Choose placement based on map orientation (Top for wide maps, Right for tall maps)
- Use sequential color schemes for continuous data, categorical schemes for discrete categories
- Ensure sufficient text color contrast for readability (high-contrast colors)
- Match `SourceType` to visualization type (Shape for shapes, Bubble for bubbles)

## Related Topics

- **Color Mappings** - Learn more about RangeColorMapping and EqualColorMapping
- **Bubble Settings** - Configure bubble appearance and behavior
- **Data Labels** - Add labels to map shapes
- **Tooltips** - Display information on hover/tap