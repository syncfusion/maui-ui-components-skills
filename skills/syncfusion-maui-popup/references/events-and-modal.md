# Popup Events and Modal Behavior

## Table of Contents
- [Overview](#overview)
- [Popup Events](#popup-events)
- [Event Arguments](#event-arguments)
- [Modal Behavior](#modal-behavior)
- [Button Events](#button-events)
- [Event Patterns](#event-patterns)
- [Best Practices](#best-practices)

## Overview

The .NET MAUI Popup provides comprehensive event handling for lifecycle management and user interactions. Key features:
- Opening, Opened, Closing, and Closed events
- Event cancellation support
- Modal and non-modal behavior
- Accept and decline button events
- Overlay interaction control

## Popup Events

### Event Lifecycle

The popup fires events in the following sequence:

1. **PositionChanging**: Before popup becomes visible (Can handle position before popup becomes visible)
2. **Opening**: Before popup becomes visible (cancellable)
3. **Opened**: After popup is fully visible
4. **Closing**: Before popup dismisses (cancellable)
5. **Closed**: After popup is fully dismissed

### PositionChanging Event

Fired before the popup becomes visible. Position of the popup can be handled before showing.

**XAML:**
```xml
<sfPopup:SfPopup x:Name="popup"
                 PositionChanging="OnPopupPositionChanging"/>
```

**C#:**
```csharp
public MainPage()
{
    InitializeComponent();
    popup.PositionChanging += OnPopupPositionChanging;
}

private void OnPopupPositionChanging(object sender, PopupPositionChangingEventArgs e)
{
    e.MoveTo = new Point(50, 100); 
    e.Handled = true;
}
```

### Opening Event

Fired before the popup becomes visible. Can be cancelled to prevent showing.

**XAML:**
```xml
<sfPopup:SfPopup x:Name="sfPopup"
                 Opening="OnPopupOpening">
</sfPopup:SfPopup>
```

**C#:**
```csharp
using Syncfusion.Maui.Popup;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        sfPopup.Opening += OnPopupOpening;
    }

    private void OnPopupOpening(object sender, CancelEventArgs e)
    {
        // Perform validation or setup before showing
        Console.WriteLine("Popup is about to open");
        
        // Cancel opening if needed
        if (ShouldPreventOpening())
        {
            e.Cancel = true;
        }
    }
    
    private bool ShouldPreventOpening()
    {
        // Your logic here
        return false;
    }
}
```

### Opened Event

Fired after the popup is fully visible and animations complete.

**XAML:**
```xml
<sfPopup:SfPopup x:Name="sfPopup"
                 Opened="OnPopupOpened">
</sfPopup:SfPopup>
```

**C#:**
```csharp
private void OnPopupOpened(object sender, EventArgs e)
{
    Console.WriteLine("Popup is now visible");
    
    // Focus first input field
    var firstEntry = FindFirstEntry(sfPopup);
    firstEntry?.Focus();
}
```

### Closing Event

Fired before the popup dismisses. Can be cancelled to prevent closing.

**XAML:**
```xml
<sfPopup:SfPopup x:Name="sfPopup"
                 Closing="OnPopupClosing">
</sfPopup:SfPopup>
```

**C#:**
```csharp
private void OnPopupClosing(object sender, CancelEventArgs e)
{
    Console.WriteLine("Popup is about to close");
    
    // Validate before closing
    if (HasUnsavedChanges())
    {
        // Prevent closing
        e.Cancel = true;
        
        // Show confirmation
        DisplayAlert("Unsaved Changes", 
                     "You have unsaved changes. Are you sure you want to close?", 
                     "OK");
    }
}

private bool HasUnsavedChanges()
{
    // Your validation logic
    return false;
}
```

### Closed Event

Fired after the popup is fully dismissed and animations complete.

**XAML:**
```xml
<sfPopup:SfPopup x:Name="sfPopup"
                 Closed="OnPopupClosed">
</sfPopup:SfPopup>
```

**C#:**
```csharp
private void OnPopupClosed(object sender, EventArgs e)
{
    Console.WriteLine("Popup is now closed");
    
    // Cleanup or navigate
    ResetForm();
}

private void ResetForm()
{
    // Reset UI state
}
```

### Subscribe to All Events

```csharp
public MainPage()
{
    InitializeComponent();
    
    sfPopup.Opening += OnPopupOpening;
    sfPopup.Opened += OnPopupOpened;
    sfPopup.Closing += OnPopupClosing;
    sfPopup.Closed += OnPopupClosed;
}

private void OnPopupOpening(object sender, CancelEventArgs e)
{
    Console.WriteLine("1. Opening");
}

private void OnPopupOpened(object sender, EventArgs e)
{
    Console.WriteLine("2. Opened");
}

private void OnPopupClosing(object sender, CancelEventArgs e)
{
    Console.WriteLine("3. Closing");
}

private void OnPopupClosed(object sender, EventArgs e)
{
    Console.WriteLine("4. Closed");
}
```

## Event Arguments

### CancelEventArgs

Used by `Opening` and `Closing` events to allow cancellation.

**Properties:**
- **`Cancel`** (bool): Set to `true` to prevent the action

```csharp
private async void OnPopupOpening(object sender, CancelEventArgs e)
{
    // Check permission before opening
    if (!await HasPermissionAsync())
    {
        e.Cancel = true;
        await DisplayAlert("Permission Denied", 
                          "You don't have permission to access this feature.", 
                          "OK");
    }
}

private async Task<bool> HasPermissionAsync()
{
    // Your permission check logic
    return true;
}
```

### Custom Event Data

Pass data through event handlers:

```csharp
private string _popupContext;

private void ShowPopupWithContext(string context)
{
    _popupContext = context;
    sfPopup.Show();
}

private void OnPopupOpened(object sender, EventArgs e)
{
    // Use the context
    Console.WriteLine($"Popup opened with context: {_popupContext}");
}
```

## Modal Behavior

### ShowOverlayAlways Property

Controls whether the overlay background is displayed and blocks interaction.

**True (Default) - Modal Behavior:**
```xml
<sfPopup:SfPopup x:Name="sfPopup"
                 ShowOverlayAlways="True">
</sfPopup:SfPopup>
```

- Displays overlay background
- Blocks interaction with content behind popup
- User must interact with popup before accessing other content

**False - Non-Modal Behavior:**
```xml
<sfPopup:SfPopup x:Name="sfPopup"
                 ShowOverlayAlways="False">
</sfPopup:SfPopup>
```

- No overlay background
- Allows interaction with content behind popup
- Popup can be left open while using the app

### Modal Dialog Example

```csharp
private void ShowModalDialog()
{
    sfPopup.HeaderTitle = "Important Message";
    sfPopup.Message = "Please read and acknowledge this message.";
    sfPopup.ShowFooter = true;
    sfPopup.ShowOverlayAlways = true; // Modal behavior
    sfPopup.AppearanceMode = PopupButtonAppearanceMode.OneButton;
    sfPopup.AcceptButtonText = "I Understand";
    
    sfPopup.Show();
}
```

### Non-Modal Notification Example

```csharp
private void ShowNonModalNotification()
{
    sfPopup.HeaderTitle = "Tip";
    sfPopup.Message = "You can continue working while this is visible.";
    sfPopup.ShowFooter = false;
    sfPopup.ShowOverlayAlways = false; // Non-modal behavior
    sfPopup.AutoCloseDuration = 5000;
    
    sfPopup.Show();
}
```

### Close on Overlay Tap

By default, tapping the overlay closes the popup. This behavior is automatic when `ShowOverlayAlways="True"`.

**Prevent Closing on Overlay Tap:**

Use the `Closing` event to conditionally prevent dismissal:

```csharp
private bool _preventDismissal = false;

private void OnPopupClosing(object sender, CancelEventArgs e)
{
    if (_preventDismissal)
    {
        // Prevent closing when user taps overlay
        e.Cancel = true;
    }
}

private void ShowCriticalPopup()
{
    _preventDismissal = true;
    sfPopup.ShowFooter = true;
    sfPopup.Show();
}
```

---

## Modal Window with StaysOpen

### StaysOpen Property

The `StaysOpen` property prevents the popup from closing when the user taps outside (on the overlay). The popup will **only close** when the user explicitly interacts with the built-in close button or calls `Dismiss()` programmatically.

> **Key Difference from `ShowOverlayAlways`:**  
> - `ShowOverlayAlways="True"` shows an overlay but tapping it still closes the popup.  
> - `StaysOpen="True"` keeps the popup open regardless of tapping outside. The popup stays until the close button or a programmatic `Dismiss()` is used.

**XAML — Modal Window (StaysOpen):**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sfPopup="clr-namespace:Syncfusion.Maui.Popup;assembly=Syncfusion.Maui.Popup"
             x:Class="PopupMauiModalWindow.MainPage">
  <ContentPage.Content>
    <StackLayout Padding="20">
        <Button x:Name="clickToShowPopup" Text="ClickToShowPopup"
                VerticalOptions="Start" HorizontalOptions="Center"
                Clicked="ClickToShowPopup_Clicked" />
        <sfPopup:SfPopup x:Name="sfPopup"
                         StaysOpen="True"
                         HeaderTitle="Modal Window"
                         ShowCloseButton="True"
                         WidthRequest="312"
                         HeightRequest="180"
                         HeaderHeight="72">
            <sfPopup:SfPopup.ContentTemplate>
                <DataTemplate>
                    <Label Text="A modal window disables the parent window while the user interacts with the child (modal) window before they return to the parent application."
                           LineBreakMode="WordWrap" LineHeight="1.2"
                           TextColor="#49454E" FontSize="14"
                           FontFamily="Roboto"/>
                </DataTemplate>
            </sfPopup:SfPopup.ContentTemplate>
        </sfPopup:SfPopup>
    </StackLayout>
  </ContentPage.Content>
</ContentPage>
```

**C# — Open the Modal Window:**
```csharp
private void ClickToShowPopup_Clicked(object sender, EventArgs e)
{
    sfPopup.IsOpen = true;
}
```

### Key Properties for Modal Window Behavior

| Property | Value | Effect |
|---|---|---|
| `StaysOpen` | `true` | Popup stays open on overlay tap; only close button or `Dismiss()` closes it |
| `ShowCloseButton` | `true` | Shows the built-in ✕ close icon in the header |
| `ShowOverlayAlways` | `true` (default) | Overlay is shown and blocks background interaction |

### When to Use StaysOpen

- Critical confirmations that must not be accidentally dismissed
- Forms that require explicit save or cancel action
- Legal/terms dialogs requiring deliberate acknowledgment
- Wizards or multi-step flows where accidental dismissal causes data loss

### Overlay Color Customization

While you can't directly set overlay color in properties, you can customize it through styling:

```xml
<!-- In your app's resource dictionary or styles -->
<Style TargetType="sfPopup:SfPopup">
    <Setter Property="OverlayColor" Value="#80000000" />
</Style>
```

## Button Events

### Accept Button Clicked Event

Although not a separate event, you can handle accept button clicks through the `Closed` event:

**Check Button Click via ShowAsync:**
```csharp
private async void OnShowPopup_Clicked(object sender, EventArgs e)
{
    sfPopup.HeaderTitle = "Confirmation";
    sfPopup.Message = "Do you want to proceed?";
    sfPopup.ShowFooter = true;
    sfPopup.AppearanceMode = PopupButtonAppearanceMode.TwoButton;
    
    bool accepted = await sfPopup.ShowAsync();
    
    if (accepted)
    {
        // User clicked Accept
        await ProcessAcceptAction();
    }
    else
    {
        // User clicked Decline or closed popup
        await ProcessDeclineAction();
    }
}

private async Task ProcessAcceptAction()
{
    await DisplayAlert("Accepted", "You clicked Accept", "OK");
}

private async Task ProcessDeclineAction()
{
    await DisplayAlert("Declined", "You clicked Decline", "OK");
}
```

### Custom Footer Button Events

When using custom footer templates, wire up button events directly:

```xml
<sfPopup:SfPopup x:Name="sfPopup" ShowFooter="True">
    <sfPopup:SfPopup.FooterTemplate>
        <DataTemplate>
            <Grid Padding="10" ColumnSpacing="10">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                
                <Button Grid.Column="0"
                        Text="Save"
                        BackgroundColor="#4CAF50"
                        TextColor="White"
                        Clicked="OnSaveClicked" />
                
                <Button Grid.Column="1"
                        Text="Cancel"
                        BackgroundColor="#F44336"
                        TextColor="White"
                        Clicked="OnCancelClicked" />
            </Grid>
        </DataTemplate>
    </sfPopup:SfPopup.FooterTemplate>
</sfPopup:SfPopup>
```

**Event Handlers:**
```csharp
private async void OnSaveClicked(object sender, EventArgs e)
{
    // Validate and save
    if (ValidateForm())
    {
        await SaveData();
        sfPopup.Dismiss();
    }
    else
    {
        await DisplayAlert("Validation Error", 
                          "Please fill all required fields.", 
                          "OK");
    }
}

private void OnCancelClicked(object sender, EventArgs e)
{
    sfPopup.Dismiss();
}

private bool ValidateForm()
{
    // Your validation logic
    return true;
}

private async Task SaveData()
{
    // Your save logic
    await Task.CompletedTask;
}
```

## Event Patterns

### Validation Pattern

Prevent closing until validation passes:

```csharp
private bool _isValidated = false;

private void OnPopupClosing(object sender, CancelEventArgs e)
{
    if (!_isValidated)
    {
        if (!ValidateInput())
        {
            e.Cancel = true;
            DisplayAlert("Error", "Please complete all required fields.", "OK");
        }
    }
}

private async void OnSubmitClicked(object sender, EventArgs e)
{
    if (ValidateInput())
    {
        _isValidated = true;
        await SaveData();
        sfPopup.Dismiss();
    }
}

private bool ValidateInput()
{
    // Your validation logic
    return true;
}
```

### Loading State Pattern

Show loading indicator during async operations:

```csharp
private async void OnPopupOpened(object sender, EventArgs e)
{
    // Show loading state
    ShowLoadingIndicator();
    
    try
    {
        // Load data
        var data = await LoadDataAsync();
        
        // Update popup content
        UpdatePopupContent(data);
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", $"Failed to load data: {ex.Message}", "OK");
        sfPopup.Dismiss();
    }
    finally
    {
        HideLoadingIndicator();
    }
}

private async Task<object> LoadDataAsync()
{
    await Task.Delay(1000); // Simulate loading
    return new object();
}

private void ShowLoadingIndicator()
{
    // Show loading UI
}

private void HideLoadingIndicator()
{
    // Hide loading UI
}

private void UpdatePopupContent(object data)
{
    // Update content
}
```

### Confirmation Pattern

Require confirmation before closing:

```csharp
private bool _confirmationShown = false;

private async void OnPopupClosing(object sender, CancelEventArgs e)
{
    if (HasUnsavedChanges() && !_confirmationShown)
    {
        e.Cancel = true;
        _confirmationShown = true;
        
        bool shouldClose = await DisplayAlert(
            "Unsaved Changes",
            "You have unsaved changes. Do you want to discard them?",
            "Discard",
            "Keep Editing"
        );
        
        if (shouldClose)
        {
            _confirmationShown = false;
            sfPopup.Dismiss();
        }
        else
        {
            _confirmationShown = false;
        }
    }
}

private bool HasUnsavedChanges()
{
    // Check for unsaved changes
    return false;
}
```

### Chain Popups Pattern

Open another popup after closing:

```csharp
private void OnFirstPopupClosed(object sender, EventArgs e)
{
    // Open second popup after first closes
    secondPopup.Show();
}

private void ShowSequentialPopups()
{
    firstPopup.Closed += OnFirstPopupClosed;
    firstPopup.Show();
}
```

### Logging Pattern

Track popup interactions:

```csharp
private void SetupPopupLogging()
{
    sfPopup.Opening += (s, e) => 
        LogEvent("Popup Opening", DateTime.Now);
    
    sfPopup.Opened += (s, e) => 
        LogEvent("Popup Opened", DateTime.Now);
    
    sfPopup.Closing += (s, e) => 
        LogEvent("Popup Closing", DateTime.Now);
    
    sfPopup.Closed += (s, e) => 
        LogEvent("Popup Closed", DateTime.Now);
}

private void LogEvent(string eventName, DateTime timestamp)
{
    Console.WriteLine($"[{timestamp:HH:mm:ss.fff}] {eventName}");
}
```

## Best Practices

### 1. Always Unsubscribe from Events

Prevent memory leaks by unsubscribing when appropriate:

```csharp
protected override void OnDisappearing()
{
    base.OnDisappearing();
    
    sfPopup.Opening -= OnPopupOpening;
    sfPopup.Opened -= OnPopupOpened;
    sfPopup.Closing -= OnPopupClosing;
    sfPopup.Closed -= OnPopupClosed;
}
```

### 2. Use Async Event Handlers Carefully

```csharp
// Don't: Fire-and-forget async void
private async void OnPopupOpened(object sender, EventArgs e)
{
    await LoadDataAsync(); // Exceptions may go unhandled
}

// Do: Handle exceptions properly
private async void OnPopupOpened(object sender, EventArgs e)
{
    try
    {
        await LoadDataAsync();
    }
    catch (Exception ex)
    {
        // Handle or log exception
        Console.WriteLine($"Error: {ex.Message}");
    }
}
```

### 3. Avoid Heavy Operations in Opening Event

```csharp
// Don't: Heavy operation in Opening
private void OnPopupOpening(object sender, CancelEventArgs e)
{
    LoadHugeDataset(); // Delays opening
}

// Do: Use Opened event for heavy operations
private async void OnPopupOpened(object sender, EventArgs e)
{
    await LoadHugeDatasetAsync();
}
```

### 4. Provide Clear Cancellation Feedback

```csharp
private void OnPopupClosing(object sender, CancelEventArgs e)
{
    if (HasUnsavedChanges())
    {
        e.Cancel = true;
        
        // Inform user why closing was prevented
        DisplayAlert(
            "Cannot Close",
            "Please save or discard your changes first.",
            "OK"
        );
    }
}
```

### 5. Use Modal Behavior Appropriately

```csharp
// Critical actions: Modal
private void ShowCriticalAction()
{
    sfPopup.ShowOverlayAlways = true;
    sfPopup.Show();
}

// Informational: Non-modal
private void ShowInformation()
{
    sfPopup.ShowOverlayAlways = false;
    sfPopup.AutoCloseDuration = 3000;
    sfPopup.Show();
}
```

### 6. Reset State in Closed Event

```csharp
private void OnPopupClosed(object sender, EventArgs e)
{
    // Reset for next use
    _isValidated = false;
    _confirmationShown = false;
    ResetFormFields();
}

private void ResetFormFields()
{
    // Clear form data
}
```

### 7. Track Popup State

```csharp
private enum PopupState
{
    Closed,
    Opening,
    Open,
    Closing
}

private PopupState _currentState = PopupState.Closed;

private void SetupStateTracking()
{
    sfPopup.Opening += (s, e) => _currentState = PopupState.Opening;
    sfPopup.Opened += (s, e) => _currentState = PopupState.Open;
    sfPopup.Closing += (s, e) => _currentState = PopupState.Closing;
    sfPopup.Closed += (s, e) => _currentState = PopupState.Closed;
}

private bool CanShowPopup()
{
    return _currentState == PopupState.Closed;
}
```

### 8. MVVM-Friendly Event Handling

```csharp
// Use commands and bindings instead of events when possible
public class PopupViewModel : INotifyPropertyChanged
{
    public ICommand OpenPopupCommand { get; }
    public ICommand ClosePopupCommand { get; }
    
    private bool _isPopupOpen;
    public bool IsPopupOpen
    {
        get => _isPopupOpen;
        set
        {
            if (_isPopupOpen != value)
            {
                _isPopupOpen = value;
                OnPropertyChanged(nameof(IsPopupOpen));
                
                if (value)
                {
                    OnPopupOpening();
                }
                else
                {
                    OnPopupClosing();
                }
            }
        }
    }
    
    protected virtual void OnPopupOpening()
    {
        // Logic when popup opens
    }
    
    protected virtual void OnPopupClosing()
    {
        // Logic when popup closes
    }
    
    // INotifyPropertyChanged implementation
    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string propertyName) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
}
```
