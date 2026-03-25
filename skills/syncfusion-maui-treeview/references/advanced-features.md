# Advanced Features

Comprehensive guide to advanced TreeView capabilities. Quick reference for all advanced features with links to dedicated documentation for in-depth coverage.

## Table of Contents
- [Right-to-Left (RTL)](#right-to-left-rtl)
- [Working with TreeViewNode](#working-with-treeviewnode)
- [Events](#events) 

---

## Right-to-Left (RTL)

Support for right-to-left languages and layouts.

> - RTL implementation patterns
> - Multi-language support (Arabic, Hebrew, Persian)
> - Dynamic RTL toggle
> - RTL templates and keyboard navigation
> - Platform-specific considerations

### Enable RTL

```xml
<syncfusion:SfTreeView x:Name="treeView" FlowDirection="RightToLeft"/>
```

```csharp
treeView.FlowDirection = FlowDirection.RightToLeft;
```

---

## Working with TreeViewNode

### Accessing TreeViewNode

```csharp
// Get node at index
var node = treeView.Nodes[5];

// Get all nodes
var allNodes = treeView.Nodes;
```

### Node Properties

```csharp
var node = treeView.GetNodeAtRowIndex(0);

var level = node.Level;                 // Depth in tree
var hasChildNodes = node.HasChildNodes; // Has children
var isExpanded = node.IsExpanded;       // Expanded state
var content = node.Content;             // Data object
var parentNode = node.ParentNode;       // Parent reference
var childNodes = node.ChildNodes;       // Children
```

---

## Events
### Loaded Event

```csharp
treeView.Loaded += OnTreeViewLoaded;

private void OnTreeViewLoaded(object sender, TreeViewLoadedEventArgs e)
{
    // TreeView fully loaded
}
```

### ItemTapped Event

```csharp
treeView.ItemTapped += OnItemTapped;

private void OnItemTapped(object sender, ItemTappedEventArgs e)
{
    var node = e.Node;
    var data = node.Content as FileManager;
}
```

### ItemDoubleTapped Event

```csharp
treeView.ItemDoubleTapped += OnItemDoubleTapped;

private void OnItemDoubleTapped(object sender, ItemDoubleTappedEventArgs e)
{
    var node = e.Node;
    // Handle double tap
}
```

### ItemRightTapped Event

```csharp
treeView.ItemRightTapped  += OnItemRightTapped;

private void OnItemRightTapped(object sender, ItemRightTappedEventArgs e)
{
    var node = e.Node;
    // Handle right tap
}
```

### ItemLongPress Event

```csharp
treeView.ItemLongPress += OnItemLongPress;

private void OnItemLongPress(object sender, ItemLongPressEventArgs e)
{
    var node = e.Node;
    var position = e.Position;
    // Show context menu
}
```

### KeyDown Event

```csharp
treeView.KeyDown += OnKeyDown;

private void OnKeyDown(object sender, TreeViewKeyEventArgs e)
{
    if (e.Key == "F2")
    {
        // Enter edit mode
        e.Handled = true;
    }
}
```

---

## Best Practices

1. **Use LoadOnDemand** for large hierarchical datasets
2. **Leverage checkboxes** for multi-selection scenarios
3. **Implement empty views** for better UX
4. **Use events judiciously** - prefer commands in MVVM
5. **Call ResetTreeViewItems** after changing ItemsSource

---