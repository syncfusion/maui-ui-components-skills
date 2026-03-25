# Events in .NET MAUI CheckBox

This guide covers event handling in the Syncfusion .NET MAUI CheckBox control, including state change events and cancellation patterns.

## Overview

The SfCheckBox control provides two key events:

1. **StateChanged** - Fires after the checkbox state changes
2. **StateChanging** - Fires before the state changes (cancellable)

## StateChanged Event

The `StateChanged` event occurs when the value of the `IsChecked` property changes, either through user interaction or programmatic changes.

### Event Arguments

The event uses `StateChangedEventArgs` with the following property:

```csharp
public class StateChangedEventArgs : EventArgs
{
    public bool? IsChecked { get; }  // The new state value
}
```

### Basic Usage

**XAML:**
```xml
<buttons:SfCheckBox x:Name="checkBox" 
                    Text="Unchecked State" 
                    IsThreeState="True" 
                    StateChanged="CheckBox_StateChanged"/>
```

**C#:**
```csharp
SfCheckBox checkBox = new SfCheckBox
{
    Text = "Unchecked State",
    IsThreeState = true
};
checkBox.StateChanged += CheckBox_StateChanged;
this.Content = checkBox;
```

### Event Handler Implementation

```csharp
private void CheckBox_StateChanged(object sender, StateChangedEventArgs e)
{
    if (e.IsChecked.HasValue && e.IsChecked.Value)
    {
        // Checked state
        checkBox.Text = "Checked State";
    }
    else if (e.IsChecked.HasValue && !e.IsChecked.Value)
    {
        // Unchecked state
        checkBox.Text = "Unchecked State";
    }
    else
    {
        // Indeterminate state (null)
        checkBox.Text = "Indeterminate State";
    }
}
```

### Common Patterns

#### Pattern 1: Display State in Label

```xml
<StackLayout Padding="20" Spacing="10">
    <buttons:SfCheckBox x:Name="checkBox" 
                        Text="Click me" 
                        StateChanged="UpdateLabel_StateChanged"/>
    <Label x:Name="statusLabel" Text="Unchecked"/>
</StackLayout>
```

```csharp
private void UpdateLabel_StateChanged(object sender, StateChangedEventArgs e)
{
    statusLabel.Text = e.IsChecked == true ? "Checked" : "Unchecked";
}
```

#### Pattern 2: Enable/Disable Controls

```xml
<StackLayout Padding="20">
    <buttons:SfCheckBox x:Name="enableCheckBox" 
                        Text="Enable additional options" 
                        StateChanged="EnableOptions_StateChanged"/>
    
    <StackLayout x:Name="optionsPanel" IsEnabled="False" Margin="20,10,0,0">
        <Entry Placeholder="Option 1"/>
        <Entry Placeholder="Option 2"/>
    </StackLayout>
</StackLayout>
```

```csharp
private void EnableOptions_StateChanged(object sender, StateChangedEventArgs e)
{
    optionsPanel.IsEnabled = e.IsChecked == true;
}
```

#### Pattern 3: Count Selected Items

```xml
<StackLayout Padding="20">
    <Label x:Name="countLabel" Text="Selected: 0"/>
    
    <buttons:SfCheckBox Text="Option 1" StateChanged="UpdateCount_StateChanged"/>
    <buttons:SfCheckBox Text="Option 2" StateChanged="UpdateCount_StateChanged"/>
    <buttons:SfCheckBox Text="Option 3" StateChanged="UpdateCount_StateChanged"/>
</StackLayout>
```

```csharp
private void UpdateCount_StateChanged(object sender, StateChangedEventArgs e)
{
    int count = 0;
    foreach (var child in ((StackLayout)countLabel.Parent).Children)
    {
        if (child is SfCheckBox cb && cb.IsChecked == true)
            count++;
    }
    countLabel.Text = $"Selected: {count}";
}
```

#### Pattern 4: Validation on State Change

```csharp
private void ValidateSelection_StateChanged(object sender, StateChangedEventArgs e)
{
    SfCheckBox checkBox = sender as SfCheckBox;
    
    if (e.IsChecked == true)
    {
        // Validate selection
        if (!IsValidSelection(checkBox))
        {
            DisplayAlert("Error", "Invalid selection", "OK");
            checkBox.IsChecked = false;  // Revert
        }
    }
}

private bool IsValidSelection(SfCheckBox checkBox)
{
    // Your validation logic
    return true;
}
```

## StateChanging Event

The `StateChanging` event fires **before** the checkbox state changes, allowing you to cancel the state change.

### Event Arguments

The event uses `StateChangingEventArgs` with the following properties:

```csharp
public class StateChangingEventArgs : CancelEventArgs
{
    public bool? IsChecked { get; }    // The new state value
    public bool Cancel { get; set; }   // Set to true to cancel the change
}
```

### Basic Usage

**XAML:**
```xml
<buttons:SfCheckBox x:Name="checkBox" 
                    Text="CheckBox" 
                    StateChanging="OnStateChanging"/>
```

