# Accessibility, Events, and Advanced Features in .NET MAUI TreeMap

Ensure your TreeMap is accessible to all users and handle advanced features like drilldown navigation and RTL support.

## Table of Contents
- [Accessibility Overview](#accessibility-overview)
- [Keyboard Navigation](#keyboard-navigation)
- [Screen Reader Support](#screen-reader-support)
- [WCAG Compliance](#wcag-compliance)
- [Events](#events)
- [Drilldown Navigation](#drilldown-navigation)
- [Right-to-Left (RTL) Support](#right-to-left-rtl-support)
- [Practical Examples](#practical-examples)
- [Troubleshooting](#troubleshooting)

## Accessibility Overview

Accessibility ensures that users with disabilities can effectively use the TreeMap. The .NET MAUI TreeMap supports various accessibility features compliant with WCAG standards.

**Key Accessibility Features:**
- Keyboard navigation support
- Screen reader compatibility
- High contrast mode support
- Touch target sizing
- Color contrast compliance
- Semantic descriptions

### Why Accessibility Matters

- **Legal Compliance**: Many regions require accessibility (ADA, Section 508, WCAG)
- **Broader Audience**: Users with disabilities can access your app
- **Better UX**: Accessibility improvements benefit all users
- **Best Practice**: Follows platform guidelines (iOS, Android, Windows)

## Keyboard Navigation

The TreeMap supports keyboard interaction for navigation and selection.

### Supported Keys

**Navigation:**
- **Tab**: Move focus to/from TreeMap
- **Arrow Keys**: Navigate between leaf items
  - Up/Down/Left/Right: Move focus to adjacent items
- **Enter/Space**: Select focused item (when SelectionMode enabled)
- **Escape**: Clear selection

### Enabling Keyboard Support

Keyboard support is enabled by default. Ensure the TreeMap is focusable:

**XAML:**
```xml
<treemap:SfTreeMap DataSource="{Binding Data}"
                   PrimaryValuePath="Value"
                   SelectionMode="Single">
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Name" />
    </treemap:SfTreeMap.LeafItemSettings>
</treemap:SfTreeMap>
```

**Key Navigation Behavior:**
1. User tabs to TreeMap → First leaf item receives focus
2. Arrow keys → Move focus between items
3. Enter/Space → Select focused item
4. Tab away → Focus moves to next control

### Focus Visual

The focused item displays a focus indicator (border or highlight) distinct from selection highlighting.

### Keyboard Accessibility Best Practices

1. **Ensure Logical Tab Order**: TreeMap should be in natural tab sequence
2. **Provide Keyboard Alternatives**: All mouse actions available via keyboard
3. **Clear Focus Indicators**: Users can see which item has focus
4. **Document Shortcuts**: Inform users of keyboard interactions

#### 3. Keyboard Accessible (2.1.1)

All functionality available via keyboard (supported by default).

#### 4. Focus Visible (2.4.7)

Focus indicators clearly visible (provided by TreeMap).

#### 5. Target Size (2.5.5)

Interactive elements at least 44x44 pixels.

**Ensure Adequate Item Size:**
- Minimum leaf item size: 44x44 pixels
- Use appropriate `Spacing` to avoid accidental taps
- Test on touch devices

```xml
<treemap:TreeMapLeafItemSettings Spacing="3" />
```

Spacing helps separate items for accurate touch targeting.

### Color Independence

Don't rely solely on color to convey information. Use additional cues:

**Good: Color + Text Labels**
```xml
<treemap:TreeMapRangeBrush LegendLabel="High Risk (>75%)" 
                           From="75" To="100" 
                           Brush="Red" />
<treemap:TreeMapRangeBrush LegendLabel="Low Risk (<25%)" 
                           From="0" To="25" 
                           Brush="Green" />
```

Legend labels provide text description alongside color.

## Events

The TreeMap raises events for user interactions and lifecycle changes.

### SelectionChanged Event

Raised when selection changes.

**Event Arguments:**
- `OldItems`: Previously selected items
- `NewItems`: Currently selected items

**Usage:**
```xml
<treemap:SfTreeMap SelectionChanged="OnSelectionChanged" />
```

```csharp
private void OnSelectionChanged(object sender, TreeMapSelectionChangedEventArgs e)
{
    foreach (var item in e.NewItems)
    {
        Debug.WriteLine($"Selected: {item}");
    }
}
```

**See:** [Selection and Interaction](selection-interaction.md) for detailed coverage.

### Common Event Patterns

#### Track User Interactions

```csharp
private void OnSelectionChanged(object sender, TreeMapSelectionChangedEventArgs e)
{
    // Log analytics
    Analytics.TrackEvent("TreeMap_ItemSelected", new Dictionary<string, string>
    {
        { "ItemCount", e.NewItems.Count.ToString() },
        { "SelectionMode", treeMap.SelectionMode.ToString() }
    });
}
```

#### Update UI Based on Selection

```csharp
private void OnSelectionChanged(object sender, TreeMapSelectionChangedEventArgs e)
{
    var hasSelection = e.NewItems.Count > 0;
    
    ActionButton.IsEnabled = hasSelection;
    StatusLabel.Text = hasSelection 
        ? $"{e.NewItems.Count} item(s) selected" 
        : "No selection";
}
```

## Drilldown Navigation

Drilldown allows users to navigate into hierarchical data by tapping group headers to view child items.

### EnableDrilldown Property

Enable drilldown navigation for multi-level hierarchical TreeMaps.

**XAML:**
```xml
<treemap:SfTreeMap DataSource="{Binding CountryData}"
                   PrimaryValuePath="Population"
                   EnableDrillDown="True">
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Name">
            <treemap:TreeMapLeafItemSettings.LabelStyle>
                <treemap:TreeMapLabelStyle FontSize="12" />
            </treemap:TreeMapLeafItemSettings.LabelStyle>
        </treemap:TreeMapLeafItemSettings>
    </treemap:SfTreeMap.LeafItemSettings>
    
    <treemap:SfTreeMap.Levels>
        <treemap:TreeMapLevel GroupPath="Continent" />
        <treemap:TreeMapLevel GroupPath="Country" />
    </treemap:SfTreeMap.Levels>
</treemap:SfTreeMap>
```

**C# Implementation:**
```csharp
treeMap.EnableDrillDown = true;
treeMap.PrimaryValuePath = "Population";

treeMap.Levels.Add(new TreeMapLevel { GroupPath = "Continent" });
treeMap.Levels.Add(new TreeMapLevel { GroupPath = "Country" });

treeMap.LeafItemSettings = new TreeMapLeafItemSettings 
{ 
    LabelPath = "Name",
    LabelStyle = new TreeMapLabelStyle { FontSize = 12 }
};
```

### Drilldown Behavior

**User Interaction:**
1. User taps a group header (e.g., "Asia")
2. TreeMap drills down to show child items (countries in Asia)
3. Breadcrumb or back button appears
4. User can drill further or navigate back

**Navigation:**
- Tap group header → Drill down to child level
- Tap back/up button → Return to parent level
- Navigate through multiple levels

### Data Structure for Drilldown

**Example Model:**
```csharp
public class PopulationData
{
    public string Continent { get; set; }
    public string Country { get; set; }
    public string City { get; set; }
    public long Population { get; set; }
}

// Sample data
var data = new List<PopulationData>
{
    new PopulationData { Continent = "Asia", Country = "China", City = "Beijing", Population = 21_540_000 },
    new PopulationData { Continent = "Asia", Country = "China", City = "Shanghai", Population = 27_058_000 },
    new PopulationData { Continent = "Asia", Country = "India", City = "Delhi", Population = 32_941_000 },
    new PopulationData { Continent = "Asia", Country = "India", City = "Mumbai", Population = 20_961_000 },
    new PopulationData { Continent = "Europe", Country = "Germany", City = "Berlin", Population = 3_769_000 },
    // More data...
};
```

**TreeMap Configuration:**
```csharp
treeMap.DataSource = data;
treeMap.PrimaryValuePath = "Population";
treeMap.EnableDrillDown = true;

// Level 1: Continents
treeMap.Levels.Add(new TreeMapLevel { GroupPath = "Continent", HeaderHeight = 30 });

// Level 2: Countries
treeMap.Levels.Add(new TreeMapLevel { GroupPath = "Country", HeaderHeight = 25 });

// Leaf items: Cities
treeMap.LeafItemSettings = new TreeMapLeafItemSettings { LabelPath = "City" };
```

**Interaction:**
1. Initial view: Groups by Continent (Asia, Europe, Africa, etc.)
2. Tap "Asia" → Drill to countries (China, India, Japan, etc.)
3. Tap "China" → Drill to cities (Beijing, Shanghai, Guangzhou, etc.)
4. Tap back → Return to countries, then continents

### Drilldown with Custom Header Style

```xml
<treemap:SfTreeMap EnableDrillDown="True">
    <treemap:SfTreeMap.Levels>
        <treemap:TreeMapLevel GroupPath="Category" 
                              HeaderHeight="35"
                              Background="#2196F3">
            <treemap:TreeMapLevel.HeaderStyle>
                <treemap:TreeMapLabelStyle TextColor="White"
                                           FontSize="14"
                                           FontAttributes="Bold" />
            </treemap:TreeMapLevel.HeaderStyle>
        </treemap:TreeMapLevel>
    </treemap:SfTreeMap.Levels>
</treemap:SfTreeMap>
```

### When to Use Drilldown

- **Large Hierarchical Datasets**: Millions of data points across multiple levels
- **Progressive Disclosure**: Show high-level overview, drill for details
- **Organizational Charts**: Departments → Teams → Individuals
- **Geographic Data**: Continents → Countries → States → Cities
- **File Systems**: Drives → Folders → Subfolders → Files

### Drilldown Best Practices

1. **Clear Headers**: Use descriptive group labels
2. **Visual Feedback**: Ensure headers look interactive (hover effects)
3. **Breadcrumbs**: Show current location in hierarchy
4. **Performance**: Optimize for large datasets with virtualization
5. **Back Navigation**: Provide obvious way to navigate up

## Right-to-Left (RTL) Support

Support RTL languages (Arabic, Hebrew, Persian, Urdu) by mirroring the TreeMap layout.

### Enabling RTL

Use the `FlowDirection` property to set layout direction.

**XAML:**
```xml
<treemap:SfTreeMap DataSource="{Binding Data}"
                   PrimaryValuePath="Value"
                   FlowDirection="RightToLeft">
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Name" />
    </treemap:SfTreeMap.LeafItemSettings>
</treemap:SfTreeMap>
```

**C# Implementation:**
```csharp
treeMap.FlowDirection = FlowDirection.RightToLeft;
```

### RTL Behavior

**Layout Changes:**
- Leaf items arranged right-to-left
- Text alignment flips (right-aligned)
- Legends and headers mirror position
- Navigation order reverses

**Example:**
- LTR Layout: Items flow left → right
- RTL Layout: Items flow right ← left

### Automatic RTL Based on Culture

Set RTL based on current culture:

```csharp
public MainPage()
{
    InitializeComponent();
    
    var currentCulture = CultureInfo.CurrentUICulture;
    var isRtl = currentCulture.TextInfo.IsRightToLeft;
    
    treeMap.FlowDirection = isRtl ? FlowDirection.RightToLeft : FlowDirection.LeftToRight;
}
```

### RTL Best Practices

1. **Test with RTL Languages**: Verify layout with Arabic/Hebrew text
2. **Icon Positioning**: Ensure icons mirror appropriately
3. **Text Alignment**: Check that labels align correctly
4. **Navigation**: Verify keyboard/touch navigation follows RTL order

### Complete RTL Example

```xml
<ContentPage xmlns:treemap="clr-namespace:Syncfusion.Maui.TreeMap;assembly=Syncfusion.Maui.TreeMap">
    <Grid>
        <treemap:SfTreeMap DataSource="{Binding ArabicData}"
                           PrimaryValuePath="Value"
                           FlowDirection="RightToLeft"
                           RangeColorValuePath="Value">
            <treemap:SfTreeMap.LeafItemSettings>
                <treemap:TreeMapLeafItemSettings LabelPath="Name">
                    <treemap:TreeMapLeafItemSettings.LabelStyle>
                        <treemap:TreeMapLabelStyle TextColor="White"
                                                   FontSize="14" />
                    </treemap:TreeMapLeafItemSettings.LabelStyle>
                </treemap:TreeMapLeafItemSettings>
            </treemap:SfTreeMap.LeafItemSettings>
            
            <treemap:SfTreeMap.LegendSettings>
                <treemap:TreeMapLegendSettings ShowLegend="True"
                                               Placement="Right" />
            </treemap:SfTreeMap.LegendSettings>
            
            <treemap:SfTreeMap.LeafItemBrushSettings>
                <treemap:TreeMapRangeBrushSettings>
                    <treemap:TreeMapRangeBrushSettings.RangeBrushes>
                        <treemap:TreeMapRangeBrush LegendLabel="عالي"  <!-- Arabic: High -->
                                                   From="75" To="100" 
                                                   Brush="#4CAF50" />
                        <treemap:TreeMapRangeBrush LegendLabel="منخفض"  <!-- Arabic: Low -->
                                                   From="0" To="75" 
                                                   Brush="#F44336" />
                    </treemap:TreeMapRangeBrushSettings.RangeBrushes>
                </treemap:TreeMapRangeBrushSettings>
            </treemap:SfTreeMap.LeafItemBrushSettings>
        </treemap:SfTreeMap>
    </Grid>
</ContentPage>
```

## Practical Examples

### Example 1: Fully Accessible TreeMap

```xml
<treemap:SfTreeMap DataSource="{Binding AccessibleData}"
                   PrimaryValuePath="Revenue"
                   SelectionMode="Single"
                   SemanticProperties.Description="Revenue visualization by product. Navigate with arrow keys, select with Enter."
                   SemanticProperties.HeadingLevel="Level2">
    <treemap:SfTreeMap.LeafItemSettings>
        <treemap:TreeMapLeafItemSettings LabelPath="Product"
                                         Spacing="4"
                                         Stroke="DarkGray"
                                         StrokeWidth="1">
            <treemap:TreeMapLeafItemSettings.LabelStyle>
                <treemap:TreeMapLabelStyle TextColor="White"
                                           FontSize="14"
                                           FontAttributes="Bold" />
            </treemap:TreeMapLeafItemSettings.LabelStyle>
        </treemap:TreeMapLeafItemSettings>
    </treemap:SfTreeMap.LeafItemSettings>
    
    <!-- High contrast color scheme -->
    <treemap:SfTreeMap.LeafItemBrushSettings>
        <treemap:TreeMapRangeBrushSettings>
            <treemap:TreeMapRangeBrushSettings.RangeBrushes>
                <treemap:TreeMapRangeBrush LegendLabel="Excellent Performance"
                                           From="90" To="100" 
                                           Brush="#2E7D32" />  <!-- Dark green, contrast: 12.6:1 -->
                <treemap:TreeMapRangeBrush LegendLabel="Good Performance"
                                           From="70" To="90" 
                                           Brush="#1976D2" />  <!-- Dark blue, contrast: 8.6:1 -->
                <treemap:TreeMapRangeBrush LegendLabel="Poor Performance"
                                           From="0" To="70" 
                                           Brush="#C62828" />  <!-- Dark red, contrast: 10.7:1 -->
            </treemap:TreeMapRangeBrushSettings.RangeBrushes>
        </treemap:TreeMapRangeBrushSettings>
    </treemap:SfTreeMap.LeafItemBrushSettings>
    
    <treemap:SfTreeMap.LegendSettings>
        <treemap:TreeMapLegendSettings ShowLegend="True"
                                       Placement="Bottom">
            <treemap:TreeMapLegendSettings.TextStyle>
                <treemap:TreeMapLabelStyle TextColor="Black"
                                           FontSize="13"
                                           FontAttributes="Bold" />
            </treemap:TreeMapLegendSettings.TextStyle>
        </treemap:TreeMapLegendSettings>
    </treemap:SfTreeMap.LegendSettings>
</treemap:SfTreeMap>
```

### Example 2: Hierarchical Drilldown TreeMap

```csharp
public class SalesData
{
    public string Region { get; set; }
    public string Country { get; set; }
    public string Product { get; set; }
    public decimal Sales { get; set; }
}

// Setup in code-behind
public void SetupDrilldownTreeMap()
{
    var salesData = GetSalesData();
    
    treeMap.DataSource = salesData;
    treeMap.PrimaryValuePath = "Sales";
    treeMap.EnableDrillDown = true;
    
    // Level 1: Regions
    treeMap.Levels.Add(new TreeMapLevel 
    { 
        GroupPath = "Region",
        HeaderHeight = 35,
        Background = new SolidColorBrush(Color.FromArgb("#2196F3")),
        HeaderStyle = new TreeMapLabelStyle
        {
            TextColor = Colors.White,
            FontSize = 15,
            FontAttributes = FontAttributes.Bold
        }
    });
    
    // Level 2: Countries
    treeMap.Levels.Add(new TreeMapLevel 
    { 
        GroupPath = "Country",
        HeaderHeight = 30,
        Background = new SolidColorBrush(Color.FromArgb("#64B5F6")),
        HeaderStyle = new TreeMapLabelStyle
        {
            TextColor = Colors.White,
            FontSize = 13,
            FontAttributes = FontAttributes.Bold
        }
    });
    
    // Leaf items: Products
    treeMap.LeafItemSettings = new TreeMapLeafItemSettings
    {
        LabelPath = "Product",
        Spacing = 3,
        LabelStyle = new TreeMapLabelStyle
        {
            TextColor = Colors.White,
            FontSize = 12
        }
    };
    
    // Accessibility
    SemanticProperties.SetDescription(treeMap, 
        "Sales data tree map. Tap region headers to drill down to countries, then products. " +
        "Use keyboard arrow keys to navigate.");
}
```

### Example 3: RTL Localized TreeMap

```csharp
public partial class LocalizedTreeMapPage : ContentPage
{
    public LocalizedTreeMapPage()
    {
        InitializeComponent();
        ConfigureTreeMapForLocale();
    }
    
    private void ConfigureTreeMapForLocale()
    {
        var currentCulture = CultureInfo.CurrentUICulture;
        var isRtl = currentCulture.TextInfo.IsRightToLeft;
        
        // Set RTL if needed
        SalesTreeMap.FlowDirection = isRtl 
            ? FlowDirection.RightToLeft 
            : FlowDirection.LeftToRight;
        
        // Set semantic description in current language
        var description = isRtl
            ? "مخطط المبيعات حسب المنتج"  // Arabic: Sales chart by product
            : "Sales chart by product";
        
        SemanticProperties.SetDescription(SalesTreeMap, description);
        
        // Configure legend placement based on direction
        SalesTreeMap.LegendSettings.Placement = isRtl
            ? TreeMapLegendPlacement.Left
            : TreeMapLegendPlacement.Right;
    }
}
```

## Troubleshooting

### Issue 1: Keyboard Navigation Not Working

**Symptoms:** Arrow keys don't navigate TreeMap

**Solutions:**
1. Ensure TreeMap can receive focus (not disabled)
2. Verify TreeMap is in tab order
3. Check that another control isn't capturing keyboard input
4. Test with `SelectionMode` enabled

### Issue 2: Focus Indicator Not Visible

**Symptoms:** Can't see which item has keyboard focus

**Solutions:**
1. Increase `Spacing` between items
2. Use contrasting `Stroke` color
3. Ensure focus indicator is not same color as selection highlight
4. Test on actual device (not just simulator)

### Issue 3: Drilldown Not Navigating

**Symptoms:** Tapping headers doesn't drill down

**Solutions:**
1. Verify `EnableDrillDown="True"` is set
2. Ensure `Levels` are defined with `GroupPath`
3. Check data has hierarchical structure matching GroupPath properties
4. Confirm header areas are tappable (sufficient HeaderHeight)

```xml
<treemap:SfTreeMap EnableDrillDown="True">
    <treemap:SfTreeMap.Levels>
        <treemap:TreeMapLevel GroupPath="Category" HeaderHeight="30" />
    </treemap:SfTreeMap.Levels>
</treemap:SfTreeMap>
```

### Issue 4: RTL Layout Not Mirroring

**Symptoms:** RTL mode doesn't flip layout

**Solutions:**
1. Ensure `FlowDirection="RightToLeft"` is set on TreeMap
2. Check parent containers don't override FlowDirection
3. Verify .NET MAUI version supports RTL (7.0+)
4. Test on actual RTL device/simulator

```csharp
treeMap.FlowDirection = FlowDirection.RightToLeft;
```

### Issue 5: Poor Color Contrast

**Symptoms:** Labels hard to read, accessibility issues

**Solutions:**
1. Test contrast ratio with WebAIM tool (minimum 4.5:1 for text)
2. Use darker background colors with white/light text
3. Use lighter background colors with black/dark text
4. Avoid red/green for critical distinctions (color blindness)

**Good Contrast:**
```csharp
// Dark background with white text = high contrast
Background = "#2C3E50", TextColor = "White"  // Ratio: ~12:1

// Light background with dark text = high contrast
Background = "#ECEFF1", TextColor = "#212121"  // Ratio: ~15:1
```

## Related Topics

- [Selection and Interaction](selection-interaction.md) - SelectionChanged event details
- [Getting Started](getting-started.md) - Basic TreeMap setup
- [Hierarchical Levels](hierarchical-levels.md) - Multi-level data for drilldown

## Summary

- **Accessibility**: Provide keyboard navigation, screen reader support, and WCAG compliance
- **Keyboard**: Tab to focus, arrow keys to navigate, Enter/Space to select
- **SelectionChanged Event**: Handle selection changes for custom logic
- **Drilldown**: Enable with `EnableDrillDown="True"` for hierarchical navigation
- **Hierarchical Data**: Define `Levels` with `GroupPath` for multi-level drilldown
- **RTL Support**: Use `FlowDirection="RightToLeft"` for RTL languages
- **Best Practices**: Test with screen readers, keyboard only, high contrast, and RTL locales
- Color contrast, semantic descriptions, and keyboard support ensure inclusive design
- Drilldown provides progressive disclosure for large hierarchical datasets
