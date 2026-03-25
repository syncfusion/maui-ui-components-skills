# Liquid Glass Effect in StepProgressBar

## Overview

The Liquid Glass Effect introduces a modern, translucent design with adaptive color tinting and light refraction, creating a sleek, glass-like user experience. This feature is available in **.NET 10** and supports **macOS 26+** and **iOS 26+** platforms.

The glass effect provides:
- Translucent, frosted glass appearance
- Adaptive color tinting based on background
- Light refraction and blur effects
- Modern, premium UI aesthetics
- Clear, accessible visual hierarchy

## Platform and Version Requirements

**Critical Requirements:**
- **.NET Framework:** .NET 10 or higher
- **Platforms:** 
  - macOS 26 or higher
  - iOS 26 or higher
- **Control:** SfGlassEffectView from Syncfusion.Maui.Core

**Not Supported:**
- Android
- Windows
- Earlier versions of .NET, macOS, or iOS

## Setup

### Step 1: Install Required Package

The `SfGlassEffectView` is part of Syncfusion.Maui.Core, which is already a dependency of Syncfusion.Maui.ProgressBar.

If not installed:
```bash
dotnet add package Syncfusion.Maui.Core
```

### Step 2: Import Namespace

Add the Syncfusion.Maui.Core namespace:

**XAML:**
```xml
xmlns:core="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
```

**C#:**
```csharp
using Syncfusion.Maui.Core;
```

## Applying Liquid Glass Effect

### Basic Implementation

Wrap the StepProgressBar step content inside `SfGlassEffectView` using a custom `StepTemplate`.

**XAML:**
```xml
<Grid>
    <!-- Gradient background for glass effect to show through -->
    <Grid.Background>
        <LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
            <GradientStop Color="#0F4C75" Offset="0.0" />
            <GradientStop Color="#3282B8" Offset="0.5" />
            <GradientStop Color="#1B262C" Offset="1.0" />
        </LinearGradientBrush>
    </Grid.Background>
    
    <Grid.BindingContext>
        <local:ViewModel />
    </Grid.BindingContext>
    
    <stepProgressBar:SfStepProgressBar 
        x:Name="stepProgress"
        VerticalOptions="Center"
        HorizontalOptions="Center"
        Orientation="Horizontal"
        LabelSpacing="12"
        ActiveStepIndex="2"
        ActiveStepProgressValue="60"
        ProgressAnimationDuration="2500"
        ItemsSource="{Binding StepProgressItem}">
        
        <stepProgressBar:SfStepProgressBar.StepTemplate>
            <DataTemplate>
                <Grid>
                    <core:SfGlassEffectView 
                        WidthRequest="32"
                        HeightRequest="32"
                        CornerRadius="16"
                        Background="#007AFF">
                        
                        <Border Background="Transparent" Margin="0.5">
                            <Border.StrokeShape>
                                <RoundRectangle CornerRadius="15" />
                            </Border.StrokeShape>
                            
                            <Label 
                                Text="&#xe70c;"
                                FontFamily="MauiMaterialAssets"
                                HorizontalOptions="Center"
                                VerticalOptions="Center"
                                TextColor="White"
                                FontSize="20" />
                        </Border>
                        
                    </core:SfGlassEffectView>
                </Grid>
            </DataTemplate>
        </stepProgressBar:SfStepProgressBar.StepTemplate>
        
    </stepProgressBar:SfStepProgressBar>
</Grid>
```

### C# Implementation

