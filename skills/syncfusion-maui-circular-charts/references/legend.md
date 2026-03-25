# Legend in .NET MAUI Circular Charts

The legend provides a visual key that helps identify data points in the chart. This guide covers how to add, position, and customize legends in circular charts.

## Enabling the Legend

Initialize the `ChartLegend` class and assign it to the chart's `Legend` property.

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.Legend>
        <chart:ChartLegend/>
    </chart:SfCircularChart.Legend>
</chart:SfCircularChart>
```

**C#:**
```csharp
SfCircularChart chart = new SfCircularChart();
chart.Legend = new ChartLegend();
```

## Legend Visibility

Control legend visibility using the `IsVisible` property (default: `true`).

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.Legend>
        <chart:ChartLegend IsVisible="True"/>
    </chart:SfCircularChart.Legend>
</chart:SfCircularChart>
```

**C#:**
```csharp
chart.Legend = new ChartLegend
{
    IsVisible = true
};
```

## Series Legend Visibility

Control individual series visibility in the legend using `IsVisibleOnLegend` (default: `true`).

**XAML:**
```xml
<chart:PieSeries ItemsSource="{Binding Data}"
                 XBindingPath="XValue"
                 YBindingPath="YValue"
                 IsVisibleOnLegend="True"/>
```

**C#:**
```csharp
PieSeries series = new PieSeries
{
    IsVisibleOnLegend = true
};
```

## Legend Positioning

Use the `Placement` property to position the legend relative to the chart area.

### Available Positions

- **Top** (default)
- **Bottom**
- **Left**
- **Right**

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.Legend>
        <chart:ChartLegend Placement="Bottom"/>
    </chart:SfCircularChart.Legend>
</chart:SfCircularChart>
```

**C#:**
```csharp
chart.Legend = new ChartLegend
{
    Placement = LegendPlacement.Bottom
};
```

### Positioning Examples

```csharp
// Place legend at the top
chart.Legend.Placement = LegendPlacement.Top;

// Place legend at the bottom
chart.Legend.Placement = LegendPlacement.Bottom;

// Place legend on the left
chart.Legend.Placement = LegendPlacement.Left;

// Place legend on the right
chart.Legend.Placement = LegendPlacement.Right;
```

## Floating Legend

Position the legend inside the chart area using `IsFloating`, `OffsetX`, and `OffsetY`.

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.Legend>
        <chart:ChartLegend Placement="Right" 
                           IsFloating="True" 
                           OffsetX="-480" 
                           OffsetY="10"/>
    </chart:SfCircularChart.Legend>
</chart:SfCircularChart>
```

**C#:**
```csharp
chart.Legend = new ChartLegend
{
    Placement = LegendPlacement.Top,
    IsFloating = true,
    OffsetX = -170,
    OffsetY = 30
};
```

### Floating Legend Properties

| Property | Type | Description |
|----------|------|-------------|
| **IsFloating** | bool | Enables floating mode |
| **OffsetX** | double | Horizontal offset from placement position |
| **OffsetY** | double | Vertical offset from placement position |

## Legend Icon Customization

### Icon Types

Change the legend icon using the `LegendIcon` property on the series.

**XAML:**
```xml
<chart:PieSeries ItemsSource="{Binding Data}"
                 XBindingPath="XValue"
                 YBindingPath="YValue"
                 LegendIcon="Diamond"/>
```

**C#:**
```csharp
PieSeries series = new PieSeries
{
    LegendIcon = ChartLegendIconType.Diamond
};
```

### Available Icon Types

- `Circle` (default)
- `Diamond`
- `Rectangle`
- `Triangle`
- `InvertedTriangle`
- `Pentagon`
- `Plus`
- `Cross`
- `HorizontalLine`
- `VerticalLine`
- `SeriesType` (matches the series type)

## Label Customization

Use the `LabelStyle` property to customize legend label appearance.

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.Legend>
        <chart:ChartLegend>
            <chart:ChartLegend.LabelStyle>
                <chart:ChartLegendLabelStyle TextColor="Blue"
                                             FontSize="18"
                                             FontAttributes="Bold"
                                             FontFamily="Arial"
                                             Margin="5"/>
            </chart:ChartLegend.LabelStyle>
        </chart:ChartLegend>
    </chart:SfCircularChart.Legend>
