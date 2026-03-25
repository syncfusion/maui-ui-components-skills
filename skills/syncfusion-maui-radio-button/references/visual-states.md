# Visual States in .NET MAUI Radio Button

This guide demonstrates how to use VisualStateManager to customize the appearance of Syncfusion .NET MAUI Radio Button (SfRadioButton) based on its state.

## Overview

The VisualStateManager in .NET MAUI allows you to define different visual appearances for controls based on their state. For the `SfRadioButton`, you can customize how the radio button looks in its **Checked** and **Unchecked** states.

## Available Visual States

The `SfRadioButton` control supports two visual states:

1. **Checked:** The radio button is selected (inner circle visible)
2. **Unchecked:** The radio button is not selected (only outer circle visible)

## Defining Visual States in XAML

Visual states are defined within the `VisualStateManager.VisualStateGroups` attached property.

### Basic Example

```xml
<buttons:SfRadioButton Text="Toggle Me">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            
            <VisualState x:Name="Checked">
                <VisualState.Setters>
                    <Setter Property="TextColor" Value="Blue"/>
                    <Setter Property="CheckedColor" Value="Blue"/>
                </VisualState.Setters>
            </VisualState>
            
            <VisualState x:Name="Unchecked">
                <VisualState.Setters>
                    <Setter Property="TextColor" Value="Gray"/>
                    <Setter Property="UncheckedColor" Value="Gray"/>
                </VisualState.Setters>
            </VisualState>
            
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</buttons:SfRadioButton>
```

### Complete XAML Example with Background

```xml
<buttons:SfRadioButton Text="Premium Plan" Padding="10">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            
            <VisualState x:Name="Checked">
                <VisualState.Setters>
                    <Setter Property="TextColor" Value="#1976D2"/>
                    <Setter Property="BackgroundColor" Value="#E3F2FD"/>
                    <Setter Property="CheckedColor" Value="#1976D2"/>
                    <Setter Property="FontAttributes" Value="Bold"/>
                </VisualState.Setters>
            </VisualState>
            
            <VisualState x:Name="Unchecked">
                <VisualState.Setters>
                    <Setter Property="TextColor" Value="#666666"/>
                    <Setter Property="BackgroundColor" Value="#F5F5F5"/>
                    <Setter Property="UncheckedColor" Value="#CCCCCC"/>
                    <Setter Property="FontAttributes" Value="None"/>
                </VisualState.Setters>
            </VisualState>
            
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</buttons:SfRadioButton>
```

### Properties You Can Customize

Within visual state setters, you can modify any visual property:

- **TextColor:** Caption text color
- **BackgroundColor:** Radio button background
- **CheckedColor:** Color when checked
- **UncheckedColor:** Color when unchecked
- **FontSize:** Text size
- **FontAttributes:** Bold, Italic, or None
- **FontFamily:** Font type
- **Opacity:** Transparency level

## Creating Visual States in C#

You can also define visual states programmatically using C#.

### Basic C# Example

```csharp
SfRadioButton radioButton = new SfRadioButton { Text = "Dynamic States" };

// Create visual state group list
VisualStateGroupList visualStateGroupList = new VisualStateGroupList();

// Create visual state group
VisualStateGroup commonStateGroup = new VisualStateGroup();

// Define Checked state
VisualState checkedState = new VisualState { Name = "Checked" };
checkedState.Setters.Add(new Setter 
{ 
    Property = SfRadioButton.TextColorProperty, 
    Value = Colors.Green 
});
checkedState.Setters.Add(new Setter 
{ 
    Property = SfRadioButton.CheckedColorProperty, 
    Value = Colors.Green 
});

// Define Unchecked state
VisualState uncheckedState = new VisualState { Name = "Unchecked" };
uncheckedState.Setters.Add(new Setter 
{ 
    Property = SfRadioButton.TextColorProperty, 
    Value = Colors.Gray 
});
uncheckedState.Setters.Add(new Setter 
{ 
    Property = SfRadioButton.UncheckedColorProperty, 
    Value = Colors.LightGray 
});

// Add states to group
commonStateGroup.States.Add(checkedState);
commonStateGroup.States.Add(uncheckedState);

// Add group to list
visualStateGroupList.Add(commonStateGroup);

// Apply to radio button
VisualStateManager.SetVisualStateGroups(radioButton, visualStateGroupList);
```

### Complete C# Example with Multiple Properties

