# Syncfusion Icon Library

## Overview

Syncfusion provides a built-in icon font library for .NET MAUI applications with 2,000+ professionally designed icons.

**Benefits:**
- Scalable vector icons (no pixelation)
- Consistent design language
- Small footprint (single font file)
- Cross-platform compatibility

## Icon Categories

**Available Categories:**
- Action icons (edit, delete, save, etc.)
- Navigation icons (home, back, menu, etc.)
- Communication icons (email, phone, chat, etc.)
- Content icons (add, remove, copy, paste, etc.)
- Device icons (phone, tablet, desktop, etc.)
- File icons (document, folder, pdf, etc.)
- Social media icons (Facebook, Twitter, LinkedIn, etc.)
- Weather icons (sun, cloud, rain, etc.)

## Installation

Icons are included with Syncfusion.Maui.Core package.

**No separate installation required** if you have Core package:

```bash
dotnet add package Syncfusion.Maui.Core
```

## Font Configuration

### Register Icon Font

**In MauiProgram.cs:**

```csharp
using Syncfusion.Maui.Core.Hosting;

var builder = MauiApp.CreateBuilder();
builder.UseMauiApp<App>()
    .ConfigureFonts(fonts =>
    {
        fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
        fonts.AddFont("OpenSans-Semibold.ttf", "OpenSansSemibold");
        
        fonts.AddFont("MauiMaterialAssets.ttf", "MaterialAssets");
    })
    .ConfigureSyncfusionCore();

return builder.Build();
```

Icon font is automatically registered when calling `.ConfigureSyncfusionCore()`.

## Using Icons in XAML

### Basic Label with Icon

```xml
<Label FontFamily="MaterialAssets"
       Text="&#xE70F;"
       FontSize="24"
       TextColor="Black"/>
```

**Unicode:** Each icon has unique unicode (e.g., `&#xE70F;`)

### Button with Icon

```xml
<Button>
    <Button.ImageSource>
        <FontImageSource FontFamily="MaterialAssets"
                         Glyph="&#xe710"
                         Size="20"
                         Color="White"/>
    </Button.ImageSource>
</Button>
```

## Common Icons

**Popular Icons:**

| Icon | Unicode | Constant |
|------|---------|----------|
| Edit | `&#xe710;` | `Edit` |

## Code-Behind Usage

```csharp
using Syncfusion.Maui.Core;

// Create label with icon
var iconLabel = new Label
{
    FontFamily = "MaterialAssets",
    Text = "&#xe710;",
    FontSize = 24,
    TextColor = Colors.Black
};

// Create button with icon
var iconButton = new Button
{
    ImageSource = new FontImageSource
    {
        FontFamily = "MaterialAssets",
        Glyph = "\ue710;",
        Size = 20,
        Color = Colors.White
    }
};
```

## Finding Icon Unicodes

**Method 1: Syncfusion Documentation**
- Browse icon gallery online
- Copy unicode or constant name


**Method 3: Material Icons Reference**
- Syncfusion icons based on Material Design
- Search similar icon in Material Icons

## Platform-Specific Rendering

Icons render consistently across platforms:
- Android
- iOS
- macOS
- Windows

**Automatic platform adaptation** for:
- Touch targets
- Visual density
- Accessibility

## Styling Icons

### Responsive Sizing

```xml
<Label FontFamily="MaterialAssets"
        Glyph="&#xe710">
    <Label.FontSize>
        <OnPlatform x:TypeArguments="x:Double">
            <On Platform="Android,iOS" Value="20"/>
            <On Platform="WinUI" Value="16"/>
        </OnPlatform>
    </Label.FontSize>
</Label>
```

### Theming Icons

```xml
<Label FontFamily="MaterialAssets"
        Glyph="&#xe710"
       TextColor="{AppThemeBinding Light=Black, Dark=White}"/>
```

## Best Practices

✅ **Use FontImageSource** for buttons and toolbar items (better rendering)

✅ **Size appropriately** for touch targets (minimum 44x44 points)

✅ **Match theme colors** for consistency

❌ **Don't hardcode unicodes** (use constants for maintainability)

❌ **Don't scale too small** (<12 points may be unclear)

## Troubleshooting

**Icons not displaying:**
- Verify `ConfigureSyncfusionCore()` called
- Check FontFamily is exactly "MaterialAssets"
- Ensure unicode is correct format: `&#xe700;`

**Icons appear as squares:**
- Font not registered properly
- Rebuild project
- Verify Syncfusion.Maui.Core package installed

**Icons wrong size:**
- Set explicit FontSize or Size property
- Check platform-specific rendering

## Alternative: Custom Icon Fonts

If you prefer custom icon fonts:

1. Add font file to Resources/Fonts/
2. Register in MauiProgram.cs:
```csharp
fonts.AddFont("CustomIcons.ttf", "CustomIcons");
```
3. Use with custom FontFamily

## Related Files

- **Visual Studio Integration:** visual-studio-integration.md
- **Getting Started:** introduction-overview.md
- **Themes:** themes-overview.md