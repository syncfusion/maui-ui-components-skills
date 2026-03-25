# Selection and Interaction in .NET MAUI Toolbar

Learn how to configure selection modes, enable tooltips, and manage keyboard navigation and item states.

## Table of Contents
- [Overview](#overview)
- [Selection Modes](#selection-modes)
- [Programmatic Selection](#programmatic-selection)
- [Selection State Management](#selection-state-management)
- [Tooltips](#tooltips)
- [Item Enabled State](#item-enabled-state)
- [Keyboard Navigation](#keyboard-navigation)
- [Best Practices](#best-practices)

## Overview

The SfToolbar supports three selection modes (Single, SingleDeselect, Multiple) and provides interactive features like tooltips, keyboard navigation, and enabled/disabled states for toolbar items. Selection is event-driven through the `SelectionChanged` event and `ToolbarSelectionChangedEventArgs`.

## Selection Modes

Control how users can select toolbar items using the `SelectionMode` property. The toolbar supports three selection modes through the `ToolbarSelectionMode` enum.

### Selection Mode Options
- **Single** - Only one item can be selected at a time (DEFAULT)
- **SingleDeselect** - Single item selection with ability to deselect by tapping again
- **Multiple** - Multiple items can be selected simultaneously

### Single Selection Mode (Default)

Only one item can be selected at a time. This is the default mode. Selecting a new item deselects the previous one.

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56" SelectionMode="Single">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Cut" ToolTipText="Cut">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE719;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        <toolbar:SfToolbarItem Name="Copy" ToolTipText="Copy">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE718;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        <toolbar:SfToolbarItem Name="Paste" ToolTipText="Paste">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE71A;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

**C#:**
```csharp
SfToolbar toolbar = new SfToolbar
{
    HeightRequest = 56,
    SelectionMode = ToolbarSelectionMode.Single
};
```

**Use Cases:**
- Text alignment options (Left, Center, Right, Justify)
- View modes (List, Grid, Tile)
- Sort options (Ascending, Descending)
- Radio button-like behavior

### SingleDeselect Selection Mode

Single item selection mode where you can also deselect the selected item by tapping it again. This provides more control over the selection state.

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56" SelectionMode="SingleDeselect">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Favorite" ToolTipText="Favorite">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE87D;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        <toolbar:SfToolbarItem Name="Share" ToolTipText="Share">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE72D;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        <toolbar:SfToolbarItem Name="Archive" ToolTipText="Archive">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE74B;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

**C#:**
```csharp
SfToolbar toolbar = new SfToolbar
{
    HeightRequest = 56,
    SelectionMode = ToolbarSelectionMode.SingleDeselect
};
```

**Use Cases:**
- Optional toggles where nothing should be selected
- Filters that can be cleared
- Settings where "no selection" has meaning
- Toggle button-like behavior



### Multiple Selection Mode

Multiple items can be selected simultaneously.

**XAML:**
```xaml
<toolbar:SfToolbar x:Name="toolbar" 
                   HeightRequest="56" 
                   SelectionMode="Multiple"
                   ItemClicked="OnToolbarItemClicked">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Bold" ToolTipText="Bold">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE770;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        <toolbar:SfToolbarItem Name="Italic" ToolTipText="Italic">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE771;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        <toolbar:SfToolbarItem Name="Underline" ToolTipText="Underline">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE762;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        <toolbar:SfToolbarItem Name="Strikethrough" ToolTipText="Strikethrough">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE761;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

**C#:**
```csharp
SfToolbar toolbar = new SfToolbar
{
    HeightRequest = 56,
    SelectionMode = ToolbarSelectionMode.Multiple
};

toolbar.SelectionChanged += OnToolbarSelectionChanged;

private void OnToolbarSelectionChanged(object sender, ToolbarSelectionChangedEventArgs e)
{
    // e.AddedItems contains newly selected items
    var selectedItems = e.AddedItems.Cast<SfToolbarItem>().Select(i => i.Name);
    
    string selectedNames = string.Join(", ", selectedItems);
    if (selectedNames.Length > 0)
    {
        DisplayAlert("Selection", $"Selected items: {selectedNames}", "OK");
    }
}
```

**Use Cases:**
- Text formatting (Bold, Italic, Underline)
- Filter options that can be combined
- Settings that can be enabled independently
- Checkbox-like behavior

## Programmatic Selection

In Syncfusion Toolbar, selection is event-driven. You respond to user interactions through the `SelectionChanged` event and `ToolbarTappedEventArgs`, rather than setting properties directly.

### Handle Selection Events

**C#:**
```csharp
// Handle when items are selected
toolbar.SelectionChanged += OnToolbarSelectionChanged;

private void OnToolbarSelectionChanged(object sender, ToolbarSelectionChangedEventArgs e)
{
    // e.AddedItems - newly selected items
    // e.RemovedItems - newly deselected items
    
    var selectedItem = e.AddedItems.FirstOrDefault() as SfToolbarItem;
    if (selectedItem != null)
    {
        DisplayAlert("Selected", $"Item selected: {selectedItem.Name}", "OK");
    }
}
```

### Track Selected Items

**C# ViewModel Example:**
```csharp
public class ToolbarViewModel : INotifyPropertyChanged
{
    private ObservableCollection<SfToolbarItem> selectedItems = new();
    
    public ObservableCollection<SfToolbarItem> SelectedItems
    {
        get => selectedItems;
        set { selectedItems = value; OnPropertyChanged(); }
    }
    
    public IRelayCommand SelectionChangedCommand { get; }
    
    public ToolbarViewModel()
    {
        SelectionChangedCommand = new RelayCommand<ToolbarSelectionChangedEventArgs>(
            OnSelectionChanged);
    }
    
    private void OnSelectionChanged(ToolbarSelectionChangedEventArgs e)
    {
        // Update observable collection with selected items
        SelectedItems.Clear();
        foreach (var item in e.AddedItems)
        {
            SelectedItems.Add(item as SfToolbarItem);
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged([CallerMemberName] string name = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
    }
}
```

### Clear All Selections

**C#:**
```csharp
// Clear all selections by accessing the toolbar
public void ClearAllSelections()
{
    // Respond to SelectionChanged event with empty selection
    // The UI will update based on selection mode
    // Typically done by handling specific interaction or using SelectionMode.SingleDeselect
}

// For SingleDeselect mode, user can tap selected item again to deselect
// For Multiple mode, you can use ItemTapped event to toggle individual items
```

## Selection State Management

Use the `SelectionChanged` event to track and manage selection state in your application.

### Complete Selection Management Example

**XAML:**
```xaml
<Grid RowDefinitions="Auto,Auto,*">
    <toolbar:SfToolbar x:Name="formattingToolbar" 
                       Grid.Row="0"
                       HeightRequest="56" 
                       SelectionMode="Multiple"
                       SelectionChanged="OnFormattingSelectionChanged">
        <toolbar:SfToolbar.Items>
            <toolbar:SfToolbarItem Name="Bold" ToolTipText="Bold">
                <toolbar:SfToolbarItem.Icon>
                    <FontImageSource Glyph="&#xE770;" FontFamily="MauiMaterialAssets" />
                </toolbar:SfToolbarItem.Icon>
            </toolbar:SfToolbarItem>
            <toolbar:SfToolbarItem Name="Italic" ToolTipText="Italic">
                <toolbar:SfToolbarItem.Icon>
                    <FontImageSource Glyph="&#xE771;" FontFamily="MauiMaterialAssets" />
                </toolbar:SfToolbarItem.Icon>
            </toolbar:SfToolbarItem>
            <toolbar:SfToolbarItem Name="Underline" ToolTipText="Underline">
                <toolbar:SfToolbarItem.Icon>
                    <FontImageSource Glyph="&#xE762;" FontFamily="MauiMaterialAssets" />
                </toolbar:SfToolbarItem.Icon>
            </toolbar:SfToolbarItem>
        </toolbar:SfToolbar.Items>
    </toolbar:SfToolbar>
    
    <HorizontalStackLayout Grid.Row="1" Spacing="10" Margin="10">
        <Button Text="Get Selection" Clicked="OnGetSelection" />
    </HorizontalStackLayout>
    
    <Editor Grid.Row="2" 
            x:Name="textEditor"
            Margin="10" />
</Grid>
```

**C# Code-behind:**
```csharp
public partial class MainPage : ContentPage
{
    private HashSet<string> selectedFormatting = new();
    
    public MainPage()
    {
        InitializeComponent();
    }
    
    private void OnFormattingSelectionChanged(object sender, ToolbarSelectionChangedEventArgs e)
    {
        // Handle newly selected items
        foreach (var item in e.AddedItems)
        {
            var toolbarItem = item as SfToolbarItem;
            if (toolbarItem != null)
            {
                selectedFormatting.Add(toolbarItem.Name);
                ApplyFormatting(toolbarItem.Name, true);
            }
        }
        
        // Handle newly deselected items
        foreach (var item in e.RemovedItems)
        {
            var toolbarItem = item as SfToolbarItem;
            if (toolbarItem != null)
            {
                selectedFormatting.Remove(toolbarItem.Name);
                ApplyFormatting(toolbarItem.Name, false);
            }
        }
    }
    
    private void OnGetSelection(object sender, EventArgs e)
    {
        string message = selectedFormatting.Any() 
            ? $"Selected: {string.Join(", ", selectedFormatting)}" 
            : "No items selected";
        
        DisplayAlert("Selection Status", message, "OK");
    }
    
    private void ApplyFormatting(string formatType, bool isSelected)
    {
        // Apply or remove formatting based on selection state
        switch (formatType)
        {
            case "Bold":
                textEditor.FontAttributes = isSelected 
                    ? textEditor.FontAttributes | FontAttributes.Bold 
                    : textEditor.FontAttributes & ~FontAttributes.Bold;
                break;
            case "Italic":
                textEditor.FontAttributes = isSelected 
                    ? textEditor.FontAttributes | FontAttributes.Italic 
                    : textEditor.FontAttributes & ~FontAttributes.Italic;
                break;
            case "Underline":
                // Handle underline (requires custom implementation)
                break;
        }
    }
}
```

## Tooltips

Display helpful text when users hover over or long-press toolbar items using the `ToolTipText` property.

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Cut" ToolTipText="Cut (Ctrl+X)">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE719;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        <toolbar:SfToolbarItem Name="Copy" ToolTipText="Copy (Ctrl+C)">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE718;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        <toolbar:SfToolbarItem Name="Paste" ToolTipText="Paste (Ctrl+V)">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE71A;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

**C#:**
```csharp
ObservableCollection<BaseToolbarItem> items = new ObservableCollection<BaseToolbarItem>
{
    new SfToolbarItem
    {
        Name = "Cut",
        ToolTipText = "Cut (Ctrl+X)",
        Icon = new FontImageSource { Glyph = "\uE719", FontFamily = "MauiMaterialAssets" }
    },
    new SfToolbarItem
    {
        Name = "Copy",
        ToolTipText = "Copy (Ctrl+C)",
        Icon = new FontImageSource { Glyph = "\uE718", FontFamily = "MauiMaterialAssets" }
    }
};
```

**Tooltip Behavior:**
- **Desktop (Windows/macOS):** Shows on mouse hover
- **Mobile (iOS/Android):** Shows on long press
- **Accessibility:** Read by screen readers

## Item Enabled State

Control whether toolbar items are interactive using the `IsEnabled` property.

**XAML:**
```xaml
<toolbar:SfToolbar x:Name="toolbar" HeightRequest="56">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Undo" 
                               ToolTipText="Undo" 
                               IsEnabled="False">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE744;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        <toolbar:SfToolbarItem Name="Redo" 
                               ToolTipText="Redo" 
                               IsEnabled="False">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE745;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        <toolbar:SeparatorToolbarItem />
        <toolbar:SfToolbarItem Name="Cut" ToolTipText="Cut">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE719;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

**Dynamic Enable/Disable:**
```csharp
private Stack<string> undoStack = new Stack<string>();
private Stack<string> redoStack = new Stack<string>();

private void UpdateUndoRedoState()
{
    var undoItem = toolbar.Items.OfType<SfToolbarItem>()
                          .FirstOrDefault(item => item.Name == "Undo");
    var redoItem = toolbar.Items.OfType<SfToolbarItem>()
                          .FirstOrDefault(item => item.Name == "Redo");
    
    if (undoItem != null)
        undoItem.IsEnabled = undoStack.Count > 0;
    
    if (redoItem != null)
        redoItem.IsEnabled = redoStack.Count > 0;
}

private void OnActionPerformed(string action)
{
    undoStack.Push(action);
    redoStack.Clear();
    UpdateUndoRedoState();
}

private void OnUndo()
{
    if (undoStack.Count > 0)
    {
        var action = undoStack.Pop();
        redoStack.Push(action);
        UpdateUndoRedoState();
    }
}

private void OnRedo()
{
    if (redoStack.Count > 0)
    {
        var action = redoStack.Pop();
        undoStack.Push(action);
        UpdateUndoRedoState();
    }
}
```

## Keyboard Navigation

The toolbar supports keyboard navigation on desktop platforms.

### Keyboard Shortcuts
- **Tab** - Move focus to toolbar
- **Arrow Keys** - Navigate between items
- **Enter/Space** - Activate selected item
- **Escape** - Remove focus from toolbar

## Best Practices

1. **Always provide tooltips** for icon-only items to improve usability
2. **Include keyboard shortcuts** in tooltip text for desktop apps
3. **Choose appropriate selection mode** based on item behavior
4. **Update enabled state dynamically** based on application context
5. **Clear visual feedback** for selected and disabled states
6. **Use Single mode** for mutually exclusive options (like text alignment)
7. **Use SingleDeselect mode** for optional toggles where "nothing selected" is valid
8. **Use Multiple mode** for independent toggles (like text formatting)
9. **Listen to SelectionChanged** events to respond to user selection
10. **Test keyboard navigation** on desktop platforms
11. **Provide alternative access** to toolbar functionality for accessibility

## Common Pitfalls

**Selection not working:**
- Check `SelectionMode` is set to `Single`, `SingleDeselect`, or `Multiple`
- Verify item is enabled (`IsEnabled="True"`)
- Ensure item is clickable (not covered by other elements)
- Confirm `SelectionChanged` event handler is properly attached

**Tooltip not showing:**
- Confirm `ToolTipText` property is set
- On mobile, try long-press instead of tap
- Check platform-specific tooltip behavior

**Selection events not firing:**
- Ensure `SelectionChanged` event is wired up correctly
- Verify `SelectionMode` is not set to a mode that doesn't support selection
- Check that items are not disabled when clicked
- Confirm event handler parameters match `ToolbarSelectionChangedEventArgs`

**Keyboard navigation not working:**
- Desktop-only feature
- Ensure toolbar has focus
- Check platform keyboard handling
- Verify keyboard accelerators are properly registered
