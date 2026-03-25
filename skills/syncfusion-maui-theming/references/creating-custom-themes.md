# Creating Custom Themes

Building completely custom themes from scratch in Syncfusion .NET MAUI applications.

## Overview

Creating a custom theme gives you complete control over the visual appearance of all Syncfusion controls without being constrained by the built-in themes. This approach is ideal for:

- Unique brand identities
- Complete visual overhauls
- Design systems not matching Material/Cupertino
- Specialized industry requirements

## Custom Theme vs Override

### When to Override (Simpler)
- Minor color adjustments
- Keeping base theme structure
- Quick branding updates
- Using most default colors

### When to Create Custom Theme (More Control)
- Complete visual redesign
- Unique design system
- All controls need consistent custom styling
- Building reusable theme package

## Basic Custom Theme Structure

### Step 1: Register Theme for Controls

```xml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <!-- Register custom theme for each control -->
            <ResourceDictionary>
                <x:String x:Key="SfButtonTheme">CustomTheme</x:String>
                <x:String x:Key="SfDataGridTheme">CustomTheme</x:String>
                <x:String x:Key="SfCalendarTheme">CustomTheme</x:String>
                <x:String x:Key="SfChartTheme">CustomTheme</x:String>
                <!-- Add for all controls you use -->
            </ResourceDictionary>
            
            <!-- Define custom colors (Step 2) -->
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

**Theme Key Pattern:** `Sf{ControlName}Theme`

### Step 2: Define All Required Colors

```xml
<ResourceDictionary>
    <!-- Button colors -->
    <Color x:Key="SfButtonNormalBackground">#667EEA</Color>
    <Color x:Key="SfButtonNormalTextColor">White</Color>
    <Color x:Key="SfButtonNormalStroke">#667EEA</Color>
    <Color x:Key="SfButtonHoverBackground">#5A67D8</Color>
    <Color x:Key="SfButtonHoverTextColor">White</Color>
    <Color x:Key="SfButtonHoverStroke">#5A67D8</Color>
    <Color x:Key="SfButtonPressedBackground">#4C51BF</Color>
    <Color x:Key="SfButtonPressedTextColor">White</Color>
    <Color x:Key="SfButtonPressedStroke">#4C51BF</Color>
    <Color x:Key="SfButtonDisabledBackground">#E2E8F0</Color>
    <Color x:Key="SfButtonDisabledTextColor">#A0AEC0</Color>
    <Color x:Key="SfButtonDisabledStroke">#E2E8F0</Color>
    
    <!-- Add all other control colors -->
