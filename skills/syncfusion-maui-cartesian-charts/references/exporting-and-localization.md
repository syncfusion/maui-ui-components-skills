# Exporting and Localization

This guide covers exporting Cartesian charts as images and localizing chart content for multiple languages and cultures.

## Chart Exporting

Export charts as images for sharing, reporting, or documentation purposes. Syncfusion .NET MAUI Cartesian Chart supports exporting to JPEG and PNG formats.

### SaveAsImage Method

The `SaveAsImage` method exports the chart as an image file to device storage.

**Requirements:**
- Chart must be added to the visual tree (rendered on screen)
- File write permissions required on mobile platforms

### Basic Export

```csharp
// In your page or view model
SfCartesianChart chart = new SfCartesianChart();
// Configure chart...
this.Content = chart;

// Export as PNG (default)
chart.SaveAsImage("ChartReport.png");

// Export as JPEG
chart.SaveAsImage("ChartReport.jpeg");
```

### Export Locations by Platform

**Android:**
- Saved to: `Pictures` directory
- Full path: `/storage/emulated/0/Pictures/`

**iOS:**
- Saved to: `Photos/Album` directory
- Accessible through Photos app

**Windows:**
- Saved to: `Pictures` library
- Full path: `C:\Users\{Username}\Pictures\`

**macOS:**
- Saved to: `Pictures` directory
- Full path: `/Users/{Username}/Pictures/`

### Platform Permissions

#### Android Permissions

Add to `AndroidManifest.xml`:

```xml
<manifest>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
</manifest>
```

For Android 10+ (API 29+), also add:

```xml
<application android:requestLegacyExternalStorage="true">
    ...
</application>
```

Request runtime permissions in code:

```csharp
// In your MainActivity or page
if (Build.VERSION.SdkInt >= BuildVersionCodes.M)
{
    if (CheckSelfPermission(Manifest.Permission.WriteExternalStorage) != Permission.Granted)
    {
        RequestPermissions(new string[] { 
            Manifest.Permission.WriteExternalStorage,
            Manifest.Permission.ReadExternalStorage
        }, 1);
    }
}
```

#### iOS Permissions

Add to `Info.plist`:

```xml
<dict>
    <key>NSPhotoLibraryUsageDescription</key>
    <string>This app needs permission to save charts to your photo library</string>
    
    <key>NSPhotoLibraryAddUsageDescription</key>
    <string>This app needs permission to add charts to your photo library</string>
</dict>
```

### GetStreamAsync Method

Get the chart as a stream for advanced scenarios (email attachments, PDF generation, cloud storage).

```csharp
using Syncfusion.Maui.Core;

SfCartesianChart chart = new SfCartesianChart();
// Configure chart...
this.Content = chart;

// Get as PNG stream
Stream pngStream = await chart.GetStreamAsync(ImageFileFormat.Png);

// Get as JPEG stream
Stream jpegStream = await chart.GetStreamAsync(ImageFileFormat.Jpeg);
```

### Using Stream for Email Attachment

```csharp
public async Task EmailChartAsync()
{
    // Get chart stream
    Stream chartStream = await chart.GetStreamAsync(ImageFileFormat.Png);
    
    // Save to temporary file
    string filePath = Path.Combine(FileSystem.CacheDirectory, "chart.png");
    using (FileStream fileStream = File.Create(filePath))
    {
        await chartStream.CopyToAsync(fileStream);
    }
    
    // Create email message
    var message = new EmailMessage
    {
        Subject = "Chart Report",
        Body = "Please find the chart attached.",
        To = new List<string> { "recipient@example.com" }
    };
    
    // Attach file
    message.Attachments.Add(new EmailAttachment(filePath));
    
    // Send
    await Email.ComposeAsync(message);
}
```

### Using Stream for PDF Generation

```csharp
public async Task AddChartToPdfAsync()
{
    // Get chart as stream
    Stream chartStream = await chart.GetStreamAsync(ImageFileFormat.Png);
    
    // Convert to byte array
    byte[] chartBytes;
    using (MemoryStream ms = new MemoryStream())
    {
        await chartStream.CopyToAsync(ms);
        chartBytes = ms.ToArray();
    }
    
    // Add to PDF (example with a PDF library)
    // PdfDocument doc = new PdfDocument();
    // PdfPage page = doc.Pages.Add();
    // PdfImage image = new PdfBitmap(new MemoryStream(chartBytes));
    // page.Graphics.DrawImage(image, new PointF(0, 0));
    // doc.Save("Report.pdf");
}
```

### Export with Button Click

```xml
<StackLayout>
    <chart:SfCartesianChart x:Name="chart">
        <chart:SfCartesianChart.XAxes>
            <chart:CategoryAxis/>
        </chart:SfCartesianChart.XAxes>
        
        <chart:SfCartesianChart.YAxes>
            <chart:NumericalAxis/>
        </chart:SfCartesianChart.YAxes>
        
        <chart:ColumnSeries ItemsSource="{Binding Data}"
                           XBindingPath="Category"
                           YBindingPath="Value"/>
    </chart:SfCartesianChart>
    
    <Button Text="Export Chart"
            Clicked="OnExportClicked"/>
