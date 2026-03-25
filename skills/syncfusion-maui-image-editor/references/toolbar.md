# Toolbar Customization

This guide covers customizing the built-in toolbar in the .NET MAUI ImageEditor control, including showing/hiding, adding custom items, and modifying appearance.

## Table of Contents
- [Toolbar Overview](#toolbar-overview)
- [Show and Hide Toolbar](#show-and-hide-toolbar)
- [Built-in Toolbar Items](#built-in-toolbar-items)
- [Customizing Toolbar Options](#customizing-toolbar-options)
- [Getting Toolbar Items](#getting-toolbar-items)
- [Customizing Toolbar Items](#customizing-toolbar-items)
- [Adding Sub-Toolbars](#adding-sub-toolbars)
- [Toolbar Events](#toolbar-events)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)
- [Next Steps](#next-steps)

## Toolbar Overview

The ImageEditor provides a built-in toolbar with comprehensive editing capabilities. You can customize the toolbar appearance, visibility, position, orientation, and add custom toolbar items.

### Toolbar Structure

The ImageEditor has two main toolbars:
1. **Header Toolbar** (Top) - Contains Browse, Undo/Redo, Zoom, and Save actions
2. **Footer Toolbar** (Bottom) - Contains editing tools (Crop, Shape, Text, etc.)

## Show and Hide Toolbar

Control toolbar visibility using the `ShowToolbar` property:

### XAML

```xml
<imageEditor:SfImageEditor Source="photo.jpg" ShowToolbar="True" />
```

### C#

```csharp
SfImageEditor imageEditor = new SfImageEditor();
imageEditor.Source = "photo.jpg";
imageEditor.ShowToolbar = true;  // Show toolbar (default)
// imageEditor.ShowToolbar = false;  // Hide toolbar
this.Content = imageEditor;
```

**When to hide toolbar:**
- Custom UI implementation
- View-only mode
- Embedded scenarios with external controls

## Built-in Toolbar Items

### Available Toolbar Item Names

The following built-in toolbar items are available:

**Header Toolbar:**
- `Browse` - Open image picker
- `Undo` - Undo last action
- `Redo` - Redo undone action
- `ZoomOut` - Decrease zoom level
- `ZoomIn` - Increase zoom level
- `Reset` - Reset image to original
- `Save` - Save edited image

**Footer Toolbar:**
- `Crop` - Crop functionality
- `Shape` - Shape annotations
- `Text` - Text annotations
- `Pen` - Freehand drawing
- `Effects` - Image effects
- `Rotate` - Rotate image
- `FlipHorizontal` - Flip horizontally
- `FlipVertical` - Flip vertically

**Image Crop Types:**
- `ellipse`, `circle`, `square`, `Free`, `Original`, `Ratio`, `Square`

**Annotation Shape Types:**
- `Rectangle`, `Circle`, `Arrow`, `Line`, `Dotted`, `DoubleArrow`, `DottedArrow`, `DottedDoubleArrow`, `Pen`, `Polygon`, `Polyline`

**Effects:**
- `Blur`, `Brightness`, `Contrast`, `Sharpen`, `Saturation`, `Hue`, `Opacity`, `Exposure`, `None`

**Important:** You cannot modify names of existing built-in toolbar items or create new items with these names.

## Customizing Toolbar Options

### Customizing Crop Types

Show only specific crop types:

**XAML:**
```xml
<imageEditor:SfImageEditor Source="photo.jpg">
    <imageEditor:SfImageEditor.ToolbarSettings>
        <imageEditor:ImageEditorToolbarSettings 
            CropTypes="Circle, Square, Free" />
    </imageEditor:SfImageEditor.ToolbarSettings>
</imageEditor:SfImageEditor>
```

**C#:**
```csharp
SfImageEditor imageEditor = new SfImageEditor();
imageEditor.Source = ImageSource.FromFile("photo.jpg");
imageEditor.ToolbarSettings.CropTypes = 
    ImageCropType.Circle | ImageCropType.Square | ImageCropType.Free;
```

**When to use:** Simplify UI by showing only relevant crop options for your use case.

### Customizing Effect Types

Show only specific effects:

**XAML:**
```xml
<imageEditor:SfImageEditor Source="photo.jpg">
    <imageEditor:SfImageEditor.ToolbarSettings>
        <imageEditor:ImageEditorToolbarSettings 
            EffectTypes="Hue, Blur, Brightness" />
    </imageEditor:SfImageEditor.ToolbarSettings>
</imageEditor:SfImageEditor>
```

**C#:**
```csharp
SfImageEditor imageEditor = new SfImageEditor();
imageEditor.Source = ImageSource.FromFile("photo.jpg");
imageEditor.ToolbarSettings.EffectTypes = 
    ImageEffect.Hue | ImageEffect.Blur | ImageEffect.Brightness;
```

### Customizing Shape Types

Show only specific shapes:

**XAML:**
```xml
<imageEditor:SfImageEditor Source="photo.jpg">
    <imageEditor:SfImageEditor.ToolbarSettings>
        <imageEditor:ImageEditorToolbarSettings 
            Shapes="Circle, Line, DottedArrow, DoubleArrow" />
    </imageEditor:SfImageEditor.ToolbarSettings>
</imageEditor:SfImageEditor>
```

**C#:**
```csharp
SfImageEditor imageEditor = new SfImageEditor();
imageEditor.Source = ImageSource.FromFile("photo.jpg");
imageEditor.ToolbarSettings.Shapes = 
    AnnotationShape.Circle | 
    AnnotationShape.Line | 
    AnnotationShape.DottedArrow | 
    AnnotationShape.DoubleArrow;
```

### Customizing Color Palette

Change default colors in toolbar color pickers:

**XAML:**
```xml
<imageEditor:SfImageEditor Source="photo.jpg">
    <imageEditor:SfImageEditor.ToolbarSettings>
        <imageEditor:ImageEditorToolbarSettings>
            <imageEditor:ImageEditorToolbarSettings.ColorPalette>
                <Color>Yellow</Color>
                <Color>Pink</Color>
                <Color>Violet</Color>
                <Color>Orange</Color>
                <Color>Cyan</Color>
            </imageEditor:ImageEditorToolbarSettings.ColorPalette>
        </imageEditor:ImageEditorToolbarSettings>
    </imageEditor:SfImageEditor.ToolbarSettings>
</imageEditor:SfImageEditor>
```

**C#:**
```csharp
SfImageEditor imageEditor = new SfImageEditor();
imageEditor.Source = ImageSource.FromFile("photo.jpg");

// Clear default colors and add custom ones
imageEditor.ToolbarSettings.ColorPalette.Clear();
imageEditor.ToolbarSettings.ColorPalette.Add(Colors.Yellow);
imageEditor.ToolbarSettings.ColorPalette.Add(Colors.Pink);
imageEditor.ToolbarSettings.ColorPalette.Add(Colors.Violet);
imageEditor.ToolbarSettings.ColorPalette.Add(Colors.Orange);
imageEditor.ToolbarSettings.ColorPalette.Add(Colors.Cyan);
```

**Note:** Color palette colors are common for Text, Shape, and Pen annotations.

## Getting Toolbar Items

### Get Item from Header Toolbar

The header toolbar contains three groups:

```csharp
SfImageEditor imageEditor = new SfImageEditor();
imageEditor.Source = ImageSource.FromFile("photo.jpg");

// Get header toolbar (index 0)
ImageEditorToolbar headerToolbar = imageEditor.Toolbars[0];

// Get browse group and item
ImageEditorToolbarGroupItem browseGroup = 
    (ImageEditorToolbarGroupItem)headerToolbar.ToolbarItems[0];
ImageEditorToolbarItem browseItem = browseGroup.Items.FirstOrDefault();

// Get save group and save item
ImageEditorToolbarGroupItem saveGroup = 
    (ImageEditorToolbarGroupItem)headerToolbar.ToolbarItems[2];
ImageEditorToolbarItem saveItem = 
    saveGroup.Items.FirstOrDefault(i => i.Name == "Save");
```

### Get Item from Footer Toolbar

The footer toolbar contains main editing tools:

```csharp
SfImageEditor imageEditor = new SfImageEditor();
imageEditor.Source = ImageSource.FromFile("photo.jpg");

// Get footer toolbar (index 1)
ImageEditorToolbar footerToolbar = imageEditor.Toolbars[1];

// Get text item
ImageEditorToolbarItem textItem = 
    (ImageEditorToolbarItem)footerToolbar.ToolbarItems
        .FirstOrDefault(i => i.Name == "Text");

// Get crop item
ImageEditorToolbarItem cropItem = 
    (ImageEditorToolbarItem)footerToolbar.ToolbarItems
        .FirstOrDefault(i => i.Name == "Crop");
```

### Get Sub-Toolbar Items

Sub-toolbars are nested within toolbar items:

```csharp
SfImageEditor imageEditor = new SfImageEditor();
imageEditor.Source = ImageSource.FromFile("photo.jpg");

ImageEditorToolbar footerToolbar = imageEditor.Toolbars[1];

// Get shape item
ImageEditorToolbarItem shapeItem = 
    (ImageEditorToolbarItem)footerToolbar.ToolbarItems
        .FirstOrDefault(i => i.Name == "Shape");

// Get shape sub-toolbar
ImageEditorToolbar subtoolbar = shapeItem.SubToolbars[0];

// Get arrow item from sub-toolbar
ImageEditorToolbarItem arrowItem = 
    (ImageEditorToolbarItem)subtoolbar.ToolbarItems
        .FirstOrDefault(i => i.Name == "Arrow");
```

## Customizing Toolbar Items

### Enable/Disable Toolbar Items

```csharp
SfImageEditor imageEditor = new SfImageEditor();
imageEditor.Source = ImageSource.FromFile("photo.jpg");

// Disable browse item
ImageEditorToolbar headerToolbar = imageEditor.Toolbars[0];
ImageEditorToolbarGroupItem browseGroup = 
    (ImageEditorToolbarGroupItem)headerToolbar.ToolbarItems[0];
ImageEditorToolbarItem browseItem = browseGroup.Items.FirstOrDefault();
browseItem.IsEnabled = false;

// Disable save item
ImageEditorToolbarGroupItem saveGroup = 
    (ImageEditorToolbarGroupItem)headerToolbar.ToolbarItems[2];
ImageEditorToolbarItem saveItem = 
    saveGroup.Items.FirstOrDefault(i => i.Name == "Save");
saveItem.IsEnabled = false;
```

**When to use:**
- Restrict certain operations
- Progressive disclosure (enable features as needed)
- Role-based permissions

### Customize Toolbar Item View

Replace toolbar item icons with custom views:

```csharp
SfImageEditor imageEditor = new SfImageEditor();
imageEditor.Source = ImageSource.FromFile("photo.jpg");

ImageEditorToolbar headerToolbar = imageEditor.Toolbars[0];
ImageEditorToolbarGroupItem saveGroup = 
    (ImageEditorToolbarGroupItem)headerToolbar.ToolbarItems[2];
ImageEditorToolbarItem saveItem = 
    saveGroup.Items.FirstOrDefault(i => i.Name == "Save");

// Custom image view
Image customIcon = new Image();
customIcon.Source = ImageSource.FromFile("custom_save_icon.png");
saveItem.View = customIcon;
```

**When to use:**
- Branding with custom icons
- Implementing specific design system
- Adding visual indicators

## Adding Sub-Toolbars

Create sub-toolbars for organizing related actions:

```csharp
SfImageEditor imageEditor = new SfImageEditor();
imageEditor.Source = ImageSource.FromFile("photo.jpg");

ImageEditorToolbar footerToolbar = imageEditor.Toolbars[1];
ImageEditorToolbarItem cropItem = 
    (ImageEditorToolbarItem)footerToolbar.ToolbarItems
        .FirstOrDefault(i => i.Name == "Crop");

// Add custom sub-toolbar
ImageEditorToolbar customSubToolbar = new ImageEditorToolbar();
// Configure sub-toolbar items...
cropItem.SubToolbars.Add(customSubToolbar);
```

## Toolbar Events

### ToolbarItemTapped Event

Handle toolbar item clicks:

```xml
<imageEditor:SfImageEditor x:Name="imageEditor"
                          Source="photo.jpg"
                          ToolbarItemSelected="OnToolbarItemSelected" />
```

```csharp
private void OnToolbarItemSelected(object sender, ToolbarItemSelectedEventArgs e)
{
    // Get tapped item name
    string itemName = e.ToolbarItem.Name;
    
    if (itemName == "Save")
    {
        // Custom save logic
        Console.WriteLine("Save button tapped");
    }
    else if (itemName == "Crop")
    {
        // Custom crop logic
        Console.WriteLine("Crop button tapped");
    }
}
```

## Common Patterns

### Simplified Toolbar (Essential Tools Only)

```csharp
SfImageEditor imageEditor = new SfImageEditor();
imageEditor.Source = ImageSource.FromFile("photo.jpg");

// Show only essential crop types
imageEditor.ToolbarSettings.CropTypes = 
    ImageCropType.Square | ImageCropType.Original | ImageCropType.Free;

// Show only common shapes
imageEditor.ToolbarSettings.Shapes = 
    AnnotationShape.Rectangle | 
    AnnotationShape.Circle | 
    AnnotationShape.Arrow;

// Show only key effects
imageEditor.ToolbarSettings.EffectTypes = 
    ImageEffect.Brightness | 
    ImageEffect.Contrast | 
    ImageEffect.Saturation;
```

### Brand Colors Palette

```csharp
private void SetBrandColorPalette()
{
    imageEditor.ToolbarSettings.ColorPalette.Clear();
    
    // Add brand colors
    imageEditor.ToolbarSettings.ColorPalette.Add(Color.FromArgb("#FF0066"));
    imageEditor.ToolbarSettings.ColorPalette.Add(Color.FromArgb("#00CCFF"));
    imageEditor.ToolbarSettings.ColorPalette.Add(Color.FromArgb("#FFCC00"));
    imageEditor.ToolbarSettings.ColorPalette.Add(Color.FromArgb("#33FF66"));
    imageEditor.ToolbarSettings.ColorPalette.Add(Color.FromArgb("#9933FF"));
}
```

### Conditional Toolbar Items

```csharp
private void ConfigureToolbarForUserRole(string userRole)
{
    ImageEditorToolbar footerToolbar = imageEditor.Toolbars[1];
    
    if (userRole == "Viewer")
    {
        // Disable all editing tools
        foreach (var item in footerToolbar.ToolbarItems)
        {
            if (item is ImageEditorToolbarItem toolbarItem)
            {
                toolbarItem.IsEnabled = false;
            }
        }
    }
    else if (userRole == "BasicEditor")
    {
        // Enable only crop and text
        var cropItem = footerToolbar.ToolbarItems
            .FirstOrDefault(i => ((ImageEditorToolbarItem)i).Name == "Crop");
        var textItem = footerToolbar.ToolbarItems
            .FirstOrDefault(i => ((ImageEditorToolbarItem)i).Name == "Text");
            
        ((ImageEditorToolbarItem)cropItem).IsEnabled = true;
        ((ImageEditorToolbarItem)textItem).IsEnabled = true;
    }
    // "AdvancedEditor" gets all tools (default)
}
```

### Custom Toolbar Click Handler

```csharp
private void OnToolbarItemSelected(object sender, ToolbarItemSelectedEventArgs e)
{
    switch (e.ToolbarItem.Name)
    {
        case "Save":
            SaveWithCustomDialog();
            break;
            
        case "Crop":
            ShowCropGuide();
            break;
            
        case "Text":
            PromptForTextInput();
            break;
            
        case "Effects":
            ShowEffectsPreview();
            break;
    }
}

private async void SaveWithCustomDialog()
{
    bool result = await DisplayAlert(
        "Save Image", 
        "Save changes to this image?", 
        "Yes", 
        "No");
        
    if (result)
    {
        imageEditor.Save();
    }
}
```

### Photo-Specific Toolbar

```csharp
private void ConfigurePhotoEditingToolbar()
{
    // Limit to photo-relevant crop ratios
    imageEditor.ToolbarSettings.CropTypes = 
        ImageCropType.Original | 
        ImageCropType.Square |      // 1:1 (Instagram)
        ImageCropType.Ratio3x2 |    // 3:2 (Standard photo)
        ImageCropType.Ratio4x3 |    // 4:3 (Classic)
        ImageCropType.Ratio16x9;    // 16:9 (Widescreen)
    
    // Photo enhancement effects
    imageEditor.ToolbarSettings.EffectTypes = 
        ImageEffect.Brightness | 
        ImageEffect.Contrast | 
        ImageEffect.Saturation | 
        ImageEffect.Sharpen |
        ImageEffect.Hue;
        
    // Minimal shapes for photo markup
    imageEditor.ToolbarSettings.Shapes = 
        AnnotationShape.Circle | 
        AnnotationShape.Rectangle | 
        AnnotationShape.Arrow;
}
```

## Troubleshooting

### Issue: Toolbar Not Showing

**Cause:** `ShowToolbar` set to false.

**Solution:**
```csharp
imageEditor.ShowToolbar = true;
```

### Issue: Custom Colors Not Appearing

**Cause:** Colors added without clearing defaults.

**Solution:**
```csharp
// Clear first, then add
imageEditor.ToolbarSettings.ColorPalette.Clear();
imageEditor.ToolbarSettings.ColorPalette.Add(Colors.Red);
```

### Issue: Cannot Find Toolbar Item

**Cause:** Incorrect toolbar index or item name.

**Solution:**
```csharp
// Verify toolbar structure
var headerToolbar = imageEditor.Toolbars[0];  // Header
var footerToolbar = imageEditor.Toolbars[1];  // Footer

// Use correct item name (case-sensitive)
var item = footerToolbar.ToolbarItems
    .FirstOrDefault(i => ((ImageEditorToolbarItem)i).Name == "Crop");
```

### Issue: Toolbar Item Disabled Unexpectedly

**Cause:** Item was programmatically disabled.

**Solution:**
```csharp
// Re-enable item
toolbarItem.IsEnabled = true;
```

### Issue: Sub-Toolbar Not Opening

**Cause:** Sub-toolbar not properly configured or empty.

**Solution:**
```csharp
// Verify sub-toolbar exists and has items
if (item.SubToolbars != null && item.SubToolbars.Count > 0)
{
    var subToolbar = item.SubToolbars[0];
    // Configure sub-toolbar
}
```

## Next Steps

- **Cropping & Transformations:** [crop-transform.md](crop-transform.md)
- **Image Effects:** [filters-effects.md](filters-effects.md)
- **Annotations:** [annotations.md](annotations.md)
- **Events:** [events.md](events.md)
- **Getting Started:** [getting-started.md](getting-started.md)