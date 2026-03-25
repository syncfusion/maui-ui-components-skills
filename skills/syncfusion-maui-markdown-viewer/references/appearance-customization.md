# Appearance Customization with MarkdownStyleSettings

## Table of Contents
- [Overview](#overview)
- [MarkdownStyleSettings Class](#markdownstylesettings-class)
- [Heading Customization](#heading-customization)
- [Body Text Styling](#body-text-styling)
- [Table Styling](#table-styling)
- [Complete Styling Example](#complete-styling-example)
- [Platform Considerations](#platform-considerations)
- [Best Practices](#best-practices)
- [Common Styling Patterns](#common-styling-patterns)

## Overview

The `MarkdownStyleSettings` class provides a property-based approach to customizing the visual appearance of Markdown content in SfMarkdownViewer. This allows you to match the Markdown display with your app's theme, improve readability, and create a consistent user experience.

### Key Features

- **Granular Control:** Customize individual elements like headings, body text, and tables
- **Simple API:** Set properties directly without writing CSS
- **Consistent Styling:** Apply uniform styles across all Markdown content
- **Theme Integration:** Easily match your app's color scheme and typography

### When to Use MarkdownStyleSettings

Use `MarkdownStyleSettings` when you need to:
- Match Markdown appearance with your app's design system
- Adjust font sizes for readability on different devices
- Apply brand colors to headings and text
- Style tables to fit your app's aesthetic
- Make quick appearance changes without custom CSS

For more advanced styling needs (hover effects, animations, complex selectors), see [custom-css-styling.md](custom-css-styling.md).

## MarkdownStyleSettings Class

The `MarkdownStyleSettings` class exposes properties for common styling scenarios:

### Property Reference

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `H1FontSize` | `string` | Font size for H1 headings (use "px" unit) | "32px" |
| `H2FontSize` | `string` | Font size for H2 headings (use "px" unit) | "24px" |
| `H3FontSize` | `string` | Font size for H3 headings (use "px" unit) | "20px" |
| `H1Color` | `string` | Text color for H1 headings | "#000000" |
| `H2Color` | `string` | Text color for H2 headings | "#000000" |
| `H3Color` | `string` | Text color for H3 headings | "#000000" |
| `BodyFontSize` | `string` | Font size for paragraph text (use "px" unit) | "16px" |
| `BodyTextColor` | `string` | Text color for body content | "#000000" |
| `TableHeaderFontSize` | `string` | Font size for table headers (use "px" unit) | "16px" |
| `TableDataFontSize` | `string` | Font size for table cells (use "px" unit) | "14px" |
| `TableHeaderTextColor` | `string` | Text color for table headers | "#000000" |
| `TableDataTextColor` | `string` | Text color for table cells | "#000000" |
| `TableBackground` | `string` | Background color for tables | "transparent" |
| `CssStyleRules` | `string` | Raw CSS for advanced styling | null |

**Important:** Font sizes must include the "px" unit (e.g., "24px", not "24" or "24pt").

## Heading Customization

Headings are the primary navigation and structure elements in Markdown documents. Customizing their appearance helps establish visual hierarchy.

### Basic Heading Styling

```xml
<markdown:SfMarkdownViewer Source="{Binding Content}">
    <markdown:SfMarkdownViewer.Settings>
        <markdown:MarkdownStyleSettings 
            H1FontSize="32px"
            H1Color="#2C3E50"
            H2FontSize="26px"
            H2Color="#34495E"
            H3FontSize="22px"
            H3Color="#5D6D7E" />
    </markdown:SfMarkdownViewer.Settings>
</markdown:SfMarkdownViewer>
```

### C# Approach

```csharp
SfMarkdownViewer markdownViewer = new SfMarkdownViewer();
markdownViewer.Settings = new MarkdownStyleSettings
{
    H1FontSize = "32px",
    H1Color = "#2C3E50",
    H2FontSize = "26px",
    H2Color = "#34495E",
    H3FontSize = "22px",
    H3Color = "#5D6D7E"
};
markdownViewer.Source = markdownContent;
```

### Creating Visual Hierarchy

Use size and color differences to create clear hierarchy:

```csharp
// Strong hierarchy with larger size differences
var settings = new MarkdownStyleSettings
{
    H1FontSize = "36px",  // Large
    H1Color = "#1A1A1A",  // Very dark
    
    H2FontSize = "28px",  // Medium-large
    H2Color = "#333333",  // Dark
    
    H3FontSize = "20px",  // Medium
    H3Color = "#666666"   // Medium gray
};
```

### Color Schemes

**Dark Theme:**
```csharp
var darkTheme = new MarkdownStyleSettings
{
    H1Color = "#E1E1E1",
    H2Color = "#C1C1C1",
    H3Color = "#A1A1A1",
    BodyTextColor = "#D1D1D1"
};
```

**Brand Colors:**
```csharp
var brandedTheme = new MarkdownStyleSettings
{
    H1Color = "#FF6B6B",  // Brand primary
    H2Color = "#4ECDC4",  // Brand secondary
    H3Color = "#45B7D1",  // Brand accent
    BodyTextColor = "#2C3E50"
};
```

## Body Text Styling

Body text makes up the majority of Markdown content. Proper styling ensures readability.

### Font Size Selection

```csharp
// Small screens (mobile)
var mobileSettings = new MarkdownStyleSettings
{
    BodyFontSize = "14px"
};

// Medium screens (tablet)
var tabletSettings = new MarkdownStyleSettings
{
    BodyFontSize = "16px"
};

// Large screens (desktop)
var desktopSettings = new MarkdownStyleSettings
{
    BodyFontSize = "18px"
};
```

### Readability Optimization

```csharp
// High contrast for accessibility
var accessibleSettings = new MarkdownStyleSettings
{
    BodyFontSize = "18px",
    BodyTextColor = "#000000"  // Pure black on white
};

// Reduced eye strain
var comfortableSettings = new MarkdownStyleSettings
{
    BodyFontSize = "17px",
    BodyTextColor = "#333333"  // Slightly softer than pure black
};
```

### Responsive Font Sizing

```csharp
public MarkdownStyleSettings GetResponsiveSettings()
{
    var displayInfo = DeviceDisplay.MainDisplayInfo;
    double width = displayInfo.Width / displayInfo.Density;
    
    if (width < 600)  // Mobile
    {
        return new MarkdownStyleSettings
        {
            H1FontSize = "28px",
            H2FontSize = "22px",
            H3FontSize = "18px",
            BodyFontSize = "14px"
        };
    }
    else if (width < 1024)  // Tablet
    {
        return new MarkdownStyleSettings
        {
            H1FontSize = "32px",
            H2FontSize = "26px",
            H3FontSize = "20px",
            BodyFontSize = "16px"
        };
    }
    else  // Desktop
    {
        return new MarkdownStyleSettings
        {
            H1FontSize = "36px",
            H2FontSize = "28px",
            H3FontSize = "22px",
            BodyFontSize = "18px"
        };
    }
}

// Usage
markdownViewer.Settings = GetResponsiveSettings();
```

## Table Styling

Tables benefit from clear visual separation between headers and data.

### Basic Table Styling

```xml
<markdown:SfMarkdownViewer Source="{Binding TableContent}">
    <markdown:SfMarkdownViewer.Settings>
        <markdown:MarkdownStyleSettings 
            TableHeaderFontSize="18px"
            TableHeaderTextColor="#FFFFFF"
            TableDataFontSize="16px"
            TableDataTextColor="#2C3E50"
            TableBackground="#ECF0F1" />
    </markdown:SfMarkdownViewer.Settings>
</markdown:SfMarkdownViewer>
```

### Professional Table Appearance

```csharp
var tableSettings = new MarkdownStyleSettings
{
    // Headers stand out
    TableHeaderFontSize = "17px",
    TableHeaderTextColor = "#1E3A8A",  // Dark blue
    
    // Data is readable
    TableDataFontSize = "15px",
    TableDataTextColor = "#374151",    // Dark gray
    
    // Subtle background
    TableBackground = "#F9FAFB"        // Very light gray
};
```

### Alternating Row Colors

While `MarkdownStyleSettings` sets overall table background, alternating rows require CSS (see [custom-css-styling.md](custom-css-styling.md)). However, you can set a base that works well:

```csharp
var settings = new MarkdownStyleSettings
{
    TableBackground = "#FAFAFA",       // Light background
    TableHeaderTextColor = "#1976D2",  // Blue headers
    TableDataTextColor = "#424242"     // Dark gray data
};
```

## Complete Styling Example

Here's a comprehensive example combining all styling options:

### XAML

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:markdown="clr-namespace:Syncfusion.Maui.MarkdownViewer;assembly=Syncfusion.Maui.MarkdownViewer"
             x:Class="MyApp.DocumentPage">

    <markdown:SfMarkdownViewer Source="{Binding DocumentContent}">
        <markdown:SfMarkdownViewer.Settings>
            <markdown:MarkdownStyleSettings 
                H1FontSize="32px"
                H1Color="#8352FB"
                H2FontSize="26px"
                H2Color="#9971FB"
                H3FontSize="22px"
                H3Color="#A98AF7"
                BodyFontSize="16px"
                BodyTextColor="#2C3E50"
                TableHeaderFontSize="17px"
                TableHeaderTextColor="#8352FB"
                TableDataFontSize="15px"
                TableDataTextColor="#34495E"
                TableBackground="#F8F9FA" />
        </markdown:SfMarkdownViewer.Settings>
    </markdown:SfMarkdownViewer>

</ContentPage>
```

### C# with ViewModel

```csharp
// ViewModel
public class DocumentViewModel : INotifyPropertyChanged
{
    private string _documentContent;
    
    public string DocumentContent
    {
        get => _documentContent;
        set
        {
            _documentContent = value;
            OnPropertyChanged();
        }
    }
    
    public DocumentViewModel()
    {
        DocumentContent = @"
# User Guide

Welcome to our application!

## Getting Started

This guide will help you **get started** quickly.

### Prerequisites

Before you begin, ensure you have:
- Requirement 1
- Requirement 2
- Requirement 3

## Features

| Feature | Description | Status |
|---------|-------------|--------|
| Feature A | Core functionality | Available |
| Feature B | Advanced tools | Available |
| Feature C | Premium features | Coming Soon |

## Conclusion

Thank you for using our application!
";
    }
    
    // INotifyPropertyChanged implementation...
}

// Code-behind
public partial class DocumentPage : ContentPage
{
    public DocumentPage()
    {
        InitializeComponent();
        
        // Alternative: Set styling in code-behind
        var markdownViewer = new SfMarkdownViewer();
        markdownViewer.Settings = new MarkdownStyleSettings
        {
            H1FontSize = "32px",
            H1Color = "#8352FB",
            H2FontSize = "26px",
            H2Color = "#9971FB",
            H3FontSize = "22px",
            H3Color = "#A98AF7",
            BodyFontSize = "16px",
            BodyTextColor = "#2C3E50",
            TableHeaderFontSize = "17px",
            TableHeaderTextColor = "#8352FB",
            TableDataFontSize = "15px",
            TableDataTextColor = "#34495E",
            TableBackground = "#F8F9FA"
        };
        
        markdownViewer.SetBinding(SfMarkdownViewer.SourceProperty, "DocumentContent");
        BindingContext = new DocumentViewModel();
        Content = markdownViewer;
    }
}
```

## Platform Considerations

### Font Size Units

**Always use "px" (pixels)** for consistent cross-platform rendering:

```csharp
// ✅ Correct
H1FontSize = "32px"

// ❌ Incorrect - will not work
H1FontSize = "32"
H1FontSize = "32pt"
H1FontSize = "2em"
```

### Color Formats

Supported color formats:
- Hex: `"#FF5733"`, `"#F57"`
- Named colors: `"Red"`, `"Blue"`, `"HotPink"`
- RGB: `"rgb(255, 87, 51)"`
- RGBA: `"rgba(255, 87, 51, 0.8)"`

```csharp
// All valid
H1Color = "#FF5733"           // Hex 6-digit
H1Color = "#F57"              // Hex 3-digit
H1Color = "DarkBlue"          // Named color
H1Color = "rgb(0, 0, 139)"    // RGB
H1Color = "rgba(0, 0, 139, 0.9)"  // RGBA with transparency
```

### Platform-Specific Defaults

Different platforms may have slightly different default rendering. Always test on target platforms with .NET 9 MAUI:

```csharp
#if ANDROID
    var settings = new MarkdownStyleSettings { BodyFontSize = "15px" };
#elif IOS
    var settings = new MarkdownStyleSettings { BodyFontSize = "16px" };
#elif WINDOWS
    var settings = new MarkdownStyleSettings { BodyFontSize = "14px" };
#endif
```

**Note:** .NET 9 MAUI provides improved cross-platform rendering consistency.

## Best Practices

### 1. Maintain Sufficient Contrast

```csharp
// ✅ Good - high contrast
BodyTextColor = "#2C3E50"  // Dark text on light background

// ❌ Poor - low contrast
BodyTextColor = "#CCCCCC"  // Light gray on white background
```

Use online contrast checkers to ensure WCAG compliance (4.5:1 ratio for normal text).

### 2. Use Consistent Color Palette

```csharp
// Define color constants
public static class AppColors
{
    public const string Primary = "#8352FB";
    public const string Secondary = "#9971FB";
    public const string Accent = "#A98AF7";
    public const string TextDark = "#2C3E50";
    public const string TextMedium = "#34495E";
}

// Use consistently
var settings = new MarkdownStyleSettings
{
    H1Color = AppColors.Primary,
    H2Color = AppColors.Secondary,
    H3Color = AppColors.Accent,
    BodyTextColor = AppColors.TextDark
};
```

### 3. Scale Font Sizes Proportionally

```csharp
// ✅ Good - clear hierarchy
H1FontSize = "32px"  // 2x base
H2FontSize = "24px"  // 1.5x base
H3FontSize = "20px"  // 1.25x base
BodyFontSize = "16px"  // Base

// ❌ Poor - confusing hierarchy
H1FontSize = "18px"
H2FontSize = "20px"  // Larger than H1!
H3FontSize = "24px"  // Even larger!
```

### 4. Test on Multiple Devices

```csharp
// Create test helper
public void TestStyling()
{
    var testContent = @"
# Heading 1
## Heading 2
### Heading 3

Regular paragraph text with **bold** and *italic*.

| Header 1 | Header 2 |
|----------|----------|
| Data 1   | Data 2   |
";
    
    markdownViewer.Source = testContent;
}
```

### 5. Consider Dark Mode

```csharp
public MarkdownStyleSettings GetThemeSettings()
{
    var isDarkMode = Application.Current.RequestedTheme == AppTheme.Dark;
    
    if (isDarkMode)
    {
        return new MarkdownStyleSettings
        {
            H1Color = "#E1E1E1",
            H2Color = "#C9C9C9",
            H3Color = "#B1B1B1",
            BodyTextColor = "#D1D1D1",
            TableBackground = "#2C2C2C",
            TableHeaderTextColor = "#FFFFFF",
            TableDataTextColor = "#D1D1D1"
        };
    }
    else
    {
        return new MarkdownStyleSettings
        {
            H1Color = "#2C3E50",
            H2Color = "#34495E",
            H3Color = "#5D6D7E",
            BodyTextColor = "#2C3E50",
            TableBackground = "#F8F9FA",
            TableHeaderTextColor = "#1E3A8A",
            TableDataTextColor = "#374151"
        };
    }
}

// Apply based on theme
markdownViewer.Settings = GetThemeSettings();

// Update when theme changes
Application.Current.RequestedThemeChanged += (s, e) =>
{
    markdownViewer.Settings = GetThemeSettings();
};
```

## Common Styling Patterns

### Pattern 1: Documentation Style

```csharp
var docStyle = new MarkdownStyleSettings
{
    H1FontSize = "32px",
    H1Color = "#1E40AF",
    H2FontSize = "24px",
    H2Color = "#1E40AF",
    H3FontSize = "20px",
    H3Color = "#3B82F6",
    BodyFontSize = "16px",
    BodyTextColor = "#374151",
    TableBackground = "#F3F4F6"
};
```

### Pattern 2: Modern App Style

```csharp
var modernStyle = new MarkdownStyleSettings
{
    H1FontSize = "36px",
    H1Color = "#10B981",
    H2FontSize = "28px",
    H2Color = "#059669",
    H3FontSize = "22px",
    H3Color = "#047857",
    BodyFontSize = "17px",
    BodyTextColor = "#111827",
    TableHeaderTextColor = "#10B981"
};
```

### Pattern 3: Minimalist Style

```csharp
var minimalStyle = new MarkdownStyleSettings
{
    H1FontSize = "28px",
    H1Color = "#000000",
    H2FontSize = "22px",
    H2Color = "#000000",
    H3FontSize = "18px",
    H3Color = "#000000",
    BodyFontSize = "16px",
    BodyTextColor = "#000000",
    TableBackground = "transparent"
};
```

### Pattern 4: High Contrast (Accessibility)

```csharp
var highContrastStyle = new MarkdownStyleSettings
{
    H1FontSize = "34px",
    H1Color = "#000000",
    H2FontSize = "26px",
    H2Color = "#000000",
    H3FontSize = "20px",
    H3Color = "#000000",
    BodyFontSize = "18px",
    BodyTextColor = "#000000",
    TableHeaderTextColor = "#FFFFFF",
    TableBackground = "#000000"
};
```

## Limitations

`MarkdownStyleSettings` provides simple property-based styling but has limitations:

- **No hover effects** — Requires CSS
- **No animations** — Requires CSS
- **No advanced selectors** — Can't target specific elements
- **Limited table styling** — Can't create striped rows or rounded corners
- **No scrollbar customization** — Requires CSS

For these advanced scenarios, use `CssStyleRules` property. See [custom-css-styling.md](custom-css-styling.md).

## Next Steps

- **Apply advanced CSS styling** → See [custom-css-styling.md](custom-css-styling.md)
- **Load content from different sources** → See [loading-content.md](loading-content.md)
- **Retrieve and convert content** → See [content-retrieval.md](content-retrieval.md)
