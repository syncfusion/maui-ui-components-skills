# Shape Layer - Syncfusion .NET MAUI Maps (SfMaps)

## Table of Contents
- [Overview](#overview)
- [Shape Styling](#shape-styling)
- [Hover State Styling](#hover-state-styling)
- [Data Binding](#data-binding)
- [Applying Colors Based on Data](#applying-colors-based-on-data)
- [Equal Color Mapping](#equal-color-mapping)
- [Range Color Mapping](#range-color-mapping)
- [API Reference](#api-reference)
- [Troubleshooting](#troubleshooting)

## Overview

The `MapShapeLayer` is the core component of Syncfusion .NET MAUI Maps (`SfMaps`) that renders vector shapes from GeoJSON or shapefile data sources. It's ideal for choropleth maps, statistical visualizations, and data-driven geographic representations.

**Key Features:**
- Shape rendering from GeoJSON/shapefile sources
- Data binding to geographic shapes
- Color mapping strategies (Equal and Range)
- Shape styling with fill, stroke, and hover states
- Integration with tooltips, data labels, and legends

**Common Use Cases:**
- Country/state/region boundary maps
- Choropleth maps (color-coded by data values)
- Statistical data visualization by geography
- Election results and demographic data
- Sales/revenue visualization by region

**Related Components:**
- `SfMaps` - Container control for map layers
- `MapShapeLayer` - Main layer for shape rendering
- `EqualColorMapping` - Categorical color mapping
- `RangeColorMapping` - Numeric range color mapping
- `MapSource` - Shape data source provider

## Shape Styling

You can customize the appearance of shapes using fill color, stroke color, and stroke thickness properties of the `MapShapeLayer`.

### Basic Shape Styling

**XAML:**
```xaml
<map:SfMaps>
    <map:SfMaps.Layer>
        <map:MapShapeLayer ShapesSource="url"
                           ShapeFill="#b5dcff"
                           ShapeStroke="#1585ed"
                           ShapeStrokeThickness="2">
        </map:MapShapeLayer>
    </map:SfMaps.Layer>
</map:SfMaps>
```

**C#:**
```csharp
using Syncfusion.Maui.Maps;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        MapShapeLayer layer = new MapShapeLayer();
        layer.ShapesSource = MapSource.FromUri("url");
        layer.ShapeFill = Color.FromRgb(181, 220, 255);
        layer.ShapeStroke = Color.FromRgb(21, 133, 237);
        layer.ShapeStrokeThickness = 2;
        
        SfMaps maps = new SfMaps();
        maps.Layer = layer;
        
        this.Content = maps;
    }
}
```

### Key APIs Used

| API | Type | Description |
|-----|------|-------------|
| `SfMaps` | Class | Main container control for map layers |
| `MapShapeLayer` | Class | Layer that renders geographic shapes |
| `ShapesSource` | Property | Source of GeoJSON/shapefile data |
| `ShapeFill` | Property | Fill color for shapes |
| `ShapeStroke` | Property | Border color for shapes |
| `ShapeStrokeThickness` | Property | Border thickness for shapes |
| `MapSource.FromUri()` | Method | Creates MapSource from a URI |

## Hover State Styling

Apply interactive hover effects to shapes using hover-specific styling properties. These properties define how shapes appear when users interact with them (mouse hover on desktop, tap on mobile).

### Configuring Hover States

**XAML:**
```xaml
<map:SfMaps>
    <map:SfMaps.Layer>
        <map:MapShapeLayer ShapesSource="url"
                           ShapeHoverFill="LightBlue"
                           ShapeHoverStroke="Blue"
                           ShapeHoverStrokeThickness="2">
        </map:MapShapeLayer>
    </map:SfMaps.Layer>
</map:SfMaps>
```

**C#:**
```csharp
using Syncfusion.Maui.Maps;
using Microsoft.Maui.Graphics;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        MapShapeLayer layer = new MapShapeLayer();
        layer.ShapesSource = MapSource.FromUri("url");// For e.g  "https://cdn.syncfusion.com/maps/map-data/world-map.json"
        layer.ShapeHoverFill = Brush.LightBlue;
        layer.ShapeHoverStroke = Brush.Blue;
        layer.ShapeHoverStrokeThickness = 2;
        
        SfMaps maps = new SfMaps();
        maps.Layer = layer;
        
        this.Content = maps;
    }
}
```

### Key APIs Used

| API | Type | Description |
|-----|------|-------------|
| `ShapeHoverFill` | Property | Fill color when shape is hovered/tapped |
| `ShapeHoverStroke` | Property | Border color when shape is hovered/tapped |
| `ShapeHoverStrokeThickness` | Property | Border thickness when shape is hovered/tapped |

## Data Binding

Bind your application data to map shapes using the `DataSource`, `PrimaryValuePath`, and `ShapeDataField` properties to create data-driven visualizations.

### Complete Example: Binding Data to Shapes

**Model Class:**
```csharp
using System;

namespace MauiMapsApp
{
    public class Model
    {
        public string State { get; set; }
        public Color Color { get; set; }
        
        public Model(string state, Color color)
        {
            State = state;
            Color = color;
        }
    }
}
```

**ViewModel:**
```csharp
using System.Collections.ObjectModel;
using Microsoft.Maui.Graphics;

namespace MauiMapsApp
{
    public class ViewModel
    {
        public ObservableCollection<Model> Data { get; set; }
        
        public ViewModel()
        {
            Data = new ObservableCollection<Model>();
            Data.Add(new Model("New South Wales", Color.FromRgb(208, 183, 0)));
            Data.Add(new Model("Northern Territory", Color.FromRgb(255, 78, 66)));
            Data.Add(new Model("Victoria", Color.FromRgb(207, 78, 238)));
            Data.Add(new Model("Tasmania", Color.FromRgb(79, 147, 216)));
            Data.Add(new Model("Queensland", Color.FromRgb(0, 213, 207)));
            Data.Add(new Model("Western Australia", Color.FromRgb(139, 106, 223)));
            Data.Add(new Model("South Australia", Color.FromRgb(123, 255, 103)));
        }
    }
}
```

**XAML:**
```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:map="clr-namespace:Syncfusion.Maui.Maps;assembly=Syncfusion.Maui.Maps"
             xmlns:local="clr-namespace:MauiMapsApp"
             x:Class="MauiMapsApp.MainPage">
    
    <ContentPage.BindingContext>
        <local:ViewModel />
    </ContentPage.BindingContext>
    
    <map:SfMaps>
        <map:SfMaps.Layer>
            <map:MapShapeLayer ShapesSource="url"
                               DataSource="{Binding Data}"
                               PrimaryValuePath="State"
                               ShapeDataField="STATE_NAME"
                               ShapeColorValuePath="Color"
                               ShapeStrokeThickness="0">
            </map:MapShapeLayer>
        </map:SfMaps.Layer>
    </map:SfMaps>
    
</ContentPage>
```

**C#:**
```csharp
using Syncfusion.Maui.Maps;
using System.Collections.ObjectModel;
using Microsoft.Maui.Graphics;

namespace MauiMapsApp
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            ViewModel viewModel = new ViewModel();
            this.BindingContext = viewModel;
            
            MapShapeLayer layer = new MapShapeLayer();
            layer.ShapesSource = MapSource.FromUri("url"); // For e.g  "https://cdn.syncfusion.com/maps/map-data/australia.json"
            layer.DataSource = viewModel.Data;
            layer.PrimaryValuePath = "State";
            layer.ShapeDataField = "STATE_NAME";
            layer.ShapeColorValuePath = "Color";
            layer.ShapeStrokeThickness = 0;
            
            SfMaps maps = new SfMaps();
            maps.Layer = layer;
            this.Content = maps;
        }
    }
}
```

### Key APIs Used

| API | Type | Description |
|-----|------|-------------|
| `DataSource` | Property | Collection of data objects to bind to shapes |
| `PrimaryValuePath` | Property | Property name in data model that matches shape data |
| `ShapeDataField` | Property | Property name in GeoJSON that matches data model |
| `ShapeColorValuePath` | Property | Property name in data model containing color/value |

## Applying Colors Based on Data

The `ShapeColorValuePath` property allows you to provide colors directly from your data model or use with `ColorMapping` for data-driven visualizations.

### Direct Color Assignment

**XAML:**
```xaml
<map:SfMaps>
    <map:SfMaps.Layer>
        <map:MapShapeLayer ShapesSource="url"
                           DataSource="{Binding Data}"
                           PrimaryValuePath="State"
                           ShapeDataField="STATE_NAME"
                           ShapeColorValuePath="Color"
                           ShapeStrokeThickness="0">
        </map:MapShapeLayer>
    </map:SfMaps.Layer>
</map:SfMaps>
```

**C#:**
```csharp
using Syncfusion.Maui.Maps;
using System.Collections.ObjectModel;

namespace MauiMapsApp
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            ViewModel viewModel = new ViewModel();
            this.BindingContext = viewModel;
            
            MapShapeLayer layer = new MapShapeLayer();
            layer.ShapesSource = MapSource.FromUri("url"); // For e.g  "https://cdn.syncfusion.com/maps/map-data/australia.json"
            layer.DataSource = viewModel.Data;
            layer.PrimaryValuePath = "State";
            layer.ShapeDataField = "STATE_NAME";
            layer.ShapeColorValuePath = "Color";
            layer.ShapeStrokeThickness = 0;
            
            SfMaps maps = new SfMaps();
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
            Data.Add(new Model("New South Wales", Color.FromRgb(208, 183, 0)));
            Data.Add(new Model("Northern Territory", Color.FromRgb(255, 78, 66)));
            Data.Add(new Model("Victoria", Color.FromRgb(207, 78, 238)));
            Data.Add(new Model("Tasmania", Color.FromRgb(79, 147, 216)));
            Data.Add(new Model("Queensland", Color.FromRgb(0, 213, 207)));
            Data.Add(new Model("Western Australia", Color.FromRgb(139, 106, 223)));
            Data.Add(new Model("South Australia", Color.FromRgb(123, 255, 103)));
        }
    }
    
    public class Model
    {
        public string State { get; set; }
        public Color Color { get; set; }
        
        public Model(string state, Color color)
        {
            State = state;
            Color = color;
        }
    }
}
```

### Key APIs Used

| API | Type | Description |
|-----|------|-------------|
| `ShapeColorValuePath` | Property | Path to property containing color or value for color mapping |
| `Color.FromRgb()` | Method | Creates Color from RGB values |

## Equal Color Mapping

Apply colors to shapes by comparing values using `EqualColorMapping`. When the value from `ShapeColorValuePath` matches the `EqualColorMapping.Value`, the specified color is applied to that shape.

### Complete Example: Equal Color Mapping

**Model Class:**
```csharp
using System;

namespace MauiMapsApp
{
    public class Model
    {
        public string Country { get; set; }
        public string Count { get; set; }
        
        public Model(string country, string count)
        {
            Country = country;
            Count = count;
        }
    }
}
```

**ViewModel:**
```csharp
using System.Collections.ObjectModel;

namespace MauiMapsApp
{
    public class ViewModel
    {
        public ObservableCollection<Model> Data { get; set; }
        
        public ViewModel()
        {
            Data = new ObservableCollection<Model>();
            Data.Add(new Model("India", "Low"));
            Data.Add(new Model("United States", "High"));
            Data.Add(new Model("Pakistan", "Low"));
        }
    }
}
```

**XAML:**
```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:map="clr-namespace:Syncfusion.Maui.Maps;assembly=Syncfusion.Maui.Maps"
             xmlns:local="clr-namespace:MauiMapsApp"
             x:Class="MauiMapsApp.MainPage">
    
    <ContentPage.BindingContext>
        <local:ViewModel />
    </ContentPage.BindingContext>
    
    <map:SfMaps>
        <map:SfMaps.Layer>
            <map:MapShapeLayer ShapesSource="url"
                               DataSource="{Binding Data}"
                               PrimaryValuePath="Country"
                               ShapeDataField="name"
                               ShapeColorValuePath="Count">
                
                <map:MapShapeLayer.ColorMappings>
                    <map:EqualColorMapping Color="Red" Value="Low" />
                    <map:EqualColorMapping Color="Green" Value="High" />
                </map:MapShapeLayer.ColorMappings>
                
            </map:MapShapeLayer>
        </map:SfMaps.Layer>
    </map:SfMaps>
    
</ContentPage>
```

**C#:**
```csharp
using Syncfusion.Maui.Maps;
using System.Collections.ObjectModel;
using Microsoft.Maui.Graphics;

namespace MauiMapsApp
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            ViewModel viewModel = new ViewModel();
            this.BindingContext = viewModel;
            
            MapShapeLayer layer = new MapShapeLayer();
            layer.ShapesSource = MapSource.FromUri("url");
            layer.DataSource = viewModel.Data;
            layer.PrimaryValuePath = "Country";
            layer.ShapeDataField = "name";
            layer.ShapeColorValuePath = "Count";
            
            EqualColorMapping colorMapping = new EqualColorMapping();
            colorMapping.Color = Colors.Red;
            colorMapping.Value = "Low";
            
            EqualColorMapping colorMapping1 = new EqualColorMapping();
            colorMapping1.Color = Colors.Green;
            colorMapping1.Value = "High";
            
            layer.ColorMappings.Add(colorMapping);
            layer.ColorMappings.Add(colorMapping1);
            
            SfMaps maps = new SfMaps();
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
            Data.Add(new Model("India", "Low"));
            Data.Add(new Model("United States", "High"));
            Data.Add(new Model("Pakistan", "Low"));
        }
    }
    
    public class Model
    {
        public string Country { get; set; }
        public string Count { get; set; }
        
        public Model(string country, string count)
        {
            Country = country;
            Count = count;
        }
    }
}
```

### Key APIs Used

| API | Type | Description |
|-----|------|-------------|
| `EqualColorMapping` | Class | Maps specific values to colors |
| `EqualColorMapping.Value` | Property | Value to match from data model |
| `EqualColorMapping.Color` | Property | Color to apply when value matches |
| `ColorMappings` | Property | Collection of color mappings for the layer |

## Range Color Mapping

Apply colors to shapes based on numeric ranges using `RangeColorMapping`. When the value from `ShapeColorValuePath` falls within the range defined by `From` and `To`, the specified color is applied.

### Complete Example: Range Color Mapping

**Model Class:**
```csharp
using System;

namespace MauiMapsApp
{
    public class Model
    {
        public string Country { get; set; }
        public int Count { get; set; }
        
        public Model(string country, int count)
        {
            Country = country;
            Count = count;
        }
    }
}
```

**ViewModel:**
```csharp
using System.Collections.ObjectModel;

namespace MauiMapsApp
{
    public class ViewModel
    {
        public ObservableCollection<Model> Data { get; set; }
        
        public ViewModel()
        {
            Data = new ObservableCollection<Model>();
            Data.Add(new Model("India", 80));
            Data.Add(new Model("United States", 30));
            Data.Add(new Model("Kazakhstan", 105));
        }
    }
}
```

**XAML:**
```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:map="clr-namespace:Syncfusion.Maui.Maps;assembly=Syncfusion.Maui.Maps"
             xmlns:local="clr-namespace:MauiMapsApp"
             x:Class="MauiMapsApp.MainPage">
    
    <ContentPage.BindingContext>
        <local:ViewModel />
    </ContentPage.BindingContext>
    
    <map:SfMaps>
        <map:SfMaps.Layer>
            <map:MapShapeLayer ShapesSource="url"
                               DataSource="{Binding Data}"
                               PrimaryValuePath="Country"
                               ShapeDataField="name"
                               ShapeColorValuePath="Count">
                
                <map:MapShapeLayer.ColorMappings>
                    <map:RangeColorMapping Color="Green" From="0" To="90" />
                    <map:RangeColorMapping Color="Red" From="100" To="150" />
                </map:MapShapeLayer.ColorMappings>
                
            </map:MapShapeLayer>
        </map:SfMaps.Layer>
    </map:SfMaps>
    
</ContentPage>
```

**C#:**
```csharp
using Syncfusion.Maui.Maps;
using System.Collections.ObjectModel;
using Microsoft.Maui.Graphics;

namespace MauiMapsApp
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
            
            ViewModel viewModel = new ViewModel();
            this.BindingContext = viewModel;
            
            MapShapeLayer layer = new MapShapeLayer();
            layer.ShapesSource = MapSource.FromUri("url"); // For e.g  "https://cdn.syncfusion.com/maps/map-data/world-map.json"
            layer.DataSource = viewModel.Data;
            layer.PrimaryValuePath = "Country";
            layer.ShapeDataField = "name";
            layer.ShapeColorValuePath = "Count";
            
            RangeColorMapping colorMapping = new RangeColorMapping();
            colorMapping.Color = Colors.Green;
            colorMapping.From = 0;
            colorMapping.To = 90;
            
            RangeColorMapping colorMapping1 = new RangeColorMapping();
            colorMapping1.Color = Colors.Red;
            colorMapping1.From = 100;
            colorMapping1.To = 150;
            
            layer.ColorMappings.Add(colorMapping);
            layer.ColorMappings.Add(colorMapping1);
            
            SfMaps maps = new SfMaps();
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
            Data.Add(new Model("India", 80));
            Data.Add(new Model("United States", 30));
            Data.Add(new Model("Kazakhstan", 105));
        }
    }
    
    public class Model
    {
        public string Country { get; set; }
        public int Count { get; set; }
        
        public Model(string country, int count)
        {
            Country = country;
            Count = count;
        }
    }
}
```

### Key APIs Used

| API | Type | Description |
|-----|------|-------------|
| `RangeColorMapping` | Class | Maps numeric ranges to colors |
| `RangeColorMapping.From` | Property | Start of the range (inclusive) |
| `RangeColorMapping.To` | Property | End of the range (inclusive) |
| `RangeColorMapping.Color` | Property | Color to apply for values in range |
| `ColorMappings` | Property | Collection of color mappings for the layer |

## API Reference

### Core Classes

#### SfMaps
Main container control for displaying maps with layers.

**Namespace:** `Syncfusion.Maui.Maps`  
**Assembly:** `Syncfusion.Maui.Maps.dll`

**Key Properties:**
- `Layer` - Gets or sets the MapShapeLayer to display

**Example:**
```csharp
SfMaps maps = new SfMaps();
maps.Layer = new MapShapeLayer();
```

#### MapShapeLayer
Layer that renders geographic shapes from GeoJSON or shapefile data.

**Namespace:** `Syncfusion.Maui.Maps`  
**Assembly:** `Syncfusion.Maui.Maps.dll`

**Key Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `ShapesSource` | MapSource | Source of GeoJSON or shapefile data |
| `DataSource` | IEnumerable | Collection of data objects to bind to shapes |
| `PrimaryValuePath` | string | Property name in data model for matching |
| `ShapeDataField` | string | Property name in GeoJSON for matching |
| `ShapeColorValuePath` | string | Property name for color/value mapping |
| `ShapeFill` | Brush | Fill color for shapes |
| `ShapeStroke` | Brush | Border color for shapes |
| `ShapeStrokeThickness` | double | Border thickness for shapes |
| `ShapeHoverFill` | Brush | Fill color when shape is hovered |
| `ShapeHoverStroke` | Brush | Border color when shape is hovered |
| `ShapeHoverStrokeThickness` | double | Border thickness when shape is hovered |
| `ColorMappings` | ObservableCollection | Collection of color mapping objects |

#### MapSource
Provides static methods to create map data sources.

**Namespace:** `Syncfusion.Maui.Maps`  
**Assembly:** `Syncfusion.Maui.Maps.dll`

**Static Methods:**

| Method | Return Type | Description |
|--------|-------------|-------------|
| `FromUri(Uri uri)` | MapSource | Creates MapSource from a URI |
| `FromFile(string path)` | MapSource | Creates MapSource from a file path |
| `FromUri(string resourceId)` | MapSource | Creates MapSource from embedded resource |
| `FromStream(Stream stream)` | MapSource | Creates MapSource from a stream |

**Example:**
```csharp
var source = MapSource.FromUri("url");
```

### Color Mapping Classes

#### EqualColorMapping
Maps specific categorical values to colors.

**Namespace:** `Syncfusion.Maui.Maps`  
**Assembly:** `Syncfusion.Maui.Maps.dll`

**Key Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `Value` | object | The value to match from ShapeColorValuePath |
| `Color` | Color | Color to apply when value matches |
| `Text` | string | Text to display in legend |

**Example:**
```csharp
var mapping = new EqualColorMapping
{
    Value = "High",
    Color = Colors.Red,
    Text = "High Risk"
};
```

#### RangeColorMapping
Maps numeric ranges to colors.

**Namespace:** `Syncfusion.Maui.Maps`  
**Assembly:** `Syncfusion.Maui.Maps.dll`

**Key Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `From` | double | Start of the range (inclusive) |
| `To` | double | End of the range (inclusive) |
| `Color` | Color | Color to apply for values in range |
| `Text` | string | Text to display in legend |

**Example:**
```csharp
var mapping = new RangeColorMapping
{
    From = 0,
    To = 100,
    Color = Colors.Green,
    Text = "0-100"
};
```

#### ColorMapping (Base Class)
Abstract base class for color mappings.

**Namespace:** `Syncfusion.Maui.Maps`  
**Assembly:** `Syncfusion.Maui.Maps.dll`

**Key Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `Color` | Color | Color to apply |
| `Text` | string | Legend text |

### Summary Table: MapShapeLayer Properties

| Category | Property | Type | Description |
|----------|----------|------|-------------|
| **Data Source** | `ShapesSource` | MapSource | GeoJSON/shapefile source |
| | `DataSource` | IEnumerable | Application data collection |
| | `PrimaryValuePath` | string | Property in data model |
| | `ShapeDataField` | string | Property in GeoJSON |
| | `ShapeColorValuePath` | string | Property for color mapping |
| **Shape Styling** | `ShapeFill` | Brush | Default fill color |
| | `ShapeStroke` | Brush | Default border color |
| | `ShapeStrokeThickness` | double | Default border thickness |
| **Hover Styling** | `ShapeHoverFill` | Brush | Hover fill color |
| | `ShapeHoverStroke` | Brush | Hover border color |
| | `ShapeHoverStrokeThickness` | double | Hover border thickness |
| **Color Mapping** | `ColorMappings` | ObservableCollection | Color mapping collection |

### Summary Table: MapSource Methods

| Method | Description | Use Case |
|--------|-------------|----------|
| `FromUri()` | Load from web URL | Remote GeoJSON files |
| `FromFile()` | Load from file path | Local file system |
| `FromResource()` | Load from embedded resource | Bundled with app |
| `FromStream()` | Load from stream | Dynamic/processed data |

## Troubleshooting

### Issue: Shapes Not Displaying

**Symptoms:** Blank map or no shapes visible

**Solutions:**
1. ✅ Verify `ShapesSource` URL is correct and accessible
2. ✅ Test the GeoJSON URL directly in a web browser
3. ✅ Check network connectivity for remote URLs
4. ✅ Ensure the GeoJSON structure is valid
5. ✅ Check debug console for error messages

**Example of correct ShapesSource:**
```csharp
layer.ShapesSource = MapSource.FromUri("url"); // For e.g  "https://cdn.syncfusion.com/maps/map-data/world-map.json"
```

### Issue: Data Not Binding to Shapes

**Symptoms:** Shapes display but no colors or data visualization

**Solutions:**
1. ✅ Verify `PrimaryValuePath` exactly matches your model property name (case-sensitive)
2. ✅ Verify `ShapeDataField` exactly matches the GeoJSON property name (case-sensitive)
3. ✅ Open GeoJSON file to find the exact property names in the `properties` object
4. ✅ Ensure matching values exist in both your DataSource and the GeoJSON
5. ✅ Check for leading/trailing whitespace in data values
6. ✅ Use debugger to verify DataSource is populated with data

**Common mistakes:**
```csharp
// ❌ Wrong - Case mismatch
layer.PrimaryValuePath = "country";  // Model has "Country"
layer.ShapeDataField = "NAME";       // GeoJSON has "name"

// ✅ Correct - Exact match
layer.PrimaryValuePath = "Country";  // Matches model property
layer.ShapeDataField = "name";       // Matches GeoJSON property
```

### Issue: Colors Not Applying

**Symptoms:** Shapes visible but all same color or wrong colors

**Solutions:**
1. ✅ Verify `ShapeColorValuePath` matches your model property name
2. ✅ For `EqualColorMapping`, ensure exact value match (case-sensitive)
3. ✅ For `RangeColorMapping`, verify values fall within defined ranges
4. ✅ Check that ColorMappings cover all possible data values
5. ✅ Ensure Color property is not null
6. ✅ Verify data types match (string for Equal, numeric for Range)

**Example of checking data coverage:**
```csharp
// ❌ Values 91-99 not covered
layer.ColorMappings.Add(new RangeColorMapping { From = 0, To = 90, Color = Colors.Green });
layer.ColorMappings.Add(new RangeColorMapping { From = 100, To = 150, Color = Colors.Red });

// ✅ All values covered
layer.ColorMappings.Add(new RangeColorMapping { From = 0, To = 90, Color = Colors.Green });
layer.ColorMappings.Add(new RangeColorMapping { From = 91, To = 150, Color = Colors.Red });
```

### Issue: Hover Effects Not Working

**Symptoms:** No visual change when hovering over shapes

**Solutions:**
1. ✅ Ensure `ShapeHoverFill` or `ShapeHoverStroke` is set
2. ✅ Verify hover colors are different from default colors
3. ✅ On mobile, try tap-and-hold instead of hover
4. ✅ Check that shapes are interactive (not disabled)

**Example:**
```csharp
// Set distinct hover colors
layer.ShapeFill = Colors.LightGray;
layer.ShapeHoverFill = Colors.LightBlue;  // Must be different
layer.ShapeHoverStroke = Colors.Blue;
layer.ShapeHoverStrokeThickness = 2;
```

## Key Takeaways

✅ **Core Syncfusion Maps APIs:**
- `MapShapeLayer` - Main layer for rendering GeoJSON shapes with data binding support
- `MapSource.FromUri()` / `FromFile()` - Methods to load GeoJSON data from various sources
- `EqualColorMapping` / `RangeColorMapping` - Classes for categorical and range-based color schemes
- `DataSource`, `PrimaryValuePath`, `ShapeDataField` - Properties for binding business data to shapes
- `ShapeFill`, `ShapeStroke`, `ShapeColorValuePath` - Properties for shape styling and data-driven coloring

✅ **Essential Concepts:**
- Shape layer renders vector shapes from GeoJSON for choropleth and statistical maps
- Data binding connects business data to geographic shapes via matching property paths
- Color mapping strategies (Equal for categories, Range for numeric data) enable data visualization
- Hover states provide interactive visual feedback for user engagement

✅ **Implementation Patterns:**
- Load GeoJSON → Bind DataSource → Match with PrimaryValuePath/ShapeDataField → Apply ColorMapping
- Use `EqualColorMapping` for discrete categories (High/Medium/Low, regions)
- Use `RangeColorMapping` for continuous numeric data (population density, sales ranges)
- Combine shape layer with markers, data labels, and legends for comprehensive visualizations

✅ **Best Practices:**
- Verify exact case-sensitive matching between `PrimaryValuePath` and model properties
- Ensure `ShapeDataField` matches the GeoJSON property name exactly
- Cover all data values in color mappings to avoid unmapped shapes
- Validate GeoJSON format at geojsonlint.com before use
- Use descriptive color schemes appropriate to data type (sequential, categorical, diverging)

