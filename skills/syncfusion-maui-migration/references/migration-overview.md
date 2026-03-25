# Migration Overview: Xamarin.Forms to .NET MAUI Components

Complete component mapping and migration roadmap for Syncfusion components transitioning from Xamarin.Forms to .NET MAUI.

## Table of Contents
- [Overview](#overview)
- [Complete Component Mapping Table](#complete-component-mapping-table)
- [Migration Workflow](#migration-workflow)
- [Package and Namespace Changes](#package-and-namespace-changes)
- [Obsolete Components](#obsolete-components)
- [Components Under Development](#components-under-development)

## Overview

Syncfusion delivers a comprehensive suite of .NET MAUI controls designed with minimal breaking changes from Xamarin.Forms. All essential Xamarin.Forms controls and features are fully available in .NET MAUI, enabling a smooth, reliable, and seamless migration experience.

**Key Migration Principles:**
- Most APIs preserved for easier migration
- Consistent API naming across MAUI platform
- Handler registration required in MauiProgram.cs
- Package names updated from `Syncfusion.Xamarin.*` to `Syncfusion.Maui.*`
- Namespace pattern: `Syncfusion.*.XForms` → `Syncfusion.Maui.*`

## Complete Component Mapping Table

### Charts and Visualizations

| Xamarin.Forms Component | .NET MAUI Component | Notes |
|------------------------|---------------------|-------|
| SfChart | SfCartesianChart | Split into 5 chart types |
| SfChart | SfCircularChart | Pie, Doughnut charts |
| SfChart | SfFunnelChart | Funnel visualization |
| SfChart | SfPyramidChart | Pyramid visualization |
| SfChart | SfPolarChart | Polar, Radar charts |
| SfSunburstChart | SfSunburstChart | Direct migration |

### Gauges

| Xamarin.Forms Component | .NET MAUI Component | Notes |
|------------------------|---------------------|-------|
| SfCircularGauge | SfRadialGauge | Renamed for clarity |
| SfLinearGauge | SfLinearGauge | Direct migration |
| SfDigitalGauge | SfDigitalGauge | Direct migration |

### Data Grids and Forms

| Xamarin.Forms Component | .NET MAUI Component | Notes |
|------------------------|---------------------|-------|
| SfDataGrid | SfDataGrid | API consistency updates |
| SfDataForm | SfDataForm | Form layout updates |
| SfTreeView | SfTreeView | Hierarchical data |
| SfTreeMap | SfTreeMap | Tree map visualization |

### Calendar and Scheduling

| Xamarin.Forms Component | .NET MAUI Component | Notes |
|------------------------|---------------------|-------|
| SfCalendar | SfCalendar | Selection-focused (no appointments) |
| SfSchedule | SfScheduler | Renamed, appointment management |
| SfDatePicker | SfDatePicker | Direct migration |
| SfTimePicker | SfTimePicker | Direct migration |
| SfDateTimeRangeNavigator | SfDateTimeRangeSelector | Renamed |

### Input Controls

| Xamarin.Forms Component | .NET MAUI Component | Notes |
|------------------------|---------------------|-------|
| SfMaskedEdit | SfMaskedEntry | Renamed |
| SfComboBox | SfComboBox | Direct migration |
| SfAutoComplete | SfAutocomplete | Direct migration |
| SfPicker | SfPicker | Direct migration |
| SfNumericTextBox | SfNumericEntry | Renamed |
| SfNumericUpDown | SfNumericEntry | Merged into SfNumericEntry |
| SfTextInputLayout | SfTextInputLayout | Direct migration |

### Buttons and Selection

| Xamarin.Forms Component | .NET MAUI Component | Notes |
|------------------------|---------------------|-------|
| SfButton | SfButton | Direct migration |
| SfCheckBox | SfCheckBox | Direct migration |
| SfRadioButton | SfRadioButton | Direct migration |
| SfChips | SfChips | Direct migration |
| SfSegmentedControl | SfSegmentedControl | Direct migration |

### Navigation and Lists

| Xamarin.Forms Component | .NET MAUI Component | Notes |
|------------------------|---------------------|-------|
| SfListView | SfListView | Direct migration |
| SfAccordion | SfAccordion | Direct migration |
| SfExpander | SfExpander | Direct migration |
| SfTabView | SfTabView | Direct migration |
| SfNavigationDrawer | SfNavigationDrawer | Direct migration |

### Progress Indicators

| Xamarin.Forms Component | .NET MAUI Component | Notes |
|------------------------|---------------------|-------|
| SfLinearProgressBar | SfLinearProgressBar | Direct migration |
| SfCircularProgressBar | SfCircularProgressBar | Direct migration |
| SfStepProgressBar | SfStepProgressBar | Direct migration |
| SfBusyIndicator | SfBusyIndicator | Direct migration |

### Visual and Effects

| Xamarin.Forms Component | .NET MAUI Component | Notes |
|------------------------|---------------------|-------|
| SfBarcodeGenerator | SfBarcodeGenerator | Renamed from SfBarcode |
| SfShimmer | SfShimmer | Direct migration |
| SfEffectsView | SfEffectsView | Direct migration |
| SfBadgeView | SfBadgeView | Direct migration |
| SfAvatarView | SfAvatarView | Direct migration |
| SfRating | SfRating | Direct migration |

### Layout and Containers

| Xamarin.Forms Component | .NET MAUI Component | Notes |
|------------------------|---------------------|-------|
| SfBackdropPage | SfBackdropPage | Direct migration |
| SfCardView | SfCardView | Direct migration |
| SfParallaxView | SfParallaxView | Direct migration |
| SfPopUpLayout | SfPopup | Renamed, simplified |

### Interactive Controls

| Xamarin.Forms Component | .NET MAUI Component | Notes |
|------------------------|---------------------|-------|
| SfSignaturePad | SfSignaturePad | Direct migration |
| SfChat | SfChat | Direct migration |
| SfImageEditor | SfImageEditor | Direct migration |
| SfPullToRefresh | SfPullToRefresh | Direct migration |

### Sliders and Range Controls

| Xamarin.Forms Component | .NET MAUI Component | Notes |
|------------------------|---------------------|-------|
| SfRangeSlider | SfSlider | Single value slider |
| SfRangeSlider | SfRangeSlider | Range selection |

### Other Controls

| Xamarin.Forms Component | .NET MAUI Component | Notes |
|------------------------|---------------------|-------|
| SfMaps | SfMaps | Direct migration |
| SfRadialMenu | SfRadialMenu | Direct migration |
| SfKanban | SfKanban | Direct migration |
| SfCarousel | SfCarousel | Direct migration |
| SfRotator | SfRotator | Direct migration |
| SfSwitch | SfSwitch | Direct migration |
| SfPdfViewer | SfPdfViewer | Direct migration |
| SfRichTextEditor | SfRichTextEditor | Direct migration |

## Obsolete Components

These Xamarin.Forms components are obsolete in .NET MAUI. Use native MAUI controls instead:

| Xamarin.Forms Component | .NET MAUI Replacement | Reason |
|------------------------|----------------------|--------|
| SfBorder | Native MAUI `Border` | Built into MAUI |
| SfGradientView | Native MAUI `Gradients` | Built into MAUI |
| SfDiagram | Syncfusion Blazor Diagram | Use Blazor component in MAUI |

## Migration Workflow

### Step 1: Inventory Your Components

List all Syncfusion components currently used in your Xamarin.Forms application.

```csharp
// Example: Document components
- SfDataGrid (2 instances)
- SfCalendar (1 instance)
- SfChart (3 instances - will become SfCartesianChart)
```

### Step 2: Review Breaking Changes

For each component, check the specific migration reference for breaking changes:
- Property renames
- Enum changes
- Event handler updates
- Namespace updates

### Step 3: Update NuGet Packages

Remove Xamarin packages and install MAUI equivalents:

```bash
# Remove
dotnet remove package Syncfusion.Xamarin.SfDataGrid

# Add
dotnet add package Syncfusion.Maui.DataGrid
```

### Step 4: Register Handlers

Add handler registration to `MauiProgram.cs`:

```csharp
using Syncfusion.Maui.Core.Hosting;

builder.ConfigureSyncfusionCore();
```

### Step 5: Update Namespaces

Change all namespace imports:

```csharp
// Old
using Syncfusion.SfDataGrid.XForms;

// New
using Syncfusion.Maui.DataGrid;
```

### Step 6: Update XAML Declarations

```xml
<!-- Old -->
xmlns:syncfusion="clr-namespace:Syncfusion.SfDataGrid.XForms;assembly=Syncfusion.SfDataGrid.XForms"

<!-- New -->
xmlns:syncfusion="clr-namespace:Syncfusion.Maui.DataGrid;assembly=Syncfusion.Maui.DataGrid"
```

### Step 7: Update API Calls

Check component-specific migration guides for property and method changes.

### Step 8: Test Incrementally

Test each migrated component before proceeding to the next.

## Package and Namespace Changes

### Package Name Pattern

| Xamarin Package Pattern | MAUI Package Pattern |
|------------------------|---------------------|
| `Syncfusion.Xamarin.*` | `Syncfusion.Maui.*` |
| `Syncfusion.Xamarin.SfDataGrid` | `Syncfusion.Maui.DataGrid` |
| `Syncfusion.Xamarin.SfChart` | `Syncfusion.Maui.Charts` |

### Namespace Pattern

| Xamarin Namespace | MAUI Namespace |
|-------------------|----------------|
| `Syncfusion.SfDataGrid.XForms` | `Syncfusion.Maui.DataGrid` |
| `Syncfusion.SfChart.XForms` | `Syncfusion.Maui.Charts` |
| `Syncfusion.XForms.Buttons` | `Syncfusion.Maui.Buttons` |
| `Syncfusion.SfCalendar.XForms` | `Syncfusion.Maui.Calendar` |
| `Syncfusion.Data` | `Syncfusion.Maui.Data` |

### Common Namespace Examples

```csharp
// Charts
using Syncfusion.SfChart.XForms;         // Xamarin
using Syncfusion.Maui.Charts;            // MAUI

// DataGrid
using Syncfusion.SfDataGrid.XForms;      // Xamarin
using Syncfusion.Maui.DataGrid;          // MAUI

// Calendar
using Syncfusion.SfCalendar.XForms;      // Xamarin
using Syncfusion.Maui.Calendar;          // MAUI

// Buttons
using Syncfusion.XForms.Buttons;         // Xamarin
using Syncfusion.Maui.Buttons;           // MAUI

// Sliders
using Syncfusion.SfSlider.XForms;        // Xamarin
using Syncfusion.Maui.Sliders;           // MAUI
```

## Version Compatibility

- **.NET MAUI requires:** .NET 9 SDK or later
- **Visual Studio:** 2026 (v18.0.0 or later) or Visual Studio Code
- **Syncfusion MAUI:** v27.2+

## Next Steps

After reviewing this overview:
1. Navigate to the specific component category migration guide
2. Review breaking changes for your components
3. Follow the component-specific migration steps
4. Test thoroughly after each component migration
