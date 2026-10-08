---
name: syncfusion-maui-treegrid
description: Implement and customize Syncfusion .NET MAUI Tree Grid (SfTreeGrid) for displaying and manipulating hierarchical data in a tabular view. Use when working with MAUI tree grids, SfTreeGrid, hierarchical data binding, tree grid columns, sorting, filtering, selection, freeze panes, or expander customization.
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
---

# Implementing .NET MAUI Tree Grid

A skill for implementing and customizing the Syncfusion .NET MAUI Tree Grid (`SfTreeGrid`) control. The Tree Grid displays and manipulates hierarchical data in a tabular view, combining the clarity of a grid with the ability to present parent-child relationships in an organized structure. Built for excellent performance, it handles large datasets efficiently while maintaining smooth interaction.

## When to Use This Skill

Use this skill when you need to:
- Display hierarchical or self-relational data in a .NET MAUI app
- Bind nested collections or self-referencing data sources to a tree grid
- Configure tree grid columns (auto-generated or manually defined)
- Choose and configure column types (Text, Numeric, Date, CheckBox, Template)
- Apply column sizing modes (Fill, Auto, LastColumnFill, FitByCell, FitByHeader)
- Sort or filter hierarchical data
- Freeze columns for navigation across wide datasets
- Implement row selection (Single, Multiple, SingleDeselect, None)
- Customize the expander column (icon, width, placement, initial expansion state)
- Style cells, headers, and rows (DefaultStyle, implicit styling, grid lines)
- Expand/collapse nodes programmatically

## Component Overview

**SfTreeGrid** is a high-performance hierarchical data grid for .NET MAUI that provides:

**Core Features:**
- Two data binding modes — self-relational (ParentPropertyName/ChildPropertyName) and nested collection (ChildPropertyName)
- Rich column types — Text, Numeric, Date, CheckBox, Template
- Auto-generated or manually defined columns
- Data operations — sorting (single/multi-column, tri-state, custom) and filtering (programmatic, condition-based)
- Freeze panes — left and right column freezing
- Selection — Single, Multiple, SingleDeselect, None
- Expander customization — icon templates, column placement, width, model-driven expansion state
- Styling — column-level, implicit (TargetType), DefaultStyle, grid line control

