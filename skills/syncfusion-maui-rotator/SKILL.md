---
name: syncfusion-maui-rotator
description: Implements Syncfusion .NET MAUI Rotator (SfRotator) control for displaying image carousels, slideshows, or rotating content. Use this for image galleries, product showcases, testimonial sliders, banner rotators, or scenarios requiring automatic or manual navigation through visual content.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing .NET MAUI Rotator

The Syncfusion .NET MAUI Rotator (SfRotator) is a data control for displaying and navigating through image content. It supports thumbnail and dots navigation modes, autoplay functionality, looping, and extensive customization options.

## When to Use This Skill

Use this skill when the user needs to:

- Implement image carousels or slideshows in .NET MAUI applications
- Create product galleries with thumbnail or dot navigation
- Build testimonial sliders or promotional banners
- Display rotating content with automatic advancement
- Create image viewers with swipe navigation
- Implement content rotators with custom navigation controls
- Build galleries that support both horizontal and vertical navigation
- Create slideshows with customizable timing and looping behavior

## Component Overview

The SfRotator control provides:

- **Multiple Navigation Modes:** Thumbnail (image previews) and Dots (indicators)
- **Data Binding:** Supports IList, ObservableCollection, and SfRotatorItem collections
- **Autoplay & Looping:** Automatic advancement with configurable delays and infinite looping
- **Directional Navigation:** Six navigation directions including horizontal, vertical, and unidirectional modes
- **Extensive Customization:** Custom colors, strokes, templates, and navigation button styling
- **Interactive Controls:** Swipe gestures, navigation buttons, and programmatic index selection
- **Event Handling:** SelectedIndexChanged event for responding to navigation actions

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installing Syncfusion.Maui.Rotator NuGet package
- Registering the handler (ConfigureSyncfusionCore)
- Basic XAML and C# implementation
- First working example with images

### Data and Content
📄 **Read:** [references/populating-data.md](references/populating-data.md)
- Creating data models and ViewModels
- Binding ItemsSource to collections (IList, ObservableCollection)
- Configuring ItemTemplate with DataTemplate
- Using SfRotatorItem for simple scenarios
- Loading online images from URLs

### Navigation Modes and Styling
📄 **Read:** [references/navigation-modes.md](references/navigation-modes.md)
- NavigationStripMode (Thumbnail, Dots)
- Dots customization (stroke, selected/unselected colors)
- Thumbnail customization (stroke colors)
- Navigation button styling (icon color, background)
- Showing/hiding navigation buttons
- DotPlacement options (Default, None, Outside)

### Position and Direction
📄 **Read:** [references/placement-and-direction.md](references/placement-and-direction.md)
- NavigationStripPosition (Top, Bottom, Left, Right)
- NavigationDirection (Horizontal, Vertical, LeftToRight, RightToLeft, TopToBottom, BottomToTop)
- Combining position with direction
- Layout best practices

### Autoplay and User Interaction
📄 **Read:** [references/autoplay-and-interaction.md](references/autoplay-and-interaction.md)
- EnableAutoPlay for automatic advancement
- NavigationDelay timing configuration
- EnableLooping for infinite scrolling
- EnableSwiping to control user interaction
- SelectedIndex for programmatic navigation

### Advanced Customization
📄 **Read:** [references/customization.md](references/customization.md)
- DataTemplateSelector for dynamic templates
- IsTextVisible for text panel display
- Custom layouts and styling
- Background colors and sizing
- Advanced template scenarios

### Events and Migration
📄 **Read:** [references/events.md](references/events.md)
- SelectedIndexChanged event handling
- Responding to navigation changes
- Event patterns in XAML and C#
- API changes and breaking changes

