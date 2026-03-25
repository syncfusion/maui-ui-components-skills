# Accessibility in .NET MAUI Picker

The Syncfusion .NET MAUI Picker (SfPicker) is designed with accessibility in mind, providing keyboard navigation and screen reader support for inclusive user experiences.

## Table of Contents
- [Accessibility Features](#accessibility-features)
- [Keyboard Navigation](#keyboard-navigation)
- [Screen Reader Support](#screen-reader-support)
- [Platform-Specific Considerations](#platform-specific-considerations)
- [Testing Accessibility](#testing-accessibility)

## Accessibility Features

The Picker control supports interaction with:
- Header text
- Column header text
- Footer buttons (OK and Cancel)
- Picker items
- Keyboard navigation

## Header Layout Accessibility

The Picker's header text is accessible to screen readers and assistive technologies.

**Implementation:**
```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.HeaderView>
        <picker:PickerHeaderView Text="Select City" Height="40" />
    </picker:SfPicker.HeaderView>
</picker:SfPicker>
```

**Accessibility Behavior:**
- Screen readers announce the header text
- Provides context for the picker's purpose
- Supports localization for multi-language applications

## Column Header Accessibility

Column headers provide descriptive labels for each data column.

**Implementation:**
```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.Columns>
        <picker:PickerColumn HeaderText="Country" 
                            ItemsSource="{Binding Countries}" />
        <picker:PickerColumn HeaderText="City" 
                            ItemsSource="{Binding Cities}" />
    </picker:SfPicker.Columns>
    
    <picker:SfPicker.ColumnHeaderView>
        <picker:PickerColumnHeaderView Height="40" />
    </picker:SfPicker.ColumnHeaderView>
</picker:SfPicker>
```

**Accessibility Behavior:**
- Screen readers announce column headers
- Helps users understand data organization
- Improves navigation in multi-column pickers

## Footer Layout Accessibility

Footer buttons (OK and Cancel) are fully accessible.

**Implementation:**
```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True"
                                 OkButtonText="OK"
                                 CancelButtonText="Cancel"
                                 Height="40" />
    </picker:SfPicker.FooterView>
</picker:SfPicker>
```

**Default Values:**
- **OkButtonText**: "OK"
- **CancelButtonText**: "Cancel"

**Accessibility Behavior:**
- Screen readers announce button labels
- Button text can be customized for clarity
- Supports localization

## Picker Items Accessibility

All items in the picker's `ItemsSource` are accessible to screen readers.

**Implementation:**
```csharp
ObservableCollection<string> cities = new ObservableCollection<string>
{
    "Chennai", "Mumbai", "Delhi", "Kolkata", 
    "Bangalore", "Hyderabad", "Pune"
};

picker.Columns.Add(new PickerColumn
{
    ItemsSource = cities,
    HeaderText = "Select City"
});
```

**Accessibility Behavior:**
- Screen readers announce each item as user scrolls
- Selected item is clearly indicated
- Items are navigable via keyboard

## Keyboard Navigation

The Picker supports comprehensive keyboard navigation for users who cannot or prefer not to use touch/mouse input.

### Keyboard Shortcuts

| Key | Description |
|-----|-------------|
| **Tab** | Focus the picker control |
| **Enter** | Open the selected picker (Dialog/RelativeDialog mode) |
| **Down Arrow** | Move selection down in the current column |
| **Up Arrow** | Move selection up in the current column |
| **Right Arrow / Tab** | Navigate to the next column (multi-column pickers) |
| **Left Arrow / Shift+Tab** | Navigate to the previous column (multi-column pickers) |
| **Esc / Enter** | Close picker and commit selection |

### Example: Full Keyboard Navigation

```xml
<picker:SfPicker x:Name="picker"
                 Mode="Dialog"
                 HeightRequest="300">
    
    <picker:SfPicker.HeaderView>
        <picker:PickerHeaderView Text="Select Options" Height="40" />
    </picker:SfPicker.HeaderView>
    
    <picker:SfPicker.Columns>
        <picker:PickerColumn HeaderText="Category" 
                            ItemsSource="{Binding Categories}" />
        <picker:PickerColumn HeaderText="Item" 
                            ItemsSource="{Binding Items}" />
    </picker:SfPicker.Columns>
    
    <picker:SfPicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True" Height="40" />
    </picker:SfPicker.FooterView>
    
</picker:SfPicker>

<Button Text="Open Picker" 
        Clicked="OpenPicker_Clicked"
        Margin="10"/>
```

**Keyboard Navigation Flow:**
1. **Tab** to focus the "Open Picker" button
2. **Enter** to open the picker dialog
3. **Up/Down Arrows** to select items in the first column
4. **Tab** or **Right Arrow** to move to the second column
5. **Up/Down Arrows** to select items in the second column
6. **Tab** to focus OK button
7. **Enter** to confirm selection and close

## Best Practices for Accessibility

### 1. Provide Meaningful Labels

Always set clear, descriptive text for headers and buttons:

```csharp
picker.HeaderView.Text = "Select Your Country";
picker.FooterView.OkButtonText = "Confirm Selection";
picker.FooterView.CancelButtonText = "Cancel Selection";
```

### 2. Use Column Headers for Multi-Column Pickers

Help users understand the purpose of each column:

```csharp
picker.Columns.Add(new PickerColumn 
{ 
    HeaderText = "Country",
    ItemsSource = countries 
});

picker.Columns.Add(new PickerColumn 
{ 
    HeaderText = "State/Province",
    ItemsSource = states 
});
```

### 3. Ensure Sufficient Color Contrast

```csharp
// Good contrast example
picker.SelectedTextStyle.TextColor = Colors.Black;
picker.SelectionView.Background = Color.FromArgb("#FFFFFF");
picker.Background = Color.FromArgb("#F5F5F5");
```

### 4. Localize All Text

Support multiple languages for global accessibility:

```xml
<picker:SfPicker x:Name="picker">
    <picker:SfPicker.HeaderView>
        <picker:PickerHeaderView Text="{x:Static local:AppResources.SelectCity}" 
                                 Height="40" />
    </picker:SfPicker.HeaderView>
    
    <picker:SfPicker.FooterView>
        <picker:PickerFooterView OkButtonText="{x:Static local:AppResources.OK}"
                                 CancelButtonText="{x:Static local:AppResources.Cancel}"
                                 Height="40" />
    </picker:SfPicker.FooterView>
</picker:SfPicker>
```

### 5. Test with Screen Readers

Test your picker implementation with:
- **iOS**: VoiceOver
- **Android**: TalkBack
- **Windows**: Narrator
- **macOS**: VoiceOver

### 6. Provide Visual Focus Indicators

Ensure keyboard focus is clearly visible:

```csharp
picker.SelectionView.Stroke = Color.FromArgb("#0078D4");
```

### 7. Use Appropriate Font Sizes

Ensure text is readable (minimum 14-16px for body text):

```csharp
picker.SelectedTextStyle.FontSize = 18;
picker.TextStyle.FontSize = 14;
picker.HeaderView.TextStyle.FontSize = 16;
```

## Testing Checklist

- [ ] All interactive elements are keyboard accessible
- [ ] Screen reader announces all text content correctly
- [ ] Tab order follows logical flow
- [ ] Focus indicators are clearly visible
- [ ] Text is resizable without breaking layout
- [ ] Picker works with VoiceOver/TalkBack enabled
- [ ] All text is localized for target languages
- [ ] Footer buttons have clear, descriptive labels
- [ ] Column headers accurately describe data

## Platform-Specific Considerations

### iOS/macOS
- VoiceOver automatically detects and announces picker elements
- Swipe gestures work alongside picker scrolling
- Test with "Speak Screen" feature

### Android
- TalkBack provides audio feedback
- Explore by Touch mode is supported
- Test with "Select to Speak" feature

### Windows
- Narrator provides full support
- Keyboard navigation is primary interaction method
- Test with high contrast themes

## Common Accessibility Issues and Solutions

### Issue: Screen reader not announcing items

**Solution:**
- Ensure items are strings or have meaningful ToString() implementations
- Verify ItemsSource is properly bound
- Test with actual device, not just simulator

### Issue: Poor contrast in custom themes

**Solution:**
- Use contrast checking tools (e.g., WebAIM Contrast Checker)
- Maintain 4.5:1 ratio minimum for text
- Test in different lighting conditions

### Issue: Keyboard navigation not working

**Solution:**
- Verify picker is properly initialized
- Check that Mode is set correctly for dialog pickers
- Ensure picker is part of the visual tree

### Issue: Focus indicator not visible

**Solution:**
- Set visible Stroke color on SelectionView
- Increase StrokeThickness if needed
- Test with keyboard-only navigation

## Additional Resources

- [MAUI Accessibility Documentation](https://learn.microsoft.com/en-us/dotnet/maui/fundamentals/accessibility)
- [iOS Accessibility Guidelines](https://developer.apple.com/accessibility/)
- [Android Accessibility Guidelines](https://developer.android.com/guide/topics/ui/accessibility)

## Example: Fully Accessible Picker

```xml
<picker:SfPicker x:Name="picker"
                 Mode="Dialog"
                 HeightRequest="350"
                 Background="White">
    
    <!-- Clear, descriptive header -->
    <picker:SfPicker.HeaderView>
        <picker:PickerHeaderView Text="Select Your Preferred Language" 
                                 Height="50"
                                 Background="#0078D4">
            <picker:PickerHeaderView.TextStyle>
                <picker:PickerTextStyle TextColor="White" 
                                       FontSize="18" 
                                       FontAttributes="Bold"/>
            </picker:PickerHeaderView.TextStyle>
        </picker:PickerHeaderView>
    </picker:SfPicker.HeaderView>
    
    <!-- Descriptive column header -->
    <picker:SfPicker.Columns>
        <picker:PickerColumn HeaderText="Language" 
                            ItemsSource="{Binding Languages}" />
    </picker:SfPicker.Columns>
    
    <picker:SfPicker.ColumnHeaderView>
        <picker:PickerColumnHeaderView Height="40">
            <picker:PickerColumnHeaderView.TextStyle>
                <picker:PickerTextStyle FontSize="14" 
                                       TextColor="#333333"/>
            </picker:PickerColumnHeaderView.TextStyle>
        </picker:PickerColumnHeaderView>
    </picker:SfPicker.ColumnHeaderView>
    
    <!-- High contrast text styles -->
    <picker:SfPicker.SelectedTextStyle>
        <picker:PickerTextStyle TextColor="Black" 
                               FontSize="18" 
                               FontAttributes="Bold"/>
    </picker:SfPicker.SelectedTextStyle>
    
    <picker:SfPicker.TextStyle>
        <picker:PickerTextStyle TextColor="#666666" 
                               FontSize="16"/>
    </picker:SfPicker.TextStyle>
    
    <!-- Visible focus indicator -->
    <picker:SfPicker.SelectionView>
        <picker:PickerSelectionView Background="#F0F0F0"
                                    Stroke="#0078D4"
                                    CornerRadius="4"/>
    </picker:SfPicker.SelectionView>
    
    <!-- Clear button labels -->
    <picker:SfPicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True"
                                 OkButtonText="Confirm Selection"
                                 CancelButtonText="Cancel"
                                 Height="50">
            <picker:PickerFooterView.TextStyle>
                <picker:PickerTextStyle TextColor="#0078D4" 
                                       FontSize="16" 
                                       FontAttributes="Bold"/>
            </picker:PickerFooterView.TextStyle>
        </picker:PickerFooterView>
    </picker:SfPicker.FooterView>
    
</picker:SfPicker>
```

This implementation follows all accessibility best practices and should provide an excellent experience for all users, including those using assistive technologies.