</StackLayout>
```

```csharp
private async void OnExportClicked(object sender, EventArgs e)
{
    try
    {
        chart.SaveAsImage("ChartExport.png");
        await DisplayAlert("Success", "Chart exported successfully!", "OK");
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", $"Failed to export: {ex.Message}", "OK");
    }
}
```

## Localization

Localization adapts chart content (axis labels, tooltips, legends) to different languages and cultures.

### Setting Application Culture

Configure culture in `App.xaml.cs`:

```csharp
using System.Globalization;
using System.Resources;
using Syncfusion.Maui.Charts;

public partial class App : Application
{
    public App()
    {
        InitializeComponent();
        
        // Set desired culture
        CultureInfo.CurrentUICulture = new CultureInfo("fr-FR");  // French
        
        // Register resource manager
        string resxPath = "YourNamespace.Resources.SfCartesianChart";
        SfCartesianChartResources.ResourceManager = new ResourceManager(
            resxPath, 
            Application.Current.GetType().Assembly
        );
        
        MainPage = new MainPage();
    }
}
```

### Creating Resource Files

1. **Create Resources folder** in your project
2. **Add resource file**: Right-click Resources → Add → New Item → Resource File
3. **Name pattern**: `SfCartesianChart.<culture-code>.resx`
   - Example: `SfCartesianChart.fr-FR.resx` (French)
   - Example: `SfCartesianChart.de-DE.resx` (German)
   - Example: `SfCartesianChart.es-ES.resx` (Spanish)
4. **Set Build Action**: EmbeddedResource

### Resource File Structure

**SfCartesianChart.fr-FR.resx** example:

| Name | Value (French) |
|------|---------------|
| Trackball | Boule de suivi |
| Close | Fermer |
| High | Haut |
| Low | Bas |
| Open | Ouvert |
| Volume | Volume |

**SfCartesianChart.de-DE.resx** example:

| Name | Value (German) |
|------|---------------|
| Trackball | Trackball |
| Close | Schließen |
| High | Hoch |
| Low | Niedrig |
| Open | Öffnen |
| Volume | Volumen |

### Localizing Axis Labels

```csharp
// Format numbers according to culture
NumericalAxis yAxis = new NumericalAxis();
yAxis.LabelCreated += (sender, e) =>
{
    if (double.TryParse(e.Label, out double value))
    {
        // Format with current culture
        e.Label = value.ToString("N0", CultureInfo.CurrentCulture);
    }
};
```

### Localizing Date Labels

```csharp
DateTimeAxis xAxis = new DateTimeAxis
{
    IntervalType = DateTimeIntervalType.Months
};

xAxis.LabelCreated += (sender, e) =>
{
    if (DateTime.TryParse(e.Label, out DateTime date))
    {
        // Format date with current culture
        e.Label = date.ToString("MMM yyyy", CultureInfo.CurrentCulture);
    }
};
```

### Currency Formatting

```csharp
NumericalAxis yAxis = new NumericalAxis();
yAxis.LabelCreated += (sender, e) =>
{
    if (double.TryParse(e.Label, out double value))
    {
        // Format as currency for current culture
        e.Label = value.ToString("C0", CultureInfo.CurrentCulture);
        // US: $1,000
        // France: 1 000 €
        // Germany: 1.000 €
    }
};
```

### Custom Tooltip Localization

```csharp
public class LocalizedTooltipBehavior : ChartTooltipBehavior
{
    protected override string GetLabelText(ChartDataPointInfo dataPointInfo)
    {
        // Get localized label from resources
        string label = GetLocalizedString("ChartValue");
        string formattedValue = dataPointInfo.DataPoint.YValue.ToString("N2", CultureInfo.CurrentCulture);
        
        return $"{label}: {formattedValue}";
    }
    
    private string GetLocalizedString(string key)
    {
        // Get from resource file
        return SfCartesianChartResources.ResourceManager.GetString(key, CultureInfo.CurrentCulture);
    }
}
```

### Switching Culture at Runtime

```xml
<ContentPage>
    <StackLayout>
        <Picker x:Name="culturePicker"
                Title="Select Language"
                SelectedIndexChanged="OnCultureChanged">
            <Picker.Items>
                <x:String>English (en-US)</x:String>
                <x:String>French (fr-FR)</x:String>
                <x:String>German (de-DE)</x:String>
                <x:String>Spanish (es-ES)</x:String>
            </Picker.Items>
        </Picker>
        
        <chart:SfCartesianChart x:Name="chart">
            <!-- Chart configuration -->
        </chart:SfCartesianChart>
    </StackLayout>
