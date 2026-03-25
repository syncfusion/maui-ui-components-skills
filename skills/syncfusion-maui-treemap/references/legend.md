# Legend in .NET MAUI TreeMap

The legend provides a visual guide that explains the color coding in the TreeMap, helping users understand what different colors represent. It's particularly useful with `RangeBrushSettings` to show data categories.

## Overview

The legend displays color indicators with labels, making it easier to interpret TreeMap visualizations. It works with `RangeBrushSettings` to show range categories.

**Key Properties:**
- `ShowLegend`: Enable/disable legend
- `IconType`: Shape of legend icons
- `IconSize`: Size of legend icons
- `Placement`: Position (Top, Bottom, Left, Right)
- `TextStyle`: Label text formatting

**Basic Setup:**

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
                                           From="75" To="100" 
                                           Brush="#4CAF50" />
                <treemap:TreeMapRangeBrush LegendLabel="Low"
                                           From="0" To="75" 
                                           Brush="#F44336" />
            </treemap:TreeMapRangeBrushSettings.RangeBrushes>
        </treemap:TreeMapRangeBrushSettings>
    </treemap:SfTreeMap.LeafItemBrushSettings>
</treemap:SfTreeMap>
```

## Enabling the Legend

### ShowLegend Property

Controls whether the legend is visible.

**XAML:**
```xml
<treemap:SfTreeMap DataSource="{Binding PopulationDetails}"
                   PrimaryValuePath="Population"
                   RangeColorValuePath="Population">
    <treemap:SfTreeMap.LegendSettings>
        <treemap:TreeMapLegendSettings ShowLegend="True" />
    </treemap:SfTreeMap.LegendSettings>
    
    <treemap:SfTreeMap.LeafItemBrushSettings>
        <treemap:TreeMapRangeBrushSettings>
            <treemap:TreeMapRangeBrushSettings.RangeBrushes>
                <treemap:TreeMapRangeBrush LegendLabel="50M - 1B"
                                           From="50000000" To="1000000000" 
                                           Brush="#F0A868" />
                <treemap:TreeMapRangeBrush LegendLabel="10M - 50M"
                                           From="10000000" To="50000000" 
                                           Brush="#F3BC8B" />
                <treemap:TreeMapRangeBrush LegendLabel="0.1M - 10M"
                                           From="100000" To="10000000" 
                                           Brush="#F8D7B9" />
            </treemap:TreeMapRangeBrushSettings.RangeBrushes>
        </treemap:TreeMapRangeBrushSettings>
    </treemap:SfTreeMap.LeafItemBrushSettings>
</treemap:SfTreeMap>
```

**C# Implementation:**
```csharp
treeMap.LegendSettings = new TreeMapLegendSettings
{
    ShowLegend = true
};

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

**Result:** Legend displays three entries with colored icons and range labels.

### Hide Legend

```xml
<treemap:TreeMapLegendSettings ShowLegend="False" />
```

## Legend Placement

Position the legend relative to the TreeMap using the `Placement` property.

**TreeMapLegendPlacement Enum:**
- `Top`: Above TreeMap
- `Bottom`: Below TreeMap (default)
- `Left`: Left of TreeMap
- `Right`: Right of TreeMap

### Top Placement

```xml
<treemap:SfTreeMap DataSource="{Binding Data}"
                   PrimaryValuePath="Value"
                   RangeColorValuePath="Value">
    <treemap:SfTreeMap.LegendSettings>
        <treemap:TreeMapLegendSettings ShowLegend="True"
                                       Placement="Top" />
    </treemap:SfTreeMap.LegendSettings>
    
    <treemap:SfTreeMap.LeafItemBrushSettings>
        <treemap:TreeMapRangeBrushSettings>
            <treemap:TreeMapRangeBrushSettings.RangeBrushes>
                <treemap:TreeMapRangeBrush LegendLabel="Category A"
                                           From="0" To="50" 
                                           Brush="#FF5722" />
                <treemap:TreeMapRangeBrush LegendLabel="Category B"
                                           From="50" To="100" 
                                           Brush="#4CAF50" />
            </treemap:TreeMapRangeBrushSettings.RangeBrushes>
        </treemap:TreeMapRangeBrushSettings>
    </treemap:SfTreeMap.LeafItemBrushSettings>
</treemap:SfTreeMap>
```

