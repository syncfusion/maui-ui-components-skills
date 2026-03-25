# Events and Handlers

Comprehensive guide to handling events in SfEffectsView for responding to touch interactions and animation states.

## Table of Contents
- [AnimationCompleted Event](#animationcompleted-event)
- [SelectionChanged Event](#selectionchanged-event)
- [LongPressed Event](#longpressed-event)
- [TouchDown Event](#touchdown-event)
- [TouchUp Event](#touchup-event)
- [Event Timing and Lifecycle](#event-timing-and-lifecycle)
- [Combining Events with Effects](#combining-events-with-effects)

## AnimationCompleted Event

Fires when rendered effects have completed their animation.

**Event Type:** `EventHandler`  
**Timing:** Fires on touch up for direct interactions, or immediately when triggered programmatically

### Basic Usage

```xaml
<effectsView:SfEffectsView TouchDownEffects="Ripple"
                            AnimationCompleted="OnAnimationCompleted">
    <Border BackgroundColor="#2196F3" Padding="20">
        <Label Text="Tap Me" TextColor="White" />
    </Border>
</effectsView:SfEffectsView>
```

```csharp
private void OnAnimationCompleted(object sender, EventArgs e)
{
    // Animation has finished
    DisplayAlert("Info", "Animation completed!", "OK");
}
```

### C# Event Subscription

```csharp
var effectsView = new SfEffectsView
{
    TouchDownEffects = SfEffects.Ripple
};

effectsView.AnimationCompleted += (sender, args) =>
{
    // Handle completion
    Console.WriteLine("Effects animation completed");
};
```

### When It Fires

**Touch Interaction:**
```
User touches down
    ↓
Effects begin
    ↓
User releases (touch up)
    ↓
Effects complete animation
    ↓
AnimationCompleted fires ← HERE
```

**Programmatic Trigger:**
```
Code sets IsSelected = true
    ↓
Selection appears immediately
    ↓
AnimationCompleted fires ← HERE (immediately, no delay)
```

### Effect-Specific Behavior

| Effect | Fires? | When |
|--------|--------|------|
| Ripple | Yes | When ripple expansion completes |
| Highlight | Yes | When highlight appears/disappears |
| Scale | Yes | When scale animation completes |
| Rotation | Yes | When rotation completes |
| Selection | **No** | Selection is persistent, not animated |

**Important:** `AnimationCompleted` does NOT fire for `SfEffects.Selection` because selection is a persistent state, not a temporary animation.

### Use Cases

#### 1. Navigation After Feedback

```xaml
<effectsView:SfEffectsView TouchDownEffects="Ripple"
                            AnimationCompleted="OnNavigateToDetails">
    <Border BackgroundColor="White" Padding="15">
        <Grid>
            <Label Text="Product Name" FontSize="16" />
            <Label Text=">" HorizontalOptions="End" FontSize="20" />
        </Grid>
    </Border>
</effectsView:SfEffectsView>
```

```csharp
private async void OnNavigateToDetails(object sender, EventArgs e)
{
    // Wait for ripple to complete, then navigate
    await Navigation.PushAsync(new DetailsPage());
}
```

#### 2. Form Submission

```xaml
<effectsView:SfEffectsView TouchDownEffects="Ripple, Scale"
                            ScaleFactor="0.95"
                            AnimationCompleted="OnSubmitForm">
    <Border BackgroundColor="#6200EE" Padding="30,15">
        <Label Text="SUBMIT" TextColor="White" FontAttributes="Bold" />
    </Border>
</effectsView:SfEffectsView>
```

```csharp
private async void OnSubmitForm(object sender, EventArgs e)
{
    // Provide visual feedback first, then submit
    if (ValidateForm())
    {
        await SubmitFormData();
        await DisplayAlert("Success", "Form submitted!", "OK");
    }
}
```

#### 3. Sequential Actions

```csharp
private async void OnAnimationCompleted(object sender, EventArgs e)
{
    // Chain actions after animation
    await Task.Delay(100);  // Small pause
    await PerformAction();
    UpdateUI();
}
```

### Multiple Effect Handling

When multiple effects are combined, `AnimationCompleted` fires once all effects have finished:

```xaml
<effectsView:SfEffectsView TouchDownEffects="Ripple, Scale, Rotation"
                            RippleAnimationDuration="600"
                            ScaleAnimationDuration="400"
                            RotationAnimationDuration="800"
                            AnimationCompleted="OnAllEffectsCompleted">
    <Label Text="Combined Effects" Padding="20" />
</effectsView:SfEffectsView>
```

```csharp
private void OnAllEffectsCompleted(object sender, EventArgs e)
{
    // Fires after the LONGEST animation completes
    // In this case, after Rotation (800ms)
}
```

## SelectionChanged Event

Fires when the SfEffectsView selection state changes (selected or unselected).

**Event Type:** `EventHandler`  
**Triggers:** Both gesture-based selection and programmatic `IsSelected` changes

### Basic Usage

```xaml
<effectsView:SfEffectsView LongPressEffects="Selection"
                            SelectionChanged="OnSelectionChanged"
                            SelectionBackground="#BBDEFB">
    <Border Padding="15" BackgroundColor="White">
        <Label Text="Long press to select" FontSize="16" />
    </Border>
</effectsView:SfEffectsView>
```

```csharp
private void OnSelectionChanged(object sender, EventArgs e)
{
    var effectsView = sender as SfEffectsView;
    bool isSelected = effectsView.IsSelected;
    
    if (isSelected)
    {
        Console.WriteLine("Item selected");
    }
    else
    {
        Console.WriteLine("Item deselected");
    }
}
```

### C# Event Subscription

```csharp
var effectsView = new SfEffectsView
{
    LongPressEffects = SfEffects.Selection,
    SelectionBackground = new SolidColorBrush(Color.FromArgb("#BBDEFB"))
};

effectsView.SelectionChanged += (sender, args) =>
{
    var view = sender as SfEffectsView;
    UpdateSelectionUI(view.IsSelected);
};
```

### Triggers

`SelectionChanged` fires in two scenarios:

#### 1. Gesture-Based Selection

```
User long-presses
    ↓
Selection effect renders
    ↓
IsSelected toggles
    ↓
SelectionChanged fires ← HERE
```

#### 2. Programmatic Selection

```csharp
effectsView.IsSelected = true;  // SelectionChanged fires
effectsView.IsSelected = false; // SelectionChanged fires again
```

### Use Cases

#### 1. Multi-Select List

```xaml
<CollectionView ItemsSource="{Binding Items}">
    <CollectionView.ItemTemplate>
        <DataTemplate x:DataType="local:ItemViewModel">
            <effectsView:SfEffectsView LongPressEffects="Selection"
                                        IsSelected="{Binding IsSelected, Mode=TwoWay}"
                                        SelectionChanged="OnItemSelectionChanged"
                                        SelectionBackground="#C8E6C9">
                <Grid Padding="15">
                    <Label Text="{Binding Name}" FontSize="16" />
                    <Label Text="✓" 
                           IsVisible="{Binding IsSelected}"
                           HorizontalOptions="End"
                           FontSize="20"
                           TextColor="Green" />
                </Grid>
            </effectsView:SfEffectsView>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

```csharp
private void OnItemSelectionChanged(object sender, EventArgs e)
{
    var effectsView = sender as SfEffectsView;
    var item = effectsView.BindingContext as ItemViewModel;
    
    if (effectsView.IsSelected)
    {
        ViewModel.SelectedItems.Add(item);
    }
    else
    {
        ViewModel.SelectedItems.Remove(item);
    }
    
    UpdateSelectionCount();
}
```

#### 2. Single Selection Mode

Allow only one item to be selected at a time:

```csharp
private SfEffectsView _currentlySelected;

private void OnSelectionChanged(object sender, EventArgs e)
{
    var effectsView = sender as SfEffectsView;
    
    if (effectsView.IsSelected)
    {
        // Deselect previously selected item
        if (_currentlySelected != null && _currentlySelected != effectsView)
        {
            _currentlySelected.IsSelected = false;
        }
        
        _currentlySelected = effectsView;
    }
    else if (_currentlySelected == effectsView)
    {
        _currentlySelected = null;
    }
}
```

#### 3. Selection Count Badge

```xaml
<Grid>
    <Label x:Name="selectionBadge"
           Text="{Binding SelectedCount}"
           BackgroundColor="#FF5722"
           TextColor="White"
           Padding="8,4"
           FontSize="12"
           HorizontalOptions="End"
           VerticalOptions="Start" />
    
    <!-- List with selectable items -->
</Grid>
```

```csharp
private void OnItemSelectionChanged(object sender, EventArgs e)
{
    int selectedCount = GetSelectedItemCount();
    selectionBadge.Text = selectedCount.ToString();
    selectionBadge.IsVisible = selectedCount > 0;
}
```

#### 4. Contextual Actions

Show/hide action buttons based on selection:

```csharp
private void OnSelectionChanged(object sender, EventArgs e)
{
    bool hasSelection = GetSelectedItemCount() > 0;
    
    deleteButton.IsVisible = hasSelection;
    shareButton.IsVisible = hasSelection;
    editButton.IsVisible = hasSelection;
}
```

## LongPressed Event

Fires when the user performs a long press gesture within the SfEffectsView bounds.

**Event Type:** `EventHandler`  
**Duration:** Approximately 500ms press-and-hold (platform-specific)

### Basic Usage

```xaml
<effectsView:SfEffectsView TouchDownEffects="Ripple"
                            TouchUpEffects="Scale"
                            LongPressed="OnLongPressed">
    <Border BackgroundColor="White" Padding="20">
        <Label Text="Long press for options" />
    </Border>
</effectsView:SfEffectsView>
```

```csharp
private async void OnLongPressed(object sender, EventArgs e)
{
    string action = await DisplayActionSheet(
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
```

### C# Event Subscription

```csharp
var effectsView = new SfEffectsView
{
    TouchDownEffects = SfEffects.Ripple,
    LongPressEffects = SfEffects.Scale
};

effectsView.LongPressed += async (sender, args) =>
{
    await ShowContextMenu();
};
```

### Use Cases

#### 1. Context Menu

```csharp
private async void OnLongPressed(object sender, EventArgs e)
{
    var effectsView = sender as SfEffectsView;
    var item = effectsView.BindingContext;
    
    string action = await DisplayActionSheet(
        "Item Options",
        "Cancel",
        "Delete",
        "Edit",
        "Share",
        "Copy");
    
    switch (action)
    {
        case "Edit":
            await Navigation.PushAsync(new EditPage(item));
            break;
        case "Share":
            await ShareItem(item);
            break;
        case "Copy":
            await CopyItem(item);
            break;
        case "Delete":
            await DeleteItem(item);
            break;
    }
}
```

#### 2. Preview/Peek

```csharp
private async void OnLongPressed(object sender, EventArgs e)
{
    var effectsView = sender as SfEffectsView;
    var imageSource = effectsView.Content as Image;
    
    // Show full-screen preview
    await Navigation.PushModalAsync(new ImagePreviewPage(imageSource.Source));
}
```

#### 3. Reorder Mode

```csharp
private void OnLongPressed(object sender, EventArgs e)
{
    var effectsView = sender as SfEffectsView;
    
    // Enter reorder mode
    EnableReorderMode();
    HighlightItem(effectsView);
}
```

### LongPressed vs LongPressEffects

**Important distinction:**

- `LongPressed` event: Code execution on long press
- `LongPressEffects` property: Visual effect on long press

You can use both together:

```xaml
<effectsView:SfEffectsView LongPressEffects="Selection"
                            LongPressed="OnLongPressed">
    <Label Text="Long press for selection + action" Padding="15" />
</effectsView:SfEffectsView>
```

```csharp
private void OnLongPressed(object sender, EventArgs e)
{
    // Selection effect renders automatically (LongPressEffects="Selection")
    // AND this code executes
    ShowItemDetails();
}
```

## TouchDown Event

Fires when the user touches down (presses) within the SfEffectsView bounds.

**Event Type:** `EventHandler`  
**Timing:** Fires immediately on touch start

### Basic Usage

```xaml
<effectsView:SfEffectsView TouchDownEffects="Ripple"
                            TouchDown="OnTouchDown">
    <Border BackgroundColor="#2196F3" Padding="20">
        <Label Text="Tap Me" TextColor="White" />
    </Border>
</effectsView:SfEffectsView>
```

```csharp
private void OnTouchDown(object sender, EventArgs e)
{
    Console.WriteLine("Touch started");
    // Perform action on touch start
}
```

### C# Event Subscription

```csharp
var effectsView = new SfEffectsView
{
    TouchDownEffects = SfEffects.Ripple
};

effectsView.TouchDown += (sender, args) =>
{
    StartFeedback();
};
```

### Use Cases

#### 1. Audio Feedback

```csharp
private void OnTouchDown(object sender, EventArgs e)
{
    // Play sound on touch
    PlayTouchSound();
}
```

#### 2. Loading Indicator

```csharp
private void OnTouchDown(object sender, EventArgs e)
{
    // Show loading spinner
    loadingIndicator.IsVisible = true;
    loadingIndicator.IsRunning = true;
}
```

#### 3. Analytics Tracking

```csharp
private void OnTouchDown(object sender, EventArgs e)
{
    var effectsView = sender as SfEffectsView;
    var item = effectsView.BindingContext;
    
    // Track interaction
    Analytics.TrackEvent("ItemTouched", new Dictionary<string, string>
    {
        { "ItemId", item.Id },
        { "Timestamp", DateTime.Now.ToString() }
    });
}
```

## TouchUp Event

Fires when the user releases the touch (lifts finger).

**Event Type:** `EventHandler`  
**Timing:** Fires on touch end

### Basic Usage

```xaml
<effectsView:SfEffectsView TouchUpEffects="Scale"
                            TouchUp="OnTouchUp">
    <Border BackgroundColor="#4CAF50" Padding="20">
        <Label Text="Release Me" TextColor="White" />
    </Border>
</effectsView:SfEffectsView>
```

```csharp
private void OnTouchUp(object sender, EventArgs e)
{
    Console.WriteLine("Touch ended");
    // Perform action on touch release
}
```

### C# Event Subscription

```csharp
var effectsView = new SfEffectsView
{
    TouchUpEffects = SfEffects.Scale
};

effectsView.TouchUp += (sender, args) =>
{
    CompleteFeedback();
};
```

### Use Cases

#### 1. Button Action

```csharp
private async void OnTouchUp(object sender, EventArgs e)
{
    // Execute button action on release
    await PerformButtonAction();
}
```

#### 2. Stop Loading

```csharp
private void OnTouchUp(object sender, EventArgs e)
{
    // Hide loading indicator
    loadingIndicator.IsRunning = false;
    loadingIndicator.IsVisible = false;
}
```

#### 3. Touch Duration Tracking

```csharp
private DateTime _touchStartTime;

private void OnTouchDown(object sender, EventArgs e)
{
    _touchStartTime = DateTime.Now;
}

private void OnTouchUp(object sender, EventArgs e)
{
    var duration = DateTime.Now - _touchStartTime;
    Console.WriteLine($"Touch duration: {duration.TotalMilliseconds}ms");
}
```

## Event Timing and Lifecycle

### Event Order

Complete touch interaction timeline:

```
User Touches Down
    ↓
TouchDown event fires ← 1st
    ↓
TouchDownEffects begin rendering
    ↓
[User holds for ~500ms]
    ↓
LongPressed event fires ← 2nd (if held long enough)
LongPressEffects begin rendering
    ↓
User Releases (Touch Up)
    ↓
TouchUp event fires ← 3rd
    ↓
TouchUpEffects begin rendering
    ↓
Effects complete
    ↓
AnimationCompleted event fires ← 4th
```

### Selection Event Flow

```
User Long-Presses
    ↓
LongPressed fires
    ↓
LongPressEffects (Selection) renders
    ↓
IsSelected toggles
    ↓
SelectionChanged fires
```

### Event Timing Example

```csharp
public partial class EventTimingPage : ContentPage
{
    public EventTimingPage()
    {
        InitializeComponent();
        
        var effectsView = new SfEffectsView
        {
            TouchDownEffects = SfEffects.Ripple,
            LongPressEffects = SfEffects.Selection,
            TouchUpEffects = SfEffects.Scale
        };
        
        effectsView.TouchDown += (s, e) => 
            Log("1. TouchDown");
        
        effectsView.LongPressed += (s, e) => 
            Log("2. LongPressed");
        
        effectsView.SelectionChanged += (s, e) => 
            Log("3. SelectionChanged");
        
        effectsView.TouchUp += (s, e) => 
            Log("4. TouchUp");
        
        effectsView.AnimationCompleted += (s, e) => 
            Log("5. AnimationCompleted");
    }
    
    private void Log(string message)
    {
        Console.WriteLine($"[{DateTime.Now:HH:mm:ss.fff}] {message}");
    }
}
```

## Combining Events with Effects

### Navigation with Feedback

```xaml
<effectsView:SfEffectsView TouchDownEffects="Ripple"
                            AnimationCompleted="NavigateToDetails">
    <Grid Padding="15">
        <Label Text="Product Name" FontSize="16" />
        <Label Text="$99.99" FontAttributes="Bold" HorizontalOptions="End" />
    </Grid>
</effectsView:SfEffectsView>
```

```csharp
private async void NavigateToDetails(object sender, EventArgs e)
{
    var effectsView = sender as SfEffectsView;
    var product = effectsView.BindingContext as Product;
    await Navigation.PushAsync(new ProductDetailsPage(product));
}
```

### Selection with Confirmation

```xaml
<effectsView:SfEffectsView LongPressEffects="Selection"
                            SelectionChanged="OnSelectionChanged"
                            SelectionBackground="#BBDEFB">
    <Grid Padding="15">
        <Label Text="Long press to select" />
    </Grid>
</effectsView:SfEffectsView>
```

```csharp
private async void OnSelectionChanged(object sender, EventArgs e)
{
    var effectsView = sender as SfEffectsView;
    
    if (effectsView.IsSelected)
    {
        bool confirmed = await DisplayAlert(
            "Confirm",
            "Add this item to selection?",
            "Yes",
            "No");
        
        if (!confirmed)
        {
            effectsView.IsSelected = false;  // Revert
        }
    }
}
```

### Touch-Hold Action

```csharp
private CancellationTokenSource _holdCts;

private async void OnTouchDown(object sender, EventArgs e)
{
    _holdCts = new CancellationTokenSource();
    
    try
    {
        // Start action after 2 seconds of holding
        await Task.Delay(2000, _holdCts.Token);
        ExecuteHoldAction();
    }
    catch (TaskCanceledException)
    {
        // User released before 2 seconds
    }
}

private void OnTouchUp(object sender, EventArgs e)
{
    // Cancel hold action if user releases
    _holdCts?.Cancel();
}
```

### Multi-Event Coordination

```csharp
private bool _isProcessing = false;

private void OnTouchDown(object sender, EventArgs e)
{
    if (_isProcessing) return;
    
    ShowFeedback();
}

private async void OnTouchUp(object sender, EventArgs e)
{
    if (_isProcessing) return;
    
    _isProcessing = true;
    await ProcessAction();
    _isProcessing = false;
}

private void OnAnimationCompleted(object sender, EventArgs e)
{
    // Clean up after all animations
    HideFeedback();
}
```

### Error Handling

```csharp
private async void OnAnimationCompleted(object sender, EventArgs e)
{
    try
    {
        await SubmitData();
    }
    catch (Exception ex)
    {
        await DisplayAlert("Error", ex.Message, "OK");
        
        // Reset state
        var effectsView = sender as SfEffectsView;
        effectsView.IsSelected = false;
    }
}
```
