# Events

The Linear ProgressBar provides two events to help you respond to progress changes and completion, enabling dynamic UI updates and custom logic based on progress state.

## Available Events

| Event | Trigger Condition | Event Args |
|-------|------------------|------------|
| **ProgressChanged** | Whenever the `Progress` property changes | `ProgressValueEventArgs` with `Progress` value |
| **ProgressCompleted** | When `Progress` reaches `Maximum` value | `ProgressValueEventArgs` with `Progress` value |

## ProgressChanged Event

Fires whenever the progress value changes, allowing you to respond in real-time to progress updates.

### Event Signature

```csharp
public event EventHandler<ProgressValueEventArgs> ProgressChanged;
```

### Event Arguments

```csharp
public class ProgressValueEventArgs : EventArgs
{
    public double Progress { get; set; }
}
```

### Basic Usage

**XAML:**

```xaml
<progressBar:SfLinearProgressBar x:Name="progressBar" 
                                 ProgressChanged="OnProgressChanged"
                                 Progress="0"/>
```

**Code-behind:**

```csharp
private void OnProgressChanged(object sender, ProgressValueEventArgs e)
{
    // Access current progress value
    double currentProgress = e.Progress;
    
    // Update UI or perform logic
    Debug.WriteLine($"Progress: {currentProgress}%");
}
```

**C# Only:**

```csharp
var progressBar = new SfLinearProgressBar { Progress = 0 };

progressBar.ProgressChanged += (sender, e) =>
{
    Debug.WriteLine($"Progress changed to: {e.Progress}%");
};
```

### Practical Example: Dynamic Color Based on Progress

```csharp
public partial class DynamicProgressPage : ContentPage
{
    public DynamicProgressPage()
    {
        InitializeComponent();
        
        progressBar.ProgressChanged += OnProgressChanged;
    }

    private void OnProgressChanged(object sender, ProgressValueEventArgs e)
    {
        // Change color based on progress thresholds
        if (e.Progress < 30)
        {
            // Low progress: Red
            progressBar.ProgressFill = Colors.Red;
            statusLabel.Text = "Getting started...";
            statusLabel.TextColor = Colors.Red;
        }
        else if (e.Progress < 70)
        {
            // Medium progress: Orange
            progressBar.ProgressFill = Colors.Orange;
            statusLabel.Text = "Making progress...";
            statusLabel.TextColor = Colors.Orange;
        }
        else if (e.Progress < 100)
        {
            // High progress: Yellow/Amber
            progressBar.ProgressFill = Color.FromArgb("#FFC107");
            statusLabel.Text = "Almost there!";
            statusLabel.TextColor = Color.FromArgb("#FFC107");
        }
        else
        {
            // Complete: Green
            progressBar.ProgressFill = Colors.Green;
            statusLabel.Text = "Complete!";
            statusLabel.TextColor = Colors.Green;
        }
    }
}
```

### Example: Update Status Label

```csharp
public class UploadPage : ContentPage
{
    private SfLinearProgressBar progressBar;
    private Label percentLabel;

    public UploadPage()
    {
        progressBar = new SfLinearProgressBar { Progress = 0 };
        percentLabel = new Label { Text = "0%", HorizontalOptions = LayoutOptions.Center };

        progressBar.ProgressChanged += (sender, e) =>
        {
            // Update percentage display
            percentLabel.Text = $"{e.Progress:F1}%";
        };

        Content = new StackLayout
        {
            Padding = 20,
            Children = { progressBar, percentLabel }
        };
    }
}
```

## ProgressCompleted Event

Fires when the progress value reaches the maximum value, signaling task completion.

### Event Signature

```csharp
public event EventHandler<ProgressValueEventArgs> ProgressCompleted;
```

### Basic Usage

**XAML:**

```xaml
<progressBar:SfLinearProgressBar x:Name="progressBar" 
                                 ProgressCompleted="OnProgressCompleted"
                                 Progress="0"/>
```

**Code-behind:**

```csharp
private void OnProgressCompleted(object sender, ProgressValueEventArgs e)
{
    // Task completed
    DisplayAlert("Success", "Task completed successfully!", "OK");
}
```

**C# Only:**

```csharp
var progressBar = new SfLinearProgressBar { Progress = 0 };

progressBar.ProgressCompleted += async (sender, e) =>
{
    await DisplayAlert("Complete", "All done!", "OK");
};
```

### Example: Hide Progress Bar on Completion

