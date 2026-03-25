# Leaf Item Customization in .NET MAUI TreeMap

Leaf items are the individual rectangles representing data points in the TreeMap. Customize their appearance through labels, spacing, borders, and styling to create clear, visually appealing visualizations.

## Table of Contents
- [Overview](#overview)
- [Label Configuration](#label-configuration)
- [Spacing and Borders](#spacing-and-borders)
- [Text Styling](#text-styling)
- [Label Overflow Modes](#label-overflow-modes)
- [Show/Hide Labels](#showhide-labels)
- [Practical Examples](#practical-examples)
- [Troubleshooting](#troubleshooting)

## Overview

The `LeafItemSettings` property controls leaf item appearance through the `TreeMapLeafItemSettings` class.

**Key Properties:**
- `LabelPath`: Data property to display as label
- `ShowLabels`: Show/hide all labels
- `LabelStyle`: Text formatting (size, color, font)
- `OverflowMode`: How labels behave when too large
- `Spacing`: Gap between leaf items
- `Stroke`: Border color
- `StrokeWidth`: Border thickness

**Basic Setup:**

```xml
<treemap:SfTreeMap DataSource="{Binding Data}"
                   PrimaryValuePath="Size">
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Name" />
    </treemap:SfTreeMap.LeafItemSettings>
</treemap:SfTreeMap>
```

## Label Configuration

### LabelPath Property

Specifies which data property to display as the label on each leaf item.

**Example Model:**
```csharp
public class Product
{
    public string Name { get; set; }
    public string Category { get; set; }
    public decimal Sales { get; set; }
}
```

**XAML:**
```xml
<treemap:SfTreeMap DataSource="{Binding Products}"
                   PrimaryValuePath="Sales">
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Name" />
    </treemap:SfTreeMap.LeafItemSettings>
</treemap:SfTreeMap>
```

Result: Each rectangle displays the `Name` property value (e.g., "Laptop", "Mouse", "Keyboard").

**C# Implementation:**
```csharp
treeMap.LeafItemSettings = new TreeMapLeafItemSettings
{
    LabelPath = "Name"
};
```

### Choosing Different Properties

```xml
<!-- Show category instead of name -->
<treemap:TreeMapLeafItemSettings LabelPath="Category" />

<!-- Show numeric values (formatted automatically) -->
<treemap:TreeMapLeafItemSettings LabelPath="Sales" />
```

## Spacing and Borders

### Spacing Property

Controls the gap between leaf item rectangles. Higher values create more white space.

**XAML:**
```xml
<treemap:SfTreeMap DataSource="{Binding Data}"
                   PrimaryValuePath="Size">
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Name"
                                         Spacing="4" />
    </treemap:SfTreeMap.LeafItemSettings>
</treemap:SfTreeMap>
```

**C# Implementation:**
```csharp
treeMap.LeafItemSettings = new TreeMapLeafItemSettings
{
    LabelPath = "Name",
    Spacing = 4
};
```

**Spacing Values:**
- `0`: No gap (items touch)
- `2-5`: Subtle separation (recommended)
- `10+`: Clear visual separation
- `20+`: Significant gaps

**Visual Comparison:**
- Spacing = 0: Packed, no visible borders
- Spacing = 3: Subtle definition between items
- Spacing = 10: Clear separation, easier to distinguish items

### Stroke (Border Color)

Adds colored borders around leaf items.

**XAML:**
```xml
<treemap:TreeMapLeafItemSettings LabelPath="Name"
                                 Spacing="4"
                                 Stroke="Black"
                                 StrokeWidth="1" />
```

**C# Implementation:**
```csharp
treeMap.LeafItemSettings = new TreeMapLeafItemSettings
{
    LabelPath = "Name",
    Spacing = 4,
    Stroke = Brush.Black,
    StrokeWidth = 1
};
```

**Using Hex Colors:**
```xml
<treemap:TreeMapLeafItemSettings Stroke="#424242" StrokeWidth="1" />
```

```csharp
Stroke = new SolidColorBrush(Color.FromArgb("#424242"))
```

### StrokeWidth Property

Controls border thickness in device-independent pixels.

**Recommended Values:**
- `0.5`: Very subtle border
- `1`: Standard border (recommended)
- `2`: Thick border
- `3+`: Very prominent border

**Example - Bold Borders:**
```xml
<treemap:TreeMapLeafItemSettings LabelPath="Name"
                                 Stroke="DarkGray"
                                 StrokeWidth="2" />
```

### Combining Spacing and Stroke

```xml
<!-- Clear separation with borders -->
<treemap:TreeMapLeafItemSettings LabelPath="Name"
                                 Spacing="5"
                                 Stroke="White"
                                 StrokeWidth="2" />
```

Creates well-defined, separated rectangles with visible white borders.

## Text Styling

### LabelStyle Property

Customize label text appearance through the `TextStyle` property.

**Available Properties:**
- `TextColor`: Label text color
- `FontSize`: Text size
- `FontFamily`: Font typeface
- `FontAttributes`: Bold, Italic, or None

### Basic Text Styling

**XAML:**
```xml
<treemap:SfTreeMap DataSource="{Binding Data}"
                   PrimaryValuePath="Size">
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Name">
            <treemap:TreeMapLeafItemSettings.LabelStyle>
                <treemap:TreeMapLabelStyle TextColor="White"
                                           FontSize="14"
                                           FontAttributes="Bold" />
            </treemap:TreeMapLeafItemSettings.LabelStyle>
        </treemap:TreeMapLeafItemSettings>
    </treemap:SfTreeMap.LeafItemSettings>
</treemap:SfTreeMap>
```

**C# Implementation:**
```csharp
treeMap.LeafItemSettings = new TreeMapLeafItemSettings
{
    LabelPath = "Name",
    LabelStyle = new TreeMapLabelStyle
    {
        TextColor = Colors.White,
        FontSize = 14,
        FontAttributes = FontAttributes.Bold
    }
};
```

### Font Size Examples

```csharp
// Small labels (for compact items)
FontSize = 10

// Normal labels (default, recommended)
FontSize = 12

// Large labels (for emphasis)
FontSize = 16

// Extra large (for prominent items)
FontSize = 20
```

### Font Attributes

**Bold:**
```xml
<treemap:TreeMapLabelStyle FontAttributes="Bold" />
```

**Italic:**
```xml
<treemap:TreeMapLabelStyle FontAttributes="Italic" />
```

**Bold and Italic:**
```xml
<treemap:TreeMapLabelStyle FontAttributes="Bold, Italic" />
```

### Custom Fonts

**XAML:**
```xml
<treemap:TreeMapLabelStyle FontFamily="Roboto"
                           FontSize="14"
                           FontAttributes="Bold" />
```

**C# Implementation:**
```csharp
LabelStyle = new TreeMapLabelStyle
{
    FontFamily = "Roboto",
    FontSize = 14,
    FontAttributes = FontAttributes.Bold
}
```

### Text Color Contrast

Ensure labels are readable against background colors:

**White Labels on Dark Backgrounds:**
```csharp
LabelStyle = new TreeMapLabelStyle { TextColor = Colors.White }
// Works well with: Dark blue, Dark green, Black, Dark red
```

**Black Labels on Light Backgrounds:**
```csharp
LabelStyle = new TreeMapLabelStyle { TextColor = Colors.Black }
// Works well with: Yellow, Light blue, Light green, White
```

**Auto-contrast (requires custom logic):**
```csharp
// Example: Choose text color based on background brightness
private Color GetContrastColor(Color backgroundColor)
{
    var brightness = (backgroundColor.Red * 299 + backgroundColor.Green * 587 + backgroundColor.Blue * 114) / 1000;
    return brightness > 128 ? Colors.Black : Colors.White;
}
```

## Label Overflow Modes

Controls what happens when a label is too long to fit inside its rectangle.

**TreeMapLabelOverflowMode Enum:**
- `Trim`: Truncate text with ellipsis (...)
- `Wrap`: Wrap text to multiple lines
- `Hide`: Hide labels that don't fit

### Trim Mode (Default)

Truncates text and adds ellipsis when label exceeds rectangle width.

**XAML:**
```xml
<treemap:TreeMapLeafItemSettings LabelPath="Name"
                                 OverflowMode="Trim">
    <treemap:TreeMapLeafItemSettings.LabelStyle>
        <treemap:TreeMapLabelStyle FontSize="12" />
    </treemap:TreeMapLeafItemSettings.LabelStyle>
</treemap:TreeMapLeafItemSettings>
```

**Example:**
- Full text: "MacBook Pro 16-inch"
- Displayed: "MacBook Pro 1..."

### Wrap Mode

Wraps text to multiple lines when too long.

**XAML:**
```xml
<treemap:TreeMapLeafItemSettings LabelPath="Name"
                                 OverflowMode="Wrap">
    <treemap:TreeMapLeafItemSettings.LabelStyle>
        <treemap:TreeMapLabelStyle FontSize="12" />
    </treemap:TreeMapLeafItemSettings.LabelStyle>
</treemap:TreeMapLeafItemSettings>
```

**Example:**
- Full text: "MacBook Pro 16-inch"
- Displayed:
  ```
  MacBook Pro
  16-inch
  ```

**Best for:** Larger rectangles where multiple lines fit comfortably.

### Hide Mode

Hides labels completely if they don't fit in the rectangle.

**XAML:**
```xml
<treemap:TreeMapLeafItemSettings LabelPath="Name"
                                 OverflowMode="Hide">
    <treemap:TreeMapLeafItemSettings.LabelStyle>
        <treemap:TreeMapLabelStyle FontSize="12" />
    </treemap:TreeMapLeafItemSettings.LabelStyle>
</treemap:TreeMapLeafItemSettings>
```

**Best for:** Clean appearance where only larger items show labels.

### Choosing the Right Mode

| Mode | Use When | Pros | Cons |
|------|----------|------|------|
| **Trim** | Need to show all labels | All items labeled | May truncate important text |
| **Wrap** | Have enough vertical space | Shows full text | Can look cluttered |
| **Hide** | Want clean aesthetics | Clean, uncluttered | Small items unlabeled |

## Show/Hide Labels

### ShowLabels Property

Toggle all labels on or off.

**Show Labels (Default):**
```xml
<treemap:TreeMapLeafItemSettings LabelPath="Name"
                                 ShowLabels="True" />
```

**Hide All Labels:**
```xml
<treemap:TreeMapLeafItemSettings LabelPath="Name"
                                 ShowLabels="False" />
```

**C# Implementation:**
```csharp
treeMap.LeafItemSettings = new TreeMapLeafItemSettings
{
    LabelPath = "Name",
    ShowLabels = false
};
```

### When to Hide Labels

- Focus purely on size and color relationships
- Avoid clutter when there are many small items
- Rely on tooltips for item identification
- Create minimalist visualizations

**Example: Label-free TreeMap with Tooltips:**
```xml
<treemap:SfTreeMap DataSource="{Binding Data}"
                   PrimaryValuePath="Size"
                   ShowToolTip="True">
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Name"
                                         ShowLabels="False" />
    </treemap:SfTreeMap.LeafItemSettings>
</treemap:SfTreeMap>
```

Users hover over items to see labels in tooltips instead of seeing them permanently.

## Practical Examples

### Example 1: Professional Style with Clear Borders

```xml
<treemap:SfTreeMap DataSource="{Binding SalesData}"
                   PrimaryValuePath="Revenue">
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="ProductName"
                                         Spacing="3"
                                         Stroke="#BDBDBD"
                                         StrokeWidth="1"
                                         OverflowMode="Trim">
            <treemap:TreeMapLeafItemSettings.LabelStyle>
                <treemap:TreeMapLabelStyle TextColor="White"
                                           FontSize="13"
                                           FontAttributes="Bold" />
            </treemap:TreeMapLeafItemSettings.LabelStyle>
        </treemap:TreeMapLeafItemSettings>
    </treemap:SfTreeMap.LeafItemSettings>
</treemap:SfTreeMap>
```

**Result:** Clean, professional appearance with subtle borders, bold white labels, and truncated text.

### Example 2: Minimalist No-Label Design

```csharp
public class Portfolio
{
    public string Stock { get; set; }
    public decimal Value { get; set; }
}

// TreeMap setup
treeMap.DataSource = portfolioData;
treeMap.PrimaryValuePath = "Value";
treeMap.ShowToolTip = true;

treeMap.LeafItemSettings = new TreeMapLeafItemSettings
{
    LabelPath = "Stock",
    ShowLabels = false,  // No labels shown
    Spacing = 2,
    Stroke = Brush.White,
    StrokeWidth = 1
};
```

**Result:** Clean visualization showing only colored rectangles. Users hover for stock names in tooltips.

### Example 3: Multi-line Labels for Large Items

```xml
<treemap:SfTreeMap DataSource="{Binding CountryData}"
                   PrimaryValuePath="Population">
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="CountryName"
                                         OverflowMode="Wrap"
                                         Spacing="5"
                                         Stroke="DarkGray"
                                         StrokeWidth="1">
            <treemap:TreeMapLeafItemSettings.LabelStyle>
                <treemap:TreeMapLabelStyle TextColor="Black"
                                           FontSize="14" />
            </treemap:TreeMapLeafItemSettings.LabelStyle>
        </treemap:TreeMapLeafItemSettings>
    </treemap:SfTreeMap.LeafItemSettings>
</treemap:SfTreeMap>
```

**Result:** Full country names displayed with wrapping for longer names.

### Example 4: Dynamic Text Color Based on Background

```csharp
// Custom logic to set text color based on item background
public void SetupTreeMapWithDynamicColors()
{
    treeMap.DataSource = data;
    treeMap.PrimaryValuePath = "Value";
    
    treeMap.LeafItemSettings = new TreeMapLeafItemSettings
    {
        LabelPath = "Name",
        Spacing = 3,
        LabelStyle = new TreeMapLabelStyle
        {
            FontSize = 12,
            FontAttributes = FontAttributes.Bold
        }
    };
    
    // Note: Requires custom renderer or template for per-item text color
    // This example shows the concept
}
```

### Example 5: Hide Small Item Labels Only

```xml
<!-- Use Hide mode so only larger rectangles show labels -->
<treemap:SfTreeMap DataSource="{Binding Data}"
                   PrimaryValuePath="Size">
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Name"
                                         OverflowMode="Hide">
            <treemap:TreeMapLeafItemSettings.LabelStyle>
                <treemap:TreeMapLabelStyle FontSize="12"
                                           TextColor="White" />
            </treemap:TreeMapLeafItemSettings.LabelStyle>
        </treemap:TreeMapLeafItemSettings>
    </treemap:SfTreeMap.LeafItemSettings>
</treemap:SfTreeMap>
```

**Result:** Only items large enough to fit the label display it. Small items remain unlabeled but visible.

## Troubleshooting

### Issue 1: Labels Not Showing

**Symptoms:** No labels visible on leaf items

**Solutions:**
1. Verify `LabelPath` matches an actual property name (case-sensitive)
2. Check that `ShowLabels` is not set to `False`
3. Ensure `OverflowMode` is not `Hide` with small items
4. Confirm `LabelStyle.FontSize` is not 0
5. Check text color contrasts with background

```csharp
// Debug: Print property names
foreach (var property in typeof(YourModel).GetProperties())
{
    Debug.WriteLine($"Property: {property.Name}");
}
```

### Issue 2: Labels Truncated Too Much

**Symptoms:** Labels showing only 1-2 characters before ellipsis

**Solutions:**
1. Reduce `FontSize` to fit more characters
2. Increase `Spacing` if rectangles are too small
3. Use `OverflowMode="Wrap"` for larger items
4. Consider abbreviating data values in the model

**Example:**
```csharp
// In your ViewModel or data preparation
public string ShortName => Name.Length > 15 ? Name.Substring(0, 12) + "..." : Name;

// Use ShortName for LabelPath
LabelPath = "ShortName"
```

### Issue 3: Wrapped Labels Look Cluttered

**Symptoms:** Multi-line labels overlap or look messy

**Solutions:**
1. Switch to `OverflowMode="Trim"` or `"Hide"`
2. Reduce `FontSize` to fit more text per line
3. Increase `Spacing` between items
4. Use shorter data values

### Issue 4: Borders Not Visible

**Symptoms:** No visible borders despite setting Stroke

**Solutions:**
1. Increase `StrokeWidth` (try 2 or higher)
2. Ensure `Stroke` color contrasts with item background
3. Check that `Spacing` > 0 (borders only visible with spacing)

```xml
<!-- Good: Clear borders -->
<treemap:TreeMapLeafItemSettings Spacing="4"
                                 Stroke="White"
                                 StrokeWidth="2" />
```

### Issue 5: Font Not Applying

**Symptoms:** Custom font not showing

**Solutions:**
1. Ensure font is installed and available on target platform
2. Use correct font family name (case-sensitive)
3. Register custom fonts in MauiProgram.cs:

```csharp
builder
    .UseMauiApp<App>()
    .ConfigureFonts(fonts =>
    {
        fonts.AddFont("YourFont.ttf", "YourFontName");
    });
```

4. Reference registered font name in LabelStyle:
```xml
<treemap:TreeMapLabelStyle FontFamily="YourFontName" />
```

### Issue 6: Text Color Unreadable

**Symptoms:** Labels hard to see against background

**Solutions:**
1. Use contrasting colors (white on dark, black on light)
2. Add stroke for text outline effect (requires custom styling)
3. Adjust brush settings for better background colors
4. Test with different color schemes

**Testing Text Contrast:**
```csharp
// Dark backgrounds
LabelStyle = new TreeMapLabelStyle { TextColor = Colors.White };

// Light backgrounds
LabelStyle = new TreeMapLabelStyle { TextColor = Colors.Black };
```

## Related Topics

- [Brush Settings](brush-settings.md) - Controlling leaf item background colors
- [Tooltip Configuration](tooltip.md) - Adding interactive tooltips
- [Getting Started](getting-started.md) - Basic TreeMap setup

## Summary

- Use `LabelPath` to specify which property displays as labels
- Control spacing with `Spacing` property (2-5 recommended)
- Add borders with `Stroke` and `StrokeWidth`
- Style text with `LabelStyle` (color, size, font, attributes)
- Choose overflow mode: `Trim` (default), `Wrap`, or `Hide`
- Toggle all labels with `ShowLabels` property
- Ensure text color contrasts with background for readability
- Use `OverflowMode="Hide"` for clean, minimalist designs
- Combine with tooltips when hiding labels for item identification
