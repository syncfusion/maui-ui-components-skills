# Customization

## Table of Contents
- [Overview](#overview)
- [Header Customization](#header-customization)
- [Column Header Customization](#column-header-customization)
- [Footer Customization](#footer-customization)
- [Selection View Customization](#selection-view-customization)
- [Background and Text Styling](#background-and-text-styling)
- [Advanced Customization](#advanced-customization)

## Overview

The DateTimePicker provides extensive customization options for headers, footers, column headers, selection views, and overall appearance. You can customize using properties or DataTemplates for complete control.

**Customizable Components:**
- Header View - Title area with date/time display
- Column Header View - Labels for day, month, year, etc.
- Footer View - OK and Cancel buttons
- Selection View - Highlighted selected item
- Background colors and text styles

## Header Customization

The header appears at the top of the picker and typically displays the selected date and time.

### Default Header

By default, the header shows the current date and time using the `DateFormat` and `TimeFormat` properties.

### Set Divider Color

Customize the line that separates the header from the picker content:

```xaml
<picker:SfDateTimePicker>
    <picker:SfDateTimePicker.HeaderView>
        <picker:DateTimePickerHeaderView DividerColor="Red" />
    </picker:SfDateTimePicker.HeaderView>
</picker:SfDateTimePicker>
```

```csharp
var picker = new SfDateTimePicker();
picker.HeaderView = new DateTimePickerHeaderView
{
    DividerColor = Colors.Red
};
```

### Header Height

```xaml
<picker:SfDateTimePicker>
    <picker:SfDateTimePicker.HeaderView>
        <picker:DateTimePickerHeaderView 
            Height="60"
            DividerColor="Blue" />
    </picker:SfDateTimePicker.HeaderView>
</picker:SfDateTimePicker>
```

### Header Background and Text Color

```xaml
<picker:SfDateTimePicker>
    <picker:SfDateTimePicker.HeaderView>
        <picker:DateTimePickerHeaderView 
            Height="50"
            Background="#2196F3"
            TextColor="White"
            DividerColor="White" />
    </picker:SfDateTimePicker.HeaderView>
</picker:SfDateTimePicker>
```

### Custom Header with DataTemplate

For complete control over header appearance, use `HeaderTemplate`:

```xaml
<picker:SfDateTimePicker x:Name="dateTimePicker">
    <picker:SfDateTimePicker.HeaderTemplate>
        <DataTemplate>
            <Grid BackgroundColor="#BB9AB1" Padding="10">
                <Label 
                    Text="Select a Date and Time"
                    TextColor="White"
                    FontSize="18"
                    FontAttributes="Bold"
                    HorizontalOptions="Center"
                    VerticalOptions="Center" />
            </Grid>
        </DataTemplate>
    </picker:SfDateTimePicker.HeaderTemplate>
</picker:SfDateTimePicker>
```

**Note**: When using `HeaderTemplate`, most `HeaderView` properties won't work except `DividerColor`.

### Header with Icon

```xaml
<picker:SfDateTimePicker>
    <picker:SfDateTimePicker.HeaderTemplate>
        <DataTemplate>
            <Grid BackgroundColor="#4CAF50" Padding="15">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                
                <Label 
                    Grid.Column="0"
                    Text="📅"
                    FontSize="24"
                    VerticalOptions="Center" />
                
                <Label 
                    Grid.Column="1"
                    Text="Pick Your Date & Time"
                    TextColor="White"
                    FontSize="16"
                    Margin="10,0,0,0"
                    VerticalOptions="Center" />
            </Grid>
        </DataTemplate>
    </picker:SfDateTimePicker.HeaderTemplate>
</picker:SfDateTimePicker>
```

### Header with DataTemplateSelector

Choose header templates dynamically based on conditions:

**Define Templates:**
```xaml
<ContentPage.Resources>
    <DataTemplate x:Key="todayTemplate">
        <Grid BackgroundColor="LightBlue">
            <Label 
                Text="TODAY - Select Time"
                TextColor="Red"
                FontAttributes="Bold"
                HorizontalOptions="Center"
                VerticalOptions="Center" />
        </Grid>
    </DataTemplate>
    
    <DataTemplate x:Key="normalTemplate">
        <Grid BackgroundColor="LightGreen">
            <Label 
                Text="Select Date and Time"
                TextColor="DarkGreen"
                HorizontalOptions="Center"
                VerticalOptions="Center" />
        </Grid>
    </DataTemplate>
    
    <local:DateTimeTemplateSelector 
        x:Key="headerSelector"
        TodayTemplate="{StaticResource todayTemplate}"
        NormalTemplate="{StaticResource normalTemplate}" />
</ContentPage.Resources>

<picker:SfDateTimePicker 
    HeaderTemplate="{StaticResource headerSelector}" />
```

**Template Selector Class:**
```csharp
public class DateTimeTemplateSelector : DataTemplateSelector
{
    public DataTemplate TodayTemplate { get; set; }
    public DataTemplate NormalTemplate { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        var picker = container as SfDateTimePicker;
        if (picker?.SelectedDate?.Date == DateTime.Today)
        {
            return TodayTemplate;
        }
        return NormalTemplate;
    }
}
```

## Column Header Customization

Column headers appear above each picker column (Day, Month, Year, Hour, etc.).

### Basic Column Header Styling

```xaml
<picker:SfDateTimePicker>
    <picker:SfDateTimePicker.ColumnHeaderView>
        <picker:DateTimePickerColumnHeaderView 
            Height="40"
            TextColor="Blue"
            Background="LightGray" />
    </picker:SfDateTimePicker.ColumnHeaderView>
</picker:SfDateTimePicker>
```

### Column Header with Custom Font

```xaml
<picker:SfDateTimePicker>
    <picker:SfDateTimePicker.ColumnHeaderView>
        <picker:DateTimePickerColumnHeaderView 
            Height="45"
            TextColor="DarkSlateGray"
            Background="#E0E0E0"
            FontSize="14"
            FontAttributes="Bold" />
    </picker:SfDateTimePicker.ColumnHeaderView>
</picker:SfDateTimePicker>
```

### Hide Column Headers

```xaml
<picker:SfDateTimePicker>
    <picker:SfDateTimePicker.ColumnHeaderView>
        <picker:DateTimePickerColumnHeaderView Height="0" />
    </picker:SfDateTimePicker.ColumnHeaderView>
</picker:SfDateTimePicker>
```

## Footer Customization

The footer contains OK and Cancel buttons for confirming or discarding selections.

### Basic Footer Styling

```xaml
<picker:SfDateTimePicker>
    <picker:SfDateTimePicker.FooterView>
        <picker:PickerFooterView 
            Height="50"
            Background="LightBlue"
            ShowOkButton="True" />
    </picker:SfDateTimePicker.FooterView>
</picker:SfDateTimePicker>
```

### Custom Button Text

```xaml
<picker:SfDateTimePicker>
    <picker:SfDateTimePicker.FooterView>
        <picker:PickerFooterView 
            Height="50"
            ShowOkButton="True"
            OkButtonText="Confirm"
            CancelButtonText="Discard"
            Background="#2196F3" />
    </picker:SfDateTimePicker.FooterView>
</picker:SfDateTimePicker>
```

### Footer with Custom Colors

```xaml
<picker:SfDateTimePicker>
    <picker:SfDateTimePicker.FooterView>
        <picker:PickerFooterView 
            Height="55"
            ShowOkButton="True"
            Background="#4CAF50"
            OkButtonText="SELECT"
            CancelButtonText="CANCEL"
            TextColor="White" />
    </picker:SfDateTimePicker.FooterView>
</picker:SfDateTimePicker>
```

### Hide Footer

```xaml
<picker:SfDateTimePicker>
    <picker:SfDateTimePicker.FooterView>
        <picker:PickerFooterView Height="0" />
    </picker:SfDateTimePicker.FooterView>
</picker:SfDateTimePicker>
```

### OK Button Only

```xaml
<picker:SfDateTimePicker>
    <picker:SfDateTimePicker.FooterView>
        <picker:PickerFooterView 
            Height="50"
            ShowOkButton="True"
            OkButtonText="Done"
            Background="White" />
    </picker:SfDateTimePicker.FooterView>
</picker:SfDateTimePicker>
```

## Selection View Customization

The selection view highlights the currently selected item in the picker.

### Basic Selection Styling

```xaml
<picker:SfDateTimePicker>
    <picker:SfDateTimePicker.SelectionView>
        <picker:PickerSelectionView 
            Background="#E3F2FD"
            CornerRadius="5"
            Stroke="Blue"
            StrokeThickness="2" />
    </picker:SfDateTimePicker.SelectionView>
</picker:SfDateTimePicker>
```

### Rounded Selection

```xaml
<picker:SfDateTimePicker>
    <picker:SfDateTimePicker.SelectionView>
        <picker:PickerSelectionView 
            Background="LightYellow"
            CornerRadius="15"
            Stroke="Orange"
            StrokeThickness="1" />
    </picker:SfDateTimePicker.SelectionView>
</picker:SfDateTimePicker>
```

### No Border Selection

```xaml
<picker:SfDateTimePicker>
    <picker:SfDateTimePicker.SelectionView>
        <picker:PickerSelectionView 
            Background="#FFEB3B"
            CornerRadius="8"
            StrokeThickness="0" />
    </picker:SfDateTimePicker.SelectionView>
</picker:SfDateTimePicker>
```

## Background and Text Styling

Customize the overall appearance of the picker.

### Picker Background

```xaml
<picker:SfDateTimePicker 
    BackgroundColor="White" />
```

### Text Color for Items

```xaml
<picker:SfDateTimePicker 
    TextColor="DarkBlue"
    BackgroundColor="White" />
```

### Selected Item Text Color

```xaml
<picker:SfDateTimePicker 
    SelectedTextColor="Blue"
    TextColor="Gray" />
```

### Font Customization

```xaml
<picker:SfDateTimePicker 
    FontSize="16"
    FontFamily="Arial"
    FontAttributes="Bold" />
```

## Advanced Customization

### Complete Custom Theme

```xaml
<picker:SfDateTimePicker 
    Mode="Dialog"
    BackgroundColor="White"
    TextColor="#333333"
    SelectedTextColor="#2196F3"
    FontSize="16">
    
    <!-- Header -->
    <picker:SfDateTimePicker.HeaderView>
        <picker:DateTimePickerHeaderView 
            Height="50"
            Background="#2196F3"
            TextColor="White"
            DividerColor="#1976D2" />
    </picker:SfDateTimePicker.HeaderView>
    
    <!-- Column Headers -->
    <picker:SfDateTimePicker.ColumnHeaderView>
        <picker:DateTimePickerColumnHeaderView 
            Height="40"
            TextColor="#2196F3"
            Background="#E3F2FD"
            FontSize="12"
            FontAttributes="Bold" />
    </picker:SfDateTimePicker.ColumnHeaderView>
    
    <!-- Selection View -->
    <picker:SfDateTimePicker.SelectionView>
        <picker:PickerSelectionView 
            Background="#E3F2FD"
            CornerRadius="5"
            Stroke="#2196F3"
            StrokeThickness="2" />
    </picker:SfDateTimePicker.SelectionView>
    
    <!-- Footer -->
    <picker:SfDateTimePicker.FooterView>
        <picker:PickerFooterView 
            Height="50"
            ShowOkButton="True"
            Background="#2196F3"
            TextColor="White"
            OkButtonText="CONFIRM"
            CancelButtonText="CANCEL" />
    </picker:SfDateTimePicker.FooterView>
    
</picker:SfDateTimePicker>
```

### Dark Theme Example

```xaml
<picker:SfDateTimePicker 
    Mode="Dialog"
    BackgroundColor="#1E1E1E"
    TextColor="#E0E0E0"
    SelectedTextColor="#BB86FC"
    FontSize="16">
    
    <picker:SfDateTimePicker.HeaderView>
        <picker:DateTimePickerHeaderView 
            Height="50"
            Background="#2C2C2C"
            TextColor="#BB86FC"
            DividerColor="#BB86FC" />
    </picker:SfDateTimePicker.HeaderView>
    
    <picker:SfDateTimePicker.ColumnHeaderView>
        <picker:DateTimePickerColumnHeaderView 
            Height="40"
            TextColor="#03DAC6"
            Background="#2C2C2C" />
    </picker:SfDateTimePicker.ColumnHeaderView>
    
    <picker:SfDateTimePicker.SelectionView>
        <picker:PickerSelectionView 
            Background="#3700B3"
            CornerRadius="5"
            Stroke="#BB86FC"
            StrokeThickness="1" />
    </picker:SfDateTimePicker.SelectionView>
    
    <picker:SfDateTimePicker.FooterView>
        <picker:PickerFooterView 
            Height="50"
            ShowOkButton="True"
            Background="#2C2C2C"
            TextColor="#BB86FC" />
    </picker:SfDateTimePicker.FooterView>
    
</picker:SfDateTimePicker>
```

### Material Design Style

```xaml
<picker:SfDateTimePicker 
    BackgroundColor="White"
    TextColor="#212121"
    SelectedTextColor="#6200EE">
    
    <picker:SfDateTimePicker.HeaderTemplate>
        <DataTemplate>
            <Grid BackgroundColor="#6200EE" Padding="16">
                <Label 
                    Text="Select Date & Time"
                    TextColor="White"
                    FontSize="20" />
            </Grid>
        </DataTemplate>
    </picker:SfDateTimePicker.HeaderTemplate>
    
    <picker:SfDateTimePicker.SelectionView>
        <picker:PickerSelectionView 
            Background="#F3E5F5"
            CornerRadius="4"
            Stroke="#6200EE"
            StrokeThickness="2" />
    </picker:SfDateTimePicker.SelectionView>
    
    <picker:SfDateTimePicker.FooterView>
        <picker:PickerFooterView 
            Height="52"
            ShowOkButton="True"
            TextColor="#6200EE"
            OkButtonText="OK" />
    </picker:SfDateTimePicker.FooterView>
    
</picker:SfDateTimePicker>
```

## Best Practices

1. **Consistent Theming**: Match your app's color scheme
2. **Readability**: Ensure sufficient contrast between text and background
3. **Touch Targets**: Keep footer buttons >= 44pt high for easy tapping
4. **Platform Conventions**: Follow iOS/Android design guidelines
5. **Accessibility**: Use semantic colors and sufficient contrast ratios
6. **Performance**: Avoid complex templates if not needed
7. **Testing**: Test on different screen sizes and orientations

## Common Customization Patterns

### Pattern 1: Branded Picker
```xaml
Background="{StaticResource BrandPrimary}"
HeaderView.Background="{StaticResource BrandDark}"
FooterView.Background="{StaticResource BrandLight}"
```

### Pattern 2: Minimal Design
```xaml
HeaderView.Height="0"
FooterView.Height="0"
SelectionView.StrokeThickness="0"
```

### Pattern 3: High Contrast
```xaml
BackgroundColor="White"
TextColor="Black"
SelectedTextColor="Blue"
SelectionView.Background="LightBlue"
SelectionView.Stroke="DarkBlue"
```
