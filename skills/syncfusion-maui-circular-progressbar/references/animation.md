# Animation in Circular ProgressBar

The Syncfusion .NET MAUI Circular ProgressBar provides rich animation support to visualize progress value changes in an interactive and engaging way.

## Table of Contents
- [Animation Overview](#animation-overview)
- [Determinate Animation](#determinate-animation)
- [Indeterminate Animation](#indeterminate-animation)
- [Easing Effects](#easing-effects)
- [SetProgress Method](#setprogress-method)
- [Animation Best Practices](#animation-best-practices)

## Animation Overview

The circular progress bar supports two types of animations:
1. **Determinate Animation**: Smooth transition when progress value changes
2. **Indeterminate Animation**: Continuous loop animation for unknown progress

## Determinate Animation

### AnimationDuration Property

Represents the animation duration (in milliseconds) for the determinate state's progress indicator.

**XAML:**
```xml
<progressBar:SfCircularProgressBar Progress="75" 
                                   AnimationDuration="2000" />
```

**C#:**
```csharp
SfCircularProgressBar circularProgressBar = new SfCircularProgressBar
{
    Progress = 75,
    AnimationDuration = 2000 // 2 seconds
};
```

### Default Duration

- **Default**: 1000 milliseconds (1 second)
- **Range**: 0 to any positive integer
- **0 ms**: Instant update, no animation

### Duration Guidelines

| Duration | Use Case | User Experience |
|----------|----------|-----------------|
| 0 ms | Instant updates, frequent changes | No animation, immediate |
| 200-500 ms | Quick feedback, rapid updates | Very fast, snappy |
| 500-1000 ms | Standard progress updates | Smooth, balanced |
| 1000-2000 ms | Slow, dramatic changes | Emphasized, dramatic |
| 2000+ ms | Very slow, special effects | Slow, for emphasis only |

## Indeterminate Animation

### IndeterminateAnimationDuration Property

Represents the animation duration for the indeterminate state's indicator (full loop duration).

**XAML:**
```xml
<progressBar:SfCircularProgressBar IsIndeterminate="True" 
                                   IndeterminateAnimationDuration="1500" />
```

**C#:**
```csharp
SfCircularProgressBar circularProgressBar = new SfCircularProgressBar
{
    IsIndeterminate = true,
    IndeterminateAnimationDuration = 1500 // 1.5 seconds per loop
};
```

### Indeterminate Duration Guidelines

- **Fast (800-1200 ms)**: Quick loading, short waits
- **Standard (1500-2000 ms)**: Normal loading, balanced
- **Slow (2500+ ms)**: Calm, relaxed loading experience

## Easing Effects

Easing functions control the animation speed curve, making animations more natural and engaging.

### AnimationEasing Property

Specifies the transfer function that controls animation speed for determinate state.

**XAML:**
```xml
<progressBar:SfCircularProgressBar Progress="75"
                                   AnimationEasing="{x:Static Easing.CubicInOut}" />
```

**C#:**
```csharp
SfCircularProgressBar circularProgressBar = new SfCircularProgressBar
{
    Progress = 75,
    AnimationEasing = Easing.CubicInOut
};
```

### Available Easing Functions

#### Linear (Default)
Constant speed throughout animation.

```xml
<progressBar:SfCircularProgressBar Progress="75"
                                   AnimationEasing="{x:Static Easing.Linear}" />
```

#### CubicInOut
Slow start, fast middle, slow end. Most popular for smooth transitions.

```xml
<progressBar:SfCircularProgressBar Progress="75"
                                   AnimationEasing="{x:Static Easing.CubicInOut}" />
```

#### CubicIn
Slow start, accelerates to the end.

```xml
<progressBar:SfCircularProgressBar Progress="75"
                                   AnimationEasing="{x:Static Easing.CubicIn}" />
```

#### CubicOut
Fast start, decelerates to the end.

```xml
<progressBar:SfCircularProgressBar Progress="75"
                                   AnimationEasing="{x:Static Easing.CubicOut}" />
```

#### BounceIn
Bouncing effect at the start.

```xml
<progressBar:SfCircularProgressBar Progress="75"
                                   AnimationEasing="{x:Static Easing.BounceIn}" />
```

#### BounceOut
Bouncing effect at the end.

```xml
<progressBar:SfCircularProgressBar Progress="75"
                                   AnimationEasing="{x:Static Easing.BounceOut}" />
```

#### SpringIn
Spring-like acceleration.

```xml
<progressBar:SfCircularProgressBar Progress="75"
                                   AnimationEasing="{x:Static Easing.SpringIn}" />
```

#### SpringOut
Spring-like deceleration.

```xml
<progressBar:SfCircularProgressBar Progress="75"
                                   AnimationEasing="{x:Static Easing.SpringOut}" />
```

### Easing Function Recommendations

| Easing | Best For | Feel |
|--------|----------|------|
| **Linear** | Technical displays, constant speed | Mechanical, predictable |
| **CubicInOut** | General progress, smooth updates | Natural, balanced |
| **CubicIn** | Starting processes | Gradual acceleration |
| **CubicOut** | Completing processes | Satisfying completion |
| **BounceIn/Out** | Playful apps, games | Fun, energetic |
| **SpringIn/Out** | Dynamic feedback | Elastic, responsive |

### Easing in C#

```csharp
private void ApplyEasing(string easingType)
{
    progressBar.AnimationEasing = easingType switch
    {
        "Linear" => Easing.Linear,
        "CubicIn" => Easing.CubicIn,
        "CubicOut" => Easing.CubicOut,
        "CubicInOut" => Easing.CubicInOut,
        "BounceIn" => Easing.BounceIn,
        "BounceOut" => Easing.BounceOut,
        "SpringIn" => Easing.SpringIn,
        "SpringOut" => Easing.SpringOut,
        _ => Easing.Linear
    };
}
```

## IndeterminateAnimationEasing Property

Specifies easing function for indeterminate state animation.

**XAML:**
```xml
<progressBar:SfCircularProgressBar IsIndeterminate="True" 
                                   IndeterminateAnimationEasing="{x:Static Easing.BounceIn}" />
```

**C#:**
```csharp
SfCircularProgressBar circularProgressBar = new SfCircularProgressBar
{
    IsIndeterminate = true,
    IndeterminateAnimationEasing = Easing.BounceIn
};
```

### Indeterminate Easing Recommendations

- **Linear**: Smooth, constant rotation
- **CubicInOut**: Gentle acceleration/deceleration per loop
- **BounceIn/Out**: Playful, attention-grabbing

## SetProgress Method

Dynamically set progress with custom animation duration and easing for a specific update.

### Method Signature

```csharp
void SetProgress(double progress, double? animationDuration = null, Easing? easing = null)
```

### Parameters

- **progress**: Target progress value
- **animationDuration**: Optional duration in milliseconds (overrides AnimationDuration property)
- **easing**: Optional easing function (overrides AnimationEasing property)

### Basic Usage

```csharp
// Use default animation settings
circularProgressBar.SetProgress(75);

// Custom duration (2 seconds)
circularProgressBar.SetProgress(75, 2000);

// Custom duration and easing
circularProgressBar.SetProgress(75, 1500, Easing.BounceOut);
```

### Important Note

The animation duration and easing parameters in `SetProgress()` do NOT affect the `AnimationDuration` and `AnimationEasing` properties. They only apply to this specific method call.

### Practical Examples

**Instant Update (No Animation):**
```csharp
circularProgressBar.SetProgress(50, 0); // Jump to 50% immediately
```

**Slow Dramatic Update:**
```csharp
circularProgressBar.SetProgress(100, 3000, Easing.CubicOut); // 3 second completion animation
```

**Quick Bounce Update:**
```csharp
circularProgressBar.SetProgress(75, 500, Easing.BounceOut); // Fast bounce to 75%
```

### Multiple Sequential Updates

```csharp

public class MultiStepCircularPage : ContentPage
{
    private SfCircularProgressBar progressBar;
    private Label statusLabel;

    public MultiStepCircularPage()
    {
        progressBar = new SfCircularProgressBar
        {
            Progress = 0,
            TrackThickness = 10,
            ProgressThickness = 10,
            ProgressFill = Color.FromArgb("#4CAF50"),
            TrackFill = Color.FromArgb("#E0E0E0"),
            WidthRequest = 150,
            HeightRequest = 150,
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center
        };

        statusLabel = new Label
        {
            Text = "Starting...",
            HorizontalOptions = LayoutOptions.Center,
            Margin = new Thickness(0, 20, 0, 0)
        };

        var startButton = new Button
        {
            Text = "Start Sequential Animation",
            HorizontalOptions = LayoutOptions.Center
        };
        startButton.Clicked += async (s, e) => await AnimateSequentialProgress();

        Content = new StackLayout
        {
            Padding = 40,
            Spacing = 20,
            Children = { progressBar, statusLabel, startButton }
        };
    }

    private async Task AnimateSequentialProgress()
    {
        statusLabel.Text = "Jumping to 25%...";
        progressBar.SetProgress(25, 500, Easing.CubicIn);
        await Task.Delay(600);

        statusLabel.Text = "Moving to 50%...";
        progressBar.SetProgress(50, 2000, Easing.Linear);
        await Task.Delay(2100);

        statusLabel.Text = "Bouncing to 75%...";
        progressBar.SetProgress(75, 800, Easing.BounceOut);
        await Task.Delay(900);

        statusLabel.Text = "Completing...";
        progressBar.SetProgress(100, 1500, Easing.CubicOut);

        statusLabel.Text = "Done!";
    }
}
```

## Complete Animation Examples

### Example 1: Smooth Progress Update with Animation

```xml
<StackLayout Padding="20" Spacing="20">
    <progressBar:SfCircularProgressBar x:Name="animatedProgressBar"
                                       Progress="0"
                                       AnimationDuration="1000"
                                       AnimationEasing="{x:Static Easing.CubicInOut}"
                                       ProgressFill="#FF2196F3"
                                       TrackFill="#332196F3" />
    
    <Button Text="Animate to 75%" 
            Clicked="AnimateButton_Clicked" />
</StackLayout>
```

```csharp
private void AnimateButton_Clicked(object sender, EventArgs e)
{
    animatedProgressBar.Progress = 75; // Animates with configured settings
}
```

### Example 2: Multi-Stage Process with Different Animations

```csharp
public class MultiStageCircularPage : ContentPage
{
    private SfCircularProgressBar progressBar;
    private Label statusLabel;

    public MultiStageCircularPage()
    {
        progressBar = new SfCircularProgressBar
        {
            Progress = 0,
            TrackThickness = 10,
            ProgressThickness = 10,
            ProgressFill = Color.FromArgb("#4CAF50"),
            TrackFill = Color.FromArgb("#E0E0E0"),
            WidthRequest = 150,
            HeightRequest = 150,
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center
        };

        statusLabel = new Label
        {
            Text = "Idle...",
            HorizontalOptions = LayoutOptions.Center,
            Margin = new Thickness(0, 20, 0, 0)
        };

        var startButton = new Button
        {
            Text = "Run Multi-Stage Animation",
            HorizontalOptions = LayoutOptions.Center
        };
        startButton.Clicked += async (s, e) => await MultiStageAnimation();

        Content = new StackLayout
        {
            Padding = 40,
            Spacing = 20,
            Children = { progressBar, statusLabel, startButton }
        };
    }

    private async Task MultiStageAnimation()
    {
        // Stage 1: Quick start (fast, bouncy)
        statusLabel.Text = "Starting...";
        progressBar.SetProgress(20, 600, Easing.BounceOut);
        await Task.Delay(700);

        // Stage 2: Processing (smooth, linear)
        statusLabel.Text = "Processing...";
        progressBar.SetProgress(70, 3000, Easing.Linear);
        await Task.Delay(3100);

        // Stage 3: Finalizing (slow, satisfying)
        statusLabel.Text = "Finalizing...";
        progressBar.SetProgress(100, 2000, Easing.CubicOut);
        await Task.Delay(2100);

        statusLabel.Text = "Complete!";
    }
}

```

### Example 3: Indeterminate with Custom Easing

```xml
<progressBar:SfCircularProgressBar IsIndeterminate="True"
                                   IndeterminateAnimationDuration="1200"
                                   IndeterminateAnimationEasing="{x:Static Easing.CubicInOut}"
                                   ProgressFill="#FFFF9800"
                                   TrackFill="#33FF9800" />
```

### Example 4: Toggling Animation

```csharp

public class ToggleAnimationPage : ContentPage
{
    private SfCircularProgressBar progressBar;
    private Label statusLabel;
    private bool isAnimationEnabled = true;

    public ToggleAnimationPage()
    {
        progressBar = new SfCircularProgressBar
        {
            Progress = 50,
            TrackThickness = 10,
            ProgressThickness = 10,
            ProgressFill = Color.FromArgb("#4CAF50"),
            TrackFill = Color.FromArgb("#E0E0E0"),
            WidthRequest = 150,
            HeightRequest = 150,
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center,
            AnimationDuration = 1000,
            AnimationEasing = Easing.CubicInOut
        };

        statusLabel = new Label
        {
            Text = "Animation Enabled",
            HorizontalOptions = LayoutOptions.Center,
            Margin = new Thickness(0, 20, 0, 0)
        };

        var toggleButton = new Button
        {
            Text = "Toggle Animation",
            HorizontalOptions = LayoutOptions.Center
        };
        toggleButton.Clicked += (s, e) => ToggleAnimation();

        var updateButton = new Button
        {
            Text = "Update Progress",
            HorizontalOptions = LayoutOptions.Center
        };
        updateButton.Clicked += (s, e) =>
        {
            // Randomly update progress to demonstrate animation effect
            var random = new Random();
            int newProgress = random.Next(0, 101);
            progressBar.SetProgress(newProgress, progressBar.AnimationDuration, progressBar.AnimationEasing);
            statusLabel.Text = $"Progress: {newProgress}%";
        };

        Content = new StackLayout
        {
            Padding = 40,
            Spacing = 20,
            Children = { progressBar, statusLabel, toggleButton, updateButton }
        };
    }

    private void ToggleAnimation()
    {
        if (isAnimationEnabled)
        {
            // Disable animation
            progressBar.AnimationDuration = 0;
            statusLabel.Text = "Animation Disabled";
            isAnimationEnabled = false;
        }
        else
        {
            // Enable animation
            progressBar.AnimationDuration = 1000;
            progressBar.AnimationEasing = Easing.CubicInOut;
            statusLabel.Text = "Animation Enabled";
            isAnimationEnabled = true;
        }
    }
}


```

### Example 6: Progress Bar with Dynamic Easing Picker

```xml
<StackLayout Padding="20" Spacing="20">
    <Picker x:Name="easingPicker"
            Title="Select Easing"
            SelectedIndexChanged="EasingPicker_SelectedIndexChanged">
        <Picker.Items>
            <x:String>Linear</x:String>
            <x:String>CubicIn</x:String>
            <x:String>CubicOut</x:String>
            <x:String>CubicInOut</x:String>
            <x:String>BounceIn</x:String>
            <x:String>BounceOut</x:String>
        </Picker.Items>
    </Picker>
    
    <progressBar:SfCircularProgressBar x:Name="dynamicProgressBar"
                                       Progress="0"
                                       AnimationDuration="1500" />
    
    <Button Text="Animate" Clicked="Animate_Clicked" />
</StackLayout>
```

```csharp
private void EasingPicker_SelectedIndexChanged(object sender, EventArgs e)
{
    var picker = (Picker)sender;
    var easingName = picker.SelectedItem?.ToString();
    
    dynamicProgressBar.AnimationEasing = easingName switch
    {
        "Linear" => Easing.Linear,
        "CubicIn" => Easing.CubicIn,
        "CubicOut" => Easing.CubicOut,
        "CubicInOut" => Easing.CubicInOut,
        "BounceIn" => Easing.BounceIn,
        "BounceOut" => Easing.BounceOut,
        _ => Easing.Linear
    };
}

private void Animate_Clicked(object sender, EventArgs e)
{
    // Reset and animate
    dynamicProgressBar.Progress = 0;
    Device.StartTimer(TimeSpan.FromMilliseconds(100), () =>
    {
        dynamicProgressBar.Progress = 75;
        return false;
    });
}
```

## Animation Best Practices

### Duration Selection

1. **Quick Feedback (200-500ms)**: Use for frequent updates, real-time progress
2. **Standard Updates (500-1500ms)**: Use for normal progress transitions
3. **Dramatic Effects (1500-3000ms)**: Use for milestone completions, final updates
4. **Disable (0ms)**: Use for very frequent updates (>10 per second)

### Easing Selection

1. **Linear**: Technical displays, constant data updates
2. **CubicInOut**: General purpose, feels most natural
3. **CubicOut**: Completion animations, satisfying finish
4. **BounceOut**: Achievements, success feedback
5. **SpringOut**: Dynamic, responsive interactions

### Performance Tips

1. **Avoid very short durations (<100ms)**: May cause performance issues
2. **Use Linear for real-time data**: Less computation than complex easing
3. **Disable animation for rapid updates**: Set duration to 0 for high-frequency updates
4. **Test on target devices**: Animation performance varies by device

### UX Guidelines

1. **Match animation to context**: Fast for quick tasks, slow for important milestones
2. **Be consistent**: Use similar durations and easing throughout your app
3. **Don't over-animate**: Too much motion can be distracting
4. **Provide feedback**: Animation confirms that something is happening
5. **Consider accessibility**: Some users may prefer reduced motion

### Common Patterns

**Pattern 1: Real-Time Updates**
```csharp
// High-frequency updates - disable animation
progressBar.AnimationDuration = 0;
progressBar.Progress = currentValue;
```

**Pattern 2: Milestone Completion**
```csharp
// Dramatic completion animation
progressBar.SetProgress(100, 2000, Easing.BounceOut);
```

**Pattern 3: Smooth Continuous Progress**
```csharp
// Standard smooth animation
progressBar.AnimationDuration = 1000;
progressBar.AnimationEasing = Easing.CubicInOut;
```

## Summary

Animation properties in the circular progress bar:
- **AnimationDuration**: Duration for determinate state (default: 1000ms)
- **IndeterminateAnimationDuration**: Duration for indeterminate loop
- **AnimationEasing**: Easing function for determinate animation
- **IndeterminateAnimationEasing**: Easing function for indeterminate animation
- **SetProgress()**: Method for custom one-time animation settings

Choose animation duration and easing based on your use case to create engaging, professional progress indicators.