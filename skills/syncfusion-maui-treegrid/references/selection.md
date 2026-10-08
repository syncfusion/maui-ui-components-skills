# Selection in TreeGrid

## Table of Contents
- [Overview](#overview)
- [Selection Modes](#selection-modes)
- [Getting Selected Rows](#getting-selected-rows)
- [Programmatic Selection](#programmatic-selection)
- [Clear Selection](#clear-selection)
- [Selection Events](#selection-events)
- [Customizing Selection Appearance](#customizing-selection-appearance)
- [Binding Selection Properties](#binding-selection-properties)

## Overview

The MAUI TreeGrid `SfTreeGrid` allows you to select one or more rows based on the `SelectionMode` property.

## Selection Modes

| Mode | Description |
|------|-------------|
| `None` | Disables selection entirely. No rows can be selected. This is the default value. |
| `Single` | Allows selection of a single row. Previous selection is cleared when selecting a different row. |
| `Multiple` | Allows selection of more than one row. Selection is not cleared when selecting additional rows. Click an already selected row a second time to deselect it. |
| `SingleDeselect` | Allows selection of a single row only. Tap the row again to deselect it. Previous selection is cleared when selecting a different row. |

```xaml
<treeGrid:SfTreeGrid x:Name="treeGrid"
                     ItemsSource="{Binding PersonDetails}"
                     ChildPropertyName="Children"
                     SelectionMode="Multiple">
</treeGrid:SfTreeGrid>
```

```csharp
SfTreeGrid treeGrid = new SfTreeGrid();
treeGrid.ItemsSource = viewModel.PersonDetails;
treeGrid.SelectionMode = TreeGridSelectionMode.Multiple;
treeGrid.ChildPropertyName = "Children";
this.Content = treeGrid;
```

## Getting Selected Rows

`SfTreeGrid` provides `SelectedIndex`, `SelectedRow`, and `SelectedRows` properties to get details of selected rows:

- **`SfTreeGrid.SelectedIndex`** — the row index of `SfTreeGrid.SelectedRow`. Denotes the index of the first selected row in multiple selections.
- **`SfTreeGrid.SelectedRow`** — the underlying data object of the selected row. Denotes the underlying data object of the first selected row in multiple selections.
- **`SfTreeGrid.SelectedRows`** — all selected records when multiple selection is enabled.

## Programmatic Selection

### Select using properties

When `SelectionMode` is set to a value other than `None`, programmatically select a row by setting the row index to `SelectedIndex` or the underlying data object to `SelectedRow`:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();

        // Perform selection using the selected index
        this.treeGrid.SelectedIndex = 3;

        // Perform selection using the selected row
        this.treeGrid.SelectedRow = viewModel.PersonDetails[3];
    }
}
```

### Multiple selection

When the selection mode is `Multiple`, select more than one row by adding the underlying data objects to the `SelectedRows` collection:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();

        treeGrid.SelectedRows.Add(viewModel.PersonDetails[2]);
        treeGrid.SelectedRows.Add(viewModel.PersonDetails[4]);
        treeGrid.SelectedRows.Add(viewModel.PersonDetails[6]);
    }
}
```

### Select all rows

Select all rows using the `SelectAll()` method. Wait until the grid has loaded so the rows are available:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        treeGrid.TreeGridLoaded += TreeGrid_TreeGridLoaded;
    }

    private void TreeGrid_TreeGridLoaded(object? sender, EventArgs e)
    {
        treeGrid.SelectAll();
    }
}
```

## Clear Selection

Clear selection either by setting `SelectionMode` to `None` or by calling the `ClearSelection()` method:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();

        // Clear selection using selection mode
        treeGrid.SelectionMode = TreeGridSelectionMode.None;

        // Clear selection using method
        treeGrid.ClearSelection();
    }
}
```

## Selection Events

### SelectionChanging

Raised before the selection changes. Cancel the selection operation by setting the `Cancel` property of `TreeGridSelectionChangingEventArgs`:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        treeGrid.SelectionChanging += TreeGrid_SelectionChanging;
    }

    private void TreeGrid_SelectionChanging(object sender, TreeGridSelectionChangingEventArgs e)
    {
        e.Cancel = true;
    }
}
```

### SelectionChanged

Raised after a row is selected. The `TreeGridSelectionChangedEventArgs` contains:

- **`AddedRows`** — the collection of rows added to the selection.
- **`RemovedRows`** — the collection of rows removed from the selection.

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        treeGrid.SelectionChanged += TreeGrid_SelectionChanged;
    }

    private void TreeGrid_SelectionChanged(object sender, TreeGridSelectionChangedEventArgs e)
    {
        if (e.AddedRows.Count > 0)
        {
            var selectedItem = e.AddedRows[0];
        }
    }
}
```

## Customizing Selection Appearance

### Selected row styling

Change the selection background color and text color of selected rows using the `SelectionBackground` and `SelectedRowTextColor` properties through `DefaultStyle`:

```xaml
<treeGrid:SfTreeGrid ItemsSource="{Binding PersonDetails}"
                     SelectionMode="Multiple">
    <treeGrid:SfTreeGrid.DefaultStyle>
        <treeGrid:TreeGridStyle SelectionBackground="#E3F2FD"
                                SelectedRowTextColor="Black"/>
    </treeGrid:SfTreeGrid.DefaultStyle>
</treeGrid:SfTreeGrid>
```

```csharp
treeGrid.DefaultStyle.SelectionBackground = Color.FromArgb("#E3F2FD");
treeGrid.DefaultStyle.SelectedRowTextColor = Colors.Black;
```

## Binding Selection Properties

`SfTreeGrid` allows binding selection-related properties such as `SelectedIndex` and `SelectedRow` directly to properties in the ViewModel:

```xaml
<treeGrid:SfTreeGrid ItemsSource="{Binding PersonDetails}"
                     SelectedIndex="{Binding TreeGridSelectedIndex}"
                     SelectedRow="{Binding TreeGridSelectedRow}" />
```

```csharp
public class ViewModel : INotifyPropertyChanged
{
    private int treeGridSelectedIndex;

    private object treeGridSelectedRow;

    public int TreeGridSelectedIndex
    {
        get => treeGridSelectedIndex;
        set
        {
            treeGridSelectedIndex = value;
            RaisePropertyChanged(nameof(TreeGridSelectedIndex));
        }
    }

    public object TreeGridSelectedRow
    {
        get => treeGridSelectedRow;
        set
        {
            treeGridSelectedRow = value;
            RaisePropertyChanged(nameof(TreeGridSelectedRow));
        }
    }

    public ViewModel()
    {
        TreeGridSelectedIndex = 2;
        TreeGridSelectedRow = PersonDetails[5];
    }

    public event PropertyChangedEventHandler? PropertyChanged;

    private void RaisePropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Choosing a selection mode

| Need | Mode |
|------|------|
| No selection at all | `None` |
| Pick exactly one row at a time | `Single` |
| Pick several rows (multi-select) | `Multiple` |
| Pick one row but allow deselecting by tapping again | `SingleDeselect` |
