# Zooming & Navigation

This guide covers zooming, panning, and layer management (z-ordering) features in the .NET MAUI ImageEditor control.

## Table of Contents
- [Zooming Overview](#zooming-overview)
- [Enabling and Disabling Zoom](#enabling-and-disabling-zoom)
- [Setting Zoom Level](#setting-zoom-level)
- [Maximum Zoom Level](#maximum-zoom-level)
- [Panning and Navigation](#panning-and-navigation)
- [Z-Ordering (Layer Management)](#z-ordering-layer-management)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)
- [Next Steps](#next-steps)

## Zooming Overview

The ImageEditor control provides built-in support for zooming and panning, allowing users to inspect and edit images at different magnification levels.

### When to Use Zooming

- **Precision Editing:** Zooming in for detailed annotations or edits
- **Image Inspection:** Examining image details closely
- **Accessibility:** Helping users with visual impairments see content better
- **Large Images:** Navigating high-resolution images efficiently

### Zoom Interactions

- **Pinch Gesture:** Pinch in/out to zoom on touch devices
- **Mouse Wheel:** Scroll to zoom on desktop
- **Programmatic:** Set zoom level via code

## Enabling and Disabling Zoom

Control whether users can zoom using the `AllowZoom` property:

### XAML

```xml
<imageEditor:SfImageEditor Source="photo.jpg" AllowZoom="True" />
```

### C#

```csharp
SfImageEditor imageEditor = new SfImageEditor();
imageEditor.Source = "photo.jpg";
imageEditor.AllowZoom = true;  // Enable zooming (default)
// imageEditor.AllowZoom = false;  // Disable zooming
this.Content = imageEditor;
```

**Default Value:** `true`

**When to disable zooming:**
- Fixed-size image displays
- Simple annotation scenarios
- Preventing accidental zoom gestures
- View-only presentations

## Setting Zoom Level

Programmatically control the zoom level using the `ZoomLevel` property:

### Basic Usage

```xml
<imageEditor:SfImageEditor Source="photo.jpg" ZoomLevel="2" />
```

```csharp
SfImageEditor imageEditor = new SfImageEditor();
imageEditor.Source = "photo.jpg";
imageEditor.ZoomLevel = 2;  // 2x magnification
this.Content = imageEditor;
```

### Zoom Level Values

- **1.0:** Original size (100%)
- **2.0:** 2x magnification (200%)
- **0.5:** Half size (50%)
- **Values > 1:** Zoom in
- **Values < 1:** Zoom out

### Dynamic Zoom Control

```xml
<Grid RowDefinitions="0.8*, 0.1*, 0.1*">
    <imageEditor:SfImageEditor x:Name="imageEditor" Source="photo.jpg" />
    
    <Slider Grid.Row="1" 
            Minimum="1" 
            Maximum="5" 
            Value="1"
            ValueChanged="OnZoomSliderChanged" />
    
    <HorizontalStackLayout Grid.Row="2" HorizontalOptions="Center">
        <Button Text="Zoom In" Clicked="OnZoomInClicked" Margin="5" />
        <Button Text="Zoom Out" Clicked="OnZoomOutClicked" Margin="5" />
        <Button Text="Reset Zoom" Clicked="OnResetZoomClicked" Margin="5" />
    </HorizontalStackLayout>
</Grid>
```

```csharp
private void OnZoomSliderChanged(object sender, ValueChangedEventArgs e)
{
    imageEditor.ZoomLevel = e.NewValue;
}

private void OnZoomInClicked(object sender, EventArgs e)
{
    if (imageEditor.ZoomLevel < imageEditor.MaximumZoomLevel)
    {
        imageEditor.ZoomLevel += 0.5;
    }
}

private void OnZoomOutClicked(object sender, EventArgs e)
{
    if (imageEditor.ZoomLevel > 1)
    {
        imageEditor.ZoomLevel -= 0.5;
    }
}

private void OnResetZoomClicked(object sender, EventArgs e)
{
    imageEditor.ZoomLevel = 1;
}
```

## Maximum Zoom Level

Define the maximum allowed zoom level using the `MaximumZoomLevel` property:

### XAML

```xml
<imageEditor:SfImageEditor Source="photo.jpg" 
                          ZoomLevel="2"
                          MaximumZoomLevel="5" />
```

### C#

```csharp
SfImageEditor imageEditor = new SfImageEditor();
imageEditor.Source = "photo.jpg";
imageEditor.MaximumZoomLevel = 5;  // Limit to 5x magnification
this.Content = imageEditor;
```

**Default Maximum:** Platform-dependent (typically 4-5x)

**When to limit zoom:**
- Preventing excessive pixelation
- Performance considerations on lower-end devices
- User experience consistency
- Image quality preservation

### Zoom Limits Example

```csharp
private void ConfigureZoomLimits()
{
    // Conservative zoom for low-res images
    if (IsLowResolution)
    {
        imageEditor.MaximumZoomLevel = 2;
    }
    // Generous zoom for high-res images
    else
    {
        imageEditor.MaximumZoomLevel = 10;
    }
}

private bool IsLowResolution
{
    get
    {
        // Check if image resolution is below threshold
        return imageWidth < 1000 || imageHeight < 1000;
    }
}
```

## Panning and Navigation

When zoomed in, users can pan (scroll) the image to navigate different areas:

### Built-in Pan Gestures

- **Touch Drag:** Drag with finger to pan on mobile
- **Mouse Drag:** Click and drag to pan on desktop
- **Automatic:** Enabled automatically when `ZoomLevel > 1`

### Programmatic Navigation

While the ImageEditor doesn't expose direct pan control, you can guide users:

```csharp
private async void ZoomToRegion(double x, double y, double zoomLevel)
{
    // Zoom in first
    imageEditor.ZoomLevel = zoomLevel;
    
    // Show guidance
    await DisplayAlert("Navigation", 
        "Drag the image to view the highlighted region", 
        "OK");
}
```

## Z-Ordering (Layer Management)

Control the stacking order of annotations using z-ordering methods:

### Available Z-Order Methods

1. **BringToFront:** Move annotation to top layer
2. **SendToBack:** Move annotation to bottom layer
3. **BringForward:** Move annotation one layer up
4. **SendBackward:** Move annotation one layer down

### BringToFront

Move selected annotation to the front of all annotations:

```xml
<Grid RowDefinitions="0.9*, 0.1*">
    <imageEditor:SfImageEditor x:Name="imageEditor" Source="photo.jpg" />
    <Button Grid.Row="1" Text="Bring To Front" Clicked="OnBringToFrontClicked" />
</Grid>
```

```csharp
private void OnBringToFrontClicked(object sender, EventArgs e)
{
    imageEditor.BringToFront();
}
```

### SendToBack

Move selected annotation to the back of all annotations:

```xml
<Grid RowDefinitions="0.9*, 0.1*">
    <imageEditor:SfImageEditor x:Name="imageEditor" Source="photo.jpg" />
    <Button Grid.Row="1" Text="Send To Back" Clicked="OnSendToBackClicked" />
</Grid>
```

```csharp
private void OnSendToBackClicked(object sender, EventArgs e)
{
    imageEditor.SendToBack();
}
```

### BringForward

Move selected annotation one step forward:

```xml
<Grid RowDefinitions="0.9*, 0.1*">
    <imageEditor:SfImageEditor x:Name="imageEditor" Source="photo.jpg" />
    <Button Grid.Row="1" Text="Bring Forward" Clicked="OnBringForwardClicked" />
</Grid>
```

```csharp
private void OnBringForwardClicked(object sender, EventArgs e)
{
    imageEditor.BringForward();
}
```

### SendBackward

Move selected annotation one step backward:

```xml
<Grid RowDefinitions="0.9*, 0.1*">
    <imageEditor:SfImageEditor x:Name="imageEditor" Source="photo.jpg" />
    <Button Grid.Row="1" Text="Send Backward" Clicked="OnSendBackwardClicked" />
</Grid>
```

```csharp
private void OnSendBackwardClicked(object sender, EventArgs e)
{
    imageEditor.SendBackward();
}
```

### When to Use Z-Ordering

- **Overlapping Annotations:** Manage visibility when annotations overlap
- **Layer Organization:** Create visual hierarchy
- **Annotation Priority:** Emphasize important annotations
- **Complex Compositions:** Build layered images with precise control

## Common Patterns

### Zoom and Pan for Precision Editing

```csharp
private void EnablePrecisionMode()
{
    // Zoom in for detailed work
    imageEditor.ZoomLevel = 3;
    
    // User can pan to desired area
    DisplayAlert("Precision Mode", 
        "Zoomed in 3x. Drag to navigate and add precise annotations.", 
        "OK");
}
```

### Zoom to Fit

```csharp
private void FitToScreen()
{
    // Reset to original view
    imageEditor.ZoomLevel = 1;
}
```

### Progressive Zoom

```csharp
private void ZoomInStep()
{
    // Increase zoom by 25%
    double newZoom = imageEditor.ZoomLevel * 1.25;
    
    if (newZoom <= imageEditor.MaximumZoomLevel)
    {
        imageEditor.ZoomLevel = newZoom;
    }
    else
    {
        imageEditor.ZoomLevel = imageEditor.MaximumZoomLevel;
        DisplayAlert("Maximum Zoom", "Reached maximum zoom level", "OK");
    }
}

private void ZoomOutStep()
{
    // Decrease zoom by 25%
    double newZoom = imageEditor.ZoomLevel * 0.75;
    
    if (newZoom >= 1)
    {
        imageEditor.ZoomLevel = newZoom;
    }
    else
    {
        imageEditor.ZoomLevel = 1;
    }
}
```

### Layer Management Workflow

```csharp
private void OrganizeAnnotationLayers()
{
    // 1. Add background rectangle
    imageEditor.AddShape(AnnotationShape.Rectangle,
        new ImageEditorShapeSettings()
        {
            Id = 1,
            Color = Colors.LightGray,
            IsFilled = true,
            Opacity = 0.5
        });
    imageEditor.SaveEdits();
    
    // 2. Add text overlay
    imageEditor.AddText("Title Text",
        new ImageEditorTextSettings()
        {
            Id = 2,
            TextStyle = new ImageEditorTextStyle()
            {
                FontSize = 20,
                TextColor = Colors.Black
            }
        });
    imageEditor.SaveEdits();
    
    // 3. If text is hidden, bring it forward
    imageEditor.SelectAnnotation(2);
    imageEditor.BringToFront();
}
```

### Custom Zoom Controls

```xml
<Grid RowDefinitions="*, Auto, Auto">
    <imageEditor:SfImageEditor x:Name="imageEditor" Source="photo.jpg" />
    
    <!-- Zoom indicator -->
    <Label Grid.Row="1" 
           x:Name="zoomLabel"
           Text="Zoom: 100%"
           HorizontalOptions="Center"
           Margin="10" />
    
    <!-- Zoom controls -->
    <Grid Grid.Row="2" ColumnDefinitions="*, *, *, *" Margin="10">
        <Button Text="−" Clicked="OnZoomOutClicked" Grid.Column="0" />
        <Button Text="1:1" Clicked="OnResetZoomClicked" Grid.Column="1" />
        <Button Text="Fit" Clicked="OnFitToScreenClicked" Grid.Column="2" />
        <Button Text="+" Clicked="OnZoomInClicked" Grid.Column="3" />
    </Grid>
</Grid>
```

```csharp
private void UpdateZoomLabel()
{
    int zoomPercent = (int)(imageEditor.ZoomLevel * 100);
    zoomLabel.Text = $"Zoom: {zoomPercent}%";
}

private void OnZoomInClicked(object sender, EventArgs e)
{
    imageEditor.ZoomLevel = Math.Min(
        imageEditor.ZoomLevel + 0.5, 
        imageEditor.MaximumZoomLevel);
    UpdateZoomLabel();
}

private void OnZoomOutClicked(object sender, EventArgs e)
{
    imageEditor.ZoomLevel = Math.Max(imageEditor.ZoomLevel - 0.5, 1);
    UpdateZoomLabel();
}

private void OnResetZoomClicked(object sender, EventArgs e)
{
    imageEditor.ZoomLevel = 1;
    UpdateZoomLabel();
}

private void OnFitToScreenClicked(object sender, EventArgs e)
{
    imageEditor.ZoomLevel = 1;
    UpdateZoomLabel();
}
```

### Annotation Depth Manager

```csharp
private void ShowLayerManager()
{
    var actions = new[]
    {
        "Bring to Front",
        "Send to Back",
        "Bring Forward",
        "Send Backward",
        "Cancel"
    };
    
    // Show action sheet
    // In actual implementation, use proper UI pattern
}

private void ExecuteLayerAction(string action)
{
    switch (action)
    {
        case "Bring to Front":
            imageEditor.BringToFront();
            break;
        case "Send to Back":
            imageEditor.SendToBack();
            break;
        case "Bring Forward":
            imageEditor.BringForward();
            break;
        case "Send Backward":
            imageEditor.SendBackward();
            break;
    }
}
```

## Troubleshooting

### Issue: Cannot Zoom

**Cause:** `AllowZoom` set to false.

**Solution:**
```csharp
imageEditor.AllowZoom = true;
```

### Issue: Zoom Level Not Changing

**Cause:** Trying to exceed `MaximumZoomLevel`.

**Solution:**
```csharp
// Check before setting
if (desiredZoom <= imageEditor.MaximumZoomLevel)
{
    imageEditor.ZoomLevel = desiredZoom;
}
```

### Issue: Image Too Pixelated When Zoomed

**Cause:** Low resolution image or excessive zoom.

**Solution:**
```csharp
// Limit zoom for low-res images
imageEditor.MaximumZoomLevel = 2;
```

### Issue: Z-Ordering Not Working

**Cause:** No annotation selected or annotation not selectable.

**Solution:**
```csharp
// Ensure annotation is selected first
imageEditor.SelectAnnotation(annotationId);
imageEditor.BringToFront();
```

### Issue: Pan Not Working

**Cause:** Zoom level is 1 (cannot pan at original size).

**Solution:**
```csharp
// Zoom in first to enable panning
if (imageEditor.ZoomLevel == 1)
{
    imageEditor.ZoomLevel = 2;
}
```

## Next Steps

- **Getting Started:** [getting-started.md](getting-started.md)
- **Annotations:** [annotations.md](annotations.md)
- **Crop & Transform:** [crop-transform.md](crop-transform.md)
- **Toolbar:** [toolbar.md](toolbar.md)