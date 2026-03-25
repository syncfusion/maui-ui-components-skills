# Events and Methods in .NET MAUI ComboBox

## Table of Contents
- [Events Overview](#events-overview)
- [Selection Events](#selection-events)
- [Dropdown Events](#dropdown-events)
- [User Interaction Events](#user-interaction-events)
- [Methods](#methods)
- [Common Event Patterns](#common-event-patterns)

## Events Overview

The Syncfusion .NET MAUI ComboBox provides a comprehensive set of events to handle user interactions, selection changes, and dropdown state changes.

## Selection Events

### SelectionChanging Event

Raised when the selection is about to change. This event can be canceled to prevent the selection change.

**Event Args:** `Syncfusion.Maui.Inputs.SelectionChangingEventArgs`

**Properties:**
- `AddedItems` - Collection of items being added to selection
- `RemovedItems` - Collection of items being removed from selection
- `Cancel` - Set to `true` to prevent the selection change

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    SelectionChanging="comboBox_SelectionChanging" />
```

**C#:**
```csharp
comboBox.SelectionChanging += comboBox_SelectionChanging;

private void comboBox_SelectionChanging(object sender, SelectionChangingEventArgs e)
{
    // Get items being added
    var addedItems = e.AddedItems;
    
    // Get items being removed
    var removedItems = e.RemovedItems;
    
    // Prevent selection of specific item
    if (addedItems.Count > 0 && addedItems[0] is SocialMedia media)
    {
        if (media.Name == "Facebook")
        {
            e.Cancel = true;
            DisplayAlert("Selection Blocked", "Facebook cannot be selected", "OK");
        }
    }
}
```

**Common Use Cases:**
- Validate selection before allowing it
- Prevent selection of disabled or restricted items
- Show confirmation dialogs for critical selections

### SelectionChanged Event

Raised after the selection has changed.

**Event Args:** `Syncfusion.Maui.Inputs.SelectionChangedEventArgs`

**Properties:**
- `AddedItems` - Collection of newly selected items
- `RemovedItems` - Collection of previously selected items

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    SelectionChanged="comboBox_SelectionChanged" />
```

**C#:**
```csharp
comboBox.SelectionChanged += comboBox_SelectionChanged;

private void comboBox_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    // Handle added items
    if (e.AddedItems != null && e.AddedItems.Count > 0)
    {
        var selectedItem = e.AddedItems[0] as SocialMedia;
        Debug.WriteLine($"Selected: {selectedItem?.Name}");
    }
    
    // Handle removed items
    if (e.RemovedItems != null && e.RemovedItems.Count > 0)
    {
        var deselectedItem = e.RemovedItems[0] as SocialMedia;
        Debug.WriteLine($"Deselected: {deselectedItem?.Name}");
    }
}
```

**Multiple Selection Example:**
```csharp
private void comboBox_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var allSelectedItems = comboBox.SelectedItems;
    Debug.WriteLine($"Total selected: {allSelectedItems.Count}");
    
    foreach (SocialMedia item in allSelectedItems)
    {
        Debug.WriteLine($"- {item.Name}");
    }
}
```

### ValueChanged Event

Raised when the `SelectedValue` property changes.

**Event Args:** `EventArgs`

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    SelectedValuePath="ID"
                    ValueChanged="comboBox_ValueChanged" />
```

**C#:**
```csharp
comboBox.ValueChanged += comboBox_ValueChanged;

private void comboBox_ValueChanged(object sender, EventArgs e)
{
    var selectedValue = comboBox.SelectedValue;
    Debug.WriteLine($"Selected Value: {selectedValue}");
}
```

## Dropdown Events

### DropDownOpening Event

Raised when the dropdown is about to open. This event can be canceled to prevent the dropdown from opening.

**Event Args:** `Syncfusion.Maui.Core.CancelEventArgs`

**Properties:**
- `Cancel` - Set to `true` to prevent dropdown from opening

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    DropDownOpening="comboBox_DropDownOpening" />
```

**C#:**
```csharp
comboBox.DropDownOpening += comboBox_DropDownOpening;

private void comboBox_DropDownOpening(object sender, CancelEventArgs e)
{
    // Prevent dropdown from opening under certain conditions
    if (!IsUserAuthenticated())
    {
        e.Cancel = true;
        DisplayAlert("Access Denied", "Please login first", "OK");
    }
}
```

### DropDownOpened Event

Raised after the dropdown has been opened.

**Event Args:** `EventArgs`

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    DropDownOpened="comboBox_DropDownOpened" />
```

**C#:**
```csharp
comboBox.DropDownOpened += comboBox_DropDownOpened;

private void comboBox_DropDownOpened(object sender, EventArgs e)
{
    Debug.WriteLine("Dropdown is now open");
    
    // Log analytics
    LogEvent("ComboBox_DropdownOpened");
    
    // Dynamically refresh items if needed
    RefreshDropdownItems();
}
```

### DropDownClosed Event

Raised after the dropdown has been closed.

**Event Args:** `EventArgs`

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    DropDownClosed="comboBox_DropDownClosed" />
```

**C#:**
```csharp
comboBox.DropDownClosed += comboBox_DropDownClosed;

private void comboBox_DropDownClosed(object sender, EventArgs e)
{
    Debug.WriteLine("Dropdown is now closed");
    
    // Save state or preferences
    SaveUserSelection();
}
```

## User Interaction Events

### Completed Event

Raised when the user finalizes text entry by pressing the return key (editable mode only).

**Event Args:** `EventArgs`

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    IsEditable="True"
                    Completed="comboBox_Completed" />
```

**C#:**
```csharp
comboBox.Completed += comboBox_Completed;

private async void comboBox_Completed(object sender, EventArgs e)
{
    var enteredText = comboBox.Text;
    await DisplayAlert("Text Entry", $"You entered: {enteredText}", "OK");
    
    // Process the entered text
    ProcessUserInput(enteredText);
}
```

**Note:** Not supported on Android platform.

### ClearButtonClicked Event

Raised when the clear button is clicked.

**Event Args:** `EventArgs`

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    IsClearButtonVisible="True"
                    ClearButtonClicked="comboBox_ClearButtonClicked" />
```

**C#:**
```csharp
comboBox.ClearButtonClicked += comboBox_ClearButtonClicked;

private void comboBox_ClearButtonClicked(object sender, EventArgs e)
{
    Debug.WriteLine("Clear button clicked");
    
    // Log analytics
    LogEvent("ComboBox_ClearButtonClicked");
    
    // Reset related state
    ResetRelatedControls();
}
```

### LoadMoreButtonTapped Event

Raised when the load more button is tapped (when `MaximumSuggestion` is set).

**Event Args:** `EventArgs`

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    MaximumSuggestion="10"
                    LoadMoreButtonTapped="comboBox_LoadMoreButtonTapped" />
```

**C#:**
```csharp
comboBox.LoadMoreButtonTapped += comboBox_LoadMoreButtonTapped;

private async void comboBox_LoadMoreButtonTapped(object sender, EventArgs e)
{
    // Show loading indicator
    IsLoadingMore = true;
    
    // Load more data from server
    var additionalItems = await FetchMoreItemsAsync();
    
    // Add to collection
    foreach (var item in additionalItems)
    {
        SocialMedias.Add(item);
    }
    
    IsLoadingMore = false;
}
```

## Methods

### Clear Method

Clears the selected items and resets the ComboBox state.

**Signature:**
```csharp
public void Clear()
```

**Usage:**
```csharp
// Clear button click handler
private void OnClearButtonClicked(object sender, EventArgs e)
{
    comboBox.Clear();
}
```

**What it clears:**
- `SelectedItem` → `null`
- `SelectedIndex` → `-1`
- `SelectedItems` → Empty collection
- `SelectedValue` → `null`
- `Text` → `string.Empty` (in editable mode)

**Example:**
```csharp
// Programmatically clear selection
comboBox.Clear();

// Verify state
Debug.Assert(comboBox.SelectedItem == null);
Debug.Assert(comboBox.SelectedIndex == -1);
Debug.Assert(comboBox.SelectedItems.Count == 0);
```

## Common Event Patterns

### 1. Selection Validation with Cancel

**C#:**
```csharp
private void comboBox_SelectionChanging(object sender, SelectionChangingEventArgs e)
{
    if (e.AddedItems.Count > 0)
    {
        var item = e.AddedItems[0] as SocialMedia;
        
        // Validate based on business rules
        if (!IsSelectionValid(item))
        {
            e.Cancel = true;
            DisplayAlert("Invalid Selection", "This item cannot be selected.", "OK");
        }
    }
}
```

### 2. Cascading ComboBox

**C#:**
```csharp
// Parent ComboBox
private void categoryComboBox_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (e.AddedItems.Count > 0)
    {
        var category = e.AddedItems[0] as Category;
        
        // Update child ComboBox items
        subCategoryComboBox.ItemsSource = GetSubCategories(category.Id);
        subCategoryComboBox.Clear();
    }
}
```

### 3. Dropdown State Management

**C#:**
```csharp
private void comboBox_DropDownOpening(object sender, CancelEventArgs e)
{
    // Refresh data before showing dropdown
    RefreshItemsSource();
}

private void comboBox_DropDownOpened(object sender, EventArgs e)
{
    // Log for analytics
    Analytics.TrackEvent("ComboBox_Opened");
}

private void comboBox_DropDownClosed(object sender, EventArgs e)
{
    // Save user preferences
    SaveSelectedItemToPreferences();
}
```

### 4. Multiple Selection Handling

**C#:**
```csharp
private void comboBox_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var selectedCount = comboBox.SelectedItems.Count;
    
    // Update UI based on selection count
    selectedCountLabel.Text = $"{selectedCount} items selected";
    
    // Enable/disable buttons
    deleteButton.IsEnabled = selectedCount > 0;
    selectAllButton.IsEnabled = selectedCount < comboBox.ItemsSource.Cast<object>().Count();
}
```

### 5. Dynamic ItemsSource Update

**C#:**
```csharp
private async void searchComboBox_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (e.AddedItems.Count > 0)
    {
        var searchTerm = e.AddedItems[0] as string;
        
        // Fetch results from server
        var results = await SearchServerAsync(searchTerm);
        
        // Update another ComboBox with results
        resultsComboBox.ItemsSource = results;
        resultsComboBox.IsDropDownOpen = true;
    }
}
```

### 6. Form Validation on Completion

**C#:**
```csharp
private void comboBox_Completed(object sender, EventArgs e)
{
    var enteredText = comboBox.Text;
    
    // Validate input
    if (string.IsNullOrWhiteSpace(enteredText))
    {
        errorLabel.Text = "Field cannot be empty";
        errorLabel.IsVisible = true;
        return;
    }
    
    // Check if entered text matches an item
    var matchingItem = comboBox.ItemsSource.Cast<SocialMedia>()
        .FirstOrDefault(x => x.Name.Equals(enteredText, StringComparison.OrdinalIgnoreCase));
    
    if (matchingItem == null)
    {
        errorLabel.Text = "Please select a valid option";
        errorLabel.IsVisible = true;
    }
    else
    {
        errorLabel.IsVisible = false;
        SubmitForm();
    }
}
```

## Event Execution Order

**When selecting an item:**
1. `SelectionChanging` (cancelable)
2. `SelectionChanged`
3. `ValueChanged` (if `SelectedValue` changes)

**When opening dropdown:**
1. `DropDownOpening` (cancelable)
2. `DropDownOpened`

**When closing dropdown:**
1. `DropDownClosed`

**When clearing selection:**
1. `SelectionChanging` (with removed items)
2. `SelectionChanged` (with removed items)
3. `ValueChanged`
4. `ClearButtonClicked` (if triggered by button)

## Best Practices

1. **Cancel Events Judiciously:**
   - Only cancel `SelectionChanging` or `DropDownOpening` when necessary
   - Provide clear feedback to the user when canceling

2. **Avoid Heavy Operations:**
   - Don't perform expensive operations in frequently-fired events
   - Use async/await for I/O operations

3. **Check Event Args:**
   - Always check `AddedItems` and `RemovedItems` count before accessing
   - Handle both null and empty collections

4. **Unsubscribe When Needed:**
   - Unsubscribe from events when the page/view is disposed to prevent memory leaks

5. **Use Appropriate Event:**
   - Use `SelectionChanging` for validation
   - Use `SelectionChanged` for reactions to selection
   - Use `DropDownOpening` for data refresh

6. **Multiple Selection:**
   - Process all items in `AddedItems` and `RemovedItems` for multiple selection mode

## Related Topics

- [Selection](selection.md) - Selection modes and configuration
- [Filtering](filtering.md) - Filter behavior and events
- [UI Customization](ui-customization.md) - Customize appearance
- [Advanced Features](advanced-features.md) - Load more and other features
