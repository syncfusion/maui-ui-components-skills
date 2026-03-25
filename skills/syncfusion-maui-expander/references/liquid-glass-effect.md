# Liquid Glass Effect

## Table of Contents
- [Overview](#overview)
- [When to Use](#when-to-use)
- [Platform Requirements](#platform-requirements)
- [Step 1: Wrap in SfGlassEffectView](#step-1-wrap-in-sfglasseffectview)
- [Step 2: Enable Liquid Glass Effect](#step-2-enable-liquid-glass-effect)
- [Step 3: Set Transparent Background](#step-3-set-transparent-background)
- [Complete Implementation](#complete-implementation)
- [Multiple Expanders with Glass Effect](#multiple-expanders-with-glass-effect)
- [Troubleshooting](#troubleshooting)

---

## Overview

The Liquid Glass Effect introduces a modern, translucent design with:
- **Adaptive color tinting** - Blends with background gradients
- **Light refraction** - Creates depth and glass-like appearance
- **Smooth translucency** - Premium, high-end user experience

This effect creates a sleek, glass-like appearance while maintaining clarity and accessibility.

---

## When to Use

**Ideal for:**
- Premium/high-end applications
- Modern UI designs with gradient backgrounds
- Apps targeting iOS 26+ and macOS 26+
- Interfaces requiring visual depth and sophistication

**Not recommended for:**
- Apps requiring iOS 25 or earlier
- Simple, flat design patterns
- High-contrast accessibility requirements
- Performance-critical scenarios (animations may be intensive)

---

## Platform Requirements

⚠️ **Important:** Liquid Glass Effect has strict platform requirements.

| Requirement | Version |
|-------------|---------|
| **.NET** | 10 or higher |
| **macOS** | 26 or higher |
| **iOS** | 26 or higher |
| **Android** | Not supported |
| **Windows** | Not supported |

**Before using:** Verify your target platforms support these requirements.

---

## Step 1: Wrap in SfGlassEffectView

To apply the Liquid Glass Effect, wrap the SfExpander inside `SfGlassEffectView`.

### Import Namespace

```xml
xmlns:core="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
xmlns:syncfusion="clr-namespace:Syncfusion.Maui.Expander;assembly=Syncfusion.Maui.Expander"
```

### Basic Wrapper

```xml
<core:SfGlassEffectView EffectType="Regular"
                        CornerRadius="20">
    <syncfusion:SfExpander>
        <!-- Expander content -->
    </syncfusion:SfExpander>
</core:SfGlassEffectView>
```

### C# Wrapper

```csharp
using Syncfusion.Maui.Core;
using Syncfusion.Maui.Expander;

var glassView = new SfGlassEffectView
{
    EffectType = LiquidGlassEffectType.Regular,
    CornerRadius = 20
};

var expander = new SfExpander();
// Configure expander...

glassView.Content = expander;
```

**For more details on SfGlassEffectView:** See [Liquid Glass Getting Started documentation](https://help.syncfusion.com/maui/liquid-glass-ui/getting-started).

---

## Step 2: Enable Liquid Glass Effect

Set the `EnableLiquidGlassEffect` property to `true` on the SfExpander control.

### XAML

```xml
<core:SfGlassEffectView EffectType="Regular" CornerRadius="20">
    <syncfusion:SfExpander EnableLiquidGlassEffect="True"
                           AnimationDuration="200">
        <syncfusion:SfExpander.Header>
            <Grid Padding="15">
                <Label Text="Glass Effect Header" FontSize="16"/>
            </Grid>
        </syncfusion:SfExpander.Header>
        <syncfusion:SfExpander.Content>
            <Grid Padding="15">
                <Label Text="Glass effect content" FontSize="14"/>
            </Grid>
        </syncfusion:SfExpander.Content>
    </syncfusion:SfExpander>
</core:SfGlassEffectView>
```

### C#

```csharp
expander.EnableLiquidGlassEffect = true;
```

**Effect scope:** When enabled, the glass effect is applied to **both header and content**, providing a consistent translucent appearance.

---

## Step 3: Set Transparent Background

To achieve the glass-like effect, set the `Background` property to `Transparent`.

### XAML

```xml
<syncfusion:SfExpander EnableLiquidGlassEffect="True"
                       Background="Transparent">
    <!-- Header and Content -->
</syncfusion:SfExpander>
```

### C#

```csharp
expander.Background = Colors.Transparent;
```

**Why transparent?** The transparent background allows the underlying gradient or image to show through, creating the signature glass effect with adaptive color tinting.

---

## Complete Implementation

Full example with gradient background and glass effect:

### XAML

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:core="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.Expander;assembly=Syncfusion.Maui.Expander"
             x:Class="ExpanderSample.MainPage">
    
    <Grid>
        <!-- Gradient Background -->
        <Grid.Background>
            <LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
                <GradientStop Color="#0F4C75" Offset="0.0"/>
                <GradientStop Color="#3282B8" Offset="0.5"/>
                <GradientStop Color="#1B262C" Offset="1.0"/>
            </LinearGradientBrush>
        </Grid.Background>
        
        <!-- Glass Effect Container -->
        <core:SfGlassEffectView EffectType="Regular"
                                CornerRadius="20"
                                Margin="20">
            <ScrollView>
                <StackLayout Spacing="12" Padding="12">
                    
                    <!-- Title -->
                    <Label Text="Invoice: #FRU037020142097"
                           HeightRequest="40"
                           FontSize="16"
                           FontFamily="Roboto-Regular"
                           FontAttributes="Bold"
                           HorizontalOptions="Center"
                           VerticalTextAlignment="Center"
                           Margin="8,12,8,8"/>
                    
                    <!-- Expander 1: Invoice Date -->
                    <syncfusion:SfExpander AnimationDuration="200"
                                           IsExpanded="True"
                                           Background="Transparent"
                                           EnableLiquidGlassEffect="True"
                                           Margin="8">
                        <syncfusion:SfExpander.Header>
                            <Grid>
                                <Grid.RowDefinitions>
                                    <RowDefinition Height="48"/>
                                </Grid.RowDefinitions>
                                <Grid.ColumnDefinitions>
                                    <ColumnDefinition Width="35"/>
                                    <ColumnDefinition Width="*"/>
                                </Grid.ColumnDefinitions>
                                <Label Text="&#xe703;" 
                                       FontSize="16" 
                                       Margin="14,2,2,2"
                                       FontFamily="AccordionFontIcons.ttf#"
                                       VerticalOptions="Center" 
                                       VerticalTextAlignment="Center"/>
                                <Label CharacterSpacing="0.25" 
                                       FontFamily="Roboto-Regular" 
                                       Text="Invoice Date" 
                                       FontSize="14" 
                                       Grid.Column="1" 
                                       VerticalOptions="Center"/>
                            </Grid>
                        </syncfusion:SfExpander.Header>
                        <syncfusion:SfExpander.Content>
                            <Grid Padding="18,8,0,18">
                                <Label CharacterSpacing="0.25" 
                                       FontFamily="Roboto-Regular" 
                                       Text="11:03 AM, 15 January 2019" 
                                       FontSize="14" 
                                       VerticalOptions="Center"/>
                            </Grid>
                        </syncfusion:SfExpander.Content>
                    </syncfusion:SfExpander>
                    
                    <!-- Expander 2: Items -->
                    <syncfusion:SfExpander AnimationDuration="200"
                                           IsExpanded="False"
                                           Background="Transparent"
                                           EnableLiquidGlassEffect="True"
                                           Margin="8">
                        <syncfusion:SfExpander.Header>
                            <Grid>
                                <Grid.RowDefinitions>
                                    <RowDefinition Height="48"/>
                                </Grid.RowDefinitions>
                                <Grid.ColumnDefinitions>
                                    <ColumnDefinition Width="35"/>
                                    <ColumnDefinition Width="*"/>
                                </Grid.ColumnDefinitions>
                                <Label Text="&#xe701;" 
                                       FontSize="16" 
                                       Margin="14,2,2,2"
                                       FontFamily="AccordionFontIcons.ttf#"
                                       VerticalOptions="Center" 
                                       VerticalTextAlignment="Center"/>
                                <Label CharacterSpacing="0.25" 
                                       FontFamily="Roboto-Regular" 
                                       Text="Item(s)" 
                                       FontSize="14" 
                                       Grid.Column="1" 
                                       VerticalOptions="Center"/>
                            </Grid>
                        </syncfusion:SfExpander.Header>
                        <syncfusion:SfExpander.Content>
                            <Grid Padding="16,12,16,12">
                                <Grid.RowDefinitions>
                                    <RowDefinition Height="20"/>
                                </Grid.RowDefinitions>
                                <Grid.ColumnDefinitions>
                                    <ColumnDefinition Width="*"/>
                                    <ColumnDefinition Width="*"/>
                                </Grid.ColumnDefinitions>
                                <Label Text="Total Amount" 
                                       CharacterSpacing="0.25" 
                                       VerticalTextAlignment="Center" 
                                       FontSize="14"/>
                                <Label Text="$36,220.00" 
                                       CharacterSpacing="0.25" 
                                       HorizontalTextAlignment="End" 
                                       VerticalTextAlignment="Center" 
                                       FontSize="14" 
                                       Grid.Column="1"/>
                            </Grid>
                        </syncfusion:SfExpander.Content>
                    </syncfusion:SfExpander>
                    
                </StackLayout>
            </ScrollView>
        </core:SfGlassEffectView>
    </Grid>
</ContentPage>
```

### C# Implementation

```csharp
using Syncfusion.Maui.Core;
using Syncfusion.Maui.Expander;

public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        CreateGlassExpanderLayout();
    }
    
    private void CreateGlassExpanderLayout()
    {
        // Gradient background
        var gradientBrush = new LinearGradientBrush
        {
            StartPoint = new Point(0, 0),
            EndPoint = new Point(0, 1),
            GradientStops = new GradientStopCollection
            {
                new GradientStop { Color = Color.FromArgb("#0F4C75"), Offset = 0.0f },
                new GradientStop { Color = Color.FromArgb("#3282B8"), Offset = 0.5f },
                new GradientStop { Color = Color.FromArgb("#1B262C"), Offset = 1.0f }
            }
        };
        
        var root = new Grid { Background = gradientBrush };
        
        // Glass effect view
        var glassView = new SfGlassEffectView
        {
            CornerRadius = 20,
            EffectType = LiquidGlassEffectType.Regular,
            Margin = new Thickness(20)
        };
        
        var scroll = new ScrollView();
        var stack = new VerticalStackLayout { Spacing = 12, Padding = new Thickness(12) };
        
        // Title
        stack.Add(new Label
        {
            Text = "Invoice: #FRU037020142097",
            HeightRequest = 40,
            FontSize = 16,
            FontFamily = "Roboto-Regular",
            FontAttributes = FontAttributes.Bold,
            HorizontalOptions = LayoutOptions.Center,
            VerticalTextAlignment = TextAlignment.Center,
            Margin = new Thickness(8, 12, 8, 8)
        });
        
        // Expander 1: Invoice Date
        var expander1 = CreateGlassExpander("&#xe703;", "Invoice Date", "11:03 AM, 15 January 2019", true);
        stack.Add(expander1);
        
        // Expander 2: Items
        var expander2 = CreateGlassExpander("&#xe701;", "Item(s)", "Total: $36,220.00", false);
        stack.Add(expander2);
        
        scroll.Content = stack;
        glassView.Content = scroll;
        root.Children.Add(glassView);
        
        this.Content = root;
    }
    
    private SfExpander CreateGlassExpander(string icon, string headerText, string contentText, bool isExpanded)
    {
        var expander = new SfExpander
        {
            AnimationDuration = 200,
            IsExpanded = isExpanded,
            Background = Colors.Transparent,
            EnableLiquidGlassEffect = true,
            Margin = new Thickness(8)
        };
        
        // Header
        var headerGrid = new Grid();
        headerGrid.RowDefinitions.Add(new RowDefinition { Height = 48 });
        headerGrid.ColumnDefinitions.Add(new ColumnDefinition { Width = 35 });
        headerGrid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Star });
        
        var iconLabel = new Label
        {
            Text = icon,
            FontSize = 16,
            Margin = new Thickness(14, 2, 2, 2),
            FontFamily = "AccordionFontIcons.ttf#",
            VerticalOptions = LayoutOptions.Center,
            VerticalTextAlignment = TextAlignment.Center
        };
        headerGrid.Children.Add(iconLabel);
        
        var headerLabel = new Label
        {
            Text = headerText,
            FontSize = 14,
            CharacterSpacing = 0.25,
            FontFamily = "Roboto-Regular",
            VerticalOptions = LayoutOptions.Center
        };
        Grid.SetColumn(headerLabel, 1);
        headerGrid.Children.Add(headerLabel);
        
        expander.Header = headerGrid;
        
        // Content
        var contentGrid = new Grid { Padding = new Thickness(18, 8, 0, 18) };
        contentGrid.Children.Add(new Label
        {
            Text = contentText,
            FontSize = 14,
            CharacterSpacing = 0.25,
            FontFamily = "Roboto-Regular",
            VerticalOptions = LayoutOptions.Center
        });
        
        expander.Content = contentGrid;
        
        return expander;
    }
}
```

---

## Multiple Expanders with Glass Effect

Create an invoice layout with multiple glass-effect expanders:

```xml
<core:SfGlassEffectView EffectType="Regular" CornerRadius="20">
    <ScrollView>
        <StackLayout Spacing="8" Padding="12">
            
            <!-- Invoice Date -->
            <syncfusion:SfExpander AnimationDuration="200"
                                   IsExpanded="True"
                                   Background="Transparent"
                                   EnableLiquidGlassEffect="True">
                <syncfusion:SfExpander.Header>
                    <Grid Padding="15">
                        <Label Text="Invoice Date" FontSize="16"/>
                    </Grid>
                </syncfusion:SfExpander.Header>
                <syncfusion:SfExpander.Content>
                    <Grid Padding="15">
                        <Label Text="11:03 AM, 15 January 2019" FontSize="14"/>
                    </Grid>
                </syncfusion:SfExpander.Content>
            </syncfusion:SfExpander>
            
            <!-- Payment Details -->
            <syncfusion:SfExpander AnimationDuration="200"
                                   IsExpanded="False"
                                   Background="Transparent"
                                   EnableLiquidGlassEffect="True">
                <syncfusion:SfExpander.Header>
                    <Grid Padding="15">
                        <Label Text="Payment Details" FontSize="16"/>
                    </Grid>
                </syncfusion:SfExpander.Header>
                <syncfusion:SfExpander.Content>
                    <StackLayout Padding="15" Spacing="8">
                        <Label Text="Card Payment: $31,200.00" FontSize="14"/>
                        <Label Text="Third-Party Coupons: $5,000.00" FontSize="14"/>
                        <Label Text="Total: $36,200.00" FontSize="14" FontAttributes="Bold"/>
                    </StackLayout>
                </syncfusion:SfExpander.Content>
            </syncfusion:SfExpander>
            
            <!-- Address -->
            <syncfusion:SfExpander AnimationDuration="200"
                                   IsExpanded="True"
                                   Background="Transparent"
                                   EnableLiquidGlassEffect="True">
                <syncfusion:SfExpander.Header>
                    <Grid Padding="15">
                        <Label Text="Address" FontSize="16"/>
                    </Grid>
                </syncfusion:SfExpander.Header>
                <syncfusion:SfExpander.Content>
                    <StackLayout Padding="15" Spacing="4">
                        <Label Text="Alex" FontSize="14" FontAttributes="Bold"/>
                        <Label Text="No.8 Blossom St, Washington, DC 20019" FontSize="14"/>
                        <Label Text="(202) 547-3555" FontSize="14"/>
                    </StackLayout>
                </syncfusion:SfExpander.Content>
            </syncfusion:SfExpander>
            
        </StackLayout>
    </ScrollView>
</core:SfGlassEffectView>
```

**Key points:**
- All expanders have `EnableLiquidGlassEffect="True"`
- All expanders have `Background="Transparent"`
- Single `SfGlassEffectView` wraps all expanders for consistent effect

---

## Troubleshooting

### Issue 1: Glass Effect Not Visible

**Problem:** Expander looks normal, no glass effect.

**Solutions:**
1. ✅ Verify `EnableLiquidGlassEffect="True"` is set
2. ✅ Ensure `Background="Transparent"` is set
3. ✅ Check that expander is wrapped in `SfGlassEffectView`
4. ✅ Verify platform requirements (.NET 10+, iOS 26+, macOS 26+)

### Issue 2: Platform Not Supported

**Problem:** App crashes or effect doesn't work on Android/Windows.

**Solution:** Use conditional compilation or runtime checks:

```csharp
#if IOS || MACCATALYST
    expander.EnableLiquidGlassEffect = true;
#else
    expander.EnableLiquidGlassEffect = false;
#endif
```

Or runtime check:

```csharp
if (DeviceInfo.Platform == DevicePlatform.iOS || 
    DeviceInfo.Platform == DevicePlatform.MacCatalyst)
{
    expander.EnableLiquidGlassEffect = true;
    expander.Background = Colors.Transparent;
}
else
{
    expander.Background = Color.FromArgb("#F5F5F5");
}
```

### Issue 3: Performance Issues

**Problem:** Animations are slow or laggy with glass effect.

**Solutions:**
1. Reduce `AnimationDuration` (e.g., 150-200ms instead of 300ms+)
2. Limit number of simultaneous expanders with glass effect
3. Test on physical devices, not just emulators
4. Consider using standard styling for less powerful devices

### Issue 4: Background Not Showing Through

**Problem:** Glass effect appears solid instead of translucent.

**Solutions:**
1. ✅ Set `Background="Transparent"` on expander
2. ✅ Ensure parent container has gradient or image background
3. ✅ Check that `SfGlassEffectView` has proper `EffectType` set

---

## Best Practices

### 1. Provide Fallback Styling

Always provide fallback for unsupported platforms:

```xml
<syncfusion:SfExpander Background="{OnPlatform iOS=Transparent, 
                                               MacCatalyst=Transparent, 
                                               Default=#F5F5F5}"
                       EnableLiquidGlassEffect="{OnPlatform iOS=True, 
                                                            MacCatalyst=True, 
                                                            Default=False}">
    <!-- Content -->
</syncfusion:SfExpander>
```

### 2. Use with Gradient Backgrounds

Glass effect works best with gradient backgrounds:

```xml
<Grid.Background>
    <LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
        <GradientStop Color="#0F4C75" Offset="0.0"/>
        <GradientStop Color="#3282B8" Offset="0.5"/>
        <GradientStop Color="#1B262C" Offset="1.0"/>
    </LinearGradientBrush>
</Grid.Background>
```

### 3. Test Accessibility

Ensure text remains readable with glass effect:
- Test with system font sizes
- Verify color contrast
- Check with accessibility tools

### 4. Optimize Animation

Use shorter animation durations for smoother experience:
```xml
<syncfusion:SfExpander AnimationDuration="200" AnimationEasing="SinOut">
```

---

## Related Features

- **Getting Started:** See `getting-started.md` for basic setup
- **Appearance:** See `appearance-styling.md` for non-glass styling options
- **Animation/Events:** See `animation-events.md` for animation control

---

## Additional Resources

- [Syncfusion Liquid Glass UI Documentation](https://help.syncfusion.com/maui/liquid-glass-ui/getting-started)
- [.NET MAUI Expander Feature Page](https://www.syncfusion.com/maui-controls/maui-expander)