### Bottom Placement (Default)

```xml
<treemap:TreeMapLegendSettings ShowLegend="True"
                               Placement="Bottom" />
```

### Left Placement

```xml
<treemap:TreeMapLegendSettings ShowLegend="True"
                               Placement="Left" />
```

**Best for:** Horizontal space available, vertical labels preferred

### Right Placement

```xml
<treemap:TreeMapLegendSettings ShowLegend="True"
                               Placement="Right" />
```

**Best for:** Additional panel-style layout

### C# Implementation

```csharp
treeMap.LegendSettings = new TreeMapLegendSettings
{
    ShowLegend = true,
    Placement = TreeMapLegendPlacement.Top
};
```

## Legend Icon Customization

### IconType Property

Specifies the shape of legend icons.

**TreeMapIconType Enum:**
- `Circle`: Round icons
- `Rectangle`: Square/rectangular icons
- `Diamond`: Diamond-shaped icons
- `Triangle`: Triangular icons

### Circle Icons

```xml
<treemap:SfTreeMap DataSource="{Binding Data}"
                   PrimaryValuePath="Value"
                   RangeColorValuePath="Value">
    <treemap:SfTreeMap.LegendSettings>
        <treemap:TreeMapLegendSettings ShowLegend="True"
                                       IconType="Circle"
                                       IconSize="16, 16" />
    </treemap:SfTreeMap.LegendSettings>
    
    <treemap:SfTreeMap.LeafItemBrushSettings>
        <treemap:TreeMapRangeBrushSettings>
            <treemap:TreeMapRangeBrushSettings.RangeBrushes>
                <treemap:TreeMapRangeBrush LegendLabel="High"
                                           From="75" To="100" 
                                           Brush="#4CAF50" />
                <treemap:TreeMapRangeBrush LegendLabel="Medium"
                                           From="50" To="75" 
                                           Brush="#FF9800" />
                <treemap:TreeMapRangeBrush LegendLabel="Low"
                                           From="0" To="50" 
                                           Brush="#F44336" />
            </treemap:TreeMapRangeBrushSettings.RangeBrushes>
        </treemap:TreeMapRangeBrushSettings>
    </treemap:SfTreeMap.LeafItemBrushSettings>
</treemap:SfTreeMap>
```

### Rectangle Icons (Default)

```xml
<treemap:TreeMapLegendSettings ShowLegend="True"
                               IconType="Rectangle"
                               IconSize="20, 12" />
```

**Best for:** Standard legend appearance, matches TreeMap leaf items

### Diamond Icons

```xml
<treemap:TreeMapLegendSettings ShowLegend="True"
                               IconType="Diamond"
                               IconSize="16, 16" />
```

**Best for:** Distinctive, decorative appearance

### Triangle Icons

```xml
<treemap:TreeMapLegendSettings ShowLegend="True"
                               IconType="Triangle"
                               IconSize="16, 16" />
```

**Best for:** Directional or hierarchical data emphasis

### C# Implementation

```csharp
treeMap.LegendSettings = new TreeMapLegendSettings
{
    ShowLegend = true,
    IconType = TreeMapIconType.Circle,
    IconSize = new Size(16, 16)
};
```

### IconSize Property

Controls the width and height of legend icons.

**Syntax:**
```xml
IconSize="width, height"
```

