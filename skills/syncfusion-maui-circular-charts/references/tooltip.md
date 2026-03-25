# Tooltip in .NET MAUI Circular Charts

Tooltips display information about chart segments when users tap or hover over them. This guide covers enabling and customizing tooltips.

## Enabling Tooltips

Enable tooltips by setting the `EnableTooltip` property to `true` on the series.

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:PieSeries EnableTooltip="True" 
                     ItemsSource="{Binding Data}"
                     XBindingPath="Product"
                     YBindingPath="SalesRate"/>
</chart:SfCircularChart>
```

**C#:**
```csharp
PieSeries series = new PieSeries
{
    EnableTooltip = true
};
```

By default, tooltips display the segment's X and Y values.

## Tooltip Customization

Customize tooltip appearance using the `TooltipBehavior` property on the chart.

### Basic Customization

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.TooltipBehavior>
        <chart:ChartTooltipBehavior Background="DarkBlue"
                                    TextColor="White"
                                    FontSize="14"
                                    FontAttributes="Bold"
                                    FontFamily="Arial"
                                    Duration="3000"
                                    Margin="5"/>
    </chart:SfCircularChart.TooltipBehavior>
    
    <chart:PieSeries EnableTooltip="True"/>
</chart:SfCircularChart>
```

**C#:**
```csharp
SfCircularChart chart = new SfCircularChart();

chart.TooltipBehavior = new ChartTooltipBehavior
{
    Background = Colors.DarkBlue,
    TextColor = Colors.White,
    FontSize = 14,
    FontAttributes = FontAttributes.Bold,
    FontFamily = "Arial",
    Duration = 3000,  // milliseconds
    Margin = new Thickness(5)
};
```

### Tooltip Properties

| Property | Type | Description |
|----------|------|-------------|
| **Background** | Brush | Tooltip background color |
| **TextColor** | Color | Text color |
| **FontSize** | double | Font size |
| **FontAttributes** | FontAttributes | Bold, Italic, None |
| **FontFamily** | string | Font family name |
| **Duration** | int | Display duration in milliseconds |
| **Margin** | Thickness | Spacing around tooltip |

## Custom Tooltip Template

Create fully custom tooltips using the `TooltipTemplate` property on the series.

### Basic Custom Template

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.Resources>
        <DataTemplate x:Key="tooltipTemplate">
            <StackLayout Orientation="Horizontal" Padding="10" 
                         BackgroundColor="White">
                <Label Text="{Binding Item.Product}"
                       TextColor="Black"
                       FontAttributes="Bold"
                       FontSize="14"/>
                <Label Text=" : "
                       TextColor="Black"
                       FontSize="14"/>
                <Label Text="{Binding Item.SalesRate}"
                       TextColor="Black"
                       FontSize="14"/>
            </StackLayout>
        </DataTemplate>
    </chart:SfCircularChart.Resources>
    
    <chart:PieSeries EnableTooltip="True"
                     ItemsSource="{Binding Data}"
                     XBindingPath="Product"
                     YBindingPath="SalesRate"
                     TooltipTemplate="{StaticResource tooltipTemplate}"/>
</chart:SfCircularChart>
```

**C#:**
```csharp
PieSeries series = new PieSeries
{
    EnableTooltip = true,
    TooltipTemplate = chart.Resources["tooltipTemplate"] as DataTemplate
};
```

### Template Binding Context

The tooltip template's binding context provides:
- **Item**: The data model object (e.g., `Item.Product`, `Item.SalesRate`)
- **X**: X value
- **Y**: Y value

### Advanced Template with Styling

```xml
<DataTemplate x:Key="advancedTooltip">
    <Border BackgroundColor="#2C3E50" 
            Padding="15,10" 
            StrokeShape="RoundRectangle 8"
            Stroke="White"
            StrokeThickness="1">
        <VerticalStackLayout Spacing="5">
            <Label Text="{Binding Item.Product}"
                   TextColor="White"
                   FontAttributes="Bold"
                   FontSize="16"/>
            <BoxView HeightRequest="1" 
                     Color="White" 
                     Opacity="0.3"/>
            <HorizontalStackLayout Spacing="10">
                <Label Text="Sales:"
                       TextColor="LightGray"
                       FontSize="12"/>
                <Label Text="{Binding Item.SalesRate, StringFormat='{0:F2}'}"
                       TextColor="White"
                       FontAttributes="Bold"
                       FontSize="14"/>
            </HorizontalStackLayout>
        </VerticalStackLayout>
    </Border>
