# Layout and Segmentation in .NET MAUI Radial Menu

## Table of Contents
- [Layout Types Overview](#layout-types-overview)
- [Default Layout](#default-layout)
- [Custom Layout](#custom-layout)
- [VisibleSegmentsCount](#visiblesegmentscount)
- [SegmentIndex](#segmentindex)
- [Complete Example](#complete-example)
- [Use Cases](#use-cases)

The Radial Menu provides two layout types that control how menu items are arranged around the circle. Understanding these layouts is crucial for creating intentional, user-friendly menu designs.

## Layout Types Overview

There are two layout types available:

1. **Default** - Sequential arrangement, segments determined by item count
2. **Custom** - Indexed positioning, fixed segment count with precise placement

Both layouts divide the available circular space, but handle item placement differently.

## Default Layout

In Default layout, the number of segments equals the number of items at each hierarchical level. Items are arranged sequentially in the order they're added.

**Key Characteristics:**
- Segment count = Item count
- Items fill the circle automatically
- Sequential order (first item, second item, etc.)
- Different segment counts per level
- No gaps unless items are fewer than expected

**XAML:**
```xaml
<radialMenu:SfRadialMenu LayoutType="Default">
    <radialMenu:SfRadialMenu.Items>
        <radialMenu:SfRadialMenuItem Text="Bold" FontSize="12"/>
        <radialMenu:SfRadialMenuItem Text="Copy" FontSize="12"/>
        <radialMenu:SfRadialMenuItem Text="Undo" FontSize="12"/>
        <radialMenu:SfRadialMenuItem Text="Paste" FontSize="12"/>
        <radialMenu:SfRadialMenuItem Text="Color" FontSize="12"/>
    </radialMenu:SfRadialMenu.Items>
</radialMenu:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu
{
    LayoutType = LayoutType.Default
};

RadialMenuItemsCollection itemCollection = new RadialMenuItemsCollection
{
    new SfRadialMenuItem { Text = "Bold", FontSize = 12 },
    new SfRadialMenuItem { Text = "Copy", FontSize = 12 },
    new SfRadialMenuItem { Text = "Undo", FontSize = 12 },
    new SfRadialMenuItem { Text = "Paste", FontSize = 12 },
    new SfRadialMenuItem { Text = "Color", FontSize = 12 }
};

radialMenu.Items = itemCollection;
this.Content = radialMenu;
```

**Result:**
- 5 items create 5 equal segments (72° each)
- Items appear at: 0°, 72°, 144°, 216°, 288°
- Automatically fills the circle
- No empty segments

**When to Use Default Layout:**
- Simple menus with varying item counts
- Dynamic content where item count changes
- No need for specific positioning
- Quick setup without complexity
- All items should be visible

**Advantages:**
- Simple to implement
- No manual positioning needed
- Adapts to item count automatically
- Consistent spacing

**Limitations:**
- Can't control precise positioning
- Can't leave intentional gaps
- All items must be contiguous

## Custom Layout

In Custom layout, you define a fixed number of segments with `VisibleSegmentsCount`, and place items at specific positions using `SegmentIndex`. This allows for precise control over item placement.

**Key Characteristics:**
- Fixed segment count across all levels
- Items placed by index
- Can leave gaps (empty segments)
- Explicit positioning required
- Same segment layout for all hierarchical levels

**XAML:**
```xaml
<radialMenu:SfRadialMenu LayoutType="Custom" 
                         VisibleSegmentsCount="8">
    <radialMenu:SfRadialMenu.Items>
        <radialMenu:SfRadialMenuItem Text="Bold" 
                                     FontSize="12"
                                     SegmentIndex="0"/>
        <radialMenu:SfRadialMenuItem Text="Copy" 
                                     FontSize="12"
                                     SegmentIndex="2"/>
        <radialMenu:SfRadialMenuItem Text="Undo" 
                                     FontSize="12"
                                     SegmentIndex="4"/>
        <radialMenu:SfRadialMenuItem Text="Paste" 
                                     FontSize="12"
                                     SegmentIndex="6"/>
    </radialMenu:SfRadialMenu.Items>
</radialMenu:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu
{
    LayoutType = LayoutType.Custom,
    VisibleSegmentsCount = 8
};

RadialMenuItemsCollection itemCollection = new RadialMenuItemsCollection
{
    new SfRadialMenuItem 
    { 
        Text = "Bold", 
        FontSize = 12,
        SegmentIndex = 0 
    },
    new SfRadialMenuItem 
    { 
        Text = "Copy", 
        FontSize = 12,
        SegmentIndex = 2 
    },
    new SfRadialMenuItem 
    { 
        Text = "Undo", 
        FontSize = 12,
        SegmentIndex = 4 
    },
    new SfRadialMenuItem 
    { 
        Text = "Paste", 
        FontSize = 12,
        SegmentIndex = 6 
    }
};

radialMenu.Items = itemCollection;
this.Content = radialMenu;
```

**Result:**
- 8 segments defined (45° each)
- Items at segments: 0, 2, 4, 6
- Empty segments: 1, 3, 5, 7
- Even spacing with gaps between items

**When to Use Custom Layout:**
- Need precise item positioning
- Want intentional gaps/spacing
- Consistent segment structure across levels
- Clockface or compass-like layouts
- Specific alignment requirements

**Advantages:**
- Precise control over placement
- Can create intentional gaps
- Consistent structure across levels
- Predictable positioning

**Limitations:**
- More setup required
- Must manage segment indices manually
- Items beyond VisibleSegmentsCount are hidden

## VisibleSegmentsCount

The `VisibleSegmentsCount` property defines the total number of segments in the circular panel when using Custom layout.

**XAML:**
```xaml
<radialMenu:SfRadialMenu LayoutType="Custom"
                         VisibleSegmentsCount="12">
    <!-- 12 segments like a clock face -->
</radialMenu:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu
{
    LayoutType = LayoutType.Custom,
    VisibleSegmentsCount = 12 // Like clock hours
};
```

**Key Points:**
- Only applies to Custom layout
- Defines available positions for items
- Items beyond this count are not displayed
- Common values: 4, 6, 8, 12, 16
- All hierarchical levels use same count

**Common Segment Counts:**
- **4 segments:** Cardinal directions (N, E, S, W)
- **6 segments:** Hexagonal layout
- **8 segments:** Compass points (N, NE, E, SE, S, SW, W, NW)
- **12 segments:** Clock face layout
- **16 segments:** Fine-grained positioning

## SegmentIndex

The `SegmentIndex` property specifies where a menu item appears in the circular panel (Custom layout only).

**XAML:**
```xaml
<radialMenu:SfRadialMenu LayoutType="Custom" 
                         VisibleSegmentsCount="12">
    <radialMenu:SfRadialMenu.Items>
        <!-- Place at 12 o'clock -->
        <radialMenu:SfRadialMenuItem Text="Top" 
                                     SegmentIndex="0"
                                     FontSize="12"/>
        
        <!-- Place at 3 o'clock -->
        <radialMenu:SfRadialMenuItem Text="Right" 
                                     SegmentIndex="3"
                                     FontSize="12"/>
        
        <!-- Place at 6 o'clock -->
        <radialMenu:SfRadialMenuItem Text="Bottom" 
                                     SegmentIndex="6"
                                     FontSize="12"/>
        
        <!-- Place at 9 o'clock -->
        <radialMenu:SfRadialMenuItem Text="Left" 
                                     SegmentIndex="9"
                                     FontSize="12"/>
    </radialMenu:SfRadialMenu.Items>
</radialMenu:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu
{
    LayoutType = LayoutType.Custom,
    VisibleSegmentsCount = 12
};

RadialMenuItemsCollection itemCollection = new RadialMenuItemsCollection
{
    new SfRadialMenuItem { Text = "Top", SegmentIndex = 0, FontSize = 12 },
    new SfRadialMenuItem { Text = "Right", SegmentIndex = 3, FontSize = 12 },
    new SfRadialMenuItem { Text = "Bottom", SegmentIndex = 6, FontSize = 12 },
    new SfRadialMenuItem { Text = "Left", SegmentIndex = 9, FontSize = 12 }
};

radialMenu.Items = itemCollection;
```

**Key Points:**
- Zero-based indexing (0 = first segment)
- Must be < VisibleSegmentsCount
- If not specified, item fills next available segment
- Can skip indices to create gaps
- Duplicate indices: Last item wins

**Automatic Placement:**
If you don't specify `SegmentIndex`, items fill available segments in order:

```csharp
// Items without SegmentIndex fill automatically
new SfRadialMenuItem { Text = "Auto 1" }, // Goes to segment 0
new SfRadialMenuItem { Text = "Auto 2" }, // Goes to segment 1
new SfRadialMenuItem { Text = "Placed", SegmentIndex = 5 }, // Goes to segment 5
new SfRadialMenuItem { Text = "Auto 3" }  // Goes to segment 6 (next available)
```

## Complete Example

Clock-style menu with social media items:

**XAML:**
```xaml
<radialMenu:SfRadialMenu x:Name="radialMenu"
                         CenterButtonFontFamily="MauiSampleFontIcon"
                         CenterButtonFontSize="30"
                         CenterButtonText="&#xe770;"
                         CenterButtonStrokeThickness="3"
                         LayoutType="Custom"
                         RimColor="Transparent"
                         RimRadius="300"
                         SeparatorThickness="0"
                         VisibleSegmentsCount="12">
    <radialMenu:SfRadialMenu.Items>
        <!-- LinkedIn at 12 o'clock -->
        <radialMenu:SfRadialMenuItem BackgroundColor="Transparent"
                                     FontFamily="MauiSampleFontIcon"
                                     FontSize="40"
                                     ItemHeight="40"
                                     ItemWidth="40"
                                     SegmentIndex="0"
                                     Text="&#xe71f;"/>
        
        <!-- Twitter at 3 o'clock -->
        <radialMenu:SfRadialMenuItem BackgroundColor="Transparent"
                                     FontFamily="MauiSampleFontIcon"
                                     FontSize="40"
                                     ItemHeight="40"
                                     ItemWidth="40"
                                     SegmentIndex="3"
                                     Text="&#xe720;"/>
        
        <!-- Facebook at 6 o'clock -->
        <radialMenu:SfRadialMenuItem BackgroundColor="Transparent"
                                     FontFamily="MauiSampleFontIcon"
                                     FontSize="40"
                                     ItemHeight="40"
                                     ItemWidth="40"
                                     SegmentIndex="6"
                                     Text="&#xe71e;"/>
        
        <!-- Instagram at 9 o'clock -->
        <radialMenu:SfRadialMenuItem BackgroundColor="Transparent"
                                     FontFamily="MauiSampleFontIcon"
                                     FontSize="40"
                                     ItemHeight="40"
                                     ItemWidth="40"
                                     SegmentIndex="9"
                                     Text="&#xe721;"/>
    </radialMenu:SfRadialMenu.Items>
</radialMenu:SfRadialMenu>
```

**Result:**
- 12 segments (like clock)
- 4 items at cardinal positions
- 8 empty segments
- Transparent rim and items
- Icon font for social media icons

## Use Cases

### Default Layout Use Cases

1. **Text Editor Menu:**
   ```csharp
   // Bold, Italic, Underline, Color, Font
   // Items: 5, Segments: 5 (automatic)
   LayoutType = LayoutType.Default
   ```

2. **Media Player Controls:**
   ```csharp
   // Play, Pause, Stop, Next, Previous
   // Variable item count
   LayoutType = LayoutType.Default
   ```

3. **Dynamic Menus:**
   ```csharp
   // Item count changes based on context
   // Let the menu adjust automatically
   LayoutType = LayoutType.Default
   ```

### Custom Layout Use Cases

1. **Compass Navigation:**
   ```csharp
   // N, NE, E, SE, S, SW, W, NW
   LayoutType = LayoutType.Custom
   VisibleSegmentsCount = 8
   // SegmentIndex: 0, 1, 2, 3, 4, 5, 6, 7
   ```

2. **Directional Controls:**
   ```csharp
   // Up, Right, Down, Left
   LayoutType = LayoutType.Custom
   VisibleSegmentsCount = 12
   // SegmentIndex: 0, 3, 6, 9 (with gaps)
   ```

3. **Social Media Sharing:**
   ```csharp
   // Place icons at specific positions
   // Leave gaps for visual balance
   LayoutType = LayoutType.Custom
   VisibleSegmentsCount = 12
   // Strategic placement at key hours
   ```

4. **Gaming Controls:**
   ```csharp
   // Action buttons at precise locations
   // Match controller layout
   LayoutType = LayoutType.Custom
   VisibleSegmentsCount = 8
   ```

## Choosing the Right Layout

**Choose Default When:**
- Simple menu with all items visible
- Item count varies
- Quick setup needed
- Sequential order is fine
- No specific positioning needed

**Choose Custom When:**
- Specific item placement required
- Need visual gaps/spacing
- Consistent structure across levels
- Clockface or compass layout
- Aligning with real-world patterns

## Tips and Best Practices

1. **Start with Default:** Use Default layout first; only switch to Custom if you need specific positioning

2. **Power of 2:** For Custom layout, use segment counts that divide 360° evenly: 4, 6, 8, 12, 16

3. **Visual Balance:** With Custom layout, space items evenly or use intentional asymmetry

4. **Test on Device:** Always test touch targets on actual devices, especially with Custom layout and gaps

5. **Consistent Structure:** If using Custom layout with nested items, keep VisibleSegmentsCount consistent across levels

6. **Document Indices:** Comment your SegmentIndex values for clarity:
   ```csharp
   SegmentIndex = 0  // Top (12 o'clock)
   SegmentIndex = 3  // Right (3 o'clock)
   ```

7. **Avoid Crowding:** Don't use more items than segments in Custom layout; excess items won't appear
