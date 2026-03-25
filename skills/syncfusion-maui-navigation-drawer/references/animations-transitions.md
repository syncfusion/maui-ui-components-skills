# Animations and Transitions

## Table of Contents
- [Overview](#overview)
- [Transition Types](#transition-types)
  - [SlideOnTop](#slideon top-default)
  - [Push](#push)
  - [Reveal](#reveal)
- [Animation Duration](#animation-duration)
- [Animation Easing](#animation-easing)
- [Choosing the Right Transition](#choosing-the-right-transition)
- [Performance Considerations](#performance-considerations)
- [Advanced Customization](#advanced-customization)

## Overview

The Navigation Drawer supports three distinct transition animations that control how the drawer opens and closes. Each animation provides a different visual experience and is suited for different use cases.

**Available Transitions:**
- **SlideOnTop** (default) - Drawer overlays main content
- **Push** - Drawer pushes main content aside
- **Reveal** - Drawer reveals from behind main content

## Transition Types

### SlideOnTop (Default)

The drawer slides over the main content, creating an overlay effect. The main content remains static while the drawer animates into view.

**XAML:**

```xml
<navigationDrawer:SfNavigationDrawer x:Name="navigationDrawer">
    <navigationDrawer:SfNavigationDrawer.DrawerSettings>
        <navigationDrawer:DrawerSettings Transition="SlideOnTop">
        </navigationDrawer:DrawerSettings>
    </navigationDrawer:SfNavigationDrawer.DrawerSettings>
</navigationDrawer:SfNavigationDrawer>
```

**C#:**

```csharp
SfNavigationDrawer navigationDrawer = new SfNavigationDrawer();
DrawerSettings drawerSettings = new DrawerSettings
{
    Transition = Transition.SlideOnTop
};
navigationDrawer.DrawerSettings = drawerSettings;
this.Content = navigationDrawer;
```

**Visual Behavior:**
- Drawer slides in from edge
- Main content stays in place
- Drawer appears above content
- Content may be dimmed/shadowed (platform-dependent)

**When to use:**
- Standard mobile navigation menus
- Temporary overlays that don't affect layout
- When main content should remain visible underneath
- Default choice for most applications
- Compatible with liquid glass effect

**Pros:**
- No layout recalculation needed
- Smooth performance
- Familiar pattern to users
- Works well on all screen sizes

**Cons:**
- Obscures main content
- May feel less integrated

### Push

The drawer pushes the main content aside as it opens. Both the drawer and content animate together.

**XAML:**

```xml
<navigationDrawer:SfNavigationDrawer x:Name="navigationDrawer">
    <navigationDrawer:SfNavigationDrawer.DrawerSettings>
        <navigationDrawer:DrawerSettings Transition="Push">
        </navigationDrawer:DrawerSettings>
    </navigationDrawer:SfNavigationDrawer.DrawerSettings>
</navigationDrawer:SfNavigationDrawer>
```

**C#:**

```csharp
DrawerSettings drawerSettings = new DrawerSettings
{
    Transition = Transition.Push
};
navigationDrawer.DrawerSettings = drawerSettings;
```

**Visual Behavior:**
- Drawer and content move together
- Main content slides to opposite side
- No overlay effect
- Content resizes to fit remaining space

**When to use:**
- Desktop or tablet applications with more screen space
- When both drawer and content should be visible simultaneously
- Applications where context from main content is important
- Side-by-side layout feel

**Pros:**
- Both panels visible at once
- Content remains accessible
- Professional desktop feel
- No content obscured

**Cons:**
- Content reflow during animation
- May not work well on small screens
- Can feel cramped on mobile devices

### Reveal

The drawer is hidden behind the main content. As the drawer opens, the main content slides away to reveal the drawer underneath.

**XAML:**

```xml
<navigationDrawer:SfNavigationDrawer x:Name="navigationDrawer">
    <navigationDrawer:SfNavigationDrawer.DrawerSettings>
        <navigationDrawer:DrawerSettings Transition="Reveal">
        </navigationDrawer:DrawerSettings>
    </navigationDrawer:SfNavigationDrawer.DrawerSettings>
</navigationDrawer:SfNavigationDrawer>
```

**C#:**

```csharp
DrawerSettings drawerSettings = new DrawerSettings
{
    Transition = Transition.Reveal
};
navigationDrawer.DrawerSettings = drawerSettings;
```

**Visual Behavior:**
- Drawer stays stationary
- Main content slides away
- Drawer "revealed" from behind
- Unique sliding effect

**When to use:**
- Creative or unique UI designs
- When you want a distinctive animation
- Applications emphasizing the drawer content
- Artistic or design-focused apps

**Pros:**
- Visually interesting
- Unique user experience
- Drawer appears already in place
- Smooth reveal effect

**Cons:**
- Less common pattern (may confuse users)
- Content still obscured
- Not compatible with liquid glass effect

## Animation Duration

The `Duration` property controls the speed of the drawer animation, measured in milliseconds.

**XAML:**

```xml
<navigationDrawer:DrawerSettings Duration="200">
</navigationDrawer:DrawerSettings>
```

**C#:**

```csharp
drawerSettings.Duration = 200;
```

**Default:** 250 milliseconds

**Guidelines:**
- **Fast:** 150-200ms (snappy, responsive feel)
- **Standard:** 250-300ms (balanced default)
- **Slow:** 350-500ms (dramatic, attention-drawing)
- **Too fast:** <100ms (jarring, hard to follow)
- **Too slow:** >500ms (sluggish, frustrating)

### Duration Examples

**Fast Animation (200ms):**

```csharp
navigationDrawer.DrawerSettings = new DrawerSettings
{
    Transition = Transition.SlideOnTop,
    Duration = 200  // Quick response
};
```

**Slow Animation (400ms):**

```csharp
navigationDrawer.DrawerSettings = new DrawerSettings
{
    Transition = Transition.Reveal,
    Duration = 400  // Dramatic reveal
};
```

**Performance vs. Polish:**
- Shorter durations feel more responsive
- Longer durations appear more polished
- Match duration to transition type
- Test on actual devices

### Duration by Transition Type

Recommended durations for each transition:

```csharp
// SlideOnTop - medium speed
new DrawerSettings
{
    Transition = Transition.SlideOnTop,
    Duration = 250  // Standard feel
};

// Push - slightly faster
new DrawerSettings
{
    Transition = Transition.Push,
    Duration = 220  // Matches content movement
};

// Reveal - can be slower
new DrawerSettings
{
    Transition = Transition.Reveal,
    Duration = 300  // Emphasizes reveal effect
};
```

## Animation Easing

The `AnimationEasing` property customizes the acceleration curve of the animation, affecting how the drawer speeds up and slows down.

**XAML:**

```xml
<navigationDrawer:DrawerSettings AnimationEasing="SpringIn">
</navigationDrawer:DrawerSettings>
```

**C#:**

```csharp
drawerSettings.AnimationEasing = Easing.SpringIn;
```

**Default:** Easing.Linear

### Available Easing Functions

**.NET MAUI Built-in Easings:**

```csharp
// Linear - Constant speed throughout
Easing.Linear

// Ease In - Starts slow, accelerates
Easing.SinIn
Easing.CubicIn

// Ease Out - Starts fast, decelerates
Easing.SinOut
Easing.CubicOut

// Ease In-Out - Slow start and end, fast middle
Easing.SinInOut
Easing.CubicInOut

// Spring - Bouncy effect
Easing.SpringIn
Easing.SpringOut

// Bounce - Bouncing ball effect
Easing.BounceIn
Easing.BounceOut
```

### Easing Examples

**Smooth Deceleration (Recommended):**

```csharp
navigationDrawer.DrawerSettings = new DrawerSettings
{
    Transition = Transition.SlideOnTop,
    Duration = 250,
    AnimationEasing = Easing.CubicOut  // Smooth slow-down
};
```

**Bouncy Effect:**

```csharp
navigationDrawer.DrawerSettings = new DrawerSettings
{
    Transition = Transition.SlideOnTop,
    Duration = 350,
    AnimationEasing = Easing.SpringOut  // Spring effect
};
```

**Custom Easing Function:**

```csharp
// Create custom easing
var customEasing = Easing.CubicInOut;

navigationDrawer.DrawerSettings = new DrawerSettings
{
    AnimationEasing = customEasing
};

// Or use lambda for complete control
navigationDrawer.DrawerSettings = new DrawerSettings
{
    AnimationEasing = new Easing(t => t * t * t)  // Cubic ease-in
};
```

### Easing Recommendations by Transition

```csharp
// SlideOnTop - CubicOut for smooth entry
new DrawerSettings
{
    Transition = Transition.SlideOnTop,
    AnimationEasing = Easing.CubicOut
};

// Push - Linear or SinInOut for coordinated movement
new DrawerSettings
{
    Transition = Transition.Push,
    AnimationEasing = Easing.SinInOut
};

// Reveal - CubicIn for accelerating reveal
new DrawerSettings
{
    Transition = Transition.Reveal,
    AnimationEasing = Easing.CubicIn
};
```

## Choosing the Right Transition

### Decision Matrix

| Scenario | Recommended Transition | Reason |
|----------|----------------------|---------|
| Mobile app navigation | SlideOnTop | Standard pattern, familiar |
| Tablet/Desktop app | Push | More screen space, side-by-side |
| Creative/Design app | Reveal | Unique visual interest |
| Settings panel | SlideOnTop | Temporary overlay |
| Filter panel | Push or SlideOnTop | Depends on content importance |
| Notification panel | SlideOnTop | Quick overlay |

### Platform Considerations

**Mobile (Phone):**
```csharp
// Prefer SlideOnTop for limited screen space
new DrawerSettings
{
    Transition = Transition.SlideOnTop,
    DrawerWidth = 280,
    Duration = 250
};
```

**Tablet:**
```csharp
// Push works well with more space
new DrawerSettings
{
    Transition = Transition.Push,
    DrawerWidth = 350,
    Duration = 220
};
```

**Desktop:**
```csharp
// Push for professional desktop feel
new DrawerSettings
{
    Transition = Transition.Push,
    DrawerWidth = 400,
    Duration = 200,
    AnimationEasing = Easing.CubicOut
};
```

### Dynamic Transition Selection

```csharp
// Adjust based on device
Transition GetOptimalTransition()
{      
    if (DeviceInfo.Idiom == DeviceIdiom.Phone)
        return Transition.SlideOnTop;

    if (DeviceInfo.Idiom == DeviceIdiom.Tablet)
        return Transition.Push;

    if (DeviceInfo.Idiom == DeviceIdiom.Desktop)
        return Transition.Push;

    return Transition.SlideOnTop;

}

navigationDrawer.DrawerSettings = new DrawerSettings
{
    Transition = GetOptimalTransition(),
    DrawerWidth = 300
};
```

## Performance Considerations

### Optimize Animation Performance

**1. Use Appropriate Duration:**
```csharp
// Shorter animations = better perceived performance
drawerSettings.Duration = 220;  // Fast but not jarring
```

**2. Avoid Complex Content During Animation:**
```csharp
// Load heavy content AFTER drawer is open
private async void OnDrawerOpened(object sender, EventArgs e)
{
    await LoadHeavyDrawerContent();
}
```

**3. Pre-render Drawer Content:**
```csharp
// Initialize drawer content early
public MainPage()
{
    InitializeComponent();
    
    // Pre-create drawer content
    PreloadDrawerContent();
}
```

**4. Use Hardware Acceleration:**
```csharp
// Ensure views are hardware accelerated (automatic on modern .NET MAUI)
// Avoid unnecessary transparency
drawerSettings.ContentBackground = Colors.White;  // Solid color
```

### Performance by Transition Type

**SlideOnTop Performance:**
- Best overall performance
- No content reflow
- Recommended for complex UIs

**Push Performance:**
- Moderate performance impact
- Content layout recalculation
- May lag on low-end devices

**Reveal Performance:**
- Similar to SlideOnTop
- Content moves but doesn't reflow
- Good performance overall

### Testing Animations

```csharp
// Test on actual devices, not just simulator
// Profile on low-end devices
// Check frame rate during animation

// Example: Simple performance test
private DateTime _animationStart;

private void OnDrawerOpening(object sender, CancelEventArgs e)
{
    _animationStart = DateTime.Now;
}

private void OnDrawerOpened(object sender, EventArgs e)
{
    var elapsed = (DateTime.Now - _animationStart).TotalMilliseconds;
    System.Diagnostics.Debug.WriteLine($"Animation took: {elapsed}ms");
}
```

## Advanced Customization

### Combining Properties for Best Effect

```csharp
// Smooth, responsive drawer
navigationDrawer.DrawerSettings = new DrawerSettings
{
    Transition = Transition.SlideOnTop,
    Duration = 220,
    AnimationEasing = Easing.CubicOut,
    DrawerWidth = 280
};

// Dramatic reveal effect
navigationDrawer.DrawerSettings = new DrawerSettings
{
    Transition = Transition.Reveal,
    Duration = 350,
    AnimationEasing = Easing.SpringOut,
    DrawerWidth = 300
};

// Professional desktop style
navigationDrawer.DrawerSettings = new DrawerSettings
{
    Transition = Transition.Push,
    Duration = 200,
    AnimationEasing = Easing.SinInOut,
    DrawerWidth = 400
};
```

### Conditional Animation

```csharp
// Disable animation for accessibility
if (Preferences.Get("ReduceMotion", false))
{
    drawerSettings.Duration = 0;  // Instant transition
}
else
{
    drawerSettings.Duration = 250;
    drawerSettings.AnimationEasing = Easing.CubicOut;
}
```

### Animation Based on Drawer State

```csharp
// Different animations for open vs close
private bool _isOpening = true;

private void OnDrawerOpening(object sender, CancelEventArgs e)
{
    _isOpening = true;
    navigationDrawer.DrawerSettings.Duration = 250;
}

private void OnDrawerClosing(object sender, CancelEventArgs e)
{
    _isOpening = false;
    navigationDrawer.DrawerSettings.Duration = 200;  // Faster close
}
```

## Common Issues

### Issue: Animation feels sluggish
**Solution:** Reduce duration to 200-220ms and use CubicOut easing.

### Issue: Animation is too jarring
**Solution:** Increase duration to 280-300ms and use SinInOut easing.

### Issue: Content jumps during Push transition
**Solution:** Pre-calculate layout or use SlideOnTop instead.

### Issue: Liquid glass effect not showing
**Solution:** Ensure Transition is set to SlideOnTop (only supported mode).
