# Advanced Features in .NET MAUI Toolbar

Learn about advanced toolbar features including overlay toolbars and the liquid glass effect for iOS 18.

## Table of Contents
- [Overview](#overview)
- [Overlay Toolbar](#overlay-toolbar)
- [Liquid Glass Effect](#liquid-glass-effect)
- [Best Practices](#best-practices)

## Overview

The SfToolbar includes advanced features for modern UI experiences:
- **Overlay Toolbar** - Sub-menu toolbar triggered from a toolbar item that displays options overlay
- **Liquid Glass Effect** - iOS 18+ visual effect with frosted glass appearance using `EnableLiquidGlassEffect`

## Overlay Toolbar

Create overlay toolbars as sub-menus using `SfOverlayToolbar` component attached to a `SfToolbarItem` via the `OverlayToolbar` property. Perfect for hierarchical menu structures and additional options.

### Basic Overlay Toolbar

Overlay toolbars appear as sub-menus when you tap a toolbar item. They display hierarchical options overlay the main toolbar.

**XAML:**
```xaml
<toolbar:SfToolbar x:Name="toolbar" HeightRequest="56">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Cut" ToolTipText="Cut">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE719;" 
                                 FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SfToolbarItem Name="Copy" ToolTipText="Copy">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE718;" 
                                 FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <!-- Toolbar item with overlay toolbar -->
        <toolbar:SfToolbarItem Name="Alignment"
                               Text="Alignment"
                               ToolTipText="View alignment options"
                               Size="80,40">
            <toolbar:SfToolbarItem.OverlayToolbar>
                <toolbar:SfOverlayToolbar x:Name="alignmentOverlay">
                    <toolbar:SfOverlayToolbar.Items>
                        <toolbar:SfToolbarItem Name="AlignLeft" ToolTipText="Align Left">
                            <toolbar:SfToolbarItem.Icon>
                                <FontImageSource Glyph="&#xE751;" 
                                                 FontFamily="MauiMaterialAssets" />
                            </toolbar:SfToolbarItem.Icon>
                        </toolbar:SfToolbarItem>
                        
                        <toolbar:SfToolbarItem Name="AlignCenter" ToolTipText="Align Center">
                            <toolbar:SfToolbarItem.Icon>
                                <FontImageSource Glyph="&#xE752;" 
                                                 FontFamily="MauiMaterialAssets" />
                            </toolbar:SfToolbarItem.Icon>
                        </toolbar:SfToolbarItem>
                        
                        <toolbar:SfToolbarItem Name="AlignRight" ToolTipText="Align Right">
                            <toolbar:SfToolbarItem.Icon>
                                <FontImageSource Glyph="&#xE753;" 
                                                 FontFamily="MauiMaterialAssets" />
                            </toolbar:SfToolbarItem.Icon>
                        </toolbar:SfToolbarItem>
                    </toolbar:SfOverlayToolbar.Items>
                </toolbar:SfOverlayToolbar>
            </toolbar:SfToolbarItem.OverlayToolbar>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

**C#:**
```csharp
SfToolbar toolbar = new SfToolbar { HeightRequest = 56 };

// Alignment overlay toolbar
var alignmentOverlay = new SfOverlayToolbar();
alignmentOverlay.Items.Add(new SfToolbarItem { Name = "AlignLeft", ToolTipText = "Align Left" });
alignmentOverlay.Items.Add(new SfToolbarItem { Name = "AlignCenter", ToolTipText = "Align Center" });
alignmentOverlay.Items.Add(new SfToolbarItem { Name = "AlignRight", ToolTipText = "Align Right" });

// Main toolbar item with overlay
var alignmentItem = new SfToolbarItem 
{ 
    Name = "Alignment",
    Text = "Alignment",
    OverlayToolbar = alignmentOverlay,
    Size = new Size(80, 40)
};

toolbar.Items.Add(alignmentItem);
```

### Format Options with Overlay Toolbar

Display text formatting options through an overlay toolbar when user taps "More".

**XAML:**
```xaml
<toolbar:SfToolbar x:Name="formatToolbar" HeightRequest="56">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Bold" ToolTipText="Bold">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE770;" 
                                 FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SfToolbarItem Name="Italic" ToolTipText="Italic">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE771;" 
                                 FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <!-- More menu with overlay toolbar -->
        <toolbar:SfToolbarItem Name="More" 
                               Text="More"
                               ToolTipText="Additional formatting options"
                               Size="60,40">
            <toolbar:SfToolbarItem.OverlayToolbar>
                <toolbar:SfOverlayToolbar x:Name="moreOptionsOverlay">
                    <toolbar:SfOverlayToolbar.Items>
                        <toolbar:SfToolbarItem Name="Underline" ToolTipText="Underline">
                            <toolbar:SfToolbarItem.Icon>
                                <FontImageSource Glyph="&#xE762;" 
                                                 FontFamily="MauiMaterialAssets" />
                            </toolbar:SfToolbarItem.Icon>
                        </toolbar:SfToolbarItem>
                        
                        <toolbar:SfToolbarItem Name="Strikethrough" ToolTipText="Strikethrough">
                            <toolbar:SfToolbarItem.Icon>
                                <FontImageSource Glyph="&#xE761;" 
                                                 FontFamily="MauiMaterialAssets" />
                            </toolbar:SfToolbarItem.Icon>
                        </toolbar:SfToolbarItem>
                        
                        <toolbar:SfToolbarItem Name="Subscript" ToolTipText="Subscript">
                            <toolbar:SfToolbarItem.Icon>
                                <FontImageSource Glyph="&#xE78B;" 
                                                 FontFamily="MauiMaterialAssets" />
                            </toolbar:SfToolbarItem.Icon>
                        </toolbar:SfToolbarItem>
                        
                        <toolbar:SfToolbarItem Name="Superscript" ToolTipText="Superscript">
                            <toolbar:SfToolbarItem.Icon>
                                <FontImageSource Glyph="&#xE78A;" 
                                                 FontFamily="MauiMaterialAssets" />
                            </toolbar:SfToolbarItem.Icon>
                        </toolbar:SfToolbarItem>
                    </toolbar:SfOverlayToolbar.Items>
                </toolbar:SfOverlayToolbar>
            </toolbar:SfToolbarItem.OverlayToolbar>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

### Handling Overlay Toolbar Interactions

Listen to selection changes in overlay toolbars to handle user actions.

**XAML:**
```xaml
<toolbar:SfToolbar x:Name="toolbar" HeightRequest="56">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Format" 
                               Text="Format"
                               ToolTipText="Format options"
                               Size="80,40"
                               SelectionChanged="OnFormatSelectionChanged">
            <toolbar:SfToolbarItem.OverlayToolbar>
                <toolbar:SfOverlayToolbar x:Name="formatOverlay">
                    <toolbar:SfOverlayToolbar.Items>
                        <toolbar:SfToolbarItem Name="Bold" ToolTipText="Bold">
                            <toolbar:SfToolbarItem.Icon>
                                <FontImageSource Glyph="&#xE770;" 
                                                 FontFamily="MauiMaterialAssets" />
                            </toolbar:SfToolbarItem.Icon>
                        </toolbar:SfToolbarItem>
                        
                        <toolbar:SfToolbarItem Name="Italic" ToolTipText="Italic">
                            <toolbar:SfToolbarItem.Icon>
                                <FontImageSource Glyph="&#xE771;" 
                                                 FontFamily="MauiMaterialAssets" />
                            </toolbar:SfToolbarItem.Icon>
                        </toolbar:SfToolbarItem>
                    </toolbar:SfOverlayToolbar.Items>
                </toolbar:SfOverlayToolbar>
            </toolbar:SfToolbarItem.OverlayToolbar>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

**C# Code-behind:**
```csharp
private void OnFormatSelectionChanged(object sender, ToolbarSelectionChangedEventArgs e)
{
    // Handle when overlay items are selected
    foreach (var item in e.AddedItems)
    {
        var selectedItem = item as SfToolbarItem;
        if (selectedItem != null)
        {
            // Apply format based on selection
            switch (selectedItem.Name)
            {
                case "Bold":
                    ApplyBold();
                    break;
                case "Italic":
                    ApplyItalic();
                    break;
            }
        }
    }
}

private void ApplyBold()
{
    // Handle bold formatting
}

private void ApplyItalic()
{
    // Handle italic formatting
}
```

### Overlay Toolbar Back Button Customization

Customize the back button appearance that closes the overlay toolbar.

**XAML:**
```xaml
<toolbar:SfToolbar x:Name="toolbar" HeightRequest="56">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Options" 
                               Text="Options"
                               Size="80,40">
            <toolbar:SfToolbarItem.OverlayToolbar>
                <toolbar:SfOverlayToolbar BackIconColor="Blue"
                                          BackIconAlignment="End"
                                          BackIconToolTipText="Close Options">
                    <toolbar:SfOverlayToolbar.Items>
                        <toolbar:SfToolbarItem Name="Option1" ToolTipText="Option 1">
                            <toolbar:SfToolbarItem.Icon>
                                <FontImageSource Glyph="&#xE756;" 
                                                 FontFamily="MauiMaterialAssets" />
                            </toolbar:SfToolbarItem.Icon>
                        </toolbar:SfToolbarItem>
                        
                        <toolbar:SfToolbarItem Name="Option2" ToolTipText="Option 2">
                            <toolbar:SfToolbarItem.Icon>
                                <FontImageSource Glyph="&#xE757;" 
                                                 FontFamily="MauiMaterialAssets" />
                            </toolbar:SfToolbarItem.Icon>
                        </toolbar:SfToolbarItem>
                    </toolbar:SfOverlayToolbar.Items>
                </toolbar:SfOverlayToolbar>
            </toolbar:SfToolbarItem.OverlayToolbar>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

**C#:**
```csharp
var overlayToolbar = new SfOverlayToolbar
{
    BackIconColor = Colors.Blue,
    BackIconAlignment = OverlayToolbarBackIconPosition.End,
    BackIconToolTipText = "Close Options"
};

overlayToolbar.Items.Add(new SfToolbarItem { Name = "Option1", ToolTipText = "Option 1" });
overlayToolbar.Items.Add(new SfToolbarItem { Name = "Option2", ToolTipText = "Option 2" });
```

## Liquid Glass Effect

**iOS 18+ exclusive feature** that provides a frosted glass blur effect for the main `SfToolbar` control (not overlay toolbars).

### Platform Requirements

- **iOS version:** 18.0 or later
- **Feature:** UIKit blur effect with vibrancy
- **Fallback:** Other platforms show standard toolbar background
- **Note:** Only works on `SfToolbar`, not `SfOverlayToolbar`

### Enabling Liquid Glass Effect

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56"
                   EnableLiquidGlassEffect="True"
                   CornerRadius="12">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Cut" ToolTipText="Cut">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE719;" 
                                 FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SfToolbarItem Name="Copy" ToolTipText="Copy">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE718;" 
                                 FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SfToolbarItem Name="Paste" ToolTipText="Paste">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE71A;" 
                                 FontFamily="MauiMaterialAssets" />
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
    EnableLiquidGlassEffect = true, // iOS 18+ only
    CornerRadius = new CornerRadius(12)
};

// Add items to toolbar
toolbar.Items.Add(new SfToolbarItem { Name = "Cut", ToolTipText = "Cut" });
toolbar.Items.Add(new SfToolbarItem { Name = "Copy", ToolTipText = "Copy" });
toolbar.Items.Add(new SfToolbarItem { Name = "Paste", ToolTipText = "Paste" });
```

### Liquid Glass Effect with Platform Check

**C#:**
```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        ConfigureToolbar();
    }
    
    private void ConfigureToolbar()
    {
        // Enable liquid glass effect only on iOS 18+
        if (DeviceInfo.Platform == DevicePlatform.iOS && 
            DeviceInfo.Version >= new Version(18, 0))
        {
            toolbar.EnableLiquidGlassEffect = true;
            toolbar.CornerRadius = new CornerRadius(12);
        }
        else
        {
            toolbar.EnableLiquidGlassEffect = false;
            toolbar.Background = Colors.White; // Fallback for other platforms
        }
    }
}
```

### Complete Toolbar with Grouped Items and Liquid Glass

Liquid Glass automatically groups toolbar items with separators.

**XAML:**
```xaml
<toolbar:SfToolbar HeightRequest="56"
                   EnableLiquidGlassEffect="True"
                   CornerRadius="12">
    <toolbar:SfToolbar.Items>
        <toolbar:SfToolbarItem Name="Cut" ToolTipText="Cut">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE719;" 
                                 FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SfToolbarItem Name="Copy" ToolTipText="Copy">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE718;" 
                                 FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SfToolbarItem Name="Paste" ToolTipText="Paste">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE71A;" 
                                 FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <!-- Separator creates new glass group -->
        <toolbar:SeparatorToolbarItem />
        
        <toolbar:SfToolbarItem Name="Undo" ToolTipText="Undo">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE744;" 
                                 FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
        
        <toolbar:SfToolbarItem Name="Redo" ToolTipText="Redo">
            <toolbar:SfToolbarItem.Icon>
                <FontImageSource Glyph="&#xE745;" 
                                 FontFamily="MauiMaterialAssets" />
            </toolbar:SfToolbarItem.Icon>
        </toolbar:SfToolbarItem>
    </toolbar:SfToolbar.Items>
</toolbar:SfToolbar>
```

**Key Features:**
- **iOS 18+:** Automatically applies blur and vibrancy effect
- **Grouping:** Items are grouped by separators into individual glass panels
- **CornerRadius:** Controls the curvature of glass panels
- **Fallback:** On other platforms, shows standard toolbar background

### Visual Characteristics

**Liquid Glass Effect provides:**
- **Blur:** Background content is blurred beneath the toolbar
- **Vibrancy:** Toolbar adapts to background colors dynamically
- **Transparency:** Semi-transparent appearance
- **Depth:** Modern layered UI feeling
- **System Integration:** Matches iOS 18 design language

### Platform Fallbacks

**Recommended approach for cross-platform liquid glass:**

```csharp
public class ToolbarConfiguration
{
    public static void ConfigureLiquidGlassToolbar(SfToolbar toolbar)
    {
        if (DeviceInfo.Platform == DevicePlatform.iOS && 
            DeviceInfo.Version >= new Version(18, 0))
        {
            // iOS 18+ with liquid glass effect
            toolbar.EnableLiquidGlassEffect = true;
            toolbar.CornerRadius = new CornerRadius(12);
        }
        else if (DeviceInfo.Platform == DevicePlatform.iOS)
        {
            // iOS pre-18 with standard background
            toolbar.EnableLiquidGlassEffect = false;
            toolbar.Background = Colors.White;
        }
        else if (DeviceInfo.Platform == DevicePlatform.Android)
        {
            // Android with material design background
            toolbar.EnableLiquidGlassEffect = false;
            toolbar.Background = Colors.White;
        }
        else
        {
            // Windows/macOS fallback
            toolbar.EnableLiquidGlassEffect = false;
            toolbar.Background = Colors.White;
        }
    }
}

// Usage
ToolbarConfiguration.ConfigureLiquidGlassToolbar(toolbar);
```

## Best Practices

### Overlay Toolbar (SfOverlayToolbar) Guidelines

1. **Use for hierarchical menus** - Show additional options when tapping toolbar items
2. **Keep overlays focused** - Show 3-6 related options per overlay
3. **Organize logically** - Group related commands together
4. **Customize back button** - Provide clear close affordance
5. **Separate with separators** - Use `SeparatorToolbarItem` in overlays to organize items
6. **Listen to selection** - Handle overlay item selection with event handlers
7. **Indicate expandable items** - Use visual cues (arrow, ellipsis) to show items have overlays
8. **Test on all platforms** - Overlay behavior may vary between iOS, Android, Windows

### Liquid Glass Effect Guidelines

1. **Check platform version** - Verify iOS 18+ before enabling
2. **Use CornerRadius** - Values like 12-16 work best with glass effect
3. **Add separators** - Creates distinct glass panels for visual organization
4. **Limit items per group** - Keep groups under 5 items
5. **Provide fallback** - Always have a standard background for other platforms
6. **Test on actual device** - Simulator rendering may differ from real device
7. **Consider performance** - Blur effects require more rendering resources
8. **Match iOS design** - Align with iOS Human Interface Guidelines

### Common Use Cases

**Overlay Toolbars (SfOverlayToolbar):**
- Text alignment options (Left, Center, Right, Justify)
- Additional formatting options (Underline, Strikethrough, etc.)
- View mode selection (List, Grid, Tile)
- Sort and filter options
- Drawing tools sub-menus
- Export/import options

**Liquid Glass Effect:**
- Modern iOS 18+ apps
- Premium application features
- Reading apps with blurred header
- Music/media players
- Photo editing interfaces
- Dashboard applications

## Common Pitfalls

**Overlay toolbar not appearing:**
- Confirm `OverlayToolbar` property is set on a `SfToolbarItem`
- Verify overlay contains `SfOverlayToolbar` component
- Check that parent item is tappable (not disabled)
- Test on actual device

**Overlay toolbar not responding:**
- Verify `SelectionChanged` event is wired up
- Check that overlay items have unique `Name` properties
- Confirm `ToolbarSelectionChangedEventArgs` handler exists
- Ensure overlay items are not all disabled

**Liquid glass effect not showing:**
- Verify running on iOS 18 or later
- Check `EnableLiquidGlassEffect="True"` is set
- Set appropriate `CornerRadius` (12-16 recommended)
- Test on actual device, not simulator
- Ensure toolbar is not using custom background conflicting with effect

**Overlay toolbar back button issues:**
- Back button appears by default - customize if needed
- Use `BackIconAlignment` to reposition
- Modify `BackIconColor` for visibility
- Use `BackIconToolTipText` to describe action

**Performance degradation:**
- Limit number of overlay toolbars per page
- Use `EnableLiquidGlassEffect` only on iOS 18+
- Avoid complex nested overlays
- Test on lower-end iOS devices
- Monitor UI rendering performance

**Layout issues:**
- Overlay toolbar automatically inherits main toolbar orientation
- Do not set explicit Height/Width on SfOverlayToolbar
- Separators create visual groups in overlays
- Test with different item counts and content
