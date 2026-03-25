# Visual States in .NET MAUI Button (SfButton)

Visual states allow you to customize the button's appearance based on user interaction and state changes. The SfButton supports five built-in visual states that provide rich interactive feedback.

## Table of Contents
- [Overview](#overview)
- [Available Visual States](#available-visual-states)
- [Implementing Visual States](#implementing-visual-states)
- [Toggle/Checkable Buttons](#togglecheckable-buttons)
- [State-Based Property Changes](#state-based-property-changes)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)

## Overview

Visual states enable dynamic styling of buttons based on user interaction. Instead of manually handling state changes in code, you declare how properties should change for each state directly in XAML or C#.

**Benefits:**
- Automatic state management by the framework
- Declarative UI without event handlers
- Smooth visual feedback for user actions
- Support for all button properties (Background, TextColor, Stroke, etc.)

## Available Visual States

The SfButton supports five visual states:

| State | When Active | Common Use |
|-------|-------------|------------|
| **Normal** | Default state when button is not interacted with | Base appearance |
| **Hovered** | Mouse pointer hovers over button (desktop/web) | Hover feedback |
| **Pressed** | Button is being touched/clicked | Active feedback |
| **Checked** | Toggle button is in checked state (requires `IsCheckable="True"`) | Toggle selection |
| **Disabled** | Button is disabled (`IsEnabled="False"`) | Inactive state |

> **Note:** `Hovered` state is primarily for desktop platforms. Mobile platforms use `Pressed` for touch feedback.

## Implementing Visual States

### Basic XAML Implementation

```xml
<buttons:SfButton Text="Interactive Button"
                  HeightRequest="50"
                  WidthRequest="200">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="Background" Value="#6200EE" />
                    <Setter Property="TextColor" Value="White" />
                </VisualState.Setters>
            </VisualState>
            
            <VisualState x:Name="Hovered">
                <VisualState.Setters>
                    <Setter Property="Background" Value="#7C4DFF" />
                    <Setter Property="TextColor" Value="White" />
                </VisualState.Setters>
            </VisualState>
            
            <VisualState x:Name="Pressed">
                <VisualState.Setters>
                    <Setter Property="Background" Value="#5000CA" />
                    <Setter Property="TextColor" Value="White" />
                </VisualState.Setters>
            </VisualState>
            
            <VisualState x:Name="Disabled">
                <VisualState.Setters>
                    <Setter Property="Background" Value="#E0E0E0" />
                    <Setter Property="TextColor" Value="#9E9E9E" />
                </VisualState.Setters>
            </VisualState>
            
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</buttons:SfButton>
```

### C# Implementation

```csharp
var button = new SfButton
{
    Text = "Interactive Button",
    HeightRequest = 50,
    WidthRequest = 200
};

var visualStateGroupList = new VisualStateGroupList();
var commonStateGroup = new VisualStateGroup();

// Normal state
var normalState = new VisualState { Name = "Normal" };
normalState.Setters.Add(new Setter 
{ 
    Property = SfButton.BackgroundProperty, 
    Value = Color.FromArgb("#6200EE") 
});
normalState.Setters.Add(new Setter 
{ 
    Property = SfButton.TextColorProperty, 
    Value = Colors.White 
});

// Hovered state
var hoveredState = new VisualState { Name = "Hovered" };
hoveredState.Setters.Add(new Setter 
{ 
    Property = SfButton.BackgroundProperty, 
    Value = Color.FromArgb("#7C4DFF") 
});

// Pressed state
var pressedState = new VisualState { Name = "Pressed" };
pressedState.Setters.Add(new Setter 
{ 
    Property = SfButton.BackgroundProperty, 
    Value = Color.FromArgb("#5000CA") 
});

// Disabled state
var disabledState = new VisualState { Name = "Disabled" };
disabledState.Setters.Add(new Setter 
{ 
    Property = SfButton.BackgroundProperty, 
    Value = Color.FromArgb("#E0E0E0") 
});
disabledState.Setters.Add(new Setter 
{ 
    Property = SfButton.TextColorProperty, 
    Value = Color.FromArgb("#9E9E9E") 
});

commonStateGroup.States.Add(normalState);
commonStateGroup.States.Add(hoveredState);
commonStateGroup.States.Add(pressedState);
commonStateGroup.States.Add(disabledState);

visualStateGroupList.Add(commonStateGroup);
VisualStateManager.SetVisualStateGroups(button, visualStateGroupList);
```

## Toggle/Checkable Buttons

To use the `Checked` state, enable the `IsCheckable` property:

### XAML Implementation

```xml
<buttons:SfButton Text="Toggle Button"
                  IsCheckable="True"
                  HeightRequest="50"
                  WidthRequest="200">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="Background" Value="#E0E0E0" />
                    <Setter Property="TextColor" Value="Black" />
                    <Setter Property="Stroke" Value="#BDBDBD" />
                </VisualState.Setters>
            </VisualState>
            
            <VisualState x:Name="Checked">
                <VisualState.Setters>
                    <Setter Property="Background" Value="#6200EE" />
                    <Setter Property="TextColor" Value="White" />
                    <Setter Property="Stroke" Value="#6200EE" />
                </VisualState.Setters>
            </VisualState>
            
            <VisualState x:Name="Hovered">
                <VisualState.Setters>
                    <Setter Property="Background" Value="#EEEEEE" />
                </VisualState.Setters>
            </VisualState>
            
            <VisualState x:Name="Pressed">
                <VisualState.Setters>
                    <Setter Property="Background" Value="#BDBDBD" />
                </VisualState.Setters>
            </VisualState>
            
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</buttons:SfButton>
```

### Programmatic Toggle Control

```csharp
// Check if button is checked
if (myButton.IsChecked)
{
    // Handle checked state
}

// Programmatically toggle
myButton.IsChecked = !myButton.IsChecked;

// Listen to state changes
myButton.Clicked += (s, e) =>
{
    var button = s as SfButton;
    if (button.IsCheckable && button.IsChecked)
    {
        DisplayAlert("Checked", "Button is now checked", "OK");
    }
};
```

## State-Based Property Changes

You can customize any button property in visual states:

### Changing Multiple Properties

```xml
<VisualState x:Name="Hovered">
    <VisualState.Setters>
        <Setter Property="Background" Value="#7C4DFF" />
        <Setter Property="TextColor" Value="White" />
        <Setter Property="FontSize" Value="16" />
        <Setter Property="Stroke" Value="White" />
        <Setter Property="StrokeThickness" Value="2" />
        <Setter Property="CornerRadius" Value="12" />
    </VisualState.Setters>
</VisualState>
```

### Customizing Icon Appearance

```xml
<buttons:SfButton Text="Favorite"
                  ShowIcon="True"
                  ImageSource="star_outline.png"
                  IsCheckable="True">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="ImageSource" Value="star_outline.png" />
                    <Setter Property="TextColor" Value="Gray" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Checked">
                <VisualState.Setters>
                    <Setter Property="ImageSource" Value="star_filled.png" />
                    <Setter Property="TextColor" Value="Gold" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</buttons:SfButton>
```

## Complete Examples

### Example 1: Material Design Button

```xml
<buttons:SfButton Text="PRIMARY ACTION"
                  CornerRadius="4"
                  HeightRequest="40"
                  WidthRequest="200"
                  FontSize="14"
                  FontAttributes="Bold"
                  TextTransform="Uppercase">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="Background" Value="#6200EE" />
                    <Setter Property="TextColor" Value="White" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Hovered">
                <VisualState.Setters>
                    <Setter Property="Background" Value="#7C4DFF" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Pressed">
                <VisualState.Setters>
                    <Setter Property="Background" Value="#5000CA" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Disabled">
                <VisualState.Setters>
                    <Setter Property="Background" Value="#E0E0E0" />
                    <Setter Property="TextColor" Value="#9E9E9E" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</buttons:SfButton>
```

### Example 2: Outline Button with States

```xml
<buttons:SfButton Text="OUTLINE"
                  Background="Transparent"
                  StrokeThickness="2"
                  CornerRadius="8"
                  HeightRequest="44"
                  WidthRequest="150">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="Stroke" Value="#6200EE" />
                    <Setter Property="TextColor" Value="#6200EE" />
                    <Setter Property="Background" Value="Transparent" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Hovered">
                <VisualState.Setters>
                    <Setter Property="Background" Value="#F3E5F5" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Pressed">
                <VisualState.Setters>
                    <Setter Property="Background" Value="#E1BEE7" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Disabled">
                <VisualState.Setters>
                    <Setter Property="Stroke" Value="#BDBDBD" />
                    <Setter Property="TextColor" Value="#BDBDBD" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</buttons:SfButton>
```

### Example 3: Toggle Button for Favorites

```xml
<buttons:SfButton Text="Favorite"
                  ShowIcon="True"
                  ImageSource="star_outline.png"
                  IsCheckable="True"
                  CornerRadius="8"
                  HeightRequest="40">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="Background" Value="Transparent" />
                    <Setter Property="TextColor" Value="#757575" />
                    <Setter Property="ImageSource" Value="star_outline.png" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Checked">
                <VisualState.Setters>
                    <Setter Property="Background" Value="#FFF3E0" />
                    <Setter Property="TextColor" Value="#FF6F00" />
                    <Setter Property="ImageSource" Value="star_filled.png" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</buttons:SfButton>
```

### Example 4: Success Button with States

```xml
<buttons:SfButton Text="Confirm"
                  CornerRadius="8"
                  HeightRequest="44"
                  WidthRequest="150">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="Background" Value="#4CAF50" />
                    <Setter Property="TextColor" Value="White" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Hovered">
                <VisualState.Setters>
                    <Setter Property="Background" Value="#66BB6A" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Pressed">
                <VisualState.Setters>
                    <Setter Property="Background" Value="#388E3C" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Disabled">
                <VisualState.Setters>
                    <Setter Property="Background" Value="#C8E6C9" />
                    <Setter Property="TextColor" Value="#81C784" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</buttons:SfButton>
```

### Example 5: Complete Multi-State Button

```xml
<buttons:SfButton Text="Interactive"
                  ShowIcon="True"
                  ImageSource="icon.png"
                  IsCheckable="True"
                  CornerRadius="8"
                  StrokeThickness="1"
                  HeightRequest="44"
                  WidthRequest="180">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="Background" Value="White" />
                    <Setter Property="TextColor" Value="Black" />
                    <Setter Property="Stroke" Value="#E0E0E0" />
                    <Setter Property="ImageSource" Value="icon_normal.png" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Hovered">
                <VisualState.Setters>
                    <Setter Property="Background" Value="#F5F5F5" />
                    <Setter Property="Stroke" Value="#BDBDBD" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Pressed">
                <VisualState.Setters>
                    <Setter Property="Background" Value="#EEEEEE" />
                    <Setter Property="Stroke" Value="#9E9E9E" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Checked">
                <VisualState.Setters>
                    <Setter Property="Background" Value="#6200EE" />
                    <Setter Property="TextColor" Value="White" />
                    <Setter Property="Stroke" Value="#6200EE" />
                    <Setter Property="ImageSource" Value="icon_checked.png" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Disabled">
                <VisualState.Setters>
                    <Setter Property="Background" Value="#FAFAFA" />
                    <Setter Property="TextColor" Value="#BDBDBD" />
                    <Setter Property="Stroke" Value="#EEEEEE" />
                    <Setter Property="ImageSource" Value="icon_disabled.png" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</buttons:SfButton>
```

## Best Practices

### 1. Always Define Normal State
Provide a fallback appearance when no other state is active:
```xml
<VisualState x:Name="Normal">
    <VisualState.Setters>
        <Setter Property="Background" Value="#6200EE" />
        <Setter Property="TextColor" Value="White" />
    </VisualState.Setters>
</VisualState>
```

### 2. Provide Clear Visual Feedback
Make state changes obvious to users:
- **Hovered:** Slightly lighter/darker shade (10-15% difference)
- **Pressed:** More pronounced change (20-30% darker)
- **Disabled:** Muted colors (grays with low opacity)

### 3. Maintain Accessibility
Ensure sufficient color contrast in all states:
- Normal and Checked: 4.5:1 contrast ratio minimum
- Disabled: Visually distinct but not alarming

### 4. Use IsCheckable for Toggles
Only enable `IsCheckable="True"` when button represents a toggle state:
```xml
<buttons:SfButton Text="Subscribe"
                  IsCheckable="True" />
```

### 5. Test on Multiple Platforms
Visual state behavior may vary:
- Desktop: Hovered state is prominent
- Mobile: Focus on Pressed state
- All: Ensure Disabled state is clear

### 6. Keep State Logic Simple
Avoid complex nested state groups. Stick to the `CommonStates` group.

### 7. Consistent Color Palette
Use your app's color scheme across all states for brand consistency.

## Common Patterns

### Success/Danger States
```xml
<!-- Success Button -->
<VisualState x:Name="Normal">
    <VisualState.Setters>
        <Setter Property="Background" Value="#4CAF50" />
    </VisualState.Setters>
</VisualState>

<!-- Danger Button -->
<VisualState x:Name="Normal">
    <VisualState.Setters>
        <Setter Property="Background" Value="#F44336" />
    </VisualState.Setters>
</VisualState>
```

### Subtle Hover Effects
```xml
<VisualState x:Name="Hovered">
    <VisualState.Setters>
        <Setter Property="Opacity" Value="0.8" />
    </VisualState.Setters>
</VisualState>
```

### Loading State (Custom)
While not built-in, you can simulate a loading state:
```csharp
myButton.IsEnabled = false;
myButton.Text = "Loading...";
// Perform async operation
myButton.IsEnabled = true;
myButton.Text = "Submit";
```

## Quick Reference

| State | Property | Trigger |
|-------|----------|---------|
| Normal | Default appearance | No interaction |
| Hovered | Hover feedback | Mouse over (desktop) |
| Pressed | Touch feedback | Tap/click active |
| Checked | Selected state | Toggle checked (requires `IsCheckable`) |
| Disabled | Inactive appearance | `IsEnabled="False"` |

**Common Properties to Customize:**
- `Background` — Background color
- `TextColor` — Text color
- `Stroke` — Border color
- `StrokeThickness` — Border width
- `ImageSource` — Icon image
- `FontSize` — Text size
- `CornerRadius` — Border radius
- `Opacity` — Transparency
