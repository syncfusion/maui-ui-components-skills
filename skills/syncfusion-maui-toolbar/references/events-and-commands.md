# Events and Commands in .NET MAUI Toolbar

Learn how to handle toolbar events and implement MVVM pattern with commands for responsive, interactive toolbars.

## Table of Contents
- [Overview](#overview)
- [Tapped Event](#tapped-event)
- [ItemTouchInteraction Event](#itemtouchinteraction-event)
- [ItemLongPressed Event](#itemlongpressed-event)
- [SelectionChanged Event](#selectionchanged-event)
- [MoreItemsChanged Event](#moreitemschanged-event)
- [MoreButtonTapped Event](#morebuttontapped-event)
- [MVVM Pattern with Commands](#mvvm-pattern-with-commands)
- [Event Arguments](#event-arguments)
- [Best Practices](#best-practices)

## Overview

The SfToolbar provides six events for handling user interactions:
1. **Tapped** - When a toolbar item is tapped
2. **ItemTouchInteraction** - Touch or pointer actions on toolbar items
3. **ItemLongPressed** - Long press/hold on an item
4. **SelectionChanged** - When toolbar item selection changes
5. **MoreItemsChanged** - When more button items change
6. **MoreButtonTapped** - When the more button is tapped

Each event provides specific event arguments with information about the interaction.

## Tapped Event

The `Tapped` event occurs each time a toolbar item is tapped. It provides both the newly tapped item and the previously tapped item.

### Basic Usage

**XAML:**
```xaml
<toolbar:SfToolbar x:Name="toolbar" 
                   HeightRequest="56"
                   Tapped="OnToolbarTapped">
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
```

**C# Code-behind:**
```csharp
private void OnToolbarTapped(object sender, ToolbarTappedEventArgs e)
{
    var newItem = e.NewToolbarItem as SfToolbarItem;
    var oldItem = e.PreviousToolbarItem as SfToolbarItem;
    
    if (newItem != null)
    {
        DisplayAlert("Item Tapped", $"You tapped: {newItem.Name}", "OK");
    }
}
```

### Advanced Tapped Handling

**C#:**
```csharp
private void OnToolbarTapped(object sender, ToolbarTappedEventArgs e)
{
    var newItem = e.NewToolbarItem as SfToolbarItem;
    var oldItem = e.PreviousToolbarItem as SfToolbarItem;
    
    if (newItem == null) return;
    
    switch (newItem.Name)
    {
        case "Bold":
            ToggleBoldFormatting();
            break;
        case "Italic":
            ToggleItalicFormatting();
            break;
        case "Underline":
            ToggleUnderlineFormatting();
            break;
        case "Cut":
            PerformCut();
            break;
        case "Copy":
            PerformCopy();
            break;
        case "Paste":
            PerformPaste();
            break;
        default:
            HandleUnknownItem(newItem.Name);
            break;
    }
}

private void ToggleBoldFormatting()
{
    var editor = this.FindByName<Editor>("textEditor");
    if (editor != null)
    {
        editor.FontAttributes = editor.FontAttributes.HasFlag(FontAttributes.Bold)
            ? editor.FontAttributes & ~FontAttributes.Bold
            : editor.FontAttributes | FontAttributes.Bold;
    }
}

private async void PerformCut()
{
    var editor = this.FindByName<Editor>("textEditor");
    if (editor != null && !string.IsNullOrEmpty(editor.Text))
    {
        await Clipboard.SetTextAsync(editor.Text);
        editor.Text = string.Empty;
    }
}
```

### Tapped Event with Previous Item Tracking

**XAML:**
```xaml
<Grid RowDefinitions="Auto,Auto,*">
    <toolbar:SfToolbar x:Name="toolbar" 
                       Grid.Row="0"
                       HeightRequest="56"
                       Tapped="OnToolbarTapped">
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
    
    <Label x:Name="statusLabel" 
           Grid.Row="1"
           Text="No item tapped" 
           Margin="10" />
    
    <Editor x:Name="textEditor" 
            Grid.Row="2"
            Margin="10" />
</Grid>
```

**C#:**
```csharp
private void OnToolbarTapped(object sender, ToolbarTappedEventArgs e)
{
    var newItem = e.NewToolbarItem as SfToolbarItem;
    var oldItem = e.PreviousToolbarItem as SfToolbarItem;
    
    string message = $"Tapped: {newItem?.Name ?? "None"}";
    if (oldItem != null)
    {
        message += $" (Previous: {oldItem.Name})";
    }
    
    statusLabel.Text = message;
}
```

## ItemTouchInteraction Event

The `ItemTouchInteraction` event occurs when the toolbar item is touched or receives a pointer action. This event provides detailed information about the touch/pointer interaction type.

### Pointer Actions

The event provides a `PointerActions` property with the following values:
- **Pressed** - Item is being pressed
- **Released** - Item press is released
- **Entered** - Pointer entered the item area
- **Exited** - Pointer exited the item area
- **Moved** - Pointer moved within the item

**XAML:**
```xaml
<toolbar:SfToolbar x:Name="toolbar" 
                   HeightRequest="56"
                   ItemTouchInteraction="OnItemTouchInteraction">
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
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

**C# Code-behind:**
```csharp
private void OnItemTouchInteraction(object sender, ToolbarItemTouchInteractionEventArgs e)
{
    var item = e.ToolbarItem as SfToolbarItem;
    var action = e.PointerActions;
    
    if (item != null)
    {
        Debug.WriteLine($"Item: {item.Name}, Action: {action}");
        
        switch (action)
        {
            case ToolbarItemPointerActions.Pressed:
                HandleItemPressed(item);
                break;
            case ToolbarItemPointerActions.Released:
                HandleItemReleased(item);
                break;
            case ToolbarItemPointerActions.Entered:
                ShowItemHighlight(item);
                break;
            case ToolbarItemPointerActions.Exited:
                HideItemHighlight(item);
                break;
        }
    }
}

private void HandleItemPressed(SfToolbarItem item)
{
    // Handle press action
}

private void HandleItemReleased(SfToolbarItem item)
{
    // Handle release action
}

private void ShowItemHighlight(SfToolbarItem item)
{
    // Show hover effect
}

private void HideItemHighlight(SfToolbarItem item)
{
    // Hide hover effect
}
```

**Use Cases:**
- Custom hover effects
- Tracking press/release cycles
- Advanced touch gesture handling
- Custom visual feedback on pointer actions

## ItemLongPressed Event

Triggered when a user long-presses or holds a toolbar item.

**XAML:**
```xaml
<toolbar:SfToolbar x:Name="toolbar" 
                   HeightRequest="56"
                   ItemLongPressed="OnToolbarItemLongPressed">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="ColorPicker" ToolTipText="Text Color">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE790;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        <toolbar:SfToolbarItem Name="FontSize" ToolTipText="Font Size">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE8F4;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

**C# Code-behind:**
```csharp
private async void OnToolbarItemLongPressed(object sender, ToolbarItemLongPressedEventArgs e)
{
    var item = e.Item as SfToolbarItem;
    
    if (item == null) return;
    
    switch (item.Name)
    {
        case "ColorPicker":
            await ShowColorPickerMenu(item);
            break;
        case "FontSize":
            await ShowFontSizeMenu(item);
            break;
    }
}

private async Task ShowColorPickerMenu(SfToolbarItem item)
{
    string action = await DisplayActionSheet(
        "Select Color",
        "Cancel",
        null,
        "Red", "Blue", "Green", "Black", "White"
    );
    
    if (action != "Cancel" && action != null)
    {
        ApplyTextColor(action);
    }
}

private async Task ShowFontSizeMenu(SfToolbarItem item)
{
    string action = await DisplayActionSheet(
        "Select Font Size",
        "Cancel",
        null,
        "8pt", "10pt", "12pt", "14pt", "16pt", "18pt", "20pt"
    );
    
    if (action != "Cancel" && action != null)
    {
        ApplyFontSize(action);
    }
}

private void ApplyTextColor(string color)
{
    var editor = FindControl<Editor>("textEditor");
    if (editor != null)
    {
        editor.TextColor = Color.FromArgb(color);
    }
}

private void ApplyFontSize(string size)
{
    var editor = FindControl<Editor>("textEditor");
    if (editor != null)
    {
        double fontSize = double.Parse(size.Replace("pt", ""));
        editor.FontSize = fontSize;
    }
}
```

**Use Cases:**
- Context menus
- Advanced options
- Alternative actions
- Mobile-friendly expanded controls

## SelectionChanged Event

The `SelectionChanged` event occurs each time toolbar item selection changes. It provides both the newly selected items and the previously selected items.

**XAML:**
```xaml
<toolbar:SfToolbar x:Name="toolbar" 
                   HeightRequest="56"
                   SelectionMode="Multiple"
                   SelectionChanged="OnSelectionChanged">
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
```

**C# Code-behind:**
```csharp
private void OnSelectionChanged(object sender, ToolbarSelectionChangedEventArgs e)
{
    var newItems = e.NewToolbarItems;
    var oldItems = e.OldToolbarItems;
    
    Debug.WriteLine($"Selection changed:");
    Debug.WriteLine($"  New items: {string.Join(", ", newItems.Cast<SfToolbarItem>().Select(i => i.Name))}");
    Debug.WriteLine($"  Old items: {string.Join(", ", oldItems.Cast<SfToolbarItem>().Select(i => i.Name))}");
    
    // Apply formatting based on selected items
    ApplyFormatting(newItems);
}

private void ApplyFormatting(IList<BaseToolbarItem> selectedItems)
{
    var editor = this.FindByName<Editor>("textEditor");
    if (editor == null) return;
    
    FontAttributes attributes = FontAttributes.None;
    
    foreach (var item in selectedItems.OfType<SfToolbarItem>())
    {
        if (item.Name == "Bold")
            attributes |= FontAttributes.Bold;
        if (item.Name == "Italic")
            attributes |= FontAttributes.Italic;
    }
    
    editor.FontAttributes = attributes;
}
```

**Use Cases:**
- Tracking text formatting state
- Synchronizing UI with selection
- Implementing undo/redo logic
- Multi-select validation

## MoreItemsChanged Event

The `MoreItemsChanged` event is invoked when more button items change (when overflow items change).

**XAML:**
```xaml
<toolbar:SfToolbar x:Name="toolbar" 
                   HeightRequest="56"
                   WidthRequest="200"
                   OverflowMode="MoreButton"
                   MoreItemsChanged="OnMoreItemsChanged">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Item1" Text="Item 1" />
        <toolbar:SfToolbarItem Name="Item2" Text="Item 2" />
        <toolbar:SfToolbarItem Name="Item3" Text="Item 3" />
        <toolbar:SfToolbarItem Name="Item4" Text="Item 4" />
        <toolbar:SfToolbarItem Name="Item5" Text="Item 5" />
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

**C# Code-behind:**
```csharp
private void OnMoreItemsChanged(object sender, ToolbarMoreItemsChangedEventArgs e)
{
    var moreItems = e.ToolbarItems;
    
    Debug.WriteLine($"More items changed: {moreItems.Count} items in overflow");
    
    foreach (var item in moreItems.OfType<SfToolbarItem>())
    {
        Debug.WriteLine($"  - {item.Name}");
    }
}
```

**Use Cases:**
- Tracking overflow items for analytics
- Adjusting UI based on available space
- Logging user experience with overflow
- Dynamic toolbar optimization

## MoreButtonTapped Event

The `MoreButtonTapped` event is invoked when the more button is tapped (in MoreButton overflow mode).

**XAML:**
```xaml
<toolbar:SfToolbar x:Name="toolbar" 
                   HeightRequest="56"
                   WidthRequest="200"
                   OverflowMode="MoreButton"
                   MoreButtonTapped="OnMoreButtonTapped">
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
        <toolbar:SfToolbarItem Name="Bold" ToolTipText="Bold">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE770;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

**C# Code-behind:**
```csharp
private void OnMoreButtonTapped(object sender, ToolbarMoreButtonTappedEventArgs e)
{
    var moreItems = e.ToolbarItems;
    
    Debug.WriteLine($"More button tapped! {moreItems.Count} items available in menu");
    
    // You can perform analytics or custom actions when user accesses overflow menu
    LogOverflowAccess(moreItems);
}

private void LogOverflowAccess(IList<BaseToolbarItem> items)
{
    // Log analytics event
    Debug.WriteLine($"User accessed overflow menu with {items.Count} items");
}
```

**Use Cases:**
- Analytics tracking
- Custom overflow menu behavior
- User experience monitoring
- Help/tutorial triggers

## MVVM Pattern with Commands

Implement toolbar interactions using commands for clean separation of concerns. The toolbar supports commands for all six events:
- **TappedCommand**
- **ItemTouchInteractionCommand**
- **ItemLongPressedCommand**
- **SelectionChangedCommand**
- **MoreItemsChangedCommand**
- **MoreButtonTappedCommand**

### ViewModel with Commands

**ViewModel:**
```csharp
using System.Collections.ObjectModel;
using System.Windows.Input;
using Microsoft.Maui.Controls;
using Syncfusion.Maui.Toolbar;

public class ToolbarViewModel : BindableObject
{
    private bool isBold;
    private bool isItalic;
    private bool isUnderline;
    
    public ObservableCollection<BaseToolbarItem> ToolbarItems { get; set; }
    
    public ICommand TappedCommand { get; }
    public ICommand SelectionChangedCommand { get; }
    public ICommand ItemLongPressedCommand { get; }
    public ICommand ItemTouchInteractionCommand { get; }
    
    public ToolbarViewModel()
    {
        TappedCommand = new Command<ToolbarTappedEventArgs>(OnToolbarTapped);
        SelectionChangedCommand = new Command<ToolbarSelectionChangedEventArgs>(OnSelectionChanged);
        ItemLongPressedCommand = new Command<ToolbarItemLongPressedEventArgs>(OnItemLongPressed);
        ItemTouchInteractionCommand = new Command<ToolbarItemTouchInteractionEventArgs>(OnItemTouchInteraction);
        
        InitializeToolbarItems();
    }
    
    private void InitializeToolbarItems()
    {
        ToolbarItems = new ObservableCollection<BaseToolbarItem>
        {
            new SfToolbarItem
            {
                Name = "Bold",
                ToolTipText = "Bold (Ctrl+B)",
                Icon = new FontImageSource { Glyph = "\uE770", FontFamily = "MauiMaterialAssets" }
            },
            new SfToolbarItem
            {
                Name = "Italic",
                ToolTipText = "Italic (Ctrl+I)",
                Icon = new FontImageSource { Glyph = "\uE771", FontFamily = "MauiMaterialAssets" }
            },
            new SfToolbarItem
            {
                Name = "Underline",
                ToolTipText = "Underline (Ctrl+U)",
                Icon = new FontImageSource { Glyph = "\uE762", FontFamily = "MauiMaterialAssets" }
            },
            new SeparatorToolbarItem(),
            new SfToolbarItem
            {
                Name = "Cut",
                ToolTipText = "Cut (Ctrl+X)",
                Icon = new FontImageSource { Glyph = "\uE719", FontFamily = "MauiMaterialAssets" }
            }
        };
    }
    
    private void OnToolbarTapped(ToolbarTappedEventArgs e)
    {
        var newItem = e.NewToolbarItem as SfToolbarItem;
        var oldItem = e.PreviousToolbarItem as SfToolbarItem;
        
        if (newItem == null) return;
        
        switch (newItem.Name)
        {
            case "Bold":
                ToggleBold();
                break;
            case "Italic":
                ToggleItalic();
                break;
            case "Underline":
                ToggleUnderline();
                break;
            case "Cut":
                PerformCut();
                break;
        }
    }
    
    private void OnSelectionChanged(ToolbarSelectionChangedEventArgs e)
    {
        var newItems = e.NewToolbarItems;
        var oldItems = e.OldToolbarItems;
        
        // Update formatting based on selection
        UpdateFormattingState(newItems);
    }
    
    private void OnItemLongPressed(ToolbarItemLongPressedEventArgs e)
    {
        var item = e.ToolbarItem as SfToolbarItem;
        
        if (item != null)
        {
            // Show context menu or options
            Debug.WriteLine($"Long pressed: {item.Name}");
        }
    }
    
    private void OnItemTouchInteraction(ToolbarItemTouchInteractionEventArgs e)
    {
        var item = e.ToolbarItem as SfToolbarItem;
        var action = e.PointerActions;
        
        Debug.WriteLine($"Touch interaction - Item: {item?.Name}, Action: {action}");
    }
    
    private void ToggleBold()
    {
        isBold = !isBold;
        OnPropertyChanged(nameof(FontAttributes));
    }
    
    private void ToggleItalic()
    {
        isItalic = !isItalic;
        OnPropertyChanged(nameof(FontAttributes));
    }
    
    private void ToggleUnderline()
    {
        isUnderline = !isUnderline;
        // Handle underline
    }
    
    private void PerformCut()
    {
        // Cut logic
    }
    
    private void UpdateFormattingState(IList<BaseToolbarItem> selectedItems)
    {
        isBold = selectedItems.OfType<SfToolbarItem>().Any(i => i.Name == "Bold");
        isItalic = selectedItems.OfType<SfToolbarItem>().Any(i => i.Name == "Italic");
        isUnderline = selectedItems.OfType<SfToolbarItem>().Any(i => i.Name == "Underline");
        
        OnPropertyChanged(nameof(FontAttributes));
    }
    
    public FontAttributes FontAttributes
    {
        get
        {
            FontAttributes attributes = FontAttributes.None;
            if (isBold) attributes |= FontAttributes.Bold;
            if (isItalic) attributes |= FontAttributes.Italic;
            return attributes;
        }
    }
}
```

### XAML View with Command Binding

**XAML:**
```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:toolbar="clr-namespace:Syncfusion.Maui.Toolbar;assembly=Syncfusion.Maui.Toolbar"
             xmlns:local="clr-namespace:YourNamespace"
             x:Class="YourNamespace.MainPage">
    
    <ContentPage.BindingContext>
        <local:ToolbarViewModel />
    </ContentPage.BindingContext>
    
    <Grid RowDefinitions="Auto,*">
        <toolbar:SfToolbar Grid.Row="0" 
                           HeightRequest="56"
                           Items="{Binding ToolbarItems}"
                           SelectionMode="Multiple"
                           TappedCommand="{Binding TappedCommand}"
                           SelectionChangedCommand="{Binding SelectionChangedCommand}"
                           ItemLongPressedCommand="{Binding ItemLongPressedCommand}"
                           ItemTouchInteractionCommand="{Binding ItemTouchInteractionCommand}" />
        
        <Editor Grid.Row="1" 
                x:Name="textEditor"
                Text="Sample text"
                FontAttributes="{Binding FontAttributes}"
                Margin="10" />
    </Grid>
</ContentPage>
```

**Code-behind (optional):**
```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }
}
```

### All Commands Example

**Complete ViewModel with All Commands:**
```csharp
public class CompleteToolbarViewModel : BindableObject
{
    public ICommand TappedCommand { get; }
    public ICommand ItemTouchInteractionCommand { get; }
    public ICommand ItemLongPressedCommand { get; }
    public ICommand SelectionChangedCommand { get; }
    public ICommand MoreItemsChangedCommand { get; }
    public ICommand MoreButtonTappedCommand { get; }
    
    public CompleteToolbarViewModel()
    {
        TappedCommand = new Command<ToolbarTappedEventArgs>(ExecuteTapped);
        ItemTouchInteractionCommand = new Command<ToolbarItemTouchInteractionEventArgs>(ExecuteTouch);
        ItemLongPressedCommand = new Command<ToolbarItemLongPressedEventArgs>(ExecuteLongPress);
        SelectionChangedCommand = new Command<ToolbarSelectionChangedEventArgs>(ExecuteSelectionChanged);
        MoreItemsChangedCommand = new Command<ToolbarMoreItemsChangedEventArgs>(ExecuteMoreItemsChanged);
        MoreButtonTappedCommand = new Command<ToolbarMoreButtonTappedEventArgs>(ExecuteMoreButtonTapped);
    }
    
    private void ExecuteTapped(ToolbarTappedEventArgs args)
    {
        var newItem = args.NewToolbarItem;
        var oldItem = args.PreviousToolbarItem;
        // Handle tap
    }
    
    private void ExecuteTouch(ToolbarItemTouchInteractionEventArgs args)
    {
        var item = args.ToolbarItem;
        var action = args.PointerActions;
        // Handle touch interaction
    }
    
    private void ExecuteLongPress(ToolbarItemLongPressedEventArgs args)
    {
        var item = args.ToolbarItem;
        // Handle long press
    }
    
    private void ExecuteSelectionChanged(ToolbarSelectionChangedEventArgs args)
    {
        var newItems = args.NewToolbarItems;
        var oldItems = args.OldToolbarItems;
        // Handle selection change
    }
    
    private void ExecuteMoreItemsChanged(ToolbarMoreItemsChangedEventArgs args)
    {
        var items = args.ToolbarItems;
        // Handle more items changed
    }
    
    private void ExecuteMoreButtonTapped(ToolbarMoreButtonTappedEventArgs args)
    {
        var items = args.ToolbarItems;
        // Handle more button tapped
    }
}
```

## Event Arguments

Each toolbar event provides specific event arguments with detailed information about the interaction.

### ToolbarTappedEventArgs

**Properties:**
- `NewToolbarItem` - The toolbar item that was tapped
- `PreviousToolbarItem` - The previously tapped toolbar item

**Usage:**
```csharp
private void OnToolbarTapped(object sender, ToolbarTappedEventArgs e)
{
    var newItem = e.NewToolbarItem as SfToolbarItem;
    var oldItem = e.PreviousToolbarItem as SfToolbarItem;
    
    Debug.WriteLine($"Tapped: {newItem?.Name}, Previous: {oldItem?.Name}");
}
```

### ToolbarItemTouchInteractionEventArgs

**Properties:**
- `ToolbarItem` - The toolbar item that received the touch interaction
- `PointerActions` - The type of pointer action (Pressed, Released, Entered, Exited, Moved)

**Usage:**
```csharp
private void OnItemTouchInteraction(object sender, ToolbarItemTouchInteractionEventArgs e)
{
    var item = e.ToolbarItem as SfToolbarItem;
    var action = e.PointerActions;
    
    Debug.WriteLine($"Item: {item?.Name}, Action: {action}");
}
```

### ToolbarItemLongPressedEventArgs

**Properties:**
- `ToolbarItem` - The toolbar item that was long pressed

**Usage:**
```csharp
private void OnItemLongPressed(object sender, ToolbarItemLongPressedEventArgs e)
{
    var item = e.ToolbarItem as SfToolbarItem;
    
    Debug.WriteLine($"Long pressed: {item?.Name}");
}
```

### ToolbarSelectionChangedEventArgs

**Properties:**
- `NewToolbarItems` - List of newly selected toolbar items
- `OldToolbarItems` - List of previously selected toolbar items

**Usage:**
```csharp
private void OnSelectionChanged(object sender, ToolbarSelectionChangedEventArgs e)
{
    var newItems = e.NewToolbarItems.Cast<SfToolbarItem>();
    var oldItems = e.OldToolbarItems.Cast<SfToolbarItem>();
    
    Debug.WriteLine($"New: {string.Join(", ", newItems.Select(i => i.Name))}");
    Debug.WriteLine($"Old: {string.Join(", ", oldItems.Select(i => i.Name))}");
}
```

### ToolbarMoreItemsChangedEventArgs

**Properties:**
- `ToolbarItems` - List of toolbar items in the more menu/overflow

**Usage:**
```csharp
private void OnMoreItemsChanged(object sender, ToolbarMoreItemsChangedEventArgs e)
{
    var items = e.ToolbarItems.Cast<SfToolbarItem>();
    
    Debug.WriteLine($"Overflow items: {string.Join(", ", items.Select(i => i.Name))}");
}
```

### ToolbarMoreButtonTappedEventArgs

**Properties:**
- `ToolbarItems` - List of toolbar items available in the more menu

**Usage:**
```csharp
private void OnMoreButtonTapped(object sender, ToolbarMoreButtonTappedEventArgs e)
{
    var items = e.ToolbarItems.Cast<SfToolbarItem>();
    
    Debug.WriteLine($"More button tapped with {items.Count()} items");
}
```

## Best Practices

1. **Use `Tapped` event** for primary item interactions (most common use case)
2. **Implement MVVM pattern** with commands for testable, maintainable code
3. **Combine `SelectionChanged` with SelectionMode** for toggle button tracking
4. **Use `ItemLongPressed`** for context menus and alternative actions
5. **Use `ItemTouchInteraction`** for custom hover effects and touch feedback
6. **Check item type** before casting (use `as SfToolbarItem` pattern)
7. **Handle null items** gracefully in event handlers
8. **Keep event handlers lightweight** - offload heavy work to async methods
9. **Use commands over events** when implementing MVVM pattern
10. **Test events on all target platforms** (behavior may vary slightly)
11. **Use `MoreItemsChanged`** to track overflow behavior for UX optimization
12. **Leverage `PointerActions`** enum values for precise touch handling

## Common Pitfalls

**Event not firing:**
- Check item `IsEnabled="True"` property
- Verify event handler name matches XAML attribute
- Ensure toolbar has items and they're interactive
- Check if custom views are intercepting touch events

**Null reference exceptions:**
- Always check if `e.NewToolbarItem` or `e.ToolbarItem` is null
- Cast safely using `as` operator, not direct cast
- Verify items exist before accessing properties

**Command not executing:**
- Confirm command property name matches event command (e.g., `TappedCommand`)
- Verify `BindingContext` is set correctly
- Check command parameter type matches event args type
- Ensure CanExecute returns true (if implemented)

**SelectionChanged not firing:**
- Verify `SelectionMode` is set to Single or Multiple (not None)
- Confirm items are selectable
- Check if items have `IsEnabled="True"`

**Wrong event args type in commands:**
- Use correct event args type for each command:
  - `TappedCommand` → `ToolbarTappedEventArgs`
  - `ItemTouchInteractionCommand` → `ToolbarItemTouchInteractionEventArgs`
  - `ItemLongPressedCommand` → `ToolbarItemLongPressedEventArgs`
  - `SelectionChangedCommand` → `ToolbarSelectionChangedEventArgs`
  - `MoreItemsChangedCommand` → `ToolbarMoreItemsChangedEventArgs`
  - `MoreButtonTappedCommand` → `ToolbarMoreButtonTappedEventArgs`

**Memory leaks:**
- Unsubscribe from events in page cleanup/disposal
- Avoid capturing large objects in event handler closures
- Use weak event patterns for long-lived subscriptions

**Platform-specific behavior:**
- `ItemTouchInteraction` may behave differently on touch vs. mouse platforms
- Test hover effects on desktop and long-press on mobile
- Pointer actions may not all fire on all platforms
