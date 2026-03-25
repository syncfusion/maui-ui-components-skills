# Dragging and Rotation in MAUI Radial Menu

The Radial Menu can float over content and be dragged anywhere within its parent layout. It also supports rotation of menu items. This guide covers placement, dragging, rotation, and related events.

## Placing Radial Menu

The Radial Menu can be placed anywhere on its parent layout. You can position it using the `Point` property or through standard layout options.

### Using Point Property

The `Point` property allows you to set the exact position of the Radial Menu. The position is calculated based on the parent layout's center point.

**XAML:**
```xaml
<radialMenu:SfRadialMenu x:Name="radialMenu" 
                         Point="100, 150">
    <radialMenu:SfRadialMenu.Items>
        <radialMenu:SfRadialMenuItem Text="Cut" FontSize="12"/>
        <radialMenu:SfRadialMenuItem Text="Copy" FontSize="12"/>
    </radialMenu:SfRadialMenu.Items>
</radialMenu:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu
{
    Point = new Point(100, 150)
};

RadialMenuItemsCollection itemCollection = new RadialMenuItemsCollection
{
    new SfRadialMenuItem { Text = "Cut", FontSize = 12 },
    new SfRadialMenuItem { Text = "Copy", FontSize = 12 }
};

radialMenu.Items = itemCollection;
this.Content = radialMenu;
```

**Point Property Details:**
- Type: `Point` (X, Y coordinates)
- X: Horizontal offset from parent center
- Y: Vertical offset from parent center
- Useful for precise positioning
- Works with dragging enabled or disabled

### Using Layout Options

**Placing in a Grid with Specific Position:**
```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    
    <!-- Your content here -->
    <Label Grid.Row="0" Text="Main Content" 
           VerticalOptions="Center" 
           HorizontalOptions="Center"/>
    
    <!-- Radial Menu in bottom-right corner -->
    <syncfusion:SfRadialMenu Grid.Row="1"
                             HorizontalOptions="End"
                             VerticalOptions="End"
                             Margin="20">
        <syncfusion:SfRadialMenu.Items>
            <syncfusion:SfRadialMenuItem Text="Cut" FontSize="12"/>
            <syncfusion:SfRadialMenuItem Text="Copy" FontSize="12"/>
        </syncfusion:SfRadialMenu.Items>
    </syncfusion:SfRadialMenu>
</Grid>
```

**Best Practices for Placement:**
- Use `Point` property for precise positioning relative to parent center

## Dragging Radial Menu

Enable dragging to allow users to move the menu anywhere within the parent layout.

### IsDragEnabled Property

**XAML:**
```xaml
<syncfusion:SfRadialMenu x:Name="radialMenu" 
                         IsDragEnabled="True"
                         CenterButtonText="Menu">
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Cut" FontSize="12"/>
        <syncfusion:SfRadialMenuItem Text="Copy" FontSize="12"/>
        <syncfusion:SfRadialMenuItem Text="Paste" FontSize="12"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu
{
    IsDragEnabled = true,
    CenterButtonText = "Menu"
};

RadialMenuItemsCollection itemCollection = new RadialMenuItemsCollection
{
    new SfRadialMenuItem { Text = "Cut", FontSize = 12 },
    new SfRadialMenuItem { Text = "Copy", FontSize = 12 },
    new SfRadialMenuItem { Text = "Paste", FontSize = 12 }
};

radialMenu.Items = itemCollection;
this.Content = radialMenu;
```

**Dragging Behavior:**
- User touches and holds the center button
- Drag gesture moves the entire menu
- Menu stays within parent layout bounds
- Released position becomes new location
- Works in both open and closed states

**When to Enable Dragging:**
- Floating toolbar scenarios
- User needs to reposition for comfort
- Menu might obscure content
- Touch-optimized interfaces
- Kiosk or tablet applications

**When to Disable Dragging:**
- Fixed position is preferred
- Menu is anchored to specific content
- Simpler interaction model needed
- Limited screen space

### Toggle Dragging Dynamically

```csharp
// Enable dragging when editing
void OnEditModeEntered()
{
    radialMenu.IsDragEnabled = true;
}

// Disable dragging when viewing
void OnEditModeExited()
{
    radialMenu.IsDragEnabled = false;
}
```

## Rotation

Control whether menu items rotate as the menu opens and closes.

### EnableRotation Property

