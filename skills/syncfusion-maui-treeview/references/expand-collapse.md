# Expand and Collapse in TreeView

This guide explains how to control node expansion and collapse behavior in the TreeView.

## Table of Contents
- [Overview](#overview)
- [Expand Action Target](#expand-action-target)
- [Auto Expand Mode](#auto-expand-mode)
- [Programmatic Expand and Collapse](#programmatic-expand-and-collapse)
- [Keyboard Expansion](#keyboard-expansion)
- [Binding IsExpanded Property](#binding-isexpanded-property)
- [Expand and Collapse Events](#expand-and-collapse-events)
- [Best Practices](#best-practices)

---

## Overview

TreeView nodes can be expanded and collapsed through:
- **User Interaction:** Clicking the expander icon or node content
- **Programmatic Control:** Using methods and properties
- **Keyboard:** Arrow keys on desktop platforms
- **Initial State:** Auto-expand configuration

---

## Expand Action Target

The `ExpandActionTarget` property determines which part of the item triggers expand/collapse.

### Options

| Value | Description | Use Case |
|-------|-------------|----------|
| `Expander` | Only expander icon triggers expansion (default) | Standard behavior, precise control |
| `Node` | Entire node triggers expansion | Quick navigation, touch-friendly |

### Setting Expand Action Target

**XAML:**

```xml
<syncfusion:SfTreeView x:Name="treeView" 
                       ExpandActionTarget="Node"/>
```

**C#:**

```csharp
// Expand by tapping entire node
treeView.ExpandActionTarget = TreeViewExpandActionTarget.Node;

// Expand only by tapping expander icon
treeView.ExpandActionTarget = TreeViewExpandActionTarget.Expander;
```

**Behavior Comparison:**

- **Expander:** Users must click the small expander icon (more precise, less accidental expansions)
- **Node:** Users can click anywhere on the node (faster, more touch-friendly)

---

## Auto Expand Mode

Control the initial expansion state of nodes when the TreeView loads.

### Available Modes

| Mode | Description | Use Case |
|------|-------------|----------|
| `None` | All nodes collapsed (default) | Large datasets, user-driven exploration |
| `RootNodesExpanded` | Only root nodes expanded | Show top-level categories |
| `AllNodesExpanded` | All nodes expanded | Small datasets, overview needed |

### Setting Auto Expand Mode

**XAML:**

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       AutoExpandMode="RootNodesExpanded"
                       ItemsSource="{Binding Files}"
                       ChildPropertyName="SubFiles"/>
```

**C#:**

```csharp
treeView.AutoExpandMode = TreeViewAutoExpandMode.RootNodesExpanded;
```

### Auto Expand Mode Examples

#### Expand All Nodes

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       AutoExpandMode="AllNodesExpanded"
                       ItemsSource="{Binding Items}"
                       ChildPropertyName="SubItems"/>
```

**Use Case:** Small hierarchies where users need to see all data immediately (settings, configuration trees).

#### Expand Root Nodes Only

```csharp
treeView.AutoExpandMode = TreeViewAutoExpandMode.RootNodesExpanded;
```

**Use Case:** File explorers, category trees where top-level view is needed but details are optional.

#### All Nodes Collapsed

```csharp
treeView.AutoExpandMode = TreeViewAutoExpandMode.None;
```

**Use Case:** Large datasets, performance-critical scenarios.

### Bound vs Unbound Mode

⚠️ **Important:**
- **Bound Mode:** Use `AutoExpandMode` property
- **Unbound Mode:** Set `IsExpanded = true` on individual `TreeViewNode` objects

**Unbound Example:**

```csharp
var node = new TreeViewNode() 
{ 
    Content = "Documents",
    IsExpanded = true  // Node starts expanded
};
```

---

## Programmatic Expand and Collapse

### Expand/Collapse Single Node

```csharp
// Get a specific node
var node = treeView.Nodes[0];

// Expand the node
treeView.ExpandNode(node);

// Collapse the node
treeView.CollapseNode(node);
```

### Expand/Collapse by Level

```csharp
// Expand all nodes at root level (level 0)
treeView.ExpandNodes(0);

// Collapse all nodes at level 1
treeView.CollapseNodes(1);

// Expand nodes at multiple levels
treeView.ExpandNodes(0);
treeView.ExpandNodes(1);
```

**Level Reference:**
- Level 0: Root nodes
- Level 1: Children of root nodes
- Level 2: Grand-children of root nodes
- And so on...

### Expand/Collapse All Nodes

```csharp
// Expand all nodes in the tree
treeView.ExpandAll();

// Collapse all nodes in the tree
treeView.CollapseAll();
```

**Use Cases:**
- "Expand All" button implementation
- "Collapse All" button implementation
- Resetting tree state
- Quick navigation features

### Complete Example with Buttons

```xml
<StackLayout>
    <HorizontalStackLayout Spacing="10" Padding="10">
        <Button Text="Expand All" Clicked="OnExpandAll"/>
        <Button Text="Collapse All" Clicked="OnCollapseAll"/>
        <Button Text="Expand Root" Clicked="OnExpandRoot"/>
    </HorizontalStackLayout>
    
    <syncfusion:SfTreeView x:Name="treeView"
                           ItemsSource="{Binding Files}"
                           ChildPropertyName="SubFiles"/>
</StackLayout>
```

```csharp
private void OnExpandAll(object sender, EventArgs e)
{
    treeView.ExpandAll();
}

private void OnCollapseAll(object sender, EventArgs e)
{
    treeView.CollapseAll();
}

private void OnExpandRoot(object sender, EventArgs e)
{
    treeView.ExpandNodes(0);
}
```

---

## Keyboard Expansion

TreeView supports keyboard-based expansion on desktop platforms.

### Keyboard Shortcuts

| Key | Action |
|-----|--------|
| **Right Arrow** | Expand focused node |
| **Left Arrow** | Collapse focused node |
| **Right Arrow** (expanded) | Move to first child |
| **Left Arrow** (collapsed) | Move to parent |

**Example Usage:**

```csharp
// Ensure keyboard focus is enabled
treeView.Focus();

// Users can now use arrow keys to expand/collapse
```

**Platforms:** Windows, macOS (not available on mobile platforms)

---

## Binding IsExpanded Property

In unbound mode, bind the `IsExpanded` property to a ViewModel property.

### XAML Binding

```xml
<syncfusion:SfTreeView x:Name="treeview">
    <syncfusion:SfTreeView.Nodes>
        <treeviewengine:TreeViewNode 
            Content="United States of America" 
            IsExpanded="{Binding Path=IsExpanded, Source={x:Reference viewmodel}}">
            <treeviewengine:TreeViewNode.ChildNodes>
                <treeviewengine:TreeViewNode 
                    Content="New York" 
                    IsExpanded="{Binding Path=IsExpanded, Source={x:Reference viewmodel}}"/>
            </treeviewengine:TreeViewNode.ChildNodes>
        </treeviewengine:TreeViewNode>
    </syncfusion:SfTreeView.Nodes>
</syncfusion:SfTreeView>
```

### ViewModel Implementation

```csharp
public class ViewModel : INotifyPropertyChanged
{
    private bool isExpanded;

    public bool IsExpanded
    {
        get { return isExpanded; }
        set
        {
            isExpanded = value;
            OnPropertyChanged("IsExpanded");
        }
    }

    public ViewModel()
    {
        // Start with all nodes expanded
        IsExpanded = true;
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Toggle Expansion from ViewModel

```csharp
// In ViewModel
public ICommand ToggleExpansionCommand { get; }

public ViewModel()
{
    ToggleExpansionCommand = new Command(() =>
    {
        IsExpanded = !IsExpanded;
    });
}
```

```xml
<Button Text="Toggle All Nodes" 
        Command="{Binding ToggleExpansionCommand}"/>
```

---

## Expand and Collapse Events

### Available Events

| Event | Timing | Cancellable | Description |
|-------|--------|-------------|-------------|
| `NodeExpanding` | Before expansion | Yes | Validate or prevent expansion |
| `NodeExpanded` | After expansion | No | Respond to expansion |
| `NodeCollapsing` | Before collapsing | Yes | Validate or prevent collapse |
| `NodeCollapsed` | After collapsing | No | Respond to collapse |

### NodeExpanding Event

Fired before a node expands. Can be cancelled.

```csharp
treeView.NodeExpanding += TreeView_NodeExpanding;

private void TreeView_NodeExpanding(object sender, NodeExpandingCollapsingEventArgs e)
{
    var node = e.Node;
    var data = node.Content as FileManager;
    
    // Prevent expansion for specific items
    if (data.IsLocked)
    {
        e.Cancel = true;
        DisplayAlert("Locked", "This folder is locked", "OK");
        return;
    }
    
    // Show loading indicator for large datasets
    if (node.ChildNodes.Count > 100)
    {
        ShowLoadingIndicator();
    }
}
```

### NodeExpanded Event

Fired after a node expands successfully.

```csharp
treeView.NodeExpanded += TreeView_NodeExpanded;

private void TreeView_NodeExpanded(object sender, NodeExpandedCollapsedEventArgs e)
{
    var node = e.Node;
    var data = node.Content as FileManager;
    
    // Log expansion
    Debug.WriteLine($"Expanded: {data.ItemName}");
    
    // Load child data if needed
    if (node.ChildNodes.Count == 0)
    {
        LoadChildNodes(node);
    }
    
    // Update UI
    HideLoadingIndicator();
}
```

### NodeCollapsing Event

Fired before a node collapses. Can be cancelled.

```csharp
treeView.NodeCollapsing += TreeView_NodeCollapsing;

private void TreeView_NodeCollapsing(object sender, NodeExpandingCollapsingEventArgs e)
{
    var node = e.Node;
    
    // Prevent collapsing root nodes
    if (node.Level == 0)
    {
        e.Cancel = true;
        return;
    }
    
    // Confirm before collapsing if it has many children
    if (node.ChildNodes.Count > 50)
    {
        var result = await DisplayAlert(
            "Confirm", 
            "Collapse node with many children?", 
            "Yes", "No");
            
        if (!result)
        {
            e.Cancel = true;
        }
    }
}
```

### NodeCollapsed Event

Fired after a node collapses successfully.

```csharp
treeView.NodeCollapsed += TreeView_NodeCollapsed;

private void TreeView_NodeCollapsed(object sender, NodeExpandedCollapsedEventArgs e)
{
    var node = e.Node;
    var data = node.Content as FileManager;
    
    // Log collapse
    Debug.WriteLine($"Collapsed: {data.ItemName}");
    
    // Clear cached data to free memory
    if (node.ChildNodes.Count > 100)
    {
        ClearCachedData(node);
    }
}
```

### Event Usage with Commands

You can also use `ExpandCommand` and `CollapseCommand` properties for MVVM scenarios:

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       ExpandCommand="{Binding ExpandingCommand}"
                       CollapseCommand="{Binding CollapsingCommand}"/>
```

```csharp
public class ViewModel
{
    public ICommand ExpandingCommand { get; }
    public ICommand CollapsingCommand { get; }
    
    public ViewModel()
    {
        ExpandingCommand = new Command<TreeViewNode>(
            execute: (node) => OnNodeExpanded(node),
            canExecute: (node) => CanExpand(node));
            
        CollapsingCommand = new Command<TreeViewNode>(
            execute: (node) => OnNodeCollapsed(node),
            canExecute: (node) => CanCollapse(node));
    }
    
    private bool CanExpand(TreeViewNode node)
    {
        var data = node.Content as FileManager;
        return !data.IsLocked;
    }
    
    private void OnNodeExpanded(TreeViewNode node)
    {
        // Handle expansion
    }
    
    private bool CanCollapse(TreeViewNode node)
    {
        return node.Level > 0; // Don't collapse root
    }
    
    private void OnNodeCollapsed(TreeViewNode node)
    {
        // Handle collapse
    }
}
```

---

## Best Practices

### Performance Optimization

1. **Use `AutoExpandMode.None` for large datasets**
   ```csharp
   treeView.AutoExpandMode = TreeViewAutoExpandMode.None;
   ```

2. **Expand on demand instead of all at once**
   ```csharp
   // Better: Expand as user navigates
   treeView.NodeExpanding += LoadChildrenOnDemand;
   
   // Avoid: Expanding everything
   // treeView.ExpandAll(); // Can be slow for large trees
   ```


### User Experience

1. **Remember expansion state when refreshing**
   ```csharp
   private void RefreshData()
   {
       var expandedNodes = GetExpandedNodePaths();
       treeView.ItemsSource = GetNewData();
       RestoreExpansionState(expandedNodes);
   }
   ```

2. **Keyboard shortcuts for power users**
   - Document keyboard shortcuts in help
   - Provide toolbar buttons for expand/collapse all

### Common Patterns

#### Expand to Specific Node

```csharp
public void ExpandToNode(TreeViewNode targetNode)
{
    var node = targetNode;
    while (node != null)
    {
        if (node.ParentNode != null)
        {
            treeView.ExpandNode(node.ParentNode);
        }
        node = node.ParentNode;
    }
    
    // Scroll to the target node
    treeView.BringIntoView(targetNode);
}
```

#### Toggle Node Expansion

```csharp
public void ToggleNodeExpansion(TreeViewNode node)
{
    if (node.IsExpanded)
    {
        treeView.CollapseNode(node);
    }
    else
    {
        treeView.ExpandNode(node);
    }
}
```

#### Expand First N Levels

```csharp
public void ExpandFirstNLevels(int levels)
{
    for (int i = 0; i < levels; i++)
    {
        treeView.ExpandNodes(i);
    }
}

// Usage
ExpandFirstNLevels(2); // Expand root and first child level
```

---

## Common Issues and Solutions

### Issue: AutoExpandMode Not Working

**Problem:** Nodes don't expand on load even with `AutoExpandMode` set.

**Solution:** Ensure you're in bound mode, not unbound mode. In unbound mode, set `IsExpanded` on nodes directly.

### Issue: Programmatic Expansion Not Visible

**Problem:** Called `ExpandNode()` but node doesn't appear expanded.

**Solution:** Ensure the node has child nodes. Empty nodes won't show expansion.

```csharp
if (node.HasChildNodes)
{
    treeView.ExpandNode(node);
}
```

### Issue: Event Fires Multiple Times

**Problem:** `NodeExpanding` event fires repeatedly.

**Solution:** Check if node is already expanded before processing:

```csharp
private void TreeView_NodeExpanding(object sender, NodeExpandingCollapsingEventArgs e)
{
    if (e.Node.IsExpanded)
        return; // Already expanded
        
    // Process expansion
}
```

---

## Related Topics

- [Load on Demand](advanced-features.md#load-on-demand) - Lazy load child nodes
- [Data Binding](data-binding.md) - Configure hierarchical data
- [MVVM Support](mvvm-support.md) - Use commands for expand/collapse
