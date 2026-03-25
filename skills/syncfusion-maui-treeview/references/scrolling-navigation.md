# Scrolling and Navigation in .NET MAUI TreeView (SfTreeView)

This guide covers programmatic scrolling, navigation methods, and keyboard interactions in TreeView.

## Table of Contents

- [Bring Into View](#bring-into-view)
- [Scroll Animation](#scroll-animation)
- [Scroll to Position](#scroll-to-position)
- [Horizontal Scrolling](#horizontal-scrolling)
- [Scrollbar Visibility](#scrollbar-visibility)
- [Keyboard Navigation](#keyboard-navigation)
- [Events](#events)
- [Best Practices](#best-practices)

---

## Bring Into View

The `BringIntoView()` method programmatically scrolls a specific node into the visible area of the TreeView.

### Basic Usage

```csharp
private void BringIntoView_Clicked(object sender, EventArgs e)
{
    var lastItem = viewModel.Items[viewModel.Items.Count - 1];
    treeView.BringIntoView(lastItem);
}
```

### Method Overloads

```csharp
// Overload 1: Basic
treeView.BringIntoView(treeViewNode);

// Overload 2: With animation control
treeView.BringIntoView(treeViewNode, disableAnimation: true);

// Overload 3: With expand option
treeView.BringIntoView(treeViewNode, disableAnimation: false, canExpand: true);

// Overload 4: With scroll position
treeView.BringIntoView(treeViewNode, disableAnimation: false, canExpand: true, 
                       scrollToPosition: ScrollToPosition.Center);
```

---

## Scroll Animation

Control whether scrolling includes animation effects.

### Disable Animation

```csharp
// Scroll without animation
treeView.BringIntoView(dataItem, disableAnimation: true);
```

### Enable Animation (Default)

```csharp
// Scroll with smooth animation
treeView.BringIntoView(dataItem, disableAnimation: false);
```

**Use Cases:**
- `disableAnimation = true`: Instant scrolling, performance-critical scenarios
- `disableAnimation = false`: Smooth UX, general navigation

---

## Scroll to Position

Position the scrolled item at a specific location on the screen using the fourth parameter.

### Scroll Position Options

```csharp
// Using ScrollToPosition enum
ScrollToPosition.Start      // Item at top of view (default)
ScrollToPosition.MakeVisible // Item visible, no unnecessary scroll
ScrollToPosition.Center      // Item centered on screen
ScrollToPosition.End         // Item at bottom of view
```

### Examples

```csharp
// Bring item to view centered
private void ScrollToCenter()
{
    var item = viewModel.Items[5];
    treeView.BringIntoView(item, false, false, ScrollToPosition.Center);
}

// Make item just visible without excess scrolling
private void ScrollMakeVisible()
{
    var item = viewModel.Items[10];
    treeView.BringIntoView(item, false, false, ScrollToPosition.MakeVisible);
}

// Scroll to bottom
private void ScrollToEnd()
{
    var item = viewModel.Items.Last();
    treeView.BringIntoView(item, false, false, ScrollToPosition.End);
}
```

---

## Expanding Collapsed Items During Scroll

The third parameter `canExpand` determines if collapsed nodes should be expanded when scrolling to them.

### Expand and Scroll to Item

```csharp
// Expand parent nodes and scroll to item
treeView.BringIntoView(nestedItem, 
                       disableAnimation: false, 
                       canExpand: true,          // ← Expand collapsed parents
                       scrollToPosition: ScrollToPosition.MakeVisible);
```

**Requirements:**
- Set `NodePopulationMode = TreeNodePopulationMode.Instant`
- Parent nodes will be expanded automatically

### Example: Find and Scroll to Nested Item

```csharp
private void ScrollToNestedItem()
{
    // Assume you want to scroll to item in deeply nested structure
    var nestedItem = FindItemInHierarchy("Target Item");
    
    if (nestedItem != null)
    {
        treeView.BringIntoView(nestedItem, 
                               disableAnimation: false, 
                               canExpand: true);
    }
}

private object FindItemInHierarchy(string itemName)
{
    // Search through your data structure
    // Return the item when found
    return null;
}
```

---

## Horizontal Scrolling

Enable horizontal scrolling when content exceeds the TreeView width.

### Enable Horizontal Scrolling

**XAML:**
```xml
<syncfusion:SfTreeView x:Name="treeView"
                       EnableHorizontalScrolling="True"/>
```

**C#:**
```csharp
treeView.EnableHorizontalScrolling = true;
```

### With Nested Content

When items have long text or wide templates, horizontal scrolling provides full content access:

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       EnableHorizontalScrolling="True"
                       ItemsSource="{Binding LongNamedItems}">
    <syncfusion:SfTreeView.ItemTemplate>
        <DataTemplate>
            <Label Text="{Binding VeryLongItemName}" 
                   FontSize="16"/>
        </DataTemplate>
    </syncfusion:SfTreeView.ItemTemplate>
</syncfusion:SfTreeView>
```

---

## Scrollbar Visibility

Control the visibility of vertical and horizontal scrollbars.

### Vertical Scrollbar Visibility

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       VerticalScrollBarVisibility="Always"/>
```

**Options:**
- `Default` - Automatic (default)
- `Always` - Always visible
- `Never` - Always hidden

### Horizontal Scrollbar Visibility

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       HorizontalScrollBarVisibility="Always"/>
```

### C# Configuration

```csharp
treeView.VerticalScrollBarVisibility = ScrollBarVisibility.Always;
treeView.HorizontalScrollBarVisibility = ScrollBarVisibility.Always;
```

### Complete Example

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       VerticalScrollBarVisibility="Always"
                       HorizontalScrollBarVisibility="Always"
                       EnableHorizontalScrolling="True"
                       ItemsSource="{Binding Items}">
</syncfusion:SfTreeView>
```

---

## Keyboard Navigation

TreeView supports keyboard navigation for accessibility and power user workflows.

### Arrow Keys

| Key | Action |
|-----|--------|
| **Right Arrow** | Expand focused node (if collapsible) |
| **Left Arrow** | Collapse focused node (if expandable) |
| **Up Arrow** | Focus previous node |
| **Down Arrow** | Focus next node |
| **Tab** | Move focus to next control (outside TreeView) |
| **Shift+Tab** | Move focus to previous control (outside TreeView) |

### Keyboard Expand/Collapse

```csharp
// When a node has focus:
// Press Right Arrow to expand
// Press Left Arrow to collapse

// Example: Focus and expand programmatically
private void FocusAndExpandNode(TreeViewNode node)
{
    treeView.ExpandNode(node);
    // Keyboard focus can then navigate with arrow keys
}
```

### Platform-Specific Navigation (WinUI, macOS)

On desktop platforms (WinUI, macOS), additional keyboard shortcuts may be available:

- **Home Key**: Navigate to first node
- **End Key**: Navigate to last node
- **Page Up**: Scroll up by page
- **Page Down**: Scroll down by page

---

## Best Practices

### ✅ Do's

1. **Use `BringIntoView` for search results**
   ```csharp
   var searchResult = FindItemByName("Documents");
   if (searchResult != null)
       treeView.BringIntoView(searchResult, canExpand: true);
   ```

2. **Enable horizontal scrolling for long content**
   ```xml
   <syncfusion:SfTreeView EnableHorizontalScrolling="True"/>
   ```

3. **Use `ScrollToPosition.Center`** for better UX on small screens
   ```csharp
   treeView.BringIntoView(item, scrollToPosition: ScrollToPosition.Center);
   ```

4. **Handle `Loaded` event** for initial navigation
   ```csharp
   treeView.Loaded += (s, e) => ScrollToFirstUnreadItem();
   ```

5. **Disable animation** for instant navigation in performance-critical code
   ```csharp
   treeView.BringIntoView(item, disableAnimation: true);
   ```

### ❌ Don'ts

1. **Don't call `BringIntoView` before TreeView is loaded**
   - Wait for `Loaded` event first

2. **Don't scroll to collapsed items without `canExpand: true`**
   - Item won't be visible if parent is collapsed

3. **Don't set both `VerticalScrollBarVisibility` and `HorizontalScrollBarVisibility` to "Never"**
   - Users won't be able to see all content

4. **Don't overuse keyboard shortcuts on mobile**
   - Mobile users won't have keyboard navigation

5. **Don't perform heavy work in scroll events**
   - Use throttling/debouncing for performance

---

## Common Scenarios

### Scroll to Last Item on Load

```csharp
private void TreeView_Loaded(object sender, TreeViewLoadedEventArgs e)
{
    if (viewModel.Items.Count > 0)
    {
        var lastItem = viewModel.Items.Last();
        treeView.BringIntoView(lastItem, 
                               disableAnimation: false,
                               scrollToPosition: ScrollToPosition.End);
    }
}
```

### Search and Navigate

```csharp
private void SearchItems(string searchTerm)
{
    var matchingItems = FindItemsByName(searchTerm);
    
    if (matchingItems.Any())
    {
        var firstMatch = matchingItems.First();
        treeView.BringIntoView(firstMatch, 
                               canExpand: true,
                               scrollToPosition: ScrollToPosition.Center);
    }
}
```

### Context Menu on Right-Tap

```csharp
private async void ShowContextMenu(TreeViewNode node)
{
    var action = await DisplayActionSheet("Actions", 
        "Cancel", 
        null, 
        "Edit", "Delete", "Duplicate");
    
    if (action == "Edit")
    {
        // Handle edit
    }
}
```
