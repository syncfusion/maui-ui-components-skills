# Columns in TreeGrid

## Table of Contents
- [Overview](#overview)
- [Automatic Column Generation](#automatic-column-generation)
- [Auto-Generation Modes](#auto-generation-modes)
- [Customize Auto-Generated Columns](#customize-auto-generated-columns)
- [Manually Generate Columns](#manually-generate-columns)
- [Column Manipulation](#column-manipulation)

## Overview

The `SfTreeGrid` allows you to create and add columns in two ways:

- **Automatically generating columns** — based on the underlying data source properties
- **Manually defining columns** — by adding `TreeGridColumn` objects to the `SfTreeGrid.Columns` collection

## Automatic Column Generation

`SfTreeGrid` creates columns automatically based on the bindable property `AutoGenerateColumnsMode`. Columns are generated based on the type of individual properties in the underlying collection set as `ItemsSource`.

The table below shows the column type created for the respective data types. For all other data types, `TreeGridTextColumn` is created.

| Data Type | Column |
|-----------|--------|
| string, object | TreeGridTextColumn |
| int, float, double, decimal and their respective nullable types | TreeGridNumericColumn |
| DateTime | TreeGridDateColumn |
| bool | TreeGridCheckBoxColumn |

## Auto-Generation Modes

The auto generation of columns is controlled by the `AutoGenerateColumnsMode` property. The default value is `AutoGenerateColumnsMode.Reset`.

| Mode | Description |
|------|-------------|
| `None` | Maintains only the columns that are explicitly defined in the `SfTreeGrid.Columns` collection. |
| `Reset` | Retains the columns defined at the application level and automatically generates columns for the remaining properties available in the data source. |
| `ResetAll` | Clears all existing columns when the `ItemsSource` changes and regenerates columns based on the new data source. Any manually defined columns are ignored and recreated from the underlying collection. |
| `RetainOld` | Generates columns for all properties in the data source only when the TreeGrid does not contain explicit column definitions. If columns are already defined, those columns are retained and no additional columns are generated. |
| `SmartReset` | Retains explicitly defined columns as well as columns whose `MappingName` matches properties in the new data source. Columns for newly introduced properties are generated automatically. |

### Choosing a mode

| Scenario | Recommended mode |
|----------|------------------|
| You define all columns manually and want no auto-generated columns | `None` |
| You want a mix of manual and auto-generated columns | `Reset` (default) or `SmartReset` |
| The data source changes entirely and columns must be fully rebuilt | `ResetAll` |
| You want to keep existing columns and avoid adding new ones | `RetainOld` |

## Customize Auto-Generated Columns

Auto-generated columns can be customized by handling the `AutoGeneratingColumn` event, which is raised when each column is auto-generated.

The `TreeGridAutoGeneratingColumnEventArgs` object contains the following properties:

- **Column** — Returns the created column that can be customized.
- **Cancel** — Cancels the column creation.
- **PropertyType** — Specifies the type of the underlying model property for which the column is created.

### Skip generating a column

```xaml
<treeGrid:SfTreeGrid x:Name="treeGrid"
                     ItemsSource="{Binding PersonDetails}"
                     ChildPropertyName="Children"
                     AutoGeneratingColumn="TreeGrid_AutoGeneratingColumn">
</treeGrid:SfTreeGrid>
```

```csharp
private void TreeGrid_AutoGeneratingColumn(object sender, TreeGridAutoGeneratingColumnEventArgs e)
{
    if (e.Column.MappingName == "Hike")
    {
        e.Cancel = true;
    }
}
```

### Apply formatting to auto-generated columns

```csharp
SfTreeGrid treeGrid = new SfTreeGrid();
EmployeeViewModel viewModel = new EmployeeViewModel();

treeGrid.ItemsSource = viewModel.Employees;
treeGrid.AutoGeneratingColumn += TreeGrid_AutoGeneratingColumn;

this.Content = treeGrid;

private void TreeGrid_AutoGeneratingColumn(object sender,
                                           TreeGridAutoGeneratingColumnEventArgs e)
{
    if (e.Column.MappingName == "Salary")
    {
        e.Column.Format = "C";
    }
    else if (e.Column.MappingName == "JoinDate")
    {
        e.Column.Format = "MMMM dd";
    }
}
```

## Manually Generate Columns

Define columns manually by adding `TreeGridColumn` objects to the `SfTreeGrid.Columns` collection. To show only the manually defined columns, set `AutoGenerateColumnsMode` to `None`.

```xaml
<treeGrid:SfTreeGrid x:Name="treeGrid"
                     ItemsSource="{Binding PersonDetails}"
                     ChildPropertyName="Children"
                     AutoGenerateColumnsMode="None">
    <treeGrid:SfTreeGrid.Columns>
        <treeGrid:TreeGridNumericColumn HeaderText="Employee ID"
                                        MappingName="EmployeeID" />
        <treeGrid:TreeGridTextColumn HeaderText="Employee Name"
                                     MappingName="Name" />
        <treeGrid:TreeGridTextColumn HeaderText="Designation"
                                     MappingName="Designation" />
        <treeGrid:TreeGridTextColumn HeaderText="Department"
                                     MappingName="Department" />
    </treeGrid:SfTreeGrid.Columns>
</treeGrid:SfTreeGrid>
```

```csharp
SfTreeGrid treeGrid = new SfTreeGrid();
treeGrid.ItemsSource = viewModel.PersonDetails;
treeGrid.AutoGenerateColumnsMode = AutoGenerateColumnsMode.None;

TreeGridNumericColumn employeeIdColumn = new TreeGridNumericColumn
{
    HeaderText = "Employee ID",
    MappingName = "EmployeeID"
};

TreeGridTextColumn nameColumn = new TreeGridTextColumn
{
    HeaderText = "Employee Name",
    MappingName = "Name"
};

TreeGridTextColumn designationColumn = new TreeGridTextColumn
{
    HeaderText = "Designation",
    MappingName = "Designation"
};

TreeGridTextColumn departmentColumn = new TreeGridTextColumn
{
    HeaderText = "Department",
    MappingName = "Department"
};

treeGrid.Columns.Add(employeeIdColumn);
treeGrid.Columns.Add(nameColumn);
treeGrid.Columns.Add(designationColumn);
treeGrid.Columns.Add(departmentColumn);

this.Content = treeGrid;
```

## Column Manipulation

You can access and manage columns through the `SfTreeGrid.Columns` property.

### Adding a column at runtime

```csharp
this.treeGrid.Columns.Add(new TreeGridTextColumn()
{
    HeaderText = "Department",
    MappingName = "Department"
});
```

### Accessing a column

Access a column through its index or `MappingName`:

```csharp
TreeGridColumn column = this.treeGrid.Columns[1];

// OR

TreeGridColumn column = this.treeGrid.Columns["EmployeeID"];
```

### Clearing or removing a column

```csharp
// Remove all columns
this.treeGrid.Columns.Clear();

// Remove a specific column
this.treeGrid.Columns.Remove(column);

// OR remove by index
this.treeGrid.Columns.RemoveAt(1);
```
