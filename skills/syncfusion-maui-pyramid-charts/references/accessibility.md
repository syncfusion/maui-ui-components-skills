# Accessibility

## Table of Contents
- [Overview](#overview)
- [Pyramid Chart Elements and Announcements](#pyramid-chart-elements-and-announcements)
- [Enable Accessibility](#enable-accessibility)
- [Keyboard Navigation](#keyboard-navigation)
- [Gesture Navigation](#gesture-navigation)
- [Providing Meaningful Labels](#providing-meaningful-labels)
- [Complete Example](#complete-example)

## Overview

The SfPyramidChart includes built-in accessibility support to ensure that chart content is usable by everyone, including users who rely on screen readers (TalkBack on Android, VoiceOver on iOS and macOS, Narrator on Windows). Each chart element — the chart title, legend items, and pyramid segments — is exposed to the platform's accessibility tree and announced when it receives focus.

Accessibility is enabled by default through semantic properties. No additional configuration is required to expose chart elements to screen readers; simply ensure that your chart data includes meaningful text for titles, legend items, and labels so that announcements are informative.

## Pyramid Chart Elements and Announcements

Screen readers announce chart elements using the following formats when an element receives focus.

| Element | Announcement Format | Example |
| --- | --- | --- |
| Chart Title | Title text | "Sales Funnel Hierarchy" |
| Pyramid Layer (Segment) | Label + Value | "Enterprise 5000" |
| Legend Item | Legend label | "Customer Segments" |

## Enable Accessibility

Enable the platform screen reader on the target device to start receiving chart announcements.

| Platform | Steps |
| --- | --- |
| Android | Settings → Accessibility → TalkBack → Toggle "On" |
| Windows | Settings → Accessibility → Turn on Narrator (or press Windows key + Enter) |
| iOS | Settings → Accessibility → VoiceOver → Toggle "On" |
| macOS | System Preferences → Accessibility → VoiceOver → Check "Enable VoiceOver" |

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
- **Pyramid layers:** Use meaningful values in your data (via `XBindingPath`/`YBindingPath` or `Name`/`Value` bindings) so each pyramid layer announcement reads naturally as "Label + Value".
- **Legend items:** The legend label is announced; provide a meaningful label for the series so the announcement is descriptive.

```xml
<chart:SfPyramidChart ItemsSource="{Binding PyramidData}"
                      XBindingPath="Name"
                      YBindingPath="Value"
                      EnableTooltip="True">
    <chart:SfPyramidChart.Title>
        <Label Text="Sales Funnel Hierarchy"/>
    </chart:SfPyramidChart.Title>

    <chart:SfPyramidChart.Legend>
        <chart:ChartLegend/>
    </chart:SfPyramidChart.Legend>
</chart:SfPyramidChart>
```

```csharp
SfPyramidChart chart = new SfPyramidChart
{
    Title = new Label() { Text = "Sales Funnel Hierarchy" },
    Legend = new ChartLegend(),
    ItemsSource = new ViewModel().PyramidData,
    XBindingPath = "Name",
    YBindingPath = "Value",
    EnableTooltip = true
};

this.Content = chart;
```

With this configuration, a screen reader announces the chart title as "Sales Funnel Hierarchy", and a focused pyramid layer (for example, Enterprise with a value of 5000) is announced as "Enterprise 5000".

## Complete Example

The following example combines a descriptive chart title, legend, and tooltips to provide a fully accessible pyramid chart.

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:chart="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts"
             x:Class="AccessibilityDemo.MainPage">

    <chart:SfPyramidChart ItemsSource="{Binding PyramidData}"
                        XBindingPath="Name"
                        YBindingPath="Value"
                        EnableTooltip="True">
        <chart:SfPyramidChart.Title>
            <Label Text="Sales Funnel Hierarchy"/>
        </chart:SfPyramidChart.Title>

        <chart:SfPyramidChart.Legend>
            <chart:ChartLegend/>
        </chart:SfPyramidChart.Legend>
 
    </chart:SfPyramidChart>
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
    public List<PyramidModel> PyramidData { get; set; }

    public ViewModel()
    {
        PyramidData = new List<PyramidModel>
        {
            new PyramidModel("Enterprise", 5000),
            new PyramidModel("Mid-Market", 3500),
            new PyramidModel("SMB", 2000),
            new PyramidModel("Startup", 1000)
        };
    }
}

public class PyramidModel
{
    public string Name { get; set; }
    public double Value { get; set; }

    public PyramidModel(string name, double value)
    {
        Name = name;
        Value = value;
    }
}
```