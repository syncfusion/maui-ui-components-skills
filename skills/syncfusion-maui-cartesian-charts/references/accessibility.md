# Accessibility

## Table of Contents
- [Overview](#overview)
- [Chart Elements and Announcements](#chart-elements-and-announcements)
- [Enable Accessibility](#enable-accessibility)
- [Series Compatibility](#series-compatibility)
- [Keyboard Navigation](#keyboard-navigation)
- [Gesture Navigation](#gesture-navigation)
- [Providing Meaningful Labels](#providing-meaningful-labels)
- [Complete Example](#complete-example)

## Overview

The SfCartesianChart includes built-in accessibility support to ensure that chart content is usable by everyone, including users who rely on screen readers (TalkBack on Android, VoiceOver on iOS and macOS, Narrator on Windows). Each chart element — header/title, legend items, data points, annotations, and trendlines — is exposed to the platform's accessibility tree and announced when it receives focus.

Accessibility is enabled by default through semantic properties. No additional configuration is required to expose chart elements to screen readers; simply ensure that your chart data includes meaningful text for titles, legend items, and labels so that announcements are informative.

## Chart Elements and Announcements

Screen readers announce chart elements using the following formats when an element receives focus.

| Element | Announcement Format | Example |
| --- | --- | --- |
| Chart Header/Title | Title text | "Q1 Sales Performance" |
| Series Legend Item | Series label | "Sales Revenue", "Profit", "Expenses" |
| Data Point (without series label) | X value + Y value | "January 2500" |
| Data Point (with series label) | X value + Y value + Series name | "January 2500 North Region" |
| Annotation (without label) | Annotation type | "Line annotation" |
| Annotation (with label) | Annotation type + Label | "Text annotation sales" |
| Trendline | Trendline type | "Power trendline" |

## Enable Accessibility

Enable the platform screen reader on the target device to start receiving chart announcements.

| Platform | Steps |
| --- | --- |
| Android | Settings → Accessibility → TalkBack → Toggle "On" |
| Windows | Settings → Accessibility → Turn on Narrator (or press Windows key + Enter) |
| iOS | Settings → Accessibility → VoiceOver → Toggle "On" |
| macOS | System Preferences → Accessibility → VoiceOver → Check "Enable VoiceOver" |

## Series Compatibility

Accessibility support for chart segment navigation and screen reader announcements is **not** available for the following series types. Individual data points in these series cannot receive accessibility focus, and their values are not announced by the screen reader.

- Fast-type series (`FastLineSeries`, `FastScatterSeries`, `FastColumnSeries`, `FastHistogramSeries`)
- Area-type series (`AreaSeries`, `SplineAreaSeries`, `StepAreaSeries`, `RangeAreaSeries`, `SplineRangeAreaSeries`, `100% Stacked Area`, `100% Stacked Spline Area`)
- `ErrorBarSeries`

All other Cartesian series types (Line, Column, Bar, Scatter, Bubble, Candle, OHLC, Stacked Column/Line, Range Column, Waterfall, etc.) support per-data-point accessibility navigation and announcements.

## Keyboard Navigation

### Windows (Narrator)

| Key Combination | Description |
| --- | --- |
| CapsLock + Right Arrow | Move to the next chart element |
| CapsLock + Left Arrow | Move to the previous chart element |
| CapsLock + Up Arrow | Announce the element text |
| Tab | Move focus to the next interactive element (buttons, checkboxes) |

### macOS (VoiceOver)

| Key Combination | Description |
| --- | --- |
| VO + Right Arrow | Move to the next element (VO is Control + Option) |
| VO + Left Arrow | Move to the previous element |
| VO + Up Arrow | Announce the element text |

## Gesture Navigation

### Android (TalkBack)

| Gesture | Description |
| --- | --- |
| Fling Right | Move to the next chart element |
| Fling Left | Move to the previous chart element |
| Single Tap | Move focus to an element |
| Double Tap | Activate the focused element (enable tooltip/selection) |

### iOS (VoiceOver)

| Gesture | Description |
| --- | --- |
| Swipe Right (one finger) | Move to the next element |
| Swipe Left (one finger) | Move to the previous element |
| Single Tap | Move focus to an element |
| Double Tap | Activate the focused element (enable tooltip/selection) |

## Providing Meaningful Labels

To make screen reader announcements informative, provide descriptive text for the chart elements that the screen reader announces.

- **Chart title:** Set the `Title` property (or `ChartTitle` text) so the chart header is announced.
- **Series labels:** Provide a `Label` for each series so legend items and data point announcements include the series name.
- **Data point values:** Use the `XBindingPath` and `YBindingPath` with meaningful underlying values (for example, a category name and a numeric value) so that data point announcements read naturally.
- **Annotation labels:** Set the `Label` on text and shape annotations so the annotation announcement includes descriptive text.
- **Trendlines:** Trendlines announce their type automatically; no additional label is required.

```xml
<chart:SfCartesianChart>
    <chart:SfCartesianChart.Title>
        <Label Text="Q1 Sales Performance"/>
    </chart:SfCartesianChart.Title>

    <chart:SfCartesianChart.Legend>
        <chart:ChartLegend/>
    </chart:SfCartesianChart.Legend>

    <chart:SfCartesianChart.XAxes>
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>

    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>

    <chart:ColumnSeries Label="North Region"
                        ItemsSource="{Binding SalesData}"
                        XBindingPath="Month"
                        YBindingPath="Revenue"
                        EnableTooltip="True">
        <chart:ColumnSeries.SelectionBehavior>
            <chart:DataPointSelectionBehavior SelectionBrush="#314A6E"/>
        </chart:ColumnSeries.SelectionBehavior>
    </chart:ColumnSeries>
</chart:SfCartesianChart>
```

```csharp
SfCartesianChart chart = new SfCartesianChart
{
    Title = new Label() { Text = "Q1 Sales Performance" },
    Legend = new ChartLegend()
};

CategoryAxis primaryAxis = new CategoryAxis();
chart.XAxes.Add(primaryAxis);

NumericalAxis secondaryAxis = new NumericalAxis();
chart.YAxes.Add(secondaryAxis);

ColumnSeries series = new ColumnSeries
{
    Label = "North Region",
    ItemsSource = new ViewModel().SalesData,
    XBindingPath = "Month",
    YBindingPath = "Revenue",
    EnableTooltip = true,
    SelectionBehavior = new DataPointSelectionBehavior
    {
        SelectionBrush = Color.FromArgb("#314A6E")
    }
};

chart.Series.Add(series);
this.Content = chart;
```

With this configuration, a screen reader announces the chart title as "Q1 Sales Performance", and a focused data point (for example, January with revenue 2500) is announced as "January 2500 North Region".

## Complete Example

The following example combines a descriptive chart title, legend, series labels, and tooltips to provide a fully accessible Cartesian chart.

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:chart="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts"
             x:Class="AccessibilityDemo.MainPage">

    <chart:SfCartesianChart>
        <chart:SfCartesianChart.Title>
            <Label Text="Q1 Sales Performance"/>
        </chart:SfCartesianChart.Title>

        <chart:SfCartesianChart.Legend>
            <chart:ChartLegend/>
        </chart:SfCartesianChart.Legend>

        <chart:SfCartesianChart.XAxes>
            <chart:CategoryAxis/>
        </chart:SfCartesianChart.XAxes>

        <chart:SfCartesianChart.YAxes>
            <chart:NumericalAxis/>
        </chart:SfCartesianChart.YAxes>

        <chart:ColumnSeries Label="North Region"
                            ItemsSource="{Binding SalesData}"
                            XBindingPath="Month"
                            YBindingPath="Revenue"
                            EnableTooltip="True">
            <chart:ColumnSeries.SelectionBehavior>
                <chart:DataPointSelectionBehavior SelectionBrush="#314A6E"/>
            </chart:ColumnSeries.SelectionBehavior>
        </chart:ColumnSeries>

        <chart:ColumnSeries Label="South Region"
                            ItemsSource="{Binding SalesData}"
                            XBindingPath="Month"
                            YBindingPath="Expense"
                            EnableTooltip="True">
            <chart:ColumnSeries.SelectionBehavior>
                <chart:DataPointSelectionBehavior SelectionBrush="#7A4F9E"/>
            </chart:ColumnSeries.SelectionBehavior>
        </chart:ColumnSeries>
    </chart:SfCartesianChart>
</ContentPage>
```

```csharp
using Syncfusion.Maui.Charts;

namespace AccessibilityDemo;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        BindingContext = new ViewModel();
    }
}

public class ViewModel
{
    public List<SalesModel> SalesData { get; set; }

    public ViewModel()
    {
        SalesData = new List<SalesModel>
        {
            new SalesModel("January", 2500, 1800),
            new SalesModel("February", 2700, 1900),
            new SalesModel("March", 3000, 2100),
            new SalesModel("April", 2800, 2000)
        };
    }
}

public class SalesModel
{
    public string Month { get; set; }
    public double Revenue { get; set; }
    public double Expense { get; set; }

    public SalesModel(string month, double revenue, double expense)
    {
        Month = month;
        Revenue = revenue;
        Expense = expense;
    }
}
```