```csharp
SfRadioButton CreateStyledRadioButton(string text)
{
    SfRadioButton radioButton = new SfRadioButton { Text = text, Padding = new Thickness(10) };
    
    VisualStateGroupList visualStateGroupList = new VisualStateGroupList();
    VisualStateGroup commonStateGroup = new VisualStateGroup();
    
    // Checked state
    VisualState checkedState = new VisualState { Name = "Checked" };
    checkedState.Setters.Add(new Setter 
    { 
        Property = SfRadioButton.TextColorProperty, 
        Value = Color.FromArgb("#1976D2") 
    });
    checkedState.Setters.Add(new Setter 
    { 
        Property = SfRadioButton.BackgroundColorProperty, 
        Value = Color.FromArgb("#E3F2FD") 
    });
    checkedState.Setters.Add(new Setter 
    { 
        Property = SfRadioButton.CheckedColorProperty, 
        Value = Color.FromArgb("#1976D2") 
    });
    checkedState.Setters.Add(new Setter 
    { 
        Property = SfRadioButton.FontAttributesProperty, 
        Value = FontAttributes.Bold 
    });
    
    // Unchecked state
    VisualState uncheckedState = new VisualState { Name = "Unchecked" };
    uncheckedState.Setters.Add(new Setter 
    { 
        Property = SfRadioButton.TextColorProperty, 
        Value = Color.FromArgb("#666666") 
    });
    uncheckedState.Setters.Add(new Setter 
    { 
        Property = SfRadioButton.BackgroundColorProperty, 
        Value = Color.FromArgb("#F5F5F5") 
    });
    uncheckedState.Setters.Add(new Setter 
    { 
        Property = SfRadioButton.UncheckedColorProperty, 
        Value = Color.FromArgb("#CCCCCC") 
    });
    uncheckedState.Setters.Add(new Setter 
    { 
        Property = SfRadioButton.FontAttributesProperty, 
        Value = FontAttributes.None 
    });
    
    commonStateGroup.States.Add(checkedState);
    commonStateGroup.States.Add(uncheckedState);
    visualStateGroupList.Add(commonStateGroup);
    
    VisualStateManager.SetVisualStateGroups(radioButton, visualStateGroupList);
    
    return radioButton;
}

// Usage
SfRadioGroup group = new SfRadioGroup();
group.Children.Add(CreateStyledRadioButton("Option 1"));
group.Children.Add(CreateStyledRadioButton("Option 2"));
group.Children.Add(CreateStyledRadioButton("Option 3"));
```

## Practical Examples

### Example 1: Success/Error States

```xml
<VerticalStackLayout Spacing="10">
    
    <!-- Success State -->
    <buttons:SfRadioButton Text="Valid Option">
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup x:Name="CommonStates">
                <VisualState x:Name="Checked">
                    <VisualState.Setters>
                        <Setter Property="TextColor" Value="#4CAF50"/>
                        <Setter Property="CheckedColor" Value="#4CAF50"/>
                        <Setter Property="BackgroundColor" Value="#E8F5E9"/>
                    </VisualState.Setters>
                </VisualState>
                <VisualState x:Name="Unchecked">
                    <VisualState.Setters>
                        <Setter Property="TextColor" Value="#757575"/>
                        <Setter Property="UncheckedColor" Value="#BDBDBD"/>
                        <Setter Property="BackgroundColor" Value="Transparent"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>
    </buttons:SfRadioButton>
    
    <!-- Error State -->
    <buttons:SfRadioButton Text="Invalid Option">
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup x:Name="CommonStates">
                <VisualState x:Name="Checked">
                    <VisualState.Setters>
                        <Setter Property="TextColor" Value="#F44336"/>
                        <Setter Property="CheckedColor" Value="#F44336"/>
                        <Setter Property="BackgroundColor" Value="#FFEBEE"/>
                    </VisualState.Setters>
                </VisualState>
                <VisualState x:Name="Unchecked">
                    <VisualState.Setters>
                        <Setter Property="TextColor" Value="#757575"/>
                        <Setter Property="UncheckedColor" Value="#BDBDBD"/>
                        <Setter Property="BackgroundColor" Value="Transparent"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>
    </buttons:SfRadioButton>
    
</VerticalStackLayout>
```

### Example 2: Card-Style Selection

```xml
<buttons:SfRadioButton Text="Premium Plan - $19.99/month" 
                       Padding="15" 
                       Margin="5">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            
            <VisualState x:Name="Checked">
                <VisualState.Setters>
                    <Setter Property="TextColor" Value="#FFFFFF"/>
                    <Setter Property="BackgroundColor" Value="#6200EE"/>
                    <Setter Property="CheckedColor" Value="#FFFFFF"/>
                    <Setter Property="FontAttributes" Value="Bold"/>
                </VisualState.Setters>
            </VisualState>
            
            <VisualState x:Name="Unchecked">
                <VisualState.Setters>
                    <Setter Property="TextColor" Value="#333333"/>
                    <Setter Property="BackgroundColor" Value="#FFFFFF"/>
                    <Setter Property="UncheckedColor" Value="#CCCCCC"/>
                    <Setter Property="FontAttributes" Value="None"/>
                </VisualState.Setters>
            </VisualState>
            
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</buttons:SfRadioButton>
```

