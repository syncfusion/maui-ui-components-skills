# Appearance and Customization in Pyramid Charts

## Table of Contents
- [Overview](#overview)
- [Custom Palette Brushes](#custom-palette-brushes)
- [Applying Gradients](#applying-gradients)
- [Pyramid Modes](#pyramid-modes)
- [Segment Spacing](#segment-spacing)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)

## Overview

The SfPyramidChart provides extensive appearance customization options including custom color palettes, gradient fills, rendering modes, and segment spacing. These features allow you to create visually stunning and professional pyramid charts that match your application's design language.

**Key Customization Features:**
- Custom color palettes with your brand colors
- Linear and radial gradient fills for modern aesthetics
- Two rendering modes (Linear and Surface)
- Adjustable segment spacing for visual clarity
- Complete control over visual presentation

## Custom Palette Brushes

Define custom colors for pyramid segments using the `PaletteBrushes` property. This property accepts a collection of `Brush` objects that are applied to segments in order.

### Basic Custom Colors

**ViewModel with Custom Brushes:**
```csharp
public class ViewModel
{
    public ObservableCollection<StageModel> Data { get; set; }
    public List<Brush> CustomBrushes { get; set; }
    
    public ViewModel()
    {
        // Create data
        Data = new ObservableCollection<StageModel>()
        {
            new StageModel() { Name = "Stage A", Value = 40 },
            new StageModel() { Name = "Stage B", Value = 30 },
            new StageModel() { Name = "Stage C", Value = 20 },
            new StageModel() { Name = "Stage D", Value = 10 }
        };
        
        // Create custom color palette
        CustomBrushes = new List<Brush>()
        {
            new SolidColorBrush(Color.FromRgb(38, 198, 218)),   // Cyan
            new SolidColorBrush(Color.FromRgb(0, 188, 212)),    // Light Blue
            new SolidColorBrush(Color.FromRgb(0, 172, 193)),    // Medium Blue
            new SolidColorBrush(Color.FromRgb(0, 151, 167)),    // Dark Blue
            new SolidColorBrush(Color.FromRgb(0, 131, 143))     // Navy Blue
        };
    }
}
```

**XAML:**
```xaml
<chart:SfPyramidChart ItemsSource="{Binding Data}" 
                      XBindingPath="Name"
                      YBindingPath="Value"
                      PaletteBrushes="{Binding CustomBrushes}"/>
```

**C#:**
```csharp
SfPyramidChart chart = new SfPyramidChart();
ViewModel viewModel = new ViewModel();
chart.BindingContext = viewModel;
chart.ItemsSource = viewModel.Data;
chart.XBindingPath = "Name";
chart.YBindingPath = "Value";
chart.PaletteBrushes = viewModel.CustomBrushes;
this.Content = chart;
```

### Color Palette Examples

**Warm Color Palette:**
```csharp
CustomBrushes = new List<Brush>()
{
    new SolidColorBrush(Color.FromArgb("#FF6B6B")),  // Red
    new SolidColorBrush(Color.FromArgb("#FFA06B")),  // Orange
    new SolidColorBrush(Color.FromArgb("#FFD93D")),  // Yellow
    new SolidColorBrush(Color.FromArgb("#FFB26B")),  // Light Orange
    new SolidColorBrush(Color.FromArgb("#FF8B8B"))   // Light Red
};
```

**Cool Color Palette:**
```csharp
CustomBrushes = new List<Brush>()
{
    new SolidColorBrush(Color.FromArgb("#667EEA")),  // Purple
    new SolidColorBrush(Color.FromArgb("#764BA2")),  // Deep Purple
    new SolidColorBrush(Color.FromArgb("#4A69BD")),  // Blue
    new SolidColorBrush(Color.FromArgb("#3C6382")),  // Navy
    new SolidColorBrush(Color.FromArgb("#1B262C"))   // Dark Navy
};
```

**Professional Palette:**
```csharp
CustomBrushes = new List<Brush>()
{
    new SolidColorBrush(Color.FromArgb("#2C3E50")),  // Dark Gray
    new SolidColorBrush(Color.FromArgb("#34495E")),  // Slate
    new SolidColorBrush(Color.FromArgb("#3498DB")),  // Blue
    new SolidColorBrush(Color.FromArgb("#5DADE2")),  // Light Blue
    new SolidColorBrush(Color.FromArgb("#85C1E9"))   // Sky Blue
};
```

**Nature-Inspired Palette:**
```csharp
CustomBrushes = new List<Brush>()
{
    new SolidColorBrush(Color.FromArgb("#27AE60")),  // Green
    new SolidColorBrush(Color.FromArgb("#52BE80")),  // Light Green
    new SolidColorBrush(Color.FromArgb("#7DCEA0")),  // Mint
    new SolidColorBrush(Color.FromArgb("#A9DFBF")),  // Pale Green
    new SolidColorBrush(Color.FromArgb("#D5F4E6"))   // Very Light Green
};
```

## Applying Gradients

Create stunning visual effects using `LinearGradientBrush` or `RadialGradientBrush` in the `PaletteBrushes` collection.

### Linear Gradient Basics

**Single Gradient Example:**
```csharp
LinearGradientBrush gradient = new LinearGradientBrush();
gradient.GradientStops = new GradientStopCollection()
{
    new GradientStop() { Offset = 0, Color = Color.FromArgb("#6991c7") },  // Start color
    new GradientStop() { Offset = 1, Color = Color.FromArgb("#a3bded") }   // End color
};
```

### Multiple Gradients in ViewModel

**ViewModel with Gradient Brushes:**
```csharp
public class ViewModel
{
    public ObservableCollection<StageModel> Data { get; set; }
    public List<Brush> CustomBrushes { get; set; }
    
    public ViewModel()
    {
        Data = new ObservableCollection<StageModel>()
        {
            new StageModel() { Name = "Stage A", Value = 40 },
            new StageModel() { Name = "Stage B", Value = 30 },
            new StageModel() { Name = "Stage C", Value = 20 },
            new StageModel() { Name = "Stage D", Value = 15 },
            new StageModel() { Name = "Stage E", Value = 10 }
        };
        
        CustomBrushes = new List<Brush>();
        
        // Gradient 1: Blue to Light Blue
        LinearGradientBrush gradientColor1 = new LinearGradientBrush();
        gradientColor1.GradientStops = new GradientStopCollection()
        {
            new GradientStop() { Offset = 1, Color = Color.FromArgb("#a3bded") },
            new GradientStop() { Offset = 0, Color = Color.FromArgb("#6991c7") }
        };
        
        // Gradient 2: Purple to Pink
        LinearGradientBrush gradientColor2 = new LinearGradientBrush();
        gradientColor2.GradientStops = new GradientStopCollection()
        {
            new GradientStop() { Offset = 1, Color = Color.FromArgb("#A5678E") },
            new GradientStop() { Offset = 0, Color = Color.FromArgb("#E8B7D4") }
        };
        
        // Gradient 3: Pink to Rose
        LinearGradientBrush gradientColor3 = new LinearGradientBrush();
        gradientColor3.GradientStops = new GradientStopCollection()
        {
            new GradientStop() { Offset = 1, Color = Color.FromArgb("#FFCAD4") },
            new GradientStop() { Offset = 0, Color = Color.FromArgb("#FB7B8E") }
        };
        
        // Gradient 4: Orange to Peach
        LinearGradientBrush gradientColor4 = new LinearGradientBrush();
        gradientColor4.GradientStops = new GradientStopCollection()
        {
            new GradientStop() { Offset = 1, Color = Color.FromArgb("#FDC094") },
            new GradientStop() { Offset = 0, Color = Color.FromArgb("#FFE5D8") }
        };
        
        // Gradient 5: Green to Light Green
        LinearGradientBrush gradientColor5 = new LinearGradientBrush();
        gradientColor5.GradientStops = new GradientStopCollection()
        {
            new GradientStop() { Offset = 1, Color = Color.FromArgb("#CFF4D2") },
            new GradientStop() { Offset = 0, Color = Color.FromArgb("#56C596") }
        };
        
        CustomBrushes.Add(gradientColor1);
        CustomBrushes.Add(gradientColor2);
        CustomBrushes.Add(gradientColor3);
        CustomBrushes.Add(gradientColor4);
        CustomBrushes.Add(gradientColor5);
    }
}
```

**XAML:**
```xaml
<chart:SfPyramidChart ItemsSource="{Binding Data}" 
                      XBindingPath="Name"
                      YBindingPath="Value"
                      PaletteBrushes="{Binding CustomBrushes}"/>
```

### Gradient Direction Control

**Vertical Gradient (Top to Bottom):**
```csharp
LinearGradientBrush gradient = new LinearGradientBrush()
{
    StartPoint = new Point(0, 0),
    EndPoint = new Point(0, 1),
    GradientStops = new GradientStopCollection()
    {
        new GradientStop() { Offset = 0, Color = Colors.DarkBlue },
        new GradientStop() { Offset = 1, Color = Colors.LightBlue }
    }
};
```

**Horizontal Gradient (Left to Right):**
```csharp
LinearGradientBrush gradient = new LinearGradientBrush()
{
    StartPoint = new Point(0, 0),
    EndPoint = new Point(1, 0),
    GradientStops = new GradientStopCollection()
    {
        new GradientStop() { Offset = 0, Color = Colors.Red },
        new GradientStop() { Offset = 1, Color = Colors.Orange }
    }
};
```

**Diagonal Gradient:**
```csharp
LinearGradientBrush gradient = new LinearGradientBrush()
{
    StartPoint = new Point(0, 0),
    EndPoint = new Point(1, 1),
    GradientStops = new GradientStopCollection()
    {
        new GradientStop() { Offset = 0, Color = Colors.Purple },
        new GradientStop() { Offset = 1, Color = Colors.Pink }
    }
};
```

### Multi-Stop Gradients

Create complex color transitions with multiple gradient stops:

```csharp
LinearGradientBrush multiStopGradient = new LinearGradientBrush();
multiStopGradient.GradientStops = new GradientStopCollection()
{
    new GradientStop() { Offset = 0.0, Color = Color.FromArgb("#FF0000") },  // Red
    new GradientStop() { Offset = 0.25, Color = Color.FromArgb("#FF7F00") }, // Orange
    new GradientStop() { Offset = 0.5, Color = Color.FromArgb("#FFFF00") },  // Yellow
    new GradientStop() { Offset = 0.75, Color = Color.FromArgb("#00FF00") }, // Green
    new GradientStop() { Offset = 1.0, Color = Color.FromArgb("#0000FF") }   // Blue
};
```

### Radial Gradient (Advanced)

```csharp
RadialGradientBrush radialGradient = new RadialGradientBrush()
{
    Center = new Point(0.5, 0.5),
    Radius = 0.5,
    GradientStops = new GradientStopCollection()
    {
        new GradientStop() { Offset = 0, Color = Colors.Yellow },
        new GradientStop() { Offset = 1, Color = Colors.Red }
    }
};
```

## Pyramid Modes

The `Mode` property determines how segment sizes are calculated. Two modes are available: **Linear** and **Surface**.

### Mode Comparison

| Mode | Description | Calculation Basis |
|------|-------------|------------------|
| **Linear** | Height-based sizing (default) | Segment height proportional to Y value |
| **Surface** | Area-based sizing | Segment area proportional to Y value |

### Linear Mode (Default)

In Linear mode, the height of each pyramid segment is directly proportional to its Y value.

**XAML:**
```xaml
<chart:SfPyramidChart ItemsSource="{Binding Data}" 
                      XBindingPath="Name" 
                      YBindingPath="Value"
                      Mode="Linear"/>
```

**C#:**
```csharp
SfPyramidChart chart = new SfPyramidChart();
chart.ItemsSource = viewModel.Data;
chart.XBindingPath = "Name";
chart.YBindingPath = "Value";
chart.Mode = PyramidMode.Linear;
this.Content = chart;
```

**Best For:**
- Emphasizing differences in values through visual height
- When linear progression is important
- Standard pyramid visualization

### Surface Mode

In Surface mode, the surface area of each pyramid segment is proportional to its Y value, creating a more dramatic visual effect for value differences.

**XAML:**
```xaml
<chart:SfPyramidChart ItemsSource="{Binding Data}" 
                      XBindingPath="Name" 
                      YBindingPath="Value"
                      Mode="Surface"/>
```

**C#:**
```csharp
SfPyramidChart chart = new SfPyramidChart();
chart.ItemsSource = viewModel.Data;
chart.XBindingPath = "Name";
chart.YBindingPath = "Value";
chart.Mode = PyramidMode.Surface;
this.Content = chart;
```

**Best For:**
- Emphasizing proportional relationships
- When area representation is more meaningful
- Creating dramatic visual differences

### Mode Selection Guide

**Use Linear Mode when:**
- Values are close in range
- You want consistent visual progression
- Height-based comparison is intuitive for your data
- Following standard pyramid chart conventions

**Use Surface Mode when:**
- Values have large variations
- You want to emphasize proportional differences
- Area-based representation fits your use case
- Creating more dramatic visualizations

## Segment Spacing

Control the gap between pyramid segments using the `GapRatio` property. Value ranges from `0` (no gap) to `1` (maximum gap). Default is `0`.

### Basic Segment Spacing

**XAML:**
```xaml
<chart:SfPyramidChart ItemsSource="{Binding Data}"
                      XBindingPath="Name"
                      YBindingPath="Value"
                      GapRatio="0.2"/>
```

**C#:**
```csharp
SfPyramidChart chart = new SfPyramidChart();
chart.ItemsSource = viewModel.Data;
chart.XBindingPath = "Name";
chart.YBindingPath = "Value";
chart.GapRatio = 0.2;  // 20% gap between segments
this.Content = chart;
```

### GapRatio Value Guidelines

| GapRatio | Visual Effect | Best For |
|----------|--------------|----------|
| **0** | No spacing (default) | Traditional pyramid, continuous flow |
| **0.1** | Subtle separation | Slight visual distinction |
| **0.2** | Moderate spacing | Balanced appearance |
| **0.3-0.4** | Clear separation | Emphasizing individual segments |
| **0.5+** | Wide spacing | Highly separated segments |

### Spacing Examples

**No Spacing:**
```csharp
chart.GapRatio = 0;  // Segments touch each other
```

**Subtle Spacing:**
```csharp
chart.GapRatio = 0.1;  // 10% gap
```

**Moderate Spacing:**
```csharp
chart.GapRatio = 0.2;  // 20% gap (recommended for most cases)
```

**Wide Spacing:**
```csharp
chart.GapRatio = 0.4;  // 40% gap for dramatic separation
```

## Complete Examples

### Example 1: Gradient Pyramid with Spacing

```xaml
<ContentPage xmlns:chart="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts"
             xmlns:local="clr-namespace:YourNamespace">
    
    <chart:SfPyramidChart ItemsSource="{Binding Data}" 
                          XBindingPath="Name"
                          YBindingPath="Value"
                          PaletteBrushes="{Binding CustomBrushes}"
                          Mode="Linear"
                          GapRatio="0.15"
                          ShowDataLabels="True">
        
        <chart:SfPyramidChart.BindingContext>
            <local:ViewModel/>
        </chart:SfPyramidChart.BindingContext>
        
        <chart:SfPyramidChart.Title>
            <Label Text="Sales Funnel" FontSize="18" FontAttributes="Bold"/>
        </chart:SfPyramidChart.Title>
        
        <chart:SfPyramidChart.Legend>
            <chart:ChartLegend Placement="Bottom"/>
        </chart:SfPyramidChart.Legend>
        
    </chart:SfPyramidChart>
    
</ContentPage>
```

### Example 2: Surface Mode with Custom Colors

```csharp
SfPyramidChart chart = new SfPyramidChart();

// Configure chart
chart.ItemsSource = viewModel.Data;
chart.XBindingPath = "Name";
chart.YBindingPath = "Value";
chart.Mode = PyramidMode.Surface;
chart.GapRatio = 0.1;

// Custom color palette
var customColors = new List<Brush>()
{
    new SolidColorBrush(Color.FromArgb("#E74C3C")),
    new SolidColorBrush(Color.FromArgb("#E67E22")),
    new SolidColorBrush(Color.FromArgb("#F39C12")),
    new SolidColorBrush(Color.FromArgb("#27AE60")),
    new SolidColorBrush(Color.FromArgb("#3498DB"))
};
chart.PaletteBrushes = customColors;

// Add features
chart.ShowDataLabels = true;
chart.EnableTooltip = true;
chart.Legend = new ChartLegend() { Placement = LegendPlacement.Right };

this.Content = chart;
```

### Example 3: Multi-Gradient with Wide Spacing

```csharp
public class ViewModel
{
    public ObservableCollection<StageModel> Data { get; set; }
    public List<Brush> CustomBrushes { get; set; }
    
    public ViewModel()
    {
        Data = new ObservableCollection<StageModel>()
        {
            new StageModel() { Name = "Awareness", Value = 1000 },
            new StageModel() { Name = "Interest", Value = 750 },
            new StageModel() { Name = "Consideration", Value = 500 },
            new StageModel() { Name = "Intent", Value = 250 },
            new StageModel() { Name = "Conversion", Value = 100 }
        };
        
        CustomBrushes = CreateGradientPalette();
    }
    
    private List<Brush> CreateGradientPalette()
    {
        return new List<Brush>()
        {
            CreateGradient("#667EEA", "#764BA2"),
            CreateGradient("#F093FB", "#F5576C"),
            CreateGradient("#4FACFE", "#00F2FE"),
            CreateGradient("#43E97B", "#38F9D7"),
            CreateGradient("#FA709A", "#FEE140")
        };
    }
    
    private LinearGradientBrush CreateGradient(string startColor, string endColor)
    {
        return new LinearGradientBrush()
        {
            GradientStops = new GradientStopCollection()
            {
                new GradientStop() { Offset = 0, Color = Color.FromArgb(startColor) },
                new GradientStop() { Offset = 1, Color = Color.FromArgb(endColor) }
            }
        };
    }
}
```

```xaml
<chart:SfPyramidChart ItemsSource="{Binding Data}"
                      XBindingPath="Name"
                      YBindingPath="Value"
                      PaletteBrushes="{Binding CustomBrushes}"
                      Mode="Linear"
                      GapRatio="0.25"/>
```

## Best Practices

### Color Selection

- **Brand Consistency**: Use brand colors in your custom palette
- **Contrast**: Ensure segments are distinguishable from each other
- **Accessibility**: Consider color-blind friendly palettes
- **Context**: Match colors to data meaning (red for decline, green for growth)

### Gradient Usage

- **Direction**: Vertical gradients work well with pyramid orientation
- **Subtlety**: Subtle gradients often look more professional
- **Consistency**: Use similar gradient patterns across segments
- **Performance**: Gradients have minimal performance impact

### Mode Selection

- **Data Range**: Use Surface mode for data with large value variations
- **Visualization Goal**: Linear for height comparison, Surface for area emphasis
- **User Familiarity**: Linear is more traditional and easier to interpret

### Segment Spacing

- **Balance**: 0.1-0.2 provides good visual separation without fragmentation
- **Context**: Use wider spacing (0.3-0.4) when segments represent distinct phases
- **Data Labels**: More spacing helps accommodate outer data labels
- **Print/Export**: Consider spacing for clarity in static images

### Performance Considerations

- Custom brushes and gradients have negligible performance impact
- Complex multi-stop gradients render efficiently
- Test on target devices for optimal appearance
- Use solid colors if performance becomes a concern (rare)

## Common Issues and Solutions

### Issue: Gradients Not Showing

**Solutions:**
- Verify GradientStops collection is properly initialized
- Check Offset values are between 0 and 1
- Ensure colors are valid (use Color.FromArgb or Color.FromRgb)
- Confirm ViewModel property is correctly bound

### Issue: Colors Not Applying

**Solutions:**
- Check PaletteBrushes binding path
- Verify ViewModel implements INotifyPropertyChanged if dynamic
- Ensure CustomBrushes list has enough colors for all segments
- Test with simple SolidColorBrush first

### Issue: Spacing Too Large or Small

**Solutions:**
- Adjust GapRatio value (recommend starting at 0.2)
- Test different values to find optimal spacing
- Consider chart size when setting spacing
- Balance spacing with data label placement

## Related Features

- **Data Labels**: See [data-labels.md](data-labels.md) for UseSeriesPalette option
- **Orientation**: See [orientation-and-effects.md](orientation-and-effects.md) for layout options
- **Legend**: See [legend.md](legend.md) for color coordination