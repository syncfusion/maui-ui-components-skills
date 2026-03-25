---
name: syncfusion-maui-avatar-view
description: Implements Syncfusion .NET MAUI Avatar View (SfAvatarView) for displaying user profile pictures, initials, or group avatars. Use when working with avatar views, profile pictures, user images, initials display, profile icons, or contact images. Ideal for implementing user profile displays, contact lists, chat interfaces, or social feeds.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing .NET MAUI Avatar Views

The Syncfusion .NET MAUI Avatar View (SfAvatarView) provides a graphical representation of user images with support for custom images, initials, preset avatars, and group views. It offers extensive customization including shapes, colors, sizes, and badge integration.

## When to Use This Skill

Use this skill when the user needs to:
- **Display user profile pictures** in applications (social apps, contact lists, chat interfaces)
- **Show user initials** when images aren't available (single or double character)
- **Create group avatars** displaying multiple users (up to 3 images/initials)
- **Add badges to avatars** for status indicators or notifications
- **Implement circular or square profile images** with customizable sizes
- **Display preset avatar characters** from built-in vector images
- **Customize avatar appearance** with colors, gradients, strokes, and sizing
- **Build user identification UI** for any MAUI application requiring visual user representation

## Component Overview

**SfAvatarView** is a versatile control for displaying user avatars with five content types:
1. **Default** - Built-in vector image
2. **Initials** - Text-based avatars (single/double character)
3. **Custom** - User-provided images
4. **AvatarCharacter** - Preset vector images
5. **Group** - Multiple users in one view (up to 3)

**Key capabilities:**
- Multiple shapes (Circle, Square, Custom)
- Five size presets (ExtraSmall to ExtraLarge)
- Automatic color generation (Dark/Light backgrounds)
- Badge integration for status/notifications
- Gradient backgrounds and stroke customization
- Font and aspect ratio controls
- MVVM-friendly with data binding support

## Documentation and Navigation Guide

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)

When you need to:
- Install and configure the Avatar View component
- Register Syncfusion handlers in MauiProgram.cs
- Create your first basic avatar view
- Add custom images to the avatar
- Set up initial XAML and C# implementation

### Content Types and Display Modes

📄 **Read:** [references/content-types.md](references/content-types.md)

When you need to:
- Choose between Default, Initials, Custom, AvatarCharacter, or Group content types
- Display single or double character initials
- Set up preset avatar characters
- Create group views with multiple images or initials
- Mix images and initials in group views
- Customize initials color or background colors in groups
- Understand when to use each content type

### Customization and Styling

📄 **Read:** [references/customization.md](references/customization.md)

When you need to:
- Control image aspect ratios (AspectFit, AspectFill, Fill, Center)
- Customize colors (stroke, background, automatic dark/light)
- Apply gradient backgrounds
- Set sizing properties (width, height, corner radius)
- Adjust stroke thickness and content padding
- Configure font properties (size, family, attributes, auto-scaling)
- Create custom styled avatars beyond built-in presets

### Visual Styles and Sizes

📄 **Read:** [references/visual-styles.md](references/visual-styles.md)

When you need to:
- Use built-in circle sizes (ExtraSmall to ExtraLarge)
- Use built-in square sizes (ExtraSmall to ExtraLarge)
- Apply consistent visual styles across multiple avatars
- Understand AvatarShape and AvatarSize properties
- Create uniform avatar displays with predefined styles

### BadgeView Integration

📄 **Read:** [references/badgeview-integration.md](references/badgeview-integration.md)

When you need to:
- Add notification badges to avatars
- Display status indicators (online, away, busy)
- Position badges on avatars (corners, custom offsets)
- Animate badge appearances
- Integrate SfBadgeView with SfAvatarView
- Show unread message counts or status icons

