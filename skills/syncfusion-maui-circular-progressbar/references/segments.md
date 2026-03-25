# Segments in Circular ProgressBar

Split the circular progress bar into multiple segments to visualize progress of sequential tasks, multi-step processes, or staged workflows.

## Overview

Segments allow you to divide the progress bar into distinct sections, making it easy to:
- Show multi-step process completion
- Display progress through workflow stages
- Visualize sequential task completion
- Create segmented progress indicators

## Basic Segmentation

### SegmentCount Property

The `SegmentCount` property splits the circular progress bar into the specified number of segments.

**XAML:**
```xml
<progressBar:SfCircularProgressBar Progress="25" 
                                   SegmentCount="7" />
```

**C#:**
```csharp
SfCircularProgressBar circularProgressBar = new SfCircularProgressBar
{
    Progress = 25,
    SegmentCount = 7
};
```

### How Segments Work

- Total progress range is divided equally among segments
- Progress fills segments sequentially from start angle
- Each segment represents `100 / SegmentCount` percent
- Example: 4 segments = each represents 25%

## Gap Customization

### SegmentGapWidth Property

Customize the spacing between segments using the `SegmentGapWidth` property.

**XAML:**
```xml
<progressBar:SfCircularProgressBar Progress="25"
                                   SegmentCount="7" 
                                   SegmentGapWidth="10" />
```

**C#:**
```csharp
SfCircularProgressBar circularProgressBar = new SfCircularProgressBar
{
    Progress = 25,
    SegmentCount = 7,
    SegmentGapWidth = 10
};
```

### Gap Width Guidelines

- **Small gap (2-5)**: Subtle separation, modern look
- **Medium gap (6-10)**: Clear separation, good visibility
- **Large gap (11-15)**: Distinct segments, emphasis on stages

## Common Patterns

### Pattern 1: Multi-Step Form Progress

Track completion of form steps (4 steps).

```xml
<StackLayout Padding="20" Spacing="20">
    <Label Text="Complete Registration (Step 2 of 4)" 
           FontSize="16" 
           HorizontalTextAlignment="Center" />
    
    <progressBar:SfCircularProgressBar x:Name="formProgressBar"
                                       Progress="50"
                                       SegmentCount="4"
                                       SegmentGapWidth="8"
                                       ProgressFill="#FF007cee"
                                       TrackFill="#33007cee"
                                       HeightRequest="150"
                                       WidthRequest="150" />
</StackLayout>
```

### Pattern 2: Installation Progress

Show installation stages (5 stages).

```xml
<StackLayout>
    <progressBar:SfCircularProgressBar x:Name="installProgressBar"
                                    Progress="60"
                                    SegmentCount="5"
                                    SegmentGapWidth="6"
                                    ProgressFill="#FF4CAF50"
                                    TrackFill="#334CAF50">
        <progressBar:SfCircularProgressBar.Content>
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition />
                    <RowDefinition />
                </Grid.RowDefinitions>
                <Label x:Name="stageLabel"
                    Text="Installing..."
                    FontSize="14"
                    FontAttributes="Bold"
                    HorizontalTextAlignment="Center"
                    VerticalTextAlignment="End" />
                <Label Grid.Row="1"
                    x:Name="percentLabel"
                    Text="60%"
                    FontSize="20"
                    HorizontalTextAlignment="Center"
                    VerticalTextAlignment="Start" />
            </Grid>
        </progressBar:SfCircularProgressBar.Content>
    </progressBar:SfCircularProgressBar>
    <Button Text="Start Installation"
                    Clicked="OnStartClicked"
                    HorizontalOptions="Center" />
</StackLayout>
```

```csharp
private async void OnStartClicked(object sender, EventArgs e)
{
    await RunInstallation();
}

private async Task RunInstallation()
{
    string[] stages = { "Downloading", "Extracting", "Installing", "Configuring", "Finalizing" };
    
    for (int i = 0; i < stages.Length; i++)
    {
        stageLabel.Text = stages[i];
        double progress = ((i + 1) / (double)stages.Length) * 100;
        installProgressBar.Progress = progress;
        percentLabel.Text = $"{progress:F0}%";
        
        await Task.Delay(2000); // Simulate stage duration
    }
    
    stageLabel.Text = "Complete!";
}
```

## Combining Segments with Other Features

### Segments with Gradient Colors

Apply color gradients to segmented progress bars.

