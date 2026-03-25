# Localization and Advanced Features in .NET MAUI Calendar

This guide covers globalization, localization, RTL support, calendar identifiers, and advanced visual features of the SfCalendar.

## Table of Contents
- [Globalization and Localization](#globalization-and-localization)
- [RTL (Right-to-Left) Support](#rtl-right-to-left-support)
- [Calendar Identifier](#calendar-identifier)
- [Liquid Glass Effect](#liquid-glass-effect)

## Globalization and Localization

The SfCalendar supports globalization to display dates and text in different languages and formats based on the application's culture settings.

### Setting Current UI Culture

Configure the application culture in `App.xaml.cs`:

**App.xaml.cs:**
```csharp
using System.Globalization;
using System.Resources;
using Syncfusion.Maui.Calendar;

public partial class App : Application
{
    public App()
    {
        InitializeComponent();

        // Set the culture to Japanese
        CultureInfo.CurrentUICulture = CultureInfo.CreateSpecificCulture("ja-JP");
        
        // Set the resource manager for Syncfusion calendar strings
        // ResXPath => Full path of the resx file
        // Example: "MyApp.Resources.SfCalendar"
        SfCalendarResources.ResourceManager = new ResourceManager(
            "MyApp.Resources.SfCalendar", 
            Application.Current.GetType().Assembly
        );

        MainPage = new AppShell();
    }
}
```

### Creating Resource Files

To localize the calendar, create resource files for each supported language.

**Steps:**

1. **Create Resources Folder:**
   - Create a folder named `Resources` in your project

2. **Add Resource File:**
   - Right-click `Resources` folder → `Add` → `New Item`
   - Select `Resource File`
   - Name it: `SfCalendar.<culture-code>.resx`
     - Example: `SfCalendar.ja-JP.resx` for Japanese
     - Example: `SfCalendar.fr-FR.resx` for French
     - Example: `SfCalendar.es-ES.resx` for Spanish

3. **Set Build Action:**
   - Select the resource file
   - In Properties, set `Build Action` to `EmbeddedResource`

4. **Add Name/Value Pairs:**
   - Open the resource designer
   - Add localized strings for calendar elements

### Resource File Example (Japanese)

**SfCalendar.ja-JP.resx:**

| Name | Value (Japanese) |
|------|-----------------|
| Sunday | 日曜日 |
| Monday | 月曜日 |
| Tuesday | 火曜日 |
| Wednesday | 水曜日 |
| Thursday | 木曜日 |
| Friday | 金曜日 |
| Saturday | 土曜日 |
| January | 1月 |
| February | 2月 |
| March | 3月 |
| ... | ... |

### Supported Cultures

Examples of culture codes:
- `en-US` - English (United States)
- `ja-JP` - Japanese (Japan)
- `fr-FR` - French (France)
- `de-DE` - German (Germany)
- `es-ES` - Spanish (Spain)
- `zh-CN` - Chinese (Simplified, China)
- `ar-SA` - Arabic (Saudi Arabia)
- `he-IL` - Hebrew (Israel)

### Complete Localization Example

```csharp
// In App.xaml.cs
public App()
{
    InitializeComponent();
    
    // Detect system language or use user preference
    var userLanguage = Preferences.Get("UserLanguage", "en-US");
    CultureInfo.CurrentUICulture = CultureInfo.CreateSpecificCulture(userLanguage);
    
    // Register resource manager
    SfCalendarResources.ResourceManager = new ResourceManager(
        "MyCalendarApp.Resources.SfCalendar",
        typeof(App).GetTypeInfo().Assembly
    );
    
    MainPage = new MainPage();
}
```

### Dynamic Culture Switching

```csharp
public void ChangeLanguage(string cultureCode)
{
    // Update culture
    CultureInfo.CurrentUICulture = CultureInfo.CreateSpecificCulture(cultureCode);
    
    // Save preference
    Preferences.Set("UserLanguage", cultureCode);
    
    // Recreate the calendar to apply new culture
    RecreateCalendar();
}

private void RecreateCalendar()
{
    // Remove old calendar
    var oldCalendar = this.Content as SfCalendar;
    
    // Create new calendar with updated culture
    var newCalendar = new SfCalendar
    {
        View = oldCalendar.View,
        SelectionMode = oldCalendar.SelectionMode,
        // ... copy other settings
    };
    
    this.Content = newCalendar;
}
```

## RTL (Right-to-Left) Support

The SfCalendar supports Right-to-Left (RTL) languages such as Arabic and Hebrew.

### Enable RTL

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar"
                     FlowDirection="RightToLeft" />
```

**C#:**
```csharp
calendar.FlowDirection = FlowDirection.RightToLeft;
```

### RTL with Arabic Culture

```csharp
// In App.xaml.cs
public App()
{
    InitializeComponent();
    
    // Set Arabic culture
    CultureInfo.CurrentUICulture = CultureInfo.CreateSpecificCulture("ar-SA");
    
    // Register Arabic resource file
    SfCalendarResources.ResourceManager = new ResourceManager(
        "MyApp.Resources.SfCalendar",
        Application.Current.GetType().Assembly
    );
    
    MainPage = new MainPage();
}

// In MainPage.xaml.cs
public MainPage()
{
    InitializeComponent();
    
    // Enable RTL for Arabic
    calendar.FlowDirection = FlowDirection.RightToLeft;
}
```

### RTL Behavior

When RTL is enabled:
- Calendar dates flow from right to left
- Week starts from right side
- Navigation arrows reverse (left arrow goes forward, right arrow goes backward)
- Text alignment changes to right-aligned
- Header and footer elements flip horizontally

### RTL with Custom FirstDayOfWeek

```csharp
calendar.FlowDirection = FlowDirection.RightToLeft;
calendar.MonthView.FirstDayOfWeek = DayOfWeek.Saturday; // Common in Middle East
```

## Calendar Identifier

The `CalendarIdentifier` property allows you to use different calendar systems such as Gregorian, Hijri (Islamic), Hebrew, and more.

### Supported Calendar Identifiers

```csharp
public enum CalendarIdentifier
{
    Gregorian,      // Standard Western calendar (default)
    Hijri,          // Islamic calendar
    Korean,         // Korean calendar
    Taiwan,         // Taiwan calendar
    ThaiBuddhist,           // Thai Buddhist calendar
    UmAlQura,       // Saudi Arabian Hijri calendar
    Persian,        // Iranian/Persian calendar
}
```

### Set Calendar Identifier

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar"
                     CalendarIdentifier="Hijri" />
```

**C#:**
```csharp
calendar.Identifier = CalendarIdentifier.Hijri;
```

### Calendar Examples

**Hijri Calendar (Islamic):**
```csharp
// Set up Hijri calendar
calendar.Identifier = CalendarIdentifier.Hijri;
CultureInfo.CurrentUICulture = CultureInfo.CreateSpecificCulture("ar-SA");
calendar.FlowDirection = FlowDirection.RightToLeft;
```

**Hebrew Calendar:**
```csharp
calendar.Identifier = CalendarIdentifier.Persian;
CultureInfo.CurrentUICulture = CultureInfo.CreateSpecificCulture("fa-IR");
calendar.FlowDirection = FlowDirection.RightToLeft;
```

**Japanese Calendar:**
```csharp
calendar.Identifier = CalendarIdentifier.Korean;
CultureInfo.CurrentUICulture = CultureInfo.CreateSpecificCulture("ko-KR");
```

**Thai Buddhist Calendar:**
```csharp
calendar.Identifier = CalendarIdentifier.ThaiBuddhist;
CultureInfo.CurrentUICulture = CultureInfo.CreateSpecificCulture("th-TH");
```

### Complete Multi-Calendar Example

```csharp
public void SetCalendarSystem(string system)
{
    switch (system)
    {
        case "Gregorian":
            calendar.Identifier = CalendarIdentifier.Gregorian;
            CultureInfo.CurrentUICulture = CultureInfo.CreateSpecificCulture("en-US");
            calendar.FlowDirection = FlowDirection.LeftToRight;
            break;
            
        case "Hijri":
            calendar.Identifier = CalendarIdentifier.Hijri;
            CultureInfo.CurrentUICulture = CultureInfo.CreateSpecificCulture("ar-SA");
            calendar.FlowDirection = FlowDirection.RightToLeft;
            break;
            
        case "Hebrew":
            calendar.Identifier = CalendarIdentifier.Persian;
            CultureInfo.CurrentUICulture = CultureInfo.CreateSpecificCulture("fa-IR");
            calendar.FlowDirection = FlowDirection.RightToLeft;
            break;
            
        case "Japanese":
            calendar.Identifier = CalendarIdentifier.ThaiBuddhist;
            CultureInfo.CurrentUICulture = CultureInfo.CreateSpecificCulture("th-TH");
            calendar.FlowDirection = FlowDirection.LeftToRight;
            break;
    }
}
```

## Liquid Glass Effect

The SfCalendar supports a modern "liquid glass" visual effect that provides a frosted, translucent appearance.

### Enable Liquid Glass Effect

**XAML:**
```xml
<calendar:SfCalendar x:Name="calendar"
                     EnableLiquidGlassEffect="True" />
```

**C#:**
```csharp
calendar.EnableLiquidGlassEffect = true;
```

**Default:** `false`

### Liquid Glass with Custom Styling

```csharp
calendar.EnableLiquidGlassEffect = true;
calendar.Background = Colors.Transparent;
calendar.MonthView = new CalendarMonthView
{
    Background = Colors.White.WithAlpha(0.5f),
    TextStyle = new CalendarTextStyle
    {
        TextColor = Colors.Black,
        FontSize = 14
    }
};
```

### Use Cases for Liquid Glass Effect

1. **Modern UI Design:** Create contemporary, glass-morphism inspired interfaces
2. **Overlay Calendars:** When displaying calendar over images or gradients
3. **Premium Feel:** Add visual depth and sophistication
4. **Branding:** Match modern design trends and corporate aesthetics

### Complete Advanced Styling Example

```csharp
// Liquid glass calendar with custom colors
calendar.EnableLiquidGlassEffect = true;
calendar.Background = new LinearGradientBrush
{
    StartPoint = new Point(0, 0),
    EndPoint = new Point(1, 1),
    GradientStops = new GradientStopCollection
    {
        new GradientStop { Color = Colors.LightBlue, Offset = 0.0f },
        new GradientStop { Color = Colors.LightPink, Offset = 1.0f }
    }
};

calendar.MonthView = new CalendarMonthView
{
    Background = Colors.White.WithAlpha(0.3f),
    TextStyle = new CalendarTextStyle
    {
        TextColor = Colors.DarkBlue,
        FontSize = 14,
        FontAttributes = FontAttributes.Bold
    },
    TodayBackground = Colors.DeepSkyBlue.WithAlpha(0.5f),
};

SelectionBackground = Colors.Purple.WithAlpha(0.4f)
```

## Best Practices

### Localization
1. **Test All Supported Languages:** Ensure UI elements don't overlap or truncate
2. **Use Resource Files:** Centralize all translatable strings
3. **Support Dynamic Language Switching:** Allow users to change language at runtime
4. **Test Date Formats:** Verify that date formatting works correctly for each culture
5. **Consider Text Expansion:** Some languages require more space (e.g., German is ~30% longer than English)

### RTL Support
1. **Test with RTL Languages:** Always test Arabic and Hebrew layouts
2. **Adjust Custom Templates:** Ensure custom headers/footers work in RTL
3. **Mirror Icons Appropriately:** Some icons should mirror in RTL, others shouldn't
4. **Verify Navigation:** Ensure swipe and arrow navigation work correctly in RTL

### Calendar Identifiers
1. **Match Culture to Calendar:** Use appropriate culture with calendar identifier
2. **Handle Date Conversions:** Be aware of date differences between calendar systems
3. **Test Date Ranges:** Some calendar systems have different year ranges
4. **Validate User Input:** Ensure dates are valid for the selected calendar system

### Liquid Glass Effect
1. **Performance Considerations:** May impact performance on older devices
2. **Test on Real Devices:** Effect may look different across platforms
3. **Accessibility:** Ensure text remains readable with the effect enabled
4. **Background Considerations:** Works best with light or gradient backgrounds
