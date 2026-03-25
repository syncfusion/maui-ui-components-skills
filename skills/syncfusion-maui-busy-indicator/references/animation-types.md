# Animation Types

The Busy Indicator provides seven built-in animation types through the `AnimationType` property. Each animation offers a distinct visual style suitable for different design languages and use cases.

## Table of Contents
- [Overview](#overview)
- [CircularMaterial](#circularmaterial)
- [LinearMaterial](#linearmaterial)
- [Cupertino](#cupertino)
- [SingleCircle](#singlecircle)
- [DoubleCircle](#doublecircle)
- [Globe](#globe)
- [HorizontalPulsingBox](#horizontalpulsingbox)
- [Choosing the Right Animation](#choosing-the-right-animation)
- [Animation Performance](#animation-performance)

## Overview

The `AnimationType` property accepts values from the `AnimationType` enumeration. You can set it in XAML or C#:

**XAML:**
```xml
<core:SfBusyIndicator AnimationType="CircularMaterial" IsRunning="True" />
```

**C#:**
```csharp
var busyIndicator = new SfBusyIndicator
{
    AnimationType = AnimationType.CircularMaterial,
    IsRunning = true
};
```

## CircularMaterial

The **CircularMaterial** animation is inspired by Android's Material Design. It features a circular loading indicator with a smooth, continuous rotation.

**Visual Style:** Circular arc that rotates and changes length  
**Best For:** Android apps, Material Design interfaces, modern applications  
**Design Language:** Material Design (Google)

### Implementation

**XAML:**
```xml
<core:SfBusyIndicator x:Name="busyIndicator"
                      IsRunning="True"
                      AnimationType="CircularMaterial" />
```

**C#:**
```csharp
SfBusyIndicator busyIndicator = new SfBusyIndicator
{
    IsRunning = true,
    AnimationType = AnimationType.CircularMaterial
};
```

### Customization Example

```xml
<core:SfBusyIndicator AnimationType="CircularMaterial"
                      IsRunning="True"
                      IndicatorColor="#512BD4"
                      SizeFactor="0.6"
                      DurationFactor="0.4"
                      Title="Loading..."
                      TitlePlacement="Bottom" />
```

### Use Cases
- Android applications following Material Design
- Modern, clean interfaces
- Default choice when no specific design language required
- Login/authentication screens

## LinearMaterial

The **LinearMaterial** animation displays a horizontal progress bar with a moving segment, also inspired by Material Design.

**Visual Style:** Horizontal bar with moving segment  
**Best For:** Top-of-page loading indicators, web-like experiences  
**Design Language:** Material Design (Google)

### Implementation

**XAML:**
```xml
<core:SfBusyIndicator x:Name="busyIndicator"
                      IsRunning="True"
                      AnimationType="LinearMaterial" />
```

**C#:**
```csharp
SfBusyIndicator busyIndicator = new SfBusyIndicator
{
    IsRunning = true,
    AnimationType = AnimationType.LinearMaterial
};
```

### Positioned at Top of Page

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="*" />
    </Grid.RowDefinitions>
    
    <!-- Linear indicator at top -->
    <core:SfBusyIndicator Grid.Row="0"
                          AnimationType="LinearMaterial"
                          IsRunning="{Binding IsLoading}"
                          IndicatorColor="Blue"
                          HeightRequest="4" />
    
    <!-- Page content -->
    <ScrollView Grid.Row="1">
        <!-- Your content here -->
    </ScrollView>
</Grid>
```

### Use Cases
- Page load indicators
- Web-style progress feedback
- Minimal visual impact scenarios
- Horizontal space-constrained layouts

## Cupertino

The **Cupertino** animation mimics the iOS activity indicator with a rotating spoke design.

**Visual Style:** Rotating radial spokes  
**Best For:** iOS apps, Apple design language  
**Design Language:** Cupertino (Apple)

### Implementation

**XAML:**
```xml
<core:SfBusyIndicator x:Name="busyIndicator"
                      IsRunning="True"
                      AnimationType="Cupertino" />
```

**C#:**
```csharp
SfBusyIndicator busyIndicator = new SfBusyIndicator
{
    IsRunning = true,
    AnimationType = AnimationType.Cupertino
};
```

### iOS-Style Example

```xml
<core:SfBusyIndicator AnimationType="Cupertino"
                      IsRunning="True"
                      IndicatorColor="#007AFF"
                      SizeFactor="0.5"
                      Title="Loading..."
                      TextColor="#8E8E93" />
```

### Platform-Specific Implementation

```csharp
// Set animation based on platform
var busyIndicator = new SfBusyIndicator
{
    IsRunning = true,
    AnimationType = DeviceInfo.Platform == DevicePlatform.iOS 
        ? AnimationType.Cupertino 
        : AnimationType.CircularMaterial
};
```

### Use Cases
- iOS applications
- Cross-platform apps targeting iOS design
- Apple ecosystem consistency
- Native iOS look and feel

## SingleCircle

The **SingleCircle** animation displays a simple rotating circle.

**Visual Style:** Single solid circle rotating  
**Best For:** Minimalist designs, small spaces  
**Design Language:** Universal

### Implementation

**XAML:**
```xml
<core:SfBusyIndicator x:Name="busyIndicator"
                      IsRunning="True"
                      AnimationType="SingleCircle" />
```

**C#:**
```csharp
SfBusyIndicator busyIndicator = new SfBusyIndicator
{
    IsRunning = true,
    AnimationType = AnimationType.SingleCircle
};
```

### Compact Example

```xml
<core:SfBusyIndicator AnimationType="SingleCircle"
                      IsRunning="True"
                      IndicatorColor="Gray"
                      SizeFactor="0.3"
                      DurationFactor="0.6" />
```

### Use Cases
- Inline loading indicators
- Small UI elements
- Minimalist interfaces
- Low-distraction scenarios

## DoubleCircle

The **DoubleCircle** animation features two concentric circles rotating in opposite directions.

**Visual Style:** Two rotating circles (one inside the other)  
**Best For:** Emphasized loading states, visually engaging feedback  
**Design Language:** Universal

### Implementation

**XAML:**
```xml
<core:SfBusyIndicator x:Name="busyIndicator"
                      IsRunning="True"
                      AnimationType="DoubleCircle" />
```

**C#:**
```csharp
SfBusyIndicator busyIndicator = new SfBusyIndicator
{
    IsRunning = true,
    AnimationType = AnimationType.DoubleCircle
};
```

### Stylized Example

```xml
<core:SfBusyIndicator AnimationType="DoubleCircle"
                      IsRunning="True"
                      IndicatorColor="#E91E63"
                      SizeFactor="0.65"
                      DurationFactor="0.35" />
```

### Use Cases
- Splash screens
- Full-screen loading overlays
- Branding opportunities
- Visually prominent loading states

## Globe

The **Globe** animation displays a rotating 3D globe effect, providing a unique and eye-catching loading indicator.

**Visual Style:** 3D rotating globe  
**Best For:** Global/international apps, data sync, network operations  
**Design Language:** Universal

### Implementation

**XAML:**
```xml
<core:SfBusyIndicator AnimationType="Globe"
                      IsRunning="True" />
```

**C#:**
```csharp
SfBusyIndicator busyIndicator = new SfBusyIndicator
{
    AnimationType = AnimationType.Globe,
    IsRunning = true
};
```

### Network Operation Example

```xml
<core:SfBusyIndicator AnimationType="Globe"
                      IsRunning="{Binding IsSyncing}"
                      IndicatorColor="#4CAF50"
                      Title="Syncing data..."
                      TitlePlacement="Bottom"
                      SizeFactor="0.6" />
```

### Use Cases
- Cloud synchronization
- Network operations
- International/global app features
- Data fetching from remote servers
- Geographic/location-based features

## HorizontalPulsingBox

The **HorizontalPulsingBox** animation displays a series of horizontally arranged boxes that pulse in sequence.

**Visual Style:** Horizontal row of pulsing boxes  
**Best For:** Modern, playful interfaces  
**Design Language:** Universal

### Implementation

**XAML:**
```xml
<core:SfBusyIndicator AnimationType="HorizontalPulsingBox"
                      IsRunning="True" />
```

**C#:**
```csharp
SfBusyIndicator busyIndicator = new SfBusyIndicator
{
    AnimationType = AnimationType.HorizontalPulsingBox,
    IsRunning = true
};
```

### Modern UI Example

```xml
<core:SfBusyIndicator AnimationType="HorizontalPulsingBox"
                      IsRunning="True"
                      IndicatorColor="#FF9800"
                      SizeFactor="0.5"
                      DurationFactor="0.4"
                      Title="Processing..."
                      TitlePlacement="Bottom" />
```

### Use Cases
- Modern, trendy applications
- Game loading screens
- Media apps
- Creative/design tools
- Youth-oriented interfaces

## Choosing the Right Animation

Consider these factors when selecting an animation type:

### Platform Consistency

| Platform | Recommended Animation | Alternative |
|----------|---------------------|-------------|
| Android | CircularMaterial | LinearMaterial |
| iOS | Cupertino | CircularMaterial |
| Windows | CircularMaterial | DoubleCircle |
| Cross-platform | CircularMaterial | Globe |

### Performance Context

| Scenario | Best Animation | Reason |
|----------|---------------|--------|
| Background task | CircularMaterial | Familiar, unintrusive |
| Network sync | Globe | Visual metaphor |
| File processing | LinearMaterial | Progress-like appearance |
| Quick operation | SingleCircle | Minimal distraction |
| Splash screen | DoubleCircle, Globe | Engaging, branded |

### Design Language

- **Material Design apps** → CircularMaterial, LinearMaterial
- **iOS/Cupertino apps** → Cupertino
- **Minimalist designs** → SingleCircle
- **Playful/modern apps** → HorizontalPulsingBox
- **Global/network apps** → Globe
- **Branded experiences** → DoubleCircle, Globe

## Animation Performance

All animations are hardware-accelerated and performant. However, consider these tips:

### Best Practices

1. **Use appropriate duration** - Faster animations (lower DurationFactor) feel more responsive
2. **Consider context** - Match animation complexity to task duration
3. **Avoid multiple indicators** - One active indicator per screen
4. **Test on target devices** - Verify smooth performance on lower-end devices

### Performance Comparison

**Lowest overhead:** SingleCircle, Cupertino  
**Medium overhead:** CircularMaterial, LinearMaterial, DoubleCircle  
**Highest overhead:** Globe, HorizontalPulsingBox

**Note:** All animations are optimized and perform well on modern devices. Overhead differences are minimal and typically not noticeable.

## Switching Animations Dynamically

You can change animation types at runtime:

```csharp
public void ChangeAnimationBasedOnTask(string taskType)
{
    busyIndicator.AnimationType = taskType switch
    {
        "network" => AnimationType.Globe,
        "processing" => AnimationType.CircularMaterial,
        "loading" => AnimationType.LinearMaterial,
        _ => AnimationType.CircularMaterial
    };
    
    busyIndicator.IsRunning = true;
}
```

## Edge Cases and Considerations

### Very Small Sizes
When `SizeFactor` is very small (<0.3), complex animations like Globe may be less visible. Consider SingleCircle or Cupertino for small indicators.

### Very Fast Animations
When `DurationFactor` is very low (<0.2), some animations may appear jittery. Test thoroughly on target devices.

### Color Contrast
Ensure sufficient contrast between `IndicatorColor` and background, especially for LinearMaterial on light backgrounds.
