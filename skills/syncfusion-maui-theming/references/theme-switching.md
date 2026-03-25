# Theme Switching

Dynamic runtime theme switching in Syncfusion .NET MAUI applications.

## Overview

Theme switching allows users to change between light and dark themes (or different theme variants) while the app is running, without restarting. This is commonly used for:

- Dark mode toggles
- User preference settings
- Time-based automatic switching (day/night)
- System theme synchronization

## Basic Theme Switching

### Switching in Code-Behind

```csharp
private void SwitchTheme(string newTheme)
{
    var mergedDictionaries = Application.Current.Resources.MergedDictionaries;
    
    // Find and remove existing theme
    var existingTheme = mergedDictionaries
        .OfType<SyncfusionThemeResourceDictionary>()
        .FirstOrDefault();
    
    if (existingTheme != null)
    {
        mergedDictionaries.Remove(existingTheme);
    }
    
    // Add new theme
    var newThemeDict = new SyncfusionThemeResourceDictionary
    {
        VisualTheme = newTheme
    };
    mergedDictionaries.Add(newThemeDict);
}

// Usage
private void OnDarkModeToggled(object sender, EventArgs e)
{
    SwitchTheme("MaterialDark");
}
```

### Switching with Enum

```csharp
using Syncfusion.Maui.Themes;

private void ApplyTheme(SfVisuals theme)
{
    var mergedDictionaries = Application.Current.Resources.MergedDictionaries;
    
    var existingTheme = mergedDictionaries
        .OfType<SyncfusionThemeResourceDictionary>()
        .FirstOrDefault();
    
    if (existingTheme != null)
    {
        existingTheme.VisualTheme = theme;
    }
    else
    {
        mergedDictionaries.Add(new SyncfusionThemeResourceDictionary 
        { 
            VisualTheme = theme 
        });
    }
}

// Usage
ApplyTheme(SfVisuals.MaterialDark);
ApplyTheme(SfVisuals.CupertinoLight);
```

## ViewModel Pattern

### ThemeService

Create a reusable service for theme management:

```csharp
public class ThemeService
{
    private const string ThemePreferenceKey = "AppTheme";
    
    public void SetTheme(SfVisuals theme)
    {
        var mergedDictionaries = Application.Current.Resources.MergedDictionaries;
        
        var existingTheme = mergedDictionaries
            .OfType<SyncfusionThemeResourceDictionary>()
            .FirstOrDefault();
        
        if (existingTheme != null)
        {
            mergedDictionaries.Remove(existingTheme);
        }
        
        mergedDictionaries.Add(new SyncfusionThemeResourceDictionary 
        { 
            VisualTheme = theme 
        });
        
        // Save preference
        Preferences.Set(ThemePreferenceKey, theme.ToString());
    }
    
    public SfVisuals GetCurrentTheme()
    {
        var themeName = Preferences.Get(ThemePreferenceKey, "MaterialLight");
        return Enum.Parse<SfVisuals>(themeName);
    }
    
    public void LoadSavedTheme()
    {
        var savedTheme = GetCurrentTheme();
        SetTheme(savedTheme);
    }
}
```

### SettingsViewModel

```csharp
public class SettingsViewModel : INotifyPropertyChanged
{
    private readonly ThemeService _themeService;
    private bool _isDarkMode;
    
    public SettingsViewModel(ThemeService themeService)
    {
        _themeService = themeService;
        
        // Load current theme
        var current = _themeService.GetCurrentTheme();
        _isDarkMode = current == SfVisuals.MaterialDark || 
                      current == SfVisuals.CupertinoDark;
    }
    
    public bool IsDarkMode
    {
        get => _isDarkMode;
        set
        {
            if (_isDarkMode != value)
            {
                _isDarkMode = value;
                OnPropertyChanged();
                
                // Apply theme
                var theme = value ? SfVisuals.MaterialDark : SfVisuals.MaterialLight;
                _themeService.SetTheme(theme);
            }
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### XAML Binding

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             x:Class="YourApp.SettingsPage">
    <StackLayout Padding="20">
        <Label Text="Appearance" FontSize="Title"/>
        
        <HorizontalStackLayout Spacing="10" Margin="0,20,0,0">
            <Label Text="Dark Mode" VerticalOptions="Center"/>
            <Switch IsToggled="{Binding IsDarkMode}"/>
        </HorizontalStackLayout>
    </StackLayout>
</ContentPage>
```

## System Theme Synchronization

### Detect and Apply System Theme

```csharp
public class ThemeService
{
    public void ApplySystemTheme()
    {
        var systemTheme = Application.Current.RequestedTheme;
        var syncfusionTheme = systemTheme == AppTheme.Dark 
            ? SfVisuals.MaterialDark 
            : SfVisuals.MaterialLight;
        
        SetTheme(syncfusionTheme);
    }
    
    public void SyncWithSystem(bool enable)
    {
        if (enable)
        {
            ApplySystemTheme();
            Preferences.Set("SyncWithSystem", true);
        }
        else
        {
            Preferences.Set("SyncWithSystem", false);
        }
    }
}
```

### Monitor System Theme Changes

```csharp
// In App.xaml.cs
public partial class App : Application
{
    private readonly ThemeService _themeService;
    
    public App(ThemeService themeService)
    {
        InitializeComponent();
        _themeService = themeService;
        
        // Load saved or system theme
        if (Preferences.Get("SyncWithSystem", true))
        {
            _themeService.ApplySystemTheme();
        }
        else
        {
            _themeService.LoadSavedTheme();
        }
    }
    
    protected override void OnResume()
    {
        base.OnResume();
        
        // Re-sync with system if enabled
        if (Preferences.Get("SyncWithSystem", true))
        {
            _themeService.ApplySystemTheme();
        }
    }
}
```