### Example 3: Theme-Based States

```xml
<!-- Light Theme Radio Button -->
<buttons:SfRadioButton Text="Light Theme" Padding="10">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Checked">
                <VisualState.Setters>
                    <Setter Property="TextColor" Value="#1976D2"/>
                    <Setter Property="BackgroundColor" Value="#FFFFFF"/>
                    <Setter Property="CheckedColor" Value="#1976D2"/>
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Unchecked">
                <VisualState.Setters>
                    <Setter Property="TextColor" Value="#666666"/>
                    <Setter Property="BackgroundColor" Value="#F5F5F5"/>
                    <Setter Property="UncheckedColor" Value="#CCCCCC"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</buttons:SfRadioButton>

<!-- Dark Theme Radio Button -->
<buttons:SfRadioButton Text="Dark Theme" Padding="10">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Checked">
                <VisualState.Setters>
                    <Setter Property="TextColor" Value="#BB86FC"/>
                    <Setter Property="BackgroundColor" Value="#1E1E1E"/>
                    <Setter Property="CheckedColor" Value="#BB86FC"/>
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Unchecked">
                <VisualState.Setters>
                    <Setter Property="TextColor" Value="#CCCCCC"/>
                    <Setter Property="BackgroundColor" Value="#2C2C2C"/>
                    <Setter Property="UncheckedColor" Value="#6D6D6D"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</buttons:SfRadioButton>
```

### Example 4: Opacity Changes

```xml
<buttons:SfRadioButton Text="Fade Effect">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Checked">
                <VisualState.Setters>
                    <Setter Property="Opacity" Value="1.0"/>
                    <Setter Property="TextColor" Value="Black"/>
                    <Setter Property="CheckedColor" Value="Blue"/>
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Unchecked">
                <VisualState.Setters>
                    <Setter Property="Opacity" Value="0.5"/>
                    <Setter Property="TextColor" Value="Gray"/>
                    <Setter Property="UncheckedColor" Value="LightGray"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</buttons:SfRadioButton>
```

## Applying Visual States to Radio Groups

You can apply consistent visual states to all radio buttons in a group:

```xml
<buttons:SfRadioGroup>
    
    <buttons:SfRadioButton Text="Option 1">
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup x:Name="CommonStates">
                <VisualState x:Name="Checked">
                    <VisualState.Setters>
                        <Setter Property="TextColor" Value="Blue"/>
                        <Setter Property="CheckedColor" Value="Blue"/>
                        <Setter Property="BackgroundColor" Value="#E3F2FD"/>
                    </VisualState.Setters>
                </VisualState>
                <VisualState x:Name="Unchecked">
                    <VisualState.Setters>
                        <Setter Property="TextColor" Value="Gray"/>
                        <Setter Property="UncheckedColor" Value="Gray"/>
                        <Setter Property="BackgroundColor" Value="Transparent"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>
    </buttons:SfRadioButton>
    
    <!-- Repeat for other buttons with the same visual states -->
    <buttons:SfRadioButton Text="Option 2">
        <!-- Same visual states as above -->
    </buttons:SfRadioButton>
    
</buttons:SfRadioGroup>
```

## Dynamic Visual State Application

You can create reusable methods to apply visual states:

