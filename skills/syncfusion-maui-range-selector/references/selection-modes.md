# Selection Modes

This guide covers the different selection modes and behaviors available in the .NET MAUI Range Selector (SfRangeSelector).

## Table of Contents
- [Overview](#overview)
- [Discrete Selection](#discrete-selection)
- [Interval Selection](#interval-selection)
- [Drag Behaviors](#drag-behaviors)
- [Deferred Update](#deferred-update)
- [Best Practices](#best-practices)

## Overview

The Range Selector supports multiple selection modes that control how users interact with the thumbs:
- **Discrete Selection**: Move thumbs in fixed steps
- **Interval Selection**: Move both thumbs to clicked interval
- **Drag Behaviors**: Control thumb dragging modes
- **Deferred Update**: Optimize performance during continuous dragging

## Discrete Selection

Move thumbs in discrete steps using the `StepSize` property. Instead of continuous movement, thumbs snap to specific values.

**Property:**
- `StepSize` (double): Step value for thumb movement. Default: `0` (continuous)

**Use Cases:**
- Integer-only selection
- Predefined value steps (e.g., multiples of 5)
- Simplified user input

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="0"
                         Maximum="10"
                         RangeStart="2"
                         RangeEnd="8"
                         Interval="2"
                         StepSize="2"
                         ShowLabels="True"
                         ShowTicks="True"
                         ShowDividers="True">
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
SfRangeSelector rangeSelector = new SfRangeSelector
{
    Minimum = 0,
    Maximum = 10,
    RangeStart = 2,
    RangeEnd = 8,
    Interval = 2,
    StepSize = 2,  // Thumbs move in steps of 2
    ShowLabels = true,
    ShowTicks = true,
    ShowDividers = true
};
```

**Behavior:** When dragging, thumbs snap to values: 0, 2, 4, 6, 8, 10 (not 1, 3, 5, etc.)

**Integer-Only Selection:**
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="100"
                         StepSize="1" ... />
<!-- Allows only whole numbers -->
```

## Interval Selection

Move both thumbs simultaneously to the tapped interval using the `EnableIntervalSelection` property.

**Property:**
- `EnableIntervalSelection` (bool): Enable interval selection mode. Default: `false`

**Behavior:**
- **false** (default): Tapping moves the nearest thumb to tap position
- **true**: Tapping moves both thumbs to span the clicked interval

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="0"
                         Maximum="10"
                         RangeStart="2"
                         RangeEnd="8"
                         Interval="2"
                         ShowTicks="True"
                         ShowLabels="True"
                         EnableIntervalSelection="True">
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
SfRangeSelector rangeSelector = new SfRangeSelector
{
    Minimum = 0,
    Maximum = 10,
    RangeStart = 2,
    RangeEnd = 8,
    Interval = 2,
    ShowTicks = true,
    ShowLabels = true,
    EnableIntervalSelection = true
};
```

**Example:**
- Interval = 2, Range = 2-8
- User taps between 4 and 6
- Result: RangeStart = 4, RangeEnd = 6

**Use Cases:**
- Quick interval selection
- Selecting predefined ranges
- Time slot selection

## Drag Behaviors

Control how thumbs respond to dragging using the `DragBehavior` property.

**Property:**
- `DragBehavior` (SliderDragBehavior): Drag mode. Default: `OnThumb`

**Values:**
- `OnThumb`: Drag individual thumbs (default)
- `BetweenThumbs`: Drag entire range (both thumbs move together)
- `Both`: Support individual thumb drag AND range drag

### OnThumb (Default)

Each thumb can be dragged independently.

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="100"
                         RangeStart="25" RangeEnd="75"
                         DragBehavior="OnThumb">
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**Behavior:**
- Drag start thumb → Only start thumb moves
- Drag end thumb → Only end thumb moves
- Cannot drag the range itself

### BetweenThumbs

Drag the entire range without changing its width.

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="100"
                         RangeStart="25" RangeEnd="75"
                         Interval="25"
                         ShowTicks="True"
                         ShowLabels="True"
                         EdgeLabelsPlacement="Inside"
                         DragBehavior="BetweenThumbs">
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**Behavior:**
- Drag between thumbs → Both thumbs move together
- Range width (RangeEnd - RangeStart) remains constant
- Cannot drag individual thumbs

**Use Cases:**
- Moving a fixed-width time window
- Sliding a constant price range
- Maintaining range size while exploring data

### Both

Support both individual thumb dragging AND range dragging.

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="100"
                         RangeStart="25" RangeEnd="75"
                         Interval="25"
                         ShowTicks="True"
                         ShowLabels="True"
                         EdgeLabelsPlacement="Inside"
                         DragBehavior="Both">
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**Behavior:**
- Drag thumbs → Individual thumb movement
- Drag between thumbs → Both thumbs move together
- Maximum flexibility

**Use Cases:**
- Advanced user interfaces
- Flexible range adjustment
- Combined fixed and variable range selection

### Comparison Table

| Feature | OnThumb | BetweenThumbs | Both |
|---------|---------|---------------|------|
| Individual thumb drag | ✅ Yes | ❌ No | ✅ Yes |
| Range drag | ❌ No | ✅ Yes | ✅ Yes |
| Range width | Variable | Fixed | Variable |
| Complexity | Simple | Simple | Advanced |

## Deferred Update

Optimize performance during continuous dragging using deferred updates.

**Properties:**
- `EnableDeferredUpdate` (bool): Enable deferred mode. Default: `false`
- `DeferredUpdateDelay` (int): Delay in milliseconds. Default: `500`

**Behavior:**
- **Normal**: `ValueChanging` fires continuously during drag
- **Deferred**: `ValueChanging` fires only after delay or drag completion

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="0"
                         Maximum="10"
                         RangeStart="2"
                         RangeEnd="8"
                         Interval="2"
                         ShowTicks="True"
                         ShowLabels="True"
                         EnableDeferredUpdate="True"
                         DeferredUpdateDelay="1000">
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
SfRangeSelector rangeSelector = new SfRangeSelector
{
    Minimum = 0,
    Maximum = 10,
    RangeStart = 2,
    RangeEnd = 8,
    Interval = 2,
    ShowTicks = true,
    ShowLabels = true,
    EnableDeferredUpdate = true,
    DeferredUpdateDelay = 1000  // 1 second delay
};
```

**When to Use:**
- Expensive operations on value change (e.g., database queries)
- Real-time chart updates with large datasets
- Network requests triggered by range changes

**Event Behavior:**
```csharp
rangeSelector.ValueChanging += (s, e) =>
{
    // With deferred update:
    // - Fires after 1 second of continuous dragging
    // - OR when user releases thumb (immediate)
    
    // Without deferred update:
    // - Fires continuously during drag
};
```

## Best Practices

### Discrete Selection
- Set `StepSize` to match your `Interval` for aligned ticks
- Use `StepSize="1"` for integer-only ranges
- Combine with `ShowDividers` to visualize steps

### Interval Selection
- Best with visible intervals (`ShowTicks` or `ShowDividers`)
- Requires `Interval > 0`
- Clear for time slot or segment selection

### Drag Behaviors
- **OnThumb**: Default for most use cases
- **BetweenThumbs**: For fixed-width range exploration
- **Both**: For power users needing flexibility

### Deferred Update
- Use for performance-sensitive scenarios
- Set delay based on operation cost:
  - Fast operations (UI updates): 300-500ms
  - Medium operations (API calls): 1000-1500ms
  - Slow operations (complex queries): 2000+ms
- Always handle final `ValueChanged` event

### Combined Usage

**Price Range with Discrete Steps:**
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="1000"
                         RangeStart="200" RangeEnd="800"
                         Interval="100"
                         StepSize="50"
                         NumberFormat="$#,0"
                         ShowLabels="True" ShowTicks="True" />
<!-- Moves in $50 increments, shows $100 labels -->
```

**Time Range with Interval Selection:**
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="24"
                         Interval="6"
                         EnableIntervalSelection="True"
                         ShowLabels="True" ShowTicks="True" />
<!-- Select 6-hour time blocks -->
```

**Flexible Range with Deferred Updates:**
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="10000"
                         DragBehavior="Both"
                         EnableDeferredUpdate="True"
                         DeferredUpdateDelay="800" />
<!-- Flexible dragging with optimized updates -->
```