**XAML:**
```xaml
<syncfusion:SfRadialMenu x:Name="radialMenu" 
                         EnableRotation="True"
                         CenterButtonText="Menu">
    <syncfusion:SfRadialMenu.Items>
        <syncfusion:SfRadialMenuItem Text="Cut" FontSize="12"/>
        <syncfusion:SfRadialMenuItem Text="Copy" FontSize="12"/>
        <syncfusion:SfRadialMenuItem Text="Paste" FontSize="12"/>
    </syncfusion:SfRadialMenu.Items>
</syncfusion:SfRadialMenu>
```

**C#:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu
{
    EnableRotation = true,
    CenterButtonText = "Menu"
};

RadialMenuItemsCollection itemCollection = new RadialMenuItemsCollection
{
    new SfRadialMenuItem { Text = "Cut", FontSize = 12 },
    new SfRadialMenuItem { Text = "Copy", FontSize = 12 },
    new SfRadialMenuItem { Text = "Paste", FontSize = 12 }
};

radialMenu.Items = itemCollection;
this.Content = radialMenu;
```

**Rotation Behavior:**
- `true`: Items rotate during open/close animation
- `false`: Items appear/disappear without rotation
- Default: true
- Affects visual presentation only

**Enable Rotation When:**
- Want dynamic, animated feel
- Modern UI aesthetic
- Smooth transitions preferred
- Performance is adequate

**Disable Rotation When:**
- Performance concerns
- Prefer instant feedback
- Accessibility requirements (reduced motion)
- Simpler animations desired

**Combined with Animation Duration:**
```csharp
SfRadialMenu radialMenu = new SfRadialMenu
{
    EnableRotation = true,
    AnimationDuration = 500 // Slower rotation
};
```

## Drag Events

The Radial Menu provides the `DragBegin` event that triggers when dragging starts. Use this to track position or restrict dragging behavior.

### DragBegin Event

Triggered when the user starts dragging the menu.

**XAML:**
```xaml
<radialMenu:SfRadialMenu x:Name="radialMenu"
                         DragBegin="RadialMenu_DragBegin"
                         IsDragEnabled="True">
    <radialMenu:SfRadialMenu.Items>
        <radialMenu:SfRadialMenuItem Text="Cut" FontSize="12"/>
    </radialMenu:SfRadialMenu.Items>
</radialMenu:SfRadialMenu>
```

**C#:**
```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        SfRadialMenu radialMenu = new SfRadialMenu
        {
            IsDragEnabled = true
        };
        
        radialMenu.DragBegin += RadialMenu_DragBegin;
        this.Content = radialMenu;
    }

    private void RadialMenu_DragBegin(object sender, DragBeginEventArgs e)
    {
        // Access starting position
        Point startPosition = e.Position;
        
        // Log for debugging
        Debug.WriteLine($"Drag started at: X={startPosition.X}, Y={startPosition.Y}");
        
        // Optionally cancel drag
        // e.Handled = true;
    }
}
```

### DragBeginEventArgs Properties

**Position:**
- Type: `Point`
- Gets the starting position of the drag operation
- Coordinates relative to parent layout
- Read-only property

**Handled:**
- Type: `bool`
- Gets or sets whether to allow dragging
- `true`: Cancel drag operation
- `false`: Allow drag to proceed (default)

### Restricting Drag Behavior

**Example: Prevent Dragging in Certain Conditions**

```csharp
private void RadialMenu_DragBegin(object sender, DragBeginEventArgs e)
{
    // Don't allow dragging when menu is open
    if (radialMenu.IsOpen)
    {
        e.Handled = true;
        return;
    }
    
    // Don't allow dragging beyond certain boundary
    if (e.Position.X < 100 || e.Position.X > 700)
    {
        e.Handled = true;
        return;
    }
}
```

**Example: Track Drag Statistics**

```csharp
private int dragCount = 0;
private Point lastDragPosition;

private void RadialMenu_DragBegin(object sender, DragBeginEventArgs e)
{
    dragCount++;
    lastDragPosition = e.Position;
    
    // Log analytics
    Debug.WriteLine($"Drag #{dragCount} from {e.Position}");
    
    // Could send to analytics service
    AnalyticsService.TrackDrag(e.Position);
}
```

**Example: Visual Feedback on Drag Start**

```csharp
private async void RadialMenu_DragBegin(object sender, DragBeginEventArgs e)
{
    // Show feedback that drag started
    await radialMenu.ScaleTo(1.1, 100);
    await radialMenu.ScaleTo(1.0, 100);
}
```

**Example: Constrain to Specific Area**

```csharp
private Rectangle allowedDragArea = new Rectangle(50, 50, 600, 800);

