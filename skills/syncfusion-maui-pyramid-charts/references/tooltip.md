# Tooltip in Pyramid Charts

This guide covers enabling and customizing tooltips in Syncfusion .NET MAUI Pyramid Charts for displaying segment information on hover.

## Overview

Tooltips provide additional information when hovering over pyramid segments. By default, tooltips display the Y value (numeric data) of the segment. They enhance user interaction by showing data without cluttering the chart with permanent labels.

**Key Features:**
- Enable with a single property
- Display segment data on hover
- Customizable appearance (fonts, colors, backgrounds)
- Control display duration
- Custom templates for advanced layouts
- Bind to data model properties

## Enable Tooltip

Set the `EnableTooltip` property to `true` on the SfPyramidChart. Default is `false`.

### XAML

```xaml
<chart:SfPyramidChart ItemsSource="{Binding Data}"
                      XBindingPath="Name"
                      YBindingPath="Value"
                      EnableTooltip="True"/>
```

### C#

```csharp
SfPyramidChart chart = new SfPyramidChart();
chart.ItemsSource = viewModel.Data;
chart.XBindingPath = "Name";
chart.YBindingPath = "Value";
chart.EnableTooltip = true;
this.Content = chart;
```

**Result:** Hover over any pyramid segment to see a tooltip displaying the Y value.

## Tooltip Customization

Use the `TooltipBehavior` property with a `ChartTooltipBehavior` instance to customize tooltip appearance and behavior.

### Available Properties

| Property | Type | Description |
|----------|------|-------------|
| **Background** | Brush | Background color of the tooltip |
| **TextColor** | Color | Color of the tooltip text |
| **FontSize** | float | Font size of the tooltip text |
| **FontFamily** | string | Font family for the tooltip text |
| **FontAttributes** | FontAttributes | Font style (Bold, Italic, None) |
| **Duration** | int | Display duration in seconds |
| **Margin** | Thickness | Margin around the tooltip content |

### Basic Customization

**XAML:**
```xaml
<chart:SfPyramidChart ItemsSource="{Binding Data}"
                      XBindingPath="Name"
                      YBindingPath="Value"
                      EnableTooltip="True">
    
    <chart:SfPyramidChart.TooltipBehavior>
        <chart:ChartTooltipBehavior Duration="4"/>
    </chart:SfPyramidChart.TooltipBehavior>
    
</chart:SfPyramidChart>
```

**C#:**
```csharp
SfPyramidChart chart = new SfPyramidChart();
chart.ItemsSource = viewModel.Data;
chart.XBindingPath = "Name";
chart.YBindingPath = "Value";
chart.EnableTooltip = true;

chart.TooltipBehavior = new ChartTooltipBehavior()
{
    Duration = 4  // Show tooltip for 4 seconds
};

this.Content = chart;
```

### Comprehensive Styling

**XAML:**
```xaml
<chart:SfPyramidChart EnableTooltip="True">
    <chart:SfPyramidChart.TooltipBehavior>
        <chart:ChartTooltipBehavior Background="DarkBlue"
                                   TextColor="White"
                                   FontSize="14"
                                   FontFamily="Arial"
                                   FontAttributes="Bold"
                                   Duration="3"
                                   Margin="10"/>
    </chart:SfPyramidChart.TooltipBehavior>
</chart:SfPyramidChart>
```

**C#:**
```csharp
chart.TooltipBehavior = new ChartTooltipBehavior()
{
    Background = Brush.DarkBlue,
    TextColor = Colors.White,
    FontSize = 14,
    FontFamily = "Arial",
    FontAttributes = FontAttributes.Bold,
    Duration = 3,
    Margin = 10
};
```

## Duration Control

The `Duration` property controls how long the tooltip remains visible after the pointer moves away from the segment (in seconds).

### Examples

```csharp
// Show for 2 seconds
chart.TooltipBehavior = new ChartTooltipBehavior() { Duration = 2 };

// Show for 5 seconds
chart.TooltipBehavior = new ChartTooltipBehavior() { Duration = 5 };

// Show indefinitely (until pointer moves to another segment)
chart.TooltipBehavior = new ChartTooltipBehavior() { Duration = 0 };
```

**Recommendation:** Use 2-4 seconds for most scenarios. Shorter durations (1-2s) for fast scanning, longer durations (4-6s) for complex data.

## Tooltip Template

Create custom tooltip layouts using the `TooltipTemplate` property with a `DataTemplate`. This allows showing multiple data points, icons, or formatted content.

### Template Binding Context

The BindingContext provides access to the data item via `Item` property:

- **Item**: The underlying data model (e.g., StageModel)
- Access all properties: `Item.Name`, `Item.Value`, etc.

### Basic Custom Template