```xml
<progressBar:SfCircularProgressBar Progress="75"
                                   SegmentCount="4"
                                   SegmentGapWidth="8">
    <progressBar:SfCircularProgressBar.GradientStops>
        <!-- Red for first segment (0-25%) -->
        <progressBar:ProgressGradientStop Color="#FF4444" Value="0"/>
        <progressBar:ProgressGradientStop Color="#FF4444" Value="25"/>
        <!-- Yellow for second segment (25-50%) -->
        <progressBar:ProgressGradientStop Color="#FFBB33" Value="25"/>
        <progressBar:ProgressGradientStop Color="#FFBB33" Value="50"/>
        <!-- Orange for third segment (50-75%) -->
        <progressBar:ProgressGradientStop Color="#FF9800" Value="50"/>
        <progressBar:ProgressGradientStop Color="#FF9800" Value="75"/>
        <!-- Green for fourth segment (75-100%) -->
        <progressBar:ProgressGradientStop Color="#00C851" Value="75"/>
        <progressBar:ProgressGradientStop Color="#00C851" Value="100"/>
    </progressBar:SfCircularProgressBar.GradientStops>
</progressBar:SfCircularProgressBar>
```

### Segments with Semi-Circle

Create semi-circle segmented progress.

```xml
<progressBar:SfCircularProgressBar Progress="50"
                                   SegmentCount="6"
                                   SegmentGapWidth="6"
                                   StartAngle="180"
                                   EndAngle="360"
                                   ProgressFill="#FF2196F3"
                                   TrackFill="#332196F3" />
```

## Use Cases for Segments

### Ideal For:
- **Multi-step forms**: Registration, checkout, onboarding
- **Installation wizards**: Download → Extract → Install → Configure
- **Workflow stages**: Draft → Review → Approve → Publish
- **Level progression**: Game levels, achievement tiers
- **Time-based tracking**: Days of week, hours of day
- **Course modules**: Learning paths, training modules
- **Project phases**: Planning → Development → Testing → Deployment

### Not Ideal For:
- **Continuous progress**: Use regular progress bar without segments
- **Single task**: No need for segmentation
- **Highly granular progress**: Too many segments (>15) become hard to see

## Best Practices

1. **Segment count**: Use 3-12 segments for best visibility
2. **Gap width**: Match gap size to segment count (fewer segments = larger gaps)
3. **Colors**: Use contrasting colors for better segment visibility
4. **Labels**: Add text to indicate segment meaning or current stage
5. **Animation**: Use moderate animation duration (1000-2000ms) for smooth transitions
6. **Responsive**: Test segment visibility on different screen sizes
7. **Accessibility**: Provide text labels alongside visual segments

## Complete Example: Multi-Step Wizard

```xml
<StackLayout Padding="20" Spacing="20">
    <Label x:Name="wizardTitle"
           Text="Account Setup - Step 1 of 4"
           FontSize="18"
           FontAttributes="Bold"
           HorizontalTextAlignment="Center" />
    
    <progressBar:SfCircularProgressBar x:Name="wizardProgressBar"
                                       Progress="25"
                                       SegmentCount="4"
                                       SegmentGapWidth="10"
                                       ProgressFill="#FF673AB7"
                                       TrackFill="#33673AB7"
                                       AnimationDuration="1000"
                                       HeightRequest="200"
                                       WidthRequest="200">
        <progressBar:SfCircularProgressBar.Content>
            <StackLayout Spacing="5">
                <Label x:Name="stepLabel"
                       Text="Personal Info"
                       FontSize="14"
                       FontAttributes="Bold"
                       HorizontalTextAlignment="Center" />
                <Label x:Name="progressLabel"
                       Text="25%"
                       FontSize="20"
                       HorizontalTextAlignment="Center" />
            </StackLayout>
        </progressBar:SfCircularProgressBar.Content>
    </progressBar:SfCircularProgressBar>
    
    <Button Text="Next Step"
            Clicked="NextStep_Clicked"
            BackgroundColor="#FF673AB7"
            TextColor="White" />
</StackLayout>
```

```csharp
private int currentStep = 1;
private int totalSteps = 4;
private string[] stepNames = { "Personal Info", "Contact Details", "Preferences", "Review" };

private void NextStep_Clicked(object sender, EventArgs e)
{
    if (currentStep < totalSteps)
    {
        currentStep++;
        UpdateWizardProgress();
    }
    else
    {
        DisplayAlert("Complete", "Wizard finished!", "OK");
    }
}

private void UpdateWizardProgress()
{
    double progress = (currentStep / (double)totalSteps) * 100;
    wizardProgressBar.Progress = progress;
    
    wizardTitle.Text = $"Account Setup - Step {currentStep} of {totalSteps}";
    stepLabel.Text = stepNames[currentStep - 1];
    progressLabel.Text = $"{progress:F0}%";
}
```

## Summary

Segments transform the circular progress bar from a simple percentage indicator into a powerful multi-stage visualization tool. Use segments whenever you need to show progress through distinct, sequential stages.