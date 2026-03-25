# Appearance Customization

The appearance of the Syncfusion .NET MAUI Sunburst Chart can be extensively customized using properties like angle, radius, inner radius, and stroke to enhance visual presentation and adapt to different design requirements.

## Angle Customization

### Start and End Angles

Control the arc span of the sunburst chart by adjusting the start and end angles. This allows creating partial circles, semi-circles, or custom angular ranges.

**Properties:**
- **StartAngle**: Starting angle in degrees (0-360)
- **EndAngle**: Ending angle in degrees (0-360)
- **Default**: StartAngle = 0, EndAngle = 360 (full circle)

**XAML:**
```xml
<sunburst:SfSunburstChart ItemsSource="{Binding DataSource}" 
                          StartAngle="180"
                          EndAngle="360"
                          ValueMemberPath="EmployeesCount">
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Country"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Department"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
SfSunburstChart sunburst = new SfSunburstChart();
sunburst.ItemsSource = (new SunburstViewModel()).DataSource;
sunburst.StartAngle = 180;
sunburst.EndAngle = 360;
sunburst.ValueMemberPath = "EmployeesCount";
// ... configure levels
this.Content = sunburst;
```

### Common Angle Configurations

**Semi-circle (Bottom Half):**
```xml
<sunburst:SfSunburstChart StartAngle="180" EndAngle="360">
```

**Semi-circle (Top Half):**
```xml
<sunburst:SfSunburstChart StartAngle="0" EndAngle="180">
```

**Three-quarter Circle:**
```xml
<sunburst:SfSunburstChart StartAngle="0" EndAngle="270">
```

**Quarter Circle:**
```xml
<sunburst:SfSunburstChart StartAngle="0" EndAngle="90">
```

**Right Semi-circle:**
```xml
<sunburst:SfSunburstChart StartAngle="270" EndAngle="90">
```

**Use cases:**
- Semi-circles for dashboard panels where space is limited
- Quarter circles for corner widgets
- Custom ranges for thematic designs (e.g., gauge-like visualizations)

## Radius Customization

### Outer Radius

Control the overall size of the sunburst chart with the Radius property.

**Property:**
- **Radius**: Outer radius relative to available space
- **Range**: 0 to 1 (0 = no chart, 1 = full available space)
- **Default**: 0.9

**XAML:**
```xml
<sunburst:SfSunburstChart ItemsSource="{Binding DataSource}"
                          Radius="0.7"
                          ValueMemberPath="EmployeesCount">
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Region"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Product"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
SfSunburstChart sunburst = new SfSunburstChart();
sunburst.ItemsSource = (new SunburstViewModel()).DataSource;
sunburst.Radius = 0.7;
sunburst.ValueMemberPath = "EmployeesCount";
// ... configure levels
this.Content = sunburst;
```

**Typical radius values:**
- **0.9**: Default, fills most of available space with padding
- **0.7-0.8**: Moderate size, good for charts with legends
- **0.5-0.6**: Smaller chart, good for inline displays
- **1.0**: Maximum size, fills entire available space

**Use cases:**
- Smaller radius (0.5-0.7) when you have multiple charts side-by-side
- Larger radius (0.9-1.0) for primary/featured visualizations
- Medium radius (0.7-0.8) when displaying with legends or annotations

## Inner Radius Customization

### Center Hole Size

Create a donut-style sunburst chart by adjusting the inner radius. This creates a hollow center that can display custom content.

**Property:**
- **InnerRadius**: Inner radius relative to outer radius
- **Range**: 0 to 1 (0 = no hole, 1 = all hole)
- **Default**: 0.4 (approximately, creates a visible center)

**XAML:**
```xml
<sunburst:SfSunburstChart ItemsSource="{Binding DataSource}"
                          InnerRadius="0.4"
                          ValueMemberPath="EmployeesCount">
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Category"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Subcategory"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
SfSunburstChart sunburst = new SfSunburstChart();
sunburst.ItemsSource = (new SunburstViewModel()).DataSource;
sunburst.InnerRadius = 0.4;
sunburst.ValueMemberPath = "EmployeesCount";
// ... configure levels
this.Content = sunburst;
```

### Inner Radius Styles

**Small Center Hole (0.2-0.3):**
```xml
<sunburst:SfSunburstChart InnerRadius="0.25">
<!-- Minimal center, emphasizes hierarchical rings -->
```

**Medium Center Hole (0.4-0.5):**
```xml
<sunburst:SfSunburstChart InnerRadius="0.45">
<!-- Balanced donut style, room for center content -->
```

**Large Center Hole (0.6-0.7):**
```xml
<sunburst:SfSunburstChart InnerRadius="0.65">
<!-- Prominent center, thin rings -->
```

**No Center Hole (0):**
```xml
<sunburst:SfSunburstChart InnerRadius="0">
<!-- Full pie style, no hollow center -->
```

**Use cases:**
- Large inner radius (0.6+) when displaying important summary data in the center
- Medium inner radius (0.4-0.5) for balanced hierarchy visualization
- Small inner radius (0.2-0.3) to maximize data display space
- Zero inner radius for traditional pie-like appearance

## Stroke Customization

### Segment Borders

Add visual separation between segments using stroke color and width.

**Properties:**
- **Stroke**: Border color for segments
- **StrokeWidth**: Border thickness in device-independent units
- **Default**: Stroke = null (no border), StrokeWidth = 0

