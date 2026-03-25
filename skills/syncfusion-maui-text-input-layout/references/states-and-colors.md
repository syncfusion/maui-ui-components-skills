# States, Colors, and Theming in .NET MAUI Text Input Layout

## Table of Contents
- [Visual States Overview](#visual-states-overview)
- [Stroke Property](#stroke-property)
- [Visual State Manager Integration](#visual-state-manager-integration)
- [Stroke Thickness](#stroke-thickness)
- [Disabled State](#disabled-state)
- [Container Background Colors](#container-background-colors)
- [Label Text Colors](#label-text-colors)
- [CurrentActiveColor](#currentactivecolor)
- [Theme Integration](#theme-integration)
- [Best Practices](#best-practices)

## Visual States Overview

SfTextInputLayout supports three main visual states that affect the appearance of borders, strokes, and labels:

- **Normal** — Default state when unfocused
- **Focused** — Active state when input has focus
- **Error** — Validation failure state (when `HasError="True"`)

Additionally, there's a **Disabled** state when `IsEnabled="False"`.

## Stroke Property

The **Stroke** property controls the color of borders and lines in the text input layout.

### Basic Stroke Color

#### XAML

```xml
<inputLayout:SfTextInputLayout Hint="Name"
                               Stroke="#00AFA0"
                               HelperText="Enter your name">
    <Entry />
</inputLayout:SfTextInputLayout>
```

#### C#

```csharp
var inputLayout = new SfTextInputLayout
{
    Hint = "Name",
    Stroke = Color.FromArgb("#00AFA0"),
    HelperText = "Enter your name",
    Content = new Entry()
};
```

### What Stroke Affects

- **Filled Container:** Bottom line stroke
- **Outlined Container:** Border around the container
- **None Container:** Bottom line stroke
- **Hint Label:** Text color when floated
- **Character Counter:** Text color

**Note:** The cursor color of the input view matches the platform's accent color, not the Stroke property.

## Visual State Manager Integration

Use Visual State Manager to customize colors for different states.

### Complete VSM Example

#### XAML

```xml
<inputLayout:SfTextInputLayout Hint="Username"
                               Stroke="#00AFA0"
                               HelperText="Enter your username">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroupList>
            <VisualStateGroup x:Name="CommonStates">
                
                <VisualState Name="Normal">
                    <VisualState.Setters>
                        <Setter Property="Stroke" Value="#79747E"/>
                    </VisualState.Setters>
                </VisualState>
                
                <VisualState Name="Focused">
                    <VisualState.Setters>
                        <Setter Property="Stroke" Value="#6750A4"/>
                    </VisualState.Setters>
                </VisualState>
                
                <VisualState Name="Error">
                    <VisualState.Setters>
                        <Setter Property="Stroke" Value="#B3261E"/>
                    </VisualState.Setters>
                </VisualState>
                
            </VisualStateGroup>
        </VisualStateGroupList>
    </VisualStateManager.VisualStateGroups>
    
    <Entry />
</inputLayout:SfTextInputLayout>
```

#### C#

```csharp
var inputLayout = new SfTextInputLayout
{
    Hint = "Username",
    HelperText = "Enter your username",
    Content = new Entry()
};

var visualStateGroupList = new VisualStateGroupList();
var visualStateGroup = new VisualStateGroup { Name = "CommonStates" };

// Normal State
var normalState = new VisualState { Name = "Normal" };
normalState.Setters.Add(new Setter 
{ 
    Property = SfTextInputLayout.StrokeProperty, 
    Value = Color.FromArgb("#79747E") 
});

// Focused State
var focusedState = new VisualState { Name = "Focused" };
focusedState.Setters.Add(new Setter 
{ 
    Property = SfTextInputLayout.StrokeProperty, 
    Value = Color.FromArgb("#6750A4") 
});

// Error State
var errorState = new VisualState { Name = "Error" };
errorState.Setters.Add(new Setter 
{ 
    Property = SfTextInputLayout.StrokeProperty, 
    Value = Color.FromArgb("#B3261E") 
});

visualStateGroup.States.Add(normalState);
visualStateGroup.States.Add(focusedState);
visualStateGroup.States.Add(errorState);

visualStateGroupList.Add(visualStateGroup);

VisualStateManager.SetVisualStateGroups(inputLayout, visualStateGroupList);
```

### Material Design 3 Color Scheme

```xml
<inputLayout:SfTextInputLayout Hint="Email"
                               ContainerType="Outlined">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroupList>
            <VisualStateGroup x:Name="CommonStates">
                <VisualState Name="Normal">
                    <VisualState.Setters>
                        <Setter Property="Stroke" Value="#79747E"/>  <!-- Outline -->
                    </VisualState.Setters>
                </VisualState>
                <VisualState Name="Focused">
                    <VisualState.Setters>
                        <Setter Property="Stroke" Value="#6750A4"/>  <!-- Primary -->
                    </VisualState.Setters>
                </VisualState>
                <VisualState Name="Error">
                    <VisualState.Setters>
                        <Setter Property="Stroke" Value="#B3261E"/>  <!-- Error -->
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateGroupList>
    </VisualStateManager.VisualStateGroups>
    <Entry />
</inputLayout:SfTextInputLayout>
```

### Custom Brand Colors

```xml
<inputLayout:SfTextInputLayout Hint="Company Email"
                               ContainerType="Filled">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroupList>
            <VisualStateGroup x:Name="CommonStates">
                <VisualState Name="Normal">
                    <VisualState.Setters>
                        <Setter Property="Stroke" Value="#CCCCCC"/>
                    </VisualState.Setters>
                </VisualState>
                <VisualState Name="Focused">
                    <VisualState.Setters>
                        <Setter Property="Stroke" Value="#0066CC"/>  <!-- Brand blue -->
                    </VisualState.Setters>
                </VisualState>
                <VisualState Name="Error">
                    <VisualState.Setters>
                        <Setter Property="Stroke" Value="#D32F2F"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateGroupList>
    </VisualStateManager.VisualStateGroups>
    <Entry />
</inputLayout:SfTextInputLayout>
```

## Stroke Thickness

Customize the thickness of strokes/borders for focused and unfocused states.

### FocusedStrokeThickness and UnfocusedStrokeThickness

#### XAML

```xml
<inputLayout:SfTextInputLayout Hint="Name"
                               ContainerType="Outlined"
                               FocusedStrokeThickness="4"
                               UnfocusedStrokeThickness="2">
    <Entry />
</inputLayout:SfTextInputLayout>
```

#### C#

```csharp
var inputLayout = new SfTextInputLayout
{
    Hint = "Name",
    ContainerType = ContainerType.Outlined,
    FocusedStrokeThickness = 4,
    UnfocusedStrokeThickness = 2,
    Content = new Entry()
};
```

### Effect by Container Type

| Container Type | Unfocused | Focused |
|----------------|-----------|---------|
| **Outlined** | Border thickness | Border thickness (thicker) |
| **Filled** | Bottom line thickness | Bottom line thickness (thicker) |
| **None** | Bottom line thickness | Bottom line thickness (thicker) |

### Recommended Values

```xml
<!-- Subtle change -->
<inputLayout:SfTextInputLayout FocusedStrokeThickness="2"
                               UnfocusedStrokeThickness="1">
```

```xml
<!-- Prominent change (default-like) -->
<inputLayout:SfTextInputLayout FocusedStrokeThickness="3"
                               UnfocusedStrokeThickness="1">
```

```xml
<!-- Bold emphasis -->
<inputLayout:SfTextInputLayout FocusedStrokeThickness="4"
                               UnfocusedStrokeThickness="2">
```

### Example with Custom Colors and Thickness

```xml
<inputLayout:SfTextInputLayout Hint="Password"
                               ContainerType="Outlined"
                               FocusedStrokeThickness="3"
                               UnfocusedStrokeThickness="1"
                               OutlineCornerRadius="8">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroupList>
            <VisualStateGroup x:Name="CommonStates">
                <VisualState Name="Normal">
                    <VisualState.Setters>
                        <Setter Property="Stroke" Value="#E0E0E0"/>
                    </VisualState.Setters>
                </VisualState>
                <VisualState Name="Focused">
                    <VisualState.Setters>
                        <Setter Property="Stroke" Value="#1976D2"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateGroupList>
    </VisualStateManager.VisualStateGroups>
    <Entry IsPassword="True" />
</inputLayout:SfTextInputLayout>
```

## Disabled State

Disable the text input layout to prevent user interaction.

### Basic Disabled State

#### XAML

```xml
<inputLayout:SfTextInputLayout Hint="Read Only"
                               IsEnabled="False">
    <Entry Text="Cannot edit this" />
</inputLayout:SfTextInputLayout>
```

#### C#

```csharp
var inputLayout = new SfTextInputLayout
{
    Hint = "Read Only",
    IsEnabled = false,
    Content = new Entry { Text = "Cannot edit this" }
};
```

### Visual Changes When Disabled

- Container and border colors dim to gray
- Text appears lighter/grayed out
- Hint label color changes to disabled state
- User cannot interact with the input
- Cannot customize disabled colors (platform default)

### Conditional Disabling

```csharp
// Enable/disable based on checkbox
private void OnAgreeCheckChanged(object sender, CheckedChangedEventArgs e)
{
    submitButton.IsEnabled = e.Value;
    emailInputLayout.IsEnabled = e.Value;
}
```

## Container Background Colors

Customize the background fill color for Filled and Outlined containers.

### Filled Container Background

```xml
<inputLayout:SfTextInputLayout Hint="Email"
                               ContainerType="Filled"
                               ContainerBackground="#E6EEF9"
                               Stroke="#0450C2">
    <Entry />
</inputLayout:SfTextInputLayout>
```

```csharp
var inputLayout = new SfTextInputLayout
{
    Hint = "Email",
    ContainerType = ContainerType.Filled,
    ContainerBackground = Color.FromArgb("#E6EEF9"),
    Stroke = Color.FromArgb("#0450C2"),
    Content = new Entry()
};
```

### Outlined Container Background

```xml
<inputLayout:SfTextInputLayout Hint="Phone"
                               ContainerType="Outlined"
                               ContainerBackground="#F5F5F5"
                               Stroke="#0450C2">
    <Entry Keyboard="Telephone" />
</inputLayout:SfTextInputLayout>
```

### Transparent Background

```xml
<inputLayout:SfTextInputLayout Hint="Name"
                               ContainerType="Filled"
                               ContainerBackground="Transparent">
    <Entry />
</inputLayout:SfTextInputLayout>
```

### Background with VSM

```xml
<inputLayout:SfTextInputLayout Hint="Username"
                               ContainerType="Filled"
                               ContainerBackground="#F5F5F5">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroupList>
            <VisualStateGroup x:Name="CommonStates">
                <VisualState Name="Normal">
                    <VisualState.Setters>
                        <Setter Property="ContainerBackground" Value="#F5F5F5"/>
                        <Setter Property="Stroke" Value="#999999"/>
                    </VisualState.Setters>
                </VisualState>
                <VisualState Name="Focused">
                    <VisualState.Setters>
                        <Setter Property="ContainerBackground" Value="#E8F0FE"/>
                        <Setter Property="Stroke" Value="#1976D2"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateGroupList>
    </VisualStateManager.VisualStateGroups>
    <Entry />
</inputLayout:SfTextInputLayout>
```

## Label Text Colors

Customize the color of hint, helper, and error labels.

### Hint Label Color

```xml
<inputLayout:SfTextInputLayout Hint="Name"
                               ContainerType="Outlined"
                               Stroke="Red">
    <inputLayout:SfTextInputLayout.HintLabelStyle>
        <inputLayout:LabelStyle TextColor="Green"/>
    </inputLayout:SfTextInputLayout.HintLabelStyle>
    <Entry Text="John" />
</inputLayout:SfTextInputLayout>
```

### Helper Text Color

```xml
<inputLayout:SfTextInputLayout Hint="Email"
                               HelperText="We'll never share your email">
    <inputLayout:SfTextInputLayout.HelperLabelStyle>
        <inputLayout:LabelStyle TextColor="Blue"/>
    </inputLayout:SfTextInputLayout.HelperLabelStyle>
    <Entry />
</inputLayout:SfTextInputLayout>
```

### Error Text Color

```xml
<inputLayout:SfTextInputLayout Hint="Password"
                               ErrorText="Password too weak"
                               HasError="True">
    <inputLayout:SfTextInputLayout.ErrorLabelStyle>
        <inputLayout:LabelStyle TextColor="Maroon"/>
    </inputLayout:SfTextInputLayout.ErrorLabelStyle>
    <Entry IsPassword="True" />
</inputLayout:SfTextInputLayout>
```

### All Labels with Custom Colors

```xml
<inputLayout:SfTextInputLayout Hint="Username"
                               HelperText="3-20 characters"
                               ErrorText="Username taken"
                               ContainerType="Outlined"
                               Stroke="Red">
    
    <inputLayout:SfTextInputLayout.HintLabelStyle>
        <inputLayout:LabelStyle TextColor="Green"/>
    </inputLayout:SfTextInputLayout.HintLabelStyle>
    
    <inputLayout:SfTextInputLayout.HelperLabelStyle>
        <inputLayout:LabelStyle TextColor="Blue"/>
    </inputLayout:SfTextInputLayout.HelperLabelStyle>
    
    <inputLayout:SfTextInputLayout.ErrorLabelStyle>
        <inputLayout:LabelStyle TextColor="Maroon"/>
    </inputLayout:SfTextInputLayout.ErrorLabelStyle>
    
    <Entry Text="John" />
    
</inputLayout:SfTextInputLayout>
```

## CurrentActiveColor

The **CurrentActiveColor** property is read-only and returns the currently applied stroke color based on the active visual state.

### Using CurrentActiveColor

```csharp
// Access the current stroke color
Color activeColor = inputLayout.CurrentActiveColor;

// Example: Update another element based on current color
borderView.BackgroundColor = inputLayout.CurrentActiveColor;
```

**Note:** You cannot set `CurrentActiveColor` directly. It reflects the current state's stroke color.

### When CurrentActiveColor Updates

- Changes when visual state changes (Normal → Focused → Error)
- Reflects the color set by Visual State Manager
- Falls back to `Stroke` property if no VSM is defined

**Important:** `CurrentActiveColor` does NOT change to error color when `HasError` is set to `true`. It only reflects visual state changes.

## Theme Integration

### Creating a Global Style

Define styles in `App.xaml` or `Resources/Styles/Styles.xaml`:

```xml
<Application.Resources>
    <ResourceDictionary>
        
        <!-- Material Design 3 Style -->
        <Style x:Key="MD3InputStyle" TargetType="inputLayout:SfTextInputLayout">
            <Setter Property="ContainerType" Value="Outlined" />
            <Setter Property="OutlineCornerRadius" Value="4" />
            <Setter Property="FocusedStrokeThickness" Value="2" />
            <Setter Property="UnfocusedStrokeThickness" Value="1" />
            <Setter Property="VisualStateManager.VisualStateGroups">
                <VisualStateGroupList>
                    <VisualStateGroup x:Name="CommonStates">
                        <VisualState Name="Normal">
                            <VisualState.Setters>
                                <Setter Property="Stroke" Value="#79747E"/>
                            </VisualState.Setters>
                        </VisualState>
                        <VisualState Name="Focused">
                            <VisualState.Setters>
                                <Setter Property="Stroke" Value="#6750A4"/>
                            </VisualState.Setters>
                        </VisualState>
                        <VisualState Name="Error">
                            <VisualState.Setters>
                                <Setter Property="Stroke" Value="#B3261E"/>
                            </VisualState.Setters>
                        </VisualState>
                    </VisualStateGroup>
                </VisualStateGroupList>
            </Setter>
        </Style>
        
    </ResourceDictionary>
</Application.Resources>
```

### Applying the Style

```xml
<inputLayout:SfTextInputLayout Style="{StaticResource MD3InputStyle}"
                               Hint="Email">
    <Entry />
</inputLayout:SfTextInputLayout>
```

### Dark Mode Theme

```xml
<Style x:Key="DarkModeInputStyle" TargetType="inputLayout:SfTextInputLayout">
    <Setter Property="ContainerType" Value="Filled" />
    <Setter Property="ContainerBackground" Value="#1E1E1E" />
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>
            <VisualStateGroup x:Name="CommonStates">
                <VisualState Name="Normal">
                    <VisualState.Setters>
                        <Setter Property="Stroke" Value="#CCCCCC"/>
                    </VisualState.Setters>
                </VisualState>
                <VisualState Name="Focused">
                    <VisualState.Setters>
                        <Setter Property="Stroke" Value="#BB86FC"/>
                    </VisualState.Setters>
                </VisualState>
                <VisualState Name="Error">
                    <VisualState.Setters>
                        <Setter Property="Stroke" Value="#CF6679"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateGroupList>
    </Setter>
    <Setter Property="HintLabelStyle">
        <inputLayout:LabelStyle TextColor="#E0E0E0" />
    </Setter>
    <Setter Property="HelperLabelStyle">
        <inputLayout:LabelStyle TextColor="#B0B0B0" />
    </Setter>
</Style>
```

## Best Practices

### Color Selection

1. **Contrast:** Ensure sufficient contrast between stroke and background
2. **Consistency:** Use the same color scheme across all inputs
3. **Accessibility:** Follow WCAG 2.1 guidelines (4.5:1 contrast ratio minimum)
4. **Brand Alignment:** Match your brand colors in focused state
5. **Error Red:** Use universally recognized red for errors (#D32F2F, #B3261E)

### Visual States

1. **Always Define:** Define Normal, Focused, and Error states
2. **Smooth Transitions:** Platform handles animations automatically
3. **Test States:** Verify all states look correct on different backgrounds
4. **Error Hierarchy:** Make error state visually distinct

### Stroke Thickness

1. **Subtle Change:** Keep difference between focused/unfocused at 1-2dp
2. **Platform Consistency:** Match platform conventions
3. **Container Type:** Adjust based on Filled vs Outlined
4. **Accessibility:** Ensure borders are visible to users with low vision

### Performance

1. **Resource Styles:** Define complex styles as resources, not inline
2. **Color Caching:** Reuse Color objects instead of parsing strings
3. **VSM Overhead:** Minimal impact, safe to use throughout app

### Testing

1. **All States:** Test Normal, Focused, Error, and Disabled
2. **Dark Mode:** Verify colors in both light and dark themes
3. **Platforms:** Test on all target platforms (colors may render slightly different)
4. **Accessibility:** Use accessibility scanners to verify contrast ratios

## Common Color Schemes

### Material Design 3

```csharp
// Normal: #79747E (Outline)
// Focused: #6750A4 (Primary)
// Error: #B3261E (Error)
```

### iOS-style

```csharp
// Normal: #C7C7CC
// Focused: #007AFF (Blue)
// Error: #FF3B30 (Red)
```

### Custom Brand

```csharp
// Normal: #E0E0E0
// Focused: Your Brand Color
// Error: #D32F2F
```
