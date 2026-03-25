# Exporting Pyramid Charts

This guide covers exporting Syncfusion .NET MAUI Pyramid Charts as images or streams for use in reports, documents, and other applications.

## Overview

The SfPyramidChart supports two export methods:

1. **SaveAsImage**: Export chart directly to a file (JPEG or PNG)
2. **GetStreamAsync**: Get chart as a stream for integration with other components

**Key Features:**
- Export to JPEG or PNG formats
- Platform-specific save locations
- Asynchronous stream retrieval
- Integration with PDF, Excel, and Word components
- High-quality image generation

**Important:** The chart must be added to the visual tree before exporting.

## Export as Image

Use the `SaveAsImage` method to export the chart view as an image file.

### Supported Formats

- **JPEG** (.jpeg, .jpg)
- **PNG** (.png) - Default if no extension specified

### Basic Image Export

**C# Example:**
```csharp
SfPyramidChart chart = new SfPyramidChart();
chart.ItemsSource = viewModel.Data;
chart.XBindingPath = "Name";
chart.YBindingPath = "Value";
chart.ShowDataLabels = true;

this.Content = chart;

// Export as PNG (default)
chart.SaveAsImage("PyramidChart.png");
```

### Export as JPEG

```csharp
// Export as JPEG
chart.SaveAsImage("PyramidChart.jpeg");

// or with .jpg extension
chart.SaveAsImage("PyramidChart.jpg");
```

### Export as PNG

```csharp
// Explicit PNG
chart.SaveAsImage("PyramidChart.png");

// Default (no extension = PNG)
chart.SaveAsImage("PyramidChart");
```

### Export with Button Click

**XAML:**
```xaml
<StackLayout>
    <chart:SfPyramidChart x:Name="pyramidChart"
                          ItemsSource="{Binding Data}"
                          XBindingPath="Name"
                          YBindingPath="Value"
                          ShowDataLabels="True"/>
    
    <Button Text="Export Chart" Clicked="OnExportClicked"/>
</StackLayout>
```

**C# Code-Behind:**
```csharp
private void OnExportClicked(object sender, EventArgs e)
{
    pyramidChart.SaveAsImage("PyramidChart_Export.png");
    
    // Show confirmation
    DisplayAlert("Success", "Chart exported successfully!", "OK");
}
```

## Platform-Specific Save Locations

Exported images are saved to different locations depending on the platform.

### Save Locations by Platform

| Platform | Save Location |
|----------|--------------|
| **Android** | Pictures directory |
| **iOS** | Photos/Album directory |
| **macOS** | Pictures directory |
| **Windows Phone** | Pictures directory |

### Android Save Location

```
/storage/emulated/0/Pictures/PyramidChart.png
```

**Accessing Exported Files:**
- Open Files app or Gallery
- Navigate to Pictures folder
- Find your exported chart file

### iOS Save Location

```
Photos app → Albums → Saved Photos
```

**Accessing Exported Files:**
- Open Photos app
- Go to Albums
- Find your chart in recent photos

### macOS Save Location

```
~/Pictures/PyramidChart.png
```

**Accessing Exported Files:**
- Open Finder
- Go to Pictures folder
- Find your exported chart file

## Required Permissions

### Android Permissions

Add to `AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

For Android 10+ (API 29+), also add:

```xml
<application android:requestLegacyExternalStorage="true">
    <!-- App configuration -->
</application>
```

### iOS Permissions

Add to `Info.plist`:

```xml
<dict>
    ...
    <key>NSPhotoLibraryUsageDescription</key>
    <string>This app needs permission to access photos</string>
    
    <key>NSPhotoLibraryAddUsageDescription</key>
    <string>This app needs permission to save charts to photos</string>
    ...
</dict>
```

### Runtime Permission Request (Android/iOS)

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

// Usage
private async void OnExportClicked(object sender, EventArgs e)
{
    if (await RequestStoragePermission())
    {
        pyramidChart.SaveAsImage("PyramidChart.png");
        await DisplayAlert("Success", "Chart exported!", "OK");
    }
    else
    {
        await DisplayAlert("Permission Denied", 
            "Storage permission is required to export charts.", "OK");
    }
}
```

## Get Chart as Stream

Use the `GetStreamAsync` method to export the chart as a stream. This is useful for integrating with PDF, Excel, Word, or custom export scenarios.

### Basic Stream Export

**C# Example:**
```csharp
SfPyramidChart chart = new SfPyramidChart();
chart.ItemsSource = viewModel.Data;
chart.XBindingPath = "Name";
chart.YBindingPath = "Value";

this.Content = chart;

// Get chart as stream (PNG format)
Stream chartStream = await chart.GetStreamAsync(ImageFileFormat.Png);
```

### Stream Format Options

```csharp
// PNG format
Stream pngStream = await chart.GetStreamAsync(ImageFileFormat.Png);

// JPEG format
Stream jpegStream = await chart.GetStreamAsync(ImageFileFormat.Jpeg);
```

### Using Stream with PDF