**Examples:**
```xml
<!-- Small icons -->
<treemap:TreeMapLegendSettings IconSize="12, 12" />

<!-- Medium icons (default) -->
<treemap:TreeMapLegendSettings IconSize="16, 16" />

<!-- Large icons -->
<treemap:TreeMapLegendSettings IconSize="24, 24" />

<!-- Rectangular icons (wider than tall) -->
<treemap:TreeMapLegendSettings IconType="Rectangle"
                               IconSize="20, 12" />
```

**C# Implementation:**
```csharp
treeMap.LegendSettings = new TreeMapLegendSettings
{
    ShowLegend = true,
    IconType = TreeMapIconType.Rectangle,
    IconSize = new Size(20, 12)
};
```

## Legend Text Styling

### TextStyle Property

Customize legend label appearance.

**Available Properties:**
- `TextColor`: Label text color
- `FontSize`: Text size
- `FontFamily`: Font typeface
- `FontAttributes`: Bold, Italic, or None

### Basic Text Styling

**XAML:**
```xml
<treemap:SfTreeMap DataSource="{Binding Data}"
                   PrimaryValuePath="Value"
                   RangeColorValuePath="Value">
    <treemap:SfTreeMap.LegendSettings>
        <treemap:TreeMapLegendSettings ShowLegend="True"
                                       IconType="Circle"
                                       IconSize="14, 14">
            <treemap:TreeMapLegendSettings.TextStyle>
                <treemap:TreeMapLabelStyle TextColor="Black"
                                           FontSize="13"
                                           FontAttributes="Bold" />
            </treemap:TreeMapLegendSettings.TextStyle>
        </treemap:TreeMapLegendSettings>
    </treemap:SfTreeMap.LegendSettings>
    
    <treemap:SfTreeMap.LeafItemBrushSettings>
        <treemap:TreeMapRangeBrushSettings>
            <treemap:TreeMapRangeBrushSettings.RangeBrushes>
                <treemap:TreeMapRangeBrush LegendLabel="Excellent"
                                           From="90" To="100" 
                                           Brush="#4CAF50" />
                <treemap:TreeMapRangeBrush LegendLabel="Good"
                                           From="70" To="90" 
                                           Brush="#8BC34A" />
                <treemap:TreeMapRangeBrush LegendLabel="Fair"
                                           From="50" To="70" 
                                           Brush="#FFC107" />
                <treemap:TreeMapRangeBrush LegendLabel="Poor"
                                           From="0" To="50" 
                                           Brush="#F44336" />
            </treemap:TreeMapRangeBrushSettings.RangeBrushes>
        </treemap:TreeMapRangeBrushSettings>
    </treemap:SfTreeMap.LeafItemBrushSettings>
</treemap:SfTreeMap>
```

**C# Implementation:**
```csharp
treeMap.LegendSettings = new TreeMapLegendSettings
{
    ShowLegend = true,
    IconType = TreeMapIconType.Circle,
    IconSize = new Size(14, 14),
    TextStyle = new TreeMapLabelStyle
    {
        TextColor = Colors.Black,
        FontSize = 13,
        FontAttributes = FontAttributes.Bold
    }
};
```

### Font Size Examples

```csharp
// Small text
TextStyle = new TreeMapLabelStyle { FontSize = 10 }

// Normal text (recommended)
TextStyle = new TreeMapLabelStyle { FontSize = 12 }

// Large text
TextStyle = new TreeMapLabelStyle { FontSize = 14 }

// Extra large text
TextStyle = new TreeMapLabelStyle { FontSize = 16 }
```

### Text Color

```xml
<!-- Black text (light backgrounds) -->
<treemap:TreeMapLabelStyle TextColor="Black" FontSize="12" />

<!-- Dark gray text -->
<treemap:TreeMapLabelStyle TextColor="#424242" FontSize="12" />

<!-- White text (dark backgrounds) -->
<treemap:TreeMapLabelStyle TextColor="White" FontSize="12" />
```

### Font Attributes

