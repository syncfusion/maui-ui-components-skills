# Legend in Pyramid Charts

## Table of Contents
- [Overview](#overview)
- [Defining the Legend](#defining-the-legend)
- [Legend Visibility](#legend-visibility)
- [Customizing Labels](#customizing-labels)
- [Legend Icon](#legend-icon)
- [Placement](#placement)
- [Floating Legend](#floating-legend)
- [Toggle Series Visibility](#toggle-series-visibility)
- [Maximum Size Request](#maximum-size-request)
- [Items Layout](#items-layout)
- [Item Template](#item-template)
- [LegendItemCreated Event](#legenditemcreated-event)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)

## Overview

The legend provides a list of pyramid segments, helping users identify corresponding data points in the chart. Each segment gets a legend item showing its icon, label, and optional customization.

**Key Features:**
- Automatic legend generation from data
- Customizable label styles and icons
- Flexible placement (Top, Bottom, Left, Right)
- Floating legend with precise positioning
- Toggle segment visibility by tapping legend items
- Scrollable legends for overflow handling
- Custom layouts and templates
- Event-driven customization

## Defining the Legend

Initialize a `ChartLegend` instance and assign it to the `Legend` property.

### XAML

```xaml
<chart:SfPyramidChart ItemsSource="{Binding Data}" 
                      XBindingPath="Name"
                      YBindingPath="Value">
    
    <chart:SfPyramidChart.Legend>
        <chart:ChartLegend/>
    </chart:SfPyramidChart.Legend>
    
</chart:SfPyramidChart>
```

### C#

```csharp
SfPyramidChart chart = new SfPyramidChart()
{
    ItemsSource = new ViewModel().Data,
    XBindingPath = "Name",
    YBindingPath = "Value",
};

chart.Legend = new ChartLegend();
this.Content = chart;
```

**Result:** Legend appears at the top of the chart (default) with one item per pyramid segment.

## Legend Visibility

Control legend visibility using the `IsVisible` property. Default is `true`.

### XAML

```xaml
<chart:SfPyramidChart ItemsSource="{Binding Data}" 
                      XBindingPath="Name"
                      YBindingPath="Value">
    
    <chart:SfPyramidChart.Legend>
        <chart:ChartLegend IsVisible="True"/>
    </chart:SfPyramidChart.Legend>
    
</chart:SfPyramidChart>
```

### C#

```csharp
chart.Legend = new ChartLegend()
{ 
   IsVisible = true 
};
```

### Dynamically Toggle Visibility

```csharp
// Hide legend
chart.Legend.IsVisible = false;

// Show legend
chart.Legend.IsVisible = true;
```

## Customizing Labels

Customize legend label appearance using the `LabelStyle` property with a `ChartLegendLabelStyle` instance.

### Available Label Style Properties

| Property | Type | Description |
|----------|------|-------------|
| **TextColor** | Color | Color of the label text |
| **FontFamily** | string | Font family for the label |
| **FontAttributes** | FontAttributes | Font style (Bold, Italic, None) |
| **FontSize** | double | Font size of the label |
| **Margin** | Thickness | Margin around the label |

### Basic Label Customization

**XAML:**
```xaml
<chart:SfPyramidChart ItemsSource="{Binding Data}" 
                      XBindingPath="Name"
                      YBindingPath="Value">
    
    <chart:SfPyramidChart.Legend>
        <chart:ChartLegend>
            <chart:ChartLegend.LabelStyle>
                <chart:ChartLegendLabelStyle TextColor="Blue" 
                                            FontSize="14" 
                                            FontAttributes="Bold" 
                                            Margin="5"/>
            </chart:ChartLegend.LabelStyle>
        </chart:ChartLegend>
    </chart:SfPyramidChart.Legend>
    
</chart:SfPyramidChart>
```

**C#:**
```csharp
chart.Legend = new ChartLegend();

ChartLegendLabelStyle labelStyle = new ChartLegendLabelStyle()
{
    TextColor = Colors.Blue,
    FontSize = 14,
    FontAttributes = FontAttributes.Bold,
    Margin = 5
};

chart.Legend.LabelStyle = labelStyle;
```

### Custom Font Family Example

```xaml
<chart:ChartLegend>
    <chart:ChartLegend.LabelStyle>
        <chart:ChartLegendLabelStyle TextColor="DarkSlateGray" 
                                    FontFamily="PlaywriteAR-Regular"
                                    FontSize="16" 
                                    FontAttributes="Italic" 
                                    Margin="8"/>
    </chart:ChartLegend.LabelStyle>
</chart:ChartLegend>
```

## Legend Icon

Customize legend icons using the `LegendIcon` property on the chart. Available icon types are defined in the `ChartLegendIconType` enum.

### Available Icon Types

- Circle (default)
- Diamond
- Rectangle
- Triangle
- InvertedTriangle
- Pentagon
- Cross
- Plus
- HorizontalLine
- VerticalLine
- SeriesType

### XAML Example

```xaml
<chart:SfPyramidChart ItemsSource="{Binding Data}"
                      XBindingPath="Name" 
                      YBindingPath="Value"
                      LegendIcon="Diamond">
    
    <chart:SfPyramidChart.Legend>
        <chart:ChartLegend/>
    </chart:SfPyramidChart.Legend>
    
</chart:SfPyramidChart>
```

### C# Example

```csharp
SfPyramidChart chart = new SfPyramidChart()
{
    ItemsSource = new ViewModel().Data,
    XBindingPath = "Name",
    YBindingPath = "Value",
    LegendIcon = ChartLegendIconType.Diamond
};

chart.Legend = new ChartLegend();
```

### Icon Type Examples

```csharp
// Rectangle icons
chart.LegendIcon = ChartLegendIconType.Rectangle;

// Triangle icons
chart.LegendIcon = ChartLegendIconType.Triangle;

// Pentagon icons
chart.LegendIcon = ChartLegendIconType.Pentagon;
```

## Placement

Position the legend using the `Placement` property. Default is `Top`.

### Available Placements

- **Top**: Above the chart
- **Bottom**: Below the chart
- **Left**: Left side of the chart
- **Right**: Right side of the chart

### XAML Example

```xaml
<chart:SfPyramidChart ItemsSource="{Binding Data}" 
                      XBindingPath="Name"
                      YBindingPath="Value">
    
    <chart:SfPyramidChart.Legend>
        <chart:ChartLegend Placement="Bottom"/>
    </chart:SfPyramidChart.Legend>
    
</chart:SfPyramidChart>
```

### C# Example

```csharp
chart.Legend = new ChartLegend()
{
    Placement = LegendPlacement.Bottom 
};
```

### All Placement Options

```csharp
// Top (default)
chart.Legend.Placement = LegendPlacement.Top;

// Bottom
chart.Legend.Placement = LegendPlacement.Bottom;

// Left
chart.Legend.Placement = LegendPlacement.Left;

// Right
chart.Legend.Placement = LegendPlacement.Right;
```

## Floating Legend

Create a floating legend that positions itself inside the chart area using `IsFloating`, `OffsetX`, and `OffsetY` properties.

### Properties

- **IsFloating**: Enable floating mode (default: false)
- **OffsetX**: Horizontal offset from placement position (in pixels)
- **OffsetY**: Vertical offset from placement position (in pixels)

### XAML Example

```xaml
<chart:SfPyramidChart ItemsSource="{Binding Data}" 
                      XBindingPath="Name"
                      YBindingPath="Value">
    
    <chart:SfPyramidChart.Legend>
        <chart:ChartLegend Placement="Right"
                           IsFloating="True" 
                           OffsetX="-300" 
                           OffsetY="80"/>
    </chart:SfPyramidChart.Legend>
    
</chart:SfPyramidChart>
```

### C# Example

```csharp
chart.Legend = new ChartLegend()
{
    Placement = LegendPlacement.Top,
    IsFloating = true,
    OffsetX = -170,
    OffsetY = 30  
};
```

### Positioning Logic

- Legend starts from the `Placement` position (Top/Bottom/Left/Right)
- Then moves by `OffsetX` (horizontal) and `OffsetY` (vertical)
- Negative offsets move left/up, positive offsets move right/down

**Example:**
```csharp
// Legend in top-left corner
chart.Legend = new ChartLegend()
{
    Placement = LegendPlacement.Top,
    IsFloating = true,
    OffsetX = 10,   // 10px from left
    OffsetY = 10    // 10px from top
};

// Legend in bottom-right corner
chart.Legend = new ChartLegend()
{
    Placement = LegendPlacement.Bottom,
    IsFloating = true,
    OffsetX = -200,  // 200px from right
    OffsetY = -50    // 50px from bottom
};
```

## Toggle Series Visibility

Enable segment visibility toggling by tapping legend items using the `ToggleSeriesVisibility` property. Default is `false`.

### XAML Example

```xaml
<chart:SfPyramidChart ItemsSource="{Binding Data}" 
                      XBindingPath="Name"         
                      YBindingPath="Value">
    
    <chart:SfPyramidChart.Legend>
        <chart:ChartLegend ToggleSeriesVisibility="True"/>
    </chart:SfPyramidChart.Legend>
    
</chart:SfPyramidChart>
```

### C# Example

```csharp
SfPyramidChart chart = new SfPyramidChart()
{
    ItemsSource = new ViewModel().Data,
    XBindingPath = "Name",
    YBindingPath = "Value"
};

chart.Legend = new ChartLegend()
{
    ToggleSeriesVisibility = true
};
```

**Behavior:**
- Tap legend item to hide corresponding segment
- Tapped segment becomes semi-transparent or hidden
- Tap again to show segment
- Useful for focusing on specific segments

## Maximum Size Request

Override the `GetMaximumSizeCoefficient` method to control the maximum space the legend can occupy. Value should be between 0 and 1.

### Implementation

**XAML:**
```xaml
<chart:SfPyramidChart>
    <chart:SfPyramidChart.Legend>
        <local:LegendExt/>
    </chart:SfPyramidChart.Legend>
</chart:SfPyramidChart>
```

**C# Custom Legend Class:**
```csharp
public class LegendExt : ChartLegend
{
    protected override double GetMaximumSizeCoefficient()
    {
        return 0.7;  // Legend can use up to 70% of available space
    }
}

// Usage
SfPyramidChart chart = new SfPyramidChart();
chart.Legend = new LegendExt();
this.Content = chart;
```

**Values:**
- `0.5` = Legend uses max 50% of space
- `0.7` = Legend uses max 70% of space (recommended)
- `1.0` = Legend uses max 100% of space (may leave no room for chart)

## Items Layout

Customize legend item arrangement using the `ItemsLayout` property. Default is `null` (vertical stack).

### FlexLayout Example

**XAML:**
```xaml
<chart:SfPyramidChart ItemsSource="{Binding Data}" 
                      XBindingPath="Name"  
                      YBindingPath="Value">
    
    <chart:SfPyramidChart.Legend>
        <chart:ChartLegend>
            <chart:ChartLegend.ItemsLayout>
                <FlexLayout Wrap="Wrap" WidthRequest="400"/>
            </chart:ChartLegend.ItemsLayout>
        </chart:ChartLegend>
    </chart:SfPyramidChart.Legend>
    
</chart:SfPyramidChart>
```

**C#:**
```csharp
ChartLegend legend = new ChartLegend();

legend.ItemsLayout = new FlexLayout()
{
    Wrap = FlexWrap.Wrap,
    WidthRequest = 400
};

chart.Legend = legend;
```

### Grid Layout Example

```csharp
legend.ItemsLayout = new Grid()
{
    RowDefinitions = new RowDefinitionCollection()
    {
        new RowDefinition() { Height = GridLength.Auto },
        new RowDefinition() { Height = GridLength.Auto }
    },
    ColumnDefinitions = new ColumnDefinitionCollection()
    {
        new ColumnDefinition() { Width = GridLength.Star },
        new ColumnDefinition() { Width = GridLength.Star }
    }
};
```

## Item Template

Customize legend item appearance using the `ItemTemplate` property with a `DataTemplate`.

### Template Binding Context

The BindingContext of the template is a `ChartLegendItem` object with these properties:

- **Text**: Legend label text
- **IconBrush**: Segment color
- **Index**: Item index
- **Item**: Reference to chart segment

### XAML Template Example

```xaml
<chart:SfPyramidChart ItemsSource="{Binding Data}" 
                      XBindingPath="Name"  
                      YBindingPath="Value" 
                      x:Name="chart">

    <chart:SfPyramidChart.Resources>
        <DataTemplate x:Key="legendTemplate">
            <StackLayout Orientation="Horizontal">
                <Rectangle HeightRequest="12" 
                           WidthRequest="12" 
                           Margin="3"
                           Background="{Binding IconBrush}"/>
                <Label Text="{Binding Text}" 
                       TextColor="Black"
                       FontSize="12"
                       Margin="3"/>
            </StackLayout>
        </DataTemplate>
    </chart:SfPyramidChart.Resources>  
    
    <chart:SfPyramidChart.Legend>
        <chart:ChartLegend ItemTemplate="{StaticResource legendTemplate}"/>
    </chart:SfPyramidChart.Legend>

</chart:SfPyramidChart>
```

### C# Template Example

```csharp
DataTemplate template = new DataTemplate(() =>
{
    var stackLayout = new StackLayout { Orientation = StackOrientation.Horizontal };
    
    var icon = new BoxView { HeightRequest = 15, WidthRequest = 15 };
    icon.SetBinding(BoxView.BackgroundColorProperty, "IconBrush");
    
    var label = new Label { FontSize = 14 };
    label.SetBinding(Label.TextProperty, "Text");
    
    stackLayout.Children.Add(icon);
    stackLayout.Children.Add(label);
    
    return stackLayout;
});

chart.Legend = new ChartLegend();
chart.Legend.ItemTemplate = template;
```

### Advanced Template with Image

```xaml
<DataTemplate x:Key="advancedLegendTemplate">
    <Grid Padding="5">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto"/>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="Auto"/>
        </Grid.ColumnDefinitions>
        
        <Ellipse Grid.Column="0" 
                 HeightRequest="20" 
                 WidthRequest="20"
                 Fill="{Binding IconBrush}"/>
        
        <Label Grid.Column="1" 
               Text="{Binding Text}" 
               VerticalOptions="Center"
               Margin="10,0,0,0"/>
        
        <Image Grid.Column="2" 
               Source="info_icon.png" 
               HeightRequest="16" 
               WidthRequest="16"/>
    </Grid>
</DataTemplate>
```

## LegendItemCreated Event

The `LegendItemCreated` event fires when each legend item is created, allowing dynamic customization.

### Event Handler

```csharp
chart.Legend = new ChartLegend();
chart.Legend.LegendItemCreated += OnLegendItemCreated;

private void OnLegendItemCreated(object sender, LegendItemEventArgs e)
{
    // Access legend item properties
    var legendItem = e.LegendItem;
    
    // Customize based on conditions
    if (legendItem.Index == 0)
    {
        legendItem.TextColor = Colors.Red;
        legendItem.FontSize = 16;
        legendItem.FontAttributes = FontAttributes.Bold;
    }
    
    // Modify icon
    legendItem.IconType = ChartLegendIconType.Diamond;
    legendItem.IconHeight = 15;
    legendItem.IconWidth = 15;
}
```

### Available LegendItem Properties

| Property | Type | Description |
|----------|------|-------------|
| **Text** | string | Label text |
| **TextColor** | Color | Label color |
| **FontFamily** | string | Font family |
| **FontAttributes** | FontAttributes | Font style |
| **FontSize** | double | Font size |
| **TextMargin** | Thickness | Label margin |
| **IconBrush** | Brush | Icon color |
| **IconType** | ChartLegendIconType | Icon shape |
| **IconHeight** | double | Icon height |
| **IconWidth** | double | Icon width |
| **IsToggled** | bool | Visibility state |
| **DisableBrush** | Brush | Color when toggled off |
| **Index** | int | Item index |
| **Item** | object | Reference to segment |

### Conditional Styling Example

```csharp
private void OnLegendItemCreated(object sender, LegendItemEventArgs e)
{
    var item = e.LegendItem;
    
    // Highlight first and last items
    if (item.Index == 0)
    {
        item.FontAttributes = FontAttributes.Bold;
        item.FontSize = 16;
    }
    else if (item.Index == chart.ItemsSource.Count - 1)
    {
        item.TextColor = Colors.Red;
    }
    
    // Custom icon for specific items
    if (item.Text.Contains("Critical"))
    {
        item.IconType = ChartLegendIconType.Triangle;
        item.IconBrush = Brush.Red;
    }
}
```

## Complete Examples

### Example 1: Bottom Legend with Custom Styling

```xaml
<chart:SfPyramidChart ItemsSource="{Binding Data}"
                      XBindingPath="Name"
                      YBindingPath="Value"
                      LegendIcon="Rectangle">
    
    <chart:SfPyramidChart.Legend>
        <chart:ChartLegend Placement="Bottom">
            <chart:ChartLegend.LabelStyle>
                <chart:ChartLegendLabelStyle TextColor="DarkBlue"
                                            FontSize="13"
                                            FontAttributes="Bold"
                                            Margin="6"/>
            </chart:ChartLegend.LabelStyle>
        </chart:ChartLegend>
    </chart:SfPyramidChart.Legend>
    
</chart:SfPyramidChart>
```

### Example 2: Floating Legend with Toggle

```csharp
SfPyramidChart chart = new SfPyramidChart();
chart.ItemsSource = viewModel.Data;
chart.XBindingPath = "Name";
chart.YBindingPath = "Value";

chart.Legend = new ChartLegend()
{
    Placement = LegendPlacement.Right,
    IsFloating = true,
    OffsetX = -50,
    OffsetY = 20,
    ToggleSeriesVisibility = true,
    LabelStyle = new ChartLegendLabelStyle()
    {
        TextColor = Colors.Black,
        FontSize = 12,
        Margin = 5
    }
};
```

### Example 3: Wrapped Legend with Custom Layout

```xaml
<chart:SfPyramidChart ItemsSource="{Binding Data}"
                      XBindingPath="Name"
                      YBindingPath="Value">
    
    <chart:SfPyramidChart.Legend>
        <chart:ChartLegend Placement="Bottom">
            <chart:ChartLegend.ItemsLayout>
                <FlexLayout Wrap="Wrap" 
                           JustifyContent="Center"
                           WidthRequest="600"/>
            </chart:ChartLegend.ItemsLayout>
        </chart:ChartLegend>
    </chart:SfPyramidChart.Legend>
    
</chart:SfPyramidChart>
```

## Best Practices

### Placement Guidelines

- **Top/Bottom**: Best for horizontal item arrangement
- **Left/Right**: Best for vertical item arrangement
- Use floating legends to save space when needed
- Ensure legend doesn't obscure important chart data

### Styling Recommendations

- Keep font sizes readable (12-14px minimum)
- Use consistent font families across the chart
- Ensure sufficient contrast between text and background
- Add margins for visual breathing room

### Performance Tips

- Avoid complex custom templates for large datasets
- Use simple layouts for legends with many items
- Test scrolling behavior with overflow legends

### Accessibility

- Ensure text color has sufficient contrast
- Use clear, descriptive label text
- Consider toggle visibility for screen reader support
- Test with different screen sizes and orientations

## Limitations

- Do not add legend items explicitly
- When using BindableLayouts, do not bind ItemsSource explicitly
- For better UX, arrange items vertically for left/right placements
- If layout's measured size exceeds MaximumHeightRequest, scrolling is enabled
- Setting MaximumHeightRequest to 1 may leave insufficient space for chart

## Related Features

- **Data Labels**: See [data-labels.md](data-labels.md) for on-segment labels
- **Tooltips**: See [tooltip.md](tooltip.md) for hover information
- **Appearance**: See [appearance-customization.md](appearance-customization.md) for colors