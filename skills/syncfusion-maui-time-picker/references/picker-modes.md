# Picker Modes in .NET MAUI TimePicker

## Table of Contents
- [Overview](#overview)
- [Mode Property](#mode-property)
- [Default Mode](#default-mode)
- [Dialog Mode](#dialog-mode)
- [RelativeDialog Mode](#relativedialog-mode)
- [IsOpen Property](#isopen-property)
- [Relative Positioning](#relative-positioning)
- [Custom Popup Size](#custom-popup-size)
- [Best Practices](#best-practices)

## Overview

The TimePicker supports three display modes that determine how the picker is presented to the user:

1. **Default** - Inline display within the layout
2. **Dialog** - Centered popup overlay
3. **RelativeDialog** - Positioned popup relative to a view or screen edge

The picker mode is controlled by the `Mode` property, which accepts values from the `PickerMode` enumeration.

## Mode Property

**Namespace:** `Syncfusion.Maui.Picker`

**Property:**
```csharp
public PickerMode Mode { get; set; }
```

**Type:** `PickerMode` (enum)

**Values:**
- `PickerMode.Default` - Inline display (default)
- `PickerMode.Dialog` - Centered popup
- `PickerMode.RelativeDialog` - Positioned popup

**XAML:**
```xml
<picker:SfTimePicker Mode="Dialog" />
```

**C#:**
```csharp
timePicker.Mode = PickerMode.Dialog;
```

## Default Mode

The default mode displays the picker inline within the page layout. The picker is always visible and takes up space in the layout.

**Characteristics:**
- Always visible
- Embedded in page layout
- No overlay or backdrop
- Direct interaction

### Example: Default Mode

**XAML:**
```xml
<StackLayout Padding="20">
    <Label Text="Select Time" FontSize="18" FontAttributes="Bold" />
    
    <picker:SfTimePicker x:Name="timePicker"
                         Mode="Default"
                         HeightRequest="280"
                         WidthRequest="300"
                         Format="hh_mm_tt">
        <picker:SfTimePicker.HeaderView>
            <picker:PickerHeaderView Text="Appointment Time" Height="40" />
        </picker:SfTimePicker.HeaderView>
        <picker:SfTimePicker.FooterView>
            <picker:PickerFooterView ShowOkButton="True" Height="40" />
        </picker:SfTimePicker.FooterView>
    </picker:SfTimePicker>
    
    <Button Text="Continue" Margin="0,20,0,0" />
</StackLayout>
```

**C#:**
```csharp
SfTimePicker timePicker = new SfTimePicker()
{
    Mode = PickerMode.Default,
    HeightRequest = 280,
    WidthRequest = 300,
    Format = PickerTimeFormat.hh_mm_tt
};

timePicker.HeaderView = new PickerHeaderView()
{
    Text = "Appointment Time",
    Height = 40
};

timePicker.FooterView = new PickerFooterView()
{
    ShowOkButton = true,
    Height = 40
};

this.Content = timePicker;
```

**Use Cases:**
- Dedicated time selection screens
- Forms where time picker is primary focus
- Always-visible time selection
- When screen space is not a constraint

## Dialog Mode

Dialog mode displays the picker in a centered popup with a semi-transparent backdrop. The picker appears on demand and overlays the content.

**Characteristics:**
- Centered on screen
- Modal overlay (blocks background interaction)
- Shown/hidden programmatically via `IsOpen` property
- Dismissible by tapping outside (closes automatically)

### Basic Dialog Mode

**XAML:**
```xml
<Grid>
    <Button Text="Select Time" 
            x:Name="openPickerButton"
            Clicked="OnOpenPickerClicked"
            VerticalOptions="Center"
            HorizontalOptions="Center" />
    
    <picker:SfTimePicker x:Name="dialogPicker"
                         Mode="Dialog"
                         Format="hh_mm_tt"
                         SelectedTime="09:00:00">
        <picker:SfTimePicker.HeaderView>
            <picker:PickerHeaderView Text="Select Time" Height="40" />
        </picker:SfTimePicker.HeaderView>
        <picker:SfTimePicker.FooterView>
            <picker:PickerFooterView ShowOkButton="True" Height="40" />
        </picker:SfTimePicker.FooterView>
    </picker:SfTimePicker>
</Grid>
```

**C#:**
```csharp
private void OnOpenPickerClicked(object sender, EventArgs e)
{
    dialogPicker.IsOpen = true;
}
```

### Complete Dialog Example

**XAML:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:picker="clr-namespace:Syncfusion.Maui.Picker;assembly=Syncfusion.Maui.Picker"
             x:Class="MyApp.DialogPage">
    
    <StackLayout Padding="20" Spacing="15">
        
        <Label Text="Appointment Booking" 
               FontSize="24" 
               FontAttributes="Bold" />
        
        <Frame BorderColor="LightGray" Padding="15">
            <StackLayout Spacing="10">
                <Label Text="Selected Time:" FontAttributes="Bold" />
                <Label x:Name="selectedTimeLabel" 
                       Text="Not selected" 
                       FontSize="18" 
                       TextColor="Blue" />
            </StackLayout>
        </Frame>
        
        <Button Text="Choose Time" 
                Clicked="OnChooseTimeClicked"
                BackgroundColor="#6200EE"
                TextColor="White"
                CornerRadius="8"
                Padding="15" />
        
        <picker:SfTimePicker x:Name="timePicker"
                             Mode="Dialog"
                             Format="hh_mm_tt"
                             SelectionChanged="OnTimeSelectionChanged">
            <picker:SfTimePicker.HeaderView>
                <picker:PickerHeaderView Text="Select Appointment Time" 
                                         Height="50"
                                         Background="#6200EE">
                    <picker:PickerHeaderView.TextStyle>
                        <picker:PickerTextStyle TextColor="White" FontSize="18" />
                    </picker:PickerHeaderView.TextStyle>
                </picker:PickerHeaderView>
            </picker:SfTimePicker.HeaderView>
            <picker:SfTimePicker.FooterView>
                <picker:PickerFooterView ShowOkButton="True" 
                                         Height="50"
                                         OkButtonText="Confirm"
                                         CancelButtonText="Cancel" />
            </picker:SfTimePicker.FooterView>
        </picker:SfTimePicker>
        
    </StackLayout>
    
</ContentPage>
```

**C#:**
```csharp
public partial class DialogPage : ContentPage
{
    public DialogPage()
    {
        InitializeComponent();
    }

    private void OnChooseTimeClicked(object sender, EventArgs e)
    {
        timePicker.IsOpen = true;
    }

    private void OnTimeSelectionChanged(object sender, TimePickerSelectionChangedEventArgs e)
    {
        if (e.NewValue.HasValue)
        {
            selectedTimeLabel.Text = e.NewValue.Value.ToString(@"hh\:mm tt");
        }
    }
}
```

**Use Cases:**
- Space-constrained layouts
- Time selection as secondary action
- Modal time selection workflows
- Mobile-first applications

## RelativeDialog Mode

RelativeDialog mode displays the picker in a popup positioned relative to a specific view or screen edge. This mode provides precise control over picker placement.

**Characteristics:**
- Positioned relative to a view or screen edge
- Can be aligned in 8 different positions
- Modal overlay (blocks background interaction)
- Shown/hidden via `IsOpen` property

### RelativePosition Enumeration

The `RelativePosition` property controls the picker's alignment:

**Available Positions:**
```csharp
public enum PickerRelativePosition
{
    AlignTop,           // Above the relative view, centered
    AlignBottom,        // Below the relative view, centered
    AlignToLeftOf,      // To the left of relative view
    AlignToRightOf,     // To the right of relative view
    AlignTopLeft,       // Top-left corner
    AlignTopRight,      // Top-right corner
    AlignBottomLeft,    // Bottom-left corner
    AlignBottomRight    // Bottom-right corner
}
```

### Basic RelativeDialog Example

**XAML:**
```xml
<Grid>
    <Button Text="Pick Time" 
            x:Name="timeButton"
            Clicked="OnTimeButtonClicked"
            VerticalOptions="Center"
            HorizontalOptions="Center"
            Padding="20,10" />
    
    <picker:SfTimePicker x:Name="relativePicker"
                         Mode="RelativeDialog"
                         RelativePosition="AlignBottom"
                         Format="hh_mm_tt">
        <picker:SfTimePicker.HeaderView>
            <picker:PickerHeaderView Text="Select Time" Height="40" />
        </picker:SfTimePicker.HeaderView>
        <picker:SfTimePicker.FooterView>
            <picker:PickerFooterView ShowOkButton="True" Height="40" />
        </picker:SfTimePicker.FooterView>
    </picker:SfTimePicker>
</Grid>
```

**C#:**
```csharp
private void OnTimeButtonClicked(object sender, EventArgs e)
{
    relativePicker.IsOpen = true;
}
```

## Relative Positioning

### AlignTop - Above the View

Positions the picker above the reference view, horizontally centered.

```xml
<picker:SfTimePicker Mode="RelativeDialog"
                     RelativePosition="AlignTop"
                     RelativeView="{x:Reference button1}" />
```

### AlignBottom - Below the View

Positions the picker below the reference view, horizontally centered.

```xml
<picker:SfTimePicker Mode="RelativeDialog"
                     RelativePosition="AlignBottom"
                     RelativeView="{x:Reference button1}" />
```

### AlignToLeftOf - Left of View

Positions the picker to the left of the reference view.

```xml
<picker:SfTimePicker Mode="RelativeDialog"
                     RelativePosition="AlignToLeftOf"
                     RelativeView="{x:Reference button1}" />
```

### AlignToRightOf - Right of View

Positions the picker to the right of the reference view.

```xml
<picker:SfTimePicker Mode="RelativeDialog"
                     RelativePosition="AlignToRightOf"
                     RelativeView="{x:Reference button1}" />
```

### Corner Alignments

**AlignTopLeft:**
```xml
<picker:SfTimePicker Mode="RelativeDialog"
                     RelativePosition="AlignTopLeft"
                     RelativeView="{x:Reference button1}" />
```

**AlignTopRight:**
```xml
<picker:SfTimePicker Mode="RelativeDialog"
                     RelativePosition="AlignTopRight"
                     RelativeView="{x:Reference button1}" />
```

**AlignBottomLeft:**
```xml
<picker:SfTimePicker Mode="RelativeDialog"
                     RelativePosition="AlignBottomLeft"
                     RelativeView="{x:Reference button1}" />
```

**AlignBottomRight:**
```xml
<picker:SfTimePicker Mode="RelativeDialog"
                     RelativePosition="AlignBottomRight"
                     RelativeView="{x:Reference button1}" />
```

## RelativeView Property

The `RelativeView` property specifies which view the picker should position itself relative to.

**Property:**
```csharp
public View RelativeView { get; set; }
```

**XAML Binding:**
```xml
<Grid>
    <Entry x:Name="timeEntry" 
           Placeholder="Select Time"
           IsReadOnly="True"
           Focused="OnTimeEntryFocused" />
    
    <picker:SfTimePicker Mode="RelativeDialog"
                         RelativePosition="AlignBottom"
                         RelativeView="{x:Reference timeEntry}" />
</Grid>
```

**C# Assignment:**
```csharp
private void OnTimeButtonClicked(object sender, EventArgs e)
{
    relativePicker.RelativeView = timeButton;
    relativePicker.IsOpen = true;
}
```

**Default Behavior:**
If no `RelativeView` is specified, the picker positions relative to the picker control itself.

### Complete RelativeView Example

**XAML:**
```xml
<StackLayout Padding="20" Spacing="20">
    
    <Label Text="Meeting Details" FontSize="20" FontAttributes="Bold" />
    
    <Frame BorderColor="Gray" Padding="15">
        <StackLayout Spacing="10">
            <Label Text="Start Time:" />
            <Button x:Name="startTimeButton"
                    Text="09:00 AM"
                    Clicked="OnStartTimeClicked"
                    HorizontalOptions="Start"
                    BackgroundColor="LightBlue" />
        </StackLayout>
    </Frame>
    
    <Frame BorderColor="Gray" Padding="15">
        <StackLayout Spacing="10">
            <Label Text="End Time:" />
            <Button x:Name="endTimeButton"
                    Text="10:00 AM"
                    Clicked="OnEndTimeClicked"
                    HorizontalOptions="Start"
                    BackgroundColor="LightGreen" />
        </StackLayout>
    </Frame>
    
    <picker:SfTimePicker x:Name="timePicker"
                         Mode="RelativeDialog"
                         RelativePosition="AlignBottomLeft"
                         Format="hh_mm_tt"
                         SelectionChanged="OnTimeChanged">
        <picker:SfTimePicker.HeaderView>
            <picker:PickerHeaderView Text="Select Time" Height="40" />
        </picker:SfTimePicker.HeaderView>
        <picker:SfTimePicker.FooterView>
            <picker:PickerFooterView ShowOkButton="True" Height="40" />
        </picker:SfTimePicker.FooterView>
    </picker:SfTimePicker>
    
</StackLayout>
```

**C#:**
```csharp
private Button activeButton;

private void OnStartTimeClicked(object sender, EventArgs e)
{
    activeButton = startTimeButton;
    timePicker.RelativeView = startTimeButton;
    timePicker.SelectedTime = TimeSpan.Parse("09:00:00");
    timePicker.IsOpen = true;
}

private void OnEndTimeClicked(object sender, EventArgs e)
{
    activeButton = endTimeButton;
    timePicker.RelativeView = endTimeButton;
    timePicker.SelectedTime = TimeSpan.Parse("10:00:00");
    timePicker.IsOpen = true;
}

private void OnTimeChanged(object sender, TimePickerSelectionChangedEventArgs e)
{
    if (e.NewValue.HasValue && activeButton != null)
    {
        activeButton.Text = e.NewValue.Value.ToString(@"hh\:mm tt");
    }
}
```

## IsOpen Property

The `IsOpen` property controls whether the picker dialog is visible (Dialog and RelativeDialog modes only).

**Property:**
```csharp
public bool IsOpen { get; set; }
```

**Default:** `false`

**Behavior:**
- Setting to `true` opens the picker dialog
- Setting to `false` closes the picker dialog
- Automatically set to `false` when user taps outside the dialog
- Only applicable when `Mode` is `Dialog` or `RelativeDialog`

### Programmatic Control

**Open the picker:**
```csharp
timePicker.IsOpen = true;
```

**Close the picker:**
```csharp
timePicker.IsOpen = false;
```

**Toggle the picker:**
```csharp
timePicker.IsOpen = !timePicker.IsOpen;
```

### Example: Toggle Picker

**XAML:**
```xml
<StackLayout Padding="20" Spacing="15">
    <Button Text="Toggle Picker" Clicked="OnToggleClicked" />
    
    <Label x:Name="statusLabel" Text="Picker is closed" />
    
    <picker:SfTimePicker x:Name="togglePicker"
                         Mode="Dialog"
                         Format="hh_mm_tt"
                         Opened="OnPickerOpened"
                         Closed="OnPickerClosed">
        <picker:SfTimePicker.HeaderView>
            <picker:PickerHeaderView Text="Select Time" Height="40" />
        </picker:SfTimePicker.HeaderView>
        <picker:SfTimePicker.FooterView>
            <picker:PickerFooterView ShowOkButton="True" Height="40" />
        </picker:SfTimePicker.FooterView>
    </picker:SfTimePicker>
</StackLayout>
```

**C#:**
```csharp
private void OnToggleClicked(object sender, EventArgs e)
{
    togglePicker.IsOpen = !togglePicker.IsOpen;
}

private void OnPickerOpened(object sender, EventArgs e)
{
    statusLabel.Text = "Picker is open";
}

private void OnPickerClosed(object sender, EventArgs e)
{
    statusLabel.Text = "Picker is closed";
}
```

## Custom Popup Size

You can customize the dialog size using `PopupWidth` and `PopupHeight` properties.

**Properties:**
```csharp
public double PopupWidth { get; set; }
public double PopupHeight { get; set; }
```

**Default:** Auto-sized based on content

### Example: Custom Size

**XAML:**
```xml
<picker:SfTimePicker x:Name="customSizePicker"
                     Mode="Dialog"
                     PopupWidth="350"
                     PopupHeight="450"
                     Format="hh_mm_ss_tt">
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView Text="Select Time" Height="50" />
    </picker:SfTimePicker.HeaderView>
    <picker:SfTimePicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True" Height="50" />
    </picker:SfTimePicker.FooterView>
</picker:SfTimePicker>
```

**C#:**
```csharp
SfTimePicker customSizePicker = new SfTimePicker()
{
    Mode = PickerMode.Dialog,
    PopupWidth = 350,
    PopupHeight = 450,
    Format = PickerTimeFormat.hh_mm_ss_tt
};
```

### Responsive Popup Size

```csharp
// Adjust popup size based on screen dimensions
private void SetResponsivePopupSize()
{
    var screenWidth = DeviceDisplay.MainDisplayInfo.Width / DeviceDisplay.MainDisplayInfo.Density;
    var screenHeight = DeviceDisplay.MainDisplayInfo.Height / DeviceDisplay.MainDisplayInfo.Density;
    
    if (screenWidth < 600) // Phone
    {
        timePicker.PopupWidth = screenWidth * 0.9;
        timePicker.PopupHeight = 400;
    }
    else // Tablet or Desktop
    {
        timePicker.PopupWidth = 400;
        timePicker.PopupHeight = 500;
    }
}
```

## Best Practices

### 1. Choose the Right Mode

**Use Default Mode:**
- Dedicated time selection screens
- When picker is the primary UI element
- Forms with ample space

**Use Dialog Mode:**
- Space-constrained layouts
- Mobile applications
- Secondary time selection
- Multiple pickers on one page

**Use RelativeDialog Mode:**
- Context-sensitive time selection
- Near related UI elements (buttons, entries)
- Dropdown-style interactions
- Precise positioning requirements

### 2. Set RelativeView in Dialog Modes

Always specify `RelativeView` when using `RelativeDialog`:

```csharp
// Good
timePicker.RelativeView = button;
timePicker.IsOpen = true;

// Better - set once in XAML
// <picker:SfTimePicker RelativeView="{x:Reference button}" />
```

### 3. Handle IsOpen State

Track picker state to update UI:

```csharp
timePicker.Opened += (s, e) => 
{
    // Update UI when picker opens
    openButton.IsEnabled = false;
};

timePicker.Closed += (s, e) => 
{
    // Update UI when picker closes
    openButton.IsEnabled = true;
};
```

### 4. Consider Screen Bounds

Ensure RelativeDialog positioning doesn't go off-screen:

```csharp
// Check available space before positioning
private PickerRelativePosition DetermineOptimalPosition(Button button)
{
    var buttonY = button.Y;
    var screenHeight = DeviceDisplay.MainDisplayInfo.Height / DeviceDisplay.MainDisplayInfo.Density;
    
    // If button is in top half, show below
    if (buttonY < screenHeight / 2)
        return PickerRelativePosition.AlignBottom;
    else
        return PickerRelativePosition.AlignTop;
}
```

### 5. Accessibility in Dialog Modes

Provide context for screen readers:

```xml
<Button Text="Select Time"
        AutomationProperties.Name="Open time picker"
        AutomationProperties.HelpText="Opens a dialog to select appointment time"
        Clicked="OnOpenPickerClicked" />
```

## Mode Comparison

| Feature | Default | Dialog | RelativeDialog |
|---------|---------|--------|----------------|
| Always Visible | Yes | No | No |
| Modal Overlay | No | Yes | Yes |
| Space Usage | High | Low | Low |
| Positioning | Static | Centered | Custom |
| IsOpen Property | N/A | Required | Required |
| RelativeView | N/A | N/A | Optional |
| Best For | Forms | Modals | Dropdowns |

## Common Issues

### Issue: Dialog Not Opening
**Solution:** Ensure `IsOpen` is set to `true` and `Mode` is `Dialog` or `RelativeDialog`

### Issue: RelativeDialog Appears at Wrong Position
**Solution:** 
- Set `RelativeView` property to target view
- Choose appropriate `RelativePosition`
- Check view is visible and has layout bounds

### Issue: Picker Dialog Cut Off
**Solution:** 
- Adjust `PopupWidth` and `PopupHeight`
- Choose different `RelativePosition`
- Use `Dialog` mode instead for centered display

### Issue: IsOpen Not Updating
**Solution:** Subscribe to `Opened` and `Closed` events to track actual state

## Summary

The TimePicker's mode system provides flexibility for different UI scenarios:
- **Default mode** for dedicated, always-visible time selection
- **Dialog mode** for space-efficient, on-demand time selection
- **RelativeDialog mode** for context-sensitive, positioned time selection
- **IsOpen property** for programmatic control
- **RelativePosition** for precise alignment
- **Custom popup sizing** for optimal display

Choose the mode that best fits your application's layout and user experience requirements.
