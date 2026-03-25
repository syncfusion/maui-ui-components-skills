# Tooltip in .NET MAUI Funnel Chart

Tooltips provide additional information when hovering over funnel segments, enhancing user interaction and data comprehension. By default, tooltips display the Y value (segment value), but you can customize appearance and content extensively.

## Enable Tooltip

Set the `EnableTooltip` property to `true` on `SfFunnelChart` to enable tooltips. The default value is `false`.

### XAML
```xaml
<chart:SfFunnelChart EnableTooltip="True"
                     ItemsSource="{Binding Data}"
                     XBindingPath="XValue"
                     YBindingPath="YValue">
    <!-- Chart configuration -->
</chart:SfFunnelChart>
```

### C#
```csharp
SfFunnelChart chart = new SfFunnelChart();
chart.ItemsSource = viewModel.Data;
chart.XBindingPath = "XValue";
chart.YBindingPath = "YValue";
chart.EnableTooltip = true;

this.Content = chart;
```

## Tooltip Behavior Customization

Use `ChartTooltipBehavior` to customize tooltip appearance and behavior. Create an instance and assign it to the `TooltipBehavior` property.

### Available Properties

| Property | Type | Description |
|----------|------|-------------|
| `Background` | Brush | Background color of tooltip |
| `FontAttributes` | FontAttributes | Font style (Bold, Italic, None) |
| `FontFamily` | string | Font family for tooltip text |
| `FontSize` | float | Font size |
| `Duration` | int | Display duration in seconds |
| `Margin` | Thickness | Margin around tooltip content |
| `TextColor` | Color | Text color |

### XAML
```xaml
<chart:SfFunnelChart EnableTooltip="True">
    <chart:SfFunnelChart.TooltipBehavior>
        <chart:ChartTooltipBehavior Duration="4"
                                   Background="LightBlue"
                                   TextColor="Black"
                                   FontSize="14"
                                   FontAttributes="Bold"
                                   Margin="10"/>
    </chart:SfFunnelChart.TooltipBehavior>
</chart:SfFunnelChart>
```

### C#
```csharp
SfFunnelChart chart = new SfFunnelChart();
chart.EnableTooltip = true;

chart.TooltipBehavior = new ChartTooltipBehavior()
{
    Duration = 4,
    Background = new SolidColorBrush(Colors.LightBlue),
    TextColor = Colors.Black,
    FontSize = 14,
    FontAttributes = FontAttributes.Bold,
    Margin = new Thickness(10)
};

this.Content = chart;
```

## Tooltip Template

Use `TooltipTemplate` to create custom tooltip layouts that display additional information beyond the default Y value.

### Template Binding Context

The tooltip template's binding context provides access to:
- `Item.XValue` - The segment's X-axis value (category/label)
- `Item.YValue` - The segment's Y-axis value (numeric data)

### XAML with Resource Dictionary

```xaml
<Grid x:Name="grid">
    <Grid.Resources>
        <DataTemplate x:Key="tooltipTemplate">
            <StackLayout Orientation="Horizontal">
                <Label Text="{Binding Item.XValue}"
                       TextColor="White"
                       FontAttributes="Bold"
                       HorizontalOptions="Center"
                       VerticalOptions="Center"/>
                <Label Text="{Binding Item.YValue, StringFormat=': {0}'}"
                       TextColor="White"
                       FontAttributes="Bold"
                       HorizontalOptions="Center"
                       VerticalOptions="Center"/>
            </StackLayout>
        </DataTemplate>
    </Grid.Resources>

    <chart:SfFunnelChart EnableTooltip="True"
                         TooltipTemplate="{StaticResource tooltipTemplate}">
        <!-- Chart configuration -->
    </chart:SfFunnelChart>
</Grid>
```

### C# with DataTemplate

```csharp
SfFunnelChart chart = new SfFunnelChart();
chart.EnableTooltip = true;
chart.TooltipTemplate = grid.Resources["tooltipTemplate"] as DataTemplate;

this.Content = chart;
```

## Custom Tooltip Examples

