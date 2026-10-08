# Zooming and Panning in Interactive Viewer

## Table of Contents
- [Enable/Disable Zoom](#enabledisable-zoom)
- [Setting Initial Zoom](#setting-initial-zoom)
- [Zoom Limits](#zoom-limits)
- [Panning Behavior](#panning-behavior)
- [Zoom Best Practices](#zoom-best-practices)
- [Complete Examples](#complete-examples)

## Enable/Disable Zoom

The `IsZoomEnabled` property controls whether users can zoom content. By default, zooming is enabled.

### XAML
```xaml
<!-- Zoom enabled (default) -->
<interactiveViewer:SfInteractiveViewer IsZoomEnabled="True">
    <Image Source="image.png" Aspect="AspectFit" />
</interactiveViewer:SfInteractiveViewer>

<!-- Zoom disabled -->
<interactiveViewer:SfInteractiveViewer IsZoomEnabled="False">
    <Image Source="image.png" Aspect="AspectFit" />
</interactiveViewer:SfInteractiveViewer>
```

### C#
```c#
using Syncfusion.Maui.InteractiveViewer;

// Enable zoom
viewer.IsZoomEnabled = true;

// Disable zoom (prevents user interaction, but programmatic zoom still works)
viewer.IsZoomEnabled = false;
```

## Setting Initial Zoom

The `ZoomFactor` property controls the zoom level. A value of 1.0 represents 100% (original size).

### XAML
```xaml
<!-- Start at 2x zoom -->
<interactiveViewer:SfInteractiveViewer ZoomFactor="2">
    <Image Source="image.png" Aspect="AspectFit" />
</interactiveViewer:SfInteractiveViewer>

<!-- Start at 50% zoom -->
<interactiveViewer:SfInteractiveViewer ZoomFactor="0.5">
    <Image Source="image.png" Aspect="AspectFit" />
</interactiveViewer:SfInteractiveViewer>
```

### C#
```c#
// Set zoom to 3x
viewer.ZoomFactor = 3;

// Set zoom to fit in view initially
viewer.ZoomFactor = 1.0;  // Original size

// Zoom to detailed inspection level
viewer.ZoomFactor = 4.5;
```

## Zoom Limits

Control minimum and maximum zoom levels to provide a bounded navigation experience.

### Minimum Zoom Factor

The `MinimumZoomFactor` property defines the smallest zoom level users can reach.

```xaml
<interactiveViewer:SfInteractiveViewer MinimumZoomFactor="0.5">
    <Image Source="image.png" Aspect="AspectFit" />
</interactiveViewer:SfInteractiveViewer>
```

```c#
// Users cannot zoom out below 50%
viewer.MinimumZoomFactor = 0.5;
```

### Maximum Zoom Factor

The `MaximumZoomFactor` property defines the largest zoom level users can reach.

```xaml
<interactiveViewer:SfInteractiveViewer MaximumZoomFactor="10">
    <Image Source="image.png" Aspect="AspectFit" />
</interactiveViewer:SfInteractiveViewer>
```

```c#
// Users cannot zoom in beyond 10x
viewer.MaximumZoomFactor = 10;
```

### Combined Zoom Constraints

```xaml
<interactiveViewer:SfInteractiveViewer 
    IsZoomEnabled="True"
    MinimumZoomFactor="0.5"
    MaximumZoomFactor="10"
    ZoomFactor="1.0">
    <Image Source="image.png" Aspect="AspectFit" />
</interactiveViewer:SfInteractiveViewer>
```

```c#
// Configure zoom bounds
viewer.MinimumZoomFactor = 0.5;  // 50% minimum
viewer.MaximumZoomFactor = 10;   // 10x maximum
viewer.ZoomFactor = 2;            // Start at 2x
```

## Panning Behavior

Panning (scrolling) is automatically enabled when content exceeds the viewer boundaries. Users can drag content to navigate.

### Pan Properties in Events

The `ScrollChanged` event provides pan axis information:

```c#
<interactiveViewer:SfInteractiveViewer ScrollChanged="OnScrollChanged">
    <Image Source="image.png" Aspect="AspectFit" />
</interactiveViewer:SfInteractiveViewer>
```

```c#
private void OnScrollChanged(object sender, InteractiveScrollChangedEventArgs e)
{
    // e.PanAxis shows current pan directions (Horizontal, Vertical, Both)
    PanAxis currentPanAxis = e.PanAxis;
    double currentZoom = e.ZoomFactor;
    
    Debug.WriteLine($"Pan Axis: {currentPanAxis}, Zoom: {currentZoom}");
}
```

## Zoom Best Practices

### Practice 1: Set Realistic Zoom Limits
```c#
// For detailed inspection (medical, technical drawings)
viewer.MinimumZoomFactor = 0.5;   // Allow zooming out to see context
viewer.MaximumZoomFactor = 8;     // Allow detailed inspection

// For general image viewing
viewer.MinimumZoomFactor = 0.75;
viewer.MaximumZoomFactor = 5;
```

### Practice 2: Match Zoom to Content Type
```c#
// High-resolution technical document
viewer.MaximumZoomFactor = 15;
viewer.MinimumZoomFactor = 0.25;

// Product photography
viewer.MaximumZoomFactor = 6;
viewer.MinimumZoomFactor = 0.5;
```

### Practice 3: Programmatic Zoom for Specific Content
```c#
// Zoom to area of interest after content loads
public void DisplayAndZoom(string imagePath, double initialZoom)
{
    viewer.Content = new Image { Source = imagePath, Aspect = Aspect.AspectFit };
    viewer.ZoomFactor = initialZoom;
}
```

## Complete Examples

### Example 1: Document Viewer with Zoom Controls

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:interactiveViewer="clr-namespace:Syncfusion.Maui.InteractiveViewer;assembly=Syncfusion.Maui.InteractiveViewer"
             x:Class="DocumentViewer.MainPage"
             Title="Document Viewer">
    
    <Grid RowDefinitions="Auto,*,Auto" Padding="10">
        <!-- Zoom Display -->
        <Label Grid.Row="0" 
               x:Name="zoomLabel" 
               Text="Zoom: 100%"
               FontSize="14" 
               Margin="0,0,0,10" />
        
        <!-- Viewer -->
        <interactiveViewer:SfInteractiveViewer 
            Grid.Row="1"
            x:Name="viewer"
            IsZoomEnabled="True"
            MinimumZoomFactor="0.5"
            MaximumZoomFactor="8"
            ZoomFactor="1.0"
            ZoomFactorChanged="OnZoomChanged">
            <Image Source="document.png" Aspect="AspectFit" />
        </interactiveViewer:SfInteractiveViewer>
        
        <!-- Zoom Controls -->
        <StackLayout Grid.Row="2" Orientation="Horizontal" Spacing="10" Margin="0,10,0,0">
            <Button Text="Zoom In" Clicked="OnZoomIn" HorizontalOptions="FillAndExpand" />
            <Button Text="Zoom Out" Clicked="OnZoomOut" HorizontalOptions="FillAndExpand" />
            <Button Text="Reset" Clicked="OnReset" HorizontalOptions="FillAndExpand" />
        </StackLayout>
    </Grid>
</ContentPage>
```

```c#
using Syncfusion.Maui.InteractiveViewer;

namespace DocumentViewer;

public partial class MainPage : ContentPage
{
    private const double ZOOM_STEP = 0.5;

    public MainPage()
    {
        InitializeComponent();
    }

    private void OnZoomIn(object sender, EventArgs e)
    {
        viewer.ZoomFactor = Math.Min(
            viewer.ZoomFactor + ZOOM_STEP,
            viewer.MaximumZoomFactor
        );
    }

    private void OnZoomOut(object sender, EventArgs e)
    {
        viewer.ZoomFactor = Math.Max(
            viewer.ZoomFactor - ZOOM_STEP,
            viewer.MinimumZoomFactor
        );
    }

    private void OnReset(object sender, EventArgs e)
    {
        viewer.Reset();
    }

    private void OnZoomChanged(object sender, ZoomFactorChangedEventArgs e)
    {
        int zoomPercent = (int)(e.NewZoomFactor * 100);
        zoomLabel.Text = $"Zoom: {zoomPercent}%";
    }
}
```

### Example 2: Product Image Viewer

```c#
// High-quality product images support significant zoom
viewer.MinimumZoomFactor = 0.75;   // See full product
viewer.MaximumZoomFactor = 6;      // Inspect details
viewer.ZoomFactor = 1.0;           // Start at normal size

// Allow user to explore details without losing context
```
