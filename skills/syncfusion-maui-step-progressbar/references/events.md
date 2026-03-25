# Events in StepProgressBar

## Overview

The StepProgressBar provides two key events for handling user interactions and step state changes:
- **StepTapped**: Triggered when a user taps/clicks on a step
- **StepStatusChanged**: Triggered when a step's status changes (completed, in-progress, not-started)

These events enable interactive navigation, logging, analytics, and dynamic UI updates.

## StepTapped Event

The `StepTapped` event fires when any step indicator is tapped or clicked.

### Event Arguments

The event provides `StepTappedEventArgs` with:

- **StepItem** (`StepProgressBarItem`): The tapped step item object

### XAML Implementation

```xml
<stepProgressBar:SfStepProgressBar 
    ItemsSource="{Binding StepProgressItem}"
    StepTapped="OnStepTapped">
</stepProgressBar:SfStepProgressBar>
```

### Event Handler

```csharp
private void OnStepTapped(object sender, Syncfusion.Maui.ProgressBar.StepTappedEventArgs e)
{
    // Get the tapped step item
    StepProgressBarItem tappedItem = e.StepItem as StepProgressBarItem;
    
    // Handle the tap action
    DisplayAlert("Step Tapped", 
                 $"You tapped step: {tappedItem.PrimaryText}", 
                 "OK");
}
```

### C# Implementation

```csharp
SfStepProgressBar stepProgressBar = new SfStepProgressBar()
{
    ItemsSource = viewModel.StepProgressItem
};

stepProgressBar.StepTapped += OnStepTapped;

this.Content = stepProgressBar;
```

### Common Use Cases

**Use Case 1: Navigation to Specific Step**

Allow users to jump to a previously completed step:

```csharp
private void OnStepTapped(object sender, StepTappedEventArgs e)
{
    var stepProgressBar = sender as SfStepProgressBar;
}
```

**Use Case 2: Show Step Details**

Display additional information about a step:

```csharp
private async void OnStepTapped(object sender, StepTappedEventArgs e)
{
    StepProgressBarItem item = e.StepItem as StepProgressBarItem;
    
    await DisplayAlert("Step Details: ", item.PrimaryText, 
                       "Close");
}
```

## StepStatusChanged Event

The `StepStatusChanged` event fires when a step's status transitions between states (completed → in-progress, in-progress → not-started, etc.).

### Event Arguments

The event provides `StepStatusChangedEventArgs` with:
- **StepItem** (`StepProgressBarItem`): The step item object
- **OldStatus** (`StepStatus`): Previous status (Completed, InProgress, NotStarted)
- **NewStatus** (`StepStatus`): New status after change

### XAML Implementation

```xml
<stepProgressBar:SfStepProgressBar 
    ItemsSource="{Binding StepProgressItem}"
    ActiveStepIndex="1"
    StepStatusChanged="OnStepStatusChanged">
</stepProgressBar:SfStepProgressBar>
```

### Event Handler

```csharp
private void OnStepStatusChanged(object sender, Syncfusion.Maui.ProgressBar.StepStatusChangedEventArgs e)
{
    // Get step details
    StepProgressBarItem item = e.StepItem as StepProgressBarItem;
    StepStatus oldStatus = e.OldStatus;
    StepStatus newStatus = e.NewStatus;
    
    // Handle the status change
    Console.WriteLine($"Step: {oldStatus} → {newStatus}");
}
```

### C# Implementation

```csharp
SfStepProgressBar stepProgressBar = new SfStepProgressBar()
{
    ItemsSource = viewModel.StepProgressItem,
    ActiveStepIndex = 1
};

stepProgressBar.StepStatusChanged += OnStepStatusChanged;

this.Content = stepProgressBar;
```

## Best Practices

### Practice 1: Event Handler Performance

Keep event handlers lightweight:
```csharp
// ✓ Good: Quick operation
private void OnStepTapped(object sender, StepTappedEventArgs e)
{
    
}

// ✗ Bad: Heavy operation blocking UI
private void OnStepTapped(object sender, StepTappedEventArgs e)
{
    // This will freeze UI
    var data = LoadLargeDataFromDatabase();
    ProcessData(data);
}

// ✓ Better: Use async for heavy operations
private async void OnStepTapped(object sender, StepTappedEventArgs e)
{
    var data = await LoadLargeDataFromDatabaseAsync();
    await ProcessDataAsync(data);
}
```

### Practice 2: Null Checks

Always check event arguments:
```csharp
private void OnStepTapped(object sender, StepTappedEventArgs e)
{
    if (e?.Item == null) return;
    
    var item = e.StepItem as StepProgressBarItem;
    // Safe to use item
}
```

### Practice 3: Unsubscribe Events

Prevent memory leaks by unsubscribing:
```csharp
protected override void OnDisappearing()
{
    base.OnDisappearing();
    stepProgress.StepTapped -= OnStepTapped;
    stepProgress.StepStatusChanged -= OnStepStatusChanged;
}
```

### Practice 4: Combine with Commands (MVVM)

For MVVM patterns, consider wrapping events in commands for better testability and separation of concerns.