**C#:**
```csharp
SfCheckBox checkBox = new SfCheckBox
{
    Text = "CheckBox"
};
checkBox.StateChanging += OnStateChanging;
this.Content = checkBox;
```

### Event Handler Implementation

```csharp
private void OnStateChanging(object sender, StateChangingEventArgs e)
{
    // Cancel the state change
    e.Cancel = true;
}
```

### Cancellation Patterns

#### Pattern 1: Require Confirmation

```xml
<buttons:SfCheckBox x:Name="deleteCheckBox" 
                    Text="Delete all data" 
                    StateChanging="ConfirmDeletion_StateChanging"/>
```

```csharp
private async void ConfirmDeletion_StateChanging(object sender, StateChangingEventArgs e)
{
    if (e.IsChecked == true)
    {
        bool confirmed = await DisplayAlert(
            "Confirm", 
            "Are you sure you want to delete all data?", 
            "Yes", 
            "No");
        
        if (!confirmed)
        {
            e.Cancel = true;  // Cancel the check
        }
    }
}
```

#### Pattern 2: Conditional Validation

```csharp
private void ValidateBeforeChange_StateChanging(object sender, StateChangingEventArgs e)
{
    if (e.IsChecked == true)
    {
        // Check if user meets requirements
        if (!HasRequiredPermissions())
        {
            DisplayAlert("Access Denied", "You don't have permission to enable this option", "OK");
            e.Cancel = true;
        }
    }
}

private bool HasRequiredPermissions()
{
    // Your permission check logic
    return false;
}
```

#### Pattern 3: Maximum Selection Limit

```xml
<StackLayout Padding="20">
    <Label Text="Select up to 2 options"/>
    <buttons:SfCheckBox Text="Option 1" StateChanging="LimitSelection_StateChanging"/>
    <buttons:SfCheckBox Text="Option 2" StateChanging="LimitSelection_StateChanging"/>
    <buttons:SfCheckBox Text="Option 3" StateChanging="LimitSelection_StateChanging"/>
    <buttons:SfCheckBox Text="Option 4" StateChanging="LimitSelection_StateChanging"/>
</StackLayout>
```

```csharp
private const int MaxSelections = 2;

private void LimitSelection_StateChanging(object sender, StateChangingEventArgs e)
{
    if (e.IsChecked == true)
    {
        int currentCount = CountCheckedBoxes();
        
        if (currentCount >= MaxSelections)
        {
            DisplayAlert("Limit Reached", $"You can only select {MaxSelections} options", "OK");
            e.Cancel = true;
        }
    }
}

private int CountCheckedBoxes()
{
    int count = 0;
    // Count logic here
    return count;
}
```

#### Pattern 4: Require Other Selections First

```xml
<StackLayout Padding="20">
    <buttons:SfCheckBox x:Name="termsCheckBox" 
                        Text="Accept Terms and Conditions"/>
    
    <buttons:SfCheckBox x:Name="newsletterCheckBox" 
                        Text="Subscribe to newsletter" 
                        StateChanging="RequireTerms_StateChanging"/>
</StackLayout>
```

```csharp
private void RequireTerms_StateChanging(object sender, StateChangingEventArgs e)
{
    if (e.IsChecked == true && termsCheckBox.IsChecked != true)
    {
        DisplayAlert("Required", "Please accept the terms first", "OK");
        e.Cancel = true;
    }
}
```

## Programmatic State Changes

When you change `IsChecked` programmatically, the events still fire.

### Example:

```csharp
// This will trigger StateChanging and StateChanged events
checkBox.IsChecked = true;
```

### Preventing Recursive Events

When coordinating multiple checkboxes, use a flag to prevent infinite recursion:

```csharp
private bool _isUpdating = false;

private void CheckBox1_StateChanged(object sender, StateChangedEventArgs e)
{
    if (!_isUpdating)
    {
        _isUpdating = true;
        
        // Update other checkboxes
        checkBox2.IsChecked = e.IsChecked;
        checkBox3.IsChecked = e.IsChecked;
        
        _isUpdating = false;
    }
}

private void CheckBox2_StateChanged(object sender, StateChangedEventArgs e)
{
    if (!_isUpdating)
    {
        // Handle without triggering recursion
    }
}
```

## Event-Driven UI Updates

### Example: Multi-Step Form

```xml
<StackLayout Padding="20">
    <Label Text="Step 1: Basic Info" FontAttributes="Bold"/>
    <Entry x:Name="nameEntry" Placeholder="Name"/>
    <buttons:SfCheckBox x:Name="step1CheckBox" 
                        Text="I've completed step 1" 
                        StateChanged="Step1_StateChanged"/>
    
    <StackLayout x:Name="step2Panel" IsVisible="False" Margin="0,20,0,0">
        <Label Text="Step 2: Additional Details" FontAttributes="Bold"/>
        <Entry Placeholder="Email"/>
    </StackLayout>
</StackLayout>
```

```csharp
private void Step1_StateChanged(object sender, StateChangedEventArgs e)
{
    step2Panel.IsVisible = e.IsChecked == true && !string.IsNullOrEmpty(nameEntry.Text);
    
    if (step2Panel.IsVisible)
    {
        // Optionally focus first control in step 2
    }
}
```

