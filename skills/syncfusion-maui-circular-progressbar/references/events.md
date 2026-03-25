# Events in Circular ProgressBar

The Syncfusion .NET MAUI Circular ProgressBar provides two key events for responding to progress changes: `ProgressChanged` and `ProgressCompleted`. Use these events to implement dynamic behavior, update UI elements, or trigger actions based on progress values.

## Overview

**Available Events:**
1. **ProgressChanged**: Triggered when progress value changes
2. **ProgressCompleted**: Triggered when progress reaches maximum value

## ProgressChanged Event

Fired whenever the `Progress` property value changes, allowing you to respond to incremental progress updates.

### Event Arguments

**Type**: `ProgressValueEventArgs`

**Properties:**
- `Progress` (double): The current progress value

### Basic Usage

**XAML:**
```xml
<progressBar:SfCircularProgressBar x:Name="circularProgressBar" 
                                   ProgressChanged="CircularProgressBar_ProgressChanged"
                                   Progress="100" />
```

**C#:**
```csharp
private void CircularProgressBar_ProgressChanged(object sender, ProgressValueEventArgs e)
{
    // Access current progress value
    double currentProgress = e.Progress;
    
    // Update UI or perform actions
    Debug.WriteLine($"Progress changed to: {currentProgress}%");
}
```

### Subscribing in Code

```csharp
SfCircularProgressBar circularProgressBar = new SfCircularProgressBar();
circularProgressBar.ProgressChanged += CircularProgressBar_ProgressChanged;
```

## Dynamic Color Changes

Change progress color based on current value to provide visual feedback.

### Color Ranges Based on Progress

```csharp

public class DynamicColorCircularPage : ContentPage
{
    private SfCircularProgressBar circularProgressBar;
    private Label statusLabel;

    public DynamicColorCircularPage()
    {
        circularProgressBar = new SfCircularProgressBar
        {
            Progress = 0,
            TrackThickness = 10,
            ProgressThickness = 10,
            WidthRequest = 150,
            HeightRequest = 150,
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center,
            ProgressFill = new SolidColorBrush(Colors.Red),
            TrackFill = new SolidColorBrush(Colors.LightGray)
        };

        // Subscribe to the ProgressChanged event
        circularProgressBar.ProgressChanged += CircularProgressBar_ProgressChanged;

        statusLabel = new Label
        {
            Text = "Progress: 0%",
            HorizontalOptions = LayoutOptions.Center,
            Margin = new Thickness(0, 20, 0, 0)
        };

        var increaseButton = new Button
        {
            Text = "Increase Progress",
            HorizontalOptions = LayoutOptions.Center
        };
        increaseButton.Clicked += (s, e) =>
        {
            // Increase progress by 10% each click
            int newProgress = (int)Math.Min(100, circularProgressBar.Progress + 10);
            circularProgressBar.SetProgress(newProgress, 800, Easing.CubicInOut);
            statusLabel.Text = $"Progress: {newProgress}%";
        };

        Content = new StackLayout
        {
            Padding = 40,
            Spacing = 20,
            Children = { circularProgressBar, statusLabel, increaseButton }
        };
    }

    private void CircularProgressBar_ProgressChanged(object sender, ProgressValueEventArgs e)
    {
        if (e.Progress < 50)
        {
            circularProgressBar.ProgressFill = new SolidColorBrush(Colors.Red);
        }
        else if (e.Progress < 80)
        {
            circularProgressBar.ProgressFill = new SolidColorBrush(Colors.Orange);
        }
        else
        {
            circularProgressBar.ProgressFill = new SolidColorBrush(Colors.Green);
        }
    }
}

```

### Three-Tier Status Colors

```xml
<progressBar:SfCircularProgressBar x:Name="statusProgressBar"
                                   ProgressChanged="StatusProgressBar_ProgressChanged"
                                   Progress="65" />
```

