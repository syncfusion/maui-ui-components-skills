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

The SfPolarChart includes built-in accessibility support to ensure that chart content is usable by everyone, including users who rely on screen readers (TalkBack on Android, VoiceOver on iOS and macOS, Narrator on Windows). Each chart element — the chart title, legend items, and data points — is exposed to the platform's accessibility tree and announced when it receives focus.

Accessibility is enabled by default through semantic properties. No additional configuration is required to expose chart elements to screen readers; simply ensure that your chart data includes meaningful text for titles, legend items, and labels so that announcements are informative.

## Chart Elements and Announcements

Screen readers announce chart elements using the following formats when an element receives focus.

| Element | Announcement Format | Example |
| --- | --- | --- |
| Chart Title | Title text | "Wind Speed Analysis" |
| Series Legend Item | Series label | "Morning Wind", "Evening Wind" |
| Data Point | XValue + YValue + Series label | "North 50 km/h Morning Wind" |

## Enable Accessibility

Enable the platform screen reader on the target device to start receiving chart announcements.

| Platform | Steps |
| --- | --- |
| Android | Settings → Accessibility → TalkBack → Toggle "On" |
| Windows | Settings → Accessibility → Turn on Narrator (or press Windows key + Enter) |
| iOS | Settings → Accessibility → VoiceOver → Toggle "On" |
| macOS | System Preferences → Accessibility → VoiceOver → Check "Enable VoiceOver" |

## Series Compatibility

Accessibility support for chart segment navigation and screen reader announcements is **not** available for `PolarAreaSeries`. Individual data points in this series cannot receive accessibility focus, and their values are not announced by the screen reader.

`PolarLineSeries` supports per-data-point accessibility navigation and announcements.

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
- **Data point values:** Use the `XBindingPath` and `YBindingPath` with meaningful underlying values (for example, a direction name and a numeric value) so that data point announcements read naturally.

```xml
<chart:SfPolarChart>
    <chart:SfPolarChart.Title>
        <Label Text="Wind Speed Analysis"/>
    </chart:SfPolarChart.Title>

    <chart:SfPolarChart.Legend>
        <chart:ChartLegend/>
    </chart:SfPolarChart.Legend>

    <chart:PolarLineSeries Label="Morning Wind"
                           ItemsSource="{Binding WindData}"
                           XBindingPath="Direction"
                           YBindingPath="Speed"
                           IsClosed="True"
                           EnableTooltip="True">
        <chart:PolarLineSeries.SelectionBehavior>
            <chart:DataPointSelectionBehavior SelectionBrush="#314A6E"/>
        </chart:PolarLineSeries.SelectionBehavior>
    </chart:PolarLineSeries>
</chart:SfPolarChart>
```

```csharp
SfPolarChart chart = new SfPolarChart
{
    Title = new Label() { Text = "Wind Speed Analysis" },
    Legend = new ChartLegend()
};

PolarLineSeries series = new PolarLineSeries
{
    Label = "Morning Wind",
    ItemsSource = new ViewModel().WindData,
    XBindingPath = "Direction",
    YBindingPath = "Speed",
    IsClosed = true,
    EnableTooltip = true,
    SelectionBehavior = new DataPointSelectionBehavior
    {
        SelectionBrush = Color.FromArgb("#314A6E")
    }
};

chart.Series.Add(series);
this.Content = chart;
```

With this configuration, a screen reader announces the chart title as "Wind Speed Analysis", and a focused data point (for example, North with a speed of 50 km/h) is announced as "North 50 km/h Morning Wind".

## Complete Example

The following example combines a descriptive chart title, legend, series labels, and tooltips to provide a fully accessible polar chart.

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:chart="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts"
             x:Class="AccessibilityDemo.MainPage">

    <chart:SfPolarChart>
        <chart:SfPolarChart.Title>
            <Label Text="Wind Speed Analysis"/>
        </chart:SfPolarChart.Title>

        <chart:SfPolarChart.Legend>
            <chart:ChartLegend/>
        </chart:SfPolarChart.Legend>

        <chart:PolarLineSeries Label="Morning Wind"
                               ItemsSource="{Binding WindData}"
                               XBindingPath="Direction"
                               YBindingPath="Speed"
                               IsClosed="True"
                               EnableTooltip="True"/>

        <chart:PolarLineSeries Label="Evening Wind"
                               ItemsSource="{Binding EveningWindData}"
                               XBindingPath="Direction"
                               YBindingPath="Speed"
                               IsClosed="True"
                               EnableTooltip="True"/>
    </chart:SfPolarChart>
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
    public List<WindModel> WindData { get; set; }
    public List<WindModel> EveningWindData { get; set; }

    public ViewModel()
    {
        WindData = new List<WindModel>
        {
            new WindModel("North", 50),
            new WindModel("East", 40),
            new WindModel("South", 30),
            new WindModel("West", 35)
        };

        EveningWindData = new List<WindModel>
        {
            new WindModel("North", 30),
            new WindModel("East", 45),
            new WindModel("South", 55),
            new WindModel("West", 25)
        };
    }
}

public class WindModel
{
    public string Direction { get; set; }
    public double Speed { get; set; }

    public WindModel(string direction, double speed)
    {
        Direction = direction;
        Speed = speed;
    }
}
```