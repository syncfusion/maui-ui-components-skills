---
name: syncfusion-maui-docklayout
description: Implements Syncfusion .NET MAUI DockLayout (SfDockLayout) for arranging and docking child elements in defined areas. Use this for creating docking layouts, positioning UI elements at edges (top/bottom/left/right), building dashboard layouts, or arranging multi-region interfaces. Covers dock positioning, edge positioning, and spacing between docked elements.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing .NET MAUI DockLayout

Guide users to implement Syncfusion .NET MAUI DockLayout (SfDockLayout), a versatile layout control that enables developers to arrange and dock child elements to specific edges (top, bottom, left, right) or center, providing a robust framework for designing complex user interfaces in mobile and desktop applications.

## When to Use This Skill

Use this skill when users need to:
- **Create docking layouts** with elements positioned at specific edges
- **Build dashboard interfaces** with multiple regions (header, footer, sidebar, content)
- **Implement adaptive layouts** that respond to screen sizes and orientations
- **Position UI elements** at top, bottom, left, right, or center of a container
- **Configure spacing** between docked elements (horizontal and vertical)
- **Control last child expansion** to fill remaining space
- **Support Right-to-Left (RTL)** layouts for internationalization
- **Work with .NET MAUI** applications using Syncfusion components

## Component Overview

The **SfDockLayout** control provides:
- **Flexible Docking**: Dock children to Top, Bottom, Left, Right, or None (center)
- **Adaptive Layouts**: Automatically adapts to various screen sizes and orientations
- **Customizable Spacing**: Configure horizontal and vertical spacing between elements
- **Last Child Expansion**: Option to expand the last child to fill remaining space
- **RTL Support**: Full Right-to-Left layout support for internationalization
- **Programmatic Control**: GetDock() and SetDock() methods for runtime manipulation

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installing Syncfusion.Maui.Core NuGet package
- Registering Syncfusion Core handler
- Creating and initializing SfDockLayout
- Setting basic dock positions for child elements
- Complete working example with all dock positions

### Docking Features
📄 **Read:** [references/docking-features.md](references/docking-features.md)
- Dock position options (Top, Bottom, Left, Right, None)
- Using Dock attached property in XAML
- Using Add() method with Dock parameter in C#
- GetDock() method to retrieve current position
- SetDock() method to change position programmatically
- Docking order and element layering

### Spacing and Layout
📄 **Read:** [references/spacing-layout.md](references/spacing-layout.md)
- HorizontalSpacing property for gaps between elements
- VerticalSpacing property for vertical gaps
- ShouldExpandLastChild property behavior
- Controlling last child expansion (true/false)
- Layout scenarios and best practices

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- Right-to-Left (RTL) layout support
- FlowDirection property configuration
- Adaptive layouts for different screen sizes
- Handling orientation changes
- Sample projects and resources

