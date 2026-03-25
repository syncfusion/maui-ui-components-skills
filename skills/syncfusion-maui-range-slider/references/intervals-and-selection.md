# Intervals and Selection in .NET MAUI Range Slider

## Table of Contents
- [Overview](#overview)
- [Interval Configuration](#interval-configuration)
  - [Numeric Interval](#numeric-interval)
  - [Auto Interval](#auto-interval)
- [Discrete Selection](#discrete-selection)
  - [StepSize Property](#stepsize-property)
- [Selection Behavior](#selection-behavior)
  - [EnableIntervalSelection](#enableintervalselection)
  - [DragBehavior](#dragbehavior)
- [Deferred Updates](#deferred-updates)
  - [EnableDeferredUpdate](#enabledeferredupdate)
  - [DeferredUpdateDelay](#deferredupdatedelay)
- [Common Scenarios](#common-scenarios)
- [Best Practices](#best-practices)
- [Related References](#related-references)

## Overview

The .NET MAUI Range Slider (`SfRangeSlider`) provides comprehensive control over intervals and selection behavior. Intervals determine where visual elements (labels, ticks, dividers) appear, while selection properties control how users interact with the thumbs. This reference covers all aspects of configuring intervals and selection behavior.

## Interval Configuration

### Numeric Interval

The `Interval` property defines the spacing between visual elements like labels, ticks, and dividers.

**Type:** `double`  
**Default:** `0` (auto-calculated)

Range Slider elements are rendered based on `Interval`, `Minimum`, and `Maximum` properties working together.

**XAML Example:**
```xaml
<sliders:SfRangeSlider Minimum="0"
                       Maximum="10"
                       RangeStart="2"
                       RangeEnd="8"
                       Interval="2"
                       ShowLabels="True"
                       ShowTicks="True"
                       ShowDividers="True" />
```

**C# Example:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    Minimum = 0,
    Maximum = 10,
    RangeStart = 2,
    RangeEnd = 8,
    Interval = 2,
    ShowLabels = true,
    ShowTicks = true,
    ShowDividers = true
};
```

**How It Works:**
- With `Minimum` = 0, `Maximum` = 10, and `Interval` = 2
- Labels, ticks, and dividers render at: 0, 2, 4, 6, 8, 10

**Decimal Intervals:**
```xaml
<sliders:SfRangeSlider Minimum="0"
                       Maximum="1"
                       RangeStart="0.2"
                       RangeEnd="0.8"
                       Interval="0.25"
                       ShowLabels="True"
                       ShowTicks="True" />
```

### Auto Interval

When `Interval` is `0` but visual elements are enabled (`ShowTicks`, `ShowLabels`, or `ShowDividers` is `True`), the interval is automatically calculated based on available space.

**XAML Example:**
```xaml
<sliders:SfRangeSlider Minimum="0"
                       Maximum="10"
                       RangeStart="2"
                       RangeEnd="8"
                       ShowLabels="True"
                       ShowTicks="True"
                       ShowDividers="True" />
```

**C# Example:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    Minimum = 0,
    Maximum = 10,
    RangeStart = 2,
    RangeEnd = 8,
    ShowLabels = true,
    ShowTicks = true,
    ShowDividers = true
    // Interval = 0 (default, auto-calculated)
};
```

**Auto-Calculation Behavior:**
- Calculates optimal interval based on slider width
- Prevents label/tick overcrowding
- Adjusts dynamically on orientation changes

## Discrete Selection

### StepSize Property

The `StepSize` property enables discrete (stepped) thumb movement instead of continuous sliding.

**Type:** `double`  
**Default:** `0` (continuous movement)

**XAML Example:**
```xaml
<sliders:SfRangeSlider Minimum="0"
                       Maximum="10"
                       RangeStart="2"
                       RangeEnd="8"
                       Interval="2"
                       StepSize="2"
                       ShowLabels="True"
                       ShowTicks="True"
                       ShowDividers="True" />
```

**C# Example:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    Minimum = 0,
    Maximum = 10,
    RangeStart = 2,
    RangeEnd = 8,
    Interval = 2,
    StepSize = 2,
    ShowLabels = true,
    ShowTicks = true,
    ShowDividers = true
};
```

**Behavior:**
- Thumbs snap to values at step intervals
- With `StepSize` = 2, thumbs can only be at 0, 2, 4, 6, 8, 10
- Provides tactile feedback for precise value selection

**Fine-Grained Steps:**
```xaml
<sliders:SfRangeSlider Minimum="0"
                       Maximum="100"
                       Interval="10"
                       StepSize="5"
                       ShowLabels="True"
                       ShowTicks="True" />
```

## Selection Behavior

### EnableIntervalSelection

The `EnableIntervalSelection` property controls whether tapping between thumbs moves both thumbs to the nearest interval.

**Type:** `bool`  
**Default:** `false`

**When `false`:**
- Tapping moves the nearest thumb to the tap position

**When `true`:**
- Tapping moves both thumbs to encompass the tapped interval
- Both thumbs snap to interval boundaries

**XAML Example:**
```xaml
<sliders:SfRangeSlider Minimum="0" 
                       Maximum="10"
                       RangeStart="2"
                       RangeEnd="8"
                       Interval="2"
                       ShowTicks="True"
                       ShowLabels="True"
                       EnableIntervalSelection="True" />
```

**C# Example:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    Minimum = 0,
    Maximum = 10,
    RangeStart = 2, 
    RangeEnd = 8,
    Interval = 2,        
    ShowLabels = true,
    ShowTicks = true,    
    EnableIntervalSelection = true
};
```

**Use Cases:**
- Selecting time slots in a schedule
- Choosing predefined ranges in a filter
- Quick interval-based selection without precise dragging

### DragBehavior

The `DragBehavior` property controls how thumbs can be moved.

**Type:** `SliderDragBehavior`  
**Default:** `SliderDragBehavior.OnThumb`

**Values:**
- `OnThumb` - Individual thumb movement only
- `BetweenThumbs` - Move both thumbs together, maintaining range width
- `Both` - Allow both individual and simultaneous thumb movement

#### OnThumb

Default behavior allowing independent thumb movement.

**XAML Example:**
```xaml
<sliders:SfRangeSlider Minimum="0" 
                       Maximum="100"
                       RangeStart="25"
                       RangeEnd="75"
                       Interval="25" 
                       ShowTicks="True"
                       ShowLabels="True"
                       DragBehavior="OnThumb" />
```

**C# Example:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    Minimum = 0,
    Maximum = 100,
    RangeStart = 25,
    RangeEnd = 75,
    Interval = 25, 
    ShowTicks = true,
    ShowLabels = true,  
    DragBehavior = SliderDragBehavior.OnThumb
};
```

#### BetweenThumbs

Allows moving both thumbs simultaneously without changing the range width.

**XAML Example:**
```xaml
<sliders:SfRangeSlider Minimum="0" 
                       Maximum="100"
                       RangeStart="25"
                       RangeEnd="75"
                       Interval="25" 
                       ShowTicks="True"
                       ShowLabels="True"
                       DragBehavior="BetweenThumbs" />
```

**C# Example:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    Minimum = 0,
    Maximum = 100,
    RangeStart = 25,
    RangeEnd = 75,
    Interval = 25, 
    ShowTicks = true,
    ShowLabels = true,   
    DragBehavior = SliderDragBehavior.BetweenThumbs
};
```

**Behavior:**
- Tap/drag between thumbs moves entire range
- Range width (distance between thumbs) remains constant
- Individual thumb movement not possible
- Drag area excludes thumb radius

**Use Cases:**
- Time window selection that maintains duration
- Moving a fixed-width filter range
- Sliding window operations

#### Both

Combines both behaviors - individual thumb movement and simultaneous range movement.

**XAML Example:**
```xaml
<sliders:SfRangeSlider Minimum="0" 
                       Maximum="100"
                       RangeStart="25"
                       RangeEnd="75"
                       Interval="25" 
                       ShowTicks="True"
                       ShowLabels="True"
                       DragBehavior="Both" />
```

**C# Example:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    Minimum = 0,
    Maximum = 100,
    RangeStart = 25,
    RangeEnd = 75,
    Interval = 25, 
    ShowTicks = true, 
    ShowLabels = true,  
    DragBehavior = SliderDragBehavior.Both
};
```

**Behavior:**
- Drag on thumb: moves that thumb individually
- Drag between thumbs: moves entire range together
- Maximum flexibility for users

## Deferred Updates

### EnableDeferredUpdate

The `EnableDeferredUpdate` property controls when dependent components are updated during continuous dragging.

**Type:** `bool`  
**Default:** `false`

**When `false`:**
- `ValueChanging` event fires continuously during drag
- Real-time updates

**When `true`:**
- Updates deferred until thumb held for `DeferredUpdateDelay` duration
- Reduces update frequency during active dragging
- Immediate update on touch release

**XAML Example:**
```xaml
<sliders:SfRangeSlider Minimum="0" 
                       Maximum="10"
                       RangeStart="2"
                       RangeEnd="8"
                       Interval="2"
                       ShowTicks="True"
                       ShowLabels="True"
                       EnableDeferredUpdate="True"
                       DeferredUpdateDelay="1000" />
```

**C# Example:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    Minimum = 0,
    Maximum = 10,
    RangeStart = 2, 
    RangeEnd = 8,
    Interval = 2,        
    ShowLabels = true,
    ShowTicks = true,    
    EnableDeferredUpdate = true,
    DeferredUpdateDelay = 1000
};
```

### DeferredUpdateDelay

The `DeferredUpdateDelay` property specifies the delay (in milliseconds) before triggering updates during deferred mode.

**Type:** `int`  
**Default:** `500` milliseconds

**XAML Example:**
```xaml
<sliders:SfRangeSlider EnableDeferredUpdate="True"
                       DeferredUpdateDelay="750" />
```

**C# Example:**
```csharp
rangeSlider.EnableDeferredUpdate = true;
rangeSlider.DeferredUpdateDelay = 750;
```

**Behavior:**
- Timer starts when thumb dragging begins
- After delay expires, `ValueChanging` event fires
- Timer resets if thumb continues moving
- On touch release, immediate update regardless of timer

**Use Cases:**
- Filtering large datasets (reduce filter recalculations)
- Live chart updates (smooth performance)
- API calls on value change (rate limiting)
- Expensive computations triggered by value changes

## Common Scenarios

### Price Range Filter
```xaml
<sliders:SfRangeSlider Minimum="0"
                       Maximum="1000"
                       RangeStart="100"
                       RangeEnd="500"
                       Interval="100"
                       StepSize="10"
                       NumberFormat="C0"
                       ShowLabels="True"
                       ShowTicks="True"
                       EnableDeferredUpdate="True"
                       DeferredUpdateDelay="500" />
```

### Time Slot Selection
```csharp
var timeSlider = new SfRangeSlider
{
    Minimum = 0,
    Maximum = 24,
    RangeStart = 9,
    RangeEnd = 17,
    Interval = 3,
    StepSize = 0.5,
    ShowLabels = true,
    ShowTicks = true,
    EnableIntervalSelection = true,
    NumberFormat = "0.0"
};
```

### Fixed-Width Window Selection
```xaml
<sliders:SfRangeSlider Minimum="0"
                       Maximum="100"
                       RangeStart="40"
                       RangeEnd="60"
                       Interval="10"
                       ShowLabels="True"
                       DragBehavior="BetweenThumbs" />
```

### Flexible Range with Performance Optimization
```xaml
<sliders:SfRangeSlider Minimum="0"
                       Maximum="1000"
                       Interval="100"
                       StepSize="5"
                       ShowLabels="True"
                       ShowTicks="True"
                       DragBehavior="Both"
                       EnableDeferredUpdate="True"
                       DeferredUpdateDelay="300" />
```

## Best Practices

1. **Interval and StepSize Relationship**:
   - `StepSize` can be smaller than `Interval` for fine control
   - `StepSize` should be divisible into the range for predictable behavior
   - Example: `Interval="10"` with `StepSize="2"` works well

2. **Auto Interval Guidelines**:
   - Use auto interval for dynamic layouts
   - Explicitly set interval for consistent appearance across devices
   - Test on different screen sizes if using auto interval

3. **Discrete Selection**:
   - Always set `StepSize` for discrete value selection
   - Match `StepSize` with data granularity
   - Combine with `ShowTicks` for visual feedback

4. **DragBehavior Selection**:
   - `OnThumb`: Most common, flexible range adjustment
   - `BetweenThumbs`: Fixed-width ranges, sliding windows
   - `Both`: Power users who need maximum control

5. **Deferred Updates**:
   - Enable for expensive operations (API calls, complex calculations)
   - Set delay between 300-1000ms based on operation cost
   - Don't use for simple UI updates (causes lag perception)

6. **EnableIntervalSelection**:
   - Use when intervals represent discrete categories
   - Combine with visible interval markers (ticks/dividers)
   - Not recommended with continuous data

7. **Performance Optimization**:
   - Reasonable interval count (10-20 max on mobile)
   - Use deferred updates for data-heavy operations
   - Avoid unnecessary event handlers

8. **User Experience**:
   - Provide visual feedback for discrete steps (ticks, haptics)
   - Clear indication of drag behavior capability
   - Consistent behavior across your app

## Related References

- [labels.md](./labels.md) - Label display based on intervals
- [ticks.md](./ticks.md) - Tick marks at intervals
- [dividers.md](./dividers.md) - Divider placement at intervals
- [events-and-commands.md](./events-and-commands.md) - ValueChanging events with deferred updates
- [thumbs-and-overlays.md](./thumbs-and-overlays.md) - Thumb interaction behavior
