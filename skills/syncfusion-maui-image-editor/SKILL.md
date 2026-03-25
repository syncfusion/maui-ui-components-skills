---
name: syncfusion-maui-image-editor
description: Implements and customize Syncfusion .NET MAUI ImageEditor (SfImageEditor) for editing, annotating, and transforming images. Use when working with MAUI image editing, photo editing, image cropping, or image transformations. Covers shape/text overlays, freehand drawing, image filters, toolbar customization, and saving edited images.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing .NET MAUI Image Editor

A comprehensive skill for implementing and customizing the Syncfusion .NET MAUI ImageEditor (SfImageEditor) control. The ImageEditor provides powerful image editing capabilities including cropping, transformations, annotations, filters, and effects with a built-in customizable toolbar.

## When to Use This Skill

Use this skill when you need to:
- Edit and manipulate images in a .NET MAUI application
- Crop images with various aspect ratios or custom selections
- Rotate or flip images (transformations)
- Add annotations: shapes, text, or freehand drawing
- Apply filters and effects (brightness, contrast, blur, saturation, etc.)
- Customize the built-in toolbar or create custom toolbars
- Zoom and pan images for detailed editing
- Save edited images to file or stream
- Serialize and deserialize editor state (annotations, transformations)
- Implement undo/redo functionality
- Handle image editing events
- Customize image editor appearance

## Component Overview

**SfImageEditor** is a comprehensive image editing control for .NET MAUI that provides:

**Core Features:**
- Image loading from file, stream, or resources
- Cropping with presets (Square, Circle, 16:9, 4:3, 3:2, etc.) or freehand
- Transformations: Rotation (90°, 180°, 270°, custom angles), Flip (horizontal/vertical)
- Annotations: Shapes (Circle, Rectangle, Arrow, Line, Path), Text, Freehand drawing
- Image filters: Brightness, Contrast, Blur, Sharpen, Exposure, Saturation, Hue, Opacity
- Built-in toolbar with full customization support
- Zoom and pan with pinch gestures
- Z-ordering for annotations (bring to front, send to back)
- Undo/Redo operations
- Save to file or stream (JPEG, JPG, PNG, BMP)
- Serialization/Deserialization of editor state
- Comprehensive event handling

**NuGet Package:** `Syncfusion.Maui.ImageEditor`

## Documentation and Navigation Guide

### Getting Started & Setup
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and NuGet package setup
- Handler registration in MauiProgram.cs
- Basic ImageEditor implementation (XAML & C#)
- Loading images from different sources
- Source property and image loading

### Cropping & Transformations
📄 **Read:** [references/crop-transform.md](references/crop-transform.md)
- Image cropping with Crop method
- ImageCropType options (Free, Square, Circle, aspect ratios)
- Interactive vs programmatic cropping
- Rotation operations (Rotate method)
- Flip operations (horizontal/vertical)
- Custom angle rotations
- Transformation events

### Annotations
📄 **Read:** [references/annotations.md](references/annotations.md)
- Adding shape annotations (Circle, Rectangle, Arrow, Line, Path)
- AddShape method and AnnotationShape enum
- Text annotations (AddText method)
- Freehand drawing capabilities
- Custom view overlays
- Annotation settings (bounds, opacity, rotation, colors)
- Annotation selection and manipulation
- AllowDrag, AllowResize, IsEditable properties

### Filters & Effects
📄 **Read:** [references/filters-effects.md](references/filters-effects.md)
- Applying image filters programmatically
- Brightness and contrast adjustments
- Exposure, saturation, and hue controls
- Blur and sharpen effects
- Opacity modifications
- Combining multiple effects
- Effect preview and reset

### Toolbar Customization
📄 **Read:** [references/toolbar.md](references/toolbar.md)
- ShowToolbar property for visibility control
- Built-in toolbar items
- Adding custom toolbar items
- Removing or reordering toolbar items
- Toolbar positioning and orientation
- Toolbar item appearance customization
- ToolbarItemSelected event
- Creating completely custom toolbars

### Zooming & Navigation
📄 **Read:** [references/zooming-navigation.md](references/zooming-navigation.md)
- Zoom functionality (pinch-to-zoom, programmatic)
- Zoom level management
- Pan and navigation controls
- FitToScreen functionality
- Z-ordering for annotations
- BringToFront and SendToBack methods
- Layer management for complex compositions

### Save & Serialization
📄 **Read:** [references/save-serialization.md](references/save-serialization.md)
- Saving edited images (Save method)
- Save to file path vs stream
- Image format options (JPEG, JPG, PNG, BMP)
- Quality and compression settings
- Serialization to JSON (annotations and state)
- Deserialization (loading saved editor state)
- Reset functionality
- Export scenarios and best practices

### Undo & Redo Operations
📄 **Read:** [references/undo-redo.md](references/undo-redo.md)
- Undo and Redo methods
- CanUndo and CanRedo properties
- History stack management
- Custom UI for undo/redo
- History limits and configuration
- Undo/Redo events

### Events & Event Handling
📄 **Read:** [references/events.md](references/events.md)
- ImageLoaded event
- ImageSaving and ImageSaved events
- ItemsSelected event (annotation selection)
- ToolbarItemSelected event
- Cropping events
- Event arguments and accessing event data
- Building event-driven workflows

### Styling & Customization
📄 **Read:** [references/styling-customization.md](references/styling-customization.md)
- Visual customization options
- Theme support and styling
- Liquid glass effect
- Control appearance customization
- Annotation style customization
- Border and background properties

### Accessibility & Localization
📄 **Read:** [references/accessibility-localization.md](references/accessibility-localization.md)
- Accessibility features and support
- Screen reader compatibility
- Keyboard navigation (desktop platforms)
- Semantic properties
- Localization and multi-language support
- Culture-specific formatting
- Right-to-left (RTL) layout support

