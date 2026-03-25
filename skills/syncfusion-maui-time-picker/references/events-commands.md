# Events and Commands in .NET MAUI TimePicker

## Table of Contents
- [Overview](#overview)
- [SelectionChanged Event](#selectionchanged-event)
- [Dialog Mode Events](#dialog-mode-events)
- [Footer View Events](#footer-view-events)
- [Commands](#commands)
- [IsSelectionImmediate Property](#isselectionimmediate-property)
- [Event Handling Patterns](#event-handling-patterns)
- [Best Practices](#best-practices)

## Overview

The TimePicker provides a comprehensive event system to track user interactions and state changes. Events are categorized into:

1. **Selection Events** - Track time selection changes
2. **Dialog Events** - Monitor picker open/close lifecycle
3. **Footer Events** - Handle OK/Cancel button clicks
4. **Commands** - MVVM-friendly command bindings

## SelectionChanged Event

The `SelectionChanged` event is raised when the selected time changes in the TimePicker.

**Event Signature:**
```csharp
public event EventHandler<TimePickerSelectionChangedEventArgs> SelectionChanged;
```

**EventArgs:**
```csharp
public class TimePickerSelectionChangedEventArgs : EventArgs
{
    public TimeSpan? NewValue { get; }
    public TimeSpan? OldValue { get; }
}
```

### Basic SelectionChanged Example

**XAML:**
```xml
<picker:SfTimePicker x:Name="timePicker"
                     SelectionChanged="OnTimePickerSelectionChanged"
                     Format="hh_mm_tt">
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView Text="Select Time" Height="40" />
    </picker:SfTimePicker.HeaderView>
    <picker:SfTimePicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True" Height="40" />
    </picker:SfTimePicker.FooterView>
</picker:SfTimePicker>
```

**C#:**
```csharp
private void OnTimePickerSelectionChanged(object sender, TimePickerSelectionChangedEventArgs e)
{
    var oldTime = e.OldValue;
    var newTime = e.NewValue;
    
    if (newTime.HasValue)
    {
        DisplayAlert("Time Changed", 
                     $"Old: {oldTime?.ToString(@"hh\:mm tt") ?? "None"}\n" +
                     $"New: {newTime.Value.ToString(@"hh\:mm tt")}", 
                     "OK");
    }
}
```

### Event Subscription in Code

```csharp
SfTimePicker timePicker = new SfTimePicker()
{
    Format = PickerTimeFormat.hh_mm_tt
};

// Subscribe to event
timePicker.SelectionChanged += OnTimePickerSelectionChanged;

// Unsubscribe from event
timePicker.SelectionChanged -= OnTimePickerSelectionChanged;

this.Content = timePicker;
```

### Accessing Event Data

```csharp
private void OnTimePickerSelectionChanged(object sender, TimePickerSelectionChangedEventArgs e)
{
    // Get the TimePicker instance
    var picker = sender as SfTimePicker;
    
    // Access old and new values
    TimeSpan? oldValue = e.OldValue;
    TimeSpan? newValue = e.NewValue;
    
    // Check if values exist
    if (oldValue.HasValue)
    {
        Console.WriteLine($"Previous time: {oldValue.Value}");
    }
    
    if (newValue.HasValue)
    {
        Console.WriteLine($"New time: {newValue.Value}");
        
        // Perform actions based on new value
        UpdateAppointmentTime(newValue.Value);
    }
}
```

### Selection Confirmation Behavior

**Important:** In `SfTimePicker`, the `SelectedTime` confirmation behavior depends on the configuration:

**When OK Button is Required:**
- `Mode` is `Dialog` or `RelativeDialog`
- `FooterView.Height` > 0
- `FooterView.ShowOkButton` is `true`
- `IsSelectionImmediate` is `false` (default)

**Result:** `SelectionChanged` fires only when the OK button is tapped.

**When Selection is Immediate:**
- `IsSelectionImmediate` is `true`

**Result:** `SelectionChanged` fires immediately as the user scrolls.

```csharp
// Immediate selection - fires as user scrolls
timePicker.IsSelectionImmediate = true;

// Deferred selection - fires only on OK button click
timePicker.IsSelectionImmediate = false;
```

## Dialog Mode Events

Dialog mode provides three lifecycle events for tracking when the picker opens and closes.

### Opened Event

Raised when the picker dialog is opened.

**Event Signature:**
```csharp
public event EventHandler Opened;
```

**XAML:**
```xml
<picker:SfTimePicker x:Name="timePicker"
                     Mode="Dialog"
                     Opened="OnTimePickerPopUpOpened"
                     Format="hh_mm_tt">
</picker:SfTimePicker>
```

**C#:**
```csharp
timePicker.Opened += OnTimePickerPopUpOpened;

private void OnTimePickerPopUpOpened(object sender, EventArgs e)
{
    Console.WriteLine("Picker opened");
    
    // Example: Disable other UI elements
    submitButton.IsEnabled = false;
    
    // Example: Track analytics
    Analytics.TrackEvent("TimePicker_Opened");
}
```

### Closing Event

Raised when the picker dialog is about to close. Can be cancelled.

**Event Signature:**
```csharp
public event EventHandler<CancelEventArgs> Closing;
```

**EventArgs:**
```csharp
public class CancelEventArgs : EventArgs
{
    public bool Cancel { get; set; }
}
```

**XAML:**
```xml
<picker:SfTimePicker x:Name="timePicker"
                     Mode="Dialog"
                     Closing="OnTimePickerPopUpClosing"
                     Format="hh_mm_tt">
</picker:SfTimePicker>
```

**C#:**
```csharp
timePicker.Closing += OnTimePickerPopUpClosing;

private void OnTimePickerPopUpClosing(object sender, CancelEventArgs e)
{
    // Example: Prevent closing if no time is selected
    if (!timePicker.SelectedTime.HasValue)
    {
        DisplayAlert("Required", "Please select a time before closing", "OK");
        e.Cancel = true; // Prevent the picker from closing
    }
}
```

### Closed Event

Raised after the picker dialog has closed.

**Event Signature:**
```csharp
public event EventHandler Closed;
```

**XAML:**
```xml
<picker:SfTimePicker x:Name="timePicker"
                     Mode="Dialog"
                     Closed="OnTimePickerPopUpClosed"
                     Format="hh_mm_tt">
</picker:SfTimePicker>
```

**C#:**
```csharp
timePicker.Closed += OnTimePickerPopUpClosed;

private void OnTimePickerPopUpClosed(object sender, EventArgs e)
{
    Console.WriteLine("Picker closed");
    
    // Example: Re-enable other UI elements
    submitButton.IsEnabled = true;
    
    // Example: Validate selection
    ValidateTimeSelection();
}
```

### Dialog Events Complete Example

```xml
<StackLayout Padding="20" Spacing="15">
    
    <Label x:Name="statusLabel" Text="Picker is closed" FontSize="16" />
    
    <Button Text="Open Time Picker" 
            x:Name="openButton"
            Clicked="OnOpenPickerClicked" />
    
    <picker:SfTimePicker x:Name="dialogPicker"
                         Mode="Dialog"
                         Format="hh_mm_tt"
                         Opened="OnPickerOpened"
                         Closing="OnPickerClosing"
                         Closed="OnPickerClosed"
                         SelectionChanged="OnTimeSelected">
        <picker:SfTimePicker.HeaderView>
            <picker:PickerHeaderView Text="Select Time" Height="40" />
        </picker:SfTimePicker.HeaderView>
        <picker:SfTimePicker.FooterView>
            <picker:PickerFooterView ShowOkButton="True" Height="40" />
        </picker:SfTimePicker.FooterView>
    </picker:SfTimePicker>
    
</StackLayout>
```

```csharp
private void OnOpenPickerClicked(object sender, EventArgs e)
{
    dialogPicker.IsOpen = true;
}

private void OnPickerOpened(object sender, EventArgs e)
{
    statusLabel.Text = "Picker is open";
    statusLabel.TextColor = Colors.Green;
    openButton.IsEnabled = false;
}

private void OnPickerClosing(object sender, CancelEventArgs e)
{
    // Validate before closing
    if (!dialogPicker.SelectedTime.HasValue)
    {
        DisplayAlert("Required", "Please select a time", "OK");
        e.Cancel = true;
    }
}

private void OnPickerClosed(object sender, EventArgs e)
{
    statusLabel.Text = "Picker is closed";
    statusLabel.TextColor = Colors.Red;
    openButton.IsEnabled = true;
}

private void OnTimeSelected(object sender, TimePickerSelectionChangedEventArgs e)
{
    if (e.NewValue.HasValue)
    {
        statusLabel.Text = $"Selected: {e.NewValue.Value.ToString(@"hh\:mm tt")}";
    }
}
```

## Footer View Events

Footer view events are triggered when the user interacts with OK or Cancel buttons.

**Note:** These events are only applicable when:
- Footer view is visible (`FooterView.Height` > 0)
- Buttons are enabled

### OkButtonClicked Event

Raised when the OK button in the footer is clicked.

**Event Signature:**
```csharp
public event EventHandler OkButtonClicked;
```

**XAML:**
```xml
<picker:SfTimePicker x:Name="timePicker"
                     OkButtonClicked="OnTimePickerOkButtonClicked"
                     Format="hh_mm_tt">
    <picker:SfTimePicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True" Height="40" />
    </picker:SfTimePicker.FooterView>
</picker:SfTimePicker>
```

**C#:**
```csharp
timePicker.OkButtonClicked += OnTimePickerOkButtonClicked;

private void OnTimePickerOkButtonClicked(object sender, EventArgs e)
{
    if (timePicker.SelectedTime.HasValue)
    {
        // Confirm the selection
        var confirmedTime = timePicker.SelectedTime.Value;
        
        // Save to database, update UI, etc.
        SaveAppointmentTime(confirmedTime);
        
        DisplayAlert("Confirmed", 
                     $"Time confirmed: {confirmedTime.ToString(@"hh\:mm tt")}", 
                     "OK");
    }
}
```

### CancelButtonClicked Event

Raised when the Cancel button in the footer is clicked.

**Event Signature:**
```csharp
public event EventHandler CancelButtonClicked;
```

**XAML:**
```xml
<picker:SfTimePicker x:Name="timePicker"
                     CancelButtonClicked="OnTimePickerCancelButtonClicked"
                     Format="hh_mm_tt">
    <picker:SfTimePicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True" Height="40" />
    </picker:SfTimePicker.FooterView>
</picker:SfTimePicker>
```

**C#:**
```csharp
timePicker.CancelButtonClicked += OnTimePickerCancelButtonClicked;

private void OnTimePickerCancelButtonClicked(object sender, EventArgs e)
{
    Console.WriteLine("User cancelled time selection");
    
    // Revert to previous value if needed
    timePicker.SelectedTime = previousTime;
    
    // Or clear selection
    // timePicker.SelectedTime = null;
    
    DisplayAlert("Cancelled", "Time selection cancelled", "OK");
}
```

### Footer Events Complete Example

```xml
<picker:SfTimePicker x:Name="appointmentPicker"
                     Mode="Dialog"
                     Format="hh_mm_tt"
                     OkButtonClicked="OnOkClicked"
                     CancelButtonClicked="OnCancelClicked"
                     SelectionChanged="OnTimeChanged">
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView Text="Appointment Time" Height="40" />
    </picker:SfTimePicker.HeaderView>
    <picker:SfTimePicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True" 
                                 Height="40"
                                 OkButtonText="Confirm"
                                 CancelButtonText="Reset" />
    </picker:SfTimePicker.FooterView>
</picker:SfTimePicker>
```

```csharp
private TimeSpan? originalTime;

private void OnOpenAppointmentPicker(object sender, EventArgs e)
{
    originalTime = appointmentPicker.SelectedTime;
    appointmentPicker.IsOpen = true;
}

private void OnOkClicked(object sender, EventArgs e)
{
    if (appointmentPicker.SelectedTime.HasValue)
    {
        // User confirmed the selection
        SaveToDatabase(appointmentPicker.SelectedTime.Value);
        ShowConfirmationMessage();
    }
}

private void OnCancelClicked(object sender, EventArgs e)
{
    // User cancelled - revert to original
    appointmentPicker.SelectedTime = originalTime;
    Console.WriteLine("Reverted to original time");
}

private void OnTimeChanged(object sender, TimePickerSelectionChangedEventArgs e)
{
    // Preview the change before confirmation
    if (e.NewValue.HasValue)
    {
        PreviewAppointmentTime(e.NewValue.Value);
    }
}
```

## Commands

Commands provide an MVVM-friendly way to handle picker interactions without code-behind.

### SelectionChangedCommand

Executes when the time selection changes.

**Property:**
```csharp
public ICommand SelectionChangedCommand { get; set; }
```

**Parameter:** `TimePickerSelectionChangedEventArgs`

**XAML:**
```xml
<picker:SfTimePicker x:Name="timePicker"
                     SelectionChangedCommand="{Binding SelectionChangedCommand}"
                     Format="hh_mm_tt">
    <picker:SfTimePicker.BindingContext>
        <local:TimePickerViewModel />
    </picker:SfTimePicker.BindingContext>
    <picker:SfTimePicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True" Height="40" />
    </picker:SfTimePicker.FooterView>
</picker:SfTimePicker>
```

**ViewModel:**
```csharp
public class TimePickerViewModel : INotifyPropertyChanged
{
    public ICommand SelectionChangedCommand { get; }
    
    public TimePickerViewModel()
    {
        SelectionChangedCommand = new Command<TimePickerSelectionChangedEventArgs>(OnSelectionChanged);
    }
    
    private void OnSelectionChanged(TimePickerSelectionChangedEventArgs args)
    {
        if (args.NewValue.HasValue)
        {
            var newTime = args.NewValue.Value;
            Console.WriteLine($"Time changed to: {newTime}");
            
            // Update model, validate, etc.
            UpdateAppointmentTime(newTime);
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
}
```

### AcceptCommand

Executes when the OK button is clicked.

**Property:**
```csharp
public ICommand AcceptCommand { get; set; }
```

**XAML:**
```xml
<picker:SfTimePicker x:Name="timePicker"
                     AcceptCommand="{Binding AcceptCommand}">
    <picker:SfTimePicker.BindingContext>
        <local:TimePickerViewModel />
    </picker:SfTimePicker.BindingContext>
    <picker:SfTimePicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True" Height="40" />
    </picker:SfTimePicker.FooterView>
</picker:SfTimePicker>
```

**ViewModel:**
```csharp
public class TimePickerViewModel : INotifyPropertyChanged
{
    public ICommand AcceptCommand { get; }
    
    public TimePickerViewModel()
    {
        AcceptCommand = new Command(OnAccept);
    }
    
    private void OnAccept()
    {
        Console.WriteLine("User confirmed time selection");
        
        // Save to database, close dialog, etc.
        SaveChanges();
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
}
```

### DeclineCommand

Executes when the Cancel button is clicked.

**Property:**
```csharp
public ICommand DeclineCommand { get; set; }
```

**XAML:**
```xml
<picker:SfTimePicker x:Name="timePicker"
                     DeclineCommand="{Binding DeclineCommand}">
    <picker:SfTimePicker.BindingContext>
        <local:TimePickerViewModel />
    </picker:SfTimePicker.BindingContext>
    <picker:SfTimePicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True" Height="40" />
    </picker:SfTimePicker.FooterView>
</picker:SfTimePicker>
```

**ViewModel:**
```csharp
public class TimePickerViewModel : INotifyPropertyChanged
{
    public ICommand DeclineCommand { get; }
    
    public TimePickerViewModel()
    {
        DeclineCommand = new Command(OnDecline);
    }
    
    private void OnDecline()
    {
        Console.WriteLine("User cancelled time selection");
        
        // Revert changes, close dialog, etc.
        RevertChanges();
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
}
```

### Complete MVVM Example

**XAML:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:picker="clr-namespace:Syncfusion.Maui.Picker;assembly=Syncfusion.Maui.Picker"
             xmlns:local="clr-namespace:MyApp.ViewModels"
             x:Class="MyApp.AppointmentPage">
    
    <ContentPage.BindingContext>
        <local:AppointmentViewModel />
    </ContentPage.BindingContext>
    
    <StackLayout Padding="20" Spacing="15">
        
        <Label Text="Appointment Scheduler" FontSize="24" FontAttributes="Bold" />
        
        <Label Text="{Binding StatusMessage}" FontSize="16" />
        
        <Button Text="Select Time" 
                Command="{Binding OpenPickerCommand}" />
        
        <picker:SfTimePicker SelectedTime="{Binding SelectedTime}"
                             SelectionChangedCommand="{Binding SelectionChangedCommand}"
                             AcceptCommand="{Binding ConfirmTimeCommand}"
                             DeclineCommand="{Binding CancelTimeCommand}"
                             Mode="Dialog"
                             Format="hh_mm_tt">
            <picker:SfTimePicker.HeaderView>
                <picker:PickerHeaderView Text="Select Appointment Time" Height="40" />
            </picker:SfTimePicker.HeaderView>
            <picker:SfTimePicker.FooterView>
                <picker:PickerFooterView ShowOkButton="True" Height="40" />
            </picker:SfTimePicker.FooterView>
        </picker:SfTimePicker>
        
    </StackLayout>
    
</ContentPage>
```

**ViewModel:**
```csharp
public class AppointmentViewModel : INotifyPropertyChanged
{
    private TimeSpan? _selectedTime;
    private string _statusMessage;
    
    public TimeSpan? SelectedTime
    {
        get => _selectedTime;
        set
        {
            _selectedTime = value;
            OnPropertyChanged();
        }
    }
    
    public string StatusMessage
    {
        get => _statusMessage;
        set
        {
            _statusMessage = value;
            OnPropertyChanged();
        }
    }
    
    public ICommand OpenPickerCommand { get; }
    public ICommand SelectionChangedCommand { get; }
    public ICommand ConfirmTimeCommand { get; }
    public ICommand CancelTimeCommand { get; }
    
    public AppointmentViewModel()
    {
        OpenPickerCommand = new Command(OpenPicker);
        SelectionChangedCommand = new Command<TimePickerSelectionChangedEventArgs>(OnSelectionChanged);
        ConfirmTimeCommand = new Command(OnConfirmTime);
        CancelTimeCommand = new Command(OnCancelTime);
        
        StatusMessage = "No time selected";
    }
    
    private void OpenPicker()
    {
        // Trigger picker opening logic if needed
        StatusMessage = "Please select a time";
    }
    
    private void OnSelectionChanged(TimePickerSelectionChangedEventArgs args)
    {
        if (args.NewValue.HasValue)
        {
            StatusMessage = $"Selected: {args.NewValue.Value.ToString(@"hh\:mm tt")}";
        }
    }
    
    private void OnConfirmTime()
    {
        if (SelectedTime.HasValue)
        {
            // Save to database or perform action
            SaveAppointment(SelectedTime.Value);
            StatusMessage = $"Appointment confirmed for {SelectedTime.Value.ToString(@"hh\:mm tt")}";
        }
    }
    
    private void OnCancelTime()
    {
        SelectedTime = null;
        StatusMessage = "Time selection cancelled";
    }
    
    private void SaveAppointment(TimeSpan time)
    {
        // Save logic here
        Console.WriteLine($"Saving appointment for {time}");
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

## IsSelectionImmediate Property

Controls when the `SelectedTime` property is updated and `SelectionChanged` event is fired.

**Property:**
```csharp
public bool IsSelectionImmediate { get; set; }
```

**Default:** `false`

**Behavior:**

**When `true`:**
- `SelectedTime` updates immediately as user scrolls
- `SelectionChanged` fires on every scroll change
- OK button click not required for confirmation

**When `false` (default):**
- `SelectedTime` updates only when OK button is clicked
- `SelectionChanged` fires only on OK button click
- User can scroll without committing changes

### Example: Immediate Selection

```xml
<picker:SfTimePicker x:Name="immediateP icker"
                     IsSelectionImmediate="True"
                     SelectionChanged="OnImmediateSelectionChanged"
                     Format="hh_mm_tt">
</picker:SfTimePicker>
```

```csharp
private void OnImmediateSelectionChanged(object sender, TimePickerSelectionChangedEventArgs e)
{
    // Fires immediately as user scrolls
    Console.WriteLine($"Immediate update: {e.NewValue}");
}
```

### Example: Deferred Selection

```xml
<picker:SfTimePicker x:Name="deferredPicker"
                     IsSelectionImmediate="False"
                     SelectionChanged="OnDeferredSelectionChanged"
                     Format="hh_mm_tt">
    <picker:SfTimePicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True" Height="40" />
    </picker:SfTimePicker.FooterView>
</picker:SfTimePicker>
```

```csharp
private void OnDeferredSelectionChanged(object sender, TimePickerSelectionChangedEventArgs e)
{
    // Fires only when OK button is clicked
    Console.WriteLine($"Confirmed selection: {e.NewValue}");
}
```

## Event Handling Patterns

### Pattern 1: Validation on Selection

```csharp
private void OnTimeSelectionChanged(object sender, TimePickerSelectionChangedEventArgs e)
{
    if (e.NewValue.HasValue)
    {
        if (!IsBusinessHours(e.NewValue.Value))
        {
            DisplayAlert("Invalid Time", 
                        "Please select a time during business hours (9 AM - 5 PM)", 
                        "OK");
            // Revert to previous value
            timePicker.SelectedTime = e.OldValue;
        }
    }
}

private bool IsBusinessHours(TimeSpan time)
{
    return time >= new TimeSpan(9, 0, 0) && time <= new TimeSpan(17, 0, 0);
}
```

### Pattern 2: Cascading Time Pickers

```csharp
private void OnStartTimeChanged(object sender, TimePickerSelectionChangedEventArgs e)
{
    if (e.NewValue.HasValue)
    {
        // Ensure end time is after start time
        endTimePicker.MinimumTime = e.NewValue.Value.Add(TimeSpan.FromMinutes(30));
        
        if (endTimePicker.SelectedTime.HasValue && 
            endTimePicker.SelectedTime.Value <= e.NewValue.Value)
        {
            endTimePicker.SelectedTime = e.NewValue.Value.Add(TimeSpan.FromHours(1));
        }
    }
}
```

### Pattern 3: Auto-Save on Selection

```csharp
private async void OnTimePickerOkButtonClicked(object sender, EventArgs e)
{
    if (timePicker.SelectedTime.HasValue)
    {
        // Show loading indicator
        loadingIndicator.IsVisible = true;
        
        try
        {
            await SaveToDatabase(timePicker.SelectedTime.Value);
            await DisplayAlert("Success", "Time saved successfully", "OK");
        }
        catch (Exception ex)
        {
            await DisplayAlert("Error", $"Failed to save: {ex.Message}", "OK");
        }
        finally
        {
            loadingIndicator.IsVisible = false;
        }
    }
}
```

## Best Practices

### 1. Always Check for Null Values

```csharp
private void OnTimeSelectionChanged(object sender, TimePickerSelectionChangedEventArgs e)
{
    if (e.NewValue.HasValue)
    {
        // Safe to use e.NewValue.Value
        var time = e.NewValue.Value;
    }
    else
    {
        // Handle null case
        Console.WriteLine("No time selected");
    }
}
```

### 2. Unsubscribe from Events

```csharp
protected override void OnDisappearing()
{
    base.OnDisappearing();
    
    // Prevent memory leaks
    timePicker.SelectionChanged -= OnTimeSelectionChanged;
    timePicker.OkButtonClicked -= OnOkClicked;
    timePicker.CancelButtonClicked -= OnCancelClicked;
}
```

### 3. Use Commands for MVVM

Prefer commands over events when using MVVM pattern:

```xml
<!-- Better for MVVM -->
<picker:SfTimePicker SelectionChangedCommand="{Binding TimeChangedCommand}" />

<!-- Avoid code-behind in MVVM -->
<!-- <picker:SfTimePicker SelectionChanged="OnTimeChanged" /> -->
```

### 4. Validate Before Saving

```csharp
private void OnOkButtonClicked(object sender, EventArgs e)
{
    if (!ValidateTimeSelection())
    {
        return;
    }
    
    SaveTimeSelection();
}

private bool ValidateTimeSelection()
{
    if (!timePicker.SelectedTime.HasValue)
    {
        DisplayAlert("Error", "Please select a time", "OK");
        return false;
    }
    
    // Additional validation logic
    return true;
}
```

### 5. Provide User Feedback

```csharp
private void OnTimeSelectionChanged(object sender, TimePickerSelectionChangedEventArgs e)
{
    if (e.NewValue.HasValue)
    {
        // Visual feedback
        confirmButton.IsEnabled = true;
        confirmButton.BackgroundColor = Colors.Green;
        
        // Update status label
        statusLabel.Text = $"Selected: {e.NewValue.Value.ToString(@"hh\:mm tt")}";
    }
}
```

## Summary

The TimePicker's event and command system provides comprehensive interaction tracking:

- **SelectionChanged** - Track time selection changes with NewValue/OldValue
- **Dialog Events** - Monitor Opened/Closing/Closed lifecycle
- **Footer Events** - Handle OkButtonClicked/CancelButtonClicked actions
- **Commands** - MVVM-friendly SelectionChangedCommand/AcceptCommand/DeclineCommand
- **IsSelectionImmediate** - Control immediate vs deferred selection confirmation
- **Validation** - Implement business logic in event handlers
- **MVVM Support** - Full command binding for clean architecture

Use events for simple scenarios and commands for MVVM patterns to create robust, user-friendly time selection workflows.
