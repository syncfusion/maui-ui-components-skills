# Center Button Customization in MAUI Radial Menu

## Table of Contents
- [Center Button Text](#center-button-text)
- [Text Colors](#text-colors)
- [Background Color](#background-color)
- [Button Radius](#button-radius)
- [Font Customization](#font-customization)
  - [Font Family](#font-family)
  - [Font Size](#font-size)
  - [Font Attributes](#font-attributes)
- [Stroke Styling](#stroke-styling)
- [Custom Views](#custom-views)
- [Animation](#animation)
- [Font Auto Scaling](#font-auto-scaling)
- [Button Size](#button-size)
- [Start Angle](#start-angle)

The center button (or back button) is the circular view at the center of the radial menu. It handles opening/closing the rim and navigating between hierarchical levels. This guide covers all customization options.

## Center Button Text

Set the text displayed on the center button and the back button (shown when navigating into nested items).

**XAML:**
```xaml
<syncfusion:SfRadialMenu CenterButtonText="Edit"
                         CenterButtonBackText="Back">
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Bold" FontSize="12"/>
        <syncfusion:SfRadialMenuItem Text="Color" FontSize="12">
            <syncfusion:SfRadialMenuItem.Items>
                <syncfusion:SfRadialMenuItem Text="Font" FontSize="12"/>
                <syncfusion:SfRadialMenuItem Text="Gradient" FontSize="12"/>
            </syncfusion:SfRadialMenuItem.Items>
        </syncfusion:SfRadialMenuItem>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu
{
    CenterButtonText = "Edit",
    CenterButtonBackText = "Back"
};
```

**Text Guidelines:**
- Keep text short (1-6 characters or single icon)
- Use descriptive labels ("Menu", "Edit", "Tools")
- Back button text often set to "Back" or "←"
- Consider using font icons instead of text

## Text Colors

Customize the text color of both the center button and back button.

**XAML:**
```xaml
<syncfusion:SfRadialMenu CenterButtonText="Edit"
                         CenterButtonTextColor="Blue"
                         CenterButtonBackText="Back"
                         CenterButtonBackTextColor="Yellow">
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Cut" FontSize="12"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu
{
    CenterButtonText = "Edit",
    CenterButtonTextColor = Colors.Blue,
    CenterButtonBackText = "Back",
    CenterButtonBackTextColor = Colors.Yellow
};
```

**Color Best Practices:**
- Ensure high contrast with background
- Match your app's theme colors
- Use consistent colors across UI
- Test in light and dark modes
- Consider accessibility guidelines (WCAG AA minimum)

## Background Color

Set the background color of the center button.

**XAML:**
```xaml
<syncfusion:SfRadialMenu CenterButtonBackgroundColor="#000000" 
                         CenterButtonText="Edit"
                         CenterButtonTextColor="White">
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Bold" FontSize="12"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu
{
    CenterButtonBackgroundColor = Color.FromArgb("#000000"),
    CenterButtonText = "Edit",
    CenterButtonTextColor = Colors.White
};
```

**Background Tips:**
- Dark backgrounds with light text
- Light backgrounds with dark text
- Semi-transparent for modern look: `Color.FromRgba(0, 0, 0, 0.7)`
- Match rim color for cohesive design
- Use accent colors sparingly

## Button Radius

Control the size of the center button by adjusting its radius.

**XAML:**
```xaml
<syncfusion:SfRadialMenu CenterButtonText="Edit" 
                         CenterButtonRadius="40">
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Bold" FontSize="12"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu
{
    CenterButtonText = "Edit",
    CenterButtonRadius = 40
};
```

**Radius Guidelines:**
- Default: 28
- Small: 20-28 (compact menus)
- Medium: 28-40 (standard)
- Large: 40-60 (easy touch target)
- Consider touch target minimum (44x44 points on iOS, 48x48dp on Android)

## Font Customization

### Font Family

Use custom fonts or icon fonts for the center button.

**XAML:**
```xaml
<syncfusion:SfRadialMenu CenterButtonFontFamily="Maui Material Assets"
                         CenterButtonText="&#xe710;"
                         CenterButtonBackFontFamily="Maui Material Assets"
                         CenterButtonBackText="&#xe72d;">
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Bold" FontSize="12"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu
{
    CenterButtonFontFamily = "Maui Material Assets",
    CenterButtonText = "\ue710",
    CenterButtonBackFontFamily = "Maui Material Assets",
    CenterButtonBackText = "\ue72d"
};
```

**Common Icon Fonts:**
- Material Icons
- Font Awesome
- Ionicons
- Syncfusion Icons
- Custom icon fonts

**Setup Icon Fonts in MAUI:**
```csharp
// MauiProgram.cs
builder
    .ConfigureFonts(fonts =>
    {
        fonts.AddFont("MaterialIcons-Regular.ttf", "MaterialIcons");
    });
```

### Font Size

Adjust the size of center button text.

**XAML:**
```xaml
<syncfusion:SfRadialMenu CenterButtonFontFamily="Maui Material Assets"
                         CenterButtonFontSize="28"
                         CenterButtonText="&#xe710;"
                         CenterButtonBackFontFamily="Maui Material Assets"
                         CenterButtonBackFontSize="24"
                         CenterButtonBackText="&#xe72d;">
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu
{
    CenterButtonFontFamily = "Maui Material Assets",
    CenterButtonFontSize = 28,
    CenterButtonText = "\ue710",
    CenterButtonBackFontFamily = "Maui Material Assets",
    CenterButtonBackFontSize = 24,
    CenterButtonBackText = "\ue72d"
};
```

**Font Size Guidelines:**
- Icon fonts: 20-32 (scale well)
- Text: 12-18 (readable)
- Consider button radius when sizing
- Test on actual devices

### Font Attributes

Apply bold, italic, or other text styling.

**XAML:**
```xaml
<syncfusion:SfRadialMenu CenterButtonText="Edit"
                         CenterButtonBackText="Back"
                         CenterButtonFontAttributes="Bold"
                         CenterButtonBackFontAttributes="Bold">
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Cut" FontSize="12"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu
{
    CenterButtonText = "Edit",
    CenterButtonBackText = "Back",
    CenterButtonFontAttributes = FontAttributes.Bold,
    CenterButtonBackFontAttributes = FontAttributes.Bold
};
```

**Available Attributes:**
- `None` - Normal text
- `Bold` - Bold text
- `Italic` - Italic text
- `Bold | Italic` - Combined (not common for short text)

## Stroke Styling

Add a border around the center button with custom color and thickness.

### Stroke Color

**XAML:**
```xaml
<syncfusion:SfRadialMenu CenterButtonText="Edit" 
                         CenterButtonStroke="Black">
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Cut" FontSize="12"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu
{
    CenterButtonText = "Edit",
    CenterButtonStroke = Colors.Black
};
```

### Stroke Thickness

**XAML:**
```xaml
<syncfusion:SfRadialMenu CenterButtonText="Edit"
                         CenterButtonStroke="Black"
                         CenterButtonStrokeThickness="5">
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Cut" FontSize="12"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu
{
    CenterButtonText = "Edit",
    CenterButtonStroke = Colors.Black,
    CenterButtonStrokeThickness = 5
};
```

**Stroke Guidelines:**
- Default: 0 (no stroke)
- Thin: 1-2 (subtle outline)
- Medium: 3-4 (visible border)
- Thick: 5+ (prominent border)
- Match stroke color with theme

## Custom Views

Replace text with completely custom MAUI views for the center button.

**XAML:**
```xaml
<syncfusion:SfRadialMenu x:Name="radialMenu">
    <syncfusion:SfRadialMenu.CenterButtonView>
        <Grid>
            <StackLayout VerticalOptions="Center">
                <Image Source="dotnet_bot.png" 
                       WidthRequest="40" 
                       HeightRequest="40"/>
            </StackLayout>
        </Grid>
    </syncfusion:SfRadialMenu.CenterButtonView>
    
    <syncfusion:SfRadialMenu.CenterButtonBackView>
        <Grid>
            <StackLayout VerticalOptions="Center">
                <Image Source="backicon.png" 
                       WidthRequest="30" 
                       HeightRequest="30"/>
            </StackLayout>
        </Grid>
    </syncfusion:SfRadialMenu.CenterButtonBackView>
    
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Bold" FontSize="12"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
// Create center button view
Grid centerButtonGrid = new Grid();
StackLayout centerButtonLayout = new StackLayout
{
    VerticalOptions = LayoutOptions.Center
};
centerButtonLayout.Children.Add(new Image 
{ 
    Source = "dotnet_bot.png",
    WidthRequest = 40,
    HeightRequest = 40
});
centerButtonGrid.Children.Add(centerButtonLayout);

// Create back button view
Grid centerButtonBackGrid = new Grid();
StackLayout centerButtonBackLayout = new StackLayout
{
    VerticalOptions = LayoutOptions.Center
};
centerButtonBackLayout.Children.Add(new Image 
{ 
    Source = "backicon.png",
    WidthRequest = 30,
    HeightRequest = 30
});
centerButtonBackGrid.Children.Add(centerButtonBackLayout);

SfRadialMenu radialMenu = new SfRadialMenu
{
    CenterButtonView = centerButtonGrid,
    CenterButtonBackView = centerButtonBackGrid
};
```

**Custom View Use Cases:**
- Brand logos
- User profile pictures
- Complex icon combinations
- Animated elements
- Multi-element layouts (icon + badge)

**Custom View Tips:**
- Keep views simple for performance
- Size appropriately for button radius
- Center content within the view
- Test on multiple screen sizes
- Avoid text input controls

## Animation

Enable or disable the center button animation.

**XAML:**
```xaml
<syncfusion:SfRadialMenu EnableCenterButtonAnimation="True">
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Cut" FontSize="12"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu
{
    EnableCenterButtonAnimation = true
};
```

**Animation Behavior:**
- `true`: Smooth rotation/scale on interaction
- `false`: Static, no animation
- Default: true

**When to Disable:**
- Performance concerns on low-end devices
- Preference for instant feedback
- Accessibility requirements
- Conflicting with custom animations

## Font Auto Scaling

Automatically adjust font size based on OS accessibility settings.

**XAML:**
```xaml
<syncfusion:SfRadialMenu CenterButtonFontAutoScalingEnabled="True"
                         CenterButtonText="Edit"
                         CenterButtonFontSize="16">
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Cut" FontSize="12"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu
{
    CenterButtonFontAutoScalingEnabled = true,
    CenterButtonText = "Edit",
    CenterButtonFontSize = 16
};
```

**Auto Scaling Details:**
- Respects user's OS text size preferences
- Improves accessibility
- Applies to both `CenterButtonText` and `CenterButtonBackText`
- Default: false
- Recommended: true for text-based buttons

## Button Size

Set the overall size of the center button (alternative to radius).

**XAML:**
```xaml
<syncfusion:SfRadialMenu CenterButtonSize="80"
                         CenterButtonText="Menu">
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Cut" FontSize="12"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu
{
    CenterButtonSize = 80,
    CenterButtonText = "Menu"
};
```

**Size vs Radius:**
- `CenterButtonSize`: Total diameter
- `CenterButtonRadius`: Half of diameter
- Default size: 56
- Choose one approach and use consistently

## Start Angle

Rotate the entire menu by setting where the first item appears.

**XAML:**
```xaml
<syncfusion:SfRadialMenu StartAngle="45"
                         CenterButtonText="Menu">
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Cut" FontSize="12"/>
        <syncfusion:SfRadialMenuItem Text="Copy" FontSize="12"/>
        <syncfusion:SfRadialMenuItem Text="Paste" FontSize="12"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu
{
    StartAngle = 45,
    CenterButtonText = "Menu"
};
```

**Start Angle Guidelines:**
- Default: 0 (top)
- 0°: First item at top
- 90°: First item at right
- 180°: First item at bottom
- 270°: First item at left
- Useful for aligning items with screen edges

## Complete Customization Example

Combining multiple properties for a fully customized center button:

```csharp
SfRadialMenu radialMenu = new SfRadialMenu
{
    // Text and Font
    CenterButtonText = "MENU",
    CenterButtonBackText = "BACK",
    CenterButtonFontSize = 14,
    CenterButtonFontAttributes = FontAttributes.Bold,
    CenterButtonFontAutoScalingEnabled = true,
    
    // Colors
    CenterButtonTextColor = Colors.White,
    CenterButtonBackTextColor = Colors.White,
    CenterButtonBackgroundColor = Color.FromArgb("#2196F3"),
    
    // Size and Border
    CenterButtonSize = 70,
    CenterButtonStroke = Colors.White,
    CenterButtonStrokeThickness = 3,
    
    // Behavior
    EnableCenterButtonAnimation = true,
    StartAngle = 0
};
```

This creates a professional-looking center button with:
- Clear, bold white text
- Blue background
- White border
- Proper sizing for touch
- Smooth animations
- Accessibility support
