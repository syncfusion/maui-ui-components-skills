# Toolbar Items in .NET MAUI Toolbar

Learn how to configure and customize toolbar items with icons, text, custom views, separators, and alignment options.

## Table of Contents
- [Overview](#overview)
- [Toolbar Item Display Options](#toolbar-item-display-options)
- [Icon Configuration](#icon-configuration)
- [Text Display](#text-display)
- [Icon with Text](#icon-with-text)
- [Item Sizing](#item-sizing)
- [Item Naming](#item-naming)
- [Separator Items](#separator-items)
- [Custom Item Views](#custom-item-views)
- [Item Spacing](#item-spacing)
- [Item Alignment](#item-alignment)
- [Clear Selection](#clear-selection)
- [Best Practices](#best-practices)

## Overview

The SfToolbar control supports various ways to display toolbar items. You can show icons only, text only, or combine both. You can also add custom views like buttons, checkboxes, or entries as toolbar items.

## Toolbar Item Display Options

The toolbar supports three main display options when the `View` property is not defined:
1. **Icon only** - Using the `Icon` property
2. **Text only** - Using the `Text` property  
3. **Icon with Text** - Using both `Icon` and `Text` properties

## Icon Configuration

Add icons to toolbar items using the `Icon` property with `FontImageSource`.

### Basic Icon Example

**XAML:**
```xaml
<toolbar:SfToolbar x:Name="toolbar" HeightRequest="56">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Bold">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE770;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SfToolbarItem Name="Underline">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE762;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SfToolbarItem Name="Italic">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE771;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

**C#:**
```csharp
SfToolbar toolbar = new SfToolbar();
toolbar.HeightRequest = 56;

ObservableCollection<BaseToolbarItem> items = new ObservableCollection<BaseToolbarItem>
{
    new SfToolbarItem
    {
        Name = "Bold",
        Icon = new FontImageSource { Glyph = "\uE770", FontFamily = "MauiMaterialAssets" }
    },
    new SfToolbarItem
    {
        Name = "Underline",
        Icon = new FontImageSource { Glyph = "\uE762", FontFamily = "MauiMaterialAssets" }
    },
    new SfToolbarItem
    {
        Name = "Italic",
        Icon = new FontImageSource { Glyph = "\uE771", FontFamily = "MauiMaterialAssets" }
    }
};

toolbar.Items = items;
```

### Icon Sizing

Control the icon size using the `IconSize` property.

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Bold" IconSize="20">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE770;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SfToolbarItem Name="Italic" IconSize="24">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE771;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SfToolbarItem Name="Underline" IconSize="28">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE762;" FontFamily="MauiMaterialAssets" />
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
        Name = "Bold",
        IconSize = 20,
        Icon = new FontImageSource { Glyph = "\uE770", FontFamily = "MauiMaterialAssets" }
    },
    new SfToolbarItem
    {
        Name = "Italic",
        IconSize = 24,
        Icon = new FontImageSource { Glyph = "\uE771", FontFamily = "MauiMaterialAssets" }
    }
};
```

## Text Display

Display text labels on toolbar items using the `Text` property.

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Bold" Text="Bold" Size="60,40" />
        <toolbar:SfToolbarItem Name="Underline" Text="Underline" Size="90,40" />
        <toolbar:SfToolbarItem Name="Italic" Text="Italic" Size="60,40" />
        <toolbar:SfToolbarItem Name="AlignLeft" Text="Align-Left" Size="100,40" />
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

**C#:**
```csharp
ObservableCollection<BaseToolbarItem> items = new ObservableCollection<BaseToolbarItem>
{
    new SfToolbarItem { Name = "Bold", Text = "Bold", Size = new Size(60, 40) },
    new SfToolbarItem { Name = "Underline", Text = "Underline", Size = new Size(90, 40) },
    new SfToolbarItem { Name = "Italic", Text = "Italic", Size = new Size(60, 40) },
    new SfToolbarItem { Name = "AlignLeft", Text = "Align-Left", Size = new Size(100, 40) }
};
```

## Icon with Text

Combine icons and text for better visual communication. Use the `TextPosition` property to control text placement.

### Text Position Options
- **Right** - Text appears to the right of the icon
- **Left** - Text appears to the left of the icon
- **Top** - Text appears above the icon
- **Bottom** - Text appears below the icon

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Bold" 
                               Text="Bold" 
                               TextPosition="Right" 
                               Size="80,40">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE770;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SfToolbarItem Name="Italic" 
                               Text="Italic" 
                               TextPosition="Right" 
                               Size="80,40">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE771;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SfToolbarItem Name="Underline" 
                               Text="Underline" 
                               TextPosition="Bottom" 
                               Size="70,60">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE762;" FontFamily="MauiMaterialAssets" />
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
        Name = "Bold",
        Text = "Bold",
        TextPosition = ToolbarItemTextPosition.Right,
        Size = new Size(80, 40),
        Icon = new FontImageSource { Glyph = "\uE770", FontFamily = "MauiMaterialAssets" }
    },
    new SfToolbarItem
    {
        Name = "Italic",
        Text = "Italic",
        TextPosition = ToolbarItemTextPosition.Right,
        Size = new Size(80, 40),
        Icon = new FontImageSource { Glyph = "\uE771", FontFamily = "MauiMaterialAssets" }
    },
    new SfToolbarItem
    {
        Name = "Underline",
        Text = "Underline",
        TextPosition = ToolbarItemTextPosition.Bottom,
        Size = new Size(70, 60),
        Icon = new FontImageSource { Glyph = "\uE762", FontFamily = "MauiMaterialAssets" }
    }
};
```

## Item Sizing

Set custom dimensions for toolbar items using the `Size` property.

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Small" Size="40,40">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE770;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SfToolbarItem Name="Medium" Size="60,40">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE771;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SfToolbarItem Name="Large" Size="80,40">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE762;" FontFamily="MauiMaterialAssets" />
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
        Name = "Small", 
        Size = new Size(40, 40),
        Icon = new FontImageSource { Glyph = "\uE770", FontFamily = "MauiMaterialAssets" }
    },
    new SfToolbarItem 
    { 
        Name = "Medium", 
        Size = new Size(60, 40),
        Icon = new FontImageSource { Glyph = "\uE771", FontFamily = "MauiMaterialAssets" }
    },
    new SfToolbarItem 
    { 
        Name = "Large", 
        Size = new Size(80, 40),
        Icon = new FontImageSource { Glyph = "\uE762", FontFamily = "MauiMaterialAssets" }
    }
};
```

## Item Naming

Set unique names for toolbar items using the `Name` property. This is essential for:
- Identifying items in event handlers
- Programmatically accessing specific items
- Debugging and logging

**XAML:**
```xaml
<toolbar:SfToolbar>
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="BoldButton">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE770;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SfToolbarItem Name="ItalicButton">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE771;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

## Separator Items

Add visual dividers between toolbar items using `SeparatorToolbarItem`.

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56">
    <toolbar:SfToolbar.Items>
        <!-- Formatting group -->
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
        
        <!-- Separator -->
        <toolbar:SeparatorToolbarItem />
        
        <!-- Alignment group -->
        <toolbar:SfToolbarItem Name="AlignLeft" ToolTipText="Align Left">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE751;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SfToolbarItem Name="AlignCenter" ToolTipText="Align Center">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE752;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

**C#:**
```csharp
ObservableCollection<BaseToolbarItem> items = new ObservableCollection<BaseToolbarItem>
{
    // Formatting group
    new SfToolbarItem 
    { 
        Name = "Bold", 
        ToolTipText = "Bold",
        Icon = new FontImageSource { Glyph = "\uE770", FontFamily = "MauiMaterialAssets" }
    },
    new SfToolbarItem 
    { 
        Name = "Italic", 
        ToolTipText = "Italic",
        Icon = new FontImageSource { Glyph = "\uE771", FontFamily = "MauiMaterialAssets" }
    },
    
    // Separator
    new SeparatorToolbarItem(),
    
    // Alignment group
    new SfToolbarItem 
    { 
        Name = "AlignLeft", 
        ToolTipText = "Align Left",
        Icon = new FontImageSource { Glyph = "\uE751", FontFamily = "MauiMaterialAssets" }
    }
};
```

## Custom Item Views

Add any .NET MAUI view as a toolbar item using the `View` property.

### Custom Button Example

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="CustomButton">
            <toolbar:SfToolbarItem.View>
                <Button Text="Action" 
                        BackgroundColor="Blue" 
                        TextColor="White"
                        WidthRequest="80"
                        HeightRequest="36"
                        CornerRadius="5"
                        Clicked="OnActionClicked" />
            </toolbar:SfToolbarItem.View>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

### Custom Entry Example

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="SearchBox">
            <toolbar:SfToolbarItem.View>
                <Entry Placeholder="Search..." 
                       WidthRequest="200"
                       BackgroundColor="White"
                       Margin="5" />
            </toolbar:SfToolbarItem.View>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

### Custom Picker Example

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="FontPicker">
            <toolbar:SfToolbarItem.View>
                <Picker WidthRequest="150" SelectedIndex="0">
                    <Picker.Items>
                        <x:String>Arial</x:String>
                        <x:String>Times New Roman</x:String>
                        <x:String>Courier New</x:String>
                        <x:String>Verdana</x:String>
                    </Picker.Items>
                </Picker>
            </toolbar:SfToolbarItem.View>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

### Complex Custom View Example

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="60">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="ColorSelector">
            <toolbar:SfToolbarItem.View>
                <HorizontalStackLayout Spacing="5" Margin="5">
                    <Label Text="Color:" VerticalOptions="Center" />
                    <BoxView Color="Red" WidthRequest="30" HeightRequest="30" CornerRadius="15">
                        <BoxView.GestureRecognizers>
                            <TapGestureRecognizer Tapped="OnColorSelected" />
                        </BoxView.GestureRecognizers>
                    </BoxView>
                    <BoxView Color="Blue" WidthRequest="30" HeightRequest="30" CornerRadius="15">
                        <BoxView.GestureRecognizers>
                            <TapGestureRecognizer Tapped="OnColorSelected" />
                        </BoxView.GestureRecognizers>
                    </BoxView>
                    <BoxView Color="Green" WidthRequest="30" HeightRequest="30" CornerRadius="15">
                        <BoxView.GestureRecognizers>
                            <TapGestureRecognizer Tapped="OnColorSelected" />
                        </BoxView.GestureRecognizers>
                    </BoxView>
                </HorizontalStackLayout>
            </toolbar:SfToolbarItem.View>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

## Item Spacing

Control spacing between toolbar items using the `ItemSpacing` property.

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56" ItemSpacing="10">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Bold">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE770;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SfToolbarItem Name="Italic">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE771;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

### Equal Spacing

Set `ItemSpacing` to `-1` to distribute items evenly across the toolbar.

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56" ItemSpacing="-1">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Item1">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE770;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SfToolbarItem Name="Item2">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE771;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SfToolbarItem Name="Item3">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE762;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

**Note:** Equal spacing works only when the toolbar is not in scrollable mode and has no leading or trailing items.

## Item Alignment

Position toolbar items at the start, center, or end of the toolbar using the `Alignment` property.

### Alignment Options
- **Start** - Items aligned to the leading edge (left in LTR, right in RTL)
- **Center** - Items aligned to the center (default)
- **End** - Items aligned to the trailing edge (right in LTR, left in RTL)

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56">
    <toolbar:SfToolbar.Items>
        <!-- Leading items -->
        <toolbar:SfToolbarItem Name="Bold" 
                               Alignment="Start"
                               Text="Bold" 
                               TextPosition="Right" 
                               Size="70,40">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE770;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SfToolbarItem Name="Italic" 
                               Alignment="Start"
                               Text="Italic" 
                               TextPosition="Right" 
                               Size="70,40">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE771;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SeparatorToolbarItem Alignment="Start" />
        
        <!-- Center items -->
        <toolbar:SfToolbarItem Name="Undo" 
                               Alignment="Center"
                               ToolTipText="Undo">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE744;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SfToolbarItem Name="Redo" 
                               Alignment="Center"
                               ToolTipText="Redo">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE745;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SeparatorToolbarItem Alignment="End" />
        
        <!-- Trailing items -->
        <toolbar:SfToolbarItem Name="Settings" 
                               Alignment="End"
                               ToolTipText="Settings">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE713;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

**Note:** Item alignment is only applicable when the toolbar is not in scrollable mode.

## Clear Selection

Clear the selected toolbar item programmatically using the `ClearSelection()` method.

**XAML:**
```xaml
<Grid RowDefinitions="Auto,*">
    <toolbar:SfToolbar x:Name="toolbar" 
                       Grid.Row="0"
                       HeightRequest="56"
                       SelectionMode="Single">
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
    
    <Button Grid.Row="1" 
            Text="Clear Selection" 
            Clicked="OnClearSelectionClicked"
            Margin="10" />
</Grid>
```

**C# Code-behind:**
```csharp
private void OnClearSelectionClicked(object sender, EventArgs e)
{
    toolbar.ClearSelection();
}
```

## Best Practices

1. **Always provide tooltips** for icon-only items to improve accessibility
2. **Use consistent sizing** across similar items for a professional look
3. **Group related items** using separator items
4. **Name items descriptively** for easier identification in event handlers
5. **Consider text position** carefully when combining icons and text
6. **Test with different screen sizes** to ensure items fit properly
7. **Use equal spacing** (`ItemSpacing="-1"`) for evenly distributed layouts
8. **Avoid overcrowding** - use overflow modes for many items
9. **Keep custom views simple** to maintain toolbar performance
10. **Test alignment** in both LTR and RTL layouts

## Common Pitfalls

**Item not clickable:**
- Check if `IsEnabled` is set to `true`
- Ensure custom views don't have `InputTransparent="True"`
- Verify no overlapping elements blocking touch

**Icons not showing:**
- Confirm font is properly registered
- Check `FontFamily` name matches registration
- Verify glyph codes are correct

**Text truncated:**
- Set appropriate `Size` property for the item
- Consider using text wrapping or shorter labels
- Test on different screen sizes

**Spacing inconsistent:**
- Set explicit `ItemSpacing` value
- Check if items have varying sizes
- Verify alignment settings are consistent
