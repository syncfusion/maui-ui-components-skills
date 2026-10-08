# Styling in TreeGrid

## Table of Contents
- [Overview](#overview)
- [Styling Priority Order](#styling-priority-order)
- [Column Styling](#column-styling)
- [Implicit Styling](#implicit-styling)
- [Default Styling](#default-styling)
- [Grid Line Customization](#grid-line-customization)

## Overview

The `SfTreeGrid` provides comprehensive styling support to customize the appearance of grid elements through multiple approaches. You can apply styles at the column level, row level, or using `DefaultStyle` and implicit styling. The `SfTreeGrid.DefaultStyle` property contains all the required styling properties for each element in the TreeGrid, while implicit styling allows you to customize the appearance of specific control types using `TargetType` styles.

## Styling Priority Order

Column-level styles (explicit) take precedence over implicit `TargetType` styles, which take precedence over the default `TreeGridStyle`.

| Priority | Style type | Scope |
|----------|------------|-------|
| 1 (highest) | Column-level (`CellStyle`, `HeaderStyle`) | A single column |
| 2 | Implicit (`TargetType` styles without a key) | All matching elements in scope |
| 3 (lowest) | `DefaultStyle` (`TreeGridStyle`) | The entire grid |

## Column Styling

### Cell styling

Apply styling for cells of a particular column using the `TreeGridColumn.CellStyle` property:

```xaml
<ContentPage.Resources>
    <Style TargetType="syncfusion:DataGridCell"
           x:Key="customCellStyle">
        <Setter Property="Background"
                Value="#5BC0EB"/>
        <Setter Property="TextColor"
                Value="#212121"/>
        <Setter Property="FontAttributes"
                Value="Italic"/>
        <Setter Property="FontSize"
                Value="14"/>
        <Setter Property="FontFamily"
                Value="TimesNewRoman"/>
    </Style>
</ContentPage.Resources>

<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding EmployeeCollection}"
                       ChildPropertyName="Children">

    <syncfusion:SfTreeGrid.Columns>
        <syncfusion:TreeGridTextColumn HeaderText="Employee ID"
                                       MappingName="EmployeeID"
                                       CellStyle="{StaticResource customCellStyle}"/>
    </syncfusion:SfTreeGrid.Columns>
</syncfusion:SfTreeGrid>
```

### Header styling

Apply styling for the header cell of a particular column using the `TreeGridColumn.HeaderStyle` property:

```xaml
<ContentPage.Resources>
    <Style TargetType="syncfusion:DataGridHeaderCell"
           x:Key="customHeaderStyle">
        <Setter Property="Background"
                Value="#4750DD"/>
        <Setter Property="TextColor"
                Value="White"/>
        <Setter Property="FontAttributes"
                Value="Bold"/>
        <Setter Property="FontSize"
                Value="14"/>
        <Setter Property="FontFamily"
                Value="TimesNewRoman"/>
    </Style>
</ContentPage.Resources>

<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding EmployeeCollection}"
                       ChildPropertyName="Children">
    <syncfusion:SfTreeGrid.Columns>
        <syncfusion:TreeGridTextColumn HeaderText="Employee ID"
                                       MappingName="EmployeeID"
                                       HeaderStyle="{StaticResource customHeaderStyle}"/>
    </syncfusion:SfTreeGrid.Columns>
</syncfusion:SfTreeGrid>
```

## Implicit Styling

The appearance of the TreeGrid and its inner elements can be customized by writing styles of `TargetType` for those controls. If the key is not specified, the style is applied to all `SfTreeGrid` instances in its scope.

### Styling record cell

Customize record cells by writing a style for `TreeGridCell` `TargetType`. The underlying record serves as the `DataContext` for `TreeGridCell`. When styling record cells, also style the expander cell (`TreeGridExpanderCell`) so the appearance is consistent across the hierarchy:

```xaml
<ContentPage.Resources>
    <Style TargetType="syncfusion:TreeGridCell">
        <Setter Property="Background"
                Value="#AFD5FB"/>
        <Setter Property="TextColor"
                Value="#212121"/>
        <Setter Property="FontAttributes"
                Value="Italic"/>
        <Setter Property="FontSize"
                Value="14"/>
        <Setter Property="FontFamily"
                Value="TimesNewRoman"/>
    </Style>
    <Style TargetType="treeGrid:TreeGridExpanderCell">
        <Setter Property="Background"
                Value="#AFD5FB"/>
        <Setter Property="TextColor"
                Value="#212121"/>
        <Setter Property="FontAttributes"
                Value="Italic"/>
        <Setter Property="FontSize"
                Value="14"/>
        <Setter Property="FontFamily"
                Value="TimesNewRoman"/>
    </Style>
</ContentPage.Resources>

<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding EmployeeCollection}"
                       ChildPropertyName="Children">
</syncfusion:SfTreeGrid>
```

### Styling header cell

Customize header cells by writing a style for `TreeGridHeaderCell` `TargetType`:

```xaml
<ContentPage.Resources>
    <Style TargetType="syncfusion:TreeGridHeaderCell">
        <Setter Property="Background" Value="#7BD389"/>
        <Setter Property="TextColor" Value="White"/>
        <Setter Property="FontAttributes" Value="Bold"/>
        <Setter Property="FontSize" Value="14"/>
        <Setter Property="FontFamily" Value="TimesNewRoman"/>
    </Style>
</ContentPage.Resources>

<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding EmployeeCollection}"
                       ChildPropertyName="Children">
</syncfusion:SfTreeGrid>
```

### Styling record row

Customize the record row by writing a style for `TreeGridRow` `TargetType`:

```xaml
<ContentPage.Resources>
    <Style TargetType="syncfusion:TreeGridRow">
        <Setter Property="Background" Value="#BADFCD"/>
    </Style>
</ContentPage.Resources>

<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding EmployeeCollection}"
                       ChildPropertyName="Children">
</syncfusion:SfTreeGrid>
```

### Styling header row

Customize the header row by writing a style for `TreeGridHeaderRow` `TargetType`:

```xaml
<ContentPage.Resources>
    <Style TargetType="syncfusion:TreeGridHeaderRow">
        <Setter Property="Background" Value="#FC8F8F"/>
    </Style>
</ContentPage.Resources>

<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding EmployeeCollection}"
                       ChildPropertyName="Children">
</syncfusion:SfTreeGrid>
```

### Implicit styling target types

| TargetType | Styles |
|------------|--------|
| `TreeGridCell` | Record cells |
| `TreeGridExpanderCell` | Expander cells (style alongside `TreeGridCell` for consistency) |
| `TreeGridHeaderCell` | Header cells |
| `TreeGridRow` | Record rows |
| `TreeGridHeaderRow` | Header row |

## Default Styling

Customize the appearance of the TreeGrid using the `DefaultStyle` property:

```xaml
<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding EmployeeCollection}"
                       ChildPropertyName="Children">
    <syncfusion:SfTreeGrid.DefaultStyle>
        <syncfusion:TreeGridStyle HeaderRowBackground="#0074E3"
                                  HeaderRowTextColor="White"
                                  RowBackground="#AFD5FB"
                                  RowTextColor="#212121"/>
    </syncfusion:SfTreeGrid.DefaultStyle>
</syncfusion:SfTreeGrid>
```

```csharp
EmployeeViewModel employeeViewModel = new EmployeeViewModel();
SfTreeGrid treeGrid = new SfTreeGrid();
treeGrid.ItemsSource = employeeViewModel.EmployeeCollection;
treeGrid.ChildPropertyName = "Children";
treeGrid.DefaultStyle.HeaderRowBackground = Color.FromArgb("#0074E3");
treeGrid.DefaultStyle.HeaderRowTextColor = Colors.White;
treeGrid.DefaultStyle.RowBackground = Color.FromArgb("#AFD5FB");
treeGrid.DefaultStyle.RowTextColor = Color.FromArgb("#212121");
this.Content = treeGrid;
```

### Set TreeGrid style from application resources

Write a custom style for the `TreeGridStyle` properties in `App.xaml` and consume it as a static resource:

```xaml
<!-- App.xaml -->
<Application.Resources>
    <ResourceDictionary>
        <syncfusion:TreeGridStyle x:Key="customStyle"
                                  RowBackground="#BADFCD"
                                  HeaderRowBackground="#05B084"
                                  RowTextColor="Black"
                                  HeaderRowTextColor="White"/>
    </ResourceDictionary>
</Application.Resources>
```

```xaml
<!-- MainPage.xaml -->
<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding EmployeeCollection}"
                       ChildPropertyName="Children"
                       DefaultStyle="{StaticResource customStyle}">
</syncfusion:SfTreeGrid>
```

### Set TreeGrid style from page resources

Write a custom style for `TreeGridStyle` properties using page resources:

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <syncfusion:TreeGridStyle x:Key="customStyle"
                                  RowBackground="#85D5F6"
                                  HeaderRowBackground="#4750DD"
                                  RowTextColor="Black"
                                  HeaderRowTextColor="White"/>
    </ResourceDictionary>
</ContentPage.Resources>

<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding EmployeeCollection}"
                       ChildPropertyName="Children"
                       DefaultStyle="{StaticResource customStyle}">
</syncfusion:SfTreeGrid>
```

## Grid Line Customization

### Visibility

Change the visibility of vertical and horizontal borders. Set `SfTreeGrid.GridLinesVisibility` for data rows or `SfTreeGrid.HeaderGridLinesVisibility` for the header row.

Available options:

- `Both`
- `Horizontal`
- `Vertical`
- `None`

```xaml
<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding EmployeeCollection}"
                       ChildPropertyName="Children"
                       GridLinesVisibility="Both"
                       HeaderGridLinesVisibility="Both"/>
```

```csharp
treeGrid.GridLinesVisibility = TreeGridLinesVisibility.Both;
treeGrid.HeaderGridLinesVisibility = TreeGridLinesVisibility.Both;
```

### Stroke

Customize the grid line color of column header and data row cells using `TreeGridStyle.GridLineColor` and `TreeGridStyle.HeaderGridLineColor`:

```xaml
<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding EmployeeCollection}"
                       GridLinesVisibility="Both"
                       HeaderGridLinesVisibility="Both">
    <syncfusion:SfTreeGrid.DefaultStyle>
        <syncfusion:TreeGridStyle HeaderGridLineColor="#219ebc"
                                  GridLineColor="#219ebc"/>
    </syncfusion:SfTreeGrid.DefaultStyle>
</syncfusion:SfTreeGrid>
```

```csharp
treeGrid.GridLinesVisibility = TreeGridLinesVisibility.Both;
treeGrid.HeaderGridLinesVisibility = TreeGridLinesVisibility.Both;
treeGrid.DefaultStyle.HeaderGridLineColor = Color.FromArgb("#219ebc");
treeGrid.DefaultStyle.GridLineColor = Color.FromArgb("#219ebc");
```

### Choosing a styling approach

| Need | Approach |
|------|----------|
| Style one specific column's cells or header | Column-level `CellStyle` / `HeaderStyle` |
| Apply a consistent look to all cells/headers across the grid | Implicit `TargetType` styles |
| Quickly set row/header backgrounds and text colors grid-wide | `DefaultStyle` (`TreeGridStyle`) |
| Reuse a style across multiple pages | Define `TreeGridStyle` in `App.xaml` resources |
| Control border visibility and color | `GridLinesVisibility` + `GridLineColor` / `HeaderGridLineColor` |
