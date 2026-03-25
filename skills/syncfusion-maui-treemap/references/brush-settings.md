# Brush Settings in .NET MAUI TreeMap

Brush settings control how leaf items are colored in the TreeMap. The TreeMap offers four brush setting types, each providing unique ways to visualize data through color.

## Table of Contents
- [Overview](#overview)
- [Uniform Brush Settings](#uniform-brush-settings)
- [Range Brush Settings](#range-brush-settings)
- [Desaturation Brush Settings](#desaturation-brush-settings)
- [Palette Brush Settings](#palette-brush-settings)
- [Choosing the Right Brush Setting](#choosing-the-right-brush-setting)
- [Combining with Legends](#combining-with-legends)
- [Practical Examples](#practical-examples)
- [Troubleshooting](#troubleshooting)

## Overview

The `LeafItemBrushSettings` property determines how leaf item rectangles are colored. Each brush setting type serves different visualization needs:

| Brush Setting | Use Case | Color Basis |
|---------------|----------|-------------|
| **UniformBrushSettings** | Single color for all items | Fixed color |
| **RangeBrushSettings** | Color by value ranges | Data ranges (From/To) |
| **DesaturationBrushSettings** | Gradient by saturation | Relative data values |
| **PaletteBrushSettings** | Multiple distinct colors | Sequential palette |

**Property:**
```csharp
public TreeMapBrushSettings LeafItemBrushSettings { get; set; }
```

## Uniform Brush Settings

Apply a single, uniform color to all leaf items regardless of their values.

### When to Use
- Simple visualizations without color-based data representation
- Aesthetic consistency across all items
- Focus on size rather than color differentiation
- Placeholder during development

### Basic Implementation

```xml
<treemap:SfTreeMap DataSource="{Binding PopulationDetails}"
                   PrimaryValuePath="Population">
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Country" />
    </treemap:SfTreeMap.LeafItemSettings>
    
    <treemap:SfTreeMap.LeafItemBrushSettings>
        <treemap:TreeMapUniformBrushSettings Brush="Orange" />
    </treemap:SfTreeMap.LeafItemBrushSettings>
</treemap:SfTreeMap>
```

### C# Implementation

```csharp
treeMap.LeafItemBrushSettings = new TreeMapUniformBrushSettings 
{ 
    Brush = Brush.Orange 
};
```

### Using Hex Colors

```xml
<treemap:TreeMapUniformBrushSettings Brush="#FF6B35" />
```

```csharp
treeMap.LeafItemBrushSettings = new TreeMapUniformBrushSettings 
{ 
    Brush = new SolidColorBrush(Color.FromArgb("#FF6B35")) 
};
```

### Using Named Colors

```csharp
treeMap.LeafItemBrushSettings = new TreeMapUniformBrushSettings 
{ 
    Brush = Brush.CornflowerBlue 
};
```

## Range Brush Settings

Color items based on data value ranges. Each range gets a specific color, ideal for categorizing data into meaningful groups.

### When to Use
- Visualizing data categories (low/medium/high)
- Highlighting thresholds or benchmarks
- Creating heat maps with discrete color zones
- Showing performance levels (poor/good/excellent)
- Displaying legends with range labels

### Key Properties

**TreeMapRangeBrush:**
- `From` (double): Start value of the range
- `To` (double): End value of the range  
- `Brush` (Brush): Color for items in this range
- `LegendLabel` (string): Text shown in legend for this range

### Basic Implementation

```xml
<treemap:SfTreeMap DataSource="{Binding PopulationDetails}"
                   PrimaryValuePath="Population"
                   RangeColorValuePath="Population">
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Country" />
    </treemap:SfTreeMap.LeafItemSettings>
    
    <treemap:SfTreeMap.LeafItemBrushSettings>
        <treemap:TreeMapRangeBrushSettings>
            <treemap:TreeMapRangeBrushSettings.RangeBrushes>
                <treemap:TreeMapRangeBrush LegendLabel="50M - 1B"
                                           From="50000000"
                                           To="1000000000" 
                                           Brush="#F0A868" />
                <treemap:TreeMapRangeBrush LegendLabel="10M - 50M"
                                           From="10000000"
                                           To="50000000" 
                                           Brush="#F3BC8B" />
                <treemap:TreeMapRangeBrush LegendLabel="0.1M - 10M"
                                           From="100000" 
                                           To="10000000"  
                                           Brush="#F8D7B9" />
            </treemap:TreeMapRangeBrushSettings.RangeBrushes>
        </treemap:TreeMapRangeBrushSettings>
    </treemap:SfTreeMap.LeafItemBrushSettings>
</treemap:SfTreeMap>
```

### C# Implementation

```csharp
treeMap.RangeColorValuePath = "Population";
treeMap.LeafItemBrushSettings = new TreeMapRangeBrushSettings
{
    RangeBrushes = new List<TreeMapRangeBrush>
    {
        new TreeMapRangeBrush 
        { 
            LegendLabel = "50M - 1B", 
            From = 50000000, 
            To = 1000000000, 
            Brush = new SolidColorBrush(Color.FromArgb("#F0A868")) 
        },
        new TreeMapRangeBrush 
        { 
            LegendLabel = "10M - 50M", 
            From = 10000000, 
            To = 50000000, 
            Brush = new SolidColorBrush(Color.FromArgb("#F3BC8B")) 
        },
        new TreeMapRangeBrush 
        { 
            LegendLabel = "0.1M - 10M", 
            From = 100000, 
            To = 10000000, 
            Brush = new SolidColorBrush(Color.FromArgb("#F8D7B9")) 
        }
    }
};
```

### Important: RangeColorValuePath Required

```xml
<!-- MUST set RangeColorValuePath for RangeBrushSettings to work -->
<treemap:SfTreeMap RangeColorValuePath="Population"
                   PrimaryValuePath="Population">
```

The `RangeColorValuePath` specifies which property value determines the color range. It can be the same as or different from `PrimaryValuePath`.

### Range Overlap Handling

**Non-overlapping ranges (recommended):**
```csharp
{ From = 0, To = 10000 }
{ From = 10000, To = 50000 }
{ From = 50000, To = 100000 }
```

**Overlapping ranges (first match wins):**
```csharp
{ From = 0, To = 10000 }     // Value 5000: Uses this
{ From = 5000, To = 15000 }  // Value 5000: Skipped (already matched)
```

### Values Outside Ranges

Items with values outside all defined ranges use a default color or remain uncolored.

```csharp
// Ranges: 100-1000, 1000-10000
// Value 50: Falls below all ranges (default color)
// Value 15000: Falls above all ranges (default color)
```

**Solution**: Add catch-all ranges:
```csharp
new TreeMapRangeBrush { From = 0, To = 100, Brush = ... },           // Below
new TreeMapRangeBrush { From = 100, To = 1000, Brush = ... },
new TreeMapRangeBrush { From = 1000, To = 10000, Brush = ... },
new TreeMapRangeBrush { From = 10000, To = double.MaxValue, Brush = ... }  // Above
```

## Desaturation Brush Settings

Apply a gradient from full saturation to desaturated (pale) based on data values. Larger values get fuller saturation, smaller values become paler.

### When to Use
- Smooth gradients without hard boundaries
- Emphasizing larger values with vibrant colors
- De-emphasizing smaller values with pale colors
- Heat map style visualizations
- Single-color theme with intensity variation

### Key Properties

- `Brush` (Brush): Base color to desaturate
- `From` (double): Saturation level for smallest values (0.0 - 1.0)
- `To` (double): Saturation level for largest values (0.0 - 1.0)

**Saturation Range:**
- `1.0` = Full saturation (vibrant)
- `0.5` = Half saturation  
- `0.0` = No saturation (grayscale)

### Basic Implementation

```xml
<treemap:SfTreeMap DataSource="{Binding PopulationDetails}"
                   PrimaryValuePath="Population">
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Country" />
    </treemap:SfTreeMap.LeafItemSettings>
    
    <treemap:SfTreeMap.LeafItemBrushSettings>
        <treemap:TreeMapDesaturationBrushSettings Brush="BlueViolet" 
                                                   From="1" 
                                                   To="0.2" />
    </treemap:SfTreeMap.LeafItemBrushSettings>
</treemap:SfTreeMap>
```

### C# Implementation

```csharp
treeMap.LeafItemBrushSettings = new TreeMapDesaturationBrushSettings 
{ 
    Brush = Brush.BlueViolet, 
    From = 1.0,    // Largest items: full saturation
    To = 0.2       // Smallest items: 20% saturation (pale)
};
```

### Gradient Direction

**From High to Low (typical):**
```csharp
From = 1.0,  // Largest values: vibrant
To = 0.2     // Smallest values: pale
```

**From Low to High (inverse):**
```csharp
From = 0.2,  // Largest values: pale
To = 1.0     // Smallest values: vibrant
```

### Color Examples

```csharp
// Vibrant blues for large items, pale blues for small
Brush = Brush.DodgerBlue, From = 1.0, To = 0.3

// Vibrant reds for large items, pale reds for small
Brush = new SolidColorBrush(Color.FromArgb("#D32F2F")), From = 1.0, To = 0.25

// Vibrant greens for large items, almost gray for small
Brush = Brush.LimeGreen, From = 1.0, To = 0.1
```

## Palette Brush Settings

Apply multiple distinct colors from a palette sequentially to leaf items. Each item gets the next color in the palette.

### When to Use
- Distinguishing between individual items
- Categorical data without numeric significance
- Visual variety without data-based meaning
- Making items easily identifiable
- Aesthetic diversity

### Basic Implementation

```xml
<treemap:SfTreeMap DataSource="{Binding PopulationDetails}"
                   PrimaryValuePath="Population">
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Country" />
    </treemap:SfTreeMap.LeafItemSettings>
    
    <treemap:SfTreeMap.LeafItemBrushSettings>
        <treemap:TreeMapPaletteBrushSettings>
            <treemap:TreeMapPaletteBrushSettings.Brushes>
                <SolidColorBrush>#116DF9</SolidColorBrush>
                <SolidColorBrush>#9215F3</SolidColorBrush>
                <SolidColorBrush>#F4890B</SolidColorBrush>
                <SolidColorBrush>#D21243</SolidColorBrush>
                <SolidColorBrush>#E2227E</SolidColorBrush>
                <SolidColorBrush>#28A745</SolidColorBrush>
            </treemap:TreeMapPaletteBrushSettings.Brushes>
        </treemap:TreeMapPaletteBrushSettings>
    </treemap:SfTreeMap.LeafItemBrushSettings>
</treemap:SfTreeMap>
```

### C# Implementation

```csharp
treeMap.LeafItemBrushSettings = new TreeMapPaletteBrushSettings
{
    Brushes = new List<Brush>
    {
        new SolidColorBrush(Color.FromArgb("#116DF9")),
        new SolidColorBrush(Color.FromArgb("#9215F3")),
        new SolidColorBrush(Color.FromArgb("#F4890B")),
        new SolidColorBrush(Color.FromArgb("#D21243")),
        new SolidColorBrush(Color.FromArgb("#E2227E")),
        new SolidColorBrush(Color.FromArgb("#28A745"))
    }
};
```

### Color Cycling

If there are more items than colors, the palette cycles:
- Item 1: Color 1
- Item 2: Color 2
- ...
- Item 6: Color 6
- Item 7: Color 1 (cycles back)
- Item 8: Color 2
- And so on...

### Pre-defined Color Palettes

```csharp
// Material Design palette
var materialColors = new List<Brush>
{
    new SolidColorBrush(Color.FromArgb("#F44336")),  // Red
    new SolidColorBrush(Color.FromArgb("#E91E63")),  // Pink
    new SolidColorBrush(Color.FromArgb("#9C27B0")),  // Purple
    new SolidColorBrush(Color.FromArgb("#3F51B5")),  // Indigo
    new SolidColorBrush(Color.FromArgb("#2196F3")),  // Blue
    new SolidColorBrush(Color.FromArgb("#009688")),  // Teal
    new SolidColorBrush(Color.FromArgb("#4CAF50")),  // Green
    new SolidColorBrush(Color.FromArgb("#FF9800"))   // Orange
};

treeMap.LeafItemBrushSettings = new TreeMapPaletteBrushSettings { Brushes = materialColors };
```

## Choosing the Right Brush Setting

### Decision Guide

| Scenario | Recommended Brush Setting | Why |
|----------|--------------------------|-----|
| Show all items same color | UniformBrushSettings | Simplest, no data meaning |
| Categorize into 3-5 levels | RangeBrushSettings | Clear discrete categories |
| Show smooth intensity gradient | DesaturationBrushSettings | Continuous scale |
| Distinguish individual items | PaletteBrushSettings | Visual variety |
| Display with legend | RangeBrushSettings | Legend support |
| Performance critical | UniformBrushSettings | Fastest rendering |
| Heat map visualization | RangeBrushSettings or Desaturation | Data-driven colors |

### Combined Scenarios

**Scenario 1: Sales Performance**
Use RangeBrushSettings with ranges for Poor/Fair/Good/Excellent performance levels.

**Scenario 2: File System**
Use PaletteBrushSettings to distinguish file types with different colors.

**Scenario 3: Population Density**
Use DesaturationBrushSettings for smooth gradient from low to high density.

**Scenario 4: Simple Mockup**
Use UniformBrushSettings during design/prototyping.

## Combining with Legends

### Range Brush Settings with Legend

```xml
<treemap:SfTreeMap DataSource="{Binding Data}"
                   PrimaryValuePath="Value"
                   RangeColorValuePath="Value">
    <treemap:SfTreeMap.LegendSettings>
        <treemap:TreeMapLegendSettings ShowLegend="True" />
    </treemap:SfTreeMap.LegendSettings>
    
    <treemap:SfTreeMap.LeafItemBrushSettings>
        <treemap:TreeMapRangeBrushSettings>
            <treemap:TreeMapRangeBrushSettings.RangeBrushes>
                <treemap:TreeMapRangeBrush LegendLabel="High"
                                           From="75"
                                           To="100" 
                                           Brush="#4CAF50" />
                <treemap:TreeMapRangeBrush LegendLabel="Medium"
                                           From="50"
                                           To="75" 
                                           Brush="#FF9800" />
                <treemap:TreeMapRangeBrush LegendLabel="Low"
                                           From="0" 
                                           To="50"  
                                           Brush="#F44336" />
            </treemap:TreeMapRangeBrushSettings.RangeBrushes>
        </treemap:TreeMapRangeBrushSettings>
    </treemap:SfTreeMap.LeafItemBrushSettings>
</treemap:SfTreeMap>
```

Legend items will display with `LegendLabel` text and corresponding brush colors.

## Practical Examples

### Example 1: Budget Visualization with Ranges

```csharp
public class BudgetItem
{
    public string Category { get; set; }
    public decimal Amount { get; set; }
}

// In TreeMap setup
treeMap.RangeColorValuePath = "Amount";
treeMap.LeafItemBrushSettings = new TreeMapRangeBrushSettings
{
    RangeBrushes = new List<TreeMapRangeBrush>
    {
        new TreeMapRangeBrush { LegendLabel = "Over Budget", From = 100000, To = double.MaxValue, Brush = Brush.Red },
        new TreeMapRangeBrush { LegendLabel = "Near Budget", From = 75000, To = 100000, Brush = Brush.Orange },
        new TreeMapRangeBrush { LegendLabel = "Under Budget", From = 0, To = 75000, Brush = Brush.Green }
    }
};
```

### Example 2: Temperature Heat Map with Desaturation

```csharp
public class CityTemperature
{
    public string City { get; set; }
    public double Temperature { get; set; }
}

// In TreeMap setup
treeMap.LeafItemBrushSettings = new TreeMapDesaturationBrushSettings 
{ 
    Brush = new SolidColorBrush(Color.FromArgb("#FF5722")),  // Red-orange
    From = 1.0,  // Hot cities: vibrant red
    To = 0.2     // Cool cities: pale red
};
```

### Example 3: File Types with Palette

```csharp
public class FileInfo
{
    public string FileName { get; set; }
    public string FileType { get; set; }
    public long Size { get; set; }
}

// In TreeMap setup (colors cycle per file type)
treeMap.LeafItemBrushSettings = new TreeMapPaletteBrushSettings
{
    Brushes = new List<Brush>
    {
        new SolidColorBrush(Color.FromArgb("#2196F3")),  // Blue for documents
        new SolidColorBrush(Color.FromArgb("#4CAF50")),  // Green for spreadsheets
        new SolidColorBrush(Color.FromArgb("#FF9800")),  // Orange for images
        new SolidColorBrush(Color.FromArgb("#9C27B0"))   // Purple for videos
    }
};
```

## Troubleshooting

### Issue 1: RangeBrushSettings Not Working

**Symptoms**: All items show default color despite setting ranges

**Solutions**:
1. Verify `RangeColorValuePath` is set and matches a numeric property
2. Ensure range values (`From`/`To`) cover your data range
3. Check that data values fall within defined ranges
4. Confirm brushes are properly defined

```csharp
// Debug: Check data values
foreach (var item in dataSource)
{
    Debug.WriteLine($"Value: {item.YourValueProperty}");
}
```

### Issue 2: Desaturation Not Visible

**Symptoms**: All items appear same color

**Solutions**:
1. Ensure `From` and `To` values are different (not both 1.0 or both 0.0)
2. Try a more vibrant base color
3. Increase the difference between From and To (e.g., From=1.0, To=0.1)

```csharp
// Good contrast
From = 1.0, To = 0.2

// Poor contrast (hard to see difference)
From = 1.0, To = 0.9
```

### Issue 3: Palette Colors Not Showing

**Symptoms**: Items don't use palette colors

**Solutions**:
1. Verify `Brushes` collection is not empty
2. Check for XAML syntax errors in SolidColorBrush definitions
3. Ensure hex colors are valid (#RRGGBB format)

### Issue 4: Legend Not Showing Range Colors

**Symptoms**: Legend appears but without colors

**Solutions**:
1. Ensure `ShowLegend="True"` in LegendSettings
2. Verify `RangeColorValuePath` is set
3. Confirm `LegendLabel` is provided for each TreeMapRangeBrush
4. Check that RangeBrushSettings (not other brush types) is used

### Issue 5: Items Outside Range Get No Color

**Solution**: Add catch-all ranges at boundaries:

```csharp
// Add minimum and maximum ranges
new TreeMapRangeBrush { From = double.MinValue, To = 0, Brush = ... },
new TreeMapRangeBrush { From = 0, To = 1000, Brush = ... },
new TreeMapRangeBrush { From = 1000, To = double.MaxValue, Brush = ... }
```

## Related Topics

- [Legend Configuration](legend.md) - Displaying legends for range colors
- [Leaf Item Customization](leaf-item-customization.md) - Additional leaf item styling
- [Getting Started](getting-started.md) - Basic TreeMap setup

## Summary

- **UniformBrushSettings**: Single color for all items
- **RangeBrushSettings**: Discrete color ranges with legend support
- **DesaturationBrushSettings**: Smooth saturation gradient
- **PaletteBrushSettings**: Multiple colors cycling through items
- Choose based on data visualization needs and whether discrete categories or continuous gradients are appropriate
- RangeBrushSettings requires `RangeColorValuePath` to be set
- Only RangeBrushSettings supports legends with color indicators
