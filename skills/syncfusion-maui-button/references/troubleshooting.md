# Troubleshooting .NET MAUI Button (SfButton)

This guide covers common issues, solutions, and best practices for implementing the Syncfusion .NET MAUI Button control.

## Common Issues and Solutions

### Handler Registration Errors

**Problem:** Application crashes with error: "Handler not found for view SfButton"

**Solution:**
Ensure `ConfigureSyncfusionCore()` is called in `MauiProgram.cs`:

```csharp
public static class MauiProgram
{
    public static MauiApp CreateMauiApp()
    {
        var builder = MauiApp.CreateBuilder();
        builder
            .UseMauiApp<App>()
            .ConfigureSyncfusionCore()  // ← Must be added
            .ConfigureFonts(fonts =>
            {
                fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
            });

        return builder.Build();
    }
}
```

**Important:** This must be called before `builder.Build()`.

---

### Image Not Displaying

**Problem:** Button icon or background image doesn't appear

**Common Causes and Solutions:**

**1. ShowIcon not enabled:**
```xml
<!-- ❌ Wrong: ShowIcon not set -->
<buttons:SfButton ImageSource="icon.png" />

<!-- ✓ Correct: ShowIcon enabled -->
<buttons:SfButton ShowIcon="True" ImageSource="icon.png" />
```

**2. Image not in Resources/Images folder:**

Ensure images are placed in:
```
YourProject/
└── Resources/
    └── Images/
        ├── icon.png
        ├── background.jpg
        └── ...
```

**3. Build action not set correctly:**

Check `.csproj` file:
```xml
<ItemGroup>
    <MauiImage Include="Resources\Images\icon.png" />
</ItemGroup>
```

**4. Incorrect file path:**
```csharp
// ✓ Correct: Just filename
button.ImageSource = "icon.png";

// ❌ Wrong: Full path
button.ImageSource = "Resources/Images/icon.png";
```

---

### Border Not Visible

**Problem:** `Stroke` property set but border doesn't appear

**Solution:**
Set `StrokeThickness` to a value greater than 0:

```xml
<!-- ❌ Wrong: StrokeThickness not set -->
<buttons:SfButton Stroke="Black" />

<!-- ✓ Correct: StrokeThickness specified -->
<buttons:SfButton Stroke="Black" StrokeThickness="2" />
```

---

### Background Color Not Working

**Problem:** `BackgroundColor` property doesn't change button color

**Solution:**
Use `Background` property instead of `BackgroundColor` for SfButton:

```xml
<!-- ❌ Wrong: Using BackgroundColor -->
<buttons:SfButton BackgroundColor="#6200EE" />

<!-- ✓ Correct: Using Background -->
<buttons:SfButton Background="#6200EE" />
```

```csharp
// ❌ Wrong
button.BackgroundColor = Colors.Blue;

// ✓ Correct
button.Background = Colors.Blue;
```

---

### Visual States Not Updating

**Problem:** Visual states defined but button appearance doesn't change

**Common Causes and Solutions:**

**1. Missing VisualStateGroup name:**
```xml
<!-- ❌ Wrong: No group name -->
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
        ...
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>

<!-- ✓ Correct: Group named "CommonStates" -->
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup x:Name="CommonStates">
        ...
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

**2. IsCheckable not enabled for Checked state:**
```xml
<!-- ❌ Wrong: Checked state without IsCheckable -->
<buttons:SfButton>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Checked">
                ...
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</buttons:SfButton>

<!-- ✓ Correct: IsCheckable enabled -->
<buttons:SfButton IsCheckable="True">
    ...
</buttons:SfButton>
```

**3. Using BackgroundColor instead of Background:**
```xml
<VisualState x:Name="Hovered">
    <VisualState.Setters>
        <!-- ❌ Wrong property -->
        <Setter Property="BackgroundColor" Value="Blue" />
        
        <!-- ✓ Correct property -->
        <Setter Property="Background" Value="Blue" />
    </VisualState.Setters>
</VisualState>
```

---

### Button Not Responding to Clicks

**Problem:** Clicked event not firing or button appears unresponsive

**Solutions:**

**1. Check IsEnabled property:**
```xml
<!-- ❌ Button disabled -->
<buttons:SfButton IsEnabled="False" Clicked="OnButtonClicked" />

<!-- ✓ Button enabled (default) -->
<buttons:SfButton Clicked="OnButtonClicked" />
```

**2. Verify event handler signature:**
```csharp
// ✓ Correct signature
private void OnButtonClicked(object sender, EventArgs e)
{
    // Handle click
}

