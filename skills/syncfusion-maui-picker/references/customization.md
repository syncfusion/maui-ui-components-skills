# Customization and Styling

Learn how to customize the visual appearance of the Syncfusion .NET MAUI Picker control through styles, templates, and visual properties.

## Table of Contents
- [Item Text Styling](#item-text-styling)
- [Background Customization](#background-customization)
- [Selection View Styling](#selection-view-styling)
- [Item Templates](#item-templates)
- [Complete Examples](#complete-examples)

## Item Text Styling

### Selected Item Style

Customize the appearance of the currently selected item using `SelectedTextStyle`.

**XAML:**
```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.SelectedTextStyle>
        <picker:PickerTextStyle FontSize="18" 
                               FontAttributes="Bold" 
                               TextColor="DarkBlue"
                               FontFamily="Arial"/>
    </picker:SfPicker.SelectedTextStyle>
</picker:SfPicker>
```

**C#:**
```csharp
picker.SelectedTextStyle = new PickerTextStyle()
{
    FontSize = 18,
    FontAttributes = FontAttributes.Bold,
    TextColor = Colors.DarkBlue,
    FontFamily = "Arial"
};
```

### Unselected Item Style

Customize the appearance of non-selected items using `TextStyle`.

**XAML:**
```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.TextStyle>
        <picker:PickerTextStyle FontSize="14" 
                               FontAttributes="None" 
                               TextColor="Gray"
                               FontFamily="Arial"/>
    </picker:SfPicker.TextStyle>
</picker:SfPicker>
```

**C#:**
```csharp
picker.TextStyle = new PickerTextStyle()
{
    FontSize = 14,
    FontAttributes = FontAttributes.None,
    TextColor = Colors.Gray,
    FontFamily = "Arial"
};
```

### Available Font Attributes

- **None**: Normal text
- **Bold**: Bold text
- **Italic**: Italic text
- **Bold | Italic**: Bold and italic combined

## Background Colors

### Picker Background

```xml
<picker:SfPicker Background="LightBlue">
    <!-- Picker configuration -->
</picker:SfPicker>
```

```csharp
picker.Background = Colors.LightBlue;
```

### Component Backgrounds

**Header Background:**
```csharp
picker.HeaderView.Background = Color.FromArgb("#6750A4");
```

**Footer Background:**
```csharp
picker.FooterView.Background = Color.FromArgb("#E8DEF8");
```

**Column Header Background:**
```csharp
picker.ColumnHeaderView.Background = Color.FromArgb("#F0F0F0");
```

## Selection View Customization

Customize the selection indicator appearance.

**Complete Example:**
```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.SelectionView>
        <picker:PickerSelectionView CornerRadius="12" 
                                    Stroke="#6750A4"
                                    Padding="10,5,10,5" 
                                    Background="#E8DEF8" />
    </picker:SfPicker.SelectionView>
</picker:SfPicker>
```

**Properties:**
- **CornerRadius**: Rounded corners (0 = square)
- **Stroke**: Border color
- **Background**: Fill color
- **Padding**: Internal spacing

## Custom Item Templates

Create fully custom item layouts using `DataTemplate`.

### Basic Custom Template

```xml
<Grid>
    <Grid.Resources>
        <DataTemplate x:Key="customItemTemplate">
            <Grid Padding="5">
                <Label HorizontalTextAlignment="Center" 
                       VerticalTextAlignment="Center" 
                       TextColor="DarkGreen"
                       FontSize="16"
                       FontAttributes="Bold"
                       Text="{Binding Data}"/>    
            </Grid>
        </DataTemplate>
    </Grid.Resources>
    
    <picker:SfPicker x:Name="picker" 
                     ItemTemplate="{StaticResource customItemTemplate}">
        <!-- Picker configuration -->
    </picker:SfPicker>
</Grid>
```

### Template with Icons

```xml
<DataTemplate x:Key="iconItemTemplate">
    <Grid Padding="5,2">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="30"/>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>
        
        <Image Grid.Column="0" 
               Source="{Binding Icon}" 
               HeightRequest="24" 
               WidthRequest="24"
               VerticalOptions="Center"/>
        
        <Label Grid.Column="1" 
               Text="{Binding Name}" 
               VerticalOptions="Center"
               FontSize="14"
               TextColor="Black"
               Margin="10,0,0,0"/>
    </Grid>
</DataTemplate>
```

### Template with Color Indicators

```xml
<DataTemplate x:Key="colorItemTemplate">
    <Grid Padding="5">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="20"/>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>
        
        <BoxView Grid.Column="0" 
                 Color="{Binding ColorValue}" 
                 HeightRequest="16" 
                 WidthRequest="16"
                 CornerRadius="8"
                 VerticalOptions="Center"/>
        
        <Label Grid.Column="1" 
               Text="{Binding ColorName}" 
               VerticalOptions="Center"
               FontSize="14"
               Margin="8,0,0,0"/>
    </Grid>
</DataTemplate>
```

## DataTemplateSelector

Apply different templates based on data conditions.

**Template Selector Class:**
```csharp
public class PickerTemplate : DataTemplateSelector
{
    public DataTemplate IndianLanguages { get; set; }
    public DataTemplate OtherLanguages { get; set; }
    
    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        PickerItemDetails pickerItemDetails = item as PickerItemDetails;
        string language = pickerItemDetails.Data.ToString();
        
        if (language == "Tamil" || language == "Telugu")
        {
            return this.IndianLanguages;
        }
        else
        {
            return this.OtherLanguages;
        }
    }
}
```

**XAML Usage:**
```xml
<Grid>
    <Grid.Resources>
        <DataTemplate x:Key="indianLanguage">
            <Grid>
                <Label HorizontalTextAlignment="Center" 
                       BackgroundColor="#808080" 
                       VerticalTextAlignment="Center" 
                       Text="{Binding Data}"/>
            </Grid>
        </DataTemplate>
        
        <DataTemplate x:Key="otherLanguage">
            <Grid>
                <Label HorizontalTextAlignment="Center" 
                       VerticalTextAlignment="Center" 
                       BackgroundColor="#D3D3D3" 
                       Text="{Binding Data}"/>
            </Grid>
        </DataTemplate>
        
        <local:PickerTemplate x:Key="pickerTemplate"
                              IndianLanguages="{StaticResource indianLanguage}" 
                              OtherLanguages="{StaticResource otherLanguage}"/>
    </Grid.Resources>
    
    <picker:SfPicker x:Name="picker" 
                     ItemTemplate="{StaticResource pickerTemplate}">
    </picker:SfPicker>
</Grid>
```

## Column Divider Customization

Style the divider between columns in multi-column pickers.

```xml
<picker:SfPicker ColumnDividerColor="#6750A4">
    <picker:SfPicker.Columns>
        <picker:PickerColumn ItemsSource="{Binding Column1Data}" />
        <picker:PickerColumn ItemsSource="{Binding Column2Data}" />
    </picker:SfPicker.Columns>
</picker:SfPicker>
```

```csharp
picker.ColumnDividerColor = Color.FromArgb("#6750A4");
```

## Complete Themed Example

Here's a complete example with consistent theming:

```xml
<picker:SfPicker x:Name="picker"
                 Background="White"
                 ColumnDividerColor="#E0E0E0"
                 HeightRequest="350"
                 WidthRequest="320">
    
    <!-- Header -->
    <picker:SfPicker.HeaderView>
        <picker:PickerHeaderView Text="Select Option" 
                                 Height="50"
                                 Background="#6750A4"
                                 DividerColor="#4A3A84">
            <picker:PickerHeaderView.TextStyle>
                <picker:PickerTextStyle TextColor="White" 
                                       FontSize="18" 
                                       FontAttributes="Bold"/>
            </picker:PickerHeaderView.TextStyle>
        </picker:PickerHeaderView>
    </picker:SfPicker.HeaderView>
    
    <!-- Column Header -->
    <picker:SfPicker.ColumnHeaderView>
        <picker:PickerColumnHeaderView Height="40" 
                                      Background="#E8DEF8"
                                      DividerColor="#D0C6E8">
            <picker:PickerColumnHeaderView.TextStyle>
                <picker:PickerTextStyle TextColor="#6750A4" 
                                       FontSize="14" 
                                       FontAttributes="Bold"/>
            </picker:PickerColumnHeaderView.TextStyle>
        </picker:PickerColumnHeaderView>
    </picker:SfPicker.ColumnHeaderView>
    
    <!-- Item Styles -->
    <picker:SfPicker.SelectedTextStyle>
        <picker:PickerTextStyle FontSize="18" 
                               FontAttributes="Bold" 
                               TextColor="#6750A4"/>
    </picker:SfPicker.SelectedTextStyle>
    
    <picker:SfPicker.TextStyle>
        <picker:PickerTextStyle FontSize="14" 
                               TextColor="#757575"/>
    </picker:SfPicker.TextStyle>
    
    <!-- Selection View -->
    <picker:SfPicker.SelectionView>
        <picker:PickerSelectionView CornerRadius="8" 
                                    Background="#F5F1FE"
                                    Stroke="#6750A4"
                                    Padding="10,5"/>
    </picker:SfPicker.SelectionView>
    
    <!-- Columns -->
    <picker:SfPicker.Columns>
        <picker:PickerColumn HeaderText="Options" 
                            ItemsSource="{Binding Options}" />
    </picker:SfPicker.Columns>
    
    <!-- Footer -->
    <picker:SfPicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True"
                                 Height="50"
                                 Background="#E8DEF8"
                                 DividerColor="#D0C6E8"
                                 OkButtonText="Confirm"
                                 CancelButtonText="Cancel">
            <picker:PickerFooterView.TextStyle>
                <picker:PickerTextStyle TextColor="#6750A4" 
                                       FontSize="16" 
                                       FontAttributes="Bold"/>
            </picker:PickerFooterView.TextStyle>
        </picker:PickerFooterView>
    </picker:SfPicker.FooterView>
    
</picker:SfPicker>
```

## Dark Theme Example

```xml
<picker:SfPicker Background="#1E1E1E"
                 ColumnDividerColor="#3A3A3A">
    
    <picker:SfPicker.HeaderView>
        <picker:PickerHeaderView Background="#2D2D30"
                                 DividerColor="#3F3F46">
            <picker:PickerHeaderView.TextStyle>
                <picker:PickerTextStyle TextColor="White" FontSize="18"/>
            </picker:PickerHeaderView.TextStyle>
        </picker:PickerHeaderView>
    </picker:SfPicker.HeaderView>
    
    <picker:SfPicker.SelectedTextStyle>
        <picker:PickerTextStyle TextColor="White" 
                               FontSize="18" 
                               FontAttributes="Bold"/>
    </picker:SfPicker.SelectedTextStyle>
    
    <picker:SfPicker.TextStyle>
        <picker:PickerTextStyle TextColor="#AAAAAA" FontSize="14"/>
    </picker:SfPicker.TextStyle>
    
    <picker:SfPicker.SelectionView>
        <picker:PickerSelectionView Background="#3A3A3A"
                                    Stroke="#007ACC"
                                    CornerRadius="8"/>
    </picker:SfPicker.SelectionView>
    
    <picker:SfPicker.FooterView>
        <picker:PickerFooterView Background="#2D2D30"
                                 ShowOkButton="True">
            <picker:PickerFooterView.TextStyle>
                <picker:PickerTextStyle TextColor="#007ACC" FontSize="16"/>
            </picker:PickerFooterView.TextStyle>
        </picker:PickerFooterView>
    </picker:SfPicker.FooterView>
    
</picker:SfPicker>
```

## Best Practices

1. **Maintain consistency:** Use the same color scheme across all components
2. **Ensure readability:** Maintain sufficient contrast between text and backgrounds
3. **Test on different devices:** Colors may appear differently on various screens
4. **Use semantic naming:** Create reusable styles with meaningful names
5. **Consider accessibility:** Ensure color choices meet WCAG guidelines
6. **Optimize templates:** Keep custom templates lightweight for performance

## Common Customization Patterns

### Material Design Style

```csharp
picker.HeaderView.Background = Color.FromArgb("#6200EE");
picker.HeaderView.TextStyle.TextColor = Colors.White;
picker.SelectionView.Background = Color.FromArgb("#BB86FC");
picker.SelectionView.CornerRadius = 4;
picker.FooterView.TextStyle.TextColor = Color.FromArgb("#6200EE");
```

### iOS Style

```csharp
picker.HeaderView.Height = 0; // No header
picker.ColumnHeaderView.Height = 0; // No column header
picker.SelectionView.Background = Colors.Transparent;
picker.SelectionView.Stroke = Colors.LightGray;
picker.SelectedTextStyle.FontSize = 20;
picker.TextStyle.TextColor = Colors.Gray;
```

### Minimal Style

```csharp
picker.Background = Colors.White;
picker.HeaderView.Height = 0;
picker.ColumnHeaderView.Height = 0;
picker.FooterView.Height = 0;
picker.SelectionView.Background = Color.FromArgb("#F5F5F5");
picker.SelectionView.Stroke = Colors.Transparent;
picker.SelectionView.CornerRadius = 8;
```

## Troubleshooting

### Issue: Custom colors not appearing

**Solution:**
- Verify color format is correct (#RRGGBB or #AARRGGBB)
- Check that background is not transparent when setting colors
- Ensure styles are applied after picker initialization

### Issue: Custom template not rendering

**Solution:**
- Verify DataTemplate syntax is correct
- Check binding paths match your data model
- Ensure ItemTemplate is set before adding columns
- Test with a simple template first

### Issue: Text not readable

**Solution:**
- Increase contrast between text and background
- Adjust font size
- Check selection view background doesn't obscure text
- Test on actual devices, not just simulator
