# Switch States in .NET MAUI Switch

## Table of Contents
- [Overview](#overview)
- [On State](#on-state)
- [Off State](#off-state)
- [Indeterminate State](#indeterminate-state)
- [Disabled States](#disabled-states)
  - [Disabled On State](#disabled-on-state)
  - [Disabled Off State](#disabled-off-state)
  - [Disabled Indeterminate State](#disabled-indeterminate-state)
- [State Transitions](#state-transitions)
- [Best Practices](#best-practices)

## Overview

The .NET MAUI Switch (SfSwitch) supports multiple states to handle various UI scenarios. Understanding these states is crucial for implementing intuitive and accessible toggle controls.

**Available States:**
- **On** - Switch is in the "true" or "enabled" position
- **Off** - Switch is in the "false" or "turned off" position (default)
- **Indeterminate** - Switch shows a neutral state (work in progress, loading, or unknown)
- **Disabled On** - On state but not interactive
- **Disabled Off** - Off state but not interactive
- **Disabled Indeterminate** - Indeterminate state but not interactive

**Key Properties:**
- `IsOn` (bool?): Controls the switch state (true, false, or null)
- `AllowIndeterminateState` (bool): Enables indeterminate state support
- `IsEnabled` (bool): Controls whether the switch is interactive

## On State

The On state represents an active or enabled condition. The switch thumb moves to the right (or left in RTL layouts), typically with a highlighted track color.

### Implementation

**XAML:**
```xml
<buttons:SfSwitch IsOn="True" />
```

**C#:**
```csharp
SfSwitch sfSwitch = new SfSwitch();
sfSwitch.IsOn = true;
this.Content = sfSwitch;
```

### When to Use
- User has enabled a feature or setting
- Showing an active state
- Default state when you want the switch "on" initially
- Representing a "yes" or "true" value in forms

### Example: Feature Toggle
```xml
<VerticalStackLayout Spacing="10">
    <Label Text="Dark Mode"/>
    <buttons:SfSwitch x:Name="darkModeSwitch" 
                      IsOn="True"
                      StateChanged="OnDarkModeChanged"/>
</VerticalStackLayout>
```

```csharp
private void OnDarkModeChanged(object sender, SwitchStateChangedEventArgs e)
{
    if (e.NewValue == true)
    {
        Application.Current.UserAppTheme = AppTheme.Dark;
    }
}
```

## Off State

The Off state is the default state, representing an inactive or disabled condition. The switch thumb is positioned to the left (or right in RTL layouts).

### Implementation

**XAML:**
```xml
<!-- Explicit Off state -->
<buttons:SfSwitch IsOn="False" />

<!-- Default (Off) - IsOn not specified -->
<buttons:SfSwitch />
```

**C#:**
```csharp
SfSwitch sfSwitch = new SfSwitch();
sfSwitch.IsOn = false;  // Explicit Off
this.Content = sfSwitch;
```

**Note:** When `IsOn` is not specified, the switch defaults to `false` (Off state).

### When to Use
- User has turned off a feature or setting
- Default state for most switches
- Representing a "no" or "false" value
- Initial state when you want user to explicitly enable something

### Example: Opt-in Setting
```xml
<VerticalStackLayout Spacing="10">
    <Label Text="Subscribe to Newsletter"/>
    <buttons:SfSwitch x:Name="newsletterSwitch" 
                      IsOn="False"/>
</VerticalStackLayout>
```

## Indeterminate State

The Indeterminate state represents an unknown, loading, or work-in-progress condition. The switch thumb typically appears in a middle position or with distinctive styling.

### Enabling Indeterminate State

**Critical:** You must set `AllowIndeterminateState="True"` to enable this feature.

**XAML:**
```xml
<buttons:SfSwitch IsOn="{x:Null}" 
                  AllowIndeterminateState="True" />
```

**C#:**
```csharp
SfSwitch sfSwitch = new SfSwitch();
sfSwitch.AllowIndeterminateState = true;
sfSwitch.IsOn = null;  // Set to indeterminate
this.Content = sfSwitch;
```

### When to Use
- Showing loading or processing state
- Representing a "partially enabled" state
- Displaying a state that's neither fully on nor off
- Handling three-state logic (true/false/unknown)
- Indicating work in progress

### Example 1: Loading State
```csharp
async Task SyncDataAsync()
{
    // Show indeterminate while loading
    syncSwitch.IsOn = null;
    syncSwitch.AllowIndeterminateState = true;
    
    try
    {
        await PerformSyncOperation();
        syncSwitch.IsOn = true;  // Success - switch to On
    }
    catch (Exception ex)
    {
        syncSwitch.IsOn = false; // Failed - switch to Off
        await DisplayAlert("Error", ex.Message, "OK");
    }
}
```

### Example 2: Partial Selection
```csharp
// User selected some but not all items
if (selectedItems.Count == 0)
{
    selectAllSwitch.IsOn = false;
}
else if (selectedItems.Count == totalItems.Count)
{
    selectAllSwitch.IsOn = true;
}
else
{
    selectAllSwitch.AllowIndeterminateState = true;
    selectAllSwitch.IsOn = null;  // Partially selected
}
```

### Behavior Notes
- By default, the switch has only two states (On and Off)
- You must explicitly enable `AllowIndeterminateState` to use null values
- User can tap the switch to cycle through: Off → On → Indeterminate (if allowed)
- Setting `IsOn = null` without enabling `AllowIndeterminateState` may cause issues

## Disabled States

Disabled states make the switch non-interactive while still displaying its current state. The switch appears with reduced opacity or muted colors to indicate it's not available for interaction.

### Disabled On State

Shows an On state that cannot be changed by user interaction.

**XAML:**
```xml
<buttons:SfSwitch IsOn="True" 
                  IsEnabled="False" />
```

**C#:**
```csharp
SfSwitch sfSwitch = new SfSwitch();
sfSwitch.IsOn = true;
sfSwitch.IsEnabled = false;
this.Content = sfSwitch;
```

**When to Use:**
- Display a setting that's enabled but locked
- Show read-only state that user cannot modify
- Indicate a feature is active but cannot be toggled (e.g., enforced by admin policy)

**Example:**
```xml
<VerticalStackLayout Spacing="10">
    <Label Text="Two-Factor Authentication (Required by Policy)"/>
    <buttons:SfSwitch IsOn="True" 
                      IsEnabled="False"/>
</VerticalStackLayout>
```

### Disabled Off State

Shows an Off state that cannot be changed by user interaction.

**XAML:**
```xml
<buttons:SfSwitch IsOn="False" 
                  IsEnabled="False" />
```

**C#:**
```csharp
SfSwitch sfSwitch = new SfSwitch();
sfSwitch.IsOn = false;
sfSwitch.IsEnabled = false;
this.Content = sfSwitch;
```

**When to Use:**
- Display a disabled feature that's not available
- Show read-only Off state
- Indicate a feature cannot be enabled due to restrictions

**Example:**
```xml
<VerticalStackLayout Spacing="10">
    <Label Text="Premium Feature (Upgrade Required)"/>
    <buttons:SfSwitch IsOn="False" 
                      IsEnabled="False"/>
</VerticalStackLayout>
```

### Disabled Indeterminate State

Shows an Indeterminate state that cannot be changed by user interaction.

**XAML:**
```xml
<buttons:SfSwitch AllowIndeterminateState="True" 
                  IsOn="{x:Null}" 
                  IsEnabled="False"/>
```

**C#:**
```csharp
SfSwitch sfSwitch = new SfSwitch();
sfSwitch.AllowIndeterminateState = true;
sfSwitch.IsOn = null;
sfSwitch.IsEnabled = false;
this.Content = sfSwitch;
```

**When to Use:**
- Display a loading state that shouldn't be interrupted
- Show a processing state in read-only mode
- Indicate an unknown state that cannot be changed yet

**Example:**
```csharp
// Disable switch while loading
loadingSwitch.IsEnabled = false;
loadingSwitch.IsOn = null;

// Re-enable after operation completes
await Task.Delay(2000);
loadingSwitch.IsEnabled = true;
loadingSwitch.IsOn = true;
```

## State Transitions

### User-Initiated Transitions

When `AllowIndeterminateState = false` (default):
```
Off ←→ On
```

When `AllowIndeterminateState = true`:
```
Off → On → Indeterminate → Off (cycles)
```

### Programmatic Transitions

You can set any state programmatically:

```csharp
// Direct state assignment
mySwitch.IsOn = true;        // On
mySwitch.IsOn = false;       // Off
mySwitch.IsOn = null;        // Indeterminate (if AllowIndeterminateState = true)

// Toggle programmatically
mySwitch.IsOn = !mySwitch.IsOn;
```

### Conditional State Management

```csharp
private void UpdateSwitchState(int progress)
{
    switch (progress)
    {
        case 0:
            progressSwitch.IsOn = false;
            break;
        case 100:
            progressSwitch.IsOn = true;
            break;
        default:
            progressSwitch.AllowIndeterminateState = true;
            progressSwitch.IsOn = null;
            break;
    }
}
```

## Best Practices

### 1. Choose Appropriate Initial State
```csharp
// Good: Meaningful default
var autoSaveSwitch = new SfSwitch { IsOn = true };  // Auto-save should be on by default

// Good: Opt-in for privacy features
var trackingSwitch = new SfSwitch { IsOn = false };  // Tracking off by default
```

### 2. Use Indeterminate Sparingly
```csharp
// Good: For loading states
sfSwitch.IsOn = null;  // While loading data

// Avoid: As a permanent third option
// Consider using a different control if you need three distinct options
```

### 3. Provide Visual Feedback
```xml
<VerticalStackLayout>
    <buttons:SfSwitch x:Name="notifySwitch" StateChanged="OnNotifyChanged"/>
    <Label x:Name="statusLabel" Text="Status: Off"/>
</VerticalStackLayout>
```

```csharp
private void OnNotifyChanged(object sender, SwitchStateChangedEventArgs e)
{
    statusLabel.Text = e.NewValue switch
    {
        true => "Status: On",
        false => "Status: Off",
        null => "Status: Indeterminate",
        _ => "Status: Unknown"
    };
}
```

### 4. Handle Disabled States Appropriately
```csharp
// Good: Explain why switch is disabled
<VerticalStackLayout>
    <buttons:SfSwitch IsEnabled="False"/>
    <Label Text="Available in Pro version" 
           TextColor="Gray" 
           FontSize="12"/>
</VerticalStackLayout>
```

### 5. Validate State Changes
```csharp
private void OnStateChanging(object sender, SwitchStateChangingEventArgs e)
{
    // Prevent switching to On if not allowed
    if (e.NewValue == true && !UserHasPermission())
    {
        e.Cancel = true;
        DisplayAlert("Access Denied", "You don't have permission", "OK");
    }
}
```

### 6. Persist State Appropriately
```csharp
// Save state to preferences
Preferences.Set("NotificationsEnabled", notificationSwitch.IsOn ?? false);

// Restore state on load
notificationSwitch.IsOn = Preferences.Get("NotificationsEnabled", true);
```

### 7. Accessibility Considerations
- Ensure sufficient color contrast between states
- Provide text labels explaining what the switch controls
- Test with screen readers to ensure state is announced correctly
- Don't rely solely on color to indicate state

## Common Patterns

### Pattern 1: Master/Detail State
```csharp
// Master switch controls detail switches
private void OnMasterSwitchChanged(object sender, SwitchStateChangedEventArgs e)
{
    detailSwitch1.IsEnabled = e.NewValue == true;
    detailSwitch2.IsEnabled = e.NewValue == true;
}
```

### Pattern 2: State Synchronization
```csharp
// Keep switch in sync with app state
private void OnAppStateChanged(AppState newState)
{
    mySwitch.IsOn = newState.IsFeatureEnabled;
}
```

### Pattern 3: Temporary Indeterminate State
```csharp
async Task ProcessAsync()
{
    processSwitch.IsOn = null;  // Show indeterminate
    await DoWork();
    processSwitch.IsOn = true;   // Show result
}
```
