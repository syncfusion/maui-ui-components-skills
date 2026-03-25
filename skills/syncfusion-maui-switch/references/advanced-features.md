# Advanced Features in .NET MAUI Switch

## Table of Contents
- [Liquid Glass Support](#liquid-glass-support)
  - [Overview](#overview)
  - [Enabling Liquid Glass Effect](#enabling-liquid-glass-effect)
  - [Best Practices for Liquid Glass](#best-practices-for-liquid-glass)
  - [Behavior and Tips](#behavior-and-tips)
  - [Design Recommendations](#design-recommendations)
- [Right-to-Left (RTL) Support](#right-to-left-rtl-support)
  - [Overview](#overview-1)
  - [Enabling RTL](#enabling-rtl)
  - [Detecting Device Language](#detecting-device-language)
  - [Conditional RTL Application](#conditional-rtl-application)
  - [Dynamic Language Switching](#dynamic-language-switching)
  - [Best Practices for RTL](#best-practices-for-rtl)
  - [Common Scenarios](#common-scenarios)
- [Combining Advanced Features](#combining-advanced-features)
- [Troubleshooting](#troubleshooting)

This guide covers advanced features of the Syncfusion .NET MAUI Switch (SfSwitch) including liquid glass effects and right-to-left (RTL) layout support.

## Liquid Glass Support

The SfSwitch control supports a glass morphism effect (also called acrylic or liquid glass) when you enable the `EnableLiquidGlassEffect` property. This creates a premium, modern UI with a translucent, glass-like appearance that works beautifully over vibrant backgrounds.

### Overview

**What is Liquid Glass Effect:**
- Modern glass morphism design trend
- Translucent, frosted glass appearance
- Enhances visual depth of UI
- Smooth transitions and clear visual feedback
- Premium, polished look and feel

**Platform Requirements:**
- **.NET 10** or later
- **iOS 26** or **macOS 26**
- Not supported on earlier versions

### Enabling Liquid Glass Effect

**XAML:**
```xml
<Grid>
    <!-- Background image to showcase the glass effect -->
    <Image Source="wallpaper.jpg" Aspect="AspectFill" />
    
    <buttons:SfSwitch EnableLiquidGlassEffect="True" />
</Grid>
```

**C#:**
```csharp
SfSwitch sfSwitch = new SfSwitch
{
    EnableLiquidGlassEffect = true
};
```

### Best Practices for Liquid Glass

#### 1. Use Vibrant Backgrounds

The glass effect is most visible and effective over colorful or image backgrounds.

**Good Examples:**
```xml
<!-- Over gradient background -->
<Grid>
    <BoxView>
        <BoxView.Background>
            <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
                <GradientStop Color="#667eea" Offset="0.0"/>
                <GradientStop Color="#764ba2" Offset="1.0"/>
            </LinearGradientBrush>
        </BoxView.Background>
    </BoxView>
    <buttons:SfSwitch EnableLiquidGlassEffect="True"/>
</Grid>

<!-- Over image -->
<Grid>
    <Image Source="vibrant_wallpaper.jpg" Aspect="AspectFill"/>
    <buttons:SfSwitch EnableLiquidGlassEffect="True"/>
</Grid>

<!-- Over colorful layout -->
<Grid BackgroundColor="#FF6B6B">
    <buttons:SfSwitch EnableLiquidGlassEffect="True"/>
</Grid>
```

**Less Effective:**
```xml
<!-- Plain white background - glass effect won't be noticeable -->
<Grid BackgroundColor="White">
    <buttons:SfSwitch EnableLiquidGlassEffect="True"/>
</Grid>
```

#### 2. Combine with Visual State Manager

Apply glass effect with custom styling:

```xml
<Grid>
    <Image Source="background.jpg" Aspect="AspectFill"/>
    
    <buttons:SfSwitch EnableLiquidGlassEffect="True" IsOn="True">
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup x:Name="CommonStates">
                <VisualState x:Name="On">
                    <VisualState.Setters>
                        <Setter Property="SwitchSettings">
                            <Setter.Value>
                                <buttons:SwitchSettings
                                    TrackBackground="#80FFFFFF"
                                    ThumbBackground="#FFFFFF"
                                    TrackHeightRequest="40"
                                    TrackWidthRequest="80"/>
                            </Setter.Value>
                        </Setter>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>
    </buttons:SfSwitch>
</Grid>
```

**Note:** Use semi-transparent colors (alpha channel) to enhance the glass effect.

#### 3. Complete Example

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:buttons="clr-namespace:Syncfusion.Maui.Buttons;assembly=Syncfusion.Maui.Buttons">

    <Grid>
        <!-- Vibrant gradient background -->
        <BoxView>
            <BoxView.Background>
                <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
                    <GradientStop Color="#f093fb" Offset="0.0"/>
                    <GradientStop Color="#f5576c" Offset="1.0"/>
                </LinearGradientBrush>
            </BoxView.Background>
        </BoxView>
        
        <!-- Settings panel with glass switches -->
        <VerticalStackLayout Padding="40" Spacing="30" VerticalOptions="Center">
            <Label Text="Settings" 
                   FontSize="32" 
                   FontAttributes="Bold"
                   TextColor="White"/>
            
            <HorizontalStackLayout Spacing="20">
                <Label Text="Notifications" 
                       VerticalOptions="Center"
                       TextColor="White"
                       FontSize="18"/>
                <buttons:SfSwitch EnableLiquidGlassEffect="True" 
                                  IsOn="True"/>
            </HorizontalStackLayout>
            
            <HorizontalStackLayout Spacing="20">
                <Label Text="Dark Mode" 
                       VerticalOptions="Center"
                       TextColor="White"
                       FontSize="18"/>
                <buttons:SfSwitch EnableLiquidGlassEffect="True" 
                                  IsOn="False"/>
            </HorizontalStackLayout>
            
            <HorizontalStackLayout Spacing="20">
                <Label Text="Location" 
                       VerticalOptions="Center"
                       TextColor="White"
                       FontSize="18"/>
                <buttons:SfSwitch EnableLiquidGlassEffect="True" 
                                  IsOn="True"/>
            </HorizontalStackLayout>
        </VerticalStackLayout>
    </Grid>
</ContentPage>
```

### Behavior and Tips

#### Glass Effect Behavior
- Applied at render time and during user interaction
- Creates frosted glass appearance with blur
- Enhances depth perception in the UI
- Smooth transitions when toggling

#### Performance Considerations
- Glass effects can be more resource-intensive
- Test on target devices to ensure smooth performance
- Use moderately detailed backgrounds for best balance
- May impact battery life on mobile devices

#### Visual Clarity
- Ensure switch remains clearly visible over background
- Test with various background images/colors
- Adjust switch colors if needed for visibility
- Consider adding subtle shadows or borders

#### Platform Detection
```csharp
// Check if glass effect is supported
public bool IsGlassEffectSupported()
{
    #if IOS || MACCATALYST
        return DeviceInfo.Version.Major >= 26;
    #else
        return false;
    #endif
}

// Conditionally enable glass effect
if (IsGlassEffectSupported())
{
    sfSwitch.EnableLiquidGlassEffect = true;
}
```

### Design Recommendations

#### Modern Card Design
```xml
<Frame Padding="20" 
       CornerRadius="20"
       HasShadow="True"
       BackgroundColor="#80FFFFFF">
    <VerticalStackLayout Spacing="15">
        <Label Text="Premium Features" FontSize="20"/>
        <buttons:SfSwitch EnableLiquidGlassEffect="True"/>
    </VerticalStackLayout>
</Frame>
```

#### Hero Section
```xml
<Grid>
    <Image Source="hero_background.jpg" Aspect="AspectFill"/>
    <VerticalStackLayout Padding="50" VerticalOptions="Center">
        <Label Text="Enable Notifications" 
               FontSize="28" 
               TextColor="White"/>
        <buttons:SfSwitch EnableLiquidGlassEffect="True" 
                          IsOn="True"
                          HorizontalOptions="Center"/>
    </VerticalStackLayout>
</Grid>
```

## Right-to-Left (RTL) Support

The SfSwitch supports right-to-left layouts for languages like Arabic, Hebrew, Persian, and Urdu. This flips the switch direction to match the reading direction of RTL languages.

### Overview

**What Changes in RTL:**
- Thumb position flips (On = left, Off = right)
- Animation direction reverses
- Layout mirrors for consistency with RTL UI
- All other functionality remains the same

### Enabling RTL

#### Method 1: FlowDirection Property

**XAML:**
```xml
<buttons:SfSwitch FlowDirection="RightToLeft" />
```

**C#:**
```csharp
SfSwitch sfSwitch = new SfSwitch();
sfSwitch.FlowDirection = FlowDirection.RightToLeft;
this.Content = sfSwitch;
```

#### Method 2: Device Language

RTL automatically activates when the device language is set to an RTL language (Arabic, Hebrew, etc.).

**No code required** - the switch automatically adapts to the device's language settings.

### Complete RTL Example

**XAML:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:buttons="clr-namespace:Syncfusion.Maui.Buttons;assembly=Syncfusion.Maui.Buttons">

    <VerticalStackLayout Padding="30" Spacing="20" FlowDirection="RightToLeft">
        <Label Text="إعدادات" FontSize="24"/>  <!-- Settings in Arabic -->
        
        <HorizontalStackLayout Spacing="15">
            <buttons:SfSwitch FlowDirection="RightToLeft" IsOn="True"/>
            <Label Text="الإشعارات" VerticalOptions="Center"/>  <!-- Notifications -->
        </HorizontalStackLayout>
        
        <HorizontalStackLayout Spacing="15">
            <buttons:SfSwitch FlowDirection="RightToLeft" IsOn="False"/>
            <Label Text="الوضع الداكن" VerticalOptions="Center"/>  <!-- Dark Mode -->
        </HorizontalStackLayout>
    </VerticalStackLayout>
</ContentPage>
```

**C# Implementation:**
```csharp
public partial class RTLSettingsPage : ContentPage
{
    public RTLSettingsPage()
    {
        InitializeComponent();
        
        // Set RTL for entire page
        this.FlowDirection = FlowDirection.RightToLeft;
        
        // Create switches with RTL
        SfSwitch notificationSwitch = new SfSwitch
        {
            FlowDirection = FlowDirection.RightToLeft,
            IsOn = true
        };
        
        SfSwitch darkModeSwitch = new SfSwitch
        {
            FlowDirection = FlowDirection.RightToLeft,
            IsOn = false
        };
        
        // Build UI...
    }
}
```

### Detecting Device Language

```csharp
public FlowDirection GetFlowDirection()
{
    var currentCulture = CultureInfo.CurrentUICulture;
    
    // Check if current culture is RTL
    if (currentCulture.TextInfo.IsRightToLeft)
    {
        return FlowDirection.RightToLeft;
    }
    
    return FlowDirection.LeftToRight;
}

// Apply to switch
sfSwitch.FlowDirection = GetFlowDirection();
```

### Conditional RTL Application

```csharp
public class LocalizationService
{
    private static readonly string[] RTLLanguages = 
    {
        "ar",   // Arabic
        "he",   // Hebrew
        "fa",   // Persian
        "ur",   // Urdu
        "yi"    // Yiddish
    };
    
    public static bool IsRTLLanguage(string languageCode)
    {
        return RTLLanguages.Contains(languageCode.ToLower());
    }
    
    public static FlowDirection GetFlowDirection()
    {
        var language = CultureInfo.CurrentUICulture.TwoLetterISOLanguageName;
        return IsRTLLanguage(language) 
            ? FlowDirection.RightToLeft 
            : FlowDirection.LeftToRight;
    }
}

// Usage
mySwitch.FlowDirection = LocalizationService.GetFlowDirection();
```

### Dynamic Language Switching

```csharp
public class LanguageSwitcher
{
    public void ChangeLanguage(string languageCode)
    {
        // Set culture
        var culture = new CultureInfo(languageCode);
        CultureInfo.CurrentCulture = culture;
        CultureInfo.CurrentUICulture = culture;
        
        // Update flow direction
        var flowDirection = LocalizationService.GetFlowDirection();
        Application.Current.MainPage.FlowDirection = flowDirection;
        
        // Notify all switches to update
        MessagingCenter.Send(this, "LanguageChanged", flowDirection);
    }
}

// In your page
MessagingCenter.Subscribe<LanguageSwitcher, FlowDirection>(
    this, 
    "LanguageChanged", 
    (sender, flowDirection) =>
    {
        notificationSwitch.FlowDirection = flowDirection;
        darkModeSwitch.FlowDirection = flowDirection;
    });
```

### Best Practices for RTL

#### 1. Test with RTL Languages
```csharp
// Set device to Arabic for testing
CultureInfo.CurrentUICulture = new CultureInfo("ar-SA");
```

#### 2. Apply RTL at Container Level
```xml
<!-- Apply to entire layout -->
<StackLayout FlowDirection="RightToLeft">
    <buttons:SfSwitch />
    <!-- Other controls inherit RTL -->
</StackLayout>
```

#### 3. Handle Mixed Content
```xml
<!-- Some content LTR, some RTL -->
<StackLayout>
    <Label Text="English Content" FlowDirection="LeftToRight"/>
    <buttons:SfSwitch FlowDirection="RightToLeft"/>
    <Label Text="محتوى عربي" FlowDirection="RightToLeft"/>
</StackLayout>
```

#### 4. Test State Transitions
Ensure On/Off states work correctly in RTL:
- On state should have thumb on LEFT (in RTL)
- Off state should have thumb on RIGHT (in RTL)
- Animation should slide left-to-right for enabling

#### 5. Consider Icons and Symbols
If using CustomPath icons, ensure they're appropriate for RTL context:
```xml
<!-- Use symmetric or culturally appropriate icons -->
<buttons:SwitchSettings 
    CustomPath="SymmetricIconPath"
    IconColor="White"/>
```

#### 6. Localization Integration
```csharp
// Use resource files for text
<Label Text="{Binding Source={x:Static resources:AppResources.Settings}}"/>
<buttons:SfSwitch FlowDirection="{Binding CurrentFlowDirection}"/>
```

### Common Scenarios

#### Scenario 1: Multi-Language App
```csharp
public class AppSettings
{
    public string Language { get; set; }
    
    public FlowDirection FlowDirection => 
        LocalizationService.IsRTLLanguage(Language) 
            ? FlowDirection.RightToLeft 
            : FlowDirection.LeftToRight;
}
```

#### Scenario 2: User Preference Override
```csharp
// Allow users to force RTL/LTR regardless of device language
public class UserPreferences
{
    public FlowDirectionMode Mode { get; set; }
}

public enum FlowDirectionMode
{
    Auto,           // Use device language
    ForceRTL,       // Always RTL
    ForceLTR        // Always LTR
}
```

#### Scenario 3: Platform-Specific Handling
```csharp
public FlowDirection GetPlatformFlowDirection()
{
    #if ANDROID
        return FlowDirection.RightToLeft;
    #elif IOS
        return FlowDirection.RightToLeft;
    #else
        return FlowDirection.LeftToRight;
    #endif
}
```

## Combining Advanced Features

### Liquid Glass with RTL

```xml
<Grid FlowDirection="RightToLeft">
    <Image Source="arabic_background.jpg" Aspect="AspectFill"/>
    
    <VerticalStackLayout Padding="30">
        <Label Text="الإعدادات" 
               FontSize="24" 
               TextColor="White"/>
        
        <buttons:SfSwitch EnableLiquidGlassEffect="True" 
                          FlowDirection="RightToLeft"
                          IsOn="True"/>
    </VerticalStackLayout>
</Grid>
```

### Dynamic Feature Detection

```csharp
public class AdvancedSwitchFactory
{
    public SfSwitch CreateSwitch()
    {
        var sfSwitch = new SfSwitch();
        
        // Enable glass effect if supported
        if (IsGlassEffectSupported())
        {
            sfSwitch.EnableLiquidGlassEffect = true;
        }
        
        // Apply RTL if needed
        sfSwitch.FlowDirection = GetFlowDirection();
        
        return sfSwitch;
    }
    
    private bool IsGlassEffectSupported()
    {
        #if IOS || MACCATALYST
            return DeviceInfo.Version.Major >= 26;
        #else
            return false;
        #endif
    }
    
    private FlowDirection GetFlowDirection()
    {
        return CultureInfo.CurrentUICulture.TextInfo.IsRightToLeft
            ? FlowDirection.RightToLeft
            : FlowDirection.LeftToRight;
    }
}
```

## Troubleshooting

### Liquid Glass Not Visible
- **Check platform requirements**: .NET 10+, iOS 26+ or macOS 26+
- **Use vibrant backgrounds**: Plain colors may not show effect
- **Verify property is set**: `EnableLiquidGlassEffect="True"`

### RTL Not Working
- **Check FlowDirection**: Ensure property is set correctly
- **Test device language**: Set device to RTL language
- **Verify parent containers**: Parent layout might override FlowDirection

### Performance Issues with Glass
- **Simplify backgrounds**: Use less complex images
- **Test on device**: Emulators may not accurately represent performance
- **Consider disabling on low-end devices**: Use feature detection
