---
name: syncfusion-maui-cards
description: Implements Syncfusion .NET MAUI Cards (SfCards) - dismissible card views and swipeable card stacks. Use when working with cards, card views, card layouts, swipeable cards, dismissible cards, or card stacks in .NET MAUI. Covers card customization, card events, card data binding, and interactive card components with swipe functionality.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing .NET MAUI Cards (SfCards)

The Syncfusion .NET MAUI Cards control, which provides both dismissible single card views (`SfCardView`) and swipeable card stacks (`SfCardLayout`). This skill covers installation, card creation, swipe gestures, customization, data binding, events, and modern visual effects.

## When to Use This Skill

Use this skill when the user needs to:

- **Create dismissible card views** - Single cards that can be swiped away (left/right)
- **Build card stacks** - Multiple stacked cards with swipe navigation
- **Implement swipe gestures** - Cards that respond to swipe in four directions (left, right, top, bottom)
- **Display card-based UI** - Visual card containers for content organization
- **Add interactive cards** - Cards with tap events, dismiss events, and state management
- **Bind data to cards** - Dynamically generate cards from data collections using BindableLayout
- **Customize card appearance** - Borders, corners, indicators, colors, and glass effects
- **Handle card events** - Tapped, dismissing, dismissed, index changing events

This skill is specifically for Syncfusion's .NET MAUI Cards control (SfCardView and SfCardLayout), not generic card layouts or other card libraries.

## Component Overview

The Syncfusion .NET MAUI Cards control provides two main components:

1. **SfCardView** - A single card that can optionally be dismissed by swiping
2. **SfCardLayout** - A container for stacking multiple SfCardView items with swipe navigation

**Key Capabilities:**
- Swipe-to-dismiss functionality
- Multi-directional swipe support (left, right, top, bottom)
- Visual customization (borders, corners, indicators)
- Programmatic card dismissal
- Data binding with BindableLayout
- Comprehensive event handling
- Modern liquid glass effect (.NET 10+)

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

Read this reference when the user needs to:
- Install the Syncfusion.Maui.Cards NuGet package
- Register Syncfusion handlers in MauiProgram.cs
- Create their first SfCardView
- Understand basic XAML and C# implementation patterns

### Card View Features
📄 **Read:** [references/card-views.md](references/card-views.md)

Read this reference when the user works with single cards and needs:
- SfCardView component overview and usage
- SwipeToDismiss property for swipe-away functionality
- IsDismissed property for programmatic dismissal
- FadeOutOnSwiping visual effect
- Single card implementation patterns
- Standalone card scenarios

### Card Layout Features
📄 **Read:** [references/card-layouts.md](references/card-layouts.md)

Read this reference when the user needs card stacks with:
- SfCardLayout component (multiple stacked cards)
- ShowSwipedCard property for edge display
- VisibleIndex property for card navigation
- SwipeDirection configuration (Left, Right, Top, Bottom)
- Multiple card management
- Swipe gesture navigation between cards

### Customization and Styling
📄 **Read:** [references/customization.md](references/customization.md)

Read this reference when the user wants to customize:
- BorderColor, BorderWidth, CornerRadius properties
- Background colors and gradients
- Indicator customization (color, thickness, position)
- FadeOutOnSwiping effect
- Advanced visual styling
- Custom card designs and themes

### Data Binding with BindableLayout
📄 **Read:** [references/data-binding.md](references/data-binding.md)

Read this reference when the user needs to:
- Generate cards dynamically from data collections
- Use BindableLayout with SfCardLayout
- Set up ViewModels and data sources
- Configure ItemsSource and ItemTemplate
- Create data-driven card interfaces
- Bind card properties to data models

### Events and Interactions
📄 **Read:** [references/events.md](references/events.md)

Read this reference when the user needs to handle:
- Tapped event (card tap detection)
- VisibleIndexChanging event (before card changes, with Cancel support)
- VisibleIndexChanged event (after card changes)
- Dismissing event (before dismiss, with Cancel support)
- Dismissed event (after dismiss completes)
- Event handler implementation and scenarios

### Liquid Glass Effect
📄 **Read:** [references/liquid-glass-effect.md](references/liquid-glass-effect.md)

Read this reference when the user wants:
- Modern translucent glass effect
- EnableLiquidGlassEffect property usage
- Platform requirements (macOS 26+, iOS 26+, .NET 10)
- Background configuration for glass appearance
- Sleek, modern card designs

