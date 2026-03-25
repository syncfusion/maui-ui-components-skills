---
name: syncfusion-maui-radial-menu
description: Implements Syncfusion .NET MAUI Radial Menu (SfRadialMenu) - a hierarchical circular menu optimized for touch. Use when working with radial menus, circular menus, context menus, touch-optimized menus, or hierarchical circular navigation. Covers floating menus, drag-enabled menus, nested circular menus, and custom segmented layouts in .NET MAUI applications.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Radial Menus in .NET MAUI

The Syncfusion .NET MAUI Radial Menu (SfRadialMenu) displays a hierarchical menu in a circular layout, optimized for touch-enabled devices. It's typically used as a context menu and can expose more menu items in the same space than traditional menus.

## When to Use This Skill

Use this skill when the user needs to:

- **Implement circular/radial menus** in .NET MAUI applications
- **Create touch-optimized context menus** with hierarchical navigation
- **Build floating menus** that can be dragged across the screen
- **Design nested menu systems** with multiple levels of items
- **Customize center buttons** with text, icons, or custom views
- **Segment menu layouts** with custom positioning and indexing
- **Add interactive menu items** with images, icons, or custom content
- **Handle menu events** for navigation, opening/closing, and item tapping
- **Implement modern UI effects** like liquid glass/acrylic styling

## Component Overview

**Key Features:**
- **Hierarchical Navigation:** Multiple levels of nested menu items
- **Touch-Optimized:** Circular layout designed for touch interaction
- **Draggable:** Float over content and drag anywhere on screen
- **Rotation Support:** Items can rotate around the center
- **Custom Segmentation:** Control item placement and spacing
- **Font Icons:** Built-in support for vector icons
- **Custom Views:** Use any MAUI view as menu item content
- **Rich Customization:** Colors, sizes, fonts, backgrounds, and more
- **Event-Driven:** Comprehensive event system for all interactions

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- NuGet package installation (Syncfusion.Maui.RadialMenu)
- Registering handlers in MauiProgram.cs
- Basic SfRadialMenu setup
- Creating your first radial menu with items
- XAML and C# implementation examples

### Populating Items
📄 **Read:** [references/populating-items.md](references/populating-items.md)
- Adding items through RadialMenuItem collection
- Text-only items, images with text, custom font icons
- Creating nested/hierarchical menu structures
- Using ItemsSource and ItemTemplate for data binding
- Animation duration, rim styling, separators
- DisplayMemberPath for property binding

### Center Button Customization
📄 **Read:** [references/center-button-customization.md](references/center-button-customization.md)
- Center button text and back button text
- Colors (text, background, stroke)
- Size and radius customization
- Font family, size, and attributes
- Custom views for center button
- Animation and auto-scaling
- Start angle configuration

### Menu Item Customization
📄 **Read:** [references/menu-item-customization.md](references/menu-item-customization.md)
- SfRadialMenuItem properties
- Text, images, and font icons
- Font customization (family, size, attributes)
- Colors (background, text)
- Size (ItemHeight, ItemWidth)
- Custom views for menu items
- Command binding
- Enable/disable states

### Layout and Segmentation
📄 **Read:** [references/layout-segmentation.md](references/layout-segmentation.md)
- Default layout (sequential arrangement)
- Custom layout (indexed positioning)
- VisibleSegmentsCount property
- SegmentIndex for precise placement
- Use cases for different layout types

### Dragging and Rotation
📄 **Read:** [references/dragging-rotation.md](references/dragging-rotation.md)
- Enabling drag functionality (IsDragEnabled)
- Rotation control (EnableRotation)
- Drag events (DragBegin)
- Restricting drag behavior
- Positioning the menu on parent layout

### Events
📄 **Read:** [references/events.md](references/events.md)
- Navigation events (Navigating, Navigated)
- Opening/Closing events
- Center button events (CenterButtonBackTapped)
- Item interaction events (ItemTapped, TouchDown, TouchUP)
- Event arguments and cancellation

