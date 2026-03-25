# Editing Modes in .NET MAUI ComboBox

The ComboBox control supports both editable and non-editable text boxes for selecting items from a data source. This guide covers how to configure and use these modes.

## Editable vs Non-Editable

The `IsEditable` property controls whether users can type in the text box or only select from the dropdown. The default value is `false`.

### Editable ComboBox

In editable mode, the ComboBox allows users to edit the text box and automatically appends the remaining letters to the entered text when it is valid.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    IsEditable="true"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    IsEditable = true,
    ItemsSource = socialMediaViewModel.SocialMedias,
    DisplayMemberPath = "Name",
    TextMemberPath = "Name"
};
```

**Benefits of Editable Mode:**
- Users can type custom values
- Auto-complete suggestions as users type
- Combined dropdown selection and free-text input
- Enables filtering functionality

### Non-Editable ComboBox

Non-editable mode prevents users from editing and instead allows them to only select from the dropdown list.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    IsEditable="false"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    IsEditable = false,
    ItemsSource = socialMediaViewModel.SocialMedias,
    DisplayMemberPath = "Name",
    TextMemberPath = "Name"
};
```

**Benefits of Non-Editable Mode:**
- Restricts user input to predefined options
- Prevents invalid entries
- Simpler user interaction

## Text Property

The `Text` property is used to get or set the user-submitted text in the ComboBox editable mode. The default value is `string.Empty`.

**Get the entered text:**
```csharp
string enteredText = comboBox.Text;
```

**Set the text programmatically:**
```csharp
comboBox.Text = "Facebook";
```

**Data binding:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    IsEditable="true"
                    Text="{Binding SelectedText}"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name" />
```

## Clear Button Visibility

By default, the clear button (X) is displayed in the editor of the ComboBox control, which can be used to clear the entered input. You can control its visibility using the `IsClearButtonVisible` property.

### Hide Clear Button

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    IsEditable="true"
                    IsClearButtonVisible="false"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    IsEditable = true,
    IsClearButtonVisible = false,
    ItemsSource = socialMediaViewModel.SocialMedias,
    DisplayMemberPath = "Name",
    TextMemberPath = "Name"
};
```

**Note:** The `IsClearButtonVisible` property has no effect in non-editable mode.

### Customize Clear Button Color

You can customize the clear button icon color using the `ClearButtonIconColor` property. The default value is `Colors.Black`.

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    IsEditable="true"
                    ClearButtonIconColor="Red"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    IsEditable = true,
    ClearButtonIconColor = Colors.Red,
    ItemsSource = socialMediaViewModel.SocialMedias,
    DisplayMemberPath = "Name",
    TextMemberPath = "Name"
};
```

## Cursor Position

The cursor position in the input view can be obtained or updated using the `CursorPosition` property.

**Set cursor position:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    IsEditable="True"
                    CursorPosition="4"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name" />
```

```csharp
SfComboBox comboBox = new SfComboBox
{
    IsEditable = true,
    CursorPosition = 4,
    ItemsSource = socialMediaViewModel.SocialMedias,
    DisplayMemberPath = "Name",
    TextMemberPath = "Name"
};
```

**Note:** 
- Cursor position support is available for editable mode only
- Two-way binding is not supported in the Android platform

## Keyboard Return Type

The `ReturnType` property specifies the return button (e.g., Next, Done, Go) of the keyboard. It helps manage the flow between multiple input fields by defining what happens when the action button is pressed.

**Available Values:**
- Default
- Done
- Go
- Next
- Search
- Send

**XAML:**
```xml
<editors:SfComboBox x:Name="comboBox"
                    IsEditable="true"
                    ReturnType="Next"
                    ItemsSource="{Binding SocialMedias}"
                    DisplayMemberPath="Name"
                    TextMemberPath="Name" />
```

**C#:**
```csharp
SfComboBox comboBox = new SfComboBox
{
    IsEditable = true,
    ReturnType = ReturnType.Next,
    ItemsSource = socialMediaViewModel.SocialMedias,
    DisplayMemberPath = "Name",
    TextMemberPath = "Name"
};
```

## Best Practices

1. **Use Editable Mode When:**
   - Users need to enter custom values not in the list
   - Implementing search/filter functionality
   - Providing auto-complete suggestions

2. **Use Non-Editable Mode When:**
   - Input must be restricted to predefined options
   - Preventing invalid data entry
   - Simplifying user selection

3. **Clear Button:**
   - Keep visible in editable mode for better UX
   - Hide only when clearing is not needed (e.g., required fields)

4. **Cursor Position:**
   - Set appropriately when pre-filling text
   - Useful for focusing on specific parts of the text

## Common Scenarios

### Required Field (No Clear Button)

```xml
<editors:SfComboBox IsEditable="true"
                    IsClearButtonVisible="false"
                    Placeholder="Required: Select an option"
                    ItemsSource="{Binding Items}" />
```

### Custom Value Entry with Validation

```csharp
comboBox.TextChanged += (s, e) =>
{
    if (!string.IsNullOrEmpty(comboBox.Text))
    {
        // Validate custom input
        if (IsValidInput(comboBox.Text))
        {
            // Process valid input
        }
    }
};
```

### Navigation Between Fields

```xml
<StackLayout Spacing="10">
    <editors:SfComboBox x:Name="firstComboBox"
                        IsEditable="true"
                        ReturnType="Next"
                        Completed="OnFirstCompleted" />
    
    <editors:SfComboBox x:Name="secondComboBox"
                        IsEditable="true"
                        ReturnType="Done"
                        Completed="OnSecondCompleted" />
</StackLayout>
```

```csharp
private void OnFirstCompleted(object sender, EventArgs e)
{
    secondComboBox.Focus();
}

private void OnSecondCompleted(object sender, EventArgs e)
{
    // Submit or move to next screen
}
```

## Related Topics

- [Selection](selection.md) - Managing selected items
- [Filtering](filtering.md) - Enabling search functionality in editable mode
- [UI Customization](ui-customization.md) - Styling the text box and buttons
- [Events and Methods](events-and-methods.md) - Handling text changes and completions
