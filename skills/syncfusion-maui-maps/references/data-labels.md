# Data Labels in Syncfusion .NET MAUI Maps (SfMaps)

## Table of Contents
- [Overview](#overview)
- [Key APIs Used](#key-apis-used)
- [Show Data Labels](#show-data-labels)
- [Overflow Mode](#overflow-mode)
- [Appearance Customization](#appearance-customization)
- [Best Practices](#best-practices)

## Overview

Data labels provide identification for map shapes by displaying their names. You can trim or hide the labels if they exceed the shape bounds. The Syncfusion .NET MAUI Maps control provides comprehensive data label functionality through the `MapDataLabelSettings` class and related properties.

**Use data labels for:**
- State/country names
- Region codes or abbreviations
- Statistical values
- Classification labels
- Quick shape identification

**Key features:**
- Automatic positioning within shapes
- Smart overflow handling (trim/hide)
- Customizable styling via MapLabelStyle
- Path-based data binding

## Key APIs Used

### Primary Classes and Properties

| API | Type | Description |
|-----|------|-------------|
| `MapShapeLayer.ShowDataLabels` | bool | Controls the visibility of data labels on the map |
| `MapShapeLayer.DataLabelSettings` | MapDataLabelSettings | Configuration object for data label settings |
| `MapDataLabelSettings` | class | Provides settings for customizing data labels |
| `MapDataLabelSettings.DataLabelPath` | string | Property name from data source to display as label |
| `MapDataLabelSettings.OverflowMode` | MapLabelOverflowMode | Controls label behavior when overflowing shape bounds |
| `MapDataLabelSettings.DataLabelStyle` | MapLabelStyle | Style settings for data labels |
| `MapLabelStyle` | class | Defines font size, color, and attributes for labels |
| `MapLabelOverflowMode` | enum | None, Trim, Hide options for overflow handling |

### MapLabelOverflowMode Enumeration

| Value | Description |
|-------|-------------|
| `None` | Labels render even if they overflow shape bounds (default) |
| `Trim` | Truncates labels that overflow with ellipsis |
| `Hide` | Completely hides labels that don't fit within shape bounds |

### MapLabelStyle Properties

| Property | Type | Description |
|----------|------|-------------|
| `FontSize` | double | Size of the label text |
| `TextColor` | Color | Color of the label text |
| `FontAttributes` | FontAttributes | None, Bold, Italic, or combination |
| `FontFamily` | string | Font family name for label text |

## Show Data Labels

You can show data labels on the map using the `ShowDataLabels` and `DataLabelPath` properties. The `ShowDataLabels` property is used to control the visibility of data labels, while the `DataLabelPath` property is used to decide which underlying property has to be displayed as data labels. The default value of `ShowDataLabels` is `false`.

### XAML Example

```xaml
<map:SfMaps>
    <map:SfMaps.Layer>
        <map:MapShapeLayer ShapesSource="url"
                           DataSource="{Binding Data}"        
                           PrimaryValuePath="State" 
                           ShapeDataField="STATE_NAME" 
                           ShowDataLabels="True">

            <map:MapShapeLayer.DataLabelSettings>
                <map:MapDataLabelSettings DataLabelPath="State" />
            </map:MapShapeLayer.DataLabelSettings>
        </map:MapShapeLayer>
    </map:SfMaps.Layer>
</map:SfMaps>
```

### C# Example

```csharp
using Syncfusion.Maui.Maps;
using System.Collections.ObjectModel;

namespace MauiMapsApp;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        ViewModel viewModel = new ViewModel();
        this.BindingContext = viewModel;
        
        MapShapeLayer layer = new MapShapeLayer();
        layer.ShapesSource = MapSource.FromUri("url"); // For e.g "https://cdn.syncfusion.com/maps/australia.json"
        layer.DataSource = viewModel.Data;
        layer.PrimaryValuePath = "State";
        layer.ShapeDataField = "STATE_NAME";
        layer.ShowDataLabels = true;

        layer.DataLabelSettings = new MapDataLabelSettings()
        {
            DataLabelPath = "State",
        };
        
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
        Data.Add(new Model("New South Wales", "New\nSouth Wales"));
        Data.Add(new Model("Queensland", "Queensland"));
        Data.Add(new Model("Northern Territory", "Northern\nTerritory"));
        Data.Add(new Model("Victoria", "Victoria"));
        Data.Add(new Model("Tasmania", "Tasmania"));
        Data.Add(new Model("Western Australia", "Western Australia"));
        Data.Add(new Model("South Australia", "South Australia"));
    }
}

public class Model
{
    public string State { get; set; }
    public string StateCode { get; set; }
    
    public Model(string state, string stateCode)
    {
        State = state;
        StateCode = stateCode;
    }
}
```

**Note:** The `DataLabelPath` property name is case-sensitive and must match exactly with the property name in your data model.

## Overflow Mode

You can trim or remove the data label when it is overflowed from the shape using the `OverflowMode` property. The possible values are `None`, `Trim`, and `Hide`. The default value of the `OverflowMode` property is `MapLabelOverflowMode.None`.

By default, the data labels will render even if they overflow from the shape.

### XAML Example

```xaml
<map:SfMaps>
    <map:SfMaps.Layer>
        <map:MapShapeLayer ShapesSource="url"
                           DataSource="{Binding Data}"        
                           PrimaryValuePath="State" 
                           ShapeDataField="STATE_NAME" 
                           ShowDataLabels="True">

            <map:MapShapeLayer.DataLabelSettings>
                <map:MapDataLabelSettings OverflowMode="Trim"
                                          DataLabelPath="State" />
            </map:MapShapeLayer.DataLabelSettings>
        </map:MapShapeLayer>
    </map:SfMaps.Layer>
</map:SfMaps>
```

### C# Example

```csharp
using Syncfusion.Maui.Maps;
using System.Collections.ObjectModel;

namespace MauiMapsApp;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        ViewModel viewModel = new ViewModel();
        this.BindingContext = viewModel;
        
        MapShapeLayer layer = new MapShapeLayer();
        layer.ShapesSource = MapSource.FromUri("url"); // For e.g "https://cdn.syncfusion.com/maps/map-data/australia.json"
        layer.DataSource = viewModel.Data;
        layer.PrimaryValuePath = "State";
        layer.ShapeDataField = "STATE_NAME";
        layer.ShowDataLabels = true;

        layer.DataLabelSettings = new MapDataLabelSettings()
        {
            DataLabelPath = "State",
            OverflowMode = MapLabelOverflowMode.Trim,
        };
        
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
        Data.Add(new Model("New South Wales", "New South Wales"));
        Data.Add(new Model("Queensland", "Queensland"));
        Data.Add(new Model("Northern Territory", "Northern Territory"));
        Data.Add(new Model("Victoria", "Victoria"));
        Data.Add(new Model("Tasmania", "Tasmania"));
        Data.Add(new Model("Western Australia", "Western Australia"));
        Data.Add(new Model("South Australia", "South Australia"));
    }
}

public class Model
{
    public string State { get; set; }
    public string StateCode { get; set; }
    
    public Model(string state, string stateCode)
    {
        State = state;
        StateCode = stateCode;
    }
}
```

## Appearance Customization

You can customize the data labels using the `DataLabelStyle` property of `MapDataLabelSettings`. The `DataLabelStyle` property allows you to specify the font size, color, font attributes, and font family for data labels.

### XAML Example

```xaml
<map:SfMaps>
    <map:SfMaps.Layer>
        <map:MapShapeLayer ShapesSource="url"
                           DataSource="{Binding Data}" 
                           PrimaryValuePath="State" 
                           ShapeDataField="STATE_NAME" 
                           ShowDataLabels="True">

            <map:MapShapeLayer.DataLabelSettings>
                <map:MapDataLabelSettings OverflowMode="Trim"
                                          DataLabelPath="State">
                    <map:MapDataLabelSettings.DataLabelStyle>
                        <map:MapLabelStyle FontSize="12"
                                           TextColor="#ff4e41"
                                           FontAttributes="Italic" />
                    </map:MapDataLabelSettings.DataLabelStyle>
                </map:MapDataLabelSettings>
            </map:MapShapeLayer.DataLabelSettings>

        </map:MapShapeLayer>
    </map:SfMaps.Layer>
</map:SfMaps>
```

### C# Example

```csharp
using Syncfusion.Maui.Maps;
using System.Collections.ObjectModel;

namespace MauiMapsApp;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        ViewModel viewModel = new ViewModel();
        this.BindingContext = viewModel;
        
        MapShapeLayer layer = new MapShapeLayer();
        layer.ShapesSource = MapSource.FromUri("url"); // For e.g "https://cdn.syncfusion.com/maps/map-data/australia.json"
        layer.DataSource = viewModel.Data;
        layer.PrimaryValuePath = "State";
        layer.ShapeDataField = "STATE_NAME";
        layer.ShowDataLabels = true;

        layer.DataLabelSettings = new MapDataLabelSettings()
        {
            DataLabelPath = "State",
            OverflowMode = MapLabelOverflowMode.Trim,
            DataLabelStyle = new MapLabelStyle()
            {
                FontSize = 12,
                FontAttributes = FontAttributes.Italic,
                TextColor = Color.FromRgb(255, 78, 65)
            },
        };
        
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
        Data.Add(new Model("New South Wales", "New\nSouth Wales"));
        Data.Add(new Model("Queensland", "Queensland"));
        Data.Add(new Model("Northern Territory", "Northern\nTerritory"));
        Data.Add(new Model("Victoria", "Victoria"));
        Data.Add(new Model("Tasmania", "Tasmania"));
        Data.Add(new Model("Western Australia", "Western Australia"));
        Data.Add(new Model("South Australia", "South Australia"));
    }
}

public class Model
{
    public string State { get; set; }
    public string StateCode { get; set; }
    
    public Model(string state, string stateCode)
    {
        State = state;
        StateCode = stateCode;
    }
}
```

### Key APIs Used

- **`MapDataLabelSettings.DataLabelStyle`** - Style settings for data labels
- **`MapLabelStyle`** - Defines font size, color, and attributes for labels
- **`MapLabelStyle.FontSize`** - Size of the label text
- **`MapLabelStyle.TextColor`** - Color of the label text
- **`MapLabelStyle.FontAttributes`** - Bold, Italic, or combination
- **`MapLabelStyle.FontFamily`** - Font family name for label text

### MapLabelStyle Properties

| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `FontSize` | double | Size of the label text | `FontSize="12"` |
| `TextColor` | Color | Color of the label text | `TextColor="#ff4e41"` or `Color.FromRgb(255, 78, 65)` |
| `FontAttributes` | FontAttributes | None, Bold, Italic, or combination | `FontAttributes="Italic"` |
| `FontFamily` | string | Font family name | `FontFamily="Arial"` |

## Best Practices

### 1. Choose Appropriate Label Content

Use short, concise labels for better readability, especially when dealing with many small shapes:

```csharp
// Good practice - Use abbreviations for maps with many shapes
public class StateData
{
    public string Name { get; set; }        // "California"
    public string Code { get; set; }        // "CA"
}
```

```xaml
<!-- Use Code property for better readability -->
<map:MapDataLabelSettings DataLabelPath="Code" />
```

### 2. Font Size Based on Map Complexity

Choose font sizes appropriate to the number and size of shapes:

```csharp
// Few large shapes (countries, states)
layer.DataLabelSettings = new MapDataLabelSettings
{
    DataLabelPath = "Name",
    DataLabelStyle = new MapLabelStyle { FontSize = 14 }
};

// Many medium shapes (counties, regions)
layer.DataLabelSettings = new MapDataLabelSettings
{
    DataLabelPath = "Code",
    DataLabelStyle = new MapLabelStyle { FontSize = 11 }
};

// Many small shapes (districts, zones)
layer.DataLabelSettings = new MapDataLabelSettings
{
    DataLabelPath = "Code",
    DataLabelStyle = new MapLabelStyle { FontSize = 9 },
    OverflowMode = MapLabelOverflowMode.Hide
};
```

### 3. Ensure Text Contrast

Ensure sufficient color contrast between labels and shape fills for readability:

```xaml
<!-- Light shapes: use dark text -->
<map:MapShapeLayer ShapeFill="LightBlue" ShowDataLabels="True">
    <map:MapShapeLayer.DataLabelSettings>
        <map:MapDataLabelSettings DataLabelPath="Name">
            <map:MapDataLabelSettings.DataLabelStyle>
                <map:MapLabelStyle TextColor="DarkBlue" />
            </map:MapDataLabelSettings.DataLabelStyle>
        </map:MapDataLabelSettings>
    </map:MapShapeLayer.DataLabelSettings>
</map:MapShapeLayer>

<!-- Dark shapes: use light text -->
<map:MapShapeLayer ShapeFill="DarkBlue" ShowDataLabels="True">
    <map:MapShapeLayer.DataLabelSettings>
        <map:MapDataLabelSettings DataLabelPath="Name">
            <map:MapDataLabelSettings.DataLabelStyle>
                <map:MapLabelStyle TextColor="White" />
            </map:MapDataLabelSettings.DataLabelStyle>
        </map:MapDataLabelSettings>
    </map:MapShapeLayer.DataLabelSettings>
</map:MapShapeLayer>
```

### 4. Use OverflowMode Strategically

Select the appropriate overflow mode based on your map requirements:

```csharp
// For professional, clean maps - hide labels that don't fit
layer.DataLabelSettings = new MapDataLabelSettings
{
    OverflowMode = MapLabelOverflowMode.Hide
};

// For informational maps - show partial text
layer.DataLabelSettings = new MapDataLabelSettings
{
    OverflowMode = MapLabelOverflowMode.Trim
};

// For development/testing - show all labels
layer.DataLabelSettings = new MapDataLabelSettings
{
    OverflowMode = MapLabelOverflowMode.None
};
```

### 5. Multi-line Labels

Use newline characters (`\n`) in your data for multi-line labels:

```csharp
public class ViewModel
{
    public ObservableCollection<Model> Data { get; set; }
    
    public ViewModel()
    {
        Data = new ObservableCollection<Model>();
        // Use \n for line breaks in label text
        Data.Add(new Model("New South Wales", "New\nSouth\nWales"));
        Data.Add(new Model("Northern Territory", "Northern\nTerritory"));
    }
}
```

### 6. Complete Styling Example

Combine all styling properties for fully customized labels:

```xaml
<map:MapShapeLayer ShowDataLabels="True">
    <map:MapShapeLayer.DataLabelSettings>
        <map:MapDataLabelSettings DataLabelPath="Code"
                                  OverflowMode="Trim">
            <map:MapDataLabelSettings.DataLabelStyle>
                <map:MapLabelStyle FontSize="12"
                                   FontAttributes="Bold"
                                   FontFamily="Arial"
                                   TextColor="#2C3E50" />
            </map:MapDataLabelSettings.DataLabelStyle>
        </map:MapDataLabelSettings>
    </map:MapShapeLayer.DataLabelSettings>
</map:MapShapeLayer>
```

## Common Scenarios

### Scenario 1: US State Map with Abbreviations

Display state abbreviations on a US map for quick identification:

```csharp
using Syncfusion.Maui.Maps;
using System.Collections.ObjectModel;

namespace MauiMapsApp;

public class StateViewModel
{
    public ObservableCollection<StateData> States { get; set; }
    
    public StateViewModel()
    {
        States = new ObservableCollection<StateData>();
        States.Add(new StateData { Name = "California", Code = "CA" });
        States.Add(new StateData { Name = "Texas", Code = "TX" });
        States.Add(new StateData { Name = "New York", Code = "NY" });
        // Add more states...
    }
}

public class StateData
{
    public string Name { get; set; }
    public string Code { get; set; }
}
```

```xaml
<map:SfMaps>
    <map:SfMaps.Layer>
        <map:MapShapeLayer ShapesSource="url"
                           DataSource="{Binding States}"
                           PrimaryValuePath="Name"
                           ShapeDataField="name"
                           ShowDataLabels="True">

            <map:MapShapeLayer.DataLabelSettings>
                <map:MapDataLabelSettings DataLabelPath="Code"
                                          OverflowMode="Hide">
                    <map:MapDataLabelSettings.DataLabelStyle>
                        <map:MapLabelStyle FontSize="12"
                                           FontAttributes="Bold"
                                           TextColor="#2C3E50" />
                    </map:MapDataLabelSettings.DataLabelStyle>
                </map:MapDataLabelSettings>
            </map:MapShapeLayer.DataLabelSettings>
        </map:MapShapeLayer>
    </map:SfMaps.Layer>
</map:SfMaps>
```

### Scenario 2: Population Density Map with Multi-line Labels

Display region names with population density:

```csharp
using Syncfusion.Maui.Maps;
using System.Collections.ObjectModel;

namespace MauiMapsApp;

public class RegionViewModel
{
    public ObservableCollection<RegionData> Regions { get; set; }
    
    public RegionViewModel()
    {
        Regions = new ObservableCollection<RegionData>();
        Regions.Add(new RegionData 
        { 
            Region = "New South Wales", 
            DisplayLabel = "New South\nWales\n10.2M" 
        });
        Regions.Add(new RegionData 
        { 
            Region = "Queensland", 
            DisplayLabel = "Queensland\n5.2M" 
        });
        // Add more regions...
    }
}

public class RegionData
{
    public string Region { get; set; }
    public string DisplayLabel { get; set; }
}
```

```xaml
<map:SfMaps>
    <map:SfMaps.Layer>
        <map:MapShapeLayer ShapesSource="url"
                           DataSource="{Binding Regions}"
                           PrimaryValuePath="Region"
                           ShapeDataField="STATE_NAME"
                           ShowDataLabels="True">

            <map:MapShapeLayer.DataLabelSettings>
                <map:MapDataLabelSettings DataLabelPath="DisplayLabel"
                                          OverflowMode="Trim">
                    <map:MapDataLabelSettings.DataLabelStyle>
                        <map:MapLabelStyle FontSize="10"
                                           TextColor="Black" />
                    </map:MapDataLabelSettings.DataLabelStyle>
                </map:MapDataLabelSettings>
            </map:MapShapeLayer.DataLabelSettings>
        </map:MapShapeLayer>
    </map:SfMaps.Layer>
</map:SfMaps>
```

### Scenario 3: Themed Labels with Custom Styling

Create visually styled labels with colors:

```csharp
using Syncfusion.Maui.Maps;
using System.Collections.ObjectModel;

namespace MauiMapsApp;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        var viewModel = new CountryViewModel();
        this.BindingContext = viewModel;
        
        MapShapeLayer layer = new MapShapeLayer();
        layer.ShapesSource = MapSource.FromUri("url"); // For e.g "https://cdn.syncfusion.com/maps/map-data/world-map.json"
        layer.DataSource = viewModel.Countries;
        layer.PrimaryValuePath = "Name";
        layer.ShapeDataField = "name";
        layer.ShowDataLabels = true;

        layer.DataLabelSettings = new MapDataLabelSettings()
        {
            DataLabelPath = "Code",
            OverflowMode = MapLabelOverflowMode.Hide,
            DataLabelStyle = new MapLabelStyle()
            {
                FontSize = 11,
                FontAttributes = FontAttributes.Bold,
                FontFamily = "Arial",
                TextColor = Color.FromRgb(0, 51, 102)
            }
        };
        
        SfMaps maps = new SfMaps();
        maps.Layer = layer;
        this.Content = maps;
    }
}

public class CountryViewModel
{
    public ObservableCollection<CountryData> Countries { get; set; }
    
    public CountryViewModel()
    {
        Countries = new ObservableCollection<CountryData>();
        Countries.Add(new CountryData { Name = "United States", Code = "USA" });
        Countries.Add(new CountryData { Name = "China", Code = "CHN" });
        Countries.Add(new CountryData { Name = "India", Code = "IND" });
        // Add more countries...
    }
}

public class CountryData
{
    public string Name { get; set; }
    public string Code { get; set; }
}
```

## API Summary

### Namespace

All data label APIs are available in the `Syncfusion.Maui.Maps` namespace:

```csharp
using Syncfusion.Maui.Maps;
```

## Key Takeaways

✅ **Core Syncfusion Maps APIs:**
- `MapShapeLayer.ShowDataLabels` - Property to enable/disable data label visibility (default: false)
- `MapDataLabelSettings` - Configuration class for all data label customization
- `DataLabelPath` - Property specifying which data model property to display as label text
- `OverflowMode` - Property controlling label behavior when exceeding shape bounds (None, Trim, Hide)
- `MapLabelStyle` - Styling class with FontSize, TextColor, FontAttributes, and FontFamily properties

✅ **Essential Concepts:**
- Data labels provide identification for map shapes by displaying text within shape boundaries
- Labels automatically position themselves within shapes for optimal readability
- Overflow handling ensures labels don't clutter the map (None=show all, Trim=ellipsis, Hide=remove)
- Styling applies uniformly to all labels but can be customized per-layer with MapLabelStyle
- Path-based data binding connects labels to specific properties in the data source

✅ **Implementation Patterns:**
- Basic setup: Enable `ShowDataLabels`, set `DataLabelPath` to property name in data model
- Overflow control: Use `Trim` for partial text visibility, `Hide` for clean professional maps
- Multi-line labels: Include `\n` characters in data strings for line breaks
- Custom styling: Configure `DataLabelStyle` with FontSize, TextColor, FontAttributes for branding
- Short labels: Use abbreviations or codes for maps with many small shapes

✅ **Best Practices:**
- Choose font sizes based on map complexity (large shapes: 14px, medium: 11px, small: 9px)
- Use abbreviations or short codes (`DataLabelPath="Code"`) for dense maps with many shapes
- Ensure sufficient color contrast between label text and shape fill colors
- Apply `OverflowMode.Hide` for professional clean maps, `Trim` for informational displays
- Property names in `DataLabelPath` are case-sensitive and must match data model exactly
- Use `OverflowMode.None` only during development/testing to see all labels

## Related Topics

- **Shape Layer** - Configure the base map layer with shapes
- **Tooltips** - Add interactive tooltips for detailed information
- **Legends** - Display legends for data visualization
- **Color Mapping** - Apply color schemes based on data values

