---
name: syncfusion-maui-barcode-generator
description: Implements Syncfusion .NET MAUI Barcode Generator (SfBarcodeGenerator) for generating machine-readable barcodes. Use when working with barcodes, barcode generation, QR codes, data matrix, EAN codes, UPC codes, Code128, or Code39. This skill covers both one-dimensional (linear) and two-dimensional (matrix) barcode symbologies with extensive customization options.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Barcode Generator

The Syncfusion .NET MAUI Barcode Generator (SfBarcodeGenerator) is a data visualization control that generates and displays data in machine-readable formats. It provides a comprehensive approach to encode text using various barcode symbology types, supporting both one-dimensional (linear) and two-dimensional (matrix) barcodes.

## When to Use This Skill

Use this skill when you need to:

- **Generate barcodes** for retail products, inventory, shipping labels, or identification
- **Create QR codes** for URLs, contact information, Wi-Fi credentials, or payment systems
- **Encode data visually** in machine-readable formats for scanning applications
- **Support multiple symbologies** in a single application (product codes, batch tracking, etc.)
- **Customize barcode appearance** to match branding or readability requirements
- **Display barcodes** in .NET MAUI applications across Windows, macOS, Android, and iOS
- **Implement scanning solutions** where barcodes need to be generated and displayed
- **Add data matrix codes** for small packaging or high-density data encoding

## Component Overview

The Barcode Generator control enables you to create barcodes for various industry-standard formats including retail (UPC, EAN), inventory management (Code128, Code39), logistics (Codabar), and modern 2D codes (QR Code, Data Matrix). It offers extensive customization for appearance, text display, and barcode sizing.

**Key capabilities:**
- **13 barcode symbologies** - 11 one-dimensional + 2 two-dimensional types
- **One-dimensional barcodes** - Code128, EAN8, EAN13, UPC-A, UPC-E, Code39, Code39 Extended, Code93, Codabar, and Code128 variants (A, B, C)
- **Two-dimensional barcodes** - QR Code with error correction, Data Matrix with multiple encodings
- **Visual customization** - Foreground/background colors, bar/dot sizing, text styling
- **Text display** - Show input values with custom fonts, colors, spacing, and alignment
- **Flexible sizing** - Module property for precise control, auto-sizing for optimal display

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

Use this reference when setting up the Barcode Generator for the first time:
- NuGet package installation (`Syncfusion.Maui.Barcode`)
- Handler registration in MauiProgram.cs
- Basic barcode initialization and namespace imports
- Setting default symbology (Code128) or QR Code
- Displaying input values with ShowText property
- Multi-IDE setup (Visual Studio, VS Code, JetBrains Rider)

### One-Dimensional Barcodes
📄 **Read:** [references/one-dimensional-barcodes.md](references/one-dimensional-barcodes.md)

Use this reference when working with linear/1D barcodes:
- **Codabar** - Libraries, blood banks, numeric + special characters
- **Code39** - Alphanumeric with optional check digit, self-checking
- **Code39 Extended** - Full ASCII support including lowercase
- **Code93** - Enhanced Code39 with higher density
- **Code128** - High efficiency, full ASCII, automatic character set selection
- **Code128A** - Uppercase, control characters (ASCII 0-95)
- **Code128B** - Upper/lowercase, standard keyboard characters (ASCII 32-127)
- **Code128C** - Numeric pairs (00-99) for dense numeric encoding
- **UPC-A** - 12-digit retail product codes (North America)
- **UPC-E** - Zero-suppressed UPC for small packaging
- **EAN-13** - 13-digit international product codes
- **EAN-8** - 8-digit short product codes
- Module property for bar width control

### Two-Dimensional Barcodes
📄 **Read:** [references/two-dimensional-barcodes.md](references/two-dimensional-barcodes.md)

Use this reference when working with 2D matrix barcodes:
- **QR Code** - Square grid barcodes for URLs, text, large data capacity
  - Error correction levels (Low 7%, Medium 15%, Quartile 25%, High 30%, Auto)
  - Input modes (Numeric, AlphaNumeric, Binary, Auto)
  - Code versions (1-40, scaling from 21×21 to 177×177 modules)
  - Reading damaged codes with error correction
- **Data Matrix** - Rectangular/square grid for labels, small packaging
  - Data types (Numeric, AlphaNumeric, Binary)
  - Encoding methods (Auto, ASCII, ASCIINumeric, Base256)
  - Capacity up to 2335 alphanumeric or 3116 numeric characters
- Module property for dot/cell size control

### Customization and Styling
📄 **Read:** [references/customization.md](references/customization.md)

Use this reference when customizing barcode appearance:
- **Text customization**
  - ShowText property to display input values
  - TextStyle (font family, size, color, attributes like italic/bold)
  - TextSpacing (gap between barcode and text)
  - TextAlignment (Start, Center, End)
- **Bar/dot customization**
  - Module property (size of smallest bar line or dot)
  - ForegroundColor (bar/dot color)
  - BackgroundColor (barcode background)
  - Behavior differences for 1D vs 2D barcodes
  - Auto-sizing when Module is not set