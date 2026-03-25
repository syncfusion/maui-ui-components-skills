# Events in .NET MAUI Radio Button

## Table of Contents
- [Overview](#overview)
- [StateChanged Event](#statechanged-event)
- [StateChanging Event](#statechanging-event)
- [Event Lifecycle](#event-lifecycle)
- [Practical Scenarios](#practical-scenarios)
- [Best Practices](#best-practices)

## Overview

The Syncfusion .NET MAUI Radio Button provides two key events for handling user interactions and state changes:

1. **StateChanged:** Fires after the radio button state has changed
2. **StateChanging:** Fires before the state changes, allowing cancellation

These events enable you to respond to user selections, perform validation, update UI elements, and control the radio button behavior dynamically.

## StateChanged Event

The `StateChanged` event occurs when the value (state) of the `IsChecked` property changes, either through user interaction or programmatic updates.

### Event Arguments

The event provides `StateChangedEventArgs` with the following property:

- **IsChecked** (`bool?`): The new state of the radio button
  - `true`: Radio button is now checked
  - `false`: Radio button is now unchecked
  - `null`: Indeterminate state (rarely used for radio buttons)

### Basic Usage

#### XAML

```xml
<buttons:SfRadioGroup x:Name="notificationGroup">
    <buttons:SfRadioButton Text="Email" 
                           StateChanged="OnNotificationChanged"/>
    <buttons:SfRadioButton Text="SMS" 
                           IsChecked="True"
                           StateChanged="OnNotificationChanged"/>
    <buttons:SfRadioButton Text="Push Notification" 
                           StateChanged="OnNotificationChanged"/>
</buttons:SfRadioGroup>

<Label x:Name="statusLabel" Margin="0,10,0,0"/>
```

```csharp
private void OnNotificationChanged(object sender, StateChangedEventArgs e)
{
    if (sender is SfRadioButton radioButton)
    {
        if (e.IsChecked.HasValue && e.IsChecked.Value)
        {
            statusLabel.Text = $"{radioButton.Text} notifications enabled";
        }
        else if (e.IsChecked.HasValue && !e.IsChecked.Value)
        {
            statusLabel.Text = $"{radioButton.Text} notifications disabled";
        }
    }
}
```

#### C#

```csharp
SfRadioGroup notificationGroup = new SfRadioGroup();

SfRadioButton emailButton = new SfRadioButton { Text = "Email" };
emailButton.StateChanged += OnNotificationChanged;

SfRadioButton smsButton = new SfRadioButton { Text = "SMS", IsChecked = true };
smsButton.StateChanged += OnNotificationChanged;

SfRadioButton pushButton = new SfRadioButton { Text = "Push Notification" };
pushButton.StateChanged += OnNotificationChanged;

notificationGroup.Children.Add(emailButton);
notificationGroup.Children.Add(smsButton);
notificationGroup.Children.Add(pushButton);
```

### Detecting Checked vs Unchecked

```csharp
private void OnStateChanged(object sender, StateChangedEventArgs e)
{
    var radioButton = sender as SfRadioButton;
    
    if (e.IsChecked.HasValue && e.IsChecked.Value)
    {
        // Radio button was checked
        Console.WriteLine($"{radioButton.Text} is now CHECKED");
        HandleCheckedState(radioButton);
    }
    else if (e.IsChecked.HasValue && !e.IsChecked.Value)
    {
        // Radio button was unchecked
        Console.WriteLine($"{radioButton.Text} is now UNCHECKED");
        HandleUncheckedState(radioButton);
    }
}
```

### Updating UI Based on State

```xml
<VerticalStackLayout Spacing="10" Padding="20">
    <Label Text="Select Theme:" FontAttributes="Bold"/>
    
    <buttons:SfRadioGroup x:Name="themeGroup">
        <buttons:SfRadioButton x:Name="lightTheme" 
                               Text="Light Theme" 
                               IsChecked="True"
                               StateChanged="OnThemeChanged"/>
        <buttons:SfRadioButton x:Name="darkTheme" 
                               Text="Dark Theme"
                               StateChanged="OnThemeChanged"/>
        <buttons:SfRadioButton x:Name="autoTheme" 
                               Text="Auto (System)"
                               StateChanged="OnThemeChanged"/>
    </buttons:SfRadioGroup>
    
    <BoxView x:Name="previewBox" 
             HeightRequest="100" 
             WidthRequest="200"
             Margin="0,20,0,0"/>
</VerticalStackLayout>
```

```csharp
private void OnThemeChanged(object sender, StateChangedEventArgs e)
{
    if (e.IsChecked.HasValue && e.IsChecked.Value)
    {
        var selectedButton = sender as SfRadioButton;
        
        if (selectedButton == lightTheme)
        {
            previewBox.Color = Colors.White;
            Application.Current.UserAppTheme = AppTheme.Light;
        }
        else if (selectedButton == darkTheme)
        {
            previewBox.Color = Colors.Black;
            Application.Current.UserAppTheme = AppTheme.Dark;
        }
        else if (selectedButton == autoTheme)
        {
            previewBox.Color = Colors.Gray;
            Application.Current.UserAppTheme = AppTheme.Unspecified;
        }
    }
}
```

### Programmatic State Changes Trigger Events

The `StateChanged` event fires even when you change the state programmatically:

```csharp
private void SetDefaultSelection()
{
    // This will trigger StateChanged event
    emailButton.IsChecked = true;
}

private void OnStateChanged(object sender, StateChangedEventArgs e)
{
    // This will be called even for programmatic changes
    Console.WriteLine("State changed (programmatically or by user)");
}
```

## StateChanging Event

The `StateChanging` event fires **before** the radio button state changes, allowing you to intercept and potentially cancel the change.

### Event Arguments

The `StateChangingEventArgs` provides:

- **IsChecked** (`bool?`): The pending new state
- **Cancel** (`bool`): Set to `true` to prevent the state change

### Basic Usage

#### XAML

```xml
<buttons:SfRadioGroup x:Name="agreementGroup">
    <buttons:SfRadioButton x:Name="agreeButton" 
                           Text="I agree to the terms" 
                           StateChanging="OnAgreementChanging"/>
    <buttons:SfRadioButton x:Name="disagreeButton" 
                           Text="I disagree"
                           StateChanging="OnAgreementChanging"/>
</buttons:SfRadioGroup>
```

```csharp
private void OnAgreementChanging(object sender, StateChangingEventArgs e)
{
    var radioButton = sender as SfRadioButton;
    
    // Prevent checking "I agree" if terms haven't been read
    if (radioButton == agreeButton && 
        e.IsChecked.HasValue && 
        e.IsChecked.Value &&
        !termsHaveBeenRead)
    {
        e.Cancel = true;
        DisplayAlert("Notice", "Please read the terms first", "OK");
    }
}
```

#### C#

```csharp
SfRadioButton restrictedButton = new SfRadioButton { Text = "Restricted Option" };
restrictedButton.StateChanging += (sender, e) =>
{
    if (!userHasPermission)
    {
        e.Cancel = true;
        DisplayAlert("Access Denied", "You don't have permission for this option", "OK");
    }
};
```

### Conditional Validation

```csharp
private bool formIsValid = false;

private void OnStateChanging(object sender, StateChangingEventArgs e)
{
    var radioButton = sender as SfRadioButton;
    
    // Only allow selection if form is valid
    if (e.IsChecked.HasValue && e.IsChecked.Value)
    {
        if (!formIsValid)
        {
            e.Cancel = true;
            DisplayAlert("Validation Error", 
                "Please complete all required fields before making a selection", 
                "OK");
        }
    }
}
```

### Confirmation Dialog Before Selection

```csharp
private async void OnDeletionOptionChanging(object sender, StateChangingEventArgs e)
{
    var radioButton = sender as SfRadioButton;
    
    if (radioButton.Text == "Delete All Data" && 
        e.IsChecked.HasValue && 
        e.IsChecked.Value)
    {
        bool confirm = await DisplayAlert(
            "Confirm Deletion", 
            "This will permanently delete all your data. Continue?", 
            "Yes", 
            "No");
        
        if (!confirm)
        {
            e.Cancel = true;
        }
    }
}
```

### Preventing Deselection

While unusual for radio buttons, you might want to prevent deselection in specific scenarios:

```csharp
private void OnStateChanging(object sender, StateChangingEventArgs e)
{
    // Prevent unchecking the last option
    if (e.IsChecked.HasValue && !e.IsChecked.Value)
    {
        // Check if this is the only checked button in the group
        if (IsLastCheckedButton(sender as SfRadioButton))
        {
            e.Cancel = true;
            DisplayAlert("Notice", "At least one option must be selected", "OK");
        }
    }
}

private bool IsLastCheckedButton(SfRadioButton button)
{
    // Implementation to check if this is the only checked button
    var group = button.Parent as SfRadioGroup;
    if (group != null)
    {
        return group.Children.OfType<SfRadioButton>()
            .Count(rb => rb.IsChecked == true) == 1;
    }
    return false;
}
```

## Event Lifecycle

Understanding the order of events is important for complex scenarios:

1. **User taps radio button** or **Programmatic change** (`IsChecked = true`)
2. **StateChanging** fires on the button being changed
   - Can be cancelled here
3. If not cancelled, **state actually changes**
4. **StateChanged** fires on the button that changed
5. If in a group, other buttons are automatically unchecked (their StateChanged events fire)

### Event Order Example

```csharp
SfRadioButton button1 = new SfRadioButton { Text = "Button 1" };
SfRadioButton button2 = new SfRadioButton { Text = "Button 2", IsChecked = true };

button1.StateChanging += (s, e) => Console.WriteLine("Button 1 - StateChanging");
button1.StateChanged += (s, e) => Console.WriteLine("Button 1 - StateChanged");

button2.StateChanging += (s, e) => Console.WriteLine("Button 2 - StateChanging");
button2.StateChanged += (s, e) => Console.WriteLine("Button 2 - StateChanged");

// User clicks Button 1 (Button 2 is currently checked)
// Console output:
// Button 1 - StateChanging
// Button 1 - StateChanged (checked)
// Button 2 - StateChanged (unchecked)
```

## Practical Scenarios

### Scenario 1: Form Validation

```xml
<VerticalStackLayout Spacing="15" Padding="20">
    <Entry x:Name="nameEntry" Placeholder="Enter your name"/>
    <Entry x:Name="emailEntry" Placeholder="Enter your email"/>
    
    <Label Text="Select Subscription:" FontAttributes="Bold" Margin="0,10,0,0"/>
    <buttons:SfRadioGroup x:Name="subscriptionGroup">
        <buttons:SfRadioButton Text="Free" StateChanging="OnSubscriptionChanging"/>
        <buttons:SfRadioButton Text="Premium" StateChanging="OnSubscriptionChanging"/>
        <buttons:SfRadioButton Text="Enterprise" StateChanging="OnSubscriptionChanging"/>
    </buttons:SfRadioGroup>
    
    <Button Text="Submit" Clicked="OnSubmit"/>
</VerticalStackLayout>
```

```csharp
private void OnSubscriptionChanging(object sender, StateChangingEventArgs e)
{
    if (e.IsChecked.HasValue && e.IsChecked.Value)
    {
        // Validate before allowing selection
        if (string.IsNullOrWhiteSpace(nameEntry.Text))
        {
            e.Cancel = true;
            DisplayAlert("Validation Error", "Please enter your name first", "OK");
            nameEntry.Focus();
            return;
        }
        
        if (string.IsNullOrWhiteSpace(emailEntry.Text))
        {
            e.Cancel = true;
            DisplayAlert("Validation Error", "Please enter your email first", "OK");
            emailEntry.Focus();
            return;
        }
    }
}
```

### Scenario 2: Dynamic Pricing Updates

```xml
<VerticalStackLayout Spacing="10" Padding="20">
    <Label Text="Select Plan:" FontAttributes="Bold"/>
    
    <buttons:SfRadioGroup x:Name="planGroup">
        <buttons:SfRadioButton Text="Monthly" Value="9.99" StateChanged="OnPlanChanged"/>
        <buttons:SfRadioButton Text="Quarterly" Value="24.99" StateChanged="OnPlanChanged"/>
        <buttons:SfRadioButton Text="Annually" Value="89.99" StateChanged="OnPlanChanged" IsChecked="True"/>
    </buttons:SfRadioGroup>
    
    <Label x:Name="priceLabel" FontSize="20" FontAttributes="Bold" Margin="0,10,0,0"/>
    <Label x:Name="savingsLabel" FontSize="14" TextColor="Green"/>
</VerticalStackLayout>
```

```csharp
private void OnPlanChanged(object sender, StateChangedEventArgs e)
{
    if (e.IsChecked.HasValue && e.IsChecked.Value)
    {
        var selectedButton = sender as SfRadioButton;
        if (selectedButton?.Value is string priceStr && 
            double.TryParse(priceStr, out double price))
        {
            priceLabel.Text = $"${price:F2}";
            
            // Calculate and show savings
            double monthlyEquivalent = selectedButton.Text switch
            {
                "Monthly" => price,
                "Quarterly" => price / 3,
                "Annually" => price / 12,
                _ => price
            };
            
            if (selectedButton.Text != "Monthly")
            {
                double savings = (9.99 * GetMonthCount(selectedButton.Text)) - price;
                savingsLabel.Text = $"Save ${savings:F2}!";
                savingsLabel.IsVisible = true;
            }
            else
            {
                savingsLabel.IsVisible = false;
            }
        }
    }
}

private int GetMonthCount(string plan) => plan switch
{
    "Quarterly" => 3,
    "Annually" => 12,
    _ => 1
};
```

### Scenario 3: Multi-Step Form with Progress

```csharp
private int currentStep = 1;
private const int totalSteps = 3;

private void OnNextStepChanging(object sender, StateChangingEventArgs e)
{
    var radioButton = sender as SfRadioButton;
    
    if (e.IsChecked.HasValue && e.IsChecked.Value)
    {
        // Prevent moving to next step if current step is incomplete
        if (!IsCurrentStepComplete())
        {
            e.Cancel = true;
            DisplayAlert("Incomplete", 
                $"Please complete step {currentStep} before proceeding", 
                "OK");
        }
    }
}

private void OnStepChanged(object sender, StateChangedEventArgs e)
{
    if (e.IsChecked.HasValue && e.IsChecked.Value)
    {
        var radioButton = sender as SfRadioButton;
        currentStep = int.Parse(radioButton.Value.ToString());
        
        UpdateProgressIndicator();
        LoadStepContent(currentStep);
    }
}

private bool IsCurrentStepComplete()
{
    return currentStep switch
    {
        1 => !string.IsNullOrWhiteSpace(step1Input.Text),
        2 => step2Checkbox.IsChecked == true,
        3 => true,
        _ => false
    };
}
```

### Scenario 4: Conditional Options

```csharp
private void OnShippingMethodChanging(object sender, StateChangingEventArgs e)
{
    var radioButton = sender as SfRadioButton;
    
    if (radioButton.Text == "International Shipping" && 
        e.IsChecked.HasValue && 
        e.IsChecked.Value)
    {
        // Check if international shipping is available
        if (!IsInternationalShippingAvailable())
        {
            e.Cancel = true;
            DisplayAlert("Not Available", 
                "International shipping is not available for your location", 
                "OK");
        }
    }
}

private void OnShippingMethodChanged(object sender, StateChangedEventArgs e)
{
    if (e.IsChecked.HasValue && e.IsChecked.Value)
    {
        var selectedButton = sender as SfRadioButton;
        
        // Show additional options for express shipping
        if (selectedButton.Text.Contains("Express"))
        {
            expressOptionsPanel.IsVisible = true;
        }
        else
        {
            expressOptionsPanel.IsVisible = false;
        }
        
        // Update shipping cost
        UpdateShippingCost(selectedButton.Value?.ToString());
    }
}
```

## Best Practices

### 1. Choose the Right Event

- **Use StateChanged** for most scenarios (responding to user selections)
- **Use StateChanging** only when you need to validate or cancel changes

### 2. Check for Null Values

Always check if `IsChecked` has a value:

```csharp
if (e.IsChecked.HasValue && e.IsChecked.Value)
{
    // Handle checked state
}
```

### 3. Avoid Heavy Processing

Keep event handlers lightweight. For heavy operations, use async/await:

```csharp
private async void OnStateChanged(object sender, StateChangedEventArgs e)
{
    if (e.IsChecked.HasValue && e.IsChecked.Value)
    {
        await ProcessSelectionAsync(sender as SfRadioButton);
    }
}
```

### 4. Provide User Feedback

When cancelling state changes, always inform the user why:

```csharp
if (!isValid)
{
    e.Cancel = true;
    await DisplayAlert("Cannot Select", "Please complete prerequisites first", "OK");
}
```

### 5. Prefer Group-Level Events

When working with `SfRadioGroup`, prefer `CheckedChanged` over individual `StateChanged` events:

```csharp
// Better approach
radioGroup.CheckedChanged += (sender, args) =>
{
    var current = args.CurrentItem as SfRadioButton;
    // Handle selection
};

// Avoid attaching StateChanged to every button
```

### 6. Unsubscribe When Necessary

If dynamically creating radio buttons, remember to unsubscribe:

```csharp
radioButton.StateChanged -= OnStateChanged;
```

### 7. Handle Both Checked and Unchecked

In groups, buttons get automatically unchecked. Handle both states if needed:

```csharp
private void OnStateChanged(object sender, StateChangedEventArgs e)
{
    var button = sender as SfRadioButton;
    
    if (e.IsChecked == true)
    {
        button.TextColor = Colors.Blue; // Highlight selected
    }
    else
    {
        button.TextColor = Colors.Gray; // Dim unselected
    }
}
```

### 8. Consider Event Order

Remember that changing one button in a group affects others:

```csharp
// When button1 is checked:
// 1. button1 StateChanging fires
// 2. button1 StateChanged fires (IsChecked = true)
// 3. button2 StateChanged fires (IsChecked = false) - automatically unchecked
```
