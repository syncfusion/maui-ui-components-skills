# Center View in .NET MAUI Sunburst Chart

Custom views can be added to the center of the sunburst chart to display additional information, summary data, or interactive controls. The center view is especially useful for showing aggregated metrics, branding, or contextual information related to the displayed hierarchy.

## Overview

The `CenterView` property allows adding any MAUI view to the chart's center area. This view sits in the hollow center created by the `InnerRadius` property and can be bound to chart data for dynamic updates.

**Key features:**
- Display any MAUI view (Label, StackLayout, Grid, custom controls)
- Bind to chart data via BindingContext
- Responsive sizing with CenterHoleSize
- Prevents overlap with segments

## Basic Center View

### Adding a Simple Label

**XAML:**
```xml
<sunburst:SfSunburstChart ItemsSource="{Binding DataSource}"
                          ValueMemberPath="EmployeesCount"
                          InnerRadius="0.5">
    
    <sunburst:SfSunburstChart.CenterView>
        <Label Text="Total Employees" 
             FontSize="16" 
             FontAttributes="Bold"
             HorizontalOptions="Center" 
             VerticalOptions="Center"/>
    </sunburst:SfSunburstChart.CenterView>
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Department"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
SfSunburstChart sunburst = new SfSunburstChart();
sunburst.InnerRadius = 0.5;

Label centerLabel = new Label
{
    Text = "Total Employees",
    FontSize = 16,
    FontAttributes = FontAttributes.Bold,
    HorizontalOptions = LayoutOptions.Center,
    VerticalOptions = LayoutOptions.Center
};
sunburst.CenterView = centerLabel;

// ... configure data and levels
this.Content = sunburst;
```

## CenterHoleSize Property

The `CenterHoleSize` property returns the diameter of the center hole in device-independent units. Use this to size your center view appropriately and prevent overlap with segments.

**Binding to CenterHoleSize:**

**XAML:**
```xml
<sunburst:SfSunburstChart x:Name="sunburst" 
                          ItemsSource="{Binding DataSource}"
                          ValueMemberPath="Sales"
                          InnerRadius="0.6">
    
    <sunburst:SfSunburstChart.CenterView>
        <Border HeightRequest="{Binding Source={x:Reference sunburst}, Path=CenterHoleSize}" 
               WidthRequest="{Binding Source={x:Reference sunburst}, Path=CenterHoleSize}"
               BackgroundColor="LightGray">
            <Label Text="Sales Data" 
                 HorizontalOptions="Center" 
                 VerticalOptions="Center"/>
        </Border>
    </sunburst:SfSunburstChart.CenterView>
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Region"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Product"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

**Why use CenterHoleSize:**
- Ensures center view fits perfectly in available space
- Prevents overlap with inner ring segments
- Maintains proper sizing when chart resizes
- Creates balanced, professional appearance

## Complex Center View Examples

### Example 1: Multi-Line Summary

```xml
<sunburst:SfSunburstChart ItemsSource="{Binding DataSource}"
                          ValueMemberPath="Count"
                          InnerRadius="0.55">
    
    <sunburst:SfSunburstChart.CenterView>
        <VerticalStackLayout HorizontalOptions="Center" 
                            VerticalOptions="Center"
                            Spacing="4">
            <Label Text="Total" 
                 FontSize="12" 
                 TextColor="Gray"
                 HorizontalTextAlignment="Center"/>
            <Label Text="{Binding TotalEmployees}" 
                 FontSize="32" 
                 FontAttributes="Bold"
                 HorizontalTextAlignment="Center"/>
            <Label Text="Employees" 
                 FontSize="14" 
                 TextColor="Gray"
                 HorizontalTextAlignment="Center"/>
        </VerticalStackLayout>
    </sunburst:SfSunburstChart.CenterView>
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Department"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Team"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

### Example 2: Styled Border with Content

