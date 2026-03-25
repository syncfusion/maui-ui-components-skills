# Drag and Drop in TreeView

This guide explains how to implement drag-and-drop functionality for reordering nodes.

## Table of Contents
- [Overview](#overview)
- [Enable Drag and Drop](#enable-drag-and-drop)
- [Dragging Multiple Items](#dragging-multiple-items)
- [Customize Drag Item View](#customize-drag-item-view)
- [ItemDragging Event](#itemdragging-event)
- [Auto Scroll Options](#auto-scroll-options)
- [Auto Expand](#auto-expand)
- [Restrictions and Validation](#restrictions-and-validation)
- [Limitations](#limitations)

---

## Overview

TreeView supports drag-and-drop reordering of nodes within the tree. Nodes can be dropped:
- **Above** another node
- **Below** another node
- **As a child** of another node

---

## Enable Drag and Drop

Set the `AllowDragging` property to `true`.

**XAML:**

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       ItemsSource="{Binding Folders}"
                       ChildPropertyName="SubFiles"
                       AllowDragging="True"/>
```

**C#:**

```csharp
treeView.AllowDragging = true;
```

**Note:** Drag-and-drop is NOT supported when Load on Demand is enabled.

---

## Dragging Multiple Items

Enable multiple selection to drag multiple items simultaneously.

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       SelectionMode="Multiple"
                       AllowDragging="True"
                       ItemsSource="{Binding Files}"
                       ChildPropertyName="SubFiles"/>
```

**Behavior:**
- Select multiple items using `Multiple` or `Extended` selection mode
- Drag any selected item to move all selected items together

---

## Customize Drag Item View

Use `DragItemTemplate` to customize the dragging visual.

```xml
<syncfusion:SfTreeView AllowDragging="True">
    <syncfusion:SfTreeView.DragItemTemplate>
        <DataTemplate>
            <Border Padding="8" 
                    StrokeThickness="1"  
                    Stroke="#6750A4"
                    Background="White">
                <Border.StrokeShape>
                    <RoundRectangle CornerRadius="8"/>
                </Border.StrokeShape>
                <HorizontalStackLayout Spacing="8">
                    <Image Source="{Binding ImageIcon}"
                           WidthRequest="24" 
                           HeightRequest="24"/>
                    <Label Text="{Binding FolderName}"
                           VerticalOptions="Center"/>
                </HorizontalStackLayout>
            </Border>
        </DataTemplate>
    </syncfusion:SfTreeView.DragItemTemplate>
</syncfusion:SfTreeView>
```

---

## ItemDragging Event

Handle drag-and-drop lifecycle events.

### Event Actions

| Action | Description | When Fired |
|--------|-------------|------------|
| `Start` | Drag operation begins | When user starts dragging |
| `Dragging` | Item is being dragged | Continuously while dragging |
| `Dropping` | Item is about to be dropped | Before drop completes |
| `Drop` | Item was dropped | After drop completes |

### Basic Event Handling

```csharp
treeView.ItemDragging += TreeView_ItemDragging;

private void TreeView_ItemDragging(object sender, ItemDraggingEventArgs e)
{
    switch (e.Action)
    {
        case DragAction.Start:
            // Drag started
            break;
            
        case DragAction.Dragging:
            // Currently dragging
            break;
            
        case DragAction.Dropping:
            // About to drop
            break;
            
        case DragAction.Drop:
            // Dropped
            break;
    }
}
```

### Disable Dragging for Specific Items

```csharp
private void TreeView_ItemDragging(object sender, ItemDraggingEventArgs e)
{
    if (e.Action == DragAction.Start)
    {
        var item = e.DraggingNodes[0].Content as FileManager;
        
        // Prevent dragging locked files
        if (item.IsLocked)
        {
            e.Cancel = true;
        }
    }
}
```

### Cancel Dropping

```csharp
private void TreeView_ItemDragging(object sender, ItemDraggingEventArgs e)
{
    if (e.Action == DragAction.Dropping)
    {
        var targetItem = e.TargetNode.Content as Folder;
        
        // Prevent dropping into read-only folders
        if (targetItem.IsReadOnly)
        {
            e.Cancel = true;
        }
    }
}
```

---

## Auto Scroll Options

### Scroll Margin

Adjust the margin that triggers auto-scrolling.

```csharp
// Auto-scroll when drag item is within 20px of edge
treeView.AutoScroller.ScrollMargin = 20;

// Disable auto-scroll
treeView.AutoScroller.ScrollMargin = 0;
```

**Default:** 15 pixels

### Scroll Interval

Adjust the auto-scroll speed.

```csharp
// Scroll every 200 milliseconds
treeView.AutoScroller.Interval = new TimeSpan(0, 0, 0, 0, 200);
```

**Default:** 150 milliseconds

### Disable Outside Scroll

Prevent scrolling when dragged item leaves the TreeView bounds.

```csharp
treeView.AutoScroller.AllowOutsideScroll = false;
```

**Default:** `true` (allows outside scroll)

---

## Auto Expand

### Enable Auto Expand

Automatically expand nodes when hovering during drag.

**XAML:**

```xml
<syncfusion:SfTreeView AllowDragging="True">
    <syncfusion:SfTreeView.DragAndDropController>
        <syncfusion:DragAndDropController CanAutoExpand="True"/>
    </syncfusion:SfTreeView.DragAndDropController>
</syncfusion:SfTreeView>
```

**C#:**

```csharp
treeView.DragAndDropController.CanAutoExpand = true;
```

### Auto Expand Delay

Set the delay before auto-expansion.

**XAML:**

```xml
<syncfusion:DragAndDropController CanAutoExpand="True" 
                                 AutoExpandDelay="0:0:1"/>
```

**C#:**

```csharp
treeView.DragAndDropController.AutoExpandDelay = new TimeSpan(0, 0, 1);
```

**Default:** 3 seconds

---

## Restrictions and Validation

### Validate Drop Position

```csharp
private void TreeView_ItemDragging(object sender, ItemDraggingEventArgs e)
{
    if (e.Action == DragAction.Dropping)
    {
        var draggedItem = e.DraggingNodes[0].Content as FileItem;
        var targetItem = e.TargetNode.Content as FileItem;
        
        // Only allow files to be dropped into folders
        if (draggedItem.IsFile && !targetItem.IsFolder)
        {
            e.Cancel = true;
        }
        
        // Prevent dropping parent into its own child
        if (IsDescendant(e.TargetNode, e.DraggingNodes[0]))
        {
            e.Cancel = true;
        }
    }
}

private bool IsDescendant(TreeViewNode target, TreeViewNode source)
{
    var node = target;
    while (node != null)
    {
        if (node == source)
            return true;
        node = node.ParentNode;
    }
    return false;
}
```

### Custom Drop Indicator

```csharp
private void TreeView_ItemDragging(object sender, ItemDraggingEventArgs e)
{
    if (e.Action == DragAction.Dragging)
    {
        // Access drag position
        var position = e.Position;
        
        // Access drop position indicator
        var dropPosition = e.DropPosition;
        
        // Handle custom visualization
        if (e.Handled)
        {
            // Custom drag handling
        }
    }
}
```

---

## Limitations

### Invalid Drop Scenarios

Drag-and-drop will show an "Invalid drop" indicator in these cases:

1. **Drop as child into same node**
   - Cannot drop a node as its own child

2. **Incompatible child node type** (with HierarchyPropertyDescriptors)
   - Target node's child type doesn't match dragged item type

3. **Different parent types** (with HierarchyPropertyDescriptors)
   - Siblings must have compatible types

### Not Supported

- **Load on Demand:** Drag-and-drop cannot be used with load-on-demand enabled
- **Cross-TreeView:** Cannot drag between different TreeView controls
- **External Drops:** Cannot drag items from outside the TreeView

---

## Best Practices

### Performance

1. **Disable animations during heavy drag operations**
2. **Limit auto-expand delay** to improve responsiveness
3. **Use `Handled` property** in `ItemDragging` for custom drag logic

### User Experience

1. **Provide visual feedback** using custom `DragItemTemplate`
2. **Show drop indicators** clearly
3. **Validate drops** in `Dropping` action, not `Drop`
4. **Use auto-expand** for deep hierarchies

### Example: Complete Drag Implementation

```csharp
public class DragDropViewModel
{
    public void SetupDragDrop(SfTreeView treeView)
    {
        treeView.AllowDragging = true;
        treeView.DragAndDropController.CanAutoExpand = true;
        treeView.DragAndDropController.AutoExpandDelay = TimeSpan.FromSeconds(1);
        treeView.AutoScroller.ScrollMargin = 20;
        
        treeView.ItemDragging += OnItemDragging;
    }
    
    private void OnItemDragging(object sender, ItemDraggingEventArgs e)
    {
        switch (e.Action)
        {
            case DragAction.Start:
                ValidateDragStart(e);
                break;
                
            case DragAction.Dropping:
                ValidateDrop(e);
                break;
                
            case DragAction.Drop:
                HandleDrop(e);
                break;
        }
    }
    
    private void ValidateDragStart(ItemDraggingEventArgs e)
    {
        var item = e.DraggingNodes[0].Content as FileItem;
        if (item.IsLocked)
        {
            e.Cancel = true;
        }
    }
    
    private void ValidateDrop(ItemDraggingEventArgs e)
    {
        // Add validation logic
    }
    
    private void HandleDrop(ItemDraggingEventArgs e)
    {
        // Update data model
        // Notify UI
    }
}
```

---

## Sample Projects

- [Drag and Drop Customization](https://github.com/SyncfusionExamples/how-to-customize-the-drag-item-view)
---

## Related Topics

- [Selection](selection.md) - Multi-select for drag
- [Events](advanced-features.md#events) - Additional event handling
