# Accessibility

This guide covers accessibility features and best practices for the DateTimePicker to ensure it's usable by all users, including those with disabilities.

## Overview

The DateTimePicker is designed with accessibility in mind, supporting screen readers, keyboard navigation, and other assistive technologies. Proper implementation ensures compliance with WCAG (Web Content Accessibility Guidelines) standards.

**Key Accessibility Features:**
- Screen reader support
- Keyboard navigation
- Semantic properties
- AutomationId for UI testing
- Focus management
- High contrast support
- Touch target sizing

## WCAG Compliance

The DateTimePicker supports WCAG 2.1 Level AA compliance when properly configured:

- **Perceivable**: Clear labels, sufficient contrast, text alternatives
- **Operable**: Keyboard navigation, sufficient time, navigable
- **Understandable**: Readable, predictable, input assistance
- **Robust**: Compatible with assistive technologies

## Screen Reader Support

The DateTimePicker works with platform screen readers:
- **iOS**: VoiceOver
- **Android**: TalkBack
- **Windows**: Narrator
- **macOS**: VoiceOver

### Default Behavior

The picker automatically provides screen reader announcements for:
- Selected date and time values
- Column headers (Day, Month, Year, Hour, etc.)
- Button labels (OK, Cancel)
- Value changes as user scrolls

### Custom Labels

No additional configuration is needed for basic screen reader support. The control automatically makes itself accessible.

## Keyboard Navigation

### Dialog Mode Keyboard Support

When using Dialog or RelativeDialog mode, the picker supports standard keyboard navigation:

**Windows/macOS:**
- **Tab**: Move focus between elements (OK, Cancel buttons)
- **Enter/Space**: Activate focused button
- **Escape**: Close dialog (same as Cancel)
- **Arrow Keys**: Navigate through values

**Mobile (with external keyboard):**
- Similar navigation patterns
- Platform-specific keyboard shortcuts

### Example: Keyboard-Friendly Dialog

```xaml
<picker:SfDateTimePicker 
    Mode="Dialog"
    x:Name="picker">
    <picker:SfDateTimePicker.FooterView>
        <picker:PickerFooterView 
            Height="50"
            ShowOkButton="True"
            OkButtonText="Confirm"
            CancelButtonText="Cancel" />
    </picker:SfDateTimePicker.FooterView>
</picker:SfDateTimePicker>
```

## Semantic Properties

Use .NET MAUI semantic properties to enhance accessibility:

### AutomationId

Set `AutomationId` for UI testing and assistive technologies:

```xaml
<picker:SfDateTimePicker 
    x:Name="appointmentPicker"
    AutomationId="AppointmentDateTimePicker" />
```

```csharp
dateTimePicker.AutomationId = "AppointmentDateTimePicker";
```

### SemanticProperties

Add semantic hints for screen readers:

```xaml
<picker:SfDateTimePicker 
    x:Name="picker"
    AutomationId="EventDateTime"
    SemanticProperties.Description="Select event date and time"
    SemanticProperties.Hint="Tap to open date and time picker" />
```

```csharp
SemanticProperties.SetDescription(dateTimePicker, "Select event date and time");
SemanticProperties.SetHint(dateTimePicker, "Tap to open date and time picker");
```

## Focus Management

### Programmatic Focus

Set focus to the picker programmatically:

```csharp
dateTimePicker.Focus();
```

### Focus Order

Control focus order using `TabIndex`:

```xaml
<StackLayout>
    <Entry TabIndex="1" />
    <picker:SfDateTimePicker TabIndex="2" />
    <Button TabIndex="3" Text="Submit" />
</StackLayout>
```

### Logical Focus Flow

Ensure logical tab order:

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    
    <Label Grid.Row="0" Text="Name:" TabIndex="0" />
    <Entry Grid.Row="1" TabIndex="1" />
    
    <Label Grid.Row="2" Text="Appointment:" TabIndex="2" />
    <picker:SfDateTimePicker Grid.Row="3" TabIndex="3" />
    
    <Button Grid.Row="4" Text="Submit" TabIndex="4" />
</Grid>
```

## Touch Target Sizing

Ensure sufficient touch target sizes for accessibility and usability:

### Minimum Touch Target

**WCAG Recommendation**: Minimum 44x44 points

**Implementation:**
```xaml
<picker:SfDateTimePicker 
    HeightRequest="50"
    WidthRequest="200">
    <picker:SfDateTimePicker.FooterView>
        <picker:PickerFooterView Height="50" />
    </picker:SfDateTimePicker.FooterView>
