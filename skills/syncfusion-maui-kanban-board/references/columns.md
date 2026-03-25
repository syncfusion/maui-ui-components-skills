# Columns in .NET MAUI Kanban Board

## Table of Contents
- [Overview](#overview)
- [Auto-Generate vs Manual Columns](#auto-generate-vs-manual-columns)
- [Column Sizing](#column-sizing)
- [Categories and Mapping](#categories-and-mapping)
- [WIP Limits](#wip-limits)
- [Column Headers](#column-headers)
- [Multiple Categories Per Column](#multiple-categories-per-column)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

## Overview

Columns in a kanban board represent different stages of a workflow (e.g., To Do, In Progress, Done). The SfKanban control provides flexible column configuration through:

- **Auto-generation** - Automatically create columns from data
- **Manual definition** - Explicitly define columns with custom properties
- **Column sizing** - Control width constraints
- **WIP limits** - Set minimum/maximum card counts
- **Header customization** - Custom column header templates

## Auto-Generate vs Manual Columns

### Auto-Generated Columns

Columns are automatically created based on unique values in the data.

**XAML:**
```xml
<kanban:SfKanban ItemsSource="{Binding Cards}"
                 AutoGenerateColumns="True"
                 ColumnMappingPath="Category" />
```

**C#:**
```csharp
kanban.AutoGenerateColumns = true;
kanban.ColumnMappingPath = "Category";
kanban.ItemsSource = viewModel.Cards;
```

**Result:** If cards have categories "Open", "In Progress", "Done", three columns are created automatically.

**When to use:**
- Quick prototyping
- Dynamic workflows (categories not known in advance)
- Simple scenarios with no WIP limits or custom column titles

### Manual Column Definition

Explicitly define columns for full control.

**XAML:**
```xml
<kanban:SfKanban ItemsSource="{Binding Cards}"
                 AutoGenerateColumns="False">
    <kanban:SfKanban.Columns>
        <kanban:KanbanColumn Title="To Do" 
                             Categories="Open" />
        <kanban:KanbanColumn Title="In Progress" 
                             Categories="In Progress" />
        <kanban:KanbanColumn Title="Code Review" 
                             Categories="Code Review" />
        <kanban:KanbanColumn Title="Done" 
                             Categories="Done" />
    </kanban:SfKanban.Columns>
</kanban:SfKanban>
```

**C#:**
```csharp
kanban.AutoGenerateColumns = false;

kanban.Columns.Add(new KanbanColumn
{
    Title = "To Do",
    Categories = new List<object> { "Open" }
});

kanban.Columns.Add(new KanbanColumn
{
    Title = "In Progress",
    Categories = new List<object> { "In Progress" }
});

kanban.Columns.Add(new KanbanColumn
{
    Title ="Code Review",
    Categories = new List<object> { "Code Review" }
});

kanban.Columns.Add(new KanbanColumn
{
    Title = "Done",
    Categories = new List<object> { "Done" }
});
```

**When to use:**
- Production applications
- Custom column titles
- WIP limits required
- Need to control column order
- Want to group multiple categories in one column

## Column Sizing

Control column widths through three properties:

### Fixed Width

Set exact width for all columns:

```xml
<kanban:SfKanban ColumnWidth="300" />
```

```csharp
kanban.ColumnWidth = 300;
```

**Use when:** All columns should have the same fixed width.

### Minimum and Maximum Width

Set range constraints:

```xml
<kanban:SfKanban MinimumColumnWidth="250" 
                 MaximumColumnWidth="400" />
```

```csharp
kanban.MinimumColumnWidth = 250;
kanban.MaximumColumnWidth = 400;
```

**Use when:** Columns should be flexible but within bounds (responsive design).

### Default Behavior

If no sizing properties are set, columns are automatically sized based on content and available space.

### Sizing Priority

```
ColumnWidth (fixed) > MinimumColumnWidth/MaximumColumnWidth > Auto-sizing
```

## Categories and Mapping

### ColumnMappingPath

Specifies which property in your data determines column placement.

```csharp
// With default KanbanModel
kanban.ColumnMappingPath = "Category";  // Default, can be omitted

// With custom model
public class TaskModel
{
    public string Status { get; set; }  // Your custom property
}

kanban.ColumnMappingPath = "Status";  // Must specify
```

### Categories Property

Maps data values to columns.

**Single Category:**
```xml
<kanban:KanbanColumn Title="In Progress" 
                     Categories="In Progress" />
```

**Multiple Categories (see next section):**
```xml
<kanban:KanbanColumn Title="Backlog" 
                     Categories="Open,Postponed,New" />
```

### How Cards are Routed

1. Card has `Category` (or mapped property) = "In Progress"
2. SfKanban looks for column with `Categories` containing "In Progress"
3. Card is placed in that column

**Important:** Category values are **case-sensitive**. "Open" ≠ "open"

## WIP Limits

Work-In-Progress (WIP) limits control the number of cards allowed in a column.

### Setting Limits

```xml
<kanban:KanbanColumn Title="In Progress"
                     Categories="In Progress"
                     MinimumLimit="2"
                     MaximumLimit="5" />
```

```csharp
var column = new KanbanColumn
{
    Title = "In Progress",
    Categories = new List<object> { "In Progress" },
    MinimumLimit = 2,
    MaximumLimit = 5
};
```

### How WIP Limits Work

**MinimumLimit:**
- Visual indicator if cards fall below this number
- Typically used to ensure minimum workflow throughput
- No enforcement (cards can still be moved out)

**MaximumLimit:**
- Visual indicator if cards exceed this number
- Helps identify bottlenecks
- No enforcement by default (cards can still be added)
- Enforcement requires workflow rules (see workflows.md)

### Visual Indicators

When WIP limits are violated:
- Column header typically shows count in warning color
- Example: "In Progress (6/5)" - exceeded maximum

### Common WIP Limit Patterns

```csharp
// Agile sprint board
new KanbanColumn
{
    Title = "In Progress",
    MinimumLimit = 1,      // Always have work
    MaximumLimit = 3       // Limit multitasking
}

// Support ticket system
new KanbanColumn
{
    Title = "Waiting for Customer",
    MaximumLimit = 10      // Alert if too many waiting
}

// Manufacturing
new KanbanColumn
{
    Title = "Assembly Line 1",
    MinimumLimit = 5,      // Keep line running
    MaximumLimit = 20      // Capacity limit
}
```

## Column Headers

### Default Header

Shows:
- Column title
- Card count
- WIP limit status (if set)

### Custom Header Template

```xml
<kanban:SfKanban.HeaderTemplate>
    <DataTemplate>
        <Border Background="#F5F5F5" 
                Padding="10"
                Margin="5,0">
            <HorizontalStackLayout Spacing="10">
                <Label Text="{Binding Title}"
                       FontAttributes="Bold"
                       FontSize="16"
                       VerticalOptions="Center" />
                <Border Background="#2196F3"
                        Padding="5,2"
                        StrokeThickness="0">
                    <Border.StrokeShape>
                        <RoundRectangle CornerRadius="10" />
                    </Border.StrokeShape>
                    <Label Text="{Binding ItemsCount}"
                           TextColor="White"
                           FontSize="12" />
                </Border>
            </HorizontalStackLayout>
        </Border>
    </DataTemplate>
</kanban:SfKanban.HeaderTemplate>
```

**BindingContext Properties:**
- `Title` - Column title
- `ItemsCount` - Number of cards in column
- `MinimumLimit` - Min WIP limit
- `MaximumLimit` - Max WIP limit

## Multiple Categories Per Column

Group related statuses into a single column.

### Example: Grouping "Done" States

```xml
<kanban:KanbanColumn Title="Completed"
                     Categories="Done,Closed,Resolved,Won't Fix" />
```

```csharp
new KanbanColumn
{
    Title = "Completed",
    Categories = new List<object> 
    { 
        "Done", 
        "Closed", 
        "Resolved", 
        "Won't Fix" 
    }
}
```

**Result:** Cards with any of these categories appear in the "Completed" column.

### Use Cases

**Backlog Column:**
```csharp
Categories = new List<object> { "New", "Open", "Postponed", "Backlog" }
```

**In Progress Column:**
```csharp
Categories = new List<object> { "In Progress", "Development", "Working" }
```

**Review Column:**
```csharp
Categories = new List<object> { "Code Review", "Testing", "QA", "Validation" }
```

## Common Patterns

### Pattern 1: Responsive Column Sizing

```xml
<kanban:SfKanban MinimumColumnWidth="200"
                 MaximumColumnWidth="400" />
```

Columns resize based on screen width but stay within bounds.

### Pattern 2: Fixed-Width for Consistency

```xml
<kanban:SfKanban ColumnWidth="320" />
```

All columns exactly 320 pixels wide - good for specific layouts.

### Pattern 3: WIP Limits for Agile

```csharp
kanban.Columns.Add(new KanbanColumn
{
    Title = "In Progress",
    Categories = new List<object> { "In Progress" },
    MinimumLimit = 1,
    MaximumLimit = 3  // Limit WIP per team
});
```

### Pattern 4: Swim Lane Style (Multiple Categories)

```csharp
// Group related items
new KanbanColumn
{
    Title = "To Do",
    Categories = new List<object> { "New", "Open", "Reopened" }
}
```

### Pattern 5: Dynamic Column Creation

```csharp
// From configuration or API
var columnConfigs = await GetColumnConfigsFromAPI();
foreach (var config in columnConfigs)
{
    kanban.Columns.Add(new KanbanColumn
    {
        Title = config.Title,
        Categories = config.Categories.ToList<object>(),
        MaximumLimit = config.WipLimit
    });
}
```

## Troubleshooting

### Issue: Cards not appearing in columns

**Check:**
1. `AutoGenerateColumns` is set correctly
2. Card's `Category` value matches column's `Categories`
3. `ColumnMappingPath` is set (for custom models)

**Debug:**
```csharp
foreach (var card in Cards)
{
    Debug.WriteLine($"Card: {card.Title}, Category: {card.Category}");
}

foreach (var column in kanban.Columns)
{
    Debug.WriteLine($"Column: {column.Title}, Categories: {string.Join(",", column.Categories)}");
}
```

### Issue: Columns in wrong order

**Solution:** When using manual columns, order them in the XAML/code as you want them displayed (left to right).

### Issue: WIP limits not working

**Clarification:** WIP limits are **visual indicators** only. They don't prevent card movement by default. To enforce limits, use workflows (see workflows.md).

### Issue: Column too narrow/wide

**Solution:**
```csharp
// Set constraints
kanban.MinimumColumnWidth = 250;
kanban.MaximumColumnWidth = 500;

// Or fixed width
kanban.ColumnWidth = 350;
```

### Issue: Category mismatch

**Problem:** Card category = "in progress", Column categories = "In Progress"

**Solution:** Ensure exact match (case-sensitive):
```csharp
card.Category = "In Progress";  // Must match exactly
```

## Next Steps

- **Implement workflows:** See [workflows.md](workflows.md) for transition rules
- **Customize cards:** See [cards.md](cards.md)
- **Handle events:** See [events.md](events.md)