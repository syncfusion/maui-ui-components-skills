# Exporting Polar Charts

## Overview

Syncfusion .NET MAUI Polar Charts can be exported as images for use in reports, presentations, or documentation. The chart provides two export methods:

- **SaveAsImage** - Save chart directly to device storage
- **GetStreamAsync** - Get chart as a stream for further processing

**Supported formats:** JPEG and PNG

## Prerequisites

The chart must be added to the visual tree before exporting. You cannot export a chart that hasn't been rendered on screen.

```csharp
// Chart must be part of the page
this.Content = chart;

// THEN export
await chart.SaveAsImage("chart.png");
```

## Export as Image File

### SaveAsImage Method

Saves the chart view as an image file to device storage.

### Basic Usage

```csharp
SfPolarChart chart = new SfPolarChart();
// Configure chart...
this.Content = chart;

// Export as PNG (default)
chart.SaveAsImage("ChartSample.png");

// Export as JPEG
chart.SaveAsImage("ChartSample.jpeg");
// or
chart.SaveAsImage("ChartSample.jpg");
```

### File Extension Determines Format

The file extension automatically determines the format:
- `.png` → PNG format (default)
- `.jpg` or `.jpeg` → JPEG format

```csharp
// PNG format
chart.SaveAsImage("polar-chart.png");

// JPEG format
chart.SaveAsImage("polar-chart.jpg");
chart.SaveAsImage("polar-chart.jpeg");  // Also JPEG
```

### Default Format

If no extension is provided, PNG is used by default:

```csharp
// Saves as PNG
chart.SaveAsImage("ChartSample");
```

## Storage Locations by Platform

Exported images are saved to different locations depending on the platform:

| Platform | Storage Location |
|----------|------------------|
| **Windows** | `Pictures` directory |
| **Android** | `Pictures` directory |
| **iOS** | `Photos/Album` directory |
| **macOS** | `Pictures` directory |

## Required Permissions

### Android

Add file writing permissions to `AndroidManifest.xml`:

```xml
<manifest>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
</manifest>
```

For Android 13+ (API 33+), use scoped storage permissions:

```xml
<uses-permission android:name="android.permission.READ_MEDIA_IMAGES" />
```

### iOS

Add photo library permissions to `Info.plist`:

```xml
<dict>
    <key>NSPhotoLibraryUsageDescription</key>
    <string>This App needs permission to access Photos</string>
    <key>NSPhotoLibraryAddUsageDescription</key>
    <string>This App needs permission to save charts to Photos</string>
</dict>
```

### Windows

No special permissions required for Windows.

## Export as Stream

### GetStreamAsync Method

Gets the chart as a stream for further processing (e.g., embedding in PDF, Excel, or Word documents).

### Basic Usage

```csharp
SfPolarChart chart = new SfPolarChart();
// Configure chart...
this.Content = chart;

// Get as JPEG stream
Stream stream = await chart.GetStreamAsync(ImageFileFormat.Jpeg);

// Get as PNG stream
Stream stream = await chart.GetStreamAsync(ImageFileFormat.Png);
```

### ImageFileFormat Enum

```csharp
// Available formats
ImageFileFormat.Jpeg
ImageFileFormat.Png
```

### Using the Stream

```csharp
// Get chart stream
Stream chartStream = await chart.GetStreamAsync(ImageFileFormat.Png);

// Save to custom location
using (FileStream fileStream = File.Create("custom-path/chart.png"))
{
    await chartStream.CopyToAsync(fileStream);
}

// Or pass to other components
await embedInPdf(chartStream);
await embedInEmail(chartStream);
```

## Common Use Cases

### Use Case 1: Export Button

```xml
<StackLayout>
    <Button Text="Export Chart" Clicked="OnExportClicked"/>
    
    <chart:SfPolarChart x:Name="polarChart">
        <!-- Chart configuration -->
    </chart:SfPolarChart>
</StackLayout>
```

```csharp
private void OnExportClicked(object sender, EventArgs e)
{
    try
    {
        string fileName = $"polar-chart-{DateTime.Now:yyyyMMdd-HHmmss}.png";
        polarChart.SaveAsImage(fileName);
        
        DisplayAlert("Success", $"Chart saved as {fileName}", "OK");
    }
    catch (Exception ex)
    {
        DisplayAlert("Error", $"Failed to export: {ex.Message}", "OK");
    }
}
```

### Use Case 2: Generate Report with Chart

```csharp
public async Task GenerateReportAsync()
{
    // Create chart
    SfPolarChart chart = new SfPolarChart();
    // Configure chart...
    
    // Temporarily add to visual tree
    this.Content = chart;
    
    // Wait for render
    await Task.Delay(500);
    
    // Export as stream
    Stream chartStream = await chart.GetStreamAsync(ImageFileFormat.Png);
    
    // Embed in report
    await CreatePdfReport(chartStream);
    
    // Remove from visual tree
    this.Content = null;
}
```

### Use Case 3: Export Multiple Formats

```csharp
public async Task ExportMultipleFormatsAsync()
{
    try
    {
        // Export as PNG
        chart.SaveAsImage("chart.png");
        
        // Export as JPEG
        chart.SaveAsImage("chart.jpg");
        
        // Get as stream for processing
        Stream stream = await chart.GetStreamAsync(ImageFileFormat.Png);
        
        await DisplayAlert("Success", "Chart exported in multiple formats", "OK");
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", $"Export failed: {ex.Message}", "OK");
    }
}
```

### Use Case 4: Export with Custom Name

```csharp
public void ExportWithTimestamp()
{
    string timestamp = DateTime.Now.ToString("yyyyMMdd_HHmmss");
    string filename = $"polar_chart_{timestamp}.png";
    
    chart.SaveAsImage(filename);
}
```