```csharp
private void StatusProgressBar_ProgressChanged(object sender, ProgressValueEventArgs e)
{
    // Low (0-33): Red - Critical
    // Medium (34-66): Yellow - Warning
    // High (67-100): Green - Good
    
    if (e.Progress <= 33)
    {
        statusProgressBar.ProgressFill = new SolidColorBrush(Color.FromArgb("#FF4444"));
        statusLabel.Text = "Critical";
        statusLabel.TextColor = Color.FromArgb("#FF4444");
    }
    else if (e.Progress <= 66)
    {
        statusProgressBar.ProgressFill = new SolidColorBrush(Color.FromArgb("#FFBB33"));
        statusLabel.Text = "Warning";
        statusLabel.TextColor = Color.FromArgb("#FFBB33");
    }
    else
    {
        statusProgressBar.ProgressFill = new SolidColorBrush(Color.FromArgb("#00C851"));
        statusLabel.Text = "Good";
        statusLabel.TextColor = Color.FromArgb("#00C851");
    }
}
```

## Updating UI Elements

### Update Multiple Labels

```xml
<StackLayout Padding="20" Spacing="20">
    <progressBar:SfCircularProgressBar x:Name="uploadProgressBar"
                                       ProgressChanged="UploadProgressBar_ProgressChanged"
                                       Progress="0" />
    
    <Label x:Name="percentLabel" 
           Text="0%"
           FontSize="20"
           HorizontalTextAlignment="Center" />
    
    <Label x:Name="statusLabel" 
           Text="Ready"
           HorizontalTextAlignment="Center" />
    
    <progressBar:SfLinearProgressBar x:Name="linearProgressBar" 
                 Progress="0" />
</StackLayout>
```

```csharp
private void UploadProgressBar_ProgressChanged(object sender, ProgressValueEventArgs e)
{
    // Update percentage label
    percentLabel.Text = $"{e.Progress:F0}%";
    
    // Update status message
    if (e.Progress < 25)
        statusLabel.Text = "Starting...";
    else if (e.Progress < 50)
        statusLabel.Text = "Processing...";
    else if (e.Progress < 75)
        statusLabel.Text = "Almost there...";
    else if (e.Progress < 100)
        statusLabel.Text = "Finalizing...";
    
    // Sync with linear progress bar
    linearProgressBar.Progress = e.Progress / 100.0;
}
```

## ProgressCompleted Event

Fired when the `Progress` value reaches the `Maximum` value, indicating task completion.

### Event Arguments

**Type**: `ProgressValueEventArgs`

**Properties:**
- `Progress` (double): The maximum progress value

### Basic Usage

**XAML:**
```xml
<progressBar:SfCircularProgressBar Minimum="100" 
                                   Maximum="500" 
                                   ProgressCompleted="CircularProgressBar_ProgressCompleted" 
                                   Progress="500">
    <progressBar:SfCircularProgressBar.Content>
        <Grid WidthRequest="150">
            <Label x:Name="completionLabel" 
                   Text="Start" 
                   FontSize="15"
                   HorizontalTextAlignment="Center" 
                   VerticalTextAlignment="Center" />
        </Grid>
    </progressBar:SfCircularProgressBar.Content>
</progressBar:SfCircularProgressBar>
```

**C#:**
```csharp
private void CircularProgressBar_ProgressCompleted(object sender, ProgressValueEventArgs e)
{
    completionLabel.Text = "Completed";
}
```

### Subscribing in Code

```csharp
SfCircularProgressBar circularProgressBar = new SfCircularProgressBar();
circularProgressBar.ProgressCompleted += CircularProgressBar_ProgressCompleted;
```

## Completion Actions

### Show Alert on Completion

```csharp
private async void CircularProgressBar_ProgressCompleted(object sender, ProgressValueEventArgs e)
{
    await DisplayAlert("Success", "Task completed successfully!", "OK");
}
```

### Navigate to Next Page

```csharp
private async void CircularProgressBar_ProgressCompleted(object sender, ProgressValueEventArgs e)
{
    await Navigation.PushAsync(new CompletionPage());
}
```

### Reset and Restart

