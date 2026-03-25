# Images and Tables

## Table of Contents
- [Overview](#overview)
- [Image Insertion](#image-insertion)
- [Table Insertion](#table-insertion)
- [Platform-Specific Considerations](#platform-specific-considerations)
- [Best Practices](#best-practices)

## Overview

The Rich Text Editor supports inserting images (JPEG, PNG) and tables into content, enabling creation of rich, structured documents. Images can be added from galleries, files, streams, or URLs, while tables provide structured data presentation.

## Image Insertion

### Enable Image Toolbar Item

Include the Image toolbar item to allow users to insert images:

```xml
<rte:SfRichTextEditor ShowToolbar="True">
    <rte:SfRichTextEditor.ToolbarItems>
        <rte:RichTextToolbarItem Type="Bold" />
        <rte:RichTextToolbarItem Type="Italic" />
        <rte:RichTextToolbarItem Type="Separator" />
        <rte:RichTextToolbarItem Type="Image" />
    </rte:SfRichTextEditor.ToolbarItems>
</rte:SfRichTextEditor>
```

### ImageRequested Event

When the user taps the Image toolbar button, the `ImageRequested` event fires. Handle this event to provide the image source.

**XAML:**
```xml
<rte:SfRichTextEditor x:Name="richTextEditor"
                      ShowToolbar="True"
                      ImageRequested="OnImageRequested" />
```

**C#:**
```csharp
private async void OnImageRequested(object sender, RichTextEditorImageRequestedEventArgs e)
{
    e.IsHandled = true;
    
    // Create image source configuration
    RichTextEditorImageSource imageSource = new RichTextEditorImageSource
    {
        ImageFormat = RichTextEditorImageFormat.Base64,
        Source = ImageSource.FromUri(new Uri("https://example.com/image.jpg")),
        Width = 300,
        Height = 200
    };
    
    // Insert the image
    richTextEditor.InsertImage(imageSource);
}
```

### Insert Image from URI

```csharp
private async void OnImageRequested(object sender, RichTextEditorImageRequestedEventArgs e)
{
    e.IsHandled = true;
    
    RichTextEditorImageSource imageSource = new RichTextEditorImageSource
    {
        ImageFormat = RichTextEditorImageFormat.Base64,
        Source = ImageSource.FromUri(new Uri("https://aka.ms/campus.jpg")),
        Width = 250,
        Height = 100
    };
    
    richTextEditor.InsertImage(imageSource);
}
```

### Insert Image from File Picker

```csharp
private async void OnImageRequested(object sender, RichTextEditorImageRequestedEventArgs e)
{
    e.IsHandled = true;
    
    try
    {
        // Use MAUI file picker
        var result = await FilePicker.PickAsync(new PickOptions
        {
            FileTypes = FilePickerFileType.Images,
            PickerTitle = "Select an image"
        });
        
        if (result != null)
        {
            // Load image from file
            var stream = await result.OpenReadAsync();
            
            RichTextEditorImageSource imageSource = new RichTextEditorImageSource
            {
                ImageFormat = RichTextEditorImageFormat.Base64,
                Source = ImageSource.FromStream(() => stream),
                Width = 400,
                Height = 300
            };
            
            richTextEditor.InsertImage(imageSource);
        }
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", $"Failed to load image: {ex.Message}", "OK");
    }
}
```

### Insert Image from Media Gallery

```csharp
private async void OnImageRequested(object sender, RichTextEditorImageRequestedEventArgs e)
{
    e.IsHandled = true;
    
    try
    {
        // Use MAUI Media Picker
        var photo = await MediaPicker.PickPhotoAsync();
        
        if (photo != null)
        {
            var stream = await photo.OpenReadAsync();
            
            RichTextEditorImageSource imageSource = new RichTextEditorImageSource
            {
                ImageFormat = RichTextEditorImageFormat.Base64,
                Source = ImageSource.FromStream(() => stream),
                Width = 350,
                Height = 250
            };
            
            richTextEditor.InsertImage(imageSource);
        }
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", $"Failed to pick photo: {ex.Message}", "OK");
    }
}
```

### Insert Image from Camera

```csharp
private async void OnCaptureImageRequested(object sender, EventArgs e)
{
    try
    {
        // Capture photo using camera
        var photo = await MediaPicker.CapturePhotoAsync();
        
        if (photo != null)
        {
            var stream = await photo.OpenReadAsync();
            
            RichTextEditorImageSource imageSource = new RichTextEditorImageSource
            {
                ImageFormat = RichTextEditorImageFormat.Base64,
                Source = ImageSource.FromStream(() => stream),
                Width = 400,
                Height = 300
            };
            
            richTextEditor.InsertImage(imageSource);
        }
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", $"Failed to capture photo: {ex.Message}", "OK");
    }
}
```

### Insert Image from Resource

```csharp
private void InsertResourceImage()
{
    RichTextEditorImageSource imageSource = new RichTextEditorImageSource
    {
        ImageFormat = RichTextEditorImageFormat.Base64,
        Source = ImageSource.FromResource("MyApp.Resources.Images.logo.png"),
        Width = 200,
        Height = 100
    };
    
    richTextEditor.InsertImage(imageSource);
}
```

### Insert Image with Custom Sizing

```csharp
private void InsertCustomSizedImage(string imageUrl, int width, int height)
{
    RichTextEditorImageSource imageSource = new RichTextEditorImageSource
    {
        ImageFormat = RichTextEditorImageFormat.Base64,
        Source = ImageSource.FromUri(new Uri(imageUrl)),
        Width = width,
        Height = height
    };
    
    richTextEditor.InsertImage(imageSource);
}

// Usage
InsertCustomSizedImage("https://example.com/image.jpg", 500, 300);
```

### Image Source Options Dialog

```csharp
private async void OnImageRequested(object sender, RichTextEditorImageRequestedEventArgs e)
{
    e.IsHandled = true;
    
    string action = await DisplayActionSheet(
        "Insert Image", 
        "Cancel", 
        null, 
        "From Gallery", 
        "From Camera", 
        "From URL"
    );
    
    switch (action)
    {
        case "From Gallery":
            await InsertFromGallery();
            break;
        case "From Camera":
            await InsertFromCamera();
            break;
        case "From URL":
            await InsertFromUrl();
            break;
    }
}

private async Task InsertFromGallery()
{
    var photo = await MediaPicker.PickPhotoAsync();
    if (photo != null)
    {
        var stream = await photo.OpenReadAsync();
        InsertImage(stream, 350, 250);
    }
}

private async Task InsertFromCamera()
{
    var photo = await MediaPicker.CapturePhotoAsync();
    if (photo != null)
    {
        var stream = await photo.OpenReadAsync();
        InsertImage(stream, 400, 300);
    }
}

private async Task InsertFromUrl()
{
    string url = await DisplayPromptAsync("Image URL", "Enter image URL:");
    if (!string.IsNullOrEmpty(url))
    {
        RichTextEditorImageSource imageSource = new RichTextEditorImageSource
        {
            ImageFormat = RichTextEditorImageFormat.Base64,
            Source = ImageSource.FromUri(new Uri(url)),
            Width = 400,
            Height = 300
        };
        richTextEditor.InsertImage(imageSource);
    }
}

private void InsertImage(Stream stream, int width, int height)
{
    RichTextEditorImageSource imageSource = new RichTextEditorImageSource
    {
        ImageFormat = RichTextEditorImageFormat.Base64,
        Source = ImageSource.FromStream(() => stream),
        Width = width,
        Height = height
    };
    
    richTextEditor.InsertImage(imageSource);
}
```

### RichTextEditorImageSource Properties

- **ImageFormat** - Format of the image (currently supports `Base64`)
- **Source** - ImageSource object (URI, stream, file, resource)
- **Width** - Width of the image in pixels
- **Height** - Height of the image in pixels

## Table Insertion

### Enable Table Toolbar Item

Include the Table toolbar item to allow users to insert tables:

```xml
<rte:SfRichTextEditor ShowToolbar="True">
    <rte:SfRichTextEditor.ToolbarItems>
        <rte:RichTextToolbarItem Type="Bold" />
        <rte:RichTextToolbarItem Type="Italic" />
        <rte:RichTextToolbarItem Type="Separator" />
        <rte:RichTextToolbarItem Type="Image" />
        <rte:RichTextToolbarItem Type="Table" />
    </rte:SfRichTextEditor.ToolbarItems>
</rte:SfRichTextEditor>
```

### Table Toolbar Behavior

When users tap the Table button:
1. A dialog appears
2. User specifies number of rows and columns
3. Table is inserted at cursor position

### Programmatic Table Insertion

Use the `InsertTable` method to insert tables programmatically:

**Syntax:**
```csharp
richTextEditor.InsertTable(int rows, int columns);
```

**Examples:**

```csharp
// Insert a 3x3 table
richTextEditor.InsertTable(3, 3);

// Insert a 5x4 table
richTextEditor.InsertTable(5, 4);

// Insert a 2x2 table
richTextEditor.InsertTable(2, 2);
```

### Insert Table with Button

```xml
<StackLayout>
    <Button Text="Insert 3x3 Table" Clicked="OnInsertTableClicked" />
    <rte:SfRichTextEditor x:Name="richTextEditor" ShowToolbar="True" />
</StackLayout>
```

```csharp
private void OnInsertTableClicked(object sender, EventArgs e)
{
    richTextEditor.InsertTable(3, 3);
}
```

### Insert Custom-Sized Tables

```csharp
public async Task InsertCustomTable()
{
    // Prompt for dimensions
    string rowsInput = await DisplayPromptAsync("Table Rows", "Enter number of rows:");
    string colsInput = await DisplayPromptAsync("Table Columns", "Enter number of columns:");
    
    if (int.TryParse(rowsInput, out int rows) && 
        int.TryParse(colsInput, out int cols))
    {
        if (rows > 0 && rows <= 20 && cols > 0 && cols <= 10)
        {
            richTextEditor.InsertTable(rows, cols);
        }
        else
        {
            await DisplayAlert("Error", "Rows (1-20) and columns (1-10) must be within limits", "OK");
        }
    }
}
```

### Predefined Table Templates

```csharp
public class TableTemplates
{
    private SfRichTextEditor editor;
    
    public TableTemplates(SfRichTextEditor editor)
    {
        this.editor = editor;
    }
    
    public void InsertSmallTable()
    {
        editor.InsertTable(2, 2);
    }
    
    public void InsertMediumTable()
    {
        editor.InsertTable(4, 4);
    }
    
    public void InsertLargeTable()
    {
        editor.InsertTable(6, 6);
    }
    
    public void InsertDataTable()
    {
        // Common for data: headers + 5 data rows
        editor.InsertTable(6, 4);
    }
    
    public void InsertScheduleTable()
    {
        // Days of week x time slots
        editor.InsertTable(7, 8);
    }
}
```

### Table Size Picker UI

```xml
<StackLayout>
    <Label Text="Insert Table" FontSize="18" FontAttributes="Bold" />
    
    <StackLayout Orientation="Horizontal" Spacing="10">
        <Label Text="Rows:" VerticalOptions="Center" />
        <Stepper x:Name="rowsStepper" 
                 Minimum="1" 
                 Maximum="20" 
                 Value="3" 
                 Increment="1" />
        <Label Text="{Binding Source={x:Reference rowsStepper}, Path=Value}" 
               VerticalOptions="Center" />
    </StackLayout>
    
    <StackLayout Orientation="Horizontal" Spacing="10">
        <Label Text="Columns:" VerticalOptions="Center" />
        <Stepper x:Name="colsStepper" 
                 Minimum="1" 
                 Maximum="10" 
                 Value="3" 
                 Increment="1" />
        <Label Text="{Binding Source={x:Reference colsStepper}, Path=Value}" 
               VerticalOptions="Center" />
    </StackLayout>
    
    <Button Text="Insert Table" Clicked="OnInsertCustomTableClicked" />
    
    <rte:SfRichTextEditor x:Name="richTextEditor" ShowToolbar="True" />
</StackLayout>
```

```csharp
private void OnInsertCustomTableClicked(object sender, EventArgs e)
{
    int rows = (int)rowsStepper.Value;
    int cols = (int)colsStepper.Value;
    richTextEditor.InsertTable(rows, cols);
}
```

## Platform-Specific Considerations

### MacCatalyst File Access Permission

On MacCatalyst, you must enable file access permissions in `Entitlements.plist`:

**Entitlements.plist:**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>com.apple.security.files.user-selected.read-write</key>
    <true/>
</dict>
</plist>
```

### Android Permissions

For Android, ensure you have storage and camera permissions in `AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.CAMERA" />
```

Request permissions at runtime:

```csharp
public async Task<bool> RequestStoragePermission()
{
    var status = await Permissions.CheckStatusAsync<Permissions.StorageRead>();
    
    if (status != PermissionStatus.Granted)
    {
        status = await Permissions.RequestAsync<Permissions.StorageRead>();
    }
    
    return status == PermissionStatus.Granted;
}

public async Task<bool> RequestCameraPermission()
{
    var status = await Permissions.CheckStatusAsync<Permissions.Camera>();
    
    if (status != PermissionStatus.Granted)
    {
        status = await Permissions.RequestAsync<Permissions.Camera>();
    }
    
    return status == PermissionStatus.Granted;
}
```

### iOS Photo Library Access

For iOS, add usage descriptions in `Info.plist`:

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>We need access to your photos to insert images</string>
<key>NSCameraUsageDescription</key>
<string>We need camera access to capture images</string>
```

### Platform-Specific Image Handling

```csharp
private async void OnImageRequested(object sender, RichTextEditorImageRequestedEventArgs e)
{
    e.IsHandled = true;
    
#if ANDROID
    if (!await RequestStoragePermission())
    {
        await DisplayAlert("Permission Denied", "Storage permission is required", "OK");
        return;
    }
#elif IOS
    // iOS handles permissions automatically through Info.plist
#endif
    
    // Proceed with image selection
    var photo = await MediaPicker.PickPhotoAsync();
    if (photo != null)
    {
        var stream = await photo.OpenReadAsync();
        InsertImage(stream, 350, 250);
    }
}
```

## Best Practices

### 1. Handle Image Loading Errors

```csharp
private async void OnImageRequested(object sender, RichTextEditorImageRequestedEventArgs e)
{
    e.IsHandled = true;
    
    try
    {
        var photo = await MediaPicker.PickPhotoAsync();
        if (photo != null)
        {
            var stream = await photo.OpenReadAsync();
            InsertImage(stream, 350, 250);
        }
    }
    catch (PermissionException)
    {
        await DisplayAlert("Permission Required", "Please grant photo access permission", "OK");
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", $"Failed to load image: {ex.Message}", "OK");
    }
}
```

### 2. Optimize Image Sizes

```csharp
private void InsertOptimizedImage(Stream stream)
{
    // Resize large images to reasonable dimensions
    const int maxWidth = 800;
    const int maxHeight = 600;
    
    RichTextEditorImageSource imageSource = new RichTextEditorImageSource
    {
        ImageFormat = RichTextEditorImageFormat.Base64,
        Source = ImageSource.FromStream(() => stream),
        Width = maxWidth,
        Height = maxHeight
    };
    
    richTextEditor.InsertImage(imageSource);
}
```

### 3. Provide Visual Feedback

```csharp
private async void OnImageRequested(object sender, RichTextEditorImageRequestedEventArgs e)
{
    e.IsHandled = true;
    
    // Show loading indicator
    loadingIndicator.IsVisible = true;
    
    try
    {
        var photo = await MediaPicker.PickPhotoAsync();
        if (photo != null)
        {
            var stream = await photo.OpenReadAsync();
            InsertImage(stream, 350, 250);
        }
    }
    finally
    {
        // Hide loading indicator
        loadingIndicator.IsVisible = false;
    }
}
```

### 4. Validate Table Dimensions

```csharp
public void InsertValidatedTable(int rows, int cols)
{
    const int maxRows = 50;
    const int maxCols = 20;
    
    if (rows < 1 || rows > maxRows)
    {
        DisplayAlert("Error", $"Rows must be between 1 and {maxRows}", "OK");
        return;
    }
    
    if (cols < 1 || cols > maxCols)
    {
        DisplayAlert("Error", $"Columns must be between 1 and {maxCols}", "OK");
        return;
    }
    
    richTextEditor.InsertTable(rows, cols);
}
```

### 5. Set Appropriate Image Formats

```csharp
// Currently only Base64 is supported
RichTextEditorImageSource imageSource = new RichTextEditorImageSource
{
    ImageFormat = RichTextEditorImageFormat.Base64,  // Required
    Source = ImageSource.FromUri(new Uri(imageUrl)),
    Width = 400,
    Height = 300
};
```

### 6. Provide Multiple Image Sources

Give users flexibility in how they add images:

```xml
<StackLayout Orientation="Horizontal" Spacing="5">
    <Button Text="📷 Camera" Clicked="OnCameraClicked" />
    <Button Text="🖼️ Gallery" Clicked="OnGalleryClicked" />
    <Button Text="🔗 URL" Clicked="OnUrlClicked" />
</StackLayout>
```

### 7. Cache Frequently Used Images

```csharp
public class ImageCache
{
    private Dictionary<string, ImageSource> cache = new Dictionary<string, ImageSource>();
    
    public ImageSource GetOrLoadImage(string url)
    {
        if (!cache.ContainsKey(url))
        {
            cache[url] = ImageSource.FromUri(new Uri(url));
        }
        return cache[url];
    }
}
```

### 8. Consider Mobile Data Usage

```csharp
private async void OnImageRequested(object sender, RichTextEditorImageRequestedEventArgs e)
{
    e.IsHandled = true;
    
    // Warn about data usage for large images
    bool proceed = await DisplayAlert(
        "Data Usage", 
        "Loading images may use cellular data. Continue?", 
        "Yes", 
        "No"
    );
    
    if (proceed)
    {
        var photo = await MediaPicker.PickPhotoAsync();
        if (photo != null)
        {
            var stream = await photo.OpenReadAsync();
            InsertImage(stream, 350, 250);
        }
    }
}
```

## Next Steps

- Implement [hyperlinks](hyperlinks.md) to complement media with interactive links
- Handle [events](events-and-interactions.md) for tracking when images/tables are added
- Explore [content management](content-management.md) for saving/loading HTML with embedded media
