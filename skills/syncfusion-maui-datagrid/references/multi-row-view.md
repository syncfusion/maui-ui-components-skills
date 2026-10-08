# Multi-Row Column Layout

## Table of Contents
- [Overview](#overview)
- [Enabling Multi-Row View](#enabling-multi-row-view)
- [Defining Column Positions](#defining-column-positions)
- [Multi-Row Layout Example](#multi-row-layout-example)
- [Limitations](#limitations)
- [When to Use Multi-Row View](#when-to-use-multi-row-view)

## Overview

The `SfDataGrid` supports displaying a record across multiple rows using the `MultiRowViewDefinition`. This feature organizes and spans cells across multiple row levels, letting you arrange columns in a customized layout and create card-like views within each record row.

Multi-Row View is useful when displaying a large number of fields — it improves readability by organizing data into multiple rows and columns instead of cramming every field into a single wide row.

## Enabling Multi-Row View

Enable Multi-Row View by defining a `DataGridMultiRowViewDefinition` in the `SfDataGrid.MultiRowViewDefinition` property. The `RowsCount` and `ColumnsCount` properties specify the number of rows and columns in the layout. The available cell space is divided based on these values.

```xaml
<syncfusion:SfDataGrid ItemsSource="{Binding OrderInfoCollection}">
    <syncfusion:SfDataGrid.MultiRowViewDefinition>
        <syncfusion:DataGridMultiRowViewDefinition RowsCount="2" ColumnsCount="2" />
    </syncfusion:SfDataGrid.MultiRowViewDefinition>
</syncfusion:SfDataGrid>
```

```csharp
var dataGrid = new SfDataGrid
{
    ItemsSource = viewModel.OrderInfoCollection,
    MultiRowViewDefinition = new DataGridMultiRowViewDefinition()
    {
        RowsCount = 2,
        ColumnsCount = 2
    }
};
```

## Defining Column Positions

Column positions in a Multi-Row View layout are customized using positioning properties on the `DataGridColumn` class:

- `Row` — Specifies the row index where the column is placed.
- `Column` — Specifies the column index where the column is placed.
- `RowSpan` — Specifies the number of rows occupied by the column.
- `ColumnSpan` — Specifies the number of columns occupied by the column.

The `Row` and `Column` properties use **zero-based indexing** to position cells within the layout. `Row` arranges cells vertically from top to bottom; `Column` arranges cells horizontally from left to right. `RowSpan` and `ColumnSpan` make a cell span across multiple rows or columns.

## Multi-Row Layout Example

The following example displays record data in a layout containing 2 rows and 3 columns. Each column is positioned using `Row` and `Column`, and spans multiple rows or columns using `RowSpan` and `ColumnSpan`.

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding OrderInfoCollection}"
                       GridLinesVisibility="Both"
                       HeaderGridLinesVisibility="Both">

    <syncfusion:SfDataGrid.MultiRowViewDefinition>
        <syncfusion:DataGridMultiRowViewDefinition RowsCount="2"
                                                   ColumnsCount="3" />
    </syncfusion:SfDataGrid.MultiRowViewDefinition>

    <syncfusion:SfDataGrid.Columns>
        <syncfusion:DataGridImageColumn MappingName="EmpImg"
                                       HeaderText="Profile"
                                       Row="0"
                                       Column="0"
                                       RowSpan="2" />

        <syncfusion:DataGridTextColumn  MappingName="OrderID"
                                       HeaderText="Order ID"
                                       Row="0"
                                       Column="1" />

        <syncfusion:DataGridTextColumn  MappingName="Customer"
                                       HeaderText="Customer"
                                       Row="0"
                                       Column="2" />

        <syncfusion:DataGridTextColumn  MappingName="CustomerID"
                                       HeaderText="Customer ID"
                                       Row="1"
                                       Column="1"
                                       ColumnSpan="2" />
    </syncfusion:SfDataGrid.Columns>
</syncfusion:SfDataGrid>
```

```csharp
SfDataGrid dataGrid = new SfDataGrid();
OrderInfoViewModel viewModel = new OrderInfoViewModel();

dataGrid.ItemsSource = viewModel.OrderInfoCollection;
dataGrid.GridLinesVisibility = GridLinesVisibility.Both;
dataGrid.HeaderGridLinesVisibility = GridLinesVisibility.Both;

dataGrid.MultiRowViewDefinition = new DataGridMultiRowViewDefinition
{
    RowsCount = 2,
    ColumnsCount = 3
};

dataGrid.Columns.Add(new DataGridImageColumn
{
    MappingName = "EmpImg",
    HeaderText = "Profile",
    Row = 0,
    Column = 0,
    RowSpan = 2
});

dataGrid.Columns.Add(new DataGridTextColumn
{
    MappingName = "OrderID",
    HeaderText = "Order ID",
    Row = 0,
    Column = 1
});

dataGrid.Columns.Add(new DataGridTextColumn
{
    MappingName = "Customer",
    HeaderText = "Customer",
    Row = 0,
    Column = 2
});

dataGrid.Columns.Add(new DataGridTextColumn
{
    MappingName = "CustomerID",
    HeaderText = "Customer ID",
    Row = 1,
    Column = 1,
    ColumnSpan = 2
});

this.Content = dataGrid;
```

In this layout:
- The **Profile** column spans two rows using `RowSpan = 2`.
- The **Customer ID** column spans two columns using `ColumnSpan = 2`.
- The remaining columns are positioned by their assigned `Row` and `Column` indexes.

## Limitations

- `Row` and `Column` values must be within the ranges specified by `RowsCount` and `ColumnsCount`.
- `RowSpan` and `ColumnSpan` values should not exceed the defined layout boundaries, and multiple columns cannot occupy the same layout cell.
- Multi-Row View does **not** support: column resizing, row resizing, frozen columns, Details View, stacked headers, column drag and drop, column chooser, row headers, and serialization.
- Only the `Fill` `ColumnWidthMode` is supported.

## When to Use Multi-Row View

| Scenario | Use Multi-Row View? |
|---|---|
| Many fields per record that don't fit comfortably in one row | ✅ Yes |
| Card-like or profile-style record layouts | ✅ Yes |
| You need column resizing or frozen columns | ❌ No (not supported) |
| You need stacked headers or Details View | ❌ No (not supported) |
| Simple tabular data with few columns | ❌ No (use the default single-row layout) |
