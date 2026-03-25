# Appearance Customization for .NET MAUI Accordion

This guide demonstrates how to customize the appearance of the Syncfusion .NET MAUI Accordion, including header styling, icon positioning, colors, and Visual State Manager integration.

## Table of Contents
- [Overview](#overview)
- [Header Icon Position](#header-icon-position)
- [Header Background Color](#header-background-color)
- [Icon Color Customization](#icon-color-customization)
- [Visual State Manager](#visual-state-manager)
  - [Expanded and Collapsed States](#expanded-and-collapsed-states)
  - [Focused State](#focused-state)
  - [PointerOver State](#pointerover-state)
  - [Normal State](#normal-state)
  - [Complete Visual State Example](#complete-visual-state-example)
- [Styling Best Practices](#styling-best-practices)

## Overview

The .NET MAUI Accordion provides extensive customization options for its appearance:

**Customizable Elements:**
- Header icon position (Start/End)
- Header background color
- Icon color
- Visual states (Expanded, Collapsed, Focused, PointerOver, Normal)

**Styling Approaches:**
1. **Direct Properties** - Set properties on individual `AccordionItem` instances
2. **Styles** - Define reusable styles for consistent appearance
3. **Visual State Manager** - Apply state-specific styling automatically

## Header Icon Position

Control the position of the expand/collapse icon in the header using `HeaderIconPosition`.

**Options:**
- `End` (Default) - Icon appears on the right side
- `Start` - Icon appears on the left side

**XAML:**
```xml
<syncfusion:SfAccordion HeaderIconPosition="Start">
    <syncfusion:SfAccordion.Items>
        <syncfusion:AccordionItem>
            <syncfusion:AccordionItem.Header>
                <Grid HeightRequest="48">
                    <Label Text="Settings" Margin="16,14,0,14" FontSize="14" />
                </Grid>
            </syncfusion:AccordionItem.Header>
            <syncfusion:AccordionItem.Content>
                <Grid Padding="16">
                    <Label Text="Configure your preferences" />
                </Grid>
            </syncfusion:AccordionItem.Content>
        </syncfusion:AccordionItem>
    </syncfusion:SfAccordion.Items>
</syncfusion:SfAccordion>
```

**C#:**
```csharp
accordion.HeaderIconPosition = Syncfusion.Maui.Expander.ExpanderIconPosition.Start;
```

**Use Cases:**
- **Start** - Left-aligned layouts, RTL languages, or when icon acts as a visual indicator before text
- **End** - Right-aligned layouts (default), conventional dropdown/expander pattern

## Header Background Color

Customize the background color of individual accordion item headers using `HeaderBackground`.

**XAML:**
```xml
<syncfusion:SfAccordion>
    <syncfusion:SfAccordion.Items>
        <syncfusion:AccordionItem HeaderBackground="#6750A4">
            <syncfusion:AccordionItem.Header>
                <Grid HeightRequest="48">
                    <Label Text="Important Section" 
                           Margin="16,14,0,14" 
                           FontSize="14"
                           TextColor="White" />
                </Grid>
            </syncfusion:AccordionItem.Header>
            <syncfusion:AccordionItem.Content>
                <Grid Padding="16" BackgroundColor="#f4f4f4">
                    <Label Text="This section has a custom header color" />
                </Grid>
            </syncfusion:AccordionItem.Content>
        </syncfusion:AccordionItem>
        
        <syncfusion:AccordionItem HeaderBackground="LightBlue">
            <syncfusion:AccordionItem.Header>
                <Grid HeightRequest="48">
                    <Label Text="Another Section" Margin="16,14,0,14" FontSize="14" />
                </Grid>
            </syncfusion:AccordionItem.Header>
            <syncfusion:AccordionItem.Content>
                <Grid Padding="16">
                    <Label Text="Different header color" />
                </Grid>
            </syncfusion:AccordionItem.Content>
        </syncfusion:AccordionItem>
    </syncfusion:SfAccordion.Items>
</syncfusion:SfAccordion>
```

**C#:**
```csharp
var item = new AccordionItem();
item.HeaderBackground = new SolidColorBrush(Color.FromArgb("#6750A4"));
```

**Color Formats:**
- Named colors: `"Red"`, `"LightBlue"`
- Hex: `"#6750A4"`, `"#FF5733"`
- RGBA: `Color.FromRgba(103, 80, 164, 255)`

## Icon Color Customization

Customize the expand/collapse icon color using `HeaderIconColor`.

**XAML:**
```xml
<syncfusion:SfAccordion>
    <syncfusion:SfAccordion.Items>
        <syncfusion:AccordionItem HeaderIconColor="Brown">
            <syncfusion:AccordionItem.Header>
                <Grid HeightRequest="48" BackgroundColor="LightYellow">
                    <Label Text="Custom Icon Color" Margin="16,14,0,14" FontSize="14" />
                </Grid>
            </syncfusion:AccordionItem.Header>
            <syncfusion:AccordionItem.Content>
                <Grid Padding="16">
                    <Label Text="The icon color is brown" />
                </Grid>
            </syncfusion:AccordionItem.Content>
        </syncfusion:AccordionItem>
        
        <syncfusion:AccordionItem HeaderBackground="#6750A4" 
                                  HeaderIconColor="White">
            <syncfusion:AccordionItem.Header>
                <Grid HeightRequest="48">
                    <Label Text="Dark Header with White Icon" 
                           Margin="16,14,0,14" 
                           FontSize="14"
                           TextColor="White" />
                </Grid>
            </syncfusion:AccordionItem.Header>
            <syncfusion:AccordionItem.Content>
                <Grid Padding="16">
                    <Label Text="High contrast icon" />
                </Grid>
            </syncfusion:AccordionItem.Content>
        </syncfusion:AccordionItem>
    </syncfusion:SfAccordion.Items>
</syncfusion:SfAccordion>
```

**C#:**
```csharp
var item = new AccordionItem();
item.HeaderIconColor = Colors.Brown;
```

**Accessibility Tip:** Ensure sufficient contrast between icon color and header background for WCAG compliance.

## Visual State Manager

The Visual State Manager enables state-based styling that automatically applies when the accordion item changes state.

**Available States:**
- `Expanded` - Item is expanded (content visible)
- `Collapsed` - Item is collapsed (content hidden)
- `Focused` - Item has keyboard focus
- `PointerOver` - Mouse/pointer is hovering over item (desktop)
- `Normal` - Default state (no interaction)

### Expanded and Collapsed States

Apply different styling when items are expanded vs. collapsed.

```xml
<ContentPage.Resources>
    <Style TargetType="syncfusion:AccordionItem">
        <Setter Property="VisualStateManager.VisualStateGroups">
            <VisualStateGroupList>
                <VisualStateGroup>
                    <VisualState Name="Expanded">
                        <VisualState.Setters>
                            <Setter Property="HeaderBackground" Value="#6750A4"/>
                            <Setter Property="HeaderIconColor" Value="White"/>
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState Name="Collapsed">
                        <VisualState.Setters>
                            <Setter Property="HeaderBackground" Value="#E8DEF8"/>
                            <Setter Property="HeaderIconColor" Value="#49454F"/>
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateGroupList>
        </Setter>
    </Style>
</ContentPage.Resources>

<syncfusion:SfAccordion>
    <syncfusion:SfAccordion.Items>
        <syncfusion:AccordionItem>
            <syncfusion:AccordionItem.Header>
                <Grid HeightRequest="48">
                    <Label Text="Dynamic Colors" Margin="16,14,0,14" FontSize="14" />
                </Grid>
            </syncfusion:AccordionItem.Header>
            <syncfusion:AccordionItem.Content>
                <Grid Padding="16">
                    <Label Text="Header color changes when expanded" />
                </Grid>
            </syncfusion:AccordionItem.Content>
        </syncfusion:AccordionItem>
    </syncfusion:SfAccordion.Items>
</syncfusion:SfAccordion>
```

**Result:**
- When collapsed: Light purple header with dark icon
- When expanded: Dark purple header with white icon

### Focused State

Style items when they receive keyboard focus.

```xml
<VisualState Name="Focused">
    <VisualState.Setters>
        <Setter Property="HeaderBackground" Value="#FFE5E5"/>
        <Setter Property="HeaderIconColor" Value="#CC0000"/>
    </VisualState.Setters>
</VisualState>
```

**When Applied:**
- User navigates with Tab key
- Item receives focus via keyboard interaction
- Improves accessibility for keyboard-only users

### PointerOver State

Apply hover effects on desktop platforms.

```xml
<VisualState Name="PointerOver">
    <VisualState.Setters>
        <Setter Property="HeaderBackground" Value="#E0E0E0"/>
        <Setter Property="HeaderIconColor" Value="#333333"/>
    </VisualState.Setters>
</VisualState>
```

**When Applied:**
- Mouse hovers over accordion item header
- Desktop/pointer-based platforms only
- Provides visual feedback for interactive elements

### Normal State

Define the default appearance when no interaction is occurring.

```xml
<VisualState Name="Normal">
    <VisualState.Setters>
        <Setter Property="HeaderBackground" Value="White"/>
        <Setter Property="HeaderIconColor" Value="Black"/>
    </VisualState.Setters>
</VisualState>
```

### Complete Visual State Example

Here's a comprehensive example using all five visual states:

**MainPage.xaml:**
```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:syncfusion="clr-namespace:Syncfusion.Maui.Accordion;assembly=Syncfusion.Maui.Expander"
             x:Class="AccordionApp.MainPage">
    
    <ContentPage.Resources>
        <Style TargetType="syncfusion:AccordionItem">
            <Setter Property="VisualStateManager.VisualStateGroups">
                <VisualStateGroupList>
                    <VisualStateGroup>
                        <!-- Expanded State -->
                        <VisualState Name="Expanded">
                            <VisualState.Setters>
                                <Setter Property="HeaderBackground" Value="#6750A4"/>
                                <Setter Property="HeaderIconColor" Value="White"/>
                            </VisualState.Setters>
                        </VisualState>
                        
                        <!-- Collapsed State -->
                        <VisualState Name="Collapsed">
                            <VisualState.Setters>
                                <Setter Property="HeaderBackground" Value="#E8DEF8"/>
                                <Setter Property="HeaderIconColor" Value="#49454F"/>
                            </VisualState.Setters>
                        </VisualState>
                        
                        <!-- Focused State (Keyboard Navigation) -->
                        <VisualState Name="Focused">
                            <VisualState.Setters>
                                <Setter Property="HeaderBackground" Value="#FFC5C5"/>
                                <Setter Property="HeaderIconColor" Value="#B00020"/>
                            </VisualState.Setters>
                        </VisualState>
                        
                        <!-- PointerOver State (Hover) -->
                        <VisualState Name="PointerOver">
                            <VisualState.Setters>
                                <Setter Property="HeaderBackground" Value="#D6D6D6"/>
                                <Setter Property="HeaderIconColor" Value="#424242"/>
                            </VisualState.Setters>
                        </VisualState>
                        
                        <!-- Normal State (Default) -->
                        <VisualState Name="Normal">
                            <VisualState.Setters>
                                <Setter Property="HeaderBackground" Value="White"/>
                                <Setter Property="HeaderIconColor" Value="Black"/>
                            </VisualState.Setters>
                        </VisualState>
                    </VisualStateGroup>
                </VisualStateGroupList>
            </Setter>
        </Style>
    </ContentPage.Resources>
    
    <syncfusion:SfAccordion ExpandMode="MultipleOrNone" BackgroundColor="#FAFAFA">
        <syncfusion:SfAccordion.Items>
            <syncfusion:AccordionItem x:Name="item1" IsExpanded="True">
                <syncfusion:AccordionItem.Header>
                    <Grid HeightRequest="48" Padding="16,0">
                        <!-- Bind TextColor to HeaderIconColor for consistency -->
                        <Label Text="Employee Details" 
                               TextColor="{Binding HeaderIconColor, Source={x:Reference item1}}"
                               VerticalTextAlignment="Center"
                               FontSize="14" />
                    </Grid>
                </syncfusion:AccordionItem.Header>
                <syncfusion:AccordionItem.Content>
                    <Grid Padding="16" BackgroundColor="White">
                        <StackLayout Spacing="8">
                            <Label Text="Name: Robin Rane" FontSize="14" />
                            <Label Text="Position: Chairman" FontSize="14" />
                            <Label Text="Organization: ABC Inc." FontSize="14" />
                        </StackLayout>
                    </Grid>
                </syncfusion:AccordionItem.Content>
            </syncfusion:AccordionItem>
            
            <syncfusion:AccordionItem x:Name="item2">
                <syncfusion:AccordionItem.Header>
                    <Grid HeightRequest="48" Padding="16,0">
                        <Label Text="Department Information" 
                               TextColor="{Binding HeaderIconColor, Source={x:Reference item2}}"
                               VerticalTextAlignment="Center"
                               FontSize="14" />
                    </Grid>
                </syncfusion:AccordionItem.Header>
                <syncfusion:AccordionItem.Content>
                    <Grid Padding="16" BackgroundColor="White">
                        <Label Text="Department: Operations" FontSize="14" />
                    </Grid>
                </syncfusion:AccordionItem.Content>
            </syncfusion:AccordionItem>
        </syncfusion:SfAccordion.Items>
    </syncfusion:SfAccordion>
</ContentPage>
```

**Behavior:**
- **Normal** - Default white header, black icon
- **PointerOver** - Gray header when hovering (desktop)
- **Focused** - Light red header when keyboard-focused
- **Collapsed** - Light purple header, dark icon
- **Expanded** - Dark purple header, white icon

**Pro Tip:** Bind the header Label's `TextColor` to the `HeaderIconColor` property to keep text and icon colors synchronized.

## Styling Best Practices

### 1. Define Global Styles in Resources

Place styles in `ContentPage.Resources` or `App.xaml` for reusability:

```xml
<Application.Resources>
    <Style TargetType="syncfusion:AccordionItem">
        <!-- Style definition -->
    </Style>
</Application.Resources>
```

### 2. Use Material Design Color Scheme

Follow Material Design 3 guidelines for professional appearance:
- Primary: `#6750A4`
- Surface: `#FFFFFF`
- On-Surface: `#1C1B1F`
- Surface-Variant: `#E8DEF8`

### 3. Ensure Sufficient Contrast

Maintain WCAG AA contrast ratio (4.5:1 for text):
- Dark text on light backgrounds
- Light text on dark backgrounds
- Test with accessibility tools

### 4. Consider Platform Differences

- **Desktop** - PointerOver states are prominent
- **Mobile** - Focus and touch states matter more
- **Accessibility** - Focused states critical for keyboard users

### 5. Test All States

Verify appearance in all five visual states:
- Normal (default)
- PointerOver (mouse hover)
- Focused (keyboard navigation)
- Expanded (content visible)
- Collapsed (content hidden)

### 6. Use Consistent Spacing

Maintain consistent padding and margins:
```xml
<Grid HeightRequest="48" Padding="16,0">
    <Label Margin="0" />
</Grid>
```

### 7. Avoid Overly Bright Colors

Use muted, professional colors for business applications:
- ✅ `#6750A4` (Material Purple)
- ✅ `#2196F3` (Material Blue)
- ❌ `#FF00FF` (Neon Magenta)
- ❌ `#00FF00` (Bright Green)

## Troubleshooting

**Problem:** Visual States not applying
**Solution:** Ensure the style is defined in Resources and `TargetType` matches `syncfusion:AccordionItem`

**Problem:** Colors not changing on state transitions
**Solution:** Verify all five states are defined, even if some use the same colors

**Problem:** Text hard to read on colored headers
**Solution:** Adjust text color based on header background for sufficient contrast

**Problem:** PointerOver not working
**Solution:** PointerOver is desktop-only; test on Windows or macOS, not mobile

## Additional Resources

- [Visual State Manager Documentation](https://learn.microsoft.com/en-us/dotnet/maui/user-interface/visual-states)
- [Material Design Color System](https://m3.material.io/styles/color/overview)
- [Sample GitHub Repository](https://github.com/SyncfusionExamples/customize-the-ui-appearance-using-visual-states-in-.net-maui-accordion)
