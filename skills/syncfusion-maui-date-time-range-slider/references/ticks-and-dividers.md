# Ticks and Dividers in .NET MAUI DateTime Range Slider

This reference guide covers ticks and dividers configuration in the Syncfusion .NET MAUI DateTime Range Slider (SfDateTimeRangeSlider) control.

## Table of Contents

- [Overview](#overview)
- [Major Ticks](#major-ticks)
  - [Show Major Ticks Without Interval](#show-major-ticks-without-interval)
  - [Show Major Ticks With Interval](#show-major-ticks-with-interval)
  - [Major Ticks Color](#major-ticks-color)
  - [Major Ticks Size](#major-ticks-size)
- [Minor Ticks](#minor-ticks)
  - [Show Minor Ticks Without Interval](#show-minor-ticks-without-interval)
  - [Show Minor Ticks With Interval](#show-minor-ticks-with-interval)
  - [Minor Ticks Color](#minor-ticks-color)
  - [Minor Ticks Size](#minor-ticks-size)
- [Ticks Offset](#ticks-offset)
- [Dividers](#dividers)
  - [Show Dividers](#show-dividers)
  - [Divider Radius](#divider-radius)
  - [Divider Color](#divider-color)
  - [Divider Stroke](#divider-stroke)
- [Visual State Manager](#visual-state-manager)
  - [Disabled Ticks](#disabled-ticks)
  - [Disabled Dividers](#disabled-dividers)
- [Property Reference](#property-reference)

## Overview

Ticks and dividers are visual indicators that help users understand the value distribution on the slider track. Major ticks represent the primary interval points, minor ticks provide finer granularity between major ticks, and dividers are circular shapes that mark interval boundaries.

## Major Ticks

### Show Major Ticks Without Interval

Enable major ticks on the track using the `ShowTicks` property. The default value is `False`.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01" 
                               Maximum="2018-01-01" 
                               RangeStart="2012-01-01" 
                               RangeEnd="2016-01-01"
                               ShowTicks="True" />
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2018, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2016, 01, 01);
rangeSlider.ShowTicks = true;
```

### Show Major Ticks With Interval

Set the `Interval` property to control the spacing between major ticks.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01" 
                               Maximum="2020-01-01" 
                               RangeStart="2012-01-01" 
                               RangeEnd="2018-01-01"
                               Interval="2"
                               ShowTicks="True" />
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2020, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2018, 01, 01);
rangeSlider.Interval = 2;
rangeSlider.ShowTicks = true;
```

### Major Ticks Color

Customize active and inactive major ticks colors using the `ActiveFill` and `InactiveFill` properties of `MajorTickStyle`.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01" 
                               Maximum="2020-01-01" 
                               RangeStart="2012-01-01" 
                               RangeEnd="2018-01-01"
                               Interval="2"
                               ShowTicks="True">
    
    <sliders:SfDateTimeRangeSlider.MajorTickStyle>
        <sliders:SliderTickStyle ActiveFill="#EE3F3F" 
                                 InactiveFill="#F7B1AE" />
    </sliders:SfDateTimeRangeSlider.MajorTickStyle>

</sliders:SfDateTimeRangeSlider>
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2020, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2018, 01, 01);
rangeSlider.Interval = 2;
rangeSlider.ShowTicks = true;
rangeSlider.MajorTickStyle.ActiveFill = new SolidColorBrush(Color.FromArgb("#EE3F3F"));
rangeSlider.MajorTickStyle.InactiveFill = new SolidColorBrush(Color.FromArgb("#F7B1AE"));
```

### Major Ticks Size

Change the size of major ticks using the `ActiveSize` and `InactiveSize` properties. The default value is `Size(2.0, 8.0)`.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01" 
                               Maximum="2018-01-01" 
                               RangeStart="2012-01-01" 
                               RangeEnd="2016-01-01"
                               Interval="2"
                               ShowTicks="True">
    
    <sliders:SfDateTimeRangeSlider.MajorTickStyle>
        <sliders:SliderTickStyle ActiveSize="2,15" 
                                 InactiveSize="2,15" />
    </sliders:SfDateTimeRangeSlider.MajorTickStyle>

</sliders:SfDateTimeRangeSlider>
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2018, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2016, 01, 01);
rangeSlider.Interval = 2;
rangeSlider.ShowTicks = true;
rangeSlider.MajorTickStyle.ActiveSize = new Size(2, 15);
rangeSlider.MajorTickStyle.InactiveSize = new Size(2, 15);
```

## Minor Ticks

### Show Minor Ticks Without Interval

Display minor ticks between major ticks using the `MinorTicksPerInterval` property. The default value is `0`.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01" 
                               Maximum="2018-01-01" 
                               RangeStart="2012-01-01" 
                               RangeEnd="2016-01-01"
                               MinorTicksPerInterval="3"
                               ShowTicks="True" />
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2018, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2016, 01, 01);
rangeSlider.MinorTicksPerInterval = 3;
rangeSlider.ShowTicks = true;
```

### Show Minor Ticks With Interval

Combine `Interval` and `MinorTicksPerInterval` properties for precise tick placement.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01" 
                               Maximum="2020-01-01" 
                               RangeStart="2012-01-01" 
                               RangeEnd="2018-01-01"
                               Interval="2" 
                               MinorTicksPerInterval="1" 
                               ShowTicks="True" />
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2020, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2018, 01, 01);
rangeSlider.Interval = 2;
rangeSlider.MinorTicksPerInterval = 1;
rangeSlider.ShowTicks = true;
```

### Minor Ticks Color

Customize active and inactive minor ticks colors using the `MinorTickStyle`.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01" 
                               Maximum="2020-01-01" 
                               RangeStart="2012-01-01" 
                               RangeEnd="2018-01-01"
                               Interval="2" 
                               ShowTicks="True" 
                               MinorTicksPerInterval="1">
    
    <sliders:SfDateTimeRangeSlider.MinorTickStyle>
        <sliders:SliderTickStyle ActiveFill="#EE3F3F" 
                                 InactiveFill="#F7B1AE" />
    </sliders:SfDateTimeRangeSlider.MinorTickStyle>

</sliders:SfDateTimeRangeSlider>
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2020, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2018, 01, 01);
rangeSlider.Interval = 2;
rangeSlider.ShowTicks = true;
rangeSlider.MinorTicksPerInterval = 1;
rangeSlider.MinorTickStyle.ActiveFill = new SolidColorBrush(Color.FromArgb("#EE3F3F"));
rangeSlider.MinorTickStyle.InactiveFill = new SolidColorBrush(Color.FromArgb("#F7B1AE"));
```

### Minor Ticks Size

Adjust minor ticks dimensions using the `ActiveSize` and `InactiveSize` properties.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01" 
                               Maximum="2018-01-01" 
                               RangeStart="2012-01-01" 
                               RangeEnd="2016-01-01"
                               Interval="2"
                               ShowTicks="True"
                               MinorTicksPerInterval="1">
    
    <sliders:SfDateTimeRangeSlider.MinorTickStyle>
        <sliders:SliderTickStyle ActiveSize="2,10" 
                                 InactiveSize="2,10" />
    </sliders:SfDateTimeRangeSlider.MinorTickStyle>

</sliders:SfDateTimeRangeSlider>
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2018, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2016, 01, 01);
rangeSlider.Interval = 2;
rangeSlider.ShowTicks = true;
rangeSlider.MinorTicksPerInterval = 1;
rangeSlider.MinorTickStyle.ActiveSize = new Size(2, 10);
rangeSlider.MinorTickStyle.InactiveSize = new Size(2, 10);
```

## Ticks Offset

Adjust the space between track and ticks using the `Offset` property. The default value is `3.0`.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01" 
                               Maximum="2018-01-01" 
                               RangeStart="2012-01-01" 
                               RangeEnd="2016-01-01"
                               Interval="2"
                               ShowTicks="True" 
                               MinorTicksPerInterval="1">
   
    <sliders:SfDateTimeRangeSlider.MinorTickStyle>
        <sliders:SliderTickStyle Offset="5" />
    </sliders:SfDateTimeRangeSlider.MinorTickStyle>
   
    <sliders:SfDateTimeRangeSlider.MajorTickStyle>
        <sliders:SliderTickStyle Offset="5" />
    </sliders:SfDateTimeRangeSlider.MajorTickStyle>

</sliders:SfDateTimeRangeSlider>
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2018, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2016, 01, 01);
rangeSlider.Interval = 2;
rangeSlider.ShowTicks = true;
rangeSlider.MinorTicksPerInterval = 1;
rangeSlider.MinorTickStyle.Offset = 5;
rangeSlider.MajorTickStyle.Offset = 5;
```

## Dividers

### Show Dividers

Enable dividers on the track using the `ShowDividers` property. The default value is `False`.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01" 
                               Maximum="2020-01-01" 
                               RangeStart="2012-01-01" 
                               RangeEnd="2018-01-01"
                               Interval="2" 
                               ShowDividers="True" />
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2020, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2018, 01, 01);
rangeSlider.Interval = 2;
rangeSlider.ShowDividers = true;
```

### Divider Radius

Change the radius of active and inactive dividers using the `ActiveRadius` and `InactiveRadius` properties.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01" 
                               Maximum="2020-01-01" 
                               RangeStart="2012-01-01" 
                               RangeEnd="2016-01-01"
                               Interval="2"
                               ShowDividers="True">
   
    <sliders:SfDateTimeRangeSlider.DividerStyle>
        <sliders:SliderDividerStyle ActiveRadius="7" 
                                    InactiveRadius="7" />
    </sliders:SfDateTimeRangeSlider.DividerStyle>

</sliders:SfDateTimeRangeSlider>
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2020, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2016, 01, 01);
rangeSlider.Interval = 2;
rangeSlider.ShowDividers = true;
rangeSlider.DividerStyle.ActiveRadius = 7;
rangeSlider.DividerStyle.InactiveRadius = 7;
```

### Divider Color

Customize divider fill colors using the `ActiveFill` and `InactiveFill` properties.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01" 
                               Maximum="2020-01-01" 
                               RangeStart="2012-01-01" 
                               RangeEnd="2018-01-01"
                               Interval="2"
                               ShowDividers="True">
    
    <sliders:SfDateTimeRangeSlider.DividerStyle>
        <sliders:SliderDividerStyle ActiveRadius="7" 
                                    InactiveRadius="7" 
                                    ActiveFill="#EE3F3F" 
                                    InactiveFill="#F7B1AE" />
    </sliders:SfDateTimeRangeSlider.DividerStyle>

</sliders:SfDateTimeRangeSlider>
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2020, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2018, 01, 01);
rangeSlider.Interval = 2;
rangeSlider.ShowDividers = true;
rangeSlider.DividerStyle.ActiveRadius = 7;
rangeSlider.DividerStyle.InactiveRadius = 7;
rangeSlider.DividerStyle.ActiveFill = new SolidColorBrush(Color.FromArgb("#EE3F3F"));
rangeSlider.DividerStyle.InactiveFill = new SolidColorBrush(Color.FromArgb("#F7B1AE"));
```

### Divider Stroke

Customize divider stroke width and color using the stroke properties.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01" 
                               Maximum="2020-01-01" 
                               RangeStart="2012-01-01" 
                               RangeEnd="2018-01-01"
                               Interval="2"
                               ShowDividers="True">
   
    <sliders:SfDateTimeRangeSlider.DividerStyle>
        <sliders:SliderDividerStyle ActiveRadius="7" 
                                    InactiveRadius="7" 
                                    ActiveStrokeThickness="2" 
                                    InactiveStrokeThickness="2" 
                                    ActiveStroke="#EE3F3F" 
                                    InactiveStroke="#F7B1AE" />
    </sliders:SfDateTimeRangeSlider.DividerStyle>

</sliders:SfDateTimeRangeSlider>
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2020, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2018, 01, 01);
rangeSlider.Interval = 2;
rangeSlider.ShowDividers = true;
rangeSlider.DividerStyle.ActiveRadius = 7;
rangeSlider.DividerStyle.InactiveRadius = 7;
rangeSlider.DividerStyle.ActiveStroke = new SolidColorBrush(Color.FromArgb("#EE3F3F"));
rangeSlider.DividerStyle.InactiveStroke = new SolidColorBrush(Color.FromArgb("#F7B1AE"));
rangeSlider.DividerStyle.ActiveStrokeThickness = 2;
rangeSlider.DividerStyle.InactiveStrokeThickness = 2;
```

## Visual State Manager

### Disabled Ticks

Customize ticks appearance in disabled state using Visual State Manager.

**XAML:**
```xaml
<ContentPage.Resources>
    <Style TargetType="sliders:SfDateTimeRangeSlider">
        <Setter Property="Minimum" Value="2010-01-01" />
        <Setter Property="Maximum" Value="2018-01-01" />
        <Setter Property="RangeStart" Value="2012-01-01" />
        <Setter Property="RangeEnd" Value="2016-01-01" />
        <Setter Property="Interval" Value="2" />
        <Setter Property="ShowTicks" Value="True" />
        <Setter Property="MinorTicksPerInterval" Value="2" />
        <Setter Property="VisualStateManager.VisualStateGroups">
            <VisualStateGroupList>
                <VisualStateGroup>
                    <VisualState x:Name="Default">
                        <VisualState.Setters>
                            <Setter Property="MajorTickStyle">
                                <Setter.Value>
                                    <sliders:SliderTickStyle ActiveSize="3,10"
                                                             InactiveSize="3,10"
                                                             ActiveFill="#EE3F3F"
                                                             InactiveFill="#F7B1AE" />
                                </Setter.Value>
                            </Setter>
                            <Setter Property="MinorTickStyle">
                                <Setter.Value>
                                    <sliders:SliderTickStyle ActiveSize="3,6"
                                                             InactiveSize="3,6"
                                                             ActiveFill="#EE3F3F"
                                                             InactiveFill="#F7B1AE" />
                                </Setter.Value>
                            </Setter>
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState x:Name="Disabled">
                        <VisualState.Setters>
                            <Setter Property="MajorTickStyle">
                                <Setter.Value>
                                    <sliders:SliderTickStyle ActiveSize="3,10"
                                                             InactiveSize="3,10"
                                                             ActiveFill="Gray"
                                                             InactiveFill="LightGray" />
                                </Setter.Value>
                            </Setter>
                            <Setter Property="MinorTickStyle">
                                <Setter.Value>
                                    <sliders:SliderTickStyle ActiveSize="3,6"
                                                             InactiveSize="3,6"
                                                             ActiveFill="Gray"
                                                             InactiveFill="LightGray" />
                                </Setter.Value>
                            </Setter>
                            <Setter Property="ThumbStyle">
                                <Setter.Value>
                                    <sliders:SliderThumbStyle Fill="Gray" />
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
    Interval = 2,
    ShowTicks = true,
    MinorTicksPerInterval = 2
};
SfDateTimeRangeSlider disabledRangeSlider = new SfDateTimeRangeSlider()
{
    Minimum = new DateTime(2010, 01, 01),
    Maximum = new DateTime(2018, 01, 01),
    RangeStart = new DateTime(2012, 01, 01),
    RangeEnd = new DateTime(2016, 01, 01),
    IsEnabled = false,
    Interval = 2,
    ShowTicks = true,
    MinorTicksPerInterval = 2
};

VisualStateGroupList visualStateGroupList = new VisualStateGroupList();
VisualStateGroup commonStateGroup = new VisualStateGroup();

// Default State
VisualState defaultState = new VisualState { Name = "Default" };
defaultState.Setters.Add(new Setter
{
    Property = SfDateTimeRangeSlider.MajorTickStyleProperty,
    Value = new SliderTickStyle
    {
        ActiveSize = new Size(3, 10),
        InactiveSize = new Size(3, 10),
        ActiveFill = Color.FromArgb("#EE3F3F"),
        InactiveFill = Color.FromArgb("#F7B1AE"),
    }
});
defaultState.Setters.Add(new Setter
{
    Property = SfDateTimeRangeSlider.MinorTickStyleProperty,
    Value = new SliderTickStyle
    {
        ActiveSize = new Size(3, 6),
        InactiveSize = new Size(3, 6),
        ActiveFill = Color.FromArgb("#EE3F3F"),
        InactiveFill = Color.FromArgb("#F7B1AE"),
    }
});

// Disabled State
VisualState disabledState = new VisualState { Name = "Disabled" };
disabledState.Setters.Add(new Setter
{
    Property = SfDateTimeRangeSlider.MajorTickStyleProperty,
    Value = new SliderTickStyle
    {
        ActiveSize = new Size(3, 10),
        InactiveSize = new Size(3, 10),
        ActiveFill = Colors.Gray,
        InactiveFill = Colors.LightGray,
    }
});
disabledState.Setters.Add(new Setter
{
    Property = SfDateTimeRangeSlider.MinorTickStyleProperty,
    Value = new SliderTickStyle
    {
        ActiveSize = new Size(3, 6),
        InactiveSize = new Size(3, 6),
        ActiveFill = Colors.Gray,
        InactiveFill = Colors.LightGray,
    }
});
disabledState.Setters.Add(new Setter
{
    Property = SfDateTimeRangeSlider.ThumbStyleProperty,
    Value = new SliderThumbStyle { Fill = Colors.Gray }
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

### Disabled Dividers

Customize dividers appearance in disabled state using Visual State Manager.

**XAML:**
```xaml
<ContentPage.Resources>
    <Style TargetType="sliders:SfDateTimeRangeSlider">
        <Setter Property="Minimum" Value="2010-01-01" />
        <Setter Property="Maximum" Value="2018-01-01" />
        <Setter Property="RangeStart" Value="2012-01-01" />
        <Setter Property="RangeEnd" Value="2016-01-01" />
        <Setter Property="Interval" Value="2" />
        <Setter Property="ShowDividers" Value="True" />
        <Setter Property="VisualStateManager.VisualStateGroups">
            <VisualStateGroupList>
                <VisualStateGroup>
                    <VisualState x:Name="Default">
                        <VisualState.Setters>
                            <Setter Property="DividerStyle">
                                <Setter.Value>
                                    <sliders:SliderDividerStyle ActiveFill="#EE3F3F"
                                                                InactiveFill="#88EE3F3F"
                                                                ActiveRadius="5"
                                                                InactiveRadius="4" />
                                </Setter.Value>
                            </Setter>
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState x:Name="Disabled">
                        <VisualState.Setters>
                            <Setter Property="DividerStyle">
                                <Setter.Value>
                                    <sliders:SliderDividerStyle ActiveFill="Gray"
                                                                InactiveFill="LightGray"
                                                                ActiveRadius="5"
                                                                InactiveRadius="4" />
                                </Setter.Value>
                            </Setter>
                            <Setter Property="TrackStyle">
                                <Setter.Value>
                                    <sliders:SliderTrackStyle ActiveFill="Gray"
                                                              InactiveFill="LightGray" />
                                </Setter.Value>
                            </Setter>
                            <Setter Property="ThumbStyle">
                                <Setter.Value>
                                    <sliders:SliderThumbStyle Fill="Gray" />
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
    Interval = 2,
    ShowDividers = true
};
SfDateTimeRangeSlider disabledRangeSlider = new SfDateTimeRangeSlider()
{
    Minimum = new DateTime(2010, 01, 01),
    Maximum = new DateTime(2018, 01, 01),
    RangeStart = new DateTime(2012, 01, 01),
    RangeEnd = new DateTime(2016, 01, 01),
    IsEnabled = false,
    Interval = 2,
    ShowDividers = true
};

VisualStateGroupList visualStateGroupList = new VisualStateGroupList();
VisualStateGroup commonStateGroup = new VisualStateGroup();

// Default State
VisualState defaultState = new VisualState { Name = "Default" };
defaultState.Setters.Add(new Setter
{
    Property = SfDateTimeRangeSlider.DividerStyleProperty,
    Value = new SliderDividerStyle
    {
        ActiveFill = Color.FromArgb("#EE3F3F"),
        InactiveFill = Color.FromArgb("#88EE3F3F"),
        ActiveRadius = 5,
        InactiveRadius = 4,
    }
});

// Disabled State
VisualState disabledState = new VisualState { Name = "Disabled" };
disabledState.Setters.Add(new Setter
{
    Property = SfDateTimeRangeSlider.DividerStyleProperty,
    Value = new SliderDividerStyle
    {
        ActiveFill = Colors.Gray,
        InactiveFill = Colors.LightGray,
        ActiveRadius = 5,
        InactiveRadius = 4,
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
disabledState.Setters.Add(new Setter
{
    Property = SfDateTimeRangeSlider.ThumbStyleProperty,
    Value = new SliderThumbStyle { Fill = Colors.Gray }
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

### Ticks Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `ShowTicks` | bool | False | Enables or disables tick marks on the track |
| `MinorTicksPerInterval` | int | 0 | Number of minor ticks between major ticks |
| `Interval` | double | 0 | Spacing between major ticks |

### MajorTickStyle Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `ActiveFill` | Brush | - | Fill color for active major ticks |
| `InactiveFill` | Brush | - | Fill color for inactive major ticks |
| `ActiveSize` | Size | Size(2.0, 8.0) | Size of active major ticks |
| `InactiveSize` | Size | Size(2.0, 8.0) | Size of inactive major ticks |
| `Offset` | double | 3.0 | Space between track and ticks |

### MinorTickStyle Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `ActiveFill` | Brush | - | Fill color for active minor ticks |
| `InactiveFill` | Brush | - | Fill color for inactive minor ticks |
| `ActiveSize` | Size | Size(2.0, 8.0) | Size of active minor ticks |
| `InactiveSize` | Size | Size(2.0, 8.0) | Size of inactive minor ticks |
| `Offset` | double | 3.0 | Space between track and ticks |

### Divider Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `ShowDividers` | bool | False | Enables or disables dividers on the track |

### DividerStyle Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `ActiveFill` | Brush | - | Fill color for active dividers |
| `InactiveFill` | Brush | - | Fill color for inactive dividers |
| `ActiveRadius` | double | - | Radius of active dividers |
| `InactiveRadius` | double | - | Radius of inactive dividers |
| `ActiveStroke` | Brush | - | Stroke color for active dividers |
| `InactiveStroke` | Brush | - | Stroke color for inactive dividers |
| `ActiveStrokeThickness` | double | - | Stroke thickness for active dividers |
| `InactiveStrokeThickness` | double | - | Stroke thickness for inactive dividers |
