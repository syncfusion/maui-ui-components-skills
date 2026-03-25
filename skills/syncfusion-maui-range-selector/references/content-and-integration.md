# Content and Chart Integration — SfRangeSelector (.NET MAUI)

## Table of Contents
- [The Content Property](#the-content-property)
- [Embedding SfCartesianChart](#embedding-sfcartesianchart)
- [ViewModel and Data Binding](#viewmodel-and-data-binding)
- [Full Example with SplineAreaSeries](#full-example-with-splineareaseries)
- [Tips for Other Chart Types](#tips-for-other-chart-types)

---

## The Content Property

`SfRangeSelector` inherits from `RangeSelectorBase` and exposes a `Content` property that accepts any `View`. This content is rendered inside the range selector — behind the track and thumbs — allowing users to visually see what they are filtering.

In practice, this is almost always a chart that represents the full dataset, while the thumbs let users select a subset.

```csharp
rangeSelector.Content = myChartOrView;
```

> **Key idea:** The range selector does not filter the chart data automatically. The chart is a visual backdrop. To actually filter data based on thumb position, listen to the `ValueChanged` event and filter your data source accordingly.

---

## Embedding SfCartesianChart

The most common pattern is embedding `SfCartesianChart` with hidden axes, so the chart looks like a clean area graph behind the selector.

**Why hide axes?** The range selector's own track/labels replace the chart's axis annotations. Showing chart axes would duplicate and clutter the UI.

**Recommended axis settings for embedded charts:**
```xaml
<charts:DateTimeAxis IsVisible="False" ShowMajorGridLines="False" />
<charts:NumericalAxis IsVisible="False" ShowMajorGridLines="False" />
```

**Minimal embedded chart (XAML):**
```xaml
<sliders:SfRangeSelector Minimum="10" Maximum="20"
                         RangeStart="13" RangeEnd="17">
    <charts:SfCartesianChart>

        <charts:SfCartesianChart.XAxes>
            <charts:DateTimeAxis IsVisible="False"
                                 ShowMajorGridLines="False" />
        </charts:SfCartesianChart.XAxes>

        <charts:SfCartesianChart.YAxes>
            <charts:NumericalAxis IsVisible="False"
                                  ShowMajorGridLines="False" />
        </charts:SfCartesianChart.YAxes>

        <charts:SfCartesianChart.Series>
            <charts:SplineAreaSeries ItemsSource="{Binding Source}"
                                     XBindingPath="X"
                                     YBindingPath="Y" />
        </charts:SfCartesianChart.Series>

    </charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

---

## ViewModel and Data Binding

Connect chart data through a ViewModel with `BindingContext`.

**ViewModel:**
```csharp
public class ViewModel
{
    public ObservableCollection<ChartData> Source { get; set; }

    public ViewModel()
    {
        Source = new ObservableCollection<ChartData>
        {
            new ChartData { X = new DateTime(2023, 1, 1), Y = 10 },
            new ChartData { X = new DateTime(2023, 2, 1), Y = 25 },
            new ChartData { X = new DateTime(2023, 3, 1), Y = 18 },
            new ChartData { X = new DateTime(2023, 4, 1), Y = 40 },
            new ChartData { X = new DateTime(2023, 5, 1), Y = 30 },
        };
    }
}

public class ChartData
{
    public DateTime X { get; set; }
    public double Y { get; set; }
}
```

**Binding in XAML:**
```xaml
<ContentPage xmlns:local="clr-namespace:MyApp">
    <ContentPage.BindingContext>
        <local:ViewModel />
    </ContentPage.BindingContext>

    <sliders:SfRangeSelector ...>
        <charts:SfCartesianChart>
            <charts:SfCartesianChart.Series>
                <charts:SplineAreaSeries ItemsSource="{Binding Source}"
                                         XBindingPath="X"
                                         YBindingPath="Y" />
            </charts:SfCartesianChart.Series>
        </charts:SfCartesianChart>
    </sliders:SfRangeSelector>
</ContentPage>
```

---

## Full Example with SplineAreaSeries

Complete page implementation with ViewModel, chart content, labels, and ticks:

**XAML:**
```xaml
<ContentPage
    xmlns:sliders="clr-namespace:Syncfusion.Maui.Sliders;assembly=Syncfusion.Maui.Sliders"
    xmlns:charts="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts"
    xmlns:local="clr-namespace:MyApp">

    <ContentPage.BindingContext>
        <local:ViewModel />
    </ContentPage.BindingContext>

    <sliders:SfRangeSelector Minimum="10"
                             Maximum="20"
                             RangeStart="13"
                             RangeEnd="17"
                             Interval="2"
                             ShowLabels="True"
                             ShowTicks="True">

        <charts:SfCartesianChart>

            <charts:SfCartesianChart.XAxes>
                <charts:DateTimeAxis IsVisible="False"
                                     ShowMajorGridLines="False" />
            </charts:SfCartesianChart.XAxes>

            <charts:SfCartesianChart.YAxes>
                <charts:NumericalAxis IsVisible="False"
                                      ShowMajorGridLines="False" />
            </charts:SfCartesianChart.YAxes>

            <charts:SfCartesianChart.Series>
                <charts:SplineAreaSeries ItemsSource="{Binding Source}"
                                         XBindingPath="X"
                                         YBindingPath="Y" />
            </charts:SfCartesianChart.Series>

        </charts:SfCartesianChart>

    </sliders:SfRangeSelector>

</ContentPage>
```

**C# equivalent:**
```csharp
SfRangeSelector rangeSelector = new SfRangeSelector();
rangeSelector.Minimum = 10;
rangeSelector.Maximum = 20;
rangeSelector.RangeStart = 13;
rangeSelector.RangeEnd = 17;

SfCartesianChart chart = new SfCartesianChart();

DateTimeAxis primaryAxis = new DateTimeAxis();
chart.XAxes = primaryAxis;

NumericalAxis secondaryAxis = new NumericalAxis();
chart.YAxes = secondaryAxis;

SplineAreaSeries series = new SplineAreaSeries();
series.ItemsSource = (new ViewModel()).Source;
series.XBindingPath = "X";
series.YBindingPath = "Y";
chart.Series.Add(series);

rangeSelector.Content = chart;
content = rangeSelector;
```

---

## Tips for Other Chart Types

You can embed any `SfCartesianChart` series type. Choose based on your data shape:

| Series Type | Best For |
|-------------|----------|
| `SplineAreaSeries` | Smooth continuous data (most common) |
| `AreaSeries` | Straight-edged area fill |
| `LineSeries` | Simple line without fill |
| `ColumnSeries` | Discrete category data |
| `StepAreaSeries` | Step-based distributions |

**Using ColumnSeries instead:**
```xaml
<charts:SfCartesianChart.Series>
    <charts:ColumnSeries ItemsSource="{Binding Source}"
                         XBindingPath="X"
                         YBindingPath="Y" />
</charts:SfCartesianChart.Series>
```

> **Reminder:** The embedded chart is purely visual — it doesn't drive the range selector's values. To use the selected range to filter data, handle the `ValueChanged` event on `SfRangeSelector` and update your data collection accordingly.
