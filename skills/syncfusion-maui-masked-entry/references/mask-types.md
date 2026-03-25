# Mask Types in .NET MAUI Masked Entry

The Masked Entry control supports two mask types: **Simple** and **RegEx**. Each type uses different mask elements to create input patterns.

## Table of Contents
- [Overview](#overview)
- [Simple Mask Type](#simple-mask-type)
- [RegEx Mask Type](#regex-mask-type)
- [Choosing Between Simple and RegEx](#choosing-between-simple-and-regex)
- [Performance Considerations](#performance-considerations)

## Overview

The `MaskType` property determines which set of mask elements can be used:

```csharp
maskedEntry.MaskType = MaskedEntryMaskType.Simple;  // Fixed-length patterns
// OR
maskedEntry.MaskType = MaskedEntryMaskType.RegEx;   // Flexible patterns
```

## Simple Mask Type

Simple masks are best for **fixed-length** inputs with predictable patterns like phone numbers, dates, and social security numbers.

### Simple Mask Elements

| Element | Description | Example |
|---------|-------------|---------|
| **0** | Digit required (0-9) | `000` = "123" |
| **9** | Digit or space, optional | `999` = "1  " or "123" |
| **#** | Digit, space, +, or -, optional | `###` = "+12" or "  3" |
| **L** | Letter required (a-z, A-Z) | `LLL` = "ABC" |
| **?** | Letter optional (a-z, A-Z) | `???` = "A  " or "ABC" |
| **C** | Character optional (any) | `CCC` = "A1@" |
| **A** | Alphanumeric required (a-z, A-Z, 0-9) | `AAA` = "A1B" |
| **<** | Shift down (convert to lowercase) | `<AAA` = "abc" |
| **>** | Shift up (convert to uppercase) | `>aaa` = "ABC" |

**Literal Characters:** Any character not in the table above is treated as a literal (separator) and automatically inserted.

### Simple Mask Examples

#### 1. Phone Number

```xml
<editors:SfMaskedEntry 
    MaskType="Simple"
    Mask="(000) 000-0000" />
```

- Input: `5551234567`
- Display: `(555) 123-4567`
- Literals: `(`, `)`, space, `-` are automatically inserted

#### 2. Date (MM/DD/YYYY)

```xml
<editors:SfMaskedEntry 
    MaskType="Simple"
    Mask="00/00/0000" />
```

- Input: `12252024`
- Display: `12/25/2024`
- Literals: `/` characters are automatically inserted

#### 3. Social Security Number

```xml
<editors:SfMaskedEntry 
    MaskType="Simple"
    Mask="000-00-0000" />
```

- Input: `123456789`
- Display: `123-45-6789`

#### 4. Time (HH:MM:SS)

```xml
<editors:SfMaskedEntry 
    MaskType="Simple"
    Mask="00:00:00" />
```

- Input: `143025`
- Display: `14:30:25`

#### 5. Credit Card

```xml
<editors:SfMaskedEntry 
    MaskType="Simple"
    Mask="0000-0000-0000-0000" />
```

- Input: `1234567890123456`
- Display: `1234-5678-9012-3456`

#### 6. Product Key (Uppercase)

```xml
<editors:SfMaskedEntry 
    MaskType="Simple"
    Mask=">AAAAA-AAAAA-AAAAA-AAAAA" />
```

- Input: `ab12cde34fgh56ijk78lmn90`
- Display: `AB12C-DE34F-GH56I-JK78L-MN90`
- The `>` shifts all following characters to uppercase

#### 7. License Plate

```xml
<editors:SfMaskedEntry 
    MaskType="Simple"
    Mask=">LLL-0000" />
```

- Input: `abc1234`
- Display: `ABC-1234`

#### 8. Postal Code (Optional Extension)

```xml
<editors:SfMaskedEntry 
    MaskType="Simple"
    Mask="00000-9999" />
```

- Input: `12345` or `123456789`
- Display: `12345-    ` or `12345-6789`
- The `9` makes the extension optional

### Case Conversion

#### Lowercase Conversion

```xml
<editors:SfMaskedEntry 
    MaskType="Simple"
    Mask="<AAAA" />
```

- Input: `TEST`
- Display: `test`

#### Uppercase Conversion

```xml
<editors:SfMaskedEntry 
    MaskType="Simple"
    Mask=">aaaa" />
```

- Input: `test`
- Display: `TEST`

#### Mixed Case

```xml
<editors:SfMaskedEntry 
    MaskType="Simple"
    Mask=">LL<LL" />
```

- Input: `ABCD`
- Display: `ABcd`

## RegEx Mask Type

RegEx masks provide **flexible, variable-length** patterns using regular expression syntax. Best for complex patterns like emails, URLs, and custom formats.

### RegEx Mask Elements

| Element | Description | Example |
|---------|-------------|---------|
| **[ABC]** | Any single character from set | `[AEI]` = "A" or "E" or "I" |
| **[^ABC]** | Any character NOT in set | `[^AEI]` = "B", "C", "D", etc. |
| **[0-9A-Z]** | Any character in range | `[0-9]` = any digit |
| **\d** | Any digit (same as [0-9]) | `\d\d\d` = "123" |
| **\D** | Any non-digit | `\D` = "A", "b", "@" |
| **\w** | Word character (a-zA-Z_0-9) | `\w+` = "Test_123" |
| **\W** | Non-word character | `\W` = "@", "!", " " |
| **\s** | Whitespace character | `\s` = space, tab |
| **\S** | Non-whitespace character | `\S+` = "NoSpaces" |
| **{n}** | Exactly n occurrences | `\d{3}` = "123" |
| **{n,}** | n or more occurrences | `\d{3,}` = "123" or "12345" |
| **{n,m}** | Between n and m occurrences | `\d{3,5}` = "123" to "12345" |
| **+** | One or more | `\d+` = "1" or "123" |
| **\*** | Zero or more | `\d*` = "" or "123" |
| **?** | Zero or one (optional) | `\d?` = "" or "1" |
| **\|** | OR operator | `cat\|dog` = "cat" or "dog" |
| **.** | Any character | `.+` = any text |
| **(?=ABC)** | Positive lookahead | Look ahead without including |
| **(?!ABC)** | Negative lookahead | Exclude pattern |

**Note:** In XAML, escape backslashes: `\\d` instead of `\d`

### RegEx Mask Examples

#### 1. Email Address

```xml
<editors:SfMaskedEntry 
    MaskType="RegEx"
    Mask="[A-Za-z0-9._%-]+@[A-Za-z0-9]+\.[A-Za-z]{2,3}" />
```

```csharp
MaskType = MaskedEntryMaskType.RegEx,
Mask = "[A-Za-z0-9._%-]+@[A-Za-z0-9]+\\.[A-Za-z]{2,3}"
```

- Accepts: `user@domain.com`, `john_doe@example.co`
- Pattern breakdown:
  - `[A-Za-z0-9._%-]+` - Username (one or more valid chars)
  - `@` - Literal @ symbol
  - `[A-Za-z0-9]+` - Domain name
  - `\.` - Literal dot
  - `[A-Za-z]{2,3}` - TLD (2-3 letters)

#### 2. Alphanumeric Code

```xml
<editors:SfMaskedEntry 
    MaskType="RegEx"
    Mask="[A-Z]{3}[0-9]{4}" />
```

- Accepts: `ABC1234`
- Pattern: 3 uppercase letters + 4 digits

#### 3. Flexible Phone Number

```xml
<editors:SfMaskedEntry 
    MaskType="RegEx"
    Mask="\d{3}-?\d{3}-?\d{4}" />
```

- Accepts: `5551234567`, `555-123-4567`, `555-1234567`
- The `?` makes dashes optional

#### 4. Password (Min 8 Characters)

```xml
<editors:SfMaskedEntry 
    MaskType="RegEx"
    Mask="\w{8,}" />
```

- Accepts: Any word character, minimum 8
- Examples: `Password1`, `MyP@ssw0rd123`

#### 5. IP Address

```xml
<editors:SfMaskedEntry 
    MaskType="RegEx"
    Mask="\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}" />
```

- Accepts: `192.168.1.1`, `10.0.0.255`
- Pattern: 1-3 digits, dot, 1-3 digits, dot, etc.

#### 6. Hex Color Code

```xml
<editors:SfMaskedEntry 
    MaskType="RegEx"
    Mask="#[0-9A-Fa-f]{6}" />
```

- Accepts: `#FF5733`, `#00aaff`
- Pattern: # followed by 6 hex digits

#### 7. Variable-Length Code

```xml
<editors:SfMaskedEntry 
    MaskType="RegEx"
    Mask="[A-Z0-9]{4,10}" />
```

- Accepts: `AB12` to `AB12CD34EF`
- Length: 4 to 10 characters

#### 8. Username (Letters, Numbers, Underscore)

```xml
<editors:SfMaskedEntry 
    MaskType="RegEx"
    Mask="[A-Za-z][A-Za-z0-9_]{2,19}" />
```

- Must start with letter
- 3-20 total characters
- Accepts: `john_doe`, `User123`

## Choosing Between Simple and RegEx

### Use Simple When:

✅ Input has **fixed length** (phone, SSN, date)  
✅ Pattern is **straightforward** with clear positions  
✅ You need **automatic separators** (dashes, parentheses)  
✅ **Performance** is critical (Simple is faster)  
✅ Pattern uses **literal characters** extensively  

**Examples:** Phone numbers, dates, credit cards, postal codes, product keys

### Use RegEx When:

✅ Input has **variable length** (email, URL, username)  
✅ Pattern requires **complex validation** (character sets, lookaheads)  
✅ You need **flexible matching** (optional characters)  
✅ Input requires **advanced patterns** (OR conditions, ranges)  
✅ **Flexibility** is more important than performance  

**Examples:** Emails, passwords, IP addresses, usernames, custom codes

## Performance Considerations

### Simple Mask Performance

- **Faster** than RegEx
- Direct character matching
- Efficient for fixed-length inputs
- Minimal overhead

**Best for:** High-frequency input, mobile devices, large forms

### RegEx Mask Performance

- **Slower** than Simple (due to regex engine)
- More processing overhead
- Complex patterns can impact performance
- Still performant for typical use cases

**Best for:** Complex validation, variable-length inputs, flexibility requirements

### Optimization Tips

1. **Choose Simple When Possible**: If your pattern fits Simple mask, use it
2. **Simplify RegEx Patterns**: Avoid unnecessary complexity
3. **Test on Target Devices**: Verify performance on actual hardware
4. **Limit Pattern Complexity**: Complex lookaheads can be expensive

## Common Patterns Reference

### Simple Mask Patterns

```
Phone (US):          (000) 000-0000
Phone (Intl):        +00 000 000 0000
Date (US):           00/00/0000
Date (ISO):          0000-00-00
Time:                00:00:00
SSN:                 000-00-0000
Credit Card:         0000-0000-0000-0000
ZIP Code:            00000-9999
Product Key:         >AAAAA-AAAAA-AAAAA
License Plate:       >LLL-0000
```

### RegEx Mask Patterns

```csharp
Email:              "[A-Za-z0-9._%-]+@[A-Za-z0-9]+\\.[A-Za-z]{2,3}"
IP Address:         "\\d{1,3}\\.\\d{1,3}\\.\\d{1,3}\\.\\d{1,3}"
Hex Color:          "#[0-9A-Fa-f]{6}"
Username:           "[A-Za-z][A-Za-z0-9_]{2,19}"
Password (8+ chars): "\\w{8,}"
Alphanumeric:       "[A-Z0-9]+"
Variable Code:      "[A-Z0-9]{4,10}"
```

## Edge Cases and Gotchas

### Simple Masks

⚠️ **Issue:** Optional elements (`9`, `?`, `C`) can leave spaces  
**Solution:** Use validation to check for complete input

⚠️ **Issue:** Case conversion affects all following characters  
**Solution:** Use `<` or `>` carefully; consider separate controls

### RegEx Masks

⚠️ **Issue:** XAML requires escaping backslashes (`\\d` not `\d`)  
**Solution:** Always use `\\` in XAML, single `\` in C#

⚠️ **Issue:** Complex patterns can be hard to debug  
**Solution:** Test patterns in a regex tester first

⚠️ **Issue:** Pattern validation doesn't guarantee semantic correctness  
**Solution:** Combine with additional validation (e.g., valid date range)

## Next Steps

- **Value Formatting:** Learn how to control value output → [value-formatting.md](value-formatting.md)
- **Validation:** Implement validation and handle events → [validation-and-events.md](validation-and-events.md)
- **Customization:** Style your masked entry → [customization.md](customization.md)
