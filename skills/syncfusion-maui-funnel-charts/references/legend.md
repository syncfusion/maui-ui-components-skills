# Legend in .NET MAUI Funnel Chart

The legend provides a visual guide to identify funnel chart segments, displaying a list of data points with corresponding icons and labels. This comprehensive guide covers legend initialization, customization, placement, interactivity, and advanced features.

## Table of Contents
- [Defining the Legend](#defining-the-legend)
- [Legend Visibility](#legend-visibility)
- [Customizing Labels](#customizing-labels)
- [Legend Icon](#legend-icon)
- [Placement](#placement)
- [Floating Legend](#floating-legend)
- [Toggle Series Visibility](#toggle-series-visibility)
- [Legend Maximum Size Request](#legend-maximum-size-request)
- [Items Layout](#items-layout)
- [Item Template](#item-template)
- [LegendItemCreated Event](#legenditemcreated-event)
- [Limitations](#limitations)

## Defining the Legend

Initialize a `ChartLegend` instance and assign it to the `Legend` property of `SfFunnelChart`:

### XAML
```xaml
<chart:SfFunnelChart ItemsSource="{Binding Data}" 
                     XBindingPath="XValue"
                     YBindingPath="YValue">
    <chart:SfFunnelChart.Legend>
        <chart:ChartLegend/>
    </chart:SfFunnelChart.Legend>
</chart:SfFunnelChart>
```

### C#
```csharp
SfFunnelChart chart = new SfFunnelChart()
{
    ItemsSource = new ViewModel().Data,
    XBindingPath = "XValue",
    YBindingPath = "YValue",
};

chart.Legend = new ChartLegend();
this.Content = chart;
```

## Legend Visibility

Control legend visibility using the `IsVisible` property. The default value is `true`.

### XAML
```xaml
<chart:SfFunnelChart>
    <chart:SfFunnelChart.Legend>
        <chart:ChartLegend IsVisible="True"/>
    </chart:SfFunnelChart.Legend>
</chart:SfFunnelChart>
```

### C#
```csharp
chart.Legend = new ChartLegend()
{ 
    IsVisible = true 
};
```

## Customizing Labels

Customize legend label appearance using the `LabelStyle` property with `ChartLegendLabelStyle`:

### Available Properties

| Property | Type | Description |
|----------|------|-------------|
| `TextColor` | Color | Color of the legend text |
| `FontFamily` | string | Font family for legend labels |
| `FontAttributes` | FontAttributes | Font style (Bold, Italic, None) |
| `FontSize` | double | Font size for legend labels |
| `Margin` | Thickness | Margin around legend labels |

### XAML Example
```xaml
<chart:SfFunnelChart ItemsSource="{Binding Data}" 
                     XBindingPath="XValue"
                     YBindingPath="YValue">
    <chart:SfFunnelChart.Legend>
        <chart:ChartLegend>
            <chart:ChartLegend.LabelStyle>
                <chart:ChartLegendLabelStyle TextColor="Blue" 
                                            Margin="5" 
                                            FontSize="18" 
                                            FontAttributes="Bold" 
                                            FontFamily="PlaywriteAR-Regular"/>
            </chart:ChartLegend.LabelStyle>
        </chart:ChartLegend>
    </chart:SfFunnelChart.Legend>
</chart:SfFunnelChart>
```

### C# Example
```csharp
SfFunnelChart chart = new SfFunnelChart()
{
    XBindingPath = "XValue",
    YBindingPath = "YValue",
    ItemsSource = new ViewModel().Data,
};

chart.Legend = new ChartLegend();
ChartLegendLabelStyle labelStyle = new ChartLegendLabelStyle()
{
    TextColor = Colors.Blue,
    FontSize = 18,
    FontAttributes = FontAttributes.Bold,
    Margin = 5,
    FontFamily = "PlaywriteAR-Regular"
};
chart.Legend.LabelStyle = labelStyle;

this.Content = chart;
```

## Legend Icon

Customize legend icons using the `LegendIcon` property on `SfFunnelChart`. The default is `Circle`.

### Available Icon Types
- `Circle` (default)
- `Diamond`
- `Rectangle`
- `Pentagon`
- `Triangle`
- `InvertedTriangle`
- `Cross`
- `Plus`
- `Hexagon`
- `SeriesType`

### XAML
```xaml
<chart:SfFunnelChart ItemsSource="{Binding Data}" 
                     XBindingPath="XValue"
                     YBindingPath="YValue"
                     LegendIcon="Diamond">
    <chart:SfFunnelChart.Legend>
        <chart:ChartLegend/>
    </chart:SfFunnelChart.Legend>
</chart:SfFunnelChart>
```

### C#
```csharp
SfFunnelChart chart = new SfFunnelChart()
{
    ItemsSource = new ViewModel().Data,
    XBindingPath = "XValue",
    YBindingPath = "YValue",
    LegendIcon = ChartLegendIconType.Diamond
};
chart.Legend = new ChartLegend();

this.Content = chart;
```

## Placement

Position the legend relative to the chart area using the `Placement` property. The default is `Top`.

### Available Placements
- `Top` (default)
- `Bottom`
- `Left`
- `Right`

### XAML
```xaml
<chart:SfFunnelChart ItemsSource="{Binding Data}" 
                     XBindingPath="XValue"  
                     YBindingPath="YValue">
    <chart:SfFunnelChart.Legend>
        <chart:ChartLegend Placement="Bottom"/>
    </chart:SfFunnelChart.Legend>
</chart:SfFunnelChart>
```

### C#
```csharp
SfFunnelChart chart = new SfFunnelChart()
{
    XBindingPath = "XValue",
    YBindingPath = "YValue",
    ItemsSource = new ViewModel().Data,
};
   
chart.Legend = new ChartLegend()
{ 
    Placement = LegendPlacement.Bottom 
};

this.Content = chart;
```

## Floating Legend

Position the legend inside the chart area using `IsFloating`, `OffsetX`, and `OffsetY` properties. When `IsFloating` is `true`, the legend floats inside the chart based on the defined placement and offset values.

### Properties
- **IsFloating** (bool): Enable floating legend (default: `false`)
- **OffsetX** (double): Horizontal distance from placement position
- **OffsetY** (double): Vertical distance from placement position

### XAML
```xaml
<chart:SfFunnelChart ItemsSource="{Binding Data}" 
                     XBindingPath="XValue"  
                     YBindingPath="YValue">
    <chart:SfFunnelChart.Legend>
        <chart:ChartLegend Placement="Right"
                           IsFloating="True" 
                           OffsetX="-250" 
                           OffsetY="-100"/>
    </chart:SfFunnelChart.Legend>
</chart:SfFunnelChart>
```

### C#
```csharp
SfFunnelChart chart = new SfFunnelChart()
{
    XBindingPath = "XValue",
    YBindingPath = "YValue",
    ItemsSource = new ViewModel().Data,
};
   
chart.Legend = new ChartLegend()
{ 
    Placement = LegendPlacement.Top,
    IsFloating = true,
    OffsetX = -170,
    OffsetY = 30
};

this.Content = chart;
```

## Toggle Series Visibility

Enable interactive segment visibility toggling by tapping legend items using the `ToggleSeriesVisibility` property. The default value is `false`.

### XAML
```xaml
<chart:SfFunnelChart ItemsSource="{Binding Data}" 
                     XBindingPath="XValue"         
                     YBindingPath="YValue">
    <chart:SfFunnelChart.Legend>
        <chart:ChartLegend ToggleSeriesVisibility="True"/>
    </chart:SfFunnelChart.Legend>
</chart:SfFunnelChart>
```

### C#
```csharp
SfFunnelChart chart = new SfFunnelChart()
{
    ItemsSource = viewModel.Data,
    XBindingPath = "XValue",
    YBindingPath = "YValue"
};

chart.Legend = new ChartLegend()
{
    ToggleSeriesVisibility = true
};

this.Content = chart;
```

## Legend Maximum Size Request

Override the `GetMaximumSizeCoefficient` method in a custom `ChartLegend` class to control the maximum size of the legend view. The value should be between 0 and 1, representing the proportion of the chart area.

### XAML
```xaml
<chart:SfFunnelChart>
    <chart:SfFunnelChart.Legend>
        <local:LegendExt/>
    </chart:SfFunnelChart.Legend>
</chart:SfFunnelChart>
```

### C# Custom Legend Class
```csharp
public class LegendExt : ChartLegend
{
    protected override double GetMaximumSizeCoefficient()
    {
        return 0.7; // Legend can use up to 70% of chart area
    }
}

// Usage
SfFunnelChart chart = new SfFunnelChart();
chart.Legend = new LegendExt();
this.Content = chart;
```

## Items Layout

Customize the arrangement of legend items using the `ItemsLayout` property. This accepts any layout type (e.g., `FlexLayout`, `Grid`, `StackLayout`).

### FlexLayout Example (Wrapping Items)

#### XAML
```xaml
<chart:SfFunnelChart ItemsSource="{Binding Data}" 
                     XBindingPath="XValue"  
                     YBindingPath="YValue">
    <chart:SfFunnelChart.Legend>
        <chart:ChartLegend>
            <chart:ChartLegend.ItemsLayout>
                <FlexLayout Wrap="Wrap" WidthRequest="400"/>
            </chart:ChartLegend.ItemsLayout>
        </chart:ChartLegend>
    </chart:SfFunnelChart.Legend>
</chart:SfFunnelChart>
```

#### C#
```csharp
SfFunnelChart chart = new SfFunnelChart()
{
    ItemsSource = new ViewModel().Data,
    XBindingPath = "XValue",
    YBindingPath = "YValue",
};

ChartLegend legend = new ChartLegend();
legend.ItemsLayout = new FlexLayout()
{
    Wrap = FlexWrap.Wrap,
    WidthRequest = 400
};

chart.Legend = legend;
this.Content = chart;
```

### Grid Layout Example

```csharp
ChartLegend legend = new ChartLegend();
Grid grid = new Grid
{
    ColumnDefinitions =
    {
        new ColumnDefinition { Width = GridLength.Auto },
        new ColumnDefinition { Width = GridLength.Auto }
    }
};
legend.ItemsLayout = grid;
chart.Legend = legend;
```

## Item Template

Customize the appearance of individual legend items using the `ItemTemplate` property with a `DataTemplate`.

> **Note:** The `BindingContext` of the template is the `ChartLegendItem` provided by the legend.

### XAML
```xaml
<chart:SfFunnelChart ItemsSource="{Binding Data}" 
                     x:Name="chart"
                     XBindingPath="XValue"  
                     YBindingPath="YValue">
    
    <chart:SfFunnelChart.Resources>
        <DataTemplate x:Key="legendTemplate">
            <StackLayout Orientation="Horizontal">
                <Rectangle HeightRequest="12" 
                           WidthRequest="12" 
                           Margin="3"
                           Background="{Binding IconBrush}"/>
                <Label Text="{Binding XValue}" 
                       Margin="3"
                       VerticalOptions="Center"/>
            </StackLayout>
        </DataTemplate>
    </chart:SfFunnelChart.Resources>  
    
    <chart:SfFunnelChart.Legend>
        <chart:ChartLegend ItemTemplate="{StaticResource legendTemplate}"/>
    </chart:SfFunnelChart.Legend>
    
</chart:SfFunnelChart>
```

### C#
```csharp
SfFunnelChart chart = new SfFunnelChart()
{
    ItemsSource = new ViewModel().Data,
    XBindingPath = "XValue",
    YBindingPath = "YValue",
};
     
ChartLegend legend = new ChartLegend();
legend.ItemTemplate = chart.Resources["legendTemplate"] as DataTemplate;
chart.Legend = legend;

this.Content = chart;
```

## LegendItemCreated Event

The `LegendItemCreated` event fires when each legend item is created, allowing runtime customization of legend items.

### Event Arguments Properties

| Property | Type | Description |
|----------|------|-------------|
| `Text` | string | Legend item text |
| `TextColor` | Color | Text color |
| `FontFamily` | string | Font family |
| `FontAttributes` | FontAttributes | Font style |
| `FontSize` | double | Font size |
| `TextMargin` | Thickness | Text margin |
| `IconBrush` | Brush | Icon color |
| `IconType` | ChartLegendIconType | Icon type |
| `IconHeight` | double | Icon height |
| `IconWidth` | double | Icon width |
| `IsToggled` | bool | Toggle state |
| `DisableBrush` | Brush | Color when toggled off |
| `Index` | int | Item index |
| `Item` | object | Associated data item |

### XAML
```xaml
<chart:SfFunnelChart>
    <chart:SfFunnelChart.Legend>
        <chart:ChartLegend LegendItemCreated="OnLegendItemCreated"/>
    </chart:SfFunnelChart.Legend>
</chart:SfFunnelChart>
```

### C# Event Handler
```csharp
private void OnLegendItemCreated(object sender, LegendItemEventArgs e)
{
    // Customize the first legend item
    if (e.LegendItem.Index == 0)
    {
        e.LegendItem.IconBrush = new SolidColorBrush(Colors.Red);
        e.LegendItem.FontSize = 16;
        e.LegendItem.FontAttributes = FontAttributes.Bold;
    }
    
    // Make specific items stand out
    if (e.LegendItem.Text == "Prospects")
    {
        e.LegendItem.TextColor = Colors.Green;
        e.LegendItem.IconType = ChartLegendIconType.Diamond;
    }
}
```

## Limitations

When using `ItemsLayout` and `ItemTemplate`:

1. **Do not add items explicitly** to the layout
2. **Do not bind ItemsSource explicitly** when using BindableLayouts
3. **Orientation recommendations:**
   - Vertical arrangement for Left/Right placements
   - Horizontal arrangement for Top/Bottom placements
4. **Scrolling behavior:**
   - Scrolling enabled if layout exceeds `MaximumHeightRequest`
5. **MaximumHeightRequest:**
   - If set to 1 and layout is larger than chart, series may not render properly

## Complete Examples

### Example 1: Bottom-Placed Legend with Custom Styling

```xaml
<chart:SfFunnelChart ItemsSource="{Binding Data}"
                     XBindingPath="Stage"
                     YBindingPath="Value"
                     LegendIcon="Pentagon">
    
    <chart:SfFunnelChart.Legend>
        <chart:ChartLegend Placement="Bottom">
            <chart:ChartLegend.LabelStyle>
                <chart:ChartLegendLabelStyle FontSize="14"
                                            FontAttributes="Bold"
                                            TextColor="DarkSlateGray"
                                            Margin="8"/>
            </chart:ChartLegend.LabelStyle>
        </chart:ChartLegend>
    </chart:SfFunnelChart.Legend>
    
</chart:SfFunnelChart>
```

### Example 2: Floating Legend with Toggle

```csharp
chart.Legend = new ChartLegend()
{
    Placement = LegendPlacement.Right,
    IsFloating = true,
    OffsetX = -200,
    OffsetY = -80,
    ToggleSeriesVisibility = true
};
```

### Example 3: Custom Legend Template

```xaml
<DataTemplate x:Key="customLegendTemplate">
    <Border Padding="8"
            Background="LightGray"
            Stroke="Gray"
            StrokeThickness="1">
        <StackLayout Orientation="Horizontal" Spacing="8">
            <BoxView Color="{Binding IconBrush}" 
                     WidthRequest="16" 
                     HeightRequest="16"
                     CornerRadius="8"/>
            <Label Text="{Binding Text}" 
                   FontSize="14"
                   FontAttributes="Bold"
                   VerticalOptions="Center"/>
        </StackLayout>
    </Border>
</DataTemplate>
```

## Best Practices

1. **Placement:**
   - Use Bottom/Top for horizontal charts
   - Use Left/Right for vertical charts
   - Avoid overlapping chart content

2. **Visibility:**
   - Enable `ToggleSeriesVisibility` for interactive exploration
   - Keep toggle behavior intuitive

3. **Styling:**
   - Ensure text is readable against backgrounds
   - Use consistent icon types across charts
   - Match font styles to your application theme

4. **Floating Legends:**
   - Test offsets on different screen sizes
   - Ensure floating legend doesn't obscure critical data

5. **Performance:**
   - Use simple templates for better rendering performance
   - Avoid complex layouts with many legend items

## Troubleshooting

**Legend not appearing:**
- Verify `Legend` property is set to a `ChartLegend` instance
- Ensure `IsVisible` is `true` (default)
- Check that data is bound correctly

**Legend items cut off:**
- Adjust `MaximumSizeCoefficient` to allow more space
- Use `ItemsLayout` with wrapping
- Consider different placement

**Toggle not working:**
- Set `ToggleSeriesVisibility="True"`
- Verify legend items are clickable (not obscured)

**Custom template not showing:**
- Ensure template resource key matches reference
- Verify `BindingContext` properties exist on `ChartLegendItem`
- Check for binding errors in output window
