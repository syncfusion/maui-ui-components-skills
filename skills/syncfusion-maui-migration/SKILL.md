---
name: syncfusion-maui-migration
description: Migrates Syncfusion components from Xamarin.Forms to .NET MAUI. Use when migrating Xamarin.Forms applications to .NET MAUI, addressing namespace changes, API renames, property migrations, or breaking changes. Covers SfChart to SfCartesianChart migration, handler registration, NuGet package updates, and enum updates.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Syncfusion Xamarin.Forms to .NET MAUI Migration

A comprehensive skill for migrating Syncfusion components from Xamarin.Forms to .NET MAUI. This skill provides detailed guidance on API changes, namespace updates, property renames, and migration patterns across all Syncfusion component categories.

## When to Use This Skill

Use this skill when you need to:
- Migrate Syncfusion Xamarin.Forms components to .NET MAUI
- Update namespaces from `Syncfusion.*.XForms` to `Syncfusion.Maui.*`
- Understand API changes between Xamarin and MAUI versions
- Find MAUI equivalents for Xamarin components
- Resolve breaking changes during migration
- Update property names, events, and enums
- Register handlers in MauiProgram.cs
- Update NuGet package references
- Fix compilation errors after upgrading
- Understand which Xamarin components are obsolete

## Migration Overview

Syncfusion actively delivers .NET MAUI controls with minimal breaking changes from Xamarin.Forms. All essential Xamarin.Forms controls and features are fully available in .NET MAUI, enabling a smooth, reliable, and seamless migration experience.

**Key Migration Aspects:**
- **Namespace Pattern**: `Syncfusion.XForms.*` → `Syncfusion.Maui.*`
- **Handler Registration**: Required in `MauiProgram.cs` using `.ConfigureSyncfusionCore()`
- **API Consistency**: Most APIs preserved with some renames for MAUI consistency
- **Package Names**: Updated from `Syncfusion.Xamarin.*` to `Syncfusion.Maui.*`
- **Breaking Changes**: Property renames (e.g., `BackgroundColor` → `Background`), enum updates
- **Obsolete Components**: Some components replaced by native MAUI controls (SfBorder, SfGradientView)

## Documentation and Navigation Guide

### Migration Overview and Component Index
📄 **Read:** [references/migration-overview.md](references/migration-overview.md)
- Complete component mapping table (Xamarin → MAUI)
- Migration roadmap and timeline
- General migration workflow
- Package and namespace changes overview
- Obsolete components and their alternatives
- Version compatibility information

### Charts Migration
📄 **Read:** [references/charts-migration.md](references/charts-migration.md)
- SfCartesianChart (from SfChart)
- SfCircularChart migration
- SfPolarChart migration
- SfFunnelChart migration
- SfPyramidChart migration
- Chart namespace and enum updates
- Series and axis property changes
- Legend and tooltip updates

### Gauges Migration
📄 **Read:** [references/gauges-migration.md](references/gauges-migration.md)
- SfRadialGauge (from SfCircularGauge)
- SfLinearGauge migration
- SfDigitalGauge migration
- Pointer and scale property mappings
- Annotation changes
- Styling updates

### Data Controls Migration
📄 **Read:** [references/data-controls-migration.md](references/data-controls-migration.md)
- SfDataGrid migration (selection, sorting, filtering)
- SfDataForm migration
- SfTreeView migration
- SfTreeMap migration
- Property renames (SelectedItem → SelectedRow)
- Event handler updates
- Enum changes (ColumnSizer → ColumnWidthMode)

### Calendar and Scheduling Controls
📄 **Read:** [references/calendar-scheduling-migration.md](references/calendar-scheduling-migration.md)
- SfCalendar migration (selection changes)
- SfScheduler (from SfSchedule)
- SfDatePicker migration
- SfTimePicker migration
- SfDateTimeRangePicker updates
- SfDateTimeRangeSelector (from SfDateTimeRangeNavigator)
- Appointment management differences

### Input and Picker Controls
📄 **Read:** [references/input-picker-migration.md](references/input-picker-migration.md)
- SfMaskedEntry (from SfMaskedEdit)
- SfComboBox migration
- SfAutocomplete migration
- SfPicker migration
- SfTextInputLayout migration
- Input validation changes
- Dropdown behavior updates

### Navigation and List Controls
📄 **Read:** [references/navigation-list-migration.md](references/navigation-list-migration.md)
- SfListView migration
- SfSegmentedControl migration
- SfAccordion migration
- SfExpander migration
- SfTabView migration
- Selection handling changes
- ItemTemplate updates

### Progress and Visual Indicators
📄 **Read:** [references/progress-visual-migration.md](references/progress-visual-migration.md)
- SfLinearProgressBar migration
- SfCircularProgressBar migration
- SfStepProgressBar migration
- SfShimmer migration
- SfBarcodeGenerator (from SfBarcode)
- Animation and styling updates
- Progress state changes

### Layout and Container Controls
📄 **Read:** [references/layout-container-migration.md](references/layout-container-migration.md)
- SfBackdropPage migration
- SfCards migration
- SfParallaxView migration
- SfPopup (from SfPopUpLayout)
- Layout property changes
- Obsolete controls (SfBorder → Border, SfGradientView → Gradients)

### Interactive and Drawing Controls
📄 **Read:** [references/interactive-drawing-migration.md](references/interactive-drawing-migration.md)
- SfSignaturePad migration
- SfChat migration
- SfSlider migration
- SfRangeSlider migration
- SfRadialMenu migration
- Pull-to-Refresh migration
- Touch and gesture changes

### Maps and Location Services
📄 **Read:** [references/maps-migration.md](references/maps-migration.md)
- SfMaps migration
- Layer configuration changes
- Marker and data binding updates
- Shape file handling
- GeoJSON support changes

### Common Migration Patterns
📄 **Read:** [references/common-migration-patterns.md](references/common-migration-patterns.md)
- Handler registration in MauiProgram.cs
- NuGet package name changes
- Namespace patterns across all controls
- Common property renames
- Color and Brush API changes
- Event argument class updates
- Enum renaming patterns
- Breaking changes by version
- Migration troubleshooting guide

