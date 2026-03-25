# Drill-Down in .NET MAUI Sunburst Chart

## Table of Contents
- [Overview](#overview)
- [Enabling Drill-Down](#enabling-drill-down)
- [Toolbar Alignment](#toolbar-alignment)
- [Toolbar Positioning](#toolbar-positioning)
- [Toolbar Customization](#toolbar-customization)
- [Drill-Down Behavior](#drill-down-behavior)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

Drill-down provides interactive hierarchical exploration of large datasets by allowing users to focus on specific branches of the hierarchy. When enabled, users can double-tap a segment to "drill into" it, making it the new root and displaying its children with smooth animations. A toolbar appears for navigation back to parent levels or resetting to the original view.

**Key features:**
- Double-tap interaction to drill into segments
- Smooth animated transitions
- Navigation toolbar with zoom-back and reset buttons
- Hierarchical bread crumb navigation
- Maintains context during exploration

## Enabling Drill-Down

Enable drill-down functionality using the `EnableDrillDown` property.

**Property:**
- **EnableDrillDown**: Enables/disables drill-down feature
- **Default**: `False`

**XAML:**
```xml
<sunburst:SfSunburstChart EnableDrillDown="True"
                          ItemsSource="{Binding DataSource}"
                          ValueMemberPath="EmployeesCount">
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Country"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Department"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Team"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
SfSunburstChart sunburst = new SfSunburstChart();
sunburst.EnableDrillDown = true;
sunburst.ItemsSource = (new SunburstViewModel()).DataSource;
sunburst.ValueMemberPath = "EmployeesCount";
// ... configure levels
this.Content = sunburst;
```

**Interaction:**
- **Double-tap** a segment to drill into it
- The selected segment becomes the new center/root
- Only its children are displayed
- Toolbar appears with navigation controls

## Toolbar Alignment

Control the vertical and horizontal alignment of the drill-down toolbar within the chart area.

### Properties

**VerticalAlignment:**
- **Start**: Top of chart plot area
- **Center**: Middle of chart plot area (default)
- **End**: Bottom of chart plot area

**HorizontalAlignment:**
- **Start**: Left of chart plot area
- **Center**: Middle of chart plot area (default)
- **End**: Right of chart plot area

### Alignment Example

**XAML:**
```xml
<sunburst:SfSunburstChart EnableDrillDown="True"
                          ItemsSource="{Binding DataSource}"
                          ValueMemberPath="Value">
    
    <sunburst:SfSunburstChart.ToolbarSettings>
        <sunburst:SunburstToolbarSettings HorizontalAlignment="End" 
                                         VerticalAlignment="End"/>
    </sunburst:SfSunburstChart.ToolbarSettings>
    
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
sunburst.EnableDrillDown = true;

SunburstToolbarSettings toolbarSettings = new SunburstToolbarSettings()
{
    HorizontalAlignment = SunburstToolbarAlignment.End,
    VerticalAlignment = SunburstToolbarAlignment.End
};
sunburst.ToolbarSettings = toolbarSettings;

// ... configure data and levels
this.Content = sunburst;
```

### Common Alignment Combinations

**Top-Left:**
```xml
<sunburst:SunburstToolbarSettings 
    HorizontalAlignment="Start" 
    VerticalAlignment="Start"/>
```
Good for: Left-to-right reading patterns, matching typical app header positions.

**Top-Right:**
```xml
<sunburst:SunburstToolbarSettings 
    HorizontalAlignment="End" 
    VerticalAlignment="Start"/>
```
Good for: Control panels, settings-style interfaces, avoiding center overlap.

**Bottom-Right:**
```xml
<sunburst:SunburstToolbarSettings 
    HorizontalAlignment="End" 
    VerticalAlignment="End"/>
```
Good for: Floating action button style, modern UI patterns, mobile interfaces.

**Bottom-Center:**
```xml
<sunburst:SunburstToolbarSettings 
    HorizontalAlignment="Center" 
    VerticalAlignment="End"/>
```
Good for: Balanced appearance, tab-bar style navigation, desktop applications.

**Center-Center (Default):**
```xml
<sunburst:SunburstToolbarSettings 
    HorizontalAlignment="Center" 
    VerticalAlignment="Center"/>
```
Good for: Prominent visibility, quick access, when chart has large inner radius.

## Toolbar Positioning

Fine-tune toolbar position using offset properties for pixel-perfect placement.

### Properties

**OffsetX:**
- Horizontal offset in device-independent units
- Positive values move right
- Negative values move left

**OffsetY:**
- Vertical offset in device-independent units
- Positive values move down
- Negative values move up

### Positioning Example

**XAML:**
```xml
<sunburst:SfSunburstChart EnableDrillDown="True"
                          ItemsSource="{Binding DataSource}"
                          ValueMemberPath="Sales">
    
    <sunburst:SfSunburstChart.ToolbarSettings>
        <sunburst:SunburstToolbarSettings 
            HorizontalAlignment="End"
            VerticalAlignment="Start"
            OffsetX="-20" 
            OffsetY="20"/>
    </sunburst:SfSunburstChart.ToolbarSettings>
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Category"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Product"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
SfSunburstChart sunburst = new SfSunburstChart();
sunburst.EnableDrillDown = true;

SunburstToolbarSettings toolbarSettings = new SunburstToolbarSettings()
{
    HorizontalAlignment = SunburstToolbarAlignment.End,
    VerticalAlignment = SunburstToolbarAlignment.Start,
    OffsetX = -20,
    OffsetY = 20
};
sunburst.ToolbarSettings = toolbarSettings;

// ... configure data and levels
this.Content = sunburst;
```

### Offset Use Cases

**Avoid Center Overlap:**
```xml
<sunburst:SunburstToolbarSettings 
    HorizontalAlignment="Center"
    VerticalAlignment="Center"
    OffsetY="100"/>
<!-- Moves toolbar below center to avoid chart content -->
```

**Align with Container Padding:**
```xml
<sunburst:SunburstToolbarSettings 
    HorizontalAlignment="Start"
    VerticalAlignment="Start"
    OffsetX="10"
    OffsetY="10"/>
<!-- Provides 10-unit padding from container edge -->
```

**Position Relative to Legend:**
```xml
<sunburst:SunburstToolbarSettings 
    HorizontalAlignment="End"
    VerticalAlignment="End"
    OffsetX="-10"
    OffsetY="-80"/>
<!-- Positions above legend positioned at bottom -->
```

## Toolbar Customization

Customize the appearance of the drill-down toolbar to match your application theme.

### Properties

**IconBrush:**
- Color of toolbar icons (back arrow, reset)
- Type: Brush (Color)

**Background:**
- Background color of toolbar
- Type: Brush

### Customization Example

**XAML:**
```xml
<sunburst:SfSunburstChart EnableDrillDown="True"
                          ItemsSource="{Binding DataSource}"
                          ValueMemberPath="Count">
    
    <sunburst:SfSunburstChart.ToolbarSettings>
        <sunburst:SunburstToolbarSettings 
            IconBrush="White" 
            Background="#2989F9"
            HorizontalAlignment="End"
            VerticalAlignment="Start"/>
    </sunburst:SfSunburstChart.ToolbarSettings>
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Division"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Department"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Team"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
SfSunburstChart sunburst = new SfSunburstChart();
sunburst.EnableDrillDown = true;

SunburstToolbarSettings toolbarSettings = new SunburstToolbarSettings()
{
    IconBrush = Colors.White,
    Background = new SolidColorBrush(Color.FromArgb("#2989F9")),
    HorizontalAlignment = SunburstToolbarAlignment.End,
    VerticalAlignment = SunburstToolbarAlignment.Start
};
sunburst.ToolbarSettings = toolbarSettings;

// ... configure data and levels
this.Content = sunburst;
```

### Style Variations

**Light Theme:**
```xml
<sunburst:SunburstToolbarSettings 
    IconBrush="Black" 
    Background="White"/>
```

**Dark Theme:**
```xml
<sunburst:SunburstToolbarSettings 
    IconBrush="White" 
    Background="#1E1E1E"/>
```

**Accent Color:**
```xml
<sunburst:SunburstToolbarSettings 
    IconBrush="White" 
    Background="{StaticResource PrimaryColor}"/>
```

**Semi-Transparent:**
```xml
<sunburst:SunburstToolbarSettings 
    IconBrush="White" 
    Background="#CC000000"/>
<!-- 80% opacity black background -->
```

**Gradient Background (C#):**
```csharp
var gradientBrush = new LinearGradientBrush
{
    StartPoint = new Point(0, 0),
    EndPoint = new Point(1, 1),
    GradientStops = new GradientStopCollection
    {
        new GradientStop { Color = Color.FromArgb("#2196F3"), Offset = 0.0f },
        new GradientStop { Color = Color.FromArgb("#21CBF3"), Offset = 1.0f }
    }
};

toolbarSettings.Background = gradientBrush;
```

## Drill-Down Behavior

### User Interaction Flow

1. **Initial View**: Full hierarchy displayed
2. **Double-tap segment**: Selected segment zooms to fill chart
3. **Toolbar appears**: Back and reset buttons shown
4. **Explore children**: Double-tap child segments to drill deeper
5. **Navigate back**: Tap back button to zoom out one level
6. **Reset**: Tap reset button to return to initial view

### Animation

Drill-down operations include smooth animated transitions:
- Zoom animation when drilling in
- Segments expand/contract smoothly
- Labels fade in/out appropriately
- Toolbar slides into view

### Toolbar Buttons

**Back Button:**
- Left arrow icon
- Returns to parent level
- Disabled at root level
- Smooth zoom-out animation

**Reset Button:**
- Home/reset icon
- Returns to initial root view
- Always enabled when drilled down
- Animates back to start

## Complete Examples

### Example 1: Corporate Dashboard Style

```xml
<sunburst:SfSunburstChart EnableDrillDown="True"
                          ItemsSource="{Binding EmployeeData}"
                          ValueMemberPath="Count"
                          Radius="0.85"
                          InnerRadius="0.5"
                          ShowLabels="True"
                          EnableTooltip="True">

    <sunburst:SfSunburstChart.Title>
        <Label Text="Organization Hierarchy" FontSize="20" FontAttributes="Bold"/>
    </sunburst:SfSunburstChart.Title>

    <sunburst:SfSunburstChart.ToolbarSettings>
        <sunburst:SunburstToolbarSettings 
            HorizontalAlignment="End"
            VerticalAlignment="Start"
            OffsetX="-15"
            OffsetY="15"
            IconBrush="White"
            Background="#0078D4"/>
    </sunburst:SfSunburstChart.ToolbarSettings>

    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Company"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Division"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Department"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Team"/>
    </sunburst:SfSunburstChart.Levels>

</sunburst:SfSunburstChart>
```

### Example 2: Mobile-Friendly Bottom Toolbar

```xml
<sunburst:SfSunburstChart EnableDrillDown="True"
                          ItemsSource="{Binding SalesData}"
                          ValueMemberPath="Revenue"
                          StartAngle="0"
                          EndAngle="360">

    <sunburst:SfSunburstChart.ToolbarSettings>
        <sunburst:SunburstToolbarSettings 
            HorizontalAlignment="Center"
            VerticalAlignment="End"
            OffsetY="-20"
            IconBrush="White"
            Background="#212121"/>
    </sunburst:SfSunburstChart.ToolbarSettings>

    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Region"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Country"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="City"/>
    </sunburst:SfSunburstChart.Levels>

</sunburst:SfSunburstChart>
```

### Example 3: Minimal Floating Toolbar

```xml
<sunburst:SfSunburstChart EnableDrillDown="True"
                          ItemsSource="{Binding FileSystemData}"
                          ValueMemberPath="Size"
                          Stroke="White"
                          StrokeWidth="2">

    <sunburst:SfSunburstChart.ToolbarSettings>
        <sunburst:SunburstToolbarSettings 
            HorizontalAlignment="End"
            VerticalAlignment="End"
            OffsetX="-25"
            OffsetY="-25"
            IconBrush="DarkSlateGray"
            Background="#F5F5F5"/>
    </sunburst:SfSunburstChart.ToolbarSettings>

    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Drive"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Folder"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Subfolder"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="File"/>
    </sunburst:SfSunburstChart.Levels>

</sunburst:SfSunburstChart>
```

### Example 4: Themed with Center Content

```xml
<sunburst:SfSunburstChart EnableDrillDown="True"
                          ItemsSource="{Binding BudgetData}"
                          ValueMemberPath="Amount"
                          InnerRadius="0.6">

    <sunburst:SfSunburstChart.CenterView>
        <VerticalStackLayout HorizontalOptions="Center" VerticalOptions="Center">
            <Label Text="{Binding CurrentLevel}" FontSize="16" FontAttributes="Bold"/>
            <Label Text="{Binding TotalAmount, StringFormat='${0:N0}'}" FontSize="24"/>
        </VerticalStackLayout>
    </sunburst:SfSunburstChart.CenterView>

    <sunburst:SfSunburstChart.ToolbarSettings>
        <sunburst:SunburstToolbarSettings 
            HorizontalAlignment="Center"
            VerticalAlignment="Center"
            OffsetY="120"
            IconBrush="White"
            Background="{StaticResource SecondaryColor}"/>
    </sunburst:SfSunburstChart.ToolbarSettings>

    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Department"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Category"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Item"/>
    </sunburst:SfSunburstChart.Levels>

</sunburst:SfSunburstChart>
```

## Best Practices

### When to Enable Drill-Down

**Enable drill-down for:**
- 4+ hierarchical levels
- Large datasets with many segments
- Exploratory data analysis scenarios
- When users need to focus on specific branches

**Don't enable drill-down for:**
- Simple 1-2 level hierarchies
- Small datasets (<20 items)
- Static/presentation views
- When full context must remain visible

### Toolbar Placement

**Consider your layout:**
- **Top-right**: Common for control buttons, familiar pattern
- **Bottom-center**: Mobile-friendly, thumb-accessible
- **Floating bottom-right**: FAB style, modern design
- **Center with offset**: When large inner radius exists

**Avoid:**
- Placing toolbar over critical data
- Positions that conflict with legends or titles
- Center placement with small inner radius

### Styling Guidelines

**Match your theme:**
- Use app's primary/accent colors for toolbar background
- Ensure sufficient contrast between icons and background
- Consider semi-transparent backgrounds for overlay effect
- Test in both light and dark modes

**Icon visibility:**
- IconBrush should contrast strongly with Background
- White icons on dark backgrounds
- Dark icons on light backgrounds
- Test on actual devices for clarity

### Performance

**Optimize for smooth animations:**
- Limit to 3-5 hierarchical levels maximum
- Avoid excessive data label customization
- Test drill-down on target devices
- Consider disabling animations if performance issues occur

## Troubleshooting

**Issue:** Drill-down not working (no response to double-tap)
- **Solution:** Verify `EnableDrillDown="True"` is set
- Ensure chart has multiple hierarchical levels configured
- Check that segments have children to drill into
- Test with single-tap first to verify touch is working

**Issue:** Toolbar not visible
- **Solution:** Check toolbar settings are configured
- Verify alignment doesn't place it off-screen
- Adjust OffsetX/OffsetY to bring it into view
- Ensure Background isn't transparent matching chart background

**Issue:** Toolbar overlaps with important content
- **Solution:** Adjust HorizontalAlignment and VerticalAlignment
- Use OffsetX and OffsetY to fine-tune position
- Consider chart InnerRadius to create center space
- Reposition legends or titles that conflict

**Issue:** Toolbar appears but buttons don't work
- **Solution:** This is rare; verify EnableDrillDown is true
- Check that you've drilled down at least one level
- Ensure no overlay views are blocking touch events
- Test on different devices

**Issue:** Animation is choppy or slow
- **Solution:** Reduce number of segments (simplify data)
- Disable data labels during testing
- Test on actual hardware (not just emulator)
- Consider device performance capabilities

**Issue:** Can't drill into certain segments
- **Solution:** Verify those segments have children in data
- Check hierarchical levels configuration
- Ensure data has values for all GroupMemberPath levels
- Leaf segments (no children) cannot be drilled into

**Issue:** Toolbar styling not applying
- **Solution:** Ensure ToolbarSettings is properly configured
- Check color format (use Color.FromArgb for hex values in C#)
- Verify brush types are correct (SolidColorBrush for solid colors)
- Test with simple colors first (Colors.White, Colors.Black)
