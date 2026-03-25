# Events and Interaction

The SfColorPicker provides three built-in events and a command for handling color selection and changes. This guide covers event types, their behavior, practical use cases, and best practices.

## Table of Contents
- [Event Overview](#event-overview)
- [ColorChanging Event](#colorchanging-event)
- [ColorChanged Event](#colorchanged-event)
- [ColorSelected Event](#colorselected-event)
- [ColorChangedCommand](#colorchangedcommand)
- [Practical Examples](#practical-examples)

## Event Overview

### Available Events

| Event | When Fired | Can Cancel | Use Case |
|-------|------------|------------|----------|
| **ColorChanging** | While color is being changed | ✅ Yes | Validation, prevent unwanted colors |
| **ColorChanged** | When color change is confirmed | ❌ No | Apply color, update UI |
| **ColorSelected** | When user clicks selected color view | ❌ No | Show color info, copy to clipboard |

### Event Sequence

1. User interacts with color picker
2. `ColorChanging` fires (can cancel)
3. If not canceled → color updates
4. `ColorChanged` fires (after Apply button or instant)
5. User clicks selected color → `ColorSelected` fires

## ColorChanging Event

Fired **while** the color is being changed, before it's applied. Allows you to validate or cancel the color change.

### Event Arguments

```csharp
public class ColorChangingEventArgs : CancelEventArgs
{
    public Color CurrentColor { get; }  // Color before change
    public Color NewColor { get; }      // Proposed new color
}
```

### Basic Usage

```xaml
<inputs:SfColorPicker x:Name="colorPicker" 
                      ColorChanging="OnColorChanging"/>
```

```csharp
private void OnColorChanging(object sender, ColorChangingEventArgs e)
{
    // Access colors
    Color oldColor = e.CurrentColor;
    Color newColor = e.NewColor;
    
    // Log change
    Debug.WriteLine($"Changing from {oldColor.ToHex()} to {newColor.ToHex()}");
}
```

### Cancel Color Change

Prevent specific colors from being selected:

```csharp
private void OnColorChanging(object sender, ColorChangingEventArgs e)
{
    // Prevent pure black
    if (e.NewColor == Colors.Black)
    {
        e.Cancel = true;
        DisplayAlert("Invalid Color", "Black is not allowed!", "OK");
    }
}
```

### Validation Example

```csharp
private void OnColorChanging(object sender, ColorChangingEventArgs e)
{
    // Ensure minimum brightness
    double brightness = (e.NewColor.Red + e.NewColor.Green + e.NewColor.Blue) / 3.0;
    
    if (brightness < 0.3) // Too dark
    {
        e.Cancel = true;
        DisplayAlert("Color Too Dark", 
                    "Please select a brighter color for better visibility.", 
                    "OK");
    }
}
```

### Restrict to Grayscale

```csharp
private void OnColorChanging(object sender, ColorChangingEventArgs e)
{
    Color color = e.NewColor;
    
    // Check if RGB values are equal (grayscale)
    bool isGrayscale = Math.Abs(color.Red - color.Green) < 0.01 && 
                       Math.Abs(color.Green - color.Blue) < 0.01;
    
    if (!isGrayscale)
    {
        e.Cancel = true;
        DisplayAlert("Grayscale Only", 
                    "Only grayscale colors are allowed in this context.", 
                    "OK");
    }
}
```

## ColorChanged Event

Fired when the color selection is **confirmed**. Behavior depends on action buttons:

- **Action buttons visible:** Fires when user clicks "Apply"
- **Action buttons hidden:** Fires immediately on selection

### Event Arguments

```csharp
public class ColorChangedEventArgs : EventArgs
{
    public Color OldColor { get; }  // Previously selected color
    public Color NewColor { get; }  // Newly selected color
}
```

### Basic Usage

```xaml
<inputs:SfColorPicker ColorChanged="OnColorChanged"/>
```

```csharp
private void OnColorChanged(object sender, ColorChangedEventArgs e)
{
    Color oldColor = e.OldColor;
    Color newColor = e.NewColor;
    
    Debug.WriteLine($"Color changed from {oldColor.ToHex()} to {newColor.ToHex()}");
}
```

### Update UI Element

```xaml
<Grid RowDefinitions="Auto,*">
    <inputs:SfColorPicker Grid.Row="0"
                          x:Name="colorPicker"
                          ColorChanged="OnColorChanged"/>
    
    <BoxView Grid.Row="1"
             x:Name="previewBox"
             Margin="20"
             CornerRadius="10"/>
</Grid>
```

```csharp
private void OnColorChanged(object sender, ColorChangedEventArgs e)
{
    previewBox.BackgroundColor = e.NewColor;
}
```

### Update Multiple Elements

```xaml
<Grid ColumnDefinitions="*,Auto" RowDefinitions="Auto,*">
    
    <inputs:SfColorPicker Grid.Row="0" Grid.ColumnSpan="2"
                          ColorChanged="OnColorChanged"/>
    
    <Label Grid.Row="1" Grid.Column="0"
           x:Name="label"
           Text="Selected Color"
           HorizontalTextAlignment="Center"
           VerticalTextAlignment="Center"
           TextColor="Black"
           BackgroundColor="LightGray"
           Margin="10"/>
    
    <BoxView Grid.Row="1" Grid.Column="1"
             x:Name="previewBox"
             WidthRequest="100"
             HeightRequest="100"
             Margin="10"/>
    
</Grid>
```

```csharp
private void OnColorChanged(object sender, ColorChangedEventArgs e)
{
    Color newColor = e.NewColor;
    
    // Update label
    label.BackgroundColor = newColor;
    label.Text = newColor.ToHex();
    
    // Update preview box
    previewBox.BackgroundColor = newColor;
}
```

### Save to Preferences

```csharp
private void OnColorChanged(object sender, ColorChangedEventArgs e)
{
    // Save selected color to preferences
    Preferences.Set("ThemeColor", e.NewColor.ToHex());
    
    // Apply to app theme
    Application.Current.Resources["PrimaryColor"] = e.NewColor;
}
```

## ColorSelected Event

Fired when the user **clicks or taps** the selected color view (the display button showing the current color).

### Event Arguments

```csharp
public class ColorSelectedEventArgs : EventArgs
{
    public Color SelectedColor { get; }  // Currently selected color
}
```

### Basic Usage

```xaml
<inputs:SfColorPicker ColorSelected="OnColorSelected"/>
```

```csharp
private void OnColorSelected(object sender, ColorSelectedEventArgs e)
{
    Color color = e.SelectedColor;
    Debug.WriteLine($"User clicked selected color: {color.ToHex()}");
}
```

### Copy Color to Clipboard

```csharp
private async void OnColorSelected(object sender, ColorSelectedEventArgs e)
{
    string hexColor = e.SelectedColor.ToHex();
    await Clipboard.SetTextAsync(hexColor);
    await DisplayAlert("Copied", $"Color {hexColor} copied to clipboard!", "OK");
}
```

### Show Color Info Dialog

```csharp
private async void OnColorSelected(object sender, ColorSelectedEventArgs e)
{
    Color color = e.SelectedColor;
    
    string message = $"HEX: {color.ToHex()}\n" +
                     $"RGB: ({(int)(color.Red * 255)}, {(int)(color.Green * 255)}, {(int)(color.Blue * 255)})\n" +
                     $"Alpha: {(int)(color.Alpha * 255)}";
    
    await DisplayAlert("Color Information", message, "OK");
}
```

### Toggle Expanded View

```csharp
private void OnColorSelected(object sender, ColorSelectedEventArgs e)
{
    // Toggle between inline and popup mode
    colorPicker.IsInline = !colorPicker.IsInline;
}
```

## ColorChangedCommand

Execute a command when the `SelectedColor` changes. Ideal for MVVM pattern.

### Basic Command Binding

```xaml
<inputs:SfColorPicker ColorChangedCommand="{Binding UpdateColorCommand}"/>
```

```csharp
// ViewModel
public class ColorViewModel : INotifyPropertyChanged
{
    public ICommand UpdateColorCommand { get; }
    
    public ColorViewModel()
    {
        UpdateColorCommand = new Command<Color>(OnColorChanged);
    }
    
    private void OnColorChanged(Color color)
    {
        Debug.WriteLine($"Color changed to: {color.ToHex()}");
        // Update properties, notify views, etc.
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
}
```

### MVVM Example with Property Binding

```xaml
<ContentPage xmlns:vm="clr-namespace:MyApp.ViewModels"
             x:DataType="vm:ThemeViewModel">
    
    <ContentPage.BindingContext>
        <vm:ThemeViewModel/>
    </ContentPage.BindingContext>
    
    <VerticalStackLayout>
        <inputs:SfColorPicker SelectedColor="{Binding PrimaryColor, Mode=TwoWay}"
                              ColorChangedCommand="{Binding ApplyThemeCommand}"/>
        
        <Label Text="{Binding PrimaryColor, StringFormat='Selected: {0}'}"
               Margin="0,20,0,0"/>
    </VerticalStackLayout>
    
</ContentPage>
```

```csharp
public class ThemeViewModel : INotifyPropertyChanged
{
    private Color primaryColor = Colors.Blue;
    
    public Color PrimaryColor
    {
        get => primaryColor;
        set
        {
            if (primaryColor != value)
            {
                primaryColor = value;
                OnPropertyChanged();
            }
        }
    }
    
    public ICommand ApplyThemeCommand { get; }
    
    public ThemeViewModel()
    {
        ApplyThemeCommand = new Command<Color>(ApplyTheme);
    }
    
    private void ApplyTheme(Color color)
    {
        // Apply theme throughout app
        Application.Current.Resources["PrimaryColor"] = color;
        
        // Save preference
        Preferences.Set("AppThemeColor", color.ToHex());
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected virtual void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

## Practical Examples

### Example 1: Real-Time Preview with Validation

```xaml
<Grid RowDefinitions="Auto,*,Auto">
    
    <inputs:SfColorPicker Grid.Row="0"
                          ColorChanging="OnColorChanging"
                          ColorChanged="OnColorChanged"
                          IsActionButtonsVisible="False"/>
    
    <BoxView Grid.Row="1"
             x:Name="previewBox"
             Margin="20"
             CornerRadius="10"/>
    
    <Label Grid.Row="2"
           x:Name="messageLabel"
           HorizontalOptions="Center"
           FontSize="14"
           TextColor="Red"/>
    
</Grid>
```

```csharp
private void OnColorChanging(object sender, ColorChangingEventArgs e)
{
    // Validate brightness
    double brightness = (e.NewColor.Red + e.NewColor.Green + e.NewColor.Blue) / 3.0;
    
    if (brightness < 0.2)
    {
        e.Cancel = true;
        messageLabel.Text = "⚠️ Color too dark - please select a brighter color";
        messageLabel.IsVisible = true;
    }
    else
    {
        messageLabel.IsVisible = false;
    }
}

private void OnColorChanged(object sender, ColorChangedEventArgs e)
{
    previewBox.BackgroundColor = e.NewColor;
    messageLabel.IsVisible = false;
}
```

### Example 2: Color History Tracker

```csharp
public partial class ColorHistoryPage : ContentPage
{
    private List<Color> colorHistory = new List<Color>();
    
    public ColorHistoryPage()
    {
        InitializeComponent();
    }
    
    private void OnColorChanged(object sender, ColorChangedEventArgs e)
    {
        // Add to history
        colorHistory.Add(e.NewColor);
        
        // Update history display
        UpdateColorHistory();
        
        // Limit history size
        if (colorHistory.Count > 10)
        {
            colorHistory.RemoveAt(0);
        }
    }
    
    private void UpdateColorHistory()
    {
        historyStackLayout.Children.Clear();
        
        foreach (var color in colorHistory)
        {
            var boxView = new BoxView
            {
                Color = color,
                WidthRequest = 50,
                HeightRequest = 50,
                CornerRadius = 5,
                Margin = new Thickness(5)
            };
            
            historyStackLayout.Children.Add(boxView);
        }
    }
}
```

### Example 3: Multi-Color Theme Builder

```csharp
public partial class ThemeBuilderPage : ContentPage
{
    private Dictionary<string, Color> themeColors = new Dictionary<string, Color>();
    
    private void OnPrimaryColorChanged(object sender, ColorChangedEventArgs e)
    {
        themeColors["Primary"] = e.NewColor;
        primaryPreview.BackgroundColor = e.NewColor;
    }
    
    private void OnSecondaryColorChanged(object sender, ColorChangedEventArgs e)
    {
        themeColors["Secondary"] = e.NewColor;
        secondaryPreview.BackgroundColor = e.NewColor;
    }
    
    private void OnAccentColorChanged(object sender, ColorChangedEventArgs e)
    {
        themeColors["Accent"] = e.NewColor;
        accentPreview.BackgroundColor = e.NewColor;
    }
    
    private async void OnSaveThemeClicked(object sender, EventArgs e)
    {
        // Save theme
        foreach (var kvp in themeColors)
        {
            Preferences.Set($"Theme_{kvp.Key}", kvp.Value.ToHex());
        }
        
        await DisplayAlert("Success", "Theme saved successfully!", "OK");
    }
}
```

## Best Practices

1. **Event Choice:**
   - Use `ColorChanging` for validation/cancellation
   - Use `ColorChanged` for applying colors and updating UI
   - Use `ColorSelected` for auxiliary actions (copy, info, etc.)

2. **Performance:**
   - Avoid heavy operations in `ColorChanging` (fires frequently)
   - Batch updates in `ColorChanged` if multiple elements need updating

3. **User Feedback:**
   - Provide immediate visual feedback in `ColorChanging`
   - Show validation messages clearly
   - Use `ColorSelected` for helpful features like copy-to-clipboard

4. **MVVM:**
   - Prefer `ColorChangedCommand` for MVVM scenarios
   - Use two-way binding with `SelectedColor` property

5. **Action Buttons:**
   - With action buttons: Changes confirmed on "Apply"
   - Without action buttons: Changes apply immediately
