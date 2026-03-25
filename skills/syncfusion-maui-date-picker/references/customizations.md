# Customizations in .NET MAUI DatePicker

The .NET MAUI DatePicker provides extensive customization options for headers, column headers, footers, selection views, and individual date columns.

## Table of Contents
- [Header Customization](#header-customization)
- [Column Header Customization](#column-header-customization)
- [Footer Customization](#footer-customization)
- [Selection View Customization](#selection-view-customization)
- [Column Divider Color](#column-divider-color)
- [Close Button Customization](#close-button-customization)
- [Column Customization](#column-customization)

## Header Customization

Customize the date picker header using the `HeaderView` property.

### Set Header Text

```xml
<picker:SfDatePicker x:Name="datePicker">
    <picker:SfDatePicker.HeaderView>
        <picker:PickerHeaderView Text="Date Picker" Height="40" />
    </picker:SfDatePicker.HeaderView>
</picker:SfDatePicker>
```

```csharp
datePicker.HeaderView = new PickerHeaderView()
{
    Text = "Date Picker",
    Height = 40,
};
```

### Set Divider Color

```xml
<picker:SfDatePicker x:Name="datePicker">
    <picker:SfDatePicker.HeaderView>
        <picker:PickerHeaderView DividerColor="Red" Height="40" />
    </picker:SfDatePicker.HeaderView>
</picker:SfDatePicker>
```

### Customize Header Style

```xml
<picker:SfDatePicker x:Name="datePicker">
    <picker:SfDatePicker.HeaderView>
        <picker:PickerHeaderView Text="Select Date" 
                                 Height="50"
                                 Background="#6200EE">
            <picker:PickerHeaderView.TextStyle>
                <picker:PickerTextStyle FontSize="18" TextColor="White" FontAttributes="Bold" />
            </picker:PickerHeaderView.TextStyle>
        </picker:PickerHeaderView>
    </picker:SfDatePicker.HeaderView>
</picker:SfDatePicker>
```

### Custom Header Template

```xml
<picker:SfDatePicker x:Name="datePicker">
    <picker:SfDatePicker.HeaderTemplate>
        <DataTemplate>
            <Grid BackgroundColor="#BB9AB1" Padding="10">
                <Label HorizontalOptions="Center" 
                       VerticalOptions="Center" 
                       Text="Select a Date" 
                       TextColor="White"
                       FontSize="20"
                       FontAttributes="Bold"/>
            </Grid>
        </DataTemplate>
    </picker:SfDatePicker.HeaderTemplate>
</picker:SfDatePicker>
```

**Note:** When using HeaderTemplate, other header properties (except DividerColor) won't have effect.

### Header DataTemplateSelector

```xml
<Grid.Resources>
    <DataTemplate x:Key="todayDatesTemplate">
        <Grid Background="LightBlue" Padding="10">
            <Label HorizontalOptions="Center" 
                   VerticalOptions="Center" 
                   Text="Select a Date" 
                   TextColor="Red"
                   FontSize="18"/>
        </Grid>
    </DataTemplate>
    <DataTemplate x:Key="normalDatesTemplate">
        <Grid Background="LightGreen" Padding="10">
            <Label HorizontalOptions="Center" 
                   VerticalOptions="Center" 
                   Text="Select a Date" 
                   TextColor="Orange"
                   FontSize="18"/>
        </Grid>
    </DataTemplate>
    <local:DateTemplateSelector x:Key="headerTemplateSelector" 
                                TodayDatesTemplate="{StaticResource todayDatesTemplate}"  
                                NormaldatesTemplate="{StaticResource normalDatesTemplate}"/>
</Grid.Resources>

<picker:SfDatePicker x:Name="datePicker" 
                     HeaderTemplate="{StaticResource headerTemplateSelector}">
</picker:SfDatePicker>
```

```csharp
public class DateTemplateSelector : DataTemplateSelector
{
    public DataTemplate TodayDatesTemplate { get; set; }
    public DataTemplate NormaldatesTemplate { get; set; }
    
    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        var datePicker = item as SfDatePicker;
        if (datePicker?.SelectedDate.HasValue == true && 
            datePicker.SelectedDate.Value < DateTime.Now.Date)
        {
            return TodayDatesTemplate;
        }
        return NormaldatesTemplate;
    }
}
```

## Column Header Customization

Customize the column headers for Day, Month, and Year columns.

### Set Custom Column Headers

```xml
<picker:SfDatePicker x:Name="datePicker">
    <picker:SfDatePicker.ColumnHeaderView>
        <picker:DatePickerColumnHeaderView DayHeaderText="Day Column"
                                           MonthHeaderText="Month Column"
                                           YearHeaderText="Year Column"
                                           Height="40"/>
    </picker:SfDatePicker.ColumnHeaderView>
</picker:SfDatePicker>
```

```csharp
datePicker.ColumnHeaderView = new DatePickerColumnHeaderView()
{
    DayHeaderText = "Day Column",
    MonthHeaderText = "Month Column",
    YearHeaderText = "Year Column",
    Height = 40
};
```

### Set Column Header Divider Color

```xml
<picker:SfDatePicker x:Name="datePicker">
    <picker:SfDatePicker.ColumnHeaderView>
        <picker:DatePickerColumnHeaderView DividerColor="Red" Height="40" />
    </picker:SfDatePicker.ColumnHeaderView>
</picker:SfDatePicker>
```

### Customize Column Header Style

```xml
<picker:SfDatePicker x:Name="datePicker">
    <picker:SfDatePicker.ColumnHeaderView>
        <picker:DatePickerColumnHeaderView Background="#F5F5F5" Height="45">
            <picker:DatePickerColumnHeaderView.TextStyle>
                <picker:PickerTextStyle FontSize="14" 
                                        TextColor="#333333" 
                                        FontAttributes="Bold" />
            </picker:DatePickerColumnHeaderView.TextStyle>
        </picker:DatePickerColumnHeaderView>
    </picker:SfDatePicker.ColumnHeaderView>
</picker:SfDatePicker>
```

### Custom Column Header Template

```xml
<picker:SfDatePicker x:Name="datePicker">
    <picker:SfDatePicker.ColumnHeaderTemplate>
        <DataTemplate>
            <Grid BackgroundColor="#E8E8E8">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Label Text="Year" Grid.Column="0" TextColor="#6200EE" 
                       HorizontalTextAlignment="Center" VerticalTextAlignment="Center"
                       FontAttributes="Bold"/>
                <Label Text="Month" Grid.Column="1" TextColor="#6200EE"  
                       HorizontalTextAlignment="Center" VerticalTextAlignment="Center"
                       FontAttributes="Bold"/>
                <Label Text="Day" Grid.Column="2" TextColor="#6200EE" 
                       HorizontalTextAlignment="Center" VerticalTextAlignment="Center"
                       FontAttributes="Bold"/>
            </Grid>
        </DataTemplate>
    </picker:SfDatePicker.ColumnHeaderTemplate>
</picker:SfDatePicker>
```

**Note:** When using ColumnHeaderTemplate, other column header properties (except DividerColor) won't have effect.

## Footer Customization

Customize the footer with OK and Cancel buttons.

### Set Footer Buttons

```xml
<picker:SfDatePicker x:Name="datePicker">
    <picker:SfDatePicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True"
                                 Height="50"
                                 OkButtonText="Confirm"
                                 CancelButtonText="Cancel"/>
    </picker:SfDatePicker.FooterView>
</picker:SfDatePicker>
```

```csharp
datePicker.FooterView = new PickerFooterView()
{
    ShowOkButton = true,
    Height = 50,
    OkButtonText = "Confirm",
    CancelButtonText = "Cancel"
};
```

### Set Footer Divider Color

```xml
<picker:SfDatePicker x:Name="datePicker">
    <picker:SfDatePicker.FooterView>
        <picker:PickerFooterView DividerColor="Red" Height="40" />
    </picker:SfDatePicker.FooterView>
</picker:SfDatePicker>
```

### Customize Footer Style

```xml
<picker:SfDatePicker x:Name="datePicker">
    <picker:SfDatePicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True"
                                 Height="50"
                                 Background="#F0F0F0">
            <picker:PickerFooterView.TextStyle>
                <picker:PickerTextStyle FontSize="16" 
                                        TextColor="#6200EE"
                                        FontAttributes="Bold" />
            </picker:PickerFooterView.TextStyle>
        </picker:PickerFooterView>
    </picker:SfDatePicker.FooterView>
</picker:SfDatePicker>
```

### Custom Footer Template

```xml
<picker:SfDatePicker x:Name="datePicker">
    <picker:SfDatePicker.FooterTemplate>
        <DataTemplate>
            <Grid BackgroundColor="#E8E8E8" Padding="10">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Button Grid.Column="0" 
                        Text="Decline" 
                        TextColor="White" 
                        BackgroundColor="#D32F2F"
                        CornerRadius="8"
                        Margin="5"/>
                <Button Grid.Column="1" 
                        Text="Accept" 
                        TextColor="White" 
                        BackgroundColor="#388E3C"
                        CornerRadius="8"
                        Margin="5"/>
            </Grid>
        </DataTemplate>
    </picker:SfDatePicker.FooterTemplate>
</picker:SfDatePicker>
```

**Note:** When using FooterTemplate, other footer properties (except DividerColor) won't have effect.

## Selection View Customization

Customize the appearance of the selected date item.

### Set Selection View Style

```xml
<picker:SfDatePicker x:Name="datePicker">
    <picker:SfDatePicker.SelectionView>
        <picker:PickerSelectionView CornerRadius="10" 
                                    Stroke="#6200EE"
                                    Padding="10,5,10,5" 
                                    Background="#E3D7FF" />
    </picker:SfDatePicker.SelectionView>
</picker:SfDatePicker>
```

```csharp
datePicker.SelectionView = new PickerSelectionView()
{
    CornerRadius = 10,
    Stroke = Color.FromArgb("#6200EE"),
    StrokeThickness = 2,
    Padding = new Thickness(10, 5, 10, 5),
    Background = Color.FromArgb("#E3D7FF"),
};
```

### Customize Selected Text Style

```xml
<picker:SfDatePicker x:Name="datePicker">
    <picker:SfDatePicker.SelectedTextStyle>
        <picker:PickerTextStyle FontSize="18" 
                                TextColor="#6200EE"
                                FontAttributes="Bold"/>
    </picker:SfDatePicker.SelectedTextStyle>
</picker:SfDatePicker>
```

```csharp
datePicker.SelectedTextStyle = new PickerTextStyle()
{
    TextColor = Color.FromArgb("#6200EE"),
    FontSize = 18,
    FontAttributes = FontAttributes.Bold
};
```

## Column Divider Color

Customize the color of dividers between date columns.

```xml
<picker:SfDatePicker x:Name="datePicker"
                     ColumnDividerColor="#6200EE">
</picker:SfDatePicker>
```

```csharp
datePicker.ColumnDividerColor = Color.FromArgb("#6200EE");
```

## Close Button Customization

### Show Close Button

Enable the close button in the header (Dialog/RelativeDialog modes only).

```xml
<picker:SfDatePicker x:Name="datePicker" 
                     Mode="Dialog"
                     ShowCloseButton="True">
    <picker:SfDatePicker.HeaderView>
        <picker:PickerHeaderView Text="Date Picker" Height="40"/>
    </picker:SfDatePicker.HeaderView>
</picker:SfDatePicker>
```

**Note:** Header view must be present for the close button to render properly.

### Custom Close Button Icon

```xml
<picker:SfDatePicker x:Name="datePicker" 
                     Mode="Dialog"
                     ShowCloseButton="True"
                     CloseButtonIcon="closeicon.png">
    <picker:SfDatePicker.HeaderView>
        <picker:PickerHeaderView Text="Date Picker" Height="40"/>
    </picker:SfDatePicker.HeaderView>
</picker:SfDatePicker>
```

**Note:** `ShowCloseButton` must be `true` for the icon to display.

## Column Customization

Customize individual date columns (Day, Month, Year).

### Day Column Customization

```xml
<picker:SfDatePicker x:Name="datePicker"
                     DayColumnWidth="120">
    <picker:SfDatePicker.DayColumnTextStyle>
        <picker:PickerTextStyle FontSize="16" TextColor="#2F855A" FontAttributes="Bold"/>
    </picker:SfDatePicker.DayColumnTextStyle>
</picker:SfDatePicker>
```

```csharp
datePicker.DayColumnWidth = 120;
datePicker.DayColumnTextStyle = new PickerTextStyle()
{
    TextColor = Color.FromArgb("#2F855A"),
    FontSize = 16,
    FontAttributes = FontAttributes.Bold
};
```

**Note:** Applying `DayColumnTextStyle` overrides the unselected `TextStyle` for the day column.

### Month Column Customization

```xml
<picker:SfDatePicker x:Name="datePicker"
                     MonthColumnWidth="150">
    <picker:SfDatePicker.MonthColumnTextStyle>
        <picker:PickerTextStyle FontSize="16" TextColor="#D53F8C" FontAttributes="Bold"/>
    </picker:SfDatePicker.MonthColumnTextStyle>
</picker:SfDatePicker>
```

```csharp
datePicker.MonthColumnWidth = 150;
datePicker.MonthColumnTextStyle = new PickerTextStyle()
{
    TextColor = Color.FromArgb("#D53F8C"),
    FontSize = 16,
    FontAttributes = FontAttributes.Bold
};
```

**Note:** Applying `MonthColumnTextStyle` overrides the unselected `TextStyle` for the month column.

### Year Column Customization

```xml
<picker:SfDatePicker x:Name="datePicker"
                     YearColumnWidth="100">
    <picker:SfDatePicker.YearColumnTextStyle>
        <picker:PickerTextStyle FontSize="16" TextColor="#2B6CB0" FontAttributes="Bold"/>
    </picker:SfDatePicker.YearColumnTextStyle>
</picker:SfDatePicker>
```

```csharp
datePicker.YearColumnWidth = 100;
datePicker.YearColumnTextStyle = new PickerTextStyle()
{
    TextColor = Color.FromArgb("#2B6CB0"),
    FontSize = 16,
    FontAttributes = FontAttributes.Bold
};
```

**Note:** Applying `YearColumnTextStyle` overrides the unselected `TextStyle` for the year column.

### All Columns Combined

```xml
<picker:SfDatePicker x:Name="datePicker"
                     DayColumnWidth="100"
                     MonthColumnWidth="140"
                     YearColumnWidth="90">
    <picker:SfDatePicker.DayColumnTextStyle>
        <picker:PickerTextStyle FontSize="15" TextColor="#2F855A"/>
    </picker:SfDatePicker.DayColumnTextStyle>
    <picker:SfDatePicker.MonthColumnTextStyle>
        <picker:PickerTextStyle FontSize="15" TextColor="#D53F8C"/>
    </picker:SfDatePicker.MonthColumnTextStyle>
    <picker:SfDatePicker.YearColumnTextStyle>
        <picker:PickerTextStyle FontSize="15" TextColor="#2B6CB0"/>
    </picker:SfDatePicker.YearColumnTextStyle>
</picker:SfDatePicker>
```

## Complete Customization Example

```xml
<picker:SfDatePicker x:Name="datePicker"
                     Mode="Dialog"
                     ShowCloseButton="True"
                     ColumnDividerColor="#6200EE"
                     DayColumnWidth="100"
                     MonthColumnWidth="140"
                     YearColumnWidth="90">
    
    <!-- Header -->
    <picker:SfDatePicker.HeaderView>
        <picker:PickerHeaderView Text="Select Date" 
                                 Height="60"
                                 Background="#6200EE"
                                 DividerColor="#3700B3">
            <picker:PickerHeaderView.TextStyle>
                <picker:PickerTextStyle FontSize="20" 
                                        TextColor="White" 
                                        FontAttributes="Bold"/>
            </picker:PickerHeaderView.TextStyle>
        </picker:PickerHeaderView>
    </picker:SfDatePicker.HeaderView>
    
    <!-- Column Header -->
    <picker:SfDatePicker.ColumnHeaderView>
        <picker:DatePickerColumnHeaderView Height="45"
                                           Background="#F5F5F5"
                                           DividerColor="#E0E0E0">
            <picker:DatePickerColumnHeaderView.TextStyle>
                <picker:PickerTextStyle FontSize="14" 
                                        TextColor="#666666" 
                                        FontAttributes="Bold"/>
            </picker:DatePickerColumnHeaderView.TextStyle>
        </picker:DatePickerColumnHeaderView>
    </picker:SfDatePicker.ColumnHeaderView>
    
    <!-- Selection View -->
    <picker:SfDatePicker.SelectionView>
        <picker:PickerSelectionView CornerRadius="8" 
                                    Stroke="#6200EE"
                                    Background="#E3D7FF"
                                    Padding="8,4,8,4"/>
    </picker:SfDatePicker.SelectionView>
    
    <!-- Selected Text Style -->
    <picker:SfDatePicker.SelectedTextStyle>
        <picker:PickerTextStyle FontSize="18" 
                                TextColor="#6200EE"
                                FontAttributes="Bold"/>
    </picker:SfDatePicker.SelectedTextStyle>
    
    <!-- Column Styles -->
    <picker:SfDatePicker.DayColumnTextStyle>
        <picker:PickerTextStyle FontSize="15" TextColor="#2F855A"/>
    </picker:SfDatePicker.DayColumnTextStyle>
    <picker:SfDatePicker.MonthColumnTextStyle>
        <picker:PickerTextStyle FontSize="15" TextColor="#D53F8C"/>
    </picker:SfDatePicker.MonthColumnTextStyle>
    <picker:SfDatePicker.YearColumnTextStyle>
        <picker:PickerTextStyle FontSize="15" TextColor="#2B6CB0"/>
    </picker:SfDatePicker.YearColumnTextStyle>
    
    <!-- Footer -->
    <picker:SfDatePicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True"
                                 Height="60"
                                 Background="#FAFAFA"
                                 OkButtonText="Confirm"
                                 CancelButtonText="Cancel"
                                 DividerColor="#E0E0E0">
            <picker:PickerFooterView.TextStyle>
                <picker:PickerTextStyle FontSize="16" 
                                        TextColor="#6200EE"
                                        FontAttributes="Bold"/>
            </picker:PickerFooterView.TextStyle>
        </picker:PickerFooterView>
    </picker:SfDatePicker.FooterView>
</picker:SfDatePicker>
```

## Best Practices

1. **Consistent Color Scheme** - Use consistent colors across header, footer, and selection views
2. **Readable Fonts** - Ensure text sizes are readable on all devices (minimum 12-14pt)
3. **Contrast** - Maintain good contrast between text and background colors
4. **Template Performance** - Use templates sparingly as they can impact performance
5. **Test on Devices** - Test custom styles on multiple devices and screen sizes

## Related Topics

- **Picker Modes** - Use customization with different display modes
- **Events** - Handle user interactions with customized controls
- **Accessibility** - Ensure customized controls remain accessible
