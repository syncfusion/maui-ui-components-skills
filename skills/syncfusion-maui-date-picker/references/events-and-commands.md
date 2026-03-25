# Events and Commands in .NET MAUI DatePicker

The .NET MAUI DatePicker provides comprehensive event handling and command support for responding to user interactions and date selection changes.

## Table of Contents
- [SelectionChanged Event](#selectionchanged-event)
- [Dialog Mode Events](#dialog-mode-events)
- [Footer Button Events](#footer-button-events)
- [Commands](#commands)
- [Event Handling Patterns](#event-handling-patterns)

## SelectionChanged Event

The `SelectionChanged` event fires when the selected date changes. This is the primary event for tracking date selection.

### Event Arguments

The event uses `DatePickerSelectionChangedEventArgs` which provides:
- **NewValue** - The newly selected date (DateTime?)
- **OldValue** - The previously selected date (DateTime?)

### XAML

```xml
<picker:SfDatePicker x:Name="datePicker"
                     SelectionChanged="OnDatePickerSelectionChanged">
</picker:SfDatePicker>
```

### C#

```csharp
this.datePicker.SelectionChanged += OnDatePickerSelectionChanged;

private void OnDatePickerSelectionChanged(object sender, DatePickerSelectionChangedEventArgs e)
{
    var oldDate = e.OldValue;
    var newDate = e.NewValue;
    
    if (newDate != null)
    {
        Console.WriteLine($"Date changed from {oldDate:d} to {newDate:d}");
    }
}
```

### Selection Confirmation Behavior

**Important Notes:**
- In `Dialog` or `RelativeDialog` modes with `ShowOkButton="True"`, the SelectedDate is confirmed only when the OK button is tapped
- When `IsSelectionImmediate="True"`, the SelectedDate updates immediately upon selection
- When `IsSelectionImmediate="False"` (default), the SelectedDate is confirmed only when OK is tapped

### Example: Track Selection Changes

```csharp
private void OnDatePickerSelectionChanged(object sender, DatePickerSelectionChangedEventArgs e)
{
    if (e.NewValue != null)
    {
        // Display selected date
        selectedDateLabel.Text = $"Selected: {e.NewValue:dddd, MMMM dd, yyyy}";
        
        // Calculate days from today
        TimeSpan diff = e.NewValue.Value - DateTime.Now.Date;
        daysLabel.Text = $"{diff.Days} days from today";
        
        // Enable/disable submit button based on selection
        submitButton.IsEnabled = true;
    }
    else
    {
        selectedDateLabel.Text = "No date selected";
        submitButton.IsEnabled = false;
    }
}
```

## Dialog Mode Events

Dialog and RelativeDialog modes provide three lifecycle events for managing the picker dialog.

### Opened Event

Fires when the picker dialog is opened.

#### XAML

```xml
<picker:SfDatePicker x:Name="datePicker"
                     Mode="Dialog"
                     Opened="OnDatePickerOpened">
</picker:SfDatePicker>
```

#### C#

```csharp
this.datePicker.Opened += OnDatePickerOpened;

private void OnDatePickerOpened(object sender, EventArgs e)
{
    Console.WriteLine("Date picker opened");
    // Perform actions when dialog opens
}
```

#### Example: Track Dialog Opens

```csharp
private void OnDatePickerOpened(object sender, EventArgs e)
{
    // Log analytics
    Analytics.TrackEvent("DatePickerOpened");
    
    // Set focus or perform initialization
    InitializePickerState();
}
```

### Closing Event

Fires when the picker dialog is closing. This event can be cancelled to prevent the dialog from closing.

#### XAML

```xml
<picker:SfDatePicker x:Name="datePicker"
                     Mode="Dialog"
                     Closing="OnDatePickerClosing">
</picker:SfDatePicker>
```

#### C#

```csharp
this.datePicker.Closing += OnDatePickerClosing;

private void OnDatePickerClosing(object sender, CancelEventArgs e)
{
    // Cancel the close operation if needed
    if (datePicker.SelectedDate == null)
    {
        e.Cancel = true;
        DisplayAlert("Required", "Please select a date", "OK");
    }
}
```

#### Example: Validate Before Closing

```csharp
private void OnDatePickerClosing(object sender, CancelEventArgs e)
{
    // Prevent closing if no date is selected
    if (datePicker.SelectedDate == null && isDateRequired)
    {
        e.Cancel = true;
        DisplayAlert("Validation", "Please select a date before closing", "OK");
        return;
    }
    
    // Prevent closing if selected date is in the past
    if (datePicker.SelectedDate < DateTime.Now.Date)
    {
        e.Cancel = true;
        DisplayAlert("Invalid Date", "Please select a future date", "OK");
    }
}
```

### Closed Event

Fires when the picker dialog is closed.

#### XAML

```xml
<picker:SfDatePicker x:Name="datePicker"
                     Mode="Dialog"
                     Closed="OnDatePickerClosed">
</picker:SfDatePicker>
```

#### C#

```csharp
this.datePicker.Closed += OnDatePickerClosed;

private void OnDatePickerClosed(object sender, EventArgs e)
{
    Console.WriteLine("Date picker closed");
    // Perform cleanup or post-close actions
}
```

#### Example: Save State After Closing

```csharp
private void OnDatePickerClosed(object sender, EventArgs e)
{
    // Save selected date to preferences
    if (datePicker.SelectedDate != null)
    {
        Preferences.Set("LastSelectedDate", datePicker.SelectedDate.Value.ToString("O"));
    }
    
    // Update UI
    UpdateRelatedFields();
}
```

## Footer Button Events

The footer view provides events for OK and Cancel button clicks.

### OkButtonClicked Event

Fires when the OK button is clicked. This event is not applicable when the footer view is not visible or the OK button is not shown.

#### XAML

```xml
<picker:SfDatePicker x:Name="datePicker"
                     OkButtonClicked="OnDatePickerOkButtonClicked">
    <picker:SfDatePicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True" Height="40" />
    </picker:SfDatePicker.FooterView>
</picker:SfDatePicker>
```

#### C#

```csharp
this.datePicker.OkButtonClicked += OnDatePickerOkButtonClicked;

private void OnDatePickerOkButtonClicked(object sender, EventArgs e)
{
    Console.WriteLine("OK button clicked - Date confirmed");
    // Process the confirmed date
}
```

#### Example: Process Confirmed Date

```csharp
private void OnDatePickerOkButtonClicked(object sender, EventArgs e)
{
    if (datePicker.SelectedDate != null)
    {
        // Save to database
        SaveAppointmentDate(datePicker.SelectedDate.Value);
        
        // Navigate to next screen
        Navigation.PushAsync(new AppointmentDetailsPage(datePicker.SelectedDate.Value));
        
        // Display confirmation
        DisplayAlert("Success", $"Date set to {datePicker.SelectedDate:d}", "OK");
    }
}
```

### CancelButtonClicked Event

Fires when the Cancel button is clicked. This event is not applicable when the footer view is not visible.

#### XAML

```xml
<picker:SfDatePicker x:Name="datePicker"
                     CancelButtonClicked="OnDatePickerCancelButtonClicked">
    <picker:SfDatePicker.FooterView>
        <picker:PickerFooterView Height="40" />
    </picker:SfDatePicker.FooterView>
</picker:SfDatePicker>
```

#### C#

```csharp
this.datePicker.CancelButtonClicked += OnDatePickerCancelButtonClicked;

private void OnDatePickerCancelButtonClicked(object sender, EventArgs e)
{
    Console.WriteLine("Cancel button clicked - Selection cancelled");
    // Revert or handle cancellation
}
```

#### Example: Revert to Previous Date

```csharp
private DateTime? previousDate;

private void OnDatePickerOpened(object sender, EventArgs e)
{
    // Store the current date when dialog opens
    previousDate = datePicker.SelectedDate;
}

private void OnDatePickerCancelButtonClicked(object sender, EventArgs e)
{
    // Revert to the previous date
    datePicker.SelectedDate = previousDate;
    
    // Display message
    DisplayAlert("Cancelled", "Date selection cancelled", "OK");
}
```

## Commands

The DatePicker supports MVVM pattern through commands that correspond to the events.

### SelectionChangedCommand

Command invoked when the selection changes. Passes `DatePickerSelectionChangedEventArgs` as the command parameter.

#### XAML

```xml
<picker:SfDatePicker x:Name="datePicker"
                     SelectionChangedCommand="{Binding SelectionChangedCommand}">
<ContentPage.BindingContext>
    <local:DatePickerViewModel/>
</ContentPage.BindingContext>
</picker:SfDatePicker>
```

#### ViewModel

```csharp
public class DatePickerViewModel : INotifyPropertyChanged
{
    public ICommand SelectionChangedCommand { get; set; }
    
    public DatePickerViewModel()
    {
        SelectionChangedCommand = new Command<DatePickerSelectionChangedEventArgs>(OnSelectionChanged);
    }
    
    private void OnSelectionChanged(DatePickerSelectionChangedEventArgs args)
    {
        if (args.NewValue != null)
        {
            Console.WriteLine($"Date selected: {args.NewValue:d}");
            // Update ViewModel properties
            SelectedDate = args.NewValue.Value;
        }
    }
    
    private DateTime _selectedDate;
    public DateTime SelectedDate
    {
        get => _selectedDate;
        set
        {
            _selectedDate = value;
            OnPropertyChanged(nameof(SelectedDate));
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### AcceptCommand

Command invoked when the OK button is clicked.

#### XAML

```xml
<picker:SfDatePicker x:Name="datePicker"
                     AcceptCommand="{Binding AcceptCommand}">
    <picker:SfDatePicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True" Height="40" />
    </picker:SfDatePicker.FooterView>
<ContentPage.BindingContext>
    <local:DatePickerViewModel/>
</ContentPage.BindingContext>
</picker:SfDatePicker>
```

#### ViewModel

```csharp
public class DatePickerViewModel
{
    public ICommand AcceptCommand { get; set; }
    
    public DatePickerViewModel()
    {
        AcceptCommand = new Command(OnAccept);
    }
    
    private void OnAccept()
    {
        Console.WriteLine("Date confirmed");
        // Perform action when date is accepted
        SaveDate();
    }
    
    private void SaveDate()
    {
        // Implementation
    }
}
```

### DeclineCommand

Command invoked when the Cancel button is clicked.

#### XAML

```xml
<picker:SfDatePicker x:Name="datePicker"
                     DeclineCommand="{Binding DeclineCommand}">
    <picker:SfDatePicker.FooterView>
        <picker:PickerFooterView Height="40" />
    </picker:SfDatePicker.FooterView>
<ContentPage.BindingContext>
    <local:DatePickerViewModel/>
</ContentPage.BindingContext>
</picker:SfDatePicker>
```

#### ViewModel

```csharp
public class DatePickerViewModel
{
    public ICommand DeclineCommand { get; set; }
    
    public DatePickerViewModel()
    {
        DeclineCommand = new Command(OnDecline);
    }
    
    private void OnDecline()
    {
        Console.WriteLine("Date selection cancelled");
        // Handle cancellation
    }
}
```

## Event Handling Patterns

### Pattern 1: Validate and Process Selection

```csharp
public class AppointmentPage : ContentPage
{
    private bool isProcessing = false;
    
    private void OnDatePickerSelectionChanged(object sender, DatePickerSelectionChangedEventArgs e)
    {
        if (isProcessing || e.NewValue == null) return;
        
        isProcessing = true;
        
        try
        {
            // Validate the selected date
            if (IsValidAppointmentDate(e.NewValue.Value))
            {
                // Check availability
                if (CheckAvailability(e.NewValue.Value))
                {
                    // Save and proceed
                    SaveAppointment(e.NewValue.Value);
                    DisplayAlert("Success", "Appointment scheduled", "OK");
                }
                else
                {
                    DisplayAlert("Unavailable", "This date is fully booked", "OK");
                    datePicker.SelectedDate = null;
                }
            }
            else
            {
                DisplayAlert("Invalid", "Please select a valid date", "OK");
                datePicker.SelectedDate = null;
            }
        }
        finally
        {
            isProcessing = false;
        }
    }
    
    private bool IsValidAppointmentDate(DateTime date)
    {
        // Business logic for validation
        return date >= DateTime.Now.Date && date.DayOfWeek != DayOfWeek.Sunday;
    }
    
    private bool CheckAvailability(DateTime date)
    {
        // Check if date is available
        return true; // Placeholder
    }
    
    private void SaveAppointment(DateTime date)
    {
        // Save to database
    }
}
```

### Pattern 2: Dialog Lifecycle Management

```csharp
public class DialogManagedPage : ContentPage
{
    private DateTime? tempSelectedDate;
    
    private void OnDatePickerOpened(object sender, EventArgs e)
    {
        // Save current state
        tempSelectedDate = datePicker.SelectedDate;
        
        // Initialize dialog
        Console.WriteLine("Dialog opened");
    }
    
    private void OnDatePickerClosing(object sender, CancelEventArgs e)
    {
        // Validate before allowing close
        if (requiresDate && datePicker.SelectedDate == null)
        {
            e.Cancel = true;
            DisplayAlert("Required", "Please select a date", "OK");
        }
    }
    
    private void OnDatePickerClosed(object sender, EventArgs e)
    {
        // Clean up or save state
        Console.WriteLine("Dialog closed");
        SavePreferences();
    }
    
    private void OnOkButtonClicked(object sender, EventArgs e)
    {
        // Confirm selection
        CommitDateSelection();
    }
    
    private void OnCancelButtonClicked(object sender, EventArgs e)
    {
        // Revert to previous state
        datePicker.SelectedDate = tempSelectedDate;
    }
}
```

### Pattern 3: MVVM with Full Command Support

```csharp
public class AppointmentViewModel : INotifyPropertyChanged
{
    private DateTime? _selectedDate;
    private string _statusMessage;
    
    public DateTime? SelectedDate
    {
        get => _selectedDate;
        set
        {
            _selectedDate = value;
            OnPropertyChanged(nameof(SelectedDate));
        }
    }
    
    public string StatusMessage
    {
        get => _statusMessage;
        set
        {
            _statusMessage = value;
            OnPropertyChanged(nameof(StatusMessage));
        }
    }
    
    public ICommand SelectionChangedCommand { get; }
    public ICommand AcceptCommand { get; }
    public ICommand DeclineCommand { get; }
    
    public AppointmentViewModel()
    {
        SelectionChangedCommand = new Command<DatePickerSelectionChangedEventArgs>(OnSelectionChanged);
        AcceptCommand = new Command(OnAccept);
        DeclineCommand = new Command(OnDecline);
    }
    
    private void OnSelectionChanged(DatePickerSelectionChangedEventArgs args)
    {
        if (args.NewValue != null)
        {
            SelectedDate = args.NewValue.Value;
            StatusMessage = $"Selected: {SelectedDate:dddd, MMMM dd, yyyy}";
        }
    }
    
    private async void OnAccept()
    {
        if (SelectedDate != null)
        {
            // Save to service
            await appointmentService.SaveAppointmentAsync(SelectedDate.Value);
            StatusMessage = "Appointment confirmed!";
        }
    }
    
    private void OnDecline()
    {
        SelectedDate = null;
        StatusMessage = "Selection cancelled";
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

## Best Practices

1. **Avoid Infinite Loops:** Be careful not to update SelectedDate within the SelectionChanged event handler if it causes the event to fire again

2. **Use Closing for Validation:** Use the Closing event with `e.Cancel = true` to prevent dialog close on invalid selections

3. **Prefer Commands for MVVM:** Use commands instead of events when following MVVM pattern

4. **Handle Null Values:** Always check for null SelectedDate values before processing

5. **Debounce Rapid Changes:** If processing is expensive, consider debouncing rapid selection changes

6. **Clean Event Handlers:** Always unsubscribe from events when disposing components to prevent memory leaks

## Related Topics

- **Date Restrictions** - Combine event handling with date restrictions for validation
- **Picker Modes** - Dialog events only work in Dialog and RelativeDialog modes
- **Customization** - Handle events on customized pickers