**XAML:**
```xaml
<Grid x:Name="grid">
    <Grid.Resources>
        <DataTemplate x:Key="tooltipTemplate">
            <StackLayout Orientation="Horizontal" Padding="10">
                <Label Text="{Binding Item.Name}"
                       TextColor="White"
                       FontAttributes="Bold"
                       FontSize="14"
                       VerticalOptions="Center"/>
                <Label Text=": "
                       TextColor="White"
                       FontSize="14"
                       VerticalOptions="Center"/>
                <Label Text="{Binding Item.Value}"
                       TextColor="White"
                       FontAttributes="Bold"
                       FontSize="14"
                       VerticalOptions="Center"/>
            </StackLayout>
        </DataTemplate>
    </Grid.Resources>

    <chart:SfPyramidChart EnableTooltip="True"
                          TooltipTemplate="{StaticResource tooltipTemplate}">
        <!-- Chart configuration -->
    </chart:SfPyramidChart>
</Grid>
```

**C#:**
```csharp
SfPyramidChart chart = new SfPyramidChart();
chart.EnableTooltip = true;
chart.TooltipTemplate = grid.Resources["tooltipTemplate"] as DataTemplate;
this.Content = chart;
```

### Advanced Template with Multiple Data Points

Assuming your data model has additional properties:

```csharp
public class StageModel
{
    public string Name { get; set; }
    public double Value { get; set; }
    public double Percentage { get; set; }
    public string Description { get; set; }
}
```

**XAML Template:**
```xaml
<DataTemplate x:Key="advancedTooltipTemplate">
    <StackLayout Padding="15" Spacing="5">
        <Label Text="{Binding Item.Name}"
               TextColor="White"
               FontSize="16"
               FontAttributes="Bold"/>
        
        <BoxView HeightRequest="1" 
                 Color="White" 
                 Opacity="0.3"
                 Margin="0,5"/>
        
        <Grid ColumnDefinitions="Auto,*" RowDefinitions="Auto,Auto,Auto">
            <Label Grid.Row="0" Grid.Column="0" 
                   Text="Value: " 
                   TextColor="LightGray"
                   FontSize="12"/>
            <Label Grid.Row="0" Grid.Column="1" 
                   Text="{Binding Item.Value}" 
                   TextColor="White"
                   FontSize="12"
                   HorizontalOptions="End"/>
            
            <Label Grid.Row="1" Grid.Column="0" 
                   Text="Percentage: " 
                   TextColor="LightGray"
                   FontSize="12"/>
            <Label Grid.Row="1" Grid.Column="1" 
                   Text="{Binding Item.Percentage, StringFormat='{0:P0}'}" 
                   TextColor="White"
                   FontSize="12"
                   HorizontalOptions="End"/>
            
            <Label Grid.Row="2" Grid.Column="0" 
                   Grid.ColumnSpan="2"
                   Text="{Binding Item.Description}" 
                   TextColor="LightGray"
                   FontSize="11"
                   Margin="0,5,0,0"/>
        </Grid>
    </StackLayout>
</DataTemplate>
```

### Template with Icon

```xaml
<DataTemplate x:Key="iconTooltipTemplate">
    <Grid Padding="12">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto"/>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>
        
        <Image Grid.Column="0" 
               Source="info_icon.png" 
               HeightRequest="24" 
               WidthRequest="24"
               VerticalOptions="Center"
               Margin="0,0,10,0"/>
        
        <StackLayout Grid.Column="1">
            <Label Text="{Binding Item.Name}"
                   TextColor="White"
                   FontSize="14"
                   FontAttributes="Bold"/>
            <Label Text="{Binding Item.Value, StringFormat='Count: {0}'}"
                   TextColor="LightGray"
                   FontSize="12"/>
        </StackLayout>
    </Grid>
</DataTemplate>
```

### Conditional Formatting in Template

```xaml
<DataTemplate x:Key="conditionalTooltipTemplate">
    <Border Padding="12" 
            BackgroundColor="Transparent"
            Stroke="White"
            StrokeThickness="1">
        <StackLayout>
            <Label Text="{Binding Item.Name}"
                   TextColor="White"
                   FontSize="14"/>
            <Label FontSize="16" FontAttributes="Bold">
                <Label.FormattedText>
                    <FormattedString>
                        <Span Text="Value: " 
                              TextColor="White"/>
                        <Span Text="{Binding Item.Value}" 
                              TextColor="{Binding Item.Value, Converter={StaticResource ValueToColorConverter}}"/>
                    </FormattedString>
                </Label.FormattedText>
            </Label>
        </StackLayout>
    </Border>
</DataTemplate>
```

## Complete Examples

### Example 1: Styled Tooltip with Duration

```xaml
<chart:SfPyramidChart ItemsSource="{Binding Data}"
                      XBindingPath="Name"
                      YBindingPath="Value"
                      EnableTooltip="True">
    
    <chart:SfPyramidChart.TooltipBehavior>
        <chart:ChartTooltipBehavior Background="#2196F3"
                                   TextColor="White"
                                   FontSize="14"
                                   FontAttributes="Bold"
                                   Duration="3"
                                   Margin="8"/>
    </chart:SfPyramidChart.TooltipBehavior>
    
</chart:SfPyramidChart>
```

