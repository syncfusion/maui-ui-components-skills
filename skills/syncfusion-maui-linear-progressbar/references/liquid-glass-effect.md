# Liquid Glass Effect

The Liquid Glass Effect introduces a modern, translucent design with adaptive color tinting and light refraction, creating a sleek, glass-like user experience that remains clear and accessible.

## Overview

The Liquid Glass Effect provides:
- **Modern translucent design**: Frosted glass appearance
- **Adaptive color tinting**: Background colors influence the glass effect
- **Light refraction**: Creates depth and visual interest
- **Accessibility**: Maintains clarity and readability

## Platform Requirements

**IMPORTANT**: This feature has specific platform and version requirements:

| Requirement | Minimum Version |
|-------------|----------------|
| **macOS** | 26 or higher |
| **iOS** | 26 or higher |
| **.NET** | .NET 10 |
| **Windows/Android** | Not currently supported |

## Implementation Steps

### Step 1: Wrap Control in SfGlassEffectView

To apply the Liquid Glass Effect, wrap the Linear ProgressBar inside the `SfGlassEffectView` component from `Syncfusion.Maui.Core`.

#### Import Namespace

```xaml
xmlns:core="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
xmlns:progressBar="clr-namespace:Syncfusion.Maui.ProgressBar;assembly=Syncfusion.Maui.ProgressBar"
```

```csharp
using Syncfusion.Maui.Core;
using Syncfusion.Maui.ProgressBar;
```

### Step 2: Set Background to Transparent

For the glass effect to work properly, set the progress bar's `Background` property to `Transparent`. The background then acts as a tinted color layer.

### Complete Implementation

#### XAML Example

```xaml
<Grid>
    <!-- Gradient background for glass effect -->
    <Grid.Background>
        <LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
            <GradientStop Color="#0F4C75" Offset="0.0" />
            <GradientStop Color="#3282B8" Offset="0.5" />
            <GradientStop Color="#1B262C" Offset="1.0" />
        </LinearGradientBrush>
    </Grid.Background>

    <StackLayout Padding="20" Spacing="20">
        <!-- Glass effect wrapper -->
        <core:SfGlassEffectView HeightRequest="18"
                                CornerRadius="9"
                                EffectType="Clear">
            <!-- Linear progress bar with transparent background -->
            <progressBar:SfLinearProgressBar x:Name="linearProgressBar"
                                             TrackHeight="8"
                                             ProgressHeight="8"
                                             ProgressCornerRadius="8"
                                             TrackCornerRadius="8"
                                             Progress="75"
                                             Margin="5"
                                             BackgroundColor="Transparent" />
        </core:SfGlassEffectView>
    </StackLayout>
</Grid>
```

#### C# Example

```csharp
// Create gradient background
var gradientBrush = new LinearGradientBrush
{
    StartPoint = new Point(0, 0),
    EndPoint = new Point(0, 1),
    GradientStops = new GradientStopCollection
    {
        new GradientStop { Color = Color.FromArgb("#0F4C75"), Offset = 0.0f },
        new GradientStop { Color = Color.FromArgb("#3282B8"), Offset = 0.5f },
        new GradientStop { Color = Color.FromArgb("#1B262C"), Offset = 1.0f }
    }
};

var grid = new Grid
{
    Background = gradientBrush
};

var stackLayout = new StackLayout
{
    Padding = 20,
    Spacing = 20
};

// Create glass effect view
var glassView = new SfGlassEffectView
{
    HeightRequest = 18,
    CornerRadius = 9,
    EffectType = LiquidGlassEffectType.Clear
};

// Create progress bar with transparent background
var linearProgressBar = new SfLinearProgressBar
{
    TrackHeight = 8,
    ProgressHeight = 8,
    ProgressCornerRadius = 8,
    TrackCornerRadius = 8,
    Progress = 75,
    Margin = new Thickness(5),
    BackgroundColor = Colors.Transparent  // CRITICAL for glass effect
};

glassView.Content = linearProgressBar;
stackLayout.Children.Add(glassView);
grid.Children.Add(stackLayout);

this.Content = grid;
```

## SfGlassEffectView Properties

### HeightRequest

Controls the height of the glass container. Should be larger than the progress bar height to show the glass effect around it.

```xaml
<core:SfGlassEffectView HeightRequest="20">
    <progressBar:SfLinearProgressBar TrackHeight="10" ProgressHeight="10"/>
</core:SfGlassEffectView>
```

### CornerRadius

Rounds the corners of the glass container:

```xaml
<core:SfGlassEffectView CornerRadius="10">
    <!-- Progress bar content -->
</core:SfGlassEffectView>
```

### EffectType

Controls the type of glass effect:

```csharp
public enum LiquidGlassEffectType
{
    Clear,   
    Regular
}
```

**Example:**

```xaml
<!-- Clear frosted glass -->
<core:SfGlassEffectView EffectType="Clear">
    <progressBar:SfLinearProgressBar ... />
</core:SfGlassEffectView>

<!-- Regular glass with color -->
<core:SfGlassEffectView EffectType="Regular">
    <progressBar:SfLinearProgressBar ... />
</core:SfGlassEffectView>
```

## Styling Tips

### Matching Glass Container to Progress Bar

```xaml
<core:SfGlassEffectView HeightRequest="24" 
                        CornerRadius="12">
    <progressBar:SfLinearProgressBar TrackHeight="16"
                                     ProgressHeight="16"
                                     TrackCornerRadius="8"
                                     ProgressCornerRadius="8"
                                     Margin="4"
                                     BackgroundColor="Transparent"/>
</core:SfGlassEffectView>
```

**Formula:**
- `GlassView.HeightRequest = ProgressBar.Height + (Margin * 2)`
- `GlassView.CornerRadius = ProgressBar.CornerRadius + Margin`

### Creating Depth with Padding/Margin

```xaml
<core:SfGlassEffectView HeightRequest="30" 
                        CornerRadius="15"
                        Padding="5">
    <progressBar:SfLinearProgressBar TrackHeight="12"
                                     ProgressHeight="12"
                                     TrackCornerRadius="6"
                                     ProgressCornerRadius="6"
                                     BackgroundColor="Transparent"/>
</core:SfGlassEffectView>
```

### Vibrant Background Colors

The glass effect works best with vibrant, contrasting background colors:

```xaml
<Grid.Background>
    <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
        <!-- Purple to pink gradient -->
        <GradientStop Color="#667eea" Offset="0.0" />
        <GradientStop Color="#764ba2" Offset="0.5" />
        <GradientStop Color="#f093fb" Offset="1.0" />
    </LinearGradientBrush>
</Grid.Background>
```

## Complete Examples

### Modern Dashboard Loading

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:core="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
             xmlns:progressBar="clr-namespace:Syncfusion.Maui.ProgressBar;assembly=Syncfusion.Maui.ProgressBar">
    
    <Grid>
        <Grid.Background>
            <LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
                <GradientStop Color="#1e3c72" Offset="0.0" />
                <GradientStop Color="#2a5298" Offset="0.5" />
                <GradientStop Color="#1e3c72" Offset="1.0" />
            </LinearGradientBrush>
        </Grid.Background>

        <VerticalStackLayout Padding="40" Spacing="30" VerticalOptions="Center">
            <Label Text="Loading Dashboard" 
                   FontSize="24" 
                   TextColor="White"
                   HorizontalOptions="Center"/>

            <core:SfGlassEffectView HeightRequest="20"
                                    CornerRadius="10"
                                    EffectType="Clear">
                <progressBar:SfLinearProgressBar Progress="65"
                                                 TrackHeight="12"
                                                 ProgressHeight="12"
                                                 TrackCornerRadius="6"
                                                 ProgressCornerRadius="6"
                                                 Margin="4"
                                                 BackgroundColor="Transparent"
                                                 AnimationDuration="1000"/>
            </core:SfGlassEffectView>

            <Label Text="65% Complete" 
                   FontSize="16" 
                   TextColor="White"
                   Opacity="0.8"
                   HorizontalOptions="Center"/>
        </VerticalStackLayout>
    </Grid>
</ContentPage>
```

### Multiple Progress Bars with Glass Effect

```csharp
public class GlassProgressDashboard : ContentPage
{
    public GlassProgressDashboard()
    {
        var gradient = new LinearGradientBrush
        {
            StartPoint = new Point(0, 0),
            EndPoint = new Point(1, 1),
            GradientStops = new GradientStopCollection
            {
                new GradientStop { Color = Color.FromArgb("#FF6B6B"), Offset = 0.0f },
                new GradientStop { Color = Color.FromArgb("#4ECDC4"), Offset = 0.5f },
                new GradientStop { Color = Color.FromArgb("#556270"), Offset = 1.0f }
            }
        };

        var stack = new StackLayout
        {
            Padding = 30,
            Spacing = 25
        };

        // Create multiple glass progress bars
        stack.Children.Add(CreateGlassProgress("CPU Usage", 75));
        stack.Children.Add(CreateGlassProgress("Memory", 60));
        stack.Children.Add(CreateGlassProgress("Disk I/O", 45));
        stack.Children.Add(CreateGlassProgress("Network", 90));

        Content = new Grid
        {
            Background = gradient,
            Children = { stack }
        };
    }

