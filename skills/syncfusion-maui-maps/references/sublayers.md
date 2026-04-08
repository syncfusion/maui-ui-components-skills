# Sublayers in .NET MAUI Maps (SfMaps)

## Table of Contents
- [Overview](#overview)
- [Key APIs Used](#key-apis-used)
- [Shape Sublayer on Tile Layer](#shape-sublayer-on-tile-layer)
- [Shape Sublayer on Shape Layer](#shape-sublayer-on-shape-layer)
- [Styling Sublayers](#styling-sublayers)
- [Color Mapping](#color-mapping)
- [Data Labels](#data-labels)
- [Bubbles](#bubbles)
- [Tooltips](#tooltips)
- [Selection](#selection)
- [Multiple Sublayers](#multiple-sublayers)
- [Best Practices](#best-practices)
- [Key Takeaways](#key-takeaways)
- [Summary](#summary)

## Overview

Sublayers enables rendering additional geographical layers on top of the main map layer (either `MapTileLayer` or `MapShapeLayer`). Sublayers are useful for highlighting specific regions, overlaying additional data, or creating complex multi-layer visualizations without modifying the base layer.

**Key features**
The Key features include adding shapes from GeoJSON, data‑driven color mapping, bubbles, data labels, tooltips, and selection, with full control over fills and strokes. nal metrics via color mapping, and composing hierarchical visualizations.

**Common use cases:**
- Drawing routes or flight paths (polyline, arc)
- Highlighting specific areas (polygon, circle)
- Connecting locations (line, arc)
- Adding nested geographic regions (shape sublayer)

## Key APIs Used

### MapShapeSublayer Class

| API | Type | Description |
|-----|------|-------------|
| `MapShapeSublayer` | Class | Renders additional shape layer from a GeoJSON data source |
| `ShapesSource` | Property (`MapSource`) | GeoJSON source for sublayer shapes |
| `ShapeDataField` | Property (`string`) | Field in GeoJSON used to identify shapes |
| `DataSource` | Property (`IEnumerable`) | Business data to bind with shapes |
| `PrimaryValuePath` | Property (`string`) | Property path in `DataSource` for shape matching |
| `ShapeColorValuePath` | Property (`string`) | Property path used for color mapping values |

### Styling Properties

| API | Type | Description |
|-----|------|-------------|
| `ShapeFill` | `Brush` | Default fill for the sublayer’s shapes |
| `ShapeStroke` | `Brush` | Border color for the sublayer’s shapes |
| `ShapeStrokeThickness` | `double` | Border width for the sublayer’s shapes (default not specified in UG) |


### Data Visualization Features

| API | Type | Description |
|-----|------|-------------|
| `ShowDataLabels` | `bool` | Enables data labels on sublayer shapes |
| `DataLabelSettings` | `MapDataLabelSettings` | Configures label appearance and path |
| `ShowBubbles` | `bool` | Enables bubbles on sublayer shapes |
| `BubbleSettings` | `MapBubbleSettings` | Configures bubble color/size mapping and min/max |
| `ShowShapeTooltip` | `bool` | Enables tooltips for sublayer shapes |
| `ShapeTooltipTemplate` | `DataTemplate` | Template for tooltip content |


### Selection Properties

| API | Type | Description |
|-----|------|-------------|
| `EnableSelection` | `bool` | Enables single‑shape selection on the sublayer |
| `SelectedShapeFill` | `Brush` | Fill when a shape is selected |
| `SelectedShapeStroke` | `Brush` | Stroke when a shape is selected |
| `SelectedShapeStrokeThickness` | `double` | Stroke thickness for the selected shape |
| `ShapeSelected` | `event` | Raised when a shape is selected |


### Color Mapping

| API | Type | Description |
|-----|------|-------------|
| `ColorMappings` | `ObservableCollection<ColorMapping>` | Collection of color mapping rules |
| `EqualColorMapping` | Class | Maps exact values to colors |
| `RangeColorMapping` | Class | Maps value ranges to colors |

## Shape Sublayer on Tile Layer

The `MapTileLayer.Sublayers` collection hosts `MapSublayer` objects; `MapShapeSublayer` performs the actual GeoJSON rendering. Set `ShapesSource` with a `MapSource` pointing to a GeoJSON file and, when needed, set `ShapeDataField` for matching to business data.The `MapTileLayer.Sublayers` collection hosts `MapSublayer` objects; `MapShapeSublayer` performs the actual GeoJSON rendering. Set `ShapesSource` with a `MapSource` pointing to a GeoJSON file and, when needed, set `ShapeDataField` for matching to business data.

### XAML Implementation

```xaml
<maps:SfMaps>
  <maps:SfMaps.Layer>
    <maps:MapTileLayer UrlTemplate="url" >
    <maps:MapTileLayer.Sublayers>
    <maps:MapShapeSublayer
      ShapesSource="url"
            ShapeStroke="DarkGray"
            ShapeFill="#c6c6c6" />
      </maps:MapTileLayer.Sublayers>
    </maps:MapTileLayer>
  </maps:SfMaps.Layer>
</maps:SfMaps>
```

### C# Implementation

```csharp
using Syncfusion.Maui.Maps;
using Microsoft.Maui.Controls;
using Microsoft.Maui.Graphics;
using System;

namespace MauiMapsSample
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            SfMaps map = new SfMaps();
            MapTileLayer tileLayer = new MapTileLayer();
            tileLayer.UrlTemplate = "url"; // For e.g  "https://tile.openstreetmap.org/{z}/{x}/{y}.png"
            MapShapeSublayer sublayer = new MapShapeSublayer();
            sublayer.ShapesSource = MapSource.FromUri(new Uri("url")); // For e.g  "https://cdn.syncfusion.com/maps/map-data/africa.json.json"
            sublayer.ShapeFill = Color.FromRgb(198, 198, 198);
            sublayer.ShapeStroke = Colors.DarkGrey;
            
            tileLayer.Sublayers.Add(sublayer);
            map.Layer = tileLayer;
            this.Content = map;
        }
    }
}
```

## Shape Sublayer on Shape Layer

Add `MapShapeSublayer` to `MapShapeLayer.Sublayers` to overlay regions on an existing shape layer.

### XAML Implementation

```xaml
<map:SfMaps>
  <map:SfMaps.Layer>
        <map:MapShapeLayer ShapesSource="url">
      <map:MapShapeLayer.Sublayers>
        <map:MapShapeSublayer ShapesSource="url" />
      </map:MapShapeLayer.Sublayers>
    </map:MapShapeLayer>
  </map:SfMaps.Layer>
</map:SfMaps>
```

### C# Implementation

```csharp
SfMaps map = new SfMaps();
MapShapeLayer shapeLayer = new MapShapeLayer();
shapeLayer.ShapesSource = MapSource.FromUri(new Uri("url")); // For e.g  "https://cdn.syncfusion.com/maps/map-data/world-map.json"

MapShapeSublayer sublayer = new MapShapeSublayer();
sublayer.ShapesSource = MapSource.FromUri(new Uri("url")); // For e.g  "https://cdn.syncfusion.com/maps/map-data/africa.json"
sublayer.ShapeFill = Color.FromRgb(198, 198, 198);
sublayer.ShapeStroke = Colors.DarkGrey;

shapeLayer.Sublayers.Add(sublayer);
map.Layer = shapeLayer;
this.Content = map;
```

## Styling Sublayers

Customize appearance with `ShapeFill`, `ShapeStroke`, and `ShapeStrokeThickness`. Use valid MAUI colors (e.g., `Colors.DarkGray`)

### XAML Implementation

```xaml
<map:SfMaps>
  <map:SfMaps.Layer>
    <map:MapShapeLayer ShapesSource="url">
      <map:MapShapeLayer.Sublayers>
        <map:MapShapeSublayer
          ShapesSource="url"
            ShapeStroke="#226ac1"
            ShapeFill="#bbdefa" />
      </map:MapShapeLayer.Sublayers>
    </map:MapShapeLayer>
  </map:SfMaps.Layer>
</map:SfMaps>
```

### C# Implementation

```csharp
SfMaps maps = new SfMaps();
MapShapeLayer layer = new MapShapeLayer
{
            ShapesSource = MapSource.FromUri(new Uri("url")) // For e.g  "https://cdn.syncfusion.com/maps/map-data/world-map.json"
};
MapShapeSublayer sublayer = new MapShapeSublayer
{
    ShapesSource = MapSource.FromUri(new Uri("url")), // For e.g  "https://cdn.syncfusion.com/maps/map-data/africa.json"
    ShapeFill = Color.FromRgb(187, 222, 250),
    ShapeStroke = Color.FromRgb(34, 106, 193)
};
layer.Sublayers.Add(sublayer);
maps.Layer = layer;
Content = maps;
```

## Color Mapping

Apply data-driven color mapping to sublayer shapes using the `ColorMappings` collection with `EqualColorMapping` or `RangeColorMapping`.

### Equal Color Mapping

`EqualColorMapping` applies colors based on exact value matches between the data and the mapping value.

#### XAML Implementation

```xaml
<map:SfMaps>
  <map:SfMaps.Layer>
    <map:MapShapeLayer ShapesSource="url">
      <map:MapShapeLayer.Sublayers>
        <map:MapShapeSublayer
          ShapesSource="url"
            ShapeDataField="name"
            DataSource="{Binding Data}"
            PrimaryValuePath="State"
            ShapeColorValuePath="Storage">
          <map:MapShapeSublayer.ColorMappings>
            <map:EqualColorMapping Color="Red"   Value="Low"  />
            <map:EqualColorMapping Color="Green" Value="High" />
          </map:MapShapeSublayer.ColorMappings>
        </map:MapShapeSublayer>
      </map:MapShapeLayer.Sublayers>
    </map:MapShapeLayer>
  </map:SfMaps.Layer>
</map:SfMaps>
```

#### C# Implementation

```csharp
using Syncfusion.Maui.Maps;
using Microsoft.Maui.Controls;
using Microsoft.Maui.Graphics;
using System;
using System.Collections.ObjectModel;

namespace MauiMapsSample
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            ViewModel viewModel = new ViewModel();
            this.BindingContext = viewModel;
            
            SfMaps maps = new SfMaps();
            MapShapeLayer layer = new MapShapeLayer
			{
				ShapesSource = MapSource.FromUri(new Uri("url"))
			};
            
            MapShapeSublayer sublayer = new MapShapeSublayer
			{
				ShapesSource = MapSource.FromUri(new Uri("url")),
				ShapeFill = Color.FromRgb(198, 198, 198),
				ShapeStroke = Colors.DarkGray,
				DataSource = vm.Data,
				PrimaryValuePath = "State",
				ShapeDataField = "name",
				ShapeColorValuePath = "Storage"
			};

            sublayer.ColorMappings.Add(new EqualColorMapping { Color = Colors.Red,   Value = "Low"  });
			sublayer.ColorMappings.Add(new EqualColorMapping { Color = Colors.Green, Value = "High" });

            layer.Sublayers.Add(sublayer);
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
            Data.Add(new Model("Algeria", "Low"));
            Data.Add(new Model("Nigeria", "High"));
            Data.Add(new Model("Libya", "High"));
        }
    }

    public class Model
    {
        public string State { get; set; }
        public string Storage { get; set; }

        public Model(string state, string storage)
        {
            State = state;
            Storage = storage;
        }
    }
}
```

### Range Color Mapping

`RangeColorMapping` applies colors based on whether values fall within specified ranges.

#### XAML Implementation

```xaml
<map:SfMaps>
  <map:SfMaps.Layer>
    <map:MapShapeLayer ShapesSource="url">
      <map:MapShapeLayer.Sublayers>
        <map:MapShapeSublayer
            ShapesSource="url"
            ShapeDataField="name"
            DataSource="{Binding Data}"
            PrimaryValuePath="State"
            ShapeColorValuePath="Count">
          <map:MapShapeSublayer.ColorMappings>
            <map:RangeColorMapping Color="Red"   From="0"   To="100" />
            <map:RangeColorMapping Color="Green" From="101" To="300" />
          </map:MapShapeSublayer.ColorMappings>
        </map:MapShapeSublayer>
      </map:MapShapeLayer.Sublayers>
    </map:MapShapeLayer>
  </map:SfMaps.Layer>
</map:SfMaps>
```

#### C# Implementation

```csharp
using Syncfusion.Maui.Maps;
using Microsoft.Maui.Controls;
using Microsoft.Maui.Graphics;
using System;
using System.Collections.ObjectModel;

namespace MauiMapsSample
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            ViewModel viewModel = new ViewModel();
            this.BindingContext = viewModel;
            
            SfMaps maps = new SfMaps();
            MapShapeLayer layer = new MapShapeLayer
			{
				ShapesSource = MapSource.FromUri(new Uri("url"))
			};
            
            MapShapeSublayer sublayer = new MapShapeSublayer
			{
				ShapesSource = MapSource.FromUri(new Uri("url")),
				ShapeFill = Color.FromRgb(198, 198, 198),
				ShapeStroke = Colors.DarkGray,
				DataSource = vm.Data,
				PrimaryValuePath = "State",
				ShapeDataField = "name",
				ShapeColorValuePath = "Count"
			};

            sublayer.ColorMappings.Add(new RangeColorMapping { Color = Colors.Red,   From = 0,   To = 100 });
			sublayer.ColorMappings.Add(new RangeColorMapping { Color = Colors.Green, From = 101, To = 300 });

            layer.Sublayers.Add(sublayer);
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
            Data.Add(new Model("Algeria", 196));
            Data.Add(new Model("Nigeria", 280));
            Data.Add(new Model("Libya", 45));
        }
    }

    public class Model
    {
        public string State { get; set; }
        public double Count { get; set; }

        public Model(string state, double count)
        {
            State = state;
            Count = count;
        }
    }
}
```

**Use Case:** Visualizing population density, sales performance, or other numeric metrics with color-coded ranges

## Data Labels

Enable and customize labels using `ShowDataLabels` and `DataLabelSettings`.

### XAML Implementation

```xaml
<map:SfMaps>
  <map:SfMaps.Layer>
    <map:MapShapeLayer ShapesSource="url">
      <map:MapShapeLayer.Sublayers>
        <map:MapShapeSublayer
            ShapesSource="url"
            ShapeDataField="name"
            DataSource="{Binding Data}"
            PrimaryValuePath="State"
            ShowDataLabels="True">
          <map:MapShapeSublayer.DataLabelSettings>
            <map:MapDataLabelSettings DataLabelPath="State">
              <map:MapDataLabelSettings.DataLabelStyle>
                <map:MapLabelStyle FontSize="6" TextColor="#ff4e41" FontAttributes="Bold" />
              </map:MapDataLabelSettings.DataLabelStyle>
            </map:MapDataLabelSettings>
          </map:MapShapeSublayer.DataLabelSettings>
        </map:MapShapeSublayer>
      </map:MapShapeLayer.Sublayers>
    </map:MapShapeLayer>
  </map:SfMaps.Layer>
</map:SfMaps>
```

### C# Implementation

```csharp
ViewModel viewModel = new ViewModel();
BindingContext = viewModel;

SfMaps maps = new SfMaps();
MapShapeLayer layer = new MapShapeLayer
{
    ShapesSource = MapSource.FromUri(new Uri("url"))
};

MapShapeSublayer sublayer = new MapShapeSublayer
{
    ShapesSource = MapSource.FromUri(new Uri("url")),
    ShapeFill = Color.FromRgb(198, 198, 198),
    ShapeStroke = Colors.DarkGray,
    DataSource = vm.Data,
    PrimaryValuePath = "State",
    ShapeDataField = "name",
    ShowDataLabels = true,
    DataLabelSettings = new MapDataLabelSettings
    {
        DataLabelPath = "State",
        DataLabelStyle = new MapLabelStyle
        {
            FontSize = 6,
            FontAttributes = FontAttributes.Bold,
            TextColor = Color.FromRgb(255, 78, 65)
        }
    }
};

layer.Sublayers.Add(sublayer);
maps.Layer = layer;
Content = maps;
```

**Use Case:** Displaying country/state names or values on sublayer shapes

## Bubbles

Enable bubbles on sublayer shapes using the `ShowBubbles` property and customize them with `BubbleSettings`.

### XAML Implementation

```xaml
<map:SfMaps>
  <map:SfMaps.Layer>
    <map:MapShapeLayer ShapesSource="url">
      <map:MapShapeLayer.Sublayers>
        <map:MapShapeSublayer
            ShapesSource="url"
            ShapeDataField="name"
            DataSource="{Binding Data}"
            PrimaryValuePath="State"
            ShowBubbles="True">
          <map:MapShapeSublayer.BubbleSettings>
            <map:MapBubbleSettings ColorValuePath="Color" SizeValuePath="Size" MinSize="10" MaxSize="20" />
          </map:MapShapeSublayer.BubbleSettings>
        </map:MapShapeSublayer>
      </map:MapShapeLayer.Sublayers>
    </map:MapShapeLayer>
  </map:SfMaps.Layer>
</map:SfMaps>
```

### C# Implementation

```csharp
ViewModel viewModel = new ViewModel();
BindingContext = viewModel;

SfMaps maps = new SfMaps();
MapShapeLayer layer = new MapShapeLayer
{
    ShapesSource = MapSource.FromUri(new Uri("url"))
};

MapShapeSublayer sublayer = new MapShapeSublayer
{
    ShapesSource = MapSource.FromUri(new Uri("url")),// For e.g  "https://cdn.syncfusion.com/maps/map-data/africa.json"
    ShapeFill = Color.FromRgb(198, 198, 198),
    ShapeStroke = Colors.DarkGray,
    DataSource = vm.Data,
    PrimaryValuePath = "State",
    ShapeDataField = "name",
    ShowBubbles = true,
    BubbleSettings = new MapBubbleSettings
    {
        ColorValuePath = "Color",
        SizeValuePath = "Size",
        MinSize = 10,
        MaxSize = 20
    }
};

layer.Sublayers.Add(sublayer);
maps.Layer = layer;
Content = maps;
```

**Use Case:** Visualizing population, sales, or other metrics as sized bubbles on sublayer regions.

## Tooltips

Enable tooltips for sublayer shapes using the `ShowShapeTooltip` property and customize with `ShapeTooltipTemplate`. The binding context exposes `MapTooltipInfo.DataItem`.

### XAML Implementation

```xaml
<map:SfMaps>
  <map:SfMaps.Layer>
    <map:MapShapeLayer ShapesSource="url">
      <map:MapShapeLayer.Sublayers>
        <map:MapShapeSublayer
            ShapesSource="url"
            ShapeDataField="name"
            DataSource="{Binding Data}"
            PrimaryValuePath="State"
            ShapeColorValuePath="Color"
            ShowShapeTooltip="True">
          <map:MapShapeSublayer.ShapeTooltipTemplate>
            <DataTemplate>
              <Grid>
                <Grid.RowDefinitions>
                  <RowDefinition />
                  <RowDefinition />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                  <ColumnDefinition />
                  <ColumnDefinition />
                </Grid.ColumnDefinitions>
                <Label Text="State:" Grid.Row="0" Grid.Column="0" TextColor="White" />
                <Label Text="{Binding DataItem.State}" Grid.Row="0" Grid.Column="1" TextColor="White" />
                <Label Text="Population :" Grid.Row="1" Grid.Column="0" TextColor="White" />
                <Label Text="{Binding DataItem.Size}" Grid.Row="1" Grid.Column="1" TextColor="White" />
              </Grid>
            </DataTemplate>
          </map:MapShapeSublayer.ShapeTooltipTemplate>
        </map:MapShapeSublayer>
      </map:MapShapeLayer.Sublayers>
      <map:MapShapeLayer.ZoomPanBehavior>
        <map:MapZoomPanBehavior ZoomLevel="2" />
      </map:MapShapeLayer.ZoomPanBehavior>
    </map:MapShapeLayer>
  </map:SfMaps.Layer>
</map:SfMaps>
```

### C# Implementation

```csharp
ViewModel viewModel = new ViewModel();
BindingContext = viewModel;

SfMaps maps = new SfMaps();
MapShapeLayer layer = new MapShapeLayer
{
    ShapesSource = MapSource.FromUri(new Uri("url")),
    ZoomPanBehavior = new MapZoomPanBehavior { ZoomLevel = 2 }
};

var sublayer = new MapShapeSublayer
{
    ShapesSource = MapSource.FromUri(new Uri("url")),// For e.g  "https://cdn.syncfusion.com/maps/map-data/africa.json"
    ShapeFill = Color.FromRgb(198, 198, 198),
    ShapeStroke = Colors.DarkGray,
    DataSource = vm.Data,
    PrimaryValuePath = "State",
    ShapeDataField = "name",
    ShapeColorValuePath = "Color",
    ShowShapeTooltip = true,
    ShapeTooltipTemplate = CreateDataTemplate()
};


layer.Sublayers.Add(sublayer);
maps.Layer = layer;
Content = maps;

private DataTemplate CreateDataTemplate()
{
    new DataTemplate(() =>
    {
        var grid = new Grid
        {
            RowDefinitions = { new RowDefinition(), new RowDefinition() },
            ColumnDefinitions = { new ColumnDefinition(), new ColumnDefinition() }
        };

        var stateLabel = new Label { Text = "State", TextColor = Colors.White, Padding = 5 };
        Grid.SetRow(stateLabel, 0); Grid.SetColumn(stateLabel, 0);

        var stateValue = new Label { TextColor = Colors.White, Padding = 5 };
        stateValue.SetBinding(Label.TextProperty, new Binding(nameof(MapTooltipInfo.DataItem) + "." + nameof(Model.State)));
        Grid.SetRow(stateValue, 0); Grid.SetColumn(stateValue, 1);

        var populationLabel = new Label { Text = "Population", TextColor = Colors.White, Padding = 5 };
        Grid.SetRow(populationLabel, 1); Grid.SetColumn(populationLabel, 0);

        var populationValue = new Label { TextColor = Colors.White, Padding = 5 };
        populationValue.SetBinding(Label.TextProperty, new Binding(nameof(MapTooltipInfo.DataItem) + "." + nameof(Model.Size)));
        Grid.SetRow(populationValue, 1); Grid.SetColumn(populationValue, 1);

        grid.Children.Add(stateLabel);
        grid.Children.Add(stateValue);
        grid.Children.Add(populationLabel);
        grid.Children.Add(populationValue);

        return grid;
    });
}
```

**Use Case:** Displaying detailed information when users tap/hover over sublayer shapes.

## Selection

Enable shape selection on sublayer using the `EnableSelection` property and customize selected shape appearance.

### XAML Implementation

```xaml
<map:SfMaps>
  <map:SfMaps.Layer>
    <map:MapShapeLayer ShapesSource="url">
      <map:MapShapeLayer.Sublayers>
        <map:MapShapeSublayer
            ShapesSource="url"
            ShapeDataField="name"
            DataSource="{Binding Data}"
            PrimaryValuePath="State"
            ShapeColorValuePath="Color"
            EnableSelection="True"
            SelectedShapeFill="#cddc44"
            SelectedShapeStroke="Black"
            SelectedShapeStrokeThickness="2" />
      </map:MapShapeLayer.Sublayers>
      <map:MapShapeLayer.ZoomPanBehavior>
        <map:MapZoomPanBehavior ZoomLevel="2" />
      </map:MapShapeLayer.ZoomPanBehavior>
    </map:MapShapeLayer>
  </map:SfMaps.Layer>
</map:SfMaps>
```

### C# Implementation

```csharp
ViewModel viewModel = new ViewModel();
BindingContext = viewModel;

SfMaps maps = new SfMaps();
MapShapeLayer layer = new MapShapeLayer
{
    ShapesSource = MapSource.FromUri(new Uri("url")),// For e.g  "https://cdn.syncfusion.com/maps/map-data/world-map.json"
    ZoomPanBehavior = new MapZoomPanBehavior { ZoomLevel = 2 }
};

MapShapeSublayer sublayer = new MapShapeSublayer
{
    ShapesSource = MapSource.FromUri(new Uri("url")),// For e.g  "https://cdn.syncfusion.com/maps/map-data/africa.json"
    ShapeFill = Color.FromRgb(198, 198, 198),
    ShapeStroke = Colors.DarkGray,
    DataSource = vm.Data,
    PrimaryValuePath = "State",
    ShapeDataField = "name",
    ShapeColorValuePath = "Color",
    EnableSelection = true,
    SelectedShapeFill = Color.FromRgb(205, 220, 68),
    SelectedShapeStroke = Colors.Black,
    SelectedShapeStrokeThickness = 2
};

layer.Sublayers.Add(sublayer);
maps.Layer = layer;
Content = maps;
```

**Use Case:** Interactive maps where users can select regions for detailed views or actions

## Multiple Sublayers

Add multiple sublayers to create complex, layered visualizations. Below, we overlay a shape sublayer and then add vector sublayers (line, arc, polyline, polygon, circle) — all inside `MapShapeLayer.Sublayers`.

### Example with Multiple Sublayers

```csharp
SfMaps maps  = new SfMaps();
MapShapeLayer layer = new MapShapeLayer
{
    ShapesSource = MapSource.FromUri(new Uri("url")),// For e.g  "https://cdn.syncfusion.com/maps/map-data/world-map.json"
    ShapeStroke  = Brush.DarkGray
};

// Shape sublayer (Africa)
layer.Sublayers.Add(new MapShapeSublayer
{
    ShapesSource = MapSource.FromUri(new Uri("url")),// For e.g  "https://cdn.syncfusion.com/maps/map-data/africa.json"
    ShapeFill = Color.FromRgb(187, 222, 250),
    ShapeStroke = Color.FromRgb(34, 106, 193)
});

// Line layer
var lineLayer = new MapLineLayer();
lineLayer.Lines.Add(new MapLine
{
    From = new MapLatLng(28.7041, 77.1025),
    To   = new MapLatLng(56.1304, -106.3468),
    Stroke = Color.FromRgb(237, 74, 71)
});

layer.Sublayers.Add(lineLayer);

// Arc layer
var arcLayer = new MapArcLayer();
arcLayer.Arcs.Add(new MapArc
{
    From = new MapLatLng(28.6139, 77.2090),
    To   = new MapLatLng(39.9042, 116.4074),
    HeightFactor = 0.2,
    Stroke = Color.FromRgb(109, 160, 242)
});

layer.Sublayers.Add(arcLayer);

// Polyline layer
var polylineLayer = new MapPolylineLayer();
polylineLayer.Polylines.Add(new MapPolyline
{
    Points = new ObservableCollection<MapLatLng>
    {
        new(13.0827, 80.2707), new(13.1746, 79.6117), new(18.5204, 73.8567), new(19.0760, 72.8777)
    },
    Stroke = Color.FromRgb(153, 63, 173),
    StrokeThickness = 3
});

layer.Sublayers.Add(polylineLayer);

// Polygon layer
var polygonLayer = new MapPolygonLayer();
polygonLayer.Polygons.Add(new MapPolygon
{
    Points = new ObservableCollection<MapLatLng>
    {
        new(55.7558, 37.6173), new(53.7596, 87.1216), new(61.5240, 105.3188)
    },
    Fill = Color.FromRgb(109, 239, 174)
});

layer.Sublayers.Add(polygonLayer);

// Circle layer
var circleLayer = new MapCircleLayer();
circleLayer.Circles.Add(new MapCircle
{
    Center = new MapLatLng(19.7515, 75.7139),
    Radius = 10,
    Fill   = Color.FromRgb(48, 153, 137)
});

layer.Sublayers.Add(circleLayer);

maps.Layer = layer;
Content    = maps;
```

## Best Practices

### 1. Use MapSource.FromUri for GeoJSON

```csharp
// Correct way to load GeoJSON
sublayer.ShapesSource = MapSource.FromUri(new Uri("url"));// For e.g  "https://cdn.syncfusion.com/maps/map-data/africa.json"
```

### 2. Match DataSource with ShapeDataField

```csharp
// Ensure PrimaryValuePath in DataSource matches ShapeDataField in GeoJSON
sublayer.DataSource = viewModel.Data;
sublayer.PrimaryValuePath = "State";  // Property in Model
sublayer.ShapeDataField = "name";     // Field in GeoJSON
```

### 3. Use ObservableCollection for Dynamic Data

```csharp
// Enables automatic UI updates when data changes
public ObservableCollection<Model> Data { get; set; }
```

### 4. Apply Transparency for Overlays

```csharp
// Allow base layer to show through
sublayer.ShapeFill = Color.FromRgb(198, 198, 198);  // Or use Color.FromArgb with alpha
```

### 5. Combine Features for Rich Visualizations

```csharp
// Sublayer with multiple features
sublayer.ShowDataLabels = true;
sublayer.ShowBubbles = true;
sublayer.EnableSelection = true;
sublayer.ShowShapeTooltip = true;
```

### 6. Use Color Mappings for Data-Driven Colors

```csharp
// Instead of static colors, use EqualColorMapping or RangeColorMapping
sublayer.ColorMappings.Add(new EqualColorMapping { Color = Colors.Red, Value = "High" });
```

### 7. Handle Zoom for Dense Sublayers

```csharp
// Add zoom behavior when sublayers have many shapes or data labels
layer.ZoomPanBehavior = new MapZoomPanBehavior { ZoomLevel = 2 };
```

## Key Takeaways

✅ **Core Syncfusion Maps APIs:**
- `MapShapeSublayer` - Class for rendering additional GeoJSON shape layers on top of base layer
- `MapShapeLayer.Sublayers` / `MapTileLayer.Sublayers` - Collections for adding multiple sublayers
- `ShapesSource`, `DataSource`, `PrimaryValuePath`, `ShapeDataField` - Properties for data binding (same as main layer)
- `ShapeFill`, `ShapeStroke`, `ShapeStrokeThickness` - Styling properties for sublayer shapes
- `ShowDataLabels`, `ShowBubbles`, `EnableSelection`, `ShowShapeTooltip` - Feature properties for sublayers

✅ **Essential Concepts:**
- Sublayers overlay additional geographic shapes from GeoJSON on main layer (tile or shape)
- Each sublayer is independent with its own data source, styling, and visualization features
- Sublayers support all main layer features: color mapping, bubbles, markers, data labels, tooltips, selection
- Multiple sublayers can be added and are rendered in the order they're added to the collection
- Sublayers enable hierarchical visualizations without modifying the base layer

✅ **Implementation Patterns:**
- Basic sublayer: Create `MapShapeSublayer`, set `ShapesSource`, add to `layer.Sublayers` collection
- Data-driven: Bind `DataSource`, set `PrimaryValuePath`/`ShapeDataField`, apply `ColorMappings`
- Highlighting: Use distinct colors/transparency to make sublayer regions stand out from base layer
- Multi-layer: Add multiple sublayers with different GeoJSON sources for complex visualizations
- Interactive: Enable `EnableSelection`, `ShowShapeTooltip`, `ShowDataLabels` for user engagement

✅ **Vector sublayers:**
- Use circles for radius-based regions  
- Use arcs for curved connections (flights, routes)  
- Use lines for straight connections  
- Use polylines for multi-segment paths  
- Use polygons for custom filled areas  
- Combine multiple sublayers for complex visualizations

✅ **Best Practices:**
- Use exact case-sensitive matching for `PrimaryValuePath` (data) and `ShapeDataField` (GeoJSON)
- Apply transparency or distinct colors to allow base layer to show through sublayers
- Load GeoJSON using `MapSource.FromUri()` for consistency with main layer pattern
- Use `ObservableCollection<Model>` for DataSource to enable automatic UI updates
- Combine sublayer features (data labels + bubbles + tooltips) for rich visualizations
- Add zoom behavior when sublayers have many shapes or dense data labels for better UX
- Keep sublayer count reasonable (2-4 typical) to maintain performance and clarity 

## Summary

`MapShapeSublayer` provides powerful capabilities for creating multi-layered geographical visualizations in .NET MAUI Maps:

**Core Features:**
- Add GeoJSON shape layers on top of tile or shape layers
- Apply data-driven styling with color mappings
- Enable bubbles, markers, data labels, and tooltips
- Support shape selection and interaction
- Create complex visualizations with multiple sublayers

**Common Use Cases:**
- Highlighting regions on world/country maps
- Visualizing regional data with color-coded shapes
- Creating hierarchical map visualizations
- Overlaying administrative boundaries on base maps
- Building interactive geographical dashboards