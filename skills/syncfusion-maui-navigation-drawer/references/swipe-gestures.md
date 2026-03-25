# Swipe Gestures

## Table of Contents
- [EnableSwipeGesture Property](#enableswipegesture-property)
- [TouchThreshold Property](#touchthreshold-property)
- [Swipe Behavior by Position](#swipe-behavior-by-position)
- [Swipe Sensitivity Guidelines](#swipe-sensitivity-guidelines)
- [Common Scenarios](#common-scenarios)
- [Multi-Drawer Swipe Behavior](#multi-drawer-swipe-behavior)
- [Gesture Conflicts and Solutions](#gesture-conflicts-and-solutions)
- [Platform-Specific Considerations](#platform-specific-considerations)
- [Testing Swipe Gestures](#testing-swipe-gestures)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)
- [Related Topics](#related-topics)
- [Quick Reference](#quick-reference)

The Navigation Drawer supports swipe gestures, allowing users to open and close the drawer by swiping from the screen edge. This guide covers enabling, disabling, and configuring swipe behavior.

## EnableSwipeGesture Property

Control whether users can swipe to open/close the drawer.

### Enable Swipe Gesture (Default)

**XAML:**

```xml
<navigationDrawer:SfNavigationDrawer x:Name="navigationDrawer">
    <navigationDrawer:SfNavigationDrawer.DrawerSettings>
        <navigationDrawer:DrawerSettings EnableSwipeGesture="True">
        </navigationDrawer:DrawerSettings>
    </navigationDrawer:SfNavigationDrawer.DrawerSettings>
</navigationDrawer:SfNavigationDrawer>
```

**C#:**

```csharp
SfNavigationDrawer navigationDrawer = new SfNavigationDrawer();
DrawerSettings drawerSettings = new DrawerSettings
{
    EnableSwipeGesture = true
};
navigationDrawer.DrawerSettings = drawerSettings;
this.Content = navigationDrawer;
```

**Default:** Swipe gesture is **enabled** by default.

### Disable Swipe Gesture

```xml
<navigationDrawer:DrawerSettings EnableSwipeGesture="False">
</navigationDrawer:DrawerSettings>
```

```csharp
drawerSettings.EnableSwipeGesture = false;
```

**When to disable:**
- When drawer should only open via button
- During specific app states (e.g., form editing)
- When swipe conflicts with other gestures
- In tutorial/onboarding flows

## TouchThreshold Property

The `TouchThreshold` property defines the swipe-sensitive region from the screen edge, measured in pixels.

### Default TouchThreshold

```csharp
// Default value is 120 pixels
drawerSettings.TouchThreshold = 120;
```

**How it works:**
- Swipe must start within this distance from the drawer's edge
- For left drawer: swipe starts within 120px from left edge
- For right drawer: swipe starts within 120px from right edge
- For top/bottom: swipe starts within threshold from top/bottom edge

### Increase TouchThreshold

```xml
<navigationDrawer:DrawerSettings TouchThreshold="200">
</navigationDrawer:DrawerSettings>
```

```csharp
drawerSettings.TouchThreshold = 200;
```

**Effect:** Larger swipe-sensitive area (easier to trigger)

**When to increase:**
- Improve discoverability for new users
- On larger devices/tablets
- When drawer is primary navigation
- For accessibility/motor control considerations

### Decrease TouchThreshold

```csharp
drawerSettings.TouchThreshold = 60;
```

**Effect:** Smaller swipe-sensitive area (harder to trigger accidentally)

**When to decrease:**
- Prevent accidental drawer opens
- When using swipeable content near edge
- In apps with horizontal scrolling
- To reduce gesture conflicts

## Swipe Behavior by Position

### Left Position (Default)

```csharp
new DrawerSettings
{
    Position = Position.Left,
    EnableSwipeGesture = true,
    TouchThreshold = 120
};
```

**Swipe to open:** Right swipe from left edge  
**Swipe to close:** Left swipe from anywhere on drawer

### Right Position

```csharp
new DrawerSettings
{
    Position = Position.Right,
    EnableSwipeGesture = true,
    TouchThreshold = 120
};
```

**Swipe to open:** Left swipe from right edge  
**Swipe to close:** Right swipe from anywhere on drawer

### Top Position

```csharp
new DrawerSettings
{
    Position = Position.Top,
    EnableSwipeGesture = true,
    TouchThreshold = 120
};
```

**Swipe to open:** Down swipe from top edge  
**Swipe to close:** Up swipe from anywhere on drawer

### Bottom Position

```csharp
new DrawerSettings
{
    Position = Position.Bottom,
    EnableSwipeGesture = true,
    TouchThreshold = 120
};
```

**Swipe to open:** Up swipe from bottom edge  
**Swipe to close:** Down swipe from anywhere on drawer

## Swipe Sensitivity Guidelines

### Mobile Phones

```csharp
// Standard sensitivity for phones
new DrawerSettings
{
    TouchThreshold = 100,  // Moderate
    EnableSwipeGesture = true
};
```

### Tablets

```csharp
// Larger threshold for tablets
new DrawerSettings
{
    TouchThreshold = 150,  // Easier discovery
    EnableSwipeGesture = true
};
```

### Desktop

```csharp
// Smaller threshold or disabled on desktop
new DrawerSettings
{
    TouchThreshold = 60,   // Or disable completely
    EnableSwipeGesture = false  // Prefer button control
};
```

### Dynamic Threshold

```csharp
// Adjust based on device
double GetOptimalTouchThreshold()
{
    if (DeviceInfo.Idiom == DeviceIdiom.Phone)
         return 100;
     if (DeviceInfo.Idiom == DeviceIdiom.Tablet)
         return 150;
     if (DeviceInfo.Idiom == DeviceIdiom.Desktop)
         return 60;

     return 120;
}

navigationDrawer.DrawerSettings = new DrawerSettings
{
    TouchThreshold = GetOptimalTouchThreshold()
};
```

## Common Scenarios

### Scenario 1: Standard Mobile App

```csharp
// Default swipe behavior
new DrawerSettings
{
    Position = Position.Left,
    EnableSwipeGesture = true,
    TouchThreshold = 120
};
```

### Scenario 2: Prevent Accidental Opens

```csharp
// Require deliberate swipe from edge
new DrawerSettings
{
    EnableSwipeGesture = true,
    TouchThreshold = 50  // Small threshold
};
```

### Scenario 3: Easy Discovery Mode

```csharp
// Large swipe area for new users
new DrawerSettings
{
    EnableSwipeGesture = true,
    TouchThreshold = 200  // Easy to discover
};
```

### Scenario 4: Disable During Form Editing

```csharp
private void OnFormFocused(object sender, FocusEventArgs e)
{
    // Disable swipe while editing
    navigationDrawer.DrawerSettings.EnableSwipeGesture = false;
}

private void OnFormUnfocused(object sender, FocusEventArgs e)
{
    // Re-enable swipe
    navigationDrawer.DrawerSettings.EnableSwipeGesture = true;
}
```

### Scenario 5: Tutorial Mode

```csharp
private void StartTutorial()
{
    // Disable swipe, force button use during tutorial
    navigationDrawer.DrawerSettings.EnableSwipeGesture = false;
    
    // Show tooltip
    ShowTooltip("Tap the menu button to open navigation");
}

private void EndTutorial()
{
    // Re-enable swipe after tutorial
    navigationDrawer.DrawerSettings.EnableSwipeGesture = true;
}
```

### Scenario 6: Conditional Swipe Based on State

```csharp
private void UpdateDrawerSwipeState()
{
    // Enable swipe only when logged in
    navigationDrawer.DrawerSettings.EnableSwipeGesture = IsUserLoggedIn;
}
```

## Multi-Drawer Swipe Behavior

When using both primary and secondary drawers positioned on different sides:

```csharp
var navigationDrawer = new SfNavigationDrawer
{
    // Primary drawer (left)
    DrawerSettings = new DrawerSettings
    {
        Position = Position.Left,
        EnableSwipeGesture = true,
        TouchThreshold = 120
    },
    
    // Secondary drawer (right)
    SecondaryDrawerSettings = new DrawerSettings
    {
        Position = Position.Right,
        EnableSwipeGesture = true,
        TouchThreshold = 120
    }
};
```

**Behavior:**
- Swipe from left edge → Opens primary drawer
- Swipe from right edge → Opens secondary drawer
- Both can be independently enabled/disabled

**⚠️ Same Position:** If both drawers are on the same side, the primary drawer responds to swipe gestures.

## Gesture Conflicts and Solutions

### Problem: Swipe Conflicts with ScrollView

When ContentView contains horizontal ScrollView near drawer edge:

**Solution 1: Reduce TouchThreshold**

```csharp
drawerSettings.TouchThreshold = 40;  // Narrow edge detection
```

**Solution 2: Disable Swipe, Use Button**

```csharp
drawerSettings.EnableSwipeGesture = false;  // Button-only control
```

**Solution 3: Add Padding to ScrollView**

```xml
<ScrollView Margin="50,0,0,0">
    <!-- Keep content away from edge -->
</ScrollView>
```

### Problem: Swipe Conflicts with CarouselView

**Solution:** Disable drawer swipe when carousel is active

```csharp
private void OnCarouselScrolled(object sender, ScrolledEventArgs e)
{
    // Temporarily disable drawer swipe
    navigationDrawer.DrawerSettings.EnableSwipeGesture = false;
}

private void OnCarouselScrollEnded(object sender, EventArgs e)
{
    // Re-enable after scrolling stops
    navigationDrawer.DrawerSettings.EnableSwipeGesture = true;
}
```

### Problem: Accidental Opens on Edge Buttons

**Solution:** Reduce threshold or adjust button position

```csharp
// Narrow threshold
drawerSettings.TouchThreshold = 60;
```

```xml
<!-- Move button away from edge -->
<Button Margin="70,0,0,0" Text="Action"/>
```

## Platform-Specific Considerations

### iOS

```csharp
#if IOS
// iOS users expect swipe-to-open
navigationDrawer.DrawerSettings.EnableSwipeGesture = true;
navigationDrawer.DrawerSettings.TouchThreshold = 120;
#endif
```

### Android

```csharp
#if ANDROID
// Android users also expect swipe gesture
navigationDrawer.DrawerSettings.EnableSwipeGesture = true;
navigationDrawer.DrawerSettings.TouchThreshold = 100;
#endif
```

### Windows/macOS

```csharp
#if WINDOWS || MACCATALYST
// Desktop users may prefer button control
navigationDrawer.DrawerSettings.EnableSwipeGesture = false;
#endif
```

## Testing Swipe Gestures

### Test on Real Devices

```csharp
// Log swipe attempts for debugging
navigationDrawer.DrawerOpening += (s, e) =>
{
    System.Diagnostics.Debug.WriteLine("Drawer opening via swipe or button");
};
```

### Verify TouchThreshold

```csharp
// Visual indicator for swipe region (during development)
private void ShowSwipeRegion()
{
    var indicator = new BoxView
    {
        Color = Colors.Red.WithAlpha(0.3f),
        WidthRequest = navigationDrawer.DrawerSettings.TouchThreshold,
        HorizontalOptions = LayoutOptions.Start
    };
    
    // Add to main content temporarily for testing
    // Remove in production
}
```

## Best Practices

### 1. Keep Default Swipe Enabled on Mobile

```csharp
// ✓ Good - Enable on mobile devices
if (DeviceInfo.Idiom == DeviceIdiom.Phone)
{
    drawerSettings.EnableSwipeGesture = true;
}
```

### 2. Provide Alternative Access Method

```csharp
// ✓ Good - Always provide a button even when swipe is enabled
// Users may not discover swipe gesture
```

### 3. Adjust Threshold for Device Type

```csharp
// ✓ Good - Responsive threshold
drawerSettings.TouchThreshold = DeviceInfo.Idiom == DeviceIdiom.Tablet ? 150 : 100;
```

### 4. Disable Temporarily for Conflicts

```csharp
// ✓ Good - Disable during specific interactions
private void OnSwipeableContentActive(bool isActive)
{
    navigationDrawer.DrawerSettings.EnableSwipeGesture = !isActive;
}
```

### 5. Test on Physical Devices

```
✓ Test swipe on actual phones/tablets
✓ Verify comfortable reach for thumb
✓ Check for accidental triggers
✓ Test with different hand positions
```

## Troubleshooting

### Issue: Swipe not working
**Solution:** Ensure EnableSwipeGesture is true and TouchThreshold is reasonable (60-200).

### Issue: Too easy to trigger accidentally
**Solution:** Reduce TouchThreshold to 50-80 pixels.

### Issue: Too hard to trigger
**Solution:** Increase TouchThreshold to 150-200 pixels.

### Issue: Conflicts with horizontal scroll
**Solution:** Reduce TouchThreshold or disable swipe, use button only.

### Issue: Works on emulator but not device
**Solution:** Test TouchThreshold on physical device; emulator sensitivity differs.

## Quick Reference

```csharp
// Enable swipe (default)
drawerSettings.EnableSwipeGesture = true;

// Disable swipe
drawerSettings.EnableSwipeGesture = false;

// Adjust sensitivity
drawerSettings.TouchThreshold = 120;  // Default
drawerSettings.TouchThreshold = 60;   // Less sensitive
drawerSettings.TouchThreshold = 200;  // More sensitive

// Dynamic configuration
drawerSettings.TouchThreshold = DeviceInfo.Idiom == DeviceIdiom.Tablet ? 150 : 100;
```

**Default Values:**
- EnableSwipeGesture: `true`
- TouchThreshold: `120` pixels