</picker:SfDateTimePicker>
```

### Button Sizing

Ensure Footer buttons meet minimum size:

```xaml
<picker:SfDateTimePicker>
    <picker:SfDateTimePicker.FooterView>
        <picker:PickerFooterView 
            Height="60"
            ShowOkButton="True" />
    </picker:SfDateTimePicker.FooterView>
</picker:SfDateTimePicker>
```

## Color Contrast

Ensure sufficient color contrast for text and interactive elements:

**WCAG AA Requirements:**
- Normal text: 4.5:1 contrast ratio
- Large text (18pt+): 3:1 contrast ratio
- UI components: 3:1 contrast ratio

### High Contrast Example

```xaml
<picker:SfDateTimePicker 
    BackgroundColor="White"
    TextColor="Black"
    SelectedTextColor="DarkBlue">
    
    <picker:SfDateTimePicker.HeaderView>
        <picker:DateTimePickerHeaderView 
            Background="Navy"
            TextColor="White" />
    </picker:SfDateTimePicker.HeaderView>
    
    <picker:SfDateTimePicker.SelectionView>
        <picker:PickerSelectionView 
            Background="LightBlue"
            Stroke="DarkBlue"
            StrokeThickness="2" />
    </picker:SfDateTimePicker.SelectionView>
    
</picker:SfDateTimePicker>
```

### Test Contrast

Use tools like:
- WebAIM Contrast Checker
- Chrome DevTools Accessibility Inspector
- Accessibility Insights

## Accessible Label Patterns

### Pattern 1: Form Field with Label

```xaml
<StackLayout>
    <Label 
        Text="Appointment Date:"
        FontSize="16"
        FontAttributes="Bold" />
    
    <picker:SfDateTimePicker 
        x:Name="appointmentPicker"
        AutomationId="AppointmentDatePicker"
        SemanticProperties.Description="Select your appointment date and time" />
</StackLayout>
```

### Pattern 2: Inline Description

```xaml
<StackLayout>
    <Label Text="Event Start Time" />
    <Label 
        Text="Choose when your event begins"
        FontSize="12"
        TextColor="Gray" />
    
    <picker:SfDateTimePicker 
        AutomationId="EventStartTime"
        SemanticProperties.Hint="24-hour format" />
</StackLayout>
```

### Pattern 3: Error State with Accessible Feedback

```xaml
<StackLayout>
    <Label Text="Birth Date *" />
    
    <picker:SfDateTimePicker 
        x:Name="birthDatePicker"
        AutomationId="BirthDatePicker"
        SemanticProperties.Description="Select your date of birth" />
    
    <Label 
        x:Name="errorLabel"
        Text="Please select a valid birth date"
        TextColor="Red"
        IsVisible="False"
        SemanticProperties.Description="Error: Please select a valid birth date" />
</StackLayout>
```

## Testing Accessibility

### Manual Testing

1. **Screen Reader Test**: Enable screen reader and navigate through picker
2. **Keyboard Navigation**: Navigate using only keyboard (Tab, Enter, Escape, Arrows)
3. **Color Contrast**: Use contrast checker tools
4. **Touch Targets**: Verify minimum 44x44 point size
5. **Zoom**: Test with 200% zoom enabled

### Automated Testing

Use .NET MAUI UI testing frameworks:

```csharp
// Example with Appium or similar
[Test]
public void DateTimePicker_Should_BeAccessible()
{
    // Find picker by AutomationId
    var picker = app.WaitForElement("AppointmentDateTimePicker");
    
    // Verify it's enabled
    Assert.IsTrue(picker.Enabled);
    
    // Verify it has accessibility label
    Assert.IsNotNull(picker.Label);
    
    // Tap and verify dialog opens
    picker.Tap();
    var dialog = app.WaitForElement("DateTimePickerDialog");
    Assert.IsNotNull(dialog);
}
```

## Platform-Specific Considerations

### iOS VoiceOver

```csharp
// iOS-specific accessibility traits (if needed)
#if IOS
dateTimePicker.On<iOS>().SetAccessibilityTraits(AccessibilityTraits.Button);
#endif
```

### Android TalkBack

```csharp
// Android-specific content description
#if ANDROID
SemanticProperties.SetDescription(dateTimePicker, 
    "Date and time picker - Double tap to open");
