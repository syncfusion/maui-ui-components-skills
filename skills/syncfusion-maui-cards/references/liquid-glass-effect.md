# Liquid Glass Effect

## Overview

The Liquid Glass Effect introduces a modern, translucent design with adaptive color tinting and light refraction, creating a sleek, glass-like user experience. This effect provides a contemporary aesthetic that remains clear and accessible while adding depth to card interfaces.

**Key Features:**
- Modern translucent glass appearance
- Adaptive color tinting
- Light refraction simulation
- Maintains content clarity and accessibility
- Platform-specific implementation

**Platform Requirements:**
- **.NET 10** or higher
- **macOS 26+** or **iOS 26+**
- Not supported on Android or Windows

## EnableLiquidGlassEffect Property

The `EnableLiquidGlassEffect` property controls whether the liquid glass effect is applied to the card view.

**Type:** `bool`  
**Default:** `false`

## Basic Implementation

### Step 1: Enable the Effect

Set the `EnableLiquidGlassEffect` property to `true`:

**XAML:**
```xml
<cards:SfCardView EnableLiquidGlassEffect="True">
    <Label Text="Glass Effect Card"/>
</cards:SfCardView>
```

**C#:**
```csharp
var card = new SfCardView
{
    EnableLiquidGlassEffect = true,
    Content = new Label { Text = "Glass Effect Card" }
};
```

### Step 2: Configure Background

For optimal glass effect, set the background to transparent:

**XAML:**
```xml
<cards:SfCardView EnableLiquidGlassEffect="True"
                  Background="Transparent"
                  BorderColor="Transparent">
    <Label Text="Glass Effect Card"/>
</cards:SfCardView>
```

**C#:**
```csharp
var card = new SfCardView
{
    EnableLiquidGlassEffect = true,
    Background = Brushes.Transparent,
    BorderColor = Colors.Transparent,
    Content = new Label { Text = "Glass Effect Card" }
};
```

**Important:** Setting the background to transparent ensures the glass effect is properly visible and blends with the background.

## Complete Example

### Example 1: Basic Glass Card

**XAML:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:cards="clr-namespace:Syncfusion.Maui.Cards;assembly=Syncfusion.Maui.Cards"
             x:Class="MyApp.GlassCardPage">
    
    <Grid>
        <!-- Gradient background for glass effect -->
        <Grid.Background>
            <LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
                <GradientStop Color="#0F4C75" Offset="0.0"/>
                <GradientStop Color="#3282B8" Offset="0.5"/>
                <GradientStop Color="#1B262C" Offset="1.0"/>
            </LinearGradientBrush>
        </Grid.Background>
        
        <!-- Glass card -->
        <cards:SfCardView EnableLiquidGlassEffect="True"
                         Background="Transparent"
                         BorderColor="Transparent"
                         WidthRequest="350"
                         HeightRequest="200"
                         HorizontalOptions="Center"
                         VerticalOptions="Center">
            <Label Text="Liquid Glass Card"
                   HorizontalOptions="Center"
                   VerticalOptions="Center"
                   FontSize="24"
                   TextColor="White"/>
        </cards:SfCardView>
    </Grid>
</ContentPage>
```

**C#:**
```csharp
// Outer grid with gradient background
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

// Glass card
var card = new SfCardView
{
    EnableLiquidGlassEffect = true,
    Background = Brushes.Transparent,
    BorderColor = Colors.Transparent,
    WidthRequest = 350,
    HeightRequest = 200,
    HorizontalOptions = LayoutOptions.Center,
    VerticalOptions = LayoutOptions.Center,
    Content = new Label
    {
        Text = "Liquid Glass Card",
        HorizontalOptions = LayoutOptions.Center,
        VerticalOptions = LayoutOptions.Center,
        FontSize = 24,
        TextColor = Colors.White
    }
};

