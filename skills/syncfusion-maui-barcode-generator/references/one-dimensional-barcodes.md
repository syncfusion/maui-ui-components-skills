# One-Dimensional Barcodes

## Table of Contents
- [Overview](#overview)
- [Common Properties](#common-properties)
- [Codabar](#codabar)
- [Code39](#code39)
- [Code39 Extended](#code39-extended)
- [Code93](#code93)
- [Code128](#code128)
- [Code128A](#code128a)
- [Code128B](#code128b)
- [Code128C](#code128c)
- [UPC-A](#upc-a)
- [UPC-E](#upc-e)
- [EAN-13](#ean-13)
- [EAN-8](#ean-8)
- [Module Property](#module-property)
- [Comparison Table](#comparison-table)
- [Use Case Guide](#use-case-guide)

## Overview

One-dimensional (1D) barcodes, also known as linear barcodes, represent data by varying the widths and spacings of parallel lines. These barcodes are widely used in retail, logistics, healthcare, and inventory management due to their simplicity and compatibility with standard barcode scanners.

The Syncfusion .NET MAUI Barcode Generator supports **11 one-dimensional symbology types**, each designed for specific use cases and character sets.

**Key characteristics:**
- Data encoded in horizontal bars of varying widths
- Read from left to right by scanners
- Support numeric, alphanumeric, or special character sets depending on type
- Widely compatible with barcode scanning hardware
- Lower data capacity than 2D barcodes but faster to scan

## Common Properties

All one-dimensional symbologies share these properties:

| Property | Type | Purpose | Default |
|----------|------|---------|---------|
| `Module` | double | Size of smallest bar line in pixels | Auto-calculated |

**Usage pattern:**

```csharp
barcode.Symbology = new Code128() { Module = 2 };
```

The `Module` property controls the width of the narrowest bar in the barcode, affecting overall size and scanning reliability.

## Codabar

### Overview

Codabar is a discrete numeric symbology used in libraries, blood banks, and various information processing applications. It's one of the oldest barcode types still in use.

**Character support:**
- Digits: 0-9
- Special characters: dash (-), colon (:), slash (/), plus (+)
- Start/stop characters: A, B, C, D

**Structure:**
- Each character has 3 bars and 4 spaces
- Self-checking (no check digit required)

### When to Use

- Library book tracking
- Blood bank labels and specimens
- FedEx airbills
- Photo processing
- Membership cards

### Implementation

**XAML:**

```xaml
<barcode:SfBarcodeGenerator Value="123456789"
                            ShowText="True"
                            HeightRequest="150">
    <barcode:SfBarcodeGenerator.Symbology>
        <barcode:Codabar Module="2" />
    </barcode:SfBarcodeGenerator.Symbology>
</barcode:SfBarcodeGenerator>
```

**C#:**

```csharp
SfBarcodeGenerator barcode = new SfBarcodeGenerator();
barcode.HeightRequest = 150;
barcode.Value = "123456789";
barcode.Symbology = new Codabar() { Module = 2 };
barcode.ShowText = true;
```

### Valid Input Examples

```csharp
// Valid Codabar values
barcode.Value = "0123456789";      // Numeric only
barcode.Value = "12-34:56/78+90";  // With special characters
barcode.Value = "A1234B";          // With start/stop characters
```

### Edge Cases

- **Invalid characters:** Letters (except A, B, C, D as start/stop) will cause errors
- **Empty value:** Will not render
- **Length:** No strict length limit, but keep practical for scanning

## Code39

### Overview

Code39 is a discrete, variable-length symbology encoding alphanumeric characters. It's self-checking and doesn't usually require a check digit for common use cases.

**Character support:**
- Digits: 0-9
- Uppercase letters: A-Z
- Special characters: space, dash (-), plus (+), period (.), dollar ($), slash (/), percent (%)

**Structure:**
- 5 bars and 4 spaces per character
- 3 wide and 6 narrow elements
- Asterisk (*) used as start/stop character

### When to Use

- Industrial applications
- Automotive industry
- U.S. Department of Defense
- Healthcare (patient identification)
- Asset tracking
- Work-in-progress tracking

### Implementation

**XAML:**

```xaml
<barcode:SfBarcodeGenerator Value="CODE39"
                            ShowText="True"
                            HeightRequest="150">
    <barcode:SfBarcodeGenerator.Symbology>
        <barcode:Code39 Module="2" />
    </barcode:SfBarcodeGenerator.Symbology>
</barcode:SfBarcodeGenerator>
```

**C#:**

```csharp
SfBarcodeGenerator barcode = new SfBarcodeGenerator();
barcode.HeightRequest = 150;
barcode.Value = "CODE39";
barcode.Symbology = new Code39() { Module = 2 };
barcode.ShowText = true;
```

### EnableCheckSum Property

Code39 supports an optional check digit for applications requiring higher accuracy:

```csharp
barcode.Symbology = new Code39() 
{ 
    Module = 2,
    EnableCheckSum = true  // Adds check digit (default: true)
};
```

**Default:** `EnableCheckSum = true`

### Valid Input Examples

```csharp
barcode.Value = "ABC123";           // Alphanumeric
barcode.Value = "INVENTORY-2024";   // With dash
barcode.Value = "PART.12345";       // With period
barcode.Value = "ITEM $100";        // With dollar and space
```

### Edge Cases

- **Lowercase letters:** Not supported, will cause errors
- **Special characters:** Only -+.$/% and space are valid
- **Asterisk (*):** Reserved for start/stop, don't include in value

## Code39 Extended

### Overview

Code39 Extended is an extended version of Code39 that supports the full 128 ASCII character set by encoding pairs of Code39 characters.

**Character support:**
- All 128 ASCII characters (0-127)
- Lowercase letters: a-z
- Control characters
- All special characters

### When to Use

- When lowercase letters are needed
- Full ASCII support required
- Legacy system integration requiring special characters

### Implementation

**XAML:**

```xaml
<barcode:SfBarcodeGenerator Value="Code39Ext"
                            ShowText="True"
                            HeightRequest="150">
    <barcode:SfBarcodeGenerator.Symbology>
        <barcode:Code39Extended Module="2" />
    </barcode:SfBarcodeGenerator.Symbology>
</barcode:SfBarcodeGenerator>
```

**C#:**

```csharp
SfBarcodeGenerator barcode = new SfBarcodeGenerator();
barcode.HeightRequest = 150;
barcode.Value = "Code39Ext";
barcode.Symbology = new Code39Extended() { Module = 2 };
barcode.ShowText = true;
```

### EnableCheckSum Property

Like Code39, supports optional check digit:

```csharp
barcode.Symbology = new Code39Extended() 
{ 
    Module = 2,
    EnableCheckSum = true  // Default: true
};
```

### Valid Input Examples

```csharp
barcode.Value = "Mixed-CASE-text";   // Mixed case
barcode.Value = "user@domain.com";   // Email with special chars
barcode.Value = "Item#12345!";       // With symbols
```

## Code93

### Overview

Code93 was designed to complement and enhance Code39, providing higher density and full ASCII support through character combinations. It's a continuous, variable-length symbology.

**Character support:**
- Uppercase letters: A-Z
- Digits: 0-9
- Special characters: *, -, $, %, space, ., /, +, and control characters

**Structure:**
- Continuous symbology (no gaps between characters)
- More compact than Code39
- Built-in check digits (2 check characters)

### When to Use

- High-density applications requiring alphanumeric encoding
- Canadian postal service
- Automotive industry
- Electronic equipment tracking

### Implementation

**XAML:**

```xaml
<barcode:SfBarcodeGenerator Value="01234567"
                            ShowText="True"
                            HeightRequest="150">
    <barcode:SfBarcodeGenerator.Symbology>
        <barcode:Code93 Module="2" />
    </barcode:SfBarcodeGenerator.Symbology>
</barcode:SfBarcodeGenerator>
```

**C#:**

```csharp
SfBarcodeGenerator barcode = new SfBarcodeGenerator();
barcode.HeightRequest = 150;
barcode.Value = "01234567";
barcode.Symbology = new Code93() { Module = 2 };
barcode.ShowText = true;
```

### Valid Input Examples

```csharp
barcode.Value = "ABC-123";
barcode.Value = "TRACKING/456";
barcode.Value = "PART$789";
```

### Edge Cases

- **Asterisk (*):** Start/stop symbol, don't include in value
- **Check digits:** Automatically calculated and added
- **Lowercase:** Not directly supported

## Code128

### Overview

Code128 is a highly efficient, high-density linear barcode that encodes the full ASCII character set. It automatically selects the most efficient character set (A, B, or C) based on the input data.

**Character support:**
- Full 128 ASCII character set (0-127)
- Automatic character set switching for optimal density

**Structure:**
- Continuous symbology
- Automatic check digit
- Variable length

### When to Use

- General-purpose barcode applications
- Shipping and packaging
- Supply chain management
- Inventory tracking
- Healthcare applications
- **Default choice** for most applications

### Implementation

**XAML:**

```xaml
<barcode:SfBarcodeGenerator Value="CODE128"
                            ShowText="True"
                            HeightRequest="150">
    <barcode:SfBarcodeGenerator.Symbology>
        <barcode:Code128 Module="2" />
    </barcode:SfBarcodeGenerator.Symbology>
</barcode:SfBarcodeGenerator>
```

**C#:**

```csharp
SfBarcodeGenerator barcode = new SfBarcodeGenerator();
barcode.HeightRequest = 150;
barcode.Value = "CODE128";
barcode.Symbology = new Code128() { Module = 2 };
barcode.ShowText = true;
```

**Note:** Code128 is the default symbology if none is specified:

```csharp
// These are equivalent:
SfBarcodeGenerator barcode1 = new SfBarcodeGenerator();
barcode1.Value = "12345";

SfBarcodeGenerator barcode2 = new SfBarcodeGenerator();
barcode2.Value = "12345";
barcode2.Symbology = new Code128();
```

### Valid Input Examples

```csharp
barcode.Value = "ABC123xyz";         // Mixed case
barcode.Value = "12345678901234";    // Long numeric
barcode.Value = "ITEM-2024-03-19";   // Alphanumeric with dashes
```

## Code128A

### Overview

Code128A (Character Set A) includes uppercase letters, control characters, and special characters. It's a subset optimized for uppercase and control character encoding.

**Character support:**
- ASCII values 0-95
- Uppercase letters: A-Z
- Digits: 0-9
- Control characters (ASCII 0-31)
- Special characters and punctuation

### When to Use

- When control characters are needed
- Uppercase-only text with special characters
- Legacy systems requiring specific character sets

### Implementation

**XAML:**

```xaml
<barcode:SfBarcodeGenerator Value="CODE128A"
                            ShowText="True"
                            HeightRequest="150">
    <barcode:SfBarcodeGenerator.Symbology>
        <barcode:Code128A Module="2" />
    </barcode:SfBarcodeGenerator.Symbology>
</barcode:SfBarcodeGenerator>
```

**C#:**

```csharp
SfBarcodeGenerator barcode = new SfBarcodeGenerator();
barcode.HeightRequest = 150;
barcode.Value = "CODE128A";
barcode.Symbology = new Code128A() { Module = 2 };
barcode.ShowText = true;
```

### Valid Input Examples

```csharp
barcode.Value = "UPPERCASE123";
barcode.Value = "CONTROL-CHARS";
```

### Edge Cases

- **Lowercase letters:** Not supported, will cause errors
- Use Code128B or Code128 for lowercase support

## Code128B

### Overview

Code128B (Character Set B) includes uppercase and lowercase letters, making it suitable for mixed-case text encoding.

**Character support:**
- ASCII values 32-127
- Uppercase letters: A-Z
- Lowercase letters: a-z
- Digits: 0-9
- Special characters and punctuation

### When to Use

- Mixed-case text encoding
- Standard keyboard characters
- Product descriptions
- Human-readable identifiers

### Implementation

**XAML:**

```xaml
<barcode:SfBarcodeGenerator Value="Code128B"
                            ShowText="True"
                            HeightRequest="150">
    <barcode:SfBarcodeGenerator.Symbology>
        <barcode:Code128B Module="2" />
    </barcode:SfBarcodeGenerator.Symbology>
</barcode:SfBarcodeGenerator>
```

**C#:**

```csharp
SfBarcodeGenerator barcode = new SfBarcodeGenerator();
barcode.HeightRequest = 150;
barcode.Value = "Code128B";
barcode.Symbology = new Code128B() { Module = 2 };
barcode.ShowText = true;
```

### Valid Input Examples

```csharp
barcode.Value = "MixedCase123";
barcode.Value = "Product-Name";
barcode.Value = "user@example.com";
```

## Code128C

### Overview

Code128C (Character Set C) is optimized for encoding numeric data pairs (00-99), achieving twice the density of Code128A or Code128B for numeric data.

**Character support:**
- Numeric pairs: 00-99
- Encodes two digits per symbol
- Most efficient for long numeric sequences

### When to Use

- Long numeric sequences (serial numbers, IDs)
- Shipping containers (SSCC barcodes)
- GS1-128 barcodes
- **Best choice for numeric-only data**

### Implementation

**XAML:**

```xaml
<barcode:SfBarcodeGenerator Value="1234567890"
                            ShowText="True"
                            HeightRequest="150">
    <barcode:SfBarcodeGenerator.Symbology>
        <barcode:Code128C Module="2" />
    </barcode:SfBarcodeGenerator.Symbology>
</barcode:SfBarcodeGenerator>
```

**C#:**

```csharp
SfBarcodeGenerator barcode = new SfBarcodeGenerator();
barcode.HeightRequest = 150;
barcode.Value = "1234567890";
barcode.Symbology = new Code128C() { Module = 2 };
barcode.ShowText = true;
```

### Valid Input Examples

```csharp
barcode.Value = "1234567890";        // Even-length numeric
barcode.Value = "00112233445566";    // Serial number
barcode.Value = "20240319";          // Date (YYYYMMDD)
```

### Edge Cases

- **Odd-length values:** May require padding
- **Non-numeric:** Will cause errors
- **Best practice:** Use Code128 (auto) for mixed content, Code128C for pure numeric

## UPC-A

### Overview

UPC-A (Universal Product Code - Type A) is the standard barcode for retail products in North America. It encodes a 12-digit number consisting of company prefix, product code, and check digit.

**Character support:**
- 12 numeric digits (0-9)
- Last digit is a check digit (auto-calculated if 11 digits provided)

**Structure:**
- Number system digit (1)
- Manufacturer code (5)
- Product code (5)
- Check digit (1)

### When to Use

- Retail point-of-sale systems
- Product packaging (North America)
- Inventory management
- **Required for retail products** in most stores

### Implementation

**XAML:**

```xaml
<barcode:SfBarcodeGenerator Value="72527273070"
                            ShowText="True"
                            HeightRequest="150">
    <barcode:SfBarcodeGenerator.Symbology>
        <barcode:UPCA Module="2" />
    </barcode:SfBarcodeGenerator.Symbology>
</barcode:SfBarcodeGenerator>
```

**C#:**

```csharp
SfBarcodeGenerator barcode = new SfBarcodeGenerator();
barcode.HeightRequest = 150;
barcode.Value = "72527273070";  // 11 digits, check digit auto-added
barcode.Symbology = new UPCA() { Module = 2 };
barcode.ShowText = true;
```

### Input Format

**11 digits:** Check digit calculated automatically

```csharp
barcode.Value = "01234567890";  // Check digit added automatically
```

**12 digits:** Last digit must be valid check digit

```csharp
barcode.Value = "012345678905";  // Check digit must be correct
```

### Valid Input Examples

```csharp
barcode.Value = "72527273070";   // 11 digits
barcode.Value = "725272730706";  // 12 digits with check
```

### Edge Cases

- **Invalid length:** Must be exactly 11 or 12 digits
- **Non-numeric:** Will cause errors
- **Invalid check digit:** If 12 digits provided, check digit must be correct

## UPC-E

### Overview

UPC-E is a zero-suppressed version of UPC-A designed for small packaging. It encodes 6 digits of product code, with the number system (0) and check digit automatically added.

**Character support:**
- 6 numeric digits (0-9)
- Number system (0) automatically prepended
- Check digit automatically appended

### When to Use

- Small product packaging
- Space-constrained labels
- Products that fit zero-suppression rules

### Implementation

**XAML:**

```xaml
<barcode:SfBarcodeGenerator Value="310194"
                            ShowText="True"
                            HeightRequest="150">
    <barcode:SfBarcodeGenerator.Symbology>
        <barcode:UPCE Module="2" />
    </barcode:SfBarcodeGenerator.Symbology>
</barcode:SfBarcodeGenerator>
```

**C#:**

```csharp
SfBarcodeGenerator barcode = new SfBarcodeGenerator();
barcode.HeightRequest = 150;
barcode.Value = "310194";
barcode.Symbology = new UPCE() { Module = 2 };
barcode.ShowText = true;
```

### Valid Input Examples

```csharp
barcode.Value = "123456";
barcode.Value = "310194";
barcode.Value = "012345";
```

### Edge Cases

- **Length:** Must be exactly 6 digits
- **Non-numeric:** Will cause errors
- **Expansion:** Can be expanded to UPC-A by reading system

## EAN-13

### Overview

EAN-13 (European Article Number - 13 digits) is the international standard for retail products, used worldwide except North America (which uses UPC-A). It's based on the UPC-A standard but uses a 2-digit country code.

**Character support:**
- 13 numeric digits (0-9)
- Last digit is check digit (auto-calculated if 12 digits provided)

**Structure:**
- Country/region code (2-3)
- Manufacturer code (variable)
- Product code (variable)
- Check digit (1)

### When to Use

- International retail products
- European product packaging
- Worldwide product identification
- **Standard outside North America**

### Implementation

**XAML:**

```xaml
<barcode:SfBarcodeGenerator Value="9735940564824"
                            ShowText="True"
                            HeightRequest="150">
    <barcode:SfBarcodeGenerator.Symbology>
        <barcode:EAN13 Module="2" />
    </barcode:SfBarcodeGenerator.Symbology>
</barcode:SfBarcodeGenerator>
```

**C#:**

```csharp
SfBarcodeGenerator barcode = new SfBarcodeGenerator();
barcode.HeightRequest = 150;
barcode.Value = "9735940564824";
barcode.Symbology = new EAN13() { Module = 2 };
barcode.ShowText = true;
```

### Input Format

**12 digits:** Check digit calculated automatically

```csharp
barcode.Value = "973594056482";  // Check digit added
```

**13 digits:** Last digit must be valid check digit

```csharp
barcode.Value = "9735940564824";  // Check digit must be correct
```

### Valid Input Examples

```csharp
barcode.Value = "9735940564824";  // 13 digits
barcode.Value = "978014300723";   // 12 digits
barcode.Value = "400112345678";   // Germany country code
```

### Difference from UPC-A

- **Number system:** EAN-13 uses 2-digit country codes (00-99), UPC-A uses single digit (0-9)
- **Length:** EAN-13 is 13 digits, UPC-A is 12 digits
- **Geography:** EAN-13 is international, UPC-A is North American

## EAN-8

### Overview

EAN-8 is the shorter version of EAN-13, designed for small product packaging. It encodes 7 digits with an automatically calculated check digit.

**Character support:**
- 8 numeric digits (0-9)
- Last digit is check digit (auto-calculated if 7 digits provided)

### When to Use

- Small product packaging (limited space)
- Shorter than UPC-E but longer than necessary for small items
- International small products

### Implementation

**XAML:**

```xaml
<barcode:SfBarcodeGenerator Value="11223344"
                            ShowText="True"
                            HeightRequest="150">
    <barcode:SfBarcodeGenerator.Symbology>
        <barcode:EAN8 Module="2" />
    </barcode:SfBarcodeGenerator.Symbology>
</barcode:SfBarcodeGenerator>
```

**C#:**

```csharp
SfBarcodeGenerator barcode = new SfBarcodeGenerator();
barcode.HeightRequest = 150;
barcode.Value = "11223344";
barcode.Symbology = new EAN8() { Module = 2 };
barcode.ShowText = true;
```

### Input Format

**7 digits:** Check digit calculated automatically

```csharp
barcode.Value = "1122334";  // Check digit added
```

**8 digits:** Last digit must be valid check digit

```csharp
barcode.Value = "11223344";  // Check digit must be correct
```

### Valid Input Examples

```csharp
barcode.Value = "11223344";
barcode.Value = "9638507";
barcode.Value = "12345670";
```

## Module Property

The `Module` property is critical for controlling the size and scanning reliability of one-dimensional barcodes.

### What is Module?

**Module** defines the width of the narrowest bar (or space) in the barcode, measured in logical pixels. It directly affects:
- Overall barcode width
- Scanning reliability
- Print quality requirements
- Minimum scanning distance

### Auto-Calculation vs Explicit Setting

#### Auto-Calculation (Module not set)

When `Module` is not specified, the barcode calculates the optimal bar width based on available space:

```csharp
// Module auto-calculated based on width
barcode.WidthRequest = 300;
barcode.Symbology = new Codabar(); // Module not set
```

**Formula:** Module = Available Width / Total Number of Bars

#### Explicit Setting (Module specified)

When `Module` is explicitly set, bar width is fixed:

```csharp
// Fixed module size
barcode.Symbology = new Codabar() { Module = 2 };
// Barcode width = Module × Total Bars
```

### Recommended Module Values

| Use Case | Module Value | Notes |
|----------|--------------|-------|
| Screen display | 1-2 | Clear on high-DPI displays |
| Mobile scanning | 2-3 | Good balance of size/scannability |
| Printed labels (300 DPI) | 2-3 | Reliable printing and scanning |
| Printed labels (600 DPI) | 1-2 | High-quality printing |
| Large signage | 4-8 | Scannable from distance |
| Warehouse labels | 3-5 | Robust for industrial scanners |

### Example: With and Without Module

**Without Module (auto-sized):**

```csharp
barcode.Value = "123456789";
barcode.WidthRequest = 300;
barcode.Symbology = new Codabar();
// Bar width calculated to fit 300px width
```

**With Module (fixed size):**

```csharp
barcode.Value = "123456789";
barcode.Symbology = new Codabar() { Module = 3 };
// Total width = 3px × number of bars
```

### Best Practices

1. **For screen display:** Let auto-calculate or use Module = 1-2
2. **For printing:** Set explicit Module based on printer DPI
3. **For scanning reliability:** Use Module ≥ 2
4. **For mobile apps:** Test with Module = 2-3 for best results
5. **For variable sizes:** Use auto-calculation with explicit Width/HeightRequest

## Comparison Table

| Symbology | Character Set | Check Digit | Best Use Case | Density |
|-----------|---------------|-------------|---------------|---------|
| **Code128** | Full ASCII | Auto | General purpose, shipping | High |
| **Code128A** | Uppercase + control | Auto | Control characters needed | High |
| **Code128B** | Upper/lowercase | Auto | Mixed case text | High |
| **Code128C** | Numeric pairs | Auto | Long numeric sequences | Very High |
| **Code39** | Alphanumeric + limited special | Optional | Industrial, automotive | Medium |
| **Code39 Extended** | Full ASCII | Optional | Full ASCII needed | Medium |
| **Code93** | Alphanumeric + special | Auto | Compact alphanumeric | High |
| **Codabar** | Numeric + limited special | None | Libraries, blood banks | Low |
| **UPC-A** | 12 digits | Auto | Retail (North America) | Medium |
| **UPC-E** | 6 digits | Auto | Small retail packaging | High |
| **EAN-13** | 13 digits | Auto | International retail | Medium |
| **EAN-8** | 8 digits | Auto | Small international products | High |

## Use Case Guide

### Choosing the Right Symbology

**For retail products:**
- North America: UPC-A or UPC-E
- International: EAN-13 or EAN-8

**For general inventory/logistics:**
- Code128 (default choice)
- Code128C for numeric-only data

**For industrial applications:**
- Code39 (standard compliance)
- Code93 (higher density needed)

**For healthcare/libraries:**
- Codabar (standard compliance)
- Code128 (modern alternative)

**For mixed case text:**
- Code128 or Code128B
- Code39 Extended for full ASCII

**For maximum numeric density:**
- Code128C (2 digits per symbol)
- EAN/UPC for standardized products

### Common Mistakes to Avoid

1. **Using wrong character set:** Code39 with lowercase will fail
2. **Forgetting check digits:** UPC/EAN require correct check digits if providing full length
3. **Not setting Module for printing:** Can result in poor print quality
4. **Using Code39 for lowercase:** Use Code39 Extended or Code128 instead
5. **Long numeric in Code128A/B:** Use Code128C for better efficiency
6. **Invalid UPC/EAN lengths:** Must be exact digit counts

## Next Steps

- **[Two-Dimensional Barcodes](two-dimensional-barcodes.md)** - QR Code and Data Matrix
- **[Customization](customization.md)** - Styling and appearance options
- **[API Reference](api-reference.md)** - Complete property documentation
