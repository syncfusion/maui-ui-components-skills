# Visual State Manager in .NET MAUI Switch

## Table of Contents
- [Overview](#overview)
- [Available Visual States](#available-visual-states)
- [Basic States](#basic-states)
- [Hovered States](#hovered-states)
- [Pressed States](#pressed-states)
- [Disabled States](#disabled-states)
- [Implementing VSM in XAML](#implementing-vsm-in-xaml)
- [Implementing VSM in C#](#implementing-vsm-in-c)
- [Complete VSM Example](#complete-vsm-example)
- [Best Practices](#best-practices)

## Overview

The Visual State Manager (VSM) in .NET MAUI allows you to change the appearance of the Switch control based on its current visual state. This provides fine-grained control over how the switch looks during user interactions like hovering, pressing, and when disabled.

**Why Use Visual State Manager:**
- Provides visual feedback for user interactions
- Creates polished, responsive UI experiences
- Supports different styling for each interaction state
- Enables advanced customization beyond basic On/Off states

**Key Concept:** Each visual state can have its own `SwitchSettings` configuration, allowing you to customize colors, sizes, borders, and icons for each state.

## Available Visual States

The SfSwitch control supports 12 distinct visual states:

### Basic States
- **On** - Switch is in the On position
- **Off** - Switch is in the Off position  
- **Indeterminate** - Switch is in an indeterminate state

### Hovered States (Mouse/Pointer Over)
- **OnHovered** - Mouse hovering over switch in On state
- **OffHovered** - Mouse hovering over switch in Off state
- **IndeterminateHovered** - Mouse hovering over switch in Indeterminate state

### Pressed States (Active Touch/Click)
- **OnPressed** - Switch being pressed while in On state
- **OffPressed** - Switch being pressed while in Off state
- **IndeterminatePressed** - Switch being pressed while in Indeterminate state

### Disabled States (Not Interactive)
- **OnDisabled** - Switch in On state but disabled
- **OffDisabled** - Switch in Off state but disabled
- **IndeterminateDisabled** - Switch in Indeterminate state but disabled

## Basic States

The fundamental states that represent the switch's value.

### On State

```xml
<VisualState x:Name="On">
    <VisualState.Setters>
        <Setter Property="SwitchSettings">
            <Setter.Value>
                <buttons:SwitchSettings
                    ThumbBackground="#F57B31"
                    ThumbStroke="#F78F50"
                    TrackBackground="#F7D40D"
                    TrackStroke="#DABA04"/>
            </Setter.Value>
        </Setter>
    </VisualState.Setters>
</VisualState>
```

### Off State

```xml
<VisualState x:Name="Off">
    <VisualState.Setters>
        <Setter Property="SwitchSettings">
            <Setter.Value>
                <buttons:SwitchSettings
                    ThumbBackground="#F0F5F8"
                    ThumbStroke="#C7C9C9"
                    TrackBackground="#4FCFF7"
                    TrackStroke="#359EBF"/>
            </Setter.Value>
        </Setter>
    </VisualState.Setters>
</VisualState>
```

### Indeterminate State

```xml
<VisualState x:Name="Indeterminate">
    <VisualState.Setters>
        <Setter Property="SwitchSettings">
            <Setter.Value>
                <buttons:SwitchSettings
                    ThumbBackground="#9ACB0D"
                    ThumbStroke="#9ACB0D"
                    TrackBackground="#DEF991"
                    TrackStroke="#9ACB0D"/>
            </Setter.Value>
        </Setter>
    </VisualState.Setters>
</VisualState>
```

## Hovered States

Provide visual feedback when the user hovers over the switch with a mouse or pointer.

**Use Case:** Desktop and web applications where hover feedback improves UX.

### OnHovered State

```xml
<VisualState x:Name="OnHovered">
    <VisualState.Setters>
        <Setter Property="SwitchSettings">
            <Setter.Value>
                <buttons:SwitchSettings
                    ThumbBackground="#F57B31"
                    ThumbStroke="#E7600F"
                    TrackBackground="#F7D40D"
                    TrackStroke="#DABA04"/>
            </Setter.Value>
        </Setter>
    </VisualState.Setters>
</VisualState>
```

**Note:** The ThumbStroke is darker (#E7600F vs #F78F50) to show hover feedback.

### OffHovered State

```xml
<VisualState x:Name="OffHovered">
    <VisualState.Setters>
        <Setter Property="SwitchSettings">
            <Setter.Value>
                <buttons:SwitchSettings
                    ThumbBackground="#FFFFFF"
                    ThumbStroke="#959595"
                    TrackBackground="#72D4F3"
                    TrackStroke="#359EBF"/>
            </Setter.Value>
        </Setter>
    </VisualState.Setters>
</VisualState>
```

### IndeterminateHovered State

```xml
<VisualState x:Name="IndeterminateHovered">
    <VisualState.Setters>
        <Setter Property="SwitchSettings">
            <Setter.Value>
                <buttons:SwitchSettings
                    ThumbBackground="#9ACB0D"
                    ThumbStroke="#7FA00A"
                    TrackBackground="#DEF991"
                    TrackStroke="#9ACB0D"/>
            </Setter.Value>
        </Setter>
    </VisualState.Setters>
</VisualState>
```

## Pressed States

Provide visual feedback when the user actively presses or clicks the switch.

**Common Pattern:** Increase thumb size to show press feedback.

### OnPressed State

```xml
<VisualState x:Name="OnPressed">
    <VisualState.Setters>
        <Setter Property="SwitchSettings">
            <Setter.Value>
                <buttons:SwitchSettings
                    ThumbBackground="#F57B31"
                    ThumbStroke="#E7600F"
                    TrackBackground="#F7D40D"
                    TrackStroke="#DABA04"
                    ThumbHeightRequest="48"
                    ThumbWidthRequest="48"
                    ThumbCornerRadius="24"/>
            </Setter.Value>
        </Setter>
    </VisualState.Setters>
</VisualState>
```

**Note:** Thumb size increased from 35 to 48 to show press feedback.

### OffPressed State

```xml
<VisualState x:Name="OffPressed">
    <VisualState.Setters>
        <Setter Property="SwitchSettings">
            <Setter.Value>
                <buttons:SwitchSettings
                    ThumbBackground="#FFFFFF"
                    ThumbStroke="#959595"
                    TrackBackground="#72D4F3"
                    TrackStroke="#359EBF"
                    ThumbHeightRequest="48"
                    ThumbWidthRequest="48"
                    ThumbCornerRadius="24"/>
            </Setter.Value>
        </Setter>
    </VisualState.Setters>
</VisualState>
```

### IndeterminatePressed State

```xml
<VisualState x:Name="IndeterminatePressed">
    <VisualState.Setters>
        <Setter Property="SwitchSettings">
            <Setter.Value>
                <buttons:SwitchSettings
                    ThumbBackground="#9ACB0D"
                    ThumbStroke="#7FA00A"
                    TrackBackground="#DEF991"
                    TrackStroke="#9ACB0D"
                    ThumbHeightRequest="48"
                    ThumbWidthRequest="48"
                    ThumbCornerRadius="24"/>
            </Setter.Value>
        </Setter>
    </VisualState.Setters>
</VisualState>
```

## Disabled States

Show that the switch is non-interactive while maintaining state visibility.

**Common Pattern:** Use muted colors and reduced opacity.

### OnDisabled State

```xml
<VisualState x:Name="OnDisabled">
    <VisualState.Setters>
        <Setter Property="SwitchSettings">
            <Setter.Value>
                <buttons:SwitchSettings
                    ThumbBackground="#B0AFB2"
                    ThumbStroke="#B0AFB2"
                    TrackBackground="#FEF7FF"
                    TrackStroke="#B0AFB2"/>
            </Setter.Value>
        </Setter>
    </VisualState.Setters>
</VisualState>
```

### OffDisabled State

```xml
<VisualState x:Name="OffDisabled">
    <VisualState.Setters>
        <Setter Property="SwitchSettings">
            <Setter.Value>
                <buttons:SwitchSettings
                    ThumbBackground="#B0AFB2"
                    ThumbStroke="#B0AFB2"
                    TrackBackground="#FEF7FF"
                    TrackStroke="#B0AFB2"/>
            </Setter.Value>
        </Setter>
    </VisualState.Setters>
</VisualState>
```

### IndeterminateDisabled State

```xml
<VisualState x:Name="IndeterminateDisabled">
    <VisualState.Setters>
        <Setter Property="SwitchSettings">
            <Setter.Value>
                <buttons:SwitchSettings
                    ThumbBackground="#B0AFB2"
                    ThumbStroke="#B0AFB2"
                    TrackBackground="#FEF7FF"
                    TrackStroke="#B0AFB2"/>
            </Setter.Value>
        </Setter>
    </VisualState.Setters>
</VisualState>
```

## Implementing VSM in XAML

### Basic VSM Structure

```xml
<buttons:SfSwitch IsEnabled="True" IsOn="True">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <!-- Define visual states here -->
            <VisualState x:Name="On">
                <!-- On state styling -->
            </VisualState>
            <VisualState x:Name="Off">
                <!-- Off state styling -->
            </VisualState>
            <!-- More states... -->
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</buttons:SfSwitch>
```

### Minimal VSM Example

```xml
<buttons:SfSwitch IsOn="True">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="On">
                <VisualState.Setters>
                    <Setter Property="SwitchSettings">
                        <Setter.Value>
                            <buttons:SwitchSettings
                                ThumbBackground="#4CAF50"
                                TrackBackground="#C8E6C9"/>
                        </Setter.Value>
                    </Setter>
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Off">
                <VisualState.Setters>
                    <Setter Property="SwitchSettings">
                        <Setter.Value>
                            <buttons:SwitchSettings
                                ThumbBackground="#E0E0E0"
                                TrackBackground="#F5F5F5"/>
                        </Setter.Value>
                    </Setter>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</buttons:SfSwitch>
```

## Implementing VSM in C#

### Basic C# Structure

```csharp
SfSwitch sfSwitch = new SfSwitch();

// Create settings for each state
SwitchSettings onStyle = new SwitchSettings { /* properties */ };
SwitchSettings offStyle = new SwitchSettings { /* properties */ };

// Create visual state group
VisualStateGroupList visualStateGroupList = new VisualStateGroupList();
VisualStateGroup commonStateGroup = new VisualStateGroup();

// Create and configure visual states
VisualState onState = new VisualState { Name = "On" };
onState.Setters.Add(new Setter 
{ 
    Property = SfSwitch.SwitchSettingsProperty, 
    Value = onStyle 
});

VisualState offState = new VisualState { Name = "Off" };
offState.Setters.Add(new Setter 
{ 
    Property = SfSwitch.SwitchSettingsProperty, 
    Value = offStyle 
});

// Add states to group
commonStateGroup.States.Add(onState);
commonStateGroup.States.Add(offState);

// Apply to switch
visualStateGroupList.Add(commonStateGroup);
VisualStateManager.SetVisualStateGroups(sfSwitch, visualStateGroupList);
```

### Complete C# Example with All States

```csharp
SfSwitch sfSwitch = new SfSwitch();

// Define settings for each state
SwitchSettings onStyle = new SwitchSettings
{
    TrackWidthRequest = 90,
    TrackHeightRequest = 50,
    ThumbWidthRequest = 35,
    ThumbHeightRequest = 35,
    TrackStrokeThickness = 1,
    ThumbStrokeThickness = 1,
    TrackCornerRadius = 25,
    ThumbCornerRadius = 20,
    TrackBackground = new SolidColorBrush(Color.FromRgba("#F7D40D")),
    ThumbBackground = new SolidColorBrush(Color.FromRgba("#F57B31")),
    TrackStroke = Color.FromRgba("#DABA04"),
    ThumbStroke = Color.FromRgba("#F78F50")
};

SwitchSettings onHoveredStyle = new SwitchSettings
{
    TrackWidthRequest = 90,
    TrackHeightRequest = 50,
    ThumbWidthRequest = 35,
    ThumbHeightRequest = 35,
    TrackStrokeThickness = 1,
    ThumbStrokeThickness = 1,
    TrackCornerRadius = 25,
    ThumbCornerRadius = 20,
    TrackBackground = new SolidColorBrush(Color.FromRgba("#F7D40D")),
    ThumbBackground = new SolidColorBrush(Color.FromRgba("#F57B31")),
    TrackStroke = Color.FromRgba("#DABA04"),
    ThumbStroke = Color.FromRgba("#E7600F")  // Darker on hover
};

SwitchSettings onPressedStyle = new SwitchSettings
{
    TrackWidthRequest = 90,
    TrackHeightRequest = 50,
    ThumbWidthRequest = 48,  // Larger when pressed
    ThumbHeightRequest = 48,
    TrackStrokeThickness = 1,
    ThumbStrokeThickness = 1,
    TrackCornerRadius = 25,
    ThumbCornerRadius = 24,
    TrackBackground = new SolidColorBrush(Color.FromRgba("#F7D40D")),
    ThumbBackground = new SolidColorBrush(Color.FromRgba("#F57B31")),
    TrackStroke = Color.FromRgba("#DABA04"),
    ThumbStroke = Color.FromRgba("#E7600F")
};

SwitchSettings onDisabledStyle = new SwitchSettings
{
    TrackWidthRequest = 90,
    TrackHeightRequest = 50,
    ThumbWidthRequest = 35,
    ThumbHeightRequest = 35,
    TrackStrokeThickness = 1,
    ThumbStrokeThickness = 1,
    TrackCornerRadius = 25,
    ThumbCornerRadius = 20,
    TrackBackground = new SolidColorBrush(Color.FromRgba("#FEF7FF")),
    ThumbBackground = new SolidColorBrush(Color.FromRgba("#B0AFB2")),
    TrackStroke = Color.FromRgba("#B0AFB2"),
    ThumbStroke = Color.FromRgba("#B0AFB2")
};

// Create similar settings for Off states...
SwitchSettings offStyle = new SwitchSettings { /* ... */ };
SwitchSettings offHoveredStyle = new SwitchSettings { /* ... */ };
SwitchSettings offPressedStyle = new SwitchSettings { /* ... */ };
SwitchSettings offDisabledStyle = new SwitchSettings { /* ... */ };

// Create visual state group
VisualStateGroupList visualStateGroupList = new VisualStateGroupList();
VisualStateGroup commonStateGroup = new VisualStateGroup();

// Create visual states
VisualState onState = new VisualState { Name = "On" };
onState.Setters.Add(new Setter { Property = SfSwitch.SwitchSettingsProperty, Value = onStyle });

VisualState onHoveredState = new VisualState { Name = "OnHovered" };
onHoveredState.Setters.Add(new Setter { Property = SfSwitch.SwitchSettingsProperty, Value = onHoveredStyle });

VisualState onPressedState = new VisualState { Name = "OnPressed" };
onPressedState.Setters.Add(new Setter { Property = SfSwitch.SwitchSettingsProperty, Value = onPressedStyle });

VisualState onDisabledState = new VisualState { Name = "OnDisabled" };
onDisabledState.Setters.Add(new Setter { Property = SfSwitch.SwitchSettingsProperty, Value = onDisabledStyle });

VisualState offState = new VisualState { Name = "Off" };
offState.Setters.Add(new Setter { Property = SfSwitch.SwitchSettingsProperty, Value = offStyle });

VisualState offHoveredState = new VisualState { Name = "OffHovered" };
offHoveredState.Setters.Add(new Setter { Property = SfSwitch.SwitchSettingsProperty, Value = offHoveredStyle });

VisualState offPressedState = new VisualState { Name = "OffPressed" };
offPressedState.Setters.Add(new Setter { Property = SfSwitch.SwitchSettingsProperty, Value = offPressedStyle });

VisualState offDisabledState = new VisualState { Name = "OffDisabled" };
offDisabledState.Setters.Add(new Setter { Property = SfSwitch.SwitchSettingsProperty, Value = offDisabledStyle });

// Add all states to group
commonStateGroup.States.Add(onState);
commonStateGroup.States.Add(onHoveredState);
commonStateGroup.States.Add(onPressedState);
commonStateGroup.States.Add(onDisabledState);
commonStateGroup.States.Add(offState);
commonStateGroup.States.Add(offHoveredState);
commonStateGroup.States.Add(offPressedState);
commonStateGroup.States.Add(offDisabledState);

// Apply to switch
visualStateGroupList.Add(commonStateGroup);
VisualStateManager.SetVisualStateGroups(sfSwitch, visualStateGroupList);
this.Content = sfSwitch;
```

## Complete VSM Example

Here's a complete XAML example with all 12 visual states:

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:buttons="clr-namespace:Syncfusion.Maui.Buttons;assembly=Syncfusion.Maui.Buttons">

    <VerticalStackLayout Padding="30" Spacing="20">
        <Label Text="Custom VSM Switch" FontSize="20"/>
        
        <buttons:SfSwitch IsEnabled="True" IsOn="True">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup x:Name="CommonStates">
                    
                    <!-- Basic States -->
                    <VisualState x:Name="On">
                        <VisualState.Setters>
                            <Setter Property="SwitchSettings">
                                <Setter.Value>
                                    <buttons:SwitchSettings
                                        ThumbBackground="#F57B31"
                                        ThumbCornerRadius="20"
                                        ThumbHeightRequest="35"
                                        ThumbStroke="#F78F50"
                                        ThumbStrokeThickness="1"
                                        ThumbWidthRequest="35"
                                        TrackBackground="#F7D40D"
                                        TrackHeightRequest="50"
                                        TrackStroke="#DABA04"
                                        TrackCornerRadius="25"
                                        TrackStrokeThickness="1"
                                        TrackWidthRequest="90"/>
                                </Setter.Value>
                            </Setter>
                        </VisualState.Setters>
                    </VisualState>
                    
                    <VisualState x:Name="Off">
                        <VisualState.Setters>
                            <Setter Property="SwitchSettings">
                                <Setter.Value>
                                    <buttons:SwitchSettings
                                        ThumbBackground="#F0F5F8"
                                        ThumbCornerRadius="20"
                                        ThumbHeightRequest="35"
                                        ThumbStroke="#C7C9C9"
                                        ThumbStrokeThickness="1"
                                        ThumbWidthRequest="35"
                                        TrackBackground="#4FCFF7"
                                        TrackHeightRequest="50"
                                        TrackStroke="#359EBF"
                                        TrackCornerRadius="25"
                                        TrackStrokeThickness="1"
                                        TrackWidthRequest="90"/>
                                </Setter.Value>
                            </Setter>
                        </VisualState.Setters>
                    </VisualState>
                    
                    <!-- Hovered States -->
                    <VisualState x:Name="OnHovered">
                        <VisualState.Setters>
                            <Setter Property="SwitchSettings">
                                <Setter.Value>
                                    <buttons:SwitchSettings
                                        ThumbBackground="#F57B31"
                                        ThumbCornerRadius="20"
                                        ThumbHeightRequest="35"
                                        ThumbStroke="#E7600F"
                                        ThumbStrokeThickness="1"
                                        ThumbWidthRequest="35"
                                        TrackBackground="#F7D40D"
                                        TrackHeightRequest="50"
                                        TrackStroke="#DABA04"
                                        TrackCornerRadius="25"
                                        TrackStrokeThickness="1"
                                        TrackWidthRequest="90"/>
                                </Setter.Value>
                            </Setter>
                        </VisualState.Setters>
                    </VisualState>
                    
                    <VisualState x:Name="OffHovered">
                        <VisualState.Setters>
                            <Setter Property="SwitchSettings">
                                <Setter.Value>
                                    <buttons:SwitchSettings
                                        ThumbBackground="#FFFFFF"
                                        ThumbCornerRadius="20"
                                        ThumbHeightRequest="35"
                                        ThumbStroke="#959595"
                                        ThumbStrokeThickness="1"
                                        ThumbWidthRequest="35"
                                        TrackBackground="#72D4F3"
                                        TrackHeightRequest="50"
                                        TrackStroke="#359EBF"
                                        TrackCornerRadius="25"
                                        TrackStrokeThickness="1"
                                        TrackWidthRequest="90"/>
                                </Setter.Value>
                            </Setter>
                        </VisualState.Setters>
                    </VisualState>
                    
                    <!-- Pressed States -->
                    <VisualState x:Name="OnPressed">
                        <VisualState.Setters>
                            <Setter Property="SwitchSettings">
                                <Setter.Value>
                                    <buttons:SwitchSettings
                                        ThumbBackground="#F57B31"
                                        ThumbCornerRadius="24"
                                        ThumbHeightRequest="48"
                                        ThumbStroke="#E7600F"
                                        ThumbStrokeThickness="1"
                                        ThumbWidthRequest="48"
                                        TrackBackground="#F7D40D"
                                        TrackHeightRequest="50"
                                        TrackStroke="#DABA04"
                                        TrackCornerRadius="25"
                                        TrackStrokeThickness="1"
                                        TrackWidthRequest="90"/>
                                </Setter.Value>
                            </Setter>
                        </VisualState.Setters>
                    </VisualState>
                    
                    <VisualState x:Name="OffPressed">
                        <VisualState.Setters>
                            <Setter Property="SwitchSettings">
                                <Setter.Value>
                                    <buttons:SwitchSettings
                                        ThumbBackground="#FFFFFF"
                                        ThumbCornerRadius="24"
                                        ThumbHeightRequest="48"
                                        ThumbStroke="#959595"
                                        ThumbStrokeThickness="1"
                                        ThumbWidthRequest="48"
                                        TrackBackground="#72D4F3"
                                        TrackHeightRequest="50"
                                        TrackStroke="#359EBF"
                                        TrackCornerRadius="25"
                                        TrackStrokeThickness="1"
                                        TrackWidthRequest="90"/>
                                </Setter.Value>
                            </Setter>
                        </VisualState.Setters>
                    </VisualState>
                    
                    <!-- Disabled States -->
                    <VisualState x:Name="OnDisabled">
                        <VisualState.Setters>
                            <Setter Property="SwitchSettings">
                                <Setter.Value>
                                    <buttons:SwitchSettings
                                        ThumbBackground="#B0AFB2"
                                        ThumbCornerRadius="20"
                                        ThumbHeightRequest="35"
                                        ThumbStroke="#B0AFB2"
                                        ThumbStrokeThickness="1"
                                        ThumbWidthRequest="35"
                                        TrackBackground="#FEF7FF"
                                        TrackHeightRequest="50"
                                        TrackStroke="#B0AFB2"
                                        TrackCornerRadius="25"
                                        TrackStrokeThickness="1"
                                        TrackWidthRequest="90"/>
                                </Setter.Value>
                            </Setter>
                        </VisualState.Setters>
                    </VisualState>
                    
                    <VisualState x:Name="OffDisabled">
                        <VisualState.Setters>
                            <Setter Property="SwitchSettings">
                                <Setter.Value>
                                    <buttons:SwitchSettings
                                        ThumbBackground="#B0AFB2"
                                        ThumbCornerRadius="20"
                                        ThumbHeightRequest="35"
                                        ThumbStroke="#B0AFB2"
                                        ThumbStrokeThickness="1"
                                        ThumbWidthRequest="35"
                                        TrackBackground="#FEF7FF"
                                        TrackHeightRequest="50"
                                        TrackStroke="#B0AFB2"
                                        TrackCornerRadius="25"
                                        TrackStrokeThickness="1"
                                        TrackWidthRequest="90"/>
                                </Setter.Value>
                            </Setter>
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </buttons:SfSwitch>
    </VerticalStackLayout>
</ContentPage>
```

## Best Practices

### 1. Provide Hover Feedback for Desktop Apps
```xml
<!-- Subtle color change on hover -->
<VisualState x:Name="OnHovered">
    <VisualState.Setters>
        <Setter Property="SwitchSettings">
            <Setter.Value>
                <buttons:SwitchSettings ThumbStroke="#DarkerColor"/>
            </Setter.Value>
        </Setter>
    </VisualState.Setters>
</VisualState>
```

### 2. Show Press Feedback
```xml
<!-- Increase thumb size when pressed -->
<VisualState x:Name="OnPressed">
    <VisualState.Setters>
        <Setter Property="SwitchSettings">
            <Setter.Value>
                <buttons:SwitchSettings ThumbHeightRequest="48" ThumbWidthRequest="48"/>
            </Setter.Value>
        </Setter>
    </VisualState.Setters>
</VisualState>
```

### 3. Make Disabled States Obvious
```xml
<!-- Use muted colors for disabled -->
<VisualState x:Name="OnDisabled">
    <VisualState.Setters>
        <Setter Property="SwitchSettings">
            <Setter.Value>
                <buttons:SwitchSettings ThumbBackground="#B0AFB2" TrackBackground="#FEF7FF"/>
            </Setter.Value>
        </Setter>
    </VisualState.Setters>
</VisualState>
```

### 4. Maintain Consistent Dimensions
- Keep track and thumb dimensions consistent across non-pressed states
- Only change dimensions in pressed states for visual feedback

### 5. Use Subtle Transitions
- Don't make hover states drastically different
- Pressed states can be more pronounced
- Disabled states should be obviously non-interactive

### 6. Test on Target Platforms
- Hover states are relevant for desktop/web but not mobile touch
- Pressed states work everywhere
- Consider platform-specific VSM configurations

### 7. Reuse Settings Where Possible
```csharp
// Create base settings
var baseSettings = new SwitchSettings { /* common properties */ };

// Clone and modify for specific states
var hoveredSettings = baseSettings.Clone();
hoveredSettings.ThumbStroke = Colors.Darker;
```

### 8. Consider Performance
- VSM adds minimal overhead
- Avoid excessive animations in state transitions
- Test on low-end devices

### 9. Accessibility Considerations
- Ensure sufficient color contrast in all states
- Don't rely solely on color to indicate state
- Test with accessibility tools and screen readers

### 10. Document Your States
```xml
<!-- Good: Add comments explaining color choices -->
<!-- On State: Brand primary color -->
<VisualState x:Name="On">
    <VisualState.Setters>
        <Setter Property="SwitchSettings">
            <Setter.Value>
                <buttons:SwitchSettings TrackBackground="#BrandPrimary"/>
            </Setter.Value>
        </Setter>
    </VisualState.Setters>
</VisualState>
```
