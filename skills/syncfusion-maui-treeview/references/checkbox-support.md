# Checkbox Support in .NET MAUI TreeView (SfTreeView)

The `SfTreeView` provides support for loading `CheckBox` controls in each node and enables users to check/uncheck nodes. This document covers checkbox implementation, states, and behaviors.

## Table of Contents

- [Overview](#overview)
- [Checkbox in Bound Mode](#checkbox-in-bound-mode)
- [Checkbox in Unbound Mode](#checkbox-in-unbound-mode)
- [CheckBoxMode Property](#checkboxmode-property)
- [Working with Checked Items](#working-with-checked-items)
  - [Access Checked Items from ViewModel](#access-checked-items-from-viewmodel-bound-mode)
  - [GetCheckedNodes() Method](#getchecknodes-method-unbound-mode)
- [IsChecked Property](#ischecked-property)
- [NodeChecked Event](#nodechecked-event)
- [Comparison: Getting Checked Items](#comparison-getting-checked-items)
- [Best Practices](#best-practices)
- [Common Issues and Troubleshooting](#common-issues-and-troubleshooting)

---

## Overview

To use checkboxes in TreeView:

1. **Add CheckBox to ItemTemplate** - Include a `SfCheckBox` control in your `ItemTemplate`
2. **Bind IsChecked** - Bind the checkbox `IsChecked` property to `TreeViewNode.IsChecked`
3. **Set ItemTemplateContextType** - Must be set to `Node` for checkbox binding
4. **Configure CheckBoxMode** - Define how checkboxes behave (Recursive, Cascade, etc.)

**Key Point:** Always set `ItemTemplateContextType="Node"` when using checkboxes.

---

## Checkbox in Bound Mode

In bound mode, use the `CheckedItems` property to work with checked items through your ViewModel.

### Step 1: Create Data Model

```csharp
public class Folder : INotifyPropertyChanged
{
    private string folderName;
    private ObservableCollection<Folder> filesInfo;

    public string FolderName
    {
        get { return folderName; }
        set
        {
            folderName = value;
            OnPropertyChanged("FolderName");
        }
    }

    public ObservableCollection<Folder> FilesInfo
    {
        get { return filesInfo; }
        set
        {
            filesInfo = value;
            OnPropertyChanged("FilesInfo");
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Step 2: Create ViewModel with CheckedItems

```csharp
public class FileManagerViewModel : INotifyPropertyChanged
{
    private ObservableCollection<object> checkedItems;
    public ObservableCollection<Folder> Folders { get; set; }

    public ObservableCollection<object> CheckedItems
    {
        get { return checkedItems; }
        set
        {
            checkedItems = value;
            OnPropertyChanged("CheckedItems");
        }
    }

    public FileManagerViewModel()
    {
        this.Folders = GetFolders();
        this.CheckedItems = new ObservableCollection<object>();
    }

    private ObservableCollection<Folder> GetFolders()
    {
        var folders = new ObservableCollection<Folder>();
        
        var documents = new Folder() { FolderName = "Documents" };
        documents.FilesInfo = new ObservableCollection<Folder>
        {
            new Folder() { FolderName = "Resume.pdf" },
            new Folder() { FolderName = "CoverLetter.docx" }
        };

        folders.Add(documents);
        return folders;
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Step 3: XAML with Checkbox

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.TreeView;assembly=Syncfusion.Maui.TreeView"
             xmlns:checkbox="clr-namespace:Syncfusion.Maui.Buttons;assembly=Syncfusion.Maui.Buttons"
             xmlns:local="clr-namespace:YourNamespace"
             x:Class="YourNamespace.MainPage">

    <ContentPage.BindingContext>
        <local:FileManagerViewModel x:Name="viewModel"/>
    </ContentPage.BindingContext>

    <syncfusion:SfTreeView x:Name="treeView"
                           ItemsSource="{Binding Folders}"
                           ChildPropertyName="FilesInfo"
                           ItemTemplateContextType="Node"
                           CheckedItems="{Binding CheckedItems}"
                           CheckBoxMode="Recursive"
                           AutoExpandMode="AllNodesExpanded">
        
        <syncfusion:SfTreeView.ItemTemplate>
            <DataTemplate>
                <Grid ColumnDefinitions="40,*" ColumnSpacing="10" Padding="5">
                    <!-- Checkbox -->
                    <checkbox:SfCheckBox Grid.Column="0"
                                        VerticalOptions="Center"
                                        HorizontalOptions="Center"
                                        IsChecked="{Binding IsChecked, Mode=TwoWay}"/>
                    
                    <!-- Label -->
                    <Label Grid.Column="1"
                           Text="{Binding Content.FolderName}"
                           VerticalOptions="Center"
                           FontSize="14"/>
                </Grid>
            </DataTemplate>
        </syncfusion:SfTreeView.ItemTemplate>
    </syncfusion:SfTreeView>
</ContentPage>
```

**Important Notes:**
- `ItemTemplateContextType="Node"` enables binding to `TreeViewNode.IsChecked`
- `CheckedItems` must be an `ObservableCollection<object>`
- Two-way binding on checkbox: `IsChecked="{Binding IsChecked, Mode=TwoWay}"`

---

## Checkbox in Unbound Mode

In unbound mode, directly access and modify the `IsChecked` property of `TreeViewNode` objects.

### XAML Example

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       ItemTemplateContextType="Node">
    
    <syncfusion:SfTreeView.Nodes>
        <treeviewengine:TreeViewNode Content="Documents" IsChecked="False">
            <treeviewengine:TreeViewNode.ChildNodes>
                <treeviewengine:TreeViewNode Content="Report.pdf" IsChecked="False"/>
                <treeviewengine:TreeViewNode Content="Budget.xlsx" IsChecked="False"/>
            </treeviewengine:TreeViewNode.ChildNodes>
        </treeviewengine:TreeViewNode>
    </syncfusion:SfTreeView.Nodes>

    <syncfusion:SfTreeView.ItemTemplate>
        <DataTemplate>
            <Grid ColumnDefinitions="40,*" ColumnSpacing="10" Padding="5">
                <checkbox:SfCheckBox Grid.Column="0"
                                    IsChecked="{Binding IsChecked, Mode=TwoWay}"/>
                <Label Grid.Column="1"
                       Text="{Binding Content}"
                       VerticalOptions="Center"/>
            </Grid>
        </DataTemplate>
    </syncfusion:SfTreeView.ItemTemplate>
</syncfusion:SfTreeView>
```

### C# Code-Behind

```csharp
// Access checked nodes programmatically
private void GetCheckedNodes()
{
    var checkedNodes = new List<TreeViewNode>();
    foreach (var node in treeView.Nodes)
    {
        CollectCheckedNodes(node, checkedNodes);
    }
}

private void CollectCheckedNodes(TreeViewNode node, List<TreeViewNode> checkedList)
{
    if (node.IsChecked)
    {
        checkedList.Add(node);
    }

    foreach (var childNode in node.ChildNodes)
    {
        CollectCheckedNodes(childNode, checkedList);
    }
}
```

---

## CheckBoxMode Property

The `CheckBoxMode` property defines how checkbox states propagate in the hierarchy.

### Options

| Mode | Description |
|------|-------------|
| **Recursive** | Checking a parent automatically checks all children; unchecking a child propagates to parent |
| **Cascade** | Similar to Recursive but with stricter cascade behavior |
| **None** | Checkboxes work independently (default) |

### Example: Recursive Mode

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       ItemsSource="{Binding Items}"
                       CheckBoxMode="Recursive"
                       ItemTemplateContextType="Node">
    <!-- ItemTemplate with CheckBox -->
</syncfusion:SfTreeView>
```

When a parent is checked in Recursive mode:
- ✅ All child nodes automatically become checked
- ✅ Parent shows as checked if all children are checked
- ✅ Parent shows as partially checked if some children are checked

---

## Working with Checked Items

### Access Checked Items from ViewModel (Bound Mode)

```csharp
public class FileManagerViewModel
{
    public ObservableCollection<object> CheckedItems { get; set; }

    public void ProcessCheckedItems()
    {
        foreach (var item in CheckedItems)
        {
            if (item is Folder folder)
            {
                // Process checked folder
                Debug.WriteLine($"Checked: {folder.FolderName}");
            }
        }
    }
}
```

### GetCheckedNodes() Method (Unbound Mode)

In unbound mode, use the `GetCheckedNodes()` method to retrieve all checked nodes.

```csharp
// Get all checked nodes
var checkedNodes = treeView.GetCheckedNodes();

foreach (var node in checkedNodes)
{
    Debug.WriteLine($"Checked Node: {node.Content}");
}
```

**Return Type:**
```csharp
// Returns: IEnumerable<TreeViewNode>
public IEnumerable<TreeViewNode> GetCheckedNodes()
```

**Key Points:**
- Returns a collection of all nodes with `IsChecked = true`
- Works only in unbound mode or when using direct `TreeViewNode` manipulation
- Useful for batch processing checked items
- Called at any time during application lifecycle

**Example: Process Checked Items**

```csharp
private void OnProcessButtonClicked(object sender, EventArgs e)
{
    var checkedNodes = treeView.GetCheckedNodes();
    
    if (!checkedNodes.Any())
    {
        DisplayAlert("Info", "No items selected", "OK");
        return;
    }

    var selectedContent = new List<string>();
    foreach (var node in checkedNodes)
    {
        selectedContent.Add(node.Content?.ToString() ?? "Unknown");
    }

    var result = string.Join(", ", selectedContent);
    DisplayAlert("Selected Items", result, "OK");
}
```

### Programmatically Set Checked State

```csharp
// Check all items
private void CheckAllItems()
{
    foreach (var node in treeView.Nodes)
    {
        SetCheckedRecursively(node, true);
    }
}

private void SetCheckedRecursively(TreeViewNode node, bool isChecked)
{
    node.IsChecked = isChecked;
    foreach (var childNode in node.ChildNodes)
    {
        SetCheckedRecursively(childNode, isChecked);
    }
}
```

---

## IsChecked Property

The `IsChecked` property of `TreeViewNode` represents the checkbox state.

### Binding in XAML

```xml
<checkbox:SfCheckBox IsChecked="{Binding IsChecked, Mode=TwoWay}"/>
```

### Property States

```csharp
public bool? IsChecked { get; set; }

// Three-state checkbox (if supported):
// true  = Checked
// false = Unchecked
// null  = Indeterminate (partial selection)
```

### Programmatically Setting Checked State

```csharp
// Set single node as checked
treeView.Nodes[0].IsChecked = true;

// Check all nodes
foreach (var node in treeView.Nodes)
{
    node.IsChecked = true;
}
```

---

## NodeChecked Event

The `NodeChecked` event is raised whenever a node's checkbox is checked or unchecked during user interaction or programmatic changes.

### Event Registration

```csharp
treeView.NodeChecked += OnTreeViewNodeChecked;

private void OnTreeViewNodeChecked(object sender, NodeCheckedEventArgs e)
{
    var checkedNode = e.Node;
    Debug.WriteLine($"Node Checked: {checkedNode.Content}");
}
```

### NodeCheckedEventArgs Properties

The `NodeCheckedEventArgs` provides the following information:

| Property | Type | Description |
|----------|------|-------------|
| `Node` | `TreeViewNode` | The node that was checked/unchecked |
| `Node.Content` | `object` | The data associated with the node |
| `Node.IsChecked` | `bool?` | Current checked state |

### Complete Event Handler Example

```csharp
private void OnTreeViewNodeChecked(object sender, NodeCheckedEventArgs e)
{
    var node = e.Node;
    
    // Get the data associated with the node
    var nodeData = node.Content as Folder;
    
    if (node.IsChecked == true)
    {
        Debug.WriteLine($"Checked: {nodeData?.FolderName}");
    }
    else if (node.IsChecked == false)
    {
        Debug.WriteLine($"Unchecked: {nodeData?.FolderName}");
    }
}
```

### Event Behavior

**Important Notes:**
- ✅ Event fires for both check and uncheck operations
- ✅ Event only fires on **UI interactions** (not programmatic changes)
- ✅ When `CheckBoxMode` is enabled, `ItemTapped` and `ItemDoubleTapped` events are **not triggered**
- ❌ Programmatic calls like `node.IsChecked = true` do NOT raise this event

### Example: Validate Before Checking

```csharp
private void OnTreeViewNodeChecked(object sender, NodeCheckedEventArgs e)
{
    var node = e.Node;
    
    // Example: Don't allow checking more than 5 items
    var checkedCount = treeView.GetCheckedNodes().Count();
    
    if (checkedCount > 5 && node.IsChecked == true)
    {
        DisplayAlert("Limit", "Cannot select more than 5 items", "OK");
        node.IsChecked = false; // Uncheck it
    }
}
```

### Example: Track Checkbox Changes

```csharp
private List<string> checkedItems = new List<string>();

private void OnTreeViewNodeChecked(object sender, NodeCheckedEventArgs e)
{
    var node = e.Node;
    var content = node.Content?.ToString() ?? "Unknown";
    
    if (node.IsChecked == true)
    {
        if (!checkedItems.Contains(content))
        {
            checkedItems.Add(content);
        }
    }
    else
    {
        checkedItems.Remove(content);
    }
    
    Debug.WriteLine($"Total Checked: {checkedItems.Count}");
}
```

---

## Comparison: Getting Checked Items

| Method | Mode | Use Case |
|--------|------|----------|
| `CheckedItems` collection | Bound Mode | Data binding with MVVM |
| `GetCheckedNodes()` | Unbound Mode | Manual node creation |
| `NodeChecked` event | Both | Real-time checkbox changes |
| Direct `IsChecked` property | Both | Individual node manipulation |

---

## Best Practices

### ✅ Do's

1. **Always use `ItemTemplateContextType="Node"`** when working with checkboxes
   ```xml
   <syncfusion:SfTreeView ItemTemplateContextType="Node">
   ```

2. **Use `CheckedItems` collection** in bound mode for MVVM
   ```csharp
   CheckedItems="{Binding CheckedItems}"
   ```

3. **Use `GetCheckedNodes()`** in unbound mode to retrieve checked items
   ```csharp
   var checkedNodes = treeView.GetCheckedNodes();
   ```

4. **Subscribe to `NodeChecked` event** for real-time checkbox changes
   ```csharp
   treeView.NodeChecked += OnTreeViewNodeChecked;
   ```

5. **Implement `INotifyPropertyChanged`** in your data models
   ```csharp
   public event PropertyChangedEventHandler PropertyChanged;
   ```

6. **Use `ObservableCollection`** for dynamic updates
   ```csharp
   public ObservableCollection<object> CheckedItems { get; set; }
   ```

7. **Bind with TwoWay mode** for checkbox updates
   ```xml
   IsChecked="{Binding IsChecked, Mode=TwoWay}"
   ```

### ❌ Don'ts

1. **Don't forget to set `ItemTemplateContextType="Node"`**
   - Without this, `IsChecked` binding will fail

2. **Don't use `CheckedItems` in unbound mode**
   - Use `GetCheckedNodes()` method instead

3. **Don't expect `NodeChecked` event for programmatic changes**
   - Event only fires on UI interactions
   - For programmatic checks, use `NodeChecked` event only for UI-initiated actions

4. **Don't mix checkbox with `SelectionMode`**
   - Choose either checkbox or selection, not both

5. **Don't perform heavy operations in `NodeChecked` event**
   - Use async/await for performance-intensive tasks

6. **Don't forget to unsubscribe from events** in cleanup
   ```csharp
   treeView.NodeChecked -= OnTreeViewNodeChecked;
   ```

---

## Common Issues and Troubleshooting

### Issue: CheckBox not showing
**Solution:** Add `ItemTemplateContextType="Node"` to TreeView
```xml
<syncfusion:SfTreeView ItemTemplateContextType="Node">
```

### Issue: Checked state not updating
**Solution:** Use two-way binding: `IsChecked="{Binding IsChecked, Mode=TwoWay}"`
```xml
<checkbox:SfCheckBox IsChecked="{Binding IsChecked, Mode=TwoWay}"/>
```

### Issue: CheckedItems collection is empty
**Solution:** Ensure `CheckBoxMode` is set appropriately and TreeView is bound correctly

### Issue: GetCheckedNodes() returns empty collection
**Cause:** In bound mode, `GetCheckedNodes()` won't work  
**Solution:** Use `CheckedItems` collection instead
```csharp
// ✅ Correct for unbound mode
var checkedNodes = treeView.GetCheckedNodes();

// ✅ Correct for bound mode
var checkedItems = treeView.CheckedItems;
```

### Issue: NodeChecked event not firing
**Cause:** Programmatic checkbox changes don't trigger the event  
**Solution:** Handle UI-initiated changes only or manually call a method for programmatic changes
```csharp
// NodeChecked event fires for this:
treeView.NodeChecked += OnNodeChecked; // User clicks checkbox

// NodeChecked event does NOT fire for this:
treeView.Nodes[0].IsChecked = true; // Programmatic change
```

### Issue: CheckBoxMode="Recursive" not working properly
**Cause:** CheckBoxMode needs to be set on TreeView declaration  
**Solution:** Ensure `CheckBoxMode` is set before binding data
```xml
<syncfusion:SfTreeView ItemsSource="{Binding Items}"
                       CheckBoxMode="Recursive">
</syncfusion:SfTreeView>
```

### Issue: Too many NodeChecked events firing
**Cause:** Cascading checks in Recursive mode trigger multiple events  
**Solution:** Use flag to prevent recursive handling
```csharp
private bool isUpdatingCheckState = false;

private void OnNodeChecked(object sender, NodeCheckedEventArgs e)
{
    if (isUpdatingCheckState)
        return;

    try
    {
        isUpdatingCheckState = true;
        // Handle the event
    }
    finally
    {
        isUpdatingCheckState = false;
    }
}
```

