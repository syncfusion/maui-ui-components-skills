# Header, Footer, and Selection View Customization

The Syncfusion .NET MAUI Picker allows extensive customization of the header, footer, and selection views to create a polished user experience.

## Table of Contents
- [Header View](#header-view)
- [Footer View](#footer-view)
- [Selection View](#selection-view)
- [Validation with Buttons](#validation-with-buttons)
- [IsSelectionImmediate Property](#isselectionimmediate-property)

## Header View

The header view displays title text at the top of the picker, providing context for the user's selection.

### Enable or Disable Header

Enable the header by setting `Height` to a value greater than 0.

**Default value:** `0` (disabled)

**XAML:**
```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.HeaderView>
        <picker:PickerHeaderView Height="40" />
    </picker:SfPicker.HeaderView>
</picker:SfPicker>
```

**C#:**
```csharp
picker.HeaderView.Height = 40;
```

### Header Text

Set descriptive text to explain the picker's purpose.

```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.HeaderView>
        <picker:PickerHeaderView Text="Select a color" Height="40" />
    </picker:SfPicker.HeaderView>
</picker:SfPicker>
```

```csharp
picker.HeaderView = new PickerHeaderView()
{
    Text = "Select a color",
    Height = 40
};
```

### Header Background

Customize the header background color.

```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.HeaderView>
        <picker:PickerHeaderView Background="#6750A4" Height="40" />
    </picker:SfPicker.HeaderView>
</picker:SfPicker>
```

```csharp
picker.HeaderView.Background = Color.FromArgb("#6750A4");
```

### Header Text Style

Customize text appearance including color, font size, family, and attributes.

```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.HeaderView>
        <picker:PickerHeaderView Height="40">
            <picker:PickerHeaderView.TextStyle>
                <picker:PickerTextStyle TextColor="Gray" 
                                       FontSize="18" 
                                       FontAttributes="Italic"/>
            </picker:PickerHeaderView.TextStyle>
        </picker:PickerHeaderView>
    </picker:SfPicker.HeaderView>
</picker:SfPicker>
```

```csharp
picker.HeaderView.TextStyle = new PickerTextStyle()
{
    TextColor = Colors.Gray,
    FontSize = 18,
    FontAttributes = FontAttributes.Italic
};
```

### Header Divider Color

Customize the divider line below the header.

```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.HeaderView>
        <picker:PickerHeaderView DividerColor="Red" Height="40" />
    </picker:SfPicker.HeaderView>
</picker:SfPicker>
```

```csharp
picker.HeaderView.DividerColor = Colors.Red;
```

### Close Button

Show a close button in the header (typically used with Dialog mode).

```xml
<Grid>
    <picker:SfPicker x:Name="picker" 
                     Mode="Dialog" 
                     ShowCloseButton="True">
        <picker:SfPicker.HeaderView>
            <picker:PickerHeaderView Height="40" Text="Select Date"/>
        </picker:SfPicker.HeaderView>
    </picker:SfPicker>
    
    <Button Text="Open Picker" 
            Clicked="Button_Clicked"/>
</Grid>
```

**C#:**
```csharp
private void Button_Clicked(object sender, EventArgs e)
{
    picker.IsOpen = true;
}
```

**Key Points:**
- `ShowCloseButton` default is `false`
- Requires header view to be present
- Automatically closes picker when clicked

### Custom Close Button Icon

```xml
<picker:SfPicker x:Name="picker" 
                 ShowCloseButton="True" 
                 Mode="Dialog" 
                 CloseButtonIcon="closeicon.png">
    <picker:SfPicker.HeaderView>
        <picker:PickerHeaderView Height="40" Text="Select"/>
    </picker:SfPicker.HeaderView>
</picker:SfPicker>
```

### Custom Header Template

Create fully custom header appearance using `DataTemplate`.

```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.HeaderTemplate>
        <DataTemplate>
            <Grid BackgroundColor="#BB9AB1">
                <Label HorizontalOptions="Center" 
                       VerticalOptions="Center" 
                       Text="Select a Color" 
                       TextColor="White"/>
            </Grid>
        </DataTemplate>
    </picker:SfPicker.HeaderTemplate>
</picker:SfPicker>
```

**Note:** When using `HeaderTemplate`, only `DividerColor` from `PickerHeaderView` properties remains effective.

### Custom Header with DataTemplateSelector

Apply different templates based on conditions.

**XAML:**
```xml
<Grid.Resources>
    <DataTemplate x:Key="selectedItemTemplate">
        <Grid Background="LightBlue">
            <Label Text="Select a Color" 
                   HorizontalOptions="Center" 
                   VerticalOptions="Center" 
                   TextColor="Red"/>
        </Grid>
    </DataTemplate>
    
    <DataTemplate x:Key="nonSelectedItemTemplate">
        <Grid Background="LightGreen">
            <Label Text="Select a Color" 
                   HorizontalOptions="Center" 
                   VerticalOptions="Center" 
                   TextColor="Orange"/>
        </Grid>
    </DataTemplate>
    
    <local:PickerTemplateSelector x:Key="headerTemplateSelector" 
                                  SelectedItemTemplate="{StaticResource selectedItemTemplate}"  
                                  NonSelectedItemTemplate="{StaticResource nonSelectedItemTemplate}"/>
</Grid.Resources>

<picker:SfPicker x:Name="picker" 
                 HeaderTemplate="{StaticResource headerTemplateSelector}">
</picker:SfPicker>
```

**C#:**
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

## Footer View

The footer view displays validation buttons (OK and Cancel) at the bottom of the picker.

### Enable or Disable Footer

Enable the footer by setting `Height` to a value greater than 0.

**Default value:** `0` (disabled)

```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.FooterView>
        <picker:PickerFooterView Height="40"/>
    </picker:SfPicker.FooterView>
</picker:SfPicker>
```

```csharp
picker.FooterView.Height = 40;
```

### Footer Background

Customize the footer background color.

```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.FooterView>
        <picker:PickerFooterView Background="#6750A4" Height="40"/>
    </picker:SfPicker.FooterView>
</picker:SfPicker>
```

```csharp
picker.FooterView.Background = Color.FromArgb("#6750A4");
```

### Button Customization

Control button visibility and text.

**Show/Hide OK Button:**
```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True" Height="40"/>
    </picker:SfPicker.FooterView>
</picker:SfPicker>
```

**Default value:** `true`

**Customize Button Text:**
```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True"
                                 OkButtonText="Done" 
                                 CancelButtonText="Exit"
                                 Height="40"/>
    </picker:SfPicker.FooterView>
</picker:SfPicker>
```

```csharp
picker.FooterView.ShowOkButton = true;
picker.FooterView.OkButtonText = "Done";
picker.FooterView.CancelButtonText = "Exit";
```

### Footer Text Style

Customize button text appearance.

```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.FooterView>
        <picker:PickerFooterView Height="40">
            <picker:PickerFooterView.TextStyle>
                <picker:PickerTextStyle TextColor="Gray" 
                                       FontSize="18" 
                                       FontAttributes="Italic"/>
            </picker:PickerFooterView.TextStyle>
        </picker:PickerFooterView>
    </picker:SfPicker.FooterView>
</picker:SfPicker>
```

```csharp
picker.FooterView.TextStyle = new PickerTextStyle()
{
    TextColor = Colors.Gray,
    FontSize = 18,
    FontAttributes = FontAttributes.Italic
};
```

### Footer Divider Color

Customize the divider line above the footer.

```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.FooterView>
        <picker:PickerFooterView DividerColor="Red" Height="40"/>
    </picker:SfPicker.FooterView>
</picker:SfPicker>
```

```csharp
picker.FooterView.DividerColor = Colors.Red;
```

### Custom Footer Template

Create a fully custom footer using `DataTemplate`.

```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.FooterTemplate>
        <DataTemplate>
            <Grid BackgroundColor="#BB9AB1">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Button Grid.Column="0" 
                        Text="Decline" 
                        TextColor="White" 
                        Background="Transparent" />
                <Button Grid.Column="1" 
                        Text="Accept" 
                        TextColor="White" 
                        Background="Transparent" />
            </Grid>
        </DataTemplate>
    </picker:SfPicker.FooterTemplate>
</picker:SfPicker>
```

**Note:** When using `FooterTemplate`, only `DividerColor` from `PickerFooterView` properties remains effective.

## Selection View

The selection view highlights the currently selected item in the picker.

### Background and Shape Customization

Customize the appearance of the selection indicator.

```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.SelectionView>
        <picker:PickerSelectionView CornerRadius="10" 
                                    Stroke="#36454F" 
                                    Padding="10,5,10,5" 
                                    Background="#808080" />
    </picker:SfPicker.SelectionView>
</picker:SfPicker>
```

```csharp
picker.SelectionView = new PickerSelectionView()
{
    CornerRadius = 10,
    Stroke = Color.FromArgb("#36454F"),
    Padding = new Thickness(10, 5, 10, 5),
    Background = Color.FromArgb("#808080"),
};
```

**Properties:**
- **CornerRadius**: Rounds the corners of the selection box
- **Stroke**: Border color of the selection box
- **Background**: Fill color of the selection box
- **Padding**: Space inside the selection box

## Validation with Buttons

Handle user validation using OK and Cancel button events.

```xml
<picker:SfPicker x:Name="picker"
                 OkButtonClicked="Picker_OkButtonClicked"
                 CancelButtonClicked="Picker_CancelButtonClicked">
    <picker:SfPicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True" Height="40"/>
    </picker:SfPicker.FooterView>
</picker:SfPicker>
```

```csharp
private void Picker_OkButtonClicked(object sender, EventArgs e)
{
    // Confirm selection
    var selectedItem = picker.Columns[0].SelectedItem;
    // Perform validation or save operation
}

private void Picker_CancelButtonClicked(object sender, EventArgs e)
{
    // Cancel selection
    // Reset to previous value or close without saving
}
```

## IsSelectionImmediate Property

Control when the selection is committed.

**When `IsSelectionImmediate` is `false` (default):**
- `SelectedIndex` and `SelectedItem` update only when OK button is tapped
- Applies when: Mode is Dialog/RelativeDialog, footer height > 0, and ShowOkButton is enabled
- For single-column pickers only

**When `IsSelectionImmediate` is `true`:**
- `SelectedIndex` and `SelectedItem` update immediately upon scrolling
- Real-time selection updates

**Example:**
```xml
<picker:SfPicker x:Name="picker"
                 IsSelectionImmediate="True">
    <!-- Picker configuration -->
</picker:SfPicker>
```

```csharp
picker.IsSelectionImmediate = true;
```

## Complete Example

```xml
<picker:SfPicker x:Name="picker"
                 HeightRequest="350"
                 WidthRequest="300">
    
    <!-- Header -->
    <picker:SfPicker.HeaderView>
        <picker:PickerHeaderView Text="Select Your Color" 
                                 Height="50"
                                 Background="#6750A4">
            <picker:PickerHeaderView.TextStyle>
                <picker:PickerTextStyle TextColor="White" 
                                       FontSize="18" 
                                       FontAttributes="Bold"/>
            </picker:PickerHeaderView.TextStyle>
        </picker:PickerHeaderView>
    </picker:SfPicker.HeaderView>
    
    <!-- Columns -->
    <picker:SfPicker.Columns>
        <picker:PickerColumn ItemsSource="{Binding Colors}" />
    </picker:SfPicker.Columns>
    
    <!-- Selection View -->
    <picker:SfPicker.SelectionView>
        <picker:PickerSelectionView CornerRadius="8" 
                                    Background="#E8DEF8"
                                    Stroke="#6750A4"
                                    Padding="10,5"/>
    </picker:SfPicker.SelectionView>
    
    <!-- Footer -->
    <picker:SfPicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True"
                                 Height="50"
                                 Background="#E8DEF8"
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

## Best Practices

1. **Always provide meaningful header text** for user context
2. **Use appropriate colors** that maintain readability and contrast
3. **Customize button text** for your app's terminology (e.g., "Select" instead of "OK")
4. **Test visibility** of selection view against your color scheme
5. **Consider accessibility** when choosing colors and font sizes
6. **Use IsSelectionImmediate wisely** - immediate updates for live previews, button validation for critical selections
