# Event Handling

## Table of Contents
- [Available Events](#available-events)
- [DrawerOpening Event](#draweropening-event)
- [DrawerOpened Event](#draweropened-event)
- [DrawerClosing Event](#drawerclosing-event)
- [DrawerClosed Event](#drawerclosed-event)
- [DrawerToggled Event](#drawertoggled-event)
- [Event Execution Order](#event-execution-order)
- [Multiple Event Handlers](#multiple-event-handlers)
- [Common Patterns](#common-patterns)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)
- [Related Topics](#related-topics)
- [Quick Reference](#quick-reference)

The Navigation Drawer provides five built-in events for responding to drawer state changes and controlling drawer behavior. These events enable you to execute custom logic, cancel operations, and synchronize UI state.

## Available Events

| Event | When Fired | Cancelable | Event Args |
|-------|------------|------------|------------|
| **DrawerOpening** | Before drawer opens | Yes | CancelEventArgs |
| **DrawerOpened** | After drawer opens | No | EventArgs |
| **DrawerClosing** | Before drawer closes | Yes | CancelEventArgs |
| **DrawerClosed** | After drawer closes | No | EventArgs |
| **DrawerToggled** | After any toggle (open/close) | No | ToggledEventArgs |

## DrawerOpening Event

Triggered **before** the drawer starts opening. Use this event to prevent the drawer from opening or to prepare content.

### Basic Usage

**XAML:**

```xml
<navigationDrawer:SfNavigationDrawer x:Name="navigationDrawer"
                                      DrawerOpening="OnDrawerOpening"/>
```

**Code-Behind:**

```csharp
private void OnDrawerOpening(object sender, CancelEventArgs e)
{
    // Code executes before drawer opens
    System.Diagnostics.Debug.WriteLine("Drawer is about to open");
}
```

### Cancel Opening

```csharp
private void OnDrawerOpening(object sender, CancelEventArgs e)
{
    // Prevent drawer from opening
    e.Cancel = true;
}
```

### Conditional Opening

```csharp
private void OnDrawerOpening(object sender, CancelEventArgs e)
{
    // Only allow opening if user is logged in
    if (!IsUserLoggedIn())
    {
        e.Cancel = true;
        DisplayAlert("Error", "Please log in first", "OK");
    }
}
```

### Prepare Content

```csharp
private void OnDrawerOpening(object sender, CancelEventArgs e)
{
    // Load drawer content before it appears
    LoadMenuItems();
    
    // Update user profile if stale
    if (_profileNeedsRefresh)
    {
        RefreshUserProfile();
    }
}
```

## DrawerOpened Event

Triggered **after** the drawer has fully opened. Use this event to execute logic after the drawer is visible.

### Basic Usage

**XAML:**

```xml
<navigationDrawer:SfNavigationDrawer x:Name="navigationDrawer"
                                      DrawerOpened="OnDrawerOpened"/>
```

**Code-Behind:**

```csharp
private void OnDrawerOpened(object sender, EventArgs e)
{
    // Code executes after drawer is fully open
    System.Diagnostics.Debug.WriteLine("Drawer is now open");
}
```

### Common Use Cases

```csharp
// Focus first menu item
private void OnDrawerOpened(object sender, EventArgs e)
{
    firstMenuItem.Focus();
}

// Log analytics event
private void OnDrawerOpened(object sender, EventArgs e)
{
    Analytics.TrackEvent("DrawerOpened");
}

// Load lazy content
private async void OnDrawerOpened(object sender, EventArgs e)
{
    await LoadDrawerContentAsync();
}

// Update UI state
private void OnDrawerOpened(object sender, EventArgs e)
{
    UpdateHamburgerIcon(isOpen: true);
}
```

## DrawerClosing Event

Triggered **before** the drawer starts closing. Use this event to prevent the drawer from closing or to save state.

### Basic Usage

**XAML:**

```xml
<navigationDrawer:SfNavigationDrawer x:Name="navigationDrawer"
                                      DrawerClosing="OnDrawerClosing"/>
```

**Code-Behind:**

```csharp
private void OnDrawerClosing(object sender, CancelEventArgs e)
{
    // Code executes before drawer closes
    System.Diagnostics.Debug.WriteLine("Drawer is about to close");
}
```

### Cancel Closing

```csharp
private void OnDrawerClosing(object sender, CancelEventArgs e)
{
    // Prevent drawer from closing
    e.Cancel = true;
}
```

### Conditional Closing

```csharp
private bool _hasUnsavedChanges = false;

private async void OnDrawerClosing(object sender, CancelEventArgs e)
{
    if (_hasUnsavedChanges)
    {
        // Cancel closing temporarily
        e.Cancel = true;
        
        // Ask user
        bool shouldClose = await DisplayAlert(
            "Unsaved Changes", 
            "Discard changes?", 
            "Yes", 
            "No");
        
        if (shouldClose)
        {
            _hasUnsavedChanges = false;
            // Close drawer manually
            navigationDrawer.ToggleDrawer();
        }
    }
}
```

### Save State Before Close

```csharp
private void OnDrawerClosing(object sender, CancelEventArgs e)
{
    // Save drawer state
    SaveDrawerPreferences();
    
    // Save scroll position
    SaveScrollPosition();
}
```

## DrawerClosed Event

Triggered **after** the drawer has fully closed. Use this event to clean up or update UI.

### Basic Usage

**XAML:**

```xml
<navigationDrawer:SfNavigationDrawer x:Name="navigationDrawer"
                                      DrawerClosed="OnDrawerClosed"/>
```

**Code-Behind:**

```csharp
private void OnDrawerClosed(object sender, EventArgs e)
{
    // Code executes after drawer is fully closed
    System.Diagnostics.Debug.WriteLine("Drawer is now closed");
}
```

### Common Use Cases

```csharp
// Clear selection
private void OnDrawerClosed(object sender, EventArgs e)
{
    menuCollectionView.SelectedItem = null;
}

// Update hamburger icon
private void OnDrawerClosed(object sender, EventArgs e)
{
    UpdateHamburgerIcon(isOpen: false);
}

// Free resources
private void OnDrawerClosed(object sender, EventArgs e)
{
    // Unload heavy drawer content
    UnloadHeavyContent();
}

// Log analytics
private void OnDrawerClosed(object sender, EventArgs e)
{
    Analytics.TrackEvent("DrawerClosed");
}
```

## DrawerToggled Event

Triggered **after** the drawer is toggled (either opened or closed). This event provides the drawer's current state.

### Basic Usage

**XAML:**

```xml
<navigationDrawer:SfNavigationDrawer x:Name="navigationDrawer"
                                      DrawerToggled="OnDrawerToggled"/>
```

**Code-Behind:**

```csharp
private void OnDrawerToggled(object sender, ToggledEventArgs e)
{
    bool isOpen = e.IsOpen;
    System.Diagnostics.Debug.WriteLine($"Drawer is now: {(isOpen ? "Open" : "Closed")}");
}
```

### Event Arguments

```csharp
private void OnDrawerToggled(object sender, ToggledEventArgs e)
{
    // Check if drawer is open or closed
    if (e.IsOpen)
    {
        // Drawer is now open
        OnDrawerOpenState();
    }
    else
    {
        // Drawer is now closed
        OnDrawerClosedState();
    }
}
```

### Update UI Based on State

```csharp
private void OnDrawerToggled(object sender, ToggledEventArgs e)
{
    // Update hamburger icon
    hamburgerIcon.Source = e.IsOpen ? "close_icon.png" : "menu_icon.png";
    
    // Update status bar color (example)
    UpdateStatusBarColor(e.IsOpen);
    
    // Dim main content when drawer is open
    mainContentOverlay.IsVisible = e.IsOpen;
}
```

## Event Execution Order

When opening a drawer:
1. **DrawerOpening** (can cancel)
2. Animation plays
3. **DrawerOpened**
4. **DrawerToggled** (IsOpen = true)

When closing a drawer:
1. **DrawerClosing** (can cancel)
2. Animation plays
3. **DrawerClosed**
4. **DrawerToggled** (IsOpen = false)

## Multiple Event Handlers

### XAML + Code-Behind

```xml
<navigationDrawer:SfNavigationDrawer x:Name="navigationDrawer"
                                      DrawerOpening="OnDrawerOpening"
                                      DrawerOpened="OnDrawerOpened"
                                      DrawerClosing="OnDrawerClosing"
                                      DrawerClosed="OnDrawerClosed"
                                      DrawerToggled="OnDrawerToggled"/>
```

### Code-Only

```csharp
public MainPage()
{
    InitializeComponent();
    
    // Subscribe to events
    navigationDrawer.DrawerOpening += OnDrawerOpening;
    navigationDrawer.DrawerOpened += OnDrawerOpened;
    navigationDrawer.DrawerClosing += OnDrawerClosing;
    navigationDrawer.DrawerClosed += OnDrawerClosed;
    navigationDrawer.DrawerToggled += OnDrawerToggled;
}

// Unsubscribe when done
protected override void OnDisappearing()
{
    base.OnDisappearing();
    
    navigationDrawer.DrawerOpening -= OnDrawerOpening;
    navigationDrawer.DrawerOpened -= OnDrawerOpened;
    navigationDrawer.DrawerClosing -= OnDrawerClosing;
    navigationDrawer.DrawerClosed -= OnDrawerClosed;
    navigationDrawer.DrawerToggled -= OnDrawerToggled;
}
```

## Common Patterns

### Pattern 1: Hamburger Icon Animation

```csharp
private void OnDrawerToggled(object sender, ToggledEventArgs e)
{
    // Animate hamburger to X and back
    if (e.IsOpen)
    {
        hamburgerIcon.RotateTo(90, 250);
    }
    else
    {
        hamburgerIcon.RotateTo(0, 250);
    }
}
```

### Pattern 2: Authentication Check

```csharp
private void OnDrawerOpening(object sender, CancelEventArgs e)
{
    if (!_authService.IsAuthenticated)
    {
        e.Cancel = true;
        Navigation.PushAsync(new LoginPage());
    }
}
```

### Pattern 3: Analytics Tracking

```csharp
private DateTime _drawerOpenTime;

private void OnDrawerOpened(object sender, EventArgs e)
{
    _drawerOpenTime = DateTime.Now;
    Analytics.TrackEvent("Drawer_Opened");
}

private void OnDrawerClosed(object sender, EventArgs e)
{
    var duration = (DateTime.Now - _drawerOpenTime).TotalSeconds;
    Analytics.TrackEvent("Drawer_Closed", new Dictionary<string, string>
    {
        { "Duration", duration.ToString() }
    });
}
```

### Pattern 4: Load Content on Demand

```csharp
private bool _contentLoaded = false;

private async void OnDrawerOpened(object sender, EventArgs e)
{
    if (!_contentLoaded)
    {
        await LoadDrawerContentAsync();
        _contentLoaded = true;
    }
}
```

### Pattern 5: Prevent Close During Operation

```csharp
private bool _isProcessing = false;

private void OnDrawerClosing(object sender, CancelEventArgs e)
{
    if (_isProcessing)
    {
        e.Cancel = true;
        DisplayAlert("Please Wait", "Operation in progress", "OK");
    }
}
```

### Pattern 6: Dim Content Overlay

```xml
<!-- Overlay grid that appears when drawer opens -->
<Grid x:Name="contentOverlay"
      IsVisible="False"
      BackgroundColor="#80000000"
      InputTransparent="False">
    <Grid.GestureRecognizers>
        <TapGestureRecognizer Tapped="OnOverlayTapped"/>
    </Grid.GestureRecognizers>
</Grid>
```

```csharp
private void OnDrawerToggled(object sender, ToggledEventArgs e)
{
    // Show/hide overlay
    contentOverlay.IsVisible = e.IsOpen;
}

private void OnOverlayTapped(object sender, EventArgs e)
{
    // Close drawer when overlay tapped
    navigationDrawer.ToggleDrawer();
}
```

### Pattern 7: Auto-Close Timer

```csharp
private CancellationTokenSource _autoCloseCts;

private void OnDrawerOpened(object sender, EventArgs e)
{
    // Auto-close after 10 seconds of inactivity
    _autoCloseCts?.Cancel();
    _autoCloseCts = new CancellationTokenSource();
    
    Task.Delay(10000, _autoCloseCts.Token)
        .ContinueWith(t =>
        {
            if (!t.IsCanceled && navigationDrawer.DrawerSettings.IsOpen)
            {
                MainThread.BeginInvokeOnMainThread(() =>
                {
                    navigationDrawer.ToggleDrawer();
                });
            }
        });
}

private void OnDrawerClosed(object sender, EventArgs e)
{
    _autoCloseCts?.Cancel();
}
```

## Best Practices

### 1. Use Appropriate Events for Each Task

```csharp
// ✓ Good - Prepare content before opening
private void OnDrawerOpening(object sender, CancelEventArgs e)
{
    PrepareContent();
}

// ✗ Wrong - Don't prepare in DrawerOpened (too late)
private void OnDrawerOpened(object sender, EventArgs e)
{
    PrepareContent();  // Drawer already visible
}
```

### 2. Always Clean Up Event Handlers

```csharp
protected override void OnDisappearing()
{
    base.OnDisappearing();
    
    // Unsubscribe to prevent memory leaks
    navigationDrawer.DrawerToggled -= OnDrawerToggled;
}
```

### 3. Handle Async Operations Properly

```csharp
// ✓ Good - Async void for event handler
private async void OnDrawerOpened(object sender, EventArgs e)
{
    await LoadContentAsync();
}

// ✗ Wrong - Don't block the UI thread
private void OnDrawerOpened(object sender, EventArgs e)
{
    LoadContentAsync().Wait();  // Blocks UI!
}
```

### 4. Use DrawerToggled for State Synchronization

```csharp
// ✓ Good - Single handler for both states
private void OnDrawerToggled(object sender, ToggledEventArgs e)
{
    SyncUIState(e.IsOpen);
}

// ✗ Less ideal - Duplicate logic
private void OnDrawerOpened(object sender, EventArgs e)
{
    SyncUIState(true);
}

private void OnDrawerClosed(object sender, EventArgs e)
{
    SyncUIState(false);
}
```

### 5. Provide User Feedback When Canceling

```csharp
// ✓ Good - Explain why cancel occurred
private void OnDrawerOpening(object sender, CancelEventArgs e)
{
    if (!CanOpenDrawer())
    {
        e.Cancel = true;
        DisplayAlert("Cannot Open", "Complete current action first", "OK");
    }
}
```

## Troubleshooting

### Issue: Event not firing
**Solution:** Ensure event is subscribed in XAML or code-behind.

### Issue: Cancel not working
**Solution:** Only DrawerOpening and DrawerClosing support cancellation.

### Issue: Drawer state out of sync
**Solution:** Use DrawerToggled event to synchronize state.

### Issue: Memory leaks
**Solution:** Unsubscribe from events in OnDisappearing.

## Quick Reference

```csharp
// Subscribe to all events
navigationDrawer.DrawerOpening += (s, e) => { /* Before open, can cancel */ };
navigationDrawer.DrawerOpened += (s, e) => { /* After opened */ };
navigationDrawer.DrawerClosing += (s, e) => { /* Before close, can cancel */ };
navigationDrawer.DrawerClosed += (s, e) => { /* After closed */ };
navigationDrawer.DrawerToggled += (s, e) => { /* After toggle, check e.IsOpen */ };

// Cancel opening/closing
e.Cancel = true;

// Check drawer state in DrawerToggled
bool isOpen = e.IsOpen;
```
