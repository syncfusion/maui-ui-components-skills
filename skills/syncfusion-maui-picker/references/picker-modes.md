# Picker Modes in .NET MAUI Picker

The Syncfusion .NET MAUI Picker (SfPicker) supports three display modes that determine how the picker appears to the user. Choose the appropriate mode based on your UI requirements.

## Table of Contents
- [Mode Overview](#mode-overview)
- [Default Mode](#default-mode)
- [Dialog Mode](#dialog-mode)
- [RelativeDialog Mode](#relativedialog-mode)
- [Controlling Picker Visibility](#controlling-picker-visibility)
- [Custom Popup Size](#custom-popup-size)
- [Common Scenarios](#common-scenarios)

## Mode Overview

The picker mode is set using the `Mode` property, which accepts one of three values:

- **Default**: Displays the picker inline within the page layout
- **Dialog**: Shows the picker in a centered popup dialog
- **RelativeDialog**: Displays the picker in a popup positioned relative to a specific view

**Default value:** `PickerMode.Default`

## Default Mode

Default mode displays the picker as an inline control embedded directly in your page layout. The picker is always visible and takes up space in the UI.

**XAML:**
```xml
<picker:SfPicker x:Name="picker"
                 Mode="Default"
                 HeightRequest="280"
                 WidthRequest="300">
    <!-- Picker configuration -->
</picker:SfPicker>
```

**C#:**
```csharp
SfPicker picker = new SfPicker()
{
    Mode = PickerMode.Default,
    HeightRequest = 280,
    WidthRequest = 300
};

this.Content = picker;
```

**When to use Default mode:**
- When the picker is the primary focus of the page
- For always-visible selection interfaces
- In dedicated picker pages or forms
- When you want the picker to be part of the scrollable content

## Dialog Mode

Dialog mode displays the picker in a centered popup that overlays the current page. The background is dimmed, focusing attention on the picker.

**XAML:**
```xml
<picker:SfPicker x:Name="picker"
                 Mode="Dialog">
    <!-- Picker configuration -->
</picker:SfPicker>
```

**C#:**
```csharp
SfPicker picker = new SfPicker()
{
    Mode = PickerMode.Dialog
};

this.Content = picker;
```

### Opening the Dialog Programmatically

Set the `IsOpen` property to `true` to display the picker dialog.

**XAML:**
```xml
<Grid>
    <picker:SfPicker x:Name="picker"
                     Mode="Dialog">
        <!-- Picker configuration -->
    </picker:SfPicker>
    
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

**C#:**
```csharp
private void Button_Clicked(object sender, EventArgs e)
{
    this.picker.IsOpen = true;
}
```

**Key Points:**
- `IsOpen` defaults to `false`
- Automatically changes to `false` when the user closes the dialog
- User can close by clicking outside the picker or the Cancel button

**When to use Dialog mode:**
- For modal selection experiences
- When you want to focus user attention on the picker
- To save space on the main page
- For occasional selections triggered by user action

## RelativeDialog Mode

RelativeDialog mode displays the picker in a popup positioned relative to a specific UI element. This provides flexibility in picker placement.

**XAML:**
```xml
<Grid>
    <picker:SfPicker x:Name="picker" 
                     Mode="RelativeDialog"
                     RelativePosition="AlignTopLeft">
        <!-- Picker configuration -->
    </picker:SfPicker>
    
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

**C#:**
```csharp
SfPicker picker = new SfPicker()
{
    Mode = PickerMode.RelativeDialog,
    RelativePosition = PickerRelativePosition.AlignTopLeft
};

private void Button_Clicked(object sender, EventArgs e)
{
    picker.IsOpen = true;
}
```

### RelativePosition Options

The `RelativePosition` property specifies where the picker appears relative to the reference view:

- **AlignTop**: Above the reference view
- **AlignBottom**: Below the reference view
- **AlignToLeftOf**: To the left of the reference view
- **AlignToRightOf**: To the right of the reference view
- **AlignTopLeft**: Top-left corner alignment
- **AlignTopRight**: Top-right corner alignment
- **AlignBottomLeft**: Bottom-left corner alignment
- **AlignBottomRight**: Bottom-right corner alignment

**Default value:** `AlignTop`

**Example with different positions:**
```csharp
// Align picker to the right of a button
picker.RelativePosition = PickerRelativePosition.AlignToRightOf;

// Align picker below a text field
picker.RelativePosition = PickerRelativePosition.AlignBottom;

// Align picker at top-right corner
picker.RelativePosition = PickerRelativePosition.AlignTopRight;
```

### Setting the RelativeView

Use the `RelativeView` property to specify which UI element the picker should be positioned relative to.

**XAML:**
```xml
<Grid>
    <picker:SfPicker x:Name="picker" 
                     Mode="RelativeDialog"
                     RelativePosition="AlignTopLeft"
                     RelativeView="{x:Reference pickerButton}">
        <!-- Picker configuration -->
    </picker:SfPicker>
    
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

**C#:**
```csharp
private void Button_Clicked(object sender, EventArgs e)
{
    picker.IsOpen = true;
    picker.RelativeView = pickerButton;
}
```

**Important Notes:**
- `RelativeView` only applies in `RelativeDialog` mode
- If no relative view is specified, the picker itself becomes the default relative view
- Automatically changes to `false` when the dialog is closed by clicking outside

**When to use RelativeDialog mode:**
- For dropdown-style pickers
- When positioning near input fields or buttons
- To create contextual selection interfaces
- For space-efficient layouts with dynamic picker placement

## Controlling Picker Visibility

### IsOpen Property

Control whether the picker dialog is visible using the `IsOpen` property. This applies to both Dialog and RelativeDialog modes.

**Programmatic Control:**
```csharp
// Open the picker
picker.IsOpen = true;

// Close the picker
picker.IsOpen = false;

// Check if picker is open
if (picker.IsOpen)
{
    // Picker is currently visible
}
```

**Binding IsOpen in XAML:**
```xml
<picker:SfPicker x:Name="picker"
                 Mode="Dialog"
                 IsOpen="{Binding IsPickerOpen}">
    <!-- Picker configuration -->
</picker:SfPicker>
```

**ViewModel:**
```csharp
public class ViewModel : INotifyPropertyChanged
{
    private bool isPickerOpen;
    
    public bool IsPickerOpen
    {
        get => isPickerOpen;
        set
        {
            isPickerOpen = value;
            OnPropertyChanged(nameof(IsPickerOpen));
        }
    }
    
    public void ShowPicker()
    {
        IsPickerOpen = true;
    }
    
    public void HidePicker()
    {
        IsPickerOpen = false;
    }
}
```

## Custom Popup Size

Customize the dimensions of the picker popup in Dialog or RelativeDialog mode using `PopupWidth` and `PopupHeight` properties.

**XAML:**
```xml
<Grid>
    <picker:SfPicker 
        x:Name="picker"
        Mode="Dialog"
        IsOpen="True"
        PopupWidth="200"
        PopupHeight="440">
        
        <picker:SfPicker.HeaderView>
            <picker:PickerHeaderView Height="40" Text="Select a color" />
        </picker:SfPicker.HeaderView>
        
        <picker:SfPicker.Columns>
            <picker:PickerColumn HeaderText="Colors" 
                                 ItemsSource="{Binding DataSource}"/>
        </picker:SfPicker.Columns>
        
        <picker:SfPicker.ColumnHeaderView>
            <picker:PickerColumnHeaderView Height="40"/>
        </picker:SfPicker.ColumnHeaderView>
        
        <picker:SfPicker.FooterView>
            <picker:PickerFooterView Height="40"/>
        </picker:SfPicker.FooterView>
    </picker:SfPicker>
</Grid>
```

**C#:**
```csharp
private void OpenPickerButton_Clicked(object sender, EventArgs e)
{
    picker.IsOpen = true;
    picker.PopupWidth = 300;
    picker.PopupHeight = 440;
}
```

**Key Points:**
- `PopupWidth` and `PopupHeight` only apply to Dialog and RelativeDialog modes
- Default values adjust based on content
- Use custom sizes for consistent appearance across different screens

## Common Scenarios

### Scenario 1: Dropdown Picker Next to Input Field

```csharp
// Position picker below an entry field
SfPicker picker = new SfPicker()
{
    Mode = PickerMode.RelativeDialog,
    RelativePosition = PickerRelativePosition.AlignBottom,
    RelativeView = entryField,
    PopupWidth = 250
};

// Open when entry is tapped
entryField.Focused += (s, e) =>
{
    picker.IsOpen = true;
};
```

### Scenario 2: Modal Selection Dialog

```csharp
// Full-screen modal picker experience
SfPicker picker = new SfPicker()
{
    Mode = PickerMode.Dialog,
    PopupWidth = 350,
    PopupHeight = 500
};

// Show on button click
selectButton.Clicked += (s, e) =>
{
    picker.IsOpen = true;
};
```

### Scenario 3: Context Menu Style Picker

```csharp
// Small picker appearing near a button
SfPicker picker = new SfPicker()
{
    Mode = PickerMode.RelativeDialog,
    RelativePosition = PickerRelativePosition.AlignToRightOf,
    RelativeView = menuButton,
    PopupWidth = 180,
    PopupHeight = 250
};
```

### Scenario 4: Always-Visible Picker Page

```csharp
// Dedicated picker page with full control
SfPicker picker = new SfPicker()
{
    Mode = PickerMode.Default,
    HeightRequest = 400,
    WidthRequest = 320
};

// Embed in page content
Content = new VerticalStackLayout
{
    Children = { titleLabel, picker, submitButton }
};
```

## Mode Comparison

| Feature | Default | Dialog | RelativeDialog |
|---------|---------|--------|----------------|
| **Display** | Inline | Centered popup | Positioned popup |
| **IsOpen Control** | N/A | Yes | Yes |
| **RelativeView** | N/A | No | Yes |
| **RelativePosition** | N/A | No | Yes |
| **PopupWidth/Height** | N/A | Yes | Yes |
| **Space Usage** | Takes layout space | Overlays content | Overlays content |
| **Background Dim** | No | Yes | Yes |
| **Best For** | Primary focus | Modal selection | Contextual selection |

## Best Practices

1. **Choose the right mode:**
   - Use Default for picker-focused pages
   - Use Dialog for important modal selections
   - Use RelativeDialog for contextual, space-efficient selections

2. **Provide clear triggers:**
   - Use buttons or tappable elements to open Dialog/RelativeDialog pickers
   - Ensure users understand how to activate the picker

3. **Consider mobile UX:**
   - Dialog mode works well on small screens
   - RelativeDialog requires careful positioning on different screen sizes

4. **Handle closing gracefully:**
   - Listen to `Closed` event to update UI state
   - Respect user's Cancel action

5. **Test on different screen sizes:**
   - Ensure RelativeDialog positioning works across devices
   - Adjust PopupWidth/Height for optimal viewing

## Troubleshooting

### Issue: Picker dialog not opening

**Solution:**
- Verify `Mode` is set to `Dialog` or `RelativeDialog`
- Check that `IsOpen` is being set to `true`
- Ensure the picker is part of the visual tree

### Issue: RelativeView positioning incorrect

**Solution:**
- Verify the RelativeView is properly initialized and rendered
- Try different `RelativePosition` values
- Check that the reference view is visible and has layout bounds

### Issue: Popup appears off-screen

**Solution:**
- Adjust `PopupWidth` and `PopupHeight` values
- Choose a different `RelativePosition`
- Use `Dialog` mode instead of `RelativeDialog` for centered display
