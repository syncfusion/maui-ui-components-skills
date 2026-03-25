# Events and Methods

This guide covers all events and methods available in the SignaturePad control, including event handling patterns, validation workflows, and practical use cases.

## Table of Contents
- [Overview](#overview)
- [DrawStarted Event](#drawstarted-event)
- [DrawCompleted Event](#drawcompleted-event)
- [Clear Method](#clear-method)
- [Event Handling Patterns](#event-handling-patterns)
- [Validation Workflows](#validation-workflows)
- [Event Sequencing and Lifecycle](#event-sequencing-and-lifecycle)
- [Complete Examples](#complete-examples)

## Overview

SignaturePad provides two key events and one primary method for signature management:

### Events
1. **DrawStarted** - Fires when drawing begins (supports cancellation)
2. **DrawCompleted** - Fires when a drawing stroke completes

### Methods
1. **Clear()** - Removes all drawn content from the pad
2. **ToImageSource()** - Converts signature to image (covered in saving-signatures.md)
3. **GetSignaturePoints()** - Retrieves point data (covered in saving-signatures.md)

## DrawStarted Event

Triggered when the user begins drawing on the SignaturePad. This event can be cancelled to prevent drawing.

### Event Signature

```csharp
public event EventHandler<CancelEventArgs>? DrawStarted;
```

### EventArgs Type
- **Type:** `CancelEventArgs`
- **Property:** `Cancel` (bool) - Set to `true` to prevent drawing

### Basic Usage (XAML)

```xml
<signaturePad:SfSignaturePad x:Name="signaturePad"
                              DrawStarted="OnDrawStarted" />
```

```csharp
private void OnDrawStarted(object sender, CancelEventArgs e)
{
    Console.WriteLine("User started drawing");
    // Allow drawing by default (e.Cancel = false)
}
```

### Basic Usage (C#)

```csharp
public MainPage()
{
    InitializeComponent();
    
    signaturePad.DrawStarted += OnDrawStarted;
}

private void OnDrawStarted(object sender, CancelEventArgs e)
{
    Console.WriteLine("Drawing started");
}
```

### Cancelling Draw Action

Prevent drawing by setting `e.Cancel = true`:

```csharp
private bool isSigningAllowed = false;

private void OnDrawStarted(object sender, CancelEventArgs e)
{
    if (!isSigningAllowed)
    {
        e.Cancel = true;
        DisplayAlert("Not Allowed", "Signing is not permitted at this time", "OK");
    }
}
```

### Use Cases for DrawStarted

**1. Permission Validation**
```csharp
private void OnDrawStarted(object sender, CancelEventArgs e)
{
    if (!IsUserAuthenticated())
    {
        e.Cancel = true;
        DisplayAlert("Authentication Required", "Please sign in before signing", "OK");
    }
}
```

**2. Document Readiness Check**
```csharp
private bool documentReviewed = false;

private void OnDrawStarted(object sender, CancelEventArgs e)
{
    if (!documentReviewed)
    {
        e.Cancel = true;
        DisplayAlert("Review Required", 
            "Please review the document before signing", "OK");
    }
}
```

**3. Terms Acceptance**
```csharp
private bool termsAccepted = false;

private void OnDrawStarted(object sender, CancelEventArgs e)
{
    if (!termsAccepted)
    {
        e.Cancel = true;
        DisplayAlert("Terms Not Accepted", 
            "Please accept the terms and conditions", "OK");
    }
}
```

**4. Time-Based Restrictions**
```csharp
private DateTime? expirationTime;

private void OnDrawStarted(object sender, CancelEventArgs e)
{
    if (expirationTime.HasValue && DateTime.Now > expirationTime.Value)
    {
        e.Cancel = true;
        DisplayAlert("Expired", "The signing window has expired", "OK");
    }
}
```

**5. Signature Limit**
```csharp
private bool signatureAlreadyCaptured = false;

private void OnDrawStarted(object sender, CancelEventArgs e)
{
    if (signatureAlreadyCaptured)
    {
        e.Cancel = true;
        DisplayAlert("Already Signed", 
            "You have already signed this document", "OK");
    }
}
```

## DrawCompleted Event

Triggered when the user completes a drawing stroke on the SignaturePad.

### Event Signature

```csharp
public event EventHandler? DrawCompleted;
```

### EventArgs Type
- **Type:** `EventArgs` (standard, no special properties)

### Basic Usage (XAML)

```xml
<signaturePad:SfSignaturePad x:Name="signaturePad"
                              DrawCompleted="OnDrawCompleted" />
```

```csharp
private void OnDrawCompleted(object sender, EventArgs e)
{
    Console.WriteLine("Drawing stroke completed");
}
```

### Basic Usage (C#)

```csharp
public MainPage()
{
    InitializeComponent();
    
    signaturePad.DrawCompleted += OnDrawCompleted;
}

private void OnDrawCompleted(object sender, EventArgs e)
{
    Console.WriteLine("Stroke completed");
}
```

### Use Cases for DrawCompleted

**1. Enable Save Button**
```csharp
private Button saveButton;

private void OnDrawCompleted(object sender, EventArgs e)
{
    // Enable save button once signature exists
    saveButton.IsEnabled = true;
}
```

**2. Auto-Save**
```csharp
private void OnDrawCompleted(object sender, EventArgs e)
{
    // Automatically save after each stroke
    AutoSaveSignature();
}

private async void AutoSaveSignature()
{
    ImageSource? signature = signaturePad.ToImageSource();
    if (signature != null)
    {
        await SaveToTempStorageAsync(signature);
    }
}
```

**3. Signature Analysis**
```csharp
private void OnDrawCompleted(object sender, EventArgs e)
{
    List<List<float>> points = signaturePad.GetSignaturePoints();
    AnalyzeSignatureQuality(points);
}

private void AnalyzeSignatureQuality(List<List<float>> points)
{
    int strokeCount = points.Count;
    int totalPoints = points.Sum(s => s.Count / 2);
    
    if (strokeCount < 2)
    {
        ShowWarning("Signature appears too simple. Please sign normally.");
    }
}
```

**4. Progress Tracking**
```csharp
private int strokeCount = 0;

private void OnDrawCompleted(object sender, EventArgs e)
{
    strokeCount++;
    UpdateProgressIndicator($"Strokes: {strokeCount}");
}
```

**5. Validation Feedback**
```csharp
private void OnDrawCompleted(object sender, EventArgs e)
{
    List<List<float>> points = signaturePad.GetSignaturePoints();
    
    if (IsSignatureTooSmall(points))
    {
        DisplayAlert("Info", "Signature is small. Consider making it larger", "OK");
    }
}

private bool IsSignatureTooSmall(List<List<float>> points)
{
    // Calculate bounding box and check size
    // Return true if too small
    return false; // Placeholder
}
```

## Clear Method

Removes all drawn content from the SignaturePad, resetting it to empty state.

### Method Signature

```csharp
public void Clear()
```

### Basic Usage

```csharp
private void OnClearButtonClicked(object sender, EventArgs e)
{
    signaturePad.Clear();
}
```

### Complete Example (XAML + Code)

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    
    <signaturePad:SfSignaturePad x:Name="signaturePad" Grid.Row="0" />
    
    <HorizontalStackLayout Grid.Row="1" Spacing="10" Padding="10">
        <Button Text="Clear" Clicked="OnClearClicked" />
        <Button Text="Save" Clicked="OnSaveClicked" />
    </HorizontalStackLayout>
</Grid>
```

```csharp
private void OnClearClicked(object sender, EventArgs e)
{
    signaturePad.Clear();
}

private void OnSaveClicked(object sender, EventArgs e)
{
    ImageSource? signature = signaturePad.ToImageSource();
    
    if (signature != null)
    {
        // Save signature
        DisplayAlert("Success", "Signature saved", "OK");
        signaturePad.Clear(); // Clear after saving
    }
}
```

### Clear with Confirmation

```csharp
private async void OnClearClicked(object sender, EventArgs e)
{
    bool confirm = await DisplayAlert(
        "Confirm", 
        "Are you sure you want to clear the signature?", 
        "Yes", 
        "No");
    
    if (confirm)
    {
        signaturePad.Clear();
    }
}
```

### Auto-Clear Scenarios

```csharp
// Clear after successful save
private async void OnSaveClicked(object sender, EventArgs e)
{
    bool saved = await SaveSignatureAsync();
    
    if (saved)
    {
        signaturePad.Clear();
        await DisplayAlert("Success", "Signature saved and cleared", "OK");
    }
}

// Clear after timeout
private async void StartSignatureTimeout()
{
    await Task.Delay(TimeSpan.FromMinutes(5));
    
    if (!signatureWasSaved)
    {
        signaturePad.Clear();
        await DisplayAlert("Timeout", "Signature cleared due to inactivity", "OK");
    }
}
```

## Event Handling Patterns

### Pattern 1: Basic Event Subscription

```csharp
public MainPage()
{
    InitializeComponent();
    
    // Subscribe to events
    signaturePad.DrawStarted += SignaturePad_DrawStarted;
    signaturePad.DrawCompleted += SignaturePad_DrawCompleted;
}

private void SignaturePad_DrawStarted(object sender, CancelEventArgs e)
{
    Console.WriteLine("Started");
}

private void SignaturePad_DrawCompleted(object sender, EventArgs e)
{
    Console.WriteLine("Completed");
}

// Don't forget to unsubscribe when appropriate
protected override void OnDisappearing()
{
    base.OnDisappearing();
    
    signaturePad.DrawStarted -= SignaturePad_DrawStarted;
    signaturePad.DrawCompleted -= SignaturePad_DrawCompleted;
}
```

### Pattern 2: Inline Lambda Expressions

```csharp
public MainPage()
{
    InitializeComponent();
    
    signaturePad.DrawStarted += (s, e) =>
    {
        if (!IsReady())
        {
            e.Cancel = true;
        }
    };
    
    signaturePad.DrawCompleted += (s, e) =>
    {
        UpdateUI();
    };
}
```

### Pattern 3: Async Event Handlers

```csharp
public MainPage()
{
    InitializeComponent();
    
    signaturePad.DrawCompleted += async (s, e) =>
    {
        await ProcessSignatureAsync();
    };
}

private async Task ProcessSignatureAsync()
{
    ImageSource? signature = signaturePad.ToImageSource();
    
    if (signature != null)
    {
        await SaveToCloudAsync(signature);
    }
}
```

### Pattern 4: State Machine

```csharp
private enum SignatureState
{
    Empty,
    Drawing,
    Completed,
    Saved
}

private SignatureState currentState = SignatureState.Empty;

private void OnDrawStarted(object sender, CancelEventArgs e)
{
    if (currentState == SignatureState.Saved)
    {
        e.Cancel = true;
        DisplayAlert("Already Signed", "Document is already signed", "OK");
        return;
    }
    
    currentState = SignatureState.Drawing;
    UpdateUIForState();
}

private void OnDrawCompleted(object sender, EventArgs e)
{
    currentState = SignatureState.Completed;
    UpdateUIForState();
}

private void UpdateUIForState()
{
    switch (currentState)
    {
        case SignatureState.Empty:
            saveButton.IsEnabled = false;
            clearButton.IsEnabled = false;
            break;
        case SignatureState.Drawing:
            saveButton.IsEnabled = false;
            clearButton.IsEnabled = true;
            break;
        case SignatureState.Completed:
            saveButton.IsEnabled = true;
            clearButton.IsEnabled = true;
            break;
        case SignatureState.Saved:
            saveButton.IsEnabled = false;
            clearButton.IsEnabled = false;
            break;
    }
}
```

## Validation Workflows

### Workflow 1: Multi-Stage Validation

```csharp
public class SignatureValidationWorkflow
{
    private SfSignaturePad signaturePad;
    private bool isDocumentReviewed;
    private bool areTermsAccepted;
    private bool isUserVerified;
    
    public SignatureValidationWorkflow(SfSignaturePad pad)
    {
        signaturePad = pad;
        signaturePad.DrawStarted += ValidateBeforeDrawing;
        signaturePad.DrawCompleted += ValidateAfterDrawing;
    }
    
    private void ValidateBeforeDrawing(object sender, CancelEventArgs e)
    {
        // Stage 1: Document review
        if (!isDocumentReviewed)
        {
            e.Cancel = true;
            ShowError("Please review the document first");
            return;
        }
        
        // Stage 2: Terms acceptance
        if (!areTermsAccepted)
        {
            e.Cancel = true;
            ShowError("Please accept the terms and conditions");
            return;
        }
        
        // Stage 3: User verification
        if (!isUserVerified)
        {
            e.Cancel = true;
            ShowError("Please verify your identity");
            return;
        }
        
        // All checks passed
        Console.WriteLine("All validation checks passed. Drawing allowed.");
    }
    
    private void ValidateAfterDrawing(object sender, EventArgs e)
    {
        List<List<float>> points = signaturePad.GetSignaturePoints();
        
        // Validate signature quality
        if (points.Count < 2)
        {
            ShowWarning("Signature appears incomplete. Please sign normally.");
        }
    }
    
    private void ShowError(string message)
    {
        // Display error to user
    }
    
    private void ShowWarning(string message)
    {
        // Display warning to user
    }
}
```

### Workflow 2: Timed Signature Capture

```csharp
public class TimedSignatureCapture
{
    private SfSignaturePad signaturePad;
    private DateTime? drawStartTime;
    private TimeSpan maximumDrawTime = TimeSpan.FromSeconds(30);
    
    public TimedSignatureCapture(SfSignaturePad pad)
    {
        signaturePad = pad;
        signaturePad.DrawStarted += OnDrawStarted;
        signaturePad.DrawCompleted += OnDrawCompleted;
    }
    
    private void OnDrawStarted(object sender, CancelEventArgs e)
    {
        if (!drawStartTime.HasValue)
        {
            drawStartTime = DateTime.Now;
            StartTimeoutTimer();
        }
    }
    
    private void OnDrawCompleted(object sender, EventArgs e)
    {
        if (drawStartTime.HasValue)
        {
            TimeSpan duration = DateTime.Now - drawStartTime.Value;
            Console.WriteLine($"Signature completed in {duration.TotalSeconds:F1} seconds");
        }
    }
    
    private async void StartTimeoutTimer()
    {
        await Task.Delay(maximumDrawTime);
        
        if (drawStartTime.HasValue)
        {
            signaturePad.Clear();
            ShowMessage("Timeout: Please sign within the time limit");
            drawStartTime = null;
        }
    }
    
    private void ShowMessage(string message)
    {
        // Display message to user
    }
}
```

### Workflow 3: Real-Time Signature Backup

```csharp
public class SignatureBackupWorkflow
{
    private SfSignaturePad signaturePad;
    private System.Timers.Timer backupTimer;
    
    public SignatureBackupWorkflow(SfSignaturePad pad)
    {
        signaturePad = pad;
        signaturePad.DrawStarted += OnDrawStarted;
        signaturePad.DrawCompleted += OnDrawCompleted;
        
        // Create backup timer (every 2 seconds)
        backupTimer = new System.Timers.Timer(2000);
        backupTimer.Elapsed += async (s, e) => await BackupSignatureAsync();
    }
    
    private void OnDrawStarted(object sender, CancelEventArgs e)
    {
        // Start auto-backup when drawing begins
        backupTimer.Start();
    }
    
    private void OnDrawCompleted(object sender, EventArgs e)
    {
        // Keep timer running for more strokes
    }
    
    private async Task BackupSignatureAsync()
    {
        ImageSource? signature = signaturePad.ToImageSource();
        
        if (signature != null)
        {
            await SaveToTempStorageAsync(signature);
            Console.WriteLine("Signature backed up");
        }
    }
    
    public void StopBackup()
    {
        backupTimer?.Stop();
        backupTimer?.Dispose();
    }
    
    private Task SaveToTempStorageAsync(ImageSource signature)
    {
        // Save to temporary storage
        return Task.CompletedTask;
    }
}
```

## Event Sequencing and Lifecycle

### Typical Event Flow

```
1. User touches pad
   ↓
2. DrawStarted event fires
   ↓
3. Event handler checks conditions
   ↓
4. If e.Cancel = true → Drawing prevented (end)
   If e.Cancel = false → Drawing allowed
   ↓
5. User draws signature stroke
   ↓
6. User lifts finger/pen
   ↓
7. DrawCompleted event fires
   ↓
8. Process completed stroke
   ↓
9. (Optional) User continues with more strokes (back to step 1)
   ↓
10. (Optional) User clicks Clear → Clear() method called → Back to initial state
```

### Multiple Strokes

```csharp
private int strokeCounter = 0;

private void OnDrawStarted(object sender, CancelEventArgs e)
{
    Console.WriteLine($"Starting stroke #{strokeCounter + 1}");
}

private void OnDrawCompleted(object sender, EventArgs e)
{
    strokeCounter++;
    Console.WriteLine($"Completed stroke #{strokeCounter}");
    
    List<List<float>> points = signaturePad.GetSignaturePoints();
    Console.WriteLine($"Total strokes in signature: {points.Count}");
}
```

### Event Lifecycle Management

```csharp
public class SignaturePageLifecycle : ContentPage
{
    private SfSignaturePad signaturePad;
    
    public SignaturePageLifecycle()
    {
        InitializeComponent();
        SetupSignaturePad();
    }
    
    private void SetupSignaturePad()
    {
        signaturePad = new SfSignaturePad();
        
        // Subscribe to events
        signaturePad.DrawStarted += OnDrawStarted;
        signaturePad.DrawCompleted += OnDrawCompleted;
        
        Content = signaturePad;
    }
    
    protected override void OnAppearing()
    {
        base.OnAppearing();
        Console.WriteLine("Page appearing - signature pad ready");
    }
    
    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        
        // Unsubscribe from events to prevent memory leaks
        if (signaturePad != null)
        {
            signaturePad.DrawStarted -= OnDrawStarted;
            signaturePad.DrawCompleted -= OnDrawCompleted;
        }
        
        Console.WriteLine("Page disappearing - events unsubscribed");
    }
    
    private void OnDrawStarted(object sender, CancelEventArgs e)
    {
        // Handler implementation
    }
    
    private void OnDrawCompleted(object sender, EventArgs e)
    {
        // Handler implementation
    }
}
```

## Complete Examples

### Example 1: Full Document Signing Workflow

```csharp
public class DocumentSigningPage : ContentPage
{
    private SfSignaturePad signaturePad;
    private CheckBox termsCheckBox;
    private Button saveButton;
    private Label statusLabel;
    
    public DocumentSigningPage()
    {
        InitializeComponent();
        SetupUI();
        SetupEventHandlers();
    }
    
    private void SetupUI()
    {
        signaturePad = new SfSignaturePad { HeightRequest = 300 };
        termsCheckBox = new CheckBox();
        saveButton = new Button { Text = "Save Signature", IsEnabled = false };
        statusLabel = new Label { Text = "Please sign above" };
        
        // Layout setup...
    }
    
    private void SetupEventHandlers()
    {
        signaturePad.DrawStarted += OnDrawStarted;
        signaturePad.DrawCompleted += OnDrawCompleted;
        saveButton.Clicked += OnSaveClicked;
        termsCheckBox.CheckedChanged += OnTermsChanged;
    }
    
    private void OnDrawStarted(object sender, CancelEventArgs e)
    {
        if (!termsCheckBox.IsChecked)
        {
            e.Cancel = true;
            DisplayAlert("Terms Required", 
                "Please accept the terms before signing", "OK");
        }
        else
        {
            statusLabel.Text = "Drawing signature...";
        }
    }
    
    private void OnDrawCompleted(object sender, EventArgs e)
    {
        statusLabel.Text = "Signature ready";
        saveButton.IsEnabled = true;
    }
    
    private async void OnSaveClicked(object sender, EventArgs e)
    {
        ImageSource? signature = signaturePad.ToImageSource();
        
        if (signature != null)
        {
            bool saved = await SaveSignatureAsync(signature);
            
            if (saved)
            {
                await DisplayAlert("Success", "Document signed successfully", "OK");
                await Navigation.PopAsync();
            }
        }
    }
    
    private void OnTermsChanged(object sender, CheckedChangedEventArgs e)
    {
        if (!e.Value)
        {
            signaturePad.Clear();
            saveButton.IsEnabled = false;
            statusLabel.Text = "Please accept terms to sign";
        }
    }
    
    private async Task<bool> SaveSignatureAsync(ImageSource signature)
    {
        // Save implementation
        return true;
    }
}
```

### Example 2: Signature with Retry Logic

```csharp
public class SignatureWithRetry
{
    private SfSignaturePad signaturePad;
    private int attemptCount = 0;
    private const int MaxAttempts = 3;
    
    public void Setup(SfSignaturePad pad)
    {
        signaturePad = pad;
        signaturePad.DrawCompleted += OnDrawCompleted;
    }
    
    private async void OnDrawCompleted(object sender, EventArgs e)
    {
        bool isValid = ValidateSignature();
        
        if (!isValid)
        {
            attemptCount++;
            
            if (attemptCount >= MaxAttempts)
            {
                await ShowAlert("Too many invalid attempts. Please contact support.");
                signaturePad.Clear();
                attemptCount = 0;
            }
            else
            {
                bool retry = await ShowRetryDialog(
                    $"Signature appears invalid. Attempt {attemptCount}/{MaxAttempts}. Try again?");
                
                if (retry)
                {
                    signaturePad.Clear();
                }
            }
        }
        else
        {
            await ShowAlert("Signature validated successfully!");
            attemptCount = 0;
        }
    }
    
    private bool ValidateSignature()
    {
        List<List<float>> points = signaturePad.GetSignaturePoints();
        return points.Count >= 2 && points.Sum(s => s.Count) >= 20;
    }
    
    private Task ShowAlert(string message)
    {
        // Display alert
        return Task.CompletedTask;
    }
    
    private Task<bool> ShowRetryDialog(string message)
    {
        // Show dialog and return user choice
        return Task.FromResult(true);
    }
}
```

## Best Practices

1. **Always check for null** when using `ToImageSource()` in event handlers
2. **Unsubscribe from events** when disposing components to prevent memory leaks
3. **Provide clear feedback** when cancelling draw actions with `e.Cancel = true`
4. **Use async/await** for I/O operations in event handlers
5. **Validate signatures** in `DrawCompleted` rather than blocking in `DrawStarted`
6. **Consider UX** when implementing validation - don't frustrate users with too many restrictions
7. **Log events** for debugging and analytics purposes
8. **Handle exceptions** gracefully in event handlers