### Example: Dynamic Pricing Display

```xml
<StackLayout Padding="20">
    <Label Text="Add-ons" FontSize="18" FontAttributes="Bold"/>
    
    <buttons:SfCheckBox Text="Extra storage ($5/mo)" 
                        StateChanged="UpdatePrice_StateChanged"
                        x:Name="storageCheckBox"/>
    <buttons:SfCheckBox Text="Premium support ($10/mo)" 
                        StateChanged="UpdatePrice_StateChanged"
                        x:Name="supportCheckBox"/>
    <buttons:SfCheckBox Text="Advanced features ($15/mo)" 
                        StateChanged="UpdatePrice_StateChanged"
                        x:Name="featuresCheckBox"/>
    
    <Label x:Name="priceLabel" 
           Text="Total: $0/mo" 
           FontSize="20" 
           FontAttributes="Bold"
           Margin="0,20,0,0"/>
</StackLayout>
```

```csharp
private void UpdatePrice_StateChanged(object sender, StateChangedEventArgs e)
{
    decimal total = 0;
    
    if (storageCheckBox.IsChecked == true) total += 5;
    if (supportCheckBox.IsChecked == true) total += 10;
    if (featuresCheckBox.IsChecked == true) total += 15;
    
    priceLabel.Text = $"Total: ${total}/mo";
}
```

## Best Practices

1. **Use StateChanging for validation** - Cancel invalid state changes before they occur
2. **Use StateChanged for UI updates** - React to confirmed state changes
3. **Check IsChecked.HasValue** - Always verify nullable bool before accessing Value
4. **Prevent recursion** - Use flags when coordinating multiple checkboxes
5. **Async operations** - Be careful with async/await in StateChanging (state may change during await)
6. **Performance** - For many checkboxes, consider throttling or batching updates
7. **User feedback** - Provide clear feedback when cancelling state changes

## Common Gotchas

### Issue: Null Reference Exception

```csharp
// ❌ WRONG - can throw if IsChecked is null
if (e.IsChecked.Value)
{
    // ...
}

// ✅ CORRECT
if (e.IsChecked == true)
{
    // ...
}
```

### Issue: Infinite Recursion

```csharp
// ❌ WRONG - causes infinite loop
private void CheckBox_StateChanged(object sender, StateChangedEventArgs e)
{
    checkBox.IsChecked = !e.IsChecked;  // Triggers event again!
}

// ✅ CORRECT - use flag
private bool _isUpdating = false;

private void CheckBox_StateChanged(object sender, StateChangedEventArgs e)
{
    if (!_isUpdating)
    {
        _isUpdating = true;
        // Make changes
        _isUpdating = false;
    }
}
```

### Issue: Async Timing in StateChanging

```csharp
// ❌ PROBLEMATIC - state may change during await
private async void OnStateChanging(object sender, StateChangingEventArgs e)
{
    bool result = await SomeAsyncCheck();
    e.Cancel = !result;  // May be too late
}

// ✅ BETTER - Use StateChanged with revert
private async void OnStateChanged(object sender, StateChangedEventArgs e)
{
    bool result = await SomeAsyncCheck();
    if (!result)
    {
        checkBox.IsChecked = !e.IsChecked;  // Revert
    }
}
```

## Complete Example: Parent-Child Coordination

```csharp
public partial class MainPage : ContentPage
{
    private bool _isUpdating = false;
    private SfCheckBox selectAll, option1, option2, option3;

    public MainPage()
    {
        InitializeComponent();
        SetupCheckBoxes();
    }

    private void SetupCheckBoxes()
    {
        selectAll = this.FindByName<SfCheckBox>("selectAll");
        option1 = this.FindByName<SfCheckBox>("option1");
        option2 = this.FindByName<SfCheckBox>("option2");
        option3 = this.FindByName<SfCheckBox>("option3");

        selectAll.StateChanged += SelectAll_StateChanged;
        option1.StateChanged += Child_StateChanged;
        option2.StateChanged += Child_StateChanged;
        option3.StateChanged += Child_StateChanged;
    }

    private void SelectAll_StateChanged(object sender, StateChangedEventArgs e)
    {
        if (!_isUpdating)
        {
            _isUpdating = true;
            option1.IsChecked = e.IsChecked;
            option2.IsChecked = e.IsChecked;
            option3.IsChecked = e.IsChecked;
            _isUpdating = false;
        }
    }

    private void Child_StateChanged(object sender, StateChangedEventArgs e)
    {
        if (!_isUpdating)
        {
            _isUpdating = true;
            
            bool allChecked = option1.IsChecked == true && 
                            option2.IsChecked == true && 
                            option3.IsChecked == true;
            
            bool noneChecked = option1.IsChecked == false && 
                             option2.IsChecked == false && 
                             option3.IsChecked == false;
            
            if (allChecked)
                selectAll.IsChecked = true;
            else if (noneChecked)
                selectAll.IsChecked = false;
            else
                selectAll.IsChecked = null;
            
            _isUpdating = false;
        }
    }
}
```
