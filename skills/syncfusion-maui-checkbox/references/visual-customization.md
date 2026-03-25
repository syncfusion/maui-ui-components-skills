# Visual Customization in .NET MAUI CheckBox

## Table of Contents
- [Shape Customization](#shape-customization)
- [State Colors](#state-colors)
- [Stroke Thickness](#stroke-thickness)
- [Caption Text Appearance](#caption-text-appearance)
- [Tick Color Customization](#tick-color-customization)
- [Line Break Mode](#line-break-mode)
- [Size Customization](#size-customization)
- [Font Auto-Scaling](#font-auto-scaling)
- [Animation Control](#animation-control)
- [Content Spacing](#content-spacing)
- [Complete Customization Examples](#complete-customization-examples)

---

This guide covers all visual customization options available for the Syncfusion .NET MAUI CheckBox control.

## Shape Customization

The `CornerRadius` property customizes the shape of the checkbox by rounding its corners.

### Syntax:

```csharp
public double CornerRadius { get; set; }
```

### Usage:

**XAML:**
```xml
<buttons:SfCheckBox x:Name="checkBox" 
                    Text="Rounded CheckBox" 
                    IsChecked="True" 
                    CornerRadius="5.0"/>
```

**C#:**
```csharp
SfCheckBox checkBox = new SfCheckBox
{
    Text = "Rounded CheckBox",
    IsChecked = true,
    CornerRadius = 5.0
};
this.Content = checkBox;
```

### Common Values:

- `0` - Square corners (default)
- `2-5` - Slightly rounded (subtle)
- `8-12` - Medium rounded (modern look)
- `20+` - Circular/pill shape

### Example: Different Corner Radii

```xml
<StackLayout Padding="20" Spacing="15">
    <buttons:SfCheckBox Text="Square (0)" 
                        IsChecked="True" 
                        CornerRadius="0"/>
    
    <buttons:SfCheckBox Text="Slightly Rounded (3)" 
                        IsChecked="True" 
                        CornerRadius="3"/>
    
    <buttons:SfCheckBox Text="Rounded (8)" 
                        IsChecked="True" 
                        CornerRadius="8"/>
    
    <buttons:SfCheckBox Text="Very Rounded (15)" 
                        IsChecked="True" 
                        CornerRadius="15"/>
</StackLayout>
```

## State Colors

Customize the colors for checked and unchecked states using `CheckedColor` and `UncheckedColor` properties.

### Properties:

```csharp
public Color CheckedColor { get; set; }     // Color for checked/indeterminate states
public Color UncheckedColor { get; set; }   // Color for unchecked state
```

### Usage:

**XAML:**
```xml
<StackLayout Padding="20" Spacing="10">
    <!-- Checked state with custom color -->
    <buttons:SfCheckBox x:Name="check" 
                        Text="Checked (Green)" 
                        IsChecked="True" 
                        CheckedColor="Green"/>
    
    <!-- Unchecked state with custom color -->
    <buttons:SfCheckBox x:Name="unCheck" 
                        Text="Unchecked (Violet)" 
                        UncheckedColor="Violet"/>
    
    <!-- Indeterminate state with custom color -->
    <buttons:SfCheckBox x:Name="intermediate" 
                        Text="Indeterminate (Purple)" 
                        IsThreeState="True" 
                        IsChecked="{x:Null}" 
                        CheckedColor="Purple"/> 
</StackLayout>
```

**C#:**
```csharp
StackLayout stackLayout = new StackLayout { Padding = 20, Spacing = 10 };

SfCheckBox check = new SfCheckBox
{
    Text = "Checked (Green)",
    IsChecked = true,
    CheckedColor = Colors.Green
};

SfCheckBox uncheck = new SfCheckBox
{
    Text = "Unchecked (Violet)",
    UncheckedColor = Colors.Violet
};

SfCheckBox intermediate = new SfCheckBox
{
    Text = "Indeterminate (Purple)",
    IsChecked = null,
    IsThreeState = true,
    CheckedColor = Colors.Purple
};

stackLayout.Children.Add(check);
stackLayout.Children.Add(uncheck);
stackLayout.Children.Add(intermediate);

this.Content = stackLayout;
```

### Color Examples:

```xml
<!-- Brand colors -->
<buttons:SfCheckBox Text="Primary" CheckedColor="#007AFF" IsChecked="True"/>
<buttons:SfCheckBox Text="Success" CheckedColor="#28A745" IsChecked="True"/>
<buttons:SfCheckBox Text="Warning" CheckedColor="#FFC107" IsChecked="True"/>
<buttons:SfCheckBox Text="Danger" CheckedColor="#DC3545" IsChecked="True"/>

<!-- Custom theme -->
<buttons:SfCheckBox Text="Custom Theme" 
                    CheckedColor="#6A1B9A" 
                    UncheckedColor="#9C27B0"
                    IsChecked="True"/>
```

## Stroke Thickness

The `StrokeThickness` property controls the border thickness of the checkbox.

### Syntax:

```csharp
public double StrokeThickness { get; set; }
```

### Usage:

**XAML:**
```xml
<StackLayout Padding="20" Spacing="10">
    <buttons:SfCheckBox Text="Thin (1)" 
                        StrokeThickness="1" 
                        UncheckedColor="Blue" 
                        FontSize="16"/>
    
    <buttons:SfCheckBox Text="Normal (2)" 
                        StrokeThickness="2" 
                        UncheckedColor="Blue" 
                        FontSize="20"/>
    
    <buttons:SfCheckBox Text="Medium (4)" 
                        StrokeThickness="4" 
                        UncheckedColor="Blue" 
                        FontSize="25"/>
    
    <buttons:SfCheckBox Text="Thick (6)" 
                        StrokeThickness="6" 
                        UncheckedColor="Blue" 
                        FontSize="30"/>
</StackLayout>
```

**C#:**
```csharp
StackLayout stackLayout = new StackLayout { Padding = 20, Spacing = 10 };

SfCheckBox check1 = new SfCheckBox
{
    Text = "Thin (1)",
    StrokeThickness = 1,
    FontSize = 16,
    UncheckedColor = Colors.Blue
};

SfCheckBox check2 = new SfCheckBox
{
    Text = "Normal (2)",
    StrokeThickness = 2,
    FontSize = 20,
    UncheckedColor = Colors.Blue
};

SfCheckBox check3 = new SfCheckBox
{
    Text = "Thick (4)",
    StrokeThickness = 4,
    FontSize = 25,
    UncheckedColor = Colors.Blue
};

stackLayout.Children.Add(check1);
stackLayout.Children.Add(check2);
stackLayout.Children.Add(check3);

this.Content = stackLayout;
```

### Recommendations:

- **1-2**: Subtle, modern look
- **2-3**: Standard, balanced appearance (default is typically 2)
- **4-6**: Bold, high visibility
- **Match with ControlSize**: Larger checkboxes benefit from thicker strokes

## Caption Text Appearance

Customize the checkbox caption text using multiple properties.

### Properties:

```csharp
public Color TextColor { get; set; }                           // Text color
public TextAlignment HorizontalTextAlignment { get; set; }     // Text alignment
public string FontFamily { get; set; }                         // Font family
public FontAttributes FontAttributes { get; set; }             // Bold, Italic, None
public double FontSize { get; set; }                          // Font size
```

### Complete Example:

**XAML:**
```xml
<buttons:SfCheckBox x:Name="caption" 
                    Text="Custom Text Styling" 
                    IsChecked="True" 
                    TextColor="Blue" 
                    HorizontalTextAlignment="Center" 
                    FontFamily="Arial" 
                    FontAttributes="Bold" 
                    FontSize="20"/>
```

**C#:**
```csharp
SfCheckBox caption = new SfCheckBox
{
    IsChecked = true,
    Text = "Custom Text Styling",
    TextColor = Colors.Blue,
    HorizontalTextAlignment = TextAlignment.Center,
    FontFamily = "Arial",
    FontAttributes = FontAttributes.Bold,
    FontSize = 20
};
this.Content = caption;
```

### Text Alignment Options:

```xml
<StackLayout Padding="20" Spacing="10">
    <buttons:SfCheckBox Text="Start Alignment" 
                        HorizontalTextAlignment="Start"/>
    
    <buttons:SfCheckBox Text="Center Alignment" 
                        HorizontalTextAlignment="Center"/>
    
    <buttons:SfCheckBox Text="End Alignment" 
                        HorizontalTextAlignment="End"/>
</StackLayout>
```

### Font Attributes:

```xml
<StackLayout Padding="20" Spacing="10">
    <buttons:SfCheckBox Text="Normal Text" 
                        FontAttributes="None"/>
    
    <buttons:SfCheckBox Text="Bold Text" 
                        FontAttributes="Bold"/>
    
    <buttons:SfCheckBox Text="Italic Text" 
                        FontAttributes="Italic"/>
    
    <buttons:SfCheckBox Text="Bold and Italic" 
                        FontAttributes="Bold, Italic"/>
</StackLayout>
```

### Custom Fonts:

```xml
<!-- Using system fonts -->
<buttons:SfCheckBox Text="Arial Font" FontFamily="Arial"/>
<buttons:SfCheckBox Text="Georgia Font" FontFamily="Georgia"/>
<buttons:SfCheckBox Text="Courier New" FontFamily="Courier New"/>

<!-- Using custom fonts (after registration in MauiProgram.cs) -->
<buttons:SfCheckBox Text="Custom Font" FontFamily="MyCustomFont"/>
```

## Tick Color Customization

The `TickColor` property customizes the color of the checkmark inside the checkbox.

### Syntax:

```csharp
public Color TickColor { get; set; }
```

### Usage:

**XAML:**
```xml
<StackLayout Padding="20" Spacing="10">
    <buttons:SfCheckBox x:Name="checkBox" 
                        IsChecked="True" 
                        CheckedColor="Aqua" 
                        TickColor="Fuchsia" 
                        Text="Custom Tick Color"/>
    
    <buttons:SfCheckBox IsChecked="True" 
                        CheckedColor="DarkBlue" 
                        TickColor="White" 
                        Text="High Contrast"/>
    
    <buttons:SfCheckBox IsChecked="True" 
                        CheckedColor="#FFD700" 
                        TickColor="#000000" 
                        Text="Gold with Black Tick"/>
</StackLayout>
```

**C#:**
```csharp
StackLayout stackLayout = new StackLayout { Padding = 20, Spacing = 10 };

SfCheckBox checkBox = new SfCheckBox
{
    IsChecked = true,
    Text = "Custom Tick Color",
    CheckedColor = Colors.Aqua,
    TickColor = Colors.Fuchsia
};

SfCheckBox highContrast = new SfCheckBox
{
    IsChecked = true,
    Text = "High Contrast",
    CheckedColor = Colors.DarkBlue,
    TickColor = Colors.White
};

stackLayout.Children.Add(checkBox);
stackLayout.Children.Add(highContrast);

this.Content = stackLayout;
```

### Accessibility Tip:

Ensure sufficient contrast between `CheckedColor` and `TickColor` for visibility:

```xml
<!-- Good contrast examples -->
<buttons:SfCheckBox CheckedColor="DarkGreen" TickColor="White" IsChecked="True"/>
<buttons:SfCheckBox CheckedColor="Navy" TickColor="Yellow" IsChecked="True"/>
<buttons:SfCheckBox CheckedColor="Black" TickColor="Lime" IsChecked="True"/>
```

## Line Break Mode

The `LineBreakMode` property controls how text wraps or truncates when it exceeds the available width.

### Syntax:

```csharp
public LineBreakMode LineBreakMode { get; set; }
```

### Options:

| Value | Description |
|-------|-------------|
| `NoWrap` | Text doesn't wrap (default) |
| `WordWrap` | Wraps text by words |
| `CharacterWrap` | Wraps text by characters |
| `HeadTruncation` | Truncates at the start with "..." |
| `MiddleTruncation` | Truncates in the middle with "..." |
| `TailTruncation` | Truncates at the end with "..." |

### Usage:

**XAML:**
```xml
<StackLayout Padding="20" Spacing="15">
    <!-- Word Wrap -->
    <buttons:SfCheckBox x:Name="wordWrap" 
                        IsChecked="True" 
                        WidthRequest="200" 
                        LineBreakMode="WordWrap" 
                        Text="The LineBreakMode allows you to wrap or truncate the text by words."/>
    
    <!-- Character Wrap -->
    <buttons:SfCheckBox WidthRequest="150" 
                        LineBreakMode="CharacterWrap" 
                        Text="CharacterWrapExample"/>
    
    <!-- Tail Truncation -->
    <buttons:SfCheckBox WidthRequest="150" 
                        LineBreakMode="TailTruncation" 
                        Text="This is a very long text that will be truncated at the end"/>
    
    <!-- Middle Truncation -->
    <buttons:SfCheckBox WidthRequest="150" 
                        LineBreakMode="MiddleTruncation" 
                        Text="This text truncates in the middle"/>
</StackLayout>
```

**C#:**
```csharp
StackLayout stackLayout = new StackLayout { Padding = 20, Spacing = 15 };

SfCheckBox wordWrap = new SfCheckBox
{
    Text = "The LineBreakMode allows you to wrap or truncate the text by words.",
    LineBreakMode = LineBreakMode.WordWrap,
    WidthRequest = 200,
    IsChecked = true
};

SfCheckBox tailTruncation = new SfCheckBox
{
    Text = "This is a very long text that will be truncated at the end",
    LineBreakMode = LineBreakMode.TailTruncation,
    WidthRequest = 150
};

stackLayout.Children.Add(wordWrap);
stackLayout.Children.Add(tailTruncation);

this.Content = stackLayout;
```

### Best Practices:

- Use `WordWrap` for multi-line labels in forms
- Use `TailTruncation` for single-line items in lists
- Use `MiddleTruncation` for file paths or URLs
- Set `WidthRequest` or constrain width when using truncation

## Size Customization

The `ControlSize` property sets the size of the checkbox box (not including text).

### Syntax:

```csharp
public double ControlSize { get; set; }
```

### Usage:

**XAML:**
```xml
<StackLayout Padding="20" Spacing="10">
    <buttons:SfCheckBox Text="Small (20)" 
                        ControlSize="20" 
                        FontSize="12"/>
    
    <buttons:SfCheckBox Text="Medium (30)" 
                        ControlSize="30" 
                        FontSize="16"/>
    
    <buttons:SfCheckBox Text="Large (40)" 
                        ControlSize="40" 
                        FontSize="20"/>
    
    <buttons:SfCheckBox Text="Extra Large (50)" 
                        ControlSize="50" 
                        FontSize="24"/>
</StackLayout>
```

**C#:**
```csharp
StackLayout stackLayout = new StackLayout { Padding = 20, Spacing = 10 };

SfCheckBox small = new SfCheckBox
{
    Text = "Small (20)",
    ControlSize = 20,
    FontSize = 12
};

SfCheckBox large = new SfCheckBox
{
    Text = "Large (40)",
    ControlSize = 40,
    FontSize = 20
};

stackLayout.Children.Add(small);
stackLayout.Children.Add(large);

this.Content = stackLayout;
```

## Font Auto-Scaling

The `FontAutoScalingEnabled` property enables automatic font scaling based on the OS text size settings.

### Syntax:

```csharp
public bool FontAutoScalingEnabled { get; set; }  // Default: false
```

### Usage:

**XAML:**
```xml
<buttons:SfCheckBox Text="Auto-Scaling Font" 
                    FontAutoScalingEnabled="True"
                    FontSize="16"/>
```

**C#:**
```csharp
SfCheckBox checkBox = new SfCheckBox
{
    Text = "Auto-Scaling Font",
    FontAutoScalingEnabled = true,
    FontSize = 16
};
```

### When to Enable:

- ✅ User-facing text in forms and settings
- ✅ Accessibility is a priority
- ✅ Text content is primary (not decorative)
- ❌ Precise layout is critical
- ❌ Tight space constraints

## Animation Control

The `EnabledAnimation` property enables or disables the state change animation.

### Syntax:

```csharp
public bool EnabledAnimation { get; set; }  // Default: true
```

### Usage:

**XAML:**
```xml
<StackLayout Padding="20" Spacing="10">
    <buttons:SfCheckBox Text="With Animation" 
                        EnabledAnimation="True"/>
    
    <buttons:SfCheckBox Text="Without Animation" 
                        EnabledAnimation="False"/>
</StackLayout>
```

**C#:**
```csharp
SfCheckBox animated = new SfCheckBox
{
    Text = "With Animation",
    EnabledAnimation = true
};

SfCheckBox instant = new SfCheckBox
{
    Text = "Without Animation",
    EnabledAnimation = false
};
```

### When to Disable:

- Performance optimization (many checkboxes)
- Accessibility preferences (reduced motion)
- Quick data entry scenarios
- Testing and automation

## Content Spacing

The `ContentSpacing` property adjusts the spacing between the checkbox box and the caption text.

### Syntax:

```csharp
public double ContentSpacing { get; set; }
```

### Usage:

**XAML:**
```xml
<StackLayout Padding="20" Spacing="15">
    <buttons:SfCheckBox Text="Tight Spacing (5)" 
                        ContentSpacing="5"/>
    
    <buttons:SfCheckBox Text="Normal Spacing (10)" 
                        ContentSpacing="10"/>
    
    <buttons:SfCheckBox Text="Wide Spacing (25)" 
                        ContentSpacing="25"/>
    
    <buttons:SfCheckBox Text="Extra Wide (40)" 
                        ContentSpacing="40"/>
</StackLayout>
```

**C#:**
```csharp
StackLayout stackLayout = new StackLayout { Padding = 20, Spacing = 15 };

SfCheckBox normal = new SfCheckBox
{
    Text = "Normal Spacing (10)",
    ContentSpacing = 10
};

SfCheckBox wide = new SfCheckBox
{
    Text = "Wide Spacing (25)",
    ContentSpacing = 25
};

stackLayout.Children.Add(normal);
stackLayout.Children.Add(wide);

this.Content = stackLayout;
```

## Complete Customization Examples

### Example 1: Modern Material Design Style

```xml
<buttons:SfCheckBox Text="Material Design" 
                    IsChecked="True"
                    CheckedColor="#6200EE"
                    TickColor="White"
                    UncheckedColor="#757575"
                    CornerRadius="3"
                    StrokeThickness="2"
                    TextColor="#212121"
                    FontSize="16"
                    ControlSize="20"
                    ContentSpacing="12"/>
```

### Example 2: High Contrast Accessibility

```xml
<buttons:SfCheckBox Text="High Contrast Mode" 
                    IsChecked="True"
                    CheckedColor="Black"
                    TickColor="Yellow"
                    UncheckedColor="Black"
                    StrokeThickness="3"
                    TextColor="Black"
                    FontSize="18"
                    FontAttributes="Bold"
                    ControlSize="44"
                    FontAutoScalingEnabled="True"/>
```

### Example 3: Soft Rounded Style

```xml
<buttons:SfCheckBox Text="Soft Design" 
                    IsChecked="True"
                    CheckedColor="#4CAF50"
                    TickColor="White"
                    UncheckedColor="#BDBDBD"
                    CornerRadius="12"
                    StrokeThickness="2"
                    TextColor="#424242"
                    FontSize="15"
                    ControlSize="24"
                    ContentSpacing="10"
                    EnabledAnimation="True"/>
```

### Example 4: Compact List Item

```xml
<buttons:SfCheckBox Text="Compact checkbox for dense lists with text truncation" 
                    ControlSize="18"
                    FontSize="14"
                    ContentSpacing="8"
                    LineBreakMode="TailTruncation"
                    WidthRequest="250"
                    EnabledAnimation="False"/>
```

### Example 5: Custom Branded Theme

```csharp
SfCheckBox brandedCheckBox = new SfCheckBox
{
    Text = "Subscribe to newsletter",
    IsChecked = false,
    CheckedColor = Color.FromArgb("#FF6B35"),      // Brand orange
    UncheckedColor = Color.FromArgb("#004E89"),    // Brand blue
    TickColor = Colors.White,
    CornerRadius = 6,
    StrokeThickness = 2.5,
    TextColor = Color.FromArgb("#004E89"),
    FontFamily = "Roboto",
    FontSize = 16,
    ControlSize = 24,
    ContentSpacing = 12,
    FontAutoScalingEnabled = true
};
```

## Best Practices

1. **Consistency**: Use the same styling for all checkboxes in a group
2. **Contrast**: Ensure sufficient color contrast for accessibility (WCAG AA: 4.5:1 for text)
3. **Touch Targets**: Minimum 44x44 for touch-friendly interfaces
4. **Proportions**: Scale `ControlSize`, `FontSize`, and `StrokeThickness` together
5. **Performance**: Disable animations (`EnabledAnimation="False"`) for large lists
6. **Wrapping**: Set `LineBreakMode` for long text labels
7. **Spacing**: Adjust `ContentSpacing` for visual balance
8. **Auto-Scaling**: Enable `FontAutoScalingEnabled` for accessibility
