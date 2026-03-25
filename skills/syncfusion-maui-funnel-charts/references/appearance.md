# Appearance Customization in .NET MAUI Funnel Chart

Customize the visual appearance of your funnel chart by defining custom color palettes and applying gradients. The `SfFunnelChart` provides flexible styling options to match your application's branding and design requirements.

## Custom PaletteBrushes

The `PaletteBrushes` property allows you to define custom colors for funnel segments. By default, the chart uses a predefined color palette, but you can override it with your own color scheme.

### XAML with ViewModel Binding

```xaml
<chart:SfFunnelChart ItemsSource="{Binding Data}" 
                     XBindingPath="XValue"
                     YBindingPath="YValue"
                     PaletteBrushes="{Binding CustomBrushes}">
    <!-- Chart configuration -->
</chart:SfFunnelChart>
```

### ViewModel with Custom Colors

```csharp
public class FunnelChartViewModel
{
    public ObservableCollection<FunnelDataModel> Data { get; set; }
    public List<Brush> CustomBrushes { get; set; }

    public FunnelChartViewModel()
    {
        // Define custom color palette
        CustomBrushes = new List<Brush>
        {
            new SolidColorBrush(Color.FromRgb(38, 198, 218)),
            new SolidColorBrush(Color.FromRgb(0, 188, 212)),
            new SolidColorBrush(Color.FromRgb(0, 172, 193)),
            new SolidColorBrush(Color.FromRgb(0, 151, 167)),
            new SolidColorBrush(Color.FromRgb(0, 131, 143))
        };

        // Initialize data
        Data = new ObservableCollection<FunnelDataModel>
        {
            new FunnelDataModel { XValue = "Prospects", YValue = 320 },
            new FunnelDataModel { XValue = "Inquiries", YValue = 290 },
            new FunnelDataModel { XValue = "Applicants", YValue = 245 },
            new FunnelDataModel { XValue = "Admits", YValue = 190 },
            new FunnelDataModel { XValue = "Enrolled", YValue = 175 }
        };
    }
}

public class FunnelDataModel
{
    public string XValue { get; set; }
    public double YValue { get; set; }
}
```

### C# Direct Assignment

```csharp
SfFunnelChart chart = new SfFunnelChart();
chart.ItemsSource = viewModel.Data;
chart.XBindingPath = "XValue";
chart.YBindingPath = "YValue";

chart.PaletteBrushes = new List<Brush>
{
    new SolidColorBrush(Color.FromRgb(38, 198, 218)),
    new SolidColorBrush(Color.FromRgb(0, 188, 212)),
    new SolidColorBrush(Color.FromRgb(0, 172, 193)),
    new SolidColorBrush(Color.FromRgb(0, 151, 167)),
    new SolidColorBrush(Color.FromRgb(0, 131, 143))
};

this.Content = chart;
```

## Applying Gradients

Enhance your funnel chart with gradient effects using `LinearGradientBrush` or `RadialGradientBrush`. Gradients add depth and visual interest to segments.

### Linear Gradient Example

#### XAML with ViewModel Binding

```xaml
<chart:SfFunnelChart ItemsSource="{Binding Data}" 
                     XBindingPath="XValue"
                     YBindingPath="YValue"
                     PaletteBrushes="{Binding GradientBrushes}">
</chart:SfFunnelChart>
```

#### ViewModel with Linear Gradients

