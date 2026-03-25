# Advanced Features and Events in .NET MAUI Text Input Layout

## Table of Contents
- [Password Visibility Toggle](#password-visibility-toggle)
- [PasswordVisibilityToggled Event](#passwordvisibilitytoggled-event)
- [Right-to-Left (RTL) Support](#right-to-left-rtl-support)
- [Supported Input Views](#supported-input-views)
- [Platform-Specific Limitations](#platform-specific-limitations)
- [Troubleshooting](#troubleshooting)
- [How-To Scenarios](#how-to-scenarios)

## Password Visibility Toggle

Enable a toggle icon that allows users to show or hide password characters.

### Enabling Password Toggle

#### XAML

```xml
<inputLayout:SfTextInputLayout Hint="Password"
                               EnablePasswordVisibilityToggle="True">
    <Entry IsPassword="True" Text="1234" />
</inputLayout:SfTextInputLayout>
```

#### C#

```csharp
var inputLayout = new SfTextInputLayout
{
    Hint = "Password",
    EnablePasswordVisibilityToggle = true,
    Content = new Entry { IsPassword = true, Text = "1234" }
};
```

### Visual Behavior

- **Icon Appearance:** An eye icon appears at the trailing edge (inside)
- **Default State:** Password is hidden (dots/asterisks)
- **Tap to Show:** Tapping the eye icon reveals the password text
- **Tap to Hide:** Tapping again hides the password

### Requirements

**Important:** Password visibility toggle only works with the `Entry` control, not with `Editor` or other input views.

```xml
<!-- ✓ Works -->
<inputLayout:SfTextInputLayout EnablePasswordVisibilityToggle="True">
    <Entry IsPassword="True" />
</inputLayout:SfTextInputLayout>

<!-- ✗ Does NOT work -->
<inputLayout:SfTextInputLayout EnablePasswordVisibilityToggle="True">
    <Editor />
</inputLayout:SfTextInputLayout>
```

### Combining with Container Types

```xml
<!-- Filled Container -->
<inputLayout:SfTextInputLayout Hint="Password"
                               EnablePasswordVisibilityToggle="True"
                               ContainerType="Filled">
    <Entry IsPassword="True" />
</inputLayout:SfTextInputLayout>

<!-- Outlined Container -->
<inputLayout:SfTextInputLayout Hint="Password"
                               EnablePasswordVisibilityToggle="True"
                               ContainerType="Outlined"
                               OutlineCornerRadius="8">
    <Entry IsPassword="True" />
</inputLayout:SfTextInputLayout>
```

### With Helper Text and Validation

```xml
<inputLayout:SfTextInputLayout x:Name="passwordInput"
                               Hint="Password"
                               HelperText="At least 8 characters"
                               ErrorText="Password too short"
                               EnablePasswordVisibilityToggle="True"
                               ContainerType="Outlined">
    <Entry IsPassword="True" TextChanged="OnPasswordChanged" />
</inputLayout:SfTextInputLayout>
```

```csharp
private void OnPasswordChanged(object sender, TextChangedEventArgs e)
{
    bool isValid = e.NewTextValue?.Length >= 8;
    passwordInput.HasError = !isValid && !string.IsNullOrEmpty(e.NewTextValue);
}
```

## PasswordVisibilityToggled Event

Respond to password visibility changes with the **PasswordVisibilityToggled** event.

### Event Handler

#### XAML

```xml
<inputLayout:SfTextInputLayout Hint="Password"
                               EnablePasswordVisibilityToggle="True"
                               PasswordVisibilityToggled="OnPasswordVisibilityToggled">
    <Entry IsPassword="True" Text="1234" />
</inputLayout:SfTextInputLayout>
```

```csharp
private void OnPasswordVisibilityToggled(object sender, PasswordVisibilityToggledEventArgs e)
{
    bool isVisible = e.IsPasswordVisible;
    
    // Log the event
    Debug.WriteLine($"Password visibility: {(isVisible ? "Shown" : "Hidden")}");
    
    // Update analytics
    AnalyticsService.TrackEvent("PasswordToggled", new Dictionary<string, string>
    {
        { "Visible", isVisible.ToString() }
    });
}
```

#### C# Only

```csharp
var inputLayout = new SfTextInputLayout
{
    Hint = "Password",
    EnablePasswordVisibilityToggle = true,
    Content = new Entry { IsPassword = true, Text = "1234" }
};

inputLayout.PasswordVisibilityToggled += (sender, e) =>
{
    bool isPasswordVisible = e.IsPasswordVisible;
    Console.WriteLine($"Password is now {(isPasswordVisible ? "visible" : "hidden")}");
};
```

### Event Arguments

The `PasswordVisibilityToggledEventArgs` provides one property:

- **IsPasswordVisible** (bool) — `true` when password is visible, `false` when hidden

### Use Cases

```csharp
// Track security metrics
private void OnPasswordVisibilityToggled(object sender, PasswordVisibilityToggledEventArgs e)
{
    if (e.IsPasswordVisible)
    {
        // User revealed password - might indicate complexity issues
        SecurityMetrics.IncrementPasswordReveals();
    }
}

// Show warning when password is visible
private void OnPasswordVisibilityToggled(object sender, PasswordVisibilityToggledEventArgs e)
{
    warningLabel.IsVisible = e.IsPasswordVisible;
    warningLabel.Text = "Password is visible";
}

// Custom icon update
private void OnPasswordVisibilityToggled(object sender, PasswordVisibilityToggledEventArgs e)
{
    visibilityIcon.Text = e.IsPasswordVisible ? "🙈" : "👁️";
}
```

## Right-to-Left (RTL) Support

Support languages that read right-to-left (Arabic, Hebrew, etc.).

### Enabling RTL

#### XAML

```xml
<inputLayout:SfTextInputLayout x:Name="inputLayout"
                               FlowDirection="RightToLeft"
                               ContainerType="Outlined"
                               Hint="نام"
                               HelperText="نام درج کریں">
    <Entry />
</inputLayout:SfTextInputLayout>
```

#### C#

```csharp
var inputLayout = new SfTextInputLayout
{
    FlowDirection = FlowDirection.RightToLeft,
    ContainerType = ContainerType.Outlined,
    Hint = "نام",
    HelperText = "نام درج کریں",
    Content = new Entry()
};
```

### RTL Behavior

When `FlowDirection="RightToLeft"`:
- Text aligns to the right
- Hint label appears on the right
- Helper/error text aligns right
- Leading view appears on the right edge
- Trailing view appears on the left edge
- Character counter appears on the left

### Dynamic RTL Based on Culture

```csharp
// Automatically set based on current culture
var culture = CultureInfo.CurrentUICulture;
bool isRtl = culture.TextInfo.IsRightToLeft;

inputLayout.FlowDirection = isRtl ? FlowDirection.RightToLeft : FlowDirection.LeftToRight;
```

### RTL with Icons

```xml
<inputLayout:SfTextInputLayout FlowDirection="RightToLeft"
                               Hint="البريد الإلكتروني"
                               LeadingViewPosition="Inside"
                               ContainerType="Outlined">
    <Entry />
    <inputLayout:SfTextInputLayout.LeadingView>
        <Label Text="📧" FontSize="20" />
    </inputLayout:SfTextInputLayout.LeadingView>
</inputLayout:SfTextInputLayout>
```

**Note:** Leading/trailing semantics remain the same, but visually flip in RTL mode.

## Supported Input Views

SfTextInputLayout works with multiple .NET MAUI input controls.

### Entry (Single-Line)

```xml
<inputLayout:SfTextInputLayout Hint="Name"
                               HelperText="Enter your name"
                               ContainerType="Outlined">
    <Entry />
</inputLayout:SfTextInputLayout>
```

**Use for:** Single-line text input (names, emails, usernames)

### Editor (Multi-Line)

```xml
<inputLayout:SfTextInputLayout Hint="Notes"
                               HelperText="Add additional notes"
                               ContainerType="Outlined">
    <Editor AutoSize="TextChanges" />
</inputLayout:SfTextInputLayout>
```

**Important:** Always set `AutoSize="TextChanges"` for proper height adjustment.

**Use for:** Multi-line text (comments, descriptions, messages)

### SfAutocomplete

```xml
<inputLayout:SfTextInputLayout Hint="Country"
                               ContainerType="Outlined">
    <autocomplete:SfAutocomplete>
        <autocomplete:SfAutocomplete.ItemsSource>
            <x:Array Type="{x:Type x:String}">
                <x:String>United States</x:String>
                <x:String>United Kingdom</x:String>
                <x:String>Canada</x:String>
            </x:Array>
        </autocomplete:SfAutocomplete.ItemsSource>
    </autocomplete:SfAutocomplete>
</inputLayout:SfTextInputLayout>
```

**Supports:** Single and multiple selection modes

**Use for:** Searchable dropdowns with autocomplete functionality

### SfComboBox

```xml
<inputLayout:SfTextInputLayout Hint="Country"
                               ContainerType="Outlined">
    <combobox:SfComboBox>
        <combobox:SfComboBox.ItemsSource>
            <x:Array Type="{x:Type x:String}">
                <x:String>United States</x:String>
                <x:String>United Kingdom</x:String>
                <x:String>Canada</x:String>
            </x:Array>
        </combobox:SfComboBox.ItemsSource>
    </combobox:SfComboBox>
</inputLayout:SfTextInputLayout>
```

**Supports:** Single and multiple selection modes with tokens/delimiters

**Use for:** Dropdown selection with search capability

### SfMaskedEntry

```xml
<inputLayout:SfTextInputLayout Hint="Card Number"
                               HelperText="Required *"
                               ContainerType="Outlined">
    <maskedEntry:SfMaskedEntry MaskType="Simple"
                               Mask="0000 0000 0000 0000" />
</inputLayout:SfTextInputLayout>
```

**Use for:** Formatted input (credit cards, phone numbers, dates)

### SfNumericEntry

```xml
<inputLayout:SfTextInputLayout Hint="Amount"
                               HelperText="Enter the amount"
                               ContainerType="Outlined">
    <numericEntry:SfNumericEntry Value="100"
                                 ShowClearButton="True"
                                 UpDownPlacementMode="Inline" />
</inputLayout:SfTextInputLayout>
```

**Use for:** Numeric input with increment/decrement buttons

### Picker

```xml
<inputLayout:SfTextInputLayout Hint="Fruit"
                               HelperText="Select a fruit"
                               ContainerType="Outlined">
    <Picker SelectedItem="Apple">
        <Picker.ItemsSource>
            <x:Array Type="{x:Type x:String}">
                <x:String>Apple</x:String>
                <x:String>Orange</x:String>
                <x:String>Strawberry</x:String>
            </x:Array>
        </Picker.ItemsSource>
    </Picker>
</inputLayout:SfTextInputLayout>
```

**Platform Note:** Windows does not support Picker as input view.

**Use for:** Selection from a predefined list

### DatePicker

```xml
<inputLayout:SfTextInputLayout Hint="Date of Birth"
                               HelperText="Select birth date"
                               ContainerType="Outlined">
    <DatePicker />
</inputLayout:SfTextInputLayout>
```

**Platform Note:** Windows does not support DatePicker as input view.

**Use for:** Date selection

### TimePicker

```xml
<inputLayout:SfTextInputLayout Hint="Time"
                               HelperText="Select a start time"
                               ContainerType="Outlined">
    <TimePicker />
</inputLayout:SfTextInputLayout>
```

**Platform Note:** Windows does not support TimePicker as input view.

**Use for:** Time selection

## Platform-Specific Limitations

### Windows Platform

The following controls are **not supported** as Content on Windows:
- **Picker** — Use SfComboBox instead
- **DatePicker** — Use DateTimePicker or custom implementation
- **TimePicker** — Use DateTimePicker or custom implementation

**Supported alternatives for Windows:**
```xml
<!-- Instead of Picker, use SfComboBox -->
<inputLayout:SfTextInputLayout Hint="Selection">
    <combobox:SfComboBox ItemsSource="{Binding Items}" />
</inputLayout:SfTextInputLayout>
```

### All Other Platforms

Full support for all input views:
- **iOS** — All controls supported
- **Android** — All controls supported
- **macOS** — All controls supported

## Troubleshooting

### Issue: Hint doesn't animate

**Possible Causes:**
1. `ShowHint="False"` is set
2. Input view not properly set as Content
3. Insufficient space to display floating hint

**Solution:**
```xml
<inputLayout:SfTextInputLayout Hint="Name" ShowHint="True">
    <Entry />
</inputLayout:SfTextInputLayout>
```

### Issue: Password toggle doesn't appear

**Possible Causes:**
1. Using Editor instead of Entry
2. `EnablePasswordVisibilityToggle` not set to `True`
3. Entry's `IsPassword` not set to `True`

**Solution:**
```xml
<inputLayout:SfTextInputLayout EnablePasswordVisibilityToggle="True">
    <Entry IsPassword="True" />
</inputLayout:SfTextInputLayout>
```

### Issue: Helper text overlaps with error text

**Cause:** Both helper and error text trying to display simultaneously

**Solution:** Error text automatically replaces helper text when `HasError="True"`

```csharp
// Error text shows when HasError is true
inputLayout.HasError = true;  // Shows ErrorText, hides HelperText

// Helper text shows when HasError is false
inputLayout.HasError = false; // Shows HelperText, hides ErrorText
```

### Issue: Character counter turns red but doesn't prevent input

**Expected Behavior:** Character counter shows visual feedback but doesn't block input.

**Solution:** Implement validation to prevent exceeding limit:

```csharp
private void OnTextChanged(object sender, TextChangedEventArgs e)
{
    if (e.NewTextValue.Length > maxLength)
    {
        var entry = sender as Entry;
        entry.Text = e.NewTextValue.Substring(0, maxLength);
    }
}
```

### Issue: Outlined border doesn't show

**Possible Causes:**
1. `ContainerType` not set to `Outlined`
2. Stroke color matches background
3. `UnfocusedStrokeThickness` set to 0

**Solution:**
```xml
<inputLayout:SfTextInputLayout ContainerType="Outlined"
                               Stroke="#000000"
                               UnfocusedStrokeThickness="1">
    <Entry />
</inputLayout:SfTextInputLayout>
```

### Issue: Editor doesn't expand with text

**Cause:** `AutoSize` property not set

**Solution:**
```xml
<inputLayout:SfTextInputLayout Hint="Notes">
    <Editor AutoSize="TextChanges" />
</inputLayout:SfTextInputLayout>
```

### Issue: Custom font doesn't apply

**Possible Causes:**
1. Font not registered in `MauiProgram.cs`
2. Incorrect font family name
3. Font file not included in project

**Solution:**

1. Add font to project (e.g., `Resources/Fonts/CustomFont.ttf`)
2. Register in `MauiProgram.cs`:
```csharp
builder.ConfigureFonts(fonts =>
{
    fonts.AddFont("CustomFont.ttf", "CustomFont");
});
```

3. Use in XAML:
```xml
<inputLayout:SfTextInputLayout.HintLabelStyle>
    <inputLayout:LabelStyle FontFamily="CustomFont" FontSize="16" />
</inputLayout:SfTextInputLayout.HintLabelStyle>
```

## How-To Scenarios

### How to Create a Login Form

```xml
<VerticalStackLayout Spacing="20" Padding="30">
    
    <Label Text="Sign In" FontSize="28" FontAttributes="Bold" />
    
    <inputLayout:SfTextInputLayout Hint="Email"
                                   HelperText="Your work or personal email"
                                   ContainerType="Outlined">
        <Entry Keyboard="Email" />
    </inputLayout:SfTextInputLayout>
    
    <inputLayout:SfTextInputLayout Hint="Password"
                                   HelperText="At least 8 characters"
                                   EnablePasswordVisibilityToggle="True"
                                   ContainerType="Outlined">
        <Entry IsPassword="True" />
    </inputLayout:SfTextInputLayout>
    
    <Button Text="Sign In" />
    
</VerticalStackLayout>
```

### How to Implement Real-Time Validation

```xml
<inputLayout:SfTextInputLayout x:Name="emailInput"
                               Hint="Email"
                               HelperText="Enter a valid email"
                               ContainerType="Outlined">
    <Entry Keyboard="Email" TextChanged="OnEmailTextChanged" />
</inputLayout:SfTextInputLayout>
```

```csharp
private void OnEmailTextChanged(object sender, TextChangedEventArgs e)
{
    string email = e.NewTextValue;
    
    if (string.IsNullOrWhiteSpace(email))
    {
        emailInput.HasError = false;
        return;
    }
    
    bool isValid = IsValidEmail(email);
    emailInput.HasError = !isValid;
    emailInput.ErrorText = isValid ? "" : "Please enter a valid email address";
}

private bool IsValidEmail(string email)
{
    try
    {
        var addr = new System.Net.Mail.MailAddress(email);
        return addr.Address == email;
    }
    catch
    {
        return false;
    }
}
```

### How to Create a Search Field with Clear Button

```xml
<inputLayout:SfTextInputLayout x:Name="searchLayout"
                               Hint="Search"
                               LeadingViewPosition="Inside"
                               TrailingViewPosition="Inside"
                               ContainerType="Filled">
    <Entry x:Name="searchEntry" TextChanged="OnSearchTextChanged" />
    
    <inputLayout:SfTextInputLayout.LeadingView>
        <Label Text="🔍" FontSize="18" />
    </inputLayout:SfTextInputLayout.LeadingView>
    
    <inputLayout:SfTextInputLayout.TrailingView>
        <Label x:Name="clearButton" 
               Text="✖️" 
               FontSize="14"
               IsVisible="False">
            <Label.GestureRecognizers>
                <TapGestureRecognizer Tapped="OnClearSearch" />
            </Label.GestureRecognizers>
        </Label>
    </inputLayout:SfTextInputLayout.TrailingView>
</inputLayout:SfTextInputLayout>
```

```csharp
private void OnSearchTextChanged(object sender, TextChangedEventArgs e)
{
    clearButton.IsVisible = !string.IsNullOrEmpty(e.NewTextValue);
    // Perform search
    PerformSearch(e.NewTextValue);
}

private void OnClearSearch(object sender, EventArgs e)
{
    searchEntry.Text = string.Empty;
    searchEntry.Focus();
}
```

### How to Implement Character Counter with Validation

```xml
<inputLayout:SfTextInputLayout x:Name="bioInput"
                               Hint="Bio"
                               CharMaxLength="200"
                               ShowCharCount="True"
                               HelperText="Tell us about yourself (max 200 chars)"
                               ErrorText="Bio must be at least 10 characters"
                               ContainerType="Outlined">
    <Editor AutoSize="TextChanges" TextChanged="OnBioTextChanged" />
</inputLayout:SfTextInputLayout>
```

```csharp
private void OnBioTextChanged(object sender, TextChangedEventArgs e)
{
    int length = e.NewTextValue?.Length ?? 0;
    
    // Show error if text is entered but less than minimum
    bioInput.HasError = length > 0 && length < 10;
}
```

### How to Toggle Between Helper and Error Text

```csharp
// When validation passes
inputLayout.HasError = false;  // Shows HelperText

// When validation fails
inputLayout.HasError = true;   // Shows ErrorText, hides HelperText
inputLayout.ErrorText = "Specific error message";
```

### How to Create a Dynamic Form

```csharp
private void CreateDynamicForm()
{
    var fields = new[]
    {
        new { Hint = "First Name", Type = Keyboard.Text },
        new { Hint = "Last Name", Type = Keyboard.Text },
        new { Hint = "Email", Type = Keyboard.Email },
        new { Hint = "Phone", Type = Keyboard.Telephone }
    };
    
    foreach (var field in fields)
    {
        var inputLayout = new SfTextInputLayout
        {
            Hint = field.Hint,
            ContainerType = ContainerType.Outlined,
            Content = new Entry { Keyboard = field.Type }
        };
        
        formContainer.Children.Add(inputLayout);
    }
}
```

## Performance Tips

1. **Reuse Styles:** Define LabelStyle once and reuse
2. **Avoid Complex Views:** Keep LeadingView/TrailingView simple
3. **Minimize VSM Setters:** Only set properties that change
4. **Lazy Loading:** Load forms on-demand, not upfront
5. **Dispose Properly:** Unsubscribe from events when done

## Best Practices Summary

1. **Always set `AutoSize="TextChanges"`** for Editor
2. **Use `EnablePasswordVisibilityToggle`** instead of custom implementation
3. **Test RTL** if your app supports international markets
4. **Validate on TextChanged** for real-time feedback
5. **Use appropriate input types** (Entry for single-line, Editor for multi-line)
6. **Check platform limitations** before using Picker/DatePicker/TimePicker on Windows
7. **Reserve space for labels** in forms with validation
8. **Provide clear error messages** that guide users to fix issues
