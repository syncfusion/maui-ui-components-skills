# Customization in .NET MAUI Button (SfButton)

The Syncfusion .NET MAUI Button control provides extensive customization options for appearance, layout, and behavior. This guide covers all available customization properties and techniques.

## Table of Contents
- [Text Customization](#text-customization)
- [Background Customization](#background-customization)
- [Image Customization](#image-customization)
- [Padding and Sizing](#padding-and-sizing)
- [Gradient Background](#gradient-background)
- [Ripple Effects](#ripple-effects)
- [Custom Content](#custom-content)
- [Command Pattern](#command-pattern)
- [Complete Examples](#complete-examples)

## Text Customization

### TextColor

Controls the color of the button text:

**XAML:**
```xml
<buttons:SfButton Text="Colored Text"
                  TextColor="White"
                  Background="#6200EE" />
```

**C#:**
```csharp
var button = new SfButton
{
    Text = "Colored Text",
    TextColor = Colors.White,
    Background = Color.FromArgb("#6200EE")
};
```

### FontSize

Adjusts the text size (in device-independent units):

**XAML:**
```xml
<buttons:SfButton Text="Large Text"
                  FontSize="20"
                  WidthRequest="150" />
```

**C#:**
```csharp
var button = new SfButton
{
    Text = "Large Text",
    FontSize = 20,
    WidthRequest = 150
};
```

**Common sizes:**
- Small: 12-14
- Normal: 14-16
- Large: 18-20
- Extra Large: 22-24

### FontAttributes

Sets text style to Bold, Italic, or None:

**XAML:**
```xml
<buttons:SfButton Text="Bold Button"
                  FontAttributes="Bold"
                  FontSize="16" />

<buttons:SfButton Text="Italic Button"
                  FontAttributes="Italic" />

<buttons:SfButton Text="Bold and Italic"
                  FontAttributes="Bold,Italic" />
```

**C#:**
```csharp
var boldButton = new SfButton
{
    Text = "Bold Button",
    FontAttributes = FontAttributes.Bold
};

var italicButton = new SfButton
{
    Text = "Italic Button",
    FontAttributes = FontAttributes.Italic
};

var combinedButton = new SfButton
{
    Text = "Bold and Italic",
    FontAttributes = FontAttributes.Bold | FontAttributes.Italic
};
```

### FontFamily

Use custom fonts for button text:

**XAML:**
```xml
<buttons:SfButton Text="Custom Font"
                  FontFamily="Roboto" />
```

**C#:**
```csharp
var button = new SfButton
{
    Text = "Custom Font",
    FontFamily = "Roboto"
};
```

**To use custom fonts:**
1. Add font files to `Resources/Fonts/` folder
2. Register in `MauiProgram.cs`:
   ```csharp
   .ConfigureFonts(fonts =>
   {
       fonts.AddFont("CustomFont.ttf", "CustomFontAlias");
   });
   ```
3. Use the alias in FontFamily property

### Text Alignment

Control horizontal and vertical text positioning:

**XAML:**
```xml
<buttons:SfButton Text="Centered"
                  HorizontalTextAlignment="Center"
                  VerticalTextAlignment="Center"
                  WidthRequest="200"
                  HeightRequest="60" />

<buttons:SfButton Text="Left-Top"
                  HorizontalTextAlignment="Start"
                  VerticalTextAlignment="Start"
                  WidthRequest="200"
                  HeightRequest="60" />
```

**C#:**
```csharp
var centeredButton = new SfButton
{
    Text = "Centered",
    HorizontalTextAlignment = TextAlignment.Center,
    VerticalTextAlignment = TextAlignment.Center
};
```

**Alignment options:**
- `Start` / `Left` — Align to left edge
- `Center` — Center alignment
- `End` / `Right` — Align to right edge

### TextTransform

Transform text casing:

**XAML:**
```xml
<buttons:SfButton Text="Submit"
                  TextTransform="Uppercase" />  <!-- Output: SUBMIT -->

<buttons:SfButton Text="CANCEL"
                  TextTransform="Lowercase" />  <!-- Output: cancel -->

<buttons:SfButton Text="help"
                  TextTransform="Default" />    <!-- Output: Help (title case) -->
```

**C#:**
```csharp
var button = new SfButton
{
    Text = "Submit",
    TextTransform = TextTransform.Uppercase
};
```

**Options:** `None`, `Default`, `Uppercase`, `Lowercase`

### LineBreakMode

Control how text wraps or truncates:

**XAML:**
```xml
<buttons:SfButton Text="This is a very long button text that needs handling"
                  LineBreakMode="MiddleTruncation"
                  WidthRequest="150" />
```

**C#:**
```csharp
var button = new SfButton
{
    Text = "This is a very long button text",
    LineBreakMode = LineBreakMode.TailTruncation,
    WidthRequest = 150
};
```

**Options:**
- `NoWrap` — Single line, no wrapping (default)
- `WordWrap` — Wrap at word boundaries
- `CharacterWrap` — Wrap anywhere
- `HeadTruncation` — "...xt here"
- `MiddleTruncation` — "Some te...here"
- `TailTruncation` — "Some text he..."

## Background Customization

### Background Color

Use the `Background` property (not `BackgroundColor`) for solid colors:

**XAML:**
```xml
<buttons:SfButton Text="Blue Button"
                  Background="#2196F3"
                  TextColor="White" />
```

**C#:**
```csharp
var button = new SfButton
{
    Text = "Blue Button",
    Background = Color.FromArgb("#2196F3"),
    TextColor = Colors.White
};
```

**Common color schemes:**
- Primary: `#6200EE` (purple)
- Secondary: `#03DAC6` (teal)
- Success: `#4CAF50` (green)
- Warning: `#FF9800` (orange)
- Danger: `#F44336` (red)
- Info: `#2196F3` (blue)

### Stroke (Border Color)

Set border color with `Stroke` property:

**XAML:**
```xml
<buttons:SfButton Text="Outline Button"
                  Background="Transparent"
                  TextColor="#6200EE"
                  Stroke="#6200EE"
                  StrokeThickness="2"
                  CornerRadius="8" />
```

**C#:**
```csharp
var outlineButton = new SfButton
{
    Text = "Outline Button",
    Background = Colors.Transparent,
    TextColor = Color.FromArgb("#6200EE"),
    Stroke = Color.FromArgb("#6200EE"),
    StrokeThickness = 2,
    CornerRadius = 8
};
```

> **Note:** `StrokeThickness` must be > 0 for border to appear.

### StrokeThickness

Control border width:

**XAML:**
```xml
<!-- Thin border -->
<buttons:SfButton Text="Thin Border"
                  Stroke="Black"
                  StrokeThickness="1" />

<!-- Thick border -->
<buttons:SfButton Text="Thick Border"
                  Stroke="Black"
                  StrokeThickness="4" />
```

**Typical values:**
- 1-2: Subtle borders
- 3-4: Prominent borders
- 5+: Extra bold (rarely used)

### CornerRadius

Create rounded corners:

**XAML:**
```xml
<!-- Slightly rounded -->
<buttons:SfButton Text="Rounded"
                  CornerRadius="8" />

<!-- Pill-shaped -->
<buttons:SfButton Text="Pill Button"
                  CornerRadius="22"
                  WidthRequest="150"
                  HeightRequest="44" />

<!-- Circular (for icon buttons) -->
<buttons:SfButton ShowIcon="True"
                  ImageSource="icon.png"
                  CornerRadius="28"
                  WidthRequest="56"
                  HeightRequest="56" />
```

**C#:**
```csharp
var roundedButton = new SfButton
{
    Text = "Rounded",
    CornerRadius = 8
};
```

**Guidelines:**
- 0: Square corners
- 4-8: Subtle rounding (material design)
- 12-20: Prominent rounding
- Height/2: Perfect circle (when width = height)

## Image Customization

### ShowIcon

Enable icon display:

**XAML:**
```xml
<buttons:SfButton Text="Save"
                  ShowIcon="True"
                  ImageSource="save_icon.png" />
```

> **Required:** Set `ShowIcon="True"` to display icons.

### ImageSource

Specify the icon image:

**XAML:**
```xml
<!-- File-based image -->
<buttons:SfButton Text="Download"
                  ShowIcon="True"
                  ImageSource="download_icon.png" />

<!-- FontImageSource -->
<buttons:SfButton Text="Heart"
                  ShowIcon="True">
    <buttons:SfButton.ImageSource>
        <FontImageSource Glyph="&#xE87D;"
                         FontFamily="MaterialIcons"
                         Size="24"
                         Color="Red" />
    </buttons:SfButton.ImageSource>
</buttons:SfButton>
```

**C#:**
```csharp
// File-based
var fileButton = new SfButton
{
    Text = "Download",
    ShowIcon = true,
    ImageSource = "download_icon.png"
};

// Font icon
var fontButton = new SfButton
{
    Text = "Heart",
    ShowIcon = true,
    ImageSource = new FontImageSource
    {
        Glyph = "\ue87d",
        FontFamily = "MaterialIcons",
        Size = 24,
        Color = Colors.Red
    }
};
```

### ImageSize

Control icon dimensions:

**XAML:**
```xml
<!-- Small icon -->
<buttons:SfButton Text="Small Icon"
                  ShowIcon="True"
                  ImageSource="icon.png"
                  ImageSize="16" />

<!-- Large icon -->
<buttons:SfButton Text="Large Icon"
                  ShowIcon="True"
                  ImageSource="icon.png"
                  ImageSize="48" />
```

**C#:**
```csharp
var button = new SfButton
{
    ShowIcon = true,
    ImageSource = "icon.png",
    ImageSize = 24
};
```

**Typical sizes:**
- 16-20: Small icons
- 24-32: Standard icons
- 40-48: Large icons

### ImageAlignment

Position icon relative to text:

**XAML:**
```xml
<!-- Icon on left (default) -->
<buttons:SfButton Text="Start"
                  ShowIcon="True"
                  ImageSource="icon.png"
                  ImageAlignment="Start" />

<!-- Icon on right -->
<buttons:SfButton Text="End"
                  ShowIcon="True"
                  ImageSource="icon.png"
                  ImageAlignment="End" />

<!-- Icon above text -->
<buttons:SfButton Text="Top"
                  ShowIcon="True"
                  ImageSource="icon.png"
                  ImageAlignment="Top" />

<!-- Icon below text -->
<buttons:SfButton Text="Bottom"
                  ShowIcon="True"
                  ImageSource="icon.png"
                  ImageAlignment="Bottom" />
```

**C#:**
```csharp
var button = new SfButton
{
    Text = "Shopping",
    ShowIcon = true,
    ImageSource = "cart_icon.png",
    ImageAlignment = Alignment.End
};
```

**Alignment options:**
- `Start` — Icon on left (respects RTL)
- `End` — Icon on right (respects RTL)
- `Top` — Icon above text
- `Bottom` — Icon below text
- `Left` — Always left (ignores RTL)
- `Right` — Always right (ignores RTL)
- `Default` — Centered with text

## Padding and Sizing

### Padding

Control internal spacing:

**XAML:**
```xml
<!-- Uniform padding -->
<buttons:SfButton Text="Padded"
                  Padding="20" />

<!-- Horizontal and vertical -->
<buttons:SfButton Text="Padded"
                  Padding="40,12" />

<!-- Individual sides (left, top, right, bottom) -->
<buttons:SfButton Text="Custom Padding"
                  Padding="16,8,16,8" />
```

**C#:**
```csharp
var button = new SfButton
{
    Text = "Padded",
    Padding = new Thickness(20, 12) // horizontal, vertical
};
```

### WidthRequest and HeightRequest

Set explicit dimensions:

**XAML:**
```xml
<buttons:SfButton Text="Fixed Size"
                  WidthRequest="200"
                  HeightRequest="44" />
```

**C#:**
```csharp
var button = new SfButton
{
    Text = "Fixed Size",
    WidthRequest = 200,
    HeightRequest = 44
};
```

**Standard button heights:**
- 32-36: Small buttons
- 40-44: Normal buttons (recommended)
- 48-56: Large buttons
- 56-64: Extra large / FAB buttons

## Gradient Background

Apply linear or radial gradients:

### Linear Gradient

**XAML:**
```xml
<buttons:SfButton Text="Linear Gradient"
                  TextColor="White"
                  CornerRadius="8">
    <buttons:SfButton.Background>
        <LinearGradientBrush EndPoint="1,0">
            <GradientStop Color="#667eea" Offset="0.0" />
            <GradientStop Color="#764ba2" Offset="1.0" />
        </LinearGradientBrush>
    </buttons:SfButton.Background>
</buttons:SfButton>
```

**C#:**
```csharp
var button = new SfButton
{
    Text = "Linear Gradient",
    TextColor = Colors.White,
    CornerRadius = 8
};

var gradient = new LinearGradientBrush
{
    EndPoint = new Point(1, 0),
    GradientStops = new GradientStopCollection
    {
        new GradientStop { Color = Color.FromArgb("#667eea"), Offset = 0.0f },
        new GradientStop { Color = Color.FromArgb("#764ba2"), Offset = 1.0f }
    }
};
button.Background = gradient;
```

### Radial Gradient

**XAML:**
```xml
<buttons:SfButton Text="Radial Gradient"
                  TextColor="White"
                  CornerRadius="8">
    <buttons:SfButton.Background>
        <RadialGradientBrush Center="0.5,0.5" Radius="1.5">
            <GradientStop Color="#ff6b6b" Offset="0" />
            <GradientStop Color="#ee5a6f" Offset="1" />
        </RadialGradientBrush>
    </buttons:SfButton.Background>
</buttons:SfButton>
```

**Popular gradient combinations:**
- Blue to Purple: `#667eea` → `#764ba2`
- Pink to Orange: `#f093fb` → `#f5576c`
- Green to Teal: `#4facfe` → `#00f2fe`
- Sunset: `#fa709a` → `#fee140`

## Ripple Effects

Enable touch feedback with ripple animation:

**XAML:**
```xml
<buttons:SfButton Text="Ripple Effect"
                  EnableRippleEffect="True"
                  Background="#6200EE"
                  TextColor="White" />
```

**C#:**
```csharp
var button = new SfButton
{
    Text = "Ripple Effect",
    EnableRippleEffect = true,
    Background = Color.FromArgb("#6200EE")
};
```

> **Note:** Ripple effects provide visual feedback on tap and are enabled by default on Android.

## Custom Content

Add custom content using below codes:

**XAML:**
```xml
<buttons:SfButton  CornerRadius="10" Text="SfButton" Background="#4125BC">
            <buttons:SfButton.Content>
                <DataTemplate>
                    <HorizontalStackLayout Spacing = "8" Padding="5">
                        <ActivityIndicator Color = "White" IsRunning="True"/>
                        <Label Text = "Loading..." VerticalOptions="Center" TextColor="White"/>
                    </HorizontalStackLayout>
                </DataTemplate>
            </buttons:SfButton.Content>
        </buttons:SfButton>
```

**C#:**
```csharp
// File-based
var customTemplate = new DataTemplate(() =>
{
    var activityIndicator = new ActivityIndicator
    {
        Color = Colors.White,
        IsRunning = true
    };
    var label = new Label
    {
        Text = "Loading...",
        TextColor = Colors.White,
        VerticalOptions = LayoutOptions.Center
    };
    var stackLayout = new HorizontalStackLayout
    {
        Spacing = 8,
        Padding = new Thickness(5)
    };
    stackLayout.Children.Add(activityIndicator);
    stackLayout.Children.Add(label);
    return stackLayout;
});
SfButton button = new SfButton
{
    Text = "SfButton",
    Background = Color.FromArgb("#4125BC"),
    CornerRadius= 10,
    Content = customTemplate
};

```

## Command Pattern

Bind buttons to ViewModel commands (MVVM):

**ViewModel:**
```csharp
public class MyViewModel : INotifyPropertyChanged
{
    private Color _backgroundColor = Colors.Blue;

    public Color BackgroundColor
    {
        get => _backgroundColor;
        set
        {
            _backgroundColor = value;
            OnPropertyChanged();
        }
    }

    public ICommand ButtonCommand => new Command(OnButtonClicked);

    private void OnButtonClicked()
    {
        BackgroundColor = BackgroundColor == Colors.Blue 
            ? Colors.Green 
            : Colors.Blue;
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged([CallerMemberName] string name = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
    }
}
```

**XAML:**
```xml
<ContentPage.BindingContext>
    <local:MyViewModel />
</ContentPage.BindingContext>

<buttons:SfButton Text="Click Me"
                  Background="{Binding BackgroundColor}"
                  Command="{Binding ButtonCommand}" />
```

## Complete Examples

### Example 1: Material Design Primary Button

```xml
<buttons:SfButton Text="PRIMARY ACTION"
                  TextColor="White"
                  Background="#6200EE"
                  CornerRadius="4"
                  HeightRequest="40"
                  WidthRequest="200"
                  FontSize="14"
                  FontAttributes="Bold"
                  TextTransform="Uppercase"
                  EnableRippleEffect="True" />
```

### Example 2: Gradient Button with Icon

```xml
<buttons:SfButton Text="Download"
                  TextColor="White"
                  ShowIcon="True"
                  ImageSource="download_icon.png"
                  ImageSize="20"
                  ImageAlignment="Start"
                  CornerRadius="8"
                  HeightRequest="44"
                  Padding="20,0">
    <buttons:SfButton.Background>
        <LinearGradientBrush EndPoint="1,0">
            <GradientStop Color="#56ab2f" Offset="0" />
            <GradientStop Color="#a8e063" Offset="1" />
        </LinearGradientBrush>
    </buttons:SfButton.Background>
</buttons:SfButton>
```

### Example 3: Outline Button

```xml
<buttons:SfButton Text="Cancel"
                  TextColor="#6200EE"
                  Background="Transparent"
                  Stroke="#6200EE"
                  StrokeThickness="2"
                  CornerRadius="8"
                  HeightRequest="40"
                  WidthRequest="150"
                  FontSize="14" />
```

### Example 4: Circular Icon Button

```xml
<buttons:SfButton ShowIcon="True"
                  ImageSource="favorite_icon.png"
                  ImageSize="24"
                  Background="#E91E63"
                  CornerRadius="28"
                  WidthRequest="56"
                  HeightRequest="56"
                  Padding="0"
                  EnableRippleEffect="True" />
```

### Example 5: Text Button (Flat)

```xml
<buttons:SfButton Text="Learn More"
                  TextColor="#6200EE"
                  Background="Transparent"
                  FontSize="14"
                  FontAttributes="Bold"
                  TextTransform="Uppercase"
                  Padding="16,8" />
```

## Best Practices

1. **Use `Background` not `BackgroundColor`** for SfButton styling
2. **Set `StrokeThickness > 0`** to make borders visible
3. **Enable `ShowIcon`** before using `ImageSource`
4. **Match corner radius** to half of height for circular buttons
5. **Use consistent sizing** across your app (e.g., 44pt height for primary buttons)
6. **Provide sufficient contrast** between text and background colors
7. **Use `EnableRippleEffect`** for better touch feedback
8. **Leverage gradients** for modern, eye-catching designs
9. **Test on multiple platforms** as rendering may vary slightly

## Quick Reference

| Property | Values | Purpose |
|----------|--------|---------|
| `Text` | string | Button text |
| `TextColor` | Color | Text color |
| `FontSize` | double | Text size |
| `FontAttributes` | None, Bold, Italic | Text style |
| `FontFamily` | string | Custom font |
| `Background` | Color/Brush | Background color or gradient |
| `Stroke` | Color | Border color |
| `StrokeThickness` | double | Border width |
| `CornerRadius` | double | Rounded corners |
| `ShowIcon` | bool | Display icon |
| `ImageSource` | ImageSource | Icon image |
| `ImageSize` | double | Icon dimensions |
| `ImageAlignment` | Alignment | Icon position |
| `Padding` | Thickness | Internal spacing |
| `WidthRequest` | double | Requested width |
| `HeightRequest` | double | Requested height |
| `EnableRippleEffect` | bool | Touch feedback |
| `Command` | ICommand | MVVM command binding |
