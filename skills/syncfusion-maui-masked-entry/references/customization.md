# Customization and Styling in .NET MAUI Masked Entry

The Masked Entry control offers extensive customization options to match your application's design and UX requirements.

## Table of Contents
- [Clear Button Customization](#clear-button-customization)
- [Font Customization](#font-customization)
- [Text and Placeholder Styling](#text-and-placeholder-styling)
- [Border and Background](#border-and-background)
- [Input Behavior](#input-behavior)
- [Keyboard Configuration](#keyboard-configuration)
- [Return Key Handling](#return-key-handling)
- [Accessibility](#accessibility)
- [Complete Styling Examples](#complete-styling-examples)

## Clear Button Customization

### ClearButtonVisibility

Controls when the clear button appears:

```csharp
public enum ClearButtonVisibility
{
    Never,         // Never show clear button (default)
    WhileEditing   // Show when control has text and is focused
}
```

**Example:**

```xml
<editors:SfMaskedEntry 
    ClearButtonVisibility="WhileEditing"
    Mask="(000) 000-0000" />
```

```csharp
maskedEntry.ClearButtonVisibility = ClearButtonVisibility.WhileEditing;
```

### ClearButtonColor

Customize the clear button icon color:

```xml
<editors:SfMaskedEntry 
    ClearButtonVisibility="WhileEditing"
    ClearButtonColor="Red" />
```

```csharp
maskedEntry.ClearButtonColor = Colors.Red;
```

### ClearButtonPath

Use custom geometry for the clear button icon:

```xml
<editors:SfMaskedEntry 
    MaskType="Simple"
    Mask="(000) 000-0000"
    ClearButtonVisibility="WhileEditing">
    <editors:SfMaskedEntry.ClearButtonPath>
        <Path Data="M1.70711 0.292893C1.31658 -0.097631 0.683417 -0.097631 0.292893 0.292893C-0.097631 0.683417 -0.097631 1.31658 0.292893 1.70711L5.58579 7L0.292893 12.2929C-0.097631 12.6834 -0.097631 13.3166 0.292893 13.7071C0.683417 14.0976 1.31658 14.0976 1.70711 13.7071L7 8.41421L12.2929 13.7071C12.6834 14.0976 13.3166 14.0976 13.7071 13.7071C14.0976 13.3166 14.0976 12.6834 13.7071 12.2929L8.41421 7L13.7071 1.70711C14.0976 1.31658 14.0976 0.683417 13.7071 0.292893C13.3166 -0.097631 12.6834 -0.097631 12.2929 0.292893L7 5.58579L1.70711 0.292893Z" 
              Fill="Red" 
              Stroke="Red"/>
    </editors:SfMaskedEntry.ClearButtonPath>
</editors:SfMaskedEntry>
```

```csharp
private string _customPath = "M1.70711 0.292893C1.31658 -0.097631...";

var converter = new PathGeometryConverter();
var path = new Path
{ 
    Data = (PathGeometry)converter.ConvertFromInvariantString(_customPath),
    Fill = Colors.Red,
    Stroke = Colors.Red
};

maskedEntry.ClearButtonPath = path;
```

## Font Customization

### FontSize

Set the text size (in device-independent units):

```xml
<editors:SfMaskedEntry 
    FontSize="18"
    Mask="(000) 000-0000" />
```

```csharp
maskedEntry.FontSize = 18;
```

**Default:** Platform-specific (typically 14-16)

### FontAttributes

Apply bold or italic styling:

```csharp
public enum FontAttributes
{
    None,   // Regular text (default)
    Bold,   // Bold text
    Italic  // Italic text
}
```

**Example:**

```xml
<editors:SfMaskedEntry 
    FontAttributes="Bold"
    Mask="00/00/0000" />
```

```csharp
maskedEntry.FontAttributes = FontAttributes.Bold;

// Or combine (Bold + Italic)
maskedEntry.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### FontFamily

Use custom fonts:

```xml
<editors:SfMaskedEntry 
    FontFamily="Lobster-Regular"
    Mask="00/00/0000" />
```

```csharp
maskedEntry.FontFamily = "Lobster-Regular";
```

**Note:** Register custom fonts in `MauiProgram.cs`:

```csharp
builder.ConfigureFonts(fonts =>
{
    fonts.AddFont("Lobster-Regular.ttf", "Lobster-Regular");
});
```

### Complete Font Example

```xml
<editors:SfMaskedEntry 
    FontSize="20"
    FontAttributes="Bold"
    FontFamily="OpenSans-Semibold"
    Mask="00/00/0000" />
```

## Text and Placeholder Styling

### TextColor

Color of the entered text:

```xml
<editors:SfMaskedEntry 
    TextColor="Green"
    Mask="(000) 000-0000"
    Value="5551234567" />
```

```csharp
maskedEntry.TextColor = Colors.Green;
```

**Default:** `Colors.Black` (light theme), `Colors.White` (dark theme)

### Placeholder

Text shown when control is empty:

```xml
<editors:SfMaskedEntry 
    Placeholder="Enter phone number"
    Mask="(000) 000-0000" />
```

```csharp
maskedEntry.Placeholder = "Enter phone number";
```

**Default:** `string.Empty`

### PlaceholderColor

Color of the placeholder text:

```xml
<editors:SfMaskedEntry 
    Placeholder="Enter phone number"
    PlaceholderColor="Gray"
    Mask="(000) 000-0000" />
```

```csharp
maskedEntry.PlaceholderColor = Colors.Gray;
```

**Default:** `Colors.Gray`

### Complete Text Styling Example

```xml
<editors:SfMaskedEntry 
    Placeholder="MM/DD/YYYY"
    PlaceholderColor="#9E9E9E"
    TextColor="#212121"
    FontSize="16"
    FontAttributes="Bold"
    Mask="00/00/0000" />
```

## Border and Background

### Stroke

Border color:

```xml
<editors:SfMaskedEntry 
    Stroke="Blue"
    Mask="00/00/0000" />
```

```csharp
maskedEntry.Stroke = Colors.Blue;
```

**Dynamic Border Color Example:**

```csharp
maskedEntry.Focused += (s, e) => maskedEntry.Stroke = Colors.Blue;
maskedEntry.Unfocused += (s, e) => maskedEntry.Stroke = Colors.Gray;

maskedEntry.ValueChanged += (s, e) =>
{
    if (maskedEntry.HasError)
        maskedEntry.Stroke = Colors.Red;
    else if (e.IsMaskCompleted)
        maskedEntry.Stroke = Colors.Green;
};
```

### ShowBorder

Show or hide the border:

```xml
<editors:SfMaskedEntry 
    ShowBorder="False"
    Mask="00/00/0000" />
```

```csharp
maskedEntry.ShowBorder = false;
```

**Default:** `true`

**Use case:** Borderless design, custom containers

### Background

Background color (useful for Liquid Glass effect):

```xml
<editors:SfMaskedEntry 
    Background="Transparent"
    Mask="00/00/0000" />
```

```csharp
maskedEntry.Background = Colors.Transparent;
```

**Note:** Typically used with `SfGlassEffectView` for modern glass effects.

### Size Properties

```xml
<editors:SfMaskedEntry 
    WidthRequest="300"
    HeightRequest="50"
    Mask="00/00/0000" />
```

```csharp
maskedEntry.WidthRequest = 300;
maskedEntry.HeightRequest = 50;
```

## Input Behavior

### CursorPosition

Get or set the cursor position:

```csharp
// Get current position
int position = maskedEntry.CursorPosition;

// Set cursor to specific position
maskedEntry.CursorPosition = 5;
```

**Use case:** Programmatically place cursor, implement custom navigation

### SelectAllOnFocus

Automatically select all text when control gains focus:

```xml
<editors:SfMaskedEntry 
    SelectAllOnFocus="True"
    Mask="00/00/0000"
    Value="12/25/2024" />
```

```csharp
maskedEntry.SelectAllOnFocus = true;
```

**Default:** `false`

**Use case:** Quick edit/replace, copy existing value

**Behavior:** When user taps the control, all text is selected. Next keystroke replaces everything.

### IsReadOnly

Make control non-editable while keeping it focusable:

```xml
<editors:SfMaskedEntry 
    IsReadOnly="True"
    Mask="00/00/0000"
    Value="12/25/2024" />
```

```csharp
maskedEntry.IsReadOnly = true;
```

**Default:** `false`

**Behavior:**
- User can focus and select text
- Cannot modify via typing, cut, or paste
- Useful for display-only scenarios with selectable text

## Keyboard Configuration

### Keyboard

Specify the virtual keyboard type:

```csharp
public enum Keyboard
{
    Default,    // Platform default
    Chat,       // Optimized for chat
    Email,      // @ and . easily accessible
    Numeric,    // Numbers only
    Plain,      // No autocorrect
    Telephone,  // Phone number pad
    Text,       // Full keyboard
    Url         // URL-specific (/ and .com)
}
```

**Example:**

```xml
<editors:SfMaskedEntry 
    Keyboard="Telephone"
    Mask="(000) 000-0000" />
```

```csharp
// Phone number - show numeric keyboard
phoneEntry.Keyboard = Keyboard.Telephone;

// Email - show email keyboard
emailEntry.Keyboard = Keyboard.Email;

// Date - show numeric keyboard
dateEntry.Keyboard = Keyboard.Numeric;
```

**Best Practices:**
- `Telephone` for phone numbers
- `Numeric` for dates, numbers
- `Email` for email addresses
- `Default` for mixed input

## Return Key Handling

### ReturnType

Customize the return key label/icon:

```csharp
public enum ReturnType
{
    Default,    // Platform default
    Done,       // "Done"
    Go,         // "Go"
    Next,       // "Next"
    Search,     // "Search"
    Send        // "Send"
}
```

**Example:**

```xml
<editors:SfMaskedEntry 
    ReturnType="Next"
    Mask="00/00/0000" />
```

```csharp
firstEntry.ReturnType = ReturnType.Next;
lastEntry.ReturnType = ReturnType.Done;
```

### ReturnCommand

Execute a command when return key is pressed:

```xml
<ContentPage.BindingContext>
    <local:MyViewModel />
</ContentPage.BindingContext>

<editors:SfMaskedEntry 
    ReturnCommand="{Binding SubmitCommand}"
    ReturnCommandParameter="PhoneEntry"
    Mask="(000) 000-0000" />
```

**ViewModel:**

```csharp
public class MyViewModel
{
    public ICommand SubmitCommand { get; }
    
    public MyViewModel()
    {
        SubmitCommand = new Command<string>(OnSubmit);
    }
    
    private async void OnSubmit(string parameter)
    {
        await Application.Current.MainPage.DisplayAlert(
            "Submitted",
            $"Field: {parameter}",
            "OK"
        );
    }
}
```

### ReturnCommandParameter

Pass data to the ReturnCommand:

```xml
<editors:SfMaskedEntry 
    ReturnCommand="{Binding NavigateCommand}"
    ReturnCommandParameter="{x:Reference emailEntry}"
    Mask="(000) 000-0000" />

<editors:SfMaskedEntry 
    x:Name="emailEntry"
    Mask="[A-Za-z0-9._%-]+@[A-Za-z0-9]+\.[A-Za-z]{2,3}" />
```

**ViewModel:**

```csharp
public ICommand NavigateCommand => new Command<SfMaskedEntry>(nextEntry =>
{
    nextEntry?.Focus();
});
```

## Accessibility

### AutomationId

Assign unique identifiers for UI automation and testing:

```xml
<editors:SfMaskedEntry 
    AutomationId="PhoneEntry"
    Mask="(000) 000-0000" />
```

```csharp
maskedEntry.AutomationId = "PhoneEntry";
```

**Generated Element IDs:**
- Entry element: `"{AutomationId} Entry"` → `"PhoneEntry Entry"`
- Clear button: `"{AutomationId} Clear Button"` → `"PhoneEntry Clear Button"`

**Use in UI Tests:**

```csharp
// Appium/UITest example
app.WaitForElement("PhoneEntry Entry");
app.EnterText("PhoneEntry Entry", "5551234567");
app.Tap("PhoneEntry Clear Button");
```

**Benefits:**
- Reliable UI test selectors
- Accessibility tool support
- Automated testing frameworks
- Screen reader support

## Complete Styling Examples

### Modern Styled Entry

```xml
<editors:SfMaskedEntry 
    WidthRequest="300"
    HeightRequest="50"
    Mask="(000) 000-0000"
    Placeholder="Phone Number"
    PlaceholderColor="#9E9E9E"
    TextColor="#212121"
    FontSize="16"
    FontAttributes="Bold"
    FontFamily="OpenSans-Semibold"
    Stroke="#2196F3"
    ClearButtonVisibility="WhileEditing"
    ClearButtonColor="#2196F3"
    SelectAllOnFocus="True"
    Keyboard="Telephone"
    ReturnType="Next"
    AutomationId="PhoneNumberEntry" />
```

### Minimal Borderless Entry

```xml
<Frame 
    Padding="10"
    BackgroundColor="White"
    BorderColor="#E0E0E0"
    CornerRadius="8">
    
    <editors:SfMaskedEntry 
        Mask="00/00/0000"
        Placeholder="MM/DD/YYYY"
        ShowBorder="False"
        Background="Transparent"
        TextColor="#212121"
        FontSize="14" />
        
</Frame>
```

### Error State Styling

```csharp
public partial class StyledEntryPage : ContentPage
{
    public StyledEntryPage()
    {
        InitializeComponent();
        ConfigureEntryValidation();
    }
    
    private void ConfigureEntryValidation()
    {
        maskedEntry.ValueChanged += (s, e) =>
        {
            if (maskedEntry.HasError || !e.IsMaskCompleted)
            {
                // Error state
                maskedEntry.Stroke = Color.FromHex("#F44336");
                maskedEntry.TextColor = Color.FromHex("#F44336");
                errorIcon.IsVisible = true;
                errorLabel.IsVisible = true;
            }
            else
            {
                // Success state
                maskedEntry.Stroke = Color.FromHex("#4CAF50");
                maskedEntry.TextColor = Color.FromHex("#212121");
                errorIcon.IsVisible = false;
                errorLabel.IsVisible = false;
            }
        };
        
        maskedEntry.Focused += (s, e) =>
        {
            // Focus state
            maskedEntry.Stroke = Color.FromHex("#2196F3");
            maskedEntry.StrokeThickness = 2;
        };
        
        maskedEntry.Unfocused += (s, e) =>
        {
            // Reset stroke thickness
            maskedEntry.StrokeThickness = 1;
        };
    }
}
```

### Dark Theme Adaptation

```xml
<editors:SfMaskedEntry 
    Mask="00/00/0000"
    Placeholder="Date">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="TextColor" Value="#FFFFFF" />
                    <Setter Property="PlaceholderColor" Value="#9E9E9E" />
                    <Setter Property="Stroke" Value="#757575" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Disabled">
                <VisualState.Setters>
                    <Setter Property="TextColor" Value="#616161" />
                    <Setter Property="Stroke" Value="#424242" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</editors:SfMaskedEntry>
```

### Material Design 3 Style

```xml
<Grid RowDefinitions="Auto,*" Padding="16">
    
    <!-- Floating Label -->
    <Label 
        x:Name="floatingLabel"
        Grid.Row="0"
        Text="Phone Number"
        FontSize="12"
        TextColor="#6200EE"
        TranslationY="20"
        Opacity="0" />
    
    <!-- Entry -->
    <editors:SfMaskedEntry 
        Grid.Row="1"
        x:Name="phoneEntry"
        Mask="(000) 000-0000"
        Placeholder="Phone Number"
        TextColor="#1C1B1F"
        PlaceholderColor="#79747E"
        ShowBorder="False"
        Background="Transparent"
        FontSize="16"
        Focused="OnEntryFocused"
        Unfocused="OnEntryUnfocused" />
    
    <!-- Underline -->
    <BoxView 
        Grid.Row="1"
        VerticalOptions="End"
        HeightRequest="1"
        Color="#79747E" />
        
</Grid>
```

```csharp
private async void OnEntryFocused(object sender, FocusEventArgs e)
{
    // Animate label up
    await Task.WhenAll(
        floatingLabel.TranslateTo(0, 0, 200),
        floatingLabel.FadeTo(1, 200)
    );
}

private async void OnEntryUnfocused(object sender, FocusEventArgs e)
{
    if (string.IsNullOrEmpty(phoneEntry.Value?.ToString()))
    {
        // Animate label down if empty
        await Task.WhenAll(
            floatingLabel.TranslateTo(0, 20, 200),
            floatingLabel.FadeTo(0, 200)
        );
    }
}
```

## Platform-Specific Considerations

### iOS

- **Keyboard Appearance:** Respects system light/dark mode
- **Clear Button:** Native iOS clear button animation
- **Cursor Color:** Inherits from iOS accent color

### Android

- **Keyboard Appearance:** Material Design keyboard
- **IME Actions:** ReturnType affects IME action button
- **Input Method:** Supports various input methods (Google Keyboard, SwiftKey, etc.)

### Windows

- **Virtual Keyboard:** On-screen keyboard for touch devices
- **Desktop:** Full keyboard support with standard shortcuts
- **High DPI:** Automatically scales for different display densities

## Next Steps

- **Advanced Features:** Culture support, passwords, Liquid Glass → [advanced-features.md](advanced-features.md)