mainGrid.Add(card);
this.Content = mainGrid;
```

### Example 2: Glass Card with Content

**XAML:**
```xml
<Grid>
    <Grid.Background>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="#667eea" Offset="0.0"/>
            <GradientStop Color="#764ba2" Offset="1.0"/>
        </LinearGradientBrush>
    </Grid.Background>
    
    <cards:SfCardView EnableLiquidGlassEffect="True"
                     Background="Transparent"
                     BorderColor="Transparent"
                     WidthRequest="320"
                     HeightRequest="400"
                     CornerRadius="20"
                     HorizontalOptions="Center"
                     VerticalOptions="Center">
        <VerticalStackLayout Padding="30" Spacing="15">
            <Label Text="Premium Features"
                   FontSize="28"
                   FontAttributes="Bold"
                   TextColor="White"
                   HorizontalTextAlignment="Center"/>
            
            <BoxView HeightRequest="2" 
                     BackgroundColor="White" 
                     Opacity="0.3"
                     Margin="0,10"/>
            
            <Label Text="✓ Unlimited access"
                   FontSize="16"
                   TextColor="White"/>
            
            <Label Text="✓ Priority support"
                   FontSize="16"
                   TextColor="White"/>
            
            <Label Text="✓ Advanced features"
                   FontSize="16"
                   TextColor="White"/>
            
            <Button Text="Upgrade Now"
                    BackgroundColor="White"
                    TextColor="#667eea"
                    CornerRadius="25"
                    Margin="0,20,0,0"/>
        </VerticalStackLayout>
    </cards:SfCardView>
</Grid>
```

### Example 3: Multiple Glass Cards

**XAML:**
```xml
<Grid Padding="20">
    <Grid.Background>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="#4158D0" Offset="0.0"/>
            <GradientStop Color="#C850C0" Offset="0.5"/>
            <GradientStop Color="#FFCC70" Offset="1.0"/>
        </LinearGradientBrush>
    </Grid.Background>
    
    <VerticalStackLayout Spacing="20" VerticalOptions="Center">
        
        <!-- Card 1 -->
        <cards:SfCardView EnableLiquidGlassEffect="True"
                         Background="Transparent"
                         BorderColor="Transparent"
                         CornerRadius="15"
                         HeightRequest="120">
            <Grid Padding="20">
                <Label Text="Dashboard"
                       FontSize="20"
                       FontAttributes="Bold"
                       TextColor="White"
                       VerticalOptions="Center"/>
                <Label Text="→"
                       FontSize="30"
                       TextColor="White"
                       HorizontalOptions="End"
                       VerticalOptions="Center"/>
            </Grid>
        </cards:SfCardView>
        
        <!-- Card 2 -->
        <cards:SfCardView EnableLiquidGlassEffect="True"
                         Background="Transparent"
                         BorderColor="Transparent"
                         CornerRadius="15"
                         HeightRequest="120">
            <Grid Padding="20">
                <Label Text="Settings"
                       FontSize="20"
                       FontAttributes="Bold"
                       TextColor="White"
                       VerticalOptions="Center"/>
                <Label Text="→"
                       FontSize="30"
                       TextColor="White"
                       HorizontalOptions="End"
                       VerticalOptions="Center"/>
            </Grid>
        </cards:SfCardView>
        
        <!-- Card 3 -->
        <cards:SfCardView EnableLiquidGlassEffect="True"
                         Background="Transparent"
                         BorderColor="Transparent"
                         CornerRadius="15"
                         HeightRequest="120">
            <Grid Padding="20">
                <Label Text="Profile"
                       FontSize="20"
                       FontAttributes="Bold"
                       TextColor="White"
                       VerticalOptions="Center"/>
                <Label Text="→"
                       FontSize="30"
                       TextColor="White"
                       HorizontalOptions="End"
                       VerticalOptions="Center"/>
            </Grid>
        </cards:SfCardView>
        
    </VerticalStackLayout>
</Grid>
```

## Best Background Styles for Glass Effect

### Gradient Backgrounds

**Cool Blue Gradient:**
```xml
<LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
    <GradientStop Color="#0F4C75" Offset="0.0"/>
    <GradientStop Color="#3282B8" Offset="0.5"/>
    <GradientStop Color="#1B262C" Offset="1.0"/>
</LinearGradientBrush>
```

**Purple Pink Gradient:**
```xml
<LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
    <GradientStop Color="#667eea" Offset="0.0"/>
    <GradientStop Color="#764ba2" Offset="1.0"/>
</LinearGradientBrush>
```

**Sunset Gradient:**
```xml
<LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
    <GradientStop Color="#fa709a" Offset="0.0"/>
    <GradientStop Color="#fee140" Offset="1.0"/>
