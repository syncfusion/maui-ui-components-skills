# Appearance and Styling

## Table of Contents
- [Overview](#overview)
- [Header Icon Position](#header-icon-position)
- [Header Background Color](#header-background-color)
- [Icon Color Customization](#icon-color-customization)
- [Visual State Manager](#visual-state-manager)
- [Complete Styling Examples](#complete-styling-examples)
- [Best Practices](#best-practices)

---

## Overview

The .NET MAUI SfExpander provides built-in properties for customizing header appearance:

- **HeaderIconPosition** - Control where the expand/collapse icon appears
- **HeaderBackground** - Customize header background color/brush
- **HeaderIconColor** - Change the expand/collapse icon color
- **Visual State Manager** - Apply different styles based on expander state

---

## Header Icon Position

The `HeaderIconPosition` property controls where the expand/collapse icon appears in the header.

**Default:** `End` (right side)

**Options:**
- `End` - Icon appears on the right (default)
- `Start` - Icon appears on the left
- `None` - Specifies that the expander icon will not be shown on the header.

### XAML

```xml
<syncfusion:SfExpander x:Name="expander" 
                       HeaderIconPosition="Start">
    <syncfusion:SfExpander.Header>
        <Grid>
            <Label Text="Settings" FontSize="16"/>
        </Grid>
    </syncfusion:SfExpander.Header>
    <syncfusion:SfExpander.Content>
        <Grid Padding="15">
            <Label Text="Settings content here"/>
        </Grid>
    </syncfusion:SfExpander.Content>
</syncfusion:SfExpander>
```

### C#

```csharp
expander.HeaderIconPosition = Syncfusion.Maui.Expander.ExpanderIconPosition.Start;
```

### Visual Comparison

```xml
<StackLayout Spacing="10">
    
    <!-- Icon at End (default) -->
    <syncfusion:SfExpander HeaderIconPosition="End">
        <syncfusion:SfExpander.Header>
            <Grid Padding="15">
                <Label Text="Icon on right (End)" FontSize="16"/>
            </Grid>
        </syncfusion:SfExpander.Header>
        <syncfusion:SfExpander.Content>
            <Grid Padding="15"><Label Text="Content"/></Grid>
        </syncfusion:SfExpander.Content>
    </syncfusion:SfExpander>
    
    <!-- Icon at Start -->
    <syncfusion:SfExpander HeaderIconPosition="Start">
        <syncfusion:SfExpander.Header>
            <Grid Padding="15">
                <Label Text="Icon on left (Start)" FontSize="16"/>
            </Grid>
        </syncfusion:SfExpander.Header>
        <syncfusion:SfExpander.Content>
            <Grid Padding="15"><Label Text="Content"/></Grid>
        </syncfusion:SfExpander.Content>
    </syncfusion:SfExpander>
    
</StackLayout>
```

**Use Cases:**
- **End (default):** Standard accordion layouts, most common pattern
- **Start:** Right-to-left (RTL) languages, custom icon placement needs

---

## Header Background Color

The `HeaderBackground` property customizes the background color or brush of the header.

### XAML - Solid Color

```xml
<syncfusion:SfExpander x:Name="expander" 
                       HeaderBackground="Pink">
    <syncfusion:SfExpander.Header>
        <Grid>
            <Label Text="Custom Background" FontSize="16"/>
        </Grid>
    </syncfusion:SfExpander.Header>
    <syncfusion:SfExpander.Content>
        <Grid Padding="15">
            <Label Text="Content here"/>
        </Grid>
    </syncfusion:SfExpander.Content>
</syncfusion:SfExpander>
```

### C# - Solid Color

```csharp
expander.HeaderBackground = Colors.Pink;
```

### XAML - Hex Color

```xml
<syncfusion:SfExpander HeaderBackground="#6750A4">
    <!-- Header and Content -->
</syncfusion:SfExpander>
```

### XAML - Gradient Background

```xml
<syncfusion:SfExpander>
    <syncfusion:SfExpander.HeaderBackground>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,0">
            <GradientStop Color="#6750A4" Offset="0.0"/>
            <GradientStop Color="#8E7CC3" Offset="1.0"/>
        </LinearGradientBrush>
    </syncfusion:SfExpander.HeaderBackground>
    
    <syncfusion:SfExpander.Header>
        <Grid>
            <Label Text="Gradient Header" FontSize="16" TextColor="White"/>
        </Grid>
    </syncfusion:SfExpander.Header>
    <syncfusion:SfExpander.Content>
        <Grid Padding="15"><Label Text="Content"/></Grid>
    </syncfusion:SfExpander.Content>
</syncfusion:SfExpander>
```

### C# - Gradient Background

```csharp
expander.HeaderBackground = new LinearGradientBrush
{
    StartPoint = new Point(0, 0),
    EndPoint = new Point(1, 0),
    GradientStops = new GradientStopCollection
    {
        new GradientStop { Color = Color.FromArgb("#6750A4"), Offset = 0.0f },
        new GradientStop { Color = Color.FromArgb("#8E7CC3"), Offset = 1.0f }
    }
};
```

---

## Icon Color Customization

The `HeaderIconColor` property changes the color of the expand/collapse icon.

### XAML

```xml
<syncfusion:SfExpander x:Name="expander"
                       HeaderIconColor="Brown">
    <syncfusion:SfExpander.Header>
        <Grid>
            <Label Text="Custom Icon Color" FontSize="16"/>
        </Grid>
    </syncfusion:SfExpander.Header>
    <syncfusion:SfExpander.Content>
        <Grid Padding="15">
            <Label Text="Content here"/>
        </Grid>
    </syncfusion:SfExpander.Content>
</syncfusion:SfExpander>
```

### C#

```csharp
expander.HeaderIconColor = Colors.Brown;
```

### Combined Styling

```xml
<syncfusion:SfExpander HeaderBackground="#6750A4"
                       HeaderIconColor="White"
                       HeaderIconPosition="Start">
    <syncfusion:SfExpander.Header>
        <Grid Padding="15">
            <Label Text="Fully Styled Header" 
                   FontSize="16" 
                   TextColor="White"/>
        </Grid>
    </syncfusion:SfExpander.Header>
    <syncfusion:SfExpander.Content>
        <Grid Padding="15" BackgroundColor="#F5F5F5">
            <Label Text="Content with styled header"/>
        </Grid>
    </syncfusion:SfExpander.Content>
</syncfusion:SfExpander>
```

---

## Visual State Manager

Use Visual State Manager (VSM) to apply different styles based on the expander's state.

**Available States:**
- **Expanded** - When content is visible
- **Collapsed** - When content is hidden
- **PointerOver** - When mouse hovers over header (desktop platforms)
- **Normal** - Default state

### Basic VSM Example

```xml
<syncfusion:SfExpander x:Name="expander1" IsExpanded="True">
    <syncfusion:SfExpander.Header>
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="48"/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="35"/>
                <ColumnDefinition Width="*"/>
            </Grid.ColumnDefinitions>
            <Label Text="&#xe703;" 
                   FontSize="16" 
                   Margin="14,2,2,2"
                   TextColor="{Binding Path=HeaderIconColor,Source={x:Reference expander1}}"
                   FontFamily='AccordionFontIcons'
                   VerticalOptions="Center" 
                   VerticalTextAlignment="Center"/>
            <Label CharacterSpacing="0.25" 
                   TextColor="{Binding Path=HeaderIconColor,Source={x:Reference expander1}}" 
                   FontFamily="Roboto-Regular" 
                   Text="Invoice Date" 
                   FontSize="14" 
                   Grid.Column="1" 
                   VerticalOptions="CenterAndExpand"/>
        </Grid>
    </syncfusion:SfExpander.Header>
    
    <syncfusion:SfExpander.Content>
        <Grid Padding="18,8,0,18">
            <Label CharacterSpacing="0.25" 
                   FontFamily="Roboto-Regular" 
                   Text="11:03 AM, 15 January 2019" 
                   FontSize="14" 
                   VerticalOptions="CenterAndExpand"/>
        </Grid>
    </syncfusion:SfExpander.Content>
    
    <!-- Visual State Manager -->
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroupList>
            <VisualStateGroup>
                
                <VisualState Name="Expanded">
                    <VisualState.Setters>
                        <Setter Property="HeaderBackground" Value="#6750A4"/>
                        <Setter Property="HeaderIconColor" Value="#FFFFFF"/>
                    </VisualState.Setters>
                </VisualState>
                
                <VisualState Name="Collapsed">
                    <VisualState.Setters>
                        <Setter Property="HeaderBackground" Value="#141C1B1F"/>
                        <Setter Property="HeaderIconColor" Value="#49454F"/>
                    </VisualState.Setters>
                </VisualState>
                
            </VisualStateGroup>
        </VisualStateGroupList>
    </VisualStateManager.VisualStateGroups>
</syncfusion:SfExpander>
```

### VSM with All States

```xml
<syncfusion:SfExpander>
    <syncfusion:SfExpander.Header>
        <Grid Padding="15">
            <Label Text="Hover and Click Me" FontSize="16"/>
        </Grid>
    </syncfusion:SfExpander.Header>
    
    <syncfusion:SfExpander.Content>
        <Grid Padding="15">
            <Label Text="Content with full VSM styling"/>
        </Grid>
    </syncfusion:SfExpander.Content>
    
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroupList>
            <VisualStateGroup>
                
                <!-- Expanded State -->
                <VisualState Name="Expanded">
                    <VisualState.Setters>
                        <Setter Property="HeaderBackground" Value="#6750A4"/>
                        <Setter Property="HeaderIconColor" Value="#FFFFFF"/>
                    </VisualState.Setters>
                </VisualState>
                
                <!-- Collapsed State -->
                <VisualState Name="Collapsed">
                    <VisualState.Setters>
                        <Setter Property="HeaderBackground" Value="#F5F5F5"/>
                        <Setter Property="HeaderIconColor" Value="#49454F"/>
                    </VisualState.Setters>
                </VisualState>
                
                <!-- PointerOver State (Desktop) -->
                <VisualState Name="PointerOver">
                    <VisualState.Setters>
                        <Setter Property="HeaderBackground" Value="#ded6d5"/>
                        <Setter Property="HeaderIconColor" Value="#524f4f"/>
                    </VisualState.Setters>
                </VisualState>
                
                <!-- Normal State -->
                <VisualState Name="Normal">
                    <VisualState.Setters>
                        <Setter Property="HeaderBackground" Value="#faf8f7"/>
                        <Setter Property="HeaderIconColor" Value="#000000"/>
                    </VisualState.Setters>
                </VisualState>
                
            </VisualStateGroup>
        </VisualStateGroupList>
    </VisualStateManager.VisualStateGroups>
</syncfusion:SfExpander>
```

### C# - Visual State Manager

```csharp
var expander = new SfExpander();

// Define header
var headerGrid = new Grid { Padding = new Thickness(15) };
headerGrid.Children.Add(new Label { Text = "Visual States", FontSize = 16 });
expander.Header = headerGrid;

// Define content
var contentGrid = new Grid { Padding = new Thickness(15) };
contentGrid.Children.Add(new Label { Text = "Content here" });
expander.Content = contentGrid;

// Create Visual State Groups
var visualStateGroup = new VisualStateGroup();

// Expanded State
var expandedState = new VisualState { Name = "Expanded" };
expandedState.Setters.Add(new Setter 
{ 
    Property = SfExpander.HeaderBackgroundProperty, 
    Value = Color.FromArgb("#6750A4") 
});
expandedState.Setters.Add(new Setter 
{ 
    Property = SfExpander.HeaderIconColorProperty, 
    Value = Colors.White 
});
visualStateGroup.States.Add(expandedState);

// Collapsed State
var collapsedState = new VisualState { Name = "Collapsed" };
collapsedState.Setters.Add(new Setter 
{ 
    Property = SfExpander.HeaderBackgroundProperty, 
    Value = Color.FromArgb("#F5F5F5") 
});
collapsedState.Setters.Add(new Setter 
{ 
    Property = SfExpander.HeaderIconColorProperty, 
    Value = Color.FromArgb("#49454F") 
});
visualStateGroup.States.Add(collapsedState);

// Apply to expander
VisualStateManager.SetVisualStateGroups(expander, new VisualStateGroupList { visualStateGroup });
```

---

## Complete Styling Examples

### Example 1: Material Design Style

```xml
<syncfusion:SfExpander AnimationDuration="200" 
                       AnimationEasing="SinOut"
                       HeaderIconPosition="End">
    <syncfusion:SfExpander.Header>
        <Grid Padding="16,12">
            <Label Text="Material Design Header" 
                   FontSize="16" 
                   FontFamily="Roboto-Medium"/>
        </Grid>
    </syncfusion:SfExpander.Header>
    
    <syncfusion:SfExpander.Content>
        <Grid Padding="16" BackgroundColor="#FAFAFA">
            <Label Text="Content with material styling" 
                   FontSize="14" 
                   FontFamily="Roboto-Regular"/>
        </Grid>
    </syncfusion:SfExpander.Content>
    
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroupList>
            <VisualStateGroup>
                <VisualState Name="Expanded">
                    <VisualState.Setters>
                        <Setter Property="HeaderBackground" Value="#6200EE"/>
                        <Setter Property="HeaderIconColor" Value="White"/>
                    </VisualState.Setters>
                </VisualState>
                <VisualState Name="Collapsed">
                    <VisualState.Setters>
                        <Setter Property="HeaderBackground" Value="White"/>
                        <Setter Property="HeaderIconColor" Value="#6200EE"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateGroupList>
    </VisualStateManager.VisualStateGroups>
</syncfusion:SfExpander>
```

### Example 2: Dark Theme

```xml
<syncfusion:SfExpander AnimationDuration="250" 
                       HeaderBackground="#1E1E1E"
                       HeaderIconColor="#E0E0E0">
    <syncfusion:SfExpander.Header>
        <Grid Padding="15">
            <Label Text="Dark Theme Header" 
                   FontSize="16" 
                   TextColor="#E0E0E0"/>
        </Grid>
    </syncfusion:SfExpander.Header>
    
    <syncfusion:SfExpander.Content>
        <Grid Padding="15" BackgroundColor="#2D2D2D">
            <Label Text="Dark themed content" 
                   FontSize="14" 
                   TextColor="#B0B0B0"/>
        </Grid>
    </syncfusion:SfExpander.Content>
</syncfusion:SfExpander>
```

### Example 3: Status Colors

```xml
<StackLayout Spacing="10">
    
    <!-- Success -->
    <syncfusion:SfExpander HeaderBackground="#4CAF50" 
                           HeaderIconColor="White">
        <syncfusion:SfExpander.Header>
            <Grid Padding="15">
                <Label Text="Success Status" TextColor="White" FontSize="16"/>
            </Grid>
        </syncfusion:SfExpander.Header>
        <syncfusion:SfExpander.Content>
            <Grid Padding="15" BackgroundColor="#E8F5E9">
                <Label Text="Operation completed successfully"/>
            </Grid>
        </syncfusion:SfExpander.Content>
    </syncfusion:SfExpander>
    
    <!-- Warning -->
    <syncfusion:SfExpander HeaderBackground="#FF9800" 
                           HeaderIconColor="White">
        <syncfusion:SfExpander.Header>
            <Grid Padding="15">
                <Label Text="Warning Status" TextColor="White" FontSize="16"/>
            </Grid>
        </syncfusion:SfExpander.Header>
        <syncfusion:SfExpander.Content>
            <Grid Padding="15" BackgroundColor="#FFF3E0">
                <Label Text="Please review this information"/>
            </Grid>
        </syncfusion:SfExpander.Content>
    </syncfusion:SfExpander>
    
    <!-- Error -->
    <syncfusion:SfExpander HeaderBackground="#F44336" 
                           HeaderIconColor="White">
        <syncfusion:SfExpander.Header>
            <Grid Padding="15">
                <Label Text="Error Status" TextColor="White" FontSize="16"/>
            </Grid>
        </syncfusion:SfExpander.Header>
        <syncfusion:SfExpander.Content>
            <Grid Padding="15" BackgroundColor="#FFEBEE">
                <Label Text="An error occurred"/>
            </Grid>
        </syncfusion:SfExpander.Content>
    </syncfusion:SfExpander>
    
</StackLayout>
```

---

## Best Practices

### 1. Ensure Sufficient Contrast

When customizing colors, maintain readable contrast between text and background:

```xml
<!-- ✅ Good contrast -->
<syncfusion:SfExpander HeaderBackground="#6750A4">
    <syncfusion:SfExpander.Header>
        <Grid><Label Text="Header" TextColor="White"/></Grid>
    </syncfusion:SfExpander.Header>
</syncfusion:SfExpander>

<!-- ❌ Poor contrast -->
<syncfusion:SfExpander HeaderBackground="#E0E0E0">
    <syncfusion:SfExpander.Header>
        <Grid><Label Text="Header" TextColor="#D0D0D0"/></Grid>
    </syncfusion:SfExpander.Header>
</syncfusion:SfExpander>
```

### 2. Use VSM for Dynamic Styling

Prefer Visual State Manager over manual event-based styling:

```xml
<!-- ✅ Recommended: VSM -->
<syncfusion:SfExpander>
    <VisualStateManager.VisualStateGroups>
        <!-- State definitions -->
    </VisualStateManager.VisualStateGroups>
</syncfusion:SfExpander>

<!-- ❌ Not recommended: Manual events -->
<syncfusion:SfExpander Expanded="OnExpanded" Collapsed="OnCollapsed"/>
<!-- Then manually change colors in code -->
```

### 3. Bind HeaderIconColor to Custom Icon Labels

When using custom icons in headers, bind their TextColor to HeaderIconColor for consistent styling:

```xml
<Label Text="&#xe701;" 
       TextColor="{Binding Path=HeaderIconColor, Source={x:Reference expander}}"/>
```

### 4. Test on Multiple Platforms

Icon positioning and colors may appear differently across platforms. Test on:
- iOS
- Android
- Windows
- macOS

### 5. Use Theme Resources

Define colors in App.xaml for consistent theming:

```xml
<!-- App.xaml -->
<Application.Resources>
    <Color x:Key="ExpanderHeaderExpanded">#6750A4</Color>
    <Color x:Key="ExpanderHeaderCollapsed">#F5F5F5</Color>
    <Color x:Key="ExpanderIconExpanded">White</Color>
    <Color x:Key="ExpanderIconCollapsed">#49454F</Color>
</Application.Resources>
```

Then reference in expanders:

```xml
<syncfusion:SfExpander>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroupList>
            <VisualStateGroup>
                <VisualState Name="Expanded">
                    <VisualState.Setters>
                        <Setter Property="HeaderBackground" 
                                Value="{StaticResource ExpanderHeaderExpanded}"/>
                        <Setter Property="HeaderIconColor" 
                                Value="{StaticResource ExpanderIconExpanded}"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateGroupList>
    </VisualStateManager.VisualStateGroups>
</syncfusion:SfExpander>
```

---

## Related Features

- **Getting Started:** See `getting-started.md` for basic setup
- **Header/Content:** See `header-content-customization.md` for layout options
- **Animation/Events:** See `animation-events.md` for animation control
- **Liquid Glass:** See `liquid-glass-effect.md` for modern translucent effects

---

## Sample Projects

- [Customize Appearance with Visual State Manager](https://github.com/SyncfusionExamples/customize-the-appearance-using-visual-state-manager-in-.net-maui-expander)