</ContentPage>
```

```csharp
private void OnCultureChanged(object sender, EventArgs e)
{
    string selectedCulture = culturePicker.SelectedIndex switch
    {
        0 => "en-US",
        1 => "fr-FR",
        2 => "de-DE",
        3 => "es-ES",
        _ => "en-US"
    };
    
    // Change culture
    CultureInfo.CurrentUICulture = new CultureInfo(selectedCulture);
    
    // Recreate chart to apply new culture
    RefreshChart();
}

private void RefreshChart()
{
    // Reload chart with new culture settings
    // This typically involves rebinding data or recreating the chart
}
```

## Complete Example

### Exportable and Localizable Chart

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:chart="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts"
             x:Class="ChartApp.MainPage">
    
    <StackLayout Padding="10">
        <Label Text="Sales Report" 
               FontSize="20" 
               FontAttributes="Bold"
               HorizontalOptions="Center"/>
        
        <chart:SfCartesianChart x:Name="salesChart" HeightRequest="400">
            <chart:SfCartesianChart.Title>
                <Label Text="Monthly Sales"/>
            </chart:SfCartesianChart.Title>
            
            <chart:SfCartesianChart.XAxes>
                <chart:CategoryAxis/>
            </chart:SfCartesianChart.XAxes>
            
            <chart:SfCartesianChart.YAxes>
                <chart:NumericalAxis/>
            </chart:SfCartesianChart.YAxes>
            
            <chart:SfCartesianChart.TrackballBehavior>
                <chart:ChartTrackballBehavior ShowLabel="True"/>
            </chart:SfCartesianChart.TrackballBehavior>
            
            <chart:ColumnSeries ItemsSource="{Binding SalesData}"
                               XBindingPath="Month"
                               YBindingPath="Amount"
                               EnableTooltip="True"/>
        </chart:SfCartesianChart>
        
        <Grid ColumnDefinitions="*,*" ColumnSpacing="10" Margin="0,20,0,0">
            <Button Text="Export PNG" 
                    Grid.Column="0"
                    Clicked="OnExportPngClicked"/>
            
            <Button Text="Export JPEG"
                    Grid.Column="1"
                    Clicked="OnExportJpegClicked"/>
        </Grid>
    </StackLayout>
</ContentPage>
```

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        ConfigureLocalization();
    }
    
    private void ConfigureLocalization()
    {
        // Configure Y-axis for currency formatting
        if (salesChart.YAxes.Count > 0 && salesChart.YAxes[0] is NumericalAxis yAxis)
        {
            yAxis.LabelCreated += (s, e) =>
            {
                if (double.TryParse(e.Label, out double value))
                {
                    e.Label = value.ToString("C0", CultureInfo.CurrentCulture);
                }
            };
        }
    }
    
    private async void OnExportPngClicked(object sender, EventArgs e)
    {
        try
        {
            string fileName = $"SalesReport_{DateTime.Now:yyyyMMdd_HHmmss}.png";
            salesChart.SaveAsImage(fileName);
            await DisplayAlert("Success", $"Chart exported as {fileName}", "OK");
        }
        catch (Exception ex)
        {
            await DisplayAlert("Error", $"Export failed: {ex.Message}", "OK");
        }
    }
    
    private async void OnExportJpegClicked(object sender, EventArgs e)
    {
        try
        {
            string fileName = $"SalesReport_{DateTime.Now:yyyyMMdd_HHmmss}.jpeg";
            salesChart.SaveAsImage(fileName);
            await DisplayAlert("Success", $"Chart exported as {fileName}", "OK");
        }
        catch (Exception ex)
        {
            await DisplayAlert("Error", $"Export failed: {ex.Message}", "OK");
        }
    }
}
```

## Tips and Best Practices

### Exporting
1. **Check Permissions**: Verify storage permissions before exporting
2. **Unique Filenames**: Use timestamps to avoid overwriting files
3. **User Feedback**: Show success/error messages after export
4. **File Format**: Use PNG for quality, JPEG for smaller file size
5. **Timing**: Ensure chart is fully rendered before exporting

### Localization
1. **Resource Files**: Keep all localizable strings in resource files
2. **Date Formats**: Use culture-specific date formatting
3. **Number Formats**: Apply culture-specific number formatting
4. **Currency**: Use CultureInfo for currency symbols
5. **Testing**: Test with multiple cultures to ensure proper display
6. **RTL Support**: Consider right-to-left languages if applicable

## Common Gotchas

### Export Before Render
```csharp
// ✗ Wrong - chart not yet rendered
chart.SaveAsImage("chart.png");
this.Content = chart;

// ✓ Correct - chart rendered first
this.Content = chart;
await Task.Delay(100);  // Give time to render
chart.SaveAsImage("chart.png");
```

### Missing Permissions
Always check and request permissions on mobile platforms before calling `SaveAsImage`.

### Resource File Build Action
Resource files must have Build Action set to **EmbeddedResource**, not **Content** or other options.

### Culture Code Format
Use correct culture codes:
- ✓ `fr-FR` (French - France)
- ✓ `de-DE` (German - Germany)
- ✗ `French` (invalid)
- ✗ `DE` (invalid)
