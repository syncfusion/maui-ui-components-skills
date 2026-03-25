# Selection in .NET MAUI Autocomplete

## Table of Contents
- [Overview](#overview)
- [Selection Modes](#selection-modes)
- [Single Selection](#single-selection)
- [Multiple Selection](#multiple-selection)
- [Selection Events](#selection-events)
- [Selected Value](#selected-value)
- [Clear Button](#clear-button)
- [Dropdown State](#dropdown-state)
- [Programmatic Clearing](#programmatic-clearing)

## Overview

The Autocomplete control supports both single and multiple item selection with various display modes and comprehensive event handling for selection changes.

## Selection Modes

Set the selection behavior using the `SelectionMode` property:

```csharp
public enum AutocompleteSelectionMode
{
    Single,    // Select one item (default)
    Multiple   // Select multiple items
}
```

## Single Selection

In single selection mode (default), users can select one item from the dropdown.

### Basic Configuration

```xml
<editors:SfAutocomplete ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name"
                        TextMemberPath="Name" />
```

```csharp
SfAutocomplete autocomplete = new SfAutocomplete
{
    SelectionMode = AutocompleteSelectionMode.Single, // Default
    ItemsSource = socialMediaViewModel.SocialMedias,
    DisplayMemberPath = "Name",
    TextMemberPath = "Name"
};
```

### Accessing Selected Item

Use the `SelectedItem` property to get or set the selected item:

```csharp
// Get selected item
var selected = autocomplete.SelectedItem as SocialMedia;
if (selected != null)
{
    Console.WriteLine($"Selected: {selected.Name}");
}

// Set selected item programmatically
autocomplete.SelectedItem = socialMediaViewModel.SocialMedias[2];
```

### Selection Methods

Users can select items by:
1. Typing and pressing **Enter** key
2. Clicking/tapping an item in the dropdown
3. Losing focus from the text box (auto-selects first match)

## Multiple Selection

Enable multiple selection to allow users to select several items from the dropdown.

### Basic Configuration

```xml
<editors:SfAutocomplete SelectionMode="Multiple"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name"
                        TextMemberPath="Name" />
```

```csharp
SfAutocomplete autocomplete = new SfAutocomplete
{
    SelectionMode = AutocompleteSelectionMode.Multiple,
    ItemsSource = socialMediaViewModel.SocialMedias,
    DisplayMemberPath = "Name",
    TextMemberPath = "Name"
};
```

### Accessing Selected Items

Use the `SelectedItems` property to get or set multiple selected items:

```csharp
// Get selected items
ObservableCollection<object> selected = autocomplete.SelectedItems;
foreach (var item in selected)
{
    var media = item as SocialMedia;
    Console.WriteLine($"Selected: {media.Name}");
}

// Set selected items programmatically
var viewModel = new SocialMediaViewModel();
autocomplete.SelectedItems = new ObservableCollection<object>
{
    viewModel.SocialMedias[0],
    viewModel.SocialMedias[2],
    viewModel.SocialMedias[4]
};
```

### Multi-Selection Display Modes

The `MultiSelectionDisplayMode` property controls how selected items are displayed:

#### Token Mode (Default)

Displays selected items as chips/tokens with close buttons:

```xml
<editors:SfAutocomplete SelectionMode="Multiple"
                        MultiSelectionDisplayMode="Token"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name"
                        TextMemberPath="Name" />
```

```csharp
autocomplete.MultiSelectionDisplayMode = AutocompleteMultiSelectionDisplayMode.Token;
```

**Token Wrap Modes:**

The `TokensWrapMode` property controls token layout:

**Wrap Mode** - Tokens wrap to next line:
```xml
<editors:SfAutocomplete SelectionMode="Multiple"
                        MultiSelectionDisplayMode="Token"
                        TokensWrapMode="Wrap"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name"
                        TextMemberPath="Name" />
```

**None Mode** - Tokens arranged horizontally with scrolling:
```xml
<editors:SfAutocomplete SelectionMode="Multiple"
                        MultiSelectionDisplayMode="Token"
                        TokensWrapMode="None"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name"
                        TextMemberPath="Name" />
```

```csharp
// Wrap mode
autocomplete.TokensWrapMode = AutocompleteTokensWrapMode.Wrap;

// None mode (horizontal scroll)
autocomplete.TokensWrapMode = AutocompleteTokensWrapMode.None;
```

#### Delimiter Mode

Displays selected items separated by a delimiter character:

```xml
<editors:SfAutocomplete SelectionMode="Multiple"
                        MultiSelectionDisplayMode="Delimiter"
                        DelimiterText="/"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name"
                        TextMemberPath="Name" />
```

```csharp
autocomplete.MultiSelectionDisplayMode = AutocompleteMultiSelectionDisplayMode.Delimiter;
autocomplete.DelimiterText = "/"; // Default is ","
```

**Example Output:** `Facebook / Twitter / Instagram`

**Custom Delimiter Examples:**
- `, ` (comma-space) - Default
- ` | ` (pipe)
- ` ; ` (semicolon)
- ` + ` (plus)

## Selection Events

### SelectionChanging Event

Fires before selection changes, allowing you to cancel the selection:

```xml
<editors:SfAutocomplete SelectionChanging="OnSelectionChanging"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name"
                        TextMemberPath="Name" />
```

```csharp
private void OnSelectionChanging(object sender, SelectionChangingEventArgs e)
{
    // Access items about to be selected
    var currentSelection = e.CurrentSelection;
    
    // Access previously selected items
    var previousSelection = e.PreviousSelection;
    
    // Cancel the selection if needed
    if (SomeCondition)
    {
        e.Cancel = true;
    }
}
```

**EventArgs Properties:**
- `CurrentSelection` - Items about to be selected
- `PreviousSelection` - Previously selected items
- `Cancel` - Set to `true` to prevent selection

**Use Cases:**
- Validate selection before accepting
- Implement selection limits
- Show confirmation dialogs

### SelectionChanged Event

Fires after selection changes:

```xml
<editors:SfAutocomplete SelectionChanged="OnSelectionChanged"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name"
                        TextMemberPath="Name" />
```

```csharp
private void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    // Get newly selected items
    if (e.AddedItems.Count > 0)
    {
        foreach (var item in e.AddedItems)
        {
            var media = item as SocialMedia;
            Console.WriteLine($"Added: {media.Name}");
        }
    }
    
    // Get removed items
    if (e.RemovedItems.Count > 0)
    {
        foreach (var item in e.RemovedItems)
        {
            var media = item as SocialMedia;
            Console.WriteLine($"Removed: {media.Name}");
        }
    }
}
```

**EventArgs Properties:**
- `AddedItems` - Newly selected items
- `RemovedItems` - Unselected items

**Note:** `CurrentSelection` and `PreviousSelection` properties are obsolete. Use `AddedItems` and `RemovedItems` instead.

## Selected Value

The `SelectedValue` property provides access to a specific property of the selected item(s) using `SelectedValuePath`.

### Single Selection

```xml
<editors:SfAutocomplete ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name"
                        TextMemberPath="Name"
                        SelectedValuePath="ID"
                        SelectionChanged="OnSelectionChanged" />

<HorizontalStackLayout>
    <Label Text="SelectedValue: " />
    <Label x:Name="selectedValueLabel" />
</HorizontalStackLayout>
```

```csharp
private void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (autocomplete.SelectedValue != null)
    {
        selectedValueLabel.Text = autocomplete.SelectedValue.ToString();
        // If ID=5 is selected, displays: "5"
    }
}
```

**How it works:**
- `SelectedItem` returns full object: `SocialMedia { Name="Facebook", ID=0 }`
- `SelectedValue` returns property specified by `SelectedValuePath`: `0`

### Multiple Selection

For multiple selection, `SelectedValue` is an `IList<object>` collection:

```xml
<editors:SfAutocomplete SelectionMode="Multiple"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name"
                        TextMemberPath="Name"
                        SelectedValuePath="ID"
                        SelectedValue="{Binding SelectedValueList}"
                        SelectionChanged="OnSelectionChanged" />

<HorizontalStackLayout>
    <Label Text="SelectedValue count: " />
    <Label x:Name="selectedCountLabel" />
</HorizontalStackLayout>
```

```csharp
private void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (autocomplete.SelectedValue is IList<object> values)
    {
        selectedCountLabel.Text = values.Count.ToString();
        // If 3 items selected, displays: "3"
        
        // Access individual values
        foreach (var value in values)
        {
            Console.WriteLine($"ID: {value}");
        }
    }
}
```

**ViewModel Setup:**
```csharp
public class SocialMediaViewModel
{
    public ObservableCollection<object> SelectedValueList { get; set; }
    
    public SocialMediaViewModel()
    {
        SelectedValueList = new ObservableCollection<object>
        {
            SocialMedias[0].ID,  // Preselect by ID
            SocialMedias[4].ID
        };
    }
}
```

**Note:** If `SelectedValuePath` is not specified, `SelectedValue` equals `SelectedItem` (single) or `SelectedItems` (multiple).

## Clear Button

The clear button "X" allows users to quickly clear entered text and selections.

### Visibility

Control clear button visibility with `IsClearButtonVisible`:

```xml
<editors:SfAutocomplete IsClearButtonVisible="false"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name"
                        TextMemberPath="Name" />
```

```csharp
autocomplete.IsClearButtonVisible = false;
```

**Default:** `true`

### Clear Button Clicked Event

Handle the clear button click:

```xml
<editors:SfAutocomplete ClearButtonClicked="OnClearButtonClicked"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name"
                        TextMemberPath="Name" />
```

```csharp
private void OnClearButtonClicked(object sender, EventArgs e)
{
    // Perform actions when clear button is clicked
    Console.WriteLine("Clear button clicked");
}
```

## Dropdown State

### IsDropDownOpen

Control dropdown visibility programmatically:

```xml
<editors:SfAutocomplete IsDropDownOpen="True"
                        ItemsSource="{Binding SocialMedias}"
                        DisplayMemberPath="Name"
                        TextMemberPath="Name" />
```

```csharp
// Open dropdown
autocomplete.IsDropDownOpen = true;

// Close dropdown
autocomplete.IsDropDownOpen = false;

// Check if dropdown is open
if (autocomplete.IsDropDownOpen)
{
    Console.WriteLine("Dropdown is visible");
}
```

**Default:** `false`

**Use Cases:**
- Open dropdown on button click
- Close dropdown after specific action
- Implement custom dropdown triggers

## Programmatic Clearing

### Clear() Method

The `Clear()` method removes all selected items and clears the input text:

```csharp
// Clear everything
autocomplete.Clear();
```

**Effects:**
- Clears `Text` property
- Clears `SelectedItem` (single mode)
- Clears `SelectedItems` (multiple mode)
- Clears `SelectedValue`
- Triggers `SelectionChanged` event

**Use Cases:**
- Reset form after submission
- Clear selection on cancel button
- Implement "Clear All" functionality

## Complete Examples

### Single Selection with Events

```xml
<StackLayout Padding="20">
    <editors:SfAutocomplete x:Name="autocomplete"
                            SelectionMode="Single"
                            ItemsSource="{Binding SocialMedias}"
                            DisplayMemberPath="Name"
                            TextMemberPath="Name"
                            SelectedValuePath="ID"
                            SelectionChanging="OnSelectionChanging"
                            SelectionChanged="OnSelectionChanged"
                            Placeholder="Select social media" />
    
    <Label x:Name="statusLabel" Margin="0,10,0,0" />
</StackLayout>
```

```csharp
private void OnSelectionChanging(object sender, SelectionChangingEventArgs e)
{
    statusLabel.Text = "Selection changing...";
}

private void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (e.AddedItems.Count > 0)
    {
        var item = e.AddedItems[0] as SocialMedia;
        statusLabel.Text = $"Selected: {item.Name}, ID: {autocomplete.SelectedValue}";
    }
}
```

### Multiple Selection with Tokens

```xml
<StackLayout Padding="20">
    <editors:SfAutocomplete x:Name="multiAutocomplete"
                            SelectionMode="Multiple"
                            MultiSelectionDisplayMode="Token"
                            TokensWrapMode="Wrap"
                            ItemsSource="{Binding SocialMedias}"
                            DisplayMemberPath="Name"
                            TextMemberPath="Name"
                            Placeholder="Add tags..."
                            SelectionChanged="OnTagsChanged" />
    
    <Button Text="Clear All" Clicked="OnClearClicked" />
    
    <Label x:Name="countLabel" Margin="0,10,0,0" />
</StackLayout>
```

```csharp
private void OnTagsChanged(object sender, SelectionChangedEventArgs e)
{
    int count = multiAutocomplete.SelectedItems.Count;
    countLabel.Text = $"Selected: {count} tag(s)";
}

private void OnClearClicked(object sender, EventArgs e)
{
    multiAutocomplete.Clear();
}
```

## Best Practices

1. **Event Usage**
   - Use `SelectionChanging` for validation and cancellation
   - Use `SelectionChanged` for acting on confirmed selections
   - Always check `AddedItems`/`RemovedItems` count before accessing

2. **Multiple Selection**
   - Use Token mode for better UX with visual feedback
   - Set `TokensWrapMode="Wrap"` for responsive layouts
   - Consider EnableAutoSize for dynamic height (see advanced-features.md)

3. **SelectedValue**
   - Set `SelectedValuePath` to extract specific properties
   - Useful for binding to IDs instead of full objects
   - Check type when casting (single: object, multiple: IList<object>)

4. **Performance**
   - Avoid heavy operations in `SelectionChanged` handler
   - Use async operations for API calls triggered by selection
   - Consider debouncing for rapid selections

## Next Steps

- **UI customization** - See [ui-customization.md](ui-customization.md) for token styling
- **Advanced features** - See [advanced-features.md](advanced-features.md) for AutoSizing
- **ValueChanged event** - See [ui-customization.md](ui-customization.md) for text change handling