// ❌ Wrong signature
private void OnButtonClicked()  // Missing parameters
{
    // Won't work
}
```

**3. Check for overlapping elements:**
```xml
<!-- ❌ Button might be covered -->
<Grid>
    <buttons:SfButton Text="Hidden" />
    <BoxView Color="Red" />  <!-- Covers button -->
</Grid>

<!-- ✓ Ensure button is on top -->
<Grid>
    <BoxView Color="Red" />
    <buttons:SfButton Text="Visible" />
</Grid>
```

---

### Layout and Sizing Issues

**Problem:** Button not displaying at expected size or position

**Solutions:**

**1. Set explicit size when needed:**
```xml
<!-- May not size correctly without constraints -->
<buttons:SfButton Text="Button" />

<!-- Better: Explicit size -->
<buttons:SfButton Text="Button"
                  WidthRequest="150"
                  HeightRequest="44" />
```

**2. Use proper layout options:**
```xml
<VerticalStackLayout>
    <!-- Button fills width -->
    <buttons:SfButton Text="Full Width"
                      HorizontalOptions="Fill" />
    
    <!-- Button centers with explicit width -->
    <buttons:SfButton Text="Centered"
                      WidthRequest="150"
                      HorizontalOptions="Center" />
</VerticalStackLayout>
```

**3. Check padding conflicts:**
```xml
<!-- Padding might make button appear incorrectly sized -->
<buttons:SfButton Text="Button"
                  Padding="50"
                  WidthRequest="100" />  <!-- Total width = 200 -->
```

---

### NuGet Package Errors

**Problem:** Package restore fails or version conflicts

**Solutions:**

**1. Clear NuGet cache:**
```bash
dotnet nuget locals all --clear
dotnet restore
```

**2. Check package version compatibility:**
```xml
<!-- Ensure compatible versions -->
<ItemGroup>
    <PackageReference Include="Syncfusion.Maui.Buttons" Version="27.*" />
    <PackageReference Include="Syncfusion.Maui.Core" Version="27.*" />
</ItemGroup>
```

**3. Update all Syncfusion packages together:**
```bash
dotnet add package Syncfusion.Maui.Buttons --version 27.1.48
```

---

### Platform-Specific Issues

#### iOS/macOS

**Problem:** Gradient backgrounds not rendering correctly

**Known Issue:** Framework-level clipping issue with gradients on iOS/macOS ([GitHub Issue](https://github.com/dotnet/maui/issues/18671))

**Workaround:**
Use solid colors instead of gradients on iOS/macOS, or wait for framework fix.

**Problem:** Liquid Glass effect not working

**Solution:**
- Verify running .NET 10+ with iOS 26 or macOS 26
- Check platform version at runtime:
  ```csharp
  #if IOS || MACCATALYST
  if (DeviceInfo.Version.Major >= 26)
  {
      button.EnableLiquidGlassEffect = true;
  }
  #endif
  ```

#### Android

**Problem:** Ripple effect not visible

**Solution:**
Ensure `EnableRippleEffect="True"` and button has solid background:
```xml
<buttons:SfButton Background="#6200EE"
                  EnableRippleEffect="True" />
```

#### Windows

**Problem:** Button doesn't show hover effects

**Solution:**
Define `Hovered` visual state explicitly for desktop platforms.

---

### Migration from Xamarin.Forms

**Problem:** Xamarin.Forms code doesn't work in MAUI

**API Changes:**

| Xamarin Property | MAUI Property | Change |
|------------------|---------------|--------|
| `BorderWidth` | `StrokeThickness` | Renamed |
| `BorderColor` | `Stroke` | Renamed |
| `BackgroundImage` | `BackgroundImageSource` | Renamed |
| `ImageWidth` | `ImageSize` | Renamed |

**Namespace Change:**
```csharp
// Xamarin
using Syncfusion.XForms.Buttons;

// MAUI
using Syncfusion.Maui.Buttons;
```

**XAML Namespace:**
```xml
<!-- Xamarin -->
xmlns:buttons="clr-namespace:Syncfusion.XForms.Buttons;assembly=Syncfusion.XForms.Buttons"

