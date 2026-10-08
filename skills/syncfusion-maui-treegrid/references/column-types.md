# Column Types in TreeGrid

## Table of Contents
- [Overview](#overview)
- [Available Column Types](#available-column-types)
- [TreeGridColumn Base Properties](#treegridcolumn-base-properties)
- [DisplayBinding and Value Converters](#displaybinding-and-value-converters)
- [Width](#width)
- [Text Alignment and Padding](#text-alignment-and-padding)
- [Header Customization](#header-customization)
- [Hiding a Column](#hiding-a-column)
- [Formatting Values](#formatting-values)

## Overview

The .NET MAUI `SfTreeGrid` supports a variety of column types, each designed to handle specific data formats and presentation requirements. By choosing the appropriate column type, you can effectively display hierarchical data based on your application's needs.

## Available Column Types

| Column Type | Renderer | Key | Description |
|-------------|----------|-----|-------------|
| `TreeGridTextColumn` | TreeGridTextBoxRenderer | Text | Display text or alphanumeric values across rows in the hierarchical structure. |
| `TreeGridCheckBoxColumn` | TreeGridCheckBoxRenderer | CheckBox | Present boolean flags or toggle states with built-in checkbox controls for each node. |
| `TreeGridTemplateColumn` | TreeGridCellTemplateRenderer | Template | Create highly customizable cells with complex layouts, combining multiple controls and visual elements. |
| `TreeGridNumericColumn` | TreeGridNumericCellRenderer | Numeric | Render numerical values with formatting capabilities and numeric editing support. |
| `TreeGridDateColumn` | TreeGridDateCellRenderer | DateTime | Display temporal data including dates, times, and datetime values with format customization. |

### Choosing a column type

| Data to display | Use |
|-----------------|-----|
| Names, titles, alphanumeric text | `TreeGridTextColumn` |
| Numbers, currency, quantities | `TreeGridNumericColumn` |
| Dates and times | `TreeGridDateColumn` |
| Boolean flags / toggle states | `TreeGridCheckBoxColumn` |
| Custom layouts combining multiple controls | `TreeGridTemplateColumn` |

## TreeGridColumn Base Properties

`TreeGridColumn` is the base column type inherited by all specialized column types. It provides the core functionality and key properties used across all columns.

| Property | Description |
|----------|-------------|
| `MappingName` | Links a column to a property in the data model. Sorting and filtering rely on this. |
| `HeaderText` | Specifies the text displayed in the column header. Defaults to `MappingName` if not set. |
| `Width` | Sets a manual column width. Falls back to `DefaultColumnWidth` when unset. |
| `Format` | Formats displayed values (e.g., `C2`, `N2`, `dd/MM/yyyy`). |
| `Visible` | Controls column visibility. Default is `True`. |
| `ColumnWidthMode` | Per-column sizing mode (overrides the grid-level setting). |
| `MinimumWidth` / `MaximumWidth` | Constrain auto-calculated widths. |
| `CellStyle` / `HeaderStyle` | Apply styles to cells and headers of this column. |
| `AllowSorting` | Enables/disables sorting for this column. Default `True`. |

## DisplayBinding and Value Converters

The `TreeGridColumn.DisplayBinding` property controls how data renders within cells. When you assign only `MappingName`, `SfTreeGrid` automatically generates an appropriate `DisplayBinding` based on that property name.

To transform or format the displayed data, attach value converters to the `DisplayBinding` property:

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <local:DisplayBindingConverter x:Key="displayBindingConverter" />
    </ResourceDictionary>
</ContentPage.Resources>

<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding EmployeeCollection}"
                       ChildPropertyName="Children">

    <syncfusion:SfTreeGrid.Columns>
        <syncfusion:TreeGridTextColumn MappingName="EmployeeID"
                                       DisplayBinding="{Binding EmployeeID,
                                       Converter={StaticResource displayBindingConverter}}" />
    </syncfusion:SfTreeGrid.Columns>
</syncfusion:SfTreeGrid>
```

```csharp
using System.Globalization;

public class DisplayBindingConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value != null)
            return "Employee : " + value.ToString();
        return null;
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```

## Width

Customize the width of each column using the `TreeGridColumn.Width` property. By default this property is unset, and the column renders based on the `DefaultColumnWidth` property.

```xaml
<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding EmployeeCollection}"
                       ChildPropertyName="Children"
                       DefaultColumnWidth="150">
    <syncfusion:SfTreeGrid.Columns>
        <syncfusion:TreeGridTextColumn MappingName="EmployeeID" Width="100" />
        <syncfusion:TreeGridTextColumn MappingName="EmployeeName" Width="200" />
    </syncfusion:SfTreeGrid.Columns>
</syncfusion:SfTreeGrid>
```

```csharp
// Auto-generated column
treeGrid.AutoGeneratingColumn += TreeGrid_AutoGeneratingColumn;

private void TreeGrid_AutoGeneratingColumn(object sender, TreeGridAutoGeneratingColumnEventArgs e)
{
    if (e.Column.MappingName == "EmployeeID")
    {
        e.Column.Width = 100;
    }
}

// Manually generated column
treeGrid.AutoGenerateColumnsMode = AutoGenerateColumnsMode.None;
treeGrid.Columns.Add(new TreeGridTextColumn() { MappingName = "EmployeeID", Width = 100 });
```

## Text Alignment and Padding

Configure text alignment for header cells and data row cells using `HeaderTextAlignment` and `CellTextAlignment`. The default alignment depends on the column type: numeric and date columns are right-aligned by default, while text columns are left-aligned.

```xaml
<syncfusion:TreeGridTextColumn MappingName="EmployeeID"
                               CellTextAlignment="Start"
                               CellPadding="10,0,0,0"
                               HeaderPadding="10,0,0,0" />
```

```csharp
TreeGridTextColumn employeeID = new TreeGridTextColumn();
employeeID.MappingName = "EmployeeID";
employeeID.CellTextAlignment = TextAlignment.Start;
employeeID.CellPadding = new Thickness(10, 0, 0, 0);
employeeID.HeaderPadding = new Thickness(10, 0, 0, 0);
```

## Header Customization

### HeaderText

Customize the display content of the header cell using the `HeaderText` property. If the header text is not defined, the `MappingName` is assigned to the header text and displayed as the column header.

### Header template

Customize the header cell using the `HeaderTemplate` property:

```xaml
<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding EmployeeCollection}"
                       ChildPropertyName="Children">
    <syncfusion:SfTreeGrid.Columns>
        <syncfusion:TreeGridTextColumn MappingName="EmployeeID" HeaderText="ID">
            <syncfusion:TreeGridTextColumn.HeaderTemplate>
                <DataTemplate>
                    <Label Text="{Binding ., StringFormat='ID'}" TextColor="Blue" FontAttributes="Bold"/>
                </DataTemplate>
            </syncfusion:TreeGridTextColumn.HeaderTemplate>
        </syncfusion:TreeGridTextColumn>
    </syncfusion:SfTreeGrid.Columns>
</syncfusion:SfTreeGrid>
```

## Hiding a Column

Hide a particular column using the `TreeGridColumn.Visible` property. The default value is `True`.

> **Note:** Set the `Visible` property to `False` instead of setting column width to `0` to hide a column.

```xaml
<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding EmployeeCollection}"
                       ChildPropertyName="Children"
                       AutoGenerateColumnsMode="None">
    <syncfusion:SfTreeGrid.Columns>
        <syncfusion:TreeGridTextColumn MappingName="EmployeeID" />
        <syncfusion:TreeGridTextColumn MappingName="EmployeeName" />
        <syncfusion:TreeGridTextColumn MappingName="Department" Visible="false" />
    </syncfusion:SfTreeGrid.Columns>
</syncfusion:SfTreeGrid>
```

```csharp
// Auto-generated column
treeGrid.AutoGeneratingColumn += TreeGrid_AutoGeneratingColumn;

private void TreeGrid_AutoGeneratingColumn(object sender, TreeGridAutoGeneratingColumnEventArgs e)
{
    if (e.Column.MappingName == "EmployeeID")
    {
        e.Column.Visible = false;
    }
}

// Manually generated column
treeGrid.AutoGenerateColumnsMode = AutoGenerateColumnsMode.None;
treeGrid.Columns.Add(new TreeGridTextColumn() { MappingName = "EmployeeID", Visible = false });
```

## Formatting Values

Format values displayed in the `TreeGridColumn` using the `Format` property. The format string is applied to the underlying data type.

Common format strings:

| Format | Meaning | Example |
|--------|---------|---------|
| `C` or `C2` | Currency | $1,234.56 |
| `N` or `N2` | Number with decimals | 1,234.56 |
| `P` or `P2` | Percentage | 123.46% |
| `d` | Short date | 7/6/2026 |
| `dd/MM/yyyy` | Custom date | 06/07/2026 |
| `hh\:mm` | Time | 14:30 |

```xaml
<syncfusion:SfTreeGrid.Columns>
    <syncfusion:TreeGridTextColumn MappingName="Salary" Format="C2" />
</syncfusion:SfTreeGrid.Columns>
```

```csharp
treeGrid.Columns.Add(new TreeGridTextColumn()
{
    MappingName = "Salary",
    Format = "C2"  // Displays as $1,234.56
});

treeGrid.Columns.Add(new TreeGridTextColumn()
{
    MappingName = "JoiningDate",
    Format = "dd/MM/yyyy"
});

treeGrid.Columns.Add(new TreeGridTextColumn()
{
    MappingName = "Bonus",
    Format = "P2"  // Displays as 25.50%
});
```

### Format a column using a converter

You can also customize the format of a particular column using a converter when the built-in format strings are insufficient for complex transformations.
