---
name: syncfusion-maui-navigation-drawer
description: Implements Syncfusion .NET MAUI Navigation Drawer (SfNavigationDrawer) component. Use when working with navigation drawers, sliding drawers, side menus, hamburger menus, or drawer panels in .NET MAUI apps. Covers drawer positioning (left/right/top/bottom), animations (SlideOnTop/Push/Reveal), content management, swipe gestures, and events.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Navigation Drawers

The .NET MAUI Navigation Drawer (SfNavigationDrawer) is a component for creating navigation panes with sliding drawer functionality. It consists of a main content area and a sliding pane that extends from any edge of the screen. The drawer can be opened via swipe gestures or programmatically, supporting multiple positions, animations, and customization options.

## When to Use This Skill

Use this skill when the user needs to:

- **Navigation menus** - Side panels for app navigation with menu items
- **Hamburger menus** - Classic mobile navigation pattern with slide-out drawer
- **Drawer-based layouts** - Apps requiring slide-out panels from any screen edge
- **Multi-drawer UIs** - Applications with drawers on multiple sides (left and right)
- **Swipe navigation** - Touch-based drawer interaction with gesture support
- **Animated transitions** - Smooth drawer animations (SlideOnTop, Push, Reveal)
- **Context panels** - Secondary content areas that slide in/out on demand

## Component Overview

The **SfNavigationDrawer** control provides:
- Four-directional positioning (Left, Right, Top, Bottom)
- Three animation types (SlideOnTop, Push, Reveal)
- Content areas: Drawer header, Drawer footer, Drawer content, Main Content
- Programmatic and gesture-based control
- Event-driven architecture for drawer state
- Multi-drawer support (primary + secondary)
- Customizable sizing and backgrounds
- Swipe gesture configuration
- Liquid glass effect (iOS 26+, macOS 26+, .NET 10+)

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

When to read: User is setting up NavigationDrawer for the first time or needs installation guidance.
- NuGet package installation (Syncfusion.Maui.NavigationDrawer)
- Registering Syncfusion handler (.ConfigureSyncfusionCore())
- Basic drawer implementation with ContentView
- Creating hamburger menu toggle
- First working example with drawer content

### Drawer Positioning and Sizing
📄 **Read:** [references/drawer-positioning.md](references/drawer-positioning.md)

When to read: User needs to position drawer on specific side or adjust drawer dimensions.
- Position property (Left, Right, Top, Bottom)
- DrawerWidth for left/right positioning
- DrawerHeight for top/bottom positioning
- Side-specific configuration examples
- Sizing best practices

### Content Management
📄 **Read:** [references/content-management.md](references/content-management.md)

When to read: User needs to set up drawer content areas, headers, footers, or main content.
- ContentView (main content area - mandatory)
- DrawerHeaderView configuration
- DrawerContentView (navigation menu content)
- DrawerFooterView setup
- Header/footer height customization
- ContentBackground styling
- Using CollectionView as drawer content
- Dynamic content switching patterns

### Animations and Transitions
📄 **Read:** [references/animations-transitions.md](references/animations-transitions.md)

When to read: User wants to customize drawer opening/closing animations or effects.
- Transition types (SlideOnTop, Push, Reveal)
- Animation differences and use cases
- Duration property for animation speed
- AnimationEasing customization
- Performance considerations
- Visual comparison of animation types

### Toggling and Drawer Control
📄 **Read:** [references/toggling-drawer.md](references/toggling-drawer.md)

When to read: User needs to open/close drawer programmatically or control drawer state.
- ToggleDrawer() method
- IsOpen property for state management
- Programmatic open/close operations
- Setting initial drawer state
- Closing drawer on menu item selection
- Integration with hamburger button

### Event Handling
📄 **Read:** [references/events.md](references/events.md)

When to read: User needs to respond to drawer state changes or control drawer behavior.
- DrawerOpening event (with cancellation)
- DrawerOpened event
- DrawerClosing event (with cancellation)
- DrawerClosed event
- DrawerToggled event
- Event arguments and usage patterns
- Preventing drawer open/close with Cancel property

### Swipe Gestures
📄 **Read:** [references/swipe-gestures.md](references/swipe-gestures.md)

When to read: User wants to enable/disable swipe gestures or adjust touch sensitivity.
- EnableSwipeGesture property
- TouchThreshold for swipe region
- Swipe-to-open behavior
- Swipe-to-close behavior
- Disabling gestures when needed
- Adjusting sensitivity for different devices

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)

When to read: User needs multi-drawer setup, liquid glass effects, or complex drawer scenarios.
- Multi-drawer implementation (DrawerSettings + SecondaryDrawerSettings)
- ToggleSecondaryDrawer() method
- Opening multiple drawers on different sides
- Liquid glass effect (EnableLiquidGlassEffect)
- LiquidGlassSettings for iOS 26+/macOS 26+
- Complex navigation patterns
- Performance optimization techniques

