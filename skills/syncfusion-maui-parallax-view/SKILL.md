---
name: syncfusion-maui-parallax-view
description: Implements Syncfusion .NET MAUI Parallax View (SfParallaxView) to create parallax scrolling effects where background elements move slower than foreground content. Use when working with parallax effects, parallax scrolling, background scroll effects, or depth scrolling. Covers both simple image parallax and complex custom control implementations for ScrollView and ListView.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion .NET MAUI Parallax View

The Syncfusion .NET MAUI Parallax View (SfParallaxView) provides a visually engaging way to create depth and motion in your applications by scrolling background elements at a different speed than foreground elements. The control binds a scrollable foreground element (Source) to a background element (Content) and moves the background at a varying speed to create a parallax effect.

## When to Use This Skill

Use this skill when you need to:

- **Create parallax scrolling effects** where background images or views move slower than the foreground content
- **Add visual depth** to scrollable content like lists, articles, or image galleries
- **Implement hero sections** with parallax backgrounds for engaging user interfaces
- **Bind ScrollView or ListView** with background elements that respond to scroll events
- **Create custom scrollable controls** that support parallax effects using the IParallaxView interface
- **Build immersive UIs** with dynamic background animations tied to scroll position
- **Enhance user experience** with smooth parallax transitions in vertical or horizontal orientations

## Component Overview

**SfParallaxView** creates a layered scrolling experience with:
- **Content**: Background view (Image, StackLayout, or any view) that moves with parallax effect
- **Source**: Foreground scrollable view (ScrollView, ListView, or custom IParallaxView controls)
- **Speed**: Customizable scroll speed for the background (0.0 to 1.0)
- **Orientation**: Support for both vertical and horizontal parallax scrolling

The background Content automatically stretches to match the Source size, ensuring seamless visual integration.

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installing Syncfusion.Maui.ParallaxView NuGet package
- Namespace registration in XAML and C#
- Basic SfParallaxView initialization
- Setting the Content property with background views
- Supported content types (Image, StackLayout, Grid, etc.)
- Basic XAML and C# implementation examples

### Source Binding and Integration
📄 **Read:** [references/source-binding.md](references/source-binding.md)
- Understanding the Source property and IParallaxView interface
- Binding ScrollView to create basic parallax effects
- Integrating Syncfusion ListView with parallax backgrounds
- Complete examples with ViewModels and data binding
- Content auto-stretch behavior to match Source size
- Known issues and workarounds (Android image sizing)
- GitHub sample links for working examples

### Customization Options
📄 **Read:** [references/customization.md](references/customization.md)
- Speed property to control parallax scroll intensity
- Speed values and their visual effects (0.0 = no movement, 1.0 = same speed)
- Orientation property for vertical or horizontal parallax
- Matching Source control orientation for proper effect
- XAML and C# configuration examples
- Performance considerations for smooth scrolling

### Custom Controls Support
📄 **Read:** [references/custom-controls.md](references/custom-controls.md)
- Implementing IParallaxView interface for custom controls
- ScrollableContentSize property for total content size
- Scrolling event with ParallaxScrollingEventArgs
- ScrollX, ScrollY, and CanAnimate parameters
- Complete CustomListView implementation example
- MeasureOverride for dynamic size calculation
- Integration patterns for custom scrollable controls

