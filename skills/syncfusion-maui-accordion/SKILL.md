---
name: syncfusion-maui-accordion
description: Implements Syncfusion .NET MAUI Accordion (SfAccordion) - a vertically collapsible panel with stacked headers for expanding/collapsing content. Use when working with accordions, collapsible panels, expandable lists, employee lists with details, or FAQ sections in .NET MAUI applications. Covers AccordionItem configuration, expand modes, and grouped content display.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion .NET MAUI Accordion

A comprehensive guide for implementing the Syncfusion .NET MAUI Accordion (SfAccordion) - a vertically collapsible panel control that allows users to expand or collapse one or multiple items simultaneously.

## When to Use This Skill

Use this skill when you need to:
- Implement collapsible/expandable content sections in .NET MAUI
- Display employee directories, contact lists, or detailed records
- Create FAQ sections with expandable answers
- Build settings panels with grouped options
- Show hierarchical data with expand/collapse behavior
- Implement vertical expander panels
- Bind accordion items to ObservableCollection data sources
- Customize accordion appearance with themes and Visual States
- Handle expansion/collapse events with validation

## Component Overview

The .NET MAUI Accordion provides a vertically stacked interface where each item consists of a header and content section. Users can tap headers to expand/collapse content, making it ideal for displaying hierarchical information, FAQs, employee directories, or any scenario requiring space-efficient grouped content display.

- Single or multiple item expansion modes
- Data binding with .NET MAUI BindableLayout
- Customizable animations (duration and easing)
- Rich appearance customization (colors, icons, Visual States)
- Event handling for expand/collapse actions
- Liquid Glass Effect support (iOS 26+, macOS 26+)
- Programmatic scroll control

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation of Syncfusion.Maui.Expander NuGet package
- Handler registration with ConfigureSyncfusionCore()
- Basic SfAccordion setup in XAML and C#
- Creating AccordionItem with Header and Content
- Expand modes: Single, Multiple, MultipleOrNone
- Animation duration and easing customization
- Item spacing configuration
- Auto scroll position settings
- BringIntoView method for programmatic scrolling

### Data Binding with BindableLayout
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- Creating data models with INotifyPropertyChanged
- Setting up ObservableCollection for accordion items
- Configuring BindingContext
- Using BindableLayout.ItemsSource for data binding
- Defining BindableLayout.ItemTemplate for item appearance
- Two-way binding with IsExpanded property
- Dynamic item generation from collections
- Complete employee list example with data binding

### Appearance Customization
📄 **Read:** [references/appearance-customization.md](references/appearance-customization.md)
- Header icon position (Start, End)
- Header background color customization
- Icon color customization
- Visual State Manager for state-based styling
  - Expanded and Collapsed states
  - Focused state for keyboard navigation
  - PointerOver state for hover effects
  - Normal state styling
- Complete Visual State styling examples

### Events and Interaction
📄 **Read:** [references/events.md](references/events.md)
- Expanding event (cancellable)
- Expanded event (completion notification)
- Collapsing event (cancellable)
- Collapsed event (completion notification)
- Event arguments: Cancel and Index properties
- Validation before expand/collapse
- Common event handling patterns

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- Auto scroll position (Top, MakeVisible)
- BringIntoView for programmatic scrolling
- Liquid Glass Effect integration
  - Wrapping SfAccordion inside SfGlassEffectView
  - EnableLiquidGlassEffect property
  - Transparent background setup
  - Platform requirements (iOS 26+, macOS 26+)
- Grid layout considerations with Height=Auto
- HorizontalOptions and VerticalOptions best practices