**NuGet Package:** `Syncfusion.Maui.TreeGrid`

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Prerequisites and project setup (.NET 9 SDK, Visual Studio 2022 v17.12+)
- Installing the Syncfusion.Maui.TreeGrid NuGet package
- Registering the Syncfusion core handler in MauiProgram.cs
- Defining data models and ViewModels
- Basic TreeGrid implementation (XAML and C#)
- Importing the TreeGrid namespace

### Data Binding
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- Binding self-relational data (ParentPropertyName, ChildPropertyName, SelfRelationRootValue)
- Binding nested collections (ChildPropertyName)
- Binding IEnumerable sources
- AutoExpandMode (None, RootNodesExpanded, AllNodesExpanded)
- Programmatic node expansion (ExpandAllNodes, ExpandNode, by level/index/business object)
- Programmatic node collapse (CollapseAllNodes, CollapseNode)
- NodeExpanding and NodeCollapsing events
- IsExpanded property on TreeNode

### Columns
📄 **Read:** [references/columns.md](references/columns.md)
- Automatic column generation (AutoGenerateColumnsMode)
- Auto-generation modes (None, Reset, ResetAll, RetainOld, SmartReset)
- Customizing auto-generated columns (AutoGeneratingColumn event)
- Manually generating columns
- Column manipulation (add, access, clear, remove)
- Column-to-data-type mapping

### Column Types
📄 **Read:** [references/column-types.md](references/column-types.md)
- TreeGridColumn base properties (MappingName, HeaderText, Width, Format)
- TreeGridTextColumn, TreeGridNumericColumn, TreeGridDateColumn, TreeGridCheckBoxColumn, TreeGridTemplateColumn
- DisplayBinding and value converters
- Text alignment (HeaderTextAlignment, CellTextAlignment)
- Padding (HeaderPadding, CellPadding)
- Header customization (HeaderText, HeaderTemplate)
- Hiding columns (Visible)
- Formatting values (C2, N2, P2, date formats)

### Column Sizing
📄 **Read:** [references/column-sizing.md](references/column-sizing.md)
- ColumnWidthMode options (Fill, Auto, LastColumnFill, FitByCell, FitByHeader, None)
- DefaultColumnWidth
- Retrieving auto-calculated width (ActualWidth)
- Per-column ColumnWidthMode
- Minimum and maximum column widths (MinimumWidth, MaximumWidth)

### Expander Customization
📄 **Read:** [references/expander-customization.md](references/expander-customization.md)
- Loading expander icon through DataTemplate (ExpanderIcon)
- Using DataTemplateSelector for expanded/collapsed icons
- Changing the expander column (ExpanderColumn)
- Customizing expander column width (ExpanderWidth)
- Controlling initial expansion through a model property (ExpandStateMappingName)

### Sorting & Filtering
📄 **Read:** [references/sorting-filtering.md](references/sorting-filtering.md)
- Programmatic sorting (SortColumnDescriptions, SortColumnDescription)
- Sorting modes (Single, Multiple, None)
- Tri-state sorting (AllowTriStateSorting)
- Showing sort numbers (ShowSortNumbers)
- Sorting gesture (tap vs double-tap, SortingGestureType)
- Sorting events (SortColumnsChanging, SortColumnsChanged)
- Disabling sorting per column (AllowSorting)
- Custom sorting (SortComparers, IComparer, ISortDirection)
- Filter levels (Root, All, Extended)
- Programmatic view filtering (View.Filter, View.RefreshFilter)
- Condition-based filtering (Equals, Contains, Does Not Equal)
- Clearing filters

### Selection
📄 **Read:** [references/selection.md](references/selection.md)
- Selection modes (None, Single, Multiple, SingleDeselect)
- Getting selected rows (SelectedIndex, SelectedRow, SelectedRows)
- Programmatic selection (index, row, multiple rows, SelectAll)
- Clearing selection (SelectionMode.None, ClearSelection)
- Selection events (SelectionChanging, SelectionChanged)
- Customizing selection appearance (SelectionBackground, SelectedRowTextColor)
- Binding selection properties to ViewModel

### Freeze Panes
📄 **Read:** [references/freeze-panes.md](references/freeze-panes.md)
- Freezing left columns (FrozenColumnCount)
- Freezing right/footer columns (FooterFrozenColumnCount)
- Freeze pane line color (FreezePaneLineColor)
- Freeze pane line thickness (FreezePaneLineStrokeThickness)

### Styling
📄 **Read:** [references/styling.md](references/styling.md)
- Column cell styling (CellStyle)
- Column header styling (HeaderStyle)
- Implicit styling (TreeGridCell, TreeGridExpanderCell, TreeGridHeaderCell, TreeGridRow, TreeGridHeaderRow)
- DefaultStyle (HeaderRowBackground, RowBackground, RowTextColor)
- Applying styles from app/page resources
- Grid line visibility (Both, Horizontal, Vertical, None)
- Grid line color and stroke customization
- Styling priority order

## Quick Start

A minimal TreeGrid bound to a nested collection:

```csharp
using Syncfusion.Maui.TreeGrid;
using System.Collections.ObjectModel;

public class EmployeeInfo
{
    public string? FirstName { get; set; }
    public string? LastName { get; set; }
    public double Salary { get; set; }
    public ObservableCollection<EmployeeInfo> Children { get; set; } = new();
}

SfTreeGrid treeGrid = new SfTreeGrid();
treeGrid.ItemsSource = new ObservableCollection<EmployeeInfo>
{
    new EmployeeInfo
    {
        FirstName = "James", LastName = "Smith", Salary = 2000000,
        Children = new ObservableCollection<EmployeeInfo>
        {
            new EmployeeInfo { FirstName = "Andrew", LastName = "Fuller", Salary = 1200000 }
        }
    }
};
treeGrid.ChildPropertyName = "Children";
this.Content = treeGrid;
```

**Namespace (XAML):**

```xaml
xmlns:syncfusion="clr-namespace:Syncfusion.Maui.TreeGrid;assembly=Syncfusion.Maui.TreeGrid"
```

**Don't forget** to register the Syncfusion core handler in `MauiProgram.cs`:

```csharp
builder.ConfigureSyncfusionCore();
```

Forgetting `ConfigureSyncfusionCore()` causes runtime errors when rendering the TreeGrid.

## Common Patterns

### Display hierarchical data with a specific column

```xaml
<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding EmployeeCollection}"
                       ChildPropertyName="Children"
                       AutoGenerateColumnsMode="None">
    <syncfusion:SfTreeGrid.Columns>
        <syncfusion:TreeGridTextColumn HeaderText="First Name" MappingName="FirstName" />
        <syncfusion:TreeGridTextColumn HeaderText="Last Name" MappingName="LastName" />
        <syncfusion:TreeGridNumericColumn HeaderText="Salary" MappingName="Salary" Format="C2" />
    </syncfusion:SfTreeGrid.Columns>
</syncfusion:SfTreeGrid>
```

### Bind self-relational data

```csharp
treeGrid.ItemsSource = employeeViewModel.Employees;
treeGrid.ParentPropertyName = "ID";
treeGrid.ChildPropertyName = "ReportsTo";
treeGrid.SelfRelationRootValue = -1;
treeGrid.AutoExpandMode = TreeGridExpandMode.RootNodesExpanded;
```

### Apply default styling

```csharp
treeGrid.DefaultStyle.HeaderRowBackground = Color.FromArgb("#0074E3");
treeGrid.DefaultStyle.HeaderRowTextColor = Colors.White;
treeGrid.DefaultStyle.RowBackground = Color.FromArgb("#AFD5FB");
treeGrid.DefaultStyle.RowTextColor = Color.FromArgb("#212121");
```

### Choosing the right reference

| User wants to... | Read |
|---|---|
| Set up the TreeGrid for the first time | `references/getting-started.md` |
| Bind self-relational or nested data | `references/data-binding.md` |
| Auto-generate or manually define columns | `references/columns.md` |
| Pick a column type or format cell values | `references/column-types.md` |
| Control column widths automatically | `references/column-sizing.md` |
| Customize the expand/collapse icon or column | `references/expander-customization.md` |
| Sort or filter hierarchical data | `references/sorting-filtering.md` |
| Select rows or respond to selection changes | `references/selection.md` |
| Keep columns visible during horizontal scroll | `references/freeze-panes.md` |
| Style cells, headers, rows, or grid lines | `references/styling.md` |
