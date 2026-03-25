# Appearance Customization in Circular ProgressBar

Comprehensive guide to customizing the visual appearance of the Syncfusion .NET MAUI Circular ProgressBar including angles, colors, gradients, thickness, radius, and corner styles.

## Table of Contents
- [Angle Customization](#angle-customization)
- [Range Colors and Gradients](#range-colors-and-gradients)
- [Thickness and Radius](#thickness-and-radius)
- [Corner Style Customization](#corner-style-customization)
- [Color Customization](#color-customization)
- [Complete Styling Examples](#complete-styling-examples)

## Angle Customization

Customize the start and end angles to create semi-circles, arcs, or custom circular shapes.

### StartAngle and EndAngle Properties

- **StartAngle**: Starting angle in degrees (0-360)
- **EndAngle**: Ending angle in degrees (0-360)
- **Default**: StartAngle=0, EndAngle=360 (full circle)

### Common Angle Patterns

#### Full Circle (Default)
```xml
<progressBar:SfCircularProgressBar Progress="75" 
                                   StartAngle="0" 
                                   EndAngle="360" />
```

#### Semi-Circle (Bottom Half)
```xml
<progressBar:SfCircularProgressBar Progress="75" 
                                   StartAngle="180" 
                                   EndAngle="360" />
```

#### Semi-Circle (Top Half)
```xml
<progressBar:SfCircularProgressBar Progress="75" 
                                   StartAngle="0" 
                                   EndAngle="180" />
```

#### Three-Quarter Circle
```xml
<progressBar:SfCircularProgressBar Progress="75" 
                                   StartAngle="90" 
                                   EndAngle="360" />
```

#### Arc (Quarter Circle)
```xml
<progressBar:SfCircularProgressBar Progress="75" 
                                   StartAngle="0" 
                                   EndAngle="90" />
```

### Angle Visualization Guide

```
        0° (Top)
          |
  270° ---+--- 90°
          |
       180° (Bottom)
```

### C# Implementation

```csharp
// Semi-circle dashboard indicator
SfCircularProgressBar circularProgressBar = new SfCircularProgressBar
{
    Progress = 75,
    StartAngle = 180,
    EndAngle = 360
};
```

### Practical Angle Examples

**Dashboard Card (Semi-Circle):**
```xml
<progressBar:SfCircularProgressBar Progress="68" 
                                   StartAngle="180" 
                                   EndAngle="360"
                                   TrackFill="#3390a84e" 
                                   ProgressFill="#FF90a84e"
                                   HeightRequest="150"
                                   WidthRequest="200" />
```

**Speedometer Style:**
```xml
<progressBar:SfCircularProgressBar Progress="75" 
                                   StartAngle="135" 
                                   EndAngle="405"
                                   ProgressThickness="20"
                                   TrackThickness="20" />
```

## Range Colors and Gradients

Visualize multiple ranges with different colors to enhance readability and provide visual cues based on progress levels.

### GradientStops Property

The `GradientStops` property holds a collection of `ProgressGradientStop` objects that define color ranges.

**ProgressGradientStop Properties:**
- **Color**: The color for the specified range
- **Value**: Start or end value for the color (0-100 by default)

### Solid Color Ranges

Define distinct color blocks for different ranges.

```xml
<progressBar:SfCircularProgressBar Progress="100">
    <progressBar:SfCircularProgressBar.GradientStops>
        <!-- Range 1: 0-25% (Teal) -->
        <progressBar:ProgressGradientStop Color="#00bdaf" Value="0"/>
        <progressBar:ProgressGradientStop Color="#00bdaf" Value="25"/>
        
        <!-- Range 2: 25-50% (Blue) -->
        <progressBar:ProgressGradientStop Color="#2f7ecc" Value="25"/>
        <progressBar:ProgressGradientStop Color="#2f7ecc" Value="50"/>
        
        <!-- Range 3: 50-75% (Pink) -->
        <progressBar:ProgressGradientStop Color="#e9648e" Value="50"/>
        <progressBar:ProgressGradientStop Color="#e9648e" Value="75"/>
        
        <!-- Range 4: 75-100% (Orange) -->
        <progressBar:ProgressGradientStop Color="#fbb78a" Value="75"/>
        <progressBar:ProgressGradientStop Color="#fbb78a" Value="100"/>
    </progressBar:SfCircularProgressBar.GradientStops>
</progressBar:SfCircularProgressBar>
```

**C# Implementation:**
```csharp
SfCircularProgressBar circularProgressBar = new SfCircularProgressBar
{
    Progress = 100
};

circularProgressBar.GradientStops.Add(new ProgressGradientStop { Color = Color.FromArgb("#00bdaf"), Value = 0 });
circularProgressBar.GradientStops.Add(new ProgressGradientStop { Color = Color.FromArgb("#00bdaf"), Value = 25 });
circularProgressBar.GradientStops.Add(new ProgressGradientStop { Color = Color.FromArgb("#2f7ecc"), Value = 25 });
circularProgressBar.GradientStops.Add(new ProgressGradientStop { Color = Color.FromArgb("#2f7ecc"), Value = 50 });
circularProgressBar.GradientStops.Add(new ProgressGradientStop { Color = Color.FromArgb("#e9648e"), Value = 50 });
circularProgressBar.GradientStops.Add(new ProgressGradientStop { Color = Color.FromArgb("#e9648e"), Value = 75 });
circularProgressBar.GradientStops.Add(new ProgressGradientStop { Color = Color.FromArgb("#fbb78a"), Value = 75 });
circularProgressBar.GradientStops.Add(new ProgressGradientStop { Color = Color.FromArgb("#fbb78a"), Value = 100 });
```

### Gradient Transitions

Create smooth color transitions between ranges.

```xml
<progressBar:SfCircularProgressBar Progress="100">
    <progressBar:SfCircularProgressBar.GradientStops>
        <progressBar:ProgressGradientStop Color="#00bdaf" Value="0"/>
        <progressBar:ProgressGradientStop Color="#2f7ecc" Value="25"/>
        <progressBar:ProgressGradientStop Color="#e9648e" Value="50"/>
        <progressBar:ProgressGradientStop Color="#fbb78a" Value="75"/>
    </progressBar:SfCircularProgressBar.GradientStops>
</progressBar:SfCircularProgressBar>
```

**Note:** When only one color is specified per value, colors blend smoothly between stops.

### Status-Based Color Ranges

**Example: Low/Medium/High Indicators**

```xml
<progressBar:SfCircularProgressBar Progress="85">
    <progressBar:SfCircularProgressBar.GradientStops>
        <!-- Red: 0-33% (Low) -->
        <progressBar:ProgressGradientStop Color="#FF4444" Value="0"/>
        <progressBar:ProgressGradientStop Color="#FF4444" Value="33"/>
        
        <!-- Yellow: 33-66% (Medium) -->
        <progressBar:ProgressGradientStop Color="#FFBB33" Value="33"/>
        <progressBar:ProgressGradientStop Color="#FFBB33" Value="66"/>
        
        <!-- Green: 66-100% (High) -->
        <progressBar:ProgressGradientStop Color="#00C851" Value="66"/>
        <progressBar:ProgressGradientStop Color="#00C851" Value="100"/>
    </progressBar:SfCircularProgressBar.GradientStops>
</progressBar:SfCircularProgressBar>
```

## Thickness and Radius

Control the size and thickness of progress and track indicators.

### Thickness Properties

- **ProgressThickness**: Thickness of progress indicator
- **TrackThickness**: Thickness of track (background)
- **ThicknessUnit**: Pixel or Factor

### Radius Properties

- **ProgressRadiusFactor**: Outer radius of progress (0-1)
- **TrackRadiusFactor**: Outer radius of track (0-1)

### ThicknessUnit: Pixel vs Factor

#### Pixel Mode
Exact pixel values for thickness.

```xml
<progressBar:SfCircularProgressBar Progress="75"
                                   ThicknessUnit="Pixel"
                                   ProgressThickness="20"
                                   TrackThickness="20" />
```

#### Factor Mode
Percentage of outer radius (0-1).

```xml
<progressBar:SfCircularProgressBar Progress="75"
                                   ThicknessUnit="Factor"
                                   ProgressThickness="0.1"
                                   TrackThickness="0.1" />
```

**Factor Calculation:**
- Factor 0.1 = 10% of outer radius
- If radius is 100px, thickness = 10px
- If radius is 200px, thickness = 20px

### Complete Thickness Example

```xml
<progressBar:SfCircularProgressBar Progress="75"
                                   TrackRadiusFactor="0.8" 
                                   ProgressRadiusFactor="0.75"
                                   ThicknessUnit="Factor"
                                   TrackThickness="0.05"
                                   ProgressThickness="0.05" />
```

**C# Implementation:**
```csharp
SfCircularProgressBar circularProgressBar = new SfCircularProgressBar
{
    Progress = 75,
    TrackRadiusFactor = 0.8,
    ProgressRadiusFactor = 0.75,
    TrackThickness = 0.05,
    ProgressThickness = 0.05,
    ThicknessUnit = SizeUnit.Factor
};
```

### Thin vs Thick Progress Bars

**Thin Progress Bar (Factor 0.03):**
```xml
<progressBar:SfCircularProgressBar Progress="75"
                                   ThicknessUnit="Factor"
                                   ProgressThickness="0.03"
                                   TrackThickness="0.03" />
```

**Thick Progress Bar (Factor 0.15):**
```xml
<progressBar:SfCircularProgressBar Progress="75"
                                   ThicknessUnit="Factor"
                                   ProgressThickness="0.15"
                                   TrackThickness="0.15" />
```

**Fixed Pixel Thickness:**
```xml
<progressBar:SfCircularProgressBar Progress="75"
                                   ThicknessUnit="Pixel"
                                   ProgressThickness="25"
                                   TrackThickness="25" />
```

## Corner Style Customization

Customize the corner shape of progress and track indicators.

### Corner Style Options

- **BothFlat**: Flat corners on both ends (default)
- **BothCurve**: Rounded corners on both ends
- **StartCurve**: Rounded start corner, flat end
- **EndCurve**: Flat start corner, rounded end

### Properties

- **ProgressCornerStyle**: Corner style for progress indicator
- **TrackCornerStyle**: Corner style for track

### BothFlat (Default)

```xml
<progressBar:SfCircularProgressBar Progress="75"
                                   ProgressCornerStyle="BothFlat"
                                   TrackCornerStyle="BothFlat" />
```

### BothCurve (Rounded)

```xml
<progressBar:SfCircularProgressBar Progress="50"
                                   TrackCornerStyle="BothCurve"
                                   ProgressCornerStyle="BothCurve"
                                   StartAngle="180"
                                   EndAngle="360" />
```

**C# Implementation:**
```csharp
SfCircularProgressBar circularProgressBar = new SfCircularProgressBar
{
    Progress = 50,
    TrackCornerStyle = CornerStyle.BothCurve,
    ProgressCornerStyle = CornerStyle.BothCurve,
    StartAngle = 180,
    EndAngle = 360
};
```

### Mixed Corner Styles

```xml
<progressBar:SfCircularProgressBar Progress="75"
                                   ProgressCornerStyle="StartCurve"
                                   TrackCornerStyle="BothFlat" />
```

### Modern Rounded Design

```xml
<progressBar:SfCircularProgressBar Progress="65"
                                   ProgressCornerStyle="BothCurve"
                                   TrackCornerStyle="BothCurve"
                                   ProgressThickness="15"
                                   TrackThickness="15"
                                   ProgressFill="#FF2196F3"
                                   TrackFill="#332196F3" />
```

## Color Customization

Simple color customization for progress and track.

### Properties

- **ProgressFill**: Color/brush for progress indicator
- **TrackFill**: Color/brush for track background

### Basic Color Customization

```xml
<progressBar:SfCircularProgressBar Progress="75"
                                   TrackFill="#3351483a" 
                                   ProgressFill="#FF51483a" />
```

**C# Implementation:**
```csharp
SfCircularProgressBar circularProgressBar = new SfCircularProgressBar
{
    Progress = 75,
    TrackFill = Color.FromArgb("#3351483a"),
    ProgressFill = Color.FromArgb("#FF51483a")
};
```

### Color with Transparency

Use alpha channel for transparency (first 2 hex digits).

- **#FF** = Fully opaque (255)
- **#33** = 20% opacity (~51)
- **#80** = 50% opacity (128)

```xml
<progressBar:SfCircularProgressBar Progress="60"
                                   TrackFill="#33c15244"   <!-- 20% opacity -->
                                   ProgressFill="#FFc15244" /> <!-- Fully opaque -->
```

### Predefined Colors

```csharp
SfCircularProgressBar circularProgressBar = new SfCircularProgressBar
{
    Progress = 75,
    TrackFill = new SolidColorBrush(Colors.LightGray),
    ProgressFill = new SolidColorBrush(Colors.Blue)
};
```

## Complete Styling Examples

### Example 1: Modern Dashboard Card

```xml
<progressBar:SfCircularProgressBar Progress="73"
                                   StartAngle="180"
                                   EndAngle="360"
                                   ProgressCornerStyle="BothCurve"
                                   TrackCornerStyle="BothCurve"
                                   ThicknessUnit="Pixel"
                                   ProgressThickness="18"
                                   TrackThickness="18"
                                   ProgressFill="#FF667eea"
                                   TrackFill="#33667eea"
                                   HeightRequest="150"
                                   WidthRequest="200">
    <progressBar:SfCircularProgressBar.Content>
        <StackLayout Spacing="5">
            <Label Text="73%"
                   FontSize="28"
                   FontAttributes="Bold"
                   HorizontalTextAlignment="Center" />
            <Label Text="Completion"
                   FontSize="14"
                   TextColor="Gray"
                   HorizontalTextAlignment="Center" />
        </StackLayout>
    </progressBar:SfCircularProgressBar.Content>
</progressBar:SfCircularProgressBar>
```

### Example 2: Health Monitor (Gradient Colors)

```xml
<progressBar:SfCircularProgressBar Progress="82">
    <progressBar:SfCircularProgressBar.GradientStops>
        <progressBar:ProgressGradientStop Color="#FF0000" Value="0"/>   <!-- Red (Critical) -->
        <progressBar:ProgressGradientStop Color="#FFA500" Value="40"/>  <!-- Orange (Warning) -->
        <progressBar:ProgressGradientStop Color="#FFD700" Value="60"/>  <!-- Gold (Caution) -->
        <progressBar:ProgressGradientStop Color="#00FF00" Value="80"/>  <!-- Green (Good) -->
        <progressBar:ProgressGradientStop Color="#00C851" Value="100"/> <!-- Dark Green (Excellent) -->
    </progressBar:SfCircularProgressBar.GradientStops>
</progressBar:SfCircularProgressBar>
```

### Example 3: Thick Arc with Custom Angles

```xml
<progressBar:SfCircularProgressBar Progress="68"
                                   StartAngle="135"
                                   EndAngle="405"
                                   ThicknessUnit="Pixel"
                                   ProgressThickness="30"
                                   TrackThickness="30"
                                   ProgressCornerStyle="BothCurve"
                                   TrackCornerStyle="BothCurve"
                                   ProgressFill="#FFE91E63"
                                   TrackFill="#33E91E63" />
```

### Example 4: Minimalist Thin Ring

```xml
<progressBar:SfCircularProgressBar Progress="45"
                                   ThicknessUnit="Factor"
                                   ProgressThickness="0.02"
                                   TrackThickness="0.02"
                                   ProgressRadiusFactor="0.9"
                                   TrackRadiusFactor="0.9"
                                   ProgressFill="#FF000000"
                                   TrackFill="#33000000" />
```

### Example 5: Multi-Color Segmented Progress

```xml
<progressBar:SfCircularProgressBar Progress="100"
                                   SegmentCount="4"
                                   SegmentGapWidth="8"
                                   ProgressThickness="20"
                                   TrackThickness="20">
    <progressBar:SfCircularProgressBar.GradientStops>
        <progressBar:ProgressGradientStop Color="#FF6B4CE6" Value="0"/>
        <progressBar:ProgressGradientStop Color="#FF6B4CE6" Value="25"/>
        <progressBar:ProgressGradientStop Color="#FF44B4D5" Value="25"/>
        <progressBar:ProgressGradientStop Color="#FF44B4D5" Value="50"/>
        <progressBar:ProgressGradientStop Color="#FF4CAF50" Value="50"/>
        <progressBar:ProgressGradientStop Color="#FF4CAF50" Value="75"/>
        <progressBar:ProgressGradientStop Color="#FFFFC107" Value="75"/>
        <progressBar:ProgressGradientStop Color="#FFFFC107" Value="100"/>
    </progressBar:SfCircularProgressBar.GradientStops>
</progressBar:SfCircularProgressBar>
```

## Best Practices

1. **Angle Selection**:
   - Use semi-circles (180-360°) for dashboard cards
   - Use full circles for complete progress indicators
   - Use arcs (90° range) for gauges and meters

2. **Color Contrast**:
   - Ensure 20-30% opacity for track fill
   - Use fully opaque colors for progress fill
   - Test colors in light and dark themes

3. **Thickness Guidelines**:
   - Use Factor mode for responsive designs
   - Use Pixel mode for consistent thickness across sizes
   - Thin (0.02-0.05) for minimal designs
   - Medium (0.08-0.12) for standard use
   - Thick (0.15-0.20) for emphasis

4. **Corner Styles**:
   - Use BothCurve for modern, friendly designs
   - Use BothFlat for technical, precise indicators
   - Match corner style to overall app design language

5. **Gradient Colors**:
   - Use 3-5 color stops for clear visual ranges
   - Apply semantic colors (red=low, yellow=medium, green=high)
   - Consider accessibility and color blindness

6. **Performance**:
   - Limit gradient stops to 8 or fewer
   - Use simple colors for indeterminate state
   - Test animation performance with complex gradients