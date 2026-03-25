# Validation and Events in .NET MAUI Masked Entry

The Masked Entry control provides comprehensive validation capabilities and a rich event system for monitoring and controlling user input.

## Table of Contents
- [Validation](#validation)
- [Focus Events and Methods](#focus-events-and-methods)
- [Value Events](#value-events)
- [Completion Events](#completion-events)
- [Event Lifecycle](#event-lifecycle)
- [Common Validation Patterns](#common-validation-patterns)

## Validation

### ValidationMode Property

The `ValidationMode` property determines when validation occurs:

```csharp
public enum InputValidationMode
{
    KeyPress,   // Validate on each key press (default)
    LostFocus   // Validate when control loses focus
}
```

#### KeyPress Validation

**When:** Validation happens immediately as user types  
**Use for:** Real-time feedback, preventing invalid characters

```xml
<editors:SfMaskedEntry 
    x:Name="phoneEntry"
    MaskType="Simple"
    Mask="(000) 000-0000"
    ValidationMode="KeyPress" />
```

```csharp
var phoneEntry = new SfMaskedEntry
{
    MaskType = MaskedEntryMaskType.Simple,
    Mask = "(000) 000-0000",
    ValidationMode = InputValidationMode.KeyPress
};
```

**Behavior:** Invalid characters are rejected immediately; user cannot type them.

#### LostFocus Validation

**When:** Validation happens when control loses focus  
**Use for:** Less intrusive validation, validating complete input

```xml
<editors:SfMaskedEntry 
    x:Name="dateEntry"
    MaskType="Simple"
    Mask="00/00/0000"
    ValidationMode="LostFocus" />
```

```csharp
var dateEntry = new SfMaskedEntry
{
    MaskType = MaskedEntryMaskType.Simple,
    Mask = "00/00/0000",
    ValidationMode = InputValidationMode.LostFocus
};
```

**Behavior:** User can type freely; validation occurs when they move to next field.

### HasError Property

**Type:** `bool` (read-only)  
**Purpose:** Indicates whether current input fails validation

```csharp
if (maskedEntry.HasError)
{
    // Input is invalid
    await DisplayAlert("Error", "Please enter valid information", "OK");
}
```

**Example: Validate on Lost Focus**

```csharp
var maskedEntry = new SfMaskedEntry
{
    MaskType = MaskedEntryMaskType.Simple,
    Mask = "00/00/0000",
    ValidationMode = InputValidationMode.LostFocus
};

maskedEntry.ValueChanged += (s, e) =>
{
    var entry = s as SfMaskedEntry;
    if (entry.HasError)
    {
        errorLabel.Text = "Please enter a valid date";
        errorLabel.IsVisible = true;
        entry.Stroke = Colors.Red;
    }
    else
    {
        errorLabel.IsVisible = false;
        entry.Stroke = Colors.Green;
    }
};
```

## Focus Events and Methods

### Focused Event

**When:** Control receives focus (user taps/tabs into it)  
**Event Args:** `FocusEventArgs`

```xml
<editors:SfMaskedEntry 
    x:Name="maskedEntry"
    Focused="OnMaskedEntryFocused" />
```

```csharp
private void OnMaskedEntryFocused(object sender, FocusEventArgs e)
{
    // Control gained focus
    var entry = sender as SfMaskedEntry;
    entry.Stroke = Colors.Blue; // Highlight border
}
```

**Use cases:**
- Highlight the control visually
- Show input hints or help text
- Track navigation flow
- Trigger analytics

### Unfocused Event

**When:** Control loses focus (user taps/tabs away)  
**Event Args:** `FocusEventArgs`

```xml
<editors:SfMaskedEntry 
    x:Name="maskedEntry"
    Unfocused="OnMaskedEntryUnfocused" />
```

```csharp
private void OnMaskedEntryUnfocused(object sender, FocusEventArgs e)
{
    // Control lost focus
    var entry = sender as SfMaskedEntry;
    
    // Validate when leaving field
    if (entry.HasError || !ValidateInput(entry.Value))
    {
        errorLabel.Text = "Invalid input";
        errorLabel.IsVisible = true;
    }
}
```

**Use cases:**
- Perform validation
- Save data automatically
- Hide hints/help text
- Reset visual highlighting

### Focus() Method

Programmatically set focus to the control:

```csharp
// Set focus to phone entry when page loads
protected override void OnAppearing()
{
    base.OnAppearing();
    phoneEntry.Focus();
}
```

### Unfocus() Method

Programmatically remove focus from the control:

```csharp
private void OnSubmitClicked(object sender, EventArgs e)
{
    // Remove focus to trigger validation
    maskedEntry.Unfocus();
    
    if (!maskedEntry.HasError)
    {
        ProcessInput();
    }
}
```

### Complete Focus Management Example

```csharp
public partial class FormPage : ContentPage
{
    public FormPage()
    {
        InitializeComponent();
        
        phoneEntry.Focused += OnEntryFocused;
        phoneEntry.Unfocused += OnEntryUnfocused;
        emailEntry.Focused += OnEntryFocused;
        emailEntry.Unfocused += OnEntryUnfocused;
    }
    
    private void OnEntryFocused(object sender, FocusEventArgs e)
    {
        var entry = sender as SfMaskedEntry;
        entry.Stroke = Colors.Blue;
        entry.StrokeThickness = 2;
    }
    
    private void OnEntryUnfocused(object sender, FocusEventArgs e)
    {
        var entry = sender as SfMaskedEntry;
        
        if (entry.HasError)
        {
            entry.Stroke = Colors.Red;
        }
        else
        {
            entry.Stroke = Colors.Green;
        }
        
        entry.StrokeThickness = 1;
    }
}
```

## Value Events

### ValueChanging Event

**When:** Before the `Value` property changes (cancelable)  
**Event Args:** `MaskedEntryValueChangingEventArgs`

**Properties:**
- `NewValue` (object): The proposed new value
- `OldValue` (object): The current value
- `IsValid` (bool): Whether input matches mask pattern
- `Cancel` (bool): Set to true to prevent the change

**Use this to:**
- Preview changes before they apply
- Cancel invalid input
- Implement custom validation
- Log changes

**Example: Prevent Specific Values**

```csharp
maskedEntry.ValueChanging += (s, e) =>
{
    string newValue = e.NewValue?.ToString();
    
    // Don't allow "1234" as input
    if (newValue == "1234")
    {
        e.Cancel = true;
        await DisplayAlert("Invalid", "Cannot use 1234", "OK");
    }
};
```

**Example: Track Valid vs Invalid Input**

```csharp
maskedEntry.ValueChanging += (s, e) =>
{
    if (e.IsValid)
    {
        validationIcon.Source = "checkmark.png";
    }
    else
    {
        validationIcon.Source = "warning.png";
    }
};
```

**Example: Log Changes**

```csharp
maskedEntry.ValueChanging += (s, e) =>
{
    Console.WriteLine($"Value changing:");
    Console.WriteLine($"  Old: {e.OldValue}");
    Console.WriteLine($"  New: {e.NewValue}");
    Console.WriteLine($"  Valid: {e.IsValid}");
};
```

### ValueChanged Event

**When:** After the `Value` property has changed  
**Event Args:** `MaskedEntryValueChangedEventArgs`

**Properties:**
- `NewValue` (object): The new value
- `OldValue` (object): The previous value
- `IsMaskCompleted` (bool): Whether all required mask positions are filled

**Use this to:**
- React to completed input
- Update UI based on value
- Enable/disable buttons
- Auto-advance to next field

**Example: Enable Button When Complete**

```csharp
maskedEntry.ValueChanged += (s, e) =>
{
    if (e.IsMaskCompleted)
    {
        submitButton.IsEnabled = true;
        submitButton.BackgroundColor = Colors.Green;
    }
    else
    {
        submitButton.IsEnabled = false;
        submitButton.BackgroundColor = Colors.Gray;
    }
};
```

**Example: Auto-Advance to Next Field**

```csharp
phoneEntry.ValueChanged += (s, e) =>
{
    if (e.IsMaskCompleted)
    {
        // Phone complete, move to next field
        emailEntry.Focus();
    }
};
```

**Example: Real-Time Display Update**

```csharp
maskedEntry.ValueChanged += (s, e) =>
{
    string newValue = e.NewValue?.ToString();
    string oldValue = e.OldValue?.ToString();
    
    resultLabel.Text = $"Current: {newValue}";
    historyLabel.Text = $"Previous: {oldValue}";
    
    if (e.IsMaskCompleted)
    {
        statusLabel.Text = "✓ Complete";
        statusLabel.TextColor = Colors.Green;
    }
    else
    {
        statusLabel.Text = "○ Incomplete";
        statusLabel.TextColor = Colors.Orange;
    }
};
```

**Example: Validate Date Range**

```csharp
dateEntry.ValueChanged += async (s, e) =>
{
    if (e.IsMaskCompleted)
    {
        if (DateTime.TryParse(e.NewValue?.ToString(), out DateTime date))
        {
            if (date > DateTime.Now)
            {
                await DisplayAlert("Invalid", "Date cannot be in the future", "OK");
                dateEntry.Value = e.OldValue; // Revert
            }
        }
    }
};
```

## Completion Events

### Completed Event

**When:** User presses the return/enter key  
**Event Args:** `EventArgs`

**Use this to:**
- Submit forms
- Move to next field
- Trigger search
- Execute commands

```xml
<editors:SfMaskedEntry 
    x:Name="searchEntry"
    Completed="OnSearchCompleted" />
```

```csharp
private async void OnSearchCompleted(object sender, EventArgs e)
{
    var entry = sender as SfMaskedEntry;
    string searchTerm = entry.Value?.ToString();
    
    if (!string.IsNullOrEmpty(searchTerm))
    {
        await PerformSearch(searchTerm);
    }
}
```

**Example: Submit Form on Enter**

```csharp
lastFieldEntry.Completed += async (s, e) =>
{
    if (ValidateForm())
    {
        await SubmitForm();
    }
};
```

### ClearButtonClicked Event

**When:** User taps the clear button  
**Event Args:** `EventArgs`

**Use this to:**
- Track user actions
- Reset related fields
- Show confirmation
- Log clear actions

```xml
<editors:SfMaskedEntry 
    x:Name="maskedEntry"
    ClearButtonVisibility="WhileEditing"
    ClearButtonClicked="OnClearButtonClicked" />
```

```csharp
private async void OnClearButtonClicked(object sender, EventArgs e)
{
    bool confirm = await DisplayAlert(
        "Clear", 
        "Are you sure you want to clear this field?", 
        "Yes", 
        "No"
    );
    
    if (!confirm)
    {
        // User cancelled, restore value
        // Note: You'd need to save previous value
    }
}
```

**Example: Reset Form Section**

```csharp
phoneEntry.ClearButtonClicked += (s, e) =>
{
    // Also clear related fields
    extensionEntry.Value = null;
    phoneTypeEntry.SelectedIndex = -1;
    
    // Log action
    analytics.LogEvent("PhoneCleared");
};
```

## Event Lifecycle

Understanding the order of events helps with complex scenarios:

### User Typing Scenario

1. **Focused** (if newly focused)
2. **ValueChanging** (before each character)
3. **ValueChanged** (after each character)
4. **ValueChanging** (before next character)
5. **ValueChanged** (after next character)
6. ...continues for each input...
7. **Unfocused** (when losing focus)

### Return Key Scenario

1. **ValueChanged** (if value was changing)
2. **Completed** (return key pressed)

### Clear Button Scenario

1. **ClearButtonClicked**
2. **ValueChanging** (value about to change to empty)
3. **ValueChanged** (value changed to empty)

### Programmatic Value Change

```csharp
maskedEntry.Value = "5551234567";
```

Triggers:
1. **ValueChanging** (once)
2. **ValueChanged** (once)

(Does NOT trigger Focused, Unfocused, or Completed)

## Common Validation Patterns

### Pattern 1: Multi-Field Validation

```csharp
public class FormPage : ContentPage
{
    public FormPage()
    {
        InitializeComponent();
        
        // Validate all fields before enabling submit
        phoneEntry.ValueChanged += ValidateForm;
        emailEntry.ValueChanged += ValidateForm;
        nameEntry.ValueChanged += ValidateForm;
    }
    
    private void ValidateForm(object sender, MaskedEntryValueChangedEventArgs e)
    {
        bool allValid = 
            phoneEntry.IsMaskCompleted && !phoneEntry.HasError &&
            emailEntry.IsMaskCompleted && !emailEntry.HasError &&
            !string.IsNullOrEmpty(nameEntry.Value?.ToString());
        
        submitButton.IsEnabled = allValid;
    }
}
```

### Pattern 2: Conditional Validation

```csharp
dateEntry.ValueChanged += (s, e) =>
{
    if (!e.IsMaskCompleted)
        return; // Don't validate incomplete input
    
    if (DateTime.TryParse(e.NewValue?.ToString(), out DateTime date))
    {
        // Custom business logic
        if (date.DayOfWeek == DayOfWeek.Sunday)
        {
            errorLabel.Text = "Sunday appointments not available";
            errorLabel.IsVisible = true;
            dateEntry.Stroke = Colors.Red;
        }
        else
        {
            errorLabel.IsVisible = false;
            dateEntry.Stroke = Colors.Green;
        }
    }
};
```

### Pattern 3: Debounced Validation

```csharp
private CancellationTokenSource _validationCts;

emailEntry.ValueChanged += async (s, e) =>
{
    // Cancel previous validation
    _validationCts?.Cancel();
    _validationCts = new CancellationTokenSource();
    
    try
    {
        // Wait 500ms before validating
        await Task.Delay(500, _validationCts.Token);
        
        // Perform expensive validation (e.g., API call)
        bool isUnique = await CheckEmailUnique(e.NewValue?.ToString());
        
        if (!isUnique)
        {
            errorLabel.Text = "Email already registered";
            errorLabel.IsVisible = true;
        }
    }
    catch (TaskCanceledException)
    {
        // User still typing, validation cancelled
    }
};
```

### Pattern 4: Cross-Field Validation

```csharp
startDateEntry.ValueChanged += ValidateDateRange;
endDateEntry.ValueChanged += ValidateDateRange;

private void ValidateDateRange(object sender, MaskedEntryValueChangedEventArgs e)
{
    if (!startDateEntry.IsMaskCompleted || !endDateEntry.IsMaskCompleted)
        return;
    
    DateTime start = DateTime.Parse(startDateEntry.Value.ToString());
    DateTime end = DateTime.Parse(endDateEntry.Value.ToString());
    
    if (end < start)
    {
        errorLabel.Text = "End date must be after start date";
        errorLabel.IsVisible = true;
        submitButton.IsEnabled = false;
    }
    else
    {
        errorLabel.IsVisible = false;
        submitButton.IsEnabled = true;
    }
}
```

### Pattern 5: Progressive Validation

```csharp
phoneEntry.ValueChanged += (s, e) =>
{
    string value = e.NewValue?.ToString();
    
    // Clear previous errors
    errorLabel.IsVisible = false;
    
    // Progressive checks
    if (string.IsNullOrEmpty(value))
    {
        statusLabel.Text = "Required field";
        return;
    }
    
    if (!e.IsMaskCompleted)
    {
        statusLabel.Text = $"Enter {GetRemainingChars(e)} more digits";
        return;
    }
    
    // Full validation
    if (IsValidAreaCode(value.Substring(0, 3)))
    {
        statusLabel.Text = "✓ Valid phone number";
        statusLabel.TextColor = Colors.Green;
    }
    else
    {
        errorLabel.Text = "Invalid area code";
        errorLabel.IsVisible = true;
    }
};
```

### Pattern 6: Async Validation with Loading State

```csharp
private bool _isValidating = false;

usernameEntry.ValueChanged += async (s, e) =>
{
    if (_isValidating || !e.IsMaskCompleted)
        return;
    
    _isValidating = true;
    loadingIndicator.IsRunning = true;
    
    try
    {
        bool available = await CheckUsernameAvailability(e.NewValue?.ToString());
        
        if (available)
        {
            statusLabel.Text = "✓ Username available";
            statusLabel.TextColor = Colors.Green;
        }
        else
        {
            statusLabel.Text = "✗ Username taken";
            statusLabel.TextColor = Colors.Red;
        }
    }
    finally
    {
        _isValidating = false;
        loadingIndicator.IsRunning = false;
    }
};
```

## Best Practices

1. **Choose Appropriate ValidationMode:**
   - Use `KeyPress` for immediate feedback (phone, credit card)
   - Use `LostFocus` for less intrusive validation (complex patterns)

2. **Check IsMaskCompleted:**
   - Don't validate incomplete input aggressively
   - Provide helpful feedback on what's needed

3. **Handle Events at the Right Level:**
   - Use `ValueChanging` to prevent invalid changes
   - Use `ValueChanged` to react to changes
   - Use `Unfocused` for final validation

4. **Provide Visual Feedback:**
   - Change border color based on validation
   - Show checkmarks/warnings
   - Display helpful error messages

5. **Don't Block the UI:**
   - Use async for expensive validation
   - Consider debouncing rapid changes
   - Show loading indicators

6. **Unsubscribe from Events:**
   - Clean up event handlers when disposing

```csharp
protected override void OnDisappearing()
{
    base.OnDisappearing();
    
    // Unsubscribe to prevent memory leaks
    maskedEntry.ValueChanged -= OnValueChanged;
}
```

## Next Steps

- **Customization:** Style your masked entry → [customization.md](customization.md)
- **Advanced Features:** Culture support, passwords → [advanced-features.md](advanced-features.md)
