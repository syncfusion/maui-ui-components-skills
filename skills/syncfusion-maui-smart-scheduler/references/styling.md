# Styling

## Table of Contents
- [Overview](#overview)
- [AssistStyle Properties](#assiststyle-properties)
- [Placeholder Styling](#placeholder-styling)
- [Header Styling](#header-styling)
- [Font Customization](#font-customization)
- [Complete Styling Examples](#complete-styling-examples)
- [Theme Integration](#theme-integration)
- [Best Practices](#best-practices)

---

## Overview

The SfSmartScheduler provides comprehensive styling options for the assist view through the `AssistStyle` property. You can customize:
- Placeholder text color
- Header text color and background
- Font family, size, and attributes
- Auto-scaling behavior

All styling is applied through the `SmartSchedulerAssistStyle` class accessible via `AssistViewSettings.AssistStyle`.

---

## AssistStyle Properties

### Overview of Available Properties

| Property | Type | Description |
|----------|------|-------------|
| `PlaceholderColor` | Color | Color of placeholder text in input field |
| `AssistViewHeaderTextColor` | Color | Color of header text |
| `AssistViewHeaderBackground` | Brush | Background color/gradient of header |
| `AssistViewHeaderFontSize` | double | Font size for header text |
| `AssistViewHeaderFontFamily` | string | Font family for header text |
| `AssistViewHeaderFontAttributes` | FontAttributes | Font attributes (Bold, Italic, None) |
| `AssistViewHeaderFontAutoScalingEnabled` | bool | Enable/disable automatic font scaling |

---

## Placeholder Styling

### Placeholder Color

Customize the color of the placeholder text in the input field:

**XAML:**
```xml
<smartScheduler:SfSmartScheduler x:Name="smartScheduler">
    <smartScheduler:SfSmartScheduler.AssistViewSettings>
        <smartScheduler:SchedulerAssistViewSettings>
            <smartScheduler:SchedulerAssistViewSettings.AssistStyle>
                <smartScheduler:SmartSchedulerAssistStyle PlaceholderColor="#6750A4" />
            </smartScheduler:SchedulerAssistViewSettings.AssistStyle>
        </smartScheduler:SchedulerAssistViewSettings>
    </smartScheduler:SfSmartScheduler.AssistViewSettings>
</smartScheduler:SfSmartScheduler>
```

**C#:**
```csharp
smartScheduler.AssistViewSettings.AssistStyle = new SmartSchedulerAssistStyle
{
    PlaceholderColor = Color.FromArgb("#6750A4")
};
```

**Color Examples:**
- `"#999999"` - Light gray (subtle)
- `"#6750A4"` - Purple (Material Design)
- `"#0078D4"` - Blue (corporate)
- `"#666666"` - Medium gray (balanced)

### Accessibility Considerations

Ensure placeholder text has adequate contrast but doesn't overpower actual input:

```csharp
// Good contrast for light background
smartScheduler.AssistViewSettings.AssistStyle.PlaceholderColor = Color.FromArgb("#757575");

// For dark mode
smartScheduler.AssistViewSettings.AssistStyle.PlaceholderColor = Color.FromArgb("#B0B0B0");
```

---

## Header Styling

### Header Text Color

Customize the color of the header text:

**XAML:**
```xml
<smartScheduler:SmartSchedulerAssistStyle AssistViewHeaderTextColor="#FFFFFF" />
```

**C#:**
```csharp
smartScheduler.AssistViewSettings.AssistStyle = new SmartSchedulerAssistStyle
{
    AssistViewHeaderTextColor = Colors.White
};
```

### Header Background

Set the background color or gradient for the header:

**Solid Color - XAML:**
```xml
<smartScheduler:SmartSchedulerAssistStyle AssistViewHeaderBackground="#6750A4" />
```

**Solid Color - C#:**
```csharp
smartScheduler.AssistViewSettings.AssistStyle = new SmartSchedulerAssistStyle
{
    AssistViewHeaderBackground = new SolidColorBrush(Color.FromArgb("#6750A4"))
};
```

**Gradient - C#:**
```csharp
smartScheduler.AssistViewSettings.AssistStyle = new SmartSchedulerAssistStyle
{
    AssistViewHeaderBackground = new LinearGradientBrush
    {
        StartPoint = new Point(0, 0),
        EndPoint = new Point(1, 1),
        GradientStops = new GradientStopCollection
        {
            new GradientStop { Color = Color.FromArgb("#6750A4"), Offset = 0 },
            new GradientStop { Color = Color.FromArgb("#4A3A8C"), Offset = 1 }
        }
    }
};
```

### Combined Header Styling

**XAML:**
```xml
<smartScheduler:SfSmartScheduler x:Name="smartScheduler">
    <smartScheduler:SfSmartScheduler.AssistViewSettings>
        <smartScheduler:SchedulerAssistViewSettings>
            <smartScheduler:SchedulerAssistViewSettings.AssistStyle>
                <smartScheduler:SmartSchedulerAssistStyle 
                    AssistViewHeaderTextColor="#FFFFFF"
                    AssistViewHeaderBackground="#6750A4" />
            </smartScheduler:SchedulerAssistViewSettings.AssistStyle>
        </smartScheduler:SchedulerAssistViewSettings>
    </smartScheduler:SfSmartScheduler.AssistViewSettings>
</smartScheduler:SfSmartScheduler>
```

**C#:**
```csharp
smartScheduler.AssistViewSettings.AssistStyle = new SmartSchedulerAssistStyle
{
    AssistViewHeaderTextColor = Colors.White,
    AssistViewHeaderBackground = new SolidColorBrush(Color.FromArgb("#6750A4"))
};
```

---

## Font Customization

### Font Size

Customize the header font size:

**XAML:**
```xml
<smartScheduler:SmartSchedulerAssistStyle AssistViewHeaderFontSize="24" />
```

**C#:**
```csharp
smartScheduler.AssistViewSettings.AssistStyle = new SmartSchedulerAssistStyle
{
    AssistViewHeaderFontSize = 24
};
```

**Size Guidelines:**
- **Small:** 14-16 (compact headers)
- **Medium:** 18-20 (standard)
- **Large:** 22-26 (prominent)
- **Extra Large:** 28+ (hero headers)

### Font Family

Use custom or system fonts:

**XAML:**
```xml
<smartScheduler:SmartSchedulerAssistStyle AssistViewHeaderFontFamily="OpenSansSemibold" />
```

**C#:**
```csharp
smartScheduler.AssistViewSettings.AssistStyle = new SmartSchedulerAssistStyle
{
    AssistViewHeaderFontFamily = "OpenSansSemibold"
};
```

**Common System Fonts:**
- Windows: "Segoe UI", "Calibri"
- macOS/iOS: "San Francisco", "Helvetica Neue"
- Android: "Roboto"

**Custom Fonts:**

1. Add font file to `Resources/Fonts/` folder
2. Register in `MauiProgram.cs`:
   ```csharp
   builder.ConfigureFonts(fonts =>
   {
       fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
       fonts.AddFont("OpenSans-Semibold.ttf", "OpenSansSemibold");
   });
   ```
3. Reference by alias:
   ```csharp
   AssistViewHeaderFontFamily = "OpenSansSemibold"
   ```

### Font Attributes

Apply bold or italic styling:

**XAML:**
```xml
<smartScheduler:SmartSchedulerAssistStyle AssistViewHeaderFontAttributes="Bold" />
```

**C#:**
```csharp
// Bold
smartScheduler.AssistViewSettings.AssistStyle = new SmartSchedulerAssistStyle
{
    AssistViewHeaderFontAttributes = FontAttributes.Bold
};

// Italic
smartScheduler.AssistViewSettings.AssistStyle = new SmartSchedulerAssistStyle
{
    AssistViewHeaderFontAttributes = FontAttributes.Italic
};

// Bold AND Italic
smartScheduler.AssistViewSettings.AssistStyle = new SmartSchedulerAssistStyle
{
    AssistViewHeaderFontAttributes = FontAttributes.Bold | FontAttributes.Italic
};

// None (regular)
smartScheduler.AssistViewSettings.AssistStyle = new SmartSchedulerAssistStyle
{
    AssistViewHeaderFontAttributes = FontAttributes.None
};
```

### Auto-Scaling Font

Enable or disable automatic font scaling based on system accessibility settings:

**XAML:**
```xml
<smartScheduler:SmartSchedulerAssistStyle AssistViewHeaderFontAutoScalingEnabled="True" />
```

**C#:**
```csharp
smartScheduler.AssistViewSettings.AssistStyle = new SmartSchedulerAssistStyle
{
    AssistViewHeaderFontAutoScalingEnabled = true
};
```

**When to use:**
- **Enable (`true`):** Respect user's system font size preferences (accessibility)
- **Disable (`false`):** Maintain fixed font size for design consistency

**Default:** Depends on Syncfusion implementation (typically `false`)

---

## Complete Styling Examples

### Example 1: Material Design Style

**XAML:**
```xml
<smartScheduler:SfSmartScheduler x:Name="smartScheduler">
    <smartScheduler:SfSmartScheduler.AssistViewSettings>
        <smartScheduler:SchedulerAssistViewSettings 
            AssistViewHeaderText="Smart Scheduler"
            Placeholder="Ask me anything...">
            <smartScheduler:SchedulerAssistViewSettings.AssistStyle>
                <smartScheduler:SmartSchedulerAssistStyle 
                    PlaceholderColor="#999999"
                    AssistViewHeaderTextColor="#FFFFFF"
                    AssistViewHeaderBackground="#6750A4"
                    AssistViewHeaderFontSize="20"
                    AssistViewHeaderFontFamily="Roboto"
                    AssistViewHeaderFontAttributes="Bold"
                    AssistViewHeaderFontAutoScalingEnabled="True" />
            </smartScheduler:SchedulerAssistViewSettings.AssistStyle>
        </smartScheduler:SchedulerAssistViewSettings>
    </smartScheduler:SfSmartScheduler.AssistViewSettings>
</smartScheduler:SfSmartScheduler>
```

**C#:**
```csharp
smartScheduler.AssistViewSettings.AssistViewHeaderText = "Smart Scheduler";
smartScheduler.AssistViewSettings.Placeholder = "Ask me anything...";
smartScheduler.AssistViewSettings.AssistStyle = new SmartSchedulerAssistStyle
{
    PlaceholderColor = Color.FromArgb("#999999"),
    AssistViewHeaderTextColor = Colors.White,
    AssistViewHeaderBackground = new SolidColorBrush(Color.FromArgb("#6750A4")),
    AssistViewHeaderFontSize = 20,
    AssistViewHeaderFontFamily = "Roboto",
    AssistViewHeaderFontAttributes = FontAttributes.Bold,
    AssistViewHeaderFontAutoScalingEnabled = true
};
```

### Example 2: Corporate Blue Theme

**C#:**
```csharp
smartScheduler.AssistViewSettings.AssistViewHeaderText = "Meeting Assistant";
smartScheduler.AssistViewSettings.Placeholder = "Schedule your meetings...";
smartScheduler.AssistViewSettings.AssistStyle = new SmartSchedulerAssistStyle
{
    PlaceholderColor = Color.FromArgb("#888888"),
    AssistViewHeaderTextColor = Colors.White,
    AssistViewHeaderBackground = new SolidColorBrush(Color.FromArgb("#0078D4")), // Microsoft Blue
    AssistViewHeaderFontSize = 22,
    AssistViewHeaderFontFamily = "Segoe UI",
    AssistViewHeaderFontAttributes = FontAttributes.Bold,
    AssistViewHeaderFontAutoScalingEnabled = false
};
```

### Example 3: Dark Mode Style

**C#:**
```csharp
smartScheduler.AssistViewSettings.AssistViewHeaderText = "AI Scheduler";
smartScheduler.AssistViewSettings.Placeholder = "Type your request...";
smartScheduler.AssistViewSettings.AssistStyle = new SmartSchedulerAssistStyle
{
    PlaceholderColor = Color.FromArgb("#B0B0B0"), // Light gray for dark background
    AssistViewHeaderTextColor = Color.FromArgb("#E0E0E0"), // Off-white
    AssistViewHeaderBackground = new SolidColorBrush(Color.FromArgb("#1E1E1E")), // Dark gray
    AssistViewHeaderFontSize = 20,
    AssistViewHeaderFontFamily = "Roboto",
    AssistViewHeaderFontAttributes = FontAttributes.Bold,
    AssistViewHeaderFontAutoScalingEnabled = true
};
```

### Example 4: Healthcare Theme

**C#:**
```csharp
smartScheduler.AssistViewSettings.AssistViewHeaderText = "Patient Scheduler";
smartScheduler.AssistViewSettings.Placeholder = "Schedule appointments...";
smartScheduler.AssistViewSettings.AssistStyle = new SmartSchedulerAssistStyle
{
    PlaceholderColor = Color.FromArgb("#999999"),
    AssistViewHeaderTextColor = Colors.White,
    AssistViewHeaderBackground = new SolidColorBrush(Color.FromArgb("#2E7D32")), // Medical green
    AssistViewHeaderFontSize = 20,
    AssistViewHeaderFontFamily = "OpenSansSemibold",
    AssistViewHeaderFontAttributes = FontAttributes.Bold,
    AssistViewHeaderFontAutoScalingEnabled = true
};
```

### Example 5: Gradient Header

**C#:**
```csharp
smartScheduler.AssistViewSettings.AssistViewHeaderText = "Smart Scheduler";
smartScheduler.AssistViewSettings.Placeholder = "What would you like to schedule?";

var gradientBrush = new LinearGradientBrush
{
    StartPoint = new Point(0, 0),
    EndPoint = new Point(1, 0), // Horizontal gradient
    GradientStops = new GradientStopCollection
    {
        new GradientStop { Color = Color.FromArgb("#667EEA"), Offset = 0 },
        new GradientStop { Color = Color.FromArgb("#764BA2"), Offset = 1 }
    }
};

smartScheduler.AssistViewSettings.AssistStyle = new SmartSchedulerAssistStyle
{
    PlaceholderColor = Color.FromArgb("#999999"),
    AssistViewHeaderTextColor = Colors.White,
    AssistViewHeaderBackground = gradientBrush,
    AssistViewHeaderFontSize = 22,
    AssistViewHeaderFontFamily = "OpenSansSemibold",
    AssistViewHeaderFontAttributes = FontAttributes.Bold,
    AssistViewHeaderFontAutoScalingEnabled = true
};
```

---

## Theme Integration

### Responding to System Theme Changes

Adapt styling based on system light/dark mode:

```csharp
public void ApplyThemeBasedStyling()
{
    var currentTheme = Application.Current.RequestedTheme;
    
    if (currentTheme == AppTheme.Dark)
    {
        // Dark mode styling
        smartScheduler.AssistViewSettings.AssistStyle = new SmartSchedulerAssistStyle
        {
            PlaceholderColor = Color.FromArgb("#B0B0B0"),
            AssistViewHeaderTextColor = Color.FromArgb("#E0E0E0"),
            AssistViewHeaderBackground = new SolidColorBrush(Color.FromArgb("#1E1E1E")),
            AssistViewHeaderFontSize = 20,
            AssistViewHeaderFontAttributes = FontAttributes.Bold
        };
    }
    else
    {
        // Light mode styling
        smartScheduler.AssistViewSettings.AssistStyle = new SmartSchedulerAssistStyle
        {
            PlaceholderColor = Color.FromArgb("#888888"),
            AssistViewHeaderTextColor = Colors.White,
            AssistViewHeaderBackground = new SolidColorBrush(Color.FromArgb("#6750A4")),
            AssistViewHeaderFontSize = 20,
            AssistViewHeaderFontAttributes = FontAttributes.Bold
        };
    }
}
```

### Listen for Theme Changes

**App.xaml.cs:**
```csharp
public partial class App : Application
{
    public App()
    {
        InitializeComponent();
        MainPage = new MainPage();
    }

    protected override void OnStart()
    {
        Application.Current.RequestedThemeChanged += OnThemeChanged;
    }

    private void OnThemeChanged(object sender, AppThemeChangedEventArgs e)
    {
        // Update scheduler styling
        MessagingCenter.Send<App, AppTheme>(this, "ThemeChanged", e.RequestedTheme);
    }
}
```

**MainPage.xaml.cs:**
```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        MessagingCenter.Subscribe<App, AppTheme>(this, "ThemeChanged", (sender, theme) =>
        {
            ApplyThemeBasedStyling();
        });
        
        // Apply initial theme
        ApplyThemeBasedStyling();
    }
}
```

### Using Resource Dictionaries

Define styles in XAML resources:

**App.xaml:**
```xml
<Application.Resources>
    <ResourceDictionary>
        <!-- Light theme -->
        <Color x:Key="AssistHeaderBgLight">#6750A4</Color>
        <Color x:Key="AssistHeaderTextLight">#FFFFFF</Color>
        <Color x:Key="PlaceholderLight">#888888</Color>
        
        <!-- Dark theme -->
        <Color x:Key="AssistHeaderBgDark">#1E1E1E</Color>
        <Color x:Key="AssistHeaderTextDark">#E0E0E0</Color>
        <Color x:Key="PlaceholderDark">#B0B0B0</Color>
    </ResourceDictionary>
</Application.Resources>
```

**MainPage.xaml.cs:**
```csharp
public void ApplyThemeFromResources()
{
    var currentTheme = Application.Current.RequestedTheme;
    var bgKey = currentTheme == AppTheme.Dark ? "AssistHeaderBgDark" : "AssistHeaderBgLight";
    var textKey = currentTheme == AppTheme.Dark ? "AssistHeaderTextDark" : "AssistHeaderTextLight";
    var placeholderKey = currentTheme == AppTheme.Dark ? "PlaceholderDark" : "PlaceholderLight";
    
    smartScheduler.AssistViewSettings.AssistStyle = new SmartSchedulerAssistStyle
    {
        AssistViewHeaderBackground = new SolidColorBrush((Color)Application.Current.Resources[bgKey]),
        AssistViewHeaderTextColor = (Color)Application.Current.Resources[textKey],
        PlaceholderColor = (Color)Application.Current.Resources[placeholderKey],
        AssistViewHeaderFontSize = 20,
        AssistViewHeaderFontAttributes = FontAttributes.Bold
    };
}
```

---

## Best Practices

### 1. Maintain Readability

Ensure text is readable against backgrounds:

**Good contrast:**
```csharp
// White text on dark background
AssistViewHeaderTextColor = Colors.White
AssistViewHeaderBackground = "#1E1E1E"

// Dark text on light background
AssistViewHeaderTextColor = "#1C1B1F"
AssistViewHeaderBackground = "#F5F5F5"
```

**Poor contrast (avoid):**
```csharp
// Light gray on white (hard to read)
AssistViewHeaderTextColor = "#CCCCCC"
AssistViewHeaderBackground = "#FFFFFF"
```

### 2. Use Brand Colors Consistently

Match your app's brand identity:

```csharp
// Extract brand colors
const string BrandPrimary = "#0078D4";
const string BrandSecondary = "#106EBE";
const string BrandText = "#FFFFFF";

smartScheduler.AssistViewSettings.AssistStyle = new SmartSchedulerAssistStyle
{
    AssistViewHeaderBackground = new SolidColorBrush(Color.FromArgb(BrandPrimary)),
    AssistViewHeaderTextColor = Color.FromArgb(BrandText),
    PlaceholderColor = Color.FromArgb("#888888")
};
```

### 3. Consider Accessibility

- **Enable auto-scaling** for users with vision impairments
- **Use sufficient contrast** (WCAG AA: 4.5:1 for normal text)
- **Test with large font sizes** to ensure layouts don't break

```csharp
smartScheduler.AssistViewSettings.AssistStyle = new SmartSchedulerAssistStyle
{
    AssistViewHeaderFontAutoScalingEnabled = true, // Respect system font size
    // Ensure adequate contrast
    AssistViewHeaderTextColor = Colors.White,
    AssistViewHeaderBackground = new SolidColorBrush(Color.FromArgb("#0078D4"))
};
```

### 4. Test on Multiple Platforms

Fonts and colors may render differently:

- Test on **Android**, **iOS**, **Windows**, **macOS**
- Verify gradient rendering
- Check font family availability
- Ensure colors display consistently

### 5. Use System Fonts as Fallback

If custom font isn't available, system font is used:

```csharp
// Specify custom font with system fallback
AssistViewHeaderFontFamily = "CustomFont, Segoe UI, Roboto, San Francisco"
```

### 6. Keep It Simple

Don't over-style—clarity is key:

```csharp
// Simple, clean styling
smartScheduler.AssistViewSettings.AssistStyle = new SmartSchedulerAssistStyle
{
    PlaceholderColor = Color.FromArgb("#999999"),
    AssistViewHeaderTextColor = Colors.White,
    AssistViewHeaderBackground = new SolidColorBrush(Color.FromArgb("#6750A4")),
    AssistViewHeaderFontSize = 20,
    AssistViewHeaderFontAttributes = FontAttributes.Bold
};
```

---

## Troubleshooting

**Issue:** Custom font not displaying  
**Solution:** Verify font file is in `Resources/Fonts/` and registered in `MauiProgram.cs`

**Issue:** Colors look different on iOS vs Android  
**Solution:** Test ARGB values on both platforms; may need platform-specific adjustments

**Issue:** Header text cut off  
**Solution:** Reduce font size or increase `AssistViewHeight`

**Issue:** Gradient not showing  
**Solution:** Ensure `LinearGradientBrush` is properly configured with `StartPoint`, `EndPoint`, and `GradientStops`

**Issue:** Auto-scaling not working  
**Solution:** Verify `AssistViewHeaderFontAutoScalingEnabled = true` and test with system font size changes

---

## Next Steps

- **Explore event handling:** [events-and-methods.md](events-and-methods.md)
- **Customize assist view layout:** [assist-view-customization.md](assist-view-customization.md)
- **Learn natural language patterns:** [natural-language-operations.md](natural-language-operations.md)
