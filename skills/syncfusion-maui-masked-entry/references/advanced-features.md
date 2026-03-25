# Advanced Features in .NET MAUI Masked Entry

This guide covers advanced features including culture support, password masking, Liquid Glass effects, and platform-specific considerations.

## Table of Contents
- [Culture Support](#culture-support)
- [Password Features](#password-features)
- [Liquid Glass Effect](#liquid-glass-effect)
- [Performance Optimization](#performance-optimization)
- [Edge Cases and Troubleshooting](#edge-cases-and-troubleshooting)

## Culture Support

The `Culture` property enables localization of currency symbols, date/time separators, decimal separators, and group separators.

### Culture-Specific Characters

| Character | Meaning | Example (en-US) | Example (fr-FR) |
|-----------|---------|-----------------|-----------------|
| **$** | Currency symbol | `$` (dollar) | `€` (euro) |
| **/** | Date separator | `/` | `/` |
| **:** | Time separator | `:` | `:` |
| **.** | Decimal separator | `.` | `,` (comma) |
| **,** | Group separator | `,` (comma) | ` ` (space) |

### Setting Culture

```csharp
using System.Globalization;

// French culture
maskedEntry.Culture = new CultureInfo("fr-FR");

// German culture
maskedEntry.Culture = new CultureInfo("de-DE");

// Japanese culture
maskedEntry.Culture = new CultureInfo("ja-JP");
```

### Currency Example

**US Culture (en-US):**

```xml
<editors:SfMaskedEntry 
    MaskType="Simple"
    Mask="$ 0,000.00"
    Culture="en-US" />
```

```csharp
var currencyEntry = new SfMaskedEntry
{
    MaskType = MaskedEntryMaskType.Simple,
    Mask = "$ 0,000.00",
    Culture = new CultureInfo("en-US")
};

// Input: 1234567
// Display: $ 1,234.56
// $ → $, , → comma, . → period
```

**French Culture (fr-FR):**

```csharp
var currencyEntry = new SfMaskedEntry
{
    MaskType = MaskedEntryMaskType.Simple,
    Mask = "$ 0,000.00",
    Culture = new CultureInfo("fr-FR")
};

// Input: 1234567
// Display: € 1 234,56
// $ → €, , → space, . → comma
```

**German Culture (de-DE):**

```csharp
var currencyEntry = new SfMaskedEntry
{
    MaskType = MaskedEntryMaskType.Simple,
    Mask = "$ 0,000.00",
    Culture = new CultureInfo("de-DE")
};

// Input: 1234567
// Display: € 1.234,56
// $ → €, , → period, . → comma
```

### Date Separator Example

**US Date Format:**

```csharp
var dateEntry = new SfMaskedEntry
{
    MaskType = MaskedEntryMaskType.Simple,
    Mask = "00/00/0000",
    Culture = new CultureInfo("en-US")
};

// Display: 12/25/2024 (MM/DD/YYYY)
```

**European Date Format:**

```csharp
var dateEntry = new SfMaskedEntry
{
    MaskType = MaskedEntryMaskType.Simple,
    Mask = "00/00/0000",
    Culture = new CultureInfo("en-GB")
};

// Display: 25/12/2024 (DD/MM/YYYY)
// Note: Mask doesn't enforce order, just separators
```

### Time Separator Example

```csharp
var timeEntry = new SfMaskedEntry
{
    MaskType = MaskedEntryMaskType.Simple,
    Mask = "00:00:00",
    Culture = new CultureInfo("en-US")
};

// Display: 14:30:25
```

### Decimal and Group Separators

**US Format:**

```csharp
var decimalEntry = new SfMaskedEntry
{
    Mask = "0,000.00",
    Culture = new CultureInfo("en-US")
};

// Input: 1234567
// Display: 1,234.56
```

**European Format:**

```csharp
var decimalEntry = new SfMaskedEntry
{
    Mask = "0,000.00",
    Culture = new CultureInfo("de-DE")
};

// Input: 1234567
// Display: 1.234,56
```

### Dynamic Culture Selection

```csharp
public partial class LocalizedFormPage : ContentPage
{
    private readonly Picker culturePicker;
    private readonly SfMaskedEntry currencyEntry;
    
    public LocalizedFormPage()
    {
        InitializeComponent();
        
        culturePicker.SelectedIndexChanged += OnCultureChanged;
    }
    
    private void OnCultureChanged(object sender, EventArgs e)
    {
        string selectedCulture = culturePicker.SelectedItem as string;
        
        currencyEntry.Culture = new CultureInfo(selectedCulture);
    }
}
```

### Common Culture Codes

| Region | Culture Code | Currency | Decimal | Group |
|--------|-------------|----------|---------|-------|
| **United States** | en-US | $ | . | , |
| **United Kingdom** | en-GB | £ | . | , |
| **France** | fr-FR | € | , | (space) |
| **Germany** | de-DE | € | , | . |
| **Spain** | es-ES | € | , | . |
| **Japan** | ja-JP | ¥ | . | , |
| **China** | zh-CN | ¥ | . | , |
| **India** | hi-IN | ₹ | . | , |
| **Brazil** | pt-BR | R$ | , | . |
| **Canada** | en-CA | $ | . | , |

## Password Features

### PasswordChar

Mask entered characters for password security:

```xml
<editors:SfMaskedEntry 
    MaskType="RegEx"
    Mask="\w+"
    PasswordChar="●"
    Placeholder="Enter password" />
```

```csharp
var passwordEntry = new SfMaskedEntry
{
    MaskType = MaskedEntryMaskType.RegEx,
    Mask = "\\w+",
    PasswordChar = '●'
};

// User types: "MyPass123"
// Display: "●●●●●●●●●"
```

**Common Password Characters:**
- `●` (bullet) - Modern, clear
- `*` (asterisk) - Traditional
- `•` (middle dot) - Subtle
- `█` (block) - High visibility

### PasswordDelayDuration

Show the last typed character briefly before masking:

```xml
<editors:SfMaskedEntry 
    MaskType="RegEx"
    Mask="\w+"
    PasswordChar="●"
    PasswordDelayDuration="1000"
    Placeholder="Enter password" />
```

```csharp

```

**Behavior:**
1. User types a character
2. Character is visible for 1000ms
3. Character is replaced with `●`
4. Process repeats for next character

**Duration Guidelines:**
- **500-800ms**: Quick mask, high security
- **1000-1500ms**: Balanced usability and security
- **2000ms+**: Easier to verify, less secure

### Password with Minimum Length

```csharp
var passwordEntry = new SfMaskedEntry
{
    MaskType = MaskedEntryMaskType.RegEx,
    Mask = "\\w{8,}",  // Minimum 8 characters
    PasswordChar = '●',
    PasswordDelayDuration = 1000
};

passwordEntry.ValueChanged += (s, e) =>
{
    string value = e.NewValue?.ToString();
    
    if (string.IsNullOrEmpty(value) || value.Length < 8)
    {
        strengthLabel.Text = "Password too short";
        strengthLabel.TextColor = Colors.Red;
    }
    else if (value.Length < 12)
    {
        strengthLabel.Text = "Moderate password";
        strengthLabel.TextColor = Colors.Orange;
    }
    else
    {
        strengthLabel.Text = "Strong password";
        strengthLabel.TextColor = Colors.Green;
    }
};
```

### PIN Entry with Password Char

```csharp
var pinEntry = new SfMaskedEntry
{
    MaskType = MaskedEntryMaskType.Simple,
    Mask = "0000",
    PasswordChar = '●',
    PasswordDelayDuration = 500,
    Keyboard = Keyboard.Numeric
};
```

### SSN with Password Masking

```csharp
var ssnEntry = new SfMaskedEntry
{
    MaskType = MaskedEntryMaskType.Simple,
    Mask = "000-00-0000",
    PasswordChar = '●',
    PasswordDelayDuration = 1500,
    ValueMaskFormat = MaskedEntryMaskFormat.ExcludePromptAndLiterals
};
```

## HidePromptOnLeave

Hide prompt characters when the control loses focus for a cleaner appearance:

```xml
<editors:SfMaskedEntry 
    Mask="00/00/0000"
    PromptChar="#"
    HidePromptOnLeave="True"
    Placeholder="Enter date" />
```

```csharp
var dateEntry = new SfMaskedEntry
{
    Mask = "00/00/0000",
    PromptChar = '#',
    HidePromptOnLeave = true
};
```

**Behavior:**
- **When Focused:** `12/##/####` (prompts visible)
- **When Unfocused:** `12/` (prompts hidden)
- **When Focused Again:** `12/##/####` (prompts restored)

**Use Cases:**
- Cleaner UI when field is inactive
- Reduce visual clutter in forms
- Progressive disclosure of input requirements

## Liquid Glass Effect

Create modern, translucent glass-like designs with the Liquid Glass Effect (requires .NET 10+, iOS 26+, or macOS 26+).

### Prerequisites

- .NET 10 or later
- iOS 26+ or macOS 26+
- Syncfusion.Maui.Core package

### Implementation

#### Step 1: Wrap in SfGlassEffectView

```xml
<Grid>
    <!-- Background image -->
    <Image Source="wallpaper.png" Aspect="AspectFill" />
    
    <!-- Glass effect container -->
    <core:SfGlassEffectView
        CornerRadius="20"
        HeightRequest="40"
        EffectType="Regular"
        EnableShadowEffect="True"
        HorizontalOptions="Center"
        VerticalOptions="Center">
        
        <editors:SfMaskedEntry
            WidthRequest="250"
            Background="Transparent"
            ClearButtonVisibility="WhileEditing"
            MaskType="RegEx"
            Mask="[A-Za-z0-9._%-]+@[A-Za-z0-9]+\.[A-Za-z]{2,3}"
            Placeholder="Email"
            PlaceholderColor="White" />
            
    </core:SfGlassEffectView>
</Grid>
```

#### Step 2: Set Background to Transparent

**Critical:** The Masked Entry must have `Background="Transparent"` for the glass effect to work.

```csharp
using Syncfusion.Maui.Core;
using Syncfusion.Maui.Inputs;

var grid = new Grid
{
    BackgroundColor = Colors.Transparent
};

var image = new Image
{
    Source = "wallpaper.png",
    Aspect = Aspect.AspectFill
};
grid.Children.Add(image);

var glassEffect = new SfGlassEffectView
{
    CornerRadius = 20,
    HeightRequest = 40,
    EffectType = LiquidGlassEffectType.Regular,
    EnableShadowEffect = true
};

var maskedEntry = new SfMaskedEntry
{
    WidthRequest = 250,
    Background = Colors.Transparent,  // Essential!
    ClearButtonVisibility = ClearButtonVisibility.WhileEditing,
    MaskType = MaskedEntryMaskType.RegEx,
    Mask = "[A-Za-z0-9._%-]+@[A-Za-z0-9]+\\.[A-Za-z]{2,3}"
};

glassEffect.Content = maskedEntry;
grid.Children.Add(glassEffect);
```

### Glass Effect Types

```csharp
public enum LiquidGlassEffectType
{
    Regular,    // Standard glass effect
    Ultra,      // More pronounced effect
    Thin        // Subtle effect
}
```

### Complete Liquid Glass Example

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:core="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
             xmlns:editors="clr-namespace:Syncfusion.Maui.Inputs;assembly=Syncfusion.Maui.Inputs">
    
    <Grid>
        <!-- Gradient background -->
        <BoxView>
            <BoxView.Background>
                <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
                    <GradientStop Color="#667eea" Offset="0.0" />
                    <GradientStop Color="#764ba2" Offset="1.0" />
                </LinearGradientBrush>
            </BoxView.Background>
        </BoxView>
        
        <!-- Form with glass effect -->
        <VerticalStackLayout 
            Spacing="20"
            HorizontalOptions="Center"
            VerticalOptions="Center">
            
            <!-- Email Entry -->
            <core:SfGlassEffectView
                CornerRadius="12"
                HeightRequest="50"
                WidthRequest="300"
                EffectType="Regular"
                EnableShadowEffect="True">
                
                <editors:SfMaskedEntry
                    Background="Transparent"
                    MaskType="RegEx"
                    Mask="[A-Za-z0-9._%-]+@[A-Za-z0-9]+\.[A-Za-z]{2,3}"
                    Placeholder="Email"
                    PlaceholderColor="White"
                    TextColor="White"
                    ShowBorder="False" />
            </core:SfGlassEffectView>
            
            <!-- Phone Entry -->
            <core:SfGlassEffectView
                CornerRadius="12"
                HeightRequest="50"
                WidthRequest="300"
                EffectType="Regular"
                EnableShadowEffect="True">
                
                <editors:SfMaskedEntry
                    Background="Transparent"
                    MaskType="Simple"
                    Mask="(000) 000-0000"
                    Placeholder="Phone"
                    PlaceholderColor="White"
                    TextColor="White"
                    ShowBorder="False" />
            </core:SfGlassEffectView>
            
        </VerticalStackLayout>
    </Grid>
    
</ContentPage>
```

### Platform Requirements

| Platform | Minimum Version | Support |
|----------|----------------|---------|
| **iOS** | 26 | ✅ Full support |
| **macOS** | 26 | ✅ Full support |
| **Android** | N/A | ❌ Not supported (fallback to standard) |
| **Windows** | N/A | ❌ Not supported (fallback to standard) |

**Graceful Degradation:** On unsupported platforms, the control renders without the glass effect but remains functional.

## Performance Optimization

### Best Practices

1. **Choose Simple over RegEx when possible**
   ```csharp
   // ✅ Faster
   Mask = "(000) 000-0000"
   
   // ❌ Slower (but more flexible)
   Mask = "\\(\\d{3}\\) \\d{3}-\\d{4}"
   ```

2. **Minimize ValidationMode=KeyPress for expensive validation**
   ```csharp
   // Use LostFocus for API calls or heavy validation
   ValidationMode = InputValidationMode.LostFocus
   ```

3. **Debounce ValueChanged events**
   ```csharp
   private CancellationTokenSource _cts;
   
   maskedEntry.ValueChanged += async (s, e) =>
   {
       _cts?.Cancel();
       _cts = new CancellationTokenSource();
       
       await Task.Delay(300, _cts.Token);
       await PerformExpensiveValidation(e.NewValue);
   };
   ```

4. **Unsubscribe from events**
   ```csharp
   protected override void OnDisappearing()
   {
       base.OnDisappearing();
       maskedEntry.ValueChanged -= OnValueChanged;
   }
   ```

## Edge Cases and Troubleshooting

### Issue: Mask Not Applying

**Symptom:** Mask pattern doesn't restrict input

**Causes & Solutions:**

1. **Wrong MaskType**
   ```csharp
   // ❌ Wrong: Using RegEx pattern with Simple type
   MaskType = MaskedEntryMaskType.Simple,
   Mask = "\\d{3}-\\d{4}"
   
   // ✅ Correct
   MaskType = MaskedEntryMaskType.RegEx,
   Mask = "\\d{3}-\\d{4}"
   ```

2. **Invalid Mask Pattern**
   ```csharp
   // ❌ Invalid Simple mask element
   Mask = "XXX-0000"  // X is not a valid element
   
   // ✅ Valid
   Mask = "AAA-0000"  // A = alphanumeric
   ```

### Issue: Value Not Setting

**Symptom:** Setting `Value` property doesn't display

**Solution:** Ensure value matches mask format

```csharp
// ❌ Wrong: Value doesn't match mask
Mask = "(000) 000-0000",
Value = "(555) 123-4567"  // Includes literals

// ✅ Correct: Value without formatting
Mask = "(000) 000-0000",
Value = "5551234567"  // Just digits
```

### Issue: Culture Not Working

**Symptom:** Currency/separator symbols don't change with culture

**Cause:** Culture must be set BEFORE mask

```csharp
// ❌ Wrong order
maskedEntry.Mask = "$ 0,000.00";
maskedEntry.Culture = new CultureInfo("fr-FR");

// ✅ Correct order
maskedEntry.Culture = new CultureInfo("fr-FR");
maskedEntry.Mask = "$ 0,000.00";
```

### Issue: Events Not Firing

**Symptom:** ValueChanged or other events don't trigger

**Causes:**

1. **Event handler not subscribed**
   ```csharp
   // ✅ Subscribe in code-behind or XAML
   maskedEntry.ValueChanged += OnValueChanged;
   ```

2. **Event unsubscribed prematurely**
   ```csharp
   // Don't unsubscribe in OnAppearing if you need it
   ```

### Issue: Password Not Masking

**Symptom:** Characters remain visible

**Causes:**

1. **PasswordChar not set**
   ```csharp
   PasswordChar = '●'  // Must be set
   ```

2. **PasswordDelayDuration too long**
   ```csharp
   PasswordDelayDuration = 10000  // 10 seconds is too long
   ```

### Issue: Liquid Glass Not Appearing

**Symptoms:** No glass effect visible

**Solutions:**

1. **Platform not supported**
   - Verify iOS 26+ or macOS 26+
   
2. **Background not transparent**
   ```csharp
   // ✅ Must be Transparent
   Background = Colors.Transparent
   ```

3. **Missing SfGlassEffectView**
   - Control must be wrapped in `SfGlassEffectView`

### Issue: Performance Degradation

**Symptoms:** Slow typing, laggy UI

**Solutions:**

1. **Simplify RegEx patterns**
2. **Use LostFocus validation**
3. **Debounce ValueChanged**
4. **Profile with .NET profiler**

### Issue: Accessibility Problems

**Symptom:** Screen readers not announcing properly

**Solution:** Set descriptive properties

```csharp
maskedEntry.AutomationId = "PhoneEntry";
maskedEntry.Placeholder = "Enter 10-digit phone number";
```

## RTL (Right-to-Left) Support

Masked Entry inherits MAUI's RTL support:

```xml
<editors:SfMaskedEntry 
    FlowDirection="RightToLeft"
    Mask="00/00/0000" />
```

```csharp
maskedEntry.FlowDirection = FlowDirection.RightToLeft;
```

**Supported Languages:** Arabic, Hebrew, Persian, Urdu

## Summary

Advanced features enable:
- **Localization** with culture-specific formats
- **Security** with password masking and delays
- **Modern UI** with Liquid Glass effects
- **Optimized performance** with best practices
- **Robust handling** of edge cases

## Additional Resources

- [Syncfusion Documentation](https://help.syncfusion.com/maui/masked-entry/overview)
- [GitHub Samples](https://github.com/SyncfusionExamples/maui-maskedentry-samples)
- [API Reference](https://help.syncfusion.com/cr/maui/Syncfusion.Maui.Inputs.SfMaskedEntry.html)
