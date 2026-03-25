# Filtering in TreeView

Guide to filtering nodes in the TreeView control.

## Overview

TreeView provides built-in text filtering and custom predicate filtering to quickly locate nodes in hierarchical data.

---

## FilterMode

Available filter modes:

| Mode | Description |
|------|-------------|
| `None` | No filtering (default) |
| `Contains` | Display text contains filter text |
| `StartsWith` | Display text starts with filter text |
| `Equals` | Display text exactly matches filter text |
| `Custom` | Use custom predicate function |

**XAML:**

```xml
<syncfusion:SfTreeView FilterMode="Contains"/>
```

**C#:**

```csharp
treeView.FilterMode = TreeViewFilterMode.Contains;
```

---

## FilterText

Bindable property for filter text:

```xml
<Entry Placeholder="Filter..." 
       Text="{Binding FilterText, Mode=TwoWay}"/>

<syncfusion:SfTreeView FilterText="{Binding FilterText}"
                       FilterMode="Contains"
                       FilterPath="ItemName"/>
```

---

## FilterPath and FilterPaths

### Single Field Filtering

```xml
<syncfusion:SfTreeView FilterText="{Binding FilterText}"
                       FilterPath="Name"
                       FilterMode="Contains"/>
```

### Multi-Field Filtering

```xml
<syncfusion:SfTreeView FilterMode="Contains">
    <syncfusion:SfTreeView.FilterPaths>
        <x:String>Name</x:String>
        <x:String>Code</x:String>
        <x:String>Description</x:String>
    </syncfusion:SfTreeView.FilterPaths>
</syncfusion:SfTreeView>
```

---

## Custom Predicate Filtering

For advanced scenarios:

```csharp
treeView.FilterMode = TreeViewFilterMode.Custom;
treeView.FilterPredicate = (item) =>
{
    var file = item as FileManager;
    return file.Size > 1000 && file.IsModified;
};
treeView.RefreshFilter();
```

---

## AutoExpandOnFilter

Automatically expand nodes containing matches:

```xml
<syncfusion:SfTreeView AutoExpandOnFilter="True"
                       FilterMode="Contains"/>
```

---

## RefreshFilter

Reapply filter after criteria changes:

```csharp
treeView.FilterMode = TreeViewFilterMode.Equals;
treeView.RefreshFilter();
```

---

## FilteredItems

Get currently filtered items:

```csharp
var filtered = treeView.FilteredItems;
Console.WriteLine($"Found {filtered.Count} matching items");
```

---

## Filtering Events

### Filtering Event

```csharp
treeView.Filtering += (sender, args) =>
{
    // Validate or modify filter criteria
};
```

### Filtered Event

```csharp
treeView.Filtered += (sender, args) =>
{
    // Update UI after filtering
    StatusLabel.Text = $"{treeView.FilteredItems.Count} items found";
};
```

---

## Best Practices

1. **Set FilterPath** - Required for filtering to work
2. **Use AutoExpandOnFilter** - Helps users find matches
3. **Debounce filter input** - Avoid filtering on every keystroke
4. **Clear filter properly** - Set `FilterText = null` or empty

---

## Related Topics

- [Data Binding](data-binding.md) - Configure data models
- [Sorting](sorting.md) - Sort filtered results
