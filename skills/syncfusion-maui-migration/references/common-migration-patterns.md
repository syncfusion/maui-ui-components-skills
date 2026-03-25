# Common Migration Patterns: Xamarin.Forms to .NET MAUI

Cross-cutting migration patterns and common changes that apply across all Syncfusion controls.

## Table of Contents
- [Handler Registration](#handler-registration)
- [NuGet Package Changes](#nuget-package-changes)
- [Namespace Patterns](#namespace-patterns)
- [Common Property Renames](#common-property-renames)
- [Color to Brush Migration](#color-to-brush-migration)
- [Event Argument Changes](#event-argument-changes)
- [Enum Renaming Patterns](#enum-renaming-patterns)
- [Breaking Changes by Version](#breaking-changes-by-version)
- [Migration Checklist](#migration-checklist)
- [Troubleshooting Guide](#troubleshooting-guide)

## Handler Registration

### Required Step for All Syncfusion MAUI Controls

**Add to `MauiProgram.cs`:**

```csharp
using Syncfusion.Maui.Core.Hosting;

public static class MauiProgram
{
    public static MauiApp CreateMauiApp()
    {
        var builder = MauiApp.CreateBuilder();
        builder
           .UseMauiApp<App>()
            .ConfigureSyncfusionCore();  // ✓ REQUIRED
        
        return builder.Build();
    }
}
```

**Why Required:**
- Registers Syncfusion handlers with .NET MAUI
- Must be called before using any Syncfusion control
- Single call handles all Syncfusion controls

**Common Error Without Registration:**
```
System.InvalidOperationException: Handler not found for view
```

## NuGet Package Changes

### Package Naming Pattern

| Xamarin Pattern | MAUI Pattern |
|----------------|--------------|
| `Syncfusion.Xamarin.*` | `Syncfusion.Maui.*` |

### Common Package Migrations

| Xamarin Package | MAUI Package |
|-----------------|--------------|
| `Syncfusion.Xamarin.SfDataGrid` | `Syncfusion.Maui.DataGrid` |
| `Syncfusion.Xamarin.SfChart` | `Syncfusion.Maui.Charts` |
| `Syncfusion.Xamarin.SfCalendar` | `Syncfusion.Maui.Calendar` |
| `Syncfusion.Xamarin.SfSchedule` | `Syncfusion.Maui.Scheduler` |
| `Syncfusion.Xamarin.Buttons` | `Syncfusion.Maui.Buttons` |
| `Syncfusion.Xamarin.Core` | `Syncfusion.Maui.Core` |

### Package Update Commands

```bash
# Remove Xamarin package
dotnet remove package Syncfusion.Xamarin.SfDataGrid

# Add MAUI package
dotnet add package Syncfusion.Maui.DataGrid
```

## Namespace Patterns

### Standard Namespace Migration

**Pattern:** `Syncfusion.*.XForms` → `Syncfusion.Maui.*`

### Common Namespace Changes

```csharp
// DataGrid
using Syncfusion.SfDataGrid.XForms;        // Xamarin
using Syncfusion.Maui.DataGrid;            // MAUI

// Charts
using Syncfusion.SfChart.XForms;           // Xamarin
using Syncfusion.Maui.Charts;              // MAUI

// Calendar
using Syncfusion.SfCalendar.XForms;        // Xamarin
using Syncfusion.Maui.Calendar;            // MAUI

// Buttons
using Syncfusion.XForms.Buttons;           // Xamarin
using Syncfusion.Maui.Buttons;             // MAUI

// Gauges
using Syncfusion.SfGauge.XForms;           // Xamarin
using Syncfusion.Maui.Gauges;              // MAUI

// Pickers
using Syncfusion.XForms.Pickers;           // Xamarin
using Syncfusion.Maui.Picker;              // MAUI (note: singular)

// Inputs
using Syncfusion.XForms.MaskedEdit;        // Xamarin
using Syncfusion.Maui.Inputs;              // MAUI

// Sliders
using Syncfusion.SfSlider.XForms;          // Xamarin
using Syncfusion.Maui.Sliders;             // MAUI (note: plural)
```

### XAML Namespace Declarations

**Xamarin:**
```xml
xmlns:syncfusion="clr-namespace:Syncfusion.SfDataGrid.XForms;assembly=Syncfusion.SfDataGrid.XForms"
```

**.NET MAUI:**
```xml
xmlns:syncfusion="clr-namespace:Syncfusion.Maui.DataGrid;assembly=Syncfusion.Maui.DataGrid"
```

## Common Property Renames

### Background and Stroke Properties

| Xamarin Pattern | MAUI Pattern | Description |
|----------------|--------------|-------------|
| `*BackgroundColor` | `*Background` | Background brush |
| `*BorderColor` | `*Stroke` | Border/stroke color |
| `*BorderWidth` | `*StrokeThickness` | Border thickness |
| `*Color` | `*Fill` | Fill/interior color |

**Examples:**
```csharp
// Xamarin
control.BackgroundColor = Color.Blue;
control.BorderColor = Color.Black;
control.BorderWidth = 2;

// MAUI
control.Background = Colors.Blue;
control.Stroke = Colors.Black;
control.StrokeThickness = 2;
```

### Text and Foreground Properties

| Xamarin Pattern | MAUI Pattern |
|----------------|--------------|
| `*ForegroundColor` | `*TextColor` |
| `*TextColor` | `*TextColor` (unchanged) |

### Data Binding Properties

| Xamarin | MAUI | Control |
|---------|------|---------|
| `SelectedItem` | `SelectedRow` | DataGrid |
| `SelectedItems` | `SelectedRows` | DataGrid |
| `CurrentItem` | `CurrentRow` | DataGrid |
| `DataSource` | `ItemsSource` | ComboBox, Autocomplete |
| `Date` | `SelectedDate` | DatePicker |
| `Time` | `SelectedTime` | TimePicker |

## Color to Brush Migration

### Why the Change?

MAUI uses `Brush` instead of `Color` for more flexibility (gradients, patterns, etc.).

### Migration Approaches

**Approach 1: Direct Color (Implicit Conversion)**
```csharp
// Xamarin
control.BackgroundColor = Color.Blue;

// MAUI (implicit conversion)
control.Background = Colors.Blue;
```

**Approach 2: Explicit SolidColorBrush**
```csharp
// MAUI
control.Background = new SolidColorBrush(Colors.Blue);
```

**Approach 3: Gradient Brush**
```csharp
// MAUI (new capability)
control.Background = new LinearGradientBrush
{
    StartPoint = new Point(0, 0),
    EndPoint = new Point(1, 1),
    GradientStops = new GradientStopCollection
    {
        new GradientStop(Colors.Blue, 0.0f),
        new GradientStop(Colors.Green, 1.0f)
    }
};
```

### Common Color/Brush Conversions

```csharp
// Xamarin → MAUI
Color.Red → Colors.Red
Color.FromHex("#FF0000") → Color.FromArgb("#FF0000")
Color.FromRgb(255, 0, 0) → Color.FromRgb(255, 0, 0)
```

## Event Argument Changes

### Event Argument Naming Pattern

Many event argument classes have control-specific prefixes in MAUI:

| Xamarin | MAUI | Control |
|---------|------|---------|
| `SelectionChangedEventArgs` | `CalendarSelectionChangedEventArgs` | Calendar |
| `SelectionChangedEventArgs` | `DataGridSelectionChangedEventArgs` | DataGrid |
| `TappedEventArgs` | `CalendarTappedEventArgs` | Calendar |
| `DayCellHoldingEventArgs` | `CalendarLongPressedEventArgs` | Calendar |
| `MonthChangedEventArgs` | `CalendarViewChangedEventArgs` | Calendar |

### Event Property Access Changes

**Xamarin:**
```csharp
selectionChanged += (s, e) =>
{
    var oldValue = e.OldValue;
    var newValue = e.NewValue;
};
```

**.NET MAUI:**
```csharp
selectionChanged += (s, e) =>
{
    var oldValue = e.OldValue;     // Same
    var newValue = e.NewValue;     // Same
    // Or control-specific properties
};
```

## Enum Renaming Patterns

### Adding Control Prefix

Many enums gained control-specific prefixes in MAUI:

| Xamarin Enum | MAUI Enum | Control |
|-------------|-----------|---------|
| `ColumnSizer` | `ColumnWidthMode` | DataGrid |
| `SelectionUnit` | `DataGridSelectionUnit` | DataGrid |
| `SortTapAction` | `DataGridSortingGestureType` | DataGrid |
| `SwipeDirection` | `DataGridSwipeDirection` | DataGrid |
| `SwipeOffsetMode` | `DataGridSwipeOffsetMode` | DataGrid |
| `ProgressStates` | `DataGridProgressState` | DataGrid |
| `AutoEllipsisMode` | `DataPagerEllipsisMode` | DataPager |
| `ButtonShape` | `DataPagerButtonShape` | DataPager |

### Enum Value Updates

**Xamarin:**
```csharp
dataGrid.SelectionUnit = SelectionUnit.Row;
```

**.NET MAUI:**
```csharp
dataGrid.SelectionUnit = DataGridSelectionUnit.Row;
```

## Breaking Changes by Version

### v20.2.x - Initial MAUI Release

- Handler registration required
- Namespace changes across all controls
- Color → Brush migration

### v20.3.x - API Consistency Updates

- Property naming standardization
- Event argument updates
- Enum prefixing

### v20.4.x and Later

- Continued refinements
- New features added
- Performance improvements

## Migration Checklist

### Pre-Migration

- [ ] Document all Syncfusion controls in use
- [ ] Review breaking changes for each control
- [ ] Backup current project
- [ ] Create migration branch

### During Migration

- [ ] Update target framework to .NET 9+
- [ ] Remove all Xamarin NuGet packages
- [ ] Install MAUI NuGet packages
- [ ] Add `.ConfigureSyncfusionCore()` to MauiProgram.cs
- [ ] Update all namespaces
- [ ] Update XAML namespace declarations
- [ ] Replace renamed controls (SfSchedule → SfScheduler, etc.)
- [ ] Update property names (SelectedItem → SelectedRow, etc.)
- [ ] Convert Color → Brush properties
- [ ] Update enum references
- [ ] Update event handler signatures
- [ ] Test incrementally per control

### Post-Migration

- [ ] Test all Syncfusion control functionality
- [ ] Verify data binding works correctly
- [ ] Test event handlers
- [ ] Check styling and theming
- [ ] Performance testing
- [ ] Full regression testing

## Troubleshooting Guide

### Handler Not Registered

**Error:** `Handler not found for view`

**Solution:**
```csharp
// Add to MauiProgram.cs
builder.ConfigureSyncfusionCore();
```

### Namespace Not Found

**Error:** `The type or namespace name '...' does not exist`

**Solutions:**
1. Verify correct NuGet package installed
2. Update namespace: `Syncfusion.*.XForms` → `Syncfusion.Maui.*`
3. Clean and rebuild solution
4. Check assembly reference in XAML

### Property Not Found

**Error:** `Property '...' does not exist`

**Solutions:**
1. Check migration guide for specific control
2. Common renames:
   - `SelectedItem` → `SelectedRow` (DataGrid)
   - `BackgroundColor` → `Background`
   - `DataSource` → `ItemsSource`
3. Search for property in MAUI API documentation

### Enum Not Found

**Error:** Enum type not recognized

**Solutions:**
1. Add control prefix: `SelectionUnit` → `DataGridSelectionUnit`
2. Check enum value names (may have changed)

### Color Assignment Error

**Error:** Cannot convert Color to Brush

**Solutions:**
```csharp
// Use implicit conversion
control.Background = Colors.Blue;

// Or explicit brush
control.Background = new SolidColorBrush(Colors.Blue);
```

### Build Errors After Package Update

**Solutions:**
1. Clean solution
2. Delete bin/ and obj/ folders
3. Rebuild solution
4. Restart Visual Studio
5. Check for package conflicts

### Runtime Crashes

**Common Causes:**
1. Handler not registered
2. Missing NuGet package
3. Incompatible control versions

**Solutions:**
1. Verify `.ConfigureSyncfusionCore()` called
2. Ensure all Syncfusion packages same version
3. Check platform-specific initialization

## Performance Optimization Tips

1. **Use Latest Versions:** MAUI controls are optimized over time
2. **Lazy Loading:** Load controls on demand
3. **Virtualization:** Use built-in virtualization in list controls
4. **Async Operations:** Use async for data loading
5. **Test on Devices:** Performance varies by platform

## Additional Resources

- **Official Documentation:** https://help.syncfusion.com/maui/
- **Migration Guides:** Per-control migration pages
- **Community Forums:** https://www.syncfusion.com/forums
- **KB Articles:** Search for specific issues

## Summary

Key migration steps:
1. Register handlers in MauiProgram.cs
2. Update NuGet packages
3. Update namespaces
4. Replace control names
5. Update properties (Color → Brush, SelectedItem → SelectedRow, etc.)
6. Update enums with prefixes
7. Test thoroughly

Most APIs remain similar to reduce migration effort, with changes made for MAUI consistency and improved functionality.
