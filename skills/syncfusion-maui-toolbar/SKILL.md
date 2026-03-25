---
name: syncfusion-maui-toolbar
description: Implements Syncfusion .NET MAUI Toolbar (SfToolbar) for navigation bars and command bars. Use when working with toolbars, toolbar items, navigation bars, toolbar buttons, or action toolbars in .NET MAUI applications. This skill covers toolbar item configuration, icons, text, overflow behavior, orientation, selection modes, and toolbar customization.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Toolbar in .NET MAUI

Guide users on implementing the Syncfusion .NET MAUI Toolbar (SfToolbar) control - a customizable UI component that provides quick access to actions or commands through buttons, icons, or menus.

## When to Use This Skill

Use this skill when users need to:

- **Add a toolbar** to their .NET MAUI application for quick access to commands
- **Configure toolbar items** with icons, text, or custom views
- **Implement text formatting toolbars** (bold, italic, underline, alignment)
- **Create document editing toolbars** with overflow handling
- **Add navigation toolbars** with horizontal or vertical layouts
- **Configure selection modes** (single, multiple, toggle)
- **Handle toolbar events** (taps, selections, interactions)
- **Customize toolbar appearance** (colors, styles, corner radius)
- **Implement overlay toolbars** for contextual actions
- **Apply modern glass effects** to toolbars
- **Support keyboard navigation** in toolbars
- **Add tooltips** to toolbar items

## Component Overview

The Syncfusion .NET MAUI Toolbar (SfToolbar) is a feature-rich control that offers:

- **Flexible item display**: Icons, text, or custom views
- **Multiple orientations**: Horizontal and vertical layouts
- **Overflow handling**: Scroll, navigation buttons, more button, multi-row, extended row
- **Selection modes**: Single, single deselect, multiple selection
- **Rich customization**: Colors, styles, fonts, corner radius
- **Event support**: Taps, long press, touch interactions, selection changes
- **Overlay toolbars**: Contextual toolbars that appear on demand
- **Modern effects**: Liquid glass effect for sleek UI
- **Accessibility**: Tooltips, keyboard navigation, ARIA support

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

**When to read:** Installation, first-time setup, basic toolbar implementation

Topics covered:
- Installing Syncfusion.Maui.Toolbar NuGet package
- Registering Syncfusion core handler in MauiProgram.cs
- Creating your first toolbar with XAML and C#
- Adding basic toolbar items
- Configuring icons with FontImageSource
- Material icons font setup

### Toolbar Items and Display
📄 **Read:** [references/toolbar-items.md](references/toolbar-items.md)

**When to read:** Configuring toolbar items, icons, text, custom views, separators, alignment

Topics covered:
- Icon-only toolbar items
- Text-only toolbar items
- Icon with text combinations
- Icon sizing and text positioning
- Custom item views (buttons, checkboxes, entries)
- Separator items for visual organization
- Item sizing and spacing
- Leading, trailing, and center alignment
- Equal spacing configuration
- Clear selection programmatically
- Item naming for identification

### Layout and Overflow
📄 **Read:** [references/layout-and-overflow.md](references/layout-and-overflow.md)

**When to read:** Setting toolbar orientation, handling overflow items, configuring scroll/navigation modes

Topics covered:
- Horizontal toolbar layout
- Vertical toolbar layout
- Scroll mode for overflow items
- Navigation buttons for stepping through items
- More button with dropdown menu
- More button positioning (auto, left, right, top, bottom)
- Multi-row support for wrapping items
- Extended row with expand/collapse button
- Canceling default dropdown view
- Width and height considerations

### Selection and Interaction
📄 **Read:** [references/selection-and-interaction.md](references/selection-and-interaction.md)

**When to read:** Configuring selection behavior, enabling tooltips, handling user interactions

Topics covered:
- Single selection mode
- Single deselect mode (toggle selection)
- Multiple selection mode
- Enabling and disabling toolbar items
- Tooltip text configuration
- Tooltip customization (background, text style, position)
- Keyboard navigation support
- IsEnabled state management
- User interaction patterns

### Events and Commands
📄 **Read:** [references/events-and-commands.md](references/events-and-commands.md)

**When to read:** Handling toolbar events, implementing MVVM commands, responding to user actions

Topics covered:
- Tapped event and TappedCommand
- ItemTouchInteraction event for pointer actions
- ItemLongPressed event for long press gestures
- MoreButtonTapped event for overflow button
- MoreItemsChanged event for overflow items
- SelectionChanged event for selection tracking
- MVVM command patterns with ViewModel integration
- Event arguments and parameters
- Command execution and CanExecute

### Customization
📄 **Read:** [references/customization.md](references/customization.md)

**When to read:** Styling toolbar items, customizing buttons, changing colors, applying corner radius

Topics covered:
- Toolbar item customization (IsEnabled, TextStyle, Color)
- SelectionHighlightColor for selected items
- Separator customization (stroke color, thickness)
- Navigation button customization (colors, backgrounds)
- Navigation button DataTemplate customization
- More button customization (icon color, background)
- Divider line customization
- Toolbar corner radius
- Selection corner radius for selected items
- Theme integration

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)

**When to read:** Implementing overlay toolbars, applying glass effects, advanced styling

Topics covered:
- Overlay toolbar implementation
- Back icon customization in overlay
- BackIconAlignment (Start/End positioning)
- BackIconTemplate for custom back icons
- BackIconToolTipText configuration
- Liquid glass effect setup
- Transparent background for glass effect
- Toolbar grouping layout with glass effect
- Group spacing with separator stroke thickness
- Platform support (macOS 26+, iOS 26+, .NET 10)

