# Events

This guide covers the events available in the DateTimePicker for handling user interactions and selection changes.

## Overview

The DateTimePicker provides several events to track user interactions:

**SelectionChanged Event:**
- Triggered when the selected date/time changes
- Available in all modes (Default, Dialog, RelativeDialog)

**Dialog Mode Events:**
- `Opened` - When dialog opens
- `Closing` - Before dialog closes (can cancel)
- `Closed` - After dialog closes

## SelectionChanged Event

The `SelectionChanged` event notifies you when the user changes the selected date and time.

### Event Arguments

**Type**: `DateTimePickerSelectionChangedEventArgs`

**Properties:**
- `NewValue` (DateTime?) - The newly selected date and time
- `OldValue` (DateTime?) - The previously selected date and time

### Basic Usage

**XAML:**
```xaml
<picker:SfDateTimePicker 
    x:Name="picker"
    SelectionChanged="OnDateTimePickerSelectionChanged" />
```

**C#:**
```csharp
private void OnDateTimePickerSelectionChanged(object sender, DateTimePickerSelectionChangedEventArgs e)
{
    var oldDateTime = e.OldValue;
    var newDateTime = e.NewValue;
    
    if (newDateTime.HasValue)
    {
        DisplayAlert("Selection Changed", 
            $"Selected: {newDateTime.Value:f}", 
            "OK");
    }
}
```

### Subscribe in Code

```csharp
dateTimePicker.SelectionChanged += OnDateTimePickerSelectionChanged;
```

### Unsubscribe from Event

```csharp
dateTimePicker.SelectionChanged -= OnDateTimePickerSelectionChanged;
```

### Complete Example

```xaml
<ContentPage xmlns:picker="clr-namespace:Syncfusion.Maui.Picker;assembly=Syncfusion.Maui.Picker">
    <StackLayout Padding="20">
        <Label Text="Select Date and Time:" />
        
        <picker:SfDateTimePicker 
            x:Name="picker"
            SelectionChanged="OnSelectionChanged" />
        
        <Label x:Name="resultLabel" Margin="0,20,0,0" />
    </StackLayout>
</ContentPage>
```

```csharp
private void OnSelectionChanged(object sender, DateTimePickerSelectionChangedEventArgs e)
{
    if (e.NewValue.HasValue)
    {
        resultLabel.Text = $"Selected: {e.NewValue.Value:f}";
    }
    else
    {
        resultLabel.Text = "No date selected";
    }
}
```

## Selection Behavior Modes

The DateTimePicker has two selection confirmation modes controlled by the `IsSelectionImmediate` property:

### Immediate Selection (IsSelectionImmediate = true)

Selection is updated immediately as the user scrolls through values:

```xaml
<picker:SfDateTimePicker 
    IsSelectionImmediate="True"
    SelectionChanged="OnSelectionChanged" />
```

**Behavior:**
- `SelectedDate` updates instantly on scroll
- `SelectionChanged` fires immediately
- No need to press OK button

### Confirmed Selection (IsSelectionImmediate = false, DEFAULT)

Selection is updated only when the user presses the OK button:

```xaml
<picker:SfDateTimePicker 
    Mode="Dialog"
    IsSelectionImmediate="False"
    SelectionChanged="OnSelectionChanged">
    <picker:SfDateTimePicker.FooterView>
        <picker:PickerFooterView 
            Height="50"
            ShowOkButton="True" />
    </picker:SfDateTimePicker.FooterView>
</picker:SfDateTimePicker>
```

**Behavior:**
- `SelectedDate` updates only when OK is pressed
- `SelectionChanged` fires when OK is pressed
- User can cancel without changing selection

**Requirements for Confirmed Selection:**
1. `Mode` must be `Dialog` or `RelativeDialog`
2. Footer `Height` must be > 0
3. `ShowOkButton` must be `True`

## Dialog Mode Events

When using `Mode="Dialog"` or `Mode="RelativeDialog"`, three additional events are available.

### Opened Event

Fired when the picker dialog is opened.

**XAML:**
```xaml
<picker:SfDateTimePicker 
    Mode="Dialog"
    Opened="OnDateTimePickerOpened" />
```

**C#:**
```csharp
private void OnDateTimePickerOpened(object sender, EventArgs e)
{
    // Dialog has opened
    Console.WriteLine("DateTimePicker dialog opened");
}
```

