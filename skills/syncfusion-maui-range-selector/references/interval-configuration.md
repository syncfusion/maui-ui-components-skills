# Interval Configuration — SfRangeSelector (.NET MAUI)

## Table of Contents
- [Overview](#overview)
- [Numeric Interval](#numeric-interval)
- [Auto Interval](#auto-interval)
- [Showing Ticks](#showing-ticks)
- [Showing Dividers](#showing-dividers)
- [Combining Interval Features](#combining-interval-features)

---

## Overview

The `Interval` property controls the spacing between labels, ticks, and dividers rendered along the `SfRangeSelector` track. These visual elements help users understand the numeric scale and select values accurately.

The three key properties that work together:
- `Minimum` — track start value
- `Maximum` — track end value
- `Interval` — step between rendered elements (0 = auto-calculated)

---

## Numeric Interval

When `Interval` is explicitly set, labels, major ticks, and dividers are rendered at every interval step from `Minimum` to `Maximum`.

**Example:** `Minimum=0`, `Maximum=10`, `Interval=2` → renders at 0, 2, 4, 6, 8, 10.

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="0"
                         Maximum="10"
                         RangeStart="2"
                         RangeEnd="8"
                         Interval="2"
                         ShowLabels="True"
                         ShowTicks="True"
                         ShowDividers="True">
    <charts:SfCartesianChart>
        ...
    </charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
SfRangeSelector rangeSelector = new SfRangeSelector();
rangeSelector.Minimum = 0;
rangeSelector.Maximum = 10;
rangeSelector.RangeStart = 2;
rangeSelector.RangeEnd = 8;
rangeSelector.Interval = 2;
rangeSelector.ShowLabels = true;
rangeSelector.ShowTicks = true;
rangeSelector.ShowDividers = true;
SfCartesianChart chart = new SfCartesianChart();
rangeSelector.Content = chart;
```

> **Default value:** `Interval = 0` (auto-calculated).

---

## Auto Interval

When `Interval` is `0` (its default) and `ShowLabels`, `ShowTicks`, or `ShowDividers` is `true`, the interval is automatically calculated based on the available rendering size.

This is useful when you want evenly distributed visual markers without hardcoding a step value.

**Example:** `Minimum=0`, `Maximum=10`, `Interval=0`, available size ≈ 350px → auto renders at 0, 2, 4, 6, 8, 10.

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="0"
                         Maximum="10"
                         RangeStart="2"
                         RangeEnd="8"
                         ShowLabels="True"
                         ShowTicks="True"
                         ShowDividers="True">
    <charts:SfCartesianChart>
        ...
    </charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
SfRangeSelector rangeSelector = new SfRangeSelector();
rangeSelector.Minimum = 0;
rangeSelector.Maximum = 10;
rangeSelector.RangeStart = 2;
rangeSelector.RangeEnd = 8;
rangeSelector.ShowLabels = true;
rangeSelector.ShowTicks = true;
rangeSelector.ShowDividers = true;
SfCartesianChart chart = new SfCartesianChart();
rangeSelector.Content = chart;
```

> **When to use auto interval:** When the component width varies (e.g., responsive layouts) and you want the interval to adapt automatically rather than produce overcrowded or sparse labels.

---

## Showing Ticks

Ticks are small markers that appear at each interval position. `MinorTicksPerInterval` adds smaller ticks between major ticks for finer visual granularity.

| Property | Type | Default | Purpose |
|----------|------|---------|---------|
| `ShowTicks` | bool | `false` | Show major ticks at each interval |
| `MinorTicksPerInterval` | int | `0` | Number of minor ticks between major ticks |

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="0"
                         Maximum="10"
                         RangeStart="2"
                         RangeEnd="8"
                         Interval="2"
                         ShowLabels="True"
                         ShowTicks="True"
                         MinorTicksPerInterval="1">
    <charts:SfCartesianChart>
        ...
    </charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
rangeSelector.ShowTicks = true;
rangeSelector.MinorTicksPerInterval = 1; // 1 minor tick between each major tick
```

> **Example:** With `Interval=2` and `MinorTicksPerInterval=1`, major ticks appear at 0, 2, 4, 6, 8, 10 and a minor tick appears at 1, 3, 5, 7, 9.

---

## Showing Dividers

Dividers are vertical markers on the track at each interval position. Unlike ticks (which appear below/above the track), dividers appear directly on the track line.

```xaml
<sliders:SfRangeSelector Minimum="0"
                         Maximum="10"
                         RangeStart="2"
                         RangeEnd="8"
                         Interval="2"
                         ShowDividers="True">
    <charts:SfCartesianChart>
        ...
    </charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

```csharp
rangeSelector.ShowDividers = true;
```

---

## Combining Interval Features

You can combine all interval-related features for a fully annotated range selector:

```xaml
<sliders:SfRangeSelector Minimum="0"
                         Maximum="10"
                         RangeStart="2"
                         RangeEnd="8"
                         Interval="2"
                         ShowLabels="True"
                         ShowTicks="True"
                         MinorTicksPerInterval="1"
                         ShowDividers="True">
    <charts:SfCartesianChart>
        ...
    </charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

This renders:
- Labels at 0, 2, 4, 6, 8, 10
- Major ticks at 0, 2, 4, 6, 8, 10
- Minor ticks at 1, 3, 5, 7, 9
- Dividers at 0, 2, 4, 6, 8, 10

> **Tip:** All three (`ShowLabels`, `ShowTicks`, `ShowDividers`) are independent — enable only the ones your UI needs to avoid visual clutter.
