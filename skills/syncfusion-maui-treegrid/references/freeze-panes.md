# Freeze Panes in TreeGrid

## Table of Contents
- [Overview](#overview)
- [Freeze Panes Properties](#freeze-panes-properties)
- [Freeze Columns (Left)](#freeze-columns-left)
- [Freeze Footer Columns (Right)](#freeze-footer-columns-right)
- [Appearance](#appearance)
- [Choosing When to Freeze](#choosing-when-to-freeze)

## Overview

Freezing panes allows you to keep specific columns visible while scrolling horizontally through large datasets, similar to Excel. This is useful when you have identifier columns that should remain visible during navigation.

The .NET MAUI TreeGrid (`SfTreeGrid`) supports freezing columns independently at the left and right edges of the grid.

## Freeze Panes Properties

You can freeze columns by setting the following properties on `SfTreeGrid`:

| Property Name | Type | Default | Description |
|---------------|------|---------|-------------|
| `FrozenColumnCount` | `int` | `0` | Sets the number of columns to freeze at the left side of the TreeGrid. |
| `FooterFrozenColumnCount` | `int` | `0` | Sets the number of columns to freeze at the right side of the TreeGrid. |

## Freeze Columns (Left)

Freeze columns by setting the `FrozenColumnCount` property to a non-negative value. Frozen columns remain visible when scrolling horizontally and are useful for identifier columns that should always be visible.

```xaml
<ContentPage.BindingContext>
    <local:EmployeeViewModel />
</ContentPage.BindingContext>

<syncfusion:SfTreeGrid
    x:Name="treeGrid"
    ItemsSource="{Binding Employees}"
    ChildPropertyName="ReportingPeople"
    FrozenColumnCount="1" />
```

```csharp
// Freezes the first column.
treeGrid.FrozenColumnCount = 1;
```

The frozen column remains visible at the left edge while scrolling horizontally through the remaining columns.

> **Note:** `FrozenColumnCount` must be less than the total number of columns in the TreeGrid. When `FrozenColumnCount` is set to `0`, no columns are frozen on the left side.

## Freeze Footer Columns (Right)

Freeze footer (rightmost) columns by setting the `FooterFrozenColumnCount` property to a non-negative value. Footer frozen columns remain visible when scrolling horizontally and are useful for summary or action columns.

```xaml
<ContentPage.BindingContext>
    <local:EmployeeViewModel />
</ContentPage.BindingContext>

<syncfusion:SfTreeGrid
    x:Name="treeGrid"
    ItemsSource="{Binding Employees}"
    ChildPropertyName="ReportingPeople"
    FooterFrozenColumnCount="1" />
```

```csharp
// Freezes the last column.
treeGrid.FooterFrozenColumnCount = 1;
```

The frozen footer column remains visible at the right edge while scrolling horizontally through the remaining columns.

> **Note:** `FooterFrozenColumnCount` must be less than the total number of columns in the TreeGrid. The combined count of `FrozenColumnCount` and `FooterFrozenColumnCount` must not exceed the total number of columns.

## Appearance

Customize the visual appearance of freeze panes using `TreeGridStyle`, applied through the `SfTreeGrid.DefaultStyle` property.

### Freeze pane line color

Customize the color of the line that divides frozen and non-frozen regions using the `TreeGridStyle.FreezePaneLineColor` property:

```xaml
<ContentPage.BindingContext>
    <local:EmployeeViewModel />
</ContentPage.BindingContext>

<syncfusion:SfTreeGrid
    x:Name="treeGrid"
    ItemsSource="{Binding Employees}"
    ChildPropertyName="ReportingPeople"
    FrozenColumnCount="1">

    <syncfusion:SfTreeGrid.DefaultStyle>
        <syncfusion:TreeGridStyle
            FreezePaneLineColor="Orange" />
    </syncfusion:SfTreeGrid.DefaultStyle>

</syncfusion:SfTreeGrid>
```

```csharp
treeGrid.DefaultStyle.FreezePaneLineColor = Colors.Orange;
```

### Freeze pane line thickness

Customize the thickness of the freeze pane line using the `TreeGridStyle.FreezePaneLineStrokeThickness` property. This affects all frozen columns in both left and right regions.

**Default Value:** `1.0`

```xaml
<ContentPage.BindingContext>
    <local:EmployeeViewModel />
</ContentPage.BindingContext>

<syncfusion:SfTreeGrid
    x:Name="treeGrid"
    ItemsSource="{Binding Employees}"
    ChildPropertyName="ReportingPeople"
    FrozenColumnCount="1">

    <syncfusion:SfTreeGrid.DefaultStyle>
        <syncfusion:TreeGridStyle
            FreezePaneLineStrokeThickness="2" />
    </syncfusion:SfTreeGrid.DefaultStyle>

</syncfusion:SfTreeGrid>
```

```csharp
treeGrid.DefaultStyle.FreezePaneLineStrokeThickness = 2;
```

## Choosing When to Freeze

| Scenario | Property | Value |
|----------|----------|-------|
| Keep the first N identifier columns visible while scrolling right | `FrozenColumnCount` | N (e.g., `1`) |
| Keep the last N action/summary columns visible while scrolling left | `FooterFrozenColumnCount` | N (e.g., `1`) |
| Both: lock ID on the left and actions on the right | Both properties | Ensure combined count < total columns |
| Remove freezing | Both properties | Set both to `0` |

### Common gotchas

- **`FrozenColumnCount` equal to or greater than total columns** will cause issues; always keep it less than the total column count.
- **Combined count exceeding total columns**: `FrozenColumnCount + FooterFrozenColumnCount` must not exceed the total number of columns.
- **Line not visible**: If `FreezePaneLineColor` matches the row background, the divider disappears. Use a contrasting color.
