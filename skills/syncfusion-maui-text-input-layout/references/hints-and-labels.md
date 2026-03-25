# Hints and Assistive Labels in .NET MAUI Text Input Layout

## Table of Contents
- [Hint Label (Floating Label)](#hint-label-floating-label)
- [Fixed Hint Position](#fixed-hint-position)
- [Helper Text](#helper-text)
- [Error Messages](#error-messages)
- [Character Counter](#character-counter)
- [Reserved Space for Labels](#reserved-space-for-labels)
- [Label Styling](#label-styling)
- [Best Practices](#best-practices)

## Hint Label (Floating Label)

The hint label is a floating label that provides context for the input field. It animates between two positions based on the input state.

### Basic Hint

#### XAML

```xml
<inputLayout:SfTextInputLayout Hint="Full Name">
    <Entry />
</inputLayout:SfTextInputLayout>
```

#### C#

```csharp
var inputLayout = new SfTextInputLayout
{
    Hint = "Full Name",
    Content = new Entry()
};
```

### Behavior

- **Unfocused + Empty:** Hint appears inside the input field
- **Focused or Has Text:** Hint floats to the top position
- **Animation:** Smooth transition between positions

### Showing/Hiding Hint

Control hint visibility with **ShowHint** property:

```xml
<inputLayout:SfTextInputLayout Hint="Name"
                               ShowHint="False">
    <Entry Placeholder="Enter your name" />
</inputLayout:SfTextInputLayout>
```

```csharp
var inputLayout = new SfTextInputLayout
{
    Hint = "Name",
    ShowHint = false,  // Hide hint label
    Content = new Entry { Placeholder = "Enter your name" }
};
```

**Default:** `ShowHint="True"`

## Fixed Hint Position

Keep the hint always at the top position, even when the input is unfocused and empty.

### Using IsHintAlwaysFloated

#### XAML

```xml
<inputLayout:SfTextInputLayout Hint="Email"
                               IsHintAlwaysFloated="True"
                               ContainerType="Outlined">
    <Entry />
</inputLayout:SfTextInputLayout>
```

#### C#

```csharp
var inputLayout = new SfTextInputLayout
{
    Hint = "Email",
    IsHintAlwaysFloated = true,
    ContainerType = ContainerType.Outlined,
    Content = new Entry()
};
```

### When to Use

- **Pre-filled forms:** When inputs have default values
- **Placeholder text:** When you want both hint and placeholder visible
- **Consistency:** When you want all fields to look the same
- **Accessibility:** When screen readers need constant label presence

### Examples by Container Type

#### Filled

```xml
<inputLayout:SfTextInputLayout Hint="Username"
                               IsHintAlwaysFloated="True"
                               ContainerType="Filled">
    <Entry Placeholder="john.doe" />
</inputLayout:SfTextInputLayout>
```

#### Outlined

```xml
<inputLayout:SfTextInputLayout Hint="Password"
                               IsHintAlwaysFloated="True"
                               ContainerType="Outlined">
    <Entry IsPassword="True" />
</inputLayout:SfTextInputLayout>
```

#### None

```xml
<inputLayout:SfTextInputLayout Hint="Phone"
                               IsHintAlwaysFloated="True"
                               ContainerType="None">
    <Entry Keyboard="Telephone" />
</inputLayout:SfTextInputLayout>
```

## Helper Text

Helper text provides additional guidance or context about the expected input.

### Basic Helper Text

#### XAML

```xml
<inputLayout:SfTextInputLayout Hint="Email"
                               HelperText="We'll never share your email">
    <Entry Keyboard="Email" />
</inputLayout:SfTextInputLayout>
```

#### C#

```csharp
var inputLayout = new SfTextInputLayout
{
    Hint = "Email",
    HelperText = "We'll never share your email",
    Content = new Entry { Keyboard = Keyboard.Email }
};
```

### Showing/Hiding Helper Text

```xml
<inputLayout:SfTextInputLayout Hint="Name"
                               HelperText="Enter your full name"
                               ShowHelperText="False">
    <Entry />
</inputLayout:SfTextInputLayout>
```

```csharp
var inputLayout = new SfTextInputLayout
{
    Hint = "Name",
    HelperText = "Enter your full name",
    ShowHelperText = false,  // Hide helper text
    Content = new Entry()
};
```

**Default:** `ShowHelperText="True"`

### Use Cases for Helper Text

```xml
<VerticalStackLayout Spacing="20" Padding="20">
    
    <!-- Format hint -->
    <inputLayout:SfTextInputLayout Hint="Phone Number"
                                   HelperText="Format: (555) 123-4567">
        <Entry Keyboard="Telephone" />
    </inputLayout:SfTextInputLayout>
    
    <!-- Privacy note -->
    <inputLayout:SfTextInputLayout Hint="Email"
                                   HelperText="Your email is kept private">
        <Entry Keyboard="Email" />
    </inputLayout:SfTextInputLayout>
    
    <!-- Length guidance -->
    <inputLayout:SfTextInputLayout Hint="Username"
                                   HelperText="Must be 3-20 characters">
        <Entry />
    </inputLayout:SfTextInputLayout>
    
    <!-- Optional field -->
    <inputLayout:SfTextInputLayout Hint="Middle Name"
                                   HelperText="Optional">
        <Entry />
    </inputLayout:SfTextInputLayout>
    
</VerticalStackLayout>
```

## Error Messages

Display error messages when validation fails.

### Basic Error Message

#### XAML

```xml
<inputLayout:SfTextInputLayout Hint="Email"
                               ErrorText="Invalid email format"
                               HasError="True"
                               ContainerType="Outlined">
    <Entry Text="notanemail" />
</inputLayout:SfTextInputLayout>
```

#### C#

```csharp
var inputLayout = new SfTextInputLayout
{
    Hint = "Email",
    ErrorText = "Invalid email format",
    HasError = true,
    ContainerType = ContainerType.Outlined,
    Content = new Entry { Text = "notanemail" }
};
```

### Dynamic Validation

```csharp
// In your code-behind or ViewModel
private void OnEmailTextChanged(object sender, TextChangedEventArgs e)
{
    bool isValid = IsValidEmail(e.NewTextValue);
    
    emailInputLayout.HasError = !isValid;
    emailInputLayout.ErrorText = isValid 
        ? string.Empty 
        : "Please enter a valid email address";
}

private bool IsValidEmail(string email)
{
    if (string.IsNullOrWhiteSpace(email))
        return false;
    
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

### Error State Visual Changes

When `HasError="True"`:
- Error text replaces helper text
- Stroke/border color changes to error color (typically red)
- Hint label color changes to error color
- Character counter (if shown) turns red

### Switching Between Helper and Error

```xml
<inputLayout:SfTextInputLayout x:Name="passwordInput"
                               Hint="Password"
                               HelperText="At least 8 characters"
                               ErrorText="Password too short"
                               HasError="False">
    <Entry IsPassword="True" 
           TextChanged="OnPasswordChanged" />
</inputLayout:SfTextInputLayout>
```

```csharp
private void OnPasswordChanged(object sender, TextChangedEventArgs e)
{
    passwordInput.HasError = e.NewTextValue.Length < 8 && e.NewTextValue.Length > 0;
    // When HasError is True, ErrorText is shown
    // When HasError is False, HelperText is shown
}
```

## Character Counter

Display a character count and enforce maximum length limits.

### Basic Character Counter

#### XAML

```xml
<inputLayout:SfTextInputLayout Hint="Bio"
                               CharMaxLength="200"
                               ShowCharCount="True"
                               HelperText="Tell us about yourself">
    <Editor AutoSize="TextChanges" />
</inputLayout:SfTextInputLayout>
```

#### C#

```csharp
var inputLayout = new SfTextInputLayout
{
    Hint = "Bio",
    CharMaxLength = 200,
    ShowCharCount = true,
    HelperText = "Tell us about yourself",
    Content = new Editor { AutoSize = EditorAutoSizeOption.TextChanges }
};
```

### Visual Behavior

- Counter displays as: **current / max** (e.g., "0 / 200")
- Counter appears in the bottom-right corner
- When limit is reached, counter turns red
- Border/stroke also turns red when limit exceeded

### Example with Password

```xml
<inputLayout:SfTextInputLayout Hint="Password"
                               CharMaxLength="20"
                               ShowCharCount="True"
                               HelperText="Enter 8 to 20 characters"
                               ContainerType="Outlined">
    <Entry IsPassword="True" />
</inputLayout:SfTextInputLayout>
```

### Character Counter with Validation

```xml
<inputLayout:SfTextInputLayout x:Name="usernameInput"
                               Hint="Username"
                               CharMaxLength="15"
                               ShowCharCount="True"
                               HelperText="3 to 15 characters"
                               ErrorText="Username must be at least 3 characters">
    <Entry TextChanged="OnUsernameChanged" />
</inputLayout:SfTextInputLayout>
```

```csharp
private void OnUsernameChanged(object sender, TextChangedEventArgs e)
{
    int length = e.NewTextValue?.Length ?? 0;
    usernameInput.HasError = length > 0 && length < 3;
}
```

### Showing/Hiding Character Counter

```csharp
// Toggle character counter visibility
inputLayout.ShowCharCount = true;  // Show counter
inputLayout.ShowCharCount = false; // Hide counter

// Change max length dynamically
inputLayout.CharMaxLength = 500;
```

## Reserved Space for Labels

Control whether space is always reserved for assistive labels (helper text, error text, character counter).

### Default Behavior (Reserved Space)

By default, space is reserved below the input:

```xml
<inputLayout:SfTextInputLayout Hint="Name"
                               ReserveSpaceForAssistiveLabels="True">
    <Entry />
</inputLayout:SfTextInputLayout>
```

**Advantage:** Layout doesn't shift when error/helper text appears

### Removing Reserved Space

```xml
<inputLayout:SfTextInputLayout Hint="Name"
                               ReserveSpaceForAssistiveLabels="False"
                               ContainerType="Outlined">
    <Entry />
</inputLayout:SfTextInputLayout>
```

```csharp
var inputLayout = new SfTextInputLayout
{
    Hint = "Name",
    ReserveSpaceForAssistiveLabels = false,
    ContainerType = ContainerType.Outlined,
    Content = new Entry()
};
```

**Effect:** Reduces vertical space, but layout shifts when text appears

### When to Use

**Reserve Space (True) when:**
- Forms with frequent validation feedback
- You want stable, predictable layouts
- Error messages appear/disappear frequently
- Professional forms with consistent spacing

**Don't Reserve Space (False) when:**
- Space is limited (mobile, compact layouts)
- Errors are rare
- Helper/error text is not used
- You want minimal vertical footprint

## Label Styling

Customize font properties for hint, helper, and error labels.

### Hint Label Style

```xml
<inputLayout:SfTextInputLayout Hint="Name"
                               ContainerType="Outlined">
    <inputLayout:SfTextInputLayout.HintLabelStyle>
        <inputLayout:LabelStyle FontSize="16" 
                               FontFamily="Roboto-Medium"
                               FontAttributes="Bold" />
    </inputLayout:SfTextInputLayout.HintLabelStyle>
    <Entry />
</inputLayout:SfTextInputLayout>
```

```csharp
var inputLayout = new SfTextInputLayout
{
    Hint = "Name",
    ContainerType = ContainerType.Outlined,
    HintLabelStyle = new LabelStyle
    {
        FontSize = 16,
        FontFamily = "Roboto-Medium",
        FontAttributes = FontAttributes.Bold
    },
    Content = new Entry()
};
```

### Helper Text Style

```xml
<inputLayout:SfTextInputLayout Hint="Email"
                               HelperText="Enter your email">
    <inputLayout:SfTextInputLayout.HelperLabelStyle>
        <inputLayout:LabelStyle FontSize="12" 
                               FontFamily="Roboto-Regular"
                               TextColor="#666666" />
    </inputLayout:SfTextInputLayout.HelperLabelStyle>
    <Entry />
</inputLayout:SfTextInputLayout>
```

```csharp
var inputLayout = new SfTextInputLayout
{
    Hint = "Email",
    HelperText = "Enter your email",
    HelperLabelStyle = new LabelStyle
    {
        FontSize = 12,
        FontFamily = "Roboto-Regular",
        TextColor = Color.FromArgb("#666666")
    },
    Content = new Entry()
};
```

### Error Text Style

```xml
<inputLayout:SfTextInputLayout Hint="Password"
                               ErrorText="Password too weak"
                               HasError="True">
    <inputLayout:SfTextInputLayout.ErrorLabelStyle>
        <inputLayout:LabelStyle FontSize="12" 
                               FontFamily="Roboto-Medium"
                               FontAttributes="Bold"
                               TextColor="#D32F2F" />
    </inputLayout:SfTextInputLayout.ErrorLabelStyle>
    <Entry IsPassword="True" />
</inputLayout:SfTextInputLayout>
```

```csharp
var inputLayout = new SfTextInputLayout
{
    Hint = "Password",
    ErrorText = "Password too weak",
    HasError = true,
    ErrorLabelStyle = new LabelStyle
    {
        FontSize = 12,
        FontFamily = "Roboto-Medium",
        FontAttributes = FontAttributes.Bold,
        TextColor = Color.FromArgb("#D32F2F")
    },
    Content = new Entry { IsPassword = true }
};
```

### Styling All Labels

```xml
<inputLayout:SfTextInputLayout Hint="Username"
                               HelperText="3-20 characters"
                               ErrorText="Username already taken"
                               ContainerType="Outlined">
    <inputLayout:SfTextInputLayout.HintLabelStyle>
        <inputLayout:LabelStyle FontSize="16" 
                               FontFamily="Poppins-SemiBold" />
    </inputLayout:SfTextInputLayout.HintLabelStyle>
    
    <inputLayout:SfTextInputLayout.HelperLabelStyle>
        <inputLayout:LabelStyle FontSize="12" 
                               FontFamily="Poppins-Regular"
                               TextColor="#757575" />
    </inputLayout:SfTextInputLayout.HelperLabelStyle>
    
    <inputLayout:SfTextInputLayout.ErrorLabelStyle>
        <inputLayout:LabelStyle FontSize="12" 
                               FontFamily="Poppins-Medium"
                               TextColor="#E53935" />
    </inputLayout:SfTextInputLayout.ErrorLabelStyle>
    
    <Entry />
</inputLayout:SfTextInputLayout>
```

### LabelStyle Properties

- **FontSize** — Size of the text (default: 14)
- **FontFamily** — Font family name (must be registered in MauiProgram.cs)
- **FontAttributes** — None, Bold, Italic, or Bold+Italic
- **TextColor** — Color of the text

**Note:** To use custom fonts, register them in `MauiProgram.cs`:

```csharp
builder.ConfigureFonts(fonts =>
{
    fonts.AddFont("Poppins-Regular.ttf", "Poppins-Regular");
    fonts.AddFont("Poppins-SemiBold.ttf", "Poppins-SemiBold");
});
```

## Best Practices

### Hint Labels

- Keep hints short and descriptive (1-3 words)
- Use sentence case, not title case
- Don't end with punctuation
- Examples: "Email address", "Full name", "Phone number"

### Helper Text

- Provide helpful context, not obvious information
- Keep it concise (1 line ideally)
- Use for format hints, privacy notes, or optional indicators
- Update dynamically if context changes

### Error Messages

- Be specific about what's wrong
- Provide actionable guidance to fix the issue
- Use friendly, non-technical language
- Examples:
  - ✓ "Email must contain an @ symbol"
  - ✗ "Invalid input"

### Character Counters

- Use for inputs with strict length limits
- Show when user is actively typing or approaching limit
- Consider hiding counter when far from limit
- Combine with validation for minimum length

### Reserved Space

- Reserve space in forms with validation
- Don't reserve space in simple, non-validated forms
- Consider mobile screen size when deciding

### Label Styling

- Maintain consistency across your app
- Ensure sufficient contrast for accessibility
- Use system fonts or widely available custom fonts
- Test on multiple devices and screen sizes

## Common Patterns

### Login Form

```xml
<VerticalStackLayout Spacing="20" Padding="30">
    
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
    
</VerticalStackLayout>
```

### Registration Form with Validation

```xml
<VerticalStackLayout Spacing="20" Padding="30">
    
    <inputLayout:SfTextInputLayout x:Name="usernameInput"
                                   Hint="Username"
                                   CharMaxLength="20"
                                   ShowCharCount="True"
                                   HelperText="3-20 characters, letters and numbers only"
                                   ContainerType="Outlined">
        <Entry TextChanged="OnUsernameChanged" />
    </inputLayout:SfTextInputLayout>
    
    <inputLayout:SfTextInputLayout x:Name="emailInput"
                                   Hint="Email"
                                   HelperText="We'll send a verification email"
                                   ContainerType="Outlined">
        <Entry Keyboard="Email" TextChanged="OnEmailChanged" />
    </inputLayout:SfTextInputLayout>
    
    <inputLayout:SfTextInputLayout x:Name="passwordInput"
                                   Hint="Password"
                                   CharMaxLength="50"
                                   ShowCharCount="True"
                                   HelperText="Minimum 8 characters"
                                   EnablePasswordVisibilityToggle="True"
                                   ContainerType="Outlined">
        <Entry IsPassword="True" TextChanged="OnPasswordChanged" />
    </inputLayout:SfTextInputLayout>
    
</VerticalStackLayout>
```
