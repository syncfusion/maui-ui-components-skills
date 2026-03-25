# Workflows in .NET MAUI Kanban Board

## Table of Contents
- [Overview](#overview)
- [KanbanWorkflow Class](#kanbanworkflow-class)
- [Defining Workflows](#defining-workflows)
- [Common Workflow Patterns](#common-workflow-patterns)
- [Preventing Transitions](#preventing-transitions)
- [Dynamic Workflows](#dynamic-workflows)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)
- [Next Steps](#next-steps)

## Overview

Workflows define rules for card transitions between columns, enabling you to:
- Restrict which columns a card can move to
- Enforce workflow processes (e.g., must go through code review before done)
- Prevent invalid transitions (e.g., can't skip testing)
- Create structured, compliant workflows

**When to use workflows:**
- Enforcing business processes
- Compliance requirements
- Quality gates (must pass review)
- Preventing workflow violations

## KanbanWorkflow Class

The `KanbanWorkflow` class defines transition rules.

### Properties

| Property | Type | Description |
|----------|------|-------------|
| `Category` | object | Source category/state (where card is moving FROM) |
| `AllowedTransitions` | List&lt;object&gt; | Target categories allowed (where card CAN move TO) |

### Basic Example

```csharp
kanban.Workflows = new List<KanbanWorkflow>
{
    new KanbanWorkflow
    {
        Category = "Open",
        AllowedTransitions = new List<object> { "In Progress" }
    },
    new KanbanWorkflow
    {
        Category = "In Progress",
        AllowedTransitions = new List<object> { "Code Review", "Open" }
    },
    new KanbanWorkflow
    {
        Category = "Code Review",
        AllowedTransitions = new List<object> { "Done", "In Progress" }
    }
};
```

**What this means:**
- From "Open" → can only move to "In Progress"
- From "In Progress" → can move to "Code Review" or back to "Open"
- From "Code Review" → can move to "Done" or back to "In Progress"

## Complete Workflow Examples

### Example 1: Simple Linear Workflow

```csharp
// To Do → In Progress → Done (no backtracking)
kanban.Workflows = new List<KanbanWorkflow>
{
    new KanbanWorkflow("Open", new List<object> { "In Progress" }),
    new KanbanWorkflow("In Progress", new List<object> { "Done" })
};
```

**Diagram:**
```
Open → In Progress → Done
```

### Example 2: Agile Sprint Workflow

```csharp
kanban.Workflows = new List<KanbanWorkflow>
{
    new KanbanWorkflow("Backlog", new List<object> { "To Do" }),
    new KanbanWorkflow("To Do", new List<object> { "In Progress", "Backlog" }),
    new KanbanWorkflow("In Progress", new List<object> { "Testing", "To Do" }),
    new KanbanWorkflow("Testing", new List<object> { "Done", "In Progress" }),
    new KanbanWorkflow("Done", new List<object>()) // No transitions from Done
};
```

**Diagram:**
```
Backlog → To Do ⇄ In Progress ⇄ Testing → Done
```

### Example 3: Code Review Workflow

```csharp
kanban.Workflows = new List<KanbanWorkflow>
{
    new KanbanWorkflow("Open", new List<object> { "In Progress", "Postponed" }),
    new KanbanWorkflow("In Progress", new List<object> { "Code Review", "Open", "Postponed" }),
    new KanbanWorkflow("Code Review", new List<object> { "Testing", "In Progress" }),
    new KanbanWorkflow("Testing", new List<object> { "Done", "Code Review", "In Progress" }),
    new KanbanWorkflow("Postponed", new List<object> { "Open" })
};
```

### Example 4: Support Ticket Workflow

```csharp
kanban.Workflows = new List<KanbanWorkflow>
{
    new KanbanWorkflow("New", new List<object> { "Assigned", "Closed" }),
    new KanbanWorkflow("Assigned", new List<object> { "In Progress", "Waiting for Customer" }),
    new KanbanWorkflow("In Progress", new List<object> { "Resolved", "Waiting for Customer", "Assigned" }),
    new KanbanWorkflow("Waiting for Customer", new List<object> { "In Progress", "Closed" }),
    new KanbanWorkflow("Resolved", new List<object> { "Closed", "In Progress" })
};
```

## XAML vs C# Configuration

**C# (Recommended for workflows):**

```csharp
this.kanban.Workflows = new List<KanbanWorkflow>
{
    new KanbanWorkflow("Open", new List<object> { "In Progress", "Closed" }),
    new KanbanWorkflow("In Progress", new List<object> { "Code Review" })
};
```

**XAML:**

```xml
<kanban:SfKanban x:Name="kanban">
    <kanban:SfKanban.Workflows>
        <kanban:KanbanWorkflow Category="Open">
            <kanban:KanbanWorkflow.AllowedTransitions>
                <x:String>In Progress</x:String>
                <x:String>Closed</x:String>
            </kanban:KanbanWorkflow.AllowedTransitions>
        </kanban:KanbanWorkflow>
        <kanban:KanbanWorkflow Category="In Progress">
            <kanban:KanbanWorkflow.AllowedTransitions>
                <x:String>Code Review</x:String>
            </kanban:KanbanWorkflow.AllowedTransitions>
        </kanban:KanbanWorkflow>
    </kanban:SfKanban.Workflows>
</kanban:SfKanban>
```

## Preventing All Transitions

To lock a column (no cards can be moved out):

```csharp
new KanbanWorkflow("Done", new List<object>())  // Empty list = no transitions
```

Or to allow dropping but prevent dragging from:

```csharp
new KanbanWorkflow("Archived", new List<object>())
```

## Common Workflow Patterns

### Pattern 1: One-Way Flow (Manufacturing)

```csharp
// Cards can only move forward, never backward
kanban.Workflows = new List<KanbanWorkflow>
{
    new KanbanWorkflow("Raw Materials", new List<object> { "Assembly" }),
    new KanbanWorkflow("Assembly", new List<object> { "Quality Check" }),
    new KanbanWorkflow("Quality Check", new List<object> { "Packaging" }),
    new KanbanWorkflow("Packaging", new List<object> { "Shipped" })
};
```

### Pattern 2: Quality Gates

```csharp
// Must pass each gate, but can fail back
kanban.Workflows = new List<KanbanWorkflow>
{
    new KanbanWorkflow("Development", new List<object> { "Unit Testing" }),
    new KanbanWorkflow("Unit Testing", new List<object> { "Integration Testing", "Development" }),
    new KanbanWorkflow("Integration Testing", new List<object> { "UAT", "Development" }),
    new KanbanWorkflow("UAT", new List<object> { "Production", "Development" })
};
```

### Pattern 3: Parallel Paths

```csharp
// Can take different paths
kanban.Workflows = new List<KanbanWorkflow>
{
    new KanbanWorkflow("New", new List<object> { "Bug Fix", "Feature Development" }),
    new KanbanWorkflow("Bug Fix", new List<object> { "Testing" }),
    new KanbanWorkflow("Feature Development", new List<object> { "Testing" }),
    new KanbanWorkflow("Testing", new List<object> { "Done" })
};
```

### Pattern 4: Approval Process

```csharp
// Requires approvals at stages
kanban.Workflows = new List<KanbanWorkflow>
{
    new KanbanWorkflow("Draft", new List<object> { "Manager Approval" }),
    new KanbanWorkflow("Manager Approval", new List<object> { "Director Approval", "Draft" }),
    new KanbanWorkflow("Director Approval", new List<object> { "Approved", "Draft" }),
    new KanbanWorkflow("Approved", new List<object>()) // Final, no changes
};
```

## Workflow Validation

Cards will be prevented from moving if the transition is not allowed.

**User Experience:**
- User drags card from "Open" to "Done"
- Workflow rules say "Open" can only go to "In Progress"
- Drag operation is cancelled
- Card returns to original position

## Dynamic Workflow Loading

```csharp
// Load workflows from configuration
public async Task LoadWorkflowsFromConfig()
{
    var workflowConfig = await _configService.GetWorkflowsAsync();
    var workflows = new List<KanbanWorkflow>();
    
    foreach (var config in workflowConfig)
    {
        workflows.Add(new KanbanWorkflow
        {
            Category = config.SourceState,
            AllowedTransitions = config.AllowedTargets.ToList<object>()
        });
    }
    
    kanban.Workflows = workflows;
}
```

## Combining with Events

Validate transitions with custom logic:

```csharp
kanban.DragEnd += (sender, e) =>
{
    var card = e.Data as KanbanModel;
    
    // Additional custom validation beyond workflows
    if (card.Category == "Done" && !IsTaskCompleted(card.ID))
    {
        e.Cancel = true;
        DisplayAlert("Cannot Complete", "Task validation failed", "OK");
    }
};
```

## Troubleshooting

### Issue: Cards still moving despite workflow rules

**Check:**
1. Workflows are assigned to kanban: `kanban.Workflows = ...`
2. Category names match exactly (case-sensitive)
3. Workflows are set BEFORE ItemsSource

### Issue: Can't move card anywhere

**Possible causes:**
1. No workflow defined for source category
2. AllowedTransitions list is empty
3. Target category not in AllowedTransitions

**Debug:**
```csharp
foreach (var workflow in kanban.Workflows)
{
    Debug.WriteLine($"From: {workflow.Category}, To: {string.Join(", ", workflow.AllowedTransitions)}");
}
```

### Issue: Workflow not enforcing

**Solution:** Ensure workflow Category matches card's current Category exactly:
```csharp
// Card category
card.Category = "In Progress"

// Workflow
new KanbanWorkflow("In Progress", ...) // Must match exactly
```

## Next Steps

- **Handle events:** See [events.md](events.md) for custom validation
- **Configure columns:** See [columns.md](columns.md)
- **WIP limits:** See [columns.md](columns.md#wip-limits)