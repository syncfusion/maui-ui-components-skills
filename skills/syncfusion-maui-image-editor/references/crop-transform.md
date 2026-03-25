# Cropping & Transformations

This guide covers image cropping with various aspect ratios and image transformations (rotation and flipping) in the .NET MAUI ImageEditor.

## Table of Contents
- [Image Cropping Overview](#image-cropping-overview)
- [Crop Types](#crop-types)
- [Manual Cropping](#manual-cropping)
- [Saving and Canceling Crops](#saving-and-canceling-crops)
- [Image Rotation](#image-rotation)
- [Image Flipping](#image-flipping)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)
- [Next Steps](#next-steps)

## Image Cropping Overview

The ImageEditor provides flexible cropping capabilities allowing users to select and crop specific sections of an image using predefined aspect ratios or freehand selection.

### Cropping Methods

Two primary approaches:
1. **Interactive Cropping:** Enable cropping UI for visual selection
2. **Programmatic Cropping:** Specify crop region via code

### Crop Workflow

```csharp
// 1. Enable cropping mode
imageEditor.Crop(ImageCropType.Square);

// 2. User adjusts crop region (or done programmatically)

// 3. Save the crop
imageEditor.SaveEdits();

// OR Cancel to revert
imageEditor.CancelEdits();
```

## Crop Types

### 1. Free Crop (Custom)

Allows free-form cropping by dragging and resizing:

```csharp
imageEditor.Crop(ImageCropType.Free);
```

```xml
<!-- XAML Example with Button -->
<Grid RowDefinitions="*, Auto">
    <imageEditor:SfImageEditor x:Name="imageEditor" Source="photo.jpg" />
    <Button Grid.Row="1" Text="Free Crop" Clicked="OnFreeCropClicked" />
</Grid>
```

```csharp
private void OnFreeCropClicked(object sender, EventArgs e)
{
    imageEditor.Crop(ImageCropType.Free);
}
```

**When to use:** General cropping where users need flexibility without aspect ratio constraints.

### 2. Original Crop

Crops image while maintaining its original aspect ratio:

```csharp
imageEditor.Crop(ImageCropType.Original);
```

**When to use:** Preserving the original width-to-height ratio while removing edges.

### 3. Square Crop

Perfect square aspect ratio (1:1):

```csharp
imageEditor.Crop(ImageCropType.Square);
```

**When to use:** Profile pictures, avatars, Instagram-style images, or any square format requirement.

### 4. Ratio Crop (Custom Aspect Ratios)

Crop with specific width:height ratio:

```csharp
// 16:9 aspect ratio (widescreen)
imageEditor.Crop(16, 9);

// 4:3 aspect ratio (standard)
imageEditor.Crop(4, 3);

// 3:2 aspect ratio (photography)
imageEditor.Crop(3, 2);

// 21:9 aspect ratio (cinematic)
imageEditor.Crop(21, 9);

// Custom 5:4 ratio
imageEditor.Crop(5, 4);
```

**Default ratio:** If you call `imageEditor.Crop(ImageCropType.Ratio)` without parameters, it defaults to 4:3.

**Common aspect ratios:**
- **16:9** — Widescreen, video content, modern displays
- **4:3** — Traditional photos, standard displays
- **3:2** — DSLR camera standard
- **1:1** — Square (use `ImageCropType.Square` instead)
- **21:9** — Ultra-widescreen, cinematic

### 5. Circle Crop

Circular crop with 1:1 ratio:

```csharp
imageEditor.Crop(ImageCropType.Circle);
```

**When to use:** Profile pictures, avatars, circular badges, or any circular visual element.

**Note:** The resulting image is still rectangular with circular content (transparent/white corners depending on format).

### 6. Ellipse Crop

Free-form elliptical crop:

```csharp
imageEditor.Crop(ImageCropType.Ellipse);
```

**When to use:** Oval portraits, artistic cropping, or designs requiring non-square circular shapes.

## Manual Cropping

### Crop by Rectangle Bounds

Specify exact crop region using `Rect`:

```csharp
// Crop(Rect rect, bool isEllipse = false)
// Rect(x, y, width, height) - coordinates in pixels

// Rectangular crop
imageEditor.Crop(new Rect(50, 50, 300, 200));
imageEditor.SaveEdits();

// Elliptical crop within rectangle bounds
imageEditor.Crop(new Rect(100, 100, 200, 150), isEllipse: true);
imageEditor.SaveEdits();
```

**Parameters:**
- **x, y:** Top-left corner coordinates
- **width, height:** Crop region dimensions
- **isEllipse:** If `true`, creates elliptical crop; `false` (default) for rectangular

**Example — Center Crop:**
```csharp
// Get current image size
Size imageSize = imageEditor.ImageRenderedSize;

// Calculate centered 300x300 crop region
double cropSize = 300;
double x = (imageSize.Width - cropSize) / 2;
double y = (imageSize.Height - cropSize) / 2;

imageEditor.Crop(new Rect(x, y, cropSize, cropSize));
imageEditor.SaveEdits();
```

### Programmatic Aspect Ratio Selection

Select crop ratio by specifying width and height values:

```csharp
// 1:1 square
imageEditor.Crop(1, 1);

// 16:9 widescreen
imageEditor.Crop(16, 9);

// 9:16 portrait (social media stories)
imageEditor.Crop(9, 16);
```

## Saving and Canceling Crops

### Save Cropped Image

After user selects crop region, apply the crop:

```csharp
// Apply crop operation
imageEditor.SaveEdits();
```

**What happens:**
- Crops the image to selected region
- Updates the displayed image
- Adds operation to undo history

### Cancel Crop Operation

Discard cropping and revert to previous state:

```csharp
// Cancel current crop operation
imageEditor.CancelEdits();
```

**What happens:**
- Removes crop selection overlay
- Restores image to state before `Crop()` was called
- No changes added to history

### Example — Crop with Confirmation

```xml
<Grid RowDefinitions="*, Auto, Auto">
    <imageEditor:SfImageEditor x:Name="imageEditor" Source="photo.jpg" />
    
    <HorizontalStackLayout Grid.Row="1" Spacing="10" Padding="10" HorizontalOptions="Center">
        <Button Text="Free Crop" Clicked="OnFreeCropClicked" />
        <Button Text="Square" Clicked="OnSquareCropClicked" />
        <Button Text="16:9" Clicked="OnWidesCropClicked" />
    </HorizontalStackLayout>
    
    <HorizontalStackLayout Grid.Row="2" Spacing="10" Padding="10" HorizontalOptions="Center">
        <Button Text="✓ Apply" Clicked="OnApplyCropClicked" BackgroundColor="LightGreen" />
        <Button Text="✗ Cancel" Clicked="OnCancelCropClicked" BackgroundColor="LightCoral" />
    </HorizontalStackLayout>
</Grid>
```

```csharp
private void OnFreeCropClicked(object sender, EventArgs e) 
    => imageEditor.Crop(ImageCropType.Free);

private void OnSquareCropClicked(object sender, EventArgs e) 
    => imageEditor.Crop(ImageCropType.Square);

private void OnWidesCropClicked(object sender, EventArgs e) 
    => imageEditor.Crop(16, 9);

private void OnApplyCropClicked(object sender, EventArgs e) 
    => imageEditor.SaveEdits();

private void OnCancelCropClicked(object sender, EventArgs e) 
    => imageEditor.CancelEdits();
```

## Image Rotation

### Rotate 90 Degrees Clockwise

```csharp
// Rotate 90° clockwise
imageEditor.Rotate();
```

Each call rotates the image 90 degrees clockwise. To rotate multiple times:

```csharp
// Rotate 180° (upside down)
imageEditor.Rotate();
imageEditor.Rotate();

// Rotate 270° (or 90° counter-clockwise)
imageEditor.Rotate();
imageEditor.Rotate();
imageEditor.Rotate();
```

**Note:** There is no direct API to specify rotation angle (e.g., 45°, 30°). Only 90° increments are supported.

### Rotation with Button Example

```xml
<Grid RowDefinitions="*, Auto">
    <imageEditor:SfImageEditor x:Name="imageEditor" Source="photo.jpg" />
    <Button Grid.Row="1" Text="Rotate 90°" Clicked="OnRotateClicked" />
</Grid>
```

```csharp
private void OnRotateClicked(object sender, EventArgs e)
{
    imageEditor.Rotate();
}
```

### Programmatic Rotation Angles

```csharp
// No rotation needed
// (Keep original)

// Rotate 90° clockwise
imageEditor.Rotate();

// Rotate180° (flip vertically and horizontally)
imageEditor.Rotate();
imageEditor.Rotate();

// Rotate 270° clockwise (or 90° counter-clockwise)
for (int i = 0; i < 3; i++)
    imageEditor.Rotate();
```

## Image Flipping

The `Flip` method creates a mirror image horizontally or vertically.

### Flip Horizontally

```csharp
imageEditor.Flip(ImageFlipDirection.Horizontal);
```

Creates a left-right mirror image.

### Flip Vertically

```csharp
imageEditor.Flip(ImageFlipDirection.Vertical);
```

Creates an upside-down mirror image.

### Flip Example with Buttons

```xml
<Grid RowDefinitions="*, Auto">
    <imageEditor:SfImageEditor x:Name="imageEditor" Source="photo.jpg" />
    <HorizontalStackLayout Grid.Row="1" Spacing="10" Padding="10" HorizontalOptions="Center">
        <Button Text="Flip Horizontal" Clicked="OnFlipHorizontalClicked" />
        <Button Text="Flip Vertical" Clicked="OnFlipVerticalClicked" />
    </HorizontalStackLayout>
</Grid>
```

```csharp
private void OnFlipHorizontalClicked(object sender, EventArgs e)
{
    imageEditor.Flip(ImageFlipDirection.Horizontal);
}

private void OnFlipVerticalClicked(object sender, EventArgs e)
{
    imageEditor.Flip(ImageFlipDirection.Vertical);
}
```

## Common Patterns

### Pattern 1: Profile Picture Crop (Circular)

```csharp
// For profile picture editing
private async void SetupProfilePictureCrop()
{
    // Load user's photo
    imageEditor.Source = await LoadUserPhoto();
    
    // Enable circular crop
    imageEditor.Crop(ImageCropType.Circle);
    
    // User adjusts, then apply
    // (Typically triggered by a "Save" button)
}

private void OnSaveProfilePictureClicked(object sender, EventArgs e)
{
    imageEditor.SaveEdits();
    await imageEditor.Save(ImageFileType.Png, "profile.jpg");
}
```

### Pattern 2: Social Media Crop Options

```csharp
// Instagram square, story portrait, widescreen posts
private void OnInstagramSquareClicked(object sender, EventArgs e)
{
    imageEditor.Crop(ImageCropType.Square); // 1:1
}

private void OnInstagramStoryClicked(object sender, EventArgs e)
{
    imageEditor.Crop(9, 16); // 9:16 portrait
}

private void OnYouTubeThumbnailClicked(object sender, EventArgs e)
{
    imageEditor.Crop(16, 9); // 16:9 widescreen
}
```

### Pattern 3: Document Scanning with Rotation Correction

```csharp
// User scans document, may need rotation to correct orientation
private async void OnDocumentScanned(Stream imageStream)
{
    imageEditor.Source = ImageSource.FromStream(() => imageStream);
    
    // Auto-detect or let user rotate
    await DisplayRotationOptions();
}

private async Task DisplayRotationOptions()
{
    string action = await DisplayActionSheet(
        "Adjust document orientation", 
        "correct", 
        null, 
        "Rotate 90°", "Rotate 180°", "Rotate 270°");
    
    switch (action)
    {
        case "Rotate 90°":
            imageEditor.Rotate();
            break;
        case "Rotate 180°":
            imageEditor.Rotate();
            imageEditor.Rotate();
            break;
        case "Rotate 270°":
            for (int i = 0; i < 3; i++) imageEditor.Rotate();
            break;
    }
}
```

### Pattern 4: Centered Crop with Fixed Size

```csharp
// Crop a specific size from center (e.g., 400x300 thumbnail)
private void CropCenteredRegion(double width, double height)
{
    Size imageSize = imageEditor.ImageRenderedSize;
    
    double x = (imageSize.Width - width) / 2;
    double y = (imageSize.Height - height) / 2;
    
    imageEditor.Crop(new Rect(x, y, width, height));
    imageEditor.SaveEdits();
}

// Usage
CropCenteredRegion(400, 300);
```

## Troubleshooting

### Issue: Crop region not visible

**Problem:** Called `Crop()` but nothing appears

**Solution:** Ensure the image is loaded first:
```csharp
imageEditor.ImageLoaded += (s, e) =>
{
    imageEditor.Crop(ImageCropType.Square);
};
```

### Issue: SaveEdits() has no effect

**Problem:** `SaveEdits()` doesn't apply the crop

**Solution:** Crop must be active before calling `SaveEdits()`. Ensure you called `Crop()` first:
```csharp
imageEditor.Crop(ImageCropType.Free);
// Wait for user adjustment
imageEditor.SaveEdits(); // Now applies crop
```

### Issue: Manual crop bounds incorrect

**Problem:** `Crop(new Rect(...))` crops wrong region

**Solution:** Use `ImageRenderedSize`, not `OriginalImageSize`. Bounds are relative to rendered size:
```csharp
Size size = imageEditor.ImageRenderedSize; // ✓
// NOT: Size size = imageEditor.OriginalImageSize;
```

### Issue: Cannot rotate to specific angle

**Problem:** Need 45° rotation, but only 90° increments work

**Solution:** ImageEditor only supports 90° increments. For custom angles, consider:
1. Pre-processing image before loading
2. Using a different image manipulation library for specific angles

### Issue: Quality loss after multiple rotations

**Problem:** Image quality degrades after rotating several times

**Solution:** Rotations are lossless transformations. If quality issues occur:
- Save at higher quality settings (`Save(ImageFileType.Png, path, filename, imagesize)`)
- Minimize number of edits before final save

## Next Steps

- **Add annotations:** Learn about shapes, text, and freehand drawing in [annotations.md](annotations.md)
- **Apply filters:** Enhance images with effects in [filters-effects.md](filters-effects.md)
- **Save edited images:** Export and save options in [save-serialization.md](save-serialization.md)
- **Handle events:** Track cropping operations in [events.md](events.md)