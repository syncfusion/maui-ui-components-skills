---
name: syncfusion-maui-pull-to-refresh
description: Implements the Syncfusion .NET MAUI PullToRefresh (SfPullToRefresh) control for interactive content refreshing. Use when working with pull-to-refresh, refresh controls, swipe to refresh, or updating content on pull gestures. Covers scenarios for refreshing ListView, DataGrid, or any scrollable content, customizing refresh indicators, MVVM refresh patterns, and pull-to-refresh events.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing .NET MAUI PullToRefresh (SfPullToRefresh)

The Syncfusion .NET MAUI PullToRefresh control enables interactive content refreshing through pull-down gestures. It displays a progress indicator during the pull action and triggers content updates when the user releases after pulling beyond a threshold distance.

## When to Use This Skill

Use this skill when you need to:

- **Implement pull-to-refresh functionality** in .NET MAUI applications for refreshing content on demand
- **Add interactive refresh controls** to scrollable views like ListView, DataGrid, or custom layouts
- **Display refresh indicators** with progress animations during pull-to-refresh actions
- **Customize refresh appearance** including colors, sizes, transition modes, and thresholds
- **Create custom refresh templates** with progress bars or animated views
- **Integrate MVVM patterns** with refresh commands and data binding
- **Handle refresh events** for controlling pulling, refreshing, and completion actions
- **Host complex views** within pullable content areas with proper refresh behavior

## Component Overview

- Interactive pull-to-refresh gesture recognition
- Two transition modes: SlideOnTop and Push
- Customizable progress indicator appearance
- Support for hosting any .NET MAUI view as pullable content
- MVVM-compatible with RefreshCommand and data binding
- Custom templates for pulling and refreshing views
- Built-in events for fine-grained refresh control
- Programmatic refresh initiation and completion


## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

When to read: First-time setup, installation, basic implementation
- Installing Syncfusion.Maui.PullToRefresh NuGet package
- Registering Syncfusion handler (ConfigureSyncfusionCore)
- Creating basic SfPullToRefresh instance
- Defining PullableContent
- Handling IsRefreshing property
- Running your first pull-to-refresh application

### Pullable Content
📄 **Read:** [references/pullable-content.md](references/pullable-content.md)

When to read: Configuring content that responds to pull gestures
- Understanding PullableContent property
- Hosting .NET MAUI DataGrid in PullToRefresh
- Hosting .NET MAUI ListView in PullToRefresh
- Loading custom views and layouts
- Best practices for pullable content configuration
- Handling content refresh with different views

### Customization and Properties
📄 **Read:** [references/customization.md](references/customization.md)

When to read: Customizing appearance, behavior, thresholds, and transition modes
- TransitionMode: SlideOnTop vs Push animations
- PullingThreshold and RefreshViewThreshold configuration
- IsRefreshing property for refresh state control
- Progress indicator customization (color, thickness, size)
- RefreshViewWidth and RefreshViewHeight properties
- ProgressBackground, ProgressColor, ProgressThickness
- Programmatic methods: StartRefreshing() and EndRefreshing()
- Size and layout considerations

### Custom Templates
📄 **Read:** [references/templates.md](references/templates.md)

When to read: Creating custom pulling and refreshing views
- PullingViewTemplate property
- RefreshingViewTemplate property
- Using SfCircularProgressBar in templates
- Creating DataTemplate for custom progress indicators
- Integrating templates with Pulling event
- Animating refresh views
- Template customization examples

### MVVM and Commands
📄 **Read:** [references/mvvm-commands.md](references/mvvm-commands.md)

When to read: Implementing MVVM pattern with PullToRefresh
- MVVM compatibility overview
- Binding IsRefreshing to ViewModel properties
- Using RefreshCommand and RefreshCommandParameter
- CanExecute() method for controlling refresh actions
- INotifyPropertyChanged implementation
- Complete ViewModel examples
- Canceling pull actions from ViewModel

### Events
📄 **Read:** [references/events.md](references/events.md)

When to read: Handling pull-to-refresh lifecycle events
- Pulling event with PullingEventArgs
- Refreshing event on pointer release
- Refreshed event on completion
- Cancel property for preventing refresh
- Progress property for tracking pull distance
- Event handling patterns and examples

