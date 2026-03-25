# Data Controls Migration: Xamarin.Forms to .NET MAUI

Migration guide for Syncfusion data controls from Xamarin.Forms to .NET MAUI.

## Table of Contents
- [Overview](#overview)
- [SfDataGrid Migration](#sfdatagrid-migration)
- [SfDataForm Migration](#sfdataform-migration)
- [SfTreeView Migration](#sftreeview-migration)
- [SfTreeMap Migration](#sftreemap-migration)

## Overview

Data controls maintain most of their APIs but with consistency improvements for MAUI naming conventions.

## SfDataGrid Migration

### Namespace Changes

```csharp
// Xamarin
using Syncfusion.SfDataGrid.XForms;
using Syncfusion.Data;
using Syncfusion.SfDataGrid.XForms.DataPager;

// MAUI
using Syncfusion.Maui.DataGrid;
using Syncfusion.Maui.Data;
using Syncfusion.Maui.DataGrid.DataPager;
```

### Key Property Changes

| Xamarin | MAUI | Description |
|---------|------|-------------|
| `AutoGenerateColumns` = false | `AutoGenerateColumnsMode.None` | Disable auto columns |
| `AllowSorting` = true | `SortingMode.Single` | Enable single sorting |
| `AllowMultiSorting` = true | `SortingMode.Multiple` | Enable multiple sorting |
| `SortTapAction` | `SortingGestureType` | Sort gesture |
| `SelectedItem` | `SelectedRow` | Selected row |
| `SelectedItems` | `SelectedRows` | Multiple selection |
| `CurrentItem` | `CurrentRow` | Current navigated row |
| `IsHidden` | `Visible` | Column visibility (inverted) |
| `FrozenRowsCount` | `FrozenRowCount` | Frozen rows |
| `FrozenColumnsCount` | `FrozenColumnCount` | Frozen columns |
| `AllowResizingColumn` | `AllowResizingColumns` | Column resizing |

### Enum Changes

| Xamarin Enum | MAUI Enum |
|--------------|-----------|
| `ColumnSizer` | `ColumnWidthMode` |
| `SortTapAction` | `DataGridSortingGestureType` |
| `Position` | `SummaryRowPosition` |
| `SelectionUnit` | `DataGridSelectionUnit` |
| `SwipeDirection` | `DataGridSwipeDirection` |
| `SwipeOffsetMode` | `DataGridSwipeOffsetMode` |
| `ProgressStates` | `DataGridProgressState` |
| `QueryColumnDraggingReason` | `DataGridDragAction` |

### Event Changes

| Xamarin Event | MAUI Event |
|---------------|-----------|
| `GridTapped` | `CellTapped` |
| `GridDoubleTapped` | `CellDoubleTapped` |
| `GridLongPressed` | `CellLongPressed` |
| `GridTappedCommand` | `CellTappedCommand` |
| `GridDoubleTappedCommand` | `CellDoubleTappedCommand` |
| `GridLongPressedCommand` | `CellLongPressedCommand` |

### Style Property Changes

| Xamarin |MAUI | Description |
|---------|------|-------------|
| `HeaderBackgroundColor` | `HeaderRowBackground` | Header background |
| `HeaderForegroundColor` | `HeaderRowTextColor` | Header text color |
| `SelectionForegroundColor` | `SelectionTextColor` | Selection text |
| `ColumnDragViewForegroundColor` | `ColumnDragViewTextColor` | Drag text color |

### Migration Example

**Xamarin:**
```xml
<syncfusion:SfDataGrid ItemsSource="{Binding Orders}"
                       AutoGenerateColumns="False"
                       AllowSorting="True"
                       AllowGrouping="True"
                       SelectedItem="{Binding SelectedOrder}">
    <syncfusion:SfDataGrid.Columns>
        <syncfusion:GridTextColumn MappingName="OrderID" 
                                   HeaderText="Order ID"/>
        <syncfusion:GridTextColumn MappingName="CustomerName" 
                                   HeaderText="Customer"
                                   IsHidden="False"/>
    </syncfusion:SfDataGrid.Columns>
</syncfusion:SfDataGrid>
```

**.NET MAUI:**
```xml
<syncfusion:SfDataGrid ItemsSource="{Binding Orders}"
                       AutoGenerateColumnsMode="None"
                       SortingMode="Single"
                       AllowGrouping="True"
                       SelectedRow="{Binding SelectedOrder}">
    <syncfusion:SfDataGrid.Columns>
        <syncfusion:DataGridTextColumn MappingName="OrderID" 
                                       HeaderText="Order ID"/>
        <syncfusion:DataGridTextColumn MappingName="CustomerName" 
                                       HeaderText="Customer"
                                       Visible="True"/>
    </syncfusion:SfDataGrid.Columns>
</syncfusion:SfDataGrid>
```

**Code Behind:**

**Xamarin:**
```csharp
dataGrid.SelectionUnit = SelectionUnit.Row;
dataGrid.SelectedItem = orders[0];
dataGrid.AllowSorting = true;
```

**.NET MAUI:**
```csharp
dataGrid.SelectionUnit = DataGridSelectionUnit.Row;
dataGrid.SelectedRow = orders[0];
dataGrid.SortingMode = DataGridSortingMode.Single;
```

### DataPager Migration

**Namespace:**
```csharp
// Xamarin
using Syncfusion.SfDataGrid.XForms.DataPager;

// MAUI
using Syncfusion.Maui.DataGrid.DataPager;
```

**Property Changes:**

| Xamarin | MAUI | Description |
|---------|------|-------------|
| `AutoEllipsisMode` | `DataPagerEllipsisMode` | Ellipsis mode |
| `ButtonShape` | `DataPagerButtonShape` | Button shape |
| `NumericButtonsGenerateMode` | `DataPagerNumericButtonsGenerateMode` | Button generation |
| `PagerDisplayMode` | `DataPagerDisplayMode` | Display mode |
| `AppearanceManager` | `DefaultStyle` | Appearance customization |
| `EnableGridPaging()` | (removed) | Use `UseOnDemandPaging` only |

## SfDataForm Migration

### Namespace Changes

```csharp
// Xamarin
using Syncfusion.XForms.DataForm;

// MAUI
using Syncfusion.Maui.DataForm;
```

### Key Changes

Most APIs are similar with minor naming adjustments for consistency.

**Common Updates:**
- Color properties → Brush properties
- Event argument classes may have updated names
- Layout customization has enhanced options

### Migration Example

**Xamarin:**
```xml
<dataForm:SfDataForm DataObject="{Binding ContactInfo}"
                     LayoutOptions="Default"
                     ValidationMode="PropertyChanged"/>
```

**.NET MAUI:**
```xml
<dataForm:SfDataForm DataObject="{Binding ContactInfo}"
                     LayoutType="Default"
                     ValidationMode="PropertyChanged"/>
```

## SfTreeView Migration

### Namespace Changes

```csharp
// Xamarin
using Syncfusion.XForms.TreeView;

// MAUI
using Syncfusion.Maui.TreeView;
```

### Key Property Changes

| Xamarin | MAUI | Description |
|---------|------|-------------|
| `ItemHeight` | `ItemHeight` | Same |
| `ChildPropertyName` | `ChildPropertyName` | Same |
| `SelectedItem` | `SelectedItem` | Same (maintained) |
| `SelectedItems` | `SelectedItems` | Same (maintained) |

**Note:** TreeView maintains high API compatibility.

### Migration Example

**Xamarin:**```xml
<treeView:SfTreeView ItemsSource="{Binding Folders}"
                     ChildPropertyName="SubFolders"
                     AllowExpanding="True"/>
```

**.NET MAUI:**
```xml
<treeView:SfTreeView ItemsSource="{Binding Folders}"
                     ChildPropertyName="SubFolders"
                     AllowExpanding="True"/>
```

## SfTreeMap Migration

### Namespace Changes

```csharp
// Xamarin
using Syncfusion.SfTreeMap.XForms;

// MAUI
using Syncfusion.Maui.TreeMap;
```

### Key Changes

Most properties maintained with updated Brush/Color handling.

### Migration Example

**Xamarin:**
```xml
<treeMap:SfTreeMap WeightValuePath="Population"
                   ColorValuePath="GrowthRate"
                   LeafItemSettings="{Binding LeafSettings}"/>
```

**.NET MAUI:**
```xml
<treeMap:SfTreeMap WeightValuePath="Population"
                   ColorValuePath="GrowthRate"
                   LeafItemSettings="{Binding LeafSettings}"/>
```

## Common Migration Patterns

### Selection Handling

**Xamarin:**
```csharp
dataGrid.SelectionChanged += (s, e) =>
{
    var selectedItem = dataGrid.SelectedItem;
    // Use selectedItem
};
```

**.NET MAUI:**
```csharp
dataGrid.SelectionChanged += (s, e) =>
{
    var selectedRow = dataGrid.SelectedRow;
    // Use selectedRow
};
```

### Column Visibility

**Xamarin:**
```csharp
column.IsHidden = false;  // Visible
column.IsHidden = true;   // Hidden
```

**.NET MAUI:**
```csharp
column.Visible = true;   // Visible
column.Visible = false;  // Hidden
```

### Sorting

**Xamarin:**
```csharp
dataGrid.AllowSorting = true;
dataGrid.AllowMultiSorting = true;
```

**.NET MAUI:**
```csharp
dataGrid.SortingMode = DataGridSortingMode.Multiple;
```

## Troubleshooting

### Issue: SelectedItem not found

**Solution:** Use `SelectedRow` instead:
```csharp
// Change
var item = dataGrid.SelectedItem;

// To
var item = dataGrid.SelectedRow;
```

### Issue: IsHidden property not found

**Solution:** Use `Visible` property (inverted logic):
```csharp
// Change
column.IsHidden = false;

// To
column.Visible = true;
```

### Issue: AllowSorting not working as expected

**Solution:** Use `SortingMode`:
```csharp
// Change
dataGrid.AllowSorting = true;

// To
dataGrid.SortingMode = DataGridSortingMode.Single;
// Or for multiple
dataGrid.SortingMode = DataGridSortingMode.Multiple;
```

### Issue: Enum not found

**Solution:** Update enum names with `DataGrid` prefix:
- `ColumnSizer` → `ColumnWidthMode`
- `SelectionUnit` → `DataGridSelectionUnit`
- `SwipeDirection` → `DataGridSwipeDirection`

## Next Steps

1. Update NuGet packages:
   - `Syncfusion.Maui.DataGrid`
   - `Syncfusion.Maui.DataForm`
   - `Syncfusion.Maui.TreeView`
   - `Syncfusion.Maui.TreeMap`
2. Update namespaces
3. Replace `SelectedItem` → `SelectedRow` in DataGrid
4. Update sorting properties
5. Update column visibility logic
6. Update enum references
7. Test data binding and interactions
