# Picker Modes in .NET MAUI DatePicker

The .NET MAUI DatePicker supports multiple display modes to control how the picker is presented to users. The `Mode` property accepts values from the `PickerMode` enumeration.

## Available Modes

- **Default** - Displays the picker inline (default mode)
- **Dialog** - Displays the picker in a centered popup dialog
- **RelativeDialog** - Displays the picker in a popup positioned relative to a view

## Default Mode

The Default mode displays the DatePicker inline within the layout. This is the default behavior when no Mode is specified.

### XAML

```xml
<picker:SfDatePicker x:Name="datePicker"
                     Mode="Default">
</picker:SfDatePicker>
```

### C#

```csharp
SfDatePicker datePicker = new SfDatePicker()
{
    Mode = PickerMode.Default
};

this.Content = datePicker;
```

**When to use:** Use Default mode when you want the picker to be always visible and part of the layout flow.

## Dialog Mode

Dialog mode displays the DatePicker in a centered popup dialog. This is useful when you want to save screen space and show the picker only when needed.

### Basic Dialog Implementation

#### XAML

```xml
<picker:SfDatePicker x:Name="datePicker"
                     Mode="Dialog">
</picker:SfDatePicker>
```

#### C#

```csharp
SfDatePicker datePicker = new SfDatePicker()
{
    Mode = PickerMode.Dialog
};

this.Content = datePicker;
```

### Opening the Dialog Programmatically

The DatePicker can be opened programmatically by setting the `IsOpen` property to `true`. By default, `IsOpen` is `false`.

**Note:** The `IsOpen` property automatically changes to `false` when the dialog is closed by clicking outside of it.

#### Complete Example

```xml
<Grid>
    <picker:SfDatePicker x:Name="datePicker"
                         Mode="Dialog">
        <picker:SfDatePicker.HeaderView>
            <picker:PickerHeaderView Text="Select Date" Height="40" />
        </picker:SfDatePicker.HeaderView>
        <picker:SfDatePicker.FooterView>
            <picker:PickerFooterView ShowOkButton="True" Height="40" />
        </picker:SfDatePicker.FooterView>
    </picker:SfDatePicker>
    
    <Button Text="Open Picker" 
            x:Name="pickerButton"
            Clicked="Button_Clicked"
            HorizontalOptions="Center"
            VerticalOptions="Center"
            HeightRequest="50" 
            WidthRequest="150">
    </Button>
</Grid>
```

```csharp
private void Button_Clicked(object sender, EventArgs e)
{
    datePicker.IsOpen = true;
}
```

**When to use:** Use Dialog mode for forms, input screens, or when screen space is limited.

## RelativeDialog Mode

RelativeDialog mode displays the DatePicker in a popup positioned relative to a specified view. This provides flexible positioning options.

### Relative Positions

The `RelativePosition` property accepts the following values:

- **AlignTop** - Aligns picker to the top of the relative view (default)
- **AlignBottom** - Aligns picker to the bottom of the relative view
- **AlignToLeftOf** - Aligns picker to the left of the relative view
- **AlignToRightOf** - Aligns picker to the right of the relative view
- **AlignTopLeft** - Aligns picker to the top-left corner
- **AlignTopRight** - Aligns picker to the top-right corner
- **AlignBottomLeft** - Aligns picker to the bottom-left corner
- **AlignBottomRight** - Aligns picker to the bottom-right corner

### Basic RelativeDialog Implementation

#### XAML

```xml
<Grid>
    <picker:SfDatePicker x:Name="datePicker" 
                         Mode="RelativeDialog"
                         RelativePosition="AlignTopLeft">
        <picker:SfDatePicker.HeaderView>
            <picker:PickerHeaderView Text="Select Date" Height="40" />
        </picker:SfDatePicker.HeaderView>
    </picker:SfDatePicker>
    
    <Button Text="Open picker" 
            x:Name="pickerButton"
            Clicked="Button_Clicked"
            HorizontalOptions="Center"
            VerticalOptions="Center"
            HeightRequest="50" 
            WidthRequest="150">
    </Button>
</Grid>
```

```csharp
private void Button_Clicked(object sender, EventArgs e)
{
    datePicker.IsOpen = true;
}
```

### Setting Relative View

The `RelativeView` property specifies which view the picker should be positioned relative to. If no relative view is specified, the picker base will be used as the default relative view.

**Note:** RelativeView is only applicable in `RelativeDialog` mode.

#### XAML

```xml
<Grid>
    <picker:SfDatePicker x:Name="datePicker" 
                         Mode="RelativeDialog"
                         RelativePosition="AlignTopLeft"
                         RelativeView="{x:Reference pickerButton}">
        <picker:SfDatePicker.HeaderView>
            <picker:PickerHeaderView Text="Select Date" Height="40" />
        </picker:SfDatePicker.HeaderView>
    </picker:SfDatePicker>
    
    <Button Text="Open picker" 
            x:Name="pickerButton"
            Clicked="Button_Clicked"
            HorizontalOptions="Center"
            VerticalOptions="Center"
            HeightRequest="50" 
            WidthRequest="150">
    </Button>
</Grid>
```

