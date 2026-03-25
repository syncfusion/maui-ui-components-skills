# Barcode Customization

## Table of Contents
- [Overview](#overview)
- [Text Customization](#text-customization)
  - [Displaying Input Value](#displaying-input-value)
  - [Text Style](#text-style)
  - [Text Spacing](#text-spacing)
  - [Text Alignment](#text-alignment)
- [Bar and Module Customization](#bar-and-module-customization)
  - [Module Property](#module-property)
  - [Foreground Color](#foreground-color)
  - [Background Color](#background-color)
- [Complete Styling Examples](#complete-styling-examples)
- [Best Practices](#best-practices)

## Overview

The Syncfusion .NET MAUI Barcode Generator provides extensive customization options to match your application's design and improve barcode readability. You can customize:

**Text display and styling:**
- Show/hide input values
- Font family, size, and attributes
- Text color and positioning
- Spacing between barcode and text

**Visual appearance:**
- Bar/dot colors (foreground)
- Background colors
- Size of smallest bar/dot (module)
- Overall dimensions

Proper customization ensures barcodes are both aesthetically pleasing and functionally optimal for scanning.

## Text Customization

### Displaying Input Value

By default, barcode input values are not displayed as text. The `ShowText` property enables text display below the barcode.

#### ShowText Property

**Type:** `bool`  
**Default:** `false`  
**Purpose:** Display the barcode value as text below the barcode

**XAML:**

```xaml
<barcode:SfBarcodeGenerator Value="12634388927" 
                            HeightRequest="150"
                            WidthRequest="300" 
                            ShowText="True"/>
```

**C#:**

```csharp
SfBarcodeGenerator barcode = new SfBarcodeGenerator();
barcode.HeightRequest = 150;
barcode.WidthRequest = 300;
barcode.Value = "12634388927";
barcode.ShowText = true;  // Enable text display
```

**When to enable:**
- Product labels where human readability is needed
- Inventory systems with manual data entry backup
- Retail applications where price/product code should be visible
- Debug/testing scenarios to verify encoded data

**When to disable:**
- Space-constrained layouts
- Security-sensitive data (passwords, tokens)
- Pure scanning applications without manual entry
- Aesthetic designs where text is redundant

### Text Style

The `TextStyle` property allows comprehensive customization of the displayed text appearance through the `BarcodeTextStyle` class.

#### BarcodeTextStyle Properties

| Property | Type | Purpose | Default |
|----------|------|---------|---------|
| `FontFamily` | string | Font name | System default |
| `FontSize` | double | Text size in points | 12 |
| `FontAttributes` | FontAttributes | Bold, Italic, or None | None |
| `TextColor` | Color | Text color | Black |

#### Setting Text Style

**XAML:**

```xaml
<barcode:SfBarcodeGenerator Value="12634388927" 
                            HeightRequest="150"
                            WidthRequest="300" 
                            ShowText="True">
    <barcode:SfBarcodeGenerator.TextStyle>
        <barcode:BarcodeTextStyle FontAttributes="Italic" 
                                  FontSize="16" 
                                  FontFamily="Times" 
                                  TextColor="Red"/>
    </barcode:SfBarcodeGenerator.TextStyle>
</barcode:SfBarcodeGenerator>
```

**C#:**

```csharp
SfBarcodeGenerator barcode = new SfBarcodeGenerator();
barcode.HeightRequest = 150;
barcode.WidthRequest = 300;
barcode.Value = "12634388927";
barcode.ShowText = true;
barcode.TextStyle = new BarcodeTextStyle()
{
    FontAttributes = FontAttributes.Italic,
    FontFamily = "Times",
    FontSize = 16,
    TextColor = Colors.Red
};
```

#### Font Attributes Options

```csharp
// No special formatting
barcode.TextStyle = new BarcodeTextStyle()
{
    FontAttributes = FontAttributes.None
};

// Bold text
barcode.TextStyle = new BarcodeTextStyle()
{
    FontAttributes = FontAttributes.Bold
};

// Italic text
barcode.TextStyle = new BarcodeTextStyle()
{
    FontAttributes = FontAttributes.Italic
};

// Bold and italic (combine with |)
barcode.TextStyle = new BarcodeTextStyle()
{
    FontAttributes = FontAttributes.Bold | FontAttributes.Italic
};
```

#### Font Family Examples

```csharp
// Serif fonts (professional, traditional)
barcode.TextStyle = new BarcodeTextStyle()
{
    FontFamily = "Times New Roman"
};

// Sans-serif fonts (modern, clean)
barcode.TextStyle = new BarcodeTextStyle()
{
    FontFamily = "Arial"
};

// Monospace fonts (technical, code-like)
barcode.TextStyle = new BarcodeTextStyle()
{
    FontFamily = "Courier New"
};

// System default
barcode.TextStyle = new BarcodeTextStyle()
{
    FontFamily = "OpenSans-Regular"  // MAUI default
};
```

**Platform-specific fonts:**
- Test fonts on all target platforms
- Provide fallback fonts if custom fonts are unavailable
- Use common system fonts for maximum compatibility

#### Text Color Examples

```csharp
// Standard colors
barcode.TextStyle = new BarcodeTextStyle()
{
    TextColor = Colors.Black  // Default, highest contrast
};

barcode.TextStyle = new BarcodeTextStyle()
{
    TextColor = Colors.Blue  // Brand colors
};

// RGB colors
barcode.TextStyle = new BarcodeTextStyle()
{
    TextColor = Color.FromRgb(255, 0, 0)  // Red
};

// RGBA colors with transparency
barcode.TextStyle = new BarcodeTextStyle()
{
    TextColor = Color.FromRgba(0, 0, 0, 0.7)  // Semi-transparent black
};

// Hex colors
barcode.TextStyle = new BarcodeTextStyle()
{
    TextColor = Color.FromArgb("#003366")  // Navy blue
};
```

#### Complete Text Style Example

```csharp
// Professional product label styling
SfBarcodeGenerator productBarcode = new SfBarcodeGenerator();
productBarcode.Value = "PRODUCT-123456";
productBarcode.HeightRequest = 150;
productBarcode.WidthRequest = 300;
productBarcode.ShowText = true;
productBarcode.TextStyle = new BarcodeTextStyle()
{
    FontFamily = "Arial",
    FontSize = 14,
    FontAttributes = FontAttributes.Bold,
    TextColor = Color.FromRgb(0, 51, 102)  // Dark blue
};
```

### Text Spacing

The `TextSpacing` property controls the vertical gap between the barcode and the displayed text.

**Type:** `double`  
**Default:** `2` (pixels)  
**Purpose:** Adjust space between barcode and text

**XAML:**

```xaml
<barcode:SfBarcodeGenerator Value="12634388927" 
                            HeightRequest="150"
                            WidthRequest="300" 
                            ShowText="True" 
                            TextSpacing="25"/>
```

**C#:**

```csharp
SfBarcodeGenerator barcode = new SfBarcodeGenerator();
barcode.HeightRequest = 150;
barcode.WidthRequest = 300;
barcode.Value = "12634388927";
barcode.ShowText = true;
barcode.TextSpacing = 25;  // 25 pixels between barcode and text
```

#### Recommended Text Spacing Values

| Barcode Height | Recommended Spacing | Use Case |
|----------------|---------------------|----------|
| 100-150px | 5-10px | Compact layouts |
| 150-250px | 10-20px | Standard labels |
| 250-400px | 20-30px | Large displays |
| 400px+ | 30-50px | Signage, posters |

#### Text Spacing Examples

```csharp
// Minimal spacing (compact)
barcode.TextSpacing = 5;

// Standard spacing
barcode.TextSpacing = 15;

// Large spacing (emphasis)
barcode.TextSpacing = 30;

// No spacing (text immediately below barcode)
barcode.TextSpacing = 0;
```

**Best practices:**
- Increase spacing for larger barcodes to maintain visual balance
- Reduce spacing for space-constrained labels
- Test printed output to ensure text doesn't touch barcode
- Consider paper/material absorption that might cause ink bleed

### Text Alignment

The `TextAlignment` property controls the horizontal positioning of the displayed text relative to the barcode.

**Type:** `TextAlignment` (enum)  
**Default:** `Center`  
**Options:** `Start`, `Center`, `End`

**XAML:**

```xaml
<barcode:SfBarcodeGenerator Value="12634" 
                            HeightRequest="150"
                            WidthRequest="240" 
                            ShowText="True" 
                            TextAlignment="End"/>
```

**C#:**

```csharp
SfBarcodeGenerator barcode = new SfBarcodeGenerator();
barcode.HeightRequest = 150;
barcode.WidthRequest = 240;
barcode.Value = "12634";
barcode.ShowText = true;
barcode.TextAlignment = TextAlignment.End;  // Right-aligned
```

#### Alignment Options

```csharp
// Left-aligned (Start)
barcode.TextAlignment = TextAlignment.Start;

// Center-aligned (default, most common)
barcode.TextAlignment = TextAlignment.Center;

// Right-aligned (End)
barcode.TextAlignment = TextAlignment.End;
```

#### When to Use Each Alignment

**Start (Left):**
- Multi-barcode layouts with left alignment
- Forms with left-aligned fields
- RTL (right-to-left) language interfaces (becomes right alignment)

**Center (Default):**
- Single barcodes
- Symmetric layouts
- Product labels
- **Most common choice**

**End (Right):**
- Multi-barcode layouts with right alignment
- Invoices or documents with right-aligned data
- Complementing right-aligned UI elements

## Bar and Module Customization

### Module Property

The `Module` property defines the size of the smallest unit in a barcode:
- **1D barcodes:** Width of the narrowest bar
- **2D barcodes:** Size of the smallest dot/square

**Type:** `double`  
**Default:** Auto-calculated based on available space  
**Unit:** Logical pixels

#### Module for One-Dimensional Barcodes

**Without Module (auto-sized):**

```csharp
barcode.Value = "123456789";
barcode.WidthRequest = 300;
barcode.Symbology = new Codabar();
// Module width = 300px / total bars (auto-calculated)
```

**With Module (fixed bar width):**

```csharp
barcode.Value = "123456789";
barcode.Symbology = new Codabar() { Module = 1 };
// Each narrow bar is 1 pixel wide
// Total width = Module × number of bars
```

**XAML Example:**

```xaml
<barcode:SfBarcodeGenerator Value="123456789" 
                            HeightRequest="150"
                            WidthRequest="240" 
                            ShowText="True" 
                            BackgroundColor="LightCyan">
    <barcode:SfBarcodeGenerator.Symbology>
        <barcode:Codabar Module="1"/>
    </barcode:SfBarcodeGenerator.Symbology>
</barcode:SfBarcodeGenerator>
```

**C# Example:**

```csharp
SfBarcodeGenerator barcode = new SfBarcodeGenerator();
barcode.HeightRequest = 150;
barcode.WidthRequest = 240;
barcode.Value = "123456789";
barcode.ShowText = true;
barcode.BackgroundColor = Colors.LightCyan;
barcode.Symbology = new Codabar() { Module = 1 };
```

#### Module for Two-Dimensional Barcodes

**Without Module (auto-sized):**

```csharp
barcode.Value = "123456789";
barcode.HeightRequest = 150;
barcode.WidthRequest = 230;
barcode.Symbology = new QRCode();
// Module size = min(150, 230) / version modules (auto-calculated)
```

**With Module (fixed dot size):**

```csharp
barcode.Value = "123456789";
barcode.Symbology = new QRCode() { Module = 2 };
// Each QR code module (dot) is 2×2 pixels
```

**XAML Example:**

```xaml
<barcode:SfBarcodeGenerator Value="123456789" 
                            HeightRequest="150"
                            WidthRequest="230"  
                            BackgroundColor="LightCyan">
    <barcode:SfBarcodeGenerator.Symbology>
        <barcode:QRCode Module="2"/>
    </barcode:SfBarcodeGenerator.Symbology>
</barcode:SfBarcodeGenerator>
```

**C# Example:**

```csharp
SfBarcodeGenerator barcode = new SfBarcodeGenerator();
barcode.HeightRequest = 150;
barcode.WidthRequest = 230;
barcode.Value = "123456789";
barcode.BackgroundColor = Colors.LightCyan;
barcode.Symbology = new QRCode() { Module = 2 };
```

#### Recommended Module Values

**Screen Display:**
| Barcode Type | Module Value | Notes |
|--------------|--------------|-------|
| 1D Barcodes | 1-2px | Clear on high-DPI screens |
| QR Codes | 2-4px | Optimal for mobile devices |
| Data Matrix | 2-3px | Balance size and clarity |

**Printing (300 DPI):**
| Barcode Type | Module Value | Notes |
|--------------|--------------|-------|
| 1D Barcodes | 2-3px | Reliable scanning |
| QR Codes | 2-3px | Good print quality |
| Data Matrix | 2-3px | Precision required |

**Printing (600 DPI):**
| Barcode Type | Module Value | Notes |
|--------------|--------------|-------|
| 1D Barcodes | 1-2px | High-quality output |
| QR Codes | 1-2px | Professional printing |
| Data Matrix | 1-2px | Fine detail |

**Large Format/Signage:**
| Barcode Type | Module Value | Notes |
|--------------|--------------|-------|
| 1D Barcodes | 4-8px | Scannable from distance |
| QR Codes | 5-10px | Billboard/poster use |
| Data Matrix | 4-6px | Industrial signage |

### Foreground Color

The `ForegroundColor` property sets the color of the barcode bars/dots.

**Type:** `Color`  
**Default:** `Black`  
**Purpose:** Color of bars (1D) or dots (2D)

**XAML:**

```xaml
<barcode:SfBarcodeGenerator Value="12634" 
                            HeightRequest="150"
                            WidthRequest="240"
                            ForegroundColor="Purple"/>
```

**C#:**

```csharp
SfBarcodeGenerator barcode = new SfBarcodeGenerator();
barcode.HeightRequest = 150;
barcode.WidthRequest = 240;
barcode.Value = "12634";
barcode.ForegroundColor = Colors.Purple;
```

#### Color Options

```csharp
// Standard black (highest contrast, recommended)
barcode.ForegroundColor = Colors.Black;

// Brand colors
barcode.ForegroundColor = Colors.Blue;
barcode.ForegroundColor = Colors.Red;
barcode.ForegroundColor = Colors.Green;

// RGB colors
barcode.ForegroundColor = Color.FromRgb(0, 51, 102);  // Navy

// RGBA with transparency (use with caution)
barcode.ForegroundColor = Color.FromRgba(0, 0, 0, 0.9);

// Hex colors
barcode.ForegroundColor = Color.FromArgb("#003366");
```

**Important considerations:**
- **Contrast is critical:** Ensure sufficient contrast with background
- **Scanner compatibility:** Some scanners require dark foreground on light background
- **Printing:** Test colored barcodes on target printers
- **CMYK conversion:** RGB colors may shift when printed
- **Avoid light colors:** Light bars may not scan reliably

### Background Color

The `BackgroundColor` property sets the color behind the barcode.

**Type:** `Color`  
**Default:** `Transparent`  
**Purpose:** Background color of barcode area

**XAML:**

```xaml
<barcode:SfBarcodeGenerator Value="123456789" 
                            HeightRequest="150"
                            WidthRequest="240"
                            BackgroundColor="LightCyan"/>
```

**C#:**

```csharp
SfBarcodeGenerator barcode = new SfBarcodeGenerator();
barcode.HeightRequest = 150;
barcode.WidthRequest = 240;
barcode.Value = "123456789";
barcode.BackgroundColor = Colors.LightCyan;
```

#### Background Color Examples

```csharp
// White (standard for printing)
barcode.BackgroundColor = Colors.White;

// Transparent (inherits container background)
barcode.BackgroundColor = Colors.Transparent;

// Light colors (ensure contrast)
barcode.BackgroundColor = Colors.LightGray;
barcode.BackgroundColor = Color.FromRgb(240, 240, 240);

// Branded backgrounds (test scanning)
barcode.BackgroundColor = Color.FromRgb(255, 250, 240);  // Cream
```

**Best practices:**
- **White is safest:** Maximum scanner compatibility
- **Light backgrounds only:** Dark backgrounds may prevent scanning
- **Test scanning:** Not all scanners handle colored backgrounds well
- **Print testing:** Colors appear differently when printed

## Complete Styling Examples

### Example 1: Professional Product Label

```csharp
SfBarcodeGenerator productLabel = new SfBarcodeGenerator();
productLabel.Value = "PROD-2024-12345";
productLabel.HeightRequest = 150;
productLabel.WidthRequest = 350;
productLabel.Symbology = new Code128() { Module = 2 };

// Visual styling
productLabel.ForegroundColor = Colors.Black;
productLabel.BackgroundColor = Colors.White;

// Text styling
productLabel.ShowText = true;
productLabel.TextSpacing = 15;
productLabel.TextAlignment = TextAlignment.Center;
productLabel.TextStyle = new BarcodeTextStyle()
{
    FontFamily = "Arial",
    FontSize = 14,
    FontAttributes = FontAttributes.Bold,
    TextColor = Colors.Black
};
```

### Example 2: Branded QR Code

```csharp
SfBarcodeGenerator brandedQR = new SfBarcodeGenerator();
brandedQR.Value = "https://company.com/product/12345";
brandedQR.HeightRequest = 300;
brandedQR.WidthRequest = 300;
brandedQR.Symbology = new QRCode()
{
    ErrorCorrectionLevel = ErrorCorrectionLevel.High,
    Module = 3
};

// Brand colors (navy and cream)
brandedQR.ForegroundColor = Color.FromRgb(0, 51, 102);  // Navy
brandedQR.BackgroundColor = Color.FromRgb(255, 250, 240);  // Cream

// Text styling
brandedQR.ShowText = true;
brandedQR.TextSpacing = 20;
brandedQR.TextStyle = new BarcodeTextStyle()
{
    FontFamily = "Arial",
    FontSize = 12,
    TextColor = Color.FromRgb(0, 51, 102)
};
```

### Example 3: Compact Inventory Label

```csharp
SfBarcodeGenerator inventoryLabel = new SfBarcodeGenerator();
inventoryLabel.Value = "INV-2024-789";
inventoryLabel.HeightRequest = 80;
inventoryLabel.WidthRequest = 200;
inventoryLabel.Symbology = new Code128() { Module = 1 };

// Minimal styling for space efficiency
inventoryLabel.ForegroundColor = Colors.Black;
inventoryLabel.BackgroundColor = Colors.White;
inventoryLabel.ShowText = true;
inventoryLabel.TextSpacing = 5;
inventoryLabel.TextStyle = new BarcodeTextStyle()
{
    FontSize = 10,
    FontFamily = "Courier New"  // Monospace for technical look
};
```

### Example 4: High-Contrast Accessibility Barcode

```csharp
SfBarcodeGenerator accessibleBarcode = new SfBarcodeGenerator();
accessibleBarcode.Value = "ACCESS-12345";
accessibleBarcode.HeightRequest = 150;
accessibleBarcode.WidthRequest = 300;
accessibleBarcode.Symbology = new Code128() { Module = 3 };  // Larger modules

// Maximum contrast for accessibility
accessibleBarcode.ForegroundColor = Colors.Black;
accessibleBarcode.BackgroundColor = Colors.White;

// Large, bold text
accessibleBarcode.ShowText = true;
accessibleBarcode.TextSpacing = 20;
accessibleBarcode.TextStyle = new BarcodeTextStyle()
{
    FontSize = 18,
    FontAttributes = FontAttributes.Bold,
    TextColor = Colors.Black
};
```

### Example 5: Styled Data Matrix for Labels

```csharp
SfBarcodeGenerator compactLabel = new SfBarcodeGenerator();
compactLabel.Value = "LOT:2024A|EXP:2026-03-19";
compactLabel.HeightRequest = 120;
compactLabel.WidthRequest = 120;
compactLabel.Symbology = new DataMatrix()
{
    Encoding = DataMatrixEncoding.Auto,
    Module = 2
};

// Clean styling
compactLabel.ForegroundColor = Colors.Black;
compactLabel.BackgroundColor = Colors.White;
compactLabel.ShowText = false;  // No text for compact design
```

## Best Practices

### Text Customization Best Practices

1. **Enable ShowText for human-readable applications**
   - Product labels
   - Inventory with manual entry backup
   - Debug/testing scenarios

2. **Use legible fonts**
   - Arial, Helvetica for sans-serif
   - Times New Roman for serif
   - Avoid decorative fonts

3. **Appropriate font sizes**
   - Minimum 10pt for readability
   - 12-14pt for standard labels
   - 16-18pt for accessibility

4. **Contrast text with background**
   - Black text on white background (standard)
   - Match foreground color for consistency

5. **Test text spacing**
   - Ensure text doesn't touch barcode
   - Consider printing bleed/absorption
   - Adjust for barcode height

### Color Customization Best Practices

1. **Maximize contrast**
   - Black on white is optimal
   - Minimum 70% contrast ratio
   - Test with actual scanners

2. **Avoid problematic combinations**
   - Light foreground on light background
   - Red on green (color-blind issues)
   - Pure blue (some scanners struggle)

3. **Test colored barcodes**
   - Print samples before production
   - Test with target scanning hardware
   - Verify across lighting conditions

4. **Consider printing**
   - RGB to CMYK conversion affects colors
   - Test print quality at target DPI
   - Account for paper/material color

5. **Brand colors with caution**
   - Use High error correction for QR codes
   - Increase Module size for colored 1D barcodes
   - Always test scanning reliability

### Module Size Best Practices

1. **For screen display: auto-calculate or small values**
   ```csharp
   // Let system calculate
   barcode.WidthRequest = 300;
   barcode.HeightRequest = 150;
   ```

2. **For printing: explicit Module values**
   ```csharp
   barcode.Symbology = new Code128() { Module = 2 };
   ```

3. **Increase Module for:**
   - Colored barcodes
   - Low-quality printers
   - Long scanning distances
   - Difficult scanning conditions

4. **Decrease Module for:**
   - High-DPI printing
   - Space-constrained labels
   - Large data capacity QR codes

### General Styling Best Practices

1. **Consistency across application**
   - Standardize colors, fonts, spacing
   - Create reusable barcode styles
   - Document styling decisions

2. **Accessibility considerations**
   - High contrast for visual impairments
   - Large text for readability
   - Test with accessibility tools

3. **Platform testing**
   - Test on iOS, Android, Windows
   - Verify font availability
   - Check color rendering

4. **Print preview before production**
   - Always print test samples
   - Verify at target print size
   - Test scanning from prints

5. **Document requirements**
   - Specify exact colors (hex/RGB)
   - Define font sizes and families
   - Note Module sizes for print

## Next Steps

- **[API Reference](api-reference.md)** - Complete property documentation
- **[One-Dimensional Barcodes](one-dimensional-barcodes.md)** - Symbology-specific customization
- **[Two-Dimensional Barcodes](two-dimensional-barcodes.md)** - QR Code and Data Matrix options
- **[Getting Started](getting-started.md)** - Basic implementation guide
