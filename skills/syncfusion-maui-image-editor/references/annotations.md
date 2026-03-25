# Annotations in .NET MAUI ImageEditor

This guide covers adding and managing annotations (shapes, text, freehand drawing, and custom views) in the .NET MAUI ImageEditor control.

## Table of Contents
- [Shape Annotations Overview](#shape-annotations-overview)
- [Available Shape Types](#available-shape-types)
- [Adding Shapes](#adding-shapes)
- [Customizing Shape Settings](#customizing-shape-settings)
- [Text Annotations](#text-annotations)
- [Freehand Drawing (Pen)](#freehand-drawing-pen)
- [Custom View Annotations](#custom-view-annotations)
- [Managing Annotations](#managing-annotations)
- [Annotation Events](#annotation-events)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)
- [Next Steps](#next-steps)

## Shape Annotations Overview

The ImageEditor control allows you to add various shapes over an image with customizable settings. Shapes can be added programmatically or through the built-in toolbar.

### When to Use Shape Annotations

- Drawing attention to specific areas (arrows, circles)
- Highlighting or marking regions (rectangles, polygons)
- Creating diagrams or flowcharts (lines, dotted lines)
- Annotating images for documentation or collaboration

## Available Shape Types

The `AnnotationShape` enum includes the following shape types:

1. **Circle** - Perfect circular shape
2. **Rectangle** - Four-sided rectangular shape
3. **Arrow** - Single arrow pointing right
4. **Line** - Straight solid line
5. **Dotted** - Dotted line
6. **DoubleArrow** - Arrow pointing both directions
7. **DottedArrow** - Dotted arrow
8. **DottedDoubleArrow** - Dotted double-sided arrow
9. **Polygon** - Multi-point closed shape
10. **Polyline** - Multi-point open shape
11. **Pen** - Represent pen

## Adding Shapes

### Basic Shape Addition

Use the `AddShape` method to add shapes programmatically:

```csharp
// Add default arrow
imageEditor.AddShape(AnnotationShape.Arrow);

// Add default circle
imageEditor.AddShape(AnnotationShape.Circle);

// Add default rectangle
imageEditor.AddShape(AnnotationShape.Rectangle);
```

**XAML Example:**

```xml
<Grid RowDefinitions="0.9*, 0.1*">
    <imageEditor:SfImageEditor x:Name="imageEditor" Source="photo.jpg" />
    <Button Grid.Row="1" Text="Add Arrow" Clicked="OnAddShapeClicked" />
</Grid>
```

```csharp
private void OnAddShapeClicked(object sender, EventArgs e)
{
    imageEditor.AddShape(AnnotationShape.Arrow);
}
```

### Adding Polygon Shapes

A polygon is formed by connecting straight lines, automatically closing the first and last points:

```csharp
private void AddPolygon()
{
    imageEditor.AddShape(AnnotationShape.Polygon,
        new ImageEditorShapeSettings()
        {
            StrokeThickness = 5,
            Color = Colors.Red,
            Points = new PointCollection
            {
                new Point(50, 0),
                new Point(150, 0),
                new Point(200, 100),
                new Point(150, 200),
                new Point(50, 200),
                new Point(0, 100)
            }
        });
}
```

**When to use:** Creating custom closed shapes like hexagons, stars, or custom highlighting regions.

### Adding Polyline Shapes

A polyline draws connected straight lines without closing the shape:

```csharp
private void AddPolyline()
{
    imageEditor.AddShape(AnnotationShape.Polyline,
        new ImageEditorShapeSettings()
        {
            StrokeThickness = 3,
            Color = Colors.Blue,
            Points = new PointCollection
            {
                new Point(0, 100),
                new Point(50, 250),
                new Point(75, 100),
                new Point(90, 400),
                new Point(115, 250),
                new Point(175, 250)
            }
        });
}
```

**When to use:** Drawing paths, connecting points, or creating open-ended annotations.

## Customizing Shape Settings

Use `ImageEditorShapeSettings` to customize shape appearance and behavior:

### Common Shape Properties

```csharp
imageEditor.AddShape(AnnotationShape.Rectangle,
    new ImageEditorShapeSettings()
    {
        // Unique identifier for the annotation
        Id = 1,
        
        // Appearance
        Color = Colors.Blue,
        StrokeThickness = 5,
        IsFilled = false,
        Opacity = 0.8,
        
        // Position and size (values between 0 and 1)
        Bounds = new Rect(0.2, 0.2, 0.5, 0.3),
        
        // Interaction
        AllowDrag = true,
        AllowResize = true
    });
```

### Property Details

**Visual Properties:**
- `Color` - Shape color (stroke or fill)
- `StrokeThickness` - Line width (not applicable to filled shapes)
- `IsFilled` - Fill shape interior (Rectangle, Circle, Polygon only)
- `Opacity` - Transparency (0-1)

**Positioning:**
- `Bounds` - Position and size (Rect with values 0-1, relative to image dimensions)

**Interaction:**
- `AllowDrag` - Enable/disable dragging (default: true)
- `AllowResize` - Enable/disable resizing (default: true)
- `Id` - Unique identifier for programmatic selection

### Filled vs Stroke Shapes

```csharp
// Filled circle
imageEditor.AddShape(AnnotationShape.Circle,
    new ImageEditorShapeSettings()
    {
        Color = Colors.Red,
        IsFilled = true,
        Opacity = 0.5
    });

// Stroke-only rectangle
imageEditor.AddShape(AnnotationShape.Rectangle,
    new ImageEditorShapeSettings()
    {
        Color = Colors.Green,
        IsFilled = false,
        StrokeThickness = 3
    });
```

## Text Annotations

Add text overlays to images using the `AddText` method:

### Basic Text Addition

```csharp
// Simple text
imageEditor.AddText("Good morning");

// With custom settings
imageEditor.AddText("Annotated Text",
    new ImageEditorTextSettings()
    {
        TextStyle = new ImageEditorTextStyle()
        {
            FontSize = 18,
            TextColor = Colors.White,
            FontFamily = "Arial",
            FontAttributes = FontAttributes.Bold
        },
        Background = Brush.SolidColorBrush(Colors.Blue),
        Opacity = 0.8
    });
```

### Text Customization Options

```csharp
imageEditor.AddText("Customized Text",
    new ImageEditorTextSettings()
    {
        // Unique ID
        Id = 100,
        
        // Position and rotation
        Bounds = new Rect(0.3, 0.3, 0.4, 0.1),
        RotationAngle = 45,
        
        // Text style
        TextStyle = new ImageEditorTextStyle()
        {
            FontSize = 14,
            TextColor = Colors.Black,
            FontFamily = "Arial",
            FontAttributes = FontAttributes.Italic
        },
        
        // Alignment
        TextAlignment = TextAlignment.Center,
        
        // Appearance
        Background = Brush.SolidColorBrush(Colors.Yellow.WithAlpha(0.5f)),
        Opacity = 0.9,
        
        // Interaction
        IsEditable = true,
        IsRotatable = true,
        AllowDrag = true,
        AllowResize = true
    });
```

**When to use:**
- Labels and captions
- Watermarks
- Instructional notes
- Copyright notices

## Freehand Drawing (Pen)

While not covered in the shape.md source, pen annotations are referenced in the toolbar and would typically be handled through toolbar interaction.

## Custom View Annotations

Add any custom MAUI view as an annotation:

### Basic Custom View

```csharp
private void AddCustomView()
{
    Image customImage = new Image() 
    { 
        HeightRequest = 100, 
        WidthRequest = 100, 
        Aspect = Aspect.Fill
    };
    customImage.Source = ImageSource.FromFile("emoji.png");
    
    imageEditor.AddCustomAnnotationView(customImage);
}
```

### Custom View with Settings

```csharp
private void AddCustomViewWithSettings()
{
    Label customLabel = new Label 
    { 
        Text = "Custom Label", 
        WidthRequest = 150, 
        HeightRequest = 40,
        BackgroundColor = Colors.LightBlue,
        HorizontalTextAlignment = TextAlignment.Center,
        VerticalTextAlignment = TextAlignment.Center
    };
    
    imageEditor.AddCustomAnnotationView(customLabel,
        new ImageEditorAnnotationSettings()
        {
            Id = 500,
            Bounds = new Rect(0.2, 0.2, 0.5, 0.1),
            RotationAngle = 45,
            AllowResize = true,
            AllowDrag = true
        });
}
```

**When to use:**
- Adding logos or branding
- Emoji or sticker overlays
- Complex custom UI elements
- Interactive overlays

## Managing Annotations

### Selecting Annotations Programmatically

Select specific annotations by their unique ID:

```csharp
// Select annotation with ID 1
imageEditor.SelectAnnotation(1);

// Handle in button click
private void SelectShape_Clicked(object sender, EventArgs e)
{
    int shapeId;
    if (int.TryParse(shapeID.Text, out shapeId))
    {
        imageEditor.SelectAnnotation(shapeId);
    }
}
```

### Deleting Annotations

```csharp
// Delete currently selected annotation
imageEditor.DeleteAnnotation();

// In button click handler
private void OnDeleteAnnotationClicked(object sender, EventArgs e)
{
    imageEditor.DeleteAnnotation();
}
```

### Clearing All Annotations

Remove all annotations (shapes, text, custom views):

```csharp
imageEditor.ClearAnnotations();
```

**Warning:** This action removes ALL annotations and cannot be undone unless using the Undo/Redo functionality.

## Annotation Events

### AnnotationSelected Event

Triggered when an annotation is selected:

```xml
<imageEditor:SfImageEditor x:Name="imageEditor"
                          Source="image.png"
                          AnnotationSelected="OnAnnotationSelected" />
```

```csharp
private void OnAnnotationSelected(object sender, AnnotationSelectedEventArgs e)
{
    if (e.AnnotationSettings is ImageEditorShapeSettings shapeSettings)
    {
        // Modify selected shape
        shapeSettings.Color = Colors.Black;
        shapeSettings.StrokeThickness = 3;
    }
    else if (e.AnnotationSettings is ImageEditorTextSettings textSettings)
    {
        // Modify selected text
        textSettings.TextStyle.FontSize = 20;
    }
}
```

### AnnotationUnselected Event

Triggered when an annotation is deselected:

```csharp
private void OnAnnotationUnselected(object sender, AnnotationUnselectedEventArgs e)
{
    if (e.AnnotationSettings is ImageEditorShapeSettings shapeSettings)
    {
        // Perform cleanup or final adjustments
        shapeSettings.IsFilled = true;
    }
}
```

## Common Patterns

### Adding Shape on Image Load

```csharp
<imageEditor:SfImageEditor x:Name="imageEditor"
                          Source="image.png"
                          ImageLoaded="OnImageLoaded" />
```

```csharp
private void OnImageLoaded(object sender, EventArgs e)
{
    // Add default arrow on load
    imageEditor.AddShape(AnnotationShape.Arrow);
    
    // Or add with specific settings
    imageEditor.AddShape(AnnotationShape.Circle,
        new ImageEditorShapeSettings()
        {
            Bounds = new Rect(0.4, 0.4, 0.2, 0.2),
            Color = Colors.Red
        });
}
```

### Adding Annotation with Manual Bounds

Position annotations precisely using bounds (0-1 relative values):

```csharp
private void AddAnnotationWithBounds()
{
    // Add arrow at specific position
    imageEditor.AddShape(AnnotationShape.Arrow,
        new ImageEditorShapeSettings()
        {
            Bounds = new Rect(0.1, 0.1, 0.5, 0.5)
        });
}
```

### Restricting Drag and Resize

```csharp
imageEditor.AddShape(AnnotationShape.Rectangle,
    new ImageEditorShapeSettings()
    {
        AllowDrag = false,    // Cannot move
        AllowResize = false,  // Cannot resize
        Bounds = new Rect(0.3, 0.3, 0.3, 0.3)
    });
```

### Multiple Annotations Workflow

```csharp
private void CreateAnnotatedImage()
{
    // 1. Add background highlight
    imageEditor.AddShape(AnnotationShape.Rectangle,
        new ImageEditorShapeSettings()
        {
            Id = 1,
            Color = Colors.Yellow,
            IsFilled = true,
            Opacity = 0.3,
            Bounds = new Rect(0.1, 0.1, 0.8, 0.2)
        });
    
    // 2. Add arrow pointer
    imageEditor.AddShape(AnnotationShape.Arrow,
        new ImageEditorShapeSettings()
        {
            Id = 2,
            Color = Colors.Red,
            StrokeThickness = 4,
            Bounds = new Rect(0.2, 0.3, 0.3, 0.1)
        });
    
    // 3. Add text label
    imageEditor.AddText("Important",
        new ImageEditorTextSettings()
        {
            Id = 3,
            Bounds = new Rect(0.15, 0.08, 0.3, 0.08),
            TextStyle = new ImageEditorTextStyle()
            {
                FontSize = 16,
                TextColor = Colors.Black,
                FontAttributes = FontAttributes.Bold
            }
        });
    
    // 4. Save edits
    imageEditor.SaveEdits();
}
```

## Troubleshooting

### Issue: Annotation Not Visible

**Cause:** Bounds are outside the visible range (0-1).

**Solution:**
```csharp
// Ensure bounds are within valid range
var bounds = new Rect(0.2, 0.2, 0.5, 0.3); // All values between 0-1
imageEditor.AddShape(AnnotationShape.Circle,
    new ImageEditorShapeSettings() { Bounds = bounds });
```

### Issue: Cannot Select Annotation

**Cause:** Annotation interaction disabled or incorrect ID used.

**Solution:**
```csharp
// Ensure annotation is selectable
imageEditor.AddShape(AnnotationShape.Rectangle,
    new ImageEditorShapeSettings()
    {
        Id = 1,
        AllowDrag = true,  // Ensure interaction enabled
        AllowResize = true
    });

// Use correct ID
imageEditor.SelectAnnotation(1);
```

### Issue: Text Not Editable

**Cause:** `IsEditable` property set to false.

**Solution:**
```csharp
imageEditor.AddText("Edit Me",
    new ImageEditorTextSettings()
    {
        IsEditable = true  // Enable editing
    });
```

### Issue: Polygon Not Drawing Correctly

**Cause:** Insufficient points or incorrect point coordinates.

**Solution:**
```csharp
// Provide at least 3 points for polygon
var points = new PointCollection
{
    new Point(50, 0),
    new Point(100, 50),
    new Point(50, 100),
    new Point(0, 50)  // At least 4 points for proper polygon
};

imageEditor.AddShape(AnnotationShape.Polygon,
    new ImageEditorShapeSettings() { Points = points });
```

## Next Steps

- **Cropping & Transformations:** [crop-transform.md](crop-transform.md)
- **Image Effects:** [filters-effects.md](filters-effects.md)
- **Saving Images:** [save-serialization.md](save-serialization.md)
- **Events:** [events.md](events.md)
- **Toolbar Customization:** [toolbar.md](toolbar.md)