**Use Cases:**
- Log analytics
- Initialize picker state
- Show help text
- Disable other UI elements

### Closing Event

Fired before the picker dialog closes. You can cancel the closing operation.

**Event Arguments**: `CancelEventArgs`

**XAML:**
```xaml
<picker:SfDateTimePicker 
    Mode="Dialog"
    Closing="OnDateTimePickerClosing" />
```

**C#:**
```csharp
private void OnDateTimePickerClosing(object sender, CancelEventArgs e)
{
    // Check if selection is valid
    if (picker.SelectedDate.HasValue)
    {
        var selected = picker.SelectedDate.Value;
        
        // Prevent selection of weekends
        if (selected.DayOfWeek == DayOfWeek.Saturday || 
            selected.DayOfWeek == DayOfWeek.Sunday)
        {
            e.Cancel = true; // Prevent closing
            DisplayAlert("Invalid Selection", 
                "Please select a weekday", 
                "OK");
        }
    }
}
```

**Use Cases:**
- Validate selection before accepting
- Show confirmation dialogs
- Enforce business rules
- Prevent closing on invalid selection

### Closed Event

Fired after the picker dialog has closed.

**XAML:**
```xaml
<picker:SfDateTimePicker 
    Mode="Dialog"
    Closed="OnDateTimePickerClosed" />
```

**C#:**
```csharp
private void OnDateTimePickerClosed(object sender, EventArgs e)
{
    // Dialog has closed
    Console.WriteLine("DateTimePicker dialog closed");
    
    // Re-enable other UI elements
    submitButton.IsEnabled = true;
}
```

**Use Cases:**
- Log analytics
- Update UI state
- Re-enable controls
- Trigger dependent actions

## Complete Dialog Event Example

```xaml
<ContentPage xmlns:picker="clr-namespace:Syncfusion.Maui.Picker;assembly=Syncfusion.Maui.Picker">
    <StackLayout Padding="20">
        <Button 
            Text="Select Date & Time"
            Clicked="OnShowPicker" />
        
        <picker:SfDateTimePicker 
            x:Name="picker"
            Mode="Dialog"
            SelectionChanged="OnSelectionChanged"
            Opened="OnPickerOpened"
            Closing="OnPickerClosing"
            Closed="OnPickerClosed">
            <picker:SfDateTimePicker.FooterView>
                <picker:PickerFooterView 
                    Height="50"
                    ShowOkButton="True" />
            </picker:SfDateTimePicker.FooterView>
        </picker:SfDateTimePicker>
        
        <Label x:Name="statusLabel" Margin="0,20,0,0" />
    </StackLayout>
</ContentPage>
```

```csharp
private void OnShowPicker(object sender, EventArgs e)
{
    picker.IsOpen = true;
}

private void OnPickerOpened(object sender, EventArgs e)
{
    statusLabel.Text = "Picker opened";
}

private void OnPickerClosing(object sender, CancelEventArgs e)
{
    // Validate selection
    if (picker.SelectedDate.HasValue)
    {
        var date = picker.SelectedDate.Value;
        
        // Example: Don't allow past dates
        if (date < DateTime.Now)
        {
            e.Cancel = true;
            DisplayAlert("Invalid Date", "Please select a future date", "OK");
        }
    }
}

private void OnPickerClosed(object sender, EventArgs e)
{
    statusLabel.Text = "Picker closed";
}

private void OnSelectionChanged(object sender, DateTimePickerSelectionChangedEventArgs e)
{
    if (e.NewValue.HasValue)
    {
        statusLabel.Text = $"Selected: {e.NewValue.Value:f}";
    }
}
```

## Data Binding with Events

### Binding to ViewModel

**ViewModel:**
```csharp
public class AppointmentViewModel : INotifyPropertyChanged
{
    private DateTime? _appointmentDateTime;
    
    public DateTime? AppointmentDateTime
    {
        get => _appointmentDateTime;
        set
        {
            _appointmentDateTime = value;
            OnPropertyChanged();
            OnPropertyChanged(nameof(IsAppointmentSelected));
        }
    }
    
    public bool IsAppointmentSelected => AppointmentDateTime.HasValue;
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**XAML:**
```xaml
<StackLayout BindingContext="{Binding AppointmentVM}">
    <picker:SfDateTimePicker 
        SelectedDate="{Binding AppointmentDateTime, Mode=TwoWay}"
        SelectionChanged="OnSelectionChanged" />
    
    <Button 
        Text="Confirm Appointment"
        IsEnabled="{Binding IsAppointmentSelected}" />
