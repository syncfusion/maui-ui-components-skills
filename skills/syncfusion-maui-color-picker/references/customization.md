# Color Picker Customization

This guide covers extensive customization options for the SfColorPicker, including UI visibility controls, palette configuration, action buttons, selection indicators, sliders, and popup behavior.

## Table of Contents
- [Input Area](#input-area)
- [Alpha Slider](#alpha-slider)
- [Action Buttons](#action-buttons)
- [Label Styles](#label-styles)
- [Palette Customization](#palette-customization)
- [Selection Indicator](#selection-indicator)
- [Slider Thumb Customization](#slider-thumb-customization)
- [Popup Customization](#popup-customization)

## Input Area

The input area displays RGB, HSV, and HEX color value fields, allowing users to manually enter color values.

### Show Input Area (Default)

```xaml
<inputs:SfColorPicker ShowInputArea="True"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    ShowInputArea = true
};
```

### Hide Input Area

```xaml
<inputs:SfColorPicker ShowInputArea="False"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    ShowInputArea = false
};
```

**Use Case:** Hide when you want visual-only color selection without manual entry.

## Alpha Slider

The alpha slider controls the transparency/opacity of the selected color (0 = fully transparent, 255 = fully opaque).

### Show Alpha Slider (Default)

```xaml
<inputs:SfColorPicker ShowAlphaSlider="True"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    ShowAlphaSlider = true
};
```

### Hide Alpha Slider

```xaml
<inputs:SfColorPicker ShowAlphaSlider="False"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    ShowAlphaSlider = false
};
```

**Use Case:** Hide when you only need opaque colors (e.g., background colors that don't support transparency).

## Action Buttons

Action buttons (Apply and Cancel) control when the selected color is applied. When hidden, color changes apply immediately.

### Show Action Buttons (Default)

```xaml
<inputs:SfColorPicker IsActionButtonsVisible="True"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    IsActionButtonsVisible = true
};
```

**Behavior:** User must click "Apply" to confirm selection. Click "Cancel" to discard changes and close popup.

### Hide Action Buttons (Instant Apply)

```xaml
<inputs:SfColorPicker IsActionButtonsVisible="False"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    IsActionButtonsVisible = false
};
```

**Behavior:** Color applies immediately upon selection, popup closes automatically.

### Customize Action Button Backgrounds

```xaml
<inputs:SfColorPicker ApplyButtonBackground="Navy"
                      CancelButtonBackground="LightGrey"
                      IsActionButtonsVisible="True"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    IsActionButtonsVisible = true,
    ApplyButtonBackground = Colors.Navy,
    CancelButtonBackground = Colors.LightGrey
};
```

### Customize Action Button Label Styles

```xaml
<inputs:SfColorPicker IsActionButtonsVisible="True">
    <inputs:SfColorPicker.ApplyButtonLabelStyle>
        <core:LabelStyle FontSize="16"
                          TextColor="White"
                          FontAttributes="Bold"/>
    </inputs:SfColorPicker.ApplyButtonLabelStyle>
    
    <inputs:SfColorPicker.CancelButtonLabelStyle>
        <core:LabelStyle FontSize="14"
                          TextColor="Gray"
                          FontAttributes="None"/>
    </inputs:SfColorPicker.CancelButtonLabelStyle>
</inputs:SfColorPicker>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    IsActionButtonsVisible = true
};

// Apply button label style
colorPicker.ApplyButtonLabelStyle = new LabelStyle
{
    FontSize = 16,
    TextColor = Colors.White,
    FontAttributes = FontAttributes.Bold
};

// Cancel button label style
colorPicker.CancelButtonLabelStyle = new LabelStyle
{
    FontSize = 14,
    TextColor = Colors.Gray,
    FontAttributes = FontAttributes.None
};
```

## Label Styles

### Recent Colors Label Style

Customize the "Recent Colors" text appearance:

```xaml
<inputs:SfColorPicker ColorMode="Palette">
    <inputs:SfColorPicker.RecentColorsLabelStyle>
        <core:LabelStyle FontSize="14"
                          TextColor="DarkGray"
                          FontAttributes="Italic"/>
    </inputs:SfColorPicker.RecentColorsLabelStyle>
</inputs:SfColorPicker>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    ColorMode = ColorPickerMode.Palette
};

colorPicker.RecentColorsLabelStyle = new LabelStyle
{
    FontSize = 14,
    TextColor = Colors.DarkGray,
    FontAttributes = FontAttributes.Italic
};
```

### Spectrum Input View Label Style

Customize HEX, RGB, HSV, and Alpha label text appearance:

```xaml
<inputs:SfColorPicker ColorMode="Spectrum">
    <inputs:SfColorPicker.SpectrumInputViewLabelStyle>
        <core:LabelStyle FontSize="14"
                          TextColor="Black"
                          FontFamily="Arial"/>
    </inputs:SfColorPicker.SpectrumInputViewLabelStyle>
</inputs:SfColorPicker>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    ColorMode = ColorPickerMode.Spectrum
};

colorPicker.SpectrumInputViewLabelStyle = new LabelStyle
{
    FontSize = 14,
    TextColor = Colors.Black,
    FontFamily = "Arial"
};
```

## Palette Customization

### Palette Row and Column Count

Control the grid dimensions (default: 10x10):

```xaml
<inputs:SfColorPicker ColorMode="Palette"
                      PaletteRowCount="5"
                      PaletteColumnCount="6"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    ColorMode = ColorPickerMode.Palette,
    PaletteRowCount = 5,
    PaletteColumnCount = 6
};
```

### Palette Spacing

Add spacing between palette cells:

```xaml
<inputs:SfColorPicker ColorMode="Palette"
                      PaletteRowSpacing="10"
                      PaletteColumnSpacing="10"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    ColorMode = ColorPickerMode.Palette,
    PaletteRowSpacing = 10,
    PaletteColumnSpacing = 10
};
```

### Palette Cell Corner Radius

Round the corners of palette cells:

```xaml
<inputs:SfColorPicker ColorMode="Palette"
                      PaletteCellCornerRadius="15,12,15,7"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    ColorMode = ColorPickerMode.Palette,
    PaletteCellCornerRadius = new CornerRadius(15, 12, 15, 7)
};
```

**Uniform corners:**

```csharp
colorPicker.PaletteCellCornerRadius = new CornerRadius(10);
```

### Palette Cell Shape

Choose from Circle, Square, or Default shape:

```xaml
<inputs:SfColorPicker ColorMode="Palette"
                      PaletteCellShape="Circle"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    ColorMode = ColorPickerMode.Palette,
    PaletteCellShape = PaletteCellShape.Circle
};
```

**Available Shapes:**
- `PaletteCellShape.Circle` - Circular cells
- `PaletteCellShape.Square` - Square cells
- `PaletteCellShape.Default` - Default rounded squares

### Palette Cell Size

Set the width and height of each palette cell:

```xaml
<inputs:SfColorPicker ColorMode="Palette"
                      PaletteCellSize="40"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    ColorMode = ColorPickerMode.Palette,
    PaletteCellSize = 40
};
```

### Custom Palette Colors

Define your own color collection:

```xaml
<inputs:SfColorPicker ColorMode="Palette">
    <inputs:SfColorPicker.PaletteColors>
        <x:List Type="{x:Type Color}">
            <Color>Red</Color>
            <Color>Green</Color>
            <Color>Blue</Color>
            <Color>Yellow</Color>
            <Color>Purple</Color>
            <Color>Orange</Color>
        </x:List>
    </inputs:SfColorPicker.PaletteColors>
</inputs:SfColorPicker>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    ColorMode = ColorPickerMode.Palette
};

// Set custom palette colors
colorPicker.PaletteColors = new List<Color>
{
    Colors.Red, Colors.Green, Colors.Blue,
    Colors.Yellow, Colors.Purple, Colors.Orange,
    Colors.Pink, Colors.Cyan, Colors.Magenta,
    Colors.Lime, Colors.Teal, Colors.Navy
};

// Dynamically add a color
colorPicker.PaletteColors.Add(Colors.Gold);
```

**Brand Color Example:**

```csharp
colorPicker.PaletteColors = new List<Color>
{
    Color.FromArgb("#FF5733"), // Brand Primary
    Color.FromArgb("#C70039"), // Brand Secondary
    Color.FromArgb("#900C3F"), // Brand Accent
    Color.FromArgb("#581845"), // Brand Dark
    Color.FromArgb("#FFC300"), // Highlight
    Color.FromArgb("#DAF7A6"), // Light
    Colors.White,
    Colors.Black,
    Colors.Gray
};
```

## Selection Indicator

Customize the visual indicator shown over the selected color.

### Selection Indicator Radius

```xaml
<inputs:SfColorPicker SelectionIndicatorRadius="6"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    SelectionIndicatorRadius = 6
};
```

### Selection Indicator Stroke

```xaml
<inputs:SfColorPicker SelectionIndicatorStroke="White"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    SelectionIndicatorStroke = Colors.White
};
```

### Selection Indicator Stroke Thickness

```xaml
<inputs:SfColorPicker SelectionIndicatorStrokeThickness="2"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    SelectionIndicatorStrokeThickness = 2
};
```

**Complete Example:**

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    ColorMode = ColorPickerMode.Palette,
    SelectionIndicatorRadius = 8,
    SelectionIndicatorStroke = Colors.Gold,
    SelectionIndicatorStrokeThickness = 3
};
```

## Slider Thumb Customization

Customize the hue and alpha slider thumb appearance (Spectrum mode).

### Slider Thumb Fill

```xaml
<inputs:SfColorPicker ColorMode="Spectrum"
                      SliderThumbFill="Blue"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    ColorMode = ColorPickerMode.Spectrum,
    SliderThumbFill = new SolidColorBrush(Colors.Blue)
};
```

### Slider Thumb Radius

```xaml
<inputs:SfColorPicker ColorMode="Spectrum"
                      SliderThumbRadius="12"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    ColorMode = ColorPickerMode.Spectrum,
    SliderThumbRadius = 12
};
```

### Slider Thumb Stroke

```xaml
<inputs:SfColorPicker ColorMode="Spectrum"
                      SliderThumbStroke="Black"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    ColorMode = ColorPickerMode.Spectrum,
    SliderThumbStroke = Colors.Black
};
```

### Slider Thumb Stroke Thickness

```xaml
<inputs:SfColorPicker ColorMode="Spectrum"
                      SliderThumbStrokeThickness="3"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    ColorMode = ColorPickerMode.Spectrum,
    SliderThumbStrokeThickness = 3
};
```

**Complete Slider Customization:**

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    ColorMode = ColorPickerMode.Spectrum,
    SliderThumbFill = new SolidColorBrush(Colors.White),
    SliderThumbRadius = 14,
    SliderThumbStroke = Colors.DarkGray,
    SliderThumbStrokeThickness = 2
};
```

## Popup Customization

### Programmatically Open Popup

```xaml
<inputs:SfColorPicker x:Name="colorPicker" IsOpen="True"/>
```

```csharp
// Open popup programmatically
colorPicker.IsOpen = true;

// Close popup programmatically
colorPicker.IsOpen = false;
```

**Button-Triggered Example:**

```xaml
<StackLayout>
    <Button Text="Open Color Picker" Clicked="OnOpenColorPickerClicked"/>
    <inputs:SfColorPicker x:Name="colorPicker"/>
</StackLayout>
```

```csharp
private void OnOpenColorPickerClicked(object sender, EventArgs e)
{
    colorPicker.IsOpen = true;
}
```

### Popup Background

```xaml
<inputs:SfColorPicker PopupBackground="Thistle"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    PopupBackground = Colors.Thistle
};
```

### Popup Relative Position

Control where the popup appears relative to the color picker button:

```xaml
<inputs:SfColorPicker PopupRelativePosition="AlignTop"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    PopupRelativePosition = PopupRelativePosition.AlignTop
};
```

**Available Positions:**
- `PopupRelativePosition.AlignBottom` (default)
- `PopupRelativePosition.AlignTop`
- `PopupRelativePosition.AlignBottomLeft`
- `PopupRelativePosition.AlignBottomRight`
- `PopupRelativePosition.AlignTopLeft`
- `PopupRelativePosition.AlignTopRight`
- `PopupRelativePosition.AlignToLeftOf`
- `PopupRelativePosition.AlignToRightOf`

## Complete Customization Examples

### Example 1: Fully Customized Palette

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    // Mode
    ColorMode = ColorPickerMode.Palette,
    IsColorModeSwitcherVisible = false,
    
    // Palette grid
    PaletteRowCount = 6,
    PaletteColumnCount = 8,
    PaletteRowSpacing = 8,
    PaletteColumnSpacing = 8,
    
    // Palette cells
    PaletteCellShape = PaletteCellShape.Circle,
    PaletteCellSize = 35,
    PaletteCellCornerRadius = new CornerRadius(5),
    
    // Selection
    SelectionIndicatorRadius = 6,
    SelectionIndicatorStroke = Colors.Gold,
    SelectionIndicatorStrokeThickness = 2,
    
    // Recent colors
    ShowRecentColors = true,
    ShowNoColor = true,
    
    // Action buttons
    IsActionButtonsVisible = false  // Instant apply
};
```

### Example 2: Customized Spectrum Mode

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    // Mode
    ColorMode = ColorPickerMode.Spectrum,
    IsColorModeSwitcherVisible = false,
    
    // UI elements
    ShowInputArea = true,
    ShowAlphaSlider = true,
    
    // Slider thumb
    SliderThumbFill = new SolidColorBrush(Colors.White),
    SliderThumbRadius = 12,
    SliderThumbStroke = Colors.Black,
    SliderThumbStrokeThickness = 2,
    
    // Action buttons
    IsActionButtonsVisible = true,
    ApplyButtonBackground = Colors.Green,
    CancelButtonBackground = Colors.Red,
    
    // Popup
    PopupBackground = Colors.WhiteSmoke,
    PopupRelativePosition = PopupRelativePosition.AlignTop
};
```

### Example 3: Brand Color Palette

```csharp
SfColorPicker brandColorPicker = new SfColorPicker()
{
    ColorMode = ColorPickerMode.Palette,
    IsColorModeSwitcherVisible = false,
    
    PaletteColors = new List<Color>
    {
        // Primary brand colors
        Color.FromArgb("#0066CC"),
        Color.FromArgb("#0052A3"),
        Color.FromArgb("#003D7A"),
        
        // Secondary colors
        Color.FromArgb("#FF6B35"),
        Color.FromArgb("#F7931E"),
        
        // Neutral colors
        Color.FromArgb("#333333"),
        Color.FromArgb("#666666"),
        Color.FromArgb("#999999"),
        Colors.White
    },
    
    PaletteColumnCount = 3,
    PaletteRowCount = 3,
    PaletteCellSize = 50,
    PaletteCellShape = PaletteCellShape.Square,
    
    ShowRecentColors = false,
    IsActionButtonsVisible = false
};
```

## Best Practices

1. **Action Buttons:** Hide for instant color apply in real-time preview scenarios
2. **Alpha Slider:** Hide when transparency isn't needed (e.g., solid backgrounds)
3. **Input Area:** Hide for simpler UI when manual entry isn't required
4. **Palette Spacing:** Use spacing (8-12) for better touch targets on mobile
5. **Cell Size:** Larger cells (40-50) improve mobile usability
6. **Custom Colors:** Limit to 20-30 colors for optimal UX
7. **Popup Position:** Adjust based on picker placement in your layout
