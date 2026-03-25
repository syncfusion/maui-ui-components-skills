# Theme Keys Reference

## Table of Contents
- [Key Naming Convention](#key-naming-convention)
- [Finding Keys](#finding-keys)
- [Common Controls by Category](#common-controls-by-category)
  - [Buttons and Actions](#buttons-and-actions)
  - [Input Controls](#input-controls)
  - [Data Display](#data-display)
  - [Navigation](#navigation)
  - [Data Visualization](#data-visualization)
  - [Layout](#layout)
- [Theme Registration Keys](#theme-registration-keys)
- [Using Keys](#using-keys)

---

## Key Naming Convention

Syncfusion theme keys follow a consistent naming pattern:

```
Sf{ControlName}{Element}{Property}
```

**Components:**
- `Sf` - Prefix for all Syncfusion keys
- `{ControlName}` - Name of the control (Button, DataGrid, Calendar, etc.)
- `{Element}` - Specific UI element (Header, Cell, Selection, etc.)
- `{Property}` - Visual property (Background, TextColor, Stroke, etc.)

**Examples:**
```
SfButtonNormalBackground
│  │      │      └─ Property (Background)
│  │      └──────── Element (Normal state)
│  └─────────────── Control (Button)
└────────────────── Prefix

SfDataGridHeaderBackgroundColor
│  │        │      │           └─ Property (Color)
│  │        │      └──────────── Property type (Background)
│  │        └─────────────────── Element (Header)
│  └──────────────────────────── Control (DataGrid)
└─────────────────────────────── Prefix
```

## Finding Keys

### Method 1: Official Documentation
Complete key listings: [help.syncfusion.com/maui/themes/keys](https://help.syncfusion.com/maui/themes/keys)

### Method 2: Pattern Recognition
If you know the control and element, construct the key:
- Control: `SfButton`
- Element: Hover state
- Property: Background color
- **Key:** `SfButtonHoverBackground`

### Method 3: Runtime Inspection
```csharp
foreach (var key in Application.Current.Resources.Keys)
{
    if (key.ToString().StartsWith("SfDataGrid"))
        Console.WriteLine(key);
}
```

## Common Controls by Category

### Buttons and Actions

#### SfButton
```xml
<!-- Backgrounds -->
<Color x:Key="SfButtonNormalBackground">#2196F3</Color>
<Color x:Key="SfButtonHoverBackground">#1976D2</Color>
<Color x:Key="SfButtonPressedBackground">#1565C0</Color>
<Color x:Key="SfButtonDisabledBackground">#E0E0E0</Color>

<!-- Text Colors -->
<Color x:Key="SfButtonNormalTextColor">White</Color>
<Color x:Key="SfButtonHoverTextColor">White</Color>
<Color x:Key="SfButtonPressedTextColor">White</Color>
<Color x:Key="SfButtonDisabledTextColor">#9E9E9E</Color>

<!-- Strokes (Borders) -->
<Color x:Key="SfButtonNormalStroke">#2196F3</Color>
<Color x:Key="SfButtonHoverStroke">#1976D2</Color>
<Color x:Key="SfButtonPressedStroke">#1565C0</Color>
<Color x:Key="SfButtonDisabledStroke">#E0E0E0</Color>
```

**States:** Normal, Hover, Pressed, Disabled

### Input Controls

#### SfAutocomplete
```xml
<!-- Normal State -->
<Color x:Key="SfAutocompleteNormalBackground">White</Color>
<Color x:Key="SfAutocompleteNormalTextColor">#212121</Color>
<Color x:Key="SfAutocompleteNormalStroke">#BDBDBD</Color>
<Color x:Key="SfAutocompletePlaceholderTextColor">#9E9E9E</Color>

<!-- Focused State -->
<Color x:Key="SfAutocompleteFocusedBackground">White</Color>
<Color x:Key="SfAutocompleteFocusTextColor">#212121</Color>
<Color x:Key="SfAutocompleteFocusedStroke">#2196F3</Color>

<!-- Other States -->
<Color x:Key="SfAutocompleteHoverBackground">White</Color>
<Color x:Key="SfAutocompleteHoverStroke">#757575</Color>
<Color x:Key="SfAutocompleteDisabledBackground">#F5F5F5</Color>
<Color x:Key="SfAutocompleteDisabledTextColor">#9E9E9E</Color>

<!-- Icons -->
<Color x:Key="SfAutocompleteNormalClearButtonIconColor">#757575</Color>
<Color x:Key="SfAutocompleteHoverClearButtonIconColor">#424242</Color>
```

**States:** Normal, Hover, Focused, Disabled  
**Elements:** Background, Text, Stroke, Placeholder, Clear Button

### Data Display

#### SfDataGrid
```xml
<!-- Header -->
<Color x:Key="SfDataGridHeaderBackgroundColor">#1976D2</Color>
<Color x:Key="SfDataGridHeaderTextColor">White</Color>

<!-- Cells -->
<Color x:Key="SfDataGridNormalCellBackground">White</Color>
<Color x:Key="SfDataGridNormalCellTextColor">#212121</Color>
<Color x:Key="SfDataGridAlternateRowBackground">#F5F5F5</Color>

<!-- Selection -->
<Color x:Key="SfDataGridSelectionBackgroundColor">#BBDEFB</Color>
<Color x:Key="SfDataGridSelectionTextColor">#0D47A1</Color>

<!-- Borders -->
<Color x:Key="SfDataGridBorderColor">#E0E0E0</Color>

<!-- Sorting/Filtering -->
<Color x:Key="SfDataGridSortIconColor">#757575</Color>
<Color x:Key="SfDataGridFilterIconColor">#757575</Color>
```

**Elements:** Header, Cells, Selection, Borders, Icons

#### SfCalendar
```xml
<!-- Header -->
<Color x:Key="SfCalendarHeaderBackgroundColor">#1976D2</Color>
<Color x:Key="SfCalendarHeaderTextColor">White</Color>

<!-- Background -->
<Color x:Key="SfCalendarNormalBackground">White</Color>

<!-- Dates -->
<Color x:Key="SfCalendarMonthDatesTextColor">#212121</Color>
<Color x:Key="SfCalendarMonthDatesBackgroundColor">Transparent</Color>

<!-- Selection -->
<Color x:Key="SfCalendarSelectionColor">#2196F3</Color>
<Color x:Key="SfCalendarMonthSelectionTextColor">White</Color>

<!-- Today -->
<Color x:Key="SfCalendarTodayHighlightColor">#FF9800</Color>
<Color x:Key="SfCalendarMonthTodayTextColor">#212121</Color>
<Color x:Key="SfCalendarMonthTodayBackgroundColor">Transparent</Color>

<!-- Special Dates -->
<Color x:Key="SfCalendarWeekendDatesTextColor">#F44336</Color>
<Color x:Key="SfCalendarSpecialDatesTextColor">#4CAF50</Color>
<Color x:Key="SfCalendarSpecialDayIconColor">#4CAF50</Color>

<!-- Disabled -->
<Color x:Key="SfCalendarMonthDisabledDatesTextColor">#BDBDBD</Color>
<Color x:Key="SfCalendarMonthDisabledDatesBackgroundColor">Transparent</Color>

<!-- Leading/Trailing -->
<Color x:Key="SfCalendarMonthTrailingLeadingDatesTextColor">#9E9E9E</Color>

<!-- Range Selection -->
<Color x:Key="SfCalendarRangeSelectionColor">#BBDEFB</Color>
<Color x:Key="SfCalendarStartRangeSelectionColor">#2196F3</Color>
<Color x:Key="SfCalendarEndRangeSelectionColor">#2196F3</Color>

<!-- Hover -->
<Color x:Key="SfCalendarHoverColor">#E3F2FD</Color>
```

**Elements:** Header, Dates, Selection, Today, Weekends, Special Dates, Ranges

#### SfCards
```xml
<Color x:Key="SfCardViewBorderColor">#E0E0E0</Color>
<Color x:Key="SfCardViewIndicatorColor">#2196F3</Color>
```

### Navigation

#### SfAccordion
```xml
<!-- Header Normal -->
<Color x:Key="SfAccordionNormalHeaderBackground">White</Color>
<Color x:Key="SfAccordionNormalHeaderIconColor">#757575</Color>

<!-- Header Hover -->
<Color x:Key="SfAccordionHoverHeaderBackground">#F5F5F5</Color>
<Color x:Key="SfAccordionHoverHeaderIconColor">#424242</Color>

<!-- Header Pressed -->
<Color x:Key="SfAccordionHeaderRippleBackground">#E0E0E0</Color>
<Color x:Key="SfAccordionPressedHeaderIconColor">#212121</Color>

<!-- Header Focused -->
<Color x:Key="SfAccordionFocusedHeaderBackground">#E3F2FD</Color>
<Color x:Key="SfAccordionFocusedHeaderIconColor">#1976D2</Color>

<!-- Borders -->
<Color x:Key="SfAccordionExpandedItemStroke">#2196F3</Color>
<x:Double x:Key="SfAccordionExpandedItemStrokeThickness">2</x:Double>
<Color x:Key="SfAccordionFocusedItemStroke">#2196F3</Color>
```

**States:** Normal, Hover, Pressed, Focused, Expanded

### Data Visualization

#### SfChart
```xml
<!-- Series -->
<Color x:Key="SfChartSeriesFillColor">#2196F3</Color>

<!-- Axis -->
<Color x:Key="SfChartAxisLabelTextColor">#757575</Color>
<Color x:Key="SfChartAxisLineColor">#E0E0E0</Color>
<Color x:Key="SfChartAxisTickLineColor">#E0E0E0</Color>

<!-- Grid Lines -->
<Color x:Key="SfChartGridLineColor">#E0E0E0</Color>

<!-- Legend -->
<Color x:Key="SfChartLegendTextColor">#212121</Color>
<Color x:Key="SfChartLegendBackgroundColor">White</Color>

<!-- Tooltip -->
<Color x:Key="SfChartTooltipBackground">#616161</Color>
<Color x:Key="SfChartTooltipTextColor">White</Color>
```

#### SfRadialGauge
```xml
<!-- Needle Pointer -->
<Color x:Key="SfRadialGaugeNeedlePointerNeedleFillColor">#212121</Color>
<Color x:Key="SfRadialGaugeNeedlePointerKnobFillColor">#2196F3</Color>
<Color x:Key="SfRadialGaugeNeedlePointerTailFillColor">#757575</Color>

<!-- Axis -->
<Color x:Key="SfRadialGaugeAxisLineFillColor">#BDBDBD</Color>
<Color x:Key="SfRadialGaugeAxisLabelTextColor">#212121</Color>

<!-- Range Pointer -->
<Color x:Key="SfRadialGaugeRangePointerFillColor">#2196F3</Color>
```

### Layout

#### SfBadgeView
```xml
<Color x:Key="SfBadgeViewNormalBackground">#F44336</Color>
<Color x:Key="SfBadgeViewNormalStroke">#F44336</Color>
```

#### SfAvatarView
```xml
<Color x:Key="SfAvatarViewNormalBackground">#9E9E9E</Color>
<Color x:Key="SfAvatarViewNormalStroke">#757575</Color>
<Color x:Key="SfAvatarViewNormalInitialsColor">White</Color>
```

#### SfBusyIndicator
```xml
<Color x:Key="SfBusyIndicatorNormalOverlayFill">#80000000</Color>
<Color x:Key="SfBusyIndicatorNormalIndicatorColor">#2196F3</Color>
<Color x:Key="SfBusyIndicatorNormalTextColor">#212121</Color>
<x:Double x:Key="SfBusyIndicatorNormalFontSize">14</x:Double>
```

## Theme Registration Keys

To create a fully custom theme, register theme names for each control:

```xml
<ResourceDictionary>
    <!-- Register custom theme -->
    <x:String x:Key="SfButtonTheme">MyCustomTheme</x:String>
    <x:String x:Key="SfDataGridTheme">MyCustomTheme</x:String>
    <x:String x:Key="SfCalendarTheme">MyCustomTheme</x:String>
    <x:String x:Key="SfChartTheme">MyCustomTheme</x:String>
    <x:String x:Key="SfAccordionTheme">MyCustomTheme</x:String>
    <x:String x:Key="SfAutocompleteTheme">MyCustomTheme</x:String>
    <x:String x:Key="SfAvatarViewTheme">MyCustomTheme</x:String>
    <x:String x:Key="SfBadgeViewTheme">MyCustomTheme</x:String>
    <x:String x:Key="SfBusyIndicatorTheme">MyCustomTheme</x:String>
    <x:String x:Key="SfCardViewTheme">MyCustomTheme</x:String>
    <!-- Add for all controls you use -->
</ResourceDictionary>
```

**Pattern:** `Sf{ControlName}Theme`

**Supported Values:**
- `"MaterialLight"` - Use Material light theme
- `"MaterialDark"` - Use Material dark theme  
- `"CupertinoLight"` - Use Cupertino light theme
- `"CupertinoDark"` - Use Cupertino dark theme
- `"CommonTheme"` - When overriding with built-in theme
- `"MyCustomTheme"` - Your custom theme name

## Using Keys

### Override Specific Keys

```xml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <!-- Base theme -->
            <syncTheme:SyncfusionThemeResourceDictionary VisualTheme="MaterialLight"/>
            
            <!-- Override specific keys -->
            <ResourceDictionary>
                <Color x:Key="SfButtonNormalBackground">#FF5722</Color>
                <Color x:Key="SfDataGridHeaderBackgroundColor">#2196F3</Color>
                <Color x:Key="SfCalendarSelectionColor">#4CAF50</Color>
            </ResourceDictionary>
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

### Check Key Existence

```csharp
if (Application.Current.Resources.TryGetValue("SfButtonNormalBackground", out var value))
{
    Console.WriteLine($"Button background: {value}");
}
```

### List All Theme Keys

```csharp
var themeKeys = Application.Current.Resources.Keys
    .Where(k => k.ToString().StartsWith("Sf"))
    .OrderBy(k => k.ToString())
    .ToList();

foreach (var key in themeKeys)
{
    Console.WriteLine($"{key}: {Application.Current.Resources[key]}");
}
```

## Common Key Patterns

### State Modifiers
- `Normal` - Default state
- `Hover` - Mouse/pointer hover
- `Pressed` - Active click/tap
- `Focused` - Keyboard focus
- `Disabled` - Inactive/disabled
- `Selected` - Selected item

### Property Suffixes
- `Background` / `BackgroundColor` - Fill color
- `TextColor` - Text foreground color
- `Stroke` - Border color
- `IconColor` - Icon tint
- `Fill` / `FillColor` - Fill color (alternate)
- `FontSize` - Text size
- `FontFamily` - Text font
- `FontAttributes` - Bold, italic, etc.

### Element Names
- `Header` - Top section
- `Cell` - Data cell
- `Row` - Data row
- `Selection` - Selected state
- `Border` - Outline
- `Indicator` - Visual indicator
- `Icon` - Icon element
- `Button` - Button element
- `Label` - Text label

## Key Discovery Tips

1. **Start with control name**: `Sf{ControlName}`
2. **Add element**: `Sf{ControlName}{Element}`
3. **Add state** (if needed): `Sf{ControlName}{State}{Element}`
4. **Add property**: `Sf{ControlName}{Element}{Property}`

**Example Discovery:**
- Want: Button hover background
- Control: `SfButton`
- State: `Hover`
- Property: `Background`
- **Result:** `SfButtonHoverBackground` ✓

## Best Practices

1. **Use Exact Names**: Keys are case-sensitive
2. **Check Documentation**: Verify key names in official docs
3. **Test Changes**: Always verify keys work as expected
4. **Group Logically**: Organize overrides by control
5. **Comment Keys**: Note what each customization affects
6. **Maintain Consistency**: Use similar colors for similar states

## Related Topics

- [Applying Themes](applying-themes.md) - Basic theme setup
- [Overriding Themes](overriding-themes.md) - Customizing specific keys
- [Creating Custom Themes](creating-custom-themes.md) - Building complete themes
- [Theme Switching](theme-switching.md) - Runtime theme changes

## Additional Resources

- [Official Keys Documentation](https://help.syncfusion.com/maui/themes/keys) - Complete key listings for ALL controls
- [Theme Studio](https://help.syncfusion.com/maui/themes/howto) - Visual theme customization tool