```xml
<!-- Bold -->
<treemap:TreeMapLabelStyle FontAttributes="Bold" />

<!-- Italic -->
<treemap:TreeMapLabelStyle FontAttributes="Italic" />

<!-- Bold and Italic -->
<treemap:TreeMapLabelStyle FontAttributes="Bold, Italic" />
```

## LegendLabel Best Practices

### Descriptive Labels

Use clear, meaningful labels that describe the data range or category.

**Good:**
```csharp
LegendLabel = "High Revenue (>$1M)"
LegendLabel = "Excellent Performance (90-100%)"
LegendLabel = "Major Cities (>500K)"
```

**Avoid:**
```csharp
LegendLabel = "Range 1"  // Too generic
LegendLabel = "75-100"   // Missing context
```

### Consistent Formatting

Use consistent formatting across all legend labels.

**Good (consistent):**
```csharp
LegendLabel = "50M - 1B"
LegendLabel = "10M - 50M"
LegendLabel = "0.1M - 10M"
```

**Avoid (inconsistent):**
```csharp
LegendLabel = "50,000,000 to 1,000,000,000"
LegendLabel = "10M - 50M"
LegendLabel = "100k-10M"
```

### Order Matters

Legend items display in the order defined in `RangeBrushes`. Order them logically:

**High to Low (recommended for most cases):**
```csharp
RangeBrushes = new List<TreeMapRangeBrush>
{
    new TreeMapRangeBrush { LegendLabel = "High", From = 75, To = 100, ... },
    new TreeMapRangeBrush { LegendLabel = "Medium", From = 50, To = 75, ... },
    new TreeMapRangeBrush { LegendLabel = "Low", From = 0, To = 50, ... }
}
```

## Practical Examples

### Example 1: Sales Performance with Top Legend

```xml
<treemap:SfTreeMap DataSource="{Binding SalesData}"
                   PrimaryValuePath="Revenue"
                   RangeColorValuePath="Revenue">
    <treemap:SfTreeMap.LegendSettings>
        <treemap:TreeMapLegendSettings ShowLegend="True"
                                       Placement="Top"
                                       IconType="Circle"
                                       IconSize="16, 16">
            <treemap:TreeMapLegendSettings.TextStyle>
                <treemap:TreeMapLabelStyle TextColor="#212121"
                                           FontSize="13"
                                           FontAttributes="Bold" />
            </treemap:TreeMapLegendSettings.TextStyle>
        </treemap:TreeMapLegendSettings>
    </treemap:SfTreeMap.LegendSettings>
    
    <treemap:SfTreeMap.LeafItemBrushSettings>
        <treemap:TreeMapRangeBrushSettings>
            <treemap:TreeMapRangeBrushSettings.RangeBrushes>
                <treemap:TreeMapRangeBrush LegendLabel="Exceeds Target (>$1M)"
                                           From="1000000" To="double.MaxValue" 
                                           Brush="#4CAF50" />
                <treemap:TreeMapRangeBrush LegendLabel="Meets Target ($500K-$1M)"
                                           From="500000" To="1000000" 
                                           Brush="#8BC34A" />
                <treemap:TreeMapRangeBrush LegendLabel="Below Target (<$500K)"
                                           From="0" To="500000" 
                                           Brush="#FF5722" />
            </treemap:TreeMapRangeBrushSettings.RangeBrushes>
        </treemap:TreeMapRangeBrushSettings>
    </treemap:SfTreeMap.LeafItemBrushSettings>
</treemap:SfTreeMap>
```

### Example 2: Temperature Map with Left Legend