```csharp

public class DownloadPage : ContentPage
{
    private SfLinearProgressBar downloadProgress;
    private Label downloadLabel;
    private Button openFileButton;

    public DownloadPage()
    {
        // Initialize controls
        downloadProgress = new SfLinearProgressBar
        {
            Progress = 0,
            IsVisible = true,
            HeightRequest = 20,
            WidthRequest = 200,
            ProgressFill = Colors.Blue
        };

        downloadLabel = new Label
        {
            Text = "Downloading...",
            TextColor = Colors.Black,
            HorizontalOptions = LayoutOptions.Center
        };

        openFileButton = new Button
        {
            Text = "Open File",
            IsVisible = false
        };

        // Subscribe to progress completed event
        downloadProgress.ProgressCompleted += (sender, e) =>
        {
            // Hide progress bar
            downloadProgress.IsVisible = false;

            // Update label
            downloadLabel.Text = "Download complete!";
            downloadLabel.TextColor = Colors.Green;

            // Show action button
            openFileButton.IsVisible = true;
        };

        // Example action for button
        openFileButton.Clicked += async (s, e) =>
        {
            await DisplayAlert("Action", "File opened successfully!", "OK");
        };

        // Layout
        Content = new StackLayout
        {
            Padding = 20,
            Spacing = 15,
            Children =
            {
                downloadLabel,
                downloadProgress,
                openFileButton
            }
        };
    }
}
```

### Example: Enable Next Button

```csharp

public class WizardPage : ContentPage
{
    private SfLinearProgressBar stepProgress;
    private Button nextButton;

    public WizardPage()
    {
        // Initialize controls
        stepProgress = new SfLinearProgressBar
        {
            Progress = 0,
            HeightRequest = 20,
            WidthRequest = 200,
            ProgressFill = Colors.Orange
        };

        nextButton = new Button
        {
            Text = "Next",
            IsEnabled = false,
            BackgroundColor = Colors.LightGray
        };

        // Subscribe to progress completed event
        stepProgress.ProgressCompleted += (sender, e) =>
        {
            // Enable navigation to next step
            nextButton.IsEnabled = true;
            nextButton.BackgroundColor = Colors.Blue;

            // Optional: Auto-advance after delay
            Device.StartTimer(TimeSpan.FromSeconds(2), () =>
            {
                OnNextButtonClicked(null, null);
                return false;  // Stop timer
            });
        };

        // Wire up button click
        nextButton.Clicked += OnNextButtonClicked;

        // Layout
        Content = new StackLayout
        {
            Padding = 20,
            Spacing = 15,
            Children =
            {
                new Label { Text = "Step 1: Progressing...", HorizontalOptions = LayoutOptions.Center },
                stepProgress,
                nextButton
            }
        };
    }
}
```

## Combining Both Events

Use both events together for comprehensive progress tracking:

```csharp
public class ComprehensiveProgressPage : ContentPage
{
    private SfLinearProgressBar progressBar;
    private Label statusLabel;
    private Label percentLabel;
    private Button cancelButton;
    private Button doneButton;

    public ComprehensiveProgressPage()
    {
        progressBar = new SfLinearProgressBar { Progress = 0 };
        statusLabel = new Label { Text = "Starting..." };
        percentLabel = new Label { Text = "0%" };
        cancelButton = new Button { Text = "Cancel", IsVisible = true };
        doneButton = new Button { Text = "Done", IsVisible = false };

        // Track all progress changes
        progressBar.ProgressChanged += (sender, e) =>
        {
            percentLabel.Text = $"{e.Progress:F1}%";
            
            if (e.Progress < 33)
                statusLabel.Text = "Initializing...";
            else if (e.Progress < 66)
                statusLabel.Text = "Processing...";
            else if (e.Progress < 100)
                statusLabel.Text = "Finalizing...";
        };

        // Handle completion
        progressBar.ProgressCompleted += (sender, e) =>
        {
            statusLabel.Text = "Complete!";
            statusLabel.TextColor = Colors.Green;
            progressBar.ProgressFill = Colors.Green;
            
            cancelButton.IsVisible = false;
            doneButton.IsVisible = true;
        };

        Content = new StackLayout
        {
            Padding = 20,
            Children = 
            { 
                statusLabel, 
                progressBar, 
                percentLabel, 
                cancelButton, 
                doneButton 
            }
        };
    }
}
```

## Common Mistakes

❌ **Memory leaks from not unsubscribing**
```csharp
// WRONG: Event handler never removed
public void StartTask()
{
    var progressBar = new SfLinearProgressBar();
    progressBar.ProgressChanged += OnProgressChanged;
    // progressBar goes out of scope but event handler remains
}
```

✅ **Proper cleanup**
```csharp
// RIGHT: Unsubscribe when done
public void StartTask()
{
    var progressBar = new SfLinearProgressBar();
    EventHandler<ProgressValueEventArgs> handler = null;
    handler = (sender, e) =>
    {
        OnProgressChanged(e.Progress);
        if (e.Progress >= 100)
        {
            progressBar.ProgressChanged -= handler;  // Cleanup
        }
    };
    
    progressBar.ProgressChanged += handler;
}
```