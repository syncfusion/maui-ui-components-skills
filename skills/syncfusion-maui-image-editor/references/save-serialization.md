# Save, Serialization & Reset

This guide covers saving edited images, serializing annotations, and resetting images to their original state in the .NET MAUI ImageEditor control.

## Table of Contents
- [Saving Images](#saving-images)
- [Image Save Events](#image-save-events)
- [Serialization](#serialization)
- [Deserialization](#deserialization)
- [Reset Functionality](#reset-functionality)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)
- [Next Steps](#next-steps)

## Saving Images

The ImageEditor allows you to save edited images in multiple formats with customizable quality settings.

### Supported File Formats

- **PNG** - Lossless compression, supports transparency
- **JPEG/JPG** - Lossy compression, smaller file size
- **BMP** - Uncompressed bitmap format

**Note:** JPG format is supported only on Android and Windows, not on macOS or iOS.

### Save Method

Use the `Save` method to save edited images:

```csharp
// Basic save (PNG format, default location)
imageEditor.Save();

// Save with specific format
imageEditor.Save(ImageFileType.Png);

// Save with custom filename and path
imageEditor.Save(ImageFileType.Png, "D:\\Pictures", "MyEditedImage");

// Save with specific size
imageEditor.Save(ImageFileType.Jpeg, null, "photo", new Size(800, 600));
```

### Complete Save Example

```xml
<Grid RowDefinitions="0.9*, 0.1*">
    <imageEditor:SfImageEditor x:Name="imageEditor" Source="photo.jpg" />
    <Button Grid.Row="1" Text="Save" Clicked="OnSaveClicked" />
</Grid>
```

```csharp
private void OnSaveClicked(object sender, EventArgs e)
{
    imageEditor.Save(ImageFileType.Png, "D:\\Syncfusion\\Pictures", "Syncfusion");
}
```

### Platform-Specific Save Locations

**Windows/MacCatalyst/iOS:**
Images are saved to `System.Environment.SpecialFolder.MyPictures`

- **Windows:** `C:\Users\{username}\Pictures`
- **MacCatalyst:** `/Users/{username}/Documents/Pictures`
- **iOS:** `/Photos/Pictures`

**Android:**
- **API 29+:** Uses `MediaStore` relative path to `Pictures` folder
- **API 28 and below:** Uses `MediaStore.Images.Media.ExternalContentUri`
- **Location:** `\Internal storage\Pictures`

### Platform Permissions

**Android:**
Add to `AndroidManifest.xml`:
```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

**macOS/iOS:**
Add to `Entitlements.plist`:
```xml
<key>com.apple.security.files.user-selected.read-write</key>
<true/>
```

Add to `Info.plist`:
```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>Pick Photos</string>
```

## Image Save Events

### ImageSaving Event

Triggered before saving the image. Allows customization and cancellation:

```xml
<imageEditor:SfImageEditor x:Name="imageEditor"
                          Source="photo.jpg"
                          ImageSaving="OnImageSaving" />
```

#### Canceling Save

```csharp
private void OnImageSaving(object sender, ImageSavingEventArgs args)
{
    if (!imageEditor.IsImageEdited)
    {
        // Cancel save if no edits made
        args.Cancel = true;
        DisplayAlert("No Changes", "No edits to save", "OK");
    }
}
```

#### Accessing Image Stream

```csharp
private Stream savedStream;

private void OnImageSaving(object sender, ImageSavingEventArgs args)
{
    // Get image as stream
    savedStream = args.ImageStream;
    
    // Use stream for custom processing
    // Upload to server, etc.
}
```

#### Custom Filename

```csharp
private void OnImageSaving(object sender, ImageSavingEventArgs args)
{
    // Set custom filename
    args.FileName = "EditedPhoto_" + DateTime.Now.ToString("yyyyMMdd_HHmmss");
}
```

#### Custom File Type

```csharp
private void OnImageSaving(object sender, ImageSavingEventArgs args)
{
    // Force PNG format
    args.FileType = ImageFileType.Png;
}
```

#### Compression Quality (Mobile)

```csharp
private void OnImageSaving(object sender, ImageSavingEventArgs args)
{
    #if ANDROID || IOS || MACCATALYST
    if (args.FileType == ImageFileType.Jpeg)
    {
        // Set JPEG compression quality (0.0 - 1.0)
        args.CompressionQuality = 0.5F;  // 50% quality
    }
    #endif
}
```

### ImageSaved Event

Triggered after the image has been saved:

```xml
<imageEditor:SfImageEditor x:Name="imageEditor"
                          Source="photo.jpg"
                          ImageSaved="OnImageSaved" />
```

```csharp
private void OnImageSaved(object sender, ImageSavedEventArgs args)
{
    // Get saved location
    string savedLocation = args.Location;
    
    DisplayAlert("Success", 
        $"Image saved to: {savedLocation}", 
        "OK");
}
```

### SavePickerOpening Event

Triggered when the save picker dialog opens:

```xml
<imageEditor:SfImageEditor x:Name="imageEditor"
                          Source="photo.jpg"
                          SavePickerOpening="OnSavePickerOpening" />
```

```csharp
private void OnSavePickerOpening(object sender, CancelEventArgs args)
{
    // Prevent picker, use default location
    args.Cancel = true;
}
```

### Check Unsaved Edits

Use `HasUnsavedEdits` property to check for pending changes:

```csharp
private void OnNavigatingAway()
{
    if (imageEditor.HasUnsavedEdits)
    {
        bool shouldSave = await DisplayAlert(
            "Unsaved Changes", 
            "Save changes before leaving?", 
            "Yes", 
            "No");
            
        if (shouldSave)
        {
            imageEditor.Save();
        }
    }
}
```

## Serialization

Serialize annotations (shapes, text, pen) to JSON for later restoration.

**Important:** Serialization is NOT applicable for custom annotation views.

### Serialize to Stream

```xml
<Grid RowDefinitions="0.9*, 0.1*">
    <imageEditor:SfImageEditor x:Name="imageEditor" Source="photo.jpg" />
    <Button Grid.Row="1" Text="Serialize" Clicked="OnSerializeClicked" />
</Grid>
```

```csharp
private void OnSerializeClicked(object sender, EventArgs e)
{
    string filePath = Path.Combine(
        FileSystem.Current.CacheDirectory, 
        "ImageEditor.xml");
        
    using (var fileStream = new FileStream(
        filePath, 
        FileMode.Create, 
        FileAccess.Write))
    {
        imageEditor.Serialize(fileStream);
    }
    
    DisplayAlert("Success", 
        $"Annotations saved to {filePath}", 
        "OK");
}
```

### When to Use Serialization

- **Save Work in Progress:** Allow users to resume editing later
- **Templates:** Save annotation layouts for reuse
- **Collaboration:** Share annotation data between users
- **Version Control:** Track annotation changes over time

## Deserialization

Reload saved annotations from serialized data:

### Deserialize from Stream

```xml
<Grid RowDefinitions="0.9*, 0.1*">
    <imageEditor:SfImageEditor x:Name="imageEditor" Source="photo.jpg" />
    <Button Grid.Row="1" Text="Deserialize" Clicked="OnDeserializeClicked" />
</Grid>
```

```csharp
private async void OnDeserializeClicked(object sender, EventArgs e)
{
    var result = await FilePicker.PickAsync();
    
    if (result != null)
    {
        using (Stream stream = await result.OpenReadAsync())
        {
            imageEditor.Deserialize(stream);
        }
        
        DisplayAlert("Success", "Annotations loaded", "OK");
    }
}
```

### AnnotationsDeserialized Event

Triggered when annotations are successfully deserialized:

```xml
<imageEditor:SfImageEditor x:Name="imageEditor"
                          Source="photo.jpg"
                          AnnotationsDeserialized="OnAnnotationsDeserialized" />
```

```csharp
private async void OnAnnotationsDeserialized(object sender, EventArgs e)
{
    await DisplayAlert("", "Annotations are deserialized", "OK");
}
```

## Reset Functionality

Reset the image to its original loaded state, discarding all edits.

### Reset Method

```xml
<Grid RowDefinitions="0.9*, 0.1*">
    <imageEditor:SfImageEditor x:Name="imageEditor" Source="photo.jpg" />
    <Button Grid.Row="1" Text="Reset" Clicked="OnResetClicked" />
</Grid>
```

```csharp
private void OnResetClicked(object sender, EventArgs e)
{
    imageEditor.Reset();
}
```

### BeginReset Event

Control reset functionality before it occurs:

```xml
<imageEditor:SfImageEditor x:Name="imageEditor"
                          Source="photo.jpg"
                          BeginReset="OnBeginReset" />
```

```csharp
private async void OnBeginReset(object sender, CancelEventArgs e)
{
    // Confirm reset
    bool shouldReset = await DisplayAlert(
        "Reset Image", 
        "Discard all changes?", 
        "Yes", 
        "No");
        
    if (!shouldReset)
    {
        e.Cancel = true;
    }
}
```

### When to Use Reset

- **Start Over:** Allow users to discard all changes easily
- **Undo All:** Provide quick way to revert everything
- **Mistake Recovery:** Help users recover from unwanted edits

## Common Patterns

### Save with Confirmation

```csharp
private async void SaveWithConfirmation()
{
    bool shouldSave = await DisplayAlert(
        "Save Image", 
        "Save edited image?", 
        "Yes", 
        "No");
        
    if (shouldSave)
    {
        imageEditor.Save(ImageFileType.Png);
        await DisplayAlert("Success", "Image saved successfully", "OK");
    }
}
```

### Auto-Save Workflow

```csharp
private void OnImageSaving(object sender, ImageSavingEventArgs args)
{
    // Auto-generate filename with timestamp
    args.FileName = $"AutoSave_{DateTime.Now:yyyyMMdd_HHmmss}";
    
    // Use high quality for JPEG
    #if ANDROID || IOS || MACCATALYST
    if (args.FileType == ImageFileType.Jpeg)
    {
        args.CompressionQuality = 0.9F;
    }
    #endif
}

private void OnImageSaved(object sender, ImageSavedEventArgs args)
{
    // Log save location
    Console.WriteLine($"Auto-saved to: {args.Location}");
}
```

### Serialize/Deserialize Workflow

```csharp
private string annotationFilePath;

private void SaveAnnotations()
{
    annotationFilePath = Path.Combine(
        FileSystem.Current.AppDataDirectory,
        "annotations",
        $"anno_{DateTime.Now:yyyyMMddHHmmss}.xml");
        
    // Ensure directory exists
    Directory.CreateDirectory(Path.GetDirectoryName(annotationFilePath));
    
    using (var stream = File.Create(annotationFilePath))
    {
        imageEditor.Serialize(stream);
    }
}

private void LoadAnnotations(string filePath)
{
    if (File.Exists(filePath))
    {
        using (var stream = File.OpenRead(filePath))
        {
            imageEditor.Deserialize(stream);
        }
    }
}
```

### Safe Reset with Backup

```csharp
private Stream backupStream;

private async void SafeReset()
{
    // Create backup first
    imageEditor.Save();
    
    bool shouldReset = await DisplayAlert(
        "Reset", 
        "This will discard all changes. Continue?", 
        "Yes", 
        "No");
        
    if (shouldReset)
    {
        imageEditor.Reset();
    }
}
```

### Quality-Based Save

```csharp
private void OnImageSaving(object sender, ImageSavingEventArgs args)
{
    #if ANDROID || IOS || MACCATALYST
    if (args.FileType == ImageFileType.Jpeg)
    {
        // Adjust quality based on image size
        var imageSize = GetImageDimensions();
        
        if (imageSize.Width > 2000 || imageSize.Height > 2000)
        {
            // Lower quality for large images
            args.CompressionQuality = 0.7F;
        }
        else
        {
            // High quality for smaller images
            args.CompressionQuality = 0.9F;
        }
    }
    #endif
}
```

## Troubleshooting

### Issue: Save Fails Silently

**Cause:** Missing platform permissions.

**Solution:** Verify permissions are added:
```xml
<!-- Android -->
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />

<!-- iOS/Mac - in Entitlements.plist -->
<key>com.apple.security.files.user-selected.read-write</key>
<true/>
```

### Issue: JPEG Not Supported

**Cause:** Using JPEG on iOS or macOS.

**Solution:**
```csharp
#if IOS || MACCATALYST
    imageEditor.Save(ImageFileType.Png);  // Use PNG instead
#else
    imageEditor.Save(ImageFileType.Jpeg);
#endif
```

### Issue: Deserialization Fails

**Cause:** Custom views in serialized data or AOT compilation limitations.

**Solution:** Serialization doesn't support custom annotation views. Only serialize shapes, text, and pen annotations.

### Issue: Reset Not Working

**Cause:** Reset event canceled.

**Solution:**
```csharp
private void OnBeginReset(object sender, CancelEventArgs e)
{
    // Don't cancel
    // e.Cancel = true;  // Remove or set to false
}
```

### Issue: Cannot Find Saved Image

**Cause:** Custom path not accessible or permissions issue.

**Solution:**
```csharp
// Use default location (no path parameter)
imageEditor.Save(ImageFileType.Png);

// Or verify path is writable
private bool IsPathWritable(string path)
{
    try
    {
        using (File.Create(Path.Combine(path, "test.tmp"), 1, FileOptions.DeleteOnClose)) { }
        return true;
    }
    catch
    {
        return false;
    }
}
```

## Next Steps

- **Undo/Redo:** [undo-redo.md](undo-redo.md)
- **Events:** [events.md](events.md)
- **Getting Started:** [getting-started.md](getting-started.md)
- **Annotations:** [annotations.md](annotations.md)