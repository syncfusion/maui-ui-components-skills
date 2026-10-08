# Reset Functionality in Interactive Viewer

## Table of Contents
- [Reset Method](#reset-method)
- [What Reset Does](#what-reset-does)
- [When to Use Reset](#when-to-use-reset)
- [UI Integration](#ui-integration)
- [Complete Examples](#complete-examples)

## Reset Method

The `Reset()` method restores the Interactive Viewer to its initial state, clearing all user interactions.

### Basic Usage

```c#
using Syncfusion.Maui.InteractiveViewer;

// Reset to original view
viewer.Reset();
```

### XAML Integration

```xaml
<interactiveViewer:SfInteractiveViewer x:Name="viewer">
    <Image Source="image.png" Aspect="AspectFit" />
</interactiveViewer:SfInteractiveViewer>

<Button Text="Reset" Clicked="OnResetClicked" />
```

```c#
private void OnResetClicked(object sender, EventArgs e)
{
    viewer.Reset();
}
```

## What Reset Does

When you call `Reset()`, the viewer restores:

| State | Reset Behavior |
|-------|-----------------|
| **Zoom Level** | Resets to 1.0 (100% - original size) |
| **Pan Position** | Resets to default viewport center |
| **Rotation** | Resets to 0° (original orientation) |
| **Content** | Remains the same (image/content unchanged) |

### Reset Behavior Example

```c#
// Initial state
viewer.ZoomFactor = 1.0;      // 100%
viewer.Rotate();              // 90° rotation
viewer.Rotate();              // 180° rotation

// User zooms in
viewer.ZoomFactor = 5.0;      // Zoomed 5x
viewer.Rotate();              // Now at 270°

// Call reset
viewer.Reset();

// Result:
// - ZoomFactor = 1.0 (original)
// - Rotation = 0° (original orientation)
// - Pan position = default (centered)
```

## When to Use Reset

### Scenario 1: Image Gallery Navigation

When users switch to a new image, reset the previous one:

```c#
private void LoadImage(string imagePath)
{
    // Reset previous viewer state
    viewer.Reset();
    
    // Load new image
    viewer.Content = new Image 
    { 
        Source = imagePath, 
        Aspect = Aspect.AspectFit 
    };
}
```

### Scenario 2: Reset Button in UI

Provide users with a button to restore default viewing:

```xaml
<Button Text="Reset View" Clicked="OnResetClicked" />
```

```c#
private void OnResetClicked(object sender, EventArgs e)
{
    viewer.Reset();
}
```

### Scenario 3: Navigation/Page Changes

Reset when leaving a viewer page:

```c#
protected override void OnDisappearing()
{
    base.OnDisappearing();
    // Ensure clean state when page unloads
    viewer.Reset();
}
```

### Scenario 4: Error Recovery

Reset after error conditions:

```c#
private async Task LoadImageWithRetry(string imagePath)
{
    try
    {
        viewer.Reset();
        viewer.Content = new Image { Source = imagePath, Aspect = Aspect.AspectFit };
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", "Failed to load image", "OK");
        viewer.Reset();  // Reset to known good state
    }
}
```

## UI Integration

### Pattern 1: Simple Reset Button

```xaml
<Grid RowDefinitions="0.9*, 0.1*">
    <interactiveViewer:SfInteractiveViewer x:Name="viewer">
        <Image Source="image.png" Aspect="AspectFit" />
    </interactiveViewer:SfInteractiveViewer>
    
    <Button Grid.Row="1" 
            Text="Reset" 
            Clicked="OnResetClicked" />
</Grid>
```

```c#
private void OnResetClicked(object sender, EventArgs e)
{
    viewer.Reset();
}
```

### Pattern 2: Reset with Other Controls

```xaml
<Grid RowDefinitions="0.9*,Auto" Padding="10">
    <interactiveViewer:SfInteractiveViewer 
        Grid.Row="0"
        x:Name="viewer">
        <Image Source="image.png" Aspect="AspectFit" />
    </interactiveViewer:SfInteractiveViewer>
    
    <StackLayout Grid.Row="1" 
                 Orientation="Horizontal" 
                 Spacing="10"
                 Margin="0,10,0,0">
        <Button Text="Rotate" 
                Clicked="OnRotateClicked" 
                HorizontalOptions="FillAndExpand" />
        <Button Text="Reset" 
                Clicked="OnResetClicked" 
                HorizontalOptions="FillAndExpand" />
        <Button Text="Save" 
                Clicked="OnSaveClicked" 
                HorizontalOptions="FillAndExpand" />
    </StackLayout>
</Grid>
```

```c#
private void OnRotateClicked(object sender, EventArgs e)
{
    viewer.Rotate();
}

private void OnResetClicked(object sender, EventArgs e)
{
    viewer.Reset();
}

private void OnSaveClicked(object sender, EventArgs e)
{
    // Save current viewing state or image
}
```

### Pattern 3: Reset on Navigation

```xaml
<!-- Image Gallery with Navigation -->
<Grid RowDefinitions="Auto,*,Auto" Padding="10">
    <!-- Image Info -->
    <Label x:Name="imageInfo" 
           Text="Image 1 of 5" 
           FontSize="12" />
    
    <!-- Viewer -->
    <interactiveViewer:SfInteractiveViewer 
        Grid.Row="1"
        x:Name="viewer">
        <Image x:Name="viewerImage" Aspect="AspectFit" />
    </interactiveViewer:SfInteractiveViewer>
    
    <!-- Navigation -->
    <StackLayout Grid.Row="2" 
                 Orientation="Horizontal" 
                 Spacing="10"
                 Margin="0,10,0,0">
        <Button Text="< Previous" 
                Clicked="OnPreviousClicked" 
                HorizontalOptions="FillAndExpand" />
        <Button Text="Reset" 
                Clicked="OnResetClicked" 
                HorizontalOptions="FillAndExpand" />
        <Button Text="Next >" 
                Clicked="OnNextClicked" 
                HorizontalOptions="FillAndExpand" />
    </StackLayout>
</Grid>
```

```c#
private string[] images = { "image1.png", "image2.png", "image3.png", "image4.png", "image5.png" };
private int currentImageIndex = 0;

private void OnPreviousClicked(object sender, EventArgs e)
{
    viewer.Reset();  // Reset before changing image
    currentImageIndex = Math.Max(0, currentImageIndex - 1);
    UpdateImage();
}

private void OnNextClicked(object sender, EventArgs e)
{
    viewer.Reset();  // Reset before changing image
    currentImageIndex = Math.Min(images.Length - 1, currentImageIndex + 1);
    UpdateImage();
}

private void OnResetClicked(object sender, EventArgs e)
{
    viewer.Reset();
}

private void UpdateImage()
{
    viewerImage.Source = ImageSource.FromFile(images[currentImageIndex]);
    imageInfo.Text = $"Image {currentImageIndex + 1} of {images.Length}";
}
```

## Complete Examples

### Example 1: Photo Viewer Application

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:interactiveViewer="clr-namespace:Syncfusion.Maui.InteractiveViewer;assembly=Syncfusion.Maui.InteractiveViewer"
             x:Class="PhotoViewer.MainPage"
             Title="Photo Viewer">
    
    <Grid RowDefinitions="Auto,*,Auto" Padding="10">
        <!-- Title -->
        <Label Text="View your photos with full control" 
               FontSize="16" 
               FontAttributes="Bold"
               Margin="0,0,0,10" />
        
        <!-- Viewer -->
        <interactiveViewer:SfInteractiveViewer 
            Grid.Row="1"
            x:Name="viewer"
            IsZoomEnabled="True"
            MinimumZoomFactor="0.5"
            MaximumZoomFactor="8">
            <Image x:Name="photoImage" Aspect="AspectFit" />
        </interactiveViewer:SfInteractiveViewer>
        
        <!-- Controls -->
        <StackLayout Grid.Row="2" 
                     Orientation="Horizontal" 
                     Spacing="10"
                     Margin="0,10,0,0">
            <Button Text="⟲ Rotate" 
                    Clicked="OnRotateClicked" 
                    HorizontalOptions="FillAndExpand" />
            <Button Text="↻ Reset" 
                    Clicked="OnResetClicked" 
                    HorizontalOptions="FillAndExpand" />
        </StackLayout>
    </Grid>
</ContentPage>
```

```c#
using Syncfusion.Maui.InteractiveViewer;

namespace PhotoViewer;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        LoadPhoto("photo.png");
    }

    private void LoadPhoto(string photoPath)
    {
        viewer.Reset();
        photoImage.Source = ImageSource.FromFile(photoPath);
    }

    private void OnRotateClicked(object sender, EventArgs e)
    {
        viewer.Rotate();
    }

    private void OnResetClicked(object sender, EventArgs e)
    {
        viewer.Reset();
    }
}
```

### Example 2: Document Review with History Reset

```c#
public class DocumentReviewPage : ContentPage
{
    private SfInteractiveViewer viewer;
    private Stack<(double zoom, int rotation)> viewingHistory;

    public DocumentReviewPage()
    {
        InitializeComponent();
        viewingHistory = new Stack<(double, int)>();
    }

    private void OnReviewDocumentClicked(string documentPath)
    {
        // Reset before loading new document
        viewer.Reset();
        viewingHistory.Clear();
        
        // Load document
        viewer.Content = new Image 
        { 
            Source = documentPath, 
            Aspect = Aspect.AspectFit 
        };
    }

    private void OnCompleteReviewClicked()
    {
        // Reset to clean state before closing
        viewer.Reset();
    }
}
```
