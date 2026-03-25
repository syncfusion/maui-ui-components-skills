# Selection and Interaction in .NET MAUI DateTime Range Slider

This reference guide covers range selection and interaction behaviors in the Syncfusion .NET MAUI DateTime Range Slider (SfDateTimeRangeSlider) control.

## Table of Contents

- [Overview](#overview)
- [Discrete Selection](#discrete-selection)
  - [Step Duration](#step-duration)
  - [Step Duration with Interval](#step-duration-with-interval)
- [Interval Selection](#interval-selection)
- [Drag Behavior](#drag-behavior)
  - [OnThumb](#onthumb)
  - [BetweenThumbs](#betweenthumbs)
  - [Both](#both)
- [Deferred Update](#deferred-update)
- [Track Configuration](#track-configuration)
  - [Track Height](#track-height)
  - [Track Colors](#track-colors)
- [Property Reference](#property-reference)

## Overview

The DateTime Range Slider supports various selection and interaction modes that control how users can interact with the thumbs to select date ranges. These include discrete stepping, interval selection, and different drag behaviors.

## Discrete Selection

### Step Duration

Move thumbs in discrete date intervals using the `StepDuration` property. This ensures that thumb values snap to specific date points.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2000-01-01"
                               Maximum="2004-01-01"
                               RangeStart="2001-01-01"
                               RangeEnd="2003-01-01"
                               Interval="1"
                               StepDuration="1"
                               ShowLabels="True"
                               ShowTicks="True"
                               ShowDividers="True" />
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2000, 01, 01);
rangeSlider.Maximum = new DateTime(2004, 01, 01);
rangeSlider.RangeStart = new DateTime(2001, 01, 01); 
rangeSlider.RangeEnd = new DateTime(2003, 01, 01);     
rangeSlider.ShowLabels = true;
rangeSlider.ShowTicks = true;      
rangeSlider.ShowDividers = true;    
rangeSlider.Interval = 1;  
rangeSlider.StepDuration = new SliderStepDuration(years: 1);
```

### Step Duration with Interval

Combine `StepDuration` with `Interval` for precise control over discrete steps.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01"
                               Maximum="2020-01-01"
                               RangeStart="2012-01-01"
                               RangeEnd="2018-01-01"
                               Interval="2"
                               StepDuration="1"
                               ShowLabels="True"
                               ShowTicks="True"
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
rangeSlider.StepDuration = new SliderStepDuration(years: 1);
rangeSlider.ShowLabels = true;
rangeSlider.ShowTicks = true;
rangeSlider.ShowDividers = true;
```

### Advanced Step Duration Configuration

Configure step duration with multiple date components.

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2015, 01, 01);
rangeSlider.RangeStart = new DateTime(2011, 06, 01);
rangeSlider.RangeEnd = new DateTime(2014, 06, 01);
// Step by 6 months
rangeSlider.StepDuration = new SliderStepDuration(months: 6);
```

## Interval Selection

When `EnableIntervalSelection` is `True`, both thumbs move to the selected interval instead of just the nearest thumb.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01" 
                               Maximum="2018-01-01" 
                               RangeStart="2012-01-01" 
                               RangeEnd="2016-01-01" 
                               Interval="2"
                               ShowTicks="True"
                               ShowLabels="True"
                               EnableIntervalSelection="True" />
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2018, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2016, 01, 01);
rangeSlider.Interval = 2;        
rangeSlider.ShowLabels = true;
rangeSlider.ShowTicks = true;    
rangeSlider.EnableIntervalSelection = true;
```

## Drag Behavior

### OnThumb

With `OnThumb` drag behavior (default), individual thumbs can be moved independently.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01" 
                               Maximum="2018-01-01" 
                               RangeStart="2012-01-01" 
                               RangeEnd="2016-01-01"
                               Interval="2" 
                               ShowTicks="True"
                               ShowLabels="True"
                               DragBehavior="OnThumb" />
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
rangeSlider.ShowLabels = true;  
rangeSlider.DragBehavior = SliderDragBehavior.OnThumb;
```

### BetweenThumbs

With `BetweenThumbs` drag behavior, both thumbs move together maintaining their distance. Individual thumb movement is not possible.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01" 
                               Maximum="2018-01-01" 
                               RangeStart="2012-01-01" 
                               RangeEnd="2016-01-01" 
                               Interval="2" 
                               ShowTicks="True"
                               ShowLabels="True"
                               DragBehavior="BetweenThumbs" />
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
rangeSlider.ShowLabels = true;   
rangeSlider.DragBehavior = SliderDragBehavior.BetweenThumbs;
```

### Both

With `Both` drag behavior, individual thumbs can be moved, and both thumbs can be moved together when dragging between them.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01" 
                               Maximum="2018-01-01" 
                               RangeStart="2012-01-01" 
                               RangeEnd="2016-01-01" 
                               Interval="2" 
                               ShowTicks="True"
                               ShowLabels="True"
                               DragBehavior="Both" />
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
rangeSlider.ShowLabels = true;  
rangeSlider.DragBehavior = SliderDragBehavior.Both;
```

## Deferred Update

Control when dependent components are updated during continuous thumb dragging. The default delay is `500` milliseconds.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01" 
                               Maximum="2018-01-01" 
                               RangeStart="2012-01-01" 
                               RangeEnd="2016-01-01" 
                               Interval="2"
                               ShowTicks="True"
                               ShowLabels="True"
                               EnableDeferredUpdate="True"
                               DeferredUpdateDelay="1000" />
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2018, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2016, 01, 01);
rangeSlider.Interval = 2;        
rangeSlider.ShowLabels = true;
rangeSlider.ShowTicks = true;    
rangeSlider.EnableDeferredUpdate = true;
rangeSlider.DeferredUpdateDelay = 1000;
```

### Deferred Update with Data Binding

Use deferred update to optimize performance when binding to external data sources.

**XAML:**
```xaml
<ContentPage.BindingContext>
    <local:ViewModel />
</ContentPage.BindingContext>

<VerticalStackLayout>
    <sliders:SfDateTimeRangeSlider Minimum="2010-01-01"
                                   Maximum="2020-01-01"
                                   RangeStart="{Binding StartDate}"
                                   RangeEnd="{Binding EndDate}"
                                   EnableDeferredUpdate="True"
                                   DeferredUpdateDelay="500" />
    
    <Label Text="{Binding StartDate, StringFormat='Start: {0:MMM dd, yyyy}'}" />
    <Label Text="{Binding EndDate, StringFormat='End: {0:MMM dd, yyyy}'}" />
</VerticalStackLayout>
```

**C#:**
```csharp
public class ViewModel : INotifyPropertyChanged
{
    private DateTime startDate = new DateTime(2012, 01, 01);
    private DateTime endDate = new DateTime(2018, 01, 01);

    public DateTime StartDate
    {
        get => startDate;
        set
        {
            startDate = value;
            OnPropertyChanged(nameof(StartDate));
        }
    }

    public DateTime EndDate
    {
        get => endDate;
        set
        {
            endDate = value;
            OnPropertyChanged(nameof(EndDate));
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

## Track Configuration

### Track Height

Customize the track height using the `TrackStyle` properties.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01"
                               Maximum="2018-01-01"
                               RangeStart="2012-01-01"
                               RangeEnd="2016-01-01">
    
    <sliders:SfDateTimeRangeSlider.TrackStyle>
        <sliders:SliderTrackStyle ActiveSize="8"
                                  InactiveSize="8" />
    </sliders:SfDateTimeRangeSlider.TrackStyle>

</sliders:SfDateTimeRangeSlider>
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2018, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2016, 01, 01);
rangeSlider.TrackStyle.ActiveSize = 8;
rangeSlider.TrackStyle.InactiveSize = 8;
```

### Track Colors

Customize active and inactive track colors.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01"
                               Maximum="2018-01-01"
                               RangeStart="2012-01-01"
                               RangeEnd="2016-01-01">
    
    <sliders:SfDateTimeRangeSlider.TrackStyle>
        <sliders:SliderTrackStyle ActiveFill="#EE3F3F"
                                  InactiveFill="#F7B1AE" />
    </sliders:SfDateTimeRangeSlider.TrackStyle>

</sliders:SfDateTimeRangeSlider>
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2018, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2016, 01, 01);
rangeSlider.TrackStyle.ActiveFill = new SolidColorBrush(Color.FromArgb("#EE3F3F"));
rangeSlider.TrackStyle.InactiveFill = new SolidColorBrush(Color.FromArgb("#F7B1AE"));
```

### Track with Gradient

Apply gradient colors to the track.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01"
                               Maximum="2018-01-01"
                               RangeStart="2012-01-01"
                               RangeEnd="2016-01-01">
    
    <sliders:SfDateTimeRangeSlider.TrackStyle>
        <sliders:SliderTrackStyle ActiveSize="8" InactiveSize="8">
            <sliders:SliderTrackStyle.ActiveFill>
                <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                    <GradientStop Color="#FFB457" Offset="0.0" />
                    <GradientStop Color="#EE3F3F" Offset="1.0" />
                </LinearGradientBrush>
            </sliders:SliderTrackStyle.ActiveFill>
            <sliders:SliderTrackStyle.InactiveFill>
                <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                    <GradientStop Color="#F7B1AE" Offset="0.0" />
                    <GradientStop Color="#FFCBA4" Offset="1.0" />
                </LinearGradientBrush>
            </sliders:SliderTrackStyle.InactiveFill>
        </sliders:SliderTrackStyle>
    </sliders:SfDateTimeRangeSlider.TrackStyle>

</sliders:SfDateTimeRangeSlider>
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider();
rangeSlider.Minimum = new DateTime(2010, 01, 01);
rangeSlider.Maximum = new DateTime(2018, 01, 01);
rangeSlider.RangeStart = new DateTime(2012, 01, 01);
rangeSlider.RangeEnd = new DateTime(2016, 01, 01);

LinearGradientBrush activeBrush = new LinearGradientBrush
{
    StartPoint = new Point(0, 0.5),
    EndPoint = new Point(1, 0.5),
    GradientStops = new GradientStopCollection
    {
        new GradientStop { Color = Color.FromArgb("#FFB457"), Offset = 0.0f },
        new GradientStop { Color = Color.FromArgb("#EE3F3F"), Offset = 1.0f }
    }
};

LinearGradientBrush inactiveBrush = new LinearGradientBrush
{
    StartPoint = new Point(0, 0.5),
    EndPoint = new Point(1, 0.5),
    GradientStops = new GradientStopCollection
    {
        new GradientStop { Color = Color.FromArgb("#F7B1AE"), Offset = 0.0f },
        new GradientStop { Color = Color.FromArgb("#FFCBA4"), Offset = 1.0f }
    }
};

rangeSlider.TrackStyle.ActiveFill = activeBrush;
rangeSlider.TrackStyle.InactiveFill = inactiveBrush;
rangeSlider.TrackStyle.ActiveSize = 8;
rangeSlider.TrackStyle.InactiveSize = 8;
```

## Property Reference

### Selection Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Minimum` | DateTime | DateTime(2000, 01, 01) | Minimum date value |
| `Maximum` | DateTime | DateTime(2000, 12, 31) | Maximum date value |
| `RangeStart` | DateTime | - | Start of the selected range |
| `RangeEnd` | DateTime | - | End of the selected range |
| `StepDuration` | SliderStepDuration | null | Discrete step interval |
| `EnableIntervalSelection` | bool | False | Enable interval-based selection |

### Interaction Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `DragBehavior` | SliderDragBehavior | OnThumb | Controls thumb drag behavior |
| `EnableDeferredUpdate` | bool | False | Enable deferred value updates |
| `DeferredUpdateDelay` | int | 500 | Delay in milliseconds for deferred updates |
| `IsEnabled` | bool | True | Enable or disable the slider |

### TrackStyle Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `ActiveFill` | Brush | - | Fill color for active track |
| `InactiveFill` | Brush | - | Fill color for inactive track |
| `ActiveSize` | double | 4.0 | Height of active track |
| `InactiveSize` | double | 4.0 | Height of inactive track |

### SliderDragBehavior Values

| Value | Description |
|-------|-------------|
| `OnThumb` | Individual thumbs can be moved independently |
| `BetweenThumbs` | Both thumbs move together maintaining distance |
| `Both` | Supports both individual and combined thumb movement |

### SliderStepDuration Constructor

```csharp
public SliderStepDuration(
    int years = 0,
    int months = 0,
    int days = 0,
    int hours = 0,
    int minutes = 0,
    int seconds = 0
)
```

**Examples:**
```csharp
// Step by 1 year
new SliderStepDuration(years: 1)

// Step by 6 months
new SliderStepDuration(months: 6)

// Step by 90 days
new SliderStepDuration(days: 90)

// Step by 1 month and 15 days
new SliderStepDuration(months: 1, days: 15)
```
