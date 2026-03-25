# Features and Configuration

Additional features and configuration options for SfEffectsView to enhance effects behavior.

## Table of Contents
- [FadeOutRipple](#fadeoutripple)
- [IsSelected](#isselected)
- [ShouldIgnoreTouches](#shouldignoretouches)
- [AutoResetEffects](#autoreseteffects)
- [Feature Combinations](#feature-combinations)
- [Platform-Specific Behaviors](#platform-specific-behaviors)

## FadeOutRipple

Controls whether the ripple effect fades out (loses opacity) as it expands.

**Property Type:** `bool`  
**Default Value:** `false`

### Basic Usage

When enabled, the ripple circle gradually fades to transparent while expanding, creating a more subtle effect.

```xaml
<effectsView:SfEffectsView TouchDownEffects="Ripple"
                            FadeOutRipple="True"
                            RippleAnimationDuration="1000">
    <Border BackgroundColor="#6200EE" Padding="30,15">
        <Label Text="Fading Ripple" TextColor="White" />
    </Border>
</effectsView:SfEffectsView>
```

```csharp
var effectsView = new SfEffectsView
{
    TouchDownEffects = SfEffects.Ripple,
    FadeOutRipple = true,
    RippleAnimationDuration = 1000
};
```

### How It Works

**Without FadeOutRipple (default):**
- Ripple maintains consistent opacity throughout expansion
- Circle is fully visible until touch up
- More prominent, attention-grabbing effect

**With FadeOutRipple:**
- Ripple starts at full opacity
- Gradually fades to 0% opacity as it expands
- More subtle, refined appearance
- Better for longer animation durations

### Visual Comparison

```xaml
<StackLayout Spacing="20" Padding="20">
    
    <!-- Standard Ripple (no fade) -->
    <Label Text="Standard Ripple:" FontAttributes="Bold" />
    <effectsView:SfEffectsView TouchDownEffects="Ripple"
                                FadeOutRipple="False"
                                RippleAnimationDuration="1000"
                                RippleBackground="#2196F3">
        <Border BackgroundColor="White" Padding="20">
            <Label Text="No Fade - Full Opacity" />
        </Border>
    </effectsView:SfEffectsView>
    
    <!-- Fading Ripple -->
    <Label Text="Fading Ripple:" FontAttributes="Bold" Margin="0,20,0,0" />
    <effectsView:SfEffectsView TouchDownEffects="Ripple"
                                FadeOutRipple="True"
                                RippleAnimationDuration="1000"
                                RippleBackground="#2196F3">
        <Border BackgroundColor="White" Padding="20">
            <Label Text="Fades Out - Subtle Effect" />
        </Border>
    </effectsView:SfEffectsView>
    
</StackLayout>
```

### Recommended Usage

**Enable FadeOutRipple when:**
- Using long animation durations (>800ms)
- Want subtle, refined interactions
- Background content visibility is important
- Creating elegant, sophisticated UX

**Disable FadeOutRipple when:**
- Using short animation durations (<500ms)
- Want prominent, attention-grabbing feedback
- Following strict Material Design guidelines
- Need clear visual confirmation of interaction

### Combining with Other Properties

```xaml
<!-- Long, slow, fading ripple for dramatic effect -->
<effectsView:SfEffectsView TouchDownEffects="Ripple"
                            FadeOutRipple="True"
                            RippleAnimationDuration="2000"
                            InitialRippleFactor="0.2"
                            RippleBackground="#FF5722">
    <Image Source="hero_image.png" 
           Aspect="AspectFill"
           HeightRequest="200" />
</effectsView:SfEffectsView>
```

## IsSelected

Gets or sets the selection state of the SfEffectsView. When `true`, the `SelectionBackground` overlay is displayed.

**Property Type:** `bool`  
**Default Value:** `false`  
**Bindable:** Yes

### Basic Usage

```xaml
<effectsView:SfEffectsView IsSelected="True"
                            SelectionBackground="#E3F2FD">
    <Border Padding="15" BackgroundColor="White">
        <Label Text="Pre-Selected Item" FontSize="16" />
    </Border>
</effectsView:SfEffectsView>
```

```csharp
var effectsView = new SfEffectsView
{
    IsSelected = true,
    SelectionBackground = new SolidColorBrush(Color.FromArgb("#E3F2FD"))
};
```

### Programmatic Selection

Toggle selection state in code:

```xaml
<effectsView:SfEffectsView x:Name="myEffectsView"
                            SelectionBackground="#C8E6C9">
    <Label Text="Click button to select" Padding="20" />
</effectsView:SfEffectsView>

<Button Text="Toggle Selection" Clicked="OnToggleClicked" />
```

```csharp
private void OnToggleClicked(object sender, EventArgs e)
{
    myEffectsView.IsSelected = !myEffectsView.IsSelected;
}
```

### Data Binding

Bind to ViewModel for MVVM pattern:

```xaml
<CollectionView ItemsSource="{Binding Items}">
    <CollectionView.ItemTemplate>
        <DataTemplate x:DataType="local:ItemViewModel">
            <effectsView:SfEffectsView IsSelected="{Binding IsSelected, Mode=TwoWay}"
                                        SelectionBackground="#BBDEFB">
                <Grid Padding="15">
                    <Label Text="{Binding Name}" FontSize="16" />
                </Grid>
            </effectsView:SfEffectsView>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

**ViewModel:**

```csharp
public class ItemViewModel : INotifyPropertyChanged
{
    private bool _isSelected;
    
    public bool IsSelected
    {
        get => _isSelected;
        set
        {
            if (_isSelected != value)
            {
                _isSelected = value;
                OnPropertyChanged();
            }
        }
    }
    
    public string Name { get; set; }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Selection vs LongPressEffects

**Important distinction:**

- `IsSelected` property: **Programmatic** control of selection state
- `LongPressEffects="Selection"`: **Gesture-based** selection trigger

You can use both together:

```xaml
<effectsView:SfEffectsView LongPressEffects="Selection"
                            IsSelected="{Binding IsSelected, Mode=TwoWay}"
                            SelectionBackground="#FFE0B2">
    <Label Text="Long press to select OR bind programmatically" Padding="15" />
</effectsView:SfEffectsView>
```

### Behavior Notes

- Setting `IsSelected = true` immediately displays the selection overlay
- Setting `IsSelected = false` immediately removes the overlay
- No animation when changing `IsSelected` programmatically
- The `SelectionChanged` event fires when `IsSelected` changes (both programmatically and via gesture)

## ShouldIgnoreTouches

Controls whether the SfEffectsView responds to direct touch interaction.

**Property Type:** `bool`  
**Default Value:** `false`

### Basic Usage

When `true`, the control ignores all touch input and does not render effects.

```xaml
<effectsView:SfEffectsView ShouldIgnoreTouches="True"
                            TouchDownEffects="Ripple">
    <Label Text="Touch Disabled" Padding="20" />
</effectsView:SfEffectsView>
```

```csharp
var effectsView = new SfEffectsView
{
    ShouldIgnoreTouches = true,
    TouchDownEffects = SfEffects.Ripple
};
```

### Use Cases

#### 1. Conditionally Disable Interactions

```xaml
<effectsView:SfEffectsView TouchDownEffects="Ripple"
                            ShouldIgnoreTouches="{Binding IsProcessing}">
    <Border BackgroundColor="#6200EE" Padding="30,15">
        <Label Text="{Binding ButtonText}" TextColor="White" />
    </Border>
</effectsView:SfEffectsView>
```

```csharp
public class MyViewModel : INotifyPropertyChanged
{
    private bool _isProcessing;
    
    public bool IsProcessing
    {
        get => _isProcessing;
        set
        {
            _isProcessing = value;
            OnPropertyChanged();
            OnPropertyChanged(nameof(ButtonText));
        }
    }
    
    public string ButtonText => IsProcessing ? "Processing..." : "Submit";
}
```

#### 2. Read-Only Mode

```csharp
public void SetReadOnlyMode(bool isReadOnly)
{
    effectsView.ShouldIgnoreTouches = isReadOnly;
    
    // Optionally adjust visual appearance
    if (isReadOnly)
    {
        effectsView.Opacity = 0.5;
    }
}
```

#### 3. Programmatic-Only Selection

Allow only programmatic selection, not user gestures:

```xaml
<effectsView:SfEffectsView ShouldIgnoreTouches="True"
                            IsSelected="{Binding IsSelected}"
                            SelectionBackground="#E8F5E9">
    <Label Text="Selection controlled by code only" Padding="15" />
</effectsView:SfEffectsView>
```

### Touch vs Gesture Recognition

`ShouldIgnoreTouches` only affects the SfEffectsView's built-in touch handling. It does NOT block:

- Gesture recognizers on child views
- Button click events on child views
- Other touch-based interactions on content

```xaml
<effectsView:SfEffectsView ShouldIgnoreTouches="True"
                            TouchDownEffects="Ripple">
    <Button Text="This still works!" Clicked="OnButtonClicked" />
    <!-- Button click will still fire despite ShouldIgnoreTouches -->
</effectsView:SfEffectsView>
```

### Visual Feedback for Disabled State

Provide visual indication when touches are ignored:

```xaml
<effectsView:SfEffectsView TouchDownEffects="Ripple"
                            ShouldIgnoreTouches="{Binding IsDisabled}"
                            Opacity="{Binding IsDisabled, Converter={StaticResource BoolToOpacityConverter}}">
    <Border BackgroundColor="#6200EE" Padding="20">
        <Label Text="Button" TextColor="White" />
    </Border>
</effectsView:SfEffectsView>
```

## AutoResetEffects

Controls which effects automatically reset (disappear) on touch up in Android and UWP platforms.

**Property Type:** `AutoResetEffects` enum  
**Default Value:** `None`  
**Platform:** Android, UWP only (other platforms ignore this property)

### AutoResetEffects Enum

```csharp
public enum AutoResetEffects
{
    None,        // No auto-reset (default)
    Highlight,   // Reset highlight on touch up
    Ripple,      // Reset ripple on touch up
    Scale,       // Reset scale on touch up
    Rotation,    // Reset rotation on touch up
    Selection    // Reset selection on touch up
}
```

### Basic Usage

```xaml
<effectsView:SfEffectsView TouchDownEffects="Highlight"
                            AutoResetEffects="Highlight"
                            HighlightBackground="#E0E0E0">
    <Label Text="Highlight auto-resets on touch up" Padding="20" />
</effectsView:SfEffectsView>
```

```csharp
var effectsView = new SfEffectsView
{
    TouchDownEffects = SfEffects.Highlight,
    AutoResetEffects = AutoResetEffects.Highlight
};
```

### How It Works

**Without AutoResetEffects (default):**
- Effect starts on touch down
- Effect continues until animation completes or touch up
- Natural behavior for most use cases

**With AutoResetEffects:**
- Effect starts on touch down
- Effect **immediately removes** on touch up
- Useful for instant feedback scenarios

### Platform Behavior

**Android & UWP:**
- `AutoResetEffects` is respected
- Effects immediately disappear on touch up

**iOS, macOS, other platforms:**
- `AutoResetEffects` is ignored
- Effects follow normal lifecycle

### Combining with TouchUpEffects

```xaml
<!-- Highlight on touch down, auto-reset, then scale on touch up -->
<effectsView:SfEffectsView TouchDownEffects="Highlight"
                            TouchUpEffects="Scale"
                            AutoResetEffects="Highlight"
                            HighlightBackground="#E0E0E0"
                            ScaleFactor="1.1">
    <Label Text="Highlight → Scale sequence" Padding="20" />
</effectsView:SfEffectsView>
```

### Use Cases

**Instant Feedback Removal:**
```xaml
<effectsView:SfEffectsView TouchDownEffects="Highlight"
                            AutoResetEffects="Highlight">
    <Label Text="Highlight disappears immediately on release" />
</effectsView:SfEffectsView>
```

**Controlled Effect Duration:**
```xaml
<!-- Long ripple animation, but user can cancel by releasing -->
<effectsView:SfEffectsView TouchDownEffects="Ripple"
                            AutoResetEffects="Ripple"
                            RippleAnimationDuration="2000">
    <Label Text="Hold to see ripple, release to cancel" Padding="20" />
</effectsView:SfEffectsView>
```

## Feature Combinations

### Fading Ripple with Scale

```xaml
<effectsView:SfEffectsView TouchDownEffects="Ripple, Scale"
                            FadeOutRipple="True"
                            RippleAnimationDuration="1000"
                            ScaleFactor="0.95"
                            ScaleAnimationDuration="200"
                            RippleBackground="#2196F3">
    <Border BackgroundColor="White" Padding="30,15">
        <Label Text="Fading Ripple + Scale" />
    </Border>
</effectsView:SfEffectsView>
```

### Pre-Selected Item with Touch Disabled

```xaml
<effectsView:SfEffectsView IsSelected="True"
                            ShouldIgnoreTouches="True"
                            SelectionBackground="#C8E6C9">
    <Grid Padding="15">
        <Label Text="Selected (Read-Only)" FontSize="16" />
    </Grid>
</effectsView:SfEffectsView>
```

### Conditional Touch Response

```xaml
<effectsView:SfEffectsView TouchDownEffects="Ripple"
                            ShouldIgnoreTouches="{Binding IsLocked}"
                            FadeOutRipple="True"
                            RippleAnimationDuration="800">
    <StackLayout Padding="15">
        <Label Text="{Binding ItemName}" FontSize="16" />
        <Label Text="{Binding LockStatus}" 
               FontSize="12" 
               TextColor="Gray"
               IsVisible="{Binding IsLocked}" />
    </StackLayout>
</effectsView:SfEffectsView>
```

### Long-Press Selection with Highlight Feedback

```xaml
<effectsView:SfEffectsView TouchDownEffects="Highlight"
                            LongPressEffects="Selection"
                            IsSelected="{Binding IsSelected, Mode=TwoWay}"
                            HighlightBackground="#20000000"
                            SelectionBackground="#BBDEFB">
    <Grid Padding="15">
        <Label Text="Tap for highlight, long-press to select" />
    </Grid>
</effectsView:SfEffectsView>
```

## Platform-Specific Behaviors

### Android

- All effects work as expected
- `AutoResetEffects` is fully supported
- Ripple performance is optimized
- Material Design effects look native

### iOS

- All effects work as expected
- `AutoResetEffects` is ignored
- Effects may feel slightly different from native iOS feedback
- Consider using more subtle effects for iOS-style UX

### Windows (UWP/WinUI)

- All effects work as expected
- `AutoResetEffects` is supported
- Consider Fluent Design guidelines for Windows apps

### macOS

- All effects work as expected
- Touch interactions map to click events
- Effects provide visual feedback for mouse clicks

### Cross-Platform Considerations

**Consistent UX:**
```csharp
public void ConfigurePlatformEffects(SfEffectsView effectsView)
{
    effectsView.TouchDownEffects = SfEffects.Ripple;
    
    if (DeviceInfo.Platform == DevicePlatform.iOS)
    {
        // iOS: Subtle, quick
        effectsView.RippleAnimationDuration = 400;
        effectsView.FadeOutRipple = true;
    }
    else if (DeviceInfo.Platform == DevicePlatform.Android)
    {
        // Android: Material Design
        effectsView.RippleAnimationDuration = 600;
        effectsView.FadeOutRipple = false;
        effectsView.AutoResetEffects = AutoResetEffects.None;
    }
    else
    {
        // Windows/macOS: Balanced
        effectsView.RippleAnimationDuration = 500;
        effectsView.FadeOutRipple = false;
    }
}
```

### Testing Recommendations

1. **Test on actual devices**, not just emulators
2. **Verify touch response** feels natural on each platform
3. **Check animation performance** with complex layouts
4. **Validate accessibility** with platform screen readers
5. **Test different screen sizes** and orientations