```csharp
public class CityTemperature
{
    public string City { get; set; }
    public double Temperature { get; set; }
}

// TreeMap setup
treeMap.DataSource = temperatureData;
treeMap.PrimaryValuePath = "Temperature";
treeMap.RangeColorValuePath = "Temperature";

treeMap.LegendSettings = new TreeMapLegendSettings
{
    ShowLegend = true,
    Placement = TreeMapLegendPlacement.Left,
    IconType = TreeMapIconType.Rectangle,
    IconSize = new Size(20, 12),
    TextStyle = new TreeMapLabelStyle
    {
        TextColor = Colors.Black,
        FontSize = 12
    }
};

treeMap.LeafItemBrushSettings = new TreeMapRangeBrushSettings
{
    RangeBrushes = new List<TreeMapRangeBrush>
    {
        new TreeMapRangeBrush 
        { 
            LegendLabel = "Very Hot (>95°F)", 
            From = 95, 
            To = 120, 
            Brush = new SolidColorBrush(Color.FromArgb("#D32F2F")) 
        },
        new TreeMapRangeBrush 
        { 
            LegendLabel = "Hot (85-95°F)", 
            From = 85, 
            To = 95, 
            Brush = new SolidColorBrush(Color.FromArgb("#FF5722")) 
        },
        new TreeMapRangeBrush 
        { 
            LegendLabel = "Warm (75-85°F)", 
            From = 75, 
            To = 85, 
            Brush = new SolidColorBrush(Color.FromArgb("#FF9800")) 
        },
        new TreeMapRangeBrush 
        { 
            LegendLabel = "Mild (65-75°F)", 
            From = 65, 
            To = 75, 
            Brush = new SolidColorBrush(Color.FromArgb("#FFC107")) 
        },
        new TreeMapRangeBrush 
        { 
            LegendLabel = "Cool (<65°F)", 
            From = 0, 
            To = 65, 
            Brush = new SolidColorBrush(Color.FromArgb("#2196F3")) 
        }
    }
};
```

### Example 3: Portfolio with Diamond Icons

```xml
<treemap:SfTreeMap DataSource="{Binding Portfolio}"
                   PrimaryValuePath="Value"
                   RangeColorValuePath="GrowthPercent">
    <treemap:SfTreeMap.LegendSettings>
        <treemap:TreeMapLegendSettings ShowLegend="True"
                                       Placement="Bottom"
                                       IconType="Diamond"
                                       IconSize="18, 18">
            <treemap:TreeMapLegendSettings.TextStyle>
                <treemap:TreeMapLabelStyle TextColor="#424242"
                                           FontSize="12"
                                           FontAttributes="Bold" />
            </treemap:TreeMapLegendSettings.TextStyle>
        </treemap:TreeMapLegendSettings>
    </treemap:SfTreeMap.LegendSettings>
    
    <treemap:SfTreeMap.LeafItemBrushSettings>
        <treemap:TreeMapRangeBrushSettings>
            <treemap:TreeMapRangeBrushSettings.RangeBrushes>
                <treemap:TreeMapRangeBrush LegendLabel="Strong Growth (>15%)"
                                           From="15" To="100" 
                                           Brush="#4CAF50" />
                <treemap:TreeMapRangeBrush LegendLabel="Moderate Growth (5-15%)"
                                           From="5" To="15" 
                                           Brush="#8BC34A" />
                <treemap:TreeMapRangeBrush LegendLabel="Flat (-5% to 5%)"
                                           From="-5" To="5" 
                                           Brush="#FFC107" />
                <treemap:TreeMapRangeBrush LegendLabel="Decline (<-5%)"
                                           From="-100" To="-5" 
                                           Brush="#F44336" />
            </treemap:TreeMapRangeBrushSettings.RangeBrushes>
        </treemap:TreeMapRangeBrushSettings>
    </treemap:SfTreeMap.LeafItemBrushSettings>
</treemap:SfTreeMap>
```

## Troubleshooting

### Issue 1: Legend Not Showing

**Symptoms:** Legend doesn't appear despite `ShowLegend="True"`

**Solutions:**
1. Ensure `RangeBrushSettings` is used (not Uniform, Desaturation, or Palette)
2. Verify `RangeColorValuePath` is set on the TreeMap
3. Check that `LegendLabel` is defined for each `TreeMapRangeBrush`
4. Confirm `ShowLegend="True"` in LegendSettings

