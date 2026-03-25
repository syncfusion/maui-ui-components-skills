# Commands and Methods

Guide to using Commands for MVVM patterns and Methods for programmatic control of effects in SfEffectsView.

## Table of Contents
- [Commands Overview](#commands-overview)
- [LongPressedCommand](#longpressedcommand)
- [TouchDownCommand](#touchdowncommand)
- [TouchUpCommand](#touchupcommand)
- [Command Parameters](#command-parameters)
- [MVVM Patterns with Commands](#mvvm-patterns-with-commands)
- [Methods Overview](#methods-overview)
- [ApplyEffects Method](#applyeffects-method)
- [Reset Method](#reset-method)
- [Programmatic Effect Control](#programmatic-effect-control)

## Commands Overview

SfEffectsView provides command properties that enable MVVM (Model-View-ViewModel) pattern support. Commands are executed when specific touch interactions occur, allowing you to bind view interactions to ViewModel logic without code-behind.

**Available Commands:**
- `LongPressedCommand` - Executes on long press
- `TouchDownCommand` - Executes on touch down
- `TouchUpCommand` - Executes on touch up

**Benefits:**
- Clean separation of UI and logic
- Testable ViewModels
- No code-behind required
- Parameter passing support
- ICommand interface compatibility

## LongPressedCommand

Executes a command when the user performs a long press gesture (holds for ~500ms).

### Basic Usage

**XAML:**
```xaml
<effectsView:SfEffectsView LongPressedCommand="{Binding ShowOptionsCommand}"
                            LongPressEffects="Selection">
    <Border Padding="15" BackgroundColor="White">
        <Label Text="Long press for options" FontSize="16" />
    </Border>
</effectsView:SfEffectsView>
```

**ViewModel:**
```csharp
public class MyViewModel : INotifyPropertyChanged
{
    public ICommand ShowOptionsCommand { get; }
    
    public MyViewModel()
    {
        ShowOptionsCommand = new Command(OnShowOptions);
    }
    
    private async void OnShowOptions()
    {
        string action = await App.Current.MainPage.DisplayActionSheet(
            "Options",
            "Cancel",
            null,
            "Edit",
            "Delete",
            "Share");
        
        if (action != "Cancel")
        {
            HandleAction(action);
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
}
```

### C# Implementation

```csharp
var effectsView = new SfEffectsView
{
    LongPressEffects = SfEffects.Selection
};

effectsView.SetBinding(
    SfEffectsView.LongPressedCommandProperty,
    new Binding("ShowOptionsCommand"));
```

### Use Cases

#### 1. Context Menu

```xaml
<effectsView:SfEffectsView LongPressedCommand="{Binding ShowContextMenuCommand}"
                            LongPressedCommandParameter="{Binding .}">
    <Grid Padding="15">
        <Label Text="{Binding ItemName}" FontSize="16" />
    </Grid>
</effectsView:SfEffectsView>
```

```csharp
public ICommand ShowContextMenuCommand { get; }

public MyViewModel()
{
    ShowContextMenuCommand = new Command<ItemModel>(OnShowContextMenu);
}

private async void OnShowContextMenu(ItemModel item)
{
    // Show context menu for specific item
}
```

#### 2. Item Selection

```csharp
public ICommand SelectItemCommand { get; }

public MyViewModel()
{
    SelectItemCommand = new Command<ItemViewModel>(OnSelectItem);
}

private void OnSelectItem(ItemViewModel item)
{
    if (item.IsSelected)
    {
        SelectedItems.Remove(item);
    }
    else
    {
        SelectedItems.Add(item);
    }
    item.IsSelected = !item.IsSelected;
}
```

#### 3. Reorder Mode

```csharp
public ICommand EnterReorderModeCommand { get; }

public MyViewModel()
{
    EnterReorderModeCommand = new Command(OnEnterReorderMode);
}

private void OnEnterReorderMode()
{
    IsReorderMode = true;
    // Enable drag-and-drop
}
```

## TouchDownCommand

Executes a command when the user touches down (presses) on the view.

### Basic Usage

**XAML:**
```xaml
<effectsView:SfEffectsView TouchDownCommand="{Binding TouchStartCommand}"
                            TouchDownEffects="Ripple">
    <Border BackgroundColor="#2196F3" Padding="30,15">
        <Label Text="TAP ME" TextColor="White" />
    </Border>
</effectsView:SfEffectsView>
```

**ViewModel:**
```csharp
public ICommand TouchStartCommand { get; }

public MyViewModel()
{
    TouchStartCommand = new Command(OnTouchStart);
}

private void OnTouchStart()
{
    // Handle touch start
    IsProcessing = true;
    StartLoadingAnimation();
}
```

### Use Cases

#### 1. Loading Indicator

```csharp
public ICommand StartLoadingCommand { get; }

public MyViewModel()
{
    StartLoadingCommand = new Command(() => 
    {
        IsLoading = true;
    });
}
```

```xaml
<Grid>
    <effectsView:SfEffectsView TouchDownCommand="{Binding StartLoadingCommand}"
                                TouchUpCommand="{Binding StopLoadingCommand}">
        <Label Text="Load Data" Padding="20" />
    </effectsView:SfEffectsView>
    
    <ActivityIndicator IsRunning="{Binding IsLoading}" />
</Grid>
```

#### 2. Analytics Tracking

```csharp
public ICommand TrackTouchCommand { get; }

public MyViewModel()
{
    TrackTouchCommand = new Command<string>(OnTrackTouch);
}

private void OnTrackTouch(string itemId)
{
    Analytics.TrackEvent("ItemTouched", new Dictionary<string, string>
    {
        { "ItemId", itemId },
        { "Timestamp", DateTime.Now.ToString() }
    });
}
```

#### 3. Audio Feedback

```csharp
public ICommand PlaySoundCommand { get; }

public MyViewModel()
{
    PlaySoundCommand = new Command(async () => 
    {
        await AudioService.PlayTouchSound();
    });
}
```

## TouchUpCommand

Executes a command when the user releases the touch (lifts finger).

### Basic Usage

**XAML:**
```xaml
<effectsView:SfEffectsView TouchUpCommand="{Binding SubmitCommand}"
                            TouchUpEffects="Scale">
    <Border BackgroundColor="#4CAF50" Padding="30,15">
        <Label Text="SUBMIT" TextColor="White" />
    </Border>
</effectsView:SfEffectsView>
```

**ViewModel:**
```csharp
public ICommand SubmitCommand { get; }

public MyViewModel()
{
    SubmitCommand = new Command(
        execute: async () => await OnSubmit(),
        canExecute: () => IsValid);
}

private async Task OnSubmit()
{
    if (!IsValid) return;
    
    IsProcessing = true;
    await SubmitFormData();
    IsProcessing = false;
}
```

### Use Cases

#### 1. Form Submission

```csharp
public ICommand SubmitFormCommand { get; }

public MyViewModel()
{
    SubmitFormCommand = new Command(
        async () => 
        {
            if (ValidateForm())
            {
                await SubmitData();
                await NavigateToConfirmation();
            }
        },
        () => !IsProcessing);
}
```

#### 2. Navigation

```csharp
public ICommand NavigateCommand { get; }

public MyViewModel()
{
    NavigateCommand = new Command<string>(async (route) => 
    {
        await Shell.Current.GoToAsync(route);
    });
}
```

```xaml
<effectsView:SfEffectsView TouchUpCommand="{Binding NavigateCommand}"
                            TouchUpCommandParameter="//details">
    <Grid Padding="15">
        <Label Text="View Details" />
        <Label Text="→" HorizontalOptions="End" />
    </Grid>
</effectsView:SfEffectsView>
```

#### 3. Toggle State

```csharp
public ICommand ToggleFavoriteCommand { get; }

public MyViewModel()
{
    ToggleFavoriteCommand = new Command<ItemViewModel>(item => 
    {
        item.IsFavorite = !item.IsFavorite;
        UpdateFavorites(item);
    });
}
```

## Command Parameters

Each command has a corresponding parameter property for passing data.

### LongPressedCommandParameter

Pass data to the long press command:

```xaml
<CollectionView ItemsSource="{Binding Items}">
    <CollectionView.ItemTemplate>
        <DataTemplate x:DataType="local:ItemViewModel">
            <effectsView:SfEffectsView 
                LongPressedCommand="{Binding Source={RelativeSource AncestorType={x:Type local:MyViewModel}}, Path=ItemLongPressedCommand}"
                LongPressedCommandParameter="{Binding .}">
                <Label Text="{Binding Name}" Padding="15" />
            </effectsView:SfEffectsView>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

```csharp
public ICommand ItemLongPressedCommand { get; }

public MyViewModel()
{
    ItemLongPressedCommand = new Command<ItemViewModel>(OnItemLongPressed);
}

private async void OnItemLongPressed(ItemViewModel item)
{
    string action = await ShowOptionsForItem(item);
    await HandleAction(action, item);
}
```

### TouchDownCommandParameter

```xaml
<effectsView:SfEffectsView x:Name="myEffectsView"
                            TouchDownCommand="{Binding TrackTouchCommand}"
                            TouchDownCommandParameter="{x:Reference myEffectsView}">
    <Label Text="Track this touch" Padding="20" />
</effectsView:SfEffectsView>
```

```csharp
public ICommand TrackTouchCommand { get; }

public MyViewModel()
{
    TrackTouchCommand = new Command<SfEffectsView>(effectsView =>
    {
        // Access the effects view instance
        var content = effectsView.Content;
        TrackInteraction(content);
    });
}
```

### TouchUpCommandParameter

```xaml
<effectsView:SfEffectsView TouchUpCommand="{Binding ExecuteActionCommand}"
                            TouchUpCommandParameter="{Binding ActionType}">
    <Border Padding="20">
        <Label Text="{Binding ActionName}" />
    </Border>
</effectsView:SfEffectsView>
```

```csharp
public ICommand ExecuteActionCommand { get; }

public MyViewModel()
{
    ExecuteActionCommand = new Command<ActionType>(async (actionType) =>
    {
        await ExecuteAction(actionType);
    });
}
```

## MVVM Patterns with Commands

### Complete MVVM Example

**ViewModel:**
```csharp
public class ProductViewModel : INotifyPropertyChanged
{
    private bool _isSelected;
    private bool _isProcessing;
    
    public string Name { get; set; }
    public string Price { get; set; }
    public string ImageUrl { get; set; }
    
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
    
    public bool IsProcessing
    {
        get => _isProcessing;
        set
        {
            _isProcessing = value;
            OnPropertyChanged();
            ((Command)ViewDetailsCommand).ChangeCanExecute();
        }
    }
    
    public ICommand ViewDetailsCommand { get; }
    public ICommand ToggleSelectionCommand { get; }
    public ICommand ShowOptionsCommand { get; }
    
    public ProductViewModel()
    {
        ViewDetailsCommand = new Command(
            async () => await OnViewDetails(),
            () => !IsProcessing);
        
        ToggleSelectionCommand = new Command(() => 
        {
            IsSelected = !IsSelected;
        });
        
        ShowOptionsCommand = new Command(async () => 
        {
            string action = await App.Current.MainPage.DisplayActionSheet(
                "Product Options",
                "Cancel",
                null,
                "Add to Cart",
                "Add to Wishlist",
                "Share");
            
            await HandleAction(action);
        });
    }
    
    private async Task OnViewDetails()
    {
        IsProcessing = true;
        await Shell.Current.GoToAsync($"//productDetails?id={Id}");
        IsProcessing = false;
    }
    
    private async Task HandleAction(string action)
    {
        switch (action)
        {
            case "Add to Cart":
                await AddToCart();
                break;
            case "Add to Wishlist":
                await AddToWishlist();
                break;
            case "Share":
                await Share();
                break;
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**View:**
```xaml
<effectsView:SfEffectsView TouchUpCommand="{Binding ViewDetailsCommand}"
                            LongPressedCommand="{Binding ShowOptionsCommand}"
                            TouchDownEffects="Ripple"
                            LongPressEffects="Selection"
                            IsSelected="{Binding IsSelected}">
    <Border Padding="15" BackgroundColor="White">
        <Grid RowDefinitions="Auto,Auto,Auto" RowSpacing="8">
            <Image Source="{Binding ImageUrl}" 
                   HeightRequest="150" 
                   Aspect="AspectFill" />
            
            <Label Grid.Row="1" 
                   Text="{Binding Name}" 
                   FontSize="16" 
                   FontAttributes="Bold" />
            
            <Label Grid.Row="2" 
                   Text="{Binding Price}" 
                   FontSize="14" 
                   TextColor="Green" />
        </Grid>
    </Border>
</effectsView:SfEffectsView>
```

### Async Command Pattern

Using community toolkit's `AsyncRelayCommand`:

```csharp
public class MyViewModel : ObservableObject
{
    [ObservableProperty]
    private bool isLoading;
    
    public IAsyncRelayCommand LoadDataCommand { get; }
    
    public MyViewModel()
    {
        LoadDataCommand = new AsyncRelayCommand(LoadDataAsync);
    }
    
    private async Task LoadDataAsync()
    {
        IsLoading = true;
        
        try
        {
            await FetchDataFromApi();
        }
        catch (Exception ex)
        {
            await ShowError(ex.Message);
        }
        finally
        {
            IsLoading = false;
        }
    }
}
```

## Methods Overview

SfEffectsView provides methods for programmatic control of effects, allowing you to trigger or reset effects from code without user interaction.

**Available Methods:**
- `ApplyEffects()` - Trigger effects programmatically
- `Reset()` - Remove effects applied via ApplyEffects

## ApplyEffects Method

Triggers effect rendering programmatically with optional parameters.

### Method Signature

```csharp
public void ApplyEffects(
    SfEffects effects = SfEffects.Ripple,
    RippleStartPosition rippleStartPosition = RippleStartPosition.Default,
    Point? rippleStartPoint = null,
    bool repeat = false)
```

### Parameters

| Parameter | Type | Description | Default |
|-----------|------|-------------|---------|
| `effects` | SfEffects | Effect type to apply | Ripple |
| `rippleStartPosition` | RippleStartPosition | Ripple origin position | Default (center) |
| `rippleStartPoint` | Point? | Custom ripple start coordinates | null |
| `repeat` | bool | Whether to repeat ripple continuously | false |

### Basic Usage

```csharp
// Apply default ripple effect
effectsView.ApplyEffects();

// Apply specific effect
effectsView.ApplyEffects(SfEffects.Highlight);

// Apply scale effect
effectsView.ApplyEffects(SfEffects.Scale);
```

### Ripple Start Positions

```csharp
public enum RippleStartPosition
{
    Default,      // Center
    Left,
    Top,
    Right,
    Bottom,
    TopLeft,
    TopRight,
    BottomLeft,
    BottomRight
}
```

**Examples:**

```csharp
// Ripple from top-left corner
effectsView.ApplyEffects(
    SfEffects.Ripple, 
    RippleStartPosition.TopLeft);

// Ripple from right edge
effectsView.ApplyEffects(
    SfEffects.Ripple, 
    RippleStartPosition.Right);
```

### Custom Ripple Start Point

Specify exact coordinates for ripple origin:

```csharp
// Ripple from custom point (x: 50, y: 75)
    effectsView.ApplyEffects(
SfEffects.Ripple,
RippleStartPosition.Default,
new System.Drawing.Point(50, 75));
```

### Repeating Ripple

Create a pulsing/breathing effect:

```csharp
// Continuous ripple animation
effectsView.ApplyEffects(
    SfEffects.Ripple,
    RippleStartPosition.Default,
    null,
    repeat: true);

// Stop with Reset()
```

**Note:** Only `SfEffects.Ripple` can be repeated. Other effects ignore the `repeat` parameter.

### Use Cases

#### 1. Notification Indicator

```csharp
private void ShowNotificationBadge()
{
    notificationBadge.IsVisible = true;
    
    // Pulse effect to draw attention
    notificationEffectsView.ApplyEffects(
        SfEffects.Ripple,
        RippleStartPosition.Default,
        null,
        repeat: true);
}

private void ClearNotifications()
{
    notificationEffectsView.Reset();
    notificationBadge.IsVisible = false;
}
```

#### 2. Simulated Button Click

```csharp
public void SimulateButtonClick()
{
    // Trigger ripple as if user tapped
    submitButton.ApplyEffects(SfEffects.Ripple);
    
    // Execute button action after animation
    Device.StartTimer(TimeSpan.FromMilliseconds(600), () =>
    {
        ExecuteSubmit();
        return false; // Stop timer
    });
}
```

#### 3. Tutorial Highlight

```csharp
private async Task ShowTutorial()
{
    // Highlight important button
    importantButton.ApplyEffects(
        SfEffects.Highlight,
        RippleStartPosition.Default);
    
    await DisplayAlert("Tip", "Tap this button to continue", "OK");
    
    importantButton.Reset();
}
```

#### 4. Success Feedback

```csharp
private async Task OnFormSubmitted()
{
    // Show success animation
    submitButton.ApplyEffects(SfEffects.Scale);
    
    await Task.Delay(300);
    
    await DisplayAlert("Success", "Form submitted!", "OK");
}
```

#### 5. Breathing Animation

```csharp
private void StartBreathingAnimation()
{
    // Continuous pulse on important element
    callToActionEffectsView.ApplyEffects(
        SfEffects.Ripple,
        repeat: true);
}

private void StopBreathingAnimation()
{
    callToActionEffectsView.Reset();
}
```

### Important Notes

**Effects Persist:** Effects applied via `ApplyEffects()` do NOT automatically reset. You must call `Reset()` to remove them.

```csharp
// Apply highlight
effectsView.ApplyEffects(SfEffects.Highlight);

// Highlight remains until explicitly reset
await Task.Delay(2000);
effectsView.Reset();  // Remove highlight
```

**Ripple Only for Repeat:** Only ripple effects support the `repeat` parameter:

```csharp
// ✓ Works: Ripple can repeat
effectsView.ApplyEffects(SfEffects.Ripple, repeat: true);

// ✗ Ignored: Scale won't repeat
effectsView.ApplyEffects(SfEffects.Scale, repeat: true);
```

## Reset Method

Removes effects that were applied using `ApplyEffects()`.

### Method Signature

```csharp
public void Reset()
```

### Basic Usage

```csharp
// Apply effect
effectsView.ApplyEffects(SfEffects.Highlight);

// Remove effect
effectsView.Reset();
```

### What Gets Reset

The `Reset()` method removes:
- ✓ `SfEffects.Ripple` (including repeating ripples)
- ✓ `SfEffects.Highlight`

The `Reset()` method does NOT affect:
- ✗ `SfEffects.Selection` (use `IsSelected = false` instead)
- ✗ `SfEffects.Scale`
- ✗ `SfEffects.Rotation`

### Use Cases

#### 1. Timed Highlight

```csharp
private async Task HighlightTemporarily(SfEffectsView effectsView, int milliseconds = 2000)
{
    effectsView.ApplyEffects(SfEffects.Highlight);
    await Task.Delay(milliseconds);
    effectsView.Reset();
}
```

#### 2. Stop Repeating Ripple

```csharp
private void StartNotificationAlert()
{
    notificationEffectsView.ApplyEffects(
        SfEffects.Ripple,
        repeat: true);
}

private void AcknowledgeNotification()
{
    notificationEffectsView.Reset();  // Stop pulsing
}
```

#### 3. Conditional Reset

```csharp
private void OnDataLoaded()
{
    if (isLoadingIndicatorActive)
    {
        loadingEffectsView.Reset();
        isLoadingIndicatorActive = false;
    }
}
```

## Programmatic Effect Control

### Complete Control Example

```csharp
public class EffectController
{
    private SfEffectsView _effectsView;
    private bool _isAnimating;
    
    public EffectController(SfEffectsView effectsView)
    {
        _effectsView = effectsView;
    }
    
    public void StartPulse()
    {
        if (_isAnimating) return;
        
        _effectsView.ApplyEffects(
            SfEffects.Ripple,
            RippleStartPosition.Default,
            null,
            repeat: true);
        
        _isAnimating = true;
    }
    
    public void StopPulse()
    {
        if (!_isAnimating) return;
        
        _effectsView.Reset();
        _isAnimating = false;
    }
    
    public async Task FlashHighlight(int durationMs = 500)
    {
        _effectsView.ApplyEffects(SfEffects.Highlight);
        await Task.Delay(durationMs);
        _effectsView.Reset();
    }
    
    public void TriggerRippleAt(Point location)
    {
        _effectsView.ApplyEffects(
            SfEffects.Ripple,
            RippleStartPosition.Default,
            location);
    }
}
```

**Usage:**

```csharp
var controller = new EffectController(myEffectsView);

// Start pulsing animation
controller.StartPulse();

// Flash highlight for 1 second
await controller.FlashHighlight(1000);

// Trigger ripple at specific point
controller.TriggerRippleAt(new Point(100, 50));

// Stop all animations
controller.StopPulse();
```

### Combining Commands and Methods

```csharp
public class InteractiveViewModel : INotifyPropertyChanged
{
    private SfEffectsView _effectsView;
    
    public ICommand TapCommand { get; }
    
    public InteractiveViewModel(SfEffectsView effectsView)
    {
        _effectsView = effectsView;
        
        TapCommand = new Command(async () =>
        {
            // Programmatically trigger effect
            _effectsView.ApplyEffects(SfEffects.Ripple);
            
            // Wait for animation
            await Task.Delay(600);
            
            // Execute action
            await NavigateToNextPage();
        });
    }
}
```

### Animation Sequences

```csharp
private async Task PlayWelcomeSequence()
{
    // Step 1: Highlight welcome button
    welcomeButton.ApplyEffects(SfEffects.Highlight);
    await Task.Delay(1000);
    welcomeButton.Reset();
    
    // Step 2: Ripple on start button
    startButton.ApplyEffects(SfEffects.Ripple);
    await Task.Delay(800);
    
    // Step 3: Scale profile icon
    profileIcon.ApplyEffects(SfEffects.Scale);
    await Task.Delay(500);
}
```

### Conditional Effect Application

```csharp
private void ApplyContextualEffect(ItemStatus status)
{
    switch (status)
    {
        case ItemStatus.New:
            itemEffectsView.ApplyEffects(
                SfEffects.Ripple, 
                repeat: true);  // Pulse for new items
            break;
            
        case ItemStatus.Warning:
            itemEffectsView.ApplyEffects(SfEffects.Highlight);
            break;
            
        case ItemStatus.Complete:
            itemEffectsView.Reset();  // No effect
            break;
    }
}
```

## Best Practices

### Commands

1. **Use Commands for MVVM:** Prefer commands over event handlers for cleaner architecture
2. **Async Commands:** Use async command implementations for long-running operations
3. **CanExecute:** Leverage CanExecute to disable commands when appropriate
4. **Parameter Typing:** Use generic Command<T> for type-safe parameters
5. **Unsubscribe:** Clean up command bindings when disposing ViewModels

### Methods

1. **Reset After Apply:** Always call `Reset()` after using `ApplyEffects()` when effect should be temporary
2. **Check State:** Track whether effects are active to avoid conflicts
3. **Timing:** Use `Task.Delay()` to coordinate with animation durations
4. **Repeat Sparingly:** Use repeating ripple only for important notifications
5. **Selection Separately:** Use `IsSelected` property for selection state, not `ApplyEffects()`