```csharp
using Microsoft.Maui.Controls;
using Syncfusion.Maui.ProgressBar;
using System.Threading.Tasks;

public class ProgressCompletedPage : ContentPage
{
    private SfCircularProgressBar circularProgressBar;
    private Label statusLabel;

    public ProgressCompletedPage()
    {
        circularProgressBar = new SfCircularProgressBar
        {
            Progress = 0,
            TrackThickness = 10,
            ProgressThickness = 10,
            WidthRequest = 150,
            HeightRequest = 150,
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center,
            ProgressFill = new SolidColorBrush(Colors.Green),
            TrackFill = new SolidColorBrush(Colors.LightGray)
        };

        // Subscribe to ProgressCompleted event
        circularProgressBar.ProgressCompleted += CircularProgressBar_ProgressCompleted;

        statusLabel = new Label
        {
            Text = "Ready to start",
            HorizontalOptions = LayoutOptions.Center,
            Margin = new Thickness(0, 20, 0, 0)
        };

        var startButton = new Button
        {
            Text = "Run Task",
            HorizontalOptions = LayoutOptions.Center
        };
        startButton.Clicked += async (s, e) =>
        {
            statusLabel.Text = "Working...";
            // Animate to 100% to trigger ProgressCompleted
            circularProgressBar.SetProgress(100, 2000, Easing.CubicInOut);
        };

        Content = new StackLayout
        {
            Padding = 40,
            Spacing = 20,
            Children = { circularProgressBar, statusLabel, startButton }
        };
    }

    private async void CircularProgressBar_ProgressCompleted(object sender, ProgressValueEventArgs e)
    {
        await DisplayAlert("Complete", "Task finished!", "OK");

        // Wait and reset
        await Task.Delay(2000);
        circularProgressBar.Progress = 0;
        statusLabel.Text = "Ready to start again";
    }
}
```

## Complete Examples

### Example 1: File Upload with Status Updates

```xml
<StackLayout Padding="20" Spacing="20">
    <progressBar:SfCircularProgressBar x:Name="fileUploadProgress"
                                       ProgressChanged="FileUploadProgress_ProgressChanged"
                                       ProgressCompleted="FileUploadProgress_ProgressCompleted"
                                       Progress="0"
                                       ProgressFill="#FF2196F3"
                                       TrackFill="#332196F3">
        <progressBar:SfCircularProgressBar.Content>
            <StackLayout Spacing="8">
                <Label x:Name="uploadPercentLabel"
                       Text="0%"
                       FontSize="24"
                       FontAttributes="Bold"
                       HorizontalTextAlignment="Center" />
                <Label x:Name="uploadStatusLabel"
                       Text="Ready"
                       FontSize="12"
                       TextColor="Gray"
                       HorizontalTextAlignment="Center" />
            </StackLayout>
        </progressBar:SfCircularProgressBar.Content>
    </progressBar:SfCircularProgressBar>
    
    <Button Text="Upload File" 
            Clicked="UploadFile_Clicked" />
</StackLayout>
```

```csharp
private void FileUploadProgress_ProgressChanged(object sender, ProgressValueEventArgs e)
{
    uploadPercentLabel.Text = $"{e.Progress:F0}%";
    
    if (e.Progress < 25)
        uploadStatusLabel.Text = "Uploading...";
    else if (e.Progress < 75)
        uploadStatusLabel.Text = "Processing...";
    else if (e.Progress < 100)
        uploadStatusLabel.Text = "Finalizing...";
}

private async void FileUploadProgress_ProgressCompleted(object sender, ProgressValueEventArgs e)
{
    uploadStatusLabel.Text = "Complete!";
    uploadStatusLabel.TextColor = Colors.Green;
    
    await DisplayAlert("Success", "File uploaded successfully!", "OK");
    
    // Reset after 2 seconds
    await Task.Delay(2000);
    fileUploadProgress.Progress = 0;
    uploadStatusLabel.Text = "Ready";
    uploadStatusLabel.TextColor = Colors.Gray;
}

private async void UploadFile_Clicked(object sender, EventArgs e)
{
    fileUploadProgress.Progress = 0;
    
    // Simulate upload
    for (int i = 0; i <= 100; i += 5)
    {
        fileUploadProgress.Progress = i;
        await Task.Delay(200);
    }
}
```

### Example 2: Health Monitoring with Dynamic Alerts

