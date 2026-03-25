# Events and Commands in .NET MAUI DateTime Range Slider

This reference guide covers events and commands available in the Syncfusion .NET MAUI DateTime Range Slider (SfDateTimeRangeSlider) control.

## Table of Contents

- [Overview](#overview)
- [Value Events](#value-events)
  - [ValueChangeStart](#valuechangestart)
  - [ValueChanging](#valuechanging)
  - [ValueChanged](#valuechanged)
  - [ValueChangeEnd](#valuechangeend)
- [Label Events](#label-events)
  - [LabelCreated](#labelcreated)
- [Tooltip Events](#tooltip-events)
  - [TooltipLabelCreated](#tooltiplabelcreated)
- [Commands](#commands)
  - [DragStartedCommand](#dragstartedcommand)
  - [DragCompletedCommand](#dragcompletedcommand)
- [MVVM Pattern Examples](#mvvm-pattern-examples)
- [Property Reference](#property-reference)

## Overview

The DateTime Range Slider provides various events and commands to handle user interactions and customize the control's behavior. Events fire during thumb manipulation, while commands enable MVVM pattern integration.

## Value Events

### ValueChangeStart

Fired when the user starts selecting a new value by tapping or pressing down on the thumb.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01" 
                               Maximum="2020-01-01" 
                               RangeStart="2012-01-01" 
                               RangeEnd="2018-01-01" 
                               ValueChangeStart="OnValueChangeStart" />
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider()
{
    Minimum = new DateTime(2010, 01, 01),
    Maximum = new DateTime(2020, 01, 01),
    RangeStart = new DateTime(2012, 01, 01),
    RangeEnd = new DateTime(2018, 01, 01),
};
rangeSlider.ValueChangeStart += OnValueChangeStart;

private void OnValueChangeStart(object sender, EventArgs e)
{
    // Handle the start of value change
    Debug.WriteLine("User started dragging thumb");
}
```

### ValueChanging

Fired continuously while the user is dragging the thumb to select a new value. Use this event for real-time updates.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01" 
                               Maximum="2020-01-01" 
                               RangeStart="2012-01-01" 
                               RangeEnd="2018-01-01"
                               ValueChanging="OnValueChanging" />
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider()
{
    Minimum = new DateTime(2010, 01, 01),
    Maximum = new DateTime(2020, 01, 01),
    RangeStart = new DateTime(2012, 01, 01),
    RangeEnd = new DateTime(2018, 01, 01),
};
rangeSlider.ValueChanging += OnValueChanging;

private void OnValueChanging(object sender, DateTimeRangeSliderValueChangingEventArgs e)
{
    // Access new values during drag
    DateTime newStart = e.NewRangeStart;
    DateTime newEnd = e.NewRangeEnd;
    
    Debug.WriteLine($"Range changing: {newStart:d} to {newEnd:d}");
    
    // Cancel the change if needed
    // e.Cancel = true;
}
```

**ValueChanging EventArgs Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `NewRangeStart` | DateTime | The new start value being selected |
| `NewRangeEnd` | DateTime | The new end value being selected |
| `Cancel` | bool | Set to true to cancel the value change |

### ValueChanged

Fired when the user completes selecting a new value. This is the most commonly used event for responding to selection changes.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01" 
                               Maximum="2020-01-01" 
                               RangeStart="2012-01-01" 
                               RangeEnd="2018-01-01"
                               ValueChanged="OnValueChanged" />
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider()
{
    Minimum = new DateTime(2010, 01, 01),
    Maximum = new DateTime(2020, 01, 01),
    RangeStart = new DateTime(2012, 01, 01),
    RangeEnd = new DateTime(2018, 01, 01),
};
rangeSlider.ValueChanged += OnValueChanged;

private void OnValueChanged(object sender, DateTimeRangeSliderValueChangedEventArgs e)
{
    // Access old and new values
    DateTime oldStart = e.OldRangeStart;
    DateTime oldEnd = e.OldRangeEnd;
    DateTime newStart = e.NewRangeStart;
    DateTime newEnd = e.NewRangeEnd;
    
    Debug.WriteLine($"Range changed from {oldStart:d}-{oldEnd:d} to {newStart:d}-{newEnd:d}");
}
```

**ValueChanged EventArgs Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `OldRangeStart` | DateTime | The previous start value |
| `OldRangeEnd` | DateTime | The previous end value |
| `NewRangeStart` | DateTime | The new start value |
| `NewRangeEnd` | DateTime | The new end value |

### ValueChangeEnd

Fired when the user stops interacting with the slider by releasing the thumb.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01" 
                               Maximum="2020-01-01" 
                               RangeStart="2012-01-01" 
                               RangeEnd="2018-01-01"
                               ValueChangeEnd="OnValueChangeEnd" />
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider()
{
    Minimum = new DateTime(2010, 01, 01),
    Maximum = new DateTime(2020, 01, 01),
    RangeStart = new DateTime(2012, 01, 01),
    RangeEnd = new DateTime(2018, 01, 01),
};
rangeSlider.ValueChangeEnd += OnValueChangeEnd;

private void OnValueChangeEnd(object sender, EventArgs e)
{
    // Handle the end of value change
    Debug.WriteLine("User stopped dragging thumb");
}
```

### All Value Events Combined

Example using all value events together:

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01" 
                               Maximum="2020-01-01" 
                               RangeStart="2012-01-01" 
                               RangeEnd="2018-01-01" 
                               ValueChangeStart="OnValueChangeStart" 
                               ValueChanging="OnValueChanging" 
                               ValueChanged="OnValueChanged" 
                               ValueChangeEnd="OnValueChangeEnd" />
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider()
{
    Minimum = new DateTime(2010, 01, 01),
    Maximum = new DateTime(2020, 01, 01),
    RangeStart = new DateTime(2012, 01, 01),
    RangeEnd = new DateTime(2018, 01, 01),
};
rangeSlider.ValueChangeStart += OnValueChangeStart;
rangeSlider.ValueChanging += OnValueChanging;
rangeSlider.ValueChanged += OnValueChanged;
rangeSlider.ValueChangeEnd += OnValueChangeEnd;

private void OnValueChangeStart(object sender, EventArgs e)
{
    Debug.WriteLine("Value change started");
}

private void OnValueChanging(object sender, DateTimeRangeSliderValueChangingEventArgs e)
{
    Debug.WriteLine($"Value changing: {e.NewRangeStart:d} to {e.NewRangeEnd:d}");
}

private void OnValueChanged(object sender, DateTimeRangeSliderValueChangedEventArgs e)
{
    Debug.WriteLine($"Value changed to: {e.NewRangeStart:d} to {e.NewRangeEnd:d}");
}

private void OnValueChangeEnd(object sender, EventArgs e)
{
    Debug.WriteLine("Value change ended");
}
```

## Label Events

### LabelCreated

Customize or format label text using the `LabelCreated` event.

**XAML:**
```xaml
<sliders:SfDateTimeRangeSlider Minimum="2010-01-01"
                               Maximum="2011-01-01"
                               RangeStart="2010-04-01"
                               RangeEnd="2010-10-01"
                               Interval="3"
                               DateFormat="MMM"
                               ShowTicks="True"
                               LabelsPlacement="BetweenTicks"
                               IntervalType="Months"
                               LabelCreated="OnLabelCreated"
                               ShowLabels="True" />
```

**C#:**
```csharp
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider()
{
    Minimum = new DateTime(2010, 01, 01),
    Maximum = new DateTime(2011, 01, 01),
    RangeStart = new DateTime(2010, 04, 01),
    RangeEnd = new DateTime(2010, 10, 01),
    Interval = 3,
    DateFormat = "MMM",
    IntervalType = SliderDateIntervalType.Months,
    LabelsPlacement = SliderLabelsPlacement.BetweenTicks,
    ShowTicks = true,
    ShowLabels = true,
};
rangeSlider.LabelCreated += OnLabelCreated;

private void OnLabelCreated(object sender, SliderLabelCreatedEventArgs e)
{
    if (e.Text == "Jan")
    {
        e.Text = "Quarter 1";
    }
    else if (e.Text == "Apr")
    {
        e.Text = "Quarter 2";
    }
    else if (e.Text == "Jul")
    {
        e.Text = "Quarter 3";
    }
    else if (e.Text == "Oct")
    {
        e.Text = "Quarter 4";
    }
}
```

**Advanced Label Styling:**

```csharp
private void OnLabelCreated(object sender, SliderLabelCreatedEventArgs e)
{
    // Customize text
    e.Text = e.Text.ToUpper();
    
    // Customize style
    e.Style = new SliderLabelStyle
    {
        TextColor = Colors.Red,
        FontSize = 12,
        FontAttributes = FontAttributes.Bold,
        Offset = new Point(0, 10)
    };
}
```

**LabelCreated EventArgs Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `Text` | string | The label text to display |
| `Style` | SliderLabelStyle | Style properties for the label |

## Tooltip Events

### TooltipLabelCreated

Customize tooltip text and appearance using the `TooltipLabelCreated` event.

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

**Advanced Tooltip Customization:**

```csharp
private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    // Customize text
    e.Text = $"Selected: {e.Text}";
    
    // Customize appearance
    e.TextColor = Colors.White;
    e.FontSize = 14;
    e.FontAttributes = FontAttributes.Bold;
    e.FontFamily = "Arial";
}
```

**TooltipLabelCreated EventArgs Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `Text` | string | The tooltip text to display |
| `TextColor` | Color | Color of the tooltip text |
| `FontSize` | double | Font size of the tooltip text |
| `FontAttributes` | FontAttributes | Font attributes (Bold, Italic) |
| `FontFamily` | string | Font family for the tooltip text |

## Commands

### DragStartedCommand

Execute a command when the user starts dragging the thumb.

**XAML:**
```xaml
<ContentPage.BindingContext>
    <local:ViewModel />
</ContentPage.BindingContext>

<sliders:SfDateTimeRangeSlider Minimum="2010-01-01"
                               Maximum="2018-01-01"
                               RangeStart="2012-01-01"
                               RangeEnd="2016-01-01"
                               DragStartedCommand="{Binding DragStartedCommand}"
                               DragStartedCommandParameter="RangeSlider1" />
```

**C#:**
```csharp
// View
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider()
{
    Minimum = new DateTime(2010, 01, 01),
    Maximum = new DateTime(2018, 01, 01),
    RangeStart = new DateTime(2012, 01, 01),
    RangeEnd = new DateTime(2016, 01, 01),
    DragStartedCommand = viewModel.DragStartedCommand,
    DragStartedCommandParameter = "RangeSlider1"
};

// ViewModel
public class ViewModel
{
    public ICommand DragStartedCommand { get; }

    public ViewModel()
    {
        DragStartedCommand = new Command<string>(OnDragStarted);
    }

    private void OnDragStarted(string parameter)
    {
        Debug.WriteLine($"Drag started on {parameter}");
    }
}
```

### DragCompletedCommand

Execute a command when the user completes dragging the thumb.

**XAML:**
```xaml
<ContentPage.BindingContext>
    <local:ViewModel />
</ContentPage.BindingContext>

<sliders:SfDateTimeRangeSlider Minimum="2010-01-01"
                               Maximum="2018-01-01"
                               RangeStart="2012-01-01"
                               RangeEnd="2016-01-01"
                               DragCompletedCommand="{Binding DragCompletedCommand}"
                               DragCompletedCommandParameter="RangeSlider1" />
```

**C#:**
```csharp
// View
SfDateTimeRangeSlider rangeSlider = new SfDateTimeRangeSlider()
{
    Minimum = new DateTime(2010, 01, 01),
    Maximum = new DateTime(2018, 01, 01),
    RangeStart = new DateTime(2012, 01, 01),
    RangeEnd = new DateTime(2016, 01, 01),
    DragCompletedCommand = viewModel.DragCompletedCommand,
    DragCompletedCommandParameter = "RangeSlider1"
};

// ViewModel
public class ViewModel
{
    public ICommand DragCompletedCommand { get; }

    public ViewModel()
    {
        DragCompletedCommand = new Command<string>(OnDragCompleted);
    }

    private void OnDragCompleted(string parameter)
    {
        Debug.WriteLine($"Drag completed on {parameter}");
    }
}
```

## MVVM Pattern Examples

### Complete MVVM Implementation

**XAML:**
```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sliders="clr-namespace:Syncfusion.Maui.Sliders;assembly=Syncfusion.Maui.Sliders"
             xmlns:local="clr-namespace:YourNamespace"
             x:Class="YourNamespace.MainPage">
    
    <ContentPage.BindingContext>
        <local:DateRangeViewModel />
    </ContentPage.BindingContext>

    <VerticalStackLayout Padding="20" Spacing="20">
        
        <sliders:SfDateTimeRangeSlider Minimum="{Binding MinDate}"
                                       Maximum="{Binding MaxDate}"
                                       RangeStart="{Binding StartDate, Mode=TwoWay}"
                                       RangeEnd="{Binding EndDate, Mode=TwoWay}"
                                       Interval="1"
                                       ShowLabels="True"
                                       ShowTicks="True"
                                       DragStartedCommand="{Binding DragStartedCommand}"
                                       DragCompletedCommand="{Binding DragCompletedCommand}"
                                       ValueChanged="OnValueChanged">
            
            <sliders:SfDateTimeRangeSlider.Tooltip>
                <sliders:SliderTooltip />
            </sliders:SfDateTimeRangeSlider.Tooltip>
            
        </sliders:SfDateTimeRangeSlider>

        <Label Text="{Binding StartDate, StringFormat='Start Date: {0:MMM dd, yyyy}'}" />
        <Label Text="{Binding EndDate, StringFormat='End Date: {0:MMM dd, yyyy}'}" />
        <Label Text="{Binding StatusMessage}" />
        
    </VerticalStackLayout>
</ContentPage>
```

**C# ViewModel:**
```csharp
using System.ComponentModel;
using System.Runtime.CompilerServices;
using System.Windows.Input;

namespace YourNamespace
{
    public class DateRangeViewModel : INotifyPropertyChanged
    {
        private DateTime minDate;
        private DateTime maxDate;
        private DateTime startDate;
        private DateTime endDate;
        private string statusMessage;

        public DateRangeViewModel()
        {
            MinDate = new DateTime(2010, 01, 01);
            MaxDate = new DateTime(2025, 12, 31);
            StartDate = new DateTime(2015, 01, 01);
            EndDate = new DateTime(2020, 12, 31);
            StatusMessage = "Ready";

            DragStartedCommand = new Command(OnDragStarted);
            DragCompletedCommand = new Command(OnDragCompleted);
        }

        public DateTime MinDate
        {
            get => minDate;
            set => SetProperty(ref minDate, value);
        }

        public DateTime MaxDate
        {
            get => maxDate;
            set => SetProperty(ref maxDate, value);
        }

        public DateTime StartDate
        {
            get => startDate;
            set
            {
                if (SetProperty(ref startDate, value))
                {
                    UpdateStatusMessage();
                }
            }
        }

        public DateTime EndDate
        {
            get => endDate;
            set
            {
                if (SetProperty(ref endDate, value))
                {
                    UpdateStatusMessage();
                }
            }
        }

        public string StatusMessage
        {
            get => statusMessage;
            set => SetProperty(ref statusMessage, value);
        }

        public ICommand DragStartedCommand { get; }
        public ICommand DragCompletedCommand { get; }

        private void OnDragStarted()
        {
            StatusMessage = "Dragging...";
        }

        private void OnDragCompleted()
        {
            StatusMessage = "Selection completed";
            // Perform any action with the selected range
            var days = (EndDate - StartDate).Days;
            Debug.WriteLine($"Selected range: {days} days");
        }

        private void UpdateStatusMessage()
        {
            var days = (EndDate - StartDate).Days;
            StatusMessage = $"Selected: {days} days";
        }

        public event PropertyChangedEventHandler PropertyChanged;

        protected bool SetProperty<T>(ref T field, T value, [CallerMemberName] string propertyName = null)
        {
            if (EqualityComparer<T>.Default.Equals(field, value))
                return false;

            field = value;
            OnPropertyChanged(propertyName);
            return true;
        }

        protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

**Code-Behind:**
```csharp
private void OnValueChanged(object sender, DateTimeRangeSliderValueChangedEventArgs e)
{
    // Additional handling if needed
    Debug.WriteLine($"Range: {e.NewRangeStart:d} to {e.NewRangeEnd:d}");
}
```

## Property Reference

### Event Properties

| Event | EventArgs | Description |
|-------|-----------|-------------|
| `ValueChangeStart` | EventArgs | Fired when thumb interaction starts |
| `ValueChanging` | DateTimeRangeSliderValueChangingEventArgs | Fired continuously during dragging |
| `ValueChanged` | DateTimeRangeSliderValueChangedEventArgs | Fired when value selection completes |
| `ValueChangeEnd` | EventArgs | Fired when thumb interaction ends |
| `LabelCreated` | SliderLabelCreatedEventArgs | Fired when a label is created |
| `TooltipLabelCreated` | SliderTooltipLabelCreatedEventArgs | Fired when tooltip label is created |

### Command Properties

| Property | Type | Description |
|----------|------|-------------|
| `DragStartedCommand` | ICommand | Command executed when drag starts |
| `DragStartedCommandParameter` | object | Parameter for DragStartedCommand |
| `DragCompletedCommand` | ICommand | Command executed when drag completes |
| `DragCompletedCommandParameter` | object | Parameter for DragCompletedCommand |

### EventArgs Properties Summary

**DateTimeRangeSliderValueChangingEventArgs:**
- `NewRangeStart` (DateTime)
- `NewRangeEnd` (DateTime)
- `Cancel` (bool)

**DateTimeRangeSliderValueChangedEventArgs:**
- `OldRangeStart` (DateTime)
- `OldRangeEnd` (DateTime)
- `NewRangeStart` (DateTime)
- `NewRangeEnd` (DateTime)

**SliderLabelCreatedEventArgs:**
- `Text` (string)
- `Style` (SliderLabelStyle)

**SliderTooltipLabelCreatedEventArgs:**
- `Text` (string)
- `TextColor` (Color)
- `FontSize` (double)
- `FontAttributes` (FontAttributes)
- `FontFamily` (string)
