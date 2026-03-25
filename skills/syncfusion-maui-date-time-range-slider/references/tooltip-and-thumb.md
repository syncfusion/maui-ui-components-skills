# Tooltip and Thumb in .NET MAUI DateTime Range Slider

This reference guide covers tooltip and thumb configuration in the Syncfusion .NET MAUI DateTime Range Slider (SfDateTimeRangeSlider) control.

## Table of Contents

- [Overview](#overview)
- [Tooltip](#tooltip)
  - [Enable Tooltip](#enable-tooltip)
  - [Show Always](#show-always)
  - [Tooltip Styling](#tooltip-styling)
  - [Tooltip Date Format](#tooltip-date-format)
  - [Tooltip Position](#tooltip-position)
- [Thumb](#thumb)
  - [Thumb Size](#thumb-size)
  - [Thumb Color](#thumb-color)
  - [Thumb Stroke](#thumb-stroke)
  - [Thumb Overlapping Stroke](#thumb-overlapping-stroke)
- [Thumb Overlay](#thumb-overlay)
  - [Overlay Size](#overlay-size)
  - [Overlay Color](#overlay-color)
- [Visual State Manager](#visual-state-manager)
  - [Disabled Thumb](#disabled-thumb)
- [Property Reference](#property-reference)

## Overview

The tooltip displays the current value when interacting with the thumb. The thumb is the draggable element that allows users to select values, and the thumb overlay provides visual feedback during interaction.

## Tooltip

### Enable Tooltip

Enable the tooltip by setting the `Tooltip` property to a `SliderTooltip` instance. The default value is `null`.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01" 
                               Maximum="2018-01-01" 
                               RangeStart="2012-01-01" 
                               RangeEnd="2016-01-01">
   
    <sliders:SfDateTimeRangeSlider.Tooltip>
        <sliders:SliderTooltip />
    </sliders:SfDateTimeRangeSlider.Tooltip>

</sliders:SfDateTimeRangeSlider>
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2018, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2016, 01, 01);
rangeSlider.Tooltip = new SliderTooltip();
```

### Show Always

Display the tooltip permanently by setting the `ShowAlways` property to `True`. The default value is `False`.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01" 
                               Maximum="2018-01-01" 
                               RangeStart="2012-01-01" 
                               RangeEnd="2016-01-01">
   
    <sliders:SfDateTimeRangeSlider.Tooltip>
        <sliders:SliderTooltip ShowAlways="True" />
    </sliders:SfDateTimeRangeSlider.Tooltip>

</sliders:SfDateTimeRangeSlider>
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2018, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2016, 01, 01);
rangeSlider.Tooltip = new SliderTooltip();
rangeSlider.Tooltip.ShowAlways = true;
```

### Tooltip Styling

Customize the tooltip appearance using various styling properties.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01"
                               Maximum="2018-01-01"
                               RangeStart="2012-01-01"
                               RangeEnd="2016-01-01">

    <sliders:SfDateTimeRangeSlider.Tooltip>
        <sliders:SliderTooltip Fill="#DFD8F7"
                               Stroke="#512BD4"
                               StrokeThickness="2"
                               TextColor="#512BD4"
                               FontSize="14"
                               FontAttributes="Bold"
                               Padding="12,12" />
    </sliders:SfDateTimeRangeSlider.Tooltip>

</sliders:SfDateTimeRangeSlider>
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2018, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2016, 01, 01);
rangeSlider.Tooltip = new SliderTooltip();
rangeSlider.Tooltip.Fill = new SolidColorBrush(Color.FromArgb("#DFD8F7"));
rangeSlider.Tooltip.Stroke = new SolidColorBrush(Color.FromArgb("#512BD4"));
rangeSlider.Tooltip.StrokeThickness = 2;
rangeSlider.Tooltip.TextColor = Color.FromArgb("#512BD4");
rangeSlider.Tooltip.FontSize = 14;
rangeSlider.Tooltip.FontAttributes = FontAttributes.Bold;
rangeSlider.Tooltip.Padding = new Thickness(12, 12);
```

### Tooltip Date Format

Format the tooltip date display using the `DateFormat` property.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01"
                               Maximum="2018-01-01"
                               RangeStart="2012-01-01"
                               RangeEnd="2016-01-01">

    <sliders:SfDateTimeRangeSlider.Tooltip>
        <sliders:SliderTooltip DateFormat="MMM dd, yyyy" />
    </sliders:SfDateTimeRangeSlider.Tooltip>

</sliders:SfDateTimeRangeSlider>
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2018, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2016, 01, 01);
rangeSlider.Tooltip = new SliderTooltip();
rangeSlider.Tooltip.DateFormat = "MMM dd, yyyy";
```

### Tooltip Position

Control the tooltip position using the `Position` property.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01"
                               Maximum="2018-01-01"
                               RangeStart="2012-01-01"
                               RangeEnd="2016-01-01">

    <sliders:SfDateTimeRangeSlider.Tooltip>
        <sliders:SliderTooltip Position="BottomRight" />
    </sliders:SfDateTimeRangeSlider.Tooltip>

</sliders:SfDateTimeRangeSlider>
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2018, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2016, 01, 01);
rangeSlider.Tooltip = new SliderTooltip();
rangeSlider.Tooltip.Position = SliderTooltipPosition.BottomRight;
```

### Custom Tooltip Text

Customize tooltip text using the `TooltipLabelCreated` event.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01"
                               Maximum="2018-01-01"
                               RangeStart="2012-01-01"
                               RangeEnd="2016-01-01"
                               Interval="2"
                               ShowTicks="True"
                               ShowLabels="True">

    <sliders:SfDateTimeRangeSlider.Tooltip>
        <sliders:SliderTooltip TooltipLabelCreated="OnTooltipLabelCreated" />
    </sliders:SfDateTimeRangeSlider.Tooltip>

</sliders:SfDateTimeRangeSlider>
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider()
{
    Minimum = new DateTime(2010, 01, 01),
    Maximum = new DateTime(2018, 01, 01),
    RangeStart = new DateTime(2012, 01, 01),
    RangeEnd = new DateTime(2016, 01, 01),
    Interval = 2,
    ShowTicks = true,
    ShowLabels = true,
    Tooltip = new SliderTooltip(),
};
rangeSlider.Tooltip.TooltipLabelCreated += OnTooltipLabelCreated;

private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    e.Text = "Year: " + e.Text;
}
```

## Thumb

### Thumb Size

Change the thumb radius using the `Radius` property of `ThumbStyle`. The default value is `10.0`.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01" 
                               Maximum="2018-01-01" 
                               RangeStart="2012-01-01" 
                               RangeEnd="2016-01-01">
   
    <sliders:SfDateTimeRangeSlider.ThumbStyle>
        <sliders:SliderThumbStyle Radius="15" />
    </sliders:SfDateTimeRangeSlider.ThumbStyle>

</sliders:SfDateTimeRangeSlider>
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2018, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2016, 01, 01);
rangeSlider.ThumbStyle.Radius = 15;
```

### Thumb Color

Customize the thumb fill color using the `Fill` property.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01" 
                               Maximum="2018-01-01" 
                               RangeStart="2012-01-01" 
                               RangeEnd="2016-01-01">
   
    <sliders:SfDateTimeRangeSlider.ThumbStyle>
        <sliders:SliderThumbStyle Fill="#EE3F3F" />
    </sliders:SfDateTimeRangeSlider.ThumbStyle>

</sliders:SfDateTimeRangeSlider>
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2018, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2016, 01, 01);
rangeSlider.ThumbStyle.Fill = new SolidColorBrush(Color.FromArgb("#EE3F3F"));
```

### Thumb Stroke

Customize the thumb stroke using the `Stroke` and `StrokeThickness` properties.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01" 
                               Maximum="2018-01-01" 
                               RangeStart="2012-01-01" 
                               RangeEnd="2016-01-01">
   
    <sliders:SfDateTimeRangeSlider.ThumbStyle>
        <sliders:SliderThumbStyle Stroke="#EE3F3F" 
                                  StrokeThickness="2" />
    </sliders:SfDateTimeRangeSlider.ThumbStyle>

</sliders:SfDateTimeRangeSlider>
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2018, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2016, 01, 01);
rangeSlider.ThumbStyle.Stroke = new SolidColorBrush(Color.FromArgb("#EE3F3F"));
rangeSlider.ThumbStyle.StrokeThickness = 2;
```

### Thumb Overlapping Stroke

Change the thumb stroke color when two thumbs overlap using the `OverlapStroke` property.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01" 
                               Maximum="2018-01-01" 
                               RangeStart="2012-01-01" 
                               RangeEnd="2016-01-01">
  
    <sliders:SfDateTimeRangeSlider.ThumbStyle>
        <sliders:SliderThumbStyle OverlapStroke="#EE3F3F" />
    </sliders:SfDateTimeRangeSlider.ThumbStyle>

</sliders:SfDateTimeRangeSlider>
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2018, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2016, 01, 01);
rangeSlider.ThumbStyle.OverlapStroke = new SolidColorBrush(Color.FromArgb("#EE3F3F"));
```

## Thumb Overlay

### Overlay Size

Change the thumb overlay radius using the `Radius` property of `ThumbOverlayStyle`. The default value is `24.0`.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01" 
                               Maximum="2018-01-01" 
                               RangeStart="2012-01-01" 
                               RangeEnd="2016-01-01">
   
    <sliders:SfDateTimeRangeSlider.ThumbOverlayStyle>
        <sliders:SliderThumbOverlayStyle Radius="18" />
    </sliders:SfDateTimeRangeSlider.ThumbOverlayStyle>

</sliders:SfDateTimeRangeSlider>
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2018, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2016, 01, 01);
rangeSlider.ThumbOverlayStyle.Radius = 18;
```

### Overlay Color

Customize the thumb overlay color using the `Fill` property.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01" 
                               Maximum="2018-01-01" 
                               RangeStart="2012-01-01" 
                               RangeEnd="2016-01-01">
   
    <sliders:SfDateTimeRangeSlider.ThumbOverlayStyle>
        <sliders:SliderThumbOverlayStyle Fill="#66FFD700" />
    </sliders:SfDateTimeRangeSlider.ThumbOverlayStyle>

</sliders:SfDateTimeRangeSlider>
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2018, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2016, 01, 01);
rangeSlider.ThumbOverlayStyle.Fill = new SolidColorBrush(Color.FromArgb("#66FFD700"));
```

## Visual State Manager

### Disabled Thumb

Customize thumb appearance in disabled state using Visual State Manager.

**XAML:**
```xaml
<ContentPage.Resources>
    <Style TargetType="sliders:SfDateTimeRangeSlider">
        <Setter Property="Minimum" Value="2010-01-01" />
        <Setter Property="Maximum" Value="2018-01-01" />
        <Setter Property="RangeStart" Value="2012-01-01" />
        <Setter Property="RangeEnd" Value="2016-01-01" />
        <Setter Property="Interval" Value="2" />
        <Setter Property="VisualStateManager.VisualStateGroups">
            <VisualStateGroupList>
                <VisualStateGroup>
                    <VisualState x:Name="Default">
                        <VisualState.Setters>
                            <Setter Property="ThumbStyle">
                                <Setter.Value>
                                    <sliders:SliderThumbStyle Radius="13"
                                                              Fill="Red"
                                                              Stroke="Yellow"
                                                              StrokeThickness="3" />
                                </Setter.Value>
                            </Setter>
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState x:Name="Disabled">
                        <VisualState.Setters>
                            <Setter Property="ThumbStyle">
                                <Setter.Value>
                                    <sliders:SliderThumbStyle Radius="13"
                                                              Fill="Gray"
                                                              Stroke="LightGray"
                                                              StrokeThickness="3" />
                                </Setter.Value>
                            </Setter>
                            <Setter Property="TrackStyle">
                                <Setter.Value>
                                    <sliders:SliderTrackStyle ActiveFill="Gray"
                                                              InactiveFill="LightGray" />
                                </Setter.Value>
                            </Setter>
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateGroupList>
        </Setter>
    </Style>
</ContentPage.Resources>

<ContentPage.Content>
    <VerticalStackLayout>
        <Label Text="Enabled" Padding="24,10" />
        <sliders:SfDateTimeRangeSlider />
        <Label Text="Disabled" Padding="24,10" />
        <sliders:SfDateTimeRangeSlider IsEnabled="False" />
    </VerticalStackLayout>
</ContentPage.Content>
```

**C#:**
```csharp
VerticalStackLayout stackLayout = new VerticalStackLayout();
SfDateTimeRangeSlider defaultRangeSlider = new SfDateTimeRangeSlider()
{
    Minimum = new DateTime(2010, 01, 01),
    Maximum = new DateTime(2018, 01, 01),
    RangeStart = new DateTime(2012, 01, 01),
    RangeEnd = new DateTime(2016, 01, 01),
    Interval = 2
};
SfDateTimeRangeSlider disabledRangeSlider = new SfDateTimeRangeSlider()
{
    Minimum = new DateTime(2010, 01, 01),
    Maximum = new DateTime(2018, 01, 01),
    RangeStart = new DateTime(2012, 01, 01),
    RangeEnd = new DateTime(2016, 01, 01),
    IsEnabled = false,
    Interval = 2
};

VisualStateGroupList visualStateGroupList = new VisualStateGroupList();
VisualStateGroup commonStateGroup = new VisualStateGroup();

// Default State
VisualState defaultState = new VisualState { Name = "Default" };
defaultState.Setters.Add(new Setter
{
    Property = SfDateTimeRangeSlider.ThumbStyleProperty,
    Value = new SliderThumbStyle
    {
        Radius = 13,
        Fill = Colors.Red,
        Stroke = Colors.Yellow,
        StrokeThickness = 3,
    }
});

// Disabled State
VisualState disabledState = new VisualState { Name = "Disabled" };
disabledState.Setters.Add(new Setter
{
    Property = SfDateTimeRangeSlider.ThumbStyleProperty,
    Value = new SliderThumbStyle
    {
        Radius = 13,
        Fill = Colors.Gray,
        Stroke = Colors.LightGray,
        StrokeThickness = 3,
    }
});
disabledState.Setters.Add(new Setter
{
    Property = SfDateTimeRangeSlider.TrackStyleProperty,
    Value = new SliderTrackStyle
    {
        ActiveFill = Colors.Gray,
        InactiveFill = Colors.LightGray,
    }
});

commonStateGroup.States.Add(defaultState);
commonStateGroup.States.Add(disabledState);
visualStateGroupList.Add(commonStateGroup);
VisualStateManager.SetVisualStateGroups(defaultRangeSlider, visualStateGroupList);
VisualStateManager.SetVisualStateGroups(disabledRangeSlider, visualStateGroupList);

stackLayout.Children.Add(new Label() { Text = "Enabled", Padding = new Thickness(24, 10) });
stackLayout.Children.Add(defaultRangeSlider);
stackLayout.Children.Add(new Label() { Text = "Disabled", Padding = new Thickness(24, 10) });
stackLayout.Children.Add(disabledRangeSlider);
this.Content = stackLayout;
```

## Property Reference

### Tooltip Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Tooltip` | SliderTooltip | null | The tooltip configuration object |
| `ShowAlways` | bool | False | Display tooltip permanently |
| `Fill` | Brush | - | Background color of the tooltip |
| `Stroke` | Brush | - | Border color of the tooltip |
| `StrokeThickness` | double | - | Border width of the tooltip |
| `TextColor` | Color | - | Color of the tooltip text |
| `FontSize` | double | - | Font size of the tooltip text |
| `FontAttributes` | FontAttributes | - | Font attributes (Bold, Italic) |
| `FontFamily` | string | - | Font family for the tooltip text |
| `Padding` | Thickness | - | Internal padding of the tooltip |
| `DateFormat` | string | - | Date format string for tooltip |
| `Position` | SliderTooltipPosition | - | Position of the tooltip |

### ThumbStyle Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Radius` | double | 10.0 | Radius of the thumb circle |
| `Fill` | Brush | - | Fill color of the thumb |
| `Stroke` | Brush | - | Stroke color of the thumb |
| `StrokeThickness` | double | - | Stroke thickness of the thumb |
| `OverlapStroke` | Brush | - | Stroke color when thumbs overlap |

### ThumbOverlayStyle Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Radius` | double | 24.0 | Radius of the thumb overlay |
| `Fill` | Brush | - | Fill color of the thumb overlay |

### Tooltip Events

| Event | EventArgs | Description |
|-------|-----------|-------------|
| `TooltipLabelCreated` | SliderTooltipLabelCreatedEventArgs | Fired when tooltip label is created, allows customization |
