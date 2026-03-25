# Events and Commands

This guide covers event handling and command binding for the .NET MAUI Slider, including value change events, drag events, and MVVM command patterns.

## Table of Contents
- [Value Change Events Overview](#value-change-events-overview)
- [ValueChangeStart Event](#valuechangestart-event)
- [ValueChanging Event](#valuechanging-event)
- [ValueChanged Event](#valuechanged-event)
- [ValueChangeEnd Event](#valuechangeend-event)
- [DragStartedCommand](#dragstartedcommand)
- [DragCompletedCommand](#dragcompletedcommand)
- [Complete Examples](#complete-examples)

## Value Change Events Overview

The Slider provides four events to handle user interactions:

1. **ValueChangeStart**: Fires when the user taps/presses the thumb
2. **ValueChanging**: Fires continuously while the user drags the thumb
3. **ValueChanged**: Fires when the user releases the thumb after dragging
4. **ValueChangeEnd**: Fires when the user stops interacting (tap/mouse up)

**Event Flow:**
```
User taps thumb → ValueChangeStart
User drags → ValueChanging (multiple times)
User releases → ValueChanged → ValueChangeEnd
```

## ValueChangeStart Event

Fires when the user begins interacting with the slider by tapping or pressing the thumb.

### Event Signature

```csharp
public event EventHandler ValueChangeStart;
```

### Usage

**XAML:**
```xml
<sliders:SfSlider ValueChangeStart="OnValueChangeStart" />
```

**C#:**
```csharp
SfSlider slider = new SfSlider();
slider.ValueChangeStart += OnValueChangeStart;

private void OnValueChangeStart(object sender, EventArgs e)
{
    // User started interacting with slider
    Console.WriteLine("Slider interaction started");
}
```

### Use Cases

- Show a visual indicator that slider is being adjusted
- Log the start of user interaction
- Disable other UI elements during slider adjustment
- Start animations or transitions

### Example: Visual Feedback

```csharp
private Label statusLabel;

private void OnValueChangeStart(object sender, EventArgs e)
{
    statusLabel.Text = "Adjusting...";
    statusLabel.TextColor = Colors.Blue;
}
```

## ValueChanging Event

Fires continuously while the user is dragging the thumb. Provides real-time feedback of the current value.

### Event Signature

```csharp
public event EventHandler<SliderValueChangingEventArgs> ValueChanging;
```

### SliderValueChangingEventArgs Properties

- **Value** (double): The current value as the user drags the thumb

### Usage

**XAML:**
```xml
<sliders:SfSlider ValueChanging="OnValueChanging" />
```

**C#:**
```csharp
SfSlider slider = new SfSlider();
slider.ValueChanging += OnValueChanging;

private void OnValueChanging(object sender, SliderValueChangingEventArgs e)
{
    double currentValue = e.Value;
    Console.WriteLine($"Current value: {currentValue}");
}
```

### Use Cases

- Real-time display of current value
- Live preview of changes (color picker, volume control)
- Continuous validation during drag
- Update dependent UI elements in real-time

### Example: Real-Time Display

```xml
<VerticalStackLayout Spacing="10">
    <Label x:Name="valueLabel" Text="Value: 50" FontSize="16" />
    <sliders:SfSlider Minimum="0"
                      Maximum="100"
                      Value="50"
                      ValueChanging="OnValueChanging" />
</VerticalStackLayout>
```

```csharp
private void OnValueChanging(object sender, SliderValueChangingEventArgs e)
{
    valueLabel.Text = $"Value: {e.Value:F0}";
}
```

### Example: Live Preview

```csharp
private BoxView previewBox;

private void OnValueChanging(object sender, SliderValueChangingEventArgs e)
{
    // Update opacity in real-time
    previewBox.Opacity = e.Value / 100.0;
}
```

### Performance Consideration

`ValueChanging` fires many times per second during drag. Avoid expensive operations:

```csharp
// ❌ Bad: Heavy operation in ValueChanging
private void OnValueChanging(object sender, SliderValueChangingEventArgs e)
{
    PerformExpensiveCalculation(e.Value);  // Called 100+ times during drag
}

// ✅ Good: Use ValueChanged for heavy operations
private void OnValueChanged(object sender, SliderValueChangedEventArgs e)
{
    PerformExpensiveCalculation(e.NewValue);  // Called once after drag
}
```

## ValueChanged Event

Fires when the user completes a value change by releasing the thumb.

### Event Signature

```csharp
public event EventHandler<SliderValueChangedEventArgs> ValueChanged;
```

### SliderValueChangedEventArgs Properties

- **NewValue** (double): The new value after the change
- **OldValue** (double): The previous value before the change

### Usage

**XAML:**
```xml
<sliders:SfSlider ValueChanged="OnValueChanged" />
```

**C#:**
```csharp
SfSlider slider = new SfSlider();
slider.ValueChanged += OnValueChanged;

private void OnValueChanged(object sender, SliderValueChangedEventArgs e)
{
    double newValue = e.NewValue;
    double oldValue = e.OldValue;
    double change = newValue - oldValue;
    
    Console.WriteLine($"Value changed from {oldValue} to {newValue} (change: {change})");
}
```

### Use Cases

- Save the new value to database or settings
- Trigger API calls based on final value
- Update models and ViewModels
- Perform validation and apply changes
- Log user actions

### Example: Save Settings

```csharp
private async void OnValueChanged(object sender, SliderValueChangedEventArgs e)
{
    // Save volume setting
    Preferences.Set("Volume", e.NewValue);
    
    // Or update ViewModel
    viewModel.Volume = e.NewValue;
    
    // Or call API
    await UpdateVolumeAsync(e.NewValue);
}
```

### Example: Conditional Logic

```csharp
private void OnValueChanged(object sender, SliderValueChangedEventArgs e)
{
    if (e.NewValue > e.OldValue)
    {
        Console.WriteLine("Value increased");
    }
    else if (e.NewValue < e.OldValue)
    {
        Console.WriteLine("Value decreased");
    }
    
    // Apply different actions based on value ranges
    if (e.NewValue < 30)
    {
        statusLabel.Text = "Low";
        statusLabel.TextColor = Colors.Blue;
    }
    else if (e.NewValue < 70)
    {
        statusLabel.Text = "Medium";
        statusLabel.TextColor = Colors.Orange;
    }
    else
    {
        statusLabel.Text = "High";
        statusLabel.TextColor = Colors.Red;
    }
}
```

## ValueChangeEnd Event

Fires when the user stops interacting with the slider (releases the thumb).

### Event Signature

```csharp
public event EventHandler ValueChangeEnd;
```

### Usage

**XAML:**
```xml
<sliders:SfSlider ValueChangeEnd="OnValueChangeEnd" />
```

**C#:**
```csharp
SfSlider slider = new SfSlider();
slider.ValueChangeEnd += OnValueChangeEnd;

private void OnValueChangeEnd(object sender, EventArgs e)
{
    Console.WriteLine("Slider interaction ended");
}
```

### Use Cases

- Hide visual indicators shown during interaction
- Re-enable UI elements disabled during adjustment
- Complete animations or transitions
- Log the end of interaction

### Example: Clear Status

```csharp
private void OnValueChangeEnd(object sender, EventArgs e)
{
    statusLabel.Text = "Ready";
    statusLabel.TextColor = Colors.Gray;
}
```

### Complete Event Flow Example

```csharp
private void OnValueChangeStart(object sender, EventArgs e)
{
    Console.WriteLine("1. Interaction started");
    statusLabel.Text = "Adjusting...";
    statusLabel.TextColor = Colors.Blue;
}

private void OnValueChanging(object sender, SliderValueChangingEventArgs e)
{
    Console.WriteLine($"2. Current value: {e.Value:F1}");
    valueLabel.Text = $"{e.Value:F0}";
}

private void OnValueChanged(object sender, SliderValueChangedEventArgs e)
{
    Console.WriteLine($"3. Value changed: {e.OldValue} → {e.NewValue}");
    SaveValue(e.NewValue);
}

private void OnValueChangeEnd(object sender, EventArgs e)
{
    Console.WriteLine("4. Interaction ended");
    statusLabel.Text = "Done";
    statusLabel.TextColor = Colors.Green;
}
```

## DragStartedCommand

The `DragStartedCommand` is executed when the user starts dragging the thumb. This is useful for MVVM patterns where you want to handle interactions in a ViewModel.

### Command Setup

**XAML with ViewModel Binding:**
```xml
<ContentPage.BindingContext>
    <local:ViewModel />
</ContentPage.BindingContext>

<sliders:SfSlider DragStartedCommand="{Binding DragStartedCommand}" />
```

**ViewModel:**
```csharp
using System.Windows.Input;

public class ViewModel
{
    public ICommand DragStartedCommand { get; }
    
    public ViewModel()
    {
        DragStartedCommand = new Command(OnDragStarted);
    }
    
    private void OnDragStarted(object obj)
    {
        Console.WriteLine("Drag started");
        // Handle drag start in ViewModel
    }
}
```

### C# Code-Behind Setup

```csharp
SfSlider slider = new SfSlider
{
    DragStartedCommand = viewModel.DragStartedCommand
};
```

### DragStartedCommandParameter

Pass parameters to the command:

**XAML:**
```xml
<sliders:SfSlider DragStartedCommand="{Binding DragStartedCommand}"
                  DragStartedCommandParameter="VolumeSlider" />
```

**ViewModel:**
```csharp
public ViewModel()
{
    DragStartedCommand = new Command<string>(OnDragStarted);
}

private void OnDragStarted(string parameter)
{
    Console.WriteLine($"Drag started on: {parameter}");
}
```

### Example with Command Parameter

```xml
<VerticalStackLayout>
    <sliders:SfSlider DragStartedCommand="{Binding DragStartedCommand}"
                      DragStartedCommandParameter="1" />
    
    <sliders:SfSlider DragStartedCommand="{Binding DragStartedCommand}"
                      DragStartedCommandParameter="2" />
</VerticalStackLayout>
```

```csharp
private void OnDragStarted(object parameter)
{
    int sliderId = Convert.ToInt32(parameter);
    Console.WriteLine($"Slider {sliderId} drag started");
}
```

## DragCompletedCommand

The `DragCompletedCommand` is executed when the user completes dragging the thumb.

### Command Setup

**XAML with ViewModel Binding:**
```xml
<ContentPage.BindingContext>
    <local:ViewModel />
</ContentPage.BindingContext>

<sliders:SfSlider DragCompletedCommand="{Binding DragCompletedCommand}" />
```

**ViewModel:**
```csharp
public class ViewModel
{
    public ICommand DragCompletedCommand { get; }
    
    public ViewModel()
    {
        DragCompletedCommand = new Command(OnDragCompleted);
    }
    
    private void OnDragCompleted(object obj)
    {
        Console.WriteLine("Drag completed");
        // Handle drag completion in ViewModel
    }
}
```

### DragCompletedCommandParameter

Pass parameters to the command:

**XAML:**
```xml
<sliders:SfSlider DragCompletedCommand="{Binding DragCompletedCommand}"
                  DragCompletedCommandParameter="1" />
```

**ViewModel:**
```csharp
public ViewModel()
{
    DragCompletedCommand = new Command<int>(OnDragCompleted);
}

private void OnDragCompleted(int sliderId)
{
    Console.WriteLine($"Slider {sliderId} drag completed");
    // Save state for this specific slider
}
```

### Combined Drag Commands

```xml
<sliders:SfSlider DragStartedCommand="{Binding DragStartedCommand}"
                  DragCompletedCommand="{Binding DragCompletedCommand}"
                  Value="{Binding Volume, Mode=TwoWay}" />
```

```csharp
public class ViewModel : INotifyPropertyChanged
{
    private double _volume = 50;
    private bool _isDragging;
    
    public double Volume
    {
        get => _volume;
        set
        {
            if (_volume != value)
            {
                _volume = value;
                OnPropertyChanged();
            }
        }
    }
    
    public ICommand DragStartedCommand { get; }
    public ICommand DragCompletedCommand { get; }
    
    public ViewModel()
    {
        DragStartedCommand = new Command(OnDragStarted);
        DragCompletedCommand = new Command(OnDragCompleted);
    }
    
    private void OnDragStarted(object obj)
    {
        _isDragging = true;
        Console.WriteLine("Started adjusting volume");
    }
    
    private void OnDragCompleted(object obj)
    {
        _isDragging = false;
        SaveVolumeSettings(Volume);
        Console.WriteLine($"Volume saved: {Volume}");
    }
    
    private void SaveVolumeSettings(double volume)
    {
        Preferences.Set("Volume", volume);
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

## Complete Examples

### Example 1: Volume Control with Events

**XAML:**
```xml
<VerticalStackLayout Padding="20" Spacing="15">
    <Label Text="Volume Control" FontSize="18" FontAttributes="Bold" />
    
    <Label x:Name="volumeLabel" Text="Volume: 50%" FontSize="16" />
    <Label x:Name="statusLabel" Text="Ready" FontSize="14" TextColor="Gray" />
    
    <sliders:SfSlider x:Name="volumeSlider"
                      Minimum="0"
                      Maximum="100"
                      Value="50"
                      Interval="25"
                      ShowLabels="True"
                      ValueChangeStart="OnVolumeChangeStart"
                      ValueChanging="OnVolumeChanging"
                      ValueChanged="OnVolumeChanged"
                      ValueChangeEnd="OnVolumeChangeEnd" />
</VerticalStackLayout>
```

**C#:**
```csharp
private void OnVolumeChangeStart(object sender, EventArgs e)
{
    statusLabel.Text = "Adjusting volume...";
    statusLabel.TextColor = Colors.Blue;
}

private void OnVolumeChanging(object sender, SliderValueChangingEventArgs e)
{
    volumeLabel.Text = $"Volume: {e.Value:F0}%";
}

private void OnVolumeChanged(object sender, SliderValueChangedEventArgs e)
{
    // Save to preferences
    Preferences.Set("Volume", e.NewValue);
    
    // Apply volume change
    AudioManager.SetVolume(e.NewValue / 100.0);
}

private void OnVolumeChangeEnd(object sender, EventArgs e)
{
    statusLabel.Text = "Volume saved";
    statusLabel.TextColor = Colors.Green;
}
```

### Example 2: MVVM Pattern with Commands

**XAML:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sliders="clr-namespace:Syncfusion.Maui.Sliders;assembly=Syncfusion.Maui.Sliders"
             xmlns:local="clr-namespace:MyApp"
             x:Class="MyApp.SliderPage">
    
    <ContentPage.BindingContext>
        <local:SliderViewModel />
    </ContentPage.BindingContext>
    
    <VerticalStackLayout Padding="20" Spacing="15">
        <Label Text="{Binding StatusMessage}" FontSize="16" />
        <Label Text="{Binding CurrentValueText}" FontSize="18" FontAttributes="Bold" />
        
        <sliders:SfSlider Minimum="0"
                          Maximum="100"
                          Value="{Binding SliderValue, Mode=TwoWay}"
                          DragStartedCommand="{Binding DragStartedCommand}"
                          DragCompletedCommand="{Binding DragCompletedCommand}"
                          ShowLabels="True"
                          Interval="25" />
    </VerticalStackLayout>
</ContentPage>
```

**ViewModel:**
```csharp
using System.ComponentModel;
using System.Runtime.CompilerServices;
using System.Windows.Input;

public class SliderViewModel : INotifyPropertyChanged
{
    private double _sliderValue = 50;
    private string _statusMessage = "Ready";
    
    public double SliderValue
    {
        get => _sliderValue;
        set
        {
            if (_sliderValue != value)
            {
                _sliderValue = value;
                OnPropertyChanged();
                OnPropertyChanged(nameof(CurrentValueText));
            }
        }
    }
    
    public string StatusMessage
    {
        get => _statusMessage;
        set
        {
            if (_statusMessage != value)
            {
                _statusMessage = value;
                OnPropertyChanged();
            }
        }
    }
    
    public string CurrentValueText => $"Value: {SliderValue:F0}";
    
    public ICommand DragStartedCommand { get; }
    public ICommand DragCompletedCommand { get; }
    
    public SliderViewModel()
    {
        DragStartedCommand = new Command(OnDragStarted);
        DragCompletedCommand = new Command(OnDragCompleted);
        
        // Load saved value
        SliderValue = Preferences.Get("SliderValue", 50.0);
    }
    
    private void OnDragStarted(object obj)
    {
        StatusMessage = "Adjusting...";
    }
    
    private void OnDragCompleted(object obj)
    {
        // Save value
        Preferences.Set("SliderValue", SliderValue);
        StatusMessage = $"Saved: {SliderValue:F0}";
        
        // Optionally reset message after delay
        Task.Delay(2000).ContinueWith(_ => 
        {
            MainThread.BeginInvokeOnMainThread(() => 
            {
                StatusMessage = "Ready";
            });
        });
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
- **ValueChangeStart/End**: UI feedback (show/hide indicators)
- **ValueChanging**: Real-time display updates (lightweight operations only)
- **ValueChanged**: Save data, API calls, heavy operations

### Performance
- Avoid expensive operations in `ValueChanging` (fires many times)
- Use `ValueChanged` for database saves and API calls
- Consider debouncing if updates are too frequent

### MVVM Pattern
- Use Commands for ViewModel communication
- Bind `Value` property with `Mode=TwoWay`
- Use `DragCompletedCommand` to save final values

### Error Handling
```csharp
private async void OnValueChanged(object sender, SliderValueChangedEventArgs e)
{
    try
    {
        await SaveValueAsync(e.NewValue);
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", $"Failed to save: {ex.Message}", "OK");
    }
}
```

## Summary

Key events and commands:

**Events:**
- `ValueChangeStart`: User starts interaction
- `ValueChanging`: Fires continuously during drag (real-time updates)
- `ValueChanged`: Fires once after drag completes (save data)
- `ValueChangeEnd`: User stops interaction

**Commands (MVVM):**
- `DragStartedCommand`: Executed when drag starts
- `DragCompletedCommand`: Executed when drag completes
- `DragStartedCommandParameter`: Pass data to DragStartedCommand
- `DragCompletedCommandParameter`: Pass data to DragCompletedCommand

Use events for code-behind scenarios, and commands for MVVM pattern with ViewModels.