```csharp
public partial class HealthMonitorPage : ContentPage
{
    private SfCircularProgressBar healthProgressBar;
    private Label healthStatusLabel;
    private Image celebrationAnimation;

    public HealthMonitorPage()
    {
        InitializeComponent();

        healthProgressBar = new SfCircularProgressBar
        {
            Progress = 0,
            TrackThickness = 10,
            ProgressThickness = 10,
            WidthRequest = 150,
            HeightRequest = 150,
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center,
            ProgressFill = new SolidColorBrush(Colors.Green),
            TrackFill = new SolidColorBrush(Colors.LightGray)
        };

        healthStatusLabel = new Label
        {
            Text = "Ready",
            HorizontalOptions = LayoutOptions.Center,
            Margin = new Thickness(0, 20, 0, 0),
            FontSize = 18,
            TextColor = Colors.Green
        };

        // Hidden celebratory image (could be any resource in your project)
        celebrationAnimation = new Image
        {
            Source = "celebration.png", // Add an image to your project resources
            Opacity = 0,
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center,
            WidthRequest = 100,
            HeightRequest = 100
        };

        var increaseButton = new Button
        {
            Text = "Increase Health",
            HorizontalOptions = LayoutOptions.Center
        };
        increaseButton.Clicked += (s, e) =>
        {
            int newProgress = (int)Math.Min(100, healthProgressBar.Progress + 20);
            healthProgressBar.SetProgress(newProgress, 1000, Easing.CubicInOut);
        };

        // Subscribe to events
        healthProgressBar.ProgressChanged += HealthProgressBar_ProgressChanged;
        healthProgressBar.ProgressCompleted += HealthProgressBar_ProgressCompleted;

        Content = new StackLayout
        {
            Padding = 40,
            Spacing = 20,
            Children = { healthProgressBar, healthStatusLabel, increaseButton, celebrationAnimation }
        };
    }

    private void HealthProgressBar_ProgressChanged(object sender, ProgressValueEventArgs e)
    {
        if (e.Progress < 20)
        {
            healthProgressBar.ProgressFill = new SolidColorBrush(Colors.Red);
            healthStatusLabel.Text = "Critical";
            healthStatusLabel.TextColor = Colors.Red;
            DisplayAlert("Warning", "Health level critical!", "OK");
        }
        else if (e.Progress < 50)
        {
            healthProgressBar.ProgressFill = new SolidColorBrush(Colors.Orange);
            healthStatusLabel.Text = "Low";
            healthStatusLabel.TextColor = Colors.Orange;
        }
        else if (e.Progress < 80)
        {
            healthProgressBar.ProgressFill = new SolidColorBrush(Colors.Yellow);
            healthStatusLabel.Text = "Fair";
            healthStatusLabel.TextColor = Colors.Yellow;
        }
        else
        {
            healthProgressBar.ProgressFill = new SolidColorBrush(Colors.Green);
            healthStatusLabel.Text = "Excellent";
            healthStatusLabel.TextColor = Colors.Green;
        }
    }

    private async void HealthProgressBar_ProgressCompleted(object sender, ProgressValueEventArgs e)
    {
        await celebrationAnimation.FadeTo(1, 500);
        await DisplayAlert("Achievement", "Perfect health achieved!", "Awesome");
        await Task.Delay(2000);
        celebrationAnimation.Opacity = 0;
        healthProgressBar.Progress = 0;
        healthStatusLabel.Text = "Ready to start again";
        healthStatusLabel.TextColor = Colors.Green;
    }
}
```

## Best Practices

### Event Handling
1. **Keep handlers lightweight**: Avoid heavy processing in event handlers
2. **Use async/await**: For operations like alerts or navigation
3. **Unsubscribe when needed**: Prevent memory leaks by removing event handlers
4. **Handle exceptions**: Wrap event code in try-catch blocks

### Performance
1. **Throttle updates**: Avoid excessive UI updates in ProgressChanged
2. **Batch operations**: Group multiple updates together
3. **Optimize calculations**: Cache computed values when possible

### User Experience
1. **Provide feedback**: Use events to inform users of progress
2. **Visual indicators**: Change colors based on progress ranges
3. **Celebrate milestones**: Acknowledge 25%, 50%, 75%, 100%
4. **Graceful completion**: Provide clear completion feedback

### Debugging
1. **Log events**: Use Debug.WriteLine for troubleshooting
2. **Track progress**: Monitor event firing frequency
3. **Validate values**: Check progress values are within expected range

## Summary

Events enable dynamic, responsive progress bars:
- **ProgressChanged**: Fired on every progress update
  - Use for: Color changes, UI updates, logging, milestone actions
- **ProgressCompleted**: Fired when progress reaches maximum
  - Use for: Completion alerts, navigation, celebrations, reset logic

Combine events with other features (colors, animations, custom content) to create engaging, informative progress indicators that enhance user experience.