#### C#

```csharp
private void Button_Clicked(object sender, EventArgs e)
{
    datePicker.IsOpen = true;
    datePicker.RelativeView = pickerButton;
}
```

**When to use:** Use RelativeDialog mode for context-sensitive pickers that should appear near specific UI elements.

## Custom Popup Size

In Dialog and RelativeDialog modes, you can customize the popup size using the `PopupWidth` and `PopupHeight` properties.

### XAML

```xml
<picker:SfDatePicker x:Name="datePicker" 
                     Mode="Dialog"
                     PopupWidth="300"
                     PopupHeight="400">
</picker:SfDatePicker>
```

### C#

```csharp
SfDatePicker datePicker = new SfDatePicker()
{
    Mode = PickerMode.Dialog,
    PopupWidth = 300,
    PopupHeight = 400
};

this.Content = datePicker;
```

## Complete Examples

### Example 1: Dialog with Custom Size and Styling

```xml
<Grid>
    <picker:SfDatePicker x:Name="datePicker"
                         Mode="Dialog"
                         PopupWidth="350"
                         PopupHeight="450"
                         SelectedDate="2023/09/15">
        <picker:SfDatePicker.HeaderView>
            <picker:PickerHeaderView Text="Select Appointment Date" 
                                     Height="50"
                                     Background="#6200EE">
                <picker:PickerHeaderView.TextStyle>
                    <picker:PickerTextStyle TextColor="White" FontSize="18" />
                </picker:PickerHeaderView.TextStyle>
            </picker:PickerHeaderView>
        </picker:SfDatePicker.HeaderView>
        
        <picker:SfDatePicker.FooterView>
            <picker:PickerFooterView ShowOkButton="True" 
                                     Height="50"
                                     OkButtonText="Confirm"
                                     CancelButtonText="Cancel"
                                     Background="#F5F5F5">
            </picker:PickerFooterView>
        </picker:SfDatePicker.FooterView>
    </picker:SfDatePicker>
    
    <Button Text="Select Date" 
            Clicked="OpenDialog_Clicked"
            BackgroundColor="#6200EE"
            TextColor="White"
            CornerRadius="8"
            HeightRequest="50" 
            WidthRequest="200"
            HorizontalOptions="Center"
            VerticalOptions="Center">
    </Button>
</Grid>
```

```csharp
private void OpenDialog_Clicked(object sender, EventArgs e)
{
    datePicker.IsOpen = true;
}
```

### Example 2: RelativeDialog with Multiple Position Options

```xml
<StackLayout Padding="20" Spacing="20">
    <Label Text="Select Date Picker Position:" FontSize="18" FontAttributes="Bold"/>
    
    <Picker x:Name="positionPicker" 
            Title="Select Position"
            SelectedIndexChanged="PositionPicker_SelectedIndexChanged">
        <Picker.Items>
            <x:String>AlignTop</x:String>
            <x:String>AlignBottom</x:String>
            <x:String>AlignToLeftOf</x:String>
            <x:String>AlignToRightOf</x:String>
            <x:String>AlignTopLeft</x:String>
            <x:String>AlignTopRight</x:String>
            <x:String>AlignBottomLeft</x:String>
            <x:String>AlignBottomRight</x:String>
        </Picker.Items>
    </Picker>
    
    <picker:SfDatePicker x:Name="datePicker" 
                         Mode="RelativeDialog"
                         RelativePosition="AlignTopLeft"
                         RelativeView="{x:Reference openButton}">
        <picker:SfDatePicker.HeaderView>
            <picker:PickerHeaderView Text="Select Date" Height="40" />
        </picker:SfDatePicker.HeaderView>
    </picker:SfDatePicker>
    
    <Button x:Name="openButton"
            Text="Open Date Picker" 
            Clicked="Button_Clicked"
            HorizontalOptions="Center"
            HeightRequest="50" 
            WidthRequest="200">
    </Button>
</StackLayout>
```

```csharp
private void Button_Clicked(object sender, EventArgs e)
{
    datePicker.IsOpen = true;
}

private void PositionPicker_SelectedIndexChanged(object sender, EventArgs e)
{
    var selectedPosition = positionPicker.SelectedItem as string;
    
    switch (selectedPosition)
    {
        case "AlignTop":
            datePicker.RelativePosition = PickerRelativePosition.AlignTop;
            break;
        case "AlignBottom":
            datePicker.RelativePosition = PickerRelativePosition.AlignBottom;
            break;
        case "AlignToLeftOf":
            datePicker.RelativePosition = PickerRelativePosition.AlignToLeftOf;
            break;
        case "AlignToRightOf":
            datePicker.RelativePosition = PickerRelativePosition.AlignToRightOf;
            break;
        case "AlignTopLeft":
            datePicker.RelativePosition = PickerRelativePosition.AlignTopLeft;
            break;
        case "AlignTopRight":
            datePicker.RelativePosition = PickerRelativePosition.AlignTopRight;
            break;
        case "AlignBottomLeft":
            datePicker.RelativePosition = PickerRelativePosition.AlignBottomLeft;
            break;
        case "AlignBottomRight":
            datePicker.RelativePosition = PickerRelativePosition.AlignBottomRight;
            break;
    }
}
```

