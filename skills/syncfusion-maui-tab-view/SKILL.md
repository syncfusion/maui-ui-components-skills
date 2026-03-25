---
name: syncfusion-maui-tab-view
description: Implements Syncfusion .NET MAUI Tab View (SfTabView) controls for tab navigation interfaces and tabbed content layouts. Use when working with tab controls, tab navigation, nested tabs, or swipeable content in .NET MAUI applications. This skill covers tab bar customization, tab item configuration, swipe gestures, events, and visual customization including indicators and Liquid Glass effects.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion .NET MAUI Tab View

The .NET MAUI Tab View (SfTabView) is a control that provides an advanced tab-navigation interface for mobile and desktop applications, offering customizable tab bars, nested tabs, swipe gestures, and extensive visual customization options.

## When to Use This Skill

Use this skill when the user needs to:

- Implement tabbed navigation interfaces in .NET MAUI applications
- Create tab views with scrollable or fixed tab bars
- Configure tab items with custom headers, icons, and content
- Build nested tab structures with multiple navigation levels
- Add swipe gestures for mobile-friendly tab navigation
- Handle tab selection events and implement custom navigation logic
- Customize tab bar appearance (placement, height, indicators, backgrounds)
- Style individual tab items with templates and visual states
- Implement data-bound tab views using ItemsSource
- Apply Liquid Glass effects or custom visual themes
- Build complex layouts requiring multiple content sections

## Component Overview

The Syncfusion .NET MAUI Tab View (SfTabView) is an advanced tab navigation control that provides:

**Core Features:**
- Fixed and scrollable tab bars with multiple width modes
- Rich header customization with text, icons, and custom templates
- Nested tab support with independent header placements
- ItemsSource binding with DataTemplate support
- Swipe gestures for mobile navigation
- Comprehensive event system for selection tracking
- Visual state management and indicator customization
- Liquid Glass theming support

**Key Use Cases:**
- Multi-section content organization
- Mobile app navigation patterns
- Dashboard layouts with categorized views
- Settings and configuration interfaces
- Content exploration with quick tab switching

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Registering handler in MauiProgram.cs
- Basic SfTabView initialization (XAML and C#)
- Populating tabs using Items collection
- Data binding with ItemsSource
- HeaderItemTemplate and ContentItemTemplate
- DataTemplateSelector for conditional templates
- Quick start code examples

### Tab Bar Configuration
📄 **Read:** [references/tab-bar-configuration.md](references/tab-bar-configuration.md)
- TabWidthMode options (Default vs SizeToContent)
- Fixed vs scrollable tab bars
- TabBarHeight customization
- TabBarPlacement (Top or Bottom or Left or Right)
- TabBarBackground styling
- Indicator configuration (color, placement, thickness)
- Tab spacing and visual layout

### Tab Item Configuration
📄 **Read:** [references/tab-item-configuration.md](references/tab-item-configuration.md)
- Header content with text and icons
- ImagePosition for icon placement (Left, Top, Right, Bottom)
- Font and text styling (FontSize, FontFamily, FontAttributes, TextColor)
- Badge and notification indicators with BadgeText and BadgeSettings
- Badge types (Error, Warning, Success, Information, custom)
- Content configuration with any .NET MAUI view
- Disabled state styling with IsEnabled
- Per-item customization and dynamic updates

### Nested Tabs
📄 **Read:** [references/nested-tabs.md](references/nested-tabs.md)
- Creating nested SfTabView structures
- Different header placements per level
- Navigation between nested tabs
- Layout patterns and best practices
- Multi-level tab hierarchies
- Complete nested examples

### Events and Interaction
📄 **Read:** [references/events-interaction.md](references/events-interaction.md)
- TabItemTapped event handling
- SelectionChanging event (cancellable)
- SelectionChanged event
- Event arguments and properties
- Programmatic selection with SelectedIndex
- Preventing tab selection
- Custom navigation logic

### Swiping Gestures
📄 **Read:** [references/swiping-gestures.md](references/swiping-gestures.md)
- Enabling swipe navigation
- EnableSwiping property
- Touch gesture configuration
- Platform-specific behavior
- Accessibility considerations
- Mobile-optimized patterns

### Visual Customization
📄 **Read:** [references/visual-customization.md](references/visual-customization.md)
- Visual state managers
- Selection indicator customization
- IndicatorBackground and placement options
- IndicatorWidthMode configurations
- Custom indicator views
- Liquid Glass effect integration
- Color schemes and themes
- Animations and transitions

### How-To Guide
📄 **Read:** [references/how-to.md](references/how-to.md)
- Programmatic tab selection with SelectedIndex
- Badge and notification implementation patterns
- Dynamic badge updates and data binding
- Disable hover effect on desktop
- Conditional tab visibility based on logic
- Custom animation duration control
- Tab accessibility with font auto-scaling
- Scroll button customization
- Common patterns (wizard/stepper, dashboard)