</chart:SfCircularChart>
```

**C#:**
```csharp
chart.Legend = new ChartLegend();
ChartLegendLabelStyle labelStyle = new ChartLegendLabelStyle
{
    TextColor = Colors.Blue,
    FontSize = 18,
    FontAttributes = FontAttributes.Bold,
    Margin = 5,
    FontFamily = "Arial"
};
chart.Legend.LabelStyle = labelStyle;
```

### Label Style Properties

| Property | Type | Description |
|----------|------|-------------|
| **TextColor** | Color | Text color |
| **FontSize** | double | Font size |
| **FontAttributes** | FontAttributes | Bold, Italic, None |
| **FontFamily** | string | Font family name |
| **Margin** | Thickness/double | Spacing around label |

## Toggle Series Visibility

Enable users to show/hide series by tapping legend items using `ToggleSeriesVisibility`.

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.Legend>
        <chart:ChartLegend ToggleSeriesVisibility="True"/>
    </chart:SfCircularChart.Legend>
</chart:SfCircularChart>
```

**C#:**
```csharp
chart.Legend = new ChartLegend
{
    ToggleSeriesVisibility = true
};
```

When enabled:
- Tap a legend item to hide the corresponding data points
- Tap again to show them
- Hidden items appear dimmed in the legend

## Custom Item Layout

Customize the arrangement of legend items using the `ItemsLayout` property.

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.Legend>
        <chart:ChartLegend>
            <chart:ChartLegend.ItemsLayout>
                <FlexLayout Wrap="Wrap" WidthRequest="400"/>
            </chart:ChartLegend.ItemsLayout>
        </chart:ChartLegend>
    </chart:SfCircularChart.Legend>
</chart:SfCircularChart>
```

**C#:**
```csharp
ChartLegend legend = new ChartLegend();
legend.ItemsLayout = new FlexLayout
{
    Wrap = FlexWrap.Wrap,
    WidthRequest = 400
};
chart.Legend = legend;
```

The `ItemsLayout` accepts any layout type (FlexLayout, StackLayout, Grid, etc.).

## Custom Item Template

Create fully custom legend items using the `ItemTemplate` property.

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.Resources>
        <DataTemplate x:Key="legendTemplate">
            <StackLayout Orientation="Horizontal">
                <Rectangle HeightRequest="12" 
                           WidthRequest="12" 
                           Margin="3"
                           Fill="{Binding IconBrush}"/>
                <Label Text="{Binding Text}" Margin="3"/>
            </StackLayout>
        </DataTemplate>
    </chart:SfCircularChart.Resources>
    
    <chart:SfCircularChart.Legend>
        <chart:ChartLegend ItemTemplate="{StaticResource legendTemplate}"/>
    </chart:SfCircularChart.Legend>
</chart:SfCircularChart>
```

**C#:**
```csharp
ChartLegend legend = new ChartLegend();
legend.ItemTemplate = chart.Resources["legendTemplate"] as DataTemplate;
chart.Legend = legend;
```

### Template Binding Context

The template's binding context (`ChartLegendItem`) provides:

| Property | Type | Description |
|----------|------|-------------|
| **Text** | string | Legend item label |
| **IconBrush** | Brush | Icon color |
| **Index** | int | Item index |
| **Item** | object | Corresponding series |
| **IsToggled** | bool | Toggle state |
| **TextColor** | Color | Text color |
| **IconType** | ChartLegendIconType | Icon shape |
| **IconWidth** | double | Icon width |
| **IconHeight** | double | Icon height |
| **FontFamily** | string | Font family |
| **FontSize** | double | Font size |
| **FontAttributes** | FontAttributes | Font style |
| **TextMargin** | Thickness | Text spacing |
| **DisableBrush** | Brush | Color when toggled off |

## Legend Maximum Size

Override the `GetMaximumSizeCoefficient` method to control legend size (value between 0 and 1).

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.Legend>
        <local:LegendExt/>
    </chart:SfCircularChart.Legend>
</chart:SfCircularChart>
```

**C#:**
```csharp
public class LegendExt : ChartLegend
{
    protected override double GetMaximumSizeCoefficient()
    {
        return 0.7;  // 70% of available space
    }
}

