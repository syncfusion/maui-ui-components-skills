# Overriding Themes

Customizing default theme colors and styles in Syncfusion .NET MAUI applications.

## Overview

Theme overriding allows you to selectively customize specific colors or styles while keeping the base theme intact. This is ideal for:

- Applying corporate branding colors
- Customizing specific controls
- Making accessibility improvements
- Fine-tuning visual appearance

## Basic Override Pattern

### Override Specific Keys

```xml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <!-- Base theme -->
            <syncTheme:SyncfusionThemeResourceDictionary VisualTheme="MaterialDark"/>
            
            <!-- Custom overrides -->
            <ResourceDictionary>
                <Color x:Key="SfButtonNormalBackground">#FF5722</Color>
                <Color x:Key="SfDataGridHeaderBackgroundColor">#2196F3</Color>
            </ResourceDictionary>
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

**Key Points:**
- Base theme goes first
- Overrides go after in a separate ResourceDictionary
- Only override keys you need to change
- Order matters: last definition wins

## Layer Order

### Merged Dictionaries Order

The order of merged dictionaries determines which values take precedence:

```xml
<ResourceDictionary.MergedDictionaries>
    <!-- 1. Base theme (lowest priority) -->
    <syncTheme:SyncfusionThemeResourceDictionary VisualTheme="MaterialDark"/>
    
    <!-- 2. Common overrides -->
    <ResourceDictionary>
        <Color x:Key="PrimaryColor">#0066CC</Color>
    </ResourceDictionary>
    
    <!-- 3. Control-specific overrides (highest priority) -->
    <ResourceDictionary>
        <Color x:Key="SfButtonNormalBackground">{StaticResource PrimaryColor}</Color>
        <Color x:Key="SfDataGridHeaderBackgroundColor">{StaticResource PrimaryColor}</Color>
    </ResourceDictionary>
</ResourceDictionary.MergedDictionaries>
```

**Rule:** Later dictionaries override earlier ones for the same key.

## Types of Theme Keys

### Primary Keys

Primary keys affect multiple controls:

```xml
<ResourceDictionary>
    <!-- These would affect multiple controls if they existed as primary keys -->
    <!-- Note: Syncfusion primarily uses control-specific keys -->
    <Color x:Key="PrimaryColor">#0066CC</Color>
    <Color x:Key="AccentColor">#FF6600</Color>
</ResourceDictionary>
```

### Control-Specific Keys

Keys for specific control elements (most common):

```xml
<ResourceDictionary>
    <!-- Button keys -->
    <Color x:Key="SfButtonNormalBackground">#2196F3</Color>
    <Color x:Key="SfButtonNormalTextColor">White</Color>
    <Color x:Key="SfButtonHoverBackground">#1976D2</Color>
    
    <!-- DataGrid keys -->
    <Color x:Key="SfDataGridHeaderBackgroundColor">#1565C0</Color>
    <Color x:Key="SfDataGridHeaderTextColor">White</Color>
    <Color x:Key="SfDataGridSelectionBackgroundColor">#64B5F6</Color>
    
    <!-- Calendar keys -->
    <Color x:Key="SfCalendarSelectionColor">#4CAF50</Color>
    <Color x:Key="SfCalendarTodayHighlightColor">#FF9800</Color>
</ResourceDictionary>
```

## Common Customization Scenarios

### Scenario 1: Corporate Branding

Apply company colors across all controls:

```xml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <syncTheme:SyncfusionThemeResourceDictionary VisualTheme="MaterialLight"/>
            
            <ResourceDictionary>
                <!-- Define brand colors -->
                <Color x:Key="BrandPrimary">#0066CC</Color>
                <Color x:Key="BrandSecondary">#FF6600</Color>
                <Color x:Key="BrandAccent">#00CC99</Color>
                <Color x:Key="BrandDanger">#DC3545</Color>
                
                <!-- Apply to primary controls -->
                <Color x:Key="SfButtonNormalBackground">{StaticResource BrandPrimary}</Color>
                <Color x:Key="SfDataGridHeaderBackgroundColor">{StaticResource BrandPrimary}</Color>
                <Color x:Key="SfCalendarSelectionColor">{StaticResource BrandPrimary}</Color>
                
                <!-- Secondary actions -->
                <Color x:Key="SfChartSeriesFillColor">{StaticResource BrandSecondary}</Color>
                
                <!-- Accents -->
                <Color x:Key="SfBadgeViewNormalBackground">{StaticResource BrandAccent}</Color>
            </ResourceDictionary>
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

### Scenario 2: High Contrast for Accessibility

