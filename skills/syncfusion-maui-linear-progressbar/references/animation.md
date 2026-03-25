# Animation

## Table of Contents
- [Animation Duration](#animation-duration)
- [Easing Effects](#easing-effects)
- [SetProgress Method](#setprogress-method)
- [Indeterminate Animation](#indeterminate-animation)
- [Animation Best Practices](#animation-best-practices)

The Linear ProgressBar provides comprehensive animation support to visualize progress value changes in an interactive and engaging way.

## Animation Duration

Control how long animations take using duration properties measured in milliseconds.

### Animation Properties

| Property | Applies To | Default | Description |
|----------|-----------|---------|-------------|
| **AnimationDuration** | Determinate state progress | 2000ms | Duration for primary progress animation |
| **SecondaryAnimationDuration** | Buffer state secondary progress | 2000ms | Duration for secondary progress animation |
| **IndeterminateAnimationDuration** | Indeterminate state | 2000ms | Duration for indeterminate animation cycle |

### Basic Animation Duration

```xaml
<progressBar:SfLinearProgressBar Progress="75" 
                                 AnimationDuration="1000"/>
```

```csharp
var progressBar = new SfLinearProgressBar
{
    Progress = 75,
    AnimationDuration = 1000  // 1 second
};
```

### Quick vs Slow Animations

```csharp
// Fast animation (500ms)
var fastProgress = new SfLinearProgressBar
{
    Progress = 80,
    AnimationDuration = 500
};

// Slow animation (3000ms)
var slowProgress = new SfLinearProgressBar
{
    Progress = 80,
    AnimationDuration = 3000
};
```

### Secondary Progress Animation

```xaml
<progressBar:SfLinearProgressBar Progress="25" 
                                 SecondaryProgress="75"
                                 AnimationDuration="800"
                                 SecondaryAnimationDuration="1200"/>
```

```csharp
var bufferProgress = new SfLinearProgressBar
{
    Progress = 25,
    SecondaryProgress = 75,
    AnimationDuration = 800,         // Primary animates in 800ms
    SecondaryAnimationDuration = 1200 // Secondary animates in 1200ms
};
```

### Indeterminate Animation Duration

```xaml
<progressBar:SfLinearProgressBar IsIndeterminate="True"
                                 IndeterminateAnimationDuration="1500"/>
```

```csharp
var loadingIndicator = new SfLinearProgressBar
{
    IsIndeterminate = true,
    IndeterminateAnimationDuration = 1500  // Each cycle takes 1.5 seconds
};
```

## Easing Effects

Easing functions control the speed curve of animations, making them feel more natural and polished.

### AnimationEasing Property

The `AnimationEasing` property accepts .NET MAUI's built-in easing functions:

```xaml
<progressBar:SfLinearProgressBar Progress="75" 
                                 AnimationEasing="{x:Static Easing.CubicInOut}"/>
```

```csharp
var progressBar = new SfLinearProgressBar
{
    Progress = 75,
    AnimationEasing = Easing.CubicInOut
};
```

### Available Easing Functions

| Easing Function | Behavior | Best For |
|----------------|----------|----------|
| **Linear** | Constant speed | Simple, utilitarian progress |
| **SinIn** | Slow start, accelerates | Subtle entrance |
| **SinOut** | Fast start, decelerates | Natural completion feel |
| **SinInOut** | Slow start and end | Smooth, balanced animation |
| **CubicIn** | Very slow start | Dramatic entrance |
| **CubicOut** | Very fast start | Quick, settling finish |
| **CubicInOut** | Smooth acceleration and deceleration | Professional, polished feel |
| **BounceIn** | Bounces at end | Playful, attention-grabbing |
| **BounceOut** | Bounces at start | Energetic entrance |
| **SpringIn** | Spring-like entrance | Dynamic, lively |
| **SpringOut** | Spring-like exit | Elastic, responsive feel |

### Easing Examples

#### Linear (Default Behavior)

```csharp
var progressBar = new SfLinearProgressBar
{
    Progress = 75,
    AnimationEasing = Easing.Linear  // Constant speed
};
```

#### CubicInOut (Recommended for Professional Look)

```csharp
var progressBar = new SfLinearProgressBar
{
    Progress = 75,
    AnimationEasing = Easing.CubicInOut,  // Smooth acceleration/deceleration
    AnimationDuration = 800
};
```

#### BounceOut (Playful Effect)

```csharp
var progressBar = new SfLinearProgressBar
{
    Progress = 75,
    AnimationEasing = Easing.BounceOut,  // Bounces when it reaches target
    AnimationDuration = 1200
};
```

#### SpringOut (Elastic Feel)

```csharp
var progressBar = new SfLinearProgressBar
{
    Progress = 75,
    AnimationEasing = Easing.SpringOut,  // Spring-like motion
    AnimationDuration = 1000
};
```

### Choosing the Right Easing

**For professional, business applications:**
```csharp
AnimationEasing = Easing.CubicInOut  // or Easing.SinInOut
```

**For loading indicators:**
```csharp
AnimationEasing = Easing.CubicOut  // Quick initial movement, settles smoothly
```

**For game health bars:**
```csharp
AnimationEasing = Easing.Linear  // Direct, immediate feedback
```

**For celebration/achievement progress:**
```csharp
AnimationEasing = Easing.BounceOut  // Fun, energetic
```

## SetProgress Method

The `SetProgress()` method allows you to update progress with one-time custom animation settings without affecting the control's default animation configuration.

### Method Signature

```csharp
void SetProgress(double progress, double? animationDuration = null, Easing? easing = null)
```

### Parameters

- **progress**: The target progress value
- **animationDuration**: Optional animation duration in milliseconds (only for this call)
- **easing**: Optional easing function (only for this call)

### Basic Usage

```csharp
// Use default animation settings
progressBar.SetProgress(75);

// Custom duration for this update
progressBar.SetProgress(75, animationDuration: 1500);

// Custom easing for this update
progressBar.SetProgress(75, easing: Easing.BounceOut);

// Custom duration AND easing
progressBar.SetProgress(75, animationDuration: 1200, easing: Easing.CubicOut);
```

### Important Notes

The parameters passed to `SetProgress()` **do NOT** change the control's `AnimationDuration` or `AnimationEasing` properties. They only apply to that specific call.

```csharp
var progressBar = new SfLinearProgressBar
{
    Progress = 0,
    AnimationDuration = 1000,
    AnimationEasing = Easing.Linear
};

// This call uses BounceOut easing for 2 seconds
progressBar.SetProgress(50, animationDuration: 2000, easing: Easing.BounceOut);

// Next call still uses the default: Linear easing, 1000ms
progressBar.SetProgress(75);
```

### Practical Examples

#### Milestone Celebrations

```csharp
public void UpdateProgress(double value)
{
    if (value >= 100)
    {
        // Special animation for completion
        progressBar.SetProgress(100, animationDuration: 1500, easing: Easing.BounceOut);
    }
    else if (value >= 50)
    {
        // Faster animation for mid-point
        progressBar.SetProgress(value, animationDuration: 600, easing: Easing.CubicOut);
    }
    else
    {
        // Normal animation
        progressBar.SetProgress(value);
    }
}
```

#### Stepped Progress with Varying Speed

```csharp
private async Task AnimateProgressSteps()
{
    // Quick start
    progressBar.SetProgress(25, animationDuration: 500, easing: Easing.CubicOut);
    await Task.Delay(1000);

    // Moderate middle
    progressBar.SetProgress(50, animationDuration: 1000, easing: Easing.Linear);
    await Task.Delay(1500);

    // Slow finish
    progressBar.SetProgress(100, animationDuration: 2000, easing: Easing.CubicInOut);
}
```

#### User-Triggered Quick Update

```csharp
private void OnSkipButtonClicked(object sender, EventArgs e)
{
    // Instant jump to end with quick animation
    progressBar.SetProgress(100, animationDuration: 300, easing: Easing.Linear);
}
```

## Indeterminate Animation

Customize the animation behavior when the progress bar is in indeterminate state.

### IndeterminateAnimationEasing Property

```xaml
<progressBar:SfLinearProgressBar IsIndeterminate="True" 
                                 IndeterminateAnimationEasing="{x:Static Easing.BounceIn}"/>
```

```csharp
var loadingBar = new SfLinearProgressBar
{
    IsIndeterminate = true,
    IndeterminateAnimationEasing = Easing.BounceIn
};
```

### Indeterminate Animation Examples

#### Smooth Linear (Default-like)

```csharp
var loadingBar = new SfLinearProgressBar
{
    IsIndeterminate = true,
    IndeterminateAnimationDuration = 2000,
    IndeterminateAnimationEasing = Easing.Linear
};
```

#### Bouncy Loading

```csharp
var loadingBar = new SfLinearProgressBar
{
    IsIndeterminate = true,
    IndeterminateAnimationDuration = 1500,
    IndeterminateAnimationEasing = Easing.BounceIn
};
```

#### Fast and Smooth

```csharp
var loadingBar = new SfLinearProgressBar
{
    IsIndeterminate = true,
    IndeterminateAnimationDuration = 1000,
    IndeterminateAnimationEasing = Easing.CubicInOut
};
```

### Combining Determinate and Indeterminate Animations

```csharp
var progressBar = new SfLinearProgressBar
{
    // Determinate animation settings
    AnimationDuration = 800,
    AnimationEasing = Easing.CubicOut,
    
    // Indeterminate animation settings
    IndeterminateAnimationDuration = 1500,
    IndeterminateAnimationEasing = Easing.SinInOut
};

// Start with indeterminate
progressBar.IsIndeterminate = true;

// Later switch to determinate
progressBar.IsIndeterminate = false;
progressBar.Progress = 0;
// Will use determinate animation settings from here on
```

## Animation Best Practices

### 1. Match Animation Speed to Context

```csharp
// Quick feedback for user actions
var userActionProgress = new SfLinearProgressBar
{
    AnimationDuration = 300,  // Fast
    AnimationEasing = Easing.CubicOut
};

// Smooth for background tasks
var backgroundTaskProgress = new SfLinearProgressBar
{
    AnimationDuration = 1000,  // Moderate
    AnimationEasing = Easing.CubicInOut
};

// Dramatic for achievements
var achievementProgress = new SfLinearProgressBar
{
    AnimationDuration = 2000,  // Slow
    AnimationEasing = Easing.BounceOut
};
```

### 2. Consistent Easing Across Your App

Create a style class for consistency:

```csharp
public static class AppProgressStyles
{
    public const int StandardDuration = 800;
    public static readonly Easing StandardEasing = Easing.CubicInOut;

    public static void ApplyStandardAnimation(SfLinearProgressBar progressBar)
    {
        progressBar.AnimationDuration = StandardDuration;
        progressBar.AnimationEasing = StandardEasing;
    }
}

// Usage
var progressBar = new SfLinearProgressBar();
AppProgressStyles.ApplyStandardAnimation(progressBar);
```

### 3. Disable Animation When Needed

For rapid successive updates, consider reducing or disabling animation:

```csharp
// High-frequency updates (like video scrubbing)
progressBar.AnimationDuration = 0;  // Instant updates

// Or very short duration
progressBar.AnimationDuration = 50;  // Minimal animation
```

### 4. Performance Considerations

```csharp
// Good: Reasonable duration
AnimationDuration = 1000  // 1 second

// Avoid: Very long animations
AnimationDuration = 10000  // 10 seconds - too slow for most use cases

// Avoid: Extremely short animations
AnimationDuration = 10  // May cause visual jitter
```

### 5. Testing Animations

Test your animations on actual devices:

```csharp
// What looks good in emulator may feel different on device
// Test on:
// - Low-end Android devices
// - Various iOS devices
// - Windows desktop vs tablet
```

## Complete Animation Examples

### Professional Dashboard Progress

```csharp
var dashboardProgress = new SfLinearProgressBar
{
    Progress = 0,
    TrackHeight = 8,
    ProgressHeight = 8,
    TrackCornerRadius = 4,
    ProgressCornerRadius = 4,
    AnimationDuration = 800,
    AnimationEasing = Easing.CubicInOut,
    ProgressFill = Color.FromArgb("#2196F3"),
    TrackFill = Color.FromArgb("#E3F2FD")
};
```

### Game Loading Screen

```csharp
var gameLoading = new SfLinearProgressBar
{
    Progress = 0,
    TrackHeight = 12,
    ProgressHeight = 12,
    TrackCornerRadius = 6,
    ProgressCornerRadius = 6,
    AnimationDuration = 600,
    AnimationEasing = Easing.Linear,  // Consistent for loading
    ProgressFill = Color.FromArgb("#00E676"),
    TrackFill = Color.FromArgb("#1B5E20")
};
```

### Achievement Unlock

```csharp
var achievementBar = new SfLinearProgressBar
{
    Progress = 0,
    TrackHeight = 16,
    ProgressHeight = 16,
    TrackCornerRadius = 8,
    ProgressCornerRadius = 8,
    AnimationDuration = 2000,
    AnimationEasing = Easing.SpringOut,  // Bouncy, celebratory
    ProgressFill = Color.FromArgb("#FFD700"),
    TrackFill = Color.FromArgb("#424242")
};

// Trigger achievement
achievementBar.SetProgress(100, animationDuration: 2500, easing: Easing.BounceOut);
```

### Video Buffer Indicator

```csharp
var videoBuffer = new SfLinearProgressBar
{
    Progress = 0,
    SecondaryProgress = 0,
    TrackHeight = 4,
    ProgressHeight = 4,
    SecondaryProgressHeight = 4,
    AnimationDuration = 200,  // Quick updates
    SecondaryAnimationDuration = 500,
    AnimationEasing = Easing.Linear,
    ProgressFill = Color.FromArgb("#FF4081"),
    SecondaryProgressFill = Color.FromArgb("#F48FB1"),
    TrackFill = Color.FromArgb("#424242")
};
```

## Common Animation Mistakes

❌ **Animation too slow for frequent updates**
```csharp
// WRONG: 3-second animation with updates every 100ms
AnimationDuration = 3000  // Animation can't keep up
```

✅ **Match duration to update frequency**
```csharp
// RIGHT: Short animation for frequent updates
AnimationDuration = 200  // Smooth even with rapid updates
```

❌ **Using bounce on serious UI**
```csharp
// WRONG: Bouncy animation in financial app
var bankingProgress = new SfLinearProgressBar
{
    AnimationEasing = Easing.BounceOut  // Too playful
};
```

✅ **Professional easing for business apps**
```csharp
// RIGHT: Smooth, professional animation
var bankingProgress = new SfLinearProgressBar
{
    AnimationEasing = Easing.CubicInOut  // Professional
};
```