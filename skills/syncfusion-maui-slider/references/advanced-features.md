# Advanced Features and Migration

This guide covers advanced slider features including discrete selection with StepSize, deferred updates, liquid glass effect, Visual State Manager.

## Table of Contents
- [Discrete Selection with StepSize](#discrete-selection-with-stepsize)
- [Deferred Update](#deferred-update)
- [IsEnabled Property](#isenabled-property)
- [Liquid Glass Effect](#liquid-glass-effect)
- [Visual State Manager](#visual-state-manager)

## Discrete Selection with StepSize

The `StepSize` property enables discrete value selection, moving the thumb in specific increments rather than continuously.

### Basic StepSize Configuration

```xml
<sliders:SfSlider Minimum="0"
                  Maximum="10"
                  Value="5"
                  StepSize="1"
                  Interval="2"
                  ShowLabels="True"
                  ShowTicks="True"
                  ShowDividers="True" />
```

```csharp
SfSlider slider = new SfSlider
{
    Minimum = 0,
    Maximum = 10,
    Value = 5,
    StepSize = 1,
    Interval = 2,
    ShowLabels = true,
    ShowTicks = true,
    ShowDividers = true
};
```

**Behavior**: The thumb will snap to values: 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 (increments of 1).

### StepSize Calculation

With StepSize set, the slider allows values at:
- Minimum
- Minimum + StepSize
- Minimum + (2 × StepSize)
- ... up to Maximum

**Example: Minimum = 20, Maximum = 40, StepSize = 5**
- Allowed values: 20, 25, 30, 35, 40

### Continuous vs Discrete

**Continuous (StepSize = 0, default):**
```xml
<sliders:SfSlider Minimum="0" Maximum="100" Value="50" />
```
The thumb can be positioned at any value between 0 and 100 (e.g., 43.7, 68.2).

**Discrete (StepSize > 0):**
```xml
<sliders:SfSlider Minimum="0" Maximum="100" Value="50" StepSize="10" />
```
The thumb snaps to: 0, 10, 20, 30, 40, 50, 60, 70, 80, 90, 100.

### Use Cases

**StepSize = 1:** Integer values only
```xml
<sliders:SfSlider Minimum="1" Maximum="10" StepSize="1" />
```
For ratings, counts, age selection.

**StepSize = 5:** Increments of 5
```xml
<sliders:SfSlider Minimum="0" Maximum="100" StepSize="5" />
```
For percentage adjustments in 5% steps.

**StepSize = 0.1:** Decimal precision
```xml
<sliders:SfSlider Minimum="0" Maximum="1" StepSize="0.1" />
```
For opacity, normalized values.

### StepSize with Interval

StepSize and Interval are independent:

```xml
<sliders:SfSlider Minimum="0"
                  Maximum="100"
                  Value="50"
                  StepSize="5"
                  Interval="25"
                  ShowLabels="True"
                  ShowTicks="True" />
```

- **StepSize = 5**: Thumb snaps to 0, 5, 10, 15, 20, 25, ..., 100
- **Interval = 25**: Labels/ticks show at 0, 25, 50, 75, 100

### Example: Rating Slider

```xml
<VerticalStackLayout Padding="20" Spacing="10">
    <Label Text="Rate this product" FontSize="16" />
    
    <sliders:SfSlider Minimum="1"
                      Maximum="5"
                      Value="3"
                      StepSize="1"
                      Interval="1"
                      ShowLabels="True"
                      ShowTicks="True"
                      LabelCreated="OnRatingLabelCreated" />
</VerticalStackLayout>
```

```csharp
private void OnRatingLabelCreated(object sender, SliderLabelCreatedEventArgs e)
{
    e.Text = e.Text switch
    {
        "1" => "⭐",
        "2" => "⭐⭐",
        "3" => "⭐⭐⭐",
        "4" => "⭐⭐⭐⭐",
        "5" => "⭐⭐⭐⭐⭐",
        _ => e.Text
    };
}
```

## Deferred Update

Deferred update controls when dependent components are updated while the thumb is being dragged. This is useful for expensive operations that shouldn't run continuously during drag.

### EnableDeferredUpdate Property

```xml
<sliders:SfSlider Minimum="0"
                  Maximum="100"
                  Value="50"
                  EnableDeferredUpdate="True"
                  DeferredUpdateDelay="1000"
                  ValueChanging="OnValueChanging" />
```

```csharp
SfSlider slider = new SfSlider
{
    Minimum = 0,
    Maximum = 100,
    Value = 50,
    EnableDeferredUpdate = true,
    DeferredUpdateDelay = 1000  // 1 second delay
};
slider.ValueChanging += OnValueChanging;
```

### DeferredUpdateDelay Property

The `DeferredUpdateDelay` property specifies the delay in milliseconds before the `ValueChanging` event is invoked during a drag-and-hold action.

**Default value**: 500 milliseconds

### Behavior

When `EnableDeferredUpdate` is `True`:
1. User drags and holds the thumb
2. After `DeferredUpdateDelay` milliseconds, `ValueChanging` event fires
3. User releases the thumb → immediate update (no delay)

**Example Timeline:**
```
0ms: User starts dragging
0-1000ms: No ValueChanging events
1000ms: ValueChanging event fires (if still dragging)
2000ms: ValueChanging event fires again
...
User releases: Immediate ValueChanged event
```

### Use Cases

- **Database queries**: Delay expensive queries until user pauses
- **API calls**: Avoid flooding server with requests during drag
- **Complex calculations**: Run only when user pauses or releases
- **Live search**: Update search results after user pauses typing/dragging

### Example: Search Filter

```xml
<VerticalStackLayout Padding="20" Spacing="10">
    <Label Text="Price Range Filter" FontSize="16" />
    
    <sliders:SfSlider Minimum="0"
                      Maximum="1000"
                      Value="500"
                      EnableDeferredUpdate="True"
                      DeferredUpdateDelay="800"
                      ValueChanging="OnPriceFilterChanging"
                      ValueChanged="OnPriceFilterChanged" />
    
    <Label x:Name="statusLabel" Text="Drag to filter" FontSize="14" />
</VerticalStackLayout>
```

```csharp
private async void OnPriceFilterChanging(object sender, SliderValueChangingEventArgs e)
{
    // This fires only if user pauses for 800ms or releases
    statusLabel.Text = "Searching...";
    await SearchProductsAsync(maxPrice: e.Value);
}

private void OnPriceFilterChanged(object sender, SliderValueChangedEventArgs e)
{
    // Always fires immediately on release
    statusLabel.Text = $"Found products under ${e.NewValue}";
}
```

### Comparison: With vs Without Deferred Update

**Without Deferred Update (default):**
```csharp
// ValueChanging fires 100+ times during a 2-second drag
private void OnValueChanging(object sender, SliderValueChangingEventArgs e)
{
    // Expensive operation runs 100+ times!
    PerformExpensiveSearch(e.Value);
}
```

**With Deferred Update:**
```csharp
// ValueChanging fires only when user pauses or releases
EnableDeferredUpdate = true;
DeferredUpdateDelay = 1000;

private void OnValueChanging(object sender, SliderValueChangingEventArgs e)
{
    // Expensive operation runs only when user pauses for 1 second
    PerformExpensiveSearch(e.Value);
}
```

## IsEnabled Property

The `IsEnabled` property controls whether the slider is interactive.

### Disabling the Slider

```xml
<sliders:SfSlider IsEnabled="False" />
```

```csharp
slider.IsEnabled = false;
```

**Behavior**: 
- Thumb cannot be dragged
- No events fire
- Default visual: Grayed out appearance

### Conditional Enabling

```csharp
private void OnCheckBoxChanged(object sender, CheckedChangedEventArgs e)
{
    slider.IsEnabled = e.Value;
}
```

```xml
<CheckBox CheckedChanged="OnCheckBoxChanged" Content="Enable Slider" />
<sliders:SfSlider x:Name="slider" IsEnabled="False" />
```

### Styling Disabled State

Use Visual State Manager to customize disabled appearance (see [Visual State Manager](#visual-state-manager) section).

## Liquid Glass Effect

The Liquid Glass Effect introduces a modern, translucent design with adaptive color tinting and light refraction, creating a sleek glass-like user experience.

### Prerequisites

**Platform Requirements:**
- **.NET 10** or later
- **iOS 26** or **macOS 26** or later

**Note**: This feature is NOT available on Android, Windows, or older iOS/macOS versions.

### Enabling Liquid Glass Effect

```xml
<Grid>
    <Image Source="Wallpaper.png" Aspect="AspectFill" />
    
    <sliders:SfSlider Minimum="0"
                      Maximum="100"
                      Value="45"
                      EnableLiquidGlassEffect="True" />
</Grid>
```

```csharp
var grid = new Grid
{
    BackgroundColor = Colors.Transparent
};

var image = new Image
{
    Source = "Wallpaper.png",
    Aspect = Aspect.AspectFill
};
grid.Children.Add(image);

var slider = new SfSlider
{
    Minimum = 0,
    Maximum = 100,
    Value = 45,
    EnableLiquidGlassEffect = true
};
grid.Children.Add(slider);

Content = grid;
```

### Visual Effect

When enabled, the liquid glass effect provides:
- **Translucent thumb**: Semi-transparent with blur
- **Adaptive tinting**: Blends with background colors
- **Light refraction**: Subtle shimmer during interaction
- **Smooth transitions**: Animated glass-like effects

### Best Practices

- Place slider over visually rich backgrounds (images, gradients)
- Ensure background has sufficient contrast
- Test on actual iOS 26/macOS 26 devices (simulator may differ)
- Provide fallback styling for unsupported platforms

### Platform Detection

```csharp
bool supportsLiquidGlass = DeviceInfo.Platform == DevicePlatform.iOS && 
                           DeviceInfo.Version >= new Version(26, 0);

slider.EnableLiquidGlassEffect = supportsLiquidGlass;
```

## Visual State Manager

Visual State Manager (VSM) allows customizing slider appearance based on its state (enabled/disabled).

### Complete VSM Example

See previous reference files for complete VSM examples:
- [ticks-and-dividers.md](ticks-and-dividers.md#disabled-dividers-with-visual-state-manager)
- [thumb-selection-overlay.md](thumb-selection-overlay.md#disabled-thumb-with-visual-state-manager)

### Quick VSM Pattern

```xml
<sliders:SfSlider>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState x:Name="Default">
                <VisualState.Setters>
                    <!-- Enabled state styling -->
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Disabled">
                <VisualState.Setters>
                    <!-- Disabled state styling -->
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</sliders:SfSlider>
```

## Best Practices

### StepSize
- Use StepSize for discrete selections (ratings, counts, levels)
- Set StepSize = 0 for continuous values (volume, brightness)
- Match StepSize to your data type (integers, decimals, percentages)

### Deferred Update
- Enable for expensive operations (API calls, database queries)
- Set delay based on expected user behavior (500-1500ms typical)
- Always handle both `ValueChanging` and `ValueChanged` for best UX

### Liquid Glass
- Only use on iOS 26+ or macOS 26+
- Ensure rich background for best effect
- Test on actual devices (not just simulators)

### Migration
- Review all property changes carefully
- Test slider behavior thoroughly after migration
- Update event handlers if property names changed

## Troubleshooting

### Issue: StepSize Not Working

**Cause**: StepSize is 0 or not set  
**Solution**: Set StepSize to desired increment:
```xml
<sliders:SfSlider StepSize="1" />
```

### Issue: Deferred Update Not Delaying

**Cause**: EnableDeferredUpdate is False  
**Solution**: Enable deferred update:
```xml
<sliders:SfSlider EnableDeferredUpdate="True" DeferredUpdateDelay="1000" />
```

### Issue: Liquid Glass Not Visible

**Cause**: Platform not supported or effect not enabled  
**Solution**: Verify platform requirements (.NET 10, iOS/macOS 26+):
```xml
<sliders:SfSlider EnableLiquidGlassEffect="True" />
```

## Summary

Key advanced features:

**Discrete Selection:**
- `StepSize`: Incremental value snapping (0 = continuous)

**Deferred Update:**
- `EnableDeferredUpdate`: Delay updates during drag
- `DeferredUpdateDelay`: Delay in milliseconds (default: 500)

**State Management:**
- `IsEnabled`: Enable/disable slider interaction
- Visual State Manager: Customize appearance per state

**Modern Effects:**
- `EnableLiquidGlassEffect`: Translucent design (.NET 10, iOS/macOS 26+)

**Migration:**
- Update namespace, control names, and property mappings
- Use style objects instead of direct properties
- Register handler with `ConfigureSyncfusionCore()`

Use these advanced features to create sophisticated, performant slider interactions.