### Use Case 5: Share Chart via Email

```csharp
public async Task ShareChartAsync()
{
    // Get chart as stream
    Stream chartStream = await chart.GetStreamAsync(ImageFileFormat.Png);
    
    // Save to temp file
    string tempPath = Path.Combine(FileSystem.CacheDirectory, "chart-temp.png");
    using (FileStream fileStream = File.Create(tempPath))
    {
        await chartStream.CopyToAsync(fileStream);
    }
    
    // Share using MAUI Essentials
    await Share.RequestAsync(new ShareFileRequest
    {
        Title = "Share Chart",
        File = new ShareFile(tempPath)
    });
}
```

## Best Practices

### 1. Ensure Chart is Rendered

```csharp
// Add to visual tree FIRST
this.Content = chart;

// Optional: Wait for render if immediately exporting
await Task.Delay(500);

// THEN export
chart.SaveAsImage("chart.png");
```

### 2. Handle Exceptions

```csharp
try
{
    chart.SaveAsImage("chart.png");
}
catch (UnauthorizedAccessException)
{
    await DisplayAlert("Permission Denied", "Grant storage permission to save charts", "OK");
}
catch (Exception ex)
{
    await DisplayAlert("Error", $"Failed to export: {ex.Message}", "OK");
}
```

### 3. Use Descriptive Filenames

```csharp
// Good - descriptive with context
string filename = $"sales-radar-{DateTime.Now:yyyy-MM-dd}.png";

// Avoid - generic names
string filename = "chart.png";  // May overwrite previous exports
```

### 4. Choose Format Based on Use

```csharp
// Use PNG for:
// - Charts with transparency
// - High-quality images
// - Professional reports

// Use JPEG for:
// - Smaller file sizes
// - Web uploads
// - Email attachments
```

### 5. Dispose Streams Properly

```csharp
Stream stream = await chart.GetStreamAsync(ImageFileFormat.Png);

try
{
    // Use stream
    await ProcessStream(stream);
}
finally
{
    stream?.Dispose();
}

// Or use using statement
using (Stream stream = await chart.GetStreamAsync(ImageFileFormat.Png))
{
    await ProcessStream(stream);
}
```

## Common Patterns

### Pattern 1: Export with User Confirmation

```csharp
private async void OnExportClicked(object sender, EventArgs e)
{
    bool answer = await DisplayAlert(
        "Export Chart",
        "Save chart as PNG image?",
        "Yes", "No");
    
    if (answer)
    {
        try
        {
            string filename = $"chart-{DateTime.Now:yyyyMMdd}.png";
            chart.SaveAsImage(filename);
            await DisplayAlert("Success", $"Saved as {filename}", "OK");
        }
        catch (Exception ex)
        {
            await DisplayAlert("Error", ex.Message, "OK");
        }
    }
}
```

### Pattern 2: Format Selection

```csharp
private async void OnExportWithFormatSelection()
{
    string format = await DisplayActionSheet(
        "Select Export Format",
        "Cancel",
        null,
        "PNG", "JPEG");
    
    if (format != "Cancel")
    {
        string extension = format == "PNG" ? ".png" : ".jpg";
        string filename = $"chart{extension}";
        chart.SaveAsImage(filename);
    }
}
```

### Pattern 3: Background Export

```csharp
private async Task ExportInBackgroundAsync()
{
    // Show loading indicator
    ActivityIndicator indicator = new ActivityIndicator { IsRunning = true };
    this.Content = indicator;
    
    try
    {
        // Export in background
        await Task.Run(async () =>
        {
            Stream stream = await chart.GetStreamAsync(ImageFileFormat.Png);
            await SaveStreamToFile(stream);
        });
        
        await DisplayAlert("Success", "Chart exported", "OK");
    }
    finally
    {
        // Restore chart
        this.Content = chart;
    }
}
```

## Troubleshooting

### Chart Not Exporting

**Problem:** SaveAsImage or GetStreamAsync doesn't work.

**Solution:**
```csharp
// Ensure chart is added to visual tree
this.Content = chart;

// Wait for chart to render
await Task.Delay(500);

// Then export
chart.SaveAsImage("chart.png");
```

### Permission Denied

**Problem:** Export fails with permission error.

**Solution:**
- **Android**: Add `WRITE_EXTERNAL_STORAGE` permission to manifest
- **iOS**: Add `NSPhotoLibraryAddUsageDescription` to Info.plist
- Request permissions at runtime on Android 13+

### File Not Found After Export

**Problem:** File saved but can't find it.

**Solution:**
```csharp
// Check platform-specific locations:
// Android/Windows: Pictures folder
// iOS: Photos app

// Or save to app-specific directory:
string appPath = FileSystem.AppDataDirectory;
string filePath = Path.Combine(appPath, "chart.png");
chart.SaveAsImage(filePath);
```

### Stream is Empty

**Problem:** GetStreamAsync returns empty or null stream.

**Solution:**
```csharp
// Ensure chart is fully rendered
this.Content = chart;
await Task.Delay(1000);  // Increase delay

// Then get stream
Stream stream = await chart.GetStreamAsync(ImageFileFormat.Png);

// Verify stream has data
if (stream == null || stream.Length == 0)
{
    throw new Exception("Stream is empty");
}
```

## Related Topics

- **Getting Started**: [getting-started.md](getting-started.md) - Basic chart creation
- **Appearance**: [appearance-customization.md](appearance-customization.md) - Style before exporting
- **Series Types**: [series-types.md](series-types.md) - Choose series for export