SfCircularChart chart = new SfCircularChart();
chart.Legend = new LegendExt();
```

## LegendItemCreated Event

Customize individual legend items dynamically using the `LegendItemCreated` event.

**C#:**
```csharp
ChartLegend legend = new ChartLegend();
legend.LegendItemCreated += OnLegendItemCreated;
chart.Legend = legend;

private void OnLegendItemCreated(object sender, LegendItemEventArgs e)
{
    // Customize the legend item
    e.LegendItem.Text = $"Category: {e.LegendItem.Text}";
    e.LegendItem.FontSize = 14;
    e.LegendItem.TextColor = Colors.DarkBlue;
    e.LegendItem.IconWidth = 20;
    e.LegendItem.IconHeight = 20;
}
```

### Event Arguments Properties

Access and modify these properties in the event handler:

- `Text` - Legend label text
- `TextColor` - Label text color
- `FontFamily` - Font family
- `FontAttributes` - Font style (Bold, Italic)
- `FontSize` - Font size
- `TextMargin` - Label margin
- `IconBrush` - Icon color
- `IconType` - Icon shape
- `IconHeight` - Icon height
- `IconWidth` - Icon width
- `IsToggled` - Toggle state
- `DisableBrush` - Color when disabled
- `Index` - Item index
- `Item` - Associated series

## Complete Examples

### Example 1: Bottom Legend with Custom Style

```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.Legend>
        <chart:ChartLegend Placement="Bottom">
            <chart:ChartLegend.LabelStyle>
                <chart:ChartLegendLabelStyle TextColor="DarkGray"
                                             FontSize="14"
                                             FontAttributes="Italic"/>
            </chart:ChartLegend.LabelStyle>
        </chart:ChartLegend>
    </chart:SfCircularChart.Legend>
    
    <chart:PieSeries ItemsSource="{Binding Data}"
                     XBindingPath="Category"
                     YBindingPath="Value"
                     LegendIcon="Diamond"/>
</chart:SfCircularChart>
```

### Example 2: Floating Legend with Toggle

```csharp
SfCircularChart chart = new SfCircularChart();

chart.Legend = new ChartLegend
{
    Placement = LegendPlacement.Right,
    IsFloating = true,
    OffsetX = -200,
    OffsetY = 50,
    ToggleSeriesVisibility = true,
    LabelStyle = new ChartLegendLabelStyle
    {
        FontSize = 12,
        TextColor = Colors.Black
    }
};

PieSeries series = new PieSeries
{
    ItemsSource = data,
    XBindingPath = "Product",
    YBindingPath = "Sales",
    LegendIcon = ChartLegendIconType.Rectangle
};

chart.Series.Add(series);
```

### Example 3: Custom Legend Template with Icons

```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.Resources>
        <DataTemplate x:Key="customTemplate">
            <Border Padding="5" BackgroundColor="LightGray" 
                    StrokeShape="RoundRectangle 5">
                <HorizontalStackLayout Spacing="8">
                    <BoxView Color="{Binding IconBrush}"
                             WidthRequest="16"
                             HeightRequest="16"
                             CornerRadius="8"/>
                    <Label Text="{Binding Text}"
                           FontAttributes="Bold"
                           VerticalOptions="Center"/>
                </HorizontalStackLayout>
            </Border>
        </DataTemplate>
    </chart:SfCircularChart.Resources>
    
    <chart:SfCircularChart.Legend>
        <chart:ChartLegend ItemTemplate="{StaticResource customTemplate}"
                           Placement="Bottom"/>
    </chart:SfCircularChart.Legend>
</chart:SfCircularChart>
```

## Best Practices

1. **Placement**: Position legends at the bottom or right for horizontal layouts
2. **Toggle**: Enable `ToggleSeriesVisibility` for interactive charts
3. **Icons**: Choose icon types that match your chart's visual style
4. **Floating**: Use floating legends to maximize chart area
5. **Layout**: Customize `ItemsLayout` for better arrangement with many items
6. **Size**: Control legend size to prevent it from dominating the chart

## Limitations

- Do not add items to the legend explicitly
- Do not bind `ItemsSource` when using `BindableLayouts`
- Arrange items vertically for left/right placements, horizontally for top/bottom
- Scrolling is enabled if legend exceeds `MaximumHeightRequest`
