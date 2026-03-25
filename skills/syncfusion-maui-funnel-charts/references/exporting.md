# Exporting in .NET MAUI Funnel Chart

Export your funnel charts as images for sharing, reporting, or embedding in other documents. The `SfFunnelChart` supports exporting to JPEG and PNG formats, as well as retrieving chart content as a stream.

## Export as an Image

Use the `SaveAsImage` method to export the chart view as an image file in JPEG or PNG format.

### Prerequisites
- The chart must be added to the visual tree before exporting
- File writing permissions may be required on some platforms

### Supported Formats
- **JPEG** (.jpeg, .jpg)
- **PNG** (.png) - Default format if no extension specified

### Basic Export

#### C# (PNG Format - Default)
```csharp
SfFunnelChart chart = new SfFunnelChart();
chart.ItemsSource = viewModel.Data;
chart.XBindingPath = "XValue";
chart.YBindingPath = "YValue";

this.Content = chart;

// Export as PNG (default)
chart.SaveAsImage("FunnelChart.png");
```

#### C# (JPEG Format)
```csharp
SfFunnelChart chart = new SfFunnelChart();
chart.ItemsSource = viewModel.Data;
chart.XBindingPath = "XValue";
chart.YBindingPath = "YValue";

this.Content = chart;

// Export as JPEG
chart.SaveAsImage("FunnelChart.jpeg");
```

### Export from Button Click

```xaml
<ContentPage xmlns:chart="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts">
    <VerticalStackLayout>
        
        <chart:SfFunnelChart x:Name="funnelChart"
                             ItemsSource="{Binding Data}"
                             XBindingPath="Stage"
                             YBindingPath="Value">
            <chart:SfFunnelChart.Title>
                <Label Text="Sales Funnel"/>
            </chart:SfFunnelChart.Title>
        </chart:SfFunnelChart>
        
        <HorizontalStackLayout Spacing="10" Padding="10">
            <Button Text="Export as PNG" 
                    Clicked="OnExportAsPngClicked"/>
            <Button Text="Export as JPEG" 
                    Clicked="OnExportAsJpegClicked"/>
        </HorizontalStackLayout>
        
    </VerticalStackLayout>
</ContentPage>
```

```csharp
private void OnExportAsPngClicked(object sender, EventArgs e)
{
    funnelChart.SaveAsImage("SalesFunnel.png");
    DisplayAlert("Success", "Chart exported as PNG", "OK");
}

private void OnExportAsJpegClicked(object sender, EventArgs e)
{
    funnelChart.SaveAsImage("SalesFunnel.jpeg");
    DisplayAlert("Success", "Chart exported as JPEG", "OK");
}
```

## Export File Locations

Exported images are saved in platform-specific directories:

| Platform | Default Location |
|----------|------------------|
| **Android** | `Pictures` directory in file system |
| **Windows** | `Pictures` directory in file system |
| **iOS** | `Photos/Album` directory |
| **macOS** | `Pictures` directory in file system |

## Platform-Specific Permissions

### Android

Add file writing permissions in `AndroidManifest.xml`:

```xml
<manifest>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
</manifest>
```

For Android 10+ (API level 29+), you may need to handle scoped storage:

```xml
<application android:requestLegacyExternalStorage="true">
```

### iOS

Add permission descriptions in `Info.plist`:

```xml
<dict>
    <key>NSPhotoLibraryUsageDescription</key>    
    <string>This app needs permission to access Photos</string>    
    <key>NSPhotoLibraryAddUsageDescription</key>    
    <string>This app needs permission to save charts to Photos</string> 
</dict>
```

### Windows

Typically no special permissions needed for `Pictures` folder.

## Get Chart as Stream

The `GetStreamAsync` method retrieves the chart as a stream asynchronously, useful for passing to other components (PDF, Excel, Word, etc.) or uploading to cloud services.

### Method Signature

```csharp
Task<Stream> GetStreamAsync(ImageFileFormat format)
```

### Supported Formats
- `ImageFileFormat.Jpeg`
- `ImageFileFormat.Png`

### Basic Usage

```csharp
SfFunnelChart chart = new SfFunnelChart();
chart.ItemsSource = viewModel.Data;
chart.XBindingPath = "XValue";
chart.YBindingPath = "YValue";

this.Content = chart;

// Get chart as PNG stream
Stream chartStream = await chart.GetStreamAsync(ImageFileFormat.Png);
```