    private View CreateGlassProgress(string label, double progress)
    {
        var container = new StackLayout { Spacing = 8 };

        // Label
        container.Children.Add(new Label
        {
            Text = label,
            TextColor = Colors.White,
            FontSize = 16,
            FontAttributes = FontAttributes.Bold
        });

        // Glass progress
        var glassView = new SfGlassEffectView
        {
            HeightRequest = 22,
            CornerRadius = 11,
            EffectType = LiquidGlassEffectType.Clear
        };

        var progressBar = new SfLinearProgressBar
        {
            Progress = progress,
            TrackHeight = 14,
            ProgressHeight = 14,
            TrackCornerRadius = 7,
            ProgressCornerRadius = 7,
            Margin = new Thickness(4),
            BackgroundColor = Colors.Transparent,
            AnimationDuration = 1200,
            AnimationEasing = Easing.CubicInOut
        };

        glassView.Content = progressBar;
        container.Children.Add(glassView);

        // Percentage
        container.Children.Add(new Label
        {
            Text = $"{progress}%",
            TextColor = Colors.White,
            FontSize = 14,
            Opacity = 0.7,
            HorizontalOptions = LayoutOptions.End
        });

        return container;
    }
}
```

### Animated Glass Loading

```csharp
public class AnimatedGlassLoading : ContentPage
{
    private SfLinearProgressBar progressBar;

    public AnimatedGlassLoading()
    {
        var gradient = new LinearGradientBrush
        {
            StartPoint = new Point(0, 0),
            EndPoint = new Point(0, 1),
            GradientStops = new GradientStopCollection
            {
                new GradientStop { Color = Color.FromArgb("#0F4C75"), Offset = 0.0f },
                new GradientStop { Color = Color.FromArgb("#3282B8"), Offset = 0.5f },
                new GradientStop { Color = Color.FromArgb("#1B262C"), Offset = 1.0f }
            }
        };

        var glassView = new SfGlassEffectView
        {
            HeightRequest = 20,
            CornerRadius = 10,
            EffectType = LiquidGlassEffectType.Clear
        };

        progressBar = new SfLinearProgressBar
        {
            Progress = 0,
            TrackHeight = 12,
            ProgressHeight = 12,
            TrackCornerRadius = 6,
            ProgressCornerRadius = 6,
            Margin = new Thickness(4),
            BackgroundColor = Colors.Transparent,
            AnimationDuration = 800,
            AnimationEasing = Easing.CubicOut
        };

        glassView.Content = progressBar;

        Content = new Grid
        {
            Background = gradient,
            Children =
            {
                new StackLayout
                {
                    Padding = 40,
                    Spacing = 20,
                    VerticalOptions = LayoutOptions.Center,
                    Children = { glassView }
                }
            }
        };

        // Start animation
        AnimateProgress();
    }

    private async void AnimateProgress()
    {
        for (int i = 0; i <= 100; i += 10)
        {
            progressBar.Progress = i;
            await Task.Delay(500);
         }
   }
}
```

## Best Practices

1. **Transparent background required**: Always set `BackgroundColor="Transparent"` on the progress bar
2. **Vibrant backgrounds work best**: Use gradient or colorful backgrounds to showcase the glass effect
3. **Proper sizing**: Glass container should be larger than progress bar (add margin/padding)
4. **Corner radius coordination**: Match glass view corner radius to progress bar design
5. **Platform detection**: Implement fallback for unsupported platforms
6. **.NET 10 required**: Verify project targets .NET 10 or higher

## Common Issues

❌ **Progress bar not transparent**
```xaml
<!-- WRONG: Opaque background hides glass effect -->
<progressBar:SfLinearProgressBar BackgroundColor="White"/>
```

✅ **Correct transparent background**
```xaml
<!-- RIGHT: Transparent shows glass effect -->
<progressBar:SfLinearProgressBar BackgroundColor="Transparent"/>
```

❌ **Glass view too small**
```xaml
<!-- WRONG: No room for glass effect -->
<core:SfGlassEffectView HeightRequest="12">
    <progressBar:SfLinearProgressBar TrackHeight="12"/>
</core:SfGlassEffectView>
```

✅ **Adequate space for effect**
```xaml
<!-- RIGHT: Enough space around progress bar -->
<core:SfGlassEffectView HeightRequest="20">
    <progressBar:SfLinearProgressBar TrackHeight="12" Margin="4"/>
</core:SfGlassEffectView>
```

## Additional Resources

- **Liquid Glass Getting Started Documentation**: Refer to Syncfusion's official documentation for `SfGlassEffectView`
- **Platform Requirements**: Verify your target platforms support the required versions
- **.NET 10 Migration**: Ensure your project is updated to .NET 10