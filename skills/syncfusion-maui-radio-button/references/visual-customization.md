# Visual Customization in .NET MAUI Radio Button

## Table of Contents
- [Overview](#overview)
- [State Color Customization](#state-color-customization)
- [Stroke Thickness](#stroke-thickness)
- [Text Appearance](#text-appearance)
- [Line Break Mode](#line-break-mode)
- [Size Customization](#size-customization)
- [Content Spacing](#content-spacing)
- [Font Auto Scaling](#font-auto-scaling)
- [Animation Control](#animation-control)
- [Complete Styling Examples](#complete-styling-examples)

## Overview

The Syncfusion .NET MAUI Radio Button provides extensive customization options to match your application's design requirements. You can customize colors, fonts, sizing, spacing, and more to create a consistent and polished user interface.

## State Color Customization

Customize the appearance of radio buttons in their checked and unchecked states using color properties.

### CheckedColor Property

The `CheckedColor` property sets the color of the radio button when it's in the checked state (when the inner circle is visible).

#### XAML

```xml
<buttons:SfRadioButton Text="Checked State" 
                       IsChecked="True" 
                       CheckedColor="#6200EE"/>
```

#### C#

```csharp
SfRadioButton radioButton = new SfRadioButton
{
    Text = "Checked State",
    IsChecked = true,
    CheckedColor = Color.FromArgb("#6200EE")
};
```

### UncheckedColor Property

The `UncheckedColor` property sets the color of the radio button when it's in the unchecked state (the outer circle).

#### XAML

```xml
<buttons:SfRadioButton Text="Unchecked State" 
                       UncheckedColor="#B0BEC5"/>
```

#### C#

```csharp
SfRadioButton radioButton = new SfRadioButton
{
    Text = "Unchecked State",
    UncheckedColor = Color.FromArgb("#B0BEC5")
};
```

### Both States Together

```xml
<buttons:SfRadioGroup>
    <buttons:SfRadioButton Text="Success" 
                           IsChecked="True" 
                           CheckedColor="#4CAF50"
                           UncheckedColor="#A5D6A7"/>
    <buttons:SfRadioButton Text="Warning" 
                           CheckedColor="#FF9800"
                           UncheckedColor="#FFE0B2"/>
    <buttons:SfRadioButton Text="Error" 
                           CheckedColor="#F44336"
                           UncheckedColor="#FFCDD2"/>
</buttons:SfRadioGroup>
```

### Theme-Based Colors

```csharp
SfRadioButton primaryButton = new SfRadioButton
{
    Text = "Primary",
    CheckedColor = Application.Current.RequestedTheme == AppTheme.Dark 
        ? Colors.LightBlue 
        : Colors.Blue,
    UncheckedColor = Application.Current.RequestedTheme == AppTheme.Dark 
        ? Colors.Gray 
        : Colors.LightGray
};
```

## Stroke Thickness

The `StrokeThickness` property controls the thickness of the radio button's circle border.

### Basic Usage

#### XAML

```xml
<buttons:SfRadioGroup>
    <buttons:SfRadioButton Text="Thin Border" StrokeThickness="1"/>
    <buttons:SfRadioButton Text="Normal Border" StrokeThickness="2" IsChecked="True"/>
    <buttons:SfRadioButton Text="Thick Border" StrokeThickness="4"/>
</buttons:SfRadioGroup>
```

#### C#

```csharp
SfRadioButton thickButton = new SfRadioButton
{
    Text = "Thick Border",
    StrokeThickness = 4,
    CheckedColor = Colors.Blue
};
```

### Recommended Values

- **Thin:** 1-1.5 (subtle, minimal appearance)
- **Normal:** 2-2.5 (default, balanced)
- **Thick:** 3-4 (bold, prominent)

## Text Appearance

Customize the caption text appearance using various font and color properties.

### TextColor

Sets the color of the caption text.

```xml
<buttons:SfRadioButton Text="Colored Text" 
                       TextColor="#1976D2" 
                       IsChecked="True"/>
```

```csharp
radioButton.TextColor = Color.FromArgb("#1976D2");
```

### FontSize

Controls the size of the caption text.

```xml
<buttons:SfRadioButton Text="Large Text" FontSize="20"/>
<buttons:SfRadioButton Text="Normal Text" FontSize="14"/>
<buttons:SfRadioButton Text="Small Text" FontSize="12"/>
```

```csharp
radioButton.FontSize = 18;
```

### FontFamily

Specifies the font family for the caption text.

```xml
<buttons:SfRadioButton Text="Custom Font" 
                       FontFamily="Arial"
                       FontSize="16"/>
```

```csharp
radioButton.FontFamily = "Arial";
```

**Note:** Ensure the font is available on the target platform or included as a custom font resource.

### FontAttributes

Sets font styling (Bold, Italic, or None).

```xml
<buttons:SfRadioButton Text="Bold Text" FontAttributes="Bold"/>
<buttons:SfRadioButton Text="Italic Text" FontAttributes="Italic"/>
```

```csharp
radioButton.FontAttributes = FontAttributes.Bold;
// Or combine multiple attributes
radioButton.FontAttributes = FontAttributes.Italic;
```

### HorizontalTextAlignment

Controls the horizontal alignment of the text.

```xml
<buttons:SfRadioButton Text="Left Aligned" 
                       HorizontalTextAlignment="Start"
                       WidthRequest="200"/>
<buttons:SfRadioButton Text="Center Aligned" 
                       HorizontalTextAlignment="Center"
                       WidthRequest="200"/>
<buttons:SfRadioButton Text="Right Aligned" 
                       HorizontalTextAlignment="End"
                       WidthRequest="200"/>
```

```csharp
radioButton.HorizontalTextAlignment = TextAlignment.Center;
```

### Complete Text Styling Example

```xml
<buttons:SfRadioButton Text="Premium Plan" 
                       IsChecked="True"
                       CheckedColor="#6200EE"
                       TextColor="#000000"
                       FontSize="18"
                       FontFamily="Arial"
                       FontAttributes="Bold"
                       HorizontalTextAlignment="Start"/>
```

```csharp
SfRadioButton styledButton = new SfRadioButton
{
    Text = "Premium Plan",
    IsChecked = true,
    CheckedColor = Color.FromArgb("#6200EE"),
    TextColor = Colors.Black,
    FontSize = 18,
    FontFamily = "Arial",
    FontAttributes = FontAttributes.Bold,
    HorizontalTextAlignment = TextAlignment.Start
};
```

## Line Break Mode

The `LineBreakMode` property controls how text is wrapped or truncated when it exceeds the available width.

### Available Modes

- **NoWrap:** Text is not wrapped (default)
- **WordWrap:** Wraps text at word boundaries
- **CharacterWrap:** Wraps text at character boundaries
- **HeadTruncation:** Truncates at the beginning (...end of text)
- **MiddleTruncation:** Truncates in the middle (start...end)
- **TailTruncation:** Truncates at the end (start of text...)

### NoWrap (Default)

```xml
<buttons:SfRadioButton Text="This is a very long text that will not wrap" 
                       LineBreakMode="NoWrap"
                       WidthRequest="200"/>
```

### WordWrap

Best for readable text wrapping:

```xml
<buttons:SfRadioButton Text="The LineBreakMode allows you to wrap or truncate the text at word boundaries for better readability" 
                       LineBreakMode="WordWrap"
                       WidthRequest="250"
                       IsChecked="True"/>
```

```csharp
SfRadioButton wrappedButton = new SfRadioButton
{
    Text = "The LineBreakMode allows you to wrap or truncate the text at word boundaries for better readability",
    LineBreakMode = LineBreakMode.WordWrap,
    WidthRequest = 250
};
```

### CharacterWrap

```xml
<buttons:SfRadioButton Text="Characterwrappingbreakslongtextatanycharacterboundary" 
                       LineBreakMode="CharacterWrap"
                       WidthRequest="200"/>
```

### Truncation Modes

```xml
<VerticalStackLayout WidthRequest="200" Spacing="10">
    <buttons:SfRadioButton Text="This is a very long text for truncation demo" 
                           LineBreakMode="HeadTruncation"/>
    <buttons:SfRadioButton Text="This is a very long text for truncation demo" 
                           LineBreakMode="MiddleTruncation"/>
    <buttons:SfRadioButton Text="This is a very long text for truncation demo" 
                           LineBreakMode="TailTruncation"/>
</VerticalStackLayout>
```

### Practical Example: Long Descriptions

```xml
<buttons:SfRadioGroup WidthRequest="300">
    <buttons:SfRadioButton Text="Basic Plan: $9.99/month - Includes core features and basic support" 
                           Value="basic"
                           LineBreakMode="WordWrap"
                           FontSize="14"/>
    <buttons:SfRadioButton Text="Premium Plan: $19.99/month - All features plus priority support and analytics" 
                           Value="premium"
                           LineBreakMode="WordWrap"
                           FontSize="14"/>
    <buttons:SfRadioButton Text="Enterprise Plan: $49.99/month - Everything in Premium plus dedicated account manager" 
                           Value="enterprise"
                           LineBreakMode="WordWrap"
                           FontSize="14"/>
</buttons:SfRadioGroup>
```

## Size Customization

The `ControlSize` property sets the size (diameter) of the radio button circle.

### Basic Sizing

```xml
<buttons:SfRadioGroup Spacing="15">
    <buttons:SfRadioButton Text="Small (20)" ControlSize="20"/>
    <buttons:SfRadioButton Text="Default (24)" ControlSize="24" IsChecked="True"/>
    <buttons:SfRadioButton Text="Medium (32)" ControlSize="32"/>
    <buttons:SfRadioButton Text="Large (40)" ControlSize="40"/>
</buttons:SfRadioGroup>
```

```csharp
SfRadioButton smallButton = new SfRadioButton
{
    Text = "Small",
    ControlSize = 20
};

SfRadioButton largeButton = new SfRadioButton
{
    Text = "Large",
    ControlSize = 40
};
```

### Matching Font Size to Control Size

For a balanced appearance, adjust font size proportionally:

```xml
<buttons:SfRadioButton Text="Small Radio" 
                       ControlSize="20" 
                       FontSize="12"/>
<buttons:SfRadioButton Text="Normal Radio" 
                       ControlSize="24" 
                       FontSize="14"/>
<buttons:SfRadioButton Text="Large Radio" 
                       ControlSize="32" 
                       FontSize="18"/>
<buttons:SfRadioButton Text="Extra Large Radio" 
                       ControlSize="40" 
                       FontSize="20"/>
```

### Accessibility Sizing

For better accessibility, use larger control sizes:

```csharp
SfRadioButton accessibleButton = new SfRadioButton
{
    Text = "Accessible Option",
    ControlSize = 36,  // Larger for easier tapping
    FontSize = 16      // Larger for better readability
};
```

## Content Spacing

The `ContentSpacing` property controls the spacing between the radio button circle and the caption text.

### Basic Usage

```xml
<buttons:SfRadioGroup>
    <buttons:SfRadioButton Text="Tight Spacing" ContentSpacing="5"/>
    <buttons:SfRadioButton Text="Normal Spacing" ContentSpacing="10" IsChecked="True"/>
    <buttons:SfRadioButton Text="Wide Spacing" ContentSpacing="20"/>
    <buttons:SfRadioButton Text="Extra Wide Spacing" ContentSpacing="30"/>
</buttons:SfRadioGroup>
```

```csharp
SfRadioButton spacedButton = new SfRadioButton
{
    Text = "Custom Spacing",
    ContentSpacing = 25
};
```

### Responsive Spacing

```csharp
// Adjust spacing based on device
double spacing = DeviceDisplay.MainDisplayInfo.Width < 400 ? 8 : 15;

radioButton.ContentSpacing = spacing;
```

## Font Auto Scaling

The `FontAutoScalingEnabled` property enables automatic font size adjustment based on the operating system's text size settings (accessibility feature).

### Enabling Auto Scaling

```xml
<buttons:SfRadioButton Text="Auto-scaled Text" 
                       FontAutoScalingEnabled="True"
                       FontSize="14"/>
```

```csharp
SfRadioButton accessibleButton = new SfRadioButton
{
    Text = "Auto-scaled Text",
    FontAutoScalingEnabled = true,
    FontSize = 14  // Base size that will scale
};
```

### When to Enable

- **Enable** for user-facing content where accessibility is important
- **Disable** for fixed-size layouts or design-critical text sizing
- **Default:** `false`

### Example: Accessibility-First Form

```xml
<buttons:SfRadioGroup>
    <buttons:SfRadioButton Text="Small Font" 
                           FontSize="12"
                           FontAutoScalingEnabled="True"/>
    <buttons:SfRadioButton Text="Medium Font" 
                           FontSize="16"
                           FontAutoScalingEnabled="True"/>
    <buttons:SfRadioButton Text="Large Font" 
                           FontSize="20"
                           FontAutoScalingEnabled="True"/>
</buttons:SfRadioGroup>
```

## Animation Control

The `EnabledAnimation` property controls whether state change animations are shown when toggling the radio button.

### Disabling Animation

```xml
<buttons:SfRadioButton Text="No Animation" 
                       EnabledAnimation="False"
                       IsChecked="True"/>
```

```csharp
SfRadioButton noAnimationButton = new SfRadioButton
{
    Text = "No Animation",
    EnabledAnimation = false
};
```

### When to Disable Animation

- Performance-sensitive scenarios with many radio buttons
- Programmatic state changes where animation is distracting
- Rapid-fire testing or automation
- Low-end devices or battery-saving modes

### Default Behavior

By default, `EnabledAnimation` is `true`, providing smooth visual feedback when states change.

## Complete Styling Examples

### Example 1: Material Design Style

```xml
<buttons:SfRadioGroup>
    <buttons:SfRadioButton Text="Material Design" 
                           IsChecked="True"
                           CheckedColor="#6200EE"
                           UncheckedColor="#757575"
                           TextColor="#000000"
                           FontSize="16"
                           FontFamily="Roboto"
                           ControlSize="24"
                           ContentSpacing="12"
                           StrokeThickness="2"/>
</buttons:SfRadioGroup>
```

### Example 2: iOS-Style Radio Buttons

```xml
<buttons:SfRadioGroup Spacing="12">
    <buttons:SfRadioButton Text="iOS Style" 
                           CheckedColor="#007AFF"
                           UncheckedColor="#C7C7CC"
                           TextColor="#000000"
                           FontSize="17"
                           FontFamily="San Francisco"
                           ControlSize="22"
                           ContentSpacing="10"/>
</buttons:SfRadioGroup>
```

### Example 3: High Contrast Accessibility

```xml
<buttons:SfRadioButton Text="High Contrast Mode" 
                       IsChecked="True"
                       CheckedColor="#FFFFFF"
                       UncheckedColor="#FFFFFF"
                       TextColor="#FFFFFF"
                       BackgroundColor="#000000"
                       FontSize="18"
                       FontAttributes="Bold"
                       ControlSize="32"
                       ContentSpacing="15"
                       StrokeThickness="3"
                       FontAutoScalingEnabled="True"/>
```

### Example 4: Custom Branded Radio Buttons

```xml
<buttons:SfRadioGroup Spacing="10">
    <buttons:SfRadioButton Text="Standard Plan" 
                           CheckedColor="#FF6B35"
                           UncheckedColor="#FFA07A"
                           TextColor="#2C3E50"
                           FontSize="16"
                           FontAttributes="Bold"
                           ControlSize="28"
                           ContentSpacing="12"/>
    <buttons:SfRadioButton Text="Pro Plan" 
                           CheckedColor="#FF6B35"
                           UncheckedColor="#FFA07A"
                           TextColor="#2C3E50"
                           FontSize="16"
                           FontAttributes="Bold"
                           ControlSize="28"
                           ContentSpacing="12"
                           IsChecked="True"/>
</buttons:SfRadioGroup>
```

```csharp
// Reusable styling method
private void ApplyBrandedStyle(SfRadioButton button)
{
    button.CheckedColor = Color.FromArgb("#FF6B35");
    button.UncheckedColor = Color.FromArgb("#FFA07A");
    button.TextColor = Color.FromArgb("#2C3E50");
    button.FontSize = 16;
    button.FontAttributes = FontAttributes.Bold;
    button.ControlSize = 28;
    button.ContentSpacing = 12;
}

// Apply to multiple buttons
ApplyBrandedStyle(option1);
ApplyBrandedStyle(option2);
ApplyBrandedStyle(option3);
```

### Example 5: Compact Form Layout

```xml
<buttons:SfRadioGroup Orientation="Horizontal" Spacing="5">
    <buttons:SfRadioButton Text="S" 
                           ControlSize="20"
                           FontSize="12"
                           ContentSpacing="5"
                           WidthRequest="60"
                           HorizontalTextAlignment="Center"/>
    <buttons:SfRadioButton Text="M" 
                           ControlSize="20"
                           FontSize="12"
                           ContentSpacing="5"
                           WidthRequest="60"
                           HorizontalTextAlignment="Center"/>
    <buttons:SfRadioButton Text="L" 
                           ControlSize="20"
                           FontSize="12"
                           ContentSpacing="5"
                           WidthRequest="60"
                           HorizontalTextAlignment="Center"/>
</buttons:SfRadioGroup>
```

### Example 6: Card-Style Selection

```xml
<buttons:SfRadioGroup Spacing="15">
    <buttons:SfRadioButton Text="Standard Shipping - 5-7 days" 
                           CheckedColor="#4CAF50"
                           UncheckedColor="#CCCCCC"
                           TextColor="#333333"
                           FontSize="14"
                           LineBreakMode="WordWrap"
                           WidthRequest="300"
                           ContentSpacing="15"
                           Padding="15"
                           BackgroundColor="#F5F5F5"
                           ControlSize="24"/>
    <buttons:SfRadioButton Text="Express Shipping - 2-3 days (+$5.99)" 
                           CheckedColor="#4CAF50"
                           UncheckedColor="#CCCCCC"
                           TextColor="#333333"
                           FontSize="14"
                           LineBreakMode="WordWrap"
                           WidthRequest="300"
                           ContentSpacing="15"
                           Padding="15"
                           BackgroundColor="#F5F5F5"
                           ControlSize="24"
                           IsChecked="True"/>
</buttons:SfRadioGroup>
```

### Example 7: Dark Theme Radio Buttons

```xml
<ContentPage BackgroundColor="#121212">
    <buttons:SfRadioGroup>
        <buttons:SfRadioButton Text="Dark Theme Option 1" 
                               CheckedColor="#BB86FC"
                               UncheckedColor="#6D6D6D"
                               TextColor="#FFFFFF"
                               FontSize="16"
                               ControlSize="24"
                               IsChecked="True"/>
        <buttons:SfRadioButton Text="Dark Theme Option 2" 
                               CheckedColor="#BB86FC"
                               UncheckedColor="#6D6D6D"
                               TextColor="#FFFFFF"
                               FontSize="16"
                               ControlSize="24"/>
    </buttons:SfRadioGroup>
</ContentPage>
```

## Styling Best Practices

### 1. Maintain Consistency

Apply the same styling across all radio buttons in a group:

```csharp
private void ApplyGroupStyle(SfRadioGroup group, RadioButtonStyle style)
{
    foreach (var child in group.Children.OfType<SfRadioButton>())
    {
        child.CheckedColor = style.CheckedColor;
        child.UncheckedColor = style.UncheckedColor;
        child.TextColor = style.TextColor;
        child.FontSize = style.FontSize;
        child.ControlSize = style.ControlSize;
        child.ContentSpacing = style.ContentSpacing;
    }
}

public class RadioButtonStyle
{
    public Color CheckedColor { get; set; }
    public Color UncheckedColor { get; set; }
    public Color TextColor { get; set; }
    public double FontSize { get; set; }
    public double ControlSize { get; set; }
    public double ContentSpacing { get; set; }
}
```

### 2. Use Resources for Shared Styles

```xml
<ContentPage.Resources>
    <Color x:Key="PrimaryColor">#6200EE</Color>
    <Color x:Key="OnSurfaceColor">#000000</Color>
    <x:Double x:Key="StandardFontSize">16</x:Double>
    <x:Double x:Key="StandardControlSize">24</x:Double>
</ContentPage.Resources>

<buttons:SfRadioButton Text="Styled Button" 
                       CheckedColor="{StaticResource PrimaryColor}"
                       TextColor="{StaticResource OnSurfaceColor}"
                       FontSize="{StaticResource StandardFontSize}"
                       ControlSize="{StaticResource StandardControlSize}"/>
```

### 3. Consider Platform Differences

```csharp
private void ApplyPlatformSpecificStyle(SfRadioButton button)
{
    button.CheckedColor = Color.FromArgb("#6200EE");
    button.TextColor = Colors.Black;
    button.FontSize = 16;
    button.ControlSize = 24;
    button.ContentSpacing = 12;
    
#if !ANDROID
    button.StrokeThickness = 2;
#endif
    
#if IOS
    button.FontFamily = "San Francisco";
#elif ANDROID
    button.FontFamily = "Roboto";
#elif WINDOWS
    button.FontFamily = "Segoe UI";
#endif
}
```

### 4. Ensure Sufficient Color Contrast

For accessibility, maintain adequate contrast between colors:

```csharp
// Good contrast for light backgrounds
button.CheckedColor = Color.FromArgb("#1976D2");  // Dark blue
button.TextColor = Colors.Black;

// Good contrast for dark backgrounds
button.CheckedColor = Color.FromArgb("#90CAF9");  // Light blue
button.TextColor = Colors.White;
```

### 5. Test with Long Text

Always test your radio buttons with various text lengths:

```xml
<buttons:SfRadioGroup WidthRequest="300">
    <buttons:SfRadioButton Text="Short" LineBreakMode="WordWrap"/>
    <buttons:SfRadioButton Text="Medium length option" LineBreakMode="WordWrap"/>
    <buttons:SfRadioButton Text="This is a much longer option that will demonstrate how the text wraps" 
                           LineBreakMode="WordWrap"/>
</buttons:SfRadioGroup>
```

### 6. Responsive Design

Adjust styling based on available space:

```csharp
private void AdjustForScreenSize()
{
    var width = DeviceDisplay.MainDisplayInfo.Width;
    
    if (width < 400)  // Small screen
    {
        radioButton.FontSize = 12;
        radioButton.ControlSize = 20;
        radioButton.ContentSpacing = 8;
    }
    else if (width < 800)  // Medium screen
    {
        radioButton.FontSize = 14;
        radioButton.ControlSize = 24;
        radioButton.ContentSpacing = 10;
    }
    else  // Large screen
    {
        radioButton.FontSize = 16;
        radioButton.ControlSize = 28;
        radioButton.ContentSpacing = 12;
    }
}
```

## Common Pitfalls

### 1. Inconsistent Sizing

**❌ Avoid:**
```xml
<buttons:SfRadioGroup>
    <buttons:SfRadioButton Text="Option 1" ControlSize="20" FontSize="18"/>
    <buttons:SfRadioButton Text="Option 2" ControlSize="32" FontSize="12"/>
</buttons:SfRadioGroup>
```

**✅ Better:**
```xml
<buttons:SfRadioGroup>
    <buttons:SfRadioButton Text="Option 1" ControlSize="24" FontSize="16"/>
    <buttons:SfRadioButton Text="Option 2" ControlSize="24" FontSize="16"/>
</buttons:SfRadioGroup>
```

### 2. Poor Color Contrast

**❌ Avoid:**
```xml
<buttons:SfRadioButton Text="Low Contrast" 
                       CheckedColor="#E0E0E0"
                       TextColor="#CCCCCC"/>
```

**✅ Better:**
```xml
<buttons:SfRadioButton Text="Good Contrast" 
                       CheckedColor="#1976D2"
                       TextColor="#000000"/>
```

### 3. Text Overflow Without LineBreakMode

**❌ Avoid:**
```xml
<buttons:SfRadioButton Text="This is a very long text that will overflow" 
                       WidthRequest="150"/>
```

**✅ Better:**
```xml
<buttons:SfRadioButton Text="This is a very long text that will wrap properly" 
                       WidthRequest="150"
                       LineBreakMode="WordWrap"/>
```
