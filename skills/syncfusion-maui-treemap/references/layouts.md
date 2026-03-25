# Layout Types in .NET MAUI TreeMap

The TreeMap control offers four distinct layout algorithms for organizing hierarchical data effectively. Each layout type provides a different visual arrangement optimized for specific data characteristics and use cases.

## Table of Contents
- [Overview](#overview)
- [Squarified Layout](#squarified-layout)
- [SliceAndDiceAuto Layout](#sliceanddiceauto-layout)
- [SliceAndDiceHorizontal Layout](#sliceanddice

-horizontal-layout)
- [SliceAndDiceVertical Layout](#sliceanddicevertical-layout)
- [Choosing the Right Layout](#choosing-the-right-layout)
- [Changing Layout Dynamically](#changing-layout-dynamically)
- [Layout with Hierarchical Levels](#layout-with-hierarchical-levels)
- [Edge Cases and Considerations](#edge-cases-and-considerations)

## Overview

The `LayoutType` property determines how rectangles are arranged within the TreeMap. Each layout algorithm calculates rectangle sizes based on the `PrimaryValuePath` property value but arranges them differently.

**Available Layout Types:**
- `Squarified` (default) - Optimal aspect ratios, square-like rectangles
- `SliceAndDiceAuto` - Adaptive direction based on available space
- `SliceAndDiceHorizontal` - Horizontal slicing only
- `SliceAndDiceVertical` - Vertical slicing only

**Property:**
```csharp
public LayoutType LayoutType { get; set; }
```

**Enum Values:**
```csharp
public enum LayoutType
{
    Squarified,
    SliceAndDiceAuto,
    SliceAndDiceHorizontal,
    SliceAndDiceVertical
}
```

## Squarified Layout

The Squarified layout is the default and most commonly used layout. It creates square-like rectangles with optimal aspect ratios, making labels more readable and the visualization more balanced.

### Characteristics

- **Default layout** for TreeMap
- Produces **rectangles as close to squares** as possible
- **Optimal aspect ratios** for better readability
- Considers **both width and height** of the parent container
- Best for **general-purpose** data visualization
- **Easier to compare** relative sizes

### When to Use

- General hierarchical data visualization
- When label readability is important
- Comparing relative sizes across many items
- Default choice for most scenarios
- Mixed-size data with varying magnitudes

### XAML Implementation

```xml
<treemap:SfTreeMap x:Name="treeMap"
                   DataSource="{Binding PopulationDetails}"
                   LayoutType="Squarified"
                   PrimaryValuePath="Population"
                   ShowToolTip="True">
    <treemap:SfTreeMap.BindingContext>
        <local:PopulationViewModel />
    </treemap:SfTreeMap.BindingContext>
    
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Country" />
    </treemap:SfTreeMap.LeafItemSettings>
    
    <treemap:SfTreeMap.LeafItemBrushSettings>
        <treemap:TreeMapUniformBrushSettings Brush="#D21243" />
    </treemap:SfTreeMap.LeafItemBrushSettings>
</treemap:SfTreeMap>
```

### C# Implementation

```csharp
SfTreeMap treeMap = new SfTreeMap
{
    DataSource = viewModel.PopulationDetails,
    LayoutType = LayoutType.Squarified,  // Default, can be omitted
    PrimaryValuePath = "Population",
    ShowToolTip = true,
    LeafItemSettings = new TreeMapLeafItemSettings { LabelPath = "Country" },
    LeafItemBrushSettings = new TreeMapUniformBrushSettings 
    { 
        Brush = new SolidColorBrush(Color.FromArgb("#D21243")) 
    }
};

this.Content = treeMap;
```

### Visual Characteristics

- Rectangles appear more **balanced and uniform**
- Easier to **read labels** due to better proportions
- Good **space utilization** with minimal wasted space
- **Visually appealing** for presentations

## SliceAndDiceAuto Layout

The SliceAndDiceAuto layout automatically alternates between horizontal and vertical slicing based on the available space dimensions. It adapts the direction dynamically for optimal space usage.

### Characteristics

- **Adaptive direction** - switches between horizontal and vertical
- Creates **long, thin rectangles** with high aspect ratios
- Direction changes based on **parent container dimensions**
- Starts with the **longer dimension** of the container
- Good for **hierarchical browsing**
- Shows **clear separation** between items

### When to Use

- Responsive layouts that adapt to different screen sizes
- When you want automatic direction optimization
- Hierarchical data with clear categorization
- Timeline or sequential data visualization
- When aspect ratio variation is acceptable

### XAML Implementation

```xml
<treemap:SfTreeMap x:Name="treeMap"
                   DataSource="{Binding PopulationDetails}"
                   LayoutType="SliceAndDiceAuto"
                   PrimaryValuePath="Population">
    <treemap:SfTreeMap.BindingContext>
        <local:PopulationViewModel />
    </treemap:SfTreeMap.BindingContext>
    
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Country" />
    </treemap:SfTreeMap.LeafItemSettings>
    
    <treemap:SfTreeMap.LeafItemBrushSettings>
        <treemap:TreeMapUniformBrushSettings Brush="#D21243" />
    </treemap:SfTreeMap.LeafItemBrushSettings>
</treemap:SfTreeMap>
```

### C# Implementation

```csharp
SfTreeMap treeMap = new SfTreeMap
{
    DataSource = viewModel.PopulationDetails,
    LayoutType = LayoutType.SliceAndDiceAuto,
    PrimaryValuePath = "Population",
    LeafItemSettings = new TreeMapLeafItemSettings { LabelPath = "Country" },
    LeafItemBrushSettings = new TreeMapUniformBrushSettings 
    { 
        Brush = new SolidColorBrush(Color.FromArgb("#D21243")) 
    }
};

this.Content = treeMap;
```

### Visual Characteristics

- Rectangles are **elongated** (high aspect ratio)
- Direction adapts to **container dimensions**
- Clear **visual separation** between groups
- Works well with **rotating devices** (portrait/landscape)

## SliceAndDiceHorizontal Layout

The SliceAndDiceHorizontal layout arranges all rectangles horizontally in rows. Each rectangle is sliced along the horizontal axis, creating a left-to-right arrangement.

### Characteristics

- **Horizontal arrangement only** - all slices are horizontal
- Creates **horizontal bars** or rows
- Rectangle width varies based on data values
- **Consistent height** within each level
- Good for **ranked or ordered data**
- Easy to **compare widths** visually

### When to Use

- Ranked data (e.g., top 10 countries by population)
- Bar chart-like visualizations
- When horizontal comparison is more important
- Limited vertical space, ample horizontal space
- Reading flow from left to right
- Timeline data

### XAML Implementation

```xml
<treemap:SfTreeMap x:Name="treeMap"
                   DataSource="{Binding PopulationDetails}"
                   LayoutType="SliceAndDiceHorizontal"
                   PrimaryValuePath="Population">
    <treemap:SfTreeMap.BindingContext>
        <local:PopulationViewModel />
    </treemap:SfTreeMap.BindingContext>
    
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Country" />
    </treemap:SfTreeMap.LeafItemSettings>
    
    <treemap:SfTreeMap.LeafItemBrushSettings>
        <treemap:TreeMapUniformBrushSettings Brush="#D21243" />
    </treemap:SfTreeMap.LeafItemBrushSettings>
</treemap:SfTreeMap>
```

### C# Implementation

```csharp
SfTreeMap treeMap = new SfTreeMap
{
    DataSource = viewModel.PopulationDetails,
    LayoutType = LayoutType.SliceAndDiceHorizontal,
    PrimaryValuePath = "Population",
    LeafItemSettings = new TreeMapLeafItemSettings { LabelPath = "Country" },
    LeafItemBrushSettings = new TreeMapUniformBrushSettings 
    { 
        Brush = new SolidColorBrush(Color.FromArgb("#D21243")) 
    }
};

this.Content = treeMap;
```

### Visual Characteristics

- Rectangles are arranged in **horizontal rows**
- Width represents **data magnitude**
- Heights are **uniform** within each row
- Resembles **horizontal bar charts**
- Easy **left-to-right scanning**

### Example Use Case: Sales by Region

```csharp
public class SalesData
{
    public string Region { get; set; }
    public decimal Revenue { get; set; }
}

// ViewModel
public class SalesViewModel
{
    public ObservableCollection<SalesData> Sales { get; set; }
    
    public SalesViewModel()
    {
        Sales = new ObservableCollection<SalesData>
        {
            new SalesData { Region = "North America", Revenue = 5500000 },
            new SalesData { Region = "Europe", Revenue = 4200000 },
            new SalesData { Region = "Asia Pacific", Revenue = 3800000 },
            new SalesData { Region = "Latin America", Revenue = 2100000 },
            new SalesData { Region = "Middle East", Revenue = 1500000 }
        };
    }
}
```

```xml
<treemap:SfTreeMap DataSource="{Binding Sales}"
                   LayoutType="SliceAndDiceHorizontal"
                   PrimaryValuePath="Revenue">
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Region" />
    </treemap:SfTreeMap.LeafItemSettings>
</treemap:SfTreeMap>
```

## SliceAndDiceVertical Layout

The SliceAndDiceVertical layout arranges all rectangles vertically in columns. Each rectangle is sliced along the vertical axis, creating a top-to-bottom arrangement.

### Characteristics

- **Vertical arrangement only** - all slices are vertical
- Creates **vertical bars** or columns
- Rectangle height varies based on data values
- **Consistent width** within each level
- Good for **vertical hierarchies**
- Easy to **compare heights** visually

### When to Use

- Hierarchical organizational structures
- Vertical comparison emphasis
- Limited horizontal space, ample vertical space
- Top-to-bottom reading flow
- Drill-down hierarchies
- Tree-like structures

### XAML Implementation

```xml
<treemap:SfTreeMap x:Name="treeMap"
                   DataSource="{Binding PopulationDetails}"
                   LayoutType="SliceAndDiceVertical"
                   PrimaryValuePath="Population"
                   ShowToolTip="True">
    <treemap:SfTreeMap.BindingContext>
        <local:PopulationViewModel />
    </treemap:SfTreeMap.BindingContext>
    
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Country" />
    </treemap:SfTreeMap.LeafItemSettings>
    
    <treemap:SfTreeMap.LeafItemBrushSettings>
        <treemap:TreeMapUniformBrushSettings Brush="#D21243" />
    </treemap:SfTreeMap.LeafItemBrushSettings>
</treemap:SfTreeMap>
```

### C# Implementation

```csharp
SfTreeMap treeMap = new SfTreeMap
{
    DataSource = viewModel.PopulationDetails,
    LayoutType = LayoutType.SliceAndDiceVertical,
    PrimaryValuePath = "Population",
    ShowToolTip = true,
    LeafItemSettings = new TreeMapLeafItemSettings { LabelPath = "Country" },
    LeafItemBrushSettings = new TreeMapUniformBrushSettings 
    { 
        Brush = new SolidColorBrush(Color.FromArgb("#D21243")) 
    }
};

this.Content = treeMap;
```

### Visual Characteristics

- Rectangles are arranged in **vertical columns**
- Height represents **data magnitude**
- Widths are **uniform** within each column
- Resembles **vertical bar charts**
- Easy **top-to-bottom scanning**

## Choosing the Right Layout

### Decision Matrix

| Scenario | Recommended Layout | Reason |
|----------|-------------------|---------|
| General purpose visualization | Squarified | Best readability and balance |
| Comparing relative sizes | Squarified | Square-like shapes easier to compare |
| Ranked/ordered data | SliceAndDiceHorizontal | Bar chart-like comparison |
| Hierarchical structures | SliceAndDiceVertical | Tree-like top-down flow |
| Responsive design | SliceAndDiceAuto | Adapts to orientation changes |
| Timeline data | SliceAndDiceHorizontal | Left-to-right reading flow |
| Organizational charts | SliceAndDiceVertical | Top-down hierarchy |
| Limited vertical space | SliceAndDiceHorizontal | Maximizes horizontal space |
| Limited horizontal space | SliceAndDiceVertical | Maximizes vertical space |
| Presentations | Squarified | Most visually appealing |

### Layout Comparison Table

| Feature | Squarified | SliceAndDiceAuto | SliceAndDiceHorizontal | SliceAndDiceVertical |
|---------|-----------|------------------|----------------------|---------------------|
| **Aspect Ratio** | Optimal (square-like) | High (thin rectangles) | High (wide) | High (tall) |
| **Readability** | Excellent | Good | Good | Good |
| **Space Efficiency** | Excellent | Good | Good | Good |
| **Visual Appeal** | Highest | Moderate | Moderate | Moderate |
| **Comparison Ease** | Excellent | Moderate | Excellent (horizontal) | Excellent (vertical) |
| **Responsive** | Moderate | Excellent | Limited | Limited |
| **Label Fitting** | Easiest | Challenging | Moderate | Moderate |

## Changing Layout Dynamically

You can change the layout at runtime based on user preferences, screen orientation, or data characteristics.

### Using Picker Control

```xml
<StackLayout>
    <Picker x:Name="layoutPicker"
            Title="Select Layout"
            SelectedIndexChanged="OnLayoutChanged">
        <Picker.ItemsSource>
            <x:Array Type="{x:Type x:String}">
                <x:String>Squarified</x:String>
                <x:String>SliceAndDiceAuto</x:String>
                <x:String>SliceAndDiceHorizontal</x:String>
                <x:String>SliceAndDiceVertical</x:String>
            </x:Array>
        </Picker.ItemsSource>
    </Picker>
    
    <treemap:SfTreeMap x:Name="treeMap"
                       DataSource="{Binding PopulationDetails}"
                       PrimaryValuePath="Population">
        <treemap:SfTreeMap.LeafItemSettings>
            <treemap:TreeMapLeafItemSettings LabelPath="Country" />
        </treemap:SfTreeMap.LeafItemSettings>
    </treemap:SfTreeMap>
</StackLayout>
```

### Code-Behind

```csharp
private void OnLayoutChanged(object sender, EventArgs e)
{
    var picker = (Picker)sender;
    var selectedLayout = picker.SelectedItem.ToString();
    
    treeMap.LayoutType = selectedLayout switch
    {
        "Squarified" => LayoutType.Squarified,
        "SliceAndDiceAuto" => LayoutType.SliceAndDiceAuto,
        "SliceAndDiceHorizontal" => LayoutType.SliceAndDiceHorizontal,
        "SliceAndDiceVertical" => LayoutType.SliceAndDiceVertical,
        _ => LayoutType.Squarified
    };
}
```

### Based on Screen Orientation

```csharp
protected override void OnSizeAllocated(double width, double height)
{
    base.OnSizeAllocated(width, height);
    
    // Change layout based on orientation
    if (width > height)
    {
        // Landscape - use horizontal layout
        treeMap.LayoutType = LayoutType.SliceAndDiceHorizontal;
    }
    else
    {
        // Portrait - use vertical layout
        treeMap.LayoutType = LayoutType.SliceAndDiceVertical;
    }
}
```

## Layout with Hierarchical Levels

When using multiple levels (GroupPath), the layout applies recursively to each level.

### Squarified with Levels

```xml
<treemap:SfTreeMap DataSource="{Binding PopulationDetails}"
                   LayoutType="Squarified"
                   PrimaryValuePath="Population">
    <treemap:SfTreeMap.Levels>
        <treemap:TreeMapLevel GroupPath="Continent" />
    </treemap:SfTreeMap.Levels>
    
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Country" />
    </treemap:SfTreeMap.LeafItemSettings>
</treemap:SfTreeMap>
```

Each continent group uses Squarified layout, and within each continent, countries are also arranged using Squarified layout.

### SliceAndDiceHorizontal with Levels

```xml
<treemap:SfTreeMap DataSource="{Binding SalesData}"
                   LayoutType="SliceAndDiceHorizontal"
                   PrimaryValuePath="Revenue">
    <treemap:SfTreeMap.Levels>
        <treemap:TreeMapLevel GroupPath="Region" />
        <treemap:TreeMapLevel GroupPath="Country" />
    </treemap:SfTreeMap.Levels>
    
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Product" />
    </treemap:SfTreeMap.LeafItemSettings>
</treemap:SfTreeMap>
```

Region groups are arranged horizontally, countries within regions are arranged horizontally, and products within countries are arranged horizontally.

## Edge Cases and Considerations

### Very Small Items

**Issue**: With Squarified layout, very small items might become tiny squares that are hard to see.

**Solution**: 
- Use SliceAndDiceHorizontal or SliceAndDiceVertical to give small items more visible width or height
- Filter out or aggregate very small values
- Use a minimum size threshold

```csharp
// Filter small values
var filteredData = sourceData.Where(item => item.Value > minThreshold);
treeMap.DataSource = filteredData;
```

### Single Large Item

**Issue**: One item dominates the TreeMap, making others too small.

**Solution**:
- Use logarithmic scaling in your data
- Separate the large item into its own visualization
- Apply a cap to maximum values

### Uneven Data Distribution

**Issue**: Some items are 1000x larger than others.

**Solution**:
- Squarified layout handles this best
- Consider grouping or aggregating data
- Use range-based coloring to highlight differences

### Long Labels in SliceAndDice Layouts

**Issue**: Labels don't fit in thin rectangles.

**Solution**:
- Use label trimming or wrapping
- Reduce font size
- Show labels only for larger items
- Use tooltips for full text

```xml
<treemap:SfTreeMap.LeafItemSettings>
    <treemap:TreeMapLeafItemSettings LabelPath="Country"
                                     OverflowMode="Trim">
        <treemap:TreeMapLeafItemSettings.TextStyle>
            <treemap:TreeMapTextStyle FontSize="10" />
        </treemap:TreeMapLeafItemSettings.TextStyle>
    </treemap:TreeMapLeafItemSettings>
</treemap:SfTreeMap.LeafItemSettings>
```

### Performance with Large Datasets

**Issue**: Squarified layout calculation can be slower with thousands of items.

**Solution**:
- Limit the number of items displayed
- Use SliceAndDice layouts for faster rendering
- Aggregate data into fewer categories
- Implement pagination or filtering

### Empty or Zero Values

**Issue**: Items with zero or negative values won't display.

**Solution**:
- Filter out zero/negative values before binding
- Use absolute values if negatives are meaningful
- Add a minimum value offset

```csharp
// Ensure all values are positive
var processedData = sourceData.Select(item => new DataModel
{
    Name = item.Name,
    Value = Math.Max(item.Value, 1) // Minimum value of 1
}).ToList();
```

## Related Topics

- [Hierarchical Levels](hierarchical-levels.md) - Multi-level grouping with layouts
- [Leaf Item Customization](leaf-item-customization.md) - Customizing item appearance
- [Getting Started](getting-started.md) - Basic TreeMap setup

## Summary

- **Squarified**: Best for general-purpose visualization with optimal readability
- **SliceAndDiceAuto**: Adapts to screen dimensions, good for responsive designs
- **SliceAndDiceHorizontal**: Best for ranked data and horizontal comparisons
- **SliceAndDiceVertical**: Best for hierarchical structures and vertical comparisons
- Choose based on your data characteristics, available space, and user needs
- Layouts can be changed dynamically based on user preference or screen orientation
