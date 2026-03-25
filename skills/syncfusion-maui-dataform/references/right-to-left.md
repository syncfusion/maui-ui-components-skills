# Right-to-Left (RTL) Support

## Overview

Right-to-left support enables DataForm to display content in RTL languages like Arabic, Hebrew, Persian, and Urdu by reversing the text flow direction.

**Key features:**
- Automatic layout mirroring
- Label and editor positioning reversal
- Icons and buttons positioned appropriately
- Validation messages aligned correctly

**Supported RTL languages:**
- Arabic (ar)
- Hebrew (he)
- Persian/Farsi (fa)
- Urdu (ur)

## Enabling RTL

Set `FlowDirection` property to `RightToLeft`:

```xaml
<dataForm:SfDataForm 
    x:Name="dataForm"
    FlowDirection="RightToLeft">
</dataForm:SfDataForm>
```

```csharp
var dataForm = new SfDataForm
{
    FlowDirection = FlowDirection.RightToLeft
};
```

## RTL with Localization

Combine RTL with localized content:

```csharp
using System.Globalization;

public partial class App : Application
{
    public App()
    {
        InitializeComponent();
        
        // Set culture to Arabic
        var culture = new CultureInfo("ar");
        CultureInfo.CurrentUICulture = culture;
        
        MainPage = new AppShell();
    }
}

// In page
var dataForm = new SfDataForm
{
    FlowDirection = FlowDirection.RightToLeft,
    DataObject = new ContactInfo()
};
```

## Dynamic RTL Switching

Toggle RTL based on user preference or culture:

```csharp
public class LocalizationService
{
    private static readonly string[] RtlLanguages = { "ar", "he", "fa", "ur" };
    
    public static FlowDirection GetFlowDirection(string cultureCode)
    {
        return RtlLanguages.Contains(cultureCode.ToLower()) 
            ? FlowDirection.RightToLeft 
            : FlowDirection.LeftToRight;
    }
}

// In page
private void OnLanguageChanged(string languageCode)
{
    var culture = new CultureInfo(languageCode);
    CultureInfo.CurrentUICulture = culture;
    
    dataForm.FlowDirection = LocalizationService.GetFlowDirection(languageCode);
    dataForm.Reload();
}
```

## RTL Behavior

### Label Positioning

**LTR (default):**
```
[Label] [Editor        ]
```

**RTL:**
```
[        Editor] [Label]
```

### Leading and Trailing Views

Icons automatically swap positions in RTL:

**LTR:**
```
[Icon] [Editor        ] [Clear]
```

**RTL:**
```
[Clear] [        Editor] [Icon]
```

### Validation Messages

Error messages align to the right in RTL mode:

**LTR:**
```
[Editor              ]
Error message here
```

**RTL:**
```
              [Editor]
          رسالة الخطأ هنا
```

## Best Practices

### 1. Test with RTL Languages

```csharp
// Create test configurations for RTL languages
public static void TestRTL()
{
    var cultures = new[] { "ar", "he", "fa", "ur" };
    
    foreach (var culture in cultures)
    {
        CultureInfo.CurrentUICulture = new CultureInfo(culture);
        // Verify layout and content
    }
}
```

### 2. Use Culture-Aware Icons

Avoid directional icons that don't make sense when mirrored:

```csharp
// Instead of hardcoded arrows
// Use culture-aware icon selection
string GetNavigationIcon()
{
    return CultureInfo.CurrentUICulture.TextInfo.IsRightToLeft
        ? "\uf053" // Left arrow for RTL
        : "\uf054"; // Right arrow for LTR
}
```

### 3. Avoid Hardcoded Positioning

```csharp
// ❌ Don't use absolute positioning
e.DataFormItem.Padding = new Thickness(10, 0, 0, 0); // Left padding only

// ✅ Use symmetric or automatic padding
e.DataFormItem.Padding = new Thickness(10); // All sides
```

## Troubleshooting

### RTL Not Applying

**Solutions:**
```csharp
// 1. Ensure FlowDirection is set
dataForm.FlowDirection = FlowDirection.RightToLeft;

// 2. Check platform-specific requirements
// Some platforms require app-level RTL setting

// 3. Reload DataForm after changing direction
dataForm.FlowDirection = FlowDirection.RightToLeft;
dataForm.Reload();
```

### Icons Not Mirroring

**Solutions:**
```csharp
// Leading/Trailing views automatically swap
// Ensure using LeadingView/TrailingView, not absolute positioning

dataForm.GenerateDataFormItem += (sender, e) =>
{
    if (e.DataFormItem != null)
    {
        e.DataFormItem.LeadingView = new Label { Text = "Icon" };
        // Icon will appear on right in RTL
    }
};
```

### Text Not Aligned Correctly

**Solutions:**
```csharp
// Labels auto-align in RTL
// For custom views, set TextAlignment based on FlowDirection

var isRTL = CultureInfo.CurrentUICulture.TextInfo.IsRightToLeft;
label.HorizontalTextAlignment = isRTL ? TextAlignment.End : TextAlignment.Start;
```

---

**Related Topics:**
- [getting-started.md](getting-started.md) - Basic DataForm setup
- [localization.md](localization.md) - Multi-language support
- [layout.md](layout.md) - Label positioning and layout
