# Toggling and Drawer Control

## Table of Contents
- [Toggle Methods Overview](#toggle-methods-overview)
- [ToggleDrawer() Method](#toggledrawer-method)
- [IsOpen Property](#isopen-property)
- [Setting Initial State](#setting-initial-state)
- [Toggle from Different UI Elements](#toggle-from-different-ui-elements)
- [Programmatic Control Patterns](#programmatic-control-patterns)
- [Multi-Drawer Toggle](#multi-drawer-toggle)
- [Common Scenarios](#common-scenarios)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)
- [Related Topics](#related-topics)
- [Quick Reference](#quick-reference)

This guide covers all methods for opening, closing, and controlling the Navigation Drawer programmatically and through user interaction.

## Toggle Methods Overview

The drawer can be toggled using three methods:
1. **ToggleDrawer() method** - Programmatic toggle
2. **IsOpen property** - Direct state control
3. **Swipe gesture** - User interaction (covered in swipe-gestures.md)

## ToggleDrawer() Method

The `ToggleDrawer()` method toggles the drawer state (open ↔ closed) regardless of current state.

### Basic Toggle

```csharp
// Toggle drawer (open if closed, close if open)
navigationDrawer.ToggleDrawer();
```

### Common Usage: Hamburger Button

**XAML:**

```xml
<ImageButton Source="hamburgericon.png"
             Clicked="OnHamburgerClicked"/>
```

**Code-Behind:**

```csharp
private void OnHamburgerClicked(object sender, EventArgs e)
{
    navigationDrawer.ToggleDrawer();
}
```

### Inline Toggle

```xml
<ImageButton Source="hamburgericon.png"
             Clicked="(s,e) => navigationDrawer.ToggleDrawer()"/>
```

### Toggle from Menu Item

```csharp
private void OnMenuItemSelected(object sender, SelectionChangedEventArgs e)
{
    // Handle selection
    ProcessMenuItem(e.CurrentSelection.FirstOrDefault());
    
    // Close drawer after selection
    navigationDrawer.ToggleDrawer();
}
```

## IsOpen Property

The `IsOpen` property provides direct control over the drawer state. Unlike `ToggleDrawer()`, you explicitly set whether the drawer is open or closed.

### Opening Drawer

```csharp
// Open the drawer
navigationDrawer.DrawerSettings.IsOpen = true;
```

```xml
<navigationDrawer:DrawerSettings IsOpen="True">
</navigationDrawer:DrawerSettings>
```

### Closing Drawer

```csharp
// Close the drawer
navigationDrawer.DrawerSettings.IsOpen = false;
```

### Checking Drawer State

```csharp
// Check if drawer is currently open
if (navigationDrawer.DrawerSettings.IsOpen)
{
    // Drawer is open
}
else
{
    // Drawer is closed
}
```

### Conditional Toggle

```csharp
// Open only if closed
if (!navigationDrawer.DrawerSettings.IsOpen)
{
    navigationDrawer.DrawerSettings.IsOpen = true;
}

// Close only if open
if (navigationDrawer.DrawerSettings.IsOpen)
{
    navigationDrawer.DrawerSettings.IsOpen = false;
}
```

## Setting Initial State

Control whether the drawer starts open or closed when the app loads.

### Start with Drawer Open

**XAML:**

```xml
<navigationDrawer:SfNavigationDrawer x:Name="navigationDrawer">
    <navigationDrawer:SfNavigationDrawer.DrawerSettings>
        <navigationDrawer:DrawerSettings IsOpen="True"
                                         DrawerWidth="250">
        </navigationDrawer:DrawerSettings>
    </navigationDrawer:SfNavigationDrawer.DrawerSettings>
</navigationDrawer:SfNavigationDrawer>
```

**C#:**

```csharp
SfNavigationDrawer navigationDrawer = new SfNavigationDrawer();
DrawerSettings drawerSettings = new DrawerSettings
{
    IsOpen = true,
    DrawerWidth = 250
};
navigationDrawer.DrawerSettings = drawerSettings;
```

### Start with Drawer Closed (Default)

```csharp
// Drawer starts closed by default
// Explicitly set if needed:
drawerSettings.IsOpen = false;
```

### Conditional Initial State

```csharp
// Open drawer on tablet/desktop, closed on mobile
bool shouldOpenDrawer = DeviceInfo.Idiom != DeviceIdiom.Phone;

navigationDrawer.DrawerSettings = new DrawerSettings
{
    IsOpen = shouldOpenDrawer,
    DrawerWidth = 300
};
```

## Toggle from Different UI Elements

### From Button

```xml
<Button Text="Menu"
        Clicked="OnMenuClicked"/>
```

```csharp
private void OnMenuClicked(object sender, EventArgs e)
{
    navigationDrawer.ToggleDrawer();
}
```

### From TapGestureRecognizer

```xml
<Label Text="☰ Menu">
    <Label.GestureRecognizers>
        <TapGestureRecognizer Tapped="OnMenuTapped"/>
    </Label.GestureRecognizers>
</Label>
```

```csharp
private void OnMenuTapped(object sender, EventArgs e)
{
    navigationDrawer.ToggleDrawer();
}
```

### From Toolbar Item

```xml
<ContentPage.ToolbarItems>
    <ToolbarItem Text="Menu"
                 IconImageSource="menu.png"
                 Clicked="OnToolbarMenuClicked"/>
</ContentPage.ToolbarItems>
```

```csharp
private void OnToolbarMenuClicked(object sender, EventArgs e)
{
    navigationDrawer.ToggleDrawer();
}
```

### From Keyboard Shortcut (Desktop)

```csharp
protected override bool OnBackButtonPressed()
{
    // Close drawer on back button (Android)
    if (navigationDrawer.DrawerSettings.IsOpen)
    {
        navigationDrawer.ToggleDrawer();
        return true;  // Handled
    }
    return base.OnBackButtonPressed();
}
```

## Programmatic Control Patterns

### Pattern 1: Open on App Start (First Time Only)

```csharp
protected override void OnAppearing()
{
    base.OnAppearing();
    
    // Show drawer on first launch
    if (!Preferences.Get("HasSeenDrawer", false))
    {
        navigationDrawer.DrawerSettings.IsOpen = true;
        Preferences.Set("HasSeenDrawer", true);
    }
}
```

### Pattern 2: Auto-Close After Delay

```csharp
private async void OnDrawerOpened(object sender, EventArgs e)
{
    // Auto-close after 5 seconds
    await Task.Delay(5000);
    
    if (navigationDrawer.DrawerSettings.IsOpen)
    {
        navigationDrawer.ToggleDrawer();
    }
}
```

### Pattern 3: Close on Content Area Tap

```xml
<navigationDrawer:SfNavigationDrawer.ContentView>
    <Grid>
        <Grid.GestureRecognizers>
            <TapGestureRecognizer Tapped="OnContentTapped"/>
        </Grid.GestureRecognizers>
        <!-- Content -->
    </Grid>
</navigationDrawer:SfNavigationDrawer.ContentView>
```

```csharp
private void OnContentTapped(object sender, EventArgs e)
{
    // Close drawer if open when content is tapped
    if (navigationDrawer.DrawerSettings.IsOpen)
    {
        navigationDrawer.ToggleDrawer();
    }
}
```

### Pattern 4: Toggle from ViewModel (MVVM)

**ViewModel:**

```csharp
public class MainViewModel : INotifyPropertyChanged
{
    private bool _isDrawerOpen;
    
    public bool IsDrawerOpen
    {
        get => _isDrawerOpen;
        set
        {
            _isDrawerOpen = value;
            OnPropertyChanged();
        }
    }
    
    public ICommand ToggleDrawerCommand { get; }
    
    public MainViewModel()
    {
        ToggleDrawerCommand = new Command(() => IsDrawerOpen = !IsDrawerOpen);
    }
}
```

**XAML:**

```xml
<navigationDrawer:DrawerSettings IsOpen="{Binding IsDrawerOpen}"/>

<Button Text="Menu" 
        Command="{Binding ToggleDrawerCommand}"/>
```

### Pattern 5: Close Drawer on Navigation

```csharp
private async void OnMenuItemSelected(object sender, SelectionChangedEventArgs e)
{
    if (e.CurrentSelection.FirstOrDefault() is MenuItem selected)
    {
        // Close drawer first
        navigationDrawer.DrawerSettings.IsOpen = false;
        
        // Wait for animation to complete
        await Task.Delay(250);
        
        // Navigate to page
        await Navigation.PushAsync(new DetailPage(selected));
    }
}
```

## Multi-Drawer Toggle

When using both primary and secondary drawers, toggle them separately.

### Toggle Primary Drawer

```csharp
// Toggle primary drawer (default behavior)
navigationDrawer.ToggleDrawer();

// Or explicitly through DrawerSettings
navigationDrawer.DrawerSettings.IsOpen = !navigationDrawer.DrawerSettings.IsOpen;
```

### Toggle Secondary Drawer

```csharp
// Toggle secondary drawer
navigationDrawer.ToggleSecondaryDrawer();

// Or explicitly through SecondaryDrawerSettings
navigationDrawer.SecondaryDrawerSettings.IsOpen = !navigationDrawer.SecondaryDrawerSettings.IsOpen;
```

### Both Drawers Example

```xml
<Grid ColumnDefinitions="Auto,*,Auto">
    <!-- Left button for primary drawer -->
    <ImageButton Grid.Column="0"
                 Source="menu_left.png"
                 Clicked="OnLeftMenuClicked"/>
    
    <Label Grid.Column="1" 
           Text="Content"
           HorizontalOptions="Center"/>
    
    <!-- Right button for secondary drawer -->
    <ImageButton Grid.Column="2"
                 Source="menu_right.png"
                 Clicked="OnRightMenuClicked"/>
</Grid>
```

```csharp
private void OnLeftMenuClicked(object sender, EventArgs e)
{
    navigationDrawer.ToggleDrawer();  // Primary
}

private void OnRightMenuClicked(object sender, EventArgs e)
{
    navigationDrawer.ToggleSecondaryDrawer();  // Secondary
}
```

**⚠️ Note:** Only one drawer (primary or secondary) can be open at a time.

## Common Scenarios

### Scenario 1: Classic Hamburger Menu

```csharp
// Simple hamburger button toggle
private void OnHamburgerClicked(object sender, EventArgs e)
{
    navigationDrawer.ToggleDrawer();
}
```

### Scenario 2: Menu with Auto-Close

```csharp
private void OnNavigationItemSelected(object sender, SelectionChangedEventArgs e)
{
    // Process selection
    var selected = e.CurrentSelection.FirstOrDefault();
    UpdateContent(selected);
    
    // Always close drawer after selection
    navigationDrawer.ToggleDrawer();
}
```

### Scenario 3: Open Drawer to Show Alert

```csharp
private async Task ShowDrawerAlert(string message)
{
    // Update drawer content with alert
    alertLabel.Text = message;
    
    // Open drawer
    navigationDrawer.DrawerSettings.IsOpen = true;
    
    // Auto-close after 3 seconds
    await Task.Delay(3000);
    navigationDrawer.DrawerSettings.IsOpen = false;
}
```

### Scenario 4: Keyboard Navigation (Desktop)

```csharp
// Handle Ctrl+B to toggle drawer
private void OnKeyboardAccelerator(object sender, KeyboardAcceleratorInvokedEventArgs e)
{
    if (e.KeyboardAccelerator.Key == "B" && e.KeyboardAccelerator.Modifiers == KeyboardAcceleratorModifiers.Ctrl)
    {
        navigationDrawer.ToggleDrawer();
        e.Handled = true;
    }
}
```

### Scenario 5: Responsive Drawer (Always Open on Desktop)

```csharp
protected override void OnSizeAllocated(double width, double height)
{
    base.OnSizeAllocated(width, height);
    
    // Keep drawer open on wide screens
    if (width > 1024)
    {
        navigationDrawer.DrawerSettings.IsOpen = true;
    }
}
```

## Best Practices

### 1. Use ToggleDrawer() for User Actions

```csharp
// ✓ Good - Toggle for button clicks
private void OnButtonClicked(object sender, EventArgs e)
{
    navigationDrawer.ToggleDrawer();
}

// ✗ Less ideal - Managing state manually
private void OnButtonClicked(object sender, EventArgs e)
{
    navigationDrawer.DrawerSettings.IsOpen = !navigationDrawer.DrawerSettings.IsOpen;
}
```

### 2. Use IsOpen for Explicit State

```csharp
// ✓ Good - Explicit state control
private void OnLogin()
{
    navigationDrawer.DrawerSettings.IsOpen = true;  // Always open
}

private void OnLogout()
{
    navigationDrawer.DrawerSettings.IsOpen = false;  // Always closed
}
```

### 3. Close Drawer on Item Selection

```csharp
// ✓ Good - Always close after selection
private void OnItemSelected(object sender, EventArgs e)
{
    ProcessSelection();
    navigationDrawer.ToggleDrawer();  // Close
}
```

### 4. Check State Before Conditional Actions

```csharp
// ✓ Good - Check state first
if (navigationDrawer.DrawerSettings.IsOpen)
{
    // Only close if currently open
    navigationDrawer.ToggleDrawer();
}
```

### 5. Coordinate with Events

```csharp
// ✓ Good - Use events to track state changes
private void OnDrawerToggled(object sender, ToggledEventArgs e)
{
    UpdateUIBasedOnState(e.IsOpen);
}
```

## Troubleshooting

### Issue: ToggleDrawer() does nothing
**Solution:** Ensure DrawerSettings is properly configured with DrawerWidth/Height.

```csharp
// Must have valid size
navigationDrawer.DrawerSettings = new DrawerSettings
{
    DrawerWidth = 250  // Required!
};
```

### Issue: IsOpen property not updating UI
**Solution:** Use data binding or set property directly on DrawerSettings.

```csharp
// ✓ Correct
navigationDrawer.DrawerSettings.IsOpen = true;

// ✗ Wrong - creates new settings instance
navigationDrawer.DrawerSettings = new DrawerSettings { IsOpen = true };
```

### Issue: Drawer opens but immediately closes
**Solution:** Check for conflicting event handlers or gestures.

```csharp
// Remove conflicting tap handlers on ContentView
// Check DrawerClosing event isn't canceling
```

### Issue: Toggle not working for secondary drawer
**Solution:** Use ToggleSecondaryDrawer() method, not ToggleDrawer().

```csharp
// ✓ Correct
navigationDrawer.ToggleSecondaryDrawer();

// ✗ Wrong - toggles primary drawer
navigationDrawer.ToggleDrawer();
```

## Quick Reference

### Methods

| Method | Description | When to Use |
|--------|-------------|-------------|
| `ToggleDrawer()` | Toggle primary drawer | User actions (buttons, taps) |
| `ToggleSecondaryDrawer()` | Toggle secondary drawer | Secondary drawer controls |

### Properties

| Property | Type | Description |
|----------|------|-------------|
| `IsOpen` | bool | Get/set drawer state directly |

### Examples

```csharp
// Toggle
navigationDrawer.ToggleDrawer();

// Open
navigationDrawer.DrawerSettings.IsOpen = true;

// Close
navigationDrawer.DrawerSettings.IsOpen = false;

// Check state
bool isOpen = navigationDrawer.DrawerSettings.IsOpen;
```
