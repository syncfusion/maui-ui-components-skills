# Custom CSS Styling for Advanced Theming

## Table of Contents
- [Overview](#overview)
- [Understanding CSS in MarkdownViewer](#understanding-css-in-markdownviewer)
- [Defining Custom CSS](#defining-custom-css)
- [Applying CSS to MarkdownViewer](#applying-css-to-markdownviewer)
- [Styling Images](#styling-images)
- [Styling Tables](#styling-tables)
- [Styling Scrollbars](#styling-scrollbars)
- [Complete Branded Example](#complete-branded-example)
- [CSS Best Practices](#css-best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

While `MarkdownStyleSettings` properties provide simple styling capabilities, the `CssStyleRules` property unlocks advanced theming with full CSS control. This allows you to create sophisticated, branded Markdown presentations with effects like shadows, borders, animations, and responsive layouts.

### When to Use Custom CSS

Use `CssStyleRules` when you need:
- **Advanced styling effects** — Shadows, borders, gradients, rounded corners
- **Hover and focus states** — Interactive styling
- **Complex selectors** — Target specific elements or patterns
- **Responsive design** — Media queries for different screen sizes
- **Striped table rows** — Alternating colors
- **Scrollbar customization** — Visibility, color, width
- **Image effects** — Borders, shadows, sizing constraints
- **Custom typography** — Font families, weights, line heights

### When to Use MarkdownStyleSettings Instead

Use property-based styling for:
- Simple color and size changes
- Quick theme adjustments
- Basic customization without CSS knowledge
- Scenarios where CSS might be overkill

## Understanding CSS in MarkdownViewer

### Two-Layer Styling System

SfMarkdownViewer uses a two-layer approach:

1. **Base Layer:** Default styles + `MarkdownStyleSettings` properties
2. **Override Layer:** `CssStyleRules` (takes precedence)

```csharp
// Both can be used together
var settings = new MarkdownStyleSettings
{
    BodyFontSize = "16px",  // Base setting
    CssStyleRules = @"
        body {
            line-height: 1.8;  // CSS override/addition
        }
    "
};
```

### CSS Precedence Rules

When both property and CSS define the same style:

```csharp
var settings = new MarkdownStyleSettings
{
    H1Color = "#000000",  // This will be overridden
    CssStyleRules = @"
        h1 {
            color: #FF0000;  // This wins
        }
    "
};
```

**Rule:** CSS always takes precedence over properties.

### Supported CSS Features

✅ **Supported:**
- Colors, fonts, sizes
- Borders, margins, padding
- Backgrounds
- Text styling
- Table styling
- Pseudo-classes (`:hover`, `:nth-child`)
- Pseudo-elements (`::before`, `::after`)
- Webkit scrollbar selectors

⚠️ **Limited Support:**
- Animations (basic support)
- Transitions (basic support)
- Media queries (platform-dependent)

❌ **Not Supported:**
- External fonts (must be bundled with app)
- External images in CSS (use inline images in Markdown)
- JavaScript integration

## Defining Custom CSS

### Method 1: Inline in C#

```csharp
var settings = new MarkdownStyleSettings
{
    CssStyleRules = @"
        body {
            background: #F5F7FA;
            font-family: 'Segoe UI', sans-serif;
            color: #2E2E2E;
        }
        
        h1 {
            color: #1E3A8A;
            font-size: 30px;
            border-bottom: 2px solid #3B82F6;
            padding-bottom: 8px;
        }
    "
};
```

### Method 2: XAML Resource Dictionary

```xml
<ContentPage.Resources>
    <ResourceDictionary>
        <x:String x:Key="CustomMarkdownStyle">
        body {
            background: #F5F7FA;
            font-family: 'Segoe UI', sans-serif;
            font-size: 16px;
            color: #2E2E2E;
            line-height: 1.7;
        }

        h1 {
            font-weight: 700;
            font-size: 30px;
            line-height: 38px;
            letter-spacing: 0.5px;
            color: #1E3A8A;
            margin-bottom: 16px;
        }

        h2 {
            font-weight: 600;
            font-size: 24px;
            line-height: 32px;
            letter-spacing: 0.4px;
            color: #3B5BAA;
            margin-top: 24px;
            margin-bottom: 12px;
        }

        h3 {
            font-weight: 500;
            font-size: 20px;
            line-height: 28px;
            letter-spacing: 0.3px;
            color: #6C83C1;
            margin-top: 20px;
            margin-bottom: 10px;
        }
        
        p {
            margin-bottom: 16px;
            line-height: 1.8;
        }
        
        code {
            background: #F1F5F9;
            padding: 2px 6px;
            border-radius: 4px;
            font-family: 'Consolas', monospace;
            font-size: 14px;
        }
        </x:String>
    </ResourceDictionary>
</ContentPage.Resources>
```

### Method 3: Load from File

```csharp
public async Task<string> LoadCssFromFileAsync()
{
    using Stream stream = await FileSystem.OpenAppPackageFileAsync("Styles/markdown.css");
    using StreamReader reader = new StreamReader(stream);
    return await reader.ReadToEndAsync();
}

// Usage
string css = await LoadCssFromFileAsync();
markdownViewer.Settings = new MarkdownStyleSettings
{
    CssStyleRules = css
};
```

## Applying CSS to MarkdownViewer

### XAML Approach

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:markdown="clr-namespace:Syncfusion.Maui.MarkdownViewer;assembly=Syncfusion.Maui.MarkdownViewer"
             x:Class="MyApp.MainPage">
    
    <ContentPage.Resources>
        <ResourceDictionary>
            <x:String x:Key="CustomStyle">
            body {
                background: #FFFFFF;
                color: #2C3E50;
                font-size: 16px;
                padding: 20px;
            }
            h1 {
                color: #E74C3C;
                border-bottom: 3px solid #E74C3C;
            }
            </x:String>
        </ResourceDictionary>
    </ContentPage.Resources>

    <markdown:SfMarkdownViewer Source="{Binding Content}">
        <markdown:SfMarkdownViewer.Settings>
            <markdown:MarkdownStyleSettings 
                CssStyleRules="{StaticResource CustomStyle}" />
        </markdown:SfMarkdownViewer.Settings>
    </markdown:SfMarkdownViewer>

</ContentPage>
```

### C# Approach

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        var markdownViewer = new SfMarkdownViewer();
        markdownViewer.Settings = new MarkdownStyleSettings
        {
            CssStyleRules = @"
                body {
                    background: #FFFFFF;
                    color: #2C3E50;
                    font-size: 16px;
                    padding: 20px;
                }
                h1 {
                    color: #E74C3C;
                    border-bottom: 3px solid #E74C3C;
                }
            "
        };
        markdownViewer.Source = markdownContent;
        Content = markdownViewer;
    }
}
```

## Styling Images

Images in Markdown can be styled using the `img` selector.

### Basic Image Styling

```css
img {
    max-width: 100%;
    height: auto;
    border-radius: 8px;
    border: 2px solid #E0E0E0;
}
```

### Image with Shadow Effect

```css
img {
    border-radius: 12px;
    border: 1px solid #E0E3EA;
    max-width: 95%;
    box-shadow: 0 4px 16px rgba(31, 45, 61, 0.15);
    margin: 16px auto;
    display: block;
}
```

### Responsive Images

```css
img {
    max-width: 100%;
    height: auto;
    border-radius: 8px;
}

/* For large screens */
@media (min-width: 1024px) {
    img {
        max-width: 80%;
        margin: 20px auto;
        display: block;
    }
}
```

### Image Hover Effect

```css
img {
    border-radius: 8px;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
}

img:hover {
    transform: scale(1.02);
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.2);
}
```

### Complete Image Styling Example

```csharp
var settings = new MarkdownStyleSettings
{
    CssStyleRules = @"
        img {
            border-radius: 12px;
            border: 8px solid #e0e3ea;
            max-width: 95%;
            box-shadow: 0 4px 16px rgba(31, 45, 61, 0.75);
            margin: 16px 16px;
            transition: transform 0.2s;
        }
        
        img:hover {
            transform: scale(1.03);
            box-shadow: 0 6px 20px rgba(31, 45, 61, 0.9);
        }
    "
};
```

## Styling Tables

Tables can be extensively customized with CSS for professional appearance.

### Basic Table Styling

```css
table {
    border-collapse: collapse;
    width: 100%;
    margin: 16px 0;
}

th, td {
    border: 1px solid #E0E4EA;
    padding: 12px;
    text-align: left;
}

th {
    background: #F3F4F6;
    font-weight: 600;
}
```

### Striped Table Rows

```css
table {
    background: #FAFBFC;
    border-collapse: collapse;
    margin: 16px 0;
    width: 100%;
}

th, td {
    border: 1px solid #E0E4EA;
    padding: 10px 16px;
    text-align: left;
}

th {
    background: #EDF2FB;
    color: #193466;
    font-size: 17px;
    font-weight: 600;
}

tr:nth-child(even) {
    background: #F5F7FB;
}

tr:nth-child(odd) {
    background: #FFFFFF;
}
```

### Modern Table with Rounded Corners

```css
table {
    background: #FFFFFF;
    border-collapse: collapse;
    border-radius: 8px;
    overflow: hidden;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    margin: 20px 0;
    width: 100%;
}

th, td {
    padding: 14px 18px;
    text-align: left;
    border-bottom: 1px solid #E5E7EB;
}

th {
    background: #F9FAFB;
    color: #111827;
    font-weight: 600;
    border-bottom: 2px solid #D1D5DB;
}

tr:last-child td {
    border-bottom: none;
}

tr:hover {
    background: #F3F4F6;
}
```

### Table with Hover Effect

```css
table {
    border-collapse: collapse;
    width: 100%;
}

th, td {
    padding: 12px;
    border: 1px solid #DDD;
}

tbody tr {
    transition: background-color 0.2s ease;
}

tbody tr:hover {
    background-color: #F0F8FF;
}
```

### Complete Table Styling Example

```csharp
var settings = new MarkdownStyleSettings
{
    CssStyleRules = @"
        table {
            background: #FAFBFC;
            border-collapse: collapse;
            margin: 16px 0;
            width: 100%;
            border-radius: 6px;
            overflow: hidden;
            box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
        }
        
        th, td {
            border: 1px solid #E0E4EA;
            padding: 10px 16px;
            text-align: left;
        }
        
        th {
            background: #EDF2FB;
            color: #193466;
            font-size: 17px;
            font-weight: 600;
        }
        
        tr:nth-child(even) {
            background: #F5F7FB;
        }
        
        tbody tr:hover {
            background: #E8EEF7;
        }
    "
};
```

## Styling Scrollbars

Customize scrollbar appearance for a polished look (primarily on Windows).

### Modern Visible Scrollbar

```css
::-webkit-scrollbar {
    width: 10px;
    background: #EEEEEE;
}

::-webkit-scrollbar-thumb {
    background: #3B82F6;
    border-radius: 8px;
    border: 2px solid #1E293B;
}

::-webkit-scrollbar-thumb:hover {
    background: #2563EB;
}
```

### Minimalist Scrollbar

```css
::-webkit-scrollbar {
    width: 8px;
    background: transparent;
}

::-webkit-scrollbar-thumb {
    background: rgba(0, 0, 0, 0.3);
    border-radius: 4px;
}

::-webkit-scrollbar-thumb:hover {
    background: rgba(0, 0, 0, 0.5);
}
```

### Hidden Scrollbar (Content Still Scrollable)

```css
::-webkit-scrollbar {
    width: 0px;
    display: none;
}

/* Alternative approach */
::-webkit-scrollbar {
    width: 0;
    height: 0;
}
```

### Dark Theme Scrollbar

```css
::-webkit-scrollbar {
    width: 12px;
    background: #1E1E1E;
}

::-webkit-scrollbar-thumb {
    background: #555555;
    border-radius: 6px;
}

::-webkit-scrollbar-thumb:hover {
    background: #777777;
}
```

### Complete Scrollbar Example

```csharp
var settings = new MarkdownStyleSettings
{
    CssStyleRules = @"
        ::-webkit-scrollbar {
            width: 10px;
            background: #F5F5F5;
        }
        
        ::-webkit-scrollbar-thumb {
            background: linear-gradient(180deg, #667EEA 0%, #764BA2 100%);
            border-radius: 10px;
            border: 2px solid #F5F5F5;
        }
        
        ::-webkit-scrollbar-thumb:hover {
            background: linear-gradient(180deg, #764BA2 0%, #667EEA 100%);
        }
    "
};
```

**Note:** Scrollbar styling is primarily supported on Windows. Android and iOS have platform-specific scrollbar behaviors that may not respect all CSS.

## Complete Branded Example

Here's a comprehensive example combining all styling techniques:

```csharp
public class BrandedMarkdownStyle
{
    public static MarkdownStyleSettings GetBrandedStyle()
    {
        return new MarkdownStyleSettings
        {
            CssStyleRules = @"
                /* Base Styles */
                body {
                    background: #FAFBFC;
                    font-family: 'Segoe UI', 'Helvetica Neue', Arial, sans-serif;
                    font-size: 16px;
                    color: #2E3440;
                    line-height: 1.7;
                    padding: 24px;
                }
                
                /* Headings */
                h1 {
                    font-weight: 700;
                    font-size: 32px;
                    color: #5E81AC;
                    border-bottom: 3px solid #5E81AC;
                    padding-bottom: 12px;
                    margin-top: 32px;
                    margin-bottom: 20px;
                }
                
                h2 {
                    font-weight: 600;
                    font-size: 26px;
                    color: #81A1C1;
                    border-left: 4px solid #81A1C1;
                    padding-left: 16px;
                    margin-top: 28px;
                    margin-bottom: 16px;
                }
                
                h3 {
                    font-weight: 600;
                    font-size: 22px;
                    color: #88C0D0;
                    margin-top: 24px;
                    margin-bottom: 12px;
                }
                
                /* Paragraphs and Text */
                p {
                    margin-bottom: 16px;
                    line-height: 1.8;
                }
                
                strong {
                    color: #3B4252;
                    font-weight: 600;
                }
                
                em {
                    color: #4C566A;
                }
                
                code {
                    background: #ECEFF4;
                    color: #BF616A;
                    padding: 3px 6px;
                    border-radius: 4px;
                    font-family: 'Consolas', 'Monaco', monospace;
                    font-size: 14px;
                }
                
                /* Links */
                a {
                    color: #5E81AC;
                    text-decoration: none;
                    border-bottom: 1px solid transparent;
                    transition: border-bottom 0.2s;
                }
                
                a:hover {
                    border-bottom: 1px solid #5E81AC;
                }
                
                /* Lists */
                ul, ol {
                    margin-bottom: 16px;
                    padding-left: 28px;
                }
                
                li {
                    margin-bottom: 8px;
                }
                
                /* Blockquotes */
                blockquote {
                    border-left: 4px solid #88C0D0;
                    background: #ECEFF4;
                    margin: 16px 0;
                    padding: 12px 20px;
                    font-style: italic;
                    color: #4C566A;
                }
                
                /* Code Blocks */
                pre {
                    background: #2E3440;
                    color: #D8DEE9;
                    padding: 16px;
                    border-radius: 8px;
                    overflow-x: auto;
                    margin: 16px 0;
                }
                
                pre code {
                    background: transparent;
                    color: #D8DEE9;
                    padding: 0;
                    font-size: 14px;
                }
                
                /* Tables */
                table {
                    background: #FFFFFF;
                    border-collapse: collapse;
                    margin: 20px 0;
                    width: 100%;
                    border-radius: 8px;
                    overflow: hidden;
                    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
                }
                
                th, td {
                    border: 1px solid #E5E9F0;
                    padding: 14px 18px;
                    text-align: left;
                }
                
                th {
                    background: #ECEFF4;
                    color: #2E3440;
                    font-weight: 600;
                    font-size: 15px;
                    border-bottom: 2px solid #D8DEE9;
                }
                
                tr:nth-child(even) {
                    background: #F9FAFB;
                }
                
                tbody tr:hover {
                    background: #EEF1F5;
                }
                
                /* Images */
                img {
                    max-width: 95%;
                    height: auto;
                    border-radius: 8px;
                    border: 1px solid #E5E9F0;
                    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
                    margin: 20px auto;
                    display: block;
                    transition: transform 0.2s, box-shadow 0.2s;
                }
                
                img:hover {
                    transform: translateY(-4px);
                    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.15);
                }
                
                /* Horizontal Rule */
                hr {
                    border: none;
                    border-top: 2px solid #E5E9F0;
                    margin: 32px 0;
                }
                
                /* Scrollbar */
                ::-webkit-scrollbar {
                    width: 10px;
                    background: #ECEFF4;
                }
                
                ::-webkit-scrollbar-thumb {
                    background: #88C0D0;
                    border-radius: 8px;
                    border: 2px solid #ECEFF4;
                }
                
                ::-webkit-scrollbar-thumb:hover {
                    background: #5E81AC;
                }
            "
        };
    }
}

// Usage
markdownViewer.Settings = BrandedMarkdownStyle.GetBrandedStyle();
```

## CSS Best Practices

### 1. Use Consistent Units

```css
/* ✅ Good - consistent units */
h1 { font-size: 32px; margin-bottom: 16px; }
h2 { font-size: 24px; margin-bottom: 12px; }

/* ❌ Inconsistent - mixing units */
h1 { font-size: 2em; margin-bottom: 1rem; }
h2 { font-size: 24px; margin-bottom: 12px; }
```

### 2. Maintain Visual Hierarchy

```css
/* Clear size progression */
h1 { font-size: 32px; }
h2 { font-size: 26px; }
h3 { font-size: 22px; }
body { font-size: 16px; }
```

### 3. Ensure Adequate Contrast

```css
/* ✅ Good contrast */
body { background: #FFFFFF; color: #2C3E50; }

/* ❌ Poor contrast */
body { background: #EEEEEE; color: #CCCCCC; }
```

### 4. Use Semantic Color Variables (via comments)

```css
/* Brand Colors:
   Primary: #5E81AC
   Secondary: #81A1C1
   Accent: #88C0D0
   Text: #2E3440
*/

h1 { color: #5E81AC; /* Primary */ }
h2 { color: #81A1C1; /* Secondary */ }
```

### 5. Test Cross-Platform

Different platforms may render CSS slightly differently. Always test on:
- Windows
- Android
- iOS
- macOS (if targeting)

### 6. Keep CSS Organized

```css
/* Group related styles */

/* === Base Styles === */
body { ... }

/* === Typography === */
h1 { ... }
h2 { ... }
p { ... }

/* === Components === */
table { ... }
img { ... }

/* === Scrollbar === */
::-webkit-scrollbar { ... }
```

## Troubleshooting

### CSS Not Applying

**Issue:** CSS rules defined but no visual changes

**Solutions:**
1. Check for syntax errors in CSS
2. Ensure `CssStyleRules` property is set correctly
3. Verify CSS selectors match Markdown elements
4. Test with simple CSS first to isolate issues
5. Check if properties are overriding CSS (CSS should win)

### Styles Look Different on Platforms

**Issue:** CSS renders differently on Windows vs. mobile

**Solutions:**
1. Avoid platform-specific CSS features
2. Test on all target platforms
3. Use standard CSS properties
4. Consider platform-specific CSS with conditional compilation

### Scrollbar Not Customizing

**Issue:** Scrollbar CSS not working on mobile

**Solutions:**
1. `::-webkit-scrollbar` primarily works on Windows/desktop
2. Android and iOS have native scrollbar behavior
3. Consider hiding scrollbar on mobile if customization is critical
4. Test on actual devices, not just emulators

### Images Not Styling

**Issue:** Image CSS not applying

**Solutions:**
1. Ensure images are actually loading in Markdown
2. Check `img` selector syntax
3. Verify image paths are correct
4. Test with simple `img { border: 5px solid red; }` to confirm selector works

### Table Styling Inconsistent

**Issue:** Tables don't look as expected

**Solutions:**
1. Use `border-collapse: collapse;` for consistent borders
2. Set explicit `width: 100%;` if needed
3. Check for `:nth-child` selector support on platform
4. Test with simple table first

## Next Steps

- **Learn basic appearance customization** → See [appearance-customization.md](appearance-customization.md)
- **Retrieve and convert content** → See [content-retrieval.md](content-retrieval.md)
- **Troubleshoot common issues** → See [troubleshooting.md](troubleshooting.md)