```csharp
using Syncfusion.Maui.Core;
using Syncfusion.Maui.ProgressBar;

public partial class GlassEffectPage : ContentPage
{
    public GlassEffectPage()
    {
        InitializeComponent();
        
        // Create gradient background
        var gradient = new LinearGradientBrush
        {
            StartPoint = new Point(0, 0),
            EndPoint = new Point(0, 1)
        };
        gradient.GradientStops.Add(new GradientStop { Color = Color.FromArgb("#0F4C75"), Offset = 0.0f });
        gradient.GradientStops.Add(new GradientStop { Color = Color.FromArgb("#3282B8"), Offset = 0.5f });
        gradient.GradientStops.Add(new GradientStop { Color = Color.FromArgb("#1B262C"), Offset = 1.0f });
        
        var grid = new Grid { Background = gradient };
        
        ViewModel viewModel = new ViewModel();
        
        SfStepProgressBar stepProgressBar = new SfStepProgressBar()
        {
            VerticalOptions = LayoutOptions.Center,
            HorizontalOptions = LayoutOptions.Center,
            Orientation = StepProgressBarOrientation.Horizontal,
            LabelSpacing = 12,
            ActiveStepIndex = 2,
            ActiveStepProgressValue = 60,
            ProgressAnimationDuration = 2500,
            ItemsSource = viewModel.StepProgressItem,
        };
        
        var stepTemplate = new DataTemplate(() =>
        {
            var templateGrid = new Grid();
            
            var glassView = new SfGlassEffectView
            {
                WidthRequest = 32,
                HeightRequest = 32,
                CornerRadius = 16,
                Background = Color.FromArgb("#007AFF")
            };
            
            var border = new Border
            {
                Background = Colors.Transparent,
                Margin = new Thickness(0.5),
                StrokeShape = new RoundRectangle
                {
                    CornerRadius = new CornerRadius(15)
                }
            };
            
            var iconLabel = new Label
            {
                Text = "\ue70c",
                FontFamily = "MauiMaterialAssets",
                HorizontalOptions = LayoutOptions.Center,
                VerticalOptions = LayoutOptions.Center,
                TextColor = Colors.White,
                FontSize = 20
            };
            
            border.Content = iconLabel;
            glassView.Content = border;
            templateGrid.Add(glassView);
            
            return templateGrid;
        });
        
        stepProgressBar.StepTemplate = stepTemplate;
        grid.Children.Add(stepProgressBar);
        this.Content = grid;
    }
}
```

## SfGlassEffectView Properties

### HeightRequest

Controls the height of the glass container. Should be larger than the progress bar height to show the glass effect around it.

```xaml
<core:SfGlassEffectView HeightRequest="20">
    <stepProgressBar:SfStepProgressBar x:Name="stepProgress"
                                       ItemsSource="{Binding StepProgressItem}"/>
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
    <stepProgressBar:SfStepProgressBar x:Name="stepProgress"
                                       ItemsSource="{Binding StepProgressItem}"/>
</core:SfGlassEffectView>

<!-- Regular glass with color -->
<core:SfGlassEffectView EffectType="Regular">
    <stepProgressBar:SfStepProgressBar x:Name="stepProgress"
                                       ItemsSource="{Binding StepProgressItem}"/>
</core:SfGlassEffectView>
```

## Styling Tips

### Matching Glass Container to Progress Bar

```xaml
<core:SfGlassEffectView HeightRequest="24" 
                        CornerRadius="12">
    <stepProgressBar:SfStepProgressBar x:Name="stepProgress"
                                       ItemsSource="{Binding StepProgressItem}"/>
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
    <stepProgressBar:SfStepProgressBar x:Name="stepProgress"
                                       ItemsSource="{Binding StepProgressItem}"/>
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

## Complete Example with Different Step States

Create different glass effects for completed, in-progress, and not-started steps:

**ViewModel:**
```csharp
public class ViewModel
{
    public ObservableCollection<StepProgressBarItem> StepProgressItem { get; set; }
    
    public ViewModel()
    {
        StepProgressItem = new ObservableCollection<StepProgressBarItem>
        {
            new StepProgressBarItem() { PrimaryText = "Cart" },
            new StepProgressBarItem() { PrimaryText = "Address" },
            new StepProgressBarItem() { PrimaryText = "Delivery" },
            new StepProgressBarItem() { PrimaryText = "Ordered" }
        };
    }
}
```

**XAML with Conditional Colors (using converter or binding):**

For a simplified approach, use the same glass effect for all steps, with step status determining the base color automatically through StepSettings:

```xml
<stepProgressBar:SfStepProgressBar.CompletedStepSettings>
    <stepProgressBar:StepSettings 
        Background="#34C759"
        ContentFillColor="White"/>
</stepProgressBar:SfStepProgressBar.CompletedStepSettings>

<stepProgressBar:SfStepProgressBar.InProgressStepSettings>
    <stepProgressBar:StepSettings 
        Background="#FF9F0A"
        ContentFillColor="White"/>
