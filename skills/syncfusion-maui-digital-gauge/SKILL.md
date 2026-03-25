---
name: syncfusion-maui-digital-gauge
description: Implements Syncfusion .NET MAUI DigitalGauge (SfDigitalGauge) control to display alphanumeric characters in LED-style digital format. Use when working with digital gauges, LED displays, seven-segment displays, digital clocks, or digital counters. Ideal for displaying numbers, letters, or special characters in a digital/electronic display format.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Digital Gauges in .NET MAUI

The Syncfusion .NET MAUI DigitalGauge (SfDigitalGauge) control displays alphanumeric characters in digital LED-style format. It supports multiple segment types for rendering numbers, alphabets, and special characters, making it ideal for digital clocks, counters, status displays, and retro-style interfaces.

## When to Use This Skill

Use this skill when you need to:
- Display text in digital/LED display format (like digital clocks, calculators, or instrument panels)
- Implement seven-segment, fourteen-segment, or sixteen-segment character displays
- Show numeric values with a retro or industrial visual style
- Create digital counters, timers, or scoreboards
- Display alphanumeric data in 8×8 dot matrix format
- Build dashboard components with digital readouts
- Implement status indicators with LED-style characters

## Component Overview

The DigitalGauge provides four different character segment types:
- **7-segment**: Best for displaying numbers and limited uppercase letters
- **14-segment**: Displays numbers and full alphabet
- **16-segment**: Clear display of numbers and alphabet
- **8×8 dot matrix**: Supports numbers, alphabet, and special characters

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installing Syncfusion.Maui.Gauges NuGet package
- Registering handlers in MauiProgram.cs
- Basic DigitalGauge implementation in XAML and C#
- Displaying text with the Text property
- Initial configuration

### Character Segment Types
📄 **Read:** [references/character-segment-types.md](references/character-segment-types.md)
- Understanding the four segment types (7, 14, 16, 8×8 dot matrix)
- Choosing the right segment type for your use case
- Seven-segment display implementation
- Fourteen-segment display implementation
- Sixteen-segment display implementation
- EightCrossEightDotMatrix display implementation
- Segment type comparison and recommendations

### Character Display Types
📄 **Read:** [references/character-display-types.md](references/character-display-types.md)
- Displaying numeric characters
- Displaying alphabetic characters
- Displaying special characters
- Text property usage patterns
- Compatible segment types for different content

### Customization Options
📄 **Read:** [references/customization.md](references/customization.md)
- Character size (CharacterHeight, CharacterWidth)
- Character spacing
- Segment colors (CharacterStroke)
- Stroke width customization
- Disabled segment styling (DisabledSegmentStroke, DisabledSegmentAlpha)
- Background color configuration
- Complete styling examples

### Events
📄 **Read:** [references/events.md](references/events.md)
- TextChanged event
- Event arguments and handling
- Event patterns in XAML and C#

