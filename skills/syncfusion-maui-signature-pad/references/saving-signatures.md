# Saving and Retrieving Signatures

This guide covers all methods for saving, exporting, and retrieving signature data from the SignaturePad control, including image export and raw signature point data.

## Table of Contents
- [Overview](#overview)
- [ToImageSource Method](#toimagesource-method)
- [Saving Signatures to File System](#saving-signatures-to-file-system)
- [GetSignaturePoints Method](#getsignaturepoints-method)
- [Working with Signature Point Data](#working-with-signature-point-data)
- [Integration with Documents and PDFs](#integration-with-documents-and-pdfs)
- [Database Storage Strategies](#database-storage-strategies)
- [Cloud Synchronization](#cloud-synchronization)
- [Complete Workflows](#complete-workflows)

## Overview

SignaturePad provides two primary methods for signature retrieval:

1. **ToImageSource()** - Converts signature to an image (ImageSource) for display, saving, or embedding
2. **GetSignaturePoints()** - Retrieves raw point data for custom rendering, analysis, or storage

Choose the method based on your use case:
- **Image export:** Easy integration with documents, UI display, simple storage
- **Point data:** Custom rendering, signature verification, compression, cross-platform compatibility

## ToImageSource Method

Converts the drawn signature into an `ImageSource` that can be displayed, saved, or processed.

### Method Signature

```csharp
public ImageSource? ToImageSource()
```

### Return Value
- **Type:** `ImageSource?` (nullable)
- **Returns:** Image representation of the signature
- **Null:** If no signature has been drawn

### Basic Usage (XAML + Code-Behind)

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*" />
        <RowDefinition Height="Auto" />
        <RowDefinition Height="200" />
    </Grid.RowDefinitions>
    
    <signaturePad:SfSignaturePad x:Name="signaturePad" Grid.Row="0" />
    
    <Button Text="Save Signature" 
            Clicked="OnSaveClicked" 
            Grid.Row="1" />
    
    <Image x:Name="signatureImage" 
           Grid.Row="2"
           Aspect="AspectFit" />
</Grid>
```

```csharp
private void OnSaveClicked(object sender, EventArgs e)
{
    ImageSource? signature = signaturePad.ToImageSource();
    
    if (signature != null)
    {
        // Display the signature
        signatureImage.Source = signature;
        
        DisplayAlert("Success", "Signature captured!", "OK");
    }
    else
    {
        DisplayAlert("Error", "Please draw a signature first", "OK");
    }
}
```

### Basic Usage (C# Only)

```csharp
public class SignaturePage : ContentPage
{
    private SfSignaturePad signaturePad;
    private Image signaturePreview;

    public SignaturePage()
    {
        signaturePad = new SfSignaturePad
        {
            HeightRequest = 300
        };

        signaturePreview = new Image
        {
            HeightRequest = 200,
            Aspect = Aspect.AspectFit
        };

        var saveButton = new Button { Text = "Save Signature" };
        saveButton.Clicked += OnSaveClicked;

        Content = new VerticalStackLayout
        {
            Children = { signaturePad, saveButton, signaturePreview }
        };
    }

    private void OnSaveClicked(object sender, EventArgs e)
    {
        ImageSource? signature = signaturePad.ToImageSource();
        
        if (signature != null)
        {
            signaturePreview.Source = signature;
        }
    }
}
```

### Null Check Pattern

Always check for null before using the signature:

```csharp
private void SaveSignature()
{
    ImageSource? signature = signaturePad.ToImageSource();
    
    if (signature == null)
    {
        // No signature drawn - handle gracefully
        DisplayAlert("Info", "Please draw your signature", "OK");
        return;
    }
    
    // Process the signature
    ProcessSignatureImage(signature);
}
```

## Saving Signatures to File System

Convert the ImageSource to a file for persistent storage.

### Save as PNG (Recommended)

```csharp
using System.IO;

private async Task<string?> SaveSignatureAsPngAsync()
{
    ImageSource? signature = signaturePad.ToImageSource();
    
    if (signature == null)
        return null;

    try
    {
        // Convert ImageSource to Stream
        if (signature is StreamImageSource streamSource)
        {
            var stream = await streamSource.Stream(CancellationToken.None);
            
            if (stream != null)
            {
                // Generate unique filename
                string fileName = $"signature_{DateTime.Now:yyyyMMdd_HHmmss}.png";
                string filePath = Path.Combine(FileSystem.AppDataDirectory, fileName);
                
                // Save to file
                using (var fileStream = File.Create(filePath))
                {
                    await stream.CopyToAsync(fileStream);
                }
                
                return filePath;
            }
        }
    }
    catch (Exception ex)
    {
        Console.WriteLine($"Error saving signature: {ex.Message}");
    }
    
    return null;
}
```

### Save with User File Picker

```csharp
private async Task SaveSignatureWithPickerAsync()
{
    ImageSource? signature = signaturePad.ToImageSource();
    
    if (signature == null)
    {
        await DisplayAlert("Error", "No signature to save", "OK");
        return;
    }

    try
    {
        // For MAUI, you might use FileSaver or community packages
        // This is a simplified example
        string tempPath = Path.Combine(FileSystem.CacheDirectory, "temp_signature.png");
        
        // Save to temp file first (using similar logic as above)
        // Then use platform-specific file save dialog
        
        await DisplayAlert("Success", $"Signature saved to {tempPath}", "OK");
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", $"Failed to save: {ex.Message}", "OK");
    }
}
```

### Complete Save Example with Workflow

```csharp
public class SignatureWorkflow
{
    private SfSignaturePad signaturePad;
    private string? savedFilePath;

    public async Task<bool> CaptureAndSaveSignatureAsync()
    {
        // 1. Get signature from pad
        ImageSource? signature = signaturePad.ToImageSource();
        
        if (signature == null)
        {
            Console.WriteLine("No signature to save");
            return false;
        }

        // 2. Save to file system
        savedFilePath = await SaveToFileAsync(signature);
        
        if (string.IsNullOrEmpty(savedFilePath))
        {
            Console.WriteLine("Failed to save signature");
            return false;
        }

        // 3. Optional: Upload to server or cloud
        bool uploaded = await UploadSignatureAsync(savedFilePath);

        // 4. Optional: Clear the pad
        signaturePad.Clear();

        return uploaded;
    }

    private async Task<string?> SaveToFileAsync(ImageSource signature)
    {
        // Implementation similar to SaveSignatureAsPngAsync above
        // Returns file path on success, null on failure
        return null; // Placeholder
    }

    private async Task<bool> UploadSignatureAsync(string filePath)
    {
        // Upload logic here
        return true; // Placeholder
    }
}
```

## GetSignaturePoints Method

Retrieves the raw signature data as a collection of point coordinates.

### Method Signature

```csharp
public List<List<float>> GetSignaturePoints()
```

### Return Value
- **Type:** `List<List<float>>`
- **Structure:** List of strokes, each stroke is a list of float values
- **Format:** Each stroke contains [x1, y1, x2, y2, x3, y3, ...] coordinates

### Basic Usage

```csharp
private void GetSignatureData()
{
    List<List<float>> signaturePoints = signaturePad.GetSignaturePoints();
    
    int strokeCount = signaturePoints.Count;
    Console.WriteLine($"Signature has {strokeCount} strokes");
    
    foreach (var stroke in signaturePoints)
    {
        int pointCount = stroke.Count / 2; // Divide by 2 because each point is x,y pair
        Console.WriteLine($"Stroke has {pointCount} points");
    }
}
```

### Complete Example with DrawCompleted Event

```xml
<signaturePad:SfSignaturePad x:Name="signaturePad"
                              DrawCompleted="OnDrawCompleted" />
```

```csharp
private void OnDrawCompleted(object sender, EventArgs e)
{
    List<List<float>> points = signaturePad.GetSignaturePoints();
    AnalyzeSignature(points);
}

private void AnalyzeSignature(List<List<float>> points)
{
    int totalStrokes = points.Count;
    int totalPoints = points.Sum(stroke => stroke.Count / 2);
    
    Console.WriteLine($"Signature Analysis:");
    Console.WriteLine($"- Total Strokes: {totalStrokes}");
    Console.WriteLine($"- Total Points: {totalPoints}");
    Console.WriteLine($"- Average Points per Stroke: {totalPoints / (double)totalStrokes:F2}");
}
```

## Working with Signature Point Data

### Understanding the Data Structure

```csharp
List<List<float>> points = signaturePad.GetSignaturePoints();

// Structure:
// points[0] = First stroke [x1, y1, x2, y2, x3, y3, ...]
// points[1] = Second stroke [x1, y1, x2, y2, ...]
// points[n] = Nth stroke

// Example: Extract first point of first stroke
if (points.Count > 0 && points[0].Count >= 2)
{
    float firstX = points[0][0];
    float firstY = points[0][1];
    Console.WriteLine($"First point: ({firstX}, {firstY})");
}
```

### Extracting Individual Points

```csharp
private List<Point> ExtractPointsFromStroke(List<float> stroke)
{
    var points = new List<Point>();
    
    for (int i = 0; i < stroke.Count; i += 2)
    {
        if (i + 1 < stroke.Count)
        {
            points.Add(new Point(stroke[i], stroke[i + 1]));
        }
    }
    
    return points;
}

// Usage
List<List<float>> signatureData = signaturePad.GetSignaturePoints();
foreach (var stroke in signatureData)
{
    List<Point> strokePoints = ExtractPointsFromStroke(stroke);
    // Process points...
}
```

### Calculating Signature Bounding Box

```csharp
private Rect CalculateBoundingBox(List<List<float>> signaturePoints)
{
    if (signaturePoints.Count == 0)
        return Rect.Zero;

    float minX = float.MaxValue;
    float minY = float.MaxValue;
    float maxX = float.MinValue;
    float maxY = float.MinValue;

    foreach (var stroke in signaturePoints)
    {
        for (int i = 0; i < stroke.Count; i += 2)
        {
            if (i + 1 < stroke.Count)
            {
                float x = stroke[i];
                float y = stroke[i + 1];
                
                minX = Math.Min(minX, x);
                minY = Math.Min(minY, y);
                maxX = Math.Max(maxX, x);
                maxY = Math.Max(maxY, y);
            }
        }
    }

    return new Rect(minX, minY, maxX - minX, maxY - minY);
}
```

### Serializing Point Data (JSON)

```csharp
using System.Text.Json;

private string SerializeSignaturePoints(List<List<float>> points)
{
    return JsonSerializer.Serialize(points);
}

private List<List<float>> DeserializeSignaturePoints(string json)
{
    return JsonSerializer.Deserialize<List<List<float>>>(json) 
           ?? new List<List<float>>();
}

// Usage
private async Task SavePointsToFileAsync()
{
    List<List<float>> points = signaturePad.GetSignaturePoints();
    string json = SerializeSignaturePoints(points);
    
    string filePath = Path.Combine(FileSystem.AppDataDirectory, "signature_points.json");
    await File.WriteAllTextAsync(filePath, json);
}
```

### Signature Complexity Analysis

```csharp
private SignatureStats AnalyzeSignatureComplexity(List<List<float>> points)
{
    int strokeCount = points.Count;
    int totalPoints = 0;
    double totalDistance = 0;

    foreach (var stroke in points)
    {
        int pointsInStroke = stroke.Count / 2;
        totalPoints += pointsInStroke;
        
        // Calculate stroke length
        for (int i = 0; i < stroke.Count - 2; i += 2)
        {
            float dx = stroke[i + 2] - stroke[i];
            float dy = stroke[i + 3] - stroke[i + 1];
            totalDistance += Math.Sqrt(dx * dx + dy * dy);
        }
    }

    return new SignatureStats
    {
        StrokeCount = strokeCount,
        TotalPoints = totalPoints,
        TotalDistance = totalDistance,
        AveragePointsPerStroke = totalPoints / (double)strokeCount,
        Complexity = (strokeCount * totalPoints) / 100.0 // Custom metric
    };
}

public class SignatureStats
{
    public int StrokeCount { get; set; }
    public int TotalPoints { get; set; }
    public double TotalDistance { get; set; }
    public double AveragePointsPerStroke { get; set; }
    public double Complexity { get; set; }
}
```

## Integration with Documents and PDFs

### Embedding in PDF (Conceptual)

```csharp
private async Task AddSignatureToPdfAsync(string pdfPath)
{
    ImageSource? signature = signaturePad.ToImageSource();
    
    if (signature == null)
        return;

    // Save signature as temporary image
    string tempImagePath = await SaveSignatureAsPngAsync();
    
    if (string.IsNullOrEmpty(tempImagePath))
        return;

    // Use PDF library (e.g., Syncfusion PDF, iTextSharp, etc.)
    // to add image to PDF at specific location
    
    // Example (pseudocode):
    // var pdfDocument = PdfDocument.Load(pdfPath);
    // var page = pdfDocument.Pages[0];
    // page.Graphics.DrawImage(tempImagePath, x: 100, y: 500, width: 200, height: 100);
    // pdfDocument.Save();
}
```

### Creating Signature Stamp

```csharp
private async Task<ImageSource?> CreateSignatureStampAsync()
{
    ImageSource? signature = signaturePad.ToImageSource();
    
    if (signature == null)
        return null;

    // Could add timestamp, border, or other elements
    // For now, returning the signature as-is
    
    return signature;
}
```

## Database Storage Strategies

### Strategy 1: Store as Base64 String

```csharp
private async Task<string?> ConvertSignatureToBase64Async()
{
    ImageSource? signature = signaturePad.ToImageSource();
    
    if (signature == null || signature is not StreamImageSource streamSource)
        return null;

    var stream = await streamSource.Stream(CancellationToken.None);
    
    if (stream == null)
        return null;

    using (var memoryStream = new MemoryStream())
    {
        await stream.CopyToAsync(memoryStream);
        byte[] imageBytes = memoryStream.ToArray();
        return Convert.ToBase64String(imageBytes);
    }
}

// Storing in database
private async Task SaveToDatabase(string userId)
{
    string? base64Signature = await ConvertSignatureToBase64Async();
    
    if (string.IsNullOrEmpty(base64Signature))
        return;

    // Save to your database
    // await database.InsertAsync(new SignatureRecord 
    // { 
    //     UserId = userId, 
    //     SignatureData = base64Signature,
    //     CreatedAt = DateTime.UtcNow 
    // });
}
```

### Strategy 2: Store Point Data (JSON)

```csharp
private async Task SavePointsToDatabase(string userId)
{
    List<List<float>> points = signaturePad.GetSignaturePoints();
    string json = JsonSerializer.Serialize(points);
    
    // Save to database
    // await database.InsertAsync(new SignatureRecord 
    // { 
    //     UserId = userId, 
    //     PointsJson = json,
    //     CreatedAt = DateTime.UtcNow 
    // });
}
```

### Strategy 3: Store File Reference

```csharp
private async Task SaveWithFileReferenceAsync(string userId)
{
    string? filePath = await SaveSignatureAsPngAsync();
    
    if (string.IsNullOrEmpty(filePath))
        return;

    // Save file reference to database
    // await database.InsertAsync(new SignatureRecord 
    // { 
    //     UserId = userId, 
    //     FilePath = filePath,
    //     CreatedAt = DateTime.UtcNow 
    // });
}
```

## Cloud Synchronization

### Upload to Cloud Storage

```csharp
private async Task<bool> UploadSignatureToCloudAsync(string userId)
{
    ImageSource? signature = signaturePad.ToImageSource();
    
    if (signature == null)
        return false;

    try
    {
        // Save locally first
        string? localPath = await SaveSignatureAsPngAsync();
        
        if (string.IsNullOrEmpty(localPath))
            return false;

        // Upload to cloud (Azure Blob, AWS S3, Firebase Storage, etc.)
        // string cloudUrl = await cloudService.UploadFileAsync(localPath);
        
        // Save cloud URL to database
        // await SaveCloudReferenceAsync(userId, cloudUrl);
        
        return true;
    }
    catch (Exception ex)
    {
        Console.WriteLine($"Cloud upload failed: {ex.Message}");
        return false;
    }
}
```

## Complete Workflows

### Workflow 1: Capture, Save, and Display

```csharp
public class SignatureCaptureWorkflow
{
    private SfSignaturePad signaturePad;
    private Image previewImage;
    
    public async Task ExecuteWorkflowAsync()
    {
        // Step 1: Wait for user to draw
        // (handle via UI events)
        
        // Step 2: Capture signature
        ImageSource? signature = signaturePad.ToImageSource();
        
        if (signature == null)
        {
            Console.WriteLine("No signature captured");
            return;
        }
        
        // Step 3: Display preview
        previewImage.Source = signature;
        
        // Step 4: Save to file
        string? filePath = await SaveSignatureAsync(signature);
        
        // Step 5: Upload to cloud (optional)
        if (!string.IsNullOrEmpty(filePath))
        {
            await UploadToCloudAsync(filePath);
        }
        
        // Step 6: Clear pad for next signature
        signaturePad.Clear();
    }
    
    private async Task<string?> SaveSignatureAsync(ImageSource signature)
    {
        // Implementation here
        return null;
    }
    
    private async Task UploadToCloudAsync(string filePath)
    {
        // Implementation here
    }
}
```

### Workflow 2: Signature Verification with Point Data

```csharp
public class SignatureVerificationWorkflow
{
    public async Task<bool> VerifySignatureAsync(string userId)
    {
        // Get current signature points
        List<List<float>> currentPoints = signaturePad.GetSignaturePoints();
        
        // Load stored signature points from database
        List<List<float>> storedPoints = await LoadStoredSignatureAsync(userId);
        
        // Compare signatures
        double similarity = CalculateSimilarity(currentPoints, storedPoints);
        
        // Threshold for verification (e.g., 85% similarity)
        return similarity >= 0.85;
    }
    
    private async Task<List<List<float>>> LoadStoredSignatureAsync(string userId)
    {
        // Load from database
        return new List<List<float>>();
    }
    
    private double CalculateSimilarity(List<List<float>> sig1, List<List<float>> sig2)
    {
        // Implement similarity algorithm
        // This is a simplified example
        return 0.9;
    }
}
```

## Error Handling Best Practices

```csharp
private async Task<bool> SafelySaveSignatureAsync()
{
    try
    {
        ImageSource? signature = signaturePad.ToImageSource();
        
        if (signature == null)
        {
            await ShowErrorAsync("Please draw a signature first");
            return false;
        }
        
        string? filePath = await SaveSignatureAsPngAsync();
        
        if (string.IsNullOrEmpty(filePath))
        {
            await ShowErrorAsync("Failed to save signature");
            return false;
        }
        
        await ShowSuccessAsync($"Signature saved to {Path.GetFileName(filePath)}");
        return true;
    }
    catch (UnauthorizedAccessException)
    {
        await ShowErrorAsync("Permission denied. Please grant storage access.");
        return false;
    }
    catch (IOException ex)
    {
        await ShowErrorAsync($"File error: {ex.Message}");
        return false;
    }
    catch (Exception ex)
    {
        await ShowErrorAsync($"Unexpected error: {ex.Message}");
        Console.WriteLine($"Error details: {ex}");
        return false;
    }
}

private Task ShowErrorAsync(string message)
{
    return DisplayAlert("Error", message, "OK");
}

private Task ShowSuccessAsync(string message)
{
    return DisplayAlert("Success", message, "OK");
}
```