```xml
<sunburst:SfSunburstChart x:Name="chart" 
                          ItemsSource="{Binding DataSource}"
                          ValueMemberPath="Revenue"
                          InnerRadius="0.5">
    
    <sunburst:SfSunburstChart.CenterView>
        <Border HeightRequest="{Binding Source={x:Reference chart}, Path=CenterHoleSize}" 
               WidthRequest="{Binding Source={x:Reference chart}, Path=CenterHoleSize}" 
               BackgroundColor="GhostWhite"
               StrokeThickness="2"
               Stroke="LightGray">
            <Border.StrokeShape>
                <RoundRectangle CornerRadius="150"/>
            </Border.StrokeShape>
            <Border.Shadow>
                <Shadow Brush="Black" Opacity="0.3" Radius="20"/>
            </Border.Shadow>
            <VerticalStackLayout HorizontalOptions="Center" 
                                VerticalOptions="Center">
                <Label Text="2026 Revenue" 
                     FontSize="14" 
                     FontAttributes="Bold"
                     HorizontalTextAlignment="Center"/>
                <Label Text="{Binding TotalRevenue, StringFormat='${0:N0}M'}" 
                     FontSize="24" 
                     FontAttributes="Bold"
                     TextColor="Green"
                     HorizontalTextAlignment="Center"/>
                <Label Text="+12% YoY" 
                     FontSize="12" 
                     TextColor="Green"
                     HorizontalTextAlignment="Center"/>
            </VerticalStackLayout>
        </Border>
    </sunburst:SfSunburstChart.CenterView>
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Region"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Product"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

### Example 3: Icon with Text

```xml
<sunburst:SfSunburstChart ItemsSource="{Binding DataSource}"
                          ValueMemberPath="Count"
                          InnerRadius="0.6">
    
    <sunburst:SfSunburstChart.CenterView>
        <VerticalStackLayout HorizontalOptions="Center" 
                            VerticalOptions="Center"
                            Spacing="8">
            <Image Source="company_logo.png" 
                 HeightRequest="50" 
                 WidthRequest="50"/>
            <Label Text="Organization" 
                 FontSize="16" 
                 FontAttributes="Bold"
                 HorizontalTextAlignment="Center"/>
            <Label Text="{Binding TotalCount, StringFormat='{0} People'}" 
                 FontSize="14" 
                 TextColor="Gray"
                 HorizontalTextAlignment="Center"/>
        </VerticalStackLayout>
    </sunburst:SfSunburstChart.CenterView>
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Division"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Department"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

### Example 4: Interactive Button

```xml
<sunburst:SfSunburstChart ItemsSource="{Binding DataSource}"
                          ValueMemberPath="Amount"
                          InnerRadius="0.5">
    
    <sunburst:SfSunburstChart.CenterView>
        <VerticalStackLayout HorizontalOptions="Center" 
                            VerticalOptions="Center"
                            Spacing="10">
            <Label Text="Budget Overview" 
                 FontSize="14" 
                 HorizontalTextAlignment="Center"/>
            <Label Text="{Binding TotalBudget, StringFormat='${0:N0}'}" 
                 FontSize="20" 
                 FontAttributes="Bold"
                 HorizontalTextAlignment="Center"/>
            <Button Text="View Details" 
                  FontSize="12"
                  Padding="10,5"
                  Clicked="OnViewDetailsClicked"
                  BackgroundColor="{StaticResource PrimaryColor}"/>
        </VerticalStackLayout>
    </sunburst:SfSunburstChart.CenterView>
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Department"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Category"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

## Advanced Example with Converters

Sometimes you need to use value converters to properly size or format center view content.

### Corner Radius Converter

**XAML:**
```xml
<ContentPage.Resources>
    <ResourceDictionary>
        <local:HalfValueConverter x:Key="HalfConverter"/>
    </ResourceDictionary>
</ContentPage.Resources>

