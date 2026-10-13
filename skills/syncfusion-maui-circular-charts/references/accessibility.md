# Accessibility

## Table of Contents
- [Overview](#overview)
- [Circular Chart Elements and Announcements](#circular-chart-elements-and-announcements)
- [Enable Accessibility](#enable-accessibility)
- [Series Compatibility](#series-compatibility)
- [Keyboard Navigation](#keyboard-navigation)
- [Gesture Navigation](#gesture-navigation)
- [Providing Meaningful Labels](#providing-meaningful-labels)
- [Complete Example](#complete-example)

## Overview

The SfCircularChart includes built-in accessibility support to ensure that chart content is usable by everyone, including users who rely on screen readers (TalkBack on Android, VoiceOver on iOS and macOS, Narrator on Windows). Each chart element — title, legend items, and circular chart segments — is exposed to the platform's accessibility tree and announced when it receives focus.

Accessibility is enabled by default through semantic properties. No additional configuration is required to expose chart elements to screen readers; simply ensure that your chart data includes meaningful text for titles, legend items, and labels so that announcements are informative.

## Circular Chart Elements and Announcements

Screen readers announce chart elements using the following formats when an element receives focus.

| Element | Announcement Format | Example |
| --- | --- | --- |
| Chart Title | Title text | "Device Distribution" |
| Segment | Label + Value | "iPad 1500" |
| Series Legend Item | Legend label | "Sales" |

## Enable Accessibility

Enable the platform screen reader on the target device to start receiving chart announcements.

| Platform | Steps |
| --- | --- |
| Android | Settings → Accessibility → TalkBack → Toggle "On" |
| Windows | Settings → Accessibility → Turn on Narrator (or press Windows key + Enter) |
| iOS | Settings → Accessibility → VoiceOver → Toggle "On" |
| macOS | System Preferences → Accessibility → VoiceOver → Check "Enable VoiceOver" |

## Series Compatibility

Accessibility support for chart segment navigation and screen reader announcements is **not** available for `RadialBarSeries`. Individual data points in this series cannot receive accessibility focus, and their values are not announced by the screen reader.

Both `PieSeries` and `DoughnutSeries` support per-segment accessibility navigation and announcements.

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
- **Legend items:** The legend label for each series is announced; provide a meaningful `Label` on the series so the announcement is descriptive.
- **Segment values:** Set the `XBindingPath` and `YBindingPath` with meaningful underlying values (for example, a category name and a numeric value) so that segment announcements read naturally as "Label + Value".

```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.Title>
        <Label Text="Device Distribution"/>
    </chart:SfCircularChart.Title>

    <chart:SfCircularChart.Legend>
        <chart:ChartLegend/>
    </chart:SfCircularChart.Legend>

    <chart:PieSeries  ItemsSource="{Binding DeviceData}"
                      XBindingPath="Device"
                      YBindingPath="Count"
                      EnableTooltip="True">
        <chart:PieSeries.SelectionBehavior>
            <chart:DataPointSelectionBehavior SelectionBrush="#314A6E"/>
        </chart:PieSeries.SelectionBehavior>
    </chart:PieSeries>
</chart:SfCircularChart>
```

```csharp
SfCircularChart chart = new SfCircularChart
{
    Title = new Label() { Text = "Device Distribution" },
    Legend = new ChartLegend()
};

PieSeries series = new PieSeries
{
    ItemsSource = new ViewModel().DeviceData,
    XBindingPath = "Device",
    YBindingPath = "Count",
    EnableTooltip = true,
    SelectionBehavior = new DataPointSelectionBehavior
    {
        SelectionBrush = Color.FromArgb("#314A6E")
    }
};

chart.Series.Add(series);
this.Content = chart;
```

With this configuration, a screen reader announces the chart title as "Device Distribution", and a focused segment (for example, iPad with a value of 1500) is announced as "iPad 1500".

## Complete Example

The following example combines a descriptive chart title, legend, series labels, and tooltips to provide a fully accessible circular chart.

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:chart="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts"
             x:Class="AccessibilityDemo.MainPage">

    <chart:SfCircularChart>
        <chart:SfCircularChart.Title>
            <Label Text="Device Distribution"/>
        </chart:SfCircularChart.Title>

        <chart:SfCircularChart.Legend>
            <chart:ChartLegend ToggleSeriesVisibility="True"/>
        </chart:SfCircularChart.Legend>

        <chart:PieSeries  ItemsSource="{Binding DeviceData}"
                          XBindingPath="Device"
                          YBindingPath="Count"
                          EnableTooltip="True">
            <chart:PieSeries.SelectionBehavior>
                <chart:DataPointSelectionBehavior SelectionBrush="#314A6E"/>
            </chart:PieSeries.SelectionBehavior>
        </chart:PieSeries>

        <chart:DoughnutSeries Label="Expenses"
                               ItemsSource="{Binding DeviceData}"
                               XBindingPath="Device"
                               YBindingPath="Expense"
                               EnableTooltip="True">
            <chart:DoughnutSeries.SelectionBehavior>
                <chart:DataPointSelectionBehavior SelectionBrush="#7A4F9E"/>
            </chart:DoughnutSeries.SelectionBehavior>
        </chart:DoughnutSeries>
    </chart:SfCircularChart>
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
    public List<DeviceModel> DeviceData { get; set; }

    public ViewModel()
    {
        DeviceData = new List<DeviceModel>
        {
            new DeviceModel("iPad", 1500, 800),
            new DeviceModel("iPhone", 2500, 1200),
            new DeviceModel("MacBook", 1200, 700),
            new DeviceModel("AirPods", 900, 400)
        };
    }
}

public class DeviceModel
{
    public string Device { get; set; }
    public double Count { get; set; }
    public double Expense { get; set; }

    public DeviceModel(string device, double count, double expense)
    {
        Device = device;
        Count = count;
        Expense = expense;
    }
}
```