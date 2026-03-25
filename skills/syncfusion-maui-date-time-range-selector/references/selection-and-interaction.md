# Selection and Interaction in DateTime Range Selector

## Table of Contents
- [Overview](#overview)
- [Step Duration](#step-duration)
- [Interval Selection](#interval-selection)
- [Drag Behavior](#drag-behavior)
- [Deferred Update](#deferred-update)
- [Common Patterns](#common-patterns)

## Overview

The DateTime Range Selector provides several properties to control how users interact with the component:

- **StepDuration**: Time increment when tapping track to move thumbs
- **EnableIntervalSelection**: Allow tapping intervals to select entire period
- **DragBehavior**: Control how thumbs move when track is tapped/dragged
- **EnableDeferredUpdate**: Delay value change events until drag completes

## Step Duration

The `StepDuration` property controls the time increment when users tap on the track to move thumbs.

### Basic Usage

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2015-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2017-01-01" 
                                 RangeEnd="2018-01-01"
                                 StepDuration="1">
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

```csharp
rangeSelector.StepDuration = new SliderStepDuration(years: 1);
```

**Behavior:** When user taps the track, the nearest thumb moves by 1 year.

### StepDuration with Different Intervals

The step duration depends on the `IntervalType`:

```xaml
<!-- Yearly Steps -->
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01"
                                 IntervalType="Years"
                                 StepDuration="2">
    <charts:SfCartesianChart />
</sliders:SfDateTimeRangeSelector>
```

```xaml
<!-- Monthly Steps -->
<sliders:SfDateTimeRangeSelector Minimum="2023-01-01" 
                                 Maximum="2024-01-01"
                                 IntervalType="Months"
                                 StepDuration="1">
    <charts:SfCartesianChart />
</sliders:SfDateTimeRangeSelector>
```

```xaml
<!-- Daily Steps -->
<sliders:SfDateTimeRangeSelector Minimum="2023-01-01" 
                                 Maximum="2023-01-31"
                                 IntervalType="Days"
                                 StepDuration="7">
    <charts:SfCartesianChart />
</sliders:SfDateTimeRangeSelector>
```

```xaml
<!-- Hourly Steps -->
<sliders:SfDateTimeRangeSelector Minimum="2023-01-01T00:00:00" 
                                 Maximum="2023-01-01T23:59:59"
                                 IntervalType="Hours"
                                 StepDuration="2">
    <charts:SfCartesianChart />
</sliders:SfDateTimeRangeSelector>
```

### StepDuration in Code

```csharp
// Years
rangeSelector.StepDuration = new SliderStepDuration(years: 1);

// Months
rangeSelector.StepDuration = new SliderStepDuration(months: 3);

// Days
rangeSelector.StepDuration = new SliderStepDuration(days: 7);

// Hours
rangeSelector.StepDuration = new SliderStepDuration(hours: 4);

// Minutes
rangeSelector.StepDuration = new SliderStepDuration(minutes: 30);

// Seconds
rangeSelector.StepDuration = new SliderStepDuration(seconds: 15);
```

### Default Behavior

If `StepDuration` is not set, tapping the track moves the thumb to the exact tap position without snapping.

## Interval Selection

The `EnableIntervalSelection` property allows users to select an entire interval by tapping it.

### Basic Usage

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2016-01-01"
                                 Interval="2" 
                                 ShowLabels="True"
                                 ShowTicks="True"
                                 EnableIntervalSelection="True">
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

```csharp
rangeSelector.EnableIntervalSelection = true;
rangeSelector.Interval = 2;
```

**Behavior:** When user taps on an interval, both thumbs automatically position to select that entire interval.

### How It Works

With `EnableIntervalSelection="True"` and `Interval="2"` (years):
- Tap on 2010-2012 interval → `RangeStart = 2010`, `RangeEnd = 2012`
- Tap on 2012-2014 interval → `RangeStart = 2012`, `RangeEnd = 2014`
- Tap on 2014-2016 interval → `RangeStart = 2014`, `RangeEnd = 2016`

### Use Cases

**Monthly Reports:**
```xaml
<sliders:SfDateTimeRangeSelector Minimum="2023-01-01" 
                                 Maximum="2024-01-01"
                                 IntervalType="Months"
                                 Interval="1"
                                 EnableIntervalSelection="True">
    <charts:SfCartesianChart />
</sliders:SfDateTimeRangeSelector>
```
**Result:** Tap a month to select that entire month.

**Quarterly Selection:**
```xaml
<sliders:SfDateTimeRangeSelector Minimum="2023-01-01" 
                                 Maximum="2024-01-01"
                                 IntervalType="Months"
                                 Interval="3"
                                 EnableIntervalSelection="True">
    <charts:SfCartesianChart />
</sliders:SfDateTimeRangeSelector>
```
**Result:** Tap a quarter to select that entire 3-month period.

### Disabling Interval Selection

```xaml
<sliders:SfDateTimeRangeSelector EnableIntervalSelection="False">
    <charts:SfCartesianChart />
</sliders:SfDateTimeRangeSelector>
```

**Default:** `False` - Users must drag thumbs individually to adjust range.

## Drag Behavior

The `DragBehavior` property controls which thumb moves when the track is tapped or dragged.

### OnThumb (Default)

Only the dragged thumb moves:

```xaml
<sliders:SfDateTimeRangeSelector DragBehavior="OnThumb" />
```

```csharp
rangeSelector.DragBehavior = SliderDragBehavior.OnThumb;
```

**Behavior:** User must explicitly tap/drag each thumb to move it.

### BetweenThumbs

Tapping between thumbs moves the entire range:

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2016-01-01"
                                 DragBehavior="BetweenThumbs">
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

```csharp
rangeSelector.DragBehavior = SliderDragBehavior.BetweenThumbs;
```

**Behavior:** 
- Tap/drag between thumbs → Both thumbs move together (range size stays constant)
- Tap/drag on a thumb → That thumb moves individually

**Use Case:** Users need to shift the entire time window without changing its duration.

### Both

Tapping anywhere on the track moves the nearest thumb:

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2016-01-01"
                                 DragBehavior="Both">
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

```csharp
rangeSelector.DragBehavior = SliderDragBehavior.Both;
```

**Behavior:** 
- Tap left of start thumb → Start thumb moves to tap position
- Tap right of end thumb → End thumb moves to tap position
- Tap between thumbs → Nearest thumb moves to tap position

**Use Case:** Quick range adjustment with minimal targeting precision.

### Comparison

| DragBehavior | Tap on Track | Tap Between Thumbs | Tap on Thumb |
|--------------|--------------|---------------------|--------------|
| **OnThumb** | No effect | No effect | Moves that thumb |
| **BetweenThumbs** | No effect | Moves both thumbs | Moves that thumb |
| **Both** | Moves nearest thumb | Moves nearest thumb | Moves that thumb |

## Deferred Update

The `EnableDeferredUpdate` property delays value change events until the user finishes dragging.

### Enable Deferred Update

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2018-01-01"
                                 EnableDeferredUpdate="True"
                                 ValueChanged="OnValueChanged">
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

```csharp
rangeSelector.EnableDeferredUpdate = true;
```

**Behavior:**
- **False (default)**: `ValueChanged` event fires continuously during drag
- **True**: `ValueChanged` event fires only when drag completes

### Benefits

1. **Performance**: Reduces event processing during continuous drag
2. **Data Loading**: Prevents excessive data queries while user is adjusting
3. **Network Calls**: Limits API calls to final selection only

### Event Sequence

**With EnableDeferredUpdate = False:**
```
User starts drag → ValueChangeStart
User drags → ValueChanging (multiple times)
User drags → ValueChanged (multiple times)
User releases → ValueChangeEnd
```

**With EnableDeferredUpdate = True:**
```
User starts drag → ValueChangeStart
User drags → ValueChanging (multiple times)
User releases → ValueChanged (once)
User releases → ValueChangeEnd
```

### Use Case Example

**Expensive data queries:**
```csharp
private async void OnValueChanged(object sender, DateTimeRangeSelectorValueChangedEventArgs e)
{
    var start = (DateTime)e.NewRangeStart;
    var end = (DateTime)e.NewRangeEnd;
    
    // This query only runs once when drag completes
    await LoadDataForDateRange(start, end);
}
```

Without deferred update, this would run on every pixel dragged, causing performance issues.

## Common Patterns

### Pattern 1: Snap to Intervals

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2016-01-01"
                                 Interval="2"
                                 IntervalType="Years"
                                 StepDuration="2"
                                 ShowLabels="True"
                                 ShowTicks="True">
    <charts:SfCartesianChart />
</sliders:SfDateTimeRangeSelector>
```

**Result:** Thumbs snap to 2-year intervals when tapping or dragging.

### Pattern 2: Quick Interval Selection

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2023-01-01" 
                                 Maximum="2024-01-01" 
                                 RangeStart="2023-04-01" 
                                 RangeEnd="2023-07-01"
                                 IntervalType="Months"
                                 Interval="3"
                                 EnableIntervalSelection="True"
                                 ShowLabels="True">
    <charts:SfCartesianChart />
</sliders:SfDateTimeRangeSelector>
```

**Result:** Tap a quarter to instantly select that 3-month period.

### Pattern 3: Slide Range Window

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2020-01-01" 
                                 Maximum="2024-01-01" 
                                 RangeStart="2021-01-01" 
                                 RangeEnd="2022-01-01"
                                 DragBehavior="BetweenThumbs">
    <charts:SfCartesianChart />
</sliders:SfDateTimeRangeSelector>
```

**Result:** Drag the range area to shift the entire 1-year window forward/backward.

### Pattern 4: Performance Optimized

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2018-01-01"
                                 EnableDeferredUpdate="True"
                                 ValueChanged="OnValueChanged">
    <charts:SfCartesianChart />
</sliders:SfDateTimeRangeSelector>
```

```csharp
private async void OnValueChanged(object sender, DateTimeRangeSelectorValueChangedEventArgs e)
{
    var start = (DateTime)e.NewRangeStart;
    var end = (DateTime)e.NewRangeEnd;
    await LoadExpensiveData(start, end);
}
```

**Result:** Data loads only once after user completes selection.

### Pattern 5: Flexible Adjustment

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01" 
                                 Maximum="2020-01-01" 
                                 RangeStart="2012-01-01" 
                                 RangeEnd="2018-01-01"
                                 DragBehavior="Both"
                                 StepDuration="1">
    <charts:SfCartesianChart />
</sliders:SfDateTimeRangeSelector>
```

**Result:** Tap anywhere to move nearest thumb by 1-year steps.

### Pattern 6: Weekly Selection

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2023-01-01" 
                                 Maximum="2023-12-31" 
                                 RangeStart="2023-01-02" 
                                 RangeEnd="2023-01-08"
                                 IntervalType="Days"
                                 Interval="7"
                                 EnableIntervalSelection="True"
                                 StepDuration="7"
                                 ShowLabels="True">
    <charts:SfCartesianChart />
</sliders:SfDateTimeRangeSelector>
```

**Result:** 
- Tap a week to select entire 7-day period
- Manual adjustment snaps to weekly boundaries

## Best Practices

1. **Match StepDuration to Interval**: Use same value for consistent behavior
2. **Deferred Update for Heavy Operations**: Always use for database queries or API calls
3. **Interval Selection for Discrete Periods**: Enable for months, quarters, weeks
4. **BetweenThumbs for Fixed Ranges**: Use when range size should stay constant
5. **Both for Quick Adjustments**: Good for mobile where precise targeting is harder
6. **Combine StepDuration + Interval**: Ensures thumbs align with labels/ticks

## Troubleshooting

**Issue:** Thumbs don't snap to intervals
- **Solution:** Set `StepDuration` to match your `Interval` value

**Issue:** Can't select entire interval by tapping
- **Solution:** Enable `EnableIntervalSelection="True"` and set `Interval` value

**Issue:** Range shifts when I only want to move one thumb
- **Solution:** Change `DragBehavior` from `BetweenThumbs` to `OnThumb` (default)

**Issue:** Data loads too many times during drag
- **Solution:** Set `EnableDeferredUpdate="True"` to load data only after drag completes

**Issue:** Tapping track has no effect
- **Solution:** Set `DragBehavior="Both"` or use `EnableIntervalSelection="True"`

**Issue:** Range moves even when tapping outside thumbs
- **Solution:** Set `DragBehavior="OnThumb"` to require explicit thumb interaction

**Issue:** StepDuration doesn't work with EnableIntervalSelection
- **Solution:** These features work independently; `EnableIntervalSelection` takes precedence when tapping intervals

## Related Properties

- `Interval` - Spacing for labels, ticks, and interval selection
- `IntervalType` - Time unit (Years, Months, Days, Hours, Minutes, Seconds)
- `ValueChangeStart` / `ValueChanging` / `ValueChanged` / `ValueChangeEnd` - Events for tracking value changes
- `Minimum` / `Maximum` - Valid range boundaries
- `RangeStart` / `RangeEnd` - Current selected values