<sunburst:SfSunburstChart x:Name="sunburstChart" 
                          ItemsSource="{Binding DataSource}"
                          ValueMemberPath="Value"
                          InnerRadius="0.55">
    
    <sunburst:SfSunburstChart.CenterView>
        <Border HeightRequest="{Binding Source={x:Reference sunburstChart}, Path=CenterHoleSize}" 
               WidthRequest="{Binding Source={x:Reference sunburstChart}, Path=CenterHoleSize}"
               BackgroundColor="WhiteSmoke">
            <Border.StrokeShape>
                <RoundRectangle CornerRadius="{Binding Source={x:Reference sunburstChart}, 
                                                      Path=CenterHoleSize, 
                                                      Converter={StaticResource HalfConverter}}"/>
            </Border.StrokeShape>
            <VerticalStackLayout HorizontalOptions="Center" VerticalOptions="Center">
                <Label Text="Behind the Data" 
                     FontSize="14" 
                     FontAttributes="Bold"
                     HorizontalTextAlignment="Center"/>
            </VerticalStackLayout>
        </Border>
    </sunburst:SfSunburstChart.CenterView>
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Category"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

**Converter C#:**
```csharp
public class HalfValueConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value is double doubleValue)
        {
            return doubleValue / 2;
        }
        return 0;
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```

## Dynamic Center View Updates

Update center view content based on chart interactions:

**ViewModel:**
```csharp
public class SunburstViewModel : INotifyPropertyChanged
{
    private string _selectedCategory;
    private double _selectedValue;

    public string SelectedCategory
    {
        get => _selectedCategory;
        set
        {
            _selectedCategory = value;
            OnPropertyChanged();
        }
    }

    public double SelectedValue
    {
        get => _selectedValue;
        set
        {
            _selectedValue = value;
            OnPropertyChanged();
        }
    }

    public ObservableCollection<DataModel> DataSource { get; set; }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**XAML with Dynamic Updates:**
```xml
<sunburst:SfSunburstChart ItemsSource="{Binding DataSource}"
                          ValueMemberPath="Value"
                          InnerRadius="0.6"
                          SelectionChanged="OnSelectionChanged">
    
    <sunburst:SfSunburstChart.CenterView>
        <VerticalStackLayout HorizontalOptions="Center" 
                            VerticalOptions="Center"
                            Spacing="8">
            <Label Text="Selected" 
                 FontSize="12" 
                 TextColor="Gray"
                 HorizontalTextAlignment="Center"/>
            <Label Text="{Binding SelectedCategory, FallbackValue='None'}" 
                 FontSize="18" 
                 FontAttributes="Bold"
                 HorizontalTextAlignment="Center"/>
            <Label Text="{Binding SelectedValue, StringFormat='{0:N0}', FallbackValue='-'}" 
                 FontSize="24" 
                 FontAttributes="Bold"
                 TextColor="Blue"
                 HorizontalTextAlignment="Center"/>
        </VerticalStackLayout>
    </sunburst:SfSunburstChart.CenterView>
    
    <sunburst:SfSunburstChart.SelectionSettings>
        <sunburst:SunburstSelectionSettings Type="Single"/>
    </sunburst:SfSunburstChart.SelectionSettings>
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Category"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

**Code-behind:**
```csharp
private void OnSelectionChanged(object sender, SunburstSelectionChangedEventArgs e)
{
    if (e.IsSelected && BindingContext is SunburstViewModel viewModel)
    {
        viewModel.SelectedCategory = e.NewSegment.Category;
        viewModel.SelectedValue = e.NewSegment.Value;
    }
}
```

## Best Practices

### Inner Radius Configuration

**For prominent center view:**
- Use InnerRadius 0.55-0.7
- Provides adequate space for content
- Balances with ring visibility

**For balanced layout:**
- Use InnerRadius 0.4-0.5
- Standard donut chart appearance
- Good for simple labels

**For minimal center:**
- Use InnerRadius 0.25-0.35
- Small center area
- Focus remains on hierarchy

