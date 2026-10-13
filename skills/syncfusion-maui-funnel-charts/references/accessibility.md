# Accessibility

## Table of Contents
- [Overview](#overview)
- [Chart Elements and Announcements](#chart-elements-and-announcements)
- [Enable Accessibility](#enable-accessibility)
- [Keyboard Navigation](#keyboard-navigation)
- [Gesture Navigation](#gesture-navigation)
- [Providing Meaningful Labels](#providing-meaningful-labels)
- [Complete Example](#complete-example)

## Overview

The SfFunnelChart includes built-in accessibility support to ensure that chart content is usable by everyone, including users who rely on screen readers (TalkBack on Android, VoiceOver on iOS and macOS, Narrator on Windows). Each chart element — the chart title, legend items, and funnel sections (layers) — is exposed to the platform's accessibility tree and announced when it receives focus.

Accessibility is enabled by default through semantic properties. No additional configuration is required to expose chart elements to screen readers; simply ensure that your chart data includes meaningful text for titles, legend items, and labels so that announcements are informative.

## Chart Elements and Announcements

Screen readers announce chart elements using the following formats when an element receives focus.

| Element | Announcement Format | Example |
| --- | --- | --- |
| Chart Title | Title text | "Sales Conversion Funnel" |
| Funnel Layer (Section) | Label + Value | "Conversion 1500 users" |
| Legend Item | Legend label | "Sales Pipeline" |

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
- **Funnel sections:** Use meaningful values in your data (via `XBindingPath`/`YBindingPath` or `Name`/`Value` bindings) so each funnel layer announcement reads naturally as "Label + Value".
- **Legend items:** The legend label is announced; provide a meaningful label for the series so the announcement is descriptive.

```xml
<chart:SfFunnelChart ItemsSource="{Binding FunnelData}"
                     XBindingPath="Name"
                     YBindingPath="Value"
                     EnableTooltip="True">
    <chart:SfFunnelChart.Title>
        <Label Text="Sales Conversion Funnel"/>
    </chart:SfFunnelChart.Title>

    <chart:SfFunnelChart.Legend>
        <chart:ChartLegend/>
    </chart:SfFunnelChart.Legend>
    
</chart:SfFunnelChart>
```

```csharp
SfFunnelChart chart = new SfFunnelChart
{
    Title = new Label() { Text = "Sales Conversion Funnel" },
    Legend = new ChartLegend(),
    ItemsSource = new ViewModel().FunnelData,
    XBindingPath = "Name",
    YBindingPath = "Value",
    EnableTooltip = true,
    SelectionBehavior = new DataPointSelectionBehavior
    {
        SelectionBrush = Color.FromArgb("#314A6E")
    }
};

this.Content = chart;
```

With this configuration, a screen reader announces the chart title as "Sales Conversion Funnel", and a focused funnel section (for example, Conversion with a value of 1500 users) is announced as "Conversion 1500 users".

## Complete Example

The following example combines a descriptive chart title, legend, and tooltips to provide a fully accessible funnel chart.

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:chart="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts"
             x:Class="AccessibilityDemo.MainPage">

    <chart:SfFunnelChart ItemsSource="{Binding FunnelData}"
                    XBindingPath="Name"
                    YBindingPath="Value"
                    EnableTooltip="True">
        <chart:SfFunnelChart.Title>
            <Label Text="Sales Conversion Funnel"/>
        </chart:SfFunnelChart.Title>

        <chart:SfFunnelChart.Legend>
            <chart:ChartLegend/>
        </chart:SfFunnelChart.Legend>    
    </chart:SfFunnelChart>

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
    public List<FunnelModel> FunnelData { get; set; }

    public ViewModel()
    {
        FunnelData = new List<FunnelModel>
        {
            new FunnelModel("Visitors", 5000),
            new FunnelModel("Leads", 3000),
            new FunnelModel("Opportunities", 2000),
            new FunnelModel("Conversion", 1500)
        };
    }
}

public class FunnelModel
{
    public string Name { get; set; }
    public double Value { get; set; }

    public FunnelModel(string name, double value)
    {
        Name = name;
        Value = value;
    }
}
```