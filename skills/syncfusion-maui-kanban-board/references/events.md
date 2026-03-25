# Events in .NET MAUI Kanban Board

## Table of Contents
- [Overview](#overview)
- [CardTapped Event](#cardtapped-event)
- [DragStart Event](#dragstart-event)
- [DragOver Event](#dragover-event)
- [DragEnd Event](#dragend-event)
- [Event Handling Best Practices](#event-handling-best-practices)
- [Common Scenarios](#common-scenarios)
- [Troubleshooting](#troubleshooting)
- [Next Steps](#next-steps)

## Overview

The SfKanban control provides events for handling user interactions:

- **CardTapped** - When a card is tapped/clicked
- **DragStart** - When card drag begins
- **DragOver** - During card drag (over columns)
- **DragEnd** - When card drag completes

**Common uses:**
- Validate card movements
- Show card details on tap
- Update backend on drag completion
- Cancel invalid operations
- Track user actions

## CardTapped Event

Fired when a user taps/clicks on a card.

### Event Args Properties

| Property | Type | Description |
|----------|------|-------------|
| `Data` | object | The data object of the tapped card |
| `Column` | KanbanColumn | The column containing the card |
| `Index` | int | Index position of card within its column |

### XAML:

```xml
<kanban:SfKanban x:Name="kanban"
                 ItemsSource="{Binding Cards}"
                 CardTapped="OnKanbanCardTapped">
</kanban:SfKanban>
```

### C#:

```csharp
kanban.CardTapped += OnKanbanCardTapped;

private void OnKanbanCardTapped(object sender, KanbanCardTappedEventArgs e)
{
    var card = e.Data as KanbanModel;
    var column = e.Column;
    var index = e.Index;
    
    // Show card details
    DisplayAlert("Card Tapped", 
        $"Title: {card.Title}\nColumn: {column.Title}\nIndex: {index}", 
        "OK");
}
```

### Common Patterns

**Pattern 1: Navigate to Detail Page**

```csharp
private async void OnKanbanCardTapped(object sender, KanbanCardTappedEventArgs e)
{
    var card = e.Data as KanbanModel;
    await Navigation.PushAsync(new CardDetailPage(card));
}
```

**Pattern 2: Show Edit Dialog**

```csharp
private async void OnKanbanCardTapped(object sender, KanbanCardTappedEventArgs e)
{
    var card = e.Data as KanbanModel;
    string newDescription = await DisplayPromptAsync(
        "Edit Card", 
        "Enter new description:",
        initialValue: card.Description);
    
    if (!string.IsNullOrEmpty(newDescription))
    {
        card.Description = newDescription;
        // Update in collection to refresh UI
        var index = Cards.IndexOf(card);
        Cards[index] = card;
    }
}
```

**Pattern 3: Toggle Card State**

```csharp
private void OnKanbanCardTapped(object sender, KanbanCardTappedEventArgs e)
{
    var card = e.Data as KanbanModel;
    
    // Toggle priority indicator
    card.IndicatorFill = card.IndicatorFill == Colors.Red 
        ? Colors.Green 
        : Colors.Red;
    
    // Refresh
    var index = Cards.IndexOf(card);
    Cards[index] = card;
}
```

## DragStart Event

Fired when a card drag operation begins.

### Event Args Properties

| Property | Type | Description |
|----------|------|-------------|
| `Data` | object | The card being dragged |
| `SourceColumn` | KanbanColumn | Column where drag started |
| `SourceIndex` | int | Original index in source column |
| `Cancel` | bool | Set to `true` to cancel drag |

### Example: Preventing Drag

```csharp
kanban.DragStart += (sender, e) =>
{
    var card = e.Data as KanbanModel;
    
    // Prevent dragging completed cards
    if (card.Category == "Done")
    {
        e.Cancel = true;
        DisplayAlert("Cannot Move", "Completed cards cannot be moved", "OK");
    }
};
```

### Example: Visual Feedback

```csharp
kanban.DragStart += (sender, e) =>
{
    var card = e.Data as KanbanModel;
    System.Diagnostics.Debug.WriteLine($"Started dragging: {card.Title}");
    
    // Could trigger UI changes, animations, etc.
};
```

## DragOver Event

Fired continuously as a card is dragged over columns.

### Event Args Properties

| Property | Type | Description |
|----------|------|-------------|
| `Data` | object | The card being dragged |
| `TargetColumn` | KanbanColumn | Column currently under the drag |
| `TargetIndex` | int | Potential drop index |
| `Cancel` | bool | Set to `true` to prevent drop in this column |

### Example: Validate Drop Target

```csharp
kanban.DragOver += (sender, e) =>
{
    var card = e.Data as KanbanModel;
    var targetColumn = e.TargetColumn;
    
    // Don't allow dropping in "Archived" column
    if (targetColumn.Title == "Archived")
    {
        e.Cancel = true;
    }
    
    // Or check WIP limits
    if (targetColumn.ItemsCount >= targetColumn.MaximumLimit)
    {
        e.Cancel = true;
    }
};
```

## DragEnd Event

Fired when a card drag operation completes (card dropped).

### Event Args Properties

| Property | Type | Description |
|----------|------|-------------|
| `Data` | object | The card that was dragged |
| `SourceColumn` | KanbanColumn | Original column |
| `SourceIndex` | int | Original index |
| `TargetColumn` | KanbanColumn | Destination column |
| `TargetIndex` | int | New index |
| `Cancel` | bool | Set to `true` to cancel the move |

### XAML:

```xml
<kanban:SfKanban x:Name="kanban"
                 ItemsSource="{Binding Cards}"
                 DragEnd="OnKanbanDragEnd">
</kanban:SfKanban>
```

### C#:

```csharp
kanban.DragEnd += OnKanbanDragEnd;

private void OnKanbanDragEnd(object sender, KanbanDragEndEventArgs e)
{
    var card = e.Data as KanbanModel;
    var sourceColumn = e.SourceColumn;
    var targetColumn = e.TargetColumn;
    
    // Log the move
    System.Diagnostics.Debug.WriteLine(
        $"Moved '{card.Title}' from {sourceColumn.Title} to {targetColumn.Title}");
    
    // Update card category to match new column
    card.Category = targetColumn.Categories.FirstOrDefault();
}
```

### Common Patterns

**Pattern 1: Update Backend API**

```csharp
private async void OnKanbanDragEnd(object sender, KanbanDragEndEventArgs e)
{
    var card = e.Data as KanbanModel;
    var newStatus = e.TargetColumn.Categories.FirstOrDefault()?.ToString();
    
    try
    {
        await _apiService.UpdateCardStatusAsync(card.ID, newStatus);
    }
    catch (Exception ex)
    {
        e.Cancel = true;  // Revert the move
        await DisplayAlert("Error", "Failed to update card status", "OK");
    }
}
```

**Pattern 2: Business Rule Validation**

```csharp
private void OnKanbanDragEnd(object sender, KanbanDragEndEventArgs e)
{
    var card = e.Data as KanbanModel;
    
    // Can't move to "Done" if description is empty
    if (e.TargetColumn.Title == "Done" && string.IsNullOrEmpty(card.Description))
    {
        e.Cancel = true;
        DisplayAlert("Validation Failed", 
            "Description required before marking done", 
            "OK");
    }
}
```

**Pattern 3: Timestamp Tracking**

```csharp
private void OnKanbanDragEnd(object sender, KanbanDragEndEventArgs e)
{
    var card = e.Data as TaskModel; // Custom model
    
    // Track when card entered each stage
    card.StatusChangedAt = DateTime.Now;
    card.PreviousStatus = e.SourceColumn.Title;
    card.CurrentStatus = e.TargetColumn.Title;
    
    // Save to history
    _historyService.LogCardMove(card);
}
```

**Pattern 4: Automatic Assignment**

```csharp
private void OnKanbanDragEnd(object sender, KanbanDragEndEventArgs e)
{
    var card = e.Data as KanbanModel;
    
    // Auto-assign when moved to "In Progress"
    if (e.TargetColumn.Title == "In Progress" && card.Tags?.Contains("Unassigned") == true)
    {
        card.Tags.Remove("Unassigned");
        card.Tags.Add($"Assigned: {_currentUser.Name}");
        
        // Refresh card
        var index = Cards.IndexOf(card);
        Cards[index] = card;
    }
}
```

**Pattern 5: Notification Triggers**

```csharp
private async void OnKanbanDragEnd(object sender, KanbanDragEndEventArgs e)
{
    var card = e.Data as KanbanModel;
    
    // Notify stakeholders when card reaches certain stages
    if (e.TargetColumn.Title == "Ready for Review")
    {
        await _notificationService.NotifyAsync(
            "Code Review Needed",
            $"Card '{card.Title}' is ready for review",
            reviewers: new[] { "reviewer1@company.com", "reviewer2@company.com" }
        );
    }
}
```

## Event Execution Order

```
1. DragStart (can cancel)
2. DragOver (multiple times during drag, can cancel)
3. DragEnd (can cancel to revert)
```

## Cancelling Operations

Set `e.Cancel = true` in any drag event to prevent the action:

```csharp
kanban.DragEnd += (sender, e) =>
{
    // Complex validation
    if (!ValidateCardMove(e.Data, e.TargetColumn))
    {
        e.Cancel = true;
        DisplayAlert("Invalid Move", "This action is not allowed", "OK");
    }
};

private bool ValidateCardMove(object cardData, KanbanColumn targetColumn)
{
    var card = cardData as KanbanModel;
    
    // Your custom validation logic
    if (card.Tags?.Contains("Blocked") == true)
        return false;
    
    if (targetColumn.ItemsCount >= targetColumn.MaximumLimit)
        return false;
    
    return true;
}
```

## Async Event Handlers

```csharp
private async void OnKanbanDragEnd(object sender, KanbanDragEndEventArgs e)
{
    var card = e.Data as KanbanModel;
    
    // Show loading indicator
    IsBusy = true;
    
    try
    {
        // Async operation
        await Task.Delay(1000); // Simulating API call
        await _apiService.UpdateCardAsync(card);
    }
    catch (Exception ex)
    {
        e.Cancel = true;
        await DisplayAlert("Error", ex.Message, "OK");
    }
    finally
    {
        IsBusy = false;
    }
}
```

## Troubleshooting

### Issue: Event not firing

**Check:**
1. Event is subscribed: `kanban.CardTapped += ...`
2. ItemsSource has data
3. Cards are visible and tappable

### Issue: Cancel not working

**Ensure:**
1. `e.Cancel = true` is set BEFORE the event handler returns
2. Not setting Cancel in CardTapped (not cancellable)

### Issue: Card data is null

**Solution:** Cast with null check:
```csharp
if (e.Data is KanbanModel card)
{
    // Safe to use card
}
```

### Issue: UI not updating after changes

**Solution:** Replace item in ObservableCollection:
```csharp
var index = Cards.IndexOf(card);
if (index >= 0)
{
    Cards[index] = card;  // Triggers UI update
}
```

## Next Steps

- **Implement workflows:** See [workflows.md](workflows.md)
- **Customize cards:** See [cards.md](cards.md)
- **Configure columns:** See [columns.md](columns.md)