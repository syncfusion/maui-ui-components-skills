# Display View Customization

The display view is the compact button shown when the color picker popup is closed. It displays the currently selected color and provides a dropdown button to open the popup. This guide covers customizing the display view appearance, including icons, templates, sizing, and styling.

## Table of Contents
- [Selected Color Icon](#selected-color-icon)
- [Selected Color Template](#selected-color-template)
- [Dropdown Button Template](#dropdown-button-template)
- [Display View Sizing](#display-view-sizing)
- [Display View Styling](#display-view-styling)

## Selected Color Icon

Customize the icon shown in the selected color area using Font Icons or Image Sources.

### Basic Icon Customization

```xaml
<inputs:SfColorPicker x:Name="colorPicker">
    <inputs:SfColorPicker.SelectedColorIcon>
        <FontImageSource FontFamily="MauiMaterialAssets" 
                        Glyph="&#xe760;" 
                        Color="{Binding Source={x:Reference colorPicker}, Path=SelectedColor}"/>
    </inputs:SfColorPicker.SelectedColorIcon>
</inputs:SfColorPicker>
```

### Dynamic Icon Color

Update icon color when selected color changes:

```csharp
using Syncfusion.Maui.Inputs;

public partial class MainPage : ContentPage
{
    private FontImageSource fontIcon;
    
    public MainPage()
    {
        InitializeComponent();
        
        SfColorPicker colorPicker = new SfColorPicker();
        
        // Create font icon
        fontIcon = new FontImageSource
        {
            FontFamily = "MauiMaterialAssets",
            Glyph = "\ue760",
            Color = colorPicker.SelectedColor
        };
        
        colorPicker.SelectedColorIcon = fontIcon;
        
        // Update icon color when color changes
        colorPicker.ColorChanged += (s, e) =>
        {
            fontIcon.Color = e.NewColor;
        };
        
        Content = colorPicker;
    }
}
```

### Using Image Files

```xaml
<inputs:SfColorPicker>
    <inputs:SfColorPicker.SelectedColorIcon>
        <FontImageSource FontFamily="MaterialIcons" Glyph="&#xE3B8;" Color="Black" Size="20" />
    </inputs:SfColorPicker.SelectedColorIcon>
</inputs:SfColorPicker>
```

```csharp
colorPicker.SelectedColorIcon = new FontImageSource
{
    FontFamily = "MaterialIcons",
    Glyph = "\ue3b8", // palette icon
    Color = Colors.Black,
    Size = 20
};
```

## Selected Color Icon Size

Control the size of the selected color icon:

```xaml
<inputs:SfColorPicker SelectedColorIconSize="32"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    SelectedColorIconSize = 32
};
```

**Recommended Sizes:**
- **Small:** 16-24 (compact UI)
- **Medium:** 28-32 (default, balanced)
- **Large:** 36-48 (prominent display)

## Selected Color Template

Create a fully custom display for the selected color using DataTemplate.

### Basic Custom Template

```xaml
<inputs:SfColorPicker x:Name="colorPicker">
    <inputs:SfColorPicker.SelectedColorTemplate>
        <DataTemplate>
            <VerticalStackLayout WidthRequest="70" Background="White">
                <Label Text="A" 
                       HorizontalOptions="Center" 
                       TextColor="Black"/>
                <BoxView HeightRequest="5" 
                         WidthRequest="30" 
                         Background="{Binding Source={x:Reference colorPicker}, Path=SelectedColor}"/>
            </VerticalStackLayout>
        </DataTemplate>
    </inputs:SfColorPicker.SelectedColorTemplate>
</inputs:SfColorPicker>
```

### Color Swatch Template

```xaml
<inputs:SfColorPicker x:Name="colorPicker">
    <inputs:SfColorPicker.SelectedColorTemplate>
        <DataTemplate>
            <Border BackgroundColor="White"
                    StrokeThickness="2"
                    Stroke="Gray"
                    Padding="8">
                <Grid ColumnDefinitions="40,*" ColumnSpacing="10">
                    <!-- Color swatch -->
                    <BoxView Grid.Column="0"
                             WidthRequest="40"
                             HeightRequest="40"
                             CornerRadius="5"
                             BackgroundColor="{Binding Source={x:Reference colorPicker}, Path=SelectedColor}"/>
                    
                    <!-- Color info -->
                    <VerticalStackLayout Grid.Column="1" VerticalOptions="Center">
                        <Label Text="Selected Color" FontSize="10" TextColor="Gray"/>
                        <Label Text="{Binding Source={x:Reference colorPicker}, Path=SelectedColor}" 
                               FontSize="12" 
                               TextColor="Black"
                               FontAttributes="Bold"/>
                    </VerticalStackLayout>
                </Grid>
            </Border>
        </DataTemplate>
    </inputs:SfColorPicker.SelectedColorTemplate>
</inputs:SfColorPicker>
```

### Code-Behind Template

```csharp
colorPicker.SelectedColorTemplate = new DataTemplate(() =>
{
    // Create label
    var label = new Label
    {
        Text = "A",
        HorizontalOptions = LayoutOptions.Center,
        TextColor = Colors.Black
    };

    // Create box view with binding
    var boxView = new BoxView
    {
        HeightRequest = 5,
        WidthRequest = 30
    };
    
    boxView.SetBinding(BoxView.BackgroundProperty, new Binding
    {
        Source = colorPicker,
        Path = "SelectedColor",
        Mode = BindingMode.OneWay
    });

    // Create layout
    var stackLayout = new VerticalStackLayout
    {
        WidthRequest = 70,
        BackgroundColor = Colors.White,
        Children = { label, boxView }
    };

    return stackLayout;
});
```

### Circular Swatch Template

```xaml
<inputs:SfColorPicker x:Name="colorPicker">
    <inputs:SfColorPicker.SelectedColorTemplate>
        <DataTemplate>
            <Grid WidthRequest="60" HeightRequest="60">
                <!-- Outer circle (border) -->
                <BoxView WidthRequest="60"
                         HeightRequest="60"
                         CornerRadius="30"
                         BackgroundColor="DarkGray"/>
                
                <!-- Inner circle (color) -->
                <BoxView WidthRequest="50"
                         HeightRequest="50"
                         CornerRadius="25"
                         HorizontalOptions="Center"
                         VerticalOptions="Center"
                         BackgroundColor="{Binding Source={x:Reference colorPicker}, Path=SelectedColor}"/>
            </Grid>
        </DataTemplate>
    </inputs:SfColorPicker.SelectedColorTemplate>
</inputs:SfColorPicker>
```

## Dropdown Button Template

Customize the dropdown button (arrow icon) that opens the color picker popup.

### Custom Dropdown Icon

```xaml
<inputs:SfColorPicker>
    <inputs:SfColorPicker.DropDownButtonTemplate>
        <DataTemplate>
            <Label Text="&#xe705;" 
                   FontFamily="MauiMaterialAssets" 
                   FontSize="14" 
                   TextColor="Black" 
                   VerticalTextAlignment="Center" 
                   HorizontalTextAlignment="Center"/>
        </DataTemplate>
    </inputs:SfColorPicker.DropDownButtonTemplate>
</inputs:SfColorPicker>
```

### Code-Behind Dropdown Template

```csharp
var dropDownTemplate = new DataTemplate(() =>
{
    return new Label
    {
        Text = "\ue705",  // Material icon glyph
        FontFamily = "MauiMaterialAssets",
        TextColor = Colors.Black,
        FontSize = 14,
        VerticalTextAlignment = TextAlignment.Center,
        HorizontalTextAlignment = TextAlignment.Center
    };
});

colorPicker.DropDownButtonTemplate = dropDownTemplate;
```

### Image-Based Dropdown Button

```xaml
<inputs:SfColorPicker>
    <inputs:SfColorPicker.DropDownButtonTemplate>
        <DataTemplate>
            <Image Source="dropdown_arrow.png"
                   WidthRequest="16"
                   HeightRequest="16"
                   Aspect="AspectFit"/>
        </DataTemplate>
    </inputs:SfColorPicker.DropDownButtonTemplate>
</inputs:SfColorPicker>
```

### Custom Styled Button

```xaml
<inputs:SfColorPicker>
    <inputs:SfColorPicker.DropDownButtonTemplate>
        <DataTemplate>
            <Border BackgroundColor="LightGray"
                    StrokeThickness="1"
                    Stroke="Gray"
                    Padding="5">
                <Label Text="▼" 
                       FontSize="12" 
                       TextColor="DarkGray"
                       HorizontalTextAlignment="Center"
                       VerticalTextAlignment="Center"/>
            </Border>
        </DataTemplate>
    </inputs:SfColorPicker.DropDownButtonTemplate>
</inputs:SfColorPicker>
```

## Display View Sizing

### Display View Height

Set the height of the entire display view:

```xaml
<inputs:SfColorPicker DisplayViewHeight="48"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    DisplayViewHeight = 48
};
```

**Recommended Heights:**
- **Compact:** 36-40
- **Standard:** 44-48 (default)
- **Large:** 52-60

### Dropdown Width

Set the width of the dropdown button area:

```xaml
<inputs:SfColorPicker DropDownWidth="48"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    DropDownWidth = 48
};
```

**Balance:** Ensure dropdown width complements display view height for visual harmony.

## Display View Styling

### Display View Stroke (Border Color)

```xaml
<inputs:SfColorPicker DisplayViewStroke="Red"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    DisplayViewStroke = Colors.Red
};
```

### Display View Stroke Thickness

```xaml
<inputs:SfColorPicker DisplayViewStrokeThickness="3"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    DisplayViewStrokeThickness = 3
};
```

### Complete Styling Example

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    // Sizing
    DisplayViewHeight = 50,
    DropDownWidth = 50,
    
    // Border styling
    DisplayViewStroke = Colors.DarkGray,
    DisplayViewStrokeThickness = 2,
    
    // Icon
    SelectedColorIconSize = 28
};
```

## Complete Examples

### Example 1: Minimal Design

```xaml
<inputs:SfColorPicker x:Name="colorPicker"
                      DisplayViewHeight="40"
                      DropDownWidth="40"
                      SelectedColorIconSize="24"
                      DisplayViewStroke="LightGray"
                      DisplayViewStrokeThickness="1"/>
```

### Example 2: Text-Based Display

```xaml
<inputs:SfColorPicker x:Name="colorPicker">
    <inputs:SfColorPicker.SelectedColorTemplate>
        <DataTemplate>
            <Grid ColumnDefinitions="*,Auto" Padding="10,0">
                <!-- HEX value display -->
                <Label Grid.Column="0"
                       Text="{Binding Source={x:Reference colorPicker}, Path=SelectedColor}"
                       VerticalOptions="Center"
                       FontFamily="Monospace"
                       FontSize="14"/>
                
                <!-- Small color preview -->
                <BoxView Grid.Column="1"
                         WidthRequest="24"
                         HeightRequest="24"
                         CornerRadius="4"
                         Margin="10,0,0,0"
                         BackgroundColor="{Binding Source={x:Reference colorPicker}, Path=SelectedColor}"/>
            </Grid>
        </DataTemplate>
    </inputs:SfColorPicker.SelectedColorTemplate>
    
    <inputs:SfColorPicker.DropDownButtonTemplate>
        <DataTemplate>
            <Label Text="⯆" FontSize="20" TextColor="Gray"/>
        </DataTemplate>
    </inputs:SfColorPicker.DropDownButtonTemplate>
</inputs:SfColorPicker>
```

### Example 3: Material Design Style

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    DisplayViewHeight = 56,
    DropDownWidth = 56,
    DisplayViewStroke = Color.FromArgb("#BDBDBD"),
    DisplayViewStrokeThickness = 1,
    SelectedColorIconSize = 32
};

// Custom template
colorPicker.SelectedColorTemplate = new DataTemplate(() =>
{
    var border = new Border
    {
        StrokeThickness = 0,
        BackgroundColor = Colors.White,
        Padding = new Thickness(12)
    };
    
    var boxView = new BoxView
    {
        WidthRequest = 32,
        HeightRequest = 32,
        CornerRadius = 16  // Circular
    };
    
    boxView.SetBinding(BoxView.BackgroundProperty, new Binding
    {
        Source = colorPicker,
        Path = "SelectedColor"
    });
    
    border.Content = boxView;
    return border;
});
```

### Example 4: Color Name Display

```xaml
<inputs:SfColorPicker x:Name="colorPicker"
                      SelectedColor="Red">
    <inputs:SfColorPicker.SelectedColorTemplate>
        <DataTemplate>
            <VerticalStackLayout Padding="10" Spacing="5">
                <!-- Color swatch -->
                <BoxView WidthRequest="40"
                         HeightRequest="40"
                         CornerRadius="5"
                         BackgroundColor="{Binding Source={x:Reference colorPicker}, Path=SelectedColor}"/>
                
                <!-- Color name (requires converter or code-behind) -->
                <Label Text="Red"
                       FontSize="10"
                       HorizontalOptions="Center"
                       TextColor="Gray"/>
            </VerticalStackLayout>
        </DataTemplate>
    </inputs:SfColorPicker.SelectedColorTemplate>
</inputs:SfColorPicker>
```

## Best Practices

1. **Icon Size:** Keep icons proportional to display view height (typically 60-70% of height)

2. **Templates:** Ensure templates bind to `SelectedColor` to update automatically

3. **Touch Targets:** Maintain minimum 44x44 for mobile accessibility

4. **Visual Hierarchy:** Use borders/shadows to distinguish display view from background

5. **Consistency:** Match display view styling with your app's design system

6. **Contrast:** Ensure dropdown button is visible against all selected colors

7. **Performance:** Keep templates simple to avoid UI lag during color changes

## Troubleshooting

### Issue: Custom icon not showing

**Solution:** Verify font family is registered in `MauiProgram.cs`:

```csharp
.ConfigureFonts(fonts =>
{
    fonts.AddFont("MaterialIcons-Regular.ttf", "MauiMaterialAssets");
})
```

### Issue: Template not updating on color change

**Solution:** Use proper binding with `Source` and `Path`:

```xaml
BackgroundColor="{Binding Source={x:Reference colorPicker}, Path=SelectedColor}"
```

### Issue: Display view too small on mobile

**Solution:** Increase sizing for better touch targets:

```csharp
colorPicker.DisplayViewHeight = 56;
colorPicker.DropDownWidth = 56;
```
