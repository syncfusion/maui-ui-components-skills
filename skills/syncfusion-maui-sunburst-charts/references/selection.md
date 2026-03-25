# Selection in .NET MAUI Sunburst Chart

## Table of Contents
- [Overview](#overview)
- [Selection Types](#selection-types)
- [Display Modes](#display-modes)
- [Selection Events](#selection-events)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

The Syncfusion .NET MAUI Sunburst Chart supports segment selection with visual highlighting, enabling users to interact with hierarchical data by tapping segments. Selection can highlight individual segments, their children, parents, or entire groups, with various display modes for visual feedback.

**Key features:**
- Multiple selection types (Single, Child, Parent, Group)
- Three display modes (Brush, Opacity, Stroke)
- Selection events for programmatic control
- Customizable highlighting styles

## Selection Types

The `Type` property of `SunburstSelectionSettings` determines which segments are highlighted when a user taps.

### Available Types

**Single:**
- Highlights only the selected segment
- Default selection type
- Use for independent segment selection

**Child:**
- Highlights the selected segment and all its children in all levels
- Useful for exploring subcategories

**Parent:**
- Highlights the selected segment and its parent up to the root
- Shows the path from selection to root

**Group:**
- Highlights all segments at the same hierarchical level as selected segment
- Useful for comparing peers

### Single Selection

Highlights only the tapped segment.

**XAML:**
```xml
<sunburst:SfSunburstChart ItemsSource="{Binding DataSource}"
                          ValueMemberPath="Value">
    
    <sunburst:SfSunburstChart.SelectionSettings>
        <sunburst:SunburstSelectionSettings Type="Single"/>
    </sunburst:SfSunburstChart.SelectionSettings>
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Region"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Country"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="City"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
SfSunburstChart sunburst = new SfSunburstChart();
SunburstSelectionSettings selectionSettings = new SunburstSelectionSettings
{
    Type = SunburstSelectionType.Single
};
sunburst.SelectionSettings = selectionSettings;
// ... configure data and levels
this.Content = sunburst;
```

**Use cases:**
- Showing detailed information for one segment
- Comparing individual segments
- When users need to focus on specific data points

### Child Selection

Highlights the selected segment and all its descendants.

**XAML:**
```xml
<sunburst:SfSunburstChart ItemsSource="{Binding DataSource}"
                          ValueMemberPath="Sales">
    
    <sunburst:SfSunburstChart.SelectionSettings>
        <sunburst:SunburstSelectionSettings Type="Child"/>
    </sunburst:SfSunburstChart.SelectionSettings>
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Company"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Division"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Department"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
SunburstSelectionSettings selectionSettings = new SunburstSelectionSettings
{
    Type = SunburstSelectionType.Child
};
sunburst.SelectionSettings = selectionSettings;
```

**Use cases:**
- Exploring hierarchical breakdowns
- Showing all subcategories of a selection
- Understanding composition of a segment
- Budget or sales drilldown analysis

### Group Selection

Highlights all segments at the same level as the selected segment.

**XAML:**
```xml
<sunburst:SfSunburstChart ItemsSource="{Binding DataSource}"
                          ValueMemberPath="Count">
    
    <sunburst:SfSunburstChart.SelectionSettings>
        <sunburst:SunburstSelectionSettings Type="Group"/>
    </sunburst:SfSunburstChart.SelectionSettings>
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Category"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Subcategory"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Item"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
SunburstSelectionSettings selectionSettings = new SunburstSelectionSettings
{
    Type = SunburstSelectionType.Group
};
sunburst.SelectionSettings = selectionSettings;
```

**Use cases:**
- Comparing peer segments
- Showing all items at same hierarchy level
- Highlighting competitive analysis
- Department or team comparisons

### Parent Selection

Highlights the selected segment and its ancestors up to the root.

**XAML:**
```xml
<sunburst:SfSunburstChart ItemsSource="{Binding DataSource}"
                          ValueMemberPath="Amount">
    
    <sunburst:SfSunburstChart.SelectionSettings>
        <sunburst:SunburstSelectionSettings Type="Parent"/>
    </sunburst:SfSunburstChart.SelectionSettings>
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Organization"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Department"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Team"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Employee"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
SunburstSelectionSettings selectionSettings = new SunburstSelectionSettings
{
    Type = SunburstSelectionType.Parent
};
sunburst.SelectionSettings = selectionSettings;
```

**Use cases:**
- Showing hierarchical path
- Understanding segment context
- Tracing organizational structure
- File system path visualization

## Display Modes

The `DisplayMode` property controls how selected segments are visually highlighted.

### Available Display Modes

**HighlightByBrush:**
- Changes segment color using Fill property
- Most noticeable highlighting method
- Default display mode

**HighlightByOpacity:**
- Reduces opacity of unselected segments
- Selected segments remain at full opacity
- Subtle, elegant effect

**HighlightByStroke:**
- Adds border to selected segments
- Customizable stroke color and width
- Good for maintaining original colors

### Highlight by Brush

Changes selected segment color to the specified Fill color.

**XAML:**
```xml
<sunburst:SfSunburstChart ItemsSource="{Binding DataSource}"
                          ValueMemberPath="Value">
    
    <sunburst:SfSunburstChart.SelectionSettings>
        <sunburst:SunburstSelectionSettings 
            Fill="DarkRed" 
            DisplayMode="HighlightByBrush" 
            Type="Child"/>
    </sunburst:SfSunburstChart.SelectionSettings>
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Category"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Product"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
SunburstSelectionSettings selectionSettings = new SunburstSelectionSettings
{
    Fill = Colors.DarkRed,
    DisplayMode = SunburstSelectionDisplayMode.HighlightByBrush,
    Type = SunburstSelectionType.Child
};
sunburst.SelectionSettings = selectionSettings;
```

**Fill color suggestions:**
- **DarkRed, DarkBlue**: High contrast, attention-grabbing
- **Gold, Orange**: Warm, emphasis colors
- **Purple, DeepPink**: Distinct from typical data colors
- Match your app's accent color for consistency

**Use cases:**
- When strong visual emphasis is needed
- For presentations or dashboards
- When original colors are less important
- Highlighting critical data

### Highlight by Opacity

Dims unselected segments while keeping selected segments at full opacity.

**XAML:**
```xml
<sunburst:SfSunburstChart ItemsSource="{Binding DataSource}"
                          ValueMemberPath="Sales">
    
    <sunburst:SfSunburstChart.SelectionSettings>
        <sunburst:SunburstSelectionSettings 
            Opacity="0.3" 
            DisplayMode="HighlightByOpacity" 
            Type="Child"/>
    </sunburst:SfSunburstChart.SelectionSettings>
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Region"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Country"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
SunburstSelectionSettings selectionSettings = new SunburstSelectionSettings
{
    Opacity = 0.3,
    DisplayMode = SunburstSelectionDisplayMode.HighlightByOpacity,
    Type = SunburstSelectionType.Child
};
sunburst.SelectionSettings = selectionSettings;
```

**Opacity values:**
- **0.2-0.3**: Strong dimming, clear focus
- **0.4-0.5**: Moderate dimming, balanced
- **0.6-0.7**: Subtle dimming, context preserved

**Use cases:**
- Maintaining original color scheme
- Elegant, professional presentations
- When context needs to remain visible
- Clean, modern UI designs

### Highlight by Stroke

Adds a border around selected segments.

**XAML:**
```xml
<sunburst:SfSunburstChart ItemsSource="{Binding DataSource}"
                          ValueMemberPath="Count">
    
    <sunburst:SfSunburstChart.SelectionSettings>
        <sunburst:SunburstSelectionSettings 
            Stroke="Black" 
            StrokeWidth="3" 
            DisplayMode="HighlightByStroke" 
            Type="Child"/>
    </sunburst:SfSunburstChart.SelectionSettings>
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Department"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Team"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
SunburstSelectionSettings selectionSettings = new SunburstSelectionSettings
{
    Stroke = Colors.Black,
    StrokeWidth = 3,
    DisplayMode = SunburstSelectionDisplayMode.HighlightByStroke,
    Type = SunburstSelectionType.Child
};
sunburst.SelectionSettings = selectionSettings;
```

**Stroke styling:**
- **Black/White**: High contrast, works with most colors
- **Accent color**: Matches app theme
- **Width 2-3**: Standard, clear definition
- **Width 4-5**: Bold, for large displays

**Use cases:**
- Preserving all segment colors
- Clean, outlined appearance
- When dimming isn't appropriate
- High-definition displays

## Selection Events

Two events provide programmatic control over selection behavior.

### SelectionChanging Event

Fired before a segment is selected, allowing cancellation.

**Event properties (SunburstSelectionChangingEventArgs):**
- **NewSegment**: Segment about to be selected
- **OldSegment**: Previously selected segment (if any)
- **Cancel**: Set to true to prevent selection

**XAML:**
```xml
<sunburst:SfSunburstChart SelectionChanging="OnSelectionChanging">
    <sunburst:SfSunburstChart.SelectionSettings>
        <sunburst:SunburstSelectionSettings Type="Single"/>
    </sunburst:SfSunburstChart.SelectionSettings>
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
private void OnSelectionChanging(object sender, SunburstSelectionChangingEventArgs e)
{
    // Prevent selection of specific segments
    if (e.NewSegment.Category == "Restricted")
    {
        e.Cancel = true;
        DisplayAlert("Selection", "This segment cannot be selected", "OK");
    }
    
    // Log selection attempts
    Debug.WriteLine($"Attempting to select: {e.NewSegment.Category}");
}
```

**Use cases:**
- Preventing selection of certain segments
- Validating user selections
- Implementing selection rules
- Logging user interactions

### SelectionChanged Event

Fired after a segment is selected or deselected.

**Event properties (SunburstSelectionChangedEventArgs):**
- **IsSelected**: True if segment was selected, false if deselected
- **NewSegment**: Currently selected segment
- **OldSegment**: Previously selected segment

**XAML:**
```xml
<sunburst:SfSunburstChart SelectionChanged="OnSelectionChanged">
    <sunburst:SfSunburstChart.SelectionSettings>
        <sunburst:SunburstSelectionSettings Type="Child"/>
    </sunburst:SfSunburstChart.SelectionSettings>
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
private void OnSelectionChanged(object sender, SunburstSelectionChangedEventArgs e)
{
    if (e.IsSelected)
    {
        // Update UI with selected segment details
        SelectedCategory.Text = e.NewSegment.Category;
        SelectedValue.Text = e.NewSegment.Value.ToString();
        
        // Load additional data
        LoadDetailsFor(e.NewSegment.Category);
    }
    else
    {
        // Clear selection display
        SelectedCategory.Text = "None";
        SelectedValue.Text = "";
    }
}
```

**Use cases:**
- Updating related UI elements
- Loading detailed data for selection
- Syncing with other visualizations
- Analytics and tracking

## Complete Examples

### Example 1: Comprehensive Selection Configuration

```xml
<sunburst:SfSunburstChart ItemsSource="{Binding EmployeeData}"
                          ValueMemberPath="Count"
                          SelectionChanging="OnSelectionChanging"
                          SelectionChanged="OnSelectionChanged">
    
    <sunburst:SfSunburstChart.SelectionSettings>
        <sunburst:SunburstSelectionSettings 
            Type="Child"
            DisplayMode="HighlightByBrush"
            Fill="#FF5722"/>
    </sunburst:SfSunburstChart.SelectionSettings>
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Department"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Team"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Role"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

### Example 2: Elegant Opacity-Based Selection

```xml
<sunburst:SfSunburstChart ItemsSource="{Binding SalesData}"
                          ValueMemberPath="Revenue"
                          ShowLabels="True"
                          EnableTooltip="True">
    
    <sunburst:SfSunburstChart.SelectionSettings>
        <sunburst:SunburstSelectionSettings 
            Type="Child"
            DisplayMode="HighlightByOpacity"
            Opacity="0.25"/>
    </sunburst:SfSunburstChart.SelectionSettings>
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Product"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Category"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Model"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

### Example 3: Stroke-Based Group Selection

```xml
<sunburst:SfSunburstChart ItemsSource="{Binding BudgetData}"
                          ValueMemberPath="Amount"
                          Radius="0.85"
                          InnerRadius="0.4">
    
    <sunburst:SfSunburstChart.SelectionSettings>
        <sunburst:SunburstSelectionSettings 
            Type="Group"
            DisplayMode="HighlightByStroke"
            Stroke="#0078D4"
            StrokeWidth="4"/>
    </sunburst:SfSunburstChart.SelectionSettings>
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Division"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Category"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

### Example 4: Parent Path Highlighting

```xml
<sunburst:SfSunburstChart ItemsSource="{Binding FileSystemData}"
                          ValueMemberPath="Size"
                          SelectionChanged="ShowFilePath">
    
    <sunburst:SfSunburstChart.SelectionSettings>
        <sunburst:SunburstSelectionSettings 
            Type="Parent"
            DisplayMode="HighlightByBrush"
            Fill="Gold"/>
    </sunburst:SfSunburstChart.SelectionSettings>
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Drive"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Folder"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Subfolder"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="File"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

## Best Practices

### Choosing Selection Type

**Use Single when:**
- Users need to select one specific segment
- Showing detailed information for individual items
- Comparing specific data points
- Simplicity is important

**Use Child when:**
- Exploring hierarchical breakdowns
- Understanding composition
- Drill-down analysis
- Budget or inventory management

**Use Group when:**
- Comparing peers
- Highlighting same-level items
- Competitive analysis
- Department comparisons

**Use Parent when:**
- Showing hierarchical paths
- Understanding context
- Organizational structure
- File/folder navigation

### Choosing Display Mode

**HighlightByBrush:**
- Strong visual impact needed
- Presentations and dashboards
- Original colors less critical
- Maximum attention

**HighlightByOpacity:**
- Professional, elegant appearance
- Context must remain visible
- Subtle emphasis
- Modern UI design

**HighlightByStroke:**
- Preserving all colors important
- Clean, outlined style
- High-definition displays
- When dimming not suitable

### Color Selection

**For HighlightByBrush:**
- Use accent colors from your app theme
- Ensure contrast with segment colors
- Test with colorblind simulation tools
- Consider cultural color meanings

**For HighlightByStroke:**
- Black or White for high contrast
- App accent color for consistency
- Thickness 3-4 for clear visibility
- Test on various backgrounds

### Event Handling

**Performance:**
- Keep event handlers lightweight
- Avoid heavy computations in SelectionChanging
- Use async/await for data loading
- Cache frequently accessed data

**User experience:**
- Provide immediate visual feedback
- Show loading indicators for slow operations
- Handle errors gracefully
- Allow users to cancel long operations

## Troubleshooting

**Issue:** Selection not working
- **Solution:** Verify SelectionSettings is configured
- Ensure segments are large enough to tap
- Check touch/click events aren't blocked
- Test with simple selection first

**Issue:** Wrong segments highlighted
- **Solution:** Verify Type property is set correctly
- Check hierarchical level configuration
- Ensure data relationships are correct
- Test with smaller dataset

**Issue:** Selection not visible
- **Solution:** Increase contrast (brighter Fill color or lower Opacity)
- Use HighlightByStroke with wider stroke
- Test DisplayMode options
- Check segment colors aren't too similar

**Issue:** SelectionChanging event not firing
- **Solution:** Verify event handler is attached
- Check event name spelling
- Ensure chart is interactive (not disabled)
- Test with simple handler first

**Issue:** Performance issues with selection
- **Solution:** Reduce number of segments
- Simplify display mode (Opacity is lightest)
- Disable animations if needed
- Test on actual hardware

**Issue:** Selection persists incorrectly
- **Solution:** This is expected behavior
- Handle deselection in code if needed
- Use SelectionChanged event to manage state
- Consider adding clear selection button
