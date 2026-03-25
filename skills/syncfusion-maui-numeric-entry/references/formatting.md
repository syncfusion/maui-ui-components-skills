# Value Formatting in .NET MAUI Numeric Entry

This guide covers all formatting options for displaying numeric values in different formats including currency, percentage, decimal, and custom formats.

## Table of Contents
- [Currency, Percentage, and Decimal Format](#currency-percentage-and-decimal-format)
- [Format Integer Digits](#format-integer-digits)
- [Format Fractional Digits](#format-fractional-digits)
- [Apply Custom Format](#apply-custom-format)
- [Culture Support](#culture-support)
- [Customize Percentage Display](#customize-percentage-display)
- [Manage Maximum Decimal Digits](#manage-maximum-decimal-digits)

## Currency, Percentage, and Decimal Format

The `CustomFormat` property allows you to format values using standard .NET numeric format strings. By default, the value is formatted based on the current culture's decimal format. The default value of `CustomFormat` is `null`.

### Standard Format Strings

| Format | Code | Example Input | Example Output (US Culture) |
|--------|------|---------------|------------------------------|
| Currency | C or C2 | 1234.56 | $1,234.56 |
| Percentage | P or P2 | 0.75 | 75.00% |
| Decimal (Number) | N or N2 | 1234.5678 | 1,234.57 |

### Currency Format (C)

```xml
<!-- Currency with 2 decimal places -->
<editors:SfNumericEntry CustomFormat="C2"
                        WidthRequest="200"
                        Value="1234.56" />
```

```csharp
var stockPrice = new SfNumericEntry
{
    CustomFormat = "C2",  // $1,234.56
    WidthRequest = 200,
    Value = 1234.56
};
```

**Variations:**
- `C0` - No decimal places: $1,235
- `C1` - One decimal place: $1,234.6
- `C2` - Two decimal places: $1,234.56
- `C3` - Three decimal places: $1,234.560

### Percentage Format (P)

```xml
<!-- Percentage with 2 decimal places -->
<editors:SfNumericEntry CustomFormat="P2"
                        WidthRequest="200"
                        Value="0.7556" />
```

```csharp
var productDiscount = new SfNumericEntry
{
    CustomFormat = "P2",  // 75.56%
    WidthRequest = 200,
    Value = 0.7556
};
```

**Variations:**
- `P0` - No decimal places: 76%
- `P1` - One decimal place: 75.6%
- `P2` - Two decimal places: 75.56%

**Important:** By default, percentage values are multiplied by 100 for display. See [Customize Percentage Display](#customize-percentage-display) for alternative behavior.

### Decimal/Number Format (N)

```xml
<!-- Decimal with 2 decimal places -->
<editors:SfNumericEntry CustomFormat="N2"
                        WidthRequest="200"
                        Value="1234.5678" />
```

```csharp
var hoursWorked = new SfNumericEntry
{
    CustomFormat = "N2",  // 1,234.57
    WidthRequest = 200,
    Value = 1234.5678
};
```

**Variations:**
- `N0` - No decimal places: 1,235
- `N1` - One decimal place: 1,234.6
- `N2` - Two decimal places: 1,234.57
- `N3` - Three decimal places: 1,234.568

### Complete Example

```xml
<VerticalStackLayout Padding="20" Spacing="15">
    
    <!-- Currency -->
    <StackLayout Spacing="5">
        <Label Text="Stock Price:" />
        <editors:SfNumericEntry CustomFormat="C2"
                                WidthRequest="200"
                                Value="1234.56" />
    </StackLayout>
    
    <!-- Percentage -->
    <StackLayout Spacing="5">
        <Label Text="Discount:" />
        <editors:SfNumericEntry CustomFormat="P2"
                                WidthRequest="200"
                                Value="0.15" />
    </StackLayout>
    
    <!-- Decimal -->
    <StackLayout Spacing="5">
        <Label Text="Hours Worked:" />
        <editors:SfNumericEntry CustomFormat="N2"
                                WidthRequest="200"
                                Value="40.75" />
    </StackLayout>
    
</VerticalStackLayout>
```

### Additional Format Strings

You can use any standard .NET numeric format string supported for `double` type. See [Microsoft documentation](https://docs.microsoft.com/en-us/dotnet/standard/base-types/standard-numeric-format-strings) for more formats.

## Format Integer Digits

Use the **0** format specifier to set the minimum number of integer digits. If the value has fewer digits, zeros are prepended.

### Zero Placeholder (0)

**0** (Zero placeholder) replaces the zero with the corresponding digit if present; otherwise, zero appears in the result string.

### Currency with Minimum Integer Digits

```xml
<!-- Minimum 5 integer digits, 2 fractional -->
<editors:SfNumericEntry CustomFormat="$00000.00"
                        WidthRequest="200"
                        Value="123.45" />
<!-- Displays: $00123.45 -->
```

```csharp
var stockPrice = new SfNumericEntry
{
    CustomFormat = "$00000.00",  // $00123.45
    WidthRequest = 200,
    Value = 123.45
};
```

### Percentage with Minimum Integer Digits

```xml
<!-- Minimum 5 integer digits, 2 fractional, percentage -->
<editors:SfNumericEntry CustomFormat="00000.00%"
                        WidthRequest="200"
                        Value="1.5" />
<!-- Displays: 00001.50% -->
```

```csharp
var productDiscount = new SfNumericEntry
{
    CustomFormat = "00000.00%",  // 00001.50%
    WidthRequest = 200,
    Value = 1.5
};
```

### Decimal with Minimum Integer Digits

```xml
<!-- Minimum 5 integer digits, 2 fractional -->
<editors:SfNumericEntry CustomFormat="00000.00"
                        WidthRequest="200"
                        Value="42.75" />
<!-- Displays: 00042.75 -->
```

```csharp
var hoursWorked = new SfNumericEntry
{
    CustomFormat = "00000.00",  // 00042.75
    WidthRequest = 200,
    Value = 42.75
};
```

### Examples

| CustomFormat | Value | Display |
|--------------|-------|---------|
| `"000.00"` | 12.5 | 012.50 |
| `"0000"` | 123 | 0123 |
| `"$00000"` | 999 | $00999 |

## Format Fractional Digits

Use the **0** format specifier to set the minimum number of fractional (decimal) digits.

### Currency with Fractional Digits

```xml
<!-- 3 integer digits, 3 fractional -->
<editors:SfNumericEntry CustomFormat="$000.000"
                        WidthRequest="200"
                        Value="12.5" />
<!-- Displays: $012.500 -->
```

```csharp
var stockPrice = new SfNumericEntry
{
    CustomFormat = "$000.000",  // $012.500
    WidthRequest = 200,
    Value = 12.5
};
```

### Percentage with Fractional Digits

```xml
<editors:SfNumericEntry CustomFormat="00.000%"
                        WidthRequest="200"
                        Value="5.1234" />
<!-- Displays: 05.123% (rounded) -->
```

```csharp
var productDiscount = new SfNumericEntry
{
    CustomFormat = "00.000%",
    WidthRequest = 200,
    Value = 5.1234
};
```

### Decimal with Fractional Digits

```xml
<editors:SfNumericEntry CustomFormat="00.000"
                        WidthRequest="200"
                        Value="8.2" />
<!-- Displays: 08.200 -->
```

```csharp
var hoursWorked = new SfNumericEntry
{
    CustomFormat = "00.000",
    WidthRequest = 200,
    Value = 8.2
};
```

### Examples

| CustomFormat | Value | Display |
|--------------|-------|---------|
| `"0.000"` | 1.5 | 1.500 |
| `"00.0000"` | 3.14 | 03.1400 |
| `"$0.000"` | 99.9 | $99.900 |

## Apply Custom Format

Combine **0** (zero placeholder) and **#** (digit placeholder) to create flexible custom formats with minimum and maximum fractional digits.

### Format Specifiers

- **0** (Zero placeholder): Replaces zero with corresponding digit if present; otherwise displays zero
- **#** (Digit placeholder): Replaces # with corresponding digit if present; otherwise displays nothing

### Minimum and Maximum Fractional Digits

```xml
<!-- Minimum 2, maximum 4 fractional digits -->
<editors:SfNumericEntry CustomFormat="#.00##"
                        WidthRequest="200" />
```

**Examples:**

| Value | Display with "#.00##" |
|-------|-----------------------|
| 1 | 1.00 |
| 1.5 | 1.50 |
| 1.567 | 1.567 |
| 1.56789 | 1.5679 (rounded) |

### Currency with Flexible Decimals

```xml
<editors:SfNumericEntry CustomFormat="$00.00##"
                        WidthRequest="200"
                        Value="12.5678" />
<!-- Displays: $12.5678 -->
```

```csharp
var stockPrice = new SfNumericEntry
{
    CustomFormat = "$00.00##",  // Min 2, max 4 decimals
    WidthRequest = 200,
    Value = 12.5678
};
```

**Examples:**

| Value | Display with "$00.00##" |
|-------|-------------------------|
| 5 | $05.00 |
| 5.1 | $05.10 |
| 5.123 | $05.123 |
| 5.12345 | $05.1235 (rounded) |

### Percentage with Flexible Decimals

```xml
<editors:SfNumericEntry CustomFormat="00.00##%"
                        WidthRequest="200"
                        Value="15.6789" />
<!-- Displays: 15.6789% -->
```

```csharp
var productDiscount = new SfNumericEntry
{
    CustomFormat = "00.00##%",
    WidthRequest = 200,
    Value = 15.6789
};
```

### Decimal with Flexible Decimals

```xml
<editors:SfNumericEntry CustomFormat="00.00##"
                        WidthRequest="200"
                        Value="42.789" />
<!-- Displays: 42.789 -->
```

```csharp
var hoursWorked = new SfNumericEntry
{
    CustomFormat = "00.00##",
    WidthRequest = 200,
    Value = 42.789
};
```

### Advanced Custom Formats

```xml
<!-- Thousands separator with flexible decimals -->
<editors:SfNumericEntry CustomFormat="#,##0.0#"
                        Value="1234567.89" />
<!-- Displays: 1,234,567.89 -->

<!-- Scientific notation -->
<editors:SfNumericEntry CustomFormat="0.00E+00"
                        Value="12345" />
<!-- Displays: 1.23E+04 -->

<!-- Custom prefix/suffix -->
<editors:SfNumericEntry CustomFormat="'Total: $'#,##0.00' USD'"
                        Value="1234.56" />
<!-- Displays: Total: $1,234.56 USD -->
```

## Culture Support

The `Culture` property allows you to format values according to specific regional settings.

### Set Culture

```csharp
using System.Globalization;

// US Culture
var numericEntry = new SfNumericEntry
{
    CustomFormat = "C2",
    Value = 1234.56,
    Culture = new CultureInfo("en-US")
};
// Displays: $1,234.56

// Euro Culture
var euroEntry = new SfNumericEntry
{
    CustomFormat = "C2",
    Value = 1234.56,
    Culture = new CultureInfo("de-DE")
};
// Displays: 1.234,56 €

// Japanese Yen
var yenEntry = new SfNumericEntry
{
    CustomFormat = "C0",
    Value = 1234,
    Culture = new CultureInfo("ja-JP")
};
// Displays: ¥1,234
```

### Culture Format Examples

| Culture | Format | Value | Display |
|---------|--------|-------|---------|
| en-US | C2 | 1234.56 | $1,234.56 |
| en-GB | C2 | 1234.56 | £1,234.56 |
| de-DE | C2 | 1234.56 | 1.234,56 € |
| fr-FR | C2 | 1234.56 | 1 234,56 € |
| ja-JP | C0 | 1234 | ¥1,234 |
| zh-CN | C2 | 1234.56 | ¥1,234.56 |

### Complete Culture Example

```csharp
public partial class FormattingPage : ContentPage
{
    public FormattingPage()
    {
        InitializeComponent();
        
        var layout = new VerticalStackLayout { Padding = 20, Spacing = 15 };
        
        // US Dollar
        layout.Children.Add(new Label { Text = "US Dollar:" });
        layout.Children.Add(new SfNumericEntry
        {
            CustomFormat = "C2",
            Value = 1234.56,
            Culture = new CultureInfo("en-US")
        });
        
        // Euro
        layout.Children.Add(new Label { Text = "Euro:" });
        layout.Children.Add(new SfNumericEntry
        {
            CustomFormat = "C2",
            Value = 1234.56,
            Culture = new CultureInfo("de-DE")
        });
        
        // British Pound
        layout.Children.Add(new Label { Text = "British Pound:" });
        layout.Children.Add(new SfNumericEntry
        {
            CustomFormat = "C2",
            Value = 1234.56,
            Culture = new CultureInfo("en-GB")
        });
        
        Content = layout;
    }
}
```

## Customize Percentage Display

When using percentage format, the `PercentDisplayMode` property controls how the value is displayed.

### PercentDisplayMode Options

| Mode | Behavior | Example Input | Example Output |
|------|----------|---------------|----------------|
| `Compute` (Default) | Multiplies value by 100 | 1000 | 100,000.00% |
| `Value` | Displays actual value | 1000 | 1000.00% |

### Compute Mode (Default)

The value is multiplied by 100 before adding the percent symbol.

```xml
<editors:SfNumericEntry CustomFormat="P"
                        Value="1000"
                        PercentDisplayMode="Compute"
                        WidthRequest="200" />
<!-- Displays: 100,000.00% -->
```

```csharp
var numericEntry = new SfNumericEntry
{
    CustomFormat = "P",
    Value = 1000,
    PercentDisplayMode = PercentDisplayMode.Compute,  // Default
    WidthRequest = 200
};
// Displays: 100,000.00%
```

**Use case:** Standard percentage calculation (0.15 → 15%)

### Value Mode

The actual value is displayed with the percent symbol (no multiplication).

```xml
<editors:SfNumericEntry CustomFormat="P"
                        Value="1000"
                        PercentDisplayMode="Value"
                        WidthRequest="200" />
<!-- Displays: 1,000.00% -->
```

```csharp
var numericEntry = new SfNumericEntry
{
    CustomFormat = "P",
    Value = 1000,
    PercentDisplayMode = PercentDisplayMode.Value,
    WidthRequest = 200
};
// Displays: 1,000.00%
```

**Use case:** Direct percentage entry (user types 50 for 50%)

### Comparison Examples

```xml
<VerticalStackLayout Padding="20" Spacing="15">
    
    <!-- Compute Mode -->
    <StackLayout Spacing="5">
        <Label Text="Compute Mode (0.75):" />
        <editors:SfNumericEntry CustomFormat="P2"
                                Value="0.75"
                                PercentDisplayMode="Compute"
                                WidthRequest="200" />
        <!-- Displays: 75.00% -->
    </StackLayout>
    
    <!-- Value Mode -->
    <StackLayout Spacing="5">
        <Label Text="Value Mode (75):" />
        <editors:SfNumericEntry CustomFormat="P2"
                                Value="75"
                                PercentDisplayMode="Value"
                                WidthRequest="200" />
        <!-- Displays: 75.00% -->
    </StackLayout>
    
</VerticalStackLayout>
```

**When to use Value mode:**
- User enters percentage directly (50 for 50%)
- Discount percentage fields
- Tax rate inputs
- Commission percentage

**When to use Compute mode:**
- Decimal fraction inputs (0.5 for 50%)
- Mathematical percentage calculations
- Rate conversions

## Manage Maximum Decimal Digits

The `MaximumNumberDecimalDigits` property specifies the maximum number of digits after the decimal point.

### Important Limitations

- ⚠️ **Only works when `CustomFormat` is NOT set**
- ⚠️ Only accepts positive values
- Default value: `2`

### Set Maximum Decimal Digits

```xml
<editors:SfNumericEntry Value="1000.23232"
                        MaximumNumberDecimalDigits="3"
                        WidthRequest="200" />
<!-- Displays: 1000.232 -->
```

```csharp
var numericEntry = new SfNumericEntry
{
    WidthRequest = 200,
    Value = 1000.23232,
    MaximumNumberDecimalDigits = 3  // Shows 1000.232
};
```

### Examples

| Value | MaximumNumberDecimalDigits | Display |
|-------|----------------------------|---------|
| 123.456789 | 0 | 123 |
| 123.456789 | 1 | 123.5 (rounded) |
| 123.456789 | 2 | 123.46 (rounded) |
| 123.456789 | 3 | 123.457 (rounded) |
| 123.456789 | 5 | 123.45679 (rounded) |

### Use Cases

```csharp
// Whole numbers only
var quantity = new SfNumericEntry
{
    MaximumNumberDecimalDigits = 0  // 123
};

// Prices (2 decimals)
var price = new SfNumericEntry
{
    MaximumNumberDecimalDigits = 2  // 19.99
};

// Precise measurements (4 decimals)
var measurement = new SfNumericEntry
{
    MaximumNumberDecimalDigits = 4  // 10.1234
};
```

**Note:** Use `CustomFormat` with 0 or # specifiers for more control over decimal display when formatting is needed.

## Complete Formatting Example

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:editors="clr-namespace:Syncfusion.Maui.Inputs;assembly=Syncfusion.Maui.Inputs"
             x:Class="NumericEntryApp.FormattingPage">
    
    <ScrollView>
        <VerticalStackLayout Padding="20" Spacing="20">
            
            <!-- Standard Formats -->
            <Label Text="Standard Formats" FontSize="18" FontAttributes="Bold" />
            
            <StackLayout Spacing="5">
                <Label Text="Currency (C2):" />
                <editors:SfNumericEntry CustomFormat="C2"
                                        Value="1234.56"
                                        WidthRequest="250" />
            </StackLayout>
            
            <StackLayout Spacing="5">
                <Label Text="Percentage (P2):" />
                <editors:SfNumericEntry CustomFormat="P2"
                                        Value="0.7556"
                                        WidthRequest="250" />
            </StackLayout>
            
            <StackLayout Spacing="5">
                <Label Text="Number (N2):" />
                <editors:SfNumericEntry CustomFormat="N2"
                                        Value="1234.5678"
                                        WidthRequest="250" />
            </StackLayout>
            
            <!-- Custom Formats -->
            <Label Text="Custom Formats" FontSize="18" FontAttributes="Bold" />
            
            <StackLayout Spacing="5">
                <Label Text="Padded ($00000.00):" />
                <editors:SfNumericEntry CustomFormat="$00000.00"
                                        Value="123.45"
                                        WidthRequest="250" />
            </StackLayout>
            
            <StackLayout Spacing="5">
                <Label Text="Flexible (00.00##):" />
                <editors:SfNumericEntry CustomFormat="00.00##"
                                        Value="42.789"
                                        WidthRequest="250" />
            </StackLayout>
            
            <!-- Percentage Display Modes -->
            <Label Text="Percentage Display Modes" FontSize="18" FontAttributes="Bold" />
            
            <StackLayout Spacing="5">
                <Label Text="Compute Mode (0.75):" />
                <editors:SfNumericEntry CustomFormat="P2"
                                        Value="0.75"
                                        PercentDisplayMode="Compute"
                                        WidthRequest="250" />
            </StackLayout>
            
            <StackLayout Spacing="5">
                <Label Text="Value Mode (75):" />
                <editors:SfNumericEntry CustomFormat="P2"
                                        Value="75"
                                        PercentDisplayMode="Value"
                                        WidthRequest="250" />
            </StackLayout>
            
            <!-- Maximum Decimal Digits -->
            <Label Text="Maximum Decimal Digits" FontSize="18" FontAttributes="Bold" />
            
            <StackLayout Spacing="5">
                <Label Text="Max 3 decimals:" />
                <editors:SfNumericEntry Value="123.456789"
                                        MaximumNumberDecimalDigits="3"
                                        WidthRequest="250" />
            </StackLayout>
            
        </VerticalStackLayout>
    </ScrollView>
</ContentPage>
```

## Summary

This reference covered:
- ✅ Standard formats: Currency (C), Percentage (P), Number (N)
- ✅ Integer digit formatting with zero placeholder (0)
- ✅ Fractional digit formatting
- ✅ Custom formats with 0 and # specifiers
- ✅ Culture-based formatting
- ✅ Percentage display modes (Compute vs Value)
- ✅ Maximum decimal digits control

**Next:** Read [restrictions.md](restrictions.md) for value validation and range constraints.
