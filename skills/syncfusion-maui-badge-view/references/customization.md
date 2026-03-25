# Badge Customization

Comprehensive guide to customizing the appearance and behavior of Badge View controls including fonts, colors, strokes, backgrounds, alignment, and visibility.

## Table of Contents
- [Font Customization](#font-customization)
- [Stroke Customization](#stroke-customization)
- [Text Customization](#text-customization)
- [Predefined Badge Types](#predefined-badge-types)
- [Custom Background Colors](#custom-background-colors)
- [Corner Radius](#corner-radius)
- [Badge Alignment](#badge-alignment)
- [Badge Alignment and Sizing Scenarios](#badge-alignment-and-sizing-scenarios)
- [Keeping Multiple Badges Aligned](#keeping-multiple-badges-aligned)
- [Font Auto Scaling](#font-auto-scaling)
- [Badge Visibility](#badge-visibility)
- [Troubleshooting](#troubleshooting)

## Font Customization

Customize badge text appearance using font properties in `BadgeSettings`.

### FontSize

Control the size of badge text:

**XAML:**
```xml
<badge:SfBadgeView BadgeText="48">
    <badge:SfBadgeView.Content>
        <Button Text="Primary" WidthRequest="120" HeightRequest="60"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings FontSize="15"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

**C#:**
```csharp
var badgeSettings = new BadgeSettings
{
    FontSize = 15
};

var badgeView = new SfBadgeView
{
    BadgeText = "48",
    BadgeSettings = badgeSettings
};
```

**Common font sizes:**
- Small badges: 10-12
- Default badges: 14-16
- Large badges: 18-20

### FontAttributes

Apply bold, italic, or both:

**XAML:**
```xml
<badge:SfBadgeView BadgeText="New">
    <badge:SfBadgeView.Content>
        <Button Text="Products"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings FontAttributes="Bold"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

**C#:**
```csharp
var badgeSettings = new BadgeSettings
{
    FontAttributes = FontAttributes.Bold
    // Or: FontAttributes.Italic
    // Or: FontAttributes.Bold | FontAttributes.Italic
};
```

### FontFamily

Use custom fonts:

**XAML:**
```xml
<badge:SfBadgeView BadgeText="12">
    <badge:SfBadgeView.Content>
        <Button Text="Messages"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings FontFamily="Arial" 
                            FontSize="14"
                            FontAttributes="Bold"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

**C#:**
```csharp
var badgeSettings = new BadgeSettings
{
    FontFamily = "Arial",
    FontSize = 14,
    FontAttributes = FontAttributes.Bold
};
```

**Using custom fonts:**
1. Add font file to Resources/Fonts folder
2. Register in MauiProgram.cs
3. Reference by registered name:

```csharp
.ConfigureFonts(fonts =>
{
    fonts.AddFont("CustomFont.ttf", "CustomFont");
});

// Then use:
badgeSettings.FontFamily = "CustomFont";
```

### Complete Font Example

**XAML:**
```xml
<badge:SfBadgeView BadgeText="VIP" 
                   WidthRequest="120" 
                   HeightRequest="80"  
                   HorizontalOptions="Center" 
                   VerticalOptions="Center">
    <badge:SfBadgeView.Content>
        <Button Text="Profile" 
                BackgroundColor="LightGray" 
                TextColor="Black"  
                WidthRequest="120" 
                HeightRequest="60"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings FontSize="12" 
                            FontAttributes="Bold" 
                            FontFamily="serif"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

## Stroke Customization

Add borders to badges using stroke properties.

### Stroke and StrokeThickness

**XAML:**
```xml
<badge:SfBadgeView BadgeText="30">
    <badge:SfBadgeView.Content>
        <Button Text="Cart" WidthRequest="120" HeightRequest="60"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings Stroke="Orange" 
                            StrokeThickness="2"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

**C#:**
```csharp
var badgeSettings = new BadgeSettings
{
    Stroke = Colors.Orange,
    StrokeThickness = 2
};

var badgeView = new SfBadgeView
{
    BadgeText = "30",
    BadgeSettings = badgeSettings
};
```

**Common stroke patterns:**
- Thin border: `StrokeThickness="1"`
- Medium border: `StrokeThickness="2"`
- Thick border: `StrokeThickness="3"`
- No border: `StrokeThickness="0"` (default)

### Stroke with Custom Colors

```csharp
var badgeSettings = new BadgeSettings
{
    Type = BadgeType.None, // Use custom colors
    Background = new SolidColorBrush(Colors.White),
    TextColor = Colors.Black,
    Stroke = Colors.DarkGray,
    StrokeThickness = 2
};
```

## Text Customization

Customize text appearance with color and padding.

### TextColor

**XAML:**
```xml
<badge:SfBadgeView BadgeText="45">
    <badge:SfBadgeView.Content>
        <Button Text="Notifications"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings TextColor="Yellow" 
                            Type="Primary"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

**C#:**
```csharp
var badgeSettings = new BadgeSettings
{
    TextColor = Colors.Yellow,
    Type = BadgeType.Primary
};
```

### TextPadding

Add spacing around badge text:

**XAML:**
```xml
<badge:SfBadgeView BadgeText="99+">
    <badge:SfBadgeView.Content>
        <Button Text="Messages"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings TextPadding="10" 
                            Type="Error"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

**C#:**
```csharp
var badgeSettings = new BadgeSettings
{
    TextPadding = 10,
    Type = BadgeType.Error
};
```

**TextPadding guidelines:**
- Single digit numbers: 5-8
- Double digit numbers: 8-12
- Text labels: 10-15

## Predefined Badge Types

Use predefined color schemes for consistent styling.

### Available Badge Types

The `Type` property supports these values:

| Type | Description | Common Use Case |
|------|-------------|-----------------|
| `Primary` | Primary brand color | General notifications |
| `Secondary` | Secondary brand color | Alternative notifications |
| `Success` | Green | Completed actions, positive status |
| `Error` | Red | Errors, urgent notifications |
| `Warning` | Orange/Yellow | Warnings, important alerts |
| `Info` | Blue | Informational badges |
| `Light` | Light gray/white | Subtle notifications |
| `Dark` | Dark gray/black | High contrast badges |

### Using Predefined Types

**XAML:**
```xml
<!-- Error Type (Red) -->
<badge:SfBadgeView BadgeText="8">
    <badge:SfBadgeView.Content>
        <Image Source="icon.png" WidthRequest="70" HeightRequest="70"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings Type="Error"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>

<!-- Success Type (Green) -->
<badge:SfBadgeView BadgeText="✓">
    <badge:SfBadgeView.Content>
        <Button Text="Complete"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings Type="Success"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>

<!-- Warning Type (Orange) -->
<badge:SfBadgeView BadgeText="!">
    <badge:SfBadgeView.Content>
        <Button Text="Alerts"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings Type="Warning"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

**C#:**
```csharp
// Error badge
var errorBadge = new BadgeSettings { Type = BadgeType.Error };

// Success badge
var successBadge = new BadgeSettings { Type = BadgeType.Success };

// Warning badge
var warningBadge = new BadgeSettings { Type = BadgeType.Warning };

// Info badge
var infoBadge = new BadgeSettings { Type = BadgeType.Info };
```

## Custom Background Colors

Create custom badge colors by setting `Type="None"` and using the `Background` property.

**XAML:**
```xml
<badge:SfBadgeView BadgeText="48">
    <badge:SfBadgeView.Content>
        <Button Text="Custom Badge"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings Type="None" 
                            Background="Green"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

**C#:**
```csharp
var badgeSettings = new BadgeSettings
{
    Type = BadgeType.None,
    Background = new SolidColorBrush(Colors.Green)
};
```

### Custom Color Examples

**Purple VIP Badge:**
```csharp
var vipBadge = new BadgeSettings
{
    Type = BadgeType.None,
    Background = new SolidColorBrush(Color.FromRgb(128, 0, 128)),
    TextColor = Colors.White,
    FontAttributes = FontAttributes.Bold
};
```

**Gradient Background (using SolidColorBrush):**
```csharp
var gradientBadge = new BadgeSettings
{
    Type = BadgeType.None,
    Background = new SolidColorBrush(Color.FromRgb(255, 100, 50)),
    TextColor = Colors.White
};
```

## Corner Radius

Control badge shape with the `CornerRadius` property.

**XAML:**
```xml
<badge:SfBadgeView BadgeText="100">
    <badge:SfBadgeView.Content>
        <Button Text="Square Badge"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings CornerRadius="5"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

**C#:**
```csharp
var badgeSettings = new BadgeSettings
{
    CornerRadius = new CornerRadius(5, 5, 5, 5)
};
```

**Common corner radius values:**
- Fully rounded (default): 10-15
- Slightly rounded: 5-8
- Square: 0
- Custom per corner: `new CornerRadius(topLeft, topRight, bottomLeft, bottomRight)`

## Badge Alignment

Position badge relative to content bounds using `BadgeAlignment`.

### Alignment Options

- `Start`: Left side
- `Center`: Center
- `End`: Right side (default when combined with top/bottom positions)

**XAML:**
```xml
<badge:SfBadgeView BadgeText="20">
    <badge:SfBadgeView.Content>
        <Label Text="CENTER" 
               BackgroundColor="LightGray" 
               HorizontalTextAlignment="Center" 
               VerticalTextAlignment="Center"
               WidthRequest="100" 
               HeightRequest="60"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings BadgeAlignment="Center" 
                            CornerRadius="0"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

**C#:**
```csharp
var badgeSettings = new BadgeSettings
{
    BadgeAlignment = BadgeAlignment.Center,
    CornerRadius = 0
};
```

## Badge Alignment and Sizing Scenarios

Badge positioning depends on how SfBadgeView and its Content are sized.

### Scenario 1: Fixed Size on SfBadgeView

Badge aligns relative to the SfBadgeView's fixed dimensions:

**XAML:**
```xml
<badge:SfBadgeView BadgeText="20"  
                   WidthRequest="100" 
                   HeightRequest="100">
    <badge:SfBadgeView.Content>
        <Label Text="Start" 
               BackgroundColor="LightGray" 
               HorizontalTextAlignment="Center" 
               VerticalTextAlignment="Center"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings BadgeAlignment="Start"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

**Use when:** You need badge at edge of specific area, regardless of content size.

### Scenario 2: Fixed Size on Content

SfBadgeView wraps content; badge aligns to content bounds:

**XAML:**
```xml
<badge:SfBadgeView BadgeText="20">
    <badge:SfBadgeView.Content>
        <Label Text="Start" 
               Background="LightGray" 
               HeightRequest="100" 
               WidthRequest="100" 
               HorizontalTextAlignment="Center" 
               VerticalTextAlignment="Center"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings BadgeAlignment="Start"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

**Use when:** Badge should align to specific control (button, image, etc.).

### Scenario 3: Automatic Sizing

Both SfBadgeView and Content size automatically:

**XAML:**
```xml
<badge:SfBadgeView BadgeText="20">
    <badge:SfBadgeView.Content>
        <Label Text="Start" 
               Background="LightGray" 
               HorizontalTextAlignment="Center" 
               VerticalTextAlignment="Center"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings BadgeAlignment="Start"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

**Use when:** Content size is dynamic or unknown.

## Keeping Multiple Badges Aligned

Use `AutoHide` to maintain consistent alignment when some badges are hidden.

**XAML:**
```xml
<HorizontalStackLayout Spacing="20" 
                       HorizontalOptions="Center" 
                       VerticalOptions="Center">
    
    <!-- Badge with count 0 - will be hidden -->
    <badge:SfBadgeView BadgeText="0">
        <badge:SfBadgeView.Content>
            <Frame WidthRequest="50" 
                   HeightRequest="50" 
                   CornerRadius="25"
                   Padding="0">
                <Image Source="avatar1.png"/>
            </Frame>
        </badge:SfBadgeView.Content>
        <badge:SfBadgeView.BadgeSettings>
            <badge:BadgeSettings AutoHide="True" 
                                Type="None" 
                                Background="Red"/>
        </badge:SfBadgeView.BadgeSettings>
    </badge:SfBadgeView>

    <!-- Badge with count 10 - will be visible -->
    <badge:SfBadgeView BadgeText="10">
        <badge:SfBadgeView.Content>
            <Frame WidthRequest="50" 
                   HeightRequest="50" 
                   CornerRadius="25"
                   Padding="0">
                <Image Source="avatar2.png"/>
            </Frame>
        </badge:SfBadgeView.Content>
        <badge:SfBadgeView.BadgeSettings>
            <badge:BadgeSettings AutoHide="True" 
                                Type="None" 
                                Background="Red"/>
        </badge:SfBadgeView.BadgeSettings>
    </badge:SfBadgeView>
    
</HorizontalStackLayout>
```

**C#:**
```csharp
var container = new HorizontalStackLayout
{
    Spacing = 20,
    HorizontalOptions = LayoutOptions.Center,
    VerticalOptions = LayoutOptions.Center
};

// Avatar 1 with zero count (hidden)
var badge1 = new SfBadgeView
{
    BadgeText = "0",
    Content = CreateAvatar("avatar1.png"),
    BadgeSettings = new BadgeSettings
    {
        AutoHide = true,
        Type = BadgeType.None,
        Background = new SolidColorBrush(Colors.Red)
    }
};

// Avatar 2 with count (visible)
var badge2 = new SfBadgeView
{
    BadgeText = "10",
    Content = CreateAvatar("avatar2.png"),
    BadgeSettings = new BadgeSettings
    {
        AutoHide = true,
        Type = BadgeType.None,
        Background = new SolidColorBrush(Colors.Red)
    }
};

container.Children.Add(badge1);
container.Children.Add(badge2);
```

**AutoHide behavior:**
- When `BadgeText = "0"` and `AutoHide = true`, badge is not rendered
- Content area remains uniformly aligned
- Useful for lists of items where some have notifications and others don't

## Font Auto Scaling

Enable automatic font size scaling based on OS text size settings.

**XAML:**
```xml
<badge:SfBadgeView BadgeText="15">
    <badge:SfBadgeView.Content>
        <Button Text="Accessible Badge"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings FontAutoScalingEnabled="True"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

**C#:**
```csharp
var badgeSettings = new BadgeSettings
{
    FontAutoScalingEnabled = true
};
```

**When to enable:**
- Accessibility-focused apps
- Apps targeting users with vision impairments
- Apps requiring WCAG compliance

**Default:** `false`

## Badge Visibility

Control badge visibility with the `IsVisible` property.

**XAML:**
```xml
<badge:SfBadgeView BadgeText="20">
    <badge:SfBadgeView.Content>
        <Button Text="Messages"/>
    </badge:SfBadgeView.Content>
    <badge:SfBadgeView.BadgeSettings>
        <badge:BadgeSettings IsVisible="True"/>
    </badge:SfBadgeView.BadgeSettings>
</badge:SfBadgeView>
```

**C#:**
```csharp
var badgeSettings = new BadgeSettings
{
    IsVisible = true
};

// Toggle visibility
badgeSettings.IsVisible = false; // Hide badge
badgeSettings.IsVisible = true;  // Show badge
```

**Dynamic visibility example:**
```csharp
public class NotificationViewModel
{
    private BadgeSettings _badgeSettings;
    
    public void UpdateNotificationCount(int count)
    {
        if (count > 0)
        {
            BadgeText = count.ToString();
            _badgeSettings.IsVisible = true;
        }
        else
        {
            _badgeSettings.IsVisible = false;
        }
    }
}
```

## Troubleshooting

### Custom Colors Not Showing

**Problem:** Custom background color doesn't appear.

**Solution:** Ensure `Type="None"`:
```xml
<badge:BadgeSettings Type="None" Background="Purple"/>
```

### Text Not Visible on Custom Background

**Problem:** Text invisible on custom colored badge.

**Solution:** Set contrasting `TextColor`:
```csharp
new BadgeSettings
{
    Type = BadgeType.None,
    Background = new SolidColorBrush(Colors.DarkBlue),
    TextColor = Colors.White // Ensure contrast
};
```

### Font Not Applying

**Problem:** Custom font family not working.

**Solutions:**
1. Register font in MauiProgram.cs
2. Use exact registered name
3. Verify font file is in Resources/Fonts
4. Check font file Build Action is set to "MauiFont"

### Badge Alignment Issues

**Problem:** Badge not aligning as expected.

**Solutions:**
1. Set explicit sizes on either SfBadgeView or Content
2. Use `Offset` property for fine-tuning
3. Consider which sizing scenario applies
4. Check HorizontalOptions and VerticalOptions

### AutoHide Not Working

**Problem:** Badge with "0" text still visible when AutoHide is true.

**Solutions:**
1. Ensure BadgeText is exactly "0" (string)
2. Verify AutoHide property is set to true
3. Check that BadgeText is being updated properly