private void RadialMenu_DragBegin(object sender, DragBeginEventArgs e)
{
    // Only allow drag if starting within allowed area
    if (!allowedDragArea.Contains(e.Position))
    {
        e.Handled = true;
        DisplayAlert("Restricted", "Cannot drag from this area", "OK");
    }
}
```

## Complete Dragging and Rotation Example

```csharp
public class MainPage : ContentPage
{
    private SfRadialMenu radialMenu;
    private Label positionLabel;

    public MainPage()
    {
        InitializeComponent();
        CreateDraggableMenu();
    }

    private void CreateDraggableMenu()
    {
        // Position label to show current location
        positionLabel = new Label
        {
            Text = "Drag the menu!",
            HorizontalOptions = LayoutOptions.Center,
            Margin = new Thickness(0, 20, 0, 0)
        };

        // Create radial menu
        radialMenu = new SfRadialMenu
        {
            IsDragEnabled = true,
            EnableRotation = true,
            AnimationDuration = 400,
            CenterButtonText = "Menu",
            CenterButtonFontSize = 14,
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center
        };

        // Add items
        radialMenu.Items = new RadialMenuItemsCollection
        {
            new SfRadialMenuItem { Text = "Cut", FontSize = 12 },
            new SfRadialMenuItem { Text = "Copy", FontSize = 12 },
            new SfRadialMenuItem { Text = "Paste", FontSize = 12 },
            new SfRadialMenuItem { Text = "Delete", FontSize = 12 }
        };

        // Wire up drag event
        radialMenu.DragBegin += OnDragBegin;

        // Layout
        Grid grid = new Grid
        {
            RowDefinitions =
            {
                new RowDefinition { Height = GridLength.Auto },
                new RowDefinition { Height = GridLength.Star }
            }
        };

        grid.Add(positionLabel, 0, 0);
        grid.Add(radialMenu, 0, 1);

        Content = grid;
    }

    private void OnDragBegin(object sender, DragBeginEventArgs e)
    {
        // Update label with position
        positionLabel.Text = $"Dragged from: X={e.Position.X:F0}, Y={e.Position.Y:F0}";
        
        // Could restrict based on conditions
        if (SomeCondition())
        {
            e.Handled = true;
        }
    }

    private bool SomeCondition()
    {
        // Your condition logic here
        return false;
    }
}
```

## Common Patterns

### Pattern 1: Floating Toolbar

```csharp
// Draggable toolbar that doesn't rotate
SfRadialMenu toolbar = new SfRadialMenu
{
    IsDragEnabled = true,
    EnableRotation = false, // Static appearance
    CenterButtonText = "Tools",
    AnimationDuration = 200 // Quick open/close
};
```

### Pattern 2: Fixed Menu with Rotation

```csharp
// Stationary menu with animated rotation
SfRadialMenu menu = new SfRadialMenu
{
    IsDragEnabled = false,
    EnableRotation = true,
    AnimationDuration = 600 // Smooth rotation
};
```

### Pattern 3: Conditional Dragging

```csharp
// Enable dragging only in edit mode
void SetEditMode(bool isEditMode)
{
    radialMenu.IsDragEnabled = isEditMode;
    radialMenu.CenterButtonText = isEditMode ? "Editing" : "View";
}
```

### Pattern 4: Save Position

```csharp
private Point savedPosition;

private void OnDragBegin(object sender, DragBeginEventArgs e)
{
    savedPosition = e.Position;
    Preferences.Set("MenuX", e.Position.X);
    Preferences.Set("MenuY", e.Position.Y);
}

private void RestoreMenuPosition()
{
    double x = Preferences.Get("MenuX", 0.0);
    double y = Preferences.Get("MenuY", 0.0);
    
    // Apply saved position
    AbsoluteLayout.SetLayoutBounds(radialMenu, 
        new Rectangle(x, y, 100, 100));
}
```

## Tips and Best Practices

1. **Test Dragging:** Always test on real devices; touch dragging feels different than mouse dragging

2. **Parent Constraints:** Ensure parent layout has enough space for dragging

3. **Performance:** Disable rotation on low-end devices if performance is an issue

4. **User Feedback:** Provide visual feedback when drag starts (subtle scale or opacity change)

5. **Persistence:** Save drag position to restore on app restart

6. **Accessibility:** Provide alternative access methods for users who can't drag

7. **Context Awareness:** Consider when dragging should be allowed vs. restricted

8. **Combine Features:** Use rotation and dragging together for rich interactions

9. **Animation Tuning:** Match AnimationDuration with EnableRotation for cohesive feel

10. **Handle Edge Cases:** Test behavior at screen edges and corners
