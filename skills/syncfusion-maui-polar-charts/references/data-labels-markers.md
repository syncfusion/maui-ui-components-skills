# Data Labels and Markers

## Table of Contents
- [Overview](#overview)
- [Data Labels](#data-labels)
- [Markers](#markers)
- [Combining Labels and Markers](#combining-labels-and-markers)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

Data labels and markers enhance polar charts by:
- **Data Labels** - Display exact values at data points
- **Markers** - Highlight data point locations with visual symbols

Both features improve chart readability and help users identify specific values quickly.

## Data Labels

Data labels display values or percentages at each data point.

### Enable Data Labels

```csharp
PolarLineSeries series = new PolarLineSeries
{
    ItemsSource = data,
    XBindingPath = "Category",
    YBindingPath = "Value",
    ShowDataLabels = true  // Enable data labels
};
```

**XAML:**
```xml
<chart:PolarLineSeries ItemsSource="{Binding PlantDetails}"
                       XBindingPath="Direction"
                       YBindingPath="Tree"
                       ShowDataLabels="True"/>
```

### Customize Data Labels

Use `DataLabelSettings` property with `PolarDataLabelSettings`:

```csharp
PolarLineSeries series = new PolarLineSeries
{
    ItemsSource = data,
    XBindingPath = "Category",
    YBindingPath = "Value",
    ShowDataLabels = true
};

series.DataLabelSettings = new PolarDataLabelSettings
{
    LabelStyle = new ChartDataLabelStyle
    {
        TextColor = Colors.Blue,
        FontSize = 12,
        FontAttributes = FontAttributes.Bold,
        Background = new SolidColorBrush(Colors.White),
        Margin = new Thickness(5)
    }
};
```

**XAML:**
```xml
<chart:PolarLineSeries ShowDataLabels="True">
    <chart:PolarLineSeries.DataLabelSettings>
        <chart:PolarDataLabelSettings>
            <chart:PolarDataLabelSettings.LabelStyle>
                <chart:ChartDataLabelStyle TextColor="Blue"
                                           FontSize="12"
                                           FontAttributes="Bold"
                                           Background="White"
                                           Margin="5"/>
            </chart:PolarDataLabelSettings.LabelStyle>
        </chart:PolarDataLabelSettings>
    </chart:PolarLineSeries.DataLabelSettings>
</chart:PolarLineSeries>
```

### Use Series Palette

Apply series color to data label background:

```csharp
series.DataLabelSettings = new PolarDataLabelSettings
{
    UseSeriesPalette = true  // Use series color for label background
};
```

**XAML:**
```xml
<chart:PolarDataLabelSettings UseSeriesPalette="True"/>
```

### Label Context

Control what value is displayed:

```csharp
PolarAreaSeries series = new PolarAreaSeries
{
    ItemsSource = data,
    XBindingPath = "Category",
    YBindingPath = "Value",
    ShowDataLabels = true,
    LabelContext = LabelContext.YValue  // Show Y value (default)
};
```

**Available options:**
- `LabelContext.YValue` - Show Y-axis value (default)
- `LabelContext.Percentage` - Show percentage of total

**XAML:**
```xml
<chart:PolarAreaSeries ShowDataLabels="True" LabelContext="Percentage"/>
```

### Custom Label Template

Create custom data label layouts:

```xml
<chart:SfPolarChart>
    <chart:SfPolarChart.Resources>
        <DataTemplate x:Key="labelTemplate">
            <HorizontalStackLayout Spacing="5">
                <Label Text="{Binding Item.Values}" 
                       VerticalOptions="Center" 
                       FontSize="15"
                       TextColor="White"/>
                <Image Source="arrow.png" 
                       WidthRequest="15" 
                       HeightRequest="15"/>
            </HorizontalStackLayout>
        </DataTemplate>
    </chart:SfPolarChart.Resources>
    
    <chart:PolarAreaSeries ItemsSource="{Binding Data}"
                           XBindingPath="Category"
                           YBindingPath="Values"
                           ShowDataLabels="True"
                           LabelTemplate="{StaticResource labelTemplate}"/>
</chart:SfPolarChart>
```

## Markers

Markers are symbols that highlight data point positions.

### Enable Markers

```csharp
PolarLineSeries series = new PolarLineSeries
{
    ItemsSource = data,
    XBindingPath = "Category",
    YBindingPath = "Value",
    ShowMarkers = true  // Enable markers
};
```

**XAML:**
```xml
<chart:PolarLineSeries ItemsSource="{Binding PlantDetails}"
                       XBindingPath="Direction"
                       YBindingPath="Tree"
                       ShowMarkers="True"/>
```

### Customize Marker Appearance

Use `MarkerSettings` to customize marker appearance:

```csharp
PolarLineSeries series = new PolarLineSeries
{
    ItemsSource = data,
    XBindingPath = "Category",
    YBindingPath = "Value",
    ShowMarkers = true
};

series.MarkerSettings = new ChartMarkerSettings
{
    Type = ShapeType.Diamond,           // Marker shape
    Fill = Colors.Red,                   // Fill color
    Stroke = Colors.DarkRed,            // Border color
    StrokeWidth = 2,                     // Border thickness
    Width = 12,                          // Marker width
    Height = 12                          // Marker height
};
```

**XAML:**
```xml
<chart:PolarLineSeries ShowMarkers="True">
    <chart:PolarLineSeries.MarkerSettings>
        <chart:ChartMarkerSettings Type="Diamond"
                                   Fill="Red"
                                   Stroke="DarkRed"
                                   StrokeWidth="2"
                                   Width="12"
                                   Height="12"/>
    </chart:PolarLineSeries.MarkerSettings>
</chart:PolarLineSeries>
```

### Available Marker Shapes

```csharp
// Available ShapeType values:
ShapeType.Circle           // ⚫ Default
ShapeType.Cross            // ✖
ShapeType.Diamond          // ◆
ShapeType.Hexagon          // ⬡
ShapeType.InvertedTriangle // ▼
ShapeType.Pentagon         // ⬠
ShapeType.Plus             // ✚
ShapeType.Rectangle        // ▪
ShapeType.Triangle         // ▲
```

### Marker Examples by Use Case

**Standard markers:**
```csharp
markerSettings.Type = ShapeType.Circle;  // Clean, professional
```

**Emphasize points:**
```csharp
markerSettings.Type = ShapeType.Diamond;
markerSettings.Width = 15;
markerSettings.Height = 15;
```

**Multiple series differentiation:**
```csharp
// Series 1
series1.MarkerSettings = new ChartMarkerSettings { Type = ShapeType.Circle };

// Series 2
series2.MarkerSettings = new ChartMarkerSettings { Type = ShapeType.Triangle };

// Series 3
series3.MarkerSettings = new ChartMarkerSettings { Type = ShapeType.Diamond };
```

## Combining Labels and Markers

Display both labels and markers for maximum clarity:

```csharp
PolarLineSeries series = new PolarLineSeries
{
    ItemsSource = data,
    XBindingPath = "Category",
    YBindingPath = "Value",
    ShowMarkers = true,
    ShowDataLabels = true
};

// Customize markers
series.MarkerSettings = new ChartMarkerSettings
{
    Type = ShapeType.Circle,
    Fill = Colors.Blue,
    Width = 10,
    Height = 10
};

// Customize labels
series.DataLabelSettings = new PolarDataLabelSettings
{
    LabelStyle = new ChartDataLabelStyle
    {
        TextColor = Colors.DarkBlue,
        FontSize = 11,
        Margin = new Thickness(0, 0, 0, 10)  // Space above marker
    }
};
```

**XAML:**
```xml
<chart:PolarLineSeries ItemsSource="{Binding Data}"
                       XBindingPath="Category"
                       YBindingPath="Value"
                       ShowMarkers="True"
                       ShowDataLabels="True">
    <chart:PolarLineSeries.MarkerSettings>
        <chart:ChartMarkerSettings Type="Circle"
                                   Fill="Blue"
                                   Width="10"
                                   Height="10"/>
    </chart:PolarLineSeries.MarkerSettings>
    
    <chart:PolarLineSeries.DataLabelSettings>
        <chart:PolarDataLabelSettings>
            <chart:PolarDataLabelSettings.LabelStyle>
                <chart:ChartDataLabelStyle TextColor="DarkBlue"
                                           FontSize="11"
                                           Margin="0,0,0,10"/>
            </chart:PolarDataLabelSettings.LabelStyle>
        </chart:PolarDataLabelSettings>
    </chart:PolarLineSeries.DataLabelSettings>
</chart:PolarLineSeries>
```

## Best Practices

### When to Use Data Labels

**Use data labels when:**
- Exact values are important
- Chart has few data points (≤8-10)
- Users need precise numbers
- Presenting to stakeholders who want specifics

**Avoid data labels when:**
- Chart has many data points (causes clutter)
- Trends are more important than exact values
- Space is limited
- Tooltips provide sufficient detail

### When to Use Markers

**Use markers when:**
- Highlighting specific data points
- Multiple series need differentiation
- Data points are sparse
- Emphasizing individual measurements

**Always use markers for:**
- PolarLineSeries (helps locate points on line)
- Multiple overlapping series (improves clarity)

### Styling Guidelines

**Marker size:**
```csharp
// Small charts or many points
Width = 8, Height = 8

// Standard charts
Width = 10, Height = 10

// Large charts or emphasis
Width = 14, Height = 14
```

**Label font size:**
```csharp
// Dense charts
FontSize = 9

// Standard charts
FontSize = 11

// Large displays or presentations
FontSize = 14
```

**Color contrast:**
```csharp
// Ensure labels are readable
LabelStyle = new ChartDataLabelStyle
{
    TextColor = Colors.White,
    Background = new SolidColorBrush(Colors.Black)
};
```

## Common Patterns

### Pattern 1: Clean Spider Chart with Markers

```csharp
PolarLineSeries series = new PolarLineSeries
{
    ItemsSource = skillData,
    XBindingPath = "Skill",
    YBindingPath = "Score",
    ShowMarkers = true
};

series.MarkerSettings = new ChartMarkerSettings
{
    Type = ShapeType.Circle,
    Fill = Colors.Blue,
    Stroke = Colors.DarkBlue,
    StrokeWidth = 2,
    Width = 10,
    Height = 10
};
```

### Pattern 2: Percentage Labels with Custom Style

```csharp
PolarAreaSeries series = new PolarAreaSeries
{
    ItemsSource = surveyData,
    XBindingPath = "Question",
    YBindingPath = "Response",
    ShowDataLabels = true,
    LabelContext = LabelContext.Percentage
};

series.DataLabelSettings = new PolarDataLabelSettings
{
    LabelStyle = new ChartDataLabelStyle
    {
        TextColor = Colors.White,
        FontSize = 12,
        FontAttributes = FontAttributes.Bold,
        Background = new SolidColorBrush(Color.FromRgba(0, 0, 0, 0.7)),
        CornerRadius = new CornerRadius(3),
        Padding = new Thickness(5, 2)
    },
    UseSeriesPalette = false
};
```

### Pattern 3: Differentiated Multi-Series Markers

```csharp
// Series 1 - Circles
series1.ShowMarkers = true;
series1.MarkerSettings = new ChartMarkerSettings 
{ 
    Type = ShapeType.Circle,
    Fill = Colors.Blue,
    Width = 10,
    Height = 10
};

// Series 2 - Triangles
series2.ShowMarkers = true;
series2.MarkerSettings = new ChartMarkerSettings 
{ 
    Type = ShapeType.Triangle,
    Fill = Colors.Red,
    Width = 10,
    Height = 10
};

// Series 3 - Diamonds
series3.ShowMarkers = true;
series3.MarkerSettings = new ChartMarkerSettings 
{ 
    Type = ShapeType.Diamond,
    Fill = Colors.Green,
    Width = 10,
    Height = 10
};
```

## Troubleshooting

### Labels Overlapping

**Problem:** Data labels overlap and are unreadable.

**Solutions:**
```csharp
// Solution 1: Reduce font size
LabelStyle = new ChartDataLabelStyle { FontSize = 9 };

// Solution 2: Use percentage instead of values
LabelContext = LabelContext.Percentage

// Solution 3: Disable labels, use tooltip instead
ShowDataLabels = false;
EnableTooltip = true;

// Solution 4: Custom template with smaller layout
```

### Markers Not Visible

**Problem:** Markers enabled but not showing.

**Solutions:**
```csharp
// Check ShowMarkers is true
ShowMarkers = true

// Increase marker size
MarkerSettings = new ChartMarkerSettings { Width = 12, Height = 12 }

// Ensure marker colors contrast with series
MarkerSettings = new ChartMarkerSettings 
{ 
    Fill = Colors.White,
    Stroke = Colors.Black,
    StrokeWidth = 2
}
```

### Labels Cut Off

**Problem:** Labels appear cut off at chart edges.

**Solution:**
```xml
<!-- Add padding to chart -->
<chart:SfPolarChart Margin="20">
    <!-- Series here -->
</chart:SfPolarChart>
```

## Related Topics

- **Series Types**: [series-types.md](series-types.md) - Learn about series that use markers
- **Tooltip**: [legend-tooltip.md](legend-tooltip.md) - Alternative to data labels
- **Appearance**: [appearance-customization.md](appearance-customization.md) - Overall styling