### Example 2: Custom Template with Name and Value

```xaml
<Grid>
    <Grid.Resources>
        <DataTemplate x:Key="customTooltip">
            <Border BackgroundColor="#1E1E1E" 
                    Padding="15" 
                    Stroke="White"
                    StrokeThickness="1.5">
                <StackLayout Spacing="5">
                    <Label Text="{Binding Item.Name}"
                           TextColor="White"
                           FontSize="15"
                           FontAttributes="Bold"
                           HorizontalOptions="Center"/>
                    <Label Text="{Binding Item.Value, StringFormat='Value: {0:N0}'}"
                           TextColor="LightGray"
                           FontSize="13"
                           HorizontalOptions="Center"/>
                </StackLayout>
            </Border>
        </DataTemplate>
    </Grid.Resources>

    <chart:SfPyramidChart EnableTooltip="True"
                          TooltipTemplate="{StaticResource customTooltip}">
        <!-- Chart content -->
    </chart:SfPyramidChart>
</Grid>
```

### Example 3: Programmatic Tooltip Setup

```csharp
SfPyramidChart chart = new SfPyramidChart();
chart.ItemsSource = viewModel.Data;
chart.XBindingPath = "Name";
chart.YBindingPath = "Value";
chart.EnableTooltip = true;

// Create custom tooltip behavior
var tooltipBehavior = new ChartTooltipBehavior()
{
    Background = new SolidColorBrush(Color.FromArgb("#424242")),
    TextColor = Colors.White,
    FontSize = 13,
    FontFamily = "Segoe UI",
    FontAttributes = FontAttributes.None,
    Duration = 4,
    Margin = new Thickness(12)
};

chart.TooltipBehavior = tooltipBehavior;
this.Content = chart;
```

## Best Practices

### When to Use Tooltips vs Data Labels

**Use Tooltips when:**
- Chart has many small segments
- Reducing visual clutter is important
- User needs data only on demand
- Working with touch or pointer interactions

**Use Data Labels when:**
- Key data should always be visible
- Chart has few segments
- Print or screenshot friendliness matters
- Accessibility is a priority

### Styling Guidelines

- **Background**: Use semi-transparent dark colors for contrast
- **Text Color**: Ensure high contrast with background
- **Font Size**: 12-14px for readability
- **Duration**: 2-4 seconds for most cases
- **Margins**: 8-12px for comfortable spacing

### Template Design Tips

- Keep templates concise and scannable
- Show only essential information
- Use hierarchy (bold headers, lighter details)
- Ensure sufficient padding around content
- Test tooltip size doesn't become too large

### Performance Considerations

- Tooltips have minimal performance impact
- Complex templates may increase rendering time slightly
- Avoid binding to computed properties that are expensive
- Test on target devices, especially mobile

## Integration with Liquid Glass Effects

For iOS/macOS applications with liquid glass styling, ensure tooltip templates work well with glass effects:

```xaml
<chart:SfPyramidChart EnableTooltip="True" 
                      EnableLiquidGlassEffect="True">
    <chart:SfPyramidChart.TooltipBehavior>
        <chart:ChartTooltipBehavior Background="Transparent"/>
    </chart:SfPyramidChart.TooltipBehavior>
</chart:SfPyramidChart>
```

**Note:** When using custom `TooltipTemplate` with liquid glass effects, set the template's background to `Transparent` to allow the glass effect to show through.

See [orientation-and-effects.md](orientation-and-effects.md) for more details on liquid glass effects.

## Common Issues and Solutions

### Issue: Tooltip Not Showing

**Possible Causes:**
- `EnableTooltip` is `false`
- Chart not added to visual tree
- Data source is empty
- Pointer/touch not working

**Solutions:**
- Verify `EnableTooltip="True"`
- Ensure chart is displayed on screen
- Check data binding is working
- Test on actual device (not just emulator)

### Issue: Tooltip Disappears Too Quickly

**Solution:**
```csharp
chart.TooltipBehavior = new ChartTooltipBehavior() 
{ 
    Duration = 5  // Increase duration
};
```

### Issue: Custom Template Not Displaying

**Solutions:**
- Ensure `TooltipTemplate` resource key matches
- Check binding paths (use `Item.PropertyName`)
- Verify data model has required properties
- Test with simple template first, then add complexity

### Issue: Text Overlapping in Template

**Solutions:**
- Add proper spacing in StackLayout/Grid
- Use margins and padding appropriately
- Reduce font sizes if needed
- Consider wrapping text for long content

## Related Features

- **Data Labels**: See [data-labels.md](data-labels.md) for permanent on-segment labels
- **Legend**: See [legend.md](legend.md) for segment identification
- **Liquid Glass Effects**: See [orientation-and-effects.md](orientation-and-effects.md)