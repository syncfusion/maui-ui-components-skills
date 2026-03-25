# Appearance Customization

## Table of Contents
- [Range Colors](#range-colors)
- [Thickness Customization](#thickness-customization)
- [Padding](#padding)
- [Corner Radius](#corner-radius)
- [Color Customization](#color-customization)
- [Complete Styling Examples](#complete-styling-examples)

The Linear ProgressBar provides extensive appearance customization options to match your application's design system and user experience requirements.

## Range Colors

Range colors allow you to map different colors to specific progress ranges, enhancing readability and providing visual cues about progress status.

### Basic Concept

Use the `GradientStops` property to define color ranges. Each `ProgressGradientStop` specifies:
- **Color**: The color to display
- **Value**: The progress value where this color applies (0-100 by default)

### Solid Color Ranges

Create distinct color blocks for different progress zones:

```xaml
<progressBar:SfLinearProgressBar Progress="100">
    <progressBar:SfLinearProgressBar.GradientStops>
        <!-- Green zone: 0-25% -->
        <progressBar:ProgressGradientStop Color="#00bdaf" Value="0"/>
        <progressBar:ProgressGradientStop Color="#00bdaf" Value="25"/>
        
        <!-- Blue zone: 25-50% -->
        <progressBar:ProgressGradientStop Color="#2f7ecc" Value="25"/>
        <progressBar:ProgressGradientStop Color="#2f7ecc" Value="50"/>
        
        <!-- Pink zone: 50-75% -->
        <progressBar:ProgressGradientStop Color="#e9648e" Value="50"/>
        <progressBar:ProgressGradientStop Color="#e9648e" Value="75"/>
        
        <!-- Orange zone: 75-100% -->
        <progressBar:ProgressGradientStop Color="#fbb78a" Value="75"/>
        <progressBar:ProgressGradientStop Color="#fbb78a" Value="100"/>
    </progressBar:SfLinearProgressBar.GradientStops>
</progressBar:SfLinearProgressBar>
```

```csharp
var progressBar = new SfLinearProgressBar { Progress = 100 };

// Add solid color ranges
progressBar.GradientStops.Add(new ProgressGradientStop { Color = Color.FromArgb("#00bdaf"), Value = 0 });
progressBar.GradientStops.Add(new ProgressGradientStop { Color = Color.FromArgb("#00bdaf"), Value = 25 });
progressBar.GradientStops.Add(new ProgressGradientStop { Color = Color.FromArgb("#2f7ecc"), Value = 25 });
progressBar.GradientStops.Add(new ProgressGradientStop { Color = Color.FromArgb("#2f7ecc"), Value = 50 });
progressBar.GradientStops.Add(new ProgressGradientStop { Color = Color.FromArgb("#e9648e"), Value = 50 });
progressBar.GradientStops.Add(new ProgressGradientStop { Color = Color.FromArgb("#e9648e"), Value = 75 });
progressBar.GradientStops.Add(new ProgressGradientStop { Color = Color.FromArgb("#fbb78a"), Value = 75 });
progressBar.GradientStops.Add(new ProgressGradientStop { Color = Color.FromArgb("#fbb78a"), Value = 100 });
```

### Gradient Transitions

Create smooth color transitions by defining gradient stops without duplicates:

```xaml
<progressBar:SfLinearProgressBar Progress="100">
    <progressBar:SfLinearProgressBar.GradientStops>
        <progressBar:ProgressGradientStop Color="#00bdaf" Value="0"/>
        <progressBar:ProgressGradientStop Color="#2f7ecc" Value="25"/>
        <progressBar:ProgressGradientStop Color="#e9648e" Value="50"/>
        <progressBar:ProgressGradientStop Color="#fbb78a" Value="75"/>
    </progressBar:SfLinearProgressBar.GradientStops>
</progressBar:SfLinearProgressBar>
```

```csharp
var gradientProgress = new SfLinearProgressBar { Progress = 100 };

gradientProgress.GradientStops.Add(new ProgressGradientStop { Color = Color.FromArgb("#00bdaf"), Value = 0 });
gradientProgress.GradientStops.Add(new ProgressGradientStop { Color = Color.FromArgb("#2f7ecc"), Value = 25 });
gradientProgress.GradientStops.Add(new ProgressGradientStop { Color = Color.FromArgb("#e9648e"), Value = 50 });
gradientProgress.GradientStops.Add(new ProgressGradientStop { Color = Color.FromArgb("#fbb78a"), Value = 75 });
```

### Practical Range Color Patterns

#### Success/Warning/Danger Zones

```csharp
// Health bar or performance indicator
var healthBar = new SfLinearProgressBar { Progress = 100 };

// Danger: 0-30% (Red)
healthBar.GradientStops.Add(new ProgressGradientStop { Color = Colors.Red, Value = 0 });
healthBar.GradientStops.Add(new ProgressGradientStop { Color = Colors.Red, Value = 30 });

// Warning: 30-70% (Orange)
healthBar.GradientStops.Add(new ProgressGradientStop { Color = Colors.Orange, Value = 30 });
healthBar.GradientStops.Add(new ProgressGradientStop { Color = Colors.Orange, Value = 70 });

// Success: 70-100% (Green)
healthBar.GradientStops.Add(new ProgressGradientStop { Color = Colors.Green, Value = 70 });
healthBar.GradientStops.Add(new ProgressGradientStop { Color = Colors.Green, Value = 100 });
```

#### Temperature-Based Colors

```csharp
// Cool to warm gradient
var tempProgress = new SfLinearProgressBar { Progress = 100 };

tempProgress.GradientStops.Add(new ProgressGradientStop { Color = Color.FromArgb("#0D47A1"), Value = 0 });   // Deep blue
tempProgress.GradientStops.Add(new ProgressGradientStop { Color = Color.FromArgb("#2196F3"), Value = 25 });  // Blue
tempProgress.GradientStops.Add(new ProgressGradientStop { Color = Color.FromArgb("#FFC107"), Value = 50 });  // Amber
tempProgress.GradientStops.Add(new ProgressGradientStop { Color = Color.FromArgb("#FF5722"), Value = 75 });  // Deep orange
tempProgress.GradientStops.Add(new ProgressGradientStop { Color = Color.FromArgb("#B71C1C"), Value = 100 }); // Dark red
```

## Thickness Customization

Adjust the height of track, progress, and secondary progress independently.

### Properties

- **TrackHeight**: Height of the background track
- **ProgressHeight**: Height of the primary progress indicator
- **SecondaryProgressHeight**: Height of the secondary progress indicator (buffer state)

### Basic Thickness

```xaml
<progressBar:SfLinearProgressBar Progress="75" 
                                 TrackHeight="10" 
                                 ProgressHeight="10"/>
```

```csharp
var progressBar = new SfLinearProgressBar
{
    Progress = 75,
    TrackHeight = 10,
    ProgressHeight = 10
};
```

### Layered Effect (Different Heights)

Create visual depth by using different heights:

```xaml
<!-- Thin progress on thick track -->
<progressBar:SfLinearProgressBar Progress="60" 
                                 TrackHeight="16" 
                                 ProgressHeight="8"/>

<!-- Thick progress on thin track -->
<progressBar:SfLinearProgressBar Progress="60" 
                                 TrackHeight="4" 
                                 ProgressHeight="12"/>
```

### Buffer State with Custom Heights

```xaml
<progressBar:SfLinearProgressBar Progress="30" 
                                 SecondaryProgress="70"
                                 TrackHeight="15"
                                 ProgressHeight="10"
                                 SecondaryProgressHeight="5"/>
```

```csharp
var bufferBar = new SfLinearProgressBar
{
    Progress = 30,
    SecondaryProgress = 70,
    TrackHeight = 15,           // Thickest
    SecondaryProgressHeight = 10, // Medium
    ProgressHeight = 8           // Thinnest
};
```

## Padding

Control the padding around the progress indicator within the track.

### ProgressPadding Property

Adds space between the track edges and the progress/secondary progress indicators:

```xaml
<progressBar:SfLinearProgressBar Progress="30"
                                 SecondaryProgress="70"
                                 TrackHeight="15"
                                 ProgressHeight="5"
                                 SecondaryProgressHeight="5"
                                 ProgressPadding="5"/>
```

```csharp
var paddedProgress = new SfLinearProgressBar
{
    Progress = 30,
    SecondaryProgress = 70,
    TrackHeight = 15,
    ProgressHeight = 5,
    SecondaryProgressHeight = 5,
    ProgressPadding = 5  // 5 pixels padding on all sides
};
```

### Visual Effect

Padding creates an inset appearance:
- Track appears as an outer border
- Progress indicators appear centered within the track
- Creates a "rail" or "channel" effect

### Use Cases for Padding

1. **Subtle design**: When you want progress to appear less dominant
2. **Multi-layer look**: Emphasize the track as a container
3. **Better contrast**: Separate progress from track visually
4. **Cleaner aesthetics**: Modern, minimalist appearance

## Corner Radius

Round the edges of track and progress indicators for softer, modern aesthetics.

### Properties

- **TrackCornerRadius**: Corner radius of the track
- **ProgressCornerRadius**: Corner radius of the progress indicator
- **SecondaryProgressCornerRadius**: Corner radius of secondary progress

### Basic Rounded Progress Bar

```xaml
<progressBar:SfLinearProgressBar Progress="50"
                                 TrackHeight="10" 
                                 ProgressHeight="10"
                                 TrackCornerRadius="5"
                                 ProgressCornerRadius="5"/>
```

```csharp
var roundedProgress = new SfLinearProgressBar
{
    Progress = 50,
    TrackHeight = 10,
    ProgressHeight = 10,
    TrackCornerRadius = 5,
    ProgressCornerRadius = 5
};
```

### Fully Rounded (Pill Shape)

For a completely rounded appearance, set corner radius to half the height:

```csharp
int height = 20;
var pillProgress = new SfLinearProgressBar
{
    Progress = 65,
    TrackHeight = height,
    ProgressHeight = height,
    TrackCornerRadius = height / 2,      // 10 = half of 20
    ProgressCornerRadius = height / 2
};
```

### Asymmetric Rounding

Different corner radius values for visual variety:

```xaml
<!-- Rounded track, sharp progress -->
<progressBar:SfLinearProgressBar Progress="70"
                                 TrackHeight="12"
                                 ProgressHeight="8"
                                 TrackCornerRadius="6"
                                 ProgressCornerRadius="0"/>
```

### Buffer State with Rounded Corners

```xaml
<progressBar:SfLinearProgressBar Progress="30"
                                 SecondaryProgress="75"
                                 TrackHeight="14"
                                 ProgressHeight="10"
                                 SecondaryProgressHeight="10"
                                 TrackCornerRadius="7"
                                 ProgressCornerRadius="5"
                                 SecondaryProgressCornerRadius="5"/>
```

## Color Customization

### Basic Color Properties

- **ProgressFill**: Color of the primary progress indicator
- **TrackFill**: Color of the background track
- **SecondaryProgressFill**: Color of the secondary progress indicator

### Simple Color Customization

```xaml
<progressBar:SfLinearProgressBar Progress="75" 
                                 TrackFill="#3351483a" 
                                 ProgressFill="#FF51483a"/>
```

```csharp
var coloredProgress = new SfLinearProgressBar
{
    Progress = 75,
    TrackFill = Color.FromArgb("#3351483a"),   // Semi-transparent
    ProgressFill = Color.FromArgb("#FF51483a")  // Opaque
};
```

### Using Color Objects

```csharp
var progressBar = new SfLinearProgressBar
{
    Progress = 60,
    TrackFill = Colors.LightGray,
    ProgressFill = Colors.DeepSkyBlue
};
```

### Using SolidColorBrush

```csharp
var progressBar = new SfLinearProgressBar
{
    Progress = 80,
    TrackFill = new SolidColorBrush(Color.FromArgb("#EEEEEE")),
    ProgressFill = new SolidColorBrush(Color.FromArgb("#4CAF50"))
};
```

### Secondary Progress Color

```xaml
<progressBar:SfLinearProgressBar Progress="25" 
                                 SecondaryProgress="75" 
                                 ProgressFill="#FF4CAF50"
                                 SecondaryProgressFill="#8BC34A"
                                 TrackFill="#E0E0E0"/>
```

```csharp
var bufferBar = new SfLinearProgressBar
{
    Progress = 25,
    SecondaryProgress = 75,
    ProgressFill = Color.FromArgb("#FF4CAF50"),      // Dark green
    SecondaryProgressFill = Color.FromArgb("#8BC34A"), // Light green
    TrackFill = Color.FromArgb("#E0E0E0")             // Light gray
};
```

### Color Opacity

Use alpha channel for transparency effects:

```csharp
var progressBar = new SfLinearProgressBar
{
    Progress = 70,
    TrackFill = Color.FromArgb("#33000000"),   // 20% black
    ProgressFill = Color.FromArgb("#FF2196F3")  // 100% blue
};
```

## Complete Styling Examples

### Modern Minimalist

```xaml
<progressBar:SfLinearProgressBar Progress="65"
                                 TrackHeight="8"
                                 ProgressHeight="8"
                                 TrackCornerRadius="4"
                                 ProgressCornerRadius="4"
                                 TrackFill="#F5F5F5"
                                 ProgressFill="#2196F3"/>
```

### Bold and Vibrant

```xaml
<progressBar:SfLinearProgressBar Progress="80"
                                 TrackHeight="16"
                                 ProgressHeight="16"
                                 TrackCornerRadius="8"
                                 ProgressCornerRadius="8"
                                 TrackFill="#1A237E"
                                 ProgressFill="#00E676"/>
```

### Glass Morphism Effect

```xaml
<progressBar:SfLinearProgressBar Progress="55"
                                 TrackHeight="12"
                                 ProgressHeight="12"
                                 TrackCornerRadius="6"
                                 ProgressCornerRadius="6"
                                 TrackFill="#40FFFFFF"
                                 ProgressFill="#80FFFFFF"/>
```

### Layered with Padding

```xaml
<progressBar:SfLinearProgressBar Progress="70"
                                 TrackHeight="20"
                                 ProgressHeight="10"
                                 ProgressPadding="5"
                                 TrackCornerRadius="10"
                                 ProgressCornerRadius="5"
                                 TrackFill="#E0E0E0"
                                 ProgressFill="#FF5722"/>
```

### Multi-Color Gradient

```xaml
<progressBar:SfLinearProgressBar Progress="100"
                                 TrackHeight="14"
                                 ProgressHeight="14"
                                 TrackCornerRadius="7"
                                 ProgressCornerRadius="7">
    <progressBar:SfLinearProgressBar.GradientStops>
        <progressBar:ProgressGradientStop Color="#E91E63" Value="0"/>
        <progressBar:ProgressGradientStop Color="#9C27B0" Value="33"/>
        <progressBar:ProgressGradientStop Color="#3F51B5" Value="66"/>
        <progressBar:ProgressGradientStop Color="#00BCD4" Value="100"/>
    </progressBar:SfLinearProgressBar.GradientStops>
</progressBar:SfLinearProgressBar>
```

### Complete Custom Style Class

```csharp
public static class ProgressBarStyles
{
    public static SfLinearProgressBar CreateModernProgress(double progress)
    {
        return new SfLinearProgressBar
        {
            Progress = progress,
            TrackHeight = 12,
            ProgressHeight = 12,
            TrackCornerRadius = 6,
            ProgressCornerRadius = 6,
            TrackFill = Color.FromArgb("#F5F5F5"),
            ProgressFill = Color.FromArgb("#4CAF50"),
            AnimationDuration = 600,
            AnimationEasing = Easing.CubicOut
        };
    }

    public static SfLinearProgressBar CreateHealthBar(double health)
    {
        var healthBar = new SfLinearProgressBar
        {
            Progress = 100,
            TrackHeight = 16,
            ProgressHeight = 16,
            TrackCornerRadius = 8,
            ProgressCornerRadius = 8,
            TrackFill = Color.FromArgb("#424242")
        };

        // Dynamic color based on health
        healthBar.GradientStops.Add(new ProgressGradientStop { Color = Colors.Red, Value = 0 });
        healthBar.GradientStops.Add(new ProgressGradientStop { Color = Colors.Red, Value = 30 });
        healthBar.GradientStops.Add(new ProgressGradientStop { Color = Colors.Orange, Value = 30 });
        healthBar.GradientStops.Add(new ProgressGradientStop { Color = Colors.Orange, Value = 70 });
        healthBar.GradientStops.Add(new ProgressGradientStop { Color = Colors.Green, Value = 70 });
        healthBar.GradientStops.Add(new ProgressGradientStop { Color = Colors.Green, Value = 100 });

        return healthBar;
    }
}
```

## Best Practices

1. **Maintain consistency**: Use the same styling across similar progress indicators in your app
2. **Consider contrast**: Ensure progress is clearly visible against track background
3. **Match corner radius to height**: For pill shapes, use `radius = height / 2`
4. **Use opacity wisely**: Transparent tracks work well on colored backgrounds
5. **Test on all platforms**: Colors may render slightly differently on iOS, Android, Windows and mac OS.
6. **Accessibility**: Ensure sufficient color contrast for users with visual impairments
7. **Brand alignment**: Match colors to your app's design system

## Common Mistakes

❌ **Corner radius larger than height**: Creates distorted appearance
```csharp
// WRONG
TrackHeight = 10,
TrackCornerRadius = 15  // Larger than height!
```

✅ **Corner radius at most half the height**:
```csharp
// RIGHT
TrackHeight = 10,
TrackCornerRadius = 5  // Half the height for pill shape
```

❌ **Low contrast**: Progress invisible on track
```csharp
// WRONG: Both very similar colors
TrackFill = "#EEEEEE",
ProgressFill = "#E0E0E0"
```

✅ **Clear contrast**:
```csharp
// RIGHT: Clear visual distinction
TrackFill = "#E0E0E0",
ProgressFill = "#2196F3"
```