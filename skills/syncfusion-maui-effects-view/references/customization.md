# Customization and Styling

Comprehensive guide to customizing SfEffectsView appearance, animation timing, colors, and behavior.

## Table of Contents
- [Animation Duration Properties](#animation-duration-properties)
- [Ripple Customization](#ripple-customization)
- [Scale Customization](#scale-customization)
- [Rotation Customization](#rotation-customization)
- [Color Customization](#color-customization)
- [Advanced Styling Examples](#advanced-styling-examples)
- [Responsive Design](#responsive-design)

## Animation Duration Properties

Control how fast effects animate by adjusting duration properties (in milliseconds).

### RippleAnimationDuration

Sets the duration for ripple effect expansion.

**Property Type:** `double` (milliseconds)  
**Default Value:** 180ms

```xaml
<effectsView:SfEffectsView TouchDownEffects="Ripple"
                            RippleAnimationDuration="600">
    <Label Text="Fast Ripple (600ms)" Padding="20" />
</effectsView:SfEffectsView>
```

```csharp
var effectsView = new SfEffectsView
{
    TouchDownEffects = SfEffects.Ripple,
    RippleAnimationDuration = 600
};
```

**Recommended Values:**
- **Fast:** 300-500ms (quick, snappy feedback)
- **Normal:** 600-800ms (balanced, default feel)
- **Slow:** 1000-1500ms (dramatic, attention-drawing)

**Example - Speed Comparison:**

```xaml
<StackLayout Spacing="20" Padding="20">
    
    <!-- Fast Ripple -->
    <effectsView:SfEffectsView TouchDownEffects="Ripple"
                                RippleAnimationDuration="300">
        <Border BackgroundColor="#4CAF50" Padding="15">
            <Label Text="Fast (300ms)" TextColor="White" />
        </Border>
    </effectsView:SfEffectsView>
    
    <!-- Normal Ripple -->
    <effectsView:SfEffectsView TouchDownEffects="Ripple"
                                RippleAnimationDuration="800">
        <Border BackgroundColor="#2196F3" Padding="15">
            <Label Text="Normal (800ms)" TextColor="White" />
        </Border>
    </effectsView:SfEffectsView>
    
    <!-- Slow Ripple -->
    <effectsView:SfEffectsView TouchDownEffects="Ripple"
                                RippleAnimationDuration="1500">
        <Border BackgroundColor="#FF9800" Padding="15">
            <Label Text="Slow (1500ms)" TextColor="White" />
        </Border>
    </effectsView:SfEffectsView>
    
</StackLayout>
```

### ScaleAnimationDuration

Sets the duration for scale effect animation.

**Property Type:** `double` (milliseconds)  
**Default Value:** 150ms

```xaml
<effectsView:SfEffectsView LongPressEffects="Scale"
                            ScaleFactor="0.85"
                            ScaleAnimationDuration="800">
    <Image Source="product.png" 
           WidthRequest="100" 
           HeightRequest="100" />
</effectsView:SfEffectsView>
```

```csharp
var effectsView = new SfEffectsView
{
    LongPressEffects = SfEffects.Scale,
    ScaleFactor = 0.85,
    ScaleAnimationDuration = 800
};
```

**Recommended Values:**
- **Fast:** 100-200ms (instant feedback)
- **Normal:** 200-400ms (smooth transition)
- **Slow:** 500-800ms (deliberate, emphasized)

### RotationAnimationDuration

Sets the duration for rotation effect animation.

**Property Type:** `double` (milliseconds)  
**Default Value:** 200ms

```xaml
<effectsView:SfEffectsView TouchDownEffects="Rotation"
                            Angle="180"
                            RotationAnimationDuration="1000">
    <Label Text="🔄" FontSize="48" />
</effectsView:SfEffectsView>
```

```csharp
var effectsView = new SfEffectsView
{
    TouchDownEffects = SfEffects.Rotation,
    Angle = 180,
    RotationAnimationDuration = 1000
};
```

**Recommended Values:**
- **Fast:** 200-400ms (quick spin)
- **Normal:** 500-700ms (balanced rotation)
- **Slow:** 800-1200ms (smooth, noticeable rotation)

### Synchronized Animation Timing

Set all durations to the same value for synchronized effects:

```xaml
<effectsView:SfEffectsView TouchDownEffects="Ripple, Scale, Rotation"
                            RippleAnimationDuration="600"
                            ScaleAnimationDuration="600"
                            RotationAnimationDuration="600"
                            ScaleFactor="0.9"
                            Angle="90">
    <Label Text="Synchronized" FontSize="20" Padding="20" />
</effectsView:SfEffectsView>
```

## Ripple Customization

### InitialRippleFactor

Controls the starting radius of the ripple as a fraction of the view size.

**Property Type:** `double`  
**Range:** 0.0 to 1.0  
**Default Value:** 0.25 (starts from touch point)

```xaml
<effectsView:SfEffectsView TouchDownEffects="Ripple"
                            InitialRippleFactor="0.3">
    <Border BackgroundColor="#E91E63" Padding="30">
        <Label Text="Large Initial Ripple" TextColor="White" />
    </Border>
</effectsView:SfEffectsView>
```

```csharp
var effectsView = new SfEffectsView
{
    TouchDownEffects = SfEffects.Ripple,
    InitialRippleFactor = 0.3  // Starts at 30% of view size
};
```

**How It Works:**
- `0.0` = Ripple starts as a small circle at touch point
- `0.5` = Ripple starts at 50% of view size
- `1.0` = Ripple starts filling the entire view (instant)

**Visual Comparison:**

```xaml
<StackLayout Spacing="20" Padding="20">
    
    <!-- Small Initial Ripple -->
    <effectsView:SfEffectsView TouchDownEffects="Ripple"
                                InitialRippleFactor="0.0">
        <Border BackgroundColor="#9C27B0" Padding="15">
            <Label Text="Factor: 0.0" TextColor="White" />
        </Border>
    </effectsView:SfEffectsView>
    
    <!-- Medium Initial Ripple -->
    <effectsView:SfEffectsView TouchDownEffects="Ripple"
                                InitialRippleFactor="0.3">
        <Border BackgroundColor="#673AB7" Padding="15">
            <Label Text="Factor: 0.3" TextColor="White" />
        </Border>
    </effectsView:SfEffectsView>
    
    <!-- Large Initial Ripple -->
    <effectsView:SfEffectsView TouchDownEffects="Ripple"
                                InitialRippleFactor="0.6">
        <Border BackgroundColor="#3F51B5" Padding="15">
            <Label Text="Factor: 0.6" TextColor="White" />
        </Border>
    </effectsView:SfEffectsView>
    
</StackLayout>
```

**Use Cases:**
- `0.0-0.1`: Traditional Material Design ripple (starts small)
- `0.2-0.4`: Faster visual feedback (less expansion time)
- `0.5+`: Subtle ripple effect (mostly filled already)

## Scale Customization

### ScaleFactor

Controls the target scale size as a multiplier of original size.

**Property Type:** `double`  
**Range:** 0.0+ (typically 0.5 to 1.5)  
**Default Value:** 1.0

```xaml
<effectsView:SfEffectsView TouchDownEffects="Scale"
                            ScaleFactor="0.85"
                            ScaleAnimationDuration="200">
    <Border BackgroundColor="#00BCD4" Padding="20">
        <Label Text="Subtle Scale" TextColor="White" />
    </Border>
</effectsView:SfEffectsView>
```

```csharp
var effectsView = new SfEffectsView
{
    TouchDownEffects = SfEffects.Scale,
    ScaleFactor = 0.85  // Scale to 85% size
};
```

**Scale Factor Guide:**

| Factor | Effect | Use Case |
|--------|--------|----------|
| 0.5 | 50% size (dramatic shrink) | Emphasis, playful interactions |
| 0.7 | 70% size (noticeable) | Strong press feedback |
| 0.8 | 80% size (default) | Standard button press |
| 0.9 | 90% size (subtle) | Gentle feedback |
| 0.95 | 95% size (minimal) | Refined, sophisticated UX |
| 1.0 | No change | Disabled scale |
| 1.1 | 110% size (grow) | Zoom in, emphasis |
| 1.5 | 150% size (large grow) | Strong emphasis, pop effect |

**Scale Down vs Scale Up:**

```xaml
<StackLayout Spacing="20" Padding="20" HorizontalOptions="Center">
    
    <!-- Scale Down on Press -->
    <effectsView:SfEffectsView TouchDownEffects="Scale"
                                ScaleFactor="0.85">
        <Border BackgroundColor="#FF5722" Padding="20">
            <Label Text="Press: Scale Down" TextColor="White" />
        </Border>
    </effectsView:SfEffectsView>
    
    <!-- Scale Up on Release -->
    <effectsView:SfEffectsView TouchUpEffects="Scale"
                                ScaleFactor="1.15">
        <Border BackgroundColor="#4CAF50" Padding="20">
            <Label Text="Release: Scale Up" TextColor="White" />
        </Border>
    </effectsView:SfEffectsView>
    
    <!-- Dramatic Shrink -->
    <effectsView:SfEffectsView TouchDownEffects="Scale"
                                ScaleFactor="0.5"
                                ScaleAnimationDuration="300">
        <Border BackgroundColor="#9C27B0" Padding="20">
            <Label Text="Dramatic Press" TextColor="White" />
        </Border>
    </effectsView:SfEffectsView>
    
</StackLayout>
```

## Rotation Customization

### Angle

Sets the rotation angle in degrees (clockwise).

**Property Type:** `int`  
**Range:** -360 to 360 (and beyond)  
**Default Value:** 0

```xaml
<effectsView:SfEffectsView TouchDownEffects="Rotation"
                            Angle="180"
                            RotationAnimationDuration="500">
    <Image Source="arrow.png" 
           WidthRequest="60" 
           HeightRequest="60" />
</effectsView:SfEffectsView>
```

```csharp
var effectsView = new SfEffectsView
{
    TouchDownEffects = SfEffects.Rotation,
    Angle = 180
};
```

**Common Angles:**

| Angle | Rotation | Use Case |
|-------|----------|----------|
| 45 | Slight tilt | Subtle interaction |
| 90 | Quarter turn | Right angle rotation |
| 180 | Half turn | Flip effect |
| 270 | Three-quarter turn | Reverse right angle |
| 360 | Full circle | Complete spin, refresh |
| -90 | Counter-clockwise quarter | Reverse rotation |

**Angle Examples:**

```xaml
<Grid RowDefinitions="Auto,Auto,Auto,Auto" 
      RowSpacing="20" 
      Padding="20">
    
    <!-- 45° Tilt -->
    <effectsView:SfEffectsView TouchDownEffects="Rotation"
                                Angle="45">
        <Label Text="45° →" FontSize="24" Padding="15" BackgroundColor="LightBlue" />
    </effectsView:SfEffectsView>
    
    <!-- 90° Right Angle -->
    <effectsView:SfEffectsView Grid.Row="1" 
                                TouchDownEffects="Rotation"
                                Angle="90">
        <Label Text="90° ↓" FontSize="24" Padding="15" BackgroundColor="LightGreen" />
    </effectsView:SfEffectsView>
    
    <!-- 180° Flip -->
    <effectsView:SfEffectsView Grid.Row="2" 
                                TouchDownEffects="Rotation"
                                Angle="180">
        <Label Text="180° ↑↓" FontSize="24" Padding="15" BackgroundColor="LightCoral" />
    </effectsView:SfEffectsView>
    
    <!-- 360° Full Spin -->
    <effectsView:SfEffectsView Grid.Row="3" 
                                TouchDownEffects="Rotation"
                                Angle="360"
                                RotationAnimationDuration="1000">
        <Label Text="360° ↻" FontSize="24" Padding="15" BackgroundColor="LightYellow" />
    </effectsView:SfEffectsView>
    
</Grid>
```

### Negative Angles (Counter-Clockwise)

Use negative values to rotate counter-clockwise:

```xaml
<effectsView:SfEffectsView TouchDownEffects="Rotation"
                            Angle="-90">
    <Label Text="↺ Counter-Clockwise" FontSize="20" Padding="15" />
</effectsView:SfEffectsView>
```

## Color Customization

### HighlightBackground

Sets the color of the highlight effect overlay.

**Property Type:** `Brush`  
**Default Value:** Semi-transparent black

```xaml
<effectsView:SfEffectsView TouchDownEffects="Highlight"
                            HighlightBackground="#2196F3">
    <Border BackgroundColor="White" Padding="20">
        <Label Text="Blue Highlight" FontSize="16" />
    </Border>
</effectsView:SfEffectsView>
```

```csharp
var effectsView = new SfEffectsView
{
    TouchDownEffects = SfEffects.Highlight,
    HighlightBackground = new SolidColorBrush(Color.FromArgb("#2196F3"))
};
```

**Color with Opacity:**

Use hex colors with alpha channel for transparency:

```xaml
<!-- 20% black overlay -->
<effectsView:SfEffectsView TouchDownEffects="Highlight"
                            HighlightBackground="#33000000">
    <Label Text="Subtle Highlight" Padding="20" />
</effectsView:SfEffectsView>

<!-- 50% white overlay -->
<effectsView:SfEffectsView TouchDownEffects="Highlight"
                            HighlightBackground="#80FFFFFF">
    <Label Text="White Highlight" Padding="20" BackgroundColor="DarkGray" />
</effectsView:SfEffectsView>
```

**Hex Color Format:** `#AARRGGBB`
- `AA` = Alpha (opacity): 00 (transparent) to FF (opaque)
- `RR` = Red
- `GG` = Green
- `BB` = Blue

### RippleBackground

Sets the color of the ripple effect.

**Property Type:** `Brush`  
**Default Value:** Semi-transparent white

```xaml
<effectsView:SfEffectsView TouchDownEffects="Ripple"
                            RippleBackground="#FF5722">
    <Border BackgroundColor="#212121" Padding="30">
        <Label Text="Orange Ripple" TextColor="White" FontSize="16" />
    </Border>
</effectsView:SfEffectsView>
```

```csharp
var effectsView = new SfEffectsView
{
    TouchDownEffects = SfEffects.Ripple,
    RippleBackground = new SolidColorBrush(Color.FromArgb("#FF5722"))
};
```

**Ripple Color Tips:**

1. **Light backgrounds:** Use darker ripple colors
   ```xaml
   <effectsView:SfEffectsView TouchDownEffects="Ripple"
                               RippleBackground="#40000000">
       <Border BackgroundColor="White" Padding="20">
           <Label Text="Dark Ripple on Light" />
       </Border>
   </effectsView:SfEffectsView>
   ```

2. **Dark backgrounds:** Use lighter ripple colors
   ```xaml
   <effectsView:SfEffectsView TouchDownEffects="Ripple"
                               RippleBackground="#40FFFFFF">
       <Border BackgroundColor="#212121" Padding="20">
           <Label Text="Light Ripple on Dark" TextColor="White" />
       </Border>
   </effectsView:SfEffectsView>
   ```

3. **Colored backgrounds:** Use complementary or accent colors
   ```xaml
   <effectsView:SfEffectsView TouchDownEffects="Ripple"
                               RippleBackground="#FFEB3B">
       <Border BackgroundColor="#2196F3" Padding="20">
           <Label Text="Yellow Ripple on Blue" TextColor="White" />
       </Border>
   </effectsView:SfEffectsView>
   ```

### SelectionBackground

Sets the color of the persistent selection overlay.

**Property Type:** `Brush`  
**Default Value:** Semi-transparent blue

```xaml
<effectsView:SfEffectsView LongPressEffects="Selection"
                            SelectionBackground="#C8E6C9">
    <Border BackgroundColor="White" Padding="20">
        <Label Text="Green Selection" FontSize="16" />
    </Border>
</effectsView:SfEffectsView>
```

```csharp
var effectsView = new SfEffectsView
{
    LongPressEffects = SfEffects.Selection,
    SelectionBackground = new SolidColorBrush(Color.FromArgb("#C8E6C9"))
};
```

**Selection Color Patterns:**

```xaml
<StackLayout Spacing="15" Padding="20">
    
    <!-- Material Design Blue -->
    <effectsView:SfEffectsView LongPressEffects="Selection"
                                SelectionBackground="#BBDEFB">
        <Grid Padding="15" BackgroundColor="White">
            <Label Text="Selected (Blue)" />
        </Grid>
    </effectsView:SfEffectsView>
    
    <!-- Material Design Green -->
    <effectsView:SfEffectsView LongPressEffects="Selection"
                                SelectionBackground="#C8E6C9">
        <Grid Padding="15" BackgroundColor="White">
            <Label Text="Selected (Green)" />
        </Grid>
    </effectsView:SfEffectsView>
    
    <!-- Material Design Orange -->
    <effectsView:SfEffectsView LongPressEffects="Selection"
                                SelectionBackground="#FFE0B2">
        <Grid Padding="15" BackgroundColor="White">
            <Label Text="Selected (Orange)" />
        </Grid>
    </effectsView:SfEffectsView>
    
</StackLayout>
```

## Advanced Styling Examples

### Material Design Primary Button

```xaml
<effectsView:SfEffectsView TouchDownEffects="Ripple"
                            RippleBackground="#FFFFFF"
                            RippleAnimationDuration="600">
    <Border BackgroundColor="#6200EE"
            Padding="32,12"
            StrokeThickness="0">
        <Border.StrokeShape>
            <RoundRectangle CornerRadius="4" />
        </Border.StrokeShape>
        <Label Text="PRIMARY ACTION"
               TextColor="White"
               FontSize="14"
               FontAttributes="Bold"
               HorizontalOptions="Center" />
    </Border>
</effectsView:SfEffectsView>
```

### Outlined Button with Ripple

```xaml
<effectsView:SfEffectsView TouchDownEffects="Ripple"
                            RippleBackground="#6200EE">
    <Border BackgroundColor="Transparent"
            Stroke="#6200EE"
            StrokeThickness="2"
            Padding="32,12">
        <Border.StrokeShape>
            <RoundRectangle CornerRadius="4" />
        </Border.StrokeShape>
        <Label Text="OUTLINED"
               TextColor="#6200EE"
               FontSize="14"
               FontAttributes="Bold"
               HorizontalOptions="Center" />
    </Border>
</effectsView:SfEffectsView>
```

### Floating Action Button (FAB)

```xaml
<effectsView:SfEffectsView TouchDownEffects="Ripple, Scale"
                            RippleBackground="#FFFFFF"
                            ScaleFactor="0.9"
                            ScaleAnimationDuration="100">
    <Border BackgroundColor="#FF4081"
            WidthRequest="56"
            HeightRequest="56"
            StrokeThickness="0">
        <Border.StrokeShape>
            <RoundRectangle CornerRadius="28" />
        </Border.StrokeShape>
        <Border.Shadow>
            <Shadow Brush="Black" Opacity="0.3" Radius="8" Offset="0,4" />
        </Border.Shadow>
        <Label Text="+"
               TextColor="White"
               FontSize="32"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
    </Border>
</effectsView:SfEffectsView>
```

### Card with Press Feedback

```xaml
<effectsView:SfEffectsView TouchDownEffects="Scale"
                            ScaleFactor="0.97"
                            ScaleAnimationDuration="150">
    <Border BackgroundColor="White"
            Padding="16"
            StrokeThickness="0">
        <Border.StrokeShape>
            <RoundRectangle CornerRadius="8" />
        </Border.StrokeShape>
        <Border.Shadow>
            <Shadow Brush="Black" Opacity="0.1" Radius="4" Offset="0,2" />
        </Border.Shadow>
        
        <Grid RowDefinitions="Auto,Auto,Auto" RowSpacing="8">
            <Label Text="Card Title"
                   FontSize="18"
                   FontAttributes="Bold" />
            <Label Grid.Row="1"
                   Text="Card subtitle or description text goes here."
                   FontSize="14"
                   TextColor="Gray" />
            <Button Grid.Row="2"
                    Text="ACTION"
                    BackgroundColor="Transparent"
                    TextColor="#6200EE"
                    HorizontalOptions="End" />
        </Grid>
    </Border>
</effectsView:SfEffectsView>
```

### Image Gallery Item

```xaml
<effectsView:SfEffectsView TouchDownEffects="Scale"
                            TouchUpEffects="None"
                            ScaleFactor="0.92"
                            ScaleAnimationDuration="200">
    <Grid>
        <Image Source="photo.jpg"
               Aspect="AspectFill"
               WidthRequest="150"
               HeightRequest="150" />
        <Border BackgroundColor="#80000000"
                VerticalOptions="End"
                Padding="8">
            <Label Text="Photo Title"
                   TextColor="White"
                   FontSize="12" />
        </Border>
    </Grid>
</effectsView:SfEffectsView>
```

## Responsive Design

### Adapting to Screen Size

Adjust animation properties based on device idiom or size:

```csharp
public class ResponsiveEffectsView : SfEffectsView
{
    public ResponsiveEffectsView()
    {
        // Faster animations on phones, slower on tablets
        if (DeviceInfo.Idiom == DeviceIdiom.Phone)
        {
            RippleAnimationDuration = 500;
            ScaleAnimationDuration = 150;
        }
        else if (DeviceInfo.Idiom == DeviceIdiom.Tablet)
        {
            RippleAnimationDuration = 800;
            ScaleAnimationDuration = 250;
        }
    }
}
```

### Platform-Specific Styling

```csharp
public void ConfigureEffectsView(SfEffectsView effectsView)
{
    effectsView.TouchDownEffects = SfEffects.Ripple;
    
    // iOS: Subtle, refined
    if (DeviceInfo.Platform == DevicePlatform.iOS)
    {
        effectsView.RippleAnimationDuration = 400;
        effectsView.RippleBackground = new SolidColorBrush(
            Color.FromArgb("#20000000"));
    }
    // Android: Material Design
    else if (DeviceInfo.Platform == DevicePlatform.Android)
    {
        effectsView.RippleAnimationDuration = 600;
        effectsView.RippleBackground = new SolidColorBrush(
            Color.FromArgb("#40FFFFFF"));
    }
}
```

### Accessibility Considerations

Reduce animations for users who prefer reduced motion:

```csharp
public void ApplyAccessibilitySettings(SfEffectsView effectsView)
{
    // Check if user prefers reduced motion (platform-specific)
    bool prefersReducedMotion = CheckReducedMotionPreference();
    
    if (prefersReducedMotion)
    {
        // Shorten animation durations
        effectsView.RippleAnimationDuration = 200;
        effectsView.ScaleAnimationDuration = 100;
        effectsView.RotationAnimationDuration = 200;
        
        // Or disable animations entirely
        effectsView.TouchDownEffects = SfEffects.Highlight;  // Instant feedback
    }
}
```

### Theme-Based Colors

Adapt effect colors to app theme:

```csharp
public void ApplyTheme(SfEffectsView effectsView, bool isDarkMode)
{
    if (isDarkMode)
    {
        effectsView.RippleBackground = new SolidColorBrush(
            Color.FromArgb("#40FFFFFF"));  // Light ripple
        effectsView.HighlightBackground = new SolidColorBrush(
            Color.FromArgb("#20FFFFFF"));  // Light highlight
        effectsView.SelectionBackground = new SolidColorBrush(
            Color.FromArgb("#304050"));    // Dark blue selection
    }
    else
    {
        effectsView.RippleBackground = new SolidColorBrush(
            Color.FromArgb("#40000000"));  // Dark ripple
        effectsView.HighlightBackground = new SolidColorBrush(
            Color.FromArgb("#10000000"));  // Dark highlight
        effectsView.SelectionBackground = new SolidColorBrush(
            Color.FromArgb("#BBDEFB"));    // Light blue selection
    }
}
```