```csharp
using Syncfusion.Pdf;
using Syncfusion.Pdf.Graphics;

private async Task ExportChartToPDF()
{
    // Get chart stream
    Stream chartStream = await pyramidChart.GetStreamAsync(ImageFileFormat.Png);
    
    // Create PDF document
    PdfDocument document = new PdfDocument();
    PdfPage page = document.Pages.Add();
    
    // Load image from stream
    PdfBitmap image = new PdfBitmap(chartStream);
    
    // Draw image on PDF
    page.Graphics.DrawImage(image, new PointF(10, 10));
    
    // Save PDF
    using (MemoryStream outputStream = new MemoryStream())
    {
        document.Save(outputStream);
        document.Close();
        
        // Save to file
        File.WriteAllBytes("ChartReport.pdf", outputStream.ToArray());
    }
}
```

### Using Stream with Excel

```csharp
using Syncfusion.XlsIO;

private async Task ExportChartToExcel()
{
    // Get chart stream
    Stream chartStream = await pyramidChart.GetStreamAsync(ImageFileFormat.Png);
    
    // Create Excel workbook
    using (ExcelEngine excelEngine = new ExcelEngine())
    {
        IApplication application = excelEngine.Excel;
        IWorkbook workbook = application.Workbooks.Create(1);
        IWorksheet worksheet = workbook.Worksheets[0];
        
        // Add chart image to Excel
        worksheet.Pictures.AddPicture(1, 1, chartStream);
        
        // Save Excel file
        using (FileStream fileStream = new FileStream("ChartData.xlsx", FileMode.Create))
        {
            workbook.SaveAs(fileStream);
        }
    }
}
```

### Using Stream with Word

```csharp
using Syncfusion.DocIO;
using Syncfusion.DocIO.DLS;

private async Task ExportChartToWord()
{
    // Get chart stream
    Stream chartStream = await pyramidChart.GetStreamAsync(ImageFileFormat.Png);
    
    // Create Word document
    using (WordDocument document = new WordDocument())
    {
        IWSection section = document.AddSection();
        IWParagraph paragraph = section.AddParagraph();
        
        // Add title
        paragraph.AppendText("Pyramid Chart Analysis");
        paragraph.ApplyStyle(BuiltinStyle.Heading1);
        
        // Add chart image
        paragraph = section.AddParagraph();
        WPicture picture = paragraph.AppendPicture(chartStream) as WPicture;
        picture.Width = 500;
        picture.Height = 400;
        
        // Save Word document
        using (FileStream fileStream = new FileStream("ChartReport.docx", FileMode.Create))
        {
            document.Save(fileStream, FormatType.Docx);
        }
    }
}
```

## Complete Examples

### Example 1: Export with Multiple Formats

```csharp
public class ExportPage : ContentPage
{
    private SfPyramidChart chart;
    
    public ExportPage()
    {
        chart = new SfPyramidChart
        {
            ItemsSource = new ViewModel().Data,
            XBindingPath = "Name",
            YBindingPath = "Value",
            ShowDataLabels = true
        };
        
        chart.Legend = new ChartLegend();
        
        var exportPngButton = new Button { Text = "Export PNG" };
        exportPngButton.Clicked += (s, e) => chart.SaveAsImage("Chart.png");
        
        var exportJpegButton = new Button { Text = "Export JPEG" };
        exportJpegButton.Clicked += (s, e) => chart.SaveAsImage("Chart.jpeg");
        
        var exportPdfButton = new Button { Text = "Export to PDF" };
        exportPdfButton.Clicked += async (s, e) => await ExportToPDF();
        
        Content = new StackLayout
        {
            Children = { chart, exportPngButton, exportJpegButton, exportPdfButton }
        };
    }
    
    private async Task ExportToPDF()
    {
        try
        {
            Stream chartStream = await chart.GetStreamAsync(ImageFileFormat.Png);
            
            // Create PDF with chart
            PdfDocument document = new PdfDocument();
            PdfPage page = document.Pages.Add();
            PdfBitmap image = new PdfBitmap(chartStream);
            page.Graphics.DrawImage(image, new PointF(0, 0));
            
            using (MemoryStream stream = new MemoryStream())
            {
                document.Save(stream);
                document.Close();
                
                File.WriteAllBytes(Path.Combine(
                    FileSystem.AppDataDirectory, "ChartReport.pdf"), 
                    stream.ToArray());
            }
            
            await DisplayAlert("Success", "PDF exported successfully!", "OK");
        }
        catch (Exception ex)
        {
            await DisplayAlert("Error", $"Export failed: {ex.Message}", "OK");
        }
    }
}
```

### Example 2: Email Chart as Attachment

```csharp
private async Task EmailChart()
{
    try
    {
        // Export chart to file
        string fileName = "PyramidChart.png";
        pyramidChart.SaveAsImage(fileName);
        
        // Get file path (platform-specific)
        string filePath = Path.Combine(
            Environment.GetFolderPath(Environment.SpecialFolder.MyPictures), 
            fileName);
        
        // Create email with attachment
        var message = new EmailMessage
        {
            Subject = "Pyramid Chart Report",
            Body = "Please find the attached pyramid chart for review.",
            To = new List<string> { "recipient@example.com" }
        };
        
        message.Attachments.Add(new EmailAttachment(filePath));
        
        await Email.ComposeAsync(message);
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", $"Failed to email chart: {ex.Message}", "OK");
    }
}
```