### Example: Save Stream to File

```csharp
private async Task ExportChartToCustomLocationAsync()
{
    try
    {
        // Get chart as stream
        Stream chartStream = await funnelChart.GetStreamAsync(ImageFileFormat.Png);
        
        // Define custom file path
        string fileName = $"FunnelChart_{DateTime.Now:yyyyMMdd_HHmmss}.png";
        string filePath = Path.Combine(FileSystem.AppDataDirectory, fileName);
        
        // Write stream to file
        using (FileStream fileStream = File.Create(filePath))
        {
            await chartStream.CopyToAsync(fileStream);
        }
        
        await DisplayAlert("Success", $"Chart saved to: {filePath}", "OK");
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", $"Failed to export: {ex.Message}", "OK");
    }
}
```

### Example: Upload Chart to Server

```csharp
private async Task UploadChartToServerAsync()
{
    try
    {
        // Get chart as JPEG stream
        Stream chartStream = await funnelChart.GetStreamAsync(ImageFileFormat.Jpeg);
        
        // Create multipart form content
        using (var content = new MultipartFormDataContent())
        {
            var streamContent = new StreamContent(chartStream);
            streamContent.Headers.ContentType = new System.Net.Http.Headers.MediaTypeHeaderValue("image/jpeg");
            content.Add(streamContent, "chart", "funnel-chart.jpeg");
            
            // Upload to server
            using (var httpClient = new HttpClient())
            {
                var response = await httpClient.PostAsync("https://api.example.com/upload", content);
                
                if (response.IsSuccessStatusCode)
                {
                    await DisplayAlert("Success", "Chart uploaded successfully", "OK");
                }
            }
        }
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", $"Upload failed: {ex.Message}", "OK");
    }
}
```

### Example: Embed in PDF Document

```csharp
private async Task EmbedChartInPdfAsync()
{
    try
    {
        // Get chart as PNG stream
        Stream chartStream = await funnelChart.GetStreamAsync(ImageFileFormat.Png);
        
        // Use a PDF library (e.g., Syncfusion PDF, iTextSharp)
        // This is a conceptual example
        
        // Convert stream to byte array
        using (MemoryStream ms = new MemoryStream())
        {
            await chartStream.CopyToAsync(ms);
            byte[] chartBytes = ms.ToArray();
            
            // Create PDF and embed image
            // (Implementation depends on your PDF library)
            
            await DisplayAlert("Success", "Chart embedded in PDF", "OK");
        }
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", $"PDF generation failed: {ex.Message}", "OK");
    }
}
```

### Example: Share Chart via Platform Share Sheet

```csharp
private async Task ShareChartAsync()
{
    try
    {
        // Get chart as stream
        Stream chartStream = await funnelChart.GetStreamAsync(ImageFileFormat.Png);
        
        // Save to temporary file
        string fileName = "FunnelChart.png";
        string filePath = Path.Combine(FileSystem.CacheDirectory, fileName);
        
        using (FileStream fileStream = File.Create(filePath))
        {
            await chartStream.CopyToAsync(fileStream);
        }
        
        // Share using .NET MAUI Share API
        await Share.RequestAsync(new ShareFileRequest
        {
            Title = "Share Funnel Chart",
            File = new ShareFile(filePath)
        });
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", $"Sharing failed: {ex.Message}", "OK");
    }
}
```

## Complete Export Example with UI

### XAML
```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:chart="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts"
             xmlns:model="clr-namespace:YourApp.ViewModels">

    <Grid RowDefinitions="*, Auto">
        
        <!-- Chart -->
        <chart:SfFunnelChart x:Name="funnelChart"
                             Grid.Row="0"
                             ItemsSource="{Binding Data}"
                             XBindingPath="Stage"
                             YBindingPath="Value"
                             ShowDataLabels="True">
            
            <chart:SfFunnelChart.Title>
                <Label Text="Sales Conversion Funnel" 
                       FontSize="20" 
                       FontAttributes="Bold"/>
            </chart:SfFunnelChart.Title>
            
            <chart:SfFunnelChart.BindingContext>
                <model:SalesFunnelViewModel/>
            </chart:SfFunnelChart.BindingContext>
            
            <chart:SfFunnelChart.Legend>
                <chart:ChartLegend Placement="Bottom"/>
            </chart:SfFunnelChart.Legend>
            
        </chart:SfFunnelChart>
        
        <!-- Export Controls -->
        <VerticalStackLayout Grid.Row="1" 
                            Padding="15" 
                            Spacing="10"
                            BackgroundColor="LightGray">
            
            <Label Text="Export Options" 
                   FontSize="16" 
                   FontAttributes="Bold"/>
            
            <HorizontalStackLayout Spacing="10">
                <Button Text="Save as PNG" 
                        Clicked="OnSaveAsPngClicked"
                        BackgroundColor="DodgerBlue"/>
                <Button Text="Save as JPEG" 
                        Clicked="OnSaveAsJpegClicked"
                        BackgroundColor="DodgerBlue"/>
                <Button Text="Share" 
                        Clicked="OnShareClicked"
                        BackgroundColor="Green"/>
            </HorizontalStackLayout>
            
        </VerticalStackLayout>
        
    </Grid>

</ContentPage>
```

