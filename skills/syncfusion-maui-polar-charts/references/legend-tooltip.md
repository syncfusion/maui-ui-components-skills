# Legend and Tooltip

## Table of Contents
- [Legend](#legend)
- [Tooltip](#tooltip)
- [Combining Legend and Tooltip](#combining-legend-and-tooltip)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Legend

The legend helps users identify series in charts with multiple data sets.

### Enable Legend

```csharp
SfPolarChart chart = new SfPolarChart();
chart.Legend = new ChartLegend();
```

**XAML:**
```xml
<chart:SfPolarChart>
    <chart:SfPolarChart.Legend>
        <chart:ChartLegend/>
    </chart:SfPolarChart.Legend>
    <!-- Series here -->
</chart:SfPolarChart>
```

###Legend Visibility

```csharp
chart.Legend = new ChartLegend
{
    IsVisible = true  // Show legend (default)
};
```

**XAML:**
```xml
<chart:ChartLegend IsVisible="True"/>
```

### Series Label

Set the label that appears in the legend:

```csharp
PolarLineSeries series = new PolarLineSeries
{
    ItemsSource = data,
    XBindingPath = "Category",
    YBindingPath = "Value",
    Label = "2023 Data"  // Appears in legend
};
```

### Legend Placement

Position the legend around the chart:

```csharp
chart.Legend = new ChartLegend
{
    Placement = LegendPlacement.Bottom  // Default is Top
};
```

**Available placements:**
- `LegendPlacement.Top` (default)
- `LegendPlacement.Bottom`
- `LegendPlacement.Left`
- `LegendPlacement.Right`

**XAML:**
```xml
<chart:ChartLegend Placement="Bottom"/>
```

### Customize Legend Labels

```csharp
chart.Legend = new ChartLegend();
chart.Legend.LabelStyle = new ChartLegendLabelStyle
{
    TextColor = Colors.Blue,
    FontSize = 14,
    FontAttributes = FontAttributes.Bold,
    FontFamily = "Arial",
    Margin = new Thickness(5)
};
```

**XAML:**
```xml
<chart:ChartLegend>
    <chart:ChartLegend.LabelStyle>
        <chart:ChartLegendLabelStyle TextColor="Blue"
                                     FontSize="14"
                                     FontAttributes="Bold"
                                     FontFamily="Arial"
                                     Margin="5"/>
    </chart:ChartLegend.LabelStyle>
</chart:ChartLegend>
```

### Legend Icons

Change the icon shape for series:

```csharp
PolarLineSeries series = new PolarLineSeries
{
    ItemsSource = data,
    XBindingPath = "Category",
    YBindingPath = "Value",
    Label = "Data",
    LegendIcon = ChartLegendIconType.Diamond
};
```

**Available icon types:**
- `Circle` (default)
- `Rectangle`
- `Diamond`
- `Pentagon`
- `Triangle`
- `InvertedTriangle`
- `Cross`
- `Plus`
- `Hexagon`

**XAML:**
```xml
<chart:PolarLineSeries Label="Data" LegendIcon="Diamond"/>
```

### Toggle Series Visibility

Allow users to show/hide series by tapping legend items:

```csharp
chart.Legend = new ChartLegend
{
    ToggleSeriesVisibility = true
};
```

**XAML:**
```xml
<chart:ChartLegend ToggleSeriesVisibility="True"/>
```

### Control Individual Series Legend Visibility

```csharp
// Show in legend
series1.IsVisibleOnLegend = true;

// Hide from legend
series2.IsVisibleOnLegend = false;
```

**XAML:**
```xml
<chart:PolarLineSeries IsVisibleOnLegend="True" Label="Visible"/>
<chart:PolarLineSeries IsVisibleOnLegend="False" Label="Hidden"/>
```

### Custom Legend Item Template

Create custom legend item layouts:

```xml
<chart:SfPolarChart>
    <chart:SfPolarChart.Resources>
        <DataTemplate x:Key="legendTemplate">
            <StackLayout Orientation="Horizontal">
                <Rectangle HeightRequest="12"
                           WidthRequest="12"
                           Margin="3"
                           Background="{Binding IconBrush}"/>
                <Label Text="{Binding Text}" Margin="3"/>
            </StackLayout>
        </DataTemplate>
    </chart:SfPolarChart.Resources>
    
    <chart:SfPolarChart.Legend>
        <chart:ChartLegend ItemTemplate="{StaticResource legendTemplate}"/>
    </chart:SfPolarChart.Legend>
</chart:SfPolarChart>
```

### Legend Item Layout

Customize legend item arrangement:

```xml
<chart:ChartLegend>
    <chart:ChartLegend.ItemsLayout>
        <FlexLayout HorizontalOptions="Start"
                    Margin="10"
                    Wrap="Wrap"/>
    </chart:ChartLegend.ItemsLayout>
</chart:ChartLegend>
```

### LegendItemCreated Event

Customize individual legend items:

```csharp
chart.Legend.LegendItemCreated += (sender, e) =>
{
    // Customize based on series
    if (e.LegendItem.Text == "Critical")
    {
        e.LegendItem.TextColor = Colors.Red;
        e.LegendItem.FontAttributes = FontAttributes.Bold;
    }
};
```

## Tooltip

Tooltips display information when users tap or hover over data points.

### Enable Tooltip

```csharp
// Enable on series
PolarLineSeries series = new PolarLineSeries
{
    ItemsSource = data,
    XBindingPath = "Category",
    YBindingPath = "Value",
    EnableTooltip = true
};

// Set tooltip behavior on chart
SfPolarChart chart = new SfPolarChart();
chart.TooltipBehavior = new ChartTooltipBehavior();
```

**XAML:**
```xml
<chart:SfPolarChart>
    <chart:SfPolarChart.TooltipBehavior>
        <chart:ChartTooltipBehavior/>
    </chart:SfPolarChart.TooltipBehavior>
    
    <chart:PolarLineSeries ItemsSource="{Binding Data}"
                           XBindingPath="Category"
                           YBindingPath="Value"
                           EnableTooltip="True"/>
</chart:SfPolarChart>
```

### Customize Tooltip Appearance

```csharp
chart.TooltipBehavior = new ChartTooltipBehavior
{
    Background = new SolidColorBrush(Colors.DarkBlue),
    TextColor = Colors.White,
    FontSize = 14,
    FontAttributes = FontAttributes.Bold,
    FontFamily = "Arial",
    Margin = new Thickness(5)
};
```

**XAML:**
```xml
<chart:ChartTooltipBehavior Background="DarkBlue"
                            TextColor="White"
                            FontSize="14"
                            FontAttributes="Bold"
                            FontFamily="Arial"
                            Margin="5"/>
```

### Tooltip Duration

Control how long tooltips stay visible:

```csharp
chart.TooltipBehavior = new ChartTooltipBehavior
{
    Duration = 5000  // 5 seconds (in milliseconds)
};
```

**XAML:**
```xml
<chart:ChartTooltipBehavior Duration="5000"/>
```

### Custom Tooltip Template

Create custom tooltip layouts:

```xml
<chart:SfPolarChart>
    <chart:SfPolarChart.Resources>
        <DataTemplate x:Key="tooltipTemplate">
            <StackLayout>
                <Label Text="{Binding Item.Direction}"
                       HorizontalTextAlignment="Center"
                       TextColor="White"
                       FontAttributes="Bold"
                       FontSize="12.5"
                       Margin="0,2,0,2"/>
                <BoxView Color="Gray" HeightRequest="1" WidthRequest="90"/>
                <StackLayout Orientation="Horizontal" Padding="3">
                    <Ellipse Stroke="White"
                             StrokeThickness="2"
                             HeightRequest="10"
                             WidthRequest="10"
                             Fill="#48988B"
                             Margin="0,1,3,0"/>
                    <Label Text="Tree"
                           TextColor="White"
                           FontSize="12"
                           Margin="3,0,3,0"/>
                    <Label Text="{Binding Item.Tree, StringFormat=' : {0}'}"
                           TextColor="White"
                           FontSize="12"
                           Margin="0,0,3,0"/>
                </StackLayout>
            </StackLayout>
        </DataTemplate>
    </chart:SfPolarChart.Resources>
    
    <chart:SfPolarChart.TooltipBehavior>
        <chart:ChartTooltipBehavior/>
    </chart:SfPolarChart.TooltipBehavior>
    
    <chart:PolarAreaSeries ItemsSource="{Binding PlantDetails}"
                           XBindingPath="Direction"
                           YBindingPath="Tree"
                           TooltipTemplate="{StaticResource tooltipTemplate}"
                           EnableTooltip="True"/>
</chart:SfPolarChart>
```

### Programmatically Show/Hide Tooltip

```csharp
// Show tooltip at specific location
chartTooltipBehavior.Show(pointX, pointY, animated: true);

// Hide tooltip
chartTooltipBehavior.Hide();
```

## Combining Legend and Tooltip

Use both for comprehensive chart interactivity:

```xml
<chart:SfPolarChart>
    <!-- Legend for series identification -->
    <chart:SfPolarChart.Legend>
        <chart:ChartLegend Placement="Bottom"/>
    </chart:SfPolarChart.Legend>
    
    <!-- Tooltip for detailed values -->
    <chart:SfPolarChart.TooltipBehavior>
        <chart:ChartTooltipBehavior Duration="3000"/>
    </chart:SfPolarChart.TooltipBehavior>
    
    <chart:PolarLineSeries ItemsSource="{Binding TreeData}"
                           XBindingPath="Direction"
                           YBindingPath="Value"
                           Label="Trees"
                           EnableTooltip="True"/>
    
    <chart:PolarLineSeries ItemsSource="{Binding FlowerData}"
                           XBindingPath="Direction"
                           YBindingPath="Value"
                           Label="Flowers"
                           EnableTooltip="True"/>
</chart:SfPolarChart>
```

## Best Practices

### Legend Guidelines

**When to use:**
- Multiple series (2+ series)
- Series names are important
- Users need to identify what data represents

**Placement recommendations:**
- **Bottom**: Wide charts, many series
- **Top**: Traditional layout, few series
- **Right/Left**: Tall charts, few series

**Styling tips:**
```csharp
// Keep labels concise
Label = "2023"  // ✓ Good
Label = "Data from the year 2023"  // ✗ Too long

// Use meaningful icons
LegendIcon = ChartLegendIconType.Diamond  // Different from default

// Make clickable for interactivity
ToggleSeriesVisibility = true
```

### Tooltip Guidelines

**When to use:**
- Exact values needed on demand
- Chart has many data points
- Space-saving alternative to data labels

**Best practices:**
```csharp
// Keep duration reasonable
Duration = 3000  // 3 seconds (not too short, not too long)

// Ensure good contrast
Background = Colors.DarkBlue,
TextColor = Colors.White

// Use custom templates for complex data
TooltipTemplate = customTemplate
```

## Common Patterns

### Pattern 1: Bottom Legend with Toggle

```csharp
SfPolarChart chart = new SfPolarChart();

chart.Legend = new ChartLegend
{
    Placement = LegendPlacement.Bottom,
    ToggleSeriesVisibility = true
};

chart.Legend.LabelStyle = new ChartLegendLabelStyle
{
    FontSize = 12,
    Margin = new Thickness(5)
};
```

### Pattern 2: Styled Tooltip

```csharp
chart.TooltipBehavior = new ChartTooltipBehavior
{
    Background = new SolidColorBrush(Color.FromRgba(0, 0, 0, 0.8)),
    TextColor = Colors.White,
    FontSize = 13,
    Duration = 4000,
    Margin = new Thickness(10, 5)
};
```

### Pattern 3: Multi-Series with Differentiated Icons

```csharp
series1.Label = "Q1";
series1.LegendIcon = ChartLegendIconType.Circle;
series1.EnableTooltip = true;

series2.Label = "Q2";
series2.LegendIcon = ChartLegendIconType.Triangle;
series2.EnableTooltip = true;

series3.Label = "Q3";
series3.LegendIcon = ChartLegendIconType.Diamond;
series3.EnableTooltip = true;
```

## Troubleshooting

### Legend Not Showing

**Problem:** Legend configured but not visible.

**Solutions:**
```csharp
// Ensure Legend is set
chart.Legend = new ChartLegend();

// Check visibility
chart.Legend.IsVisible = true;

// Ensure series have labels
series.Label = "Data";

// Check IsVisibleOnLegend
series.IsVisibleOnLegend = true;
```

### Tooltip Not Appearing

**Problem:** Tooltip enabled but not showing on tap/hover.

**Solutions:**
```csharp
// Enable on series
series.EnableTooltip = true;

// Set tooltip behavior on chart
chart.TooltipBehavior = new ChartTooltipBehavior();

// Check data point exists at tap location
// Ensure chart view is added to visual tree
```

### Legend Items Cut Off

**Problem:** Legend text or items are clipped.

**Solutions:**
```xml
<!-- Add padding to chart -->
<chart:SfPolarChart Padding="10">
    <chart:SfPolarChart.Legend>
        <chart:ChartLegend Placement="Bottom"/>
    </chart:SfPolarChart.Legend>
</chart:SfPolarChart>
```

## Related Topics

- **Series Types**: [series-types.md](series-types.md) - Series configuration
- **Data Labels**: [data-labels-markers.md](data-labels-markers.md) - Alternative to tooltips
- **Appearance**: [appearance-customization.md](appearance-customization.md) - Overall styling
