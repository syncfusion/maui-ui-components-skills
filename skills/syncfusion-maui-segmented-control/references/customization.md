# Appearance Customization

## Table of Contents
- [Overview](#overview)
- [Border Customization](#border-customization)
- [Corner Radius](#corner-radius)
- [Text Styling](#text-styling)
- [Background Colors](#background-colors)
- [Separator Visibility](#separator-visibility)
- [DataTemplate Customization](#datatemplate-customization)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

The Segmented Control offers extensive appearance customization options to match your app's design system. Customize borders, colors, text styles, corner radius, separators, and create fully custom segment layouts using DataTemplates.

**Customization Levels:**
- **Global:** Apply styles to all segments via SfSegmentedControl properties
- **Per-Item:** Customize individual segments via SfSegmentItem properties
- **Custom Templates:** Complete control with DataTemplates

## Border Customization

### Border Color

Customize the outer border color of the entire segmented control.

**XAML:**
```xml
<buttons:SfSegmentedControl Stroke="#E91E63" StrokeThickness="2">
    <buttons:SfSegmentedControl.ItemsSource>
        <x:Array Type="{x:Type x:String}">
            <x:String>Day</x:String>
            <x:String>Week</x:String>
            <x:String>Month</x:String>
        </x:Array>
    </buttons:SfSegmentedControl.ItemsSource>
</buttons:SfSegmentedControl>
```

**C#:**
```csharp
var segmentedControl = new SfSegmentedControl
{
    Stroke = new SolidColorBrush(Color.FromArgb("#E91E63")),
    StrokeThickness = 2,
    ItemsSource = new List<string> { "Day", "Week", "Month" }
};
```

**Common colors:**
- Primary brand color for emphasis
- Light gray (#E0E0E0) for subtle borders
- Transparent for borderless appearance

### Border Thickness

Control the width of the outer border.

**XAML:**
```xml
<buttons:SfSegmentedControl StrokeThickness="3">
    <!-- Items -->
</buttons:SfSegmentedControl>
```

**C#:**
```csharp
segmentedControl.StrokeThickness = 3;
```

**Recommended values:**
- Thin: 1 pixel (subtle)
- Standard: 2 pixels (balanced)
- Thick: 3-4 pixels (prominent)

**Note:** Setting `StrokeThickness="0"` removes the border entirely.

## Corner Radius

### Control Corner Radius

Apply corner radius to the first and last segments only (creates capsule-like ends).

**XAML:**
```xml
<buttons:SfSegmentedControl CornerRadius="20">
    <buttons:SfSegmentedControl.ItemsSource>
        <x:Array Type="{x:Type x:String}">
            <x:String>Day</x:String>
            <x:String>Week</x:String>
            <x:String>Month</x:String>
        </x:Array>
    </buttons:SfSegmentedControl.ItemsSource>
</buttons:SfSegmentedControl>
```

**C#:**
```csharp
segmentedControl.CornerRadius = 20;
```

**Effects:**
- First segment: Left side rounded
- Middle segments: Square edges
- Last segment: Right side rounded

**Use case:** Pill-shaped segmented controls, iOS-style switches.

### Segment Corner Radius

Apply corner radius to all segments individually.

**XAML:**
```xml
<buttons:SfSegmentedControl SegmentCornerRadius="8">
    <buttons:SfSegmentedControl.ItemsSource>
        <x:Array Type="{x:Type x:String}">
            <x:String>Day</x:String>
            <x:String>Week</x:String>
            <x:String>Month</x:String>
        </x:Array>
    </buttons:SfSegmentedControl.ItemsSource>
</buttons:SfSegmentedControl>
```

**C#:**
```csharp
segmentedControl.SegmentCornerRadius = 8;
```

**Effects:**
- All segments have rounded corners
- Creates separated, button-like appearance
- Works well with spacing between segments

**Difference from CornerRadius:**
- `CornerRadius`: Only first and last segments rounded
- `SegmentCornerRadius`: All segments rounded

### Non-Uniform Corner Radius

Apply different radius values to each corner.

**C#:**
```csharp
segmentedControl.CornerRadius = new CornerRadius(20, 20, 0, 0);  
// topLeft, topRight, bottomRight, bottomLeft
```

**Use case:** Attach segmented control to the bottom of another element (round only top corners).

## Text Styling

### Global Text Style

Apply text styling to all segments.

**XAML:**
```xml
<buttons:SfSegmentedControl>
    <buttons:SfSegmentedControl.TextStyle>
        <buttons:SegmentTextStyle 
            TextColor="#212121"
            FontSize="16"
            FontAttributes="Bold"
            FontFamily="Arial"/>
    </buttons:SfSegmentedControl.TextStyle>
    <buttons:SfSegmentedControl.ItemsSource>
        <x:Array Type="{x:Type x:String}">
            <x:String>Day</x:String>
            <x:String>Week</x:String>
            <x:String>Month</x:String>
        </x:Array>
    </buttons:SfSegmentedControl.ItemsSource>
</buttons:SfSegmentedControl>
```

**C#:**
```csharp
segmentedControl.TextStyle = new SegmentTextStyle
{
    TextColor = Color.FromArgb("#212121"),
    FontSize = 16,
    FontAttributes = FontAttributes.Bold,
    FontFamily = "Arial"
};
```

**SegmentTextStyle Properties:**
- `TextColor`: Text color (unselected state)
- `FontSize`: Text size in device-independent units
- `FontAttributes`: None, Bold, Italic, or Bold | Italic
- `FontFamily`: Font name (must be registered in MauiProgram.cs)

### Per-Item Text Style

Customize text styling for individual segments.

**C#:**
```csharp
var segmentedControl = new SfSegmentedControl
{
    ItemsSource = new List<SfSegmentItem>
    {
        new SfSegmentItem 
        { 
            Text = "Low",
            TextStyle = new SegmentTextStyle 
            { 
                TextColor = Colors.Green,
                FontSize = 14
            }
        },
        new SfSegmentItem 
        { 
            Text = "Medium",
            TextStyle = new SegmentTextStyle 
            { 
                TextColor = Colors.Orange,
                FontSize = 16,
                FontAttributes = FontAttributes.Bold
            }
        },
        new SfSegmentItem 
        { 
            Text = "High",
            TextStyle = new SegmentTextStyle 
            { 
                TextColor = Colors.Red,
                FontSize = 18,
                FontAttributes = FontAttributes.Bold
            }
        }
    }
};
```

**Use case:** Emphasize specific segments, semantic coloring, priority levels.

### Font Size Recommendations

- **Small devices (phones):** 12-14 pixels
- **Standard:** 14-16 pixels
- **Large/accessible:** 18-20 pixels
- **Avoid:** Below 10 pixels (readability issues)

## Background Colors

### Global Segment Background

Set the background color for all unselected segments.

**XAML:**
```xml
<buttons:SfSegmentedControl SegmentBackground="#F5F5F5">
    <buttons:SfSegmentedControl.ItemsSource>
        <x:Array Type="{x:Type x:String}">
            <x:String>Day</x:String>
            <x:String>Week</x:String>
            <x:String>Month</x:String>
        </x:Array>
    </buttons:SfSegmentedControl.ItemsSource>
</buttons:SfSegmentedControl>
```

**C#:**
```csharp
segmentedControl.SegmentBackground = Color.FromArgb("#F5F5F5");
```

**Common patterns:**
- Light background with dark text for light themes
- Dark background with light text for dark themes
- Transparent background for overlay controls

### Per-Item Background

Customize the background for individual segments.

**C#:**
```csharp
var segmentedControl = new SfSegmentedControl
{
    ItemsSource = new List<SfSegmentItem>
    {
        new SfSegmentItem 
        { 
            Text = "Active",
            Background = Colors.LightGreen 
        },
        new SfSegmentItem 
        { 
            Text = "Pending",
            Background = Colors.LightYellow 
        },
        new SfSegmentItem 
        { 
            Text = "Inactive",
            Background = Colors.LightGray 
        }
    }
};
```

**Use case:** Status indicators, color-coded categories, visual grouping.

### Gradient Backgrounds

Apply gradient backgrounds using Brush objects.

**C#:**
```csharp
var segmentedControl = new SfSegmentedControl
{
    ItemsSource = new List<SfSegmentItem>
    {
        new SfSegmentItem 
        { 
            Text = "Sunrise",
            Background = new LinearGradientBrush
            {
                StartPoint = new Point(0, 0),
                EndPoint = new Point(1, 0),
                GradientStops = new GradientStopCollection
                {
                    new GradientStop { Color = Colors.Orange, Offset = 0.0f },
                    new GradientStop { Color = Colors.Yellow, Offset = 1.0f }
                }
            }
        }
    }
};
```

## Separator Visibility

Control the visibility of separators between segments.

### Hide Separators

**XAML:**
```xml
<buttons:SfSegmentedControl ShowSeparator="False">
    <buttons:SfSegmentedControl.ItemsSource>
        <x:Array Type="{x:Type x:String}">
            <x:String>Day</x:String>
            <x:String>Week</x:String>
            <x:String>Month</x:String>
        </x:Array>
    </buttons:SfSegmentedControl.ItemsSource>
</buttons:SfSegmentedControl>
```

**C#:**
```csharp
segmentedControl.ShowSeparator = false;
```

**Effect:** Removes vertical lines between segments, creating a seamless appearance.

### Show Separators (Default)

**XAML:**
```xml
<buttons:SfSegmentedControl ShowSeparator="True">
    <!-- Items -->
</buttons:SfSegmentedControl>
```

**C#:**
```csharp
segmentedControl.ShowSeparator = true;  // Default
```

**When to hide separators:**
- Using SegmentCornerRadius (separated segments)
- Custom backgrounds make divisions obvious
- Minimalist design requires cleaner appearance

**When to show separators:**
- Default appearance with contiguous segments
- Similar segment colors need visual division
- Accessibility: clearer boundaries for touch targets

## DataTemplate Customization

Use DataTemplates to create fully custom segment layouts beyond text and icons.

### Basic DataTemplate

**XAML:**
```xml
<buttons:SfSegmentedControl>
    <buttons:SfSegmentedControl.ItemsSource>
        <x:Array Type="{x:Type x:String}">
            <x:String>Day</x:String>
            <x:String>Week</x:String>
            <x:String>Month</x:String>
        </x:Array>
    </buttons:SfSegmentedControl.ItemsSource>
    
    <buttons:SfSegmentedControl.SegmentTemplate>
        <DataTemplate>
            <Grid BackgroundColor="LightBlue" Padding="10">
                <Label Text="{Binding Text}" 
                       TextColor="DarkBlue"
                       FontSize="16"
                       HorizontalOptions="Center"
                       VerticalOptions="Center"/>
            </Grid>
        </DataTemplate>
    </buttons:SfSegmentedControl.SegmentTemplate>
</buttons:SfSegmentedControl>
```

**BindingContext:** When using `SegmentTemplate`, the BindingContext is the `SfSegmentItem` object. Access properties via `{Binding Text}`, `{Binding ImageSource}`, etc.

### Advanced DataTemplate with Icons

**XAML:**
```xml
<buttons:SfSegmentedControl>
    <buttons:SfSegmentedControl.SegmentTemplate>
        <DataTemplate>
            <StackLayout Orientation="Vertical" 
                         Padding="10" 
                         Spacing="5">
                <Image Source="{Binding ImageSource}" 
                       HeightRequest="24" 
                       WidthRequest="24"
                       HorizontalOptions="Center"/>
                <Label Text="{Binding Text}" 
                       FontSize="12"
                       HorizontalOptions="Center"/>
            </StackLayout>
        </DataTemplate>
    </buttons:SfSegmentedControl.SegmentTemplate>
</buttons:SfSegmentedControl>
```

### Selected State in DataTemplate

Use the `IsSelected` property to apply different styles based on selection state.

**XAML:**
```xml
<ContentPage.Resources>
    <local:SelectedColorConverter x:Key="SelectedColorConverter"/>
</ContentPage.Resources>

<buttons:SfSegmentedControl>
    <buttons:SfSegmentedControl.ItemsSource>
        <x:Array Type="{x:Type x:String}">
            <x:String>Active</x:String>
            <x:String>Completed</x:String>
            <x:String>Archived</x:String>
        </x:Array>
    </buttons:SfSegmentedControl.ItemsSource>
    
    <buttons:SfSegmentedControl.SegmentTemplate>
        <DataTemplate>
            <Grid BackgroundColor="Transparent">
                <Label Text="{Binding Text}"
                       TextColor="{Binding IsSelected, Converter={StaticResource SelectedColorConverter}}"
                       FontAttributes="{Binding IsSelected, Converter={StaticResource SelectedFontConverter}}"
                       FontSize="16"
                       Padding="10"
                       HorizontalOptions="Center"
                       VerticalOptions="Center"/>
            </Grid>
        </DataTemplate>
    </buttons:SfSegmentedControl.SegmentTemplate>
</buttons:SfSegmentedControl>
```

**Converter Implementation:**
```csharp
public class SelectedColorConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value is bool isSelected)
        {
            return isSelected ? Colors.Blue : Colors.Gray;
        }
        return Colors.Black;
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```

### Complex Custom Layout

**C#:**
```csharp
segmentedControl.SegmentTemplate = new DataTemplate(() =>
{
    var grid = new Grid
    {
        RowDefinitions = 
        {
            new RowDefinition { Height = GridLength.Auto },
            new RowDefinition { Height = GridLength.Auto }
        },
        Padding = 10
    };

    var icon = new Image 
    { 
        HeightRequest = 32, 
        WidthRequest = 32,
        HorizontalOptions = LayoutOptions.Center
    };
    icon.SetBinding(Image.SourceProperty, "ImageSource");
    Grid.SetRow(icon, 0);

    var label = new Label 
    { 
        FontSize = 12,
        HorizontalOptions = LayoutOptions.Center
    };
    label.SetBinding(Label.TextProperty, "Text");
    Grid.SetRow(label, 1);

    grid.Children.Add(icon);
    grid.Children.Add(label);

    return grid;
});
```

## Best Practices

### Color Contrast

Ensure text is readable against backgrounds (WCAG AA minimum 4.5:1 for normal text, 3:1 for large text):

```csharp
// Good contrast
SegmentBackground = Colors.White
TextColor = Colors.Black

// Poor contrast (avoid)
SegmentBackground = Colors.LightGray
TextColor = Colors.White
```

### Consistent Styling

- Apply consistent corner radius across all segments
- Use the same font family throughout the control
- Maintain uniform border thickness

### Performance

- Avoid complex DataTemplates with many nested elements
- Cache images and fonts
- Use simple layouts for better rendering performance

### Accessibility

- Minimum touch target size: 44x44 points (iOS), 48x48 dp (Android)
- Ensure color is not the only indicator (use icons or text)
- Provide sufficient spacing between segments (minimum 8 pixels)

### Responsive Design

- Test on multiple screen sizes
- Use relative sizing (percentage-based widths) when possible
- Adjust font sizes based on device screen density

## Troubleshooting

### Custom Colors Not Applying

**Cause:** Property set on wrong level (control vs item)  
**Solution:** Use `SegmentBackground` for control-level, `Background` on SfSegmentItem for item-level

### Text Cut Off

**Cause:** Insufficient segment width or height  
**Solution:** Increase `SegmentWidth` or `SegmentHeight`, or reduce `FontSize`

### DataTemplate Not Rendering

**Cause:** Missing BindingContext or incorrect property binding  
**Solution:** Verify BindingContext is SfSegmentItem and properties match (Text, ImageSource, IsSelected)

### Separator Still Visible After Setting ShowSeparator=False

**Cause:** Theme or custom style overriding property  
**Solution:** Set ShowSeparator programmatically after control initialization

### Corner Radius Not Uniform

**Cause:** Using CornerRadius instead of SegmentCornerRadius  
**Solution:** Use `SegmentCornerRadius` to apply to all segments equally

## Next Steps

- **Configure selection:** See [selection.md](selection.md) for selection indicators and modes
- **Layout sizing:** See [layout.md](layout.md) for width, height, and scrolling
- **Handle events:** See [events.md](events.md) for user interaction responses