### Example 1: Simple Custom Tooltip

```xaml
<DataTemplate x:Key="simpleTooltip">
    <Frame Background="#2C3E50" 
           Padding="12" 
           CornerRadius="8"
           HasShadow="True">
        <Label Text="{Binding Item.YValue, StringFormat='Value: {0:N0}'}"
               TextColor="White"
               FontSize="16"
               FontAttributes="Bold"/>
    </Frame>
</DataTemplate>
```

### Example 2: Detailed Tooltip with Multiple Values

```xaml
<DataTemplate x:Key="detailedTooltip">
    <Border Background="White"
            Stroke="Gray"
            StrokeThickness="1"
            Padding="15">
        <VerticalStackLayout Spacing="8">
            <Label Text="{Binding Item.XValue}"
                   FontSize="18"
                   FontAttributes="Bold"
                   TextColor="Black"/>
            <BoxView HeightRequest="1" 
                     Background="LightGray"/>
            <HorizontalStackLayout Spacing="5">
                <Label Text="Count:"
                       FontSize="14"
                       TextColor="Gray"/>
                <Label Text="{Binding Item.YValue, StringFormat='{0:N0}'}"
                       FontSize="14"
                       FontAttributes="Bold"
                       TextColor="Black"/>
            </HorizontalStackLayout>
        </VerticalStackLayout>
    </Border>
</DataTemplate>
```

### Example 3: Tooltip with Icon and Percentage

```xaml
<DataTemplate x:Key="iconTooltip">
    <Grid Background="#34495E" 
          Padding="12">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto"/>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>
        
        <!-- Icon placeholder -->
        <BoxView Grid.Column="0"
                 WidthRequest="24"
                 HeightRequest="24"
                 Background="{Binding IconBrush}"
                 CornerRadius="12"
                 Margin="0,0,10,0"/>
        
        <VerticalStackLayout Grid.Column="1" Spacing="4">
            <Label Text="{Binding Item.XValue}"
                   TextColor="White"
                   FontSize="14"
                   FontAttributes="Bold"/>
            <Label Text="{Binding Item.YValue, StringFormat='{0:N0} users'}"
                   TextColor="LightGray"
                   FontSize="12"/>
        </VerticalStackLayout>
    </Grid>
</DataTemplate>
```

### Example 4: C# Code-Behind Custom Template

```csharp
public DataTemplate CreateCustomTooltipTemplate()
{
    return new DataTemplate(() =>
    {
        var frame = new Frame
        {
            BackgroundColor = Color.FromArgb("#16A085"),
            Padding = new Thickness(15),
            CornerRadius = 10,
            HasShadow = true
        };

        var stackLayout = new VerticalStackLayout { Spacing = 5 };

        var stageLabel = new Label
        {
            FontSize = 16,
            FontAttributes = FontAttributes.Bold,
            TextColor = Colors.White
        };
        stageLabel.SetBinding(Label.TextProperty, "Item.XValue");

        var valueLabel = new Label
        {
            FontSize = 14,
            TextColor = Colors.White
        };
        valueLabel.SetBinding(Label.TextProperty, new Binding("Item.YValue", stringFormat: "Count: {0:N0}"));

        stackLayout.Children.Add(stageLabel);
        stackLayout.Children.Add(valueLabel);
        frame.Content = stackLayout;

        return frame;
    });
}

// Usage
chart.TooltipTemplate = CreateCustomTooltipTemplate();
```

## Complete Working Example

