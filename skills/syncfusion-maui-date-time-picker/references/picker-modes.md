# Picker Modes and Display

## Table of Contents
- [Overview](#overview)
- [Default Mode](#default-mode)
- [Dialog Mode](#dialog-mode)
- [RelativeDialog Mode](#relativedialog-mode)
- [Mode Selection Guide](#mode-selection-guide)
- [Advanced Usage](#advanced-usage)

## Overview

The DateTimePicker supports three display modes that control how the picker appears to users:

1. **Default** - Inline display within the page layout
2. **Dialog** - Popup modal dialog with overlay
3. **RelativeDialog** - Positioned popup relative to a reference point

**Property**: `Mode` (type: `PickerMode`)  
**Default**: `PickerMode.Default`

## Default Mode

In default mode, the picker is displayed inline within your page layout. It takes up space like any other control.

### When to Use
- Dedicated picker screens
- Forms with ample space
- Desktop/tablet applications
- When the picker is always visible

### Basic Usage

```xaml
<picker:SfDateTimePicker 
    x:Name="picker"
    Mode="Default" />
```

```csharp
var picker = new SfDateTimePicker
{
    Mode = PickerMode.Default
};
```

### Example: Inline Picker in Form

```xaml
<StackLayout Padding="20">
    <Label Text="Select Date and Time:" />
    
    <picker:SfDateTimePicker 
        Mode="Default"
        SelectedDate="{Binding AppointmentDate}" />
    
    <Button Text="Submit" Command="{Binding SubmitCommand}" />
</StackLayout>
```

## Dialog Mode

Dialog mode displays the picker in a popup with a modal overlay. The picker appears centered on the screen and blocks interaction with content underneath.

### When to Use
- Mobile applications (space-constrained)
- After button taps
- Modal selection workflows
- Focused selection experience
- Forms with multiple inputs

### Basic Usage

```xaml
<picker:SfDateTimePicker 
    x:Name="picker"
    Mode="Dialog" />
```

```csharp
var picker = new SfDateTimePicker
{
    Mode = PickerMode.Dialog
};
```

### Opening the Dialog Programmatically

Use the `IsOpen` property to show/hide the dialog:

```xaml
<Grid>
    <Button 
        Text="Select Date & Time" 
        Clicked="OnPickerButtonClicked" />
    
    <picker:SfDateTimePicker 
        x:Name="picker"
        Mode="Dialog"
        SelectedDate="{Binding SelectedDateTime}" />
</Grid>
```

```csharp
private void OnPickerButtonClicked(object sender, EventArgs e)
{
    picker.IsOpen = true;
}
```

**Note**: The `IsOpen` property automatically changes to `false` when you close the dialog by clicking outside of it or pressing the Cancel button.

### Complete Dialog Example

```xaml
<ContentPage xmlns:picker="clr-namespace:Syncfusion.Maui.Picker;assembly=Syncfusion.Maui.Picker">
    <Grid Padding="20">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
        
        <Label 
            Grid.Row="0"
            Text="Selected: Not set"
            x:Name="displayLabel" />
        
        <Button 
            Grid.Row="1"
            Text="Pick DateTime"
            Clicked="ShowPicker"
            Margin="0,20,0,0" />
        
        <picker:SfDateTimePicker 
            Grid.Row="2"
            x:Name="dialogPicker"
            Mode="Dialog"
            SelectionChanged="OnSelectionChanged"
            DateFormat="dd_MMM_yyyy"
            TimeFormat="h_mm_tt" />
    </Grid>
</ContentPage>
```

```csharp
private void ShowPicker(object sender, EventArgs e)
{
    dialogPicker.IsOpen = true;
}

private void OnSelectionChanged(object sender, DateTimePickerSelectionChangedEventArgs e)
{
    if (e.NewValue.HasValue)
    {
        displayLabel.Text = $"Selected: {e.NewValue.Value:f}";
    }
}
```

### Dialog with Custom Footer

```xaml
<picker:SfDateTimePicker 
    Mode="Dialog"
    SelectedDate="{Binding EventDate}">
    <picker:SfDateTimePicker.FooterView>
        <picker:PickerFooterView 
            Height="50"
            ShowOkButton="True"
            OkButtonText="Confirm"
            CancelButtonText="Discard"
            Background="LightBlue" />
    </picker:SfDateTimePicker.FooterView>
</picker:SfDateTimePicker>
```

## RelativeDialog Mode

RelativeDialog mode displays the picker in a popup positioned relative to a specific location. Unlike Dialog mode, it doesn't center the popup but aligns it according to the `RelativePosition` property.

### When to Use
- Dropdown-style pickers
- Context-aware positioning
- Custom UI layouts
- Toolbar/navigation bar pickers
- Space-optimized mobile UIs

### Basic Usage

```xaml
<picker:SfDateTimePicker 
    x:Name="picker"
    Mode="RelativeDialog"
    RelativePosition="AlignBottom" />
```

```csharp
var picker = new SfDateTimePicker
{
    Mode = PickerMode.RelativeDialog,
    RelativePosition = RelativePosition.AlignBottom
};
```

### Relative Positions

The picker can be aligned in **8 different positions**:

| Position | Description |
|----------|-------------|
| `AlignTop` | Above the reference point (default) |
| `AlignBottom` | Below the reference point |
| `AlignToLeftOf` | Left of the reference point |
| `AlignToRightOf` | Right of the reference point |
| `AlignTopLeft` | Top-left corner |
| `AlignTopRight` | Top-right corner |
| `AlignBottomLeft` | Bottom-left corner |
| `AlignBottomRight` | Bottom-right corner |

### Example: Dropdown-Style Picker

```xaml
<Grid Padding="20">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="*" />
    </Grid.RowDefinitions>
    
    <!-- Button at top -->
    <Button 
        Grid.Row="0"
        Text="▼ Select Date & Time"
        Clicked="ShowDropdown"
        HorizontalOptions="Start"
        VerticalOptions="Start" />
    
    <!-- Picker appears below button -->
    <picker:SfDateTimePicker 
        Grid.Row="0"
        x:Name="dropdownPicker"
        Mode="RelativeDialog"
        RelativePosition="AlignBottom"
        SelectedDate="{Binding SelectedDate}" />
    
    <!-- Rest of your content -->
    <StackLayout Grid.Row="1" />
</Grid>
```

```csharp
private void ShowDropdown(object sender, EventArgs e)
{
    dropdownPicker.IsOpen = true;
}
```

### All Position Examples

**Align Top (Default):**
```xaml
<picker:SfDateTimePicker 
    Mode="RelativeDialog"
    RelativePosition="AlignTop" />
```

**Align Bottom:**
```xaml
<picker:SfDateTimePicker 
    Mode="RelativeDialog"
    RelativePosition="AlignBottom" />
```

**Align to Left:**
```xaml
<picker:SfDateTimePicker 
    Mode="RelativeDialog"
    RelativePosition="AlignToLeftOf" />
```

**Align to Right:**
```xaml
<picker:SfDateTimePicker 
    Mode="RelativeDialog"
    RelativePosition="AlignToRightOf" />
```

**Corner Positions:**
```xaml
<!-- Top-left corner -->
<picker:SfDateTimePicker 
    Mode="RelativeDialog"
    RelativePosition="AlignTopLeft" />

<!-- Top-right corner -->
<picker:SfDateTimePicker 
    Mode="RelativeDialog"
    RelativePosition="AlignTopRight" />

<!-- Bottom-left corner -->
<picker:SfDateTimePicker 
    Mode="RelativeDialog"
    RelativePosition="AlignBottomLeft" />

<!-- Bottom-right corner -->
<picker:SfDateTimePicker 
    Mode="RelativeDialog"
    RelativePosition="AlignBottomRight" />
```

### Opening Relative Dialog Programmatically

```csharp
// Set IsOpen to true
relativePicker.IsOpen = true;

// Automatically changes to false when closed
```

**Note**: The `IsOpen` property automatically changes to `false` when you close the dialog by clicking outside of it.

## Mode Selection Guide

### Use Default Mode When:
- You have dedicated space for the picker
- Building tablet/desktop applications
- The picker is the primary focus of the screen
- Users need to see the picker constantly

### Use Dialog Mode When:
- Building mobile applications
- Space is limited
- Picker is triggered by a button
- You need modal behavior
- Clear OK/Cancel actions are important

### Use RelativeDialog Mode When:
- Creating dropdown-style pickers
- You need specific positioning
- Building compact UIs
- Picker should appear near trigger element
- Custom positioning is required

## Advanced Usage

### Switching Modes Dynamically

```csharp
// Switch based on device
private void SetPickerMode()
{
    if (DeviceInfo.Platform == DevicePlatform.Android || 
        DeviceInfo.Platform == DevicePlatform.iOS)
    {
        dateTimePicker.Mode = PickerMode.Dialog;
    }
    else
    {
        dateTimePicker.Mode = PickerMode.Default;
    }
}
```

### Mode with Custom Positioning

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="60" />
        <RowDefinition Height="*" />
    </Grid.RowDefinitions>
    
    <!-- Toolbar -->
    <StackLayout 
        Grid.Row="0"
        Orientation="Horizontal"
        Padding="10"
        BackgroundColor="LightGray">
        
        <Button 
            Text="📅"
            Clicked="ShowDateTimePicker"
            WidthRequest="40" />
    </StackLayout>
    
    <!-- Picker aligned below toolbar -->
    <picker:SfDateTimePicker 
        Grid.Row="0"
        x:Name="toolbarPicker"
        Mode="RelativeDialog"
        RelativePosition="AlignBottom" />
    
    <!-- Main content -->
    <ScrollView Grid.Row="1">
        <!-- Your content -->
    </ScrollView>
</Grid>
```

### Responsive Mode Selection

```xaml
<picker:SfDateTimePicker 
    x:Name="responsivePicker"
    Mode="{OnPlatform Default={x:Static picker:PickerMode.Default},
                      Android={x:Static picker:PickerMode.Dialog},
                      iOS={x:Static picker:PickerMode.Dialog}}" />
```

### Mode with Styling

```xaml
<picker:SfDateTimePicker 
    Mode="Dialog"
    BackgroundColor="White">
    <picker:SfDateTimePicker.HeaderView>
        <picker:DateTimePickerHeaderView 
            Height="50"
            Background="#2196F3"
            TextColor="White" />
    </picker:SfDateTimePicker.HeaderView>
    <picker:SfDateTimePicker.FooterView>
        <picker:PickerFooterView 
            Height="50"
            Background="#2196F3"
            OkButtonText="Select"
            ShowOkButton="True" />
    </picker:SfDateTimePicker.FooterView>
</picker:SfDateTimePicker>
```

## Best Practices

1. **Mobile First**: Use Dialog or RelativeDialog mode for mobile apps
2. **Consistent Experience**: Keep the same mode throughout your app
3. **Button Triggers**: Always provide clear buttons to open Dialog/RelativeDialog pickers
4. **User Feedback**: Show selected value after picker closes
5. **Platform Conventions**: Match platform-specific patterns (iOS/Android dialogs)
6. **Accessibility**: Ensure IsOpen can be triggered via keyboard/screen readers
7. **Testing**: Test RelativePosition on different screen sizes

## Common Patterns

### Pattern 1: Mobile-First Dialog
```csharp
Mode = PickerMode.Dialog;
IsOpen = false; // Open via button
```

### Pattern 2: Dropdown from Toolbar
```csharp
Mode = PickerMode.RelativeDialog;
RelativePosition = RelativePosition.AlignBottom;
```

### Pattern 3: Inline Desktop Picker
```csharp
Mode = PickerMode.Default; // Always visible
```

### Pattern 4: Context Menu Style
```csharp
Mode = PickerMode.RelativeDialog;
RelativePosition = RelativePosition.AlignBottomRight;
```