### Example 3: Dialog with Programmatic Control

```xml
<StackLayout Padding="20" Spacing="15">
    <Label Text="Date Picker Dialog Control" 
           FontSize="20" 
           FontAttributes="Bold"/>
    
    <picker:SfDatePicker x:Name="datePicker"
                         Mode="Dialog"
                         SelectionChanged="DatePicker_SelectionChanged">
        <picker:SfDatePicker.HeaderView>
            <picker:PickerHeaderView Text="Select Date" Height="40" />
        </picker:SfDatePicker.HeaderView>
        <picker:SfDatePicker.FooterView>
            <picker:PickerFooterView ShowOkButton="True" Height="40" />
        </picker:SfDatePicker.FooterView>
    </picker:SfDatePicker>
    
    <Label x:Name="selectedDateLabel" 
           Text="No date selected" 
           FontSize="16"/>
    
    <Grid ColumnDefinitions="*,*" ColumnSpacing="10">
        <Button Grid.Column="0"
                Text="Open Picker" 
                Clicked="OpenPicker_Clicked"/>
        <Button Grid.Column="1"
                Text="Close Picker" 
                Clicked="ClosePicker_Clicked"/>
    </Grid>
</StackLayout>
```

```csharp
private void OpenPicker_Clicked(object sender, EventArgs e)
{
    datePicker.IsOpen = true;
}

private void ClosePicker_Clicked(object sender, EventArgs e)
{
    datePicker.IsOpen = false;
}

private void DatePicker_SelectionChanged(object sender, DatePickerSelectionChangedEventArgs e)
{
    if (e.NewValue != null)
    {
        selectedDateLabel.Text = $"Selected: {e.NewValue:dddd, MMMM dd, yyyy}";
    }
}
```

## Mode Comparison

| Feature | Default | Dialog | RelativeDialog |
|---------|---------|--------|----------------|
| **Display** | Always visible | Popup (centered) | Popup (positioned) |
| **Space Usage** | Takes layout space | Overlay | Overlay |
| **IsOpen Property** | Not applicable | Required | Required |
| **RelativePosition** | Not applicable | Not applicable | Available |
| **RelativeView** | Not applicable | Not applicable | Available |
| **Custom Size** | HeightRequest/WidthRequest | PopupWidth/PopupHeight | PopupWidth/PopupHeight |
| **Use Case** | Forms, always-visible | Space-saving dialogs | Context menus, tooltips |

## Best Practices

### 1. Choose the Right Mode
- **Default:** For dedicated date selection screens
- **Dialog:** For forms where space is limited
- **RelativeDialog:** For context-sensitive date selection

### 2. Provide Clear Open/Close Controls
When using Dialog or RelativeDialog modes, provide obvious buttons or triggers to open the picker.

### 3. Handle Dialog Close Events
Subscribe to `Closing` and `Closed` events to handle user actions when the dialog is dismissed.

```csharp
datePicker.Closing += (s, e) =>
{
    // Validate before closing
    if (datePicker.SelectedDate == null)
    {
        e.Cancel = true; // Prevent closing
        DisplayAlert("Required", "Please select a date", "OK");
    }
};
```

### 4. Set Appropriate Popup Sizes
When customizing popup size, ensure adequate space for all date columns and controls:
- Minimum recommended width: 280px
- Minimum recommended height: 300px

### 5. Test on Different Screen Sizes
Test Dialog and RelativeDialog modes on various screen sizes to ensure proper positioning and visibility.

## Troubleshooting

### Issue: Dialog not opening
**Solution:** Ensure `Mode` is set to `Dialog` or `RelativeDialog` and set `IsOpen = true`.

### Issue: RelativePosition not working
**Solution:** Verify that `Mode` is set to `RelativeDialog`. RelativePosition only works in RelativeDialog mode.

### Issue: Popup appears off-screen
**Solution:** Adjust `RelativePosition` or set custom `PopupWidth` and `PopupHeight` values.

### Issue: IsOpen doesn't close the picker
**Solution:** The `IsOpen` property automatically changes to `false` when clicking outside the dialog. To manually close, set `IsOpen = false`.

## Related Topics

- **Events** - Handle Opened, Closing, and Closed events for dialog modes
- **Customization** - Customize the appearance of dialog headers and footers
- **Date Restrictions** - Combine modes with date range restrictions