### Example 3: Save to Custom Location

```csharp
private async Task SaveToCustomLocation()
{
    try
    {
        // Get chart as stream
        Stream chartStream = await pyramidChart.GetStreamAsync(ImageFileFormat.Png);
        
        // Define custom path
        string customPath = Path.Combine(
            FileSystem.AppDataDirectory, 
            "Charts", 
            $"Pyramid_{DateTime.Now:yyyyMMdd_HHmmss}.png");
        
        // Ensure directory exists
        Directory.CreateDirectory(Path.GetDirectoryName(customPath));
        
        // Save stream to file
        using (FileStream fileStream = File.Create(customPath))
        {
            await chartStream.CopyToAsync(fileStream);
        }
        
        await DisplayAlert("Success", $"Chart saved to: {customPath}", "OK");
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", $"Save failed: {ex.Message}", "OK");
    }
}
```

## Best Practices

### When to Export

- **After chart is rendered**: Ensure chart is in visual tree
- **Data is loaded**: Verify ItemsSource has data
- **Size is adequate**: Chart has appropriate width/height
- **User action**: Trigger from button click or menu option

### File Naming

- Use descriptive names: `SalesFunnel_Q4_2024.png`
- Include timestamps for versioning
- Avoid special characters in filenames
- Use consistent naming conventions

### Format Selection

**Use PNG when:**
- Need transparency support
- Quality is priority
- File size is not a concern
- Exporting charts with gradients

**Use JPEG when:**
- File size needs to be smaller
- Transparency not required
- Suitable for photographs/complex visuals
- Email attachments with size limits

### Performance Considerations

- Export is relatively fast for pyramid charts
- Large charts take slightly longer to export
- Use async methods to avoid UI blocking
- Consider showing loading indicator for large exports

### Error Handling

```csharp
private async Task SafeExport()
{
    try
    {
        if (await RequestStoragePermission())
        {
            pyramidChart.SaveAsImage("Chart.png");
            await DisplayAlert("Success", "Chart exported!", "OK");
        }
    }
    catch (UnauthorizedAccessException)
    {
        await DisplayAlert("Error", "Permission denied", "OK");
    }
    catch (IOException ex)
    {
        await DisplayAlert("Error", $"File error: {ex.Message}", "OK");
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", $"Export failed: {ex.Message}", "OK");
    }
}
```

## Common Issues and Solutions

### Issue: Chart Not Exporting

**Possible Causes:**
- Chart not added to visual tree
- Data not loaded
- Permissions not granted

**Solutions:**
- Ensure `this.Content = chart` is called
- Verify ItemsSource has data
- Request and check permissions

### Issue: Exported Image is Blank

**Solutions:**
- Wait for chart to render before exporting
- Add delay after chart initialization:
  ```csharp
  await Task.Delay(500);
  chart.SaveAsImage("Chart.png");
  ```
- Ensure chart has non-zero size

### Issue: File Not Found After Export

**Solutions:**
- Check platform-specific save location
- Verify storage permissions are granted
- Use absolute path to verify save location:
  ```csharp
  string path = Path.Combine(
      Environment.GetFolderPath(Environment.SpecialFolder.MyPictures),
      "Chart.png");
  ```

### Issue: Poor Image Quality

**Solutions:**
- Increase chart size before exporting
- Use PNG format instead of JPEG
- Ensure chart is fully rendered
- Set explicit chart dimensions

## Integration Examples

### Export for Reporting

```csharp
public async Task GenerateReport()
{
    // Multiple charts export
    var charts = new List<SfPyramidChart> { chart1, chart2, chart3 };
    
    PdfDocument document = new PdfDocument();
    
    foreach (var chart in charts)
    {
        Stream stream = await chart.GetStreamAsync(ImageFileFormat.Png);
        PdfPage page = document.Pages.Add();
        PdfBitmap image = new PdfBitmap(stream);
        page.Graphics.DrawImage(image, new PointF(50, 50));
    }
    
    document.Save("ComprehensiveReport.pdf");
    document.Close();
}
```

### Share via Social Media

```csharp
private async Task ShareChart()
{
    // Export chart
    string fileName = "PyramidChart.png";
    pyramidChart.SaveAsImage(fileName);
    
    string filePath = Path.Combine(
        Environment.GetFolderPath(Environment.SpecialFolder.MyPictures), 
        fileName);
    
    // Share using platform share dialog
    await Share.RequestAsync(new ShareFileRequest
    {
        Title = "Share Pyramid Chart",
        File = new ShareFile(filePath)
    });
}
```

## Related Features

- **Appearance**: See [appearance-customization.md](appearance-customization.md) for styling before export
- **Orientation**: See [orientation-and-effects.md](orientation-and-effects.md) for layout options
- **Data Labels**: See [data-labels.md](data-labels.md) to include labels in exports