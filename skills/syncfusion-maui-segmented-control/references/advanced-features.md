# Advanced Features

This guide covers advanced features including Right-to-Left (RTL) support, liquid glass effects, and other special capabilities.

## Right-to-Left (RTL) Support

The Segmented Control supports right-to-left rendering for languages like Arabic, Hebrew, and Persian.

### Enabling RTL

Set the `FlowDirection` property to `RightToLeft`.

**XAML:**
```xml
<buttons:SfSegmentedControl FlowDirection="RightToLeft">
    <buttons:SfSegmentedControl.ItemsSource>
        <x:Array Type="{x:Type x:String}">
            <x:String>اليوم</x:String>
            <x:String>أسبوع</x:String>
            <x:String>شهر</x:String>
            <x:String>سنة</x:String>
        </x:Array>
    </buttons:SfSegmentedControl.ItemsSource>
</buttons:SfSegmentedControl>
```

**C#:**
```csharp
var segmentedControl = new SfSegmentedControl
{
    FlowDirection = FlowDirection.RightToLeft,
    ItemsSource = new List<string> { "اليوم", "أسبوع", "شهر", "سنة" }
};
```

### Effects of RTL

When `FlowDirection` is set to `RightToLeft`:
- **Segment order:** Reversed (rightmost segment becomes first)
- **Text alignment:** Right-aligned within segments
- **Selection indicator:** Starts from right
- **Scrolling:** Scrolls from right to left

### Automatic RTL Based on Culture

Set FlowDirection based on current culture:

```csharp
public void SetSegmentedControlDirection()
{
    var currentCulture = CultureInfo.CurrentCulture;
    
    // Check if current culture is RTL
    bool isRightToLeft = currentCulture.TextInfo.IsRightToLeft;
    
    segmentedControl.FlowDirection = isRightToLeft 
        ? FlowDirection.RightToLeft 
        : FlowDirection.LeftToRight;
}
```

### RTL with Icons

Icons maintain their orientation but the segment order reverses:

```csharp
var segmentedControl = new SfSegmentedControl
{
    FlowDirection = FlowDirection.RightToLeft,
    ItemsSource = new List<SfSegmentItem>
    {
        new SfSegmentItem { Text = "الصفحة الرئيسية", ImageSource = "home.png" },
        new SfSegmentItem { Text = "بحث", ImageSource = "search.png" },
        new SfSegmentItem { Text = "الملف الشخصي", ImageSource = "profile.png" }
    }
};
```

### Testing RTL Layout

Test RTL support by:
1. Changing device language to Arabic, Hebrew, or Persian
2. Setting FlowDirection property explicitly
3. Using RTL text content
4. Verifying segment order and selection behavior

## Liquid Glass Effect

The Liquid Glass Effect creates a modern, translucent design with adaptive color tinting and light refraction.

### Requirements

- **.NET 10 or higher**
- **Platform support:** macOS 26+, iOS 26+
- **Package:** Syncfusion.Maui.Core (for SfGlassEffectView)

### Enabling Liquid Glass Effect

#### Step 1: Wrap Control in SfGlassEffectView

```xml
<core:SfGlassEffectView EffectType="Clear">
    <buttons:SfSegmentedControl 
        EnableLiquidGlassEffect="True"
        Background="Transparent">
        <!-- Items -->
    </buttons:SfSegmentedControl>
</core:SfGlassEffectView>
```

#### Step 2: Set EnableLiquidGlassEffect Property

**XAML:**
```xml
<Grid>
    <!-- Background image for glass effect -->
    <Image Source="wallpaper.jpg" Aspect="AspectFill"/>
    
    <core:SfGlassEffectView EffectType="Clear">
        <buttons:SfSegmentedControl 
            EnableLiquidGlassEffect="True"
            Stroke="#1F000000"
            Background="#EDEDED"
            StrokeThickness="1">
            <buttons:SfSegmentedControl.SelectionIndicatorSettings>
                <buttons:SelectionIndicatorSettings 
                    TextColor="#0088FF" 
                    Background="#19F2F2F7"/>
            </buttons:SfSegmentedControl.SelectionIndicatorSettings>
            <buttons:SfSegmentedControl.ItemsSource>
                <x:Array Type="{x:Type x:String}">
                    <x:String>Day</x:String>
                    <x:String>Week</x:String>
                    <x:String>Month</x:String>
                </x:Array>
            </buttons:SfSegmentedControl.ItemsSource>
        </buttons:SfSegmentedControl>
    </core:SfGlassEffectView>
</Grid>
```

**C#:**
```csharp
using Syncfusion.Maui.Core;
using Syncfusion.Maui.Buttons;

var backgroundImage = new Image
{
    Source = "wallpaper.jpg",
    Aspect = Aspect.AspectFill
};

var glassView = new SfGlassEffectView
{
    EffectType = LiquidGlassEffectType.Clear
};

var segmentedControl = new SfSegmentedControl
{
    EnableLiquidGlassEffect = true,
    Stroke = Color.FromArgb("#1F000000"),
    Background = Color.FromArgb("#EDEDED"),
    StrokeThickness = 1,
    ItemsSource = new List<SfSegmentItem>
    {
        new SfSegmentItem { Text = "Day" },
        new SfSegmentItem { Text = "Week" },
        new SfSegmentItem { Text = "Month" }
    }
};

segmentedControl.SelectionIndicatorSettings = new SelectionIndicatorSettings
{
    TextColor = Color.FromArgb("#0088FF"),
    Background = Color.FromArgb("#19F2F2F7")
};

glassView.Content = segmentedControl;

var grid = new Grid();
grid.Children.Add(backgroundImage);
grid.Children.Add(glassView);

this.Content = grid;
```

