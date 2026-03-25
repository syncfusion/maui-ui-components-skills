# Undo & Redo Operations

This guide covers undo and redo functionality in the .NET MAUI ImageEditor control, allowing users to revert and restore editing operations.

## Table of Contents
- [Undo/Redo Overview](#undoredo-overview)
- [Undo Method](#undo-method)
- [Redo Method](#redo-method)
- [Checking Undo/Redo Availability](#checking-undoredo-availability)
- [History Management](#history-management)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)
- [Next Steps](#next-steps)

## Undo/Redo Overview

The ImageEditor provides built-in undo and redo functionality to revert and restore editing operations, enhancing the user experience by allowing mistake correction and experimentation.

### Supported Operations

Undo/Redo works with:
- **Shape annotations** (rectangles, circles, arrows, etc.)
- **Text annotations**
- **Pen/freehand drawing**
- **Custom view annotations**
- **Image transformations** (rotation, flipping)
- **Cropping operations**
- **Image effects** (brightness, contrast, filters)

### When to Use Undo/Redo

- **Mistake Recovery:** Allow users to quickly correct errors
- **Experimentation:** Encourage users to try different edits without fear
- **Workflow Enhancement:** Provide professional-level editing experience
- **User Confidence:** Reduce anxiety about making permanent changes

## Undo Method

The `Undo` method reverts the most recent editing operation.

### Basic Usage

```xml
<Grid RowDefinitions="0.9*, 0.1*">
    <imageEditor:SfImageEditor x:Name="imageEditor" Source="photo.jpg" />
    <Button Grid.Row="1" Text="Undo" Clicked="OnUndoClicked" />
</Grid>
```

```csharp
private void OnUndoClicked(object sender, EventArgs e)
{
    imageEditor.Undo();
}
```

### What Undo Does

- Reverts the last completed action
- Moves to previous state in history stack
- Preserves redo capability (until new action performed)
- Can be called multiple times to undo sequential actions

### Example: Undo Multiple Times

```csharp
private void UndoLastThreeActions()
{
    // Undo last 3 operations
    for (int i = 0; i < 3; i++)
    {
        imageEditor.Undo();
    }
}
```

## Redo Method

The `Redo` method restores an operation that was previously undone.

### Basic Usage

```xml
<Grid RowDefinitions="0.9*, 0.1*">
    <imageEditor:SfImageEditor x:Name="imageEditor" Source="photo.jpg" />
    <Button Grid.Row="1" Text="Redo" Clicked="OnRedoClicked" />
</Grid>
```

```csharp
private void OnRedoClicked(object sender, EventArgs e)
{
    imageEditor.Redo();
}
```

### Important Redo Behavior

**Critical:** Performing any new action after using Undo will clear the Redo history. Once you've undone several actions and then make a new edit, you cannot redo the previously undone actions.

```csharp
// Scenario demonstrating redo behavior
imageEditor.AddShape(AnnotationShape.Circle);  // Action 1
imageEditor.AddText("Hello");                   // Action 2
imageEditor.AddShape(AnnotationShape.Arrow);   // Action 3

imageEditor.Undo();  // Removes arrow
imageEditor.Undo();  // Removes text

// At this point, you can redo to restore arrow and text
imageEditor.Redo();  // Restores text

// But if you perform a new action...
imageEditor.AddShape(AnnotationShape.Rectangle);  // New action

// Now the arrow cannot be redone - redo history is cleared
imageEditor.Redo();  // Won't restore arrow
```

## Checking Undo/Redo Availability

While not explicitly documented in the source, you would typically check history state before enabling/disabling undo/redo buttons.

### Typical Pattern (Conceptual)

```xml
<Grid RowDefinitions="0.9*, 0.1*">
    <imageEditor:SfImageEditor x:Name="imageEditor" Source="photo.jpg" />
    
    <HorizontalStackLayout Grid.Row="1" HorizontalOptions="Center">
        <Button x:Name="undoButton" 
                Text="Undo" 
                Clicked="OnUndoClicked" 
                Margin="5" />
        <Button x:Name="redoButton" 
                Text="Redo" 
                Clicked="OnRedoClicked" 
                Margin="5" />
    </HorizontalStackLayout>
</Grid>
```

```csharp
// Note: This is conceptual - actual implementation may vary
private void UpdateUndoRedoState()
{
    // You would typically track history state
}
```

## History Management

### History Stack Behavior

The ImageEditor maintains an internal history stack:

1. **New Action:** Added to stack
2. **Undo:** Move backward in stack
3. **Redo:** Move forward in stack
4. **New Action After Undo:** Clears forward history

### Typical Stack Example

```
Initial State: [Empty]

Add Circle → [Circle]
Add Text → [Circle, Text]
Add Arrow → [Circle, Text, Arrow]

Undo → [Circle, Text] (Arrow in redo history)
Undo → [Circle] (Text and Arrow in redo history)

Redo → [Circle, Text] (Arrow still in redo history)

Add Rectangle → [Circle, Text, Rectangle] (Arrow permanently lost)
```

### History Limits

While not explicitly documented, most editors have practical history limits to manage memory usage.

## Common Patterns

### Undo/Redo Toolbar

```xml
<Grid RowDefinitions="*, Auto, Auto">
    <imageEditor:SfImageEditor x:Name="imageEditor" Source="photo.jpg" />
    
    <!-- Edit tools -->
    <HorizontalStackLayout Grid.Row="1" 
                          HorizontalOptions="Center" 
                          Margin="10">
        <Button Text="Add Circle" Clicked="OnAddCircleClicked" Margin="5" />
        <Button Text="Add Text" Clicked="OnAddTextClicked" Margin="5" />
        <Button Text="Add Arrow" Clicked="OnAddArrowClicked" Margin="5" />
    </HorizontalStackLayout>
    
    <!-- Undo/Redo controls -->
    <HorizontalStackLayout Grid.Row="2" 
                          HorizontalOptions="Center" 
                          Margin="10">
        <Button x:Name="undoButton" 
                Text="↶ Undo" 
                Clicked="OnUndoClicked" 
                Margin="5" />
        <Button x:Name="redoButton" 
                Text="↷ Redo" 
                Clicked="OnRedoClicked" 
                Margin="5" />
    </HorizontalStackLayout>
</Grid>
```

```csharp
private void OnAddCircleClicked(object sender, EventArgs e)
{
    imageEditor.AddShape(AnnotationShape.Circle);
}

private void OnAddTextClicked(object sender, EventArgs e)
{
    imageEditor.AddText("Sample Text");
}

private void OnAddArrowClicked(object sender, EventArgs e)
{
    imageEditor.AddShape(AnnotationShape.Arrow);
}

private void OnUndoClicked(object sender, EventArgs e)
{
    imageEditor.Undo();
}

private void OnRedoClicked(object sender, EventArgs e)
{
    imageEditor.Redo();
}
```

### Keyboard Shortcuts Handler

```csharp
// In your page or view
private void OnKeyDown(object sender, KeyEventArgs e)
{
    // Ctrl+Z for Undo
    if (e.Key == Key.Z && e.Modifiers == KeyModifiers.Ctrl)
    {
        imageEditor.Undo();
        e.Handled = true;
    }
    // Ctrl+Y for Redo
    else if (e.Key == Key.Y && e.Modifiers == KeyModifiers.Ctrl)
    {
        imageEditor.Redo();
        e.Handled = true;
    }
    // Ctrl+Shift+Z for Redo (alternative)
    else if (e.Key == Key.Z && 
             e.Modifiers == (KeyModifiers.Ctrl | KeyModifiers.Shift))
    {
        imageEditor.Redo();
        e.Handled = true;
    }
}
```

### Safe Undo with State Check

```csharp
private void SafeUndo()
{
    try
    {
        imageEditor.Undo();
    }
    catch (Exception ex)
    {
        // Handle case where undo history is empty
        DisplayAlert("Cannot Undo", "No more actions to undo", "OK");
    }
}

private void SafeRedo()
{
    try
    {
        imageEditor.Redo();
    }
    catch (Exception ex)
    {
        // Handle case where redo history is empty
        DisplayAlert("Cannot Redo", "No more actions to redo", "OK");
    }
}
```

### Undo Batch Operations

```csharp
private void UndoGroup()
{
    // If you added 3 annotations as a group
    // Undo all 3
    int groupSize = 3;
    for (int i = 0; i < groupSize; i++)
    {
        imageEditor.Undo();
    }
}
```

### Confirmation Before Undo

```csharp
private async void UndoWithConfirmation()
{
    bool shouldUndo = await DisplayAlert(
        "Undo", 
        "Undo the last action?", 
        "Yes", 
        "No");
        
    if (shouldUndo)
    {
        imageEditor.Undo();
    }
}
```

### History Indicator

```xml
<Grid RowDefinitions="*, Auto, Auto">
    <imageEditor:SfImageEditor x:Name="imageEditor" Source="photo.jpg" />
    
    <!-- History indicator -->
    <Label x:Name="historyLabel" 
           Grid.Row="1" 
           Text="Actions: 0 | Can Undo: No | Can Redo: No"
           HorizontalOptions="Center"
           Margin="10" />
    
    <!-- Controls -->
    <HorizontalStackLayout Grid.Row="2" HorizontalOptions="Center">
        <Button Text="Undo" Clicked="OnUndoClicked" Margin="5" />
        <Button Text="Redo" Clicked="OnRedoClicked" Margin="5" />
    </HorizontalStackLayout>
</Grid>
```

```csharp
private int actionCount = 0;

private void OnAnnotationAdded()
{
    actionCount++;
    UpdateHistoryLabel();
}

private void OnUndoClicked(object sender, EventArgs e)
{
    imageEditor.Undo();
    actionCount--;
    UpdateHistoryLabel();
}

private void OnRedoClicked(object sender, EventArgs e)
{
    imageEditor.Redo();
    actionCount++;
    UpdateHistoryLabel();
}

private void UpdateHistoryLabel()
{
    historyLabel.Text = $"Actions: {actionCount}";
}
```

### Undo All

```csharp
private async void UndoAll()
{
    bool confirmed = await DisplayAlert(
        "Undo All", 
        "Undo all editing operations?", 
        "Yes", 
        "No");
        
    if (confirmed)
    {
        // Keep undoing until no more history
        // In practice, track operation count or use try-catch
        const int maxUndos = 100;  // Safety limit
        
        for (int i = 0; i < maxUndos; i++)
        {
            try
            {
                imageEditor.Undo();
            }
            catch
            {
                // No more undo history
                break;
            }
        }
    }
}
```

## Troubleshooting

### Issue: Undo Not Working

**Cause:** No actions have been performed yet or history is empty.

**Solution:** Ensure operations were completed before attempting undo:
```csharp
// Ensure operation is saved
imageEditor.AddShape(AnnotationShape.Circle);
imageEditor.SaveEdits();  // Make it part of history

// Now undo will work
imageEditor.Undo();
```

### Issue: Redo Not Working After New Action

**Cause:** Redo history cleared by new action (expected behavior).

**Solution:** This is by design. Once a new action is performed after undo, redo history is cleared:
```csharp
// This is normal behavior
imageEditor.AddShape(AnnotationShape.Circle);
imageEditor.Undo();
// Can redo at this point

imageEditor.AddShape(AnnotationShape.Rectangle);  // New action
// Cannot redo circle anymore
```

### Issue: Multiple Undos Not Working

**Cause:** Operations not individual or history exhausted.

**Solution:** Track operation count:
```csharp
private List<string> operationHistory = new();

private void TrackOperation(string operation)
{
    operationHistory.Add(operation);
}

private void PerformUndo()
{
    if (operationHistory.Count > 0)
    {
        imageEditor.Undo();
        operationHistory.RemoveAt(operationHistory.Count - 1);
    }
}
```

### Issue: Undo Affects Wrong Operation

**Cause:** Multiple operations performed, undo order is LIFO (Last In First Out).

**Solution:** Understand LIFO behavior:
```csharp
// Operations are undone in reverse order
imageEditor.AddShape(AnnotationShape.Circle);   // First
imageEditor.AddText("Hello");                    // Second
imageEditor.AddShape(AnnotationShape.Arrow);    // Third

imageEditor.Undo();  // Removes arrow (last operation)
imageEditor.Undo();  // Removes text (second-to-last)
imageEditor.Undo();  // Removes circle (first operation)
```

### Issue: Cannot Undo After Reset

**Cause:** Reset clears all history.

**Solution:** This is expected. Reset returns to original state:
```csharp
imageEditor.AddShape(AnnotationShape.Circle);
imageEditor.Reset();  // Clears all, including history
// Cannot undo here
```

## Next Steps

- **Saving Images:** [save-serialization.md](save-serialization.md)
- **Events:** [events.md](events.md)
- **Annotations:** [annotations.md](annotations.md)
- **Getting Started:** [getting-started.md](getting-started.md)