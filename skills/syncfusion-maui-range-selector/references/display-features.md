# Display Features — SfRangeSelector (.NET MAUI)

## Table of Contents
- [Enabling Labels](#enabling-labels)
- [Enabling Ticks](#enabling-ticks)
- [Label Formatting with NumberFormat](#label-formatting-with-numberformat)
- [Inversed Layout](#inversed-layout)
- [Configuring RangeStart and RangeEnd](#configuring-rangestart-and-rangeend)

---

## Enabling Labels

Labels are value annotations rendered at each interval position along the track. Enable them with `ShowLabels="True"`. Labels only appear when `Interval` is set (or auto-calculated).

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="0"
                         Maximum="10"
                         RangeStart="2"
                         RangeEnd="8"
                         Interval="2"
                         ShowLabels="True">
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
SfCartesianChart chart = new SfCartesianChart();
rangeSelector.Content = chart;
```

> **Default:** `ShowLabels = false`. Labels are rendered below the track by default.

---

## Enabling Ticks

Ticks are small tick marks at each major interval. Minor ticks provide additional marks between majors for finer granularity.

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
rangeSelector.MinorTicksPerInterval = 1;
```

| Property | Default | Description |
|----------|---------|-------------|
| `ShowTicks` | `false` | Render major tick marks at each interval |
| `MinorTicksPerInterval` | `0` | Minor ticks between each pair of major ticks |

---

## Label Formatting with NumberFormat

Use the `NumberFormat` property to add a prefix or suffix to all rendered labels. This accepts standard .NET numeric format strings.

**Common formats:**

| `NumberFormat` | Input Value | Rendered Label |
|----------------|-------------|----------------|
| `"$#"` | 40 | `$40` |
| `"#%"` | 0.5 | `50%` |
| `"#.0"` | 4 | `4.0` |
| `"C"` | 100 | `$100.00` (culture-aware) |

**XAML — Dollar prefix:**
```xaml
<sliders:SfRangeSelector Minimum="20"
                         Maximum="100"
                         RangeStart="40"
                         RangeEnd="80"
                         Interval="20"
                         ShowLabels="True"
                         ShowTicks="True"
                         NumberFormat="$#">
    <charts:SfCartesianChart>
        ...
    </charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
SfRangeSelector rangeSelector = new SfRangeSelector();
rangeSelector.Minimum = 20;
rangeSelector.Maximum = 100;
rangeSelector.RangeStart = 40;
rangeSelector.RangeEnd = 80;
rangeSelector.ShowLabels = true;
rangeSelector.ShowTicks = true;
rangeSelector.Interval = 20;
rangeSelector.NumberFormat = "$#";
SfCartesianChart chart = new SfCartesianChart();
rangeSelector.Content = chart;
```

> **Note:** `NumberFormat` applies to ALL labels on the track, including the min and max edge labels.

---

## Inversed Layout

Set `IsInversed="True"` to reverse the track direction — the maximum value appears on the left and minimum on the right. Useful for right-to-left reading contexts or specific UX patterns.

**Default:** `IsInversed = false` (left = min, right = max).

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="0"
                         Maximum="10"
                         RangeStart="2"
                         RangeEnd="8"
                         Interval="2"
                         ShowLabels="True"
                         ShowTicks="True"
                         MinorTicksPerInterval="1"
                         IsInversed="True">
    <charts:SfCartesianChart>
        ...
    </charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
rangeSelector.IsInversed = true;
```

> **Use case:** Price-descending sliders ("from high to low"), RTL UIs, or reversed chart axes where data flows right-to-left.

---

## Configuring RangeStart and RangeEnd

`RangeStart` and `RangeEnd` define the initial selected range between the two thumbs. These can be set statically or bound to a ViewModel for two-way interaction.

**Static values:**
```xaml
<sliders:SfRangeSelector Minimum="0"
                         Maximum="100"
                         RangeStart="25"
                         RangeEnd="75" />
```

**ViewModel binding (two-way):**
```xaml
<sliders:SfRangeSelector Minimum="0"
                         Maximum="100"
                         RangeStart="{Binding StartValue, Mode=TwoWay}"
                         RangeEnd="{Binding EndValue, Mode=TwoWay}" />
```

```csharp
public class ViewModel : INotifyPropertyChanged
{
    private double _startValue = 25;
    private double _endValue = 75;

    public double StartValue
    {
        get => _startValue;
        set { _startValue = value; OnPropertyChanged(); }
    }

    public double EndValue
    {
        get => _endValue;
        set { _endValue = value; OnPropertyChanged(); }
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged([CallerMemberName] string name = null)
        => PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

> **Constraint:** `RangeStart` must be ≥ `Minimum` and ≤ `RangeEnd`. `RangeEnd` must be ≤ `Maximum`. Violating these constraints may produce undefined visual behavior.