```xml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <syncTheme:SyncfusionThemeResourceDictionary VisualTheme="MaterialDark"/>
            
            <ResourceDictionary>
                <!-- High contrast colors -->
                <Color x:Key="HighContrastBackground">#000000</Color>
                <Color x:Key="HighContrastForeground">#FFFFFF</Color>
                <Color x:Key="HighContrastAccent">#FFFF00</Color>
                
                <!-- Apply high contrast -->
                <Color x:Key="SfButtonNormalBackground">{StaticResource HighContrastAccent}</Color>
                <Color x:Key="SfButtonNormalTextColor">{StaticResource HighContrastBackground}</Color>
                
                <Color x:Key="SfDataGridHeaderBackgroundColor">{StaticResource HighContrastForeground}</Color>
                <Color x:Key="SfDataGridHeaderTextColor">{StaticResource HighContrastBackground}</Color>
                
                <Color x:Key="SfCalendarSelectionColor">{StaticResource HighContrastAccent}</Color>
            </ResourceDictionary>
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

### Scenario 3: Specific Control Customization

Only customize DataGrid, keep other controls default:

```xml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <syncTheme:SyncfusionThemeResourceDictionary VisualTheme="MaterialLight"/>
            
            <ResourceDictionary>
                <!-- Only DataGrid customization -->
                <Color x:Key="SfDataGridHeaderBackgroundColor">#1976D2</Color>
                <Color x:Key="SfDataGridHeaderTextColor">White</Color>
                <Color x:Key="SfDataGridNormalCellBackground">White</Color>
                <Color x:Key="SfDataGridNormalCellTextColor">#212121</Color>
                <Color x:Key="SfDataGridSelectionBackgroundColor">#BBDEFB</Color>
                <Color x:Key="SfDataGridSelectionTextColor">#0D47A1</Color>
                <Color x:Key="SfDataGridBorderColor">#BDBDBD</Color>
            </ResourceDictionary>
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

### Scenario 4: State-Specific Overrides

Customize specific interaction states:

```xml
<ResourceDictionary>
    <!-- Normal state -->
    <Color x:Key="SfButtonNormalBackground">#2196F3</Color>
    <Color x:Key="SfButtonNormalTextColor">White</Color>
    
    <!-- Hover state -->
    <Color x:Key="SfButtonHoverBackground">#1976D2</Color>
    <Color x:Key="SfButtonHoverTextColor">White</Color>
    
    <!-- Pressed state -->
    <Color x:Key="SfButtonPressedBackground">#1565C0</Color>
    <Color x:Key="SfButtonPressedTextColor">White</Color>
    
    <!-- Disabled state -->
    <Color x:Key="SfButtonDisabledBackground">#E0E0E0</Color>
    <Color x:Key="SfButtonDisabledTextColor">#9E9E9E</Color>
</ResourceDictionary>
```

## Using Named Resources

Define reusable colors:

```xml
<Application.Resources>
    <ResourceDictionary>
        <!-- Define named colors first -->
        <Color x:Key="PrimaryBlue">#0066CC</Color>
        <Color x:Key="SecondaryOrange">#FF6600</Color>
        <Color x:Key="SuccessGreen">#28A745</Color>
        <Color x:Key="DangerRed">#DC3545</Color>
        <Color x:Key="WarningYellow">#FFC107</Color>
        
        <ResourceDictionary.MergedDictionaries>
            <syncTheme:SyncfusionThemeResourceDictionary VisualTheme="MaterialLight"/>
            
            <ResourceDictionary>
                <!-- Reference named colors -->
                <Color x:Key="SfButtonNormalBackground">{StaticResource PrimaryBlue}</Color>
                <Color x:Key="SfBadgeViewNormalBackground">{StaticResource SuccessGreen}</Color>
                <Color x:Key="SfChartSeriesFillColor">{StaticResource SecondaryOrange}</Color>
            </ResourceDictionary>
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

**Benefits:**
- Single source of truth for colors
- Easy to maintain and update
- Reusable across keys
- Clear naming convention

## Finding Keys to Override

### Method 1: Official Documentation

Visit [Syncfusion Theme Keys Documentation](https://help.syncfusion.com/maui/themes/keys) for complete key listings.

### Method 2: Inspect Key Pattern

Keys follow this pattern:
```
Sf{ControlName}{Element}{Property}
```

**Examples:**
- `SfDataGridHeaderBackgroundColor` → DataGrid → Header → Background → Color
- `SfButtonNormalTextColor` → Button → Normal state → Text → Color
- `SfCalendarSelectionColor` → Calendar → Selection → Color

### Method 3: Theme Keys Reference

See [theme-keys-reference.md](theme-keys-reference.md) for organized key listings.

### Method 4: Runtime Inspection

```csharp
// Print all theme keys
var resources = Application.Current.Resources;
foreach (var key in resources.Keys)
{
    if (key.ToString().StartsWith("Sf"))
    {
        Console.WriteLine($"{key}: {resources[key]}");
    }
}
```

## Override in Code-Behind

### Dynamic Overrides

```csharp
public void ApplyCustomColors()
{
    var resources = Application.Current.Resources;
    
    // Override specific keys
    resources["SfButtonNormalBackground"] = Color.FromArgb("#FF5722");
    resources["SfDataGridHeaderBackgroundColor"] = Color.FromArgb("#2196F3");
    resources["SfCalendarSelectionColor"] = Color.FromArgb("#4CAF50");
}