<!-- MAUI -->
xmlns:buttons="clr-namespace:Syncfusion.Maui.Buttons;assembly=Syncfusion.Maui.Buttons"
```

---

## Best Practices

### 1. Performance

**Avoid excessive visual states:**
```xml
<!-- ❌ Overkill -->
<VisualState x:Name="Normal">
    <VisualState.Setters>
        <Setter Property="Background" Value="Blue" />
        <Setter Property="TextColor" Value="White" />
        <Setter Property="FontSize" Value="14" />
        <Setter Property="Padding" Value="12" />
        <Setter Property="Margin" Value="5" />
        <!-- Too many setters -->
    </VisualState.Setters>
</VisualState>

<!-- ✓ Better -->
<VisualState x:Name="Normal">
    <VisualState.Setters>
        <Setter Property="Background" Value="Blue" />
        <Setter Property="TextColor" Value="White" />
    </VisualState.Setters>
</VisualState>
```

**Reuse button styles:**
```xml
<ContentPage.Resources>
    <Style x:Key="PrimaryButtonStyle" TargetType="buttons:SfButton">
        <Setter Property="Background" Value="#6200EE" />
        <Setter Property="TextColor" Value="White" />
        <Setter Property="CornerRadius" Value="8" />
        <Setter Property="HeightRequest" Value="44" />
    </Style>
</ContentPage.Resources>

<buttons:SfButton Text="Submit" Style="{StaticResource PrimaryButtonStyle}" />
<buttons:SfButton Text="Continue" Style="{StaticResource PrimaryButtonStyle}" />
```

### 2. Accessibility

**Provide semantic descriptions:**
```xml
<buttons:SfButton Text="Submit"
                  SemanticProperties.Description="Submit form data"
                  SemanticProperties.Hint="Double tap to submit" />
```

**Ensure sufficient touch target size:**
```xml
<!-- Minimum 44x44 for touch targets -->
<buttons:SfButton Text="Small"
                  HeightRequest="44"
                  WidthRequest="44" />
```

### 3. Error Handling

**Disable button during async operations:**
```csharp
private async void OnSubmit(object sender, EventArgs e)
{
    var button = sender as SfButton;
    button.IsEnabled = false;
    
    try
    {
        await SubmitDataAsync();
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", ex.Message, "OK");
    }
    finally
    {
        button.IsEnabled = true;
    }
}
```

### 4. Testing

**Test on multiple platforms:**
- Visual states behave differently on desktop vs. mobile
- Test gradients on iOS/macOS specifically
- Verify RTL layout with actual RTL languages

**Check different screen sizes:**
```csharp
// Responsive button sizing
var buttonWidth = DeviceDisplay.MainDisplayInfo.Width * 0.8;
myButton.WidthRequest = Math.Min(buttonWidth, 400);
```

---

## Diagnostic Checklist

When encountering issues, verify:

- [ ] `ConfigureSyncfusionCore()` called in `MauiProgram.cs`
- [ ] NuGet packages installed and restored
- [ ] Image files in `Resources/Images/` folder
- [ ] `ShowIcon="True"` for icon buttons
- [ ] `StrokeThickness > 0` for visible borders
- [ ] `Background` (not `BackgroundColor`) for button color
- [ ] `IsCheckable="True"` for toggle buttons with Checked state
- [ ] Event handler signatures match `(object sender, EventArgs e)`
- [ ] Button not covered by other elements
- [ ] `IsEnabled="True"` (default)

---

## Getting Help

If issues persist:

1. **Check documentation:** [Syncfusion MAUI Button Docs](https://help.syncfusion.com/maui/button/getting-started)
2. **View samples:** [GitHub Examples](https://github.com/SyncfusionExamples/maui-button-samples)
3. **Report issues:** [Syncfusion Support](https://www.syncfusion.com/support)
4. **Community forums:** Search for similar issues

---

## Quick Reference: Common Fixes

| Issue | Quick Fix |
|-------|-----------|
| Handler error | Add `ConfigureSyncfusionCore()` to `MauiProgram.cs` |
| Icon not showing | Set `ShowIcon="True"` |
| Border invisible | Set `StrokeThickness="2"` |
| Background not working | Use `Background` not `BackgroundColor` |
| Visual states not updating | Use `CommonStates` group name |
| Checked state not working | Set `IsCheckable="True"` |
| Button not clicking | Check `IsEnabled` and event handler signature |
| Image not found | Place in `Resources/Images/` |
| Gradient not rendering (iOS) | Known issue - use solid colors or wait for fix |
| Liquid Glass not working | Requires .NET 10+ with iOS 26/macOS 26 |
