# Selection in .NET MAUI ComboBox

## Table of Contents
- [Overview](#overview)
- [Single Selection](#single-selection)
  - [UI Selection](#ui-selection)
  - [Programmatic Selection](#programmatic-selection)
- [Multiple Selection](#multiple-selection)
  - [Delimiter Mode](#delimiter-mode)
  - [Token Mode](#token-mode)
- [Selection Events](#selection-events)
  - [SelectionChanging Event](#selectionchanging-event)
  - [SelectionChanged Event](#selectionchanged-event)
- [Selected Value](#selected-value)
- [Opening Dropdown Programmatically](#opening-dropdown-programmatically)
- [Clearing Selection](#clearing-selection)
- [Best Practices](#best-practices)

## Overview

The .NET MAUI ComboBox allows users to select single or multiple items from the dropdown list. The selection mode can be set using the `SelectionMode` property with two available modes:
- **Single:** Select one item at a time (default)
- **Multiple:** Select multiple items

## Single Selection

Single selection mode allows users to select one item from the dropdown list.

### UI Selection

The selected item can be changed interactively by selecting from the dropdown list or entering the value (in editable mode).

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    IsEditable="True"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    IsEditable = true,
    ItemsSource = socialMediaViewModel.SocialMedias,
    DisplayMemberPath = "Name",
    TextMemberPath = "Name"
};
```

### Programmatic Selection

The selected item can be changed programmatically using the `SelectedItem` or `SelectedIndex` properties.

#### Using SelectedItem

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    MaxDropDownHeight="250"
                    IsEditable="True"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    SelectedItem="{Binding SelectedMedia}" />
```

**C#:**
```csharp
// Set by object reference
comboBox.SelectedItem = socialMediaViewModel.SocialMedias[2];

// Get selected item
var selected = comboBox.SelectedItem as SocialMedia;
```

#### Using SelectedIndex

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    MaxDropDownHeight="250"
                    IsEditable="True"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    SelectedIndex="2" />
```

**C#:**
```csharp
// Set by index (0-based)
comboBox.SelectedIndex = 2;

// Get selected index
int selectedIndex = comboBox.SelectedIndex;
```

## Multiple Selection

Multiple selection mode allows users to select multiple items from the dropdown list by setting `SelectionMode` to `Multiple`.

**Basic Setup:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ItemsSource="{Binding SocialMedias}"
                    SelectedItems="{Binding SelectedItemsList}"
                    SelectionMode="Multiple"
                    MaxDropDownHeight="250"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name" />
```

**ViewModel:**
```csharp
public ObservableCollection<SocialMedia> SelectedItemsList { get; set; }

public SocialMediaViewModel()
{
    ObservableCollection<SocialMedia> socialMediasList = SocialMedias;
    SelectedItemsList = new ObservableCollection<SocialMedia>();
    SelectedItemsList.Add(socialMediasList[0]);
    SelectedItemsList.Add(socialMediasList[2]);
}
```

### Multi-Selection Display Modes

There are two ways to display multi-selection items using the `MultiSelectionDisplayMode` property:
- **Token:** Display selected items as chips/tokens (default)
- **Delimiter:** Display selected items separated by a delimiter character

### Delimiter Mode

When `MultiSelectionDisplayMode` is set to `Delimiter`, selected items are separated by a specified character. You can set the delimiter text using the `DelimiterText` property. The default delimiter is `","`.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ItemsSource="{Binding SocialMedias}"
                    SelectionMode="Multiple"
                    MultiSelectionDisplayMode="Delimiter"
                    DelimiterText="/"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    Placeholder="Enter Media" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    ItemsSource = socialMediaViewModel.SocialMedias,
    SelectionMode = ComboBoxSelectionMode.Multiple,
    MultiSelectionDisplayMode = ComboBoxMultiSelectionDisplayMode.Delimiter,
    DelimiterText = "/",
    DisplayMemberPath = "Name",
    TextMemberPath = "Name",
    Placeholder = "Enter Media"
};
```

**Result:** Selected items displayed as "Facebook/Instagram/Twitter"

### Token Mode

Token mode displays selected items as individual chips that can be removed independently. It has two layout options controlled by the `TokensWrapMode` property:
- **Wrap:** Tokens wrap to the next line
- **None:** Tokens scroll horizontally

#### Wrap Mode

When `TokensWrapMode` is set to `Wrap`, selected items wrap to the next line.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ItemsSource="{Binding SocialMedias}"
                    SelectionMode="Multiple"
                    Placeholder="Enter Media"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    TokensWrapMode="Wrap" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    ItemsSource = socialMediaViewModel.SocialMedias,
    SelectionMode = ComboBoxSelectionMode.Multiple,
    Placeholder = "Enter Media",
    DisplayMemberPath = "Name",
    TextMemberPath = "Name",
    TokensWrapMode = ComboBoxTokensWrapMode.Wrap
};
```

#### None Mode

When `TokensWrapMode` is set to `None`, selected items are arranged in a horizontal scrollable orientation.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    ItemsSource="{Binding SocialMedias}"
                    SelectionMode="Multiple"
                    Placeholder="Enter Media"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name"
                    TokensWrapMode="None" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    ItemsSource = socialMediaViewModel.SocialMedias,
    SelectionMode = ComboBoxSelectionMode.Multiple,
    Placeholder = "Enter Media",
    DisplayMemberPath = "Name",
    TextMemberPath = "Name",
    TokensWrapMode = ComboBoxTokensWrapMode.None
};
```

## Selection Events

### SelectionChanging Event

The `SelectionChanging` event is triggered when a user attempts to select an item from the dropdown list. This event allows you to intercept and cancel the selection.

**Event Arguments:**
- `CurrentSelection` - Item(s) about to be selected
- `PreviousSelection` - Previously selected item(s)
- `Cancel` - Set to `true` to prevent the selection change

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    WidthRequest="250"
                    HeightRequest="40"
                    ItemsSource="{Binding SocialMedias}"
                    TextMemberPath="Name"
                    DisplayMemberPath="Name"
                    SelectionChanging="OnSelectionChanging" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    WidthRequest = 250,
    HeightRequest = 40,
    ItemsSource = socialMediaViewModel.SocialMedias,
    TextMemberPath = "Name",
    DisplayMemberPath = "Name"
};
comboBox.SelectionChanging += OnSelectionChanging;

private async void OnSelectionChanging(object sender, SelectionChangingEventArgs e)
{
    // Validate selection before it happens
    var currentSelection = e.CurrentSelection;
    var previousSelection = e.PreviousSelection;
    
    // Cancel selection if needed
    if (ShouldCancelSelection(currentSelection))
    {
        e.Cancel = true;
        await DisplayAlert("Alert", "Selection not allowed", "Ok");
    }
}
```

### SelectionChanged Event

The `SelectionChanged` event is triggered after an item is selected from the dropdown list.

**Event Arguments:**
- `AddedItems` - Items that were currently selected
- `RemovedItems` - Items that were unselected

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    TextMemberPath="Name"
                    DisplayMemberPath="Name"
                    ItemsSource="{Binding SocialMedias}"
                    SelectionChanged="OnSelectionChanged" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    ItemsSource = socialMediaViewModel.SocialMedias,
    DisplayMemberPath = "Name",
    TextMemberPath = "Name"
};
comboBox.SelectionChanged += OnSelectionChanged;

private async void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var addedItems = e.AddedItems;
    var removedItems = e.RemovedItems;
    
    await DisplayAlert("Alert", $"Selected Item has changed", "Ok");
    
    // Process newly selected items
    foreach (var item in addedItems)
    {
        var socialMedia = item as SocialMedia;
        // Handle selection
    }
}
```

**Note:** The event arguments `CurrentSelection` and `PreviousSelection` are marked as obsolete. Use `AddedItems` and `RemovedItems` instead.

## Selected Value

The `SelectedValue` property allows you to get or set the selected value based on the `SelectedItem` or `SelectedItems`, depending on the selection mode. The `SelectedValuePath` property specifies which property of the selected item is used to populate the `SelectedValue`.

### Single Selection Mode

In single selection mode, `SelectedValue` holds the value defined by `SelectedValuePath`, such as "ID".

**XAML:**
```xml
<StackLayout>
    <Label Text="SelectedValue:" />
    <Label x:Name="selectedValue" />
    
    <editors:SfComboBox x:Name="comboBox"
                        MaxDropDownHeight="250"
                        TextMemberPath="Name"
                        DisplayMemberPath="Name"
                        ItemsSource="{Binding SocialMedias}"
                        SelectedValuePath="ID"
                        SelectionChanged="OnSelectionChanged" />
</StackLayout>
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    MaxDropDownHeight = 250,
    ItemsSource = socialMediaViewModel.SocialMedias,
    DisplayMemberPath = "Name",
    TextMemberPath = "Name",
    SelectedValuePath = "ID"
};
comboBox.SelectionChanged += OnSelectionChanged;

private void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    selectedValue.Text = comboBox.SelectedValue?.ToString();
}
```

### Multiple Selection Mode

In multi-selection mode, `SelectedValue` is a collection of values derived from `SelectedItems` based on the `SelectedValuePath`.

**XAML:**
```xml
<StackLayout>
    <Label Text="SelectedValue count:" />
    <Label x:Name="selectedValue" />
    
    <editors:SfComboBox x:Name="comboBox"
                        TextMemberPath="Name"
                        DisplayMemberPath="Name"
                        ItemsSource="{Binding SocialMedias}"
                        SelectionMode="Multiple"
                        SelectedValuePath="ID"
                        SelectedValue="{Binding SelectedValueList}"
                        SelectionChanged="OnSelectionChanged" />
</StackLayout>
```

**ViewModel:**
```csharp
public ObservableCollection<object> SelectedValueList { get; set; }

public SocialMediaViewModel()
{
    ObservableCollection<SocialMedia> socialMediasList = SocialMedias;
    SelectedValueList = new ObservableCollection<object>();
    SelectedValueList.Add(socialMediasList[4].ID);
    SelectedValueList.Add(socialMediasList[6].ID);
}
```

**C#:**
```csharp
private void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (comboBox != null && comboBox.SelectedValue is IList<object> value)
    {
        selectedValue.Text = value.Count.ToString();
    }
}
```

**Note:** If `SelectedValuePath` is not specified, `SelectedValue` will be the same as `SelectedItem` or `SelectedItems`.

## Opening Dropdown Programmatically

The dropdown list can be opened or closed programmatically using the `IsDropDownOpen` property. The default value is `false`.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    IsEditable="true"
                    ItemsSource="{Binding SocialMedias}"
                    IsDropDownOpen="true"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    IsEditable = true,
    ItemsSource = socialMediaViewModel.SocialMedias,
    IsDropDownOpen = true,
    DisplayMemberPath = "Name",
    TextMemberPath = "Name"
};

// Open dropdown programmatically
comboBox.IsDropDownOpen = true;

// Close dropdown programmatically
comboBox.IsDropDownOpen = false;
```

## Clearing Selection

Users can remove selected items and input text using the `Clear` method.

```csharp
// Clear all selections and text
comboBox.Clear();
```

This method:
- Clears `SelectedItem` or `SelectedItems`
- Clears the `Text` property
- Resets the control to its initial state

## Best Practices

### Single Selection
1. Use `SelectedItem` for object-based binding
2. Use `SelectedIndex` for index-based selection
3. Use `SelectedValue` with `SelectedValuePath` for value-based scenarios

### Multiple Selection
1. Use `Token` mode for better visibility of selections
2. Use `Wrap` mode when space allows
3. Use `Delimiter` mode for compact display
4. Bind `SelectedItems` to an ObservableCollection for two-way binding

### Selection Events
1. Use `SelectionChanging` to validate and potentially cancel selections
2. Use `SelectionChanged` to respond after selection is confirmed
3. Process `AddedItems` and `RemovedItems` for efficient handling

### Performance
1. Avoid heavy operations in `SelectionChanged` for large datasets
2. Use `SelectedValue` instead of searching `SelectedItem` properties
3. Debounce selection operations when needed

## Common Scenarios

### Pre-selecting Items

```csharp
// Single selection
comboBox.SelectedIndex = 0; // Select first item

// Multiple selection
var itemsToSelect = new ObservableCollection<SocialMedia>
{
    SocialMedias[0],
    SocialMedias[2],
    SocialMedias[5]
};
comboBox.SelectedItems = itemsToSelect;
```

### Validating Selection

```csharp
comboBox.SelectionChanging += (s, e) =>
{
    var currentItem = e.CurrentSelection.FirstOrDefault() as SocialMedia;
    if (currentItem != null && !IsValidSelection(currentItem))
    {
        e.Cancel = true;
        DisplayAlert("Invalid", "This selection is not allowed", "OK");
    }
};
```

### Maximum Selection Limit

```csharp
comboBox.SelectionChanging += (s, e) =>
{
    if (e.CurrentSelection.Count > 3)
    {
        e.Cancel = true;
        DisplayAlert("Limit", "Maximum 3 selections allowed", "OK");
    }
};
```

## Related Topics

- [Editing Modes](editing-modes.md) - Configuring editable behavior
- [Filtering](filtering.md) - Filtering items during selection
- [UI Customization](ui-customization.md) - Styling selected items and tokens
- [Events and Methods](events-and-methods.md) - Complete event reference