</stepProgressBar:SfStepProgressBar.InProgressStepSettings>

<stepProgressBar:SfStepProgressBar.NotStartedStepSettings>
    <stepProgressBar:StepSettings 
        Background="#8E8E93"
        ContentFillColor="White"/>
</stepProgressBar:SfStepProgressBar.NotStartedStepSettings>
```

## Background Requirements

For the glass effect to be visible and effective:

### Requirement 1: Non-Solid Background

Place StepProgressBar over a gradient or image background:

**Gradient Background:**
```xml
<Grid.Background>
    <LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
        <GradientStop Color="#667eea" Offset="0.0" />
        <GradientStop Color="#764ba2" Offset="1.0" />
    </LinearGradientBrush>
</Grid.Background>
```

**Image Background:**
```xml
<Grid>
    <Image Source="background.jpg" Aspect="AspectFill"/>
    <stepProgressBar:SfStepProgressBar .../>
</Grid>
```

### Requirement 2: Transparent Inner Content

Set the Border or inner content `Background` to `Transparent` to allow the glass effect to show:

```xml
<core:SfGlassEffectView Background="#007AFF">
    <Border Background="Transparent">  <!-- Critical: Transparent -->
        <!-- Content -->
    </Border>
</core:SfGlassEffectView>
```

## Limitations and Notes

### Platform Limitation

Glass effect **only works on**:
- macOS 26+
- iOS 26+

On unsupported platforms, the control renders as a regular view without glass effects.

### Performance Considerations

Glass effects use GPU rendering and blur calculations:
- **Acceptable:** 5-10 glass elements on screen
- **Caution:** 10-20 glass elements may impact performance
- **Avoid:** 20+ glass elements simultaneously

For StepProgressBar, this typically isn't an issue (4-8 steps is common).

### Design Guidelines

**Do:**
- Use over gradient or image backgrounds
- Maintain adequate color contrast for content visibility
- Use subtle, semi-transparent base colors
- Test on actual devices

**Don't:**
- Use over solid white/black backgrounds (effect not visible)
- Overuse glass effects on every UI element
- Use excessively bright background colors (reduces accessibility)

## Troubleshooting

### Issue 1: Glass Effect Not Visible

**Problem:** Glass effect doesn't appear or looks like a solid color.

**Solutions:**
1. **Check platform:** Ensure running on macOS 26+ or iOS 26+
2. **Check .NET version:** Must be .NET 10+
3. **Verify background:** Must have gradient, image, or varied background (not solid)
4. **Set inner content transparent:** Border or content inside glass view should be `Background="Transparent"`

### Issue 2: Content Not Visible

**Problem:** Icon or text inside glass view is not visible.

**Solutions:**
1. **Increase contrast:** Use white text on dark glass, or dark text on light glass
2. **Adjust glass color:** Make base color lighter/darker
3. **Check z-index:** Ensure content is layered above glass view

### Issue 3: Performance Issues

**Problem:** Animation stutters or UI feels sluggish.

**Solutions:**
1. **Reduce glass elements:** Limit to 5-8 steps
2. **Simplify background:** Use simpler gradients instead of complex images
3. **Reduce animation:** Lower `ProgressAnimationDuration`

## Best Practices

### Practice 1: Test on Target Devices

Always test glass effects on actual iOS or macOS devices. Simulators may not accurately render the effect.

### Practice 2: Provide Fallbacks

The effect gracefully degrades on unsupported platforms, but test the appearance without glass effects:

```csharp
bool supportsGlass = DeviceInfo.Platform == DevicePlatform.iOS && 
                     DeviceInfo.Version.Major >= 26;

// Adjust design accordingly
```

### Practice 3: Maintain Accessibility

Ensure sufficient contrast between glass content and background:
- Text contrast: Minimum 4.5:1 (WCAG AA)
- Icon visibility: Clear at all background variations

### Practice 4: Use Semantic Colors

Match glass colors to step status:
- Completed: Green glass
- In Progress: Orange/Blue glass
- Not Started: Gray glass
- Error: Red glass

## Resources

- [Syncfusion .NET MAUI Core Documentation](https://help.syncfusion.com/maui/introduction/overview)
- [Apple Design Resources - Materials and Blur](https://developer.apple.com/design/resources/)