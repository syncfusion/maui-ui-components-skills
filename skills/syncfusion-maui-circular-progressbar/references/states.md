# States in Circular ProgressBar

The Syncfusion .NET MAUI Circular ProgressBar supports two states: determinate and indeterminate. Understanding when to use each state is crucial for providing the right user experience.

## Overview

- **Determinate State**: Default state where progress is known and measurable (0-100%)
- **Indeterminate State**: Used when progress duration or completion time is unknown

## Determinate State

The determinate state is the default mode. Use it when you can calculate or estimate the progress of a task.

### When to Use Determinate State

- File uploads/downloads with known size
- Multi-step processes with defined steps
- Tasks with calculable completion percentage
- Progress that can be measured numerically
- Any scenario where you know "X out of Y" is complete

### Basic Determinate Progress

**XAML:**
```xml
<progressBar:SfCircularProgressBar Progress="75" />
```

**C#:**
```csharp
SfCircularProgressBar circularProgressBar = new SfCircularProgressBar 
{ 
    Progress = 75 
};
```

### Updating Determinate Progress Dynamically

**Example: File Upload Simulation**

```csharp
public partial class UploadPage : ContentPage
{
    private SfCircularProgressBar uploadProgressBar;
    
    public UploadPage()
    {
        InitializeComponent();
        
        uploadProgressBar = new SfCircularProgressBar
        {
            Progress = 0,
            ProgressFill = new SolidColorBrush(Colors.Blue)
        };
        
        Content = uploadProgressBar;
        
        SimulateFileUpload();
    }
    
    private async void SimulateFileUpload()
    {
        for (int i = 0; i <= 100; i += 10)
        {
            uploadProgressBar.Progress = i;
            await Task.Delay(500); // Simulate upload time
        }
        
        await DisplayAlert("Success", "Upload complete!", "OK");
    }
}
```

## Indeterminate State

The indeterminate state shows continuous animation without a specific progress value. It indicates that work is happening, but the duration or completion percentage is unknown.

### When to Use Indeterminate State

- Initial loading when connecting to a server
- API calls with unknown response time
- Background processing without measurable progress
- Database operations with unpredictable duration
- Any task where you can't calculate percentage completion

### Basic Indeterminate Progress

**XAML:**
```xml
<progressBar:SfCircularProgressBar IsIndeterminate="True" />
```

**C#:**
```csharp
SfCircularProgressBar circularProgressBar = new SfCircularProgressBar 
{ 
    IsIndeterminate = true 
};
```

## Visual Differences

### Determinate State Appearance
- Shows progress value from 0 to max
- Progress indicator stops at current value
- Static until progress value changes

### Indeterminate State Appearance
- Continuous circular animation
- No specific progress value shown
- Animated loop until state changes

## Best Practices

### When to Use Determinate
- **Known duration**: Use when you can estimate completion time
- **Measurable progress**: Use when you can calculate percentage
- **User feedback**: Provides specific information about remaining time
- **Multi-step tasks**: Shows clear progress through defined steps

### When to Use Indeterminate
- **Unknown duration**: Use for operations with unpredictable time
- **Initial loading**: Use while establishing connections
- **Background tasks**: Use for tasks without measurable increments
- **Better than nothing**: Use when you can't show specific progress but want to indicate activity

### State Transition Tips
1. **Start indeterminate** if initial connection time is unknown
2. **Switch to determinate** once you know the total work
3. **Reset properly** after completion (set Progress to 0, IsIndeterminate to false)
4. **Provide feedback** with status labels alongside progress indicators
5. **Handle errors** by resetting state in finally blocks

## Complete Example: Real-World Scenario

```xml
<StackLayout Padding="20" Spacing="20">
    <Label x:Name="statusLabel" 
           Text="Ready to start" 
           FontSize="16" 
           HorizontalTextAlignment="Center" />
    
    <progressBar:SfCircularProgressBar x:Name="progressBar" 
                                       Progress="0"
                                       HeightRequest="150"
                                       WidthRequest="150" />
    
    <Button Text="Process Data" 
            Clicked="ProcessButton_Clicked" />
</StackLayout>
```

```csharp
private async void ProcessButton_Clicked(object sender, EventArgs e)
{
    try
    {
        // Step 1: Indeterminate - Initializing
        progressBar.IsIndeterminate = true;
        statusLabel.Text = "Initializing...";
        await Task.Delay(2000);
        
        // Step 2: Determinate - Processing
        progressBar.IsIndeterminate = false;
        progressBar.Progress = 0;
        
        for (int i = 1; i <= 5; i++)
        {
            statusLabel.Text = $"Processing step {i} of 5...";
            progressBar.Progress = (i / 5.0) * 100;
            await Task.Delay(1000);
        }
        
        // Step 3: Complete
        statusLabel.Text = "Processing complete!";
        await Task.Delay(1000);
        
        // Reset
        progressBar.Progress = 0;
        statusLabel.Text = "Ready to start";
    }
    catch (Exception ex)
    {
        statusLabel.Text = $"Error: {ex.Message}";
        progressBar.IsIndeterminate = false;
        progressBar.Progress = 0;
    }
}
```

## Summary

| Aspect | Determinate | Indeterminate |
|--------|-------------|---------------|
| **Progress Value** | Specific (0-100%) | No specific value |
| **Animation** | Static until updated | Continuous loop |
| **Use Case** | Known progress | Unknown duration |
| **User Info** | Shows exact completion | Shows activity only |
| **Property** | `Progress` | `IsIndeterminate="True"` |