```csharp
public class FunnelChartViewModel
{
    public ObservableCollection<FunnelDataModel> Data { get; set; }
    public List<Brush> GradientBrushes { get; set; }

    public FunnelChartViewModel()
    {
        GradientBrushes = new List<Brush>();

        // Gradient 1: Blue shades
        LinearGradientBrush gradient1 = new LinearGradientBrush();
        gradient1.GradientStops = new GradientStopCollection()
        {
            new GradientStop { Offset = 1, Color = Color.FromArgb("#a3bded") },
            new GradientStop { Offset = 0, Color = Color.FromArgb("#6991c7") }
        };

        // Gradient 2: Purple shades
        LinearGradientBrush gradient2 = new LinearGradientBrush();
        gradient2.GradientStops = new GradientStopCollection()
        {
            new GradientStop { Offset = 1, Color = Color.FromArgb("#A5678E") },
            new GradientStop { Offset = 0, Color = Color.FromArgb("#E8B7D4") }
        };

        // Gradient 3: Pink shades
        LinearGradientBrush gradient3 = new LinearGradientBrush();
        gradient3.GradientStops = new GradientStopCollection()
        {
            new GradientStop { Offset = 1, Color = Color.FromArgb("#FFCAD4") },
            new GradientStop { Offset = 0, Color = Color.FromArgb("#FB7B8E") }
        };

        // Gradient 4: Orange shades
        LinearGradientBrush gradient4 = new LinearGradientBrush();
        gradient4.GradientStops = new GradientStopCollection()
        {
            new GradientStop { Offset = 1, Color = Color.FromArgb("#FDC094") },
            new GradientStop { Offset = 0, Color = Color.FromArgb("#FFE5D8") }
        };

        // Gradient 5: Green shades
        LinearGradientBrush gradient5 = new LinearGradientBrush();
        gradient5.GradientStops = new GradientStopCollection()
        {
            new GradientStop { Offset = 1, Color = Color.FromArgb("#CFF4D2") },
            new GradientStop { Offset = 0, Color = Color.FromArgb("#56C596") }
        };

        GradientBrushes.Add(gradient1);
        GradientBrushes.Add(gradient2);
        GradientBrushes.Add(gradient3);
        GradientBrushes.Add(gradient4);
        GradientBrushes.Add(gradient5);

        // Initialize data
        Data = new ObservableCollection<FunnelDataModel>
        {
            new FunnelDataModel { XValue = "Prospects", YValue = 320 },
            new FunnelDataModel { XValue = "Inquiries", YValue = 290 },
            new FunnelDataModel { XValue = "Applicants", YValue = 245 },
            new FunnelDataModel { XValue = "Admits", YValue = 190 },
            new FunnelDataModel { XValue = "Enrolled", YValue = 175 }
        };
    }
}
```

### C# Direct Gradient Assignment

```csharp
SfFunnelChart chart = new SfFunnelChart();
chart.ItemsSource = viewModel.Data;
chart.XBindingPath = "XValue";
chart.YBindingPath = "YValue";

List<Brush> gradients = new List<Brush>();

// Create gradient 1
LinearGradientBrush gradient1 = new LinearGradientBrush
{
    GradientStops = new GradientStopCollection
    {
        new GradientStop { Offset = 1, Color = Color.FromArgb("#a3bded") },
        new GradientStop { Offset = 0, Color = Color.FromArgb("#6991c7") }
    }
};

// Create gradient 2
LinearGradientBrush gradient2 = new LinearGradientBrush
{
    GradientStops = new GradientStopCollection
    {
        new GradientStop { Offset = 1, Color = Color.FromArgb("#A5678E") },
        new GradientStop { Offset = 0, Color = Color.FromArgb("#E8B7D4") }
    }
};

gradients.Add(gradient1);
gradients.Add(gradient2);
// Add more gradients as needed...

chart.PaletteBrushes = gradients;
this.Content = chart;
```

## Radial Gradient Example

```csharp
RadialGradientBrush radialGradient = new RadialGradientBrush
{
    Center = new Point(0.5, 0.5),
    Radius = 0.5,
    GradientStops = new GradientStopCollection
    {
        new GradientStop { Offset = 0, Color = Color.FromArgb("#FFD700") },
        new GradientStop { Offset = 1, Color = Color.FromArgb("#FFA500") }
    }
};

List<Brush> brushes = new List<Brush> { radialGradient };
chart.PaletteBrushes = brushes;
```

## Color Scheme Examples

### Professional Blue Theme

```csharp
CustomBrushes = new List<Brush>
{
    new SolidColorBrush(Color.FromRgb(13, 71, 161)),   // Dark blue
    new SolidColorBrush(Color.FromRgb(25, 118, 210)),  // Medium blue
    new SolidColorBrush(Color.FromRgb(66, 165, 245)),  // Light blue
    new SolidColorBrush(Color.FromRgb(144, 202, 249)), // Lighter blue
    new SolidColorBrush(Color.FromRgb(187, 222, 251))  // Lightest blue
};
```

### Warm Sunset Theme

```csharp
CustomBrushes = new List<Brush>
{
    new SolidColorBrush(Color.FromRgb(230, 74, 25)),   // Deep orange
    new SolidColorBrush(Color.FromRgb(244, 143, 177)), // Pink
    new SolidColorBrush(Color.FromRgb(255, 179, 0)),   // Amber
    new SolidColorBrush(Color.FromRgb(255, 213, 79)),  // Yellow
    new SolidColorBrush(Color.FromRgb(255, 238, 88))   // Light yellow
};
```

### Corporate Green Theme

