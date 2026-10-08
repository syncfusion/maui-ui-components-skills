# Rotation in Interactive Viewer

## Table of Contents
- [Rotate Method](#rotate-method)
- [Rotation Behavior](#rotation-behavior)
- [UI Integration](#ui-integration)
- [Rotation Constraints](#rotation-constraints)
- [Complete Examples](#complete-examples)

## Rotate Method

The `Rotate()` method rotates content by 90 degrees clockwise. Each invocation rotates the content another 90 degrees, cycling through four orientations (0°, 90°, 180°, 270°).

### Basic Usage

```c#
using Syncfusion.Maui.InteractiveViewer;

// Rotate content 90 degrees clockwise
viewer.Rotate();

// Call again to rotate another 90 degrees
viewer.Rotate();  // Now at 180°

// Continue cycling through orientations
viewer.Rotate();  // Now at 270°
viewer.Rotate();  // Back to 0° (original)
```

### XAML Example

```xaml
<interactiveViewer:SfInteractiveViewer x:Name="viewer">
    <Image Source="image.png" Aspect="AspectFit" />
</interactiveViewer:SfInteractiveViewer>
```

```c#
private void OnRotateClicked(object sender, EventArgs e)
{
    viewer.Rotate();
}
```

## Rotation Behavior

### Key Characteristics

1. **Fixed 90-Degree Increment** - Only 90-degree clockwise rotations are supported
2. **Cycles Through Orientations** - Rotates cycle: 0° → 90° → 180° → 270° → 0°
3. **Preserves Zoom/Pan State** - Rotation maintains current zoom and pan position
4. **No Custom Angles** - Arbitrary rotation angles are not supported

### Rotation Sequence

```
Original (0°)
    ↓ .Rotate()
90° Clockwise
    ↓ .Rotate()
180° (Upside Down)
    ↓ .Rotate()
270° (or 90° Counter-Clockwise)
    ↓ .Rotate()
Back to Original (0°)
```

## UI Integration

### Pattern 1: Simple Rotate Button

```xaml
<Grid RowDefinitions="0.9*, 0.1*">
    <interactiveViewer:SfInteractiveViewer x:Name="viewer">
        <Image Source="image.png" Aspect="AspectFit" />
    </interactiveViewer:SfInteractiveViewer>
    
    <Button Grid.Row="1" 
            Text="Rotate" 
            Clicked="OnRotateClicked" />
</Grid>
```

```c#
private void OnRotateClicked(object sender, EventArgs e)
{
    viewer.Rotate();
}
```

### Pattern 2: Rotate with Display Label

```xaml
<Grid RowDefinitions="Auto,*,Auto" Padding="10">
    <!-- Rotation Indicator -->
    <Label x:Name="rotationLabel" 
           Text="Rotation: 0°"
           FontSize="14"
           Margin="0,0,0,10" />
    
    <!-- Viewer -->
    <interactiveViewer:SfInteractiveViewer 
        Grid.Row="1"
        x:Name="viewer">
        <Image Source="image.png" Aspect="AspectFit" />
    </interactiveViewer:SfInteractiveViewer>
    
    <!-- Rotate Button -->
    <Button Grid.Row="2" 
            Text="Rotate" 
            Clicked="OnRotateClicked"
            Margin="0,10,0,0" />
</Grid>
```

```c#
public partial class MainPage : ContentPage
{
    private int rotationIndex = 0;  // 0, 1, 2, 3 for 0°, 90°, 180°, 270°
    private int[] rotationAngles = { 0, 90, 180, 270 };

    private void OnRotateClicked(object sender, EventArgs e)
    {
        viewer.Rotate();
        rotationIndex = (rotationIndex + 1) % 4;
        rotationLabel.Text = $"Rotation: {rotationAngles[rotationIndex]}°";
    }
}
```

### Pattern 3: Rotate with Reset Options

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
    viewer.Reset();  // Resets zoom, pan, and rotation
}
```

## Rotation Constraints

### What Is NOT Supported

- ❌ **Custom rotation angles** - Cannot rotate by arbitrary degrees (e.g., 45°)
- ❌ **Counter-clockwise rotation** - Only clockwise rotation is supported
- ❌ **Incremental angles** - Only 90-degree increments available

### Workaround for Custom Angles

If you need custom rotation angles, you'll need to:

1. Pre-rotate images using image processing libraries
2. Store rotated versions of images
3. Switch between pre-rotated versions in the viewer

```c#
// Example: Switch between pre-rotated images
private int currentRotationIndex = 0;
private string[] rotatedImages = 
{ 
    "image_0.png",    // Original
    "image_90.png",   // 90° rotated
    "image_180.png",  // 180° rotated
    "image_270.png"   // 270° rotated
};

private void OnRotateClicked(object sender, EventArgs e)
{
    currentRotationIndex = (currentRotationIndex + 1) % 4;
    var imageSource = ImageSource.FromFile(rotatedImages[currentRotationIndex]);
    (viewer.Content as Image).Source = imageSource;
}
```

## Complete Examples

### Example 1: Document Scanner App

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:interactiveViewer="clr-namespace:Syncfusion.Maui.InteractiveViewer;assembly=Syncfusion.Maui.InteractiveViewer"
             x:Class="DocumentScanner.MainPage"
             Title="Scan Preview">
    
    <Grid RowDefinitions="Auto,*,Auto" Padding="10">
        <!-- Status -->
        <Label Text="Rotate to correct orientation" 
               FontSize="14" 
               Margin="0,0,0,10" />
        
        <!-- Viewer -->
        <interactiveViewer:SfInteractiveViewer 
            Grid.Row="1"
            x:Name="viewer">
            <Image Source="scanned_document.png" Aspect="AspectFit" />
        </interactiveViewer:SfInteractiveViewer>
        
        <!-- Controls -->
        <StackLayout Grid.Row="2" 
                     Orientation="Horizontal" 
                     Spacing="10"
                     Margin="0,10,0,0">
            <Button Text="⟲ Rotate" 
                    Clicked="OnRotateClicked" 
                    HorizontalOptions="FillAndExpand" />
            <Button Text="✓ Accept" 
                    Clicked="OnAcceptClicked" 
                    HorizontalOptions="FillAndExpand" />
        </StackLayout>
    </Grid>
</ContentPage>
```

```c#
using Syncfusion.Maui.InteractiveViewer;

namespace DocumentScanner;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }

    private void OnRotateClicked(object sender, EventArgs e)
    {
        viewer.Rotate();
    }

    private void OnAcceptClicked(object sender, EventArgs e)
    {
        // Save the current state and proceed
        DisplayAlert("Success", "Document orientation confirmed", "OK");
    }
}
```

### Example 2: Image Editor with Rotation

```c#
public class ImageEditorViewModel : INotifyPropertyChanged
{
    private int rotationAngle = 0;

    public void RotateImage()
    {
        // Viewer internally handles the rotation
        // Update our tracking
        rotationAngle = (rotationAngle + 90) % 360;
        OnPropertyChanged(nameof(RotationAngle));
    }

    public int RotationAngle
    {
        get => rotationAngle;
        set => SetProperty(ref rotationAngle, value);
    }

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
