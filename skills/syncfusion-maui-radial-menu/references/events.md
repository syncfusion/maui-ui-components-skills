# Events in MAUI Radial Menu

## Table of Contents
- [Navigation Events](#navigation-events)
  - [Navigating](#navigating)
  - [Navigated](#navigated)
- [Opening and Closing Events](#opening-and-closing-events)
  - [Opening](#opening)
  - [Opened](#opened)
  - [Closing](#closing)
  - [Closed](#closed)
- [Center Button Events](#center-button-events)
  - [CenterButtonBackTapped](#centerbuttonbacktapped)
- [Item Interaction Events](#item-interaction-events)
  - [ItemTapped](#itemtapped)
  - [TouchDown](#touchdown)
  - [TouchUP](#touchup)
- [Event Patterns](#event-patterns)

The Radial Menu provides a comprehensive event system for handling user interactions, navigation between levels, and menu state changes.

## Navigation Events

Navigation events fire when moving between hierarchical levels in the menu (from outer rim to inner rim and back).

### Navigating

Occurs when the menu begins to navigate from one level to another. This event can be cancelled.

**XAML:**
```xaml
<syncfusion:SfRadialMenu Navigating="SfRadialMenu_Navigating">
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Bold" FontSize="12"/>
        <syncfusion:SfRadialMenuItem Text="Color" FontSize="12">
            <syncfusion:SfRadialMenuItem.Items>
                <syncfusion:SfRadialMenuItem Text="Red" FontSize="12"/>
                <syncfusion:SfRadialMenuItem Text="Blue" FontSize="12"/>
            </syncfusion:SfRadialMenuItem.Items>
        </syncfusion:SfRadialMenuItem>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu();
radialMenu.Navigating += SfRadialMenu_Navigating;
this.Content = radialMenu;

private async void SfRadialMenu_Navigating(object sender, NavigatingEventArgs e)
{
    // Display alert before navigation
    await DisplayAlert("Navigation", "Navigating to next level", "OK");
    
    // Optionally cancel navigation
    // e.Cancel = true;
}
```

**NavigatingEventArgs Properties:**
- **Cancel** (bool): Set to `true` to prevent navigation

**Use Cases:**
- Validate before allowing nested menu access
- Show warnings or confirmations
- Log navigation analytics
- Implement custom navigation logic
- Prevent navigation based on conditions

**Example: Conditional Navigation**
```csharp
private void SfRadialMenu_Navigating(object sender, NavigatingEventArgs e)
{
    // Only allow navigation if user has permission
    if (!UserHasAccess())
    {
        e.Cancel = true;
        DisplayAlert("Access Denied", "You don't have permission", "OK");
    }
}
```

### Navigated

Occurs after the menu has successfully navigated to a new level.

**XAML:**
```xaml
<syncfusion:SfRadialMenu Navigated="SfRadialMenu_Navigated">
    <!-- Items here -->
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu();
radialMenu.Navigated += SfRadialMenu_Navigated;

private async void SfRadialMenu_Navigated(object sender, NavigatedEventArgs e)
{
    // Perform action after navigation completes
    await DisplayAlert("Navigation", "Arrived at new level", "OK");
    
    // Update UI or state
    UpdateBreadcrumb();
}
```

**Use Cases:**
- Update breadcrumb or navigation indicators
- Trigger animations or visual feedback
- Load additional data for the level
- Log completed navigation
- Update application state

**Example: Track Navigation Depth**
```csharp
private int navigationDepth = 0;

private void SfRadialMenu_Navigated(object sender, NavigatedEventArgs e)
{
    navigationDepth++;
    breadcrumbLabel.Text = $"Level {navigationDepth}";
}
```

## Opening and Closing Events

These events track the menu's open/close state changes.

### Opening

Occurs when the menu begins to open. Can be used to prepare data or cancel opening.

**XAML:**
```xaml
<syncfusion:SfRadialMenu Opening="SfRadialMenu_Opening">
    <!-- Items here -->
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu();
radialMenu.Opening += SfRadialMenu_Opening;

private async void SfRadialMenu_Opening(object sender, OpeningEventArgs e)
{
    await DisplayAlert("Alert", "Menu is opening", "OK");
    
    // Prepare data or resources
    LoadMenuData();
}
```

**Use Cases:**
- Load dynamic menu items
- Update item states
- Show loading indicators
- Log menu usage
- Prepare resources

### Opened

Occurs after the menu is fully opened.

**XAML:**
```xaml
<syncfusion:SfRadialMenu Opened="SfRadialMenu_Opened">
    <!-- Items here -->
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu();
radialMenu.Opened += SfRadialMenu_Opened;

private async void SfRadialMenu_Opened(object sender, OpenedEventArgs e)
{
    await DisplayAlert("Alert", "Menu is now open", "OK");
    
    // Start animations or effects
    StartGlowEffect();
}
```

**Use Cases:**
- Trigger post-open animations
- Start timers or auto-close logic
- Update UI indicators
- Track usage statistics
- Enable contextual features

**Example: Auto-Close Timer**
```csharp
private CancellationTokenSource autoCloseTimer;

private async void SfRadialMenu_Opened(object sender, OpenedEventArgs e)
{
    // Start 5-second auto-close timer
    autoCloseTimer = new CancellationTokenSource();
    try
    {
        await Task.Delay(5000, autoCloseTimer.Token);
        radialMenu.IsOpen = false;
    }
    catch (TaskCanceledException)
    {
        // Timer was cancelled
    }
}
```

### Closing

Occurs when the menu begins to close.

**XAML:**
```xaml
<syncfusion:SfRadialMenu Closing="SfRadialMenu_Closing">
    <!-- Items here -->
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu();
radialMenu.Closing += SfRadialMenu_Closing;

private async void SfRadialMenu_Closing(object sender, ClosingEventArgs e)
{
    await DisplayAlert("Alert", "Menu is closing", "OK");
    
    // Cancel auto-close timer
    autoCloseTimer?.Cancel();
}
```

**Use Cases:**
- Cancel timers
- Save state
- Clean up resources
- Log session duration
- Prepare for closed state

### Closed

Occurs after the menu is fully closed.

**XAML:**
```xaml
<syncfusion:SfRadialMenu Closed="SfRadialMenu_Closed">
    <!-- Items here -->
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu();
radialMenu.Closed += SfRadialMenu_Closed;

private async void SfRadialMenu_Closed(object sender, ClosedEventArgs e)
{
    await DisplayAlert("Alert", "Menu is now closed", "OK");
    
    // Reset UI or state
    ResetMenuState();
}
```

**Use Cases:**
- Reset menu to initial state
- Clear selections
- Release resources
- Update indicators
- Track total interaction time

**Example: Track Interaction Time**
```csharp
private DateTime menuOpenedTime;

private void SfRadialMenu_Opened(object sender, OpenedEventArgs e)
{
    menuOpenedTime = DateTime.Now;
}

private void SfRadialMenu_Closed(object sender, ClosedEventArgs e)
{
    TimeSpan duration = DateTime.Now - menuOpenedTime;
    Analytics.TrackMenuUsage(duration);
}
```

## Center Button Events

### CenterButtonBackTapped

Occurs when the center back button is tapped (when viewing nested items).

**XAML:**
```xaml
<syncfusion:SfRadialMenu CenterButtonBackTapped="SfRadialMenu_CenterButtonBackTapped">
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Bold" FontSize="12"/>
        <syncfusion:SfRadialMenuItem Text="Color" FontSize="12">
            <syncfusion:SfRadialMenuItem.Items>
                <syncfusion:SfRadialMenuItem Text="Red" FontSize="12"/>
            </syncfusion:SfRadialMenuItem.Items>
        </syncfusion:SfRadialMenuItem>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu();
radialMenu.CenterButtonBackTapped += SfRadialMenu_CenterButtonBackTapped;

private async void SfRadialMenu_CenterButtonBackTapped(object sender, CenterButtonBackTappedEventArgs e)
{
    await DisplayAlert("Alert", "Back button tapped", "OK");
    
    // Navigate back or close menu
    // Default behavior handles navigation automatically
}
```

**Use Cases:**
- Track back navigation
- Trigger animations on back
- Update navigation indicators
- Log user navigation patterns
- Custom back behavior

**Example: Breadcrumb Update**
```csharp
private Stack<string> navigationStack = new Stack<string>();

private void SfRadialMenu_CenterButtonBackTapped(object sender, CenterButtonBackTappedEventArgs e)
{
    if (navigationStack.Count > 0)
    {
        navigationStack.Pop();
        UpdateBreadcrumb();
    }
}
```

## Item Interaction Events

### ItemTapped

Occurs when a menu item is tapped.

**XAML:**
```xaml
<syncfusion:SfRadialMenu>
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Bold" 
                                     FontSize="12" 
                                     ItemTapped="SfRadialMenuItem_ItemTapped"/>
        <syncfusion:SfRadialMenuItem Text="Italic" 
                                     FontSize="12" 
                                     ItemTapped="SfRadialMenuItem_ItemTapped"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu();

RadialMenuItemsCollection itemCollection = new RadialMenuItemsCollection
{
    new SfRadialMenuItem { Text = "Bold", FontSize = 12 }
};

radialMenu.Items = itemCollection;
radialMenu.Items[0].ItemTapped += SfRadialMenuItem_ItemTapped;

private async void SfRadialMenuItem_ItemTapped(object sender, ItemTappedEventArgs e)
{
    await DisplayAlert("Alert", "Item was tapped", "OK");
    
    // Perform action based on item
    var item = sender as SfRadialMenuItem;
    PerformAction(item.Text);
}
```

**Use Cases:**
- Execute item action
- Navigate to screens
- Toggle states
- Update selection
- Trigger commands

**Example: Handle Multiple Items**
```csharp
private void CreateMenuWithHandlers()
{
    var boldItem = new SfRadialMenuItem { Text = "Bold", FontSize = 12 };
    var italicItem = new SfRadialMenuItem { Text = "Italic", FontSize = 12 };
    var underlineItem = new SfRadialMenuItem { Text = "Underline", FontSize = 12 };

    boldItem.ItemTapped += (s, e) => ApplyFormatting("Bold");
    italicItem.ItemTapped += (s, e) => ApplyFormatting("Italic");
    underlineItem.ItemTapped += (s, e) => ApplyFormatting("Underline");

    radialMenu.Items = new RadialMenuItemsCollection 
    { 
        boldItem, italicItem, underlineItem 
    };
}

private void ApplyFormatting(string format)
{
    // Apply formatting to selected text
    Debug.WriteLine($"Applying {format}");
}
```

### TouchDown

Occurs when a menu item is pressed (touch/mouse down).

**XAML:**
```xaml
<syncfusion:SfRadialMenu>
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Bold" 
                                     FontSize="12" 
                                     TouchDown="SfRadialMenuItem_TouchDown"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenuItem boldItem = new SfRadialMenuItem 
{ 
    Text = "Bold", 
    FontSize = 12 
};
boldItem.TouchDown += SfRadialMenuItem_TouchDown;

private async void SfRadialMenuItem_TouchDown(object sender, RadialMenuItemEventArgs e)
{
    await DisplayAlert("Alert", "Item pressed down", "OK");
    
    // Provide haptic feedback
    HapticFeedback.Perform(HapticFeedbackType.Click);
}
```

**Use Cases:**
- Immediate feedback on press
- Haptic feedback
- Visual press states
- Start long-press timers
- Track press timing

### TouchUP

Occurs when a menu item is released (touch/mouse up).

**XAML:**
```xaml
<syncfusion:SfRadialMenu>
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Bold" 
                                     FontSize="12" 
                                     TouchDown="SfRadialMenuItem_TouchDown"
                                     TouchUP="SfRadialMenuItem_TouchUP"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenuItem boldItem = new SfRadialMenuItem 
{ 
    Text = "Bold", 
    FontSize = 12 
};
boldItem.TouchDown += SfRadialMenuItem_TouchDown;
boldItem.TouchUP += SfRadialMenuItem_TouchUP;

private async void SfRadialMenuItem_TouchDown(object sender, RadialMenuItemEventArgs e)
{
    await DisplayAlert("Alert", "Item pressed", "OK");
}

private async void SfRadialMenuItem_TouchUP(object sender, RadialMenuItemEventArgs e)
{
    await DisplayAlert("Alert", "Item released", "OK");
}
```

**Use Cases:**
- Complete press-release cycle
- Measure press duration
- Implement long-press
- Cancel actions if released outside
- Animations on release

**Example: Long Press Detection**
```csharp
private DateTime pressStartTime;
private const int LongPressDurationMs = 500;

private void SfRadialMenuItem_TouchDown(object sender, RadialMenuItemEventArgs e)
{
    pressStartTime = DateTime.Now;
}

private void SfRadialMenuItem_TouchUP(object sender, RadialMenuItemEventArgs e)
{
    TimeSpan pressDuration = DateTime.Now - pressStartTime;
    
    if (pressDuration.TotalMilliseconds >= LongPressDurationMs)
    {
        // Long press detected
        ShowContextMenu();
    }
    else
    {
        // Normal tap
        PerformNormalAction();
    }
}
```

## Event Patterns

### Pattern 1: Menu State Tracking

```csharp
public class MenuStateTracker
{
    private bool isMenuOpen;
    private int currentLevel;

    public void AttachToMenu(SfRadialMenu menu)
    {
        menu.Opening += OnOpening;
        menu.Opened += OnOpened;
        menu.Closing += OnClosing;
        menu.Closed += OnClosed;
        menu.Navigating += OnNavigating;
        menu.Navigated += OnNavigated;
    }

    private void OnOpening(object sender, OpeningEventArgs e)
    {
        Debug.WriteLine("Menu opening...");
    }

    private void OnOpened(object sender, OpenedEventArgs e)
    {
        isMenuOpen = true;
        Debug.WriteLine("Menu opened");
    }

    private void OnClosing(object sender, ClosingEventArgs e)
    {
        Debug.WriteLine("Menu closing...");
    }

    private void OnClosed(object sender, ClosedEventArgs e)
    {
        isMenuOpen = false;
        currentLevel = 0;
        Debug.WriteLine("Menu closed");
    }

    private void OnNavigating(object sender, NavigatingEventArgs e)
    {
        Debug.WriteLine($"Navigating from level {currentLevel}");
    }

    private void OnNavigated(object sender, NavigatedEventArgs e)
    {
        currentLevel++;
        Debug.WriteLine($"Navigated to level {currentLevel}");
    }
}
```

### Pattern 2: Analytics Integration

```csharp
private void SetupAnalytics()
{
    radialMenu.Opened += (s, e) => Analytics.Track("MenuOpened");
    radialMenu.Closed += (s, e) => Analytics.Track("MenuClosed");
    
    foreach (var item in radialMenu.Items)
    {
        item.ItemTapped += (s, e) =>
        {
            var menuItem = s as SfRadialMenuItem;
            Analytics.Track("ItemTapped", new Dictionary<string, string>
            {
                { "ItemText", menuItem.Text }
            });
        };
    }
}
```

### Pattern 3: Undo/Redo with Events

```csharp
private Stack<Action> undoStack = new Stack<Action>();

private void SetupUndoableActions()
{
    foreach (var item in radialMenu.Items)
    {
        item.ItemTapped += (s, e) =>
        {
            var menuItem = s as SfRadialMenuItem;
            
            // Execute action
            var action = GetActionForItem(menuItem);
            action();
            
            // Add to undo stack
            var undoAction = GetUndoActionForItem(menuItem);
            undoStack.Push(undoAction);
        };
    }
}
```

### Pattern 4: Conditional Item Enabling

```csharp
private void SfRadialMenu_Opening(object sender, OpeningEventArgs e)
{
    // Update item states based on context
    UpdateItemStates();
}

private void UpdateItemStates()
{
    var cutItem = radialMenu.Items[0];
    var copyItem = radialMenu.Items[1];
    var pasteItem = radialMenu.Items[2];

    // Enable/disable based on conditions
    cutItem.IsEnabled = HasSelection();
    copyItem.IsEnabled = HasSelection();
    pasteItem.IsEnabled = HasClipboardContent();
}
```

### Pattern 5: Event Chaining

```csharp
private async void SetupEventChain()
{
    radialMenu.Navigating += async (s, e) =>
    {
        // Show loading
        await ShowLoading();
    };

    radialMenu.Navigated += async (s, e) =>
    {
        // Load data for level
        await LoadLevelData();
        
        // Hide loading
        await HideLoading();
    };
}
```

## Complete Example

Full event integration:

```csharp
public partial class MainPage : ContentPage
{
    private SfRadialMenu radialMenu;
    private Label statusLabel;

    public MainPage()
    {
        InitializeComponent();
        CreateMenuWithEvents();
    }

    private void CreateMenuWithEvents()
    {
        statusLabel = new Label
        {
            Text = "Interact with the menu",
            HorizontalOptions = LayoutOptions.Center
        };

        radialMenu = new SfRadialMenu
        {
            CenterButtonText = "Menu"
        };

        // Menu state events
        radialMenu.Opening += (s, e) => UpdateStatus("Opening...");
        radialMenu.Opened += (s, e) => UpdateStatus("Opened");
        radialMenu.Closing += (s, e) => UpdateStatus("Closing...");
        radialMenu.Closed += (s, e) => UpdateStatus("Closed");

        // Navigation events
        radialMenu.Navigating += (s, e) => UpdateStatus("Navigating...");
        radialMenu.Navigated += (s, e) => UpdateStatus("Navigated");
        radialMenu.CenterButtonBackTapped += (s, e) => UpdateStatus("Back tapped");

        // Create items with events
        var boldItem = new SfRadialMenuItem { Text = "Bold", FontSize = 12 };
        boldItem.ItemTapped += (s, e) => UpdateStatus("Bold tapped");
        boldItem.TouchDown += (s, e) => UpdateStatus("Bold pressed");
        boldItem.TouchUP += (s, e) => UpdateStatus("Bold released");

        radialMenu.Items.Add(boldItem);

        // Layout
        Grid grid = new Grid();
        grid.Add(statusLabel);
        grid.Add(radialMenu);
        Content = grid;
    }

    private void UpdateStatus(string message)
    {
        statusLabel.Text = $"{DateTime.Now:HH:mm:ss} - {message}";
    }
}
```
