# Column Sizing in TreeGrid

## Table of Contents
- [Overview](#overview)
- [ColumnWidthMode Options](#columnwidthmode-options)
- [Change the Default Column Width](#change-the-default-column-width)
- [Retrieve Auto-Calculated Width](#retrieve-auto-calculated-width)
- [Apply ColumnWidthMode to a Particular Column](#apply-columnwidthmode-to-a-particular-column)
- [Minimum and Maximum Column Widths](#minimum-and-maximum-column-widths)

## Overview

The .NET MAUI Tree Grid allows you to set column widths based on certain logic using the `SfTreeGrid.ColumnWidthMode` or `TreeGridColumn.ColumnWidthMode` property.

## ColumnWidthMode Options

| Type | Column width behavior |
|------|----------------------|
| `Fill` | Divides the total width equally for columns. |
| `Auto` | Calculates the width of columns based on header and cell contents so that header and cell contents are not truncated. |
| `LastColumnFill` | The column width is adjusted with respect to the `DefaultColumnWidth` property. If the columns do not fill the entire view space, the width of the last column fills the unoccupied space in the view. |
| `FitByCell` | Calculates the width of columns based on cell contents so that cell contents are not truncated. |
| `FitByHeader` | Calculates the width of columns based on header content so that header content is not truncated. |
| `None` | Default column width or the explicitly defined width set to a column. |

> **Note:** `ColumnWidthMode` will not work when the column width is defined explicitly. The `ColumnWidthMode` calculates the column width based on `MinimumWidth` and `MaximumWidth` properties.

### Choosing a sizing mode

| Scenario | Recommended mode |
|----------|------------------|
| Fill the available viewport width evenly | `Fill` |
| Avoid truncating both header and cell text | `Auto` |
| Fill leftover space with the last column | `LastColumnFill` |
| Fit columns to the widest cell content only | `FitByCell` |
| Fit columns to the header text only | `FitByHeader` |
| Use explicit / default widths, no auto sizing | `None` |

### Apply Fill mode

```xaml
<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding PersonDetails}"
                       ChildPropertyName="Children"
                       ColumnWidthMode="Fill">
</syncfusion:SfTreeGrid>
```

```csharp
SfTreeGrid treeGrid = new SfTreeGrid();
EmployeeViewModel employeeViewModel = new EmployeeViewModel();
treeGrid.ItemsSource = employeeViewModel.PersonDetails;
treeGrid.ChildPropertyName = "Children";
treeGrid.ColumnWidthMode = TreeGridColumnWidthMode.Fill;
this.Content = treeGrid;
```

## Change the Default Column Width

Set a common width for all columns using the `DefaultColumnWidth` property:

```xaml
<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding PersonDetails}"
                       ChildPropertyName="Children"
                       DefaultColumnWidth="120">
</syncfusion:SfTreeGrid>
```

```csharp
SfTreeGrid treeGrid = new SfTreeGrid();
EmployeeViewModel employeeViewModel = new EmployeeViewModel();
treeGrid.ItemsSource = employeeViewModel.PersonDetails;
treeGrid.ChildPropertyName = "Children";
treeGrid.DefaultColumnWidth = 120d;
this.Content = treeGrid;
```

## Retrieve Auto-Calculated Width

Retrieve the width of columns when auto-calculated based on `ColumnWidthMode` using the `ActualWidth` property. The `ActualWidth` is only accurate after the TreeGrid has been loaded and laid out.

```xaml
<Grid RowDefinitions="*,50">
    <syncfusion:SfTreeGrid x:Name="treeGrid"
                           Grid.Row="0"
                           ItemsSource="{Binding PersonDetails}"
                           ChildPropertyName="Children"
                           ColumnWidthMode="Auto"/>
    <Button Text="Get Column Width"
            Grid.Row="1"
            WidthRequest="300"
            HorizontalOptions="Center"
            Clicked="Button_Clicked">
    </Button>
</Grid>
```

```csharp
private void Button_Clicked(object sender, EventArgs e)
{
    double width = treeGrid.Columns["FirstName"]?.ActualWidth ?? 0;
}
```

## Apply ColumnWidthMode to a Particular Column

Apply column sizing to an individual column using the `TreeGridColumn.ColumnWidthMode` property. If the per-column mode is not explicitly set, it takes the value of `SfTreeGrid.ColumnWidthMode`.

```xaml
<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding PersonDetails}"
                       ChildPropertyName="Children"
                       ColumnWidthMode="None">

    <syncfusion:SfTreeGrid.Columns>
        <syncfusion:TreeGridTextColumn HeaderText="First Name"
                                       MappingName="FirstName"
                                       ColumnWidthMode="Auto">
        </syncfusion:TreeGridTextColumn>
    </syncfusion:SfTreeGrid.Columns>

</syncfusion:SfTreeGrid>
```

```csharp
SfTreeGrid treeGrid = new SfTreeGrid();
EmployeeViewModel employeeViewModel = new EmployeeViewModel();
treeGrid.ItemsSource = employeeViewModel.PersonDetails;
treeGrid.ChildPropertyName = "Children";
treeGrid.ColumnWidthMode = TreeGridColumnWidthMode.None;

TreeGridTextColumn textColumn = new TreeGridTextColumn();
textColumn.MappingName = "FirstName";
textColumn.HeaderText = "First Name";
textColumn.ColumnWidthMode = TreeGridColumnWidthMode.Auto;

treeGrid.Columns.Add(textColumn);

this.Content = treeGrid;
```

## Minimum and Maximum Column Widths

Constrain the auto-calculated column width by setting the `MinimumWidth` and `MaximumWidth` properties on individual columns. These constraints are respected when any `ColumnWidthMode` is applied.

> **Note:** `MinimumWidth` and `MaximumWidth` work with all `ColumnWidthMode` options (Auto, Fill, FitByCell, FitByHeader, LastColumnFill) to ensure columns stay within defined bounds.

```xaml
<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding PersonDetails}"
                       ChildPropertyName="Children"
                       ColumnWidthMode="Auto">

    <syncfusion:SfTreeGrid.Columns>
        <syncfusion:TreeGridTextColumn HeaderText="First Name"
                                       MappingName="FirstName"
                                       MinimumWidth="80"
                                       MaximumWidth="150">
        </syncfusion:TreeGridTextColumn>
        <syncfusion:TreeGridTextColumn HeaderText="Last Name"
                                       MappingName="LastName"
                                       MinimumWidth="100"
                                       MaximumWidth="150">
        </syncfusion:TreeGridTextColumn>
    </syncfusion:SfTreeGrid.Columns>

</syncfusion:SfTreeGrid>
```

```csharp
SfTreeGrid treeGrid = new SfTreeGrid();
EmployeeViewModel employeeViewModel = new EmployeeViewModel();
treeGrid.ItemsSource = employeeViewModel.PersonDetails;
treeGrid.ChildPropertyName = "Children";
treeGrid.ColumnWidthMode = TreeGridColumnWidthMode.Auto;

treeGrid.Columns.Add(new TreeGridTextColumn()
{
    HeaderText = "First Name",
    MappingName = "FirstName",
    MinimumWidth = 80,
    MaximumWidth = 150
});

treeGrid.Columns.Add(new TreeGridTextColumn()
{
    HeaderText = "Last Name",
    MappingName = "LastName",
    MinimumWidth = 100,
    MaximumWidth = 150
});

this.Content = treeGrid;
```