```csharp
CustomBrushes = new List<Brush>
{
    new SolidColorBrush(Color.FromRgb(27, 94, 32)),    // Dark green
    new SolidColorBrush(Color.FromRgb(56, 142, 60)),   // Medium green
    new SolidColorBrush(Color.FromRgb(102, 187, 106)), // Light green
    new SolidColorBrush(Color.FromRgb(165, 214, 167)), // Lighter green
    new SolidColorBrush(Color.FromRgb(200, 230, 201))  // Lightest green
};
```

### Monochrome Gray Theme

```csharp
CustomBrushes = new List<Brush>
{
    new SolidColorBrush(Color.FromRgb(33, 33, 33)),    // Very dark gray
    new SolidColorBrush(Color.FromRgb(97, 97, 97)),    // Dark gray
    new SolidColorBrush(Color.FromRgb(158, 158, 158)), // Medium gray
    new SolidColorBrush(Color.FromRgb(189, 189, 189)), // Light gray
    new SolidColorBrush(Color.FromRgb(224, 224, 224))  // Very light gray
};
```

## Complete Example with Custom Appearance

### XAML
```xaml
<ContentPage xmlns:chart="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts"
             xmlns:model="clr-namespace:YourApp.ViewModels">

    <chart:SfFunnelChart ItemsSource="{Binding Data}" 
                         XBindingPath="Stage"
                         YBindingPath="Count"
                         PaletteBrushes="{Binding CustomBrushes}"
                         ShowDataLabels="True">

        <chart:SfFunnelChart.Title>
            <Label Text="Sales Conversion Funnel"
                   FontSize="20"
                   FontAttributes="Bold"
                   HorizontalTextAlignment="Center"/>
        </chart:SfFunnelChart.Title>

        <chart:SfFunnelChart.BindingContext>
            <model:FunnelChartViewModel/>
        </chart:SfFunnelChart.BindingContext>

        <chart:SfFunnelChart.Legend>
            <chart:ChartLegend Placement="Bottom"/>
        </chart:SfFunnelChart.Legend>

    </chart:SfFunnelChart>

</ContentPage>
```

### C# Complete Implementation

```csharp
public class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();

        SfFunnelChart chart = new SfFunnelChart();
        
        // Set title
        chart.Title = new Label
        {
            Text = "Sales Conversion Funnel",
            FontSize = 20,
            FontAttributes = FontAttributes.Bold,
            HorizontalTextAlignment = TextAlignment.Center
        };

        // Create ViewModel
        FunnelChartViewModel viewModel = new FunnelChartViewModel();
        chart.BindingContext = viewModel;

        // Bind data
        chart.ItemsSource = viewModel.Data;
        chart.XBindingPath = "Stage";
        chart.YBindingPath = "Count";

        // Apply custom colors
        chart.PaletteBrushes = viewModel.CustomBrushes;

        // Enable data labels
        chart.ShowDataLabels = true;

        // Add legend
        chart.Legend = new ChartLegend { Placement = LegendPlacement.Bottom };

        this.Content = chart;
    }
}
```

## Best Practices for Appearance Customization

1. **Color Selection:**
   - Choose colors that align with your brand identity
   - Ensure sufficient contrast between adjacent segments
   - Use color progression to show hierarchy or flow

2. **Gradients:**
   - Use gradients sparingly for visual interest without overwhelming
   - Keep gradient transitions smooth (similar hues)
   - Test gradients on different screen sizes and resolutions

3. **Accessibility:**
   - Avoid red-green combinations for colorblind users
   - Provide sufficient contrast ratios (WCAG guidelines)
   - Don't rely solely on color to convey information

4. **Consistency:**
   - Use the same color palette across related charts
   - Maintain consistent segment colors for the same categories
   - Match chart colors to your application's theme

5. **Performance:**
   - Limit the number of gradient stops for better performance
   - Use solid colors when gradients aren't necessary
   - Test appearance on target devices

## Troubleshooting

**Custom colors not appearing:**
- Verify `PaletteBrushes` is correctly bound or assigned
- Ensure the list contains enough colors for all segments
- Check that colors are defined with valid RGB/Hex values

**Gradients look incorrect:**
- Verify `GradientStop` offsets are between 0 and 1
- Check gradient direction (linear vs radial)
- Ensure `GradientStopCollection` is properly initialized

**Colors conflict with data labels:**
- Adjust `DataLabelSettings` `TextColor` for contrast
- Use `UseSeriesPalette` carefully with custom colors
- Consider using `Background` in `LabelStyle` for readability
