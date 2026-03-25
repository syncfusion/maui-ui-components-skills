---
name: syncfusion-maui-rating
description: Implements Syncfusion .NET MAUI Rating (SfRating) control. Use when implementing star ratings, review systems, feedback mechanisms, or product ratings in MAUI apps. Covers rating control configuration, star rating, half-star rating, custom rating shapes, rating precision, and rating events.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing .NET MAUI Rating

A comprehensive skill for implementing the Syncfusion .NET MAUI Rating control. The `SfRating` control provides a flexible and customizable rating interface with support for various precision modes, shapes, and styling options.

## When to Use This Skill

Use this skill when users need to:
- Implement star ratings or review systems in MAUI applications
- Create product rating displays (e.g., e-commerce apps)
- Build feedback mechanisms for user experiences
- Display or collect ratings with custom precision (full, half, exact)
- Use custom rating shapes (heart, diamond, circle, or custom paths)
- Style rating controls with custom colors and strokes
- Handle rating value changes through events
- Create read-only rating displays
- Implement any rating UI component in .NET MAUI

## Component Overview

The `SfRating` control is part of the `Syncfusion.Maui.Inputs` package and provides:

- **Flexible Precision**: Standard (full), Half (half-step), or Exact (precise) rating modes
- **Multiple Shapes**: Star, Heart, Diamond, Circle, or custom SVG paths
- **Rich Customization**: Colors, strokes, sizes, spacing, and styling
- **Interactive & Display Modes**: Editable or read-only ratings
- **Event Support**: ValueChanged event for handling rating changes
- **Easy Integration**: Simple XAML and C# APIs

**Common Use Cases:**
- Product reviews (1-5 stars)
- Movie/content ratings
- User feedback forms
- Service quality ratings
- Skill level indicators
- Preference selections

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and NuGet package setup (`Syncfusion.Maui.Inputs`)
- Handler registration in `MauiProgram.cs`
- Basic `SfRating` implementation in XAML and C#
- Setting `ItemCount` and `Value` properties
- Namespace imports and initial setup
- Simple rating example

### Precision and Accuracy
📄 **Read:** [references/precision-modes.md](references/precision-modes.md)
- **Standard Precision**: Full item rating (whole stars)
- **Half Precision**: Half-step rating (half stars)
- **Exact Precision**: Precise decimal rating
- Choosing the right precision mode
- Use cases for each precision type
- Code examples for all modes

### Visual Customization
📄 **Read:** [references/rating-shapes.md](references/rating-shapes.md)
- Predefined shapes (Star, Heart, Diamond, Circle)
- Custom shapes with SVG path data
- Path property and sizing guidelines

📄 **Read:** [references/appearance-styling.md](references/appearance-styling.md)
- Item size and spacing configuration
- Fill colors (RatedFill, UnratedFill)
- Stroke colors and thickness
- RatingSettings comprehensive guide
- Design best practices

### Interactive Features
📄 **Read:** [references/interactive-features.md](references/interactive-features.md)
- IsReadOnly property for display-only ratings
- ValueChanged event handling
- User interaction control
- Event data and scenarios
- Data binding with events

