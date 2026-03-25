# Advanced Features in .NET MAUI DatePicker

The .NET MAUI DatePicker includes advanced features for modern UI experiences, including text display modes and liquid glass effects.

## Text Display Modes

Text display modes control how date picker items are displayed relative to the selected item, creating visual effects that enhance usability and aesthetics.

### Available Modes

The `TextDisplayMode` property accepts values from the `PickerTextDisplayMode` enumeration:

- **Default** - Standard display without effects (default)
- **Fade** - Gradually decreases opacity of items away from selection
- **Shrink** - Decreases font size of items away from selection
- **FadeAndShrink** - Combines both fade and shrink effects

### Default Mode

The standard display mode without visual effects.

```xml
<picker:SfDatePicker x:Name="datePicker"
                     TextDisplayMode="Default">
</picker:SfDatePicker>
```

```csharp
SfDatePicker datePicker = new SfDatePicker()
{
    TextDisplayMode = PickerTextDisplayMode.Default
};
```

**Use Case:** When visual effects might be distracting or when maximum readability is required.

### Fade Mode

Gradually decreases the visibility (opacity) of unselected items relative to the selected item.

```xml
<picker:SfDatePicker x:Name="datePicker"
                     TextDisplayMode="Fade">
</picker:SfDatePicker>
```

```csharp
SfDatePicker datePicker = new SfDatePicker()
{
    TextDisplayMode = PickerTextDisplayMode.Fade
};
```

**Behavior:**
- Selected item: 100% opacity
- Items one position away: ~75% opacity
- Items two positions away: ~50% opacity
- Items three+ positions away: ~25% opacity

**Use Case:** Drawing focus to the selected item while keeping context visible.

### Shrink Mode

Decreases the font size of items as they move away from the selected item.

```xml
<picker:SfDatePicker x:Name="datePicker"
                     TextDisplayMode="Shrink">
</picker:SfDatePicker>
```

```csharp
SfDatePicker datePicker = new SfDatePicker()
{
    TextDisplayMode = PickerTextDisplayMode.Shrink
};
```

**Behavior:**
- Selected item: 100% font size
- Items one position away: ~85% font size
- Items two positions away: ~70% font size
- Items three+ positions away: ~60% font size

**Use Case:** Creating depth perception and emphasizing the selected value.

### FadeAndShrink Mode

Combines both fade and shrink effects for maximum visual emphasis.

```xml
<picker:SfDatePicker x:Name="datePicker"
                     TextDisplayMode="FadeAndShrink">
</picker:SfDatePicker>
```

```csharp
SfDatePicker datePicker = new SfDatePicker()
{
    TextDisplayMode = PickerTextDisplayMode.FadeAndShrink
};
```

**Behavior:**
- Applies both opacity reduction and font size reduction simultaneously
- Creates the strongest visual focus on the selected item

**Use Case:** Modern, visually appealing UIs where the selected item should stand out prominently.

### Comparison Example

```xml
<StackLayout Padding="20" Spacing="30">
    <Label Text="Text Display Modes Comparison" 
           FontSize="22" 
           FontAttributes="Bold"/>
    
    <!-- Default Mode -->
    <StackLayout>
        <Label Text="Default Mode" FontAttributes="Bold"/>
        <picker:SfDatePicker TextDisplayMode="Default" HeightRequest="200"/>
    </StackLayout>
    
    <!-- Fade Mode -->
    <StackLayout>
        <Label Text="Fade Mode" FontAttributes="Bold"/>
        <picker:SfDatePicker TextDisplayMode="Fade" HeightRequest="200"/>
    </StackLayout>
    
    <!-- Shrink Mode -->
    <StackLayout>
        <Label Text="Shrink Mode" FontAttributes="Bold"/>
        <picker:SfDatePicker TextDisplayMode="Shrink" HeightRequest="200"/>
    </StackLayout>
    
    <!-- FadeAndShrink Mode -->
    <StackLayout>
        <Label Text="Fade and Shrink Mode" FontAttributes="Bold"/>
        <picker:SfDatePicker TextDisplayMode="FadeAndShrink" HeightRequest="200"/>
    </StackLayout>
</StackLayout>
```

