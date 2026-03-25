# Title Configuration

The Busy Indicator supports displaying text titles with extensive customization options including placement, spacing, color, and font properties. This guide covers all title-related features.

## Table of Contents
- [Setting the Title](#setting-the-title)
- [Text Color](#text-color)
- [Title Placement](#title-placement)
- [Title Spacing](#title-spacing)
- [Font Customization](#font-customization)
- [Font Auto-Scaling](#font-auto-scaling)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)

## Setting the Title

The `Title` property displays informative text alongside the busy indicator to provide context about the loading operation.

### Basic Title

**XAML:**
```xml
<core:SfBusyIndicator IsRunning="True"
                      AnimationType="CircularMaterial"
                      Title="Loading..." />
```

**C#:**
```csharp
SfBusyIndicator busyIndicator = new SfBusyIndicator
{
    IsRunning = true,
    AnimationType = AnimationType.CircularMaterial,
    Title = "Loading..."
};
```

### Dynamic Titles

```csharp
// Update title based on operation
public void StartOperation(string operationName)
{
    busyIndicator.Title = $"Loading {operationName}...";
    busyIndicator.IsRunning = true;
}

// Examples
StartOperation("user profile");      // "Loading user profile..."
StartOperation("dashboard data");    // "Loading dashboard data..."
StartOperation("settings");          // "Loading settings..."
```

### Title with Progress Information

```csharp
private int _currentStep = 0;
private int _totalSteps = 5;

public async Task ProcessStepsAsync()
{
    for (_currentStep = 1; _currentStep <= _totalSteps; _currentStep++)
    {
        busyIndicator.Title = $"Processing step {_currentStep} of {_totalSteps}...";
        await ProcessStepAsync(_currentStep);
    }
    
    busyIndicator.IsRunning = false;
}
```

## Text Color

The `TextColor` property controls the color of the title text.

### Basic Color

**XAML:**
```xml
<core:SfBusyIndicator IsRunning="True"
                      AnimationType="CircularMaterial"
                      Title="Loading..."
                      TextColor="Red" />
```

**C#:**
```csharp
SfBusyIndicator busyIndicator = new SfBusyIndicator
{
    IsRunning = true,
    AnimationType = AnimationType.CircularMaterial,
    Title = "Loading...",
    TextColor = Colors.Red
};
```

### Using Hex Colors

**XAML:**
```xml
<core:SfBusyIndicator Title="Loading..."
                      TextColor="#512BD4" />
```

**C#:**
```csharp
busyIndicator.TextColor = Color.FromArgb("#512BD4");
```

### Matching Brand Colors

```xml
<core:SfBusyIndicator IsRunning="True"
                      AnimationType="CircularMaterial"
                      Title="Loading your data..."
                      IndicatorColor="#FF6B35"
                      TextColor="#FF6B35" />
```

### Contrast for Dark Backgrounds

```xml
<core:SfBusyIndicator IsRunning="True"
                      Title="Processing..."
                      IndicatorColor="White"
                      TextColor="White"
                      OverlayFill="#CC000000" />
```

### Theme-Aware Colors

```csharp
public void ApplyThemeColors(bool isDarkMode)
{
    if (isDarkMode)
    {
        busyIndicator.TextColor = Colors.White;
        busyIndicator.IndicatorColor = Colors.White;
    }
    else
    {
        busyIndicator.TextColor = Color.FromArgb("#333333");
        busyIndicator.IndicatorColor = Color.FromArgb("#512BD4");
    }
}
```

## Title Placement

The `TitlePlacement` property determines where the title appears relative to the indicator. It accepts three values:

- `Top` - Title above indicator
- `Bottom` - Title below indicator (default)
- `None` - No title displayed

### Bottom Placement (Default)

**XAML:**
```xml
<core:SfBusyIndicator IsRunning="True"
                      Title="Loading..."
                      TitlePlacement="Bottom" />
```

**C#:**
```csharp
busyIndicator.TitlePlacement = BusyIndicatorTitlePlacement.Bottom;
```

### Top Placement

**XAML:**
```xml
<core:SfBusyIndicator IsRunning="True"
                      AnimationType="CircularMaterial"
                      Title="Loading..."
                      TextColor="Red"
                      TitlePlacement="Top" />
```

**C#:**
```csharp
SfBusyIndicator busyIndicator = new SfBusyIndicator
{
    IsRunning = true,
    AnimationType = AnimationType.CircularMaterial,
    Title = "Loading...",
    TextColor = Colors.Red,
    TitlePlacement = BusyIndicatorTitlePlacement.Top
};
```

### Hide Title

To hide the title while keeping the `Title` property set:

**XAML:**
```xml
<core:SfBusyIndicator IsRunning="True"
                      Title="Loading..."
                      TitlePlacement="None" />
```

**C#:**
```csharp
busyIndicator.TitlePlacement = BusyIndicatorTitlePlacement.None;
```

### Dynamic Placement

```csharp
public void ToggleTitlePosition()
{
    busyIndicator.TitlePlacement = busyIndicator.TitlePlacement == BusyIndicatorTitlePlacement.Top
        ? BusyIndicatorTitlePlacement.Bottom
        : BusyIndicatorTitlePlacement.Top;
}
```

### Context-Based Placement

```csharp
public void SetPlacementByContext(string layoutType)
{
    busyIndicator.TitlePlacement = layoutType switch
    {
        "top-bar" => BusyIndicatorTitlePlacement.Bottom,
        "center" => BusyIndicatorTitlePlacement.Bottom,
        "full-screen" => BusyIndicatorTitlePlacement.Bottom,
        "minimal" => BusyIndicatorTitlePlacement.None,
        _ => BusyIndicatorTitlePlacement.Bottom
    };
}
```

## Title Spacing

The `TitleSpacing` property controls the space between the indicator and the title text, specified in device-independent units.

### Basic Spacing

**XAML:**
```xml
<core:SfBusyIndicator IsRunning="True"
                      AnimationType="CircularMaterial"
                      Title="Loading..."
                      TextColor="Red"
                      TitlePlacement="Top"
                      TitleSpacing="20" />
```

**C#:**
```csharp
SfBusyIndicator busyIndicator = new SfBusyIndicator
{
    IsRunning = true,
    AnimationType = AnimationType.CircularMaterial,
    Title = "Loading...",
    TextColor = Colors.Red,
    TitlePlacement = BusyIndicatorTitlePlacement.Top,
    TitleSpacing = 20
};
```

### Spacing Examples

```xml
<!-- Compact spacing -->
<core:SfBusyIndicator Title="Loading..." TitleSpacing="5" />

<!-- Default spacing -->
<core:SfBusyIndicator Title="Loading..." TitleSpacing="8" />

<!-- Comfortable spacing -->
<core:SfBusyIndicator Title="Loading..." TitleSpacing="15" />

<!-- Generous spacing -->
<core:SfBusyIndicator Title="Loading..." TitleSpacing="25" />
```

### Responsive Spacing

```csharp
public void AdaptSpacing()
{
    var screenWidth = DeviceDisplay.MainDisplayInfo.Width;
    
    busyIndicator.TitleSpacing = screenWidth switch
    {
        < 600 => 10,   // Phone
        < 900 => 15,   // Small tablet
        < 1200 => 20,  // Tablet
        _ => 25        // Desktop
    };
}
```

### Spacing by Size Factor

```csharp
// Proportional spacing based on indicator size
public void SetProportionalSpacing(double sizeFactor)
{
    busyIndicator.SizeFactor = sizeFactor;
    busyIndicator.TitleSpacing = sizeFactor * 30; // Scale spacing with size
}
```

## Font Customization

The Busy Indicator provides comprehensive font customization through several properties.

### Font Size

**XAML:**
```xml
<core:SfBusyIndicator IsRunning="True"
                      Title="Loading..."
                      FontSize="16" />
```

**C#:**
```csharp
busyIndicator.FontSize = 16;
```

### Font Attributes (Bold/Italic)

**XAML:**
```xml
<core:SfBusyIndicator IsRunning="True"
                      AnimationType="CircularMaterial"
                      Title="Loading..."
                      TextColor="Red"
                      FontSize="16"
                      FontAttributes="Bold" />
```

**C#:**
```csharp
SfBusyIndicator busyIndicator = new SfBusyIndicator
{
    IsRunning = true,
    AnimationType = AnimationType.CircularMaterial,
    Title = "Loading...",
    TextColor = Colors.Red,
    FontSize = 16,
    FontAttributes = FontAttributes.Bold
};
```

**Available FontAttributes:**
- `None` - Regular text (default)
- `Bold` - Bold text
- `Italic` - Italic text
- `Bold | Italic` - Bold and italic combined

### Bold and Italic Combined

**XAML:**
```xml
<core:SfBusyIndicator Title="Loading..."
                      FontAttributes="Bold, Italic" />
```

**C#:**
```csharp
busyIndicator.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### Font Family

**XAML:**
```xml
<core:SfBusyIndicator IsRunning="True"
                      AnimationType="CircularMaterial"
                      Title="Loading..."
                      TextColor="Red"
                      FontSize="16"
                      FontAttributes="Bold"
                      FontFamily="serif" />
```

**C#:**
```csharp
SfBusyIndicator busyIndicator = new SfBusyIndicator
{
    IsRunning = true,
    AnimationType = AnimationType.CircularMaterial,
    Title = "Loading...",
    TextColor = Colors.Red,
    FontSize = 16,
    FontAttributes = FontAttributes.Bold,
    FontFamily = "serif"
};
```

### Custom Font Families

```xml
<!-- Use built-in fonts -->
<core:SfBusyIndicator Title="Loading..." FontFamily="Arial" />
<core:SfBusyIndicator Title="Loading..." FontFamily="Times New Roman" />

<!-- Use custom fonts (must be registered in MauiProgram.cs) -->
<core:SfBusyIndicator Title="Loading..." FontFamily="CustomFont" />
```

### Platform-Specific Fonts

```csharp
public void ApplyPlatformFont()
{
    if (DeviceInfo.Platform == DevicePlatform.iOS)
        busyIndicator.FontFamily = "San Francisco";
    else if (DeviceInfo.Platform == DevicePlatform.Android)
        busyIndicator.FontFamily = "Roboto";
    else if (DeviceInfo.Platform == DevicePlatform.WinUI)
        busyIndicator.FontFamily = "Segoe UI";
}
```

## Font Auto-Scaling

The `FontAutoScalingEnabled` property allows the title text to scale according to the device's accessibility settings.

### Enable Auto-Scaling

**XAML:**
```xml
<core:SfBusyIndicator FontAutoScalingEnabled="True" />
```

**C#:**
```csharp
SfBusyIndicator busyIndicator = new SfBusyIndicator
{
    FontAutoScalingEnabled = true
};
```

### When to Use Auto-Scaling

**Enable when:**
- Accessibility is a priority
- Supporting users with visual impairments
- Following platform accessibility guidelines
- Large text settings should affect your UI

**Disable when:**
- Fixed layout requirements
- Design constraints prevent text scaling
- Title must remain a specific size

### Auto-Scaling with Size Constraints

```csharp
// Enable auto-scaling but set reasonable bounds
busyIndicator.FontAutoScalingEnabled = true;
busyIndicator.FontSize = 14; // Base size

// Note: Actual rendered size will scale with OS settings
// You may need to test with different text size settings
```

## Complete Examples

### Example 1: Branded Loading Screen

```xml
<core:SfBusyIndicator IsRunning="True"
                      AnimationType="Globe"
                      IndicatorColor="White"
                      SizeFactor="0.7"
                      Title="Loading Your Experience..."
                      TitlePlacement="Bottom"
                      TitleSpacing="20"
                      TextColor="White"
                      FontSize="18"
                      FontAttributes="Bold"
                      FontFamily="Arial">
    <core:SfBusyIndicator.OverlayFill>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="#512BD4" Offset="0.0" />
            <GradientStop Color="#8B5CF6" Offset="1.0" />
        </LinearGradientBrush>
    </core:SfBusyIndicator.OverlayFill>
</core:SfBusyIndicator>
```

### Example 2: Minimal iOS-Style Loader

```xml
<core:SfBusyIndicator IsRunning="True"
                      AnimationType="Cupertino"
                      IndicatorColor="#007AFF"
                      SizeFactor="0.5"
                      Title="Loading"
                      TitlePlacement="Bottom"
                      TitleSpacing="12"
                      TextColor="#8E8E93"
                      FontSize="14"
                      FontFamily="San Francisco" />
```

### Example 3: Material Design Loader

```xml
<core:SfBusyIndicator IsRunning="True"
                      AnimationType="CircularMaterial"
                      IndicatorColor="#6200EE"
                      SizeFactor="0.6"
                      Title="Please wait"
                      TitlePlacement="Bottom"
                      TitleSpacing="16"
                      TextColor="#6200EE"
                      FontSize="16"
                      FontAttributes="Bold"
                      FontFamily="Roboto" />
```

### Example 4: Accessible Loader

```xml
<core:SfBusyIndicator IsRunning="True"
                      AnimationType="CircularMaterial"
                      Title="Loading content"
                      TitlePlacement="Bottom"
                      FontSize="16"
                      FontAutoScalingEnabled="True"
                      TextColor="#333333" />
```

### Example 5: Dynamic Title Updates

```csharp
public class LoadingViewModel : INotifyPropertyChanged
{
    private bool _isLoading;
    private string _loadingTitle = "Initializing...";
    
    public bool IsLoading
    {
        get => _isLoading;
        set
        {
            _isLoading = value;
            OnPropertyChanged(nameof(IsLoading));
        }
    }
    
    public string LoadingTitle
    {
        get => _loadingTitle;
        set
        {
            _loadingTitle = value;
            OnPropertyChanged(nameof(LoadingTitle));
        }
    }
    
    public async Task LoadDataAsync()
    {
        IsLoading = true;
        
        try
        {
            LoadingTitle = "Connecting to server...";
            await Task.Delay(1000);
            
            LoadingTitle = "Fetching data...";
            await FetchDataAsync();
            
            LoadingTitle = "Processing results...";
            await ProcessDataAsync();
            
            LoadingTitle = "Almost done...";
            await Task.Delay(500);
        }
        finally
        {
            IsLoading = false;
        }
    }
}
```

```xml
<core:SfBusyIndicator IsRunning="{Binding IsLoading}"
                      Title="{Binding LoadingTitle}"
                      AnimationType="CircularMaterial"
                      TitlePlacement="Bottom"
                      FontSize="16"
                      TextColor="#512BD4" />
```

## Best Practices

### Title Content Guidelines

**Do:**
- Keep titles concise (< 50 characters)
- Use action-oriented language ("Loading...", "Processing...", "Syncing...")
- Provide context about what's loading
- End with ellipsis (...) to indicate ongoing action
- Update title for multi-step operations

**Don't:**
- Use overly technical language
- Display error messages as titles (use dialogs instead)
- Create unnecessarily long titles
- Use ALL CAPS (except for acronyms)

### Font Size Guidelines

| Context | Recommended Font Size |
|---------|----------------------|
| Mobile (inline) | 12-14 |
| Mobile (prominent) | 14-16 |
| Tablet | 16-18 |
| Desktop | 16-20 |
| Large displays | 20-24 |

### Spacing Guidelines

| Indicator Size | Recommended Spacing |
|----------------|---------------------|
| Small (0.2-0.4) | 8-12 |
| Medium (0.4-0.6) | 12-16 |
| Large (0.6-0.8) | 16-25 |
| Very large (0.8-1.0) | 25-35 |

### Color Contrast

Ensure sufficient contrast between text and background:
- **Light backgrounds:** Use dark text colors (#333333, #000000)
- **Dark backgrounds:** Use light text colors (#FFFFFF, #F5F5F5)
- **Colored overlays:** Match or contrast with overlay color
- **WCAG compliance:** Aim for at least 4.5:1 contrast ratio

### Accessibility

1. **Enable auto-scaling** when possible
2. **Use readable font sizes** (minimum 12-14)
3. **Ensure color contrast** meets WCAG guidelines
4. **Provide meaningful titles** that screen readers can announce
5. **Avoid relying solely on color** to convey information

## Edge Cases

### Long Titles

```csharp
// Truncate long titles
public void SetTitle(string title)
{
    const int maxLength = 50;
    busyIndicator.Title = title.Length > maxLength 
        ? title.Substring(0, maxLength - 3) + "..." 
        : title;
}
```

### Empty or Null Titles

```xml
<!-- Title property can be null or empty -->
<core:SfBusyIndicator IsRunning="True" Title="" />
<core:SfBusyIndicator IsRunning="True" Title="{x:Null}" />
```

### Multi-Line Titles

Titles are single-line by default. For multi-line scenarios, consider using a separate Label:

```xml
<Grid>
    <StackLayout VerticalOptions="Center" HorizontalOptions="Center">
        <core:SfBusyIndicator IsRunning="True"
                              TitlePlacement="None"
                              SizeFactor="0.6" />
        <Label Text="Loading your data&#x0a;This may take a moment"
               HorizontalTextAlignment="Center"
               Margin="0,20,0,0" />
    </StackLayout>
</Grid>
```

### Very Small Indicators

When `SizeFactor < 0.3`, consider hiding the title or using a smaller font:

```csharp
public void AdaptTitleToSize(double sizeFactor)
{
    if (sizeFactor < 0.3)
    {
        busyIndicator.TitlePlacement = BusyIndicatorTitlePlacement.None;
    }
    else
    {
        busyIndicator.TitlePlacement = BusyIndicatorTitlePlacement.Bottom;
        busyIndicator.FontSize = sizeFactor * 30; // Proportional sizing
    }
}
```