#endif
```

### Windows Narrator

```csharp
// Windows-specific automation properties
#if WINDOWS
dateTimePicker.AutomationProperties.SetName("Appointment date and time picker");
dateTimePicker.AutomationProperties.SetHelpText("Opens a dialog to select date and time");
#endif
```

## Best Practices

1. **Always Set AutomationId**: Essential for UI testing and automation
2. **Provide Descriptions**: Use SemanticProperties for context
3. **Logical Labels**: Use descriptive, meaningful labels
4. **Sufficient Contrast**: Follow WCAG AA standards minimum
5. **Touch Target Size**: Minimum 44x44 points
6. **Keyboard Navigation**: Ensure full keyboard accessibility
7. **Error Messages**: Make errors accessible to screen readers
8. **Focus Indicators**: Ensure visible focus states
9. **Test with Real Users**: Test with actual assistive technology users
10. **Platform Guidelines**: Follow iOS HIG and Material Design guidelines

## Complete Accessible Example

```xaml
<ContentPage Title="Accessible Appointment Booking">
    <ScrollView>
        <StackLayout Padding="20" Spacing="15">
            
            <!-- Patient Name -->
            <Label 
                Text="Patient Name"
                FontSize="16"
                FontAttributes="Bold" />
            <Entry 
                x:Name="nameEntry"
                Placeholder="Enter your name"
                AutomationId="PatientNameEntry"
                TabIndex="1" />
            
            <!-- Appointment Date & Time -->
            <Label 
                Text="Appointment Date and Time"
                FontSize="16"
                FontAttributes="Bold"
                Margin="0,10,0,0" />
            
            <Label 
                Text="Select your preferred appointment slot"
                FontSize="12"
                TextColor="Gray" />
            
            <Button 
                Text="Select Date & Time"
                Clicked="OnShowPicker"
                AutomationId="ShowDateTimePickerButton"
                TabIndex="2"
                HeightRequest="50" />
            
            <picker:SfDateTimePicker 
                x:Name="appointmentPicker"
                Mode="Dialog"
                AutomationId="AppointmentDateTimePicker"
                SemanticProperties.Description="Select appointment date and time"
                SemanticProperties.Hint="Opens a dialog to pick date and time"
                MinimumDate="{x:Static sys:DateTime.Now}"
                DateFormat="dd_MMM_yyyy"
                TimeFormat="h_mm_tt"
                SelectionChanged="OnAppointmentSelected">
                
                <picker:SfDateTimePicker.HeaderView>
                    <picker:DateTimePickerHeaderView 
                        Height="60"
                        Background="#2196F3"
                        TextColor="White"
                        DividerColor="White" />
                </picker:SfDateTimePicker.HeaderView>
                
                <picker:SfDateTimePicker.FooterView>
                    <picker:PickerFooterView 
                        Height="60"
                        ShowOkButton="True"
                        OkButtonText="Confirm"
                        CancelButtonText="Cancel"
                        Background="#F5F5F5" />
                </picker:SfDateTimePicker.FooterView>
                
            </picker:SfDateTimePicker>
            
            <!-- Selected Date Display -->
            <Label 
                x:Name="selectedDateLabel"
                Text="No appointment selected"
                FontSize="14"
                SemanticProperties.Description="Currently selected appointment time" />
            
            <!-- Error Message -->
            <Label 
                x:Name="errorLabel"
                Text=""
                TextColor="Red"
                IsVisible="False"
                SemanticProperties.Description="Error message" />
            
            <!-- Submit Button -->
            <Button 
                Text="Book Appointment"
                Command="{Binding BookCommand}"
                AutomationId="BookAppointmentButton"
                TabIndex="3"
                HeightRequest="50"
                Margin="0,20,0,0" />
                
        </StackLayout>
    </ScrollView>
</ContentPage>
```

## Accessibility Checklist

Before shipping, verify:

- [ ] All interactive elements have AutomationId
- [ ] Semantic descriptions provided for complex controls
- [ ] Color contrast meets WCAG AA (4.5:1 for text, 3:1 for UI)
- [ ] Touch targets are at least 44x44 points
- [ ] Full keyboard navigation works
- [ ] Screen reader announces all important information
- [ ] Focus indicators are clearly visible
- [ ] Error messages are accessible
- [ ] Tested with actual screen readers (VoiceOver, TalkBack, Narrator)
- [ ] Logical tab order implemented
- [ ] Works at 200% zoom/text scaling
- [ ] No reliance on color alone for information

Following these guidelines ensures your DateTimePicker is accessible to all users, regardless of ability.
