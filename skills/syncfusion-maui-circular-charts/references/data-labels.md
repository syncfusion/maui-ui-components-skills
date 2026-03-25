# Data Labels in .NET MAUI Circular Charts

Data labels display values or information about chart segments, helping users understand the data at a glance. This guide covers enabling, positioning, and customizing data labels in circular charts.

## Table of Contents
- [Enabling Data Labels](#enabling-data-labels)
- [Label Position](#label-position)
- [Smart Label Alignment](#smart-label-alignment)
- [Label Content and Formatting](#label-content-and-formatting)
- [Connector Lines](#connector-lines)
- [Custom Label Templates](#custom-label-templates)
- [Styling Data Labels](#styling-data-labels)

## Enabling Data Labels

Enable data labels by setting the `ShowDataLabels` property to `true`.

**XAML:**
```xml
<chart:PieSeries ItemsSource="{Binding Data}"  
                 XBindingPath="Product" 
                 YBindingPath="SalesRate"
                 ShowDataLabels="True"/>
```

**C#:**
```csharp
PieSeries series = new PieSeries
{
    ShowDataLabels = true
};
```

> **Note:** Data labels are only supported for **PieSeries** and **DoughnutSeries**, not for RadialBarSeries.

## Label Position

The `LabelPosition` property controls whether labels appear inside or outside the chart segments.

### Inside Position (Default)

Labels are placed within the chart segments:

**XAML:**
```xml
<chart:PieSeries ShowDataLabels="True">
    <chart:PieSeries.DataLabelSettings>
        <chart:CircularDataLabelSettings LabelPosition="Inside"/>
    </chart:PieSeries.DataLabelSettings>
</chart:PieSeries>
```

**C#:**
```csharp
PieSeries series = new PieSeries
{
    ShowDataLabels = true,
    DataLabelSettings = new CircularDataLabelSettings
    {
        LabelPosition = ChartDataLabelPosition.Inside
    }
};
```

### Outside Position

Labels are placed outside the chart segments with connector lines:

**XAML:**
```xml
<chart:PieSeries ShowDataLabels="True">
    <chart:PieSeries.DataLabelSettings>
        <chart:CircularDataLabelSettings LabelPosition="Outside"/>
    </chart:PieSeries.DataLabelSettings>
</chart:PieSeries>
```

**C#:**
```csharp
series.DataLabelSettings = new CircularDataLabelSettings
{
    LabelPosition = ChartDataLabelPosition.Outside
};
```

### When to Use Each Position

- **Inside**: Best for larger segments where labels fit comfortably
- **Outside**: Better for small segments or when labels would overlap

## Smart Label Alignment

The `SmartLabelAlignment` property prevents label overlap by automatically adjusting positions.

### Available Options

1. **Shift** (default): Repositions overlapping labels
2. **Hide**: Hides overlapping labels
3. **None**: Allows labels to overlap

### Shift Alignment

**XAML:**
```xml
<chart:PieSeries ShowDataLabels="True">
    <chart:PieSeries.DataLabelSettings>
        <chart:CircularDataLabelSettings LabelPosition="Outside" 
                                         SmartLabelAlignment="Shift"/>
    </chart:PieSeries.DataLabelSettings>
</chart:PieSeries>
```

**C#:**
```csharp
series.DataLabelSettings = new CircularDataLabelSettings
{
    LabelPosition = ChartDataLabelPosition.Outside,
    SmartLabelAlignment = SmartLabelAlignment.Shift
};
```

### Hide Overlapping Labels

```csharp
series.DataLabelSettings = new CircularDataLabelSettings
{
    SmartLabelAlignment = SmartLabelAlignment.Hide
};
```

### Allow Overlap

```csharp
series.DataLabelSettings = new CircularDataLabelSettings
{
    SmartLabelAlignment = SmartLabelAlignment.None
};
```

### Smart Label Behavior by Position

| Label Position | Smart Alignment | Behavior |
|----------------|-----------------|----------|
| Inside | Shift | Labels move outside if they overlap |
| Inside | Hide | Overlapping labels are hidden |
| Outside | Shift | Labels rearrange to avoid overlap |
| Outside | Hide | Overlapping labels are hidden |
| Any | None | Labels stay in place (may overlap) |

> **Note:** When SmartLabelAlignment is Shift, labels that extend beyond the chart area are trimmed.

## Label Content and Formatting

### LabelContext Property

Control what data is displayed in labels using the `LabelContext` property.

#### Show Percentage

**XAML:**
```xml
<chart:PieSeries ItemsSource="{Binding Data}"
                 XBindingPath="Product"
                 YBindingPath="SalesRate"
                 ShowDataLabels="True"
                 LabelContext="Percentage"/>
```

**C#:**
```csharp
series.LabelContext = LabelContext.Percentage;
```

#### Show Y Value

**XAML:**
```xml
<chart:PieSeries LabelContext="YValue"/>
```

**C#:**
```csharp
series.LabelContext = LabelContext.YValue;
```

### Label Content Options

- **Percentage**: Shows each segment as a percentage of the total (e.g., "25%")
- **YValue**: Shows the raw data value (e.g., "150")

## Connector Lines

Connector lines join data labels to their corresponding segments when labels are positioned outside.

### Connector Line Customization

Use the `ConnectorLineSettings` property to customize connector line appearance:

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.Resources>
        <DoubleCollection x:Key="dashArray">
            <x:Double>5</x:Double>
            <x:Double>2</x:Double>
        </DoubleCollection>
    </chart:SfCircularChart.Resources>
    
    <chart:PieSeries ShowDataLabels="True">
        <chart:PieSeries.DataLabelSettings>
            <chart:CircularDataLabelSettings LabelPosition="Outside">
                <chart:CircularDataLabelSettings.ConnectorLineSettings>
                    <chart:ConnectorLineStyle Stroke="Black"
                                              StrokeWidth="2"
                                              StrokeDashArray="{StaticResource dashArray}"
                                              ConnectorType="Curve"/>
                </chart:CircularDataLabelSettings.ConnectorLineSettings>
            </chart:CircularDataLabelSettings>
        </chart:PieSeries.DataLabelSettings>
    </chart:PieSeries>
</chart:SfCircularChart>
```

**C#:**
```csharp
DoubleCollection dashArray = new DoubleCollection { 5, 2 };

var connectorLineStyle = new ConnectorLineStyle
{
    Stroke = Colors.Black,
    StrokeWidth = 2,
    StrokeDashArray = dashArray,
    ConnectorType = ConnectorType.Curve
};

series.DataLabelSettings = new CircularDataLabelSettings
{
    LabelPosition = ChartDataLabelPosition.Outside,
    ConnectorLineSettings = connectorLineStyle
};
```

### Connector Line Properties

| Property | Description | Example Values |
|----------|-------------|----------------|
| **Stroke** | Line color | `Colors.Black`, `Color.FromArgb("#FF0000")` |
| **StrokeWidth** | Line thickness | `1`, `2`, `3` |
| **StrokeDashArray** | Dash pattern | `{5, 2}` for dashed line |
| **ConnectorType** | Line style | `Line` (straight), `Curve` (curved) |

### Connector Type Options

```csharp
// Straight connector lines
connectorLineStyle.ConnectorType = ConnectorType.Line;

// Curved connector lines
connectorLineStyle.ConnectorType = ConnectorType.Curve;
```

## Custom Label Templates

Create fully custom data labels using the `LabelTemplate` property.

### Basic Custom Template

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.Resources>
        <DataTemplate x:Key="labelTemplate">
            <HorizontalStackLayout Spacing="5">
                <Label Text="{Binding Item.Product}" 
                       TextColor="White" 
                       FontSize="13"/>
                <Label Text=" : " 
                       TextColor="White" 
                       FontSize="13"/>
                <Label Text="{Binding Item.SalesRate}" 
                       TextColor="White" 
                       FontSize="13"/>
            </HorizontalStackLayout>
        </DataTemplate>
    </chart:SfCircularChart.Resources>

    <chart:PieSeries ShowDataLabels="True"
                     LabelTemplate="{StaticResource labelTemplate}"/>
</chart:SfCircularChart>
```

**C#:**
```csharp
DataTemplate labelTemplate = new DataTemplate(() =>
{
    var layout = new HorizontalStackLayout { Spacing = 5 };

    var productLabel = new Label
    {
        TextColor = Colors.White,
        FontSize = 13
    };
    productLabel.SetBinding(Label.TextProperty, "Item.Product");

    var separatorLabel = new Label
    {
        Text = " : ",
        TextColor = Colors.White,
        FontSize = 13
    };

    var salesLabel = new Label
    {
        TextColor = Colors.White,
        FontSize = 13
    };
    salesLabel.SetBinding(Label.TextProperty, "Item.SalesRate");

    layout.Children.Add(productLabel);
    layout.Children.Add(separatorLabel);
    layout.Children.Add(salesLabel);

    return layout;
});

series.LabelTemplate = labelTemplate;
```

### Template Binding Context

The binding context provides access to:
- **Item**: The data model object (e.g., `Item.Product`, `Item.SalesRate`)
- **Segment properties**: Information about the chart segment

### Advanced Custom Template with Icons

```xml
<DataTemplate x:Key="advancedTemplate">
    <StackLayout Orientation="Horizontal" Padding="5">
        <Image Source="{Binding Item.Icon}" 
               WidthRequest="16" 
               HeightRequest="16"/>
        <Label Text="{Binding Item.Product}" 
               FontAttributes="Bold"/>
        <Label Text="{Binding Item.SalesRate, StringFormat='{0:F1}%'}" 
               Margin="5,0,0,0"/>
    </StackLayout>
</DataTemplate>
```

## Styling Data Labels

### Using Series Palette

Apply the series color to the label background:

**XAML:**
```xml
<chart:PieSeries ShowDataLabels="True">
    <chart:PieSeries.DataLabelSettings>
        <chart:CircularDataLabelSettings UseSeriesPalette="True"/>
    </chart:PieSeries.DataLabelSettings>
</chart:PieSeries>
```

**C#:**
```csharp
series.DataLabelSettings = new CircularDataLabelSettings
{
    UseSeriesPalette = true
};
```

### Custom Label Styling

Use the `LabelStyle` property to customize label appearance:

**XAML:**
```xml
<chart:PieSeries ShowDataLabels="True">
    <chart:PieSeries.DataLabelSettings>
        <chart:CircularDataLabelSettings>
            <chart:CircularDataLabelSettings.LabelStyle>
                <chart:ChartDataLabelStyle TextColor="White"
                                           FontSize="14"
                                           FontAttributes="Bold"
                                           Background="DarkBlue"
                                           Margin="5"
                                           FontFamily="Arial"/>
            </chart:CircularDataLabelSettings.LabelStyle>
        </chart:CircularDataLabelSettings>
    </chart:PieSeries.DataLabelSettings>
</chart:PieSeries>
```

**C#:**
```csharp
series.DataLabelSettings = new CircularDataLabelSettings
{
    LabelStyle = new ChartDataLabelStyle
    {
        TextColor = Colors.White,
        FontSize = 14,
        FontAttributes = FontAttributes.Bold,
        Background = Colors.DarkBlue,
        Margin = new Thickness(5),
        FontFamily = "Arial"
    }
};
```

### Available Style Properties

| Property | Type | Description |
|----------|------|-------------|
| **TextColor** | Color | Label text color |
| **FontSize** | double | Label font size |
| **FontAttributes** | FontAttributes | Bold, Italic, None |
| **FontFamily** | string | Font family name |
| **Background** | Brush | Label background color |
| **Margin** | Thickness | Space around label |
| **Stroke** | Brush | Label border color |
| **StrokeWidth** | double | Label border thickness |

## Complete Examples

### Example 1: Outside Labels with Percentage

```xml
<chart:SfCircularChart>
    <chart:PieSeries ItemsSource="{Binding Data}"
                     XBindingPath="Category"
                     YBindingPath="Value"
                     ShowDataLabels="True"
                     LabelContext="Percentage">
        <chart:PieSeries.DataLabelSettings>
            <chart:CircularDataLabelSettings LabelPosition="Outside"
                                             SmartLabelAlignment="Shift">
                <chart:CircularDataLabelSettings.LabelStyle>
                    <chart:ChartDataLabelStyle FontSize="12" 
                                               FontAttributes="Bold"/>
                </chart:CircularDataLabelSettings.LabelStyle>
            </chart:CircularDataLabelSettings>
        </chart:PieSeries.DataLabelSettings>
    </chart:PieSeries>
</chart:SfCircularChart>
```

### Example 2: Styled Labels with Custom Connectors

```csharp
PieSeries series = new PieSeries
{
    ItemsSource = data,
    XBindingPath = "Product",
    YBindingPath = "Sales",
    ShowDataLabels = true
};

series.DataLabelSettings = new CircularDataLabelSettings
{
    LabelPosition = ChartDataLabelPosition.Outside,
    UseSeriesPalette = false,
    LabelStyle = new ChartDataLabelStyle
    {
        TextColor = Colors.Black,
        FontSize = 13,
        Background = Colors.White,
        Stroke = Colors.Gray,
        StrokeWidth = 1,
        Margin = new Thickness(3)
    },
    ConnectorLineSettings = new ConnectorLineStyle
    {
        Stroke = Colors.Gray,
        StrokeWidth = 1,
        ConnectorType = ConnectorType.Curve
    },
    SmartLabelAlignment = SmartLabelAlignment.Shift
};
```

### Example 3: Inside Labels with Series Colors

```xml
<chart:DoughnutSeries ShowDataLabels="True" LabelContext="Percentage">
    <chart:DoughnutSeries.DataLabelSettings>
        <chart:CircularDataLabelSettings LabelPosition="Inside"
                                         UseSeriesPalette="True">
            <chart:CircularDataLabelSettings.LabelStyle>
                <chart:ChartDataLabelStyle TextColor="White" 
                                           FontSize="14" 
                                           FontAttributes="Bold"/>
            </chart:CircularDataLabelSettings.LabelStyle>
        </chart:CircularDataLabelSettings>
    </chart:DoughnutSeries.DataLabelSettings>
</chart:DoughnutSeries>
```

## Best Practices

1. **Position**: Use `Outside` for charts with many small segments
2. **Smart Labels**: Always enable `SmartLabelAlignment` to prevent overlap
3. **Content**: Show percentages for proportional data, values for absolute amounts
4. **Styling**: Match label colors to your app's theme
5. **Connectors**: Use curved connectors for a modern look, straight for technical charts
6. **Templates**: Create custom templates when you need complex label content