### Content Guidelines

**Keep it concise:**
- 2-3 lines of text maximum
- Large, readable font sizes
- Clear hierarchy of information

**Use appropriate sizing:**
- Bind to CenterHoleSize for responsive layouts
- Test on various screen sizes
- Consider mobile constraints

**Maintain readability:**
- High contrast text colors
- Adequate font sizes (14+ for body text)
- Bold for emphasis

### Styling Tips

**Professional appearance:**
- Use subtle backgrounds (light gray, off-white)
- Add shadows for depth
- Round corners for polish

**Brand integration:**
- Include company logo or colors
- Match app theme
- Consistent typography

**Interactive elements:**
- Buttons should be clearly tappable
- Adequate padding and spacing
- Visual feedback on interaction

## Common Patterns

### Pattern 1: Summary Metric Display

```xml
<sunburst:SfSunburstChart.CenterView>
    <VerticalStackLayout HorizontalOptions="Center" VerticalOptions="Center">
        <Label Text="Total" FontSize="14" TextColor="Gray"/>
        <Label Text="{Binding TotalValue}" FontSize="32" FontAttributes="Bold"/>
    </VerticalStackLayout>
</sunburst:SfSunburstChart.CenterView>
```

### Pattern 2: Logo Branding

```xml
<sunburst:SfSunburstChart.CenterView>
    <Image Source="logo.png" 
         HeightRequest="80" 
         WidthRequest="80"
         HorizontalOptions="Center" 
         VerticalOptions="Center"/>
</sunburst:SfSunburstChart.CenterView>
```

### Pattern 3: Dynamic Selection Display

```xml
<sunburst:SfSunburstChart.CenterView>
    <VerticalStackLayout HorizontalOptions="Center" VerticalOptions="Center">
        <Label Text="{Binding SelectedItem, FallbackValue='Select an item'}" 
             FontSize="16" 
             FontAttributes="Bold"/>
        <Label Text="{Binding SelectedValue, StringFormat='${0:N0}'}" 
             FontSize="20"/>
    </VerticalStackLayout>
</sunburst:SfSunburstChart.CenterView>
```

### Pattern 4: Call-to-Action

```xml
<sunburst:SfSunburstChart.CenterView>
    <VerticalStackLayout HorizontalOptions="Center" VerticalOptions="Center" Spacing="10">
        <Label Text="Explore Data" FontSize="16"/>
        <Button Text="Start" Clicked="OnExploreClicked"/>
    </VerticalStackLayout>
</sunburst:SfSunburstChart.CenterView>
```

## Troubleshooting

**Issue:** Center view not visible
- **Solution:** Increase InnerRadius (try 0.5 or higher)
- Verify CenterView is set
- Check background colors aren't transparent
- Ensure HorizontalOptions and VerticalOptions are set to Center

**Issue:** Center view too small/large
- **Solution:** Bind to CenterHoleSize for automatic sizing
- Adjust InnerRadius to change available space
- Set explicit HeightRequest/WidthRequest if needed
- Test with different chart sizes

**Issue:** Center view overlaps with segments
- **Solution:** Increase InnerRadius value
- Reduce center view content size
- Use CenterHoleSize binding for proper sizing
- Add padding to prevent edge overlap

**Issue:** Text cut off or wrapped incorrectly
- **Solution:** Reduce font size
- Use shorter text
- Increase InnerRadius for more space
- Set LineBreakMode on labels

**Issue:** Center view not updating dynamically
- **Solution:** Verify BindingContext is set correctly
- Implement INotifyPropertyChanged in ViewModel
- Check binding paths are correct
- Use FallbackValue for debugging

**Issue:** Blurry or pixelated content
- **Solution:** Use vector graphics (SVG) instead of raster images
- Ensure image resolution is sufficient
- Test on actual devices (not just emulator)
- Use appropriate HeightRequest/WidthRequest values
