# Events and Commands

This guide covers events and commands in the .NET MAUI Range Selector (SfRangeSelector) for handling value changes, customizing labels/tooltips, and implementing MVVM patterns.

## Table of Contents
- [Overview](#overview)
- [Value Change Events](#value-change-events)
- [Label Customization Event](#label-customization-event)
- [Tooltip Customization Event](#tooltip-customization-event)
- [Commands](#commands)
- [Complete MVVM Example](#complete-mvvm-example)
- [Best Practices](#best-practices)

## Overview

Events and commands enable reactive programming and MVVM patterns:
- **Events**: Handle value changes and customize text
- **Commands**: Integrate with ViewModels for MVVM architecture

## Value Change Events

Track user interaction and value changes through four lifecycle events.

**Events:**
- `ValueChangeStart`: Fires when user starts dragging thumb (touch down)
- `ValueChanging`: Fires continuously while dragging
- `ValueChanged`: Fires when new value is selected (during drag)
- `ValueChangeEnd`: Fires when user stops dragging (touch up)

### ValueChangeStart

Fires when user begins thumb interaction.

**Event Args:** `EventArgs`

**XAML:**
```xaml
<sliders:SfRangeSelector ValueChangeStart="OnValueChangeStart"
                         Minimum="0" Maximum="100"
                         RangeStart="25" RangeEnd="75">
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
rangeSelector.ValueChangeStart += OnValueChangeStart;

private void OnValueChangeStart(object sender, EventArgs e)
{
    Debug.WriteLine("User started dragging");
    // Show loading indicator, disable other controls, etc.
}
```

### ValueChanging

Fires continuously during drag. Use for real-time updates.

**Event Args:** `RangeSelectorValueChangingEventArgs`
- `NewValue` (RangeSliderValueChangeEventArgs): New range values
  - `NewRangeStart` (double): New start value
  - `NewRangeEnd` (double): New end value
- `Cancel` (bool): Set to true to cancel the value change

**XAML:**
```xaml
<sliders:SfRangeSelector ValueChanging="OnValueChanging"
                         Minimum="0" Maximum="100"
                         RangeStart="25" RangeEnd="75">
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
rangeSelector.ValueChanging += OnValueChanging;

private void OnValueChanging(object sender, RangeSelectorValueChangingEventArgs e)
{
    Debug.WriteLine($"Changing: {e.NewRangeStart} - {e.NewRangeEnd}");
    
    // Update UI in real-time
    StartLabel.Text = e.NewRangeStart.ToString("F2");
    EndLabel.Text = e.NewRangeEnd.ToString("F2");
}
```

**Cancel Invalid Ranges:**
```csharp
private void OnValueChanging(object sender, RangeSelectorValueChangingEventArgs e)
{
    // Prevent ranges smaller than 10
    if (e.NewRangeEnd - e.NewRangeStart < 10)
    {
        e.Cancel = true;
    }
}
```

### ValueChanged

Fires when a new value is selected (during or after drag completion).

**Event Args:** `RangeSelectorValueChangedEventArgs`
- `NewValue` (RangeSliderValueChangeEventArgs): New range values
  - `NewRangeStart` (double): New start value
  - `NewRangeEnd` (double): New end value
- `OldValue` (RangeSliderValueChangeEventArgs): Previous range values
  - `OldRangeStart` (double): Previous start value
  - `OldRangeEnd` (double): Previous end value

**XAML:**
```xaml
<sliders:SfRangeSelector ValueChanged="OnValueChanged"
                         Minimum="0" Maximum="100"
                         RangeStart="25" RangeEnd="75">
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
rangeSelector.ValueChanged += OnValueChanged;

private void OnValueChanged(object sender, RangeSelectorValueChangedEventArgs e)
{
    // Update chart data, trigger API calls, etc.
    FilterChartData(e.NewRangeStart, e.NewRangeEnd);
}
```

### ValueChangeEnd

Fires when user stops interacting (touch up).

**Event Args:** `EventArgs`

**XAML:**
```xaml
<sliders:SfRangeSelector ValueChangeEnd="OnValueChangeEnd"
                         Minimum="0" Maximum="100"
                         RangeStart="25" RangeEnd="75">
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
rangeSelector.ValueChangeEnd += OnValueChangeEnd;

private void OnValueChangeEnd(object sender, EventArgs e)
{
    Debug.WriteLine("User finished dragging");
    // Save values, hide loading indicator, etc.
    SaveRangePreferences();
}
```

### Event Lifecycle

```
User touches thumb
    ↓
ValueChangeStart fires
    ↓
User drags thumb
    ↓
ValueChanging fires (continuously)
ValueChanged fires (when value changes)
    ↓
User releases thumb
    ↓
ValueChangeEnd fires
```

## Label Customization Event

Customize label text dynamically when labels are created.

**Event:** `LabelCreated`

**Event Args:** `SliderLabelCreatedEventArgs`
- `Text` (string): Get/set label text
- `Style` (SliderLabelStyle): Get/set label style

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="2" Maximum="10"
                         RangeStart="4" RangeEnd="8"
                         Interval="2"
                         ShowLabels="True"
                         ShowTicks="True"
                         LabelCreated="OnLabelCreated">
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
rangeSelector.LabelCreated += OnLabelCreated;

private void OnLabelCreated(object sender, SliderLabelCreatedEventArgs e)
{
    // Add currency prefix
    e.Text = "$" + e.Text;
}
```

**See:** [labels-customization.md](labels-customization.md#custom-label-text) for more examples.

## Tooltip Customization Event

Customize tooltip text and styling when tooltips are shown.

**Event:** `Tooltip.TooltipLabelCreated`

**Event Args:** `SliderTooltipLabelCreatedEventArgs`
- `Text` (string): Get/set tooltip text
- `TextColor` (Color): Get/set text color
- `FontSize` (double): Get/set font size
- `FontFamily` (string): Get/set font family
- `FontAttributes` (FontAttributes): Get/set font attributes

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="2" Maximum="10"
                         RangeStart="4" RangeEnd="8"
                         Interval="2">
    <sliders:SfRangeSelector.Tooltip>
        <sliders:SliderTooltip TooltipLabelCreated="OnTooltipLabelCreated" />
    </sliders:SfRangeSelector.Tooltip>
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**C#:**
```csharp
rangeSelector.Tooltip = new SliderTooltip();
rangeSelector.Tooltip.TooltipLabelCreated += OnTooltipLabelCreated;

private void OnTooltipLabelCreated(object sender, SliderTooltipLabelCreatedEventArgs e)
{
    // Add currency prefix
    e.Text = "$" + e.Text;
}
```

**See:** [tooltip-configuration.md](tooltip-configuration.md#custom-tooltip-text) for more examples.

## Commands

Commands enable MVVM pattern by binding range selector actions to ViewModel commands.

**Available Commands:**
- `DragStartedCommand`: Executes when drag starts
- `DragCompletedCommand`: Executes when drag completes

**Parameters:**
- `DragStartedCommandParameter`: Optional parameter for DragStartedCommand
- `DragCompletedCommandParameter`: Optional parameter for DragCompletedCommand

### DragStartedCommand

**XAML:**
```xaml
<ContentPage.BindingContext>
    <local:RangeSelectorViewModel />
</ContentPage.BindingContext>

<sliders:SfRangeSelector Minimum="0" Maximum="100"
                         RangeStart="{Binding SelectedStart}"
                         RangeEnd="{Binding SelectedEnd}"
                         DragStartedCommand="{Binding DragStartedCommand}">
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**ViewModel:**
```csharp
public class RangeSelectorViewModel : INotifyPropertyChanged
{
    public ICommand DragStartedCommand { get; }

    public RangeSelectorViewModel()
    {
        DragStartedCommand = new Command(OnDragStarted);
    }

    private void OnDragStarted(object parameter)
    {
        Debug.WriteLine("Drag started");
        // Show loading, disable buttons, etc.
    }
}
```

### DragCompletedCommand

**XAML:**
```xaml
<sliders:SfRangeSelector Minimum="0" Maximum="100"
                         RangeStart="{Binding SelectedStart}"
                         RangeEnd="{Binding SelectedEnd}"
                         DragCompletedCommand="{Binding DragCompletedCommand}">
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**ViewModel:**
```csharp
public ICommand DragCompletedCommand { get; }

public RangeSelectorViewModel()
{
    DragCompletedCommand = new Command(OnDragCompleted);
}

private void OnDragCompleted(object parameter)
{
    Debug.WriteLine($"Drag completed: {SelectedStart} - {SelectedEnd}");
    // Save preferences, trigger API call, etc.
    SaveRangeAsync(SelectedStart, SelectedEnd);
}
```

### Command Parameters

**XAML:**
```xaml
<sliders:SfRangeSelector DragStartedCommand="{Binding DragStartedCommand}"
                         DragStartedCommandParameter="PriceRange"
                         DragCompletedCommand="{Binding DragCompletedCommand}"
                         DragCompletedCommandParameter="PriceRange">
    <charts:SfCartesianChart><!-- Chart --></charts:SfCartesianChart>
</sliders:SfRangeSelector>
```

**ViewModel:**
```csharp
private void OnDragStarted(object parameter)
{
    string rangeType = parameter as string;  // "PriceRange"
    Debug.WriteLine($"Started dragging {rangeType}");
}

private void OnDragCompleted(object parameter)
{
    string rangeType = parameter as string;  // "PriceRange"
    Debug.WriteLine($"Completed dragging {rangeType}");
    
    if (rangeType == "PriceRange")
    {
        FilterProductsByPrice();
    }
}
```

## Complete MVVM Example

**XAML:**
```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sliders="clr-namespace:Syncfusion.Maui.Sliders;assembly=Syncfusion.Maui.Sliders"
             xmlns:local="clr-namespace:MyApp.ViewModels"
             x:Class="MyApp.Views.PriceFilterPage">
    
    <ContentPage.BindingContext>
        <local:PriceFilterViewModel />
    </ContentPage.BindingContext>

    <StackLayout Padding="20">
        <Label Text="{Binding RangeDescription}" FontSize="16" />
        
        <sliders:SfRangeSelector Minimum="0"
                                 Maximum="1000"
                                 RangeStart="{Binding MinPrice, Mode=TwoWay}"
                                 RangeEnd="{Binding MaxPrice, Mode=TwoWay}"
                                 Interval="100"
                                 NumberFormat="$#,0"
                                 ShowLabels="True"
                                 ShowTicks="True"
                                 DragStartedCommand="{Binding DragStartedCommand}"
                                 DragCompletedCommand="{Binding DragCompletedCommand}">
            <sliders:SfRangeSelector.Tooltip>
                <sliders:SliderTooltip NumberFormat="$#,0" />
            </sliders:SfRangeSelector.Tooltip>
        </sliders:SfRangeSelector>
        
        <Button Text="Apply Filter"
                Command="{Binding ApplyFilterCommand}"
                IsEnabled="{Binding CanApplyFilter}" />
    </StackLayout>
</ContentPage>
```

**ViewModel:**
```csharp
using System.ComponentModel;
using System.Runtime.CompilerServices;
using System.Windows.Input;

public class PriceFilterViewModel : INotifyPropertyChanged
{
    private double _minPrice = 0;
    private double _maxPrice = 1000;
    private bool _isDragging;

    public double MinPrice
    {
        get => _minPrice;
        set
        {
            if (_minPrice != value)
            {
                _minPrice = value;
                OnPropertyChanged();
                OnPropertyChanged(nameof(RangeDescription));
            }
        }
    }

    public double MaxPrice
    {
        get => _maxPrice;
        set
        {
            if (_maxPrice != value)
            {
                _maxPrice = value;
                OnPropertyChanged();
                OnPropertyChanged(nameof(RangeDescription));
            }
        }
    }

    public string RangeDescription =>
        $"Price Range: ${MinPrice:N0} - ${MaxPrice:N0}";

    public bool CanApplyFilter => !_isDragging;

    public ICommand DragStartedCommand { get; }
    public ICommand DragCompletedCommand { get; }
    public ICommand ApplyFilterCommand { get; }

    public PriceFilterViewModel()
    {
        DragStartedCommand = new Command(OnDragStarted);
        DragCompletedCommand = new Command(OnDragCompleted);
        ApplyFilterCommand = new Command(OnApplyFilter);
    }

    private void OnDragStarted(object parameter)
    {
        _isDragging = true;
        OnPropertyChanged(nameof(CanApplyFilter));
    }

    private void OnDragCompleted(object parameter)
    {
        _isDragging = false;
        OnPropertyChanged(nameof(CanApplyFilter));
        
        // Auto-apply filter after drag
        ApplyPriceFilter();
    }

    private void OnApplyFilter()
    {
        ApplyPriceFilter();
    }

    private void ApplyPriceFilter()
    {
        // Filter products, update chart, trigger API call, etc.
        Debug.WriteLine($"Filtering products: ${MinPrice} - ${MaxPrice}");
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

## Best Practices

### Event Usage
- Use `ValueChangeStart` for preparation (show loading, disable UI)
- Use `ValueChanging` for real-time updates (charts, previews)
- Use `ValueChanged` for state updates and side effects
- Use `ValueChangeEnd` for cleanup and finalization

### Performance
- Avoid heavy operations in `ValueChanging` (fires continuously)
- Use `EnableDeferredUpdate` for expensive operations
- Debounce API calls triggered by value changes
- Cache computed values when possible

### MVVM Pattern
- Bind `RangeStart`/`RangeEnd` with `TwoWay` mode
- Use commands instead of event handlers
- Keep business logic in ViewModels
- Use command parameters to identify multiple selectors

### Error Handling
- Validate ranges in `ValueChanging` (use `e.Cancel`)
- Handle edge cases (Min=Max, Start=End)
- Provide user feedback for invalid ranges
- Log errors for debugging
