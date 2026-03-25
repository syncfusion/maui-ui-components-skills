# Interval and Formatting in DateTime Range Selector

## Table of Contents
- [Overview](#overview)
- [Date Interval Configuration](#date-interval-configuration)
- [Interval Types](#interval-types)
- [Auto Interval Calculation](#auto-interval-calculation)
- [Date Format Patterns](#date-format-patterns)
- [Common Interval Scenarios](#common-interval-scenarios)

## Overview

The DateTime Range Selector renders labels, ticks, and dividers based on the `Interval`, `IntervalType`, `Minimum`, and `Maximum` properties. These properties work together to create meaningful time divisions for your data visualization.

## Date Interval Configuration

The `Interval` property defines the spacing between major elements (labels, ticks, dividers) on the track. When combined with `IntervalType`, it creates date-based intervals.

### Basic Interval Setup

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2000-01-01" 
                                 Maximum="2005-01-01"  
                                 RangeStart="2001-01-01" 
                                 RangeEnd="2004-01-01"
                                 Interval="1"
                                 IntervalType="Years" 
                                 DateFormat="yyyy"  
                                 ShowLabels="True" 
                                 ShowTicks="True" 
                                 ShowDividers="True">
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

```csharp
SfDateTimeRangeSelector rangeSelector = new SfDateTimeRangeSelector();
rangeSelector.Minimum = new DateTime(2000, 01, 01);
rangeSelector.Maximum = new DateTime(2005, 01, 01);
rangeSelector.RangeStart = new DateTime(2001, 01, 01); 
rangeSelector.RangeEnd = new DateTime(2004, 01, 01);            
rangeSelector.Interval = 1;
rangeSelector.IntervalType = SliderDateIntervalType.Years;
rangeSelector.DateFormat = "yyyy";
rangeSelector.ShowLabels = true;
rangeSelector.ShowTicks = true;
rangeSelector.ShowDividers = true;
```

**Result:** Renders labels, ticks, and dividers at 2000, 2001, 2002, 2003, 2004, 2005.

## Interval Types

The `IntervalType` property determines the unit of time for intervals:

### Years

```xaml
<sliders:SfDateTimeRangeSelector IntervalType="Years" Interval="2" />
```

Displays intervals in years (e.g., 2020, 2022, 2024).

### Months

```xaml
<sliders:SfDateTimeRangeSelector IntervalType="Months" 
                                 Interval="3" 
                                 DateFormat="MMM yyyy" />
```

Displays intervals in months (e.g., Jan 2024, Apr 2024, Jul 2024).

### Days

```xaml
<sliders:SfDateTimeRangeSelector IntervalType="Days" 
                                 Interval="7" 
                                 DateFormat="MMM dd" />
```

Displays intervals in days (e.g., weekly intervals).

### Hours

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2024-01-01T00:00:00"
                                 Maximum="2024-01-01T24:00:00"
                                 IntervalType="Hours" 
                                 Interval="4" 
                                 DateFormat="h tt" />
```

Displays intervals in hours (e.g., 12 AM, 4 AM, 8 AM).

### Minutes

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2024-01-01T08:00:00"
                                 Maximum="2024-01-01T09:00:00"
                                 IntervalType="Minutes" 
                                 Interval="15" 
                                 DateFormat="h:mm tt" />
```

Displays intervals in minutes (e.g., 8:00 AM, 8:15 AM, 8:30 AM).

### Seconds

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2024-01-01T08:00:00"
                                 Maximum="2024-01-01T08:01:00"
                                 IntervalType="Seconds" 
                                 Interval="10" 
                                 DateFormat="h:mm:ss tt" />
```

Displays intervals in seconds (e.g., 10-second intervals).

## Auto Interval Calculation

When `Interval` is set to `0`, the control automatically calculates appropriate intervals based on available space and the date range.

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2000-01-01" 
                                 Maximum="2005-01-01"  
                                 RangeStart="2001-01-01" 
                                 RangeEnd="2004-01-01"
                                 Interval="0"
                                 ShowLabels="True" 
                                 ShowTicks="True" 
                                 ShowDividers="True">
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

```csharp
rangeSelector.Interval = 0; // Auto-calculate
rangeSelector.ShowLabels = true;
rangeSelector.ShowTicks = true;
rangeSelector.ShowDividers = true;
```

**How it works:**
- If `Interval = 0` and `ShowLabels`, `ShowTicks`, or `ShowDividers` is `True`, the interval is automatically calculated
- If `IntervalType` or `DateFormat` is not set, they are also calculated automatically
- Example: For Minimum=2000-01-01 and Maximum=2001-01-01, with 350px width, it might render labels at Jan 01, Jul 01, Nov 01

## Date Format Patterns

The `DateFormat` property uses standard .NET DateTime format strings to control label display.

### Common Format Patterns

| Format | Example Output | Use Case |
|--------|---------------|----------|
| `yyyy` | 2024 | Years only |
| `MMM` | Jan | Month abbreviation |
| `MMMM` | January | Full month name |
| `MMM yyyy` | Jan 2024 | Month and year |
| `MM/dd/yyyy` | 01/15/2024 | Full date |
| `dd MMM` | 15 Jan | Day and month |
| `ddd` | Mon | Day of week abbreviation |
| `dddd` | Monday | Full day of week |
| `h tt` | 2 PM | 12-hour time |
| `HH:mm` | 14:30 | 24-hour time |
| `h:mm tt` | 2:30 PM | 12-hour time with minutes |
| `HH:mm:ss` | 14:30:45 | 24-hour time with seconds |

### Examples with Different Formats

**Hourly Format:**
```xaml
<sliders:SfDateTimeRangeSelector Minimum="2000-01-01T09:00:00" 
                                 Maximum="2000-01-01T17:00:00" 
                                 RangeStart="2000-01-01T11:00:00" 
                                 RangeEnd="2000-01-01T15:00:00" 
                                 IntervalType="Hours" 
                                 Interval="2" 
                                 DateFormat="h tt"
                                 ShowLabels="True">
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

**Result:** 9 AM, 11 AM, 1 PM, 3 PM, 5 PM

**Monthly Format:**
```xaml
<sliders:SfDateTimeRangeSelector Minimum="2010-01-01"
                                 Maximum="2011-01-01"
                                 RangeStart="2010-04-01"
                                 RangeEnd="2010-10-01"
                                 DateFormat="MMM"
                                 IntervalType="Months"
                                 Interval="2"
                                 ShowLabels="True">
    <charts:SfCartesianChart>
        <!-- Chart content -->
    </charts:SfCartesianChart>
</sliders:SfDateTimeRangeSelector>
```

**Result:** Jan, Mar, May, Jul, Sep, Nov

## Common Interval Scenarios

### Scenario 1: Yearly Financial Data (5-Year Range)

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2020-01-01" 
                                 Maximum="2025-01-01"
                                 RangeStart="2021-01-01" 
                                 RangeEnd="2024-01-01"
                                 Interval="1"
                                 IntervalType="Years"
                                 DateFormat="yyyy"
                                 ShowLabels="True"
                                 ShowTicks="True">
    <!-- Chart content -->
</sliders:SfDateTimeRangeSelector>
```

### Scenario 2: Monthly Sales Reports (1-Year Range)

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2024-01-01" 
                                 Maximum="2024-12-31"
                                 RangeStart="2024-03-01" 
                                 RangeEnd="2024-09-01"
                                 Interval="1"
                                 IntervalType="Months"
                                 DateFormat="MMM"
                                 ShowLabels="True"
                                 ShowTicks="True">
    <!-- Chart content -->
</sliders:SfDateTimeRangeSelector>
```

### Scenario 3: Weekly Activity Log (3-Month Range)

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2024-01-01" 
                                 Maximum="2024-03-31"
                                 RangeStart="2024-01-15" 
                                 RangeEnd="2024-03-15"
                                 Interval="7"
                                 IntervalType="Days"
                                 DateFormat="MMM dd"
                                 ShowLabels="True"
                                 ShowTicks="True">
    <!-- Chart content -->
</sliders:SfDateTimeRangeSelector>
```

### Scenario 4: Hourly Production Data (24-Hour Range)

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2024-01-01T00:00:00" 
                                 Maximum="2024-01-02T00:00:00"
                                 RangeStart="2024-01-01T06:00:00" 
                                 RangeEnd="2024-01-01T18:00:00"
                                 Interval="3"
                                 IntervalType="Hours"
                                 DateFormat="h tt"
                                 ShowLabels="True"
                                 ShowTicks="True">
    <!-- Chart content -->
</sliders:SfDateTimeRangeSelector>
```

### Scenario 5: Minute-by-Minute Monitoring (1-Hour Range)

```xaml
<sliders:SfDateTimeRangeSelector Minimum="2024-01-01T08:00:00" 
                                 Maximum="2024-01-01T09:00:00"
                                 RangeStart="2024-01-01T08:15:00" 
                                 RangeEnd="2024-01-01T08:45:00"
                                 Interval="10"
                                 IntervalType="Minutes"
                                 DateFormat="h:mm"
                                 ShowLabels="True"
                                 ShowTicks="True">
    <!-- Chart content -->
</sliders:SfDateTimeRangeSelector>
```

## Best Practices

1. **Match Interval to Data Granularity**: Use years for multi-year data, months for annual data, days for monthly data, etc.
2. **Choose Readable Formats**: Ensure the date format provides enough context without overcrowding (e.g., "MMM" for monthly data, not "MM/dd/yyyy")
3. **Use Auto Interval**: Set `Interval="0"` when unsure about optimal spacing—the control calculates appropriate intervals
4. **Consider Label Overlap**: Increase interval or use shorter date formats if labels overlap
5. **Test Different Sizes**: Date formatting may need adjustment for different screen sizes

## Troubleshooting

**Issue:** Labels show as numbers instead of dates
- **Solution:** Ensure `DateFormat` is set (e.g., `DateFormat="yyyy"`)

**Issue:** Labels are too crowded
- **Solution:** Increase `Interval` value or use auto interval (`Interval="0"`)

**Issue:** Labels don't show expected format
- **Solution:** Verify `IntervalType` matches your format (e.g., use `IntervalType="Months"` with `DateFormat="MMM"`)

**Issue:** Auto interval doesn't work
- **Solution:** Ensure `Interval="0"` and at least one of `ShowLabels`, `ShowTicks`, or `ShowDividers` is `True`

## Related Properties

- `MinorTicksPerInterval` - Adds minor ticks between intervals
- `ShowDividers` - Shows vertical dividers at each interval
- `LabelsPlacement` - Controls label positioning relative to ticks
- `EdgeLabelsPlacement` - Controls first/last label positioning