## Time-Based Theme Switching

### Automatic Day/Night Themes

```csharp
public class ThemeService
{
    private Timer _themeTimer;
    
    public void EnableAutomaticThemeSwitching(TimeSpan dayStart, TimeSpan nightStart)
    {
        _themeTimer = new Timer(_ => CheckAndApplyTimeBasedTheme(dayStart, nightStart), 
                                null, 
                                TimeSpan.Zero, 
                                TimeSpan.FromMinutes(30));
    }
    
    private void CheckAndApplyTimeBasedTheme(TimeSpan dayStart, TimeSpan nightStart)
    {
        var now = DateTime.Now.TimeOfDay;
        
        var shouldBeDark = now < dayStart || now >= nightStart;
        var theme = shouldBeDark ? SfVisuals.MaterialDark : SfVisuals.MaterialLight;
        
        MainThread.BeginInvokeOnMainThread(() => SetTheme(theme));
    }
    
    public void DisableAutomaticThemeSwitching()
    {
        _themeTimer?.Dispose();
        _themeTimer = null;
    }
}

// Usage
themeService.EnableAutomaticThemeSwitching(
    dayStart: new TimeSpan(6, 0, 0),    // 6 AM
    nightStart: new TimeSpan(18, 0, 0)  // 6 PM
);
```

## Theme Picker UI

### Multiple Theme Options

```csharp
public class SettingsViewModel : INotifyPropertyChanged
{
    private SfVisuals _selectedTheme;
    
    public ObservableCollection<ThemeOption> AvailableThemes { get; }
    
    public SettingsViewModel(ThemeService themeService)
    {
        AvailableThemes = new ObservableCollection<ThemeOption>
        {
            new ThemeOption("Material Light", SfVisuals.MaterialLight),
            new ThemeOption("Material Dark", SfVisuals.MaterialDark),
            new ThemeOption("Cupertino Light", SfVisuals.CupertinoLight),
            new ThemeOption("Cupertino Dark", SfVisuals.CupertinoDark)
        };
        
        _selectedTheme = themeService.GetCurrentTheme();
    }
    
    public SfVisuals SelectedTheme
    {
        get => _selectedTheme;
        set
        {
            if (_selectedTheme != value)
            {
                _selectedTheme = value;
                OnPropertyChanged();
                _themeService.SetTheme(value);
            }
        }
    }
}

public class ThemeOption
{
    public string Name { get; }
    public SfVisuals Theme { get; }
    
    public ThemeOption(string name, SfVisuals theme)
    {
        Name = name;
        Theme = theme;
    }
}
```

```xml
<Picker ItemsSource="{Binding AvailableThemes}"
        ItemDisplayBinding="{Binding Name}"
        SelectedItem="{Binding SelectedTheme}"/>
```

## Handling Theme Transitions

### Smooth Transition

```csharp
public async Task SetThemeWithTransitionAsync(SfVisuals theme)
{
    var mainPage = Application.Current.MainPage;
    
    // Fade out
    await mainPage.FadeTo(0, 150);
    
    // Switch theme
    SetTheme(theme);
    
    // Fade in
    await mainPage.FadeTo(1, 150);
}
```

## Edge Cases and Gotchas

### Multiple Theme Dictionaries

**Problem:** Accidentally adding multiple theme dictionaries

```csharp
// ❌ Don't do this - creates conflicts
mergedDictionaries.Add(new SyncfusionThemeResourceDictionary { VisualTheme = "MaterialLight" });
mergedDictionaries.Add(new SyncfusionThemeResourceDictionary { VisualTheme = "MaterialDark" });
```

**Solution:** Always remove existing before adding new

```csharp
// ✅ Correct approach
var existing = mergedDictionaries.OfType<SyncfusionThemeResourceDictionary>().ToList();
foreach (var theme in existing)
{
    mergedDictionaries.Remove(theme);
}
mergedDictionaries.Add(new SyncfusionThemeResourceDictionary { VisualTheme = newTheme });
```

### Thread Safety

**Problem:** Theme switching from background thread

```csharp
// ❌ May crash if not on UI thread
Task.Run(() => SetTheme(SfVisuals.MaterialDark));
```

**Solution:** Ensure UI thread

```csharp
// ✅ Correct
MainThread.BeginInvokeOnMainThread(() => SetTheme(SfVisuals.MaterialDark));
```

### Custom Overrides Lost

**Problem:** Custom color overrides disappear after theme switch

**Solution:** Re-apply overrides after switching, or include them in theme dictionary

```csharp
private void SetThemeWithCustomizations(SfVisuals theme)
{
    var mergedDictionaries = Application.Current.Resources.MergedDictionaries;
    
    // Remove old theme
    var existing = mergedDictionaries.OfType<SyncfusionThemeResourceDictionary>().FirstOrDefault();
    if (existing != null) mergedDictionaries.Remove(existing);
    
    // Add new theme
    mergedDictionaries.Add(new SyncfusionThemeResourceDictionary { VisualTheme = theme });
    
    // Re-apply customizations
    ApplyCustomColors();
}
```

## Best Practices

1. **Remove Before Adding**: Always remove existing theme before adding new
2. **Save Preferences**: Persist user's theme choice
3. **Use Service Pattern**: Centralize theme logic in a service
4. **UI Thread**: Ensure theme changes happen on main thread
5. **Smooth Transitions**: Add fade effects for better UX
6. **Initial Load**: Apply saved theme in App constructor
7. **System Sync Option**: Provide option to follow system theme
8. **Test Switching**: Verify all controls update correctly

## Related Topics

- [Applying Themes](applying-themes.md) - Basic theme setup
- [Overriding Themes](overriding-themes.md) - Customizing colors
- [Creating Custom Themes](creating-custom-themes.md) - Building themes from scratch