### Dynamic Mode Switching

```csharp
public class DatePickerPage : ContentPage
{
    private SfDatePicker datePicker;
    
    public DatePickerPage()
    {
        InitializeComponent();
        SetupPicker();
    }
    
    private void SetupPicker()
    {
        datePicker = new SfDatePicker();
        // Set initial mode
        datePicker.TextDisplayMode = PickerTextDisplayMode.FadeAndShrink;
    }
    
    private void OnModeChanged(object sender, EventArgs e)
    {
        var selectedMode = modePicker.SelectedItem as string;
        
        datePicker.TextDisplayMode = selectedMode switch
        {
            "Default" => PickerTextDisplayMode.Default,
            "Fade" => PickerTextDisplayMode.Fade,
            "Shrink" => PickerTextDisplayMode.Shrink,
            "Fade and Shrink" => PickerTextDisplayMode.FadeAndShrink,
            _ => PickerTextDisplayMode.Default
        };
    }
}
```

## Liquid Glass Effect

The Liquid Glass Effect introduces a modern, translucent design with adaptive color tinting and light refraction, creating a sleek, glass-like user experience.

### Platform Requirements

**⚠️ Important:** Liquid Glass Effect is available only in:
- **.NET 10** or higher
- **macOS 26+**
- **iOS 26+**

### Step 1: Wrap Control in Glass Effect View

Wrap the DatePicker inside the `SfGlassEffectView` control.

**Prerequisites:** Install the `Syncfusion.Maui.Core` NuGet package (included as a dependency of `Syncfusion.Maui.Picker`).

### Step 2: Enable Liquid Glass Effect

Set the `EnableLiquidGlassEffect` property to `true`.

### Step 3: Set Transparent Background

Set the `Background` property to `Transparent` for the glass effect to work properly.

### Complete Implementation

#### XAML

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:picker="clr-namespace:Syncfusion.Maui.Picker;assembly=Syncfusion.Maui.Picker"
             xmlns:core="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
             x:Class="MyApp.DatePickerPage">
    
    <Grid>
        <!-- Background gradient for glass effect -->
        <Grid.Background>
            <LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
                <GradientStop Color="#0F4C75" Offset="0.0"/>
                <GradientStop Color="#3282B8" Offset="0.5"/>
                <GradientStop Color="#1B262C" Offset="1.0"/>
            </LinearGradientBrush>
        </Grid.Background>
        
        <!-- Glass Effect View -->
        <core:SfGlassEffectView EffectType="Regular"
                                CornerRadius="20"
                                WidthRequest="350"
                                HeightRequest="350"
                                HorizontalOptions="Center"
                                VerticalOptions="Center">
            
            <!-- DatePicker with Liquid Glass Effect -->
            <picker:SfDatePicker x:Name="datePicker"
                                 EnableLiquidGlassEffect="True"
                                 Background="Transparent">
                
                <!-- Column Header with Transparent Background -->
                <picker:SfDatePicker.ColumnHeaderView>
                    <picker:DatePickerColumnHeaderView Background="Transparent"/>
                </picker:SfDatePicker.ColumnHeaderView>
                
            </picker:SfDatePicker>
            
        </core:SfGlassEffectView>
    </Grid>
    
</ContentPage>
```

#### C#

```csharp
using Syncfusion.Maui.Core;
using Syncfusion.Maui.Picker;

namespace MyApp
{
    public partial class DatePickerPage : ContentPage
    {
        public DatePickerPage()
        {
            InitializeComponent();
            SetupLiquidGlassDatePicker();
        }
        
