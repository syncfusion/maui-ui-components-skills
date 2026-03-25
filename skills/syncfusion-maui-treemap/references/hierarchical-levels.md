# Hierarchical Levels in .NET MAUI TreeMap

Hierarchical levels enable multi-level data categorization in TreeMap, allowing you to group and visualize nested hierarchical structures such as Continent → Country → City or Department → Team → Employee.

## Table of Contents
- [Overview](#overview)
- [Understanding Levels](#understanding-levels)
- [GroupPath Property](#grouppath-property)
- [Adding Multiple Levels](#adding-multiple-levels)
- [Level Appearance Customization](#level-appearance-customization)
- [Group Item Brush Settings](#group-item-brush-settings)
- [Practical Examples](#practical-examples)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

The `Levels` property in TreeMap allows you to define hierarchical groupings using the `GroupPath` property. Each level represents a categorization layer in your data structure, creating nested visual groups with headers.

**Key Concepts:**
- **Level**: A hierarchical layer defined by `TreeMapLevel`
- **GroupPath**: Property name used to group items at that level
- **Group Header**: Visual header displayed for each group
- **Leaf Items**: The final data items (no children)

## Understanding Levels

### What are Levels?

Levels define how your flat data collection is grouped hierarchically. Without levels, TreeMap displays all items as peers. With levels, items are grouped by common property values.

**Example Data Structure:**
```csharp
public class PopulationData
{
    public string Continent { get; set; }  // Level 1 group
    public string Country { get; set; }    // Leaf item label
    public int Population { get; set; }    // Size determinant
}
```

**Without Levels:**
All countries displayed as individual rectangles side-by-side.

**With One Level (Continent):**
Countries grouped under continent headers, with continent groups arranged first, then countries within each continent.

### How Levels Work

1. TreeMap groups data by the first level's `GroupPath`
2. Creates a header for each unique value in that property
3. Within each group, displays leaf items or applies next level
4. Process repeats for each subsequent level
5. Final items are displayed as leaf rectangles

## GroupPath Property

The `GroupPath` property specifies which data property to use for grouping at a specific level.

### Basic GroupPath Example

```xml
<treemap:SfTreeMap DataSource="{Binding PopulationDetails}"
                   PrimaryValuePath="Population">
    <treemap:SfTreeMap.Levels>
        <treemap:TreeMapLevel GroupPath="Continent" />
    </treemap:SfTreeMap.Levels>
    
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Country" />
    </treemap:SfTreeMap.LeafItemSettings>
    
    <treemap:SfTreeMap.LeafItemBrushSettings>
        <treemap:TreeMapUniformBrushSettings Brush="Orange" />
    </treemap:SfTreeMap.LeafItemBrushSettings>
</treemap:SfTreeMap>
```

### C# Approach

```csharp
SfTreeMap treeMap = new SfTreeMap
{
    DataSource = viewModel.PopulationDetails,
    PrimaryValuePath = "Population",
    LeafItemSettings = new TreeMapLeafItemSettings { LabelPath = "Country" },
    LeafItemBrushSettings = new TreeMapUniformBrushSettings { Brush = Brush.Orange }
};

treeMap.Levels.Add(new TreeMapLevel { GroupPath = "Continent" });

this.Content = treeMap;
```

### Data Model Requirements

Your data model must have the property specified in `GroupPath`:

```csharp
public class PopulationData
{
    public string Country { get; set; }
    public string Continent { get; set; }  // Must exist for GroupPath="Continent"
    public int Population { get; set; }
}
```

**Important**: GroupPath is case-sensitive and must exactly match the property name.

## Adding Multiple Levels

You can add multiple levels to create deeper hierarchies.

### Two-Level Example: Continent → Country

```csharp
public class DetailedPopulationData
{
    public string Continent { get; set; }    // Level 1
    public string Subregion { get; set; }    // Level 2
    public string Country { get; set; }      // Leaf label
    public int Population { get; set; }
}
```

```xml
<treemap:SfTreeMap DataSource="{Binding DetailedData}"
                   PrimaryValuePath="Population">
    <treemap:SfTreeMap.Levels>
        <treemap:TreeMapLevel GroupPath="Continent" />
        <treemap:TreeMapLevel GroupPath="Subregion" />
    </treemap:SfTreeMap.Levels>
    
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Country" />
    </treemap:SfTreeMap.LeafItemSettings>
</treemap:SfTreeMap>
```

### Three-Level Example: Organization Structure

```csharp
public class EmployeeData
{
    public string Department { get; set; }   // Level 1
    public string Team { get; set; }         // Level 2
    public string Role { get; set; }         // Level 3
    public string EmployeeName { get; set; } // Leaf label
    public int Salary { get; set; }          // Size
}
```

```csharp
SfTreeMap treeMap = new SfTreeMap
{
    DataSource = viewModel.EmployeeData,
    PrimaryValuePath = "Salary",
    LeafItemSettings = new TreeMapLeafItemSettings { LabelPath = "EmployeeName" }
};

treeMap.Levels.Add(new TreeMapLevel { GroupPath = "Department" });
treeMap.Levels.Add(new TreeMapLevel { GroupPath = "Team" });
treeMap.Levels.Add(new TreeMapLevel { GroupPath = "Role" });

this.Content = treeMap;
```

**Result**: Department headers → Team headers within departments → Role headers within teams → Employee rectangles within roles.

### Unlimited Levels

TreeMap supports n number of levels. However, for practical readability:
- **1-2 levels**: Recommended for most use cases
- **3-4 levels**: Acceptable for complex hierarchies
- **5+ levels**: May become difficult to visualize and navigate

## Level Appearance Customization

Each level can be customized independently with various properties.

### Available Customization Properties

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `GroupPath` | string | Property name for grouping | Required |
| `Spacing` | double | Space between group items | 1 |
| `HeaderHeight` | double | Height of group header | 24 |
| `Background` | Brush | Header background color | null |
| `Stroke` | Color | Header border color | LightGray |
| `StrokeWidth` | double | Header border thickness | 1 |
| `TextStyle` | TreeMapTextStyle | Header text styling | Default |

### Spacing Customization

Control the space between group header items:

```xml
<treemap:SfTreeMap.Levels>
    <treemap:TreeMapLevel GroupPath="Continent"
                          Spacing="3" />
</treemap:SfTreeMap.Levels>
```

```csharp
treeMap.Levels.Add(new TreeMapLevel 
{ 
    GroupPath = "Continent", 
    Spacing = 3 
});
```

**Effect**: Increases the gap between continent groups from 1 pixel (default) to 3 pixels.

### Header Height Customization

Adjust the height of group headers:

```xml
<treemap:SfTreeMap.Levels>
    <treemap:TreeMapLevel GroupPath="Continent"
                          HeaderHeight="30" />
</treemap:SfTreeMap.Levels>
```

```csharp
treeMap.Levels.Add(new TreeMapLevel 
{ 
    GroupPath = "Continent", 
    HeaderHeight = 30 
});
```

**Use Cases:**
- Increase for longer text or multiple lines
- Decrease to save space
- Differentiate level importance

### Background Customization

Set custom background colors for group headers:

```xml
<treemap:SfTreeMap.Levels>
    <treemap:TreeMapLevel GroupPath="Continent"
                          Background="LightGreen" />
</treemap:SfTreeMap.Levels>
```

```csharp
treeMap.Levels.Add(new TreeMapLevel 
{ 
    GroupPath = "Continent", 
    Background = Brush.LightGreen 
});
```

### Using Hex Colors

```xml
<treemap:TreeMapLevel GroupPath="Continent"
                      Background="#E8F5E9" />
```

```csharp
treeMap.Levels.Add(new TreeMapLevel 
{ 
    GroupPath = "Continent", 
    Background = new SolidColorBrush(Color.FromArgb("#E8F5E9")) 
});
```

### Stroke Customization

Customize header border colors:

```xml
<treemap:SfTreeMap.Levels>
    <treemap:TreeMapLevel GroupPath="Continent"
                          Stroke="Red"
                          StrokeWidth="2" />
</treemap:SfTreeMap.Levels>
```

```csharp
treeMap.Levels.Add(new TreeMapLevel 
{ 
    GroupPath = "Continent", 
    Stroke = Colors.Red,
    StrokeWidth = 2 
});
```

### TextStyle Customization

Customize header text appearance:

```xml
<treemap:SfTreeMap.Levels>
    <treemap:TreeMapLevel GroupPath="Continent">
        <treemap:TreeMapLevel.TextStyle>
            <treemap:TreeMapTextStyle TextColor="DarkBlue"
                                      FontSize="16"
                                      FontFamily="Arial"
                                      FontAttributes="Bold" />
        </treemap:TreeMapLevel.TextStyle>
    </treemap:TreeMapLevel>
</treemap:SfTreeMap.Levels>
```

```csharp
treeMap.Levels.Add(new TreeMapLevel 
{ 
    GroupPath = "Continent",
    TextStyle = new TreeMapTextStyle
    {
        TextColor = Colors.DarkBlue,
        FontSize = 16,
        FontFamily = "Arial",
        FontAttributes = FontAttributes.Bold
    }
});
```

### Complete Customization Example

```xml
<treemap:SfTreeMap DataSource="{Binding PopulationDetails}"
                   PrimaryValuePath="Population">
    <treemap:SfTreeMap.Levels>
        <treemap:TreeMapLevel GroupPath="Continent"
                              Spacing="5"
                              HeaderHeight="35"
                              Background="#E3F2FD"
                              Stroke="#1976D2"
                              StrokeWidth="2">
            <treemap:TreeMapLevel.TextStyle>
                <treemap:TreeMapTextStyle TextColor="#0D47A1"
                                          FontSize="18"
                                          FontAttributes="Bold" />
            </treemap:TreeMapLevel.TextStyle>
        </treemap:TreeMapLevel>
    </treemap:SfTreeMap.Levels>
    
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Country" />
    </treemap:SfTreeMap.LeafItemSettings>
    
    <treemap:SfTreeMap.LeafItemBrushSettings>
        <treemap:TreeMapUniformBrushSettings Brush="Orange" />
    </treemap:SfTreeMap.LeafItemBrushSettings>
</treemap:SfTreeMap>
```

### Different Styles for Different Levels

```xml
<treemap:SfTreeMap.Levels>
    <!-- Level 1: Department -->
    <treemap:TreeMapLevel GroupPath="Department"
                          Background="#BBDEFB"
                          HeaderHeight="40">
        <treemap:TreeMapLevel.TextStyle>
            <treemap:TreeMapTextStyle FontSize="18"
                                      FontAttributes="Bold" />
        </treemap:TreeMapLevel.TextStyle>
    </treemap:TreeMapLevel>
    
    <!-- Level 2: Team -->
    <treemap:TreeMapLevel GroupPath="Team"
                          Background="#C8E6C9"
                          HeaderHeight="30">
        <treemap:TreeMapLevel.TextStyle>
            <treemap:TreeMapTextStyle FontSize="14"
                                      FontAttributes="Italic" />
        </treemap:TreeMapLevel.TextStyle>
    </treemap:TreeMapLevel>
</treemap:SfTreeMap.Levels>
```

## Group Item Brush Settings

Use `GroupItemBrushSettings` to color group headers differently from leaf items. This only applies when levels are defined.

### Using PaletteBrushSettings for Groups

```xml
<treemap:SfTreeMap DataSource="{Binding PopulationDetails}"
                   PrimaryValuePath="Population">
    <treemap:SfTreeMap.Levels>
        <treemap:TreeMapLevel GroupPath="Continent" />
    </treemap:SfTreeMap.Levels>
    
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Country" />
    </treemap:SfTreeMap.LeafItemSettings>
    
    <treemap:SfTreeMap.GroupItemBrushSettings>
        <treemap:TreeMapPaletteBrushSettings>
            <treemap:TreeMapPaletteBrushSettings.Brushes>
                <SolidColorBrush>#003790</SolidColorBrush>
                <SolidColorBrush>#FF8F00</SolidColorBrush>
                <SolidColorBrush>#28A745</SolidColorBrush>
            </treemap:TreeMapPaletteBrushSettings.Brushes>
        </treemap:TreeMapPaletteBrushSettings>
    </treemap:SfTreeMap.GroupItemBrushSettings>
</treemap:SfTreeMap>
```

```csharp
treeMap.GroupItemBrushSettings = new TreeMapPaletteBrushSettings
{
    Brushes = new List<Brush>
    {
        new SolidColorBrush(Color.FromArgb("#003790")),
        new SolidColorBrush(Color.FromArgb("#FF8F00")),
        new SolidColorBrush(Color.FromArgb("#28A745"))
    }
};
```

**Effect**: Each group (continent) gets a different color from the palette. The first group uses the first color, the second group uses the second color, and so on. If there are more groups than colors, it cycles through the palette.

### Group vs Leaf Coloring

- **GroupItemBrushSettings**: Colors group headers and the background area of groups
- **LeafItemBrushSettings**: Colors individual leaf items (data rectangles)

Both can be used together for clear visual hierarchy.

## Practical Examples

### Example 1: Sales Hierarchy (Region → Country → Product)

```csharp
public class SalesData
{
    public string Region { get; set; }
    public string Country { get; set; }
    public string Product { get; set; }
    public decimal Revenue { get; set; }
}

public class SalesViewModel
{
    public ObservableCollection<SalesData> Sales { get; set; }
    
    public SalesViewModel()
    {
        Sales = new ObservableCollection<SalesData>
        {
            new SalesData { Region = "Americas", Country = "USA", Product = "Laptop", Revenue = 450000 },
            new SalesData { Region = "Americas", Country = "USA", Product = "Tablet", Revenue = 280000 },
            new SalesData { Region = "Americas", Country = "Canada", Product = "Laptop", Revenue = 180000 },
            new SalesData { Region = "Europe", Country = "UK", Product = "Laptop", Revenue = 320000 },
            new SalesData { Region = "Europe", Country = "Germany", Product = "Tablet", Revenue = 240000 },
            new SalesData { Region = "Asia", Country = "Japan", Product = "Laptop", Revenue = 380000 },
        };
    }
}
```

```xml
<treemap:SfTreeMap DataSource="{Binding Sales}"
                   PrimaryValuePath="Revenue">
    <treemap:SfTreeMap.Levels>
        <treemap:TreeMapLevel GroupPath="Region"
                              HeaderHeight="35"
                              Background="#E8EAF6">
            <treemap:TreeMapLevel.TextStyle>
                <treemap:TreeMapTextStyle FontSize="16"
                                          FontAttributes="Bold" />
            </treemap:TreeMapLevel.TextStyle>
        </treemap:TreeMapLevel>
        
        <treemap:TreeMapLevel GroupPath="Country"
                              HeaderHeight="25"
                              Background="#F3E5F5">
            <treemap:TreeMapLevel.TextStyle>
                <treemap:TreeMapTextStyle FontSize="12" />
            </treemap:TreeMapLevel.TextStyle>
        </treemap:TreeMapLevel>
    </treemap:SfTreeMap.Levels>
    
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Product" />
    </treemap:SfTreeMap.LeafItemSettings>
</treemap:SfTreeMap>
```

### Example 2: File System Visualization

```csharp
public class FileSystemItem
{
    public string Drive { get; set; }
    public string Folder { get; set; }
    public string FileName { get; set; }
    public long SizeInBytes { get; set; }
}
```

```csharp
SfTreeMap treeMap = new SfTreeMap
{
    DataSource = viewModel.FileSystemItems,
    PrimaryValuePath = "SizeInBytes",
    LeafItemSettings = new TreeMapLeafItemSettings { LabelPath = "FileName" }
};

treeMap.Levels.Add(new TreeMapLevel 
{ 
    GroupPath = "Drive",
    Background = new SolidColorBrush(Color.FromArgb("#FFF9C4")),
    HeaderHeight = 40
});

treeMap.Levels.Add(new TreeMapLevel 
{ 
    GroupPath = "Folder",
    Background = new SolidColorBrush(Color.FromArgb("#E1F5FE")),
    HeaderHeight = 28
});

this.Content = treeMap;
```

### Example 3: Budget Breakdown (Department → Category → Expense)

```csharp
public class BudgetData
{
    public string Department { get; set; }
    public string Category { get; set; }
    public string ExpenseType { get; set; }
    public decimal Amount { get; set; }
}
```

```xml
<treemap:SfTreeMap DataSource="{Binding BudgetItems}"
                   PrimaryValuePath="Amount"
                   RangeColorValuePath="Amount">
    <treemap:SfTreeMap.Levels>
        <treemap:TreeMapLevel GroupPath="Department"
                              Spacing="4"
                              HeaderHeight="38"
                              Stroke="#1976D2"
                              StrokeWidth="2">
            <treemap:TreeMapLevel.TextStyle>
                <treemap:TreeMapTextStyle TextColor="#FFFFFF"
                                          FontSize="16"
                                          FontAttributes="Bold" />
            </treemap:TreeMapLevel.TextStyle>
        </emap:TreeMapLevel>
        
        <treemap:TreeMapLevel GroupPath="Category"
                              HeaderHeight="28">
            <treemap:TreeMapLevel.TextStyle>
                <treemap:TreeMapTextStyle FontSize="13" />
            </treemap:TreeMapLevel.TextStyle>
        </treemap:TreeMapLevel>
    </treemap:SfTreeMap.Levels>
    
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="ExpenseType" />
    </treemap:SfTreeMap.LeafItemSettings>
    
    <treemap:SfTreeMap.GroupItemBrushSettings>
        <treemap:TreeMapPaletteBrushSettings>
            <treemap:TreeMapPaletteBrushSettings.Brushes>
                <SolidColorBrush>#1976D2</SolidColorBrush>
                <SolidColorBrush>#388E3C</SolidColorBrush>
                <SolidColorBrush>#D32F2F</SolidColorBrush>
                <SolidColorBrush>#F57C00</SolidColorBrush>
            </treemap:TreeMapPaletteBrushSettings.Brushes>
        </treemap:TreeMapPaletteBrushSettings>
    </treemap:SfTreeMap.GroupItemBrushSettings>
</treemap:SfTreeMap>
```

## Best Practices

### 1. Limit Level Depth

**Recommended**: 1-3 levels for optimal readability
**Avoid**: More than 4-5 levels (becomes cluttered)

### 2. Use Meaningful GroupPath Names

```csharp
// Good - clear hierarchy
GroupPath = "Region"
GroupPath = "Country"
GroupPath = "Product"

// Avoid - generic names
GroupPath = "Category1"
GroupPath = "Category2"
```

### 3. Differentiate Level Appearance

Make each level visually distinct:
- Different header heights
- Different background colors
- Different font sizes
- Progressive indentation (implicit through nesting)

### 4. Consistent Naming Convention

Ensure GroupPath values in data are consistent:

```csharp
// Good - consistent
{ Continent = "North America", ... }
{ Continent = "South America", ... }
{ Continent = "Europe", ... }

// Problematic - inconsistent
{ Continent = "North America", ... }
{ Continent = "N. America", ... }  // Will create separate group
{ Continent = "north america", ... }  // Case-sensitive, separate group
```

### 5. Order Levels Logically

Order levels from broad to specific:

```xml
<!-- Good: Broad to specific -->
<treemap:TreeMapLevel GroupPath="Continent" />
<treemap:TreeMapLevel GroupPath="Country" />
<treemap:TreeMapLevel GroupPath="City" />

<!-- Avoid: Random order confuses hierarchy -->
<treemap:TreeMapLevel GroupPath="City" />
<treemap:TreeMapLevel GroupPath="Continent" />
```

### 6. Performance Considerations

- Fewer levels = better performance
- Limit leaf items per group (< 100 recommended)
- Consider data aggregation for large datasets

## Troubleshooting

### Issue 1: Levels Not Showing

**Symptoms**: TreeMap displays but no group headers appear

**Solutions**:
1. Verify `GroupPath` exactly matches a property name in your data model (case-sensitive)
2. Ensure the property has values (not all null or empty)
3. Check that data has items with different values for that property
4. Confirm `Levels.Add()` is called or XAML `Levels` collection is defined

### Issue 2: All Items in One Group

**Symptoms**: Single group header with all items underneath

**Cause**: All data items have the same value for the GroupPath property

**Solution**: Verify your data has variety in the grouping property

```csharp
// Check data
foreach (var item in dataSource)
{
    Debug.WriteLine($"GroupProperty: {item.GroupProperty}");
}
```

### Issue 3: Header Text Not Showing

**Symptoms**: Group headers appear but no text labels

**Solutions**:
1. Increase `HeaderHeight` if it's too small
2. Check `TextStyle.TextColor` isn't same as `Background`
3. Verify `FontSize` is reasonable (not 0 or negative)
4. Ensure property values aren't null

### Issue 4: Wrong Property Used for Grouping

**Symptoms**: Unexpected grouping behavior

**Solution**: Double-check property name spelling and case:

```csharp
// Data model
public string Continent { get; set; }

// TreeMap level - must match exactly
GroupPath = "Continent"  // Correct
GroupPath = "continent"  // Wrong - case sensitive
GroupPath = "Region"     // Wrong - different property
```

### Issue 5: GroupItemBrushSettings Not Applying

**Symptoms**: Group colors not changing

**Solutions**:
1. Ensure `Levels` collection is defined (GroupItemBrushSettings only works with levels)
2. Verify `GroupItemBrushSettings` is set, not `LeafItemBrushSettings`
3. Check brush collection has colors defined

### Issue 6: Headers Too Small/Large

**Solution**: Adjust `HeaderHeight` based on font size and content:

```csharp
// Rule of thumb: HeaderHeight should be ~2x FontSize
TextStyle = new TreeMapTextStyle { FontSize = 14 }
HeaderHeight = 28  // Good proportion

TextStyle = new TreeMapTextStyle { FontSize = 20 }
HeaderHeight = 40  // Good proportion
```

## Related Topics

- [Getting Started](getting-started.md) - Basic TreeMap setup
- [Layouts](layouts.md) - Layout algorithms for hierarchical data
- [Brush Settings](brush-settings.md) - Coloring strategies
- [Leaf Item Customization](leaf-item-customization.md) - Customizing data items

## Summary

- Use `Levels` to create multi-level hierarchies in TreeMap
- `GroupPath` specifies which property to group by at each level
- Support for unlimited levels (1-3 recommended)
- Customize each level's appearance independently
- Use `GroupItemBrushSettings` to color group headers
- Ensure GroupPath property names match your data model exactly
- Order levels from broad to specific for logical hierarchy
