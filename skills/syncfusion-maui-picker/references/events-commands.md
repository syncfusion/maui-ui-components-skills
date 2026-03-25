# Events and Commands in .NET MAUI Picker

The Syncfusion .NET MAUI Picker provides comprehensive event handling and command support for responding to user interactions.

## Table of Contents
- [Picker Events](#picker-events)
- [Event Details](#event-details)
- [Commands](#commands)
- [Event Handling Patterns](#event-handling-patterns)
- [Common Scenarios](#common-scenarios)

## Picker Events

The Picker provides six main events for handling user interactions, primarily used when the picker is in Dialog or RelativeDialog mode.

### Event List

| Event | Description | When Triggered |
|-------|-------------|----------------|
| **Opened** | Picker popup is opened | Dialog/RelativeDialog opens |
| **Closing** | Picker popup is about to close | Before closing (cancellable) |
| **Closed** | Picker popup is closed | After closing |
| **SelectionChanged** | Selected item changes | Selection is updated |
| **OkButtonClicked** | OK button is clicked | User confirms selection |
| **CancelButtonClicked** | Cancel button is clicked | User cancels selection |

## Event Details

### Opened Event

Triggered when the picker popup opens (Dialog or RelativeDialog mode).

**XAML:**
```xml
<picker:SfPicker x:Name="picker"
                 Opened="Picker_Opened">
    <!-- Picker configuration -->
</picker:SfPicker>
```

**C#:**
```csharp
picker.Opened += Picker_Opened;

private void Picker_Opened(object sender, EventArgs e)
{
    // Handle the open action
    // Example: Log analytics, show instructions, etc.
    Console.WriteLine("Picker opened");
}
```

### Closing Event

Triggered before the picker popup closes. Can be canceled by setting `e.Cancel` to `true`.

**XAML:**
```xml
<picker:SfPicker x:Name="picker"
                 Closing="Picker_Closing">
    <!-- Picker configuration -->
</picker:SfPicker>
```

**C#:**
```csharp
picker.Closing += Picker_Closing;

private void Picker_Closing(object sender, Syncfusion.Maui.Core.CancelEventArgs e)
{
    // Prevent closing if validation fails
    if (!ValidateSelection())
    {
        e.Cancel = true;
        DisplayAlert("Invalid Selection", "Please select a valid option", "OK");
    }
}

private bool ValidateSelection()
{
    // Add your validation logic
    return picker.Columns[0].SelectedIndex >= 0;
}
```

**Use Cases for Cancelling:**
- Validate selection before closing
- Require minimum selection criteria
- Show confirmation dialog
- Prevent accidental closure

### Closed Event

Triggered after the picker popup is closed.

**XAML:**
```xml
<picker:SfPicker x:Name="picker"
                 Closed="Picker_Closed">
    <!-- Picker configuration -->
</picker:SfPicker>
```

**C#:**
```csharp
picker.Closed += Picker_Closed;

private void Picker_Closed(object sender, EventArgs e)
{
    // Handle post-close actions
    Console.WriteLine("Picker closed");
    
    // Update UI based on selection
    UpdateUI();
}
```

### SelectionChanged Event

Triggered after the selected index changes in the picker.

**Important Notes:**
- In single-column pickers with footer and OK button enabled:
  - When `IsSelectionImmediate = false` (default): Event fires only when OK button is tapped
  - When `IsSelectionImmediate = true`: Event fires immediately upon scrolling
- In multi-column pickers: Event fires for each column selection change

**XAML:**
```xml
<picker:SfPicker x:Name="picker"
                 SelectionChanged="Picker_SelectionChanged">
    <!-- Picker configuration -->
</picker:SfPicker>
```

**C#:**
```csharp
picker.SelectionChanged += Picker_SelectionChanged;

private void Picker_SelectionChanged(object sender, PickerSelectionChangedEventArgs e)
{
    // Access the selected values
    if (e.NewValue != null && e.NewValue.Count > 0)
    {
        var selectedColumn = e.NewValue[0] as PickerColumn;
        var selectedItem = selectedColumn.SelectedItem;
        var selectedIndex = selectedColumn.SelectedIndex;
        
        Console.WriteLine($"Selected: {selectedItem} at index {selectedIndex}");
        
        // Update UI or perform actions
        UpdateBasedOnSelection(selectedItem);
    }
}
```

**Event Arguments:**
- `NewValue`: Collection of updated `PickerColumn` objects
- `OldValue`: Collection of previous `PickerColumn` objects

### OkButtonClicked Event

Triggered when the OK button in the footer is clicked.

**Requirements:**
- Footer view must be visible (`Height > 0`)
- `ShowOkButton` must be `true`

**XAML:**
```xml
<picker:SfPicker x:Name="picker"
                 OkButtonClicked="Picker_OkButtonClicked">
    <picker:SfPicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True" Height="40"/>
    </picker:SfPicker.FooterView>
</picker:SfPicker>
```

**C#:**
```csharp
picker.OkButtonClicked += Picker_OkButtonClicked;

private void Picker_OkButtonClicked(object sender, EventArgs e)
{
    // Confirm and process selection
    var selectedItem = picker.Columns[0].SelectedItem;
    
    // Save to database, update UI, etc.
    SaveSelection(selectedItem);
    
    DisplayAlert("Confirmed", $"You selected: {selectedItem}", "OK");
}
```

### CancelButtonClicked Event

Triggered when the Cancel button in the footer is clicked.

**Requirements:**
- Footer view must be visible (`Height > 0`)

**XAML:**
```xml
<picker:SfPicker x:Name="picker"
                 CancelButtonClicked="Picker_CancelButtonClicked">
    <picker:SfPicker.FooterView>
        <picker:PickerFooterView Height="40"/>
    </picker:SfPicker.FooterView>
</picker:SfPicker>
```

**C#:**
```csharp
picker.CancelButtonClicked += Picker_CancelButtonClicked;

private void Picker_CancelButtonClicked(object sender, EventArgs e)
{
    // Revert to previous selection
    RevertSelection();
    
    Console.WriteLine("Selection cancelled");
}
```

### Complete Event Registration Example

```csharp
// Register all events
picker.Opened += Picker_Opened;
picker.Closing += Picker_Closing;
picker.Closed += Picker_Closed;
picker.SelectionChanged += Picker_SelectionChanged;
picker.OkButtonClicked += Picker_OkButtonClicked;
picker.CancelButtonClicked += Picker_CancelButtonClicked;

private void Picker_Opened(object sender, EventArgs e)
{
    Console.WriteLine("Picker opened");
}

private void Picker_Closing(object sender, Syncfusion.Maui.Core.CancelEventArgs e)
{
    // Validate before closing
    if (!ValidateSelection())
    {
        e.Cancel = true;
    }
}

private void Picker_Closed(object sender, EventArgs e)
{
    Console.WriteLine("Picker closed");
}

private void Picker_SelectionChanged(object sender, PickerSelectionChangedEventArgs e)
{
    // Handle selection change
    if (e.NewValue != null && e.NewValue.Count > 0)
    {
        ProcessSelection(e.NewValue);
    }
}

private void Picker_OkButtonClicked(object sender, EventArgs e)
{
    // Confirm selection
    ConfirmSelection();
}

private void Picker_CancelButtonClicked(object sender, EventArgs e)
{
    // Cancel selection
    CancelSelection();
}
```

## Commands

The Picker supports commands for MVVM pattern implementation.

### SelectionChangedCommand

Execute a command when selection changes (MVVM-friendly).

**XAML:**
```xml
<picker:SfPicker x:Name="picker"
                 SelectionChangedCommand="{Binding SelectionChangedCommand}">
    <picker:SfPicker.Columns>
        <picker:PickerColumn ItemsSource="{Binding Items}" />
    </picker:SfPicker.Columns>
</picker:SfPicker>

<ContentPage.BindingContext>
    <local:ViewModel/>
</ContentPage.BindingContext>
```

**ViewModel:**
```csharp
using System.Windows.Input;

public class ViewModel
{
    public ICommand SelectionChangedCommand { get; set; }
    
    public ViewModel()
    {
        SelectionChangedCommand = new Command(OnSelectionChanged);
    }
    
    private void OnSelectionChanged()
    {
        // Handle selection change in ViewModel
        Console.WriteLine("Selection changed via command");
        
        // Update other properties, trigger navigation, etc.
    }
}
```

**With Parameters:**
```csharp
SelectionChangedCommand = new Command<PickerSelectionChangedEventArgs>(OnSelectionChanged);

private void OnSelectionChanged(PickerSelectionChangedEventArgs args)
{
    if (args.NewValue != null && args.NewValue.Count > 0)
    {
        var selectedColumn = args.NewValue[0] as PickerColumn;
        // Process selection
    }
}
```

### AcceptCommand

Execute a command when the OK button is clicked.

**XAML:**
```xml
<picker:SfPicker x:Name="picker"
                 AcceptCommand="{Binding AcceptCommand}">
    <picker:SfPicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True" Height="40"/>
    </picker:SfPicker.FooterView>
</picker:SfPicker>
```

**ViewModel:**
```csharp
public class ViewModel
{
    public ICommand AcceptCommand { get; set; }
    
    public ViewModel()
    {
        AcceptCommand = new Command(OnAccept);
    }
    
    private void OnAccept()
    {
        // Confirm and save selection
        Console.WriteLine("Selection accepted");
        SaveData();
    }
}
```

### DeclineCommand

Execute a command when the Cancel button is clicked.

**XAML:**
```xml
<picker:SfPicker x:Name="picker"
                 DeclineCommand="{Binding DeclineCommand}">
    <picker:SfPicker.FooterView>
        <picker:PickerFooterView Height="40"/>
    </picker:SfPicker.FooterView>
</picker:SfPicker>
```

**ViewModel:**
```csharp
public class ViewModel
{
    public ICommand DeclineCommand { get; set; }
    
    public ViewModel()
    {
        DeclineCommand = new Command(OnDecline);
    }
    
    private void OnDecline()
    {
        // Handle cancellation
        Console.WriteLine("Selection declined");
        RevertChanges();
    }
}
```

### Complete MVVM Command Example

```csharp
public class PickerViewModel : INotifyPropertyChanged
{
    private ObservableCollection<string> items;
    private string selectedItem;

    public ObservableCollection<string> Items
    {
        get => items;
        set
        {
            items = value;
            OnPropertyChanged();
        }
    }

    public string SelectedItem
    {
        get => selectedItem;
        set
        {
            selectedItem = value;
            OnPropertyChanged();
        }
    }

    public ICommand SelectionChangedCommand { get; set; }
    public ICommand AcceptCommand { get; set; }
    public ICommand DeclineCommand { get; set; }

    public PickerViewModel()
    {
        Items = new ObservableCollection<string>
        {
            "Option 1", "Option 2", "Option 3", "Option 4"
        };

        SelectionChangedCommand = new Command(OnSelectionChanged);
        AcceptCommand = new Command(OnAccept);
        DeclineCommand = new Command(OnDecline);
    }

    private void OnSelectionChanged()
    {
        Console.WriteLine($"Selection changed to: {SelectedItem}");
        // Update UI, trigger calculations, etc.
    }

    private void OnAccept()
    {
        Console.WriteLine($"Accepted: {SelectedItem}");
        // Save to database, navigate, etc.
    }

    private void OnDecline()
    {
        Console.WriteLine("Selection cancelled");
        // Revert changes
        SelectedItem = null;
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected virtual void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

## Event Handling Patterns

### Pattern 1: Validation Before Closing

```csharp
private void Picker_Closing(object sender, Syncfusion.Maui.Core.CancelEventArgs e)
{
    bool isValid = ValidateSelection();
    
    if (!isValid)
    {
        e.Cancel = true;
        DisplayAlert("Error", "Please select a valid option", "OK");
    }
}

private bool ValidateSelection()
{
    return picker.Columns[0].SelectedIndex >= 0 && 
           picker.Columns[0].SelectedItem != null;
}
```

### Pattern 2: Immediate UI Updates

```csharp
picker.IsSelectionImmediate = true;

picker.SelectionChanged += (s, e) =>
{
    if (e.NewValue != null && e.NewValue.Count > 0)
    {
        var column = e.NewValue[0] as PickerColumn;
        UpdatePreview(column.SelectedItem);
    }
};
```

### Pattern 3: Multi-Column Dependent Selection

```csharp
picker.SelectionChanged += (s, e) =>
{
    if (e.NewValue != null && e.NewValue.Count > 0)
    {
        // First column changed, update second column
        var firstColumn = e.NewValue[0] as PickerColumn;
        UpdateDependentColumn(firstColumn.SelectedItem);
    }
};

private void UpdateDependentColumn(object selectedValue)
{
    // Update second column based on first column selection
    var newItems = GetRelatedItems(selectedValue);
    picker.Columns[1].ItemsSource = newItems;
}
```

## Common Scenarios

### Scenario 1: Form Validation

```csharp
private void Picker_OkButtonClicked(object sender, EventArgs e)
{
    var selected = picker.Columns[0].SelectedItem?.ToString();
    
    if (string.IsNullOrEmpty(selected))
    {
        DisplayAlert("Error", "Please make a selection", "OK");
        return;
    }
    
    // Save form data
    SaveFormData(selected);
}
```

### Scenario 2: Live Preview

```csharp
picker.IsSelectionImmediate = true;

picker.SelectionChanged += (s, e) =>
{
    if (e.NewValue != null)
    {
        var selectedColor = e.NewValue[0].SelectedItem?.ToString();
        UpdateColorPreview(selectedColor);
    }
};
```

### Scenario 3: Logging and Analytics

```csharp
picker.Opened += (s, e) =>
{
    LogEvent("Picker_Opened");
};

picker.OkButtonClicked += (s, e) =>
{
    var selection = picker.Columns[0].SelectedItem;
    LogEvent("Selection_Confirmed", selection);
};

picker.CancelButtonClicked += (s, e) =>
{
    LogEvent("Selection_Cancelled");
};
```

## Best Practices

1. **Use IsSelectionImmediate appropriately:**
   - `true` for live previews and immediate feedback
   - `false` for confirmable selections

2. **Handle null values:** Always check for null in event handlers

3. **Unsubscribe from events:** When disposing views, unsubscribe to prevent memory leaks

4. **Use commands for MVVM:** Prefer commands over events in MVVM applications

5. **Provide user feedback:** Show loading indicators or confirmation messages

6. **Validate selections:** Use the Closing event to prevent invalid selections

## Troubleshooting

### Issue: SelectionChanged not firing

**Solution:**
- Check if `IsSelectionImmediate` is set correctly
- Ensure footer with OK button is configured if using default behavior
- Verify event is properly subscribed

### Issue: OkButtonClicked event not firing

**Solution:**
- Verify `ShowOkButton = true`
- Ensure footer view height > 0
- Check that the picker is in Dialog or RelativeDialog mode

### Issue: Commands not executing

**Solution:**
- Verify binding context is set
- Check command is properly initialized in ViewModel
- Ensure command implements ICommand interface correctly
