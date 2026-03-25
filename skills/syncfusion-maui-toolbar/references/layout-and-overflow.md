# Layout and Overflow in .NET MAUI Toolbar

Learn how to control toolbar orientation, manage overflow behavior, and handle multiple rows of toolbar items.

## Table of Contents
- [Overview](#overview)
- [Toolbar Orientation](#toolbar-orientation)
- [Overflow Modes](#overflow-modes)
- [Scroll Mode](#scroll-mode)
- [Navigation Button Mode](#navigation-button-mode)
- [More Button Mode](#more-button-mode)
- [Multi-Row Toolbar](#multi-row-toolbar)
- [Extended Row Support](#extended-row-support)
- [Best Practices](#best-practices)

## Overview

The SfToolbar provides flexible layout options to handle different screen sizes and item counts. You can control orientation (horizontal or vertical), manage overflow with different modes (scrolling, navigation buttons, or more button), and arrange items across multiple rows.

## Toolbar Orientation

Set the toolbar orientation using the `Orientation` property.

### Horizontal Toolbar (Default)

**XAML:**
```xaml
<toolbar:SfToolbar x:Name="toolbar" 
                   Orientation="Horizontal" 
                   HeightRequest="56">
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

**C#:**
```csharp
SfToolbar toolbar = new SfToolbar
{
    Orientation = ToolbarOrientation.Horizontal,
    HeightRequest = 56
};
```

### Vertical Toolbar

**XAML:**
```xaml
<toolbar:SfToolbar x:Name="toolbar" 
                   Orientation="Vertical" 
                   WidthRequest="70">
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

**C#:**
```csharp
SfToolbar toolbar = new SfToolbar
{
    Orientation = ToolbarOrientation.Vertical,
    WidthRequest = 70
};
```

**Key Difference:** Horizontal toolbars use `HeightRequest`, vertical toolbars use `WidthRequest`.

## Overflow Modes

When toolbar items exceed available space, use the `OverflowMode` property to control how items are displayed.

### Overflow Mode Options
- **Scroll** - Items scroll horizontally or vertically (default)
- **NavigationButtons** - Navigation arrows to move between pages of items
- **MoreButton** - Overflow menu button showing hidden items

## Scroll Mode

Items scroll when they exceed available space. This is the default behavior.

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56" OverflowMode="Scroll">
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
        <toolbar:SfToolbarItem Name="AlignRight" ToolTipText="Align Right">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE753;" FontFamily="MauiMaterialAssets" />
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
    OverflowMode = ToolbarItemOverflowMode.Scroll
};
```

**Behavior:**
- User can swipe/scroll to see hidden items
- No visual indicator for overflow
- Smooth scrolling experience
- Best for frequently accessed items

## Navigation Button Mode

Display navigation buttons (arrows) to move between pages of toolbar items.

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56" OverflowMode="NavigationButtons">
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
SfToolbar toolbar = new SfToolbar
{
    HeightRequest = 56,
    OverflowMode = ToolbarItemOverflowMode.NavigationButtons
};
```

**Behavior:**
- Previous/Next arrow buttons appear when items overflow
- Click arrows to navigate between pages
- Clear visual feedback about overflow
- Best for structured navigation through items

## More Button Mode

Display a "more" button (three dots) that opens a dropdown menu with overflow items.

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56" OverflowMode="MoreButton">
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
        <toolbar:SfToolbarItem Name="AlignRight" ToolTipText="Align Right">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE753;" FontFamily="MauiMaterialAssets" />
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
    OverflowMode = ToolbarItemOverflowMode.MoreButton
};
```

**Behavior:**
- Three-dot "more" button appears when items overflow
- Clicking shows dropdown menu with hidden items
- Conserves screen space
- Common mobile pattern
- Best for less frequently used items

## Multi-Row Toolbar

Display toolbar items across multiple rows using the OverflowMode="MultiRow" property.

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="120" OverflowMode="MultiRow">
    <toolbar:SfToolbar.Items>
        <!-- Row 1 items -->
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
        <toolbar:SfToolbarItem Name="Undo" ToolTipText="Undo">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE744;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        <toolbar:SfToolbarItem Name="Redo" ToolTipText="Redo">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE745;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <!-- Row 2 items -->
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
SfToolbar toolbar = new SfToolbar
{
    HeightRequest = 120,
    OverflowMode = ToolbarItemOverflowMode.MultiRow
};
```

**Key Points:**
- Items automatically wrap to next row when they exceed available width
- Set appropriate `HeightRequest` to accommodate all rows
- Each row height matches the tallest item in that row
- Useful for displaying many items without scrolling

## Extended Row Support

Control the number of rows using the OverflowMode="Extended property (default is 1).

**XAML - Two Rows:**
```xaml
<toolbar:SfToolbar HeightRequest="120" 
                  OverflowMode="Extended"
                  >
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Item1" ToolTipText="Item 1">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE719;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        <toolbar:SfToolbarItem Name="Item2" ToolTipText="Item 2">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE718;" FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        <!-- Add more items... -->
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

**XAML - Three Rows:**
```xaml
<toolbar:SfToolbar HeightRequest="180" 
                   OverflowMode="Extended">
    <toolbar:SfToolbar.Items>
        <!-- Add 15-20 items for three rows -->
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

**C#:**
```csharp
SfToolbar toolbar = new SfToolbar
{
    HeightRequest = 180,
    OverflowMode = ToolbarItemOverflowMode.Extended
};
```

**Height Calculation Guide:**
- 1 row: ~56-60 pixels
- 2 rows: ~112-120 pixels
- 3 rows: ~168-180 pixels
- Add extra padding if needed

## Best Practices

### Choosing Overflow Mode

**Use Scroll when:**
- Items are frequently accessed
- User expects to see all options
- Touch/swipe gestures are natural
- Limited screen space on mobile

**Use NavigationButtons when:**
- Items can be logically grouped
- Desktop application with mouse navigation
- Clear indication of more items needed
- Structured navigation is preferred

**Use MoreButton when:**
- Some items are rarely used
- Following mobile app conventions
- Minimizing visual clutter
- Providing secondary actions

### Multi-Row Considerations

**Advantages:**
- Display more items without scrolling
- Group related items by row
- Better discoverability

**Disadvantages:**
- Takes more vertical space
- May not fit on smaller screens
- Harder to scan visually

### Orientation Guidelines

**Horizontal Toolbar:**
- Default choice for most apps
- Top or bottom placement
- Standard desktop/mobile pattern

**Vertical Toolbar:**
- Side panel placement
- Drawing/design applications
- When vertical space is limited

### Responsive Design

**Adapt to screen size:**

```csharp
public partial class MainPage : ContentPage
{
    private SfToolbar toolbar;
    
    public MainPage()
    {
        InitializeComponent();
        
        // Adjust overflow mode based on screen width
        if (DeviceInfo.Platform == DevicePlatform.Android || 
            DeviceInfo.Platform == DevicePlatform.iOS)
        {
            if (DeviceDisplay.MainDisplayInfo.Width < 600)
            {
                toolbar.OverflowMode = ToolbarItemOverflowMode.MoreButton;
            }
            else
            {
                toolbar.OverflowMode = ToolbarItemOverflowMode.Scroll;
            }
        }
    }
}
```

### Testing Checklist

- [ ] Test with minimum screen size
- [ ] Verify overflow behavior with many items
- [ ] Check orientation switching (if supported)
- [ ] Test on different platforms (iOS, Android, Windows, macOS)
- [ ] Verify multi-row layout on tablets
- [ ] Test RTL layout for international apps

## Common Pitfalls

**Multi-row not working:**
- Ensure OverflowMode="Multirow" is set
- Check `HeightRequest` is sufficient for multiple rows
- Verify items don't have fixed positions

**Navigation buttons not appearing:**
- Confirm `OverflowMode="NavigationButtons"`
- Ensure enough items to cause overflow
- Check toolbar width is constrained

**More button showing empty menu:**
- Verify items exist in the collection
- Check item visibility settings
- Ensure proper item configuration

**Items not wrapping to next row:**
- Set OverflowMode="Extended"
- Increase `ExtendedRow` count if needed
- Check item widths aren't too large

**Scroll not working:**
- Verify `OverflowMode="Scroll"`
- Check toolbar has constrained width/height
- Ensure items exceed available space

## Platform-Specific Considerations

**iOS:**
- More button pattern is familiar to users
- Scrolling feels natural with swipe gestures

**Android:**
- Navigation buttons match Material Design
- More button (three dots) is standard pattern

**Windows:**
- Navigation buttons work well with mouse
- Consider ribbon-style multi-row layout

**macOS:**
- Toolbar typically at window top
- More button or scroll preferred over navigation buttons
