# Touch Interactions and Effects Types

Comprehensive guide to touch interactions and the five effect types available in SfEffectsView.

## Table of Contents
- [Touch Interaction Types](#touch-interaction-types)
- [Effect Types Overview](#effect-types-overview)
- [Ripple Effect](#ripple-effect)
- [Highlight Effect](#highlight-effect)
- [Selection Effect](#selection-effect)
- [Scale Effect](#scale-effect)
- [Rotation Effect](#rotation-effect)
- [Combining Multiple Effects](#combining-multiple-effects)
- [Effect Lifecycle](#effect-lifecycle)

## Touch Interaction Types

SfEffectsView responds to three types of touch interactions. You can assign different effects to each interaction type.

### TouchDownEffects

Effects that render when the user touches down (presses) on the view.

```xaml
<effectsView:SfEffectsView TouchDownEffects="Ripple">
    <Label Text="Tap Me" />
</effectsView:SfEffectsView>
```

```csharp
var effectsView = new SfEffectsView
{
    TouchDownEffects = SfEffects.Ripple
};
```

**Use cases:** Immediate feedback, ripple animations, highlight overlays

### TouchUpEffects

Effects that render when the user releases the touch (lifts finger).

```xaml
<effectsView:SfEffectsView TouchUpEffects="Scale">
    <Label Text="Release to Scale" />
</effectsView:SfEffectsView>
```

```csharp
var effectsView = new SfEffectsView
{
    TouchUpEffects = SfEffects.Scale
};
```

**Use cases:** Button release feedback, scale-up animations

### LongPressEffects

Effects that render when the user performs a long press gesture.

```xaml
<effectsView:SfEffectsView LongPressEffects="Selection">
    <Label Text="Long Press to Select" />
</effectsView:SfEffectsView>
```

```csharp
var effectsView = new SfEffectsView
{
    LongPressEffects = SfEffects.Selection
};
```

**Use cases:** Item selection, context menu triggers, long-press actions

**Long Press Duration:** Default is approximately 500ms. The duration is platform-specific and follows system defaults.

## Effect Types Overview

SfEffectsView provides five distinct effect types:

| Effect | Description | Best For |
|--------|-------------|----------|
| **Ripple** | Expandable circle from touch point | Material Design buttons, interactive elements |
| **Highlight** | Solid color overlay | List items, subtle feedback |
| **Selection** | Persistent highlight state | Selectable cards, multi-select lists |
| **Scale** | Zoom in/out animation | Images, buttons, press feedback |
| **Rotation** | Rotate view by angle | Icons, playful interactions |

### SfEffects Enum Values

```csharp
public enum SfEffects
{
    None,        // No effect
    Ripple,      // Ripple animation
    Highlight,   // Color overlay
    Selection,   // Persistent selection
    Scale,       // Zoom animation
    Rotation     // Rotation animation
}
```

## Ripple Effect

An expandable circle animation that grows from the touch point, filling the entire view. This is the most popular effect, providing Material Design-style feedback.

### Basic Ripple

```xaml
<effectsView:SfEffectsView TouchDownEffects="Ripple">
    <Border BackgroundColor="#2196F3" 
            Padding="30,10"
            StrokeThickness="0">
        <Border.StrokeShape>
            <RoundRectangle CornerRadius="8" />
        </Border.StrokeShape>
        <Label Text="TAP ME" TextColor="White" FontAttributes="Bold" />
    </Border>
</effectsView:SfEffectsView>
```

```csharp
var effectsView = new SfEffectsView
{
    TouchDownEffects = SfEffects.Ripple
};
```

### How Ripple Works

1. **Touch Down:** User taps the view
2. **Ripple Start:** A circle appears at the touch point
3. **Expansion:** Circle grows outward until it fills the view bounds
4. **Touch Up:** Ripple completes and fades out

### Ripple Behavior

- **Origin:** Ripple starts at the exact touch point coordinates
- **Growth:** Expands uniformly in all directions
- **Size:** Initial size controlled by `InitialRippleFactor` property
- **Duration:** Controlled by `RippleAnimationDuration` property (default: 800ms)
- **Fade:** Optional fade-out during expansion via `FadeOutRipple` property

### Ripple Positioning

The ripple effect originates from where the user touches:

```xaml
<!-- Ripple always starts from touch point -->
<effectsView:SfEffectsView TouchDownEffects="Ripple">
    <Grid WidthRequest="200" HeightRequest="100" BackgroundColor="LightBlue">
        <Label Text="Tap anywhere" HorizontalOptions="Center" VerticalOptions="Center" />
    </Grid>
</effectsView:SfEffectsView>
```

**Tip:** For centered ripple effects regardless of touch location, use a small `InitialRippleFactor` (e.g., 0.1) so the ripple always appears to start from the center area.

## Highlight Effect

A solid color overlay that appears over the view, providing subtle visual feedback.

### Basic Highlight

```xaml
<effectsView:SfEffectsView TouchDownEffects="Highlight"
                            HighlightBackground="#E0E0E0">
    <Grid Padding="15">
        <Label Text="Tap for Highlight" FontSize="16" />
    </Grid>
</effectsView:SfEffectsView>
```

```csharp
var effectsView = new SfEffectsView
{
    TouchDownEffects = SfEffects.Highlight,
    HighlightBackground = new SolidColorBrush(Color.FromArgb("#E0E0E0"))
};
```

### How Highlight Works

1. **Touch Down:** Highlight overlay appears instantly
2. **Hold:** Overlay remains while finger is down
3. **Touch Up:** Overlay disappears immediately

### Highlight Behavior

- **No Animation:** Highlight appears/disappears instantly (no fade)
- **Full Coverage:** Overlay covers the entire view bounds
- **Opacity:** Use semi-transparent colors for best effect (e.g., `#80000000` for 50% black)
- **Layering:** Highlight renders on top of the content view

### Highlight for List Items

Perfect for providing feedback in list views:

```xaml
<CollectionView ItemsSource="{Binding Items}">
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <effectsView:SfEffectsView TouchDownEffects="Highlight"
                                        HighlightBackground="#10000000">
                <Grid Padding="15">
                    <Label Text="{Binding Name}" FontSize="16" />
                </Grid>
            </effectsView:SfEffectsView>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

## Selection Effect

A persistent highlight that remains after interaction, indicating selected state. Unlike Highlight, Selection persists until explicitly cleared.

### Basic Selection

```xaml
<effectsView:SfEffectsView LongPressEffects="Selection"
                            SelectionBackground="#E3F2FD"
                            SelectionChanged="OnSelectionChanged">
    <Border Padding="20" BackgroundColor="White">
        <Label Text="Long Press to Select" FontSize="16" />
    </Border>
</effectsView:SfEffectsView>
```

```csharp
var effectsView = new SfEffectsView
{
    LongPressEffects = SfEffects.Selection,
    SelectionBackground = new SolidColorBrush(Color.FromArgb("#E3F2FD"))
};

effectsView.SelectionChanged += (s, e) =>
{
    bool isSelected = effectsView.IsSelected;
    // Handle selection state change
};
```

### How Selection Works

1. **Trigger:** User performs the configured gesture (usually long press)
2. **Selection On:** `SelectionBackground` overlay appears
3. **State Persists:** Overlay remains until state is cleared
4. **Selection Off:** Overlay disappears when `IsSelected` is set to `false`

### Programmatic Selection

Control selection state via the `IsSelected` property:

```xaml
<effectsView:SfEffectsView x:Name="myEffectsView"
                            LongPressEffects="Selection"
                            SelectionBackground="#BBDEFB">
    <Label Text="Selectable Item" />
</effectsView:SfEffectsView>

<Button Text="Toggle Selection" Clicked="ToggleSelection" />
```

```csharp
private void ToggleSelection(object sender, EventArgs e)
{
    myEffectsView.IsSelected = !myEffectsView.IsSelected;
}
```

### Selection in Multi-Select Lists

Implement multi-selection with data binding:

```xaml
<CollectionView ItemsSource="{Binding Items}">
    <CollectionView.ItemTemplate>
        <DataTemplate x:DataType="local:ItemViewModel">
            <effectsView:SfEffectsView LongPressEffects="Selection"
                                        IsSelected="{Binding IsSelected, Mode=TwoWay}"
                                        SelectionBackground="#C5E1A5">
                <Grid Padding="15">
                    <Label Text="{Binding Name}" FontSize="16" />
                </Grid>
            </effectsView:SfEffectsView>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

### Selection Toggle Behavior

By default, selection toggles on/off with repeated long presses:
- First long press → Selected (`IsSelected = true`)
- Second long press → Deselected (`IsSelected = false`)

To prevent deselection, handle the `SelectionChanged` event:

```csharp
effectsView.SelectionChanged += (s, e) =>
{
    if (!effectsView.IsSelected)
    {
        // Prevent deselection
        effectsView.IsSelected = true;
    }
};
```

## Scale Effect

Animates the view size up or down, creating a zoom effect.

### Scale Down (Press Effect)

```xaml
<effectsView:SfEffectsView TouchDownEffects="Scale"
                            ScaleFactor="0.85"
                            ScaleAnimationDuration="200">
    <Image Source="icon.png" 
           WidthRequest="100" 
           HeightRequest="100" />
</effectsView:SfEffectsView>
```

```csharp
var effectsView = new SfEffectsView
{
    TouchDownEffects = SfEffects.Scale,
    ScaleFactor = 0.85,  // Scale to 85%
    ScaleAnimationDuration = 200
};
```

### Scale Up (Release Effect)

```xaml
<effectsView:SfEffectsView TouchUpEffects="Scale"
                            ScaleFactor="1.15"
                            ScaleAnimationDuration="300">
    <Label Text="Release to Zoom" FontSize="20" />
</effectsView:SfEffectsView>
```

### How Scale Works

1. **Trigger:** Configured touch event occurs
2. **Animation:** View smoothly scales to `ScaleFactor` value
3. **Duration:** Animation time set by `ScaleAnimationDuration`
4. **Reset:** View returns to original size after animation completes

### ScaleFactor Values

- **< 1.0:** Scale down (shrink)
  - `0.9` = 90% size (subtle press effect)
  - `0.8` = 80% size (default, moderate press)
  - `0.5` = 50% size (dramatic shrink)
- **= 1.0:** No change
- **> 1.0:** Scale up (grow)
  - `1.1` = 110% size (subtle zoom)
  - `1.5` = 150% size (noticeable zoom)
  - `2.0` = 200% size (double size)

### Press and Release Pattern

Common pattern: Scale down on press, return on release:

```xaml
<effectsView:SfEffectsView TouchDownEffects="Scale"
                            TouchUpEffects="None"
                            ScaleFactor="0.9"
                            ScaleAnimationDuration="100">
    <Border BackgroundColor="#FF5722" Padding="20,10">
        <Label Text="PRESS ME" TextColor="White" FontAttributes="Bold" />
    </Border>
</effectsView:SfEffectsView>
```

**Note:** Setting `TouchUpEffects="None"` allows the scale animation to auto-reverse on touch up.

## Rotation Effect

Rotates the view by a specified angle.

### Basic Rotation

```xaml
<effectsView:SfEffectsView TouchDownEffects="Rotation"
                            Angle="180"
                            RotationAnimationDuration="500">
    <Label Text="🔄" FontSize="48" />
</effectsView:SfEffectsView>
```

```csharp
var effectsView = new SfEffectsView
{
    TouchDownEffects = SfEffects.Rotation,
    Angle = 180,
    RotationAnimationDuration = 500
};
```

### How Rotation Works

1. **Trigger:** Touch event occurs
2. **Animation:** View rotates to `Angle` degrees
3. **Direction:** Clockwise rotation
4. **Center:** Rotates around the view's center point
5. **Reset:** Returns to 0° after animation completes

### Angle Values

- `90` = Quarter turn (right angle)
- `180` = Half turn (upside down)
- `270` = Three-quarter turn
- `360` = Full rotation (complete circle)
- Negative values rotate counter-clockwise: `-90`, `-180`, etc.

### Flip Effect

Create a flip animation with 180°:

```xaml
<effectsView:SfEffectsView TouchDownEffects="Rotation"
                            Angle="180"
                            RotationAnimationDuration="400">
    <Border BackgroundColor="#9C27B0" 
            Padding="30"
            WidthRequest="120"
            HeightRequest="120">
        <Label Text="FLIP" 
               TextColor="White" 
               FontSize="20"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
    </Border>
</effectsView:SfEffectsView>
```

### Continuous Spin

Use 360° for a full rotation:

```xaml
<effectsView:SfEffectsView TouchDownEffects="Rotation"
                            Angle="360"
                            RotationAnimationDuration="1000">
    <Image Source="refresh_icon.png" 
           WidthRequest="50" 
           HeightRequest="50" />
</effectsView:SfEffectsView>
```

## Combining Multiple Effects

Apply multiple effects simultaneously for rich interactions.

### Syntax

**XAML:** Use comma-separated values
```xaml
TouchDownEffects="Ripple, Scale"
```

**C#:** Use bitwise OR operator
```csharp
effectsView.TouchDownEffects = SfEffects.Ripple | SfEffects.Scale;
```

### Ripple + Scale

Popular combination for Material Design buttons:

```xaml
<effectsView:SfEffectsView TouchDownEffects="Ripple, Scale"
                            ScaleFactor="0.95"
                            RippleBackground="#FFFFFF">
    <Border BackgroundColor="#6200EE" Padding="30,15">
        <Label Text="SUBMIT" TextColor="White" FontAttributes="Bold" />
    </Border>
</effectsView:SfEffectsView>
```

### Ripple + Rotation

Create playful interactive elements:

```xaml
<effectsView:SfEffectsView TouchDownEffects="Ripple, Rotation"
                            Angle="180"
                            RippleBackground="#FF5722"
                            RotationAnimationDuration="800">
    <Image Source="star_icon.png" 
           WidthRequest="80" 
           HeightRequest="80" />
</effectsView:SfEffectsView>
```

### Highlight + Selection

Touch feedback followed by persistent selection:

```xaml
<effectsView:SfEffectsView TouchDownEffects="Highlight"
                            LongPressEffects="Selection"
                            HighlightBackground="#20000000"
                            SelectionBackground="#E8F5E9">
    <Grid Padding="15" BackgroundColor="White">
        <Label Text="Tap for highlight, long press for selection" />
    </Grid>
</effectsView:SfEffectsView>
```

### All Effects Combined

```xaml
<effectsView:SfEffectsView TouchDownEffects="Ripple, Scale, Rotation, Highlight"
                            ScaleFactor="0.9"
                            Angle="45"
                            RippleBackground="#2196F3"
                            HighlightBackground="#20000000">
    <Label Text="Kitchen Sink" FontSize="24" Padding="20" />
</effectsView:SfEffectsView>
```

**Warning:** Using too many effects simultaneously can be visually overwhelming. Use combinations purposefully.

## Effect Lifecycle

Understanding when effects start, animate, and complete.

### Timeline Diagram

```
Touch Down → Effects Start → Animation Duration → Touch Up → Effects Complete
```

### Touch Down Effects

```
User Touches Down
    ↓
TouchDownEffects Begin
    ↓
Animation Plays (RippleAnimationDuration, ScaleAnimationDuration, etc.)
    ↓
[User still holding]
    ↓
User Releases
    ↓
Effects Complete/Reverse
    ↓
AnimationCompleted Event Fires
```

### Long Press Effects

```
User Touches Down
    ↓
Hold for ~500ms
    ↓
LongPressed Event Fires
    ↓
LongPressEffects Begin
    ↓
Animation Plays
    ↓
User Releases
    ↓
Effects Complete (except Selection)
```

### Effect Persistence

- **Ripple, Highlight, Scale, Rotation:** Temporary, auto-reverse after touch up
- **Selection:** Persistent, remains until `IsSelected = false`

### Animation Completion

The `AnimationCompleted` event fires when effects finish:

```csharp
effectsView.AnimationCompleted += (s, e) =>
{
    // Effects have completed
    // Good place to trigger navigation, submit forms, etc.
};
```

**Note:** `AnimationCompleted` does NOT fire for `Selection` effect (it's persistent, not animated).

### Programmatic Effects

Trigger effects without touch by setting properties:

```csharp
// Programmatically trigger selection
effectsView.IsSelected = true;  // Selection appears immediately

// Note: Other effects (Ripple, Scale, etc.) cannot be triggered programmatically
// They only respond to touch interaction
```

## Effect Interaction Patterns

### Disabled Effects

Set effects to `None` to disable:

```xaml
<effectsView:SfEffectsView TouchDownEffects="None"
                            TouchUpEffects="None"
                            LongPressEffects="None">
    <Label Text="No Effects" />
</effectsView:SfEffectsView>
```

### Conditional Effects

Change effects based on state:

```csharp
// Enable effects when item is selectable
if (item.IsSelectable)
{
    effectsView.LongPressEffects = SfEffects.Selection;
}
else
{
    effectsView.LongPressEffects = SfEffects.None;
}
```

### Platform-Specific Effects

Some effects may behave differently across platforms. Test on target devices for consistent experience.
