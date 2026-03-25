# Drawer Positioning and Sizing

## Table of Contents
- [Position Property](#position-property)
- [Left Position (Default)](#left-position-default)
- [Right Position](#right-position)
- [Top Position](#top-position)
- [Bottom Position](#bottom-position)
- [DrawerWidth Property](#drawerwidth-property)
- [DrawerHeight Property](#drawerheight-property)
- [Position and Size Combinations](#position-and-size-combinations)
- [Multi-Drawer Positioning](#multi-drawer-positioning)
- [Best Practices](#best-practices)
- [Common Positioning Scenarios](#common-positioning-scenarios)
- [Troubleshooting](#troubleshooting)
- [Related Topics](#related-topics)
- [Reference](#reference)
- [See Also](#see-also)

The Navigation Drawer can be positioned on any of the four screen edges (Left, Right, Top, Bottom) and sized appropriately for the chosen position. This guide covers positioning configuration and sizing best practices.

## Position Property

The `Position` property controls which edge of the screen the drawer slides from. The drawer can be positioned on:

- **Left** (default) - Slides from left edge
- **Right** - Slides from right edge
- **Top** - Slides from top edge
- **Bottom** - Slides from bottom edge

## Left Position (Default)

The drawer slides in from the left side of the screen.

**XAML:**

```xml
<navigationDrawer:SfNavigationDrawer x:Name="navigationDrawer">
    <navigationDrawer:SfNavigationDrawer.DrawerSettings>
        <navigationDrawer:DrawerSettings Position="Left"
                                         DrawerWidth="250">
        </navigationDrawer:DrawerSettings>
    </navigationDrawer:SfNavigationDrawer.DrawerSettings>
    <navigationDrawer:SfNavigationDrawer.ContentView>
        <Grid/>
    </navigationDrawer:SfNavigationDrawer.ContentView>
</navigationDrawer:SfNavigationDrawer>
```

**C#:**

```csharp
SfNavigationDrawer navigationDrawer = new SfNavigationDrawer();
DrawerSettings drawerSettings = new DrawerSettings
{
    Position = Position.Left,
    DrawerWidth = 250
};
navigationDrawer.DrawerSettings = drawerSettings;
this.Content = navigationDrawer;
```

**When to use:** Standard navigation menus, hamburger menus, primary navigation patterns.

## Right Position

The drawer slides in from the right side of the screen.

**XAML:**

```xml
<navigationDrawer:SfNavigationDrawer x:Name="navigationDrawer">
    <navigationDrawer:SfNavigationDrawer.DrawerSettings>
        <navigationDrawer:DrawerSettings Position="Right"
                                         DrawerWidth="250">
        </navigationDrawer:DrawerSettings>
    </navigationDrawer:SfNavigationDrawer.DrawerSettings>
    <navigationDrawer:SfNavigationDrawer.ContentView>
        <Grid/>
    </navigationDrawer:SfNavigationDrawer.ContentView>
</navigationDrawer:SfNavigationDrawer>
```

**C#:**

```csharp
DrawerSettings drawerSettings = new DrawerSettings
{
    Position = Position.Right,
    DrawerWidth = 250
};
navigationDrawer.DrawerSettings = drawerSettings;
```

**When to use:** Secondary navigation, settings panels, filter options, context-specific actions.

## Top Position

The drawer slides in from the top edge of the screen.

**XAML:**

```xml
<navigationDrawer:SfNavigationDrawer x:Name="navigationDrawer">
    <navigationDrawer:SfNavigationDrawer.DrawerSettings>
        <navigationDrawer:DrawerSettings Position="Top"
                                         DrawerHeight="300">
        </navigationDrawer:DrawerSettings>
    </navigationDrawer:SfNavigationDrawer.DrawerSettings>
    <navigationDrawer:SfNavigationDrawer.ContentView>
        <Grid/>
    </navigationDrawer:SfNavigationDrawer.ContentView>
</navigationDrawer:SfNavigationDrawer>
```

**C#:**

```csharp
DrawerSettings drawerSettings = new DrawerSettings
{
    Position = Position.Top,
    DrawerHeight = 300
};
navigationDrawer.DrawerSettings = drawerSettings;
```

**When to use:** Notification panels, alerts, dropdown menus, search bars.

**💡 Note:** Use `DrawerHeight` (not DrawerWidth) for Top position.

## Bottom Position

The drawer slides in from the bottom edge of the screen.

**XAML:**

```xml
<navigationDrawer:SfNavigationDrawer x:Name="navigationDrawer">
    <navigationDrawer:SfNavigationDrawer.DrawerSettings>
        <navigationDrawer:DrawerSettings Position="Bottom"
                                         DrawerHeight="300">
        </navigationDrawer:DrawerSettings>
    </navigationDrawer:SfNavigationDrawer.DrawerSettings>
    <navigationDrawer:SfNavigationDrawer.ContentView>
        <Grid/>
    </navigationDrawer:SfNavigationDrawer.ContentView>
</navigationDrawer:SfNavigationDrawer>
```

**C#:**

```csharp
DrawerSettings drawerSettings = new DrawerSettings
{
    Position = Position.Bottom,
    DrawerHeight = 300
};
navigationDrawer.DrawerSettings = drawerSettings;
```

**When to use:** Bottom sheets, action menus, media controls, player interfaces.

**💡 Note:** Use `DrawerHeight` (not DrawerWidth) for Bottom position.

## DrawerWidth Property

Controls the width of the drawer when positioned on Left or Right sides.

```xml
<navigationDrawer:DrawerSettings Position="Left" DrawerWidth="250"/>
```

```csharp
drawerSettings.DrawerWidth = 250;
```

**Guidelines:**
- **Mobile:** 250-300 pixels (optimal for phone screens)
- **Tablet:** 300-400 pixels (more screen real estate)
- **Desktop:** 350-450 pixels (larger displays)
- **Minimum:** 200 pixels (avoid going smaller)
- **Maximum:** 80% of screen width (leave main content visible)

**Dynamic Sizing Example:**

```csharp
// Responsive drawer width based on device
var screenWidth = DeviceDisplay.MainDisplayInfo.Width / DeviceDisplay.MainDisplayInfo.Density;
var drawerWidth = screenWidth < 600 ? 250 : 350; // Mobile vs Tablet/Desktop

navigationDrawer.DrawerSettings = new DrawerSettings
{
    DrawerWidth = drawerWidth
};
```

## DrawerHeight Property

Controls the height of the drawer when positioned on Top or Bottom edges.

```xml
<navigationDrawer:DrawerSettings Position="Top" DrawerHeight="300"/>
```

```csharp
drawerSettings.DrawerHeight = 300;
```

**Guidelines:**
- **Notification Panel:** 200-350 pixels
- **Search Bar:** 100-150 pixels
- **Bottom Sheet:** 300-500 pixels
- **Media Player:** 80-120 pixels
- **Maximum:** 60% of screen height (leave content visible)

**Dynamic Sizing Example:**

```csharp
// Responsive drawer height based on content
var screenHeight = DeviceDisplay.MainDisplayInfo.Height / DeviceDisplay.MainDisplayInfo.Density;
var drawerHeight = Math.Min(400, screenHeight * 0.5); // Max 50% of screen

navigationDrawer.DrawerSettings = new DrawerSettings
{
    Position = Position.Bottom,
    DrawerHeight = drawerHeight
};
```

## Position and Size Combinations

### Left/Right Positions
- Use `DrawerWidth` property
- `DrawerHeight` is ignored
- Drawer extends full screen height

```csharp
// Left or Right drawer configuration
new DrawerSettings
{
    Position = Position.Left,  // or Position.Right
    DrawerWidth = 250          // Only DrawerWidth matters
}
```

### Top/Bottom Positions
- Use `DrawerHeight` property
- `DrawerWidth` is ignored
- Drawer extends full screen width

```csharp
// Top or Bottom drawer configuration
new DrawerSettings
{
    Position = Position.Top,   // or Position.Bottom
    DrawerHeight = 300         // Only DrawerHeight matters
}
```

## Multi-Drawer Positioning

When using both primary and secondary drawers, position them on different sides:

```csharp
var navigationDrawer = new SfNavigationDrawer
{
    // Primary drawer on left
    DrawerSettings = new DrawerSettings
    {
        Position = Position.Left,
        DrawerWidth = 250
    },
    
    // Secondary drawer on right
    SecondaryDrawerSettings = new DrawerSettings
    {
        Position = Position.Right,
        DrawerWidth = 200
    }
};
```

**⚠️ Important:** If primary and secondary drawers are set to the same position, the primary drawer will respond to swipe gestures.

## Best Practices

### 1. Choose Position Based on Purpose

| Purpose | Recommended Position | Reasoning |
|---------|---------------------|-----------|
| Main navigation | Left | Standard pattern, familiar to users |
| Settings/Filters | Right | Secondary actions, doesn't interfere with primary nav |
| Notifications | Top | Mimics OS notification behavior |
| Actions/Media | Bottom | Easy thumb reach on mobile |

### 2. Consistent Sizing

```csharp
// Define standard sizes as constants
public static class DrawerConstants
{
    public const double MobileDrawerWidth = 250;
    public const double TabletDrawerWidth = 350;
    public const double NotificationHeight = 250;
    public const double BottomSheetHeight = 400;
}

// Use consistently
drawerSettings.DrawerWidth = DrawerConstants.MobileDrawerWidth;
```

### 3. Responsive Sizing

```csharp
// Adjust based on device idiom
 double GetDrawerWidth()
 {
     if (DeviceInfo.Idiom == DeviceIdiom.Phone)
         return 250;
     if (DeviceInfo.Idiom == DeviceIdiom.Tablet)
         return 350;
     if (DeviceInfo.Idiom == DeviceIdiom.Desktop)
         return 400;

     return 280;
 }

navigationDrawer.DrawerSettings = new DrawerSettings
{
    DrawerWidth = GetDrawerWidth()
};
```

### 4. Consider Safe Areas

```csharp
// Account for notches and system UI on iOS/Android
protected override void OnAppearing()
{
    base.OnAppearing();
    
    var safeInsets = this.SafeAreaInsets;
    
    // Adjust drawer header height to avoid notch
    navigationDrawer.DrawerSettings.DrawerHeaderHeight += safeInsets.Top;
}
```

### 5. Test Different Orientations

```csharp
// Handle orientation changes
protected override void OnSizeAllocated(double width, double height)
{
    base.OnSizeAllocated(width, height);
    
    // Adjust drawer size on orientation change
    bool isPortrait = height > width;
    navigationDrawer.DrawerSettings.DrawerWidth = isPortrait ? 250 : 350;
}
```

## Common Positioning Scenarios

### Scenario 1: Standard Mobile App Navigation

```csharp
// Left drawer with mobile-optimized width
var drawer = new SfNavigationDrawer
{
    DrawerSettings = new DrawerSettings
    {
        Position = Position.Left,
        DrawerWidth = 280
    }
};
```

### Scenario 2: E-Commerce Filters

```csharp
// Right drawer for filter options
var drawer = new SfNavigationDrawer
{
    SecondaryDrawerSettings = new DrawerSettings
    {
        Position = Position.Right,
        DrawerWidth = 300
    }
};
```

### Scenario 3: Notification Center

```csharp
// Top drawer for notifications
var drawer = new SfNavigationDrawer
{
    DrawerSettings = new DrawerSettings
    {
        Position = Position.Top,
        DrawerHeight = 350
    }
};
```

### Scenario 4: Music Player Controls

```csharp
// Bottom drawer for player
var drawer = new SfNavigationDrawer
{
    DrawerSettings = new DrawerSettings
    {
        Position = Position.Bottom,
        DrawerHeight = 120
    }
};
```

## Troubleshooting

### Issue: Drawer too narrow/short to see content
**Solution:** Increase DrawerWidth or DrawerHeight values. Minimum recommended is 200px.

### Issue: Drawer covers entire screen
**Solution:** Reduce DrawerWidth/Height. Keep below 80% of screen dimensions.

### Issue: Wrong property has no effect
**Solution:** 
- Left/Right positions: Use `DrawerWidth` only
- Top/Bottom positions: Use `DrawerHeight` only

### Issue: Drawer content cut off on notched devices
**Solution:** Account for safe area insets:

```csharp
var safeInsets = this.SafeAreaInsets;
drawerSettings.DrawerHeaderHeight += safeInsets.Top;
```