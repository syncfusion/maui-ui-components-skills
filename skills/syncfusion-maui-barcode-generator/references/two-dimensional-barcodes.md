# Two-Dimensional Barcodes

## Table of Contents
- [Overview](#overview)
- [QR Code](#qr-code)
  - [Overview](#qr-code-overview)
  - [When to Use](#when-to-use-qr-code)
  - [Basic Implementation](#basic-qr-code-implementation)
  - [Error Correction Level](#error-correction-level)
  - [Input Mode](#input-mode)
  - [Code Version](#code-version)
  - [Module Property](#qr-code-module-property)
  - [Complete Examples](#qr-code-complete-examples)
- [Data Matrix](#data-matrix)
  - [Overview](#data-matrix-overview)
  - [When to Use](#when-to-use-data-matrix)
  - [Basic Implementation](#basic-data-matrix-implementation)
  - [Data Format](#data-matrix-format)
  - [Encoding Methods](#encoding-methods)
  - [Complete Examples](#data-matrix-complete-examples)
- [Comparison: QR Code vs Data Matrix](#comparison-qr-code-vs-data-matrix)
- [Best Practices](#best-practices)

## Overview

Two-dimensional (2D) barcodes represent information using a two-dimensional approach with patterns of squares, dots, or other geometric shapes. Unlike one-dimensional barcodes that store data horizontally, 2D barcodes store data both horizontally and vertically, enabling significantly higher data capacity.

**Key advantages of 2D barcodes:**
- **High data capacity** - Store thousands of characters vs tens in 1D
- **Error correction** - Can be read even when partially damaged
- **Compact size** - More data in less space
- **Omnidirectional scanning** - Can be scanned from any angle
- **Multiple data types** - URLs, text, binary data, contact information

The Syncfusion .NET MAUI Barcode Generator supports **2 two-dimensional symbologies:**
1. **QR Code** - Most popular, used for marketing, URLs, payments
2. **Data Matrix** - Compact, used for electronics, labels, small packaging

## QR Code

### QR Code Overview

QR Code (Quick Response Code) is a two-dimensional matrix barcode consisting of black squares arranged on a white square grid. It was designed for high-speed reading and can store significantly more data than traditional barcodes.

**Key characteristics:**
- **Grid-based:** Square patterns from 21×21 to 177×177 modules
- **Versions:** 40 different sizes (Version 1 to Version 40)
- **Data capacity:** Up to 7,089 numeric or 4,296 alphanumeric characters
- **Error correction:** 4 levels (7%, 15%, 25%, 30% recovery)
- **Character sets:** Numeric, alphanumeric, binary, Kanji
- **Omnidirectional:** Can be scanned from any angle

**Data structure:**
- **Finder patterns** - Three corner squares for orientation
- **Alignment patterns** - For precise positioning
- **Timing patterns** - Coordinate system
- **Data area** - Encoded information
- **Error correction** - Reed-Solomon codes

### When to Use QR Code

**Ideal for:**
- **Marketing campaigns** - URLs to websites, landing pages, promotions
- **Product packaging** - Product information, authenticity verification
- **Event tickets** - Digital tickets, passes, entry codes
- **Payment systems** - Mobile payments, cryptocurrency wallets
- **Contact sharing** - vCards, email addresses, phone numbers
- **Wi-Fi sharing** - Network credentials
- **App downloads** - Deep links to app stores
- **Document tracking** - Version control, asset management
- **Authentication** - 2FA codes, login tokens

**Advantages:**
- Universal smartphone support (camera apps)
- High brand recognition
- Large data capacity
- Resilient to damage with error correction
- Fast scanning from mobile devices

### Basic QR Code Implementation

**XAML:**

```xaml
<barcode:SfBarcodeGenerator Value="https://www.syncfusion.com"
                            ShowText="True"
                            HeightRequest="300"
                            WidthRequest="300">
    <barcode:SfBarcodeGenerator.Symbology>
        <barcode:QRCode />
    </barcode:SfBarcodeGenerator.Symbology>
</barcode:SfBarcodeGenerator>
```

**C#:**

```csharp
SfBarcodeGenerator barcode = new SfBarcodeGenerator();
barcode.HeightRequest = 300;
barcode.WidthRequest = 300;
barcode.Value = "https://www.syncfusion.com";
barcode.Symbology = new QRCode();
barcode.ShowText = true;
```

**Best practice:** Make QR codes square by setting equal HeightRequest and WidthRequest.

### Error Correction Level

The `ErrorCorrectionLevel` property enables QR codes to be readable even when damaged, dirty, or partially obscured. Higher error correction means more redundancy but also larger QR codes.

#### Error Correction Levels

| Level | Recovery Rate | Use Case |
|-------|---------------|----------|
| **Low** | ~7% | Clean environments, digital displays |
| **Medium** | ~15% | Standard use, minimal damage expected |
| **Quartile** | ~25% | Print materials, outdoor use |
| **High** | ~30% | High-damage environments, critical data |
| **Auto** | Variable | Automatic selection based on data |

#### Setting Error Correction

**XAML:**

```xaml
<barcode:SfBarcodeGenerator Value="https://www.syncfusion.com"
                            ShowText="True"
                            HeightRequest="300" 
                            WidthRequest="300">
    <barcode:SfBarcodeGenerator.Symbology>
        <barcode:QRCode ErrorCorrectionLevel="High"/>
    </barcode:SfBarcodeGenerator.Symbology>
</barcode:SfBarcodeGenerator>
```

**C#:**

```csharp
barcode.Symbology = new QRCode() 
{ 
    ErrorCorrectionLevel = ErrorCorrectionLevel.High  // 30% recovery
};
```

#### Error Correction Examples

```csharp
// Low - 7% recovery (smallest QR code, clean environments)
barcode.Symbology = new QRCode() 
{ 
    ErrorCorrectionLevel = ErrorCorrectionLevel.Low 
};

// Medium - 15% recovery (default recommended)
barcode.Symbology = new QRCode() 
{ 
    ErrorCorrectionLevel = ErrorCorrectionLevel.Medium 
};

// Quartile - 25% recovery (print materials)
barcode.Symbology = new QRCode() 
{ 
    ErrorCorrectionLevel = ErrorCorrectionLevel.Quartile 
};

// High - 30% recovery (maximum reliability)
barcode.Symbology = new QRCode() 
{ 
    ErrorCorrectionLevel = ErrorCorrectionLevel.High 
};

// Auto - calculated automatically
barcode.Symbology = new QRCode() 
{ 
    ErrorCorrectionLevel = ErrorCorrectionLevel.Auto 
};
```

#### Choosing Error Correction Level

**Use Low (7%):**
- Digital displays only
- Maximum data capacity needed
- Pristine environments

**Use Medium (15%):**
- General purpose applications
- Standard print materials
- Indoor use

**Use Quartile (25%):**
- Outdoor signage
- Product labels
- Glossy or reflective surfaces

**Use High (30%):**
- Construction sites or industrial environments
- Long-term outdoor exposure
- Critical data that must not fail
- QR codes with logos or graphics overlaid

**Use Auto:**
- When unsure of optimal level
- Dynamic content generation
- Let system optimize for data and version

### Input Mode

The `InputMode` property optimizes how data is encoded in the QR code, affecting size and capacity.

#### Input Modes

| Mode | Character Set | Capacity | Best For |
|------|---------------|----------|----------|
| **Numeric** | 0-9 | 7,089 digits | Phone numbers, IDs, numeric codes |
| **AlphaNumeric** | 0-9, A-Z, space, $%*+-./:  | 4,296 chars | URLs, codes, tracking numbers |
| **Binary** | Full 8-bit bytes, Shift JIS | 2,953 bytes | Any text, special chars, Kanji |
| **Auto** | Automatic selection | Optimized | General use, mixed content |

#### Setting Input Mode

**XAML:**

```xaml
<barcode:SfBarcodeGenerator Value="1263438892737643894930872"
                            ShowText="True"
                            HeightRequest="300"
                            WidthRequest="300">
    <barcode:SfBarcodeGenerator.Symbology>
        <barcode:QRCode InputMode="Numeric"/>
    </barcode:SfBarcodeGenerator.Symbology>
</barcode:SfBarcodeGenerator>
```

**C#:**

```csharp
barcode.Symbology = new QRCode() 
{ 
    InputMode = QRInputMode.Numeric  // Optimize for numbers
};
```

#### Input Mode Examples

```csharp
// Numeric - for phone numbers, IDs, numeric sequences
barcode.Value = "1234567890123456789";
barcode.Symbology = new QRCode() 
{ 
    InputMode = QRInputMode.Numeric 
};

// AlphaNumeric - for URLs, tracking codes, uppercase text
barcode.Value = "HTTPS://EXAMPLE.COM/PRODUCT-12345";
barcode.Symbology = new QRCode() 
{ 
    InputMode = QRInputMode.AlphaNumeric 
};

// Binary - for any text, special characters, mixed case
barcode.Value = "Hello, World! 你好世界 🌍";
barcode.Symbology = new QRCode() 
{ 
    InputMode = QRInputMode.Binary 
};

// Auto - let system choose optimal mode
barcode.Value = "Mixed Content 12345 ABC";
barcode.Symbology = new QRCode() 
{ 
    InputMode = QRInputMode.Auto 
};
```

#### Choosing Input Mode

**Use Numeric when:**
- Data contains only digits (0-9)
- Maximum capacity needed for numbers
- Examples: Phone numbers, credit card numbers, serial numbers

**Use AlphaNumeric when:**
- Data is uppercase alphanumeric
- URLs in uppercase format
- Examples: HTTPS://SITE.COM, PRODUCT-CODE-12345

**Use Binary when:**
- Mixed case text
- Special characters beyond alphanumeric
- Non-Latin characters (Unicode)
- Examples: Email addresses, full URLs, multilingual text

**Use Auto when:**
- Unsure of data format
- Dynamic content
- Mixed numeric and text content

### Code Version

The `CodeVersion` property controls the size and capacity of the QR code. QR codes come in 40 versions, from Version 1 (21×21 modules) to Version 40 (177×177 modules).

#### Version Sizing

| Version | Module Size | Example Capacity (Numeric, Low EC) |
|---------|-------------|-------------------------------------|
| Version 1 | 21×21 | 41 digits |
| Version 5 | 37×37 | 271 digits |
| Version 10 | 57×57 | 652 digits |
| Version 15 | 77×77 | 1,269 digits |
| Version 20 | 97×97 | 2,061 digits |
| Version 25 | 117×117 | 3,057 digits |
| Version 30 | 137×137 | 4,296 digits |
| Version 40 | 177×177 | 7,089 digits |
| **Auto** | Variable | Optimal for data |

**Note:** Each version increases by 4 modules per side.

#### Setting Code Version

**XAML:**

```xaml
<barcode:SfBarcodeGenerator Value="https://www.syncfusion.com"
                            ShowText="True"
                            HeightRequest="300" 
                            WidthRequest="300">
    <barcode:SfBarcodeGenerator.Symbology>
        <barcode:QRCode CodeVersion="Version09"/>
    </barcode:SfBarcodeGenerator.Symbology>
</barcode:SfBarcodeGenerator>
```

**C#:**

```csharp
barcode.Symbology = new QRCode() 
{ 
    CodeVersion = QRCodeVersion.Version09  // 53×53 modules
};
```

#### Version Examples

```csharp
// Version 1 - Smallest (21×21), minimal data
barcode.Symbology = new QRCode() 
{ 
    CodeVersion = QRCodeVersion.Version01 
};

// Version 10 - Medium size (57×57)
barcode.Symbology = new QRCode() 
{ 
    CodeVersion = QRCodeVersion.Version10 
};

// Version 40 - Largest (177×177), maximum data
barcode.Symbology = new QRCode() 
{ 
    CodeVersion = QRCodeVersion.Version40 
};

// Auto - system chooses optimal size (recommended)
barcode.Symbology = new QRCode() 
{ 
    CodeVersion = QRCodeVersion.Auto 
};
```

#### Choosing Code Version

**Use Auto (recommended):**
- Let system select optimal size for data
- Ensures data fits without oversizing
- Best for most applications

**Use specific version when:**
- Consistent QR code size required
- Designing for fixed layout space
- Ensuring minimum size for scanning distance
- Creating aesthetic designs with predictable dimensions

**Best practice:** Use Auto unless you have specific size requirements.

### QR Code Module Property

The `Module` property for QR codes defines the size of the smallest dot (module) in pixels.

**Without Module (auto-sized):**

```csharp
barcode.HeightRequest = 300;
barcode.WidthRequest = 300;
barcode.Symbology = new QRCode();
// Module size calculated to fit 300×300 pixels
```

**With Module (fixed dot size):**

```csharp
barcode.Symbology = new QRCode() { Module = 2 };
// Each module is 2×2 pixels
// Total size = Module × Version size (e.g., 2 × 21 = 42px for Version 1)
```

**Recommended Module values:**
- **Screen display:** 2-4 pixels
- **Mobile scanning:** 3-5 pixels
- **Printed (300 DPI):** 2-3 pixels
- **Printed (600 DPI):** 1-2 pixels
- **Large signage:** 5-10 pixels

### QR Code Complete Examples

#### Example 1: Marketing URL with High Error Correction

```csharp
// Promotional QR code for print materials
SfBarcodeGenerator marketingQR = new SfBarcodeGenerator();
marketingQR.Value = "https://yourcompany.com/spring-sale-2024";
marketingQR.HeightRequest = 300;
marketingQR.WidthRequest = 300;
marketingQR.Symbology = new QRCode()
{
    ErrorCorrectionLevel = ErrorCorrectionLevel.High,  // 30% damage tolerance
    InputMode = QRInputMode.Binary,
    CodeVersion = QRCodeVersion.Auto
};
marketingQR.ShowText = true;
marketingQR.BackgroundColor = Colors.White;
marketingQR.ForegroundColor = Colors.Black;
```

#### Example 2: Numeric ID with Optimal Size

```csharp
// Employee ID badge
SfBarcodeGenerator employeeQR = new SfBarcodeGenerator();
employeeQR.Value = "1234567890123";  // Employee ID
employeeQR.HeightRequest = 200;
employeeQR.WidthRequest = 200;
employeeQR.Symbology = new QRCode()
{
    ErrorCorrectionLevel = ErrorCorrectionLevel.Medium,
    InputMode = QRInputMode.Numeric,  // Smallest size for numeric data
    CodeVersion = QRCodeVersion.Auto
};
employeeQR.ShowText = true;
```

#### Example 3: Wi-Fi Network Credentials

```csharp
// Wi-Fi QR code for guest network
string wifiString = "WIFI:T:WPA;S:GuestNetwork;P:SecurePassword123;;";
SfBarcodeGenerator wifiQR = new SfBarcodeGenerator();
wifiQR.Value = wifiString;
wifiQR.HeightRequest = 300;
wifiQR.WidthRequest = 300;
wifiQR.Symbology = new QRCode()
{
    ErrorCorrectionLevel = ErrorCorrectionLevel.Quartile,  // 25% recovery
    InputMode = QRInputMode.Binary,
    CodeVersion = QRCodeVersion.Auto
};
wifiQR.ShowText = false;  // Don't show password in text
```

#### Example 4: vCard Contact Information

```csharp
// Contact card QR code
string vCard = @"BEGIN:VCARD
VERSION:3.0
FN:John Doe
ORG:Company Name
TEL:+1-555-123-4567
EMAIL:john.doe@company.com
END:VCARD";

SfBarcodeGenerator contactQR = new SfBarcodeGenerator();
contactQR.Value = vCard;
contactQR.HeightRequest = 350;
contactQR.WidthRequest = 350;
contactQR.Symbology = new QRCode()
{
    ErrorCorrectionLevel = ErrorCorrectionLevel.Medium,
    InputMode = QRInputMode.Binary,
    CodeVersion = QRCodeVersion.Auto
};
contactQR.ShowText = false;
```

## Data Matrix

### Data Matrix Overview

Data Matrix is a two-dimensional barcode consisting of black and white modules arranged in a square or rectangular pattern. It's optimized for small marking on products and compact data storage.

**Key characteristics:**
- **Shape:** Square or rectangular grid
- **Sizes:** Multiple standard sizes from 10×10 to 144×144
- **Data capacity:** Up to 3,116 numeric or 2,335 alphanumeric characters
- **Error correction:** Reed-Solomon error correction built-in
- **Compact:** More data density than QR codes
- **Industrial standard:** ECC200 (Error Correction Code 200)

**Data structure:**
- **Finder pattern** - L-shaped solid border
- **Timing patterns** - Alternating modules on opposite borders
- **Data region** - Encoded information
- **Error correction** - Built-in redundancy

### When to Use Data Matrix

**Ideal for:**
- **Electronics marking** - PCB components, ICs, connectors
- **Small product labels** - Pharmaceuticals, cosmetics, medical devices
- **Warehouse tracking** - Small parts, inventory
- **Logistics** - Shipping labels, parcel tracking
- **Aerospace** - Component traceability
- **Automotive** - Parts identification
- **Document management** - File tracking, version control

**Advantages:**
- Very compact size
- High data density
- Readable even when only 20% visible
- Excellent for small products
- Omnidirectional scanning
- No quiet zone required (can print to edge)

### Basic Data Matrix Implementation

**XAML:**

```xaml
<barcode:SfBarcodeGenerator Value="https://www.syncfusion.com"
                            ShowText="True"
                            HeightRequest="300" 
                            WidthRequest="300">
    <barcode:SfBarcodeGenerator.Symbology>
        <barcode:DataMatrix/>
    </barcode:SfBarcodeGenerator.Symbology>
</barcode:SfBarcodeGenerator>
```

**C#:**

```csharp
SfBarcodeGenerator barcode = new SfBarcodeGenerator();
barcode.HeightRequest = 300;
barcode.WidthRequest = 300;
barcode.Value = "https://www.syncfusion.com";
barcode.Symbology = new DataMatrix();
barcode.ShowText = true;
```

### Data Matrix Format

#### Capacity

**Maximum data capacity:**
- **Numeric:** 3,116 digits
- **Alphanumeric:** 2,335 characters
- **Binary:** 1,556 bytes

**Actual capacity** depends on:
- Encoding method used
- Data Matrix size selected
- Data type (numeric, alphanumeric, binary)

#### Supported Data Types

1. **Numeric** - Digits 0-9
2. **AlphaNumeric** - A-Z, 0-9, and common symbols
3. **Binary** - Full 8-bit bytes, any ASCII character

### Encoding Methods

The `Encoding` property determines how data is encoded within the Data Matrix, affecting capacity and efficiency.

#### Encoding Types

| Encoding | Description | Best For |
|----------|-------------|----------|
| **Auto** | Automatic selection | General use, mixed content |
| **ASCII** | Standard ASCII encoding | Alphanumeric text, symbols |
| **ASCIINumeric** | Optimized for digit pairs | Numeric sequences, IDs |
| **Base256** | Binary byte encoding | Binary data, special chars |

#### ASCII Encoding

**Character range:** ASCII 0-127  
**Code word calculation:** ASCII value + 1  
**Best for:** Standard text and symbols

```csharp
barcode.Symbology = new DataMatrix()
{
    Encoding = DataMatrixEncoding.ASCII
};
barcode.Value = "ABC123";
```

#### ASCIINumeric Encoding

**Character range:** Digit pairs 00-99  
**Code word calculation:** Numeric value pair + 130  
**Best for:** Long numeric sequences (highest numeric density)

```csharp
barcode.Symbology = new DataMatrix()
{
    Encoding = DataMatrixEncoding.ASCIINumeric
};
barcode.Value = "1234567890123456";
```

#### Base256 Encoding

**Character range:** ASCII 128-255 (extended ASCII)  
**Code word calculation:** First code = 235, second = ASCII value - 127  
**Best for:** Binary data, extended characters

```csharp
barcode.Symbology = new DataMatrix()
{
    Encoding = DataMatrixEncoding.Base256
};
barcode.Value = "Extended characters ©®™";
```

#### Auto Encoding (Recommended)

Automatically selects the most efficient encoding based on data content.

```csharp
barcode.Symbology = new DataMatrix()
{
    Encoding = DataMatrixEncoding.Auto  // Default
};
```

### Data Matrix Complete Examples

#### Example 1: Serial Number Tracking

```csharp
// Product serial number with alphanumeric encoding
SfBarcodeGenerator serialBarcode = new SfBarcodeGenerator();
serialBarcode.Value = "SN:2024-ABC-12345678";
serialBarcode.HeightRequest = 150;
serialBarcode.WidthRequest = 150;
serialBarcode.Symbology = new DataMatrix()
{
    Encoding = DataMatrixEncoding.Auto
};
serialBarcode.ShowText = true;
serialBarcode.ForegroundColor = Colors.Black;
serialBarcode.BackgroundColor = Colors.White;
```

#### Example 2: Pharmaceutical Label

```csharp
// Drug identification with lot and expiry date
string pharmaData = "NDC:12345-678-90|LOT:2024A123|EXP:2026-03-19";
SfBarcodeGenerator pharmaBarcode = new SfBarcodeGenerator();
pharmaBarcode.Value = pharmaData;
pharmaBarcode.HeightRequest = 200;
pharmaBarcode.WidthRequest = 200;
pharmaBarcode.Symbology = new DataMatrix()
{
    Encoding = DataMatrixEncoding.ASCII
};
pharmaBarcode.ShowText = false;
```

#### Example 3: Component Tracking (Numeric)

```csharp
// PCB component with numeric ID
SfBarcodeGenerator componentBarcode = new SfBarcodeGenerator();
componentBarcode.Value = "1234567890123456789012";
componentBarcode.HeightRequest = 100;
componentBarcode.WidthRequest = 100;
componentBarcode.Symbology = new DataMatrix()
{
    Encoding = DataMatrixEncoding.ASCIINumeric,  // Optimal for numeric
    Module = 2
};
componentBarcode.ShowText = false;
```

#### Example 4: Logistics Label with Multiple Fields

```csharp
// Shipping package with structured data
string logisticsData = "SHIP:US-2024-0319|FROM:NYC|TO:LAX|WT:25.5kg|DIM:50x40x30cm";
SfBarcodeGenerator logisticsBarcode = new SfBarcodeGenerator();
logisticsBarcode.Value = logisticsData;
logisticsBarcode.HeightRequest = 250;
logisticsBarcode.WidthRequest = 250;
logisticsBarcode.Symbology = new DataMatrix()
{
    Encoding = DataMatrixEncoding.Auto
};
logisticsBarcode.ShowText = true;
logisticsBarcode.TextSpacing = 10;
```

## Comparison: QR Code vs Data Matrix

| Feature | QR Code | Data Matrix |
|---------|---------|-------------|
| **Shape** | Always square | Square or rectangular |
| **Data capacity (numeric)** | 7,089 | 3,116 |
| **Data capacity (alphanumeric)** | 4,296 | 2,335 |
| **Size** | Larger for same data | More compact |
| **Error correction** | 4 levels (7-30%) | Built-in (varies by size) |
| **Scanning** | Smartphones (universal) | Industrial scanners (common) |
| **Finder pattern** | 3 corner squares | L-shaped border |
| **Quiet zone** | Required | Not required |
| **Best for** | Marketing, consumer apps | Industrial, small labels |
| **Recognition** | High (consumer familiarity) | Low (industrial use) |
| **Print quality** | More forgiving | Requires precision |
| **Minimum size** | Larger | Smaller |

### When to Choose QR Code

- Consumer-facing applications
- Smartphone scanning required
- Marketing and promotional materials
- URLs and web links
- Large data capacity needed
- High damage/error tolerance required

### When to Choose Data Matrix

- Industrial applications
- Space-constrained labels
- Electronics marking
- Professional scanning equipment available
- No quiet zone space available
- Small product marking (pharmaceuticals, cosmetics)
- High-density data in minimal space

## Best Practices

### QR Code Best Practices

1. **Always use square dimensions** for proper scanning
   ```csharp
   barcode.HeightRequest = 300;
   barcode.WidthRequest = 300;  // Equal dimensions
   ```

2. **Choose error correction based on environment**
   - Digital: Low or Medium
   - Print: Medium or Quartile
   - Outdoor/Critical: High

3. **Use appropriate input mode**
   - Numeric for phone numbers, IDs
   - Auto for general use
   - Binary for URLs with mixed case

4. **Test on actual devices** before printing
   - Scan with multiple phones
   - Test at intended scanning distance
   - Verify in different lighting conditions

5. **Maintain adequate quiet zone** (white space around code)
   - Minimum: 4 modules width
   - Add padding in layout

6. **Optimize URLs** to reduce QR code size
   - Use URL shorteners
   - Remove unnecessary parameters

### Data Matrix Best Practices

1. **Use for compact spaces** where QR codes are too large

2. **Choose encoding based on data type**
   - ASCIINumeric for pure numeric (most efficient)
   - ASCII for standard text
   - Auto for mixed content

3. **Test with target scanner equipment**
   - Data Matrix requires quality scanning hardware
   - May not work with smartphone cameras

4. **Use high-contrast colors**
   - Black on white recommended
   - Avoid colored backgrounds

5. **Consider print resolution**
   - Minimum 300 DPI for reliable scanning
   - 600 DPI preferred for small Data Matrix codes

### General 2D Barcode Best Practices

1. **Set explicit Module size for printing**
   ```csharp
   barcode.Symbology = new QRCode() { Module = 2 };
   ```

2. **Use high contrast**
   ```csharp
   barcode.ForegroundColor = Colors.Black;
   barcode.BackgroundColor = Colors.White;
   ```

3. **Test before deployment**
   - Multiple devices/scanners
   - Various distances
   - Different angles

4. **Provide fallback** for critical data
   - Display text value with ShowText
   - Include manual entry option

5. **Consider data permanence**
   - URLs might change (use redirects)
   - Include expiry dates if time-sensitive

6. **Validate data before encoding**
   - Check format correctness
   - Ensure character set compatibility
   - Test for appropriate length

## Next Steps

- **[Customization](customization.md)** - Style 2D barcodes with colors, fonts, and sizing
- **[API Reference](api-reference.md)** - Complete property documentation for QR Code and Data Matrix
- **[One-Dimensional Barcodes](one-dimensional-barcodes.md)** - Learn about linear barcode types
