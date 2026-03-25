# Layout Configuration

This guide covers sizing, dimensions, and scrolling behavior for the Segmented Control.

## Overview

Configure the layout properties to control:
- Segment width (uniform or per-item)
- Segment height
- Number of visible segments (enables scrolling)
- Auto-sizing vs fixed dimensions

## Segment Width

### Uniform Segment Width

Set a fixed width for all segments using `SegmentWidth`.

**XAML:**
```xml
<buttons:SfSegmentedControl SegmentWidth="80">
    <buttons:SfSegmentedControl.ItemsSource>
        <x:Array Type="{x:Type x:String}">
            <x:String>Day</x:String>
            <x:String>Week</x:String>
            <x:String>Month</x:String>
            <x:String>Year</x:String>
        </x:Array>
    </buttons:SfSegmentedControl.ItemsSource>
</buttons:SfSegmentedControl>
```

**C#:**
```csharp
segmentedControl.SegmentWidth = 80;
```

**Effects:**
- All segments have equal width
- Control width = SegmentWidth × ItemCount
- No automatic text wrapping (text may truncate)

**Use case:** Consistent appearance, predictable layout, fixed-width designs.

### Auto-Sizing (Default)

When `SegmentWidth` is not set, segments automatically size to fit content with equal distribution.

```csharp
// Auto-sizing (default behavior)
var segmentedControl = new SfSegmentedControl
{
    HorizontalOptions = LayoutOptions.FillAndExpand,
    ItemsSource = new List<string> { "Day", "Week", "Month", "Year" }
};
```

**Effects:**
- Segments distribute evenly across available width
- Adapts to container size
- Text determines minimum width

**Use case:** Responsive layouts, variable text lengths, flexible designs.

### Per-Item Width

Customize the width for individual segments.

**C#:**
```csharp
var segmentedControl = new SfSegmentedControl
{
    ItemsSource = new List<SfSegmentItem>
    {
        new SfSegmentItem { Text = "S", Width = 40 },        // Narrow
        new SfSegmentItem { Text = "M", Width = 50 },
        new SfSegmentItem { Text = "Large", Width = 100 }    // Wide
    }
};
```

**Effects:**
- Each segment has its specified width
- Accommodates varying text lengths
- Useful for emphasizing specific options

**Use case:** Abbreviated labels with different lengths, icon + text combinations, priority emphasis.

### Width Priority

When both `SegmentWidth` and per-item `Width` are set:
1. Per-item `Width` takes precedence
2. `SegmentWidth` applies to items without explicit width

```csharp
segmentedControl.SegmentWidth = 80;  // Default width

segmentedControl.ItemsSource = new List<SfSegmentItem>
{
    new SfSegmentItem { Text = "Day" },           // Uses SegmentWidth (80)
    new SfSegmentItem { Text = "Week", Width = 100 },  // Uses explicit width (100)
    new SfSegmentItem { Text = "Month" }          // Uses SegmentWidth (80)
};
```

## Segment Height

Control the height of all segments using `SegmentHeight`.

**XAML:**
```xml
<buttons:SfSegmentedControl SegmentHeight="50">
    <buttons:SfSegmentedControl.ItemsSource>
        <x:Array Type="{x:Type x:String}">
            <x:String>Day</x:String>
            <x:String>Week</x:String>
            <x:String>Month</x:String>
        </x:Array>
    </buttons:SfSegmentedControl.ItemsSource>
</buttons:SfSegmentedControl>
```

**C#:**
```csharp
segmentedControl.SegmentHeight = 50;
```

**Default:** 32 device-independent pixels

**Recommended values:**
- Compact: 32-36 pixels (default)
- Standard: 40-48 pixels (comfortable)
- Large: 50-60 pixels (accessible, touch-friendly)

**Considerations:**
- Minimum touch target: 44 pixels (iOS), 48 pixels (Android)
- Ensure text and icons fit without clipping
- Taller segments improve accessibility

## Visible Segment Count

Control how many segments are visible without scrolling using `VisibleSegmentsCount`.

**XAML:**
```xml
<buttons:SfSegmentedControl VisibleSegmentsCount="3">
    <buttons:SfSegmentedControl.ItemsSource>
        <x:Array Type="{x:Type x:String}">
            <x:String>Jan</x:String>
            <x:String>Feb</x:String>
            <x:String>Mar</x:String>
            <x:String>Apr</x:String>
            <x:String>May</x:String>
            <x:String>Jun</x:String>
        </x:Array>
    </buttons:SfSegmentedControl.ItemsSource>
</buttons:SfSegmentedControl>
```

**C#:**
```csharp
segmentedControl.VisibleSegmentsCount = 3;
```

**Effects:**
- Only specified number of segments visible at once
- Remaining segments accessible via horizontal scrolling
- Control width = calculated segment width × VisibleSegmentsCount

**Use case:** Many segments (6+), limited horizontal space, month/day selectors.

### Scrolling Behavior

When `VisibleSegmentsCount` is set:
- Horizontal scrolling enabled automatically
- Selected segment scrolls into view
- Smooth scroll animation
- Scrollbar appearance depends on platform

**Example with scrolling:**
```csharp
var segmentedControl = new SfSegmentedControl
{
    VisibleSegmentsCount = 4,
    ItemsSource = new List<string> 
    { 
        "Jan", "Feb", "Mar", "Apr", "May", "Jun",
        "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"
    }
};
```

Only 4 months visible at a time; user scrolls to see others.

## Layout Calculations