```xml
<!-- All required elements -->
<treemap:SfTreeMap RangeColorValuePath="Value">  <!-- Required -->
    <treemap:SfTreeMap.LegendSettings>
        <treemap:TreeMapLegendSettings ShowLegend="True" />  <!-- Required -->
    </treemap:SfTreeMap.LegendSettings>
    
    <treemap:SfTreeMap.LeafItemBrushSettings>
        <treemap:TreeMapRangeBrushSettings>  <!-- Required -->
            <treemap:TreeMapRangeBrushSettings.RangeBrushes>
                <treemap:TreeMapRangeBrush LegendLabel="Label"  <!-- Required -->
                                           From="0" To="50" 
                                           Brush="Green" />
            </treemap:TreeMapRangeBrushSettings.RangeBrushes>
        </treemap:TreeMapRangeBrushSettings>
    </treemap:SfTreeMap.LeafItemBrushSettings>
</treemap:SfTreeMap>
```

### Issue 2: Legend Labels Missing

**Symptoms:** Legend shows colored icons but no text labels

**Solutions:**
1. Verify `LegendLabel` property is set for each range
2. Check `TextStyle.FontSize` is not 0
3. Ensure `TextStyle.TextColor` contrasts with background
4. Confirm label strings are not empty or whitespace

```csharp
// Correct: Label defined
new TreeMapRangeBrush { LegendLabel = "High", From = 75, To = 100, Brush = ... }

// Wrong: Missing label
new TreeMapRangeBrush { From = 75, To = 100, Brush = ... }
```

### Issue 3: Legend Icons Too Small/Large

**Symptoms:** Icons difficult to see or too prominent

**Solutions:**
1. Adjust `IconSize` property
2. Typical sizes: 12x12 (small), 16x16 (medium), 20x20 (large)
3. Match icon size to font size for balance

```xml
<!-- Balanced appearance -->
<treemap:TreeMapLegendSettings IconSize="16, 16">
    <treemap:TreeMapLegendSettings.TextStyle>
        <treemap:TreeMapLabelStyle FontSize="13" />
    </treemap:TreeMapLegendSettings.TextStyle>
</treemap:TreeMapLegendSettings>
```

### Issue 4: Legend Overlaps TreeMap

**Symptoms:** Legend covers part of the TreeMap

**Solutions:**
1. Try different `Placement` (Top/Bottom/Left/Right)
2. Reduce legend text size
3. Ensure parent container has adequate space

### Issue 5: Legend Colors Don't Match TreeMap

**Symptoms:** Legend shows different colors than leaf items

**Solutions:**
1. Verify `RangeColorValuePath` matches the property used for coloring
2. Check data values fall within defined ranges
3. Ensure brush definitions are identical in legend and TreeMap

```csharp
// Consistent brush definition
var greenBrush = new SolidColorBrush(Color.FromArgb("#4CAF50"));

new TreeMapRangeBrush 
{ 
    LegendLabel = "High", 
    From = 75, 
    To = 100, 
    Brush = greenBrush  // Use same brush instance
}
```

## Related Topics

- [Brush Settings](brush-settings.md) - RangeBrushSettings for color ranges
- [Getting Started](getting-started.md) - Basic TreeMap setup
- [Leaf Item Customization](leaf-item-customization.md) - Styling leaf items

## Summary

- Legend works with `RangeBrushSettings` only
- Enable with `ShowLegend="True"` in `LegendSettings`
- Each `TreeMapRangeBrush` must have `LegendLabel` defined
- Position legend with `Placement`: Top, Bottom, Left, or Right
- Customize icons with `IconType` (Circle, Rectangle, Diamond, Triangle)
- Set icon size with `IconSize` property (width, height)
- Style legend text with `TextStyle` (color, size, font, attributes)
- Use descriptive, consistent labels for clarity
- Ensure `RangeColorValuePath` is set on the TreeMap
- Order `RangeBrushes` logically (typically high to low)
