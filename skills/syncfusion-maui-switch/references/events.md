# Events in .NET MAUI Switch

## Table of Contents
- [Available Events](#available-events)
- [StateChanged Event](#statechanged-event)
  - [Event Arguments](#event-arguments)
  - [Use Cases for StateChanged](#use-cases-for-statechanged)
- [StateChanging Event](#statechanging-event)
  - [Event Arguments](#event-arguments-1)
  - [Use Cases for StateChanging](#use-cases-for-statechanging)
- [Using Both Events Together](#using-both-events-together)
- [Complete Working Examples](#complete-working-examples)
- [Best Practices](#best-practices)
- [Common Pitfalls](#common-pitfalls)

This guide covers the event handling capabilities of the Syncfusion .NET MAUI Switch (SfSwitch) control, including state change notifications and validation.

## Available Events

The SfSwitch control provides two key events:

1. **StateChanged** - Fired after the state has changed
2. **StateChanging** - Fired before the state changes (with cancellation support)

## StateChanged Event

The `StateChanged` event occurs after the value or state of the `IsOn` property changes. This happens when:
- User taps the switch
- You programmatically set the `IsOn` property
- State transitions complete

### Event Arguments

The `SwitchStateChangedEventArgs` provides:

- **NewValue** (bool?): The current state after the change
- **OldValue** (bool?): The previous state before the change

### Basic Implementation

**XAML:**
```xml
<buttons:SfSwitch x:Name="sfSwitch" 
                  StateChanged="OnSwitchStateChanged"/>
```

**C#:**
```csharp
SfSwitch sfSwitch = new SfSwitch();
sfSwitch.StateChanged += OnSwitchStateChanged;
this.Content = sfSwitch;
```

**Event Handler:**
```csharp
private void OnSwitchStateChanged(object sender, SwitchStateChangedEventArgs e)
{
    bool? newValue = e.NewValue;
    bool? oldValue = e.OldValue;
    
    // Perform actions based on state change
    Console.WriteLine($"State changed from {oldValue} to {newValue}");
}
```

### Use Cases for StateChanged

#### 1. Display Alert on State Change
```csharp
private async void OnSwitchStateChanged(object sender, SwitchStateChangedEventArgs e)
{
    await DisplayAlert("State Changed", 
                      $"Switch is now {(e.NewValue == true ? "ON" : "OFF")}", 
                      "OK");
}
```

#### 2. Update UI Elements
```xml
<VerticalStackLayout>
    <buttons:SfSwitch x:Name="notificationSwitch" 
                      StateChanged="OnNotificationChanged"/>
    <Label x:Name="statusLabel" Text="Notifications: OFF"/>
</VerticalStackLayout>
```

```csharp
private void OnNotificationChanged(object sender, SwitchStateChangedEventArgs e)
{
    statusLabel.Text = e.NewValue switch
    {
        true => "Notifications: ON",
        false => "Notifications: OFF",
        null => "Notifications: Loading...",
        _ => "Notifications: Unknown"
    };
    
    statusLabel.TextColor = e.NewValue == true ? Colors.Green : Colors.Red;
}
```

#### 3. Save Preferences
```csharp
private void OnSwitchStateChanged(object sender, SwitchStateChangedEventArgs e)
{
    // Save state to local preferences
    Preferences.Set("DarkModeEnabled", e.NewValue ?? false);
    
    // Apply theme change
    if (e.NewValue == true)
    {
        Application.Current.UserAppTheme = AppTheme.Dark;
    }
    else
    {
        Application.Current.UserAppTheme = AppTheme.Light;
    }
}
```

#### 4. Trigger API Calls
```csharp
private async void OnSwitchStateChanged(object sender, SwitchStateChangedEventArgs e)
{
    if (e.NewValue == true)
    {
        await EnableFeatureOnServer();
    }
    else
    {
        await DisableFeatureOnServer();
    }
}

private async Task EnableFeatureOnServer()
{
    // API call to enable feature
    await ApiService.UpdateUserPreference("feature_enabled", true);
}
```

#### 5. Handle Indeterminate State
```csharp
private void OnSwitchStateChanged(object sender, SwitchStateChangedEventArgs e)
{
    if (e.NewValue == null)
    {
        // Handle indeterminate state
        statusLabel.Text = "Processing...";
    }
    else if (e.NewValue == true)
    {
        statusLabel.Text = "Enabled";
    }
    else
    {
        statusLabel.Text = "Disabled";
    }
}
```

## StateChanging Event

The `StateChanging` event occurs before the state actually changes. This event is crucial for:
- Validating state transitions
- Preventing unwanted changes
- Displaying confirmation dialogs
- Applying business logic

### Event Arguments

The `SwitchStateChangingEventArgs` provides:

- **NewValue** (bool?): The state the switch is about to change to
- **OldValue** (bool?): The current state before the change
- **Cancel** (bool): Set to `true` to prevent the state change

### Basic Implementation

**XAML:**
```xml
<buttons:SfSwitch x:Name="sfSwitch" 
                  StateChanging="OnSwitchStateChanging"/>
```

**C#:**
```csharp
SfSwitch sfSwitch = new SfSwitch();
sfSwitch.StateChanging += OnSwitchStateChanging;
this.Content = sfSwitch;
```

**Event Handler:**
```csharp
private void OnSwitchStateChanging(object sender, SwitchStateChangingEventArgs e)
{
    bool? newValue = e.NewValue;
    bool? oldValue = e.OldValue;
    
    // Optionally cancel the change
    if (ShouldPreventChange())
    {
        e.Cancel = true;
    }
}
```

### Use Cases for StateChanging

#### 1. Block Indeterminate State
```csharp
private void OnSwitchStateChanging(object sender, SwitchStateChangingEventArgs e)
{
    // Prevent switching to indeterminate state
    if (e.NewValue == null)
    {
        e.Cancel = true;
        DisplayAlert("Invalid State", "Indeterminate state is not allowed", "OK");
    }
}
```

#### 2. Permission Validation
```csharp
private void OnSwitchStateChanging(object sender, SwitchStateChangingEventArgs e)
{
    // Only allow admins to enable this feature
    if (e.NewValue == true && !CurrentUser.IsAdmin)
    {
        e.Cancel = true;
        DisplayAlert("Access Denied", 
                    "Only administrators can enable this feature", 
                    "OK");
    }
}
```

#### 3. Confirmation Dialog
```csharp
private async void OnSwitchStateChanging(object sender, SwitchStateChangingEventArgs e)
{
    if (e.NewValue == false && e.OldValue == true)
    {
        // Confirm before disabling
        bool confirm = await DisplayAlert("Confirm", 
                                         "Are you sure you want to disable this feature?", 
                                         "Yes", "No");
        
        if (!confirm)
        {
            e.Cancel = true;  // User cancelled, keep current state
        }
    }
}
```

#### 4. Business Rule Validation
```csharp
private void OnSwitchStateChanging(object sender, SwitchStateChangingEventArgs e)
{
    // Don't allow disabling if dependent features are active
    if (e.NewValue == false && HasDependentFeaturesActive())
    {
        e.Cancel = true;
        DisplayAlert("Cannot Disable", 
                    "Please disable dependent features first", 
                    "OK");
    }
}

private bool HasDependentFeaturesActive()
{
    return feature1Switch.IsOn == true || feature2Switch.IsOn == true;
}
```

#### 5. Async Validation
```csharp
private async void OnSwitchStateChanging(object sender, SwitchStateChangingEventArgs e)
{
    if (e.NewValue == true)
    {
        // Check server-side permission before enabling
        bool hasPermission = await CheckServerPermissionAsync();
        
        if (!hasPermission)
        {
            e.Cancel = true;
            await DisplayAlert("Error", "Server denied permission", "OK");
        }
    }
}

private async Task<bool> CheckServerPermissionAsync()
{
    // Simulate server check
    await Task.Delay(500);
    return true;  // Replace with actual API call
}
```

## Using Both Events Together

Combining both events provides complete control over state changes:

**XAML:**
```xml
<buttons:SfSwitch x:Name="sfSwitch"
                  StateChanging="OnSwitchStateChanging"
                  StateChanged="OnSwitchStateChanged"/>
```

**C#:**
```csharp
private void OnSwitchStateChanging(object sender, SwitchStateChangingEventArgs e)
{
    // Validation before change
    if (!ValidateStateChange(e.NewValue))
    {
        e.Cancel = true;
        return;
    }
    
    // Optional: Show loading indicator
    LoadingIndicator.IsVisible = true;
}

private void OnSwitchStateChanged(object sender, SwitchStateChangedEventArgs e)
{
    // Hide loading indicator
    LoadingIndicator.IsVisible = false;
    
    // Perform action after successful change
    SavePreference(e.NewValue);
    UpdateUI(e.NewValue);
}
```

## Complete Working Examples

### Example 1: Settings Page with Validation

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:buttons="clr-namespace:Syncfusion.Maui.Buttons;assembly=Syncfusion.Maui.Buttons">
    <VerticalStackLayout Padding="20" Spacing="20">
        
        <Label Text="App Settings" FontSize="24" FontAttributes="Bold"/>
        
        <HorizontalStackLayout Spacing="10">
            <Label Text="Dark Mode" VerticalOptions="Center"/>
            <buttons:SfSwitch x:Name="darkModeSwitch"
                              StateChanged="OnDarkModeChanged"/>
        </HorizontalStackLayout>
        
        <HorizontalStackLayout Spacing="10">
            <Label Text="Notifications" VerticalOptions="Center"/>
            <buttons:SfSwitch x:Name="notificationsSwitch"
                              StateChanging="OnNotificationsChanging"
                              StateChanged="OnNotificationsChanged"/>
        </HorizontalStackLayout>
        
        <HorizontalStackLayout Spacing="10">
            <Label Text="Auto-Save" VerticalOptions="Center"/>
            <buttons:SfSwitch x:Name="autoSaveSwitch"
                              IsOn="True"
                              StateChanged="OnAutoSaveChanged"/>
        </HorizontalStackLayout>
        
        <Label x:Name="statusLabel" 
               Text=""
               TextColor="Gray"
               FontSize="12"/>
    </VerticalStackLayout>
</ContentPage>
```

```csharp
public partial class SettingsPage : ContentPage
{
    public SettingsPage()
    {
        InitializeComponent();
        LoadSettings();
    }
    
    private void LoadSettings()
    {
        // Load saved preferences
        darkModeSwitch.IsOn = Preferences.Get("DarkMode", false);
        notificationsSwitch.IsOn = Preferences.Get("Notifications", true);
        autoSaveSwitch.IsOn = Preferences.Get("AutoSave", true);
    }
    
    private void OnDarkModeChanged(object sender, SwitchStateChangedEventArgs e)
    {
        Application.Current.UserAppTheme = e.NewValue == true 
            ? AppTheme.Dark 
            : AppTheme.Light;
            
        Preferences.Set("DarkMode", e.NewValue ?? false);
        statusLabel.Text = $"Theme changed to {(e.NewValue == true ? "Dark" : "Light")}";
    }
    
    private async void OnNotificationsChanging(object sender, SwitchStateChangingEventArgs e)
    {
        // Require confirmation to disable notifications
        if (e.NewValue == false)
        {
            bool confirm = await DisplayAlert(
                "Disable Notifications?",
                "You will not receive important updates",
                "Disable",
                "Cancel");
            
            if (!confirm)
            {
                e.Cancel = true;
            }
        }
    }
    
    private void OnNotificationsChanged(object sender, SwitchStateChangedEventArgs e)
    {
        Preferences.Set("Notifications", e.NewValue ?? false);
        statusLabel.Text = $"Notifications {(e.NewValue == true ? "enabled" : "disabled")}";
    }
    
    private void OnAutoSaveChanged(object sender, SwitchStateChangedEventArgs e)
    {
        Preferences.Set("AutoSave", e.NewValue ?? false);
        statusLabel.Text = $"Auto-save {(e.NewValue == true ? "enabled" : "disabled")}";
    }
}
```

### Example 2: Master-Detail Switch Control

```csharp
// Master switch enables/disables detail switches
private void OnMasterSwitchChanging(object sender, SwitchStateChangingEventArgs e)
{
    if (e.NewValue == false)
    {
        // Warn user that detail switches will be disabled
        DisplayAlert("Warning", 
                    "This will disable all sub-features", 
                    "OK");
    }
}

private void OnMasterSwitchChanged(object sender, SwitchStateChangedEventArgs e)
{
    // Enable or disable all detail switches
    detailSwitch1.IsEnabled = e.NewValue == true;
    detailSwitch2.IsEnabled = e.NewValue == true;
    detailSwitch3.IsEnabled = e.NewValue == true;
    
    // Optionally turn off detail switches when master is off
    if (e.NewValue == false)
    {
        detailSwitch1.IsOn = false;
        detailSwitch2.IsOn = false;
        detailSwitch3.IsOn = false;
    }
}
```

## Best Practices

### 1. Choose the Right Event
- Use **StateChanged** for actions after the change (saving data, updating UI)
- Use **StateChanging** for validation before the change (permissions, confirmations)

### 2. Handle Async Operations Carefully
```csharp
// Good: Simple async operation
private async void OnStateChanged(object sender, SwitchStateChangedEventArgs e)
{
    await SaveToServerAsync(e.NewValue);
}

// Better: With error handling
private async void OnStateChanged(object sender, SwitchStateChangedEventArgs e)
{
    try
    {
        await SaveToServerAsync(e.NewValue);
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", ex.Message, "OK");
        // Revert switch state
        ((SfSwitch)sender).IsOn = e.OldValue;
    }
}
```

### 3. Provide User Feedback
```csharp
private void OnStateChanging(object sender, SwitchStateChangingEventArgs e)
{
    if (e.Cancel)
    {
        // Always explain why the change was cancelled
        DisplayAlert("Action Blocked", "Reason for blocking", "OK");
    }
}
```

### 4. Avoid Recursive Changes
```csharp
// Bad: Can cause infinite loop
private void OnSwitch1Changed(object sender, SwitchStateChangedEventArgs e)
{
    switch2.IsOn = e.NewValue;  // Triggers switch2's StateChanged
}

private void OnSwitch2Changed(object sender, SwitchStateChangedEventArgs e)
{
    switch1.IsOn = e.NewValue;  // Triggers switch1's StateChanged - INFINITE LOOP!
}

// Good: Use flags to prevent recursion
private bool isUpdating = false;

private void OnSwitch1Changed(object sender, SwitchStateChangedEventArgs e)
{
    if (isUpdating) return;
    isUpdating = true;
    switch2.IsOn = e.NewValue;
    isUpdating = false;
}
```

### 5. Clean Up Event Handlers
```csharp
protected override void OnDisappearing()
{
    base.OnDisappearing();
    
    // Unsubscribe to prevent memory leaks
    sfSwitch.StateChanged -= OnSwitchStateChanged;
    sfSwitch.StateChanging -= OnSwitchStateChanging;
}
```

## Common Pitfalls

### Pitfall 1: Not Checking Null Values
```csharp
// Bad: Can throw NullReferenceException
private void OnStateChanged(object sender, SwitchStateChangedEventArgs e)
{
    if (e.NewValue.Value)  // Crashes if NewValue is null
    {
        // ...
    }
}

// Good: Handle null properly
private void OnStateChanged(object sender, SwitchStateChangedEventArgs e)
{
    if (e.NewValue == true)  // Safe null check
    {
        // ...
    }
}
```

### Pitfall 2: Forgetting to Cancel
```csharp
// Bad: Validation doesn't prevent change
private void OnStateChanging(object sender, SwitchStateChangingEventArgs e)
{
    if (!IsValid(e.NewValue))
    {
        DisplayAlert("Invalid", "Cannot change", "OK");
        // Missing: e.Cancel = true;
    }
}
```

### Pitfall 3: Long-Running Operations
```csharp
// Bad: Blocks UI thread
private void OnStateChanged(object sender, SwitchStateChangedEventArgs e)
{
    Thread.Sleep(5000);  // UI freezes!
    SaveData();
}

// Good: Use async
private async void OnStateChanged(object sender, SwitchStateChangedEventArgs e)
{
    await Task.Run(() => SaveData());
}
```