**XAML:**
```xml
<sunburst:SfSunburstChart ItemsSource="{Binding DataSource}" 
                          Stroke="Black"
                          StrokeWidth="2"
                          ValueMemberPath="EmployeesCount">
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Region"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Country"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
SfSunburstChart sunburst = new SfSunburstChart();
sunburst.ItemsSource = (new SunburstViewModel()).DataSource;
sunburst.Stroke = Colors.Black;
sunburst.StrokeWidth = 2;
sunburst.ValueMemberPath = "EmployeesCount";
// ... configure levels
this.Content = sunburst;
```

### Stroke Styling Options

**Subtle Separation:**
```xml
<sunburst:SfSunburstChart Stroke="LightGray" StrokeWidth="1">
<!-- Gentle borders for clean look -->
```

**Bold Separation:**
```xml
<sunburst:SfSunburstChart Stroke="Black" StrokeWidth="3">
<!-- Strong borders for high contrast -->
```

**White Borders (Dark Background):**
```xml
<sunburst:SfSunburstChart Stroke="White" StrokeWidth="2">
<!-- Crisp separation on dark themes -->
```

**Transparent (No Borders):**
```xml
<sunburst:SfSunburstChart Stroke="Transparent" StrokeWidth="0">
<!-- Seamless segments without borders -->
```

**Use cases:**
- Thick strokes (3-4) for presentations and large displays
- Medium strokes (2) for standard desktop/tablet viewing
- Thin strokes (1) for mobile or dense visualizations
- No stroke for seamless, flowing appearance

## Combined Appearance Examples

### Example 1: Dashboard Semi-Circle Widget

```xml
<sunburst:SfSunburstChart ItemsSource="{Binding DataSource}"
                          StartAngle="180" EndAngle="360"
                          Radius="0.8"
                          InnerRadius="0.5"
                          Stroke="White"
                          StrokeWidth="2"
                          ValueMemberPath="Value">
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Category"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Subcategory"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```
Creates a bottom semi-circle with large center hole and white borders.

### Example 2: Compact Inline Visualization

```xml
<sunburst:SfSunburstChart ItemsSource="{Binding DataSource}"
                          Radius="0.6"
                          InnerRadius="0.3"
                          Stroke="LightGray"
                          StrokeWidth="1"
                          ValueMemberPath="Count">
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Type"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Subtype"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```
Small, subtle chart suitable for embedding in content.

### Example 3: Full-Featured Display

```xml
<sunburst:SfSunburstChart ItemsSource="{Binding DataSource}"
                          StartAngle="0" EndAngle="360"
                          Radius="0.9"
                          InnerRadius="0.45"
                          Stroke="DarkGray"
                          StrokeWidth="2"
                          ValueMemberPath="Amount">
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Division"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Department"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Team"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```
Full circle with prominent display and clear segment separation.

### Example 4: Minimal Clean Style

```xml
<sunburst:SfSunburstChart ItemsSource="{Binding DataSource}"
                          Radius="0.85"
                          InnerRadius="0.4"
                          Stroke="Transparent"
                          StrokeWidth="0"
                          ValueMemberPath="Value">
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Category"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```
Clean, borderless design with smooth color transitions.

## Design Best Practices

### Choosing Radius Values

**Consider your layout:**
- Charts with legends: Use Radius 0.7-0.8 to leave space
- Standalone charts: Use Radius 0.9-1.0 for maximum impact
- Multiple charts: Use Radius 0.5-0.6 for uniform sizing

### Choosing Inner Radius Values

**Based on content and purpose:**
- Displaying center content: Use InnerRadius 0.5-0.7 for adequate space
- Emphasizing hierarchy: Use InnerRadius 0.3-0.4 for balanced rings
- Maximizing data: Use InnerRadius 0.2-0.3 for thicker segments

### Stroke Considerations

**Visual clarity:**
- Use strokes when segments have similar colors
- Omit strokes for clean, modern appearance
- Match stroke color to background for subtle effect
- Use contrasting stroke for high definition

### Angle Considerations

**Layout integration:**
- Full circles (0-360°) for standalone visualizations
- Semi-circles for dashboard panels or headers
- Quarter circles for corner widgets or compact views
- Custom ranges for creative layouts

## Performance Considerations

- Smaller radius values render faster (less pixels to draw)
- Strokes add rendering overhead, especially at high widths
- Full circles (360°) are optimized compared to partial arcs
- Inner radius doesn't significantly impact performance

## Common Patterns

### Pattern: Emphasize Center Content
```xml
<sunburst:SfSunburstChart InnerRadius="0.65" Radius="0.85">
```

### Pattern: Maximize Data Display
```xml
<sunburst:SfSunburstChart InnerRadius="0.25" Radius="0.95">
```

### Pattern: Clean Modern Look
```xml
<sunburst:SfSunburstChart Stroke="Transparent" StrokeWidth="0">
```

### Pattern: High Definition Separation
```xml
<sunburst:SfSunburstChart Stroke="White" StrokeWidth="3">
```

### Pattern: Compact Dashboard Widget
```xml
<sunburst:SfSunburstChart StartAngle="180" EndAngle="360" Radius="0.7" InnerRadius="0.5">
```

## Troubleshooting

**Issue:** Chart appears too small
- **Solution:** Increase Radius property (try 0.9 or 1.0)

**Issue:** Center hole too large, segments barely visible
- **Solution:** Decrease InnerRadius (try 0.3-0.4)

**Issue:** Segments blend together
- **Solution:** Add stroke with contrasting color (e.g., White or Black)

**Issue:** Strokes appear pixelated or jagged
- **Solution:** Reduce StrokeWidth or use smoother colors

**Issue:** Partial circle doesn't display correctly
- **Solution:** Ensure EndAngle > StartAngle, and both are within 0-360 range
