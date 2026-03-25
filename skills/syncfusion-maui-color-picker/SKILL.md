---
name: syncfusion-maui-color-picker
description: Implements Syncfusion .NET MAUI Color Picker (SfColorPicker). Use when implementing color picker functionality, color selection UI, palette mode, spectrum mode, or alpha slider configuration in .NET MAUI applications. Covers inline rendering, popup customization, color changed events, localization, and liquid glass effect.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# .NET MAUI Color Picker Implementation

A comprehensive skill for implementing and customizing the Syncfusion .NET MAUI Color Picker (SfColorPicker) control. This control enables users to select colors from palette grids or spectrum gradients, with support for custom colors, recent colors, alpha transparency, and multiple display modes.

## When to Use This Skill

Use this skill when you need to:
- Install and set up the Syncfusion .NET MAUI Color Picker
- Implement color selection UI in .NET MAUI applications
- Switch between Palette and Spectrum color modes
- Customize color picker appearance (palettes, sliders, buttons)
- Display color picker inline or as a popup/dropdown
- Handle color selection events (ColorChanging, ColorChanged, ColorSelected)
- Configure recent colors panel and custom color collections
- Implement alpha/opacity control for colors
- Customize display view (icons, templates, dropdown button)
- Add localization support for different cultures
- Enable modern Liquid Glass effect (.NET 10+)
- Troubleshoot color picker implementation issues

## Component Overview

The **SfColorPicker** is a versatile color selection control that provides:

- **Two color modes**: Palette (predefined color grid) and Spectrum (gradient-based selection)
- **Custom colors**: Add/remove custom colors to palette
- **Recent colors**: Automatic tracking of recently selected colors
- **Alpha control**: Adjustable transparency via alpha slider
- **Manual input**: Direct color value entry (RGB, HSV, HEX formats)
- **Inline & Popup modes**: Embed directly in layout or show as dropdown
- **No color option**: Allow users to deselect/clear color
- **Rich customization**: Extensive styling for palettes, sliders, buttons, and indicators
- **Localization**: Multi-language support via resource files
- **Modern effects**: Liquid Glass/Acrylic visual effect (.NET 10+)

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation via NuGet package
- Handler registration in MauiProgram.cs
- Basic SfColorPicker implementation (XAML & C#)
- First working color picker example

### Color Modes and Selection
📄 **Read:** [references/modes-and-selection.md](references/modes-and-selection.md)
- ColorMode property (Palette vs Spectrum)
- Setting default SelectedColor
- Color mode switcher visibility
- Recent colors panel (ShowRecentColors)
- ClearRecentColors method
- ShowNoColor option

### Customization
📄 **Read:** [references/customization.md](references/customization.md)
- Input area visibility
- Alpha slider configuration
- Action buttons (Apply/Cancel) visibility and styling
- Button background and label styles
- Recent colors and spectrum input view label styles
- Palette customization (row/column count, spacing, cell shape, size, corner radius)
- Custom palette colors (PaletteColors)
- Selection indicator customization (radius, stroke, thickness)
- Slider thumb customization (fill, radius, stroke, thickness)
- Popup customization (IsOpen, background, relative position)

### Display View Customization
📄 **Read:** [references/display-view.md](references/display-view.md)
- Selected color icon (SelectedColorIcon)
- Selected color icon size
- Selected color template (SelectedColorTemplate)
- Drop-down button template (DropDownButtonTemplate)
- Display view height, stroke, and stroke thickness
- Dropdown button width

### Inline Rendering
📄 **Read:** [references/inline-rendering.md](references/inline-rendering.md)
- IsInline property
- When to use inline vs popup mode
- Inline color picker implementation
- Use cases for embedded color pickers

### Events and Interaction
📄 **Read:** [references/events-and-interaction.md](references/events-and-interaction.md)
- ColorChanging event (with cancellation support)
- ColorChanged event (behavior with action buttons)
- ColorSelected event
- ColorChangedCommand
- Event arguments and properties
- Practical examples with UI updates

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- Localization setup and configuration
- Setting CurrentUICulture
- Resource file structure
- Liquid Glass effect (EnableLiquidGlassEffect)
- Platform requirements (.NET 10+, iOS 26, macOS 26)
- Visual tips and best practices

