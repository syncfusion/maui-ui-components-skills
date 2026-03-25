# Color Modes and Selection

The SfColorPicker supports two distinct color selection modes: **Palette** (predefined color grid) and **Spectrum** (gradient-based picker). This guide covers how to configure these modes, set default colors, and manage recent color history.

## Color Modes

### Palette Mode

Palette mode displays a grid of predefined colors for quick selection. Ideal for:
- Brand color selection
- Predefined color schemes
- Quick access to common colors
- Limited color choices

**Set Palette Mode:**

```xaml
<inputs:SfColorPicker ColorMode="Palette"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    ColorMode = ColorPickerMode.Palette
};
```

**Key Features:**
- Grid-based color selection
- Customizable rows and columns
- Recent colors panel
- Custom color additions
- "No Color" option

### Spectrum Mode (Default)

Spectrum mode provides gradient-based color selection with fine-grained control. Ideal for:
- Precise color selection
- Full RGB/HSV control
- Custom color creation
- Detailed color adjustments

**Set Spectrum Mode:**

```xaml
<inputs:SfColorPicker ColorMode="Spectrum"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    ColorMode = ColorPickerMode.Spectrum
};
```

**Key Features:**
- Gradient spectrum selector
- Hue slider
- Alpha/opacity slider
- RGB, HSV, HEX input fields
- Precise color value control

## Color Mode Switcher

By default, users can toggle between Palette and Spectrum modes using a switcher button within the color picker.

### Show Mode Switcher (Default)

```xaml
<inputs:SfColorPicker IsColorModeSwitcherVisible="True"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    IsColorModeSwitcherVisible = true
};
```

### Hide Mode Switcher

Lock the color picker to a single mode:

```xaml
<inputs:SfColorPicker ColorMode="Palette" 
                      IsColorModeSwitcherVisible="False"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    ColorMode = ColorPickerMode.Palette,
    IsColorModeSwitcherVisible = false  // Users cannot switch modes
};
```

**Use Case:** When you want to restrict users to either palette or spectrum selection only.

## Selected Color

### Setting Default Color

Display a default color when the color picker first loads:

```xaml
<inputs:SfColorPicker SelectedColor="DodgerBlue"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    SelectedColor = Colors.DodgerBlue
};
```

### Setting Color from HEX

```csharp
// Using Color.FromArgb
colorPicker.SelectedColor = Color.FromArgb("#FF5733");

// Using Color.FromRgb
colorPicker.SelectedColor = Color.FromRgb(255, 87, 51);

// Using Color.FromRgba
colorPicker.SelectedColor = Color.FromRgba(255, 87, 51, 200);
```

### Getting Selected Color

```csharp
// Get the current selected color
Color selectedColor = colorPicker.SelectedColor;

// Convert to HEX string
string hexValue = selectedColor.ToHex();  // Returns "#FF5733"

// Access RGB components
float red = selectedColor.Red;      // 0.0 to 1.0
float green = selectedColor.Green;  // 0.0 to 1.0
float blue = selectedColor.Blue;    // 0.0 to 1.0
float alpha = selectedColor.Alpha;  // 0.0 to 1.0 (opacity)
```

## Recent Colors

The Recent Colors panel automatically tracks colors selected by the user, providing quick access to previously chosen colors. Available in **Palette mode only**.

### Show Recent Colors (Default)

```xaml
<inputs:SfColorPicker ColorMode="Palette"
                      ShowRecentColors="True"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    ColorMode = ColorPickerMode.Palette,
    ShowRecentColors = true
};
```

### Hide Recent Colors

```xaml
<inputs:SfColorPicker ColorMode="Palette"
                      ShowRecentColors="False"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    ColorMode = ColorPickerMode.Palette,
    ShowRecentColors = false
};
```

### Clear Recent Colors

Programmatically clear the recent colors history:

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    ColorMode = ColorPickerMode.Palette,
    ShowRecentColors = true
};

// Clear all recent colors
colorPicker.ClearRecentColors();
```

**Use Cases:**
- Reset color history for new user session
- Clear history after saving a color scheme
- Implement a "Clear History" button

**Example with Button:**

```xaml
<VerticalStackLayout>
    <inputs:SfColorPicker x:Name="colorPicker" 
                          ColorMode="Palette"
                          ShowRecentColors="True"/>
    
    <Button Text="Clear Recent Colors"
            Clicked="OnClearRecentColorsClicked"
            Margin="0,10,0,0"/>
</VerticalStackLayout>
```

```csharp
private void OnClearRecentColorsClicked(object sender, EventArgs e)
{
    colorPicker.ClearRecentColors();
    DisplayAlert("Success", "Recent colors cleared!", "OK");
}
```

## No Color Option

The "No Color" option allows users to explicitly select no color (transparent/null color). Useful for:
- Removing previously applied colors
- Optional color fields
- Transparent backgrounds

### Show No Color Option

```xaml
<inputs:SfColorPicker ShowNoColor="True"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    ShowNoColor = true
};
```

### Hide No Color Option (Default)

```xaml
<inputs:SfColorPicker ShowNoColor="False"/>
```

```csharp
SfColorPicker colorPicker = new SfColorPicker()
{
    ShowNoColor = false
};
```

### Detecting No Color Selection

```csharp
colorPicker.ColorChanged += (sender, e) =>
{
    if (e.NewColor == Colors.Transparent)
    {
        // User selected "No Color"
        targetElement.BackgroundColor = Colors.Transparent;
    }
    else
    {
        // User selected an actual color
        targetElement.BackgroundColor = e.NewColor;
    }
};
```

## Complete Examples

### Example 1: Palette Mode with Recent Colors

```xaml
<inputs:SfColorPicker ColorMode="Palette"
                      IsColorModeSwitcherVisible="False"
                      ShowRecentColors="True"
                      ShowNoColor="True"
                      SelectedColor="CornflowerBlue"/>
```

### Example 2: Spectrum Mode Only

```xaml
<inputs:SfColorPicker ColorMode="Spectrum"
                      IsColorModeSwitcherVisible="False"
                      SelectedColor="#FF6B35"/>
```

### Example 3: Mode Switcher with Default Palette

```xaml
<inputs:SfColorPicker ColorMode="Palette"
                      IsColorModeSwitcherVisible="True"
                      ShowRecentColors="True"
                      SelectedColor="MediumSeaGreen"/>
```

### Example 4: MVVM Pattern with SelectedColor Binding

```xaml
<inputs:SfColorPicker SelectedColor="{Binding BackgroundColor, Mode=TwoWay}"
                      ColorMode="Palette"/>
```

```csharp
// ViewModel
public class ColorViewModel : INotifyPropertyChanged
{
    private Color backgroundColor = Colors.White;
    
    public Color BackgroundColor
    {
        get => backgroundColor;
        set
        {
            if (backgroundColor != value)
            {
                backgroundColor = value;
                OnPropertyChanged();
            }
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected virtual void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

## Best Practices

1. **Choose the Right Mode:**
   - Use **Palette** for predefined brand colors or limited choices
   - Use **Spectrum** for creative tools or detailed color control

2. **Mode Switcher:**
   - Keep visible for flexibility
   - Hide when you want to enforce a specific selection method

3. **Recent Colors:**
   - Enable for improved UX and quick color reuse
   - Provide a clear method if persistence is needed across sessions

4. **No Color Option:**
   - Enable when colors are optional
   - Clearly handle transparent color in your UI logic

5. **Default Colors:**
   - Set meaningful defaults based on your app's theme
   - Use brand primary colors as defaults when appropriate
