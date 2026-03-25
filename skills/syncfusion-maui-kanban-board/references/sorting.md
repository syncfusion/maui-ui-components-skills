# Sorting in .NET MAUI Kanban Board

## Table of Contents
- [Overview](#overview)
- [SortingMappingPath Property](#sortingmappingpath-property)
- [SortingOrder Property](#sortingorder-property)
- [Custom Field Sorting](#custom-field-sorting)
- [Index-Based Sorting](#index-based-sorting)
- [Dynamic Sorting](#dynamic-sorting)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)
- [Next Steps](#next-steps)

## Overview

The SfKanban control supports sorting cards within columns based on data properties. Sorting helps organize cards by priority, due date, index, or any custom field.

**Sorting features:**
- Sort by any property in your data model
- Ascending or descending order
- Custom field sorting (priority, date, etc.)
- Index-based sorting for manual ordering
- Dynamic sorting updates on drag-and-drop

## Configuration Properties

| Property | Type | Description |
|----------|------|-------------|
| `SortingMappingPath` | string | Property name to sort by |
| `SortingOrder` | KanbanSortingOrder | Ascending or Descending |

## Basic Sorting Setup

### Example: Sort by Priority

**XAML:**
```xml
<kanban:SfKanban ItemsSource="{Binding Cards}"
                 SortingMappingPath="Priority"
                 SortingOrder="Ascending" />
```

**C#:**
```csharp
kanban.SortingMappingPath = "Priority";
kanban.SortingOrder = KanbanSortingOrder.Ascending;
```

**Data Model:**
```csharp
public class TaskModel
{
    public string Title { get; set; }
    public int Priority { get; set; }  // Lower number = higher priority
    public string Category { get; set; }
}
```

**Result:** Cards with Priority=1 appear first, then Priority=2, etc.

## Sorting Order

### Ascending
```csharp
kanban.SortingOrder = KanbanSortingOrder.Ascending;
```
- Lower values appear first
- A-Z for strings
- Earlier dates first
- 1, 2, 3... for numbers

### Descending
```csharp
kanban.SortingOrder = KanbanSortingOrder.Descending;
```
- Higher values appear first
- Z-A for strings
- Later dates first
- ...3, 2, 1 for numbers

## Common Sorting Scenarios

### Scenario 1: Sort by Priority

```csharp
// Model
public class TaskModel
{
    public string PriorityLevel { get; set; }  // "High", "Medium", "Low"
}

// Setup
kanban.SortingMappingPath = "PriorityLevel";
kanban.SortingOrder = KanbanSortingOrder.Ascending;
```

**Result:** "High" appears before "Low" (alphabetically)

**Better Approach - Use Numeric Priority:**
```csharp
public class TaskModel
{
    public int PriorityValue { get; set; }  // 1=High, 2=Medium, 3=Low
}

kanban.SortingMappingPath = "PriorityValue";
kanban.SortingOrder = KanbanSortingOrder.Ascending;
```

### Scenario 2: Sort by Due Date

```csharp
public class TaskModel
{
    public DateTime DueDate { get; set; }
}

kanban.SortingMappingPath = "DueDate";
kanban.SortingOrder = KanbanSortingOrder.Ascending;  // Earliest first
```

### Scenario 3: Sort by Custom Index

```csharp
public class TaskModel
{
    public int Index { get; set; }  // Manual ordering
}

kanban.SortingMappingPath = "Index";
kanban.SortingOrder = KanbanSortingOrder.Ascending;
```

## Index-Based Sorting

Index-based sorting allows precise manual ordering of cards.

### Implementation

**Model:**
```csharp
public class CardDetails
{
    public string Title { get; set; }
    public int Index { get; set; }
    public string Category { get; set; }
}
```

**Setup:**
```xml
<kanban:SfKanban ItemsSource="{Binding Cards}"
                 SortingMappingPath="Index"
                 SortingOrder="Ascending"
                 DragEnd="OnKanbanDragEnd" />
```

**Update Index on Drag:**
```csharp
private void OnKanbanDragEnd(object sender, KanbanDragEndEventArgs e)
{
    UpdateCardIndices(e);
    kanban.RefreshKanbanColumn();
}

private void UpdateCardIndices(KanbanDragEndEventArgs e)
{
    var targetItems = e.TargetColumn.Items.Cast<CardDetails>().ToList();
    
    for (int i = 0; i < targetItems.Count; i++)
    {
        targetItems[i].Index = i + 1;
    }
}
```

## Refreshing After Drag

**Important:** Call `RefreshKanbanColumn()` after drag operations to re-apply sorting.

```csharp
kanban.DragEnd += (sender, e) =>
{
    // Update any data properties
    UpdateCardData(e.Data);
    
    // Refresh to re-sort
    kanban.RefreshKanbanColumn();
};
```

## Disabling Sorting

To disable sorting, don't set `SortingMappingPath`:

```csharp
// No sorting - cards appear in ItemsSource order
kanban.SortingMappingPath = string.Empty;  // or don't set it
```

## Custom Sorting Logic

For complex sorting, pre-sort your ItemsSource:

```csharp
public ObservableCollection<TaskModel> Cards { get; set; }

public void LoadTasks()
{
    var tasks = GetTasksFromDatabase();
    
    // Custom multi-level sort
    var sorted = tasks
        .OrderBy(t => t.Status == "Blocked" ? 0 : 1)  // Blocked first
        .ThenBy(t => t.Priority)
        .ThenBy(t => t.DueDate);
    
    Cards = new ObservableCollection<TaskModel>(sorted);
}
```

## Common Patterns

### Pattern 1: Priority-Based with Visual Indicator

```csharp
public class TaskModel
{
    public int Priority { get; set; }
    public Color IndicatorColor => Priority switch
    {
        1 => Colors.Red,
        2 => Colors.Orange,
        3 => Colors.Green,
        _ => Colors.Gray
    };
}

kanban.SortingMappingPath = "Priority";
kanban.SortingOrder = KanbanSortingOrder.Ascending;
```

### Pattern 2: Due Date with Overdue Highlighting

```csharp
public class TaskModel
{
    public DateTime DueDate { get; set; }
    public bool IsOverdue => DueDate < DateTime.Now;
}

kanban.SortingMappingPath = "DueDate";
kanban.SortingOrder = KanbanSortingOrder.Ascending;
```

### Pattern 3: Drag-and-Drop Index Update

```csharp
private void OnKanbanDragEnd(object sender, KanbanDragEndEventArgs e)
{
    if (e.SourceColumn == e.TargetColumn)
    {
        // Reorder within same column
        ReorderIndices(e.TargetColumn);
    }
    else
    {
        // Move to different column - update indices in both
        ReorderIndices(e.SourceColumn);
        ReorderIndices(e.TargetColumn);
    }
    
    kanban.RefreshKanbanColumn();
}

private void ReorderIndices(KanbanColumn column)
{
    var items = column.Items.Cast<CardDetails>().ToList();
    for (int i = 0; i < items.Count; i++)
    {
        items[i].Index = i + 1;
    }
}
```

## Troubleshooting

### Issue: Sorting not working

**Check:**
1. `SortingMappingPath` matches property name exactly (case-sensitive)
2. Property is sortable type (int, string, DateTime, etc.)
3. Property is public with getter
4. Property has values (not all null)

**Debug:**
```csharp
foreach (var card in Cards)
{
    var prop = card.GetType().GetProperty(kanban.SortingMappingPath);
    var value = prop?.GetValue(card);
    Debug.WriteLine($"Card: {card.Title}, Sort Value: {value}");
}
```

### Issue: Cards not reordering after drag

**Solution:** Call `RefreshKanbanColumn()`:
```csharp
kanban.DragEnd += (sender, e) =>
{
    kanban.RefreshKanbanColumn();  // Required!
};
```

### Issue: Sort order incorrect

**Check sorting direction:**
```csharp
// Ascending: 1, 2, 3 or A, B, C
kanban.SortingOrder = KanbanSortingOrder.Ascending;

// Descending: 3, 2, 1 or Z, Y, X
kanban.SortingOrder = KanbanSortingOrder.Descending;
```

## Next Steps

- **Configure columns:** See [columns.md](columns.md)
- **Handle events:** See [events.md](events.md)
- **Customize cards:** See [cards.md](cards.md)