</LinearGradientBrush>
```

**Ocean Gradient:**
```xml
<LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
    <GradientStop Color="#2E3192" Offset="0.0"/>
    <GradientStop Color="#1BFFFF" Offset="1.0"/>
</LinearGradientBrush>
```

### Image Backgrounds

```xml
<Grid>
    <Image Source="background.jpg" Aspect="AspectFill"/>
    
    <!-- Glass card on top -->
    <cards:SfCardView EnableLiquidGlassEffect="True"
                     Background="Transparent"
                     BorderColor="Transparent">
        <!-- Content -->
    </cards:SfCardView>
</Grid>
```

## Design Tips

### 1. Use Rounded Corners

Rounded corners enhance the modern glass effect:

```xml
<cards:SfCardView EnableLiquidGlassEffect="True"
                 CornerRadius="20"
                 Background="Transparent">
    <!-- Content -->
</cards:SfCardView>
```

### 2. Maintain Content Contrast

Ensure text is readable with proper color contrast:

```xml
<Label Text="Readable Text"
       TextColor="White"
       FontAttributes="Bold"/>
```

### 3. Combine with Spacing

Add padding and margin for breathing room:

```xml
<cards:SfCardView EnableLiquidGlassEffect="True"
                 Background="Transparent"
                 Margin="20"
                 Padding="25">
    <!-- Content -->
</cards:SfCardView>
```

### 4. Layer Multiple Glass Elements

Create depth with multiple glass cards:

```xml
<Grid>
    <!-- Background card -->
    <cards:SfCardView EnableLiquidGlassEffect="True"
                     Background="Transparent"
                     WidthRequest="350"
                     HeightRequest="450"/>
    
    <!-- Foreground card -->
    <cards:SfCardView EnableLiquidGlassEffect="True"
                     Background="Transparent"
                     WidthRequest="320"
                     HeightRequest="420"
                     TranslationY="10"/>
</Grid>
```

## Platform Compatibility

### Checking Platform Support

```csharp
public bool IsLiquidGlassSupported()
{
#if IOS || MACCATALYST
    if (DeviceInfo.Platform == DevicePlatform.iOS)
    {
        // Check iOS version (requires 26+)
        return DeviceInfo.Version.Major >= 26;
    }
    if (DeviceInfo.Platform == DevicePlatform.macOS)
    {
        // Check macOS version (requires 26+)
        return DeviceInfo.Version.Major >= 26;
    }
#endif
    return false;
}
```

### Conditional Usage

```csharp
var card = new SfCardView
{
    EnableLiquidGlassEffect = IsLiquidGlassSupported(),
    Background = Brushes.Transparent,
    Content = new Label { Text = "Adaptive Card" }
};

// Fallback styling for unsupported platforms
if (!IsLiquidGlassSupported())
{
    card.BackgroundColor = Colors.White.WithAlpha(0.9f);
    card.Shadow = new Shadow
    {
        Brush = Brushes.Black,
        Opacity = 0.3f,
        Radius = 10
    };
}
```

## Troubleshooting

### Issue: Glass Effect Not Visible

**Solution 1:** Ensure background is set to transparent:
```csharp
card.Background = Brushes.Transparent;
card.BorderColor = Colors.Transparent;
```

**Solution 2:** Verify platform requirements:
- .NET 10 or higher
- macOS 26+ or iOS 26+

**Solution 3:** Check parent background has content:
```xml
<!-- Glass needs something to show through -->
<Grid>
    <Grid.Background>
        <LinearGradientBrush>
            <!-- Gradient stops -->
        </LinearGradientBrush>
    </Grid.Background>
    <cards:SfCardView EnableLiquidGlassEffect="True"/>
</Grid>
```

### Issue: Content Not Readable

**Solution:** Adjust text colors and add contrast:
```csharp
new Label
{
    Text = "Content",
    TextColor = Colors.White,  // High contrast
    FontAttributes = FontAttributes.Bold  // Better visibility
}
```

## Best Practices

1. **Use on supported platforms only** - Check platform version before enabling
2. **Provide fallback styling** - For unsupported platforms
3. **Use transparent backgrounds** - For proper glass effect
4. **Ensure content contrast** - White or bold text works best
5. **Combine with gradients** - Enhances the glass appearance
6. **Use appropriate corner radius** - 15-25 for modern look
7. **Test on actual devices** - Simulator may not accurately show effect
