# Visual States in .NET MAUI CheckBox

## Table of Contents
- [Overview](#overview)
- [Visual State Manager Basics](#visual-state-manager-basics)
- [Available Visual States](#available-visual-states)
- [XAML Implementation](#xaml-implementation)
- [C# Implementation](#c-implementation)
- [Customizable Properties](#customizable-properties)
- [Complete Examples](#complete-examples)
- [Advanced Scenarios](#advanced-scenarios)

---

This guide covers how to use the Visual State Manager (VSM) to customize the appearance of the CheckBox control based on its state.

## Overview

The Visual State Manager allows you to define different visual appearances for the CheckBox based on its current state. Instead of manually changing properties in event handlers, you can declaratively define how the control should look in each state.

**Benefits:**
- Declarative, XAML-based styling
- Cleaner code without event handler logic for visual changes
- Consistent state-based styling
- Better separation of concerns

## Visual State Manager Basics

The VisualStateManager is a .NET MAUI feature that applies visual changes based on the control's state. For checkboxes, states automatically transition when `IsChecked` changes.

### How it Works:

1. Define a `VisualStateGroup` containing multiple `VisualState` objects
2. Each `VisualState` represents a state (Checked, Unchecked, Indeterminate)
3. Each state contains `Setter` objects that modify properties
4. The framework automatically applies the appropriate state's setters

## Available Visual States

The SfCheckBox control supports three visual states:

| State Name | Triggered When | Common Use |
|------------|----------------|------------|
| **Checked** | `IsChecked = true` | Show selected appearance |
| **Unchecked** | `IsChecked = false` | Show default/unselected appearance |
| **Intermediate** | `IsChecked = null` | Show partial/indeterminate appearance |

**Note:** The Intermediate state only applies when `IsThreeState="True"`.

## XAML Implementation

### Basic Structure

```xml
<buttons:SfCheckBox Text="CheckBox" IsThreeState="True">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            
            <VisualState x:Name="Checked">
                <VisualState.Setters>
                    <!-- Property setters for checked state -->
                </VisualState.Setters>
            </VisualState>
            
            <VisualState x:Name="Unchecked">
                <VisualState.Setters>
                    <!-- Property setters for unchecked state -->
                </VisualState.Setters>
            </VisualState>
            
            <VisualState x:Name="Intermediate">
                <VisualState.Setters>
                    <!-- Property setters for intermediate state -->
                </VisualState.Setters>
            </VisualState>
            
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</buttons:SfCheckBox>
```

### Complete XAML Example

```xml
<buttons:SfCheckBox Text="CheckBox" IsThreeState="True">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            
            <VisualState x:Name="Checked">
                <VisualState.Setters>
                    <Setter Property="TextColor" Value="Blue"/>
                    <Setter Property="CheckedColor" Value="Blue"/>
                    <Setter Property="TickColor" Value="White"/>
                </VisualState.Setters>
            </VisualState>
            
            <VisualState x:Name="Unchecked">
                <VisualState.Setters>
                    <Setter Property="TextColor" Value="#ea3737"/>
                    <Setter Property="UncheckedColor" Value="#ea3737"/>
                </VisualState.Setters>
            </VisualState>
            
            <VisualState x:Name="Intermediate">
                <VisualState.Setters>
                    <Setter Property="CheckedColor" Value="Blue"/>
                    <Setter Property="Text" Value="Intermediate State"/>
                    <Setter Property="TextColor" Value="Gray"/>
                </VisualState.Setters>
            </VisualState>
            
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</buttons:SfCheckBox>
```

## C# Implementation

### Basic Structure

```csharp
SfCheckBox checkBox = new SfCheckBox 
{ 
    Text = "CheckBox", 
    IsThreeState = true 
};

VisualStateGroupList visualStateGroupList = new VisualStateGroupList();
VisualStateGroup commonStateGroup = new VisualStateGroup();

// Define Checked state
VisualState checkedState = new VisualState { Name = "Checked" };
checkedState.Setters.Add(new Setter { Property = SfCheckBox.TextColorProperty, Value = Colors.Blue });
checkedState.Setters.Add(new Setter { Property = SfCheckBox.CheckedColorProperty, Value = Colors.Blue });

// Define Unchecked state
VisualState uncheckedState = new VisualState { Name = "Unchecked" };
uncheckedState.Setters.Add(new Setter { Property = SfCheckBox.TextColorProperty, Value = Color.FromArgb("#ea3737") });
uncheckedState.Setters.Add(new Setter { Property = SfCheckBox.UncheckedColorProperty, Value = Color.FromArgb("#ea3737") });

// Define Intermediate state
VisualState intermediateState = new VisualState { Name = "Intermediate" };
intermediateState.Setters.Add(new Setter { Property = SfCheckBox.TextProperty, Value = "Intermediate State" });
intermediateState.Setters.Add(new Setter { Property = SfCheckBox.CheckedColorProperty, Value = Colors.Blue });

// Add states to group
commonStateGroup.States.Add(checkedState);
commonStateGroup.States.Add(uncheckedState);
commonStateGroup.States.Add(intermediateState);

// Apply to checkbox
visualStateGroupList.Add(commonStateGroup);
VisualStateManager.SetVisualStateGroups(checkBox, visualStateGroupList);

this.Content = checkBox;
```

### Complete C# Example

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
        
        SfCheckBox checkBox = CreateStyledCheckBox();
        this.Content = checkBox;
    }

    private SfCheckBox CreateStyledCheckBox()
    {
        SfCheckBox checkBox = new SfCheckBox
        {
            Text = "CheckBox",
            IsThreeState = true
        };

        VisualStateGroupList visualStateGroupList = new VisualStateGroupList();
        VisualStateGroup commonStateGroup = new VisualStateGroup();

        // Checked State
        VisualState checkedState = new VisualState
        {
            Name = "Checked"
        };
        checkedState.Setters.Add(new Setter 
        { 
            Property = SfCheckBox.TextColorProperty, 
            Value = Colors.Blue 
        });
        checkedState.Setters.Add(new Setter 
        { 
            Property = SfCheckBox.CheckedColorProperty, 
            Value = Colors.Blue 
        });
        checkedState.Setters.Add(new Setter 
        { 
            Property = SfCheckBox.TickColorProperty, 
            Value = Colors.White 
        });

        // Unchecked State
        VisualState uncheckedState = new VisualState
        {
            Name = "Unchecked"
        };
        uncheckedState.Setters.Add(new Setter 
        { 
            Property = SfCheckBox.TextColorProperty, 
            Value = Color.FromArgb("#ea3737") 
        });
        uncheckedState.Setters.Add(new Setter 
        { 
            Property = SfCheckBox.UncheckedColorProperty, 
            Value = Color.FromArgb("#ea3737") 
        });

        // Intermediate State
        VisualState intermediateState = new VisualState
        {
            Name = "Intermediate"
        };
        intermediateState.Setters.Add(new Setter 
        { 
            Property = SfCheckBox.TextProperty, 
            Value = "Intermediate State" 
        });
        intermediateState.Setters.Add(new Setter 
        { 
            Property = SfCheckBox.CheckedColorProperty, 
            Value = Colors.Blue 
        });
        intermediateState.Setters.Add(new Setter 
        { 
            Property = SfCheckBox.TextColorProperty, 
            Value = Colors.Gray 
        });

        commonStateGroup.States.Add(checkedState);
        commonStateGroup.States.Add(uncheckedState);
        commonStateGroup.States.Add(intermediateState);

        visualStateGroupList.Add(commonStateGroup);
        VisualStateManager.SetVisualStateGroups(checkBox, visualStateGroupList);

        return checkBox;
    }
}
```

## Customizable Properties

The following properties can be customized in visual states:

### Color Properties
- `TextColor` - Caption text color
- `CheckedColor` - Checkbox background color when checked/intermediate
- `UncheckedColor` - Checkbox border color when unchecked
- `TickColor` - Checkmark color

### Text Properties
- `Text` - Caption text (can change per state)
- `FontSize` - Text size
- `FontFamily` - Text font
- `FontAttributes` - Bold, Italic, None

### Visual Properties
- `CornerRadius` - Checkbox corner rounding
- `StrokeThickness` - Border thickness
- `ControlSize` - Checkbox box size
- `ContentSpacing` - Space between box and text

### Other Properties
- `IsEnabled` - Enable/disable the control
- `Opacity` - Transparency level

## Complete Examples

### Example 1: Traffic Light Pattern

```xml
<buttons:SfCheckBox Text="Status" IsThreeState="True">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            
            <!-- Green = Checked (Good) -->
            <VisualState x:Name="Checked">
                <VisualState.Setters>
                    <Setter Property="CheckedColor" Value="#28A745"/>
                    <Setter Property="TextColor" Value="#28A745"/>
                    <Setter Property="Text" Value="✓ Approved"/>
                    <Setter Property="FontAttributes" Value="Bold"/>
                </VisualState.Setters>
            </VisualState>
            
            <!-- Red = Unchecked (Bad) -->
            <VisualState x:Name="Unchecked">
                <VisualState.Setters>
                    <Setter Property="UncheckedColor" Value="#DC3545"/>
                    <Setter Property="TextColor" Value="#DC3545"/>
                    <Setter Property="Text" Value="✗ Rejected"/>
                    <Setter Property="FontAttributes" Value="Bold"/>
                </VisualState.Setters>
            </VisualState>
            
            <!-- Yellow = Intermediate (Pending) -->
            <VisualState x:Name="Intermediate">
                <VisualState.Setters>
                    <Setter Property="CheckedColor" Value="#FFC107"/>
                    <Setter Property="TextColor" Value="#FFC107"/>
                    <Setter Property="Text" Value="⏳ Pending"/>
                    <Setter Property="FontAttributes" Value="Bold"/>
                </VisualState.Setters>
            </VisualState>
            
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</buttons:SfCheckBox>
```

### Example 2: High Contrast Mode

```xml
<buttons:SfCheckBox Text="Toggle" IsThreeState="False">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            
            <VisualState x:Name="Checked">
                <VisualState.Setters>
                    <Setter Property="CheckedColor" Value="Black"/>
                    <Setter Property="TickColor" Value="Yellow"/>
                    <Setter Property="TextColor" Value="Black"/>
                    <Setter Property="StrokeThickness" Value="3"/>
                    <Setter Property="FontSize" Value="18"/>
                    <Setter Property="FontAttributes" Value="Bold"/>
                </VisualState.Setters>
            </VisualState>
            
            <VisualState x:Name="Unchecked">
                <VisualState.Setters>
                    <Setter Property="UncheckedColor" Value="Black"/>
                    <Setter Property="TextColor" Value="Black"/>
                    <Setter Property="StrokeThickness" Value="3"/>
                    <Setter Property="FontSize" Value="18"/>
                    <Setter Property="FontAttributes" Value="Bold"/>
                </VisualState.Setters>
            </VisualState>
            
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</buttons:SfCheckBox>
```

### Example 3: Soft Material Design

```xml
<buttons:SfCheckBox Text="Material CheckBox" IsThreeState="False">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            
            <VisualState x:Name="Checked">
                <VisualState.Setters>
                    <Setter Property="CheckedColor" Value="#6200EE"/>
                    <Setter Property="TickColor" Value="White"/>
                    <Setter Property="TextColor" Value="#6200EE"/>
                    <Setter Property="CornerRadius" Value="3"/>
                    <Setter Property="StrokeThickness" Value="2"/>
                </VisualState.Setters>
            </VisualState>
            
            <VisualState x:Name="Unchecked">
                <VisualState.Setters>
                    <Setter Property="UncheckedColor" Value="#757575"/>
                    <Setter Property="TextColor" Value="#212121"/>
                    <Setter Property="CornerRadius" Value="3"/>
                    <Setter Property="StrokeThickness" Value="2"/>
                </VisualState.Setters>
            </VisualState>
            
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</buttons:SfCheckBox>
```

### Example 4: Dynamic Text Per State

```xml
<buttons:SfCheckBox IsThreeState="True" IsChecked="{x:Null}">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            
            <VisualState x:Name="Checked">
                <VisualState.Setters>
                    <Setter Property="Text" Value="All tasks completed!"/>
                    <Setter Property="CheckedColor" Value="Green"/>
                    <Setter Property="TextColor" Value="Green"/>
                </VisualState.Setters>
            </VisualState>
            
            <VisualState x:Name="Unchecked">
                <VisualState.Setters>
                    <Setter Property="Text" Value="No tasks completed"/>
                    <Setter Property="UncheckedColor" Value="Red"/>
                    <Setter Property="TextColor" Value="Red"/>
                </VisualState.Setters>
            </VisualState>
            
            <VisualState x:Name="Intermediate">
                <VisualState.Setters>
                    <Setter Property="Text" Value="Some tasks pending..."/>
                    <Setter Property="CheckedColor" Value="Orange"/>
                    <Setter Property="TextColor" Value="Orange"/>
                </VisualState.Setters>
            </VisualState>
            
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</buttons:SfCheckBox>
```

## Advanced Scenarios

### Scenario 1: Theme-Based Visual States

Create reusable styles with visual states:

```xml
<ContentPage.Resources>
    <Style x:Key="ThemeCheckBoxStyle" TargetType="buttons:SfCheckBox">
        <Setter Property="IsThreeState" Value="True"/>
        <Setter Property="VisualStateManager.VisualStateGroups">
            <VisualStateGroupList>
                <VisualStateGroup x:Name="CommonStates">
                    <VisualState x:Name="Checked">
                        <VisualState.Setters>
                            <Setter Property="CheckedColor" Value="{AppThemeBinding Light=#007AFF, Dark=#0A84FF}"/>
                            <Setter Property="TextColor" Value="{AppThemeBinding Light=#000000, Dark=#FFFFFF}"/>
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState x:Name="Unchecked">
                        <VisualState.Setters>
                            <Setter Property="UncheckedColor" Value="{AppThemeBinding Light=#8E8E93, Dark=#636366}"/>
                            <Setter Property="TextColor" Value="{AppThemeBinding Light=#000000, Dark=#FFFFFF}"/>
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateGroupList>
        </Setter>
    </Style>
</ContentPage.Resources>

<buttons:SfCheckBox Text="Themed CheckBox" Style="{StaticResource ThemeCheckBoxStyle}"/>
```

### Scenario 2: Animation-Like Effect with Opacity

```xml
<buttons:SfCheckBox Text="Fade Effect" IsThreeState="False">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            
            <VisualState x:Name="Checked">
                <VisualState.Setters>
                    <Setter Property="CheckedColor" Value="Blue"/>
                    <Setter Property="Opacity" Value="1.0"/>
                </VisualState.Setters>
            </VisualState>
            
            <VisualState x:Name="Unchecked">
                <VisualState.Setters>
                    <Setter Property="UncheckedColor" Value="Gray"/>
                    <Setter Property="Opacity" Value="0.5"/>
                </VisualState.Setters>
            </VisualState>
            
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</buttons:SfCheckBox>
```

### Scenario 3: Size Changes Per State

```xml
<buttons:SfCheckBox Text="Dynamic Size" IsThreeState="False">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            
            <VisualState x:Name="Checked">
                <VisualState.Setters>
                    <Setter Property="ControlSize" Value="32"/>
                    <Setter Property="FontSize" Value="18"/>
                    <Setter Property="CheckedColor" Value="Blue"/>
                </VisualState.Setters>
            </VisualState>
            
            <VisualState x:Name="Unchecked">
                <VisualState.Setters>
                    <Setter Property="ControlSize" Value="24"/>
                    <Setter Property="FontSize" Value="16"/>
                    <Setter Property="UncheckedColor" Value="Gray"/>
                </VisualState.Setters>
            </VisualState>
            
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</buttons:SfCheckBox>
```

## Best Practices

1. **Use Visual States for appearance only** - Keep business logic in event handlers
2. **Define all three states** when using `IsThreeState="True"`
3. **Keep consistency** - Use similar styling patterns across states
4. **Test all states** - Verify appearance in all state combinations
5. **Consider accessibility** - Ensure sufficient color contrast in all states
6. **Reuse with styles** - Create reusable Style resources for common patterns
7. **Document state changes** - Comment complex visual state configurations

## Visual State vs. Property Setting

### When to use Visual States:
- ✅ State-dependent styling (different colors per state)
- ✅ Complex appearance changes
- ✅ Declarative, XAML-based styling
- ✅ Reusable style patterns

### When to use direct properties:
- ✅ Static styling (same for all states)
- ✅ Simple customization
- ✅ Runtime/dynamic changes
- ✅ Code-based configuration

## Common Gotchas

**Issue**: Visual states not applying  
**Solution**: Ensure `VisualStateGroup` name is "CommonStates" and state names match exactly (case-sensitive)

**Issue**: Intermediate state doesn't show  
**Solution**: Set `IsThreeState="True"` on the CheckBox

**Issue**: Properties not changing  
**Solution**: Verify property names match exactly (use IntelliSense or check documentation)

**Issue**: Conflicting property values  
**Solution**: Visual state setters override direct property assignments; remove direct assignments if using visual states
