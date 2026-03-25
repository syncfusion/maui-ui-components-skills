# Segments

Segments allow you to divide the Linear ProgressBar into multiple visual sections, making it ideal for showing progress across multiple sequential tasks or stages.

## When to Use Segments

Use segmented progress bars when you need to:

- **Visualize multi-step processes**: Installation wizards, onboarding flows, form completion
- **Show sequential tasks**: Build pipelines, deployment stages, workflow steps
- **Display gradual completion**: Learning modules, course progress, level progression
- **Break down complex operations**: Multi-phase data processing, batch operations
- **Emphasize milestones**: Show distinct progress checkpoints

## Basic Segmentation

### Setting Segment Count

Use the `SegmentCount` property to divide the progress bar into equal sections:

```xaml
<progressBar:SfLinearProgressBar Progress="25" 
                                 SegmentCount="4" />
```

```csharp
var progressBar = new SfLinearProgressBar
{
    Progress = 25,
    SegmentCount = 4  // Divides into 4 equal segments
};
```

### Visual Behavior

- Each segment represents an equal portion of the total range
- With 4 segments and Progress=25, the first segment is filled
- With 4 segments and Progress=50, the first two segments are filled
- Progress fills segments sequentially from left to right

## Segment Gap Customization

### Default Gap Width

By default, segments have a small gap between them. Customize this with `SegmentGapWidth`:

```xaml
<progressBar:SfLinearProgressBar Progress="50" 
                                 SegmentCount="4"
                                 SegmentGapWidth="5" />
```

```csharp
var progressBar = new SfLinearProgressBar
{
    Progress = 50,
    SegmentCount = 4,
    SegmentGapWidth = 5  // 5 pixels between segments
};
```

### No Gaps (Continuous Look)

Set `SegmentGapWidth` to 0 for a continuous appearance:

```csharp
var progressBar = new SfLinearProgressBar
{
    Progress = 75,
    SegmentCount = 4,
    SegmentGapWidth = 0  // No gaps, but segments still visually distinct
};
```

### Wide Gaps (Emphasized Separation)

Use larger values to emphasize discrete steps:

```csharp
var progressBar = new SfLinearProgressBar
{
    Progress = 50,
    SegmentCount = 4,
    SegmentGapWidth = 10  // Wide gaps for clear separation
};
```

## Segment Styling

### Basic Styling

```xaml
<progressBar:SfLinearProgressBar Progress="75" 
                                 SegmentCount="4"
                                 SegmentGapWidth="5"
                                 TrackHeight="12"
                                 ProgressHeight="12"
                                 TrackCornerRadius="6"
                                 ProgressCornerRadius="6"
                                 ProgressFill="#2196F3"
                                 TrackFill="#BBDEFB"/>
```

### Different Colors per Segment (Using Range Colors)

Combine segments with gradient stops for color-coded progress:

```xaml
<progressBar:SfLinearProgressBar Progress="100" 
                                 SegmentCount="4"
                                 SegmentGapWidth="5">
    <progressBar:SfLinearProgressBar.GradientStops>
        <!-- Segment 1: Green -->
        <progressBar:ProgressGradientStop Color="#4CAF50" Value="0"/>
        <progressBar:ProgressGradientStop Color="#4CAF50" Value="25"/>
        <!-- Segment 2: Blue -->
        <progressBar:ProgressGradientStop Color="#2196F3" Value="25"/>
        <progressBar:ProgressGradientStop Color="#2196F3" Value="50"/>
        <!-- Segment 3: Orange -->
        <progressBar:ProgressGradientStop Color="#FF9800" Value="50"/>
        <progressBar:ProgressGradientStop Color="#FF9800" Value="75"/>
        <!-- Segment 4: Red -->
        <progressBar:ProgressGradientStop Color="#F44336" Value="75"/>
        <progressBar:ProgressGradientStop Color="#F44336" Value="100"/>
    </progressBar:SfLinearProgressBar.GradientStops>
</progressBar:SfLinearProgressBar>
```

## Practical Examples

### Installation Wizard

```csharp
public class InstallationWizard : ContentPage
{
    private SfLinearProgressBar installProgress;
    private Label statusLabel;
    private readonly string[] installSteps = 
    { 
        "Preparing...", 
        "Installing Files...", 
        "Configuring...", 
        "Finalizing..." 
    };
    
    private int currentStepIndex = 0;

    public InstallationWizard()
    {
        installProgress = new SfLinearProgressBar
        {
            SegmentCount = 4,
            SegmentGapWidth = 6,
            Progress = 0,
            TrackHeight = 14,
            ProgressHeight = 14,
            ProgressCornerRadius = 7,
            TrackCornerRadius = 7,
            ProgressFill = Color.FromArgb("#4CAF50"),
            TrackFill = Color.FromArgb("#E0E0E0")
        };

        statusLabel = new Label
        {
            Text = installSteps[0],
            HorizontalOptions = LayoutOptions.Center,
            Margin = new Thickness(0, 10, 0, 0)
        };

        Content = new StackLayout
        {
            Padding = 40,
            Spacing = 20,
            Children = { installProgress, statusLabel }
        };

        StartInstallation();
    }

    private async void StartInstallation()
    {
        for (int i = 0; i < installSteps.Length; i++)
        {
            currentStepIndex = i;
            statusLabel.Text = installSteps[i];
            
            // Update progress
            double progress = ((i + 1) / (double)installSteps.Length) * 100;
            installProgress.Progress = progress;
            
            // Simulate step duration
            await Task.Delay(2000);
        }

        statusLabel.Text = "Installation Complete!";
    }
}
```

## Best Practices

1. **Match segment count to actual steps**: If you have 5 steps, use `SegmentCount="5"`
2. **Keep segment count reasonable**: 3-10 segments work best visually
3. **Use descriptive labels**: Show which step the user is on
4. **Provide context**: Display "Step X of Y" alongside the visual progress
5. **Consider gap width**: Smaller gaps for many segments, larger for fewer segments
6. **Use consistent sizing**: Keep track and progress heights the same for clean look

## Common Mistakes to Avoid

❌ **Too many segments**: More than 10-12 segments becomes hard to distinguish
```csharp
// WRONG: Too granular
var progressBar = new SfLinearProgressBar { SegmentCount = 50 };
```

✅ **Appropriate segment count**:
```csharp
// RIGHT: Clear, countable segments
var progressBar = new SfLinearProgressBar { SegmentCount = 5 };
```

❌ **Mismatched progress and segments**: Progress doesn't align with logical steps
```csharp
// WRONG: 4 segments but calculating for 5 steps
progressBar.SegmentCount = 4;
progressBar.Progress = (currentStep / 5.0) * 100;  // Doesn't align
```

✅ **Aligned calculation**:
```csharp
// RIGHT: Segments match step count
progressBar.SegmentCount = 5;
progressBar.Progress = (currentStep / 5.0) * 100;
```