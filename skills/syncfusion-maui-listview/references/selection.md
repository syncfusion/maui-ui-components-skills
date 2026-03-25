# Selection in .NET MAUI ListView

## Table of Contents
- [Overview](#overview)
- [Selection Modes](#selection-modes)
- [Selection Gestures](#selection-gestures)
- [Selection Properties](#selection-properties)
- [Selection Events](#selection-events)
- [Programmatic Selection](#programmatic-selection)
- [Common Selection Patterns](#common-selection-patterns)

## Overview

SfListView supports item selection with multiple modes and gestures. Selection enables users to choose one or more items for actions like delete, share, or details view.

**Key Features:**
- Multiple selection modes (None, Single, SingleDeselect, Multiple, Extended)
- Customizable selection gestures (Tap, DoubleTap, Hold)
- Selection highlight customization
- Selection change events with cancellation support
- Programmatic selection control

## Selection Modes

Control how many items can be selected using the `SelectionMode` property.

### None - No Selection

```xml
<syncfusion:SfListView SelectionMode="None" />
```

```csharp
using Syncfusion.Maui.ListView;

listView.SelectionMode = SelectionMode.None;
```

**Use when:** Selection is not needed, or you handle taps custom with ItemTapped event.

### Single - Select One Item

```xml
<syncfusion:SfListView SelectionMode="Single" />
```

```csharp
listView.SelectionMode = SelectionMode.Single;
```

**Default mode**. Only one item can be selected at a time. Selecting a new item deselects the previous one.

**Use for:** 
- Master-detail views
- Settings with radio-button behavior
- Navigation lists

### SingleDeselect - Toggle Selection

```xml
<syncfusion:SfListView SelectionMode="SingleDeselect" />
```

```csharp
listView.SelectionMode = SelectionMode.SingleDeselect;
```

Like single, but tapping a selected item deselects it (toggle behavior).

**Use for:**
- User-friendly single-select
- Tag/category selection
- Filter options

### Multiple - Select Multiple Items

```xml
<syncfusion:SfListView SelectionMode="Multiple" />
```

```csharp
listView.SelectionMode = SelectionMode.Multiple;
```

Multiple items can be selected simultaneously. Tapping a selected item does NOT deselect it.

**Use for:**
- Bulk operations (delete, move, share)
- Email/message selection
- Multi-item checkout

### Extended - Select Multiple Items

```xml
<syncfusion:SfListView SelectionMode="Extended" />
```

```csharp
listView.SelectionMode = SelectionMode.Extended;
```

Multiple items can be selected simultaneously. Use Shift to select ranges and use Ctrl on Windows or Command on macOS to toggle items.



## Selection Gestures

Control which gesture triggers selection using the `SelectionGesture` property.

### Tap (Default)

```xml
<syncfusion:SfListView SelectionGesture="Tap" />
```

```csharp
using Syncfusion.Maui.ListView;

listView.SelectionGesture = TouchGesture.Tap;
```

Single tap selects the item.

### DoubleTap

```xml
<syncfusion:SfListView SelectionGesture="DoubleTap" />
```

```csharp
listView.SelectionGesture = TouchGesture.DoubleTap;
```

Double-tap required to select. Single tap can trigger `ItemTapped` event without selection.

**Use when:** You want tap for navigation and double-tap for selection.

### LongPress (Hold)

```xml
<syncfusion:SfListView SelectionGesture="LongPress" />
```

```csharp
listView.SelectionGesture = TouchGesture.LongPress;
```

Long press selects the item.

**Use for:**
- Context menu activation
- Android-style long-press selection

**Note:** Hold gesture not supported with mouse on WinUI platform.

## Selection Properties

### SelectedItem

Gets or sets the currently selected item (Single mode).

```csharp
// Get selected item
var selectedProduct = listView.SelectedItem as ProductInfo;

// Set selected item programmatically
listView.SelectedItem = viewModel.Products[0];

// Clear selection
listView.SelectedItem = null;
```

**XAML Binding:**
```xml
<syncfusion:SfListView SelectionMode="Single"
                       SelectedItem="{Binding SelectedProduct, Mode=TwoWay}" />
```

### SelectedItems

Gets or sets the collection of selected items (Multiple modes).

```csharp
// Get selected items
var selectedProducts = listView.SelectedItems
    .Cast<ProductInfo>()
    .ToList();

// Select multiple items
listView.SelectedItems.Add(viewModel.Products[0]);
listView.SelectedItems.Add(viewModel.Products[1]);

// Clear all selections
listView.SelectedItems.Clear();
```

**XAML Binding:**
```xml
<syncfusion:SfListView SelectionMode="Multiple"
                       SelectedItems="{Binding SelectedProducts, Mode=TwoWay}" />
```

### SelectionBackground

Customizes the selected item background color/brush.

```xml
<syncfusion:SfListView SelectionBackground="#4CAF50" />
```

```csharp
listView.SelectionBackground = Colors.LightBlue;
```


## Selection Events

### SelectionChanging

Fired **before** selection changes. Can be cancelled.

```csharp
listView.SelectionChanging += (sender, e) =>
{
    // e.AddedItems - items being added to selection
    // e.RemovedItems - items being removed from selection
    
    // Cancel selection of specific items
    var product = e.AddedItems.FirstOrDefault() as ProductInfo;
    if (product != null && product.IsLocked)
    {
        e.Cancel = true; // Prevent selection
        DisplayAlert("Locked", "This item cannot be selected", "OK");
    }
};
```

**EventArgs Properties:**
- `AddedItems` - Items being selected
- `RemovedItems` - Items being deselected
- `Cancel` - Set true to prevent the selection change

### SelectionChanged

Fired **after** selection changes. Cannot be cancelled.

```csharp
listView.SelectionChanged += (sender, e) =>
{
    // e.AddedItems - items added to selection
    // e.RemovedItems - items removed from selection

    var selectedCount =listView.SelectedItems.Count;
    statusLabel.Text = $"{selectedCount} item(s) selected";
    
    // Enable/disable action buttons based on selection
    deleteButton.IsEnabled = selectedCount > 0;
};
```

**EventArgs Properties:**
- `AddedItems` - Newly selected items
- `RemovedItems` - Newly deselected items

## Programmatic Selection

### Select a item on Load

```csharp
listView.Loaded += (sender, e) =>
{
    // Select first item
    if (viewModel.Products.Count > 0)
    {
        listView.SelectedItem = viewModel.Products[0];
    }
};
```

### Select All Items

```csharp
private void OnSelectAllClicked(object sender, EventArgs e)
{
    listView.SelectAll();
}
```

### Deselect All Items

```csharp
private void OnDeselectAllClicked(object sender, EventArgs e)
{
    listView.SelectedItems.Clear();
}
```


### Select by Condition

```csharp
private void OnSelectFavoritesClicked(object sender, EventArgs e)
{
    listView.SelectedItems.Clear();
    
    foreach (var product in viewModel.Products)
    {
        if (product.IsFavorite)
        {
            listView.SelectedItems.Add(product);
        }
    }
}
```

## Common Selection Patterns

### Pattern 1: Master-Detail with Single Selection

```xml
<Grid ColumnDefinitions="*, 2*">
    <!-- Master list -->
    <syncfusion:SfListView Grid.Column="0"
                           x:Name="masterList"
                           ItemsSource="{Binding Products}"
                           SelectionMode="Single"
                           SelectedItem="{Binding SelectedProduct, Mode=TwoWay}" />
    
    <!-- Detail view -->
    <StackLayout Grid.Column="1" 
                 Padding="20"
                 BindingContext="{Binding SelectedProduct}">
        <Label Text="{Binding Name}" FontSize="24" FontAttributes="Bold" />
        <Label Text="{Binding Description}" />
        <Label Text="{Binding Price, StringFormat='${0:F2}'}" />
    </StackLayout>
</Grid>
```

### Pattern 2: Multi-Select with Action Bar

```xml
<Grid RowDefinitions="Auto, *">
    <!-- Action bar (visible when items selected) -->
    <Grid Grid.Row="0" 
          Padding="10"
          BackgroundColor="LightBlue"
          IsVisible="{Binding HasSelectedItems}">
        <HorizontalStackLayout Spacing="10">
            <Label Text="{Binding SelectedCount, StringFormat='{0} selected'}"
                   VerticalOptions="Center" />
            <Button Text="Delete" Command="{Binding DeleteCommand}" />
            <Button Text="Share" Command="{Binding ShareCommand}" />
        </HorizontalStackLayout>
    </Grid>
    
    <!-- List -->
    <syncfusion:SfListView Grid.Row="1"
                           ItemsSource="{Binding Items}"
                           SelectionMode="Multiple"
                           SelectedItems="{Binding SelectedItems}"
                           SelectionChanged="OnSelectionChanged" />
</Grid>
```

```csharp
private void OnSelectionChanged(object sender, ItemSelectionChangedEventArgs e)
{
    viewModel.HasSelectedItems = listView.SelectedItems.Count > 0;
    viewModel.SelectedCount = listView.SelectedItems.Count;
}
```

### Pattern 3: Checkbox-Style Selection

```xml
<syncfusion:SfListView SelectionMode="Multiple" SelectionBackground="Transparent">
    <syncfusion:SfListView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                
                <!-- Checkbox indicator (bound to selection state) -->
                <CheckBox Grid.Column="0"
                          IsChecked="{Binding IsSelected}"
                          VerticalOptions="Center" />
                
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       VerticalOptions="Center"
                       Margin="10,0,0,0" />
            </Grid>
        </DataTemplate>
    </syncfusion:SfListView.ItemTemplate>
</syncfusion:SfListView>
```

### Pattern 4: Conditional Selection

```csharp
listView.SelectionChanging += (sender, e) =>
{
    foreach (var item in e.AddedItems)
    {
        var product = item as ProductInfo;
        
        // Prevent selection if out of stock
        if (product != null && !product.InStock)
        {
            e.Cancel = true;
            DisplayAlert("Unavailable", 
                "Out of stock items cannot be selected", "OK");
            break;
        }
        
        // Limit selection to 5 items
        if (listView.SelectedItems.Count >= 5)
        {
            e.Cancel = true;
            DisplayAlert("Limit Reached", 
                "Maximum 5 items can be selected", "OK");
            break;
        }
    }
};
```

### Pattern 5: Select All / Deselect All Buttons

```xml
<StackLayout>
    <Grid ColumnDefinitions="*, *" Padding="10">
        <Button Grid.Column="0" 
                Text="Select All"
                Clicked="OnSelectAll" />
        <Button Grid.Column="1" 
                Text="Deselect All"
                Clicked="OnDeselectAll" />
    </Grid>
    
    <syncfusion:SfListView x:Name="listView"
                           ItemsSource="{Binding Items}"
                           SelectionMode="Multiple" />
</StackLayout>
```

```csharp
private void OnSelectAll(object sender, EventArgs e)
{
    listView.SelectAll();
}

private void OnDeselectAll(object sender, EventArgs e)
{
    listView.SelectedItems.Clear();
}
```

### Pattern 6: Long Press for Multi-Select Mode

```csharp
private bool isMultiSelectMode = false;

private void InitializeSelectionBehavior()
{
    listView.SelectionMode = SelectionMode.None;
    listView.ItemLongPress += OnItemLongPress;
    listView.ItemTapped += OnItemTapped;
}

private void OnItemLongPress(object sender, ItemLongPressEventArgs e)
{
    // Enter multi-select mode
    isMultiSelectMode = true;
    listView.SelectionMode = SelectionMode.Multiple;
    listView.SelectedItems.Add(e.DataItem);
    
    // Show action bar
    actionBar.IsVisible = true;
}

private void OnItemTapped(object sender, ItemTappedEventArgs e)
{
    if (!isMultiSelectMode)
    {
        // Normal tap - navigate to details
        Navigation.PushAsync(new DetailPage(e.DataItem));
    }
}

private void ExitMultiSelectMode()
{
    isMultiSelectMode = false;
    listView.SelectionMode = SelectionMode.None;
    listView.SelectedItems.Clear();
    actionBar.IsVisible = false;
}
```

## Troubleshooting

**Issue:** Selection not working
→ Check SelectionMode is not None

**Issue:** SelectedItem binding not updating
→ Ensure Mode=TwoWay in binding

**Issue:** SelectionChanged event not firing
→ Verify event is subscribed before items are added

**Issue:** Can't deselect a node
→ Use SingleDeselect mode

**Issue:** Hold gesture not working on Windows
→ Hold gesture with mouse not supported on WinUI; use keyboard/touch

**Issue:** Selection cleared unexpectedly
→ Check if ItemsSource is being reset (recreates collection)
