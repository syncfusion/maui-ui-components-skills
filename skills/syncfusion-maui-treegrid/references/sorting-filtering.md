# Sorting and Filtering in TreeGrid

## Table of Contents
- [Sorting Overview](#sorting-overview)
- [Programmatic Sorting](#programmatic-sorting)
- [Sorting Modes](#sorting-modes)
- [Tri-State Sorting](#tri-state-sorting)
- [Show Sort Numbers](#show-sort-numbers)
- [Sort Column on Double Tap](#sort-column-on-double-tap)
- [Sorting Events](#sorting-events)
- [Disable Sorting for a Column](#disable-sorting-for-a-column)
- [Custom Sorting](#custom-sorting)
- [Filtering Overview](#filtering-overview)
- [Filter Level](#filter-level)
- [Programmatic View Filtering](#programmatic-view-filtering)
- [Condition-Based Filtering](#condition-based-filtering)
- [Clearing Filters](#clearing-filters)

## Sorting Overview

The `SfTreeGrid` provides built-in support for sorting one or more columns using the `SfTreeGrid.SortingMode` property. When sorting is applied to a column, parent records and their child nodes are automatically sorted according to the specified sort criteria while preserving the hierarchical structure. Sort by tapping the column header; a sort icon appears in the header to indicate the sort direction.

## Programmatic Sorting

Sort data programmatically by adding or removing `SortColumnDescription` objects in the `SfTreeGrid.SortColumnDescriptions` property.

The `SortColumnDescription` object holds two properties:

- **ColumnName** — the name of the column to sort.
- **SortDirection** — a `ListSortDirection` value that defines the sorting direction.

```xaml
<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding PersonDetails}"
                       ChildPropertyName="Children">

    <syncfusion:SfTreeGrid.SortColumnDescriptions>
        <syncfusion:SortColumnDescription ColumnName="FirstName"
                                          SortDirection="Ascending"/>
    </syncfusion:SfTreeGrid.SortColumnDescriptions>

</syncfusion:SfTreeGrid>
```

```csharp
SfTreeGrid treeGrid = new SfTreeGrid();
EmployeeViewModel employeeViewModel = new EmployeeViewModel();
treeGrid.ItemsSource = employeeViewModel.PersonDetails;
treeGrid.ChildPropertyName = "Children";
treeGrid.SortColumnDescriptions.Add(new SortColumnDescription() { ColumnName = "FirstName", SortDirection = System.ComponentModel.ListSortDirection.Ascending });
this.Content = treeGrid;
```

## Sorting Modes

`SfTreeGrid` sorts data against one or more columns based on the `SfTreeGrid.SortingMode` property:

- **Single** — allows sorting only one column at a time.
- **Multiple** — allows sorting more than one column at a time.
- **None** — does not allow any column to be sorted.

To apply sorting to multiple columns, tap the desired column headers after setting `SortingMode` to `Multiple`.

```xaml
<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding PersonDetails}"
                       ChildPropertyName="Children"
                       SortingMode="Multiple">
</syncfusion:SfTreeGrid>
```

```csharp
treeGrid.SortingMode = TreeGridSortingMode.Multiple;
```

## Tri-State Sorting

In addition to ascending and descending, `SfTreeGrid` allows unsorting data back to its original order by clicking the header again after sorting in descending order. Set `SfTreeGrid.AllowTriStateSorting` to `true`. Each column then iterates through three sort states: `ascending`, `descending`, and `unsorted`.

```xaml
<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding PersonDetails}"
                       ChildPropertyName="Children"
                       SortingMode="Single"
                       AllowTriStateSorting="True">
</syncfusion:SfTreeGrid>
```

```csharp
treeGrid.SortingMode = TreeGridSortingMode.Single;
treeGrid.AllowTriStateSorting = true;
```

## Show Sort Numbers

Display sequence numbers denoting the order in which columns are sorted during multi-column sorting by setting `SfTreeGrid.ShowSortNumbers` to `true`. This is applicable when `SortingMode` is `Multiple`.

```xaml
<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding PersonDetails}"
                       ChildPropertyName="Children"
                       SortingMode="Multiple"
                       ShowSortNumbers="True">
</syncfusion:SfTreeGrid>
```

```csharp
treeGrid.SortingMode = TreeGridSortingMode.Multiple;
treeGrid.ShowSortNumbers = true;
```

## Sort Column on Double Tap

By default, the column sorts when its header is clicked. Change this to a double-click action by setting `SfTreeGrid.SortingGestureType` to `DoubleTap`.

```xaml
<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding PersonDetails}"
                       ChildPropertyName="Children"
                       SortingMode="Single"
                       SortingGestureType="DoubleTap">
</syncfusion:SfTreeGrid>
```

```csharp
treeGrid.SortingMode = TreeGridSortingMode.Single;
treeGrid.SortingGestureType = TreeGridSortingGestureType.DoubleTap;
```

## Sorting Events

The TreeGrid provides the following sorting events:

- **`SortColumnsChanging`** — invoked before the column is sorted. Cancel the sorting action by setting the `Cancel` property of `TreeGridSortColumnsChangingEventArgs`.
- **`SortColumnsChanged`** — invoked after the column is sorted.

Both event args contain:
- **AddedItems** — the `SortColumnDescription` objects added to the `SortColumnDescriptions` collection.
- **RemovedItems** — the `SortColumnDescription` objects removed from the collection.

Cancel sorting for a particular column:

```xaml
<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding PersonDetails}"
                       ChildPropertyName="Children"
                       SortingMode="Single"
                       SortColumnsChanging="treeGrid_SortColumnsChanging">
</syncfusion:SfTreeGrid>
```

```csharp
private void treeGrid_SortColumnsChanging(object sender, TreeGridSortColumnsChangingEventArgs e)
{
    var addedItem = e.AddedItems?.Cast<SortColumnDescription>().LastOrDefault();

    if (addedItem?.ColumnName == "FirstName")
    {
        e.Cancel = true;
    }
}
```

## Disable Sorting for a Column

### For auto-generated columns

Disable sorting for an individual column during auto-generation by setting `e.Column.AllowSorting` to `false` in the `AutoGeneratingColumn` event:

```xaml
<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding PersonDetails}"
                       ChildPropertyName="Children"
                       SortingMode="Single"
                       AutoGeneratingColumn="treeGrid_AutoGeneratingColumn">
</syncfusion:SfTreeGrid>
```

```csharp
private void treeGrid_AutoGeneratingColumn(object sender, TreeGridAutoGeneratingColumnEventArgs e)
{
    if (e.Column.MappingName == "FirstName")
    {
        e.Column.AllowSorting = false;
    }
}
```

### For manually defined columns

Set the `TreeGridColumn.AllowSorting` property to `false`. The default value is `true`.

```xaml
<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding PersonDetails}"
                       ChildPropertyName="Children"
                       SortingMode="Single"
                       AutoGenerateColumnsMode="None">
    <syncfusion:SfTreeGrid.Columns>
        <syncfusion:TreeGridTextColumn HeaderText="First Name"
                                       MappingName="FirstName"
                                       AllowSorting="False"/>
        <syncfusion:TreeGridTextColumn HeaderText="Last Name"
                                       MappingName="LastName"/>
        <syncfusion:TreeGridTextColumn HeaderText="Employee ID"
                                       MappingName="EmployeeID"/>
    </syncfusion:SfTreeGrid.Columns>
</syncfusion:SfTreeGrid>
```

```csharp
treeGrid.SortingMode = TreeGridSortingMode.Single;
treeGrid.AutoGenerateColumnsMode = AutoGenerateColumnsMode.None;
treeGrid.Columns.Add(new TreeGridTextColumn { MappingName = "FirstName", HeaderText = "First Name", AllowSorting = false });
treeGrid.Columns.Add(new TreeGridTextColumn { MappingName = "LastName", HeaderText = "Last Name" });
treeGrid.Columns.Add(new TreeGridTextColumn { MappingName = "EmployeeID", HeaderText = "Employee ID" });
```

## Custom Sorting

`SfTreeGrid` supports sorting columns based on custom logic when standard sorting does not meet requirements. Add `SortComparer` objects to the `SfTreeGrid.SortComparers` collection.

The `SortComparer` object contains:
- **PropertyName** — the `MappingName` of the column that applies custom sorting.
- **Comparer** — the custom comparer implementing `IComparer` and `ISortDirection`.

The following example sorts columns based on the length of their cell values:

```xaml
<ContentPage xmlns:comparer="clr-namespace:GettingStarted.Comparer"
             xmlns:data="clr-namespace:Syncfusion.Maui.Data;assembly=Syncfusion.Maui.Data"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.TreeGrid;assembly=Syncfusion.Maui.TreeGrid">

    <ContentPage.Resources>
        <ResourceDictionary>
            <comparer:CustomSortComparer x:Key="comparer"/>
        </ResourceDictionary>
    </ContentPage.Resources>

    <ContentPage.BindingContext>
        <local:EmployeeViewModel/>
    </ContentPage.BindingContext>

    <syncfusion:SfTreeGrid x:Name="treeGrid"
                           ItemsSource="{Binding PersonDetails}"
                           ChildPropertyName="Children"
                           SortingMode="Single">

        <syncfusion:SfTreeGrid.SortComparers>
            <data:SortComparer Comparer="{StaticResource comparer}"
                               PropertyName="LastName"/>
        </syncfusion:SfTreeGrid.SortComparers>

        <syncfusion:SfTreeGrid.SortColumnDescriptions>
            <syncfusion:SortColumnDescription ColumnName="LastName"
                                              SortDirection="Ascending"/>
        </syncfusion:SfTreeGrid.SortColumnDescriptions>

    </syncfusion:SfTreeGrid>

</ContentPage>
```

Implement the custom comparer:

```csharp
using System.Collections;
using Syncfusion.Maui.Data;

public class CustomSortComparer : IComparer<object>, ISortDirection
{
    public ListSortDirection SortDirection { get; set; }

    public int Compare(object x, object y)
    {
        string name1 = (x as EmployeeInfo)?.LastName;
        string name2 = (y as EmployeeInfo)?.LastName;

        int result = name1.Length.CompareTo(name2.Length);

        if (SortDirection == ListSortDirection.Descending)
        {
            result = -result;
        }

        return result;
    }
}
```

## Filtering Overview

Filtering retrieves values from a collection that satisfy specified conditions. `SfTreeGrid` provides programmatic filtering through predicates.

## Filter Level

Filter nodes by level using the `SfTreeGrid.FilterLevel` property.

```xaml
<syncfusion:SfTreeGrid ItemsSource="{Binding PersonDetails}"
                       ChildPropertyName="Children"
                       x:Name="treeGrid"
                       FilterLevel="All">
</syncfusion:SfTreeGrid>
```

```csharp
treeGrid.FilterLevel = FilterLevel.All;
```

| Level | Behavior |
|-------|----------|
| `Root` | The filter is applied only to root nodes. For other nodes, `IsFiltered` is `false` and they are always displayed. |
| `All` | The filter is applied to all nodes. If a parent node does not match the filter condition, the filter is not applied to its child nodes. Otherwise, the filter is also applied to its child nodes. |
| `Extended` | Filtering is applied based on the node hierarchy. If a node matches the filter condition, all of its ancestor nodes are displayed to preserve the hierarchy, even if those ancestors do not match. In such cases, the ancestor node's `IsFiltered` property is set to `false`. |

> **Note:** You can change the `FilterLevel` at run time.

## Programmatic View Filtering

`SfTreeGrid` supports filtering records by setting the `SfTreeGrid.View.Filter` property to a filter predicate.

> **Note:** The `View` property is automatically initialized when `ItemsSource` is set on the TreeGrid. Ensure the TreeGrid has loaded and `ItemsSource` is assigned before accessing `View`.

```csharp
private void Button_Clicked(object sender, EventArgs e)
{
    treeGrid.View.Filter = FilterRecords;
    treeGrid.View.RefreshFilter();
}

/// <summary>
/// RefreshFilter() must be called after setting the Filter predicate to apply the filter.
/// </summary>
public bool FilterRecords(object record)
{
    EmployeeInfo? employeeInfo = record as EmployeeInfo;

    if (employeeInfo != null && employeeInfo.Department == "Sales")
    {
        return true;
    }
    return false;
}
```

## Condition-Based Filtering

Implement condition-based filtering where records are filtered based on user-defined logic — for example, filtering to include specific values (`Contains`, `Equals`) or exclude values (`Does Not Equal`). Custom condition-based filtering can be applied to all columns or to individual columns.

```csharp
public bool FilterRecords(object record)
{
    EmployeeInfo employeeInfo = record as EmployeeInfo;

    if (employeeInfo != null)
    {
        if (columns.SelectedItem.ToString() == "All Columns")
        {
            if (conditions.SelectedItem.ToString() == "Contains")
            {
                var vm = this.BindingContext as EmployeeInfoViewModel;
                var filterText = vm?.FilterText?.ToLower() ?? string.Empty;
                if (employeeInfo.EmpId.ToString().ToLower().Contains(filterText) ||
                    employeeInfo.FirstName.ToLower().Contains(filterText) ||
                    employeeInfo.LastName.ToLower().Contains(filterText) ||
                    employeeInfo.Title.ToLower().Contains(filterText) ||
                    employeeInfo.Salary.ToString().ToLower().Contains(filterText) ||
                    employeeInfo.Hike.ToString().ToLower().Contains(filterText))
                    return true;
                return false;
            }
            else if (conditions.SelectedItem.ToString() == "Equals")
            {
                if (CheckEquals(employeeInfo.EmpId.ToString()) ||
                    CheckEquals(employeeInfo.FirstName) ||
                    CheckEquals(employeeInfo.LastName) ||
                    CheckEquals(employeeInfo.Title) ||
                    CheckEquals(employeeInfo.Salary.ToString()) ||
                    CheckEquals(employeeInfo.Hike.ToString()))
                    return true;
                return false;
            }
            else
            {
                if (!CheckEquals(employeeInfo.EmpId.ToString()) ||
                   !CheckEquals(employeeInfo.FirstName) ||
                   !CheckEquals(employeeInfo.LastName) ||
                   !CheckEquals(employeeInfo.Title) ||
                   !CheckEquals(employeeInfo.Salary.ToString()) ||
                   !CheckEquals(employeeInfo.Hike.ToString()))
                    return true;
                return false;
            }
        }
        else
        {
            var value = record.GetType().GetProperty(columns.SelectedItem.ToString().Replace(" ", ""));
            if (value == null) return false;
            var exactValue = value.GetValue(record, null);
            if (exactValue == null) return false;
            if (conditions.SelectedItem.ToString() == "Contains")
            {
                var vm = this.BindingContext as EmployeeInfoViewModel;
                var filterText = vm?.FilterText?.ToLower() ?? string.Empty;
                return filterText.Contains(exactValue.ToString().ToLower());
            }
            else if (conditions.SelectedItem.ToString() == "Equals")
            {
                return CheckEquals(exactValue.ToString());
            }
            else
            {
                return !CheckEquals(exactValue.ToString());
            }
        }
    }
    return false;
}

public bool CheckEquals(string value)
{
    return FilterText.Equals(value);
}

private void Button_Clicked(object sender, EventArgs e)
{
    treeGrid.View.Filter = FilterRecords;
    treeGrid.View.RefreshFilter();
}
```

### UI for condition-based filtering

The following example shows a `Picker` for conditions, a text entry, and a filter button:

```xaml
<ContentPage.BindingContext>
    <local:EmployeeInfoViewModel x:Name="viewModel"/>
</ContentPage.BindingContext>

<Grid RowDefinitions="50, *">
    <syncfusion:SfTreeGrid ItemsSource="{Binding PersonDetails}"
                           ChildPropertyName="Children"
                           x:Name="treeGrid"
                           Grid.Row="1">
    </syncfusion:SfTreeGrid>

    <Grid Grid.Row="0">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="150"/>
            <ColumnDefinition Width="150"/>
            <ColumnDefinition Width="150"/>
        </Grid.ColumnDefinitions>

        <Entry Grid.Column="0" Text="{Binding FilterText}" Placeholder="Enter filter text" />

        <Picker x:Name="columns" Grid.Column="1">
            <Picker.Items>
                <x:String>All Columns</x:String>
                <x:String>First Name</x:String>
                <x:String>Last Name</x:String>
                <x:String>Employee ID</x:String>
                <x:String>Salary</x:String>
                <x:String>Title</x:String>
                <x:String>Hike</x:String>
            </Picker.Items>
            <Picker.SelectedItem>
                <x:String>All Columns</x:String>
            </Picker.SelectedItem>
        </Picker>

        <Picker x:Name="conditions" Grid.Column="2">
            <Picker.Items>
                <x:String>Equals</x:String>
                <x:String>Does Not Equal</x:String>
                <x:String>Contains</x:String>
            </Picker.Items>
            <Picker.SelectedItem>
                <x:String>Does Not Equal</x:String>
            </Picker.SelectedItem>
        </Picker>

        <Button Grid.Column="3" Text="Filter" Clicked="Button_Clicked"/>
    </Grid>
</Grid>
```

## Clearing Filters

Remove all applied filters and display the complete dataset:

```csharp
private void ClearFilter(object sender, EventArgs e)
{
    treeGrid.View.Filter = null;
    treeGrid.View.RefreshFilter();
}
```

> **Note:** Filters are applied before the sorting operation. When you clear a filter, the view is refreshed, and sorting is reapplied to the full dataset.