### C# Code-Behind
```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }

    private void OnSaveAsPngClicked(object sender, EventArgs e)
    {
        try
        {
            string fileName = $"FunnelChart_{DateTime.Now:yyyyMMdd_HHmmss}.png";
            funnelChart.SaveAsImage(fileName);
            DisplayAlert("Success", $"Chart saved as {fileName}", "OK");
        }
        catch (Exception ex)
        {
            DisplayAlert("Error", $"Failed to save: {ex.Message}", "OK");
        }
    }

    private void OnSaveAsJpegClicked(object sender, EventArgs e)
    {
        try
        {
            string fileName = $"FunnelChart_{DateTime.Now:yyyyMMdd_HHmmss}.jpeg";
            funnelChart.SaveAsImage(fileName);
            DisplayAlert("Success", $"Chart saved as {fileName}", "OK");
        }
        catch (Exception ex)
        {
            DisplayAlert("Error", $"Failed to save: {ex.Message}", "OK");
        }
    }

    private async void OnShareClicked(object sender, EventArgs e)
    {
        try
        {
            // Get chart as stream
            Stream chartStream = await funnelChart.GetStreamAsync(ImageFileFormat.Png);
            
            // Save to cache
            string fileName = "FunnelChart.png";
            string filePath = Path.Combine(FileSystem.CacheDirectory, fileName);
            
            using (FileStream fileStream = File.Create(filePath))
            {
                await chartStream.CopyToAsync(fileStream);
            }
            
            // Share
            await Share.RequestAsync(new ShareFileRequest
            {
                Title = "Share Funnel Chart",
                File = new ShareFile(filePath)
            });
        }
        catch (Exception ex)
        {
            await DisplayAlert("Error", $"Sharing failed: {ex.Message}", "OK");
        }
    }
}
```

## Best Practices

1. **Timing:**
   - Export only after the chart is fully rendered
   - Ensure `Content` is set and chart is in visual tree
   - Wait for data binding to complete

2. **File Naming:**
   - Use descriptive, unique filenames
   - Include timestamps to avoid overwriting
   - Use appropriate file extensions (.png, .jpeg)

3. **Error Handling:**
   - Wrap export calls in try-catch blocks
   - Validate chart state before exporting
   - Handle permission denials gracefully

4. **Format Selection:**
   - Use **PNG** for charts with transparency or sharp text
   - Use **JPEG** for smaller file sizes (no transparency)
   - PNG is generally recommended for charts

5. **Permissions:**
   - Request permissions before exporting
   - Provide clear permission descriptions
   - Handle permission denials appropriately

6. **Stream Management:**
   - Dispose of streams after use
   - Use `using` statements for automatic disposal
   - Close file handles promptly

## Troubleshooting

**Export fails silently:**
- Verify chart is added to visual tree (`this.Content = chart`)
- Ensure chart has finished rendering
- Check for platform-specific permissions

**Permission denied errors:**
- Add required permissions to platform manifest files
- Request runtime permissions if needed (Android 6+)
- Verify permission descriptions in Info.plist (iOS)

**File not found after export:**
- Check platform-specific save locations
- Verify file system permissions
- Look in device's Pictures/Photos folders

**Stream is null or empty:**
- Ensure chart is visible and rendered
- Wait for async operations to complete
- Verify chart contains data

**Poor image quality:**
- PNG provides better quality than JPEG for charts
- Ensure chart has sufficient size before export
- Check that chart isn't being scaled down