### Auto-Sizing Formula

When `VisibleSegmentsCount` is not set:
```
Segment Width = (Control Width - Total Border Width) / Item Count
```

### Fixed Segment Width Formula

When `SegmentWidth` is set:
```
Control Width = SegmentWidth × Item Count + Total Border Width
```

### Visible Segments Formula

When `VisibleSegmentsCount` is set:
```
Segment Width = Control Width / VisibleSegmentsCount
Control Width = User-specified or container width
```

**Important:** When `VisibleSegmentsCount` is set:
- `SegmentWidth` property is ignored
- Per-item `Width` property is ignored
- All segments auto-size to fit visible count

## Responsive Layout Patterns

### Full-Width Distribution

Segments fill available horizontal space equally:

```xml
<buttons:SfSegmentedControl HorizontalOptions="FillAndExpand">
    <buttons:SfSegmentedControl.ItemsSource>
        <x:Array Type="{x:Type x:String}">
            <x:String>List</x:String>
            <x:String>Grid</x:String>
            <x:String>Map</x:String>
        </x:Array>
    </buttons:SfSegmentedControl.ItemsSource>
</buttons:SfSegmentedControl>
```

### Centered with Fixed Width

Control centered with explicit segment widths:

```xml
<buttons:SfSegmentedControl HorizontalOptions="Center" SegmentWidth="100">
    <!-- Items -->
</buttons:SfSegmentedControl>
```

### Adaptive for Screen Size

Adjust visible segments based on screen width:

```csharp
var screenWidth = DeviceDisplay.MainDisplayInfo.Width / DeviceDisplay.MainDisplayInfo.Density;

int visibleCount = screenWidth > 600 ? 6 : 4;  // More segments on tablets

segmentedControl.VisibleSegmentsCount = visibleCount;
```

## Common Layout Scenarios

### Compact Navigation (3-4 Items)

```csharp
var navigation = new SfSegmentedControl
{
    SegmentHeight = 40,
    HorizontalOptions = LayoutOptions.FillAndExpand,
    ItemsSource = new List<string> { "Home", "Search", "Profile" }
};
```

### Scrollable Month Selector (12 Items)

```csharp
var monthSelector = new SfSegmentedControl
{
    VisibleSegmentsCount = 4,
    SegmentHeight = 36,
    ItemsSource = new List<string> 
    { 
        "Jan", "Feb", "Mar", "Apr", "May", "Jun",
        "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"
    }
};
```

### Time Period with Custom Widths

```csharp
var timePeriod = new SfSegmentedControl
{
    ItemsSource = new List<SfSegmentItem>
    {
        new SfSegmentItem { Text = "1D", Width = 50 },
        new SfSegmentItem { Text = "1W", Width = 50 },
        new SfSegmentItem { Text = "1M", Width = 50 },
        new SfSegmentItem { Text = "3M", Width = 50 },
        new SfSegmentItem { Text = "1Y", Width = 50 },
        new SfSegmentItem { Text = "All Time", Width = 100 }  // Wider
    }
};
```

## Best Practices

### Segment Count

- **Optimal:** 2-5 segments without scrolling
- **Acceptable:** 6-8 segments with scrolling enabled
- **Avoid:** More than 10 segments (consider dropdown instead)

### Width Guidelines

- **Short text (1-3 chars):** 40-60 pixels
- **Medium text (4-8 chars):** 60-100 pixels
- **Long text (9+ chars):** 100-150 pixels or use auto-sizing

### Height Guidelines

- **Minimum:** 32 pixels (default)
- **Touch-friendly:** 44-48 pixels
- **Accessible:** 50+ pixels

### Scrolling

- Enable scrolling (`VisibleSegmentsCount`) when items exceed 5-6
- Ensure at least 3-4 segments visible for context
- Avoid scrolling for 2-3 items (wastes space)

### Responsive Design

- Test on different screen sizes (phone, tablet)
- Use `HorizontalOptions="FillAndExpand"` for adaptive width
- Adjust `VisibleSegmentsCount` based on device type

## Troubleshooting

### Segments Too Narrow

**Cause:** Too many items without scrolling, or SegmentWidth too small  
**Solution:** 
- Set `VisibleSegmentsCount` to enable scrolling
- Increase `SegmentWidth`
- Reduce number of items

### Text Truncating

**Cause:** Segment width insufficient for text length  
**Solution:**
- Increase `SegmentWidth`
- Use abbreviations
- Enable auto-sizing (remove SegmentWidth)
- Set per-item Width for longer text items

### Control Too Wide

**Cause:** SegmentWidth × ItemCount exceeds screen width  
**Solution:**
- Reduce `SegmentWidth`
- Enable scrolling with `VisibleSegmentsCount`
- Use auto-sizing

### Segments Not Scrolling

**Cause:** VisibleSegmentsCount not set or set to item count  
**Solution:** Set `VisibleSegmentsCount` to value less than `ItemsSource.Count`

### Uneven Segment Widths (When Not Desired)

**Cause:** Per-item Width set on some items  
**Solution:** Remove Width property from SfSegmentItem objects, or set uniform SegmentWidth

### Height Not Applying

**Cause:** Container constraints override SegmentHeight  
**Solution:** Ensure parent layout allows specified height; use `VerticalOptions="Center"`

## Next Steps

- **Customize appearance:** See [customization.md](customization.md) for colors and styles
- **Configure selection:** See [selection.md](selection.md) for selection indicators
- **Handle events:** See [events.md](events.md) for user interactions
