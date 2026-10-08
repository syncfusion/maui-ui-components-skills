# Events and Interactions in Interactive Viewer

## Table of Contents
- [Overview](#overview)
- [ZoomFactorChanged Event](#zoomfactorchanged-event)
- [ScrollChanged Event](#scrollchanged-event)
- [Event Handling Patterns](#event-handling-patterns)
- [Real-World Examples](#real-world-examples)

## Overview

The Interactive Viewer supports two key events for tracking user interactions:

| Event | Triggers | Use Case |
|-------|----------|----------|
| `ZoomFactorChanged` | After zoom level changes | Track zoom level, update UI indicators |
| `ScrollChanged` | When pan position changes | Track navigation, enable/disable controls |

## ZoomFactorChanged Event

### Event Arguments

The `ZoomFactorChangedEventArgs` provides zoom information:

```c#
public class ZoomFactorChangedEventArgs : EventArgs
{
    public double OldZoomFactor { get; set; }  // Previous zoom level
    public double NewZoomFactor { get; set; }  // New zoom level
}
```

### Basic Implementation

#### XAML Setup
```xaml
<interactiveViewer:SfInteractiveViewer x:Name="viewer"
                                       ZoomFactorChanged="OnZoomFactorChanged">
    <Image Source="image.png" Aspect="AspectFit" />
</interactiveViewer:SfInteractiveViewer>
```

#### Code-Behind Handler
```c#
using Syncfusion.Maui.InteractiveViewer;

private void OnZoomFactorChanged(object sender, ZoomFactorChangedEventArgs e)
{
    double oldZoom = e.OldZoomFactor;
    double newZoom = e.NewZoomFactor;
    
    Debug.WriteLine($"Zoom changed: {oldZoom} → {newZoom}");
}
```

### Real-Time Zoom Display

Display current zoom percentage to the user:

```xaml
<Grid RowDefinitions="Auto,*" Padding="10">
    <!-- Zoom Display -->
    <Label x:Name="zoomLabel" 
           Text="Zoom: 100%"
           FontSize="14"
           HorizontalTextAlignment="Center"
           Margin="0,0,0,10" />
    
    <!-- Viewer -->
    <interactiveViewer:SfInteractiveViewer 
        Grid.Row="1"
        x:Name="viewer"
        ZoomFactorChanged="OnZoomFactorChanged">
        <Image Source="image.png" Aspect="AspectFit" />
    </interactiveViewer:SfInteractiveViewer>
</Grid>
```

```c#
private void OnZoomFactorChanged(object sender, ZoomFactorChangedEventArgs e)
{
    int zoomPercent = (int)(e.NewZoomFactor * 100);
    zoomLabel.Text = $"Zoom: {zoomPercent}%";
}
```

### Zoom Event with Limits Checking

```c#
private void OnZoomFactorChanged(object sender, ZoomFactorChangedEventArgs e)
{
    double newZoom = e.NewZoomFactor;
    
    // Detect zoom limits reached
    if (newZoom >= viewer.MaximumZoomFactor)
    {
        Debug.WriteLine("Maximum zoom reached");
    }
    else if (newZoom <= viewer.MinimumZoomFactor)
    {
        Debug.WriteLine("Minimum zoom reached");
    }
    
    // Update UI
    UpdateZoomUI(newZoom);
}

private void UpdateZoomUI(double zoomFactor)
{
    int zoomPercent = (int)(zoomFactor * 100);
    zoomPercentLabel.Text = $"{zoomPercent}%";
}
```

## ScrollChanged Event

### Event Arguments

The `InteractiveScrollChangedEventArgs` provides scroll/pan information:

```c#
public class InteractiveScrollChangedEventArgs : EventArgs
{
    public PanAxis PanAxis { get; set; }       // Direction(s) panning is allowed
    public double ZoomFactor { get; set; }     // Current zoom level
}

// PanAxis enum values
public enum PanAxis
{
    Horizontal,   // Can pan left/right
    Vertical,     // Can pan up/down
    Both,         // Can pan in all directions
    None          // No panning allowed
}
```

### Basic Implementation

#### XAML Setup
```xaml
<interactiveViewer:SfInteractiveViewer x:Name="viewer"
                                       ScrollChanged="OnScrollChanged">
    <Image Source="image.png" Aspect="AspectFit" />
</interactiveViewer:SfInteractiveViewer>
```

#### Code-Behind Handler
```c#
using Syncfusion.Maui.InteractiveViewer;

private void OnScrollChanged(object sender, InteractiveScrollChangedEventArgs e)
{
    PanAxis panAxis = e.PanAxis;
    double zoomFactor = e.ZoomFactor;
    
    Debug.WriteLine($"Pan Axis: {panAxis}, Zoom: {zoomFactor}");
}
```

### Detecting Pan Directions

```c#
private void OnScrollChanged(object sender, InteractiveScrollChangedEventArgs e)
{
    switch (e.PanAxis)
    {
        case PanAxis.Horizontal:
            Debug.WriteLine("Panning horizontally (left/right)");
            break;
        
        case PanAxis.Vertical:
            Debug.WriteLine("Panning vertically (up/down)");
            break;
        
        case PanAxis.Both:
            Debug.WriteLine("Panning in both directions");
            break;
        
        case PanAxis.None:
            Debug.WriteLine("No panning possible (content fits in view)");
            break;
    }
}
```

## Event Handling Patterns

### Pattern 1: Dual Event Tracking

```xaml
<Grid RowDefinitions="Auto,*,Auto" Padding="10">
    <!-- Status Display -->
    <StackLayout Spacing="5" Margin="0,0,0,10">
        <Label x:Name="zoomLabel" Text="Zoom: 100%" FontSize="12" />
        <Label x:Name="panLabel" Text="Pan Axis: None" FontSize="12" />
    </StackLayout>
    
    <!-- Viewer with Both Events -->
    <interactiveViewer:SfInteractiveViewer 
        Grid.Row="1"
        x:Name="viewer"
        ZoomFactorChanged="OnZoomFactorChanged"
        ScrollChanged="OnScrollChanged">
        <Image Source="image.png" Aspect="AspectFit" />
    </interactiveViewer:SfInteractiveViewer>
</Grid>
```

```c#
private void OnZoomFactorChanged(object sender, ZoomFactorChangedEventArgs e)
{
    int zoomPercent = (int)(e.NewZoomFactor * 100);
    zoomLabel.Text = $"Zoom: {zoomPercent}%";
}

private void OnScrollChanged(object sender, InteractiveScrollChangedEventArgs e)
{
    panLabel.Text = $"Pan Axis: {e.PanAxis}";
}
```

### Pattern 2: Conditional UI Updates

```c#
private void OnZoomFactorChanged(object sender, ZoomFactorChangedEventArgs e)
{
    // Only enable zoom in button if not at max
    zoomInButton.IsEnabled = e.NewZoomFactor < viewer.MaximumZoomFactor;
    
    // Only enable zoom out button if not at min
    zoomOutButton.IsEnabled = e.NewZoomFactor > viewer.MinimumZoomFactor;
    
    // Update display
    int zoomPercent = (int)(e.NewZoomFactor * 100);
    zoomLabel.Text = $"{zoomPercent}%";
}
```

### Pattern 3: Event-Based Logging

```c#
private class InteractionLogger
{
    private DateTime lastZoomTime;
    private DateTime lastPanTime;
    private List<string> eventLog = new();

    public void OnZoomFactorChanged(object sender, ZoomFactorChangedEventArgs e)
    {
        lastZoomTime = DateTime.Now;
        string logEntry = $"[{lastZoomTime:HH:mm:ss}] Zoom: {e.OldZoomFactor} → {e.NewZoomFactor}";
        eventLog.Add(logEntry);
        Debug.WriteLine(logEntry);
    }

    public void OnScrollChanged(object sender, InteractiveScrollChangedEventArgs e)
    {
        lastPanTime = DateTime.Now;
        string logEntry = $"[{lastPanTime:HH:mm:ss}] Pan: {e.PanAxis}";
        eventLog.Add(logEntry);
        Debug.WriteLine(logEntry);
    }

    public void ExportLog(string filePath)
    {
        File.WriteAllLines(filePath, eventLog);
    }
}
```

## Real-World Examples

### Example 1: Image Inspection Tool

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:interactiveViewer="clr-namespace:Syncfusion.Maui.InteractiveViewer;assembly=Syncfusion.Maui.InteractiveViewer"
             x:Class="ImageInspector.MainPage"
             Title="Image Inspector">
    
    <Grid RowDefinitions="Auto,*,Auto" Padding="10" RowSpacing="10">
        <!-- Status Bar -->
        <Grid ColumnDefinitions="*,*" Spacing="10">
            <StackLayout Spacing="3">
                <Label Text="Zoom Level:" FontSize="12" FontAttributes="Bold" />
                <Label x:Name="zoomLabel" Text="100%" FontSize="16" />
            </StackLayout>
            
            <StackLayout Spacing="3" Grid.Column="1">
                <Label Text="Pan Available:" FontSize="12" FontAttributes="Bold" />
                <Label x:Name="panLabel" Text="None" FontSize="16" />
            </StackLayout>
        </Grid>
        
        <!-- Viewer -->
        <interactiveViewer:SfInteractiveViewer 
            Grid.Row="1"
            x:Name="viewer"
            IsZoomEnabled="True"
            MinimumZoomFactor="0.5"
            MaximumZoomFactor="10"
            ZoomFactorChanged="OnZoomFactorChanged"
            ScrollChanged="OnScrollChanged">
            <Image x:Name="inspectionImage" 
                   Source="document.png" 
                   Aspect="AspectFit" />
        </interactiveViewer:SfInteractiveViewer>
        
        <!-- Control Buttons -->
        <StackLayout Grid.Row="2" 
                     Orientation="Horizontal" 
                     Spacing="10">
            <Button x:Name="zoomInButton" 
                    Text="Zoom In" 
                    Clicked="OnZoomInClicked" 
                    HorizontalOptions="FillAndExpand" />
            <Button x:Name="zoomOutButton" 
                    Text="Zoom Out" 
                    Clicked="OnZoomOutClicked" 
                    HorizontalOptions="FillAndExpand" />
            <Button Text="Reset" 
                    Clicked="OnResetClicked" 
                    HorizontalOptions="FillAndExpand" />
        </StackLayout>
    </Grid>
</ContentPage>
```

```c#
using Syncfusion.Maui.InteractiveViewer;

namespace ImageInspector;

public partial class MainPage : ContentPage
{
    private const double ZOOM_STEP = 0.5;

    public MainPage()
    {
        InitializeComponent();
    }

    private void OnZoomFactorChanged(object sender, ZoomFactorChangedEventArgs e)
    {
        // Update zoom display
        int zoomPercent = (int)(e.NewZoomFactor * 100);
        zoomLabel.Text = $"{zoomPercent}%";
        
        // Update button states
        zoomInButton.IsEnabled = e.NewZoomFactor < viewer.MaximumZoomFactor;
        zoomOutButton.IsEnabled = e.NewZoomFactor > viewer.MinimumZoomFactor;
    }

    private void OnScrollChanged(object sender, InteractiveScrollChangedEventArgs e)
    {
        // Update pan axis display
        panLabel.Text = e.PanAxis switch
        {
            PanAxis.Both => "Both directions",
            PanAxis.Horizontal => "Horizontal",
            PanAxis.Vertical => "Vertical",
            _ => "None"
        };
    }

    private void OnZoomInClicked(object sender, EventArgs e)
    {
        viewer.ZoomFactor = Math.Min(
            viewer.ZoomFactor + ZOOM_STEP,
            viewer.MaximumZoomFactor
        );
    }

    private void OnZoomOutClicked(object sender, EventArgs e)
    {
        viewer.ZoomFactor = Math.Max(
            viewer.ZoomFactor - ZOOM_STEP,
            viewer.MinimumZoomFactor
        );
    }

    private void OnResetClicked(object sender, EventArgs e)
    {
        viewer.Reset();
    }
}
```

### Example 2: Medical Imaging with Event Log

```c#
public class MedicalImagingViewModel : INotifyPropertyChanged
{
    private SfInteractiveViewer viewer;
    private ObservableCollection<string> eventLog;
    private double currentZoomFactor;

    public MedicalImagingViewModel()
    {
        eventLog = new ObservableCollection<string>();
    }

    public void OnZoomFactorChanged(object sender, ZoomFactorChangedEventArgs e)
    {
        currentZoomFactor = e.NewZoomFactor;
        
        // Log the interaction
        string logEntry = $"[{DateTime.Now:HH:mm:ss.fff}] Zoom: {e.OldZoomFactor:F2} → {e.NewZoomFactor:F2}";
        eventLog.Insert(0, logEntry);
        
        OnPropertyChanged(nameof(CurrentZoomFactor));
        OnPropertyChanged(nameof(EventLog));
    }

    public void OnScrollChanged(object sender, InteractiveScrollChangedEventArgs e)
    {
        string logEntry = $"[{DateTime.Now:HH:mm:ss.fff}] Pan: {e.PanAxis} (Zoom: {e.ZoomFactor:F2})";
        eventLog.Insert(0, logEntry);
        
        OnPropertyChanged(nameof(EventLog));
    }

    public double CurrentZoomFactor
    {
        get => currentZoomFactor;
        set => SetProperty(ref currentZoomFactor, value);
    }

    public ObservableCollection<string> EventLog => eventLog;

    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }

    public event PropertyChangedEventHandler PropertyChanged;

    private void SetProperty<T>(ref T field, T value, [CallerMemberName] string name = "")
    {
        if (!Equals(field, value))
        {
            field = value;
            OnPropertyChanged(name);
        }
    }
}
```