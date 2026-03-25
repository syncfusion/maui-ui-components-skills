---
name: syncfusion-maui-badge-view
description: Implements Syncfusion .NET MAUI Badge View (SfBadgeView) control. Use when adding badge notifications, status indicators, unread counts, or notification overlays to MAUI applications. Covers SfBadgeView setup, badge text customization, position adjustments, animations, predefined symbols, and accessibility features.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Badge Views in .NET MAUI

A comprehensive guide for implementing and customizing Syncfusion's .NET MAUI Badge View (SfBadgeView) control. Badge views display notification counts, status indicators, or alerts overlaid on content like buttons, images, or avatars.

## When to Use This Skill

Use this skill when you need to:
- Add notification badges to buttons, icons, or profile images
- Display unread message counts or notification numbers
- Show status indicators (available, busy, away)
- Implement notification overlays with custom positioning
- Add animated badge notifications
- Create badges with predefined symbols or icons
- Customize badge appearance (colors, fonts, shapes)
- Position badges around content elements
- Implement accessible badge notifications with screen reader support

## Component Overview

The Badge View (SfBadgeView) is a notification control that overlays badges on content elements. It supports:
- **Text badges:** Display notification counts or short text
- **Icon badges:** Show predefined status symbols (available, away, busy, etc.)
- **Positioning:** Place badges at 8 positions around content (TopRight, TopLeft, BottomRight, etc.)
- **Customization:** Fonts, colors, strokes, backgrounds, corner radius
- **Animation:** Scale animations when badge text changes
- **Predefined styles:** 8 color types (Primary, Error, Success, Warning, etc.)
- **Accessibility:** Screen reader support for badge content

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

Read this reference when you need to:
- Install Syncfusion.Maui.Core NuGet package
- Register the Syncfusion handler in MauiProgram.cs
- Create your first badge view
- Add badge text or content
- Implement basic badge notifications
- Add accessibility with ScreenReaderText

### Badge Customization
📄 **Read:** [references/customization.md](references/customization.md)

Read this reference when you need to:
- Customize font properties (size, family, attributes)
- Add stroke borders and adjust thickness
- Change text color and padding
- Apply predefined badge types (Primary, Error, Success, Warning, Info, Dark, Light, Secondary)
- Set custom background colors
- Configure corner radius for rounded badges
- Align badges (Center, Start, End)
- Handle badge alignment with different sizing scenarios
- Keep multiple badges aligned uniformly with AutoHide
- Enable font auto-scaling for accessibility
- Control badge visibility

### Position Customization
📄 **Read:** [references/positioning.md](references/positioning.md)

Read this reference when you need to:
- Position badges around content (TopRight, TopLeft, BottomRight, BottomLeft, Left, Top, Right, Bottom)
- Fine-tune badge placement with Offset property
- Adjust X,Y coordinates for precise positioning
- Position badges on images, buttons, or custom controls

### Animation
📄 **Read:** [references/animation.md](references/animation.md)

Read this reference when you need to:
- Enable or disable badge animations
- Configure scale animation when badge text changes
- Adjust animation duration for faster or slower effects
- Create dynamic badge notifications

### Predefined Symbols
📄 **Read:** [references/predefined-symbols.md](references/predefined-symbols.md)

Read this reference when you need to:
- Use badge icons instead of text (Add, Available, Away, Busy, Delete, Dot, Prohibit)
- Display status indicators with predefined symbols
- Understand icon vs text priority
- Implement icon-based badges on avatars or images

