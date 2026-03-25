---
name: syncfusion-maui-datagrid
description: Implements and customize Syncfusion .NET MAUI DataGrid (SfDataGrid) for displaying tabular data. Use when working with MAUI data grids, SfDataGrid, tabular data display, data binding to grids, or column configuration. Covers editing cells, sorting, filtering, grouping, paging, exporting to Excel/PDF, row operations, selection, and summaries.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing .NET MAUI DataGrid

A comprehensive skill for implementing and customizing the Syncfusion .NET MAUI DataGrid (SfDataGrid) control. The DataGrid displays and manipulates data in a tabular format with support for columns, editing, sorting, filtering, grouping, summaries, paging, and export features.

## When to Use This Skill

Use this skill when you need to:
- Display tabular data in a .NET MAUI application
- Bind data sources (IEnumerable, ObservableCollection, DataTable) to a grid
- Configure columns (auto-generated or manual)
- Implement cell editing and validation
- Add sorting, filtering, or searching functionality
- Group data and display summaries/aggregates
- Implement paging or incremental loading
- Enable row operations (drag-drop, swipe, add/delete)
- Export grid data to Excel or PDF
- Customize grid appearance and behavior
- Optimize performance for large datasets
- Implement master-detail views or custom row templates

## Component Overview

**SfDataGrid** is a high-performance data grid control for .NET MAUI that provides:

**Core Features:**
- Multiple data binding options (IEnumerable, DataTable, dynamic objects)
- Auto-generated or manually defined columns
- 8+ column types (Text, Numeric, Date, Checkbox, Image, ComboBox, Picker, Template)
- In-cell editing with validation
- Multi-column sorting and filtering
- Interactive grouping with summaries
- Selection modes (Single, Multiple, None)
- Paging and data virtualization
- Excel and PDF export
- Advanced views (Master-Details, Record Templates)

**NuGet Package:** `Syncfusion.Maui.DataGrid`

## Documentation and Navigation Guide

### Getting Started & Data Binding
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and NuGet package setup
- Handler registration in MauiProgram.cs
- Basic DataGrid implementation (XAML & C#)
- Creating data models and ViewModels
- Binding with IEnumerable and ObservableCollection
- Binding with DataTable and dynamic objects
- Initial configuration and first steps

### Columns & Column Configuration
📄 **Read:** [references/columns.md](references/columns.md)
- Auto-generating columns (AutoGenerateColumnsMode)
- Manually defining columns
- Column types (Text, Numeric, Date, Checkbox, Image, ComboBox, Picker, Template)
- Column properties (MappingName, HeaderText, Format, Width)
- Column sizing and width options
- Column visibility and ordering
- AutoGenerateColumnsMode options (None, Reset, ResetAll, RetainOld)

### Column Operations
📄 **Read:** [references/column-operations.md](references/column-operations.md)
- Column resizing (manual and auto-fit)
- Column drag and drop (reordering)
- Stacked headers (multi-level column headers)
- Unbound columns (calculated/expression columns)
- Freeze panes (frozen columns)
- Column customization and events

### Cell Editing
📄 **Read:** [references/editing.md](references/editing.md)
- Enabling editing (AllowEditing, NavigationMode, SelectionMode)
- Column-level editing control
- Edit modes and triggers
- Cell editing events (BeginEdit, EndEdit, CellValueChanged)
- Programmatic editing
- Enter key and Tab navigation during editing

### Data Validation
📄 **Read:** [references/data-validation.md](references/data-validation.md)
- Cell value validation
- Row-level validation
- Custom validation logic
- Validation events
- Error handling and display
- IDataErrorInfo support

### Sorting & Filtering
📄 **Read:** [references/sorting-filtering.md](references/sorting-filtering.md)
- Single and multi-column sorting
- Custom sorting logic
- Programmatic sorting
- Sort icons and UI customization
- Filtering basics and filter types
- Filter row implementation
- Programmatic filtering
- Custom filter predicates

### Searching
📄 **Read:** [references/searching.md](references/searching.md)
- Search functionality setup
- Text search and highlighting
- Case sensitivity options
- Search navigation (next/previous)
- Programmatic search control

### Grouping & Summaries
📄 **Read:** [references/grouping-summaries.md](references/grouping-summaries.md)
- Data grouping (single and multi-column)
- Group expand/collapse behavior
- Custom grouping logic
- Group header customization
- Summary rows (Table, Group, Caption summaries)
- Built-in aggregate functions (Sum, Average, Count, Min, Max)
- Custom summary calculations
- Summary display formatting

### Selection & Navigation
📄 **Read:** [references/selection.md](references/selection.md)
- Selection modes (Single, Multiple, SingleDeselect, None)
- Row and cell selection
- Selection events (SelectionChanging, SelectionChanged)
- Programmatic selection
- Current cell vs selected items
- Keyboard navigation (Windows platform)

### Paging & Data Virtualization
📄 **Read:** [references/paging-virtualization.md](references/paging-virtualization.md)
- Paging setup and configuration
- Page size and page count
- Page navigation controls
- Custom paging UI
- Load More (incremental loading)
- Pull to Refresh
- Data virtualization for performance
- Large dataset handling

### Row Operations
📄 **Read:** [references/row-operations.md](references/row-operations.md)
- Row height customization
- Auto row height (QueryRowHeight event)
- Row drag and drop
- Row swiping actions
- Adding new rows programmatically
- Deleting rows
- Unbound rows (additional summary rows at top/bottom)
- Row-level events

### Exporting & Clipboard
📄 **Read:** [references/exporting.md](references/exporting.md)
- Export to Excel functionality
- Excel export customization (columns, rows, styling)
- Excel export options and events
- Export to PDF functionality
- PDF export customization
- Clipboard operations (copy/paste)
- Custom clipboard formats

### Advanced Features & Views
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- Master-Details View (hierarchical/relational data)
- Record Template View (custom row layouts)
- Empty view customization
- Context menu implementation
- Tooltips for cells
- Merged cells
- Serialization (save/load grid state)
- Conditional styling by data

### Styling & Customization
📄 **Read:** [references/styling-customization.md](references/styling-customization.md)
- Grid styling basics
- Cell styling (CellStyle)
- Conditional cell styling
- Row styling and alternating row colors
- Header styling
- Theme customization
- Custom cell templates
- Liquid glass effect

### Performance & Events
📄 **Read:** [references/performance-events.md](references/performance-events.md)
- Performance optimization techniques
- Large dataset best practices
- Memory management strategies
- Grid events overview
- Common event scenarios
- Event handling patterns
- Performance monitoring

### Localization
📄 **Read:** [references/localization.md](references/localization.md)
- Localization setup
- Multi-language support
- RTL (Right-to-Left) support
- Scrolling modes and optimization

