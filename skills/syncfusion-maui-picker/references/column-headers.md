# Column Header Customization

The Syncfusion .NET MAUI Picker allows you to add and customize column headers that provide descriptive labels for each column in multi-column pickers.

## Table of Contents
- [Enable or Disable Column Header](#enable-or-disable-column-header)
- [Set Column Header Text](#set-column-header-text)
- [Customize Column Header Style](#customize-column-header-style)
- [Column Header Templates](#column-header-templates)
- [Advanced Examples](#advanced-examples)

## Enable or Disable Column Header

Enable the column header view by setting the `Height` property to a value greater than 0.

**Default value:** `0` (disabled)

**XAML:**
```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.ColumnHeaderView>
        <picker:PickerColumnHeaderView Height="40"/>
    </picker:SfPicker.ColumnHeaderView>
</picker:SfPicker>
```

**C#:**
```csharp
picker.ColumnHeaderView.Height = 40;
```

## Setting Column Header Text

Add descriptive text to individual columns using the `HeaderText` property on each `PickerColumn`.

**XAML:**
```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.HeaderView>
        <picker:PickerHeaderView Text="Select a color" Height="40" />
    </picker:SfPicker.HeaderView>

    <picker:SfPicker.Columns>
        <picker:PickerColumn HeaderText="Colors" 
                            ItemsSource="{Binding DataSource}" />
    </picker:SfPicker.Columns>

    <picker:SfPicker.ColumnHeaderView>
        <picker:PickerColumnHeaderView Height="40" />
    </picker:SfPicker.ColumnHeaderView>
</picker:SfPicker>
```

**C#:**
```csharp
ItemInfo itemInfo = new ItemInfo();

SfPicker picker = new SfPicker()
{
    HeaderView = new PickerHeaderView()
    {
        Text = "Select a color",
        Height = 40,
    },

    Columns = new ObservableCollection<PickerColumn>()
    {
        new PickerColumn()
        {
            HeaderText = "Colors",
            ItemsSource = itemInfo.DataSource,
        }
    },

    ColumnHeaderView = new PickerColumnHeaderView()
    {
        Height = 40,
    },
};

this.Content = picker;
```

## Column Header Customization

### Background Color

Customize the background color of the column header view.

**XAML:**
```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.ColumnHeaderView>
        <picker:PickerColumnHeaderView Background="#E5E4E2" Height="40"/>
    </picker:SfPicker.ColumnHeaderView>
</picker:SfPicker>
```

**C#:**
```csharp
picker.ColumnHeaderView.Background = Color.FromArgb("#E5E4E2");
```

### Text Style

Customize the text style including color, font size, font family, and font attributes.

**XAML:**
```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.ColumnHeaderView>
        <picker:PickerColumnHeaderView Height="40">
            <picker:PickerColumnHeaderView.TextStyle>
                <picker:PickerTextStyle TextColor="Gray" 
                                       FontSize="18" 
                                       FontAttributes="Italic"/>
            </picker:PickerColumnHeaderView.TextStyle>
        </picker:PickerColumnHeaderView>
    </picker:SfPicker.ColumnHeaderView>
</picker:SfPicker>
```

**C#:**
```csharp
picker.ColumnHeaderView.TextStyle = new PickerTextStyle()
{
    TextColor = Colors.Gray,
    FontSize = 18,
    FontAttributes = FontAttributes.Italic
};
```

### Divider Color

Customize the divider line color below the column headers.

**XAML:**
```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.ColumnHeaderView>
        <picker:PickerColumnHeaderView DividerColor="Red" Height="40"/>
    </picker:SfPicker.ColumnHeaderView>
</picker:SfPicker>
```

**C#:**
```csharp
picker.ColumnHeaderView.DividerColor = Colors.Red;
```

## Multi-Column Headers

When using multiple columns, each column can have its own header text.

**Example:**
```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.Columns>
        <picker:PickerColumn HeaderText="Country" 
                            ItemsSource="{Binding Countries}" />
        <picker:PickerColumn HeaderText="State" 
                            ItemsSource="{Binding States}" />
        <picker:PickerColumn HeaderText="City" 
                            ItemsSource="{Binding Cities}" />
    </picker:SfPicker.Columns>

    <picker:SfPicker.ColumnHeaderView>
        <picker:PickerColumnHeaderView Height="40" 
                                      Background="#F0F0F0"/>
    </picker:SfPicker.ColumnHeaderView>
</picker:SfPicker>
```

**C#:**
```csharp
picker.Columns.Add(new PickerColumn 
{ 
    HeaderText = "Country", 
    ItemsSource = countries 
});

picker.Columns.Add(new PickerColumn 
{ 
    HeaderText = "State", 
    ItemsSource = states 
});

picker.Columns.Add(new PickerColumn 
{ 
    HeaderText = "City", 
    ItemsSource = cities 
});

picker.ColumnHeaderView = new PickerColumnHeaderView()
{
    Height = 40,
    Background = Color.FromArgb("#F0F0F0")
};
```

## Custom Column Header Template

Create a fully custom column header appearance using `DataTemplate`.

**XAML:**
```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.ColumnHeaderTemplate>
        <DataTemplate>
            <Grid BackgroundColor="#BB9AB1">
                <Label Text="Colors" 
                       TextColor="White" 
                       HorizontalTextAlignment="Center" 
                       VerticalTextAlignment="Center"/>
            </Grid>
        </DataTemplate>
    </picker:SfPicker.ColumnHeaderTemplate>
</picker:SfPicker>
```

**Important Note:** When using `ColumnHeaderTemplate`, the remaining column header properties (except `DividerColor`) will not have any effect.

## Custom Column Header with DataTemplateSelector

Apply different templates based on conditions using `DataTemplateSelector`.

**XAML:**
```xml
<Grid.Resources>
    <DataTemplate x:Key="selectedItemTemplate">
        <Grid Background="LightBlue">
            <Label Text="Colors"  
                   HorizontalOptions="Center" 
                   VerticalOptions="Center" 
                   TextColor="Red"/>
        </Grid>
    </DataTemplate>
    
    <DataTemplate x:Key="nonSelectedItemTemplate">
        <Grid Background="LightGreen">
            <Label Text="Colors"  
                   HorizontalOptions="Center" 
                   VerticalOptions="Center" 
                   TextColor="Orange"/>
        </Grid>
    </DataTemplate>
    
    <local:PickerTemplateSelector x:Key="columnHeaderTemplateSelector" 
                                  SelectedItemTemplate="{StaticResource selectedItemTemplate}"  
                                  NonSelectedItemTemplate="{StaticResource nonSelectedItemTemplate}"/>
</Grid.Resources>

<picker:SfPicker x:Name="picker" 
                 ColumnHeaderTemplate="{StaticResource columnHeaderTemplateSelector}">
</picker:SfPicker>
```

**C# Template Selector:**
```csharp
public class PickerTemplateSelector : DataTemplateSelector
{
    public DataTemplate SelectedItemTemplate { get; set; }
    public DataTemplate NonSelectedItemTemplate { get; set; }
    
    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {  
        var details = item as PickerColumn;
        if (details != null && details.SelectedIndex <= 4)
            return SelectedItemTemplate;
        
        return NonSelectedItemTemplate;
    }
}
```

## Complete Example

Here's a complete example combining all column header customization options:

```xml
<picker:SfPicker x:Name="picker"
                 HeightRequest="350"
                 WidthRequest="320">
    
    <!-- Header -->
    <picker:SfPicker.HeaderView>
        <picker:PickerHeaderView Text="Select Location" Height="40" />
    </picker:SfPicker.HeaderView>
    
    <!-- Columns with Header Text -->
    <picker:SfPicker.Columns>
        <picker:PickerColumn HeaderText="Country" 
                            ItemsSource="{Binding Countries}" 
                            SelectedIndex="0"/>
        <picker:PickerColumn HeaderText="City" 
                            ItemsSource="{Binding Cities}" 
                            SelectedIndex="0"/>
    </picker:SfPicker.Columns>
    
    <!-- Column Header View -->
    <picker:SfPicker.ColumnHeaderView>
        <picker:PickerColumnHeaderView Height="45" 
                                      Background="#E8DEF8"
                                      DividerColor="#6750A4">
            <picker:PickerColumnHeaderView.TextStyle>
                <picker:PickerTextStyle TextColor="#6750A4" 
                                       FontSize="14" 
                                       FontAttributes="Bold"/>
            </picker:PickerColumnHeaderView.TextStyle>
        </picker:PickerColumnHeaderView>
    </picker:SfPicker.ColumnHeaderView>
    
    <!-- Footer -->
    <picker:SfPicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True" Height="40"/>
    </picker:SfPicker.FooterView>
    
</picker:SfPicker>
```

## Best Practices

1. **Always set meaningful header text** for each column to help users understand the data
2. **Keep header text concise** - avoid long labels that may overflow
3. **Use consistent styling** across all columns for a professional appearance
4. **Ensure readability** with appropriate text color and background contrast
5. **Enable column headers for multi-column pickers** to improve usability
6. **Test on different screen sizes** to ensure headers fit properly

## Troubleshooting

### Issue: Column headers not visible

**Solution:**
- Verify `ColumnHeaderView.Height` is set to a value greater than 0
- Check that `HeaderText` is set on individual `PickerColumn` objects
- Ensure the column header view is properly initialized

### Issue: Header text cut off

**Solution:**
- Increase `ColumnHeaderView.Height`
- Reduce `FontSize` in `TextStyle`
- Use shorter header text
- Adjust column widths

### Issue: Custom template not showing

**Solution:**
- Verify `ColumnHeaderTemplate` is properly bound
- Check that the `DataTemplate` contains valid XAML
- Ensure template selector logic is correct
- Remember that other column header properties (except `DividerColor`) won't work with custom templates