</ResourceDictionary>
```

## Complete Custom Theme Example

### Full Implementation

```xml
<Application xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="YourApp.App">
    
    <Application.Resources>
        <ResourceDictionary>
            <!-- Define base color palette -->
            <Color x:Key="Primary">#667EEA</Color>
            <Color x:Key="PrimaryDark">#5A67D8</Color>
            <Color x:Key="PrimaryDarker">#4C51BF</Color>
            <Color x:Key="Secondary">#48BB78</Color>
            <Color x:Key="Accent">#ECC94B</Color>
            <Color x:Key="Background">#FFFFFF</Color>
            <Color x:Key="Surface">#F7FAFC</Color>
            <Color x:Key="TextPrimary">#2D3748</Color>
            <Color x:Key="TextSecondary">#718096</Color>
            <Color x:Key="TextDisabled">#A0AEC0</Color>
            <Color x:Key="Border">#E2E8F0</Color>
            <Color x:Key="Error">#F56565</Color>
            <Color x:Key="Success">#48BB78</Color>
            
            <ResourceDictionary.MergedDictionaries>
                <!-- Register custom theme -->
                <ResourceDictionary>
                    <x:String x:Key="SfButtonTheme">CustomTheme</x:String>
                    <x:String x:Key="SfDataGridTheme">CustomTheme</x:String>
                    <x:String x:Key="SfCalendarTheme">CustomTheme</x:String>
                    <x:String x:Key="SfChartTheme">CustomTheme</x:String>
                    <x:String x:Key="SfAutocompleteTheme">CustomTheme</x:String>
                </ResourceDictionary>
                
                <!-- Button Theme -->
                <ResourceDictionary>
                    <Color x:Key="SfButtonNormalBackground">{StaticResource Primary}</Color>
                    <Color x:Key="SfButtonNormalTextColor">White</Color>
                    <Color x:Key="SfButtonNormalStroke">{StaticResource Primary}</Color>
                    <Color x:Key="SfButtonHoverBackground">{StaticResource PrimaryDark}</Color>
                    <Color x:Key="SfButtonHoverTextColor">White</Color>
                    <Color x:Key="SfButtonHoverStroke">{StaticResource PrimaryDark}</Color>
                    <Color x:Key="SfButtonPressedBackground">{StaticResource PrimaryDarker}</Color>
                    <Color x:Key="SfButtonPressedTextColor">White</Color>
                    <Color x:Key="SfButtonPressedStroke">{StaticResource PrimaryDarker}</Color>
                    <Color x:Key="SfButtonDisabledBackground">{StaticResource Surface}</Color>
                    <Color x:Key="SfButtonDisabledTextColor">{StaticResource TextDisabled}</Color>
                    <Color x:Key="SfButtonDisabledStroke">{StaticResource Border}</Color>
                </ResourceDictionary>
                
                <!-- DataGrid Theme -->
                <ResourceDictionary>
                    <Color x:Key="SfDataGridHeaderBackgroundColor">{StaticResource Primary}</Color>
                    <Color x:Key="SfDataGridHeaderTextColor">White</Color>
                    <Color x:Key="SfDataGridNormalCellBackground">{StaticResource Background}</Color>
                    <Color x:Key="SfDataGridNormalCellTextColor">{StaticResource TextPrimary}</Color>
                    <Color x:Key="SfDataGridAlternateRowBackground">{StaticResource Surface}</Color>
                    <Color x:Key="SfDataGridSelectionBackgroundColor">{StaticResource Primary}</Color>
                    <Color x:Key="SfDataGridSelectionTextColor">White</Color>
                    <Color x:Key="SfDataGridBorderColor">{StaticResource Border}</Color>
                </ResourceDictionary>
                
                <!-- Calendar Theme -->
                <ResourceDictionary>
                    <Color x:Key="SfCalendarHeaderBackgroundColor">{StaticResource Primary}</Color>
                    <Color x:Key="SfCalendarHeaderTextColor">White</Color>
                    <Color x:Key="SfCalendarNormalBackground">{StaticResource Background}</Color>
                    <Color x:Key="SfCalendarMonthDatesTextColor">{StaticResource TextPrimary}</Color>
                    <Color x:Key="SfCalendarMonthDatesBackgroundColor">{StaticResource Background}</Color>
                    <Color x:Key="SfCalendarSelectionColor">{StaticResource Primary}</Color>
                    <Color x:Key="SfCalendarMonthSelectionTextColor">White</Color>
                    <Color x:Key="SfCalendarTodayHighlightColor">{StaticResource Accent}</Color>
                    <Color x:Key="SfCalendarWeekendDatesTextColor">{StaticResource Error}</Color>
                </ResourceDictionary>
                
                <!-- Chart Theme -->
                <ResourceDictionary>
                    <Color x:Key="SfChartSeriesFillColor">{StaticResource Primary}</Color>
                    <Color x:Key="SfChartAxisLabelTextColor">{StaticResource TextSecondary}</Color>
                    <Color x:Key="SfChartAxisLineColor">{StaticResource Border}</Color>
                    <Color x:Key="SfChartGridLineColor">{StaticResource Border}</Color>
                    <Color x:Key="SfChartLegendTextColor">{StaticResource TextPrimary}</Color>
                </ResourceDictionary>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

## Dark Mode Custom Theme

### Separate Dark Theme

```xml
<Application.Resources>
    <ResourceDictionary>
        <!-- Dark mode color palette -->
        <Color x:Key="Primary">#667EEA</Color>
        <Color x:Key="PrimaryLight">#7F9CF5</Color>
        <Color x:Key="PrimaryLighter">#A3BFFA</Color>
        <Color x:Key="Background">#1A202C</Color>
        <Color x:Key="Surface">#2D3748</Color>
        <Color x:Key="TextPrimary">#F7FAFC</Color>
        <Color x:Key="TextSecondary">#CBD5E0</Color>
        <Color x:Key="TextDisabled">#718096</Color>
        <Color x:Key="Border">#4A5568</Color>
        
        <ResourceDictionary.MergedDictionaries>
            <!-- Register dark theme -->
            <ResourceDictionary>
                <x:String x:Key="SfButtonTheme">CustomDarkTheme</x:String>
                <x:String x:Key="SfDataGridTheme">CustomDarkTheme</x:String>
                <!-- Other controls -->
            </ResourceDictionary>
            
            <!-- Button Dark Theme -->
            <ResourceDictionary>
                <Color x:Key="SfButtonNormalBackground">{StaticResource Primary}</Color>
                <Color x:Key="SfButtonNormalTextColor">{StaticResource TextPrimary}</Color>
                <Color x:Key="SfButtonHoverBackground">{StaticResource PrimaryLight}</Color>
                <!-- More dark theme colors -->
            </ResourceDictionary>
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

## Organizing Custom Themes

### External Resource Dictionary

**Create:** `Themes/CustomLightTheme.xaml`

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<ResourceDictionary xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
                    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml">
    
    <!-- Base colors -->
    <Color x:Key="Primary">#667EEA</Color>
    <Color x:Key="Secondary">#48BB78</Color>
    <!-- More colors -->
    
    <!-- Theme registrations -->
    <x:String x:Key="SfButtonTheme">CustomLightTheme</x:String>
    <x:String x:Key="SfDataGridTheme">CustomLightTheme</x:String>
    
    <!-- Button colors -->
    <Color x:Key="SfButtonNormalBackground">{StaticResource Primary}</Color>
    <!-- More button colors -->
    
    <!-- DataGrid colors -->
    <Color x:Key="SfDataGridHeaderBackgroundColor">{StaticResource Primary}</Color>
    <!-- More DataGrid colors -->
    
</ResourceDictionary>
```

