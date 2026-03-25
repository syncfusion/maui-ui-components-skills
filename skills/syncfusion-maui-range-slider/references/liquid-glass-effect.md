# Liquid Glass Effect in .NET MAUI Range Slider

## Overview

The Liquid Glass Effect introduces a modern, translucent design with adaptive color tinting and light refraction to the .NET MAUI Range Slider (`SfRangeSlider`). This effect creates a sleek, glass-like user experience that provides responsive visual feedback during thumb interaction while maintaining clarity and accessibility.

This reference covers the setup, requirements, and best practices for implementing the Liquid Glass Effect in your Range Slider.

## Table of Contents
- [Overview](#overview)
- [Requirements](#requirements)
- [Core Property](#core-property)
  - [EnableLiquidGlassEffect](#enableliquidglasseffect)
- [Setup and Configuration](#setup-and-configuration)
  - [Basic Setup](#basic-setup)
  - [With Background Images](#with-background-images)
- [Visual Interaction Feedback](#visual-interaction-feedback)
- [Common Scenarios](#common-scenarios)
- [Best Practices](#best-practices)
- [Related References](#related-references)

## Requirements

**Platform Requirements:**
- **.NET 10** or higher
- **iOS 26** (for iOS devices)
- **macOS 26** (for macOS devices)

**Important:** The Liquid Glass Effect is only supported on .NET 10 with iOS 26 and macOS 26. It will not function on earlier versions or other platforms.

## Core Property

### EnableLiquidGlassEffect

The `EnableLiquidGlassEffect` property enables or disables the Liquid Glass Effect on the Range Slider.

**Type:** `bool`  
**Default:** `false`

**When enabled (`true`):**
- Thumbs display translucent glass effect during interaction
- Adaptive color tinting based on background
- Light refraction effects on press/drag
- Smooth, engaging user experience

**When disabled (`false`):**
- Standard thumb appearance
- No glass effect rendering

**XAML Example:**
```xaml
<sliders:SfRangeSlider Minimum="10"
                       Maximum="20"
                       EnableLiquidGlassEffect="True" />
```

**C# Example:**
```csharp
SfRangeSlider rangeSlider = new SfRangeSlider
{
    Minimum = 10,
    Maximum = 20,
    EnableLiquidGlassEffect = true
};
```

## Setup and Configuration

### Basic Setup

The simplest way to enable the Liquid Glass Effect is to set the `EnableLiquidGlassEffect` property to `true`.

**XAML Example:**
```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sliders="clr-namespace:Syncfusion.Maui.Sliders;assembly=Syncfusion.Maui.Sliders"
             x:Class="MyApp.MainPage">
    <sliders:SfRangeSlider Minimum="0"
                           Maximum="100"
                           RangeStart="25"
                           RangeEnd="75"
                           EnableLiquidGlassEffect="True" />
</ContentPage>
```

**C# Example:**
```csharp
using Syncfusion.Maui.Sliders;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        var rangeSlider = new SfRangeSlider
        {
            Minimum = 0,
            Maximum = 100,
            RangeStart = 25,
            RangeEnd = 75,
            EnableLiquidGlassEffect = true
        };
        
        Content = rangeSlider;
    }
}
```

### With Background Images

The Liquid Glass Effect works best with background images or complex backgrounds, where the translucent effect can showcase the underlying content.

**XAML Example:**
```xaml
<Grid>
    <Image Source="wallpaper.png" 
           Aspect="AspectFill" />
    
    <sliders:SfRangeSlider Minimum="10"
                           Maximum="20"
                           RangeStart="12"
                           RangeEnd="18"
                           EnableLiquidGlassEffect="True"
                           VerticalOptions="Center"
                           Margin="20,0" />
</Grid>
```

**C# Example:**
```csharp
var grid = new Grid
{
    BackgroundColor = Colors.Transparent
};

var backgroundImage = new Image
{
    Source = "wallpaper.png",
    Aspect = Aspect.AspectFill
};
grid.Children.Add(backgroundImage);

var rangeSlider = new SfRangeSlider
{
    Minimum = 10,
    Maximum = 20,
    RangeStart = 12,
    RangeEnd = 18,
    EnableLiquidGlassEffect = true,
    VerticalOptions = LayoutOptions.Center,
    Margin = new Thickness(20, 0)
};
grid.Children.Add(rangeSlider);

Content = grid;
```

**With Gradient Background:**
```xaml
<Grid>
    <BoxView>
        <BoxView.Background>
            <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
                <GradientStop Color="#667eea" Offset="0.0" />
                <GradientStop Color="#764ba2" Offset="1.0" />
            </LinearGradientBrush>
        </BoxView.Background>
    </BoxView>
    
    <sliders:SfRangeSlider EnableLiquidGlassEffect="True"
                           VerticalOptions="Center"
                           Margin="30" />
</Grid>
```

## Visual Interaction Feedback

The Liquid Glass Effect provides responsive visual feedback during user interaction:

**Interaction States:**

1. **Idle State**
   - Glass effect subtle or not visible
   - Standard thumb appearance

2. **Pressed State**
   - Glass effect activates immediately
   - Translucent overlay appears
   - Adaptive color tinting begins

3. **Dragging State**
   - Glass effect remains active
   - Light refraction effects during movement
   - Smooth visual feedback follows thumb position

4. **Released State**
   - Glass effect gradually fades
   - Returns to idle state

**Implementation Example:**
```xaml
<Grid>
    <Image Source="nature_background.jpg" Aspect="AspectFill" />
    
    <VerticalStackLayout Padding="20" Spacing="30" VerticalOptions="Center">
        <Label Text="Volume Range"
               FontSize="18"
               FontAttributes="Bold"
               TextColor="White"
               HorizontalOptions="Center" />
        
        <sliders:SfRangeSlider Minimum="0"
                               Maximum="100"
                               RangeStart="30"
                               RangeEnd="70"
                               ShowLabels="True"
                               ShowTicks="True"
                               Interval="10"
                               EnableLiquidGlassEffect="True" />
    </VerticalStackLayout>
</Grid>
```

## Common Scenarios

### Media Player Volume Control
```xaml
<Grid>
    <Image Source="album_art.jpg" Aspect="AspectFill" />
    <BoxView Color="#80000000" />
    
    <VerticalStackLayout Padding="30" VerticalOptions="End">
        <Label Text="Volume Control" 
               TextColor="White"
               FontSize="16"
               Margin="0,0,0,10" />
        
        <sliders:SfRangeSlider Minimum="0"
                               Maximum="100"
                               RangeStart="20"
                               RangeEnd="80"
                               ShowLabels="True"
                               EnableLiquidGlassEffect="True">
            <sliders:SfRangeSlider.Tooltip>
                <sliders:SliderTooltip ShowAlways="False" />
            </sliders:SfRangeSlider.Tooltip>
        </sliders:SfRangeSlider>
    </VerticalStackLayout>
</Grid>
```

### Price Range Filter
```xaml
<Grid>
    <Image Source="shopping_background.jpg" Aspect="AspectFill" />
    
    <Frame BackgroundColor="#99FFFFFF"
           CornerRadius="15"
           Padding="20"
           Margin="20"
           VerticalOptions="Center">
        <StackLayout Spacing="15">
            <Label Text="Price Range"
                   FontSize="20"
                   FontAttributes="Bold"
                   HorizontalOptions="Center" />
            
            <sliders:SfRangeSlider Minimum="0"
                                   Maximum="1000"
                                   RangeStart="100"
                                   RangeEnd="500"
                                   Interval="100"
                                   NumberFormat="C0"
                                   ShowLabels="True"
                                   ShowTicks="True"
                                   EnableLiquidGlassEffect="True" />
        </StackLayout>
    </Frame>
</Grid>
```

### Time Range Selector
```csharp
var grid = new Grid();

// Background
var background = new Image
{
    Source = "timeline_background.jpg",
    Aspect = Aspect.AspectFill
};
grid.Children.Add(background);

// Semi-transparent overlay
var overlay = new BoxView
{
    Color = Color.FromArgb("#66000000")
};
grid.Children.Add(overlay);

// Range Slider
var timeRangeSlider = new SfRangeSlider
{
    Minimum = 0,
    Maximum = 24,
    RangeStart = 9,
    RangeEnd = 17,
    Interval = 3,
    ShowLabels = true,
    ShowTicks = true,
    EnableLiquidGlassEffect = true,
    VerticalOptions = LayoutOptions.Center,
    Margin = new Thickness(30)
};

// Custom label formatting for hours
timeRangeSlider.LabelCreated += (s, e) =>
{
    if (double.TryParse(e.Text, out double hour))
    {
        e.Text = $"{hour:00}:00";
    }
};

grid.Children.Add(timeRangeSlider);
Content = grid;
```

### Image Editing Range
```xaml
<Grid>
    <Image Source="photo_to_edit.jpg" 
           Aspect="AspectFill" />
    
    <VerticalStackLayout Padding="20" 
                         Spacing="20"
                         VerticalOptions="End"
                         BackgroundColor="#AA000000">
        <Label Text="Adjust Brightness"
               TextColor="White"
               FontSize="16" />
        
        <sliders:SfRangeSlider Minimum="0"
                               Maximum="100"
                               RangeStart="40"
                               RangeEnd="60"
                               ShowLabels="True"
                               Interval="20"
                               EnableLiquidGlassEffect="True">
            <sliders:SfRangeSlider.Tooltip>
                <sliders:SliderTooltip ShowAlways="True" />
            </sliders:SfRangeSlider.Tooltip>
        </sliders:SfRangeSlider>
    </VerticalStackLayout>
</Grid>
```

## Best Practices

1. **Platform Checking**:
   ```csharp
   if (DeviceInfo.Platform == DevicePlatform.iOS || 
       DeviceInfo.Platform == DevicePlatform.macOS)
   {
       rangeSlider.EnableLiquidGlassEffect = true;
   }
   ```

2. **Use with Rich Backgrounds**:
   - Background images enhance the glass effect
   - Gradient backgrounds work well
   - Avoid plain white backgrounds
   - Semi-transparent overlays can improve contrast

3. **Contrast Considerations**:
   - Ensure thumbs remain visible on all backgrounds
   - Test with various background colors/images
   - Consider adding subtle shadows or borders if needed

4. **Performance**:
   - Glass effect has minimal performance impact
   - Test on target devices
   - Monitor for any rendering issues

5. **Accessibility**:
   - Glass effect should not reduce usability
   - Maintain sufficient contrast
   - Ensure thumbs are easily identifiable
   - Test with accessibility features enabled

6. **Design Integration**:
   - Match overall app aesthetic
   - Consistent with iOS/macOS design guidelines
   - Consider light and dark mode appearances
   - Test on different screen sizes

7. **User Experience**:
   - Glass effect should enhance, not distract
   - Smooth interaction is key
   - Provide clear visual feedback
   - Test with real users

8. **Fallback Strategy**:
   ```csharp
   public void ConfigureRangeSlider(SfRangeSlider slider)
   {
       // Check platform and version
       bool supportsGlassEffect = IsGlassEffectSupported();
       
       if (supportsGlassEffect)
       {
           slider.EnableLiquidGlassEffect = true;
       }
       else
       {
           // Use alternative styling
           slider.ThumbStyle = new SliderThumbStyle
           {
               Fill = Colors.White,
               Stroke = Colors.Blue,
               StrokeThickness = 2
           };
       }
   }
   
   private bool IsGlassEffectSupported()
   {
       // Check for .NET 10 and iOS 26 / macOS 26
       // Implement version checking logic
       return false; // Placeholder
   }
   ```

9. **Testing**:
   - Test on physical iOS 26 devices
   - Test on macOS 26
   - Verify effect on different backgrounds
   - Check performance impact

10. **Documentation**:
    - Document platform requirements clearly
    - Provide visual examples
    - Include fallback behavior notes
    - Update when platform support changes

## Related References

- [thumbs-and-overlays.md](./thumbs-and-overlays.md) - Thumb styling and interaction
- [getting-started.md](../getting-started.md) - Initial Range Slider setup
- [track.md](./track.md) - Track styling considerations
- [tooltips.md](./tooltips.md) - Tooltip appearance with glass effect
