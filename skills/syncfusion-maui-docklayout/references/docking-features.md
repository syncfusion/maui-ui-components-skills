# Docking Features

## Table of Contents
- [Overview](#overview)
- [Dock Position Options](#dock-position-options)
- [Setting Dock Positions in XAML](#setting-dock-positions-in-xaml)
- [Setting Dock Positions in C#](#setting-dock-positions-in-c)
- [GetDock Method](#getdock-method)
- [SetDock Method](#setdock-method)
- [Docking Order and Layering](#docking-order-and-layering)
- [Advanced Docking Scenarios](#advanced-docking-scenarios)
- [Troubleshooting](#troubleshooting)

## Overview

The SfDockLayout control provides powerful docking capabilities that allow you to position child elements at specific edges of the container or in the remaining center space. This reference covers all docking features, methods, and best practices.

## Dock Position Options

The `Dock` enum provides the following values:

| Position | Description | Size Requirements |
|----------|-------------|-------------------|
| `Dock.Top` | Docks element to the top edge | Requires `HeightRequest` |
| `Dock.Bottom` | Docks element to the bottom edge | Requires `HeightRequest` |
| `Dock.Left` | Docks element to the left edge | Requires `WidthRequest` |
| `Dock.Right` | Docks element to the right edge | Requires `WidthRequest` |
| `Dock.None` | No docking, fills remaining space | Auto-sizes to fill available space |

**Default behavior:** If no dock position is specified, the element is treated as `Dock.None`.

## Setting Dock Positions in XAML

Use the `sf:SfDockLayout.Dock` attached property to specify the docking position.

### Syntax

```xaml
<sf:SfDockLayout>
    <View sf:SfDockLayout.Dock="Position" />
</sf:SfDockLayout>
```

### Example: All Dock Positions

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sf="clr-namespace:Syncfusion.Maui.Core;assembly=Syncfusion.Maui.Core"
             x:Class="MyApp.MainPage">

    <sf:SfDockLayout>
        <!-- Top header -->
        <BoxView Color="Red" 
                 HeightRequest="60" 
                 sf:SfDockLayout.Dock="Top" />
        
        <!-- Bottom footer -->
        <BoxView Color="Green" 
                 HeightRequest="60" 
                 sf:SfDockLayout.Dock="Bottom" />
        
        <!-- Left sidebar -->
        <BoxView Color="Blue" 
                 WidthRequest="80" 
                 sf:SfDockLayout.Dock="Left" />
        
        <!-- Right sidebar -->
        <BoxView Color="Orange" 
                 WidthRequest="80" 
                 sf:SfDockLayout.Dock="Right" />
        
        <!-- Center content (fills remaining space) -->
        <BoxView Color="LightGray" />
    </sf:SfDockLayout>
    
</ContentPage>
```

### Example: Complex Navigation Layout

```xaml
<sf:SfDockLayout>
    <!-- App bar at top -->
    <Grid HeightRequest="56" 
          BackgroundColor="#6200EE"
          sf:SfDockLayout.Dock="Top">
        <Label Text="My Application" 
               TextColor="White" 
               FontSize="20" 
               VerticalOptions="Center" 
               Margin="16,0" />
    </Grid>
    
    <!-- Navigation drawer at left -->
    <VerticalStackLayout WidthRequest="250" 
                         BackgroundColor="#F5F5F5"
                         sf:SfDockLayout.Dock="Left">
        <Label Text="Navigation" FontSize="18" Margin="16" />
        <Button Text="Home" />
        <Button Text="Settings" />
        <Button Text="About" />
    </VerticalStackLayout>
    
    <!-- Status bar at bottom -->
    <Label Text="Ready" 
           HeightRequest="30" 
           BackgroundColor="#E0E0E0"
           VerticalTextAlignment="Center"
           Margin="8,0"
           sf:SfDockLayout.Dock="Bottom" />
    
    <!-- Main content area -->
    <ScrollView>
        <Label Text="Main Content Area" 
               Margin="20" 
               FontSize="16" />
    </ScrollView>
</sf:SfDockLayout>
```

## Setting Dock Positions in C#

Use the `Add()` method overload that accepts a `Dock` parameter, or add the child first and then use the `SetDock()` method.

### Method 1: Add with Dock Parameter

```csharp
using Syncfusion.Maui.Core;

public class MainPage : ContentPage
{
    public MainPage()
    {
        var dockLayout = new SfDockLayout();
        
        // Add elements with dock positions
        dockLayout.Children.Add(
            new BoxView { Color = Colors.Red, HeightRequest = 60 }, 
            Dock.Top
        );
        
        dockLayout.Children.Add(
            new BoxView { Color = Colors.Green, HeightRequest = 60 }, 
            Dock.Bottom
        );
        
        dockLayout.Children.Add(
            new BoxView { Color = Colors.Blue, WidthRequest = 80 }, 
            Dock.Left
        );
        
        dockLayout.Children.Add(
            new BoxView { Color = Colors.Orange, WidthRequest = 80 }, 
            Dock.Right
        );
        
        // Last element fills remaining space
        dockLayout.Children.Add(
            new BoxView { Color = Colors.LightGray }
        );
        
        Content = dockLayout;
    }
}
```

### Method 2: Add Then SetDock

```csharp
var dockLayout = new SfDockLayout();

var topBar = new Label { Text = "Header", HeightRequest = 60 };
dockLayout.Children.Add(topBar);
SfDockLayout.SetDock(topBar, Dock.Top);

var leftPanel = new Label { Text = "Sidebar", WidthRequest = 200 };
dockLayout.Children.Add(leftPanel);
SfDockLayout.SetDock(leftPanel, Dock.Left);

var mainContent = new Label { Text = "Content" };
dockLayout.Children.Add(mainContent);
// No need to set dock for center element (defaults to None)

Content = dockLayout;
```

## GetDock Method

The `GetDock()` static method retrieves the current docking position of a child element.

### Signature

```csharp
public static Dock GetDock(BindableObject view)
```

### Parameters
- **view** (`BindableObject`): The child element to query

### Returns
- `Dock` enum value (Top, Bottom, Left, Right, or None)

### Example

```csharp
SfDockLayout dockLayout = new SfDockLayout();

var leftLabel = new Label 
{ 
    Text = "Left", 
    WidthRequest = 80, 
    Background = Color.FromArgb("#CA7842") 
};

dockLayout.Children.Add(leftLabel, Dock.Left);
Content = dockLayout;

// Get the current dock position
Dock currentPosition = SfDockLayout.GetDock(leftLabel);
Console.WriteLine($"Current position: {currentPosition}"); // Output: Left
```

### Use Case: Conditional Logic Based on Position

```csharp
private void OnElementTapped(object sender, EventArgs e)
{
    var element = sender as View;
    var currentDock = SfDockLayout.GetDock(element);
    
    if (currentDock == Dock.Left)
    {
        // Element is on the left, move it to the right
        SfDockLayout.SetDock(element, Dock.Right);
    }
    else if (currentDock == Dock.Right)
    {
        // Element is on the right, move it to the left
        SfDockLayout.SetDock(element, Dock.Left);
    }
}
```

## SetDock Method

The `SetDock()` static method assigns or changes the docking position of a child element at runtime.

### Signature

```csharp
public static void SetDock(BindableObject view, Dock position)
```

### Parameters
- **view** (`BindableObject`): The child element to reposition
- **position** (`Dock`): The target docking position

### Example: Basic Usage

```csharp
SfDockLayout dockLayout = new SfDockLayout();

var panel = new Label { Text = "Panel", WidthRequest = 100 };
dockLayout.Children.Add(panel);

// Initially dock to left
SfDockLayout.SetDock(panel, Dock.Left);

// Later, move to right (e.g., in a button click handler)
SfDockLayout.SetDock(panel, Dock.Right);
```

### Example: Dynamic Repositioning

```csharp
private SfDockLayout dockLayout;
private Label sidePanel;

public MainPage()
{
    dockLayout = new SfDockLayout();
    
    sidePanel = new Label 
    { 
        Text = "Sidebar", 
        WidthRequest = 200,
        BackgroundColor = Colors.LightBlue
    };
    
    dockLayout.Children.Add(sidePanel, Dock.Left);
    dockLayout.Children.Add(new Label { Text = "Main Content" });
    
    // Add toggle button
    var toggleButton = new Button 
    { 
        Text = "Toggle Sidebar Position",
        HeightRequest = 50
    };
    toggleButton.Clicked += ToggleSidebarPosition;
    dockLayout.Children.Add(toggleButton, Dock.Top);
    
    Content = dockLayout;
}

private void ToggleSidebarPosition(object sender, EventArgs e)
{
    var currentDock = SfDockLayout.GetDock(sidePanel);
    
    if (currentDock == Dock.Left)
    {
        SfDockLayout.SetDock(sidePanel, Dock.Right);
    }
    else
    {
        SfDockLayout.SetDock(sidePanel, Dock.Left);
    }
}
```

## Docking Order and Layering

The order in which child elements are added to the DockLayout determines how space is allocated.

### Rule: First Added, First Positioned

Elements are docked in the sequence they're added. Earlier elements take priority in space allocation.

### Example: Order Matters

```csharp
// Scenario 1: Top added first
dockLayout.Children.Add(topBar, Dock.Top);       // Full width
dockLayout.Children.Add(leftPanel, Dock.Left);   // Remaining height after top

// Scenario 2: Left added first
dockLayout.Children.Add(leftPanel, Dock.Left);   // Full height
dockLayout.Children.Add(topBar, Dock.Top);       // Remaining width after left
```

**Visual difference:**
- **Scenario 1**: Top bar spans the entire width, left panel is shorter
- **Scenario 2**: Left panel spans the entire height, top bar is narrower

### Best Practice

For typical application layouts (header, sidebar, footer, content):
1. Add **Top** elements first (headers, toolbars)
2. Add **Bottom** elements second (footers, status bars)
3. Add **Left/Right** elements third (sidebars, panels)
4. Add **center** content last

This ensures headers and footers span the full width, and sidebars fit between them.

## Advanced Docking Scenarios

### Multiple Elements on Same Edge

You can dock multiple elements to the same edge. They stack in the order added.

```csharp
// Two top bars
dockLayout.Children.Add(
    new Label { Text = "Menu Bar", HeightRequest = 40 }, 
    Dock.Top
);
dockLayout.Children.Add(
    new Label { Text = "Toolbar", HeightRequest = 50 }, 
    Dock.Top
);
// Result: Menu bar at very top, toolbar below it
```

### Nested DockLayouts

Create complex layouts by nesting DockLayouts.

```csharp
var outerDock = new SfDockLayout();

// Top header
outerDock.Children.Add(
    new Label { Text = "Header", HeightRequest = 60 }, 
    Dock.Top
);

// Nested inner layout for left panel and content
var innerDock = new SfDockLayout();
innerDock.Children.Add(
    new Label { Text = "Sidebar", WidthRequest = 200 }, 
    Dock.Left
);
innerDock.Children.Add(
    new Label { Text = "Main Content" }
);

outerDock.Children.Add(innerDock); // No dock = fills remaining space

Content = outerDock;
```

### Responsive Docking

Change dock positions based on screen size or orientation.

```csharp
protected override void OnSizeAllocated(double width, double height)
{
    base.OnSizeAllocated(width, height);
    
    if (width < 600) // Mobile portrait
    {
        SfDockLayout.SetDock(sidePanel, Dock.Top);
        sidePanel.WidthRequest = -1;
        sidePanel.HeightRequest = 150;
    }
    else // Tablet/Desktop
    {
        SfDockLayout.SetDock(sidePanel, Dock.Left);
        sidePanel.WidthRequest = 250;
        sidePanel.HeightRequest = -1;
    }
}
```

## Troubleshooting

### Element Not Appearing
- **Cause**: Missing size constraint (WidthRequest for Left/Right, HeightRequest for Top/Bottom)
- **Solution**: Add explicit size: `WidthRequest="100"` or `HeightRequest="60"`

### Element Not Filling Space
- **Cause**: Element is not the last child, or `ShouldExpandLastChild` is false
- **Solution**: Move element to be last, or set `ShouldExpandLastChild="True"`

### Elements Overlapping
- **Cause**: Incorrect docking order or missing dock positions
- **Solution**: Verify dock positions are set correctly and adjust the order of adding children

### SetDock Not Working
- **Cause**: Element is not in the DockLayout's Children collection
- **Solution**: Add the element to the collection before calling SetDock

### GetDock Returns None Unexpectedly
- **Cause**: Element was added without specifying a dock position
- **Solution**: This is expected behavior; explicitly set the dock position when adding the element