</DataTemplate>
```

### Template with Icons and Colors

```xml
<DataTemplate x:Key="iconTooltip">
    <Grid Padding="10" BackgroundColor="White">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto"/>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>
        
        <Ellipse Grid.Row="0" Grid.RowSpan="2"
                 Fill="{Binding IconBrush}"
                 WidthRequest="30"
                 HeightRequest="30"
                 Margin="0,0,10,0"/>
        
        <Label Grid.Row="0" Grid.Column="1"
               Text="{Binding Item.Product}"
               FontAttributes="Bold"
               FontSize="14"
               TextColor="Black"/>
        
        <Label Grid.Row="1" Grid.Column="1"
               Text="{Binding Item.SalesRate, StringFormat='Value: {0}'}"
               FontSize="12"
               TextColor="Gray"/>
    </Grid>
</DataTemplate>
```

## Tooltip Duration

Control how long tooltips remain visible using the `Duration` property (in milliseconds).

```csharp
chart.TooltipBehavior = new ChartTooltipBehavior
{
    Duration = 5000  // Show for 5 seconds
};
```

- **Default**: 2000 ms (2 seconds)
- **Short display**: 1000 ms
- **Long display**: 5000 ms or more

## Complete Examples

### Example 1: Styled Tooltip with Custom Colors

```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.TooltipBehavior>
        <chart:ChartTooltipBehavior Background="#34495E"
                                    TextColor="#ECF0F1"
                                    FontSize="13"
                                    FontAttributes="Bold"
                                    Duration="2500"
                                    Margin="8"/>
    </chart:SfCircularChart.TooltipBehavior>
    
    <chart:DoughnutSeries EnableTooltip="True"
                          ItemsSource="{Binding Data}"
                          XBindingPath="Category"
                          YBindingPath="Value"/>
</chart:SfCircularChart>
```

### Example 2: Custom Template with Percentage

```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.Resources>
        <DataTemplate x:Key="percentTooltip">
            <StackLayout BackgroundColor="#16A085" Padding="12">
                <Label Text="{Binding Item.Category}"
                       TextColor="White"
                       FontAttributes="Bold"
                       FontSize="15"/>
                <Label TextColor="White"
                       FontSize="13">
                    <Label.FormattedText>
                        <FormattedString>
                            <Span Text="{Binding Item.Value}"/>
                            <Span Text=" ("/>
                            <Span Text="{Binding Percentage, StringFormat='{0:F1}%'}"/>
                            <Span Text=")"/>
                        </FormattedString>
                    </Label.FormattedText>
                </Label>
            </StackLayout>
        </DataTemplate>
    </chart:SfCircularChart.Resources>
    
    <chart:PieSeries EnableTooltip="True"
                     TooltipTemplate="{StaticResource percentTooltip}"/>
</chart:SfCircularChart>
```

### Example 3: Minimal Tooltip Setup

```csharp
SfCircularChart chart = new SfCircularChart();

// Simple tooltip with default appearance
PieSeries series = new PieSeries
{
    ItemsSource = data,
    XBindingPath = "Product",
    YBindingPath = "Sales",
    EnableTooltip = true
};

chart.Series.Add(series);
```

### Example 4: Multi-Line Tooltip

```xml
<DataTemplate x:Key="multiLineTooltip">
    <StackLayout BackgroundColor="White" 
                 Padding="15" 
                 Spacing="8">
        <Label Text="Product Information"
               FontAttributes="Bold"
               FontSize="12"
               TextColor="Gray"/>
        
        <BoxView HeightRequest="1" Color="LightGray"/>
        
        <Label TextColor="Black" FontSize="14">
            <Label.FormattedText>
                <FormattedString>
                    <Span Text="Name: " FontAttributes="Bold"/>
                    <Span Text="{Binding Item.Product}"/>
                </FormattedString>
            </Label.FormattedText>
        </Label>
        
        <Label TextColor="Black" FontSize="14">
            <Label.FormattedText>
                <FormattedString>
                    <Span Text="Sales: " FontAttributes="Bold"/>
                    <Span Text="{Binding Item.SalesRate}"/>
                </FormattedString>
            </Label.FormattedText>
        </Label>
        
        <Label TextColor="Gray" FontSize="11">
            <Label.FormattedText>
                <FormattedString>
                    <Span Text="Market Share: "/>
                    <Span Text="{Binding Item.Percentage, StringFormat='{0:F1}%'}"/>
                </FormattedString>
            </Label.FormattedText>
        </Label>
    </StackLayout>
</DataTemplate>
```

## Best Practices

1. **Keep it Simple**: Display only essential information in tooltips
2. **Duration**: Use 2-3 seconds for optimal readability
3. **Contrast**: Ensure good contrast between text and background
4. **Font Size**: Use readable font sizes (12-16 for body text)
5. **Custom Templates**: Use templates for complex data that needs formatting
6. **Consistency**: Match tooltip styling to your app's overall design

## Troubleshooting

### Tooltip Not Showing

**Check:**
- `EnableTooltip="True"` is set on the series
- Chart is rendering correctly with data
- No overlapping UI elements blocking touch input

### Custom Template Not Displaying

**Check:**
- DataTemplate is defined in Resources with correct key
- Template is bound using `{StaticResource ...}`
- Binding paths in template match your data model properties