// Call after theme is loaded
protected override void OnAppearing()
{
    base.OnAppearing();
    ApplyCustomColors();
}
```

### Conditional Overrides

```csharp
public void ApplyThemeWithBranding(bool useBranding)
{
    var mergedDictionaries = Application.Current.Resources.MergedDictionaries;
    
    // Apply base theme
    mergedDictionaries.Add(new SyncfusionThemeResourceDictionary 
    { 
        VisualTheme = SfVisuals.MaterialLight 
    });
    
    if (useBranding)
    {
        // Apply brand overrides
        var brandDict = new ResourceDictionary();
        brandDict.Add("SfButtonNormalBackground", Color.FromArgb("#0066CC"));
        brandDict.Add("SfDataGridHeaderBackgroundColor", Color.FromArgb("#0066CC"));
        mergedDictionaries.Add(brandDict);
    }
}
```

## Selective Control Theming

### Theme Specific Controls Only

```xml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <!-- Register theme for specific controls only -->
            <ResourceDictionary>
                <x:String x:Key="SfDataGridTheme">CustomTheme</x:String>
                <x:String x:Key="SfChartTheme">CustomTheme</x:String>
                <!-- Other controls use default -->
            </ResourceDictionary>
            
            <!-- Define custom colors for those controls -->
            <ResourceDictionary>
                <Color x:Key="SfDataGridHeaderBackgroundColor">#1565C0</Color>
                <Color x:Key="SfChartSeriesFillColor">#FF6F00</Color>
            </ResourceDictionary>
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

## Testing Overrides

### Verify Override Applied

```csharp
public void VerifyThemeOverrides()
{
    var resources = Application.Current.Resources;
    
    // Check if key exists and has expected value
    if (resources.TryGetValue("SfButtonNormalBackground", out var value))
    {
        Console.WriteLine($"Button background: {value}");
    }
    else
    {
        Console.WriteLine("Key not found - override may not be applied");
    }
}
```

### Visual Testing Checklist

1. ✅ Verify overridden controls show custom colors
2. ✅ Check all interaction states (normal, hover, pressed, disabled)
3. ✅ Test light and dark theme variants
4. ✅ Verify contrast ratios meet accessibility standards
5. ✅ Test on all target platforms
6. ✅ Ensure text remains readable

## Common Mistakes

### Typo in Key Name

❌ **Incorrect:**
```xml
<Color x:Key="SfButtonBackgroundColor">#FF5722</Color>
<!-- Missing "Normal" in key name -->
```

✅ **Correct:**
```xml
<Color x:Key="SfButtonNormalBackground">#FF5722</Color>
```

### Wrong Override Order

❌ **Incorrect:**
```xml
<ResourceDictionary.MergedDictionaries>
    <!-- Overrides first (will be replaced by theme) -->
    <ResourceDictionary>
        <Color x:Key="SfButtonNormalBackground">#FF5722</Color>
    </ResourceDictionary>
    
    <syncTheme:SyncfusionThemeResourceDictionary VisualTheme="MaterialDark"/>
</ResourceDictionary.MergedDictionaries>
```

✅ **Correct:**
```xml
<ResourceDictionary.MergedDictionaries>
    <!-- Theme first -->
    <syncTheme:SyncfusionThemeResourceDictionary VisualTheme="MaterialDark"/>
    
    <!-- Overrides after -->
    <ResourceDictionary>
        <Color x:Key="SfButtonNormalBackground">#FF5722</Color>
    </ResourceDictionary>
</ResourceDictionary.MergedDictionaries>
```

### Invalid Color Format

❌ **Incorrect:**
```xml
<Color x:Key="SfButtonNormalBackground">rgb(255, 87, 34)</Color>
```

✅ **Correct:**
```xml
<Color x:Key="SfButtonNormalBackground">#FF5722</Color>
<!-- or -->
<Color x:Key="SfButtonNormalBackground">Color.FromRgb(255, 87, 34)</Color>
```

## Best Practices

1. **Override Selectively**: Only customize what you need
2. **Use Named Resources**: Define colors once, reference everywhere
3. **Maintain Consistency**: Apply brand colors consistently
4. **Test Thoroughly**: Verify all states and platforms
5. **Document Overrides**: Keep track of customized keys
6. **Respect Accessibility**: Ensure sufficient contrast
7. **Layer Properly**: Base theme first, overrides after
8. **Group Logically**: Organize overrides by control or purpose

## Related Topics

- [Applying Themes](applying-themes.md) - Basic theme setup
- [Creating Custom Themes](creating-custom-themes.md) - Building themes from scratch
- [Theme Keys Reference](theme-keys-reference.md) - Complete key listings
- [Theme Switching](theme-switching.md) - Runtime theme changes