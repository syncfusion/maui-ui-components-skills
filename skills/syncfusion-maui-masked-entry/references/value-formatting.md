# Value Formatting in .NET MAUI Masked Entry

The `ValueMaskFormat` property controls how the `Value` property includes or excludes prompt characters and literal characters from the mask pattern.

## Table of Contents
- [Overview](#overview)
- [ValueMaskFormat Options](#valuemaskformat-options)
- [Use Cases](#use-cases)
- [Visual Comparison](#visual-comparison)
- [Best Practices](#best-practices)

## Overview

When you retrieve the `Value` property from a Masked Entry, you can choose what to include:

- **Prompt characters**: The placeholder characters (default `_`) shown for unfilled positions
- **Literal characters**: Separators and fixed characters in the mask (e.g., `-`, `/`, `(`, `)`)

The `ValueMaskFormat` enum provides four options to control this behavior.

## ValueMaskFormat Options

```csharp
public enum MaskedEntryMaskFormat
{
    ExcludePromptAndLiterals,
    IncludePrompt,
    IncludeLiterals,
    IncludePromptAndLiterals  // Default
}
```

### 1. ExcludePromptAndLiterals

**Includes:** Only the characters typed by the user  
**Excludes:** Prompt characters AND literal characters

**Use this when:** You want raw, unformatted user input for storage or processing.

```xml
<editors:SfMaskedEntry 
    WidthRequest="200"
    MaskType="Simple"
    Mask=">AAAAA-AAAAA-AAAAA-AAAAA"
    Value="DF321SD1A"
    ValueMaskFormat="ExcludePromptAndLiterals" />
```

```csharp
var maskedEntry = new SfMaskedEntry
{
    WidthRequest = 200,
    MaskType = MaskedEntryMaskType.Simple,
    Mask = ">AAAAA-AAAAA-AAAAA-AAAAA",
    Value = "DF321SD1A",
    ValueMaskFormat = MaskedEntryMaskFormat.ExcludePromptAndLiterals
};

// Value retrieved: "DF321SD1A"
// Display shown: "DF321-SD1A_-_____-_____-_____"
```

**Result:**
- **Display:** `DF321-SD1A_-_____-_____-_____`
- **Value:** `DF321SD1A` (no dashes, no prompts)

### 2. IncludePrompt

**Includes:** Typed characters AND prompt characters  
**Excludes:** Literal characters

**Use this when:** You need to preserve position information but not separators.

```xml
<editors:SfMaskedEntry 
    WidthRequest="200"
    MaskType="Simple"
    Mask=">AAAAA-AAAAA-AAAAA-AAAAA"
    Value="DF321SD1A"
    ValueMaskFormat="IncludePrompt" />
```

```csharp
var maskedEntry = new SfMaskedEntry
{
    WidthRequest = 200,
    MaskType = MaskedEntryMaskType.Simple,
    Mask = ">AAAAA-AAAAA-AAAAA-AAAAA",
    Value = "DF321SD1A",
    ValueMaskFormat = MaskedEntryMaskFormat.IncludePrompt
};

// Value retrieved: "DF321SD1A__________"
// Display shown: "DF321-SD1A_-_____-_____-_____"
```

**Result:**
- **Display:** `DF321-SD1A_-_____-_____-_____`
- **Value:** `DF321SD1A__________` (no dashes, includes underscores)

### 3. IncludeLiterals

**Includes:** Typed characters AND literal characters  
**Excludes:** Prompt characters

**Use this when:** You want formatted output but omit unfilled positions.

```xml
<editors:SfMaskedEntry 
    WidthRequest="200"
    MaskType="Simple"
    Mask=">AAAAA-AAAAA-AAAAA-AAAAA"
    Value="DF321SD1A"
    ValueMaskFormat="IncludeLiterals" />
```

```csharp
var maskedEntry = new SfMaskedEntry
{
    WidthRequest = 200,
    MaskType = MaskedEntryMaskType.Simple,
    Mask = ">AAAAA-AAAAA-AAAAA-AAAAA",
    Value = "DF321SD1A",
    ValueMaskFormat = MaskedEntryMaskFormat.IncludeLiterals
};

// Value retrieved: "DF321-SD1A"
// Display shown: "DF321-SD1A_-_____-_____-_____"
```

**Result:**
- **Display:** `DF321-SD1A_-_____-_____-_____`
- **Value:** `DF321-SD1A` (includes dashes, no prompts)

### 4. IncludePromptAndLiterals (Default)

**Includes:** Typed characters, prompt characters, AND literal characters  
**Excludes:** Nothing

**Use this when:** You want the complete formatted string exactly as displayed.

```xml
<editors:SfMaskedEntry 
    WidthRequest="200"
    MaskType="Simple"
    Mask=">AAAAA-AAAAA-AAAAA-AAAAA"
    Value="DF321SD1A"
    ValueMaskFormat="IncludePromptAndLiterals" />
```

```csharp
var maskedEntry = new SfMaskedEntry
{
    WidthRequest = 200,
    MaskType = MaskedEntryMaskType.Simple,
    Mask = ">AAAAA-AAAAA-AAAAA-AAAAA",
    Value = "DF321SD1A",
    ValueMaskFormat = MaskedEntryMaskFormat.IncludePromptAndLiterals
};

// Value retrieved: "DF321-SD1A_-_____-_____-_____"
// Display shown: "DF321-SD1A_-_____-_____-_____"
```

**Result:**
- **Display:** `DF321-SD1A_-_____-_____-_____`
- **Value:** `DF321-SD1A_-_____-_____-_____` (complete formatted string)

## Use Cases

### Database Storage: ExcludePromptAndLiterals

**Scenario:** Store phone numbers without formatting

```csharp
var phoneEntry = new SfMaskedEntry
{
    MaskType = MaskedEntryMaskType.Simple,
    Mask = "(000) 000-0000",
    ValueMaskFormat = MaskedEntryMaskFormat.ExcludePromptAndLiterals
};

// User enters: (555) 123-4567
// Value for database: "5551234567"
// No parentheses, spaces, or dashes
```

**Benefit:** Consistent storage format, easy to search and validate

### API Submission: IncludeLiterals

**Scenario:** Submit credit card with formatting

```csharp
var cardEntry = new SfMaskedEntry
{
    MaskType = MaskedEntryMaskType.Simple,
    Mask = "0000-0000-0000-0000",
    ValueMaskFormat = MaskedEntryMaskFormat.IncludeLiterals
};

// User enters: 1234567890123456
// Value for API: "1234-5678-9012-3456"
// Includes dashes for display/validation
```

**Benefit:** Human-readable format, easier debugging

### Display Copy: IncludePromptAndLiterals

**Scenario:** User copies formatted value

```csharp
var ssnEntry = new SfMaskedEntry
{
    MaskType = MaskedEntryMaskType.Simple,
    Mask = "000-00-0000",
    ValueMaskFormat = MaskedEntryMaskFormat.IncludePromptAndLiterals
};

// User enters partial: 123-45
// Value copied: "123-45-____"
// Shows what's filled and what's not
```

**Benefit:** User sees exactly what they entered, position-aware

### Internal Processing: IncludePrompt

**Scenario:** Validate complete vs incomplete input

```csharp
var dateEntry = new SfMaskedEntry
{
    MaskType = MaskedEntryMaskType.Simple,
    Mask = "00/00/0000",
    ValueMaskFormat = MaskedEntryMaskFormat.IncludePrompt,
    PromptChar = '_'
};

// User enters: 12/25
// Value: "1225________"
// Can check if underscores remain to determine completeness
```

**Benefit:** Preserve positions without separator clutter

## Visual Comparison

### Phone Number Example

**Mask:** `(000) 000-0000`  
**Input:** `5551234567`  
**PromptChar:** `_`

| ValueMaskFormat | Display | Value Property |
|----------------|---------|----------------|
| **ExcludePromptAndLiterals** | `(555) 123-4567` | `5551234567` |
| **IncludePrompt** | `(555) 123-4567` | `5551234567` |
| **IncludeLiterals** | `(555) 123-4567` | `(555) 123-4567` |
| **IncludePromptAndLiterals** | `(555) 123-4567` | `(555) 123-4567` |

*Note: For complete input, IncludePrompt and ExcludePromptAndLiterals produce same result*

### Product Key Example (Partial Input)

**Mask:** `>AAAAA-AAAAA-AAAAA-AAAAA`  
**Input:** `AB12C` (partial)  
**PromptChar:** `_`

| ValueMaskFormat | Display | Value Property |
|----------------|---------|----------------|
| **ExcludePromptAndLiterals** | `AB12C-_____-_____-_____` | `AB12C` |
| **IncludePrompt** | `AB12C-_____-_____-_____` | `AB12C_______________` |
| **IncludeLiterals** | `AB12C-_____-_____-_____` | `AB12C-` |
| **IncludePromptAndLiterals** | `AB12C-_____-_____-_____` | `AB12C-_____-_____-_____` |

### Date Example (Partial Input)

**Mask:** `00/00/0000`  
**Input:** `1225` (MM/DD only)  
**PromptChar:** `_`

| ValueMaskFormat | Display | Value Property |
|----------------|---------|----------------|
| **ExcludePromptAndLiterals** | `12/25/____` | `1225` |
| **IncludePrompt** | `12/25/____` | `1225____` |
| **IncludeLiterals** | `12/25/____` | `12/25/` |
| **IncludePromptAndLiterals** | `12/25/____` | `12/25/____` |

## Best Practices

### 1. Separate Display from Storage

**Don't:** Store formatted strings in database

```csharp
// ❌ Bad: Stores "(555) 123-4567"
ValueMaskFormat = MaskedEntryMaskFormat.IncludeLiterals
```

**Do:** Store raw data, format for display

```csharp
// ✅ Good: Stores "5551234567"
ValueMaskFormat = MaskedEntryMaskFormat.ExcludePromptAndLiterals

// Format for display when needed
string displayValue = FormatPhoneNumber(rawValue);
```

### 2. Validate Complete Input

Check if mask is complete before processing:

```csharp
maskedEntry.ValueChanged += (s, e) =>
{
    if (e.IsMaskCompleted)
    {
        // Safe to process - all required positions filled
        string rawValue = maskedEntry.Value?.ToString();
        ProcessPhoneNumber(rawValue);
    }
    else
    {
        // Input incomplete - wait or show error
        errorLabel.Text = "Please complete the phone number";
    }
};
```

### 3. Match API Expectations

**API requires formatted:** Use `IncludeLiterals`

```csharp
// API expects: "123-45-6789"
ValueMaskFormat = MaskedEntryMaskFormat.IncludeLiterals
```

**API requires raw:** Use `ExcludePromptAndLiterals`

```csharp
// API expects: "123456789"
ValueMaskFormat = MaskedEntryMaskFormat.ExcludePromptAndLiterals
```

### 4. Consider User Copy/Paste

If users might copy the value, consider including literals:

```csharp
// User can copy formatted value
ValueMaskFormat = MaskedEntryMaskFormat.IncludeLiterals
```

### 5. Document Your Choice

Add comments explaining why you chose a specific format:

```csharp
var ssnEntry = new SfMaskedEntry
{
    Mask = "000-00-0000",
    // Store without dashes for database consistency
    ValueMaskFormat = MaskedEntryMaskFormat.ExcludePromptAndLiterals
};
```

## Common Scenarios

### Scenario 1: Database Storage

```csharp
// Store phone number without formatting
var phoneEntry = new SfMaskedEntry
{
    Mask = "(000) 000-0000",
    ValueMaskFormat = MaskedEntryMaskFormat.ExcludePromptAndLiterals
};

// Save to database
string phoneForDB = phoneEntry.Value?.ToString(); // "5551234567"
database.SavePhone(phoneForDB);
```

### Scenario 2: Display Retrieved Data

```csharp
// Load from database and display
string phoneFromDB = "5551234567"; // No formatting

var phoneEntry = new SfMaskedEntry
{
    Mask = "(000) 000-0000",
    ValueMaskFormat = MaskedEntryMaskFormat.ExcludePromptAndLiterals,
    Value = phoneFromDB
};

// Displays as: (555) 123-4567
```

### Scenario 3: Validation Feedback

```csharp
var ssnEntry = new SfMaskedEntry
{
    Mask = "000-00-0000",
    ValueMaskFormat = MaskedEntryMaskFormat.ExcludePromptAndLiterals,
    PromptChar = '_'
};

ssnEntry.ValueChanged += (s, e) =>
{
    string value = e.NewValue?.ToString();
    
    if (value.Length < 9)
    {
        statusLabel.Text = "SSN incomplete";
        statusLabel.TextColor = Colors.Orange;
    }
    else
    {
        statusLabel.Text = "SSN complete";
        statusLabel.TextColor = Colors.Green;
    }
};
```

### Scenario 4: Export to Excel

```csharp
// Include formatting for readability in export
var phoneEntry = new SfMaskedEntry
{
    Mask = "(000) 000-0000",
    ValueMaskFormat = MaskedEntryMaskFormat.IncludeLiterals
};

// Export formatted value
string exportValue = phoneEntry.Value?.ToString(); // "(555) 123-4567"
excelCell.Value = exportValue;
```

## Edge Cases

### Empty Input

All formats return empty or minimal string when no input:

```csharp
// No input provided
ExcludePromptAndLiterals → ""
IncludePrompt → "__________"
IncludeLiterals → ""
IncludePromptAndLiterals → "(___) ___-____"
```

### Partial Input with Optional Elements

Mask: `00000-9999` (ZIP+4, extension optional)

Input: `12345` (no extension)

```csharp
ExcludePromptAndLiterals → "12345"
IncludePrompt → "12345____"
IncludeLiterals → "12345-"
IncludePromptAndLiterals → "12345-____"
```

### RegEx Masks

ValueMaskFormat behavior is similar but without literal separators (RegEx masks don't have automatic literals):

```csharp
Mask = "\\d{3,5}"  // 3-5 digits
Input = "123"

ExcludePromptAndLiterals → "123"
IncludePrompt → "123__"  (if max length is 5)
IncludeLiterals → "123"  (no literals in RegEx)
IncludePromptAndLiterals → "123__"
```

## Performance Considerations

All formatting options have negligible performance impact. Choose based on functional requirements, not performance.

## Next Steps

- **Validation:** Learn how to validate input → [validation-and-events.md](validation-and-events.md)
- **Customization:** Style your masked entry → [customization.md](customization.md)
- **Advanced Features:** Culture support and more → [advanced-features.md](advanced-features.md)
