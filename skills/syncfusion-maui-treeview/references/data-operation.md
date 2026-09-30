# Data Operation in TreeView

Comprehensive guide to sorting and filtering nodes in the TreeView control.

## Table of Contents
- [Sorting](#sorting)
- [Filtering](#filtering)
- [Best Practices](#best-practices)

## Sorting

TreeView provides built-in sorting through `SortDescriptors` collection with support for custom sorting logic.

---

### Programmatic Sorting

Add `SortDescriptor` to sort items:

**XAML:**

```xaml
<syncfusion:SfTreeView>
    <syncfusion:SfTreeView.SortDescriptors>
        <treeviewengine:SortDescriptor PropertyName="ItemName" 
                                      Direction="Ascending"/>
    </syncfusion:SfTreeView.SortDescriptors>
</syncfusion:SfTreeView>
```

**C#:**

```csharp
var sortDescriptor = new SortDescriptor()
{
    PropertyName = "ItemName",
    Direction = TreeViewSortDirection.Ascending
};
treeView.SortDescriptors.Add(sortDescriptor);
```

---

### Sort Direction

| Direction | Description |
|-----------|-------------|
| `Ascending` | A to Z, 0 to 9 |
| `Descending` | Z to A, 9 to 0 |

---

### Custom Sorting

Use `IComparer` for custom logic:

```csharp
public class CustomDateSortComparer : IComparer<object>
{
    public int Compare(object x, object y)
    {
        if (x is FileManager xFile && y is FileManager yFile)
        {
            // Latest dates first (descending)
            return -DateTime.Compare(xFile.Date, yFile.Date);
        }
        return 0;
    }
}
```

**XAML:**

```xaml
<ContentPage.Resources>
    <local:CustomDateSortComparer x:Key="CustomSortComparer"/>
</ContentPage.Resources>

<syncfusion:SfTreeView>
    <syncfusion:SfTreeView.SortDescriptors>
        <treeviewengine:SortDescriptor Comparer="{StaticResource CustomSortComparer}"/>
    </syncfusion:SfTreeView.SortDescriptors>
</syncfusion:SfTreeView>
```

---

### Clear Sorting

Restore original order:

```csharp
treeView.SortDescriptors.Clear();
```

---

### Multiple Sort Descriptors

```csharp
treeView.SortDescriptors.Add(new SortDescriptor 
{ 
    PropertyName = "Category", 
    Direction = TreeViewSortDirection.Ascending 
});
treeView.SortDescriptors.Add(new SortDescriptor 
{ 
    PropertyName = "Name", 
    Direction = TreeViewSortDirection.Ascending 
});
```

---

## Filtering

TreeView provides built-in text filtering and custom predicate filtering to quickly locate nodes in hierarchical data.

---

### FilterMode

Available filter modes:

| Mode | Description |
|------|-------------|
| `None` | No filtering (default) |
| `Contains` | Display text contains filter text |
| `StartsWith` | Display text starts with filter text |
| `Equals` | Display text exactly matches filter text |
| `Custom` | Use custom predicate function |

**XAML:**

```xaml
<syncfusion:SfTreeView FilterMode="Contains"/>
```

**C#:**

```csharp
treeView.FilterMode = TreeViewFilterMode.Contains;
```

---

### FilterText

Bindable property for filter text:

```xaml
<Entry Placeholder="Filter..." 
       Text="{Binding FilterText, Mode=TwoWay}"/>

<syncfusion:SfTreeView FilterText="{Binding FilterText}"
                       FilterMode="Contains"
                       FilterPath="ItemName"/>
```

---

### FilterPath and FilterPaths

#### Single Field Filtering

```xaml
<syncfusion:SfTreeView FilterText="{Binding FilterText}"
                       FilterPath="Name"
                       FilterMode="Contains"/>
```

#### Multi-Field Filtering

```xaml
<syncfusion:SfTreeView FilterMode="Contains">
    <syncfusion:SfTreeView.FilterPaths>
        <x:String>Name</x:String>
        <x:String>Code</x:String>
        <x:String>Description</x:String>
    </syncfusion:SfTreeView.FilterPaths>
</syncfusion:SfTreeView>
```

---

### Custom Predicate Filtering

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

### AutoExpandOnFilter

Automatically expand nodes containing matches:

```xaml
<syncfusion:SfTreeView AutoExpandOnFilter="True"
                       FilterMode="Contains"/>
```

---

### RefreshFilter

Reapply filter after criteria changes:

```csharp
treeView.FilterMode = TreeViewFilterMode.Equals;
treeView.RefreshFilter();
```

---

### FilteredItems

Get currently filtered items:

```csharp
var filtered = treeView.FilteredItems;
Console.WriteLine($"Found {filtered.Count} matching items");
```

---

### Filtering Events

#### Filtering Event

```csharp
treeView.Filtering += (sender, args) =>
{
    // Validate or modify filter criteria
};
```

#### Filtered Event

```csharp
treeView.Filtered += (sender, args) =>
{
    // Update UI after filtering
    StatusLabel.Text = $"{treeView.FilteredItems.Count} items found";
};
```

---

## Best Practices

1. **Clear and reinitialize** when ItemsSource changes
2. **Use custom comparers** for complex sorting
3. **Combine with filtering** for powerful data views
4. **PropertyName is mandatory** for property-based sorting
5. **Set FilterPath** - Required for filtering to work
6. **Use AutoExpandOnFilter** - Helps users find matches
7. **Debounce filter input** - Avoid filtering on every keystroke
8. **Clear filter properly** - Set `FilterText = null` or empty

---

## Sample Projects

- [Custom Sorting](https://github.com/SyncfusionExamples/custom-sorting-in-.net-maui-treeview)

---

## Related Topics

- [Data Binding](data-binding.md) - Configure data models
