# Data Labels in .NET MAUI Sunburst Chart

## Table of Contents
- [Overview](#overview)
- [Enabling Data Labels](#enabling-data-labels)
- [Overflow Mode](#overflow-mode)
- [Rotation Mode](#rotation-mode)
- [Customization](#customization)
- [Common Patterns](#common-patterns)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

Data labels display information about segments at their visual location, helping users understand the data without needing to hover or select segments. Labels can show category names, values, or custom formatted text.

## Enabling Data Labels

Data labels are controlled by the `ShowLabels` property, which is `False` by default.

**XAML:**
```xml
<sunburst:SfSunburstChart ShowLabels="True"
                          ItemsSource="{Binding DataSource}"
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
sunburst.ValueMemberPath = "EmployeesCount";
sunburst.ShowLabels = true;
// ... configure levels
this.Content = sunburst;
```

**Default behavior:**
- Labels display the category name from GroupMemberPath
- Labels are positioned at the center of each segment
- Labels automatically rotate to follow segment angle

## Overflow Mode

When data labels are too large to fit within their segments, they can overlap. The `OverflowMode` property controls how to handle this situation.

**Available modes:**
- **Trim**: Truncates labels that don't fit (default)
- **Hide**: Hides labels that don't fit completely

### Trim Mode (Default)

Truncates labels with ellipsis (...) when they exceed segment space.

**XAML:**
```xml
<sunburst:SfSunburstChart ShowLabels="True"
                          ItemsSource="{Binding DataSource}"
                          ValueMemberPath="Value">
    <sunburst:SfSunburstChart.DataLabelSettings>
        <sunburst:SunburstDataLabelSettings OverFlowMode="Trim"/>
    </sunburst:SfSunburstChart.DataLabelSettings>
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Category"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
SfSunburstChart sunburst = new SfSunburstChart();
sunburst.ShowLabels = true;
sunburst.DataLabelSettings = new SunburstDataLabelSettings()
{
    OverFlowMode = SunburstLabelOverflowMode.Trim
};
// ... configure data and levels
this.Content = sunburst;
```

**Use cases:**
- When all segments should show some label text
- For long category names that need truncation
- When partial information is better than no information

### Hide Mode

Completely hides labels that don't fit within their segments.

**XAML:**
```xml
<sunburst:SfSunburstChart ShowLabels="True"
                          ItemsSource="{Binding DataSource}"
                          ValueMemberPath="Value">
    <sunburst:SfSunburstChart.DataLabelSettings>
        <sunburst:SunburstDataLabelSettings OverFlowMode="Hide"/>
    </sunburst:SfSunburstChart.DataLabelSettings>
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Product"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Model"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
SfSunburstChart sunburst = new SfSunburstChart();
sunburst.ShowLabels = true;
sunburst.DataLabelSettings = new SunburstDataLabelSettings()
{
    OverFlowMode = SunburstLabelOverflowMode.Hide
};
// ... configure data and levels
this.Content = sunburst;
```

**Use cases:**
- For clean, uncluttered visualizations
- When small segments shouldn't have labels
- When tooltips provide the missing information

## Rotation Mode

The `RotationMode` property controls how labels are oriented within segments.

**Available modes:**
- **Angle**: Labels rotate to follow segment angle (default)
- **Normal**: Labels remain horizontal/upright

### Angle Mode (Default)

Labels rotate to align with the segment's radial direction, creating a sunburst effect.

**XAML:**
```xml
<sunburst:SfSunburstChart ShowLabels="True"
                          ItemsSource="{Binding DataSource}"
                          ValueMemberPath="Sales">
    <sunburst:SfSunburstChart.DataLabelSettings>
        <sunburst:SunburstDataLabelSettings RotationMode="Angle"/>
    </sunburst:SfSunburstChart.DataLabelSettings>
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Region"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Country"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
SfSunburstChart sunburst = new SfSunburstChart();
sunburst.ShowLabels = true;
sunburst.DataLabelSettings = new SunburstDataLabelSettings()
{
    RotationMode = SunburstLabelRotationMode.Angle     
};
// ... configure data and levels
this.Content = sunburst;
```

**Characteristics:**
- Labels follow segment curvature
- Natural radial reading pattern
- Better space utilization for narrow segments
- May require rotating device/head for bottom segments

**Use cases:**
- Standard sunburst visualizations
- When emphasizing radial hierarchy
- For artistic or poster-style presentations

### Normal Mode

Labels remain horizontal regardless of segment angle, improving readability.

**XAML:**
```xml
<sunburst:SfSunburstChart ShowLabels="True"
                          ItemsSource="{Binding DataSource}"
                          ValueMemberPath="Count">
    <sunburst:SfSunburstChart.DataLabelSettings>
        <sunburst:SunburstDataLabelSettings RotationMode="Normal"/>
    </sunburst:SfSunburstChart.DataLabelSettings>
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Division"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Department"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
SfSunburstChart sunburst = new SfSunburstChart();
sunburst.ShowLabels = true;
sunburst.DataLabelSettings = new SunburstDataLabelSettings()
{
    RotationMode = SunburstLabelRotationMode.Normal     
};
// ... configure data and levels
this.Content = sunburst;
```

**Characteristics:**
- All labels horizontal and upright
- Easier to read without head rotation
- May not fit as well in narrow segments
- Better for business/professional reports

**Use cases:**
- Business dashboards and reports
- When readability is paramount
- Mobile applications where rotation is inconvenient
- Accessibility requirements

## Customization

Customize label appearance using the `DataLabelSettings` property with `SunburstDataLabelSettings`.

### Available Properties

**Font Properties:**
- **FontAttributes**: Font style (Bold, Italic, None)
- **FontFamily**: Font name (e.g., "Arial", "Verdana")
- **FontSize**: Font size in device-independent units

**Color Properties:**
- **TextColor**: Label text color

### Font Customization Example

**XAML:**
```xml
<sunburst:SfSunburstChart ShowLabels="True"
                          ItemsSource="{Binding DataSource}"
                          ValueMemberPath="Amount">
    <sunburst:SfSunburstChart.DataLabelSettings>
        <sunburst:SunburstDataLabelSettings     
            TextColor="Red"   
            FontSize="10"    
            FontAttributes="Bold"
            FontFamily="Arial"/>
    </sunburst:SfSunburstChart.DataLabelSettings>
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Category"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Product"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
SfSunburstChart sunburst = new SfSunburstChart();
sunburst.ShowLabels = true;
sunburst.DataLabelSettings = new SunburstDataLabelSettings()
{
    TextColor = Colors.Red,
    FontSize = 10,
    FontAttributes = FontAttributes.Bold,
    FontFamily = "Arial"
};
// ... configure data and levels
this.Content = sunburst;
```

### Style Variations

**Subtle Labels:**
```xml
<sunburst:SunburstDataLabelSettings 
    TextColor="Gray" 
    FontSize="9" 
    FontAttributes="None"/>
```

**Prominent Labels:**
```xml
<sunburst:SunburstDataLabelSettings 
    TextColor="Black" 
    FontSize="14" 
    FontAttributes="Bold"/>
```

**Themed Labels:**
```xml
<sunburst:SunburstDataLabelSettings 
    TextColor="{StaticResource PrimaryTextColor}" 
    FontSize="11" 
    FontAttributes="Italic"/>
```

## Common Patterns

### Pattern 1: Outer Ring Labels Only

Show labels only for the outermost level using conditional styling or by controlling segment sizes.

```xml
<sunburst:SfSunburstChart ShowLabels="True" InnerRadius="0.6">
    <!-- Large inner radius makes inner rings thinner, 
         hiding their labels naturally -->
    <sunburst:SfSunburstChart.DataLabelSettings>
        <sunburst:SunburstDataLabelSettings OverFlowMode="Hide"/>
    </sunburst:SfSunburstChart.DataLabelSettings>
</sunburst:SfSunburstChart>
```

### Pattern 2: High Contrast Labels

Use contrasting colors for maximum readability.

```xml
<sunburst:SfSunburstChart ShowLabels="True">
    <sunburst:SfSunburstChart.DataLabelSettings>
        <sunburst:SunburstDataLabelSettings 
            TextColor="White" 
            FontSize="12" 
            FontAttributes="Bold"/>
    </sunburst:SfSunburstChart.DataLabelSettings>
</sunburst:SfSunburstChart>
```

### Pattern 3: Compact Mobile-Friendly Labels

Smaller fonts with normal rotation for mobile readability.

```xml
<sunburst:SfSunburstChart ShowLabels="True">
    <sunburst:SfSunburstChart.DataLabelSettings>
        <sunburst:SunburstDataLabelSettings 
            FontSize="9"
            RotationMode="Normal"
            OverFlowMode="Hide"/>
    </sunburst:SfSunburstChart.DataLabelSettings>
</sunburst:SfSunburstChart>
```

### Pattern 4: Dashboard Style

Clean labels with selective display.

```xml
<sunburst:SfSunburstChart ShowLabels="True" Radius="0.8">
    <sunburst:SfSunburstChart.DataLabelSettings>
        <sunburst:SunburstDataLabelSettings 
            TextColor="DarkGray"
            FontSize="10"
            FontFamily="Segoe UI"
            OverFlowMode="Hide"
            RotationMode="Normal"/>
    </sunburst:SfSunburstChart.DataLabelSettings>
</sunburst:SfSunburstChart>
```

## Best Practices

### Font Size Guidelines

**Desktop/Tablet:**
- Small charts: 8-10pt
- Medium charts: 10-12pt
- Large charts: 12-14pt

**Mobile:**
- Compact view: 8-9pt
- Standard view: 9-10pt
- Readable view: 10-11pt

### Rotation Mode Selection

**Use Angle rotation when:**
- Creating traditional sunburst visualizations
- Emphasizing radial nature
- Space is limited in segments
- Design aesthetics are priority

**Use Normal rotation when:**
- Readability is critical
- Targeting mobile devices
- Creating business reports
- Accessibility is important

### Overflow Mode Selection

**Use Trim when:**
- All segments should show some text
- Users can access tooltips for full names
- Partial information is valuable
- Chart is not too cluttered

**Use Hide when:**
- Clean appearance is priority
- Small segments are numerous
- Tooltips provide complete information
- Labels would overlap heavily

### Color Selection

**For light backgrounds:**
- Dark text colors (Black, DarkGray, Navy)
- Bold font weights for better contrast

**For dark backgrounds:**
- Light text colors (White, LightGray, Yellow)
- Consider adding subtle borders/shadows

**For colored segments:**
- White or Black text depending on segment brightness
- Test contrast ratios for accessibility

## Complete Example

Combining all label features:

```xml
<sunburst:SfSunburstChart ItemsSource="{Binding DataSource}"
                          ValueMemberPath="Sales"
                          ShowLabels="True"
                          Radius="0.85"
                          InnerRadius="0.4"
                          EnableTooltip="True">

    <sunburst:SfSunburstChart.Title>
        <Label Text="Regional Sales Distribution" FontSize="18"/>
    </sunburst:SfSunburstChart.Title>

    <sunburst:SfSunburstChart.DataLabelSettings>
        <sunburst:SunburstDataLabelSettings 
            TextColor="White"
            FontSize="11"
            FontAttributes="Bold"
            FontFamily="Segoe UI"
            RotationMode="Angle"
            OverFlowMode="Hide"/>
    </sunburst:SfSunburstChart.DataLabelSettings>

    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Region"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Country"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="City"/>
    </sunburst:SfSunburstChart.Levels>

</sunburst:SfSunburstChart>
```

## Troubleshooting

**Issue:** Labels not appearing
- **Solution:** Verify `ShowLabels="True"` is set
- Check that segments are large enough (increase Radius)
- Ensure data has category values from GroupMemberPath

**Issue:** Labels overlapping
- **Solution:** Set `OverFlowMode="Hide"` to hide labels that don't fit
- Reduce font size in DataLabelSettings
- Increase chart Radius to provide more space

**Issue:** Labels are too small/large
- **Solution:** Adjust FontSize property in DataLabelSettings
- Test on target devices at appropriate sizes
- Consider responsive font sizing based on screen size

**Issue:** Labels hard to read on colored segments
- **Solution:** Use high contrast TextColor (White or Black)
- Add FontAttributes="Bold" for better visibility
- Consider using lighter segment colors

**Issue:** Labels appear upside down or sideways
- **Solution:** Use `RotationMode="Normal"` for horizontal labels
- This is expected with `RotationMode="Angle"` for bottom segments
- Consider using semi-circle (StartAngle/EndAngle) to avoid problematic angles

**Issue:** Some labels show while others don't
- **Solution:** This is normal with `OverFlowMode="Hide"`
- Segments too small won't show labels
- Increase chart size or reduce font size
- Use tooltips to show hidden information

**Issue:** Labels cut off at chart edges
- **Solution:** Reduce Radius (e.g., 0.8 instead of 0.9)
- This provides padding between chart edge and container bounds
- Consider container padding as well