```csharp
public static class RadioButtonStyleHelper
{
    public static void ApplySuccessStates(SfRadioButton radioButton)
    {
        VisualStateGroupList visualStateGroupList = new VisualStateGroupList();
        VisualStateGroup commonStateGroup = new VisualStateGroup();
        
        // Checked state - Success green
        VisualState checkedState = new VisualState { Name = "Checked" };
        checkedState.Setters.Add(new Setter 
        { 
            Property = SfRadioButton.TextColorProperty, 
            Value = Color.FromArgb("#4CAF50") 
        });
        checkedState.Setters.Add(new Setter 
        { 
            Property = SfRadioButton.CheckedColorProperty, 
            Value = Color.FromArgb("#4CAF50") 
        });
        checkedState.Setters.Add(new Setter 
        { 
            Property = SfRadioButton.BackgroundColorProperty, 
            Value = Color.FromArgb("#E8F5E9") 
        });
        
        // Unchecked state - Neutral gray
        VisualState uncheckedState = new VisualState { Name = "Unchecked" };
        uncheckedState.Setters.Add(new Setter 
        { 
            Property = SfRadioButton.TextColorProperty, 
            Value = Color.FromArgb("#757575") 
        });
        uncheckedState.Setters.Add(new Setter 
        { 
            Property = SfRadioButton.UncheckedColorProperty, 
            Value = Color.FromArgb("#BDBDBD") 
        });
        uncheckedState.Setters.Add(new Setter 
        { 
            Property = SfRadioButton.BackgroundColorProperty, 
            Value = Colors.Transparent 
        });
        
        commonStateGroup.States.Add(checkedState);
        commonStateGroup.States.Add(uncheckedState);
        visualStateGroupList.Add(commonStateGroup);
        
        VisualStateManager.SetVisualStateGroups(radioButton, visualStateGroupList);
    }
    
    public static void ApplyWarningStates(SfRadioButton radioButton)
    {
        // Similar implementation with warning colors (orange/yellow)
    }
    
    public static void ApplyErrorStates(SfRadioButton radioButton)
    {
        // Similar implementation with error colors (red)
    }
}

// Usage
RadioButtonStyleHelper.ApplySuccessStates(option1);
RadioButtonStyleHelper.ApplyWarningStates(option2);
RadioButtonStyleHelper.ApplyErrorStates(option3);
```

## Best Practices

### 1. Keep Visual States Consistent

Apply the same visual state pattern across all radio buttons in a group for consistency.

### 2. Provide Clear Visual Feedback

Ensure there's a noticeable difference between checked and unchecked states:

```xml
<!-- Good contrast -->
<VisualState x:Name="Checked">
    <VisualState.Setters>
        <Setter Property="TextColor" Value="#000000"/>
        <Setter Property="CheckedColor" Value="#1976D2"/>
        <Setter Property="BackgroundColor" Value="#E3F2FD"/>
    </VisualState.Setters>
</VisualState>
```

### 3. Consider Accessibility

Ensure color combinations meet accessibility standards (WCAG AA or AAA):

- Maintain sufficient color contrast
- Don't rely solely on color to convey state
- Use additional visual cues (bold text, backgrounds)

### 4. Test on Multiple Themes

If your app supports light and dark themes, test visual states in both:

```csharp
private void ApplyThemeAwareStates(SfRadioButton radioButton)
{
    var isDarkTheme = Application.Current.RequestedTheme == AppTheme.Dark;
    
    // Apply different visual states based on theme
    if (isDarkTheme)
    {
        ApplyDarkThemeStates(radioButton);
    }
    else
    {
        ApplyLightThemeStates(radioButton);
    }
}
```

### 5. Avoid Overcomplicating

Keep visual states simple and focused. Too many property changes can be distracting:

```xml
<!-- Good - focused changes -->
<VisualState x:Name="Checked">
    <VisualState.Setters>
        <Setter Property="TextColor" Value="Blue"/>
        <Setter Property="CheckedColor" Value="Blue"/>
        <Setter Property="FontAttributes" Value="Bold"/>
    </VisualState.Setters>
</VisualState>

<!-- Avoid - too many changes -->
<VisualState x:Name="Checked">
    <VisualState.Setters>
        <Setter Property="TextColor" Value="Blue"/>
        <Setter Property="CheckedColor" Value="Red"/>
        <Setter Property="BackgroundColor" Value="Yellow"/>
        <Setter Property="FontSize" Value="20"/>
        <Setter Property="Opacity" Value="0.7"/>
        <Setter Property="Rotation" Value="5"/>
    </VisualState.Setters>
</VisualState>
```

### 6. Performance Considerations

Visual states are lightweight, but avoid creating overly complex visual trees. Keep the number of setters reasonable.

## Troubleshooting

### Visual States Not Applying

**Issue:** Visual states don't seem to work.

**Solutions:**
1. Verify the state names are exactly "Checked" and "Unchecked" (case-sensitive)
2. Check that property names are correct
3. Ensure the VisualStateGroup is named "CommonStates"
4. Verify you're using the correct property type for values

### States Only Work Partially

**Issue:** Some properties change but others don't.

**Solution:** Check that all properties you're trying to set are bindable properties of `SfRadioButton`.

### C# Visual States Not Working

**Issue:** Programmatically created visual states don't apply.

**Solution:** Ensure you call `VisualStateManager.SetVisualStateGroups()` after creating all states and before adding the control to the visual tree.

## Summary

Visual states provide a powerful way to customize radio button appearance based on user interaction. They offer:

- **Declarative styling** in XAML or code
- **Dynamic appearance changes** without event handlers
- **Reusable patterns** across your application
- **Clean separation** of appearance from behavior

Use visual states when you need sophisticated visual feedback beyond basic color changes, especially for card-style selections, themed interfaces, or state-dependent styling.