**In App.xaml:**

```xml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <ResourceDictionary Source="Themes/CustomLightTheme.xaml"/>
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

### Multiple Theme Files

Organize by control:

```
Themes/
├── CustomLightTheme.xaml       (base colors)
├── ButtonTheme.xaml            (button-specific)
├── DataGridTheme.xaml          (grid-specific)
├── CalendarTheme.xaml          (calendar-specific)
└── ChartTheme.xaml             (chart-specific)
```

**Merge all:**

```xml
<ResourceDictionary.MergedDictionaries>
    <ResourceDictionary Source="Themes/CustomLightTheme.xaml"/>
    <ResourceDictionary Source="Themes/ButtonTheme.xaml"/>
    <ResourceDictionary Source="Themes/DataGridTheme.xaml"/>
    <ResourceDictionary Source="Themes/CalendarTheme.xaml"/>
    <ResourceDictionary Source="Themes/ChartTheme.xaml"/>
</ResourceDictionary.MergedDictionaries>
```

## Design System Integration

### Implement Design Tokens

```xml
<ResourceDictionary>
    <!-- Spacing tokens -->
    <x:Double x:Key="SpacingXS">4</x:Double>
    <x:Double x:Key="SpacingSM">8</x:Double>
    <x:Double x:Key="SpacingMD">16</x:Double>
    <x:Double x:Key="SpacingLG">24</x:Double>
    <x:Double x:Key="SpacingXL">32</x:Double>
    
    <!-- Font sizes -->
    <x:Double x:Key="FontSizeXS">12</x:Double>
    <x:Double x:Key="FontSizeSM">14</x:Double>
    <x:Double x:Key="FontSizeMD">16</x:Double>
    <x:Double x:Key="FontSizeLG">20</x:Double>
    <x:Double x:Key="FontSizeXL">24</x:Double>
    
    <!-- Border radius -->
    <x:Double x:Key="BorderRadiusSM">4</x:Double>
    <x:Double x:Key="BorderRadiusMD">8</x:Double>
    <x:Double x:Key="BorderRadiusLG">16</x:Double>
    
    <!-- Shadows -->
    <Shadow x:Key="ShadowSM">
        <Shadow.Offset>0, 2</Shadow.Offset>
        <Shadow.Radius>4</Shadow.Radius>
        <Shadow.Opacity>0.1</Shadow.Opacity>
    </Shadow>
    
    <!-- Use in theme -->
    <Color x:Key="SfButtonNormalBackground">{StaticResource Primary}</Color>
    <x:Double x:Key="SfButtonFontSize">{StaticResource FontSizeMD}</x:Double>
</ResourceDictionary>
```

## Hybrid Approach: Base Theme + Custom

Combine built-in theme with custom overrides:

```xml
<ResourceDictionary.MergedDictionaries>
    <!-- Start with built-in theme -->
    <syncTheme:SyncfusionThemeResourceDictionary VisualTheme="MaterialLight"/>
    
    <!-- Register custom theme for specific controls -->
    <ResourceDictionary>
        <x:String x:Key="SfButtonTheme">CustomTheme</x:String>
        <x:String x:Key="SfDataGridTheme">CustomTheme</x:String>
        <!-- Calendar and Chart use MaterialLight -->
    </ResourceDictionary>
    
    <!-- Custom colors for registered controls -->
    <ResourceDictionary>
        <Color x:Key="SfButtonNormalBackground">#667EEA</Color>
        <Color x:Key="SfDataGridHeaderBackgroundColor">#667EEA</Color>
    </ResourceDictionary>
