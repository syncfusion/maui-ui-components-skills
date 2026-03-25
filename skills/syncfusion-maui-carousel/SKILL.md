---
name: syncfusion-maui-carousel
description: Implements Syncfusion .NET MAUI Carousel (SfCarousel) for navigating through image data or content collections. Use when implementing carousel controls, populating carousel items, or configuring view modes (linear/default). Covers animations, load more functionality, UI virtualization, transformations, and swipe events.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing .NET MAUI Carousel

A comprehensive skill for implementing and customizing the Syncfusion .NET MAUI Carousel (SfCarousel) control. The carousel allows users to navigate through image data or content in an interactive way with various customization options for item arrangements.

## When to Use This Skill

Use this skill when users need to:
- Implement a carousel/slideshow component for .NET MAUI applications
- Navigate through collections of images, cards, or custom content
- Create interactive image galleries or product showcases
- Display content with linear or default (3D) view arrangements
- Implement swipe gestures for content navigation
- Add load more functionality for dynamic content loading
- Optimize performance with UI virtualization for large datasets
- Customize carousel animations and transitions
- Handle carousel selection and swipe events

## Component Overview

The **SfCarousel** control provides:
- **Multiple View Modes**: Linear arrangement or Default (3D perspective) layout
- **Flexible Data Population**: Through data binding or direct item creation
- **Animation Control**: Customizable transition duration and effects
- **Load More Support**: Dynamically load additional items on demand
- **UI Virtualization**: Performance optimization for large item collections
- **Rich Events**: Selection changed, swipe started/ended, swiping events
- **Customization**: Item templates, rotation angles, spacing, transformations

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and NuGet package setup
- Handler registration in MauiProgram.cs
- Basic carousel implementation with minimal code
- Adding carousel items (through binding or direct)
- Setting item dimensions (ItemHeight, ItemWidth)
- Configuring SelectedIndex to show desired item

### Populating Data
📄 **Read:** [references/populating-data.md](references/populating-data.md)
- Data binding with ItemsSource and ItemTemplate
- Creating models and ViewModels for carousel data
- Custom DataTemplate for carousel items
- Using SfCarouselItem with ItemContent property
- Using ImageName property for simple image carousels
- ObservableCollection for dynamic data

### View Modes and Layout
📄 **Read:** [references/view-modes.md](references/view-modes.md)
- Linear arrangement (ViewMode.Linear) for stacked layout
- Default arrangement (ViewMode.Default) for 3D perspective
- Offset property for spacing between unselected items
- Rotation angle for visual effects
- ItemSpacing configuration

### Animation
📄 **Read:** [references/animation.md](references/animation.md)
- Duration property for transition timing
- Controlling animation speed (milliseconds)
- Smooth vs fast transitions
- Default animation behavior (600ms)

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- Load More functionality (AllowLoadMore, LoadMoreItemsCount)
- Customizing LoadMoreView appearance
- UI Virtualization (EnableVirtualization) for performance
- Transformation effects (rotation, scale)
- Performance optimization techniques

### Swipe Events
📄 **Read:** [references/swipe-events.md](references/swipe-events.md)
- SwipeStarted event handling
- SwipeEnded event for post-swipe actions
- Swiping event for during-swipe feedback
- Custom swipe behavior implementation
- Event argument properties

### How-To
📄 **Read:** [references/howto.md](references/howto.md)
- Performing operations during item changes
- SelectionChanged event usage
- Best practices and tips

