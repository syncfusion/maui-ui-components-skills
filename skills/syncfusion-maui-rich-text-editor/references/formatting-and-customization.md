# Formatting and Customization

## Table of Contents
- [Overview](#overview)
- [Editor Appearance](#editor-appearance)
- [Character Formatting](#character-formatting)
- [List Formatting](#list-formatting)
- [Text Alignment](#text-alignment)
- [Text Styling](#text-styling)
- [Paragraph Formatting](#paragraph-formatting)
- [Indentation Control](#indentation-control)
- [Default Text Styles](#default-text-styles)
- [Combined Formatting Examples](#combined-formatting-examples)
- [Best Practices](#best-practices)

## Overview

The Rich Text Editor provides extensive programmatic formatting capabilities. You can apply formatting through toolbar interaction or by calling methods directly. This is essential for building custom UI, implementing keyboard shortcuts, or applying formatting based on business logic.

## Editor Appearance

Customize the visual appearance of the editor content area.

### Available Properties

- **EditorBackgroundColor** - Background color of the content area
- **BorderColor** - Color of the border around the editor
- **BorderThickness** - Thickness of the border
- **EnableWordWrap** - Text wrapping behavior (default: `True`)

### XAML Configuration

```xml
<rte:SfRichTextEditor EditorBackgroundColor="LightYellow"
                      BorderColor="SlateGray"
                      BorderThickness="2"
                      EnableWordWrap="True" />
```

### C# Configuration

```csharp
SfRichTextEditor richTextEditor = new SfRichTextEditor
{
    EditorBackgroundColor = Colors.LightYellow,
    BorderColor = Colors.SlateGray,
    BorderThickness = 2,
    EnableWordWrap = true
};
```

### Common Appearance Patterns

**Clean White Editor:**
```csharp
richTextEditor.EditorBackgroundColor = Colors.White;
richTextEditor.BorderColor = Colors.LightGray;
richTextEditor.BorderThickness = 1;
```

**Dark Mode Editor:**
```csharp
richTextEditor.EditorBackgroundColor = Color.FromRgb(30, 30, 30);
richTextEditor.BorderColor = Color.FromRgb(60, 60, 60);
richTextEditor.BorderThickness = 1;
richTextEditor.DefaultTextColor = Colors.White;
```

**Note-Style Editor:**
```csharp
richTextEditor.EditorBackgroundColor = Color.FromRgb(255, 250, 205); // Light yellow
richTextEditor.BorderColor = Color.FromRgb(218, 165, 32); // Goldenrod
richTextEditor.BorderThickness = 2;
```

**Borderless Editor:**
```csharp
richTextEditor.EditorBackgroundColor = Colors.Transparent;
richTextEditor.BorderThickness = 0;
```

## Character Formatting

Apply common text styles like bold, italic, underline, and strikethrough.

### Toggle Methods

These methods toggle the formatting state for the current selection:

- **ToggleBold()** - Toggles bold text
- **ToggleItalic()** - Toggles italic text
- **ToggleUnderline()** - Toggles underline
- **ToggleStrikethrough()** - Toggles strikethrough
- **ToggleSubscript()** - Toggles subscript (H₂O)
- **ToggleSuperscript()** - Toggles superscript (x²)

### Basic Usage

```csharp
// Toggle bold on selected text
richTextEditor.ToggleBold();

// Toggle italic
richTextEditor.ToggleItalic();

// Toggle underline
richTextEditor.ToggleUnderline();

// Toggle strikethrough
richTextEditor.ToggleStrikethrough();
```

### Script Formatting

```csharp
// Apply subscript (e.g., H₂O)
richTextEditor.ToggleSubscript();

// Apply superscript (e.g., x²)
richTextEditor.ToggleSuperscript();
```

### Implementing Custom Formatting Buttons

```xml
<StackLayout Orientation="Horizontal" Spacing="5">
    <Button Text="B" FontAttributes="Bold" Clicked="OnBoldClicked" />
    <Button Text="I" FontAttributes="Italic" Clicked="OnItalicClicked" />
    <Button Text="U" TextDecorations="Underline" Clicked="OnUnderlineClicked" />
</StackLayout>

<rte:SfRichTextEditor x:Name="richTextEditor" ShowToolbar="False" />
```

```csharp
private void OnBoldClicked(object sender, EventArgs e)
{
    richTextEditor.ToggleBold();
}

private void OnItalicClicked(object sender, EventArgs e)
{
    richTextEditor.ToggleItalic();
}

private void OnUnderlineClicked(object sender, EventArgs e)
{
    richTextEditor.ToggleUnderline();
}
```

### Keyboard Shortcuts Handler

```csharp
// In your page or view
private void SetupKeyboardShortcuts()
{
    // Note: Actual keyboard shortcut implementation depends on platform
    // This is conceptual - Rich Text Editor handles common shortcuts automatically
    
    // Ctrl+B / Cmd+B for Bold - handled automatically
    // Ctrl+I / Cmd+I for Italic - handled automatically
    // Ctrl+U / Cmd+U for Underline - handled automatically
}
```

## List Formatting

Apply bulleted or numbered lists to paragraphs.

### Methods

- **ToggleBulletList()** - Toggles bulleted list
- **ToggleNumberList()** - Toggles numbered list

### Basic Usage

```csharp
// Apply bulleted list to current paragraph
richTextEditor.ToggleBulletList();

// Apply numbered list to current paragraph
richTextEditor.ToggleNumberList();
```

### Custom List Buttons

```xml
<StackLayout Orientation="Horizontal" Spacing="5">
    <Button Text="• Bullets" Clicked="OnBulletsClicked" />
    <Button Text="1. Numbers" Clicked="OnNumbersClicked" />
</StackLayout>
```

```csharp
private void OnBulletsClicked(object sender, EventArgs e)
{
    richTextEditor.ToggleBulletList();
}

private void OnNumbersClicked(object sender, EventArgs e)
{
    richTextEditor.ToggleNumberList();
}
```

### List Behavior

- Calling toggle method on a list item **removes** the list formatting
- Calling on multiple selected paragraphs applies list to all
- Nested lists are supported through indentation

## Text Alignment

Control horizontal alignment of paragraphs.

### Methods

- **AlignLeft()** - Left-aligns text
- **AlignRight()** - Right-aligns text
- **AlignCenter()** - Centers text
- **AlignJustify()** - Justifies text (full-width alignment)

### Basic Usage

```csharp
// Left align
richTextEditor.AlignLeft();

// Center align
richTextEditor.AlignCenter();

// Right align
richTextEditor.AlignRight();

// Justify
richTextEditor.AlignJustify();
```

### Custom Alignment UI

```xml
<StackLayout Orientation="Horizontal" Spacing="5">
    <Button Text="⬅" Clicked="OnAlignLeftClicked" />
    <Button Text="⬌" Clicked="OnAlignCenterClicked" />
    <Button Text="➡" Clicked="OnAlignRightClicked" />
    <Button Text="≡" Clicked="OnAlignJustifyClicked" />
</StackLayout>
```

```csharp
private void OnAlignLeftClicked(object sender, EventArgs e)
{
    richTextEditor.AlignLeft();
}

private void OnAlignCenterClicked(object sender, EventArgs e)
{
    richTextEditor.AlignCenter();
}

private void OnAlignRightClicked(object sender, EventArgs e)
{
    richTextEditor.AlignRight();
}

private void OnAlignJustifyClicked(object sender, EventArgs e)
{
    richTextEditor.AlignJustify();
}
```

## Text Styling

Apply fonts, sizes, and colors to text.

### Methods

- **ApplyFontFamily(string fontName)** - Changes font family
- **ApplyFontSize(double fontSize)** - Changes font size
- **ApplyTextColor(Color textColor)** - Changes text color
- **ApplyHighlightColor(Color highlightColor)** - Changes background highlight

### Font Family

```csharp
// Apply specific font
richTextEditor.ApplyFontFamily("Arial");
richTextEditor.ApplyFontFamily("Times New Roman");
richTextEditor.ApplyFontFamily("Courier New");
richTextEditor.ApplyFontFamily("Georgia");
```

### Font Size

```csharp
// Apply font sizes
richTextEditor.ApplyFontSize(12);
richTextEditor.ApplyFontSize(14);
richTextEditor.ApplyFontSize(18);
richTextEditor.ApplyFontSize(24);
```

### Text Color

```csharp
// Apply predefined colors
richTextEditor.ApplyTextColor(Colors.Red);
richTextEditor.ApplyTextColor(Colors.Blue);
richTextEditor.ApplyTextColor(Colors.Green);

// Apply custom RGB colors
richTextEditor.ApplyTextColor(Color.FromRgb(255, 87, 34));
richTextEditor.ApplyTextColor(Color.FromRgb(33, 150, 243));
```

### Highlight Color

```csharp
// Apply highlight (background) color
richTextEditor.ApplyHighlightColor(Colors.Yellow);
richTextEditor.ApplyHighlightColor(Colors.LightGreen);
richTextEditor.ApplyHighlightColor(Colors.Pink);

// Remove highlight
richTextEditor.ApplyHighlightColor(Colors.Transparent);
```

### Custom Font Picker

```xml
<Picker x:Name="fontPicker" 
        Title="Select Font"
        SelectedIndexChanged="OnFontSelected">
    <Picker.ItemsSource>
        <x:Array Type="{x:Type x:String}">
            <x:String>Arial</x:String>
            <x:String>Times New Roman</x:String>
            <x:String>Courier New</x:String>
            <x:String>Georgia</x:String>
            <x:String>Verdana</x:String>
        </x:Array>
    </Picker.ItemsSource>
</Picker>
```

```csharp
private void OnFontSelected(object sender, EventArgs e)
{
    if (fontPicker.SelectedItem != null)
    {
        string fontName = fontPicker.SelectedItem.ToString();
        richTextEditor.ApplyFontFamily(fontName);
    }
}
```

### Custom Color Picker

```xml
<StackLayout Orientation="Horizontal" Spacing="5">
    <Button BackgroundColor="Red" WidthRequest="40" HeightRequest="40" 
            Clicked="OnRedClicked" />
    <Button BackgroundColor="Blue" WidthRequest="40" HeightRequest="40" 
            Clicked="OnBlueClicked" />
    <Button BackgroundColor="Green" WidthRequest="40" HeightRequest="40" 
            Clicked="OnGreenClicked" />
</StackLayout>
```

```csharp
private void OnRedClicked(object sender, EventArgs e)
{
    richTextEditor.ApplyTextColor(Colors.Red);
}

private void OnBlueClicked(object sender, EventArgs e)
{
    richTextEditor.ApplyTextColor(Colors.Blue);
}

private void OnGreenClicked(object sender, EventArgs e)
{
    richTextEditor.ApplyTextColor(Colors.Green);
}
```

## Paragraph Formatting

Apply heading styles and paragraph formats.

### Method

- **ApplyParagraphFormat(RichTextEditorParagraphFormat format)** - Applies paragraph style

### Available Formats

- `RichTextEditorParagraphFormat.Normal` - Standard paragraph
- `RichTextEditorParagraphFormat.Heading1` - Largest heading
- `RichTextEditorParagraphFormat.Heading2`
- `RichTextEditorParagraphFormat.Heading3`
- `RichTextEditorParagraphFormat.Heading4`
- `RichTextEditorParagraphFormat.Heading5`
- `RichTextEditorParagraphFormat.Heading6` - Smallest heading

### Basic Usage

```csharp
// Apply heading 1
richTextEditor.ApplyParagraphFormat(RichTextEditorParagraphFormat.Heading1);

// Apply heading 2
richTextEditor.ApplyParagraphFormat(RichTextEditorParagraphFormat.Heading2);

// Reset to normal paragraph
richTextEditor.ApplyParagraphFormat(RichTextEditorParagraphFormat.Normal);
```

### Custom Heading Picker

```xml
<Picker x:Name="headingPicker" 
        Title="Paragraph Style"
        SelectedIndexChanged="OnHeadingSelected">
    <Picker.ItemsSource>
        <x:Array Type="{x:Type x:String}">
            <x:String>Normal</x:String>
            <x:String>Heading 1</x:String>
            <x:String>Heading 2</x:String>
            <x:String>Heading 3</x:String>
            <x:String>Heading 4</x:String>
            <x:String>Heading 5</x:String>
            <x:String>Heading 6</x:String>
        </x:Array>
    </Picker.ItemsSource>
</Picker>
```

```csharp
private void OnHeadingSelected(object sender, EventArgs e)
{
    if (headingPicker.SelectedIndex >= 0)
    {
        var format = headingPicker.SelectedIndex switch
        {
            0 => RichTextEditorParagraphFormat.Normal,
            1 => RichTextEditorParagraphFormat.Heading1,
            2 => RichTextEditorParagraphFormat.Heading2,
            3 => RichTextEditorParagraphFormat.Heading3,
            4 => RichTextEditorParagraphFormat.Heading4,
            5 => RichTextEditorParagraphFormat.Heading5,
            6 => RichTextEditorParagraphFormat.Heading6,
            _ => RichTextEditorParagraphFormat.Normal
        };
        
        richTextEditor.ApplyParagraphFormat(format);
    }
}
```

## Indentation Control

Increase or decrease paragraph indentation.

### Methods

- **IncreaseIndent()** - Increases indentation level
- **DecreaseIndent()** - Decreases indentation level

### Basic Usage

```csharp
// Increase indentation
richTextEditor.IncreaseIndent();

// Decrease indentation
richTextEditor.DecreaseIndent();
```

### Custom Indentation Buttons

```xml
<StackLayout Orientation="Horizontal" Spacing="5">
    <Button Text="← Outdent" Clicked="OnOutdentClicked" />
    <Button Text="Indent →" Clicked="OnIndentClicked" />
</StackLayout>
```

```csharp
private void OnIndentClicked(object sender, EventArgs e)
{
    richTextEditor.IncreaseIndent();
}

private void OnOutdentClicked(object sender, EventArgs e)
{
    richTextEditor.DecreaseIndent();
}
```

### Indentation with Lists

Indentation works with lists to create nested structures:

```csharp
// Create a bulleted list
richTextEditor.ToggleBulletList();

// Indent to create nested bullet
richTextEditor.IncreaseIndent();

// Outdent back to parent level
richTextEditor.DecreaseIndent();
```

## Default Text Styles

Set default styling for newly typed text.

### Properties

- **DefaultFontFamily** - Default font family
- **DefaultFontSize** - Default font size
- **DefaultTextColor** - Default text color

### XAML Configuration

```xml
<rte:SfRichTextEditor DefaultFontFamily="Arial"
                      DefaultFontSize="14"
                      DefaultTextColor="Black" />
```

### C# Configuration

```csharp
SfRichTextEditor richTextEditor = new SfRichTextEditor
{
    DefaultFontFamily = "Arial",
    DefaultFontSize = 14,
    DefaultTextColor = Colors.Black
};
```

### Use Cases

**Professional Document Editor:**
```csharp
richTextEditor.DefaultFontFamily = "Times New Roman";
richTextEditor.DefaultFontSize = 12;
richTextEditor.DefaultTextColor = Colors.Black;
```

**Modern UI Editor:**
```csharp
richTextEditor.DefaultFontFamily = "Segoe UI";
richTextEditor.DefaultFontSize = 14;
richTextEditor.DefaultTextColor = Color.FromRgb(33, 33, 33);
```

**Coding/Technical Editor:**
```csharp
richTextEditor.DefaultFontFamily = "Courier New";
richTextEditor.DefaultFontSize = 13;
richTextEditor.DefaultTextColor = Color.FromRgb(0, 0, 0);
```

## Combined Formatting Examples

### Formatted Heading Creation

```csharp
public void CreateFormattedHeading(string text)
{
    // Set text
    richTextEditor.Text = text;
    
    // Select all
    // (Note: Selection API depends on implementation)
    
    // Apply heading 1
    richTextEditor.ApplyParagraphFormat(RichTextEditorParagraphFormat.Heading1);
    
    // Make bold
    richTextEditor.ToggleBold();
    
    // Apply color
    richTextEditor.ApplyTextColor(Color.FromRgb(33, 150, 243));
    
    // Center align
    richTextEditor.AlignCenter();
}
```

### Highlighted Note

```csharp
public void CreateHighlightedNote(string note)
{
    richTextEditor.Text = note;
    
    // Apply bright background
    richTextEditor.ApplyHighlightColor(Colors.Yellow);
    
    // Bold text
    richTextEditor.ToggleBold();
    
    // Slightly larger font
    richTextEditor.ApplyFontSize(16);
}
```

### Code Block Styling

```csharp
public void FormatAsCode(string code)
{
    richTextEditor.Text = code;
    
    // Monospace font
    richTextEditor.ApplyFontFamily("Courier New");
    
    // Slightly smaller
    richTextEditor.ApplyFontSize(12);
    
    // Light gray background
    richTextEditor.ApplyHighlightColor(Color.FromRgb(240, 240, 240));
    
    // Dark text
    richTextEditor.ApplyTextColor(Color.FromRgb(51, 51, 51));
}
```

### Emphasis Text

```csharp
public void EmphasizeText()
{
    // Bold + Italic + Color for strong emphasis
    richTextEditor.ToggleBold();
    richTextEditor.ToggleItalic();
    richTextEditor.ApplyTextColor(Colors.Red);
}
```

### Quote Formatting

```csharp
public void FormatAsQuote()
{
    // Italic text
    richTextEditor.ToggleItalic();
    
    // Increase indent for visual separation
    richTextEditor.IncreaseIndent();
    
    // Slightly smaller font
    richTextEditor.ApplyFontSize(13);
    
    // Gray color
    richTextEditor.ApplyTextColor(Colors.Gray);
}
```

## Best Practices

### 1. Provide Visual Feedback

Update custom UI to reflect current formatting state using FormatChanged event:

```csharp
richTextEditor.FormatChanged += (sender, e) =>
{
    // Update button states based on current formatting
    // (Event details provide current format state)
};
```

### 2. Group Related Formatting

Apply multiple related formats together:

```csharp
public void ApplyTitleFormat()
{
    richTextEditor.ApplyParagraphFormat(RichTextEditorParagraphFormat.Heading1);
    richTextEditor.ToggleBold();
    richTextEditor.ApplyTextColor(Color.FromRgb(33, 150, 243));
    richTextEditor.AlignCenter();
}
```

### 3. Validate Before Applying

Check if editor has content before applying formatting:

```csharp
public void SafeApplyFormatting()
{
    if (!string.IsNullOrEmpty(richTextEditor.Text))
    {
        richTextEditor.ToggleBold();
    }
}
```

### 4. Use Default Styles

Set appropriate defaults to reduce manual formatting:

```csharp
// Set good defaults for your use case
richTextEditor.DefaultFontFamily = "Arial";
richTextEditor.DefaultFontSize = 14;
richTextEditor.DefaultTextColor = Colors.Black;
```

### 5. Test Color Accessibility

Ensure text colors have sufficient contrast:

```csharp
// Good contrast
richTextEditor.ApplyTextColor(Colors.Black);
richTextEditor.EditorBackgroundColor = Colors.White;

// Avoid poor contrast
// richTextEditor.ApplyTextColor(Colors.LightGray);  // Hard to read on white
```

### 6. Provide Format Presets

Offer common format combinations as presets:

```csharp
public void ApplyPreset(string presetName)
{
    switch (presetName)
    {
        case "Title":
            ApplyTitleFormat();
            break;
        case "Subtitle":
            ApplySubtitleFormat();
            break;
        case "Code":
            ApplyCodeFormat();
            break;
        case "Quote":
            ApplyQuoteFormat();
            break;
    }
}
```

### 7. Respect Platform Conventions

Use platform-appropriate fonts and sizes:

```csharp
#if WINDOWS
    richTextEditor.DefaultFontFamily = "Segoe UI";
    richTextEditor.DefaultFontSize = 14;
#elif MACCATALYST
    richTextEditor.DefaultFontFamily = "San Francisco";
    richTextEditor.DefaultFontSize = 13;
#elif ANDROID
    richTextEditor.DefaultFontFamily = "Roboto";
    richTextEditor.DefaultFontSize = 14;
#elif IOS
    richTextEditor.DefaultFontFamily = "San Francisco";
    richTextEditor.DefaultFontSize = 14;
#endif
```

## Next Steps

- Explore [content management](content-management.md) for handling HTML text
- Learn about [events](events-and-interactions.md) to track formatting changes
- Implement [hyperlinks](hyperlinks.md) for rich content linking