</ResourceDictionary.MergedDictionaries>
```

**Benefits:**
- Less work (only customize what you need)
- Fallback to built-in theme for uncustomized controls
- Gradual customization approach

## Finding All Required Keys

### Per-Control Key Discovery

For each control, you need to define keys for:

1. **Background colors**: Normal, hover, pressed, disabled, focused
2. **Text colors**: Normal, hover, pressed, disabled
3. **Border colors**: Normal, hover, focused
4. **State-specific colors**: Selection, highlight, error
5. **Typography**: Font sizes, font families

**Reference:** See [theme-keys-reference.md](theme-keys-reference.md) for complete lists.

### Minimum Viable Theme

Start with essential keys:

```xml
<!-- Minimum button theme -->
<Color x:Key="SfButtonNormalBackground">#667EEA</Color>
<Color x:Key="SfButtonNormalTextColor">White</Color>
<Color x:Key="SfButtonDisabledBackground">#E2E8F0</Color>
<Color x:Key="SfButtonDisabledTextColor">#A0AEC0</Color>

<!-- Minimum DataGrid theme -->
<Color x:Key="SfDataGridHeaderBackgroundColor">#667EEA</Color>
<Color x:Key="SfDataGridHeaderTextColor">White</Color>
<Color x:Key="SfDataGridSelectionBackgroundColor">#A3BFFA</Color>
```

Then expand as needed.

## Testing Custom Themes

### Visual Testing Checklist

1. ✅ All controls render with custom colors
2. ✅ Interaction states work (hover, press, disabled)
3. ✅ Text remains readable (contrast check)
4. ✅ Borders and separators visible
5. ✅ Special states (selection, error) clear
6. ✅ Platform consistency (iOS, mac OS, Android, Windows)
7. ✅ Light and dark variants both work

### Automated Color Contrast Check

```csharp
public bool HasSufficientContrast(Color foreground, Color background)
{
    // WCAG AA requires 4.5:1 for normal text
    var contrast = CalculateContrastRatio(foreground, background);
    return contrast >= 4.5;
}

private double CalculateContrastRatio(Color c1, Color c2)
{
    var l1 = GetRelativeLuminance(c1);
    var l2 = GetRelativeLuminance(c2);
    
    var lighter = Math.Max(l1, l2);
    var darker = Math.Min(l1, l2);
    
    return (lighter + 0.05) / (darker + 0.05);
}

private double GetRelativeLuminance(Color color)
{
    var r = GetColorComponent(color.Red);
    var g = GetColorComponent(color.Green);
    var b = GetColorComponent(color.Blue);
    
    return 0.2126 * r + 0.7152 * g + 0.0722 * b;
}

private double GetColorComponent(float component)
{
    return component <= 0.03928
        ? component / 12.92
        : Math.Pow((component + 0.055) / 1.055, 2.4);
}
```

## Version Control for Themes

### Git-Friendly Structure

```
Themes/
├── Base/
│   ├── Colors.xaml          (color palette)
│   ├── Typography.xaml      (font definitions)
│   └── Spacing.xaml         (spacing tokens)
├── Light/
│   └── CustomLightTheme.xaml
└── Dark/
    └── CustomDarkTheme.xaml
```

**Benefits:**
- Easy to review color changes
- Merge conflicts easier to resolve
- Clear theme structure

## Performance Considerations

### Lazy Loading Themes

Load themes only when needed:

```csharp
public async Task LoadThemeAsync(string themePath)
{
    var dictionary = new ResourceDictionary
    {
        Source = new Uri(themePath, UriKind.Relative)
    };
    
    await MainThread.InvokeOnMainThreadAsync(() =>
    {
        Application.Current.Resources.MergedDictionaries.Add(dictionary);
    });
}

// Usage
await LoadThemeAsync("Themes/CustomLightTheme.xaml");
```

### Resource Dictionary Caching

```csharp
private static Dictionary<string, ResourceDictionary> _themeCache = new();

public ResourceDictionary GetTheme(string themeName)
{
    if (!_themeCache.TryGetValue(themeName, out var theme))
    {
        theme = new ResourceDictionary
        {
            Source = new Uri($"Themes/{themeName}.xaml", UriKind.Relative)
        };
        _themeCache[themeName] = theme;
    }
    
    return theme;
}
```

## Best Practices

1. **Start Simple**: Begin with minimum viable theme, expand gradually
2. **Use Design Tokens**: Define base colors, reference everywhere
3. **Organize Files**: Separate theme files by control or category
4. **Document Colors**: Comment what each color is used for
5. **Test Accessibility**: Ensure sufficient contrast ratios
6. **Version Control**: Track theme changes in source control
7. **Platform Test**: Verify on all target platforms
8. **Maintain Consistency**: Use systematic naming conventions
9. **Cache Themes**: Load once, reuse for performance
10. **Provide Fallbacks**: Handle missing keys gracefully

## Related Topics

- [Applying Themes](applying-themes.md) - Basic theme setup
- [Overriding Themes](overriding-themes.md) - Selective customization
- [Theme Keys Reference](theme-keys-reference.md) - Complete key listings
- [Theme Switching](theme-switching.md) - Runtime theme changes