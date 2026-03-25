# Customization

The Busy Indicator provides extensive customization options for colors, sizes, animation speed, and overlay backgrounds. This guide covers all customization properties and techniques.

## Table of Contents
- [Indicator Color](#indicator-color)
- [Overlay Background](#overlay-background)
- [Gradient Backgrounds](#gradient-backgrounds)
- [Animation Duration](#animation-duration)
- [Indicator Sizing](#indicator-sizing)
- [Complete Styling Examples](#complete-styling-examples)
- [Design Patterns](#design-patterns)

## Indicator Color

The `IndicatorColor` property controls the color of the animated indicator itself.

### Basic Color Customization

**XAML:**
```xml
<core:SfBusyIndicator IsRunning="True"
                      AnimationType="CircularMaterial"
                      IndicatorColor="Red" />
```

**C#:**
```csharp
SfBusyIndicator busyIndicator = new SfBusyIndicator
{
    IsRunning = true,
    AnimationType = AnimationType.CircularMaterial,
    IndicatorColor = Colors.Red
};
```

### Using Hex Colors

**XAML:**
```xml
<core:SfBusyIndicator IsRunning="True"
                      IndicatorColor="#512BD4" />
```

**C#:**
```csharp
busyIndicator.IndicatorColor = Color.FromArgb("#512BD4");
```

### Brand Color Example

```xml
<core:SfBusyIndicator IsRunning="True"
                      AnimationType="CircularMaterial"
                      IndicatorColor="#FF6B35"
                      Title="Loading..."
                      TitlePlacement="Bottom" />
```

### Dynamic Color Changes

```csharp
public void SetIndicatorColorByStatus(string status)
{
    busyIndicator.IndicatorColor = status switch
    {
        "success" => Colors.Green,
        "warning" => Colors.Orange,
        "error" => Colors.Red,
        _ => Color.FromArgb("#512BD4") // Default brand color
    };
}
```

### Platform-Specific Colors

```csharp
// Match platform accent colors
public void ApplyPlatformIndicatorColor()
{
    if (DeviceInfo.Platform == DevicePlatform.iOS)
    {
        busyIndicator.IndicatorColor = Color.FromArgb("#007AFF");
    }
    else if (DeviceInfo.Platform == DevicePlatform.Android)
    {
        busyIndicator.IndicatorColor = Color.FromArgb("#6200EE");
    }
    else
    {
        busyIndicator.IndicatorColor = Color.FromArgb("#0078D4");
    }
}
```

## Overlay Background

The `OverlayFill` property sets the background color behind the indicator, creating an overlay effect that can block user interaction.

### Solid Color Overlay

**XAML:**
```xml
<core:SfBusyIndicator IsRunning="True"
                      AnimationType="CircularMaterial"
                      IndicatorColor="White"
                      TextColor="White"
                      Title="Searching..."
                      OverlayFill="#512BD4" />
```

**C#:**
```csharp
SfBusyIndicator busyIndicator = new SfBusyIndicator
{
    IsRunning = true,
    AnimationType = AnimationType.CircularMaterial,
    Title = "Searching...",
    TextColor = Colors.White,
    IndicatorColor = Colors.White,
    OverlayFill = Color.FromArgb("#512BD4")
};
```

### Semi-Transparent Overlay

A semi-transparent overlay allows users to see the underlying content while preventing interaction:

**XAML:**
```xml
<core:SfBusyIndicator IsRunning="True"
                      IndicatorColor="White"
                      Title="Processing..."
                      TextColor="White"
                      OverlayFill="#88000000" />
```

The `#88000000` color format:
- `88` = Alpha channel (transparency: 00=transparent, FF=opaque)
- `000000` = RGB color (black)

### Common Overlay Colors

```xml
<!-- Light semi-transparent white -->
<core:SfBusyIndicator OverlayFill="#88FFFFFF" IndicatorColor="Black" />

<!-- Medium dark overlay -->
<core:SfBusyIndicator OverlayFill="#AA000000" IndicatorColor="White" />

<!-- Branded semi-transparent overlay -->
<core:SfBusyIndicator OverlayFill="#CC512BD4" IndicatorColor="White" />

<!-- No overlay (transparent) -->
<core:SfBusyIndicator OverlayFill="Transparent" />
```

### Modal vs Non-Modal Overlays

```csharp
// Modal overlay (blocks interaction)
public void ShowModalLoading()
{
    busyIndicator.OverlayFill = Color.FromArgb("#CC000000");
    busyIndicator.IsRunning = true;
}

// Non-modal indicator (no overlay)
public void ShowNonModalLoading()
{
    busyIndicator.OverlayFill = Colors.Transparent;
    busyIndicator.IsRunning = true;
}
```

## Gradient Backgrounds

The `OverlayFill` property accepts `Brush` types, allowing for sophisticated gradient backgrounds.

### Linear Gradient

**XAML:**
```xml
<core:SfBusyIndicator IsRunning="True"
                      AnimationType="CircularMaterial"
                      IndicatorColor="White"
                      Title="Loading...">
    <core:SfBusyIndicator.OverlayFill>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="#512BD4" Offset="0.0" />
            <GradientStop Color="#8B5CF6" Offset="1.0" />
        </LinearGradientBrush>
    </core:SfBusyIndicator.OverlayFill>
</core:SfBusyIndicator>
```

**C#:**
```csharp
var busyIndicator = new SfBusyIndicator
{
    IsRunning = true,
    AnimationType = AnimationType.CircularMaterial,
    IndicatorColor = Colors.White,
    Title = "Loading...",
    OverlayFill = new LinearGradientBrush
    {
        StartPoint = new Point(0, 0),
        EndPoint = new Point(1, 1),
        GradientStops = new GradientStopCollection
        {
            new GradientStop { Color = Color.FromArgb("#512BD4"), Offset = 0.0f },
            new GradientStop { Color = Color.FromArgb("#8B5CF6"), Offset = 1.0f }
        }
    }
};
```

### Radial Gradient

**XAML:**
```xml
<core:SfBusyIndicator IsRunning="True"
                      AnimationType="CircularMaterial"
                      IndicatorColor="#e64c93"
                      Title="Searching...">
    <core:SfBusyIndicator.OverlayFill>
        <RadialGradientBrush>
            <GradientStop Color="#44e64c93" Offset="0.1" />
            <GradientStop Color="#AA9d40db" Offset="1.0" />
        </RadialGradientBrush>
    </core:SfBusyIndicator.OverlayFill>
</core:SfBusyIndicator>
```

**C#:**
```csharp
busyIndicator.OverlayFill = new RadialGradientBrush
{
    GradientStops = new GradientStopCollection
    {
        new GradientStop { Color = Color.FromArgb("#44e64c93"), Offset = 0.1f },
        new GradientStop { Color = Color.FromArgb("#AA9d40db"), Offset = 1.0f }
    }
};
```

### Multi-Color Gradient

```xml
<core:SfBusyIndicator IsRunning="True">
    <core:SfBusyIndicator.OverlayFill>
        <LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
            <GradientStop Color="#FF6B35" Offset="0.0" />
            <GradientStop Color="#F7931E" Offset="0.33" />
            <GradientStop Color="#FDC830" Offset="0.66" />
            <GradientStop Color="#F37335" Offset="1.0" />
        </LinearGradientBrush>
    </core:SfBusyIndicator.OverlayFill>
</core:SfBusyIndicator>
```

## Animation Duration

The `DurationFactor` property controls the speed of the animation. It accepts values from 0.0 to 1.0, where:
- **Lower values** = Faster animation
- **Higher values** = Slower animation
- **Default** = 0.5

### Basic Duration Control

**XAML:**
```xml
<core:SfBusyIndicator IsRunning="True"
                      AnimationType="CircularMaterial"
                      DurationFactor="0.2" />
```

**C#:**
```csharp
SfBusyIndicator busyIndicator = new SfBusyIndicator
{
    IsRunning = true,
    AnimationType = AnimationType.CircularMaterial,
    DurationFactor = 0.2 // Fast animation
};
```

### Duration Examples

```xml
<!-- Very fast (urgent operations) -->
<core:SfBusyIndicator DurationFactor="0.1" />

<!-- Fast (quick operations) -->
<core:SfBusyIndicator DurationFactor="0.3" />

<!-- Default (balanced) -->
<core:SfBusyIndicator DurationFactor="0.5" />

<!-- Slow (long operations) -->
<core:SfBusyIndicator DurationFactor="0.7" />

<!-- Very slow (background tasks) -->
<core:SfBusyIndicator DurationFactor="0.9" />
```

### Context-Based Duration

```csharp
public void SetDurationByContext(string operationType)
{
    busyIndicator.DurationFactor = operationType switch
    {
        "quick-search" => 0.2,      // Fast for responsive feel
        "network-request" => 0.4,   // Medium-fast
        "file-processing" => 0.6,   // Medium-slow
        "data-sync" => 0.8,         // Slow for long operations
        _ => 0.5
    };
}
```

### Performance Considerations

- **Fast animations (< 0.3):** May appear jittery on lower-end devices
- **Slow animations (> 0.7):** May feel unresponsive
- **Recommended range:** 0.3 to 0.6 for most scenarios

## Indicator Sizing

The `SizeFactor` property controls the size of the indicator relative to its container. It accepts values from 0.0 to 1.0.

### Basic Size Control

**XAML:**
```xml
<core:SfBusyIndicator IsRunning="True"
                      AnimationType="CircularMaterial"
                      Title="Loading..."
                      SizeFactor="0.7" />
```

**C#:**
```csharp
SfBusyIndicator busyIndicator = new SfBusyIndicator
{
    IsRunning = true,
    AnimationType = AnimationType.CircularMaterial,
    Title = "Loading...",
    SizeFactor = 0.7
};
```

### Size Examples

```xml
<!-- Very small (inline indicators) -->
<core:SfBusyIndicator SizeFactor="0.2" />

<!-- Small (compact UI) -->
<core:SfBusyIndicator SizeFactor="0.4" />

<!-- Default (balanced) -->
<core:SfBusyIndicator SizeFactor="0.5" />

<!-- Large (prominent display) -->
<core:SfBusyIndicator SizeFactor="0.7" />

<!-- Very large (splash screens) -->
<core:SfBusyIndicator SizeFactor="0.9" />
```

### Responsive Sizing

```csharp
public void AdaptSizeToScreen()
{
    var screenWidth = DeviceDisplay.MainDisplayInfo.Width;
    
    busyIndicator.SizeFactor = screenWidth switch
    {
        < 600 => 0.6,   // Phone
        < 900 => 0.5,   // Small tablet
        < 1200 => 0.4,  // Tablet
        _ => 0.3        // Desktop
    };
}
```

### Size by Context

```xml
<!-- Full-screen loading -->
<core:SfBusyIndicator SizeFactor="0.8" />

<!-- Modal dialog -->
<core:SfBusyIndicator SizeFactor="0.6" />

<!-- Inline list loading -->
<core:SfBusyIndicator SizeFactor="0.3" />

<!-- Button loading indicator -->
<core:SfBusyIndicator SizeFactor="0.2" />
```

## Complete Styling Examples

### Example 1: Branded Full-Screen Loader

```xml
<core:SfBusyIndicator IsRunning="True"
                      AnimationType="Globe"
                      IndicatorColor="White"
                      SizeFactor="0.7"
                      DurationFactor="0.4"
                      Title="Loading Your Experience..."
                      TitlePlacement="Bottom"
                      TextColor="White"
                      FontSize="18"
                      FontAttributes="Bold">
    <core:SfBusyIndicator.OverlayFill>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="#512BD4" Offset="0.0" />
            <GradientStop Color="#8B5CF6" Offset="1.0" />
        </LinearGradientBrush>
    </core:SfBusyIndicator.OverlayFill>
</core:SfBusyIndicator>
```

### Example 2: Minimal Inline Loader

```xml
<core:SfBusyIndicator IsRunning="{Binding IsLoading}"
                      AnimationType="SingleCircle"
                      IndicatorColor="Gray"
                      SizeFactor="0.25"
                      DurationFactor="0.3"
                      OverlayFill="Transparent" />
```

### Example 3: iOS-Style Modal Loader

```xml
<core:SfBusyIndicator IsRunning="True"
                      AnimationType="Cupertino"
                      IndicatorColor="#007AFF"
                      SizeFactor="0.5"
                      DurationFactor="0.5"
                      Title="Loading..."
                      TextColor="#8E8E93"
                      OverlayFill="#CC000000" />
```

### Example 4: Material Design Loader

```xml
<core:SfBusyIndicator IsRunning="True"
                      AnimationType="CircularMaterial"
                      IndicatorColor="#6200EE"
                      SizeFactor="0.6"
                      DurationFactor="0.4"
                      Title="Please wait"
                      TitlePlacement="Bottom"
                      TextColor="#6200EE"
                      OverlayFill="#88FFFFFF" />
```

## Design Patterns

### Pattern 1: Login Screen Overlay

```xml
<Grid>
    <!-- Login form -->
    <StackLayout VerticalOptions="Center" Padding="20">
        <Entry Placeholder="Email" />
        <Entry Placeholder="Password" IsPassword="True" />
        <Button Text="Sign In" />
    </StackLayout>
    
    <!-- Loading overlay -->
    <core:SfBusyIndicator x:Name="loginLoader"
                          IsRunning="False"
                          AnimationType="CircularMaterial"
                          IndicatorColor="White"
                          Title="Signing in..."
                          TextColor="White"
                          SizeFactor="0.6"
                          OverlayFill="#DD000000" />
</Grid>
```

### Pattern 2: Page Refresh Indicator

```xml
<!-- Top of page loader -->
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="*" />
    </Grid.RowDefinitions>
    
    <core:SfBusyIndicator Grid.Row="0"
                          IsRunning="{Binding IsRefreshing}"
                          AnimationType="LinearMaterial"
                          IndicatorColor="#512BD4"
                          OverlayFill="Transparent"
                          HeightRequest="4" />
    
    <ScrollView Grid.Row="1">
        <!-- Content -->
    </ScrollView>
</Grid>
```

### Pattern 3: Card Loading State

```xml
<Frame Padding="15" HasShadow="True">
    <Grid>
        <!-- Card content -->
        <StackLayout>
            <Label Text="Data will appear here" />
        </StackLayout>
        
        <!-- Card loader -->
        <core:SfBusyIndicator IsRunning="{Binding IsLoadingCard}"
                              AnimationType="CircularMaterial"
                              SizeFactor="0.4"
                              IndicatorColor="Gray"
                              OverlayFill="#EEEEEEEE" />
    </Grid>
</Frame>
```

## Best Practices

### Color Contrast
Ensure sufficient contrast between indicator and overlay:
```csharp
// Good contrast combinations
IndicatorColor = "White", OverlayFill = "#AA000000"  // ✓
IndicatorColor = "#512BD4", OverlayFill = "#DDFFFFFF"     // ✓

// Poor contrast
IndicatorColor = "#CCCCCC", OverlayFill = "#DDCCCCCC"     // ✗
```

### Size Guidelines
- **Inline/small:** 0.2 - 0.3
- **Standard:** 0.4 - 0.6
- **Prominent:** 0.7 - 0.9

### Duration Guidelines
- **Quick tasks (< 2s):** 0.2 - 0.3
- **Normal tasks (2-5s):** 0.4 - 0.5
- **Long tasks (> 5s):** 0.6 - 0.8

### Overlay Opacity
- **Fully block interaction:** 0.8 - 1.0 alpha (AA - FF)
- **Partial visibility:** 0.5 - 0.7 alpha (88 - CC)
- **Minimal obstruction:** 0.2 - 0.4 alpha (33 - 66)

## Edge Cases

### Very Fast Animations
When `DurationFactor < 0.2`, animations may appear choppy. Test on target devices.

### Very Small Sizes
When `SizeFactor < 0.3`, complex animations (Globe, DoubleCircle) may be hard to see. Use simpler animations (SingleCircle, Cupertino).

### Transparent Overlays
When `OverlayFill="Transparent"`, users can interact with underlying UI. Ensure this is intentional.

### Dark Mode Compatibility
```csharp
public void UpdateForDarkMode(bool isDarkMode)
{
    if (isDarkMode)
    {
        busyIndicator.IndicatorColor = Colors.White;
        busyIndicator.TextColor = Colors.White;
        busyIndicator.OverlayFill = Color.FromArgb("#AA000000");
    }
    else
    {
        busyIndicator.IndicatorColor = Color.FromArgb("#512BD4");
        busyIndicator.TextColor = Color.FromArgb("#333333");
        busyIndicator.OverlayFill = Color.FromArgb("#CCFFFFFF");
    }
}
```
