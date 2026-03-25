# Progress States

The Linear ProgressBar supports three distinct states to handle different progress scenarios. Understanding when to use each state is crucial for providing appropriate user feedback.

## Overview of States

| State | Use When | Visual Behavior |
|-------|----------|----------------|
| **Determinate** | Progress is known and measurable | Progress indicator fills from 0% to 100% |
| **Indeterminate** | Duration is unknown or being calculated | Continuous animation shows activity |
| **Buffer** | Two concurrent progress operations | Shows both primary and secondary progress |

## Determinate State

**Default state** - Use when you can calculate and display exact progress.

### When to Use

- File uploads/downloads with known file size
- Data processing with countable items
- Multi-step forms with known total steps
- Installation processes with defined stages
- Any task where you can calculate: `(completed / total) * 100`

### Implementation

```xaml
<!-- Progress value between 0 and 100 -->
<progressBar:SfLinearProgressBar Progress="75"/>
```

```csharp
var progressBar = new SfLinearProgressBar 
{ 
    Progress = 75  // 75% complete
};
```

### Dynamic Progress Updates

```csharp
// Update progress programmatically
public void UpdateProgress(double completed, double total)
{
    double progressValue = (completed / total) * 100;
    linearProgressBar.Progress = progressValue;
}

// Example: File upload
private async Task UploadFileAsync(Stream fileStream)
{
    long totalBytes = fileStream.Length;
    long uploadedBytes = 0;
    byte[] buffer = new byte[8192];
    int bytesRead;

    while ((bytesRead = await fileStream.ReadAsync(buffer, 0, buffer.Length)) > 0)
    {
        // Upload chunk...
        uploadedBytes += bytesRead;
        
        // Update progress bar
        double progress = (uploadedBytes / (double)totalBytes) * 100;
        linearProgressBar.Progress = progress;
    }
}
```

### Custom Range

By default, Progress accepts 0-100. For different ranges:

```csharp
// Use 0.0 to 1.0 (factor values)
var progressBar = new SfLinearProgressBar
{
    Minimum = 0,
    Maximum = 1,
    Progress = 0.75  // 75%
};

// Custom range: 0 to 500
var customProgress = new SfLinearProgressBar
{
    Minimum = 0,
    Maximum = 500,
    Progress = 375  // 75%
};
```

## Indeterminate State

Use when task duration is unknown or progress cannot be calculated.

### When to Use

- Initial connection to server (unknown latency)
- Processing tasks without countable steps
- Waiting for external service responses
- Tasks where calculation overhead would slow performance
- Login/authentication flows
- Search operations with unknown result count

### Implementation

```xaml
<progressBar:SfLinearProgressBar IsIndeterminate="True"/>
```

```csharp
var loadingIndicator = new SfLinearProgressBar 
{ 
    IsIndeterminate = true 
};
```

### Visual Behavior

The indeterminate state displays a continuous animation where the progress indicator moves back and forth across the track, showing that work is being done without specific completion percentage.

## Buffer State

Shows two progress indicators: primary progress and secondary (buffer) progress.

### When to Use

- **Video/Audio streaming**: Playback position (primary) vs buffered content (secondary)
- **Multi-phase operations**: Current step (primary) vs preparation of next step (secondary)
- **Background pre-loading**: Active task (primary) vs pre-cached content (secondary)
- **Download managers**: Current file (primary) vs total download queue (secondary)

### Implementation

```xaml
<progressBar:SfLinearProgressBar Progress="25" 
                                 SecondaryProgress="75"/>
```

```csharp
var bufferProgressBar = new SfLinearProgressBar
{
    Progress = 25,            // Primary: 25%
    SecondaryProgress = 75    // Secondary: 75%
};
```

### Visual Appearance

- **Track**: Background (usually light gray)
- **Secondary Progress**: Shows buffered/prepared content (usually lighter accent color)
- **Primary Progress**: Shows current active progress (usually darker/primary accent color)

### Styling Buffer Progress

Customize colors to distinguish primary from secondary:

```xaml
<progressBar:SfLinearProgressBar Progress="30" 
                                 SecondaryProgress="70"
                                 ProgressFill="#FF4CAF50"
                                 SecondaryProgressFill="#8BC34A"
                                 TrackFill="#E0E0E0"/>
```

```csharp
var bufferBar = new SfLinearProgressBar
{
    Progress = 30,
    SecondaryProgress = 70,
    ProgressFill = Color.FromArgb("#FF4CAF50"),      // Green
    SecondaryProgressFill = Color.FromArgb("#8BC34A"), // Light green
    TrackFill = Color.FromArgb("#E0E0E0")             // Light gray
};
```

## State Decision Tree

```
Is progress measurable/calculable?
├── YES → Use Determinate State
│   ├── Single operation? → Set Progress property
│   └── Multiple phases? → Consider Buffer State with SecondaryProgress
│
└── NO → Use Indeterminate State
    ├── Will it become measurable later? → Start indeterminate, switch to determinate
    └── Always unknown? → Keep indeterminate throughout
```

## Best Practices

1. **Always provide feedback**: Never leave the UI frozen without any progress indicator
2. **Choose the right state**: Match the state to the actual nature of the task
3. **Smooth transitions**: When switching states, ensure visual continuity
4. **Hide when done**: Remove or hide the progress bar once the task completes
5. **Accessibility**: Indeterminate states should have aria-labels indicating "loading" or "processing"

## Common Mistakes to Avoid

❌ **Using determinate with guessed progress**: Don't fake progress values if you can't calculate them accurately
```csharp
// WRONG: Fake progress that doesn't reflect reality
progressBar.Progress = 50;  // Just showing "something"
```

✅ **Use indeterminate instead**:
```csharp
// RIGHT: Show activity without misleading the user
progressBar.IsIndeterminate = true;
```

❌ **Forgetting to hide progress bars**:
```csharp
// WRONG: Progress bar stays visible after completion
await LongRunningTaskAsync();
// Forgot to hide or update the UI
```

✅ **Always clean up**:
```csharp
// RIGHT: Hide or update when done
try
{
    progressBar.IsVisible = true;
    await LongRunningTaskAsync();
}
finally
{
    progressBar.IsVisible = false;
}
```