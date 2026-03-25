# Events in .NET MAUI ImageEditor

This guide covers the various events available in the .NET MAUI ImageEditor control for handling user interactions and state changes.

## Table of Contents
- [Events Overview](#events-overview)
- [Image Events](#image-events)
- [Annotation Events](#annotation-events)
- [Save Events](#save-events)
- [Toolbar Events](#toolbar-events)
- [Reset Events](#reset-events)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)
- [Next Steps](#next-steps)

## Events Overview

The ImageEditor control provides a comprehensive set of events to react to user actions, state changes, and lifecycle events.

### Available Events

- `ImageLoaded` - Image loaded into editor
- `ImageSaving` - Before saving image
- `ImageSaved` - After image saved
- `SavePickerOpening` - Save dialog opening
- `BrowseImage` - Browse button clicked
- `AnnotationSelected` - Annotation selected
- `AnnotationUnselected` - Annotation deselected
- `AnnotationsDeserialized` - Annotations loaded from serialization
- `ToolbarItemSelected` - Toolbar item clicked
- `BeginReset` - Before image reset

## Image Events

### ImageLoaded Event

Triggered after an image has been successfully loaded into the editor.

**Common Uses:**
- Initial image processing
- Adding default annotations
- Applying default effects
- Logging or analytics

```xml
<imageEditor:SfImageEditor x:Name="imageEditor"
                          Source="photo.jpg"
                          ImageLoaded="OnImageLoaded" />
```

```csharp
private void OnImageLoaded(object sender, EventArgs e)
{
    // Image is now ready for editing
    Console.WriteLine("Image loaded successfully");
    
    // Add watermark automatically
    imageEditor.AddText("© 2026 Company Name",
        new ImageEditorTextSettings()
        {
            Bounds = new Rect(0.7, 0.9, 0.25, 0.05),
            TextStyle = new ImageEditorTextStyle()
            {
                FontSize = 12,
                TextColor = Colors.White
            },
            Opacity = 0.7
        });
}
```

### Example: Auto-Rotate on Load

```csharp
private void OnImageLoaded(object sender, EventArgs e)
{
    // Automatically rotate image if needed
    imageEditor.Rotate();
}
```

### Example: Apply Default Filter

```csharp
private void OnImageLoaded(object sender, EventArgs e)
{
    // Apply default enhancement
    imageEditor.ImageEffect(ImageEffect.Brightness, 0.1);
    imageEditor.ImageEffect(ImageEffect.Contrast, 0.15);
    imageEditor.SaveEdits();
}
```

### BrowseImage Event

Triggered when the browse button is clicked in the toolbar.

**Common Uses:**
- Custom image selection UI
- Image validation before loading
- Restricting image sources

```xml
<imageEditor:SfImageEditor x:Name="imageEditor"
                          Source="photo.jpg"
                          BrowseImage="OnBrowseImage" />
```

```csharp
private void OnBrowseImage(object sender, CancelEventArgs e)
{
    // Cancel default browse dialog
    e.Cancel = true;
    
    // Implement custom image selection
    CustomImagePicker();
}

private async void CustomImagePicker()
{
    var result = await FilePicker.PickAsync(new PickOptions
    {
        PickerTitle = "Select an Image",
        FileTypes = FilePickerFileType.Images
    });
    
    if (result != null)
    {
        imageEditor.Source = ImageSource.FromFile(result.FullPath);
    }
}
```

## Annotation Events

### AnnotationSelected Event

Triggered when an annotation (shape, text, or custom view) is selected.

**Common Uses:**
- Displaying annotation properties
- Enabling editing controls
- Tracking user interactions
- Implementing custom selection UI

```xml
<imageEditor:SfImageEditor x:Name="imageEditor"
                          Source="photo.jpg"
                          AnnotationSelected="OnAnnotationSelected" />
```

```csharp
private void OnAnnotationSelected(object sender, AnnotationSelectedEventArgs e)
{
    if (e.AnnotationSettings is ImageEditorShapeSettings shapeSettings)
    {
        Console.WriteLine($"Shape selected: {shapeSettings.Id}");
        
        // Highlight selected shape
        shapeSettings.Color = Colors.Blue;
        shapeSettings.StrokeThickness = 5;
    }
    else if (e.AnnotationSettings is ImageEditorTextSettings textSettings)
    {
        Console.WriteLine($"Text selected: {textSettings.Id}");
        
        // Make text larger when selected
        textSettings.TextStyle.FontSize = 20;
    }
    else if (e.AnnotationSettings is ImageEditorAnnotationSettings annotationSettings)
    {
        Console.WriteLine($"Custom view selected: {annotationSettings.Id}");
        
        // Rotate custom view
        annotationSettings.RotationAngle = 90;
    }
}
```

### AnnotationUnselected Event

Triggered when an annotation is deselected.

**Common Uses:**
- Resetting annotation appearance
- Saving annotation state
- Cleaning up resources
- Updating UI state

```xml
<imageEditor:SfImageEditor x:Name="imageEditor"
                          Source="photo.jpg"
                          AnnotationUnselected="OnAnnotationUnselected" />
```

```csharp
private void OnAnnotationUnselected(object sender, AnnotationUnselectedEventArgs e)
{
    if (e.AnnotationSettings is ImageEditorShapeSettings shapeSettings)
    {
        Console.WriteLine($"Shape unselected: {shapeSettings.Id}");
        
        // Reset appearance
        shapeSettings.IsFilled = true;
        shapeSettings.StrokeThickness = 3;
    }
    else if (e.AnnotationSettings is ImageEditorTextSettings textSettings)
    {
        Console.WriteLine($"Text unselected: {textSettings.Id}");
        
        // Save text content
        SaveTextAnnotation(textSettings);
    }
}

private void SaveTextAnnotation(ImageEditorTextSettings settings)
{
    // Save to database or storage
}
```

### AnnotationsDeserialized Event

Triggered after annotations are successfully loaded from serialization.

**Common Uses:**
- Confirming restoration
- Post-load processing
- Analytics/logging
- UI updates

```xml
<imageEditor:SfImageEditor x:Name="imageEditor"
                          Source="photo.jpg"
                          AnnotationsDeserialized="OnAnnotationsDeserialized" />
```

```csharp
private async void OnAnnotationsDeserialized(object sender, EventArgs e)
{
    await DisplayAlert("Success", "Annotations loaded successfully", "OK");
    
    // Log restoration
    Console.WriteLine("Annotations deserialized at: " + DateTime.Now);
}
```

## Save Events

### ImageSaving Event

Triggered before the image is saved. Allows customization and cancellation.

**Event Arguments:**
- `Cancel` - Cancel the save operation
- `ImageStream` - Access image data as stream
- `FileName` - Set custom filename
- `FileType` - Change file format
- `CompressionQuality` - Adjust JPEG quality (mobile)

```xml
<imageEditor:SfImageEditor x:Name="imageEditor"
                          Source="photo.jpg"
                          ImageSaving="OnImageSaving" />
```

```csharp
private void OnImageSaving(object sender, ImageSavingEventArgs args)
{
    // Cancel if no edits made
    if (!imageEditor.IsImageEdited)
    {
        args.Cancel = true;
        DisplayAlert("No Changes", "No edits to save", "OK");
        return;
    }
    
    // Set custom filename with timestamp
    args.FileName = $"Photo_{DateTime.Now:yyyyMMdd_HHmmss}";
    
    // Force PNG format
    args.FileType = ImageFileType.Png;
    
    // For JPEG, set quality (mobile only)
    #if ANDROID || IOS || MACCATALYST
    if (args.FileType == ImageFileType.Jpeg)
    {
        args.CompressionQuality = 0.85F;
    }
    #endif
    
    // Access image stream for custom processing
    Stream imageStream = args.ImageStream;
    // Upload to server, etc.
}
```

### ImageSaved Event

Triggered after the image has been successfully saved.

**Event Arguments:**
- `Location` - File path where image was saved

```xml
<imageEditor:SfImageEditor x:Name="imageEditor"
                          Source="photo.jpg"
                          ImageSaved="OnImageSaved" />
```

```csharp
private void OnImageSaved(object sender, ImageSavedEventArgs args)
{
    string savedLocation = args.Location;
    
    DisplayAlert("Success", 
        $"Image saved to:\n{savedLocation}", 
        "OK");
    
    // Log save event
    Console.WriteLine($"Image saved: {savedLocation}");
    
    // Share or upload
    ShareImage(savedLocation);
}

private void ShareImage(string filePath)
{
    // Implement sharing logic
}
```

### SavePickerOpening Event

Triggered when the save file picker dialog is about to open.

**Common Uses:**
- Using default save location instead of picker
- Custom save dialog
- Restricting save options

```xml
<imageEditor:SfImageEditor x:Name="imageEditor"
                          Source="photo.jpg"
                          SavePickerOpening="OnSavePickerOpening" />
```

```csharp
private void OnSavePickerOpening(object sender, CancelEventArgs args)
{
    // Cancel native picker, use default location
    args.Cancel = true;
    
    // Save directly to default location
    imageEditor.Save(ImageFileType.Png);
}
```

## Toolbar Events

### ToolbarItemSelected Event

Triggered when a toolbar item is selected.

**Event Arguments:**
- `ToolbarItem` - The tapped toolbar item (includes Name property)

**Common Uses:**
- Custom toolbar actions
- Analytics/tracking
- Conditional behavior
- Custom workflows

```xml
<imageEditor:SfImageEditor x:Name="imageEditor"
                          Source="photo.jpg"
                          ToolbarItemSelected="OnToolbarItemSelected" />
```

```csharp
private void OnToolbarItemSelected(object sender, ToolbarItemSelectedEventArgs e)
{
    string itemName = e.ToolbarItem.Name;
    
    Console.WriteLine($"Toolbar item tapped: {itemName}");
    
    switch (itemName)
    {
        case "Save":
            // Custom save logic
            CustomSaveDialog();
            break;
            
        case "Crop":
            // Show crop guide
            DisplayCropInstructions();
            break;
            
        case "Text":
            // Custom text input
            ShowTextInputDialog();
            break;
            
        case "Effects":
            // Custom effects panel
            ShowEffectsPreview();
            break;
    }
}

private async void CustomSaveDialog()
{
    bool result = await DisplayAlert(
        "Save Image",
        "Save your edited image?",
        "Yes",
        "No");
        
    if (result)
    {
        imageEditor.Save();
    }
}

private void DisplayCropInstructions()
{
    DisplayAlert("Crop",
        "Select crop area and tap checkmark to apply",
        "OK");
}
```

## Reset Events

### BeginReset Event

Triggered before the image is reset to its original state.

**Event Arguments:**
- `Cancel` - Cancel the reset operation

**Common Uses:**
- Confirmation dialogs
- Saving work before reset
- Conditional reset
- Analytics

```xml
<imageEditor:SfImageEditor x:Name="imageEditor"
                          Source="photo.jpg"
                          BeginReset="OnBeginReset" />
```

```csharp
private async void OnBeginReset(object sender, CancelEventArgs e)
{
    bool shouldReset = await DisplayAlert(
        "Reset Image",
        "Discard all changes and reset to original?",
        "Yes",
        "No");
        
    if (!shouldReset)
    {
        // Cancel reset
        e.Cancel = true;
        return;
    }
    
    // Log reset
    Console.WriteLine("Image reset at: " + DateTime.Now);
}
```

## Common Patterns

### Complete Event Workflow

```csharp
public partial class ImageEditorPage : ContentPage
{
    public ImageEditorPage()
    {
        InitializeComponent();
        
        // Subscribe to all events
        imageEditor.ImageLoaded += OnImageLoaded;
        imageEditor.ImageSaving += OnImageSaving;
        imageEditor.ImageSaved += OnImageSaved;
        imageEditor.AnnotationSelected += OnAnnotationSelected;
        imageEditor.ToolbarItemSelected += OnToolbarItemSelected;
        imageEditor.BeginReset += OnBeginReset;
    }
    
    private void OnImageLoaded(object sender, EventArgs e)
    {
        Console.WriteLine("1. Image loaded");
    }
    
    private void OnAnnotationSelected(object sender, AnnotationSelectedEventArgs e)
    {
        Console.WriteLine("2. Annotation selected");
    }
    
    private void OnToolbarItemSelected(object sender, ToolbarItemSelectedEventArgs e)
    {
        Console.WriteLine($"3. Toolbar: {e.ToolbarItem.Name}");
    }
    
    private void OnImageSaving(object sender, ImageSavingEventArgs e)
    {
        Console.WriteLine("4. Preparing to save");
    }
    
    private void OnImageSaved(object sender, ImageSavedEventArgs e)
    {
        Console.WriteLine($"5. Saved to: {e.Location}");
    }
    
    private void OnBeginReset(object sender, CancelEventArgs e)
    {
        Console.WriteLine("6. Resetting image");
    }
}
```

### Event-Driven Analytics

```csharp
private void TrackImageLoaded(object sender, EventArgs e)
{
    Analytics.Track("ImageEditor_ImageLoaded", new Dictionary<string, string>
    {
        { "timestamp", DateTime.Now.ToString() },
        { "source", imageEditor.Source?.ToString() }
    });
}

private void TrackAnnotationAdded(object sender, AnnotationSelectedEventArgs e)
{
    string annotationType = e.AnnotationSettings switch
    {
        ImageEditorShapeSettings => "Shape",
        ImageEditorTextSettings => "Text",
        _ => "CustomView"
    };
    
    Analytics.Track("ImageEditor_AnnotationAdded", new Dictionary<string, string>
    {
        { "type", annotationType },
        { "timestamp", DateTime.Now.ToString() }
    });
}

private void TrackSave(object sender, ImageSavedEventArgs e)
{
    Analytics.Track("ImageEditor_ImageSaved", new Dictionary<string, string>
    {
        { "location", e.Location },
        { "timestamp", DateTime.Now.ToString() }
    });
}
```

### Conditional Save Based on Quality

```csharp
private void OnImageSaving(object sender, ImageSavingEventArgs args)
{
    // Check if high-quality save is needed
    bool isHighQuality = DetermineQualityNeeded();
    
    if (isHighQuality)
    {
        args.FileType = ImageFileType.Png;  // Lossless
    }
    else
    {
        args.FileType = ImageFileType.Jpeg;
        #if ANDROID || IOS || MACCATALYST
        args.CompressionQuality = 0.7F;
        #endif
    }
}

private bool DetermineQualityNeeded()
{
    // Your logic here
    return true;
}
```

### Auto-Save with Events

```csharp
private DateTime lastSaveTime = DateTime.MinValue;
private readonly TimeSpan autoSaveInterval = TimeSpan.FromMinutes(5);

private void OnAnnotationUnselected(object sender, AnnotationUnselectedEventArgs e)
{
    // Auto-save after annotation edit
    if (DateTime.Now - lastSaveTime > autoSaveInterval)
    {
        AutoSave();
    }
}

private void AutoSave()
{
    string autoSavePath = Path.Combine(
        FileSystem.Current.AppDataDirectory,
        "autosave",
        $"autosave_{DateTime.Now:yyyyMMddHHmmss}.png");
        
    imageEditor.Save(ImageFileType.Png, 
        Path.GetDirectoryName(autoSavePath),
        Path.GetFileNameWithoutExtension(autoSavePath));
        
    lastSaveTime = DateTime.Now;
}
```

## Troubleshooting

### Issue: Event Not Firing

**Cause:** Event not subscribed or handler syntax incorrect.

**Solution:**
```xml
<!-- Ensure correct event name -->
<imageEditor:SfImageEditor ImageLoaded="OnImageLoaded" />
```

```csharp
// Or subscribe in code
imageEditor.ImageLoaded += OnImageLoaded;
```

### Issue: Multiple Event Handlers Firing

**Cause:** Event subscribed multiple times.

**Solution:**
```csharp
// Unsubscribe before resubscribing
imageEditor.ImageLoaded -= OnImageLoaded;
imageEditor.ImageLoaded += OnImageLoaded;
```

### Issue: Cannot Access Event Args Properties

**Cause:** Wrong event args type or accessing non-existent property.

**Solution:**
```csharp
private void OnAnnotationSelected(object sender, AnnotationSelectedEventArgs e)
{
    // Check type before casting
    if (e.AnnotationSettings is ImageEditorShapeSettings shape)
    {
        // Now safe to access shape properties
        var color = shape.Color;
    }
}
```

### Issue: ImageLoaded Not Firing

**Cause:** Image source not set or invalid.

**Solution:**
```csharp
// Ensure valid image source
imageEditor.Source = ImageSource.FromFile("photo.jpg");

// Or check if source is set
if (imageEditor.Source != null)
{
    // ImageLoaded should fire
}
```

## Next Steps

- **Getting Started:** [getting-started.md](getting-started.md)
- **Annotations:** [annotations.md](annotations.md)
- **Saving Images:** [save-serialization.md](save-serialization.md)
- **Undo/Redo:** [undo-redo.md](undo-redo.md)
- **Toolbar:** [toolbar.md](toolbar.md)