        private void SetupLiquidGlassDatePicker()
        {
            // Create main grid with gradient background
            var mainGrid = new Grid
            {
                Background = new LinearGradientBrush
                {
                    StartPoint = new Point(0, 0),
                    EndPoint = new Point(0, 1),
                    GradientStops =
                    {
                        new GradientStop { Color = Color.FromArgb("#0F4C75"), Offset = 0.0f },
                        new GradientStop { Color = Color.FromArgb("#3282B8"), Offset = 0.5f },
                        new GradientStop { Color = Color.FromArgb("#1B262C"), Offset = 1.0f }
                    }
                }
            };
            
            // Create glass effect view
            var glassView = new SfGlassEffectView
            {
                CornerRadius = 20,
                EffectType = LiquidGlassEffectType.Regular,
                WidthRequest = 350,
                HeightRequest = 350,
                HorizontalOptions = LayoutOptions.Center,
                VerticalOptions = LayoutOptions.Center
            };
            
            // Create date picker with liquid glass effect
            var datePicker = new SfDatePicker
            {
                EnableLiquidGlassEffect = true,
                Background = Colors.Transparent,
                ColumnHeaderView = new DatePickerColumnHeaderView
                {
                    Background = Colors.Transparent
                }
            };
            
            glassView.Content = datePicker;
            mainGrid.Children.Add(glassView);
            
            this.Content = mainGrid;
        }
    }
}
```

### Glass Effect Types

The `SfGlassEffectView` supports different effect types:

```csharp
// Regular glass effect (default)
glassView.EffectType = LiquidGlassEffectType.Regular;

// Thin glass effect (more translucent)
glassView.EffectType = LiquidGlassEffectType.Clear;
```

### Customizing Glass Effect

```xml
<core:SfGlassEffectView EffectType="Regular"
                        CornerRadius="25"
                        WidthRequest="350"
                        HeightRequest="400"
                        Padding="10">
    
    <picker:SfDatePicker EnableLiquidGlassEffect="True"
                         Background="Transparent">
        
        <!-- Header with transparent background -->
        <picker:SfDatePicker.HeaderView>
            <picker:PickerHeaderView Text="Select Date"
                                     Height="50"
                                     Background="Transparent">
                <picker:PickerHeaderView.TextStyle>
                    <picker:PickerTextStyle TextColor="White" 
                                            FontSize="20" 
                                            FontAttributes="Bold"/>
                </picker:PickerHeaderView.TextStyle>
            </picker:PickerHeaderView>
        </picker:SfDatePicker.HeaderView>
        
        <!-- Column header with transparent background -->
        <picker:SfDatePicker.ColumnHeaderView>
            <picker:DatePickerColumnHeaderView Background="Transparent">
                <picker:DatePickerColumnHeaderView.TextStyle>
                    <picker:PickerTextStyle TextColor="White" FontSize="14"/>
                </picker:DatePickerColumnHeaderView.TextStyle>
            </picker:DatePickerColumnHeaderView>
        </picker:SfDatePicker.ColumnHeaderView>
        
        <!-- Selected text style -->
        <picker:SfDatePicker.SelectedTextStyle>
            <picker:PickerTextStyle TextColor="White" 
                                    FontSize="18" 
                                    FontAttributes="Bold"/>
        </picker:SfDatePicker.SelectedTextStyle>
        
    </picker:SfDatePicker>
    
</core:SfGlassEffectView>
```

### Best Practices for Liquid Glass Effect

1. **Use Contrasting Backgrounds** - The glass effect works best with gradient or image backgrounds

2. **Set All Backgrounds to Transparent** - Ensure the DatePicker and its child elements (header, column header, footer) have transparent backgrounds

3. **Adjust Text Colors** - Use light text colors (white, light gray) for better visibility on glass backgrounds

4. **Test on Target Platforms** - Only available on macOS 26+ and iOS 26+ with .NET 10

5. **Provide Fallback** - For older platforms, provide a non-glass alternative design

### Platform Availability Check

```csharp
public bool IsLiquidGlassSupported()
{
    #if NET10_0_OR_GREATER
        #if IOS || MACCATALYST
            if (UIDevice.CurrentDevice.CheckSystemVersion(26, 0))
            {
                return true;
            }
        #endif
    #endif
    return false;
}

