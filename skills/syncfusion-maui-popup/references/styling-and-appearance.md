# Styling and Appearance - Syncfusion .NET MAUI Popup

**Purpose:** Comprehensive guide for customizing the visual appearance of SfPopup controls using the PopupStyle property, including header, footer, content, stroke, and overlay styling.

**When to use this reference:**
- Need to customize header appearance (background, fonts, colors)
- Want to style footer buttons (colors, corner radius)
- Need to customize message/content appearance
- Want to add borders, corner radius, or shadow effects
- Need to customize overlay background color or blur effects
- Want to change the close button icon
- Need to match popup styling to app branding

---

## Table of Contents

1. [PopupStyle Overview](#popupstyle-overview)
2. [Header Styling](#header-styling)
3. [Footer Styling](#footer-styling)
4. [Message/Content Styling](#messagecontent-styling)
5. [Stroke and Border Customization](#stroke-and-border-customization)
6. [Popup Background](#popup-background)
7. [Overlay Background Styling](#overlay-background-styling)
8. [Blur Effects](#blur-effects)
9. [Close Button Customization](#close-button-customization)
10. [Shadow Effects](#shadow-effects)
11. [Style Limitations](#style-limitations)
12. [Complete Styling Examples](#complete-styling-examples)

---

## PopupStyle Overview

The `SfPopup` control uses the `PopupStyle` property to apply styles to all popup elements. This property provides a comprehensive set of styling options for headers, footers, content, borders, and overlays.

### Basic PopupStyle Setup

**XAML:**
```xml
<sfPopup:SfPopup x:Name="myPopup">
    <sfPopup:SfPopup.PopupStyle>
        <sfPopup:PopupStyle 
            HeaderBackground="Blue"
            FooterBackground="LightGray"
            MessageBackground="White" />
    </sfPopup:SfPopup.PopupStyle>
</sfPopup:SfPopup>
```

**C#:**
```csharp
var popup = new SfPopup();
popup.PopupStyle.HeaderBackground = Colors.Blue;
popup.PopupStyle.FooterBackground = Colors.LightGray;
popup.PopupStyle.MessageBackground = Colors.White;
```

---

## Header Styling

Customize the popup header appearance using these PopupStyle properties:

### Available Header Properties

| Property | Type | Description |
|----------|------|-------------|
| `HeaderBackground` | Color | Background color for the header |
| `HeaderTextColor` | Color | Text color for the header title |
| `HeaderFontSize` | double | Font size for the header title |
| `HeaderFontFamily` | string | Font family for the header title |
| `HeaderFontAttribute` | FontAttributes | Font attributes (Bold, Italic, None) |
| `HeaderTextAlignment` | TextAlignment | Text alignment (Start, Center, End) |

### Example: Styled Header

**XAML:**
```xml
<sfPopup:SfPopup x:Name="styledHeaderPopup"
                 HeaderTitle="Custom Header"
                 ShowHeader="True">
    <sfPopup:SfPopup.PopupStyle>
        <sfPopup:PopupStyle 
            HeaderBackground="DimGray"
            HeaderTextColor="White"
            HeaderFontSize="25"
            HeaderFontFamily="Roboto-Medium"
            HeaderFontAttribute="Bold"
            HeaderTextAlignment="Center" />
    </sfPopup:SfPopup.PopupStyle>
</sfPopup:SfPopup>
```

**C#:**
```csharp
public partial class MainPage : ContentPage
{
    SfPopup popup;
    
    public MainPage()
    {
        InitializeComponent();
        
        popup = new SfPopup
        {
            HeaderTitle = "Custom Header",
            ShowHeader = true
        };
        
        // Header styling
        popup.PopupStyle.HeaderBackground = Color.FromRgb(105, 105, 105);
        popup.PopupStyle.HeaderTextColor = Colors.White;
        popup.PopupStyle.HeaderFontSize = 25;
        popup.PopupStyle.HeaderFontFamily = "Roboto-Medium";
        popup.PopupStyle.HeaderFontAttribute = FontAttributes.Bold;
        popup.PopupStyle.HeaderTextAlignment = TextAlignment.Center;
    }
}
```

### Common Header Styles

**Professional Blue Header:**
```xml
<sfPopup:PopupStyle 
    HeaderBackground="#2196F3"
    HeaderTextColor="White"
    HeaderFontSize="20"
    HeaderFontAttribute="Bold"
    HeaderTextAlignment="Center" />
```

**Dark Mode Header:**
```xml
<sfPopup:PopupStyle 
    HeaderBackground="#1E1E1E"
    HeaderTextColor="#E0E0E0"
    HeaderFontSize="18"
    HeaderTextAlignment="Start" />
```

---

## Footer Styling

Customize footer appearance including button colors and corner radius:

### Available Footer Properties

| Property | Type | Description |
|----------|------|-------------|
| `FooterBackground` | Color | Background color for the footer |
| `AcceptButtonBackground` | Color | Background color for Accept button |
| `AcceptButtonTextColor` | Color | Text color for Accept button |
| `DeclineButtonBackground` | Color | Background color for Decline button |
| `DeclineButtonTextColor` | Color | Text color for Decline button |
| `FooterButtonCornerRadius` | CornerRadius | Corner radius for footer buttons (default: 20) |

### Example: Styled Footer with Custom Buttons

**XAML:**
```xml
<sfPopup:SfPopup x:Name="styledFooterPopup"
                 AppearanceMode="TwoButton"
                 ShowFooter="True"
                 AcceptButtonText="Confirm"
                 DeclineButtonText="Cancel">
    <sfPopup:SfPopup.PopupStyle>
        <sfPopup:PopupStyle 
            FooterBackground="LightGray"
            AcceptButtonBackground="DimGray"
            AcceptButtonTextColor="White"
            DeclineButtonBackground="DimGray"
            DeclineButtonTextColor="White"
            FooterButtonCornerRadius="0,20,20,0" />
    </sfPopup:SfPopup.PopupStyle>
</sfPopup:SfPopup>
```

**C#:**
```csharp
public partial class MainPage : ContentPage
{
    SfPopup popup;
    
    public MainPage()
    {
        InitializeComponent();
        
        popup = new SfPopup
        {
            ShowFooter = true,
            AppearanceMode = PopupButtonAppearanceMode.TwoButton,
            AcceptButtonText = "Confirm",
            DeclineButtonText = "Cancel"
        };
        
        // Footer styling
        popup.PopupStyle.FooterBackground = Colors.LightGray;
        popup.PopupStyle.AcceptButtonBackground = Color.FromRgb(105, 105, 105);
        popup.PopupStyle.AcceptButtonTextColor = Colors.White;
        popup.PopupStyle.DeclineButtonBackground = Color.FromRgb(105, 105, 105);
        popup.PopupStyle.DeclineButtonTextColor = Colors.White;
        popup.PopupStyle.FooterButtonCornerRadius = new CornerRadius(0, 20, 20, 0);
    }
}
```

### Common Footer Button Styles

**Primary/Secondary Button Pair:**
```xml
<sfPopup:PopupStyle 
    FooterBackground="White"
    AcceptButtonBackground="#2196F3"
    AcceptButtonTextColor="White"
    DeclineButtonBackground="Transparent"
    DeclineButtonTextColor="#2196F3"
    FooterButtonCornerRadius="8" />
```

**Danger Action Footer (Delete/Cancel):**
```xml
<sfPopup:PopupStyle 
    FooterBackground="#F5F5F5"
    AcceptButtonBackground="#DC3545"
    AcceptButtonTextColor="White"
    DeclineButtonBackground="#6C757D"
    DeclineButtonTextColor="White"
    FooterButtonCornerRadius="5" />
```

---

## Message/Content Styling

Customize the appearance of popup message content:

### Available Message Properties

| Property | Type | Description |
|----------|------|-------------|
| `MessageBackground` | Color | Background color of content area |
| `MessageTextColor` | Color | Foreground/text color of content |
| `MessageFontSize` | double | Font size of the content |
| `MessageFontFamily` | string | Font family for content text |
| `MessageFontAttribute` | FontAttributes | Font attributes (Bold, Italic, None) |
| `MessageTextAlignment` | TextAlignment | Text alignment for content |

### Example: Styled Message

**XAML:**
```xml
<sfPopup:SfPopup x:Name="styledMessagePopup"
                 Message="This is a custom styled message.">
    <sfPopup:SfPopup.PopupStyle>
        <sfPopup:PopupStyle 
            MessageBackground="#4F6750A4"
            MessageTextColor="Gray"
            MessageFontSize="18"
            MessageFontFamily="Roboto-Medium"
            MessageFontAttribute="Bold"
            MessageTextAlignment="Center" />
    </sfPopup:SfPopup.PopupStyle>
</sfPopup:SfPopup>
```

**C#:**
```csharp
public partial class MainPage : ContentPage
{
    SfPopup popup;
    
    public MainPage()
    {
        InitializeComponent();
        
        popup = new SfPopup
        {
            Message = "This is a custom styled message."
        };
        
        // Message styling
        popup.PopupStyle.MessageBackground = Color.FromArgb("#4F6750A4");
        popup.PopupStyle.MessageTextColor = Colors.Gray;
        popup.PopupStyle.MessageFontSize = 18;
        popup.PopupStyle.MessageFontFamily = "Roboto-Medium";
        popup.PopupStyle.MessageFontAttribute = FontAttributes.Bold;
        popup.PopupStyle.MessageTextAlignment = TextAlignment.Center;
    }
}
```

### Common Message Styles

**Informational Message:**
```xml
<sfPopup:PopupStyle 
    MessageBackground="#E3F2FD"
    MessageTextColor="#1976D2"
    MessageFontSize="16"
    MessageTextAlignment="Start" />
```

**Warning Message:**
```xml
<sfPopup:PopupStyle 
    MessageBackground="#FFF3E0"
    MessageTextColor="#F57C00"
    MessageFontSize="16"
    MessageFontAttribute="Bold"
    MessageTextAlignment="Center" />
```

---

## Stroke and Border Customization

Customize the popup border appearance:

### Available Stroke Properties

| Property | Type | Description |
|----------|------|-------------|
| `Stroke` | Color | Stroke/border color for the popup |
| `StrokeThickness` | double | Thickness of the border |
| `CornerRadius` | CornerRadius | Corner radius for the popup view |

### Platform-Specific CornerRadius Behavior

**⚠️ Important:** On Android 33 and above, you can set different corner radius for each corner. On Android versions below 33, corner radius is applied only if the same value is provided for all corners.

### Example: Stroke Customization

**XAML:**
```xml
<sfPopup:SfPopup x:Name="borderedPopup">
    <sfPopup:SfPopup.PopupStyle>
        <sfPopup:PopupStyle 
            Stroke="LightBlue"
            StrokeThickness="10"
            CornerRadius="5" />
    </sfPopup:SfPopup.PopupStyle>
</sfPopup:SfPopup>
```

**C#:**
```csharp
public partial class MainPage : ContentPage
{
    SfPopup popup;
    
    public MainPage()
    {
        InitializeComponent();
        
        popup = new SfPopup();
        
        // Stroke styling
        popup.PopupStyle.Stroke = Colors.LightBlue;
        popup.PopupStyle.StrokeThickness = 10;
        popup.PopupStyle.CornerRadius = 5;
    }
}
```

### Different Corner Radius (Android 33+)

**XAML:**
```xml
<sfPopup:PopupStyle 
    Stroke="#2196F3"
    StrokeThickness="2"
    CornerRadius="20,5,20,5" />
```

**C#:**
```csharp
popup.PopupStyle.Stroke = Color.FromArgb("#2196F3");
popup.PopupStyle.StrokeThickness = 2;
popup.PopupStyle.CornerRadius = new CornerRadius(20, 5, 20, 5);
```

### Common Border Styles

**Material Design Border:**
```xml
<sfPopup:PopupStyle 
    Stroke="#E0E0E0"
    StrokeThickness="1"
    CornerRadius="8" />
```

**Accent Border:**
```xml
<sfPopup:PopupStyle 
    Stroke="#FF6B6B"
    StrokeThickness="3"
    CornerRadius="12" />
```

---

## Popup Background

Customize the background color of the entire popup view:

### PopupBackground Property

**XAML:**
```xml
<sfPopup:SfPopup x:Name="customBackgroundPopup">
    <sfPopup:SfPopup.PopupStyle>
        <sfPopup:PopupStyle PopupBackground="#C3B0D6" />
    </sfPopup:SfPopup.PopupStyle>
</sfPopup:SfPopup>
```

**C#:**
```csharp
public partial class MainPage : ContentPage
{
    SfPopup popup;
    
    public MainPage()
    {
        InitializeComponent();
        
        popup = new SfPopup();
        popup.PopupStyle.PopupBackground = Color.FromArgb("#C3B0D6");
    }
}
```

### Common Background Colors

**Light Theme:**
```xml
<sfPopup:PopupStyle PopupBackground="#FFFFFF" />
```

**Dark Theme:**
```xml
<sfPopup:PopupStyle PopupBackground="#2C2C2C" />
```

**Branded Background:**
```xml
<sfPopup:PopupStyle PopupBackground="#F0F4FF" />
```

---

## Overlay Background Styling

Customize the background overlay that appears behind the popup:

### OverlayColor Property

**XAML:**
```xml
<sfPopup:SfPopup x:Name="customOverlayPopup">
    <sfPopup:SfPopup.PopupStyle>
        <sfPopup:PopupStyle OverlayColor="LightPink" />
    </sfPopup:SfPopup.PopupStyle>
</sfPopup:SfPopup>
```

**C#:**
```csharp
public partial class MainPage : ContentPage
{
    SfPopup popup;
    
    public MainPage()
    {
        InitializeComponent();
        
        popup = new SfPopup();
        popup.PopupStyle.OverlayColor = Colors.LightPink;
    }
}
```

### Set Overlay Opacity

Use RGBA hexadecimal values to adjust overlay transparency:

**XAML:**
```xml
<sfPopup:SfPopup x:Name="semiTransparentOverlay">
    <sfPopup:SfPopup.PopupStyle>
        <!-- 30 = ~19% opacity, FF0000 = red -->
        <sfPopup:PopupStyle OverlayColor="#30FF0000" />
    </sfPopup:SfPopup.PopupStyle>
</sfPopup:SfPopup>
```

**C#:**
```csharp
popup.PopupStyle.OverlayColor = Color.FromArgb("#30FF0000");
```

### Common Overlay Colors

**Dark Modal Overlay (50% opacity):**
```xml
<sfPopup:PopupStyle OverlayColor="#80000000" />
```

**Light Overlay (30% opacity):**
```xml
<sfPopup:PopupStyle OverlayColor="#4DFFFFFF" />
```

**Subtle Gray Overlay:**
```xml
<sfPopup:PopupStyle OverlayColor="#66212121" />
```

---

## Blur Effects

Apply blur effects to the background content behind the popup:

### OverlayMode and BlurIntensity

**Available BlurIntensity Values:**
- `ExtraLight` - Minimal blur effect
- `Light` - Light blur
- `Dark` - Dark blur with reduced brightness
- `ExtraDark` - Heavy dark blur
- `Custom` - Use custom `BlurRadius` value

### Example: Blurred Background

**XAML:**
```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sfPopup="clr-namespace:Syncfusion.Maui.Popup;assembly=Syncfusion.Maui.Popup"
             x:Class="MyApp.MainPage">
    <StackLayout>
        <Image Source="background_image.png" Aspect="Fill">
            <Image.GestureRecognizers>
                <TapGestureRecognizer Tapped="OnImageTapped"/>
            </Image.GestureRecognizers>
        </Image>
        
        <sfPopup:SfPopup x:Name="blurPopup" 
                         OverlayMode="Blur"
                         ShowCloseButton="True">
            <sfPopup:SfPopup.PopupStyle>
                <sfPopup:PopupStyle BlurIntensity="ExtraDark" />
            </sfPopup:SfPopup.PopupStyle>
        </sfPopup:SfPopup>
    </StackLayout>
</ContentPage>
```

**C#:**
```csharp
using Syncfusion.Maui.Popup;

namespace MyApp
{
    public partial class MainPage : ContentPage
    {
        SfPopup popup;
        
        public MainPage()
        {
            InitializeComponent();
            
            popup = new SfPopup
            {
                ShowCloseButton = true,
                OverlayMode = PopupOverlayMode.Blur
            };
            
            popup.PopupStyle.BlurIntensity = PopupBlurIntensity.ExtraDark;
            
            var layout = new StackLayout();
            var image = new Image 
            { 
                Source = "background_image.png", 
                Aspect = Aspect.Fill 
            };
            
            var tapGesture = new TapGestureRecognizer();
            tapGesture.Tapped += OnImageTapped;
            image.GestureRecognizers.Add(tapGesture);
            
            layout.Children.Add(image);
            Content = layout;
        }
        
        private void OnImageTapped(object sender, EventArgs e)
        {
            popup.Show();
        }
    }
}
```

### Custom Blur Radius

For fine-grained control, use `Custom` BlurIntensity with `BlurRadius`:

**XAML:**
```xml
<sfPopup:SfPopup x:Name="customBlurPopup" 
                 OverlayMode="Blur"
                 ShowCloseButton="True">
    <sfPopup:SfPopup.PopupStyle>
        <sfPopup:PopupStyle 
            BlurIntensity="Custom"
            BlurRadius="3" />
    </sfPopup:SfPopup.PopupStyle>
</sfPopup:SfPopup>
```

**C#:**
```csharp
popup.OverlayMode = PopupOverlayMode.Blur;
popup.PopupStyle.BlurIntensity = PopupBlurIntensity.Custom;
popup.PopupStyle.BlurRadius = 3;
```

### Blur Effect Examples

**Subtle Blur:**
```xml
<sfPopup:PopupStyle 
    BlurIntensity="Light" />
```

**Heavy Blur:**
```xml
<sfPopup:PopupStyle 
    BlurIntensity="ExtraDark" />
```

**Custom Moderate Blur:**
```xml
<sfPopup:PopupStyle 
    BlurIntensity="Custom"
    BlurRadius="5" />
```

---

## Close Button Customization

Change the close button icon to match your app's design:

### CloseButtonIcon Property

**XAML:**
```xml
<sfPopup:SfPopup x:Name="customCloseButtonPopup"
                 ShowCloseButton="True">
    <sfPopup:SfPopup.PopupStyle>
        <sfPopup:PopupStyle CloseButtonIcon="closeicon.png" />
    </sfPopup:SfPopup.PopupStyle>
</sfPopup:SfPopup>
```

**C#:**
```csharp
public partial class MainPage : ContentPage
{
    SfPopup popup;
    
    public MainPage()
    {
        InitializeComponent();
        
        popup = new SfPopup
        {
            ShowCloseButton = true
        };
        
        popup.PopupStyle.CloseButtonIcon = "closeicon.png";
    }
}
```

### Custom Close Icon Examples

**Using Font Icon:**
```csharp
// Add icon font to Resources/Fonts/
popup.PopupStyle.CloseButtonIcon = "close_icon.ttf#FontAwesome";
```

**Using SVG Icon:**
```csharp
popup.PopupStyle.CloseButtonIcon = "close.svg";
```

---

## Shadow Effects

Add shadow effects to make the popup appear elevated:

### HasShadow Property

**XAML:**
```xml
<sfPopup:SfPopup x:Name="shadowPopup">
    <sfPopup:SfPopup.PopupStyle>
        <sfPopup:PopupStyle HasShadow="True" />
    </sfPopup:SfPopup.PopupStyle>
</sfPopup:SfPopup>
```

**C#:**
```csharp
public partial class MainPage : ContentPage
{
    SfPopup popup;
    
    public MainPage()
    {
        InitializeComponent();
        
        popup = new SfPopup();
        popup.PopupStyle.HasShadow = true;
    }
}
```

### Combining Shadow with Other Styles

**Material Design Elevation:**
```xml
<sfPopup:SfPopup x:Name="elevatedPopup">
    <sfPopup:SfPopup.PopupStyle>
        <sfPopup:PopupStyle 
            HasShadow="True"
            CornerRadius="8"
            PopupBackground="White"
            Stroke="Transparent" />
    </sfPopup:SfPopup.PopupStyle>
</sfPopup:SfPopup>
```

---

## Style Limitations

**⚠️ Important Limitation:**

The `SfPopup` control **does not support dynamic style changes while it is open**. 

### Workaround for Dynamic Styling

If you need to change styles at runtime:

1. Close the popup
2. Apply style changes
3. Reopen the popup

**Example:**
```csharp
// Close popup
myPopup.IsOpen = false;

// Apply new styles
myPopup.PopupStyle.HeaderBackground = Colors.Blue;
myPopup.PopupStyle.MessageTextColor = Colors.White;

// Reopen popup with new styles
myPopup.IsOpen = true;
```

### Best Practice

**Set all styles before first display:**
```csharp
public MainPage()
{
    InitializeComponent();
    
    // Configure all styles BEFORE showing popup
    ConfigurePopupStyles();
}

private void ConfigurePopupStyles()
{
    myPopup.PopupStyle.HeaderBackground = Colors.Blue;
    myPopup.PopupStyle.FooterBackground = Colors.LightGray;
    myPopup.PopupStyle.MessageTextColor = Colors.Black;
    myPopup.PopupStyle.HasShadow = true;
}

private void ShowPopup()
{
    myPopup.Show(); // Styles are already configured
}
```

---

## Complete Styling Examples

### Example 1: Professional Business Popup

**XAML:**
```xml
<sfPopup:SfPopup x:Name="businessPopup"
                 HeaderTitle="Confirm Action"
                 Message="Are you sure you want to proceed?"
                 ShowHeader="True"
                 ShowFooter="True"
                 AppearanceMode="TwoButton"
                 AcceptButtonText="Yes, Proceed"
                 DeclineButtonText="Cancel">
    <sfPopup:SfPopup.PopupStyle>
        <sfPopup:PopupStyle 
            <!-- Popup Container -->
            PopupBackground="White"
            HasShadow="True"
            CornerRadius="12"
            Stroke="#E0E0E0"
            StrokeThickness="1"
            
            <!-- Header -->
            HeaderBackground="#2196F3"
            HeaderTextColor="White"
            HeaderFontSize="20"
            HeaderFontAttribute="Bold"
            HeaderTextAlignment="Center"
            
            <!-- Message -->
            MessageBackground="White"
            MessageTextColor="#333333"
            MessageFontSize="16"
            MessageTextAlignment="Center"
            
            <!-- Footer Buttons -->
            FooterBackground="#F5F5F5"
            AcceptButtonBackground="#2196F3"
            AcceptButtonTextColor="White"
            DeclineButtonBackground="Transparent"
            DeclineButtonTextColor="#2196F3"
            FooterButtonCornerRadius="8"
            
            <!-- Overlay -->
            OverlayColor="#80000000" />
    </sfPopup:SfPopup.PopupStyle>
</sfPopup:SfPopup>
```

### Example 2: Dark Mode Popup

**C#:**
```csharp
public void CreateDarkModePopup()
{
    var darkPopup = new SfPopup
    {
        HeaderTitle = "Dark Mode Popup",
        Message = "This popup uses dark theme styling.",
        ShowHeader = true,
        ShowFooter = true,
        AppearanceMode = PopupButtonAppearanceMode.TwoButton,
        AcceptButtonText = "Accept",
        DeclineButtonText = "Decline"
    };
    
    // Dark theme styling
    darkPopup.PopupStyle.PopupBackground = Color.FromArgb("#2C2C2C");
    darkPopup.PopupStyle.HasShadow = true;
    darkPopup.PopupStyle.CornerRadius = 10;
    darkPopup.PopupStyle.Stroke = Color.FromArgb("#404040");
    darkPopup.PopupStyle.StrokeThickness = 1;
    
    // Header
    darkPopup.PopupStyle.HeaderBackground = Color.FromArgb("#1E1E1E");
    darkPopup.PopupStyle.HeaderTextColor = Color.FromArgb("#E0E0E0");
    darkPopup.PopupStyle.HeaderFontSize = 18;
    darkPopup.PopupStyle.HeaderFontAttribute = FontAttributes.Bold;
    
    // Message
    darkPopup.PopupStyle.MessageBackground = Color.FromArgb("#2C2C2C");
    darkPopup.PopupStyle.MessageTextColor = Color.FromArgb("#CCCCCC");
    darkPopup.PopupStyle.MessageFontSize = 15;
    
    // Footer
    darkPopup.PopupStyle.FooterBackground = Color.FromArgb("#1E1E1E");
    darkPopup.PopupStyle.AcceptButtonBackground = Color.FromArgb("#4CAF50");
    darkPopup.PopupStyle.AcceptButtonTextColor = Colors.White;
    darkPopup.PopupStyle.DeclineButtonBackground = Color.FromArgb("#F44336");
    darkPopup.PopupStyle.DeclineButtonTextColor = Colors.White;
    darkPopup.PopupStyle.FooterButtonCornerRadius = 6;
    
    // Overlay
    darkPopup.PopupStyle.OverlayColor = Color.FromArgb("#99000000");
    
    darkPopup.Show();
}
```

### Example 3: Minimal Modern Popup with Blur

**XAML:**
```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sfPopup="clr-namespace:Syncfusion.Maui.Popup;assembly=Syncfusion.Maui.Popup"
             x:Class="MyApp.MinimalPopupPage">
    <Grid>
        <Image Source="app_background.png" Aspect="AspectFill" />
        
        <sfPopup:SfPopup x:Name="minimalPopup"
                         HeaderTitle="Notification"
                         Message="You have a new message."
                         ShowHeader="True"
                         ShowFooter="True"
                         ShowCloseButton="True"
                         AppearanceMode="OneButton"
                         AcceptButtonText="OK"
                         OverlayMode="Blur">
            <sfPopup:SfPopup.PopupStyle>
                <sfPopup:PopupStyle 
                    <!-- Blur Effect -->
                    BlurIntensity="Light"
                    
                    <!-- Clean Container -->
                    PopupBackground="#FAFAFA"
                    CornerRadius="16"
                    Stroke="Transparent"
                    HasShadow="True"
                    
                    <!-- Minimal Header -->
                    HeaderBackground="Transparent"
                    HeaderTextColor="#212121"
                    HeaderFontSize="22"
                    HeaderFontAttribute="Bold"
                    HeaderTextAlignment="Start"
                    
                    <!-- Message -->
                    MessageBackground="Transparent"
                    MessageTextColor="#616161"
                    MessageFontSize="16"
                    MessageTextAlignment="Start"
                    
                    <!-- Single Action Button -->
                    FooterBackground="Transparent"
                    AcceptButtonBackground="#2196F3"
                    AcceptButtonTextColor="White"
                    FooterButtonCornerRadius="12" />
            </sfPopup:SfPopup.PopupStyle>
        </sfPopup:SfPopup>
    </Grid>
</ContentPage>
```

### Example 4: Alert/Warning Popup

**C#:**
```csharp
public void ShowWarningPopup()
{
    var warningPopup = new SfPopup
    {
        HeaderTitle = "⚠️ Warning",
        Message = "This action cannot be undone. Please confirm to continue.",
        ShowHeader = true,
        ShowFooter = true,
        AppearanceMode = PopupButtonAppearanceMode.TwoButton,
        AcceptButtonText = "Continue",
        DeclineButtonText = "Cancel"
    };
    
    // Warning theme
    warningPopup.PopupStyle.PopupBackground = Colors.White;
    warningPopup.PopupStyle.CornerRadius = 8;
    warningPopup.PopupStyle.HasShadow = true;
    
    // Orange warning header
    warningPopup.PopupStyle.HeaderBackground = Color.FromArgb("#FF9800");
    warningPopup.PopupStyle.HeaderTextColor = Colors.White;
    warningPopup.PopupStyle.HeaderFontSize = 20;
    warningPopup.PopupStyle.HeaderFontAttribute = FontAttributes.Bold;
    warningPopup.PopupStyle.HeaderTextAlignment = TextAlignment.Center;
    
    // Warning message
    warningPopup.PopupStyle.MessageBackground = Color.FromArgb("#FFF3E0");
    warningPopup.PopupStyle.MessageTextColor = Color.FromArgb("#F57C00");
    warningPopup.PopupStyle.MessageFontSize = 16;
    warningPopup.PopupStyle.MessageTextAlignment = TextAlignment.Center;
    
    // Action buttons
    warningPopup.PopupStyle.FooterBackground = Colors.White;
    warningPopup.PopupStyle.AcceptButtonBackground = Color.FromArgb("#F57C00");
    warningPopup.PopupStyle.AcceptButtonTextColor = Colors.White;
    warningPopup.PopupStyle.DeclineButtonBackground = Colors.LightGray;
    warningPopup.PopupStyle.DeclineButtonTextColor = Color.FromArgb("#616161");
    warningPopup.PopupStyle.FooterButtonCornerRadius = 5;
    
    warningPopup.Show();
}
```

---

## Best Practices

### 1. Consistent Styling Across App

Create a reusable popup styling method:

```csharp
public class PopupStyleHelper
{
    public static void ApplyAppTheme(SfPopup popup)
    {
        popup.PopupStyle.PopupBackground = Colors.White;
        popup.PopupStyle.HeaderBackground = Color.FromArgb("#2196F3");
        popup.PopupStyle.HeaderTextColor = Colors.White;
        popup.PopupStyle.HeaderFontSize = 20;
        popup.PopupStyle.CornerRadius = 12;
        popup.PopupStyle.HasShadow = true;
        popup.PopupStyle.OverlayColor = Color.FromArgb("#80000000");
    }
    
    public static void ApplyDarkTheme(SfPopup popup)
    {
        popup.PopupStyle.PopupBackground = Color.FromArgb("#2C2C2C");
        popup.PopupStyle.HeaderBackground = Color.FromArgb("#1E1E1E");
        popup.PopupStyle.HeaderTextColor = Color.FromArgb("#E0E0E0");
        popup.PopupStyle.MessageTextColor = Color.FromArgb("#CCCCCC");
        popup.PopupStyle.OverlayColor = Color.FromArgb("#99000000");
    }
}

// Usage:
var popup = new SfPopup();
PopupStyleHelper.ApplyAppTheme(popup);
```

### 2. Use Color Constants

```csharp
public static class AppColors
{
    public static Color Primary = Color.FromArgb("#2196F3");
    public static Color Secondary = Color.FromArgb("#FF9800");
    public static Color Success = Color.FromArgb("#4CAF50");
    public static Color Danger = Color.FromArgb("#F44336");
    public static Color Background = Colors.White;
    public static Color TextPrimary = Color.FromArgb("#212121");
    public static Color TextSecondary = Color.FromArgb("#616161");
}

// Usage:
popup.PopupStyle.HeaderBackground = AppColors.Primary;
popup.PopupStyle.AcceptButtonBackground = AppColors.Success;
```

### 3. Platform-Specific Styling

```csharp
public void ConfigurePlatformSpecificStyles(SfPopup popup)
{
#if ANDROID
    popup.PopupStyle.CornerRadius = 8; // Material Design corners
#elif IOS
    popup.PopupStyle.CornerRadius = 12; // iOS-style corners
#elif WINDOWS
    popup.PopupStyle.CornerRadius = 4; // WinUI style
#endif
}
```

### 4. Accessibility Considerations

- Ensure sufficient contrast between text and backgrounds
- Use minimum font size of 14 for readability
- Test with high contrast mode enabled

```csharp
// Good contrast example
popup.PopupStyle.HeaderBackground = Colors.DarkBlue;
popup.PopupStyle.HeaderTextColor = Colors.White; // High contrast

popup.PopupStyle.MessageTextColor = Color.FromArgb("#212121"); // Dark text
popup.PopupStyle.MessageBackground = Colors.White; // Light background
```

---

## Troubleshooting

### Issue: Styles Not Applying

**Problem:** Style changes are not visible when popup is shown.

**Solution:** Ensure styles are set BEFORE calling `Show()` or setting `IsOpen = true`:

```csharp
// ❌ Wrong - styles set after showing
popup.Show();
popup.PopupStyle.HeaderBackground = Colors.Blue; // Won't apply

// ✅ Correct - styles set before showing
popup.PopupStyle.HeaderBackground = Colors.Blue;
popup.Show();
```

### Issue: CornerRadius Not Working on Android

**Problem:** Different corner radius not applying on older Android versions.

**Solution:** Use uniform corner radius for Android < 33:

```csharp
#if ANDROID
    if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Tiramisu)
    {
        popup.PopupStyle.CornerRadius = new CornerRadius(20, 5, 20, 5);
    }
    else
    {
        popup.PopupStyle.CornerRadius = 10; // Uniform for older versions
    }
#endif
```

### Issue: Overlay Color Not Visible

**Problem:** Overlay color appears solid or wrong opacity.

**Solution:** Use ARGB format with alpha channel:

```csharp
// ❌ Wrong - no alpha channel
popup.PopupStyle.OverlayColor = Color.FromArgb("#000000");

// ✅ Correct - 50% opacity black overlay
popup.PopupStyle.OverlayColor = Color.FromArgb("#80000000");
```

---

## Related Topics

- **Layout Customization** - Use ContentTemplate for advanced layouts
- **Events and Modal Behavior** - Handle popup lifecycle events
- **Positioning** - Control where popups appear on screen
- **Animations** - Combine styles with animation effects

---

## Summary

The PopupStyle property provides comprehensive styling options for SfPopup:
- **Header, Footer, Message** styling with colors, fonts, and alignment
- **Stroke and borders** with thickness and corner radius
- **Background and overlay** customization with opacity control
- **Blur effects** for modern, polished UIs
- **Shadow effects** for visual elevation
- **Close button icon** customization

**Key Reminder:** Apply all styles BEFORE showing the popup, as dynamic style changes are not supported while the popup is open.