### Glass Effect Types

The `SfGlassEffectView` supports multiple effect types:

- **Clear:** Transparent glass with subtle blur
- **Frosted:** Frosted glass with more blur
- **Tinted:** Glass with color tint
- **Adaptive:** Adapts based on background

**Example:**
```xml
<core:SfGlassEffectView EffectType="Frosted">
    <buttons:SfSegmentedControl EnableLiquidGlassEffect="True">
        <!-- Items -->
    </buttons:SfSegmentedControl>
</core:SfGlassEffectView>
```

### Best Practices for Glass Effect

1. **Background:** Use visually interesting backgrounds (images, gradients)
2. **Transparency:** Set semi-transparent backgrounds on segments
3. **Contrast:** Ensure text remains readable over blurred backgrounds
4. **Performance:** Test on target devices (effect may be resource-intensive)

### Platform Limitations

- **iOS 26+:** Full support
- **macOS 26+:** Full support
- **Android/Windows:** Not supported (falls back to standard appearance)

### Fallback for Unsupported Platforms

```csharp
public void ApplyGlassEffectIfSupported()
{
    bool supportsGlassEffect = DeviceInfo.Platform == DevicePlatform.iOS 
                                || DeviceInfo.Platform == DevicePlatform.macOS;
    
    if (supportsGlassEffect && DeviceInfo.Version.Major >= 26)
    {
        segmentedControl.EnableLiquidGlassEffect = true;
        // Apply glass-specific styling
    }
    else
    {
        // Use standard styling for other platforms
        segmentedControl.Background = Color.FromArgb("#FFFFFF");
    }
}
```

## Platform-Specific Considerations

### iOS

- **Touch targets:** Ensure minimum 44x44 points
- **Safe areas:** Account for notches and home indicators
- **Dark mode:** Test appearance in both light and dark modes

### Android

- **Touch targets:** Ensure minimum 48x48 dp
- **Material Design:** Consider Material Design guidelines for segmented buttons
- **API levels:** Test on various Android versions

### Windows

- **Mouse interaction:** Hover states may differ
- **Keyboard navigation:** Support Tab key navigation
- **High contrast:** Test with high contrast themes

### macOS

- **Native feel:** Match macOS UI conventions
- **Trackpad gestures:** Consider swipe gestures for scrolling
- **Accessibility:** Support VoiceOver

## Accessibility Features

### Screen Reader Support

Segments are automatically announced by screen readers with their text content.

**Improve accessibility:**
```csharp
var segmentedControl = new SfSegmentedControl
{
    ItemsSource = new List<SfSegmentItem>
    {
        new SfSegmentItem 
        { 
            Text = "Day",
            // Automation ID for testing
            AutomationId = "DaySegment"
        }
    }
};
```

### Keyboard Navigation

Support keyboard navigation (desktop platforms):
- **Tab:** Focus on control
- **Arrow keys:** Navigate between segments
- **Space/Enter:** Select focused segment

### High Contrast Modes

Test in high contrast modes to ensure visibility:

```csharp
public void ApplyAccessibleColors()
{
    // Check for high contrast mode
    bool isHighContrast = Application.Current.RequestedTheme == AppTheme.Light;
    
    if (isHighContrast)
    {
        segmentedControl.Stroke = Colors.Black;
        segmentedControl.SelectionIndicatorSettings = new SelectionIndicatorSettings
        {
            Background = Colors.Yellow,
            TextColor = Colors.Black
        };
    }
}
```

## Performance Optimization

### Minimize Redraws

Avoid frequent updates to ItemsSource:

```csharp
// ✗ Bad: Creates new collection on every call
segmentedControl.ItemsSource = GetSegments();

// ✓ Good: Update once
var segments = GetSegments();
segmentedControl.ItemsSource = segments;
```

### Lazy Loading for Scrollable Segments

For many segments, consider lazy loading:

```csharp
public void LoadSegmentsAsNeeded(int visibleCount)
{
    var initialSegments = allSegments.Take(visibleCount).ToList();
    segmentedControl.ItemsSource = initialSegments;
    
    // Load more as user scrolls (implement custom logic)
}
```

### Image Caching

Cache images to improve performance:

```csharp
var segmentedControl = new SfSegmentedControl
{
    ItemsSource = new List<SfSegmentItem>
    {
        new SfSegmentItem 
        { 
            ImageSource = ImageSource.FromFile("icon.png"),
            // Images are cached by default in MAUI
        }
    }
};
```

## Custom Animations

While the control provides built-in ripple animation, you can add custom animations:

```csharp
segmentedControl.SelectionChanged += async (sender, e) =>
{
    // Custom fade animation on selection
    await segmentedControl.FadeTo(0.5, 100);
    await segmentedControl.FadeTo(1.0, 100);
};
```

## Troubleshooting

### RTL Not Working

**Cause:** FlowDirection not set or overridden by parent  
**Solution:** Set FlowDirection explicitly on SfSegmentedControl

### Glass Effect Not Visible

**Cause:** Platform not supported or .NET version < 10  
**Solution:** Check platform and .NET version, add fallback styling

### Performance Issues with Glass Effect

**Cause:** Complex background or too many segments  
**Solution:** Simplify background, reduce visible segments, test on device

### Keyboard Navigation Not Working

**Cause:** Control not focusable or platform doesn't support  
**Solution:** Test on desktop platforms, ensure control is in visual tree

## Next Steps

- **Getting started:** See [getting-started.md](getting-started.md) for initial setup
- **Customization:** See [customization.md](customization.md) for appearance options
- **Events:** See [events.md](events.md) for handling user interactions