</StackLayout>
```

## Event Handling Patterns

### Pattern 1: Update Multiple UI Elements

```csharp
private void OnSelectionChanged(object sender, DateTimePickerSelectionChangedEventArgs e)
{
    if (e.NewValue.HasValue)
    {
        var dateTime = e.NewValue.Value;
        
        dateLabel.Text = dateTime.ToShortDateString();
        timeLabel.Text = dateTime.ToShortTimeString();
        dayOfWeekLabel.Text = dateTime.DayOfWeek.ToString();
        confirmButton.IsEnabled = true;
    }
}
```

### Pattern 2: Trigger Validation

```csharp
private void OnSelectionChanged(object sender, DateTimePickerSelectionChangedEventArgs e)
{
    ValidateDateTime(e.NewValue);
}

private void ValidateDateTime(DateTime? dateTime)
{
    if (!dateTime.HasValue)
    {
        errorLabel.Text = "Please select a date and time";
        errorLabel.IsVisible = true;
        return;
    }
    
    if (dateTime.Value < DateTime.Now)
    {
        errorLabel.Text = "Cannot select past date/time";
        errorLabel.IsVisible = true;
    }
    else
    {
        errorLabel.IsVisible = false;
    }
}
```

### Pattern 3: Call API or Save Data

```csharp
private async void OnSelectionChanged(object sender, DateTimePickerSelectionChangedEventArgs e)
{
    if (e.NewValue.HasValue)
    {
        await SaveAppointmentAsync(e.NewValue.Value);
    }
}

private async Task SaveAppointmentAsync(DateTime dateTime)
{
    try
    {
        await appointmentService.SaveAsync(dateTime);
        await DisplayAlert("Success", "Appointment saved", "OK");
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", ex.Message, "OK");
    }
}
```

### Pattern 4: Compare Old and New Values

```csharp
private void OnSelectionChanged(object sender, DateTimePickerSelectionChangedEventArgs e)
{
    if (e.OldValue.HasValue && e.NewValue.HasValue)
    {
        var difference = e.NewValue.Value - e.OldValue.Value;
        
        if (Math.Abs(difference.TotalDays) > 30)
        {
            DisplayAlert("Large Change", 
                $"Date changed by {difference.TotalDays:F0} days", 
                "OK");
        }
    }
}
```

### Pattern 5: Conditional Actions

```csharp
private void OnSelectionChanged(object sender, DateTimePickerSelectionChangedEventArgs e)
{
    if (e.NewValue.HasValue)
    {
        var dateTime = e.NewValue.Value;
        
        // Weekend warning
        if (dateTime.DayOfWeek == DayOfWeek.Saturday || 
            dateTime.DayOfWeek == DayOfWeek.Sunday)
        {
            warningLabel.Text = "⚠️ Weekend selected";
            warningLabel.IsVisible = true;
        }
        else
        {
            warningLabel.IsVisible = false;
        }
    }
}
```

## Best Practices

1. **Check for Null**: Always check if `NewValue` or `OldValue` is not null
2. **Async Operations**: Use async/await for API calls or database operations
3. **Error Handling**: Wrap event handlers in try-catch blocks
4. **Performance**: Avoid heavy operations in SelectionChanged (immediate mode)
5. **Unsubscribe**: Remove event handlers when disposing to prevent memory leaks
6. **User Feedback**: Show loading indicators for async operations
7. **Validation**: Use Closing event to validate before accepting selection

## Important Notes

1. **IsSelectionImmediate = false**: `SelectedDate` is confirmed only when OK is pressed (requires Dialog/RelativeDialog mode with footer)
2. **IsSelectionImmediate = true**: `SelectedDate` updates immediately as user scrolls
3. **Closing Event**: Can be canceled by setting `e.Cancel = true`
4. **Dialog-Only Events**: Opened, Closing, Closed only work with Dialog or Relative Dialog modes
5. **Event Order**: Opened → (user selects) → Closing → SelectionChanged → Closed
