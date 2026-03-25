# Selection in TreeView

This guide explains how to implement and customize selection in the TreeView control.

## Table of Contents
- [Overview](#overview)
- [Selection Modes](#selection-modes)
- [UI Selection](#ui-selection)
- [Programmatic Selection](#programmatic-selection)
- [Selected Items Management](#selected-items-management)
- [Full Row Selection](#full-row-selection)
- [Selection Styling](#selection-styling)
- [Selection Events](#selection-events)
- [Keyboard Navigation](#keyboard-navigation)
- [Best Practices](#best-practices)

---

## Overview

The TreeView provides comprehensive selection capabilities including single selection, multiple selection, and extended selection with keyboard modifiers. Selection can be triggered through UI interactions or programmatically.

---

## Selection Modes

Set the selection mode using the `SelectionMode` property.

### Available Modes

| Mode | Description | Use Case |
|------|-------------|----------|
| `None` | Selection disabled | Read-only views |
| `Single` | Single item selection, no deselect on re-click | Standard selection |
| `SingleDeselect` | Single item, deselect on re-click | Toggle selection |
| `Multiple` | Multiple items, click to toggle | Multi-select lists |
| `Extended` | Multiple items with keyboard modifiers | Advanced multi-select |

### Setting Selection Mode

**XAML:**

```xml
<syncfusion:SfTreeView x:Name="treeView" 
                       SelectionMode="Multiple"/>
```

**C#:**

```csharp
treeView.SelectionMode = TreeViewSelectionMode.Multiple;
```

---

## UI Selection

### Single Selection

Default mode where only one item can be selected at a time.

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       ItemsSource="{Binding Items}"
                       ChildPropertyName="SubItems"
                       SelectionMode="Single"/>
```

**Behavior:**
- Click an item to select it
- Click another item to change selection
- Selected item remains selected even when clicked again

### SingleDeselect Selection

Similar to Single but allows deselection by clicking the selected item.

```csharp
treeView.SelectionMode = TreeViewSelectionMode.SingleDeselect;
```

**Behavior:**
- Click an item to select it
- Click the same item to deselect it
- Only one item can be selected at a time

### Multiple Selection

Allows selecting multiple items by clicking each one.

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       SelectionMode="Multiple"
                       ItemsSource="{Binding Files}"
                       ChildPropertyName="SubFiles"/>
```

**Behavior:**
- Click items to toggle selection
- Multiple items can be selected
- No keyboard modifiers required

### Extended Selection

Multiple selection using keyboard modifiers.

```csharp
treeView.SelectionMode = TreeViewSelectionMode.Extended;
```

**Behavior:**
- **Click:** Select single item (clear previous)
- **Ctrl+Click:** Toggle individual items
- **Shift+Click:** Select range
- **Ctrl+A:** Select all (keyboard)

---

## Programmatic Selection

### Single Item Selection

Set the `SelectedItem` property to select a single item.

```csharp
// Select a specific item
treeView.SelectedItem = viewModel.Items[2];
```

**XAML Binding:**

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       SelectedItem="{Binding SelectedPlace}"
                       ItemsSource="{Binding CountriesInfo}"
                       ChildPropertyName="States"/>
```

**ViewModel:**

```csharp
public class CountriesViewModel : INotifyPropertyChanged
{
    private object selectedPlace;
    
    public object SelectedPlace
    {
        get { return selectedPlace; }
        set
        {
            selectedPlace = value;
            OnPropertyChanged("SelectedPlace");
        }
    }
    
    public CountriesViewModel()
    {
        // Set initial selection
        SelectedPlace = CountriesInfo[0].States[0];
    }
    
    // INotifyPropertyChanged implementation...
}
```

### Multiple Items Selection

Add items to the `SelectedItems` collection.

```csharp
// Select multiple items
treeView.SelectedItems.Add(viewModel.Items[2]);
treeView.SelectedItems.Add(viewModel.Items[3]);
treeView.SelectedItems.Add(viewModel.Items[5]);
```

**XAML Binding:**

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       SelectionMode="Multiple"
                       SelectedItems="{Binding SelectedCountries}"
                       ItemsSource="{Binding CountriesInfo}"
                       ChildPropertyName="States"/>
```

**ViewModel:**

```csharp
public class CountriesViewModel
{
    public ObservableCollection<object> SelectedCountries { get; set; }
    
    public CountriesViewModel()
    {
        SelectedCountries = new ObservableCollection<object>();
        
        // Pre-select items
        SelectedCountries.Add(CountriesInfo[0].States[0]);
        SelectedCountries.Add(CountriesInfo[0].States[1]);
    }
}
```

### Important Constraints

⚠️ **Exception Cases:**

1. **SelectionMode = None:** Setting `SelectedItem` or `SelectedItems` throws an exception
2. **Single/SingleDeselect Mode:** Adding multiple items to `SelectedItems` throws an exception

---

## Selected Items Management

### Get Selected Items

#### Single Selection

```csharp
var selected = treeView.SelectedItem;
if (selected != null)
{
    var data = selected as YourDataModel;
    // Process selected item
}
```

#### Multiple Selection

```csharp
var selectedItems = treeView.SelectedItems;
foreach (var item in selectedItems)
{
    var data = item as YourDataModel;
    // Process each selected item
}
```

### Clear Selection

```csharp
// Clear all selections
treeView.SelectedItems.Clear();
```

### CurrentItem vs SelectedItem

| Property | Single Selection | Multiple Selection |
|----------|------------------|-------------------|
| `SelectedItem` | Selected item | First selected item |
| `CurrentItem` | Selected item | Last selected item |

```csharp
// In multiple selection
var firstSelected = treeView.SelectedItem;  // First item selected
var lastSelected = treeView.CurrentItem;     // Last item selected
```

---

## Full Row Selection

By default, selection starts from the indent level. Enable full row selection to span the entire width.

### Enable Full Row Selection

**XAML:**

```xml
<syncfusion:SfTreeView x:Name="treeView" 
                       FullRowSelect="True"
                       SelectionMode="Single"/>
```

**C#:**

```csharp
treeView.FullRowSelect = true;
```

**Effect:**
- Selection highlight spans the full width of the TreeView
- Better visual feedback
- Easier to see selected items

---

## Selection Styling

### Selection Background

Customize the background color of selected items.

**XAML:**

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       SelectionBackground="#EADDFF"
                       SelectionMode="Single"/>
```

**C#:**

```csharp
treeView.SelectionBackground = Color.FromHex("#EADDFF");
```

### Selection Foreground

Customize the text color of selected items (unbound mode only).

**XAML:**

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       SelectionForeground="#1C1B1F"
                       SelectionBackground="#EADDFF"
                       SelectionMode="Single"/>
```

**C#:**

```csharp
treeView.SelectionForeground = Color.FromHex("#1C1B1F");
treeView.SelectionBackground = Color.FromHex("#EADDFF");
```

**Note:** `SelectionForeground` only works in unbound mode. For bound mode, use custom templates with converters.

### Runtime Style Changes

```csharp
// Change selection colors at runtime
private void ApplyDarkTheme()
{
    treeView.SelectionBackground = Color.FromHex("#2C2C2C");
    treeView.SelectionForeground = Color.FromHex("#FFFFFF");
}

private void ApplyLightTheme()
{
    treeView.SelectionBackground = Color.FromHex("#EADDFF");
    treeView.SelectionForeground = Color.FromHex("#1C1B1F");
}
```

---

## Selection Events

### SelectionChanging Event

Fired before selection changes. Allows cancellation.

```csharp
treeView.SelectionChanging += TreeView_SelectionChanging;

private void TreeView_SelectionChanging(object sender, ItemSelectionChangingEventArgs e)
{
    // Get items being added
    if (e.AddedItems.Count > 0)
    {
        var newItem = e.AddedItems[0];
        
        // Cancel selection for specific items
        if (newItem == restrictedItem)
        {
            e.Cancel = true;
            return;
        }
    }
    
    // Get items being removed
    if (e.RemovedItems.Count > 0)
    {
        var oldItem = e.RemovedItems[0];
        // Handle deselection
    }
}
```

**Event Args Properties:**
- `AddedItems`: Collection of items being selected
- `RemovedItems`: Collection of items being deselected
- `Cancel`: Set to `true` to prevent the selection change

**Use Cases:**
- Validate selection before allowing
- Prevent selection of certain items
- Show confirmation dialogs
- Implement custom selection logic

### SelectionChanged Event

Fired after selection changes complete.

```csharp
treeView.SelectionChanged += TreeView_SelectionChanged;

private void TreeView_SelectionChanged(object sender, ItemSelectionChangedEventArgs e)
{
    // Get newly selected items
    if (e.AddedItems.Count > 0)
    {
        var newItem = e.AddedItems[0] as FileManager;
        DisplayAlert("Selected", $"{newItem.ItemName} was selected", "OK");
    }
    
    // Get deselected items
    if (e.RemovedItems.Count > 0)
    {
        var oldItem = e.RemovedItems[0] as FileManager;
        // Handle deselection
    }
    
    // Update UI or perform actions
    UpdateDetailsPanel();
}
```

**Event Args Properties:**
- `AddedItems`: Collection of newly selected items
- `RemovedItems`: Collection of deselected items

**Use Cases:**
- Update detail views
- Trigger navigation
- Update toolbar states
- Perform actions based on selection

**Important:** Both events only fire for UI interactions, not programmatic selection changes.

---

## Keyboard Navigation

TreeView supports keyboard navigation on desktop platforms (Windows, macOS).

### Single/SingleDeselect Mode

- **Arrow Keys:** Navigate through items
- **Enter/Space:** Select focused item
- **Focus border** displayed around current item

### Multiple/Extended Mode

- **Arrow Keys:** Move focus (CurrentItem)
- **Space:** Toggle selection of focused item
- **Ctrl+A:** Select all items
- **Focus border** shown on CurrentItem, not selected items

### Keyboard Selection Example

```csharp
// Access the current focused item
var focusedItem = treeView.CurrentItem;

// In Multiple mode, CurrentItem may differ from SelectedItem
if (treeView.SelectionMode == TreeViewSelectionMode.Multiple)
{
    var firstSelected = treeView.SelectedItem;
    var currentFocus = treeView.CurrentItem;
    
    // These can be different items
    if (firstSelected != currentFocus)
    {
        // Handle difference
    }
}
```

---

## Best Practices

### Choose the Right Selection Mode

- **None:** For display-only views, informational trees
- **Single:** Most common, for navigating hierarchies
- **SingleDeselect:** When users need to toggle selection
- **Multiple:** For batch operations, multi-select actions
- **Extended:** For power users, complex selection scenarios

### Performance Tips

1. **Avoid frequent programmatic changes** to `SelectedItems` in loops
2. **Clear selection before bulk changes:**
   ```csharp
   treeView.SelectedItems.Clear();
   // Add multiple items
   treeView.SelectedItems.Add(item1);
   treeView.SelectedItems.Add(item2);
   ```
3. **Use `SelectionChanging` to validate** instead of checking in `SelectionChanged`

### Selection Validation Pattern

```csharp
private void TreeView_SelectionChanging(object sender, ItemSelectionChangingEventArgs e)
{
    if (e.AddedItems.Count > 0)
    {
        var item = e.AddedItems[0] as FileItem;
        
        // Prevent selection of locked files
        if (item.IsLocked)
        {
            e.Cancel = true;
            DisplayAlert("Locked", "Cannot select locked items", "OK");
            return;
        }
        
        // Limit selection count
        if (treeView.SelectedItems.Count >= 10)
        {
            e.Cancel = true;
            DisplayAlert("Limit", "Maximum 10 items can be selected", "OK");
            return;
        }
    }
}
```

### MVVM Pattern

Always use two-way binding for selection properties:

```xml
<syncfusion:SfTreeView SelectedItem="{Binding SelectedItem, Mode=TwoWay}"
                       SelectedItems="{Binding SelectedItems, Mode=TwoWay}"/>
```

---

## Common Issues and Solutions

### Issue: Selection Styling Not Visible

**Problem:** Background color is overridden by ItemTemplate.

**Solution:** Don't set background in ItemTemplate Grid:

```xml
<!-- ❌ Wrong -->
<DataTemplate>
    <Grid BackgroundColor="White">
        <Label Text="{Binding Name}"/>
    </Grid>
</DataTemplate>

<!-- ✅ Correct -->
<DataTemplate>
    <Grid>
        <Label Text="{Binding Name}"/>
    </Grid>
</DataTemplate>
```

### Issue: Duplicated Items Selection

**Problem:** Only the first instance of duplicated items gets selected.

**Solution:** Ensure unique data items in the collection or use IDs for comparison.

### Issue: SelectionForeground Not Working

**Problem:** `SelectionForeground` has no effect.

**Solution:** `SelectionForeground` only works in unbound mode. For bound mode, use a converter in ItemTemplate.

---

## Related Topics

- [Templating](templating.md) - Customize selected item appearance
- [MVVM Support](mvvm-support.md) - Selection binding in MVVM
- [Keyboard Navigation](#keyboard-navigation) - Desktop selection patterns
