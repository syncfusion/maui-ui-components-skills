# Sorting in TreeView

Guide to sorting nodes in the TreeView control.

## Overview

TreeView provides built-in sorting through `SortDescriptors` collection with support for custom sorting logic.

---

## Programmatic Sorting

Add `SortDescriptor` to sort items:

**XAML:**

```xml
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

## Sort Direction

| Direction | Description |
|-----------|-------------|
| `Ascending` | A to Z, 0 to 9 |
| `Descending` | Z to A, 9 to 0 |

---

## Custom Sorting

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

```xml
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

## Clear Sorting

Restore original order:

```csharp
treeView.SortDescriptors.Clear();
```

---

## Multiple Sort Descriptors

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

## Best Practices

1. **Clear and reinitialize** when ItemsSource changes
2. **Use custom comparers** for complex sorting
3. **Combine with filtering** for powerful data views
4. **PropertyName is mandatory** for property-based sorting

---

## Sample Projects

- [Custom Sorting](https://github.com/SyncfusionExamples/custom-sorting-in-.net-maui-treeview)

---

## Related Topics

- [Filtering](filtering.md) - Filter before sorting
- [Data Binding](data-binding.md) - Configure data models