### XAML
```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:chart="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts"
             xmlns:model="clr-namespace:YourApp.ViewModels">

    <Grid x:Name="grid">
        <Grid.Resources>
            <DataTemplate x:Key="tooltipTemplate">
                <Frame Background="#2980B9" 
                       Padding="12" 
                       CornerRadius="8">
                    <VerticalStackLayout Spacing="5">
                        <Label Text="{Binding Item.XValue}"
                               TextColor="White"
                               FontSize="16"
                               FontAttributes="Bold"/>
                        <Label Text="{Binding Item.YValue, StringFormat='Value: {0:N0}'}"
                               TextColor="White"
                               FontSize="14"/>
                    </VerticalStackLayout>
                </Frame>
            </DataTemplate>
        </Grid.Resources>

        <chart:SfFunnelChart ItemsSource="{Binding Data}"
                             XBindingPath="Stage"
                             YBindingPath="Count"
                             EnableTooltip="True"
                             TooltipTemplate="{StaticResource tooltipTemplate}">

            <chart:SfFunnelChart.BindingContext>
                <model:SalesFunnelViewModel/>
            </chart:SfFunnelChart.BindingContext>

            <chart:SfFunnelChart.TooltipBehavior>
                <chart:ChartTooltipBehavior Duration="3"/>
            </chart:SfFunnelChart.TooltipBehavior>

        </chart:SfFunnelChart>
    </Grid>

</ContentPage>
```

### C# Code-Behind
```csharp
using Syncfusion.Maui.Charts;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();

        Grid grid = new Grid();
        
        // Create custom tooltip template
        DataTemplate tooltipTemplate = new DataTemplate(() =>
        {
            var frame = new Frame
            {
                BackgroundColor = Color.FromArgb("#2980B9"),
                Padding = new Thickness(12),
                CornerRadius = 8
            };

            var stackLayout = new VerticalStackLayout { Spacing = 5 };

            var stageLabel = new Label
            {
                FontSize = 16,
                FontAttributes = FontAttributes.Bold,
                TextColor = Colors.White
            };
            stageLabel.SetBinding(Label.TextProperty, "Item.XValue");

            var valueLabel = new Label
            {
                FontSize = 14,
                TextColor = Colors.White
            };
            valueLabel.SetBinding(Label.TextProperty, 
                new Binding("Item.YValue", stringFormat: "Value: {0:N0}"));

            stackLayout.Children.Add(stageLabel);
            stackLayout.Children.Add(valueLabel);
            frame.Content = stackLayout;

            return frame;
        });

        // Create chart
        SfFunnelChart chart = new SfFunnelChart();
        SalesFunnelViewModel viewModel = new SalesFunnelViewModel();
        chart.BindingContext = viewModel;

        chart.ItemsSource = viewModel.Data;
        chart.XBindingPath = "Stage";
        chart.YBindingPath = "Count";
        chart.EnableTooltip = true;
        chart.TooltipTemplate = tooltipTemplate;

        chart.TooltipBehavior = new ChartTooltipBehavior
        {
            Duration = 3
        };

        grid.Children.Add(chart);
        this.Content = grid;
    }
}
```

## Best Practices

1. **Content:**
   - Keep tooltip content concise and relevant
   - Show the most important information first
   - Use clear labels for numeric values

2. **Styling:**
   - Ensure sufficient color contrast for readability
   - Use consistent styling across your application
   - Avoid overly complex layouts that slow rendering

3. **Duration:**
   - Set appropriate `Duration` based on content complexity
   - Default (2 seconds) works for simple tooltips
   - Increase for tooltips with more information

4. **Template Design:**
   - Use padding and margins for breathing room
   - Keep template size reasonable (not too large)
   - Test on different screen sizes

5. **Accessibility:**
   - Use readable font sizes (minimum 12-14)
   - Ensure text contrasts with background
   - Consider users with visual impairments

## Troubleshooting

**Tooltip not appearing:**
- Verify `EnableTooltip="True"` is set
- Check that chart has data bound correctly
- Ensure segments are not too small to hover over

**Tooltip displays default format:**
- Confirm `TooltipTemplate` is properly assigned
- Verify resource key matches in XAML
- Check binding paths in template

**Tooltip styling not applied:**
- Ensure `TooltipBehavior` is set before rendering
- Verify brush/color values are valid
- Check for binding errors in debug output

**Custom template not showing data:**
- Use correct binding path: `Item.XValue` or `Item.YValue`
- Verify ViewModel properties match binding paths
- Check for null values in data source

**Tooltip disappears too quickly:**
- Increase `Duration` property value
- Duration is in seconds (e.g., `Duration="5"` for 5 seconds)
