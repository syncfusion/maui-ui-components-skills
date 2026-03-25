# Advanced Features

## Table of Contents
- [Multi-Drawer Implementation](#multi-drawer-implementation)
- [Liquid Glass Effect](#liquid-glass-effect)
- [Complex Navigation Patterns](#complex-navigation-patterns)
- [Performance Optimization](#performance-optimization)

## Multi-Drawer Implementation

The Navigation Drawer supports opening drawers on multiple sides simultaneously using primary and secondary drawer settings.

### Overview

**Key Points:**
- Two drawers: Primary (`DrawerSettings`) and Secondary (`SecondaryDrawerSettings`)
- Each drawer can be on a different side (Left, Right, Top, Bottom)
- **Only one drawer can be open at a time**
- Each drawer has independent configuration

### Setting Up Multi-Drawer

**XAML:**

```xml
<navigationDrawer:SfNavigationDrawer x:Name="navigationDrawer">
    <!-- Primary Drawer (Left) -->
    <navigationDrawer:SfNavigationDrawer.DrawerSettings>
        <navigationDrawer:DrawerSettings DrawerWidth="250"
                                         Position="Left"
                                         Transition="SlideOnTop"
                                         ContentBackground="LightGray">
            <navigationDrawer:DrawerSettings.DrawerContentView>
                <VerticalStackLayout>
                    <Label Text="Primary Menu" FontSize="18"/>
                    <!-- Menu items -->
                </VerticalStackLayout>
            </navigationDrawer:DrawerSettings.DrawerContentView>
        </navigationDrawer:DrawerSettings>
    </navigationDrawer:SfNavigationDrawer.DrawerSettings>
    
    <!-- Secondary Drawer (Right) -->
    <navigationDrawer:SfNavigationDrawer.SecondaryDrawerSettings>
        <navigationDrawer:DrawerSettings x:Name="secondaryDrawer"
                                         DrawerWidth="200"
                                         Position="Right"
                                         Transition="SlideOnTop"
                                         ContentBackground="Blue">
            <navigationDrawer:DrawerSettings.DrawerContentView>
                <VerticalStackLayout>
                    <Label Text="Secondary Menu" FontSize="18"/>
                    <!-- Settings/Filter items -->
                </VerticalStackLayout>
            </navigationDrawer:DrawerSettings.DrawerContentView>
        </navigationDrawer:DrawerSettings>
    </navigationDrawer:SfNavigationDrawer.SecondaryDrawerSettings>
    
    <!-- Main Content -->
    <navigationDrawer:SfNavigationDrawer.ContentView>
        <Grid ColumnDefinitions="Auto,*,Auto">
            <ImageButton Grid.Column="0" 
                         Source="menu_left.png"
                         Clicked="OnLeftMenuClicked"/>
            <Label Grid.Column="1" 
                   Text="Content"
                   HorizontalOptions="Center"/>
            <ImageButton Grid.Column="2"
                         Source="menu_right.png"
                         Clicked="OnRightMenuClicked"/>
        </Grid>
    </navigationDrawer:SfNavigationDrawer.ContentView>
</navigationDrawer:SfNavigationDrawer>
```

**Code-Behind:**

```csharp
SfNavigationDrawer navigationDrawer = new SfNavigationDrawer();

// Primary drawer (left)
DrawerSettings primaryDrawer = new DrawerSettings
{
    DrawerWidth = 250,
    Position = Position.Left,
    Transition = Transition.SlideOnTop,
    ContentBackground = Colors.LightGray
};
navigationDrawer.DrawerSettings = primaryDrawer;

// Secondary drawer (right)
DrawerSettings secondaryDrawer = new DrawerSettings
{
    DrawerWidth = 200,
    Position = Position.Right,
    Transition = Transition.SlideOnTop,
    ContentBackground = Colors.Blue
};
navigationDrawer.SecondaryDrawerSettings = secondaryDrawer;

this.Content = navigationDrawer;
```

### Toggling Secondary Drawer

Use `ToggleSecondaryDrawer()` method:

```csharp
private void OnLeftMenuClicked(object sender, EventArgs e)
{
    // Toggle primary drawer
    navigationDrawer.ToggleDrawer();
}

private void OnRightMenuClicked(object sender, EventArgs e)
{
    // Toggle secondary drawer
    navigationDrawer.ToggleSecondaryDrawer();
}
```

### Opening Secondary Drawer Programmatically

```csharp
// Open secondary drawer
navigationDrawer.SecondaryDrawerSettings.IsOpen = true;

// Close secondary drawer
navigationDrawer.SecondaryDrawerSettings.IsOpen = false;

// Check if secondary drawer is open
bool isSecondaryOpen = navigationDrawer.SecondaryDrawerSettings.IsOpen;
```

### Multi-Drawer Use Cases

**Use Case 1: Navigation + Settings**

```csharp
// Left: Main navigation menu
primaryDrawer.Position = Position.Left;
primaryDrawer.DrawerWidth = 280;

// Right: Settings/filters panel
secondaryDrawer.Position = Position.Right;
secondaryDrawer.DrawerWidth = 250;
```

**Use Case 2: Dual Menus**

```csharp
// Left: User menu
primaryDrawer.Position = Position.Left;

// Right: Admin menu
secondaryDrawer.Position = Position.Right;
```

**Use Case 3: Top Notifications + Left Navigation**

```csharp
// Top: Notification panel
primaryDrawer.Position = Position.Top;
primaryDrawer.DrawerHeight = 300;

// Left: Navigation menu (secondary)
secondaryDrawer.Position = Position.Left;
secondaryDrawer.DrawerWidth = 250;
```

### Same Position Behavior

**⚠️ Important:** If both drawers are on the same position:

```csharp
primaryDrawer.Position = Position.Left;
secondaryDrawer.Position = Position.Left;  // Same side!
```

- **Swipe gesture** opens the primary drawer
- Both drawers still accessible via methods:
  - `ToggleDrawer()` → primary
  - `ToggleSecondaryDrawer()` → secondary

## Liquid Glass Effect

The liquid glass effect (glass morphism/acrylic) provides a frosted, translucent appearance that blends with the background.

### Requirements

- **.NET 10** or later
- **iOS 26+** or **macOS 26+**
- **SlideOnTop transition** (only supported mode)

### Enabling Liquid Glass

**XAML:**

```xml
<Grid>
    <!-- Background to make effect visible -->
    <Image Source="wallpaper.jpg" Aspect="AspectFill"/>
    
    <navigationDrawer:SfNavigationDrawer EnableLiquidGlassEffect="True">
        <navigationDrawer:SfNavigationDrawer.DrawerSettings>
            <navigationDrawer:DrawerSettings Position="Left"
                                             DrawerWidth="250"
                                             Transition="SlideOnTop">
            </navigationDrawer:DrawerSettings>
        </navigationDrawer:SfNavigationDrawer.DrawerSettings>
    </navigationDrawer:SfNavigationDrawer>
</Grid>
```

**C#:**

```csharp
SfNavigationDrawer navigationDrawer = new SfNavigationDrawer
{
    EnableLiquidGlassEffect = true
};
```

### Optimizing Liquid Glass Appearance

**1. Use Transparent Backgrounds**

```xml
<navigationDrawer:DrawerSettings ContentBackground="Transparent">
    <navigationDrawer:DrawerSettings.DrawerContentView>
        <VerticalStackLayout Background="Transparent">
            <!-- Content -->
        </VerticalStackLayout>
    </navigationDrawer:DrawerSettings.DrawerContentView>
</navigationDrawer:DrawerSettings>
```

**2. Transparent ContentView**

```xml
<navigationDrawer:SfNavigationDrawer.ContentView>
    <Grid Background="Transparent">
        <!-- Content -->
    </Grid>
</navigationDrawer:SfNavigationDrawer.ContentView>
```

**3. Rich Background Content**

```csharp
// Place visually interesting background
var backgroundImage = new Image
{
    Source = "gradient_wallpaper.jpg",
    Aspect = Aspect.AspectFill
};

var grid = new Grid();
grid.Children.Add(backgroundImage);
grid.Children.Add(navigationDrawer);
```

### Platform Availability

```csharp
// Check platform support before enabling
bool SupportsLiquidGlass()
{
#if IOS || MACCATALYST
    if (DeviceInfo.Version.Major >= 26)
    {
        return true;
    }
#endif
    return false;
}

if (SupportsLiquidGlass())
{
    navigationDrawer.EnableLiquidGlassEffect = true;
}
```

### Limitations

- **Only SlideOnTop transition** - Push and Reveal are not supported
- **iOS 26+ / macOS 26+ only** - Not available on earlier versions or other platforms
- **Performance impact** - May affect animation smoothness on lower-end devices

## Complex Navigation Patterns

### Pattern 1: Master-Detail with Drawer

```csharp
// Drawer for category selection
// ContentView shows detail based on selection

private void OnCategorySelected(object sender, SelectionChangedEventArgs e)
{
    if (e.CurrentSelection.FirstOrDefault() is Category category)
    {
        // Update detail view
        detailFrame.Content = new DetailView(category);
        
        // Close drawer
        navigationDrawer.ToggleDrawer();
    }
}
```

### Pattern 2: Tabbed Navigation Inside Drawer

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:tabView="clr-namespace:Syncfusion.Maui.TabView;assembly=Syncfusion.Maui.TabView"
             xmlns:navigationDrawer="clr-namespace:Syncfusion.Maui.NavigationDrawer;assembly=Syncfusion.Maui.NavigationDrawer"
             x:Class="TabViewGettingStarted.MainPage">
<navigationDrawer:DrawerSettings.DrawerContentView>
    <tabView:SfTabView>
        <tabView:SfTabItem Header="Primary">
            <!-- Primary menu -->
        </tabView:SfTabItem>
        <tabView:SfTabItem Header="Settings">
            <!-- Settings menu -->
        </tabView:SfTabItem>
    </tabView:SfTabView>
</navigationDrawer:DrawerSettings.DrawerContentView>
```

### Pattern 3: Hierarchical Navigation

```csharp
// Navigate deeper into drawer menu hierarchy
private void OnMenuItemClicked(MenuItem item)
{
    if (item.HasChildren)
    {
        // Show sub-menu
        ShowSubMenu(item.Children);
    }
    else
    {
        // Navigate to page
        NavigateToPage(item);
        navigationDrawer.ToggleDrawer();
    }
}
```

### Pattern 4: Contextual Drawer Content

```csharp
// Change drawer content based on app state
private void UpdateDrawerForState(AppState state)
{
    switch (state)
    {
        case AppState.LoggedOut:
            drawerContent.Content = new LoginMenuView();
            break;
        case AppState.LoggedIn:
            drawerContent.Content = new UserMenuView();
            break;
        case AppState.Admin:
            drawerContent.Content = new AdminMenuView();
            break;
    }
}
```

## Performance Optimization

### 1. Lazy Load Drawer Content

```csharp
private bool _drawerContentLoaded = false;

private async void OnDrawerOpened(object sender, EventArgs e)
{
    if (!_drawerContentLoaded)
    {
        await LoadDrawerContentAsync();
        _drawerContentLoaded = true;
    }
}

private async Task LoadDrawerContentAsync()
{
    // Load menu items from database/API
    var menuItems = await _dataService.GetMenuItemsAsync();
    menuCollectionView.ItemsSource = menuItems;
}
```

### 2. Virtualize Long Lists

```xml
<!-- Use CollectionView for virtualization -->
<CollectionView ItemsSource="{Binding MenuItems}"
                SelectionMode="Single">
    <!-- CollectionView automatically virtualizes -->
</CollectionView>
```

### 3. Reduce Animation Complexity

```csharp
// Faster duration for better performance
drawerSettings.Duration = 200;  // Instead of 300+

// Simple easing function
drawerSettings.AnimationEasing = Easing.Linear;
```

### 4. Minimize Drawer Content Complexity

```csharp
// ✓ Good - Simple, flat menu
<VerticalStackLayout>
    <Label Text="Home"/>
    <Label Text="Profile"/>
    <Label Text="Settings"/>
</VerticalStackLayout>

// ✗ Avoid - Complex nested layouts in drawer
<Grid>
    <StackLayout>
        <Frame>
            <Grid>...</Grid>
        </Frame>
    </StackLayout>
</Grid>
```

### 5. Pre-render Common Views

```csharp
// Create views early, reuse
private View _homeView;
private View _profileView;

public MainPage()
{
    InitializeComponent();
    
    // Pre-create views
    _homeView = new HomeView();
    _profileView = new ProfileView();
}

private void ShowView(string viewName)
{
    contentFrame.Content = viewName switch
    {
        "Home" => _homeView,      // Reuse
        "Profile" => _profileView, // Reuse
        _ => _homeView
    };
}
```

## Common Advanced Scenarios

### Scenario 1: E-Commerce App with Filters

```csharp
// Left: Category navigation
primaryDrawer.Position = Position.Left;
primaryDrawer.DrawerWidth = 280;

// Right: Product filters
secondaryDrawer.Position = Position.Right;
secondaryDrawer.DrawerWidth = 300;
```

### Scenario 2: Admin Dashboard

```csharp
// Top: Notifications
primaryDrawer.Position = Position.Top;
primaryDrawer.DrawerHeight = 350;

// Left: Main menu
secondaryDrawer.Position = Position.Left;
secondaryDrawer.DrawerWidth = 250;
```

### Scenario 3: Media Player

```csharp
// Bottom: Player controls
primaryDrawer.Position = Position.Bottom;
primaryDrawer.DrawerHeight = 120;
primaryDrawer.EnableSwipeGesture = true;

// Left: Playlist
secondaryDrawer.Position = Position.Left;
secondaryDrawer.DrawerWidth = 300;
```

## Troubleshooting Advanced Features

### Issue: Liquid glass effect not visible
**Solution:** Ensure .NET 10+, iOS/macOS 26+, SlideOnTop transition, and transparent backgrounds.

### Issue: Secondary drawer not opening
**Solution:** Use `ToggleSecondaryDrawer()` method, not `ToggleDrawer()`.

### Issue: Both drawers open simultaneously
**Solution:** This shouldn't happen; only one drawer can be open at a time. Check for logic errors.

### Issue: Performance issues with multi-drawer
**Solution:** Lazy load content, reduce animation duration, simplify layouts.

## Quick Reference

### Multi-Drawer

```csharp
// Primary drawer
navigationDrawer.ToggleDrawer();
navigationDrawer.DrawerSettings.IsOpen = true;

// Secondary drawer
navigationDrawer.ToggleSecondaryDrawer();
navigationDrawer.SecondaryDrawerSettings.IsOpen = true;
```

### Liquid Glass

```csharp
navigationDrawer.EnableLiquidGlassEffect = true;  // iOS 26+, macOS 26+, .NET 10+
```

### Requirements

- Multi-drawer: All .NET MAUI versions
- Liquid glass: .NET 10+, iOS 26+/macOS 26+, SlideOnTop only
