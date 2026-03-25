# Customization and Styling in .NET MAUI TimePicker

## Table of Contents
- [Overview](#overview)
- [Header View Customization](#header-view-customization)
- [Footer View Customization](#footer-view-customization)
- [Column Header View Customization](#column-header-view-customization)
- [Selection View Customization](#selection-view-customization)
- [Text Display Modes](#text-display-modes)
- [Liquid Glass Effect](#liquid-glass-effect)
- [Complete Styling Examples](#complete-styling-examples)

## Overview

The TimePicker provides extensive customization options for all visual elements:

- **HeaderView** - Title bar customization
- **FooterView** - Button text and styling
- **ColumnHeaderView** - Column labels (Hour, Minute, Second, AM/PM)
- **SelectionView** - Highlight box for selected item
- **TextDisplayMode** - Visual effects for non-selected items
- **Liquid Glass Effect** - Modern translucent design

## Header View Customization

The header view appears at the top of the picker and can be fully customized.

### Header Properties

**PickerHeaderView Properties:**
- `Text` (string) - Header text
- `Height` (double) - Header height (must be > 0 to show)
- `Background` (Brush) - Background color/gradient
- `DividerColor` (Color) - Bottom divider line color
- `TextStyle` (PickerTextStyle) - Font, size, color, attributes

### Basic Header Customization

**XAML:**
```xml
<picker:SfTimePicker x:Name="timePicker">
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView Text="Select Time" 
                                 Height="50"
                                 Background="#6200EE"
                                 DividerColor="#BB86FC">
            <picker:PickerHeaderView.TextStyle>
                <picker:PickerTextStyle TextColor="White" 
                                       FontSize="18" 
                                       FontAttributes="Bold" />
            </picker:PickerHeaderView.TextStyle>
        </picker:PickerHeaderView>
    </picker:SfTimePicker.HeaderView>
</picker:SfTimePicker>
```

**C#:**
```csharp
SfTimePicker timePicker = new SfTimePicker();

timePicker.HeaderView = new PickerHeaderView()
{
    Text = "Select Time",
    Height = 50,
    Background = new SolidColorBrush(Color.FromArgb("#6200EE")),
    DividerColor = Color.FromArgb("#BB86FC"),
    TextStyle = new PickerTextStyle()
    {
        TextColor = Colors.White,
        FontSize = 18,
        FontAttributes = FontAttributes.Bold
    }
};

this.Content = timePicker;
```

### Header with Gradient Background

```xml
<picker:SfTimePicker.HeaderView>
    <picker:PickerHeaderView Text="Appointment Time" Height="60">
        <picker:PickerHeaderView.Background>
            <LinearGradientBrush StartPoint="0,0" EndPoint="1,0">
                <GradientStop Color="#6200EE" Offset="0.0" />
                <GradientStop Color="#3700B3" Offset="1.0" />
            </LinearGradientBrush>
        </picker:PickerHeaderView.Background>
        <picker:PickerHeaderView.TextStyle>
            <picker:PickerTextStyle TextColor="White" 
                                   FontSize="20" 
                                   FontAttributes="Bold"
                                   FontFamily="Arial" />
        </picker:PickerHeaderView.TextStyle>
    </picker:PickerHeaderView>
</picker:SfTimePicker.HeaderView>
```

### PickerTextStyle Properties

```csharp
public class PickerTextStyle
{
    public Color TextColor { get; set; }          // Text color
    public double FontSize { get; set; }           // Font size
    public string FontFamily { get; set; }         // Font family name
    public FontAttributes FontAttributes { get; set; }  // None, Bold, Italic
}
```

**Example:**
```csharp
var textStyle = new PickerTextStyle()
{
    TextColor = Colors.DarkBlue,
    FontSize = 16,
    FontFamily = "OpenSans-Semibold",
    FontAttributes = FontAttributes.Bold | FontAttributes.Italic
};
```

## Footer View Customization

The footer view contains OK and Cancel buttons for confirming or cancelling the selection.

### Footer Properties

**PickerFooterView Properties:**
- `Height` (double) - Footer height (must be > 0 to show)
- `ShowOkButton` (bool) - Show/hide OK button
- `OkButtonText` (string) - OK button text (default: "OK")
- `CancelButtonText` (string) - Cancel button text (default: "Cancel")
- `Background` (Brush) - Background color/gradient
- `TextStyle` (PickerTextStyle) - Button text styling

### Basic Footer Customization

**XAML:**
```xml
<picker:SfTimePicker x:Name="timePicker">
    <picker:SfTimePicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True"
                                 Height="50"
                                 Background="#F5F5F5"
                                 OkButtonText="Confirm"
                                 CancelButtonText="Reset">
            <picker:PickerFooterView.TextStyle>
                <picker:PickerTextStyle TextColor="#6200EE" 
                                       FontSize="16" 
                                       FontAttributes="Bold" />
            </picker:PickerFooterView.TextStyle>
        </picker:PickerFooterView>
    </picker:SfTimePicker.FooterView>
</picker:SfTimePicker>
```

**C#:**
```csharp
timePicker.FooterView = new PickerFooterView()
{
    ShowOkButton = true,
    Height = 50,
    Background = new SolidColorBrush(Color.FromArgb("#F5F5F5")),
    OkButtonText = "Confirm",
    CancelButtonText = "Reset",
    TextStyle = new PickerTextStyle()
    {
        TextColor = Color.FromArgb("#6200EE"),
        FontSize = 16,
        FontAttributes = FontAttributes.Bold
    }
};
```

### Footer with Custom Colors

```xml
<picker:SfTimePicker.FooterView>
    <picker:PickerFooterView ShowOkButton="True"
                             Height="60"
                             Background="#263238"
                             OkButtonText="✓ Accept"
                             CancelButtonText="✗ Cancel">
        <picker:PickerFooterView.TextStyle>
            <picker:PickerTextStyle TextColor="#4CAF50" 
                                   FontSize="18" 
                                   FontAttributes="Bold" />
        </picker:PickerFooterView.TextStyle>
    </picker:PickerFooterView>
</picker:SfTimePicker.FooterView>
```

## Column Header View Customization

Column headers display labels for each time component (Hour, Minute, Second, AM/PM).

### Column Header Properties

**TimePickerColumnHeaderView Properties:**
- `Height` (double) - Column header height
- `HourHeaderText` (string) - Hour column label (default: "Hour")
- `MinuteHeaderText` (string) - Minute column label (default: "Minute")
- `SecondHeaderText` (string) - Second column label (default: "Second")
- `MeridiemHeaderText` (string) - AM/PM column label (default: "")
- `MillisecondHeaderText` (string) - Millisecond column label (default: "Millisecond")
- `Background` (Brush) - Background color
- `TextStyle` (PickerTextStyle) - Text styling
- `DividerColor` (Color) - Bottom divider color

### Basic Column Header Customization

**XAML:**
```xml
<picker:SfTimePicker x:Name="timePicker" Format="hh_mm_ss_tt">
    <picker:SfTimePicker.ColumnHeaderView>
        <picker:TimePickerColumnHeaderView Height="40"
                                          HourHeaderText="HR"
                                          MinuteHeaderText="MIN"
                                          SecondHeaderText="SEC"
                                          MeridiemHeaderText="Period"
                                          Background="#E3F2FD">
            <picker:TimePickerColumnHeaderView.TextStyle>
                <picker:PickerTextStyle TextColor="#1976D2" 
                                       FontSize="14" 
                                       FontAttributes="Bold" />
            </picker:TimePickerColumnHeaderView.TextStyle>
        </picker:TimePickerColumnHeaderView>
    </picker:SfTimePicker.ColumnHeaderView>
</picker:SfTimePicker>
```

**C#:**
```csharp
timePicker.ColumnHeaderView = new TimePickerColumnHeaderView()
{
    Height = 40,
    HourHeaderText = "HR",
    MinuteHeaderText = "MIN",
    SecondHeaderText = "SEC",
    MeridiemHeaderText = "Period",
    Background = new SolidColorBrush(Color.FromArgb("#E3F2FD")),
    TextStyle = new PickerTextStyle()
    {
        TextColor = Color.FromArgb("#1976D2"),
        FontSize = 14,
        FontAttributes = FontAttributes.Bold
    }
};
```

### Localized Column Headers

```xml
<picker:SfTimePicker.ColumnHeaderView>
    <picker:TimePickerColumnHeaderView Height="35"
                                      HourHeaderText="Heure"
                                      MinuteHeaderText="Minute"
                                      SecondHeaderText="Seconde">
        <picker:TimePickerColumnHeaderView.TextStyle>
            <picker:PickerTextStyle TextColor="DarkSlateGray" FontSize="13" />
        </picker:TimePickerColumnHeaderView.TextStyle>
    </picker:TimePickerColumnHeaderView>
</picker:SfTimePicker.ColumnHeaderView>
```

## Selection View Customization

The selection view is the highlight box that indicates the currently selected item.

### Selection View Properties

**PickerSelectionView Properties:**
- `Background` (Brush) - Fill color
- `Stroke` (Brush) - Border color
- `StrokeThickness` (double) - Border width
- `CornerRadius` (CornerRadius) - Rounded corners
- `TextStyle` (PickerTextStyle) - Selected text styling

### Basic Selection View Customization

**XAML:**
```xml
<picker:SfTimePicker x:Name="timePicker">
    <picker:SfTimePicker.SelectionView>
        <picker:PickerSelectionView Background="#E3F2FD"
                                    Stroke="#1976D2"
                                    StrokeThickness="2"
                                    CornerRadius="8">
            <picker:PickerSelectionView.TextStyle>
                <picker:PickerTextStyle TextColor="#1976D2" 
                                       FontSize="20" 
                                       FontAttributes="Bold" />
            </picker:PickerSelectionView.TextStyle>
        </picker:PickerSelectionView>
    </picker:SfTimePicker.SelectionView>
</picker:SfTimePicker>
```

**C#:**
```csharp
timePicker.SelectionView = new PickerSelectionView()
{
    Background = new SolidColorBrush(Color.FromArgb("#E3F2FD")),
    Stroke = new SolidColorBrush(Color.FromArgb("#1976D2")),
    StrokeThickness = 2,
    CornerRadius = new CornerRadius(8),
    TextStyle = new PickerTextStyle()
    {
        TextColor = Color.FromArgb("#1976D2"),
        FontSize = 20,
        FontAttributes = FontAttributes.Bold
    }
};
```

### Selection View with Gradient

```xml
<picker:SfTimePicker.SelectionView>
    <picker:PickerSelectionView StrokeThickness="3" CornerRadius="12">
        <picker:PickerSelectionView.Background>
            <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
                <GradientStop Color="#FFEB3B" Offset="0.0" />
                <GradientStop Color="#FFC107" Offset="1.0" />
            </LinearGradientBrush>
        </picker:PickerSelectionView.Background>
        <picker:PickerSelectionView.Stroke>
            <SolidColorBrush Color="#FF6F00" />
        </picker:PickerSelectionView.Stroke>
        <picker:PickerSelectionView.TextStyle>
            <picker:PickerTextStyle TextColor="#FF6F00" 
                                   FontSize="22" 
                                   FontAttributes="Bold" />
        </picker:PickerSelectionView.TextStyle>
    </picker:PickerSelectionView>
</picker:SfTimePicker.SelectionView>
```

### Transparent Selection View

```xml
<picker:SfTimePicker.SelectionView>
    <picker:PickerSelectionView Background="Transparent"
                                Stroke="#4CAF50"
                                StrokeThickness="2"
                                CornerRadius="10">
        <picker:PickerSelectionView.TextStyle>
            <picker:PickerTextStyle TextColor="#4CAF50" 
                                   FontSize="20" 
                                   FontAttributes="Bold" />
        </picker:PickerSelectionView.TextStyle>
    </picker:PickerSelectionView>
</picker:SfTimePicker.SelectionView>
```

## Text Display Modes

The `TextDisplayMode` property controls how non-selected items are displayed relative to the selected item.

### Available Modes

```csharp
public enum PickerTextDisplayMode
{
    Default,        // Normal display
    Fade,           // Gradual opacity decrease
    Shrink,         // Gradual font size decrease
    FadeAndShrink   // Both opacity and size decrease
}
```

### Default Mode

Normal display without visual effects.

```xml
<picker:SfTimePicker TextDisplayMode="Default" />
```

### Fade Mode

Items farther from selection have reduced opacity.

**XAML:**
```xml
<picker:SfTimePicker x:Name="timePicker"
                     TextDisplayMode="Fade"
                     Format="hh_mm_tt">
</picker:SfTimePicker>
```

**C#:**
```csharp
timePicker.TextDisplayMode = PickerTextDisplayMode.Fade;
```

### Shrink Mode

Items farther from selection have smaller font size.

**XAML:**
```xml
<picker:SfTimePicker x:Name="timePicker"
                     TextDisplayMode="Shrink"
                     Format="hh_mm_tt">
</picker:SfTimePicker>
```

**C#:**
```csharp
timePicker.TextDisplayMode = PickerTextDisplayMode.Shrink;
```

### FadeAndShrink Mode

Combines both opacity and size reduction.

**XAML:**
```xml
<picker:SfTimePicker x:Name="timePicker"
                     TextDisplayMode="FadeAndShrink"
                     Format="hh_mm_tt">
</picker:SfTimePicker>
```

**C#:**
```csharp
timePicker.TextDisplayMode = PickerTextDisplayMode.FadeAndShrink;
```

**Visual Effect:** Creates a 3D-like depth effect with the selected item appearing most prominent.

### Comparison Example

```xml
<StackLayout Padding="20" Spacing="20">
    
    <Label Text="Default Mode" FontAttributes="Bold" />
    <picker:SfTimePicker TextDisplayMode="Default" HeightRequest="200" />
    
    <Label Text="Fade Mode" FontAttributes="Bold" />
    <picker:SfTimePicker TextDisplayMode="Fade" HeightRequest="200" />
    
    <Label Text="Shrink Mode" FontAttributes="Bold" />
    <picker:SfTimePicker TextDisplayMode="Shrink" HeightRequest="200" />
    
    <Label Text="Fade and Shrink Mode" FontAttributes="Bold" />
    <picker:SfTimePicker TextDisplayMode="FadeAndShrink" HeightRequest="200" />
    
</StackLayout>
```

## Liquid Glass Effect

The Liquid Glass Effect creates a modern, translucent design with adaptive color tinting.

### Prerequisites

1. Install **Syncfusion.Maui.Core** NuGet package
2. Use **SfGlassEffectView** to wrap the TimePicker

### Enabling Liquid Glass Effect

**Step 1:** Import namespaces

```xml
xmlns:picker="clr-namespace:Syncfusion.Maui.Picker;assembly=Syncfusion.Maui.Picker"
xmlns:core="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
```

**Step 2:** Wrap TimePicker in SfGlassEffectView

```xml
<ContentPage>
    <Grid>
        <!-- Background -->
        <Grid.Background>
            <LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
                <GradientStop Color="#0F4C75" Offset="0.0"/>
                <GradientStop Color="#3282B8" Offset="0.5"/>
                <GradientStop Color="#1B262C" Offset="1.0"/>
            </LinearGradientBrush>
        </Grid.Background>
        
        <!-- Glass Effect -->
        <core:SfGlassEffectView CornerRadius="20"
                               Margin="20"
                               VerticalOptions="Center">
            <picker:SfTimePicker EnableLiquidGlassEffect="True"
                                Background="Transparent"
                                Format="hh_mm_tt"
                                HeightRequest="300">
                <picker:SfTimePicker.HeaderView>
                    <picker:PickerHeaderView Text="Select Time" 
                                            Height="50"
                                            Background="Transparent">
                        <picker:PickerHeaderView.TextStyle>
                            <picker:PickerTextStyle TextColor="White" 
                                                   FontSize="18" 
                                                   FontAttributes="Bold" />
                        </picker:PickerHeaderView.TextStyle>
                    </picker:PickerHeaderView>
                </picker:SfTimePicker.HeaderView>
                <picker:SfTimePicker.FooterView>
                    <picker:PickerFooterView ShowOkButton="True" 
                                            Height="50"
                                            Background="Transparent" />
                </picker:SfTimePicker.FooterView>
            </picker:SfTimePicker>
        </core:SfGlassEffectView>
    </Grid>
</ContentPage>
```

**Step 3:** Enable liquid glass effect

```csharp
timePicker.EnableLiquidGlassEffect = true;
timePicker.Background = Brushes.Transparent;
```

### Liquid Glass with Custom Background

```xml
<core:SfGlassEffectView CornerRadius="25" 
                       Margin="30"
                       VerticalOptions="Center"
                       BackgroundColor="#80FFFFFF">
    <picker:SfTimePicker EnableLiquidGlassEffect="True"
                        Background="Transparent"
                        Format="hh_mm_ss_tt"
                        TextDisplayMode="FadeAndShrink">
        <picker:SfTimePicker.SelectionView>
            <picker:PickerSelectionView Background="#40FFFFFF"
                                       Stroke="White"
                                       StrokeThickness="2"
                                       CornerRadius="10">
                <picker:PickerSelectionView.TextStyle>
                    <picker:PickerTextStyle TextColor="White" 
                                           FontSize="22" 
                                           FontAttributes="Bold" />
                </picker:PickerSelectionView.TextStyle>
            </picker:PickerSelectionView>
        </picker:SfTimePicker.SelectionView>
    </picker:SfTimePicker>
</core:SfGlassEffectView>
```

## Complete Styling Examples

### Example 1: Material Design Theme

```xml
<picker:SfTimePicker Format="hh_mm_tt" HeightRequest="320">
    <!-- Header -->
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView Text="Select Time" 
                                Height="60"
                                Background="#6200EE">
            <picker:PickerHeaderView.TextStyle>
                <picker:PickerTextStyle TextColor="White" 
                                       FontSize="20" 
                                       FontAttributes="Bold" />
            </picker:PickerHeaderView.TextStyle>
        </picker:PickerHeaderView>
    </picker:SfTimePicker.HeaderView>
    
    <!-- Column Headers -->
    <picker:SfTimePicker.ColumnHeaderView>
        <picker:TimePickerColumnHeaderView Height="40"
                                          HourHeaderText="Hour"
                                          MinuteHeaderText="Minute"
                                          MeridiemHeaderText="Period"
                                          Background="#F5F5F5">
            <picker:TimePickerColumnHeaderView.TextStyle>
                <picker:PickerTextStyle TextColor="#6200EE" 
                                       FontSize="14" 
                                       FontAttributes="Bold" />
            </picker:TimePickerColumnHeaderView.TextStyle>
        </picker:TimePickerColumnHeaderView>
    </picker:SfTimePicker.ColumnHeaderView>
    
    <!-- Selection -->
    <picker:SfTimePicker.SelectionView>
        <picker:PickerSelectionView Background="#E8DEF8"
                                    Stroke="#6200EE"
                                    StrokeThickness="2"
                                    CornerRadius="8">
            <picker:PickerSelectionView.TextStyle>
                <picker:PickerTextStyle TextColor="#6200EE" 
                                       FontSize="20" 
                                       FontAttributes="Bold" />
            </picker:PickerSelectionView.TextStyle>
        </picker:PickerSelectionView>
    </picker:SfTimePicker.SelectionView>
    
    <!-- Footer -->
    <picker:SfTimePicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True"
                                Height="60"
                                Background="White"
                                OkButtonText="OK"
                                CancelButtonText="CANCEL">
            <picker:PickerFooterView.TextStyle>
                <picker:PickerTextStyle TextColor="#6200EE" 
                                       FontSize="16" 
                                       FontAttributes="Bold" />
            </picker:PickerFooterView.TextStyle>
        </picker:PickerFooterView>
    </picker:SfTimePicker.FooterView>
</picker:SfTimePicker>
```

### Example 2: Dark Theme

```xml
<picker:SfTimePicker Format="HH_mm" 
                     HeightRequest="300"
                     TextDisplayMode="FadeAndShrink"
                     Background="#1E1E1E">
    <!-- Header -->
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView Text="Military Time" 
                                Height="55"
                                Background="#2D2D30"
                                DividerColor="#3E3E42">
            <picker:PickerHeaderView.TextStyle>
                <picker:PickerTextStyle TextColor="#E0E0E0" 
                                       FontSize="18" 
                                       FontAttributes="Bold" />
            </picker:PickerHeaderView.TextStyle>
        </picker:PickerHeaderView>
    </picker:SfTimePicker.HeaderView>
    
    <!-- Column Headers -->
    <picker:SfTimePicker.ColumnHeaderView>
        <picker:TimePickerColumnHeaderView Height="35"
                                          HourHeaderText="HR"
                                          MinuteHeaderText="MIN"
                                          Background="#252526">
            <picker:TimePickerColumnHeaderView.TextStyle>
                <picker:PickerTextStyle TextColor="#BB86FC" FontSize="13" />
            </picker:TimePickerColumnHeaderView.TextStyle>
        </picker:TimePickerColumnHeaderView>
    </picker:SfTimePicker.ColumnHeaderView>
    
    <!-- Selection -->
    <picker:SfTimePicker.SelectionView>
        <picker:PickerSelectionView Background="#3E3E42"
                                    Stroke="#BB86FC"
                                    StrokeThickness="2"
                                    CornerRadius="6">
            <picker:PickerSelectionView.TextStyle>
                <picker:PickerTextStyle TextColor="#BB86FC" 
                                       FontSize="22" 
                                       FontAttributes="Bold" />
            </picker:PickerSelectionView.TextStyle>
        </picker:PickerSelectionView>
    </picker:SfTimePicker.SelectionView>
    
    <!-- Footer -->
    <picker:SfTimePicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True"
                                Height="55"
                                Background="#252526">
            <picker:PickerFooterView.TextStyle>
                <picker:PickerTextStyle TextColor="#BB86FC" FontSize="15" />
            </picker:PickerFooterView.TextStyle>
        </picker:PickerFooterView>
    </picker:SfTimePicker.FooterView>
</picker:SfTimePicker>
```

### Example 3: Colorful Theme

```xml
<picker:SfTimePicker Format="h_mm_ss_tt" 
                     HeightRequest="350"
                     TextDisplayMode="Fade">
    <!-- Header with Gradient -->
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView Text="🕐 Pick Your Time" Height="65">
            <picker:PickerHeaderView.Background>
                <LinearGradientBrush StartPoint="0,0" EndPoint="1,0">
                    <GradientStop Color="#FF6B6B" Offset="0.0" />
                    <GradientStop Color="#4ECDC4" Offset="1.0" />
                </LinearGradientBrush>
            </picker:PickerHeaderView.Background>
            <picker:PickerHeaderView.TextStyle>
                <picker:PickerTextStyle TextColor="White" 
                                       FontSize="22" 
                                       FontAttributes="Bold" />
            </picker:PickerHeaderView.TextStyle>
        </picker:PickerHeaderView>
    </picker:SfTimePicker.HeaderView>
    
    <!-- Column Headers -->
    <picker:SfTimePicker.ColumnHeaderView>
        <picker:TimePickerColumnHeaderView Height="40"
                                          Background="#FFE66D">
            <picker:TimePickerColumnHeaderView.TextStyle>
                <picker:PickerTextStyle TextColor="#495057" 
                                       FontSize="14" 
                                       FontAttributes="Bold" />
            </picker:TimePickerColumnHeaderView.TextStyle>
        </picker:TimePickerColumnHeaderView>
    </picker:SfTimePicker.ColumnHeaderView>
    
    <!-- Selection -->
    <picker:SfTimePicker.SelectionView>
        <picker:PickerSelectionView Background="#FFF3E0"
                                    Stroke="#FF6B6B"
                                    StrokeThickness="3"
                                    CornerRadius="15">
            <picker:PickerSelectionView.TextStyle>
                <picker:PickerTextStyle TextColor="#D73A49" 
                                       FontSize="24" 
                                       FontAttributes="Bold" />
            </picker:PickerSelectionView.TextStyle>
        </picker:PickerSelectionView>
    </picker:SfTimePicker.SelectionView>
    
    <!-- Footer -->
    <picker:SfTimePicker.FooterView>
        <picker:PickerFooterView ShowOkButton="True"
                                Height="60"
                                Background="#F8F9FA"
                                OkButtonText="✓ Done"
                                CancelButtonText="✗ Cancel">
            <picker:PickerFooterView.TextStyle>
                <picker:PickerTextStyle TextColor="#4ECDC4" 
                                       FontSize="17" 
                                       FontAttributes="Bold" />
            </picker:PickerFooterView.TextStyle>
        </picker:PickerFooterView>
    </picker:SfTimePicker.FooterView>
</picker:SfTimePicker>
```

## Best Practices

### 1. Maintain Sufficient Contrast

Ensure text is readable against backgrounds:

```xml
<!-- Good contrast -->
<picker:PickerHeaderView Background="#1976D2">
    <picker:PickerHeaderView.TextStyle>
        <picker:PickerTextStyle TextColor="White" />
    </picker:PickerHeaderView.TextStyle>
</picker:PickerHeaderView>

<!-- Poor contrast - avoid -->
<!-- <picker:PickerHeaderView Background="#E3F2FD">
    <picker:PickerHeaderView.TextStyle>
        <picker:PickerTextStyle TextColor="White" />
    </picker:PickerHeaderView.TextStyle>
</picker:PickerHeaderView> -->
```

### 2. Consistent Styling Across App

Define reusable styles:

```xml
<ContentPage.Resources>
    <picker:PickerTextStyle x:Key="HeaderTextStyle"
                           TextColor="White"
                           FontSize="18"
                           FontAttributes="Bold" />
    
    <picker:PickerTextStyle x:Key="SelectionTextStyle"
                           TextColor="#1976D2"
                           FontSize="20"
                           FontAttributes="Bold" />
</ContentPage.Resources>

<picker:SfTimePicker>
    <picker:SfTimePicker.HeaderView>
        <picker:PickerHeaderView TextStyle="{StaticResource HeaderTextStyle}" />
    </picker:SfTimePicker.HeaderView>
    <picker:SfTimePicker.SelectionView>
        <picker:PickerSelectionView TextStyle="{StaticResource SelectionTextStyle}" />
    </picker:SfTimePicker.SelectionView>
</picker:SfTimePicker>
```

### 3. Test with Different Content

Ensure styling works with all time formats:

```csharp
// Test with different formats
timePicker.Format = PickerTimeFormat.HH_mm_ss_fff; // Longest
timePicker.Format = PickerTimeFormat.hh_tt;        // Shortest
```

### 4. Use TextDisplayMode for Better UX

Enhance visual hierarchy:

```xml
<picker:SfTimePicker TextDisplayMode="FadeAndShrink" />
```

### 5. Transparent Backgrounds for Glass Effect

```xml
<picker:SfTimePicker EnableLiquidGlassEffect="True"
                    Background="Transparent" />
```

## Summary

The TimePicker provides extensive customization capabilities:

- **HeaderView** - Title, colors, gradients, text styling, divider
- **FooterView** - Button labels, styling, background
- **ColumnHeaderView** - Column labels, localization, styling
- **SelectionView** - Highlight box with borders, corners, colors
- **TextDisplayMode** - Fade, Shrink, or FadeAndShrink for depth effect
- **Liquid Glass Effect** - Modern translucent design with SfGlassEffectView
- **Complete themes** - Material Design, Dark Mode, Custom colors

Customize all visual elements to match your application's design language and branding requirements.
