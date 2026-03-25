# Interactive Features in .NET MAUI ListView

## Table of Contents
- [Overview](#overview)
- [Swiping](#swiping)
- [Drag and Drop](#drag-and-drop)
- [Best Practices](#best-practices)

## Overview

SfListView provides rich interactive features for enhanced user experience:
- **Swiping** - Reveal actions by swiping items left or right
- **Drag and Drop** - Reorder items or move between lists

## Swiping

Swipe to reveal action buttons or content on the left or right side of items.

### Enable Swiping

```xml
<syncfusion:SfListView AllowSwiping="True" SwipeOffset="100" />
```

```csharp
listView.AllowSwiping = true;
listView.SwipeOffset = 100; // Distance in pixels to trigger swipe
```

### Start Swipe Template

```xml
<syncfusion:SfListView AllowSwiping="True" SwipeOffset="80">
    <syncfusion:SfListView.StartSwipeTemplate>
        <DataTemplate>
            <Grid BackgroundColor="#4CAF50" Padding="20">
                <Label Text="Archive" 
                       TextColor="White"
                       FontSize="16"
                       VerticalOptions="Center" 
                       HorizontalOptions="Center" />
            </Grid>
        </DataTemplate>
    </syncfusion:SfListView.StartSwipeTemplate>
</syncfusion:SfListView>
```

### End Swipe Template

```xml
<syncfusion:SfListView.EndSwipeTemplate >
    <DataTemplate>
        <Grid BackgroundColor="#F44336" Padding="20">
            <Label Text="Delete" 
                   TextColor="White"
                   FontSize="16"
                   VerticalOptions="Center" 
                   HorizontalOptions="Center" />
        </Grid>
    </DataTemplate>
</syncfusion:SfListView.EndSwipeTemplate >
```

### Both Templates

```xml
<syncfusion:SfListView AllowSwiping="True" SwipeOffset="100">
    <!-- Swipe Right to Archive -->
    <syncfusion:SfListView.StartSwipeTemplate>
        <DataTemplate>
            <Grid BackgroundColor="#4CAF50">
                <Label Text="✓ Archive" TextColor="White" 
                       HorizontalOptions="Center" VerticalOptions="Center" />
            </Grid>
        </DataTemplate>
    </syncfusion:SfListView.StartSwipeTemplate>
    
    <!-- Swipe Left to Delete -->
    <syncfusion:SfListView.EndSwipeTemplate >
        <DataTemplate>
            <Grid BackgroundColor="#F44336">
                <Label Text="✕ Delete" TextColor="White" 
                       HorizontalOptions="Center" VerticalOptions="Center" />
            </Grid>
        </DataTemplate>
    </syncfusion:SfListView.EndSwipeTemplate >
</syncfusion:SfListView>
```

### Swipe Events

#### SwipeStarting

Fired when swipe begins:

```csharp
listView.SwipeStarting += (sender, e) =>
{
    // e.DataItem - The swiped item
    // e.SwipeDirection - Left or Right
    // e.SwipeOffset - Current swipe offset
    
    Debug.WriteLine($"Swipe started on {(e.DataItem as ItemInfo)?.Name}");
};
```

#### Swiping

Fired continuously during swipe:

```csharp
listView.Swiping += (sender, e) =>
{
    // Update UI based on swipe progress
    Debug.WriteLine($"Swipe offset: {e.SwipeOffset}");
};
```

#### SwipeEnded

Fired when swipe completes:

```csharp
listView.SwipeEnded += (sender, e) =>
{
    // e.DataItem - The swiped item
    // e.SwipeDirection - Left or Right
    // e.SwipeOffset - Final swipe offset
    
    var item = e.DataItem as EmailInfo;
    
    if (e.Direction == SwipeDirection.Right && e.Offset >= 100)
    {
        // Archive action
        viewModel.ArchiveEmail(item);
    }
    else if (e.Direction == SwipeDirection.Left && e.Offset >= 100)
    {
        // Delete action
        viewModel.DeleteEmail(item);
    }
};
```

### Swipe Actions with Multiple Buttons

```xml
<syncfusion:SfListView.StartSwipeTemplate>
    <DataTemplate>
        <Grid ColumnDefinitions="Auto, Auto, Auto">
            <Grid Grid.Column="0" 
                  BackgroundColor="#FF9800" 
                  Padding="15"
                  WidthRequest="80">
                <Grid.GestureRecognizers>
                    <TapGestureRecognizer Tapped="OnFlagTapped" />
                </Grid.GestureRecognizers>
                <Label Text="🚩 Flag" 
                       TextColor="White"
                       HorizontalOptions="Center" 
                       VerticalOptions="Center" />
            </Grid>
            
            <Grid Grid.Column="1" 
                  BackgroundColor="#2196F3" 
                  Padding="15"
                  WidthRequest="80">
                <Grid.GestureRecognizers>
                    <TapGestureRecognizer Tapped="OnMoveTapped" />
                </Grid.GestureRecognizers>
                <Label Text="📁 Move" 
                       TextColor="White"
                       HorizontalOptions="Center" 
                       VerticalOptions="Center" />
            </Grid>
            
            <Grid Grid.Column="2" 
                  BackgroundColor="#F44336" 
                  Padding="15"
                  WidthRequest="80">
                <Grid.GestureRecognizers>
                    <TapGestureRecognizer Tapped="OnDeleteTapped" />
                </Grid.GestureRecognizers>
                <Label Text="🗑️ Delete" 
                       TextColor="White"
                       HorizontalOptions="Center" 
                       VerticalOptions="Center" />
            </Grid>
        </Grid>
    </DataTemplate>
</syncfusion:SfListView.StartSwipeTemplate>
```

```csharp
private void OnDeleteTapped(object sender, EventArgs e)
{
    var grid = sender as Grid;
    var item = grid?.BindingContext as EmailInfo;
    
    if (item != null)
    {
        viewModel.Emails.Remove(item);
    }
}
```

### Reset Swipe Programmatically

```csharp
private void ListView_SwipeEnded(object sender, SwipeEndedEventArgs e)
{
  if (listView.SwipeOffset > 70)
      listView.SwipeOffset = 0;
}
```

## Drag and Drop

Reorder items within the list or drag items between multiple lists.

### Enable Drag and Drop

```xml
<syncfusion:SfListView DragStartMode="OnHold" />
```

```csharp
listView.DragStartMode = DragStartMode.OnHold;
```

**DragStartMode Options:**
- `None` - Drag disabled
- `OnHold` - Long press to start drag
- `OnDragIndicator` - Tap drag handle to start

### Drag Indicator Template

Create a custom drag handle:

```xml
<syncfusion:SfListView DragStartMode="OnDragIndicator">
    <syncfusion:SfListView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10" ColumnSpacing="10">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                
                <!-- Drag handle -->
                <Image Grid.Column="0" 
                       Source="drag_handle.png"
                       WidthRequest="24"
                       HeightRequest="24"
                       VerticalOptions="Center" />
                
                <Label Grid.Column="1" 
                       Text="{Binding Name}"
                       VerticalOptions="Center" />
            </Grid>
        </DataTemplate>
    </syncfusion:SfListView.ItemTemplate>
</syncfusion:SfListView>
```

### Drag Events

> ⚠️ **IMPORTANT:** SfListView has **only one drag event**: `ItemDragging`.  
> There is **no** `DragStarting`, `DragEnded`, `ItemDragEnded`, or `ItemDropping` event.  
> All drag lifecycle stages (start, dragging, drop) are handled via `e.Action` inside the single `ItemDragging` event.

#### ItemDragging (Single Event for All Drag Actions)

The `ItemDragging` event is raised throughout the entire drag-and-drop lifecycle. Use `e.Action` to differentiate between stages.

**ItemDraggingEventArgs Members:**
| Member | Description |
|--------|-------------|
| `Action` | The drag action: `Start`, `Dragging`, or `Drop` |
| `DataItem` | The underlying data of the dragging item |
| `OldIndex` | Index in `DataSource.DisplayItems` where drag started (same as `NewIndex` when `Action = Start`) |
| `NewIndex` | Index in `DataSource.DisplayItems` where item will be dropped |
| `Bounds` | Bounds of the drag item during dragging and dropping |
| `Position` | Touch position from screen coordinates |
| `Handled` | Set to `true` to handle dragging manually (only applicable when `Action = Dragging`) |

```csharp
listView.ItemDragging += (sender, e) =>
{
    var item = e.DataItem as TaskInfo;

    switch (e.Action)
    {
        case Syncfusion.Maui.ListView.DragAction.Start:
            // Drag has started
            // e.OldIndex == e.NewIndex at this point
            Debug.WriteLine($"Drag started: {item.Name} at index {e.OldIndex}");

            // Prevent dragging locked items
            if (item.IsLocked)
                e.Handled = true; // Cancel the drag start
            break;

        case Syncfusion.Maui.ListView.DragAction.Dragging:
            // Item is being dragged over other items
            // e.OldIndex = original position, e.NewIndex = current hover position
            Debug.WriteLine($"Dragging from {e.OldIndex} to {e.NewIndex}");

            // Set e.Handled = true to prevent default reorder at this position
            // e.Handled = true;
            break;

        case Syncfusion.Maui.ListView.DragAction.Drop:
            // Item has been dropped
            // e.OldIndex = original position, e.NewIndex = final drop position
            Debug.WriteLine($"Dropped {item.Name} from {e.OldIndex} to {e.NewIndex}");

            // Optionally update your data source manually
            break;
    }
};
```

**Prevent Dragging Specific Items (using Action = Start):**
```csharp
listView.ItemDragging += (sender, e) =>
{
    if (e.Action == Syncfusion.Maui.ListView.DragAction.Start)
    {
        var item = e.DataItem as TaskInfo;
        if (item != null && item.IsLocked)
        {
            e.Handled = true; // Cancels the drag
        }
    }
};
```

**Persist Reorder to Source Collection (using Action = Drop):**
```csharp
listView.ItemDragging += (sender, e) =>
{
    if (e.Action == Syncfusion.Maui.ListView.DragAction.Drop)
    {
        var item = e.DataItem as TaskInfo;
        int oldIndex = e.OldIndex;
        int newIndex = e.NewIndex;

        if (oldIndex != newIndex && oldIndex >= 0 && newIndex >= 0
            && newIndex < viewModel.Tasks.Count)
        {
            viewModel.Tasks.Move(oldIndex, newIndex);
        }
    }
};
```

### Cross-List Drag and Drop

Drag items between two ListViews:

```xml
<Grid ColumnDefinitions="*, *" ColumnSpacing="10">
    <StackLayout Grid.Column="0">
        <Label Text="To Do" FontAttributes="Bold" />
        <syncfusion:SfListView x:Name="todoList"
                               ItemsSource="{Binding TodoItems}"
                               DragStartMode="OnHold" />
    </StackLayout>
    
    <StackLayout Grid.Column="1">
        <Label Text="Done" FontAttributes="Bold" />
        <syncfusion:SfListView x:Name="doneList"
                               ItemsSource="{Binding DoneItems}"
                               DragStartMode="OnHold" />
    </StackLayout>
</Grid>
```

```csharp
private void SetupCrossListDrag()
{
    // Use ItemDragging with Action = Drop for both lists
    todoList.ItemDragging += OnTodoListItemDragging;
    doneList.ItemDragging += OnDoneListItemDragging;
}

private void OnTodoListItemDragging(object sender, Syncfusion.Maui.ListView.ItemDraggingEventArgs e)
{
    if (e.Action == Syncfusion.Maui.ListView.DragAction.Drop)
    {
        var item = e.DataItem as TaskInfo;

        // Check if dropped position is outside this list (cross-list logic)
        if (e.NewIndex >= viewModel.TodoItems.Count)
        {
            viewModel.TodoItems.Remove(item);
            viewModel.DoneItems.Add(item);
        }
    }
}

private void OnDoneListItemDragging(object sender, Syncfusion.Maui.ListView.ItemDraggingEventArgs e)
{
    if (e.Action == Syncfusion.Maui.ListView.DragAction.Drop)
    {
        var item = e.DataItem as TaskInfo;
        Debug.WriteLine($"Done list drop: {item.Name} at {e.NewIndex}");
    }
}
```

### Custom Drag Visual

Customize the appearance while dragging using `ItemDragging` with `Action = Start`:

```csharp
// Note: Custom DragView is not a built-in property of ItemDraggingEventArgs.
// Visual feedback during drag is achieved by styling the ItemTemplate
// or using the Bounds/Position values from the ItemDragging event.
listView.ItemDragging += (sender, e) =>
{
    if (e.Action == Syncfusion.Maui.ListView.DragAction.Start)
    {
        var item = e.DataItem as TaskInfo;
        Debug.WriteLine($"Started dragging: {item.Name} at bounds {e.Bounds}");
        // Apply visual feedback via ViewModel flags or triggers if needed
    }
};
```
### Drag Item Template

By defining the `SfListView.DragItemTemplate` property you can display a custom UI while an item is being dragged. The template receives the dragged item's binding context, so you can bind to its properties (for example `Name`, `Title`, etc.). The template can be defined in XAML or in C#.

XAML

```xml
<ContentPage xmlns:syncfusion="clr-namespace:Syncfusion.Maui.ListView;assembly=Syncfusion.Maui.ListView">
    <syncfusion:SfListView x:Name="listView" 
                                     ItemsSource="{Binding ToDoList}"
                                     DragStartMode="OnHold">
        <syncfusion:SfListView.DragItemTemplate>
            <DataTemplate>
                <Grid Padding="10">
                    <Label x:Name="textLabel" Text="{Binding Name}" FontSize="15" />
                </Grid>
            </DataTemplate>
        </syncfusion:SfListView.DragItemTemplate>
    </syncfusion:SfListView>
</ContentPage>
```

C# (creating the template in code)

```csharp
listView.DragStartMode = DragStartMode.OnHold;
listView.DragItemTemplate = new DataTemplate(() =>
{
        var grid = new Grid { Padding = 10 };
        var label = new Label { FontSize = 15 };
        label.SetBinding(Label.TextProperty, "Name");
        grid.Children.Add(label);
        return grid;
});
```

Notes:
- The `DragItemTemplate` is used only as a visual representation during the drag operation; it does not replace the `ItemTemplate` shown in the list.
- Keep the drag template lightweight for best performance.


## Best Practices

### Swipe Best Practices

1. **Clear Visual Feedback** - Use distinct colors for different actions
2. **Sufficient SwipeOffset** - Set to at least 80-100 pixels
3. **Limit Actions** - Max 2-3 swipe actions per direction
4. **Common Patterns** - Follow platform conventions (e.g., right swipe to archive)
5. **Reset After Action** - Call `ResetSwipe()` after handling swipe action

### Drag and Drop Best Practices

1. **Visual Indicators** - Show clear drag handles or affordances
2. **Provide Feedback** - Visual feedback during drag (shadow, opacity)
3. **Validate Drops** - Use `ItemDropping` to enforce business rules
4. **Handle Edge Cases** - Dragging first/last items, empty lists
5. **Performance** - Avoid complex calculations in drag events

### Accessibility

Ensure swipe and drag features are accessible:

```xml
<!-- Add accessibility descriptions -->
<Grid AutomationProperties.Name="Delete action" 
      AutomationProperties.HelpText="Swipe left to delete">
    <!-- Swipe template content -->
</Grid>
```

## Complete Example: Email List with Swipe Actions

```xml
<syncfusion:SfListView x:Name="emailList"
                       ItemsSource="{Binding Emails}"
                       AllowSwiping="True"
                       SwipeOffset="200"
                       SwipeEnded="OnSwipeEnded">
    
    <syncfusion:SfListView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="15,10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>
                
                <Label Text="{Binding Subject}" 
                       FontAttributes="Bold" 
                       FontSize="16" />
                <Label Grid.Row="1" 
                       Text="{Binding Sender}" 
                       FontSize="12" 
                       TextColor="Gray" />
            </Grid>
        </DataTemplate>
    </syncfusion:SfListView.ItemTemplate>
    
    <!-- Swipe Right: Archive -->
    <syncfusion:SfListView.StartSwipeTemplate>
        <DataTemplate>
            <Grid BackgroundColor="#4CAF50">
                <StackLayout HorizontalOptions="Start" 
                             VerticalOptions="Center"
                             Padding="20">
                    <Image Source="archive_icon.png" 
                           HeightRequest="24" 
                           WidthRequest="24" />
                    <Label Text="Archive" 
                           TextColor="White" 
                           FontSize="12" />
                </StackLayout>
            </Grid>
        </DataTemplate>
    </syncfusion:SfListView.StartSwipeTemplate>
    
    <!-- Swipe Left: Delete -->
    <syncfusion:SfListView.EndSwipeTemplate>
        <DataTemplate>
            <Grid BackgroundColor="#F44336">
                <StackLayout HorizontalOptions="End" 
                             VerticalOptions="Center"
                             Padding="20">
                    <Image Source="delete_icon.png" 
                           HeightRequest="24" 
                           WidthRequest="24" />
                    <Label Text="Delete" 
                           TextColor="White" 
                           FontSize="12" />
                </StackLayout>
            </Grid>
        </DataTemplate>
    </syncfusion:SfListView.EndSwipeTemplate>
</syncfusion:SfListView>
```

```csharp
private void OnSwipeEnded(object sender, SwipeEndedEventArgs e)
{
    var email = e.DataItem as EmailInfo;
    
    if (e.Direction == SwipeDirection.Right && e.Offset >= 200)
    {
        // Archive
        viewModel.ArchiveEmail(email);
        emailList.ResetSwipe();
    }
    else if (e.Direction == SwipeDirection.Left && e.Offset >= 200)
    {
        // Delete
        viewModel.DeleteEmail(email);
        emailList.ResetSwipe();
    }
}
```