public void SetupDatePicker()
{
    if (IsLiquidGlassSupported())
    {
        // Use liquid glass effect
        SetupLiquidGlassDatePicker();
    }
    else
    {
        // Use standard styling
        SetupStandardDatePicker();
    }
}
```

## Combining Advanced Features

### Example: Liquid Glass with FadeAndShrink Mode

```xml
<Grid>
    <Grid.Background>
        <LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
            <GradientStop Color="#1A237E" Offset="0.0"/>
            <GradientStop Color="#283593" Offset="0.5"/>
            <GradientStop Color="#3F51B5" Offset="1.0"/>
        </LinearGradientBrush>
    </Grid.Background>
    
    <core:SfGlassEffectView EffectType="Regular"
                            CornerRadius="20"
                            WidthRequest="350"
                            HeightRequest="380"
                            HorizontalOptions="Center"
                            VerticalOptions="Center">
        
        <picker:SfDatePicker EnableLiquidGlassEffect="True"
                             Background="Transparent"
                             TextDisplayMode="FadeAndShrink">
            
            <picker:SfDatePicker.HeaderView>
                <picker:PickerHeaderView Text="Modern Date Picker"
                                         Height="50"
                                         Background="Transparent">
                    <picker:PickerHeaderView.TextStyle>
                        <picker:PickerTextStyle TextColor="White" 
                                                FontSize="20" 
                                                FontAttributes="Bold"/>
                    </picker:PickerHeaderView.TextStyle>
                </picker:PickerHeaderView>
            </picker:SfDatePicker.HeaderView>
            
            <picker:SfDatePicker.ColumnHeaderView>
                <picker:DatePickerColumnHeaderView Background="Transparent">
                    <picker:DatePickerColumnHeaderView.TextStyle>
                        <picker:PickerTextStyle TextColor="White" 
                                                FontSize="14" 
                                                FontAttributes="Bold"/>
                    </picker:DatePickerColumnHeaderView.TextStyle>
                </picker:DatePickerColumnHeaderView>
            </picker:SfDatePicker.ColumnHeaderView>
            
            <picker:SfDatePicker.SelectedTextStyle>
                <picker:PickerTextStyle TextColor="White" 
                                        FontSize="20" 
                                        FontAttributes="Bold"/>
            </picker:SfDatePicker.SelectedTextStyle>
            
            <picker:SfDatePicker.TextStyle>
                <picker:PickerTextStyle TextColor="#E0E0E0" 
                                        FontSize="16"/>
            </picker:SfDatePicker.TextStyle>
            
        </picker:SfDatePicker>
        
    </core:SfGlassEffectView>
</Grid>
```

This combines:
- **Liquid Glass Effect** for modern translucent appearance
- **FadeAndShrink Mode** for visual emphasis on selected items
- **Gradient Background** for depth and visual appeal
- **Custom Text Styles** for enhanced readability

## Best Practices

### Text Display Modes

1. **Match UI Design Language** - Choose modes that align with your app's overall design
2. **Consider Readability** - Ensure unselected items remain readable, especially for accessibility
3. **Test on Real Devices** - Visual effects may appear differently on various devices
4. **Use Sparingly** - Not every picker needs effects; use where it enhances UX

### Liquid Glass Effect

1. **Platform Compatibility** - Check platform version before enabling
2. **Performance** - Glass effects can impact performance on lower-end devices
3. **Accessibility** - Ensure sufficient contrast for users with vision impairments
4. **Background Choice** - Use visually interesting backgrounds to showcase the glass effect

## Troubleshooting

### Issue: TextDisplayMode not visible
**Solution:** Ensure the picker has sufficient height to display multiple items. Effects are most visible with 5+ visible items.

### Issue: Liquid glass effect not showing
**Solution:** Verify:
- Platform is .NET 10 with macOS 26+ or iOS 26+
- `EnableLiquidGlassEffect="True"`
- `Background="Transparent"` on DatePicker and child views
- Wrapped in `SfGlassEffectView`

### Issue: Text not readable with glass effect
**Solution:** Adjust text colors to white or light colors for better contrast against glass backgrounds.

## Related Topics

- **Customization** - Combine advanced features with customization options
- **Picker Modes** - Use advanced features with different picker modes
- **Accessibility** - Ensure advanced features don't compromise accessibility
