# Data Binding in TreeGrid

## Table of Contents
- [Overview](#overview)
- [Binding Self-Relational Data](#binding-self-relational-data)
- [Binding Nested Collection](#binding-nested-collection)
- [Binding with IEnumerable](#binding-with-ienumerable)
- [AutoExpandMode](#autoexpandmode)
- [Expanding Nodes Programmatically](#expanding-nodes-programmatically)
- [Collapsing Nodes Programmatically](#collapsing-nodes-programmatically)
- [Node Expansion Events](#node-expansion-events)

## Overview

The `SfTreeGrid` is designed to display self-relational and hierarchical data in a tree structure with columns. The `ItemsSource` property binds the control to a collection of objects, and the `ChildPropertyName` property establishes the hierarchical relationship.

There are two ways to bind a data source to `SfTreeGrid`:
- **Binding self-relational data** — uses `ParentPropertyName` and `ChildPropertyName`
- **Binding nested collection** — uses only `ChildPropertyName`

## Binding Self-Relational Data

`SfTreeGrid` supports binding self-relational data by setting the `ParentPropertyName` and `ChildPropertyName` properties. In self-relational data, the tree structure is formed based on matching relationships between parent and child items within a single flat collection.

- `ParentPropertyName` — denotes the property in the data object used to identify root nodes.
- `ChildPropertyName` — denotes the property in the data object that references the parent. This value is matched against the `ParentPropertyName` of other objects to establish the hierarchy.

Data objects with unique values in `ParentPropertyName`, or values matching the `SelfRelationRootValue`, are treated as root nodes.

### Creating the Model

```csharp
using System.ComponentModel;
using System.Collections.ObjectModel;

public class EmployeeInfo : INotifyPropertyChanged
{
    private int _id;
    private string _firstName;
    private string _lastName;
    private string _title;
    private double _salary;
    private int _reportsTo;

    public int ID
    {
        get { return _id; }
        set { _id = value; OnPropertyChanged(nameof(ID)); }
    }

    public string FirstName
    {
        get { return _firstName; }
        set { _firstName = value; OnPropertyChanged(nameof(FirstName)); }
    }

    public string LastName
    {
        get { return _lastName; }
        set { _lastName = value; OnPropertyChanged(nameof(LastName)); }
    }

    public string Title
    {
        get { return _title; }
        set { _title = value; OnPropertyChanged(nameof(Title)); }
    }

    public double Salary
    {
        get { return _salary; }
        set { _salary = value; OnPropertyChanged(nameof(Salary)); }
    }

    public int ReportsTo
    {
        get { return _reportsTo; }
        set { _reportsTo = value; OnPropertyChanged(nameof(ReportsTo)); }
    }

    public event PropertyChangedEventHandler? PropertyChanged;

    private void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Creating the ViewModel

```csharp
public class EmployeeViewModel : INotifyPropertyChanged
{
    private ObservableCollection<EmployeeInfo> _employees;

    public ObservableCollection<EmployeeInfo> Employees
    {
        get { return _employees; }
        set { _employees = value; OnPropertyChanged(nameof(Employees)); }
    }

    public EmployeeViewModel()
    {
        Employees = GetEmployees();
    }

    private ObservableCollection<EmployeeInfo> GetEmployees()
    {
        var employeeDetails = new ObservableCollection<EmployeeInfo>();

        // Root level employees (ReportsTo = -1)
        employeeDetails.Add(new EmployeeInfo { FirstName = "James", LastName = "Smith", ID = 1, Title = "Management", Salary = 2000000, ReportsTo = -1 });
        employeeDetails.Add(new EmployeeInfo { FirstName = "John", LastName = "Adams", ID = 2, Title = "Accounts", Salary = 2000000, ReportsTo = -1 });

        // Children of James (ID = 1)
        employeeDetails.Add(new EmployeeInfo { FirstName = "Michael", LastName = "Fuller", ID = 5, Title = "Vice President", Salary = 1200000, ReportsTo = 1 });
        employeeDetails.Add(new EmployeeInfo { FirstName = "Janet", LastName = "Leverling", ID = 6, Title = "GM", Salary = 1000000, ReportsTo = 1 });

        // Children of John (ID = 2)
        employeeDetails.Add(new EmployeeInfo { FirstName = "Nancy", LastName = "Davolio", ID = 8, Title = "Accounts Manager", Salary = 850000, ReportsTo = 2 });
        employeeDetails.Add(new EmployeeInfo { FirstName = "Margaret", LastName = "Peacock", ID = 9, Title = "Accountant", Salary = 700000, ReportsTo = 2 });

        return employeeDetails;
    }

    public event PropertyChangedEventHandler? PropertyChanged;
    private void OnPropertyChanged(string propertyName) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
}
```

### Binding to the TreeGrid

Set `ParentPropertyName`, `ChildPropertyName`, and `SelfRelationRootValue` to identify root nodes:

```xaml
<ContentPage.BindingContext>
    <local:EmployeeViewModel/>
</ContentPage.BindingContext>

<syncfusion:SfTreeGrid x:Name="treeGrid"
                        ItemsSource="{Binding Employees}"
                        ParentPropertyName="ID"
                        ChildPropertyName="ReportsTo"
                        SelfRelationRootValue="-1"
                        AutoExpandMode="RootNodesExpanded" />
```

```csharp
EmployeeViewModel employeeViewModel = new EmployeeViewModel();
SfTreeGrid treeGrid = new SfTreeGrid();
treeGrid.ItemsSource = employeeViewModel.Employees;
treeGrid.ParentPropertyName = "ID";
treeGrid.ChildPropertyName = "ReportsTo";
treeGrid.SelfRelationRootValue = -1;
treeGrid.AutoExpandMode = TreeGridExpandMode.RootNodesExpanded;
this.Content = treeGrid;
```

**Key properties for self-relational binding:**

| Property | Purpose |
|----------|---------|
| `ParentPropertyName` | The property whose value identifies a parent (e.g., `ID`) |
| `ChildPropertyName` | The property whose value references the parent (e.g., `ReportsTo`) |
| `SelfRelationRootValue` | The value in `ChildPropertyName` that marks a root node (e.g., `-1`) |

## Binding Nested Collection

The TreeGrid supports binding nested or hierarchical collections where each data object contains a property holding its child collection. This approach is commonly used for organizational hierarchies or folder structures.

### Creating the Model

```csharp
public class PersonInfo : INotifyPropertyChanged
{
    private string _firstName;
    private string _lastName;
    private bool _available;
    private double _salary;
    private ObservableCollection<PersonInfo> _children;

    public string FirstName
    {
        get { return _firstName; }
        set { _firstName = value; OnPropertyChanged(nameof(FirstName)); }
    }

    public string LastName
    {
        get { return _lastName; }
        set { _lastName = value; OnPropertyChanged(nameof(LastName)); }
    }

    public bool Available
    {
        get { return _available; }
        set { _available = value; OnPropertyChanged(nameof(Available)); }
    }

    public double Salary
    {
        get { return _salary; }
        set { _salary = value; OnPropertyChanged(nameof(Salary)); }
    }

    public ObservableCollection<PersonInfo> Children
    {
        get { return _children; }
        set { _children = value; OnPropertyChanged(nameof(Children)); }
    }

    public event PropertyChangedEventHandler? PropertyChanged;
    private void OnPropertyChanged(string propertyName) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
}
```

### Binding to the TreeGrid

Set `ItemsSource` to the root collection and `ChildPropertyName` to the property containing child items:

```xaml
<ContentPage.BindingContext>
    <local:EmployeeViewModel/>
</ContentPage.BindingContext>

<syncfusion:SfTreeGrid x:Name="treeGrid"
                        ItemsSource="{Binding EmployeeCollection}"
                        ChildPropertyName="Children" />
```

```csharp
EmployeeViewModel employeeViewModel = new EmployeeViewModel();
SfTreeGrid treeGrid = new SfTreeGrid();
treeGrid.ItemsSource = employeeViewModel.EmployeeCollection;
treeGrid.ChildPropertyName = "Children";
this.Content = treeGrid;
```

## Binding with IEnumerable

`SfTreeGrid` supports binding any collection that implements the `IEnumerable` interface. Data operations such as sorting and filtering are supported when the binding collection is derived from `IEnumerable`. The hierarchical structure is maintained through the `ChildPropertyName` property, which specifies where child data is located.

## AutoExpandMode

By default, items load in a collapsed state. Control how nodes expand on load using the `AutoExpandMode` property, which accepts values of the `TreeGridExpandMode` enum:

| Value | Behavior |
|-------|----------|
| `None` | All items are collapsed when loaded. This is the default. |
| `RootNodesExpanded` | Expands only the root-level items when loaded. |
| `AllNodesExpanded` | Expands all items when loaded. |

```xaml
<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding EmployeeCollection}"
                       ChildPropertyName="Children"
                       AutoExpandMode="AllNodesExpanded" />
```

```csharp
treeGrid.AutoExpandMode = TreeGridExpandMode.AllNodesExpanded;
```

## Expanding Nodes Programmatically

The TreeGrid provides multiple methods to expand nodes at runtime:

| Method | Description |
|--------|-------------|
| `ExpandAllNodes()` | Expands all nodes including all child nodes |
| `ExpandAllNodes(int level)` | Expands all nodes up to the given level |
| `ExpandAllNodes(TreeNode node)` | Expands the particular node and all its child nodes |
| `ExpandNode(int level)` | Expands all nodes at a particular level |
| `ExpandNode(TreeNode node)` | Expands the particular node |
| `ExpandNode(int rowIndex)` | Expands the node at a specific row index |

### Expand all nodes

```csharp
treeGrid.ExpandAllNodes();
```

Expand a specific node and all its children:

```csharp
var node = treeGrid.View.Nodes[0];
treeGrid.ExpandAllNodes(node);
```

### Expand nodes based on level

Level 0 represents the root level:

```csharp
// Expand all nodes at root level (level 0)
treeGrid.ExpandNode(0);

// Expand all nodes at level 1
treeGrid.ExpandNode(1);
```

### Expand a specific node by index

```csharp
treeGrid.ExpandNode(3);
```

### Expand a node based on a business object

Resolve the node from your data object and expand it:

```csharp
var employeeViewModel = new EmployeeViewModel();
var data = employeeViewModel.EmployeDetails[0];
var node = this.treeGrid.View.Nodes.GetNode(data);
treeGrid.ExpandNode(node);
```

## Collapsing Nodes Programmatically

The TreeGrid provides corresponding collapse methods:

| Method | Description |
|--------|-------------|
| `CollapseAllNodes()` | Collapses all nodes including all child nodes |
| `CollapseAllNodes(int level)` | Collapses all nodes up to the given level |
| `CollapseNode(int level)` | Collapses all nodes at a particular level |
| `CollapseNode(TreeNode node)` | Collapses the particular node |
| `CollapseNode(int rowIndex)` | Collapses the node at a specific row index |

```csharp
treeGrid.CollapseAllNodes();
```

The `IsExpanded` property on a `TreeNode` reflects and controls the expansion state of an individual node.

## Node Expansion Events

### NodeExpanding

Triggered before a node is expanded. This is a cancelable event, allowing you to prevent specific nodes from expanding based on business logic. The `TreeGridNodeExpandingEventArgs` contains:

- `Cancel` — set to `true` to cancel the node expansion
- `Node` — the node being expanded

```xaml
<syncfusion:SfTreeGrid x:Name="treeGrid"
                       ItemsSource="{Binding EmployeeCollection}"
                       ChildPropertyName="Children"
                       NodeExpanding="treeGrid_NodeExpanding" />
```

```csharp
treeGrid.NodeExpanding += treeGrid_NodeExpanding;

private void treeGrid_NodeExpanding(object sender, TreeGridNodeExpandingEventArgs e)
{
    // Prevent expanding nodes beyond level 2
    if (e.Node.Level > 2)
    {
        e.Cancel = true;
    }
}
```

### NodeCollapsing

Triggered before a node is collapsed. Also cancelable via the `Cancel` property of `TreeGridNodeCollapsingEventArgs`. Use it to keep specific nodes expanded when business logic requires it.

### NodeExpanded

Triggered after a node is expanded successfully. It helps to get the details of the node that was expanded. 

### NodeCollapsed

Triggered after a node is collapsed successfully. It helps to get the details of the node that was collapsed. 

### Choosing a binding approach

| Data shape | Use |
|------------|-----|
| Flat list with parent/child ID fields | Self-relational binding (`ParentPropertyName` + `ChildPropertyName` + `SelfRelationRootValue`) |
| Objects each containing a child collection | Nested collection binding (`ChildPropertyName` only) |
| Any `IEnumerable`-derived source | Either approach works; hierarchy driven by `ChildPropertyName` |
