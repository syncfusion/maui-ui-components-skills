---
name: implementing-interactive-viewer
description: Implement intuitive content viewing with zoom, pan, rotate, and reset capabilities. Use when building image viewers, document visualization, or any application requiring detailed content inspection with smooth navigation. ALWAYS use this skill when users need interactive content viewing, zooming on images, panning large content, or rotating visual materials.
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
---

# Implementing Interactive Viewer in .NET MAUI

The `.NET MAUI Interactive Viewer` provides an intuitive way to view and navigate visual content through zooming, panning, and rotating. It's ideal for image viewers, document exploration, architectural visualization, medical imaging, and e-commerce product displays.

## When to Use This Skill

Use this skill when you need to:
- **Display zoomable images** in your application
- **Enable content panning** for large or complex visual content
- **Implement rotation** for viewing content from different angles
- **Reset views** to original state after user interactions
- **Handle zoom and scroll events** for custom interactions
- **Configure zoom limits** for controlled content navigation
- **Build viewers** for diagrams, blueprints, or technical drawings
- **Create e-commerce** product image viewers
- **Handle medical imaging** or diagnostic visualization

## Quick Overview

The Interactive Viewer control wraps visual content (typically images) and provides these core capabilities:

```csharp
using Syncfusion.Maui.InteractiveViewer;

// Basic setup
var viewer = new SfInteractiveViewer
{
    IsZoomEnabled = true,
    ZoomFactor = 1.0,
    MinimumZoomFactor = 0.5,
    MaximumZoomFactor = 10.0,
    Content = new Image { Source = "image.png", Aspect = Aspect.AspectFit }
};

// Methods for interaction
viewer.Rotate();        // Rotate 90 degrees clockwise
viewer.Reset();         // Restore original state
```

## Documentation and Navigation Guide

Navigate to the reference guide that matches your current need:

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- .NET MAUI environment setup
- NuGet package installation steps
- Handler registration
- Component namespace imports
- Basic initialization pattern
- Embedding content in XAML

### Zooming and Panning
📄 **Read:** [references/zooming-and-panning.md](references/zooming-and-panning.md)
- Enable/disable zoom capability
- Set programmatic zoom factor
- Configure minimum zoom limit
- Configure maximum zoom limit
- Control pan interactions
- Responsive zoom behavior

### Rotation
📄 **Read:** [references/rotation.md](references/rotation.md)
- Rotate method usage
- 90-degree clockwise rotation
- Rotation cycling behavior
- Integration with UI buttons
- Rotation constraints

### Reset Functionality
📄 **Read:** [references/reset-functionality.md](references/reset-functionality.md)
- Reset method overview
- Restore default zoom and pan
- Integration with UI
- When to call reset
- Reset behavior guarantees

### Events and Interactions
📄 **Read:** [references/events-and-interactions.md](references/events-and-interactions.md)
- ZoomFactorChanged event
- ScrollChanged event
- Event argument properties
- Real-time interaction patterns
- Custom event handling

## Common Patterns

### Pattern 1: Simple Image Viewer with Controls
```csharp
// XAML
<Grid RowDefinitions="0.9*, 0.1*">
    <interactiveViewer:SfInteractiveViewer x:Name="viewer">
        <Image Source="image.png" Aspect="AspectFit" />
    </interactiveViewer:SfInteractiveViewer>
    
    <StackLayout Grid.Row="1" Orientation="Horizontal" Spacing="10">
        <Button Text="Rotate" Clicked="OnRotate" />
        <Button Text="Reset" Clicked="OnReset" />
    </StackLayout>
</Grid>

// Code-behind
private void OnRotate(object sender, EventArgs e) => viewer.Rotate();
private void OnReset(object sender, EventArgs e) => viewer.Reset();
```

### Pattern 2: Zoom Control with Limits
```csharp
<interactiveViewer:SfInteractiveViewer 
    IsZoomEnabled="True"
    MinimumZoomFactor="0.5"
    MaximumZoomFactor="10.0"
    ZoomFactor="1.0">
    <Image Source="document.png" Aspect="AspectFit" />
</interactiveViewer:SfInteractiveViewer>
```

### Pattern 3: Event-Driven Zoom Tracking
```csharp
<interactiveViewer:SfInteractiveViewer 
    ZoomFactorChanged="OnZoomChanged"
    ScrollChanged="OnScroll">
    <Image Source="image.png" Aspect="AspectFit" />
</interactiveViewer:SfInteractiveViewer>

// Track zoom changes in real-time
private void OnZoomChanged(object sender, ZoomFactorChangedEventArgs e)
{
    Debug.WriteLine($"Zoom: {e.OldZoomFactor} → {e.NewZoomFactor}");
}
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `IsZoomEnabled` | bool | Enable/disable zoom functionality (default: true) |
| `ZoomFactor` | double | Current zoom level (programmatic control) |
| `MinimumZoomFactor` | double | Minimum allowed zoom level |
| `MaximumZoomFactor` | double | Maximum allowed zoom level |
| `Content` | View | The visual content to display |

## Key Methods

| Method | Description |
|--------|-------------|
| `Rotate()` | Rotate content 90 degrees clockwise |
| `Reset()` | Restore content to original zoom and pan position |

## Key Events

| Event | Description |
|-------|-------------|
| `ZoomFactorChanged` | Fired when zoom level changes |
| `ScrollChanged` | Fired when pan position changes |

## Real-World Use Cases

1. **Medical Imaging Viewer** - Zoom and pan detailed diagnostic images
2. **Architectural Blueprint Viewer** - Navigate large technical drawings
3. **E-commerce Product Viewer** - Interactive product image exploration
4. **Document Scanner** - View and navigate scanned documents
5. **GIS/Map Viewer** - Zoom into geographic data and imagery

---

**Next Step:** Choose a reference based on your current task, or start with [Getting Started](references/getting-started.md) if this is your first time.
