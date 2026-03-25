# Exporting Charts in .NET MAUI Circular Charts

Export circular charts as images or streams for sharing, reporting, or documentation purposes. This guide covers exporting charts in various formats.

## Table of Contents
- [Export as Image File](#export-as-image-file)
- [Export as Stream](#export-as-stream)
- [Supported Formats](#supported-formats)
- [Platform-Specific Behavior](#platform-specific-behavior)
- [Permissions and Setup](#permissions-and-setup)

## Export as Image File

Use the `SaveAsImage` method to save the chart directly to a file.

### Basic Export

**C#:**
```csharp
SfCircularChart chart = new SfCircularChart();
// ... configure chart ...
this.Content = chart;

// Export as PNG (default)
chart.SaveAsImage("ChartExport.png");

// Export as JPEG
chart.SaveAsImage("ChartExport.jpeg");
chart.SaveAsImage("ChartExport.jpg");
```

### Method Signature

```csharp
void SaveAsImage(string filename)
```

**Parameters:**
- `filename`: The name of the file with extension (.png, .jpg, or .jpeg)

### Format Detection

The format is determined by the file extension:
- `.png` → PNG format
- `.jpg` or `.jpeg` → JPEG format
- No extension → Defaults to PNG

```csharp
// These all create PNG files
chart.SaveAsImage("chart1");
chart.SaveAsImage("chart2.png");

// These create JPEG files
chart.SaveAsImage("chart3.jpg");
chart.SaveAsImage("chart4.jpeg");
```

### Important Prerequisite

> **Critical:** The chart MUST be added to the visual tree before exporting. Call `SaveAsImage` after setting the chart as content or adding it to the view hierarchy.

**Correct:**
```csharp
SfCircularChart chart = new SfCircularChart();
// Configure chart...
this.Content = chart;  // Add to visual tree FIRST
await Task.Delay(100);  // Optional: Ensure rendering completes
chart.SaveAsImage("MyChart.png");
```

**Incorrect:**
```csharp
SfCircularChart chart = new SfCircularChart();
// Configure chart...
chart.SaveAsImage("MyChart.png");  // ❌ Won't work - not in visual tree
this.Content = chart;
```

## Export as Stream

Use the `GetStreamAsync` method to get the chart as a stream for further processing.

### Basic Stream Export

**C#:**
```csharp
SfCircularChart chart = new SfCircularChart();
// ... configure chart ...
this.Content = chart;

// Get as PNG stream
Stream pngStream = await chart.GetStreamAsync(ImageFileFormat.Png);

// Get as JPEG stream
Stream jpegStream = await chart.GetStreamAsync(ImageFileFormat.Jpeg);
```

### Method Signature

```csharp
Task<Stream> GetStreamAsync(ImageFileFormat format)
```

**Parameters:**
- `format`: `ImageFileFormat.Png` or `ImageFileFormat.Jpeg`

**Returns:** `Task<Stream>` containing the chart image

### Using the Stream

```csharp
// Save stream to file
using (Stream stream = await chart.GetStreamAsync(ImageFileFormat.Png))
{
    using (FileStream fileStream = File.Create("chart.png"))
    {
        await stream.CopyToAsync(fileStream);
    }
}

// Send stream over network
Stream chartStream = await chart.GetStreamAsync(ImageFileFormat.Jpeg);
await SendToServer(chartStream);

// Convert to byte array
using (Stream stream = await chart.GetStreamAsync(ImageFileFormat.Png))
{
    using (MemoryStream ms = new MemoryStream())
    {
        await stream.CopyToAsync(ms);
        byte[] imageBytes = ms.ToArray();
    }
}
```

## Supported Formats

### PNG (Portable Network Graphics)

**Characteristics:**
- Lossless compression
- Supports transparency
- Larger file size
- Best for: Presentations, web display, high-quality prints

**Usage:**
```csharp
chart.SaveAsImage("chart.png");
// or
Stream stream = await chart.GetStreamAsync(ImageFileFormat.Png);
```

### JPEG (Joint Photographic Experts Group)

**Characteristics:**
- Lossy compression
- No transparency
- Smaller file size
- Best for: Email attachments, quick sharing, space-constrained scenarios

**Usage:**
```csharp
chart.SaveAsImage("chart.jpeg");
// or
Stream stream = await chart.GetStreamAsync(ImageFileFormat.Jpeg);
```

## Platform-Specific Behavior

### Save Locations by Platform

| Platform | Save Location |
|----------|---------------|
| **Android** | `Pictures` directory |
| **iOS** | `Photos/Album` directory |
| **macOS** | `Pictures` directory |
| **Windows** | `Pictures` directory |

### Accessing Saved Files

**Android:**
```
/storage/emulated/0/Pictures/ChartExport.png
```

**iOS:**
- Accessible through the Photos app
- Cannot directly access file path

**Windows/Mac:**
```
C:\Users\[Username]\Pictures\ChartExport.png
~/Pictures/ChartExport.png
```

## Permissions and Setup

### Android Permissions

Add to `AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

For Android 13+ (API 33+), add:

```xml
<uses-permission android:name="android.permission.READ_MEDIA_IMAGES" />
```

**Request at Runtime:**
```csharp
public async Task<bool> RequestStoragePermission()
{
    var status = await Permissions.CheckStatusAsync<Permissions.StorageWrite>();
    
    if (status != PermissionStatus.Granted)
    {
        status = await Permissions.RequestAsync<Permissions.StorageWrite>();
    }
    
    return status == PermissionStatus.Granted;
}

// Before exporting
if (await RequestStoragePermission())
{
    chart.SaveAsImage("MyChart.png");
}
```

### iOS Permissions

Add to `Info.plist`:

```xml
<dict>
    <key>NSPhotoLibraryUsageDescription</key>
    <string>This app needs access to save chart images to your photo library</string>
    
    <key>NSPhotoLibraryAddUsageDescription</key>
    <string>This app needs permission to add chart images to your photo library</string>
</dict>
```

**Request at Runtime:**
```csharp
public async Task<bool> RequestPhotosPermission()
{
    var status = await Permissions.CheckStatusAsync<Permissions.Photos>();
    
    if (status != PermissionStatus.Granted)
    {
        status = await Permissions.RequestAsync<Permissions.Photos>();
    }
    
    return status == PermissionStatus.Granted;
}
```

## Complete Examples

### Example 1: Simple Export Button

**XAML:**
```xml
<StackLayout>
    <Button Text="Export Chart" 
            Clicked="OnExportClicked"/>
    
    <chart:SfCircularChart x:Name="chart">
        <chart:PieSeries ItemsSource="{Binding Data}"
                         XBindingPath="Category"
                         YBindingPath="Value"/>
    </chart:SfCircularChart>
</StackLayout>
```

**C#:**
```csharp
private void OnExportClicked(object sender, EventArgs e)
{
    try
    {
        chart.SaveAsImage("SalesChart.png");
        DisplayAlert("Success", "Chart exported successfully!", "OK");
    }
    catch (Exception ex)
    {
        DisplayAlert("Error", $"Export failed: {ex.Message}", "OK");
    }
}
```

### Example 2: Export with Format Selection

```csharp
public async Task ExportChartWithFormat(ImageFormat format)
{
    string filename = format == ImageFormat.PNG 
        ? "Chart.png" 
        : "Chart.jpeg";
    
    try
    {
        chart.SaveAsImage(filename);
        await DisplayAlert("Exported", $"Saved as {filename}", "OK");
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", ex.Message, "OK");
    }
}

public enum ImageFormat
{
    PNG,
    JPEG
}
```

### Example 3: Export to Stream and Share

```csharp
public async Task ExportAndShare()
{
    try
    {
        // Export to stream
        Stream chartStream = await chart.GetStreamAsync(ImageFileFormat.Png);
        
        // Save to temporary file
        string tempPath = Path.Combine(FileSystem.CacheDirectory, "chart.png");
        using (FileStream fileStream = File.Create(tempPath))
        {
            await chartStream.CopyToAsync(fileStream);
        }
        
        // Share using MAUI Share API
        await Share.RequestAsync(new ShareFileRequest
        {
            Title = "Share Chart",
            File = new ShareFile(tempPath)
        });
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", $"Failed to share: {ex.Message}", "OK");
    }
}
```

### Example 4: Export to PDF (via Stream)

```csharp
public async Task ExportToPdf()
{
    try
    {
        // Get chart as stream
        Stream chartStream = await chart.GetStreamAsync(ImageFileFormat.Png);
        
        // Create PDF document (using a PDF library)
        PdfDocument document = new PdfDocument();
        PdfPage page = document.AddPage();
        
        using (var imageStream = new MemoryStream())
        {
            await chartStream.CopyToAsync(imageStream);
            imageStream.Position = 0;
            
            // Add image to PDF
            XImage image = XImage.FromStream(imageStream);
            XGraphics gfx = XGraphics.FromPdfPage(page);
            gfx.DrawImage(image, 0, 0);
        }
        
        // Save PDF
        string pdfPath = Path.Combine(FileSystem.AppDataDirectory, "chart.pdf");
        document.Save(pdfPath);
        
        await DisplayAlert("Success", "PDF created successfully", "OK");
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", ex.Message, "OK");
    }
}
```

### Example 5: Export with Permission Handling

```csharp
public async Task ExportChartSafely()
{
    try
    {
        // Check/request permissions
        bool hasPermission = await RequestStoragePermission();
        
        if (!hasPermission)
        {
            await DisplayAlert("Permission Denied", 
                "Storage permission is required to save charts", 
                "OK");
            return;
        }
        
        // Add small delay to ensure chart is fully rendered
        await Task.Delay(100);
        
        // Export
        string filename = $"Chart_{DateTime.Now:yyyyMMdd_HHmmss}.png";
        chart.SaveAsImage(filename);
        
        await DisplayAlert("Success", 
            $"Chart saved as {filename}", 
            "OK");
    }
    catch (Exception ex)
    {
        await DisplayAlert("Export Failed", ex.Message, "OK");
    }
}

private async Task<bool> RequestStoragePermission()
{
    var status = await Permissions.CheckStatusAsync<Permissions.StorageWrite>();
    
    if (status != PermissionStatus.Granted)
    {
        status = await Permissions.RequestAsync<Permissions.StorageWrite>();
    }
    
    return status == PermissionStatus.Granted;
}
```

### Example 6: Export Multiple Formats

```csharp
public async Task ExportBothFormats()
{
    try
    {
        // Export PNG
        chart.SaveAsImage("ChartHighQuality.png");
        
        // Small delay between exports
        await Task.Delay(100);
        
        // Export JPEG
        chart.SaveAsImage("ChartCompressed.jpeg");
        
        await DisplayAlert("Success", 
            "Exported in both PNG and JPEG formats", 
            "OK");
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", ex.Message, "OK");
    }
}
```

## Best Practices

1. **Timing**: Add a small delay after adding chart to view before exporting
   ```csharp
   this.Content = chart;
   await Task.Delay(100);
   chart.SaveAsImage("chart.png");
   ```

2. **Permissions**: Always check permissions before exporting
   
3. **Error Handling**: Wrap export calls in try-catch blocks

4. **Filename**: Use descriptive names with timestamps
   ```csharp
   string filename = $"Sales_Report_{DateTime.Now:yyyy-MM-dd}.png";
   ```

5. **Format Choice**: 
   - Use PNG for quality and transparency
   - Use JPEG for smaller file size

6. **Stream Cleanup**: Dispose streams properly
   ```csharp
   using (Stream stream = await chart.GetStreamAsync(ImageFileFormat.Png))
   {
       // Use stream
   }  // Automatically disposed
   ```

## Troubleshooting

### Chart Not Exporting

**Cause:** Chart not added to visual tree  
**Solution:** Ensure `this.Content = chart` or add to view hierarchy first

### Empty/Blank Image

**Cause:** Export called before chart finished rendering  
**Solution:** Add delay: `await Task.Delay(100);`

### Permission Denied Error

**Cause:** Missing storage permissions  
**Solution:** Add permissions to manifest and request at runtime

### File Not Found

**Cause:** Looking in wrong directory  
**Solution:** Check platform-specific save locations

### iOS Export Fails

**Cause:** Missing Info.plist entries  
**Solution:** Add NSPhotoLibraryUsageDescription and NSPhotoLibraryAddUsageDescription

## Summary

- **SaveAsImage**: Direct file export (.png or .jpeg)
- **GetStreamAsync**: Get chart as stream for processing
- **Formats**: PNG (quality) and JPEG (size)
- **Prerequisite**: Chart must be in visual tree
- **Permissions**: Required on Android and iOS
- **Platform-specific**: Different save locations per platform
